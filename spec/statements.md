---
ms.openlocfilehash: 7248a91976c479dc1b6b64b799639635617a7bec
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704047"
---
# <a name="statements"></a>语句

C#提供各种语句。 对于在 C 和C++中进行了编程的开发人员，这些语句中的大部分将很熟悉。

```antlr
statement
    : labeled_statement
    | declaration_statement
    | embedded_statement
    ;

embedded_statement
    : block
    | empty_statement
    | expression_statement
    | selection_statement
    | iteration_statement
    | jump_statement
    | try_statement
    | checked_statement
    | unchecked_statement
    | lock_statement
    | using_statement
    | yield_statement
    | embedded_statement_unsafe
    ;
```

*Embedded_statement*非终止符用于在其他语句中显示的语句。 使用*embedded_statement*而不是*语句*不包括在这些上下文中声明语句和标记语句的使用。 示例
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
导致编译时错误，因为 `if` 语句需要*embedded_statement*而不是*语句*用于 if 分支。 如果允许此代码，则将声明变量 `i`，但绝不能使用它。 但请注意，通过将 `i`的声明放置在块中，该示例是有效的。

## <a name="end-points-and-reachability"></a>终结点和可访问性

每个语句都有一个***终结点***。 简而言之，语句的结束点是紧跟在语句之后的位置。 复合语句的执行规则（包含嵌入语句的语句）指定在控件到达嵌入语句的终结点时所采取的操作。 例如，当控件到达块中语句的终点时，控制将转移到块中的下一条语句。

如果语句可以通过执行到达，则表明该语句是可***访问***的。 相反，如果不可能执行语句，则认为该语句是无法***访问***的。

示例中
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
无法访问 `Console.WriteLine` 的第二个调用，因为不可能执行该语句。

如果编译器确定无法访问某个语句，则会报告警告。 它特别不是错误，无法访问语句。

