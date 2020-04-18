---
ms.openlocfilehash: 0e2b4b70e0555145f54be2cc57da9f50f5a43548
ms.sourcegitcommit: 1ecebc412f073f3959395f33b70666c8ede88f3c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "81612098"
---
# <a name="simple-programs"></a><span data-ttu-id="4b5ec-101">简单程序</span><span class="sxs-lookup"><span data-stu-id="4b5ec-101">Simple programs</span></span>

* <span data-ttu-id="4b5ec-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="4b5ec-102">[x] Proposed</span></span>
* <span data-ttu-id="4b5ec-103">[x] 原型：已启动</span><span class="sxs-lookup"><span data-stu-id="4b5ec-103">[x] Prototype: Started</span></span>
* <span data-ttu-id="4b5ec-104">[x] 实现：已启动</span><span class="sxs-lookup"><span data-stu-id="4b5ec-104">[x] Implementation: Started</span></span>
* <span data-ttu-id="4b5ec-105">[ ] 规格：未启动</span><span class="sxs-lookup"><span data-stu-id="4b5ec-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="4b5ec-106">总结</span><span class="sxs-lookup"><span data-stu-id="4b5ec-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="4b5ec-107">允许在*compilation_unit（* 即源文件）*的namespace_member_declaration*之前发生一系列*语句*。</span><span class="sxs-lookup"><span data-stu-id="4b5ec-107">Allow a sequence of *statements* to occur right before the *namespace_member_declaration*s of a *compilation_unit* (i.e. source file).</span></span>

<span data-ttu-id="4b5ec-108">语义是，如果存在这样的*语句*序列，将发出以下类型声明，即实际类型名称和方法名称的 modulo：</span><span class="sxs-lookup"><span data-stu-id="4b5ec-108">The semantics are that if such a sequence of *statements* is present, the following type declaration, modulo the actual type name and the method name, would be emitted:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

<span data-ttu-id="4b5ec-109">另请参阅 https://github.com/dotnet/csharplang/issues/3117。</span><span class="sxs-lookup"><span data-stu-id="4b5ec-109">See also https://github.com/dotnet/csharplang/issues/3117.</span></span>

## <a name="motivation"></a><span data-ttu-id="4b5ec-110">动机</span><span class="sxs-lookup"><span data-stu-id="4b5ec-110">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="4b5ec-111">由于需要一种显式`Main`方法，即使是最简单的程序，也会有一定数量的样板。</span><span class="sxs-lookup"><span data-stu-id="4b5ec-111">There's a certain amount of boilerplate surrounding even the simplest of programs, because of the need for an explicit `Main` method.</span></span> <span data-ttu-id="4b5ec-112">这似乎妨碍了语言学习和程序清晰度。</span><span class="sxs-lookup"><span data-stu-id="4b5ec-112">This seems to get in the way of language learning and program clarity.</span></span> <span data-ttu-id="4b5ec-113">因此，为了学习者和代码的清晰度，该功能的主要目标是允许 C# 程序周围没有不必要的样板。</span><span class="sxs-lookup"><span data-stu-id="4b5ec-113">The primary goal of the feature therefore is to allow C# programs without unnecessary boilerplate around them, for the sake of learners and the clarity of code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="4b5ec-114">详细设计</span><span class="sxs-lookup"><span data-stu-id="4b5ec-114">Detailed design</span></span>
[design]: #detailed-design

### <a name="syntax"></a><span data-ttu-id="4b5ec-115">语法</span><span class="sxs-lookup"><span data-stu-id="4b5ec-115">Syntax</span></span>

<span data-ttu-id="4b5ec-116">唯一的附加语法是允许在编译单元中设置一系列*语句*，就在*namespace_member_declaration*之前：</span><span class="sxs-lookup"><span data-stu-id="4b5ec-116">The only additional syntax is allowing a sequence of *statement*s in a compilation unit, just before the *namespace_member_declaration*s:</span></span>

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

<span data-ttu-id="4b5ec-117">只允许一*个compilation_unit*具有*语句*。</span><span class="sxs-lookup"><span data-stu-id="4b5ec-117">Only one *compilation_unit* is allowed to have *statement*s.</span></span> 

