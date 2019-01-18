---
ms.openlocfilehash: 90001cf3d48f216787fc65e59166ec57c5d0ca34
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229511"
---
# <a name="unsafe-code"></a>不安全代码

Core C# 语言中，在前面的章节中，定义不同值得注意的是与 C 和 c + + 中的指针的数据类型作为其省略。 相反，C# 提供引用和创建由垃圾回收器管理的对象的功能。 这种设计，结合其他功能，使 C# 更安全语言比 C 或 c + +。 在 core C# 语言不只是可能具有未初始化的变量、"无关联"的指针或索引超出其边界的数组的表达式。 整个类别的 bug 总是烦扰 C 和 c + + 程序这样就消除了。

虽然在 C 或 c + + 中的几乎每个指针类型构造在 C# 中有引用类型对应，尽管如此，有些情况下指针类型的访问权限，有必要。 例如，与基础操作系统进行交互，访问内存映射的设备，或实现时间关键型算法可能可能或可行而无需访问指针。 为了满足此需求，C# 提供的功能编写***不安全代码***。

不安全代码中，则可以声明和对指针执行指针和整数类型，可以获取变量的地址之间的转换等等。 在某种意义上，编写不安全代码非常类似于编写 C# 程序中的 C 代码。

不安全代码实际上是从开发人员和用户的角度来看"安全"功能。 不安全代码必须清楚地标记为修饰符`unsafe`，因此，开发人员可能不能使用不安全的功能不小心，并且执行引擎的工作方式以确保不能在不受信任环境中执行不安全代码。

## <a name="unsafe-contexts"></a>不安全的上下文

仅在不安全上下文中可用的 C# 的不安全功能。 通过包括引入了不安全的上下文`unsafe`中声明的类型或成员，也可以使用修饰符*unsafe_statement*:

*  类、 结构、 接口或委托的声明可能包括`unsafe`修饰符，在这种情况下 （包括类、 结构或接口的正文） 该类型声明的整个文本范围被认为不安全的上下文。
*  字段、 方法、 属性、 事件、 索引器、 运算符、 实例构造函数、 析构函数或静态构造函数的声明可能包括`unsafe`修饰符，在这种情况下该成员声明的整个文本范围被认为不安全上下文。
*  *Unsafe_statement*允许不安全的上下文中使用*块*。 关联的整个文本范围*块*被认为是不安全上下文。

下面显示了关联的语法生产。

```antlr
class_modifier_unsafe
    : 'unsafe'
    ;

struct_modifier_unsafe
    : 'unsafe'
    ;

interface_modifier_unsafe
    : 'unsafe'
    ;

delegate_modifier_unsafe
    : 'unsafe'
    ;

field_modifier_unsafe
    : 'unsafe'
    ;

method_modifier_unsafe
    : 'unsafe'
    ;

property_modifier_unsafe
    : 'unsafe'
    ;

event_modifier_unsafe
    : 'unsafe'
    ;

indexer_modifier_unsafe
    : 'unsafe'
    ;

operator_modifier_unsafe
    : 'unsafe'
    ;

constructor_modifier_unsafe
    : 'unsafe'
    ;

destructor_declaration_unsafe
    : attributes? 'extern'? 'unsafe'? '~' identifier '(' ')' destructor_body
    | attributes? 'unsafe'? 'extern'? '~' identifier '(' ')' destructor_body
    ;

static_constructor_modifiers_unsafe
    : 'extern'? 'unsafe'? 'static'
    | 'unsafe'? 'extern'? 'static'
    | 'extern'? 'static' 'unsafe'?
    | 'unsafe'? 'static' 'extern'?
    | 'static' 'extern'? 'unsafe'?
    | 'static' 'unsafe'? 'extern'?
    ;

embedded_statement_unsafe
    : unsafe_statement
    | fixed_statement
    ;

unsafe_statement
    : 'unsafe' block
    ;
```

在示例

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

`unsafe`结构声明中指定的修饰符将导致该结构声明变得不安全的上下文的整个文本范围。 因此，它是可以声明`Left`和`Right`字段为指针类型。 此外可以编写上面的示例

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

在这里，`unsafe`中的字段声明修饰符导致这些声明被认为是不安全上下文。

除了建立安全上下文中，从而允许使用指针类型`unsafe`修饰符的类型或成员上无效。 在示例

```csharp
public class A
{
    public unsafe virtual void F() {
        char* p;
        ...
    }
}

public class B: A
{
    public override void F() {
        base.F();
        ...
    }
}
```

`unsafe`上的修饰符`F`中的方法`A`只需将导致的文本范围`F`变得不安全的上下文可以在其中使用的语言不安全功能。 重写`F`中`B`，无需重新指定`unsafe`修饰符-除非，当然，`F`中的方法`B`本身需要访问不安全的功能。

指针类型为方法的一部分时，这种情况是签名的略有不同

```csharp
public unsafe class A
{
    public virtual void F(char* p) {...}
}

public class B: A
{
    public unsafe override void F(char* p) {...}
}
```

此处，因为`F`的签名包含类型的指针，它可以只编写不安全的上下文中。 但是，可以通过使整个类不安全，因为这种情况在引入不安全的上下文`A`，或通过包括`unsafe`修饰符在方法声明中，在这种情况`B`。

## <a name="pointer-types"></a>指针类型

