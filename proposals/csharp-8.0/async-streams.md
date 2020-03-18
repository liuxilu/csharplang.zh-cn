---
ms.openlocfilehash: 9ed1aa75d581cecbf754a84d1f523c6334b8c0ac
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "79484146"
---
# <a name="async-streams"></a>异步流

* [x] 建议
* [x] 原型
* [] 实现
* [] 规范

## <a name="summary"></a>总结
[summary]: #summary

C#支持迭代器方法和异步方法，但不支持同时作为迭代器和异步方法的方法。  我们应该通过允许 `await` 在新的 `async` 迭代器（返回 `IAsyncEnumerable<T>` 或 `IAsyncEnumerator<T>` 而不是 `IEnumerable<T>` 或 `IEnumerator<T>`）中使用来纠正这种情况，在新 `IAsyncEnumerable<T>` 中使用 `await foreach`。  `IAsyncDisposable` 接口还用于启用异步清理。

## <a name="related-discussion"></a>相关讨论
- https://github.com/dotnet/roslyn/issues/261
- https://github.com/dotnet/roslyn/issues/114

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

## <a name="interfaces"></a>界面

### <a name="iasyncdisposable"></a>IAsyncDisposable

对 `IAsyncDisposable` （例如 https://github.com/dotnet/roslyn/issues/114) 以及它是否是一个好主意）有很多讨论。  不过，它是添加异步迭代器支持所需的概念。  由于 `finally` 块可能包含 `await`，并且由于需要在处理迭代器的过程中运行 `finally` 块，因此我们需要异步处理。  通常情况下，任何时候清理资源所需的时间都很有用，例如关闭文件（需要刷新）、取消注册回调并提供一种了解注销完成时间等的方法。

以下接口将添加到核心 .NET 库（例如 System.private.corelib/Runtime）：
```csharp
namespace System
{
    public interface IAsyncDisposable
    {
        ValueTask DisposeAsync();
    }
}
```
与 `Dispose`一样，多次调用 `DisposeAsync` 是可接受的，并且第一次调用之后的调用应视为 nops，并返回同步完成任务（`DisposeAsync` 无需线程安全），并且无需支持并发调用。  此外，类型可以同时实现 `IDisposable` 和 `IAsyncDisposable`，如果这样做，则可以像调用 `Dispose`，然后 `DisposeAsync`，反之亦然，而只有第一种方法是有意义的，后续调用都应该是 nop。  因此，如果类型确实实现了这两种类型，则鼓励使用者调用一次，并且只能调用一次基于上下文的相关方法，`Dispose` 在同步上下文中并在异步环境中 `DisposeAsync`。

（我将讨论 `IAsyncDisposable` 如何与 `using` 进行相互讨论。  稍后将在此建议中处理与 `foreach` 进行交互的方式。）

考虑的替代方案：
- _`DisposeAsync` 接受 `CancellationToken`_ ：尽管在理论上，任何异步操作都可以取消，处置就是清除、关闭 free'ing 资源等，这通常不是应取消的内容;对于取消的工作，清除仍很重要。  导致实际工作被取消的相同 `CancellationToken` 通常是传递给 `DisposeAsync`的同一令牌，`DisposeAsync` 毫无意义，因为取消工作会导致 `DisposeAsync` 为 nop。  如果有人想要避免被阻止以等待处置，则可以避免等待产生的 `ValueTask`，或只等待一段时间。
- _`DisposeAsync` 返回 `Task`_ ：现在，非泛型 `ValueTask` 存在并可从 `IValueTaskSource`中构造，从 `ValueTask` 返回 `DisposeAsync` 允许使用现有对象作为表示 `DisposeAsync`的最终异步完成的承诺，在 `Task` 异步完成的情况下，保存 `DisposeAsync` 分配。
- _使用 `bool continueOnCapturedContext` （`ConfigureAwait`）配置 `DisposeAsync`_ ：尽管可能存在与此类概念的公开方式相关的问题 `using`、`foreach`和其他使用该概念的语言构造，但从接口角度来看，实际上不会执行任何 `await`的操作，并且没有任何内容可供配置 .。。`ValueTask` 的使用者可以使用它们。
- _继承 `IDisposable``IAsyncDisposable`_ ：只应使用其中一项或另一项，因此强制类型实现两者并无意义。
- _`IDisposableAsync`，而不是 `IAsyncDisposable`_ ：我们一直在执行命名操作，其中，事物为 "异步内容"，而操作是 "异步" 的，因此，类型将 "async" 作为前缀，方法将 "async" 作为后缀。

