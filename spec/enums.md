---
ms.openlocfilehash: c3b716e6eb331be2ee33fffeb42c1e2406cd3a5c
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229560"
---
# <a name="enums"></a>枚举

***枚举类型***是一个非重复值类型 ([值类型](types.md#value-types))，其用于声明一系列已命名常数。

该示例

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

声明名为枚举类型`Color`与成员`Red`， `Green`，和`Blue`。

## <a name="enum-declarations"></a>枚举声明

枚举声明声明新的枚举类型。 枚举声明以关键字开头`enum`，并定义名称、 可访问性、 基础类型和枚举的成员。

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

每个枚举类型具有对应的整型类型称为***基础类型***的枚举类型。 此基础类型必须能够表示枚举中定义的所有枚举器值。 枚举声明可以显式声明的基础类型`byte`， `sbyte`， `short`， `ushort`， `int`， `uint`，`long`或`ulong`。 请注意，`char`不能用作的基础类型。 未显式声明基础类型的枚举声明都有的基础类型`int`。

该示例

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

声明枚举的基础类型`long`。 开发人员可以选择使用的基础类型`long`，如下所示的示例中，若要启用的范围内的值的用法`long`但不是在的范围内`int`，还是保留此选项可在将来。

## <a name="enum-modifiers"></a>枚举修饰符

*Enum_declaration*可以根据需要包含一系列枚举修饰符：

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

它是要在枚举声明中多次出现的相同修饰符的编译时错误。

枚举声明修饰符具有相同的含义的类声明 ([类修饰符](classes.md#class-modifiers))。 但请注意，该`abstract`和`sealed`枚举声明中不允许使用修饰符。 枚举不能是抽象的不允许派生。

## <a name="enum-members"></a>枚举成员

枚举类型声明的主体定义零个或多个枚举成员，它们是枚举类型的命名的常量。 没有两个枚举成员可以具有相同的名称。

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

每个枚举成员具有关联的常数值。 此值的类型是包含枚举的基础类型。 每个枚举成员的常量值必须是枚举的基础类型范围内。 该示例

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

导致编译时错误，因为常量值`-1`， `-2`，并`-3`不在的基础整型类型的范围内`uint`。

多个枚举成员可能会共享相同的关联的值。 该示例

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

显示枚举中的两个枚举成员--`Blue`和`Max`-具有相同的关联值。

隐式或显式分配的枚举成员关联的值。 如果枚举成员的声明具有*constant_expression*初始值设定项，隐式转换为枚举的基础类型的常量表达式的值是枚举成员的关联的值。 如果枚举成员的声明有没有初始值设定项，隐式，按如下所示设置其关联的值：

*  如果该枚举成员是枚举类型中声明的第一个枚举成员，其关联的值为零。
*  否则，枚举成员的关联的值被获取的原文前的枚举成员的关联的值加 1。 增加的该值必须在基础类型可以表示的值的范围内，否则会发生编译时错误。

该示例

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

将输出的枚举成员名称和其关联的值。 输出为：

```
Red = 0
Green = 10
Blue = 11
```

原因如下：

*  枚举成员`Red`自动分配值 0 （因为它没有初始值设定项，并且是第一个枚举成员）。
*  枚举成员`Green`显式提供了值`10`;
*  和枚举成员`Blue`自动分配的值比文本上位于前面的成员。

枚举成员的关联的值可能无法直接或间接使用其自身关联的枚举成员的值。 此循环除了限制外，其他的枚举成员初始值设定项，而不考虑它们的文本位置可以自由地引用枚举成员初始值设定项。 枚举成员初始值设定项，在其他枚举成员的值始终被当作具有其基础类型的类型，以便引用其他枚举成员时，可能不是必要的强制转换。

该示例

```csharp
enum Circular
{
    A = B,
    B
}
```

因为会导致编译时错误的声明`A`和`B`是循环的。 `A` 取决于`B`显式，并且`B`取决于`A`隐式。

名为枚举成员，作用域以完全等同于类中的字段的方式。 枚举成员的作用域是其包含的枚举类型的正文。 在该范围内，可以通过它们的简单名称称为枚举成员。 从所有其他代码，必须具有其枚举类型名称限定枚举成员的名称。 枚举成员不具有任何声明可访问性--枚举成员是可访问其包含的枚举类型是否可访问。

## <a name="the-systemenum-type"></a>System.Enum 类型

类型`System.Enum`是所有枚举类型 （这是不同，这不同于枚举类型的基础类型） 的抽象基类和成员继承自`System.Enum`任何枚举类型中可用。 装箱转换 ([装箱转换](types.md#boxing-conversions)) 存在从任何枚举类型设置为`System.Enum`，和取消装箱转换 ([取消装箱转换](types.md#unboxing-conversions)) 存在从`System.Enum`为任何枚举类型。

请注意，`System.Enum`本身不是*enum_type*。 相反，它是*class_type*所有*enum_type*s 派生。 类型`System.Enum`继承自类型`System.ValueType`([System.ValueType 类型](types.md#the-systemvaluetype-type))，而后者又继承自类型`object`。 在运行时类型的值`System.Enum`可以是`null`或对任何枚举类型的装箱值的引用。

## <a name="enum-values-and-operations"></a>枚举值和操作

每个枚举类型定义不同的类型;显式枚举转换 ([显式枚举转换](conversions.md#explicit-enumeration-conversions)) 所需的枚举类型和整型类型，之间或两个枚举类型之间转换。 枚举类型可以采用上的值的集不受其枚举成员。 具体而言，枚举的基础类型的任何值可强制转换为枚举类型，并为该枚举类型的不同有效值。

枚举的成员具有其包含的枚举类型的类型 (在其他枚举成员初始值设定项除外： 请参阅[枚举成员](enums.md#enum-members))。 枚举类型中声明的枚举成员值`E`相关联的值与`v`是`(E)v`。

以下运算符可以用于枚举类型的值： `==`， `!=`， `<`， `>`， `<=`， `>=`  ([枚举比较运算符](expressions.md#enumeration-comparison-operators))二进制`+`  ([加法运算符](expressions.md#addition-operator))，二进制`-`  ([减法运算符](expressions.md#subtraction-operator))， `^`， `&``|`  ([枚举逻辑运算符](expressions.md#enumeration-logical-operators))， `~`  ([按位求补运算符](expressions.md#bitwise-complement-operator))，`++`和`--` ([后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators)并[前缀递增和递减运算符](expressions.md#prefix-increment-and-decrement-operators))。

每个枚举类型自动派生自类`System.Enum`(其中，反过来，派生`System.ValueType`和`object`)。 因此，此类的继承的方法和属性可以使用枚举类型的值。
