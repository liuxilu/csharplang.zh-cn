---
ms.openlocfilehash: e5ab385f498c0d96c55e60751bb204e7217f6eab
ms.sourcegitcommit: 95f5f86ba2e2a23cd4fb37bd9d1ff690c83d1191
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "81646688"
---
# <a name="function-pointers"></a>函数指针

## <a name="summary"></a>总结

此建议提供语言构造，用于公开当前无法高效访问的 IL 操作代码，或者当前在 C# 中完全访问`ldftn` `calli`IL 操作代码：和 。 这些 IL 操作代码在高性能代码中非常重要，开发人员需要一种有效的方法来访问它们。

## <a name="motivation"></a>动机

此功能的动机和背景在以下问题中描述（该功能的潜在实现）：

https://github.com/dotnet/csharplang/issues/191

这是[编译器内部函数](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)的替代设计建议

## <a name="detailed-design"></a>详细设计

### <a name="function-pointers"></a>函数指针

该语言将允许使用`delegate*`语法声明函数指针。 下一节将详细介绍完整的语法，但它旨在类似于 和`Func``Action`类型声明使用的语法。

``` csharp
unsafe class Example {
    void Example(Action<int> a, delegate*<int, void> f) {
        a(42);
        f(42);
    }
}
```

这些类型使用 ECMA-335 中概述的函数指针类型表示。 这意味着`delegate*`调用 将在`calli`方法上`delegate``callvirt``Invoke`使用 调用 。
语法虽然调用是相同的两个构造。

方法指针的 ECMA-335 定义包括调用约定作为类型签名的一部分（第 7.1 节）。
默认调用约定将为`managed`。 可以通过在`delegate*`语法： `managed`、 `cdecl` `stdcall` `thiscall`、 或`unmanaged`上添加相应的修饰符来指定备用窗体。 示例：

``` csharp
// This method will be invoked using the cdecl calling convention
delegate* cdecl<int, int>;

// This method will be invoked using the stdcall calling convention
delegate* stdcall<int, int>;
```

类型之间的`delegate*`转换基于其签名（包括调用约定）进行。

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

类型`delegate*`是指针类型，这意味着它具有标准指针类型的所有功能和限制：

- 仅在`unsafe`上下文中有效。
- 只能从`unsafe`上下文中调用`delegate*`包含参数或返回类型的方法。
- 无法转换为`object`。
- 不能用作泛型参数。
- 可以隐式`delegate*`转换为`void*`。
- 可以显式从`void*`转换为`delegate*`。

限制：

- 自定义属性不能应用于 或其`delegate*`任何元素。
- 参数`delegate*`不能标记为`params`
- 类型`delegate*`具有法线指针类型的所有限制。

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

调用`unmanaged`约定表示当前平台上本机代码的默认调用约定，并编码为 winapi。
所有`calling_convention`s 都是上下文关键字，前面是`delegate*`。

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

