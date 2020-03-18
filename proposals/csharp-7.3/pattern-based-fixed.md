---
ms.openlocfilehash: 2070cf3b3269585055791adc3427cbd134df444d
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483630"
---
# <a name="pattern-based-fixed-statement"></a><span data-ttu-id="cd901-101">基于模式的 `fixed` 语句</span><span class="sxs-lookup"><span data-stu-id="cd901-101">Pattern-based `fixed` statement</span></span>

## <a name="summary"></a><span data-ttu-id="cd901-102">总结</span><span class="sxs-lookup"><span data-stu-id="cd901-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="cd901-103">引入允许类型参与 `fixed` 语句的模式。</span><span class="sxs-lookup"><span data-stu-id="cd901-103">Introduce a pattern that would allow types to participate in `fixed` statements.</span></span> 

## <a name="motivation"></a><span data-ttu-id="cd901-104">动机</span><span class="sxs-lookup"><span data-stu-id="cd901-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="cd901-105">语言提供了一种机制，用于固定托管数据并获取指向基础缓冲区的本机指针。</span><span class="sxs-lookup"><span data-stu-id="cd901-105">The language provides a mechanism for pinning managed data and obtain a native pointer to the underlying buffer.</span></span>

```csharp
fixed(byte* ptr = byteArray)
{
   // ptr is a native pointer to the first element of the array
   // byteArray is protected from being moved/collected by the GC for the duration of this block 
}

```

<span data-ttu-id="cd901-106">可以参与 `fixed` 的一组类型是硬编码的，并仅限于数组和 `System.String`。</span><span class="sxs-lookup"><span data-stu-id="cd901-106">The set of types that can participate in `fixed` is hardcoded and limited to arrays and `System.String`.</span></span> <span data-ttu-id="cd901-107">硬编码 "特殊" 类型不会在引入 `ImmutableArray<T>`、`Span<T>``Utf8String` 的新基元时进行缩放。</span><span class="sxs-lookup"><span data-stu-id="cd901-107">Hardcoding "special" types does not scale when new primitives such as `ImmutableArray<T>`, `Span<T>`, `Utf8String` are introduced.</span></span> 

<span data-ttu-id="cd901-108">此外，`System.String` 的当前解决方案依赖于相当严格的 API。</span><span class="sxs-lookup"><span data-stu-id="cd901-108">In addition, the current solution for `System.String` relies on a fairly rigid API.</span></span> <span data-ttu-id="cd901-109">API 的形状表示，`System.String` 是一个连续的对象，它将 UTF16 编码数据嵌入对象标头的固定偏移量。</span><span class="sxs-lookup"><span data-stu-id="cd901-109">The shape of the API implies that `System.String` is a contiguous object that embeds UTF16 encoded data at a fixed offset from the object header.</span></span> <span data-ttu-id="cd901-110">在多个可能需要更改基础布局的建议中发现此类方法存在问题。</span><span class="sxs-lookup"><span data-stu-id="cd901-110">Such approach has been found problematic in several proposals that could require changes to the underlying layout.</span></span> <span data-ttu-id="cd901-111">需要能够切换到一些更灵活的方法，以便在非托管互操作目的上将 `System.String` 对象与其内部表示分离。</span><span class="sxs-lookup"><span data-stu-id="cd901-111">It would be desirable to be able to switch to something more flexible that decouples `System.String` object from its internal representation for the purpose of unmanaged interop.</span></span> 

## <a name="detailed-design"></a><span data-ttu-id="cd901-112">详细设计</span><span class="sxs-lookup"><span data-stu-id="cd901-112">Detailed design</span></span>
[design]: #detailed-design