### <a name="iasyncenumerable--iasyncenumerator"></a>IAsyncEnumerable/IAsyncEnumerator

向核心 .NET 库添加了两个接口：

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

典型使用情况（无其他语言功能）如下所示：

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

丢弃的选项：
- _`Task<bool> MoveNextAsync(); T current { get; }`_ ：使用 `Task<bool>` 支持使用缓存的任务对象来表示同步、成功的 `MoveNextAsync` 调用，但异步完成仍需要分配。  通过返回 `ValueTask<bool>`，我们可以使枚举器对象自身实现 `IValueTaskSource<bool>`，并将其用作从 `MoveNextAsync`返回的 `ValueTask<bool>` 的后备，进而大大降低了开销。
- _`ValueTask<(bool, T)> MoveNextAsync();`_ ：不仅更难使用，而且它意味着 `T` 不再是协变的。
- _`ValueTask<T?> TryMoveNextAsync();`_ ：不是协变的。
- _`Task<T?> TryMoveNextAsync();`_ ：不是协变的，每个调用的分配等。
- _`ITask<T?> TryMoveNextAsync();`_ ：不是协变的，每个调用的分配等。
- _`ITask<(bool,T)> TryMoveNextAsync();`_ ：不是协变的，每个调用的分配等。
- _`Task<bool> TryMoveNextAsync(out T result);`_ ：当操作以同步方式返回时，需要设置 `out` 结果，而不是在未来的某个时间内异步完成任务，此时无法传递结果。
- _`IAsyncEnumerator<T>` 未实现 `IAsyncDisposable`_ ：我们可以选择将它们分开。  不过，这样做会使建议的其他某些区域变得更加复杂，因为代码必须能够处理枚举器不提供释放的可能性，这使得编写基于模式的帮助器变得困难。  此外，枚举器还需要释放（例如，具有 finally 块的任何异步迭代器C# 、从网络连接中枚举数据等的任何异步迭代器），如果不需要，则可以简单地将该方法作为 `public ValueTask DisposeAsync() => default(ValueTask);` 实现，只需最小的额外开销。
- _ `IAsyncEnumerator<T> GetAsyncEnumerator()`：无取消标记参数。

#### <a name="viable-alternative"></a>可行的替代方案：

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

在内部循环中使用 `TryGetNext`，只要它们可以同步使用，就会使用单个接口调用的项。  当无法同步检索下一项时，它将返回 false，并在每次返回 false 时，调用方必须随后调用 `WaitForNextAsync` 以等待下一项可用或确定将永远不会有其他项。 典型使用情况（无其他语言功能）如下所示：

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

这样做的优点是两折：
- _小调：允许枚举器支持多个使用者_。 在某些情况下，可以使用枚举器来支持多个并发使用者。  当 `MoveNextAsync` 和 `Current` 单独以便实现无法使其使用原子时，无法实现。  与此相反，此方法提供了一种方法 `TryGetNext`，该方法支持向前推送枚举器和获取下一项，因此，如果需要，枚举器可以启用原子性。  但是，这种情况很可能是通过为每个使用者提供来自共享可枚举的使用者自己的枚举器来实现的。  此外，我们不想强制每个枚举器都支持并发使用，因为这样做会将不重要的开销添加到不需要它的多数情况，这意味着接口的使用者通常不能以任何方式依赖于这种情况。
- _主要：性能_。 `MoveNextAsync`/`Current` 方法需要每个操作有两个接口调用，而 `WaitForNextAsync`/`TryGetNext` 的最佳情况是，大多数迭代都是同步完成的，因此，使用 `TryGetNext`实现了一个紧密的内部循环，以便每个操作仅有一个接口调用。  这在接口调用占据计算的情况下可能会产生实实在在的影响。

