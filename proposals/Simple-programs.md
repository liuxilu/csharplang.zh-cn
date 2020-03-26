---
ms.openlocfilehash: 54ae4ffabde6dca49b7e6bfb626d65837eabc8f5
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281939"
---
# <a name="simple-programs"></a><span data-ttu-id="d5cd5-101">简单程序</span><span class="sxs-lookup"><span data-stu-id="d5cd5-101">Simple programs</span></span>

* <span data-ttu-id="d5cd5-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="d5cd5-102">[x] Proposed</span></span>
* <span data-ttu-id="d5cd5-103">[x] 原型：已启动</span><span class="sxs-lookup"><span data-stu-id="d5cd5-103">[x] Prototype: Started</span></span>
* <span data-ttu-id="d5cd5-104">[] 实现：未启动</span><span class="sxs-lookup"><span data-stu-id="d5cd5-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="d5cd5-105">[] 规范：未启动</span><span class="sxs-lookup"><span data-stu-id="d5cd5-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="d5cd5-106">总结</span><span class="sxs-lookup"><span data-stu-id="d5cd5-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="d5cd5-107">允许一系列*语句*在*compilation_unit*的*namespace_member_declaration*（即源文件）之前立即发生。</span><span class="sxs-lookup"><span data-stu-id="d5cd5-107">Allow a sequence of *statements* to occur right before the *namespace_member_declaration*s of a *compilation_unit* (i.e. source file).</span></span>

<span data-ttu-id="d5cd5-108">语义是，如果存在这样一系列*语句*，则将发出下面的类型声明，取模实际类型名称和方法名称：</span><span class="sxs-lookup"><span data-stu-id="d5cd5-108">The semantics are that if such a sequence of *statements* is present, the following type declaration, modulo the actual type name and the method name, would be emitted:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

<span data-ttu-id="d5cd5-109">另请参阅 https://github.com/dotnet/csharplang/issues/3117。</span><span class="sxs-lookup"><span data-stu-id="d5cd5-109">See also https://github.com/dotnet/csharplang/issues/3117.</span></span>

## <a name="motivation"></a><span data-ttu-id="d5cd5-110">动机</span><span class="sxs-lookup"><span data-stu-id="d5cd5-110">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="d5cd5-111">由于需要显式 `Main` 方法，因此，甚至最简单的程序都有一定程度的样板。</span><span class="sxs-lookup"><span data-stu-id="d5cd5-111">There's a certain amount of boilerplate surrounding even the simplest of programs, because of the need for an explicit `Main` method.</span></span> <span data-ttu-id="d5cd5-112">这似乎会使语言学习和编程清晰。</span><span class="sxs-lookup"><span data-stu-id="d5cd5-112">This seems to get in the way of language learning and program clarity.</span></span> <span data-ttu-id="d5cd5-113">因此，此功能的主要目标是使C#程序在没有必要的样本上无必要的样本，以方便学习和编写代码。</span><span class="sxs-lookup"><span data-stu-id="d5cd5-113">The primary goal of the feature therefore is to allow C# programs without unnecessary boilerplate around them, for the sake of learners and the clarity of code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="d5cd5-114">详细设计</span><span class="sxs-lookup"><span data-stu-id="d5cd5-114">Detailed design</span></span>
[design]: #detailed-design

### <a name="syntax"></a><span data-ttu-id="d5cd5-115">语法</span><span class="sxs-lookup"><span data-stu-id="d5cd5-115">Syntax</span></span>

<span data-ttu-id="d5cd5-116">唯一的附加语法允许编译单元中的一系列*语句*，刚好在*namespace_member_declaration*之前：</span><span class="sxs-lookup"><span data-stu-id="d5cd5-116">The only additional syntax is allowing a sequence of *statement*s in a compilation unit, just before the *namespace_member_declaration*s:</span></span>

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

<span data-ttu-id="d5cd5-117">只允许一个*compilation_unit*具有*语句*。</span><span class="sxs-lookup"><span data-stu-id="d5cd5-117">Only one *compilation_unit* is allowed to have *statement*s.</span></span> 

<span data-ttu-id="d5cd5-118">示例：</span><span class="sxs-lookup"><span data-stu-id="d5cd5-118">Example:</span></span>

``` c#
if (System.Environment.CommandLine.Length == 0
    || !int.TryParse(System.Environment.CommandLine, out int n)
    || n < 0) return;
Console.WriteLine(Fib(n).curr);

(int curr, int prev) Fib(int i)
{
    if (i == 0) return (1, 0);
    var (curr, prev) = Fib(i - 1);
    return (curr + prev, curr);
}
```

### <a name="semantics"></a><span data-ttu-id="d5cd5-119">语义</span><span class="sxs-lookup"><span data-stu-id="d5cd5-119">Semantics</span></span>

<span data-ttu-id="d5cd5-120">如果任何顶级语句出现在程序的任何编译单元中，就像它们合并到全局命名空间中 `Program` 类的 `Main` 方法的块体中一样，如下所示：</span><span class="sxs-lookup"><span data-stu-id="d5cd5-120">If any top-level statements are present in any compilation unit of the program, the meaning is as if they were combined in the block body of a `Main` method of a `Program` class in the global namespace, as follows:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

<span data-ttu-id="d5cd5-121">请注意，名称 "Program" 和 "Main" 仅用于说明，编译器使用的实际名称是依赖实现的，并且类型和方法都不能从源代码中按名称引用。</span><span class="sxs-lookup"><span data-stu-id="d5cd5-121">Note that the names "Program" and "Main" are used only for illustrations purposes, actual names used by compiler are implementation dependent and neither the type, nor the method can be referenced by name from source code.</span></span>

