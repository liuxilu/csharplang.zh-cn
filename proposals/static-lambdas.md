---
ms.openlocfilehash: 6f05bbc1365e59d737103b586db9d4a242c6d306
ms.sourcegitcommit: e9afb74cc1abd56db93b4b50bd5e6765e27c1c5d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2019
ms.locfileid: "79483996"
---
# <a name="static-lambdas"></a><span data-ttu-id="376a4-101">静态 lambda</span><span class="sxs-lookup"><span data-stu-id="376a4-101">Static lambdas</span></span>

## <a name="summary"></a><span data-ttu-id="376a4-102">总结</span><span class="sxs-lookup"><span data-stu-id="376a4-102">Summary</span></span>

<span data-ttu-id="376a4-103">支持禁止从封闭范围捕获状态的 lambda。</span><span class="sxs-lookup"><span data-stu-id="376a4-103">Support lambdas that disallow capturing state from the enclosing scope.</span></span>

## <a name="motivation"></a><span data-ttu-id="376a4-104">动机</span><span class="sxs-lookup"><span data-stu-id="376a4-104">Motivation</span></span>

<span data-ttu-id="376a4-105">避免意外捕获来自封闭上下文的状态。</span><span class="sxs-lookup"><span data-stu-id="376a4-105">Avoid unintentionally capturing state from the enclosing context.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="376a4-106">详细设计</span><span class="sxs-lookup"><span data-stu-id="376a4-106">Detailed design</span></span>

<span data-ttu-id="376a4-107">具有 `static` 的 lambda 无法从封闭范围中捕获状态。</span><span class="sxs-lookup"><span data-stu-id="376a4-107">A lambdas with `static` cannot capture state from the enclosing scope.</span></span>
<span data-ttu-id="376a4-108">因此，封闭范围中的局部变量、参数和 `this` 在 `static` lambda 中不可用。</span><span class="sxs-lookup"><span data-stu-id="376a4-108">As a result, locals, parameters, and `this` from the enclosing scope are not available within a `static` lambda.</span></span>

<span data-ttu-id="376a4-109">`static` lambda 无法通过隐式或显式 `this` 或 `base` 引用引用实例成员。</span><span class="sxs-lookup"><span data-stu-id="376a4-109">A `static` lambda cannot reference instance members from an implicit or explicit `this` or `base` reference.</span></span>

<span data-ttu-id="376a4-110">`static` lambda 可以引用封闭范围 `static` 成员。</span><span class="sxs-lookup"><span data-stu-id="376a4-110">A `static` lambda may reference `static` members from the enclosing scope.</span></span>

<span data-ttu-id="376a4-111">`static` lambda 可以引用来自封闭范围 `constant` 定义。</span><span class="sxs-lookup"><span data-stu-id="376a4-111">A `static` lambda may reference `constant` definitions from the enclosing scope.</span></span>

<span data-ttu-id="376a4-112">`static` lambda 中的 `nameof()` 可以引用局部变量、参数或 `this` 或从封闭范围中 `base`。</span><span class="sxs-lookup"><span data-stu-id="376a4-112">`nameof()` in a `static` lambda may reference locals, parameters, or `this` or `base` from the enclosing scope.</span></span>

<span data-ttu-id="376a4-113">对于 `static` 和非`static` lambda，封闭范围中 `private` 成员的可访问性规则是相同的。</span><span class="sxs-lookup"><span data-stu-id="376a4-113">Accessibility rules for `private` members in the enclosing scope are the same for `static` and non-`static` lambdas.</span></span>

<span data-ttu-id="376a4-114">不保证是否在元数据中将 `static` lambda 定义作为 `static` 方法发出。</span><span class="sxs-lookup"><span data-stu-id="376a4-114">No guarantee is made as to whether a `static` lambda definition is emitted as a `static` method in metadata.</span></span> <span data-ttu-id="376a4-115">这留给了编译器实现进行优化。</span><span class="sxs-lookup"><span data-stu-id="376a4-115">This is left up to the compiler implementation to optimize.</span></span>

<span data-ttu-id="376a4-116">非`static` 本地函数或 lambda 可以从封闭的 `static` lambda 捕获状态，但不能在封闭 `static` lambda 之外捕获状态。</span><span class="sxs-lookup"><span data-stu-id="376a4-116">A non-`static` local function or lambda can capture state from an enclosing `static` lambda but cannot capture state outside the enclosing `static` lambda.</span></span>

<span data-ttu-id="376a4-117">`static` lambda 可以在表达式树中使用。</span><span class="sxs-lookup"><span data-stu-id="376a4-117">A `static` lambda can be used in an expression tree.</span></span>

<span data-ttu-id="376a4-118">从有效程序的 lambda 中删除 `static` 修饰符不会更改程序的含义。</span><span class="sxs-lookup"><span data-stu-id="376a4-118">Removing the `static` modifier from a lambda in a valid program does not change the meaning of the program.</span></span>
