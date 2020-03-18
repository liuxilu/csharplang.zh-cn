---
ms.openlocfilehash: 392d932459ff0a4cb0d6d32c1606f73f9b913c68
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483456"
---
# <a name="covariant-return-types"></a><span data-ttu-id="ddd5d-101">协变返回类型</span><span class="sxs-lookup"><span data-stu-id="ddd5d-101">covariant return types</span></span>

* <span data-ttu-id="ddd5d-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="ddd5d-102">[x] Proposed</span></span>
* <span data-ttu-id="ddd5d-103">[] 原型：未启动</span><span class="sxs-lookup"><span data-stu-id="ddd5d-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="ddd5d-104">[] 实现：未启动</span><span class="sxs-lookup"><span data-stu-id="ddd5d-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="ddd5d-105">[] 规范：未启动</span><span class="sxs-lookup"><span data-stu-id="ddd5d-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="ddd5d-106">总结</span><span class="sxs-lookup"><span data-stu-id="ddd5d-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="ddd5d-107">支持_协变返回类型_。</span><span class="sxs-lookup"><span data-stu-id="ddd5d-107">Support _covariant return types_.</span></span> <span data-ttu-id="ddd5d-108">具体而言，允许重写方法的派生引用类型比它重写的方法更多。</span><span class="sxs-lookup"><span data-stu-id="ddd5d-108">Specifically, allow an overriding method to have a more derived reference type than the method it overrides.</span></span>

## <a name="motivation"></a><span data-ttu-id="ddd5d-109">动机</span><span class="sxs-lookup"><span data-stu-id="ddd5d-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="ddd5d-110">代码中的一种常见模式是，必须使用不同的方法名称来解决语言约束，而重写必须返回与重写的方法相同的类型。</span><span class="sxs-lookup"><span data-stu-id="ddd5d-110">It is a common pattern in code that different method names have to be invented to work around the language constraint that overrides must return the same type as the overridden method.</span></span> <span data-ttu-id="ddd5d-111">请参阅下面的 Roslyn 代码库中的示例。</span><span class="sxs-lookup"><span data-stu-id="ddd5d-111">See below for an example from the Roslyn code base.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="ddd5d-112">详细设计</span><span class="sxs-lookup"><span data-stu-id="ddd5d-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="ddd5d-113">支持_协变返回类型_。</span><span class="sxs-lookup"><span data-stu-id="ddd5d-113">Support _covariant return types_.</span></span> <span data-ttu-id="ddd5d-114">具体而言，允许重写方法的派生引用类型比它重写的方法更多。</span><span class="sxs-lookup"><span data-stu-id="ddd5d-114">Specifically, allow an overriding method to have a more derived reference type than the method it overrides.</span></span> <span data-ttu-id="ddd5d-115">这将应用到方法和属性，并且在类和接口中受支持。</span><span class="sxs-lookup"><span data-stu-id="ddd5d-115">This would apply to methods and properties, and be supported in classes and interfaces.</span></span>

<span data-ttu-id="ddd5d-116">这在工厂模式下非常有用。</span><span class="sxs-lookup"><span data-stu-id="ddd5d-116">This would be useful in the factory pattern.</span></span> <span data-ttu-id="ddd5d-117">例如，在 Roslyn 代码库中，我们将</span><span class="sxs-lookup"><span data-stu-id="ddd5d-117">For example, in the Roslyn code base we would have</span></span>

``` cs
class Compilation ...
{
    virtual Compilation WithOptions(Options options)...
}
```

``` cs
class CSharpCompilation : Compilation
{
    override CSharpCompilation WithOptions(Options options)...
}
```

<span data-ttu-id="ddd5d-118">此方法的实现是为了使编译器将重写方法作为 "新" 虚拟方法发出，该方法隐藏了基类方法，同时还提供了一个使用派生类方法调用来实现基类方法的_bridge 方法_。</span><span class="sxs-lookup"><span data-stu-id="ddd5d-118">The implementation of this would be for the compiler to emit the overriding method as a "new" virtual method that hides the base class method, along with a _bridge method_ that implements the base class method with a call to the derived class method.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="ddd5d-119">缺点</span><span class="sxs-lookup"><span data-stu-id="ddd5d-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="ddd5d-120">[] 每个语言更改都必须为其本身付费。</span><span class="sxs-lookup"><span data-stu-id="ddd5d-120">[ ] Every language change must pay for itself.</span></span>
- <span data-ttu-id="ddd5d-121">[] 我们应该确保性能合理，甚至在深层继承层次结构中也是如此</span><span class="sxs-lookup"><span data-stu-id="ddd5d-121">[ ] We should ensure that the performance is reasonable, even in the case of deep inheritance hierarchies</span></span>
- <span data-ttu-id="ddd5d-122">[] 我们应该确保翻译策略的项目不会影响语言语义，即使在使用旧编译器的新 IL 时也是如此。</span><span class="sxs-lookup"><span data-stu-id="ddd5d-122">[ ] We should ensure that artifacts of the translation strategy do not affect language semantics, even when consuming new IL from old compilers.</span></span>

## <a name="alternatives"></a><span data-ttu-id="ddd5d-123">备选项</span><span class="sxs-lookup"><span data-stu-id="ddd5d-123">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="ddd5d-124">我们可以略微放宽语言规则，在源中，</span><span class="sxs-lookup"><span data-stu-id="ddd5d-124">We could relax the language rules slightly to allow, in source,</span></span>

```csharp
abstract class Cloneable
{
    public abstract Cloneable Clone();
}

class Digit : Cloneable
{
    public override Cloneable Clone()
    {
        return this.Clone();
    }

    public new Digit Clone() // Error: 'Digit' already defines a member called 'Clone' with the same parameter types
    {
        return this;
    }
}
```

## <a name="unresolved-questions"></a><span data-ttu-id="ddd5d-125">未解决的问题</span><span class="sxs-lookup"><span data-stu-id="ddd5d-125">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="ddd5d-126">[] 编译为使用此功能的 Api 将在较旧版本的语言中工作？</span><span class="sxs-lookup"><span data-stu-id="ddd5d-126">[ ] How will APIs that have been compiled to use this feature work in older versions of the language?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="ddd5d-127">设计会议</span><span class="sxs-lookup"><span data-stu-id="ddd5d-127">Design meetings</span></span>

<span data-ttu-id="ddd5d-128">尚无。</span><span class="sxs-lookup"><span data-stu-id="ddd5d-128">None yet.</span></span> <span data-ttu-id="ddd5d-129">在 <https://github.com/dotnet/roslyn/issues/357>有一些讨论。</span><span class="sxs-lookup"><span data-stu-id="ddd5d-129">There has been some discussion at <https://github.com/dotnet/roslyn/issues/357>.</span></span>