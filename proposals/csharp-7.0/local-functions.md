---
ms.openlocfilehash: a4b0fbbc600eaf1e705ad8e6bd9fcecb44100761
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483444"
---
# <a name="local-functions"></a>本地函数

我们扩展C#了以支持块范围内的函数声明。 本地函数可以使用封闭范围内的（捕获）变量。

编译器使用流分析来检测本地函数使用的变量，然后再为其赋值。 函数的每个调用都需要明确赋值此类变量。 同样，编译器会确定返回时明确赋值的变量。 调用本地函数后，此类变量被视为明确赋值。

在定义之前，可以从词法点调用本地函数。 当本地函数声明语句不可访问时，它们不会导致警告。

TODO：_编写规范_

## <a name="syntax-grammar"></a>语法语法

此语法表示为与当前规范语法的不同之处。

```diff
declaration-statement
    : local-variable-declaration ';'
    | local-constant-declaration ';'
+   | local-function-declaration
    ;

+local-function-declaration
+   : local-function-header local-function-body
+   ;

+local-function-header
+   : local-function-modifiers? return-type identifier type-parameter-list?
+       ( formal-parameter-list? ) type-parameter-constraints-clauses
+   ;

+local-function-modifiers
+   : (async | unsafe)
+   ;

+local-function-body
+   : block
+   | arrow-expression-body
+   ;
```

本地函数可能使用封闭范围中定义的变量。 当前实现要求在本地函数内读取的每个变量都是明确赋值的，如同在其定义点执行本地函数一样。 此外，还必须在任何使用点 "执行" 本地函数定义。

在试验这一点之后（例如，不可能定义两个相互递归的本地函数），我们已经修改了我们希望明确赋值的工作方式。 修订版本（尚未实现）是在本地函数中读取的所有局部变量都必须在本地函数的每个调用中明确赋值。 这实际上比声音更为微妙，而且还有一组工作要使其正常工作。 完成后，您将能够将您的本地函数移动到它的封闭块末尾。

新的明确赋值规则与推断本地函数的返回类型不兼容，因此，我们可能会删除对推断返回类型的支持。

除非将本地函数转换为委托，否则将在值类型为的帧中进行捕获。 这意味着，不会因将本地函数与捕获一起使用而获得任何垃圾回收压力。

### <a name="reachability"></a>可访问性

我们添加到规范

> 语句 expression-bodied lambda 表达式或本地函数的主体被视为可访问。
