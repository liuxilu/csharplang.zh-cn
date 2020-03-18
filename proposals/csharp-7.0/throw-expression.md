---
ms.openlocfilehash: 2532a24e867930d2f27614f19c77585dbce80dfa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483534"
---
# <a name="throw-expression"></a>Throw 表达式

我们扩展了要包含的表达式窗体集

```antlr
throw_expression
    : 'throw' null_coalescing_expression
    ;

null_coalescing_expression
    : throw_expression
    ;
```

类型规则如下所示：

- *Throw_expression*没有类型。
- *Throw_expression*可以通过隐式转换转换为每种类型。

*Throw 表达式*引发的值是通过计算 null_coalescing_expression 得出的值，该*null_coalescing_expression*必须表示从 `System.Exception` 派生的类类型 `System.Exception`的类类型的值，或者是具有其有效基类的 `System.Exception` （或子类）的类型参数类型的值。 如果表达式的计算生成 `null`，则改为引发 `System.NullReferenceException`。

计算*引发表达式*时的行为与为[ *throw 语句*指定](../../spec/statements.md#the-throw-statement)的行为相同。

流分析规则如下所示：

- 对于每个*变量 v*，都将在*throw_expression* iff 的*null_coalescing_expression*之前明确赋值*v* ， *throw_expression*之前，该变量已明确赋值。
- 对于每个变量*v*， *v*在*throw_expression*后都是明确赋值。

仅在以下语法上下文中允许*引发表达式*：
- 作为三元条件运算符的第二个或第三个操作数 `?:`
- 作为 null 合并运算符的第二个操作数 `??`
- 作为 expression-bodied lambda 或方法的主体。
