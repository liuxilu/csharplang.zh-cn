---
ms.openlocfilehash: 38740069a2e105f920fa275c443f4560055e2901
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108360"
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

- **任何结构类型**（包括元组类型）
- **任何引用类型**（包括委托类型）
- 具有构造函数或 `struct` 约束的**任何类型参数**

但有以下例外：

- **枚举类型：** 并非所有枚举类型都包含常量零，因此应使用显式枚举成员。
- **接口类型：** 这是一项小型功能，应更好地显式提及该类型。
- **数组类型：** 数组需要一个特殊的语法来提供长度。
- **动态：** 我们不允许 `new dynamic()`，因此不允许 `new()` `dynamic` 作为目标类型。

还会排除*object_creation_expression*中不允许的所有其他类型，例如，指针类型。

如果目标类型是可以为 null 的值类型，则目标类型 `new` 将转换为基础类型而不是可以为 null 的类型。

> **打开问题：** 我们是否应允许委托和元组作为目标类型？

以上规则包括委托（引用类型）和元组（结构类型）。 尽管这两种类型都是可构造，但如果类型为 inferable，则可以使用匿名函数或元组文本。
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

### Miscellaneous

`throw new()` is disallowed.

Target-typed `new` is not allowed with binary operators.

It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...

It is also disallowed as a `ref`.

## Drawbacks
[drawbacks]: #drawbacks

There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.

## Alternatives
[alternatives]: #alternatives

Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.

## Questions
[questions]: #questions

- Should we forbid usages in expression trees? (no)
- How the feature interacts with `dynamic` arguments? (no special treatment)
- How IntelliSense should work with `new()`? (only when there is a single target-type)

## Design meetings

- [LDM-2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [LDM-2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM-2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM-2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM-2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
