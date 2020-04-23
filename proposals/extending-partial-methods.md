---
ms.openlocfilehash: 04dca01bad04d5c53aa1c7c876343fb7ef33d2fa
ms.sourcegitcommit: 5c7cc619214ade6a8f3a0caddfb4862635f5241d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/22/2020
ms.locfileid: "82021914"
---
<a name="extending-partial-methods"></a><span data-ttu-id="9cfa8-101">扩展部分方法</span><span class="sxs-lookup"><span data-stu-id="9cfa8-101">Extending Partial Methods</span></span>
=====

## <a name="summary"></a><span data-ttu-id="9cfa8-102">总结</span><span class="sxs-lookup"><span data-stu-id="9cfa8-102">Summary</span></span>
<span data-ttu-id="9cfa8-103">此建议旨在删除有关 C#`partial`中方法签名的所有限制。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-103">This proposal aims to remove all restrictions around the signatures of `partial` methods in C#.</span></span> <span data-ttu-id="9cfa8-104">目标是扩展这些方法可以使用源生成器的方案集，以及 C# 方法的更通用声明形式。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-104">The goal being to expand the set of scenarios in which these methods can work with source generators as well as being a more general declaration form for C# methods.</span></span>

## <a name="motivation"></a><span data-ttu-id="9cfa8-105">动机</span><span class="sxs-lookup"><span data-stu-id="9cfa8-105">Motivation</span></span>
<span data-ttu-id="9cfa8-106">C# 对开发人员将方法拆分为声明和定义/实现的支持有限。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-106">C# has limited support for developers splitting methods into declarations and definitions / implementations.</span></span> 

```cs 
partial class C
{
    // The declaration of C.M
    partial void M(string message);
}

partial class C
{
    // The definition of C.M
    partial void M(string message) => Console.WriteLine(message);
}
```

<span data-ttu-id="9cfa8-107">方法的一`partial`个行为是，当定义不存在时，语言将简单地擦除对`partial`方法的任何调用。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-107">One behavior of `partial` methods is that when the definition is absent then the language will simply erase any calls to the `partial` method.</span></span> <span data-ttu-id="9cfa8-108">从本质上讲，它就像对将`[Conditional]`条件计算为 false 的方法的调用。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-108">Essentially it behaves like a call to a `[Conditional]` method where the condition was evaluated to false.</span></span> 

```cs
partial class D
{
    partial void M(string message);

    void Example()
    {
        M(GetIt()); // Call to M and GetIt erased at compile time
    }

    string GetIt() => "Hello World";
}
```

<span data-ttu-id="9cfa8-109">此功能的最初动机是以设计器生成的代码的形式生成源代码。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-109">The original motivation for this feature was source generation in the form of designer generated code.</span></span> <span data-ttu-id="9cfa8-110">用户不断编辑生成的代码，因为他们希望挂钩生成的代码的某些方面。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-110">Users were constantly editing the generated code because they wanted to hook some aspect of the generated code.</span></span> <span data-ttu-id="9cfa8-111">最值得注意的是 Windows 窗体启动过程的一部分，在组件初始化后。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-111">Most notably parts of the Windows Forms startup process, after components were initialized.</span></span>

<span data-ttu-id="9cfa8-112">编辑生成的代码容易出错，因为导致设计器重新生成代码的任何操作都将导致用户编辑被擦除。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-112">Editing the generated code was error prone because any action which caused the designer to regenerate the code would cause the user edit to be erased.</span></span> <span data-ttu-id="9cfa8-113">该方法`partial`功能缓解了这种紧张，因为它允许设计人员以方法的形式`partial`发出挂钩。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-113">The `partial` method feature eased this tension because it allowed designers to emit hooks in the form of `partial` methods.</span></span> 

