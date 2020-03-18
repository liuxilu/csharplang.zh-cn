---
ms.openlocfilehash: 7ea62713416ef82034963aef06f3cb11703342ed
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483588"
---
# <a name="stackalloc-array-initializers"></a><span data-ttu-id="509a5-101">Stackalloc 数组初始值设定项</span><span class="sxs-lookup"><span data-stu-id="509a5-101">Stackalloc array initializers</span></span>

## <a name="summary"></a><span data-ttu-id="509a5-102">总结</span><span class="sxs-lookup"><span data-stu-id="509a5-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="509a5-103">允许将数组初始值设定项语法用于 `stackalloc`。</span><span class="sxs-lookup"><span data-stu-id="509a5-103">Allow array initializer syntax to be used with `stackalloc`.</span></span>

## <a name="motivation"></a><span data-ttu-id="509a5-104">动机</span><span class="sxs-lookup"><span data-stu-id="509a5-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="509a5-105">普通数组可以在创建时初始化其元素。</span><span class="sxs-lookup"><span data-stu-id="509a5-105">Ordinary arrays can have their elements initialized at creation time.</span></span> <span data-ttu-id="509a5-106">在 `stackalloc` 情况下，似乎合理。</span><span class="sxs-lookup"><span data-stu-id="509a5-106">It seems reasonable to allow that in `stackalloc` case.</span></span>

<span data-ttu-id="509a5-107">不允许使用这种语法的问题，这种情况的发生方式 `stackalloc` 相对频繁。</span><span class="sxs-lookup"><span data-stu-id="509a5-107">The question of why such syntax is not allowed with `stackalloc` arises fairly frequently.</span></span>  
<span data-ttu-id="509a5-108">请参阅，例如[#1112](https://github.com/dotnet/csharplang/issues/1112)</span><span class="sxs-lookup"><span data-stu-id="509a5-108">See, for example, [#1112](https://github.com/dotnet/csharplang/issues/1112)</span></span>

## <a name="detailed-design"></a><span data-ttu-id="509a5-109">详细设计</span><span class="sxs-lookup"><span data-stu-id="509a5-109">Detailed design</span></span>

<span data-ttu-id="509a5-110">可以通过以下语法创建普通的数组：</span><span class="sxs-lookup"><span data-stu-id="509a5-110">Ordinary arrays can be created through the following syntax:</span></span>

```csharp
new int[3]
new int[3] { 1, 2, 3 }
new int[] { 1, 2, 3 }
new[] { 1, 2, 3 }
```

<span data-ttu-id="509a5-111">我们应该允许通过创建堆栈分配的数组：</span><span class="sxs-lookup"><span data-stu-id="509a5-111">We should allow stack allocated arrays be created through:</span></span>  

```csharp
stackalloc int[3]               // currently allowed
stackalloc int[3] { 1, 2, 3 }
stackalloc int[] { 1, 2, 3 }
stackalloc[] { 1, 2, 3 }
```

<span data-ttu-id="509a5-112">所有事例的语义大致与数组的语义相同。</span><span class="sxs-lookup"><span data-stu-id="509a5-112">The semantics of all cases is roughly the same as with arrays.</span></span>  
<span data-ttu-id="509a5-113">例如：在最后一种情况下，元素类型从初始值设定项中推断出来，并且必须为 "非托管" 类型。</span><span class="sxs-lookup"><span data-stu-id="509a5-113">For example: in the last case the element type is inferred from the initializer and must be an "unmanaged" type.</span></span>

<span data-ttu-id="509a5-114">注意：该功能不依赖于要 `Span<T>`的目标。</span><span class="sxs-lookup"><span data-stu-id="509a5-114">NOTE: the feature is not dependent on the target being a `Span<T>`.</span></span> <span data-ttu-id="509a5-115">它就像在 `T*` 情况下一样适用，因此在 `Span<T>` 情况下将其谓语视为合理。</span><span class="sxs-lookup"><span data-stu-id="509a5-115">It is just as applicable in `T*` case, so it does not seem reasonable to predicate it on `Span<T>` case.</span></span>  

## <a name="translation"></a><span data-ttu-id="509a5-116">翻译</span><span class="sxs-lookup"><span data-stu-id="509a5-116">Translation</span></span>

<span data-ttu-id="509a5-117">简单的实现可以在创建后通过一系列基于元素的赋值直接初始化数组。</span><span class="sxs-lookup"><span data-stu-id="509a5-117">The naive implementation could just initialize the array right after creation through a series of element-wise assignments.</span></span>  

<span data-ttu-id="509a5-118">与使用数组的情况相似，检测所有或大多数元素都是可直接复制到类型的情况，并通过复制所有常量元素的预先创建的状态来使用更有效的方法。</span><span class="sxs-lookup"><span data-stu-id="509a5-118">Similarly to the case with arrays, it might be possible and desirable to detect cases where all or most of the elements are blittable types and use more efficient techniques by copying over the pre-created state of all the constant elements.</span></span> 

## <a name="drawbacks"></a><span data-ttu-id="509a5-119">缺点</span><span class="sxs-lookup"><span data-stu-id="509a5-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

## <a name="alternatives"></a><span data-ttu-id="509a5-120">备选项</span><span class="sxs-lookup"><span data-stu-id="509a5-120">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="509a5-121">这是一项便利功能。</span><span class="sxs-lookup"><span data-stu-id="509a5-121">This is a convenience feature.</span></span> <span data-ttu-id="509a5-122">可以不执行任何操作。</span><span class="sxs-lookup"><span data-stu-id="509a5-122">It is possible to just do nothing.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="509a5-123">未解决的问题</span><span class="sxs-lookup"><span data-stu-id="509a5-123">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="509a5-124">设计会议</span><span class="sxs-lookup"><span data-stu-id="509a5-124">Design meetings</span></span>

<span data-ttu-id="509a5-125">尚无。</span><span class="sxs-lookup"><span data-stu-id="509a5-125">None yet.</span></span> 
