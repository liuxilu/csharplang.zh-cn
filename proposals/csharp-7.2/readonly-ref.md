---
ms.openlocfilehash: 39fb0aab5e8bb0d422f25fd2e92ab3d8256d3f59
ms.sourcegitcommit: b8f1103eb686c5d82e294837c9386d9b667da292
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2019
ms.locfileid: "79484020"
---
# <a name="readonly-references"></a>只读引用

* [x] 建议
* [x] 原型
* [x] 实现：已启动
* [] 规范：未启动

## <a name="summary"></a>总结
[summary]: #summary

"只读引用" 功能实际上是一组功能，这些功能利用通过引用传递变量的效率，但不会将数据公开给修改：  
- `in` 参数
- `ref readonly` 返回
- `readonly` 结构
- `ref`/`in` 扩展方法
- `ref readonly` 局部变量
- `ref` 条件表达式

## <a name="passing-arguments-as-readonly-references"></a>作为 readonly 引用传递参数。

现有的建议与本主题 https://github.com/dotnet/roslyn/issues/115 为只读参数的特例，而不会有许多详细信息。
在这里，我只是要承认自己的想法并不是很新的。

### <a name="motivation"></a>动机

在此功能C#之前，没有一种有效的方法来表示将结构变量传递到方法调用以实现 readonly，无需修改。 标准的按值参数传递意味着复制，这会增加不必要的成本。  这会使用户使用按引用参数传递，并依赖注释/文档来指示数据不应由被调用方转变。 这不是一个很好的解决方案，原因很多。  
这些示例是图形库中的大量矢量/矩阵数学运算符，[例如，](https://msdn.microsoft.com/library/bb194944.aspx)由于性能方面的考虑，它会知道，只需使用 ref 操作数。 Roslyn 编译器本身有一些代码，它使用结构来避免分配，并按引用传递它们，以避免复制成本。

### <a name="solution-in-parameters"></a>解决方案（`in` 参数）

与 `out` 参数类似，`in` 参数作为托管引用传递，并具有来自被调用方的其他保证。  
与在任何其他使用之前_必须_由被调用方分配的 `out` 参数不同，`in` 的参数无法由被调用方分配。

因此，`in` 参数可实现间接参数传递的有效性，而不会向被调用方公开突变的参数。

### <a name="declaring-in-parameters"></a>声明 `in` 参数

`in` 参数是使用 `in` 关键字作为参数签名中的修饰符来声明的。

出于所有目的，`in` 参数被视为 `readonly` 变量。 在方法中对 `in` 参数的使用的大多数限制与 `readonly` 字段相同。

> 的确 `in` 参数可以表示 `readonly` 字段。 限制的相似性并不是一种巧合。

对于包含结构类型 `in` 参数的示例字段，它们都以递归方式分类为 `readonly` 变量。

```csharp
static Vector3 Add (in Vector3 v1, in Vector3 v2)
{
    // not OK!!
    v1 = default(Vector3);

    // not OK!!
    v1.X = 0;

    // not OK!!
    foo(ref v1.X);

    // OK
    return new Vector3(v1.X + v2.X, v1.Y + v2.Y, v1.Z + v2.Z);
}
```

- 允许使用普通 byval 参数的任何位置都允许 `in` 参数。 这包括索引器、运算符（包括转换）、委托、lambda、本地函数。

> ```csharp
>  (in int x) => x                                                     // lambda expression  
>  TValue this[in TKey index];                                         // indexer
>  public static Vector3 operator +(in Vector3 x, in Vector3 y) => ... // operator
>  ```

- 不允许将 `in` 与 `out` 或 `out` 未合并的任何内容结合使用。

- 不允许重载 `ref`/`out`/`in` 差异。

- 允许重载普通 byval，并 `in` 差异。

- 出于 OHI （重载、隐藏和实现）的目的，`in` 的行为类似于 `out` 参数。
所有相同的规则都适用。
例如，重写方法必须与具有可转换类型的 `in` 参数的 `in` 参数匹配。

- 出于委托/lambda/方法组转换的目的，`in` 的行为类似于 `out` 参数。
Lambda 和适用的方法组转换候选项必须与具有可转换类型的 `in` 参数的目标委托 `in` 参数匹配。

- 出于泛型方差的目的，`in` 参数是 nonvariant 的。

> 注意：对于具有引用类型或基元类型 `in` 参数没有警告。
通常，它可能是毫无意义的，但在某些情况下，用户必须/希望将基元作为 `in`传递。 示例-在 `T` 被替换为 `int`时，或在具有 `Volatile.Read(in int location)` 类似的方法时重写泛型方法，如 `Method(in T param)`
>
> 可以让分析器在低效使用 `in` 参数的情况下发出警告，但这种分析的规则会过于模糊，无法成为语言规范的一部分。

### <a name="use-of-in-at-call-sites-in-arguments"></a>在调用站点使用 `in`。 （`in` 参数）

可以通过两种方式将参数传递给 `in` 参数。

#### <a name="in-arguments-can-match-in-parameters"></a>`in` 参数可以与 `in` 参数匹配：

调用站点上带有 `in` 修饰符的参数可以与 `in` 参数匹配。

```csharp
int x = 1;

void M1<T>(in T x)
{
  // . . .
}

var x = M1(in x);  // in argument to a method

class D
{
    public string this[in Guid index];
}

D dictionary = . . . ;
var y = dictionary[in Guid.Empty]; // in argument to an indexer
```

- `in` 参数必须是_可读_的左值（*）。
示例： `M1(in 42)` 无效

> (*)[LValue/RValue](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue)的概念因语言而异。  
此处，按左值 I 表示一个表达式，该表达式表示可以直接引用的位置。
和 RValue 表示一个表达式，该表达式产生的临时结果不会自行保存。  

- 具体而言，将 `readonly` 字段、`in` 参数或其他正式 `readonly` 变量作为 `in` 参数传递是有效的。
示例： `dictionary[in Guid.Empty]` 是合法的。 `Guid.Empty` 为静态只读字段。

- `in` 参数的类型必须能够_转换_为参数的类型。
示例： `M1<object>(in Guid.Empty)` 无效。 `Guid.Empty` 不能_转换_为 `object`

上述规则的动机是 `in` 参数可保证参数变量的_别名_。 被调用方始终接收对由参数表示的同一位置的直接引用。

- 在极少数情况下，由于用作相同调用的操作数的 `await` 表达式，`in` 参数必须为堆栈溢出，因此该行为与 `out` 和 `ref` 参数相同-如果无法以引用透明方式溢出变量，则会报告错误。

示例：
1. `M1(in staticField, await SomethingAsync())` 有效。
`staticField` 是一种静态字段，可在不具有明显副作用的情况下多次访问。 因此，可以提供副作用和别名要求的顺序。
2. `M1(in RefReturningMethod(), await SomethingAsync())` 将产生一个错误。
`RefReturningMethod()` 是 `ref` 返回方法。 方法调用可能具有可观察到的副作用，因此必须在 `SomethingAsync()` 操作数之前计算。 但是，调用的结果是不能在 `await` 挂起点上保留的引用，这不会导致直接引用要求。

> 注意： stack 溢出错误被视为特定于实现的限制。 因此，它们不会影响重载决策或 lambda 推理。

#### <a name="ordinary-byval-arguments-can-match-in-parameters"></a>普通 byval 参数可以匹配 `in` 参数：

不带修饰符的普通参数可以与 `in` 参数匹配。 在这种情况下，参数具有的宽松约束与普通 byval 参数相同。

此方案的动机是，如果不能将参数传递为直接引用（例如：文本、计算或 `await`的结果，或发生更具体的类型的参数），则 Api 中的 `in` 参数可能会导致不便用户。  
所有这些情况都有一种简单的解决方案，可将参数值存储在适当类型的临时本地并作为 `in` 参数传递该本地。  
若要减少此类样板代码的需求，可以在需要时执行相同的转换（如果需要） `in` 修饰符在调用站点上不存在。  

此外，在某些情况下（例如运算符或 `in` 扩展方法的调用），根本无法指定 `in`。 只需在匹配 `in` 参数时指定普通 byval 参数的行为。

具体而言：

- 传递右是有效的。
在这种情况下，将传递对临时的引用。
示例：
```csharp
Print("hello");      // not an error.

void Print<T>(in T x)
{
  //. . .
}
```

- 允许使用隐式转换。

> 这实际上是传递 RValue 的一种特殊情况  

在这种情况下，将传递对临时保存转换值的引用。
示例：
```csharp
Print<int>(Short.MaxValue)     // not an error.
```

- 如果 `in` 扩展方法的接收方（而不是 `ref` 扩展方法），则允许使用右或隐式的_参数转换_。
在这种情况下，将传递对临时保存转换值的引用。
示例：
```csharp
public static IEnumerable<T> Concat<T>(in this (IEnumerable<T>, IEnumerable<T>) arg)  => . . .;

("aa", "bb").Concat<char>()    // not an error.
```
本文档中进一步介绍了 `ref`/`in` 扩展方法的详细信息。

- 由于 `await` 操作数可能会在必要时溢出 "按值"，因此参数溢出。
在提供对参数的直接引用不可能的情况下，由于介入 `await` 将改为溢出参数值的副本。  
示例：
```csharp
M1(RefReturningMethod(), await SomethingAsync())   // not an error.
```
由于有副作用的调用的结果是不能跨 `await` 挂起保留的引用，因此将保留包含实际值的临时值（与在普通 byval 参数情况下的情况相同）。

#### <a name="omitted-optional-arguments"></a>省略的可选参数

允许 `in` 参数指定默认值。 这使相应参数成为可选参数。

在调用站点省略可选参数会导致通过临时传递默认值。

```csharp
Print("hello");      // not an error, same as
Print("hello", c: Color.Black);

void Print(string s, in Color c = Color.Black)
{
    // . . .
}
```

### <a name="aliasing-behavior-in-general"></a>别名行为概述

与 `ref` 和 `out` 变量一样，`in` 变量是现有位置的引用/别名。

当被调用方不允许向其中写入时，读取 `in` 参数可能会将不同的值视为其他计算的副作用。

示例：

```C#
static Vector3 v = Vector3.UnitY;

static void Main()
{
    Test(v);
}

static void Test(in Vector3 v1)
{
    Debug.Assert(v1 == Vector3.UnitY);
    // changes v1 deterministically (no races required)
    ChangeV();
    Debug.Assert(v1 == Vector3.UnitX);
}

static void ChangeV()
{
    v = Vector3.UnitX;
}
```

### <a name="in-parameters-and-capturing-of-local-variables"></a>`in` 参数和捕获局部变量。  
出于 lambda/async 捕获的目的 `in` 参数的行为与 `out` 和 `ref` 参数相同。

- 无法在闭包中捕获 `in` 参数
- 迭代器方法中不允许使用 `in` 参数
- 异步方法中不允许使用 `in` 参数

### <a name="temporary-variables"></a>临时变量。  
某些使用 `in` 参数传递可能需要间接使用临时本地变量：  
- 当调用站点使用 `in`时，`in` 参数将始终作为直接别名传递。 在这种情况下，永远不会使用暂时。
- 如果调用站点不使用 `in`，则 `in` 参数不需要是直接别名。 如果参数不是左值，则可以使用临时参数。
- `in` 参数可能具有默认值。 当在调用站点上省略相应的参数时，默认值将通过一个临时值传递。
- `in` 参数可能具有隐式转换，包括不保留标识的参数。 在这种情况下，将使用一个临时。
- 普通结构调用的接收方可能不是可写的左值（**现有事例！** ）。 在这种情况下，将使用一个临时。

自变量临时内存的生存时间与调用站点最接近的范围。

临时变量的正式生存时间对于涉及引用返回的变量的转义分析的方案，在语义上非常重要。

### <a name="metadata-representation-of-in-parameters"></a>`in` 参数的元数据表示形式。
将 `System.Runtime.CompilerServices.IsReadOnlyAttribute` 应用于 byref 参数时，这意味着该参数是一个 `in` 参数。

此外，如果该方法为*抽象*方法或*虚*方法，则此类参数（并且只有这样的参数）的签名必须具有 `modreq[System.Runtime.InteropServices.InAttribute]`。

**动机**：这样做是为了确保在方法重写/实现 `in` 参数匹配的情况下。

相同要求适用于委托中的 `Invoke` 方法。

**动机**：这是为了确保在创建或分配委托时现有编译器不能简单地忽略 `readonly`。

## <a name="returning-by-readonly-reference"></a>由 readonly 引用返回。

### <a name="motivation"></a>动机
此子功能的动机大致对称到 `in` 参数的原因-避免复制，而是在返回方。 在此功能之前，方法或索引器有两个选项：1）返回引用并公开给可能的突变或2）返回的值会导致复制。

### <a name="solution-ref-readonly-returns"></a>解决方案（`ref readonly` 返回）  
此功能允许成员按引用返回变量，而不会将它们公开给突变。

### <a name="declaring-ref-readonly-returning-members"></a>声明 `ref readonly` 返回成员

返回签名 `ref readonly` 修饰符的组合用于指示成员返回 readonly 引用。

对于所有目的，将 `ref readonly` 成员视为 `readonly` 变量-类似于 `readonly` 字段和 `in` 参数。

例如，具有结构类型 `ref readonly` 成员的字段将以递归方式分类为 `readonly` 变量。 -允许将它们作为 `in` 参数传递，但不能作为 `ref` 或 `out` 参数传递。

```csharp
ref readonly Guid Method1()
{
}

Method2(in Method1()); // valid. Can pass as `in` argument.

Method3(ref Method1()); // not valid. Cannot pass as `ref` argument
```

- 同一位置允许 `ref readonly` 返回 `ref` 允许返回。
这包括索引器、委托、lambda、本地函数。

- 不允许 /`ref readonly`/差异 `ref`重载。

- 允许重载普通 byval，并 `ref readonly` 返回差异。

- 出于 OHI （重载、隐藏和实现）的目的，`ref readonly` 类似于 `ref`，但不同于。
例如，一种替代 `ref readonly` 方法，它本身必须 `ref readonly` 并且具有可转换的类型。

- 出于委托/lambda/方法组转换的目的，`ref readonly` 类似，但不同于 `ref`。
Lambda 和适用的方法组转换候选项必须匹配 `ref readonly` 返回目标委托，并且该类型的 `ref readonly` 返回为可转换的类型。

- 出于泛型方差的目的，`ref readonly` 返回是 nonvariant 的。

> 注意：对于具有引用类型或基元类型的 `ref readonly` 返回，没有警告。
通常，它可能是毫无意义的，但在某些情况下，用户必须/希望将基元作为 `in`传递。 示例-在 `T` 替换为 `int`时，重写泛型方法，如 `ref readonly T Method()`。
>
>可以让分析器在低效使用 `ref readonly` 返回的情况下发出警告，但这种分析的规则过于模糊，无法成为语言规范的一部分。

### <a name="returning-from-ref-readonly-members"></a>从 `ref readonly` 成员返回
在方法体中，语法与常规引用返回相同。 将从包含方法推断 `readonly`。

动机是 `return ref readonly <expression>` 不必要长时间，只允许 `readonly` 部件上出现不匹配的情况，这将始终导致错误。
但是，与通过严格的别名与按值传递的其他方案相比，`ref` 是一致的。

> 与 `in` 参数的情况不同，`ref readonly` 返回从不通过本地副本返回的。 考虑到，在返回此类做法后，复制会立即停止存在是毫无意义的。 因此 `ref readonly` 返回始终是直接引用。

示例：

```csharp
struct ImmutableArray<T>
{
    private readonly T[] array;

    public ref readonly T ItemRef(int i)
    {
        // returning a readonly reference to an array element
        return ref this.array[i];
    }
}

```

- `return ref` 的参数必须是左值（**现有规则**）
- `return ref` 的参数必须是 "可安全返回" （**现有规则**）
- 在 `ref readonly` 成员中，`return ref` 的参数_不需要是可写_的。
例如，此类成员可以引用 readonly 字段或其 `in` 参数之一。

### <a name="safe-to-return-rules"></a>返回规则的安全。
对于引用的正常返回规则也适用于 readonly 引用。

请注意，`ref readonly` 可以从常规的 `ref` 本地/参数/返回，但不能通过其他方法获取。 否则，`ref readonly` 返回的安全性的推断方式与常规 `ref` 返回的安全性相同。

考虑到右可以作为 `in` 参数传递并返回，`ref readonly` 我们需要另一个规则-**右不安全地按引用返回**。

> 当右值通过复制传递到 `in` 参数，然后以 `ref readonly`形式返回时，请考虑这种情况。 在调用方的上下文中，此类调用的结果是对本地数据的引用，因此不安全返回。
> 右不安全返回后，现有规则 `#6` 已经处理了这种情况。

示例：
```csharp
ref readonly Vector3 Test1()
{
    // can pass an RValue as "in" (via a temp copy)
    // but the result is not safe to return
    // because the RValue argument was not safe to return by reference
    return ref Test2(default(Vector3));
}

ref readonly Vector3 Test2(in Vector3 r)
{
    // this is ok, r is returnable
    return ref r;
}
```

更新 `safe to return` 规则：

1.  **可以安全地返回堆中的变量引用**
2.  可以**安全地返回
`in` 参数的 ref/in 参数**，该参数只能作为 readonly 返回。
3.  **out 参数可以安全返回**（但必须明确赋值，因为现在已经是这样）
4.  **只要接收方可以安全返回，就可以安全地返回实例结构字段**
5.  **"this" 不能安全地从结构成员返回**
6.  **如果作为形参传递给该方法的所有 refs/out 都可以安全返回，则从另一种方法返回的 ref 将是安全的。** *具体而言
，无论接收方是结构、类还是类型为泛型类型参数，都是不相关的。*
7.  **右无法安全地按引用返回。** 
*具体说来，作为 in 参数传递是安全的右。*

> 注意：当涉及引用类型和引用操作时，有关于安全的返回的其他规则。
> 这些规则同样适用于 `ref` 和 `ref readonly` 成员，因此不在此处提及。

### <a name="aliasing-behavior"></a>别名行为。
`ref readonly` 成员提供与普通 `ref` 成员相同的别名行为（只读）。
因此，为了捕获 lambda、async、迭代器、stack 溢出等 .。。同样的限制也适用。 亦. 由于无法捕获实际引用和成员计算的副作用性质，因此不允许这样做。

> 当 `ref readonly` 返回是正则结构方法的接收方时，允许和需要进行复制，这会将 `this` 视为普通的可写引用。 在这种情况下，在所有情况下，都会对只读变量应用本地副本。

### <a name="metadata-representation"></a>元数据表示形式。
当 `System.Runtime.CompilerServices.IsReadOnlyAttribute` 应用于返回 byref 返回方法的时，这意味着该方法返回 readonly 引用。

此外，此类方法的结果签名（并且只有那些方法）必须有 `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`。

**动机**：这是为了确保在调用方法时现有编译器不能简单地忽略 `readonly` `ref readonly` 返回

## <a name="readonly-structs"></a>Readonly 结构
简短的一项功能，它使结构的所有实例成员（构造函数除外） `this` 参数，而 `in` 参数。

### <a name="motivation"></a>动机
编译器必须假定对结构实例的任何方法调用都可以修改该实例。 确实会将可写引用作为 `this` 参数传递给方法，并完全启用此行为。 若要允许在 `readonly` 变量上进行此类调用，请将调用应用到临时副本。 这可能是 unintuitive 的，有时出于性能方面的原因，会强制用户放弃 `readonly`。  
示例： https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/

添加对 `in` 参数的支持后，`ref readonly` 返回防御性复制的问题会变得更糟，因为 readonly 变量将变得更常见。

### <a name="solution"></a>解决方案
允许对结构声明执行 `readonly` 修饰符，这将导致 `this` 被视为除构造函数之外的所有结构实例方法上的 `in` 参数。

```csharp
static void Test(in Vector3 v1)
{
    // no need to make a copy of v1 since Vector3 is a readonly struct
    System.Console.WriteLine(v1.ToString());
}

readonly struct Vector3
{
    . . .

    public override string ToString()
    {
        // not OK!!  `this` is an `in` parameter
        foo(ref this.X);

        // OK
        return $"X: {X}, Y: {Y}, Z: {Z}";
    }
}
```

### <a name="restrictions-on-members-of-readonly-struct"></a>对 readonly 结构成员的限制
- 只读结构的实例字段必须是只读的。  
**动机：** 只能从外部写入，而不能通过成员写入。
- 只读结构的实例 autoproperties 必须是只是获取的。  
**动机：** 对实例字段的限制的影响。
- Readonly 结构不能声明类似于字段的事件。  
**动机：** 对实例字段的限制的影响。

### <a name="metadata-representation"></a>元数据表示形式。
将 `System.Runtime.CompilerServices.IsReadOnlyAttribute` 应用于值类型时，这意味着该类型为 `readonly struct`。

具体而言：
-  `IsReadOnlyAttribute` 类型的标识不重要。 事实上，如果需要，它可以在包含程序集中由编译器嵌入。

## <a name="refin-extension-methods"></a>`ref`/`in` 扩展方法
实际上现有的提议（ https://github.com/dotnet/roslyn/issues/165) 和相应的原型 PR （ https://github.com/dotnet/roslyn/pull/15650)）。
我只想确认这种想法并不完全新。 但在这里，这一点与此相关，因为 `ref readonly` 完美地删除了有关此类方法的最争用资源问题-如何处理右值接收器。

一般思路允许扩展方法按引用获取 `this` 参数，前提是该类型已知为结构类型。

```csharp
public static void Extension(ref this Guid self)
{
    // do something
}
```

编写这种扩展方法的原因主要是：  
1.  接收方是大型结构时避免复制
2.  允许在结构上改变扩展方法

我们不想在类上允许此操作的原因  
1.  它的用途非常有限。
2.  这会中断长时间的固定，方法调用无法将非`null` 接收方转换为在调用后变为 `null`。
> 事实上，除非 `ref` 或 `out`_显式_赋值或传递，否则当前无法 `null` 非`null` 变量。
> 这可以极大帮助提高可读性或其他形式的 "这在此处为空"。
3.  这种情况很难与 "计算一次" 对 null 条件访问语义进行协调。
示例： `obj.stringField?.RefExtension(...)`-需要捕获 `stringField` 的副本以使 null 检查有意义，但随后不会将对 RefExtension 内 `this` 的赋值反映回字段。

在采用第一个参数的**结构**上声明扩展方法的能力是长时间的请求。 其中一个阻止性注意事项是 "如果接收方不是左值，会发生什么情况？"。

- 有一个引用者，还可以将任何扩展方法作为静态方法调用（有时这是解析多义性的唯一方法）。 它会规定应禁止 RValue 接收方。
- 另一方面，在涉及结构实例方法时，可以在类似情况下对副本进行调用。

"隐式复制" 之所以存在的原因是因为大多数结构方法不会实际修改结构，而不能指示。 因此，最可行的解决方案是只对副本进行调用，但这种做法已知会对性能造成损害并引发 bug。

现在，在 `in` 参数的可用性的情况下，扩展可以指示意向。 因此，可以通过使用可写接收方调用 `ref` 扩展来解决难题，并在必要时 `in` 扩展允许隐式复制。

```csharp
// this can be called on either RValue or an LValue
public static void Reader(in this Guid self)
{
    // do something nonmutating.
    WriteLine(self == default(Guid));
}

// this can be called only on an LValue
public static void Mutator(ref this Guid self)
{
    // can mutate self
    self = new Guid();
}
```

### <a name="in-extensions-and-generics"></a>`in` 扩展和泛型。
`ref` 扩展方法的目的是直接或通过调用变异成员来改变接收方。 因此，只要 `T` 被约束为结构，就允许 `ref this T` 扩展。

另一方面 `in` 扩展方法的存在是为了减少隐式复制。 但 `in T` 参数的任何使用都必须通过接口成员来完成。 由于所有接口成员都被视为改变，因此任何此类使用都需要复制。 -不是减少复制，而是相反。 因此，当 `T` 是泛型类型参数，而不考虑约束时，不允许 `in this T`。

### <a name="valid-kinds-of-extension-methods-recap"></a>有效的扩展方法类型（概述）：
现在允许在扩展方法中使用以下形式的 `this` 声明：
1) `this T arg` 规则 byval 扩展。 （**现有事例**）
- T 可以是任何类型，包括引用类型或类型参数。
实例在调用后将是相同的变量。
允许对_此参数转换_类型进行隐式转换。
可在右上调用。

