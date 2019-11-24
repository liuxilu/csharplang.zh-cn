---
ms.openlocfilehash: 3b142d7dbda8a94a4cf2c973a2e380065dcbf5ee
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703970"
---
# <a name="enums"></a>枚举

***枚举类型***是声明一组命名常量的非重复值类型（[值类型](types.md#value-types)）。

示例

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

使用成员 `Red`、`Green`和 `Blue`声明名为 `Color` 的枚举类型。

## <a name="enum-declarations"></a>枚举声明

枚举声明声明一个新的枚举类型。 枚举声明以关键字 `enum`开头，并定义枚举的名称、可访问性、基础类型和成员。

```antlr
enum_declaration
    : attributes? enum_modifier* 'enum' identifier enum_base? enum_body ';'?
    ;

enum_base
    : ':' integral_type
    ;

enum_body
    : '{' enum_member_declarations? '}'
    | '{' enum_member_declarations ',' '}'
    ;
```

每个枚举类型都有一个对应的整型类型，称为枚举类型的***基础类型***。 此基础类型必须能够表示枚举中定义的所有枚举器值。 枚举声明可以显式声明基础类型的 `byte`、`sbyte`、`short`、`ushort`、`int`、`uint`、`long` 或 `ulong`。 请注意，`char` 不能用作基础类型。 不显式声明基础类型的枚举声明具有基础类型的 `int`。

示例

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

使用 `long`的基础类型声明枚举。 开发人员可以选择使用基础类型的 `long`（如本示例所示），以允许使用 `long` 范围内但不在 `int`范围内的值，或保留此选项以供将来使用。

## <a name="enum-modifiers"></a>枚举修饰符

*Enum_declaration*可以选择性地包含一系列枚举修饰符：

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

同一修饰符在一个枚举声明中多次出现是编译时错误。

枚举声明的修饰符与类声明（[类修饰符](classes.md#class-modifiers)）的修饰符具有相同的含义。 但请注意，在枚举声明中不允许使用 `abstract` 和 `sealed` 修饰符。 枚举不能是抽象的并且不允许派生。

## <a name="enum-members"></a>枚举成员

枚举类型声明的主体定义零个或多个枚举成员，这是枚举类型的命名常量。 两个枚举成员不能具有相同的名称。

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

每个枚举成员都有一个关联的常量值。 此值的类型是包含枚举的基础类型。 每个枚举成员的常数值必须在该枚举的基础类型的范围内。 示例

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

导致编译时错误，因为 `-1`、`-2`和 `-3` 的常量值不在基础整型类型 `uint`的范围内。

多个枚举成员可能共享相同的关联值。 示例

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

显示枚举，其中两个枚举成员--`Blue` 和 `Max` 具有相同的关联值。

枚举成员的关联值是隐式或显式分配的。 如果枚举成员的声明具有*constant_expression*初始值设定项，则该常量表达式的值将隐式转换为枚举的基础类型，这是枚举成员的关联值。 如果枚举成员的声明没有初始值设定项，则将隐式设置其关联值，如下所示：

*  如果枚举成员是枚举类型中声明的第一个枚举成员，则其关联值为零。
*  否则，枚举成员的关联值将通过将每个按日的枚举成员的关联值增加一个来获得。 这一增加的值必须在可由基础类型表示的值范围内，否则将发生编译时错误。

示例

```csharp
using System;

enum Color
{
    Red,
    Green = 10,
    Blue
}

class Test
{
    static void Main() {
        Console.WriteLine(StringFromColor(Color.Red));
        Console.WriteLine(StringFromColor(Color.Green));
        Console.WriteLine(StringFromColor(Color.Blue));
    }

    static string StringFromColor(Color c) {
        switch (c) {
            case Color.Red: 
                return String.Format("Red = {0}", (int) c);

            case Color.Green:
                return String.Format("Green = {0}", (int) c);

            case Color.Blue:
                return String.Format("Blue = {0}", (int) c);

            default:
                return "Invalid color";
        }
    }
}
```

输出枚举成员名称及其关联值。 输出为：

```console
Red = 0
Green = 10
Blue = 11
```

原因如下：

*  枚举成员 `Red` 会自动赋予零值（因为它没有初始值设定项，并且是第一个枚举成员）;
*  为枚举成员 `Green` 显式赋予 `10`值;
*  和枚举成员 `Blue` 会自动赋给一个值，其值大于在一个上按其进行的成员。

枚举成员的关联值不能直接或间接地使用其自己的关联枚举成员的值。 除了此循环限制之外，枚举成员初始值设定项可以随意引用其他枚举成员初始值设定项，而不考虑其文本位置。 在枚举成员初始值设定项中，其他枚举成员的值始终被视为具有其基础类型的类型，因此在引用其他枚举成员时不需要强制转换。

示例

```csharp
enum Circular
{
    A = B,
    B
}
```

导致编译时错误，因为 `A` 和 `B` 的声明是循环的。 `A` 显式依赖于 `B`，`B` 依赖于 `A` 隐式。

枚举成员的命名方式与类中的字段完全类似。 枚举成员的作用域是其包含的枚举类型的正文。 在该范围内，枚举成员可以通过其简单名称引用。 对于所有其他代码，枚举成员的名称必须用其枚举类型的名称进行限定。 枚举成员不具有任何已声明的可访问性-如果其包含枚举类型可访问，则枚举成员是可访问的。

## <a name="the-systemenum-type"></a>System.object 类型

类型 `System.Enum` 是所有枚举类型的抽象基类（这是截然不同的，不同于枚举类型的基础类型），并且从 `System.Enum` 继承的成员可用于任何枚举类型。 从任何枚举类型到 `System.Enum`都存在装箱转换（[装箱](types.md#boxing-conversions)转换），并且从 `System.Enum` 到任何枚举类型都存在取消装箱转换（[取消装箱](types.md#unboxing-conversions)转换）。

请注意，`System.Enum` 本身不是*enum_type*。 相反，它是派生所有*enum_type*的*class_type* 。 类型 `System.Enum` 从类型 `System.ValueType` （[系统类型](types.md#the-systemvaluetype-type)）继承而来，后者又继承自类型 `object`。 在运行时，可以 `null` 类型 `System.Enum` 的值或对任何枚举类型的装箱值的引用。

## <a name="enum-values-and-operations"></a>枚举值和运算

每个枚举类型都定义了一个不同的类型;若要在枚举类型和整型之间进行转换，或在两个枚举类型之间进行转换，必须使用显式枚举转换（[显式枚举转换](conversions.md#explicit-enumeration-conversions)）。 枚举类型可以采用的值集不受其枚举成员限制。 特别是，枚举的基础类型的任何值都可以转换为枚举类型，并且是该枚举类型的非重复有效值。

枚举成员具有其包含枚举类型的类型（在其他枚举成员初始值设定项中除外：请参见[枚举成员](enums.md#enum-members)）。 使用关联值 `v` 在枚举类型 `E` 中声明的枚举成员的值 `(E)v`。

以下运算符可用于枚举类型的值： `==`、`!=`、`<`、`>`、`<=`、`>=` （[枚举比较运算符](expressions.md#enumeration-comparison-operators)）、二进制 `+` （[加法运算符](expressions.md#addition-operator)[）、](expressions.md#subtraction-operator)`-`、 、`^``&`（[枚举逻辑运算符](expressions.md#enumeration-logical-operators)）、`|` （[按位求补运算符](expressions.md#bitwise-complement-operator)）、`~`和  `++` （[后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators)以及[前缀增量和减量运算符](expressions.md#prefix-increment-and-decrement-operators)）。`--` 

每个枚举类型都自动派生自类 `System.Enum` （反过来，派生自 `System.ValueType` 和 `object`）。 因此，此类的继承方法和属性可用于枚举类型的值。
