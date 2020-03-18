---
ms.openlocfilehash: d9080202f9413f8beb80db222d47f5fc082ae641
ms.sourcegitcommit: f3170512e7a3193efbcea52ec330648375e36915
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2020
ms.locfileid: "79484356"
---
# <a name="function-pointers"></a>函数指针

## <a name="summary"></a>总结

此建议提供的语言构造可公开当前无法有效访问的 IL 操作码，或C#目前为： `ldftn` 和 `calli`。 这些 IL 操作码在高性能代码中非常重要，开发人员需要一种高效的访问方式。

## <a name="motivation"></a>动机

以下问题中介绍了此功能的动机和背景（这是此功能的一个潜在实现方式）：

https://github.com/dotnet/csharplang/issues/191

这是[编译器内部函数](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)的替代设计建议

## <a name="detailed-design"></a>详细设计

### <a name="function-pointers"></a>函数指针

该语言将允许使用 `delegate*` 语法来声明函数指针。 下一节中详细介绍了完整的语法，但它与 `Func` 和 `Action` 类型声明所使用的语法类似。

``` csharp
unsafe class Example {
    void Example(Action<int> a, delegate*<int, void> f) {
        a(42);
        f(42);
    }
}
```

这些类型使用 ECMA-335 中所述的函数指针类型来表示。 这意味着调用 `delegate*` 将使用 `calli`，在这种情况下，`delegate` 的调用将对 `Invoke` 方法使用 `callvirt`。
当然，对于这两种构造，调用是相同的。

方法指针的 ECMA-335 定义将调用约定包含为类型签名（第7.1 节）的一部分。
将 `managed`默认调用约定。 可以通过在 `delegate*` 语法之后添加适当的修饰符来指定替代形式： `managed`、`cdecl`、`stdcall`、`thiscall`或 `unmanaged`。 示例：

``` csharp
// This method will be invoked using the cdecl calling convention
delegate* cdecl<int, int>;

// This method will be invoked using the stdcall calling convention
delegate* stdcall<int, int>;
```

`delegate*` 类型之间的转换是根据其签名（包括调用约定）完成的。

``` csharp
unsafe class Example {
    void Conversions() {
        delegate*<int, int, int> p1 = ...;
        delegate* managed<int, int, int> p2 = ...;
        delegate* cdecl<int, int, int> p3 = ...;

        p1 = p2; // okay p1 and p2 have compatible signatures
        Console.WriteLine(p2 == p1); // True
        p2 = p3; // error: calling conventions are incompatible
    }
}
```

`delegate*` 类型是指针类型，这意味着它具有标准指针类型的所有功能和限制：

- 仅在 `unsafe` 上下文中有效。
- 只能从 `unsafe` 的上下文中调用包含 `delegate*` 参数或返回类型的方法。
- 无法转换为 `object`。
- 不能用作泛型参数。
- 可以将 `delegate*` 隐式转换为 `void*`。
- 可以从 `void*` 显式转换为 `delegate*`。

限制：

- 自定义特性不能应用于 `delegate*` 或它的任何元素。
- 不能将 `delegate*` 参数标记为 `params`
- `delegate*` 类型具有正常指针类型的所有限制。

### <a name="function-pointer-syntax"></a>函数指针语法

完整的函数指针语法由以下语法表示：

```antlr
pointer_type
    : ...
    | funcptr_type
    ;

funcptr_type
    : 'delegate' '*' calling_convention? '<' (funcptr_parameter_modifier? type ',')* funcptr_return_modifier? return_type '>'
    ;

calling_convention
    : 'cdecl'
    | 'managed'
    | 'stdcall'
    | 'thiscall'
    | 'unmanaged'
    ;

funcptr_parameter_modifier
    : 'ref'
    | 'out'
    | 'in'
    ;

funcptr_return_modifier
    : 'ref'
    | 'ref readonly'
    ;
```

`unmanaged` 调用约定表示当前平台上的本机代码的默认调用约定，并且编码为 winapi。
所有 `calling_convention`都是上下文关键字，在 `delegate*`之前。

``` csharp
delegate int Func1(string s);
delegate Func1 Func2(Func1 f);

// Function pointer equivalent without calling convention
delegate*<string, int>;
delegate*<delegate*<string, int>, delegate*<string, int>>;

// Function pointer equivalent with calling convention
delegate* managed<string, int>;
delegate*<delegate* managed<string, int>, delegate*<string, int>>;
```

### <a name="function-pointer-conversions"></a>函数指针转换