- `in this T self` - `in` 扩展。
T 必须是实际的结构类型。
实例在调用后将是相同的变量。
允许对_此参数转换_类型进行隐式转换。
可在右上调用（如果需要，可以在 temp 上调用）。

- `ref this T self` - `ref` 扩展。
T 必须是结构类型或约束为结构的泛型类型参数。
实例可以通过调用来写入。
仅允许标识转换。
必须在可写的左值上调用。 （从不通过 temp 调用）。

## <a name="readonly-ref-locals"></a>Readonly 引用局部变量。

### <a name="motivation"></a>推动.
一旦引入 `ref readonly` 成员后，就清楚地需要将它们与适当的本地类型成对使用。 成员的计算可能会产生或观察到副作用，因此，如果必须多次使用该结果，则需要对其进行存储。 普通 `ref` 局部变量在此处不起作用，因为无法为它们分配 `readonly` 引用。   

### <a name="solution"></a>解决方案.
允许声明 `ref readonly` 局部变量。 这是一种新的 `ref` 不能写入的局部变量。 因此 `ref readonly` 局部变量可以接受对只读变量的引用，而不会公开这些变量来写入。

### <a name="declaring-and-using-ref-readonly-locals"></a>声明和使用局部变量 `ref readonly`。

此类局部变量的语法使用声明站点（以该特定顺序） `ref readonly` 修饰符。 类似于普通 `ref` 局部变量，`ref readonly` 局部变量必须在声明中进行引用初始化。 与常规 `ref` 局部变量不同，`ref readonly` 局部变量可以引用 `readonly` 左值，如 `in` 参数、`readonly` 字段 `ref readonly` 方法。

