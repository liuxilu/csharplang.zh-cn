---
ms.openlocfilehash: a78567594d39fc4e204e12c4f2f247b8d6995c38
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483858"
---
# <a name="expression-variables-in-initializers"></a>初始化表达式中的表达式变量

## <a name="summary"></a>总结
[summary]: #summary

我们扩展了7中C#引入的功能，以允许包含字段初始值设定项、属性初始值设定项、ctor 初始值设定项和查询子句中的表达式变量（out 变量声明和声明模式）的表达式。

## <a name="motivation"></a>动机
[motivation]: #motivation

由于缺乏时间，这就完成了C#语言的几个粗略的边缘。

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

我们消除了在 ctor 初始值设定项中阻止表达式变量（out 变量声明和声明模式）声明的限制。 此类声明的变量在构造函数的整个主体范围内。

我们消除了在字段或属性初始值设定项中阻止表达式变量（out 变量声明和声明模式）的声明的限制。 此类声明的变量在整个初始化表达式的范围内。

我们消除了阻止在转换为 lambda 主体的查询表达式子句中声明表达式变量（out 变量声明和声明模式）的限制。 此类声明的变量在整个查询子句表达式的范围内。

## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

无。

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

在这些上下文中声明的表达式变量的适当范围并不明显，值得进一步进行 LDM 讨论。

## <a name="unresolved-questions"></a>未解决的问题
[unresolved]: #unresolved-questions

- [] 这些变量的适当作用域是什么？

## <a name="design-meetings"></a>设计会议

无。