在不安全的上下文中，可以使用隐式转换集（隐式转换）进行扩展，以包括以下隐式指针转换：
- [_现有转换_](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#pointer-conversions)
- 如果满足以下所有条件，则从_funcptr\_类型_`F0` 到其他_funcptr\_类型_`F1`：
    - `F0` 和 `F1` 具有相同数量的参数，并且 `F0` 中的每个参数 `D0n` 与 `ref`中的相应参数 `out`、`in` 或 `D1n` 修饰符相同。`F1`
    - 对于每个值参数（不带 `ref`、`out`或 `in` 修饰符的参数），从 `F0` 中的参数类型到 `F1`中的相应参数类型存在标识转换、隐式引用转换或隐式指针转换。
    - 对于每个 `ref`、`out`或 `in` 参数，`F0` 中的参数类型与 `F1`中的相应参数类型相同。
    - 如果返回类型是 by 值（无 `ref` 或 `ref readonly`），则从 `F1` 的返回类型到 `F0`的返回类型存在标识、隐式引用或隐式指针转换。
    - 如果返回类型是按引用（`ref` 或 `ref readonly`），则 `F1` 的返回类型和 `ref` 修饰符与 `ref` 的返回类型和 `F0`修饰符相同。
    - `F0` 的调用约定与 `F1`的调用约定相同。

### <a name="allow-address-of-to-target-methods"></a>允许目标方法的地址

现在将允许方法组作为表达式的参数。 此类表达式的类型将为 `delegate*`，它具有目标方法的等效签名和托管调用约定：

``` csharp
unsafe class Util {
    public static void Log() { }

    void Use() {
        delegate*<void> ptr1 = &Util.Log;

        // Error: type "delegate*<void>" not compatible with "delegate*<int>";
        delegate*<int> ptr2 = &Util.Log;

        // Okay. Conversion to void* is always allowed.
        void* v = &Util.Log;
   }
}
```

在不安全的上下文中，如果满足以下所有条件，则方法 `M` 与函数指针类型兼容 `F`：
- `M` 和 `F` 具有相同数量的参数，并且 `D` 中的每个参数都具有与 `out`中的相应参数相同的 `ref`、`in` 或 `F`修饰符。
- 对于每个值参数（不带 `ref`、`out`或 `in` 修饰符的参数），从 `M` 中的参数类型到 `F`中的相应参数类型存在标识转换、隐式引用转换或隐式指针转换。
- 对于每个 `ref`、`out`或 `in` 参数，`M` 中的参数类型与 `F`中的相应参数类型相同。
- 如果返回类型是 by 值（无 `ref` 或 `ref readonly`），则从 `F` 的返回类型到 `M`的返回类型存在标识、隐式引用或隐式指针转换。
- 如果返回类型是按引用（`ref` 或 `ref readonly`），则 `F` 的返回类型和 `ref` 修饰符与 `ref` 的返回类型和 `M`修饰符相同。
- `M` 的调用约定与 `F`的调用约定相同。
- `M` 是一种静态方法。

在不安全的上下文中，从地址为的表达式中存在隐式转换，其目标为方法组 `E` 到兼容的函数指针类型 `F` 如果 `E` 至少包含一个方法，该方法适用于通过使用 `F`的参数类型和修饰符构建的参数列表，如下所述。
- 选择单个方法 `M` 与 `E(A)` 窗体的方法调用对应，并进行以下修改：
   - 参数列表 `A` 是一个表达式列表，每个表达式都归类为一个变量，并使用相应_正式\_参数_的类型和修饰符（`ref`、`out`或 `in`）\_`D`列表。
   - 候选方法只是这些方法，这些方法只是那些适用于其普通形式的方法，而不是那些在其展开形式中适用的方法。
- 如果方法调用的算法产生错误，则会发生编译时错误。 否则，该算法将生成单个最佳方法 `M` 与 `F` 具有相同数量的参数，并将转换视为存在。
- 所选方法 `M` 必须与函数指针类型兼容（如上定义） `F`。 否则，将发生编译时错误。
- 转换的结果是 `F`类型的函数指针。

如果 `E`中只有一个静态方法 `M`，则目标为方法组 `void*` `E` 的地址表达式中存在隐式转换。
如果有一个静态方法，则 `M``E` 的单个最佳方法。
否则，将发生编译时错误。

这意味着开发人员可以依赖重载决策规则与地址运算符结合使用：

``` csharp
unsafe class Util {
    public static void Log() { }
    public static void Log(string p1) { }
    public static void Log(int i) { };

    void Use() {
        delegate*<void> a1 = &Log; // Log()
        delegate*<int, void> a2 = &Log; // Log(int i)

        // Error: ambiguous conversion from method group Log to "void*"
        void* v = &Log;
    }
```

将使用 `ldftn` 指令来实现此地址的操作员。

此功能的限制：

- 仅适用于标记为 `static`的方法。
- 不能在 `&`中使用非`static` 本地函数。 这些方法的实现细节特意不由语言指定。 这包括它们是静态的还是实例的，或者是否与它们一起发出的签名完全相同。

### <a name="better-function-member"></a>更好的函数成员

更好的函数成员规范将更改为包含以下行：

> `delegate*` 比 `void*` 更为具体

这意味着，可以在 `void*` 和 `delegate*` 上重载，但仍揭示使用地址运算符。

## <a name="open-issues"></a>打开的问题

### <a name="nativecallableattribute"></a>NativeCallableAttribute

这是 CLR 在调用时用于避免托管到本机序言的属性。 此特性标记的方法只能从本机代码中调用，而不能从托管（无法调用方法，创建委托，等等）中调用。 特性并非 mscorlib 专用;运行时会将具有此名称的任何属性视为相同的语义。

运行时和语言可以协同工作以完全支持此操作。 该语言可以选择将具有 `NativeCallable` 属性的 `static` 成员作为具有指定调用约定的 `delegate*` 进行处理。

``` csharp
unsafe class NativeCallableExample {
    [NativeCallable(CallingConvention.CDecl)]
    static void CloseHandle(IntPtr p) => Marshal.FreeHGlobal(p);

    void Use() {
        delegate*<IntPtr, void> p1 = &CloseHandle; // Error: Invalid calling convention

        delegate* cdecl<IntPtr, void> p2 = &CloseHandle; // Okay
    }
}

```

另外，该语言可能还需要：

- 将使用 `NativeCallable` 标记的方法的任何托管调用标记为错误。 如果无法从托管代码调用函数，编译器应会阻止开发人员尝试此类调用。
- 当方法标记为 `NativeCallable`时，阻止方法组转换为 `delegate`。

不过，这并不是支持 `NativeCallable` 的必要条件。 编译器可以像使用现有语法一样支持 `NativeCallable` 特性。 在强制转换为正确的 `delegate*` 签名之前，程序只需强制转换为 `void*`。 这并不比今天的支持更糟。

``` csharp
void* v = &CloseHandle;
delegate* cdecl<IntPtr, bool> f1 = (delegate* cdecl<IntPtr, bool>)v;
```

### <a name="extensible-set-of-unmanaged-calling-conventions"></a>可扩展的非托管调用约定集

当前 ECMA 335 编码支持的非托管调用约定集已过时。 我们已了解到添加对更多非托管调用约定的支持的请求，例如：

- [vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall) https://github.com/dotnet/coreclr/issues/12120
- StdCall，此 https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750

此功能的设计应允许在将来根据需要扩展一组非托管调用约定。 这些问题包括限制调用约定的有限空间（`IMAGE_CEE_CS_CALLCONV_MASK`中采用了16个值中的12个值）和需要接触的位置数，以便添加新的调用约定。 一种可能的解决方案是使用[`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention)枚举引入表示调用约定的新编码。

https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h 提供了 LLVM 支持的调用约定列表作为参考。 虽然 .NET 不太可能需要支持所有这些文件，但它也会表明调用约定的空间非常丰富。

## <a name="considerations"></a>注意事项

### <a name="allow-instance-methods"></a>允许实例方法

通过利用 `EXPLICITTHIS` CLI 调用约定（在代码中C#指定为 `instance`），可以将该建议扩展为支持实例方法。 这种形式的 CLI 函数指针将 `this` 参数作为函数指针语法的显式第一个参数。

``` csharp
unsafe class Instance {
    void Use() {
        delegate* instance<Instance, string> f = &ToString;
        f(this);
    }
}
```

这听起来很复杂，但在提议中增加一些复杂化。 特别是，由于与调用约定不同的函数指针 `instance` 和 `managed` 会不兼容，即使这两种情况都用于使用相同C#的签名调用托管方法。 此外，在每种情况下，在这种情况下，有一个简单的解决方法是很有价值的：使用 `static` 本地函数。

``` csharp
unsafe class Instance {
    void Use() {
        static string toString(Instance i) = i.ToString();
        delgate*<Instance, string> f = &toString;
        f(this);
    }
}
```

### <a name="dont-require-unsafe-at-declaration"></a>声明时不需要 unsafe

不需要每次使用 `delegate*`都需要 `unsafe`，只需在方法组转换为 `delegate*`时才需要。 这就是核心安全问题的播放（知道在值处于活动状态时不能卸载包含程序集）。 需要在其他位置上 `unsafe`。

这就是设计的最初设计方式。 但产生的语言规则觉得非常笨拙。 无法隐藏这一事实：这是一个指针值，即使没有 `unsafe` 关键字，它仍可查看。 例如，不能允许转换为 `object`，它不能是 `class`的成员，等等。这C#种设计需要对所有指针使用 `unsafe`，因此这种设计遵循这一设计。

开发人员仍可以在 `delegate*` 的值的基础上呈现_安全_包装器，其方式与当今普通指针类型的方式相同。 请考虑：

``` csharp
unsafe struct Action {
    delegate*<void> _ptr;

    Action(delegate*<void> ptr) => _ptr = ptr;
    public void Invoke() => _ptr();
}
```

### <a name="using-delegates"></a>使用委托

`delegate*`，不使用新的语法元素，只需将现有 `delegate` 类型与 `*` 后的类型结合使用：

``` csharp
Func<object, object, bool>* ptr = &object.ReferenceEquals;
```

可以通过使用指定 `CallingConvention` 值的属性来注释 `delegate` 类型来处理调用约定。 缺少特性会表示托管调用约定。

在 IL 中对此进行编码会出现问题。 基础值需要表示为一个指针，但它也必须：

1. 具有唯一的类型，以允许具有不同函数指针类型的重载。
1. 对于跨程序集边界的 OHI 是等效的。

最后一点特别有问题。 这意味着，使用 `Func<int>*` 的每个程序集都必须在元数据中编码等效的类型，即使 `Func<int>*` 是在程序集中定义的，但不进行控制。
此外，在非 mscorlib 的程序集中，使用名称 `System.Func<T>` 定义的任何其他类型必须不同于 mscorlib 中定义的版本。

研究的一个选项是发出 `mod_req(Func<int>) void*`的指针。 但这不起作用，因为 `mod_req` 无法绑定到 `TypeSpec`，因此不能以泛型实例化为目标。

### <a name="named-function-pointers"></a>命名函数指针

函数指针语法可能比较繁琐，尤其是在复杂情况下，例如嵌套函数指针。 不是让开发人员在每次 `delegate`使用时都允许函数指针的命名声明，而不是每次都键入签名。

``` csharp
func* void Action();

unsafe class NamedExample {
    void M(Action a) {
        a();
    }
}
```

此处的问题部分是基础 CLI 基元没有名称，因此，这只是一C#种发明，需要使用一种元数据才能实现。 这是可行的，但这是一个重要的工作。 实质上， C#只需将类型定义表与这些名称一起使用。

此外，当检查命名函数指针的参数时，我们发现它们可以同样适用于许多其他方案。 例如，将命名元组声明为在所有情况下都可以轻松地在所有情况下都需要键入完整的签名。

``` csharp
(int x, int y) Point;

class NamedTupleExample {
    void M(Point p) {
        Console.WriteLine(p.x);
    }
}
```

讨论后，我们决定不允许 `delegate*` 类型的命名声明。 如果我们发现，根据客户使用反馈，需要特别需要此功能，我们将调查适用于函数指针、元组、泛型等的命名解决方案。这种形式的格式可能与其他建议类似，如语言的完整 `typedef` 支持。

## <a name="future-considerations"></a>未来的注意事项

### <a name="static-local-functions"></a>静态本地函数

这是指允许对本地函数进行 `static` 修饰符的[提议](https://github.com/dotnet/csharplang/issues/1565)。 此类函数将保证作为 `static` 和在源代码中指定的确切签名发出。 此类函数应为 `&` 的有效参数，因为它不包含本地函数当前所具有的任何问题

### <a name="static-delegates"></a>静态委托

这是指允许 `delegate` 类型声明的[建议](https://github.com/dotnet/csharplang/issues/302)，这些类型只能引用 `static` 成员。 其优势在于，此类 `delegate` 实例可在性能敏感的情况下免费分配和更好地分配。

如果实现函数指针功能，`static delegate` 建议可能会关闭。此功能的建议优点是分配免费性质。 但最近的调查发现，由于程序集卸载，无法实现。 必须有一个从 `static delegate` 到它所引用的方法的强句柄，以便使程序集无法从其下卸载。

若要维护每个 `static delegate` 实例，需要分配一个运行该句柄目标的新句柄。 在某些设计中，分配可能会分摊到每个调用站点的单个分配，但这有点复杂，不值得权衡。

这意味着开发人员实质上必须决定以下权衡：

1. 程序集卸载面上的安全性：这需要分配，因此 `delegate` 已是一个足够的选项。
1. 程序集卸载没有任何安全：使用 `delegate*`。 这可以包装在 `struct` 中，以允许在其余代码的外部使用 `unsafe` 上下文。
