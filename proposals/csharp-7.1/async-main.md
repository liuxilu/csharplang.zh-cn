---
ms.openlocfilehash: 405153448d0e3685d6f22725e00d75d9250b3e20
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483750"
---
# <a name="async-main"></a><span data-ttu-id="f8936-101">Async Main</span><span class="sxs-lookup"><span data-stu-id="f8936-101">Async Main</span></span>

* <span data-ttu-id="f8936-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="f8936-102">[x] Proposed</span></span>
* <span data-ttu-id="f8936-103">[] 原型</span><span class="sxs-lookup"><span data-stu-id="f8936-103">[ ] Prototype</span></span>
* <span data-ttu-id="f8936-104">[] 实现</span><span class="sxs-lookup"><span data-stu-id="f8936-104">[ ] Implementation</span></span>
* <span data-ttu-id="f8936-105">[] 规范</span><span class="sxs-lookup"><span data-stu-id="f8936-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="f8936-106">总结</span><span class="sxs-lookup"><span data-stu-id="f8936-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="f8936-107">允许在应用程序的 Main/entrypoint 方法中使用 `await`，方法是允许入口点返回 `Task` / `Task<int>` 并将其标记为 `async`。</span><span class="sxs-lookup"><span data-stu-id="f8936-107">Allow `await` to be used in an application's Main / entrypoint method by allowing the entrypoint to return `Task` / `Task<int>` and be marked `async`.</span></span>

## <a name="motivation"></a><span data-ttu-id="f8936-108">动机</span><span class="sxs-lookup"><span data-stu-id="f8936-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="f8936-109">在学习C#时，在编写基于控制台的实用程序时，以及在编写要从 Main 调用和 `await` `async` 方法的小型测试应用程序时，此方法非常常见。</span><span class="sxs-lookup"><span data-stu-id="f8936-109">It is very common when learning C#, when writing console-based utilities, and when writing small test apps to want to call and `await` `async` methods from Main.</span></span>  <span data-ttu-id="f8936-110">如今，我们通过强制在单独的异步方法中完成此类 `await`的操作，在此处添加了一个级别的复杂程度，使开发人员只需编写如下所示的样板即可开始使用：</span><span class="sxs-lookup"><span data-stu-id="f8936-110">Today we add a level of complexity here by forcing such `await`'ing to be done in a separate async method, which causes developers to need to write boilerplate like the following just to get started:</span></span>

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

<span data-ttu-id="f8936-111">我们可以消除对这种样板的需求，使其更易于开始，只需允许主要 `async` 以便在其中使用 `await`。</span><span class="sxs-lookup"><span data-stu-id="f8936-111">We can remove the need for this boilerplate and make it easier to get started simply by allowing Main itself to be `async` such that `await`s can be used in it.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="f8936-112">详细设计</span><span class="sxs-lookup"><span data-stu-id="f8936-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="f8936-113">当前允许 s 以下签名：</span><span class="sxs-lookup"><span data-stu-id="f8936-113">The following signatures are currently allowed entrypoints:</span></span>

```csharp
static void Main()
static void Main(string[])
static int Main()
static int Main(string[])
```

<span data-ttu-id="f8936-114">我们将扩展允许的 s 的列表，使其包括：</span><span class="sxs-lookup"><span data-stu-id="f8936-114">We extend the list of allowed entrypoints to include:</span></span>

```csharp
static Task Main()
static Task<int> Main()
static Task Main(string[])
static Task<int> Main(string[])
```

<span data-ttu-id="f8936-115">为避免出现兼容性风险，如果不存在以前集的任何重载，则这些新签名将仅被视为有效的 s。</span><span class="sxs-lookup"><span data-stu-id="f8936-115">To avoid compatibility risks, these new signatures will only be considered as valid entrypoints if no overloads of the previous set are present.</span></span>
<span data-ttu-id="f8936-116">语言/编译器不需要将 entrypoint 标记为 `async`，不过，大多数使用情况都将标记为。</span><span class="sxs-lookup"><span data-stu-id="f8936-116">The language / compiler will not require that the entrypoint be marked as `async`, though we expect the vast majority of uses will be marked as such.</span></span>