对于所有目的，将 `ref readonly` local 视为 `readonly` 变量。 对使用的大多数限制都与 `readonly` 字段或 `in` 参数相同。

对于包含结构类型 `in` 参数的示例字段，它们都以递归方式分类为 `readonly` 变量。   

```csharp
static readonly ref Vector3 M1() => . . .

static readonly ref Vector3 M1_Trace()
{
    // OK
    ref readonly var r1 = ref M1();

    // Not valid. Need an LValue
    ref readonly Vector3 r2 = ref default(Vector3);

    // Not valid. r1 is readonly.
    Mutate(ref r1);

    // OK.
    Print(in r1);

    // OK.
    return ref r1;
}
```

### <a name="restrictions-on-use-of-ref-readonly-locals"></a>`ref readonly` 局部变量的使用限制
除了其 `readonly` 性质外，`ref readonly` 局部变量的行为类似于普通 `ref` 局部变量，并且受到完全相同的限制。  
有关与在闭包中捕获相关的限制，`async` 方法或 `safe-to-return` 分析中的声明同样适用于 `ref readonly` 局部变量。

## <a name="ternary-ref-expressions-aka-conditional-lvalues"></a>三元 `ref` 表达式。 （也称为 "条件左值"）

### <a name="motivation"></a>动机
使用 `ref` 和 `ref readonly` 局部变量时，需要根据条件将此类局部变量替换为一个或另一个目标变量。

