---
ms.openlocfilehash: 07b4afe4a3fcbf10c978f05e642dfd8a47d53ea5
ms.sourcegitcommit: 194a043db72b9244f8db45db326cc82de6cec965
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "80217198"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="e1dd2-101">目标类型 `new` 表达式</span><span class="sxs-lookup"><span data-stu-id="e1dd2-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="e1dd2-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="e1dd2-102">[x] Proposed</span></span>
* <span data-ttu-id="e1dd2-103">[x][原型](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="e1dd2-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="e1dd2-104">[] 实现</span><span class="sxs-lookup"><span data-stu-id="e1dd2-104">[ ] Implementation</span></span>
* <span data-ttu-id="e1dd2-105">[] 规范</span><span class="sxs-lookup"><span data-stu-id="e1dd2-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="e1dd2-106">总结</span><span class="sxs-lookup"><span data-stu-id="e1dd2-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="e1dd2-107">当类型已知时，不需要构造函数的类型规范。</span><span class="sxs-lookup"><span data-stu-id="e1dd2-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="e1dd2-108">动机</span><span class="sxs-lookup"><span data-stu-id="e1dd2-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="e1dd2-109">允许字段初始化，而不复制类型。</span><span class="sxs-lookup"><span data-stu-id="e1dd2-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

<span data-ttu-id="e1dd2-110">如果可从用法推断，则允许省略类型。</span><span class="sxs-lookup"><span data-stu-id="e1dd2-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

<span data-ttu-id="e1dd2-111">实例化对象，而不会对类型进行拼写检查。</span><span class="sxs-lookup"><span data-stu-id="e1dd2-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```

## <a name="detailed-design"></a><span data-ttu-id="e1dd2-112">详细设计</span><span class="sxs-lookup"><span data-stu-id="e1dd2-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="e1dd2-113">在出现括号时，将修改*object_creation_expression*语法，使*类型*成为可选的。</span><span class="sxs-lookup"><span data-stu-id="e1dd2-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="e1dd2-114">这是解决*anonymous_object_creation_expression*的多义性所必需的。</span><span class="sxs-lookup"><span data-stu-id="e1dd2-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```

<span data-ttu-id="e1dd2-115">目标类型 `new` 可转换为任何类型。</span><span class="sxs-lookup"><span data-stu-id="e1dd2-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="e1dd2-116">因此，它不会导致重载决策。</span><span class="sxs-lookup"><span data-stu-id="e1dd2-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="e1dd2-117">这主要是为了避免意外的重大更改。</span><span class="sxs-lookup"><span data-stu-id="e1dd2-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="e1dd2-118">参数列表和初始值设定项表达式将在确定类型后进行绑定。</span><span class="sxs-lookup"><span data-stu-id="e1dd2-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="e1dd2-119">表达式的类型将从目标类型推断而来，这需要是以下类型之一：</span><span class="sxs-lookup"><span data-stu-id="e1dd2-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="e1dd2-120">**任何结构类型**（包括元组类型）</span><span class="sxs-lookup"><span data-stu-id="e1dd2-120">**Any struct type** (including tuple types)</span></span>
- <span data-ttu-id="e1dd2-121">**任何引用类型**（包括委托类型）</span><span class="sxs-lookup"><span data-stu-id="e1dd2-121">**Any reference type** (including delegate types)</span></span>
- <span data-ttu-id="e1dd2-122">具有构造函数或 `struct` 约束的**任何类型参数**</span><span class="sxs-lookup"><span data-stu-id="e1dd2-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="e1dd2-123">但有以下例外：</span><span class="sxs-lookup"><span data-stu-id="e1dd2-123">with the following exceptions:</span></span>

- <span data-ttu-id="e1dd2-124">**枚举类型：** 并非所有枚举类型都包含常量零，因此应使用显式枚举成员。</span><span class="sxs-lookup"><span data-stu-id="e1dd2-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="e1dd2-125">**接口类型：** 这是一项小型功能，应更好地显式提及该类型。</span><span class="sxs-lookup"><span data-stu-id="e1dd2-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="e1dd2-126">**数组类型：** 数组需要一个特殊的语法来提供长度。</span><span class="sxs-lookup"><span data-stu-id="e1dd2-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="e1dd2-127">**动态：** 我们不允许 `new dynamic()`，因此不允许 `new()` `dynamic` 作为目标类型。</span><span class="sxs-lookup"><span data-stu-id="e1dd2-127">**dynamic:** we don't allow `new dynamic()`, so we don't allow `new()` with `dynamic` as a target type.</span></span>

<span data-ttu-id="e1dd2-128">还会排除*object_creation_expression*中不允许的所有其他类型，例如，指针类型。</span><span class="sxs-lookup"><span data-stu-id="e1dd2-128">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

<span data-ttu-id="e1dd2-129">如果目标类型是可以为 null 的值类型，则目标类型 `new` 将转换为基础类型而不是可以为 null 的类型。</span><span class="sxs-lookup"><span data-stu-id="e1dd2-129">When the target type is a nullable value type, the target-typed `new` will be converted to the underlying type instead of the nullable type.</span></span>

> <span data-ttu-id="e1dd2-130">**打开问题：** 我们是否应允许委托和元组作为目标类型？</span><span class="sxs-lookup"><span data-stu-id="e1dd2-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="e1dd2-131">以上规则包括委托（引用类型）和元组（结构类型）。</span><span class="sxs-lookup"><span data-stu-id="e1dd2-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="e1dd2-132">尽管这两种类型都是可构造，但如果类型为 inferable，则可以使用匿名函数或元组文本。</span><span class="sxs-lookup"><span data-stu-id="e1dd2-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a><span data-ttu-id="e1dd2-133">杂项</span><span class="sxs-lookup"><span data-stu-id="e1dd2-133">Miscellaneous</span></span>

<span data-ttu-id="e1dd2-134">不允许 `throw new()`。</span><span class="sxs-lookup"><span data-stu-id="e1dd2-134">`throw new()` is disallowed.</span></span>

<span data-ttu-id="e1dd2-135">不允许将目标类型 `new` 与二元运算符一起使用。</span><span class="sxs-lookup"><span data-stu-id="e1dd2-135">Target-typed `new` is not allowed with binary operators.</span></span>

<span data-ttu-id="e1dd2-136">当没有目标类型时，不允许使用该方法：一元运算符、`foreach`的集合在 `using`中，在析构中，在 `await` 表达式中作为匿名类型属性（`new { Prop = new() }`），在 `lock` 语句中，在 `sizeof`中，在 `fixed` 语句中，在成员访问（`new().field`）中，在 LINQ 查询中，作为`someDynamic.Method(new())`运算符的操作数，作为 `is` 运算符的左操作数。,  ...`??`</span><span class="sxs-lookup"><span data-stu-id="e1dd2-136">It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...</span></span>

<span data-ttu-id="e1dd2-137">它也不允许作为 `ref`。</span><span class="sxs-lookup"><span data-stu-id="e1dd2-137">It is also disallowed as a `ref`.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="e1dd2-138">缺点</span><span class="sxs-lookup"><span data-stu-id="e1dd2-138">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="e1dd2-139">在创建新类别的重大更改时，目标类型 `new` 存在一些问题，`default``null` 但这并不是一个重要问题。</span><span class="sxs-lookup"><span data-stu-id="e1dd2-139">There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.</span></span>

## <a name="alternatives"></a><span data-ttu-id="e1dd2-140">备选项</span><span class="sxs-lookup"><span data-stu-id="e1dd2-140">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="e1dd2-141">大多数关于字段初始化中太长时间的投诉都是关于类型*参数*，而不是类型本身，我们只能推断 `new Dictionary(...)` （或类似）类型参数，并在本地从参数或集合初始值设定项推断类型参数。</span><span class="sxs-lookup"><span data-stu-id="e1dd2-141">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="e1dd2-142">问题</span><span class="sxs-lookup"><span data-stu-id="e1dd2-142">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="e1dd2-143">是否应在表达式树中禁止使用情况？</span><span class="sxs-lookup"><span data-stu-id="e1dd2-143">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="e1dd2-144">不</span><span class="sxs-lookup"><span data-stu-id="e1dd2-144">(no)</span></span>
- <span data-ttu-id="e1dd2-145">此功能如何与 `dynamic` 参数交互？</span><span class="sxs-lookup"><span data-stu-id="e1dd2-145">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="e1dd2-146">（无特殊处理）</span><span class="sxs-lookup"><span data-stu-id="e1dd2-146">(no special treatment)</span></span>
- <span data-ttu-id="e1dd2-147">IntelliSense 应如何处理 `new()`？</span><span class="sxs-lookup"><span data-stu-id="e1dd2-147">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="e1dd2-148">（仅当存在单个目标类型时）</span><span class="sxs-lookup"><span data-stu-id="e1dd2-148">(only when there is a single target-type)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="e1dd2-149">设计会议</span><span class="sxs-lookup"><span data-stu-id="e1dd2-149">Design meetings</span></span>

- [<span data-ttu-id="e1dd2-150">LDM-2017-10-18</span><span class="sxs-lookup"><span data-stu-id="e1dd2-150">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [<span data-ttu-id="e1dd2-151">LDM-2018-05-21</span><span class="sxs-lookup"><span data-stu-id="e1dd2-151">LDM-2018-05-21</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [<span data-ttu-id="e1dd2-152">LDM-2018-06-25</span><span class="sxs-lookup"><span data-stu-id="e1dd2-152">LDM-2018-06-25</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [<span data-ttu-id="e1dd2-153">LDM-2018-08-22</span><span class="sxs-lookup"><span data-stu-id="e1dd2-153">LDM-2018-08-22</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [<span data-ttu-id="e1dd2-154">LDM-2018-10-17</span><span class="sxs-lookup"><span data-stu-id="e1dd2-154">LDM-2018-10-17</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
