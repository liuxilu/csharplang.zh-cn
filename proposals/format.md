---
ms.openlocfilehash: 1457c1eb018e12af30ce5b38be704bf8851d4b25
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "79483936"
---
# <a name="efficient-params-and-string-formatting"></a><span data-ttu-id="fea07-101">有效的参数和字符串格式设置</span><span class="sxs-lookup"><span data-stu-id="fea07-101">Efficient Params and String Formatting</span></span>

## <a name="summary"></a><span data-ttu-id="fea07-102">总结</span><span class="sxs-lookup"><span data-stu-id="fea07-102">Summary</span></span>
<span data-ttu-id="fea07-103">这种功能组合会提高格式 `string` 值和 `params` 样式参数的传递效率。</span><span class="sxs-lookup"><span data-stu-id="fea07-103">This combination of features will increase the efficiency of formatting `string` values and passing of `params` style arguments.</span></span>

## <a name="motivation"></a><span data-ttu-id="fea07-104">动机</span><span class="sxs-lookup"><span data-stu-id="fea07-104">Motivation</span></span>
<span data-ttu-id="fea07-105">格式 `string` 值的分配开销会使多个基于文本的应用程序的性能成为主导的：从 `struct` 类型的装箱损失，在 `string` 调用期间，`params` 和中间 `string.Format` 分配的 `object[]` 分配。</span><span class="sxs-lookup"><span data-stu-id="fea07-105">The allocation overhead of formatting `string` values can dominate the performance of many text based applications: from the boxing penalty of `struct` types, the `object[]` allocation for `params` and the intermediate `string` allocations during `string.Format` calls.</span></span> <span data-ttu-id="fea07-106">为了保持高效率，此类应用程序通常需要放弃工作效率功能，如 `params` 和 `string` 插值，并迁移到非标准的、手动编码的解决方案。</span><span class="sxs-lookup"><span data-stu-id="fea07-106">In order to maintain efficiency such applications often need to abandon productivity features such as `params` and `string` interpolation and move to non-standard, hand coded solutions.</span></span> 

<span data-ttu-id="fea07-107">以 MSBuild 为例。</span><span class="sxs-lookup"><span data-stu-id="fea07-107">Consider MSBuild as an example.</span></span> <span data-ttu-id="fea07-108">这是使用许多现代C#功能编写的，开发人员意识到性能。</span><span class="sxs-lookup"><span data-stu-id="fea07-108">This is written using a lot of modern C# features by developers who are conscious of performance.</span></span> <span data-ttu-id="fea07-109">但在一个代表性的生成示例中，MSBuild 将使用最小的详细级别生成 `string` 分配的262MB。</span><span class="sxs-lookup"><span data-stu-id="fea07-109">Yet in one representative build sample MSBuild will generate 262MB of `string` allocation using minimal verbosity.</span></span> <span data-ttu-id="fea07-110">其中，1/2 的分配是 `string.Format`内的短生存期分配。</span><span class="sxs-lookup"><span data-stu-id="fea07-110">Of that 1/2 of the allocations are short lived allocations inside `string.Format`.</span></span> <span data-ttu-id="fea07-111">由于 `Span<T>` 的可用性，这些功能将在 .NET Desktop 上删除很多功能，并使其在 .NET Core 上几乎为零：</span><span class="sxs-lookup"><span data-stu-id="fea07-111">These features would remove much of that on .NET Desktop and get it down to nearly zero on .NET Core due to the availability of `Span<T>`</span></span>

<span data-ttu-id="fea07-112">此处所述的一组语言功能使应用程序能够继续使用这些功能，而不会对应用程序代码库进行极少或没有改动，同时在大多数情况下删除意外的分配开销。</span><span class="sxs-lookup"><span data-stu-id="fea07-112">The set of language features described here will enable applications to continue using these features, with very little or no churn to their application code base, while removing the unintended allocation overhead in the majority of cases.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="fea07-113">详细设计</span><span class="sxs-lookup"><span data-stu-id="fea07-113">Detailed Design</span></span> 
<span data-ttu-id="fea07-114">此处有一组功能可用于实现这些结果：</span><span class="sxs-lookup"><span data-stu-id="fea07-114">There are a set of features that will be used here to achieve these results:</span></span>

- <span data-ttu-id="fea07-115">扩展 `params` 以支持一组更广泛的集合类型。</span><span class="sxs-lookup"><span data-stu-id="fea07-115">Expanding `params` to support a broader set of collection types.</span></span>
- <span data-ttu-id="fea07-116">允许开发人员自定义如何实现 `string` 内插。</span><span class="sxs-lookup"><span data-stu-id="fea07-116">Allowing for developers to customize how `string` interpolation is achieved.</span></span> 
- <span data-ttu-id="fea07-117">允许插 `string` 绑定到更高效 `string.Format` 重载。</span><span class="sxs-lookup"><span data-stu-id="fea07-117">Allowing for interpolated `string` to bind to more efficient `string.Format` overloads.</span></span>

