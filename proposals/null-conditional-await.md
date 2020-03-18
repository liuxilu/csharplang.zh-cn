---
ms.openlocfilehash: 3d10cacef98e802333c8cbe65edb93c19c74cabf
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483492"
---
# <a name="null-conditional-await"></a><span data-ttu-id="5efa6-101">null-条件 await</span><span class="sxs-lookup"><span data-stu-id="5efa6-101">null-conditional await</span></span>

* <span data-ttu-id="5efa6-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="5efa6-102">[x] Proposed</span></span>
* <span data-ttu-id="5efa6-103">[] 原型：无</span><span class="sxs-lookup"><span data-stu-id="5efa6-103">[ ] Prototype: None</span></span>
* <span data-ttu-id="5efa6-104">[] 实现：无</span><span class="sxs-lookup"><span data-stu-id="5efa6-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="5efa6-105">[] 规范：已开始，小于</span><span class="sxs-lookup"><span data-stu-id="5efa6-105">[ ] Specification: Started, below</span></span>

## <a name="summary"></a><span data-ttu-id="5efa6-106">总结</span><span class="sxs-lookup"><span data-stu-id="5efa6-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="5efa6-107">支持形式 `await? e`的表达式，该表达式在 `e` 为非 null 时等待，否则将导致 `null`。</span><span class="sxs-lookup"><span data-stu-id="5efa6-107">Support an expression of the form `await? e`, which awaits `e` if it is non-null, otherwise it results in `null`.</span></span>

## <a name="motivation"></a><span data-ttu-id="5efa6-108">动机</span><span class="sxs-lookup"><span data-stu-id="5efa6-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="5efa6-109">这是一种常见的编码模式，此功能与现有的 null 传播和 null 合并运算符的协同工作非常好。</span><span class="sxs-lookup"><span data-stu-id="5efa6-109">This is a common coding pattern, and this feature would have nice synergy with the existing null-propagating and null-coalescing operators.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="5efa6-110">详细设计</span><span class="sxs-lookup"><span data-stu-id="5efa6-110">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="5efa6-111">我们添加了一种新形式的*await_expression*：</span><span class="sxs-lookup"><span data-stu-id="5efa6-111">We add a new form of the *await_expression*:</span></span>

```antlr
await_expression
    : 'await' '?' unary_expression
    ;
```

<span data-ttu-id="5efa6-112">只有在该操作数为非 null 时，null 条件 `await` 运算符才会等待其操作数。</span><span class="sxs-lookup"><span data-stu-id="5efa6-112">The null-conditional `await` operator awaits its operand only if that operand is non-null.</span></span> <span data-ttu-id="5efa6-113">否则，应用运算符的结果为 null。</span><span class="sxs-lookup"><span data-stu-id="5efa6-113">Otherwise the result of applying the operator is null.</span></span>

<span data-ttu-id="5efa6-114">使用[null 条件运算符的规则](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator)计算结果的类型。</span><span class="sxs-lookup"><span data-stu-id="5efa6-114">The type of the result is computed using the [rules for the null-conditional operator](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator).</span></span>

> <span data-ttu-id="5efa6-115">**注意：** 如果 `e` 的类型为 `Task`，则如果 `e` 为 `null`，则 `await? e;` 不会执行任何操作，并且等待 `e` `null`。</span><span class="sxs-lookup"><span data-stu-id="5efa6-115">**NOTE:** If `e` is of type `Task`, then `await? e;` would do nothing if `e` is `null`, and await `e` if it is not `null`.</span></span>
>
> <span data-ttu-id="5efa6-116">如果 `e` 的类型 `Task<K>` （其中 `K` 为值类型），则 `await? e` 将生成类型 `K?`的值。</span><span class="sxs-lookup"><span data-stu-id="5efa6-116">If `e` is of type `Task<K>` where `K` is a value type, then `await? e` would yield a value of type `K?`.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="5efa6-117">缺点</span><span class="sxs-lookup"><span data-stu-id="5efa6-117">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="5efa6-118">与任何语言功能一样，我们必须回答对语言的额外复杂性是否偿还，以提供给将从功能中受益的C#程序的主体。</span><span class="sxs-lookup"><span data-stu-id="5efa6-118">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="5efa6-119">备选项</span><span class="sxs-lookup"><span data-stu-id="5efa6-119">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="5efa6-120">尽管它需要一些样板代码，但该运算符的用法通常可以替换为表达式，如 `(e == null) ? null : await e` 或 `if (e != null) await e`之类的语句。</span><span class="sxs-lookup"><span data-stu-id="5efa6-120">Although it requires some boilerplate code, uses of this operator can often be replaced by an expression something like `(e == null) ? null : await e` or a statement like `if (e != null) await e`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="5efa6-121">未解决的问题</span><span class="sxs-lookup"><span data-stu-id="5efa6-121">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="5efa6-122">[] 需要 LDM 审核</span><span class="sxs-lookup"><span data-stu-id="5efa6-122">[ ] Requires LDM review</span></span>

## <a name="design-meetings"></a><span data-ttu-id="5efa6-123">设计会议</span><span class="sxs-lookup"><span data-stu-id="5efa6-123">Design meetings</span></span>

<span data-ttu-id="5efa6-124">无。</span><span class="sxs-lookup"><span data-stu-id="5efa6-124">None.</span></span>
