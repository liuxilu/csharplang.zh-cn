---
ms.openlocfilehash: 1852356b830e29c3537a4de559fef32fd2c2f8b6
ms.sourcegitcommit: f7952cdddf85316a4beec493a0ecc14fcb3241c8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "79483912"
---
# <a name="target-typed-default-literal"></a>目标类型 "默认" 文本

* [x] 建议
* [x] 原型
* [x] 实现
* [] 规范

## <a name="summary"></a>总结
[summary]: #summary

目标类型化 `default` 功能是 `default(T)` 运算符的较短形式，这允许省略类型。 改为通过目标类型推断其类型。 除此之外，它的行为类似于 `default(T)`。

## <a name="motivation"></a>动机
[motivation]: #motivation

主要动机是避免键入冗余信息。

例如，在调用 `void Method(ImmutableArray<SomeType> array)`时，*默认*文本允许 `M(default)` 代替 `M(default(ImmutableArray<SomeType>))`。

这适用于许多方案，例如：

- 声明局部变量（`ImmutableArray<SomeType> x = default;`）
- 三元运算（`var x = flag ? default : ImmutableArray<SomeType>.Empty;`）
- 方法和 lambda 中的返回（`return default;`）
- 声明可选参数的默认值（`void Method(ImmutableArray<SomeType> arrayOpt = default)`）
- 在数组创建表达式中包含默认值（`var x = new[] { default, ImmutableArray.Create(y) };`）


## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

引入了一个新的表达式，即*默认*文本。 具有此分类的表达式可通过*默认文本转换*隐式转换为任何类型。 

*默认*文本的类型的推理与*null*文本的类型的推理相同，不同之处在于允许任何类型（而不仅仅是引用类型）。

此转换将生成推断类型的默认值。

*默认*文本可能具有常量值，具体取决于推断类型。 `const int x = default;` 是合法的，但 `const int? y = default;` 不是这样。

只要另一个操作数具有类型，*默认*文本就可以是相等运算符的操作数。 因此 `default == x` 和 `x == default` 是有效的表达式，但 `default == default` 是非法的。

## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

一个次要缺点是，*默认*文本可用于在大多数上下文中替代*null*文本。 两个异常 `throw null;` 和 `null == null`，这两个异常允许用于*null*文本，但不是*默认*文本。

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

有几种方法可以考虑：

- 现状：此功能并不在其自身的优点上进行调整，开发人员继续使用具有显式类型的默认运算符。
- 扩展 null 文本：这是 `Nothing`的 VB 方法。 我们可能允许 `int x = null;`。

## <a name="unresolved-questions"></a>未解决的问题
[unresolved]: #unresolved-questions

- [x] 应*默认*允许*作为 or 运算符的操作数*？ *as* 答：不允许 `default is T`、允许 `x is default`、允许 `default as RefType` （始终为空的警告）

## <a name="design-meetings"></a>设计会议

- [LDM 3/7/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-07.md)
- [LDM 3/28/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-28.md)
- [LDM 5/31/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md#default-in-operators)