### <a name="extending-params"></a><span data-ttu-id="fea07-118">扩展参数</span><span class="sxs-lookup"><span data-stu-id="fea07-118">Extending params</span></span>
<span data-ttu-id="fea07-119">语言将允许方法签名中的 `params` `Span<T>`、`ReadOnlySpan<T>` 和 `IEnumerable<T>`类型。</span><span class="sxs-lookup"><span data-stu-id="fea07-119">The language will allow for `params` in a method signature to have the types `Span<T>`, `ReadOnlySpan<T>` and `IEnumerable<T>`.</span></span> <span data-ttu-id="fea07-120">调用的相同规则将应用于这些新类型，这些新类型适用于 `params T[]`：</span><span class="sxs-lookup"><span data-stu-id="fea07-120">The same rules for invocation will apply to these new types that apply to `params T[]`:</span></span>

- <span data-ttu-id="fea07-121">不能重载，其中唯一的差异是 `params` 关键字。</span><span class="sxs-lookup"><span data-stu-id="fea07-121">Can't overload where the only difference is a `params` keyword.</span></span>
- <span data-ttu-id="fea07-122">可以通过传递一系列可隐式转换为 `T` 或单个 `Span<T>` / 
`ReadOnlySpan<T>` / `IEnumerable<T>` 参数的参数来调用。</span><span class="sxs-lookup"><span data-stu-id="fea07-122">Can invoke by passing a series of arguments that are implicitly convertible to `T` or a single `Span<T>` / 
`ReadOnlySpan<T>` / `IEnumerable<T>` argument.</span></span>
- <span data-ttu-id="fea07-123">必须是方法签名中的最后一个参数。</span><span class="sxs-lookup"><span data-stu-id="fea07-123">Must be the last parameter in a method signature.</span></span>
- <span data-ttu-id="fea07-124">等等 。</span><span class="sxs-lookup"><span data-stu-id="fea07-124">Etc ...</span></span> 

<span data-ttu-id="fea07-125">为了简单起见，`Span<T>` 和 `ReadOnlySpan<T>` 变体 `Span<T>` 如下所示。</span><span class="sxs-lookup"><span data-stu-id="fea07-125">The `Span<T>` and `ReadOnlySpan<T>` variants will be referred to as `Span<T>` below for simplicity.</span></span> <span data-ttu-id="fea07-126">在 `ReadOnlySpan<T>` 的行为不同的情况下，将显式调用它。</span><span class="sxs-lookup"><span data-stu-id="fea07-126">In cases where the behavior of `ReadOnlySpan<T>` differs it will be explicitly called out.</span></span> 

<span data-ttu-id="fea07-127">`params` 提供的 `Span<T>` 变体的优点在于，它在为 `Span<T>` 值分配后备存储的方式上为编译器提供了很大的灵活性。</span><span class="sxs-lookup"><span data-stu-id="fea07-127">The advantage the `Span<T>` variants of `params` provides is it gives the compiler great flexibility in how it allocates the backing storage for the `Span<T>` value.</span></span> <span data-ttu-id="fea07-128">使用 `params T[]` 编译器必须为 `params` 方法的每个调用分配一个新 `T[]`。</span><span class="sxs-lookup"><span data-stu-id="fea07-128">With a `params T[]` the compiler must allocate a new `T[]` for every invocation of a `params` method.</span></span> <span data-ttu-id="fea07-129">重复使用是不可能的，因为它必须假定被调用方存储并重用参数。</span><span class="sxs-lookup"><span data-stu-id="fea07-129">Re-use is not possible because it must assume the callee stored and reused the parameter.</span></span> <span data-ttu-id="fea07-130">这可能会导致大量 `params` 调用的方法中的较大效率。</span><span class="sxs-lookup"><span data-stu-id="fea07-130">This can lead to a large inefficiency in methods with lots of `params` invocations.</span></span>

<span data-ttu-id="fea07-131">给定的 `Span<T>` 变体 `ref struct` 被调用方无法存储参数。</span><span class="sxs-lookup"><span data-stu-id="fea07-131">Given `Span<T>` variants are `ref struct` the callee cannot store the argument.</span></span> <span data-ttu-id="fea07-132">因此，编译器可以通过执行重复使用参数等操作来优化调用站点。</span><span class="sxs-lookup"><span data-stu-id="fea07-132">Hence the compiler can optimize the call sites by taking actions like re-using the argument.</span></span> <span data-ttu-id="fea07-133">与 `T[]`相比，这可以更高效地进行调用。</span><span class="sxs-lookup"><span data-stu-id="fea07-133">This can make repeated invocations very efficient as compared to `T[]`.</span></span> <span data-ttu-id="fea07-134">不过，该语言将不会对此类 dlr 进行优化的特定保证。</span><span class="sxs-lookup"><span data-stu-id="fea07-134">The language though will make no specific guarantees about how such callsites are optimized.</span></span> <span data-ttu-id="fea07-135">请注意，在调用 `params Span<T>` 方法时，编译器可以自由使用除 `T[]` 以外的值。</span><span class="sxs-lookup"><span data-stu-id="fea07-135">Only note that the compiler is free to use values other than `T[]` when invoking a `params Span<T>` method.</span></span> 

