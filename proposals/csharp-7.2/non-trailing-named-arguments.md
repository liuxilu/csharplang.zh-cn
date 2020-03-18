---
ms.openlocfilehash: ac2b233eb703b5eea3bd2dfdbeeadd7494b0c695
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483642"
---
# <a name="non-trailing-named-arguments"></a>非尾随命名参数

## <a name="summary"></a>总结
[summary]: #summary
如果命名参数用于其正确位置，则允许在非尾随位置使用该参数。 例如：`DoSomething(isEmployed:true, name, age);`。

## <a name="motivation"></a>动机
[motivation]: #motivation

主要动机是避免键入冗余信息。 为了阐明代码而不是按顺序传递参数，通常可以将作为文本的参数（例如 `null`、`true`）命名为。
当前不允许（`CS1738`），除非还将所有以下参数命名为。

```csharp
DoSomething(isEmployed:true, name, age); // currently disallowed, even though all arguments are in position
// CS1738 "Named argument specifications must appear after all fixed arguments have been specified"
```

一些其他示例：
```csharp
public void DoSomething(bool isEmployed, string personName, int personAge) { ... }

DoSomething(isEmployed:true, name, age); // currently CS1738, but would become legal
DoSomething(true, personName:name, age); // currently CS1738, but would become legal
DoSomething(name, isEmployed:true, age); // remains illegal
DoSomething(name, age, isEmployed:true); // remains illegal
DoSomething(true, personAge:age, personName:name); // already legal
```

这也适用于 params：
```csharp
public class Task
{
    public static Task When(TaskStatus all, TaskStatus any, params Task[] tasks);
}
Task.When(all: TaskStatus.RanToCompletion, any: TaskStatus.Faulted, task1, task2)
```

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

在§7.5.1 （自变量列表）中，规范当前指出：
> 带有*参数名*的*参数*称为__命名参数__，而没有*参数名* *的参数是*__位置参数__。 位置参数出现在*参数列表*中的命名参数之后的错误。

建议删除此错误并更新用于查找自变量的相应参数的规则（第7.5.1.1）：

参数中的参数-实例构造函数、方法、索引器和委托的列表：
- [现有规则]
- 未命名的参数在不在指定位置的命名参数或命名的 params 参数之后，不对应于参数。

具体而言，这会阻止调用 `M(c: false, valueB);``void M(bool a = true, bool b = true, bool c = true, );`。 第一个参数在位置上使用（参数在第一个位置使用，但名为 "c" 的参数在第三个位置），因此应将以下参数命名为。

换言之，仅当名称和位置导致查找相同的相应参数时，才允许使用非尾随命名参数。

## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

此建议在重载决策中 exacerbates 现有微妙之处和命名参数。 例如：

```csharp
void M(int x, int y) { }
void M<T>(T y, int x) { }

void M2()
{
    M(3, 4);
    M(y: 3, x: 4); // Invokes M(int, int)
    M(y: 3, 4); // Invokes M<T>(T, int)
}
```

你现在可以通过交换参数来实现这种情况：

```csharp
void M(int y, int x) { }
void M<T>(int x, T y) { }

void M2()
{
    M(3, 4);
    M(x: 3, y: 4); // Invokes M(int, int)
    M(3, y: 4); // Invokes M<T>(int, T)
}
```

同样，如果你有两种方法 `void M(int a, int b)` 和 `void M(int x, string y)`，则被误认为调用 `M(x: 1, 2)` 将基于第二个重载生成诊断（"无法从 ' int ' 转换为 ' string '"）。 如果在尾随位置使用命名参数，则已存在此问题。

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

有几种方法可以考虑：

- 现状
- 当你在中间键入特定名称时，提供 IDE 协助来填充尾随参数的所有名称。

这两种方法都受到更详细程度的影响，因为它们会引入多个命名参数，即使只需要参数列表开头有一个文本名称，也是如此。

## <a name="unresolved-questions"></a>未解决的问题
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>设计会议
[ldm]: #ldm
此功能在5月 2017 16 日的 LDM 中进行了简单的介绍，并按原则进行审批（可转到提议/原型）。 它还在2017年6月28日讨论。

与倡导问题相关的初始讨论 https://github.com/dotnet/csharplang/issues/518 https://github.com/dotnet/csharplang/issues/570
