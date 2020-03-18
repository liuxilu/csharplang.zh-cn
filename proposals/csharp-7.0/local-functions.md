---
ms.openlocfilehash: a4b0fbbc600eaf1e705ad8e6bd9fcecb44100761
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483444"
---
# <a name="local-functions"></a><span data-ttu-id="0d31e-101">本地函数</span><span class="sxs-lookup"><span data-stu-id="0d31e-101">Local functions</span></span>

<span data-ttu-id="0d31e-102">我们扩展C#了以支持块范围内的函数声明。</span><span class="sxs-lookup"><span data-stu-id="0d31e-102">We extend C# to support the declaration of functions in block scope.</span></span> <span data-ttu-id="0d31e-103">本地函数可以使用封闭范围内的（捕获）变量。</span><span class="sxs-lookup"><span data-stu-id="0d31e-103">Local functions may use (capture) variables from the enclosing scope.</span></span>

<span data-ttu-id="0d31e-104">编译器使用流分析来检测本地函数使用的变量，然后再为其赋值。</span><span class="sxs-lookup"><span data-stu-id="0d31e-104">The compiler uses flow analysis to detect which variables a local function uses before assigning it a value.</span></span> <span data-ttu-id="0d31e-105">函数的每个调用都需要明确赋值此类变量。</span><span class="sxs-lookup"><span data-stu-id="0d31e-105">Every call of the function requires such variables to be definitely assigned.</span></span> <span data-ttu-id="0d31e-106">同样，编译器会确定返回时明确赋值的变量。</span><span class="sxs-lookup"><span data-stu-id="0d31e-106">Similarly the compiler determines which variables are definitely assigned on return.</span></span> <span data-ttu-id="0d31e-107">调用本地函数后，此类变量被视为明确赋值。</span><span class="sxs-lookup"><span data-stu-id="0d31e-107">Such variables are considered definitely assigned after the local function is invoked.</span></span>

<span data-ttu-id="0d31e-108">在定义之前，可以从词法点调用本地函数。</span><span class="sxs-lookup"><span data-stu-id="0d31e-108">Local functions may be called from a lexical point before its definition.</span></span> <span data-ttu-id="0d31e-109">当本地函数声明语句不可访问时，它们不会导致警告。</span><span class="sxs-lookup"><span data-stu-id="0d31e-109">Local function declaration statements do not cause a warning when they are not reachable.</span></span>

<span data-ttu-id="0d31e-110">TODO：_编写规范_</span><span class="sxs-lookup"><span data-stu-id="0d31e-110">TODO: _WRITE SPEC_</span></span>

## <a name="syntax-grammar"></a><span data-ttu-id="0d31e-111">语法语法</span><span class="sxs-lookup"><span data-stu-id="0d31e-111">Syntax grammar</span></span>

<span data-ttu-id="0d31e-112">此语法表示为与当前规范语法的不同之处。</span><span class="sxs-lookup"><span data-stu-id="0d31e-112">This grammar is represented as a diff from the current spec grammar.</span></span>

```diff
declaration-statement
    : local-variable-declaration ';'
    | local-constant-declaration ';'
+   | local-function-declaration
    ;

+local-function-declaration
+   : local-function-header local-function-body
+   ;

+local-function-header
+   : local-function-modifiers? return-type identifier type-parameter-list?
+       ( formal-parameter-list? ) type-parameter-constraints-clauses
+   ;

+local-function-modifiers
+   : (async | unsafe)
+   ;

+local-function-body
+   : block
+   | arrow-expression-body
+   ;
```

<span data-ttu-id="0d31e-113">本地函数可能使用封闭范围中定义的变量。</span><span class="sxs-lookup"><span data-stu-id="0d31e-113">Local functions may use variables defined in the enclosing scope.</span></span> <span data-ttu-id="0d31e-114">当前实现要求在本地函数内读取的每个变量都是明确赋值的，如同在其定义点执行本地函数一样。</span><span class="sxs-lookup"><span data-stu-id="0d31e-114">The current implementation requires that every variable read inside a local function be definitely assigned, as if executing the local function at its point of definition.</span></span> <span data-ttu-id="0d31e-115">此外，还必须在任何使用点 "执行" 本地函数定义。</span><span class="sxs-lookup"><span data-stu-id="0d31e-115">Also, the local function definition must have been "executed" at any use point.</span></span>

<span data-ttu-id="0d31e-116">在试验这一点之后（例如，不可能定义两个相互递归的本地函数），我们已经修改了我们希望明确赋值的工作方式。</span><span class="sxs-lookup"><span data-stu-id="0d31e-116">After experimenting with that a bit (for example, it is not possible to define two mutually recursive local functions), we've since revised how we want the definite assignment to work.</span></span> <span data-ttu-id="0d31e-117">修订版本（尚未实现）是在本地函数中读取的所有局部变量都必须在本地函数的每个调用中明确赋值。</span><span class="sxs-lookup"><span data-stu-id="0d31e-117">The revision (not yet implemented) is that all local variables read in a local function must be definitely assigned at each invocation of the local function.</span></span> <span data-ttu-id="0d31e-118">这实际上比声音更为微妙，而且还有一组工作要使其正常工作。</span><span class="sxs-lookup"><span data-stu-id="0d31e-118">That's actually more subtle than it sounds, and there is a bunch of work remaining to make it work.</span></span> <span data-ttu-id="0d31e-119">完成后，您将能够将您的本地函数移动到它的封闭块末尾。</span><span class="sxs-lookup"><span data-stu-id="0d31e-119">Once it is done you'll be able to move your local functions to the end of its enclosing block.</span></span>

<span data-ttu-id="0d31e-120">新的明确赋值规则与推断本地函数的返回类型不兼容，因此，我们可能会删除对推断返回类型的支持。</span><span class="sxs-lookup"><span data-stu-id="0d31e-120">The new definite assignment rules are incompatible with inferring the return type of a local function, so we'll likely be removing support for inferring the return type.</span></span>

<span data-ttu-id="0d31e-121">除非将本地函数转换为委托，否则将在值类型为的帧中进行捕获。</span><span class="sxs-lookup"><span data-stu-id="0d31e-121">Unless you convert a local function to a delegate, capturing is done into frames that are value types.</span></span> <span data-ttu-id="0d31e-122">这意味着，不会因将本地函数与捕获一起使用而获得任何垃圾回收压力。</span><span class="sxs-lookup"><span data-stu-id="0d31e-122">That means you don't get any GC pressure from using local functions with capturing.</span></span>

### <a name="reachability"></a><span data-ttu-id="0d31e-123">可访问性</span><span class="sxs-lookup"><span data-stu-id="0d31e-123">Reachability</span></span>

<span data-ttu-id="0d31e-124">我们添加到规范</span><span class="sxs-lookup"><span data-stu-id="0d31e-124">We add to the spec</span></span>

> <span data-ttu-id="0d31e-125">语句 expression-bodied lambda 表达式或本地函数的主体被视为可访问。</span><span class="sxs-lookup"><span data-stu-id="0d31e-125">The body of a statement-bodied lambda expression or local function is considered reachable.</span></span>
