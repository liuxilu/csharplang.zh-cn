---
ms.openlocfilehash: 7522bee6ac14d205aaf2a8491c13a321d2c18d62
ms.sourcegitcommit: c30039481ee8a75c3b3e4ddd369fdf8f84f8945b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82598565"
---
# <a name="module-initializers"></a><span data-ttu-id="10a27-101">模块初始值设定项</span><span class="sxs-lookup"><span data-stu-id="10a27-101">Module Initializers</span></span>

* <span data-ttu-id="10a27-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="10a27-102">[x] Proposed</span></span>
* <span data-ttu-id="10a27-103">[] 原型：[正在进行](https://github.com/jnm2/roslyn/tree/module_initializer)</span><span class="sxs-lookup"><span data-stu-id="10a27-103">[ ] Prototype: [In Progress](https://github.com/jnm2/roslyn/tree/module_initializer)</span></span>
* <span data-ttu-id="10a27-104">[] 实现：[正在进行](https://github.com/dotnet/roslyn/tree/features/module-initializers)</span><span class="sxs-lookup"><span data-stu-id="10a27-104">[ ] Implementation: [In Progress](https://github.com/dotnet/roslyn/tree/features/module-initializers)</span></span>
* <span data-ttu-id="10a27-105">[] 规范：[未启动]()</span><span class="sxs-lookup"><span data-stu-id="10a27-105">[ ] Specification: [Not Started]()</span></span>

## <a name="summary"></a><span data-ttu-id="10a27-106">总结</span><span class="sxs-lookup"><span data-stu-id="10a27-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="10a27-107">尽管 .NET 平台具有直接支持编写程序集（从技术上讲为模块）的初始化代码的功能，但它不是用 c # 公开的。</span><span class="sxs-lookup"><span data-stu-id="10a27-107">Although the .NET platform has a feature that directly supports writing initialization code for the assembly (technically, the module), it is not exposed in C#.</span></span>  <span data-ttu-id="10a27-108">这是一个相当小的方案，但一旦进入该方案，解决方案就显得非常令人头痛。</span><span class="sxs-lookup"><span data-stu-id="10a27-108">This is a rather niche scenario, but once you run into it the solutions appear to be pretty painful.</span></span>  <span data-ttu-id="10a27-109">有[大量客户](https://www.google.com/search?q=.net+module+constructor+c%23&oq=.net+module+constructor)（在 Microsoft 内部和外部）正在解决问题，并且没有任何不清楚记录的情况。</span><span class="sxs-lookup"><span data-stu-id="10a27-109">There are reports of [a number of customers](https://www.google.com/search?q=.net+module+constructor+c%23&oq=.net+module+constructor) (inside and outside Microsoft) struggling with the problem, and there are no doubt more undocumented cases.</span></span>

## <a name="motivation"></a><span data-ttu-id="10a27-110">动机</span><span class="sxs-lookup"><span data-stu-id="10a27-110">Motivation</span></span>
[motivation]: #motivation

- <span data-ttu-id="10a27-111">允许库在加载时执行预先的一次性初始化，开销最小，用户无需显式调用任何内容</span><span class="sxs-lookup"><span data-stu-id="10a27-111">Enable libraries to do eager, one-time initialization when loaded, with minimal overhead and without the user needing to explicitly call anything</span></span>
- <span data-ttu-id="10a27-112">当前`static`构造函数方法的一个特别难点是，运行时必须对使用静态构造函数的类型使用额外的检查，以确定是否需要运行静态构造函数。</span><span class="sxs-lookup"><span data-stu-id="10a27-112">One particular pain point of current `static` constructor approaches is that the runtime must do additional checks on usage of a type with a static constructor, in order to decide whether the static constructor needs to be run or not.</span></span> <span data-ttu-id="10a27-113">这会增加可度量的开销。</span><span class="sxs-lookup"><span data-stu-id="10a27-113">This adds measurable overhead.</span></span>
- <span data-ttu-id="10a27-114">使源生成器无需显式调用任何内容即可运行一些全局初始化逻辑</span><span class="sxs-lookup"><span data-stu-id="10a27-114">Enable source generators to run some global initialization logic without the user needing to explicitly call anything</span></span>

## <a name="detailed-design"></a><span data-ttu-id="10a27-115">详细设计</span><span class="sxs-lookup"><span data-stu-id="10a27-115">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="10a27-116">C # 编译器将识别以下特性：</span><span class="sxs-lookup"><span data-stu-id="10a27-116">The C# compiler will recognize the following attribute:</span></span>

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

<span data-ttu-id="10a27-117">使用此类</span><span class="sxs-lookup"><span data-stu-id="10a27-117">You would use it like this</span></span>

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

<span data-ttu-id="10a27-118">然后，c # 编译器将发出一个模块构造函数，该构造函数导致触发标识的类型的静态构造函数：</span><span class="sxs-lookup"><span data-stu-id="10a27-118">and the C# compiler would then emit a module constructor that causes the static constructor of the identified type to be triggered:</span></span>

``` c#
void .cctor()
{
    // synthesize and call a dummy method with an unspeakable name,
    // which will cause the runtime to call the static constructor
    MyModuleInitializer.<TriggerClassConstructor>();
}
```

<span data-ttu-id="10a27-119">请注意，检查静态构造函数是否已在模块初始值设定项类型（`MyModuleInitializer`在此示例中）上运行的开销是否可以通过只引用`[module: ModuleInitializer(typeof(MyModuleInitializer))]`特性中的类型来保持最小。</span><span class="sxs-lookup"><span data-stu-id="10a27-119">Note that the overhead of checking whether the static constructor has run on a module initializer type (`MyModuleInitializer` in this example) can be kept to a minimum by referencing the type only in the `[module: ModuleInitializer(typeof(MyModuleInitializer))]` attribute.</span></span>

<span data-ttu-id="10a27-120">此建议使用`AttributeUsage(AllowMultiple = false)`禁止用户声明多个初始值设定项类，而无需添加任何特殊语言规则即可实现此目的。</span><span class="sxs-lookup"><span data-stu-id="10a27-120">This proposal uses `AttributeUsage(AllowMultiple = false)` to disallow the user from declaring more than one initializer class without adding any special language rules to accomplish that.</span></span> <span data-ttu-id="10a27-121">它非常轻量，因为它使用了属性的现有语法和语义规则。</span><span class="sxs-lookup"><span data-stu-id="10a27-121">It is very lightweight in that it uses existing syntax and semantic rules for attributes.</span></span> <span data-ttu-id="10a27-122">另一方面，这可能会使用户更难认识到它们所查看的静态构造函数是模块初始值设定项，因为属性可能远离实际包含模块初始值设定项的类，并且还需要我们决定当特性用于引用另一个程序集中的类型时所发生的情况。</span><span class="sxs-lookup"><span data-stu-id="10a27-122">On the other hand, it may make it more difficult for users to realize that the static constructor they are looking at is the module initializer, because the attribute may be far away from the class that actually contains the module initializer, and it also requires us to decide what happens if the attribute is used to reference a type from another assembly.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="10a27-123">未解决的问题</span><span class="sxs-lookup"><span data-stu-id="10a27-123">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

#### <a name="should-we-permit-multiple-types-to-be-decorated-with-moduleinitializerattribute-in-a-compilation-if-so-in-what-order-should-the-static-constructors-be-invoked"></a><span data-ttu-id="10a27-124">在编译中，我们是否允许使用 ModuleInitializerAttribute 修饰多种类型？</span><span class="sxs-lookup"><span data-stu-id="10a27-124">Should we permit multiple types to be decorated with ModuleInitializerAttribute in a compilation?</span></span> <span data-ttu-id="10a27-125">如果是这样，则应以何种顺序调用静态构造函数？</span><span class="sxs-lookup"><span data-stu-id="10a27-125">If so, in what order should the static constructors be invoked?</span></span>

<span data-ttu-id="10a27-126">一个选项：为分部类中的静态初始值设定项执行相同的操作。</span><span class="sxs-lookup"><span data-stu-id="10a27-126">One option: do the same thing we do for static initializers in partial classes.</span></span> <span data-ttu-id="10a27-127">根据文件顺序 + 源位置对它们进行排序。</span><span class="sxs-lookup"><span data-stu-id="10a27-127">Order them based on file order+source position.</span></span>

#### <a name="should-we-permit-using-a-type-from-another-assembly-as-the-module-initializer"></a><span data-ttu-id="10a27-128">是否应该允许使用另一程序集中的类型作为模块初始值设定项？</span><span class="sxs-lookup"><span data-stu-id="10a27-128">Should we permit using a type from another assembly as the module initializer?</span></span>

<span data-ttu-id="10a27-129">192.168.0.2.`[module: ModuleInitializerAttribute(typeof(InitializerFromOtherAssembly))]`</span><span class="sxs-lookup"><span data-stu-id="10a27-129">e.g.  `[module: ModuleInitializerAttribute(typeof(InitializerFromOtherAssembly))]`</span></span>

<span data-ttu-id="10a27-130">这可能很难通过调用虚方法来调用类构造函数，因为我们无法对类型合成此类方法。</span><span class="sxs-lookup"><span data-stu-id="10a27-130">This could make it difficult to invoke the class constructor simply by calling a dummy method, because we won't be able to synthesize such a method on the type.</span></span> <span data-ttu-id="10a27-131">在这种情况下， `System.Runtime.CompilerServices.RuntimeHelpers.RunClassConstructor()`我们可以考虑回退到，但它引入了反射堆栈的依赖项。</span><span class="sxs-lookup"><span data-stu-id="10a27-131">We could consider falling back to `System.Runtime.CompilerServices.RuntimeHelpers.RunClassConstructor()` in this case, but it introduces a dependency on the reflection stack.</span></span>

<span data-ttu-id="10a27-132">此用例表示初始值设定项类不是声明它的程序集的模块初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="10a27-132">This use case implies that the initializer class is not the module initializer for the assembly it is declared in.</span></span> <span data-ttu-id="10a27-133">应改为使用此用例，而不是通过公开外部使用方应从其模块初始值设定项调用的方法来进行处理。</span><span class="sxs-lookup"><span data-stu-id="10a27-133">It feels like this use case should instead by handled by exposing a method that the external consumer should call from their module initializer.</span></span>

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

## <a name="drawbacks"></a><span data-ttu-id="10a27-134">缺点</span><span class="sxs-lookup"><span data-stu-id="10a27-134">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="10a27-135">为什么*不*应这样做？</span><span class="sxs-lookup"><span data-stu-id="10a27-135">Why should we *not* do this?</span></span>

- <span data-ttu-id="10a27-136">"注入" 模块初始值设定项的现有第三方工具或许足以满足要求使用此功能的用户。</span><span class="sxs-lookup"><span data-stu-id="10a27-136">Perhaps the existing third-party tooling for "injecting" module initializers is sufficient for users who have been asking for this feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="10a27-137">备选项</span><span class="sxs-lookup"><span data-stu-id="10a27-137">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="10a27-138">考虑了其他哪些设计？</span><span class="sxs-lookup"><span data-stu-id="10a27-138">What other designs have been considered?</span></span> <span data-ttu-id="10a27-139">不这样做会产生什么影响？</span><span class="sxs-lookup"><span data-stu-id="10a27-139">What is the impact of not doing this?</span></span>

<span data-ttu-id="10a27-140">公开此功能的一种可能的方法是使用语言：</span><span class="sxs-lookup"><span data-stu-id="10a27-140">There are a number of possible ways of exposing this feature in the language:</span></span>

### <a name="1-special-global-method-declaration"></a><span data-ttu-id="10a27-141">1. 特殊的全局方法声明</span><span class="sxs-lookup"><span data-stu-id="10a27-141">1. Special global method declaration</span></span>

<span data-ttu-id="10a27-142">模块初始值设定项是通过在全局范围内编写一种特殊类型的方法提供的：</span><span class="sxs-lookup"><span data-stu-id="10a27-142">A module initializer would be provided by writing a special kind of method in the global scope:</span></span>

```csharp
internal void operator init() ...
```

<span data-ttu-id="10a27-143">这将为新语言构造自己的语法。</span><span class="sxs-lookup"><span data-stu-id="10a27-143">This gives the new language construct its own syntax.</span></span> <span data-ttu-id="10a27-144">但是，考虑到方案的罕见程度和具体程度，这可能是一种方法。</span><span class="sxs-lookup"><span data-stu-id="10a27-144">However, given how rare and niche the scenario is, this is probably far too heavyweight an approach.</span></span>

### <a name="2-attribute-on-the-type-to-be-initialized"></a><span data-ttu-id="10a27-145">2. 要初始化的类型的属性</span><span class="sxs-lookup"><span data-stu-id="10a27-145">2. Attribute on the type to be initialized</span></span>

<span data-ttu-id="10a27-146">可能将属性放置在要初始化的类型上，而不是模块级特性</span><span class="sxs-lookup"><span data-stu-id="10a27-146">Instead of a module-level attribute, perhaps the attribute would be placed on the type to be initialized</span></span>

```csharp
[ModuleInitializer]
class ToInitialize
{
    static ToInitialize() ...
}
```

<span data-ttu-id="10a27-147">使用此方法时，需要拒绝包含此属性的多个应用程序的程序，或提供某些策略来定义顺序，以防多次使用此属性。</span><span class="sxs-lookup"><span data-stu-id="10a27-147">With this approach, we would either need to reject a program that contains more than one application of this attribute, or provide some policy to define the ordering in case it is used multiple times.</span></span> <span data-ttu-id="10a27-148">无论采用哪种方式，它都比上述原始方案更复杂。</span><span class="sxs-lookup"><span data-stu-id="10a27-148">Either way, it is more complex than the original proposal above.</span></span>

#### <a name="3-attribute-on-the-static-constructor-to-be-initialized"></a><span data-ttu-id="10a27-149">3. 要初始化的静态构造函数上的特性</span><span class="sxs-lookup"><span data-stu-id="10a27-149">3. Attribute on the static constructor to be initialized</span></span>

<span data-ttu-id="10a27-150">可能将属性放置在要初始化的静态构造函数上，而不是模块级特性</span><span class="sxs-lookup"><span data-stu-id="10a27-150">Instead of a module-level attribute, perhaps the attribute would be placed on the static constructor to be initialized</span></span>

```csharp
class ToInitialize
{
    [ModuleInitializer]
    static ToInitialize() ...
}
```

<span data-ttu-id="10a27-151">使用此方法时，需要拒绝包含此属性的多个应用程序的程序，或提供某些策略来定义顺序，以防多次使用此属性。</span><span class="sxs-lookup"><span data-stu-id="10a27-151">With this approach, we would either need to reject a program that contains more than one application of this attribute, or provide some policy to define the ordering in case it is used multiple times.</span></span> <span data-ttu-id="10a27-152">无论采用哪种方式，它都比上述原始方案更复杂。</span><span class="sxs-lookup"><span data-stu-id="10a27-152">Either way, it is more complex than the original proposal above.</span></span>

#### <a name="4-attribute-on-a-static-method-to-be-called"></a><span data-ttu-id="10a27-153">4. 要调用的静态方法上的特性</span><span class="sxs-lookup"><span data-stu-id="10a27-153">4. Attribute on a static method to be called</span></span>

<span data-ttu-id="10a27-154">可能将属性放置在要调用以执行初始化的方法上，而不是模块级特性。</span><span class="sxs-lookup"><span data-stu-id="10a27-154">Instead of a module-level attribute, perhaps the attribute would be placed on the method to be called to perform the initialization</span></span>

```csharp
class Any
{
    [ModuleInitializer]
    static void Initializer() ...
}
```

<span data-ttu-id="10a27-155">与前一种方法一样，需要拒绝包含此属性的多个应用程序的程序，或提供某些策略来定义顺序，以防多次使用此属性。</span><span class="sxs-lookup"><span data-stu-id="10a27-155">As in the previous approach, we would either need to reject a program that contains more than one application of this attribute, or provide some policy to define the ordering in case it is used multiple times.</span></span> <span data-ttu-id="10a27-156">无论采用哪种方式，它都比原始提议更复杂。</span><span class="sxs-lookup"><span data-stu-id="10a27-156">Either way, it is more complex than the original proposal.</span></span> <span data-ttu-id="10a27-157">如果具有参数或非`void`返回类型，则还可能需要使用此特性拒绝任何方法。</span><span class="sxs-lookup"><span data-stu-id="10a27-157">We also would probably need to reject any method with this attribute if it has parameters or a non-`void` return type.</span></span>

#### <a name="5-do-nothing"></a><span data-ttu-id="10a27-158">5. 不执行任何操作</span><span class="sxs-lookup"><span data-stu-id="10a27-158">5. Do nothing</span></span>
<span data-ttu-id="10a27-159">如果未实现此功能，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="10a27-159">If we do not implement this feature, then:</span></span>
- <span data-ttu-id="10a27-160">确实需要模块初始值设定项的用户继续依赖第三方工具将它们注入到程序集中。</span><span class="sxs-lookup"><span data-stu-id="10a27-160">Users who really need module initializers continue to rely on third-party tooling to inject them into their assemblies.</span></span>
- <span data-ttu-id="10a27-161">源生成器必须依赖于静态构造函数，该构造函数会增加用户必须显式调用的系统开销或 Init （）方法。</span><span class="sxs-lookup"><span data-stu-id="10a27-161">Source generators will have to rely on static constructors which add overhead or on Init() methods that users must explicitly call.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="10a27-162">设计会议</span><span class="sxs-lookup"><span data-stu-id="10a27-162">Design meetings</span></span>

### <a name="april-8th-2020"></a>[<span data-ttu-id="10a27-163">2020年4月8日</span><span class="sxs-lookup"><span data-stu-id="10a27-163">April 8th, 2020</span></span>](/meetings/2020/LDM-2020-04-08.md#module-initializers)
<span data-ttu-id="10a27-164">让我们将任何静态方法用作模块初始值设定项（上述方案中的选项4），并使用众所周知的特性标记该方法。</span><span class="sxs-lookup"><span data-stu-id="10a27-164">Let's let any static method be a module initializer (option 4 in the above proposal), and mark that method using a well-known attribute.</span></span> <span data-ttu-id="10a27-165">我们还将允许使用多个模块初始值设定项方法，并按保留但确定性顺序调用每个方法。</span><span class="sxs-lookup"><span data-stu-id="10a27-165">We'll also allow multiple module initializer methods, and they will each be called in a reserved, but deterministic order.</span></span>
