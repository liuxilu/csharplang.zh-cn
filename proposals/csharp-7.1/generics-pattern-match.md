---
ms.openlocfilehash: 4f66c0f60d05ed6509a1d0843318a71d1b36c351
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483672"
---
# <a name="pattern-matching-with-generics"></a>采用泛型的模式匹配

* [x] 建议
* [] 原型：
* [] 实现：
* [] 规范：

## <a name="summary"></a>总结
[summary]: #summary

对于[ C#现有 as 运算符](../../spec/expressions.md#the-as-operator)的规范，如果这两个类型都是开放类型，则不允许在操作数的类型与指定类型之间进行转换。 但在 7 C#中，`Type identifier` 模式要求输入类型与给定类型之间存在转换。

建议放宽这一点，并更改 `expression is Type identifier`，除了允许在C# 7 中使用条件以外，还可以在允许 `expression as Type` 时允许。 具体而言，这种情况下，表达式或指定类型的类型是开放类型。 

## <a name="motivation"></a>动机
[motivation]: #motivation

当前允许模式匹配 "明显" 的情况当前无法编译。 请参阅 https://github.com/dotnet/roslyn/issues/16195。

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

我们更改了模式匹配规范中的段落（建议的加法以粗体显示）：

> 左侧和给定类型的静态类型的某些组合被视为不兼容并导致编译时错误。 如果存在标识转换、隐式引用转换、装箱转换、显式引用转换或从 `E` 到 `T`的取消装箱转换，**或者 `E` 或 `T` 是开放类型**，则将静态类型 `E` 的值视为与类型 `T`*模式兼容*。 如果类型 `E` 的表达式不与它所匹配的类型模式中的类型兼容，则会发生编译时错误。

## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

无。

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

无。

## <a name="unresolved-questions"></a>未解决的问题
[unresolved]: #unresolved-questions

无。

## <a name="design-meetings"></a>设计会议

LDM 认为这是一个问题，认为这是一个 bug 修复级别的更改。 我们将其视为一种独立的语言功能，因为只要在语言发布后进行更改，就会引入不兼容性。 使用建议的更改需要程序员指定语言版本7.1。