典型的解决方法是引入如下方法：

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

请注意，`Choice` 不是三元的完全替换，因为必须在调用站点中计算_所有_参数，这将导致 unintuitive 行为和 bug。

以下操作不会按预期方式工作：

```csharp
    // will crash with NRE because 'arr[0]' will be executed unconditionally
    ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

### <a name="solution"></a>解决方案
允许特殊类型的条件表达式，该表达式的计算结果为基于条件的左值参数之一的引用。

### <a name="using-ref-ternary-expression"></a>使用 `ref` 三元表达式。

条件表达式的 `ref` 风格的语法 `<condition> ? ref <consequence> : ref <alternative>;`

就像普通的条件表达式一样，只根据布尔条件表达式的结果计算 `<consequence>` 或 `<alternative>`。

与普通的条件表达式不同，`ref` 条件表达式：
- 要求 `<consequence>` 和 `<alternative>` 左值。
- `ref` 条件表达式本身是左值和
- 如果 `<consequence>` 和 `<alternative>` 都是可写的左值，则 `ref` 条件表达式是可写的

示例：  
`ref` 三元是左值，因此它可以通过引用传递/赋值/返回;
```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

作为 LValue，还可以分配给。
```csharp
     // assign to
     (arr != null ? ref arr[0]: ref otherArr[0]) = 1;

     // error. readOnlyField is readonly and thus conditional expression is readonly
     (arr != null ? ref arr[0]: ref obj.readOnlyField) = 1;
```