<span data-ttu-id="4b5ec-118">示例：</span><span class="sxs-lookup"><span data-stu-id="4b5ec-118">Example:</span></span>

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

### <a name="semantics"></a><span data-ttu-id="4b5ec-119">语义</span><span class="sxs-lookup"><span data-stu-id="4b5ec-119">Semantics</span></span>

<span data-ttu-id="4b5ec-120">如果程序的任何编译单元中存在任何顶级语句，则含义就像它们组合在全局命名空间中`Main``Program`类方法的块正文中一样，如下所示：</span><span class="sxs-lookup"><span data-stu-id="4b5ec-120">If any top-level statements are present in any compilation unit of the program, the meaning is as if they were combined in the block body of a `Main` method of a `Program` class in the global namespace, as follows:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

<span data-ttu-id="4b5ec-121">请注意，"程序"和"Main"的名称仅用于插图目的，编译器使用的实际名称取决于实现，并且既不能使用类型，也不能用源代码中的名称引用方法。</span><span class="sxs-lookup"><span data-stu-id="4b5ec-121">Note that the names "Program" and "Main" are used only for illustrations purposes, actual names used by compiler are implementation dependent and neither the type, nor the method can be referenced by name from source code.</span></span>

<span data-ttu-id="4b5ec-122">该方法被指定为程序的入口点。</span><span class="sxs-lookup"><span data-stu-id="4b5ec-122">The method is designated as the entry point of the program.</span></span> <span data-ttu-id="4b5ec-123">明确声明的方法，根据惯例可以被视为入口点候选者将被忽略。</span><span class="sxs-lookup"><span data-stu-id="4b5ec-123">Explicitly declared methods that by convention could be considered as an entry point candidates are ignored.</span></span> <span data-ttu-id="4b5ec-124">发生此情况时将报告警告。</span><span class="sxs-lookup"><span data-stu-id="4b5ec-124">A warning is reported when that happens.</span></span> <span data-ttu-id="4b5ec-125">当存在顶级语句时，`-main:<type>`指定编译器开关是错误的。</span><span class="sxs-lookup"><span data-stu-id="4b5ec-125">It is an error to specify `-main:<type>` compiler switch when there are top-level statements.</span></span>

<span data-ttu-id="4b5ec-126">在顶级语句中允许异步操作，使其在常规异步入口点方法中的语句中允许。</span><span class="sxs-lookup"><span data-stu-id="4b5ec-126">Async operations are allowed in top-level statements to the degree they are allowed in statements within a regular async entry point method.</span></span> <span data-ttu-id="4b5ec-127">但是，它们不是必需的，如果`await`省略表达式和其他异步操作，则不生成警告。</span><span class="sxs-lookup"><span data-stu-id="4b5ec-127">However, they are not required, if `await` expressions and other async operations are omitted, no warning is produced.</span></span>

<span data-ttu-id="4b5ec-128">生成的入口点方法的签名根据顶级语句使用的操作确定，如下所示：</span><span class="sxs-lookup"><span data-stu-id="4b5ec-128">The signature of the generated entry point method is determined based on operations used by the top level statements as follows:</span></span>
<span data-ttu-id="4b5ec-129">**异步操作_返回与表达式**</span><span class="sxs-lookup"><span data-stu-id="4b5ec-129">**Async-operations\Return-with-expression**</span></span> | <span data-ttu-id="4b5ec-130">**存在**</span><span class="sxs-lookup"><span data-stu-id="4b5ec-130">**Present**</span></span> | <span data-ttu-id="4b5ec-131">**不存在**</span><span class="sxs-lookup"><span data-stu-id="4b5ec-131">**Absent**</span></span>
----------------------------------------| -------------|-------------
<span data-ttu-id="4b5ec-132">**存在**</span><span class="sxs-lookup"><span data-stu-id="4b5ec-132">**Present**</span></span> | ```static Task<int> Main()```| ```static Task Main()```
<span data-ttu-id="4b5ec-133">**不存在**</span><span class="sxs-lookup"><span data-stu-id="4b5ec-133">**Absent**</span></span>  | ```static int Main()``` | ```static void Main()```

<span data-ttu-id="4b5ec-134">上面的示例将生成以下`$Main`方法声明：</span><span class="sxs-lookup"><span data-stu-id="4b5ec-134">The example above would yield the following `$Main` method declaration:</span></span>

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

