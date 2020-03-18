---
ms.openlocfilehash: 032cb8590a0b6e83f8ab6201e10720f1b254c605
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483522"
---
# <a name="nullable-enhanced-common-type"></a>可为 null 的通用类型

* [x] 建议
* [] 原型：无
* [] 实现：无
* [] 规范：请参见下面

## <a name="summary"></a>总结
[summary]: #summary

在某些情况下，当前通用类型算法结果是非常直观的，并会导致程序员将冗余强制转换为代码。 进行此更改后，表达式（如 `condition ? 1 : null`）将导致类型为 `int?`的值，`condition ? x : 1.0` 其中 `x` 类型为 `int?` 的表达式将导致类型 `double?`的值。

## <a name="motivation"></a>动机
[motivation]: #motivation

这是类似于程序员的常见原因，如不必要的样板代码。

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

我们修改了用于[查找一组表达式的最佳通用类型的](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions)规范，以影响下列情况：

- 如果一个表达式是不可以为 null 的值类型 `T` 而另一个为 null 文本，则结果为类型 `T?`。
- 如果一个表达式是可以为 null 的值类型 `T?`，另一个表达式的值类型为 `U`，并且存在从 `T` 到 `U`的隐式转换，则结果为类型 `U?`。

这应该会影响语言的以下方面：

- [三元表达式](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)
- 隐式类型的[数组创建表达式](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions)
- 推断[lambda 的返回类型](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type)推断类型
- 涉及泛型的事例，如将 `M<T>(T a, T b)` 作为 `M(1, null)`调用。

更准确地讲，我们将更改规范的以下部分（以粗体插入，删除删除线）：

> #### <a name="output-type-inferences"></a>输出类型推断
> 
> *输出类型推断*是通过以下方式*从*`E`*到*类型 `T` 的表达式进行的：
> 
> *  如果 `E` 是 `U` （[推断返回类型](expressions.md#inferred-return-type)）且 `T` 为委托类型或具有返回类型 `Tb`的表达式树类型的匿名函数，则将*从*`U`[Lower-bound inferences](expressions.md#lower-bound-inferences)*到*`Tb`进行*下限推理*。
> *  否则，如果 `E` 为方法组，`T` 是参数类型为 `T1...Tk` 和返回类型 `Tb`的委托类型或表达式树类型，并且 `E` 生成一个返回类型 `T1...Tk` 的单个方法，则*从*`U`*到*`U`*生成一个方法*。`Tb`
> *  \* * 否则，如果 `E` 是可以为 null 的值类型为 *`U?`的表达式*，则*从*`U`*到*`T`，并将*null 界限*添加到 `T`。 **
> *  否则，如果 `E` 是 `U`类型的表达式，则将*从*`U`*到*`T`进行*下限推理*。
> *  **否则，如果 `E` 是值为 `null`的常量表达式，则会将一个*null 界限*添加到 `T`** 
> *  否则，不进行推断。

> #### <a name="fixing"></a>更正
> 
> 使用一组界限 `Xi` 未*固定*的类型变量是*固定*的，如下所示：
> 
> *  *候选类型*集 `Uj` 以 `Xi`的边界集中的所有类型的集合开头。
> *  然后，我们依次检查每个绑定 for `Xi`：对于每个完全绑定 `U`，`Xi` `Uj` 的所有类型，这些类型与从候选集删除 `U` 不完全相同。 对于 `Xi` 的每个下限 `U`，从候选集中将*不*会从 `U` 中删除任何 `Uj` 类型的隐式转换。 对于 `Xi` 的每个上限 *`U`，`Uj`* 将从候选集删除没有隐式转换到 `U` 的所有类型。
> *  如果其余的候选类型 `Uj` 有一个唯一的类型 `V` 从该类型中有一个到所有其他候选类型的隐式转换，则~~`Xi` 固定到 `V`。~~
>     -  **如果 `V` 是一个值类型，并且 `Xi`的*绑定为 null* ，则 `Xi` 固定为 `V?`**
>     -  **否则 `Xi` 固定到 `V`**
> *  否则，类型推理将失败。

## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

此建议可能会导致某些不兼容问题。

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

无。

## <a name="unresolved-questions"></a>未解决的问题
[unresolved]: #unresolved-questions

- [] 此提议引入的不兼容性严重性（如果有）以及如何进行仲裁？

## <a name="design-meetings"></a>设计会议

无。
