---
ms.openlocfilehash: a01cf9387b8dc47de036bf0bd1496c19a441d81c
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876793"
---
# <a name="variables"></a>变量

变量表示存储位置。 每个变量都有一个类型，用于确定可以在变量中存储的值。 C#是一种类型安全的语言， C#编译器保证变量中存储的值始终为适当的类型。 可以通过赋值或使用`++`和`--`运算符来更改变量的值。

在可以获取变量的值之前，必须对其进行***明确***赋值（[明确赋值](variables.md#definite-assignment)）。

如以下各节所述，变量***最初已赋值***或***最初未分配***。 最初分配的变量具有定义完善的初始值，并始终被视为明确赋值。 初始未赋值的变量没有初始值。 对于在某个特定位置被视为明确赋值的初始未赋值的变量，对该变量的赋值必须出现在通向该位置的每个可能的执行路径中。

## <a name="variable-categories"></a>变量类别

C#定义七类变量：静态变量、实例变量、数组元素、值参数、引用参数、输出参数和局部变量。 以下各节介绍了其中的每个类别。

示例中
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
`x`是一个静态变量， `y`它是一个实例变量`v[0]` ，是一个数组元素`a` ，是一个`c`值参数`b` ，是一个引用参数，是一个输出参数`i` ，是一个本地变量.

### <a name="static-variables"></a>静态变量

使用`static`修饰符声明的字段称为***静态变量***。 静态变量在为其包含类型的静态构造函数（[静态构造函数](classes.md#static-constructors)）的执行之前就已存在，如果关联的应用程序域停止存在，则不再存在。

静态变量的初始值是变量类型的默认值（[默认](variables.md#default-values)值）。

出于明确赋值检查的目的，静态变量被视为初始赋值。

### <a name="instance-variables"></a>实例变量

不使用`static`修饰符声明的字段称为***实例变量***。

#### <a name="instance-variables-in-classes"></a>类中的实例变量

当创建该类的新实例时，该类的实例变量就会成为存在，如果没有对该实例的引用，并且已执行了该实例的析构函数（如果有），就不再存在。

类的实例变量的初始值是变量类型的默认值（[默认](variables.md#default-values)值）。

出于明确赋值检查的目的，类的实例变量被视为初始赋值。

#### <a name="instance-variables-in-structs"></a>结构中的实例变量

结构的实例变量与它所属的结构变量的生存期完全相同。 换而言之，当结构类型的变量变为存在或不再存在时，该结构的实例变量也是如此。

结构的实例变量的初始赋值状态与包含结构变量的初始赋值状态相同。 换言之，当结构变量被视为初始已赋值，因此它的实例变量也被视为未赋值时，它的实例变量会同样取消分配。

### <a name="array-elements"></a>数组元素

当创建数组实例时，数组的元素便会存在，并且在没有对该数组实例的引用时停止存在。

数组中每个元素的初始值为数组元素类型的默认值（[默认](variables.md#default-values)值）。

出于明确赋值检查的目的，数组元素被视为初始已赋值。

### <a name="value-parameters"></a>值参数

不带`ref`或`out`修饰符声明的参数是一个***值参数***。

值参数会在调用该参数所属的函数成员（方法、实例构造函数、访问器或运算符）或匿名函数时成为存在，并使用调用中给定的自变量的值进行初始化。 当函数成员或匿名函数返回时，值参数通常不存在。 但是，如果值参数由匿名函数（[匿名函数表达式](expressions.md#anonymous-function-expressions)）捕获，则其生存期将至少扩展到从该匿名函数创建的委托或表达式树符合垃圾回收的条件。

出于明确赋值检查的目的，值参数被视为初始已赋值。

### <a name="reference-parameters"></a>引用参数

使用`ref`修饰符声明的参数是一个***引用参数***。

引用参数不会创建新的存储位置。 相反，引用参数与给定作为函数成员或匿名函数调用中的参数的变量表示相同的存储位置。 因此，引用参数的值始终与基础变量相同。

以下明确赋值规则适用于引用参数。 请注意[输出参数](variables.md#output-parameters)中所述的输出参数的不同规则。

*  在函数成员或委托调用中将变量作为引用参数传递之前，必须对其进行明确赋值（[明确赋值](variables.md#definite-assignment)）。
*  在函数成员或匿名函数中，引用参数被视为初始赋值。

在结构类型的实例方法或实例访问器中，关键字`this`的行为与结构类型（[此访问](expressions.md#this-access)）的引用参数完全相同。

### <a name="output-parameters"></a>输出参数

使用`out`修饰符声明的参数是一个***输出参数***。

Output 参数不会创建新的存储位置。 相反，output 参数表示作为函数成员或委托调用中的参数提供的相同存储位置。 因此，output 参数的值始终与基础变量相同。

以下明确的赋值规则适用于 output 参数。 请注意[引用参数](variables.md#reference-parameters)中所述的引用参数的不同规则。

*  在函数成员或委托调用中将变量作为 output 参数传递之前，不需要明确赋值。
*  完成函数成员或委托调用的正常完成后，作为输出参数传递的每个变量都被视为在该执行路径中分配。
*  在函数成员或匿名函数中，output 参数被视为初始未赋值。
*  函数成员或匿名函数的每个输出参数都必须在函数成员或匿名函数正常返回之前明确赋值（[明确赋值](variables.md#definite-assignment)）。

在结构类型的实例构造函数中，关键字`this`的行为与结构类型的输出参数（[此访问](expressions.md#this-access)）完全相同。

### <a name="local-variables"></a>局部变量

***局部变量***由*local_variable_declaration*声明，该变量可能出现在*块*、 *for_statement*、 *switch_statement*或*using_statement*中。或*try_statement*的*foreach_statement*或*specific_catch_clause* 。

局部变量的生存期是程序执行的一部分，在该过程中，将保证存储的存储空间。 此生存期至少会将输入内容扩展到*块*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*或与其关联的*specific_catch_clause* ，直到*该块*的执行、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*或*specific_catch_clause*以任何方式结束。 （输入封闭的*块*或调用方法会挂起，但不会结束、执行当前*块*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*或*specific_catch_clause*。）如果通过匿名函数（[捕获的外部变量](expressions.md#captured-outer-variables)）捕获本地变量，则其生存期至少将一直扩展到从匿名函数创建的委托或表达式树以及引用捕获的变量适用于垃圾回收。

如果以递归方式进入父*block*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*或*specific_catch_clause* ，则将创建一个新的本地变量实例，每次计算时间及其*local_variable_initializer*（如果有）。

*Local_variable_declaration*引入的本地变量不会自动初始化，因此没有默认值。 出于明确赋值检查的目的， *local_variable_declaration*引入的局部变量被视为初始未分配。 *Local_variable_declaration*可以包括*local_variable_initializer*，在这种情况下，仅在初始化表达式（[声明语句](variables.md#declaration-statements)）后才将变量视为明确赋值。

在*local_variable_declaration*引入的局部变量范围内，在其*local_variable_declarator*之前的文本位置引用该局部变量是编译时错误。 如果局部变量声明是隐式的（[局部变量声明](statements.md#local-variable-declarations)），则在其*local_variable_declarator*中引用该变量也是错误的。

*Foreach_statement*或*specific_catch_clause*引入的局部变量在整个范围内被视为已明确赋值。

局部变量的实际生存期取决于实现。 例如，编译器可能会以静态方式确定块中的某个局部变量仅用于该块的一小部分。 使用此分析，编译器可以生成代码，使变量的存储具有比包含块更短的生存期。

局部引用变量所引用的存储将独立于该本地引用变量的生存期（[自动内存管理](basic-concepts.md#automatic-memory-management)）进行回收。

## <a name="default-values"></a>默认值

以下类别的变量会自动初始化为其默认值：

*  静态变量。
*  类实例的实例变量。
*  数组元素。

变量的默认值取决于变量的类型，并按如下所示确定：

*  对于*value_type*的变量，默认值与*value_type*的默认构造函数（[默认构造函数](types.md#default-constructors)）所计算的值相同。
*  对于*reference_type*的变量，默认值为`null`。

默认值的初始化通常是通过使内存管理器或垃圾回收器将内存初始化为全部位零来完成的，然后再将其分配给使用。 出于此原因，可以方便地使用所有位数表示空引用。

## <a name="definite-assignment"></a>明确赋值

在函数成员的可执行代码中的给定位置 ***，如果编译器***可以通过特定的静态流分析（[确定明确赋值的精确规则](variables.md#precise-rules-for-determining-definite-assignment)）来证明变量已自动初始化或已成为至少一个赋值的目标。 非正式地说，明确赋值的规则如下：

*  最初分配的变量（[最初分配的变量](variables.md#initially-assigned-variables)）始终被视为明确赋值。
*  如果指向该位置的所有可能的执行路径至少包含以下内容之一，则初始未分配的变量（[最初未分配](variables.md#initially-unassigned-variables)的变量）将被视为在给定位置明确赋值：
    * 简单赋值（[简单赋值](expressions.md#simple-assignment)），其中变量是左操作数。
    * 将变量作为输出参数传递的调用表达式（[调用表达式](expressions.md#invocation-expressions)）或对象创建表达式（[对象创建表达式](expressions.md#object-creation-expressions)）。
    * 对于本地变量，则为包含变量初始值设定项的局部变量声明（[局部变量声明](statements.md#local-variable-declarations)）。

[最初分配的变量](variables.md#initially-assigned-variables)、[最初未分配的变量](variables.md#initially-unassigned-variables)和[用于确定明确赋值的精确规则](variables.md#precise-rules-for-determining-definite-assignment)中介绍了上述非正式规则基础的正式规范。

*Struct_type*变量的实例变量的明确赋值状态是单独跟踪的。 除了以上规则，以下规则适用于*struct_type*变量及其实例变量：

*  如果实例变量的包含*struct_type*变量被视为已明确赋值，则将其视为明确赋值。
*  如果将*struct_type*变量的每个实例变量都视为明确赋值，则该变量将被视为明确赋值。

在以下上下文中，明确赋值是必需的：

*  变量必须在获取其值的每个位置明确赋值。 这可确保永远不会出现未定义的值。 表达式中的变量的出现位置被视为获取该变量的值，但在
    * 变量是简单赋值的左操作数，
    * 变量作为 output 参数传递，或
    * 变量是*struct_type*变量，作为成员访问的左操作数出现。
*  变量必须在作为引用参数传递的每个位置上进行明确赋值。 这可确保被调用的函数成员可以考虑最初分配的引用参数。
*  函数成员的所有输出参数都必须在函数成员返回的每个位置明确赋值（通过`return`语句或通过执行到达函数成员主体的末尾）。 这可确保函数成员不在输出参数中返回未定义的值，从而使编译器可以考虑使用变量作为输出参数的函数成员调用，该参数等效于对变量赋值的输出参数。
*  必须在该实例构造函数返回的每个位置明确分配*struct_type*实例构造函数的变量。`this`

### <a name="initially-assigned-variables"></a>最初分配的变量

以下类别的变量被分类为初始分配：

*  静态变量。
*  类实例的实例变量。
*  最初分配的结构变量的实例变量。
*  数组元素。
*  值参数。
*  引用参数。
*  `catch` 子句`foreach`或语句中声明的变量。

### <a name="initially-unassigned-variables"></a>初始未赋值变量

以下类别的变量被分类为初始未赋值：

*  初始未分配的结构变量的实例变量。
*  输出参数，包括`this`结构实例构造函数的变量。
*  局部变量（ `catch`子句`foreach`或语句中声明的变量除外）。

### <a name="precise-rules-for-determining-definite-assignment"></a>确定明确赋值的精确规则

为了确定每个已使用的变量是否已明确赋值，编译器必须使用与本部分中所述等效的进程。

编译器处理每个具有一个或多个初始未赋值变量的函数成员的正文。 对于每个初始未赋值的变量*v*，编译器会在函数成员的以下各点处确定*v*的***明确赋值状态***：

*  在每个语句的开头
*  每个语句的终结点（[终结点和可访问](statements.md#end-points-and-reachability)性）
*  在将控制转移到另一条语句或语句结束点的每个弧线上
*  在每个表达式的开头
*  在每个表达式的末尾

*V*的明确赋值状态可以是：

*  明确赋值。 这表明，在所有可能的控制流到目前为止， *v*已分配有一个值。
*  未明确赋值。 对于类型`bool`为的表达式末尾的变量状态，未明确赋值的变量的状态可能（但不一定）属于以下子状态之一：
    * 在 true 表达式后明确赋值。 此状态表明，如果布尔表达式的计算结果为 true，则会明确赋值*v* ; 如果布尔表达式的计算结果为 false，则不一定赋值。
    * 在 false 表达式后明确赋值。 此状态表明，如果布尔表达式的计算结果为 false，则会明确赋值*v* ; 如果布尔表达式的计算结果为 true，则不一定赋值。

以下规则控制如何在每个位置确定变量*v*的状态。

#### <a name="general-rules-for-statements"></a>语句的一般规则

*  在函数成员主体的开头， *v*未明确赋值。
*  在任何无法访问的语句开始时，均明确赋值*v* 。
*  *V*在任何其他语句开始时的明确赋值状态是通过检查*v*在该语句开头的所有控制流传输上的明确赋值状态。 如果在所有此类控制流传输上明确分配了（且仅当） *v* ，则在语句的开头明确赋值*v* 。 可能的控制流传输集的确定方式与检查语句可访问性（[终结点和可访问](statements.md#end-points-and-reachability)性）相同。
*  *V*在`checked`块、 `do` `unchecked` 、、`if`、、、、、、或的终结点上的明确赋值状态`while` `for` `foreach` `lock` `using` `switch`语句是通过检查以该语句的终结点为目标的所有控制流传输上的*v*的明确赋值状态来确定的。 如果对所有此类控制流传输明确赋值*v* ，则*v*在语句的终结点上是明确赋值的。 本来在语句的结束点上不明确赋值*v* 。 可能的控制流传输集的确定方式与检查语句可访问性（[终结点和可访问](statements.md#end-points-and-reachability)性）相同。

#### <a name="block-statements-checked-and-unchecked-statements"></a>Block 语句、checked 和 unchecked 语句

如果控件传输到块中语句列表的第一条语句（如果语句列表为空，则为到块的终结点），则控件上的*v*的明确赋值状态与块之前的*v*的明确赋值语句相同。、 `checked`或`unchecked`语句。

#### <a name="expression-statements"></a>表达式语句

对于包含表达式*expr*的表达式语句*stmt* ：

*  在*stmt*开始时， *v*在*expr*开头具有相同的明确赋值状态。
*  如果在*expr*的末尾指定了 " *v* "，则它在*stmt*的终结点上进行明确赋值;本来它不是在*stmt*终结点明确赋值的。

#### <a name="declaration-statements"></a>声明语句

*  如果*stmt*是没有初始值设定项的声明语句，则*v*在*stmt*的终结点上的明确赋值状态与*stmt*的开头相同。
*  如果*stmt*是带有初始值设定项的声明语句，则*v*的明确赋值状态被确定为 ，但对于带有初始值设定项的每个声明使用一个赋值语句（顺序为声明）。

#### <a name="if-statements"></a>If 语句

对于窗体的语句`if` stmt：
```csharp
if ( expr ) then_stmt else else_stmt
```

*  在*stmt*开始时， *v*在*expr*开头具有相同的明确赋值状态。
*  如果在*expr*结束时， *v*是明确赋值的，则在到*then_stmt*的控制流传输上，并将其指定为*else_stmt* ，如果没有 else 子句，则将其分配给*stmt*的终结点。
*  如果在*expr*的末尾， *v*的状态为 "的确赋值为 true expression"，则它在控制流到*then_stmt*的传输上明确赋值，而不是在控制流传输上明确分配到任一*else_* 如果没有 else 子句，则为 stmt 或*stmt*的终结点。
*  如果*v*在*expr*的末尾具有状态 "在假表达式后明确赋值"，则它在控制流到*else_stmt*的传输上明确赋值，在控制流到 then_stmt 的传输上未明确赋值。 当且仅当在*then_stmt*的终结点明确赋值时，它才会在*stmt*的终结点上进行明确赋值。
*  否则，在对*then_stmt*或*else_stmt*的控制流传输上， *v*被视为未明确分配，如果不存在 else 子句，则被视为*stmt*的终结点。

#### <a name="switch-statements"></a>Switch 语句

使用控制表达式 *expr*的语句`switch` stmt：

*  *Expr*开头的*v*的明确赋值状态与*stmt*开头的*v*的状态相同。
*  到可访问的 switch 块语句列表的控制流传输上， *v*的明确分配状态与*expr*末尾的*v*的明确赋值状态相同。

#### <a name="while-statements"></a>While 语句

对于窗体的语句`while` stmt：
```csharp
while ( expr ) while_body
```

*  在*stmt*开始时， *v*在*expr*开头具有相同的明确赋值状态。
*  如果在*expr*结束时， *v*是明确赋值的，则它在控制流到*while_body*的传递和到*stmt*的结束点上都是明确赋值的。
*  如果在*expr*的末尾， *v*的状态为 "明确分配给 true 表达式"，则它在控制流到*while_body*的传输上明确赋值，但不是在*stmt*终结点明确赋值。
*  如果在*expr*的末尾， *v*的状态为 "指定为 false expression 后明确赋值"，则它在控制流到*stmt*终点的传输上明确赋值，但并不是在控制流传输时明确赋值。 *_body*。

#### <a name="do-statements"></a>Do 语句

对于窗体的语句`do` stmt：
```csharp
do do_body while ( expr ) ;
```

*  *v*在从*stmt*开始到*do_body*的控制流传输上具有相同的明确赋值状态，就像在*stmt*的开头。
*  对于位于*do_body*的终结点的*expr* ， *v*具有相同的明确赋值状态。
*  如果在*expr*结束时， *v*是明确赋值的，则它在控制流到*stmt*终点的传输上明确赋值。
*  如果在*expr*的末尾， *v*的状态为 "在假表达式后明确赋值"，则它在控制流到*stmt*终点的传输上明确赋值。

#### <a name="for-statements"></a>For 语句

对以下形式的`for`语句的明确赋值检查：
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
像编写语句一样完成：
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

如果`for`语句中省略*for_condition* ，则对明确赋值的计算将继续执行，就像上述扩展`true`中已将*for_condition*替换为。

#### <a name="break-continue-and-goto-statements"></a>Break、continue 和 goto 语句

由 `break`、或语句`goto`导致的控制流传输上的 v 的明确赋值状态与语句开头的 v 的明确赋值状态相同。 `continue`

#### <a name="throw-statements"></a>Throw 语句

对于窗体的语句*stmt*
```csharp
throw expr ;
```

*Expr*开头的*v*的明确赋值状态与*stmt*开头的*v*的明确赋值状态相同。

#### <a name="return-statements"></a>Return 语句

对于窗体的语句*stmt*
```csharp
return expr ;
```

*  *Expr*开头的*v*的明确赋值状态与*stmt*开头的*v*的明确赋值状态相同。
*  如果*v*是 output 参数，则必须为其赋值：
    * *expr*之后
    * 或`finally`包含语句`return`的块`try` - `finally` 的结尾- 。 `try` `catch` - `finally`

对于窗体的语句 stmt：
```csharp
return ;
```

*  如果*v*是 output 参数，则必须为其赋值：
    * *stmt*之前
    * 或`finally`包含语句`return`的块`try` - `finally` 的结尾- 。 `try` `catch` - `finally`

#### <a name="try-catch-statements"></a>Try-catch 语句

对于窗体的语句*stmt* ：
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  在*try_block*开始时， *v*的明确赋值状态与*stmt*开头的*v*的明确赋值状态相同。
*  *Catch_block_i*开头的*v*的明确分配状态（适用于任何*i*）与*stmt*开头的*v*的明确赋值状态相同。
*  如果在*try_block*的终结点和每个 catch_block_i （*对于每个* *，则为*1 到 n 的每个 ）明确分配*v*时，).

#### <a name="try-finally-statements"></a>Try-catch 语句

对于窗体的语句`try` stmt：
```csharp
try try_block finally finally_block
```

*  在*try_block*开始时， *v*的明确赋值状态与*stmt*开头的*v*的明确赋值状态相同。
*  在*finally_block*开始时， *v*的明确赋值状态与*stmt*开头的*v*的明确赋值状态相同。
*  如果（且仅当）满足以下条件之一，则会明确分配*v*在*stmt*终结点上的明确赋值状态：
    * 在*try_block*的终结点明确赋值*v*
    * 在*finally_block*的终结点明确赋值*v*

如果控制`goto`流传输（例如，语句）是在*try_block*中开始，并在*try_block*之外结束，则在该控制流传输上， *v*也被视为明确赋值（如果*v*是在*finally_block*的终结点明确赋值。 （这并不是唯一的情况，如果对此控制流传输的另一个原因明确指定*v* ，则仍将其视为明确分配。）

#### <a name="try-catch-finally-statements"></a>Try-catch-finally 语句

对`try` 以下形式`catch`的语句的`finally`明确赋值分析： - -
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
如`try`语句-是包含语句的`try`语句一样完成： `finally` - `catch`
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

下面的示例演示`try`语句的不同块（[try 语句](statements.md#the-try-statement)）如何影响明确赋值。
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

对于窗体的语句`foreach` stmt：
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  *Expr*开头的*v*的明确赋值状态与*stmt*开头的*v*的状态相同。
*  将控制流传输到*embedded_statement*或*stmt*终结点时， *v*的明确分配状态与*expr*末尾处的*v*状态相同。

#### <a name="using-statements"></a>Using 语句

对于窗体的语句`using` stmt：
```csharp
using ( resource_acquisition ) embedded_statement
```

*  在*resource_acquisition*开始时， *v*的明确赋值状态与*stmt*开头的*v*的状态相同。
*  控制流传输到*embedded_statement*的*v*的明确分配状态与*resource_acquisition*末尾的*v*状态相同。

#### <a name="lock-statements"></a>Lock 语句

对于窗体的语句`lock` stmt：
```csharp
lock ( expr ) embedded_statement
```

*  *Expr*开头的*v*的明确赋值状态与*stmt*开头的*v*的状态相同。
*  控制流传输到*embedded_statement*的*v*的明确分配状态与*expr*末尾处的*v*状态相同。

#### <a name="yield-statements"></a>Yield 语句

对于窗体的语句`yield return` stmt：
```csharp
yield return expr ;
```

*  *Expr*开头的*v*的明确赋值状态与*stmt*开头的*v*的状态相同。
*  在*stmt*结束时， *v*的明确赋值状态与*expr*末尾的*v*状态相同。
*  `yield break`语句对明确赋值状态没有影响。

#### <a name="general-rules-for-simple-expressions"></a>简单表达式的一般规则

以下规则适用于这些类型的表达式：文本（[文本](expressions.md#literals)）、简单名称（[简单名称](expressions.md#simple-names)）、成员访问表达式（[成员访问](expressions.md#member-access)）、非索引的基本访问表达式（[基本访问](expressions.md#base-access)）、 `typeof`表达式（[typeof 运算符](expressions.md#the-typeof-operator)）、默认值表达式（[默认值表达式](expressions.md#default-value-expressions)）和`nameof`表达式（[Nameof 表达式](expressions.md#nameof-expressions)）。

*  此类表达式结束时， *v*的明确赋值状态与表达式开头*v*的明确赋值状态相同。

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a>带有嵌入式表达式的表达式的一般规则

以下规则适用于这些类型的表达式：带圆括号的表达式（[带括号的表达式](expressions.md#parenthesized-expressions)）、元素访问表达式（[元素访问](expressions.md#element-access)）、带有索引的基本访问表达式（[基本访问](expressions.md#base-access)）、增量和减量表达式（[后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators)，[前缀增量和减量运算符](expressions.md#prefix-increment-and-decrement-operators)），强制转换表达式（[强制转换表达式](expressions.md#cast-expressions)） `+`， `-`一元`~`，，， `*`表达式、二进制`+` `-` 、`*` 、`/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`, `|`, `^` 表达式 （[算术运算符](expressions.md#arithmetic-operators)、[移位运算符](expressions.md#shift-operators)、[关系和类型测试运算符](expressions.md#relational-and-type-testing-operators)、[逻辑运算符](expressions.md#logical-operators)）、复合赋值表达式（[复合](expressions.md#compound-assignment) `checked`赋值）和`unchecked`表达式（[checked 和 unchecked 运算符](expressions.md#the-checked-and-unchecked-operators)）以及数组和委托创建表达式（[new 运算符](expressions.md#the-new-operator)）。

其中每个表达式都具有一个或多个按固定顺序无条件计算的子表达式。 例如，二元`%`运算符计算运算符的左侧，然后计算右侧。 索引操作将计算索引表达式的值，然后按从左至右的顺序计算每个索引表达式。 对于具有子表达式*e1、e2、...、eN*的表达式*expr*，按以下顺序计算：

*  在*e1*开始时， *v*的明确赋值状态与*expr*开头的明确赋值状态相同。
*  *Ei* （*i*大于1）开头的*v*的明确赋值状态与上一个子表达式末尾的明确赋值状态相同。
*  在*expr*结束时， *v*的明确赋值状态与*eN*末尾的明确赋值状态相同

#### <a name="invocation-expressions-and-object-creation-expressions"></a>调用表达式和对象创建表达式

对于以下形式的调用表达式*expr* ：
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
或形式的对象创建表达式：
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  对于调用表达式， *v*之前的*primary_expression*的明确赋值状态与*expr*前面的*v*的状态相同。
*  对于调用表达式，v 之前的*v*的明确赋值状态*与* *primary_expression*后*v*的状态相同。
*  对于对象创建表达式，v 之前的*v*的明确赋值状态*与* *expr*前面的*v*的状态相同。
*  对于每个*参数 argi*， *argi*之后的`ref` *v*的明确赋值状态由标准表达式规则确定，忽略任何或`out`修饰符。
*  对于每*个大于一*的参数*argi* ， *argi*之前的*v*的明确赋值状态与上一个*arg*后*v*的状态相同。
*  如果在任何参数中将变量 v `out`作为参数（即， `out v`形式的参数）传递，则*expr*后的*v*状态将明确赋值。 本来*expr*后*v*的状态与*argN*后*v*的状态相同。
*  For 数组初始值设定项（[数组创建表达式](expressions.md#array-creation-expressions)）、对象初始值设定项（[对象初始值设定项](expressions.md#object-initializers)）、集合初始值设定项（[集合初始值设定项](expressions.md#collection-initializers)）和匿名对象初始值设定项（[匿名对象创建表达式](expressions.md#anonymous-object-creation-expressions)），明确的赋值状态由定义这些构造的扩展确定。

#### <a name="simple-assignment-expressions"></a>简单赋值表达式

对于以下`w = expr_rhs`形式*的表达式表达式*：

*  *Expr_rhs*之前的*v*的明确赋值状态与*expr*之前的*v*的明确赋值状态相同。
*  *Expr*后*v*的明确赋值状态由确定：
   * 如果*w*是与*v*相同的变量，则*expr*后*v*的明确赋值状态是明确赋值的。
   * 否则，如果在结构类型的实例构造函数中发生分配，则如果*w*是属性访问，它指定了正在构造的实例上自动实现的属性*P* ，而*v*是的隐藏支持字段*P*，则*expr*后*v*的明确赋值状态为明确赋值。
   * 否则，在*expr*后， *v*的明确赋值状态与*expr_rhs*后*v*的明确赋值状态相同。

#### <a name="-conditional-and-expressions"></a>& & （条件和）表达式

对于以下`expr_first && expr_second`形式*的表达式表达式*：

*  *Expr_first*之前的*v*的明确赋值状态与*expr*之前的*v*的明确赋值状态相同。
*  如果*expr_first*后*v*的状态为明确赋值或 "true expression 之后明确赋值"，则*expr_second*之前*v*的明确赋值状态为明确赋值。 否则，不会明确赋值。
*  *Expr*后*v*的明确赋值状态由确定：
    * 如果*expr_first*是一个具有值`false`的常量表达式，则在*expr*后*v*的明确赋值状态与*expr_first*后*v*的明确赋值状态相同。
    * 否则，如果*expr_first*后*v*的状态为明确赋值，则*expr*后*v*的状态将明确赋值。
    * 否则，如果*expr_second*后*v*的状态为 "已明确分配"，而 " *expr_first* *" 的状态*为 "在 false 表达式后明确赋值"，则*expr*后*v*的状态肯定是已.
    * 否则，如果*expr_second*后*v*的状态为 "明确赋值" 或 "true expression 之后明确赋值"，则*expr*后*v*的状态为 "在 true 表达式后明确赋值"。
    * 否则，如果*expr_first*后*v*的状态为 "在假表达式后明确赋值"，并且*expr_second*之后的*v*状态是 *"在 false*表达式后明确赋值"，则*expr*是 "在假表达式后明确赋值"。
    * 否则，不明确赋值*expr*后的*v*状态。

示例中
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
在`if`语句`i`的一个嵌入语句中（而不是在另一个语句中），该变量被视为已明确赋值。 在方法`if` `F`的语句中，变量`i`在第一个嵌入语句中是明确赋值的，因为在执行`(i = y)`此嵌入语句之前，表达式的执行始终为。 与此相反，第`i`二个嵌入语句中的变量并不是明确`x >= 0`赋值的，因为可能已对 false 进行`i`了测试，导致变量未赋值。

#### <a name="-conditional-or-expressions"></a>||（条件或）表达式

对于以下`expr_first || expr_second`形式*的表达式表达式*：

*  *Expr_first*之前的*v*的明确赋值状态与*expr*之前的*v*的明确赋值状态相同。
*  如果*expr_first*后*v*的状态为明确赋值或 "在 false 表达式后明确赋值"，则*expr_second*之前*v*的明确赋值状态为明确赋值。 否则，不会明确赋值。
*  *Expr*后的*v*的明确赋值语句由确定：
    * 如果*expr_first*是一个具有值`true`的常量表达式，则在*expr*后*v*的明确赋值状态与*expr_first*后*v*的明确赋值状态相同。
    * 否则，如果*expr_first*后*v*的状态为明确赋值，则*expr*后*v*的状态将明确赋值。
    * 否则，如果*expr_second*后*v*的状态是明确赋值的，并且*expr_first*之后的*v*状态是 "在真正表达式后明确赋值"，则*expr*后*v*的状态肯定是已.
    * 否则，如果*expr_second*后*v*的状态是明确赋值的或 "在假表达式后明确赋值"，则*expr*后*v*的状态为 "在 false 表达式后明确赋值"。
    * 否则，如果*expr_first*后*v*的状态为 "在 true 表达式后明确赋值"，并且*expr_second*之后的*v*状态是 "true expression 之后明确赋值"，则在 expr 后的*v*状态为 "true 表达式后明确赋值"。
    * 否则，不明确赋值*expr*后的*v*状态。

示例中
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
在`if`语句`i`的一个嵌入语句中（而不是在另一个语句中），该变量被视为已明确赋值。 在方法`if` `G`的语句中，变量`i`在第二个嵌入语句中是明确赋值的，因为在`(i = y)`执行此嵌入语句之前，表达式的执行始终为。 与此相反，第`i`一个嵌入语句中的变量不是明确赋值的`x >= 0` ，因为可能测试了 true，导致变量`i`被分配。

#### <a name="-logical-negation-expressions"></a>! （逻辑非）表达式

对于以下`! expr_operand`形式*的表达式表达式*：

*  *Expr_operand*之前的*v*的明确赋值状态与*expr*之前的*v*的明确赋值状态相同。
*  *Expr*后*v*的明确赋值状态由确定：
    * 如果在 * expr_operand * 之后的*v*状态为明确赋值，则*expr*后*v*的状态将明确赋值。
    * 如果未明确赋值 * expr_operand * 之后的*v*状态，则不明确赋值*expr*后的*v*状态。
    * 如果在 * expr_operand * 之后的*v*状态是 "在假表达式后明确赋值"，则*expr*后面的*v*状态是 "在真正表达式后明确赋值"。
    * 如果在 * expr_operand * 之后的*v*状态是 "在 true 表达式后明确赋值"，则*expr*后*v*的状态为 "在假表达式后明确赋值"。

#### <a name="-null-coalescing-expressions"></a>?? （null 合并）表达式

对于以下`expr_first ?? expr_second`形式*的表达式表达式*：

*  *Expr_first*之前的*v*的明确赋值状态与*expr*之前的*v*的明确赋值状态相同。
*  *Expr_second*之前的*v*的明确赋值状态与*expr_first*后*v*的明确赋值状态相同。
*  *Expr*后的*v*的明确赋值语句由确定：
    * 如果*expr_first*是一个值为 null 的常量表达式（[常数表达式](expressions.md#constant-expressions)），则在*expr*后， *v*的状态与*expr_second*后*v*的状态相同。
*  否则，在*expr*后， *v*的状态与*expr_first*后*v*的明确赋值状态相同。

#### <a name="-conditional-expressions"></a>？：（条件）表达式

对于以下`expr_cond ? expr_true : expr_false`形式*的表达式表达式*：

*  *Expr_cond*之前的*v*的明确赋值状态与*expr*前面的*v*的状态相同。
*  当且仅当以下其中一项保留时， *v 前 v*的明确赋值*状态才是*明确赋值的：
    * *expr_cond*是具有值的常量表达式`false`
    * *expr_cond*后*v*的状态是明确赋值的，或者是 "true 表达式后明确赋值"。
*  当且仅当以下其中一项保留时， *v 前 v*的明确赋值*状态才是*明确赋值的：
    * *expr_cond*是具有值的常量表达式`true`
*  *expr_cond*之后的*v*状态是明确赋值的，或者是 "在假表达式后明确赋值"。
*  *Expr*后*v*的明确赋值状态由确定：
    * 如果*expr_cond*是`true`具有值的常量表达式（[常数表达式](expressions.md#constant-expressions)），则*expr*后*v*的状态与*expr_true*后*v*的状态相同。
    * 否则，如果*expr_cond*是`false`具有值的常量表达式（[常数表达式](expressions.md#constant-expressions)），则*expr*后*v*的状态与*expr_false*后*v*的状态相同。
    * 否则，如果*expr_true*后*v*的状态为 "已明确分配" 并且 expr_false*的*状态为 "已明确分配"，则*expr*后*v*的状态将明确赋值。
    * 否则，不明确赋值*expr*后的*v*状态。

#### <a name="anonymous-functions"></a>匿名函数

对于带有主体（*块*或*表达式*）*正文*的*lambda_expression*或*anonymous_method_expression* *expr* ：

*  在*body*之前，外部变量*v*的明确赋值状态与*expr*前面的*v*的状态相同。 也就是说，外部变量的明确赋值状态是从匿名函数的上下文继承而来的。
*  *Expr*后面的外部变量*v*的明确赋值状态与*expr*前面的*v*的状态相同。

示例
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
生成编译时错误，因为`max`在声明匿名函数的位置未明确赋值。 示例
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
还会生成编译时错误，因为对匿名函数`n`的赋值不会影响匿名函数`n`之外的明确赋值状态。

## <a name="variable-references"></a>变量引用

*Variable_reference*是分类为变量的*表达式*。 *Variable_reference*表示可访问的存储位置以提取当前值并存储新值。

```antlr
variable_reference
    : expression
    ;
```

在 C 和C++中， *variable_reference*称为*lvalue*。

## <a name="atomicity-of-variable-references"></a>变量引用的原子性

以下数据类型的读取和写入是原子的： `bool`、 `char` `short` `ushort` `byte` `sbyte`、、 `uint` 、、`int`、、、和引用类型。 `float` 此外，在前面的列表中，具有基础类型的枚举类型的读取和写入也是原子的。 其他类型（ `long`包括、 `ulong`、 `double`、以及`decimal`用户定义类型）的读取和写入不能保证为原子性。 除了为此目的而设计的库函数外，不保证原子读修改写入，如递增或递减。

