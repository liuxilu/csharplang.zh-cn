---
ms.openlocfilehash: 155c1beecddfdfcce2e7948bcb8d6b80428fbd7a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488844"
---
# <a name="arrays"></a>数组

数组是包含许多通过计算索引访问的变量的数据结构。 一个数组，也称为数组的元素中包含的变量包括所有相同的类型，而这种类型称为数组的元素类型。

一个数组，具有用于确定与每个数组元素相关联的索引数的排名。 数组的秩也称为数组的维数。 调用具有一个级别的数组***维数组***。 数组大于一项称为排名***多维数组***。 特定大小的多维数组通常称为二维数组、 三维数组等。

一个数组的每个维度具有一个关联的长度，它是大于或等于零的整数。 维的长度不是数组，该类型的一部分，但在运行时创建的数组类型的实例时而建立。 维度的长度决定了该维度的索引的有效范围：维度的长度`N`，索引的范围可以从`0`到`N - 1`非独占。 数组中元素的总数是数组中每个维度的长度的产品。 如果一个或多个数组的维度的长度为零，则称该数组为空。

数组的元素类型可以是任意类型（包括数组类型）。

## <a name="array-types"></a>数组类型

数组类型被写为*non_array_type*跟一个或多个*rank_specifier*s:

```antlr
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
```

一个*non_array_type*可以是任何*类型*，它是本身不*array_type*。

数组类型的秩的给定的最左侧*rank_specifier*中*array_type*:一个*rank_specifier*指示数组为排名数加一的数组"`,`"的令牌*rank_specifier*。

数组类型的元素类型是删除最左侧所得到的类型*rank_specifier*:

*  数组类型的窗体`T[R]`是带有秩的数组`R`和非数组元素类型`T`。
*  数组类型的窗体`T[R][R1]...[Rn]`是带有秩的数组`R`和元素类型`T[R1]...[Rn]`。

实际上， *rank_specifier*s 从左到右前的最后一个非数组元素类型读取。 类型`int[][,,][,]`是一个一维数组的三维数组的二维数组的元素`int`。

在运行时数组类型的值可以是`null`或对该数组类型的实例的引用。

### <a name="the-systemarray-type"></a>System.Array 类型

