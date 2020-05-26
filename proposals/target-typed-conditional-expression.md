---
ms.openlocfilehash: cd73a3d7289205f65f5144a98d32da06dfed3efc
ms.sourcegitcommit: e355841daad8c4672fae6a49c98653952d89a9cb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2020
ms.locfileid: "83775415"
---
# <a name="target-typed-conditional-expression"></a><span data-ttu-id="0aea1-101">目标类型的条件表达式</span><span class="sxs-lookup"><span data-stu-id="0aea1-101">Target-Typed Conditional Expression</span></span>

<span data-ttu-id="0aea1-102">对于条件表达式 `c ? e1 : e2` ，当</span><span class="sxs-lookup"><span data-stu-id="0aea1-102">For a conditional expression `c ? e1 : e2`, when</span></span>

1. <span data-ttu-id="0aea1-103">和没有通用类型 `e1` `e2` ，或</span><span class="sxs-lookup"><span data-stu-id="0aea1-103">there is no common type for `e1` and `e2`, or</span></span>
2. <span data-ttu-id="0aea1-104">，它的通用类型为，但其中一个表达式 `e1` 为 `e2` ，或者没有到该类型的隐式转换</span><span class="sxs-lookup"><span data-stu-id="0aea1-104">for which a common type exists but one of the expressions `e1` or `e2` has no implicit conversion to that type</span></span>

<span data-ttu-id="0aea1-105">我们定义了一个新的*条件表达式转换*，该转换允许从条件表达式到的任何类型的隐式转换 `T` （从到的转换到 `e1` `T` ，以及从到的转换） `e2` `T` 。</span><span class="sxs-lookup"><span data-stu-id="0aea1-105">we define a new *conditional expression conversion* that permits an implicit conversion from the conditional expression to any type `T` for which there is a conversion-from-expression from `e1` to `T` and also from `e2` to `T`.</span></span>  <span data-ttu-id="0aea1-106">如果条件表达式在和之间既没有通用类型，也不符合 `e1` `e2` *条件表达式转换*，则是错误的。</span><span class="sxs-lookup"><span data-stu-id="0aea1-106">It is an error if a conditional expression neither has a common type between `e1` and `e2` nor is subject to a *conditional expression conversion*.</span></span>

### <a name="open-issues"></a><span data-ttu-id="0aea1-107">未结的问题</span><span class="sxs-lookup"><span data-stu-id="0aea1-107">Open Issues</span></span>

<span data-ttu-id="0aea1-108">我们想要将此目标类型扩展到条件表达式具有通用类型的情况 `e1` ， `e2` 但不存在从该通用类型到目标类型的转换。</span><span class="sxs-lookup"><span data-stu-id="0aea1-108">We would like to extend this target typing to cases in which the conditional expression has a common type for `e1` and `e2` but there is no conversion from that common type to the target type.</span></span> <span data-ttu-id="0aea1-109">这会使条件表达式的目标类型化成为 switch 表达式的目标类型的对齐方式。</span><span class="sxs-lookup"><span data-stu-id="0aea1-109">That would bring target typing of the conditional expression into alignment of target typing of the switch expression.</span></span> <span data-ttu-id="0aea1-110">但是，我们担心会是一项重大更改：</span><span class="sxs-lookup"><span data-stu-id="0aea1-110">However we are concerned that would be a breaking change:</span></span>

```csharp
M(b ? 1 : 2); // calls M(long) without this feature; calls M(short) with this feature

void M(short);
void M(long);
```

<span data-ttu-id="0aea1-111">我们可以通过修改规则以*更好地从表达式转换*来减少重大更改的范围：如果转换为 t1 不是*条件表达式转换*，则从表达式转换为 t1 是比转换到 t2 更好的转换，而转换到 t2 是*条件表达式转换*。</span><span class="sxs-lookup"><span data-stu-id="0aea1-111">We could reduce the scope of the breaking change by modifying the rules for *better conversion from expression*: the conversion from a conditional expression to T1 is a better conversion from expression than the conversion to T2 if the conversion to T1 is not a *conditional expression conversion* and the conversion to T2 is a *conditional expression conversion*.</span></span>  <span data-ttu-id="0aea1-112">这解决了上述程序中的重大更改（它 `M(long)` 通过或不带此功能调用）。</span><span class="sxs-lookup"><span data-stu-id="0aea1-112">That resolves the breaking change in the above program (it calls `M(long)` with or without this feature).</span></span> <span data-ttu-id="0aea1-113">此方法有两个小缺点。</span><span class="sxs-lookup"><span data-stu-id="0aea1-113">This approach does have two small downsides.</span></span>  <span data-ttu-id="0aea1-114">首先，它与 switch 表达式并不完全相同：</span><span class="sxs-lookup"><span data-stu-id="0aea1-114">First, it is not quite the same as the switch expression:</span></span>

```csharp
M(b ? 1 : 2); // calls M(long)
M(b switch { true => 1, false => 2 }); // calls M(short)
```

<span data-ttu-id="0aea1-115">这仍是一项重大更改，但其作用域不太可能影响实际程序：</span><span class="sxs-lookup"><span data-stu-id="0aea1-115">This is still a breaking change, but its scope is less likely to affect real programs:</span></span>

