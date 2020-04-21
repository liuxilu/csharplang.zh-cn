---
ms.openlocfilehash: 80ccdb75e2f5a022e367f3c023ea4464195deaaf
ms.sourcegitcommit: 95f5f86ba2e2a23cd4fb37bd9d1ff690c83d1191
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "81647118"
---
# <a name="target-typed-conditional-expression"></a><span data-ttu-id="da9e4-101">目标类型条件表达式</span><span class="sxs-lookup"><span data-stu-id="da9e4-101">Target-Typed Conditional Expression</span></span>

<span data-ttu-id="da9e4-102">对于条件表达式`c ? e1 : e2`，</span><span class="sxs-lookup"><span data-stu-id="da9e4-102">For a conditional expression `c ? e1 : e2`, when</span></span>

1. <span data-ttu-id="da9e4-103">和 ， 没有通用`e1`类型`e2`， 或</span><span class="sxs-lookup"><span data-stu-id="da9e4-103">there is no common type for `e1` and `e2`, or</span></span>
2. <span data-ttu-id="da9e4-104">公共类型存在，但其中一个表达式`e1`或`e2`没有隐式转换为该类型</span><span class="sxs-lookup"><span data-stu-id="da9e4-104">for which a common type exists but one of the expressions `e1` or `e2` has no implicit conversion to that type</span></span>

<span data-ttu-id="da9e4-105">我们定义一个新的*条件表达式转换*，允许隐式从条件`T`表达式转换为任何类型，其中`e1``T``e2``T`有从 到 和</span><span class="sxs-lookup"><span data-stu-id="da9e4-105">we define a new *conditional expression conversion* that permits an implicit conversion from the conditional expression to any type `T` for which there is a conversion-from-expression from `e1` to `T` and also from `e2` to `T`.</span></span>  <span data-ttu-id="da9e4-106">如果条件表达式之间`e1`既没有公共类型，也`e2`不受*条件表达式转换*的约束，则这是一个错误。</span><span class="sxs-lookup"><span data-stu-id="da9e4-106">It is an error if a conditional expression neither has a common type between `e1` and `e2` nor is subject to a *conditional expression conversion*.</span></span>

### <a name="open-issues"></a><span data-ttu-id="da9e4-107">Open Issues</span><span class="sxs-lookup"><span data-stu-id="da9e4-107">Open Issues</span></span>

<span data-ttu-id="da9e4-108">我们希望将此目标类型扩展到条件表达式具有公共类型`e1`，`e2`但没有从该公共类型转换为目标类型的情况。</span><span class="sxs-lookup"><span data-stu-id="da9e4-108">We would like to extend this target typing to cases in which the conditional expression has a common type for `e1` and `e2` but there is no conversion from that common type to the target type.</span></span> <span data-ttu-id="da9e4-109">这将使条件表达式的目标类型与交换机表达式的目标类型对齐。</span><span class="sxs-lookup"><span data-stu-id="da9e4-109">That would bring target typing of the conditional expression into alignment of target typing of the switch expression.</span></span> <span data-ttu-id="da9e4-110">然而，我们担心这将是一个重大的变化：</span><span class="sxs-lookup"><span data-stu-id="da9e4-110">However we are concerned that would be a breaking change:</span></span>

```csharp
M(b ? 1 : 2); // calls M(long) without this feature; calls M(short) with this feature

void M(short);
void M(long);
```

<span data-ttu-id="da9e4-111">我们可以通过修改规则来缩小中断更改的范围，以便*更好地从表达式转换*：如果转换为 T1 不是*条件表达式转换*，并且转换为 T2 是*条件表达式转换*，则从表达式转换为 T1 比转换 T2 更好。</span><span class="sxs-lookup"><span data-stu-id="da9e4-111">We could reduce the scope of the breaking change by modifying the rules for *better conversion from expression*: the conversion from a conditional expression to T1 is a better conversion from expression than the conversion to T2 if the conversion to T1 is not a *conditional expression conversion* and the conversion to T2 is a *conditional expression conversion*.</span></span>  <span data-ttu-id="da9e4-112">这解决了上述程序中的突发性更改（无论是否使用此功能调用`M(long)`）。</span><span class="sxs-lookup"><span data-stu-id="da9e4-112">That resolves the breaking change in the above program (it calls `M(long)` with or without this feature).</span></span> <span data-ttu-id="da9e4-113">这种方法确实有两个小缺点。</span><span class="sxs-lookup"><span data-stu-id="da9e4-113">This approach does have two small downsides.</span></span>  <span data-ttu-id="da9e4-114">首先，它与交换机表达式不完全相同：</span><span class="sxs-lookup"><span data-stu-id="da9e4-114">First, it is not quite the same as the switch expression:</span></span>

```csharp
M(b ? 1 : 2); // calls M(long)
M(b switch { true => 1, false => 2 }); // calls M(short)
```

