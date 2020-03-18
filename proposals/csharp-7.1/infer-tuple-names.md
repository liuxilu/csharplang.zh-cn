---
ms.openlocfilehash: 25e95b3ab8c384a7e66e59a7f9422cc9699074d7
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483684"
---
# <a name="infer-tuple-names-aka-tuple-projection-initializers"></a><span data-ttu-id="65005-101">推断元组名称（也称为</span><span class="sxs-lookup"><span data-stu-id="65005-101">Infer tuple names (aka.</span></span> <span data-ttu-id="65005-102">元组投影初始值设定项）</span><span class="sxs-lookup"><span data-stu-id="65005-102">tuple projection initializers)</span></span>

## <a name="summary"></a><span data-ttu-id="65005-103">总结</span><span class="sxs-lookup"><span data-stu-id="65005-103">Summary</span></span>
[summary]: #summary

<span data-ttu-id="65005-104">在许多常见情况下，此功能允许忽略元组元素名称，而不是推断。</span><span class="sxs-lookup"><span data-stu-id="65005-104">In a number of common cases, this feature allows the tuple element names to be omitted and instead be inferred.</span></span> <span data-ttu-id="65005-105">例如，可以从 `(x.f1, x?.f2)`推断元素名称 "f1" 和 "f2"，而不是键入 `(f1: x.f1, f2: x?.f2)`。</span><span class="sxs-lookup"><span data-stu-id="65005-105">For instance, instead of typing `(f1: x.f1, f2: x?.f2)`, the element names "f1" and "f2" can be inferred from `(x.f1, x?.f2)`.</span></span>

<span data-ttu-id="65005-106">这与匿名类型的行为类似，允许在创建过程中推断成员名称。</span><span class="sxs-lookup"><span data-stu-id="65005-106">This parallels the behavior of  anonymous types, which allow inferring member names during creation.</span></span> <span data-ttu-id="65005-107">例如，`new { x.f1, y?.f2 }` 声明成员 "f1" 和 "f2"。</span><span class="sxs-lookup"><span data-stu-id="65005-107">For instance, `new { x.f1, y?.f2 }` declares members "f1" and "f2".</span></span>

<span data-ttu-id="65005-108">在 LINQ 中使用元组时，此操作特别有用：</span><span class="sxs-lookup"><span data-stu-id="65005-108">This is particularly handy when using tuples in LINQ:</span></span>

```csharp
// "c" and "result" have element names "f1" and "f2"
var result = list.Select(c => (c.f1, c.f2)).Where(t => t.f2 == 1); 
```

## <a name="detailed-design"></a><span data-ttu-id="65005-109">详细设计</span><span class="sxs-lookup"><span data-stu-id="65005-109">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="65005-110">更改包括两个部分：</span><span class="sxs-lookup"><span data-stu-id="65005-110">There are two parts to the change:</span></span>

1.  <span data-ttu-id="65005-111">尝试为没有显式名称的每个元组元素推断候选名称：</span><span class="sxs-lookup"><span data-stu-id="65005-111">Try to infer a candidate name for each tuple element which does not have an explicit name:</span></span>
    -   <span data-ttu-id="65005-112">使用与匿名类型的名称推理相同的规则。</span><span class="sxs-lookup"><span data-stu-id="65005-112">Using same rules as name inference for anonymous types.</span></span>
        - <span data-ttu-id="65005-113">在C#中，这可以实现三种情况： `y` （标识符）、`x.y` （简单成员访问）和 `x?.y` （条件性访问）。</span><span class="sxs-lookup"><span data-stu-id="65005-113">In C#, this allows three cases: `y` (identifier), `x.y` (simple member access) and `x?.y` (conditional access).</span></span>
        - <span data-ttu-id="65005-114">在 VB 中，这可以实现其他情况，如 `x.y()`。</span><span class="sxs-lookup"><span data-stu-id="65005-114">In VB, this allows for additional cases, such as `x.y()`.</span></span>
    -   <span data-ttu-id="65005-115">正在拒绝保留元组名称（在中C#区分大小写，不区分大小写），因为它们被禁止或已隐式。</span><span class="sxs-lookup"><span data-stu-id="65005-115">Rejecting reserved tuple names (case-sensitive in C#, case-insensitive in VB), as they are either forbidden or already implicit.</span></span> <span data-ttu-id="65005-116">例如 `ItemN`、`Rest`和 `ToString`。</span><span class="sxs-lookup"><span data-stu-id="65005-116">For instance, such as `ItemN`, `Rest`, and `ToString`.</span></span>
    -   <span data-ttu-id="65005-117">如果任何候选名称在整个元组中都是C#重复的（在中区分大小写，不区分大小写），则删除这些候选项，</span><span class="sxs-lookup"><span data-stu-id="65005-117">If any candidate names are duplicates (case-sensitive in C#, case-insensitive in VB) within the entire tuple, we drop those candidates,</span></span>