## <a name="pattern"></a><span data-ttu-id="cd901-113">*模式*</span><span class="sxs-lookup"><span data-stu-id="cd901-113">*Pattern*</span></span> ##
<span data-ttu-id="cd901-114">可行的基于模式的 "固定" 需要：</span><span class="sxs-lookup"><span data-stu-id="cd901-114">A viable pattern-based “fixed” need to:</span></span>
-   <span data-ttu-id="cd901-115">提供托管引用以固定实例并初始化指针（最好是相同的引用）</span><span class="sxs-lookup"><span data-stu-id="cd901-115">Provide the managed references to pin the instance and to initialize the pointer (preferably this is the same reference)</span></span>
-   <span data-ttu-id="cd901-116">明确传达非托管元素的类型（即 "char" for "string"）</span><span class="sxs-lookup"><span data-stu-id="cd901-116">Convey unambiguously the type of the unmanaged element   (i.e. “char” for “string”)</span></span>
-   <span data-ttu-id="cd901-117">当没有任何引用时，规定 "empty" 情况下的行为。</span><span class="sxs-lookup"><span data-stu-id="cd901-117">Prescribe the behavior in "empty" case when there is nothing to refer to.</span></span> 
-   <span data-ttu-id="cd901-118">不应将 API 作者推送到会损害 `fixed`之外的类型的设计决策。</span><span class="sxs-lookup"><span data-stu-id="cd901-118">Should not push API authors toward design decisions that hurt the use of the type outside of `fixed`.</span></span>

<span data-ttu-id="cd901-119">我认为上述内容可以通过识别一个特殊命名的引用成员： `ref [readonly] T GetPinnableReference()`来实现。</span><span class="sxs-lookup"><span data-stu-id="cd901-119">I think the above could be satisfied by recognizing a specially named ref-returning member: `ref [readonly] T GetPinnableReference()`.</span></span>

<span data-ttu-id="cd901-120">为了 `fixed` 语句使用，必须满足以下条件：</span><span class="sxs-lookup"><span data-stu-id="cd901-120">In order to be used by the `fixed` statement the following conditions must be met:</span></span>

1. <span data-ttu-id="cd901-121">只为一个类型提供了一个这样的成员。</span><span class="sxs-lookup"><span data-stu-id="cd901-121">There is only one such member provided for a type.</span></span>
1. <span data-ttu-id="cd901-122">按 `ref` 或 `ref readonly`返回。</span><span class="sxs-lookup"><span data-stu-id="cd901-122">Returns by `ref` or `ref readonly`.</span></span> <span data-ttu-id="cd901-123">（允许`readonly` 使得不可变/只读类型的作者可以实现模式，而无需添加可在安全代码中使用的可写 API）</span><span class="sxs-lookup"><span data-stu-id="cd901-123">(`readonly` is permitted so that authors of immutable/readonly types could implement the pattern without adding writeable API that could be used in safe code)</span></span>
1. <span data-ttu-id="cd901-124">T 是非托管类型。</span><span class="sxs-lookup"><span data-stu-id="cd901-124">T is an unmanaged type.</span></span>
<span data-ttu-id="cd901-125">（因为 `T*` 成为指针类型。</span><span class="sxs-lookup"><span data-stu-id="cd901-125">(since `T*` becomes the pointer type.</span></span> <span data-ttu-id="cd901-126">如果展开 "非托管" 的概念，则该限制将自然展开。</span><span class="sxs-lookup"><span data-stu-id="cd901-126">The restriction will naturally expand if/when the notion of "unmanaged" is expanded)</span></span>
1. <span data-ttu-id="cd901-127">如果没有要固定的数据，则返回托管 `nullptr` –可能是传达空的最便宜的方式。</span><span class="sxs-lookup"><span data-stu-id="cd901-127">Returns managed `nullptr` when there is no data to pin – probably the cheapest way to convey emptiness.</span></span>
<span data-ttu-id="cd901-128">（请注意，"" 字符串返回对 "\ 0" 的引用，因为字符串以 null 结尾）</span><span class="sxs-lookup"><span data-stu-id="cd901-128">(note that “” string returns a ref to '\0' since strings are null-terminated)</span></span>

