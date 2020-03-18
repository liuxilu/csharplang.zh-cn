---
ms.openlocfilehash: 1457c1eb018e12af30ce5b38be704bf8851d4b25
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "79483936"
---
# <a name="efficient-params-and-string-formatting"></a>有效的参数和字符串格式设置

## <a name="summary"></a>总结
这种功能组合会提高格式 `string` 值和 `params` 样式参数的传递效率。

## <a name="motivation"></a>动机
格式 `string` 值的分配开销会使多个基于文本的应用程序的性能成为主导的：从 `struct` 类型的装箱损失，在 `string` 调用期间，`params` 和中间 `string.Format` 分配的 `object[]` 分配。 为了保持高效率，此类应用程序通常需要放弃工作效率功能，如 `params` 和 `string` 插值，并迁移到非标准的、手动编码的解决方案。 

以 MSBuild 为例。 这是使用许多现代C#功能编写的，开发人员意识到性能。 但在一个代表性的生成示例中，MSBuild 将使用最小的详细级别生成 `string` 分配的262MB。 其中，1/2 的分配是 `string.Format`内的短生存期分配。 由于 `Span<T>` 的可用性，这些功能将在 .NET Desktop 上删除很多功能，并使其在 .NET Core 上几乎为零：

此处所述的一组语言功能使应用程序能够继续使用这些功能，而不会对应用程序代码库进行极少或没有改动，同时在大多数情况下删除意外的分配开销。

## <a name="detailed-design"></a>详细设计 
此处有一组功能可用于实现这些结果：

- 扩展 `params` 以支持一组更广泛的集合类型。
- 允许开发人员自定义如何实现 `string` 内插。 
- 允许插 `string` 绑定到更高效 `string.Format` 重载。

### <a name="extending-params"></a>扩展参数
语言将允许方法签名中的 `params` `Span<T>`、`ReadOnlySpan<T>` 和 `IEnumerable<T>`类型。 调用的相同规则将应用于这些新类型，这些新类型适用于 `params T[]`：

- 不能重载，其中唯一的差异是 `params` 关键字。
- 可以通过传递一系列可隐式转换为 `T` 或单个 `Span<T>` / 
`ReadOnlySpan<T>` / `IEnumerable<T>` 参数的参数来调用。
- 必须是方法签名中的最后一个参数。
- 等等 。 

为了简单起见，`Span<T>` 和 `ReadOnlySpan<T>` 变体 `Span<T>` 如下所示。 在 `ReadOnlySpan<T>` 的行为不同的情况下，将显式调用它。 

`params` 提供的 `Span<T>` 变体的优点在于，它在为 `Span<T>` 值分配后备存储的方式上为编译器提供了很大的灵活性。 使用 `params T[]` 编译器必须为 `params` 方法的每个调用分配一个新 `T[]`。 重复使用是不可能的，因为它必须假定被调用方存储并重用参数。 这可能会导致大量 `params` 调用的方法中的较大效率。

给定的 `Span<T>` 变体 `ref struct` 被调用方无法存储参数。 因此，编译器可以通过执行重复使用参数等操作来优化调用站点。 与 `T[]`相比，这可以更高效地进行调用。 不过，该语言将不会对此类 dlr 进行优化的特定保证。 请注意，在调用 `params Span<T>` 方法时，编译器可以自由使用除 `T[]` 以外的值。 

这种潜在的实现如下所示。 考虑方法体中的所有 `params` 调用。 编译器可以分配一个大小等于最大 `params` 调用的数组，并通过在数组上创建适当大小的 `Span<T>` 实例，将其用于所有调用。 例如：

``` csharp
static class OneAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("jaredpar");
        Use("hello", "world");
        Use("a", "longer", "set");
    }
}
```

编译器可能会选择发出 `Go` 的正文，如下所示：

``` csharp
    static void Go() {
        var args = new string[3];
        args[0] = "jaredpar";
        Use(new Span<string>(args, start: 0, length: 1));

        args[0] = "hello";
        args[1] = "world";
        Use(new Span<string>(args, start: 0, length: 2));

        args[0] = "a";
        args[1] = "longer";
        args[2] = "set";
        Use(new Span<string>(args, start: 0, length: 3));
   }
```

这可以显著减少应用程序中分配的数组数。 如果运行时为数组的更智能堆栈分配提供实用工具，则分配可能更进一步。

不过，此优化不能始终应用。 即使被调用方无法捕获 `params` 参数，但当 `ref` 或 `out / ref` 参数本身是 `ref struct` 类型时，它仍可以在调用方中捕获。 

``` csharp
static class SneakyCapture {
    static ref int M(params Span<T> span) => ref span[0];

    static void Oops() {
        // This now holds onto the memory backing the Span<T> 
        ref int r = ref M(42);
    }
}
```

不过，这些情况是静态检测的。 只要有 `ref` 返回或 `out` 或 `ref`传递 `ref struct` 参数，就可能会发生这种情况。 在这种情况下，编译器必须为每个调用分配一个全新的 `T[]`。 

本文档末尾讨论了几个其他可能的优化策略。

