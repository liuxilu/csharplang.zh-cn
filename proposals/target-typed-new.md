---
ms.openlocfilehash: 4e2a536bab00859b003e8d967cb1927a99a9fa21
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483528"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="3b7a7-101">目标类型 `new` 表达式</span><span class="sxs-lookup"><span data-stu-id="3b7a7-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="3b7a7-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="3b7a7-102">[x] Proposed</span></span>
* <span data-ttu-id="3b7a7-103">[x][原型](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="3b7a7-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="3b7a7-104">[] 实现</span><span class="sxs-lookup"><span data-stu-id="3b7a7-104">[ ] Implementation</span></span>
* <span data-ttu-id="3b7a7-105">[] 规范</span><span class="sxs-lookup"><span data-stu-id="3b7a7-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="3b7a7-106">总结</span><span class="sxs-lookup"><span data-stu-id="3b7a7-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="3b7a7-107">当类型已知时，不需要构造函数的类型规范。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="3b7a7-108">动机</span><span class="sxs-lookup"><span data-stu-id="3b7a7-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="3b7a7-109">允许字段初始化，而不复制类型。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```
<span data-ttu-id="3b7a7-110">如果可从用法推断，则允许省略类型。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```
<span data-ttu-id="3b7a7-111">实例化对象，而不会对类型进行拼写检查。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```
## <a name="detailed-design"></a><span data-ttu-id="3b7a7-112">详细设计</span><span class="sxs-lookup"><span data-stu-id="3b7a7-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="3b7a7-113">在出现括号时，将修改*object_creation_expression*语法，使*类型*成为可选的。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="3b7a7-114">这是解决*anonymous_object_creation_expression*的多义性所必需的。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```
<span data-ttu-id="3b7a7-115">目标类型 `new` 可转换为任何类型。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="3b7a7-116">因此，它不会导致重载决策。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="3b7a7-117">这主要是为了避免意外的重大更改。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="3b7a7-118">参数列表和初始值设定项表达式将在确定类型后进行绑定。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="3b7a7-119">表达式的类型将从目标类型推断而来，这需要是以下类型之一：</span><span class="sxs-lookup"><span data-stu-id="3b7a7-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="3b7a7-120">**任何结构类型**</span><span class="sxs-lookup"><span data-stu-id="3b7a7-120">**Any struct type**</span></span>
- <span data-ttu-id="3b7a7-121">**任何引用类型**</span><span class="sxs-lookup"><span data-stu-id="3b7a7-121">**Any reference type**</span></span>
- <span data-ttu-id="3b7a7-122">具有构造函数或 `struct` 约束的**任何类型参数**</span><span class="sxs-lookup"><span data-stu-id="3b7a7-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="3b7a7-123">但有以下例外：</span><span class="sxs-lookup"><span data-stu-id="3b7a7-123">with the following exceptions:</span></span>

- <span data-ttu-id="3b7a7-124">**枚举类型：** 并非所有枚举类型都包含常量零，因此应使用显式枚举成员。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="3b7a7-125">**接口类型：** 这是一项小型功能，应更好地显式提及该类型。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="3b7a7-126">**数组类型：** 数组需要一个特殊的语法来提供长度。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="3b7a7-127">**Struct 默认构造函数**：这会排除所有基元类型和大多数值类型。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-127">**Struct default constructor**: this rules out all primitive types and most value types.</span></span> <span data-ttu-id="3b7a7-128">如果要使用此类类型的默认值，可以改为写入 `default`。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-128">If you wanted to use the default value of such types you could write `default` instead.</span></span>

<span data-ttu-id="3b7a7-129">还会排除*object_creation_expression*中不允许的所有其他类型，例如，指针类型。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-129">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

> <span data-ttu-id="3b7a7-130">**打开问题：** 我们是否应允许委托和元组作为目标类型？</span><span class="sxs-lookup"><span data-stu-id="3b7a7-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="3b7a7-131">以上规则包括委托（引用类型）和元组（结构类型）。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="3b7a7-132">尽管这两种类型都是可构造，但如果类型为 inferable，则可以使用匿名函数或元组文本。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

var x = new() == (1, 2); // ruled out by "use of struct default constructor"
var x = new(1, 2) == (1, 2) // "new" is redundant
```


> <span data-ttu-id="3b7a7-133">**打开问题：** 我们是否应允许 `Exception` 作为目标类型的 `throw new()`？</span><span class="sxs-lookup"><span data-stu-id="3b7a7-133">**Open Issue:** should we allow `throw new()` with `Exception` as the target-type?</span></span>

<span data-ttu-id="3b7a7-134">目前 `throw null`，但不 `throw default` （尽管它具有相同的效果）。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-134">We have `throw null` today, but not `throw default` (though it would have the same effect).</span></span> <span data-ttu-id="3b7a7-135">另一方面，`throw new()` 可以用作 `throw new Exception(...)`的简写。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-135">On the other hand, `throw new()` could be actually useful as a shorthand for `throw new Exception(...)`.</span></span> <span data-ttu-id="3b7a7-136">请注意，当前规范已允许使用此方法。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-136">Note that it is already allowed by the current specification.</span></span> <span data-ttu-id="3b7a7-137">`Exception` 是引用类型，并且 throw 语句的规范表明表达式已转换为 `Exception`。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-137">`Exception` is a reference type, and the specification for the throw statement says that the expression is converted to `Exception`.</span></span>

> <span data-ttu-id="3b7a7-138">**打开问题：** 我们是否应该允许使用用户定义的比较和算术运算符的目标类型 `new`？</span><span class="sxs-lookup"><span data-stu-id="3b7a7-138">**Open Issue:** should we allow usages of a target-typed `new` with user-defined comparison and arithmetic operators?</span></span>

<span data-ttu-id="3b7a7-139">为了进行比较，`default` 仅支持相等（用户定义的和内置的）运算符。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-139">For comparison, `default` only supports equality (user-defined and built-in) operators.</span></span> <span data-ttu-id="3b7a7-140">还需要支持 `new()` 的其他运算符吗？</span><span class="sxs-lookup"><span data-stu-id="3b7a7-140">Would it make sense to support other operators for `new()` as well?</span></span>

## <a name="drawbacks"></a><span data-ttu-id="3b7a7-141">缺点</span><span class="sxs-lookup"><span data-stu-id="3b7a7-141">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="3b7a7-142">无。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-142">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="3b7a7-143">备选项</span><span class="sxs-lookup"><span data-stu-id="3b7a7-143">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="3b7a7-144">大多数关于字段初始化中太长时间的投诉都是关于类型*参数*，而不是类型本身，我们只能推断 `new Dictionary(...)` （或类似）类型参数，并在本地从参数或集合初始值设定项推断类型参数。</span><span class="sxs-lookup"><span data-stu-id="3b7a7-144">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="3b7a7-145">问题</span><span class="sxs-lookup"><span data-stu-id="3b7a7-145">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="3b7a7-146">是否应在表达式树中禁止使用情况？</span><span class="sxs-lookup"><span data-stu-id="3b7a7-146">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="3b7a7-147">不</span><span class="sxs-lookup"><span data-stu-id="3b7a7-147">(no)</span></span>
- <span data-ttu-id="3b7a7-148">此功能如何与 `dynamic` 参数交互？</span><span class="sxs-lookup"><span data-stu-id="3b7a7-148">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="3b7a7-149">（无特殊处理）</span><span class="sxs-lookup"><span data-stu-id="3b7a7-149">(no special treatment)</span></span>
- <span data-ttu-id="3b7a7-150">IntelliSense 应如何处理 `new()`？</span><span class="sxs-lookup"><span data-stu-id="3b7a7-150">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="3b7a7-151">（仅当存在单个目标类型时）</span><span class="sxs-lookup"><span data-stu-id="3b7a7-151">(only when there is a single target-type)</span></span>
## <a name="design-meetings"></a><span data-ttu-id="3b7a7-152">设计会议</span><span class="sxs-lookup"><span data-stu-id="3b7a7-152">Design meetings</span></span>

- [<span data-ttu-id="3b7a7-153">LDM-2017-10-18</span><span class="sxs-lookup"><span data-stu-id="3b7a7-153">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
