---
ms.openlocfilehash: fdd858c895d56a7a6b410e6ea7be3032e4851fd6
ms.sourcegitcommit: 5a88d5432d32c690c6b870fc4be32cf26cadd76f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/11/2019
ms.locfileid: "79483888"
---
# <a name="null-coalescing-assignment"></a><span data-ttu-id="0835c-101">NULL 合并分配</span><span class="sxs-lookup"><span data-stu-id="0835c-101">null coalescing assignment</span></span>

* <span data-ttu-id="0835c-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="0835c-102">[x] Proposed</span></span>
* <span data-ttu-id="0835c-103">[] 原型：未启动</span><span class="sxs-lookup"><span data-stu-id="0835c-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="0835c-104">[] 实现：未启动</span><span class="sxs-lookup"><span data-stu-id="0835c-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="0835c-105">[] 规范：以下</span><span class="sxs-lookup"><span data-stu-id="0835c-105">[ ] Specification: Below</span></span>

## <a name="summary"></a><span data-ttu-id="0835c-106">总结</span><span class="sxs-lookup"><span data-stu-id="0835c-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="0835c-107">简化了一个常见的编码模式，其中如果变量为 null，则为该变量分配一个值。</span><span class="sxs-lookup"><span data-stu-id="0835c-107">Simplifies a common coding pattern where a variable is assigned a value if it is null.</span></span>

<span data-ttu-id="0835c-108">作为此建议的一部分，我们还将放宽 `??` 上的类型要求，以允许类型为不受约束的类型参数的表达式在左侧使用。</span><span class="sxs-lookup"><span data-stu-id="0835c-108">As part of this proposal, we will also loosen the type requirements on `??` to allow an expression whose type is an unconstrained type parameter to be used on the left-hand side.</span></span>

## <a name="motivation"></a><span data-ttu-id="0835c-109">动机</span><span class="sxs-lookup"><span data-stu-id="0835c-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="0835c-110">通常会看到窗体的代码</span><span class="sxs-lookup"><span data-stu-id="0835c-110">It is common to see code of the form</span></span>

```csharp
if (variable == null)
{
    variable = expression;
}
```

<span data-ttu-id="0835c-111">此建议向执行此函数的语言添加不可重载的二元运算符。</span><span class="sxs-lookup"><span data-stu-id="0835c-111">This proposal adds a non-overloadable binary operator to the language that performs this function.</span></span>

<span data-ttu-id="0835c-112">此功能至少有8个单独的社区请求。</span><span class="sxs-lookup"><span data-stu-id="0835c-112">There have been at least eight separate community requests for this feature.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="0835c-113">详细设计</span><span class="sxs-lookup"><span data-stu-id="0835c-113">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="0835c-114">我们添加了一个新的赋值运算符形式</span><span class="sxs-lookup"><span data-stu-id="0835c-114">We add a new form of assignment operator</span></span>

``` antlr
assignment_operator
    : '??='
    ;
```

