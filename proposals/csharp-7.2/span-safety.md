---
ms.openlocfilehash: 6088a5cd41926d828013f1b8e5736fd2b7939e44
ms.sourcegitcommit: da452002c3f472165a0e1fa7759f494cc703ae31
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "79483960"
---
# <a name="compile-time-enforcement-of-safety-for-ref-like-types"></a>对类似引用类型执行安全编译的时间

## <a name="introduction"></a>介绍

处理类型（如 `Span<T>` 和 `ReadOnlySpan<T>`）时其他安全规则的主要原因是，此类类型必须局限于执行堆栈。
 
`Span<T>` 和类似类型必须为仅堆栈类型有两个原因。

1. `Span<T>` 在语义上是包含引用和范围 `(ref T data, int length)`的结构。 不考虑实际的实现，因此，对此类结构的写入将不是原子的。 此类结构的并发 "撕裂" 将导致 `length` 与 `data`不匹配，导致超出范围的访问和类型安全冲突，最终可能会导致垃圾回收堆在看似 "safe" 代码中损坏。
2. `Span<T>` 的某些实现在字面上包含一个托管指针。 不支持将托管指针作为堆对象的字段，管理用于将托管指针放置在 GC 堆上的代码通常会在 JIT 时崩溃。

如果 `Span<T>` 的实例被约束为仅存在于执行堆栈中，则上述所有问题都将会缓解。 

由于撰写导致的其他问题。 通常需要生成更复杂的数据类型，以便嵌入 `Span<T>` 和 `ReadOnlySpan<T>` 实例。 此类复合类型必须是结构，并将分担 `Span<T>`的所有危险和要求。 因此，应查看此处所述的安全规则，使其适用于 **_类似于引用类型_** 的整个范围。

