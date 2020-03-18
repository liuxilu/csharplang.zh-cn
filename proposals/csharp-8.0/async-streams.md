---
ms.openlocfilehash: 9ed1aa75d581cecbf754a84d1f523c6334b8c0ac
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "79484146"
---
# <a name="async-streams"></a><span data-ttu-id="35890-101">异步流</span><span class="sxs-lookup"><span data-stu-id="35890-101">Async Streams</span></span>

* <span data-ttu-id="35890-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="35890-102">[x] Proposed</span></span>
* <span data-ttu-id="35890-103">[x] 原型</span><span class="sxs-lookup"><span data-stu-id="35890-103">[x] Prototype</span></span>
* <span data-ttu-id="35890-104">[] 实现</span><span class="sxs-lookup"><span data-stu-id="35890-104">[ ] Implementation</span></span>
* <span data-ttu-id="35890-105">[] 规范</span><span class="sxs-lookup"><span data-stu-id="35890-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="35890-106">总结</span><span class="sxs-lookup"><span data-stu-id="35890-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="35890-107">C#支持迭代器方法和异步方法，但不支持同时作为迭代器和异步方法的方法。</span><span class="sxs-lookup"><span data-stu-id="35890-107">C# has support for iterator methods and async methods, but no support for a method that is both an iterator and an async method.</span></span>  <span data-ttu-id="35890-108">我们应该通过允许 `await` 在新的 `async` 迭代器（返回 `IAsyncEnumerable<T>` 或 `IAsyncEnumerator<T>` 而不是 `IEnumerable<T>` 或 `IEnumerator<T>`）中使用来纠正这种情况，在新 `IAsyncEnumerable<T>` 中使用 `await foreach`。</span><span class="sxs-lookup"><span data-stu-id="35890-108">We should rectify this by allowing for `await` to be used in a new form of `async` iterator, one that returns an `IAsyncEnumerable<T>` or `IAsyncEnumerator<T>` rather than an `IEnumerable<T>` or `IEnumerator<T>`, with `IAsyncEnumerable<T>` consumable in a new `await foreach`.</span></span>  <span data-ttu-id="35890-109">`IAsyncDisposable` 接口还用于启用异步清理。</span><span class="sxs-lookup"><span data-stu-id="35890-109">An `IAsyncDisposable` interface is also used to enable asynchronous cleanup.</span></span>

## <a name="related-discussion"></a><span data-ttu-id="35890-110">相关讨论</span><span class="sxs-lookup"><span data-stu-id="35890-110">Related discussion</span></span>
- https://github.com/dotnet/roslyn/issues/261
- https://github.com/dotnet/roslyn/issues/114

## <a name="detailed-design"></a><span data-ttu-id="35890-111">详细设计</span><span class="sxs-lookup"><span data-stu-id="35890-111">Detailed design</span></span>
[design]: #detailed-design

## <a name="interfaces"></a><span data-ttu-id="35890-112">界面</span><span class="sxs-lookup"><span data-stu-id="35890-112">Interfaces</span></span>

### <a name="iasyncdisposable"></a><span data-ttu-id="35890-113">IAsyncDisposable</span><span class="sxs-lookup"><span data-stu-id="35890-113">IAsyncDisposable</span></span>

<span data-ttu-id="35890-114">对 `IAsyncDisposable` （例如 https://github.com/dotnet/roslyn/issues/114) 以及它是否是一个好主意）有很多讨论。</span><span class="sxs-lookup"><span data-stu-id="35890-114">There has been much discussion of `IAsyncDisposable` (e.g. https://github.com/dotnet/roslyn/issues/114) and whether it's a good idea.</span></span>  <span data-ttu-id="35890-115">不过，它是添加异步迭代器支持所需的概念。</span><span class="sxs-lookup"><span data-stu-id="35890-115">However, it's a required concept to add in support of async iterators.</span></span>  <span data-ttu-id="35890-116">由于 `finally` 块可能包含 `await`，并且由于需要在处理迭代器的过程中运行 `finally` 块，因此我们需要异步处理。</span><span class="sxs-lookup"><span data-stu-id="35890-116">Since `finally` blocks may contain `await`s, and since `finally` blocks need to be run as part of disposing of iterators, we need async disposal.</span></span>  <span data-ttu-id="35890-117">通常情况下，任何时候清理资源所需的时间都很有用，例如关闭文件（需要刷新）、取消注册回调并提供一种了解注销完成时间等的方法。</span><span class="sxs-lookup"><span data-stu-id="35890-117">It's also just generally useful any time cleaning up of resources might take any period of time, e.g. closing files (requiring flushes), deregistering callbacks and providing a way to know when deregistration has completed, etc.</span></span>

<span data-ttu-id="35890-118">以下接口将添加到核心 .NET 库（例如 System.private.corelib/Runtime）：</span><span class="sxs-lookup"><span data-stu-id="35890-118">The following interface is added to the core .NET libraries (e.g. System.Private.CoreLib / System.Runtime):</span></span>
```csharp
namespace System
{
    public interface IAsyncDisposable
    {
        ValueTask DisposeAsync();
    }
}
```
<span data-ttu-id="35890-119">与 `Dispose`一样，多次调用 `DisposeAsync` 是可接受的，并且第一次调用之后的调用应视为 nops，并返回同步完成任务（`DisposeAsync` 无需线程安全），并且无需支持并发调用。</span><span class="sxs-lookup"><span data-stu-id="35890-119">As with `Dispose`, invoking `DisposeAsync` multiple times is acceptable, and subsequent invocations after the first should be treated as nops, returning a synchronously completed successful task (`DisposeAsync` need not be thread-safe, though, and need not support concurrent invocation).</span></span>  <span data-ttu-id="35890-120">此外，类型可以同时实现 `IDisposable` 和 `IAsyncDisposable`，如果这样做，则可以像调用 `Dispose`，然后 `DisposeAsync`，反之亦然，而只有第一种方法是有意义的，后续调用都应该是 nop。</span><span class="sxs-lookup"><span data-stu-id="35890-120">Further, types may implement both `IDisposable` and `IAsyncDisposable`, and if they do, it's similarly acceptable to invoke `Dispose` and then `DisposeAsync` or vice versa, but only the first should be meaningful and subsequent invocations of either should be a nop.</span></span>  <span data-ttu-id="35890-121">因此，如果类型确实实现了这两种类型，则鼓励使用者调用一次，并且只能调用一次基于上下文的相关方法，`Dispose` 在同步上下文中并在异步环境中 `DisposeAsync`。</span><span class="sxs-lookup"><span data-stu-id="35890-121">As such, if a type does implement both, consumers are encouraged to call once and only once the more relevant method based on the context, `Dispose` in synchronous contexts and `DisposeAsync` in asynchronous ones.</span></span>

