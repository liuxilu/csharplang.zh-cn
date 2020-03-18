---
ms.openlocfilehash: fa3326bf69c83b6042b1db7b5567fd5c28d6f81a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483516"
---
# <a name="callerargumentexpression"></a><span data-ttu-id="63963-101">CallerArgumentExpression</span><span class="sxs-lookup"><span data-stu-id="63963-101">CallerArgumentExpression</span></span>

* <span data-ttu-id="63963-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="63963-102">[x] Proposed</span></span>
* <span data-ttu-id="63963-103">[] 原型：未启动</span><span class="sxs-lookup"><span data-stu-id="63963-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="63963-104">[] 实现：未启动</span><span class="sxs-lookup"><span data-stu-id="63963-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="63963-105">[] 规范：未启动</span><span class="sxs-lookup"><span data-stu-id="63963-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="63963-106">总结</span><span class="sxs-lookup"><span data-stu-id="63963-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="63963-107">允许开发人员捕获传递给方法的表达式，在诊断/测试 Api 中启用更好的错误消息并减少击键次数。</span><span class="sxs-lookup"><span data-stu-id="63963-107">Allow developers to capture the expressions passed to a method, to enable better error messages in diagnostic/testing APIs and reduce keystrokes.</span></span>

## <a name="motivation"></a><span data-ttu-id="63963-108">动机</span><span class="sxs-lookup"><span data-stu-id="63963-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="63963-109">当断言或参数验证失败时，开发人员希望尽可能多地了解其失败的位置和原因。</span><span class="sxs-lookup"><span data-stu-id="63963-109">When an assertion or argument validation fails, the developer wants to know as much as possible about where and why it failed.</span></span> <span data-ttu-id="63963-110">但今天的诊断 Api 并不能完全简化这一点。</span><span class="sxs-lookup"><span data-stu-id="63963-110">However, today's diagnostic APIs do not fully facilitate this.</span></span> <span data-ttu-id="63963-111">请考虑以下方法：</span><span class="sxs-lookup"><span data-stu-id="63963-111">Consider the following method:</span></span>

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null);
    Debug.Assert(array.Length == 1);

    return array[0];
}
```

<span data-ttu-id="63963-112">当其中一个断言失败时，堆栈跟踪中只会提供文件名、行号和方法名。</span><span class="sxs-lookup"><span data-stu-id="63963-112">When one of the asserts fail, only the filename, line number, and method name will be provided in the stack trace.</span></span> <span data-ttu-id="63963-113">开发人员无法判断哪些断言未能通过此信息--（s）他必须打开文件并导航到提供的行号来查看发生了什么问题。</span><span class="sxs-lookup"><span data-stu-id="63963-113">The developer will not be able to tell which assert failed from this information-- (s)he will have to open the file and navigate to the provided line number to see what went wrong.</span></span>

<span data-ttu-id="63963-114">这也是测试框架必须提供各种断言方法的原因。</span><span class="sxs-lookup"><span data-stu-id="63963-114">This is also the reason testing frameworks have to provide a variety of assert methods.</span></span> <span data-ttu-id="63963-115">使用 xUnit 时，不经常使用 `Assert.True` 和 `Assert.False`，因为它们不能提供有关失败原因的足够上下文。</span><span class="sxs-lookup"><span data-stu-id="63963-115">With xUnit, `Assert.True` and `Assert.False` are not frequently used because they do not provide enough context about what failed.</span></span>

<span data-ttu-id="63963-116">虽然对于参数验证而言，这种情况更好一些，因为开发人员向开发人员显示无效参数的名称，开发人员必须手动将这些名称传递给异常。</span><span class="sxs-lookup"><span data-stu-id="63963-116">While the situation is a bit better for argument validation because the names of invalid arguments are shown to the developer, the developer must pass these names to exceptions manually.</span></span> <span data-ttu-id="63963-117">如果重写上面的示例以使用传统参数验证而不是 `Debug.Assert`，它将如下所示</span><span class="sxs-lookup"><span data-stu-id="63963-117">If the above example were rewritten to use traditional argument validation instead of `Debug.Assert`, it would look like</span></span>

```csharp
T Single<T>(this T[] array)
{
    if (array == null)
    {
        throw new ArgumentNullException(nameof(array));
    }

    if (array.Length != 1)
    {
        throw new ArgumentException("Array must contain a single element.", nameof(array));
    }

    return array[0];
}
```

<span data-ttu-id="63963-118">请注意，必须将 `nameof(array)` 传递给每个异常，尽管它在自变量无效的上下文中已经明确了。</span><span class="sxs-lookup"><span data-stu-id="63963-118">Notice that `nameof(array)` must be passed to each exception, although it's already clear from context which argument is invalid.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="63963-119">详细设计</span><span class="sxs-lookup"><span data-stu-id="63963-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="63963-120">在上面的示例中，包含断言消息中的字符串 `"array != null"` 或 `"array.Length == 1"` 有助于开发人员确定失败的内容。</span><span class="sxs-lookup"><span data-stu-id="63963-120">In the above examples, including the string `"array != null"` or `"array.Length == 1"` in the assert message would help the developer determine what failed.</span></span> <span data-ttu-id="63963-121">输入 `CallerArgumentExpression`：此属性是框架可用于获取与特定方法参数关联的字符串的属性。</span><span class="sxs-lookup"><span data-stu-id="63963-121">Enter `CallerArgumentExpression`: it's an attribute the framework can use to obtain the string associated with a particular method argument.</span></span> <span data-ttu-id="63963-122">我们会将它添加到 `Debug.Assert`，如</span><span class="sxs-lookup"><span data-stu-id="63963-122">We would add it to `Debug.Assert` like so</span></span>

```csharp
public static class Debug
{
    public static void Assert(bool condition, [CallerArgumentExpression("condition")] string message = null);
}
```

<span data-ttu-id="63963-123">以上示例中的源代码将保持不变。</span><span class="sxs-lookup"><span data-stu-id="63963-123">The source code in the above example would stay the same.</span></span> <span data-ttu-id="63963-124">但编译器实际发出的代码将对应于</span><span class="sxs-lookup"><span data-stu-id="63963-124">However, the code the compiler actually emits would correspond to</span></span>

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null, "array != null");
    Debug.Assert(array.Length == 1, "array.Length == 1");

    return array[0];
}
```