若要确定特定的语句或终结点是否可访问，编译器将根据为每个语句定义的可访问性规则执行流分析。 流分析考虑了控制语句行为的常量表达式（[常量表达式](expressions.md#constant-expressions)）的值，但不考虑非常量表达式的可能值。 换句话说，出于控制流分析的目的，给定类型的非常量表达式被视为具有该类型的任何可能值。

示例中
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
`if` 语句的布尔表达式是常量表达式，因为 `==` 运算符的两个操作数都是常量。 在编译时计算常量表达式，并 `false`生成值时，`Console.WriteLine` 调用被视为不可访问。 但是，如果 `i` 更改为本地变量
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
`Console.WriteLine` 调用被视为可访问，但实际上，它将永远不会执行。

函数成员的*块*始终被认为是可访问的。 通过依次计算块中每个语句的可访问性规则，可以确定任何给定语句的可访问性。

示例中
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
确定第二个 `Console.WriteLine` 的可访问性，如下所示：

*  第一个 `Console.WriteLine` expression 语句是可访问的，因为 `F` 方法的块是可访问的。
*  第一个 `Console.WriteLine` expression 语句的结束点是可访问的，因为该语句是可访问的。
*  由于第一个 `Console.WriteLine` 表达式语句的结束点是可访问的，因此可以访问 `if` 语句。
*  第二个 `Console.WriteLine` expression 语句是可访问的，因为 `if` 语句的布尔表达式不具有 `false`的常量值。

在以下两种情况下，可能会发生编译时错误，导致语句的终结点可到达：

*  由于 `switch` 语句不允许 switch 节 "贯穿" 到下一个开关部分，因此在开关部分的语句列表的终结点可访问时，会发生编译时错误。 如果发生此错误，则通常指示缺少 `break` 语句。
*  对于计算要访问的值的函数成员块，它是一个编译时错误。 如果发生此错误，则通常指示缺少 `return` 语句。

## <a name="blocks"></a>Blocks

使用*代码块*，可以在允许编写一个语句的上下文中编写多个语句。

```antlr
block
    : '{' statement_list? '}'
    ;
```

*块*由括在大括号中的可选*statement_list* （[语句列表](statements.md#statement-lists)）组成。 如果省略了语句列表，则称块为空。

块可以包含声明语句（[声明语句](statements.md#declaration-statements)）。 块中声明的局部变量或常量的范围为块。

块的执行方式如下：

*  如果块为空，控制将被传输到块的终结点。
*  如果块不为空，则将控制转移到语句列表。 当和如果控件到达语句列表的终点时，控制将被传输到块的终结点。

如果块本身是可访问的，则块的语句列表是可访问的。

如果块为空或语句列表的终结点是可访问的，则块的终结点是可访问的。

包含一个或多个 `yield` 语句（[yield 语句](statements.md#the-yield-statement)）的*块*称为迭代器块。 迭代器块用于将函数成员作为迭代器（[迭代](classes.md#iterators)器）实现。 迭代器块适用于一些附加限制：

*  `return` 语句出现在迭代器块中（但允许 `yield return` 语句），这是编译时错误。
*  迭代器块包含不安全的上下文（[不安全](unsafe-code.md#unsafe-contexts)上下文）是编译时错误。 迭代器块始终定义安全上下文，即使其声明嵌套在不安全的上下文中也是如此。

### <a name="statement-lists"></a>语句列表

***语句列表***包含一个或多个按顺序编写的语句。 语句列表在*块*s （[块](statements.md#blocks)）中出现，在*switch_block*s 中（[switch 语句](statements.md#the-switch-statement)）。

```antlr
statement_list
    : statement+
    ;
```

语句列表通过将控制转移到第一条语句来执行。 当和如果控件到达语句的结束点时，控制将转移到下一个语句。 当和如果控件到达最后一个语句的终点时，控件将被传输到语句列表的终结点。

如果以下至少一个条件为 true，则语句列表中的语句是可访问的：

*  语句是第一条语句，语句列表本身是可访问的。
*  可以访问前面语句的终点。
*  语句是标记的语句，并且标签由可访问的 `goto` 语句引用。

如果列表中最后一个语句的结束点是可访问的，则该语句列表的终结点是可访问的。

## <a name="the-empty-statement"></a>空语句

*Empty_statement*不执行任何操作。

```antlr
empty_statement
    : ';'
    ;
```

在需要语句的上下文中没有要执行的操作时，将使用空语句。

空语句的执行只是将控制转移到语句的终结点。 因此，如果可以访问空语句，则可以访问空语句的结束点。

使用 null 体写入 `while` 语句时，可以使用空语句：
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

此外，空语句还可用于在块的关闭 "`}`" 之前声明标签：
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a>带标签的语句

*Labeled_statement*允许语句以标签为前缀。 标记语句允许出现在块中，但不允许作为嵌入语句使用。

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

标记语句使用*标识符*给定的名称声明标签。 标签的作用域是在其中声明标签的整个块，包括任何嵌套块。 对于具有相同名称的两个同名标签，它是编译时错误。

可以从标签范围内的 `goto` 语句（[goto 语句](statements.md#the-goto-statement)）引用标签。 这意味着 `goto` 语句可以将控制转移到块中，而不是块中的块。

标签具有自己的声明空间，不会干扰其他标识符。 示例
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
有效，并使用名称 `x` 同时作为参数和标签。

标记语句的执行与标签后面的语句的执行完全对应。

除了正常控制流提供的可访问性外，如果标签由可访问的 `goto` 语句引用，则可以访问标记的语句。 （异常：如果 `goto` 语句位于包含 `finally` 块的 `try` 中，且标记语句位于 `try`外，并且无法访问 `finally` 块的终结点，则无法从该 `goto` 语句访问标记的语句。）

## <a name="declaration-statements"></a>声明语句

声明局部变量或常量的*declaration_statement* 。 声明语句在块中是允许的，但不允许作为嵌入语句。

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a>局部变量声明

*Local_variable_declaration*声明一个或多个局部变量。

```antlr
local_variable_declaration
    : local_variable_type local_variable_declarators
    ;

local_variable_type
    : type
    | 'var'
    ;

local_variable_declarators
    : local_variable_declarator
    | local_variable_declarators ',' local_variable_declarator
    ;

local_variable_declarator
    : identifier
    | identifier '=' local_variable_initializer
    ;

local_variable_initializer
    : expression
    | array_initializer
    | local_variable_initializer_unsafe
    ;
```

*Local_variable_declaration*的*local_variable_type*直接指定声明引入的变量类型，或者使用标识符 `var` 指示应基于初始值设定项推断类型。 该类型后跟一个*local_variable_declarator*s 列表，其中每个都引入一个新变量。 *Local_variable_declarator*包含命名变量的*标识符*，可选择后跟 "`=`" 标记和提供变量初始值的*local_variable_initializer* 。

在局部变量声明的上下文中，标识符 var 用作上下文关键字（[关键字](lexical-structure.md#keywords)）。如果将*local_variable_type*指定为 `var` 并且未在范围中指定名为 `var` 的类型，则声明为***隐式类型化局部变量声明***，其类型是从关联的初始值设定项表达式的类型推断而来的。 隐式类型的局部变量声明受到以下限制：

*  *Local_variable_declaration*不能包含多个*local_variable_declarator*。
*  *Local_variable_declarator*必须包含*local_variable_initializer*。
*  *Local_variable_initializer*必须是*表达式*。
*  初始值设定项*表达式*必须具有编译时类型。
*  初始值设定项*表达式*不能引用声明的变量本身

下面是错误的隐式类型化局部变量声明的示例：

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

局部变量的值是使用*simple_name* （[简单名称](expressions.md#simple-names)）在表达式中获取的，使用*赋值*（[赋值运算符](expressions.md#assignment-operators)）修改本地变量的值。 局部变量在获取其值的每个位置都必须明确赋值（[明确赋值](variables.md#definite-assignment)）。

在*local_variable_declaration*中声明的局部变量的作用域是在其中进行声明的块。 在本地变量*local_variable_declarator*之前的文本位置引用局部变量是错误的。 在局部变量的作用域内，使用相同的名称声明另一个局部变量或常量时，会发生编译时错误。

声明多个变量的局部变量声明等效于多个具有相同类型的单个变量的声明。 此外，局部变量声明中的变量初始值设定项完全对应于紧接在声明后插入的赋值语句。

示例
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
完全对应于
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

在隐式类型的局部变量声明中，声明的局部变量的类型将被视为与用于初始化变量的表达式的类型相同。 例如：
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

上面的隐式类型局部变量声明完全等效于以下显式类型化声明：
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a>局部常量声明

一个*local_constant_declaration*声明一个或多个本地常量。

```antlr
local_constant_declaration
    : 'const' type constant_declarators
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

*Local_constant_declaration*的*类型*指定声明引入的常量的类型。 该类型后跟一个*constant_declarator*s 列表，其中每个都引入一个新常量。 *Constant_declarator*包含一个*标识符*，该标识符对常量进行命名，后跟一个 "`=`" 标记，后跟一个提供常量值的*constant_expression* （[常数表达式](expressions.md#constant-expressions)）。

局部常量声明的*类型*和*constant_expression*必须遵循与常量成员声明（[常量](classes.md#constants)）相同的规则。

局部变量的值是使用*simple_name* （[简单名称](expressions.md#simple-names)）在表达式中获取的。

局部常数的作用域是在其中进行声明的块。 在*constant_declarator*之前的文本位置引用本地常量是错误的。 在局部常数的范围内，使用相同的名称声明另一个局部变量或常量时，会发生编译时错误。

声明多个常量的局部常量声明等效于多个具有相同类型的单个常量的声明。

## <a name="expression-statements"></a>表达式语句

*Expression_statement*计算给定表达式的值。 由表达式计算的值（如果有）将被丢弃。

```antlr
expression_statement
    : statement_expression ';'
    ;

statement_expression
    : invocation_expression
    | null_conditional_invocation_expression
    | object_creation_expression
    | assignment
    | post_increment_expression
    | post_decrement_expression
    | pre_increment_expression
    | pre_decrement_expression
    | await_expression
    ;
```

并非所有表达式都允许作为语句。 具体而言，不允许使用只计算值（将被丢弃）的表达式（如 `x + y` 和 `x == 1`）作为语句。

执行*expression_statement*将计算包含的表达式，然后将控制转移到*expression_statement*的终结点。 如果*expression_statement*可访问，则可到达*expression_statement*的终结点。

## <a name="selection-statements"></a>选择语句

选择语句根据某个表达式的值为执行选择多个可能的语句之一。

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a>If 语句

`if` 语句基于布尔表达式的值来选择要执行的语句。

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

`else` 部分与语法允许的上一个词法上最近的 `if` 相关联。 因此，形式的 `if` 语句
```csharp
if (x) if (y) F(); else G();
```
等效于
```csharp
if (x) {
    if (y) {
        F();
    }
    else {
        G();
    }
}
```

执行 `if` 语句，如下所示：

*  计算*boolean_expression* （[布尔表达式](expressions.md#boolean-expressions)）。
*  如果布尔表达式产生 `true`，则将控制转移到第一个嵌入语句。 当和如果控件到达该语句的终点时，控制将被传输到 `if` 语句的终点。
*  如果布尔表达式产生 `false` 并且 `else` 部分存在，则将控制权转移到第二个嵌入语句。 当和如果控件到达该语句的终点时，控制将被传输到 `if` 语句的终点。
*  如果布尔表达式产生 `false` 并且 `else` 部分不存在，则将控制权转移到 `if` 语句的终点。

如果 `if` 语句可访问且布尔表达式不具有 `false`的常量值，则可以访问 `if` 语句的第一个嵌入语句。

如果 `if` 语句的第二个嵌入语句（如果存在）可访问，如果 `if` 语句可访问且布尔表达式不具有 `true`的常量值。

如果可以访问一个或多个嵌入语句的结束点，则可到达 `if` 语句的终点。 此外，如果 `if` 语句是可访问的，并且布尔表达式没有 `true`的常量值，则可访问没有 `else` 部分的 `if` 语句的结束点。

### <a name="the-switch-statement"></a>Switch 语句

Switch 语句为执行选择一个语句列表，其中包含与 switch 表达式的值相对应的关联 switch 标签。

```antlr
switch_statement
    : 'switch' '(' expression ')' switch_block
    ;

switch_block
    : '{' switch_section* '}'
    ;

switch_section
    : switch_label+ statement_list
    ;

switch_label
    : 'case' constant_expression ':'
    | 'default' ':'
    ;
```

*Switch_statement*包含关键字 `switch`，后跟带括号的表达式（称为 switch 表达式），后跟一个*switch_block*。 *Switch_block*由零个或多个*switch_section*组成，括在大括号中。 每个*switch_section*都包含一个或多个*switch_label*，后跟一个*statement_list* （[语句列表](statements.md#statement-lists)）。

`switch` 语句的***管理类型***由 switch 表达式建立。

*  如果 switch 表达式的类型为 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`bool`、`char`、`string`或*enum_type*，或者它是对应于其中一个类型的可以为 null 的类型，则这是 `switch` 语句的管理类型。
*  否则，必须有一个用户定义的隐式转换（[用户定义的转换](conversions.md#user-defined-conversions)）从 switch 表达式的类型到以下可能的管辖类型之一： `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`string`或，这是与这些类型之一对应的可以为 null 的类型。
*  否则，如果不存在这样的隐式转换，或者存在多个这样的隐式转换，则会发生编译时错误。

每个 `case` 标签的常量表达式必须表示一个可隐[式转换为](conversions.md#implicit-conversions)`switch` 语句的管理类型的值。 如果同一 `switch` 语句中的两个或多个 `case` 标签指定相同的常量值，则会发生编译时错误。

Switch 语句中最多只能有一个 `default` 标签。

执行 `switch` 语句，如下所示：

*  将计算 switch 表达式并将其转换为管理类型。
*  如果在同一 `switch` 语句的 `case` 标签中指定的其中一个常量等于 switch 表达式的值，则会将控制转移到匹配的 `case` 标签之后的语句列表中。
*  如果在同一 `switch` 语句的 `case` 标签中指定的常量均不等于 switch 表达式的值，并且如果存在 `default` 标签，则会将控制转移到后面的 `default` 标签后面的语句列表中。
*  如果在同一 `switch` 语句的 `case` 标签中指定的常量均不等于 switch 表达式的值，并且如果不存在 `default` 标签，则会将控制转移到 `switch` 语句的终点。

如果开关部分的语句列表的结束点是可访问的，则会发生编译时错误。 这称为 "不贯穿" 规则。 示例
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
default:
    CaseOthers();
    break;
}
```
有效，因为没有开关部分具有可访问的终结点。 与 C 和C++不同，switch 节的执行不允许 "贯穿" 到下一个开关部分，示例
```csharp
switch (i) {
case 0:
    CaseZero();
case 1:
    CaseZeroOrOne();
default:
    CaseAny();
}
```
导致编译时错误。 当执行 switch 节后，若要执行另一个 switch 节，必须使用显式 `goto case` 或 `goto default` 语句：
```csharp
switch (i) {
case 0:
    CaseZero();
    goto case 1;
case 1:
    CaseZeroOrOne();
    goto default;
default:
    CaseAny();
    break;
}
```

*Switch_section*允许使用多个标签。 示例
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
case 2:
default:
    CaseTwo();
    break;
}
```
有效。 该示例不违反 "no 贯穿" 规则，因为标签 `case 2:` 和 `default:` 属于同一*switch_section*。

"不贯穿" 规则可防止 C 中发生的常见错误类，以及C++在意外省略 `break` 语句时。 此外，由于此规则，可以任意重新排列 `switch` 语句的 switch 部分，而不会影响语句的行为。 例如，上面的 `switch` 语句的各个部分可以反向，而不会影响语句的行为：
```csharp
switch (i) {
default:
    CaseAny();
    break;
case 1:
    CaseZeroOrOne();
    goto default;
case 0:
    CaseZero();
    goto case 1;
}
```

开关部分的语句列表通常以 `break`、`goto case`或 `goto default` 语句结束，但允许不能使用呈现语句列表的终结点的任何构造。 例如，已知由布尔表达式 `true` 控制的 `while` 语句永远不会到达其终结点。 同样，`throw` 或 `return` 语句始终会将控制转移到其他位置，而永远不会到达终结点。 因此，下面的示例是有效的：
```csharp
switch (i) {
case 0:
    while (true) F();
case 1:
    throw new ArgumentException();
case 2:
    return;
}
```

`switch` 语句的管理类型可能是 `string`类型。 例如：
```csharp
void DoCommand(string command) {
    switch (command.ToLower()) {
    case "run":
        DoRun();
        break;
    case "save":
        DoSave();
        break;
    case "quit":
        DoQuit();
        break;
    default:
        InvalidCommand(command);
        break;
    }
}
```

与字符串相等运算符（[string 相等运算符](expressions.md#string-equality-operators)）一样，`switch` 语句区分大小写，并且仅当 switch 表达式字符串与 `case` 标签常量完全匹配时，才会执行给定的 switch 节。

当 `string``switch` 语句的管理类型时，允许将值 `null` 为 case 标签常量。

*Switch_block*的*statement_list*可能包含声明语句（[声明语句](statements.md#declaration-statements)）。 Switch 块中声明的局部变量或常量的范围为 switch 块。

如果 `switch` 语句可访问，且至少满足以下条件之一，则给定 switch 节的语句列表是可访问的：

*  Switch 表达式是一个非常量的值。
*  Switch 表达式是一个与开关部分中的 `case` 标签匹配的常量值。
*  Switch 表达式是不与任何 `case` 标签匹配的常量值，并且开关部分包含 `default` 标签。
*  开关部分的开关标签由可访问的 `goto case` 或 `goto default` 语句引用。

如果以下至少一个条件为 true，则可到达 `switch` 语句的终结点：

*  `switch` 语句包含可访问的 `break` 语句，该语句可退出 `switch` 语句。
*  `switch` 语句是可访问的，则 switch 表达式为非常量值，并且不存在任何 `default` 标签。
*  `switch` 语句是可访问的，则 switch 表达式是一个与任何 `case` 标签都不匹配的常量值，并且不存在 `default` 标签。

## <a name="iteration-statements"></a>迭代语句

迭代语句重复执行嵌入语句。

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a>While 语句

`while` 语句有条件地执行一个嵌入语句零次或多次。

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

执行 `while` 语句，如下所示：

*  计算*boolean_expression* （[布尔表达式](expressions.md#boolean-expressions)）。
*  如果布尔表达式产生 `true`，则将控制转移到嵌入语句。 当和如果控件到达嵌入语句的结束点（可能是执行 `continue` 语句）时，控制将转移到 `while` 语句的开头。
*  如果布尔表达式产生 `false`，则将控制转移到 `while` 语句的终点。

在 `while` 语句的嵌入语句中，`break` 语句（[break 语句](statements.md#the-break-statement)）可用于将控制转移到 `while` 语句的终结点（从而结束嵌入语句的迭代），并且 `continue` 语句（[continue 语句](statements.md#the-continue-statement)）可用于将控制转移到嵌入语句的终结点（从而执行 `while` 语句的另一次迭代）。

如果 `while` 语句可访问且布尔表达式不具有 `false`的常量值，则可以访问 `while` 语句的嵌入语句。

如果以下至少一个条件为 true，则可到达 `while` 语句的终结点：

*  `while` 语句包含可访问的 `break` 语句，该语句可退出 `while` 语句。
*  `while` 语句是可访问的，并且布尔表达式没有 `true`的常量值。

### <a name="the-do-statement"></a>Do 语句

`do` 语句有条件地执行一次或多次嵌入式语句。

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

执行 `do` 语句，如下所示：

*  控件将被传输到嵌入语句。
*  当和如果控件到达嵌入语句的终结点（可能是执行 `continue` 语句时），将计算*boolean_expression* （[布尔表达式](expressions.md#boolean-expressions)）。 如果布尔表达式产生 `true`，则将控制转移到 `do` 语句的开头。 否则，控制将转移到 `do` 语句的终点。

在 `do` 语句的嵌入语句中，`break` 语句（[break 语句](statements.md#the-break-statement)）可用于将控制转移到 `do` 语句的终结点（从而结束嵌入语句的迭代），而 `continue` 语句（[continue 语句](statements.md#the-continue-statement)）可用于将控制转移到嵌入语句的终结点。

如果 `do` 语句可访问，则可以访问 `do` 语句的嵌入语句。

如果以下至少一个条件为 true，则可到达 `do` 语句的终结点：

*  `do` 语句包含可访问的 `break` 语句，该语句可退出 `do` 语句。
*  嵌入语句的结束点是可访问的，并且布尔表达式没有 `true`的常量值。

### <a name="the-for-statement"></a>For 语句

`for` 语句计算一系列初始化表达式，然后，在条件为 true 时，重复执行嵌入语句并计算迭代表达式的序列。

```antlr
for_statement
    : 'for' '(' for_initializer? ';' for_condition? ';' for_iterator? ')' embedded_statement
    ;

for_initializer
    : local_variable_declaration
    | statement_expression_list
    ;

for_condition
    : boolean_expression
    ;

for_iterator
    : statement_expression_list
    ;

statement_expression_list
    : statement_expression (',' statement_expression)*
    ;
```

*For_initializer*（如果存在）由*local_variable_declaration* （[局部变量声明](statements.md#local-variable-declarations)）或以逗号分隔的*statement_expression*s （[expression 语句](statements.md#expression-statements)）的列表组成。 *For_initializer*声明的局部变量的作用域从变量的*local_variable_declarator*开始，并延伸到嵌入语句的末尾。 范围包括*for_condition*和*for_iterator*。

*For_condition*（如果存在）必须是*boolean_expression* （[布尔表达式](expressions.md#boolean-expressions)）。

*For_iterator*（如果存在）由以逗号分隔的*Statement_expression*s （[expression 语句](statements.md#expression-statements)）列表组成。

For 语句的执行方式如下：

*  如果*for_initializer*存在，变量初始值设定项或语句表达式将按照它们的写入顺序执行。 此步骤只执行一次。
*  如果*for_condition*存在，则将对其进行计算。
*  如果*for_condition*不存在或者计算产生 `true`，则将控制转移到嵌入语句。 当和如果控件到达嵌入语句的结束点（可能是执行 `continue` 语句）时，将按顺序计算*for_iterator*的表达式（如果有），然后执行另一次迭代，从对上述步骤中的*for_condition*进行计算开始。
*  如果*for_condition*存在并且计算生成 `false`，则控制将转移到 `for` 语句的终结点。

在 `for` 语句的嵌入语句内，`break` 语句（[break 语句](statements.md#the-break-statement)）可用于将控制转移到 `for` 语句的终结点（从而结束嵌入语句的迭代），而 `continue` 语句（[continue 语句](statements.md#the-continue-statement)）可用于将控制转移到嵌入语句的终结点（从而执行*for_iterator*并执行 `for` 语句的另一次迭代，从*for_condition*开始）。

如果满足以下条件之一，则可以访问 `for` 语句的嵌入语句：

*  `for` 语句是可访问的，并且不存在*for_condition* 。
*  `for` 语句是可访问的，并且*for_condition*存在并且没有 `false`的常量值。

如果以下至少一个条件为 true，则可到达 `for` 语句的终结点：

*  `for` 语句包含可访问的 `break` 语句，该语句可退出 `for` 语句。
*  `for` 语句是可访问的，并且*for_condition*存在并且没有 `true`的常量值。

### <a name="the-foreach-statement"></a>Foreach 语句

`foreach` 语句枚举集合中的元素，并对集合中的每个元素执行嵌入语句。

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

`foreach` 语句的*类型*和*标识符*声明语句的***迭代变量***。 如果 `var` 标识符作为*local_variable_type*提供，且没有任何名为 `var` 的类型在范围内，则将迭代变量称为***隐式类型的迭代变量***，并将其类型视为 `foreach` 语句的元素类型，如下所示。 迭代变量对应于一个只读局部变量，该局部变量具有扩展到嵌入语句的范围。 在 `foreach` 语句执行过程中，迭代变量表示当前正在为其执行迭代的集合元素。 如果嵌入的语句尝试修改迭代变量（通过赋值或 `++` 和 `--` 运算符），或将迭代变量作为 `ref` 或 `out` 参数传递，则会发生编译时错误。

在以下情况下，为简洁起见，`IEnumerable`、`IEnumerator``IEnumerable<T>` 和 `IEnumerator<T>` 引用命名空间中的相应类型 `System.Collections` 和 `System.Collections.Generic`。

Foreach 语句的编译时处理首先确定表达式的***集合类型***、***枚举器类型***和***元素类型***。 这种决定将按如下方式进行：

*  如果*表达式*的类型 `X` 为数组类型，则从 `X` 到 `IEnumerable` 接口（因为 `System.Array` 实现此接口）存在隐式引用转换。 ***集合类型***是 `IEnumerable` 接口，***枚举器类型***为 `IEnumerator` 接口，***元素类型***是数组类型 `X`的元素类型。
*  如果 `dynamic` `X` 的*表达式*的类型，则将从*expression*到 `IEnumerable` 接口（[隐式动态转换](conversions.md#implicit-dynamic-conversions)）的隐式转换。 ***集合类型***是 `IEnumerable` 接口，***枚举器类型***为 `IEnumerator` 接口。 如果将 `var` 标识符指定为*local_variable_type*则 `dynamic`元素类型，否则将 `object`该***元素类型***。
*  否则，请确定类型 `X` 是否具有适当的 `GetEnumerator` 方法：
   * 在类型 `X` 上执行成员查找，标识符 `GetEnumerator`，不包含任何类型参数。 如果成员查找不会生成匹配项，或者它产生了多义性，或者产生了不是方法组的匹配项，请检查可枚举的接口，如下所述。 如果成员查找产生了除方法组以外的任何内容，则建议发出警告。
   * 使用生成的方法组和空参数列表执行重载决策。 如果重载决策导致没有适用的方法、导致歧义或产生单个最佳方法，但该方法是静态的或非公共的，请按如下所述检查可枚举的接口。 如果重载决策产生了除明确的公共实例方法之外的任何内容，或者没有适用方法，则建议发出警告。
   * 如果 `GetEnumerator` 方法的返回类型 `E` 不是类、结构或接口类型，则会生成错误，并且不会执行任何其他步骤。
   * 成员查找在标识符为 `Current` 且无类型参数 `E` 上执行。 如果成员查找没有生成任何匹配项，则结果为错误，或结果为除允许读取的公共实例属性之外的任何内容，并不执行任何其他步骤。
   * 成员查找在标识符为 `MoveNext` 且无类型参数 `E` 上执行。 如果成员查找没有生成任何匹配项，则结果为错误，或结果为除方法组外的任何内容，并不执行任何其他步骤。
   * 使用空参数列表对方法组执行重载决策。 如果重载决策导致没有适用的方法，导致歧义，或者导致单个最佳方法，但该方法是静态的或非公共的，或者它的返回类型不 `bool`，则会生成错误，并且不会执行任何其他步骤。
   * ***集合类型***为 `X`，***枚举器类型***为 `E`，***元素类型***是 `Current` 属性的类型。

*  否则，请检查可枚举的接口：
   * 如果 `Ti` 的所有类型都是从 `X` 到 `IEnumerable<Ti>`的隐式转换，则有一个唯一类型 `T`，`T` 不 `dynamic`，而对于所有其他 `Ti` 存在从 `IEnumerable<T>` 到 `IEnumerable<Ti>`的隐式转换，则该***集合类型***为接口 `IEnumerable<T>`，***枚举器类型***为接口 `IEnumerator<T>`，且***元素类型***为 `T`。
   * 否则，如果有多个这样的类型 `T`，则会生成错误，并且不会执行任何其他步骤。
   * 否则，如果存在从 `X` 到 `System.Collections.IEnumerable` 接口的隐式转换，则***集合类型***为此接口，***枚举器类型***为接口 `System.Collections.IEnumerator`，并且***元素类型***是 `object`的。
   * 否则，将生成错误，并且不执行任何其他步骤。

如果成功，上述步骤会明确生成 `C`、枚举器类型 `E` 和元素类型 `T`的集合类型。 窗体的 foreach 语句
```csharp
foreach (V v in x) embedded_statement
```
然后扩展为：
```csharp
{
    E e = ((C)(x)).GetEnumerator();
    try {
        while (e.MoveNext()) {
            V v = (V)(T)e.Current;
            embedded_statement
        }
    }
    finally {
        ... // Dispose e
    }
}
```

变量 `e` 对 `x` 或程序的嵌入语句或任何其他源代码都不可见或可访问。 变量 `v` 在嵌入语句中是只读的。 如果没有从 `T` （元素类型）到 `V` （foreach 语句中的*local_variable_type* ）的显式转换（[显式](conversions.md#explicit-conversions)转换），则会生成错误，并且不会执行任何其他步骤。 如果 `x` 的值 `null`，则在运行时将引发 `System.NullReferenceException`。

允许实现以不同的方式实现给定的 foreach 语句（例如出于性能原因），前提是该行为与上述扩展一致。

While 循环内 `v` 的位置对于在*embedded_statement*中发生的任何匿名函数的捕获是非常重要的。

例如：
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
如果 `v` 是在 while 循环之外声明的，则它将在所有迭代之间共享，而 for 循环后，其值将是最终值 `13`，这是 `f` 的调用的输出。 相反，由于每个迭代都有其自己的变量 `v`，因此，第一次迭代中 `f` 捕获的变量将继续保存值 `7`，这就是要打印的值。 （注意：更早版本C#的声明在 while 循环外 `v`。）

Finally 块的主体按照以下步骤进行构造：

*  如果存在从 `E` 到 `System.IDisposable` 接口的隐式转换，则
   *  如果 `E` 是不可以为 null 的值类型，则将 finally 子句扩展为语义等效项：

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  否则，finally 子句将扩展到语义等效项：

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   除非 `E` 是值类型，或者是实例化为值类型的类型参数，否则，将 `e` 转换为 `System.IDisposable` 将不会导致装箱发生。

*  否则，如果 `E` 是密封类型，则 finally 子句将扩展为空块：

   ```csharp
   finally {
   }
   ```

*  否则，finally 子句将扩展为：

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   局部变量 `d` 对任何用户代码都不可见或不可访问。 具体而言，它不会与范围包含 finally 块的任何其他变量发生冲突。

`foreach` 遍历数组元素的顺序如下：对于一维数组元素按递增的索引顺序进行遍历，从索引 `0` 开始，以 index `Length - 1`结尾。 对于多维数组，会遍历元素，以便先增加最右侧维度的索引，然后再增加下一个左侧维度，依此类推。

下面的示例按元素顺序输出二维数组中的每个值：
```csharp
using System;

class Test
{
    static void Main() {
        double[,] values = {
            {1.2, 2.3, 3.4, 4.5},
            {5.6, 6.7, 7.8, 8.9}
        };

        foreach (double elementValue in values)
            Console.Write("{0} ", elementValue);

        Console.WriteLine();
    }
}
```
生成的输出如下所示：
```console
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

示例中
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
`n` 的类型将被推断为 `int`（`numbers`的元素类型）。

## <a name="jump-statements"></a>跳转语句

跳转语句无条件传输控制。

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

跳转语句将控制转移到的位置称为跳转语句的***目标***。

如果跳转语句发生在块中，并且该跳转语句的目标在该块外，则可以使用跳转语句***退出***块。 尽管一个跳转语句可以将控制转移到块外，但它永远不能将控制转移到块中。

由于存在干预 `try` 语句，执行跳转语句非常复杂。 在缺少此类 `try` 语句的情况下，跳转语句会无条件地将控制从跳转语句转移到其目标。 如果存在此类干预 `try` 语句，则执行会更复杂。 如果跳转语句退出一个或多个具有关联 `finally` 块的 `try` 块，则控件最初会传输到最内层 `try` 语句的 `finally` 块。 当和如果控件到达 `finally` 块的终点时，控制将转移到下一个封闭 `try` 语句的 `finally` 块。 重复此过程，直到执行了所有干预 `try` 语句 `finally` 块。

示例中
```csharp
using System;

class Test
{
    static void Main() {
        while (true) {
            try {
                try {
                    Console.WriteLine("Before break");
                    break;
                }
                finally {
                    Console.WriteLine("Innermost finally block");
                }
            }
            finally {
                Console.WriteLine("Outermost finally block");
            }
        }
        Console.WriteLine("After break");
    }
}
```
在控件传输到跳转语句的目标之前，将执行与两个 `try` 语句关联的 `finally` 块。

生成的输出如下所示：
```console
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a>Break 语句

`break` 语句退出最近的封闭 `switch`、`while`、`do`、`for`或 `foreach` 语句。

```antlr
break_statement
    : 'break' ';'
    ;
```

`break` 语句的目标是最近的封闭 `switch`、`while`、`do`、`for`或 `foreach` 语句的终点。 如果 `break` 语句未由 `switch`、`while`、`do`、`for`或 `foreach` 语句括起来，则会发生编译时错误。

当多个 `switch`、`while`、`do`、`for`或 `foreach` 语句彼此嵌套时，`break` 语句仅适用于最内层的语句。 若要跨多个嵌套级别传递控制，必须使用 `goto` 语句（[goto 语句](statements.md#the-goto-statement)）。

`break` 语句无法退出 `finally` 块（[try 语句](statements.md#the-try-statement)）。 如果 `break` 语句发生在 `finally` 块中，则 `break` 语句的目标必须在同一 `finally` 块内;否则，会发生编译时错误。

执行 `break` 语句，如下所示：

*  如果 `break` 语句退出一个或多个具有关联 `finally` 块的 `try` 块，则控件最初会传输到最内层 `try` 语句的 `finally` 块。 当和如果控件到达 `finally` 块的终点时，控制将转移到下一个封闭 `try` 语句的 `finally` 块。 重复此过程，直到执行了所有干预 `try` 语句 `finally` 块。
*  控件传输到 `break` 语句的目标。

由于 `break` 语句无条件地将控制转移到其他位置，因此无法访问 `break` 语句的终点。

### <a name="the-continue-statement"></a>Continue 语句

`continue` 语句启动最近的封闭 `while`、`do`、`for`或 `foreach` 语句的新迭代。

```antlr
continue_statement
    : 'continue' ';'
    ;
```

`continue` 语句的目标是最近的封闭 `while`、`do`、`for`或 `foreach` 语句的嵌入语句的终点。 如果 `continue` 语句未由 `while`、`do`、`for`或 `foreach` 语句括起来，则会发生编译时错误。

当多个 `while`、`do`、`for`或 `foreach` 语句彼此嵌套时，`continue` 语句仅适用于最内层的语句。 若要跨多个嵌套级别传递控制，必须使用 `goto` 语句（[goto 语句](statements.md#the-goto-statement)）。

`continue` 语句无法退出 `finally` 块（[try 语句](statements.md#the-try-statement)）。 如果 `continue` 语句发生在 `finally` 块中，则 `continue` 语句的目标必须在同一 `finally` 块内;否则，会发生编译时错误。

执行 `continue` 语句，如下所示：

*  如果 `continue` 语句退出一个或多个具有关联 `finally` 块的 `try` 块，则控件最初会传输到最内层 `try` 语句的 `finally` 块。 当和如果控件到达 `finally` 块的终点时，控制将转移到下一个封闭 `try` 语句的 `finally` 块。 重复此过程，直到执行了所有干预 `try` 语句 `finally` 块。
*  控件传输到 `continue` 语句的目标。

由于 `continue` 语句无条件地将控制转移到其他位置，因此无法访问 `continue` 语句的终点。

### <a name="the-goto-statement"></a>goto 语句

`goto` 语句将控制转移到由标签标记的语句。

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

`goto` *identifier*语句的目标是带有给定标签的标记语句。 如果当前函数成员中不存在具有给定名称的标签，或者如果 `goto` 语句不在标签范围内，则会发生编译时错误。 此规则允许使用 `goto` 语句将控制转移出嵌套作用域，但不允许使用嵌套作用域。 示例中
```csharp
using System;

class Test
{
    static void Main(string[] args) {
        string[,] table = {
            {"Red", "Blue", "Green"},
            {"Monday", "Wednesday", "Friday"}
        };

        foreach (string str in args) {
            int row, colm;
            for (row = 0; row <= 1; ++row)
                for (colm = 0; colm <= 2; ++colm)
                    if (str == table[row,colm])
                         goto done;

            Console.WriteLine("{0} not found", str);
            continue;
    done:
            Console.WriteLine("Found {0} at [{1}][{2}]", str, row, colm);
        }
    }
}
```
`goto` 语句用于将控制转移出嵌套作用域。

`goto case` 语句的目标是直接封闭 `switch` 语句（[switch 语句](statements.md#the-switch-statement)）中的语句列表，其中包含具有给定常数值的 `case` 标签。 如果 `goto case` 语句未使用 `switch` 语句括起来，则如果*constant_expression*不能隐[式转换为](conversions.md#implicit-conversions)最近的封闭 `switch` 语句的管理类型，或者最近的封闭 `switch` 语句不包含具有给定常数值的 `case` 标签，则会发生编译时错误。

`goto default` 语句的目标是直接封闭 `switch` 语句（[switch 语句](statements.md#the-switch-statement)）中的语句列表，其中包含一个 `default` 标签。 如果 `goto default` 语句未用 `switch` 语句括起来，或者最近的封闭 `switch` 语句不包含 `default` 标签，则会发生编译时错误。

`goto` 语句无法退出 `finally` 块（[try 语句](statements.md#the-try-statement)）。 如果 `goto` 语句发生在 `finally` 块中，则 `goto` 语句的目标必须在同一 `finally` 块中，否则将发生编译时错误。

执行 `goto` 语句，如下所示：

*  如果 `goto` 语句退出一个或多个具有关联 `finally` 块的 `try` 块，则控件最初会传输到最内层 `try` 语句的 `finally` 块。 当和如果控件到达 `finally` 块的终点时，控制将转移到下一个封闭 `try` 语句的 `finally` 块。 重复此过程，直到执行了所有干预 `try` 语句 `finally` 块。
*  控件传输到 `goto` 语句的目标。

由于 `goto` 语句无条件地将控制转移到其他位置，因此无法访问 `goto` 语句的终点。

### <a name="the-return-statement"></a>Return 语句

`return` 语句将控制权返回到 `return` 语句显示函数的当前调用方。

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

不带表达式的 `return` 语句只能在不计算值的函数成员中使用，即，具有结果类型（[方法主体](classes.md#method-body)） `void`的方法、属性或索引器的 `set` 访问器、事件的 `add` 和 `remove` 访问器、实例构造函数、静态构造函数或析构函数。

带有表达式的 `return` 语句只能在计算值的函数成员中使用，即，具有非 void 结果类型的方法、属性或索引器的 `get` 访问器或用户定义的运算符。 隐式转换（[隐式](conversions.md#implicit-conversions)转换）必须存在于表达式的类型到包含函数成员的返回类型。

Return 语句还可用于匿名函数表达式（[匿名函数表达式](expressions.md#anonymous-function-expressions)）的主体中，并参与确定哪些转换存在这些函数。

`return` 语句出现在 `finally` 块中（[try 语句](statements.md#the-try-statement)），这是编译时错误。

执行 `return` 语句，如下所示：

*  如果 `return` 语句指定一个表达式，将计算该表达式，并通过隐式转换将生成的值转换为包含函数的返回类型。 转换的结果成为函数生成的结果值。
*  如果 `return` 语句由一个或多个具有关联 `finally` 块的 `try` 或 `catch` 块括起来，则控件最初会传输到最内层 `finally` 语句的 `try` 块。 当和如果控件到达 `finally` 块的终点时，控制将转移到下一个封闭 `try` 语句的 `finally` 块。 此过程将重复进行，直到执行完所有封闭 `try` 语句 `finally` 块。
*  如果包含函数不是异步函数，则会将控件返回到包含函数的调用方，同时返回结果值（如果有）。
*  如果包含函数是一个异步函数，则将控制权返回给当前调用方，并在返回任务中记录结果值（如果有），如（[枚举器接口](classes.md#enumerator-interfaces)）中所述。

由于 `return` 语句无条件地将控制转移到其他位置，因此无法访问 `return` 语句的终点。

### <a name="the-throw-statement"></a>Throw 语句

`throw` 语句引发异常。

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

带有表达式的 `throw` 语句将引发通过计算表达式生成的值。 表达式必须表示从 `System.Exception` 派生的类类型 `System.Exception`类类型的值，或者是具有其有效基类的 `System.Exception` （或子类）的类型参数类型的值。 如果表达式的计算生成 `null`，则改为引发 `System.NullReferenceException`。

不带表达式的 `throw` 语句只能用在 `catch` 块中，在这种情况下，该语句会重新引发该 `catch` 块当前正在处理的异常。

由于 `throw` 语句无条件地将控制转移到其他位置，因此无法访问 `throw` 语句的终点。

当引发异常时，控制将转移到可处理异常的封闭 `try` 语句中的第一个 `catch` 子句。 从引发的异常点到将控件传输到适当的异常处理程序的时间点的过程称为 "***异常传播***"。 异常的传播包括重复计算以下步骤，直到找到与异常匹配的 `catch` 子句。 在此说明中，引发异常的位置最初为***引发点***的位置。

*  在当前函数成员中，将检查包含引发点的每个 `try` 语句。 对于 `S`的每个语句，从最内层的 `try` 语句开始，到最外面的 `try` 语句结束，计算以下步骤：

   * 如果 `S` 的 `try` 块包含了序列点，并且如果 S 具有一个或多个 `catch` 子句，则将根据 " [Try" 语句](statements.md#the-try-statement)部分中指定的规则，按显示顺序检查 `catch` 子句以查找异常的合适处理程序。 如果找到匹配的 `catch` 子句，则通过将控制转移到该 `catch` 子句的块来完成异常传播。

   * 否则，如果 `try` 块或 `catch` 块 `S` 包含了引发点，并且 `S` 有一个 `finally` 块，则将控制转移到 `finally` 块。 如果 `finally` 块引发另一个异常，则终止处理当前异常。 否则，当控件到达 `finally` 块的终点时，将继续处理当前异常。

*  如果在当前函数调用中未找到异常处理程序，则终止函数调用，并发生以下情况之一：

   * 如果当前函数为非异步，则会为函数调用方重复上述步骤，并将引发点与调用函数成员的语句对应。

   * 如果当前函数为 async 并返回任务，则会在返回任务中记录异常，该异常将被置于 "[枚举器接口](classes.md#enumerator-interfaces)" 中所述的 "出错" 或 "已取消" 状态。

   * 如果当前函数为 async 和 void 返回，则会通知当前线程的同步上下文，如可[枚举接口](classes.md#enumerable-interfaces)中所述。

*  如果异常处理终止当前线程中的所有函数成员调用，指示该线程没有异常的处理程序，则该线程本身将终止。 此类终止的影响是由实现定义的。

## <a name="the-try-statement"></a>Try 语句

`try` 语句提供一种机制，用于捕获在执行块期间发生的异常。 此外，`try` 语句还可以指定在控制离开 `try` 语句时始终执行的代码块。

```antlr
try_statement
    : 'try' block catch_clause+
    | 'try' block finally_clause
    | 'try' block catch_clause+ finally_clause
    ;

catch_clause
    : 'catch' exception_specifier? exception_filter?  block
    ;

exception_specifier
    : '(' type identifier? ')'
    ;

exception_filter
    : 'when' '(' expression ')'
    ;

finally_clause
    : 'finally' block
    ;
```

有三种可能的 `try` 语句形式：

*  后跟一个或多个 `catch` 块的 `try` 块。
*  后跟 `finally` 块的 `try` 块。
*  后跟一个或多个 `catch` 块后跟一个 `finally` 块的 `try` 块。

当 `catch` 子句指定一个*exception_specifier*时，该类型必须是 `System.Exception`、从 `System.Exception` 派生的类型或将 `System.Exception` （或子类）作为其有效基类的类型参数类型。

当 `catch` 子句使用*标识符*同时指定*exception_specifier*时，将声明一个具有给定名称和类型的***异常变量***。 异常变量对应于一个本地变量，该变量的作用域扩展了 `catch` 子句。 在*exception_filter*和*块*的执行期间，异常变量表示当前正在处理的异常。 出于明确赋值检查的目的，在其整个范围内将异常变量视为明确赋值。

除非 `catch` 子句包含异常变量名称，否则无法访问筛选器和 `catch` 块中的异常对象。

不指定*exception_specifier*的 `catch` 子句称为一般 `catch` 子句。

某些编程语言可能会支持不能表示为派生自 `System.Exception`的对象的异常，但C#代码不会生成此类异常。 可以使用常规 `catch` 子句来捕捉此类异常。 因此，一般的 `catch` 子句在语义上与指定类型 `System.Exception`的子句不同，因为前者还可以捕获其他语言的异常。

若要查找异常的处理程序，请按词法顺序检查 `catch` 子句。 如果 `catch` 子句指定了一种类型，但没有指定异常筛选器，则在同一 `try` 语句中，后面的 `catch` 子句的编译时错误为指定与该类型相同或派生自该类型的类型。 如果 `catch` 子句未指定任何类型并且没有筛选器，则它必须是该 `try` 语句的最后一个 `catch` 子句。

在 `catch` 块中，不带表达式的 `throw` 语句（[throw 语句](statements.md#the-throw-statement)）可用于重新引发由 `catch` 块捕获的异常。 对异常变量的赋值不会改变重新引发的异常。

示例中
```csharp
using System;

class Test
{
    static void F() {
        try {
            G();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in F: " + e.Message);
            e = new Exception("F");
            throw;                // re-throw
        }
    }

    static void G() {
        throw new Exception("G");
    }

    static void Main() {
        try {
            F();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in Main: " + e.Message);
        }
    }
}
```
方法 `F` 捕获异常，将一些诊断信息写入控制台，更改异常变量，并重新引发异常。 重新引发的异常是原始异常，因此生成的输出为：
```console
Exception in F: G
Exception in Main: G
```

如果第一个 catch 块引发 `e` 而不是重新引发当前异常，则生成的输出将如下所示：
```console
Exception in F: G
Exception in Main: F
```

`break`、`continue`或 `goto` 语句将控制转移到 `finally` 块，这是编译时错误。 当 `finally` 块中出现 `break`、`continue`或 `goto` 语句时，语句的目标必须在同一 `finally` 块中，否则将发生编译时错误。

`return` 语句发生在 `finally` 块中是编译时错误。

执行 `try` 语句，如下所示：

*  控件传输到 `try` 块。
*  当和如果控件到达 `try` 块的终点时：
   *  如果 `try` 语句具有 `finally` 块，则将执行 `finally` 块。
   *  Control 会传输到 `try` 语句的终点。

*  如果在执行 `try` 块期间将异常传播到 `try` 语句：
   *  `catch` 子句（如果有）将按外观的顺序进行检查，以查找适用于异常的处理程序。 如果 `catch` 子句未指定类型，或指定异常类型或异常类型的基类型：
      *  如果 `catch` 子句声明异常变量，则会将异常对象分配给异常变量。
      *  如果 `catch` 子句声明异常筛选器，则会计算筛选器。 如果计算结果为 `false`，catch 子句不是匹配项，并且搜索将继续执行适用于适当处理程序的任何后续 `catch` 子句。
      *  否则，`catch` 子句被视为匹配，并将控制转移到匹配的 `catch` 块。
      *  当和如果控件到达 `catch` 块的终点时：
         * 如果 `try` 语句具有 `finally` 块，则将执行 `finally` 块。
         * Control 会传输到 `try` 语句的终点。
      *  如果在执行 `catch` 块期间将异常传播到 `try` 语句：
         *  如果 `try` 语句具有 `finally` 块，则将执行 `finally` 块。
         *  异常将传播到下一个封闭 `try` 语句。
   *  如果 `try` 语句没有 `catch` 子句，或者如果没有 `catch` 子句与异常匹配，则为：
      *  如果 `try` 语句具有 `finally` 块，则将执行 `finally` 块。
      *  异常将传播到下一个封闭 `try` 语句。

当控制离开 `try` 语句时，始终会执行 `finally` 块的语句。 无论是由于执行 `break`、`continue`、`goto`或 `return` 语句导致控件传输，还是由于将异常传播到 `try` 语句而导致的，都是如此。

如果在执行 `finally` 块期间引发了异常，并且未在同一 finally 块中捕获该异常，则该异常将传播到下一个封闭 `try` 语句。 如果正在传播其他异常，则该异常将丢失。 传播异常的过程将在 `throw` 语句（[throw 语句](statements.md#the-throw-statement)）的说明中进一步讨论。

如果 `try` 语句可访问，则可访问 `try` 语句的 `try` 块。

如果 `try` 语句可访问，则可访问 `try` 语句 `catch` 块。

如果 `try` 语句可访问，则可访问 `try` 语句的 `finally` 块。

如果以下两个条件均为 true，则可以访问 `try` 语句的终结点：

*  `try` 块的终结点是可访问的，或者至少有一个 `catch` 块可访问的终结点。
*  如果存在 `finally` 块，则可访问 `finally` 块的终结点。

## <a name="the-checked-and-unchecked-statements"></a>Checked 和 unchecked 语句

`checked` 和 `unchecked` 语句用于控制整型算术运算和转换的***溢出检查上下文***。

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

`checked` 语句导致在已检查的上下文中计算*块*中的所有表达式，而 `unchecked` 语句导致在未检查的上下文中计算*块*中的所有表达式。

`checked` 和 `unchecked` 语句完全等效于`checked`和`unchecked`运算符（[checked 和 unchecked 运算符](expressions.md#the-checked-and-unchecked-operators)），只不过它们在块而不是表达式上操作。

## <a name="the-lock-statement"></a>Lock 语句

`lock` 语句获取给定对象的互斥锁，执行语句，然后释放该锁。

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

`lock` 语句的表达式必须表示已知为*reference_type*的类型的值。 对于 `lock` 语句的表达式，不会执行任何隐式装箱转换（[装箱转换](conversions.md#boxing-conversions)），因此，表达式会出现编译时错误，以表示*value_type*的值。

窗体的 `lock` 语句
```csharp
lock (x) ...
```
其中 `x` 是*reference_type*的表达式，它完全等效于
```csharp
bool __lockWasTaken = false;
try {
    System.Threading.Monitor.Enter(x, ref __lockWasTaken);
    ...
}
finally {
    if (__lockWasTaken) System.Threading.Monitor.Exit(x);
}
```
不同的是 `x` 只计算一次。

持有互斥锁时，在同一执行线程中执行的代码也可以获取和释放锁。 但是，在其他线程中执行的代码被阻止获取锁定，直到锁定被释放。

不建议锁定 `System.Type` 对象以同步对静态数据的访问。 其他代码可能会锁定同一类型，这可能会导致死锁。 更好的方法是通过锁定专用静态对象来同步对静态数据的访问。 例如：
```csharp
class Cache
{
    private static readonly object synchronizationObject = new object();

    public static void Add(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }

    public static void Remove(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }
}
```

## <a name="the-using-statement"></a>using 语句

`using` 语句获取一个或多个资源，执行语句，然后释放资源。

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

***资源***是实现 `System.IDisposable`的类或结构，其中包括一个名为 `Dispose`的无参数方法。 使用资源的代码可以调用 `Dispose`，以指示不再需要资源。 如果未调用 `Dispose`，则最终将作为垃圾回收的结果发生。

如果*local_variable_declaration* *resource_acquisition*的形式，则*local_variable_declaration*的类型必须是 `dynamic` 或可隐式转换为 `System.IDisposable`的类型。 如果*resource_acquisition*的形式为*expression* ，则此表达式必须可隐式转换为 `System.IDisposable`。

在*resource_acquisition*中声明的局部变量是只读的，并且必须包含初始值设定项。 如果嵌入的语句尝试修改这些本地变量（通过赋值或 `++` 和 `--` 运算符），则会发生编译时错误，请获取它们的地址，或将其作为 `ref` 或 `out` 参数传递。

`using` 语句转换为三个部分：获取、使用和处理。 资源的使用隐式包含在包含 `finally` 子句的 `try` 语句中。 此 `finally` 子句将释放资源。 如果获取 `null` 资源，则不会调用任何 `Dispose`，并且不会引发异常。 如果资源的类型为 `dynamic` 则通过隐式动态转换（[隐式动态](conversions.md#implicit-dynamic-conversions)转换）将其动态转换为在获取期间 `IDisposable`，以确保转换在使用和处置之前成功。

窗体的 `using` 语句
```csharp
using (ResourceType resource = expression) statement
```
对应于三个可能的扩展中的一个。 如果 `ResourceType` 是不可为 null 的值类型，则扩展为
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        ((IDisposable)resource).Dispose();
    }
}
```

否则，当 `ResourceType` 是可以为 null 的值类型或引用类型而不是 `dynamic`时，展开是
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        if (resource != null) ((IDisposable)resource).Dispose();
    }
}
```

否则，当 `dynamic``ResourceType` 时，展开是
```csharp
{
    ResourceType resource = expression;
    IDisposable d = (IDisposable)resource;
    try {
        statement;
    }
    finally {
        if (d != null) d.Dispose();
    }
}
```

在任一扩展中，`resource` 变量在嵌入语句中是只读的，并且在嵌入的语句中不可访问 `d` 变量，并且不可见。

允许实现以不同方式实现给定的 using 语句（例如出于性能原因），前提是该行为与上述扩展一致。

窗体的 `using` 语句
```csharp
using (expression) statement
```
具有相同的三个可能的扩展。 在这种情况下 `ResourceType` 隐式编译时 `expression`类型（如果有）。 否则，接口 `IDisposable` 自身用作 `ResourceType`。 在嵌入的语句中，不能访问 `resource` 变量，并且该变量不可见。

如果*resource_acquisition*采用*local_variable_declaration*的形式，则可以获取给定类型的多个资源。 窗体的 `using` 语句
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
完全等效于一系列嵌套的 `using` 语句：
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

下面的示例创建一个名为 `log.txt` 的文件，并向该文件写入两行文本。 然后，该示例将打开该文件以进行读取，并将包含的文本行复制到控制台。
```csharp
using System;
using System.IO;

class Test
{
    static void Main() {
        using (TextWriter w = File.CreateText("log.txt")) {
            w.WriteLine("This is line one");
            w.WriteLine("This is line two");
        }

        using (TextReader r = File.OpenText("log.txt")) {
            string s;
            while ((s = r.ReadLine()) != null) {
                Console.WriteLine(s);
            }

        }
    }
}
```

由于 `TextWriter` 和 `TextReader` 类实现了 `IDisposable` 接口，因此该示例可以使用 `using` 语句，以确保在执行写入或读取操作后正确关闭基础文件。

## <a name="the-yield-statement"></a>Yield 语句

在迭代器块（[块](statements.md#blocks)）中使用 `yield` 语句，以向迭代器的枚举器对象（[枚举器对象](classes.md#enumerator-objects)）或可枚举对象（可[枚举](classes.md#enumerable-objects)对象）生成值或表示迭代结束。

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

`yield` 不是保留字;仅当紧靠在 `return` 或 `break` 关键字之前使用时，它才具有特殊意义。 在其他上下文中，`yield` 可以用作标识符。

`yield` 语句可以出现的位置有多个限制，如下所述。

*  如果 `yield` 语句（窗体）出现在*method_body*外， *operator_body*或*accessor_body*将出现编译时错误。
*  在匿名函数内出现的 `yield` 语句（任一窗体）的编译时错误。
*  在 `try` 语句的 `finally` 子句中出现 `yield` 语句（窗体）的编译时错误。
*  如果 `yield return` 语句出现在包含任何 `catch` 子句的 `try` 语句中的任何位置，则会发生编译时错误。

下面的示例演示 `yield` 语句的一些有效和无效用法。

```csharp
delegate IEnumerable<int> D();

IEnumerator<int> GetEnumerator() {
    try {
        yield return 1;        // Ok
        yield break;           // Ok
    }
    finally {
        yield return 2;        // Error, yield in finally
        yield break;           // Error, yield in finally
    }

    try {
        yield return 3;        // Error, yield return in try...catch
        yield break;           // Ok
    }
    catch {
        yield return 4;        // Error, yield return in try...catch
        yield break;           // Ok
    }

    D d = delegate { 
        yield return 5;        // Error, yield in an anonymous function
    }; 
}

int MyMethod() {
    yield return 1;            // Error, wrong return type for an iterator block
}
```

隐式转换（[隐式](conversions.md#implicit-conversions)转换）必须从 `yield return` 语句中表达式的类型到迭代器的 yield 类型（[yield 类型](classes.md#yield-type)）存在。

执行 `yield return` 语句，如下所示：

*  计算语句中给定的表达式，将其隐式转换为 yield 类型，并将其分配给枚举器对象的 `Current` 属性。
*  迭代器块的执行被挂起。 如果 `yield return` 语句位于一个或多个 `try` 块中，则不会执行关联的 `finally` 块。
*  枚举器对象的 `MoveNext` 方法将 `true` 返回到其调用方，指示枚举器对象已成功地前进到下一项。

对枚举器对象的 `MoveNext` 方法的下一次调用会从其最后挂起的位置继续执行迭代器块。

执行 `yield break` 语句，如下所示：

*  如果 `yield break` 语句由一个或多个具有关联 `finally` 块的 `try` 块括起来，则控件最初会传输到最内部 `try` 语句的 `finally` 块。 当和如果控件到达 `finally` 块的终点时，控制将转移到下一个封闭 `try` 语句的 `finally` 块。 此过程将重复进行，直到执行完所有封闭 `try` 语句 `finally` 块。
*  控制权将返回给迭代器块的调用方。 这是枚举器对象的 `MoveNext` 方法或 `Dispose` 方法。

由于 `yield break` 语句无条件地将控制转移到其他位置，因此无法访问 `yield break` 语句的终点。