不过，这种缺点不太重要，包括手动使用这些方法时显著增加的复杂性，以及使用它们时引入 bug 的可能性也会增加。  尽管性能优势在 microbenchmarks 中显示，但我们并不相信它们会有影响力于绝大多数实际使用。  如果出现这种情况，我们可以用一种全新的方式引入一组接口。

丢弃的选项：
- `ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`： `out` 参数不能是协变的。  这里也有一个小的影响（一般的试用模式问题），这可能会导致引用类型结果出现运行时写入屏障。

#### <a name="cancellation"></a>取消

可以通过多种方法来支持取消：
1. `IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` 是取消无关的： `CancellationToken` 不会出现在任何位置。  通过以任何适当的方式将 `CancellationToken` 以逻辑方式收录到可枚举的和/或枚举器来实现取消，例如，在调用迭代器时，将 `CancellationToken` 作为参数传递给迭代器方法并在迭代器的正文中使用它，就像使用任何其他参数完成的操作一样。
2. `IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`：将 `CancellationToken` 传递到 `GetAsyncEnumerator`，并且后续的 `MoveNextAsync` 操作可以遵守此操作。
3. `IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`：向每个单独的 `MoveNextAsync` 调用传递一个 `CancellationToken`。
4. 1 & & 2：将 `CancellationToken`嵌入到可枚举/枚举器，并将 `CancellationToken`传递到 `GetAsyncEnumerator`。
5. 1 & & 3：将 `CancellationToken`嵌入到可枚举/枚举器，并将 `CancellationToken`传递到 `MoveNextAsync`。

从纯粹的理论角度来看，（5）是最强大的，在这种情况下（a） `MoveNextAsync` 接受 `CancellationToken` 可对取消的内容进行最精细的控制，而（b） `CancellationToken` 只是可以作为参数传递到迭代器中的任何其他类型，如嵌入到任意类型等。

但是，该方法有多个问题：
- 如何将 `CancellationToken` 传递给 `GetAsyncEnumerator` 使其成为迭代器的主体？  我们可以公开一个新的 `iterator` 关键字，您可以从该关键字开始，以访问传递到 `GetEnumerator`的 `CancellationToken`。但这是一个额外的机制，b）我们将它变成一个非常先进的公民，c），99% 的事例似乎与调用迭代器并对其调用 `GetAsyncEnumerator` 的代码相同，在这种情况下，它只需将 `CancellationToken` 作为参数传递到方法中。
- 如何将 `CancellationToken` 传递到 `MoveNextAsync` 进入方法体？  这种情况更糟，就像它是从 `iterator` 本地对象公开的一样，其值可以在等待期间更改，这意味着，使用令牌注册的任何代码都需要在等待之前从其注销，然后在之后重新注册;不管是由编译器在迭代器中实现还是由开发人员手动实现，都需要在每个 `MoveNextAsync` 调用中进行此类注册和注销。
- 开发人员如何取消 `foreach` 循环？  如果是通过向可枚举/枚举器提供 `CancellationToken` 来完成的，则这两个操作都需要支持 `foreach`的对枚举器的运算，这会将它们提升为第一类公民，现在你需要考虑围绕枚举器（例如 LINQ 方法）或 b 构建的生态系统，因此，我们需要将 `CancellationToken` 嵌入到可枚举的中，这是因为在将存储所提供的令牌的 `IAsyncEnumerable<T>`，并在返回的结构上的 `GetAsyncEnumerator` `WithCancellation` `GetAsyncEnumerator`被调用（忽略该标记）。  或者，您可以只使用在 foreach 的主体中的 `CancellationToken`。
- 如果支持查询理解，则如何向每个子句传递 `GetEnumerator` 或 `MoveNextAsync` 的 `CancellationToken`？  最简单的方法是让子句捕获该语句，此时将忽略任何标记传递到 `GetAsyncEnumerator`/`MoveNextAsync` 会被忽略。

