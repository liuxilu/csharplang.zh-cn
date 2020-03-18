---
ms.openlocfilehash: 9647fff40a1e45bef917f140612ae4e91abea958
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483558"
---
# <a name="lambda-attributes"></a><span data-ttu-id="7cbc5-101">Lambda 特性</span><span class="sxs-lookup"><span data-stu-id="7cbc5-101">Lambda Attributes</span></span>

* <span data-ttu-id="7cbc5-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="7cbc5-102">[x] Proposed</span></span>
* <span data-ttu-id="7cbc5-103">[] 原型</span><span class="sxs-lookup"><span data-stu-id="7cbc5-103">[ ] Prototype</span></span>
* <span data-ttu-id="7cbc5-104">[] 实现</span><span class="sxs-lookup"><span data-stu-id="7cbc5-104">[ ] Implementation</span></span>
* <span data-ttu-id="7cbc5-105">[] 规范</span><span class="sxs-lookup"><span data-stu-id="7cbc5-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="7cbc5-106">总结</span><span class="sxs-lookup"><span data-stu-id="7cbc5-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="7cbc5-107">允许将特性应用于 lambda （和匿名方法）以及 lambda/匿名方法参数，因为它们可以在常规方法上。</span><span class="sxs-lookup"><span data-stu-id="7cbc5-107">Allow attributes to be applied to lambdas (and anonymous methods) and to lambda / anonymous method parameters, as they can be on regular methods.</span></span>

## <a name="motivation"></a><span data-ttu-id="7cbc5-108">动机</span><span class="sxs-lookup"><span data-stu-id="7cbc5-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="7cbc5-109">两个主要动机：</span><span class="sxs-lookup"><span data-stu-id="7cbc5-109">Two primary motivations:</span></span>

1. <span data-ttu-id="7cbc5-110">在编译时提供分析器的元数据可见性。</span><span class="sxs-lookup"><span data-stu-id="7cbc5-110">To provide metadata visible to analyzers at compile-time.</span></span>
2. <span data-ttu-id="7cbc5-111">若为，则在运行时提供对反射和工具的可见性。</span><span class="sxs-lookup"><span data-stu-id="7cbc5-111">To provide metadata visible to reflection and tooling at run-time.</span></span>

<span data-ttu-id="7cbc5-112">作为（1）的一个示例：对于性能敏感的代码，能够使分析器在为关闭状态的 lambda 分配闭包和委托时能够标记。</span><span class="sxs-lookup"><span data-stu-id="7cbc5-112">As an example of (1): For performance-sensitive code, it is helpful to be able to have an analyzer that flags when closures and delegates are being allocated for lambdas that close over state.</span></span>  <span data-ttu-id="7cbc5-113">通常，此类代码的开发人员将不再能够避免捕获任何状态，以便编译器可以为该方法生成静态方法和可缓存的委托，或者开发人员将确保只 `this`关闭的唯一状态，从而至少允许编译器避免分配闭包对象。</span><span class="sxs-lookup"><span data-stu-id="7cbc5-113">Often a developer of such code will go out of his or her way to avoid capturing any state, so that the compiler can generate a static method and a cacheable delegate for the method, or the developer will ensure that the only state being closed over is `this`, allowing the compiler at least to avoid allocating a closure object.</span></span>  <span data-ttu-id="7cbc5-114">但是，如果没有语言支持来限制可能捕获的内容，则很容易意外关闭状态。</span><span class="sxs-lookup"><span data-stu-id="7cbc5-114">But, without language support for limiting what may be captured, it is all too easy to accidentally close over state.</span></span>  <span data-ttu-id="7cbc5-115">如果开发人员可以使用特性批注 lambda 以指示允许关闭的状态，这会很有用，例如：</span><span class="sxs-lookup"><span data-stu-id="7cbc5-115">It would be valuable if a developer could annotate lambdas with attributes to indicate what state they're allowed to close over, for example:</span></span>

```csharp
[CaptureNone] // can't close over any instance state
[CaptureThis] // can only capture `this` and no other instance state
[CaptureAny] // can close over any instance state
```

<span data-ttu-id="7cbc5-116">当错误捕获状态时，可以编写分析器来标记，例如：</span><span class="sxs-lookup"><span data-stu-id="7cbc5-116">Then an analyzer can be written to flag when state is captured incorrectly, for example:</span></span>

```csharp
var results = collection.Select([CaptureNone](i) => Process(item)); // Analyzer error: [CaptureNone] lambdas captures `this`
...
private U Process(T item) { ... }
```

## <a name="detailed-design"></a><span data-ttu-id="7cbc5-117">详细设计</span><span class="sxs-lookup"><span data-stu-id="7cbc5-117">Detailed design</span></span>
[design]: #detailed-design

