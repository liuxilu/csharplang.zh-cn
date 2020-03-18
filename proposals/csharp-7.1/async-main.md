---
ms.openlocfilehash: 405153448d0e3685d6f22725e00d75d9250b3e20
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483750"
---
# <a name="async-main"></a>Async Main

* [x] 建议
* [] 原型
* [] 实现
* [] 规范

## <a name="summary"></a>总结
[summary]: #summary

允许在应用程序的 Main/entrypoint 方法中使用 `await`，方法是允许入口点返回 `Task` / `Task<int>` 并将其标记为 `async`。

## <a name="motivation"></a>动机
[motivation]: #motivation

在学习C#时，在编写基于控制台的实用程序时，以及在编写要从 Main 调用和 `await` `async` 方法的小型测试应用程序时，此方法非常常见。  如今，我们通过强制在单独的异步方法中完成此类 `await`的操作，在此处添加了一个级别的复杂程度，使开发人员只需编写如下所示的样板即可开始使用：

```csharp
public static void Main()
{
    MainAsync().GetAwaiter().GetResult();
}

private static async Task MainAsync()
{
    ... // Main body here
}
```

我们可以消除对这种样板的需求，使其更易于开始，只需允许主要 `async` 以便在其中使用 `await`。

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

当前允许 s 以下签名：

```csharp
static void Main()
static void Main(string[])
static int Main()
static int Main(string[])
```

我们将扩展允许的 s 的列表，使其包括：

```csharp
static Task Main()
static Task<int> Main()
static Task Main(string[])
static Task<int> Main(string[])
```

为避免出现兼容性风险，如果不存在以前集的任何重载，则这些新签名将仅被视为有效的 s。
语言/编译器不需要将 entrypoint 标记为 `async`，不过，大多数使用情况都将标记为。

当其中一项被标识为入口点时，编译器将合成调用以下编码方法之一的实际 entrypoint 方法：
- ```static Task Main()``` 将导致编译器发出等效的 ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```
- ```static Task Main(string[])``` 将导致编译器发出等效的 ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```
- ```static Task<int> Main()``` 将导致编译器发出等效的 ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```
- ```static Task<int> Main(string[])``` 将导致编译器发出等效的 ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```

用法示例：

```csharp
using System;
using System.Net.Http;

class Test
{
    static async Task Main(string[] args) =>
        Console.WriteLine(await new HttpClient().GetStringAsync(args[0]));
}
```

## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

主要缺点只是支持其他入口点签名的额外复杂性。

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

其他变体：

允许 `async void`。  对于直接调用该语义的代码，我们需要将其保持不变，这样就会使得生成的入口点调用它（未返回任务）变得困难。  我们可以通过生成另外两种方法来解决这种情况，例如：

```csharp
public static async void Main()
{
   ... // await code
}
```

变成

```csharp
public static async void Main() => await $MainTask();

private static void $EntrypointMain() => Main().GetAwaiter().GetResult();

private static async Task $MainTask()
{
    ... // await code
}
```

还有关于鼓励使用 `async void`的问题。

使用 "MainAsync" 而不是 "Main" 作为名称。  尽管建议对返回任务的方法使用 async 后缀，但主要是关于库功能（主要不是），并且支持其他入口点名称超出 "Main"。

## <a name="unresolved-questions"></a>未解决的问题
[unresolved]: #unresolved-questions

不适用

## <a name="design-meetings"></a>设计会议

不适用
