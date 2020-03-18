---
ms.openlocfilehash: 3d10cacef98e802333c8cbe65edb93c19c74cabf
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483492"
---
# <a name="null-conditional-await"></a>null-条件 await

* [x] 建议
* [] 原型：无
* [] 实现：无
* [] 规范：已开始，小于

## <a name="summary"></a>总结
[summary]: #summary

支持形式 `await? e`的表达式，该表达式在 `e` 为非 null 时等待，否则将导致 `null`。

## <a name="motivation"></a>动机
[motivation]: #motivation

这是一种常见的编码模式，此功能与现有的 null 传播和 null 合并运算符的协同工作非常好。

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

我们添加了一种新形式的*await_expression*：

```antlr
await_expression
    : 'await' '?' unary_expression
    ;
```

只有在该操作数为非 null 时，null 条件 `await` 运算符才会等待其操作数。 否则，应用运算符的结果为 null。

使用[null 条件运算符的规则](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator)计算结果的类型。

> **注意：** 如果 `e` 的类型为 `Task`，则如果 `e` 为 `null`，则 `await? e;` 不会执行任何操作，并且等待 `e` `null`。
>
> 如果 `e` 的类型 `Task<K>` （其中 `K` 为值类型），则 `await? e` 将生成类型 `K?`的值。

## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

与任何语言功能一样，我们必须回答对语言的额外复杂性是否偿还，以提供给将从功能中受益的C#程序的主体。

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

尽管它需要一些样板代码，但该运算符的用法通常可以替换为表达式，如 `(e == null) ? null : await e` 或 `if (e != null) await e`之类的语句。

## <a name="unresolved-questions"></a>未解决的问题
[unresolved]: #unresolved-questions

- [] 需要 LDM 审核

## <a name="design-meetings"></a>设计会议

无。