可用作方法调用的接收方，并在必要时跳过复制。
```csharp
     // no copies
     (arr != null ? ref arr[0]: ref otherArr[0]).StructMethod();

     // invoked on a copy.
     // The receiver is `readonly` because readOnlyField is readonly.
     (arr != null ? ref arr[0]: ref obj.readOnlyField).StructMethod();

     // no copies. `ReadonlyStructMethod` is a method on a `readonly` struct
     // and can be invoked directly on a readonly receiver
     (arr != null ? ref arr[0]: ref obj.readOnlyField).ReadonlyStructMethod();
```

也可以在常规（不引用）上下文中使用三元 `ref`。
```csharp
     // only an example
     // a regular ternary could work here just the same
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```

### <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

对于引用和只读引用的增强支持，我可以看到两个主要参数：

1) 此处解决的问题很旧。 为什么现在突然解决它们，尤其是因为它不会帮助现有代码？

正如我们C#在新域中使用的一样，一些问题会变得更加突出。  
作为比计算开销平均更重要的环境的示例，我可以列出

* 用于计算费用和响应能力的云/数据中心方案是一种竞争优势。
* 游戏/VR/AR，其中包含对延迟的软实时要求     

此功能不会牺牲任何现有的优势（如类型安全），同时允许在某些常见情况下降低开销。

2) 我们是否可以合理保证被调用方将在选择 `readonly` 合同时由规则进行播放？

使用 `out`时，有类似的信任关系。 `out` 的实现不正确可能会导致未指定的行为，但在现实中，这种情况很少发生。  

使正式验证规则熟悉 `ref readonly` 会进一步降低信任问题。

### <a name="alternatives"></a>备选项
[alternatives]: #alternatives

主要的竞争设计实际上是 "无任何操作"。

### <a name="unresolved-questions"></a>未解决的问题
[unresolved]: #unresolved-questions

### <a name="design-meetings"></a>设计会议

https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md
