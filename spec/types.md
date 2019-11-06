---
ms.openlocfilehash: 088c4a77cecde490c556c44c239a3496f896582e
ms.sourcegitcommit: 4ddf18d000734c1b6d0a48127bf338086fc3f2c3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2019
ms.locfileid: "73616143"
---
# <a name="types"></a>类型

C#语言的类型分为两个主要类别：***值类型***和***引用类型***。 值类型和引用类型都可以是采用一个或多个***类型参数***的***泛型类型***。 类型参数可以同时指定值类型和引用类型。

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

类型（指针）的最终类别仅在不安全代码中提供。 [指针类型](unsafe-code.md#pointer-types)中对此进行了进一步讨论。

值类型在值类型的变量中直接包含其数据时不同于引用类型，而引用类型的变量存储对其数据的***引用***，后者被称为***对象***。 对于引用类型，两个变量可以引用同一对象，因此，对一个变量执行的运算可能会影响另一个变量所引用的对象。 对于值类型，每个变量都有自己的数据副本，对一个变量执行的操作不可能影响另一个。

C#的类型系统是统一的，因此任何类型的值都可以被视为对象。 每种 C# 类型都直接或间接地派生自 `object` 类类型，而 `object` 是所有类型的最终基类。 只需将值视为类型 `object`，即可将引用类型的值视为对象。 通过执行装箱和取消装箱操作（[装箱和取消](types.md#boxing-and-unboxing)装箱），值类型的值被视为对象。

## <a name="value-types"></a>值类型

值类型是结构类型或枚举类型。 C#提供一组称为***简单类型***的预定义的结构类型。 简单类型通过保留字进行标识。

```antlr
value_type
    : struct_type
    | enum_type
    ;

struct_type
    : type_name
    | simple_type
    | nullable_type
    ;

simple_type
    : numeric_type
    | 'bool'
    ;

numeric_type
    : integral_type
    | floating_point_type
    | 'decimal'
    ;

integral_type
    : 'sbyte'
    | 'byte'
    | 'short'
    | 'ushort'
    | 'int'
    | 'uint'
    | 'long'
    | 'ulong'
    | 'char'
    ;

floating_point_type
    : 'float'
    | 'double'
    ;

nullable_type
    : non_nullable_value_type '?'
    ;

non_nullable_value_type
    : type
    ;

enum_type
    : type_name
    ;
```

与引用类型的变量不同，值类型的变量仅在值类型是可以为 null 的类型时才可以包含值 `null`。  对于每个不可为 null 的值类型，都有一个对应的可以为 null 的值类型，用于表示相同的一组值加上 `null`的值。

对值类型的变量赋值会创建要分配的值的副本。 这不同于对引用类型的变量的赋值，后者复制引用，而不是由引用标识的对象。

### <a name="the-systemvaluetype-type"></a>System.object 类型

所有值类型都隐式继承自类 `System.ValueType`，而该类又从类 `object`继承。 任何类型都不能从值类型派生，因此值类型将隐式密封（[密封类](classes.md#sealed-classes)）。

请注意，`System.ValueType` 本身并不*value_type*。 相反，它是一个*class_type* ，所有*value_type*都自动派生。

### <a name="default-constructors"></a>默认构造函数

所有值类型都隐式声明称为***默认构造函数***的公共无参数实例构造函数。 默认构造函数返回一个名为的零初始化实例，该实例称为 "值类型" 的***默认值***：

*  对于所有*simple_type*，默认值是由所有零的位模式生成的值：
    * 对于 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`和 `ulong`，默认值为 `0`。
    * 对于 `char`，默认值为 "`'\x0000'`"。
    * 对于 `float`，默认值为 "`0.0f`"。
    * 对于 `double`，默认值为 "`0.0d`"。
    * 对于 `decimal`，默认值为 "`0.0m`"。
    * 对于 `bool`，默认值为 "`false`"。
*  对于*为*`E`，默认值为 `0`，转换为类型 `E`。
*  对于*struct_type*，默认值是通过将所有值类型的字段设置为其默认值，并将所有引用类型字段设置为 `null`生成的值。
*  对于*nullable_type* ，默认值为 `HasValue` 属性为 false 且 `Value` 属性未定义的实例。 默认值也称为可以为 null 的类型的***null 值***。

与任何其他实例构造函数一样，值类型的默认构造函数使用 `new` 运算符进行调用。 出于效率方面的考虑，此要求实际上不会使实现生成构造函数调用。 在下面的示例中，变量 `i` 和 `j` 都初始化为零。

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

由于每个值类型都隐式具有一个公共的无参数实例构造函数，因此结构类型不可能包含无参数构造函数的显式声明。 但允许结构类型声明参数化实例构造函数（[构造函数](structs.md#constructors)）。

### <a name="struct-types"></a>结构类型

结构类型是一种值类型，可以声明常量、字段、方法、属性、索引器、运算符、实例构造函数、静态构造函数和嵌套类型。 结构[声明](structs.md#struct-declarations)中描述了结构类型的声明。

### <a name="simple-types"></a>简单类型

C#提供一组称为***简单类型***的预定义的结构类型。 简单类型通过保留字标识，但是这些保留字只是 `System` 命名空间中预定义的结构类型的别名，如下表中所述。


| __保留字__ | __别名类型__ |
|-------------------|------------------|
| `sbyte`           | `System.SByte`   | 
| `byte`            | `System.Byte`    | 
| `short`           | `System.Int16`   | 
| `ushort`          | `System.UInt16`  | 
| `int`             | `System.Int32`   | 
| `uint`            | `System.UInt32`  | 
| `long`            | `System.Int64`   | 
| `ulong`           | `System.UInt64`  | 
| `char`            | `System.Char`    | 
| `float`           | `System.Single`  | 
| `double`          | `System.Double`  | 
| `bool`            | `System.Boolean` | 
| `decimal`         | `System.Decimal` | 

因为简单类型是结构类型的别名，所以每个简单类型都具有成员。 例如，`int` 在 `System.Int32` 中声明了成员，并且从 `System.Object`继承了成员，并且允许以下语句：

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

简单类型不同于其他结构类型，简单类型允许某些附加操作：

*  大多数简单类型允许通过编写*文本*（[文本](lexical-structure.md#literals)）来创建值。 例如，`123` 是类型 `int` 的文字，`'a'` 是 `char`类型的文字。 C#通常不会对结构类型的文本进行任何设置，并且最终总是通过这些结构类型的实例构造函数创建其他结构类型的非默认值。
*  如果表达式的操作数都是简单类型常量，则编译器可能会在编译时计算表达式。 此类表达式称为*constant_expression* （[常量表达式](expressions.md#constant-expressions)）。 涉及其他结构类型定义的运算符的表达式不会被视为常量表达式。
*  通过 `const` 声明，可以声明简单类型（[常量](classes.md#constants)）的常量。 不能有其他结构类型的常量，但 `static readonly` 字段提供了类似的效果。
*  涉及简单类型的转换可以参与对由其他结构类型定义的转换运算符的计算，但用户定义的转换运算符永远不能参与对另一个用户定义的运算符的求值（[计算为用户定义的转换](conversions.md#evaluation-of-user-defined-conversions)）。

### <a name="integral-types"></a>整型

C#支持9整型类型： `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`和 `char`。 整数类型具有以下大小和值范围：

*  `sbyte` 类型表示有符号的8位整数，其值介于-128 和127之间。
*  `byte` 类型表示值介于0到255之间的无符号8位整数。
*  `short` 类型表示有符号的16位整数，其值介于-32768 和32767之间。
*  `ushort` 类型表示值介于0到65535之间的16位无符号整数。
*  `int` 类型表示有符号32位整数，其值介于-2147483648 和2147483647之间。
*  `uint` 类型表示无符号32位整数，其值介于0和4294967295之间。
*  `long` 类型表示有符号64位整数，其值介于-9223372036854775808 和9223372036854775807之间。
*  `ulong` 类型表示值介于0和18446744073709551615之间的未签名64位整数。
*  `char` 类型表示值介于0到65535之间的16位无符号整数。 `char` 类型的可能值集对应于 Unicode 字符集。 尽管 `char` 与 `ushort`具有相同的表示形式，但并不允许对一种类型允许的所有操作。

整型一元运算符和二元运算符始终操作有符号32位精度、无符号32位精度、有符号64位精度或无符号的64位精度：

*  对于一元 `+` 和 `~` 运算符，操作数转换为类型 `T`，其中 `T` 是 `int`、`uint`、`long`和 `ulong` 中的第一个，可以完全表示操作数的所有可能值。 然后，使用 `T`类型的精度执行运算，并 `T`结果的类型。
*  对于一元 `-` 运算符，操作数转换为类型 `T`，其中 `T` 是 `int` 和 `long` 中的第一个，可以完全表示操作数的所有可能值。 然后，使用 `T`类型的精度执行运算，并 `T`结果的类型。 一元 `-` 运算符不能应用于 `ulong`类型的操作数。
*  对于二进制 `+`，`-`、`*`、`/`、`%`、`&`、`^`、`|`、`==`、`!=`、`>`、`<`、`>=`和 `<=` 运算符，操作数将转换为类型 `T`，其中 `T` 是 `int`、`uint`、`long`和 `ulong` 中的第一个，可以完全表示这两个操作数的所有可能值。 然后，使用 `T`类型的精度执行该操作，结果的类型为 `T` （或关系运算符的 `bool`）。 不允许一个操作数的类型为 `long`，另一个操作数的类型 `ulong` 为二元运算符。
*  对于二进制 `<<` 和 `>>` 运算符，左操作数转换为类型 `T`，其中 `T` 是 `int`、`uint`、`long`和 `ulong` 中的第一个，可以完全表示操作数的所有可能值。 然后，使用 `T`类型的精度执行运算，并 `T`结果的类型。

`char` 类型分类为整型类型，但它在两个方面与其他整型类型不同：

*  无法将其他类型隐式转换为 `char` 类型。 具体而言，即使 `sbyte`、`byte`和 `ushort` 类型都具有使用 `char` 类型完全表示的值范围，从 `sbyte`、`byte`或 `ushort` 到 `char` 的隐式转换不存在。
*  必须将 `char` 类型的常量编写为*character_literal*s 或*integer_literal*s，并将强制转换为类型 `char`。 例如，`(char)10` 和 `'\x000A'` 相同。

`checked` 和 `unchecked` 运算符和语句用于控制整型算术运算和转换的溢出检查（[选中和未选中的运算符](expressions.md#the-checked-and-unchecked-operators)）。 在 `checked` 上下文中，溢出会生成编译时错误或引发 `System.OverflowException`。 在 `unchecked` 上下文中，将忽略溢出，并且不会丢弃任何不适合目标类型的高序位。

### <a name="floating-point-types"></a>浮点类型

C#支持两种浮点类型： `float` 和 `double`。 `float` 和 `double` 类型使用32位单精度和64位双精度 IEEE 754 格式表示，这些格式提供以下值集：

*  正零和负零。 在大多数情况下，正零和负零的行为与简单值零的行为相同，但某些运算区分这两个（[除法运算符](expressions.md#division-operator)）。
*  正无穷大和负无穷大。 此类操作会产生无穷大，将非零数除以零。 例如，`1.0 / 0.0` 产生正无穷，`-1.0 / 0.0` 产生负无穷。
*  ***非数字***值，通常缩写为 NaN。 Nan 由无效的浮点运算生成，如将零除以零。
*  `s * m * 2^e`形式的非零的有限值集，其中 `s` 为1或-1，`m` 和 `e` 由特定浮点类型确定：对于 `float`、`0 < m < 2^24` 和 `-149 <= e <= 104`对于 `double`，`0 < m < 2^53` 和 `-1075 <= e <= 970`。 非标准化浮点数被视为有效的非零值。

`float` 类型可以表示从大约 `1.5 * 10^-45` 到 `3.4 * 10^38` 精度为7位的值。

`double` 类型可以表示从大约 `5.0 * 10^-324` 到 `1.7 × 10^308` 精度为15-16 位的值。

如果二元运算符的一个操作数为浮点类型，则另一个操作数必须为整型类型或浮点类型，并且运算将按如下方式计算：

*  如果其中一个操作数为整型类型，则该操作数将转换为另一个操作数的浮点类型。
*  然后，如果任意一个操作数为 `double`类型，则另一个操作数将转换为 `double`，则使用至少 `double` 范围和精度执行操作，并且结果的类型为 `double` （或关系运算符的 `bool`）。
*  否则，使用至少 `float` 范围和精度执行操作，并且结果的类型为 `float` （或关系运算符的 `bool`）。

浮点运算符（包括赋值运算符）从不产生异常。 相反，在异常情况下，浮点运算产生零、无穷或 NaN，如下所述：

*  如果浮点运算的结果对于目标格式来说太小，则运算的结果将变为正零或负零。
*  如果浮点运算的结果对于目标格式来说太大，则运算的结果将变为正无穷或负无穷。
*  如果浮点运算无效，则运算的结果将变成 NaN。
*  如果浮点运算的一个或两个操作数为 NaN，则运算结果变成 NaN。

与运算的结果类型相比，浮点运算的精度可能更高。 例如，某些硬件体系结构支持比 `double` 类型更大的范围和精度的 "扩展" 或 "long double" 浮点类型，并使用这种更高的精度类型隐式执行所有浮点运算。 仅在性能较高的情况下，才可以使用较小的精度来执行浮点运算，而不需要实现使， C#而不是要求实现更高的精度类型用于所有浮点运算。 除了提供更精确的结果，这种情况很少会带来任何实实在在的影响。 但是，在形式 `x * y / z`的表达式中，乘法产生的结果不在 `double` 范围内，而后续除法会将临时结果返回到 `double` 范围中，这就是在中计算表达式的事实较大范围格式可能导致生成有限的结果而不是无穷。

### <a name="the-decimal-type"></a>Decimal 类型

`decimal` 类型是适用于财务和货币计算的 128 位数据类型。 `decimal` 类型可以表示从 `1.0 * 10^-28` 到大约 `7.9 * 10^28` 28-29 有效位的值。

`decimal` 类型的有限值集采用 `(-1)^s * c * 10^-e`形式，其中符号 `s` 是0或1，系数 `c` 由 `0 <= *c* < 2^96`给定，小数位数 `e` 为 `0 <= e <= 28`。`decimal` 类型不支持有符号的零、无穷大或 NaN。 `decimal` 表示为96位整数，由10的幂进行缩放。 对于绝对值小于 `1.0m`的 `decimal`，该值精确到28个小数位，但再也不是。 对于绝对值大于或等于 `1.0m``decimal`s，该值精确到28或29位。 与 `float` 和 `double` 数据类型相反，十进制小数值（如0.1）可精确表示 `decimal` 表示形式。 在 `float` 和 `double` 表示形式中，此类数字通常为无限小数，使这些表示形式更容易出现舍入错误。

如果二元运算符的一个操作数为 `decimal`类型，则另一个操作数必须为整型类型或 `decimal`类型。 如果存在整数类型操作数，则在执行操作之前，它将转换为 `decimal`。

对类型 `decimal` 的值执行运算的结果是：计算确切的结果（根据每个运算符的定义保留小数位数），然后舍入以适合表示形式。 结果将舍入为最接近的可表示值，并且，当结果同样接近两个可表示值时，为在最小有效位位置（称为 "银行家舍入"）中具有偶数的值。 如果结果为零，则其符号始终为0，小数位数为0。

如果十进制算术运算生成的值小于或等于绝对值 `5 * 10^-29`，则运算结果将变为零。 如果 `decimal` 算术运算生成的结果对 `decimal` 格式而言太大，则会引发 `System.OverflowException`。

`decimal` 类型具有比浮点类型更高的精度，但范围更小。 因此，从浮点类型到 `decimal` 的转换可能会产生溢出异常，而从 `decimal` 到浮点类型的转换可能会导致精度损失。 由于这些原因，浮点类型和 `decimal`之间不存在隐式转换，并且没有显式强制转换，因此不能在同一个表达式中混合使用浮点和 `decimal` 操作数。

### <a name="the-bool-type"></a>Bool 类型

`bool` 类型表示布尔的逻辑数量。 `bool` 类型的可能值 `true` 和 `false`。

`bool` 和其他类型之间不存在标准转换。 特别是，`bool` 类型是不同的，并且不同于整型，`bool` 并且不能用于替代整数值，反之亦然。

在 C 和C++语言中，零整数或浮点值，或者 null 指针可以转换为布尔值 `false`、非零整数或浮点值，或者可以将非 null 指针转换为布尔值 `true`。 在C#中，此类转换通过将整数或浮点值显式比较为零来实现，或通过将对象引用显式比较为 `null`来实现。

### <a name="enumeration-types"></a>枚举类型

枚举类型是具有已命名常数的不同类型。 每个枚举类型都有一个基础类型，该类型必须是 `byte`、`sbyte`、`short`、`ushort`、`int`、`uint`、`long` 或 `ulong`。 枚举类型的值集与基础类型的值集相同。 枚举类型的值不局限于命名常量的值。 枚举类型是通过枚举声明（[枚举声明](enums.md#enum-declarations)）定义的。

### <a name="nullable-types"></a>可以为 null 的类型

可以为 null 的类型可以表示其***基础类型***的所有值以及其他 null 值。 可以为 null 的类型写入 `T?`，其中 `T` 为基础类型。 此语法是 `System.Nullable<T>`的速记，这两种形式可互换使用。

与此相反，不***可以为 null 的值类型***为除 `System.Nullable<T>` 和其速记 `T?` 以外的任何值类型（适用于任何 `T`）以及任何被约束为不可为 null 的值类型的类型参数（即，具有 `struct` 的任何类型参数约束）。 `System.Nullable<T>` 类型为 `T` （[类型参数约束](classes.md#type-parameter-constraints)）指定值类型约束，这意味着可以为 null 的类型的基础类型可以是任何不可以为 null 的值类型。 可以为 null 的类型的基础类型不能是可以为 null 的类型或引用类型。 例如，`int??` 和 `string?` 是无效类型。

可以为 null 的类型的实例 `T?` 有两个公共只读属性：

*  `bool` 类型的 `HasValue` 属性
*  `T` 类型的 `Value` 属性

`HasValue` 为 true 的实例被称为非 null。 非 null 实例包含已知值，`Value` 返回该值。

`HasValue` 为 false 的实例称为 null。 空实例具有未定义的值。 尝试读取 null 实例的 `Value` 将导致引发 `System.InvalidOperationException`。 访问可以为 null 的实例的 `Value` 属性的过程称为***解包***。

除了默认构造函数，每个可以为 null 的类型 `T?` 都有一个公共构造函数，该构造函数采用 `T`类型的单个自变量。 给定 `T`类型 `x` 的值，该窗体的构造函数调用

```csharp
new T?(x)
```
创建 `Value` 属性 `x`的 `T?` 的非 null 实例。 为给定的值创建可为 null 的类型的非 null 实例的过程称为***包装***。

可从 `null` 文本使用隐式转换，以 `T?` （[Null 文本转换](conversions.md#null-literal-conversions)）和从 `T` 到 `T?` （[隐式可为 null 转换](conversions.md#implicit-nullable-conversions)）。

## <a name="reference-types"></a>引用类型

引用类型是类类型、接口类型、数组类型或委托类型。

```antlr
reference_type
    : class_type
    | interface_type
    | array_type
    | delegate_type
    ;

class_type
    : type_name
    | 'object'
    | 'dynamic'
    | 'string'
    ;

interface_type
    : type_name
    ;

array_type
    : non_array_type rank_specifier+
    ;

non_array_type
    : type
    ;

rank_specifier
    : '[' dim_separator* ']'
    ;

dim_separator
    : ','
    ;

delegate_type
    : type_name
    ;
```

引用类型值是对类型***实例***的引用，后者称为***对象***。 `null` 的特殊值与所有引用类型兼容，并指示缺少实例。

### <a name="class-types"></a>类类型

类类型定义一个数据结构，该结构包含数据成员（常量和字段）、函数成员（方法、属性、事件、索引器、运算符、实例构造函数、析构函数和静态构造函数）和嵌套类型。 类类型支持继承，这是一个派生类可以扩展和特殊化基类的机制。 类类型的实例是使用*object_creation_expression*s （[对象创建表达式](expressions.md#object-creation-expressions)）创建的。

类中对类类型进行了[说明。](classes.md)

某些预定义的C#类类型在语言中具有特殊含义，如下表中所述。


| __类类型__     | __描述__                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | 所有其他类型的最终基类。 请参阅[对象类型](types.md#the-object-type)。 | 
| `System.String`    | C#语言的字符串类型。 请参阅[字符串类型](types.md#the-string-type)。         |
| `System.ValueType` | 所有值类型的基类。 请参阅[system.object 类型](types.md#the-systemvaluetype-type)。          |
| `System.Enum`      | 所有枚举类型的基类。 请参阅[枚举](enums.md)。              |
| `System.Array`     | 所有数组类型的基类。 请参阅[数组](arrays.md)。             |
| `System.Delegate`  | 所有委托类型的基类。 请参阅[委托](delegates.md)。          |
| `System.Exception` | 所有异常类型的基类。 请参见[异常](exceptions.md)。         |

### <a name="the-object-type"></a>对象类型

`object` 类类型是所有其他类型的最终基类。 C#直接或间接派生自 `object` 类类型的每个类型。

关键字 `object` 只是预定义的类 `System.Object`的别名。

### <a name="the-dynamic-type"></a>动态类型

`dynamic` 类型（如 `object`）可引用任何对象。 将运算符应用于类型 `dynamic`的表达式时，其解决方法会推迟到程序运行。 因此，如果运算符无法合法地应用于被引用对象，则在编译过程中不会提供错误。 当运算符的解析在运行时失败时，将引发异常。

其目的是允许动态绑定，这在[动态绑定](expressions.md#dynamic-binding)中进行了详细说明。

`dynamic` 被视为等同于 `object`，但以下方面除外：

*  `dynamic` 类型的表达式的操作可以动态绑定（[动态绑定](expressions.md#dynamic-binding)）。
*  如果两者都是候选项，则类型推理（[类型推理](expressions.md#type-inference)）优先于 `object` `dynamic`。

由于这种等效性，以下内容包含：

*  `object` 和 `dynamic`之间存在隐式标识转换，在将 `dynamic` 替换为 `object`
*  与 `object` 的隐式和显式转换也适用于 `dynamic`。
*  将 `dynamic` 替换为 `object` 时相同的方法签名被视为相同的签名
*  类型 `dynamic` 在运行时与 `object` 不区分。
*  `dynamic` 类型的表达式称为***动态表达式***。

### <a name="the-string-type"></a>字符串类型

`string` 类型是直接从 `object`继承的密封类类型。 `string` 类的实例表示 Unicode 字符串。

`string` 类型的值可以编写为字符串文本（[字符串文字](lexical-structure.md#string-literals)）。

关键字 `string` 只是预定义的类 `System.String`的别名。

### <a name="interface-types"></a>接口类型

接口定义协定。 实现接口的类或结构必须遵循它的协定。 接口可以从多个基接口继承，类或结构可以实现多个接口。

接口类型在[接口](interfaces.md)中进行了介绍。

### <a name="array-types"></a>数组类型

数组是一种数据结构，其中包含零个或多个通过计算索引访问的变量。 数组中包含的变量（也称为数组的元素）都属于同一类型，并且此类型称为数组的元素类型。

数组中介绍了数组[类型。](arrays.md)

### <a name="delegate-types"></a>委托类型

委托是指一个或多个方法的数据结构。 对于实例方法，它还引用其相应的对象实例。

C 或C++中委托的最接近等效项是函数指针，但函数指针只能引用静态函数，而委托可以引用静态和实例方法。 在后一种情况下，委托不仅存储对方法入口点的引用，还存储对调用方法的对象实例的引用。

[委托中介绍](delegates.md)了委托类型。

## <a name="boxing-and-unboxing"></a>装箱和取消装箱

装箱和取消装箱的概念是类型系统C#的核心。 它通过允许将*value_type*的任何值相互转换为类型 `object`，在*value_type*s 和*reference_type*之间提供桥梁。 装箱和取消装箱可实现类型系统的统一视图，其中，任何类型的值最终都可以被视为对象。

### <a name="boxing-conversions"></a>装箱转换

装箱转换允许将*value_type*隐式转换为*reference_type*。 存在以下装箱转换：

*  从任何*value_type*到类型 `object`。
*  从任何*value_type*到类型 `System.ValueType`。
*  从任何*non_nullable_value_type*到*value_type*实现的任何*interface_type* 。
*  从任何*nullable_type*到由*nullable_type*的基础类型实现的任何*interface_type* 。
*  从任何*为*到类型 `System.Enum`。
*  从具有基础*为*的任何*nullable_type*到类型 `System.Enum`。
*  请注意，如果在运行时从值类型到引用类型的转换（[涉及类型参数的隐式转换](conversions.md#implicit-conversions-involving-type-parameters)）结束，则从类型参数进行的隐式转换将作为装箱转换执行。

将*non_nullable_value_type*的值装箱包括分配对象实例，并将*non_nullable_value_type*值复制到该实例中。

如果将*nullable_type*的值装箱为 `null` 值（`HasValue` 为 `false`）或解包和装箱基础值的结果，则会生成空引用。

将*non_nullable_value_type*的值装箱的实际过程最好通过应该构想泛型***装箱类***的存在进行说明，该类的行为类似于声明为以下形式：

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

`T` 类型的值 `v` 的装箱现在包含执行表达式 `new Box<T>(v)`，并将生成的实例作为类型 `object`的值返回。 因此，语句
```csharp
int i = 123;
object box = i;
```
概念上对应于
```csharp
int i = 123;
object box = new Box<int>(i);
```

上面的装箱类（如 `Box<T>`）实际上不存在，装箱值的动态类型实际上不是类类型。 相反，类型 `T` 的装箱值具有动态类型 `T`，使用 `is` 运算符的动态类型检查只需引用类型 `T`即可。 例如，应用于对象的
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
会在控制台上输出字符串 "`Box contains an int`"。

装箱转换表示生成装箱的值的副本。 这不同于*reference_type*到类型 `object`的转换，在这种情况下，该值会继续引用相同的实例，并且只被视为 `object`派生程度较小的类型。 例如，给定声明
```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
以下语句
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
会在控制台上输出值10，因为在 `p` 分配给 `box` 时发生的隐式装箱操作会导致复制 `p` 的值。 改为 `Point` 声明 `class`，因为 `p` 和 `box` 将引用同一个实例，所以值为20。

### <a name="unboxing-conversions"></a>取消装箱转换

取消装箱转换允许将*reference_type*显式转换为*value_type*。 存在以下取消装箱转换：

*  从类型 `object` 到任何*value_type*。
*  从类型 `System.ValueType` 到任何*value_type*。
*  从任何*interface_type*到任何实现*interface_type*的*non_nullable_value_type* 。
*  从任何*interface_type*到任何基础类型都实现*interface_type*的*nullable_type* 。
*  从类型 `System.Enum` 到任何*为*。
*  从类型 `System.Enum` 到具有基础*为*的任何*nullable_type* 。
*  请注意，如果在运行时结束将引用类型转换为值类型（[显式动态转换](conversions.md#explicit-dynamic-conversions)），则会将显式转换为类型参数，以取消装箱转换。

对*non_nullable_value_type*的取消装箱操作包括：首先检查对象实例是否是给定*non_nullable_value_type*的装箱值，然后将值复制到该实例之外。

如果 `null`的源操作数为 null，则为对*nullable_type*的取消装箱将生成*nullable_type*的 null 值; 否则，会将对象实例取消装箱的结果返回到*nullable_type*的基础类型。

使用上一节中所述的虚部装箱类，`box` 到*value_type* `T` 的对象的取消装箱转换包含执行表达式 `((Box<T>)box).value`。 因此，语句
```csharp
object box = 123;
int i = (int)box;
```
概念上对应于
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

若要在运行时成功转换到给定的*non_nullable_value_type* ，源操作数的值必须是该*non_nullable_value_type*的装箱值的引用。 如果 `null`源操作数，则会引发 `System.NullReferenceException`。 如果源操作数是对不兼容的对象的引用，则会引发 `System.InvalidCastException`。

若要在运行时成功转换到给定的*nullable_type* ，源操作数的值必须是 `null` 或对*nullable_type*基础*non_nullable_value_type*的装箱值的引用。 如果源操作数是对不兼容的对象的引用，则会引发 `System.InvalidCastException`。

## <a name="constructed-types"></a>构造类型

泛型类型声明本身表示***未绑定的泛型类型***，该类型用作 "蓝图" 以形成多种不同类型，方法是应用***类型参数***。 类型参数以紧跟在泛型类型名称后面的尖括号（`<` 和 `>`）来编写。 包含至少一个类型参数的类型称为***构造类型***。 构造类型可用于语言中可显示类型名称的大多数位置。 未绑定的泛型类型只能在*typeof_expression* （[typeof 运算符](expressions.md#the-typeof-operator)）中使用。

构造类型还可以在表达式中用作简单名称（[简单名称](expressions.md#simple-names)），也可以在访问成员（[成员访问](expressions.md#member-access)）时使用。

计算*namespace_or_type_name*时，只考虑具有正确数量的类型参数的泛型类型。 因此，只要类型具有不同数目的类型参数，就可以使用相同的标识符来识别不同的类型。 在同一程序中混合泛型和非泛型类时，这很有用：

```csharp
namespace Widgets
{
    class Queue {...}
    class Queue<TElement> {...}
}

namespace MyApplication
{
    using Widgets;

    class X
    {
        Queue q1;            // Non-generic Widgets.Queue
        Queue<int> q2;       // Generic Widgets.Queue
    }
}
```

*Type_name*可以标识构造的类型，即使它不直接指定类型参数也是如此。 如果类型嵌套在泛型类声明中，并且包含声明的实例类型隐式用于名称查找（[泛型类中的嵌套类型](classes.md#nested-types-in-generic-classes)），则可能会发生这种情况。

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

在不安全代码中，构造类型不能用作*unmanaged_type* （[指针类型](unsafe-code.md#pointer-types)）。

### <a name="type-arguments"></a>类型参数

类型实参列表中的每个实参只是一*种类型*。

```antlr
type_argument_list
    : '<' type_arguments '>'
    ;

type_arguments
    : type_argument (',' type_argument)*
    ;

type_argument
    : type
    ;
```

在不安全代码（[不安全](unsafe-code.md)代码）中， *type_argument*可能不是指针类型。 每个类型参数都必须满足相应类型参数的任何约束（[类型参数约束](classes.md#type-parameter-constraints)）。

### <a name="open-and-closed-types"></a>打开和关闭的类型

所有类型都可以分类为***开放式类型***或***关闭类型***。 开放式类型是一种涉及类型参数的类型。 更具体地说：

*  类型参数定义开放类型。
*  当且仅当数组的元素类型为开放类型时，该类型才是开放类型。
*  当且仅当一个或多个类型参数是开放类型时，构造类型是开放类型。 当且仅当其包含类型的一个或多个类型参数或其包含类型的类型参数是开放类型时，构造的嵌套类型是开放类型。

关闭的类型是不是开放类型的类型。

在运行时，泛型类型声明中的所有代码在通过将类型自变量应用于泛型声明而创建的封闭式构造类型的上下文中执行。 泛型类型中的每个类型形参都绑定到特定的运行时类型。 所有语句和表达式的运行时处理始终出现在关闭的类型中，并且打开的类型仅在编译时处理期间出现。

每个封闭式构造类型都有自己的静态变量集，它们不与任何其他封闭构造类型共享。 由于打开的类型在运行时不存在，因此没有与开放式类型关联的静态变量。 如果两个封闭式构造类型是从同一个未绑定的泛型类型构造的，则这两个封闭式构造类型都是相同的类型，并且其对应的类型参数是相同的类型

### <a name="bound-and-unbound-types"></a>绑定类型和未绑定类型

术语 "***未绑定类型***" 引用非泛型类型或未绑定的泛型类型。 术语 "***绑定类型***" 是指非泛型类型或构造类型。

未绑定类型引用由类型声明声明的实体。 未绑定的泛型类型本身不是类型，不能用作变量、参数或返回值的类型，也不能用作基类型。 无法引用未绑定的泛型类型的唯一构造是 `typeof` 表达式（[typeof 运算符](expressions.md#the-typeof-operator)）。

### <a name="satisfying-constraints"></a>满足约束

只要引用构造类型或泛型方法，就会对照对泛型类型或方法（[类型参数约束](classes.md#type-parameter-constraints)）声明的类型参数约束来检查提供的类型参数。 对于每个 `where` 子句，对照每个约束检查对应于命名类型参数 `A` 类型参数，如下所示：

*  如果约束是类类型、接口类型或类型参数，则让 `C` 用提供的类型参数替换约束中出现的任何类型参数的约束。 若要满足约束，必须为类型 `A` 可转换为以下类型之一 `C`：
    * 标识转换（[标识转换](conversions.md#identity-conversion)）
    * 隐式引用转换（[隐式引用](conversions.md#implicit-reference-conversions)转换）
    * 如果类型 A 是不可为 null 的值类型，则装箱转换（[装箱](conversions.md#boxing-conversions)转换）。
    * 从类型参数 `A` 到 `C`的隐式引用、装箱或类型参数转换。
*  如果约束是引用类型约束（`class`），则类型 `A` 必须满足以下条件之一：
    * `A` 是接口类型、类类型、委托类型或数组类型。 请注意，`System.ValueType` 和 `System.Enum` 是满足此约束的引用类型。
    * `A` 是已知为引用类型的类型参数（[类型参数约束](classes.md#type-parameter-constraints)）。
*  如果约束是值类型约束（`struct`），则类型 `A` 必须满足以下条件之一：
    * `A` 是结构类型或枚举类型，但不能是可以为 null 的类型。 请注意，`System.ValueType` 和 `System.Enum` 是不满足此约束的引用类型。
    * `A` 是具有值类型约束（[类型参数约束](classes.md#type-parameter-constraints)）的类型参数。
*  如果约束是 `new()`的构造函数约束，则类型 `A` 不得 `abstract`，并且必须具有公共的无参数构造函数。 如果满足以下条件之一，则满足此要求：
    * `A` 是一种值类型，因为所有值类型都具有公共的默认构造函数（[默认构造](types.md#default-constructors)函数）。
    * `A` 是具有构造函数约束的类型参数（[类型参数约束](classes.md#type-parameter-constraints)）。
    * `A` 是具有值类型约束（[类型参数约束](classes.md#type-parameter-constraints)）的类型参数。
    * `A` 是一种不 `abstract` 的类，包含不带参数的显式声明的 `public` 构造函数。
    * `A` 不 `abstract` 并且具有默认的构造函数（[默认构造函数](classes.md#default-constructors)）。

如果给定的类型参数不满足一个或多个类型参数的约束，则会发生编译时错误。

由于类型参数不是继承的，因此永远不会继承约束。 在下面的示例中，`D` 需要指定其类型参数上的约束 `T` 以便 `T` 满足由基类 `B<T>`强制的约束。 与此相反，类 `E` 无需指定约束，因为 `List<T>` 为任何 `T`实现 `IEnumerable`。

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a>类型参数

类型参数是一个标识符，它指定在运行时参数绑定到的值类型或引用类型。

```antlr
type_parameter
    : identifier
    ;
```

由于类型参数可以使用许多不同的实际类型参数进行实例化，因此类型参数与其他类型相比，操作和限制略有不同。 这些方法包括：

*  类型参数不能直接用于声明基类（[基类](classes.md#base-class)）或接口（[变量类型参数列表](interfaces.md#variant-type-parameter-lists)）。
*  对类型参数进行成员查找的规则取决于应用于该类型参数的约束（如果有）。 [成员查找](expressions.md#member-lookup)中对它们进行了详细介绍。
*  类型参数的可用转换取决于应用于该类型参数的约束（如果有）。 其中详细介绍了[涉及类型参数](conversions.md#implicit-conversions-involving-type-parameters)和[显式动态转换](conversions.md#explicit-dynamic-conversions)的隐式转换。
*  文本 `null` 无法转换为类型参数给定的类型，除非已知该类型参数是引用类型（[涉及类型参数的隐式转换](conversions.md#implicit-conversions-involving-type-parameters)）。 但是，可以改为使用 `default` 表达式（[默认值表达式](expressions.md#default-value-expressions)）。 此外，如果类型参数具有值类型约束，则可以使用 `==` 和 `!=` （[引用类型相等运算符](expressions.md#reference-type-equality-operators)）与 `null` 比较类型参数指定的类型的值。
*  如果类型参数受*constructor_constraint*或值类型约束（[类型参数约束](classes.md#type-parameter-constraints)）的约束，则只能将 `new` 表达式（[对象创建表达式](expressions.md#object-creation-expressions)）与类型参数一起使用。
*  类型参数不能在属性中的任何位置使用。
*  在成员访问（[成员访问](expressions.md#member-access)）或类型名称（[命名空间和类型名称](basic-concepts.md#namespace-and-type-names)）中不能使用类型参数来标识静态成员或嵌套类型。
*  在不安全代码中，类型参数不能用作*unmanaged_type* （[指针类型](unsafe-code.md#pointer-types)）。

类型参数只是一种编译时构造。 在运行时，每个类型参数都绑定到一个运行时类型，该类型是通过向泛型类型声明提供类型参数来指定的。 因此，使用类型参数声明的变量的类型将在运行时是封闭式构造类型（[开放和闭合类型](types.md#open-and-closed-types)）。 所有涉及类型参数的语句和表达式的运行时执行都使用作为该参数的类型参数提供的实际类型。

## <a name="expression-tree-types"></a>表达式树类型

***表达式树***允许将 lambda 表达式表示为数据结构而不是可执行代码。 表达式树是格式为 `System.Linq.Expressions.Expression<D>`的***表达式树类型***的值，其中 `D` 为任意委托类型。 对于本规范的其余部分，我们将使用速记 `Expression<D>`引用这些类型。

如果从 lambda 表达式到委托类型 `D`存在转换，则 `Expression<D>`的表达式树类型也存在转换。 尽管将 lambda 表达式转换为委托类型会生成一个委托，该委托引用 lambda 表达式的可执行代码，但转换为表达式树类型会创建 lambda 表达式的表达式树表示形式。

表达式树是 lambda 表达式的有效内存中数据表示形式，并使 lambda 表达式的结构透明和显式。

与委托类型 `D`一样，`Expression<D>` 称为具有参数和返回类型，它们与 `D`的类型相同。

下面的示例将 lambda 表达式表示为可执行代码，并表示为表达式树。 由于 `Func<int,int>`存在转换，因此 `Expression<Func<int,int>>`也存在转换：

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

按照这些分配，委托 `del` 引用返回 `x + 1`的方法，并且表达式树 `exp` 引用用于描述表达式 `x => x + 1`的数据结构。

在将 lambda 表达式转换为表达式树类型时，泛型类型 `Expression<D>` 的确切定义以及用于构造表达式树的准确规则都超出了此规范的范围。

要做出显式操作，需要注意以下两项：

*  并非所有 lambda 表达式都可以转换为表达式树。 例如，具有语句体的 lambda 表达式和包含赋值表达式的 lambda 表达式不能表示。 在这些情况下，转换仍存在，但会在编译时失败。 [匿名函数转换](conversions.md#anonymous-function-conversions)中详细介绍了这些异常。
*   `Expression<D>` 提供了一个实例方法 `Compile`，该方法生成类型 `D`的委托：

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    调用此委托将导致执行表达式树所表示的代码。 因此，根据上面的定义，del 和 del2 是等效的，以下两个语句将具有相同的效果：

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    执行此代码后，`i1` 和 `i2` 都具有 `2`的值。

