---
ms.openlocfilehash: 2e054c629f71ae37b112300905c3106f9ce977e8
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483636"
---
# <a name="auto-implemented-property-field-targeted-attributes"></a><span data-ttu-id="93796-101">自动实现的属性字段目标特性</span><span class="sxs-lookup"><span data-stu-id="93796-101">Auto-Implemented Property Field-Targeted Attributes</span></span>

## <a name="summary"></a><span data-ttu-id="93796-102">总结</span><span class="sxs-lookup"><span data-stu-id="93796-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="93796-103">此功能旨在允许开发人员将属性直接应用到自动实现属性的支持字段。</span><span class="sxs-lookup"><span data-stu-id="93796-103">This feature intends to allow developers to apply attributes directly to the backing fields of auto-implemented properties.</span></span>

## <a name="motivation"></a><span data-ttu-id="93796-104">动机</span><span class="sxs-lookup"><span data-stu-id="93796-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="93796-105">目前不能将属性应用到自动实现的属性的支持字段。</span><span class="sxs-lookup"><span data-stu-id="93796-105">Currently it is not possible to apply attributes to the backing fields of auto-implemented properties.</span></span>  <span data-ttu-id="93796-106">在这种情况下，开发人员必须使用面向字段的属性，因此，它们会强制手动声明字段并使用更详细的属性语法。</span><span class="sxs-lookup"><span data-stu-id="93796-106">In those cases where the developer must use a field-targeting attribute they are forced to declare the field manually and use the more verbose property syntax.</span></span>  <span data-ttu-id="93796-107">如果在C#事件的生成支持字段上始终支持字段目标属性，则有必要将相同的功能扩展到其属性 kin。</span><span class="sxs-lookup"><span data-stu-id="93796-107">Given that C# has always supported field-targeted attributes on the generated backing field for events it makes sense to extend the same functionality to their property kin.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="93796-108">详细设计</span><span class="sxs-lookup"><span data-stu-id="93796-108">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="93796-109">简而言之，以下内容是合法C#的，不会产生警告：</span><span class="sxs-lookup"><span data-stu-id="93796-109">In short, the following would be legal C# and not produce a warning:</span></span>

```csharp
[Serializable]
public class Foo 
{
    [field: NonSerialized]
    public string MySecret { get; set; }
}
```

<span data-ttu-id="93796-110">这会导致将字段目标属性应用于编译器生成的支持字段：</span><span class="sxs-lookup"><span data-stu-id="93796-110">This would result in the field-targeted attributes being applied to the compiler-generated backing field:</span></span>

```csharp
[Serializable]
public class Foo 
{
    [NonSerialized]
    private string _mySecretBackingField;
    
    public string MySecret
    {
        get { return _mySecretBackingField; }
        set { _mySecretBackingField = value; }
    }
}
```

<span data-ttu-id="93796-111">如前文所述，这会引入带有C# 1.0 的事件语法的奇偶校验，因为以下内容已合法并按预期方式工作：</span><span class="sxs-lookup"><span data-stu-id="93796-111">As mentioned, this brings parity with event syntax from C# 1.0 as the following is already legal and behaves as expected:</span></span>

```csharp
[Serializable]
public class Foo
{
    [field: NonSerialized]
    public event EventHandler MyEvent;
}
```

## <a name="drawbacks"></a><span data-ttu-id="93796-112">缺点</span><span class="sxs-lookup"><span data-stu-id="93796-112">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="93796-113">实现此更改有两个潜在缺点：</span><span class="sxs-lookup"><span data-stu-id="93796-113">There are two potential drawbacks to implementing this change:</span></span>

1. <span data-ttu-id="93796-114">尝试将特性应用于自动实现属性的字段将产生编译器警告，指出该块中的特性将被忽略。</span><span class="sxs-lookup"><span data-stu-id="93796-114">Attempting to apply an attribute to the field of an auto-implemented property produces a compiler warning that the attributes in that block will be ignored.</span></span>  <span data-ttu-id="93796-115">如果更改了编译器以支持这些属性，则这些属性将应用于后续重新编译时，这可能会在运行时更改程序的行为。</span><span class="sxs-lookup"><span data-stu-id="93796-115">If the compiler were changed to support those attributes they would be applied to the backing field on a subsequent recompilation which could alter the behavior of the program at runtime.</span></span>
1. <span data-ttu-id="93796-116">尝试将属性的 AttributeUsage 目标应用到自动实现属性的字段时，编译器当前不会验证这些属性的目标。</span><span class="sxs-lookup"><span data-stu-id="93796-116">The compiler does not currently validate the AttributeUsage targets of the attributes when attempting to apply them to the field of the auto-implemented property.</span></span>  <span data-ttu-id="93796-117">如果编译器已更改为支持字段目标属性，但无法将相关属性应用于字段，则编译器将发出错误而不是警告，从而中断生成。</span><span class="sxs-lookup"><span data-stu-id="93796-117">If the compiler were changed to support field-targeted attributes and the attribute in question cannot be applied to a field the compiler would emit an error instead of a warning, breaking the build.</span></span>

## <a name="alternatives"></a><span data-ttu-id="93796-118">备选项</span><span class="sxs-lookup"><span data-stu-id="93796-118">Alternatives</span></span>
[alternatives]: #alternatives

## <a name="unresolved-questions"></a><span data-ttu-id="93796-119">未解决的问题</span><span class="sxs-lookup"><span data-stu-id="93796-119">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="93796-120">设计会议</span><span class="sxs-lookup"><span data-stu-id="93796-120">Design meetings</span></span>