<span data-ttu-id="f8936-117">当其中一项被标识为入口点时，编译器将合成调用以下编码方法之一的实际 entrypoint 方法：</span><span class="sxs-lookup"><span data-stu-id="f8936-117">When one of these is identified as the entrypoint, the compiler will synthesize an actual entrypoint method that calls one of these coded methods:</span></span>
- <span data-ttu-id="f8936-118">```static Task Main()``` 将导致编译器发出等效的 ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="f8936-118">```static Task Main()``` will result in the compiler emitting the equivalent of ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="f8936-119">```static Task Main(string[])``` 将导致编译器发出等效的 ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="f8936-119">```static Task Main(string[])``` will result in the compiler emitting the equivalent of ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="f8936-120">```static Task<int> Main()``` 将导致编译器发出等效的 ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="f8936-120">```static Task<int> Main()``` will result in the compiler emitting the equivalent of ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="f8936-121">```static Task<int> Main(string[])``` 将导致编译器发出等效的 ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="f8936-121">```static Task<int> Main(string[])``` will result in the compiler emitting the equivalent of ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span></span>

<span data-ttu-id="f8936-122">用法示例：</span><span class="sxs-lookup"><span data-stu-id="f8936-122">Example usage:</span></span>

```csharp
using System;
using System.Net.Http;

class Test
{
    static async Task Main(string[] args) =>
        Console.WriteLine(await new HttpClient().GetStringAsync(args[0]));
}
```

## <a name="drawbacks"></a><span data-ttu-id="f8936-123">缺点</span><span class="sxs-lookup"><span data-stu-id="f8936-123">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="f8936-124">主要缺点只是支持其他入口点签名的额外复杂性。</span><span class="sxs-lookup"><span data-stu-id="f8936-124">The main drawback is simply the additional complexity of supporting additional entrypoint signatures.</span></span>

## <a name="alternatives"></a><span data-ttu-id="f8936-125">备选项</span><span class="sxs-lookup"><span data-stu-id="f8936-125">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="f8936-126">其他变体：</span><span class="sxs-lookup"><span data-stu-id="f8936-126">Other variants considered:</span></span>

<span data-ttu-id="f8936-127">允许 `async void`。</span><span class="sxs-lookup"><span data-stu-id="f8936-127">Allowing `async void`.</span></span>  <span data-ttu-id="f8936-128">对于直接调用该语义的代码，我们需要将其保持不变，这样就会使得生成的入口点调用它（未返回任务）变得困难。</span><span class="sxs-lookup"><span data-stu-id="f8936-128">We need to keep the semantics the same for code calling it directly, which would then make it difficult for a generated entrypoint to call it (no Task returned).</span></span>  <span data-ttu-id="f8936-129">我们可以通过生成另外两种方法来解决这种情况，例如：</span><span class="sxs-lookup"><span data-stu-id="f8936-129">We could solve this by generating two other methods, e.g.</span></span>

```csharp
public static async void Main()
{
   ... // await code
}
```

<span data-ttu-id="f8936-130">变成</span><span class="sxs-lookup"><span data-stu-id="f8936-130">becomes</span></span>

```csharp
public static async void Main() => await $MainTask();

private static void $EntrypointMain() => Main().GetAwaiter().GetResult();

private static async Task $MainTask()
{
    ... // await code
}
```

<span data-ttu-id="f8936-131">还有关于鼓励使用 `async void`的问题。</span><span class="sxs-lookup"><span data-stu-id="f8936-131">There are also concerns around encouraging usage of `async void`.</span></span>

<span data-ttu-id="f8936-132">使用 "MainAsync" 而不是 "Main" 作为名称。</span><span class="sxs-lookup"><span data-stu-id="f8936-132">Using "MainAsync" instead of "Main" as the name.</span></span>  <span data-ttu-id="f8936-133">尽管建议对返回任务的方法使用 async 后缀，但主要是关于库功能（主要不是），并且支持其他入口点名称超出 "Main"。</span><span class="sxs-lookup"><span data-stu-id="f8936-133">While the async suffix is recommended for Task-returning methods, that's primarily about library functionality, which Main is not, and supporting additional entrypoint names beyond "Main" is not worth it.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="f8936-134">未解决的问题</span><span class="sxs-lookup"><span data-stu-id="f8936-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="f8936-135">不适用</span><span class="sxs-lookup"><span data-stu-id="f8936-135">n/a</span></span>

## <a name="design-meetings"></a><span data-ttu-id="f8936-136">设计会议</span><span class="sxs-lookup"><span data-stu-id="f8936-136">Design meetings</span></span>

<span data-ttu-id="f8936-137">不适用</span><span class="sxs-lookup"><span data-stu-id="f8936-137">n/a</span></span>