<span data-ttu-id="cd901-129">如果是 `#3`，我们可以允许空事例中的结果为未定义或特定于实现的结果。</span><span class="sxs-lookup"><span data-stu-id="cd901-129">Alternatively for the `#3` we can allow the result in empty cases be undefined or implementation-specific.</span></span> <span data-ttu-id="cd901-130">但是，这可能会使 API 更具危险性，并且容易受到滥用和意外的兼容性负担。</span><span class="sxs-lookup"><span data-stu-id="cd901-130">That, however, may make the API more dangerous and prone to abuse and unintended compatibility burdens.</span></span> 

## <a name="translation"></a><span data-ttu-id="cd901-131">*翻译*</span><span class="sxs-lookup"><span data-stu-id="cd901-131">*Translation*</span></span> ##

```csharp
fixed(byte* ptr = thing)
{ 
    // <BODY>
}
```

<span data-ttu-id="cd901-132">成为以下伪代码（并非中C#的全部可表达）</span><span class="sxs-lookup"><span data-stu-id="cd901-132">becomes the following pseudocode (not all expressible in C#)</span></span>

```csharp
byte* ptr;
// specially decorated "pinned" IL local slot, not visible to user code.
pinned ref byte _pinned;

try
{
    // NOTE: null check is omitted for value types 
    // NOTE: `thing` is evaluated only once (temporary is introduced if necessary) 
    if (thing != null)
    {
        // obtain and "pin" the reference
        _pinned = ref thing.GetPinnableReference();

        // unsafe cast in IL
        ptr = (byte*)_pinned;
    }
    else
    {
        ptr = default(byte*);
    }

    // <BODY> 
}
finally   // finally can be omitted when not observable
{
    // "unpin" the object
    _pinned = nullptr;
}
```

## <a name="drawbacks"></a><span data-ttu-id="cd901-133">缺点</span><span class="sxs-lookup"><span data-stu-id="cd901-133">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="cd901-134">GetPinnableReference 仅用于 `fixed`中，但不会阻止它在安全代码中使用，因此实现器必须记住这一点。</span><span class="sxs-lookup"><span data-stu-id="cd901-134">GetPinnableReference is intended to be used only in `fixed`, but nothing prevents its use in safe code, so implementor must keep that in mind.</span></span>

## <a name="alternatives"></a><span data-ttu-id="cd901-135">备选项</span><span class="sxs-lookup"><span data-stu-id="cd901-135">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="cd901-136">用户可以引入 GetPinnableReference 或类似成员，并将其用作</span><span class="sxs-lookup"><span data-stu-id="cd901-136">Users can introduce GetPinnableReference or similar member and use it as</span></span>
 
```csharp
fixed(byte* ptr = thing.GetPinnableReference())
{ 
    // <BODY>
}
```

<span data-ttu-id="cd901-137">如果需要备用解决方案，则没有适用于 `System.String` 的解决方案。</span><span class="sxs-lookup"><span data-stu-id="cd901-137">There is no solution for `System.String` if alternative solution is desired.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="cd901-138">未解决的问题</span><span class="sxs-lookup"><span data-stu-id="cd901-138">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="cd901-139">[] 在 "empty" 状态中的行为。</span><span class="sxs-lookup"><span data-stu-id="cd901-139">[ ] Behavior in "empty" state.</span></span><span data-ttu-id="cd901-140"> - `nullptr` 或 `undefined`？</span><span class="sxs-lookup"><span data-stu-id="cd901-140"> - `nullptr` or `undefined` ?</span></span> 
- <span data-ttu-id="cd901-141">[] 是否应考虑扩展方法？</span><span class="sxs-lookup"><span data-stu-id="cd901-141">[ ] Should the extension methods be considered ?</span></span> 
- <span data-ttu-id="cd901-142">[] 如果在 `System.String`上检测到模式，它是否会赢得此模式？</span><span class="sxs-lookup"><span data-stu-id="cd901-142">[ ] If a pattern is detected on `System.String`, should it win over ?</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="cd901-143">设计会议</span><span class="sxs-lookup"><span data-stu-id="cd901-143">Design meetings</span></span>

<span data-ttu-id="cd901-144">尚无。</span><span class="sxs-lookup"><span data-stu-id="cd901-144">None yet.</span></span> 