<span data-ttu-id="35890-122">（我将讨论 `IAsyncDisposable` 如何与 `using` 进行相互讨论。</span><span class="sxs-lookup"><span data-stu-id="35890-122">(I'm leaving discussion of how `IAsyncDisposable` interacts with `using` to a separate discussion.</span></span>  <span data-ttu-id="35890-123">稍后将在此建议中处理与 `foreach` 进行交互的方式。）</span><span class="sxs-lookup"><span data-stu-id="35890-123">And coverage of how it interacts with `foreach` is handled later in this proposal.)</span></span>

<span data-ttu-id="35890-124">考虑的替代方案：</span><span class="sxs-lookup"><span data-stu-id="35890-124">Alternatives considered:</span></span>
- <span data-ttu-id="35890-125">_`DisposeAsync` 接受 `CancellationToken`_ ：尽管在理论上，任何异步操作都可以取消，处置就是清除、关闭 free'ing 资源等，这通常不是应取消的内容;对于取消的工作，清除仍很重要。</span><span class="sxs-lookup"><span data-stu-id="35890-125">_`DisposeAsync` accepting a `CancellationToken`_: while in theory it makes sense that anything async can be canceled, disposal is about cleanup, closing things out, free'ing resources, etc., which is generally not something that should be canceled; cleanup is still important for work that's canceled.</span></span>  <span data-ttu-id="35890-126">导致实际工作被取消的相同 `CancellationToken` 通常是传递给 `DisposeAsync`的同一令牌，`DisposeAsync` 毫无意义，因为取消工作会导致 `DisposeAsync` 为 nop。</span><span class="sxs-lookup"><span data-stu-id="35890-126">The same `CancellationToken` that caused the actual work to be canceled would typically be the same token passed to `DisposeAsync`, making `DisposeAsync` worthless because cancellation of the work would cause `DisposeAsync` to be a nop.</span></span>  <span data-ttu-id="35890-127">如果有人想要避免被阻止以等待处置，则可以避免等待产生的 `ValueTask`，或只等待一段时间。</span><span class="sxs-lookup"><span data-stu-id="35890-127">If someone wants to avoid being blocked waiting for disposal, they can avoid waiting on the resulting `ValueTask`, or wait on it only for some period of time.</span></span>
- <span data-ttu-id="35890-128">_`DisposeAsync` 返回 `Task`_ ：现在，非泛型 `ValueTask` 存在并可从 `IValueTaskSource`中构造，从 `ValueTask` 返回 `DisposeAsync` 允许使用现有对象作为表示 `DisposeAsync`的最终异步完成的承诺，在 `Task` 异步完成的情况下，保存 `DisposeAsync` 分配。</span><span class="sxs-lookup"><span data-stu-id="35890-128">_`DisposeAsync` returning a `Task`_: Now that a non-generic `ValueTask` exists and can be constructed from an `IValueTaskSource`, returning `ValueTask` from `DisposeAsync` allows an existing object to be reused as the promise representing the eventual async completion of `DisposeAsync`, saving a `Task` allocation in the case where `DisposeAsync` completes asynchronously.</span></span>
- <span data-ttu-id="35890-129">_使用 `bool continueOnCapturedContext` （`ConfigureAwait`）配置 `DisposeAsync`_ ：尽管可能存在与此类概念的公开方式相关的问题 `using`、`foreach`和其他使用该概念的语言构造，但从接口角度来看，实际上不会执行任何 `await`的操作，并且没有任何内容可供配置 .。。`ValueTask` 的使用者可以使用它们。</span><span class="sxs-lookup"><span data-stu-id="35890-129">_Configuring `DisposeAsync` with a `bool continueOnCapturedContext` (`ConfigureAwait`)_: While there may be issues related to how such a concept is exposed to `using`, `foreach`, and other language constructs that consume this, from an interface perspective it's not actually doing any `await`'ing and there's nothing to configure... consumers of the `ValueTask` can consume it however they wish.</span></span>
- <span data-ttu-id="35890-130">_继承 `IDisposable``IAsyncDisposable`_ ：只应使用其中一项或另一项，因此强制类型实现两者并无意义。</span><span class="sxs-lookup"><span data-stu-id="35890-130">_`IAsyncDisposable` inheriting `IDisposable`_:  Since only one or the other should be used, it doesn't make sense to force types to implement both.</span></span>
- <span data-ttu-id="35890-131">_`IDisposableAsync`，而不是 `IAsyncDisposable`_ ：我们一直在执行命名操作，其中，事物为 "异步内容"，而操作是 "异步" 的，因此，类型将 "async" 作为前缀，方法将 "async" 作为后缀。</span><span class="sxs-lookup"><span data-stu-id="35890-131">_`IDisposableAsync` instead of `IAsyncDisposable`_: We've been following the naming that things/types are an "async something" whereas operations are "done async", so types have "Async" as a prefix and methods have "Async" as a suffix.</span></span>

### <a name="iasyncenumerable--iasyncenumerator"></a><span data-ttu-id="35890-132">IAsyncEnumerable/IAsyncEnumerator</span><span class="sxs-lookup"><span data-stu-id="35890-132">IAsyncEnumerable / IAsyncEnumerator</span></span>

<span data-ttu-id="35890-133">向核心 .NET 库添加了两个接口：</span><span class="sxs-lookup"><span data-stu-id="35890-133">Two interfaces are added to the core .NET libraries:</span></span>

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken cancellationToken = default);
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> MoveNextAsync();
        T Current { get; }
    }
}
```

<span data-ttu-id="35890-134">典型使用情况（无其他语言功能）如下所示：</span><span class="sxs-lookup"><span data-stu-id="35890-134">Typical consumption (without additional language features) would look like:</span></span>

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
        Use(enumerator.Current);
    }
}
finally { await enumerator.DisposeAsync(); }
```

<span data-ttu-id="35890-135">丢弃的选项：</span><span class="sxs-lookup"><span data-stu-id="35890-135">Discarded options considered:</span></span>
- <span data-ttu-id="35890-136">_`Task<bool> MoveNextAsync(); T current { get; }`_ ：使用 `Task<bool>` 支持使用缓存的任务对象来表示同步、成功的 `MoveNextAsync` 调用，但异步完成仍需要分配。</span><span class="sxs-lookup"><span data-stu-id="35890-136">_`Task<bool> MoveNextAsync(); T current { get; }`_: Using `Task<bool>` would support using a cached task object to represent synchronous, successful `MoveNextAsync` calls, but an allocation would still be required for asynchronous completion.</span></span>  <span data-ttu-id="35890-137">通过返回 `ValueTask<bool>`，我们可以使枚举器对象自身实现 `IValueTaskSource<bool>`，并将其用作从 `MoveNextAsync`返回的 `ValueTask<bool>` 的后备，进而大大降低了开销。</span><span class="sxs-lookup"><span data-stu-id="35890-137">By returning `ValueTask<bool>`, we enable the enumerator object to itself implement `IValueTaskSource<bool>` and be used as the backing for the `ValueTask<bool>` returned from `MoveNextAsync`, which in turn allows for significantly reduced overheads.</span></span>
- <span data-ttu-id="35890-138">_`ValueTask<(bool, T)> MoveNextAsync();`_ ：不仅更难使用，而且它意味着 `T` 不再是协变的。</span><span class="sxs-lookup"><span data-stu-id="35890-138">_`ValueTask<(bool, T)> MoveNextAsync();`_: It's not only harder to consume, but it means that `T` can no longer be covariant.</span></span>
- <span data-ttu-id="35890-139">_`ValueTask<T?> TryMoveNextAsync();`_ ：不是协变的。</span><span class="sxs-lookup"><span data-stu-id="35890-139">_`ValueTask<T?> TryMoveNextAsync();`_: Not covariant.</span></span>
- <span data-ttu-id="35890-140">_`Task<T?> TryMoveNextAsync();`_ ：不是协变的，每个调用的分配等。</span><span class="sxs-lookup"><span data-stu-id="35890-140">_`Task<T?> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="35890-141">_`ITask<T?> TryMoveNextAsync();`_ ：不是协变的，每个调用的分配等。</span><span class="sxs-lookup"><span data-stu-id="35890-141">_`ITask<T?> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="35890-142">_`ITask<(bool,T)> TryMoveNextAsync();`_ ：不是协变的，每个调用的分配等。</span><span class="sxs-lookup"><span data-stu-id="35890-142">_`ITask<(bool,T)> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="35890-143">_`Task<bool> TryMoveNextAsync(out T result);`_ ：当操作以同步方式返回时，需要设置 `out` 结果，而不是在未来的某个时间内异步完成任务，此时无法传递结果。</span><span class="sxs-lookup"><span data-stu-id="35890-143">_`Task<bool> TryMoveNextAsync(out T result);`_: The `out` result would need to be set when the operation returns synchronously, not when it asynchronously completes the task potentially sometime long in the future, at which point there'd be no way to communicate the result.</span></span>
- <span data-ttu-id="35890-144">_`IAsyncEnumerator<T>` 未实现 `IAsyncDisposable`_ ：我们可以选择将它们分开。</span><span class="sxs-lookup"><span data-stu-id="35890-144">_`IAsyncEnumerator<T>` not implementing `IAsyncDisposable`_: We could choose to separate these.</span></span>  <span data-ttu-id="35890-145">不过，这样做会使建议的其他某些区域变得更加复杂，因为代码必须能够处理枚举器不提供释放的可能性，这使得编写基于模式的帮助器变得困难。</span><span class="sxs-lookup"><span data-stu-id="35890-145">However, doing so complicates certain other areas of the proposal, as code must then be able to deal with the possibility that an enumerator doesn't provide disposal, which makes it difficult to write pattern-based helpers.</span></span>  <span data-ttu-id="35890-146">此外，枚举器还需要释放（例如，具有 finally 块的任何异步迭代器C# 、从网络连接中枚举数据等的任何异步迭代器），如果不需要，则可以简单地将该方法作为 `public ValueTask DisposeAsync() => default(ValueTask);` 实现，只需最小的额外开销。</span><span class="sxs-lookup"><span data-stu-id="35890-146">Further, it will be common for enumerators to have a need for disposal (e.g. any C# async iterator that has a finally block, most things enumerating data from a network connection, etc.), and if one doesn't, it is simple to implement the method purely as `public ValueTask DisposeAsync() => default(ValueTask);` with minimal additional overhead.</span></span>
- <span data-ttu-id="35890-147">_ `IAsyncEnumerator<T> GetAsyncEnumerator()`：无取消标记参数。</span><span class="sxs-lookup"><span data-stu-id="35890-147">_ `IAsyncEnumerator<T> GetAsyncEnumerator()`: No cancellation token parameter.</span></span>

#### <a name="viable-alternative"></a><span data-ttu-id="35890-148">可行的替代方案：</span><span class="sxs-lookup"><span data-stu-id="35890-148">Viable alternative:</span></span>

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator();
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> WaitForNextAsync();
        T TryGetNext(out bool success);
    }
}
```