在不安全上下文中，*类型*([类型](types.md)) 可能*pointer_type* ，以及*value_type*或*reference_type*. 但是， *pointer_type*也可能在使用`typeof`表达式 ([匿名对象创建表达式](expressions.md#anonymous-object-creation-expressions)) 不安全上下文之外这种情况下不使用不安全。

```antlr
type_unsafe
    : pointer_type
    ;
```

一个*pointer_type*写作*unmanaged_type*或关键字`void`后, 跟`*`令牌：

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

之前指定的类型`*`在指针类型称为***宏中类型***指针类型。 它表示指向指针类型的值的变量的类型。

与不同的引用 （引用类型的值），指针不会跟踪垃圾回收器--垃圾回收器是未知的指针和指向的数据。 对于不允许使用指向此原因以指向引用或为结构，其中包含引用，并且的指针的引用类型必须是*unmanaged_type*。

*Unmanaged_type*是任何类型，不是*reference_type*或构造类型，并且不包含*reference_type*或构造类型的任何级别的字段嵌套。 换而言之， *unmanaged_type*是以下之一：

*  `sbyte``byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`， `double`， `decimal`，或`bool`。
*  任何*enum_type*。
*  任何*pointer_type*。
*  任何用户定义*struct_type*的构造的类型并不包含的字段*unmanaged_type*仅。

指针和引用的混合的直观规则是，引用 （对象） 的目标可以包含指针，但不是允许包含引用的指针的引用者。

下表给出了指针类型的一些示例：

| __示例__ | __说明__                               |
|-------------|-----------------------------------------------|
| `byte*`     | 指向 `byte`                             |
| `char*`     | 指向 `char`                             |
| `int**`     | 指针到指向 `int`                   |
| `int*[]`    | 指向的指针的一维数组 `int` |
| `void*`     | 指向未知类型                       |

对于给定的实现，所有指针类型必须都具有相同的大小和表示形式。

与 C 和 c + +，当在同一声明中，在 C# 中声明多个指针时不同`*`编写基础类型，以及不为每个指针名称的前缀标点符号。 例如

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

具有类型的指针的值`T*`表示的类型的变量的地址`T`。 指针间接寻址运算符`*`([指针间接寻址](unsafe-code.md#pointer-indirection)) 可用于访问此变量。 例如，给定变量`P`类型的`int*`，该表达式`*P`表示`int`变量中包含的地址，请参阅`P`。

对象引用，如指针可能`null`。 将应用间接寻址运算符到`null`指针会导致实现定义的行为。 具有值的指针`null`由所有位均为零。

`void*`类型表示指向未知类型的指针。 目标类型是未知的因为间接寻址运算符不能应用于类型的指针`void*`，也不能对这样的指针执行任何算术运算。 但是，类型的指针`void*`可转换为任何其他指针类型 （反之亦然）。

指针类型是单独类别的类型。 引用类型和值类型，与指针类型不继承自`object`和指针类型之间不存在转换和`object`。 具体而言，装箱和取消装箱 ([装箱和取消装箱](types.md#boxing-and-unboxing)) 不支持的指针。 但是，指针类型和整型类型之间以及不同的指针类型之间允许转换。 这中所述[指针转换](unsafe-code.md#pointer-conversions)。

一个*pointer_type*不能用作类型参数 ([构造类型](types.md#constructed-types))，和类型推理 ([类型推理](expressions.md#type-inference)) 上将具有推断的泛型方法调用失败类型实参为指针类型。

一个*pointer_type*可能用作可变字段的类型 ([可变字段](classes.md#volatile-fields))。

尽管可以作为传递指针`ref`或`out`参数，这样做可能会导致未定义的行为，因为指针可能也会设置为指向本地变量不再存在时被调用的方法返回时或它的固定的对象用于指明，不再固定的。 例如：

```csharp
using System;

class Test
{
    static int value = 20;

    unsafe static void F(out int* pi1, ref int* pi2) {
        int i = 10;
        pi1 = &i;

        fixed (int* pj = &value) {
            // ...
            pi2 = pj;
        }
    }

    static void Main() {
        int i = 10;
        unsafe {
            int* px1;
            int* px2 = &i;

            F(out px1, ref px2);

            Console.WriteLine("*px1 = {0}, *px2 = {1}",
                *px1, *px2);    // undefined behavior
        }
    }
}
```

一种方法可以返回某些类型的值和该类型可以是指针。 例如，当的连续序列提供一个指向`int`s，该序列的元素计数和一些其他`int`值，以下方法在该序列中，返回该值的地址，如果出现匹配项; 否则返回`null`:

```csharp
unsafe static int* Find(int* pi, int size, int value) {
    for (int i = 0; i < size; ++i) {
        if (*pi == value) 
            return pi;
        ++pi;
    }
    return null;
}
```

在不安全上下文中，多个构造是可用于操作指针：

*  `*`可用于执行指针间接寻址运算符 ([指针间接寻址](unsafe-code.md#pointer-indirection))。
*  `->`运算符可用于通过指针访问结构成员 ([指针成员访问](unsafe-code.md#pointer-member-access))。
*  `[]`运算符可用于索引指针 ([指针元素访问](unsafe-code.md#pointer-element-access))。
*  `&`运算符可用于获取变量的地址 ([address-of 运算符](unsafe-code.md#the-address-of-operator))。
*  `++`并`--`运算符可以用于递增和递减指针 ([指针递增和递减](unsafe-code.md#pointer-increment-and-decrement))。
*  `+`并`-`可用于执行指针算术运算符 ([指针算法](unsafe-code.md#pointer-arithmetic))。
*  `==`， `!=`， `<`， `>`， `<=`，并且`=>`运算符可用于比较指针 ([指针比较](unsafe-code.md#pointer-comparison))。
*  `stackalloc`运算符可用于从调用堆栈中分配内存 ([固定大小的缓冲区](unsafe-code.md#fixed-size-buffers))。
*  `fixed`语句可用于临时固定变量，因此可以获取其地址 ([fixed 的语句](unsafe-code.md#the-fixed-statement))。

## <a name="fixed-and-moveable-variables"></a>固定和可移动变量

Address-of 运算符 ([address-of 运算符](unsafe-code.md#the-address-of-operator)) 和`fixed`语句 ([fixed 的语句](unsafe-code.md#the-fixed-statement)) 将变量划分为两个类别：***固定变量***并***可移动变量***。

固定的变量驻留在不受垃圾回收器的操作的存储位置中。 （固定变量的示例包括本地变量、 值参数和变量创建的取消引用指针）。另一方面，可移动变量驻留在受到重定位或垃圾回收器处置的存储位置中。 （可移动的变量的示例中包括字段对象和数组的元素。）

`&`运算符 ([address-of 运算符](unsafe-code.md#the-address-of-operator)) 允许不受限制地获取为固定变量的地址。 但是，由于受重定位或垃圾回收器处置是可移动变量，可移动的变量的地址可以仅使用获取`fixed`语句 ([fixed 的语句](unsafe-code.md#the-fixed-statement))，并且该地址专用于该期间保持有效`fixed`语句。

准确地说，固定的变量是以下值之一：

*  从变量从而*simple_name* ([简单名称](expressions.md#simple-names))，是指本地变量或值参数，除非该变量所捕获的匿名函数。
*  从变量产生*member_access* ([成员访问](expressions.md#member-access)) 的窗体`V.I`，其中`V`的固定变量*struct_type*。
*  从变量从而*pointer_indirection_expression* ([指针间接寻址](unsafe-code.md#pointer-indirection)) 的窗体`*P`、 一个*pointer_member_access* ([指针成员访问](unsafe-code.md#pointer-member-access)) 的窗体`P->I`，或*pointer_element_access* ([指针元素访问](unsafe-code.md#pointer-element-access)) 的窗体`P[E]`。

所有其他变量被归类为可移动的变量。

请注意，静态字段归为可移动的变量。 另请注意，`ref`或`out`参数分类为可移动的变量，即使指定的参数的参数是固定的变量。 最后，请注意生成的取消引用某个指针的变量始终属于固定变量。

## <a name="pointer-conversions"></a>指针转换

在不安全上下文中，可隐式转换的集合 ([隐式转换](conversions.md#implicit-conversions)) 扩展为包括以下隐式指针转换：

*  从任何*pointer_type*为类型`void*`。
*  从`null`任何文字*pointer_type*。

此外，在不安全的上下文，可显式转换的集合 ([显式转换](conversions.md#explicit-conversions)) 扩展为包括以下显式指针转换：

*  从任何*pointer_type*到任何其他*pointer_type*。
*  从`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`，或`ulong`到任何*pointer_type*。
*  从任何*pointer_type*到`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`，或`ulong`。

最后，在不安全的上下文，标准隐式转换的集合 ([标准隐式转换](conversions.md#standard-implicit-conversions)) 包括下列指针转换：

*  从任何*pointer_type*为类型`void*`。

两个指针类型之间的转换永远不会更改实际指针值。 换而言之，从一个指针类型转换为另一个具有对指定的指针的基础地址没有影响。

当一个指针类型转换到另一个，如果生成的指针不指向的类型正确对齐时，则该行为不确定，如果结果取消引用。 一般情况下，"正确对齐"的概念是可传递： 如果指向类型的指针`A`指向类型的指针正确对齐`B`，而后者又正确对齐指向类型的指针`C`，然后指向类型`A`指向类型的指针正确对齐`C`。

请考虑以下情况，在其中具有一种类型的变量访问通过指向不同类型的指针：

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

当指针类型转换为指向字节，结果将指向的最低寻址字节的变量。 结果，最大的变量的大小的连续递增值产生指向该变量的剩余字节数。 例如，以下方法显示中每个 8 个字节的双精度为十六进制值：

```csharp
using System;

class Test
{
    unsafe static void Main() {
      double d = 123.456e23;
        unsafe {
           byte* pb = (byte*)&d;
            for (int i = 0; i < sizeof(double); ++i)
               Console.Write("{0:X2} ", *pb++);
            Console.WriteLine();
        }
    }
}
```

当然，生成的输出取决于字节顺序。

指针和整数之间的映射是实现定义的。 但是，在 32 * 和 64 位的 CPU 体系结构具有线性地址空间的转换的指针的整数类型通常具有的行为完全相同的转换`uint`或`ulong`分别向或从这些整型值。

### <a name="pointer-arrays"></a>指针数组

在不安全上下文中，可以构造的指针的数组。 只有某些适用于其他数组类型的转换适用于指针数组：

*  隐式引用转换 ([隐式引用转换](conversions.md#implicit-reference-conversions)) 从任何*array_type*到`System.Array`并且它还实现的接口适用于指针数组。 但是，任何尝试访问数组元素通过`System.Array`或其实现的接口会导致在运行时异常，因为指针类型不是可转换为`object`。
*  隐式和显式引用转换 ([隐式引用转换](conversions.md#implicit-reference-conversions)，[显式引用转换](conversions.md#explicit-reference-conversions)) 从一维数组类型`S[]`到`System.Collections.Generic.IList<T>`和其通用的基接口永远不会用于指针的数组，因为指针类型不能用作类型参数，并且不存在于非指针类型转换指针类型。
*  显式引用转换 ([显式引用转换](conversions.md#explicit-reference-conversions)) 从`System.Array`到任何实现接口*array_type*适用于指针数组。
*  显式引用转换 ([显式引用转换](conversions.md#explicit-reference-conversions)) 从`System.Collections.Generic.IList<S>`到一维数组类型及其基接口`T[]`永远不会应用于指针数组，因为指针类型不能为使用作为类型参数，并且没有指针类型转换为非指针类型。

这些限制意味着为扩展`foreach`语句对数组中所述[foreach 语句](statements.md#the-foreach-statement)不能应用于指针数组。 相反，在窗体的 foreach 语句

```csharp
foreach (V v in x) embedded_statement
```

其中的类型`x`是数组类型的窗体`T[,,...,]`，`N`是数减 1 的维度和`T`或`V`是指针类型，将展开使用嵌套的 for 循环，如下所示：

```csharp
{
    T[,,...,] a = x;
    for (int i0 = a.GetLowerBound(0); i0 <= a.GetUpperBound(0); i0++)
    for (int i1 = a.GetLowerBound(1); i1 <= a.GetUpperBound(1); i1++)
    ...
    for (int iN = a.GetLowerBound(N); iN <= a.GetUpperBound(N); iN++) {
        V v = (V)a.GetValue(i0,i1,...,iN);
        embedded_statement
    }
}
```

变量`a`， `i0`， `i1`，...，`iN`不可见或不可以访问`x`或*embedded_statement*或程序的任何其他源代码。 该变量`v`是嵌入的语句中以只读的。 如果不存在的显式转换 ([指针转换](unsafe-code.md#pointer-conversions)) 从`T`（元素类型） 到`V`，将会生成错误并采取任何进一步的步骤。 如果`x`具有值`null`、`System.NullReferenceException`在运行时引发。

## <a name="pointers-in-expressions"></a>表达式中的指针

在不安全上下文中，表达式可能产生的结果是指针类型，但它不安全的上下文之外是表达式是指针类型的表达式的编译时错误。 准确地说，不安全的上下文之外的编译时错误如果会发生任何*simple_name* ([简单名称](expressions.md#simple-names))， *member_access* ([成员访问](expressions.md#member-access))， *invocation_expression* ([调用表达式](expressions.md#invocation-expressions))，或*element_access* ([元素访问](expressions.md#element-access)) 的指针类型。

在不安全上下文中， *primary_no_array_creation_expression* ([主表达式](expressions.md#primary-expressions)) 和*unary_expression* ([一元运算符](expressions.md#unary-operators))生产允许以下其他构造：

```antlr
primary_no_array_creation_expression_unsafe
    : pointer_member_access
    | pointer_element_access
    | sizeof_expression
    ;

unary_expression_unsafe
    : pointer_indirection_expression
    | addressof_expression
    ;
```

这些构造以下各节所述。 语法暗示的优先顺序和不安全的运算符的关联性。

### <a name="pointer-indirection"></a>指针间接寻址

一个*pointer_indirection_expression*包含一个星号 (`*`) 后跟*unary_expression*。

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

一元`*`运算符表示指针间接寻址和用于获取一个指针指向的变量。 计算结果`*P`，其中`P`是指针类型的表达式`T*`，类型的变量`T`。 它会导致编译时错误应用一元`*`类型的表达式的运算符`void*`或不是指针类型的表达式。

应用一元的效果`*`运算符`null`是实现定义的指针。 特别是，则此操作抛出不能保证`System.NullReferenceException`。

如果已经分配了无效的值的指针，一元的行为`*`运算符是不确定。 无效值时取消指针引用由一元`*`运算符是为指向类型正确对齐的地址 (示例中的，请参阅[指针转换](unsafe-code.md#pointer-conversions))，以及之后的变量的地址其生命周期结束。

为了明确赋值分析，通过计算形式的表达式将产生的变量`*P`被认为是最初赋值 ([最初分配变量](variables.md#initially-assigned-variables))。

### <a name="pointer-member-access"></a>指针成员访问

一个*pointer_member_access*组成*primary_expression*后, 跟"`->`"令牌后, 跟*标识符*和可选*type_argument_list*。

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

在窗体的指针成员访问`P->I`，`P`必须是指针类型的表达式不是`void*`，和`I`必须表示为的类型的可访问成员`P`点。

指针成员访问窗体`P->I`评估完全为`(*P).I`。 有关说明的指针间接寻址运算符 (`*`)，请参阅[指针间接寻址](unsafe-code.md#pointer-indirection)。 有关成员访问运算符的说明 (`.`)，请参阅[成员访问](expressions.md#member-access)。

在示例

```csharp
using System;

struct Point
{
    public int x;
    public int y;

    public override string ToString() {
        return "(" + x + "," + y + ")";
    }
}

class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            p->x = 10;
            p->y = 20;
            Console.WriteLine(p->ToString());
        }
    }
}
```

`->`运算符用于访问字段并调用通过指针结构的方法。 因为该操作`P->I`恰好等同于`(*P).I`，则`Main`方法可能很有效编写：

```csharp
class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            (*p).x = 10;
            (*p).y = 20;
            Console.WriteLine((*p).ToString());
        }
    }
}
```

### <a name="pointer-element-access"></a>指针元素访问

一个*pointer_element_access*组成*primary_no_array_creation_expression*括起来的表达式后跟"`[`"和"`]`"。

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

在指针元素访问窗体`P[E]`，`P`必须是指针类型的表达式不是`void*`，和`E`必须可以隐式转换为的表达式`int`， `uint`， `long`，或`ulong`。

指针元素访问窗体`P[E]`评估完全为`*(P + E)`。 有关说明的指针间接寻址运算符 (`*`)，请参阅[指针间接寻址](unsafe-code.md#pointer-indirection)。 有关说明的指针加法运算符 (`+`)，请参阅[指针算法](unsafe-code.md#pointer-arithmetic)。

在示例

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) p[i] = (char)i;
        }
    }
}
```

指针元素访问用来初始化中的字符缓冲区`for`循环。 因为该操作`P[E]`恰好等同于`*(P + E)`，该示例可以很有效编写：

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) *(p + i) = (char)i;
        }
    }
}
```

指针元素访问运算符不检查越界错误和访问时的行为元素未定义超出边界。 这是与 C 和 c + + 相同。

### <a name="the-address-of-operator"></a>Address-of 运算符

*Addressof_expression*包含与号 (`&`) 后跟*unary_expression*。

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

提供了一个表达式`E`它属于类型`T`归类为固定变量和 ([固定和可移动变量](unsafe-code.md#fixed-and-moveable-variables))，则构造`&E`计算通过给出的变量的地址`E`. 结果的类型是`T*`和分类为一个值。 如果会发生编译时错误`E`未归类为变量中，如果`E`归类为只读的本地变量，或者如果`E`表示可移动的变量。 在最后一种情况下，fixed 语句 ([fixed 的语句](unsafe-code.md#the-fixed-statement)) 可以用于暂时"修复"该变量，再获取其地址。 如中所述[成员访问](expressions.md#member-access)、 实例构造函数或静态构造函数的结构或类，它定义外部`readonly`字段中，该字段被视为一个值，而不是变量。 在这种情况下，不能采用其地址。 同样，不能采用常量的地址。

`&`运算符不要求其自变量明确赋值，但以下`&`操作，该运算符应用到的变量被视为发生该操作的执行路径中明确赋值。 它是变量的程序员以确保正确初始化的责任实际进行这种情况下。

在示例

```csharp
using System;

class Test
{
    static void Main() {
        int i;
        unsafe {
            int* p = &i;
            *p = 123;
        }
        Console.WriteLine(i);
    }
}
```

`i` 明确赋值后被视为`&i`操作用于初始化`p`。 分配给`*p`有效初始化`i`，但包含此初始化是程序员的责任时，会发生任何编译时错误，如果分配的已删除。

明确赋值规则`&`运算符存在，这样可以避免多余的本地变量的初始化。 例如，许多外部 Api 会将指向由 API 填充的结构的指针。 对此类 Api 的调用通常会传递本地结构变量的地址，并且如果没有规则，多余的初始化结构变量的需要。

### <a name="pointer-increment-and-decrement"></a>指针递增和递减

在不安全上下文中，`++`并`--`运算符 ([后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators)并[前缀递增和递减运算符](expressions.md#prefix-increment-and-decrement-operators)) 可以应用于指针除之外的所有类型的变量`void*`。 因此，对于每个指针类型`T*`，隐式定义了以下运算符：

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

运算符将产生与相同的结果`x + 1`并`x - 1`分别 ([指针算法](unsafe-code.md#pointer-arithmetic))。 换句话说，对于类型的指针变量`T*`，则`++`运算符将添加`sizeof(T)`的变量中包含的地址和`--`运算符减去`sizeof(T)`从变量中包含的地址。

如果指针递增或递减运算溢出指针类型的域，则结果是实现定义的但不会产生异常。

### <a name="pointer-arithmetic"></a>指针算术

在不安全上下文中，`+`并`-`运算符 ([加法运算符](expressions.md#addition-operator)并[减法运算符](expressions.md#subtraction-operator)) 可应用于除之外的所有指针类型的值`void*`。 因此，对于每个指针类型`T*`，隐式定义了以下运算符：

```csharp
T* operator +(T* x, int y);
T* operator +(T* x, uint y);
T* operator +(T* x, long y);
T* operator +(T* x, ulong y);

T* operator +(int x, T* y);
T* operator +(uint x, T* y);
T* operator +(long x, T* y);
T* operator +(ulong x, T* y);

T* operator -(T* x, int y);
T* operator -(T* x, uint y);
T* operator -(T* x, long y);
T* operator -(T* x, ulong y);

long operator -(T* x, T* y);
```

提供了一个表达式`P`的指针类型`T*`和表达式`N`类型的`int`， `uint`， `long`，或者`ulong`，表达式`P + N`和`N + P`计算类型的指针值`T*`相加得到`N * sizeof(T)`到由给定的地址`P`。 同样，表达式`P - N`计算类型的指针值`T*`中时得到的减去`N * sizeof(T)`从给定的地址`P`。

给定的两个表达式之间`P`和`Q`，指针类型的`T*`，该表达式`P - Q`计算给定的地址之间的差异`P`和`Q`然后将该差值除以`sizeof(T)`. 结果的类型始终是`long`。 实际上，`P - Q`计算为`((long)(P) - (long)(Q)) / sizeof(T)`。

例如：

```csharp
using System;

class Test
{
    static void Main() {
        unsafe {
            int* values = stackalloc int[20];
            int* p = &values[1];
            int* q = &values[15];
            Console.WriteLine("p - q = {0}", p - q);
            Console.WriteLine("q - p = {0}", q - p);
        }
    }
}
```

这会生成输出：

```
p - q = -14
q - p = 14
```

如果指针算术运算溢出指针类型的域，将结果截断以实现定义的方式，但不会产生异常。

### <a name="pointer-comparison"></a>指针比较

在不安全上下文中， `==`， `!=`， `<`， `>`， `<=`，并且`=>`运算符 ([关系和类型测试运算符](expressions.md#relational-and-type-testing-operators)) 可应用于所有的值指针类型。 指针比较运算符为：

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

因为存在从到任何指针类型的隐式转换`void*`可以使用这些运算符比较类型，任何指针类型的操作数。 比较运算符比较两个操作数给定就好像这是无符号的整数的地址。

### <a name="the-sizeof-operator"></a>Sizeof 运算符

`sizeof`运算符将返回给定类型的变量所占用的字节数。 指定为的操作数的类型`sizeof`必须是*unmanaged_type* ([指针类型](unsafe-code.md#pointer-types))。

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

结果`sizeof`运算符是类型的值`int`。 对于某些预定义的类型，`sizeof`运算符产生的常量值下, 表中所示。


| __表达式__   | __结果__ |
|------------------|------------|
| `sizeof(sbyte)`  | `1`        |
| `sizeof(byte)`   | `1`        |
| `sizeof(short)`  | `2`        |
| `sizeof(ushort)` | `2`        |
| `sizeof(int)`    | `4`        |
| `sizeof(uint)`   | `4`        |
| `sizeof(long)`   | `8`        |
| `sizeof(ulong)`  | `8`        |
| `sizeof(char)`   | `2`        |
| `sizeof(float)`  | `4`        |
| `sizeof(double)` | `8`        |
| `sizeof(bool)`   | `1`        |

对于所有其他类型，结果的`sizeof`运算符是实现定义的和分类为一个值，不是常量。

未指定成员打包到结构的顺序。

出于对齐目的，可能有未命名的填充结构内的结构的开头和末尾的结构。 填充为所使用的位的内容是不确定的。

应用于具有结构类型的操作数时，结果将是包括所有填充该类型的变量中的字节总数。

## <a name="the-fixed-statement"></a>Fixed 的语句

在不安全上下文中， *embedded_statement* ([语句](statements.md)) 生产允许的其他构造`fixed`用于"修复"可移动的变量的语句，以便其语句的持续时间内，地址保持不变。

```antlr
fixed_statement
    : 'fixed' '(' pointer_type fixed_pointer_declarators ')' embedded_statement
    ;

fixed_pointer_declarators
    : fixed_pointer_declarator (','  fixed_pointer_declarator)*
    ;

fixed_pointer_declarator
    : identifier '=' fixed_pointer_initializer
    ;

fixed_pointer_initializer
    : '&' variable_reference
    | expression
    ;
```

每个*fixed_pointer_declarator*声明的局部变量给定*pointer_type* ，并使用计算相应的地址初始化该本地变量*fixed_pointer_initializer*。 声明中的本地变量`fixed`语句是在任何可访问*fixed_pointer_initializer*出现在该变量的声明，并在右侧*embedded_statement*的`fixed`语句。 声明的局部变量`fixed`语句被视为只读的。 如果嵌入的语句试图修改此本地变量，会发生编译时错误 (通过分配或`++`并`--`运算符) 或将其作为传递`ref`或`out`参数。

一个*fixed_pointer_initializer*可以是以下之一：

*  令牌"`&`"后跟*variable_reference* ([精确规则，用于确定明确的赋值](variables.md#precise-rules-for-determining-definite-assignment)) 为可移动的变量 ([固定和可移动变量](unsafe-code.md#fixed-and-moveable-variables))非托管类型的`T`，前提是类型`T*`隐式转换为指针类型中给出`fixed`语句。 在这种情况下，初始值设定项计算给定变量的地址，而变量将保证的持续时间内保持在固定地址`fixed`语句。
*  表达式*array_type*非托管类型的元素`T`，前提是类型`T*`隐式转换为指针类型中给出`fixed`语句。 在这种情况下，初始值设定项计算数组中的第一个元素的地址，并保证整个数组的持续时间内保持在固定地址`fixed`语句。 如果数组表达式为 null 或该数组包含零个元素，该初始值设定项计算地址等于零。
*  类型的表达式`string`，前提是类型`char*`隐式转换为指针类型中给出`fixed`语句。 在这种情况下，初始值设定项计算的第一个字符在字符串中的地址，并保证整个字符串的持续时间内保持在固定地址`fixed`语句。 行为`fixed`语句是实现定义的字符串表达式为 null 时。
*  一个*simple_name*或*member_access*提供固定的大小缓冲区成员的类型为隐式转换为给定指针类型引用的可移动的变量，固定的大小缓冲区成员在`fixed`语句。 在这种情况下，初始值设定项计算第一个元素的固定的大小缓冲区的指针 ([表达式中固定大小的缓冲区](unsafe-code.md#fixed-size-buffers-in-expressions))，和固定的大小缓冲区被保证期间保持在固定地址`fixed`语句。

通过计算每个地址*fixed_pointer_initializer* `fixed`语句可确保由该地址引用该变量不受重定位或的持续时间由垃圾回收器可供使用`fixed`语句。 例如，如果该地址由计算*fixed_pointer_initializer*引用对象的字段或数组实例，元素`fixed`语句可保证包含的对象实例没有重定位或释放该语句的生存期内。

它是程序员有责任确保通过创建了指针`fixed`语句执行这些语句之后不再存在。 例如，指针创建的`fixed`语句传递给外部 Api，它是程序员有责任确保 Api 保留这些指针没有内存。

固定的对象可能会导致的堆碎片 （因为它们不能移动）。 因此，仅在绝对必要时才应修复对象，然后仅为最短的时间量。

该示例

```csharp
class Test
{
    static int x;
    int y;

    unsafe static void F(int* p) {
        *p = 1;
    }

    static void Main() {
        Test t = new Test();
        int[] a = new int[10];
        unsafe {
            fixed (int* p = &x) F(p);
            fixed (int* p = &t.y) F(p);
            fixed (int* p = &a[0]) F(p);
            fixed (int* p = a) F(p);
        }
    }
}
```

演示的几种用法`fixed`语句。 第一条语句修补程序，并获取静态字段的地址、 第二个语句修补程序，并获取实例字段的地址和第三个语句修补程序，并获取数组元素的地址。 每种情况下起来可能会使用常规错误`&`运算符，因为这些变量都属于可移动的变量。

第四个`fixed`语句在上面的示例生成到第三个类似的结果。

此示例中的`fixed`语句使用`string`:

```csharp
class Test
{
    static string name = "xx";

    unsafe static void F(char* p) {
        for (int i = 0; p[i] != '\0'; ++i)
            Console.WriteLine(p[i]);
    }

    static void Main() {
        unsafe {
            fixed (char* p = name) F(p);
            fixed (char* p = "xx") F(p);
        }
    }
}
```

在不安全的上下文中的一维数组的数组元素存储在不断增加的索引顺序，从索引中`0`，以索引结束`Length - 1`。 对于多维数组，数组元素的存储，以便首先，增加的最右边的维度索引然后下一步左边的维度，依此类推左侧。 内`fixed`获取的指针的语句`p`数组实例`a`中，指针值范围从`p`到`p + a.Length - 1`表示数组中元素的地址。 同样，从范围的变量`p[0]`到`p[a.Length - 1]`表示实际的数组元素。 给定数组存储的方式，我们可以处理，就好像它是线性的任何维度的数组。

例如：

```csharp
using System;

class Test
{
    static void Main() {
        int[,,] a = new int[2,3,4];
        unsafe {
            fixed (int* p = a) {
                for (int i = 0; i < a.Length; ++i)    // treat as linear
                    p[i] = i;
            }
        }

        for (int i = 0; i < 2; ++i)
            for (int j = 0; j < 3; ++j) {
                for (int k = 0; k < 4; ++k)
                    Console.Write("[{0},{1},{2}] = {3,2} ", i, j, k, a[i,j,k]);
                Console.WriteLine();
            }
    }
}
```

这会生成输出：

```
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

在示例

```csharp
class Test
{
    unsafe static void Fill(int* p, int count, int value) {
        for (; count != 0; count--) *p++ = value;
    }

    static void Main() {
        int[] a = new int[100];
        unsafe {
            fixed (int* p = a) Fill(p, 100, -1);
        }
    }
}
```

`fixed`语句用于修复一个数组，因此，其地址可以传递给采用指针的方法。

在下面的示例中：

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    Font f;

    unsafe static void Main()
    {
        Test test = new Test();
        test.f.size = 10;
        fixed (char* p = test.f.name) {
            PutString("Times New Roman", p, 32);
        }
    }
}
```

fixed 的语句用于修复固定的大小缓冲区的结构，因此，其地址可以用作指针。

一个`char*`产生的固定字符串实例始终指向以 null 结尾的字符串值。 获取的指针的固定语句中`p`到字符串实例`s`中，指针值范围从`p`到`p + s.Length - 1`表示字符串和指针值中的字符的地址`p + s.Length`始终指向 null 字符 (具有值的字符`'\0'`)。

修改对象的托管类型通过固定指针会导致未定义的行为。 例如，由于字符串是不可变的它是程序员的责任，以确保不会修改等于固定字符串的指针所引用的字符。

调用都应为"C 样式"字符串的外部 Api 时，自动的 null 终止的字符串是特别方便。 但是，请注意，字符串实例，允许以包含 null 字符。 如果存在此类为 null 字符，字符串将出现截断，当被视为 null 值结束`char*`。

## <a name="fixed-size-buffers"></a>固定的大小缓冲区

固定的大小的缓冲区用于将"C 样式"行中数组声明为成员的结构，并且主要用于与非托管 Api 进行连接。

### <a name="fixed-size-buffer-declarations"></a>固定的大小缓冲区的声明

一个***固定大小缓冲区***是表示对于给定类型的变量的固定的长度缓冲区的存储空间的成员。 固定的大小缓冲区声明引入了给定的元素类型的一个或多个固定的大小的缓冲区。 固定的大小缓冲区只能在结构声明中使用和只能发生在不安全的上下文 ([不安全的上下文](unsafe-code.md#unsafe-contexts))。

```antlr
struct_member_declaration_unsafe
    : fixed_size_buffer_declaration
    ;

fixed_size_buffer_declaration
    : attributes? fixed_size_buffer_modifier* 'fixed' buffer_element_type fixed_size_buffer_declarator+ ';'
    ;

fixed_size_buffer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'unsafe'
    ;

buffer_element_type
    : type
    ;

fixed_size_buffer_declarator
    : identifier '[' constant_expression ']'
    ;
```

固定的大小缓冲区声明可能包括一组属性 ([特性](attributes.md))、 一个`new`修饰符 ([修饰符](classes.md#modifiers))，四种访问修饰符的有效组合 ([类型参数和约束](classes.md#type-parameters-and-constraints)) 和一个`unsafe`修饰符 ([不安全的上下文](unsafe-code.md#unsafe-contexts))。 特性和修饰符适用于所有固定的大小缓冲区声明由声明的成员。 它是同一修饰符在固定的大小缓冲区声明中多次出现的错误的。

不允许使用固定的大小缓冲区声明以包括`static`修饰符。

缓冲区元素类型的固定的大小缓冲区声明指定该声明引入的元素类型的缓冲区。 缓冲区元素类型必须是一个预定义的类型`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`， `double`，或`bool`。

缓冲区元素类型后跟一系列固定的大小缓冲区的声明符，其中每个引入了新的成员。 固定的大小缓冲区声明符包含名称的成员后, 跟一个常量表达式括起来的标识符`[`和`]`令牌。 常量表达式表示引入该固定的大小缓冲区声明符的成员中的元素数。 常量表达式的类型必须是隐式转换为键入`int`，其值必须为非零的正整数。

固定的大小缓冲区的元素都保证在内存中顺序依次布局。

声明多个固定的大小缓冲区的固定的大小缓冲区声明是等效于多个具有相同的属性和元素类型的单个固定的大小缓冲区声明的声明。 例如

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

等效于

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a>在表达式中的固定的大小缓冲区

成员查找 ([运算符](expressions.md#operators)) 的固定大小缓冲区成员将继续与成员查找字段的完全相同。

可以在表达式中使用引用的固定的大小缓冲区*simple_name* ([类型推理](expressions.md#type-inference)) 或*member_access* ([编译时检查动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution))。

当固定的大小缓冲区成员作为简单名称引用时，效果等同于窗体的成员访问`this.I`，其中`I`是固定的大小缓冲区成员。

在窗体的成员访问`E.I`，如果`E`的结构类型和成员的查找`I`，结构类型标识固定的大小成员，然后`E.I`是评估分类，如下所示：

*  如果表达式`E.I`不会出现在不安全上下文中，将发生编译时错误。
*  如果`E`分类为一个值，则发生编译时错误。
*  否则为如果`E`是可移动变量 ([固定和可移动变量](unsafe-code.md#fixed-and-moveable-variables)) 和表达式`E.I`不是*fixed_pointer_initializer* ([固定语句](unsafe-code.md#the-fixed-statement))，则发生编译时错误。
*  否则为`E`引用为固定的变量和表达式的结果是指向第一个元素的固定的大小缓冲区成员`I`中`E`。 结果为类型`S*`，其中`S`的元素类型是`I`，和分类为一个值。

可以使用从第一个元素的指针操作访问的固定的大小缓冲区的后续元素。 与不同于数组的访问权限，对固定的大小缓冲区的元素的访问是不安全的操作并不是选中的范围。

下面的示例声明并使用具有固定的大小缓冲区成员的结构。

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    unsafe static void Main()
    {
        Font f;
        f.size = 10;
        PutString("Times New Roman", f.name, 32);
    }
}
```

### <a name="definite-assignment-checking"></a>明确赋值检查

固定的大小的缓冲区将不明确的赋值检查 ([明确赋值](variables.md#definite-assignment))，和固定的大小缓冲区成员被检查的结构类型的变量明确赋值的目的。

静态变量，一个类实例或数组元素的一个实例变量的固定的大小缓冲区成员包含的最外层结构变量时，固定的大小缓冲区的元素将自动初始化为其默认值 ([默认值](variables.md#default-values))。 在所有其他情况下，固定的大小缓冲区的初始内容未定义。

## <a name="stack-allocation"></a>堆栈分配

在不安全上下文中，本地变量声明 ([局部变量声明](statements.md#local-variable-declarations)) 可能包括从调用堆栈中分配内存的堆栈分配初始值设定项。

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

*Unmanaged_type*指示将在新分配的位置中，存储的项的类型和*表达式*指示这些项的数目。 合起来看，这些名称指定的所需的分配大小。 由于堆栈分配的大小不能为负数，它是编译时错误，以指定的项作为数*constant_expression*的计算结果为负值。

在窗体的堆栈分配初始值设定项`stackalloc T[E]`需要`T`为非托管的类型 ([指针类型](unsafe-code.md#pointer-types)) 和`E`是类型的表达式`int`。 构造分配`E * sizeof(T)`个字节从调用堆栈，并返回类型的指针`T*`，为新分配的块。 如果`E`为负值，则该行为不确定。 如果`E`是零，则不进行任何分配，并且返回的指针是实现定义的。 如果没有足够内存可用于分配指定大小的块`System.StackOverflowException`引发。

新分配的内存的内容未定义。

中不允许使用堆栈分配初始值设定项`catch`或`finally`块 ([try 语句](statements.md#the-try-statement))。

没有方法来显式释放内存分配使用`stackalloc`。 该函数成员返回时，会自动丢弃函数成员的执行过程中创建的所有堆栈中分配的内存块。 这对应于`alloca`函数、 C 和 c + + 实现中常见的扩展。

在示例

```csharp
using System;

class Test
{
    static string IntToString(int value) {
        int n = value >= 0? value: -value;
        unsafe {
            char* buffer = stackalloc char[16];
            char* p = buffer + 16;
            do {
                *--p = (char)(n % 10 + '0');
                n /= 10;
            } while (n != 0);
            if (value < 0) *--p = '-';
            return new string(p, 0, (int)(buffer + 16 - p));
        }
    }

    static void Main() {
        Console.WriteLine(IntToString(12345));
        Console.WriteLine(IntToString(-999));
    }
}
```

`stackalloc`初始值设定项中使用`IntToString`方法来分配在堆栈上的 16 个字符的缓冲区。 该方法返回时，缓冲区将自动被丢弃。

## <a name="dynamic-memory-allocation"></a>动态内存分配

除`stackalloc`运算符，C# 提供任何预定义的构造管理非垃圾收集的内存。 通常，此类服务提供的支持类库或直接从基础操作系统导入。 例如，`Memory`以下类说明了可以如何从 C# 访问基础操作系统的堆函数：

```csharp
using System;
using System.Runtime.InteropServices;

public unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    static int ph = GetProcessHeap();

    // Private instance constructor to prevent instantiation.
    private Memory() {}

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size) {
        void* result = HeapAlloc(ph, HEAP_ZERO_MEMORY, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count) {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd) {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd) {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block) {
        if (!HeapFree(ph, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size) {
        void* result = HeapReAlloc(ph, HEAP_ZERO_MEMORY, block, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block) {
        int result = HeapSize(ph, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    static extern int GetProcessHeap();

    [DllImport("kernel32")]
    static extern void* HeapAlloc(int hHeap, int flags, int size);

    [DllImport("kernel32")]
    static extern bool HeapFree(int hHeap, int flags, void* block);

    [DllImport("kernel32")]
    static extern void* HeapReAlloc(int hHeap, int flags, void* block, int size);

    [DllImport("kernel32")]
    static extern int HeapSize(int hHeap, int flags, void* block);
}
```

使用的示例，`Memory`类如下：

```csharp
class Test
{
    static void Main() {
        unsafe {
            byte* buffer = (byte*)Memory.Alloc(256);
            try {
                for (int i = 0; i < 256; i++) buffer[i] = (byte)i;
                byte[] array = new byte[256];
                fixed (byte* p = array) Memory.Copy(buffer, p, 256); 
            }
            finally {
                Memory.Free(buffer);
            }
            for (int i = 0; i < 256; i++) Console.WriteLine(array[i]);
        }
    }
}
```

示例分配 256 个字节的内存通过`Memory.Alloc`和增加从 0 到 255 的值初始化内存块。 然后会分配 256 个元素字节数组，并使用`Memory.Copy`内存块的内容复制到的字节数组。 最后，释放内存块时使用`Memory.Free`和字节数组的内容可能会在控制台上的输出。
