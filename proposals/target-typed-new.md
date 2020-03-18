---
ms.openlocfilehash: 4e2a536bab00859b003e8d967cb1927a99a9fa21
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483528"
---

# <a name="target-typed-new-expressions"></a>目标类型 `new` 表达式

* [x] 建议
* [x][原型](https://github.com/alrz/roslyn/tree/features/target-typed-new)
* [] 实现
* [] 规范

## <a name="summary"></a>总结
[summary]: #summary

当类型已知时，不需要构造函数的类型规范。 

## <a name="motivation"></a>动机
[motivation]: #motivation

允许字段初始化，而不复制类型。
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```
如果可从用法推断，则允许省略类型。
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```
实例化对象，而不会对类型进行拼写检查。
```cs
private readonly static object s_syncObj = new();
```
## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

在出现括号时，将修改*object_creation_expression*语法，使*类型*成为可选的。 这是解决*anonymous_object_creation_expression*的多义性所必需的。
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```
目标类型 `new` 可转换为任何类型。 因此，它不会导致重载决策。 这主要是为了避免意外的重大更改。

参数列表和初始值设定项表达式将在确定类型后进行绑定。

表达式的类型将从目标类型推断而来，这需要是以下类型之一：

- **任何结构类型**
- **任何引用类型**
- 具有构造函数或 `struct` 约束的**任何类型参数**

但有以下例外：

- **枚举类型：** 并非所有枚举类型都包含常量零，因此应使用显式枚举成员。
- **接口类型：** 这是一项小型功能，应更好地显式提及该类型。
- **数组类型：** 数组需要一个特殊的语法来提供长度。
- **Struct 默认构造函数**：这会排除所有基元类型和大多数值类型。 如果要使用此类类型的默认值，可以改为写入 `default`。

还会排除*object_creation_expression*中不允许的所有其他类型，例如，指针类型。

> **打开问题：** 我们是否应允许委托和元组作为目标类型？

以上规则包括委托（引用类型）和元组（结构类型）。 尽管这两种类型都是可构造，但如果类型为 inferable，则可以使用匿名函数或元组文本。
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

var x = new() == (1, 2); // ruled out by "use of struct default constructor"
var x = new(1, 2) == (1, 2) // "new" is redundant
```


> **打开问题：** 我们是否应允许 `Exception` 作为目标类型的 `throw new()`？

目前 `throw null`，但不 `throw default` （尽管它具有相同的效果）。 另一方面，`throw new()` 可以用作 `throw new Exception(...)`的简写。 请注意，当前规范已允许使用此方法。 `Exception` 是引用类型，并且 throw 语句的规范表明表达式已转换为 `Exception`。

> **打开问题：** 我们是否应该允许使用用户定义的比较和算术运算符的目标类型 `new`？

为了进行比较，`default` 仅支持相等（用户定义的和内置的）运算符。 还需要支持 `new()` 的其他运算符吗？

## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

无。

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

大多数关于字段初始化中太长时间的投诉都是关于类型*参数*，而不是类型本身，我们只能推断 `new Dictionary(...)` （或类似）类型参数，并在本地从参数或集合初始值设定项推断类型参数。

## <a name="questions"></a>问题
[questions]: #questions

- 是否应在表达式树中禁止使用情况？ 不
- 此功能如何与 `dynamic` 参数交互？ （无特殊处理）
- IntelliSense 应如何处理 `new()`？ （仅当存在单个目标类型时）
## <a name="design-meetings"></a>设计会议

- [LDM-2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