<span data-ttu-id="35890-149">在内部循环中使用 `TryGetNext`，只要它们可以同步使用，就会使用单个接口调用的项。</span><span class="sxs-lookup"><span data-stu-id="35890-149">`TryGetNext` is used in an inner loop to consume items with a single interface call as long as they're available synchronously.</span></span>  <span data-ttu-id="35890-150">当无法同步检索下一项时，它将返回 false，并在每次返回 false 时，调用方必须随后调用 `WaitForNextAsync` 以等待下一项可用或确定将永远不会有其他项。</span><span class="sxs-lookup"><span data-stu-id="35890-150">When the next item can't be retrieved synchronously, it returns false, and any time it returns false, a caller must subsequently invoke `WaitForNextAsync` to either wait for the next item to be available or to determine that there will never be another item.</span></span> <span data-ttu-id="35890-151">典型使用情况（无其他语言功能）如下所示：</span><span class="sxs-lookup"><span data-stu-id="35890-151">Typical consumption (without additional language features) would look like:</span></span>

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.WaitForNextAsync())
    {
        while (true)
        {
            int item = enumerator.TryGetNext(out bool success);
            if (!success) break;
            Use(item);
        }
    }
}
finally { await enumerator.DisposeAsync(); }
```

<span data-ttu-id="35890-152">这样做的优点是两折：</span><span class="sxs-lookup"><span data-stu-id="35890-152">The advantage of this is two-fold, one minor and one major:</span></span>
- <span data-ttu-id="35890-153">_小调：允许枚举器支持多个使用者_。</span><span class="sxs-lookup"><span data-stu-id="35890-153">_Minor: Allows for an enumerator to support multiple consumers_.</span></span> <span data-ttu-id="35890-154">在某些情况下，可以使用枚举器来支持多个并发使用者。</span><span class="sxs-lookup"><span data-stu-id="35890-154">There may be scenarios where it's valuable for an enumerator to support multiple concurrent consumers.</span></span>  <span data-ttu-id="35890-155">当 `MoveNextAsync` 和 `Current` 单独以便实现无法使其使用原子时，无法实现。</span><span class="sxs-lookup"><span data-stu-id="35890-155">That can't be achieved when `MoveNextAsync` and `Current` are separate such that an implementation can't make their usage atomic.</span></span>  <span data-ttu-id="35890-156">与此相反，此方法提供了一种方法 `TryGetNext`，该方法支持向前推送枚举器和获取下一项，因此，如果需要，枚举器可以启用原子性。</span><span class="sxs-lookup"><span data-stu-id="35890-156">In contrast, this approach provides a single method `TryGetNext` that supports pushing the enumerator forward and getting the next item, so the enumerator can enable atomicity if desired.</span></span>  <span data-ttu-id="35890-157">但是，这种情况很可能是通过为每个使用者提供来自共享可枚举的使用者自己的枚举器来实现的。</span><span class="sxs-lookup"><span data-stu-id="35890-157">However, it's likely that such scenarios could also be enabled by giving each consumer its own enumerator from a shared enumerable.</span></span>  <span data-ttu-id="35890-158">此外，我们不想强制每个枚举器都支持并发使用，因为这样做会将不重要的开销添加到不需要它的多数情况，这意味着接口的使用者通常不能以任何方式依赖于这种情况。</span><span class="sxs-lookup"><span data-stu-id="35890-158">Further, we don't want to enforce that every enumerator support concurrent usage, as that would add non-trivial overheads to the majority case that doesn't require it, which means a consumer of the interface generally couldn't rely on this any way.</span></span>
- <span data-ttu-id="35890-159">_主要：性能_。</span><span class="sxs-lookup"><span data-stu-id="35890-159">_Major: Performance_.</span></span> <span data-ttu-id="35890-160">`MoveNextAsync`/`Current` 方法需要每个操作有两个接口调用，而 `WaitForNextAsync`/`TryGetNext` 的最佳情况是，大多数迭代都是同步完成的，因此，使用 `TryGetNext`实现了一个紧密的内部循环，以便每个操作仅有一个接口调用。</span><span class="sxs-lookup"><span data-stu-id="35890-160">The `MoveNextAsync`/`Current` approach requires two interface calls per operation, whereas the best case for `WaitForNextAsync`/`TryGetNext` is that most iterations complete synchronously, enabling a tight inner loop with `TryGetNext`, such that we only have one interface call per operation.</span></span>  <span data-ttu-id="35890-161">这在接口调用占据计算的情况下可能会产生实实在在的影响。</span><span class="sxs-lookup"><span data-stu-id="35890-161">This can have a measurable impact in situations where the interface calls dominate the computation.</span></span>

<span data-ttu-id="35890-162">不过，这种缺点不太重要，包括手动使用这些方法时显著增加的复杂性，以及使用它们时引入 bug 的可能性也会增加。</span><span class="sxs-lookup"><span data-stu-id="35890-162">However, there are non-trivial downsides, including significantly increased complexity when consuming these manually, and an increased chance of introducing bugs when using them.</span></span>  <span data-ttu-id="35890-163">尽管性能优势在 microbenchmarks 中显示，但我们并不相信它们会有影响力于绝大多数实际使用。</span><span class="sxs-lookup"><span data-stu-id="35890-163">And while the performance benefits show up in microbenchmarks, we don't believe they'll be impactful in the vast majority of real usage.</span></span>  <span data-ttu-id="35890-164">如果出现这种情况，我们可以用一种全新的方式引入一组接口。</span><span class="sxs-lookup"><span data-stu-id="35890-164">If it turns out they are, we can introduce a second set of interfaces in a light-up fashion.</span></span>

<span data-ttu-id="35890-165">丢弃的选项：</span><span class="sxs-lookup"><span data-stu-id="35890-165">Discarded options considered:</span></span>
- <span data-ttu-id="35890-166">`ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`： `out` 参数不能是协变的。</span><span class="sxs-lookup"><span data-stu-id="35890-166">`ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`: `out` parameters can't be covariant.</span></span>  <span data-ttu-id="35890-167">这里也有一个小的影响（一般的试用模式问题），这可能会导致引用类型结果出现运行时写入屏障。</span><span class="sxs-lookup"><span data-stu-id="35890-167">There's also a small impact here (an issue with the try pattern in general) that this likely incurs a runtime write barrier for reference type results.</span></span>

#### <a name="cancellation"></a><span data-ttu-id="35890-168">取消</span><span class="sxs-lookup"><span data-stu-id="35890-168">Cancellation</span></span>

<span data-ttu-id="35890-169">可以通过多种方法来支持取消：</span><span class="sxs-lookup"><span data-stu-id="35890-169">There are several possible approaches to supporting cancellation:</span></span>
1. <span data-ttu-id="35890-170">`IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` 是取消无关的： `CancellationToken` 不会出现在任何位置。</span><span class="sxs-lookup"><span data-stu-id="35890-170">`IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` are cancellation-agnostic: `CancellationToken` doesn't appear anywhere.</span></span>  <span data-ttu-id="35890-171">通过以任何适当的方式将 `CancellationToken` 以逻辑方式收录到可枚举的和/或枚举器来实现取消，例如，在调用迭代器时，将 `CancellationToken` 作为参数传递给迭代器方法并在迭代器的正文中使用它，就像使用任何其他参数完成的操作一样。</span><span class="sxs-lookup"><span data-stu-id="35890-171">Cancellation is achieved by logically baking the `CancellationToken` into the enumerable and/or enumerator in whatever manner is appropriate, e.g. when calling an iterator, passing the `CancellationToken` as an argument to the iterator method and using it in the body of the iterator, as is done with any other parameter.</span></span>
2. <span data-ttu-id="35890-172">`IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`：将 `CancellationToken` 传递到 `GetAsyncEnumerator`，并且后续的 `MoveNextAsync` 操作可以遵守此操作。</span><span class="sxs-lookup"><span data-stu-id="35890-172">`IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`: You pass a `CancellationToken` to `GetAsyncEnumerator`, and subsequent `MoveNextAsync` operations respect it however it can.</span></span>
3. <span data-ttu-id="35890-173">`IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`：向每个单独的 `MoveNextAsync` 调用传递一个 `CancellationToken`。</span><span class="sxs-lookup"><span data-stu-id="35890-173">`IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`: You pass a `CancellationToken` to each individual `MoveNextAsync` call.</span></span>
4. <span data-ttu-id="35890-174">1 & & 2：将 `CancellationToken`嵌入到可枚举/枚举器，并将 `CancellationToken`传递到 `GetAsyncEnumerator`。</span><span class="sxs-lookup"><span data-stu-id="35890-174">1 && 2: You both embed `CancellationToken`s into your enumerable/enumerator and pass `CancellationToken`s into `GetAsyncEnumerator`.</span></span>
5. <span data-ttu-id="35890-175">1 & & 3：将 `CancellationToken`嵌入到可枚举/枚举器，并将 `CancellationToken`传递到 `MoveNextAsync`。</span><span class="sxs-lookup"><span data-stu-id="35890-175">1 && 3: You both embed `CancellationToken`s into your enumerable/enumerator and pass `CancellationToken`s into `MoveNextAsync`.</span></span>

<span data-ttu-id="35890-176">从纯粹的理论角度来看，（5）是最强大的，在这种情况下（a） `MoveNextAsync` 接受 `CancellationToken` 可对取消的内容进行最精细的控制，而（b） `CancellationToken` 只是可以作为参数传递到迭代器中的任何其他类型，如嵌入到任意类型等。</span><span class="sxs-lookup"><span data-stu-id="35890-176">From a purely theoretical perspective, (5) is the most robust, in that (a) `MoveNextAsync` accepting a `CancellationToken` enables the most fine-grained control over what's canceled, and (b) `CancellationToken` is just any other type that can passed as an argument into iterators, embedded in arbitrary types, etc.</span></span>

<span data-ttu-id="35890-177">但是，该方法有多个问题：</span><span class="sxs-lookup"><span data-stu-id="35890-177">However, there are multiple problems with that approach:</span></span>
- <span data-ttu-id="35890-178">如何将 `CancellationToken` 传递给 `GetAsyncEnumerator` 使其成为迭代器的主体？</span><span class="sxs-lookup"><span data-stu-id="35890-178">How does a `CancellationToken` passed to `GetAsyncEnumerator` make it into the body of the iterator?</span></span>  <span data-ttu-id="35890-179">我们可以公开一个新的 `iterator` 关键字，您可以从该关键字开始，以访问传递到 `GetEnumerator`的 `CancellationToken`。但这是一个额外的机制，b）我们将它变成一个非常先进的公民，c），99% 的事例似乎与调用迭代器并对其调用 `GetAsyncEnumerator` 的代码相同，在这种情况下，它只需将 `CancellationToken` 作为参数传递到方法中。</span><span class="sxs-lookup"><span data-stu-id="35890-179">We could expose a new `iterator` keyword that you could dot off of to get access to the `CancellationToken` passed to `GetEnumerator`, but a) that's a lot of additional machinery, b) we're making it a very first-class citizen, and c) the 99% case would seem to be the same code both calling an iterator and calling `GetAsyncEnumerator` on it, in which case it can just pass the `CancellationToken` as an argument into the method.</span></span>
- <span data-ttu-id="35890-180">如何将 `CancellationToken` 传递到 `MoveNextAsync` 进入方法体？</span><span class="sxs-lookup"><span data-stu-id="35890-180">How does a `CancellationToken` passed to `MoveNextAsync` get into the body of the method?</span></span>  <span data-ttu-id="35890-181">这种情况更糟，就像它是从 `iterator` 本地对象公开的一样，其值可以在等待期间更改，这意味着，使用令牌注册的任何代码都需要在等待之前从其注销，然后在之后重新注册;不管是由编译器在迭代器中实现还是由开发人员手动实现，都需要在每个 `MoveNextAsync` 调用中进行此类注册和注销。</span><span class="sxs-lookup"><span data-stu-id="35890-181">This is even worse, as if it's exposed off of an `iterator` local object, its value could change across awaits, which means any code that registered with the token would need to unregister from it prior to awaits and then re-register after; it's also potentially quite expensive to need to do such registering and unregistering in every `MoveNextAsync` call, regardless of whether implemented by the compiler in an iterator or by a developer manually.</span></span>
- <span data-ttu-id="35890-182">开发人员如何取消 `foreach` 循环？</span><span class="sxs-lookup"><span data-stu-id="35890-182">How does a developer cancel a `foreach` loop?</span></span>  <span data-ttu-id="35890-183">如果是通过向可枚举/枚举器提供 `CancellationToken` 来完成的，则这两个操作都需要支持 `foreach`的对枚举器的运算，这会将它们提升为第一类公民，现在你需要考虑围绕枚举器（例如 LINQ 方法）或 b 构建的生态系统，因此，我们需要将 `CancellationToken` 嵌入到可枚举的中，这是因为在将存储所提供的令牌的 `IAsyncEnumerable<T>`，并在返回的结构上的 `GetAsyncEnumerator` `WithCancellation` `GetAsyncEnumerator`被调用（忽略该标记）。</span><span class="sxs-lookup"><span data-stu-id="35890-183">If it's done by giving a `CancellationToken` to an enumerable/enumerator, then either a) we need to support `foreach`'ing over enumerators, which raises them to being first-class citizens, and now you need to start thinking about an ecosystem built up around enumerators (e.g. LINQ methods) or b) we need to embed the `CancellationToken` in the enumerable anyway by having some `WithCancellation` extension method off of `IAsyncEnumerable<T>` that would store the provided token and then pass it into  the wrapped enumerable's `GetAsyncEnumerator` when the `GetAsyncEnumerator` on the returned struct is invoked (ignoring that token).</span></span>  <span data-ttu-id="35890-184">或者，您可以只使用在 foreach 的主体中的 `CancellationToken`。</span><span class="sxs-lookup"><span data-stu-id="35890-184">Or, you can just use the `CancellationToken` you have in the body of the foreach.</span></span>
- <span data-ttu-id="35890-185">如果支持查询理解，则如何向每个子句传递 `GetEnumerator` 或 `MoveNextAsync` 的 `CancellationToken`？</span><span class="sxs-lookup"><span data-stu-id="35890-185">If/when query comprehensions are supported, how would the `CancellationToken` supplied to `GetEnumerator` or `MoveNextAsync` be passed into each clause?</span></span>  <span data-ttu-id="35890-186">最简单的方法是让子句捕获该语句，此时将忽略任何标记传递到 `GetAsyncEnumerator`/`MoveNextAsync` 会被忽略。</span><span class="sxs-lookup"><span data-stu-id="35890-186">The easiest way would simply be for the clause to capture it, at which point whatever token is passed to `GetAsyncEnumerator`/`MoveNextAsync` is ignored.</span></span>

<span data-ttu-id="35890-187">此文档的较早版本（1），但我们已切换到（4）。</span><span class="sxs-lookup"><span data-stu-id="35890-187">An earlier version of this document recommended (1), but we since switched to (4).</span></span>

<span data-ttu-id="35890-188">（1）主要有两个问题：</span><span class="sxs-lookup"><span data-stu-id="35890-188">The two main problems with (1):</span></span>
- <span data-ttu-id="35890-189">可取消枚举的创建者必须实现一些样本，并且只能利用编译器对异步迭代器的支持来实现 `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)` 方法。</span><span class="sxs-lookup"><span data-stu-id="35890-189">producers of cancellable enumerables have to implement some boilerplate, and can only leverage the compiler's support for async-iterators to implement a `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)` method.</span></span>
- <span data-ttu-id="35890-190">很多的创建者可能只是将 `CancellationToken` 参数添加到其异步可枚举签名，而这会阻止使用者在给定 `IAsyncEnumerable` 类型时传递所需的取消令牌。</span><span class="sxs-lookup"><span data-stu-id="35890-190">it is likely that many producers would be tempted to just add a `CancellationToken` parameter to their async-enumerable signature instead, which will prevent consumers from passing the cancellation token they want when they are given an `IAsyncEnumerable` type.</span></span>

<span data-ttu-id="35890-191">有两种主要的消耗方案：</span><span class="sxs-lookup"><span data-stu-id="35890-191">There are two main consumption scenarios:</span></span>
1. <span data-ttu-id="35890-192">`await foreach (var i in GetData(token)) ...` 使用者调用异步迭代器方法的位置，</span><span class="sxs-lookup"><span data-stu-id="35890-192">`await foreach (var i in GetData(token)) ...` where the consumer calls the async-iterator method,</span></span>
2. <span data-ttu-id="35890-193">`await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...` 使用者处理给定 `IAsyncEnumerable` 实例的位置。</span><span class="sxs-lookup"><span data-stu-id="35890-193">`await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...` where the consumer deals with a given `IAsyncEnumerable` instance.</span></span>

<span data-ttu-id="35890-194">我们发现一种合理的折衷方法是以一种可方便地使用异步流的生成者和使用者的方式来支持这两种方案。</span><span class="sxs-lookup"><span data-stu-id="35890-194">We find that a reasonable compromise to support both scenarios in a way that is convenient for both producers and consumers of async-streams is to use a specially annotated parameter in the async-iterator method.</span></span> <span data-ttu-id="35890-195">`[EnumeratorCancellation]` 特性用于实现此目的。</span><span class="sxs-lookup"><span data-stu-id="35890-195">The `[EnumeratorCancellation]` attribute is used for this purpose.</span></span> <span data-ttu-id="35890-196">如果将此属性放置在参数上，则会告诉编译器如果将令牌传递到 `GetAsyncEnumerator` 方法，则应使用该令牌，而不是最初为参数传递的值。</span><span class="sxs-lookup"><span data-stu-id="35890-196">Placing this attribute on a parameter tells the compiler that if a token is passed to the `GetAsyncEnumerator` method, that token should be used instead of the value originally passed for the parameter.</span></span>

<span data-ttu-id="35890-197">以 `IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)` 为例。</span><span class="sxs-lookup"><span data-stu-id="35890-197">Consider `IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)`.</span></span> <span data-ttu-id="35890-198">此方法的实施者只需使用方法体中的参数即可。</span><span class="sxs-lookup"><span data-stu-id="35890-198">The implementer of this method can simply use the parameter in the method body.</span></span> <span data-ttu-id="35890-199">使用者可以使用上面的两种消费模式：</span><span class="sxs-lookup"><span data-stu-id="35890-199">The consumer can use either consumption patterns above:</span></span>
1. <span data-ttu-id="35890-200">如果使用 `GetData(token)`，则令牌将保存到 async 可枚举中，并在迭代中使用。</span><span class="sxs-lookup"><span data-stu-id="35890-200">if you use `GetData(token)`, then the token is saved into the async-enumerable and will be used in iteration,</span></span>
2. <span data-ttu-id="35890-201">如果使用 `givenIAsyncEnumerable.WithCancellation(token)`，则传递到 `GetAsyncEnumerator` 的令牌将取代所有保存在异步可枚举中的令牌。</span><span class="sxs-lookup"><span data-stu-id="35890-201">if you use `givenIAsyncEnumerable.WithCancellation(token)`, then the token passed to `GetAsyncEnumerator` will supersede any token saved in the async-enumerable.</span></span>

## <a name="foreach"></a><span data-ttu-id="35890-202">foreach</span><span class="sxs-lookup"><span data-stu-id="35890-202">foreach</span></span>

<span data-ttu-id="35890-203">`foreach` 将进一步补充，以支持 `IAsyncEnumerable<T>` 以及其对 `IEnumerable<T>`的现有支持。</span><span class="sxs-lookup"><span data-stu-id="35890-203">`foreach` will be augmented to support `IAsyncEnumerable<T>` in addition to its existing support for `IEnumerable<T>`.</span></span>  <span data-ttu-id="35890-204">如果相关成员公开公开后，它将支持将 `IAsyncEnumerable<T>` 视为一种模式，以便启用基于结构的扩展，以避免分配，并使用替代的等待作为 `MoveNextAsync` 和 `DisposeAsync`的返回类型。</span><span class="sxs-lookup"><span data-stu-id="35890-204">And it will support the equivalent of `IAsyncEnumerable<T>` as a pattern if the relevant members are exposed publicly, falling back to using the interface directly if not, in order to enable struct-based extensions that avoid allocating as well as using alternative awaitables as the return type of `MoveNextAsync` and `DisposeAsync`.</span></span>

### <a name="syntax"></a><span data-ttu-id="35890-205">语法</span><span class="sxs-lookup"><span data-stu-id="35890-205">Syntax</span></span>

<span data-ttu-id="35890-206">使用语法：</span><span class="sxs-lookup"><span data-stu-id="35890-206">Using the syntax:</span></span>

```csharp
foreach (var i in enumerable)
```

<span data-ttu-id="35890-207">C#将继续将 `enumerable` 视为同步可枚举，这样即使它公开了用于 async 枚举的相关 Api （公开模式或实现接口），它也只会考虑同步 Api。</span><span class="sxs-lookup"><span data-stu-id="35890-207">C# will continue to treat `enumerable` as a synchronous enumerable, such that even if it exposes the relevant APIs for async enumerables (exposing the pattern or implementing the interface), it will only consider the synchronous APIs.</span></span>

<span data-ttu-id="35890-208">若要强制 `foreach` 改为仅考虑异步 Api，请按如下所示插入 `await`：</span><span class="sxs-lookup"><span data-stu-id="35890-208">To force `foreach` to instead only consider the asynchronous APIs, `await` is inserted as follows:</span></span>

```csharp
await foreach (var i in enumerable)
```

<span data-ttu-id="35890-209">不会提供支持使用 async 或 sync Api 的语法;开发人员必须根据所使用的语法进行选择。</span><span class="sxs-lookup"><span data-stu-id="35890-209">No syntax would be provided that would support using either the async or the sync APIs; the developer must choose based on the syntax used.</span></span>

<span data-ttu-id="35890-210">丢弃的选项：</span><span class="sxs-lookup"><span data-stu-id="35890-210">Discarded options considered:</span></span>
- <span data-ttu-id="35890-211">_`foreach (var i in await enumerable)`_ ：这是有效的语法，并且更改其含义会是一项重大更改。</span><span class="sxs-lookup"><span data-stu-id="35890-211">_`foreach (var i in await enumerable)`_: This is already valid syntax, and changing its meaning would be a breaking change.</span></span>  <span data-ttu-id="35890-212">这意味着要 `await` `enumerable`，从其返回可迭代同步的内容，然后以同步方式循环访问。</span><span class="sxs-lookup"><span data-stu-id="35890-212">This means to `await` the `enumerable`, get back something synchronously iterable from it, and then synchronously iterate through that.</span></span>
- <span data-ttu-id="35890-213">_`foreach (var i await in enumerable)`，`foreach (var await i in enumerable)`，`foreach (await var i in enumerable)`_ ：这一切都表示我们正在等待下一项，但在 foreach 中涉及其他等待，特别是如果可枚举的是 `IAsyncDisposable`，我们将 `await`其异步处理。</span><span class="sxs-lookup"><span data-stu-id="35890-213">_`foreach (var i await in enumerable)`, `foreach (var await i in enumerable)`, `foreach (await var i in enumerable)`_: These all suggest that we're awaiting the next item, but there are other awaits involved in foreach, in particular if the enumerable is an `IAsyncDisposable`, we will be `await`'ing its async disposal.</span></span>  <span data-ttu-id="35890-214">该 await 作为 foreach 的作用域，而不是每个单个元素的作用域，因此 `await` 关键字一定处于 `foreach` 级别。</span><span class="sxs-lookup"><span data-stu-id="35890-214">That await is as the scope of the foreach rather than for each individual element, and thus the `await` keyword deserves to be at the `foreach` level.</span></span>  <span data-ttu-id="35890-215">此外，将其与 `foreach` 相关联为我们提供了一种用不同的术语（例如 "await foreach"）描述 `foreach` 的方法。</span><span class="sxs-lookup"><span data-stu-id="35890-215">Further, having it associated with the `foreach` gives us a way to describe the `foreach` with a different term, e.g. a "await foreach".</span></span>  <span data-ttu-id="35890-216">但更重要的是，在考虑 `foreach` 语法 `using` 语法的同时，还存在一些价值，以便它们彼此保持一致，`using (await ...)` 已经是有效的语法。</span><span class="sxs-lookup"><span data-stu-id="35890-216">But more importantly, there's value in considering `foreach` syntax at the same time as `using` syntax, so that they remain consistent with each other, and `using (await ...)` is already valid syntax.</span></span>
- _`foreach await (var i in enumerable)`_

<span data-ttu-id="35890-217">仍要考虑：</span><span class="sxs-lookup"><span data-stu-id="35890-217">Still to consider:</span></span>
- <span data-ttu-id="35890-218">目前 `foreach` 不支持循环访问枚举器。</span><span class="sxs-lookup"><span data-stu-id="35890-218">`foreach` today does not support iterating through an enumerator.</span></span>  <span data-ttu-id="35890-219">我们预计 `IAsyncEnumerator<T>`就会更常见，因此，`IAsyncEnumerable<T>` 和 `IAsyncEnumerator<T>`都可以支持 `await foreach`。</span><span class="sxs-lookup"><span data-stu-id="35890-219">We expect it will be more common to have `IAsyncEnumerator<T>`s handed around, and thus it's tempting to support `await foreach` with both `IAsyncEnumerable<T>` and `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="35890-220">但一旦我们添加了此类支持，它就会引入 `IAsyncEnumerator<T>` 是否是第一类公民的问题，以及是否需要对枚举器进行除枚举以外的组合器重载？</span><span class="sxs-lookup"><span data-stu-id="35890-220">But once we add such support, it introduces the question of whether `IAsyncEnumerator<T>` is a first-class citizen, and whether we need to have overloads of combinators that operate on enumerators in addition to enumerables?</span></span>    <span data-ttu-id="35890-221">我们想要鼓励方法返回枚举器而不是枚举？</span><span class="sxs-lookup"><span data-stu-id="35890-221">Do we want to encourage methods to return enumerators rather than enumerables?</span></span> <span data-ttu-id="35890-222">我们应该继续讨论这一点。</span><span class="sxs-lookup"><span data-stu-id="35890-222">We should continue to discuss this.</span></span>  <span data-ttu-id="35890-223">如果我们决定不想对其提供支持，我们可能希望引入 `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);` 的扩展方法，该方法允许枚举器仍 `foreach`。</span><span class="sxs-lookup"><span data-stu-id="35890-223">If we decide we don't want to support it, we might want to introduce an extension method `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);` that would allow an enumerator to still be `foreach`'d.</span></span>  <span data-ttu-id="35890-224">如果我们决定要支持，则还需要决定 `await foreach` 是否负责调用枚举器上的 `DisposeAsync`，以及答案是否为 "不，控制过度处置应由调用 `GetEnumerator`的人员处理"。</span><span class="sxs-lookup"><span data-stu-id="35890-224">If we decide we do want to support it, we'll need to also decide on whether the `await foreach` would be responsible for calling `DisposeAsync` on the enumerator, and the answer is likely "no, control over disposal should be handled by whoever called `GetEnumerator`."</span></span>

