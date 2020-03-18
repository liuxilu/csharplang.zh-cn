---
ms.openlocfilehash: 36f3e6204d12c2569b3a55f3a47f58337e8a08e4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483606"
---
# <a name="indexing-fixed-fields-should-not-require-pinning-regardless-of-the-movableunmovable-context"></a><span data-ttu-id="faf63-101">与可移动/不可移动的上下文无关，索引 `fixed` 字段不应进行固定。</span><span class="sxs-lookup"><span data-stu-id="faf63-101">Indexing `fixed` fields should not require pinning regardless of the movable/unmovable context.</span></span> #

<span data-ttu-id="faf63-102">更改的大小为 bug 修复。</span><span class="sxs-lookup"><span data-stu-id="faf63-102">The change has the size of a bug fix.</span></span> <span data-ttu-id="faf63-103">它可以处于7.3，并且不会与我们更进一步的方向发生冲突。</span><span class="sxs-lookup"><span data-stu-id="faf63-103">It can be in 7.3 and does not conflict with whatever direction we take further.</span></span>
<span data-ttu-id="faf63-104">这一更改只是为了允许下列方案工作，即使 `s` 可移动。</span><span class="sxs-lookup"><span data-stu-id="faf63-104">This change is only about allowing the following scenario to work even though `s` is moveable.</span></span> <span data-ttu-id="faf63-105">当 `s` 不可可移动时，它已经有效。</span><span class="sxs-lookup"><span data-stu-id="faf63-105">It is already valid when `s` is not moveable.</span></span> 

<span data-ttu-id="faf63-106">注意：在任何一种情况下，它仍需要 `unsafe` 上下文。</span><span class="sxs-lookup"><span data-stu-id="faf63-106">NOTE: in either case, it still requires `unsafe` context.</span></span> <span data-ttu-id="faf63-107">可以读取未初始化的数据或甚至超出范围。</span><span class="sxs-lookup"><span data-stu-id="faf63-107">It is possible to read uninitialized data or even out of range.</span></span> <span data-ttu-id="faf63-108">这不会更改。</span><span class="sxs-lookup"><span data-stu-id="faf63-108">That is not changing.</span></span>

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int p = s.myFixedField[5]; // indexing fixed-size array fields would be ok
    }
}
```

<span data-ttu-id="faf63-109">我在这里看到的主要 "挑战" 是如何解释规范中的 relaxation。特别是，由于以下情况仍需要固定。</span><span class="sxs-lookup"><span data-stu-id="faf63-109">The main “challenge” that I see here is how to explain the relaxation in the spec. In particular, since the following would still need pinning.</span></span> <span data-ttu-id="faf63-110">（因为 `s` 是可移动的，并将该字段显式用作指针）</span><span class="sxs-lookup"><span data-stu-id="faf63-110">(because `s` is moveable and we explicitly use the field as a pointer)</span></span>

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int* ptr = s.myFixedField; // taking a pointer explicitly still requires pinning.
        int p = ptr[5];
    }
}
```

<span data-ttu-id="faf63-111">需要固定目标的原因之一是代码生成策略的项目，我们总是转换为非托管指针，从而强制用户通过 `fixed` 语句进行固定。</span><span class="sxs-lookup"><span data-stu-id="faf63-111">One reason why we require pinning of the target when it is movable is the artifact of our code generation strategy, - we always convert to an unmanaged pointer and thus force the user to pin via `fixed` statement.</span></span> <span data-ttu-id="faf63-112">但是，在执行索引时，不需要转换为非托管。</span><span class="sxs-lookup"><span data-stu-id="faf63-112">However, conversion to unmanaged is unnecessary when doing indexing.</span></span> <span data-ttu-id="faf63-113">当使用托管指针形式的接收方时，相同的 unsafe 指针公式同样适用。</span><span class="sxs-lookup"><span data-stu-id="faf63-113">The same unsafe pointer math is equally applicable when we have the receiver in the form of a managed pointer.</span></span> <span data-ttu-id="faf63-114">如果这样做，则会管理中间引用（GC 跟踪），无需进行固定。</span><span class="sxs-lookup"><span data-stu-id="faf63-114">If we do that, then the intermediate ref is managed (GC-tracked) and the pinning is unnecessary.</span></span>

<span data-ttu-id="faf63-115">更改 https://github.com/dotnet/roslyn/pull/24966 是放宽此要求的原型 PR。</span><span class="sxs-lookup"><span data-stu-id="faf63-115">The change https://github.com/dotnet/roslyn/pull/24966 is a prototype PR that relaxes this requirement.</span></span>