类型`System.Array`是所有数组类型的抽象基类型。 隐式引用转换 ([隐式引用转换](conversions.md#implicit-reference-conversions)) 存在从任何数组类型设置为`System.Array`，并显式引用转换 ([显式引用转换](conversions.md#explicit-reference-conversions)) 存在从`System.Array`到任何数组类型。 请注意，`System.Array`本身不是*array_type*。 相反，它是*class_type*所有*array_type*s 派生。

在运行时类型的值`System.Array`可以是`null`或对任何数组类型的实例的引用。

### <a name="arrays-and-the-generic-ilist-interface"></a>数组和泛型 IList 接口

一维数组`T[]`实现接口`System.Collections.Generic.IList<T>`(`IList<T>`简称) 及其基接口。 相应地，没有隐式转换`T[]`到`IList<T>`及其基接口。 此外，如果没有从的隐式引用转换`S`到`T`然后`S[]`实现`IList<T>`，并且没有一种从隐式引用转换`S[]`到`IList<T>`及其基接口 （[隐式引用转换](conversions.md#implicit-reference-conversions))。 如果没有显式引用转换从`S`到`T`则没有显式引用转换从`S[]`到`IList<T>`及其基接口 ([显式引用转换](conversions.md#explicit-reference-conversions)). 例如：
```csharp
using System.Collections.Generic;

class Test
{
    static void Main() {
        string[] sa = new string[5];
        object[] oa1 = new object[5];
        object[] oa2 = sa;

        IList<string> lst1 = sa;                    // Ok
        IList<string> lst2 = oa1;                   // Error, cast needed
        IList<object> lst3 = sa;                    // Ok
        IList<object> lst4 = oa1;                   // Ok

        IList<string> lst5 = (IList<string>)oa1;    // Exception
        IList<string> lst6 = (IList<string>)oa2;    // Ok
    }
}
```

赋值`lst2 = oa1`以来从转换将生成编译时错误`object[]`到`IList<string>`是不隐式的显式转换。 强制转换`(IList<string>)oa1`将导致在运行时自引发异常`oa1`引用`object[]`而不`string[]`。 但是该强制转换`(IList<string>)oa2`不会因为引发异常`oa2`引用`string[]`。

只要没有从的隐式或显式引用转换`S[]`到`IList<T>`，此外，还有一种从显式引用转换`IList<T>`及其基接口到`S[]`([显式引用转换](conversions.md#explicit-reference-conversions))。

当数组类型`S[]`实现`IList<T>`，某些实现的接口的成员可能会引发异常。 接口的实现的确切行为已超出本规范的范畴。

## <a name="array-creation"></a>数组创建

通过创建数组实例*array_creation_expression*s ([数组创建表达式](expressions.md#array-creation-expressions)) 或由字段或局部变量声明包含*array_initializer*([数组初始值设定项](arrays.md#array-initializers))。

创建数组实例后，级别和每个维的长度建立和实例的整个生存期内保持不变。 换而言之，不能更改现有数组实例，秩也不可能以调整其尺寸。

数组实例始终是数组类型。 `System.Array`类型为抽象类型，不能实例化。

创建的数组的元素*array_creation_expression*s 始终将初始化为其默认值 ([默认值](variables.md#default-values))。

## <a name="array-element-access"></a>数组元素访问

使用访问数组元素*element_access*表达式 ([数组访问](expressions.md#array-access)) 的窗体`A[I1, I2, ..., In]`，其中`A`的数组类型和每个表达式`Ix`是类型的表达式`int`， `uint`， `long`， `ulong`，或可以隐式转换为一个或多个这些类型。 数组元素访问的结果是一个变量，即所选的索引的数组元素。

可以使用枚举数组的元素`foreach`语句 ([foreach 语句](statements.md#the-foreach-statement))。

## <a name="array-members"></a>数组成员

每个数组类型继承的成员声明的`System.Array`类型。

## <a name="array-covariance"></a>数组协方差

对于任何两个*reference_type*s`A`并`B`，则隐式引用转换 ([隐式引用转换](conversions.md#implicit-reference-conversions)) 或显式引用转换 ([显式引用转换](conversions.md#explicit-reference-conversions)) 中存在`A`到`B`，然后从数组类型也存在相同的引用转换`A[R]`为数组类型`B[R]`，其中`R`可以是任何给定*rank_specifier* （但相同数组类型）。 这种关系称为***数组协方差***。 数组协方差尤其意味着，数组类型的值`A[R]`实际上可能是对的数组类型实例的引用`B[R]`，则从存在的隐式引用转换`B`到`A`。

数组协方差，由于引用类型数组的元素赋值中包括一个运行时检查，以确保分配给数组元素的值确实是允许的类型 ([简单的赋值](expressions.md#simple-assignment))。 例如：
```csharp
class Test
{
    static void Fill(object[] array, int index, int count, object value) {
        for (int i = index; i < index + count; i++) array[i] = value;
    }

    static void Main() {
        string[] strings = new string[100];
        Fill(strings, 0, 100, "Undefined");
        Fill(strings, 0, 10, null);
        Fill(strings, 90, 10, 0);
    }
}
```

分配给`array[i]`中`Fill`方法隐式包含一个运行时检查，以所引用的对象可确保`value`是`null`的实际元素类型与兼容实例或`array`. 在中`Main`的前两个调用`Fill`成功，但第三个调用会导致`System.ArrayTypeMismatchException`引发时执行的第一个分配到`array[i]`。 发生异常的一个装箱`int`不能存储在`string`数组。

数组协方差专门未扩展到的数组*value_type*s。 例如，不存在转换召开`int[]`作为要视作`object[]`。

## <a name="array-initializers"></a>数组初始值设定项

可能在字段声明中指定数组初始值设定项 ([字段](classes.md#fields))，本地变量声明 ([局部变量声明](statements.md#local-variable-declarations))，和数组创建表达式 ([数组创建表达式](expressions.md#array-creation-expressions)):

```antlr
array_initializer
    : '{' variable_initializer_list? '}'
    | '{' variable_initializer_list ',' '}'
    ;

variable_initializer_list
    : variable_initializer (',' variable_initializer)*
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

数组初始值设定项包含变量的初始值设定项，括在一系列"`{`"和"`}`"标记和分隔"`,`"令牌。 每个变量的初始值设定项是一个表达式或对于多维数组，嵌套的数组初始值设定项。

在其中使用数组初始值设定项的上下文确定正在初始化数组的类型。 在数组创建表达式中，数组类型立即之前初始值设定项，或从数组初始值设定项中的表达式推断。 在字段或变量声明中，数组类型是字段或声明变量的类型。 数组初始值设定项中使用时的字段或变量声明，如：
```csharp
int[] a = {0, 2, 4, 6, 8};
```
它是简写为等效的数组创建表达式：
```csharp
int[] a = new int[] {0, 2, 4, 6, 8};
```

对于一维数组，数组初始值设定项必须包含的一系列数组的元素类型与赋值兼容的表达式。 表达式初始化按升序排列，从索引零开始的元素开始的数组元素。 数组初始值设定项中的表达式数决定要创建的数组实例的长度。 例如，上面的数组初始值设定项创建`int[]`长度为 5 的实例并初始化该实例使用以下值：
```csharp
a[0] = 0; a[1] = 2; a[2] = 4; a[3] = 6; a[4] = 8;
```

对于多维数组，数组初始值设定项必须尽可能多的嵌套数组中没有维度级别。 最外层嵌套级别对应于最左边的维度，最内部的嵌套级别对应于最右边的维度。 数组的每个维的长度确定数组初始值设定项中相应嵌套级别的元素数。 对于每个嵌套的数组初始值设定项中，元素的数目必须与同一级别的其他数组初始值设定项相同。 下面的示例：
```csharp
int[,] b = {{0, 1}, {2, 3}, {4, 5}, {6, 7}, {8, 9}};
```
创建一个二维数组的长度为 5 为最左侧的维度的两个最右边的维度的长度：
```csharp
int[,] b = new int[5, 2];
```
然后初始化该数组实例设置以下值：
```csharp
b[0, 0] = 0; b[0, 1] = 1;
b[1, 0] = 2; b[1, 1] = 3;
b[2, 0] = 4; b[2, 1] = 5;
b[3, 0] = 6; b[3, 1] = 7;
b[4, 0] = 8; b[4, 1] = 9;
```

如果之外最右边的维度的长度为零，则假定后续维度还具有长度为零。 下面的示例：
```csharp
int[,] c = {};
```
使用长度为零的最左边和右边的维度中创建一个二维数组：
```csharp
int[,] c = new int[0, 0];
```

当数组创建表达式中包含显式维的长度和数组初始值设定项时，长度必须是常量表达式，并在每个嵌套级别的元素的数目必须与相应的维度长度。 下面是一些可能的恶意活动：
```csharp
int i = 3;
int[] x = new int[3] {0, 1, 2};        // OK
int[] y = new int[i] {0, 1, 2};        // Error, i not a constant
int[] z = new int[3] {0, 1, 2, 3};     // Error, length/initializer mismatch
```

此处的初始值设定项`y`会导致编译时错误，因为维度长度的表达式不是常量和的初始值设定项`z`导致编译时错误，因为长度和中的元素数初始值设定项不一致。
