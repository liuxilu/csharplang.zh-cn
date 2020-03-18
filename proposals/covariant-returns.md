---
ms.openlocfilehash: 392d932459ff0a4cb0d6d32c1606f73f9b913c68
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483456"
---
# <a name="covariant-return-types"></a>协变返回类型

* [x] 建议
* [] 原型：未启动
* [] 实现：未启动
* [] 规范：未启动

## <a name="summary"></a>总结
[summary]: #summary

支持_协变返回类型_。 具体而言，允许重写方法的派生引用类型比它重写的方法更多。

## <a name="motivation"></a>动机
[motivation]: #motivation

代码中的一种常见模式是，必须使用不同的方法名称来解决语言约束，而重写必须返回与重写的方法相同的类型。 请参阅下面的 Roslyn 代码库中的示例。

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

支持_协变返回类型_。 具体而言，允许重写方法的派生引用类型比它重写的方法更多。 这将应用到方法和属性，并且在类和接口中受支持。

这在工厂模式下非常有用。 例如，在 Roslyn 代码库中，我们将

``` cs
class Compilation ...
{
    virtual Compilation WithOptions(Options options)...
}
```

``` cs
class CSharpCompilation : Compilation
{
    override CSharpCompilation WithOptions(Options options)...
}
```

此方法的实现是为了使编译器将重写方法作为 "新" 虚拟方法发出，该方法隐藏了基类方法，同时还提供了一个使用派生类方法调用来实现基类方法的_bridge 方法_。

## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

- [] 每个语言更改都必须为其本身付费。
- [] 我们应该确保性能合理，甚至在深层继承层次结构中也是如此
- [] 我们应该确保翻译策略的项目不会影响语言语义，即使在使用旧编译器的新 IL 时也是如此。

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

我们可以略微放宽语言规则，在源中，

```csharp
abstract class Cloneable
{
    public abstract Cloneable Clone();
}

class Digit : Cloneable
{
    public override Cloneable Clone()
    {
        return this.Clone();
    }

    public new Digit Clone() // Error: 'Digit' already defines a member called 'Clone' with the same parameter types
    {
        return this;
    }
}
```

## <a name="unresolved-questions"></a>未解决的问题
[unresolved]: #unresolved-questions

- [] 编译为使用此功能的 Api 将在较旧版本的语言中工作？

## <a name="design-meetings"></a>设计会议

尚无。 在 <https://github.com/dotnet/roslyn/issues/357>有一些讨论。