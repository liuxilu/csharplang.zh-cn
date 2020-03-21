---
ms.openlocfilehash: 38740069a2e105f920fa275c443f4560055e2901
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108360"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="81952-101">目标类型 `new` 表达式</span><span class="sxs-lookup"><span data-stu-id="81952-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="81952-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="81952-102">[x] Proposed</span></span>
* <span data-ttu-id="81952-103">[x][原型](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="81952-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="81952-104">[] 实现</span><span class="sxs-lookup"><span data-stu-id="81952-104">[ ] Implementation</span></span>
* <span data-ttu-id="81952-105">[] 规范</span><span class="sxs-lookup"><span data-stu-id="81952-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="81952-106">总结</span><span class="sxs-lookup"><span data-stu-id="81952-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="81952-107">当类型已知时，不需要构造函数的类型规范。</span><span class="sxs-lookup"><span data-stu-id="81952-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="81952-108">动机</span><span class="sxs-lookup"><span data-stu-id="81952-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="81952-109">允许字段初始化，而不复制类型。</span><span class="sxs-lookup"><span data-stu-id="81952-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

<span data-ttu-id="81952-110">如果可从用法推断，则允许省略类型。</span><span class="sxs-lookup"><span data-stu-id="81952-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

<span data-ttu-id="81952-111">实例化对象，而不会对类型进行拼写检查。</span><span class="sxs-lookup"><span data-stu-id="81952-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```

## <a name="detailed-design"></a><span data-ttu-id="81952-112">详细设计</span><span class="sxs-lookup"><span data-stu-id="81952-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="81952-113">在出现括号时，将修改*object_creation_expression*语法，使*类型*成为可选的。</span><span class="sxs-lookup"><span data-stu-id="81952-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="81952-114">这是解决*anonymous_object_creation_expression*的多义性所必需的。</span><span class="sxs-lookup"><span data-stu-id="81952-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```

<span data-ttu-id="81952-115">目标类型 `new` 可转换为任何类型。</span><span class="sxs-lookup"><span data-stu-id="81952-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="81952-116">因此，它不会导致重载决策。</span><span class="sxs-lookup"><span data-stu-id="81952-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="81952-117">这主要是为了避免意外的重大更改。</span><span class="sxs-lookup"><span data-stu-id="81952-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="81952-118">参数列表和初始值设定项表达式将在确定类型后进行绑定。</span><span class="sxs-lookup"><span data-stu-id="81952-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="81952-119">表达式的类型将从目标类型推断而来，这需要是以下类型之一：</span><span class="sxs-lookup"><span data-stu-id="81952-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="81952-120">**任何结构类型**（包括元组类型）</span><span class="sxs-lookup"><span data-stu-id="81952-120">**Any struct type** (including tuple types)</span></span>
- <span data-ttu-id="81952-121">**任何引用类型**（包括委托类型）</span><span class="sxs-lookup"><span data-stu-id="81952-121">**Any reference type** (including delegate types)</span></span>
- <span data-ttu-id="81952-122">具有构造函数或 `struct` 约束的**任何类型参数**</span><span class="sxs-lookup"><span data-stu-id="81952-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="81952-123">但有以下例外：</span><span class="sxs-lookup"><span data-stu-id="81952-123">with the following exceptions:</span></span>

- <span data-ttu-id="81952-124">**枚举类型：** 并非所有枚举类型都包含常量零，因此应使用显式枚举成员。</span><span class="sxs-lookup"><span data-stu-id="81952-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="81952-125">**接口类型：** 这是一项小型功能，应更好地显式提及该类型。</span><span class="sxs-lookup"><span data-stu-id="81952-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="81952-126">**数组类型：** 数组需要一个特殊的语法来提供长度。</span><span class="sxs-lookup"><span data-stu-id="81952-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="81952-127">**动态：** 我们不允许 `new dynamic()`，因此不允许 `new()` `dynamic` 作为目标类型。</span><span class="sxs-lookup"><span data-stu-id="81952-127">**dynamic:** we don't allow `new dynamic()`, so we don't allow `new()` with `dynamic` as a target type.</span></span>

<span data-ttu-id="81952-128">还会排除*object_creation_expression*中不允许的所有其他类型，例如，指针类型。</span><span class="sxs-lookup"><span data-stu-id="81952-128">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

<span data-ttu-id="81952-129">如果目标类型是可以为 null 的值类型，则目标类型 `new` 将转换为基础类型而不是可以为 null 的类型。</span><span class="sxs-lookup"><span data-stu-id="81952-129">When the target type is a nullable value type, the target-typed `new` will be converted to the underlying type instead of the nullable type.</span></span>

> <span data-ttu-id="81952-130">**打开问题：** 我们是否应允许委托和元组作为目标类型？</span><span class="sxs-lookup"><span data-stu-id="81952-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="81952-131">以上规则包括委托（引用类型）和元组（结构类型）。</span><span class="sxs-lookup"><span data-stu-id="81952-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="81952-132">尽管这两种类型都是可构造，但如果类型为 inferable，则可以使用匿名函数或元组文本。</span><span class="sxs-lookup"><span data-stu-id="81952-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

### Miscellaneous

`throw new()` is disallowed.

Target-typed `new` is not allowed with binary operators.

It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...

It is also disallowed as a `ref`.

## Drawbacks
[drawbacks]: #drawbacks

There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.

## Alternatives
[alternatives]: #alternatives

Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.

## Questions
[questions]: #questions

- Should we forbid usages in expression trees? (no)
- How the feature interacts with `dynamic` arguments? (no special treatment)
- How IntelliSense should work with `new()`? (only when there is a single target-type)

## Design meetings

- [LDM-2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [LDM-2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM-2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM-2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM-2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
