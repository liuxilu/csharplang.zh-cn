---
ms.openlocfilehash: 50f2bd2d0a84064cfe35fe65b9e5c59c052d19ac
ms.sourcegitcommit: 1dbb8e82bed5012a58a3a035bf2c3737ed570d07
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "80122951"
---
# <a name="ranges"></a><span data-ttu-id="7cd69-101">范围</span><span class="sxs-lookup"><span data-stu-id="7cd69-101">Ranges</span></span>

## <a name="summary"></a><span data-ttu-id="7cd69-102">总结</span><span class="sxs-lookup"><span data-stu-id="7cd69-102">Summary</span></span>

<span data-ttu-id="7cd69-103">此功能与提供两个新的运算符，它们允许构造 `System.Index` 和 `System.Range` 对象，并使用它们在运行时索引/切片集合。</span><span class="sxs-lookup"><span data-stu-id="7cd69-103">This feature is about delivering two new operators that allow constructing `System.Index` and `System.Range` objects, and using them to index/slice collections at runtime.</span></span>

## <a name="overview"></a><span data-ttu-id="7cd69-104">概述</span><span class="sxs-lookup"><span data-stu-id="7cd69-104">Overview</span></span>

### <a name="well-known-types-and-members"></a><span data-ttu-id="7cd69-105">已知类型和成员</span><span class="sxs-lookup"><span data-stu-id="7cd69-105">Well-known types and members</span></span>

<span data-ttu-id="7cd69-106">若要将新的句法形式用于 `System.Index` 和 `System.Range`，可能需要新的已知类型和成员，具体取决于所使用的语法格式。</span><span class="sxs-lookup"><span data-stu-id="7cd69-106">To use the new syntactic forms for `System.Index` and `System.Range`, new well-known types and members may be necessary, depending on which syntactic forms are used.</span></span>

<span data-ttu-id="7cd69-107">若要使用 "hat" 运算符（`^`），需要满足以下要求</span><span class="sxs-lookup"><span data-stu-id="7cd69-107">To use the "hat" operator (`^`), the following is required</span></span>

```csharp
namespace System
{
    public readonly struct Index
    {
        public Index(int value, bool fromEnd);
    }
}
```

<span data-ttu-id="7cd69-108">若要在数组元素访问中使用 `System.Index` 类型作为参数，需要以下成员：</span><span class="sxs-lookup"><span data-stu-id="7cd69-108">To use the `System.Index` type as an argument in an array element access, the following member is required:</span></span>

```csharp
int System.Index.GetOffset(int length);
```

<span data-ttu-id="7cd69-109">`System.Range` 的 `..` 语法将要求 `System.Range` 类型，以及以下一个或多个成员：</span><span class="sxs-lookup"><span data-stu-id="7cd69-109">The `..` syntax for `System.Range` will require the `System.Range` type, as well as one or more of the following members:</span></span>

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

<span data-ttu-id="7cd69-110">`..` 语法允许不存在它的任何一个、两个或不存在任何参数。</span><span class="sxs-lookup"><span data-stu-id="7cd69-110">The `..` syntax allows for either, both, or none of its arguments to be absent.</span></span> <span data-ttu-id="7cd69-111">无论参数的数量如何，`Range` 构造函数始终都足以使用 `Range` 语法。</span><span class="sxs-lookup"><span data-stu-id="7cd69-111">Regardless of the number of arguments, the `Range` constructor is always sufficient for using the `Range` syntax.</span></span> <span data-ttu-id="7cd69-112">但是，如果存在任何其他成员并且缺少一个或多个 `..` 参数，则相应的成员可能会被替换。</span><span class="sxs-lookup"><span data-stu-id="7cd69-112">However, if any of the other members are present and one or more of the `..` arguments are missing, the appropriate member may be substituted.</span></span>

<span data-ttu-id="7cd69-113">最后，对于要在数组元素访问表达式中使用 `System.Range` 类型的值，必须存在以下成员：</span><span class="sxs-lookup"><span data-stu-id="7cd69-113">Finally, for a value of type `System.Range` to be used in an array element access expression, the following member must be present:</span></span>

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeHelpers
    {
        public static T[] GetSubArray<T>(T[] array, System.Range range);
    }
}
```

### <a name="systemindex"></a><span data-ttu-id="7cd69-114">System.object</span><span class="sxs-lookup"><span data-stu-id="7cd69-114">System.Index</span></span>

<span data-ttu-id="7cd69-115">C#无法从末尾开始为集合编制索引，而是大多数索引器使用 "从开始" 概念，或执行 "长度-i" 表达式。</span><span class="sxs-lookup"><span data-stu-id="7cd69-115">C# has no way of indexing a collection from the end, but rather most indexers use the "from start" notion, or do a "length - i" expression.</span></span> <span data-ttu-id="7cd69-116">我们引入了一个表示 "从末尾" 的新索引表达式。</span><span class="sxs-lookup"><span data-stu-id="7cd69-116">We introduce a new Index expression that means "from the end".</span></span> <span data-ttu-id="7cd69-117">此功能将引入一个新的一元前缀 "hat" 运算符。</span><span class="sxs-lookup"><span data-stu-id="7cd69-117">The feature will introduce a new unary prefix "hat" operator.</span></span> <span data-ttu-id="7cd69-118">其单个操作数必须可转换为 `System.Int32`。</span><span class="sxs-lookup"><span data-stu-id="7cd69-118">Its single operand must be convertible to `System.Int32`.</span></span> <span data-ttu-id="7cd69-119">它将降低为适当的 `System.Index` 工厂方法调用。</span><span class="sxs-lookup"><span data-stu-id="7cd69-119">It will be lowered into the appropriate `System.Index` factory method call.</span></span>

<span data-ttu-id="7cd69-120">我们通过以下附加语法形式补充*unary_expression*的语法：</span><span class="sxs-lookup"><span data-stu-id="7cd69-120">We augment the grammar for *unary_expression* with the following additional syntax form:</span></span>

```antlr
unary_expression
    : '^' unary_expression
    ;