在不安全的上下文中，一组可用的隐式转换（隐式转换）将扩展为包括以下隐式指针转换：
- [_现有转换_](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#pointer-conversions)
- 从_funcptr\_类型_`F0`到另一种_funcptr\_类型_`F1`，前提是以下所有类型都是正确的：
    - `F0`和`F1`具有相同的参数数，并且 中的每个`D0n``F0`参数具有相同的`ref`、`out`或`in`修改器与 中的`D1n``F1`相应参数相同。
    - 对于每个值`ref`参数（没有 的`out`参数， 或`in`修改器），标识转换、隐式引用转换或隐式指针转换从 中的`F0`参数类型到 中的`F1`相应参数类型存在。
    - 对于每个`ref` `out`，`in`或 参数 中`F0`参数类型与 中的`F1`相应参数类型相同。
    - 如果返回类型按值（`ref`否或`ref readonly`），标识、隐式引用或隐式指针转换从 返回`F1`类型到`F0`返回类型存在。
    - `ref`如果返回类型是引用 （ 或`ref readonly`），则 返回`ref``F1`类型和修饰符与`ref``F0`的返回类型和修饰符相同。
    - 的`F0`调用约定与 的`F1`调用约定相同。

### <a name="allow-address-of-to-target-methods"></a>允许对目标方法的地址

方法组现在将被允许作为表达式地址的参数。 此类表达式的类型将是`delegate*`具有目标方法和托管调用约定的等效签名的 表达式的类型：

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

在不安全的上下文中，如果以下所有`M`方法都为 true，则`F`方法与函数指针类型兼容：
- `M`和`F`具有相同的参数数，并且 中的每个`D`参数具有相同的`ref`、`out`或`in`修改器与 中的`F`相应参数相同。
- 对于每个值`ref`参数（没有 的`out`参数， 或`in`修改器），标识转换、隐式引用转换或隐式指针转换从 中的`M`参数类型到 中的`F`相应参数类型存在。
- 对于每个`ref` `out`，`in`或 参数 中`M`参数类型与 中的`F`相应参数类型相同。
- 如果返回类型按值（`ref`否或`ref readonly`），标识、隐式引用或隐式指针转换从 返回`F`类型到`M`返回类型存在。
- `ref`如果返回类型是引用 （ 或`ref readonly`），则 返回`ref``F`类型和修饰符与`ref``M`的返回类型和修饰符相同。
- 的`M`调用约定与 的`F`调用约定相同。
- `M` 是静态方法。

在不安全的`E`上下文中，隐式转换从目标为方法组到兼容函数指针类型的`F`表达式存在，如果`E`包含至少一个以正常形式适用于使用 参数类型和修饰符构建的`F`参数列表的方法，如下文所述。
- 选择与窗体`M``E(A)`的方法调用对应的单个方法，并进行以下修改：
   - 参数`A`列表是表达式的列表，每个表达式都归类为变量，并且具有`ref``out``in``D`相应的_正式\_参数\_列表_的类型和修饰符 （、或 ）。
   - 候选方法只是以正常形式适用的方法，而不是以扩展形式适用的方法。
   - 候选方法仅是静态方法。
- 如果方法调用算法生成错误，则会发生编译时间错误。 否则，算法将生成具有相同参数数的`M`单个最佳方法`F`，并且转换被视为存在。
- 所选方法`M`必须与函数指针类型`F`兼容（如上所述）。 否则，将发生编译时错误。
- 转换的结果是类型的`F`函数指针。

隐式转换从目标为方法`E`组的表达式中存在，`void*`如果 中`M``E`只有一个静态方法。
如果有一个静态方法，则 中的`E`单个最佳方法为`M`。
否则，将发生编译时错误。

这意味着开发人员可以依赖重载解析规则与运营商的地址一起使用：

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

将使用`ldftn`该指令实现运算符的地址。

此功能的限制：

- 仅适用于标记为`static`的方法。
- 非`static`本地函数不能在 中使用`&`。 这些方法的实现细节是故意不由语言指定的。 这包括它们是静态的还是实例的，或者它们发出的签名的确切内容。


### <a name="operators-on-function-pointer-types"></a>函数指针类型的运算符

对运算符的不安全代码中的部分进行了修改：

> 在不安全的上下文中，可以使用多个构造在所有_pointertype_s\_上操作，这些构造不_funcptrtype_s：\_
>
> *  运算符`*`可用于执行指针间接 （[指针间接](unsafe-code.md#pointer-indirection)）。
> *  运算符`->`可用于通过指针（[指针成员访问](unsafe-code.md#pointer-member-access)）访问结构的成员。
> *  运算符`[]`可用于索引指针 （[指针元素访问](unsafe-code.md#pointer-element-access)）。
> *  运算符`&`可用于获取变量的地址（[运算符的地址](unsafe-code.md#the-address-of-operator)）。
> *  和 运算符可用于增量和递减指针（[指针增量和递减）。](unsafe-code.md#pointer-increment-and-decrement) `++` `--`
> *  `+`和`-`运算符可用于执行指针算术 （[指针算术](unsafe-code.md#pointer-arithmetic)）。
> *  `==` `!=`、、、、`>``<``<=`和`=>`运算符可用于比较指针（[指针比较](unsafe-code.md#pointer-comparison)）。
> *  运算符`stackalloc`可用于从调用堆栈（[固定大小缓冲区](unsafe-code.md#fixed-size-buffers)）分配内存。
> *  语句`fixed`可用于临时修复变量，以便可以获取其地址 （[固定语句](unsafe-code.md#the-fixed-statement)）。
> 
> 在不安全的上下文中，可以使用多个构造在所有_funcptrtype_s\_上运行：
> *  运算符`&`可用于获取静态方法的地址（[允许对目标方法的地址](function-pointers.md#allow-address-of-to-target-methods)）
> *  `==` `!=`、、、、`>``<``<=`和`=>`运算符可用于比较指针（[指针比较](unsafe-code.md#pointer-comparison)）。

此外，我们修改中的所有`Pointers in expressions`节以禁止函数指针类型，除 和`Pointer comparison``The sizeof operator`。

### <a name="better-function-member"></a>更好的功能成员

更好的功能成员规范将更改为包括以下行：

> A`delegate*`比`void*`

这意味着可以重载`void*`和 和 仍然`delegate*`明智地使用运算符的地址。

## <a name="metadata-representation-of-in-out-and-ref-readonly-parameters-and-return-types"></a>的元数据表示形式，`ref readonly`和 参数和返回类型`out``in`

函数指针签名没有参数标志位置，因此我们必须编码参数和返回类型是`in`，`out`还是`ref readonly`使用 modreqs。

### `in`

我们重用`System.Runtime.InteropServices.InAttribute`，作为`modreq`引用指定器应用于参数或返回类型，表示以下内容：
* 如果应用于参数引用指定器，则此参数将被视为`in`。
* 如果应用于返回类型 ref 指定器，则返回类型将被视为`ref readonly`。

### `out`

我们使用`System.Runtime.InteropServices.OutAttribute`、作为`modreq`应用于参数类型的 ref 指定器，表示参数是`out`参数。

### <a name="errors"></a>错误

* 将 modreq`OutAttribute`应用于返回类型是错误的。
* 将两者`InAttribute`以及`OutAttribute`作为 modreq 应用于参数类型是错误的。
* 如果通过 modopt 指定任一，则忽略它们。

## <a name="open-issues"></a>Open Issues

### <a name="nativecallableattribute"></a>本机可调用属性

这是 CLR 用于避免调用时托管到本机序言的属性。 使用此属性标记的方法只能从本机代码调用，不受托管（无法调用方法、创建委托等）。 该属性对 mscorlib 不特殊;因此，该属性对 mscorlib 不一应有。运行时将使用此名称处理具有相同语义的任何属性。

运行时和语言可以协同工作以完全支持这一点。 该语言可以选择使用属性`static``NativeCallable`将成员的地址视为`delegate*`具有指定调用约定的成员的地址。

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

此外，语言可能还希望：

- 标记对标记为`NativeCallable`错误的方法的任何托管调用。 给定无法从托管代码调用函数，编译器应阻止开发人员尝试此类调用。
- 防止方法组转换到`delegate`使用`NativeCallable`标记 方法时。

不过，这没有必要支持`NativeCallable`。 编译器可以像使用现有`NativeCallable`语法一样支持该属性。 在转换为正确的`delegate*`签名之前，程序只需`void*`强制转换为。 这不会比今天的支持更糟糕。

``` csharp
void* v = &CloseHandle;
delegate* cdecl<IntPtr, bool> f1 = (delegate* cdecl<IntPtr, bool>)v;
```

### <a name="extensible-set-of-unmanaged-calling-conventions"></a>可扩展的一组非托管调用约定

当前 ECMA-335 编码支持的一组非托管调用约定已过时。 我们已经看到请求添加对更多非托管调用约定的支持，例如：

- [矢量调用](https://docs.microsoft.com/cpp/cpp/vectorcall)https://github.com/dotnet/coreclr/issues/12120
- StdCall 与显式此https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750

此功能的设计应允许将来根据需要扩展一组非托管调用约定。 这些问题包括编码调用约定的空间有限（16 个值中有 12 个在`IMAGE_CEE_CS_CALLCONV_MASK`），以及需要触摸的位置数以添加新调用约定。 一个可能的解决方案是引入一[`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention)种使用枚举表示调用约定的新编码。

作为参考，https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h有 LLVM 支持的调用约定列表。 虽然 .NET 不太可能需要支持所有这些，但它表明调用约定的空间非常丰富。

## <a name="considerations"></a>注意事项

### <a name="allow-instance-methods"></a>允许实例方法

该建议可以通过利用`EXPLICITTHIS`CLI 调用约定（在 C# 代码中命名`instance`）扩展以支持实例方法。 CLI 函数指针的这种形式将`this`参数作为函数指针语法的显式第一个参数。

``` csharp
unsafe class Instance {
    void Use() {
        delegate* instance<Instance, string> f = &ToString;
        f(this);
    }
}
```

这是合理的，但增加了一些复杂的建议。 特别是因为函数指针因调用约定`instance`而异，即使`managed`这两种情况都用于使用相同的 C# 签名调用托管方法，该指针也是不兼容的。 此外，在每种情况下，考虑到这样做是有价值的，有一个简单的工作围绕：使用`static`本地函数。

``` csharp
unsafe class Instance {
    void Use() {
        static string toString(Instance i) = i.ToString();
        delgate*<Instance, string> f = &toString;
        f(this);
    }
}
```

### <a name="dont-require-unsafe-at-declaration"></a>在申报时不需要不安全

每次使用`unsafe``delegate*`时都不需要，而是在方法组转换为`delegate*`的点要求它。 这是核心安全问题发挥作用的地方（知道在值处于活动状态时无法卸载包含的程序集）。 对其他`unsafe`位置要求可能被视为过高。

这就是设计的初衷。 但由此产生的语言规则感到很尴尬。 这是一个指针值，即使没有`unsafe`关键字，它也会不断偷看这一事实， 这是不可能掩盖的。 例如，不能允许转换为`object`，它不能是`class`的成员，等等...C# 设计是所有指针使用`unsafe`的要求，因此此设计遵循此设计。

开发人员仍然能够像今天对普通指针类型_safe_一样，在`delegate*`值之上呈现安全包装器。 请考虑：

``` csharp
unsafe struct Action {
    delegate*<void> _ptr;

    Action(delegate*<void> ptr) => _ptr = ptr;
    public void Invoke() => _ptr();
}
```

### <a name="using-delegates"></a>使用委托

而不是使用新的语法元素，`delegate*`只需使用具有以下`delegate``*`类型的现有类型：

``` csharp
Func<object, object, bool>* ptr = &object.ReferenceEquals;
```

处理调用约定可以通过使用指定`delegate``CallingConvention`值的属性对类型进行说明来完成。 缺少属性表示托管调用约定。

在 IL 中编码此是有问题的。 基础值需要表示为指针，但还必须：

1. 具有唯一类型，允许具有不同函数指针类型的重载。
1. 在装配边界上为 OHI 目的等效。

最后一点特别成问题。 这意味着使用的每个程序集`Func<int>*`都必须在元数据中对等效类型进行编码，即使`Func<int>*`是在程序集中定义的，尽管不控制。
此外，在程序集中使用名称`System.Func<T>`定义的不是 mscorlib 的任何其他类型必须不同于 mscorlib 中定义的版本。

被探讨的一个选项是发出这样的`mod_req(Func<int>) void*`指针， 这不起作用，因为`mod_req`不能绑定到 ，`TypeSpec`因此不能以通用实例化为目标。

### <a name="named-function-pointers"></a>命名函数指针

函数指针语法可能很麻烦，尤其是在嵌套函数指针等复杂情况下。 而不是让开发人员键入签名，每次语言可以允许命名声明的函数指针，就像使用`delegate`一样。

``` csharp
func* void Action();

unsafe class NamedExample {
    void M(Action a) {
        a();
    }
}
```

此处的部分问题是底层 CLI 基元没有名称，因此这纯粹是 C# 发明，需要一些元数据工作才能启用。 这是可行的，但很重要的工作。 它基本上要求 C# 具有仅针对这些名称的类型定义表的配套。

此外，当检查命名函数指针的参数时，我们发现它们可以同样很好地应用于许多其他方案。 例如，声明命名 tup 同样方便，以减少在所有情况下键入完整签名的需要。

``` csharp
(int x, int y) Point;

class NamedTupleExample {
    void M(Point p) {
        Console.WriteLine(p.x);
    }
}
```

经过讨论，我们决定不允许命名`delegate*`类型声明。 如果我们发现有基于客户使用情况反馈的重大需求，那么我们将调查一个命名解决方案，适用于函数指针，tups，泛型等...这在形式上可能与其他建议类似，如语言中的完全支持`typedef`。

## <a name="future-considerations"></a>未来的注意事项

### <a name="static-local-functions"></a>静态局部函数

这是指允许[the proposal](https://github.com/dotnet/csharplang/issues/1565)`static`修改器对局部函数进行的建议。 这种函数将保证在源代码中指定的确切签名`static`时发出。 这样的函数应该是一个有效的参数，`&`因为它不包含任何问题，本地函数今天

### <a name="static-delegates"></a>静态委托

这是指允许声明只能引用`delegate``static`成员的类型[的建议](https://github.com/dotnet/csharplang/issues/302)。 优点是，此类`delegate`实例可以在性能敏感方案中自由分配，更好分配。

如果实现了函数指针功能，`static delegate`则建议可能会关闭。该功能的建议优点是分配自由性质。 然而，最近的调查发现，由于装配卸载，无法实现这一目标。 必须有一个强句柄，从`static delegate`它引用的方法，以防止从它下卸载程序集。

为了维护每个`static delegate`实例，需要分配一个与建议目标背道而驰的新句柄。 有些设计可以摊销到每个调用站点的单个分配，但这有点复杂，似乎不值得权衡。

这意味着开发人员基本上必须决定以下权衡：

1. 面对装配体卸载时的安全性：这需要分配，因此`delegate`已经是一个足够的选择。
1. 在装配卸载时没有安全性：使用`delegate*`。 这可以包装在 中`struct`，以允许在`unsafe`代码的其余部分中的上下文外部使用。
