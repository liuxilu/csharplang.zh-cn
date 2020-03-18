---
ms.openlocfilehash: d6519ff57b4a98c4eec8ccbf310303432ac3255e
ms.sourcegitcommit: 65ea1e6dc02853e37e7f2088e2b6cc08d01d1044
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "79483948"
---
# <a name="ranges"></a>范围

## <a name="summary"></a>总结

此功能与提供两个新的运算符，它们允许构造 `System.Index` 和 `System.Range` 对象，并使用它们在运行时索引/切片集合。

## <a name="overview"></a>概述

### <a name="well-known-types-and-members"></a>已知类型和成员

若要将新的句法形式用于 `System.Index` 和 `System.Range`，可能需要新的已知类型和成员，具体取决于所使用的语法格式。

若要使用 "hat" 运算符（`^`），需要满足以下要求

```csharp
namespace System
{
    public readonly struct Index
    {
        public Index(int value, bool fromEnd);
    }
}
```

若要在数组元素访问中使用 `System.Index` 类型作为参数，需要以下成员：

```csharp
int System.Index.GetOffset(int length);
```

`System.Range` 的 `..` 语法将要求 `System.Range` 类型，以及以下一个或多个成员：

```csharp
namespace System
{
    public readonly struct Range
    {
        public Range(System.Index start, System.Index end);
        public static Range StartAt(System.Index start);
        public static Range EndAt(System.Index end);
        public static Range All { get; }
    }
}
```

`..` 语法允许不存在它的任何一个、两个或不存在任何参数。 无论参数的数量如何，`Range` 构造函数始终都足以使用 `Range` 语法。 但是，如果存在任何其他成员并且缺少一个或多个 `..` 参数，则相应的成员可能会被替换。

最后，对于要在数组元素访问表达式中使用 `System.Range` 类型的值，必须存在以下成员：

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeHelpers
    {
        public static T[] GetSubArray<T>(T[] array, System.Range range);
    }
}
```

### <a name="systemindex"></a>System.object

C#无法从末尾开始为集合编制索引，而是大多数索引器使用 "从开始" 概念，或执行 "长度-i" 表达式。 我们引入了一个表示 "从末尾" 的新索引表达式。 此功能将引入一个新的一元前缀 "hat" 运算符。 其单个操作数必须可转换为 `System.Int32`。 它将降低为适当的 `System.Index` 工厂方法调用。

我们通过以下附加语法形式补充*unary_expression*的语法：

```antlr
unary_expression
    : '^' unary_expression
    ;
```

我们*从 end*运算符将此索引称为索引。 最终运算符*中*的预定义索引如下所示：

```csharp
    System.Index operator ^(int fromEnd);
```

仅为大于或等于零的输入值定义此运算符的行为。

示例：

```csharp
var thirdItem = list[2];                // list[2]
var lastItem = list[^1];                // list[Index.CreateFromEnd(1)]

var multiDimensional = list[3, ^2]      // list[3, Index.CreateFromEnd(2)]
```

#### <a name="systemrange"></a>System.object

C#没有语法方法来访问集合的 "范围" 或 "切片"。 通常，用户被迫实现复杂的结构来筛选/操作内存的切片，或利用 `list.Skip(5).Take(2)`等 LINQ 方法。 添加了 `System.Span<T>` 和其他类似类型后，就可以在语言/运行时中的更深级别上支持这种操作，并使接口具有统一的。

语言将引入新的范围运算符 `x..y`。 它是一个接受两个表达式的二元中缀运算符。 可以省略任一操作数（以下示例），并且它们必须可转换为 `System.Index`。 它将降低为适当的 `System.Range` 工厂方法调用。

-我们将C# *multiplicative_expression*的语法规则替换为以下内容（以便引入新的优先级别）：

```antlr
range_expression
    : unary_expression
    | range_expression? '..' range_expression?
    ;

