---
ms.openlocfilehash: 032cb8590a0b6e83f8ab6201e10720f1b254c605
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483522"
---
# <a name="nullable-enhanced-common-type"></a><span data-ttu-id="f9921-101">可为 null 的通用类型</span><span class="sxs-lookup"><span data-stu-id="f9921-101">Nullable-Enhanced Common Type</span></span>

* <span data-ttu-id="f9921-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="f9921-102">[x] Proposed</span></span>
* <span data-ttu-id="f9921-103">[] 原型：无</span><span class="sxs-lookup"><span data-stu-id="f9921-103">[ ] Prototype: None</span></span>
* <span data-ttu-id="f9921-104">[] 实现：无</span><span class="sxs-lookup"><span data-stu-id="f9921-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="f9921-105">[] 规范：请参见下面</span><span class="sxs-lookup"><span data-stu-id="f9921-105">[ ] Specification: See below</span></span>

## <a name="summary"></a><span data-ttu-id="f9921-106">总结</span><span class="sxs-lookup"><span data-stu-id="f9921-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="f9921-107">在某些情况下，当前通用类型算法结果是非常直观的，并会导致程序员将冗余强制转换为代码。</span><span class="sxs-lookup"><span data-stu-id="f9921-107">There is a situation in which the current common-type algorithm results are counter-intuitive, and results in the programmer adding what feels like a redundant cast to the code.</span></span> <span data-ttu-id="f9921-108">进行此更改后，表达式（如 `condition ? 1 : null`）将导致类型为 `int?`的值，`condition ? x : 1.0` 其中 `x` 类型为 `int?` 的表达式将导致类型 `double?`的值。</span><span class="sxs-lookup"><span data-stu-id="f9921-108">With this change, an expression such as `condition ? 1 : null` would result in a value of type `int?`, and an expression such as `condition ? x : 1.0` where `x` is of type `int?` would result in a value of type `double?`.</span></span>

## <a name="motivation"></a><span data-ttu-id="f9921-109">动机</span><span class="sxs-lookup"><span data-stu-id="f9921-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="f9921-110">这是类似于程序员的常见原因，如不必要的样板代码。</span><span class="sxs-lookup"><span data-stu-id="f9921-110">This is a common cause of what feels to the programmer like needless boilerplate code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="f9921-111">详细设计</span><span class="sxs-lookup"><span data-stu-id="f9921-111">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="f9921-112">我们修改了用于[查找一组表达式的最佳通用类型的](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions)规范，以影响下列情况：</span><span class="sxs-lookup"><span data-stu-id="f9921-112">We modify the specification for [finding the best common type of a set of expressions](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) to affect the following situations:</span></span>

- <span data-ttu-id="f9921-113">如果一个表达式是不可以为 null 的值类型 `T` 而另一个为 null 文本，则结果为类型 `T?`。</span><span class="sxs-lookup"><span data-stu-id="f9921-113">If one expression is of a non-nullable value type `T` and the other is a null literal, the result is of type `T?`.</span></span>
- <span data-ttu-id="f9921-114">如果一个表达式是可以为 null 的值类型 `T?`，另一个表达式的值类型为 `U`，并且存在从 `T` 到 `U`的隐式转换，则结果为类型 `U?`。</span><span class="sxs-lookup"><span data-stu-id="f9921-114">If one expression is of a nullable value type `T?` and the other is of a value type `U`, and there is an implicit conversion from `T` to `U`, then the result is of type `U?`.</span></span>

<span data-ttu-id="f9921-115">这应该会影响语言的以下方面：</span><span class="sxs-lookup"><span data-stu-id="f9921-115">This is expected to affect the following aspects of the language:</span></span>

- <span data-ttu-id="f9921-116">[三元表达式](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)</span><span class="sxs-lookup"><span data-stu-id="f9921-116">the [ternary expression](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)</span></span>
- <span data-ttu-id="f9921-117">隐式类型的[数组创建表达式](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions)</span><span class="sxs-lookup"><span data-stu-id="f9921-117">implicitly typed [array creation expression](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions)</span></span>
- <span data-ttu-id="f9921-118">推断[lambda 的返回类型](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type)推断类型</span><span class="sxs-lookup"><span data-stu-id="f9921-118">inferring the [return type of a lambda](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type) for type inference</span></span>
- <span data-ttu-id="f9921-119">涉及泛型的事例，如将 `M<T>(T a, T b)` 作为 `M(1, null)`调用。</span><span class="sxs-lookup"><span data-stu-id="f9921-119">cases involving generics, such as invoking `M<T>(T a, T b)` as `M(1, null)`.</span></span>

<span data-ttu-id="f9921-120">更准确地讲，我们将更改规范的以下部分（以粗体插入，删除删除线）：</span><span class="sxs-lookup"><span data-stu-id="f9921-120">More precisely, we change the following sections of the specification (insertions in bold, deletions in strikethrough):</span></span>