```

<span data-ttu-id="7cd69-121">我们*从 end*运算符将此索引称为索引。</span><span class="sxs-lookup"><span data-stu-id="7cd69-121">We call this the *index from end* operator.</span></span> <span data-ttu-id="7cd69-122">最终运算符*中*的预定义索引如下所示：</span><span class="sxs-lookup"><span data-stu-id="7cd69-122">The predefined *index from end* operators are as follows:</span></span>

```csharp
    System.Index operator ^(int fromEnd);
```

<span data-ttu-id="7cd69-123">仅为大于或等于零的输入值定义此运算符的行为。</span><span class="sxs-lookup"><span data-stu-id="7cd69-123">The behavior of this operator is only defined for input values greater than or equal to zero.</span></span>

<span data-ttu-id="7cd69-124">示例：</span><span class="sxs-lookup"><span data-stu-id="7cd69-124">Examples:</span></span>

```csharp
var thirdItem = list[2];                // list[2]
var lastItem = list[^1];                // list[Index.CreateFromEnd(1)]

var multiDimensional = list[3, ^2]      // list[3, Index.CreateFromEnd(2)]
```

#### <a name="systemrange"></a><span data-ttu-id="7cd69-125">System.object</span><span class="sxs-lookup"><span data-stu-id="7cd69-125">System.Range</span></span>

<span data-ttu-id="7cd69-126">C#没有语法方法来访问集合的 "范围" 或 "切片"。</span><span class="sxs-lookup"><span data-stu-id="7cd69-126">C# has no syntactic way to access "ranges" or "slices" of collections.</span></span> <span data-ttu-id="7cd69-127">通常，用户被迫实现复杂的结构来筛选/操作内存的切片，或利用 `list.Skip(5).Take(2)`等 LINQ 方法。</span><span class="sxs-lookup"><span data-stu-id="7cd69-127">Usually users are forced to implement complex structures to filter/operate on slices of memory, or resort to LINQ methods like `list.Skip(5).Take(2)`.</span></span> <span data-ttu-id="7cd69-128">添加了 `System.Span<T>` 和其他类似类型后，就可以在语言/运行时中的更深级别上支持这种操作，并使接口具有统一的。</span><span class="sxs-lookup"><span data-stu-id="7cd69-128">With the addition of `System.Span<T>` and other similar types, it becomes more important to have this kind of operation supported on a deeper level in the language/runtime, and have the interface unified.</span></span>

<span data-ttu-id="7cd69-129">语言将引入新的范围运算符 `x..y`。</span><span class="sxs-lookup"><span data-stu-id="7cd69-129">The language will introduce a new range operator `x..y`.</span></span> <span data-ttu-id="7cd69-130">它是一个接受两个表达式的二元中缀运算符。</span><span class="sxs-lookup"><span data-stu-id="7cd69-130">It is a binary infix operator that accepts two expressions.</span></span> <span data-ttu-id="7cd69-131">可以省略任一操作数（以下示例），并且它们必须可转换为 `System.Index`。</span><span class="sxs-lookup"><span data-stu-id="7cd69-131">Either operand can be omitted (examples below), and they have to be convertible to `System.Index`.</span></span> <span data-ttu-id="7cd69-132">它将降低为适当的 `System.Range` 工厂方法调用。</span><span class="sxs-lookup"><span data-stu-id="7cd69-132">It will be lowered to the appropriate `System.Range` factory method call.</span></span>

<span data-ttu-id="7cd69-133">我们将C# *multiplicative_expression*的语法规则替换为以下内容（以便引入新的优先级别）：</span><span class="sxs-lookup"><span data-stu-id="7cd69-133">We replace the C# grammar rules for *multiplicative_expression* with the following (in order to introduce a new precedence level):</span></span>

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

<span data-ttu-id="7cd69-134">所有形式的*range 运算符*都具有相同的优先级。</span><span class="sxs-lookup"><span data-stu-id="7cd69-134">All forms of the *range operator* have the same precedence.</span></span> <span data-ttu-id="7cd69-135">此新优先级组低于*一元运算符*，并且高于*mulitiplicative 算术运算符*。</span><span class="sxs-lookup"><span data-stu-id="7cd69-135">This new precedence group is lower than the *unary operators* and higher than the *mulitiplicative arithmetic operators*.</span></span>

<span data-ttu-id="7cd69-136">我们将 `..` 运算符称为*范围运算符*。</span><span class="sxs-lookup"><span data-stu-id="7cd69-136">We call the `..` operator the *range operator*.</span></span> <span data-ttu-id="7cd69-137">可以大致理解内置范围运算符，使其与此窗体的内置运算符的调用相对应：</span><span class="sxs-lookup"><span data-stu-id="7cd69-137">The built-in range operator can roughly be understood to correspond to the invocation of a built-in operator of this form:</span></span>

```csharp
    System.Range operator ..(Index start = 0, Index end = ^0);