```csharp
M(b ? 1 : 2, 1); // calls M(long, long) without this feature; ambiguous with this feature.

M(short, short);
M(long, long);
```

<span data-ttu-id="0aea1-116">这种方法变得不明确，因为的转换 `long` 更适合第一个参数（因为它不使用*条件表达式转换*），但转换为 `short` 更适合于第二个参数（因为 `short` 比*更好的转换目标* `long` ）。</span><span class="sxs-lookup"><span data-stu-id="0aea1-116">This becomes ambiguous because the conversion to `long` is better for the first argument (because it does not use the *conditional expression conversion*), but the conversion to `short` is better for the second argument (because `short` is a *better conversion target* than `long`).</span></span> <span data-ttu-id="0aea1-117">此重大更改看起来不太严重，因为它不会在无提示的情况下更改现有程序的行为。</span><span class="sxs-lookup"><span data-stu-id="0aea1-117">This breaking change seems less serious because it does not silently change the behavior of an existing program.</span></span>

<span data-ttu-id="0aea1-118">如果我们选择对建议进行此更改，我们将更改</span><span class="sxs-lookup"><span data-stu-id="0aea1-118">Should we elect to make this change to the proposal, we would change</span></span>

> #### <a name="better-conversion-from-expression"></a><span data-ttu-id="0aea1-119">表达式的更好转换</span><span class="sxs-lookup"><span data-stu-id="0aea1-119">Better conversion from expression</span></span>
> 
> <span data-ttu-id="0aea1-120">给定了 `C1` 从表达式转换为类型的隐式转换 `E` `T1` ，以及 `C2` 从表达式转换为类型的隐式转换， `E` `T2` `C1` 是比***better conversion*** `C2` `E` 不完全匹配 `T2` 且至少包含以下其中一项的更好的转换：</span><span class="sxs-lookup"><span data-stu-id="0aea1-120">Given an implicit conversion `C1` that converts from an expression `E` to a type `T1`, and an implicit conversion `C2` that converts from an expression `E` to a type `T2`, `C1` is a ***better conversion*** than `C2` if `E` does not exactly match `T2` and at least one of the following holds:</span></span>
> 
> * <span data-ttu-id="0aea1-121">`E`完全匹配 `T1` （[完全匹配的表达式](expressions.md#exactly-matching-expression)）</span><span class="sxs-lookup"><span data-stu-id="0aea1-121">`E` exactly matches `T1` ([Exactly matching Expression](expressions.md#exactly-matching-expression))</span></span>
> * <span data-ttu-id="0aea1-122">`T1`比更好的转换目标 `T2` （[更好的转换目标](expressions.md#better-conversion-target)）</span><span class="sxs-lookup"><span data-stu-id="0aea1-122">`T1` is a better conversion target than `T2` ([Better conversion target](expressions.md#better-conversion-target))</span></span>

<span data-ttu-id="0aea1-123">更改为</span><span class="sxs-lookup"><span data-stu-id="0aea1-123">to</span></span>

> #### <a name="better-conversion-from-expression"></a><span data-ttu-id="0aea1-124">表达式的更好转换</span><span class="sxs-lookup"><span data-stu-id="0aea1-124">Better conversion from expression</span></span>
> 
> <span data-ttu-id="0aea1-125">给定了 `C1` 从表达式转换为类型的隐式转换 `E` `T1` ，以及 `C2` 从表达式转换为类型的隐式转换， `E` `T2` `C1` 是比***better conversion*** `C2` `E` 不完全匹配 `T2` 且至少包含以下其中一项的更好的转换：</span><span class="sxs-lookup"><span data-stu-id="0aea1-125">Given an implicit conversion `C1` that converts from an expression `E` to a type `T1`, and an implicit conversion `C2` that converts from an expression `E` to a type `T2`, `C1` is a ***better conversion*** than `C2` if `E` does not exactly match `T2` and at least one of the following holds:</span></span>
> 
> * <span data-ttu-id="0aea1-126">`E`完全匹配 `T1` （[完全匹配的表达式](expressions.md#exactly-matching-expression)）</span><span class="sxs-lookup"><span data-stu-id="0aea1-126">`E` exactly matches `T1` ([Exactly matching Expression](expressions.md#exactly-matching-expression))</span></span>
> * <span data-ttu-id="0aea1-127">\**`C1`不是一个*条件表达式转换\*，并且 `C2` 是一个 \* 条件表达式转换 \* \* \*。</span><span class="sxs-lookup"><span data-stu-id="0aea1-127">\*\*`C1` is not a *conditional expression conversion* and `C2` is a \*conditional expression conversion\*\*\*.</span></span>
> * <span data-ttu-id="0aea1-128">`T1`比 `T2` （[更好的转换目标](expressions.md#better-conversion-target)） \* \* 更好的转换目标，并且 `C1` 和 `C2` 都是*条件表达式*转换，或者两者都不是 \* 条件表达式转换 \* \* \*。</span><span class="sxs-lookup"><span data-stu-id="0aea1-128">`T1` is a better conversion target than `T2` ([Better conversion target](expressions.md#better-conversion-target)) \*\*and either `C1` and `C2` are both *conditional expression conversions* or neither is a \*conditional expression conversion\*\*\*.</span></span>