<span data-ttu-id="63963-125">编译器专门识别 `Debug.Assert`上的属性。</span><span class="sxs-lookup"><span data-stu-id="63963-125">The compiler specially recognizes the attribute on `Debug.Assert`.</span></span> <span data-ttu-id="63963-126">它在调用站点传递与属性的构造函数中引用的参数（在本例中为 `condition`）关联的字符串。</span><span class="sxs-lookup"><span data-stu-id="63963-126">It passes the string associated with the argument referred to in the attribute's constructor (in this case, `condition`) at the call site.</span></span> <span data-ttu-id="63963-127">当任何一个断言失败时，开发人员将显示 "假" 这一条件，并知道哪一项失败。</span><span class="sxs-lookup"><span data-stu-id="63963-127">When either assert fails, the developer will be shown the condition that was false and will know which one failed.</span></span>

<span data-ttu-id="63963-128">对于参数验证，特性不能直接使用，但可以通过帮助器类来使用：</span><span class="sxs-lookup"><span data-stu-id="63963-128">For argument validation, the attribute cannot be used directly, but can be made use of through a helper class:</span></span>

```csharp
public static class Verify
{
    public static void Argument(bool condition, string message, [CallerArgumentExpression("condition")] string conditionExpression = null)
    {
        if (!condition) throw new ArgumentException(message: message, paramName: conditionExpression);
    }

    public static void InRange(int argument, int low, int high,
        [CallerArgumentExpression("argument")] string argumentExpression = null,
        [CallerArgumentExpression("low")] string lowExpression = null,
        [CallerArgumentExpression("high")] string highExpression = null)
    {
        if (argument < low)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be less than {lowExpression} ({low}).");
        }

        if (argument > high)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be greater than {highExpression} ({high}).");
        }
    }

    public static void NotNull<T>(T argument, [CallerArgumentExpression("argument")] string argumentExpression = null)
        where T : class
    {
        if (argument == null) throw new ArgumentNullException(paramName: argumentExpression);
    }
}

T Single<T>(this T[] array)
{
    Verify.NotNull(array); // paramName: "array"
    Verify.Argument(array.Length == 1, "Array must contain a single element."); // paramName: "array.Length == 1"

    return array[0];
}

T ElementAt(this T[] array, int index)
{
    Verify.NotNull(array); // paramName: "array"
    // paramName: "index"
    // message: "index (-1) cannot be less than 0 (0).", or
    //          "index (6) cannot be greater than array.Length - 1 (5)."
    Verify.InRange(index, 0, array.Length - 1);

    return array[index];
}
```