```

<span data-ttu-id="7cd69-138">示例：</span><span class="sxs-lookup"><span data-stu-id="7cd69-138">Examples:</span></span>

```csharp
var slice1 = list[2..^3];               // list[Range.Create(2, Index.CreateFromEnd(3))]
var slice2 = list[..^3];                // list[Range.ToEnd(Index.CreateFromEnd(3))]
var slice3 = list[2..];                 // list[Range.FromStart(2)]
var slice4 = list[..];                  // list[Range.All]

var multiDimensional = list[1..2, ..]   // list[Range.Create(1, 2), Range.All]
```

<span data-ttu-id="7cd69-139">此外，`System.Index` 应从 `System.Int32`中进行隐式转换，以便不需要对混合使用多维签名的整数和索引进行重载。</span><span class="sxs-lookup"><span data-stu-id="7cd69-139">Moreover, `System.Index` should have an implicit conversion from `System.Int32`, in order not to need to overload for mixing integers and indexes over multi-dimensional signatures.</span></span>

## <a name="adding-index-and-range-support-to-existing-library-types"></a><span data-ttu-id="7cd69-140">向现有库类型添加索引和范围支持</span><span class="sxs-lookup"><span data-stu-id="7cd69-140">Adding Index and Range support to existing library types</span></span>

### <a name="implicit-index-support"></a><span data-ttu-id="7cd69-141">隐式索引支持</span><span class="sxs-lookup"><span data-stu-id="7cd69-141">Implicit Index support</span></span>

<span data-ttu-id="7cd69-142">该语言将为满足以下条件的类型提供一个实例索引器成员，其中包含一个类型为 `Index` 类型的参数：</span><span class="sxs-lookup"><span data-stu-id="7cd69-142">The language will provide an instance indexer member with a single parameter of type `Index` for types which meet the following criteria:</span></span>

- <span data-ttu-id="7cd69-143">类型为可数。</span><span class="sxs-lookup"><span data-stu-id="7cd69-143">The type is Countable.</span></span>
- <span data-ttu-id="7cd69-144">该类型具有可访问的实例索引器，该索引器使用单个 `int` 作为参数。</span><span class="sxs-lookup"><span data-stu-id="7cd69-144">The type has an accessible instance indexer which takes a single `int` as the argument.</span></span>
- <span data-ttu-id="7cd69-145">该类型没有可访问的实例索引器，该索引器将 `Index` 用作第一个参数。</span><span class="sxs-lookup"><span data-stu-id="7cd69-145">The type does not have an accessible instance indexer which takes an `Index` as the first parameter.</span></span> <span data-ttu-id="7cd69-146">`Index` 必须是唯一的参数，否则剩余的参数必须是可选的。</span><span class="sxs-lookup"><span data-stu-id="7cd69-146">The `Index` must be the only parameter or the remaining parameters must be optional.</span></span>

<span data-ttu-id="7cd69-147">如果某个类型具有一个名为 `Length` 的属性或具有可访问的 getter 的 `Count` 并且返回类型为 `int`，则该类型为***可数***。</span><span class="sxs-lookup"><span data-stu-id="7cd69-147">A type is ***Countable*** if it  has a property named `Length` or `Count` with an accessible getter and a return type of `int`.</span></span> <span data-ttu-id="7cd69-148">语言可以利用此属性将类型 `Index` 的表达式转换为表达式点 `int`，而无需全部使用该 `Index` 类型。</span><span class="sxs-lookup"><span data-stu-id="7cd69-148">The language can make use of this property to convert an expression of type `Index` into an `int` at the point of the expression without the need to use the type `Index` at all.</span></span> <span data-ttu-id="7cd69-149">如果同时存在 `Length` 和 `Count`，则需要 `Length`。</span><span class="sxs-lookup"><span data-stu-id="7cd69-149">In case both `Length` and `Count` are present, `Length` will be preferred.</span></span> <span data-ttu-id="7cd69-150">为方便起见，该建议将使用名称 `Length` 来表示 `Count` 或 `Length`。</span><span class="sxs-lookup"><span data-stu-id="7cd69-150">For simplicity going forward, the proposal will use the name `Length` to represent `Count` or `Length`.</span></span>

<span data-ttu-id="7cd69-151">对于此类类型，该语言将充当格式的索引器成员 `T this[Index index]` 其中，`T` 是基于 `int` 的索引器（包括任何 `ref` 样式批注）的返回类型。</span><span class="sxs-lookup"><span data-stu-id="7cd69-151">For such types, the language will act as if there is an indexer member of the form `T this[Index index]` where `T` is the return type of the `int` based indexer including any `ref` style annotations.</span></span> <span data-ttu-id="7cd69-152">新成员将具有与 `int` 索引器匹配的可访问性 `set` 成员 `get` 和。</span><span class="sxs-lookup"><span data-stu-id="7cd69-152">The new member will have the same `get` and `set` members with matching accessibility as the `int` indexer.</span></span> 

<span data-ttu-id="7cd69-153">通过将类型 `Index` 的参数转换为 `int` 并发出对基于 `int` 的索引器的调用来实现新的索引器。</span><span class="sxs-lookup"><span data-stu-id="7cd69-153">The new indexer will be implemented by converting the argument of type `Index` into an `int` and emitting a call to the `int` based indexer.</span></span> <span data-ttu-id="7cd69-154">出于讨论目的，我们使用 `receiver[expr]`的示例。</span><span class="sxs-lookup"><span data-stu-id="7cd69-154">For discussion purposes, let's use the example of `receiver[expr]`.</span></span> <span data-ttu-id="7cd69-155">将 `expr` 转换为 `int` 的方式如下：</span><span class="sxs-lookup"><span data-stu-id="7cd69-155">The conversion of `expr` to `int` will occur as follows:</span></span>

- <span data-ttu-id="7cd69-156">当参数的格式为 `^expr2` 并且 `expr2` 类型 `int`时，它将转换为 `receiver.Length - expr2`。</span><span class="sxs-lookup"><span data-stu-id="7cd69-156">When the argument is of the form `^expr2` and the type of `expr2` is `int`, it will be translated to `receiver.Length - expr2`.</span></span>
- <span data-ttu-id="7cd69-157">否则，它将转换为 `expr.GetOffset(receiver.Length)`。</span><span class="sxs-lookup"><span data-stu-id="7cd69-157">Otherwise, it will be translated as `expr.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="7cd69-158">这允许开发人员在现有类型上使用 `Index` 功能，而无需修改。</span><span class="sxs-lookup"><span data-stu-id="7cd69-158">This allows for developers to use the `Index` feature on existing types without the need for modification.</span></span> <span data-ttu-id="7cd69-159">例如：</span><span class="sxs-lookup"><span data-stu-id="7cd69-159">For example:</span></span>