### <a name="pattern-based-compilation"></a><span data-ttu-id="35890-225">基于模式的编译</span><span class="sxs-lookup"><span data-stu-id="35890-225">Pattern-based Compilation</span></span>

<span data-ttu-id="35890-226">编译器将绑定到基于模式的 Api （如果存在），并首选使用接口（使用实例方法或扩展方法可满足此模式）。</span><span class="sxs-lookup"><span data-stu-id="35890-226">The compiler will bind to the pattern-based APIs if they exist, preferring those over using the interface (the pattern may be satisfied with instance methods or extension methods).</span></span>  <span data-ttu-id="35890-227">模式的要求如下：</span><span class="sxs-lookup"><span data-stu-id="35890-227">The requirements for the pattern are:</span></span>
- <span data-ttu-id="35890-228">可枚举必须公开一个 `GetAsyncEnumerator` 方法，该方法可在没有参数的情况调用，并返回一个满足相关模式的枚举器。</span><span class="sxs-lookup"><span data-stu-id="35890-228">The enumerable must expose a `GetAsyncEnumerator` method that may be called with no arguments and that returns an enumerator that meets the relevant pattern.</span></span>
- <span data-ttu-id="35890-229">枚举器必须公开一个 `MoveNextAsync` 方法，该方法可在不使用任何参数的情况调用，并且返回可能在 `await`ed 并且其 `GetResult()` 返回 `bool`的内容。</span><span class="sxs-lookup"><span data-stu-id="35890-229">The enumerator must expose a `MoveNextAsync` method that may be called with no arguments and that returns something which may be `await`ed and whose `GetResult()` returns a `bool`.</span></span>
- <span data-ttu-id="35890-230">枚举器还必须公开 `Current` 属性，该属性的 getter 返回一个表示所枚举的数据类型的 `T`。</span><span class="sxs-lookup"><span data-stu-id="35890-230">The enumerator must also expose `Current` property whose getter returns a `T` representing the kind of data being enumerated.</span></span>
- <span data-ttu-id="35890-231">枚举器可以选择性地公开 `DisposeAsync` 方法，该方法可在不使用任何参数的情况调用，并返回可 `await`ed 并且 `GetResult()` 返回 `void`的内容。</span><span class="sxs-lookup"><span data-stu-id="35890-231">The enumerator may optionally expose a `DisposeAsync` method that may be invoked with no arguments and that returns something that can be `await`ed and whose `GetResult()` returns `void`.</span></span>

