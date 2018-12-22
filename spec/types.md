# <a name="types"></a>类型

C# 语言的类型分为两个主要类别：***值类型***并***引用类型***。 值类型和引用类型可能***泛型类型***，这需要一个或多个***类型参数***。 类型参数可以指定这两种值类型和引用类型。

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

仅在不安全代码中提供的类型、 指针、 的最后一个类别。 对此进行讨论中进一步[指针类型](unsafe-code.md#pointer-types)。

值类型不同于引用类型，值类型的变量直接包含其数据，而变量引用的类型应用商店***引用***对其数据，后者称为***对象***. 对于引用类型，它是两个变量来引用同一对象，并因此可能会对一个变量以影响其他变量所引用的对象的操作。 对于值类型，每个变量都有自己的数据的副本并不是可能会影响另一个操作。

C# 类型系统被统一的这样可以任何类型的值视为对象。 每种 C# 类型都直接或间接地派生自 `object` 类类型，而 `object` 是所有类型的最终基类。 只需将值视为类型 `object`，即可将引用类型的值视为对象。 值类型的值作为对象处理通过执行装箱和取消装箱操作 ([装箱和取消装箱](types.md#boxing-and-unboxing))。

## <a name="value-types"></a>值类型

值类型是结构类型或枚举类型。 C# 提供了一组预定义的结构类型称为***简单类型***。 通过保留字，简单类型进行标识。

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

与引用类型的变量，不同的变量的值类型可以包含值`null`仅当值类型为 null 的类型。  对于每个不可为 null 的值类型没有相应的可以为 null 值类型表示一组相同的值以及值`null`。

值类型的变量进行赋值会创建一份所赋的值。 这不同于分配给变量的引用类型，复制引用而不是由引用标识的对象。

### <a name="the-systemvaluetype-type"></a>System.ValueType 类型

所有值类型隐式都继承自类`System.ValueType`，而后者又继承自类`object`。 不能为任何类型派生自值类型，并因此隐式密封的值类型 ([密封类](classes.md#sealed-classes))。

请注意，`System.ValueType`本身不是*value_type*。 相反，它是*class_type*所有*value_type*会自动派生 s。

### <a name="default-constructors"></a>默认构造函数

所有值类型隐式都声明公共无参数实例构造函数调用***默认构造函数***。 默认构造函数将返回零初始化的实例称为***默认值***对于值类型：

*  为所有*simple_type*s，默认值是生成全为零的位模式的值：
    * 有关`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`，并且`ulong`，默认值是`0`。
    * 有关`char`，默认值是`'\x0000'`。
    * 有关`float`，默认值是`0.0f`。
    * 有关`double`，默认值是`0.0d`。
    * 有关`decimal`，默认值是`0.0m`。
    * 有关`bool`，默认值是`false`。
*  有关*enum_type* `E`，默认值为`0`转换为的类型、 `E`。
*  有关*struct_type*，默认值是生成通过将所有值类型字段都设置为其默认值和所有引用类型字段的值`null`。
*  有关*nullable_type*的默认值是为其实例`HasValue`属性为 false 和`Value`未定义属性。 默认值是也称为***null 值***的为 null 的类型。

值类型的默认构造函数调用使用任何其他实例构造函数，如`new`运算符。 出于效率考虑，此要求不适用于实际具有生成的构造函数调用的实现。 在以下示例中，变量`i`和`j`均被初始化为零。

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

每个值类型隐式具有公共无参数实例构造函数，因为它不能包含无参数构造函数的显式声明为结构类型。 但是允许结构类型声明参数化的实例构造函数 ([构造函数](structs.md#constructors))。

### <a name="struct-types"></a>结构类型

结构类型是常量、 字段、 方法、 属性、 索引器、 运算符、 实例构造函数、 静态构造函数和嵌套的类型可以声明的值类型。 结构类型的声明中所述[结构声明](structs.md#struct-declarations)。

### <a name="simple-types"></a>简单类型

C# 提供了一组预定义的结构类型称为***简单类型***。 简单类型进行标识通过保留字，但这些保留的字是只需在预定义的结构类型的别名`System`命名空间下, 表中所述。


| __保留的字__ | __使用别名类型__ |
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

简单类型的结构类型别名，因为每个简单类型都具有成员。 例如，`int`具有中声明成员`System.Int32`和成员继承自`System.Object`，并允许使用以下语句：

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

简单类型不同于其他结构类型，简单类型允许某些附加操作：

*  大多数简单类型允许值，若要创建的编写*文字*([文本](lexical-structure.md#literals))。 例如，`123`是类型的文字`int`并`'a'`是类型的文本`char`。 C# 一般情况下，可以为文本的结构类型没有预配和其他结构类型的非默认值最终始终通过创建这些结构类型的实例构造函数。
*  所有简单类型常量表达式的操作数时，就可以为编译器在编译时表达式进行求值。 此类表达式被称为*constant_expression* ([常量表达式](expressions.md#constant-expressions))。 包含由其他结构类型定义运算符的表达式都不是常量表达式。
*  通过`const`声明就可以声明的简单类型的常量 ([常量](classes.md#constants))。 不能具有常量的其他结构类型，但未提供类似的效果`static readonly`字段。
*  涉及简单类型的转换可以参与的其他结构类型，所定义的转换运算符的计算，但用户定义的转换运算符可以永远不会参与的另一个用户定义运算符的计算 ([的评估用户定义的转换](conversions.md#evaluation-of-user-defined-conversions))。

### <a name="integral-types"></a>整型

C# 支持九个整型类型： `sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`，和`char`。 整型类型具有以下大小和范围的值：

*  `sbyte`类型表示有符号 8 位整数的值介于-128 到 127 之间。
*  `byte`类型表示其值介于 0 和 255 之间的 8 位无符号的整数。
*  `short`类型表示有符号 16 位整数值介于-32768 和 32767 之间。
*  `ushort`类型表示无符号的 16 位整数，其值介于 0 和 65535 之间。
*  `int`类型表示有符号 32 位整数值介于-2147483648 到 2147483647 之间。
*  `uint`类型表示值介于 0 到 4294967295 之间的 32 位无符号的整数。
*  `long`类型表示有符号 64 位整数的值介于-9223372036854775808 到 9223372036854775807 之间。
*  `ulong`类型表示值介于 0 到 18446744073709551615 之间的 64 位无符号的整数。
*  `char`类型表示无符号的 16 位整数，其值介于 0 和 65535 之间。 可能值的集`char`类型对应于 Unicode 字符集。 尽管`char`具有相同的表示形式`ushort`上的其他允许一种类型上允许的不是所有操作。

整型类型一元和二元运算符总是对有符号的 32 位精度、 无符号的 32 位精度，有符号的 64 位精度，或无符号的 64 位精度操作：

*  对于一元`+`并`~`运算符，操作数将转换为类型`T`，其中`T`是第一种`int`， `uint`， `long`，和`ulong`，可以充分表示所有可能的操作数的值。 然后使用类型的精度执行该操作`T`，并且结果的类型为`T`。
*  对于一元`-`运算符，操作数将转换为类型`T`，其中`T`是第一种`int`和`long`，可以完全表示操作数的所有可能的值。 然后使用类型的精度执行该操作`T`，并且结果的类型为`T`。 一元`-`运算符不能应用于类型的操作数`ulong`。
*  二进制文件`+`， `-`， `*`， `/`， `%`， `&`， `^`， `|`， `==`， `!=`， `>`， `<`， `>=`，并`<=`运算符，操作数转换为类型`T`，其中`T`是第一种`int`， `uint`， `long`，和`ulong`，可以完全表示所有可能这两个操作数的值。 然后使用类型的精度执行该操作`T`，并且结果的类型为`T`(或`bool`对于关系运算符)。 它不允许的一个操作数的类型是`long`和其他类型`ulong`使用二元运算符。
*  二进制文件`<<`并`>>`运算符的左操作数将转换为类型`T`，其中`T`是第一种`int`， `uint`， `long`，和`ulong`，可以充分表示所有可能的操作数的值。 然后使用类型的精度执行该操作`T`，并且结果的类型为`T`。

`char`类型分类为一种整型类型，但它不同于其他整数类型以两种方式：

*  不存在隐式转换为其他类型`char`类型。 具体而言，即使`sbyte`， `byte`，并`ushort`类型具有的是完全可表示使用的值范围`char`类型，隐式转换从`sbyte`， `byte`，或`ushort`到`char`不存在。
*  常数`char`类型必须写为*character_literal*s 或被*integer_literal*结合强制转换为类型 s `char`。 例如，`(char)10` 和 `'\x000A'` 相同。

`checked`并`unchecked`运算符和语句用于控制溢出检查的整型类型算术运算和转换 ([checked 和 unchecked 运算符](expressions.md#the-checked-and-unchecked-operators))。 在中`checked`上下文，溢出会生成编译时错误，或导致`System.OverflowException`引发。 在`unchecked`上下文中，将忽略溢出并不适合目标类型中的任何高序位将被丢弃。

### <a name="floating-point-types"></a>浮点类型

C# 支持两个浮点类型：`float`和`double`。 `float`和`double`类型都使用为 32 位单精度和 64 位双精度 IEEE 754 格式提供下列值集来表示：

*  正零和负零。 在大多数情况下，正零和负零具有相同行为简单的值为零，但某些操作区分两个 ([除法运算符](expressions.md#division-operator))。
*  正无穷大和负无穷大。 无穷大会生成被零除一个非零数字作为此类操作。 例如，`1.0 / 0.0`产生正无穷和`-1.0 / 0.0`产生负无穷大。
*  ***Not 数字***值，通常缩写的 NaN。 无效的浮点运算，如零被零除会生成 Nan。
*  一组有限的窗体的值为非零`s * m * 2^e`，其中`s`为 1 或-1，并`m`和`e`取决于特定的浮点类型：有关`float`，`0 < m < 2^24`并`-149 <= e <= 104`，并为`double`，`0 < m < 2^53`和`1075 <= e <= 970`。 非标准化浮点数被视为有效的非零值。

`float`类型可以表示其值范围从大约`1.5 * 10^-45`到`3.4 * 10^38`且精度为 7 位数字。

`double`类型可以表示其值范围从大约`5.0 * 10^-324`到`1.7 × 10^308`且精度为 15 到 16 位。

如果浮点类型的二元运算符的操作数之一，然后另一个操作数必须是整型或浮点型，并且运算，如下所示：

*  如果其中一个操作数的整数类型，则该操作数转换为另一个操作数的浮点类型。
*  然后，如果任一操作数的类型`double`，另一个操作数转换为`double`，使用至少执行该操作`double`范围和精度和结果的类型是`double`(或`bool`为关系运算符）。
*  否则，该操作将执行使用至少`float`范围和精度和结果的类型是`float`(或`bool`对于关系运算符)。

浮点运算符，包括赋值运算符，永远不会生成异常。 相反，在异常情况下，浮点运算生成零个、 无穷大或 NaN，如下所述：

*  如果目标格式太小浮点运算的结果，操作的结果将成为正零或负零。
*  如果浮点运算的结果太大，目标格式，该操作的结果将成为正无穷或负无穷大。
*  如果浮点运算无效，操作的结果将成为 NaN。
*  如果浮点运算的一个或两个操作数为 NaN，操作的结果将成为 NaN。

可能会执行该操作的结果类型比更高精度的浮点运算。 例如，某些硬件体系结构支持"扩展"或"长双精度"浮点类型，具有更大的范围和精度比`double`类型，并且隐式地执行所有使用此较高的精度类型的浮点运算。 仅在性能开销过多此类硬件体系结构可用于执行在精度较低，浮点运算，而不是需要实现只好性能和精度，C# 允许更高的精度类型为用于所有浮点运算。 不提供更精确的结果，这很少会有任何显著的影响。 但是，在窗体的表达式`x * y / z`，其中该乘法运算生成结果超出`double`范围，但后面的除法使临时结果返回到`double`范围，该表达式是这一事实计算在更高版本范围格式可能会导致有限生成而不是无限的结果。

### <a name="the-decimal-type"></a>十进制类型

`decimal` 类型是适用于财务和货币计算的 128 位数据类型。 `decimal`类型可以表示值介于`1.0 * 10^-28`到大约`7.9 * 10^28`具有 28-29 个有效数字。

一组有限的类型的值`decimal`的形式`(-1)^s * c * 10^-e`，其中符号`s`为 0 或 1，决定系数`c`由给定`0 <= *c* < 2^96`，和规模`e`是以便`0 <= e <= 28`。`decimal`类型不支持签名的零、 无穷大或 NaN 的。 一个`decimal`表示为 10 的幂缩放有 96 位整数。 有关`decimal`s，使用绝对值小于`1.0m`，值为精确到 28 位小数，但不要再。 有关`decimal`使用绝对值大于或等于 s `1.0m`，值为精确到 28 或 29 位数。 到 contrary`float`并`double`数据类型，如 0.1 十进制小数数字可表示严格按照`decimal`表示形式。 在中`float`和`double`表示形式，此类数字都是通常无限小，这使得更容易以轮关闭这些表示形式的错误。

如果其中一个二元运算符的操作数是类型`decimal`，则另一个操作数必须是一种整型类型或类型`decimal`。 如果存在整型操作数，则它将转换为`decimal`执行此操作之前。

类型的值的运算的结果`decimal`是这会导致从计算确切的结果 （保留缩放、 按每个运算符的定义），然后舍入以适合表示形式。 结果将舍入为最近的可表示的值，以及当结果同样接近两个可表示值，以最低有效位数位置 （这名为"银行家的舍入"） 中具有偶数数目的值。 零个结果始终具有 0 的符号和小数位数为 0。

如果十进制算术运算产生的值小于或等于`5 * 10^-29`中绝对值的数值，该操作的结果则为零。 如果`decimal`算术运算产生的结果太大`decimal`格式，`System.OverflowException`引发。

`decimal`类型具有更高的精度比浮点类型的较小范围。 因此，对浮点类型转换`decimal`可能导致溢出异常，以及从转换`decimal`到浮点类型可能会导致丢失精度。 出于这些原因，隐式之间不存在转换浮点类型和`decimal`，而无需显式强制转换，则可以混合使用浮点和`decimal`在同一表达式中的操作数。

### <a name="the-bool-type"></a>Bool 类型

`bool`类型表示布尔逻辑的数量。 可能的值的类型`bool`都`true`和`false`。

标准之间不存在转换`bool`和其他类型。 具体而言，`bool`类型是不同，独立于整型类型和一个`bool`值不能代替整数值，反之亦然。

在 C 和 c + + 语言中，零整型或浮点值或 null 指针可以转换为布尔值`false`，和一个非零的整数或浮点值或非 null 指针可以转换为布尔值`true`。 在 C# 中，都完成此类转换，通过显式地将整数或浮点数值为零，或通过显式比较的对象引用`null`。

### <a name="enumeration-types"></a>枚举类型

枚举类型是具有已命名常数的不同类型。 每个枚举类型有一个基础类型，它必须是`byte`， `sbyte`， `short`， `ushort`， `int`， `uint`，`long`或`ulong`。 枚举类型的值集是相同的基础类型的值集。 枚举类型的值不限于已命名常数的值。 枚举类型定义通过枚举声明 ([枚举声明](enums.md#enum-declarations))。

### <a name="nullable-types"></a>可以为 null 的类型

可以为 null 的类型可以表示的所有值及其***基础类型***加上其他的 null 值。 可以为 null 的类型被写`T?`，其中`T`是基础类型。 此语法是简写`System.Nullable<T>`，并且可以互换使用两种形式。

一个***不可以为 null 的值类型***相反而不是任意值类型`System.Nullable<T>`和其简写`T?`(对于任何`T`)，以及任何类型参数约束为不可为 null 的值类型 （即，任何类型参数与`struct`约束)。 `System.Nullable<T>`类型指定的值类型约束`T`([类型参数约束](classes.md#type-parameter-constraints))，这意味着可以为 null 的类型的基础类型可以是任何非 null 的值类型。 可以为 null 的类型或引用类型，不能为 null 的类型的基础类型。 例如，`int??`和`string?`是无效的类型。

可以为 null 的类型的实例`T?`具有两个公共只读属性：

*  一个`HasValue`类型的属性 `bool`
*  一个`Value`类型的属性 `T`

为其实例`HasValue`是 true 则称其为非 null。 非 null 实例包含已知的值和`Value`返回该值。

为其实例`HasValue`是 false 则称其为 null。 空实例有未定义的值。 尝试读取`Value`的空实例会导致`System.InvalidOperationException`引发。 访问的过程`Value`可以为 null 实例的属性被称为***解包***。

除了默认构造函数中，每个可以为 null 的类型`T?`具有一个自变量的类型的公共构造函数`T`。 赋值`x`类型的`T`，窗体的构造函数调用

```csharp
new T?(x)
```
创建的非 null 实例`T?`为其`Value`属性是`x`。 对于给定的值被称为创建可以为 null 的类型的非 null 实例的过程***包装***。

隐式转换都可通过从`null`文字`T?`([Null 文字转换](conversions.md#null-literal-conversions)) 和从`T`到`T?`([可以为 null 的隐式转换](conversions.md#implicit-nullable-conversions)).

## <a name="reference-types"></a>引用类型

引用类型是类类型、 接口类型、 数组类型或委托类型。

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

引用类型值是指***实例***的类型，后者称为***对象***。 特殊值`null`与所有引用类型兼容，并表示实例不存在。

### <a name="class-types"></a>类类型

类类型定义包含数据成员 （常量和字段），函数成员 （方法、 属性、 事件、 索引器、 运算符、 实例构造函数、 析构函数和静态构造函数） 和嵌套的类型的数据结构。 类类型支持继承，因此派生的类可以扩展和专门针对基类的机制。 使用创建类类型的实例*object_creation_expression*s ([对象创建表达式](expressions.md#object-creation-expressions))。

中介绍了类类型[类](classes.md)。

某些预定义的类类型具有特殊含义，在 C# 语言中下, 表中所述。


| __类类型__     | __说明__                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | 所有其他类型的最终基类。 请参阅[的对象类型](types.md#the-object-type)。 | 
| `System.String`    | C# 语言的字符串类型。 请参阅[字符串类型](types.md#the-string-type)。         |
| `System.ValueType` | 所有值类型的基类。 请参阅[System.ValueType 类型](types.md#the-systemvaluetype-type)。          |
| `System.Enum`      | 所有枚举类型的基类。 请参阅[枚举](enums.md)。              |
| `System.Array`     | 所有数组类型的基类。 请参阅[数组](arrays.md)。             |
| `System.Delegate`  | 所有委托类型的基类。 请参阅[委托](delegates.md)。          |
| `System.Exception` | 所有异常类型的基类。 请参阅[异常](exceptions.md)。         |

### <a name="the-object-type"></a>对象类型

`object`类类型是所有其他类型的最基本基类。 每个类型在 C# 中的直接或间接派生`object`类类型。

关键字`object`是只需为预定义的类的别名`System.Object`。

### <a name="the-dynamic-type"></a>动态类型

`dynamic`类型，如`object`，可以引用任何其他对象。 当将运算符应用于类型的表达式`dynamic`，其解决推迟到运行该程序。 因此，如果合法不能将运算符应用于所引用对象，会在编译期间发出错误信息。 而是运算符的解析在运行时失败时，将引发异常。

其目的是允许动态绑定，在详细信息中所述[动态绑定](expressions.md#dynamic-binding)。

`dynamic` 被视为等于`object`在以下方面除外：

*  类型的表达式上的操作`dynamic`可以为动态绑定 ([动态绑定](expressions.md#dynamic-binding))。
*  类型推理 ([类型推理](expressions.md#type-inference)) 会更喜欢`dynamic`转移`object`如果两者都是候选项。

由于此等效性，以下包含：

*  没有之间的隐式标识转换`object`并`dynamic`，并替换时是相同的构造类型之间`dynamic`与 `object`
*  之间的隐式和显式转换`object`还将应用与`dynamic`。
*  替换时是相同的方法签名`dynamic`与`object`被视为相同的签名
*  类型`dynamic`相区别`object`在运行时。
*  类型的表达式`dynamic`被称为***动态表达式***。

### <a name="the-string-type"></a>字符串类型

`string`类型是密封的类类型直接继承`object`。 实例`string`类表示 Unicode 字符字符串。

值`string`类型可以写为字符串文本 ([的字符串文本](lexical-structure.md#string-literals))。

关键字`string`是只需为预定义的类的别名`System.String`。

### <a name="interface-types"></a>接口类型

接口定义一个协定。 类或结构实现的接口必须遵守其协定。 一个接口可能从多个基接口继承，类或结构可以实现多个接口。

接口类型所述[接口](interfaces.md)。

### <a name="array-types"></a>数组类型

数组是包含零个或多个可通过计算索引访问的变量的数据结构。 一个数组，也称为数组的元素中包含的变量包括所有相同的类型，而这种类型称为数组的元素类型。

数组类型中所述[数组](arrays.md)。

### <a name="delegate-types"></a>委托类型

委托是指一个或多个方法的数据结构。 对于实例方法，它还指其对应的对象实例。

最接近的 C 或 c + + 中的委托是函数指针，但函数指针只能引用静态函数，而委托可以引用静态和实例方法。 在后一种情况下，委托不仅存储存储的引用方法的入口点，还对在其上调用该方法的对象实例的引用。

中介绍了委托类型[委托](delegates.md)。

## <a name="boxing-and-unboxing"></a>装箱和取消装箱

装箱和取消装箱的概念是 C# 类型系统的核心。 它提供了之间的桥梁*value_type*s 和*reference_type*允许存在的任何值的 s *value_type*要转换到和从类型`object`。 装箱和取消装箱使类型系统，其中任何类型的值最终都可以作为对象处理的统一的视图。

### <a name="boxing-conversions"></a>装箱转换

装箱转换允许*value_type*隐式转换为*reference_type*。 存在以下装箱转换：

*  从任何*value_type*为类型`object`。
*  从任何*value_type*为类型`System.ValueType`。
*  从任何*non_nullable_value_type*任何*interface_type*由实现*value_type*。
*  从任何*nullable_type*任何*interface_type*实现的基础类型的*nullable_type*。
*  从任何*enum_type*为类型`System.Enum`。
*  从任何*nullable_type*与基础*enum_type*为类型`System.Enum`。
*  请注意从类型参数的隐式转换将执行作为装箱转换中，是否在运行时其结果是从值类型转换为引用类型 ([涉及类型参数的隐式转换](conversions.md#implicit-conversions-involving-type-parameters))。

装箱的值*non_nullable_value_type*分配给对象实例和复制组成*non_nullable_value_type*到该实例的值。

装箱的值*nullable_type*生成一个空引用，它是否`null`值 (`HasValue`是`false`)，或解包和否则装箱的基础值的结果。

装箱的值的实际过程*non_nullable_value_type*很好地解释由专门的泛型是否存在***装箱类***，其行为就像它已声明，如下所示：

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

值的装箱`v`类型的`T`现在包含执行表达式`new Box<T>(v)`，并返回生成的实例类型的值作为`object`。 因此，下面的语句
```csharp
int i = 123;
object box = i;
```
从概念上讲对应于
```csharp
int i = 123;
object box = new Box<int>(i);
```

装箱类，如`Box<T>`更高版本并不实际存在和装箱值的动态类型不是实际的类类型。 相反，类型的装箱的值`T`具有动态类型`T`，和动态类型检查使用`is`运算符只是引用类型`T`。 例如，应用于对象的
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
将输出字符串"`Box contains an int`"控制台上。

装箱转换隐含着一份待装箱的值。 这与不同的转换*reference_type*键入`object`，将继续引用同一个实例以及仅被视为派生程度更小的类型值中`object`。 例如，给定声明
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
下面的语句
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
将输出在控制台上的值 10，因为发生的分配中的隐式装箱操作`p`到`box`的值将导致`p`要复制。 有`Point`已声明`class`相反，值 20 会是输出，因为`p`和`box`将引用同一个实例。

### <a name="unboxing-conversions"></a>取消装箱转换

取消装箱转换允许*reference_type*显式转换为*value_type*。 存在以下取消装箱转换：

*  从类型`object`到任何*value_type*。
*  从类型`System.ValueType`到任何*value_type*。
*  从任何*interface_type*任何*non_nullable_value_type*实现*interface_type*。
*  从任何*interface_type*任何*nullable_type*其基础类型实现*interface_type*。
*  从类型`System.Enum`到任何*enum_type*。
*  从类型`System.Enum`到任何*nullable_type*与基础*enum_type*。
*  请注意类型参数的显式转换将执行作为取消装箱转换中，是否在运行时其结果是从引用类型转换为值类型 ([显式动态转换](conversions.md#explicit-dynamic-conversions))。

取消装箱操作向*non_nullable_value_type*组成的对象实例的装箱的值的第一个检查给定*non_nullable_value_type*，然后复制出的值实例。

到取消装箱*nullable_type*生成的 null 值*nullable_type*如果源操作数为`null`，或取消装箱的基础类型的对象实例的已包装的结果*nullable_type*否则为。

引用在上一部分中，取消装箱转换的对象中所述的虚部装箱类`box`到*value_type* `T`组成执行表达式`((Box<T>)box).value`。 因此，下面的语句
```csharp
object box = 123;
int i = (int)box;
```
从概念上讲对应于
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

取消装箱转换为给定*non_nullable_value_type*若要成功运行时，源操作数的值必须是对已装箱值，该值的引用*non_nullable_value_type*。 如果源操作数`null`、`System.NullReferenceException`引发。 如果源操作数是一个不兼容的对象的引用`System.InvalidCastException`引发。

取消装箱转换为给定*nullable_type*若要成功运行时，源操作数的值必须是`null`或对基础的装箱值的引用*non_nullable_value_type*的*nullable_type*。 如果源操作数是一个不兼容的对象的引用`System.InvalidCastException`引发。

## <a name="constructed-types"></a>构造的类型

泛型类型声明，其本身而言，表示***未绑定的泛型类型***用作"蓝图"以形成许多不同类型，通过应用***类型参数***。 类型实参编写在尖括号 (`<`和`>`) 紧跟泛型类型的名称。 包含至少一个类型参数的类型称为***构造类型***。 可在类型名称可以在其中显示的语言中的大多数位置构造的类型。 未绑定的泛型类型仅可在*typeof_expression* ([typeof 运算符](expressions.md#the-typeof-operator))。

构造的类型还可在表达式中作为简单名称 ([的简单名称](expressions.md#simple-names)) 或访问成员时 ([成员访问](expressions.md#member-access))。

当*namespace_or_type_name*是使用正确的类型参数被视为数计算，仅泛型类型。 因此，它是可以使用相同的标识符来标识不同类型，只要类型具有不同数量的类型参数。 混合在同一程序中的泛型和非泛型类时，这非常有用：

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

一个*type_name*可能会标识构造的类型，即使它不直接指定类型参数。 这可能是其中一种类型嵌套在泛型类声明中，并且包含声明的实例类型隐式用于名称查找 ([嵌套在泛型类类型](classes.md#nested-types-in-generic-classes)):

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

不安全代码中构造的类型不能用作*unmanaged_type* ([指针类型](unsafe-code.md#pointer-types))。

### <a name="type-arguments"></a>类型参数

类型实参列表中的每个参数是只需*类型*。

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

在不安全代码 ([不安全代码](unsafe-code.md))、 一个*type_argument*可能不是指针类型。 每个类型实参必须满足在相应的所有约束都类型参数 ([都类型参数约束](classes.md#type-parameter-constraints))。

### <a name="open-and-closed-types"></a>打开和关闭类型

可以为所有类型进行都分类***开放类型***或***封闭类型***。 开放类型是涉及类型参数的类型。 具体而言：

*  类型参数定义开放类型。
*  数组类型是开放类型，当且仅当其元素类型是开放类型。
*  构造的类型是开放类型，当且仅当一个或多个其类型参数是开放类型。 当且仅当一个或多个其类型参数或其包含类型的类型参数是开放类型，构造的嵌套的类型是开放类型。

关闭的类型是不是开放类型的类型。

在运行时，所有的泛型类型声明中的代码是通过将类型参数应用于泛型声明创建封闭式构造类型的上下文中执行。 泛型类型中的每个类型参数是绑定到特定的运行时类型。 运行时处理的所有语句和表达式始终是使用封闭类型，并打开类型仅出现在编译时处理。

每个封闭的构造的类型都有自己的静态变量，不与任何其他封闭式构造类型共享的一组。 在运行时，打开类型不存在，因为没有静态变量与开放类型相关联。 如果它们从同一个未绑定泛型类型，构造和其相应的类型参数具有相同的类型，则两个封闭式构造的类型具有相同的类型。

### <a name="bound-and-unbound-types"></a>绑定和未绑定类型

术语***未绑定类型***指非泛型类型或未绑定的泛型类型。 术语***绑定类型***指非泛型类型或构造的类型。

未绑定的类型是指声明的类型声明的实体。 未绑定的泛型类型本身不是一种类型，并为变量、 参数或返回值的类型或基类型不能使用。 可以在其中引用未绑定的泛型类型的唯一构造是`typeof`表达式 ([typeof 运算符](expressions.md#the-typeof-operator))。

### <a name="satisfying-constraints"></a>令人满意的约束

每当引用构造的类型或泛型方法时，都会根据泛型类型或方法上声明的类型参数约束检查提供的类型实参 ([类型参数约束](classes.md#type-parameter-constraints))。 每个`where`子句中，类型参数`A`对应于已命名类型参数检查针对每个约束，如下所示：

*  如果约束为类类型、 接口类型或类型参数，则假设`C`表示具有提供的类型参数约束替换为该约束中出现的任何类型参数。 若要满足的约束，它必须是类型的用例`A`可转换为类型`C`由以下项之一：
    * 标识转换 ([标识转换](conversions.md#identity-conversion))
    * 隐式引用转换 ([隐式引用转换](conversions.md#implicit-reference-conversions))
    * 装箱转换 ([装箱转换](conversions.md#boxing-conversions))，前提是类型 A 是不可以为 null 的值类型。
    * 从类型参数的隐式引用、 装箱或类型参数转换`A`到`C`。
*  如果约束是引用类型约束 (`class`)，该类型`A`必须满足以下项之一：
    * `A` 接口类型、 类类型、 委托类型或数组类型。 请注意，`System.ValueType`和`System.Enum`是满足此约束的引用类型。
    * `A` 是一个已知为引用类型的类型参数 ([类型参数约束](classes.md#type-parameter-constraints))。
*  如果约束是值类型约束 (`struct`)，该类型`A`必须满足以下项之一：
    * `A` 为结构类型或枚举类型，但不是为 null 的类型。 请注意，`System.ValueType`和`System.Enum`是引用类型，不满足此约束。
    * `A` 为具有值类型约束的类型形参 ([类型参数约束](classes.md#type-parameter-constraints))。
*  如果约束是构造函数约束`new()`，类型`A`不能`abstract`并且必须具有公共无参数构造函数。 如果以下项之一为 true，条件则满足此问题：
    * `A` 为值类型，因为所有值类型都具有公共默认构造函数 ([默认构造函数](types.md#default-constructors))。
    * `A` 为具有构造函数约束的类型形参 ([类型参数约束](classes.md#type-parameter-constraints))。
    * `A` 为具有值类型约束的类型形参 ([类型参数约束](classes.md#type-parameter-constraints))。
    * `A` 是一个类，不是`abstract`和包含显式声明`public`不带任何参数的构造函数。
    * `A` 不是`abstract`并具有默认构造函数 ([默认构造函数](classes.md#default-constructors))。

如果按给定的类型参数不满足一个或多个类型形参的约束，则，发生编译时错误。

约束类型参数不继承的因为是永远不会继承任何一个。 在以下示例中，`D`需要指定其类型形参上的约束`T`以便`T`满足所施加的类的基类约束`B<T>`。 与此相反，类`E`不需要指定一个约束，因为`List<T>`实现`IEnumerable`任何`T`。

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a>类型参数

类型参数是值类型或引用类型的参数在运行时绑定到指定的标识符。

```antlr
type_parameter
    : identifier
    ;
```

由于类型参数可以具有多个不同的实际类型参数实例化，类型参数具有与其他类型稍有不同的操作和限制。 这些问题包括：

*  类型参数不能用于直接声明基类 ([基类](classes.md#base-class)) 或接口 ([变体类型的参数列表](interfaces.md#variant-type-parameter-lists))。
*  参数取决于该约束，如果有，成员查找类型上的规则应用于类型参数。 中详细介绍[成员查找](expressions.md#member-lookup)。
*  可用的转换为类型参数取决于该约束，如果有的话，应用于类型参数。 中详细介绍[涉及类型参数的隐式转换](conversions.md#implicit-conversions-involving-type-parameters)并[显式动态转换](conversions.md#explicit-dynamic-conversions)。
*  文字`null`不能转换为如果类型参数已知的引用类型的类型参数，除给定类型 ([涉及类型参数的隐式转换](conversions.md#implicit-conversions-involving-type-parameters))。 但是，`default`表达式 ([默认值表达式](expressions.md#default-value-expressions)) 可改为使用。 此外，具有类型参数提供的类型的值可以与进行比较`null`使用`==`并`!=`([引用类型相等运算符](expressions.md#reference-type-equality-operators)) 除非类型参数的值类型约束。
*  一个`new`表达式 ([对象创建表达式](expressions.md#object-creation-expressions)) 可以仅将其用于类型参数如果类型形参受*constructor_constraint*或值类型约束 ([类型参数约束](classes.md#type-parameter-constraints))。
*  类型参数不能在特性中的任意位置使用。
*  类型参数不能使用成员访问中 ([成员访问](expressions.md#member-access)) 或类型名称 ([Namespace 和类型名称](basic-concepts.md#namespace-and-type-names)) 来标识的静态成员或嵌套的类型。
*  在不安全代码中，类型参数不能用作*unmanaged_type* ([指针类型](unsafe-code.md#pointer-types))。

作为一种类型，类型参数都是纯粹是一个编译时构造。 在运行时，每个类型参数绑定到提供的泛型类型声明的类型参数指定的运行时类型。 因此，使用类型参数将在运行时，声明一个变量的类型为封闭式构造的类型 ([打开和关闭类型](types.md#open-and-closed-types))。 运行时执行的所有语句和包含类型参数的表达式使用提供的实际类型作为该参数的类型参数。

## <a name="expression-tree-types"></a>表达式树类型

***表达式树***允许 lambda 表达式，表示为数据结构，而不是可执行代码。 表达式树是值***表达式树类型***窗体`System.Linq.Expressions.Expression<D>`，其中`D`是任何委托类型。 此规范的剩余时间内我们将把这些类型使用速记`Expression<D>`。

如果转换为委托类型从 lambda 表达式存在`D`，转换为表达式树类型也存在于`Expression<D>`。 为委托类型的 lambda 表达式转换生成引用 lambda 表达式的可执行代码的委托，而转换为表达式树类型创建 lambda 表达式的表达式树表示形式。

表达式树的 lambda 表达式的高效内存中数据表示形式，并使 lambda 表达式结构的透明和显式。

就像是委托类型`D`，`Expression<D>`称其具有参数和返回类型，它们是相同的`D`。

下面的示例表示 lambda 表达式作为可执行代码和表达式树。 由于存在到转换`Func<int,int>`，转换也存在到`Expression<Func<int,int>>`:

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

遵循这些分配委托`del`引用返回的方法`x + 1`，并在表达式树`exp`引用描述表达式的数据结构`x => x + 1`。

泛型类型的精确定义`Expression<D>`构造表达式树 lambda 表达式转换为表达式目录树类型时的精确规则，以及将此规范的范围之内。

以下两种情况非常重要明确：

*  并非所有的 lambda 表达式可以转换为表达式树。 例如，不能表示具有语句体的 lambda 表达式和其中包含赋值表达式的 lambda 表达式。 在这些情况下，转换仍存在，但将在编译时失败。 中详细介绍了这些异常[匿名函数转换](conversions.md#anonymous-function-conversions)。
*   `Expression<D>` 提供的实例方法`Compile`可生成类型的委托`D`:

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    调用此委托会导致要执行表达式树所表示的代码。 因此，给定上述定义后，del 和 del2 等效的并且在以下两个语句都将具有相同的效果：

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    执行此代码后`i1`并`i2`都将拥有值`2`。

