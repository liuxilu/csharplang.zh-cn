---
ms.openlocfilehash: 0e2b4b70e0555145f54be2cc57da9f50f5a43548
ms.sourcegitcommit: 1ecebc412f073f3959395f33b70666c8ede88f3c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "81612098"
---
# <a name="simple-programs"></a>简单程序

* [x] 建议
* [x] 原型：已启动
* [x] 实现：已启动
* [ ] 规格：未启动

## <a name="summary"></a>总结
[summary]: #summary

允许在*compilation_unit（* 即源文件）*的namespace_member_declaration*之前发生一系列*语句*。

语义是，如果存在这样的*语句*序列，将发出以下类型声明，即实际类型名称和方法名称的 modulo：

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

由于需要一种显式`Main`方法，即使是最简单的程序，也会有一定数量的样板。 这似乎妨碍了语言学习和程序清晰度。 因此，为了学习者和代码的清晰度，该功能的主要目标是允许 C# 程序周围没有不必要的样板。

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

### <a name="syntax"></a>语法

唯一的附加语法是允许在编译单元中设置一系列*语句*，就在*namespace_member_declaration*之前：

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

只允许一*个compilation_unit*具有*语句*。 

示例：

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

### <a name="semantics"></a>语义

如果程序的任何编译单元中存在任何顶级语句，则含义就像它们组合在全局命名空间中`Main``Program`类方法的块正文中一样，如下所示：

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

请注意，"程序"和"Main"的名称仅用于插图目的，编译器使用的实际名称取决于实现，并且既不能使用类型，也不能用源代码中的名称引用方法。

该方法被指定为程序的入口点。 明确声明的方法，根据惯例可以被视为入口点候选者将被忽略。 发生此情况时将报告警告。 当存在顶级语句时，`-main:<type>`指定编译器开关是错误的。

在顶级语句中允许异步操作，使其在常规异步入口点方法中的语句中允许。 但是，它们不是必需的，如果`await`省略表达式和其他异步操作，则不生成警告。

生成的入口点方法的签名根据顶级语句使用的操作确定，如下所示：
**异步操作_返回与表达式** | **存在** | **不存在**
----------------------------------------| -------------|-------------
**存在** | ```static Task<int> Main()```| ```static Task Main()```
**不存在**  | ```static int Main()``` | ```static void Main()```

上面的示例将生成以下`$Main`方法声明：

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

同时，例如：
``` c#
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

会产生：
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

像这样的示例：
``` c#
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
return 0;
```

会产生：
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

例如：
``` c#
System.Console.WriteLine("Hi!");
return 2;
```

会产生：
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

### <a name="scope-of-top-level-local-variables-and-local-functions"></a>顶级局部变量和局部函数的范围

即使顶级局部变量和函数被"包装"到生成的入口点方法中，它们仍应在整个编译单元中在整个程序中。
为了进行简单名称评估，一旦达到全局命名空间：
- 首先，尝试在生成的入口点方法中评估名称，并且仅当此尝试失败时 
- 将执行全局命名空间声明中的"常规"评估。 

这可能会导致命名空间和类型在全局命名空间中声明的名称阴影，以及导入名称的隐藏。

如果简单名称计算发生在顶级语句之外，并且计算生成顶级局部变量或函数，则应会导致错误。

通过这种方式，我们保护我们未来更好地处理"顶级函数"的能力（方案中 2），https://github.com/dotnet/csharplang/issues/3117)并能够为错误地认为支持它们的用户提供有用的诊断。

