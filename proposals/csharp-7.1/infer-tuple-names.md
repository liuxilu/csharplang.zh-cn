---
ms.openlocfilehash: 25e95b3ab8c384a7e66e59a7f9422cc9699074d7
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483684"
---
# <a name="infer-tuple-names-aka-tuple-projection-initializers"></a>推断元组名称（也称为 元组投影初始值设定项）

## <a name="summary"></a>总结
[summary]: #summary

在许多常见情况下，此功能允许忽略元组元素名称，而不是推断。 例如，可以从 `(x.f1, x?.f2)`推断元素名称 "f1" 和 "f2"，而不是键入 `(f1: x.f1, f2: x?.f2)`。

这与匿名类型的行为类似，允许在创建过程中推断成员名称。 例如，`new { x.f1, y?.f2 }` 声明成员 "f1" 和 "f2"。

在 LINQ 中使用元组时，此操作特别有用：

```csharp
// "c" and "result" have element names "f1" and "f2"
var result = list.Select(c => (c.f1, c.f2)).Where(t => t.f2 == 1); 
```

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

更改包括两个部分：

1.  尝试为没有显式名称的每个元组元素推断候选名称：
    -   使用与匿名类型的名称推理相同的规则。
        - 在C#中，这可以实现三种情况： `y` （标识符）、`x.y` （简单成员访问）和 `x?.y` （条件性访问）。
        - 在 VB 中，这可以实现其他情况，如 `x.y()`。
    -   正在拒绝保留元组名称（在中C#区分大小写，不区分大小写），因为它们被禁止或已隐式。 例如 `ItemN`、`Rest`和 `ToString`。
    -   如果任何候选名称在整个元组中都是C#重复的（在中区分大小写，不区分大小写），则删除这些候选项，
2.  在转换过程中（这会对从元组文本中删除名称进行检查和发出警告），推断名称不会生成任何警告。 这样可以避免破坏现有的元组代码。

请注意，处理重复项的规则与匿名类型的规则不同。 例如，`new { x.f1, x.f1 }` 生成错误，但仍允许 `(x.f1, x.f1)` （只是没有任何推断名称）。 这样可以避免破坏现有的元组代码。

为保持一致性，将应用到析构（在中C#为）生成的元组：

```csharp
// tuple has element names "f1" and "f2" 
var tuple = ((x.f1, x?.f2) = (1, 2));
```

这同样也适用于 VB 元组，使用特定于 VB 的规则从表达式推断名称，不区分大小写的名称比较。

将C# 7.1 编译器（或更高版本）与语言版本 "7.0" 一起使用时，将推断元素名称（尽管此功能不可用），但会出现用于尝试访问它们的使用站点错误。 这将限制稍后会出现兼容性问题的新代码的添加（如下所述）。

## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

主要缺点是，这会引入C# 7.0 的兼容性中断：

```csharp
Action y = () => M();
var t = (x: x, y);
t.y(); // this might have previously picked up an extension method called “y”, but would now call the lambda.
```

兼容性委员会发现，此中断是可接受的，因为它受到限制，并且自元组（ C#在7.0 中）后的时间范围很短。

## <a name="references"></a>参考
- [LDM 4 月 4 2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md#tuple-names)
- [Github 讨论](https://github.com/dotnet/csharplang/issues/370)（感谢 @alrz 提出此问题）
- [元组设计](https://github.com/dotnet/roslyn/blob/master/docs/features/tuples.md)