<span data-ttu-id="63963-129">将此类帮助器类添加到框架的建议在 https://github.com/dotnet/corefx/issues/17068进行。</span><span class="sxs-lookup"><span data-stu-id="63963-129">A proposal to add such a helper class to the framework is underway at https://github.com/dotnet/corefx/issues/17068.</span></span> <span data-ttu-id="63963-130">如果实现了此语言功能，则可以更新该建议以利用此功能。</span><span class="sxs-lookup"><span data-stu-id="63963-130">If this language feature was implemented, the proposal could be updated to take advantage of this feature.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="63963-131">扩展方法</span><span class="sxs-lookup"><span data-stu-id="63963-131">Extension methods</span></span>

<span data-ttu-id="63963-132">扩展方法中的 `this` 参数可能由 `CallerArgumentExpression`引用。</span><span class="sxs-lookup"><span data-stu-id="63963-132">The `this` parameter in an extension method may be referenced by `CallerArgumentExpression`.</span></span> <span data-ttu-id="63963-133">例如：</span><span class="sxs-lookup"><span data-stu-id="63963-133">For example:</span></span>

```csharp
public static void ShouldBe<T>(this T @this, T expected, [CallerArgumentExpression("this")] string thisExpression = null) {}

contestant.Points.ShouldBe(1337); // thisExpression: "contestant.Points"
```

<span data-ttu-id="63963-134">`thisExpression` 将接收与点前面的对象相对应的表达式。</span><span class="sxs-lookup"><span data-stu-id="63963-134">`thisExpression` will receive the expression corresponding to the object before the dot.</span></span> <span data-ttu-id="63963-135">如果它是通过静态方法语法（如 `Ext.ShouldBe(contestant.Points, 1337)`）调用的，则其行为就像第一个参数未标记为 `this`。</span><span class="sxs-lookup"><span data-stu-id="63963-135">If it's called with static method syntax, e.g. `Ext.ShouldBe(contestant.Points, 1337)`, it will behave as if first parameter wasn't marked `this`.</span></span>

<span data-ttu-id="63963-136">应始终存在与 `this` 参数对应的表达式。</span><span class="sxs-lookup"><span data-stu-id="63963-136">There should always be an expression corresponding to the `this` parameter.</span></span> <span data-ttu-id="63963-137">即使类的实例自行调用扩展方法（例如从集合类型内 `this.Single()`），`this` 也由编译器强制，因此 `"this"` 会通过。</span><span class="sxs-lookup"><span data-stu-id="63963-137">Even if an instance of a class calls an extension method on itself, e.g. `this.Single()` from inside a collection type, the `this` is mandated by the compiler so `"this"` will get passed.</span></span> <span data-ttu-id="63963-138">如果将来更改此规则，可以考虑传递 `null` 或空字符串。</span><span class="sxs-lookup"><span data-stu-id="63963-138">If this rule is changed in the future, we can consider passing `null` or the empty string.</span></span>

### <a name="extra-details"></a><span data-ttu-id="63963-139">额外详细信息</span><span class="sxs-lookup"><span data-stu-id="63963-139">Extra details</span></span>