``` csharp
List<char> list = ...;
var value = list[^1]; 

// Gets translated to 
var value = list[list.Count - 1]; 
```

<span data-ttu-id="7cd69-160">将对 `receiver` 和 `Length` 表达式进行适当的溢出，确保只执行一次任何副作用。</span><span class="sxs-lookup"><span data-stu-id="7cd69-160">The `receiver` and `Length` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="7cd69-161">例如：</span><span class="sxs-lookup"><span data-stu-id="7cd69-161">For example:</span></span>

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

<span data-ttu-id="7cd69-162">此代码将打印 "获取长度 3"。</span><span class="sxs-lookup"><span data-stu-id="7cd69-162">This code will print "Get Length 3".</span></span>

### <a name="implicit-range-support"></a><span data-ttu-id="7cd69-163">隐式范围支持</span><span class="sxs-lookup"><span data-stu-id="7cd69-163">Implicit Range support</span></span>

<span data-ttu-id="7cd69-164">该语言将为满足以下条件的类型提供一个实例索引器成员，其中包含一个类型为 `Range` 类型的参数：</span><span class="sxs-lookup"><span data-stu-id="7cd69-164">The language will provide an instance indexer member with a single parameter of type `Range` for types which meet the following criteria:</span></span>

- <span data-ttu-id="7cd69-165">类型为可数。</span><span class="sxs-lookup"><span data-stu-id="7cd69-165">The type is Countable.</span></span>
- <span data-ttu-id="7cd69-166">该类型具有一个名为 `Slice` 的可访问成员，它具有两个类型为 `int`的参数。</span><span class="sxs-lookup"><span data-stu-id="7cd69-166">The type has an accessible member named `Slice` which has two parameters of type `int`.</span></span>
- <span data-ttu-id="7cd69-167">该类型没有实例索引器，该索引器使用单个 `Range` 作为第一个参数。</span><span class="sxs-lookup"><span data-stu-id="7cd69-167">The type does not have an instance indexer which takes a single `Range` as the first parameter.</span></span> <span data-ttu-id="7cd69-168">`Range` 必须是唯一的参数，否则剩余的参数必须是可选的。</span><span class="sxs-lookup"><span data-stu-id="7cd69-168">The `Range` must be the only parameter or the remaining parameters must be optional.</span></span>

<span data-ttu-id="7cd69-169">对于这种类型的情况，语言将绑定为，因为 `T` 是窗体 `T this[Range range]` 的索引器成员，其中是 `Slice` 方法的返回类型，其中包括任何 `ref` 样式批注。</span><span class="sxs-lookup"><span data-stu-id="7cd69-169">For such types, the language will bind as if there is an indexer member of the form `T this[Range range]` where `T` is the return type of the `Slice` method including any `ref` style annotations.</span></span> <span data-ttu-id="7cd69-170">新成员还将具有与 `Slice`匹配的可访问性。</span><span class="sxs-lookup"><span data-stu-id="7cd69-170">The new member will also have matching accessibility with `Slice`.</span></span> 

<span data-ttu-id="7cd69-171">在名为 `receiver`的表达式上绑定基于 `Range` 的索引器时，会将 `Range` 表达式转换为两个值，然后将这些值传递给 `Slice` 方法，从而降低。</span><span class="sxs-lookup"><span data-stu-id="7cd69-171">When the `Range` based indexer is bound on an expression named `receiver`, it will be lowered by converting the `Range` expression into two values that are then passed to the `Slice` method.</span></span> <span data-ttu-id="7cd69-172">出于讨论目的，我们使用 `receiver[expr]`的示例。</span><span class="sxs-lookup"><span data-stu-id="7cd69-172">For discussion purposes, let's use the example of `receiver[expr]`.</span></span>

<span data-ttu-id="7cd69-173">将通过以下方式转换范围类型化表达式来获取 `Slice` 的第一个参数：</span><span class="sxs-lookup"><span data-stu-id="7cd69-173">The first argument of `Slice` will be obtained by converting the range typed expression in the following way:</span></span>

