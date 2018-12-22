# <a name="statements"></a>语句

C# 提供了各种不同的语句。 大多数这些语句将熟悉的开发人员可以在 C 和 c + + 中具有编程经验。

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

*Embedded_statement*非终止符出现在其他语句的语句的使用。 利用*embedded_statement*而非*语句*不包括的声明语句和标记的语句下面的上下文中使用。 该示例
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
导致编译时错误，因为`if`语句需要*embedded_statement*而非*语句*的 if 分支。 如果允许这种代码，然后在变量`i`将声明，但可能永远不会使用它。 但请注意，通过将放置`i`的块中的声明，该示例是有效的。

## <a name="end-points-and-reachability"></a>终结点和可访问性

每个语句具有***终结点***。 直观地讲，在语句的结束点是紧跟在语句的位置。 复合语句 （包含嵌入的语句的语句） 执行的规则指定当控件到达终结点的嵌入的语句时执行的操作。 例如，当控件到达终结点的块中的语句，控制转到下一个语句块中。

如果一个语句可能是可通过执行访问，该语句称***可访问***。 相反，如果没有要执行的语句不可能，该语句称***无法访问***。

在示例
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
第二个调用`Console.WriteLine`是无法访问，因为不可能将执行该语句。

如果编译器确定语句是无法访问，将报告警告。 具体而言不是错误的语句无法访问它。