<span data-ttu-id="4b5ec-135">同时，例如：</span><span class="sxs-lookup"><span data-stu-id="4b5ec-135">At the same time an example like this:</span></span>
``` c#
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

<span data-ttu-id="4b5ec-136">会产生：</span><span class="sxs-lookup"><span data-stu-id="4b5ec-136">would  yield:</span></span>
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

<span data-ttu-id="4b5ec-137">像这样的示例：</span><span class="sxs-lookup"><span data-stu-id="4b5ec-137">An example like this:</span></span>
``` c#
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
return 0;
```

<span data-ttu-id="4b5ec-138">会产生：</span><span class="sxs-lookup"><span data-stu-id="4b5ec-138">would  yield:</span></span>
``` c#
static class $Program
{
    static async Task<int> $Main()
    {
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
        return 0;
    }
}
```

<span data-ttu-id="4b5ec-139">例如：</span><span class="sxs-lookup"><span data-stu-id="4b5ec-139">And an example like this:</span></span>
``` c#
System.Console.WriteLine("Hi!");
return 2;
```

<span data-ttu-id="4b5ec-140">会产生：</span><span class="sxs-lookup"><span data-stu-id="4b5ec-140">would  yield:</span></span>
``` c#
static class $Program
{
    static int $Main()
    {
        System.Console.WriteLine("Hi!");
        return 2;
    }
}
```

### <a name="scope-of-top-level-local-variables-and-local-functions"></a><span data-ttu-id="4b5ec-141">顶级局部变量和局部函数的范围</span><span class="sxs-lookup"><span data-stu-id="4b5ec-141">Scope of top-level local variables and local functions</span></span>

<span data-ttu-id="4b5ec-142">即使顶级局部变量和函数被"包装"到生成的入口点方法中，它们仍应在整个编译单元中在整个程序中。</span><span class="sxs-lookup"><span data-stu-id="4b5ec-142">Even though top-level local variables and functions are "wrapped" into the generated entry point method, they should still be in scope throughout the program in every compilation unit.</span></span>
<span data-ttu-id="4b5ec-143">为了进行简单名称评估，一旦达到全局命名空间：</span><span class="sxs-lookup"><span data-stu-id="4b5ec-143">For the purpose of simple-name evaluation, once the global namespace is reached:</span></span>
- <span data-ttu-id="4b5ec-144">首先，尝试在生成的入口点方法中评估名称，并且仅当此尝试失败时</span><span class="sxs-lookup"><span data-stu-id="4b5ec-144">First, an attempt is made to evaluate the name within the generated entry point method and only if this attempt fails</span></span> 
- <span data-ttu-id="4b5ec-145">将执行全局命名空间声明中的"常规"评估。</span><span class="sxs-lookup"><span data-stu-id="4b5ec-145">The "regular" evaluation within the global namespace declaration is performed.</span></span> 

<span data-ttu-id="4b5ec-146">这可能会导致命名空间和类型在全局命名空间中声明的名称阴影，以及导入名称的隐藏。</span><span class="sxs-lookup"><span data-stu-id="4b5ec-146">This could lead to name shadowing of namespaces and types declared within the global namespace as well as to shadowing of imported names.</span></span>

<span data-ttu-id="4b5ec-147">如果简单名称计算发生在顶级语句之外，并且计算生成顶级局部变量或函数，则应会导致错误。</span><span class="sxs-lookup"><span data-stu-id="4b5ec-147">If the simple name evaluation occurs outside of the top-level statements and the evaluation yields a top-level local variable or function, that should lead to an error.</span></span>

<span data-ttu-id="4b5ec-148">通过这种方式，我们保护我们未来更好地处理"顶级函数"的能力（方案中 2），https://github.com/dotnet/csharplang/issues/3117)并能够为错误地认为支持它们的用户提供有用的诊断。</span><span class="sxs-lookup"><span data-stu-id="4b5ec-148">In this way we protect our future ability to better address "Top-level functions" (scenario 2 in https://github.com/dotnet/csharplang/issues/3117), and are able to give useful diagnostics to users who mistakenly believe them to be supported.</span></span>