此文档的较早版本（1），但我们已切换到（4）。

（1）主要有两个问题：
- 可取消枚举的创建者必须实现一些样本，并且只能利用编译器对异步迭代器的支持来实现 `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)` 方法。
- 很多的创建者可能只是将 `CancellationToken` 参数添加到其异步可枚举签名，而这会阻止使用者在给定 `IAsyncEnumerable` 类型时传递所需的取消令牌。

有两种主要的消耗方案：
1. `await foreach (var i in GetData(token)) ...` 使用者调用异步迭代器方法的位置，
2. `await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...` 使用者处理给定 `IAsyncEnumerable` 实例的位置。

我们发现一种合理的折衷方法是以一种可方便地使用异步流的生成者和使用者的方式来支持这两种方案。 `[EnumeratorCancellation]` 特性用于实现此目的。 如果将此属性放置在参数上，则会告诉编译器如果将令牌传递到 `GetAsyncEnumerator` 方法，则应使用该令牌，而不是最初为参数传递的值。

以 `IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)` 为例。 此方法的实施者只需使用方法体中的参数即可。 使用者可以使用上面的两种消费模式：
1. 如果使用 `GetData(token)`，则令牌将保存到 async 可枚举中，并在迭代中使用。
2. 如果使用 `givenIAsyncEnumerable.WithCancellation(token)`，则传递到 `GetAsyncEnumerator` 的令牌将取代所有保存在异步可枚举中的令牌。

## <a name="foreach"></a>foreach

`foreach` 将进一步补充，以支持 `IAsyncEnumerable<T>` 以及其对 `IEnumerable<T>`的现有支持。  如果相关成员公开公开后，它将支持将 `IAsyncEnumerable<T>` 视为一种模式，以便启用基于结构的扩展，以避免分配，并使用替代的等待作为 `MoveNextAsync` 和 `DisposeAsync`的返回类型。

### <a name="syntax"></a>语法

使用语法：

```csharp
foreach (var i in enumerable)
```

C#将继续将 `enumerable` 视为同步可枚举，这样即使它公开了用于 async 枚举的相关 Api （公开模式或实现接口），它也只会考虑同步 Api。

若要强制 `foreach` 改为仅考虑异步 Api，请按如下所示插入 `await`：

```csharp
await foreach (var i in enumerable)
```

不会提供支持使用 async 或 sync Api 的语法;开发人员必须根据所使用的语法进行选择。

丢弃的选项：
- _`foreach (var i in await enumerable)`_ ：这是有效的语法，并且更改其含义会是一项重大更改。  这意味着要 `await` `enumerable`，从其返回可迭代同步的内容，然后以同步方式循环访问。
- _`foreach (var i await in enumerable)`，`foreach (var await i in enumerable)`，`foreach (await var i in enumerable)`_ ：这一切都表示我们正在等待下一项，但在 foreach 中涉及其他等待，特别是如果可枚举的是 `IAsyncDisposable`，我们将 `await`其异步处理。  该 await 作为 foreach 的作用域，而不是每个单个元素的作用域，因此 `await` 关键字一定处于 `foreach` 级别。  此外，将其与 `foreach` 相关联为我们提供了一种用不同的术语（例如 "await foreach"）描述 `foreach` 的方法。  但更重要的是，在考虑 `foreach` 语法 `using` 语法的同时，还存在一些价值，以便它们彼此保持一致，`using (await ...)` 已经是有效的语法。
- _`foreach await (var i in enumerable)`_

仍要考虑：
- 目前 `foreach` 不支持循环访问枚举器。  我们预计 `IAsyncEnumerator<T>`就会更常见，因此，`IAsyncEnumerable<T>` 和 `IAsyncEnumerator<T>`都可以支持 `await foreach`。  但一旦我们添加了此类支持，它就会引入 `IAsyncEnumerator<T>` 是否是第一类公民的问题，以及是否需要对枚举器进行除枚举以外的组合器重载？    我们想要鼓励方法返回枚举器而不是枚举？ 我们应该继续讨论这一点。  如果我们决定不想对其提供支持，我们可能希望引入 `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);` 的扩展方法，该方法允许枚举器仍 `foreach`。  如果我们决定要支持，则还需要决定 `await foreach` 是否负责调用枚举器上的 `DisposeAsync`，以及答案是否为 "不，控制过度处置应由调用 `GetEnumerator`的人员处理"。