- <span data-ttu-id="7cd69-174">如果 `expr` 的格式为 `expr1..expr2` （其中可以省略 `expr2`），并且 `expr1` 的类型为 `int`，则会将其作为 `expr1`发出。</span><span class="sxs-lookup"><span data-stu-id="7cd69-174">When `expr` is of the form `expr1..expr2` (where `expr2` can be omitted) and `expr1` has type `int`, then it will be emitted as `expr1`.</span></span>
- <span data-ttu-id="7cd69-175">当 `expr` 格式为 `^expr1..expr2` （其中 `expr2` 可以省略）时，它将作为 `receiver.Length - expr1`发出。</span><span class="sxs-lookup"><span data-stu-id="7cd69-175">When `expr` is of the form `^expr1..expr2` (where `expr2` can be omitted), then it will be emitted as `receiver.Length - expr1`.</span></span>
- <span data-ttu-id="7cd69-176">当 `expr` 格式为 `..expr2` （其中 `expr2` 可以省略）时，它将作为 `0`发出。</span><span class="sxs-lookup"><span data-stu-id="7cd69-176">When `expr` is of the form `..expr2` (where `expr2` can be omitted), then it will be emitted as `0`.</span></span>
- <span data-ttu-id="7cd69-177">否则，它将作为 `expr.Start.GetOffset(receiver.Length)`发出。</span><span class="sxs-lookup"><span data-stu-id="7cd69-177">Otherwise, it will be emitted as `expr.Start.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="7cd69-178">此值将在第二个 `Slice` 参数的计算中重复使用。</span><span class="sxs-lookup"><span data-stu-id="7cd69-178">This value will be re-used in the calculation of the second `Slice` argument.</span></span> <span data-ttu-id="7cd69-179">执行此操作时，它将被称为 `start`。</span><span class="sxs-lookup"><span data-stu-id="7cd69-179">When doing so it will be referred to as `start`.</span></span> <span data-ttu-id="7cd69-180">将通过以下方式转换范围类型化表达式来获取 `Slice` 的第二个参数：</span><span class="sxs-lookup"><span data-stu-id="7cd69-180">The second argument of `Slice` will be obtained by converting the range typed expression in the following way:</span></span>

- <span data-ttu-id="7cd69-181">如果 `expr` 的格式为 `expr1..expr2` （其中可以省略 `expr1`），并且 `expr2` 的类型为 `int`，则会将其作为 `expr2 - start`发出。</span><span class="sxs-lookup"><span data-stu-id="7cd69-181">When `expr` is of the form `expr1..expr2` (where `expr1` can be omitted) and `expr2` has type `int`, then it will be emitted as `expr2 - start`.</span></span>
- <span data-ttu-id="7cd69-182">当 `expr` 格式为 `expr1..^expr2` （其中 `expr1` 可以省略）时，它将作为 `(receiver.Length - expr2) - start`发出。</span><span class="sxs-lookup"><span data-stu-id="7cd69-182">When `expr` is of the form `expr1..^expr2` (where `expr1` can be omitted), then it will be emitted as `(receiver.Length - expr2) - start`.</span></span>
- <span data-ttu-id="7cd69-183">当 `expr` 格式为 `expr1..` （其中 `expr1` 可以省略）时，它将作为 `receiver.Length - start`发出。</span><span class="sxs-lookup"><span data-stu-id="7cd69-183">When `expr` is of the form `expr1..` (where `expr1` can be omitted), then it will be emitted as `receiver.Length - start`.</span></span>
- <span data-ttu-id="7cd69-184">否则，它将作为 `expr.End.GetOffset(receiver.Length) - start`发出。</span><span class="sxs-lookup"><span data-stu-id="7cd69-184">Otherwise, it will be emitted as `expr.End.GetOffset(receiver.Length) - start`.</span></span>

<span data-ttu-id="7cd69-185">将对 `receiver`、`Length`和 `expr` 表达式进行适当的溢出，以确保只执行一次副作用。</span><span class="sxs-lookup"><span data-stu-id="7cd69-185">The `receiver`, `Length`, and `expr` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="7cd69-186">例如：</span><span class="sxs-lookup"><span data-stu-id="7cd69-186">For example:</span></span>

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

<span data-ttu-id="7cd69-187">此代码将打印 "获取长度 2"。</span><span class="sxs-lookup"><span data-stu-id="7cd69-187">This code will print "Get Length 2".</span></span>

<span data-ttu-id="7cd69-188">此语言将用以下已知类型作为特例：</span><span class="sxs-lookup"><span data-stu-id="7cd69-188">The language will special case the following known types:</span></span> 

- <span data-ttu-id="7cd69-189">`string`：将使用方法 `Substring`，而不是 `Slice`。</span><span class="sxs-lookup"><span data-stu-id="7cd69-189">`string`: the method `Substring` will be used instead of `Slice`.</span></span>
- <span data-ttu-id="7cd69-190">`array`：将使用方法 `System.Reflection.CompilerServices.GetSubArray`，而不是 `Slice`。</span><span class="sxs-lookup"><span data-stu-id="7cd69-190">`array`: the method `System.Reflection.CompilerServices.GetSubArray` will be used instead of `Slice`.</span></span>

## <a name="alternatives"></a><span data-ttu-id="7cd69-191">备选项</span><span class="sxs-lookup"><span data-stu-id="7cd69-191">Alternatives</span></span>

<span data-ttu-id="7cd69-192">新运算符（`^` 和 `..`）是语法糖。</span><span class="sxs-lookup"><span data-stu-id="7cd69-192">The new operators (`^` and `..`) are syntactic sugar.</span></span> <span data-ttu-id="7cd69-193">此功能可以通过显式调用来实现 `System.Index` 和 `System.Range` 工厂方法，但它会导致更多样板代码，并将 unintuitive 体验。</span><span class="sxs-lookup"><span data-stu-id="7cd69-193">The functionality can be implemented by explicit calls to `System.Index` and `System.Range` factory methods, but it will result in a lot more boilerplate code, and the experience will be unintuitive.</span></span>

## <a name="il-representation"></a><span data-ttu-id="7cd69-194">IL 表示形式</span><span class="sxs-lookup"><span data-stu-id="7cd69-194">IL Representation</span></span>

<span data-ttu-id="7cd69-195">这两个运算符将降低为常规索引器/方法调用，后续编译器层不会更改。</span><span class="sxs-lookup"><span data-stu-id="7cd69-195">These two operators will be lowered to regular indexer/method calls, with no change in subsequent compiler layers.</span></span>

## <a name="runtime-behavior"></a><span data-ttu-id="7cd69-196">运行时行为</span><span class="sxs-lookup"><span data-stu-id="7cd69-196">Runtime behavior</span></span>

- <span data-ttu-id="7cd69-197">编译器可以优化内置类型（如数组和字符串）的索引器，并将索引的索引减小到适当的现有方法。</span><span class="sxs-lookup"><span data-stu-id="7cd69-197">Compiler can optimize indexers for built-in types like arrays and strings, and lower the indexing to the appropriate existing methods.</span></span>
- <span data-ttu-id="7cd69-198">如果用负值构造，则会引发 `System.Index`。</span><span class="sxs-lookup"><span data-stu-id="7cd69-198">`System.Index` will throw if constructed with a negative value.</span></span>
- <span data-ttu-id="7cd69-199">`^0` 不会引发，但会转换为其提供的集合/可枚举的长度。</span><span class="sxs-lookup"><span data-stu-id="7cd69-199">`^0` does not throw, but it translates to the length of the collection/enumerable it is supplied to.</span></span>
- <span data-ttu-id="7cd69-200">`Range.All` 在语义上等效于 `0..^0`，可以析构这些索引。</span><span class="sxs-lookup"><span data-stu-id="7cd69-200">`Range.All` is semantically equivalent to `0..^0`, and can be deconstructed to these indices.</span></span>

## <a name="considerations"></a><span data-ttu-id="7cd69-201">注意事项</span><span class="sxs-lookup"><span data-stu-id="7cd69-201">Considerations</span></span>

### <a name="detect-indexable-based-on-icollection"></a><span data-ttu-id="7cd69-202">基于 ICollection 检测可索引</span><span class="sxs-lookup"><span data-stu-id="7cd69-202">Detect Indexable based on ICollection</span></span>

<span data-ttu-id="7cd69-203">此行为的灵感是集合初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="7cd69-203">The inspiration for this behavior was collection initializers.</span></span> <span data-ttu-id="7cd69-204">使用类型的结构来表明它已选择加入功能。</span><span class="sxs-lookup"><span data-stu-id="7cd69-204">Using the structure of a type to convey that it had opted into a feature.</span></span> <span data-ttu-id="7cd69-205">如果集合初始值设定项类型可通过实现接口 `IEnumerable` （非泛型）来选择功能。</span><span class="sxs-lookup"><span data-stu-id="7cd69-205">In the case of collection initializers types can opt into the feature by implementing the interface `IEnumerable` (non generic).</span></span>

<span data-ttu-id="7cd69-206">本建议最初要求类型实现 `ICollection`，才能将其限定为可索引。</span><span class="sxs-lookup"><span data-stu-id="7cd69-206">This proposal initially required that types implement `ICollection` in order to qualify as Indexable.</span></span> <span data-ttu-id="7cd69-207">但这需要一些特殊情况：</span><span class="sxs-lookup"><span data-stu-id="7cd69-207">That required a number of special cases though:</span></span>

- <span data-ttu-id="7cd69-208">`ref struct`：这些类无法实现接口，如 `Span<T>` 是索引/范围支持的理想选择。</span><span class="sxs-lookup"><span data-stu-id="7cd69-208">`ref struct`: these cannot implement interfaces yet types like `Span<T>` are ideal for index / range support.</span></span> 
- <span data-ttu-id="7cd69-209">`string`：不实现 `ICollection` 并添加 `interface` 的成本较高。</span><span class="sxs-lookup"><span data-stu-id="7cd69-209">`string`: does not implement `ICollection` and adding that `interface` has a large cost.</span></span>

<span data-ttu-id="7cd69-210">这意味着支持已需要的密钥类型特殊大小写。</span><span class="sxs-lookup"><span data-stu-id="7cd69-210">This means to support key types special casing is already needed.</span></span> <span data-ttu-id="7cd69-211">`string` 的特殊大小写不太感兴趣，因为语言在其他区域（`foreach` 降低、常量等）中执行此功能。`ref struct` 的特殊大小写更多，因为它是整个类型类的特殊大小写。</span><span class="sxs-lookup"><span data-stu-id="7cd69-211">The special casing of `string` is less interesting as the language does this in other areas (`foreach` lowering, constants, etc ...). The special casing of `ref struct` is more concerning as it's special casing an entire class of types.</span></span> <span data-ttu-id="7cd69-212">如果它们只具有一个名为 `Count` 的属性，且返回类型为 `int`，则它们将被标记为可索引。</span><span class="sxs-lookup"><span data-stu-id="7cd69-212">They get labeled as Indexable if they simply have a property named `Count` with a return type of `int`.</span></span> 

<span data-ttu-id="7cd69-213">考虑到该设计已规范化为，表示具有属性 `Count` / `Length` 具有返回类型 `int` 的任何类型都可以编制索引。</span><span class="sxs-lookup"><span data-stu-id="7cd69-213">After consideration the design was normalized to say that any type which has a property `Count` / `Length` with a return type of `int` is Indexable.</span></span> <span data-ttu-id="7cd69-214">这将删除所有特殊大小写，即使对于 `string` 和数组也是如此。</span><span class="sxs-lookup"><span data-stu-id="7cd69-214">That removes all special casing, even for `string` and arrays.</span></span>

### <a name="detect-just-count"></a><span data-ttu-id="7cd69-215">仅检测计数</span><span class="sxs-lookup"><span data-stu-id="7cd69-215">Detect just Count</span></span>

<span data-ttu-id="7cd69-216">检测属性名称 `Count` 或 `Length` 确实会使设计复杂化。</span><span class="sxs-lookup"><span data-stu-id="7cd69-216">Detecting on the property names `Count` or `Length` does complicate the design a bit.</span></span> <span data-ttu-id="7cd69-217">只需选择一项进行标准化即可实现标准化，因为它最终会排除大量类型：</span><span class="sxs-lookup"><span data-stu-id="7cd69-217">Picking just one to standardize though is not sufficient as it ends up excluding a large number of types:</span></span>

- <span data-ttu-id="7cd69-218">使用 `Length`：排除 System.web 和子命名空间中的每个集合。</span><span class="sxs-lookup"><span data-stu-id="7cd69-218">Use `Length`: excludes pretty much every collection in System.Collections and sub-namespaces.</span></span> <span data-ttu-id="7cd69-219">它们往往派生自 `ICollection`，因此更喜欢 `Count` 长度。</span><span class="sxs-lookup"><span data-stu-id="7cd69-219">Those tend to derive from `ICollection` and hence prefer `Count` over length.</span></span>
- <span data-ttu-id="7cd69-220">使用 `Count`：排除 `string`、数组、`Span<T>` 和基于 `ref struct` 的大多数类型</span><span class="sxs-lookup"><span data-stu-id="7cd69-220">Use `Count`: excludes `string`, arrays, `Span<T>` and most `ref struct` based types</span></span>

<span data-ttu-id="7cd69-221">在其他方面，对可编制索引的类型的初始检测的额外问题在有点简化。</span><span class="sxs-lookup"><span data-stu-id="7cd69-221">The extra complication on the initial detection of Indexable types is outweighed by its simplification in other aspects.</span></span>

### <a name="choice-of-slice-as-a-name"></a><span data-ttu-id="7cd69-222">将切片选为名称</span><span class="sxs-lookup"><span data-stu-id="7cd69-222">Choice of Slice as a name</span></span>

<span data-ttu-id="7cd69-223">选择名称 `Slice` 因为它是 .NET 中切片样式操作的实际标准名称。</span><span class="sxs-lookup"><span data-stu-id="7cd69-223">The name `Slice` was chosen as it's the de-facto standard name for slice style operations in .NET.</span></span> <span data-ttu-id="7cd69-224">从 netcoreapp 2.1 开始，所有范围样式类型使用切片操作的名称 `Slice`。</span><span class="sxs-lookup"><span data-stu-id="7cd69-224">Starting with netcoreapp2.1 all span style types use the name `Slice` for slicing operations.</span></span> <span data-ttu-id="7cd69-225">在 netcoreapp 2.1 之前，没有任何关于示例的切片的示例。</span><span class="sxs-lookup"><span data-stu-id="7cd69-225">Prior to netcoreapp2.1 there really aren't any examples of slicing to look to for an example.</span></span> <span data-ttu-id="7cd69-226">`List<T>`、`ArraySegment<T>`、`SortedList<T>` 等类型对切片而言非常理想，但添加类型时该概念并不存在。</span><span class="sxs-lookup"><span data-stu-id="7cd69-226">Types like `List<T>`, `ArraySegment<T>`, `SortedList<T>` would've been ideal for slicing but the concept didn't exist when types were added.</span></span> 

<span data-ttu-id="7cd69-227">因此，`Slice` 是唯一的示例，则将其选择为名称。</span><span class="sxs-lookup"><span data-stu-id="7cd69-227">Thus, `Slice` being the sole example, it was chosen as the name.</span></span>

### <a name="index-target-type-conversion"></a><span data-ttu-id="7cd69-228">索引目标类型转换</span><span class="sxs-lookup"><span data-stu-id="7cd69-228">Index target type conversion</span></span>

<span data-ttu-id="7cd69-229">在索引器表达式中查看 `Index` 转换的另一种方法是作为目标类型转换。</span><span class="sxs-lookup"><span data-stu-id="7cd69-229">Another way to view the `Index` transformation in an indexer expression is as a target type conversion.</span></span> <span data-ttu-id="7cd69-230">此语言不会像 `return_type this[Index]`形式的成员那样绑定，而是将目标类型化转换分配给 `int`。</span><span class="sxs-lookup"><span data-stu-id="7cd69-230">Instead of binding as if there is a member of the form `return_type this[Index]`, the language instead assigns a target typed conversion to `int`.</span></span> 

<span data-ttu-id="7cd69-231">此概念可以通用化到对可数类型的所有成员访问。</span><span class="sxs-lookup"><span data-stu-id="7cd69-231">This concept could be generalized to all member access on Countable types.</span></span> <span data-ttu-id="7cd69-232">只要将类型 `Index` 的表达式用作实例成员调用的参数，且接收方可数，则表达式将具有到 `int`的目标类型转换。</span><span class="sxs-lookup"><span data-stu-id="7cd69-232">Whenever an expression with type `Index` is used as an argument to an instance member invocation and the receiver is Countable then the expression will have a target type conversion to `int`.</span></span> <span data-ttu-id="7cd69-233">适用于此转换的成员调用包括方法、索引器、属性、扩展方法等。仅排除构造函数，因为它们没有接收方。</span><span class="sxs-lookup"><span data-stu-id="7cd69-233">The member invocations applicable for this conversion include methods, indexers, properties, extension methods, etc ... Only constructors are excluded as they have no receiver.</span></span> 

<span data-ttu-id="7cd69-234">对于具有一种类型的 `Index`的任何表达式，将按如下所示实现目标类型转换。</span><span class="sxs-lookup"><span data-stu-id="7cd69-234">The target type conversion will be implemented as follows for any expression which has a type of `Index`.</span></span> <span data-ttu-id="7cd69-235">出于讨论目的，可以使用 `receiver[expr]`的示例：</span><span class="sxs-lookup"><span data-stu-id="7cd69-235">For discussion purposes lets use the example of `receiver[expr]`:</span></span>

- <span data-ttu-id="7cd69-236">当 `expr` 的形式为 `^expr2` 并且 `expr2` 类型 `int`时，它将转换为 `receiver.Length - expr2`。</span><span class="sxs-lookup"><span data-stu-id="7cd69-236">When `expr` is of the form `^expr2` and the type of `expr2` is `int`, it will be translated to `receiver.Length - expr2`.</span></span>
- <span data-ttu-id="7cd69-237">否则，它将转换为 `expr.GetOffset(receiver.Length)`。</span><span class="sxs-lookup"><span data-stu-id="7cd69-237">Otherwise, it will be translated as `expr.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="7cd69-238">将对 `receiver` 和 `Length` 表达式进行适当的溢出，确保只执行一次任何副作用。</span><span class="sxs-lookup"><span data-stu-id="7cd69-238">The `receiver` and `Length` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="7cd69-239">例如：</span><span class="sxs-lookup"><span data-stu-id="7cd69-239">For example:</span></span>

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