2.  <span data-ttu-id="65005-118">在转换过程中（这会对从元组文本中删除名称进行检查和发出警告），推断名称不会生成任何警告。</span><span class="sxs-lookup"><span data-stu-id="65005-118">During conversions (which check and warn about dropping names from tuple literals), inferred names would not produce any warnings.</span></span> <span data-ttu-id="65005-119">这样可以避免破坏现有的元组代码。</span><span class="sxs-lookup"><span data-stu-id="65005-119">This avoids breaking existing tuple code.</span></span>

<span data-ttu-id="65005-120">请注意，处理重复项的规则与匿名类型的规则不同。</span><span class="sxs-lookup"><span data-stu-id="65005-120">Note that the rule for handling duplicates is different than that for anonymous types.</span></span> <span data-ttu-id="65005-121">例如，`new { x.f1, x.f1 }` 生成错误，但仍允许 `(x.f1, x.f1)` （只是没有任何推断名称）。</span><span class="sxs-lookup"><span data-stu-id="65005-121">For instance, `new { x.f1, x.f1 }` produces an error, but `(x.f1, x.f1)` would still be allowed (just without any inferred names).</span></span> <span data-ttu-id="65005-122">这样可以避免破坏现有的元组代码。</span><span class="sxs-lookup"><span data-stu-id="65005-122">This avoids breaking existing tuple code.</span></span>

<span data-ttu-id="65005-123">为保持一致性，将应用到析构（在中C#为）生成的元组：</span><span class="sxs-lookup"><span data-stu-id="65005-123">For consistency, the same would apply to tuples produced by deconstruction-assignments (in C#):</span></span>

```csharp
// tuple has element names "f1" and "f2" 
var tuple = ((x.f1, x?.f2) = (1, 2));
```

<span data-ttu-id="65005-124">这同样也适用于 VB 元组，使用特定于 VB 的规则从表达式推断名称，不区分大小写的名称比较。</span><span class="sxs-lookup"><span data-stu-id="65005-124">The same would also apply to VB tuples, using the VB-specific rules for inferring name from expression and case-insensitive name comparisons.</span></span>

<span data-ttu-id="65005-125">将C# 7.1 编译器（或更高版本）与语言版本 "7.0" 一起使用时，将推断元素名称（尽管此功能不可用），但会出现用于尝试访问它们的使用站点错误。</span><span class="sxs-lookup"><span data-stu-id="65005-125">When using the C# 7.1 compiler (or later) with language version "7.0", the element names will be inferred (despite the feature not being available), but there will be a use-site error for trying to access them.</span></span> <span data-ttu-id="65005-126">这将限制稍后会出现兼容性问题的新代码的添加（如下所述）。</span><span class="sxs-lookup"><span data-stu-id="65005-126">This will limit additions of new code that would later face the compatibility issue (described below).</span></span>

## <a name="drawbacks"></a><span data-ttu-id="65005-127">缺点</span><span class="sxs-lookup"><span data-stu-id="65005-127">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="65005-128">主要缺点是，这会引入C# 7.0 的兼容性中断：</span><span class="sxs-lookup"><span data-stu-id="65005-128">The main drawback is that this introduces a compatibility break from C# 7.0:</span></span>

```csharp
Action y = () => M();
var t = (x: x, y);
t.y(); // this might have previously picked up an extension method called “y”, but would now call the lambda.
```

<span data-ttu-id="65005-129">兼容性委员会发现，此中断是可接受的，因为它受到限制，并且自元组（ C#在7.0 中）后的时间范围很短。</span><span class="sxs-lookup"><span data-stu-id="65005-129">The compatibility council found this break acceptable, given that it is limited and the time window since tuples shipped (in C# 7.0) is short.</span></span>

## <a name="references"></a><span data-ttu-id="65005-130">参考</span><span class="sxs-lookup"><span data-stu-id="65005-130">References</span></span>
- [<span data-ttu-id="65005-131">LDM 4 月 4 2017</span><span class="sxs-lookup"><span data-stu-id="65005-131">LDM April 4th 2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md#tuple-names)
- <span data-ttu-id="65005-132">[Github 讨论](https://github.com/dotnet/csharplang/issues/370)（感谢 @alrz 提出此问题）</span><span class="sxs-lookup"><span data-stu-id="65005-132">[Github discussion](https://github.com/dotnet/csharplang/issues/370) (thanks @alrz for bringing this issue up)</span></span>
- [<span data-ttu-id="65005-133">元组设计</span><span class="sxs-lookup"><span data-stu-id="65005-133">Tuples design</span></span>](https://github.com/dotnet/roslyn/blob/master/docs/features/tuples.md)