<span data-ttu-id="35890-232">此代码：</span><span class="sxs-lookup"><span data-stu-id="35890-232">This code:</span></span>

```csharp
var enumerable = ...;
await foreach (T item in enumerable)
{
   ...
}
```

<span data-ttu-id="35890-233">转换为的等效项：</span><span class="sxs-lookup"><span data-stu-id="35890-233">is translated to the equivalent of:</span></span>

```csharp
var enumerable = ...;
var enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
       T item = enumerator.Current;
       ...
    }
}
finally
{
    await enumerator.DisposeAsync(); // omitted, along with the try/finally, if the enumerator doesn't expose DisposeAsync
}
```

<span data-ttu-id="35890-234">如果迭代类型未公开正确的模式，则将使用这些接口。</span><span class="sxs-lookup"><span data-stu-id="35890-234">If the iterated type doesn't expose the right pattern, the interfaces will be used.</span></span>

### <a name="configureawait"></a><span data-ttu-id="35890-235">ConfigureAwait</span><span class="sxs-lookup"><span data-stu-id="35890-235">ConfigureAwait</span></span>

<span data-ttu-id="35890-236">此基于模式的编译允许通过 `ConfigureAwait` 扩展方法在所有等待中使用 `ConfigureAwait`：</span><span class="sxs-lookup"><span data-stu-id="35890-236">This pattern-based compilation will allow `ConfigureAwait` to be used on all of the awaits, via a `ConfigureAwait` extension method:</span></span>