若要确定是否可到达特定语句或终结点，编译器会执行流分析根据为每个语句定义的可访问性规则。 流分析会考虑常量表达式的值 ([常量表达式](expressions.md#constant-expressions)) 的控制行为的语句，但并不认为非常量表达式的可能值。 换而言之，用于控制流分析的目的，给定类型的非常量表达式被视为具有该类型的任何可能的值。

在示例
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
布尔表达式`if`语句是一个常量表达式，因为这两个操作数的`==`运算符是常量。 常量表达式将在编译时计算，生成值`false`，则`Console.WriteLine`调用被视为无法访问。 但是，如果`i`更改为本地变量
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
`Console.WriteLine`调用被视为可访问，即使在现实中，它将永远不会执行。

*块*函数的成员始终被认为是可访问。 通过依次计算每个语句块中的可访问性规则，可确定任何给定的语句的可访问性。

在示例
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
第二个可访问性`Console.WriteLine`，如下所示确定：

*  第一个`Console.WriteLine`表达式是可到达因为的块`F`方法是可访问。
*  第一个终结点`Console.WriteLine`表达式语句是可访问，因为该语句是可访问。
*  `if`语句是可访问，因为终结点的第一个`Console.WriteLine`表达式语句是可访问。
*  第二个`Console.WriteLine`表达式是可到达因为的布尔表达式`if`语句不具有常量值`false`。

有两种情况下，在其中它是语句可到达的终结点的编译时错误：

*  因为`switch`语句不允许在下一步的开关部分"贯穿"的开关部分，它是开关部分可到达语句列表的终结点的编译时错误。 如果发生此错误，它通常是一个指示，指明的`break`语句缺少。
*  它是终结点的块的计算值为可访问的函数成员的编译时错误。 如果发生此错误，它通常是一个指示，指明的`return`语句缺少。

## <a name="blocks"></a>Blocks

使用*代码块*，可以在允许编写一个语句的上下文中编写多个语句。

```antlr
block
    : '{' statement_list? '}'
    ;
```

一个*块*组成的可选*statement_list* ([语句列出了](statements.md#statement-lists))、 大括号内。 如果省略语句列表，则称块是为空。

块可以包含声明语句 ([声明语句](statements.md#declaration-statements))。 本地变量或常量的作用域在块中声明是块。

块的执行方式，如下所示：

*  如果块为空，控制传输到块的结束点。
*  如果块不为空，控制转到语句列表。 当且控制到达语句列表的终结点，则将控制转移到其中的块的终结点。

如果块本身是可访问的块的语句列表。

如果块为空或语句列表的结束点是可访问的块的终结点访问。

一个*块*，其中包含一个或多个`yield`语句 ([yield 语句](statements.md#the-yield-statement)) 调用迭代器块。 使用迭代器块实现迭代器作为函数成员 ([迭代器](classes.md#iterators))。 某些其他限制适用于迭代器块：

*  它是编译时错误`return`语句出现在迭代器块中 (但`yield return`允许语句)。
*  它是一个迭代器块，以包含不安全的上下文的编译时错误 ([不安全的上下文](unsafe-code.md#unsafe-contexts))。 迭代器块始终定义的安全上下文，即使其声明嵌套在不安全的上下文中。

### <a name="statement-lists"></a>语句列表

一个***语句列表***包含一个或多个序列中编写的语句。 语句列表中出现*块*s ([块](statements.md#blocks)) 并在*switch_block*s ([switch 语句](statements.md#the-switch-statement))。

```antlr
statement_list
    : statement+
    ;
```

通过将控制转移到第一个语句执行语句列表。 当和控制到达终结点的语句，控制传输到下一个语句。 当和控制到达最后一个语句的结束点，则将控制转移到其中的语句列表的终结点。

如果至少一个以下为 true，则语句列表中的语句不可用：

*  该语句是第一条语句和语句列表本身是可访问。
*  访问终结点的前面的语句。
*  该语句是带标签的语句和标签引用的可访问`goto`语句。

如果在列表中的最后一个语句的结束点是可访问终结点语句列表的访问。

## <a name="the-empty-statement"></a>空语句

*Empty_statement*不执行任何操作。

```antlr
empty_statement
    : ';'
    ;
```

时没有任何操作的上下文中执行语句的所需使用一个空语句。

执行一个空语句只需将控制转移到该语句的结束点。 因此，一个空语句终结点是空语句是可访问的情况下可访问。

写入时，可以使用一个空语句`while`语句体为空：
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

此外，一个空语句可用于声明只是在关闭前的一个标签"`}`"块的：
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a>带标签的语句

一个*labeled_statement*允许一个语句，以标签作为前缀。 标记的语句出现在块中，但不是能作为嵌入的语句。

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

使用给定的名称的标记的语句声明了一个标签*标识符*。 标签的范围是整个块在其中声明了标签，包括任何嵌套块。 它是具有相同的名称，具有重叠范围的两个标签的编译时错误。

可以从引用的标签`goto`语句 ([goto 语句](statements.md#the-goto-statement)) 标签的范围内。 这意味着，`goto`语句可将控制块内和外部，但永远不会分解成块传输。

标签具有其自己的声明空间并不会干扰其他标识符。 该示例
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
有效，并使用名称`x`用作参数和一个标签。

标记语句的执行完全对应于该标签后的语句的执行。

除了由正常控制流提供的可访问性，带标签的语句是标签引用的可访问的情况下可访问`goto`语句。 (异常：如果`goto`语句位于`try`，其中包含`finally`块中和标记的语句是外部`try`，和的终点`finally`块是无法访问，则标记的语句不是可从`goto`语句。)

## <a name="declaration-statements"></a>声明语句

一个*declaration_statement*声明本地变量或常量。 声明语句出现在块中，但不是能作为嵌入的语句。

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a>本地变量声明

一个*local_variable_declaration*声明一个或多个本地变量。

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

*Local_variable_type*的*local_variable_declaration*直接指定该声明引入的变量的类型或具有标识符指示`var`，应根据初始值设定项推断的类型。 键入后跟一系列*local_variable_declarator*s，其中每个引入了新的变量。 一个*local_variable_declarator*组成*标识符*命名变量，可以选择后跟"`=`"令牌和一个*local_variable_initializer* ，它给出的变量的初始值。

在本地变量声明的上下文中，标识符 var 充当上下文关键字 ([关键字](lexical-structure.md#keywords))。当*local_variable_type*指定为`var`和名为任何类型`var`是在范围内，声明是***隐式类型化局部变量声明***，其类型是从关联的初始值设定项表达式的类型推断。 隐式类型化局部变量声明受到以下限制：

*  *Local_variable_declaration*不能包含多个*local_variable_declarator*s。
*  *Local_variable_declarator*必须包括*local_variable_initializer*。
*  *Local_variable_initializer*必须是*表达式*。
*  初始值设定项*表达式*必须编译时类型。
*  初始值设定项*表达式*声明的变量本身不能引用

不正确的隐式类型本地变量声明的示例如下：

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

在中使用表达式获取本地变量的值*simple_name* ([简单名称](expressions.md#simple-names))，并使用本地变量的值进行修改*分配*([赋值运算符](expressions.md#assignment-operators))。 本地变量必须明确赋值 ([明确赋值](variables.md#definite-assignment)) 在每个位置获取其值。

在中声明本地变量的作用域*local_variable_declaration*是块中进行声明。 它是错误之前的文本位置中的本地变量是指*local_variable_declarator*的本地变量。 在本地变量的范围内，它是编译时错误，若要声明另一个本地变量或常量具有相同的名称。

声明多个变量的本地变量声明是等效于具有相同类型的单个变量的多个声明。 此外，在本地变量声明中的变量初始值设定项与赋值语句声明后立即插入完全对应。

该示例
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
设置完全对应
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

在隐式类型本地变量声明中，声明的本地变量的类型执行要用来初始化该变量的表达式的类型相同。 例如：
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

隐式类型化局部变量声明上面是完全等效于以下显式类型声明：
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

*类型*的*local_constant_declaration*指定该声明引入的常量的类型。 键入后跟一系列*constant_declarator*s，其中每个引入了新的常量。 一个*constant_declarator*组成*标识符*该名称的常量后, 跟"`=`"令牌后, 跟*constant_expression* ([常量表达式](expressions.md#constant-expressions)) 提供的常量的值。

*类型*并*constant_expression*的局部常量声明必须遵循相同的规则与常量成员声明 ([常量](classes.md#constants))。

在表达式中使用获取的值的局部常量*simple_name* ([简单名称](expressions.md#simple-names))。

局部常量的作用域是在块中进行声明。 它是错误的文本的位置之前的局部常量是指其*constant_declarator*。 范围内的局部常量，它是变量或常量具有相同名称声明另一个本地导致编译时错误。

声明多个常数在局部常量声明是等效于单个常量的具有相同类型的多个声明。

## <a name="expression-statements"></a>表达式语句

*Expression_statement*计算给定的表达式的值。 值计算的表达式，如果有，被丢弃。

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

并非所有表达式都允许作为语句。 特别地，表达式中`x + y`和`x == 1`的只是计算一个值，（该值将被放弃），不能作为语句。

执行*expression_statement*包含的表达式的计算结果，然后将控制转移到的终结点*expression_statement*。 终结点*expression_statement*可访问，如果该*expression_statement*可访问。

## <a name="selection-statements"></a>选择语句

选择语句选择其中一个的可能根据一些表达式的值执行的语句数。

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a>If 语句

`if`语句选择语句用于执行基于布尔表达式的值。

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

`else`部分是与前面从词法上最近`if`的允许的语法。 因此，`if`窗体的语句
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

`if`执行语句，如下所示：

*  *Boolean_expression* ([布尔表达式](expressions.md#boolean-expressions)) 进行计算。
*  如果布尔表达式生成`true`，控制权转交给第一条嵌入式语句。 当且控制到达该语句的结束点，将控制转移到其中的终结点`if`语句。
*  如果布尔表达式会生成`false`; 如果`else`一部分存在，控制权转交给第二个嵌入的语句。 当且控制到达该语句的结束点，将控制转移到其中的终结点`if`语句。
*  如果布尔表达式会生成`false`; 如果`else`一部分不存在，控制转移到其中的终结点`if`语句。

第一个嵌入的语句`if`语句是可访问如果`if`语句可访问且布尔表达式不具有常量值`false`。

第二个嵌入的语句`if`语句中，如果存在，是可访问如果`if`语句可访问且布尔表达式不具有常量值`true`。

终结点的`if`语句是至少一个嵌入的语句的结束点是可访问的情况下可访问。 此外，终结点的`if`没有语句`else`部分是可访问如果`if`语句是可访问和布尔表达式不具有常量值`true`。

### <a name="the-switch-statement"></a>Switch 语句

Switch 语句执行具有与 switch 表达式的值相对应的关联的交换机标签的语句列表中选择。

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

一个*switch_statement*包含关键字`switch`后, 跟带括号的表达式 （称为 switch 表达式） 后, 跟*switch_block*。 *Switch_block*由零个或多个组成*switch_section*s，括在大括号。 每个*switch_section*包含一个或多个*switch_label*s 跟*statement_list* ([语句列出了](statements.md#statement-lists))。

***用于控制类型***的`switch`语句建立的 switch 表达式。

*  Switch 表达式的类型是否`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `bool`， `char`， `string`，或*enum_type*，或如果它是可以为 null 的类型对应于其中一种类型，则这是用于管理类型的`switch`语句。
*  否则，只有一个用户定义的隐式转换 ([用户定义的转换](conversions.md#user-defined-conversions)) 必须存在从 switch 表达式的类型为以下可能用于控制类型之一： `sbyte`， `byte`， `short``ushort`， `int`， `uint`， `long`， `ulong`， `char`， `string`，或为 null 的类型对应于这些类型的一个。
*  否则，如果不存在此类隐式转换，或者如果有多个存在一个此类隐式转换，则发生编译时错误。

每个常量表达式`case`标签必须表示为隐式转换的值 ([隐式转换](conversions.md#implicit-conversions)) 到用于控制类型的`switch`语句。 如果两个或多个会发生编译时错误`case`中相同的标签`switch`语句指定相同的常量值。

可以是最多有`default`switch 语句中的标签。

一个`switch`执行语句，如下所示：

*  Switch 表达式计算并将其转换为用于控制类型。
*  如果在指定某一常量`case`中相同的标签`switch`语句是 switch 表达式的值相等，控制权转至以下匹配的语句列表`case`标签。
*  如果没有在指定的常量，则`case`中相同的标签`switch`语句等同于 switch 表达式的值; 如果`default`标签不存在，控制传输到后面的语句列表`default`标签。
*  如果没有在指定的常量，则`case`中相同的标签`switch`语句是 switch 表达式的值相等，如果没有`default`标签不存在、 控制转移到其中的终结点`switch`语句。

如果可访问的开关部分语句列表的终结点，会发生编译时错误。 这称为"无贯穿"规则。 该示例
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
无效，因为没有开关部分具有可访问的终结点。 与 C 和 c + +，不同的开关部分的执行不允许使用以"贯穿"到下一步的开关部分和示例
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
在编译时错误的结果。 执行的开关部分时的另一个开关部分，显式执行后接`goto case`或`goto default`必须使用语句：
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

在允许多个标签*switch_section*。 该示例
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
是有效的。 该示例不违反"无贯穿"规则，因为标签`case 2:`并`default:`是属于同一*switch_section*。

"无贯穿"规则将禁止在 C 和 c + + 中出现的 bug 的公共类时`break`语句无意中遗漏了。 此外，由于此规则的开关部分`switch`语句可以随意重新排列而不会影响该语句的行为。 例如，部分`switch`上述语句可反转而不会影响该语句的行为：
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

开关部分的语句列表通常以结尾`break`， `goto case`，或`goto default`允许语句，但呈现的语句列表的终结点无法访问任何构造。 例如，`while`控制的布尔表达式的语句`true`知道永远不会覆盖其终结点。 同样，`throw`或`return`语句始终会传输到其他位置的控件，永远不会到达其终结点。 因此，下面的示例是有效的：
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

用于控制的类型`switch`语句可以是类型`string`。 例如：
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

像字符串相等运算符 ([字符串是否相等运算符](expressions.md#string-equality-operators))，则`switch`语句是区分大小写和 switch 表达式字符串完全匹配才会执行给定的开关部分`case`标签常量。

用于管理的类型时`switch`语句是`string`，值`null`允许为 case 标签常量。

*Statement_list*的 s *switch_block*可能包含声明语句 ([声明语句](statements.md#declaration-statements))。 本地变量或常量的作用域中的 switch 块声明是 switch 块。

给定的开关部分的语句列表是如果`switch`语句为可访问且至少一个以下为 true:

*  Switch 表达式是一个非常量值。
*  Switch 表达式是与匹配的常量值`case`开关部分中的标签。
*  Switch 表达式是与任何不匹配的常数值`case`标签和开关部分包含`default`标签。
*  Switch 标签的开关部分引用的可访问`goto case`或`goto default`语句。

终结点的`switch`语句是可访问，如果在至少一个以下条件：

*  `switch`语句包含可访问`break`语句可退出`switch`语句。
*  `switch`语句是可访问，switch 表达式是一个非常量值，但没有`default`标签不存在。
*  `switch`语句是可访问，switch 表达式是与任何不匹配的常数值`case`标签，但没有`default`标签不存在。

## <a name="iteration-statements"></a>迭代语句

迭代语句重复执行嵌入的语句。

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a>While 语句

`while`语句有条件地执行嵌入的语句零次或多次。

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

一个`while`执行语句，如下所示：

*  *Boolean_expression* ([布尔表达式](expressions.md#boolean-expressions)) 进行计算。
*  如果布尔表达式生成`true`，控制传输到嵌入的语句。 如果控制到达嵌入的语句的结束点 (可能来自执行`continue`语句)，控制转移到其中的开头`while`语句。
*  如果布尔表达式会生成`false`，控制转移到其中的终结点`while`语句。

中的嵌入的语句`while`语句中，`break`语句 ([break 语句](statements.md#the-break-statement)) 可用于将控件传输到的终结点`while`语句 （因此结束迭代的嵌入语句），和一个`continue`语句 ([continue 语句](statements.md#the-continue-statement)) 可用于将控件传输到嵌入的语句结束点 (从而执行的另一个迭代`while`语句)。

嵌入的语句`while`语句是可访问如果`while`语句可访问且布尔表达式不具有常量值`false`。

终结点的`while`语句是可访问，如果在至少一个以下条件：

*  `while`语句包含可访问`break`语句可退出`while`语句。
*  `while`语句可访问且布尔表达式不具有常量值`true`。

### <a name="the-do-statement"></a>Do 语句

`do`语句有条件地执行嵌入的语句的一个或多个时间。

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

一个`do`执行语句，如下所示：

*  控制传输到嵌入的语句。
*  当和控制到达嵌入的语句的结束点 (可能来自执行`continue`语句)，则*boolean_expression* ([布尔表达式](expressions.md#boolean-expressions)) 进行计算。 如果布尔表达式会生成`true`，控制转移到其中的开头`do`语句。 否则，将控制转移到其中的终结点`do`语句。

中的嵌入的语句`do`语句中，`break`语句 ([break 语句](statements.md#the-break-statement)) 可用于将控件传输到的终结点`do`语句 （因此结束迭代的嵌入语句），和一个`continue`语句 ([continue 语句](statements.md#the-continue-statement)) 可用于将控件传输到嵌入的语句结束点。

嵌入的语句`do`语句是可访问如果`do`语句是可访问。

终结点的`do`语句是可访问，如果在至少一个以下条件：

*  `do`语句包含可访问`break`语句可退出`do`语句。
*  嵌入的语句的结束点是可访问和布尔表达式不具有常量值`true`。

### <a name="the-for-statement"></a>For 语句

`for`语句计算初始化表达式的一个序列，然后，当条件为真时，重复执行嵌入的语句和一系列迭代表达式的计算结果。

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

*For_initializer*(如果有） 由*local_variable_declaration* ([局部变量声明](statements.md#local-variable-declarations)) 或一系列*statement_表达式*s ([表达式语句](statements.md#expression-statements)) 以逗号隔开。 声明的局部变量的作用域*for_initializer*开始*local_variable_declarator*变量并延伸到嵌入的语句结束。 作用域包括*for_condition*并*for_iterator*。

*For_condition*(如果有） 必须*boolean_expression* ([布尔表达式](expressions.md#boolean-expressions))。

*For_iterator*(如果有） 的列表组成*statement_expression*s ([表达式语句](statements.md#expression-statements)) 以逗号隔开。

FOR 语句执行，如下所示：

*  如果*for_initializer*显示变量的初始值设定项或写入时的顺序执行语句表达式。 此步骤将仅执行一次。
*  如果*for_condition*存在，则它将进行计算。
*  如果*for_condition*不存在或如果评估结果`true`，控制传输到嵌入的语句。 时和控制到达嵌入的语句的结束点 (可能来自执行`continue`语句)，表达式*for_iterator*，如果任何，计算在序列中，并且另一个迭代，则执行开始计算*for_condition*在前面步骤中。
*  如果*for_condition*存在而计算产生`false`，控制转移到其中的终结点`for`语句。

中的嵌入的语句`for`语句中，`break`语句 ([break 语句](statements.md#the-break-statement)) 可用于将控件传输到的终结点`for`语句 （因此结束迭代的嵌入语句），和一个`continue`语句 ([continue 语句](statements.md#the-continue-statement)) 可用于将控件传输到嵌入的语句结束点 (从而执行*for_iterator*和执行的另一个迭代`for`语句，从开始*for_condition*)。

嵌入的语句的`for`语句是可访问，如果以下项之一为 true:

*  `for`是可到达和无*for_condition*存在。
*  `for`语句是可访问和一个*for_condition*存在且不具有常量值`false`。

终结点的`for`语句是可访问，如果在至少一个以下条件：

*  `for`语句包含可访问`break`语句可退出`for`语句。
*  `for`语句是可访问和一个*for_condition*存在且不具有常量值`true`。

### <a name="the-foreach-statement"></a>Foreach 语句

`foreach`语句枚举集合，执行嵌入的语句的每个元素的集合中的元素。

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

*类型*并*标识符*的`foreach`语句声明***迭代变量***的语句。 如果`var`标识符中指定为*local_variable_type*，并没有名为的类型`var`是在范围内，迭代变量称为***隐式类型化的迭代变量***，和类型的元素类型将被当成`foreach`语句，如下所示。 迭代变量对应于只读本地变量具有通过嵌入的语句进行了扩展作用域。 执行期间`foreach`语句，该迭代变量表示当前正在为其执行迭代的集合元素。 如果尝试修改的迭代变量嵌入的语句会发生编译时错误 (通过分配或`++`并`--`运算符) 或传递作为的迭代变量`ref`或`out`参数。

在下面的示例，为简洁起见， `IEnumerable`， `IEnumerator`，`IEnumerable<T>`并`IEnumerator<T>`命名空间中的相应类型，请参阅`System.Collections`和`System.Collections.Generic`。

Foreach 语句的编译时处理首先确定***集合类型***，***枚举器类型***并***元素类型***的表达式。 此决定继续，如下所示：

*  如果类型`X`的*表达式*为数组类型，则不从隐式引用转换`X`到`IEnumerable`接口 (因为`System.Array`实现此接口)。 ***集合类型***是`IEnumerable`接口，***枚举器类型***是`IEnumerator`接口和***元素类型***是元素类型数组类型`X`。
*  如果类型`X`的*表达式*是`dynamic`隐式转换则*表达式*到`IEnumerable`接口 ([隐式动态转换](conversions.md#implicit-dynamic-conversions))。 ***集合类型***是`IEnumerable`接口并***枚举器类型***是`IEnumerator`接口。 如果`var`标识符中指定为*local_variable_type*则***元素类型***是`dynamic`，否则它是`object`。
*  否则，确定是否类型`X`具有相应`GetEnumerator`方法：
   * 对类型执行成员查找`X`具有标识符`GetEnumerator`且不使用类型参数。 如果成员查找不会生成匹配项，或者它会造成多义性，或生成不是方法组的匹配项，检查可枚举接口如下所述。 建议如果成员查找生成的任何内容除外方法组或没有匹配项的情况下发出警告。
   * 执行重载决策使用生成的方法组和参数列表为空。 如果没有适用的方法的重载解析结果，导致二义性，或导致单个的最佳方法，但该方法是静态或非公共，检查的可枚举接口，如下所述。 建议如果除明确的公共实例方法或不适用的方法的重载解析生成任何内容的情况下发出警告。
   * 如果的返回类型`E`的`GetEnumerator`方法不是类、 结构或接口类型，错误会生成和采取任何进一步的步骤。
   * 在执行成员查找`E`具有标识符`Current`且不使用类型参数。 如果成员查找不产生任何匹配，结果是一个错误，或者会得到一个公共实例属性，允许读取以外，将会生成错误并采取任何进一步的步骤。
   * 在执行成员查找`E`具有标识符`MoveNext`且不使用类型参数。 如果成员查找不产生任何匹配，结果是一个错误，或者会得到一个方法组以外，将会生成错误并采取任何进一步的步骤。
   * 重载解析执行方法在组上使用参数列表为空。 如果没有适用的方法、 导致二义性或在单个的最佳方法，但该方法的结果中的重载解析结果是静态或非公共，或其返回类型不是`bool`，将会生成错误并采取任何进一步的步骤。
   * ***集合类型***是`X`，则***枚举器类型***是`E`，以及***元素类型***是一种`Current`属性。

*  否则，检查为可枚举接口：
   * 如果在所有类型`Ti`它们没有隐式转换`X`到`IEnumerable<Ti>`，没有唯一类型`T`以便`T`不是`dynamic`以及所有其他`Ti`没有从隐式转换`IEnumerable<T>`到`IEnumerable<Ti>`，然后***集合类型***是接口`IEnumerable<T>`，则***枚举器类型***是接口`IEnumerator<T>`，并***元素类型***是`T`。
   * 否则为如果多个此类类型`T`，然后将会生成错误并采取任何进一步的步骤。
   * 否则为如果隐式转换`X`到`System.Collections.IEnumerable`接口，则***集合类型***是此接口***枚举器类型***是接口`System.Collections.IEnumerator`，并***元素类型***是`object`。
   * 否则为将会生成错误并采取任何进一步的步骤。

上述步骤中，如果成功，明确地生成集合类型`C`，枚举器类型`E`和元素类型`T`。 窗体的 foreach 语句
```csharp
foreach (V v in x) embedded_statement
```
然后扩展到：
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

在变量`e`不可见或供表达式`x`或嵌入的语句或程序的任何其他源代码。 该变量`v`是嵌入的语句中以只读的。 如果不存在的显式转换 ([显式转换](conversions.md#explicit-conversions)) 从`T`（元素类型） 对`V`( *local_variable_type* foreach 语句中)，将会生成错误并采取任何进一步的步骤。 如果`x`具有值`null`、`System.NullReferenceException`在运行时引发。

实现允许以实现给定的 foreach 语句不同，例如出于性能原因，只要行为是一致具有更高版本的扩展。

位置`v`在 while 循环是有关如何捕获由任何匿名函数中发生重要*embedded_statement*。

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
如果`v`声明外部 while 循环中，它会在所有迭代和后的其值间共享循环将是最后一个值， `13`，哪些调用属于`f`将打印。 相反，因为每个迭代都有其自己的变量`v`，通过捕获的一个`f`第一次迭代将继续保存值`7`，这是将打印的内容。 (注意： 早期版本的 C# 声明`v`外部的 while 循环。)

正文最后块构造根据以下步骤：

*  如果没有隐式转换`E`到`System.IDisposable`接口，则
   *  如果`E`是不可以为 null 的值类型则 finally 子句扩展为的语义等效项：

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  否则为 finally 子句扩展为的语义等效项：

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   除非，如果`E`是值类型或类型参数为值类型，则强制转换的实例化`e`到`System.IDisposable`不会导致装箱发生。

*  否则为如果`E`是密封的类型，最后将子句扩展为一个空白块：

   ```csharp
   finally {
   }
   ```

*  否则为最后子句扩展为：

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   本地变量`d`不可见或任何用户代码可以访问。 具体而言，不是冲突与其作用域包括的任何其他变量 finally 块。

依据的顺序`foreach`遍历数组的元素，如下所示：为升序索引顺序遍历一维数组元素，从开始索引 `0`，以索引结束`Length - 1`。 对于多维数组，以便最右边的维度的索引是提高第一个，然后左边紧邻的维度，等向左遍历元素。

下面的示例输出中元素顺序的二维数组中每个值：
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
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

在示例
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
类型`n`被推断为`int`的元素类型`numbers`。

## <a name="jump-statements"></a>跳转语句

跳转语句无条件地将控制权。

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

跳转语句将控制转移到的位置称为***目标***的跳转语句。

时跳转语句发生在块中，并且该跳转语句的目标是该块外部，称跳转语句***退出***块。 虽然跳转语句可能会将控件转移出一个块，它可永远不会将控制转移到一个块。

跳转语句的执行十分复杂，是否存在干预性`try`语句。 如果没有此类`try`语句，跳转语句无条件将控制从跳转语句到其目标。 出现这种干预性的情况下`try`语句，执行是更复杂。 如果跳转语句退出一个或多个`try`相关联的块`finally`块，控制最初为传输`finally`块的最内层`try`语句。 如果控制到达的终结点`finally`块中，控制传输到`finally`的下一步封闭块`try`语句。 重复此过程，直到`finally`的所有块干预`try`已执行语句。

在示例
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
`finally`与两个相关联的块`try`语句执行之前将控制权转交给跳转语句的目标。

生成的输出如下所示：
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a>Break 语句

`break`语句将退出的最近的封闭`switch`， `while`， `do`， `for`，或`foreach`语句。

```antlr
break_statement
    : 'break' ';'
    ;
```

目标`break`语句是终结点的最近的封闭`switch`， `while`， `do`， `for`，或`foreach`语句。 如果`break`语句不括`switch`， `while`， `do`， `for`，或`foreach`语句中，会发生编译时错误。

当多个`switch`， `while`， `do`， `for`，或`foreach`语句嵌套在彼此之上，`break`语句仅适用于最内部的语句。 若要跨多个嵌套级别将控制权`goto`语句 ([goto 语句](statements.md#the-goto-statement)) 必须使用。

一个`break`语句无法退出`finally`块 ([try 语句](statements.md#the-try-statement))。 当`break`语句出现在`finally`block 的目标`break`语句必须是在同一个`finally`阻止; 否则，会发生编译时错误。

一个`break`执行语句，如下所示：

*  如果`break`语句退出一个或多个`try`相关联的块`finally`块，控制最初为传输`finally`块的最内层`try`语句。 如果控制到达的终结点`finally`块中，控制传输到`finally`的下一步封闭块`try`语句。 重复此过程，直到`finally`的所有块干预`try`已执行语句。
*  控件被转移到的目标`break`语句。

因为`break`语句无条件将控制转移到其他位置，终结点的`break`语句永远不会是可访问。

### <a name="the-continue-statement"></a>Continue 语句

`continue`语句将启动最近的封闭的新迭代`while`， `do`， `for`，或`foreach`语句。

```antlr
continue_statement
    : 'continue' ';'
    ;
```

目标`continue`语句是终结点的最近的封闭的嵌入式语句`while`， `do`， `for`，或`foreach`语句。 如果`continue`语句不括`while`， `do`， `for`，或`foreach`语句中，会发生编译时错误。

当多个`while`， `do`， `for`，或`foreach`语句嵌套在彼此之上，`continue`语句仅适用于最内部的语句。 若要跨多个嵌套级别将控制权`goto`语句 ([goto 语句](statements.md#the-goto-statement)) 必须使用。

一个`continue`语句无法退出`finally`块 ([try 语句](statements.md#the-try-statement))。 当`continue`语句出现在`finally`block 的目标`continue`语句必须是在同一个`finally`阻止; 否则会发生编译时错误。

一个`continue`执行语句，如下所示：

*  如果`continue`语句退出一个或多个`try`相关联的块`finally`块，控制最初为传输`finally`块的最内层`try`语句。 如果控制到达的终结点`finally`块中，控制传输到`finally`的下一步封闭块`try`语句。 重复此过程，直到`finally`的所有块干预`try`已执行语句。
*  控件被转移到的目标`continue`语句。

因为`continue`语句无条件将控制转移到其他位置，终结点的`continue`语句永远不会是可访问。

### <a name="the-goto-statement"></a>goto 语句

`goto`语句将控制转移到由标签标记的语句。

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

目标`goto`*标识符*语句是用给定标签标记的语句。 如果当前的函数成员中不存在具有给定名称的标签或`goto`语句不是标签，作用域内将发生编译时错误。 此规则将允许使用`goto`语句来移交控制权超出嵌套作用域，而不是嵌套的作用域。 在示例
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
`goto`语句用于将控件转移出嵌套的作用域。

目标`goto case`语句是语句列表中立即封闭`switch`语句 ([switch 语句](statements.md#the-switch-statement))，其中包含`case`使用给定的常量值的标签。 如果`goto case`语句不括`switch`语句中，如果*constant_expression*不能隐式转换 ([隐式转换](conversions.md#implicit-conversions)) 用于控制的类型最近的封闭`switch`语句，或者，如果最近的封闭`switch`语句不包含`case`标签使用给定的常量值，则发生编译时错误。

目标`goto default`语句是语句列表中立即封闭`switch`语句 ([switch 语句](statements.md#the-switch-statement))，其中包含`default`标签。 如果`goto default`语句不括`switch`语句，或者，如果最近的封闭`switch`语句不包含`default`标签，则发生编译时错误。

一个`goto`语句无法退出`finally`块 ([try 语句](statements.md#the-try-statement))。 当`goto`语句出现在`finally`block 的目标`goto`语句必须是在同一个`finally`块中，否则发生编译时错误。

一个`goto`执行语句，如下所示：

*  如果`goto`语句退出一个或多个`try`相关联的块`finally`块，控制最初为传输`finally`块的最内层`try`语句。 如果控制到达的终结点`finally`块中，控制传输到`finally`的下一步封闭块`try`语句。 重复此过程，直到`finally`的所有块干预`try`已执行语句。
*  控件被转移到的目标`goto`语句。

因为`goto`语句无条件将控制转移到其他位置，终结点的`goto`语句永远不会是可访问。

### <a name="the-return-statement"></a>Return 语句

`return`语句将控制权返回到在其中函数的当前调用方`return`语句将出现。

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

一个`return`仅在不会计算一个值，即，具有结果类型的方法的函数成员中可以使用不包含表达式的语句 ([方法主体](classes.md#method-body)) `void`，则`set`的属性访问器或索引器`add`和`remove`事件、 实例构造函数、 静态构造函数或析构函数的访问器。

一个`return`使用的表达式的语句只能在计算一个值，它是具有非 void 结果类型的方法的函数成员中使用`get`访问器的属性或索引器或用户定义的运算符。 隐式转换 ([隐式转换](conversions.md#implicit-conversions)) 到包含的函数成员的返回类型必须存在从表达式的类型。

语句还可以使用匿名函数表达式的正文中返回 ([匿名函数表达式](expressions.md#anonymous-function-expressions))，并参与确定这些函数的转换。

它是编译时错误`return`语句出现在`finally`块 ([try 语句](statements.md#the-try-statement))。

一个`return`执行语句，如下所示：

*  如果`return`语句指定一个表达式，计算该表达式并生成的值转换为包含函数的返回类型的隐式转换。 转换的结果将成为函数生成的结果值。
*  如果`return`语句包含一个或多个`try`或`catch`相关联的块`finally`块，控制最初为传输`finally`块的最内层`try`语句。 如果控制到达的终结点`finally`块中，控制传输到`finally`的下一步封闭块`try`语句。 重复此过程，直到`finally`所有封闭的块`try`已执行语句。
*  如果包含的函数不是异步函数，如果任何控件被返回到结果值，以及包含函数的调用方中。
*  如果包含的函数是异步函数，控制权返回给当前调用方，并且结果记录的值，如果有的话，是在返回的任务中所述 ([枚举器接口](classes.md#enumerator-interfaces))。

因为`return`语句无条件将控制转移到其他位置，终结点的`return`语句永远不会是可访问。

### <a name="the-throw-statement"></a>Throw 语句

`throw`语句将引发异常。

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

一个`throw`的表达式的语句将引发计算该表达式生成的值。 表达式必须表示类类型的值`System.Exception`，派生的类类型的`System.Exception`类型或类型参数类型具有`System.Exception`（或其中的子类） 用作其有效的基类。 如果该表达式的计算将生成`null`、`System.NullReferenceException`改为引发。

一个`throw`可以仅在使用不包含表达式的语句`catch`阻止，在这种情况下该语句重新引发的异常当前正在处理的`catch`块。

因为`throw`语句无条件将控制转移到其他位置，终结点的`throw`语句永远不会是可访问。

当引发异常时，将控制转移到第一个`catch`子句中封闭`try`能够处理该异常的语句。 从点到点将控制转移到合适的异常处理程序所引发的异常发生过程被称为***异常传播***。 传播异常包含的重复计算以下步骤，直到`catch`找到匹配异常的子句。 在此描述中，***引发点***是最初引发异常时的位置。

*  在当前的函数成员，每个`try`检查封闭引发点的语句。 为每个语句`S`，从最里层开始`try`语句和结束的最外层`try`语句中，按以下步骤进行计算：

   * 如果`try`块`S`封闭引发点，并且 S 有一个或多个`catch`子句，`catch`子句进行检查的出现顺序来查找异常，根据规则中指定一个合适的处理程序部分[try 语句](statements.md#the-try-statement)。 如果匹配`catch`所在的子句，可以通过将控制转移到块的完成异常传播`catch`子句。

   * 否则为如果`try`块或`catch`块`S`封闭引发点; 如果`S`具有`finally`块中，控制传输到`finally`块。 如果`finally`块引发另一个异常，终止当前异常的处理。 否则为当控件到达的终结点时`finally`块中，继续执行当前异常的处理。

*  如果在当前函数调用中未找到异常处理程序函数调用会终止，并出现以下情况之一：

   * 如果当前函数，非异步上面是重复执行步骤函数的调用方使用对应于从中调用的函数成员的语句的引发点。

   * 如果当前函数，async 和返回任务的返回任务，如中所述将置于出现故障或已取消状态中记录异常[枚举器接口](classes.md#enumerator-interfaces)。

   * Async 和返回 void 的当前函数时，当前线程的同步上下文收到通知，如中所述[可枚举接口](classes.md#enumerable-interfaces)。

*  如果异常处理终止当前线程中的所有函数成员调用，表明线程没有处理程序异常，然后在线程本身就是终止。 此类终止的影响是实现定义的。

## <a name="the-try-statement"></a>Try 语句

`try`语句提供了一种机制，用于捕获块的执行过程中发生的异常。 此外，`try`语句提供了指定的块失去控制权时始终执行的代码的能力`try`语句。

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

有三个可能的形式`try`语句：

*  一个`try`块后跟一个或多个`catch`块。
*  一个`try`块后跟`finally`块。
*  一个`try`块后跟一个或多个`catch`块跟`finally`块。

当`catch`子句指定*exception_specifier*，类型必须是`System.Exception`，派生的类型`System.Exception`或具有的类型参数类型`System.Exception`（或其中的子类） 作为其有效基本类。

时`catch`子句同时指定*exception_specifier*与*标识符*、 一个***异常变量***声明的给定名称和类型。 异常变量对应于局部变量，通过进行了扩展作用域`catch`子句。 执行期间*exception_filter*并*块*，异常变量表示当前正在处理的异常。 为了明确赋值检查，异常变量被视为在其整个范围内明确赋值。

除非`catch`子句包括了异常的变量名称，就无法访问在筛选器中的异常对象和`catch`块。

一个`catch`未指定子句*exception_specifier*称为常规`catch`子句。

某些编程语言可能支持不是可表示为一个派生自的异常`System.Exception`，尽管 C# 代码可能永远不会生成此类异常。 一种通用`catch`子句可用于捕获此类异常。 因此，一种通用`catch`子句是从一个指定的类型在语义上不同`System.Exception`、 在于，前者可能也会捕获其他语言中的异常。

若要查找的处理程序异常，`catch`子句词法顺序进行检查。 如果`catch`子句指定某个类型，但没有异常筛选器，为更高版本是编译时错误`catch`在同一子句`try`语句指定的类型相同，或派生自的类型。 如果`catch`子句指定没有类型，并且任何筛选器，它必须是最后一个`catch`子句的`try`语句。

内`catch`块中，`throw`语句 ([throw 语句](statements.md#the-throw-statement)) 不包含表达式可用于重新引发异常的捕获了`catch`块。 对异常变量的赋值不会更改被重新引发的异常。

在示例
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
该方法`F`捕获异常、 一些诊断信息写入控制台，更改异常变量和重新引发的异常。 将重新引发的异常是原始异常，因此生成的输出为：
```
Exception in F: G
Exception in Main: G
```

如果第一个 catch 块必须引发`e`而不是重新引发当前异常，生成的输出将如下：
```csharp
Exception in F: G
Exception in Main: F
```

它是编译时错误`break`， `continue`，或`goto`语句将控制转移出`finally`块。 当`break`， `continue`，或`goto`语句中出现`finally`块中，语句的目标必须是在同一个`finally`块，否则会发生编译时错误或。

它是编译时错误`return`语句出现在`finally`块。

一个`try`执行语句，如下所示：

*  控制转移到其中`try`块。
*  如果控制到达的终结点`try`块：
   *  如果`try`语句具有`finally`块中，`finally`执行块。
   *  控制转移到其中的终结点`try`语句。

*  如果异常传播到`try`语句的执行期间`try`块：
   *  `catch`子句，如果有的话，会按顺序检查的外观以找到合适的异常的处理程序。 如果`catch`子句未指定类型，或指定异常类型或基类型的异常类型：
      *  如果`catch`子句声明了一个异常变量，异常对象分配到的异常变量。
      *  如果`catch`子句声明了一个异常筛选器，则计算筛选器。 如果其计算结果为`false`，catch 子句没有匹配项，并且搜索工作一直持续到任何后续`catch`合适的处理程序的子句。
      *  否则为`catch`子句被认为是匹配项，并控制转移到其中的匹配`catch`块。
      *  如果控制到达的终结点`catch`块：
         * 如果`try`语句具有`finally`块中，`finally`执行块。
         * 控制转移到其中的终结点`try`语句。
      *  如果异常传播到`try`语句的执行期间`catch`块：
         *  如果`try`语句具有`finally`块中，`finally`执行块。
         *  该异常传播到下一步封闭`try`语句。
   *  如果`try`语句没有`catch`子句或如果没有`catch`子句与异常匹配：
      *  如果`try`语句具有`finally`块中，`finally`执行块。
      *  该异常传播到下一步封闭`try`语句。

语句`finally`失去控制权时始终执行块`try`语句。 这是 true 的控制转移是否由于正常执行，因为执行而发生`break`， `continue`， `goto`，或`return`语句，或由于传播出的异常而`try`语句。

如果在执行期间引发异常`finally`阻止，且未被捕获在同一个 finally 块，该异常传播到下一步封闭`try`语句。 如果另一个异常传播的过程中，该异常将丢失。 讨论传播异常的过程中的说明进一步`throw`语句 ([throw 语句](statements.md#the-throw-statement))。

`try`块`try`是可到达的如果`try`语句是可访问。

一个`catch`块`try`是可到达的如果`try`语句是可访问。

`finally`块`try`是可到达的如果`try`语句是可访问。

终结点的`try`语句是可访问，如果下列两个条件成立：

*  终结点`try`块是可访问或结束点的至少一个`catch`块是可访问。
*  如果`finally`块是否存在、 终结点的`finally`块是可访问。

## <a name="the-checked-and-unchecked-statements"></a>Checked 和 unchecked 语句

`checked`并`unchecked`语句用于控制***溢出检查上下文***整型类型算术运算和转换。

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

`checked`语句将导致中的所有表达式*块*若要在 checked 上下文中计算和`unchecked`语句都导致中的所有表达式*块*中要计算未检查的上下文。

`checked`并`unchecked`语句是恰好等同于`checked`并`unchecked`运算符 ([checked 和 unchecked 运算符](expressions.md#the-checked-and-unchecked-operators))，只不过它们对块而不是表达式进行操作.

## <a name="the-lock-statement"></a>Lock 语句

`lock`语句获取给定对象的互斥锁，执行语句，然后释放该锁。

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

表达式`lock`语句的已知类型的值必须属于*reference_type*。 任何隐式装箱的转换 ([装箱转换](conversions.md#boxing-conversions)) 在单次执行的表达式执行`lock`语句，因此它是要表示的值的表达式的编译时错误*value_type*.

一个`lock`窗体的语句
```csharp
lock (x) ...
```
其中`x`的表达式*reference_type*，恰好等同于
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

互斥锁被保留，而在同一执行线程中执行的代码还可以获取和释放锁。 但是，在其他线程中执行的代码被无法获取锁，直到锁被释放。

锁定`System.Type`不建议以同步对静态数据的访问的对象。 其他代码可能会锁定同一类型，可能会导致死锁。 更好的方法是通过锁定私有静态对象同步对静态数据的访问。 例如：
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

`using`语句获得一个或多个资源、 执行语句，然后释放资源。

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

一个***资源***是类或结构实现`System.IDisposable`，其中包括一个名为的无参数方法`Dispose`。 使用资源的代码可以调用`Dispose`来指示不再需要该资源。 如果`Dispose`未调用，则自动可供使用最终会出现由于垃圾回收。

如果的窗体*resource_acquisition*是*local_variable_declaration*的类型*local_variable_declaration*必须是`dynamic`或类型可以隐式转换为`System.IDisposable`。 如果的窗体*resource_acquisition*是*表达式*则此表达式必须是隐式转换为`System.IDisposable`。

在中声明的局部变量*resource_acquisition*是只读的并且必须包含初始值设定项。 如果尝试修改这些本地变量嵌入的语句会发生编译时错误 (通过分配或`++`并`--`运算符)，需要它们的地址，或将它们作为传递`ref`或`out`参数。

一个`using`语句转换为三个部分： 获取、 使用情况和可供使用。 对资源的使用隐式括`try`包含语句`finally`子句。 这`finally`子句释放的资源。 如果`null`获取资源，则没有调用`Dispose`进行，并且不会引发异常。 如果该资源的类型`dynamic`它动态地转换在隐式的动态转换 ([隐式动态转换](conversions.md#implicit-dynamic-conversions)) 到`IDisposable`期间为了确保转换的采集成功执行在使用情况和可供使用。

一个`using`窗体的语句
```csharp
using (ResourceType resource = expression) statement
```
对应于一个三个可能的扩展。 当`ResourceType`是不可以为 null 的值类型，在扩展
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

否则为当`ResourceType`而不是可以为 null 值类型还是引用类型`dynamic`，扩展
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

否则为当`ResourceType`是`dynamic`，扩展
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

在任一扩展`resource`变量是只读中嵌入的语句和`d`变量到嵌入的语句是在中，不可访问，并且不可见。

实现允许以不同的方式，例如出于性能原因，实现给定的 using 语句，前提是与上述展开一致的行为。

一个`using`窗体的语句
```csharp
using (expression) statement
```
具有相同的三个可能的扩展。 在这种情况下`ResourceType`是隐式的编译时类型`expression`，如果有的话。 否则为接口`IDisposable`本身用作`ResourceType`。 `resource`变量到嵌入的语句是在中，不可访问，并且不可见。

当*resource_acquisition*采用的形式*local_variable_declaration*，可以获取给定类型的多个资源。 一个`using`窗体的语句
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
精确地等效于一系列嵌套`using`语句：
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

下面的示例创建名为的文件`log.txt`和两行文本写入文件。 该示例然后打开该文件进行读取，并将所包含的文本行复制到控制台。
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

由于`TextWriter`并`TextReader`类将实现`IDisposable`接口，该示例可以使用`using`语句，以确保基础文件正确关闭以下写入或读取操作。

## <a name="the-yield-statement"></a>Yield 语句

`yield`迭代器块中使用语句 ([块](statements.md#blocks)) 以生成枚举数对象值 ([枚举器对象](classes.md#enumerator-objects)) 或可枚举对象 ([可枚举对象](classes.md#enumerable-objects))迭代器，或在迭代结束的信号。

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

`yield` 不是保留的字;它具有特殊含义仅当使用紧靠`return`或`break`关键字。 在其他上下文中`yield`可以用作标识符。

在哪里有多个限制`yield`出现语句，如以下所述。

*  它是编译时错误`yield`语句 （包括两种形式） 显示之外*method_body*， *operator_body*或*accessor_body*
*  它是编译时错误`yield`（的两种形式） 的语句才会出现在匿名函数内。
*  它是编译时错误`yield`语句 （包括两种形式） 出现在`finally`子句的`try`语句。
*  它是编译时错误`yield return`语句出现在任何位置`try`包含任何语句`catch`子句。

下面的示例显示了一些有效和无效的用法`yield`语句。

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

隐式转换 ([隐式转换](conversions.md#implicit-conversions)) 必须存在从类型中的表达式`yield return`语句产生类型 ([产生类型](classes.md#yield-type)) 的迭代器。

一个`yield return`执行语句，如下所示：

*  在语句中给定的表达式是计算、 隐式转换为 yield 类型和分配给`Current`枚举器对象的属性。
*  挂起的迭代器块的执行。 如果`yield return`语句是在一个或多个`try`阻止，关联`finally`块不会在这一次执行。
*  `MoveNext`的枚举器对象的方法将返回`true`，该值指示枚举器对象成功地推进到下一项，其调用方。

枚举数对象的下一步调用`MoveNext`方法恢复执行迭代器块从上次已挂起。

一个`yield break`执行语句，如下所示：

*  如果`yield break`语句包含一个或多个`try`相关联的块`finally`块，控制最初为传输`finally`块的最内层`try`语句。 如果控制到达的终结点`finally`块中，控制传输到`finally`的下一步封闭块`try`语句。 重复此过程，直到`finally`所有封闭的块`try`已执行语句。
*  控制权返回给调用方的迭代器块。 这可以是`MoveNext`方法或`Dispose`枚举器对象的方法。

因为`yield break`语句无条件将控制转移到其他位置，终结点的`yield break`语句永远不会是可访问。