<span data-ttu-id="9cfa8-114">设计人员可以发出这样的`partial void OnComponentInit()`钩子，开发人员可以定义它们的声明，或者不定义它们。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-114">Designers could emit hooks like `partial void OnComponentInit()` and developers could define declarations for them or not define them.</span></span> <span data-ttu-id="9cfa8-115">在这两种情况下，尽管生成的代码将编译，对该过程感兴趣的开发人员可以根据需要挂接。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-115">In either case though the generated code would compile and developers who were interested in the process could hook in as needed.</span></span> 

<span data-ttu-id="9cfa8-116">这确实意味着部分方法具有以下几个限制：</span><span class="sxs-lookup"><span data-stu-id="9cfa8-116">This does mean that partial methods have several restrictions:</span></span>

1. <span data-ttu-id="9cfa8-117">必须具有`void`返回类型。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-117">Must have a `void` return type.</span></span>
1. <span data-ttu-id="9cfa8-118">不能有`ref``out`或 参数。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-118">Cannot have `ref` or `out` parameters.</span></span> 
1. <span data-ttu-id="9cfa8-119">不能有任何可访问性（`private`隐式）。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-119">Cannot have any accessibility (implicitly `private`).</span></span>

<span data-ttu-id="9cfa8-120">存在这些限制是因为语言必须在删除调用站点时发出代码。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-120">These restrictions exist because the language must be able to emit code when the call site is erased.</span></span> <span data-ttu-id="9cfa8-121">给定它们可以擦除`private`是唯一可能的可访问性，因为成员无法在程序集元数据中公开。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-121">Given they can be erased `private` is the only possible accessibility because the member can't be exposed in assembly metadata.</span></span> <span data-ttu-id="9cfa8-122">这些限制还用于限制可以应用`partial`方法的方案集。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-122">These restrictions also serve to limit the set of scenarios in which `partial` methods can be applied.</span></span>

<span data-ttu-id="9cfa8-123">此处的建议是删除有关`partial`方法的所有现有限制。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-123">The proposal here is to remove all of the existing restrictions around `partial` methods.</span></span> <span data-ttu-id="9cfa8-124">本质上，让他们有`out`，非 void 返回类型或任何类型的可访问性。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-124">Essentially let them have `out`, non-void return types or any type of accessibility.</span></span> <span data-ttu-id="9cfa8-125">然后`partial`，此类声明将附加要求，即必须存在定义。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-125">Such `partial` declarations would then have the added requirement that a definition must exist.</span></span> <span data-ttu-id="9cfa8-126">这意味着语言不必考虑正在调用站点的已使用的影响。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-126">That means the language does not have to consider the impact of erasing the call sites.</span></span> 

<span data-ttu-id="9cfa8-127">这将扩展方法可以参与的生成器场景集，`partial`从而与我们的源生成器功能很好地链接。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-127">This would expand the set of generator scenarios that `partial` methods could participate in and hence link in nicely with our source generators feature.</span></span> <span data-ttu-id="9cfa8-128">例如，可以使用以下模式定义正则表达式：</span><span class="sxs-lookup"><span data-stu-id="9cfa8-128">For example a regex could be defined using the following pattern:</span></span>

```cs
[RegexGenerated("(dog|cat|fish)")]
partial bool IsPetMatch(string input);
```

<span data-ttu-id="9cfa8-129">这为开发人员提供了一种简单的声明性方法，可以选择生成器，同时为生成器提供一组非常简单的声明，以便查看源代码以驱动其生成的输出。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-129">This gives both the developer a simple declarative way of opting into generators as well as giving generators a very easy set of declarations to look through in the source code to drive their generated output.</span></span> 

<span data-ttu-id="9cfa8-130">与生成器连接以下代码段的难度进行比较。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-130">Compare that with the difficulty that a generator would have hooking up the following snippet of code.</span></span> 

```cs
var regex = new RegularExpression("(dog|cat|fish)");
if (regex.IsMatch(someInput))
{

}
```