```csharp
await foreach (T item in enumerable.ConfigureAwait(false))
{
   ...
}
```

<span data-ttu-id="35890-237">这将基于我们还将添加到 .NET 的类型（可能为 ""）：</span><span class="sxs-lookup"><span data-stu-id="35890-237">This will be based on types we'll add to .NET as well, likely to System.Threading.Tasks.Extensions.dll:</span></span>

```csharp
// Approximate implementation, omitting arg validation and the like
namespace System.Threading.Tasks
{
    public static class AsyncEnumerableExtensions
    {
        public static ConfiguredAsyncEnumerable<T> ConfigureAwait<T>(this IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext) =>
            new ConfiguredAsyncEnumerable<T>(enumerable, continueOnCapturedContext);

        public struct ConfiguredAsyncEnumerable<T>
        {
            private readonly IAsyncEnumerable<T> _enumerable;
            private readonly bool _continueOnCapturedContext;

            internal ConfiguredAsyncEnumerable(IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext)
            {
                _enumerable = enumerable;
                _continueOnCapturedContext = continueOnCapturedContext;
            }

            public ConfiguredAsyncEnumerator<T> GetAsyncEnumerator() =>
                new ConfiguredAsyncEnumerator<T>(_enumerable.GetAsyncEnumerator(), _continueOnCapturedContext);

            public struct Enumerator
            {
                private readonly IAsyncEnumerator<T> _enumerator;
                private readonly bool _continueOnCapturedContext;

                internal Enumerator(IAsyncEnumerator<T> enumerator, bool continueOnCapturedContext)
                {
                    _enumerator = enumerator;
                    _continueOnCapturedContext = continueOnCapturedContext;
                }

                public ConfiguredValueTaskAwaitable<bool> MoveNextAsync() =>
                    _enumerator.MoveNextAsync().ConfigureAwait(_continueOnCapturedContext);

                public T Current => _enumerator.Current;

                public ConfiguredValueTaskAwaitable DisposeAsync() =>
                    _enumerator.DisposeAsync().ConfigureAwait(_continueOnCapturedContext);
            }
        }
    }
}
```

