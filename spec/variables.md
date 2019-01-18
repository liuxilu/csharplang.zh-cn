---
ms.openlocfilehash: b7bb7dd575d9e2e6d5dd85bdd3e535411e29fcf4
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229510"
---
# <a name="variables"></a>变量

变量表示存储位置。 每个变量具有类型，用于确定值可以是存储在变量中。 C# 是类型安全语言和 C# 编译器保证在将存储在变量中的值始终是适当的类型。 可以更改变量的值，通过分配或通过使用`++`和`--`运算符。

变量必须是***明确赋值***([明确赋值](variables.md#definite-assignment)) 可以获取其值之前。

如以下各节中所述，变量是***最初分配***或***最初未分配***。 最初分配的变量具有定义完善的初始值，并始终被视为已明确赋值。 最初未赋值的变量具有无初始值。 若要在某个位置被视为明确赋值最初未赋值的变量，向变量赋值必须指向该位置的每个可能的执行路径中。

## <a name="variable-categories"></a>变量类别

C# 定义的变量的七种类别： 静态变量、 实例变量、 数组元素、 值参数、 引用参数、 输出参数和局部变量。 以下各节介绍了每个类别。

在示例
```csharp
class A
{
    public static int x;
    int y;

    void F(int[] v, int a, ref int b, out int c) {
        int i = 1;
        c = a + b++;
    }
}
```
`x` 是一个静态变量，`y`是一个实例变量，`v[0]`是一个数组元素，`a`是值参数，而`b`是一个引用参数，`c`是一个 output 参数，和`i`是一个本地变量。

### <a name="static-variables"></a>静态变量

使用字段声明`static`调用修饰符***静态变量***。 静态变量之前就存在了静态构造函数的执行 ([静态构造函数](classes.md#static-constructors)) 为其包含类型和不再存在时关联的应用程序域将不再存在。

静态变量的初始值是默认值 ([默认值](variables.md#default-values)) 的变量的类型。

为了明确赋值检查，静态变量被视为最初分配。

### <a name="instance-variables"></a>实例变量

字段声明而无需`static`调用修饰符***实例变量***。

#### <a name="instance-variables-in-classes"></a>中的类的实例变量

类的一个实例变量时，存在到该类的新实例创建，并且将不再存在时对该实例的引用并且已执行实例的析构函数 （如果有）。

类的一个实例变量的初始值是默认值 ([默认值](variables.md#default-values)) 的变量的类型。

为了明确赋值检查，类的一个实例变量被视为最初分配。

#### <a name="instance-variables-in-structs"></a>结构中的实例变量

结构的一个实例变量具有其所属的结构变量的生存期完全相同。 换而言之，当结构类型的变量进入是否存在或不再存在，因此也执行该结构的实例变量。

包含结构变量相同的结构实例变量的初始分配状态。 换而言之，何时结构变量被视为最初分配的因此也是它的实例变量和结构变量被视为最初未赋值，它的实例变量已赋值。

### <a name="array-elements"></a>数组元素

数组的元素创建数组实例后，就应该考虑存在并在没有对该数组实例的引用时停止存在。

每个数组的元素的初始值是默认值 ([默认值](variables.md#default-values)) 的数组元素的类型。

为了明确赋值检查，数组元素被视为最初分配。

### <a name="value-parameters"></a>值参数

无需声明的参数`ref`或`out`修饰符***value 参数***。

Value 参数开始的函数成员 （方法、 实例构造函数、 访问器或运算符） 或匿名函数的调用时存在于该参数属于和初始化的调用中给定的参数值。 值参数通常将不再存在时返回的匿名函数的函数成员。 但是，如果值参数所捕获的匿名函数 ([匿名函数表达式](expressions.md#anonymous-function-expressions))、 其生存时间将至少延长到委托或从该匿名函数创建表达式树不符合条件垃圾回收。

为了明确赋值检查，value 参数被视为最初分配。

### <a name="reference-parameters"></a>引用参数

与声明的参数`ref`修饰符***引用参数***。

引用参数不会创建新的存储位置。 相反，引用参数表示相同的存储位置与在匿名函数调用的函数成员作为参数给出的变量。 因此，引用参数的值始终是相同的基础变量。

以下的明确赋值规则适用于引用参数。 请注意输出参数中所述的不同规则[输出参数](variables.md#output-parameters)。

*  变量必须明确赋值 ([明确赋值](variables.md#definite-assignment)) 它可以作为引用参数的函数成员或委托调用中传递之前。
*  在内部函数成员或匿名函数，引用参数被视为最初分配。

实例方法或结构类型的实例访问器内`this`关键字的行为与完全相同的结构类型的引用参数 ([此访问权限](expressions.md#this-access))。

### <a name="output-parameters"></a>输出参数

与声明的参数`out`修饰符***输出参数***。

输出参数不会创建新的存储位置。 而是输出参数表示相同的存储位置与作为函数成员或委托调用中的参数给出的变量。 因此，输出参数的值始终是相同的基础变量。

以下的明确赋值规则适用于输出参数。 请注意引用参数中所述的不同规则[引用参数](variables.md#reference-parameters)。

*  变量在其可以作为函数成员中的输出参数传递或委托调用之前不需要明确赋值。
*  在函数成员或委托调用正常完成，因为被认为是 output 参数传递的每个变量赋值的执行路径中。
*  在函数成员或匿名函数，输出参数被视为最初未赋值。
*  必须明确赋值函数成员或匿名函数的每个输出参数 ([明确赋值](variables.md#definite-assignment)) 之前该函数成员或匿名函数正常返回。

结构类型的实例构造函数内`this`关键字的行为与完全相同的输出参数的结构类型 ([此访问权限](expressions.md#this-access))。

### <a name="local-variables"></a>局部变量

一个***局部变量***来声明*local_variable_declaration*，这可能是在*块*、 *for_statement*， *switch_statement*或*using_statement*; 或通过*foreach_statement*或者*specific_catch_clause*为*try_statement*。

本地变量的生存期是在此期间保证存储可为其保留的程序执行的部分。 此生命周期中的条目从至少持续*块*， *for_statement*， *switch_statement*， *using_statement*， *foreach_statement*，或*specific_catch_clause*与它关联，执行，直到*块*， *for_statement*， *switch_statement*， *using_statement*， *foreach_statement*，或*specific_catch_clause*以任何方式结束。 (输入括住*块*或调用方法将挂起，但不结束当前的执行*块*， *for_statement*， *switch_statement*， *using_statement*， *foreach_statement*，或*specific_catch_clause*。)如果匿名函数捕获本地变量 ([捕获外部变量](expressions.md#captured-outer-variables))，其生存期内将至少延长到从匿名函数，以及到出现的任何其他对象创建的委托或表达式树引用捕获的变量，可以进行垃圾回收。

如果父级*块*， *for_statement*， *switch_statement*， *using_statement*， *foreach_statement*，或*specific_catch_clause*输入以递归方式，每次创建是有本地变量的新实例并将其*local_variable_initializer*，如果计算每次。

引入局部变量*local_variable_declaration*不会自动初始化，因此没有默认值。 为了明确赋值检查，本地变量引入*local_variable_declaration*被视为最初未赋值。 一个*local_variable_declaration*可能包括*local_variable_initializer*，在这种情况下该变量明确赋值被视为仅在初始化表达式后 ([声明语句](variables.md#declaration-statements))。

引入本地变量的作用域内*local_variable_declaration*，它会导致编译时错误到该本地变量的文本的位置之前，请参阅其*local_variable_declarator*. 如果本地变量声明是隐式 ([局部变量声明](statements.md#local-variable-declarations))，它也是错误引用中的变量及其*local_variable_declarator*。

引入局部变量*foreach_statement*或*specific_catch_clause*在其整个范围内被视为明确赋值。

本地变量的实际生存期是依赖于实现的。 例如，编译器可能会以静态方式确定块中的本地变量仅用于该块的一小部分。 使用此分析，编译器无法生成结果变量的生存期短于包含的块的存储中的代码。

由局部引用变量引用的存储与该局部引用变量的生存期无关的回收 ([自动内存管理](basic-concepts.md#automatic-memory-management))。

## <a name="default-values"></a>默认值

以下类别的变量将自动初始化为其默认值：

*  静态变量。
*  类实例的实例变量。
*  数组元素。

变量的默认值取决于变量的类型，并按如下所示确定：

*  变量的*value_type*，默认值是通过计算出来的值与相同*value_type*的默认构造函数 ([默认构造函数](types.md#default-constructors))。
*  变量的*reference_type*，默认值是`null`。

初始化为默认值通常是通过让内存管理器或垃圾回收器内存初始化为所有位归零之前已分配供使用。 出于此原因，是可以方便地使用所有位归零来表示 null 引用。

## <a name="definite-assignment"></a>明确赋值

在给定位置中的可执行代码的函数成员，变量称为***明确赋值***可以证明编译器，由特定的静态流分析 ([精确规则，用于确定明确分配](variables.md#precise-rules-for-determining-definite-assignment))，该变量会自动初始化或已至少一个赋值的目标。 非正式地讲，是赋值的明确的规则：

*  最初分配的变量 ([最初分配变量](variables.md#initially-assigned-variables)) 始终被视为明确赋值。
*  最初未赋值的变量 ([最初未分配的变量](variables.md#initially-unassigned-variables)) 如果被视为明确赋值在给定位置指向该位置的所有可能的执行路径包含至少一个以下：
    * 简单赋值 ([简单的赋值](expressions.md#simple-assignment)) 中的变量是左的操作数。
    * 调用表达式 ([调用表达式](expressions.md#invocation-expressions)) 或对象创建表达式 ([对象创建表达式](expressions.md#object-creation-expressions)) 将变量传递作为输出参数。
    * 对于本地变量，本地变量声明 ([局部变量声明](statements.md#local-variable-declarations))，其中包含变量的初始值设定项。

基础上述非正式规则的正式规范中所述[最初分配变量](variables.md#initially-assigned-variables)，[最初未分配的变量](variables.md#initially-unassigned-variables)，和[精确规则，用于确定明确赋值](variables.md#precise-rules-for-determining-definite-assignment)。

实例变量明确赋值状态*struct_type*单独以及统一跟踪的变量。 在其他规则更高版本，以下规则应用于*struct_type*变量和其实例的变量：

*  一个实例变量被视为明确赋值，如果其包含*struct_type*变量被视为已明确赋值。
*  一个*struct_type*变量被视为明确赋值，如果它的实例变量的每个被视为明确赋值。

明确赋值是一项要求以下上下文中：

*  变量必须在其值获取其中每个位置中明确赋值。 这可确保永远不会发生未定义的值。 在表达式中的变量的匹配项被视为要获取变量的值，除非当
    * 该变量是分配的一个简单的左的操作数
    * 该变量传递作为输出参数，或
    * 该变量是*struct_type*变量，并且发生作为成员访问的左操作数。
*  变量必须在其作为引用参数传递的每个位置中明确赋值。 这可确保所调用的函数成员可以考虑最初分配的引用参数。
*  必须在其中的函数成员返回每个位置明确赋值的函数成员的所有输出参数 (通过`return`语句，或者通过执行的函数成员正文结束)。 这可确保，成员函数不返回未定义的值在输出参数中，从而使编译器可以考虑将作为输出参数的变量等效于向变量赋值的函数成员调用。
*  `this`的变量*struct_type*实例构造函数都必须在该实例构造函数将返回其中每个位置明确赋值。

### <a name="initially-assigned-variables"></a>最初已分配的变量

最初分配分类变量的以下类别：

*  静态变量。
*  类实例的实例变量。
*  最初分配的结构变量的实例变量。
*  数组元素。
*  值的参数。
*  引用参数。
*  中声明的变量`catch`子句或`foreach`语句。

### <a name="initially-unassigned-variables"></a>最初未赋值的变量

以下类别的变量被归类为最初未分配：

*  变量最初未分配的结构变量的实例。
*  输出参数，包括`this`结构实例构造函数的变量。
*  本地变量，除了那些中声明`catch`子句或`foreach`语句。

### <a name="precise-rules-for-determining-definite-assignment"></a>精确规则，用于确定明确的赋值

若要确定每个使用的变量明确赋值，编译器必须使用等效于在本部分中所述的过程。

编译器在处理每个具有一个或多个最初未赋值的变量的函数成员的正文。 对于每个最初未赋值变量*v*，编译器确定***明确赋值状态***有关*v*在每个函数成员中的以下几点：

*  每个语句的开头
*  在终结点 ([终结点和可访问性](statements.md#end-points-and-reachability)) 的每个语句
*  在每个弧线上将控制转移到另一个语句或语句的终结点
*  在每个表达式的开头
*  每个表达式的末尾

明确赋值国*v*可以是：

*  明确赋值。 这指示在到目前为止，所有可能的控制流上*v*已分配有一个值。
*  不明确赋值。 变量类型的表达式结尾处的状态`bool`，未明确赋值可能 （但不一定） 的变量的状态划分为以下子状态之一：
    * 明确赋值后，则返回 true 的表达式。 此状态表明*v*如果布尔表达式计算为 true，但如果布尔表达式的计算结果为 false 则不一定要分配明确赋值。
    * 明确赋值后 false 的表达式。 此状态表明*v*如果布尔表达式计算结果为 false，但如果布尔表达式计算为 true 则不一定要分配明确赋值。

以下规则控制如何将变量的状态*v*在每个位置确定。

#### <a name="general-rules-for-statements"></a>语句的一般规则

*  *v*开头的函数成员正文中未明确赋值。
*  *v*明确赋值无法访问的任何语句开头。
*  明确赋值国*v*任何其他语句开始处通过检查的明确赋值状态来确定*v*上为目标的开头的所有控制流传输语句。 当 （且仅当） *v*明确赋值的所有这些控制流传输，然后*v*明确赋值语句的开始处。 用于检查语句可访问性的相同方式确定可能的控制流传输的集 ([终结点和可访问性](statements.md#end-points-and-reachability))。
*  明确赋值国*v*在块中，终结点`checked`， `unchecked`， `if`， `while`， `do`， `for`， `foreach`， `lock`， `using`，或`switch`通过检查的明确赋值状态来确定语句*v*面向该语句的结束点的所有控制流传输。 如果*v*明确赋值的所有这些控制流传输，然后*v*明确赋值语句的结束点。 否则为*v*在终结点的语句不明确赋值。 用于检查语句可访问性的相同方式确定可能的控制流传输的集 ([终结点和可访问性](statements.md#end-points-and-reachability))。

#### <a name="block-statements-checked-and-unchecked-statements"></a>选中之后，块语句和 unchecked 的语句

明确赋值国*v*在控件上传输到第一个语句块中的语句列表 （或块，如果语句列表为空的终结点） 等同于明确的赋值语句*v*块的前面`checked`，或`unchecked`语句。

#### <a name="expression-statements"></a>表达式语句

表达式语句*stmt*表达式组成*expr*:

*  *v*具有的开始处的同一个明确的赋值状态*expr*作为开头*stmt*。
*  如果*v*如果明确赋值的结尾处*expr*，在终结点的明确赋值*stmt*; 否则为未明确赋值在终结点*stmt*。

#### <a name="declaration-statements"></a>声明语句

*  如果*stmt*是声明语句没有初始值设定项，然后*v*具有在的终结点的同一个明确的赋值状态*stmt*作为开头*stmt*。
*  如果*stmt*是明确的赋值状态的声明语句具有初始值设定项，然后*v*决定好像*stmt*是一个语句列表，其中一个分配对于每个声明使用初始值设定项 （按顺序列出声明） 的语句。

#### <a name="if-statements"></a>如果语句

有关`if`语句*stmt*窗体：
```csharp
if ( expr ) then_stmt else else_stmt
```

*  *v*具有的开始处的同一个明确的赋值状态*expr*作为开头*stmt*。
*  如果*v*末尾的明确赋值*expr*，然后它指向的控制流转移上明确赋值*then_stmt* ，并*else_stmt*或到的终结点*stmt*如果没有其他子句。
*  如果*v*末尾处具有"明确赋值后，则返回 true 的表达式"的状态*expr*，然后它指向的控制流转移上明确赋值*then_stmt*，而不在控制流传输到任何一个上明确赋值*else_stmt*或到的终结点*stmt*如果没有其他子句。
*  如果*v*末尾处具有"明确赋值后 false 的表达式"的状态*expr*，然后它指向的控制流转移上明确赋值*else_stmt*，而不在控制流传输到上明确赋值*then_stmt*。 在终结点的明确赋值*stmt*当且仅当在终结点的明确赋值*then_stmt*。
*  否则为*v*被视为不在控制流传输到任何一个上明确赋值*then_stmt*或*else_stmt*，或向终结点的*stmt*如果没有其他子句。

#### <a name="switch-statements"></a>Switch 语句

在中`switch`语句*stmt*与控制表达式*expr*:

*  明确赋值状态*v*的开头*expr*的状态相同*v*开头*stmt*。
*  明确赋值国*v*上的控制流传输到可访问的 switch 块语句列表是明确的赋值状态的相同*v*末尾的*expr*.

#### <a name="while-statements"></a>While 语句

有关`while`语句*stmt*窗体：
```csharp
while ( expr ) while_body
```

*  *v*具有的开始处的同一个明确的赋值状态*expr*作为开头*stmt*。
*  如果*v*末尾的明确赋值*expr*，然后它指向的控制流转移上明确赋值*while_body*和终结点到*stmt*。
*  如果*v*末尾处具有"明确赋值后，则返回 true 的表达式"的状态*expr*，然后它指向的控制流转移上明确赋值*while_body*，但不是在终结点的明确赋值*stmt*。
*  如果*v*末尾处具有"明确赋值后 false 的表达式"的状态*expr*，则它控制流传输到的终结点上明确赋值*stmt*但不是在控制流传输到明确赋值*while_body*。

#### <a name="do-statements"></a>执行语句

有关`do`语句*stmt*窗体：
```csharp
do do_body while ( expr ) ;
```

*  *v*已从开始处的控制流转移上的同一个明确的赋值状态*stmt*到*do_body*一样的开始处*stmt*。
*  *v*具有的开始处的同一个明确的赋值状态*expr*为终结点*do_body*。
*  如果*v*末尾的明确赋值*expr*，则它控制流传输到的终结点上明确赋值*stmt*。
*  如果*v*末尾处具有"明确赋值后 false 的表达式"的状态*expr*，则它控制流传输到的终结点上明确赋值*stmt*.

#### <a name="for-statements"></a>语句

正在检查的明确赋值`for`窗体的语句：
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
编写该语句已完成：
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

如果*for_condition*中省略`for`语句，然后评估明确赋值将继续像*for_condition*已替换为`true`上述扩展中.

#### <a name="break-continue-and-goto-statements"></a>中断、 继续，请和 goto 语句

明确赋值国*v*上引起的控制流转移`break`， `continue`，或`goto`语句是明确的赋值状态的相同*v*在该语句的起始处。

#### <a name="throw-statements"></a>Throw 语句

语句*stmt*窗体
```csharp
throw expr ;
```

明确赋值状态*v*的开头*expr*是明确的赋值状态的相同*v*开头*stmt*.

#### <a name="return-statements"></a>Return 语句

语句*stmt*窗体
```csharp
return expr ;
```

*  明确赋值状态*v*的开头*expr*是明确的赋值状态的相同*v*开头*stmt*.
*  如果*v*为 output 参数，则它必须绝对分配：
    * 后*expr*
    * 或末尾处`finally`块`try` - `finally`或`try` - `catch` - `finally`包含`return`语句。

为窗体语句 stmt:
```csharp
return ;
```

*  如果*v*为 output 参数，则它必须绝对分配：
    * before *stmt*
    * 或末尾处`finally`块`try` - `finally`或`try` - `catch` - `finally`包含`return`语句。

#### <a name="try-catch-statements"></a>Try catch 语句

语句*stmt*窗体：
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  明确赋值状态*v*的开头*try_block*是明确的赋值状态的相同*v*开头*stmt*.
*  明确赋值国*v*的开头*catch_block_i* (任何*我*) 是明确的赋值状态的相同*v*的开头*stmt*。
*  明确赋值国*v*处的终结点*stmt*是明确赋值的当 （且仅当） *v*明确赋值的结束点处*try_block*和每*catch_block_i* (为每个*我*从 1 到*n*)。

#### <a name="try-finally-statements"></a>Try finally 语句

有关`try`语句*stmt*窗体：
```csharp
try try_block finally finally_block
```

*  明确赋值状态*v*的开头*try_block*是明确的赋值状态的相同*v*开头*stmt*.
*  明确赋值状态*v*的开头*finally_block*是明确的赋值状态的相同*v*开头*stmt*.
*  明确赋值国*v*处的终结点*stmt*是明确赋值的当 （且仅当） 至少下列任一条件为 true:
    * *v*明确赋值的结束点处*try_block*
    * *v*明确赋值的结束点处*finally_block*

如果控制流传输 (例如，`goto`语句) 进行了，则在开始*try_block*，和外部的结束*try_block*，然后*v*也是如果认为是该控制流转移上明确赋值*v*明确赋值的结束点处*finally_block*。 (这不是仅当 — 如果*v*则它仍被视为明确赋值明确赋值上此控制流传输，另一个原因。)

#### <a name="try-catch-finally-statements"></a>Try – catch – finally 语句

为明确的赋值 analysis `try` - `catch` - `finally`窗体的语句：
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
像该语句已完成`try` - `finally`语句封闭`try` - `catch`语句：
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

下面的示例演示如何不同的块`try`语句 ([try 语句](statements.md#the-try-statement)) 会影响明确赋值。
```csharp
class A
{
    static void F() {
        int i, j;
        try {
            goto LABEL;
            // neither i nor j definitely assigned
            i = 1;
            // i definitely assigned
        }

        catch {
            // neither i nor j definitely assigned
            i = 3;
            // i definitely assigned
        }

        finally {
            // neither i nor j definitely assigned
            j = 5;
            // j definitely assigned
            }
        // i and j definitely assigned
        LABEL:;
        // j definitely assigned

    }
}
```

#### <a name="foreach-statements"></a>Foreach 语句

有关`foreach`语句*stmt*窗体：
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  明确赋值状态*v*的开头*expr*的状态相同*v*开头*stmt*。
*  明确赋值国*v*在控制流传输到*embedded_statement*或终结点的*stmt*的状态相同*v*的末尾*expr*。

#### <a name="using-statements"></a>Using 语句

有关`using`语句*stmt*窗体：
```csharp
using ( resource_acquisition ) embedded_statement
```

*  明确赋值状态*v*的开头*resource_acquisition*的状态相同*v*开头*stmt*.
*  明确赋值国*v*在控制流传输到*embedded_statement*的状态相同*v*末尾的*resource_获取*。

#### <a name="lock-statements"></a>Lock 语句

有关`lock`语句*stmt*窗体：
```csharp
lock ( expr ) embedded_statement
```

*  明确赋值状态*v*的开头*expr*的状态相同*v*开头*stmt*。
*  明确赋值国*v*在控制流传输到*embedded_statement*的状态相同*v*末尾的*expr*.

#### <a name="yield-statements"></a>Yield 语句

有关`yield return`语句*stmt*窗体：
```csharp
yield return expr ;
```

*  明确赋值状态*v*的开头*expr*的状态相同*v*开头*stmt*。
*  明确赋值状态*v*末尾处*stmt*的状态相同*v*末尾*expr*。
*  一个`yield break`语句具有明确的赋值状态没有影响。

#### <a name="general-rules-for-simple-expressions"></a>简单表达式的一般规则

以下规则适用于这些类型的表达式： 文本 ([文字](expressions.md#literals))，简单名称 ([简单名称](expressions.md#simple-names))，成员访问表达式 ([成员访问](expressions.md#member-access))，非索引基访问表达式 ([基本访问权限](expressions.md#base-access))，`typeof`表达式 ([typeof 运算符](expressions.md#the-typeof-operator))，默认值表达式 ([默认值表达式](expressions.md#default-value-expressions)) 和`nameof`表达式 ([Nameof 表达式](expressions.md#nameof-expressions))。

*  明确赋值国*v*末尾的此类表达式是明确的赋值状态的相同*v*表达式的开头。

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a>使用嵌入式表达式的表达式的一般规则

以下规则适用于这些类型的表达式： 带括号的表达式 ([带括号的表达式](expressions.md#parenthesized-expressions))，元素访问表达式 ([元素访问](expressions.md#element-access))、 基访问表达式索引 ([基本访问权限](expressions.md#base-access))、 递增和递减表达式 ([后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators)，[前缀递增和递减运算符](expressions.md#prefix-increment-and-decrement-operators))，强制转换表达式 ([强制转换表达式](expressions.md#cast-expressions))，一元`+`， `-`， `~`，`*`表达式、 二进制`+`， `-`， `*`，`/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`， `|`，`^`表达式 ([算术运算符](expressions.md#arithmetic-operators)，[移位运算符](expressions.md#shift-operators)，[关系和类型测试运算符](expressions.md#relational-and-type-testing-operators)[逻辑运算符](expressions.md#logical-operators))，复合赋值表达式 ([复合赋值](expressions.md#compound-assignment))，`checked`并`unchecked`表达式 ([checked 和 unchecked运算符](expressions.md#the-checked-and-unchecked-operators))，以及数组和委托创建表达式 ([new 运算符](expressions.md#the-new-operator))。

每两个表达式都有一个或多个无条件地按固定顺序计算的子表达式。 例如，二进制`%`运算符会求值的左侧的运算符，然后在右侧。 索引操作的索引的表达式的计算结果，然后计算每个索引表达式，从左到右的顺序。 表达式*expr*，其中包含的子表达式*e1、 e2，...，eN*、 按该顺序计算：

*  明确赋值国*v*的开头*e1*相同的开始处的明确赋值状态*expr*。
*  明确赋值国*v*的开头*ei* (*我*大于 1) 等同于上一个子表达式的末尾的明确赋值状态。
*  明确赋值国*v*末尾处*expr*等同于结尾处的明确赋值状态*eN*

#### <a name="invocation-expressions-and-object-creation-expressions"></a>调用表达式和对象创建表达式

调用表达式*expr*窗体：
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
或在窗体的对象创建表达式：
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  调用表达式，明确赋值国*v*之前*primary_expression*的状态相同*v*之前*expr*.
*  调用表达式，明确赋值国*v*之前*arg1*的状态相同*v*后*primary_expression*.
*  为对象创建表达式，明确赋值国*v*之前*arg1*的状态相同*v*之前*expr*。
*  对于每个自变量*argi*，明确赋值国*v*后*argi*由正则表达式规则，忽略任何`ref`或`out`修饰符。
*  对于每个自变量*argi*任何*我*大于 1 的明确赋值状态*v*之前*argi*的状态相同*v*之后的上一个*arg*。
*  如果将变量*v*作为传递`out`自变量 (即，在窗体的自变量`out v`) 中的参数，则状态的任何*v*后*expr*明确赋值。 否则为状态*v*后*expr*的状态相同*v*后*argN*。
*  有关数组初始值设定项 ([数组创建表达式](expressions.md#array-creation-expressions))，对象初始值设定项 ([对象初始值设定项](expressions.md#object-initializers))，集合初始值设定项 ([集合初始值设定项](expressions.md#collection-initializers)) 和匿名对象初始值设定项 ([匿名对象创建表达式](expressions.md#anonymous-object-creation-expressions))，明确赋值状态由这些构造定义的扩展。

#### <a name="simple-assignment-expressions"></a>简单的赋值表达式

表达式*expr*窗体的`w = expr_rhs`:

*  明确赋值国*v*之前*expr_rhs*是明确的赋值状态的相同*v*之前*expr*。
*  明确赋值国*v*后*expr*由：
   * 如果*w*是相同的变量作为*v*，然后的明确赋值状态*v*后*expr*明确赋值。
   * 否则为如果进行结构类型的实例构造函数内分配*w*是指定一个自动实现的属性的属性访问*P*正在构造的实例上并*v*是隐藏的支持字段*P*，然后的明确赋值状态*v*后*expr*绝对分配。
   * 否则为明确赋值国*v*后*expr*是明确的赋值状态的相同*v*后*expr_rhs*。

#### <a name="-conditional-and-expressions"></a>& & (AND 条件) 表达式

表达式*expr*窗体的`expr_first && expr_second`:

*  明确赋值国*v*之前*expr_first*是明确的赋值状态的相同*v*之前*expr*。
*  明确赋值国*v*之前*expr_second*如果明确赋值的状态*v*后*expr_first*是明确赋值或者"明确赋值后，则返回 true 的表达式"。 否则，它就不是明确赋值的。
*  明确赋值国*v*后*expr*由：
    * 如果*expr_first*是常量表达式的值`false`，然后明确赋值的状态*v*后*expr*等同于明确赋值状态*v*后*expr_first*。
    * 否则为如果的状态*v*后*expr_first*未明确赋值，然后的状态*v*后*expr*明确赋值。
    * 否则为如果的状态*v*后*expr_second*明确赋值，和的状态*v*后*expr_first* "肯定是分配后 false 的表达式"，然后的状态*v*后*expr*明确赋值。
    * 否则为如果的状态*v*后*expr_second*明确赋值或"后，则返回 true 的表达式明确赋值"，然后的状态*v*后*expr* "肯定后被分配，则返回 true 的表达式"。
    * 否则为如果的状态*v*后*expr_first*为"false 表达式之后明确赋值"，和状态*v*后*expr_second*为"false 表达式之后明确赋值"，则状态*v*后*expr* "肯定后被分配 false 的表达式"。
    * 否则为状态*v*后*expr*未明确赋值。

在示例
```csharp
class A
{
    static void F(int x, int y) {
        int i;
        if (x >= 0 && (i = y) >= 0) {
            // i definitely assigned
        }
        else {
            // i not definitely assigned
        }
        // i not definitely assigned
    }
}
```
在变量`i`被视为一个嵌入的语句中明确赋值`if`语句但不是在其他。 在`if`方法中的语句`F`，该变量`i`明确赋值中第一条嵌入式语句由于表达式的执行`(i = y)`始终执行前执行此嵌入式语句。 与之相反，变量`i`不明确赋值在第二个嵌入的语句中，由于`x >= 0`可能已测试为 false，从而在变量中`i`未分配。

#### <a name="-conditional-or-expressions"></a>||（条件或） 表达式

表达式*expr*窗体的`expr_first || expr_second`:

*  明确赋值国*v*之前*expr_first*是明确的赋值状态的相同*v*之前*expr*。
*  明确赋值国*v*之前*expr_second*如果明确赋值的状态*v*后*expr_first*是明确赋值或者"明确赋值后 false 的表达式"。 否则，它就不是明确赋值的。
*  明确的赋值语句*v*后*expr*由：
    * 如果*expr_first*是常量表达式的值`true`，然后明确赋值的状态*v*后*expr*等同于明确赋值状态*v*后*expr_first*。
    * 否则为如果的状态*v*后*expr_first*未明确赋值，然后的状态*v*后*expr*明确赋值。
    * 否则为如果的状态*v*后*expr_second*明确赋值，和的状态*v*后*expr_first* "肯定是分配的则返回 true 表达式之后"，然后的状态*v*后*expr*明确赋值。
    * 否则为如果的状态*v*后*expr_second*明确赋值或"false 表达式之后明确赋值"，然后的状态*v*后*expr* "肯定后被分配 false 的表达式"。
    * 否则为如果的状态*v*后*expr_first*是"，则返回 true 表达式之后明确赋值"，和状态*v*后*expr_second*为"true 表达式之后明确赋值"，则状态*v*后*expr* "肯定后被分配，则返回 true 的表达式"。
    * 否则为状态*v*后*expr*未明确赋值。

在示例
```csharp
class A
{
    static void G(int x, int y) {
        int i;
        if (x >= 0 || (i = y) >= 0) {
            // i not definitely assigned
        }
        else {
            // i definitely assigned
        }
        // i not definitely assigned
    }
}
```
在变量`i`被视为一个嵌入的语句中明确赋值`if`语句但不是在其他。 在`if`方法中的语句`G`，该变量`i`因为明确赋值在第二个嵌入的语句中表达式的执行`(i = y)`始终执行前执行此嵌入式语句。 与之相反，变量`i`肯定中未分配的第一个嵌入语句，因为`x >= 0`可能已测试为 true，从而导致变量`i`未分配。

#### <a name="-logical-negation-expressions"></a>! （逻辑非运算） 表达式

表达式*expr*窗体的`! expr_operand`:

*  明确赋值国*v*之前*expr_operand*是明确的赋值状态的相同*v*之前*expr*。
*  明确赋值国*v*后*expr*由：
    * 如果状态*v*后 * expr_operand * 未明确赋值，然后的状态*v*后*expr*明确赋值。
    * 如果状态*v*后 * expr_operand * 不明确赋值，然后的状态*v*后*expr*未明确赋值。
    * 如果状态*v*后 * expr_operand * 为"false 表达式之后明确赋值"，则状态*v*后*expr* "明确赋值后 true表达式"。
    * 如果状态*v*后 * expr_operand * 为"true 表达式之后明确赋值"，则状态*v*后*expr* "明确赋值后 false表达式"。

#### <a name="-null-coalescing-expressions"></a>?? （null 合并） 表达式

表达式*expr*窗体的`expr_first ?? expr_second`:

*  明确赋值国*v*之前*expr_first*是明确的赋值状态的相同*v*之前*expr*。
*  明确赋值国*v*之前*expr_second*是明确的赋值状态的相同*v*后*expr_first*。
*  明确的赋值语句*v*后*expr*由：
    * 如果*expr_first*是常量表达式 ([常量表达式](expressions.md#constant-expressions)) 具有值为 null，则状态*v*后*expr*相同状态*v*后*expr_second*。
*  否则为状态*v*后*expr*是明确的赋值状态的相同*v*后*expr_first*。

#### <a name="-conditional-expressions"></a>？: （条件） 表达式

表达式*expr*窗体的`expr_cond ? expr_true : expr_false`:

*  明确赋值国*v*之前*expr_cond*的状态相同*v*之前*expr*。
*  明确赋值国*v*之前*expr_true*明确赋值的当且仅当以下之一保存：
    * *expr_cond*是具有值的常量表达式 `false`
    * 状态*v*后*expr_cond*明确赋值或"明确赋值后，则返回 true 的表达式"。
*  明确赋值国*v*之前*expr_false*明确赋值的当且仅当以下之一保存：
    * *expr_cond*是具有值的常量表达式 `true`
*  状态*v*后*expr_cond*明确赋值或"明确赋值后 false 的表达式"。
*  明确赋值国*v*后*expr*由：
    * 如果*expr_cond*是常量表达式 ([常量表达式](expressions.md#constant-expressions)) 值`true`然后的状态*v*后*expr*状态相同*v*后*expr_true*。
    * 否则为如果*expr_cond*是常量表达式 ([常量表达式](expressions.md#constant-expressions)) 值`false`然后的状态*v*后*expr*的状态相同*v*后*expr_false*。
    * 否则为如果的状态*v*后*expr_true*明确赋值和状态*v*后*expr_false*绝对分配，则状态*v*后*expr*明确赋值。
    * 否则为状态*v*后*expr*未明确赋值。

#### <a name="anonymous-functions"></a>匿名函数

有关*lambda_expression*或*anonymous_method_expression* *expr*正文 (任一*块*或*表达式*)*正文*:

*  外部变量的明确赋值状态*v*之前*正文*的状态相同*v*之前*expr*。 也就是说的外部变量明确赋值状态继承自的匿名函数的上下文。
*  外部变量的明确赋值状态*v*后*expr*的状态相同*v*之前*expr*。

该示例
```csharp
delegate bool Filter(int i);

void F() {
    int max;

    // Error, max is not definitely assigned
    Filter f = (int n) => n < max;

    max = 5;
    DoWork(f);
}
```
生成编译时错误，因为`max`未明确赋值的匿名函数的声明位置。 该示例
```csharp
delegate void D();

void F() {
    int n;
    D d = () => { n = 1; };

    d();

    // Error, n is not definitely assigned
    Console.WriteLine(n);
}
```
分配到以来还会生成编译时错误`n`匿名函数中产生任何影响明确分配状态的`n`匿名函数的外部。

## <a name="variable-references"></a>变量引用

一个*variable_reference*是*表达式*的分类为变量。 一个*variable_reference*表示来提取当前值并存储一个新值可以访问的存储位置。

```antlr
variable_reference
    : expression
    ;
```

在 C 和 c + + *variable_reference*称为*左值*。

## <a name="atomicity-of-variable-references"></a>变量引用的原子性

是原子操作读取和写入以下数据类型： `bool`， `char`， `byte`， `sbyte`， `short`， `ushort`， `uint`， `int`， `float`，和引用类型。 此外，读取和写入与前面的列表中的基础类型的枚举类型也是原子。 读取和写入的其他类型，包括`long`， `ulong`， `double`，和`decimal`，以及用户定义类型，不保证是原子性。 除了为该目的而设计的库函数，则无法保证的原子读取-修改-写入，例如在递增或递减的情况下。

