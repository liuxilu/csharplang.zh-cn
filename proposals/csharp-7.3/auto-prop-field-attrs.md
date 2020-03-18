---
ms.openlocfilehash: 2e054c629f71ae37b112300905c3106f9ce977e8
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483636"
---
# <a name="auto-implemented-property-field-targeted-attributes"></a>自动实现的属性字段目标特性

## <a name="summary"></a>总结
[summary]: #summary

此功能旨在允许开发人员将属性直接应用到自动实现属性的支持字段。

## <a name="motivation"></a>动机
[motivation]: #motivation

目前不能将属性应用到自动实现的属性的支持字段。  在这种情况下，开发人员必须使用面向字段的属性，因此，它们会强制手动声明字段并使用更详细的属性语法。  如果在C#事件的生成支持字段上始终支持字段目标属性，则有必要将相同的功能扩展到其属性 kin。

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

简而言之，以下内容是合法C#的，不会产生警告：

```csharp
[Serializable]
public class Foo 
{
    [field: NonSerialized]
    public string MySecret { get; set; }
}
```

这会导致将字段目标属性应用于编译器生成的支持字段：

```csharp
[Serializable]
public class Foo 
{
    [NonSerialized]
    private string _mySecretBackingField;
    
    public string MySecret
    {
        get { return _mySecretBackingField; }
        set { _mySecretBackingField = value; }
    }
}
```

如前文所述，这会引入带有C# 1.0 的事件语法的奇偶校验，因为以下内容已合法并按预期方式工作：

```csharp
[Serializable]
public class Foo
{
    [field: NonSerialized]
    public event EventHandler MyEvent;
}
```

## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

实现此更改有两个潜在缺点：

1. 尝试将特性应用于自动实现属性的字段将产生编译器警告，指出该块中的特性将被忽略。  如果更改了编译器以支持这些属性，则这些属性将应用于后续重新编译时，这可能会在运行时更改程序的行为。
1. 尝试将属性的 AttributeUsage 目标应用到自动实现属性的字段时，编译器当前不会验证这些属性的目标。  如果编译器已更改为支持字段目标属性，但无法将相关属性应用于字段，则编译器将发出错误而不是警告，从而中断生成。

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

## <a name="unresolved-questions"></a>未解决的问题
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>设计会议
