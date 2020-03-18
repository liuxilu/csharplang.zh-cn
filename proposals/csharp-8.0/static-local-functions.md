---
ms.openlocfilehash: fecd5a6c1e0f6c7a7a7beac0e4e60445281c7846
ms.sourcegitcommit: 1b488e76c2c07aafc377bc5e8a7197252c82f425
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "79484008"
---
# <a name="static-local-functions"></a><span data-ttu-id="c3317-101">静态本地函数</span><span class="sxs-lookup"><span data-stu-id="c3317-101">Static local functions</span></span>

## <a name="summary"></a><span data-ttu-id="c3317-102">总结</span><span class="sxs-lookup"><span data-stu-id="c3317-102">Summary</span></span>

<span data-ttu-id="c3317-103">支持禁止从封闭范围捕获状态的本地函数。</span><span class="sxs-lookup"><span data-stu-id="c3317-103">Support local functions that disallow capturing state from the enclosing scope.</span></span>

## <a name="motivation"></a><span data-ttu-id="c3317-104">动机</span><span class="sxs-lookup"><span data-stu-id="c3317-104">Motivation</span></span>

<span data-ttu-id="c3317-105">避免意外捕获来自封闭上下文的状态。</span><span class="sxs-lookup"><span data-stu-id="c3317-105">Avoid unintentionally capturing state from the enclosing context.</span></span>
<span data-ttu-id="c3317-106">允许在需要 `static` 方法的情况下使用本地函数。</span><span class="sxs-lookup"><span data-stu-id="c3317-106">Allow local functions to be used in scenarios where a `static` method is required.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="c3317-107">详细设计</span><span class="sxs-lookup"><span data-stu-id="c3317-107">Detailed design</span></span>

<span data-ttu-id="c3317-108">`static` 声明的本地函数无法从封闭范围捕获状态。</span><span class="sxs-lookup"><span data-stu-id="c3317-108">A local function declared `static` cannot capture state from the enclosing scope.</span></span>
<span data-ttu-id="c3317-109">因此，封闭范围中的局部变量、参数和 `this` 在 `static` 本地函数中不可用。</span><span class="sxs-lookup"><span data-stu-id="c3317-109">As a result, locals, parameters, and `this` from the enclosing scope are not available within a `static` local function.</span></span>

<span data-ttu-id="c3317-110">`static` 局部函数不能通过隐式或显式 `this` 或 `base` 引用来引用实例成员。</span><span class="sxs-lookup"><span data-stu-id="c3317-110">A `static` local function cannot reference instance members from an implicit or explicit `this` or `base` reference.</span></span>

<span data-ttu-id="c3317-111">`static` 本地函数可以引用封闭范围 `static` 成员。</span><span class="sxs-lookup"><span data-stu-id="c3317-111">A `static` local function may reference `static` members from the enclosing scope.</span></span>

<span data-ttu-id="c3317-112">`static` 本地函数可以从封闭范围中引用 `constant` 定义。</span><span class="sxs-lookup"><span data-stu-id="c3317-112">A `static` local function may reference `constant` definitions from the enclosing scope.</span></span>

<span data-ttu-id="c3317-113">`static` 的本地函数中的 `nameof()` 可以引用局部变量、参数或 `this` 或从封闭范围中 `base`。</span><span class="sxs-lookup"><span data-stu-id="c3317-113">`nameof()` in a `static` local function may reference locals, parameters, or `this` or `base` from the enclosing scope.</span></span>

<span data-ttu-id="c3317-114">封闭范围中 `private` 成员的可访问性规则与 `static` 和非`static` 本地函数相同。</span><span class="sxs-lookup"><span data-stu-id="c3317-114">Accessibility rules for `private` members in the enclosing scope are the same for `static` and non-`static` local functions.</span></span>

<span data-ttu-id="c3317-115">在元数据中作为 `static` 方法发出 `static` 本地函数定义，即使仅在委托中使用也是如此。</span><span class="sxs-lookup"><span data-stu-id="c3317-115">A `static` local function definition is emitted as a `static` method in metadata, even if only used in a delegate.</span></span>

<span data-ttu-id="c3317-116">非`static` 本地函数或 lambda 可以从封闭 `static` 本地函数捕获状态，但不能在封闭 `static` 本地函数之外捕获状态。</span><span class="sxs-lookup"><span data-stu-id="c3317-116">A non-`static` local function or lambda can capture state from an enclosing `static` local function but cannot capture state outside the enclosing `static` local function.</span></span>

<span data-ttu-id="c3317-117">不能在表达式树中调用 `static` 本地函数。</span><span class="sxs-lookup"><span data-stu-id="c3317-117">A `static` local function cannot be invoked in an expression tree.</span></span>

<span data-ttu-id="c3317-118">无论本地函数是否 `static`，都将以 `call` 而不是 `callvirt`的方式发出对本地函数的调用。</span><span class="sxs-lookup"><span data-stu-id="c3317-118">A call to a local function is emitted as `call` rather than `callvirt`, regardless of whether the local function is `static`.</span></span>

<span data-ttu-id="c3317-119">本地函数内的调用的重载解析不受 `static`本地函数的影响。</span><span class="sxs-lookup"><span data-stu-id="c3317-119">Overload resolution of a call within a local function not affected by whether the local function is `static`.</span></span>

<span data-ttu-id="c3317-120">从有效程序的本地函数中删除 `static` 修饰符不会更改程序的含义。</span><span class="sxs-lookup"><span data-stu-id="c3317-120">Removing the `static` modifier from a local function in a valid program does not change the meaning of the program.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="c3317-121">设计会议</span><span class="sxs-lookup"><span data-stu-id="c3317-121">Design meetings</span></span>

https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-09-10.md#static-local-functions