<span data-ttu-id="d5cd5-122">方法被指定为程序的入口点。</span><span class="sxs-lookup"><span data-stu-id="d5cd5-122">The method is designated as the entry point of the program.</span></span> <span data-ttu-id="d5cd5-123">按照约定可将显式声明的方法视为忽略入口点候选项。</span><span class="sxs-lookup"><span data-stu-id="d5cd5-123">Explicitly declared methods that by convention could be considered as an entry point candidates are ignored.</span></span> <span data-ttu-id="d5cd5-124">出现这种情况时，会报告警告。</span><span class="sxs-lookup"><span data-stu-id="d5cd5-124">A warning is reported when that happens.</span></span> <span data-ttu-id="d5cd5-125">如果存在顶级语句，则指定 `-main:<type>` 编译器开关是错误的。</span><span class="sxs-lookup"><span data-stu-id="d5cd5-125">It is an error to specify `-main:<type>` compiler switch when there are top-level statements.</span></span>

<span data-ttu-id="d5cd5-126">在顶级语句中允许使用异步操作，使其能够在常规异步入口点方法中的语句中使用。</span><span class="sxs-lookup"><span data-stu-id="d5cd5-126">Async operations are allowed in top-level statements to the degree they are allowed in statements within a regular async entry point method.</span></span> <span data-ttu-id="d5cd5-127">但是，它们不是必需的，如果省略 `await` 表达式和其他异步操作，则不会生成任何警告。</span><span class="sxs-lookup"><span data-stu-id="d5cd5-127">However, they are not required, if `await` expressions and other async operations are omitted, no warning is produced.</span></span> <span data-ttu-id="d5cd5-128">相反，生成的入口点方法的签名等效于</span><span class="sxs-lookup"><span data-stu-id="d5cd5-128">Instead the signature of the generated entry point method is equivalent to</span></span> 
``` c#
    static void Main()
```

<span data-ttu-id="d5cd5-129">上面的示例将生成以下 `$Main` 方法声明：</span><span class="sxs-lookup"><span data-stu-id="d5cd5-129">The example above would yield the following `$Main` method declaration:</span></span>

``` c#
static class $Program
{
    static void $Main()
    {
        if (System.Environment.CommandLine.Length == 0
            || !int.TryParse(System.Environment.CommandLine, out int n)
            || n < 0) return;
        Console.WriteLine(Fib(n).curr);
        
        (int curr, int prev) Fib(int i)
        {
            if (i == 0) return (1, 0);
            var (curr, prev) = Fib(i - 1);
            return (curr + prev, curr);
        }
    }
}
```

<span data-ttu-id="d5cd5-130">同时，示例如下所示：</span><span class="sxs-lookup"><span data-stu-id="d5cd5-130">At the same time an example like this:</span></span>
``` c#
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

<span data-ttu-id="d5cd5-131">将产生：</span><span class="sxs-lookup"><span data-stu-id="d5cd5-131">would  yield:</span></span>
``` c#
static class $Program
{
    static async Task $Main()
    {
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
    }
}
```

### <a name="scope-of-top-level-local-variables-and-local-functions"></a><span data-ttu-id="d5cd5-132">顶层本地变量和本地函数的作用域</span><span class="sxs-lookup"><span data-stu-id="d5cd5-132">Scope of top-level local variables and local functions</span></span>

<span data-ttu-id="d5cd5-133">即使顶级局部变量和函数被 "包装" 到生成的入口点方法中，它们仍应在每个编译单元中的整个程序范围内。</span><span class="sxs-lookup"><span data-stu-id="d5cd5-133">Even though top-level local variables and functions are "wrapped" into the generated entry point method, they should still be in scope throughout the program in every compilation unit.</span></span>
<span data-ttu-id="d5cd5-134">出于简单名称计算的目的，一旦达到全局命名空间即可：</span><span class="sxs-lookup"><span data-stu-id="d5cd5-134">For the purpose of simple-name evaluation, once the global namespace is reached:</span></span>
- <span data-ttu-id="d5cd5-135">首先，尝试在生成的入口点方法中计算名称，并且仅在此尝试失败时进行计算</span><span class="sxs-lookup"><span data-stu-id="d5cd5-135">First, an attempt is made to evaluate the name within the generated entry point method and only if this attempt fails</span></span> 
- <span data-ttu-id="d5cd5-136">全局命名空间声明中的 "常规" 计算是执行的。</span><span class="sxs-lookup"><span data-stu-id="d5cd5-136">The "regular" evaluation within the global namespace declaration is performed.</span></span> 

<span data-ttu-id="d5cd5-137">这可能导致在全局命名空间中声明的命名空间和类型进行名称隐藏，以及对导入的名称进行隐藏。</span><span class="sxs-lookup"><span data-stu-id="d5cd5-137">This could lead to name shadowing of namespaces and types declared within the global namespace as well as to shadowing of imported names.</span></span>

<span data-ttu-id="d5cd5-138">如果简单名称计算在顶级语句之外进行，并且计算产生顶级局部变量或函数，则会导致错误。</span><span class="sxs-lookup"><span data-stu-id="d5cd5-138">If the simple name evaluation occurs outside of the top-level statements and the evaluation yields a top-level local variable or function, that should lead to an error.</span></span>

<span data-ttu-id="d5cd5-139">通过这种方式，我们可以帮助我们更好地解决 "顶级函数" （ https://github.com/dotnet/csharplang/issues/3117)中的方案2），并能够向错误地认为受支持的用户提供有用的诊断。</span><span class="sxs-lookup"><span data-stu-id="d5cd5-139">In this way we protect our future ability to better address "Top-level functions" (scenario 2 in https://github.com/dotnet/csharplang/issues/3117), and are able to give useful diagnostics to users who mistakenly believe them to be supported.</span></span>