### <a name="pattern-based-compilation"></a>基于模式的编译

编译器将绑定到基于模式的 Api （如果存在），并首选使用接口（使用实例方法或扩展方法可满足此模式）。  模式的要求如下：
- 可枚举必须公开一个 `GetAsyncEnumerator` 方法，该方法可在没有参数的情况调用，并返回一个满足相关模式的枚举器。
- 枚举器必须公开一个 `MoveNextAsync` 方法，该方法可在不使用任何参数的情况调用，并且返回可能在 `await`ed 并且其 `GetResult()` 返回 `bool`的内容。
- 枚举器还必须公开 `Current` 属性，该属性的 getter 返回一个表示所枚举的数据类型的 `T`。
- 枚举器可以选择性地公开 `DisposeAsync` 方法，该方法可在不使用任何参数的情况调用，并返回可 `await`ed 并且 `GetResult()` 返回 `void`的内容。

此代码：

```csharp
var enumerable = ...;
await foreach (T item in enumerable)
{
   ...
}
```

转换为的等效项：

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

如果迭代类型未公开正确的模式，则将使用这些接口。

### <a name="configureawait"></a>ConfigureAwait

此基于模式的编译允许通过 `ConfigureAwait` 扩展方法在所有等待中使用 `ConfigureAwait`：

```csharp
await foreach (T item in enumerable.ConfigureAwait(false))
{
   ...
}
```

这将基于我们还将添加到 .NET 的类型（可能为 ""）：

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

请注意，此方法不会允许 `ConfigureAwait` 与基于模式的枚举一起使用。但再次强调，`ConfigureAwait` 仅作为 `Task`/上的扩展公开 `Task<T>`/`ValueTask`/`ValueTask<T>` 并且不能应用于任意可等待的操作，因为它仅在应用于任务时才有意义（它控制在任务的继续支持中实现的行为），因此，在使用可等待的情况下，这种情况在不能是任务时是有意义的。  返回可等待的任何人都可以在此类高级方案中提供自己的自定义行为。

（如果我们可以通过某种方式来支持作用域或程序集级别的 `ConfigureAwait` 解决方案，则不需要这样做。）

## <a name="async-iterators"></a>异步迭代器

语言/编译器除了使用它们外，还支持生成 `IAsyncEnumerable<T>`和 `IAsyncEnumerator<T>`。 如今，该语言支持编写迭代器，如下所示：

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

但不能在这些迭代器的正文中使用 `await`。  我们将添加该支持。

### <a name="syntax"></a>语法

迭代器的现有语言支持根据方法是否包含任何 `yield`推断方法的迭代器特性。  对于异步迭代器，情况也是如此。  此类异步迭代器将 demarcated，并与同步迭代器进行区分，方法是将 `async` 添加到签名，然后必须将 `IAsyncEnumerable<T>` 或 `IAsyncEnumerator<T>` 为其返回类型。  例如，可以将上面的示例编写为异步迭代器，如下所示：

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

考虑的替代方案：
- 在_签名中不使用 `async`_ ：编译器在技术上可能需要使用 `async`，因为它使用它来确定 `await` 在该上下文中是否有效。  但即使不是必需的，我们也已经建立 `await` 只能在标记为 `async`的方法中使用，因此保持一致性非常重要。
- 为 _`IAsyncEnumerable<T>`启用自定义生成器_：这就是我们将来要探讨的事情，但这是一项非常复杂的事情，我们不支持同步的对应项。
- _签名中具有 `iterator` 关键字_：异步迭代器将在签名中使用 `async iterator`，`yield` 只能在包含 `iterator`的 `async` 方法中使用;然后，`iterator` 在同步迭代器上是可选的。  根据您的观点，这一优点在于，无论是否允许 `yield`，该方法的签名都可以很清楚地确定 `IAsyncEnumerable<T>` 类型的实例，而不是根据代码是否使用 `yield` 来返回编译器。  但这不同于同步迭代器，而不能对其进行要求。  此外，某些开发人员不喜欢额外的语法。  如果我们是从头开始设计的，则可能需要这样做，但此时，将异步迭代器保持在同步迭代器附近会有更多的价值。

