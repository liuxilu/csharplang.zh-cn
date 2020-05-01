---
ms.openlocfilehash: 7522bee6ac14d205aaf2a8491c13a321d2c18d62
ms.sourcegitcommit: c30039481ee8a75c3b3e4ddd369fdf8f84f8945b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82598565"
---
# <a name="module-initializers"></a>模块初始值设定项

* [x] 建议
* [] 原型：[正在进行](https://github.com/jnm2/roslyn/tree/module_initializer)
* [] 实现：[正在进行](https://github.com/dotnet/roslyn/tree/features/module-initializers)
* [] 规范：[未启动]()

## <a name="summary"></a>总结
[summary]: #summary

尽管 .NET 平台具有直接支持编写程序集（从技术上讲为模块）的初始化代码的功能，但它不是用 c # 公开的。  这是一个相当小的方案，但一旦进入该方案，解决方案就显得非常令人头痛。  有[大量客户](https://www.google.com/search?q=.net+module+constructor+c%23&oq=.net+module+constructor)（在 Microsoft 内部和外部）正在解决问题，并且没有任何不清楚记录的情况。

## <a name="motivation"></a>动机
[motivation]: #motivation

- 允许库在加载时执行预先的一次性初始化，开销最小，用户无需显式调用任何内容
- 当前`static`构造函数方法的一个特别难点是，运行时必须对使用静态构造函数的类型使用额外的检查，以确定是否需要运行静态构造函数。 这会增加可度量的开销。
- 使源生成器无需显式调用任何内容即可运行一些全局初始化逻辑

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

C # 编译器将识别以下特性：

``` c#
namespace System.Runtime.CompilerServices
{
    // Note: an Obsolete attribute is not needed here,
    // because only C# 9 compilers will have access to attributes added in .NET 5.
    // LangVersion checks will still be necessary.
    [AttributeUsage(AttributeTargets.Module, AllowMultiple = false)]
    public class ModuleInitializerAttribute : Attribute
    {
        public ModuleInitializerAttribute(Type type) { }
    }
}
```

使用此类

``` c#
using System.Runtime.CompilerServices;

[module: ModuleInitializer(typeof(MyModuleInitializer))]

internal static class MyModuleInitializer
{
    static MyModuleInitializer()
    {
        // put your module initializer here
    }
}
```

然后，c # 编译器将发出一个模块构造函数，该构造函数导致触发标识的类型的静态构造函数：

``` c#
void .cctor()
{
    // synthesize and call a dummy method with an unspeakable name,
    // which will cause the runtime to call the static constructor
    MyModuleInitializer.<TriggerClassConstructor>();
}
```

请注意，检查静态构造函数是否已在模块初始值设定项类型（`MyModuleInitializer`在此示例中）上运行的开销是否可以通过只引用`[module: ModuleInitializer(typeof(MyModuleInitializer))]`特性中的类型来保持最小。

此建议使用`AttributeUsage(AllowMultiple = false)`禁止用户声明多个初始值设定项类，而无需添加任何特殊语言规则即可实现此目的。 它非常轻量，因为它使用了属性的现有语法和语义规则。 另一方面，这可能会使用户更难认识到它们所查看的静态构造函数是模块初始值设定项，因为属性可能远离实际包含模块初始值设定项的类，并且还需要我们决定当特性用于引用另一个程序集中的类型时所发生的情况。

## <a name="unresolved-questions"></a>未解决的问题
[unresolved]: #unresolved-questions

#### <a name="should-we-permit-multiple-types-to-be-decorated-with-moduleinitializerattribute-in-a-compilation-if-so-in-what-order-should-the-static-constructors-be-invoked"></a>在编译中，我们是否允许使用 ModuleInitializerAttribute 修饰多种类型？ 如果是这样，则应以何种顺序调用静态构造函数？

一个选项：为分部类中的静态初始值设定项执行相同的操作。 根据文件顺序 + 源位置对它们进行排序。

#### <a name="should-we-permit-using-a-type-from-another-assembly-as-the-module-initializer"></a>是否应该允许使用另一程序集中的类型作为模块初始值设定项？

192.168.0.2.`[module: ModuleInitializerAttribute(typeof(InitializerFromOtherAssembly))]`

这可能很难通过调用虚方法来调用类构造函数，因为我们无法对类型合成此类方法。 在这种情况下， `System.Runtime.CompilerServices.RuntimeHelpers.RunClassConstructor()`我们可以考虑回退到，但它引入了反射堆栈的依赖项。

此用例表示初始值设定项类不是声明它的程序集的模块初始值设定项。 应改为使用此用例，而不是通过公开外部使用方应从其模块初始值设定项调用的方法来进行处理。

```cs
// Assembly 1
public class MyLibInit
{
    public static void Init() { }
}

// Assembly 2
using System.Runtime.CompilerServices;

[module: ModuleInitializer(typeof(MyInit))]

class MyInit
{
    static
    {
        MyLibInit.Init();
    }
}
```

## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

为什么*不*应这样做？

- "注入" 模块初始值设定项的现有第三方工具或许足以满足要求使用此功能的用户。

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

考虑了其他哪些设计？ 不这样做会产生什么影响？

公开此功能的一种可能的方法是使用语言：

### <a name="1-special-global-method-declaration"></a>1. 特殊的全局方法声明

模块初始值设定项是通过在全局范围内编写一种特殊类型的方法提供的：

```csharp
internal void operator init() ...
```

这将为新语言构造自己的语法。 但是，考虑到方案的罕见程度和具体程度，这可能是一种方法。

### <a name="2-attribute-on-the-type-to-be-initialized"></a>2. 要初始化的类型的属性

可能将属性放置在要初始化的类型上，而不是模块级特性

```csharp
[ModuleInitializer]
class ToInitialize
{
    static ToInitialize() ...
}
```

使用此方法时，需要拒绝包含此属性的多个应用程序的程序，或提供某些策略来定义顺序，以防多次使用此属性。 无论采用哪种方式，它都比上述原始方案更复杂。

#### <a name="3-attribute-on-the-static-constructor-to-be-initialized"></a>3. 要初始化的静态构造函数上的特性

可能将属性放置在要初始化的静态构造函数上，而不是模块级特性

```csharp
class ToInitialize
{
    [ModuleInitializer]
    static ToInitialize() ...
}
```

使用此方法时，需要拒绝包含此属性的多个应用程序的程序，或提供某些策略来定义顺序，以防多次使用此属性。 无论采用哪种方式，它都比上述原始方案更复杂。

#### <a name="4-attribute-on-a-static-method-to-be-called"></a>4. 要调用的静态方法上的特性

可能将属性放置在要调用以执行初始化的方法上，而不是模块级特性。

```csharp
class Any
{
    [ModuleInitializer]
    static void Initializer() ...
}
```

与前一种方法一样，需要拒绝包含此属性的多个应用程序的程序，或提供某些策略来定义顺序，以防多次使用此属性。 无论采用哪种方式，它都比原始提议更复杂。 如果具有参数或非`void`返回类型，则还可能需要使用此特性拒绝任何方法。

#### <a name="5-do-nothing"></a>5. 不执行任何操作
如果未实现此功能，请执行以下操作：
- 确实需要模块初始值设定项的用户继续依赖第三方工具将它们注入到程序集中。
- 源生成器必须依赖于静态构造函数，该构造函数会增加用户必须显式调用的系统开销或 Init （）方法。

## <a name="design-meetings"></a>设计会议

### <a name="april-8th-2020"></a>[2020年4月8日](/meetings/2020/LDM-2020-04-08.md#module-initializers)
让我们将任何静态方法用作模块初始值设定项（上述方案中的选项4），并使用众所周知的特性标记该方法。 我们还将允许使用多个模块初始值设定项方法，并按保留但确定性顺序调用每个方法。