<span data-ttu-id="fea07-136">这种潜在的实现如下所示。</span><span class="sxs-lookup"><span data-stu-id="fea07-136">One such potential implementation is the following.</span></span> <span data-ttu-id="fea07-137">考虑方法体中的所有 `params` 调用。</span><span class="sxs-lookup"><span data-stu-id="fea07-137">Consider all `params` invocation in a method body.</span></span> <span data-ttu-id="fea07-138">编译器可以分配一个大小等于最大 `params` 调用的数组，并通过在数组上创建适当大小的 `Span<T>` 实例，将其用于所有调用。</span><span class="sxs-lookup"><span data-stu-id="fea07-138">The compiler could allocate an array which has a size equal to the largest `params` invocation and use that for all of the invocations by creating appropriately sized `Span<T>` instances over the array.</span></span> <span data-ttu-id="fea07-139">例如：</span><span class="sxs-lookup"><span data-stu-id="fea07-139">For example:</span></span>

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

<span data-ttu-id="fea07-140">编译器可能会选择发出 `Go` 的正文，如下所示：</span><span class="sxs-lookup"><span data-stu-id="fea07-140">The compiler could choose to emit the body of `Go` as follows:</span></span>

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

<span data-ttu-id="fea07-141">这可以显著减少应用程序中分配的数组数。</span><span class="sxs-lookup"><span data-stu-id="fea07-141">This can significantly reduce the number of arrays allocated in an application.</span></span> <span data-ttu-id="fea07-142">如果运行时为数组的更智能堆栈分配提供实用工具，则分配可能更进一步。</span><span class="sxs-lookup"><span data-stu-id="fea07-142">Allocations can be even further reduced if the runtime provides utilities for smarter stack allocation of arrays.</span></span>

<span data-ttu-id="fea07-143">不过，此优化不能始终应用。</span><span class="sxs-lookup"><span data-stu-id="fea07-143">This optimization cannot always be applied though.</span></span> <span data-ttu-id="fea07-144">即使被调用方无法捕获 `params` 参数，但当 `ref` 或 `out / ref` 参数本身是 `ref struct` 类型时，它仍可以在调用方中捕获。</span><span class="sxs-lookup"><span data-stu-id="fea07-144">Even though the callee cannot capture the `params` argument it can still be captured in the caller when there is a `ref` or a `out / ref` parameter that is itself a `ref struct` type.</span></span> 

``` csharp
static class SneakyCapture {
    static ref int M(params Span<T> span) => ref span[0];

    static void Oops() {
        // This now holds onto the memory backing the Span<T> 
        ref int r = ref M(42);
    }
}
```

<span data-ttu-id="fea07-145">不过，这些情况是静态检测的。</span><span class="sxs-lookup"><span data-stu-id="fea07-145">These cases are statically detectable though.</span></span> <span data-ttu-id="fea07-146">只要有 `ref` 返回或 `out` 或 `ref`传递 `ref struct` 参数，就可能会发生这种情况。</span><span class="sxs-lookup"><span data-stu-id="fea07-146">It potentially occurs whenever there is a `ref` return or a `ref struct` parameter passed by `out` or `ref`.</span></span> <span data-ttu-id="fea07-147">在这种情况下，编译器必须为每个调用分配一个全新的 `T[]`。</span><span class="sxs-lookup"><span data-stu-id="fea07-147">In such a case the compiler must allocate a fresh `T[]` for every invocation.</span></span> 

<span data-ttu-id="fea07-148">本文档末尾讨论了几个其他可能的优化策略。</span><span class="sxs-lookup"><span data-stu-id="fea07-148">Several other potential optimization strategies are discussed at the end of this document.</span></span>

<span data-ttu-id="fea07-149">`IEnumerable<T>` 变体只是一个便利重载。</span><span class="sxs-lookup"><span data-stu-id="fea07-149">The `IEnumerable<T>` variant is a merely a convenience overload.</span></span> <span data-ttu-id="fea07-150">这对于经常使用 `IEnumerable<T>` 但也有大量 `params` 使用的方案非常有用。</span><span class="sxs-lookup"><span data-stu-id="fea07-150">It's useful in scenarios which have frequent uses of `IEnumerable<T>` but also have lots of `params` usage.</span></span> <span data-ttu-id="fea07-151">在 `T` 参数形式调用时，后备存储将作为 `T[]` 分配，就像现在 `params T[]` 进行的一样。</span><span class="sxs-lookup"><span data-stu-id="fea07-151">When invoked in `T` argument form the backing storage will be allocated as a `T[]` just as `params T[]` is done today.</span></span>

### <a name="params-overload-resolution-changes"></a><span data-ttu-id="fea07-152">参数重载决策更改</span><span class="sxs-lookup"><span data-stu-id="fea07-152">params overload resolution changes</span></span>
<span data-ttu-id="fea07-153">此提议表明，该语言现在具有四个 `params` 的变体，在此之前有一个。</span><span class="sxs-lookup"><span data-stu-id="fea07-153">This proposal means the language now has four variants of `params` where before it had one.</span></span> <span data-ttu-id="fea07-154">方法只是定义唯一不同于 `params` 声明的类型的方法的重载是明智的。</span><span class="sxs-lookup"><span data-stu-id="fea07-154">It is sensible for methods to define overloads of methods that differ only on the type of a `params` declarations.</span></span> 

