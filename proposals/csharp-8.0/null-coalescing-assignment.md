---
ms.openlocfilehash: 962236dd30365b2e9c33bb1f6ec5328fdf22e90a
ms.sourcegitcommit: c2fe8f1d150ac6ac171072d1c6f9df883adcbb40
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/12/2020
ms.locfileid: "83203261"
---
# <a name="null-coalescing-assignment"></a>NULL 合并分配

* [x] 建议
* [x] 原型：已完成
* [x] 实现：已完成
* [x] 规范：以下

## <a name="summary"></a>总结
[summary]: #summary

简化了一个常见的编码模式，其中如果变量为 null，则为该变量分配一个值。

作为此建议的一部分，我们还将放宽的类型要求， `??` 以允许类型为不受约束的类型参数的表达式在左侧使用。

## <a name="motivation"></a>动机
[motivation]: #motivation

通常会看到窗体的代码

```csharp
if (variable == null)
{
    variable = expression;
}
```

此建议向执行此函数的语言添加不可重载的二元运算符。

此功能至少有8个单独的社区请求。

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

我们添加了一个新的赋值运算符形式

``` antlr
assignment_operator
    : '??='
    ;
```

它遵循[复合赋值运算符的现有语义规则](../../spec/expressions.md#compound-assignment)，只不过如果左侧非 null，则 elide 赋值。 此功能的规则如下所示。

给定 `a ??= b` ，其中 `A` 是的类型 `a` ， `B` 是的类型 `b` ， `A0` 是 if 的基础类型（ `A` 如果 `A` 是可以为 null 的值类型）：

1. 如果 `A` 不存在或是不可以为 null 的值类型，则会发生编译时错误。
2. 如果 `B` 无法隐式转换为 `A` 或 `A0` （如果 `A0` 存在），则会发生编译时错误。
3. 如果 `A0` 存在且可 `B` 隐式转换为，并且不是动态的，则的 `A0` `B` 类型 `a ??= b` 为 `A0` 。 `a ??= b`在运行时评估为：
   ```C#
   var tmp = a.GetValueOrDefault();
   if (!a.HasValue) { tmp = b; a = tmp; }
   tmp
   ```
   除外， `a` 只计算一次。
4. 否则，的类型 `a ??= b` 为 `A` 。 `a ??= b`在运行时将作为计算 `a ?? (a = b)` ，只不过 `a` 只计算一次。


对于的类型要求的 relaxation `??` ，我们将更新其当前声明的规范，给定 `a ?? b` ，其中 `A` 是 `a` 以下类型：

> 1. 如果存在并且不是可以为 null 的类型或引用类型，则会发生编译时错误。

我们放宽了以下要求：

1. 如果存在并且是不可为 null 的值类型，则会发生编译时错误。

这允许空合并运算符处理不受约束的类型参数，因为不存在不受约束的类型参数 T，它不是可以为 null 的类型，并且不是引用类型。

## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

与任何语言功能一样，我们必须回答对 c # 程序正文提供的更清晰的偿还，以使其受益于该功能。

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

程序员可以编写 `(x = x ?? y)` 、 `if (x == null) x = y;` 或手动编写 `x ?? (x = y)` 。

## <a name="unresolved-questions"></a>未解决的问题
[unresolved]: #unresolved-questions

- [] 需要 LDM 审核
- [] 是否还支持 `&&=` 和 `||=` 操作员？

## <a name="design-meetings"></a>设计会议

无。
