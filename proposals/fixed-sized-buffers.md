---
ms.openlocfilehash: b51f27b2f58fd19851c80beb9cedcbd32b80b165
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483570"
---
# <a name="fixed-sized-buffers"></a><span data-ttu-id="bf878-101">固定大小的缓冲区</span><span class="sxs-lookup"><span data-stu-id="bf878-101">Fixed Sized Buffers</span></span>

* <span data-ttu-id="bf878-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="bf878-102">[x] Proposed</span></span>
* <span data-ttu-id="bf878-103">[] 原型：未启动</span><span class="sxs-lookup"><span data-stu-id="bf878-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="bf878-104">[] 实现：未启动</span><span class="sxs-lookup"><span data-stu-id="bf878-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="bf878-105">[] 规范：未启动</span><span class="sxs-lookup"><span data-stu-id="bf878-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="bf878-106">总结</span><span class="sxs-lookup"><span data-stu-id="bf878-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="bf878-107">提供用于将固定大小的缓冲区声明为C#语言的常规用途和安全机制。</span><span class="sxs-lookup"><span data-stu-id="bf878-107">Provide a general-purpose and safe mechanism for declaring fixed sized buffers to the C# language.</span></span>

## <a name="motivation"></a><span data-ttu-id="bf878-108">动机</span><span class="sxs-lookup"><span data-stu-id="bf878-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="bf878-109">目前，用户能够在不安全的上下文中创建固定大小的缓冲区。</span><span class="sxs-lookup"><span data-stu-id="bf878-109">Today, users have the ability to create fixed-sized buffers in an unsafe-context.</span></span> <span data-ttu-id="bf878-110">但是，这需要用户处理指针、手动执行边界检查并仅支持有限的一组类型（`bool`、`byte`、`char`、`short`、`int`、`long`、`sbyte`、`ushort`、`uint`、`ulong`、`float`和 `double`）。</span><span class="sxs-lookup"><span data-stu-id="bf878-110">However, this requires the user to deal with pointers, manually perform bounds checks, and only supports a limited set of types (`bool`, `byte`, `char`, `short`, `int`, `long`, `sbyte`, `ushort`, `uint`, `ulong`, `float`, and `double`).</span></span>

<span data-ttu-id="bf878-111">最常见的问题是，固定大小的缓冲区无法在安全代码中编制索引。</span><span class="sxs-lookup"><span data-stu-id="bf878-111">The most common complaint is that fixed-size buffers cannot be indexed in safe code.</span></span> <span data-ttu-id="bf878-112">无法使用更多的类型。</span><span class="sxs-lookup"><span data-stu-id="bf878-112">Inability to use more types is the second.</span></span>

<span data-ttu-id="bf878-113">只需稍作调整，就能提供通用固定大小的缓冲区（支持任何类型），可以在安全的上下文中使用，并执行自动边界检查。</span><span class="sxs-lookup"><span data-stu-id="bf878-113">With a few minor tweaks, we could provide general-purpose fixed-sized buffers which support any type, can be used in a safe context, and have automatic bounds checking performed.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="bf878-114">详细设计</span><span class="sxs-lookup"><span data-stu-id="bf878-114">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="bf878-115">其中一种方法是通过以下内容声明安全固定大小的缓冲区：</span><span class="sxs-lookup"><span data-stu-id="bf878-115">One would declare a safe fixed-sized buffer via the following:</span></span>

```csharp
public fixed DXGI_RGB GammaCurve[1025];
```

<span data-ttu-id="bf878-116">编译器会将声明转换为类似于以下内容的内部表示形式</span><span class="sxs-lookup"><span data-stu-id="bf878-116">The declaration would get translated into an internal representation by the compiler that is similar to the following</span></span>

```csharp
[FixedBuffer(typeof(DXGI_RGB), 1024)]
public ConsoleApp1.<Buffer>e__FixedBuffer_1024<DXGI_RGB> GammaCurve;

// Pack = 0 is the default packing and should result in indexable layout.
[CompilerGenerated, UnsafeValueType, StructLayout(LayoutKind.Sequential, Pack = 0)]
struct <Buffer>e__FixedBuffer_1024<T>
{
    private T _e0;
    private T _e1;
    // _e2 ... _e1023
    private T _e1024;

    public ref T this[int index] => ref (uint)index <= 1024u ?
                                         ref RefAdd<T>(ref _e0, index):
                                         throw new IndexOutOfRange();
}
```

<span data-ttu-id="bf878-117">由于固定大小的缓冲区不再需要使用 `fixed`，因此允许任何元素类型是有意义的。</span><span class="sxs-lookup"><span data-stu-id="bf878-117">Since such fixed-sized buffers no longer require use of `fixed`, it makes sense to allow any element type.</span></span>  

