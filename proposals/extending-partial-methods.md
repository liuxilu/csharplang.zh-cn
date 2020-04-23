---
ms.openlocfilehash: 04dca01bad04d5c53aa1c7c876343fb7ef33d2fa
ms.sourcegitcommit: 5c7cc619214ade6a8f3a0caddfb4862635f5241d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/22/2020
ms.locfileid: "82021914"
---
<a name="extending-partial-methods"></a>扩展部分方法
=====

## <a name="summary"></a>总结
此建议旨在删除有关 C#`partial`中方法签名的所有限制。 目标是扩展这些方法可以使用源生成器的方案集，以及 C# 方法的更通用声明形式。

## <a name="motivation"></a>动机
C# 对开发人员将方法拆分为声明和定义/实现的支持有限。 

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

方法的一`partial`个行为是，当定义不存在时，语言将简单地擦除对`partial`方法的任何调用。 从本质上讲，它就像对将`[Conditional]`条件计算为 false 的方法的调用。 

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

此功能的最初动机是以设计器生成的代码的形式生成源代码。 用户不断编辑生成的代码，因为他们希望挂钩生成的代码的某些方面。 最值得注意的是 Windows 窗体启动过程的一部分，在组件初始化后。

编辑生成的代码容易出错，因为导致设计器重新生成代码的任何操作都将导致用户编辑被擦除。 该方法`partial`功能缓解了这种紧张，因为它允许设计人员以方法的形式`partial`发出挂钩。 

设计人员可以发出这样的`partial void OnComponentInit()`钩子，开发人员可以定义它们的声明，或者不定义它们。 在这两种情况下，尽管生成的代码将编译，对该过程感兴趣的开发人员可以根据需要挂接。 

这确实意味着部分方法具有以下几个限制：

1. 必须具有`void`返回类型。
1. 不能有`ref``out`或 参数。 
1. 不能有任何可访问性（`private`隐式）。

存在这些限制是因为语言必须在删除调用站点时发出代码。 给定它们可以擦除`private`是唯一可能的可访问性，因为成员无法在程序集元数据中公开。 这些限制还用于限制可以应用`partial`方法的方案集。

此处的建议是删除有关`partial`方法的所有现有限制。 本质上，让他们有`out`，非 void 返回类型或任何类型的可访问性。 然后`partial`，此类声明将附加要求，即必须存在定义。 这意味着语言不必考虑正在调用站点的已使用的影响。 

这将扩展方法可以参与的生成器场景集，`partial`从而与我们的源生成器功能很好地链接。 例如，可以使用以下模式定义正则表达式：

```cs
[RegexGenerated("(dog|cat|fish)")]
partial bool IsPetMatch(string input);
```

这为开发人员提供了一种简单的声明性方法，可以选择生成器，同时为生成器提供一组非常简单的声明，以便查看源代码以驱动其生成的输出。 

与生成器连接以下代码段的难度进行比较。 

```cs
var regex = new RegularExpression("(dog|cat|fish)");
if (regex.IsMatch(someInput))
{

}
```

由于编译器不允许生成器修改连接此模式的代码，对于生成器来说，这几乎是不可能的。 他们需要在`IsMatch`实现中采用反射，或者要求用户将其调用站点更改为新方法 - 重构 regex 以将字符串文本作为参数传递。 太乱了

## <a name="detailed-design"></a>详细设计
语言将更改，以允许`partial`使用显式辅助功能修改器对方法进行给下用。 这意味着它们可以标记为`private`、`public`等... 

当方法`partial`具有显式辅助功能修改器时，尽管语言将要求声明具有匹配的定义，即使辅助功能为`private`：

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

此外，该语言将删除对具有显式可访问性`partial`的方法上可以显示的内容的所有限制。 此类声明可以包含非 void 返回类型，`ref`或`out`参数、`extern`修改器等...这些签名具有 C# 语言的完整表达性。

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

这显式允许`partial`方法参与`overrides`和`interface`实现：

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

当方法`partial`包含非法元素时，编译器将更改它发出的错误，基本上说：

> 不能用于`ref`缺乏显式`partial`可访问性的方法 

这将有助于在使用此功能时将开发人员指向正确的方向。

限制：
- `partial`具有显式可访问性的声明必须具有定义
- `partial`声明和定义签名必须与所有方法和参数修改器匹配。 唯一可以不同的方面是参数名称和属性列表（这不是新的，而是方法的现有`partial`要求）。

## <a name="questions"></a>问题

### <a name="partial-on-all-members"></a>部分对所有成员
鉴于我们正在扩展`partial`，以更友好的来源生成器，我们也应该扩展它工作的所有类成员？ 例如，我们应该能够声明`partial`构造函数、运算符等...

**分辨率**这个想法是健全的，但在C#9时间表的这个点，我们试图避免不必要的功能蠕变。 要解决扩展功能以使用现代源生成器的紧迫问题。 

扩展`partial`以支持其他成员将考虑为 C# 10 版本。 看来我们会考虑这个扩展。

### <a name="use-abstract-instead-of-partial"></a>使用抽象而不是部分
这项建议的症结在于确保宣言具有相应的定义/执行。 鉴于我们应使用`abstract`，因为它已经是一个语言关键字，迫使开发人员考虑有一个实现？

**分辨率**有一个健康的讨论，但最终决定反对。
是的，要求是熟悉的，但概念却大相径庭。
很容易让开发人员相信他们在创建虚拟插槽时，他们没有这样做。
