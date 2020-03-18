---
ms.openlocfilehash: 2e4a3a32def6900797c151264c984378b09b4988
ms.sourcegitcommit: 5983461e05be62f39c77383cb7857539518cb04f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2019
ms.locfileid: "79484032"
---

# <a name="discriminated-unions--enum-class"></a>可区分联合/`enum class`

`enum class`es 是类型声明的一种新类型，有时称为可区分联合，其中每个可能的实例都列出了该类型，并且每个实例都是非重叠的。

使用以下语法定义 `enum class`：

```antlr
enum_class
    : 'partial'? 'enum class' identifier type_parameter_list? type_parameter_constraints_clause* 
      '{' enum_class_body '}'
    ;

enum_class_body
    : enum_class_cases?
    | enum_class_cases ','
    ;

enum_class_cases
    : enum_class_case
    | enum_class_case ',' enum_class_cases
    ;

enum_class_case
    : enum_class
    | class_declaration
    | identifier type_parameter_list? '(' formal_parameter_list? ')'
    | identifier
    ;

```

示例语法：

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius),
}
```

## <a name="semantics"></a>语义

`enum class` 定义定义一个根类型，该类型是一个与 `enum class` 声明同名的抽象类和一组成员，其中每个成员都有一个类型，该类型是根类型的子类型。 如果有多个 `partial enum class` 定义，则所有成员都将被视为 enum 类定义的成员。 与用户定义的抽象类定义不同，`enum class` 根类型在默认情况下为 partial，并定义为具有默认的*无参数的*构造函数。

请注意，由于根类型被定义为分部抽象类，因此还可以添加*根类型*的分部定义，其中允许使用类主体的标准语法形式。
但是，除了指定为 `enum class` 成员的类型之外，任何类型都不能直接从根类型继承。 此外，根类型不允许使用用户定义的构造函数。

有四种类型的 `enum class` 成员声明：

* 简单类成员

* 复杂类成员

* `enum class` 成员

* 值成员。

### <a name="simple-class-members"></a>简单类成员

简单的类成员声明定义了一个名为的新嵌套 "record" 类（此文档中特意不定义）。 嵌套类从根类型继承。

给定上面的示例代码，

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius)
}
```

`enum class` 声明的语义等效于以下声明

```C#
partial abstract class Shape
{
    public data class Rectangle(float Width, float Length) : Shape,
    public data class Circle(float Radius) : Shape
}
```

### <a name="complex-class-members"></a>复杂类成员

还可以将整个类声明嵌套在 `enum class` 声明的下面。 它将被视为根类型的嵌套类。 语法允许任何类声明，但复杂类成员需要从直接包含 `enum class` 声明中继承。 

### <a name="enum-class-members"></a>`enum class` 成员

`enum classes` 可以相互嵌套，例如

```C#
enum class Expr
{
    enum class Binary
    {
        Addition(Expr left, Expr right),
        Multiplication(Expr left, Expr right)
    }
}
```

这与顶级 `enum class`的语义几乎完全相同，不同之处在于：嵌套枚举类定义了嵌套根类型，而嵌套的 enum 类下面的所有内容都是嵌套根类型的子类型，而不是顶级根类型。

```C#
partial abstract class Expr
{
    partial abstract class Binary : Expr
    {
        public data class Addition(Expr left, Expr right) : Binary,
        public data class Multiplication(Expr left, Expr right) : Binary
    }
}
```

### <a name="value-members"></a>值成员

`enum classes` 还可以包含值成员。 值成员在根类型上定义同时返回根类型的公共只读静态属性，例如

```C#
enum class Color
{
    Red,
    Green
}
```

的属性等效于

```C#
partial abstract class Color
{
    public static Color Red => ...;
    public static Color Green => ...;
}
```

完整的语义被视为实现详细信息，但保证每个属性都返回一个唯一实例，并在重复调用时返回同一个实例。


### <a name="switch-expression-and-patterns"></a>Switch 表达式和模式

需要对模式匹配和用于处理 `enum classes`的 switch 表达式进行一些调整。 Switch 表达式可能已通过变量模式匹配类型，但对于引用类型，当前不会将 switch 表达式中的任何一组开关臂视为已完成，除非对参数的静态类型或子类型进行匹配。

Switch 表达式将进行更改，这种情况下，如果 `enum class` 的根类型是 switch 表达式的参数的静态类型，并且有一组模式与枚举的所有成员相匹配，则会将该开关视为详尽。

由于值成员不是常量，并且不定义新的静态类型，因此它们当前无法通过模式进行匹配。 为了实现这一点，将添加使用常量模式语法的新模式，以允许匹配 `enum class` 值成员。 当且仅当模式匹配的参数和 `enum class` 值成员返回的值引用相等时，才会将 match 定义为 "成功"，尽管实现不需要执行此检查。


## <a name="open-questions"></a>打开问题

- [] 通用类型算法对 `enum class` 成员说什么？ 这是有效的代码吗？
    * `var x = b ? new Shape.Rectangle(...) : new Shape.Circle(...)`

- [] 仅为值成员添加新模式似乎非常繁重。 是否有更通用的版本构造要有意义？
    - [] 值成员还不能很好地映射到并行嵌套类构造，因为这样

- [] 正在使用可保证为固定时间的 `enum class` 静态类型的参数进行切换？

- [] 在 switch 表达式中是否应将 `enum class`es 视为已完成？ `virtual`的前缀？

- [] 应允许哪些修饰符 `enum class`？