multiplicative_expression
    : range_expression
    | multiplicative_expression '*' range_expression
    | multiplicative_expression '/' range_expression
    | multiplicative_expression '%' range_expression
    ;
```

所有形式的*range 运算符*都具有相同的优先级。 此新优先级组低于*一元运算符*，并且高于*mulitiplicative 算术运算符*。

我们将 `..` 运算符称为*范围运算符*。 可以大致理解内置范围运算符，使其与此窗体的内置运算符的调用相对应：

```csharp
    System.Range operator ..(Index start = 0, Index end = ^0);
```

示例：

```csharp
var slice1 = list[2..^3];               // list[Range.Create(2, Index.CreateFromEnd(3))]
var slice2 = list[..^3];                // list[Range.ToEnd(Index.CreateFromEnd(3))]
var slice3 = list[2..];                 // list[Range.FromStart(2)]
var slice4 = list[..];                  // list[Range.All]

var multiDimensional = list[1..2, ..]   // list[Range.Create(1, 2), Range.All]
```

此外，`System.Index` 应从 `System.Int32`中进行隐式转换，以便不需要对混合使用多维签名的整数和索引进行重载。

## <a name="adding-index-and-range-support-to-existing-library-types"></a>向现有库类型添加索引和范围支持

### <a name="implicit-index-support"></a>隐式索引支持

该语言将为满足以下条件的类型提供一个实例索引器成员，其中包含一个类型为 `Index` 类型的参数：

- 类型为可数。
- 该类型具有可访问的实例索引器，该索引器使用单个 `int` 作为参数。
- 该类型没有可访问的实例索引器，该索引器将 `Index` 用作第一个参数。 `Index` 必须是唯一的参数，否则剩余的参数必须是可选的。

如果某个类型具有一个名为 `Length` 的属性或具有可访问的 getter 的 `Count` 并且返回类型为 `int`，则该类型为***可数***。 语言可以利用此属性将类型 `Index` 的表达式转换为表达式点 `int`，而无需全部使用该 `Index` 类型。 如果同时存在 `Length` 和 `Count`，则需要 `Length`。 为方便起见，该建议将使用名称 `Length` 来表示 `Count` 或 `Length`。

对于此类类型，该语言将充当格式 `T this[Index index]` 的索引成员，其中 `T` 是基于 `int` 的索引器的返回类型，包括任何 `ref` 样式批注。 新成员将具有与 `int` 索引器匹配的可访问性 `set` 成员 `get` 和。 

通过将类型 `Index` 的参数转换为 `int` 并发出对基于 `int` 的索引器的调用来实现新的索引器。 出于讨论目的，我们使用 `receiver[expr]`的示例。 将 `expr` 转换为 `int` 的方式如下：

- 当参数的格式为 `^expr2` 并且 `expr2` 类型 `int`时，它将转换为 `receiver.Length - expr2`。
- 否则，它将转换为 `expr.GetOffset(receiver.Length)`。

这允许开发人员在现有类型上使用 `Index` 功能，而无需修改。 例如：

``` csharp
List<char> list = ...;
var value = list[^1]; 

// Gets translated to 
var value = list[list.Count - 1]; 
```

将对 `receiver` 和 `Length` 表达式进行适当的溢出，确保只执行一次任何副作用。 例如：

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int this[int index] => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get()[^1];
        Console.WriteLine(i);
    }
}
```

此代码将打印 "获取长度 3"。

### <a name="implicit-range-support"></a>隐式范围支持

该语言将为满足以下条件的类型提供一个实例索引器成员，其中包含一个类型为 `Range` 类型的参数：

- 类型为可数。
- 该类型具有一个名为 `Slice` 的可访问成员，它具有两个类型为 `int`的参数。
- 该类型没有实例索引器，该索引器使用单个 `Range` 作为第一个参数。 `Range` 必须是唯一的参数，否则剩余的参数必须是可选的。