<span data-ttu-id="9cfa8-131">由于编译器不允许生成器修改连接此模式的代码，对于生成器来说，这几乎是不可能的。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-131">Given that the compiler doesn't allow generators to modify code hooking up this pattern would be pretty much impossible for generators.</span></span> <span data-ttu-id="9cfa8-132">他们需要在`IsMatch`实现中采用反射，或者要求用户将其调用站点更改为新方法 - 重构 regex 以将字符串文本作为参数传递。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-132">They would need to resort to reflection in the `IsMatch` implementation, or asking users to change their call sites to a new method + refactor the regex to pass the string literal as an argument.</span></span> <span data-ttu-id="9cfa8-133">太乱了</span><span class="sxs-lookup"><span data-stu-id="9cfa8-133">It's pretty messy.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="9cfa8-134">详细设计</span><span class="sxs-lookup"><span data-stu-id="9cfa8-134">Detailed Design</span></span>
<span data-ttu-id="9cfa8-135">语言将更改，以允许`partial`使用显式辅助功能修改器对方法进行给下用。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-135">The language will change to allow `partial` methods to be annotated with an explicit accessibility modifier.</span></span> <span data-ttu-id="9cfa8-136">这意味着它们可以标记为`private`、`public`等...</span><span class="sxs-lookup"><span data-stu-id="9cfa8-136">This means they can be labeled as `private`, `public`, etc ...</span></span> 

<span data-ttu-id="9cfa8-137">当方法`partial`具有显式辅助功能修改器时，尽管语言将要求声明具有匹配的定义，即使辅助功能为`private`：</span><span class="sxs-lookup"><span data-stu-id="9cfa8-137">When a `partial` method has an explicit accessibility modifier though the language will require that the declaration has a matching definition even when the accessibility is `private`:</span></span>

```cs
partial class C
{
    // Okay because no definition is required here
    partial void M1();

    // Okay because M2 has a definition
    private partial void M2();

    // Error: partial method M3 must have a definition
    private partial void M3();
}

partial class C
{
    private partial void M2() { }
}
```

<span data-ttu-id="9cfa8-138">此外，该语言将删除对具有显式可访问性`partial`的方法上可以显示的内容的所有限制。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-138">Further the language will remove all restrictions on what can appear on a `partial` method which has an explicit accessibility.</span></span> <span data-ttu-id="9cfa8-139">此类声明可以包含非 void 返回类型，`ref`或`out`参数、`extern`修改器等...这些签名具有 C# 语言的完整表达性。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-139">Such declarations can contain non-void return types, `ref` or `out` parameters, `extern` modifier, etc ... These signatures will have the full expressivity of the C# language.</span></span>

```cs
partial class D
{
    // Okay
    internal partial bool TryParse(string s, out int i); 
}

partial class D
{
    internal partial bool TryParse(string s, out int i) { }
}
```

<span data-ttu-id="9cfa8-140">这显式允许`partial`方法参与`overrides`和`interface`实现：</span><span class="sxs-lookup"><span data-stu-id="9cfa8-140">This explicitly allows for `partial` methods to participate in `overrides` and `interface` implementations:</span></span>

```cs
interface IStudent
{
    string GetName();
}

partial class C : IStudent
{
    public virtual partial string GetName(); 
}

partial class C
{
    public virtual partial string GetName() => "Jarde";
}
```

<span data-ttu-id="9cfa8-141">当方法`partial`包含非法元素时，编译器将更改它发出的错误，基本上说：</span><span class="sxs-lookup"><span data-stu-id="9cfa8-141">The compiler will change the error it emits when a `partial` method contains an illegal element to essentially say:</span></span>

> <span data-ttu-id="9cfa8-142">不能用于`ref`缺乏显式`partial`可访问性的方法</span><span class="sxs-lookup"><span data-stu-id="9cfa8-142">Cannot use `ref` on a `partial` method that lacks explicit accessibility</span></span> 

<span data-ttu-id="9cfa8-143">这将有助于在使用此功能时将开发人员指向正确的方向。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-143">This will help point developers in the right direction when using this feature.</span></span>