> <span data-ttu-id="bf878-118">注意：仍将支持 `fixed`，但仅当元素类型为 `blittable`</span><span class="sxs-lookup"><span data-stu-id="bf878-118">NOTE: `fixed` will still be supported, but only if the element type is `blittable`</span></span>

## <a name="drawbacks"></a><span data-ttu-id="bf878-119">缺点</span><span class="sxs-lookup"><span data-stu-id="bf878-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

* <span data-ttu-id="bf878-120">向后兼容性可能存在一些挑战，但由于现有固定大小的缓冲区仅适用于选择的基元类型，因此，如果用户将固定缓冲区视为，则编译器可能会继续 "只工作"变为.</span><span class="sxs-lookup"><span data-stu-id="bf878-120">There could be some challenges with backwards compatibility, but given that the existing fixed-sized buffers only work with a selection of primitive types, it should be possible for the compiler to continue "just-working" if the user treats the fixed-buffer as a pointer.</span></span>
* <span data-ttu-id="bf878-121">不兼容的构造可能需要使用略微不同 `v2` 编码才能隐藏旧编译器中的字段。</span><span class="sxs-lookup"><span data-stu-id="bf878-121">Incompatible constructs may need to use slightly different `v2` encoding to hide the fields from old compiler.</span></span>
* <span data-ttu-id="bf878-122">对于泛型类型，在 IL 规范中未定义打包。</span><span class="sxs-lookup"><span data-stu-id="bf878-122">Packing is not well defined in IL spec for generic types.</span></span> <span data-ttu-id="bf878-123">尽管此方法应可行，但我们将边上未记录的行为。</span><span class="sxs-lookup"><span data-stu-id="bf878-123">While the approach should work, we will be bordering on undocumented behavior.</span></span> <span data-ttu-id="bf878-124">我们应该将其记录下来，确保像 Mono 这样的其他 Jit 具有相同的行为。</span><span class="sxs-lookup"><span data-stu-id="bf878-124">We should make that documented and make sure other JITs like Mono have the same behavior.</span></span>
* <span data-ttu-id="bf878-125">为每个长度指定一个单独的类型（如果支持，则可能为 `readonly` 字段指定一个类型）将对元数据产生影响。</span><span class="sxs-lookup"><span data-stu-id="bf878-125">Specifying a separate type for every length (an possibly another for `readonly` fields, if supported) will have impact on metadata.</span></span> <span data-ttu-id="bf878-126">它将受给定应用中不同大小的数组数目的限制。</span><span class="sxs-lookup"><span data-stu-id="bf878-126">It will be bound by the number of arrays of different sizes in the given app.</span></span>
* <span data-ttu-id="bf878-127">`ref` math 不是正式可验证的（因为它不安全）。</span><span class="sxs-lookup"><span data-stu-id="bf878-127">`ref` math is not formally verifiable (since it is unsafe).</span></span> <span data-ttu-id="bf878-128">我们需要寻找一种方法来更新验证规则，以了解我们的使用是否正常。</span><span class="sxs-lookup"><span data-stu-id="bf878-128">We will need to find a way to update verification rules to know that our use is ok.</span></span>

## <a name="alternatives"></a><span data-ttu-id="bf878-129">备选项</span><span class="sxs-lookup"><span data-stu-id="bf878-129">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="bf878-130">手动声明你的结构，并使用不安全代码来构造索引器。</span><span class="sxs-lookup"><span data-stu-id="bf878-130">Manually declare your structures and use unsafe code to construct indexers.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="bf878-131">未解决的问题</span><span class="sxs-lookup"><span data-stu-id="bf878-131">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="bf878-132">是否应允许 `readonly`？</span><span class="sxs-lookup"><span data-stu-id="bf878-132">should we allow `readonly`?</span></span>  <span data-ttu-id="bf878-133">（具有 readonly 索引器）</span><span class="sxs-lookup"><span data-stu-id="bf878-133">(with readonly indexer)</span></span>
- <span data-ttu-id="bf878-134">是否应允许数组初始值设定项？</span><span class="sxs-lookup"><span data-stu-id="bf878-134">should we allow array initializers?</span></span>
- <span data-ttu-id="bf878-135">是否需要 `fixed` 关键字？</span><span class="sxs-lookup"><span data-stu-id="bf878-135">is `fixed` keyword necessary?</span></span>
- <span data-ttu-id="bf878-136">`foreach`?</span><span class="sxs-lookup"><span data-stu-id="bf878-136">`foreach`?</span></span>
- <span data-ttu-id="bf878-137">仅结构中的实例字段？</span><span class="sxs-lookup"><span data-stu-id="bf878-137">only instance fields in structs?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="bf878-138">设计会议</span><span class="sxs-lookup"><span data-stu-id="bf878-138">Design meetings</span></span>

<span data-ttu-id="bf878-139">链接到影响此建议的设计说明，并在一个句子中介绍它们所导致的更改。</span><span class="sxs-lookup"><span data-stu-id="bf878-139">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>