<span data-ttu-id="fea07-155">请考虑 `StringBuilder.AppendFormat` 除了 `params object[]`，还应添加 `params ReadOnlySpan<object>` 重载。</span><span class="sxs-lookup"><span data-stu-id="fea07-155">Consider that `StringBuilder.AppendFormat` would certainly add a `params ReadOnlySpan<object>` overload in addition to the `params object[]`.</span></span> <span data-ttu-id="fea07-156">这使得它可以通过减少收集分配来大幅提高性能，而无需对调用代码进行任何更改。</span><span class="sxs-lookup"><span data-stu-id="fea07-156">This would allow it to substantially improve performance by reducing collection allocations without requiring any changes to the calling code.</span></span> 

<span data-ttu-id="fea07-157">为了便于实现此语言，此语言会引入以下重载决策关联中断规则。</span><span class="sxs-lookup"><span data-stu-id="fea07-157">To facilitate this the language will introduce the following overload resolution tie breaking rule.</span></span> <span data-ttu-id="fea07-158">当候选方法仅在 `params` 参数上存在差异时，优先顺序如下：</span><span class="sxs-lookup"><span data-stu-id="fea07-158">When the candidate methods differ only by the `params` parameter then the candidates will be preferred in the following order:</span></span>

1. `ReadOnlySpan<T>`
1. `Span<T>`
1. `T[]`
1. `IEnumerable<T>`

<span data-ttu-id="fea07-159">此顺序最适合一般情况。</span><span class="sxs-lookup"><span data-stu-id="fea07-159">This order is the most to the least efficient for the general case.</span></span>

