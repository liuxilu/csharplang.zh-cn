---
ms.openlocfilehash: a0d80afc47e9f0073237db9b8d7a4f0b045c1b0b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483504"
---
# <a name="out-variable-declarations"></a>Out 变量声明

*Out 变量声明*功能允许在将变量作为 `out` 参数传递的位置声明该变量。

```antlr
argument_value
    : 'out' type identifier
    | ...
    ;
```

用这种方式声明的变量称为*out 变量*。 您可以对变量的类型使用上下文关键字 `var`。 作用域将与通过模式匹配引入的*模式变量*的作用域相同。

根据语言规范（7.6.7 元素访问部分），元素访问的参数列表（索引表达式）不包含 ref 或 out 参数。 但编译器允许它们用于各种方案，例如，在接受 `out`的元数据中声明的索引器。

在 argument_value 引入的本地变量的范围内，在其声明之前的文本位置引用该局部变量是编译时错误。

在立即包含其声明的同一个参数列表中引用隐式类型化的（第8.5.1） out 变量也是错误的。

重载决策的修改方式如下：

我们添加了一个新的转换：

> 存在从隐式类型化的 out 变量声明到每个类型的*表达式的转换*。

又

> 显式类型化出变量的类型是声明的类型。

and

> 隐式类型化出变量没有类型。

从隐式类型化出变量声明中的*表达式的转换*不会被视为比*从表达式*进行的任何其他转换都好。

隐式类型化 out 变量的类型是重载决策选择的方法签名中的相应参数的类型。

添加了新的语法节点 `DeclarationExpressionSyntax` 以表示 out var 参数中的声明。
