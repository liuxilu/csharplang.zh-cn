---
ms.openlocfilehash: b9697fc1d772ba59ed3b1de339a5a3d4eb24b1bd
ms.sourcegitcommit: 36b028f4d6e88bd7d4a843c6d384d1b63cc73334
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/19/2020
ms.locfileid: "79484116"
---
# <a name="simple-programs"></a>简单程序

* [x] 建议
* [x] 原型：已启动
* [] 实现：未启动
* [] 规范：未启动

## <a name="summary"></a>总结
[summary]: #summary

允许一系列*语句*在*compilation_unit*的*namespace_member_declaration*（即源文件）之前立即发生。

语义是，如果存在这样一系列*语句*，则将发出下面的类型声明，取模实际类型名称和方法名称：

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

另请参阅 https://github.com/dotnet/csharplang/issues/3117。

## <a name="motivation"></a>动机
[motivation]: #motivation

由于需要显式 `Main` 方法，因此，甚至最简单的程序都有一定程度的样板。 这似乎会使语言学习和编程清晰。 因此，此功能的主要目标是使C#程序在没有必要的样本上无必要的样本，以方便学习和编写代码。

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

### <a name="syntax"></a>语法

唯一的附加语法允许编译单元中的一系列*语句*，刚好在*namespace_member_declaration*之前：

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

除了一个*compilation_unit*语句中，*语句*必须全部为本地函数声明。 

示例：

``` c#
// File 1 - any statements
if (args.Length == 0
    || !int.TryParse(args[0], out int n)
    || n < 0) return;
Console.WriteLine(Fib(n).curr);

// File 2 - only local functions
(int curr, int prev) Fib(int i)
{
    if (i == 0) return (1, 0);
    var (curr, prev) = Fib(i - 1);
    return (curr + prev, curr);
}
```

### <a name="semantics"></a>语义

如果任何顶级语句出现在程序的任何编译单元中，就像它们合并到全局命名空间中 `Program` 类的 `Main` 方法的块体中一样，如下所示：

``` c#
static class Program
{
    static async Task Main()
    {
        // File 1 statements
        // File 2 local functions
        // ...
    }
}
```

请注意，名称 "Program" 和 "Main" 仅用于说明，编译器使用的实际名称是依赖实现的，并且类型和方法都不能从源代码中按名称引用。

方法被指定为程序的入口点。 按照约定可将显式声明的方法视为忽略入口点候选项。 出现这种情况时，会报告警告。 指定 `-main:<type>` 编译器开关是错误的。

如果任何一个编译单元包含局部函数声明以外的语句，则将首先执行该编译单元中的语句。 这会使一个文件中的本地函数合法地引用另一个文件中的局部变量。 其他编译单元的语句发布顺序（完全是本地函数）未定义。

在顶级语句中允许使用异步操作，使其能够在常规异步入口点方法中的语句中使用。 但是，它们不是必需的，如果省略 `await` 表达式和其他异步操作，则不会生成任何警告。 相反，生成的入口点方法的签名等效于 
``` c#
    static void Main()
```

上面的示例将生成以下 `$Main` 方法声明：

``` c#
static class $Program
{
    static void $Main()
    {
        // Statements from File 1
        if (args.Length == 0
            || !int.TryParse(args[0], out int n)
            || n < 0) return;
        Console.WriteLine(Fib(n).curr);
        
        // Local functions from File 2
        (int curr, int prev) Fib(int i)
        {
            if (i == 0) return (1, 0);
            var (curr, prev) = Fib(i - 1);
            return (curr + prev, curr);
        }
    }
}
```

同时，示例如下所示：
``` c#
// File 1
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

将产生：
``` c#
static class $Program
{
    static async Task $Main()
    {
        // Statements from File 1
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
    }
}
```

### <a name="scope-of-top-level-local-variables-and-local-functions"></a>顶层本地变量和本地函数的作用域

即使顶层本地变量和函数被 "包装" 到生成的入口点方法中，它们仍应在整个程序中处于范围内。
出于简单名称计算的目的，一旦达到全局命名空间即可：
- 首先，尝试在生成的入口点方法中计算名称，并且仅在此尝试失败时进行计算 
- 全局命名空间声明中的 "常规" 计算是执行的。 

这可能导致在全局命名空间中声明的命名空间和类型进行名称隐藏，以及对导入的名称进行隐藏。

如果简单名称计算在顶级语句之外进行，并且计算产生顶级局部变量或函数，则会导致错误。

通过这种方式，我们可以帮助我们更好地解决 "顶级函数" （ https://github.com/dotnet/csharplang/issues/3117)中的方案2），并能够向错误地认为受支持的用户提供有用的诊断。

