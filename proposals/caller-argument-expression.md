---
ms.openlocfilehash: fa3326bf69c83b6042b1db7b5567fd5c28d6f81a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483516"
---
# <a name="callerargumentexpression"></a>CallerArgumentExpression

* [x] 建议
* [] 原型：未启动
* [] 实现：未启动
* [] 规范：未启动

## <a name="summary"></a>总结
[summary]: #summary

允许开发人员捕获传递给方法的表达式，在诊断/测试 Api 中启用更好的错误消息并减少击键次数。

## <a name="motivation"></a>动机
[motivation]: #motivation

当断言或参数验证失败时，开发人员希望尽可能多地了解其失败的位置和原因。 但今天的诊断 Api 并不能完全简化这一点。 请考虑以下方法：

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null);
    Debug.Assert(array.Length == 1);

    return array[0];
}
```

当其中一个断言失败时，堆栈跟踪中只会提供文件名、行号和方法名。 开发人员无法判断哪些断言未能通过此信息--（s）他必须打开文件并导航到提供的行号来查看发生了什么问题。

这也是测试框架必须提供各种断言方法的原因。 使用 xUnit 时，不经常使用 `Assert.True` 和 `Assert.False`，因为它们不能提供有关失败原因的足够上下文。

虽然对于参数验证而言，这种情况更好一些，因为开发人员向开发人员显示无效参数的名称，开发人员必须手动将这些名称传递给异常。 如果重写上面的示例以使用传统参数验证而不是 `Debug.Assert`，它将如下所示

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

请注意，必须将 `nameof(array)` 传递给每个异常，尽管它在自变量无效的上下文中已经明确了。

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

在上面的示例中，包含断言消息中的字符串 `"array != null"` 或 `"array.Length == 1"` 有助于开发人员确定失败的内容。 输入 `CallerArgumentExpression`：此属性是框架可用于获取与特定方法参数关联的字符串的属性。 我们会将它添加到 `Debug.Assert`，如

```csharp
public static class Debug
{
    public static void Assert(bool condition, [CallerArgumentExpression("condition")] string message = null);
}
```

以上示例中的源代码将保持不变。 但编译器实际发出的代码将对应于

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null, "array != null");
    Debug.Assert(array.Length == 1, "array.Length == 1");

    return array[0];
}
```

编译器专门识别 `Debug.Assert`上的属性。 它在调用站点传递与属性的构造函数中引用的参数（在本例中为 `condition`）关联的字符串。 当任何一个断言失败时，开发人员将显示 "假" 这一条件，并知道哪一项失败。

对于参数验证，特性不能直接使用，但可以通过帮助器类来使用：

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

将此类帮助器类添加到框架的建议在 https://github.com/dotnet/corefx/issues/17068进行。 如果实现了此语言功能，则可以更新该建议以利用此功能。

### <a name="extension-methods"></a>扩展方法

扩展方法中的 `this` 参数可能由 `CallerArgumentExpression`引用。 例如：

```csharp
public static void ShouldBe<T>(this T @this, T expected, [CallerArgumentExpression("this")] string thisExpression = null) {}

contestant.Points.ShouldBe(1337); // thisExpression: "contestant.Points"
```

`thisExpression` 将接收与点前面的对象相对应的表达式。 如果它是通过静态方法语法（如 `Ext.ShouldBe(contestant.Points, 1337)`）调用的，则其行为就像第一个参数未标记为 `this`。

应始终存在与 `this` 参数对应的表达式。 即使类的实例自行调用扩展方法（例如从集合类型内 `this.Single()`），`this` 也由编译器强制，因此 `"this"` 会通过。 如果将来更改此规则，可以考虑传递 `null` 或空字符串。

### <a name="extra-details"></a>额外详细信息

- 与其他 `Caller*` 属性（如 `CallerMemberName`）一样，此属性只能用于具有默认值的参数。
- 允许使用 `CallerArgumentExpression` 标记的多个参数，如上所示。
- 将 `System.Runtime.CompilerServices`特性的命名空间。
- 如果提供 `null` 或非参数名称的字符串（如 `"notAParameterName"`），则编译器将传入一个空字符串。

## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

- 知道如何使用 decompilers 的人员能够在调用站点上查看使用此特性标记的方法的一些源代码。 对于关闭的源软件，这可能是不必要的/意外的。

- 尽管这不是功能本身中的一个缺陷，但仍存在一个问题来源，因为目前只有一个 `bool`的 `Debug.Assert` API。 即使采用消息的重载的第二个参数使用此属性进行标记并使其成为可选参数，编译器仍会在重载决策中选取非消息一。 因此，必须删除非消息重载才能利用此功能，这将是二进制（尽管不是源）的重大更改。

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

- 如果可以在调用站点上查看使用此属性证明有问题的方法的源代码，则可以选择启用该属性的效果。 开发人员将通过其置于 `AssemblyInfo.cs`中的程序集范围 `[assembly: EnableCallerArgumentExpression]` 特性来启用它。
  - 如果未启用该特性的效果，则调用用特性标记的方法将不会导致错误，从而使现有方法能够使用特性并维护源兼容性。 但是，将忽略属性，并将通过提供的任何默认值调用方法。

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

- 若要防止每次将新的调用方信息添加到 `Debug.Assert`时出现[二进制兼容性问题][ drawbacks] ，另一种解决方案是将 `CallerInfo` 结构添加到包含有关调用方的所有必要信息的框架中。

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

最初建议在 https://github.com/dotnet/csharplang/issues/87上进行此操作。

此方法有几个缺点：

- 尽管通过允许你指定所需的属性，但仍可以通过为表达式/调用 `MethodBase.GetCurrentMethod` 分配数组来大幅损害性能，即使在断言通过时也是如此。

- 此外，将新标志传递到 `CallerInfo` 特性不会是一项重大更改，`Debug.Assert` 不能保证实际接收来自针对旧版本方法编译的调用站点的新参数。

## <a name="unresolved-questions"></a>未解决的问题
[unresolved]: #unresolved-questions

TBD

## <a name="design-meetings"></a>设计会议

空值