<span data-ttu-id="9cfa8-144">限制：</span><span class="sxs-lookup"><span data-stu-id="9cfa8-144">Restrictions:</span></span>
- <span data-ttu-id="9cfa8-145">`partial`具有显式可访问性的声明必须具有定义</span><span class="sxs-lookup"><span data-stu-id="9cfa8-145">`partial` declarations with explicit accessibility must have a definition</span></span>
- <span data-ttu-id="9cfa8-146">`partial`声明和定义签名必须与所有方法和参数修改器匹配。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-146">`partial` declarations and definition signatures must match on all method and parameter modifiers.</span></span> <span data-ttu-id="9cfa8-147">唯一可以不同的方面是参数名称和属性列表（这不是新的，而是方法的现有`partial`要求）。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-147">The only aspects which can differ are parameter names and attribute lists (this is not new but rather an existing requirement of `partial` methods).</span></span>

## <a name="questions"></a><span data-ttu-id="9cfa8-148">问题</span><span class="sxs-lookup"><span data-stu-id="9cfa8-148">Questions</span></span>

### <a name="partial-on-all-members"></a><span data-ttu-id="9cfa8-149">部分对所有成员</span><span class="sxs-lookup"><span data-stu-id="9cfa8-149">partial on all members</span></span>
<span data-ttu-id="9cfa8-150">鉴于我们正在扩展`partial`，以更友好的来源生成器，我们也应该扩展它工作的所有类成员？</span><span class="sxs-lookup"><span data-stu-id="9cfa8-150">Given that we're expanding `partial` to be more friendly to source generators should we also expand it to work on all class members?</span></span> <span data-ttu-id="9cfa8-151">例如，我们应该能够声明`partial`构造函数、运算符等...</span><span class="sxs-lookup"><span data-stu-id="9cfa8-151">For example should we be able to declare `partial` constructors, operators, etc ...</span></span>

<span data-ttu-id="9cfa8-152">**分辨率**这个想法是健全的，但在C#9时间表的这个点，我们试图避免不必要的功能蠕变。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-152">**Resolution** The idea is sound but at this point in the C# 9 schedule we're trying to avoid unnecessary feature creep.</span></span> <span data-ttu-id="9cfa8-153">要解决扩展功能以使用现代源生成器的紧迫问题。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-153">Want to solve the immediate problem of expanding the feature to work with modern source generators.</span></span> 

<span data-ttu-id="9cfa8-154">扩展`partial`以支持其他成员将考虑为 C# 10 版本。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-154">Extending `partial` to support other members will be considered for the C# 10 release.</span></span> <span data-ttu-id="9cfa8-155">看来我们会考虑这个扩展。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-155">Seems likely that we will consider this extension.</span></span>

### <a name="use-abstract-instead-of-partial"></a><span data-ttu-id="9cfa8-156">使用抽象而不是部分</span><span class="sxs-lookup"><span data-stu-id="9cfa8-156">Use abstract instead of partial</span></span>
<span data-ttu-id="9cfa8-157">这项建议的症结在于确保宣言具有相应的定义/执行。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-157">The crux of this proposal is essentially ensuring that a declaration has a corresponding definition / implementation.</span></span> <span data-ttu-id="9cfa8-158">鉴于我们应使用`abstract`，因为它已经是一个语言关键字，迫使开发人员考虑有一个实现？</span><span class="sxs-lookup"><span data-stu-id="9cfa8-158">Given that should we use `abstract` since it's already a language keyword that forces the developer to think about having an implementation?</span></span>

<span data-ttu-id="9cfa8-159">**分辨率**有一个健康的讨论，但最终决定反对。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-159">**Resolution** There was a healthy discussion about this but eventually it was decided against.</span></span>
<span data-ttu-id="9cfa8-160">是的，要求是熟悉的，但概念却大相径庭。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-160">Yes the requirements are familiar but the concepts are significantly different.</span></span>
<span data-ttu-id="9cfa8-161">很容易让开发人员相信他们在创建虚拟插槽时，他们没有这样做。</span><span class="sxs-lookup"><span data-stu-id="9cfa8-161">Could easily lead the developer to believe they were creating virtual slots when they were not doing so.</span></span>
