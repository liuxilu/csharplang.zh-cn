---
ms.openlocfilehash: a78567594d39fc4e204e12c4f2f247b8d6995c38
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483858"
---
# <a name="expression-variables-in-initializers"></a><span data-ttu-id="7752d-101">初始化表达式中的表达式变量</span><span class="sxs-lookup"><span data-stu-id="7752d-101">Expression variables in initializers</span></span>

## <a name="summary"></a><span data-ttu-id="7752d-102">总结</span><span class="sxs-lookup"><span data-stu-id="7752d-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="7752d-103">我们扩展了7中C#引入的功能，以允许包含字段初始值设定项、属性初始值设定项、ctor 初始值设定项和查询子句中的表达式变量（out 变量声明和声明模式）的表达式。</span><span class="sxs-lookup"><span data-stu-id="7752d-103">We extend the features introduced in C# 7 to permit expressions containing expression variables (out variable declarations and declaration patterns) in field initializers, property initializers, ctor-initializers, and query clauses.</span></span>

## <a name="motivation"></a><span data-ttu-id="7752d-104">动机</span><span class="sxs-lookup"><span data-stu-id="7752d-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="7752d-105">由于缺乏时间，这就完成了C#语言的几个粗略的边缘。</span><span class="sxs-lookup"><span data-stu-id="7752d-105">This completes a couple of the rough edges left in the C# language due to lack of time.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="7752d-106">详细设计</span><span class="sxs-lookup"><span data-stu-id="7752d-106">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="7752d-107">我们消除了在 ctor 初始值设定项中阻止表达式变量（out 变量声明和声明模式）声明的限制。</span><span class="sxs-lookup"><span data-stu-id="7752d-107">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a ctor-initializer.</span></span> <span data-ttu-id="7752d-108">此类声明的变量在构造函数的整个主体范围内。</span><span class="sxs-lookup"><span data-stu-id="7752d-108">Such a declared variable is in scope throughout the body of the constructor.</span></span>

<span data-ttu-id="7752d-109">我们消除了在字段或属性初始值设定项中阻止表达式变量（out 变量声明和声明模式）的声明的限制。</span><span class="sxs-lookup"><span data-stu-id="7752d-109">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a field or property initializer.</span></span> <span data-ttu-id="7752d-110">此类声明的变量在整个初始化表达式的范围内。</span><span class="sxs-lookup"><span data-stu-id="7752d-110">Such a declared variable is in scope throughout the initializing expression.</span></span>

<span data-ttu-id="7752d-111">我们消除了阻止在转换为 lambda 主体的查询表达式子句中声明表达式变量（out 变量声明和声明模式）的限制。</span><span class="sxs-lookup"><span data-stu-id="7752d-111">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a query expression clause that is translated into the body of a lambda.</span></span> <span data-ttu-id="7752d-112">此类声明的变量在整个查询子句表达式的范围内。</span><span class="sxs-lookup"><span data-stu-id="7752d-112">Such a declared variable is in scope throughout that expression of the query clause.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="7752d-113">缺点</span><span class="sxs-lookup"><span data-stu-id="7752d-113">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="7752d-114">无。</span><span class="sxs-lookup"><span data-stu-id="7752d-114">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="7752d-115">备选项</span><span class="sxs-lookup"><span data-stu-id="7752d-115">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="7752d-116">在这些上下文中声明的表达式变量的适当范围并不明显，值得进一步进行 LDM 讨论。</span><span class="sxs-lookup"><span data-stu-id="7752d-116">The appropriate scope for expression variables declared in these contexts is not obvious, and deserves further LDM discussion.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="7752d-117">未解决的问题</span><span class="sxs-lookup"><span data-stu-id="7752d-117">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="7752d-118">[] 这些变量的适当作用域是什么？</span><span class="sxs-lookup"><span data-stu-id="7752d-118">[ ] What is the appropriate scope for these variables?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="7752d-119">设计会议</span><span class="sxs-lookup"><span data-stu-id="7752d-119">Design meetings</span></span>

<span data-ttu-id="7752d-120">无。</span><span class="sxs-lookup"><span data-stu-id="7752d-120">None.</span></span>
