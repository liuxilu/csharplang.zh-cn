---
ms.openlocfilehash: 4f66c0f60d05ed6509a1d0843318a71d1b36c351
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483672"
---
# <a name="pattern-matching-with-generics"></a><span data-ttu-id="cd955-101">采用泛型的模式匹配</span><span class="sxs-lookup"><span data-stu-id="cd955-101">pattern-matching with generics</span></span>

* <span data-ttu-id="cd955-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="cd955-102">[x] Proposed</span></span>
* <span data-ttu-id="cd955-103">[] 原型：</span><span class="sxs-lookup"><span data-stu-id="cd955-103">[ ] Prototype:</span></span>
* <span data-ttu-id="cd955-104">[] 实现：</span><span class="sxs-lookup"><span data-stu-id="cd955-104">[ ] Implementation:</span></span>
* <span data-ttu-id="cd955-105">[] 规范：</span><span class="sxs-lookup"><span data-stu-id="cd955-105">[ ] Specification:</span></span>

## <a name="summary"></a><span data-ttu-id="cd955-106">总结</span><span class="sxs-lookup"><span data-stu-id="cd955-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="cd955-107">对于[ C#现有 as 运算符](../../spec/expressions.md#the-as-operator)的规范，如果这两个类型都是开放类型，则不允许在操作数的类型与指定类型之间进行转换。</span><span class="sxs-lookup"><span data-stu-id="cd955-107">The specification for the [existing C# as operator](../../spec/expressions.md#the-as-operator) permits there to be no conversion between the type of the operand and the specified type when either is an open type.</span></span> <span data-ttu-id="cd955-108">但在 7 C#中，`Type identifier` 模式要求输入类型与给定类型之间存在转换。</span><span class="sxs-lookup"><span data-stu-id="cd955-108">However, in C# 7 the `Type identifier` pattern requires there be a conversion between the type of the input and the given type.</span></span>

<span data-ttu-id="cd955-109">建议放宽这一点，并更改 `expression is Type identifier`，除了允许在C# 7 中使用条件以外，还可以在允许 `expression as Type` 时允许。</span><span class="sxs-lookup"><span data-stu-id="cd955-109">We propose to relax this and change `expression is Type identifier`, in addition to being permitted in the conditions when it is permitted in C# 7, to also be permitted when `expression as Type` would be allowed.</span></span> <span data-ttu-id="cd955-110">具体而言，这种情况下，表达式或指定类型的类型是开放类型。</span><span class="sxs-lookup"><span data-stu-id="cd955-110">Specifically, the new cases are cases where the type of the expression or the specified type is an open type.</span></span> 

## <a name="motivation"></a><span data-ttu-id="cd955-111">动机</span><span class="sxs-lookup"><span data-stu-id="cd955-111">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="cd955-112">当前允许模式匹配 "明显" 的情况当前无法编译。</span><span class="sxs-lookup"><span data-stu-id="cd955-112">Cases where pattern-matching should "obviously" be permitted currently fail to compile.</span></span> <span data-ttu-id="cd955-113">请参阅 https://github.com/dotnet/roslyn/issues/16195。</span><span class="sxs-lookup"><span data-stu-id="cd955-113">See, for example, https://github.com/dotnet/roslyn/issues/16195.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="cd955-114">详细设计</span><span class="sxs-lookup"><span data-stu-id="cd955-114">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="cd955-115">我们更改了模式匹配规范中的段落（建议的加法以粗体显示）：</span><span class="sxs-lookup"><span data-stu-id="cd955-115">We change the paragraph in the pattern-matching specification (the proposed addition is shown in bold):</span></span>

> <span data-ttu-id="cd955-116">左侧和给定类型的静态类型的某些组合被视为不兼容并导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="cd955-116">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="cd955-117">如果存在标识转换、隐式引用转换、装箱转换、显式引用转换或从 `E` 到 `T`的取消装箱转换，**或者 `E` 或 `T` 是开放类型**，则将静态类型 `E` 的值视为与类型 `T`*模式兼容*。</span><span class="sxs-lookup"><span data-stu-id="cd955-117">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`**, or if either `E` or `T` is an open type**.</span></span> <span data-ttu-id="cd955-118">如果类型 `E` 的表达式不与它所匹配的类型模式中的类型兼容，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="cd955-118">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="cd955-119">缺点</span><span class="sxs-lookup"><span data-stu-id="cd955-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="cd955-120">无。</span><span class="sxs-lookup"><span data-stu-id="cd955-120">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="cd955-121">备选项</span><span class="sxs-lookup"><span data-stu-id="cd955-121">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="cd955-122">无。</span><span class="sxs-lookup"><span data-stu-id="cd955-122">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="cd955-123">未解决的问题</span><span class="sxs-lookup"><span data-stu-id="cd955-123">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="cd955-124">无。</span><span class="sxs-lookup"><span data-stu-id="cd955-124">None.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="cd955-125">设计会议</span><span class="sxs-lookup"><span data-stu-id="cd955-125">Design meetings</span></span>

<span data-ttu-id="cd955-126">LDM 认为这是一个问题，认为这是一个 bug 修复级别的更改。</span><span class="sxs-lookup"><span data-stu-id="cd955-126">LDM considered this question and felt it was a bug-fix level change.</span></span> <span data-ttu-id="cd955-127">我们将其视为一种独立的语言功能，因为只要在语言发布后进行更改，就会引入不兼容性。</span><span class="sxs-lookup"><span data-stu-id="cd955-127">We are treating it as a separate language feature because just making the change after the language has been released would introduce a forward incompatibility.</span></span> <span data-ttu-id="cd955-128">使用建议的更改需要程序员指定语言版本7.1。</span><span class="sxs-lookup"><span data-stu-id="cd955-128">Using the proposed change requires that the programmer specify language version 7.1.</span></span>
