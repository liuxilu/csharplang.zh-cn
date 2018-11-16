# <a name="conversions"></a>转换

一个***转换***使表达式能够被视为是指特定类型。 转换可能会导致要被视为具有不同类型的给定类型的表达式或它可能会导致没有要获得一种类型的类型的表达式。 转换可以是***隐式***或***显式***，这将确定是否需要显式强制转换。 例如，从类型转换`int`键入`long`是隐式的因此表达式的类型`int`隐式视为类型`long`。 从类型相反转换`long`键入`int`，是显式的因此显式强制转换为所需。

```csharp
int a = 123;
long b = a;         // implicit conversion from int to long
int c = (int) b;    // explicit conversion from long to int
```

某些转换是由语言定义的。 程序也可以定义其自己的转换 ([用户定义的转换](conversions.md#user-defined-conversions))。

## <a name="implicit-conversions"></a>隐式转换

以下转换被归类为隐式转换：

*  标识转换
*  隐式数值转换
*  枚举隐式转换。
*  可以为 null 的隐式转换
*  Null 文本转换
*  隐式引用转换
*  装箱转换
*  隐式动态转换
*  常量表达式隐式转换
*  用户定义的隐式转换
*  匿名函数转换
*  方法组转换

隐式转换可能在很多情况下，其中包括函数成员调用 ([进行编译时检查的动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution))，强制转换表达式 ([强制转换表达式](expressions.md#cast-expressions))，和分配 ([赋值运算符](expressions.md#assignment-operators))。

预定义隐式转换始终成功并永远不会导致引发异常。 正确设计的用户定义的隐式转换应表现出这些特征也。

用于转换，类型`object`和`dynamic`被视为等效。

但是，动态转换 ([隐式动态转换](conversions.md#implicit-dynamic-conversions)并[显式动态转换](conversions.md#explicit-dynamic-conversions)) 仅适用于类型的表达式`dynamic`([动态类型](types.md#the-dynamic-type)).

### <a name="identity-conversion"></a>标识转换

标识转换将从任何类型转换为同一类型。 这样可以认为已具有所需的类型的实体可以转换为该类型已存在此转换。

*  因为对象和动态被视为等效之间不存在标识转换`object`并`dynamic`，并替换的所有匹配项时都是相同的构造类型之间`dynamic`与`object`。

### <a name="implicit-numeric-conversions"></a>隐式数值转换

隐式数值转换为：

*  从`sbyte`到`short`， `int`， `long`， `float`， `double`，或`decimal`。
*  从`byte`到`short`， `ushort`， `int`， `uint`， `long`， `ulong`， `float`， `double`，或`decimal`。
*  从`short`到`int`， `long`， `float`， `double`，或`decimal`。
*  从`ushort`到`int`， `uint`， `long`， `ulong`， `float`， `double`，或`decimal`。
*  从`int`到`long`， `float`， `double`，或`decimal`。
*  从`uint`到`long`， `ulong`， `float`， `double`，或`decimal`。
*  从`long`到`float`， `double`，或`decimal`。
*  从`ulong`到`float`， `double`，或`decimal`。
*  从`char`到`ushort`， `int`， `uint`， `long`， `ulong`， `float`， `double`，或`decimal`。
*  从`float`到`double`。

从转换`int`， `uint`， `long`，或`ulong`到`float`来回`long`或`ulong`到`double`可能会导致丢失精度，但将永远不会导致丢失量值。 其他隐式数值转换永远不会丢失任何信息。

不存在隐式转换到`char`类型，因此其他整数类型的值没有自动转换为`char`类型。

### <a name="implicit-enumeration-conversions"></a>枚举隐式转换

隐式枚举转换允许*decimal_integer_literal* `0`可转换为任何*enum_type*和任何*nullable_type*其基础类型是*enum_type*。 在后一种情况中，转换计算通过将转换为基础*enum_type*包装结果和 ([为 Null 的类型](types.md#nullable-types))。

### <a name="implicit-interpolated-string-conversions"></a>隐式的内插的字符串转换

隐式的内插字符串转换允许*interpolated_string_expression* ([内插字符串](expressions.md#interpolated-strings)) 转换为`System.IFormattable`或`System.FormattableString`（它可实现`System.IFormattable`).

应用此转换时不的内插字符串组成的字符串值。 改为实例`System.FormattableString`创建，为进一步中所述[内插字符串](expressions.md#interpolated-strings)。

### <a name="implicit-nullable-conversions"></a>可以为 null 的隐式转换

此外可以为这些类型的 null 的形式使用不可为 null 的值类型运行的预定义隐式转换。 为每个预定义隐式标识和不可以为 null 的值类型转换的数值转换`S`为非 null 的值类型`T`，存在以下隐式可以为 null 的转换：

*  隐式转换`S?`到`T?`。
*  隐式转换`S`到`T?`。

可以为 null 的隐式转换的计算基于一种从基础转换`S`到`T`继续，如下所示：

*  如果可以为 null 的转换是从`S?`到`T?`:
    * 如果源值为 null (`HasValue`属性为 false)，结果是类型的 null 值`T?`。
    * 否则，则转换计算过程为从`S?`到`S`后, 跟从基础转换`S`到`T`后, 跟一个包装 ([为 Null 的类型](types.md#nullable-types)) 从`T`到`T?`。

*  如果可以为 null 的转换是从`S`到`T?`，转换计算为从基础转换`S`到`T`跟从一个包装`T`到`T?`。

### <a name="null-literal-conversions"></a>Null 文本转换

隐式转换`null`文字转换为 null 的类型。 这种转换产生 null 值 ([为 Null 的类型](types.md#nullable-types)) 的给定为 null 的类型。

### <a name="implicit-reference-conversions"></a>隐式引用转换

隐式引用转换为：

*  从任何*reference_type*到`object`和`dynamic`。
*  从任何*class_type* `S`任何*class_type* `T`提供`S`派生自`T`。
*  从任何*class_type* `S`任何*interface_type* `T`提供`S`实现`T`。
*  从任何*interface_type* `S`任何*interface_type* `T`提供`S`派生自`T`。
*  从*array_type* `S`具有元素类型`SE`到*array_type* `T`具有元素类型`TE`，提供以下所有项都为 true:
    * `S` 和`T`的区别只在于元素类型。 换而言之，`S`和`T`具有相同的维数。
    * 这两`SE`并`TE`都*reference_type*s。
    * 从存在的隐式引用转换`SE`到`TE`。
*  从任何*array_type*到`System.Array`和其实现的接口。
*  从一维数组类型`S[]`到`System.Collections.Generic.IList<T>`及其基接口，提供从的隐式标识或引用转换`S`到`T`。
*  从任何*delegate_type*到`System.Delegate`和其实现的接口。
*  从任何 null 字面值*reference_type*。
*  从任何*reference_type*到*reference_type* `T`如果它具有隐式标识或引用转换为*reference_type* `T0`和`T0`标识转换为`T`。
*  从任何*reference_type*到接口或委托类型`T`是否为接口或委托类型的隐式标识或引用转换`T0`和`T0`可变化转换 ([变化转换](interfaces.md#variance-conversion)) 到`T`。
*  涉及已知为引用类型的类型参数的隐式转换。 请参阅[涉及类型参数的隐式转换](conversions.md#implicit-conversions-involving-type-parameters)有关涉及类型参数的隐式转换的详细信息。

隐式引用转换指那些之间*reference_type*s 可以保证始终成功，并因此需要在运行时没有检查。

引用转换，隐式或显式的永远不会更改要转换的对象的引用标识。 换而言之，而是引用转换可能会更改引用的类型，它永远不会更改的类型或所引用对象的值。

### <a name="boxing-conversions"></a>装箱转换

装箱转换允许*value_type*隐式转换为引用类型。 从任何已存在的装箱转换*non_nullable_value_type*到`object`并`dynamic`到`System.ValueType`和任何*interface_type*由实现*non_nullable_value_type*。 此外*enum_type*可以转换为类型`System.Enum`。

存在从装箱转换*nullable_type*为引用类型，当且仅当装箱转换存在从基础*non_nullable_value_type*为引用类型。

值类型都具有为接口类型的装箱转换`I`是否为接口类型的装箱转换`I0`并`I0`标识转换为`I`。

值类型都具有为接口类型的装箱转换`I`是否为接口或委托类型的装箱转换`I0`并`I0`可变化转换 ([变化转换](interfaces.md#variance-conversion)) 到`I`.

装箱的值*non_nullable_value_type*分配给对象实例和复制组成*value_type*到该实例的值。 结构可以被封装到类型`System.ValueType`，因为它是所有的结构的基类 ([继承](structs.md#inheritance))。

装箱的值*nullable_type*继续，如下所示：

*  如果源值为 null (`HasValue`属性为 false)，结果为 null 引用的目标类型。
*  否则，结果是一个装箱的引用`T`解包和装箱源值生成的。

装箱转换中进行了描述进一步[装箱转换](types.md#boxing-conversions)。

### <a name="implicit-dynamic-conversions"></a>隐式动态转换

存在从类型的表达式的隐式的动态转换`dynamic`为任何类型`T`。 动态绑定转换 ([动态绑定](expressions.md#dynamic-binding))，这意味着，会在运行时从运行时类型的表达式与要求的隐式转换`T`。 如果不找到任何转换，则引发运行时异常。

请注意此隐式转换看似违反开头中的建议[隐式转换](conversions.md#implicit-conversions)的隐式转换时应永远不会导致异常。 但是它不是转换本身，但*查找*会导致异常的转换。 运行时异常的风险是固有的动态绑定的用法。 如果不想转换的动态绑定，该表达式可以首先转换为`object`，然后到所需的类型。

下面的示例说明了动态的隐式转换：

```csharp
object o  = "object"
dynamic d = "dynamic";

string s1 = o; // Fails at compile-time -- no conversion exists
string s2 = d; // Compiles and succeeds at run-time
int i     = d; // Compiles but fails at run-time -- no conversion exists
```

对分配`s2`和`i`同时采用隐式动态转换，直到运行时中挂起的操作的绑定。 在运行时，隐式转换将从运行时类型的寻求`d`  --  `string` -为目标类型。 为发现转换`string`而不适用于`int`。

### <a name="implicit-constant-expression-conversions"></a>常量表达式隐式转换

隐式的常量表达式转换允许以下转换：

*  一个*constant_expression* ([常量表达式](expressions.md#constant-expressions)) 的类型`int`可转换为类型`sbyte`， `byte`， `short`， `ushort`， `uint`，或`ulong`，提供的值*constant_expression*目标类型的范围内。
*  一个*constant_expression*类型的`long`可转换为类型`ulong`，提供的值*constant_expression*不是负数。

### <a name="implicit-conversions-involving-type-parameters"></a>涉及类型参数的隐式转换

以下隐式转换为给定的类型参数存在`T`:

*  从`T`向其有效的基类`C`，从`T`到的任何基类`C`，并从`T`实现任何接口到`C`。 在运行时，如果`T`是值类型，则转换将执行作为装箱转换。 否则，转换将执行作为隐式引用转换或标识转换。
*  从`T`为接口类型`I`中`T`的有效接口集并从`T`到的任何基接口`I`。 在运行时，如果`T`是值类型，则转换将执行作为装箱转换。 否则，转换将执行作为隐式引用转换或标识转换。
*  从`T`给类型形参`U`提供`T`取决于`U`([类型参数约束](classes.md#type-parameter-constraints))。 在运行时，如果`U`为值类型，则`T`和`U`一定具有相同的类型并不执行任何转换。 否则为如果`T`是值类型，则转换将执行作为装箱转换。 否则，转换将执行作为隐式引用转换或标识转换。
*  从为 null 字面值`T`提供`T`已知为引用类型。
*  从`T`为引用类型`I`是否为引用类型的隐式转换`S0`并`S0`标识转换为`S`。 转换将在运行时执行方式与转换为相同`S0`。
*  从`T`为接口类型`I`是否为接口或委托类型的隐式转换`I0`并`I0`可变化转换为`I`([变化转换](interfaces.md#variance-conversion)). 在运行时，如果`T`是值类型，则转换将执行作为装箱转换。 否则，转换将执行作为隐式引用转换或标识转换。

如果`T`已知为引用类型 ([类型参数约束](classes.md#type-parameter-constraints))，则上面的转换归类为隐式引用转换 ([隐式引用转换](conversions.md#implicit-reference-conversions))。 如果`T`是已知不是引用类型，则上面的转换归类为装箱转换 ([装箱转换](conversions.md#boxing-conversions))。

### <a name="user-defined-implicit-conversions"></a>用户定义的隐式转换

用户定义的隐式转换包含的可选的标准隐式转换后, 跟的一个用户定义的隐式转换运算符后, 跟另一个可选的标准隐式转换的执行。 用于评估用户定义的隐式转换的确切规则所述[处理的用户定义的隐式转换](conversions.md#processing-of-user-defined-implicit-conversions)。

### <a name="anonymous-function-conversions-and-method-group-conversions"></a>匿名函数转换和方法组转换

匿名函数和方法组不具有类型自身，但可能会隐式转换为委托类型或表达式树类型。 中更详细地描述了匿名函数转换[匿名函数转换](conversions.md#anonymous-function-conversions)中的和方法组转换[方法组转换](conversions.md#method-group-conversions)。

## <a name="explicit-conversions"></a>显式转换

以下转换被归类为显式转换：

*  所有隐式转换。
*  显式数值转换。
*  显式枚举转换。
*  可以为 null 的显式转换。
*  显式引用转换。
*  显式接口转换。
*  取消装箱转换。
*  动态的显式转换
*  用户定义的显式转换。

显式转换可以出现在强制转换表达式 ([强制转换表达式](expressions.md#cast-expressions))。

显式转换该集包含所有隐式转换。 这意味着允许多余的强制转换表达式。

不是隐式转换的显式转换为跨类型采用显式明显不同的域不能保证始终成功的转换、 转换已知有可能丢失信息和转换表示法。

### <a name="explicit-numeric-conversions"></a>显式数值转换

显式数值转换是从转换*numeric_type*到另一个*numeric_type*为其隐式数值转换 ([隐式数值转换](conversions.md#implicit-numeric-conversions))尚不存在：

*  从`sbyte`到`byte`， `ushort`， `uint`， `ulong`，或`char`。
*  从`byte`到`sbyte`和`char`。
*  从`short`到`sbyte`， `byte`， `ushort`， `uint`， `ulong`，或`char`。
*  从`ushort`到`sbyte`， `byte`， `short`，或`char`。
*  从`int`到`sbyte`， `byte`， `short`， `ushort`， `uint`， `ulong`，或`char`。
*  从`uint`到`sbyte`， `byte`， `short`， `ushort`， `int`，或`char`。
*  从`long`到`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `ulong`，或`char`。
*  从`ulong`到`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`，或`char`。
*  从`char`到`sbyte`， `byte`，或`short`。
*  从`float`到`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`，或`decimal`。
*  从`double`到`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`，或`decimal`。
*  从`decimal`到`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`，或`double`。

因为显式转换包括所有隐式和显式数值转换，并总是能够从任何转换*numeric_type*到任何其他*numeric_type*使用强制转换表达式 （[强制转换表达式](expressions.md#cast-expressions))。

显式数值转换可能会丢失信息，或可能会导致引发异常。 显式数值转换进行处理，如下所示：

*  对于从一种整型类型转换为另一种整型类型，在处理取决于溢出检查上下文 ([checked 和 unchecked 运算符](expressions.md#the-checked-and-unchecked-operators)) 都位于其将转换的位置：
    * 在中`checked`上下文中，转换成功，如果源操作数的值是目标类型的范围内，但会引发`System.OverflowException`如果源操作数的值超出目标类型的范围。
    * 在`unchecked`上下文中，转换始终成功，并继续，如下所示。
        * 如果源类型大于目标类型，则通过放弃其“额外”最高有效位来截断源值。 结果会被视为目标类型的值。
        * 如果源类型小于目标类型，则源值是符号扩展或零扩展，以使其与目标类型的大小相同。 如果源类型带符号，则是符号扩展；如果源类型是无符号的，则是零扩展。 结果会被视为目标类型的值。
        * 如果源类型与目标类型的大小相同，则源值将被视为目标类型的值。
*  对于从`decimal`为整型类型，源值向零舍入为最接近的整数值，并且此整数值将成为转换的结果。 如果生成的整数值超出目标类型的范围`System.OverflowException`引发。
*  对于从`float`或`double`为整型类型，在处理取决于溢出检查上下文 ([checked 和 unchecked 运算符](expressions.md#the-checked-and-unchecked-operators)) 都位于其将转换的位置：
    * 在`checked`上下文中，转换将继续，如下所示：
        * 如果操作数的值为 NaN 或无穷大，`System.OverflowException`引发。
        * 否则，源操作数是向零到最接近的整数值舍入。 如果此整数值处于目标类型的范围内的此值是转换的结果。
        * 否则，将会引发 `System.OverflowException`。
    * 在`unchecked`上下文中，转换始终成功，并继续，如下所示。
        * 如果操作数的值为 NaN 或无限的转换的结果是目标类型的未指定的值。
        * 否则，源操作数是向零到最接近的整数值舍入。 如果此整数值处于目标类型的范围内的此值是转换的结果。
        * 否则，转换的结果是目标类型的未指定的值。
*  对于从`double`到`float`，则`double`值舍入为最接近`float`值。 如果`double`值因过小而无法表示为`float`，结果将成为正零或负零。 如果`double`该值太大而无法表示为`float`，其结果成为正无穷或负无穷大。 如果`double`值为 NaN，则结果也为 NaN。
*  对于从`float`或`double`到`decimal`，源值转换为`decimal`表示形式和舍入为最接近的数字 28 位小数后根据需要 ([十进制类型](types.md#the-decimal-type)). 如果源值就太小，无法表示为`decimal`，结果则为零。 如果源值为 NaN、 无穷大或太大而无法表示为`decimal`、`System.OverflowException`引发。
*  对于从`decimal`到`float`或`double`，则`decimal`值舍入为最接近`double`或`float`值。 此转换可能会丢失精度，尽管它从不会导致引发异常。

### <a name="explicit-enumeration-conversions"></a>显式枚举转换

显式枚举转换为：

*  从`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`， `double`，或`decimal`到任何*enum_type*。
*  从任何*enum_type*到`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`， `double`，或`decimal`。
*  从任何*enum_type*到任何其他*enum_type*。

两个类型之间的显式枚举转换处理方法是将任何参与*enum_type*的基础类型为*enum_type*，，然后再执行隐式或显式在结果类型之间的数值转换。 例如，给定*enum_type* `E`使用和的基础类型`int`，从转换`E`到`byte`作为显式数值转换处理 ([Explicit数值转换](conversions.md#explicit-numeric-conversions)) 从`int`到`byte`，并从转换`byte`到`E`被处理为隐式数值转换 ([隐式数值转换](conversions.md#implicit-numeric-conversions))从`byte`到`int`。

### <a name="explicit-nullable-conversions"></a>可以为 null 的显式转换

***可以为 null 的显式转换***允许预定义的操作不可以为 null 的值类型也可用于这些类型的可以为 null 的窗体的显式转换。 为每个预定义的显式转换将从非 null 的值类型转换`S`为非 null 的值类型`T`([标识转换](conversions.md#identity-conversion)，[隐式数值转换](conversions.md#implicit-numeric-conversions)，[隐式枚举转换](conversions.md#implicit-enumeration-conversions)，[显式数值转换](conversions.md#explicit-numeric-conversions)，并且[显式枚举转换](conversions.md#explicit-enumeration-conversions))，以下可以为 null 的转换存在：

*  从的显式转换`S?`到`T?`。
*  从的显式转换`S`到`T?`。
*  从的显式转换`S?`到`T`。

可以为 null 的转换的计算基于一种从基础转换`S`到`T`继续，如下所示：

*  如果可以为 null 的转换是从`S?`到`T?`:
    * 如果源值为 null (`HasValue`属性为 false)，结果是类型的 null 值`T?`。
    * 否则，则转换计算过程为从`S?`到`S`后, 跟从基础转换`S`到`T`后, 跟从包装`T`到`T?`。
*  如果可以为 null 的转换是从`S`到`T?`，转换计算为从基础转换`S`到`T`跟从一个包装`T`到`T?`。
*  如果可以为 null 的转换是从`S?`到`T`，则转换过程为从计算`S?`到`S`跟从基础转换`S`到`T`。

请注意，尝试打开可以为 null 值将引发异常，是否值为`null`。

### <a name="explicit-reference-conversions"></a>显式引用转换

显式引用转换为：

*  从`object`并`dynamic`到任何其他*reference_type*。
*  从任何*class_type* `S`任何*class_type* `T`提供`S`是基类的`T`。
*  从任何*class_type* `S`任何*interface_type* `T`提供`S`未密封的并提供`S`不实现`T`。
*  从任何*interface_type* `S`任何*class_type* `T`提供`T`未密封的或未提供`T`实现`S`。
*  从任何*interface_type* `S`任何*interface_type* `T`提供`S`不派生自`T`。
*  从*array_type* `S`具有元素类型`SE`到*array_type* `T`具有元素类型`TE`，提供以下所有项都为 true:
    * `S` 和`T`的区别只在于元素类型。 换而言之，`S`和`T`具有相同的维数。
    * 这两`SE`并`TE`都*reference_type*s。
    * 存在从的显式引用转换`SE`到`TE`。
*  从`System.Array`和它对任何实现的接口*array_type*。
*  从一维数组类型`S[]`到`System.Collections.Generic.IList<T>`并提供没有显式引用转换从其基接口`S`到`T`。
*  从`System.Collections.Generic.IList<S>`到一维数组类型及其基接口`T[]`，前提是没有从的显式标识或引用转换`S`到`T`。
*  从`System.Delegate`和它对任何实现的接口*delegate_type*。
*  从引用类型的引用类型`T`是否为引用类型的显式引用转换`T0`并`T0`标识转换`T`。
*  从接口或委托类型的引用类型`T`是否为接口或委托类型的显式引用转换`T0`并将`T0`可变化转换到`T`或`T`是变化转换到`T0`([变化转换](interfaces.md#variance-conversion))。
*  从`D<S1...Sn>`到`D<T1...Tn>`其中`D<X1...Xn>`是泛型委托类型，`D<S1...Sn>`不符合或相同`D<T1...Tn>`，并为每个类型参数`Xi`的`D`以下包含：
    * 如果`Xi`不变，然后`Si`等同于`Ti`。
    * 如果`Xi`是协变，则隐式或显式标识或引用转换从`Si`到`Ti`。
    * 如果`Xi`然后为逆变`Si`和`Ti`为相同或同时引用类型。
*  涉及已知为引用类型的类型参数的显式转换。 有关涉及类型参数的显式转换的更多详细信息，请参阅[涉及类型参数的显式转换](conversions.md#explicit-conversions-involving-type-parameters)。

显式引用转换指那些需要运行时检查，以确保它们正确无误的引用类型之间。

对于在运行时成功的显式引用转换，源操作数的值必须`null`，或者源操作数所引用的对象的实际类型必须是可通过隐式引用转换为目标类型的类型转换 ([隐式引用转换](conversions.md#implicit-reference-conversions)) 或装箱转换 ([装箱转换](conversions.md#boxing-conversions))。 如果显式引用转换失败，`System.InvalidCastException`引发。

引用转换，隐式或显式的永远不会更改要转换的对象的引用标识。 换而言之，而是引用转换可能会更改引用的类型，它永远不会更改的类型或所引用对象的值。

### <a name="unboxing-conversions"></a>取消装箱转换

取消装箱转换允许显式转换为引用类型*value_type*。 取消装箱转换存在从类型`object`，`dynamic`并`System.ValueType`任何*non_nullable_value_type*，并从任何*interface_type*任何*non_nullable_value_type* ，它实现*interface_type*。 此外键入`System.Enum`可以是任何未装箱*enum_type*。

取消装箱转换从引用类型到存在*nullable_type*如果取消装箱转换从引用类型存在于基础*non_nullable_value_type*的*nullable_type*。

值类型`S`已从接口类型的取消装箱转换`I`它是否有从接口类型的取消装箱转换`I0`并`I0`标识转换为`I`。

值类型`S`已从接口类型的取消装箱转换`I`如果有从接口或委托类型的取消装箱转换`I0`并将`I0`可变化转换到`I`或`I`可变化转换到`I0`([变化转换](interfaces.md#variance-conversion))。

取消装箱操作组成的对象实例的装箱的值的第一个检查给定*value_type*，然后复制实例以外的值。 取消装箱对的 null 引用*nullable_type*生成的 null 值*nullable_type*。 结构可以是从类型未装箱`System.ValueType`，因为它是所有的结构的基类 ([继承](structs.md#inheritance))。

取消装箱转换都是中作了进一步介绍[取消装箱转换](types.md#unboxing-conversions)。

### <a name="explicit-dynamic-conversions"></a>动态的显式转换

类型的表达式中存在一个显式的动态转换`dynamic`为任何类型`T`。 动态绑定转换 ([动态绑定](expressions.md#dynamic-binding))，这意味着，会在运行时从运行时类型的表达式与要求的显式转换`T`。 如果不找到任何转换，则引发运行时异常。

如果不想转换的动态绑定，该表达式可以首先转换为`object`，然后到所需的类型。

假设以下类的定义：
```csharp
class C
{
    int i;

    public C(int i) { this.i = i; }

    public static explicit operator C(string s) 
    {
        return new C(int.Parse(s));
    }
}
```

下面的示例说明了动态的显式转换：
```csharp
object o  = "1";
dynamic d = "2";

var c1 = (C)o; // Compiles, but explicit reference conversion fails
var c2 = (C)d; // Compiles and user defined conversion succeeds
```

最佳的转换`o`到`C`找到在编译时为显式引用转换。 因为在运行时失败`"1"`实际上不是`C`。 转换`d`到`C`但是，为显式的动态转换，挂起到运行时，其中的用户定义的运行时类型转换`d`  --  `string` -为`C`找到，则将会成功。

### <a name="explicit-conversions-involving-type-parameters"></a>涉及类型参数的显式转换

以下的显式转换为给定的类型参数存在`T`:

*  从有效基类`C`的`T`到`T`并从任何基类`C`到`T`。 在运行时，如果`T`是值类型，则转换将执行作为取消装箱转换。 否则，转换将执行作为显式引用转换或标识转换。
*  从任何接口类型到`T`。 在运行时，如果`T`是值类型，则转换将执行作为取消装箱转换。 否则，转换将执行作为显式引用转换或标识转换。
*  从`T`到任何*interface_type* `I`尚没有隐式转换提供`T`到`I`。 在运行时，如果`T`是值类型，则转换将执行作为显式引用转换后跟装箱转换。 否则，转换将执行作为显式引用转换或标识转换。
*  从类型参数`U`到`T`提供`T`取决于`U`([类型参数约束](classes.md#type-parameter-constraints))。 在运行时，如果`U`为值类型，则`T`和`U`一定具有相同的类型并不执行任何转换。 否则为如果`T`是值类型，则转换将执行作为取消装箱转换。 否则，转换将执行作为显式引用转换或标识转换。

如果`T`是已知为引用类型，则上面的转换所有归类为显式引用转换 ([显式引用转换](conversions.md#explicit-reference-conversions))。 如果`T`是已知不是引用类型，则上面的转换将归类为取消装箱转换 ([取消装箱转换](conversions.md#unboxing-conversions))。

上述规则不允许为非接口类型，直接从受约束的类型参数显式转换可能会令人惊讶。 此规则的原因是防止产生混乱并请清除此类转换的语义。 例如，请考虑以下声明：
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)t;                // Error 
    }
}
```

如果直接显式转换`t`到`int`允许，极有可能认为该`X<int>.F(7)`将返回 `7L`。 但是，不这样做，因为已知类型将是在绑定时数字时，将仅考虑标准数值转换。 为了使语义清晰、 上面的示例中必须改为编写：
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)(object)t;        // Ok, but will only work when T is long
    }
}
```

此代码现在可以正常编译但正在执行`X<int>.F(7)`将然后引发一个异常在运行时，由于一个装箱`int`不能直接转换`long`。

### <a name="user-defined-explicit-conversions"></a>用户定义的显式转换

用户定义的显式转换包含的可选的标准显式转换后, 跟的一个用户定义的隐式或显式转换运算符后, 跟另一个可选的标准显式转换的执行。 用于评估用户定义的显式转换的确切规则所述[处理的用户定义的显式转换](conversions.md#processing-of-user-defined-explicit-conversions)。

## <a name="standard-conversions"></a>标准转换

标准转换将这些预定义的转换可能出现的用户定义的转换的一部分。

### <a name="standard-implicit-conversions"></a>标准隐式转换

以下隐式转换被归类为标准的隐式转换：

*  标识转换 ([标识转换](conversions.md#identity-conversion))
*  隐式数值转换 ([隐式数值转换](conversions.md#implicit-numeric-conversions))
*  可以为 null 的隐式转换 ([可以为 null 的隐式转换](conversions.md#implicit-nullable-conversions))
*  隐式引用转换 ([隐式引用转换](conversions.md#implicit-reference-conversions))
*  装箱转换 ([装箱转换](conversions.md#boxing-conversions))
*  常量表达式隐式转换 ([隐式动态转换](conversions.md#implicit-dynamic-conversions))
*  隐式转换涉及类型参数 ([涉及类型参数的隐式转换](conversions.md#implicit-conversions-involving-type-parameters))

标准的隐式转换专门排除用户定义的隐式转换。

### <a name="standard-explicit-conversions"></a>标准的显式转换

标准的显式转换包括所有标准的隐式转换以及子网的相反标准隐式转换为其存在显式转换。 换而言之，如果标准隐式转换存在从类型`A`为某种`B`，然后从类型存在标准的显式转换`A`键入`B`和从类型`B`键入`A`。

## <a name="user-defined-conversions"></a>用户定义的转换

C# 允许将预定义的隐式和显式转换，以通过扩充***用户定义的转换***。 用户定义的转换引入的声明转换运算符 ([转换运算符](classes.md#conversion-operators)) 中类和结构类型。

### <a name="permitted-user-defined-conversions"></a>允许用户定义的转换

C# 允许仅特定用户定义的转换声明。 具体而言，不能重新定义已存在的隐式或显式转换。

给定的源类型`S`和目标类型`T`，如果`S`或`T`是可以为 null 的类型，让`S0`并`T0`指其基础类型，否则`S0`和`T0`是等于`S`和`T`分别。 类或结构允许以声明的源类型转换`S`为目标类型`T`只有在满足以下所有时：

*  `S0` 和`T0`是不同的类型。
*  要么`S0`或`T0`是在运算符声明发生的类或结构类型。
*  既不`S0`也不`T0`是*interface_type*。
*  除用户定义的转换，转换不存在从`S`到`T`或从`T`到`S`。

应用到用户定义的转换的限制的讨论中进一步[转换运算符](classes.md#conversion-operators)。

### <a name="lifted-conversion-operators"></a>提升的转换运算符

提供将从非 null 的值类型转换的用户定义的转换运算符`S`给不可以为 null 的值类型`T`、 一个***提升转换运算符***存在从将`S?`到`T?`. 此提升的转换运算符执行从`S?`到`S`从用户定义的转换后跟`S`到`T`跟从一个包装`T`到`T?`，只不过 null值`S?`直接与 null 值转换为`T?`。

提升的转换运算符具有相同隐式或显式作为其基础的用户定义的转换运算符的分类。 术语"用户定义的转换"适用于对这两者的使用用户定义和提升转换运算符。

### <a name="evaluation-of-user-defined-conversions"></a>用户定义的转换的评估

用户定义的转换将值转换其名为的类型从***源类型***，到另一种类型，称为***目标类型***。 有关查找的用户定义的转换的评估中心***最具体***特定的源和目标类型的用户定义的转换运算符。 此决定分为几个步骤：

*  查找类和结构将被视为用户定义的转换运算符的集。 此集包含的源类型和及其基类和目标类型和及其基类 （与仅类和结构可以声明用户定义的运算符，并且非类类型不具有没有基类的隐式假设）。 此步骤中，如果源或目标类型是出于*nullable_type*，其基础类型改为使用。
*  从该集中的类型确定的用户和提升转换运算符都适用。 转换运算符适用，它必须能够执行的标准转换 ([标准转换](conversions.md#standard-conversions)) 从源类型到操作数的运算符和它的类型必须是可以执行的标准转换从为目标类型的运算符的结果类型。
*  从适用的用户定义运算符，确定哪一个运算符明确最具体的组。 概括地说，最具体的运算符是的运算符的操作数类型是"最靠近"的源类型，并且结果类型为"最靠近"目标类型。 用户定义的转换运算符是首选，通过提升的转换运算符。 以下各节中定义的建立最具体的用户定义的转换运算符的确切规则。

一旦确定最特定的用户定义的转换运算符，实际执行用户定义的转换涉及到最多三个步骤：

*  首先，如果需要，请执行从源类型到用户定义的或提升转换运算符的操作数类型的标准转换。
*  接下来，调用用户定义或提升转换运算符来执行此转换。
*  最后，如果需要，请执行用户定义的或提升转换运算符的结果类型中的标准转换为目标类型。

计算的用户定义转换永远不会涉及多个用户定义或提升转换运算符。 换而言之，从类型转换`S`键入`T`永远不会首先执行中的用户定义的转换`S`到`X`，然后执行用户定义的转换，从`X`到`T`。

以下各节给出的用户定义的隐式或显式转换的确切定义。 定义可使用以下术语：

*  如果标准隐式转换 ([标准隐式转换](conversions.md#standard-implicit-conversions)) 存在从类型`A`为某种`B`，和如果既没有`A`也不`B`是*interface_type*s，然后`A`称为***包含*** `B`，并且`B`可以说负责***包含*** `A`。
*  ***最大的类型***中的一组类型是一个包含在集中的所有其他类型的类型。 如果没有一个类型包含所有其他类型，然后集都有没有最大的类型。 更直观地讲，最大的类型是在集中的"最大"类型，每个其他类型可以隐式转换成一种类型。
*  ***最包含类型***在一组类型是在集中的所有其他类型包含一个类型。 如果没有一个类型被包含所有其他类型，一组具有最不包含类型。 更直观地讲，包含程度最大的类型是在集中的"最小"类型，可以隐式转换为其他类型的每一种类型。

### <a name="processing-of-user-defined-implicit-conversions"></a>处理的用户定义的隐式转换

从类型的用户定义的隐式转换`S`键入`T`进行处理，如下所示：

*  确定的类型`S0`和`T0`。 如果`S`或`T`是可以为 null 的类型，`S0`和`T0`是它们的基础类型，否则`S0`并`T0`等于`S`和`T`分别。
*  查找组的类型， `D`，将被视为哪些用户定义的转换运算符。 此集组成`S0`(如果`S0`是类或结构)，类的基类`S0`(如果`S0`是一个类)，并`T0`(如果`T0`是类或结构)。
*  查找适用的用户和提升转换运算符的组`U`。 此集包含的类或结构中的声明的用户和提升隐式转换运算符`D`，将从包含的类型转换`S`包含一个类型为`T`。 如果`U`是空的转换未定义，则发生编译时错误。
*  查找最具体的源类型`SX`中的运算符`U`:
    * 如果任何中的运算符`U`将从转换`S`，然后`SX`是`S`。
    * 否则为`SX`是包含程度最大的类型中的运算符的源类型的组合集`U`。 如果一个最包含找不到类型，然后转换不明确，则发生编译时错误。
*  查找最具体的目标类型`TX`中的运算符`U`:
    * 如果任何中的运算符`U`将转换为`T`，然后`TX`是`T`。
    * 否则为`TX`是最大的类型中的目标类型中的运算符的组合集`U`。 如果找不到一个最大的类型，则转换是不明确，并将发生编译时错误。
*  查找最精确的转换运算符：
    * 如果`U`包含一个将从转换的用户定义的转换运算符`SX`到`TX`，则表明这是最具体的转换运算符。
    * 否则为如果`U`包含一个从转换的提升的转换运算符`SX`到`TX`，则表明这是最具体的转换运算符。
    * 否则为转换不明确，则发生编译时错误。
*  最后，将应用转换：
    * 如果`S`不是`SX`，然后从标准隐式转换`S`到`SX`执行。
    * 最具体的转换运算符调用以从转换`SX`到`TX`。
    * 如果`TX`不是`T`，然后从标准隐式转换`TX`到`T`执行。

### <a name="processing-of-user-defined-explicit-conversions"></a>处理的用户定义的显式转换

从类型的用户定义的显式转换`S`键入`T`进行处理，如下所示：

*  确定的类型`S0`和`T0`。 如果`S`或`T`是可以为 null 的类型，`S0`和`T0`是它们的基础类型，否则`S0`并`T0`等于`S`和`T`分别。
*  查找组的类型， `D`，将被视为哪些用户定义的转换运算符。 此集组成`S0`(如果`S0`是类或结构)，类的基类`S0`(如果`S0`是一个类)， `T0` (如果`T0`是类或结构)，和的基类`T0`(如果`T0`是一个类)。
*  查找适用的用户和提升转换运算符的组`U`。 此集包含用户和提升的隐式或显式转换运算符声明为类或结构中的`D`，将从包含的类型转换或包含`S`到包含自或包含的类型`T`. 如果`U`是空的转换未定义，则发生编译时错误。
*  查找最具体的源类型`SX`中的运算符`U`:
    * 如果任何中的运算符`U`将从转换`S`，然后`SX`是`S`。
    * 否则为如果中的运算符的任何`U`从包含的类型转换`S`，然后`SX`是中的源类型的这些运算符的组合集包含程度最大的类型。 如果最直接包含找不到类型，然后转换不明确，则发生编译时错误。
    * 否则为`SX`是最大的类型中的源类型中的运算符的组合集`U`。 如果找不到一个最大的类型，则转换是不明确，并将发生编译时错误。
*  查找最具体的目标类型`TX`中的运算符`U`:
    * 如果任何中的运算符`U`将转换为`T`，然后`TX`是`T`。
    * 否则为如果中的运算符的任何`U`将转换为类型通过产生了哪些`T`，然后`TX`是最大的类型中的目标类型的这些运算符的组合集。 如果找不到一个最大的类型，则转换是不明确，并将发生编译时错误。
    * 否则为`TX`是包含程度最大的类型的目标类型中的运算符的组合集`U`。 如果最直接包含找不到类型，然后转换不明确，则发生编译时错误。
*  查找最精确的转换运算符：
    * 如果`U`包含一个将从转换的用户定义的转换运算符`SX`到`TX`，则表明这是最具体的转换运算符。
    * 否则为如果`U`包含一个从转换的提升的转换运算符`SX`到`TX`，则表明这是最具体的转换运算符。
    * 否则为转换不明确，则发生编译时错误。
*  最后，将应用转换：
    * 如果`S`不是`SX`，然后从标准显式转换`S`到`SX`执行。
    * 调用的最特定的用户定义的转换运算符，以转换从`SX`到`TX`。
    * 如果`TX`不是`T`，然后从标准显式转换`TX`到`T`执行。

## <a name="anonymous-function-conversions"></a>匿名函数转换

*Anonymous_method_expression*或*lambda_expression*归类为匿名函数 ([匿名函数表达式](expressions.md#anonymous-function-expressions))。 表达式不具有一种类型，但可以隐式转换为兼容的委托类型或表达式树类型。 具体而言，一个匿名函数`F`委托类型与兼容`D`提供：

*  如果`F`包含*anonymous_function_signature*，然后`D`和`F`具有相同数量的参数。
*  如果`F`不包含*anonymous_function_signature*，然后`D`可能有零个或多个参数的任何类型的任何参数，只要`D`具有`out`参数修饰符。
*  如果`F`具有显式类型化的参数列表中，在每个参数`D`中的相应参数具有相同的类型和修饰符`F`。
*  如果`F`具有隐式类型化的参数列表，`D`不具有`ref`或`out`参数。
*  如果正文`F`是一个表达式，并且要么`D`具有`void`返回类型或`F`是异步和`D`具有返回类型`Task`，然后当的每个参数`F`给定的类型中的相应参数`D`的正文`F`是有效的表达式 (wrt[表达式](expressions.md))，将允许作为*statement_expression* ([表达式语句](statements.md#expression-statements))。
*  如果正文`F`是语句块，并且要么`D`已`void`返回类型或`F`是异步和`D`具有返回类型`Task`，然后当的每个参数`F`给定的类型中的相应参数`D`的正文`F`是一个有效的语句块 (wrt[块](statements.md#blocks)) 无`return`语句指定一个表达式。
*  如果正文`F`是一个表达式，并*任一*`F`是非异步并`D`具有非 void 返回类型`T`，*或*`F`是异步和`D`具有返回类型`Task<T>`，然后当的每个参数`F`提供的中的相应参数类型`D`，正文`F`是有效的表达式 (wrt [表达式](expressions.md)) 这是隐式转换为`T`。
*  如果正文`F`是一个语句块，并*任一*`F`是非异步和`D`具有非 void 返回类型`T`，*或*`F`是异步和`D`具有返回类型`Task<T>`，然后当的每个参数`F`提供的中的相应参数类型`D`，正文`F`是有效的语句块 (wrt[块](statements.md#blocks)) 使用了非可访问的终结点，其中，每个`return`语句指定一个表达式，隐式转换为`T`。

为了简洁起见，本部分使用的缩写形式的任务类型`Task`并`Task<T>`([异步函数](classes.md#async-functions))。

Lambda 表达式`F`与表达式目录树类型兼容`Expression<D>`如果`F`与委托类型兼容`D`。 请注意，这不适用于于匿名方法，只有 lambda 表达式。

某些 lambda 表达式无法转换为表达式树类型： 即使转换*存在*，它在编译时失败。 这是如果 lambda 表达式：

*  具有*块*正文
*  包含简单或复合赋值运算符
*  包含动态绑定的表达式
*  为异步

下面的示例使用泛型委托类型`Func<A,R>`它表示采用类型的自变量的函数`A`，并返回类型的值`R`:
```csharp
delegate R Func<A,R>(A arg);
```

在下面的赋值
```csharp
Func<int,int> f1 = x => x + 1;                 // Ok

Func<int,double> f2 = x => x + 1;              // Ok

Func<double,int> f3 = x => x + 1;              // Error

Func<int, Task<int>> f4 = async x => x + 1;    // Ok
```
每个匿名函数的参数和返回类型将根据向其分配的匿名函数的变量的类型。

第一个分配已成功将匿名函数转换为委托类型`Func<int,int>`因为，当`x`给定类型`int`，`x+1`是隐式转换为键入的有效表达式`int`。

同样，第二个分配已成功转换为匿名函数的委托类型`Func<int,double>`因为的结果`x+1`(类型的`int`) 是隐式转换为键入`double`。

但是，第三个分配是编译时错误，因为，当`x`给定类型`double`的结果`x+1`(类型的`double`) 不是隐式转换为键入`int`。

第四个分配已成功将匿名异步函数转换为委托类型`Func<int, Task<int>>`因为的结果`x+1`(类型的`int`) 隐式转换的结果类型为`int`的任务类型`Task<int>`.

匿名函数可能会影响重载决策，并参与类型推理。 请参阅[函数成员](expressions.md#function-members)的更多详细信息。

### <a name="evaluation-of-anonymous-function-conversions-to-delegate-types"></a>匿名函数转换为委托类型的计算

匿名函数转换为委托类型生成一个委托实例引用匿名函数，并捕获在计算时处于活动状态的外部变量的 （可能为空） 集。 当调用委托时，执行匿名函数的主体。 在正文中的代码执行中使用的捕获由该委托引用的外部变量。

将匿名函数中生成的委托的调用列表包含单个条目。 未指定确切的目标对象和目标方法的委托。 具体而言，没有指定的委托的目标对象是`null`，则`this`封闭函数成员或某个其他对象的值。

转换的语义上完全相同的匿名函数具有相同的捕获外部变量的实例 （可能为空） 集于相同的委托类型都是允许 （但不是必需的） 返回相同的委托实例。 此处用语义上完全相同的术语来表示，在所有情况下，匿名函数的执行将产生相同的结果提供了相同的自变量。 此规则允许如下所示进行优化的代码。

```csharp
delegate double Function(double x);

class Test
{
    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void F(double[] a, double[] b) {
        a = Apply(a, (double x) => Math.Sin(x));
        b = Apply(b, (double y) => Math.Sin(y));
        ...
    }
}
```

由于两个匿名函数委托具有相同 （空） 的集捕获的外部变量，并且由于是在语义上是相同的匿名函数，被允许编译器具有引用相同的目标方法的委托。 实际上，编译器允许从这两种匿名函数表达式返回完全相同的委托实例。

### <a name="evaluation-of-anonymous-function-conversions-to-expression-tree-types"></a>匿名函数转换为表达式树类型的计算

匿名函数转换为表达式目录树类型生成表达式树 ([表达式树类型](types.md#expression-tree-types))。 更确切地说，匿名函数转换的计算会导致构造的对象结构，它表示匿名函数本身的结构。 表达式树中，以及用于创建它，确切的过程的确切结构是定义的实现。

### <a name="implementation-example"></a>实现示例

本部分介绍从其他 C# 构造的角度的匿名函数转换的一个可能实现。 此处所述的实现基于 Microsoft C# 编译器所使用的相同的原则，但它不是强制性的实现，也不是只有一个可能的。 它仅简要说明了一些转换为表达式树，因为它们的准确语义超出了本规范的范围。

本部分的其余部分提供了多个包含具有不同特征的匿名函数的代码示例。 对于每个示例中，提供了使用仅其他 C# 构造的代码到相应的翻译。 在示例中，标识符`D`假定由表示以下委托类型：
```csharp
public delegate void D();
```

匿名函数的最简单形式是捕获任何外部变量：
```csharp
class Test
{
    static void F() {
        D d = () => { Console.WriteLine("test"); };
    }
}
```

这可以转换为引用在其中放置的匿名函数的代码的编译器生成的静态方法的委托实例化：
```csharp
class Test
{
    static void F() {
        D d = new D(__Method1);
    }

    static void __Method1() {
        Console.WriteLine("test");
    }
}
```

以下示例中，在匿名函数引用的实例成员`this`:
```csharp
class Test
{
    int x;

    void F() {
        D d = () => { Console.WriteLine(x); };
    }
}
```

这可以转换为编译器生成的实例方法，其中包含匿名函数的代码：
```csharp
class Test
{
    int x;

    void F() {
        D d = new D(__Method1);
    }

    void __Method1() {
        Console.WriteLine(x);
    }
}
```

在此示例中，匿名函数会捕获本地变量：
```csharp
class Test
{
    void F() {
        int y = 123;
        D d = () => { Console.WriteLine(y); };
    }
}
```

本地变量的生存期现在必须至少扩展到匿名函数委托的生存期。 这可以通过本地变量"提升"到编译器生成类的字段来实现。 本地变量的实例化 ([的本地变量实例化](expressions.md#instantiation-of-local-variables)) 然后对应于创建实例的编译器生成类，并访问本地变量对应于访问的实例中的字段编译器生成的类。 此外，匿名函数会成为编译器生成类的实例方法：
```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.y = 123;
        D d = new D(__locals1.__Method1);
    }

    class __Locals1
    {
        public int y;

        public void __Method1() {
            Console.WriteLine(y);
        }
    }
}
```

最后，以下匿名函数捕获`this`以及使用不同的生存期的两个本地变量：
```csharp
class Test
{
    int x;

    void F() {
        int y = 123;
        for (int i = 0; i < 10; i++) {
            int z = i * 2;
            D d = () => { Console.WriteLine(x + y + z); };
        }
    }
}
```

在这里，为每个语句创建一个编译器生成的类中，以便在不同的块中的局部变量可以有独立的生存期将为捕获局部变量阻止。 实例`__Locals2`，内部语句块中，编译器生成类包含本地变量`z`引用的实例的字段以及`__Locals1`。  实例`__Locals1`，外部语句块中，编译器生成类包含本地变量`y`引用的字段以及`this`封闭函数成员。 使用可以访问这些数据结构所有捕获外部变量的实例通过`__Local2`，并因此可以作为该类的实例方法实现的匿名函数的代码。

```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.__this = this;
        __locals1.y = 123;
        for (int i = 0; i < 10; i++) {
            __Locals2 __locals2 = new __Locals2();
            __locals2.__locals1 = __locals1;
            __locals2.z = i * 2;
            D d = new D(__locals2.__Method1);
        }
    }

    class __Locals1
    {
        public Test __this;
        public int y;
    }

    class __Locals2
    {
        public __Locals1 __locals1;
        public int z;

        public void __Method1() {
            Console.WriteLine(__locals1.__this.x + __locals1.y + z);
        }
    }
}
```

将匿名函数转换为表达式树时，还可以使用此处应用以捕获本地变量的相同技术： 对编译器生成对象的引用可存储在表达式树中，并可以是本地变量的访问权限表示为字段访问这些对象。 此方法的优点是它允许"提升"的本地变量的委托和表达式树之间共享。

## <a name="method-group-conversions"></a>方法组转换

隐式转换 ([隐式转换](conversions.md#implicit-conversions)) 从一个方法组存在 ([表达式分类](expressions.md#expression-classifications)) 到兼容的委托类型。 对于给定的委托类型`D`和表达式`E`型方法组，从存在的隐式转换`E`到`D`如果`E`包含在其正常形式 （适用的至少一种方法[适用函数成员](expressions.md#applicable-function-member)) 到参数列表构造的使用的参数类型及修饰符的`D`，如在下面的示例所述。

从方法组转换的编译时应用程序`E`为委托类型`D`在下面的示例所述。 请注意，是否存在隐式转换`E`到`D`不保证转换的编译时应用程序将成功且没有错误。

*  单个方法`M`已选中对应于方法调用 ([方法调用](expressions.md#method-invocations)) 的窗体`E(A)`，并进行以下修改：
    * 参数列表`A`是一系列表达式中，每个已分类为变量和使用的类型和修饰符 (`ref`或`out`) 中的相应参数*formal_parameter_list* 的`D`.
    * 考虑的候选方法是适用于其正常的窗体方法 ([适用函数成员](expressions.md#applicable-function-member))，不是那些仅在其扩展形式中。
*  如果的算法[方法调用](expressions.md#method-invocations)产生一个错误，则会发生编译时错误。 否则，该算法将生成单个的最佳方法`M`具有相同数量的参数作为`D`和转换被视为存在。
*  所选的方法`M`必须是兼容的 ([委托兼容性](delegates.md#delegate-compatibility)) 与委托类型`D`，否则，发生编译时错误。
*  如果所选的方法`M`是实例方法，与关联的实例表达式`E`确定委托的目标对象。
*  如果选定的方法 M 表示通过成员访问的实例表达式上的扩展方法，该实例表达式确定委托的目标对象。
*  转换的结果是类型的值 `D`，即一个引用所选的方法和目标对象的新创建的委托。
*  请注意，如果此过程可能会导致扩展方法的委托创建的算法[方法调用](expressions.md#method-invocations)无法找到的实例方法，但在处理的调用成功`E(A)`作为扩展方法调用 ([扩展方法调用](expressions.md#extension-method-invocations))。 因此创建的委托捕获扩展方法，以及其第一个参数。

下面的示例演示方法组转换：
```csharp
delegate string D1(object o);

delegate object D2(string s);

delegate object D3();

delegate string D4(object o, params object[] a);

delegate string D5(int i);

class Test
{
    static string F(object o) {...}

    static void G() {
        D1 d1 = F;            // Ok
        D2 d2 = F;            // Ok
        D3 d3 = F;            // Error -- not applicable
        D4 d4 = F;            // Error -- not applicable in normal form
        D5 d5 = F;            // Error -- applicable but not compatible

    }
}
```

分配给`d1`隐式将方法组转换`F`类型的值为`D1`。

分配给`d2`演示如何可以创建具有较少派生 （逆变） 参数类型的方法的委托并更派生 （协变） 的返回类型。

分配给`d3`显示了如何不存在转换方法不适用。

分配给`d4`显示了如何，该方法必须是以普通形式适用。

分配给`d5`显示了如何允许参数和返回类型的委托和方法仅为引用类型不同。

与所有其他隐式和显式转换，强制转换运算符可以用于显式执行方法组转换。 因此，示例
```csharp
object obj = new EventHandler(myDialog.OkClick);
```
可以改为编写
```csharp
object obj = (EventHandler)myDialog.OkClick;
```

方法组可能会影响重载决策，并参与类型推理。 请参阅[函数成员](expressions.md#function-members)的更多详细信息。

方法组转换运行时计算过程继续执行，如下所示：

*  如果在编译时选择的方法是实例方法，或者它是作为实例方法来访问该扩展方法，从与关联的实例表达式确定目标对象的委托的`E`:
    * 计算实例表达式。 如果此计算导致了异常，不执行任何进一步的操作。
    * 如果实例表达式属于*reference_type*，计算实例表达式的值将成为目标对象。 如果所选的方法是实例方法，目标对象是`null`、`System.NullReferenceException`引发并执行任何进一步的操作。
    * 如果实例表达式属于*value_type*，装箱操作 ([装箱转换](types.md#boxing-conversions)) 执行将值转换为一个对象，此对象将成为目标对象。
*  否则所选的方法是静态方法调用的一部分，委托的目标对象是`null`。
*  委托类型的新实例`D`分配。 如果没有足够内存可用于分配新实例，`System.OutOfMemoryException`引发并执行任何进一步的操作。
*  对已在编译时确定的方法的引用初始化新的委托实例和上面计算对目标对象的引用。