`IEnumerable<T>` 变体只是一个便利重载。 这对于经常使用 `IEnumerable<T>` 但也有大量 `params` 使用的方案非常有用。 在 `T` 参数形式调用时，后备存储将作为 `T[]` 分配，就像现在 `params T[]` 进行的一样。

### <a name="params-overload-resolution-changes"></a>参数重载决策更改
此提议表明，该语言现在具有四个 `params` 的变体，在此之前有一个。 方法只是定义唯一不同于 `params` 声明的类型的方法的重载是明智的。 

请考虑 `StringBuilder.AppendFormat` 除了 `params object[]`，还应添加 `params ReadOnlySpan<object>` 重载。 这使得它可以通过减少收集分配来大幅提高性能，而无需对调用代码进行任何更改。 

为了便于实现此语言，此语言会引入以下重载决策关联中断规则。 当候选方法仅在 `params` 参数上存在差异时，优先顺序如下：

1. `ReadOnlySpan<T>`
1. `Span<T>`
1. `T[]`
1. `IEnumerable<T>`

此顺序最适合一般情况。

### <a name="variant"></a>Variant
CoreFX 正在原型为一个名为[Variant](https://github.com/dotnet/corefxlab/pull/2595)的新托管类型。 此类型旨在在需要异类值但不希望使用 `object` 作为参数的情况下使用的 Api 中使用。 `Variant` 类型提供通用存储，但可避免对最常用的类型进行装箱分配。 在诸如 `string.Format` 的 Api 中使用此类型可以消除大多数情况下的装箱开销。

此类型本身不一定是语言的特殊类型。 它在本文档中单独引入，因为它成为方案的其他部分的实现细节。 

### <a name="efficient-interpolated-strings"></a>有效的内插字符串
内插字符串是中C#常见但低效的功能。 使用内插 `string` 作为 `string`的最常见语法转换为 `string.Format(string, params object[])` 调用。 这将导致所有值类型的装箱分配，中间 `string` 分配作为实现的主要用途是在参数数目超出了 `string.Format`"快速" 重载的参数量的情况下，使用 `object.ToString` 进行格式设置以及数组分配。 

该语言将更改它的内插，以考虑 `string.Format`的备用重载。 它将考虑 `string.Format(string, params)` 的所有形式，并选取满足参数类型的 "最佳" 重载。
"最佳" `params` 重载将由前面讨论的规则确定。 这意味着内插 `string` 现在可以绑定到非常高效的重载，如 `string.Format(string format, params ReadOnlySpan<Variant> args)`。 在许多情况下，这将删除所有中间分配。

### <a name="customizable-interpolated-strings"></a>可自定义的内插字符串
开发人员能够通过 `FormattableString`自定义内插字符串的行为。 这包含进入内插字符串的数据：格式 `string` 和作为数组的参数。 但这仍具有装箱和参数数组分配以及 `FormattableString` （这是一个 `abstract class`）的分配。 因此，几乎不能使用 `string` 格式的分配繁重的应用程序。

为了高效地进行内插字符串格式设置，语言将识别新的类型： `System.ValueFormattableString`。 所有内插字符串都将具有到此类型的目标类型转换。 这将通过将内插字符串转换到调用中 `ValueFormattableString.Create` 完全按照今天 `FormattableString.Create` 的方式来实现。 查找最适合的 `ValueFormattableString.Create` 方法时，该语言将支持本文档中所述的所有 `params` 选项。 

``` csharp
readonly struct ValueFormattableString {
    public static ValueFormattableString Create(Variant v) { ... } 
    public static ValueFormattableString Create(string s) { ... } 
    public static ValueFormattableString Create(string s, params ReadOnlySpan<Variant> collection) { ... } 
}

class ConsoleEx { 
    static void Write(ValueFormattableString f) { ... }
}

class Program { 
    static void Main() { 
        ConsoleEx.Write(42);
        ConsoleEx.Write($"hello {DateTime.UtcNow}");

        // Translates into 
        ConsoleEx.Write(ValueFormattableString.Create((Variant)42));
        ConsoleEx.Write(ValueFormattableString.Create(
            "hello {0}", 
            new Variant(DateTime.UtcNow));
    }
}
```

当参数为内插字符串时，重载决策规则将更改为首选 `ValueFormattableString` 超出 `string`。 这意味着，具有仅在 `string` 和 `ValueFormattableString`上不同的重载会很有价值。 现在使用 `FormattableString` 的这种重载并不重要，因为编译器将始终优先于 `string` 版本（除非开发人员使用显式强制转换）。 

## <a name="open-issues"></a>打开的问题

### <a name="valueformattablestring-breaking-change"></a>ValueFormattableString 重大更改
对 `string` 的重载决策过程中首选 `ValueFormattableString` 的更改是一项重大更改。 开发人员可以定义目前称为 `ValueFormattableString` 类型，并在 `string`的方法重载中使用该类型。 此建议的更改将导致编译器在此功能集实现后选取不同重载。 

出现这种情况的可能性非常低。 类型需要 `System.ValueFormattableString` 全名，并且需要具有名为 `Create``static` 方法。 鉴于开发人员强烈建议不要定义 `System` 命名空间中的任何类型，这种做法似乎是一个合理的折衷。

### <a name="expanding-to-more-types"></a>扩展到更多类型
假设我们位于区域，应考虑将 `IList<T>`、`ICollection<T>` 和 `IReadOnlyList<T>` 添加到支持 `params` 的集合集。 就实现而言，这里的其他工作将花费较小的费用。

不过，LDM 需要确定是否有必要的复杂语言。 添加 `IEnumerable<T>` 会删除非常特定的摩擦点。 缺少这 `params` 解决方案，许多客户在调用 `params` 方法时被迫从 `IEnumerable<T>` 分配 `T[]`。 但添加 `IEnumerable<T>` 会解决此问题。 其他接口在此不会解决的特定摩擦点。 

## <a name="considerations"></a>注意事项

### <a name="variant2-and-variant3"></a>Variant2 和 Variant3
CoreFX 团队还具有一组非分配的存储类型，最多包含三个 `Variant` 参数。 这些是单个 `Variant``Variant2` 和 `Variant3`。 所有方法都具有一对用于 `Span<Variant>` 之外获取分配的方法： `CreateSpan` 和 `KeepAlive`。 这意味着，对于最多三个参数的 `params Span<Variant>`，调用站点可以完全自由分配。

``` csharp
static class ZeroAllocation {
    static void Use(params Span<Variant> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

`Go` 方法可以降低到以下各项：

``` csharp
static class ZeroAllocation {
    static void Go() {
        Variant2 _v;
        _v.Variant1 = new Variant("hello");
        _v.Variant2 = new Variant("word");
        Use(_v.CreateSpan());
        _v.KeepAlive();
    }
}
```

这几乎不需要在建议的基础上重复使用 `params Span<T>` 调用之间 `T[]`。 编译器已经需要按调用管理临时，并在一种情况下进行清理工作（即使在一种情况下，它只是将内部 temp 标记为可用）。 

注意：仅在桌面上需要 `KeepAlive` 函数。 在 .NET Core 中，此方法不可用，因此编译器不会发出对它的调用。

### <a name="clr-stack-allocation-helpers"></a>CLR 堆栈分配帮助程序
CLR 只为连续内存的堆栈分配提供[localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) 。 此指令仅适用于 `unmanaged` 类型。 这意味着不能将其用作通用解决方案来有效地为 `params 
 Span<T>`分配后备存储。 

但这种限制并不是一些基本限制，而是更多的历史记录。 CLR 可以选择添加新的操作代码/内部函数，它们提供通用堆栈分配。 然后，可以使用它们为大多数 `params Span<T>` 调用分配后备存储。

``` csharp
static class BetterAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

`Go` 方法可以降低到以下各项：

``` csharp
static class ZeroAllocation {
    static void Go() {
        Span<T> span = RuntimeIntrinsic.StackAlloc<string>(length: 2);
        span[0] = "hello";
        span[1] = "world";
        Use(span);
    }
}
```

虽然这种方法非常有效，但它确实会导致额外的堆栈使用。 在具有深度堆栈和大量 `params` 使用的算法中，这可能会导致在简单 `T[]` 分配成功的情况下生成 `StackOverflowException`。 

遗憾C#的是，并没有为方法间分析进行设置，在这种情况下，可以对调用是否应使用 `params`的堆栈或堆分配进行了明智的决定。 它只能真正考虑每个调用。

CLR 最适用于在运行时进行此类确定。 因此，我们可能会让运行时为通用堆栈分配提供两种方法：

1. `Span<T> StackAlloc<T>(int length)`：这与 `localloc` 具有相同的行为和限制，但它可以在任何类型 `T`上使用。 
1. `Span<T> MaybeStackAlloc<T>(int length)`：此运行时可选择通过执行堆栈或堆分配来实现此目的。 然后，运行时可以使用调用它的执行上下文来确定如何分配 `Span<T>`。 不过，调用方将始终将其视为堆栈分配。

对于非常简单的事例，如一到两个参数C# ，编译器可以始终使用 `StackAlloc<T>` 变量。 大多数情况下，这不太可能导致堆栈耗尽。 对于其他情况，编译器可以改为选择使用 `MaybeStackAlloc<T>`，并让运行时进行调用。

我们选择的方法可能需要对实际应用进行更深入的调查和检查。 但是，如果这些新的内部函数可用，则它将为我们提供这种灵活性。

### <a name="why-not-varargs"></a>为什么不是 varargs？ 
此处的现有[varargs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017)功能被视为可行的解决方案。 此功能的主要目的是用于C++/cli 方案，并在其他方案中具有已知的漏洞。 此外，将此迁移到 Unix 需要花费大量成本。 因此，它并不是一个可行的解决方案。

## <a name="related-issues"></a>相关问题
此规范与以下问题相关： 

- https://github.com/dotnet/csharplang/issues/1757
- https://github.com/dotnet/csharplang/issues/179
- https://github.com/dotnet/corefxlab/pull/2595