- <span data-ttu-id="63963-140">与其他 `Caller*` 属性（如 `CallerMemberName`）一样，此属性只能用于具有默认值的参数。</span><span class="sxs-lookup"><span data-stu-id="63963-140">Like the other `Caller*` attributes, such as `CallerMemberName`, this attribute may only be used on parameters with default values.</span></span>
- <span data-ttu-id="63963-141">允许使用 `CallerArgumentExpression` 标记的多个参数，如上所示。</span><span class="sxs-lookup"><span data-stu-id="63963-141">Multiple parameters marked with `CallerArgumentExpression` are permitted, as shown above.</span></span>
- <span data-ttu-id="63963-142">将 `System.Runtime.CompilerServices`特性的命名空间。</span><span class="sxs-lookup"><span data-stu-id="63963-142">The attribute's namespace will be `System.Runtime.CompilerServices`.</span></span>
- <span data-ttu-id="63963-143">如果提供 `null` 或非参数名称的字符串（如 `"notAParameterName"`），则编译器将传入一个空字符串。</span><span class="sxs-lookup"><span data-stu-id="63963-143">If `null` or a string that is not a parameter name (e.g. `"notAParameterName"`) is provided, the compiler will pass in an empty string.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="63963-144">缺点</span><span class="sxs-lookup"><span data-stu-id="63963-144">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="63963-145">知道如何使用 decompilers 的人员能够在调用站点上查看使用此特性标记的方法的一些源代码。</span><span class="sxs-lookup"><span data-stu-id="63963-145">People who know how to use decompilers will be able to see some of the source code at call sites for methods marked with this attribute.</span></span> <span data-ttu-id="63963-146">对于关闭的源软件，这可能是不必要的/意外的。</span><span class="sxs-lookup"><span data-stu-id="63963-146">This may be undesirable/unexpected for closed-source software.</span></span>

- <span data-ttu-id="63963-147">尽管这不是功能本身中的一个缺陷，但仍存在一个问题来源，因为目前只有一个 `bool`的 `Debug.Assert` API。</span><span class="sxs-lookup"><span data-stu-id="63963-147">Although this is not a flaw in the feature itself, a source of concern may be that there exists a `Debug.Assert` API today that only takes a `bool`.</span></span> <span data-ttu-id="63963-148">即使采用消息的重载的第二个参数使用此属性进行标记并使其成为可选参数，编译器仍会在重载决策中选取非消息一。</span><span class="sxs-lookup"><span data-stu-id="63963-148">Even if the overload taking a message had its second parameter marked with this attribute and made optional, the compiler would still pick the no-message one in overload resolution.</span></span> <span data-ttu-id="63963-149">因此，必须删除非消息重载才能利用此功能，这将是二进制（尽管不是源）的重大更改。</span><span class="sxs-lookup"><span data-stu-id="63963-149">Therefore, the no-message overload would have to be removed to take advantage of this feature, which would be a binary (although not source) breaking change.</span></span>

## <a name="alternatives"></a><span data-ttu-id="63963-150">备选项</span><span class="sxs-lookup"><span data-stu-id="63963-150">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="63963-151">如果可以在调用站点上查看使用此属性证明有问题的方法的源代码，则可以选择启用该属性的效果。</span><span class="sxs-lookup"><span data-stu-id="63963-151">If being able to see source code at call sites for methods that use this attribute proves to be a problem, we can make the attribute's effects opt-in.</span></span> <span data-ttu-id="63963-152">开发人员将通过其置于 `AssemblyInfo.cs`中的程序集范围 `[assembly: EnableCallerArgumentExpression]` 特性来启用它。</span><span class="sxs-lookup"><span data-stu-id="63963-152">Developers will enable it through an assembly-wide `[assembly: EnableCallerArgumentExpression]` attribute they put in `AssemblyInfo.cs`.</span></span>
  - <span data-ttu-id="63963-153">如果未启用该特性的效果，则调用用特性标记的方法将不会导致错误，从而使现有方法能够使用特性并维护源兼容性。</span><span class="sxs-lookup"><span data-stu-id="63963-153">In the case the attribute's effects are not enabled, calling methods marked with the attribute would not be an error, to allow existing methods to use the attribute and maintain source compatibility.</span></span> <span data-ttu-id="63963-154">但是，将忽略属性，并将通过提供的任何默认值调用方法。</span><span class="sxs-lookup"><span data-stu-id="63963-154">However, the attribute would be ignored and the method would be called with whatever default value was provided.</span></span>

```csharp
// Assembly1

void Foo(string bar); // V1
void Foo(string bar, string barExpression = "not provided"); // V2
void Foo(string bar, [CallerArgumentExpression("bar")] string barExpression = "not provided"); // V3

// Assembly2

Foo(a); // V1: Compiles to Foo(a), V2, V3: Compiles to Foo(a, "not provided")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")

// Assembly3

[assembly: EnableCallerArgumentExpression]

Foo(a); // V1: Compiles to Foo(a), V2: Compiles to Foo(a, "not provided"), V3: Compiles to Foo(a, "a")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")
```