<span data-ttu-id="7cd69-240">此代码将打印 "获取长度 3"。</span><span class="sxs-lookup"><span data-stu-id="7cd69-240">This code will print "Get Length 3".</span></span> 

<span data-ttu-id="7cd69-241">对于具有表示索引的参数的任何成员，此功能将非常有用。</span><span class="sxs-lookup"><span data-stu-id="7cd69-241">This feature would be beneficial to any member which had a parameter that represented an index.</span></span> <span data-ttu-id="7cd69-242">例如，`List<T>.InsertAt`。</span><span class="sxs-lookup"><span data-stu-id="7cd69-242">For example `List<T>.InsertAt`.</span></span> <span data-ttu-id="7cd69-243">这也可能会造成混淆，因为该语言不能提供任何有关表达式是否用于索引的指导。</span><span class="sxs-lookup"><span data-stu-id="7cd69-243">This also has the potential for confusion as the language can't give any guidance as to whether or not an expression is meant for indexing.</span></span> <span data-ttu-id="7cd69-244">在调用可数类型的成员时，它可以将任何 `Index` 表达式转换为 `int`。</span><span class="sxs-lookup"><span data-stu-id="7cd69-244">All it can do is convert any `Index` expression to `int` when invoking a member on a Countable type.</span></span> 