<span data-ttu-id="0835c-115">它遵循[复合赋值运算符的现有语义规则](../../spec/expressions.md#compound-assignment)，只不过如果左侧非 null，则 elide 赋值。</span><span class="sxs-lookup"><span data-stu-id="0835c-115">Which follows the [existing semantic rules for compound assignment operators](../../spec/expressions.md#compound-assignment), except that we elide the assignment if the left-hand side is non-null.</span></span> <span data-ttu-id="0835c-116">此功能的规则如下所示。</span><span class="sxs-lookup"><span data-stu-id="0835c-116">The rules for this feature are as follows.</span></span>

<span data-ttu-id="0835c-117">给定的 `a ??= b`，其中 `A` 是 `a`类型，`B` 是 `b`的类型，`A0` 是 `A` 的基础类型（如果 `A` 是可以为 null 的值类型）：</span><span class="sxs-lookup"><span data-stu-id="0835c-117">Given `a ??= b`, where `A` is the type of `a`, `B` is the type of `b`, and `A0` is the underlying type of `A` if `A` is a nullable value type:</span></span>

1. <span data-ttu-id="0835c-118">如果 `A` 不存在或者不是可以为 null 的值类型，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="0835c-118">If `A` does not exist or is a non-nullable value type, a compile-time error occurs.</span></span>
2. <span data-ttu-id="0835c-119">如果 `B` 无法隐式转换为 `A` 或 `A0` （如果 `A0` 存在），则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="0835c-119">If `B` is not implicitly convertible to `A` or `A0` (if `A0` exists), a compile-time error occurs.</span></span>
3. <span data-ttu-id="0835c-120">如果 `A0` 存在并且 `B` 可隐式转换为 `A0`，并且 `B` 不是动态的，则 `a ??= b` 的类型为 `A0`。</span><span class="sxs-lookup"><span data-stu-id="0835c-120">If `A0` exists and `B` is implicitly convertible to `A0`, and `B` is not dynamic, then the type of `a ??= b` is `A0`.</span></span> <span data-ttu-id="0835c-121">在运行时计算 `a ??= b`：</span><span class="sxs-lookup"><span data-stu-id="0835c-121">`a ??= b` is evaluated at runtime as:</span></span>
   ```C#
   var tmp = a.GetValueOrDefault();
   if (!a.HasValue) { tmp = b; a = tmp; }
   tmp
   ```
   <span data-ttu-id="0835c-122">但 `a` 只计算一次。</span><span class="sxs-lookup"><span data-stu-id="0835c-122">Except that `a` is only evaluated once.</span></span>
4. <span data-ttu-id="0835c-123">否则，将 `A``a ??= b` 的类型。</span><span class="sxs-lookup"><span data-stu-id="0835c-123">Otherwise, the type of `a ??= b` is `A`.</span></span> <span data-ttu-id="0835c-124">`a ??= b` 在运行时作为 `a ?? (a = b)`计算，只不过 `a` 只计算一次。</span><span class="sxs-lookup"><span data-stu-id="0835c-124">`a ??= b` is evaluated at runtime as `a ?? (a = b)`, except that `a` is only evaluated once.</span></span>


<span data-ttu-id="0835c-125">为实现 `??`类型要求的 relaxation，我们将更新当前声明的规范，如果 `a ?? b`，其中 `A` 是 `a`的类型：</span><span class="sxs-lookup"><span data-stu-id="0835c-125">For the relaxation of the type requirements of `??`, we update the spec where it currently states that, given `a ?? b`, where `A` is the type of `a`:</span></span>

> 1. <span data-ttu-id="0835c-126">如果存在并且不是可以为 null 的类型或引用类型，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="0835c-126">If A exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>

<span data-ttu-id="0835c-127">我们放宽了以下要求：</span><span class="sxs-lookup"><span data-stu-id="0835c-127">We relax this requirement to:</span></span>

1. <span data-ttu-id="0835c-128">如果存在并且是不可为 null 的值类型，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="0835c-128">If A exists and is a non-nullable value type, a compile-time error occurs.</span></span>

<span data-ttu-id="0835c-129">这允许空合并运算符处理不受约束的类型参数，因为不存在不受约束的类型参数 T，它不是可以为 null 的类型，并且不是引用类型。</span><span class="sxs-lookup"><span data-stu-id="0835c-129">This allows the null coalescing operator to work on unconstrained type parameters, as the unconstrained type parameter T exists, is not a nullable type, and is not a reference type.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="0835c-130">缺点</span><span class="sxs-lookup"><span data-stu-id="0835c-130">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="0835c-131">与任何语言功能一样，我们必须回答对语言的额外复杂性是否偿还，以提供给将从功能中受益的C#程序的主体。</span><span class="sxs-lookup"><span data-stu-id="0835c-131">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="0835c-132">备选项</span><span class="sxs-lookup"><span data-stu-id="0835c-132">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="0835c-133">程序员可以手动编写 `(x = x ?? y)`、`if (x == null) x = y;`或 `x ?? (x = y)`。</span><span class="sxs-lookup"><span data-stu-id="0835c-133">The programmer can write `(x = x ?? y)`, `if (x == null) x = y;`, or `x ?? (x = y)` by hand.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="0835c-134">未解决的问题</span><span class="sxs-lookup"><span data-stu-id="0835c-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="0835c-135">[] 需要 LDM 审核</span><span class="sxs-lookup"><span data-stu-id="0835c-135">[ ] Requires LDM review</span></span>
- <span data-ttu-id="0835c-136">[] 是否还支持 `&&=` 和 `||=` 运算符？</span><span class="sxs-lookup"><span data-stu-id="0835c-136">[ ] Should we also support `&&=` and `||=` operators?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="0835c-137">设计会议</span><span class="sxs-lookup"><span data-stu-id="0835c-137">Design meetings</span></span>

<span data-ttu-id="0835c-138">无。</span><span class="sxs-lookup"><span data-stu-id="0835c-138">None.</span></span>