对于这种类型的情况，语言将绑定为，因为 `T` 是形式 `T this[Range range]` 的索引成员，其中是包含任何 `ref` 样式批注的 `Slice` 方法的返回类型。 新成员还将具有与 `Slice`匹配的可访问性。 

在名为 `receiver`的表达式上绑定基于 `Range` 的索引器时，会将 `Range` 表达式转换为两个值，然后将这些值传递给 `Slice` 方法，从而降低。 出于讨论目的，我们使用 `receiver[expr]`的示例。

将通过以下方式转换类型化表达式来获取 `Slice` 的第一个参数：

- 如果 `expr` 的格式为 `expr1..expr2` （其中可以省略 `expr2`），并且 `expr1` 的类型为 `int`，则会将其作为 `expr1`发出。
- 当 `expr` 格式为 `^expr1..expr2` （其中 `expr2` 可以省略）时，它将作为 `receiver.Length - expr1`发出。
- 当 `expr` 格式为 `..expr2` （其中 `expr2` 可以省略）时，它将作为 `0`发出。
- 否则，它将作为 `expr.Start.GetOffset(receiver.Length)`发出。

此值将在第二个 `Slice` 参数的计算中重复使用。 执行此操作时，它将被称为 `start`。 将通过以下方式转换范围类型化表达式来获取 `Slice` 的第二个参数：

- 如果 `expr` 的格式为 `expr1..expr2` （其中可以省略 `expr1`），并且 `expr2` 的类型为 `int`，则会将其作为 `expr2 - start`发出。
- 当 `expr` 格式为 `expr1..^expr2` （其中 `expr1` 可以省略）时，它将作为 `(receiver.Length - expr2) - start`发出。
- 当 `expr` 格式为 `expr1..` （其中 `expr1` 可以省略）时，它将作为 `receiver.Length - start`发出。
- 否则，它将作为 `expr.End.GetOffset(receiver.Length) - start`发出。

