---
ms.openlocfilehash: c1a77d9337ecd16f5ea1c30d18f6422552b0dfb2
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483582"
---
# <a name="ref-local-reassignment"></a>引用本地重新分配

在C# 7.3 中，我们添加了对重新绑定 ref 局部变量或 ref 参数的引用的支持。

我们将以下项添加到 `assignment_operator`集。

```antlr
assignment_operator
    : '=' 'ref'
    ;
```

`=ref` 运算符称为***ref 赋值运算符***。 它不是*复合赋值运算符*。 左操作数必须是绑定到 ref 局部变量的表达式、ref 参数（不是 `this`）或 out 参数。 右操作数必须是生成左值的表达式，该表达式指定与左操作数类型相同的值。

右操作数必须在 ref 赋值时明确赋值。

当左操作数绑定到 `out` 参数时，如果未在 ref 赋值运算符的开头明确赋值 `out` 参数，则是错误的。

如果左操作数为可写引用（即它指定了除 `ref readonly` 本地或 `in` 参数以外的任何内容），则右操作数必须是可写的左值。

Ref 赋值运算符生成已分配类型的左值。 如果左操作数可写（即不 `ref readonly` 或 `in`），则它是可写的。

此操作员的安全规则如下：

- 对于引用重新分配 `e1 = ref e2`，`e2` 的*ref 安全到转义符*的范围必须至少与 `e1`的*ref 安全对转义*。

在中，引用*安全到转义*的定义在[安全性上适用于类似引用类型](../csharp-7.2/span-safety.md)