<span data-ttu-id="35890-238">请注意，此方法不会允许 `ConfigureAwait` 与基于模式的枚举一起使用。但再次强调，`ConfigureAwait` 仅作为 `Task`/上的扩展公开 `Task<T>`/`ValueTask`/`ValueTask<T>` 并且不能应用于任意可等待的操作，因为它仅在应用于任务时才有意义（它控制在任务的继续支持中实现的行为），因此，在使用可等待的情况下，这种情况在不能是任务时是有意义的。</span><span class="sxs-lookup"><span data-stu-id="35890-238">Note that this approach will not enable `ConfigureAwait` to be used with pattern-based enumerables, but then again it's already the case that the `ConfigureAwait` is only exposed as an extension on `Task`/`Task<T>`/`ValueTask`/`ValueTask<T>` and can't be applied to arbitrary awaitable things, as it only makes sense when applied to Tasks (it controls a behavior implemented in Task's continuation support), and thus doesn't make sense when using a pattern where the awaitable things may not be tasks.</span></span>  <span data-ttu-id="35890-239">返回可等待的任何人都可以在此类高级方案中提供自己的自定义行为。</span><span class="sxs-lookup"><span data-stu-id="35890-239">Anyone returning awaitable things can provide their own custom behavior in such advanced scenarios.</span></span>

<span data-ttu-id="35890-240">（如果我们可以通过某种方式来支持作用域或程序集级别的 `ConfigureAwait` 解决方案，则不需要这样做。）</span><span class="sxs-lookup"><span data-stu-id="35890-240">(If we can come up with some way to support a scope- or assembly-level `ConfigureAwait` solution, then this won't be necessary.)</span></span>

## <a name="async-iterators"></a><span data-ttu-id="35890-241">异步迭代器</span><span class="sxs-lookup"><span data-stu-id="35890-241">Async Iterators</span></span>

<span data-ttu-id="35890-242">语言/编译器除了使用它们外，还支持生成 `IAsyncEnumerable<T>`和 `IAsyncEnumerator<T>`。</span><span class="sxs-lookup"><span data-stu-id="35890-242">The language / compiler will support producing `IAsyncEnumerable<T>`s and `IAsyncEnumerator<T>`s in addition to consuming them.</span></span> <span data-ttu-id="35890-243">如今，该语言支持编写迭代器，如下所示：</span><span class="sxs-lookup"><span data-stu-id="35890-243">Today the language supports writing an iterator like:</span></span>

```csharp
static IEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            Thread.Sleep(1000);
            yield return i;
        }
    }
    finally
    {
        Thread.Sleep(200);
        Console.WriteLine("finally");
    }
}
```

<span data-ttu-id="35890-244">但不能在这些迭代器的正文中使用 `await`。</span><span class="sxs-lookup"><span data-stu-id="35890-244">but `await` can't be used in the body of these iterators.</span></span>  <span data-ttu-id="35890-245">我们将添加该支持。</span><span class="sxs-lookup"><span data-stu-id="35890-245">We will add that support.</span></span>

### <a name="syntax"></a><span data-ttu-id="35890-246">语法</span><span class="sxs-lookup"><span data-stu-id="35890-246">Syntax</span></span>

<span data-ttu-id="35890-247">迭代器的现有语言支持根据方法是否包含任何 `yield`推断方法的迭代器特性。</span><span class="sxs-lookup"><span data-stu-id="35890-247">The existing language support for iterators infers the iterator nature of the method based on whether it contains any `yield`s.</span></span>  <span data-ttu-id="35890-248">对于异步迭代器，情况也是如此。</span><span class="sxs-lookup"><span data-stu-id="35890-248">The same will be true for async iterators.</span></span>  <span data-ttu-id="35890-249">此类异步迭代器将 demarcated，并与同步迭代器进行区分，方法是将 `async` 添加到签名，然后必须将 `IAsyncEnumerable<T>` 或 `IAsyncEnumerator<T>` 为其返回类型。</span><span class="sxs-lookup"><span data-stu-id="35890-249">Such async iterators will be demarcated and differentiated from synchronous iterators via adding `async` to the signature, and must then also have either `IAsyncEnumerable<T>` or `IAsyncEnumerator<T>` as its return type.</span></span>  <span data-ttu-id="35890-250">例如，可以将上面的示例编写为异步迭代器，如下所示：</span><span class="sxs-lookup"><span data-stu-id="35890-250">For example, the above example could be written as an async iterator as follows:</span></span>

```csharp
static async IAsyncEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            await Task.Delay(1000);
            yield return i;
        }
    }
    finally
    {
        await Task.Delay(200);
        Console.WriteLine("finally");
    }
}
```

<span data-ttu-id="35890-251">考虑的替代方案：</span><span class="sxs-lookup"><span data-stu-id="35890-251">Alternatives considered:</span></span>
- <span data-ttu-id="35890-252">在_签名中不使用 `async`_ ：编译器在技术上可能需要使用 `async`，因为它使用它来确定 `await` 在该上下文中是否有效。</span><span class="sxs-lookup"><span data-stu-id="35890-252">_Not using `async` in the signature_: Using `async` is likely technically required by the compiler, as it uses it to determine whether `await` is valid in that context.</span></span>  <span data-ttu-id="35890-253">但即使不是必需的，我们也已经建立 `await` 只能在标记为 `async`的方法中使用，因此保持一致性非常重要。</span><span class="sxs-lookup"><span data-stu-id="35890-253">But even if it's not required, we've established that `await` may only be used in methods marked as `async`, and it seems important to keep the consistency.</span></span>
- <span data-ttu-id="35890-254">为 _`IAsyncEnumerable<T>`启用自定义生成器_：这就是我们将来要探讨的事情，但这是一项非常复杂的事情，我们不支持同步的对应项。</span><span class="sxs-lookup"><span data-stu-id="35890-254">_Enabling custom builders for `IAsyncEnumerable<T>`_:  That's something we could look at for the future, but the machinery is complicated and we don't support that for the synchronous counterparts.</span></span>
- <span data-ttu-id="35890-255">_签名中具有 `iterator` 关键字_：异步迭代器将在签名中使用 `async iterator`，`yield` 只能在包含 `iterator`的 `async` 方法中使用;然后，`iterator` 在同步迭代器上是可选的。</span><span class="sxs-lookup"><span data-stu-id="35890-255">_Having an `iterator` keyword in the signature_: Async iterators would use `async iterator` in the signature, and `yield` could only be used in `async` methods that included `iterator`; `iterator` would then be made optional on synchronous iterators.</span></span>  <span data-ttu-id="35890-256">根据您的观点，这一优点在于，无论是否允许 `yield`，该方法的签名都可以很清楚地确定 `IAsyncEnumerable<T>` 类型的实例，而不是根据代码是否使用 `yield` 来返回编译器。</span><span class="sxs-lookup"><span data-stu-id="35890-256">Depending on your perspective, this has the benefit of making it very clear by the signature of the method whether `yield` is allowed and whether the method is actually meant to return instances of type `IAsyncEnumerable<T>` rather than the compiler manufacturing one based on whether the code uses `yield` or not.</span></span>  <span data-ttu-id="35890-257">但这不同于同步迭代器，而不能对其进行要求。</span><span class="sxs-lookup"><span data-stu-id="35890-257">But it is different from synchronous iterators, which don't and can't be made to require one.</span></span>  <span data-ttu-id="35890-258">此外，某些开发人员不喜欢额外的语法。</span><span class="sxs-lookup"><span data-stu-id="35890-258">Plus some developers don't like the extra syntax.</span></span>  <span data-ttu-id="35890-259">如果我们是从头开始设计的，则可能需要这样做，但此时，将异步迭代器保持在同步迭代器附近会有更多的价值。</span><span class="sxs-lookup"><span data-stu-id="35890-259">If we were designing it from scratch, we'd probably make this required, but at this point there's much more value in keeping async iterators close to sync iterators.</span></span>

## <a name="linq"></a><span data-ttu-id="35890-260">LINQ</span><span class="sxs-lookup"><span data-stu-id="35890-260">LINQ</span></span>

<span data-ttu-id="35890-261">`System.Linq.Enumerable` 类上有超过200的方法重载，所有这些重载都是 `IEnumerable<T>`的;其中一些接受 `IEnumerable<T>`，其中一些会生成 `IEnumerable<T>`，许多都是这样的。</span><span class="sxs-lookup"><span data-stu-id="35890-261">There are over ~200 overloads of methods on the `System.Linq.Enumerable` class, all of which work in terms of `IEnumerable<T>`; some of these accept `IEnumerable<T>`, some of them produce `IEnumerable<T>`, and many do both.</span></span>  <span data-ttu-id="35890-262">为 `IAsyncEnumerable<T>` 添加 LINQ 支持可能需要为其复制所有这些重载，为另一个 ~ 200。</span><span class="sxs-lookup"><span data-stu-id="35890-262">Adding LINQ support for `IAsyncEnumerable<T>` would likely entail duplicating all of these overloads for it, for another ~200.</span></span>  <span data-ttu-id="35890-263">由于 `IAsyncEnumerator<T>` 可能比 `IEnumerator<T>` 在同步领域中的独立实体更常见，因此，可能需要另一 ~ 200 重载来处理 `IAsyncEnumerator<T>`。</span><span class="sxs-lookup"><span data-stu-id="35890-263">And since `IAsyncEnumerator<T>` is likely to be more common as a standalone entity in the asynchronous world than `IEnumerator<T>` is in the synchronous world, we could potentially need another ~200 overloads that work with `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="35890-264">此外，大量重载处理谓词（例如，使用 `Func<T, bool>`的 `Where`），并且可能需要具有处理同步和异步谓词的基于 `IAsyncEnumerable<T>`的重载（`Func<T, ValueTask<bool>>` 例如，除了 `Func<T, bool>`之外）。</span><span class="sxs-lookup"><span data-stu-id="35890-264">Plus, a large number of the overloads deal with predicates (e.g. `Where` that takes a `Func<T, bool>`), and it may be desirable to have `IAsyncEnumerable<T>`-based overloads that deal with both synchronous and asynchronous predicates (e.g. `Func<T, ValueTask<bool>>` in addition to `Func<T, bool>`).</span></span>  <span data-ttu-id="35890-265">虽然这并不适用于所有现在 ~ 400 的新重载，但大致的计算是它适用于一半，这意味着另一个 ~ 200 重载，总共 ~ 600 个新方法。</span><span class="sxs-lookup"><span data-stu-id="35890-265">While this isn't applicable to all of the now ~400 new overloads, a rough calculation is that it'd be applicable to half, which means another ~200 overloads, for a total of ~600 new methods.</span></span>

<span data-ttu-id="35890-266">这是一种惊人的 Api，在考虑到诸如交互式扩展（Ix）这样的扩展库时，可能会出现更多的情况。</span><span class="sxs-lookup"><span data-stu-id="35890-266">That is a staggering number of APIs, with the potential for even more when extension libraries like Interactive Extensions (Ix) are considered.</span></span>  <span data-ttu-id="35890-267">但 Ix 已经实现了其中的许多，但看起来不是复制该工作的重要原因;当开发人员想要将 LINQ 与 `IAsyncEnumerable<T>`结合使用时，我们应帮助社区改进 Ix 并建议使用它。</span><span class="sxs-lookup"><span data-stu-id="35890-267">But Ix already has an implementation of many of these, and there doesn't seem to be a great reason to duplicate that work; we should instead help the community improve Ix and recommend it for when developers want to use LINQ with `IAsyncEnumerable<T>`.</span></span>

<span data-ttu-id="35890-268">此外，还存在与查询理解语法有关的问题。</span><span class="sxs-lookup"><span data-stu-id="35890-268">There is also the issue of query comprehension syntax.</span></span>  <span data-ttu-id="35890-269">查询理解的基于模式的性质使其能够 "只使用" 一些运算符，例如，如果 Ix 提供以下方法：</span><span class="sxs-lookup"><span data-stu-id="35890-269">The pattern-based nature of query comprehensions would allow them to "just work" with some operators, e.g. if Ix provides the following methods:</span></span>

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, TResult> func);
public static IAsyncEnumerable<T> Where(this IAsyncEnumerable<T> source, Func<T, bool> func);
```

<span data-ttu-id="35890-270">此C#代码将 "只工作"：</span><span class="sxs-lookup"><span data-stu-id="35890-270">then this C# code will "just work":</span></span>

```csharp
IAsyncEnumerable<int> enumerable = ...;
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select item * 2;
```

<span data-ttu-id="35890-271">但是，没有支持在子句中使用 `await` 的查询理解语法，因此，如果添加了 Ix，例如：</span><span class="sxs-lookup"><span data-stu-id="35890-271">However, there is no query comprehension syntax that supports using `await` in the clauses, so if Ix added, for example:</span></span>

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, ValueTask<TResult>> func);
```

<span data-ttu-id="35890-272">这会 "只工作"：</span><span class="sxs-lookup"><span data-stu-id="35890-272">then this would "just work":</span></span>

```csharp
IAsyncEnumerable<string> result = from url in urls
                                  where item % 2 == 0
                                  select SomeAsyncMethod(item);

async ValueTask<int> SomeAsyncMethod(int item)
{
    await Task.Yield();
    return item * 2;
}
```

<span data-ttu-id="35890-273">但在 `select` 子句中，无法用 `await` 内联来编写该方法。</span><span class="sxs-lookup"><span data-stu-id="35890-273">but there'd be no way to write it with the `await` inline in the `select` clause.</span></span>  <span data-ttu-id="35890-274">作为一个单独的工作，我们可能会考虑向语言添加 `async { ... }` 表达式，此时，我们可以允许它们在 query 理解中使用，而上述操作可以改为编写为：</span><span class="sxs-lookup"><span data-stu-id="35890-274">As a separate effort, we could look into adding `async { ... }` expressions to the language, at which point we could allow them to be used in query comprehensions and the above could instead be written as:</span></span>

```csharp
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select async
                               {
                                   await Task.Yield();
                                   return item * 2;
                               };
```

<span data-ttu-id="35890-275">如果为，则允许在表达式中直接使用 `await`，如支持 `async from`。</span><span class="sxs-lookup"><span data-stu-id="35890-275">or to enabling `await` to be used directly in expressions, such as by supporting `async from`.</span></span>  <span data-ttu-id="35890-276">但是，这种设计不太可能会对功能集的其余部分产生任何影响，并且这不是我们目前要投入的高价值的东西，所以建议不要在此处执行任何其他操作。</span><span class="sxs-lookup"><span data-stu-id="35890-276">However, it's unlikely a design here would impact the rest of the feature set one way or the other, and this isn't a particularly high-value thing to invest in right now, so the proposal is to do nothing additional here right now.</span></span>

## <a name="integration-with-other-asynchronous-frameworks"></a><span data-ttu-id="35890-277">与其他异步框架集成</span><span class="sxs-lookup"><span data-stu-id="35890-277">Integration with other asynchronous frameworks</span></span>

<span data-ttu-id="35890-278">与 `IObservable<T>` 和其他异步框架（例如，反应性流）的集成将在库级别而不是在语言级别进行。</span><span class="sxs-lookup"><span data-stu-id="35890-278">Integration with `IObservable<T>` and other asynchronous frameworks (e.g. reactive streams) would be done at the library level rather than at the language level.</span></span>  <span data-ttu-id="35890-279">例如，`IAsyncEnumerator<T>` 中的所有数据都可以通过 `await foreach`"对枚举器进行操作并 `OnNext`" 将数据输入到观察者来发布到 `IObserver<T>`，因此可以使用 `AsObservable<T>` 扩展方法。</span><span class="sxs-lookup"><span data-stu-id="35890-279">For example, all of the data from an `IAsyncEnumerator<T>` can be published to an `IObserver<T>` simply by `await foreach`'ing over the enumerator and `OnNext`'ing the data to the observer, so an `AsObservable<T>` extension method is possible.</span></span>  <span data-ttu-id="35890-280">在 `await foreach` 中使用 `IObservable<T>` 需要缓冲数据（如果在以前的项仍在进行处理的情况下推送另一项），但这样一来，就可以轻松实现此类推送请求适配器，以使 `IObservable<T>` 能够与 `IAsyncEnumerator<T>`请求。</span><span class="sxs-lookup"><span data-stu-id="35890-280">Consuming an `IObservable<T>` in a `await foreach` requires buffering the data (in case another item is pushed while the previous item is still being processing), but such a push-pull adapter can easily be implemented to enable an `IObservable<T>` to be pulled from with an `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="35890-281">等. Rx/Ix 已经提供此类实现的原型，如 https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels 提供各种缓冲数据结构。</span><span class="sxs-lookup"><span data-stu-id="35890-281">Etc.  Rx/Ix already provide prototypes of such implementations, and libraries like https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels provide various kinds of buffering data structures.</span></span>  <span data-ttu-id="35890-282">此阶段不需要涉及此语言。</span><span class="sxs-lookup"><span data-stu-id="35890-282">The language need not be involved at this stage.</span></span>