<span data-ttu-id="da9e4-115">这仍然是一个重大的变化，但其范围不太可能影响实际程序：</span><span class="sxs-lookup"><span data-stu-id="da9e4-115">This is still a breaking change, but its scope is less likely to affect real programs:</span></span>

```csharp
M(b ? 1 : 2, 1); // calls M(long, long) without this feature; ambiguous with this feature.

M(short, short);
M(long, long);
```

<span data-ttu-id="da9e4-116">这变得模棱两可，`long`因为转换对第一个参数更好（因为它不使用*条件表达式转换*），`short`但转换为对第二个参数更好（因为`short`转换目标比 ）*better conversion target*`long`更好。</span><span class="sxs-lookup"><span data-stu-id="da9e4-116">This becomes ambiguous because the conversion to `long` is better for the first argument (because it does not use the *conditional expression conversion*), but the conversion to `short` is better for the second argument (because `short` is a *better conversion target* than `long`).</span></span> <span data-ttu-id="da9e4-117">这种重大更改似乎不太严重，因为它不会默默地更改现有程序的行为。</span><span class="sxs-lookup"><span data-stu-id="da9e4-117">This breaking change seems less serious because it does not silently change the behavior of an existing program.</span></span>

<span data-ttu-id="da9e4-118">如果我们选择对提案作这种改变，我们就会改变</span><span class="sxs-lookup"><span data-stu-id="da9e4-118">Should we elect to make this change to the proposal, we would change</span></span>

> #### <a name="better-conversion-from-expression"></a><span data-ttu-id="da9e4-119">更好地从表达式转换</span><span class="sxs-lookup"><span data-stu-id="da9e4-119">Better conversion from expression</span></span>
> 
> <span data-ttu-id="da9e4-120">`C1`给定从表达式`E`转换为类型的`T1`隐式转换，以及从表达式`C2``E`转换为类型的`T2`隐式转换`C1`，与不完全匹配`C2``E``T2`且至少包含以下一项的***转换相比，它是一种更好的转换***：</span><span class="sxs-lookup"><span data-stu-id="da9e4-120">Given an implicit conversion `C1` that converts from an expression `E` to a type `T1`, and an implicit conversion `C2` that converts from an expression `E` to a type `T2`, `C1` is a ***better conversion*** than `C2` if `E` does not exactly match `T2` and at least one of the following holds:</span></span>
> 
> * <span data-ttu-id="da9e4-121">`E`完全匹配`T1`（[完全匹配表达式](expressions.md#exactly-matching-expression)）</span><span class="sxs-lookup"><span data-stu-id="da9e4-121">`E` exactly matches `T1` ([Exactly matching Expression](expressions.md#exactly-matching-expression))</span></span>
> * <span data-ttu-id="da9e4-122">`T1`是一个更好的转换目标比`T2`（[更好的转换目标](expressions.md#better-conversion-target)）</span><span class="sxs-lookup"><span data-stu-id="da9e4-122">`T1` is a better conversion target than `T2` ([Better conversion target](expressions.md#better-conversion-target))</span></span>

<span data-ttu-id="da9e4-123">to</span><span class="sxs-lookup"><span data-stu-id="da9e4-123">to</span></span>

> #### <a name="better-conversion-from-expression"></a><span data-ttu-id="da9e4-124">更好地从表达式转换</span><span class="sxs-lookup"><span data-stu-id="da9e4-124">Better conversion from expression</span></span>
> 
> <span data-ttu-id="da9e4-125">`C1`给定从表达式`E`转换为类型的`T1`隐式转换，以及从表达式`C2``E`转换为类型的`T2`隐式转换`C1`，与不完全匹配`C2``E``T2`且至少包含以下一项的***转换相比，它是一种更好的转换***：</span><span class="sxs-lookup"><span data-stu-id="da9e4-125">Given an implicit conversion `C1` that converts from an expression `E` to a type `T1`, and an implicit conversion `C2` that converts from an expression `E` to a type `T2`, `C1` is a ***better conversion*** than `C2` if `E` does not exactly match `T2` and at least one of the following holds:</span></span>
> 
> * <span data-ttu-id="da9e4-126">`E`完全匹配`T1`（[完全匹配表达式](expressions.md#exactly-matching-expression)）</span><span class="sxs-lookup"><span data-stu-id="da9e4-126">`E` exactly matches `T1` ([Exactly matching Expression](expressions.md#exactly-matching-expression))</span></span>
> * <span data-ttu-id="da9e4-127">`T1``T2`是比 （[更好的转换目标](expressions.md#better-conversion-target)） 更好的转换目标`C1`， 和 不是*条件表达式转换*或`C2`[条件表达式转换] 更好。</span><span class="sxs-lookup"><span data-stu-id="da9e4-127">`T1` is a better conversion target than `T2` ([Better conversion target](expressions.md#better-conversion-target)) \*\*and either `C1` is not a *conditional expression conversion* or `C2` is a \*conditional expression conversion\*\*\*.</span></span>