将对 `receiver`、`Length` 和 `expr` 表达式进行适当的溢出，以确保只执行一次任何副作用。 例如：

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int[] Slice(int start, int length) { 
        var slice = new int[length];
        Array.Copy(_array, start, slice, 0, length);
        return slice;
    }
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        var array = Get()[0..2];
        Console.WriteLine(array.length);
    }
}
```

此代码将打印 "获取长度 2"。

此语言将用以下已知类型作为特例： 

- `string`：将使用方法 `Substring`，而不是 `Slice`。
- `array`：将使用方法 `System.Reflection.CompilerServices.GetSubArray`，而不是 `Slice`。

## <a name="alternatives"></a>备选项

新运算符（`^` 和 `..`）是语法糖。 此功能可以通过显式调用来实现 `System.Index` 和 `System.Range` 工厂方法，但它会导致更多样板代码，并将 unintuitive 体验。

## <a name="il-representation"></a>IL 表示形式

这两个运算符将降低为常规索引器/方法调用，后续编译器层不会更改。

## <a name="runtime-behavior"></a>运行时行为

- 编译器可以优化内置类型（如数组和字符串）的索引器，并将索引的索引减小到适当的现有方法。
- 如果用负值构造，则会引发 `System.Index`。
- `^0` 不会引发，但会转换为其提供的集合/可枚举的长度。
- `Range.All` 在语义上等效于 `0..^0`，可以析构这些索引。

## <a name="considerations"></a>注意事项

### <a name="detect-indexable-based-on-icollection"></a>基于 ICollection 检测可索引

此行为的灵感是集合初始值设定项。 使用类型的结构来表明它已选择加入功能。 如果集合初始值设定项类型可通过实现接口 `IEnumerable` （非泛型）来选择功能。

本建议最初要求类型实现 `ICollection`，才能将其限定为可索引。 但这需要一些特殊情况：

- `ref struct`：这些类无法实现接口，如 `Span<T>` 是索引/范围支持的理想选择。 
- `string`：不实现 `ICollection` 并添加 `interface` 的成本较高。

这意味着支持已需要的密钥类型特殊大小写。 `string` 的特殊大小写不太感兴趣，因为语言在其他区域（`foreach` 降低、常量等）中执行此功能。`ref struct` 的特殊大小写更多，因为它是整个类型类的特殊大小写。 如果它们只具有一个名为 `Count` 的属性，且返回类型为 `int`，则它们将被标记为可索引。 

考虑到该设计已规范化为，表示具有属性 `Count` / `Length` 具有返回类型 `int` 的任何类型都可以编制索引。 这将删除所有特殊大小写，即使对于 `string` 和数组也是如此。

### <a name="detect-just-count"></a>仅检测计数

检测属性名称 `Count` 或 `Length` 确实会使设计复杂化。 只需选择一项进行标准化即可实现标准化，因为它最终会排除大量类型：

- 使用 `Length`：排除 System.web 和子命名空间中的每个集合。 它们往往派生自 `ICollection`，因此更喜欢 `Count` 长度。
- 使用 `Count`：排除 `string`、数组、`Span<T>` 和基于 `ref struct` 的大多数类型

在其他方面，对可编制索引的类型的初始检测的额外问题在有点简化。

### <a name="choice-of-slice-as-a-name"></a>将切片选为名称

选择名称 `Slice` 因为它是 .NET 中切片样式操作的实际标准名称。 从 netcoreapp 2.1 开始，所有范围样式类型使用切片操作的名称 `Slice`。 在 netcoreapp 2.1 之前，没有任何关于示例的切片的示例。 `List<T>`、`ArraySegment<T>`、`SortedList<T>` 等类型对切片而言非常理想，但添加类型时该概念并不存在。 

因此，`Slice` 是唯一的示例，则将其选择为名称。

### <a name="index-target-type-conversion"></a>索引目标类型转换

在索引器表达式中查看 `Index` 转换的另一种方法是作为目标类型转换。 此语言不会像 `return_type this[Index]`形式的成员那样绑定，而是将目标类型化转换分配给 `int`。 

此概念可以通用化到对可数类型的所有成员访问。 只要将类型 `Index` 的表达式用作实例成员调用的参数，且接收方可数，则表达式将具有到 `int`的目标类型转换。 适用于此转换的成员调用包括方法、索引器、属性、扩展方法等。仅排除构造函数，因为它们没有接收方。 

对于具有一种类型的 `Index`的任何表达式，将按如下所示实现目标类型转换。 出于讨论目的，可以使用 `receiver[expr]`的示例：

- 当 `expr` 的形式为 `^expr2` 并且 `expr2` 类型 `int`时，它将转换为 `receiver.Length - expr2`。
- 否则，它将转换为 `expr.GetOffset(receiver.Length)`。

将对 `receiver` 和 `Length` 表达式进行适当的溢出，确保只执行一次任何副作用。 例如：

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int GetAt(int index) => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get().GetAt(^1);
        Console.WriteLine(i);
    }
}
```

此代码将打印 "获取长度 3"。 

对于具有表示索引的参数的任何成员，此功能将非常有用。 例如，`List<T>.InsertAt`。 这也可能会造成混淆，因为该语言不能提供任何有关表达式是否用于索引的指导。 在调用可数类型的成员时，它可以将任何 `Index` 表达式转换为 `int`。 

限制：

- 仅当类型 `Index` 的表达式直接是成员的参数时，此转换才适用。 它不适用于任何嵌套表达式。

## <a name="decisions-made-during-implementation"></a>在实现过程中做出的决策

- 模式中的所有成员都必须是实例成员
- 如果找到长度方法但其返回类型错误，则继续查找计数
- 用于索引模式的索引器必须正好有一个 int 参数
- 用于范围模式的切片方法必须正好有两个 int 参数
- 查找模式成员时，查找原始定义，而不是构造成员

## <a name="design-meetings"></a>设计会议

- https://github.com/dotnet/csharplang/blob/master/meetings/2019/LDM-2019-04-01.md

## <a name="questions"></a>问题