## <a name="linq"></a>LINQ

`System.Linq.Enumerable` 类上有超过200的方法重载，所有这些重载都是 `IEnumerable<T>`的;其中一些接受 `IEnumerable<T>`，其中一些会生成 `IEnumerable<T>`，许多都是这样的。  为 `IAsyncEnumerable<T>` 添加 LINQ 支持可能需要为其复制所有这些重载，为另一个 ~ 200。  由于 `IAsyncEnumerator<T>` 可能比 `IEnumerator<T>` 在同步领域中的独立实体更常见，因此，可能需要另一 ~ 200 重载来处理 `IAsyncEnumerator<T>`。  此外，大量重载处理谓词（例如，使用 `Func<T, bool>`的 `Where`），并且可能需要具有处理同步和异步谓词的基于 `IAsyncEnumerable<T>`的重载（`Func<T, ValueTask<bool>>` 例如，除了 `Func<T, bool>`之外）。  虽然这并不适用于所有现在 ~ 400 的新重载，但大致的计算是它适用于一半，这意味着另一个 ~ 200 重载，总共 ~ 600 个新方法。

这是一种惊人的 Api，在考虑到诸如交互式扩展（Ix）这样的扩展库时，可能会出现更多的情况。  但 Ix 已经实现了其中的许多，但看起来不是复制该工作的重要原因;当开发人员想要将 LINQ 与 `IAsyncEnumerable<T>`结合使用时，我们应帮助社区改进 Ix 并建议使用它。

此外，还存在与查询理解语法有关的问题。  查询理解的基于模式的性质使其能够 "只使用" 一些运算符，例如，如果 Ix 提供以下方法：

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, TResult> func);
public static IAsyncEnumerable<T> Where(this IAsyncEnumerable<T> source, Func<T, bool> func);
```

此C#代码将 "只工作"：

```csharp
IAsyncEnumerable<int> enumerable = ...;
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select item * 2;
```

但是，没有支持在子句中使用 `await` 的查询理解语法，因此，如果添加了 Ix，例如：

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, ValueTask<TResult>> func);
```

这会 "只工作"：

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

但在 `select` 子句中，无法用 `await` 内联来编写该方法。  作为一个单独的工作，我们可能会考虑向语言添加 `async { ... }` 表达式，此时，我们可以允许它们在 query 理解中使用，而上述操作可以改为编写为：

```csharp
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select async
                               {
                                   await Task.Yield();
                                   return item * 2;
                               };
```

如果为，则允许在表达式中直接使用 `await`，如支持 `async from`。  但是，这种设计不太可能会对功能集的其余部分产生任何影响，并且这不是我们目前要投入的高价值的东西，所以建议不要在此处执行任何其他操作。

## <a name="integration-with-other-asynchronous-frameworks"></a>与其他异步框架集成

与 `IObservable<T>` 和其他异步框架（例如，反应性流）的集成将在库级别而不是在语言级别进行。  例如，`IAsyncEnumerator<T>` 中的所有数据都可以通过 `await foreach`"对枚举器进行操作并 `OnNext`" 将数据输入到观察者来发布到 `IObserver<T>`，因此可以使用 `AsObservable<T>` 扩展方法。  在 `await foreach` 中使用 `IObservable<T>` 需要缓冲数据（如果在以前的项仍在进行处理的情况下推送另一项），但这样一来，就可以轻松实现此类推送请求适配器，以使 `IObservable<T>` 能够与 `IAsyncEnumerator<T>`请求。  等. Rx/Ix 已经提供此类实现的原型，如 https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels 提供各种缓冲数据结构。  此阶段不需要涉及此语言。