[草稿语言规范](#draft-language-specification)旨在确保仅在堆栈上出现类似于引用类型的值。

## <a name="generalized-ref-like-types-in-source-code"></a>源代码中的一般化 `ref-like` 类型

使用 `ref` 修饰符在源代码中显式标记 `ref-like` 结构：

```csharp
ref struct TwoSpans<T>
{
    // can have ref-like instance fields
    public Span<T> first;
    public Span<T> second;
} 

// error: arrays of ref-like types are not allowed. 
TwoSpans<T>[] arr = null;

```

将结构指定为同类引用后，该结构将允许该结构具有类似于引用的实例字段，并且还会使与该结构相关的 ref 类型的所有要求。 

## <a name="metadata-representation-or-ref-like-structs"></a>元数据表示形式或类似于引用的结构

类似于引用的结构将用**system.runtime.compilerservices. IsRefLikeAttribute**属性进行标记。

该属性将添加到公共基库，如 `mscorlib`。 在这种情况下，如果该属性不可用，编译器将生成一个与其他嵌入的按需属性（如 `IsReadOnlyAttribute`）相似的内部属性。

将采取额外的措施来防止在编译器中不熟悉安全规则（这包括C#在其上实现此功能的编译器之前）使用类似于引用的结构。 

如果没有其他适用于旧编译器的替代方法，不提供服务，则会将具有已知字符串的 `Obsolete` 属性添加到所有类似于引用的结构。 知道如何使用类似引用的类型的编译器将忽略此特定形式的 `Obsolete`。

典型的元数据表示形式：

```csharp
    [IsRefLike]
    [Obsolete("Types with embedded references are not supported in this version of your compiler.")]
    public struct TwoSpans<T>
    {
       // . . . .
    }
```

注意：不是这样做的目标，因此在旧编译器上使用类似于引用的类型会导致100%。 这并不是必需的。 例如，总会有一种方法可以使用动态代码来解决 `Obsolete`，例如通过反射创建类似于引用类型的数组。

特别是，如果用户想要在类似于引用的类型上实际放置 `Obsolete` 或 `Deprecated` 属性，则除了不发出预定义的类型之外，我们还没有选择，因为 `Obsolete` 属性不能应用多次。  

## <a name="examples"></a>示例：

```csharp
SpanLikeType M1(ref SpanLikeType x, Span<byte> y)
{
    // this is all valid, unconcerned with stack-referring stuff
    var local = new SpanLikeType(y);
    x = local;
    return x;
}

void Test1(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    // this is allowed
    stackReferring2 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    stackReferring2 = M1(ref param1, stackReferring1);

    // this is NOT allowed
    param1 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    param2 = stackReferring1.Slice(10);

    // this is allowed
    param1 = new SpanLikeType(param2);

    // this is allowed
    stackReferring2 = param1;
}

ref SpanLikeType M2(ref SpanLikeType x)
{
    return ref x;
}

ref SpanLikeType Test2(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    ref var stackReferring3 = M2(ref stackReferring2);

    // this is allowed
    stackReferring3 = M1(ref stackReferring2, stackReferring1);

    // this is allowed
    M2(ref stackReferring3) = stackReferring2;

    // this is NOT allowed
    M1(ref param1) = stackReferring2;

    // this is NOT allowed
    param1 = stackReferring3;

    // this is NOT allowed
    return ref stackReferring3;

    // this is allowed
    return ref param1;
}

```

----------------

## <a name="draft-language-specification"></a>草稿语言规范

下面我们介绍一组类似于引用类型（`ref struct`）的安全规则，以确保这些类型的值仅出现在堆栈上。 如果无法通过引用传递局部变量，则可以使用一组不同的更简单的安全规则。 此规范还允许重新指定 ref 局部变量。

### <a name="overview"></a>概述

在编译时，与每个表达式相关联的是允许该表达式转义的范围的概念，即 "安全到转义"。 同样，对于每个左值，我们都将保留对其引用范围的概念，以允许对其进行转义，即 "引用安全到转义"。 对于给定的左值表达式，这些表达式可能不同。

它们类似于引用局部变量功能的 "安全返回"，但更细化。 如果表达式的 "安全到返回" 只记录了（或不是）它是否可以将封闭方法作为一个整体进行转义，则安全到转义的记录可能会被转义的范围（它可能不会被转义的范围）。 基本安全机制按如下方式强制执行。 给定从具有安全到转义范围 S1 的 expression E1 到（左值）表达式 E2 的赋值，使用安全到转义范围 S2，如果 S2 的范围比 S1 更宽，则是错误的。 按照构造，两个作用域 S1 和 S2 在一个嵌套关系中，因为合法表达式始终是从包含表达式的某些范围中恢复为安全的。

在这种情况下，为了进行分析，只支持两个范围：外部到方法和方法的顶级范围。 这是因为无法创建具有内部范围的类似引用的值，并且引用局部变量不支持重新赋值。 不过，这些规则可支持两个以上的作用域级别。

若要计算表达式的*安全恢复返回*状态和管理表达式合法性的规则，请遵循。

### <a name="ref-safe-to-escape"></a>引用安全-转义

*Ref safe 到 escape*是一个范围，其中包含左值表达式，对 lvalue 的引用可以安全地转义给它。 如果该作用域是整个方法，我们说到左值的引用可以*安全地*从方法返回。

### <a name="safe-to-escape"></a>安全到转义

*安全 to escape*是一个包含表达式的范围，其中的值可以安全地转义。 如果该作用域是整个方法，我们就会说，*可以安全地*从方法返回值。

类型不是 `ref struct` 类型的表达式是从整个封闭方法中*安全返回*的。 否则，请参阅下面的规则。

#### <a name="parameters"></a>parameters

指定形参的左值是引用*安全转义*（按引用），如下所示：
- 如果参数是一个 `ref`、`out`或 `in` 参数，则为整个方法（例如通过 `return ref` 语句）进行*引用安全的转义*;本来
- 如果参数是结构类型的 `this` 参数，则它是对方法的顶级范围（而不是从整个方法本身）进行*引用安全的引用*;[示例](#struct-this-escape)
- 否则，该参数是一个值参数，它是对方法的顶级范围（而不是从方法本身）的*引用安全的转义*。

指定形参使用的右值的表达式（通过值）从整个方法（例如，通过 `return` 语句）进行*安全的转义*。 这也适用于 `this` 参数。

#### <a name="locals"></a>局部变量

指定局部变量的左值是引用*安全*的（按引用），如下所示：
- 如果变量是 `ref` 变量，则它的*ref 安全到转义*将从其初始化表达式的*ref 安全对转义*;本来
- 变量是*引用安全*的，可对它的声明范围进行转义。

指定局部变量使用的右值表达式是*安全转义*（通过值），如下所示：
- 但上面的一般规则是，其类型不是 `ref struct` 类型的本地是*安全地*从整个封闭方法返回的。
- 如果该变量是 `foreach` 循环的迭代变量，则该变量的*safe 到 escape*范围与 `foreach` 循环的表达式的*安全转义*等效。
- 在声明点上，不能从整个封闭方法中*安全地返回*`ref struct` 类型的本地和在声明点未初始化。
- 否则，该变量的类型为 `ref struct` 类型，该变量的声明需要初始值设定项。 变量的*安全到转义*的范围与它的初始值设定项的*安全转义*。

#### <a name="field-reference"></a>字段引用

指定对字段的引用的左值 `e.F`，它是引用*安全引用转义*（按引用），如下所示：
- 如果 `e` 的类型为引用类型，则它是对整个方法的引用*安全*的引用;本来
- 如果 `e` 是值类型，则将从 `e`的*ref 安全到转义*获取它的*ref 安全到转义*。

指定对字段 `e.F`的引用具有*安全到转义*的范围，该范围等同于 `e`的*安全转义*。

#### <a name="operators-including-"></a>包括 `?:` 的运算符

用户定义的运算符的应用程序被视为方法调用。

对于生成右值的运算符（如 `e1 + e2` 或 `c ? e1 : e2`），结果的*安全对转义*是运算符的操作数的*安全转义*的最小范围。  因此，对于产生右值的一元运算符（如 `+e`），该结果的*安全对转义*是操作数的*安全转义*。

对于产生左值的运算符，例如 `c ? ref e1 : ref e2`
- 结果的*ref 安全对转义*是运算符的操作数的*ref 安全到转义*中的最小范围。
- 操作数的*安全对转义*必须一致，这是生成的左值的*安全转义*。

#### <a name="method-invocation"></a>方法调用

由引用返回的方法调用生成的左值 `e1.M(e2, ...)` 是*引用安全*的最小的范围：
- 整个封闭方法
- 所有 `ref` 和 `out` 参数表达式（接收方除外）的*ref 安全转义*
- 对于方法的每个 `in` 参数，如果有一个对应的表达式是左值，则为它的*ref 安全到转义*，否则为最近的封闭范围
- 所有自变量表达式（包括接收方）的*安全转义*

> 注意：必须具有最后一个项目符号才能处理代码，如
> ```csharp
> var sp = new Span(...)
> return ref sp[0];
> ```
> 或
> ```csharp
> return ref M(sp, 0);
> ```

方法调用生成的右值 `e1.M(e2, ...)` 是从以下范围的最小值中*安全到转义*：
- 整个封闭方法
- 所有自变量表达式（包括接收方）的*安全转义*

#### <a name="an-rvalue"></a>右值
右值是从最近的封闭范围中*引用安全的引用*。 例如，在 `M(ref d.Length)` 的调用（其中 `d` 类型 `dynamic`）中，会发生这种情况。 它还与（可能是 subsumes）处理与 `in` 参数相对应的参数的处理方式一致。 *

#### <a name="property-invocations"></a>属性调用

通过上述规则将属性调用（`get` 或 `set`）视为基础方法的方法调用。

#### `stackalloc`

Stackalloc 表达式是对方法的顶级范围（而不是整个方法本身）进行*安全转义*的右值。

#### <a name="constructor-invocations"></a>构造函数调用

调用构造函数的 `new` 表达式将与被视为返回正在构造的类型的方法调用的规则服从。

此外，如果存在初始值设定项，则不会以递归方式在对象初始值设定项表达式的所有参数/操作数的最小*转义*范围内进行*安全到*转义。 

#### <a name="span-constructor"></a>Span 构造函数
语言依赖于 `Span<T>` 不具有以下形式的构造函数：

```csharp
void Example(ref int x)
{
    // Create a span of length one
    var span = new Span<int>(ref x); 
}
```

此类构造函数使 `Span<T>` 用作与 `ref` 字段不区分的字段。 本文档中所述的安全规则依赖于不是或 .NET 中C#的有效构造 `ref` 字段。

#### <a name="default-expressions"></a>`default` 表达式

`default` 表达式是从整个封闭方法中*安全地转义*的。

## <a name="language-constraints"></a>语言约束

我们希望确保没有 `ref` 本地变量，并且没有 `ref struct` 类型的变量引用堆栈内存或不再处于活动状态的变量。 因此，我们具有以下语言约束：

- Ref 参数既不是引用本地，也不能将 `ref struct` 类型的参数或局部变量提升为 lambda 或局部函数。

- `ref struct` 类型的 ref 参数和参数都不能是迭代器方法或 `async` 方法的参数。

- 在 `yield return` 语句或 `await` 表达式的点，引用本地和 `ref struct` 类型都不能在范围内。

- `ref struct` 类型不能用作类型参数或元组类型中的元素类型。

- `ref struct` 类型可能不是字段的声明类型，只是它可能是另一个 `ref struct`的实例字段的声明类型。

- `ref struct` 类型不能是数组的元素类型。

- `ref struct` 类型的值不能装箱：
  - 没有从 `ref struct` 类型到类型 `object` 或类型 `System.ValueType`的转换。
  - 不能声明一个 `ref struct` 类型来实现任何接口
  - 在 `object` 或未在 `System.ValueType` 中声明但在 `ref struct` 类型中未重写的实例方法可以使用该 `ref struct` 类型的接收方调用。
  - 不能通过方法转换为委托类型来捕获 `ref struct` 类型的实例方法。

- 对于引用重新分配 `ref e1 = ref e2`，`e2` 的*ref 安全到转义符*的范围必须至少与 `e1`的*ref 安全对转义*。

- 对于 ref return 语句 `return ref e1`，`e1` 的*ref 安全到转义*必须是整个方法的*引用*安全转义。 （TODO：我们还需要一个规则，该规则 `e1` 必须对整个方法是*安全*的，还是冗余的？）

- 对于 `return e1`的 return 语句，`e1` 的*安全对转义*必须是整个方法的*安全转义*。

- 对于分配 `e1 = e2`，如果 `e1` 类型是 `ref struct` 类型，则 `e2` 的*安全对转义*必须至少为范围，作为 `e1`的*安全到转义*的范围。

- 对于方法调用（如果存在 `ref struct` 类型（包括接收方）的 `ref` 或 `out` 参数（包括接收方）和*安全到转义*e1，则不会有任何参数（包括接收方）的*安全转义转义*比 E1 更小。 [示例](#method-arguments-must-match)

- 本地函数或匿名函数不能引用在封闭范围内声明的 `ref struct` 类型的本地或参数。

> ***打开问题：*** 我们需要一些规则，以便在需要溢出 await 表达式的 `ref struct` 类型的堆栈值时（例如，在代码中）生成错误
> ```csharp
> Foo(new Span<int>(...), await e2);
> ```

## <a name="explanations"></a>方便
这些说明和示例有助于解释上述许多安全规则存在的原因

### <a name="method-arguments-must-match"></a>方法参数必须匹配
如果调用的方法中有 `out`，`ref` 参数是 `ref struct` 包括接收方，则所有 `ref struct` 都需要具有相同的生存期。 这是必需的C# ，因为必须根据方法签名中提供的信息和调用站点上值的生存期，使其所有的决策都围绕生存期安全。 

如果有 `ref` 的参数 `ref struct`，则它们可以在其内容周围交换。 因此，在调用站点，我们必须确保所有这些**可能**的交换都是兼容的。 如果该语言未强制执行该语言，则会允许错误的代码，如下所示。

```csharp
void M1(ref Span<int> s1)
{
    Span<int> s2 = stackalloc int[1];
    Swap(ref s1, ref s2);
}

void Swap(ref Span<int> x, ref int Span<int> y)
{
    // This will effectively assign the stackalloc to the s1 parameter and allow it
    // to escape to the caller of M1
    ref x = ref y; 
}
```

对接收器的限制是必要的，因为它的任何内容都不是引用安全的，它可以存储提供的值。 这意味着，使用不匹配的生存期，可以通过以下方式创建类型安全漏洞：

```csharp
ref struct S
{
    public Span<int> Span;

    public void Set(Span<int> span)
    {
        Span = span;
    }
}

void Broken(ref S s)
{
    Span<int> span = stackalloc int[1];

    // The result of a stackalloc is now stored in s.Span and escaped to the caller
    // of Broken
    s.Set(span); 
}
```

### <a name="struct-this-escape"></a>Struct This Escape
当涉及到跨安全规则时，实例成员中的 `this` 值将建模为成员的参数。 现在，对于 `struct` `this` 的类型实际上是 `ref S`，其中 `class` 只是 `S` （对于名为的 `class / struct` 的成员）。 

但 `this` 具有不同于其他 `ref` 参数的转义规则。 具体而言，它不是引用安全的，而是其他参数：

```csharp
ref struct S
{ 
    int Field;

    // Illegal because this isn't safe to escape as ref
    ref int Get() => ref Field;

    // Legal
    ref int GetParam(ref int p) => ref p;
}
```

与 `struct` 成员调用相比，此限制的原因实际上很少。 对于接收方为右值 `struct` 成员的成员调用，需要遵循一些规则。 但这非常易学。 

此限制的原因实际上就是接口调用。 具体而言，它会出现在下面的示例中是否应编译;

```csharp
interface I1
{
    ref int Get();
}

ref int Use<T>(T p)
    where T : I1
{
    return ref p.Get();
}
```

请考虑将 `T` 实例化为 `struct`的情况。 如果 `this` 参数为 ref-safe to escape，则返回 `p.Get` 可能指向堆栈（具体而言，它可以是 `T`的实例化类型内的字段）。 这意味着，语言不允许此示例编译，因为它可以将 `ref` 返回到堆栈位置。 另一方面，如果 `this` 不是引用安全的 to escape，则 `p.Get` 无法引用堆栈，因此可以安全返回。 

这就是 escapability 在 `struct` 中 `this` 的根本就是与接口有关的原因。 绝对可以正常工作，但它已经有了折衷。 设计最终会使接口更灵活。 

但在今后，我们可能会放松这一点。 

## <a name="future-considerations"></a>未来的注意事项

### <a name="length-one-spant-over-ref-values"></a>长度为一个跨度\<T > 超过 ref 值
虽然目前不合法，但在某些情况下，在一个值上创建一个 `Span<T>` 实例的长度将是有益的：

```csharp
void RefExample()
{
    int x = ...;

    // Today creating a length one Span<int> requires a stackalloc and a new 
    // local
    Span<int> span1 = stackalloc [] { x };
    Use(span1);
    x = span1[0]; 

    // Simpler to just allow length one span
    var span2 = new Span<int>(ref x);
    Use(span2);
}
```

如果我们对[固定大小的缓冲区](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md)提出限制，因为它允许 `Span<T>` 的实例甚至更长，则此功能将变得更加引人注目。 

如果需要关闭此路径，则该语言可通过确保此类 `Span<T>` 实例仅向下来满足此要求。 这就是，它们只是对创建它们的范围而言是*安全*的。 这确保了语言决不需要考虑 `ref` 值：通过 `ref struct`的 `ref struct` 返回或字段来转义方法。 这可能还需要进一步的更改以通过这种方式捕获 `ref` 参数。