- <span data-ttu-id="63963-155">若要防止每次将新的调用方信息添加到 `Debug.Assert`时出现[二进制兼容性问题][ drawbacks] ，另一种解决方案是将 `CallerInfo` 结构添加到包含有关调用方的所有必要信息的框架中。</span><span class="sxs-lookup"><span data-stu-id="63963-155">To prevent the [binary compatibility problem][drawbacks] from occurring every time we want to add new caller info to `Debug.Assert`, an alternative solution would be to add a `CallerInfo` struct to the framework that contains all the necessary information about the caller.</span></span>

```csharp
struct CallerInfo
{
    public string MemberName { get; set; }
    public string TypeName { get; set; }
    public string Namespace { get; set; }
    public string FullTypeName { get; set; }
    public string FilePath { get; set; }
    public int LineNumber { get; set; }
    public int ColumnNumber { get; set; }
    public Type Type { get; set; }
    public MethodBase Method { get; set; }
    public string[] ArgumentExpressions { get; set; }
}

[Flags]
enum CallerInfoOptions
{
    MemberName = 1, TypeName = 2, ...
}

public static class Debug
{
    public static void Assert(bool condition,
        // If a flag is not set here, the corresponding CallerInfo member is not populated by the caller, so it's
        // pay-for-play friendly.
        [CallerInfo(CallerInfoOptions.FilePath | CallerInfoOptions.Method | CallerInfoOptions.ArgumentExpressions)] CallerInfo callerInfo = default(CallerInfo))
    {
        string filePath = callerInfo.FilePath;
        MethodBase method = callerInfo.Method;
        string conditionExpression = callerInfo.ArgumentExpressions[0];
        ...
    }
}

class Bar
{
    void Foo()
    {
        Debug.Assert(false);

        // Translates to:

        var callerInfo = new CallerInfo();
        callerInfo.FilePath = @"C:\Bar.cs";
        callerInfo.Method = MethodBase.GetCurrentMethod();
        callerInfo.ArgumentExpressions = new string[] { "false" };
        Debug.Assert(false, callerInfo);
    }
}
```

<span data-ttu-id="63963-156">最初建议在 https://github.com/dotnet/csharplang/issues/87上进行此操作。</span><span class="sxs-lookup"><span data-stu-id="63963-156">This was originally proposed at https://github.com/dotnet/csharplang/issues/87.</span></span>

<span data-ttu-id="63963-157">此方法有几个缺点：</span><span class="sxs-lookup"><span data-stu-id="63963-157">There are a few disadvantages of this approach:</span></span>

- <span data-ttu-id="63963-158">尽管通过允许你指定所需的属性，但仍可以通过为表达式/调用 `MethodBase.GetCurrentMethod` 分配数组来大幅损害性能，即使在断言通过时也是如此。</span><span class="sxs-lookup"><span data-stu-id="63963-158">Despite being pay-for-play friendly by allowing you to specify which properties you need, it could still hurt perf significantly by allocating an array for the expressions/calling `MethodBase.GetCurrentMethod` even when the assert passes.</span></span>

- <span data-ttu-id="63963-159">此外，将新标志传递到 `CallerInfo` 特性不会是一项重大更改，`Debug.Assert` 不能保证实际接收来自针对旧版本方法编译的调用站点的新参数。</span><span class="sxs-lookup"><span data-stu-id="63963-159">Additionally, while passing a new flag to the `CallerInfo` attribute won't be a breaking change, `Debug.Assert` won't be guaranteed to actually receive that new parameter from call sites that compiled against an old version of the method.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="63963-160">未解决的问题</span><span class="sxs-lookup"><span data-stu-id="63963-160">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="63963-161">TBD</span><span class="sxs-lookup"><span data-stu-id="63963-161">TBD</span></span>

## <a name="design-meetings"></a><span data-ttu-id="63963-162">设计会议</span><span class="sxs-lookup"><span data-stu-id="63963-162">Design meetings</span></span>

<span data-ttu-id="63963-163">空值</span><span class="sxs-lookup"><span data-stu-id="63963-163">N/A</span></span>