### <a name="variant"></a><span data-ttu-id="fea07-160">Variant</span><span class="sxs-lookup"><span data-stu-id="fea07-160">Variant</span></span>
<span data-ttu-id="fea07-161">CoreFX 正在原型为一个名为[Variant](https://github.com/dotnet/corefxlab/pull/2595)的新托管类型。</span><span class="sxs-lookup"><span data-stu-id="fea07-161">CoreFX is prototyping a new managed type named [Variant](https://github.com/dotnet/corefxlab/pull/2595).</span></span> <span data-ttu-id="fea07-162">此类型旨在在需要异类值但不希望使用 `object` 作为参数的情况下使用的 Api 中使用。</span><span class="sxs-lookup"><span data-stu-id="fea07-162">This type is meant to be used in APIs which expect heterogeneous values but don't want the overhead brought on by using `object` as the parameter.</span></span> <span data-ttu-id="fea07-163">`Variant` 类型提供通用存储，但可避免对最常用的类型进行装箱分配。</span><span class="sxs-lookup"><span data-stu-id="fea07-163">The `Variant` type provides universal storage but avoids the boxing allocation for the most commonly used types.</span></span> <span data-ttu-id="fea07-164">在诸如 `string.Format` 的 Api 中使用此类型可以消除大多数情况下的装箱开销。</span><span class="sxs-lookup"><span data-stu-id="fea07-164">Using this type in APIs like `string.Format` can eliminate the boxing overhead in the majority of cases.</span></span>

<span data-ttu-id="fea07-165">此类型本身不一定是语言的特殊类型。</span><span class="sxs-lookup"><span data-stu-id="fea07-165">This type itself is not necessarily special to the language.</span></span> <span data-ttu-id="fea07-166">它在本文档中单独引入，因为它成为方案的其他部分的实现细节。</span><span class="sxs-lookup"><span data-stu-id="fea07-166">It is being introduced in this document separately though as it becomes an implementation detail of other parts of the proposal.</span></span> 

### <a name="efficient-interpolated-strings"></a><span data-ttu-id="fea07-167">有效的内插字符串</span><span class="sxs-lookup"><span data-stu-id="fea07-167">Efficient interpolated strings</span></span>
<span data-ttu-id="fea07-168">内插字符串是中C#常见但低效的功能。</span><span class="sxs-lookup"><span data-stu-id="fea07-168">Interpolated strings are a popular yet inefficient feature in C#.</span></span> <span data-ttu-id="fea07-169">使用内插 `string` 作为 `string`的最常见语法转换为 `string.Format(string, params object[])` 调用。</span><span class="sxs-lookup"><span data-stu-id="fea07-169">The most common syntax, using an interpolated `string` as a `string`, translates into a `string.Format(string, params object[])` call.</span></span> <span data-ttu-id="fea07-170">这将导致所有值类型的装箱分配，中间 `string` 分配作为实现的主要用途是在参数数目超出了 `string.Format`"快速" 重载的参数量的情况下，使用 `object.ToString` 进行格式设置以及数组分配。</span><span class="sxs-lookup"><span data-stu-id="fea07-170">That will incur boxing allocations for all value types, intermediate `string` allocations as the implementation largely uses `object.ToString` for formatting as well as array allocations once the number of arguments exceeds the amount of parameters on the "fast" overloads of `string.Format`.</span></span> 

<span data-ttu-id="fea07-171">该语言将更改它的内插，以考虑 `string.Format`的备用重载。</span><span class="sxs-lookup"><span data-stu-id="fea07-171">The language will change its interpolation lowering to consider alternate overloads of `string.Format`.</span></span> <span data-ttu-id="fea07-172">它将考虑 `string.Format(string, params)` 的所有形式，并选取满足参数类型的 "最佳" 重载。</span><span class="sxs-lookup"><span data-stu-id="fea07-172">It will consider all forms of `string.Format(string, params)` and pick the "best" overload which satisfies the argument types.</span></span>
<span data-ttu-id="fea07-173">"最佳" `params` 重载将由前面讨论的规则确定。</span><span class="sxs-lookup"><span data-stu-id="fea07-173">The "best" `params` overload will be determined by the rules discussed above.</span></span> <span data-ttu-id="fea07-174">这意味着内插 `string` 现在可以绑定到非常高效的重载，如 `string.Format(string format, params ReadOnlySpan<Variant> args)`。</span><span class="sxs-lookup"><span data-stu-id="fea07-174">This means interpolated `string` can now bind to very efficient overloads like `string.Format(string format, params ReadOnlySpan<Variant> args)`.</span></span> <span data-ttu-id="fea07-175">在许多情况下，这将删除所有中间分配。</span><span class="sxs-lookup"><span data-stu-id="fea07-175">In many cases this will remove all intermediate allocations.</span></span>

### <a name="customizable-interpolated-strings"></a><span data-ttu-id="fea07-176">可自定义的内插字符串</span><span class="sxs-lookup"><span data-stu-id="fea07-176">Customizable interpolated strings</span></span>
<span data-ttu-id="fea07-177">开发人员能够通过 `FormattableString`自定义内插字符串的行为。</span><span class="sxs-lookup"><span data-stu-id="fea07-177">Developers are able to customize the behavior of interpolated strings with `FormattableString`.</span></span> <span data-ttu-id="fea07-178">这包含进入内插字符串的数据：格式 `string` 和作为数组的参数。</span><span class="sxs-lookup"><span data-stu-id="fea07-178">This contains the data which goes into an interpolated string: the format `string` and the arguments as an array.</span></span> <span data-ttu-id="fea07-179">但这仍具有装箱和参数数组分配以及 `FormattableString` （这是一个 `abstract class`）的分配。</span><span class="sxs-lookup"><span data-stu-id="fea07-179">This though still has the boxing and argument array allocation as well as the allocation for `FormattableString` (it's an `abstract class`).</span></span> <span data-ttu-id="fea07-180">因此，几乎不能使用 `string` 格式的分配繁重的应用程序。</span><span class="sxs-lookup"><span data-stu-id="fea07-180">Hence it's of little use to applications which are allocation heavy in `string` formatting.</span></span>

<span data-ttu-id="fea07-181">为了高效地进行内插字符串格式设置，语言将识别新的类型： `System.ValueFormattableString`。</span><span class="sxs-lookup"><span data-stu-id="fea07-181">To make interpolated string formatting efficient the language will recognize a new type: `System.ValueFormattableString`.</span></span> <span data-ttu-id="fea07-182">所有内插字符串都将具有到此类型的目标类型转换。</span><span class="sxs-lookup"><span data-stu-id="fea07-182">All interpolated strings will have a target type conversion to this type.</span></span> <span data-ttu-id="fea07-183">这将通过将内插字符串转换到调用中 `ValueFormattableString.Create` 完全按照今天 `FormattableString.Create` 的方式来实现。</span><span class="sxs-lookup"><span data-stu-id="fea07-183">This will be implemented by translating the interpolated string into the call `ValueFormattableString.Create` exactly as is done for `FormattableString.Create` today.</span></span> <span data-ttu-id="fea07-184">查找最适合的 `ValueFormattableString.Create` 方法时，该语言将支持本文档中所述的所有 `params` 选项。</span><span class="sxs-lookup"><span data-stu-id="fea07-184">The language will support all `params` options described in this document when looking for the most suitable `ValueFormattableString.Create` method.</span></span> 

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

<span data-ttu-id="fea07-185">当参数为内插字符串时，重载决策规则将更改为首选 `ValueFormattableString` 超出 `string`。</span><span class="sxs-lookup"><span data-stu-id="fea07-185">Overload resolution rules will be changed to prefer `ValueFormattableString` over `string` when the argument is an interpolated string.</span></span> <span data-ttu-id="fea07-186">这意味着，具有仅在 `string` 和 `ValueFormattableString`上不同的重载会很有价值。</span><span class="sxs-lookup"><span data-stu-id="fea07-186">This means it will be valuable to have overloads which differ only on `string` and `ValueFormattableString`.</span></span> <span data-ttu-id="fea07-187">现在使用 `FormattableString` 的这种重载并不重要，因为编译器将始终优先于 `string` 版本（除非开发人员使用显式强制转换）。</span><span class="sxs-lookup"><span data-stu-id="fea07-187">Such an overload today with `FormattableString` is not valuable as the compiler will always prefer the `string` version (unless the developer uses an explicit cast).</span></span> 

## <a name="open-issues"></a><span data-ttu-id="fea07-188">打开的问题</span><span class="sxs-lookup"><span data-stu-id="fea07-188">Open Issues</span></span>

### <a name="valueformattablestring-breaking-change"></a><span data-ttu-id="fea07-189">ValueFormattableString 重大更改</span><span class="sxs-lookup"><span data-stu-id="fea07-189">ValueFormattableString breaking change</span></span>
<span data-ttu-id="fea07-190">对 `string` 的重载决策过程中首选 `ValueFormattableString` 的更改是一项重大更改。</span><span class="sxs-lookup"><span data-stu-id="fea07-190">The change to prefer `ValueFormattableString` during overload resolution over `string` is a breaking change.</span></span> <span data-ttu-id="fea07-191">开发人员可以定义目前称为 `ValueFormattableString` 类型，并在 `string`的方法重载中使用该类型。</span><span class="sxs-lookup"><span data-stu-id="fea07-191">It is possible for a developer to have defined a type called `ValueFormattableString` today and use it in method overloads with `string`.</span></span> <span data-ttu-id="fea07-192">此建议的更改将导致编译器在此功能集实现后选取不同重载。</span><span class="sxs-lookup"><span data-stu-id="fea07-192">This proposed change would cause the compiler to pick a different overload once this set of features was implemented.</span></span> 

<span data-ttu-id="fea07-193">出现这种情况的可能性非常低。</span><span class="sxs-lookup"><span data-stu-id="fea07-193">The possibility of this seems reasonably low.</span></span> <span data-ttu-id="fea07-194">类型需要 `System.ValueFormattableString` 全名，并且需要具有名为 `Create``static` 方法。</span><span class="sxs-lookup"><span data-stu-id="fea07-194">The type would need the full name `System.ValueFormattableString` and it would need to have `static` methods named `Create`.</span></span> <span data-ttu-id="fea07-195">鉴于开发人员强烈建议不要定义 `System` 命名空间中的任何类型，这种做法似乎是一个合理的折衷。</span><span class="sxs-lookup"><span data-stu-id="fea07-195">Given that developers are strongly discouraged from defining any type in the `System` namespace this break seems like a reasonable compromise.</span></span>

### <a name="expanding-to-more-types"></a><span data-ttu-id="fea07-196">扩展到更多类型</span><span class="sxs-lookup"><span data-stu-id="fea07-196">Expanding to more types</span></span>
<span data-ttu-id="fea07-197">假设我们位于区域，应考虑将 `IList<T>`、`ICollection<T>` 和 `IReadOnlyList<T>` 添加到支持 `params` 的集合集。</span><span class="sxs-lookup"><span data-stu-id="fea07-197">Given we're in the area we should consider adding `IList<T>`, `ICollection<T>` and `IReadOnlyList<T>` to the set of collections for which `params` is supported.</span></span> <span data-ttu-id="fea07-198">就实现而言，这里的其他工作将花费较小的费用。</span><span class="sxs-lookup"><span data-stu-id="fea07-198">In terms of implementation it will cost a small amount over the other work here.</span></span>

<span data-ttu-id="fea07-199">不过，LDM 需要确定是否有必要的复杂语言。</span><span class="sxs-lookup"><span data-stu-id="fea07-199">LDM needs to decide if the complication to the language is worth it though.</span></span> <span data-ttu-id="fea07-200">添加 `IEnumerable<T>` 会删除非常特定的摩擦点。</span><span class="sxs-lookup"><span data-stu-id="fea07-200">The addition of `IEnumerable<T>` removes a very specific friction point.</span></span> <span data-ttu-id="fea07-201">缺少这 `params` 解决方案，许多客户在调用 `params` 方法时被迫从 `IEnumerable<T>` 分配 `T[]`。</span><span class="sxs-lookup"><span data-stu-id="fea07-201">Lacking this `params` solution many customers were forced to allocate `T[]` from an `IEnumerable<T>` when calling a `params` method.</span></span> <span data-ttu-id="fea07-202">但添加 `IEnumerable<T>` 会解决此问题。</span><span class="sxs-lookup"><span data-stu-id="fea07-202">The addition of `IEnumerable<T>` fixes this though.</span></span> <span data-ttu-id="fea07-203">其他接口在此不会解决的特定摩擦点。</span><span class="sxs-lookup"><span data-stu-id="fea07-203">There is no specific friction point that the other interfaces fix here.</span></span> 

## <a name="considerations"></a><span data-ttu-id="fea07-204">注意事项</span><span class="sxs-lookup"><span data-stu-id="fea07-204">Considerations</span></span>

### <a name="variant2-and-variant3"></a><span data-ttu-id="fea07-205">Variant2 和 Variant3</span><span class="sxs-lookup"><span data-stu-id="fea07-205">Variant2 and Variant3</span></span>
<span data-ttu-id="fea07-206">CoreFX 团队还具有一组非分配的存储类型，最多包含三个 `Variant` 参数。</span><span class="sxs-lookup"><span data-stu-id="fea07-206">The CoreFX team also has a non-allocating set of storage types for up to three `Variant` arguments.</span></span> <span data-ttu-id="fea07-207">这些是单个 `Variant``Variant2` 和 `Variant3`。</span><span class="sxs-lookup"><span data-stu-id="fea07-207">These are a single `Variant`, `Variant2` and `Variant3`.</span></span> <span data-ttu-id="fea07-208">所有方法都具有一对用于 `Span<Variant>` 之外获取分配的方法： `CreateSpan` 和 `KeepAlive`。</span><span class="sxs-lookup"><span data-stu-id="fea07-208">All have a pair of methods for getting an allocation free `Span<Variant>` off of them: `CreateSpan` and `KeepAlive`.</span></span> <span data-ttu-id="fea07-209">这意味着，对于最多三个参数的 `params Span<Variant>`，调用站点可以完全自由分配。</span><span class="sxs-lookup"><span data-stu-id="fea07-209">This means for a `params Span<Variant>` of up to three arguments the call site can be entirely allocation free.</span></span>

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

<span data-ttu-id="fea07-210">`Go` 方法可以降低到以下各项：</span><span class="sxs-lookup"><span data-stu-id="fea07-210">The `Go` method can be lowered to the following:</span></span>

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

<span data-ttu-id="fea07-211">这几乎不需要在建议的基础上重复使用 `params Span<T>` 调用之间 `T[]`。</span><span class="sxs-lookup"><span data-stu-id="fea07-211">This requires very little work on top of the proposal to re-use `T[]` between `params Span<T>` calls.</span></span> <span data-ttu-id="fea07-212">编译器已经需要按调用管理临时，并在一种情况下进行清理工作（即使在一种情况下，它只是将内部 temp 标记为可用）。</span><span class="sxs-lookup"><span data-stu-id="fea07-212">The compiler already needs to manage a temporary per call and do clean up work after (even if in one case it's just marking an internal temp as free).</span></span> 

<span data-ttu-id="fea07-213">注意：仅在桌面上需要 `KeepAlive` 函数。</span><span class="sxs-lookup"><span data-stu-id="fea07-213">Note: the `KeepAlive` function is only necessary on desktop.</span></span> <span data-ttu-id="fea07-214">在 .NET Core 中，此方法不可用，因此编译器不会发出对它的调用。</span><span class="sxs-lookup"><span data-stu-id="fea07-214">On .NET Core the method will not be available and hence the compiler won't emit a call to it.</span></span>

### <a name="clr-stack-allocation-helpers"></a><span data-ttu-id="fea07-215">CLR 堆栈分配帮助程序</span><span class="sxs-lookup"><span data-stu-id="fea07-215">CLR stack allocation helpers</span></span>
<span data-ttu-id="fea07-216">CLR 只为连续内存的堆栈分配提供[localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) 。</span><span class="sxs-lookup"><span data-stu-id="fea07-216">The CLR only provides only [localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) for stack allocation of contiguous memory.</span></span> <span data-ttu-id="fea07-217">此指令仅适用于 `unmanaged` 类型。</span><span class="sxs-lookup"><span data-stu-id="fea07-217">This instruction is limited in that it only works for `unmanaged` types.</span></span> <span data-ttu-id="fea07-218">这意味着不能将其用作通用解决方案来有效地为 `params 
 Span<T>`分配后备存储。</span><span class="sxs-lookup"><span data-stu-id="fea07-218">This means it can't be used as a universal solution for efficiently allocating the backing storage for `params 
 Span<T>`.</span></span> 

<span data-ttu-id="fea07-219">但这种限制并不是一些基本限制，而是更多的历史记录。</span><span class="sxs-lookup"><span data-stu-id="fea07-219">This limitation is not some fundamental restriction though but instead more an artifact of history.</span></span> <span data-ttu-id="fea07-220">CLR 可以选择添加新的操作代码/内部函数，它们提供通用堆栈分配。</span><span class="sxs-lookup"><span data-stu-id="fea07-220">The CLR could choose to add new op codes / intrinsics which provide universal stack allocation.</span></span> <span data-ttu-id="fea07-221">然后，可以使用它们为大多数 `params Span<T>` 调用分配后备存储。</span><span class="sxs-lookup"><span data-stu-id="fea07-221">These could then be used to allocate the backing storage for most `params Span<T>` calls.</span></span>

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

<span data-ttu-id="fea07-222">`Go` 方法可以降低到以下各项：</span><span class="sxs-lookup"><span data-stu-id="fea07-222">The `Go` method can be lowered to the following:</span></span>

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

<span data-ttu-id="fea07-223">虽然这种方法非常有效，但它确实会导致额外的堆栈使用。</span><span class="sxs-lookup"><span data-stu-id="fea07-223">While this approach is very heap efficient it does cause extra stack usage.</span></span> <span data-ttu-id="fea07-224">在具有深度堆栈和大量 `params` 使用的算法中，这可能会导致在简单 `T[]` 分配成功的情况下生成 `StackOverflowException`。</span><span class="sxs-lookup"><span data-stu-id="fea07-224">In an algorithm which has a deep stack and lots of `params` usage it's possible this could cause a `StackOverflowException` to be generated where a simple `T[]` allocation would succeed.</span></span> 

<span data-ttu-id="fea07-225">遗憾C#的是，并没有为方法间分析进行设置，在这种情况下，可以对调用是否应使用 `params`的堆栈或堆分配进行了明智的决定。</span><span class="sxs-lookup"><span data-stu-id="fea07-225">Unfortunately C# is not set up for the type of inter-method analysis where it could make an educated determination of whether or not call should use stack or heap allocation of `params`.</span></span> <span data-ttu-id="fea07-226">它只能真正考虑每个调用。</span><span class="sxs-lookup"><span data-stu-id="fea07-226">It can only really consider each call on its own.</span></span>

<span data-ttu-id="fea07-227">CLR 最适用于在运行时进行此类确定。</span><span class="sxs-lookup"><span data-stu-id="fea07-227">The CLR is best setup for making this type of determination at runtime.</span></span> <span data-ttu-id="fea07-228">因此，我们可能会让运行时为通用堆栈分配提供两种方法：</span><span class="sxs-lookup"><span data-stu-id="fea07-228">Hence we'd likely have the runtime provide two methods for universal stack allocation:</span></span>

1. <span data-ttu-id="fea07-229">`Span<T> StackAlloc<T>(int length)`：这与 `localloc` 具有相同的行为和限制，但它可以在任何类型 `T`上使用。</span><span class="sxs-lookup"><span data-stu-id="fea07-229">`Span<T> StackAlloc<T>(int length)`: this has the same behaviors and limitations of `localloc` except it can work on any type `T`.</span></span> 
1. <span data-ttu-id="fea07-230">`Span<T> MaybeStackAlloc<T>(int length)`：此运行时可选择通过执行堆栈或堆分配来实现此目的。</span><span class="sxs-lookup"><span data-stu-id="fea07-230">`Span<T> MaybeStackAlloc<T>(int length)`: this runtime can choose to implement this by doing a stack or heap allocation.</span></span> <span data-ttu-id="fea07-231">然后，运行时可以使用调用它的执行上下文来确定如何分配 `Span<T>`。</span><span class="sxs-lookup"><span data-stu-id="fea07-231">The runtime can then use the execution context in which it's called to determine how the `Span<T>` is allocated.</span></span> <span data-ttu-id="fea07-232">不过，调用方将始终将其视为堆栈分配。</span><span class="sxs-lookup"><span data-stu-id="fea07-232">The caller though will always treat it as if it were stack allocated.</span></span>

<span data-ttu-id="fea07-233">对于非常简单的事例，如一到两个参数C# ，编译器可以始终使用 `StackAlloc<T>` 变量。</span><span class="sxs-lookup"><span data-stu-id="fea07-233">For very simple cases, like one to two arguments, the C# compiler could always use `StackAlloc<T>` variant.</span></span> <span data-ttu-id="fea07-234">大多数情况下，这不太可能导致堆栈耗尽。</span><span class="sxs-lookup"><span data-stu-id="fea07-234">This is unlikely to significantly contribute to stack exhaustion in most cases.</span></span> <span data-ttu-id="fea07-235">对于其他情况，编译器可以改为选择使用 `MaybeStackAlloc<T>`，并让运行时进行调用。</span><span class="sxs-lookup"><span data-stu-id="fea07-235">For other cases the compiler could choose to use `MaybeStackAlloc<T>` instead and let the runtime make the call.</span></span>

<span data-ttu-id="fea07-236">我们选择的方法可能需要对实际应用进行更深入的调查和检查。</span><span class="sxs-lookup"><span data-stu-id="fea07-236">How we choose will likely require a deeper investigation and examination of real world apps.</span></span> <span data-ttu-id="fea07-237">但是，如果这些新的内部函数可用，则它将为我们提供这种灵活性。</span><span class="sxs-lookup"><span data-stu-id="fea07-237">But if these new intrinsics are available then it will give us this type of flexibility.</span></span>

### <a name="why-not-varargs"></a><span data-ttu-id="fea07-238">为什么不是 varargs？</span><span class="sxs-lookup"><span data-stu-id="fea07-238">Why not varargs?</span></span> 
<span data-ttu-id="fea07-239">此处的现有[varargs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017)功能被视为可行的解决方案。</span><span class="sxs-lookup"><span data-stu-id="fea07-239">The existing [varargs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017) feature was considered here as a possible solution.</span></span> <span data-ttu-id="fea07-240">此功能的主要目的是用于C++/cli 方案，并在其他方案中具有已知的漏洞。</span><span class="sxs-lookup"><span data-stu-id="fea07-240">This feature though is meant primarily for C++/CLI scenarios and has known holes for other scenarios.</span></span> <span data-ttu-id="fea07-241">此外，将此迁移到 Unix 需要花费大量成本。</span><span class="sxs-lookup"><span data-stu-id="fea07-241">Additionally there is significant cost in porting this to Unix.</span></span> <span data-ttu-id="fea07-242">因此，它并不是一个可行的解决方案。</span><span class="sxs-lookup"><span data-stu-id="fea07-242">Hence it wasn't seen as a viable solution.</span></span>

## <a name="related-issues"></a><span data-ttu-id="fea07-243">相关问题</span><span class="sxs-lookup"><span data-stu-id="fea07-243">Related Issues</span></span>
<span data-ttu-id="fea07-244">此规范与以下问题相关：</span><span class="sxs-lookup"><span data-stu-id="fea07-244">This spec is related to the following issues:</span></span> 

- https://github.com/dotnet/csharplang/issues/1757
- https://github.com/dotnet/csharplang/issues/179
- https://github.com/dotnet/corefxlab/pull/2595

