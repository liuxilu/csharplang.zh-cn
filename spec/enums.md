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

声明一个名为 `Color` 的枚举类型，成员 `Red`、`Green` 和 @no__t。

## <a name="enum-declarations"></a>枚举声明

枚举声明声明一个新的枚举类型。 枚举声明以关键字 @no__t 开始，并定义枚举的名称、可访问性、基础类型和成员。

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

每个枚举类型都有一个对应的整型类型，称为枚举类型的***基础类型***。 此基础类型必须能够表示枚举中定义的所有枚举器值。 枚举声明可以显式声明一 @no__t 的基础类型，`sbyte`，`short`，`ushort`，`int`，`uint`，`long` 或 @no__t 7。 请注意，不能将 `char` 用作基础类型。 不显式声明基础类型的枚举声明的基础类型为 `int`。

示例

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

声明基础类型为 `long` 的枚举。 开发人员可以选择使用 @no__t 的基础类型（如示例所示），以允许使用 @no__t 范围内的值，而不是在 `int` 范围内的值，或者为将来保留此选项。

## <a name="enum-modifiers"></a>枚举修饰符

*Enum_declaration*可以选择性地包含枚举修饰符的序列：

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

枚举声明的修饰符与类声明（[类修饰符](classes.md#class-modifiers)）的修饰符具有相同的含义。 但请注意，在枚举声明中不允许使用 @no__t 0 和 @no__t 修饰符。 枚举不能是抽象的并且不允许派生。

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

导致编译时错误，因为常量值 `-1`、`-2` 和 `-3` 不在基础整数类型 `uint` 的范围内。

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

显示一个枚举，其中两个枚举成员--`Blue` 和 @no__t--具有相同的关联值。

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

*  枚举成员 `Red` 会自动赋予值零（因为它没有初始值设定项，并且是第一个枚举成员）;
*  枚举成员 `Green` 显式给定值 `10`;
*  和枚举成员 `Blue` 会自动分配一个大于在该成员前面有一个的成员的值。

枚举成员的关联值不能直接或间接地使用其自己的关联枚举成员的值。 除了此循环限制之外，枚举成员初始值设定项可以随意引用其他枚举成员初始值设定项，而不考虑其文本位置。 在枚举成员初始值设定项中，其他枚举成员的值始终被视为具有其基础类型的类型，因此在引用其他枚举成员时不需要强制转换。

示例

```csharp
enum Circular
{
    A = B,
    B
}
```

导致编译时错误，因为 `A` 和 `B` 的声明是循环的。 `A` 显式依赖于 @no__t 1，`B` 隐式依赖于 @no__t。

枚举成员的命名方式与类中的字段完全类似。 枚举成员的作用域是其包含的枚举类型的正文。 在该范围内，枚举成员可以通过其简单名称引用。 对于所有其他代码，枚举成员的名称必须用其枚举类型的名称进行限定。 枚举成员不具有任何已声明的可访问性-如果其包含枚举类型可访问，则枚举成员是可访问的。

## <a name="the-systemenum-type"></a>System.object 类型

类型 `System.Enum` 是所有枚举类型的抽象基类（这与枚举类型的基础类型截然不同，不同于枚举类型的基础类型），而从 `System.Enum` 继承的成员可用于任何枚举类型。 从任何枚举类型到 `System.Enum` 的装箱转换（[装箱](types.md#boxing-conversions)转换），都存在从 @no__t 3 到任何枚举类型的取消装箱转换（[取消](types.md#unboxing-conversions)装箱转换）。

请注意，`System.Enum` 本身并不*为*。 相反，它是从其派生所有*为*的*class_type* 。 类型 @no__t 从类型 @no__t （[系统类型](types.md#the-systemvaluetype-type)）继承而来，后者又继承自类型 `object`。 在运行时，类型 `System.Enum` 的值可以 `null` 或对任何枚举类型的装箱值的引用。

## <a name="enum-values-and-operations"></a>枚举值和运算

每个枚举类型都定义了一个不同的类型;若要在枚举类型和整型之间进行转换，或在两个枚举类型之间进行转换，必须使用显式枚举转换（[显式枚举转换](conversions.md#explicit-enumeration-conversions)）。 枚举类型可以采用的值集不受其枚举成员限制。 特别是，枚举的基础类型的任何值都可以转换为枚举类型，并且是该枚举类型的非重复有效值。

枚举成员具有其包含枚举类型的类型（在其他枚举成员初始值设定项中除外：请参见[枚举成员](enums.md#enum-members)）。 在枚举类型 `E` 中声明的枚举成员的值（`v` 的关联值）为 `(E)v`。

以下运算符可用于枚举类型的值： `==`，`!=`，`<`，`>`，`<=`，`>=` @ no__t （[枚举比较运算符](expressions.md#enumeration-comparison-operators)），二进制 `+` @ No__t （[加法运算符](expressions.md#addition-operator)），二进制 1 @ no__t-12 （[减法运算符](expressions.md#subtraction-operator)），4，5，6 @ no__t-17 （[枚举逻辑运算符](expressions.md#enumeration-logical-operators)），9 @ no__t （[按位求补运算符](expressions.md#bitwise-complement-operator)），2，3 @ no__t （[后缀增量和减量运算符](expressions.md#postfix-increment-and-decrement-operators)以及[前缀增量和减量运算符](expressions.md#prefix-increment-and-decrement-operators)）。

每个枚举类型都自动派生自类 `System.Enum` （反过来，派生自 `System.ValueType` 和 `object`）。 因此，此类的继承方法和属性可用于枚举类型的值。