> #### <a name="output-type-inferences"></a><span data-ttu-id="f9921-121">输出类型推断</span><span class="sxs-lookup"><span data-stu-id="f9921-121">Output type inferences</span></span>
> 
> <span data-ttu-id="f9921-122">*输出类型推断*是通过以下方式*从*`E`*到*类型 `T` 的表达式进行的：</span><span class="sxs-lookup"><span data-stu-id="f9921-122">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>
> 
> *  <span data-ttu-id="f9921-123">如果 `E` 是 `U` （[推断返回类型](expressions.md#inferred-return-type)）且 `T` 为委托类型或具有返回类型 `Tb`的表达式树类型的匿名函数，则将*从*`U`[Lower-bound inferences](expressions.md#lower-bound-inferences)*到*`Tb`进行*下限推理*。</span><span class="sxs-lookup"><span data-stu-id="f9921-123">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
> *  <span data-ttu-id="f9921-124">否则，如果 `E` 为方法组，`T` 是参数类型为 `T1...Tk` 和返回类型 `Tb`的委托类型或表达式树类型，并且 `E` 生成一个返回类型 `T1...Tk` 的单个方法，则*从*`U`*到*`U`*生成一个方法*。`Tb`</span><span class="sxs-lookup"><span data-stu-id="f9921-124">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
> *  <span data-ttu-id="f9921-125">\* \* 否则，如果 `E` 是可以为 null 的值类型为 *`U?`的表达式*，则*从*`U`*到*`T`，并将*null 界限*添加到 `T`。</span><span class="sxs-lookup"><span data-stu-id="f9921-125">\*\*Otherwise, if `E` is an expression with nullable value type `U?`, then a *lower-bound inference* is made *from* `U` *to* `T` and a *null bound* is added to `T`.</span></span> **
> *  <span data-ttu-id="f9921-126">否则，如果 `E` 是 `U`类型的表达式，则将*从*`U`*到*`T`进行*下限推理*。</span><span class="sxs-lookup"><span data-stu-id="f9921-126">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
> *  <span data-ttu-id="f9921-127">**否则，如果 `E` 是值为 `null`的常量表达式，则会将一个*null 界限*添加到 `T`**</span><span class="sxs-lookup"><span data-stu-id="f9921-127">**Otherwise, if `E` is a constant expression with value `null`, then a *null bound* is added to `T`**</span></span> 
> *  <span data-ttu-id="f9921-128">否则，不进行推断。</span><span class="sxs-lookup"><span data-stu-id="f9921-128">Otherwise, no inferences are made.</span></span>

> #### <a name="fixing"></a><span data-ttu-id="f9921-129">更正</span><span class="sxs-lookup"><span data-stu-id="f9921-129">Fixing</span></span>
> 
> <span data-ttu-id="f9921-130">使用一组界限 `Xi` 未*固定*的类型变量是*固定*的，如下所示：</span><span class="sxs-lookup"><span data-stu-id="f9921-130">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>
> 
> *  <span data-ttu-id="f9921-131">*候选类型*集 `Uj` 以 `Xi`的边界集中的所有类型的集合开头。</span><span class="sxs-lookup"><span data-stu-id="f9921-131">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
> *  <span data-ttu-id="f9921-132">然后，我们依次检查每个绑定 for `Xi`：对于每个完全绑定 `U`，`Xi` `Uj` 的所有类型，这些类型与从候选集删除 `U` 不完全相同。</span><span class="sxs-lookup"><span data-stu-id="f9921-132">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="f9921-133">对于 `Xi` 的每个下限 `U`，从候选集中将*不*会从 `U` 中删除任何 `Uj` 类型的隐式转换。</span><span class="sxs-lookup"><span data-stu-id="f9921-133">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="f9921-134">对于 `Xi` 的每个上限 *`U`，`Uj`* 将从候选集删除没有隐式转换到 `U` 的所有类型。</span><span class="sxs-lookup"><span data-stu-id="f9921-134">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
> *  <span data-ttu-id="f9921-135">如果其余的候选类型 `Uj` 有一个唯一的类型 `V` 从该类型中有一个到所有其他候选类型的隐式转换，则~~`Xi` 固定到 `V`。~~</span><span class="sxs-lookup"><span data-stu-id="f9921-135">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then ~~`Xi` is fixed to `V`.~~</span></span>
>     -  <span data-ttu-id="f9921-136">**如果 `V` 是一个值类型，并且 `Xi`的*绑定为 null\* ，则 `Xi` 固定为 `V?`*\*</span><span class="sxs-lookup"><span data-stu-id="f9921-136">**If `V` is a value type and there is a *null bound* for `Xi`, then `Xi` is fixed to `V?`**</span></span>
>     -  <span data-ttu-id="f9921-137">**否则 `Xi` 固定到 `V`**</span><span class="sxs-lookup"><span data-stu-id="f9921-137">**Otherwise   `Xi` is fixed to `V`**</span></span>
> *  <span data-ttu-id="f9921-138">否则，类型推理将失败。</span><span class="sxs-lookup"><span data-stu-id="f9921-138">Otherwise, type inference fails.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="f9921-139">缺点</span><span class="sxs-lookup"><span data-stu-id="f9921-139">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="f9921-140">此建议可能会导致某些不兼容问题。</span><span class="sxs-lookup"><span data-stu-id="f9921-140">There may be some incompatibilities introduced by this proposal.</span></span>

## <a name="alternatives"></a><span data-ttu-id="f9921-141">备选项</span><span class="sxs-lookup"><span data-stu-id="f9921-141">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="f9921-142">无。</span><span class="sxs-lookup"><span data-stu-id="f9921-142">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="f9921-143">未解决的问题</span><span class="sxs-lookup"><span data-stu-id="f9921-143">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="f9921-144">[] 此提议引入的不兼容性严重性（如果有）以及如何进行仲裁？</span><span class="sxs-lookup"><span data-stu-id="f9921-144">[ ] What is the severity of incompatibility introduced by this proposal, if any, and how can it be moderated?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="f9921-145">设计会议</span><span class="sxs-lookup"><span data-stu-id="f9921-145">Design meetings</span></span>

<span data-ttu-id="f9921-146">无。</span><span class="sxs-lookup"><span data-stu-id="f9921-146">None.</span></span>