- <span data-ttu-id="7cbc5-118">使用与普通方法相同的特性语法，特性可应用于 lambda 或匿名方法的开头，例如：</span><span class="sxs-lookup"><span data-stu-id="7cbc5-118">Using the same attribute syntax as on normal methods, attributes may be applied at the beginning of a lambda or anonymous method, for example:</span></span>

```csharp
[SomeAttribute(...)] () => { ... }
[SomeAttribute(...)] delegate (int i) { ... }
```

- <span data-ttu-id="7cbc5-119">若要避免对某个特性是应用于 lambda 方法还是应用于其中一个参数的歧义，只能在将括号用于任何参数时使用特性，例如：</span><span class="sxs-lookup"><span data-stu-id="7cbc5-119">To avoid ambiguity as to whether an attribute applies to the lambda method or to one of the arguments, attributes may only be used when parens are used around any arguments, for example:</span></span>

```csharp
[SomeAttribute] i => { ... } // ERROR
[SomeAttribute] (i) => { ... } // Ok
[SomeAttribute] (int i) => { ... } // Ok
```

- <span data-ttu-id="7cbc5-120">对于匿名方法，在 `delegate` 关键字之前，无需将属性应用于方法，例如：</span><span class="sxs-lookup"><span data-stu-id="7cbc5-120">With anonymous methods, parens are not needed in order to apply an attribute to the method before the `delegate` keyword, for example:</span></span>

```csharp
[SomeAttribute] delegate { ... } // Ok
[SomeAttribute] delegate (int i) => { ... } // Ok
```

- <span data-ttu-id="7cbc5-121">可以应用多个属性，无论是通过标准的逗号分隔语法，还是通过完整属性语法，例如：</span><span class="sxs-lookup"><span data-stu-id="7cbc5-121">Multiple attributes may be applied, either via standard comma-delimited syntax or via full-attribute syntax, for example:</span></span>

```csharp
[FirstAttribute, SecondAttribute] (i) => { ... } // Ok
[FirstAttribute] [SecondAttribute] (i) => { .... } // Ok
```

- <span data-ttu-id="7cbc5-122">特性可应用于匿名方法或 lambda 的参数，但仅当使用括号时，才可用于任何参数，例如：</span><span class="sxs-lookup"><span data-stu-id="7cbc5-122">Attributes may be applied to the parameters to an anonymous method or lambda, but only when parens are used around any arguments, for example:</span></span>

```csharp
[SomeAttribute] i => { ... } // ERROR
([SomeAttribute] i) => { .... } // Ok
([SomeAttribute] int i) => { ... } // Ok
([SomeAttribute] i, [SomeOtherAttribute] j) => { ... } // Ok
```

- <span data-ttu-id="7cbc5-123">使用逗号分隔或完整属性语法，可以将多个属性应用于匿名方法或 lambda 的参数，例如：</span><span class="sxs-lookup"><span data-stu-id="7cbc5-123">Multiple attributes may be applied to the parameters of an anonymous method or lambda, using either the comma-delimited or full-attribute syntax, for example:</span></span>

```csharp
([FirstAttribute, SecondAttribute] i) => { ... } // Ok
([FirstAttribute] [SecondAttribute] i) => { ... } // Ok
```

- <span data-ttu-id="7cbc5-124">`return`目标特性还可用于 lambda，例如：</span><span class="sxs-lookup"><span data-stu-id="7cbc5-124">`return`-targeted attributes may also be used on lambdas, for example:</span></span>

```csharp
([return: SomeAttribute] (i) => { ... }) // Ok
```

- <span data-ttu-id="7cbc5-125">编译器会将属性输出到生成的方法，将参数输出到这些方法，就像对任何其他方法一样。</span><span class="sxs-lookup"><span data-stu-id="7cbc5-125">The compiler outputs the attributes onto the generated method and arguments to those methods as it would for any other method.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="7cbc5-126">缺点</span><span class="sxs-lookup"><span data-stu-id="7cbc5-126">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="7cbc5-127">不适用</span><span class="sxs-lookup"><span data-stu-id="7cbc5-127">n/a</span></span>

## <a name="alternatives"></a><span data-ttu-id="7cbc5-128">备选项</span><span class="sxs-lookup"><span data-stu-id="7cbc5-128">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="7cbc5-129">不适用</span><span class="sxs-lookup"><span data-stu-id="7cbc5-129">n/a</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="7cbc5-130">未解决的问题</span><span class="sxs-lookup"><span data-stu-id="7cbc5-130">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="7cbc5-131">不适用</span><span class="sxs-lookup"><span data-stu-id="7cbc5-131">n/a</span></span>

## <a name="design-meetings"></a><span data-ttu-id="7cbc5-132">设计会议</span><span class="sxs-lookup"><span data-stu-id="7cbc5-132">Design meetings</span></span>

<span data-ttu-id="7cbc5-133">不适用</span><span class="sxs-lookup"><span data-stu-id="7cbc5-133">n/a</span></span>