<span data-ttu-id="7cd69-245">限制：</span><span class="sxs-lookup"><span data-stu-id="7cd69-245">Restrictions:</span></span>

- <span data-ttu-id="7cd69-246">仅当类型 `Index` 的表达式直接是成员的参数时，此转换才适用。</span><span class="sxs-lookup"><span data-stu-id="7cd69-246">This conversion is only applicable when the expression with type `Index` is directly an argument to the member.</span></span> <span data-ttu-id="7cd69-247">它不适用于任何嵌套表达式。</span><span class="sxs-lookup"><span data-stu-id="7cd69-247">It would not apply to any nested expressions.</span></span>

## <a name="decisions-made-during-implementation"></a><span data-ttu-id="7cd69-248">在实现过程中做出的决策</span><span class="sxs-lookup"><span data-stu-id="7cd69-248">Decisions made during implementation</span></span>

- <span data-ttu-id="7cd69-249">模式中的所有成员都必须是实例成员</span><span class="sxs-lookup"><span data-stu-id="7cd69-249">All members in the pattern must be instance members</span></span>
- <span data-ttu-id="7cd69-250">如果找到长度方法但其返回类型错误，则继续查找计数</span><span class="sxs-lookup"><span data-stu-id="7cd69-250">If a Length method is found but it has the wrong return type, continue looking for Count</span></span>
- <span data-ttu-id="7cd69-251">用于索引模式的索引器必须正好有一个 int 参数</span><span class="sxs-lookup"><span data-stu-id="7cd69-251">The indexer used for the Index pattern must have exactly one int parameter</span></span>
- <span data-ttu-id="7cd69-252">用于范围模式的切片方法必须正好有两个 int 参数</span><span class="sxs-lookup"><span data-stu-id="7cd69-252">The Slice method used for the Range pattern must have exactly two int parameters</span></span>
- <span data-ttu-id="7cd69-253">查找模式成员时，查找原始定义，而不是构造成员</span><span class="sxs-lookup"><span data-stu-id="7cd69-253">When looking for the pattern members, we look for original definitions, not constructed members</span></span>

## <a name="design-meetings"></a><span data-ttu-id="7cd69-254">设计会议</span><span class="sxs-lookup"><span data-stu-id="7cd69-254">Design Meetings</span></span>

- https://github.com/dotnet/csharplang/blob/master/meetings/2019/LDM-2019-04-01.md

## <a name="questions"></a><span data-ttu-id="7cd69-255">问题</span><span class="sxs-lookup"><span data-stu-id="7cd69-255">Questions</span></span>
