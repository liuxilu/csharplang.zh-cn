---
ms.openlocfilehash: f61039abd6bd557ac0ea625e6aac1c8bafa57b02
ms.sourcegitcommit: e134bb7058e9848120b93b345f96d6ac0cb8c815
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/17/2020
ms.locfileid: "71704079"
---
# <a name="expressions"></a>表达式

表达式是运算符和操作数构成的序列。 本章定义语法、操作数和运算符的计算顺序，以及表达式的含义。

## <a name="expression-classifications"></a>表达式分类

表达式分类为以下类别之一：

*  一个 值。 每个值都有关联的类型。
*  一个变量。 每个变量都有关联的类型，即变量的声明类型。
*  一个命名空间。 具有此分类的表达式只能作为*member_access* （[成员访问](expressions.md#member-access)）的左侧出现。 在任何其他上下文中，归类为命名空间的表达式会导致编译时错误。
*  一种类型。 具有此分类的表达式只能作为*member_access* （[成员访问](expressions.md#member-access)）的左侧，或者作为 `as` 运算符（[as 运算符](expressions.md#the-as-operator)）、`is` 运算符（[is 运算符](expressions.md#the-is-operator)）或 `typeof` 运算符（[typeof 运算符](expressions.md#the-typeof-operator)）的操作数出现。 在其他任何上下文中，归类为类型的表达式会导致编译时错误。
*  一个方法组，它是从成员查找（[成员查找](expressions.md#member-lookup)）生成的一组重载方法。 方法组可以有一个关联的实例表达式和一个关联的类型参数列表。 调用实例方法时，实例表达式的计算结果将成为由 `this` （[此访问](expressions.md#this-access)）表示的实例。 *Invocation_expression* （[调用表达式](expressions.md#invocation-expressions)）、 *delegate_creation_expression* （[委托创建表达式](expressions.md#delegate-creation-expressions)）和 is 运算符的左侧都允许使用方法组，并可将其隐式转换为兼容的委托类型（[方法组转换](conversions.md#method-group-conversions)）。 在其他任何上下文中，归类为方法组的表达式会导致编译时错误。
*  空文本。 具有此分类的表达式可隐式转换为引用类型或可以为 null 的类型。
*  匿名函数。 具有此分类的表达式可隐式转换为兼容的委托类型或表达式目录树类型。
*  属性访问。 每个属性访问都有关联的类型，即属性的类型。 而且，属性访问可能具有关联的实例表达式。 调用实例属性访问的访问器（`get` 或 `set` 块）时，实例表达式的计算结果将成为 `this` （[此访问](expressions.md#this-access)）表示的实例。
*  事件访问。 每个事件访问都有关联的类型，即事件的类型。 而且，事件访问可能具有关联的实例表达式。 事件访问可能显示为 `+=` 和 `-=` 运算符（[事件分配](expressions.md#event-assignment)）的左操作数。 在其他任何上下文中，归类为事件访问的表达式会导致编译时错误。
*  索引器访问。 每个索引器访问都有关联的类型，即索引器的元素类型。 此外，索引器访问具有关联的实例表达式和关联的参数列表。 调用索引器访问的访问器（`get` 或 `set` 块）时，实例表达式的计算结果将成为 `this` （[此访问](expressions.md#this-access)）表示的实例，而参数列表的计算结果将成为调用的参数列表。
*  无变化。 当表达式是 `void`的返回类型的方法的调用时，会发生这种情况。 归类为 nothing 的表达式仅在*statement_expression* （[expression 语句](statements.md#expression-statements)）的上下文中有效。

表达式的最终结果绝不会是命名空间、类型、方法组或事件访问。 相反，如上所述，这些类别的表达式是仅在某些上下文中允许的中间构造。

通过执行*get 访问器*或*set 访问器*的调用，始终可以将属性访问或索引器访问重新分类为值。 特定访问器由属性或索引器访问的上下文确定：如果访问是赋值的目标，则调用*set 访问器*来分配新值（[简单赋值](expressions.md#simple-assignment)）。 否则，将调用*get 访问器*以获取当前值（[表达式的值](expressions.md#values-of-expressions)）。

### <a name="values-of-expressions"></a>表达式的值

大多数涉及表达式的构造都需要表达式来表示***值***。 在这种情况下，如果实际表达式表示命名空间、类型、方法组或不执行任何操作，则会发生编译时错误。 但是，如果表达式表示属性访问、索引器访问或变量，则将隐式替换属性、索引器或变量的值：

*  变量的值只是当前存储在变量标识的存储位置中的值。 必须先将变量视为明确赋值（[明确赋值](variables.md#definite-assignment)），才能获得其值，否则将发生编译时错误。
*  属性访问表达式的值是通过调用属性的*get 访问器*获取的。 如果该属性没有*get 访问器*，则会发生编译时错误。 否则，将执行函数成员调用（对[动态重载决策进行编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)），并且调用的结果将成为属性访问表达式的值。
*  索引器访问表达式的值是通过调用索引器的*get 访问器*获取的。 如果索引器没有*get 访问器*，则会发生编译时错误。 否则，将使用与索引器访问表达式关联的参数列表来执行函数成员调用（[动态重载决策的编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)），并且调用的结果将成为索引器访问表达式的值。

## <a name="static-and-dynamic-binding"></a>静态和动态绑定

根据构成表达式的类型或值（参数、操作数、接收方）确定操作含义的过程通常称为 "***绑定***"。 例如，方法调用的含义是根据接收方和参数的类型确定的。 运算符的含义取决于其操作数的类型。

在C#编译时，通常基于其构成表达式的编译时类型确定操作的含义。 同样，如果表达式包含错误，编译器将检测并报告错误。 此方法称为***静态绑定***。

但是，如果表达式是动态表达式（即类型为 `dynamic`），则表示它参与的任何绑定都应基于其运行时类型（即，它在运行时表示的对象的实际类型），而不是它在编译时所具有的类型。 这样，此类操作的绑定就会推迟到运行该程序的时间。 这称为***动态绑定***。

当动态绑定操作时，编译器不会执行任何检查。 相反，如果运行时绑定失败，则在运行时将错误报告为异常。

中C#的以下操作服从绑定：

*  成员访问： `e.M`
*  方法调用： `e.M(e1, ..., eN)`
*  委托调用：`e(e1, ..., eN)`
*  元素访问： `e[e1, ..., eN]`
*  对象创建： `new C(e1, ..., eN)`
*  重载的一元运算符： `+`、`-`、`!`、`~`、`++`、`--`、`true`、`false`
*  重载的二元运算符： `+`、`-`、`*`、`/`、`%`、`&`、`&&`、`|`、`||`、`??`、`^`、`<<`、`>>`、`==`、`!=`、`>`、`<`、`>=`、`<=`
*  赋值运算符： `=`、`+=`、`-=`、`*=`、`/=`、`%=`、`&=`、`|=`、`^=`、`<<=`、`>>=`
*  隐式和显式转换

如果不涉及动态表达式， C#则默认为静态绑定，这意味着在选择过程中使用构成表达式的编译时类型。 但是，当上面列出的操作中的其中一个构成表达式为动态表达式时，将改为动态绑定该操作。

### <a name="binding-time"></a>绑定时间

在编译时进行静态绑定，而动态绑定在运行时发生。 在下面几节中，术语 "***绑定时间***" 指编译时或运行时，具体取决于绑定发生的时间。

下面的示例演示了静态和动态绑定的概念以及绑定时间：
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

前两个调用是静态绑定的：根据参数的编译时类型选取 `Console.WriteLine` 的重载。 因此，绑定时间为编译时。

第三个调用是动态绑定的：根据参数的运行时类型选取 `Console.WriteLine` 的重载。 出现这种情况的原因是，参数是动态表达式--它的编译时类型是 `dynamic`。 因此，第三次调用的绑定时间是运行时。

### <a name="dynamic-binding"></a>动态绑定

动态绑定的目的是允许C#程序与***动态对象***交互，即不遵循C#类型系统的常规规则的对象。 动态对象可以是具有不同类型系统的其他编程语言中的对象，也可以是以编程方式设置以实现不同操作的绑定语义的对象。

动态对象实现其自身语义的机制是定义的实现。 定义的给定接口再次实现--由动态对象实现，以向C#运行时发出信号，指示它们具有特殊语义。 因此，无论动态对象上的操作是动态绑定的，无论是在该文档中指定的C# ，而不是在本文档中指定的语义，都需要接管。

尽管动态绑定的目的是允许与动态对象进行互操作， C#但允许动态绑定所有对象，无论它们是否为动态的。 这允许动态对象的更平滑集成，因为它们的操作结果可能不是动态对象，而是在编译时对程序员来说是未知类型。 此外，动态绑定还有助于消除容易出错的基于反射的代码，即使在没有任何对象是动态对象时也是如此。

以下各节描述了在应用动态绑定时与语言中的每个构造完全相同的内容、所应用的编译时检查（如果有），以及编译时结果和表达式分类的定义。

### <a name="types-of-constituent-expressions"></a>构成表达式的类型

静态绑定操作时，构成表达式的类型（例如，接收方、参数、索引或操作数）始终被认为是该表达式的编译时类型。

动态绑定操作时，将根据构成表达式的编译时类型以不同的方式确定构成表达式的类型：

*  编译时类型 `dynamic` 的构成表达式被视为在运行时使用表达式计算结果的实际值类型
*  在运行时将其编译时类型为类型参数的构成表达式视为具有类型参数绑定到的类型
*  否则，构成表达式被视为具有其编译时类型。

## <a name="operators"></a>运算符

表达式是从***操作数***和***运算符***构造的。 表达式的运算符指明了向操作数应用的运算。 运算符的示例包括 `+`、`-`、`*`、`/` 和 `new`。 操作数的示例包括文本、字段、局部变量和表达式。

有三种类型的运算符：

*  一元运算符。 一元运算符采用一个操作数，并使用前缀表示法（如 `--x`）或后缀表示法（如 `x++`）。
*  二元运算符。 二元运算符采用两个操作数，并使用中缀表示法（如 `x + y`）。
*  三元运算符。 只存在一个三元运算符 `?:`;它采用三个操作数，并使用中缀表示法（`c ? x : y`）。

表达式中运算符的计算顺序由运算符的***优先级***和***关联***性（[运算符优先级和结合](expressions.md#operator-precedence-and-associativity)性）决定。

表达式中的操作数从左到右进行计算。 例如，在 `F(i) + G(i++) * H(i)`中，使用旧值 `i`调用方法 `F`，然后使用旧值 `i`调用方法 `G`，最后，将使用新值 `H` 调用方法 `i`。 这与运算符优先级不同。

特定的运算符可***重载***。 运算符重载允许为操作指定用户定义的运算符实现，其中一个或两个操作数属于用户定义的类或结构类型（[运算符重载](expressions.md#operator-overloading)）。

### <a name="operator-precedence-and-associativity"></a>运算符优先级和关联性

如果表达式包含多个运算符，那么运算符的***优先级***决定了各个运算符的计算顺序。 例如，表达式 `x + y * z` 的计算结果为 `x + (y * z)`，因为 `*` 运算符的优先级高于 binary `+` 运算符。 运算符的优先级由其关联的语法生产的定义来确定。 例如， *additive_expression*由 `+` 或 `-` 运算符分隔的一系列*multiplicative_expression*组成，从而使 `+` 和 `-` 运算符的优先级低于 `*`、`/`和 `%` 运算符。

下表按优先级从高到低的顺序汇总了所有运算符：

| __Section__                                                                                   | __类别__                | __运算符__ | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [主要表达式](expressions.md#primary-expressions)                                     | Primary                     | `x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate` | 
| [一元运算符](expressions.md#unary-operators)                                             | 一元                       | `+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x` | 
| [算术运算符](expressions.md#arithmetic-operators)                                   | 乘法              | `*`  `/`  `%` | 
| [算术运算符](expressions.md#arithmetic-operators)                                   | 加法                    | `+`  `-`      | 
| [移位运算符](expressions.md#shift-operators)                                             | 移位                       | `<<`  `>>`    | 
| [关系和类型测试运算符](expressions.md#relational-and-type-testing-operators) | 关系和类型测试 | `<`  `>`  `<=`  `>=`  `is`  `as` | 
| [关系和类型测试运算符](expressions.md#relational-and-type-testing-operators) | 相等                    | `==`  `!=`    | 
| [逻辑运算符](expressions.md#logical-operators)                                         | 逻辑“与”                 | `&`           | 
| [逻辑运算符](expressions.md#logical-operators)                                         | 逻辑 XOR                 | `^`           | 
| [逻辑运算符](expressions.md#logical-operators)                                         | 逻辑“或”                  | <code>&#124;</code>           |
| [条件逻辑运算符](expressions.md#conditional-logical-operators)                 | 条件“与”             | `&&`          | 
| [条件逻辑运算符](expressions.md#conditional-logical-operators)                 | 条件“或”              | <code>&#124;&#124;</code>          | 
| [Null 合并运算符](expressions.md#the-null-coalescing-operator)                   | null 合并             | `??`          | 
| [条件运算符](expressions.md#conditional-operator)                                   | 条件                 | `?:`          | 
| [赋值运算符](expressions.md#assignment-operators)，[匿名函数表达式](expressions.md#anonymous-function-expressions)  | 赋值和 lambda 表达式 | `=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>` | 

如果两个运算符在优先级相同的两个运算符之间出现，则运算符的关联性将控制运算的执行顺序：

*  除了赋值运算符和 null 合并运算符外，所有二元运算符都是***左结合***运算符，这意味着运算从左至右执行。 例如， `x + y + z` 将计算为 `(x + y) + z`。
*  赋值运算符、null 合并运算符和条件运算符（`?:`）是***右结合***运算符，这意味着运算从右到左执行。 例如， `x = y = z` 将计算为 `x = (y = z)`。

可以使用括号控制优先级和结合性。 例如，`x + y * z` 先计算 `y` 乘 `z`，并将结果与 `x` 相加，而 `(x + y) * z` 则先计算 `x` 加 `y`，然后将结果与 `z` 相乘。

### <a name="operator-overloading"></a>运算符重载

所有一元运算符和二元运算符都具有在任何表达式中自动可用的预定义实现。 除了预定义的实现外，还可以通过在类和结构（[运算符](classes.md#operators)）中包括 `operator` 声明来引入用户定义的实现。 用户定义的运算符实现始终优先于预定义运算符实现：只有当不存在适用的用户定义运算符实现时，才会考虑预定义运算符实现，如[一元运算符重载决策](expressions.md#unary-operator-overload-resolution)和[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)中所述。

可***重载的一元运算符***是：
```csharp
+   -   !   ~   ++   --   true   false
```

尽管 `true` 和 `false` 并不显式在表达式中使用（因此它们不会以[运算符优先级和相关性](expressions.md#operator-precedence-and-associativity)的优先级表的顺序包括在内），但是它们被视为运算符，因为它们是在多个表达式上下文中调用的：布尔表达式（[布尔表达式](expressions.md#boolean-expressions)）和涉及条件[运算符](expressions.md#conditional-operator)的表达式（条件[逻辑](expressions.md#conditional-logical-operators)运算符）。

可***重载的二元运算符***是：
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

只能重载上面列出的运算符。 具体而言，无法重载成员访问、方法调用或 `=`、`&&`、`||`、`??`、`?:`、`=>`、`checked`、`unchecked`、`new`、`typeof`、`default`、`as`和 `is` 运算符。

重载二元运算符时，也会隐式重载相应的赋值运算符（若有）。 例如，运算符 `*` 的重载也是运算符 `*=`的重载。 这将在[复合赋值](expressions.md#compound-assignment)中进一步说明。 请注意，赋值运算符本身（`=`）无法重载。 赋值始终将值的简单的逐位副本用于变量。

强制转换操作（如 `(T)x`）是通过提供用户定义的转换（[用户定义的转换](conversions.md#user-defined-conversions)）而重载的。

元素访问（如 `a[x]`）不被视为可重载的运算符。 而是通过索引器（[索引器](classes.md#indexers)）支持用户定义的索引。

在表达式中，运算符是使用运算符表示法引用的，而在声明中，使用函数表示法引用运算符。 下表显示了一元运算符和二元运算符的运算符和函数表示法之间的关系。 在第一个条目中， *op*表示任何可重载的一元前缀运算符。 在第二个条目中， *op*表示一元后缀 `++` 和 `--` 运算符。 在第三个条目中， *op*表示任何可重载的二元运算符。


| __运算符表示法__ | __函数表示法__ |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

用户定义的运算符声明始终需要至少一个参数成为包含运算符声明的类或结构类型。 因此，用户定义的运算符不能与预定义的运算符具有相同的签名。

用户定义的运算符声明不能修改运算符的语法、优先级或关联性。 例如，`/` 运算符始终为二元运算符，始终具有在[运算符优先级和关联](expressions.md#operator-precedence-and-associativity)性中指定的优先级别，并且始终为左结合。

尽管用户定义的运算符可以执行 pleases 的任何计算，但强烈不建议使用生成的实现，而不是所需的结果。 例如，`operator ==` 的实现应比较两个操作数的相等性并返回相应的 `bool` 结果。

通过[条件逻辑运算符](expressions.md#conditional-logical-operators)对[主要表达式](expressions.md#primary-expressions)中的单个运算符进行的说明指定运算符的预定义实现以及适用于每个运算符的任何其他规则。 这些说明使用术语 "***一元运算符重载解析***"、"***二元运算符重载解析***" 和 "***数值升级***"，其中包括以下各节。

### <a name="unary-operator-overload-resolution"></a>一元运算符重载决策

`op x` 或 `x op`形式的操作（其中 `op` 是一个可重载的一元运算符），并且 `x` 是 `X`类型的表达式，则按以下方式处理：

*  `X` 为操作 `operator op(x)` 提供的候选用户定义运算符集是使用[候选用户定义运算符](expressions.md#candidate-user-defined-operators)的规则确定的。
*  如果候选用户定义的运算符集不为空，则这将成为操作的候选运算符集。 否则，预定义的一元 `operator op` 实现（包括其提升形式）会成为操作的候选运算符集。 给定运算符的预定义实现在运算符的说明（[主表达式](expressions.md#primary-expressions)和[一元运算符](expressions.md#unary-operators)）中指定。
*  [重载](expressions.md#overload-resolution)决策的重载决策规则应用于候选运算符集，以选择与 `(x)`参数列表相关的最佳运算符，而此运算符将成为重载决策过程的结果。 如果重载决策未能选择单个最佳运算符，则会发生绑定时错误。

### <a name="binary-operator-overload-resolution"></a>二元运算符重载决策

`x op y`格式的操作，其中 `op` 是可重载的二元运算符，`x` 是 `X`类型的表达式，而 `y` 是类型 `Y`的表达式，则按如下方式进行处理：

*  确定操作 `operator op(x,y)` `X` 和 `Y` 提供的候选用户定义运算符集。 该集由 `X` 提供的候选运算符与 `Y`提供的候选运算符联合组成，每个运算符都是使用[候选用户定义的运算符](expressions.md#candidate-user-defined-operators)的规则确定的。 如果 `X` 和 `Y` 属于同一类型，或者如果 `X` 和 `Y` 派生自一个公共的基类型，则仅在组合集内发生一次共享候选运算符。
*  如果候选用户定义的运算符集不为空，则这将成为操作的候选运算符集。 否则，预定义的二进制 `operator op` 实现（包括其提升形式）会成为操作的候选运算符集。 给定运算符的预定义实现是在运算符的说明（通过[条件逻辑运算符](expressions.md#conditional-logical-operators)进行[算术运算符](expressions.md#arithmetic-operators)）中指定的。 对于预定义的枚举和委托运算符，唯一认为的运算符是由作为一个操作数的绑定时类型的枚举或委托类型定义的运算符。
*  [重载](expressions.md#overload-resolution)决策的重载决策规则应用于候选运算符集，以选择与 `(x,y)`参数列表相关的最佳运算符，而此运算符将成为重载决策过程的结果。 如果重载决策未能选择单个最佳运算符，则会发生绑定时错误。

### <a name="candidate-user-defined-operators"></a>候选用户定义的运算符

给定一个类型 `T` 和一个操作 `operator op(A)`，其中 `op` 是一个可重载的运算符，并且 `A` 是一个参数列表，则按如下方式确定 `T` 为 `operator op(A)` 提供的候选用户定义运算符集：

*  确定 `T0`类型。 如果 `T` 是可以为 null 的类型，则 `T0` 是其基础类型，否则 `T0` 等于 `T`。
*  对于 `T0` 中的所有 `operator op` 声明以及此类运算符的所有提升形式，如果至少有一个[运算符适用于](expressions.md#applicable-function-member)参数列表 `A`，则候选运算符集包含 `T0`中的所有此类适用运算符。
*  否则，如果 `object``T0`，则候选运算符集为空。
*  否则，`T0` 提供的候选运算符集是 `T0`的直接基类提供的候选运算符集，如果 `T0` 为类型参数，则为 `T0` 的有效基类。

### <a name="numeric-promotions"></a>数值提升

数值提升包括自动执行预定义的一元运算符和二元数值运算符的操作数的某些隐式转换。 数值升级不是一种不同的机制，而是将重载决策应用于预定义运算符的效果。 尽管可以实现用户定义的运算符来表现出类似的效果，但具体的数值升级却不会影响用户定义的运算符的计算。

作为数值升级的一个示例，请考虑二元 `*` 运算符的预定义实现：

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

当重载决策规则（[重载决策](expressions.md#overload-resolution)）应用于这组运算符时，其效果是选择在操作数类型中存在隐式转换的第一个运算符。 例如，对于操作 `b * s`，其中 `b` 为 `byte`，而 `s` 为 `short`，则重载决策将 `operator *(int,int)` 选为最佳运算符。 因此，其效果是将 `b` 和 `s` 转换为 `int`，并 `int`结果的类型。 同样，对于操作 `i * d`（其中，`i` 是一个 `int` 并且 `d` 为 `double`），重载决策选择 `operator *(double,double)` 作为最佳运算符。

#### <a name="unary-numeric-promotions"></a>一元数值促销

一元数值升级发生于预定义 `+`、`-`和 `~` 一元运算符的操作数。 一元数值提升只包含将类型 `sbyte`、`byte`、`short`、`ushort`或 `char` 的操作数转换为类型 `int`。 此外，对于一元 `-` 运算符，一元数值提升将类型 `uint` 的操作数转换为类型 `long`。

#### <a name="binary-numeric-promotions"></a>二进制数值升级

二元数值升级发生在预定义 `+`、`-`、`*`、`/`、`%`、`&`、`|`、`^`、`==`、`!=`、`>`、`<`、`>=`和 `<=` 二元运算符的操作数。 二进制数值升级会隐式地将两个操作数转换为通用类型，在非关系运算符的情况下，也会成为运算的结果类型。 二进制数值升级包括应用下列规则，顺序如下：

*  如果任一操作数为类型 `decimal`，则将另一个操作数转换为类型 `decimal`，如果另一个操作数的类型为 `float` 或 `double`，则会发生绑定时错误。
*  否则，如果其中一个操作数为 `double`类型，则另一个操作数将转换为类型 `double`。
*  否则，如果其中一个操作数为 `float`类型，则另一个操作数将转换为类型 `float`。
*  否则，如果其中一个操作数为 `ulong`类型，则另一个操作数将转换为类型 `ulong`，如果另一个操作数的类型为 `sbyte`、`short`、`int`或 `long`，则会发生绑定时错误。
*  否则，如果其中一个操作数为 `long`类型，则另一个操作数将转换为类型 `long`。
*  否则，如果其中一个操作数的类型为 `uint`，另一个操作数的类型为 `sbyte`、`short`或 `int`，则这两个操作数都将转换为类型 `long`。
*  否则，如果其中一个操作数为 `uint`类型，则另一个操作数将转换为类型 `uint`。
*  否则，两个操作数都将转换为 `int`类型。

请注意，第一个规则禁止将 `decimal` 类型与 `double` 和 `float` 类型一起使用的任何操作。 规则如下所示，这是因为 `decimal` 类型与 `double` 和 `float` 类型之间没有隐式转换。

另请注意，如果另一个操作数为有符号整数类型，则操作数不能 `ulong` 类型。 原因在于，不存在可以表示全部 `ulong` 范围和有符号整数类型的整数类型。

在上述两种情况下，强制转换表达式可用于将一个操作数显式转换为与另一个操作数兼容的类型。

示例中
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
发生绑定时错误，因为 `decimal` 不能与 `double`相乘。 通过将第二个操作数显式转换为 `decimal`来解决此错误，如下所示：

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a>提升的运算符

***提升运算符***允许对不可以为 null 的值类型进行运算的预定义运算符和用户定义的运算符也可用于这些类型的可以为 null 的形式。 提升运算符是根据满足某些要求的预定义运算符和用户定义的运算符构造的，如下所述：

*   对于一元运算符

    ```csharp
    +  ++  -  --  !  ~
    ```

    如果操作数和结果类型都是不可为 null 的值类型，则会存在运算符的提升形式。 提升形式是通过向操作数和结果类型添加单个 `?` 修饰符来构造的。 如果操作数为 null，则提升运算符将生成 null 值。 否则，提升运算符将对操作数进行解包，应用基础运算符并包装结果。

*   对于二元运算符

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    如果操作数和结果类型都是不可以为 null 的值类型，则该运算符的提升形式存在。 提升形式是通过向每个操作数和结果类型添加单个 `?` 修饰符来构造的。 如果有一个或两个操作数为空（`bool?` 类型的 `&` 和 `|` 运算符的异常，如[布尔逻辑运算符](expressions.md#boolean-logical-operators)中所述），则提升的运算符将生成 null 值。 否则，提升运算符将对操作数进行解包，应用基础运算符并包装结果。

*   对于相等运算符

    ```csharp
    ==  !=
    ```

    如果操作数类型既是不可为 null 的值类型，也不是 `bool`的结果类型，则运算符的提升形式存在。 提升形式是通过向每个操作数类型添加单个 `?` 修饰符来构造的。 提升的运算符将两个 null 值视为相等，并将 null 值视为非 null 值。 如果两个操作数都非空，则提升运算符将对操作数进行解包，并应用基础运算符来生成 `bool` 结果。

*   对于关系运算符

    ```csharp
    <  >  <=  >=
    ```

    如果操作数类型既是不可为 null 的值类型，也不是 `bool`的结果类型，则运算符的提升形式存在。 提升形式是通过向每个操作数类型添加单个 `?` 修饰符来构造的。 如果一个或两个操作数为 null，则提升运算符将生成值 `false`。 否则，提升运算符将对操作数进行解包，并应用基础运算符来产生 `bool` 结果。

## <a name="member-lookup"></a>成员查找

成员查找是指确定类型上下文中名称含义的进程。 成员查找在计算表达式中的*simple_name* （[简单名称](expressions.md#simple-names)）或*member_access* （[成员访问](expressions.md#member-access)）的过程中可能发生。 如果*simple_name*或*member_access*作为*invocation_expression* （[方法调用](expressions.md#method-invocations)）的*primary_expression*出现，则称成员被调用。

如果成员是方法或事件，或者它是委托类型（[委托](delegates.md)）或类型 `dynamic` （[动态类型](types.md#the-dynamic-type)）的常量、字段或属性，则称成员是*invocable*。

成员查找不仅考虑了成员的名称，还考虑了成员具有的类型参数的数目以及该成员是否可访问。 出于成员查找的目的，泛型方法和嵌套泛型类型具有其各自声明中指示的类型参数的数目，并且所有其他成员都有零个类型参数。

类型 `T` 的 `K` 类型参数 `N` 名称的成员查找按如下方式进行处理：

*  首先，确定一组名为 `N` 的可访问成员：
    * 如果 `T` 是类型参数，则该集是在每个指定为 `object` `N` `T`的主约束或辅助约束（[类型参数约束](classes.md#type-parameter-constraints)）的每个类型中名为 `N` 的可访问成员集的并集。
    * 否则，该集由 `T`中名为 `N` 的所有可访问（[成员访问](basic-concepts.md#member-access)）成员组成，其中包括 `object`中名为 `N` 的继承成员和可访问成员。 如果 `T` 是构造类型，则通过替换类型参数来获取成员集，如[构造类型成员](classes.md#members-of-constructed-types)中所述。 包含 `override` 修饰符的成员将从集合中排除。
*  接下来，如果 `K` 为零，则将删除所有声明都包含类型参数的嵌套类型。 如果 `K` 不为零，则删除所有具有不同数量的类型参数的成员。 请注意，当 `K` 为零时，不会删除具有类型参数的方法，因为类型推理过程（[类型推理](expressions.md#type-inference)）可能能够推断类型参数。
*  接下来，如果*调用*成员，则将从集合中删除所有非*invocable*成员。
*  接下来，将从集合中删除其他成员隐藏的成员。 对于集中的每个成员 `S.M` （其中 `S` 是声明成员 `M` 的类型），将应用以下规则：
    * 如果 `M` 是常量、字段、属性、事件或枚举成员，则将从集合中删除在 `S` 的基类型中声明的所有成员。
    * 如果 `M` 是类型声明，则从该集中删除所有在 `S` 的基类型中声明的非类型，并且与在 `S` 的基类型中声明的 `M` 类型参数相同的所有类型声明都将从集合中删除。
    * 如果 `M` 是一种方法，则会从该集内删除在 `S` 的基类型中声明的所有非方法成员。
*  接下来，将从集合中删除类成员隐藏的接口成员。 仅当 `T` 是类型参数并且 `T` 既具有 `object` 的有效基类，也具有非空的有效接口集（[类型参数约束](classes.md#type-parameter-constraints)）时，此步骤才会生效。 对于集合中的每个成员 `S.M` （其中 `S` 是声明成员 `M` 的类型），如果 `S` 为除 `object`之外的类声明，则应用以下规则：
    * 如果 `M` 是常量、字段、属性、事件、枚举成员或类型声明，则会从集中删除在接口声明中声明的所有成员。
    * 如果 `M` 是方法，则将从集合中删除在接口声明中声明的所有非方法成员，并且将从该集内删除与在接口声明中声明的 `M` 具有相同签名的所有方法。
*  最后，删除隐藏成员后，将确定查找的结果：
    * 如果集由不是方法的单个成员组成，则此成员是查找的结果。
    * 否则，如果该集仅包含方法，则此方法组是查找的结果。
    * 否则，查找将不明确，并发生绑定时错误。

对于类型参数和接口以外的类型中的成员查找以及严格为单一继承的接口中的成员查找（继承链中的每个接口都具有完全零或一个直接基接口），查找规则的效果是只是派生成员将隐藏具有相同名称或签名的基成员。 这种单一继承查找从来都不明确。 [接口成员访问](interfaces.md#interface-member-access)中介绍了可能从多继承接口中的成员查找产生的歧义。

### <a name="base-types"></a>基类型

出于成员查找的目的，类型 `T` 被视为具有以下基本类型：

*  如果 `object``T`，则 `T` 没有基类型。
*  如果 `T` 是*enum_type*，则 `T` 的基类型是类类型 `System.Enum`、`System.ValueType`和 `object`。
*  如果 `T` 是*struct_type*，则 `T` 的基类型是类类型 `System.ValueType` 和 `object`。
*  如果 `T` 是*class_type*，则 `T` 的基类型是 `T`的基类，包括类类型 `object`。
*  如果 `T` 是*interface_type*，则 `T` 的基类型是 `T` 和类类型 `object`的基接口。
*  如果 `T` 是*array_type*，则 `T` 的基类型是类类型 `System.Array` 和 `object`。
*  如果 `T` 是*delegate_type*，则 `T` 的基类型是类类型 `System.Delegate` 和 `object`。

## <a name="function-members"></a>函数成员

函数成员是包含可执行语句的成员。 函数成员始终是类型的成员，不能是命名空间的成员。 C#定义以下类别的函数成员：

*  方法
*  属性
*  Events
*  索引器
*  用户定义的运算符
*  实例构造函数
*  静态构造函数
*  析构函数。

除析构函数和静态构造函数（无法显式调用）以外，函数成员中包含的语句是通过函数成员调用来执行的。 用于编写函数成员调用的实际语法取决于特定的函数成员类别。

函数成员调用的参数列表（[参数](expressions.md#argument-lists)列表）为函数成员的参数提供实际值或变量引用。

调用泛型方法时，可以使用类型推理来确定要传递给方法的类型参数集。 此过程在[类型推理](expressions.md#type-inference)中进行了介绍。

调用方法、索引器、运算符和实例构造函数会使用重载决策来确定要调用的候选函数成员集中哪一组。 [重载决策](expressions.md#overload-resolution)中介绍了此过程。

在绑定时确定了特定函数成员（可能通过重载决策）后，调用函数成员的实际运行时过程将在[编译时检查动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution)中进行说明。

下表总结了构造中发生的处理，涉及到可显式调用的六类函数成员。 在表中，`e`、`x`、`y`和 `value` 指示归类为变量或值的表达式，`T` 指示归类为类型的表达式，`F` 是方法的简单名称，`P` 是属性的简单名称。


| __构造__     | __示例__    | __描述__ |
|-------------------|----------------|-----------------|
| 方法调用 | `F(x,y)`       | 应用重载决策，以选择包含类或结构中 `F` 的最佳方法。 方法是通过参数列表 `(x,y)`调用的。 如果未 `static`方法，则 `this`实例表达式。 | 
|                   | `T.F(x,y)`     | 应用重载决策，以选择类或结构 `T`中 `F` 的最佳方法。 如果未 `static`方法，则会发生绑定时错误。 方法是通过参数列表 `(x,y)`调用的。 | 
|                   | `e.F(x,y)`     | 重载决策适用于在类、结构或接口中选择由 `e`类型给定的最佳方法 F。 如果 `static`方法，则会发生绑定时错误。 方法是通过实例表达式 `e` 调用的，而参数列表 `(x,y)`。 | 
| 和   | `P`            | 调用包含类或结构中的属性 `P` 的 `get` 访问器。 如果 `P` 是只写的，则会发生编译时错误。 如果未 `static``P`，则 `this`实例表达式。 | 
|                   | `P = value`    | 包含类或结构中的属性 `P` 的 `set` 访问器是通过参数列表 `(value)`调用的。 如果 `P` 是只读的，则会发生编译时错误。 如果未 `static``P`，则 `this`实例表达式。 | 
|                   | `T.P`          | 调用类或结构 `T` 中 `P` 属性的 `get` 访问器。 如果 `P` 未 `static` 或 `P` 是只写的，则会发生编译时错误。 | 
|                   | `T.P = value`  | 类或结构 `T` 中 `P` 属性的 `set` 访问器是通过参数列表 `(value)`调用的。 如果 `P` 未 `static` 或 `P` 为只读，则会发生编译时错误。 | 
|                   | `e.P`          | `e` 类型指定的类、结构或接口中的属性 `P` 的 `get` 访问器在实例表达式 `e`中调用。 如果 `P` `static` 或 `P` 是只写的，则会发生绑定时错误。 | 
|                   | `e.P = value`  | `e` 类型给定的类、结构或接口中的属性 `P` 的 `set` 访问器是使用实例表达式 `e` 和参数列表 `(value)`调用的。 如果 `P` `static` 或 `P` 为只读，则会发生绑定时错误。 | 
| 事件访问      | `E += value`   | 调用包含类或结构中的事件 `E` 的 `add` 访问器。 如果 `E` 不是静态的，则 `this`实例表达式。 | 
|                   | `E -= value`   | 调用包含类或结构中的事件 `E` 的 `remove` 访问器。 如果 `E` 不是静态的，则 `this`实例表达式。 | 
|                   | `T.E += value` | 调用类或结构 `T` 中 `E` 事件的 `add` 访问器。 如果 `E` 不是静态的，则会发生绑定时错误。 | 
|                   | `T.E -= value` | 调用类或结构 `T` 中 `E` 事件的 `remove` 访问器。 如果 `E` 不是静态的，则会发生绑定时错误。 | 
|                   | `e.E += value` | `e` 类型给定的类、结构或接口中的事件 `E` 的 `add` 访问器在实例表达式 `e`中调用。 如果 `E` 是静态的，则会发生绑定时错误。 | 
|                   | `e.E -= value` | `e` 类型给定的类、结构或接口中的事件 `E` 的 `remove` 访问器在实例表达式 `e`中调用。 如果 `E` 是静态的，则会发生绑定时错误。 | 
| 索引器访问    | `e[x,y]`       | 重载决策适用于在由 e 类型给定的类、结构或接口中选择最佳索引器。 索引器的 `get` 访问器在实例表达式 `e` 和参数列表 `(x,y)`调用。 如果索引器是只写的，则会发生绑定时错误。 | 
|                   | `e[x,y] = value` | 重载决策适用于在类、结构或接口中选择由 `e`类型给定的最佳索引器。 索引器的 `set` 访问器在实例表达式 `e` 和参数列表 `(x,y,value)`调用。 如果索引器是只读的，则会发生绑定时错误。 | 
| 运算符调用 | `-x`         | 应用重载决策，以选择类或结构中由 `x`类型给定的最佳一元运算符。 用参数列表 `(x)`调用所选运算符。 | 
|                     | `x + y`      | 重载决策适用于在由 `x` 类型或 `y`类型给定的类或结构中选择最佳的二进制运算符。 用参数列表 `(x,y)`调用所选运算符。 | 
| 实例构造函数调用 | `new T(x,y)` | 重载决策适用于在类或结构 `T`中选择最佳实例构造函数。 实例构造函数用参数列表 `(x,y)`调用。 | 

### <a name="argument-lists"></a>参数列表

每个函数成员和委托调用都包含一个参数列表，该列表为函数成员的参数提供实际值或变量引用。 指定函数成员调用的参数列表的语法取决于函数成员类别：

*  对于实例构造函数、方法、索引器和委托，参数被指定为*argument_list*，如下所述。 对于索引器，调用 `set` 访问器时，自变量列表还包括指定为赋值运算符的右操作数的表达式。
*  对于属性，调用 `get` 访问器时，参数列表是空的，并且包含在调用 `set` 访问器时指定为赋值运算符的右操作数的表达式。
*  对于事件，参数列表包含指定为 `+=` 或 `-=` 运算符的右操作数的表达式。
*  对于用户定义的运算符，参数列表包含一元运算符的单个操作数或二元运算符的两个操作数。

属性（[属性](classes.md#properties)）、事件（[事件](classes.md#events)）和用户定义的运算符（[运算符](classes.md#operators)）的参数始终作为值参数传递（[值参数](classes.md#value-parameters)）。 索引器（[索引器](classes.md#indexers)）的参数始终作为值参数（[值参数](classes.md#value-parameters)）或参数数组（[参数数组](classes.md#parameter-arrays)）传递。 这些类别的函数成员不支持 Reference 和 output 参数。

实例构造函数、方法、索引器或委托调用的参数指定为*argument_list*：

```antlr
argument_list
    : argument (',' argument)*
    ;

argument
    : argument_name? argument_value
    ;

argument_name
    : identifier ':'
    ;

argument_value
    : expression
    | 'ref' variable_reference
    | 'out' variable_reference
    ;
```

*Argument_list*由一个或多个由逗号分隔的*参数*组成。 每个自变量都包含一个可选的*argument_name*后跟一个*argument_value*。 带有*argument_name*的*参数*称为***命名参数***，而没有*argument_name*的*参数*是***位置参数***。 在*argument_list*中的命名参数后，位置参数出现错误。

*Argument_value*可以采用以下形式之一：

*  一个*表达式*，指示参数作为值参数传递（[值参数](classes.md#value-parameters)）。
*  关键字 `ref` 后跟*variable_reference* （[变量引用](variables.md#variable-references)），指示参数作为引用参数（[引用参数](classes.md#reference-parameters)）传递。 在将变量作为引用参数传递之前，必须对其进行明确赋值（[明确赋值](variables.md#definite-assignment)）。 关键字 `out` 后跟*variable_reference* （[变量引用](variables.md#variable-references)），指示参数作为输出参数传递（[输出参数](classes.md#output-parameters)）。 在将变量作为 output 参数传递的函数成员调用之后，变量被视为明确赋值（[明确赋值](variables.md#definite-assignment)）。

#### <a name="corresponding-parameters"></a>对应的参数

对于参数列表中的每个参数，必须在要调用的函数成员或委托中包含相应的参数。

以下内容中使用的参数列表的确定方式如下：

*  对于在类中定义的虚拟方法和索引器，将从函数成员的最特定声明或重写中选取参数列表，从接收方的静态类型开始，然后搜索其基类。
*  对于接口方法和索引器，从接口类型和搜索基接口开始，选取参数列表作为成员的最明确定义。 如果找不到唯一的参数列表，则将构造一个具有不可访问的名称且不带可选参数的参数列表，以便调用不能使用命名参数或省略可选参数。
*  对于分部方法，使用定义分部方法声明的参数列表。
*  对于所有其他函数成员和委托，只有一个参数列表，该列表是使用的。

参数或参数的位置定义为参数列表或参数列表中其前面的参数或参数的数目。

按如下所示建立函数成员参数的对应参数：

*  实例构造函数、方法、索引器和委托的*argument_list*中的参数：
    * 一个位置参数，其中固定参数出现在参数列表中的同一位置，该参数对应于该参数。
    * 函数成员的位置自变量，其正常形式中调用的参数数组对应于参数数组，该参数数组必须出现在参数列表中的同一位置。
    * 函数成员的位置自变量，其中的参数数组以其展开形式调用，其中没有固定参数出现在参数列表中的同一位置，它对应于参数数组中的一个元素。
    * 命名参数对应于参数列表中具有相同名称的参数。
    * 对于索引器，调用 `set` 访问器时，指定为赋值运算符的右操作数的表达式对应于 `set` 访问器声明的隐式 `value` 参数。
*  对于属性，当调用 `get` 访问器时，没有参数。 调用 `set` 访问器时，指定为赋值运算符的右操作数的表达式对应于 `set` 访问器声明的隐式 `value` 参数。
*  对于用户定义的一元运算符（包括转换），单个操作数对应于运算符声明的单个参数。
*  对于用户定义的二进制运算符，左操作数对应于第一个参数，右侧操作数对应于运算符声明的第二个参数。

#### <a name="run-time-evaluation-of-argument-lists"></a>参数列表的运行时计算

在运行时处理函数成员调用期间（对[动态重载决策进行编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)），将按从左至右的顺序计算参数列表的表达式或变量引用，如下所示：

*  对于值参数，将计算参数表达式，并执行到相应参数类型的隐式转换（[隐式](conversions.md#implicit-conversions)转换）。 生成的值将成为函数成员调用中 value 参数的初始值。
*  对于引用参数或输出参数，将对变量引用进行计算，并将生成的存储位置成为函数成员调用中的参数所表示的存储位置。 如果作为引用或输出参数提供的变量引用是*reference_type*的数组元素，则执行运行时检查以确保数组的元素类型与参数的类型相同。 如果此检查失败，则会引发 `System.ArrayTypeMismatchException`。

方法、索引器和实例构造函数可以将其最右侧的参数声明为参数数组（[参数](classes.md#parameter-arrays)数组）。 此类函数成员的调用方式可以是普通形式，也可以是其展开形式，具体取决于适用的（[适用的函数成员](expressions.md#applicable-function-member)）：

*  当使用参数数组的函数成员以其正常形式调用时，为参数数组提供的参数必须是可隐[式转换为](conversions.md#implicit-conversions)参数数组类型的单个表达式。 在这种情况下，参数数组的作用与值参数完全相同。
*  当具有参数数组的函数成员以其展开形式调用时，该调用必须为参数数组指定零个或多个位置参数，其中每个参数都是可隐[式转换为](conversions.md#implicit-conversions)参数数组元素类型的表达式。 在这种情况下，调用会创建一个参数数组类型的实例，该实例的长度与参数的数量相对应，使用给定的参数值初始化数组实例的元素，并使用新创建的数组实例作为实际实际.

自变量列表的表达式始终按写入它们的顺序进行计算。 因此，示例
```csharp
class Test
{
    static void F(int x, int y = -1, int z = -2) {
        System.Console.WriteLine("x = {0}, y = {1}, z = {2}", x, y, z);
    }

    static void Main() {
        int i = 0;
        F(i++, i++, i++);
        F(z: i++, x: i++);
    }
}
```
生成输出
```console
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

如果存在从 `B` 到 `A`的隐式引用转换，数组协方差规则（[数组协方](arrays.md#array-covariance)差）允许 `A[]` 为数组类型 `B[]`的实例的引用。 由于这些规则，当*reference_type*的数组元素作为引用或输出参数传递时，需要运行时检查以确保数组的实际元素类型与参数的类型相同。 示例中
```csharp
class Test
{
    static void F(ref object x) {...}

    static void Main() {
        object[] a = new object[10];
        object[] b = new string[10];
        F(ref a[0]);        // Ok
        F(ref b[1]);        // ArrayTypeMismatchException
    }
}
```
`F` 的第二次调用导致引发 `System.ArrayTypeMismatchException`，因为 `b` 的实际元素类型为 `string` 而不是 `object`。

当具有参数数组的函数成员以其展开形式调用时，该调用的处理方式与使用数组初始值设定项（[数组创建表达式](expressions.md#array-creation-expressions)）的数组创建表达式在展开的参数周围插入的方式完全相同。 例如，给定声明
```csharp
void F(int x, int y, params object[] args);
```
对方法的展开形式的以下调用
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
完全对应于
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

特别要注意的是，如果参数数组提供的参数为零，则将创建一个空数组。

当使用相应的可选参数从函数成员中省略参数时，将隐式传递函数成员声明的默认参数。 因为这些始终是常量，所以其计算不会影响剩余参数的计算顺序。

### <a name="type-inference"></a>类型推理

如果在未指定类型实参的情况下调用泛型方法，则***类型推理***进程将尝试推断调用的类型参数。 类型推理的存在允许使用更方便的语法来调用泛型方法，并允许程序员避免指定冗余类型信息。 例如，给定方法声明：
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
无需显式指定类型参数即可调用 `Choose` 方法：
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

通过类型推理，`int` 和 `string` 的类型参数由方法的自变量确定。

类型推理作为方法调用（[方法调用](expressions.md#method-invocations)）的绑定时处理的一部分发生，并在调用的重载解析步骤之前发生。 如果在方法调用中指定了特定方法组，但没有将任何类型参数指定为方法调用的一部分，则会将类型推理应用于方法组中的每个泛型方法。 如果类型推理成功，则使用推断出的类型参数来确定参数的类型，以供后续重载决策使用。 如果重载决策选择泛型方法作为要调用的方法，则会将推断的类型参数用作调用的实际类型参数。 如果特定方法的类型推理失败，则该方法将不参与重载决策。 类型推理的失败本身不会导致绑定时错误。 但是，当重载决策失败并找不到任何适用的方法时，通常会导致绑定时错误。

如果所提供的参数数目不同于方法中的参数数量，则推理会立即失败。 否则，假定泛型方法具有以下签名：
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

使用窗体的方法调用 `M(E1...Em)` 类型推理的任务是查找 `S1...Sn` 的每个类型参数的唯一类型参数 `X1...Xn` 以便调用 `M<S1...Sn>(E1...Em)` 变为有效。

在推断过程中，每个类型参数 `Xi` 都*固定*到特定类型 `Si` 或未*使用关联*的*界限*集进行固定。 每个边界都是 `T`的一种类型。 最初，每个类型变量 `Xi` 都不包含一组空的界限。

类型推理分阶段发生。 每个阶段都将尝试根据上一阶段的发现来推导更多类型变量的类型参数。 第一阶段会对边界进行一些初始推断，而第二阶段则将类型变量修复为特定类型，并推导出进一步的边界。 第二阶段可能需要重复一次。

*注意：* 仅当调用泛型方法时，才会发生类型推理。 用于转换方法组的类型推理中介绍了用于转换[方法](expressions.md#type-inference-for-conversion-of-method-groups)组的类型推理和查找一组表达式的最佳通用[类型中介绍的方法。](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)

#### <a name="the-first-phase"></a>第一阶段

对于每个方法参数 `Ei`：

*   如果 `Ei` 是匿名函数，则*显式参数类型推理*（[显式参数类型推断](expressions.md#explicit-parameter-type-inferences)）是从 `Ei` 到 `Ti`
*   否则，如果 `Ei` 具有类型 `U` 并且 `xi` 是值参数，则将*从*`U`*到*`Ti`进行*下限推理*。
*   否则，如果 `Ei` 具有类型 `U` 并且 `xi` 是 `ref` 或 `out` 参数，则将*从*`U`*到*`Ti`进行*精确推断*。
*   否则，不会对此参数进行推理。


#### <a name="the-second-phase"></a>第二阶段

第二个阶段按如下方式进行：

*   不*依赖于*（[依赖](expressions.md#dependence)）任何 `Xj`*的所有未固定的*类型 `Xi` 变量都是固定的（正在[修复](expressions.md#fixing)）。
*   如果不存在这样的类型变量，则所有*未固定的类型变量*`Xi` 都是*固定*的，其中所有的保留都是：
    *   至少有一个类型变量 `Xj` 依赖于 `Xi`
    *   `Xi` 具有非空的界限集
*   如果不存在这样的类型变量，并且仍有未*固定*的类型变量，则类型推理将失败。
*   否则，如果不存在进一步的未*固定*类型变量，则类型推理将成功。
*   否则，对于具有相应参数类型的所有参数 `Ei` `Ti` 其中*输出类型*（[输出类型](expressions.md#output-types)）包含未*固定*的类型变量 `Xj` 但*输入类型*（[输入类型](expressions.md#input-types)）不存在，则将*从*`Ei`[Output type inferences](expressions.md#output-type-inferences)*到*`Ti`进行*输出类型推理*。 然后，重复第二阶段。

#### <a name="input-types"></a>输入类型

如果 `E` 是方法组或隐式类型的匿名函数，并且 `T` 为委托类型或表达式树类型，则 `T` 的所有参数类型均为*类型*为 `T`的 `E` 的*输入类型*。

####  <a name="output-types"></a>输出类型

如果 `E` 为方法组或匿名函数，并且 `T` 为委托类型或表达式树类型，则 `T` 的返回类型为*类型*为 `T`的 `E` 的*输出类型*。

#### <a name="dependence"></a>依赖关系

未*固定的类型*变量 `Xi`*直接依赖于*未固定的类型变量 `Xj` 如果对于具有类型 `Xj` 的某些参数 `Tk` `Ek`，`Ek`*在类型为*`Tk` 的输出类型为 `Xi` 的*输出类型*中发生。

如果 `Xj`*直接依赖于*`Xi` 或者 `Xi`*直接*依赖于 `Xk` 并且 `Xk`*依赖于*`Xj`，则 `Xj`*依赖于*`Xi`。 因此 "依赖于" 是传递的，而不是 "直接依赖于" 的反身闭包。

#### <a name="output-type-inferences"></a>输出类型推断

*输出类型推断*是通过以下方式*从*`E`*到*类型 `T` 的表达式进行的：

*  如果 `E` 是 `U` （[推断返回类型](expressions.md#inferred-return-type)）且 `T` 为委托类型或具有返回类型 `Tb`的表达式树类型的匿名函数，则将*从*`U`[Lower-bound inferences](expressions.md#lower-bound-inferences)*到*`Tb`进行*下限推理*。
*  否则，如果 `E` 为方法组，`T` 是参数类型为 `T1...Tk` 和返回类型 `Tb`的委托类型或表达式树类型，并且 `E` 生成一个返回类型 `T1...Tk` 的单个方法，则*从*`U`*到*`U`*生成一个方法*。
*  否则，如果 `E` 是 `U`类型的表达式，则将*从*`U`*到*`T`进行*下限推理*。
*  否则，不进行推断。

#### <a name="explicit-parameter-type-inferences"></a>显式参数类型推断

*显式参数类型推理*是通过以下方式*从*`E`*到*类型 `T` 的表达式进行的：

*  如果 `E` *为* 的参数类型`U1...Uk`为-1 的显式类型匿名函数，`T` 是具有参数类型 `V1...Vk` 的委托类型或表达式树类型，则对每个 `Ui` 进行精确推理（[精确推断](expressions.md#exact-inferences)）从到对应的 0。`Ui``Vi`

#### <a name="exact-inferences"></a>精确推断

*从*类型 `U`*到*类型 `V` 的*精确推理*如下所示：

*  如果 `V` 是未*固定*的 `Xi` 之一，则 `U` 添加到 `Xi`的完全限定集。

*  否则，将通过检查是否存在以下任何情况来确定 `V1...Vk` 和 `U1...Uk`：

   *  `V` 是 `V1[...]` 数组类型，`U` 是同一排名 `U1[...]` 的数组类型
   *  `V` 是 `V1?` 类型，`U` 为类型 `U1?`
   *  `V` 是构造类型 `C<V1...Vk>`并且 `U` 为构造类型 `C<U1...Uk>`

   如果上述任何情况都适用，则将*从*每个 `Ui`*到*相应的 `Vi`进行*确切的推理*。

*  否则，不进行推断。

#### <a name="lower-bound-inferences"></a>下限推理

*从*类型 `U`*到*类型 `V` 的*下限推理*如下所示：

*  如果 `V` 是未*固定*的 `Xi` 之一，则 `U` 添加到 `Xi`的下限集。
*  否则，如果 `V` 是类型 `V1?`并且 `U` 为类型 `U1?` 则从 `U1` 到 `V1`进行下限推理。
*  否则，将通过检查是否存在以下任何情况来确定 `U1...Uk` 和 `V1...Vk`：
   *  `V` 是 `V1[...]` 数组类型，`U` 是同一排名 `U1[...]` （或其有效基类型为 `U1[...]`的类型参数）的数组类型
   *  `V` 是 `IEnumerable<V1>`、`ICollection<V1>` 或 `IList<V1>` 中的一种，`U` 是一维数组类型 `U1[]`（或其有效基类型为 `U1[]`的类型参数）
   *  `V` 是 `C<V1...Vk>` 构造的类、结构、接口或委托类型，并且有一个唯一类型 `C<U1...Uk>` 以便 `U` （或者，如果 `U` 是一个类型参数，其有效的基类或其有效接口集的任何成员）等同于、从（直接或间接） `C<U1...Uk>`继承。

      （"唯一性" 限制意味着在 case 接口 `C<T> {} class U: C<X>, C<Y> {}`中，从 `U` 推断到 `C<T>` 时不进行推理，因为 `U1` 可以 `X` 或 `Y`。）

   如果这些情况中的任何一种都适用，则将*从*每*个 `Ui` 推导为相应*的 `Vi`，如下所示：

   *  如果 `Ui` 已知为引用类型，则进行*精确推理*
   *  否则，如果 `U` 是数组类型，则进行*下限推理*
   *  否则，如果 `C<V1...Vk>` `V`，则推理依赖于 `C`的第 i 个类型参数：
      *  如果它是协变的，则进行*下限推理*。
      *  如果它是逆变的，则进行*上限推断*。
      *  如果它是固定的，则进行*精确推理*。
*  否则，不进行推断。

#### <a name="upper-bound-inferences"></a>上限推理

*从*类型 `U`*到*类型 `V` 的*上限推理*如下所示：

*  如果 `V` 是未*固定*的 `Xi` 之一，则 `U` 添加到 `Xi`的上限。
*  否则，将通过检查是否存在以下任何情况来确定 `V1...Vk` 和 `U1...Uk`：
   *  `U` 是 `U1[...]` 数组类型，`V` 是同一排名 `V1[...]` 的数组类型
   *  `U` 是 `IEnumerable<Ue>`、`ICollection<Ue>` 或 `IList<Ue>`，`V` 是一维数组类型 `Ve[]`
   *  `U` 是 `U1?` 类型，`V` 为类型 `V1?`
   *  `U` 是构造的类、结构、接口或委托类型 `C<U1...Uk>` 并且 `V` 是一个与相同的类、结构、接口或委托类型，继承自（直接或间接），或实现（直接或间接）唯一类型 `C<V1...Vk>`

      （"唯一性" 限制意味着如果有 `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`，则从 `C<U1>` 推断到 `V<Q>`时不进行推理。 不从 `U1` 到 `X<Q>` 或 `Y<Q>`进行推断。）

   如果这些情况中的任何一种都适用，则将*从*每*个 `Ui` 推导为相应*的 `Vi`，如下所示：
   *  如果 `Ui` 已知为引用类型，则进行*精确推理*
   *  否则，如果 `V` 是数组类型，则进行*上限推理*
   *  否则，如果 `C<U1...Uk>` `U`，则推理依赖于 `C`的第 i 个类型参数：
      *  如果它是协变的，则进行*上限推理*。
      *  如果它是逆变的，则进行*下限推理*。
      *  如果它是固定的，则进行*精确推理*。
*  否则，不进行推断。   

#### <a name="fixing"></a>更正

使用一组界限 `Xi` 未*固定*的类型变量是*固定*的，如下所示：

*  *候选类型*集 `Uj` 以 `Xi`的边界集中的所有类型的集合开头。
*  然后，我们依次检查每个绑定 for `Xi`：对于每个完全绑定 `U`，`Xi` `Uj` 的所有类型，这些类型与从候选集删除 `U` 不完全相同。 对于 `Xi` 的每个下限 `U`，从候选集中将*不*会从 `U` 中删除任何 `Uj` 类型的隐式转换。 对于 `Xi` 的每个上限 *`U`，`Uj`* 将从候选集删除没有隐式转换到 `U` 的所有类型。
*  如果其余的候选类型 `Uj` 有一个唯一的类型 `V` 从该类型中有一个到所有其他候选类型的隐式转换，则 `Xi` 固定到 `V`。
*  否则，类型推理将失败。

#### <a name="inferred-return-type"></a>推断出的返回类型

推断出的匿名函数的返回类型 `F` 在类型推理和重载决策过程中使用。 只能为所有参数类型已知的匿名函数确定推理出的返回类型，因为这些参数类型是显式给定的，它是通过匿名函数转换提供的，或者是在对封闭泛型的类型推理期间推断的方法调用。

按如下方式确定***推断结果类型***：

*  如果 `F` 的主体是具有类型的*表达式*，则推断出的结果类型 `F` 是该表达式的类型。
*  如果 `F` 的主体是一个*块*，块的 `return` 语句中的表达式集有一个最常见的类型 `T` （[查找一组表达式的最佳通用类型](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)），则将 `T`推理出的结果 `F` 类型。
*  否则，无法推断 `F`的结果类型。

按如下方式确定***推断出的返回类型***：

*  如果 `F` 是异步的，并且 `F` 的主体是分类为 nothing （[表达式分类](expressions.md#expression-classifications)）的表达式，或者是没有 return 语句的语句块包含表达式，则推断出的返回类型为 `System.Threading.Tasks.Task`
*  如果 `F` 为 async 并且 `T`推理结果类型，则推断出的返回类型为 `System.Threading.Tasks.Task<T>`。
*  如果 `F` 不是异步的，并且 `T`推断结果类型，则推断出的返回类型为 `T`。
*  否则，将无法为 `F`推断返回类型。

作为涉及匿名函数的类型推理的示例，请考虑在 `System.Linq.Enumerable` 类中声明 `Select` 扩展方法：
```csharp
namespace System.Linq
{
    public static class Enumerable
    {
        public static IEnumerable<TResult> Select<TSource,TResult>(
            this IEnumerable<TSource> source,
            Func<TSource,TResult> selector)
        {
            foreach (TSource element in source) yield return selector(element);
        }
    }
}
```

假定 `System.Linq` 命名空间是使用 `using` 子句导入的，并且给定的类 `Customer` 具有类型 `string`的 `Name` 属性，则可以使用 `Select` 方法选择客户列表的名称：
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

通过将调用重写为静态方法调用来处理 `Select` 的扩展方法调用（[扩展方法](expressions.md#extension-method-invocations)调用）：
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

由于未显式指定类型参数，因此将使用类型推理来推断类型参数。 首先，`customers` 自变量与 `source` 参数相关，推理 `T` 为 `Customer`。 然后，使用上面所述的匿名函数类型推理过程，`c` 给定类型 `Customer`，并且表达式 `c.Name` 与 `selector` 参数的返回类型相关，并推断 `S` 为 `string`。 因此，调用等效于
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
结果的类型为 `IEnumerable<string>`。

下面的示例演示匿名函数类型推理如何允许类型信息在泛型方法调用中的参数之间 "流"。 给定方法：
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

调用的类型推理：
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
按如下所示进行操作：首先，参数 `"1:15:30"` 与 `value` 参数相关，推理 `X` 为 `string`。 然后，将 `s`第一个匿名函数的参数 `string`，并将表达式 `TimeSpan.Parse(s)` 与 `f1`的返回类型相关联，推断 `Y` 为 `System.TimeSpan`。 最后，为第二个匿名函数 `t`的参数赋予 `System.TimeSpan`推断出的类型，并且表达式 `t.TotalSeconds` 与 `f2`的返回类型相关，推断 `Z` 为 `double`。 因此，调用的结果为 `double`类型。

#### <a name="type-inference-for-conversion-of-method-groups"></a>用于转换方法组的类型推理

与泛型方法的调用类似，当包含泛型方法的方法组 `M` 转换为给定委托类型 `D` （[方法组转换](conversions.md#method-group-conversions)）时，也必须应用类型推理。 给定方法
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
并且方法组 `M` 分配给委托类型 `D` 推断类型的任务是查找类型参数 `S1...Sn` 以便表达式：
```csharp
M<S1...Sn>
```
与 `D`成为兼容的（[委托声明](delegates.md#delegate-declarations)）。

与泛型方法调用的类型推理算法不同，在本例中，只有参数*类型*，没有参数*表达式*。 具体而言，不存在任何匿名函数，因此不需要进行多个推理阶段。

相反，所有 `Xi` 都被视为未*固定*的，并且从 `D` 的每个参数类型*到*`M`的相应参数 `Tj` 类型，将*从*每个参数类型 `Uj` 进行*下限推理*。 如果没有找到任何 `Xi` 的边界，则类型推理将失败。 否则，所有 `Xi` 都*固定*到相应的 `Si`，这是类型推理的结果。

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a>查找一组表达式的最佳通用类型

在某些情况下，需要为一组表达式推断通用类型。 特别是，隐式类型化数组的元素类型和带*块*体的匿名函数的返回类型是通过这种方式找到的。

直观地说，给定一组表达式 `E1...Em` 此推理应等效于调用方法
```csharp
Tr M<X>(X x1 ... X xm)
```
作为参数的 `Ei`。

更准确地说，推理最初使用未*固定*的类型变量 `X`。 然后*从*每个 `Ei`*到*`X`进行*输出类型推断*。 最后，`X` 是*固定*的，如果成功，则结果类型 `S` 是生成的表达式的最佳通用类型。 如果不存在这样的 `S`，则表达式没有最常见的类型。

### <a name="overload-resolution"></a>重载决策

重载决策是用于选择在给定参数列表和一组候选函数成员时调用的最佳函数成员的绑定时机制。 重载决策选择要在中C#的下列不同上下文中调用的函数成员：

*  调用*invocation_expression*中名为的方法（[方法调用](expressions.md#method-invocations)）。
*  调用*object_creation_expression*中名为的实例构造函数（[对象创建表达式](expressions.md#object-creation-expressions)）。
*  通过*element_access* （[元素访问](expressions.md#element-access)）调用索引器访问器。
*  表达式中引用的预定义运算符或用户定义的运算符的调用（[一元运算符重载决策](expressions.md#unary-operator-overload-resolution)和[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）。

其中每个上下文都定义候选函数成员的集合和参数列表，其独特方式为，如以上所列部分中所述。 例如，方法调用的候选项不包括标记为 `override` （[成员查找](expressions.md#member-lookup)）的方法，并且派生类中的任何方法都适用时，基类中的方法不是候选项（[方法调用](expressions.md#method-invocations)）。

确定候选函数成员和参数列表后，在所有情况下，最佳函数成员的选择是相同的：

*  给定一组适用的候选函数成员后，该集合中的最佳函数成员就是。 如果集只包含一个函数成员，则该函数成员是最佳函数成员。 否则，最佳函数成员就是比给定自变量列表更好的所有其他函数成员的一个函数成员，前提是将每个函数成员与在[更好的函数成员](expressions.md#better-function-member)中使用规则的所有其他函数成员进行比较。 如果不是正好有一个比所有其他函数成员更好的函数成员，则函数成员调用是不明确的，并发生绑定时错误。

以下部分定义了术语***适用的函数成员***和***更好的函数成员***的精确含义。

#### <a name="applicable-function-member"></a>适用的函数成员

当满足以下所有条件时，函数成员被称为适用于自变量列表的***函数成员***`A`：

*  `A` 中的每个参数都对应于函数成员声明中的一个参数（如[相应的参数](expressions.md#corresponding-parameters)中所述），并且没有参数对应的任何参数都是可选参数。
*  对于 `A`中的每个自变量，参数的参数传递模式（即值、`ref`或 `out`）与相应参数的参数传递模式相同，并且
   *  对于值参数或参数数组，存在从参数到相应参数类型的隐式转换（[隐式转换](conversions.md#implicit-conversions)）或
   *  对于 `ref` 或 `out` 参数，参数的类型与对应参数的类型相同。 毕竟，`ref` 或 `out` 参数是传递的参数的别名。

对于包含参数数组的函数成员，如果函数成员适用于上述规则，则称它适用于其***正常形式***。 如果包含参数数组的函数成员在其正常形式下不适用，则函数成员可以采用其***展开形式***：

*  通过使用参数数组元素类型的零个或多个值参数替换函数成员声明中的参数数组，使参数列表中的参数数目与参数的总数 `A` 匹配，来构造展开后的形式。 如果 `A` 的参数少于函数成员声明中的固定参数数目，则无法构造函数成员的展开形式，因此不适用。
*  否则，如果为中的每个参数 `A` 参数传递模式，则展开形式适用，参数的参数传递模式与相应参数的参数传递模式相同，并且
   *  对于固定值参数或扩展创建的值参数，存在从自变量的类型到相应参数类型的隐式转换（[隐式转换](conversions.md#implicit-conversions)）或
   *  对于 `ref` 或 `out` 参数，参数的类型与对应参数的类型相同。

#### <a name="better-function-member"></a>更好的函数成员

出于确定更好的函数成员的目的，一个被去除的自变量列表 A 构造，它以其在原始参数列表中出现的顺序仅包含自变量表达式本身。

按以下方式构造每个候选函数成员的参数列表：

*  如果函数成员仅适用于扩展窗体，则使用展开的窗体。
*  将从参数列表中删除不具有相应参数的可选参数
*  参数会进行重新排序，以便它们在与参数列表中的相应参数相同的位置上发生。

给定一个参数列表 `A`，其中包含一组参数表达式 `{E1, E2, ..., En}` 和两个适用的函数成员 `Mp`，并 `Mq` 参数类型 `{P1, P2, ..., Pn}` 和 `{Q1, Q2, ..., Qn}`，则 `Mp` 定义为比 `Mq`***更好的函数成员***

*  对于每个参数，从 `Ex` 到 `Qx` 的隐式转换并不优于从 `Ex` 到 `Px`的隐式转换和
*  对于至少一个参数，从 `Ex` 到 `Px` 的转换优于从 `Ex` 到 `Qx`的转换。

当执行此计算时，如果 `Mp` 或 `Mq` 适用于其展开形式，则 `Px` 或 `Qx` 引用参数列表的展开形式的参数。

如果参数类型序列 `{P1, P2, ..., Pn}` 和 `{Q1, Q2, ..., Qn}` 等效（即每个 `Pi` 的标识转换为相应的 `Qi`），则将按顺序应用以下附加中断规则，以确定更好的函数成员。

*  如果 `Mp` 为非泛型方法并且 `Mq` 为泛型方法，则 `Mp` 比 `Mq`更好。
*  否则，如果 `Mp` 适用于其正常窗体并且 `Mq` 具有 `params` 数组，并且仅适用于其展开形式，则 `Mp` 比 `Mq`更好。
*  否则，如果 `Mp` 具有比 `Mq`更多的声明参数，则 `Mp` 比 `Mq`更好。 如果两种方法都有 `params` 数组，并且仅适用于其展开的形式，则会发生这种情况。
*  否则，如果 `Mp` 的所有参数都有相应的参数，而默认参数需要替换为 `Mq` 中的至少一个可选参数，则 `Mp` 比 `Mq`更好。
*  否则，如果 `Mp` 具有比 `Mq`更具体的参数类型，则 `Mp` 比 `Mq`更好。 让 `{R1, R2, ..., Rn}` 和 `{S1, S2, ..., Sn}` 表示 `Mp` 和 `Mq`的未实例化和未扩展的参数类型。 `Mp`的参数类型比 `Mq`的更具体，因为对于每个参数，`Rx` 不太具体于 `Sx`，并且对于至少一个参数，`Rx` 比 `Sx`更为具体：
   *  类型参数不如非类型参数的特定类型。
   *  如果至少有一个类型自变量是更具体的类型，并且没有类型参数的特定于其他中的相应类型参数，则递归方式比另一个构造类型（具有相同数量的类型参数）更为具体。
   *  如果第一个数组的元素类型比第二个元素的类型更具体，则数组类型比另一个数组类型更具体（具有相同维数的数组）。
*  否则，如果一个成员是非提升运算符，而另一个成员是提升运算符，则不提升的运算符更好。
*  否则，两个函数成员都不会更好。

#### <a name="better-conversion-from-expression"></a>表达式的更好转换

给定一个隐式转换 `C1`，该转换将 `E` 类型的表达式转换为类型 `T1`，并从表达式 `E` 转换为类型 `T2`的隐式转换 `C2`，`C1` 是比 `C2`***更好的转换***（如果 `E` 并不完全匹配 `T2` 和至少一个保留项）：

* `E` 完全匹配 `T1` （[完全匹配的表达式](expressions.md#exactly-matching-expression)）
* `T1` 是比 `T2` 更好的转换目标（[更好的转换目标](expressions.md#better-conversion-target)）

#### <a name="exactly-matching-expression"></a>完全匹配的表达式

给定表达式 `E` 和类型 `T`，如果下列其中一项保留，`E` 与 `T` 完全匹配：

*  `E` 具有类型 `S`，并且存在从 `S` 到 `T` 的标识转换
*  `E` 是一种匿名函数，`T` 为委托类型 `D` 或表达式目录树类型 `Expression<D>` 和以下保留之一：
   *  `E` 在 `D` 参数列表的上下文中存在的推断返回类型 `X` （[推断返回类型](expressions.md#inferred-return-type)），并且存在从 `X` 到返回类型的标识转换 `D`
   *  `E` 为非异步的，并且 `D` 具有返回类型 `Y` 或者 `E` 为 async 并且 `D` 具有返回类型 `Task<Y>`，以及以下保留项之一：
      * `E` 的主体是一个完全匹配的表达式 `Y`
      * `E` 的主体是一个语句块，其中每个 return 语句返回一个完全匹配的表达式 `Y`

#### <a name="better-conversion-target"></a>更好的转换目标

如果 `T1` 和 `T2`两种不同的类型，`T1` 是比 `T2` 更好的转换目标，前提是不存在从 `T2` 到 `T1` 的隐式转换，并且至少包含以下其中一项：

*  从 `T1` 到 `T2` 存在的隐式转换
*  `T1` 为委托类型 `D1` 或表达式树类型 `Expression<D1>`，`T2` 为委托类型 `D2` 或表达式树类型 `Expression<D2>`，`D1` 具有返回类型 `S1` 和以下保留之一：
   * `D2` 返回 void
   * `D2` 具有 `S2`的返回类型，`S1` 是比 `S2` 更好的转换目标
*  `T1` `Task<S1>`，`T2` `Task<S2>`，`S1` 是比 `S2` 更好的转换目标
*  `T1` `S1` 或 `S1?`，其中 `S1` 是有符号整数类型，`T2` `S2` 或 `S2?`，其中 `S2` 是无符号整数类型。 具体而言：
   * `S1` 是 `sbyte` 的，`S2` 为 `byte`、`ushort`、`uint`或 `ulong`
   * `S1` 是 `short` 并且 `S2` 为 `ushort`、`uint`或 `ulong`
   * `S1` 是 `int` 并且 `S2` 为 `uint`或 `ulong`
   * `S1` 是 `long` 并且 `S2` 为 `ulong`

#### <a name="overloading-in-generic-classes"></a>泛型类中的重载

尽管声明的签名必须是唯一的，但类型参数的替换可能导致相同的签名。 在这种情况下，以上重载解决方法的分解规则将选取最特定的成员。

下面的示例显示了根据此规则有效和无效的重载：

```csharp
interface I1<T> {...}

interface I2<T> {...}

class G1<U>
{
    int F1(U u);                  // Overload resolution for G<int>.F1
    int F1(int i);                // will pick non-generic

    void F2(I1<U> a);             // Valid overload
    void F2(I2<U> a);
}

class G2<U,V>
{
    void F3(U u, V v);            // Valid, but overload resolution for
    void F3(V v, U u);            // G2<int,int>.F3 will fail

    void F4(U u, I1<V> v);        // Valid, but overload resolution for    
    void F4(I1<V> v, U u);        // G2<I1<int>,int>.F4 will fail

    void F5(U u1, I1<V> v2);      // Valid overload
    void F5(V v1, U u2);

    void F6(ref U u);             // valid overload
    void F6(out V v);
}
```

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a>动态重载解析的编译时检查

对于大多数动态绑定操作，解析的可能候选项集在编译时是未知的。 但在某些情况下，在编译时，候选集是已知的：

*  具有动态参数的静态方法调用
*  接收方不是动态表达式的实例方法调用
*  接收方不是动态表达式的索引器调用
*  具有动态参数的构造函数调用

在这些情况下，将对每个候选项执行有限的编译时检查，以查看是否有可能在运行时应用它们。此检查包括以下步骤：

*  分部类型推理：使用[类型推理](expressions.md#type-inference)的规则推断不直接或间接依赖于类型 `dynamic` 的参数的任何类型参数。 其余类型参数是未知的。
*  部分适用性检查：根据[适用的函数成员](expressions.md#applicable-function-member)检查适用性，但忽略类型未知的参数。
*  如果没有候选项通过此测试，则会发生编译时错误。

### <a name="function-member-invocation"></a>函数成员调用

本节介绍在运行时调用特定函数成员的过程。 假定绑定时间进程已确定要调用的特定成员（可能通过将重载决策应用于一组候选函数成员）。

出于描述调用过程的目的，函数成员分为两类：

*  静态函数成员。 这些是实例构造函数、静态方法、静态属性访问器和用户定义的运算符。 静态函数成员始终是非虚拟的。
*  实例函数成员。 这些是实例方法、实例属性访问器和索引器访问器。 实例函数成员既不是虚拟的也不是虚拟的，始终在特定的实例上调用。 实例由实例表达式计算，它在函数成员内可作为 `this` （[此访问](expressions.md#this-access)）进行访问。

函数成员调用的运行时处理包括以下步骤，其中 `M` 是函数成员，如果 `M` 为实例成员，`E` 为实例表达式：

*  如果 `M` 是静态函数成员：
   * 参数列表按[参数](expressions.md#argument-lists)列表中所述进行计算。
   * 调用 `M`。

*  如果 `M` 是在*value_type*中声明的实例函数成员：
   * 计算 `E`。 如果此计算导致异常，则不执行其他步骤。
   * 如果 `E` 未分类为变量，则创建 `E`类型的临时本地变量，并将 `E` 的值分配给该变量。 然后，将 `E` 重新分类为对该临时本地变量的引用。 临时变量在 `M`内作为 `this` 访问，但不能以其他方式访问。 因此，只有在 `E` 为真正的变量时，调用方才能观察 `M` 对 `this`所做的更改。
   * 参数列表按[参数](expressions.md#argument-lists)列表中所述进行计算。
   * 调用 `M`。 `E` 引用的变量成为 `this`引用的变量。

*  如果 `M` 是在*reference_type*中声明的实例函数成员：
   * 计算 `E`。 如果此计算导致异常，则不执行其他步骤。
   * 参数列表按[参数](expressions.md#argument-lists)列表中所述进行计算。
   * 如果 `E` 的类型为*value_type*，则执行装箱转换（[装箱转换](types.md#boxing-conversions)）以将 `E` 转换为类型 `object`，并在以下步骤中将 `E` 视为类型 `object`。 在这种情况下，`M` 只能是 `System.Object`的成员。
   * 检查 `E` 的值是否有效。 如果 `null``E` 的值，则将引发 `System.NullReferenceException`，并且不执行任何其他步骤。
   * 确定要调用的函数成员实现：
     * 如果 `E` 的绑定时类型是接口，则调用的函数成员是由 `E`引用的实例的运行时类型提供的 `M` 的实现。 此函数成员是通过以下方式确定的：应用接口映射规则（[接口映射](interfaces.md#interface-mapping)），以确定 `E`所引用的实例的运行时类型提供的 `M` 实现。
     * 否则，如果 `M` 为虚拟函数成员，则调用的函数成员是由 `E`引用的实例的运行时类型提供的 `M` 的实现。 此函数成员是通过应用规则来确定的，这些规则用于确定 `E`所引用的实例的运行时类型 `M` 的派生程度最高的实现（[虚拟方法](classes.md#virtual-methods)）。
     * 否则，`M` 为非虚拟函数成员，调用的函数成员 `M` 本身。
   * 调用上述步骤中确定的函数成员实现。 `E` 引用的对象成为 `this`引用的对象。

#### <a name="invocations-on-boxed-instances"></a>在装箱实例上调用

在*value_type*中实现的函数成员可在以下情况下通过*value_type*的装箱实例进行调用：

*  当函数成员是从类型 `object` 继承的方法的 `override` 时，将通过类型 `object`的实例表达式调用。
*  当函数成员是接口函数成员的实现并通过*interface_type*的实例表达式调用时。
*  函数成员通过委托调用时。

在这些情况下，已装箱的实例被视为包含*value_type*的变量，此变量将成为函数成员调用中 `this` 所引用的变量。 具体而言，这意味着当对装箱实例调用函数成员时，该函数成员可以修改装箱实例中包含的值。

## <a name="primary-expressions"></a>主要表达式

主要表达式包括表达式的最简单形式。

```antlr
primary_expression
    : primary_no_array_creation_expression
    | array_creation_expression
    ;

primary_no_array_creation_expression
    : literal
    | interpolated_string_expression
    | simple_name
    | parenthesized_expression
    | member_access
    | invocation_expression
    | element_access
    | this_access
    | base_access
    | post_increment_expression
    | post_decrement_expression
    | object_creation_expression
    | delegate_creation_expression
    | anonymous_object_creation_expression
    | typeof_expression
    | checked_expression
    | unchecked_expression
    | default_value_expression
    | nameof_expression
    | anonymous_method_expression
    | primary_no_array_creation_expression_unsafe
    ;
```

主表达式划分*array_creation_expression*s 和*primary_no_array_creation_expression*之间。 以这种方式对待数组创建表达式，而不是将其与其他简单的表达式窗体一起列出，而是允许语法禁止可能的代码混乱，如
```csharp
object o = new int[3][1];
```
否则，会将其解释为
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a>文本

由*文本*（[文本](lexical-structure.md#literals)）组成的*primary_expression*分类为值。


### <a name="interpolated-strings"></a>内插字符串

*Interpolated_string_expression*包含一个 `$` 符号后跟一个常规或逐字字符串文本，其中的洞由 `{` 和 `}`分隔，其中包含表达式和格式规范。 内插字符串表达式是已分解为单独标记的*interpolated_string_literal*的结果，如内[插字符串文本](lexical-structure.md#interpolated-string-literals)中所述。

```antlr
interpolated_string_expression
    : '$' interpolated_regular_string
    | '$' interpolated_verbatim_string
    ;

interpolated_regular_string
    : interpolated_regular_string_whole
    | interpolated_regular_string_start interpolated_regular_string_body interpolated_regular_string_end
    ;

interpolated_regular_string_body
    : interpolation (interpolated_regular_string_mid interpolation)*
    ;

interpolation
    : expression
    | expression ',' constant_expression
    ;

interpolated_verbatim_string
    : interpolated_verbatim_string_whole
    | interpolated_verbatim_string_start interpolated_verbatim_string_body interpolated_verbatim_string_end
    ;

interpolated_verbatim_string_body
    : interpolation (interpolated_verbatim_string_mid interpolation)+
    ;
```

内插中的*constant_expression*必须具有到 `int`的隐式转换。

*Interpolated_string_expression*分类为值。 如果它立即转换为 `System.IFormattable` 或使用隐式内插字符串转换（[隐式内插](conversions.md#implicit-interpolated-string-conversions)的字符串转换） `System.FormattableString`，则内插字符串表达式具有该类型。 否则，它的类型为 `string`。

如果内插字符串的类型是 `System.IFormattable` 或 `System.FormattableString`，则表示对 `System.Runtime.CompilerServices.FormattableStringFactory.Create`的调用。 如果类型 `string`，则表达式的含义是对 `string.Format`的调用。 在这两种情况下，调用的参数列表都包含格式字符串文本和每个内插的占位符，以及与占位符相对应的每个表达式的参数。

格式字符串文字按如下方式构造，其中 `N` 是*interpolated_string_expression*中的内插数：

*  如果*interpolated_regular_string_whole*或*interpolated_verbatim_string_whole*后跟 `$` 符号，则格式字符串文本就是该标记。
*  否则，格式字符串文本包含： 
   *  首先*interpolated_regular_string_start*或*interpolated_verbatim_string_start*
   *  然后，将每个数字从 `0` `I` 到 `N-1`： 
      * `I` 的十进制表示形式
      * 然后，如果相应的*内插*具有*constant_expression*，则 `,` （逗号），后跟的值的十进制表示形式*constant_expression*
      * 然后， *interpolated_regular_string_mid*、 *interpolated_regular_string_end*、 *interpolated_verbatim_string_mid*或*interpolated_verbatim_string_end*后跟相应的内插。

后续参数只是*内插*中的*表达式*（如果有），按顺序排列。

TODO：示例。


### <a name="simple-names"></a>简单名称

*Simple_name*包含标识符，可选择后跟类型参数列表：

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

*Simple_name*格式为 `I` 或格式为 `I<A1,...,Ak>`，其中 `I` 是单个标识符，`<A1,...,Ak>` 是可选*type_argument_list*。 当未指定*type_argument_list*时，请考虑 `K` 为零。 将按如下所示对*simple_name*进行评估和分类：

*  如果 `K` 为零且*simple_name*出现在*块*中，并且*块*的（或封闭*块*的）局部变量声明空间（[声明](basic-concepts.md#declarations)）包含名称 `I`的局部变量、参数或常量，则*simple_name*引用该局部变量、参数或常量，并归类为变量或值。
*  如果 `K` 为零，且*simple_name*出现在泛型方法声明的主体中，并且该声明包含名称为 `I`的类型参数，则*simple_name*引用该类型参数。
*  否则，对于每个实例类型 `T` （[实例类型](classes.md#the-instance-type)），从立即封闭类型声明的实例类型开始，并继续执行每个封闭类或结构声明的实例类型（如果有）：
   *  如果 `K` 为零，且 `T` 的声明包含名称为 `I`的类型参数，则*simple_name*将引用该类型参数。
   *  否则，如果 `T` 中的 `I` 的成员查找（[成员查找](expressions.md#member-lookup)） `K` 类型参数生成匹配：
      * 如果 `T` 是直接封闭类或结构类型的实例类型，并且查找标识了一个或多个方法，则结果为具有 `this`的关联实例表达式的方法组。 如果指定了类型参数列表，它将用于调用泛型方法（[方法调用](expressions.md#method-invocations)）。
      * 否则，如果 `T` 是直接封闭类或结构类型的实例类型，如果查找标识实例成员，并且引用出现在实例构造函数、实例方法或实例访问器的主体中，则结果与 `this.I`窗体的成员访问（[成员访问](expressions.md#member-access)）相同。 仅当 `K` 为零时，才会发生这种情况。
      * 否则，结果将与表单的成员访问（[成员访问](expressions.md#member-access)） `T.I` 或 `T.I<A1,...,Ak>`相同。 在这种情况下， *simple_name*引用实例成员，则为绑定时错误。

*  否则，对于每个命名空间 `N`，从发生*simple_name*的命名空间开始，继续每个封闭命名空间（如果有），并以全局命名空间结束，将计算以下步骤直到找到实体：
   *  如果 `K` 为零且 `I` 是 `N`中的命名空间的名称，则：
      * 如果*simple_name*发生的位置由 `N` 的命名空间声明括起来，并且命名空间声明包含将名称 `I` 与命名空间或类型相关联的*extern_alias_directive*或*using_alias_directive* ，则*simple_name*为不明确，并发生编译时错误。
      * 否则， *simple_name*引用 `N`中名为 `I` 的命名空间。
   *  否则，如果 `N` 包含名称 `I` 且 `K` 类型参数的可访问类型，则：
      * 如果 `K` 为零，且*simple_name*的位置由 `N` 的命名空间声明括起来，并且命名空间声明包含将名称 using_alias_directive 与命名空间或类型相关联的*extern_alias_directive*或 * `I`* ，则*simple_name*是不明确的，并发生编译时错误。
      * 否则， *namespace_or_type_name*引用用给定类型参数构造的类型。
   *  否则，如果*simple_name*发生的位置由 `N`的命名空间声明括起来：
      * 如果 `K` 为零，且命名空间声明包含将名称 `I` 与导入的命名空间或类型相关联的*extern_alias_directive*或*using_alias_directive* ，则*simple_name*引用该命名空间或类型。
      * 否则，如果由*using_namespace_directive*s 和*using_static_directive*s 命名空间声明导入的命名空间和类型声明包含一个名称 `I` 和 `K` 类型参数的可访问类型或非扩展静态成员，则*simple_name*引用使用给定类型参数构造的该类型或成员。
      * 否则，如果命名空间声明中*using_namespace_directive*s 所导入的命名空间和类型包含多个具有名称 `I` 和 `K` 类型参数的可访问的类型或非扩展方法静态成员，则*simple_name*不明确，并发生错误。

   请注意，整个步骤与处理*namespace_or_type_name* （[命名空间和类型名称](basic-concepts.md#namespace-and-type-names)）的相应步骤完全相同。

*  否则， *simple_name*是未定义的，并且发生编译时错误。


### <a name="parenthesized-expressions"></a>带括号的表达式

*Parenthesized_expression*包含用括号括起来的*表达式*。

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

通过计算括号内的*表达式*来计算*parenthesized_expression* 。 如果括号内的*表达式*表示命名空间或类型，则会发生编译时错误。 否则， *parenthesized_expression*的结果是包含的*表达式*的计算结果。

### <a name="member-access"></a>成员访问

*Member_access*包含*primary_expression*、 *predefined_type*或*qualified_alias_member*，后跟一个 "`.`" 标记，后跟一个*标识符*，可选择后跟*type_argument_list*。

```antlr
member_access
    : primary_expression '.' identifier type_argument_list?
    | predefined_type '.' identifier type_argument_list?
    | qualified_alias_member '.' identifier
    ;

predefined_type
    : 'bool'   | 'byte'  | 'char'  | 'decimal' | 'double' | 'float' | 'int' | 'long'
    | 'object' | 'sbyte' | 'short' | 'string'  | 'uint'   | 'ulong' | 'ushort'
    ;
```

*Qualified_alias_member*生产是在[命名空间别名限定符](namespaces.md#namespace-alias-qualifiers)中定义的。

*Member_access*的格式为 `E.I` 或格式 `E.I<A1, ..., Ak>`，其中 `E` 是主表达式，`I` 是单个标识符，`<A1, ..., Ak>` 是可选*type_argument_list*。 当未指定*type_argument_list*时，请考虑 `K` 为零。

*Primary_expression*类型 `dynamic` 的*member_access*是动态绑定的（[动态绑定](expressions.md#dynamic-binding)）。 在这种情况下，编译器将成员访问分类为 `dynamic`类型的属性访问。 下面的规则使用运行时类型（而不是*primary_expression*的编译时类型）在运行时应用*member_access*的含义。 如果此运行时分类导致了方法组，则成员访问必须是*invocation_expression*的*primary_expression* 。

将按如下所示对*member_access*进行评估和分类：

*  如果 `K` 为零且 `E` 为命名空间并且 `E` 包含名称 `I`的嵌套命名空间，则结果为该命名空间。
*  否则，如果 `E` 是一个命名空间，并且 `E` 包含名称 `I` 和 `K` 类型参数的可访问类型，则结果为使用给定类型参数构造的类型。
*  如果 `E` 是*predefined_type*或归类为类型的*primary_expression* ，则如果 `E` 不是类型形参，并且 `I` 中 `E` 的成员查找（[成员查找](expressions.md#member-lookup)）使用 `K`，则类型参数将生成匹配项，然后按如下方式计算  和分类：
   *  如果 `I` 标识一个类型，则结果是使用给定的类型参数构造的类型。
   *  如果 `I` 标识一个或多个方法，则结果是没有关联的实例表达式的方法组。 如果指定了类型参数列表，它将用于调用泛型方法（[方法调用](expressions.md#method-invocations)）。
   *  如果 `I` 确定 `static` 属性，则结果为无关联的实例表达式的属性访问。
   *  如果 `I` 确定 `static` 字段：
      * 如果字段为 `readonly` 并且引用出现在声明该字段的类或结构的静态构造函数之外，则结果为值，即 `E`中的静态字段 `I` 的值。
      * 否则，结果为变量，即 `E`中 `I` 的静态字段。
   *  如果 `I` 标识 `static` 事件：
      * 如果引用出现在声明事件的类或结构中，并且声明事件时没有*event_accessor_declarations* （[事件](classes.md#events)），则 `E.I` 的处理方式与 `I` 为静态字段的方式完全相同。
      * 否则，结果是没有关联的实例表达式的事件访问。
   *  如果 `I` 标识一个常量，则结果为值，即该常量的值。
    * 如果 `I` 标识枚举成员，则结果为值，即枚举成员的值。
    * 否则，`E.I` 是无效的成员引用，并发生编译时错误。
*  如果 `E` 是属性访问、索引器访问、变量或值、 `T`的类型，并且在 `T` 中包含 `K`的 `I` 的成员查找（[成员查找](expressions.md#member-lookup)）， 类型参数会生成匹配项，然后按如下所示对 `E.I` 进行计算和分类：
   *  首先，如果 `E` 是属性或索引器访问，则获取属性或索引器访问的值（[表达式的值](expressions.md#values-of-expressions)），并将 `E` 重新分类为值。
   *  如果 `I` 标识了一个或多个方法，则结果将是具有 `E`的关联实例表达式的方法组。 如果指定了类型参数列表，它将用于调用泛型方法（[方法调用](expressions.md#method-invocations)）。
   *  如果 `I` 标识实例属性，
      * 如果 `E` `this`，`I` 将标识一个自动实现的属性（[自动实现的属性](classes.md#automatically-implemented-properties)），而不使用 setter，并且引用出现在类或结构类型 `T`的实例构造函数中，则结果是一个变量，即 `I` 给定的 `T` 的实例中的 `this`所给定的自动属性的隐藏支持字段。
      * 否则，结果为具有 `E`的关联实例表达式的属性访问。
   *  如果 `T` 是*class_type*并且 `I` 标识该*class_type*的实例字段：
      * 如果 `null``E` 的值，则将引发 `System.NullReferenceException`。
      * 否则，如果字段 `readonly` 并且引用出现在声明该字段的类的实例构造函数之外，则结果为值，即 `E`引用的对象中的字段的值 `I`。
      * 否则，结果为变量，即 `E`所引用的对象中 `I` 字段。
   *  如果 `T` 是*struct_type*并且 `I` 标识该*struct_type*的实例字段：
      * 如果 `E` 是一个值，或如果该字段 `readonly` 并且引用出现在声明该字段的结构的实例构造函数之外，则结果是一个值，即 `E`指定的结构实例中的字段的值 `I`。
      * 否则，结果为变量，即 `E`指定的结构实例中的字段 `I`。
   *  如果 `I` 标识实例事件：
      * 如果引用出现在声明了事件的类或结构中，并且事件是在没有*event_accessor_declarations* （[事件](classes.md#events)）的情况下声明的，并且引用不是作为 `+=` 或 `-=` 运算符的左侧出现，则 `E.I` 的处理方式与 `I` 为实例字段的方式完全相同。
      * 否则，结果为具有 `E`的关联实例表达式的事件访问。
*  否则，会尝试将 `E.I` 处理为扩展方法调用（[扩展方法](expressions.md#extension-method-invocations)调用）。 如果此操作失败，`E.I` 是无效的成员引用，并发生绑定时错误。

#### <a name="identical-simple-names-and-type-names"></a>相同的简单名称和类型名称

在窗体 `E.I`的成员访问中，如果 `E` 是单个标识符，并且 `E` 为*simple_name* （[简单名称](expressions.md#simple-names)）是与 *`E` （* [命名空间和类型名称](basic-concepts.md#namespace-and-type-names)）具有相同类型的常数、字段、属性、局部变量或参数，则允许 type_name 的两种可能含义。 `E.I` 的两个可能含义不明确，因为 `I` 必须是这两种情况下 `E` 类型的成员。 换言之，该规则只允许访问静态成员和嵌套类型的 `E`，在这种情况下，会发生编译时错误。 例如：
```csharp
struct Color
{
    public static readonly Color White = new Color(...);
    public static readonly Color Black = new Color(...);

    public Color Complement() {...}
}

class A
{
    public Color Color;                // Field Color of type Color

    void F() {
        Color = Color.Black;           // References Color.Black static member
        Color = Color.Complement();    // Invokes Complement() on Color field
    }

    static void G() {
        Color c = Color.White;         // References Color.White static member
    }
}
```

#### <a name="grammar-ambiguities"></a>语法多义性

*Simple_name*的生产（[简单名称](expressions.md#simple-names)）和*member_access* （[成员访问](expressions.md#member-access)）可以在表达式的语法中带来歧义。 例如，语句：
```csharp
F(G<A,B>(7));
```
可以解释为对具有两个自变量的 `F` 的调用 `G < A` 和 `B > (7)`。 或者，它可以解释为对 `F` 的调用，该参数是一个参数，它是对具有两个类型参数和一个正则参数的泛型方法的调用 `G`。

如果可以将标记序列（在上下文中）作为*simple_name* （[简单名称](expressions.md#simple-names)）、 *member_access* （[成员访问](expressions.md#member-access)）或*pointer_member_access* （[指针成员访问](unsafe-code.md#pointer-member-access)）以*type_argument_list* （[类型参数](types.md#type-arguments)）结尾，则将检查紧跟 `>` 结束标记的标记。 如果是
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
然后， *type_argument_list*将作为*simple_name*的一部分保留， *member_access*或*pointer_member_access* ，并放弃标记序列的任何其他可能分析。 否则，不会将*type_argument_list*视为*simple_name*、 *member_access*或*pointer_member_access*的一部分，即使没有其他可能的标记序列分析也是如此。 请注意，分析*namespace_or_type_name*中的*type_argument_list* （[命名空间和类型名称](basic-concepts.md#namespace-and-type-names)）时，不会应用这些规则。 语句
```csharp
F(G<A,B>(7));
```
根据此规则，将解释为对 `F` 的调用，该参数是一个参数，它是对具有两个类型参数和一个正则参数的泛型方法的调用 `G`。 语句
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
每个都将解释为对具有两个自变量的 `F` 的调用。 语句
```csharp
x = F < A > +y;
```
将解释为小于运算符、大于运算符和一元正运算符，就好像语句已 `x = (F < A) > (+y)`编写，而不是以*type_argument_list*后跟二元加运算符的*simple_name* 。 在语句中
```csharp
x = y is C<T> + z;
```
标记 `C<T>` 被解释为具有*type_argument_list*的*namespace_or_type_name* 。

### <a name="invocation-expressions"></a>调用表达式

用于调用方法的*invocation_expression* 。

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

如果以下至少一项保留，则*invocation_expression*是动态绑定的（[动态绑定](expressions.md#dynamic-binding)）：

* *Primary_expression*具有 `dynamic`的编译时类型。
* 至少一个可选*argument_list*的参数 `dynamic` 编译时类型，而*primary_expression*没有委托类型。

在这种情况下，编译器将*invocation_expression*分类为 `dynamic`类型的值。 下面的规则用于确定*invocation_expression*的含义，然后使用运行时类型（而不是*primary_expression*的参数和编译时类型 `dynamic`的编译时类型）在运行时应用。 如果*primary_expression*没有 `dynamic`的编译时类型，则方法调用会经历有限的编译时检查，如[编译时检查动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution)中所述。

*Invocation_expression*的*primary_expression*必须为方法组或*delegate_type*的值。 如果*primary_expression*为方法组，则*invocation_expression*为方法调用（[方法调用](expressions.md#method-invocations)）。 如果*primary_expression*是*delegate_type*的值，则*Invocation_expression*为委托调用（[委托调用](expressions.md#delegate-invocations)）。 如果*primary_expression*既不是方法组也不是*delegate_type*的值，则发生绑定时错误。

可选*argument_list* （[参数列表](expressions.md#argument-lists)）为方法的参数提供值或变量引用。

评估*invocation_expression*的结果归类如下：

*  如果*invocation_expression*调用返回 `void`的方法或委托，则结果为 nothing。 仅在*statement_expression* （[expression 语句](statements.md#expression-statements)）的上下文中或作为*lambda_expression* （[匿名函数表达式](expressions.md#anonymous-function-expressions)）的主体时，才允许将表达式归类为 nothing。 否则，会发生绑定时错误。
*  否则，结果为方法或委托返回的类型的值。

#### <a name="method-invocations"></a>方法调用

对于方法调用， *invocation_expression*的*primary_expression*必须为方法组。 方法组标识一种要调用的方法或用于从中选择要调用的特定方法的重载方法集。 在后一种情况下，确定要调用的特定方法的方式取决于*argument_list*中的参数类型所提供的上下文。

对形式的方法调用的绑定时处理 `M(A)`，其中 `M` 是一个方法组（可能包括一个*type_argument_list*），而 `A` 是一个可选*argument_list*，其中包含以下步骤：

*  构造方法调用的候选方法集。 对于每个与方法组相关联 `F` 方法 `M`：
   *  如果 `F` 不是泛型的，则在以下情况下 `F` 为候选项：
      * `M` 没有类型参数列表，并且
      * `F` 适用于 `A` （[适用的函数成员](expressions.md#applicable-function-member)）。
   *  如果 `F` 是泛型且 `M` 没有类型参数列表，则在以下情况下 `F` 是候选项：
      * 类型推理（[类型推理](expressions.md#type-inference)）成功，推断调用的类型参数列表，
      * 将推断类型参数替换为相应的方法类型参数后，F 的参数列表中的所有构造类型都满足其约束（[满足约束](types.md#satisfying-constraints)），`F` 的参数列表适用于 `A` （[适用的函数成员](expressions.md#applicable-function-member)）。
   *  如果 `F` 是泛型并且 `M` 包含类型参数列表，则在以下情况下 `F` 是候选项：
      * `F` 具有与类型参数列表中提供的相同数量的方法类型参数，并且
      * 将类型参数替换为相应的方法类型参数后，F 的参数列表中的所有构造类型都满足其约束（[满足约束](types.md#satisfying-constraints)），并且 `F` 的参数列表适用于 `A` （[适用的函数成员](expressions.md#applicable-function-member)）。
*  候选方法集减少为仅包含派生程度最高的类型中的方法：对于集中 `C.F` 的每个方法，其中 `C` 是声明方法 `F` 的类型，而在 `C` 的基类型中声明的所有方法将从集合中删除。 此外，如果 `C` 是 `object`以外的类类型，则将从集合中删除在接口类型中声明的所有方法。 （仅当方法组是具有非 object 和非空有效接口集的有效基类的类型形参上的成员查找结果时，这一规则才会生效。）
*  如果生成的候选方法集为空，则将放弃按照以下步骤进行的进一步处理，而是尝试将调用作为扩展方法调用（[扩展方法调用](expressions.md#extension-method-invocations)）进行处理。 如果此操作失败，则不存在适用的方法，并且发生绑定时错误。
*  使用[重载决策](expressions.md#overload-resolution)的重载决策规则标识候选方法集的最佳方法。 如果无法识别单个最佳方法，则方法调用是不明确的，并发生绑定时错误。 在执行重载决策时，将在替换相应方法类型参数的类型参数（提供或推断）后考虑泛型方法的参数。
*  执行所选最佳方法的最终验证：
   * 方法在方法组的上下文中进行验证：如果最佳方法为静态方法，则方法组必须通过类型*simple_name*或*member_access* 。 如果最佳方法是实例方法，则方法组必须由*simple_name*、 *member_access*通过变量或值或*base_access*导致。 如果这两个要求都不成立，则发生绑定时错误。
   * 如果最佳方法是泛型方法，则会对照泛型方法上声明的约束（[满足约束](types.md#satisfying-constraints)）检查类型参数（提供或推断）。 如果任何类型自变量不满足类型参数上的相应约束，则会发生绑定时错误。

按照上述步骤在绑定时选择并验证方法后，将根据[动态重载解析的编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)中所述的函数成员调用规则来处理实际的运行时调用。

上述解决方法的直观效果如下所示：若要查找由方法调用调用的特定方法，请从方法调用指示的类型开始，然后继续继承链，直到至少有一个适用的。可访问，但却找到了非替代方法声明。 然后，对该类型中声明的适用的、可访问的、非重写方法集执行类型推理和重载决策，并调用此方法。 如果未找到方法，请改为尝试将调用作为扩展方法调用来处理。

#### <a name="extension-method-invocations"></a>扩展方法调用

在方法调用中（[装箱实例上的调用](expressions.md#invocations-on-boxed-instances)），其中一个窗体
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
如果调用的正常处理找不到适用的方法，则会尝试将构造作为扩展方法调用来处理。 如果*expr*或任意*参数*的编译时类型 `dynamic`，则扩展方法将不适用。

目标是查找最佳*type_name* `C`，以便可以进行相应的静态方法调用：
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

如果满足以下条件，扩展方法 `Ci.Mj`***适用***：

*  `Ci` 是非泛型非泛型类
*  `Mj` 的名称是*标识符*
*  当作为如下所示的静态方法应用于参数时，`Mj` 是可访问和适用的
*  存在从*expr*到 `Mj`的第一个参数类型的隐式标识、引用或装箱转换。

搜索 `C` 按如下方式进行：

*  从最接近的封闭命名空间声明开始，继续每个封闭命名空间声明，并以包含编译单元结束，接下来尝试查找一组候选的扩展方法：
   * 如果给定的命名空间或编译单元直接包含 `Ci` 具有符合条件的扩展 `Mj`方法的非泛型类型声明，则这些扩展方法集是候选集。
   * 如果在给定的命名空间或编译单元中*using_namespace_directive*s 导入的命名空间中 `Ci` 导入*using_static_declarations*并直接声明的类型 `Mj`，则这些扩展方法集是候选集。
*  如果在任何封闭命名空间声明或编译单元中均未找到候选集，则会发生编译时错误。
*  否则，将重载决策应用于候选集，如中所述（[重载决策](expressions.md#overload-resolution)）。 如果未找到单个最佳方法，则会发生编译时错误。
*  `C` 是将最佳方法声明为扩展方法的类型。

将 `C` 用作目标，然后将方法调用处理为静态方法调用（对[动态重载决策进行编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）。

前面的规则意味着实例方法优先于扩展方法，内部命名空间声明中提供的扩展方法优先于外部命名空间声明中提供的扩展方法和该扩展直接在命名空间中声明的方法优先于通过使用命名空间指令导入到同一命名空间中的扩展方法。 例如：
```csharp
public static class E
{
    public static void F(this object obj, int i) { }

    public static void F(this object obj, string s) { }
}

class A { }

class B
{
    public void F(int i) { }
}

class C
{
    public void F(object obj) { }
}

class X
{
    static void Test(A a, B b, C c) {
        a.F(1);              // E.F(object, int)
        a.F("hello");        // E.F(object, string)

        b.F(1);              // B.F(int)
        b.F("hello");        // E.F(object, string)

        c.F(1);              // C.F(object)
        c.F("hello");        // C.F(object)
    }
}
```

在此示例中，`B`的方法优先于第一个扩展方法，`C`的方法优先于这两个扩展方法。

```csharp
public static class C
{
    public static void F(this int i) { Console.WriteLine("C.F({0})", i); }
    public static void G(this int i) { Console.WriteLine("C.G({0})", i); }
    public static void H(this int i) { Console.WriteLine("C.H({0})", i); }
}

namespace N1
{
    public static class D
    {
        public static void F(this int i) { Console.WriteLine("D.F({0})", i); }
        public static void G(this int i) { Console.WriteLine("D.G({0})", i); }
    }
}

namespace N2
{
    using N1;

    public static class E
    {
        public static void F(this int i) { Console.WriteLine("E.F({0})", i); }
    }

    class Test
    {
        static void Main(string[] args)
        {
            1.F();
            2.G();
            3.H();
        }
    }
}
```

此示例的输出为：
```console
E.F(1)
D.G(2)
C.H(3)
```
`D.G` 优先于 `C.G`，`E.F` 优先于 `D.F` 和 `C.F`。

#### <a name="delegate-invocations"></a>委托调用

对于委托调用， *invocation_expression*的*primary_expression*必须是*delegate_type*的值。 此外，将*delegate_type*视为与*delegate_type*具有相同参数列表的函数成员， *delegate_type*必须适用于与*invocation_expression*的*argument_list*相关的（[适用的函数成员](expressions.md#applicable-function-member)）。

`D(A)`格式的委托调用的运行时处理，其中 `D` 是*delegate_type*的*primary_expression* ，`A` 是一个可选的*argument_list*，其中包含以下步骤：

*  计算 `D`。 如果此计算导致异常，则不执行其他步骤。
*  检查 `D` 的值是否有效。 如果 `null``D` 的值，则将引发 `System.NullReferenceException`，并且不执行任何其他步骤。
*  否则，`D` 是对委托实例的引用。 对委托调用列表中的每个可调用实体执行函数成员调用（对[动态重载决策进行编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）。 对于包含实例和实例方法的可调用实体，调用实例是包含在可调用实体中的实例。

### <a name="element-access"></a>元素访问

*Element_access*包含一个*primary_no_array_creation_expression*，后跟一个 "`[`" 标记，后跟一个*argument_list*，后跟一个 "`]`" 标记。 *Argument_list*由一个或多个由逗号分隔的*参数*组成。

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

不允许*element_access*的*argument_list*包含 `ref` 或 `out` 参数。

如果以下至少一项保留，则*element_access*是动态绑定的（[动态绑定](expressions.md#dynamic-binding)）：

* *Primary_no_array_creation_expression*具有 `dynamic`的编译时类型。
* 至少一个*argument_list*的表达式 `dynamic` 有编译时类型，而*primary_no_array_creation_expression*没有数组类型。

在这种情况下，编译器将*element_access*分类为 `dynamic`类型的值。 下面的规则用于确定*element_access*的含义，然后使用运行时类型（而不是*primary_no_array_creation_expression*和*argument_list*具有编译时类型 `dynamic`的表达式的编译时类型）在运行时应用。 如果*primary_no_array_creation_expression*没有 `dynamic`的编译时类型，则元素访问会经历有限的编译时检查，如对[动态重载决策进行编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)中所述。

如果*element_access*的*primary_no_array_creation_expression*是*array_type*的值，则*element_access*是数组访问（[数组访问](expressions.md#array-access)）。 否则， *primary_no_array_creation_expression*必须是具有一个或多个索引器成员的类、结构或接口类型的变量或值，在这种情况下， *element_access*是索引器访问（[索引器访问](expressions.md#indexer-access)）。

#### <a name="array-access"></a>数组访问

对于数组访问， *element_access*的*primary_no_array_creation_expression*必须是*array_type*的值。 而且，不允许数组访问的*argument_list*包含命名参数。*Argument_list*中的表达式的数目必须与*array_type*的排名相同，并且每个表达式的类型必须为 `int`、`uint`、`long`、`ulong`，或者必须能够隐式转换为这些类型中的一个或多个。

计算数组访问的结果是数组的元素类型的一个变量，即由*argument_list*中的表达式的值所选择的数组元素。

`P[A]`格式的数组访问的运行时处理，其中 `P` 是*array_type*的*primary_no_array_creation_expression* ，`A` 是*argument_list*，其中包含以下步骤：

*  计算 `P`。 如果此计算导致异常，则不执行其他步骤。
*  将按从左到右的顺序计算*argument_list*的索引表达式。 对于每个索引表达式的求值，将执行隐式转换（[隐式](conversions.md#implicit-conversions)转换）为以下类型之一： `int`、`uint`、`long``ulong`。 在此列表中选择了隐式转换的第一种类型。 例如，如果索引表达式的类型为 `short` 则执行到 `int` 的隐式转换，因为从 `short` 到 `int` 的隐式转换和从 `short` 到 `long` 都是可能的。 如果索引表达式的计算或后续的隐式转换导致异常，则不会再计算索引表达式，也不会执行任何其他步骤。
*  检查 `P` 的值是否有效。 如果 `null``P` 的值，则将引发 `System.NullReferenceException`，并且不执行任何其他步骤。
*  根据 `P`引用的数组实例的每个维度的实际界限检查*argument_list*中每个表达式的值。 如果一个或多个值超出范围，则会引发 `System.IndexOutOfRangeException`，而不执行进一步的步骤。
*  计算索引表达式给定的数组元素的位置，此位置成为数组访问的结果。

#### <a name="indexer-access"></a>索引器访问

对于索引器访问， *element_access*的*primary_no_array_creation_expression*必须是类、结构或接口类型的变量或值，并且此类型必须实现适用于*element_access*的*argument_list*的一个或多个索引器。

对窗体 `P[A]`的索引器访问的绑定时处理，其中 `P` 是类、结构或接口类型 `T`的*primary_no_array_creation_expression* ，`A` 是*argument_list*，其中包含以下步骤：

*  构造由 `T` 提供的一组索引器。 该集包含在 `T` 中声明的所有索引器，或不 `override` 声明并且可在当前上下文（[成员访问](basic-concepts.md#member-access)）中访问的 `T` 基类型。
*  该集被简化为那些适用且不被其他索引器隐藏的索引器。 以下规则应用于集中的每个索引器 `S.I`，其中 `S` 是声明索引器 `I` 的类型：
   * 如果 `I` 不适用于 `A` （[适用的函数成员](expressions.md#applicable-function-member)），则从集中删除 `I`。
   * 如果 `I` 适用于 `A` （[适用的函数成员](expressions.md#applicable-function-member)），则将从集合中删除在 `S` 的基类型中声明的所有索引器。
   * 如果 `I` 适用于 `A` （[适用的函数成员](expressions.md#applicable-function-member)），而 `S` 是 `object`以外的类类型，则将从集合中删除在接口中声明的所有索引器。
*  如果生成的候选索引器集为空，则不存在适用的索引器，并且发生绑定时错误。
*  使用[重载决策](expressions.md#overload-resolution)的重载决策规则识别候选索引器集的最佳索引器。 如果无法识别单个最佳索引器，索引器访问不明确，并发生绑定时错误。
*  将按从左到右的顺序计算*argument_list*的索引表达式。 处理索引器访问的结果是分类为索引器访问的表达式。 索引器访问表达式引用前面步骤中确定的索引器，并且具有 `P` 的关联实例表达式和 `A`的关联自变量列表。

索引器访问会导致索引器的*get*访问器或*set 访问*器的调用，具体取决于使用它的上下文。 如果索引器访问是赋值的目标，则调用*set 访问器*来分配新值（[简单赋值](expressions.md#simple-assignment)）。 在所有其他情况下，将调用*get 访问器*以获取当前值（[表达式的值](expressions.md#values-of-expressions)）。

### <a name="this-access"></a>此访问

*This_access*包含保留字 `this`。

```antlr
this_access
    : 'this'
    ;
```

只能在实例构造函数、实例方法或实例访问器的*块*中使用*this_access* 。 它具有下列含义之一：

*  当在类的实例构造函数中的*primary_expression*中使用 `this` 时，它将分类为值。 值的类型为在其中发生使用的类的实例类型（[实例类型](classes.md#the-instance-type)），值是对正在构造的对象的引用。
*  当在类的实例方法或实例访问器中的*primary_expression*中使用 `this` 时，它将分类为值。 值的类型为在其中发生使用的类的实例类型（[实例类型](classes.md#the-instance-type)），值是对为其调用方法或访问器的对象的引用。
*  如果在结构的实例构造函数中的*primary_expression*中使用 `this`，则将其归类为变量。 变量的类型是在其中发生使用的结构的实例类型（[实例类型](classes.md#the-instance-type)），而变量表示正在构造的结构。 结构的实例构造函数的 `this` 变量的行为与结构类型的 `out` 参数完全相同，具体而言，这意味着必须在实例构造函数的每个执行路径中明确分配变量。
*  如果在结构的实例方法或实例访问器中的*primary_expression*中使用 `this`，则将其归类为变量。 变量的类型是发生使用的结构的实例类型（[实例类型](classes.md#the-instance-type)）。
   * 如果方法或访问器不是迭代器（[迭代](classes.md#iterators)器），则 `this` 变量表示为其调用方法或访问器的结构，其行为与结构类型的 `ref` 参数完全相同。
   * 如果方法或访问器是迭代器，则 `this` 变量表示为其调用了方法或访问器的结构的副本，其行为与结构类型的值参数完全相同。

如果在不是上面列出的上下文中的*primary_expression*中使用 `this`，则会发生编译时错误。 具体而言，不能引用静态方法、静态属性访问器或字段声明的*variable_initializer*中的 `this`。

### <a name="base-access"></a>基本访问权限

*Base_access*包含保留字 `base` 后跟 "`.`" 标记、标识符或用方括号括起来的*argument_list* ：

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

*Base_access*用于访问当前类或结构中名称类似的成员隐藏的基类成员。 只能在实例构造函数、实例方法或实例访问器的*块*中使用*base_access* 。 当在类或结构中发生 `base.I` 时，`I` 必须表示该类或结构的基类的成员。 同样，在类中发生 `base[E]` 时，基类中必须存在适用的索引器。

在绑定时，`base.I` 和 `base[E]` 形式的*base_access*表达式的计算方式与 `((B)this).I` 和 `((B)this)[E]`编写时完全一样，其中 `B` 是出现构造的类或结构的基类。 因此，`base.I` 和 `base[E]` 对应于 `this.I` 和 `this[E]`，只不过 `this` 被视为基类的实例。

当*base_access*引用虚拟函数成员（方法、属性或索引器）时，将更改在运行时调用哪个函数成员（[动态重载决策的编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）。 调用的函数成员是通过查找与 `B` 相关的函数成员的派生程度最高（[虚拟方法](classes.md#virtual-methods)）来确定的，而不是与 `this`的运行时类型相关，而在非基本访问中通常是如此。 因此，在 `virtual` 函数成员的 `override` 中， *base_access*可用于调用函数成员的继承实现。 如果*base_access*引用的函数成员是抽象的，则会发生绑定时错误。

### <a name="postfix-increment-and-decrement-operators"></a>后缀增量和减量运算符

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

后缀递增或递减运算的操作数必须是分类为变量、属性访问或索引器访问的表达式。 操作的结果是与操作数类型相同的值。

如果*primary_expression*具有编译时类型 `dynamic` 则运算符是动态绑定的（[动态绑定](expressions.md#dynamic-binding)）， *post_increment_expression*或*post_decrement_expression*具有编译时类型 `dynamic` 并且以下规则在运行时使用*primary_expression*的运行时类型应用。

如果后缀递增或递减运算的操作数是属性或索引器访问，则属性或索引器必须同时具有 `get` 和 `set` 访问器。 如果不是这种情况，则会发生绑定时错误。

一元运算符重载决策（[一元运算符重载决策](expressions.md#unary-operator-overload-resolution)）应用于选择特定的运算符实现。 为以下类型存在预定义的 `++` 和 `--` 运算符： `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`和任何枚举类型。 预定义的 `++` 运算符返回通过向操作数添加1而生成的值，预定义的 `--` 运算符返回通过从操作数中减去1而生成的值。 在 `checked` 上下文中，如果此加法或减法的结果超出了结果类型的范围，且结果类型为整型类型或枚举类型，则将引发 `System.OverflowException`。

`x++` 或 `x--` 形式的后缀递增或递减运算的运行时处理包括以下步骤：

*   如果 `x` 归类为变量：
    * 计算 `x` 以产生变量。
    * 保存 `x` 的值。
    * 将调用选定的运算符，并将 `x` 的保存值作为其参数。
    * 运算符返回的值存储在 `x`的计算给定的位置。
    * 的保存值 `x` 成为操作的结果。
*   如果 `x` 归类为属性或索引器访问：
    * 实例表达式（如果 `x` 不 `static`）和参数列表（如果 `x` 是索引器访问）与 `x` 关联，并在后续的 `get` 和 `set` 访问器调用中使用结果。
    * 调用 `x` 的 `get` 访问器，并保存返回值。
    * 将调用选定的运算符，并将 `x` 的保存值作为其参数。
    * 使用运算符作为其 `value` 参数返回的值调用 `x` 的 `set` 访问器。
    * 的保存值 `x` 成为操作的结果。

`++` 和 `--` 运算符还支持前缀表示法（[前缀增量和减量运算符](expressions.md#prefix-increment-and-decrement-operators)）。 通常，`x++` 或 `x--` 的结果是操作前 `x` 的值，而 `++x` 或 `--x` 的结果是操作后 `x` 的值。 在任一情况下，操作后 `x` 本身具有相同的值。

可以使用后缀或前缀表示法来调用 `operator ++` 或 `operator --` 实现。 对于这两个表示法，不能有单独的运算符实现。

### <a name="the-new-operator"></a>调用 new 运算符

`new` 运算符用于创建类型的新实例。

有三种形式的 `new` 表达式：

*  对象创建表达式用于创建类类型和值类型的新实例。
*  数组创建表达式用于创建数组类型的新实例。
*  委托创建表达式用于创建委托类型的新实例。

`new` 运算符表示创建类型的实例，但不一定表示动态分配内存。 具体而言，值类型的实例不需要超出它们所在的变量的额外内存，并且当 `new` 用于创建值类型的实例时，不会发生动态分配。

#### <a name="object-creation-expressions"></a>对象创建表达式

*Object_creation_expression*用于创建*class_type*或*value_type*的新实例。

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;

object_or_collection_initializer
    : object_initializer
    | collection_initializer
    ;
```

*Object_creation_expression*的*类型*必须是*class_type*、 *value_type*或*type_parameter*。 *类型*不能是 `abstract` *class_type*。

仅当*类型*为*class_type*或*struct_type*时，才允许使用可选*argument_list* （[参数列表](expressions.md#argument-lists)）。

如果对象创建表达式包括对象初始值设定项或集合初始值设定项，则它可以省略构造函数参数列表和外括号。 省略构造函数参数列表和外括号等效于指定空参数列表。

处理包括对象初始值设定项或集合初始值设定项的对象创建表达式包括首先处理实例构造函数，然后处理对象初始值设定项（[对象初始值设定](expressions.md#object-initializers)项）或集合初始值设定项（[集合](expressions.md#collection-initializers)初始值设定项）指定的成员或元素初始化。

如果可选*argument_list*中的任何参数具有编译时类型 `dynamic` 则*object_creation_expression*为动态绑定（[动态绑定](expressions.md#dynamic-binding)），并且以下规则在运行时使用编译时类型为 `dynamic`的*argument_list*的参数的运行时类型。 但是，对象创建会经历有限的编译时检查，如对[动态重载决策进行编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)中所述。

格式为 `new T(A)`的*object_creation_expression*的绑定时处理，其中 `T` 是*class_type*或*value_type* ，`A` 是可选的*argument_list*，其中包含以下步骤：

*   如果 `T` 是*value_type*并且 `A` 不存在：
    * *Object_creation_expression*是默认的构造函数调用。 *Object_creation_expression*的结果是 `T`类型的值，即在 system.string[类型](types.md#the-systemvaluetype-type)中定义 `T` 的默认值。
*   否则，如果 `T` 是*type_parameter*并且 `A` 不存在：
    * 如果没有为 `T`指定任何值类型约束或构造函数约束（[类型参数约束](classes.md#type-parameter-constraints)），将发生绑定时错误。
    * *Object_creation_expression*的结果是类型参数已绑定到的运行时类型的值，即调用该类型的默认构造函数的结果。 运行时类型可以是引用类型或值类型。
*   否则，如果 `T` 是*class_type*或*struct_type*：
    * 如果 `T` 是 `abstract` *class_type*，则会发生编译时错误。
    * 使用[重载决策](expressions.md#overload-resolution)的重载决策规则确定要调用的实例构造函数。 候选实例构造函数集由 `T` 中声明的所有可访问实例构造函数组成，它们适用于 `A` （[适用的函数成员](expressions.md#applicable-function-member)）。 如果候选实例构造函数集为空，或者无法识别单个最佳实例构造函数，则会发生绑定时错误。
    * *Object_creation_expression*的结果是 `T`类型的值，即通过调用上述步骤中确定的实例构造函数生成的值。
*  否则， *object_creation_expression*无效，并发生绑定时错误。

即使*object_creation_expression*是动态绑定的，编译时类型仍会 `T`。

格式为 `new T(A)`的*object_creation_expression*的运行时处理，其中 `T` 是*class_type*或*struct_type* ，`A` 是可选的*argument_list*，其中包含以下步骤：

*   如果 `T` 是*class_type*：
    * 分配 `T` 类的新实例。 如果内存不足，无法分配新的实例，则会引发 `System.OutOfMemoryException`，而不执行进一步的步骤。
    * 新实例的所有字段都初始化为其默认值（[默认值](variables.md#default-values)）。
    * 实例构造函数根据函数成员调用的规则进行调用（对[动态重载决策进行编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）。 对新分配的实例的引用会自动传递到实例构造函数，并且可以从该构造函数内作为 `this`访问该实例。
*   如果 `T` 是*struct_type*：
    * `T` 类型的实例是通过分配临时本地变量创建的。 由于需要*struct_type*的实例构造函数来明确地为要创建的实例的每个字段赋值，因此不需要初始化临时变量。
    * 实例构造函数根据函数成员调用的规则进行调用（对[动态重载决策进行编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）。 对新分配的实例的引用会自动传递到实例构造函数，并且可以从该构造函数内作为 `this`访问该实例。

#### <a name="object-initializers"></a>对象初始值设定项

***对象初始值设定项***为零个或多个字段、属性或对象的索引元素指定值。

```antlr
object_initializer
    : '{' member_initializer_list? '}'
    | '{' member_initializer_list ',' '}'
    ;

member_initializer_list
    : member_initializer (',' member_initializer)*
    ;

member_initializer
    : initializer_target '=' initializer_value
    ;

initializer_target
    : identifier
    | '[' argument_list ']'
    ;

initializer_value
    : expression
    | object_or_collection_initializer
    ;
```

对象初始值设定项由成员初始值设定项的序列组成，括在 `{` 和 `}` 标记中，并用逗号分隔。 每个*member_initializer*为初始化指定一个目标。 *标识符*必须命名要初始化的对象的可访问字段或属性，而用方括号括起来的*argument_list*必须在要初始化的对象上指定可访问索引器的参数。 对象初始值设定项为同一个字段或属性包含多个成员初始值设定项是错误的。

每个*initializer_target*后跟一个等号和一个表达式、一个对象初始值设定项或集合初始值设定项。 对象初始值设定项中的表达式不能引用它要初始化的新创建的对象。

在按与目标的赋值（[简单赋值](expressions.md#simple-assignment)）相同的方式处理等号后，指定表达式的成员初始值设定项。

在等号后指定对象初始值设定项的成员初始值设定项是***嵌套的对象初始值设定***项，即嵌入对象的初始化。 嵌套的对象初始值设定项中的赋值被视为对该字段或属性的成员的赋值，而不是为字段或属性赋值。 嵌套的对象初始值设定项不能应用于值类型的属性，也不能应用于具有值类型的只读字段。

在等号后指定集合初始值设定项的成员初始值设定项是嵌入集合的初始化。 将初始值设定项中给定的元素添加到目标所引用的集合，而不是将新集合分配给目标字段、属性或索引器。 目标必须为满足[集合初始值设定项](expressions.md#collection-initializers)中指定的要求的集合类型。

索引初始值设定项的参数将始终只计算一次。 因此，即使参数不会使用（例如，由于空的嵌套初始值设定项），也会对其副作用进行评估。

下面的类表示一个具有两个坐标的点：
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

可以创建和初始化 `Point` 的实例，如下所示：
```csharp
Point a = new Point { X = 0, Y = 1 };
```
这与
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
其中 `__a` 是其他不可见且无法访问的临时变量。 下面的类表示从两个点创建的矩形：
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

可以创建和初始化 `Rectangle` 的实例，如下所示：
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
这与
```csharp
Rectangle __r = new Rectangle();
Point __p1 = new Point();
__p1.X = 0;
__p1.Y = 1;
__r.P1 = __p1;
Point __p2 = new Point();
__p2.X = 2;
__p2.Y = 3;
__r.P2 = __p2; 
Rectangle r = __r;
```
其中 `__r`、`__p1` 和 `__p2` 是临时变量，这些变量不可见且不可访问。

如果 `Rectangle`的构造函数分配两个嵌入的 `Point` 实例
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
以下构造可用于初始化嵌入的 `Point` 实例，而不是分配新实例：
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
这与
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

给定 C 的适当定义，以下示例：
```csharp
var c = new C {
    x = true,
    y = { a = "Hello" },
    z = { 1, 2, 3 },
    ["x"] = 5,
    [0,0] = { "a", "b" },
    [1,2] = {}
};
```
等效于这一系列的赋值：
```csharp
C __c = new C();
__c.x = true;
__c.y.a = "Hello";
__c.z.Add(1); 
__c.z.Add(2);
__c.z.Add(3);
string __i1 = "x";
__c[__i1] = 5;
int __i2 = 0, __i3 = 0;
__c[__i2,__i3].Add("a");
__c[__i2,__i3].Add("b");
int __i4 = 1, __i5 = 2;
var c = __c;
```
其中 `__c`（等等）生成的变量不可见且无法访问源代码。 请注意，`[0,0]` 的参数只计算一次，即使从未使用过，也会对 `[1,2]` 的参数进行一次计算。

#### <a name="collection-initializers"></a>集合初始值设定项

集合初始值设定项指定集合的元素。

```antlr
collection_initializer
    : '{' element_initializer_list '}'
    | '{' element_initializer_list ',' '}'
    ;

element_initializer_list
    : element_initializer (',' element_initializer)*
    ;

element_initializer
    : non_assignment_expression
    | '{' expression_list '}'
    ;

expression_list
    : expression (',' expression)*
    ;
```

集合初始值设定项由元素初始值设定项序列组成，括在 `{` 和 `}` 标记之间，并用逗号分隔。 每个元素初始值设定项指定一个元素，该元素将被添加到要初始化的集合对象，并由 `{` 和 `}` 令牌中包含的表达式列表以及用逗号分隔。  单表达式元素初始值设定项可以不带大括号编写，但是不能是赋值表达式，以避免成员初始值设定项出现歧义。 *Non_assignment_expression*生产是在[expression](expressions.md#expression)中定义的。

下面是包含集合初始值设定项的对象创建表达式的示例：
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

集合初始值设定项应用于的集合对象必须是实现 `System.Collections.IEnumerable` 或发生编译时错误的类型。 对于按顺序排列的每个指定元素，集合初始值设定项使用元素初始值设定项的表达式列表作为参数列表调用目标对象上的 `Add` 方法，并对每个调用应用常规成员查找和重载解析。 因此，集合对象必须具有适用于每个元素初始值设定项 `Add` 名称的相应实例或扩展方法。

下面的类表示一个联系人，其中包含名称和电话号码的列表：
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

可以按如下所示创建和初始化 `List<Contact>`：
```csharp
var contacts = new List<Contact> {
    new Contact {
        Name = "Chris Smith",
        PhoneNumbers = { "206-555-0101", "425-882-8080" }
    },
    new Contact {
        Name = "Bob Harris",
        PhoneNumbers = { "650-555-0199" }
    }
};
```
这与
```csharp
var __clist = new List<Contact>();
Contact __c1 = new Contact();
__c1.Name = "Chris Smith";
__c1.PhoneNumbers.Add("206-555-0101");
__c1.PhoneNumbers.Add("425-882-8080");
__clist.Add(__c1);
Contact __c2 = new Contact();
__c2.Name = "Bob Harris";
__c2.PhoneNumbers.Add("650-555-0199");
__clist.Add(__c2);
var contacts = __clist;
```
其中 `__clist`、`__c1` 和 `__c2` 是临时变量，这些变量不可见且不可访问。

#### <a name="array-creation-expressions"></a>数组创建表达式

*Array_creation_expression*用于创建*array_type*的新实例。

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

第一种形式的数组创建表达式将分配从表达式列表中删除每个表达式所得到的类型的数组实例。 例如，数组创建表达式 `new int[10,20]` 生成 `int[,]`类型的数组实例，而数组创建表达式 `new int[10][,]` 生成类型 `int[][,]`的数组。 表达式列表中的每个表达式的类型必须为 `int`、`uint`、`long`或 `ulong`，或可隐式转换为这些类型中的一个或多个。 每个表达式的值决定了新分配的数组实例中相应维度的长度。 由于数组维度的长度必须为非负，因此在表达式列表中有一个具有负值的*constant_expression*编译时错误。

除非在不安全的上下文中（[不安全](unsafe-code.md#unsafe-contexts)上下文），否则不指定数组的布局。

如果第一种形式的数组创建表达式包含数组初始值设定项，则表达式列表中的每个表达式都必须是常量，并且表达式列表指定的秩和维度长度必须与数组初始值设定项的匹配项和维度长度匹配。

在第二个或第三个窗体的数组创建表达式中，指定数组类型或秩说明符的秩必须与数组初始值设定项的秩匹配。 各个维度长度是从数组初始值设定项的每个相应嵌套级别中的元素数推断出来的。 因此，表达式
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
完全对应于
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

第三种形式的数组创建表达式称为***隐式类型化数组创建表达式***。 它类似于第二个窗体，只不过数组的元素类型不是显式给定的，而是确定为数组初始值设定项中的一组表达式的最佳通用类型（[查找一组表达式的最佳通用类型](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)）。 对于多维数组（即，其中*rank_specifier*至少包含一个逗号），此集包含在嵌套*array_initializer*中找到的所有*表达式*。

数组[初始值设定项中进一步](arrays.md#array-initializers)介绍了数组初始值设定项。

计算数组创建表达式的结果是一个值，即对新分配的数组实例的引用。 数组创建表达式的运行时处理包括以下步骤：

*  将按从左到右的顺序计算*expression_list*的维度长度表达式。 对每个表达式进行以下计算后，将执行隐式转换（[隐式](conversions.md#implicit-conversions)转换）为以下类型之一： `int`、`uint`、`long``ulong`。 在此列表中选择了隐式转换的第一种类型。 如果表达式的计算或后续的隐式转换导致异常，则不会计算进一步的表达式，也不会执行任何其他步骤。
*  按如下所示验证维度长度的计算值。 如果一个或多个值小于零，则会引发 `System.OverflowException`，而不执行进一步的步骤。
*  分配具有给定维度长度的数组实例。 如果内存不足，无法分配新的实例，则会引发 `System.OutOfMemoryException`，而不执行进一步的步骤。
*  新数组实例的所有元素都初始化为其默认值（[默认值](variables.md#default-values)）。
*  如果数组创建表达式包含数组初始值设定项，则计算数组初始值设定项中的每个表达式并将其分配给其相应的数组元素。 计算和分配按表达式在数组初始值设定项中写入的顺序执行，换言之，元素按递增的索引顺序进行初始化，最右边的维度首先增长。 如果给定表达式的计算或对相应数组元素的后续赋值导致异常，则不会初始化其他元素（因此剩余的元素将具有其默认值）。

数组创建表达式允许实例化包含数组类型的元素的数组，但此类数组的元素必须手动初始化。 例如，语句
```csharp
int[][] a = new int[100][];
```
创建具有100类型 `int[]`元素的一维数组。 每个元素的初始值都是 `null`。 同一个数组创建表达式不可能同时实例化子数组和语句
```csharp
int[][] a = new int[100][5];        // Error
```
导致编译时错误。 子数组的实例化必须改为手动执行，如下所示
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

当数组的数组具有 "矩形" 形状时，即当子数组的长度完全相同时，使用多维数组更为有效。 在上面的示例中，数组数组的实例化将创建101对象，即一个外部数组和100子数组。 相反，
```csharp
int[,] = new int[100, 5];
```
仅创建单个对象和二维数组，并在单个语句中完成分配。

下面是隐式类型的数组创建表达式的示例：
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

最后一个表达式会导致编译时错误，因为 `int` 和 `string` 都不能隐式转换为另一种类型，因此没有最常见的类型。 在这种情况下，必须使用显式类型化的数组创建表达式，例如指定要 `object[]`的类型。 或者，可以将其中一个元素强制转换为公共基类型，后者随后将成为推断元素类型。

隐式类型的数组创建表达式可与匿名对象初始值设定项（[匿名对象创建表达式](expressions.md#anonymous-object-creation-expressions)）结合，以创建匿名类型的数据结构。 例如：
```csharp
var contacts = new[] {
    new {
        Name = "Chris Smith",
        PhoneNumbers = new[] { "206-555-0101", "425-882-8080" }
    },
    new {
        Name = "Bob Harris",
        PhoneNumbers = new[] { "650-555-0199" }
    }
};
```

#### <a name="delegate-creation-expressions"></a>委托创建表达式

*Delegate_creation_expression*用于创建*delegate_type*的新实例。

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

委托创建表达式的参数必须是方法组、匿名函数或编译时类型的值 `dynamic` 或*delegate_type*。 如果参数为方法组，则它将标识方法，并为实例方法标识要为其创建委托的对象。 如果参数是匿名函数，它将直接定义委托目标的参数和方法主体。 如果参数是一个值，则它标识要创建副本的委托实例。

如果*表达式*具有编译时类型 `dynamic`，则*delegate_creation_expression*是动态绑定的（[动态绑定](expressions.md#dynamic-binding)），下面的规则将使用*表达式*的运行时类型在运行时应用。 否则，规则将在编译时应用。

格式为 `new D(E)`的*delegate_creation_expression*的绑定时处理，其中 `D` 是一个*delegate_type* ，`E` 是一个*表达式*，包含以下步骤：

*  如果 `E` 是方法组，则将使用与从 `E` 到 `D`的方法[组转换相同](conversions.md#method-group-conversions)的方式来处理委托创建表达式。
*  如果 `E` 是匿名函数，则以与从 `E` 到 `D`的匿名[函数转换相同](conversions.md#anonymous-function-conversions)的方式处理委托创建表达式。
*  如果 `E` 是值，则 `E` 必须与 `D`兼容（[委托声明](delegates.md#delegate-declarations)），并且结果是对类型 `D` 的新创建委托的引用，该委托引用与 `E`相同的调用列表。 如果 `E` 与 `D`不兼容，则会发生编译时错误。

格式为 `new D(E)`的*delegate_creation_expression*的运行时处理，其中 `D` 是一个*delegate_type* ，`E` 是一个*表达式*，它包含以下步骤：

*   如果 `E` 是方法组，则将委托创建表达式作为方法组转换（[方法组](conversions.md#method-group-conversions)转换）作为 `E` `D`进行计算。
*   如果 `E` 是匿名函数，则委托创建将作为从 `E` 到 `D` （[匿名函数转换](conversions.md#anonymous-function-conversions)）的匿名函数转换进行计算。
*   如果 `E` 是*delegate_type*的值：
    * 计算 `E`。 如果此计算导致异常，则不执行其他步骤。
    * 如果 `null``E` 的值，则将引发 `System.NullReferenceException`，并且不执行任何其他步骤。
    * 分配 `D` 委托类型的新实例。 如果内存不足，无法分配新的实例，则会引发 `System.OutOfMemoryException`，而不执行进一步的步骤。
    * 使用与 `E`给定的委托实例相同的调用列表来初始化新的委托实例。

委托的调用列表是在对委托进行实例化时确定的，然后在该委托的整个生存期内保持不变。 换句话说，创建委托后，不能更改其目标可调用的实体。 合并两个委托或从另一个委托（[委托声明](delegates.md#delegate-declarations)）中删除一个委托时，将生成一个新委托;现有委托的内容未发生更改。

不能创建引用属性、索引器、用户定义的运算符、实例构造函数、析构函数或静态构造函数的委托。

如上所述，从方法组创建委托时，委托的形参列表和返回类型确定要选择的重载方法。 示例中
```csharp
delegate double DoubleFunc(double x);

class A
{
    DoubleFunc f = new DoubleFunc(Square);

    static float Square(float x) {
        return x * x;
    }

    static double Square(double x) {
        return x * x;
    }
}
```
使用引用第二个 `Square` 方法的委托初始化 `A.f` 字段，因为该方法与 `DoubleFunc`的形参列表和返回类型完全匹配。 如果第二个 `Square` 方法不存在，则会发生编译时错误。

#### <a name="anonymous-object-creation-expressions"></a>匿名对象创建表达式

*Anonymous_object_creation_expression*用于创建匿名类型的对象。

```antlr
anonymous_object_creation_expression
    : 'new' anonymous_object_initializer
    ;

anonymous_object_initializer
    : '{' member_declarator_list? '}'
    | '{' member_declarator_list ',' '}'
    ;

member_declarator_list
    : member_declarator (',' member_declarator)*
    ;

member_declarator
    : simple_name
    | member_access
    | base_access
    | null_conditional_member_access
    | identifier '=' expression
    ;
```

匿名对象初始值设定项声明匿名类型并返回该类型的实例。 匿名类型是直接从 `object`继承的无类类类型。 匿名类型的成员是从用于创建该类型实例的匿名对象初始值设定项中推断出的只读属性的序列。 具体而言，是窗体的匿名对象初始值设定项
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
声明窗体的匿名类型
```csharp
class __Anonymous1
{
    private readonly T1 f1;
    private readonly T2 f2;
    ...
    private readonly Tn fn;

    public __Anonymous1(T1 a1, T2 a2, ..., Tn an) {
        f1 = a1;
        f2 = a2;
        ...
        fn = an;
    }

    public T1 p1 { get { return f1; } }
    public T2 p2 { get { return f2; } }
    ...
    public Tn pn { get { return fn; } }

    public override bool Equals(object __o) { ... }
    public override int GetHashCode() { ... }
}
```
其中每个 `Tx` 都是对应的表达式 `ex`的类型。 *Member_declarator*中使用的表达式的类型必须为。 因此， *member_declarator*中的表达式为 null 或匿名函数是编译时错误。 这也是表达式具有不安全类型的编译时错误。

匿名类型及其 `Equals` 方法的参数的名称由编译器自动生成，不能在程序文本中引用。

在同一程序中，两个用相同顺序指定相同名称和编译时类型的属性序列的匿名对象初始值设定项将生成相同的匿名类型的实例。

示例中
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
允许在最后一行上进行赋值，因为 `p1` 和 `p2` 是相同的匿名类型。

匿名类型上的 `Equals` 和 `GetHashcode` 方法会重写从 `object`继承的方法，并在属性的 `Equals` 和 `GetHashcode` 中定义，因此，当且仅当所有属性都相等时，相同匿名类型的两个实例才相等。

成员声明符可以缩写为简单名称（[类型推理](expressions.md#type-inference)）、成员访问（[动态重载决策的编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）、基访问（[基本访问](expressions.md#base-access)）或 null 条件成员访问（[null 条件表达式作为投影初始值设定项](expressions.md#null-conditional-expressions-as-projection-initializers)）。 这称为***投影初始值设定项***，是的声明和对具有相同名称的属性赋值的简写形式。 具体而言，是窗体的成员声明符
```csharp
identifier
expr.identifier
```
与以下各项完全等效：
```csharp
identifier = identifier
identifier = expr.identifier
```

因此，在投影初始值设定项中，*标识符*既选择值，又选择要向其分配值的字段或属性。 直观而言，投影初始值设定项不只是一个值，而是值的名称。

### <a name="the-typeof-operator"></a>Typeof 运算符

`typeof` 运算符用于获取类型的 `System.Type` 对象。

```antlr
typeof_expression
    : 'typeof' '(' type ')'
    | 'typeof' '(' unbound_type_name ')'
    | 'typeof' '(' 'void' ')'
    ;

unbound_type_name
    : identifier generic_dimension_specifier?
    | identifier '::' identifier generic_dimension_specifier?
    | unbound_type_name '.' identifier generic_dimension_specifier?
    ;

generic_dimension_specifier
    : '<' comma* '>'
    ;

comma
    : ','
    ;
```

第一种形式的*typeof_expression*由 `typeof` 关键字后跟带括号的*类型*组成。 此窗体的表达式的结果是指定类型的 `System.Type` 对象。 对于任何给定类型，只有一个 `System.Type` 对象。 这意味着对于类型 `T`，`typeof(T) == typeof(T)` 始终为 true。 无法 `dynamic`*类型*。

第二种形式的*typeof_expression*包含一个 `typeof` 关键字，后跟一个带圆括号的*unbound_type_name*。 *Unbound_type_name*与*type_name* （[命名空间和类型名称](basic-concepts.md#namespace-and-type-names)）非常相似，不同之处在于*unbound_type_name*包含*type_name*包含*type_argument_list*的*generic_dimension_specifier*。 当*typeof_expression*的操作数为满足*unbound_type_name*和*type_name*语法的标记序列时，即当它既不包含*generic_dimension_specifier*也不包含*type_argument_list*时，将标记序列视为*type_name*。 确定*unbound_type_name*的含义，如下所示：

*  将标记序列替换为*type_name* ，方法是将每个*generic_dimension_specifier*替换为具有相同的逗号数的*type_argument_list* ，并且关键字 `object` 每个*type_argument*。
*  计算生成的*type_name*，同时忽略所有类型参数约束。
*  *Unbound_type_name*解析为与生成的构造类型（[绑定的和未绑定](types.md#bound-and-unbound-types)的类型）关联的未绑定的泛型类型。

*Typeof_expression*的结果是生成的未绑定泛型类型的 `System.Type` 对象。

第三种形式的*typeof_expression*包含 `typeof` 关键字，后跟带括号的 `void` 关键字。 此形式的表达式的结果是表示缺少类型的 `System.Type` 对象。 `typeof(void)` 返回的类型对象不同于为任何类型返回的类型对象。 此特殊类型对象在允许反射到语言中的方法的类库中很有用，在这种情况下，这些方法希望有一种方法来表示任何方法（包括 void 方法）的返回类型以及 `System.Type`的实例。

可在类型参数上使用 `typeof` 运算符。 结果是绑定到类型参数的运行时类型的 `System.Type` 对象。 `typeof` 运算符还可用于构造类型或未绑定的泛型类型（[绑定类型和未绑定类型](types.md#bound-and-unbound-types)）。 未绑定的泛型类型的 `System.Type` 对象与实例类型的 `System.Type` 对象不同。 实例类型在运行时始终是封闭式构造类型，因此它的 `System.Type` 对象取决于正在使用的运行时类型参数，而未绑定的泛型类型没有类型参数。

示例
```csharp
using System;

class X<T>
{
    public static void PrintTypes() {
        Type[] t = {
            typeof(int),
            typeof(System.Int32),
            typeof(string),
            typeof(double[]),
            typeof(void),
            typeof(T),
            typeof(X<T>),
            typeof(X<X<T>>),
            typeof(X<>)
        };
        for (int i = 0; i < t.Length; i++) {
            Console.WriteLine(t[i]);
        }
    }
}

class Test
{
    static void Main() {
        X<int>.PrintTypes();
    }
}
```
生成以下输出：
```console
System.Int32
System.Int32
System.String
System.Double[]
System.Void
System.Int32
X`1[System.Int32]
X`1[X`1[System.Int32]]
X`1[T]
```

请注意，`int` 和 `System.Int32` 是相同的类型。

另请注意，`typeof(X<>)` 的结果不取决于类型参数，但 `typeof(X<T>)` 的结果。

### <a name="the-checked-and-unchecked-operators"></a>checked 和 unchecked 运算符

`checked` 和 `unchecked` 运算符用于控制整型算术运算和转换的***溢出检查上下文***。

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

`checked` 运算符在已检查的上下文中计算包含的表达式，而 `unchecked` 运算符在未检查的上下文中计算包含的表达式。 *Checked_expression*或*unchecked_expression*完全对应于*parenthesized_expression* （[带括号的表达式](expressions.md#parenthesized-expressions)），只是在给定溢出检查上下文中计算包含的表达式。

还可以通过 `checked` 和 `unchecked` 语句（[checked 和 unchecked 语句](statements.md#the-checked-and-unchecked-statements)）控制溢出检查上下文。

以下操作受 `checked` 和 `unchecked` 运算符和语句所建立的溢出检查上下文的影响：

*  当操作数为整数类型时，预定义的 `++` 和 `--` 一元运算符（[后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators)以及[前缀增量和减量运算符](expressions.md#prefix-increment-and-decrement-operators)）。
*  当操作数为整数类型时，预定义的 `-` 一元运算符（[一元负运算符](expressions.md#unary-minus-operator)）。
*  预定义的 `+`、`-`、`*`和 `/` 二元运算符（[算术运算符](expressions.md#arithmetic-operators)），这两个操作数都是整数类型。
*  显式数值转换（[显式数字转换](conversions.md#explicit-numeric-conversions)）从一个整型类型转换为另一个整型类型，或从 `float` 或 `double` 转换为整型类型。

当上述操作之一产生的结果太大而无法在目标类型中表示时，执行该操作的上下文控制产生的行为：

*  在 `checked` 上下文中，如果操作是常量表达式（[常量表达式](expressions.md#constant-expressions)），则会发生编译时错误。 否则，在运行时执行操作时，将引发 `System.OverflowException`。
*  在 `unchecked` 上下文中，将放弃不适合目标类型的任何高序位，从而截断结果。

对于非常量表达式（在运行时计算的表达式），该表达式未由任何 `checked` 或 `unchecked` 运算符或语句括起来，则默认溢出检查上下文将 `unchecked`，除非 `checked` 计算的外部因素（如编译器开关和执行环境配置）调用。

对于常量表达式（可在编译时完全计算的表达式），默认溢出检查上下文始终 `checked`。 除非在 `unchecked` 上下文中显式放置了一个常数表达式，否则在该表达式的编译时计算期间发生的溢出将始终导致编译时错误。

匿名函数的主体不受 `checked` 或发生匿名函数 `unchecked` 上下文的影响。

示例中
```csharp
class Test
{
    static readonly int x = 1000000;
    static readonly int y = 1000000;

    static int F() {
        return checked(x * y);      // Throws OverflowException
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Depends on default
    }
}
```
不会报告编译时错误，因为在编译时无法计算两个表达式。 在运行时，`F` 方法引发 `System.OverflowException`，`G` 方法返回-727379968 （超出范围的结果的32位）。 `H` 方法的行为取决于编译的默认溢出检查上下文，但它与 `F` 或与 `G`相同。

示例中
```csharp
class Test
{
    const int x = 1000000;
    const int y = 1000000;

    static int F() {
        return checked(x * y);      // Compile error, overflow
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Compile error, overflow
    }
}
```
在对 `F` 和 `H` 中的常量表达式进行计算时所发生的溢出会导致报告编译时错误，因为在 `checked` 上下文中对表达式进行求值。 在 `G`中计算常数表达式时也会发生溢出，但由于在 `unchecked` 上下文中发生了计算，因此不会报告溢出。

`checked` 和 `unchecked` 运算符仅对那些在 "`(`" 和 "`)`" 标记中包含的按 运算符对在计算包含的表达式时被调用的函数成员不起作用。 示例中
```csharp
class Test
{
    static int Multiply(int x, int y) {
        return x * y;
    }

    static int F() {
        return checked(Multiply(1000000, 1000000));
    }
}
```
在 `F` 中使用 `checked` 不会影响 `Multiply`中的 `x * y` 计算，因此在默认溢出检查上下文中对 `x * y` 进行求值。

用十六进制表示法编写带符号整数类型的常量时，`unchecked` 运算符非常方便。 例如：
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

上述两个十六进制常量均为类型 `uint`。 由于常量不在 `int` 范围内（没有 `unchecked` 运算符），因此强制转换为 `int` 会产生编译时错误。

`checked` 和 `unchecked` 运算符和语句使程序员能够控制某些数值计算的某些方面。 但是，某些数值运算符的行为取决于其操作数的数据类型。 例如，两个小数相乘始终导致溢出异常，即使在显式 `unchecked` 的构造中也是如此。 同样，如果在显式 `checked` 的构造中，将两个浮点数相乘会导致溢出异常。 此外，其他运算符永远不会受检查模式（无论是默认的还是显式的）的影响。

### <a name="default-value-expressions"></a>默认值表达式

默认值表达式用于获取类型的默认值（[默认值](variables.md#default-values)）。 通常，默认值表达式用于类型参数，因为如果类型参数是值类型或引用类型，则它可能不是已知的。 （除非已知类型参数是引用类型，否则不存在从 `null` 文本到类型参数的转换。）

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

如果*default_value_expression*中的*类型*在运行时计算为引用类型，则结果将 `null` 转换为该类型。 如果*default_value_expression*中的*类型*在运行时计算为值类型，则结果为*Value_type*的默认值（[默认构造函数](types.md#default-constructors)）。

如果类型是引用类型或已知为引用类型的类型参数（[类型参数约束](classes.md#type-parameter-constraints)），则*default_value_expression*是常量表达式（[常量表达式](expressions.md#constant-expressions)）。 此外，如果该类型为以下值类型之一，则*default_value_expression*为常量表达式： `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`、`bool`或任意枚举类型。


### <a name="nameof-expressions"></a>Nameof 表达式

*Nameof_expression*用于获取作为常量字符串的程序实体的名称。

```antlr
nameof_expression
    : 'nameof' '(' named_entity ')'
    ;

named_entity
    : simple_name
    | named_entity_target '.' identifier type_argument_list?
    ;

named_entity_target
    : 'this'
    | 'base'
    | named_entity 
    | predefined_type 
    | qualified_alias_member
    ;
```

语法上说， *named_entity*操作数始终是表达式。 由于 `nameof` 不是保留关键字，因此，nameof 表达式在语法上始终不明确，因为调用简单名称 `nameof`。 出于兼容性原因，如果名称的名称查找（[简单名称](expressions.md#simple-names)） `nameof` 成功，则无论调用是否合法，都将表达式视为*invocation_expression* 。 否则为*nameof_expression*。

*Nameof_expression*的*named_entity*的含义是表达式的含义;即，作为*simple_name*、 *base_access*或*member_access*。 但是，在[简单名称](expressions.md#simple-names)和[成员访问](expressions.md#member-access)中描述的查找会导致错误，因为实例成员是在静态上下文中找到的，所以*nameof_expression*不产生这样的错误。

如果*named_entity*指定方法组具有*type_argument_list*，则会发生编译时错误。 *Named_entity_target*的类型 `dynamic`为编译时错误。

*Nameof_expression*是 `string`类型的常量表达式，在运行时不起作用。 具体而言，它的*named_entity*不会进行评估，出于明确的赋值分析目的将被忽略（[简单表达式的一般规则](variables.md#general-rules-for-simple-expressions)）。 它的值是可选的最终*type_argument_list*之前*named_entity*的最后一个标识符，转换方式如下：

* 如果使用前缀 "`@`"，则将其删除。
* 每个*unicode_escape_sequence*都转换为其对应的 unicode 字符。
* 删除任何*formatting_characters* 。

当在标识符之间测试相等性时，[标识符](lexical-structure.md#identifiers)中会应用相同的转换。

TODO：示例

### <a name="anonymous-method-expressions"></a>匿名方法表达式

*Anonymous_method_expression*是定义匿名函数的两种方法之一。 [匿名函数表达式](expressions.md#anonymous-function-expressions)中进一步介绍了这些功能。

## <a name="unary-operators"></a>一元运算符

`?`、`+`、`-`、`!`、`~`、`++`、`--`、强制转换和 `await` 运算符称为一元运算符。

```antlr
unary_expression
    : primary_expression
    | null_conditional_expression
    | '+' unary_expression
    | '-' unary_expression
    | '!' unary_expression
    | '~' unary_expression
    | pre_increment_expression
    | pre_decrement_expression
    | cast_expression
    | await_expression
    | unary_expression_unsafe
    ;
```

如果*unary_expression*的操作数 `dynamic`编译时类型，则它是动态绑定的（[动态绑定](expressions.md#dynamic-binding)）。 在这种情况下，将 `dynamic`*unary_expression*的编译时类型，并将使用操作数的运行时类型在运行时进行以下描述的解决方法。

### <a name="null-conditional-operator"></a>Null 条件运算符

仅当操作数为非 null 时，null 条件运算符才将操作列表应用于其操作数。 否则，将 `null`应用运算符的结果。

```antlr
null_conditional_expression
    : primary_expression null_conditional_operations
    ;

null_conditional_operations
    : null_conditional_operations? '?' '.' identifier type_argument_list?
    | null_conditional_operations? '?' '[' argument_list ']'
    | null_conditional_operations '.' identifier type_argument_list?
    | null_conditional_operations '[' argument_list ']'
    | null_conditional_operations '(' argument_list? ')'
    ;
```

操作列表可以包括成员访问和元素访问操作（可能自身为空条件）以及调用。

例如，表达式 `a.b?[0]?.c()` 是一种具有*primary_expression* `a.b` 和*null_conditional_operations* `?[0]` （null 条件元素访问）、`?.c` （null 条件成员访问）和 `()` （调用）的*null_conditional_expression* 。

对于具有*primary_expression* `P`的*null_conditional_expression* `E`，让 `E0` 是通过从每个包含一个的 `?` *null_conditional_operations*中删除前导 `E` 而获得的表达式。 从概念上讲，`E0` 是将计算的表达式，如果 `?`所表示的 null 检查没有找到 `null`，则会计算该表达式。

此外，让 `E1` 是通过从 `E`中的第一个*null_conditional_operations*只删除前导 `?` 来获取的表达式。 这可能会导致*主表达式*（如果只有一个 `?`）或另一个*null_conditional_expression*。

例如，如果 `E` 是表达式 `a.b?[0]?.c()`，则 `E0` 为表达式 `a.b[0].c()`，`E1` 为表达式 `a.b[0]?.c()`。

如果 `E0` 分类为 "无"，则 `E` 分类为 "无"。 否则，将 E 分类为值。

`E0` 和 `E1` 用于确定 `E`的含义：

*  如果 `E` 作为*statement_expression* `E` 的含义与语句相同

   ```csharp
   if ((object)P != null) E1;
   ```

   但 P 只计算一次。

*  否则，如果 `E0` 归类为 nothing，则会发生编译时错误。

*  否则，让 `T0` 成为 `E0`的类型。

   *  如果 `T0` 是未知类型参数，或者是不可以为 null 的值类型，则会发生编译时错误。

   *  如果 `T0` 是不可以为 null 的值类型，则 `E` 的类型 `T0?`，并且 `E` 的含义与

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      但 `P` 只计算一次。

   *  否则，E 的类型为 T0，E 的含义与

      ```csharp
      ((object)P == null) ? null : E1
      ```

      但 `P` 只计算一次。

如果 `E1` 本身就是*null_conditional_expression*，则将再次应用这些规则，并为 `null` 嵌套测试，直到没有更多的 `?`，并且表达式已缩小到主-表达式 `E0`。

例如，如果表达式 `a.b?[0]?.c()` 作为语句表达式出现，如语句中所示：
```csharp
a.b?[0]?.c();
```
其含义等效于：
```csharp
if (a.b != null) a.b[0]?.c();
```
这同样等效于：
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
除了 `a.b` 和 `a.b[0]` 只计算一次。

如果它发生在使用其值的上下文中，如下所示：
```csharp
var x = a.b?[0]?.c();
```
并且假设最终调用的类型不是可以为 null 的值类型，其含义等效于：
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
除了 `a.b` 和 `a.b[0]` 只计算一次。

#### <a name="null-conditional-expressions-as-projection-initializers"></a>空条件表达式作为投影初始值设定项

如果在*anonymous_object_creation_expression* （[匿名对象创建表达式](expressions.md#anonymous-object-creation-expressions)）结束时，只允许使用 null 条件表达式作为*member_declarator* （也可以是 null 条件）成员访问。 语法上，此要求可表示为：

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

这是上述*null_conditional_expression*语法的一种特殊情况。 在[匿名对象创建表达式](expressions.md#anonymous-object-creation-expressions)中*member_declarator*的生产仅包括*null_conditional_member_access*。

#### <a name="null-conditional-expressions-as-statement-expressions"></a>空条件表达式作为语句表达式

如果以 null 条件表达式结束，则仅允许将其作为*statement_expression* （[expression 语句](statements.md#expression-statements)）。 语法上，此要求可表示为：

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

这是上述*null_conditional_expression*语法的一种特殊情况。 然后，[表达式语句](statements.md#expression-statements)中*statement_expression*的生产仅包括*null_conditional_invocation_expression*。


### <a name="unary-plus-operator"></a>一元加运算符

对于 `+x`形式的操作，将应用一元运算符重载决策（[一元运算符重载决策](expressions.md#unary-operator-overload-resolution)）来选择特定的运算符实现。 操作数将转换为所选运算符的参数类型，结果的类型是运算符的返回类型。 预定义的一元加法运算符是：

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

对于每个运算符，结果只是操作数的值。

### <a name="unary-minus-operator"></a>一元减法运算符

对于 `-x`形式的操作，将应用一元运算符重载决策（[一元运算符重载决策](expressions.md#unary-operator-overload-resolution)）来选择特定的运算符实现。 操作数将转换为所选运算符的参数类型，结果的类型是运算符的返回类型。 预定义的求反运算符是：

*  整数取反：

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   通过从零中减去 `x` 来计算结果。 如果 `x` 的值是操作数类型的最小可表示的值（对于 `int` 为-2 ^ 31，对于 `long`，则为-2 ^ 63），则不能在操作数类型内表示 `x` 的数学求反。 如果在 `checked` 上下文中发生此情况，则会引发 `System.OverflowException`;如果它出现在 `unchecked` 上下文内，则结果为操作数的值，并且不报告溢出。

   如果求反运算符的操作数为类型 `uint`，则将其转换为类型 `long`，并 `long`结果的类型。 例外情况是允许将 `int` 值-2147483648 （-2 ^ 31）编写为十进制整数文本（[整数文本](lexical-structure.md#integer-literals)）的规则。

   如果求反运算符的操作数为类型 `ulong`，则会发生编译时错误。 例外情况是允许将 `long` 值-9223372036854775808 （-2 ^ 63）写入为十进制整数文本（[整数文本](lexical-structure.md#integer-literals)）的规则。

*  浮点反：

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   结果为其符号反转 `x` 的值。 如果 `x` 为 NaN，则结果也为 NaN。

*  小数求反：

   ```csharp
   decimal operator -(decimal x);
   ```

   通过从零中减去 `x` 来计算结果。 Decimal 求反等效于使用 `System.Decimal`类型的一元减号运算符。

### <a name="logical-negation-operator"></a>逻辑非运算符

对于 `!x`形式的操作，将应用一元运算符重载决策（[一元运算符重载决策](expressions.md#unary-operator-overload-resolution)）来选择特定的运算符实现。 操作数将转换为所选运算符的参数类型，结果的类型是运算符的返回类型。 只存在一个预定义的逻辑求反运算符：
```csharp
bool operator !(bool x);
```

此运算符计算操作数的逻辑非运算：如果操作数为 `true`，则结果为 `false`。 如果操作数为 `false`，则结果为 `true`。

### <a name="bitwise-complement-operator"></a>按位求补运算符

对于 `~x`形式的操作，将应用一元运算符重载决策（[一元运算符重载决策](expressions.md#unary-operator-overload-resolution)）来选择特定的运算符实现。 操作数将转换为所选运算符的参数类型，结果的类型是运算符的返回类型。 预定义的按位求补运算符是：
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

对于每个运算符，操作的结果是 `x`的按位求补。

每个枚举类型 `E` 隐式提供以下按位求补运算符：

```csharp
E operator ~(E x);
```

计算 `~x`的结果（其中，`x` 是 `E` 具有基础类型 `U`的枚举类型的表达式）与计算 `(E)(~(U)x)`完全相同，不同之处在于，在 `E` 上下文（[检查的和未](expressions.md#the-checked-and-unchecked-operators)检查的运算符）中始终执行到 `unchecked` 的转换。

### <a name="prefix-increment-and-decrement-operators"></a>前缀增量和减量运算符

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

前缀递增或递减运算的操作数必须是分类为变量、属性访问或索引器访问的表达式。 操作的结果是与操作数类型相同的值。

如果前缀递增或递减运算的操作数是属性或索引器访问，则属性或索引器必须同时具有 `get` 和 `set` 访问器。 如果不是这种情况，则会发生绑定时错误。

一元运算符重载决策（[一元运算符重载决策](expressions.md#unary-operator-overload-resolution)）应用于选择特定的运算符实现。 为以下类型存在预定义的 `++` 和 `--` 运算符： `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`和任何枚举类型。 预定义的 `++` 运算符返回通过向操作数添加1而生成的值，预定义的 `--` 运算符返回通过从操作数中减去1而生成的值。 在 `checked` 上下文中，如果此加法或减法的结果超出了结果类型的范围，且结果类型为整型类型或枚举类型，则将引发 `System.OverflowException`。

`++x` 或 `--x` 形式的前缀递增或递减操作的运行时处理包括以下步骤：

*   如果 `x` 归类为变量：
    * 计算 `x` 以产生变量。
    * 将 `x` 所选运算符作为其参数。
    * 运算符返回的值存储在 `x`的计算给定的位置。
    * 运算符返回的值成为操作的结果。
*   如果 `x` 归类为属性或索引器访问：
    * 实例表达式（如果 `x` 不 `static`）和参数列表（如果 `x` 是索引器访问）与 `x` 关联，并在后续的 `get` 和 `set` 访问器调用中使用结果。
    * 调用 `x` 的 `get` 访问器。
    * 使用 `get` 访问器返回的值作为其参数调用所选运算符。
    * 使用运算符作为其 `value` 参数返回的值调用 `x` 的 `set` 访问器。
    * 运算符返回的值成为操作的结果。

`++` 和 `--` 运算符还支持后缀表示法（[后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators)）。 通常，`x++` 或 `x--` 的结果是操作前 `x` 的值，而 `++x` 或 `--x` 的结果是操作后 `x` 的值。 在任一情况下，操作后 `x` 本身具有相同的值。

可以使用后缀或前缀表示法来调用 `operator++` 或 `operator--` 实现。 对于这两个表示法，不能有单独的运算符实现。

### <a name="cast-expressions"></a>强制转换表达式

*Cast_expression*用于将表达式显式转换为给定类型。

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

格式为 `(T)E`的*cast_expression* ，其中 `T` 是一*种类型*，`E` 为*unary_expression*，则执行 `E` 的值到类型 `T`的显式转换（[显式转换](conversions.md#explicit-conversions)）。 如果不存在从 `E` 到 `T`的显式转换，则发生绑定时错误。 否则，结果为显式转换生成的值。 即使 `E` 表示变量，结果也始终归类为值。

*Cast_expression*的语法导致某些语义歧义。 例如，表达式 `(x)-y` 可以解释为*cast_expression* （将 `-y` 强制转换为类型 `x`），或作为*additive_expression*与*parenthesized_expression*组合（这将计算该值 `x - y)`。

若要解决*cast_expression*歧义，请注意以下规则：仅当满足以下条件之一时，括在括号中的一个或多个*标记*为的序列（[空格](lexical-structure.md#white-space)）才被视为*cast_expression*的开头：

*  标记序列对于*类型*是正确的语法，而不是*表达式*的语法。
*  标记序列对于*类型*是正确的语法，紧跟在右括号后面的标记是标记 "`~`"、标记 "`!`"、令牌 "`(`"、*标识符*（[Unicode 字符转义序列](lexical-structure.md#unicode-character-escape-sequences)）、*文本*（[文本](lexical-structure.md#literals)）或除 `as` 和 `is`之外的任何*关键字*（[关键字](lexical-structure.md#keywords)）。

上面的 "更正语法" 一词只是指标记序列必须符合特定的语法生产。 具体而言，它不考虑任何构成标识符的实际含义。 例如，如果 `x` 和 `y` 是标识符，则 `x.y` 是类型的正确语法，即使 `x.y` 实际上不表示类型也是如此。

从消除歧义规则开始，如果 `x` 和 `y` 为标识符、`(x)y`、`(x)(y)`和 `(x)(-y)`，则 cast_expression 为 *`(x)-y`* ，即使 `x` 标识一个类型，也是如此。 但是，如果 `x` 是标识预定义类型（如 `int`）的关键字，则所有四个窗体*cast_expression*s （因为这种关键字本身不可能是表达式）。

### <a name="await-expressions"></a>Await 表达式

Await 运算符用于暂停封闭异步函数的计算，直到操作数表示的异步操作完成。

```antlr
await_expression
    : 'await' unary_expression
    ;
```

只允许在异步函数（[迭代](classes.md#iterators)器）的主体中使用*await_expression* 。 在最近的封闭 async 函数中， *await_expression*可能不会出现在以下位置：

*  嵌套（非异步）匿名函数内
*  在*lock_statement*块内
*  在不安全的上下文中

请注意， *await_expression*不能出现在*query_expression*内的大多数位置，因为这些是在语法上转换为使用非 async lambda 表达式。

在异步函数内部，不能将 `await` 用作标识符。 因此，await 表达式和涉及标识符的各种表达式之间没有语法多义性。 在异步函数的外部，`await` 充当普通标识符。

*Await_expression*的操作数称为***任务***。 它表示一个异步操作，该操作在计算*await_expression*时可能会也可能不会完成。 Await 运算符的用途是在等待的任务完成之前挂起封闭异步函数的执行，然后获取其结果。

#### <a name="awaitable-expressions"></a>可等待表达式

Await 表达式的任务需要***可等待***。 如果以下包含其中一项，则表达式 `t` 为可等待：

*  `t` 的编译时类型为 `dynamic`
*  `t` 具有一个名为 `GetAwaiter` 的可访问实例或扩展方法，其中没有任何参数和类型参数，以及以下所有保留 `A` 的返回类型：
   * `A` 实现接口 `System.Runtime.CompilerServices.INotifyCompletion` （对于简洁起见，为 `INotifyCompletion`）
   * `A` 具有类型的可访问的可读实例属性 `IsCompleted` `bool`
   * `A` 具有可访问的实例方法 `GetResult`，无任何参数和类型参数

`GetAwaiter` 方法的目的是获取任务的***awaiter*** 。 类型 `A` 称为 await 表达式的***awaiter 类型***。

`IsCompleted` 属性的目的是确定任务是否已完成。 如果是这样，则无需暂停评估。

`INotifyCompletion.OnCompleted` 方法的目的是将 "延续" 注册到任务;也就是说，将在任务完成后调用的委托（类型为 `System.Action`）。

`GetResult` 方法的目的是在任务完成后获取任务的结果。 此结果可能会成功完成（可能包含结果值），也可能是由 `GetResult` 方法引发的异常。

#### <a name="classification-of-await-expressions"></a>Await 表达式的分类

表达式 `await t` 的分类方式与 `(t).GetAwaiter().GetResult()`表达式的方式相同。 因此，如果 `GetResult` 的返回类型为 `void`，则*await_expression*分类为 nothing。 如果 `T`，则将*await_expression*分类为类型 `T`的值。

#### <a name="runtime-evaluation-of-await-expressions"></a>Await 表达式的运行时计算

在运行时，表达式 `await t` 的计算方法如下：

*  通过 `(t).GetAwaiter()`计算表达式来获取 awaiter `a`。
*  通过计算表达式 `(a).IsCompleted`获取 `bool` `b`。
*  如果 `false` `b`，则评估将取决于 `a` 是否实现了接口 `System.Runtime.CompilerServices.ICriticalNotifyCompletion` （对于简洁起见，也称为 `ICriticalNotifyCompletion`）。 此检查是在绑定时进行的;例如，如果 `a` 的编译时间类型 `dynamic`，则在运行时，否则为。 让 `r` 表示恢复委托（[迭代](classes.md#iterators)器）：
    * 如果 `a` 未实现 `ICriticalNotifyCompletion`，则计算 `(a as (INotifyCompletion)).OnCompleted(r)` 表达式。
    * 如果 `a` 实现 `ICriticalNotifyCompletion`，则计算 `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` 表达式。
    * 然后，将挂起计算，并将控制权返回给 async 函数的当前调用方。
*  紧随之后（如果 `true``b`），或在以后调用恢复委托（如果 `b` `false`），将计算表达式 `(a).GetResult()`。 如果返回值，则该值是*await_expression*的结果。 否则，结果为 nothing。

Awaiter 实现的接口方法 `INotifyCompletion.OnCompleted` 和 `ICriticalNotifyCompletion.UnsafeOnCompleted` 应导致委托 `r` 最多调用一次。 否则，封闭异步函数的行为是不确定的。

## <a name="arithmetic-operators"></a>算术运算符

`*`、`/`、`%`、`+`和 `-` 运算符称为算术运算符。

```antlr
multiplicative_expression
    : unary_expression
    | multiplicative_expression '*' unary_expression
    | multiplicative_expression '/' unary_expression
    | multiplicative_expression '%' unary_expression
    ;

additive_expression
    : multiplicative_expression
    | additive_expression '+' multiplicative_expression
    | additive_expression '-' multiplicative_expression
    ;
```

如果算术运算符的操作数具有编译时类型 `dynamic`，则该表达式将动态绑定（[动态绑定](expressions.md#dynamic-binding)）。 在这种情况下，将 `dynamic`表达式的编译时类型，并将使用编译时类型为 `dynamic`的操作数的运行时类型在运行时执行下面描述的解决方法。

### <a name="multiplication-operator"></a>乘法运算符

对于 `x * y`形式的操作，将应用二元运算符重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）来选择特定的运算符实现。 操作数将转换为所选运算符的参数类型，结果的类型就是运算符的返回类型。

下面列出了预定义的乘法运算符。 运算符都计算 `x` 和 `y`的产品。

*  整数乘法：

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   在 `checked` 上下文中，如果该产品超出了结果类型的范围，则会引发 `System.OverflowException`。 在 `unchecked` 上下文中，不会报告溢出，并且放弃结果类型范围外的任何重要高序位。


*  浮点乘法：

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   根据 IEEE 754 算法的规则计算产品。 下表列出了非零有限值、零、无穷大和 NaN 的所有可能组合的结果。 在表中，`x` 和 `y` 为正有限值。 `z` 是 `x * y`的结果。 如果结果对于目标类型来说太大，则 `z` 无限大。 如果结果对于目标类型来说太小，则 `z` 为零。

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | +y   | -y   | +0  | -0  | +inf | -inf | NaN | 
   | +x   | +z   | -z   | +0  | -0  | +inf | -inf | NaN | 
   | -x   | -z   | +z   | -0  | +0  | -inf | +inf | NaN | 
   | +0   | +0   | -0   | +0  | -0  | NaN  | NaN  | NaN | 
   | -0   | -0   | +0   | -0  | +0  | NaN  | NaN  | NaN | 
   | +inf | +inf | -inf | NaN | NaN | +inf | -inf | NaN | 
   | -inf | -inf | +inf | NaN | NaN | -inf | +inf | NaN | 
   | NaN  | NaN  | NaN  | NaN | NaN | NaN  | NaN  | NaN | 

*  小数乘法：

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   如果生成的值太大而无法表示为 `decimal` 格式，则会引发 `System.OverflowException`。 如果结果值太小而无法表示为 `decimal` 格式，则结果为零。 在进行任何舍入之前，结果的小数位数是两个操作数的刻度之和。

   Decimal 等效于使用 `System.Decimal`类型的乘法运算符。


### <a name="division-operator"></a>除法运算符

对于 `x / y`形式的操作，将应用二元运算符重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）来选择特定的运算符实现。 操作数将转换为所选运算符的参数类型，结果的类型就是运算符的返回类型。

下面列出了预定义的除法运算符。 运算符都计算 `x` 和 `y`的商。

*  整数相除：

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   如果右操作数的值为零，则会引发 `System.DivideByZeroException`。

   相除将结果舍入到零。 因此，结果的绝对值是小于或等于两个操作数的商的绝对值的最大整数。 如果两个操作数具有相同的符号，则结果为零或正; 如果两个操作数具有相反的符号，则结果为零或负。

   如果左操作数是 `int` 或 `long` 值的最小表示，且右操作数 `-1`，则会发生溢出。 在 `checked` 上下文中，这将导致引发 `System.ArithmeticException` （或其子类）。 在 `unchecked` 上下文中，它的实现定义为：是否引发了 `System.ArithmeticException` （或其中的子类），或者溢出与左操作数的结果值无报告。

*  浮点除法：

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   根据 IEEE 754 算法的规则计算商。 下表列出了非零有限值、零、无穷大和 NaN 的所有可能组合的结果。 在表中，`x` 和 `y` 为正有限值。 `z` 是 `x / y`的结果。 如果结果对于目标类型来说太大，则 `z` 无限大。 如果结果对于目标类型来说太小，则 `z` 为零。

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | +y   | -y   | +0   | -0   | +inf | -inf | NaN  | 
   | +x   | +z   | -z   | +inf | -inf | +0   | -0   | NaN  | 
   | -x   | -z   | +z   | -inf | +inf | -0   | +0   | NaN  | 
   | +0   | +0   | -0   | NaN  | NaN  | +0   | -0   | NaN  | 
   | -0   | -0   | +0   | NaN  | NaN  | -0   | +0   | NaN  | 
   | +inf | +inf | -inf | +inf | -inf | NaN  | NaN  | NaN  | 
   | -inf | -inf | +inf | -inf | +inf | NaN  | NaN  | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  小数部分：

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   如果右操作数的值为零，则会引发 `System.DivideByZeroException`。 如果生成的值太大而无法表示为 `decimal` 格式，则会引发 `System.OverflowException`。 如果结果值太小而无法表示为 `decimal` 格式，则结果为零。 结果的小数位数是最小的小数位数，将结果等于最接近的可表示的十进制值到真正的数学结果。

   小数部分等效于使用 `System.Decimal`类型的除法运算符。


### <a name="remainder-operator"></a>余数运算符

对于 `x % y`形式的操作，将应用二元运算符重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）来选择特定的运算符实现。 操作数将转换为所选运算符的参数类型，结果的类型就是运算符的返回类型。

下面列出了预定义的余数运算符。 运算符都计算 `x` 和 `y`之间的相除余数。

*  整数余数：

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   `x % y` 的结果是 `x - (x / y) * y`生成的值。 如果 `y` 为零，则将引发 `System.DivideByZeroException`。

   如果左操作数是 `int` 或 `long` 值的最小值，且右操作数 `-1`，则将引发 `System.OverflowException`。 在任何情况下，`x % y` 引发异常，`x / y` 不会引发异常。

*  浮点余数：

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   下表列出了非零有限值、零、无穷大和 NaN 的所有可能组合的结果。 在表中，`x` 和 `y` 为正有限值。 `z` 是 `x % y` 的结果，并按 `x - n * y`计算，其中 `n` 是小于或等于 `x / y`的最大整数。 计算余数的这种方法类似于用于整数操作数，但不同于 IEEE 754 定义（其中 `n` 是最接近 `x / y`的整数）。

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | +y   | -y   | +0   | -0   | +inf | -inf | NaN  | 
   | +x   | +z   | +z   | NaN  | NaN  | x    | x    | NaN  | 
   | -x   | -z   | -z   | NaN  | NaN  | -x   | -x   | NaN  | 
   | +0   | +0   | +0   | NaN  | NaN  | +0   | +0   | NaN  | 
   | -0   | -0   | -0   | NaN  | NaN  | -0   | -0   | NaN  | 
   | +inf | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 
   | -inf | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  小数余数：

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   如果右操作数的值为零，则会引发 `System.DivideByZeroException`。 在进行舍入之前，结果的小数位数是两个操作数的刻度中较大的值，并且如果非零，则结果的符号与 `x`相同。

   小数余数等效于使用 `System.Decimal`类型的余数运算符。


### <a name="addition-operator"></a>加法运算符

对于 `x + y`形式的操作，将应用二元运算符重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）来选择特定的运算符实现。 操作数将转换为所选运算符的参数类型，结果的类型就是运算符的返回类型。

下面列出了预定义的加法运算符。 对于数值类型和枚举类型，预定义的加法运算符计算两个操作数之和。 当一个或两个操作数为字符串类型时，预定义的加法运算符将连接操作数的字符串表示形式。

*  整数加法：

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   在 `checked` 上下文中，如果求和超出了结果类型的范围，则会引发 `System.OverflowException`。 在 `unchecked` 上下文中，不会报告溢出，并且放弃结果类型范围外的任何重要高序位。

*  浮点加法：

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   Sum 根据 IEEE 754 算法的规则进行计算。 下表列出了非零有限值、零、无穷大和 NaN 的所有可能组合的结果。 在表中，`x` 和 `y` 是非零的有限值，`z` 是 `x + y`的结果。 如果 `x` 和 `y` 的量相同但符号相反，`z` 为正零。 如果 `x + y` 太大而无法在目标类型中表示，则 `z` 使用与 `x + y`相同的无穷。

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | y    | +0   | -0   | +inf | -inf | NaN  | 
   | x    | z    | x    | x    | +inf | -inf | NaN  | 
   | +0   | y    | +0   | +0   | +inf | -inf | NaN  | 
   | -0   | y    | +0   | -0   | +inf | -inf | NaN  | 
   | +inf | +inf | +inf | +inf | +inf | NaN  | NaN  | 
   | -inf | -inf | -inf | -inf | NaN  | -inf | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  小数加：

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   如果生成的值太大而无法表示为 `decimal` 格式，则会引发 `System.OverflowException`。 在进行舍入之前，结果的小数位数是两个操作数的刻度中较大的一个。

   Decimal 加法等效于使用 `System.Decimal`类型的加法运算符。

*  枚举加法。 每个枚举类型都隐式提供以下预定义运算符，其中 `E` 是枚举类型，`U` 是 `E`的基础类型：

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   在运行时，这些运算符的计算结果与 `(E)((U)x + (U)y)`完全相同。

*  字符串串联：

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   二元 `+` 运算符的这些重载执行字符串串联。 如果 `null`字符串串联的操作数，则将替换空字符串。 否则，通过调用从类型 `object`继承的虚拟 `ToString` 方法，将任何非字符串参数转换为其字符串表示形式。 如果 `ToString` 返回 `null`，则将替换空字符串。

   ```csharp
   using System;
   
   class Test
   {
       static void Main() {
           string s = null;
           Console.WriteLine("s = >" + s + "<");        // displays s = ><
           int i = 1;
           Console.WriteLine("i = " + i);               // displays i = 1
           float f = 1.2300E+15F;
           Console.WriteLine("f = " + f);               // displays f = 1.23E+15
           decimal d = 2.900m;
           Console.WriteLine("d = " + d);               // displays d = 2.900
       }
   }
   ```

   字符串串联运算符的结果是一个字符串，该字符串由左操作数的字符后跟右操作数的字符组成。 字符串串联运算符从不返回 `null` 值。 如果没有足够的内存可用于分配生成的字符串，可能会引发 `System.OutOfMemoryException`。

*  委托组合。 每个委托类型都隐式提供以下预定义运算符，其中 `D` 为委托类型：

   ```csharp
   D operator +(D x, D y);
   ```

   二元 `+` 运算符在两个操作数均为某个委托类型 `D`时执行委托组合。 （如果操作数具有不同的委托类型，则会发生绑定时错误。）如果 `null`第一个操作数，则操作的结果为第二个操作数的值（即使还 `null`）。 否则，如果 `null`第二个操作数，则运算结果为第一个操作数的值。 否则，该操作的结果是一个新的委托实例，它在调用时调用第一个操作数，然后调用第二个操作数。 有关委托组合的示例，请参阅[减法运算符](expressions.md#subtraction-operator)和[委托调用](delegates.md#delegate-invocation)。 由于 `System.Delegate` 不是委托类型，因此不会为其定义 `operator` `+`。

### <a name="subtraction-operator"></a>减法运算符

对于 `x - y`形式的操作，将应用二元运算符重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）来选择特定的运算符实现。 操作数将转换为所选运算符的参数类型，结果的类型就是运算符的返回类型。

下面列出了预定义的减法运算符。 运算符都从 `x`中减去 `y`。

*  整数减法：

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   在 `checked` 上下文中，如果差异超出了结果类型的范围，则会引发 `System.OverflowException`。 在 `unchecked` 上下文中，不会报告溢出，并且放弃结果类型范围外的任何重要高序位。

*  浮点减法：

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   根据 IEEE 754 算法的规则计算差异。 下表列出了非零有限值、零、无穷大和 Nan 的所有可能组合的结果。 在表中，`x` 和 `y` 是非零的有限值，`z` 是 `x - y`的结果。 如果 `x` 和 `y` 相等，`z` 为正零。 如果 `x - y` 太大而无法在目标类型中表示，则 `z` 使用与 `x - y`相同的无穷。

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | y    | +0   | -0   | +inf | -inf | NaN | 
   | x    | z    | x    | x    | -inf | +inf | NaN | 
   | +0   | -y   | +0   | +0   | -inf | +inf | NaN | 
   | -0   | -y   | -0   | +0   | -inf | +inf | NaN | 
   | +inf | +inf | +inf | +inf | NaN  | +inf | NaN | 
   | -inf | -inf | -inf | -inf | -inf | NaN  | NaN | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN | 

*  小数减：

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   如果生成的值太大而无法表示为 `decimal` 格式，则会引发 `System.OverflowException`。 在进行舍入之前，结果的小数位数是两个操作数的刻度中较大的一个。

   Decimal 等效于使用 `System.Decimal`类型的减法运算符。

*  枚举减法。 每个枚举类型都隐式提供以下预定义运算符，其中 `E` 是枚举类型，`U` 是 `E`的基础类型：

   ```csharp
   U operator -(E x, E y);
   ```

   此运算符的计算结果与 `(U)((U)x - (U)y)`完全相同。 换言之，运算符计算 `x` 和 `y`的序号值之间的差异，结果的类型为枚举的基础类型。

   ```csharp
   E operator -(E x, U y);
   ```

   此运算符的计算结果与 `(E)((U)x - y)`完全相同。 换言之，运算符从枚举的基础类型中减去一个值，从而生成一个枚举值。

*  委托删除。 每个委托类型都隐式提供以下预定义运算符，其中 `D` 为委托类型：

   ```csharp
   D operator -(D x, D y);
   ```

   二元 `-` 运算符在两个操作数均为某个委托类型 `D`时执行委托删除。 如果操作数具有不同的委托类型，则会发生绑定时错误。 如果第一个操作数为 `null`，则操作结果为 `null`。 否则，如果 `null`第二个操作数，则运算结果为第一个操作数的值。 否则，两个操作数都表示具有一个或多个项的调用列表（[委托声明](delegates.md#delegate-declarations)），并且结果是一个新的调用列表，其中包含第一个操作数的列表（从中删除第二个操作数的条目），前提是第二个操作数的列表是第一个操作数的正确连续子列表。     （若要确定子列表相等性，请将对应的项与委托相等运算符（[委托相等运算符](expressions.md#delegate-equality-operators)）进行比较。）否则，结果为左操作数的值。 进程中的两个操作数都不是更改的。 如果第二个操作数的列表与第一个操作数列表中连续条目的多个子列表匹配，则会删除最右侧匹配的子列表。 如果删除行为导致出现空列表，则结果为 `null`。 例如：

   ```csharp
   delegate void D(int x);
   
   class C
   {
       public static void M1(int i) { /* ... */ }
       public static void M2(int i) { /* ... */ }
   }

   class Test
   {
       static void Main() { 
           D cd1 = new D(C.M1);
           D cd2 = new D(C.M2);
           D cd3 = cd1 + cd2 + cd2 + cd1;   // M1 + M2 + M2 + M1
           cd3 -= cd1;                      // => M1 + M2 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd2;                // => M2 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd2;                // => M1 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd1;                // => M1 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd1;                // => M1 + M2 + M2 + M1
       }
   }
   ```

## <a name="shift-operators"></a>移位运算符

`<<` 和 `>>` 运算符用于执行移位运算。

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

如果*shift_expression*的操作数 `dynamic`编译时类型，则该表达式将动态绑定（[动态绑定](expressions.md#dynamic-binding)）。 在这种情况下，将 `dynamic`表达式的编译时类型，并将使用编译时类型为 `dynamic`的操作数的运行时类型在运行时执行下面描述的解决方法。

对于 `x << count` 或 `x >> count`形式的操作，将应用二进制运算符重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）以选择特定的运算符实现。 操作数将转换为所选运算符的参数类型，结果的类型就是运算符的返回类型。

在声明重载移位运算符时，第一个操作数的类型必须始终为包含运算符声明的类或结构，并且第二个操作数的类型必须始终 `int`。

下面列出了预定义的移位运算符。

*  左移：

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   `<<` 运算符将 `x` 按如下所述计算的位数向左移动。

   丢弃 `x` 的结果类型范围外的高序位，剩余位将向左移动，并将低序位空位位置设置为零。

*  右移：

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   `>>` 运算符将 `x` 向右移位按下面所述计算的位数。

   当 `x` 的类型为 `int` 或 `long`时，将放弃 `x` 的低序位，并向右移动剩余的位，并将高顺序空位位置设置为零（如果 `x` 为非负值，则设置为1）。

   当 `x` 的类型为 `uint` 或 `ulong`时，将丢弃 `x` 的低序位，其余位将向右移动，而高阶空位位置设置为零。

对于预定义的运算符，将计算要移位的位数，如下所示：

*  当 `x` 的类型 `int` 或 `uint`时，移位计数由 `count`的低序位5位提供。 换句话说，移位计数由 `count & 0x1F`计算得出。
*  当 `x` 的类型 `long` 或 `ulong`时，移位计数由 `count`的低序位六位给定。 换句话说，移位计数由 `count & 0x3F`计算得出。

如果生成的移位计数为零，则移位运算符只返回 `x`的值。

移位运算从不导致溢出，并在 `checked` 和 `unchecked` 上下文中产生相同的结果。

当 `>>` 运算符的左操作数为有符号整数类型时，运算符将执行算术右移，其中操作数的最高有效位（符号位）的值将传播到高阶空位位置。 当 `>>` 运算符的左操作数是无符号整数类型时，运算符将执行逻辑移位，其中高阶空位位置始终设置为零。 若要执行从操作数类型推断得出的相反运算，可以使用显式强制转换。 例如，如果 `x` 是 `int`类型的变量，则运算 `unchecked((int)((uint)x >> y))` 执行 `x`的逻辑向右移位。

## <a name="relational-and-type-testing-operators"></a>关系和类型测试运算符

`==`、`!=`、`<`、`>`、`<=`、`>=`、`is` 和 `as` 运算符称为关系和类型测试运算符。

```antlr
relational_expression
    : shift_expression
    | relational_expression '<' shift_expression
    | relational_expression '>' shift_expression
    | relational_expression '<=' shift_expression
    | relational_expression '>=' shift_expression
    | relational_expression 'is' type
    | relational_expression 'as' type
    ;

equality_expression
    : relational_expression
    | equality_expression '==' relational_expression
    | equality_expression '!=' relational_expression
    ;
```

[Is](expressions.md#the-is-operator)运算符中介绍了 `is` 运算符，并在[as 运算符](expressions.md#the-as-operator)中描述了 `as` 运算符。

`==`、`!=`、`<`、`>`、`<=` 和 `>=` 运算符是***比较运算符***。

如果比较运算符的操作数 `dynamic`编译时类型，则该表达式将动态绑定（[动态绑定](expressions.md#dynamic-binding)）。 在这种情况下，将 `dynamic`表达式的编译时类型，并将使用编译时类型为 `dynamic`的操作数的运行时类型在运行时执行下面描述的解决方法。

对于 `x` *op* `y`形式的操作，其中*op*是比较运算符，将应用重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）以选择特定的运算符实现。 操作数将转换为所选运算符的参数类型，结果的类型就是运算符的返回类型。

以下各节介绍了预定义的比较运算符。 所有预定义的比较运算符都返回 `bool`类型的结果，如下表所述。


| __Operation__ | __结果__                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | `true` 如果 `x` 等于 `y``false`，则为; 否则为。                 | 
| `x != y`      | 如果 `x` 不等于 `y``false`，则 `true`; 否则为。             | 
| `x < y`       | 如果 `x` 小于 `y``false`，则 `true`; 否则为。                | 
| `x > y`       | 如果 `x` 大于 `y``false`，则 `true`; 否则为。             | 
| `x <= y`      | 如果 `x` 小于或等于 `y``false`，则 `true`; 否则为。    | 
| `x >= y`      | 如果 `x` 大于或等于 `y``false`，则 `true`; 否则为。 | 

### <a name="integer-comparison-operators"></a>整数比较运算符

预定义的整数比较运算符是：
```csharp
bool operator ==(int x, int y);
bool operator ==(uint x, uint y);
bool operator ==(long x, long y);
bool operator ==(ulong x, ulong y);

bool operator !=(int x, int y);
bool operator !=(uint x, uint y);
bool operator !=(long x, long y);
bool operator !=(ulong x, ulong y);

bool operator <(int x, int y);
bool operator <(uint x, uint y);
bool operator <(long x, long y);
bool operator <(ulong x, ulong y);

bool operator >(int x, int y);
bool operator >(uint x, uint y);
bool operator >(long x, long y);
bool operator >(ulong x, ulong y);

bool operator <=(int x, int y);
bool operator <=(uint x, uint y);
bool operator <=(long x, long y);
bool operator <=(ulong x, ulong y);

bool operator >=(int x, int y);
bool operator >=(uint x, uint y);
bool operator >=(long x, long y);
bool operator >=(ulong x, ulong y);
```

其中每个运算符比较两个整数操作数的数值，并返回一个 `bool` 值，该值指示是否 `true` 或 `false`特定关系。

### <a name="floating-point-comparison-operators"></a>浮点比较运算符

预定义的浮点比较运算符是：
```csharp
bool operator ==(float x, float y);
bool operator ==(double x, double y);

bool operator !=(float x, float y);
bool operator !=(double x, double y);

bool operator <(float x, float y);
bool operator <(double x, double y);

bool operator >(float x, float y);
bool operator >(double x, double y);

bool operator <=(float x, float y);
bool operator <=(double x, double y);

bool operator >=(float x, float y);
bool operator >=(double x, double y);
```

运算符根据 IEEE 754 标准的规则对操作数进行比较：

*  如果其中一个操作数为 NaN，则结果为 `true`的所有运算符（`!=`除外） `false`。 对于任意两个操作数，`x != y` 始终产生与 `!(x == y)`相同的结果。 但是，当一个或两个操作数为 NaN 时，`<`、`>`、`<=`和 `>=` 运算符不会生成与相对运算符逻辑求反的结果。 例如，如果 `x` 和 `y` 均为 NaN，则 `false``x < y`，但 `!(x >= y)` `true`。
*  当两个操作数均为 NaN 时，运算符比较两个浮点操作数的值与排序相关

   ```csharp
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   其中 `min` 和 `max` 是可以用给定浮点格式表示的最小和最大的正有限值。 此顺序的明显效果如下：
   * 负零和正零被视为相等。
   * 负无穷被视为小于所有其他值，但等于其他负无穷。
   * 正无穷视为大于所有其他值，但等于其他正无穷。

### <a name="decimal-comparison-operators"></a>小数比较运算符

预定义的小数比较运算符是：
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

其中每个运算符比较两个 decimal 操作数的数值，并返回一个 `bool` 值，该值指示是否 `true` 或 `false`特定关系。 每个 decimal 比较等效于使用 `System.Decimal`类型的对应关系或相等运算符。

### <a name="boolean-equality-operators"></a>布尔相等运算符

预定义的布尔相等运算符是：
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

如果 `x` 和 `y` 均 `true`，则 `true` `==` 的结果，或者 `x` 和 `y` 都 `false`。 否则，结果为 `false`。

如果 `x` 和 `y` 均 `true`，则 `false` `!=` 的结果，或者 `x` 和 `y` 都 `false`。 否则，结果为 `true`。 当操作数的类型为 `bool`时，`!=` 运算符产生与 `^` 运算符相同的结果。

### <a name="enumeration-comparison-operators"></a>枚举比较运算符

每个枚举类型都隐式提供以下预定义的比较运算符：
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

计算 `x op y`的结果，其中 `x` 和 `y` 是 `E` 基础类型为的枚举类型的表达式，`U`是比较运算符之一，与评估 `op` 完全相同。 换言之，枚举类型比较运算符只是比较两个操作数的基础整数值。

### <a name="reference-type-equality-operators"></a>引用类型相等运算符

预定义的引用类型相等运算符是：
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

运算符返回比较两个引用是否相等或不相等的结果。

由于预定义的引用类型相等运算符接受 `object`类型的操作数，因此它们适用于未声明适用的 `operator ==` 和 `operator !=` 成员的所有类型。 相反，任何适用的用户定义的相等运算符都可以有效地隐藏预定义的引用类型相等运算符。

预定义的引用类型相等运算符需要以下项之一：

*  两个操作数都是已知为*reference_type*或文本 `null`的类型的值。 而且，显式引用转换（[显式引用转换](conversions.md#explicit-reference-conversions)）是从任一操作数的类型到另一个操作数的类型。
*  一个操作数是类型 `T` 的值，其中 `T` 为*type_parameter* ，另一个操作数为文本 `null`。 此外 `T` 没有值类型约束。

除非满足这些条件之一，否则将发生绑定时错误。 这些规则的明显含义是：

*  使用预定义的引用类型相等运算符比较已知在绑定时不同的两个引用是绑定时错误。 例如，如果操作数的绑定时类型是 `A` 和 `B`的两个类类型，并且 `A` 和 `B` 都不是派生自另一个，则两个操作数不可能引用同一个对象。 因此，操作被视为绑定时错误。
*  预定义的引用类型相等运算符不允许比较值类型操作数。 因此，除非结构类型声明自己的相等运算符，否则不能比较该结构类型的值。
*  预定义的引用类型相等运算符永远不会导致其操作数发生装箱操作。 执行此类装箱操作会毫无意义，因为对新分配的装箱实例的引用必须与所有其他引用不同。
*  如果将类型参数类型 `T` 的操作数与 `null`进行比较，并且 `T` 的运行时类型是值类型，则将 `false`比较的结果。

下面的示例检查是否 `null`不受约束的类型参数类型的参数。
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

即使 `T` 可以表示一个值类型，也允许 `x == null` 构造，并且仅当 `T` 为值类型时，才将结果定义为 `false`。

对于 `x == y` 或 `x != y`形式的操作，如果存在任何适用的 `operator ==` 或 `operator !=`，运算符重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）规则将选择该运算符而不是预定义的引用类型相等运算符。 但是，始终可以通过将一个或两个操作数显式强制转换为类型 `object`来选择预定义的引用类型相等运算符。 示例
```csharp
using System;

class Test
{
    static void Main() {
        string s = "Test";
        string t = string.Copy(s);
        Console.WriteLine(s == t);
        Console.WriteLine((object)s == t);
        Console.WriteLine(s == (object)t);
        Console.WriteLine((object)s == (object)t);
    }
}
```
生成输出
```console
True
False
False
False
```

`s` 和 `t` 变量引用包含相同字符的两个不同的 `string` 实例。 第一个比较输出 `True`，因为当两个操作数为类型 `string`时，将选择预定义的字符串相等运算符（[字符串相等运算符](expressions.md#string-equality-operators)）。 其余的比较 `False` 输出，因为当两个操作数为类型 `object`时，将选择预定义的引用类型相等运算符。

请注意，上述方法对于值类型没有意义。 示例
```csharp
class Test
{
    static void Main() {
        int i = 123;
        int j = 123;
        System.Console.WriteLine((object)i == (object)j);
    }
}
```
输出 `False` 因为强制转换会创建对两个不同的装箱 `int` 值实例的引用。

### <a name="string-equality-operators"></a>字符串相等运算符

预定义的字符串相等运算符是：
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

如果满足以下任一条件，则将两个 `string` 值视为相等：

*  这两个值都是 `null`。
*  对于在每个字符位置都具有相同长度和相同字符的字符串实例，这两个值都是非空引用。

字符串相等运算符比较字符串值而不是字符串引用。 当两个单独的字符串实例包含完全相同的字符序列时，字符串的值相等，但引用不同。 如[引用类型相等运算符](expressions.md#reference-type-equality-operators)中所述，引用类型相等运算符可用于比较字符串引用而不是字符串值。

### <a name="delegate-equality-operators"></a>委托相等运算符

每个委托类型都隐式提供以下预定义的比较运算符：

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

两个委托实例被视为相等，如下所示：

*  如果 `null`的任何一个委托实例，则当且仅当两者都 `null`时，它们才相等。
*  如果委托具有不同的运行时类型，则它们永远不相等。
*  如果两个委托实例都具有调用列表（[委托声明](delegates.md#delegate-declarations)），则当且仅当其调用列表具有相同的长度，并且一个调用列表中的每一项都等于（如下所定义）到另一个调用列表中的相应项时，这些实例相等。

以下规则控制调用列表项的相等性：

*  如果两个调用列表项引用相同的静态方法，则这些项相等。
*  如果两个调用列表项引用相同的目标对象上的相同非静态方法（由引用相等运算符定义），则这些项相等。
*  在语义上完全相同的*anonymous_method_expression*s 或*lambda_expression*s 的计算中，允许（但不要求）具有相同（可能为空）一组捕获的外部变量实例的调用列表项是相等的。

### <a name="equality-operators-and-null"></a>相等运算符和 null

`==` 和 `!=` 运算符允许一个操作数是可以为 null 的类型的值，另一个操作数是 `null` 文本，即使该操作没有预定义或用户定义的运算符（在 unlifted 或提升形式）。

对于某个窗体的操作
```csharp
x == null
null == x
x != null
null != x
```
其中 `x` 是可以为 null 的类型的表达式，如果运算符重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）未能找到适用的运算符，则结果将改为从 `x`的 `HasValue` 属性进行计算。 具体而言，前两个窗体转换为 `!x.HasValue`，最后两个窗体转换为 `x.HasValue`。

### <a name="the-is-operator"></a>Is 运算符

`is` 运算符用于动态检查对象的运行时类型与给定类型是否兼容。 操作的结果 `E is T`，其中 `E` 是一个表达式，而 `T` 是一个类型，它是一个布尔值，指示是否可以通过引用转换、装箱转换或取消装箱转换将 `E` 成功转换为类型 `T`。 在将类型参数替换为所有类型参数后，按如下所示计算运算：

*  如果 `E` 是匿名函数，则会发生编译时错误。
*  如果 `E` 为方法组或 `null` 文本，则如果 `E` 的类型为引用类型或可以为 null 的类型并且 `E` 的值为 null，则结果为 false。
*  否则，让 `D` 表示 `E` 的动态类型，如下所示：
   * 如果 `E` 的类型为引用类型，则 `D` 是 `E`的实例引用的运行时类型。
   * 如果 `E` 的类型是可以为 null 的类型，则 `D` 是可以为 null 的类型的基础类型。
   * 如果 `E` 的类型是不可以为 null 的值类型，则 `D` 是 `E`的类型。
*  操作的结果取决于 `D` 和 `T`，如下所示：
   * 如果 `T` 是引用类型，如果 `D` 和 `T` 为同一类型，则结果为 true; 如果 `D` 是引用类型，并且存在从 `D` 到 `T` 的隐式引用转换，或者 `D` 是一个值类型，并且 `D` 存在从 `T` 到的装箱转换。
   * 如果 `T` 是可以为 null 的类型，则如果 `D` 是 `T`的基础类型，则结果为 true。
   * 如果 `T` 是不可以为 null 的值类型，则如果 `D` 和 `T` 为同一类型，则结果为 true。
   * 否则，结果为 false。

请注意，`is` 运算符不考虑用户定义的转换。

### <a name="the-as-operator"></a>As 运算符

`as` 运算符用于将值显式转换为给定引用类型或可以为 null 的类型。 与强制转换表达式（[强制转换](expressions.md#cast-expressions)表达式）不同，`as` 运算符永远不会引发异常。 相反，如果不可能指定的转换，则将 `null`结果值。

在 `E as T`形式的操作中，`E` 必须是表达式，并且 `T` 必须是引用类型、已知为引用类型的类型参数或可以为 null 的类型。 此外，必须至少满足以下条件之一，否则将发生编译时错误：

*  标识（[标识转换](conversions.md#identity-conversion)）、隐式引用（[隐式](conversions.md#implicit-nullable-conversions)[引用](conversions.md#implicit-reference-conversions)转换）、装箱（[装箱转换](conversions.md#boxing-conversions)）、显式可为 null （[显式](conversions.md#explicit-nullable-conversions)[引用](conversions.md#explicit-reference-conversions)转换）或取消装箱（[取消装箱转换](conversions.md#unboxing-conversions)）转换 `E` 到 `T`中。
*  `E` 类型或 `T` 是开放类型。
*  `E` 是 `null` 文本。

如果未 `dynamic``E` 的编译时类型，则操作 `E as T` 生成的结果与
```csharp
E is T ? (T)(E) : (T)null
```
不同的是 `E` 只计算一次。 编译器可以优化 `E as T` 最多执行一次动态类型检查，而不是上述扩展隐含的两种动态类型检查。

如果 `dynamic``E` 的编译时类型，则与强制转换运算符不同，`as` 运算符不会动态绑定（[动态绑定](expressions.md#dynamic-binding)）。 因此，在这种情况下，展开是：
```csharp
E is T ? (T)(object)(E) : (T)null
```

请注意，某些转换（例如用户定义的转换）无法与 `as` 运算符一起使用，应改为使用强制转换表达式执行。

示例中
```csharp
class X
{

    public string F(object o) {
        return o as string;        // OK, string is a reference type
    }

    public T G<T>(object o) where T: Attribute {
        return o as T;             // Ok, T has a class constraint
    }

    public U H<U>(object o) {
        return o as U;             // Error, U is unconstrained 
    }
}
```
`G` 的类型参数 `T` 称为引用类型，因为它具有类约束。 但 `H` 的类型参数 `U` 不是;因此，不允许在 `H` 中使用 `as` 运算符。

## <a name="logical-operators"></a>逻辑运算符

`&`、`^`和 `|` 运算符称为逻辑运算符。

```antlr
and_expression
    : equality_expression
    | and_expression '&' equality_expression
    ;

exclusive_or_expression
    : and_expression
    | exclusive_or_expression '^' and_expression
    ;

inclusive_or_expression
    : exclusive_or_expression
    | inclusive_or_expression '|' exclusive_or_expression
    ;
```

如果逻辑运算符的操作数具有编译时类型 `dynamic`，则该表达式将动态绑定（[动态绑定](expressions.md#dynamic-binding)）。 在这种情况下，将 `dynamic`表达式的编译时类型，并将使用编译时类型为 `dynamic`的操作数的运行时类型在运行时执行下面描述的解决方法。

对于 `x op y`形式的操作，其中 `op` 为逻辑运算符之一，将应用重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）来选择特定的运算符实现。 操作数将转换为所选运算符的参数类型，结果的类型就是运算符的返回类型。

以下各节介绍了预定义的逻辑运算符。

### <a name="integer-logical-operators"></a>整数逻辑运算符

预定义的整数逻辑运算符为：
```csharp
int operator &(int x, int y);
uint operator &(uint x, uint y);
long operator &(long x, long y);
ulong operator &(ulong x, ulong y);

int operator |(int x, int y);
uint operator |(uint x, uint y);
long operator |(long x, long y);
ulong operator |(ulong x, ulong y);

int operator ^(int x, int y);
uint operator ^(uint x, uint y);
long operator ^(long x, long y);
ulong operator ^(ulong x, ulong y);
```

`&` 运算符计算两个操作数的按位逻辑 `AND`，`|` 运算符计算两个操作数的按位逻辑 `OR`，而 `^` 运算符计算两个操作数的按位逻辑独占 `OR`。 这些操作不能有溢出。

### <a name="enumeration-logical-operators"></a>枚举逻辑运算符

每个枚举类型 `E` 隐式提供以下预定义的逻辑运算符：

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

计算 `x op y`的结果，其中 `x` 和 `y` 是 `E` 基础类型为的枚举类型的表达式，`U`是逻辑运算符之一，与评估 `op` 完全相同。 换言之，枚举类型逻辑运算符只对两个操作数的基础类型执行逻辑运算。

### <a name="boolean-logical-operators"></a>Boolean 逻辑运算符

预定义的布尔逻辑运算符为：
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

如果 `x` 和 `y` 都为 `true`，则 `x & y` 的结果为 `true`。 否则，结果为 `false`。

如果 `x` 或 `y` 为 `true`，则 `true` `x | y` 的结果。 否则，结果为 `false`。

如果 `x` 为 `true`，`y` 为 `false`，或 `x` `false` `y` `true`，则会 `true` `x ^ y` 的结果。 否则，结果为 `false`。 当操作数为类型 `bool`时，`^` 运算符将计算与 `!=` 运算符相同的结果。

### <a name="nullable-boolean-logical-operators"></a>可以为 null 的布尔逻辑运算符

可以为 null 的布尔类型 `bool?` 可以表示三个值： `true`、`false`和 `null`，在概念上类似于用于 SQL 中的布尔表达式的三值类型。 为了确保 `bool?` 操作数的 `&` 和 `|` 运算符产生的结果与 SQL 的三值逻辑一致，提供了以下预定义运算符：

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

下表列出了这些运算符为 `true`、`false`和 `null`的所有值组合所产生的结果。

| `x`     | `y`     | `x & y` | <code>x &#124; y</code> |
|:-------:|:-------:|:-------:|:-------:|
| `true`  | `true`  | `true`  | `true`  | 
| `true`  | `false` | `false` | `true`  | 
| `true`  | `null`  | `null`  | `true`  | 
| `false` | `true`  | `false` | `true`  | 
| `false` | `false` | `false` | `false` | 
| `false` | `null`  | `false` | `null`  | 
| `null`  | `true`  | `null`  | `true`  | 
| `null`  | `false` | `false` | `null`  | 
| `null`  | `null`  | `null`  | `null`  | 

## <a name="conditional-logical-operators"></a>条件逻辑运算符

`&&` 和 `||` 运算符称为条件逻辑运算符。 它们也称为 "短路" 逻辑运算符。

```antlr
conditional_and_expression
    : inclusive_or_expression
    | conditional_and_expression '&&' inclusive_or_expression
    ;

conditional_or_expression
    : conditional_and_expression
    | conditional_or_expression '||' conditional_and_expression
    ;
```

`&&` 和 `||` 运算符是 `&` 和 `|` 运算符的条件版本：

*  操作 `x && y` 对应于操作 `x & y`，只不过仅当 `x` 未 `false`时才会计算 `y`。
*  操作 `x || y` 对应于操作 `x | y`，只不过仅当 `x` 未 `true`时才会计算 `y`。

如果条件逻辑运算符的操作数具有编译时类型 `dynamic`，则该表达式将动态绑定（[动态绑定](expressions.md#dynamic-binding)）。 在这种情况下，将 `dynamic`表达式的编译时类型，并将使用编译时类型为 `dynamic`的操作数的运行时类型在运行时执行下面描述的解决方法。

`x && y` 或 `x || y` 形式的操作通过应用重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）来处理，就像该操作是 `x & y` 还是 `x | y`编写的。 那么：

*  如果重载决策未能找到单个最佳运算符，或者重载决策选择预定义的整数逻辑运算符之一，则会发生绑定时错误。
*  否则，如果所选运算符是预定义的布尔逻辑运算符之一（[布尔逻辑运算符](expressions.md#boolean-logical-operators)）或可以为 null 的布尔逻辑运算符（[可为 null](expressions.md#nullable-boolean-logical-operators)的布尔逻辑运算符），则按照[布尔条件逻辑运算符](expressions.md#boolean-conditional-logical-operators)中所述处理操作。
*  否则，所选运算符是用户定义的运算符，并且按照[用户定义的条件逻辑运算符](expressions.md#user-defined-conditional-logical-operators)中的描述处理操作。

不能直接重载条件逻辑运算符。 但是，因为条件逻辑运算符是按常规逻辑运算符计算的，所以常规逻辑运算符的重载在某些限制条件下也被视为条件逻辑运算符的重载。 [用户定义的条件逻辑运算符](expressions.md#user-defined-conditional-logical-operators)中对此进行了进一步说明。

### <a name="boolean-conditional-logical-operators"></a>布尔条件逻辑运算符

如果 `&&` 或 `||` 的操作数的类型为 `bool`，或者当操作数的类型没有定义适用的 `operator &` 或 `operator |`，但确实定义了到 `bool`的隐式转换时，将按如下所示处理操作：

*  运算 `x && y` 的计算结果为 `x ? y : false`。 换句话说，`x` 首先计算并转换为类型 `bool`。 然后，如果 `true``x`，则计算 `y`，并将其转换为类型 `bool`，这就成为了运算结果。 否则，将 `false`操作的结果。
*  运算 `x || y` 的计算结果为 `x ? true : y`。 换句话说，`x` 首先计算并转换为类型 `bool`。 然后，如果 `true``x`，则将 `true`操作的结果。 否则，将计算 `y`，并将其转换为类型 `bool`，这就成为了运算结果。

### <a name="user-defined-conditional-logical-operators"></a>用户定义的条件逻辑运算符

如果 `&&` 或 `||` 的操作数是声明适用的用户定义 `operator &` 或 `operator |`的类型，则以下两项都必须为 true，其中 `T` 是声明所选运算符的类型：

*  所选运算符的每个参数的返回类型和类型必须是 `T`。 换言之，运算符必须计算 `T`类型的两个操作数的逻辑 `AND` 或逻辑 `OR`，并且必须返回 `T`类型的结果。
*  `T` 必须包含 `operator true` 和 `operator false`的声明。

如果不满足上述任一要求，则会发生绑定时错误。 否则，通过将用户定义的 `operator true` 或 `operator false` 与选定的用户定义运算符相结合来计算 `&&` 或 `||` 操作：

*  运算 `x && y` 的计算结果为 `T.false(x) ? x : T.&(x, y)`，其中 `T.false(x)` 是在 `T`中声明的 `operator false` 的调用，`T.&(x, y)` 是所选 `operator &`的调用。 换而言之，首先计算 `x`，并对结果调用 `operator false`，以确定 `x` 是否确实为 false。 然后，如果 `x` 肯定为 false，则操作的结果是之前为 `x`计算的值。 否则，将对 `y` 进行计算，并对之前为 `x` 计算的值调用所选 `operator &`，并为 `y` 生成操作结果的值。
*  运算 `x || y` 的计算结果为 `T.true(x) ? x : T.|(x, y)`，其中 `T.true(x)` 是在 `T`中声明的 `operator true` 的调用，`T.|(x,y)` 是所选 `operator|`的调用。 换言之，首先计算 `x` 并对结果调用 `operator true`，以确定 `x` 是否确实为 true。 然后，如果 `x` 确实为 true，则操作的结果是之前为 `x`计算的值。 否则，将对 `y` 进行计算，并对之前为 `x` 计算的值调用所选 `operator |`，并为 `y` 生成操作结果的值。

在上述任一操作中，由 `x` 给定的表达式只计算一次，并且 `y` 指定的表达式不会进行任何计算或只计算一次。

有关实现 `operator true` 和 `operator false`的类型的示例，请参阅[数据库布尔类型](structs.md#database-boolean-type)。

## <a name="the-null-coalescing-operator"></a>Null 合并运算符

`??` 运算符称为 null 合并运算符。

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

格式 `a ?? b` 的 null 合并表达式需要 `a` 为可以为 null 的类型或引用类型。 如果 `a` 为非 null，则 `a ?? b` 的结果 `a`;否则，结果为 `b`。 仅当 `a` 为 null 时，操作才计算 `b`。

Null 合并运算符是右结合运算符，这意味着运算从右到左分组。 例如，形式 `a ?? b ?? c` 的表达式的计算结果为 `a ?? (b ?? c)`。 一般来说，形式 `E1 ?? E2 ?? ... ?? En` 的表达式返回非 null 的第一个操作数，如果所有操作数为 null，则返回 null。

`a ?? b` 表达式的类型取决于在操作数上可用的隐式转换。 按照优先顺序，`a ?? b` 的类型为 `A0`、`A`或 `B`，其中 `A` 是 `a` 的类型（前提是 `a` 具有类型），`B` 是 `b` 的类型（前提是 `b` 有一个类型），`A0` 是 `A` 的基础类型（如果 `A` 是可以为 null 的类型），或者 `A` 为。 具体而言，按如下所示处理 `a ?? b`：

*  如果 `A` 存在，并且不是可以为 null 的类型或引用类型，则会发生编译时错误。
*  如果 `b` 是动态表达式，则结果类型为 `dynamic`。 在运行时，首先计算 `a`。 如果 `a` 不为 null，则 `a` 转换为动态，这将成为结果。 否则，将计算 `b`，这就是结果。
*  否则，如果 `A` 存在并且是可以为 null 的类型并且存在从 `b` 到 `A0`的隐式转换，则结果类型为 `A0`。 在运行时，首先计算 `a`。 如果 `a` 不为 null，`a` 将解包为类型 `A0`，这就成为了结果。 否则，将计算 `b`，并将其转换为类型 `A0`，这就成为了结果。
*  否则，如果 `A` 存在，而从 `b` 到 `A`存在隐式转换，则结果类型为 `A`。 在运行时，首先计算 `a`。 如果 `a` 不为 null，则 `a` 会成为结果。 否则，将计算 `b`，并将其转换为类型 `A`，这就成为了结果。
*  否则，如果 `b` 具有类型 `B` 并且存在从 `a` 到 `B`的隐式转换，则结果类型为 `B`。 在运行时，首先计算 `a`。 如果 `a` 不为 null，则 `a` 将解包到类型 `A0` （如果 `A` 存在并且可为 null），并转换为类型 `B`，这就成为了结果。 否则，将计算 `b` 并使其成为结果。
*  否则，`a` 和 `b` 不兼容，并发生编译时错误。

## <a name="conditional-operator"></a>条件运算符

`?:` 运算符称为条件运算符。 有时也称为三元运算符。

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

`b ? x : y` 首先计算 `b`的条件表达式。 然后，如果 `true``b`，则将计算 `x`，并成为操作的结果。 否则，将计算 `y`，并成为操作的结果。 条件表达式从不同时计算 `x` 和 `y`。

条件运算符是右结合运算符，这意味着运算从右到左分组。 例如，形式 `a ? b : c ? d : e` 的表达式的计算结果为 `a ? b : (c ? d : e)`。

`?:` 运算符的第一个操作数必须是可以隐式转换为 `bool`的表达式，或者是实现 `operator true`的类型的表达式。 如果满足这两个要求，则会发生编译时错误。

`?:` 运算符的第二个和第三个操作数 `x` 和 `y`控制条件表达式的类型。

*  如果 `x` 具有类型 `X` 并且 `y` 具有类型 `Y` 则
   * 如果 `X` 中存在隐式转换（[隐式转换](conversions.md#implicit-conversions)）到 `Y`，但不是从 `Y` 到 `X`，则 `Y` 是条件表达式的类型。
   * 如果 `Y` 中存在隐式转换（[隐式转换](conversions.md#implicit-conversions)）到 `X`，但不是从 `X` 到 `Y`，则 `X` 是条件表达式的类型。
   * 否则，不能确定表达式类型，并且会发生编译时错误。
*  如果 `x` 和 `y` 中只有一个具有类型，并且的 `x` 和 `y`都可以隐式转换为该类型，则这是条件表达式的类型。
*  否则，不能确定表达式类型，并且会发生编译时错误。

格式 `b ? x : y` 的条件表达式的运行时处理包括以下步骤：

*  首先，将计算 `b`，并确定 `b` 的 `bool` 值：
   * 如果从 `b` 类型到 `bool` 存在隐式转换，则执行此隐式转换以生成 `bool` 值。
   * 否则，将调用 `b` 类型定义的 `operator true` 以生成 `bool` 值。
*  如果上面的步骤生成的 `bool` 值为 `true`，则计算 `x`，并将其转换为条件表达式的类型，这就成为条件表达式的结果。
*  否则，将计算 `y`，并将其转换为条件表达式的类型，这将成为条件表达式的结果。

## <a name="anonymous-function-expressions"></a>匿名函数表达式

***匿名函数***是表示 "内联" 方法定义的表达式。 匿名函数本身不具有值或类型，但可以转换为兼容的委托或表达式树类型。 匿名函数转换的计算取决于转换的目标类型：如果该类型为委托类型，则转换将计算为引用匿名函数定义的方法的委托值。 如果该类型为表达式树类型，则转换为表达式树，该树将方法的结构表示为对象结构。

由于历史原因，匿名函数有两种语法风格，即*lambda_expression*s 和*anonymous_method_expression*s。 几乎所有情况下， *lambda_expression*比*anonymous_method_expression*的更简洁、更具表现力，这仍然是用于向后兼容的语言。

```antlr
lambda_expression
    : anonymous_function_signature '=>' anonymous_function_body
    ;

anonymous_method_expression
    : 'delegate' explicit_anonymous_function_signature? block
    ;

anonymous_function_signature
    : explicit_anonymous_function_signature
    | implicit_anonymous_function_signature
    ;

explicit_anonymous_function_signature
    : '(' explicit_anonymous_function_parameter_list? ')'
    ;

explicit_anonymous_function_parameter_list
    : explicit_anonymous_function_parameter (',' explicit_anonymous_function_parameter)*
    ;

explicit_anonymous_function_parameter
    : anonymous_function_parameter_modifier? type identifier
    ;

anonymous_function_parameter_modifier
    : 'ref'
    | 'out'
    ;

implicit_anonymous_function_signature
    : '(' implicit_anonymous_function_parameter_list? ')'
    | implicit_anonymous_function_parameter
    ;

implicit_anonymous_function_parameter_list
    : implicit_anonymous_function_parameter (',' implicit_anonymous_function_parameter)*
    ;

implicit_anonymous_function_parameter
    : identifier
    ;

anonymous_function_body
    : expression
    | block
    ;
```

`=>` 运算符具有与赋值（`=`）相同的优先级，并且是右结合运算符。

带有 `async` 修饰符的匿名函数是一个异步函数，并遵循[迭代](classes.md#iterators)器中所述的规则。

可以显式或隐式类型地*lambda_expression*形式的匿名函数的参数。 在显式类型化参数列表中，每个参数的类型都是显式声明的。 在隐式类型的参数列表中，参数的类型是从发生匿名函数的上下文中推断出来的：具体而言，当匿名函数转换为兼容的委托类型或表达式目录树类型时，该类型提供参数类型（[匿名函数转换](conversions.md#anonymous-function-conversions)）。

在具有单个隐式类型化参数的匿名函数中，可以从参数列表中省略括号。 换句话说，形式的匿名函数
```csharp
( param ) => expr
```
可以缩写为
```csharp
param => expr
```

*Anonymous_method_expression*格式的匿名函数的参数列表是可选的。 如果给定，则必须显式键入参数。 如果不是，则该匿名函数可转换为包含任何参数列表（不包含 `out` 参数）的委托。

匿名函数的*块*体是可访问的（[终结点和可访问](statements.md#end-points-and-reachability)性），除非匿名函数出现在无法访问的语句中。

下面是匿名函数的一些示例：

```csharp
x => x + 1                              // Implicitly typed, expression body
x => { return x + 1; }                  // Implicitly typed, statement body
(int x) => x + 1                        // Explicitly typed, expression body
(int x) => { return x + 1; }            // Explicitly typed, statement body
(x, y) => x * y                         // Multiple parameters
() => Console.WriteLine()               // No parameters
async (t1,t2) => await t1 + await t2    // Async
delegate (int x) { return x + 1; }      // Anonymous method expression
delegate { return 1 + 1; }              // Parameter list omitted
```

*Lambda_expression*和*anonymous_method_expression*s 的行为是相同的，但以下情况除外：

*  *anonymous_method_expression*允许完全省略参数列表，则可以 convertibility 委托任何值参数列表的类型。
*  *lambda_expression*允许省略和推断参数类型，而*anonymous_method_expression*则需要显式声明参数类型。
*  *Lambda_expression*的主体可以是表达式或语句块，而*anonymous_method_expression*的主体必须是语句块。
*  只有*lambda_expression*s 具有转换为兼容的表达式树类型（[表达式树类型](types.md#expression-tree-types)）。

### <a name="anonymous-function-signatures"></a>匿名函数签名

匿名函数的可选*anonymous_function_signature*定义匿名函数的名称和可选的形参类型。 匿名函数的参数的作用域是*anonymous_function_body*。 （[范围](basic-concepts.md#scopes)）与参数列表（如果给定）一起构成了声明空间（[声明](basic-concepts.md#declarations)）。 因此，匿名函数的参数名称的编译时错误与本地变量、本地常量或参数的名称（其范围包含*anonymous_method_expression*或*lambda_expression*）相匹配。

如果匿名函数有*explicit_anonymous_function_signature*，则兼容的委托类型集和表达式树类型将被限制为具有相同顺序的相同参数类型和修饰符的类型。 与方法组转换（[方法组转换](conversions.md#method-group-conversions)）相比，匿名函数参数类型的差异不受支持。 如果匿名函数没有*anonymous_function_signature*，则兼容的委托类型集和表达式树类型将被限制为没有 `out` 参数的类型。

请注意， *anonymous_function_signature*不能包含特性或参数数组。 尽管如此， *anonymous_function_signature*可能与其参数列表包含参数数组的委托类型兼容。

另请注意，即使兼容，转换为表达式树类型也可能仍会在编译时（[表达式树类型](types.md#expression-tree-types)）失败。

### <a name="anonymous-function-bodies"></a>匿名函数体

匿名函数的主体（*表达式*或*块*）将遵循以下规则：

*  如果匿名函数包含签名，则在签名中指定的参数在正文中可用。 如果匿名函数没有签名，则可以将其转换为具有参数的委托类型或表达式类型（[匿名函数转换](conversions.md#anonymous-function-conversions)），但不能在正文中访问这些参数。
*  除了在最近的封闭匿名函数的签名（如果有）中指定的 `ref` 或 `out` 参数外，主体访问 `ref` 或 `out` 参数的编译时错误。
*  当 `this` 的类型为结构类型时，主体访问 `this`时会发生编译时错误。 无论访问是显式的（如 `this.x`）还是隐式的（如在 `x` （其中 `x` 是结构的实例成员），都是如此。 此规则只是禁止此类访问，不会影响成员查找是否会导致结构的成员。
*  主体有权访问匿名函数的外部变量（[外部变量](expressions.md#outer-variables)）。 外部变量的访问将引用在计算*lambda_expression*或*anonymous_method_expression*时处于活动状态的变量实例（[匿名函数表达式的计算](expressions.md#evaluation-of-anonymous-function-expressions)）。
*  如果主体包含的 `goto` 语句、`break` 语句或 `continue` 语句的目标在正文外或包含的匿名函数的主体中，则它是编译时错误。
*  正文中的 `return` 语句从最近的封闭匿名函数（而不是来自封闭函数成员）的调用返回控制权。 `return` 语句中指定的表达式必须可隐式转换为最接近*lambda_expression*或*anonymous_method_expression*转换为的委托类型或表达式树类型的返回类型（[匿名函数转换](conversions.md#anonymous-function-conversions)）。

明确指出，无论是否有任何方法可以执行匿名函数的块，而不是通过计算和调用*lambda_expression*或*anonymous_method_expression*。 特别是，编译器可以通过综合一个或多个命名方法或类型来选择实现匿名函数。 任何此类合成元素的名称必须是保留供编译器使用的形式。

### <a name="overload-resolution-and-anonymous-functions"></a>重载决策和匿名函数

自变量列表中的匿名函数参与类型推理和重载解析。 请参阅[类型推理](expressions.md#type-inference)和[重载决策](expressions.md#overload-resolution)，了解确切的规则。

下面的示例演示匿名函数对重载决策的影响。

```csharp
class ItemList<T>: List<T>
{
    public int Sum(Func<T,int> selector) {
        int sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }

    public double Sum(Func<T,double> selector) {
        double sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }
}
```

`ItemList<T>` 类有两个 `Sum` 方法。 每个都采用 `selector` 参数，该参数从列表项中提取要求和的值。 提取的值可以是 `int` 或 `double`，所得的总和也可以是 `int` 或 `double`。

例如，`Sum` 方法可用于计算按订单列出的详细信息行的总和。

```csharp
class Detail
{
    public int UnitCount;
    public double UnitPrice;
    ...
}

void ComputeSums() {
    ItemList<Detail> orderDetails = GetOrderDetails(...);
    int totalUnits = orderDetails.Sum(d => d.UnitCount);
    double orderTotal = orderDetails.Sum(d => d.UnitPrice * d.UnitCount);
    ...
}
```

在第一次调用 `orderDetails.Sum`中，这两个 `Sum` 方法都适用，因为匿名函数 `d => d. UnitCount` 与 `Func<Detail,int>` 和 `Func<Detail,double>`都兼容。 但重载决策将选取第一个 `Sum` 方法，因为转换到 `Func<Detail,int>` 比转换到 `Func<Detail,double>`好。

在第二次调用 `orderDetails.Sum`中，只会使用第二个 `Sum` 方法，因为匿名函数 `d => d.UnitPrice * d.UnitCount` 生成类型 `double`的值。 因此，重载决策为该调用选择第二个 `Sum` 方法。

### <a name="anonymous-functions-and-dynamic-binding"></a>匿名函数和动态绑定

匿名函数不能是动态绑定操作的接收方、参数或操作数。

### <a name="outer-variables"></a>外部变量

任何本地变量、值参数或其范围包含*lambda_expression*或*anonymous_method_expression*的参数数组称为匿名函数的***外部变量***。 在类的实例函数成员中，`this` 值被视为值参数，并且是函数成员内包含的任何匿名函数的外部变量。

#### <a name="captured-outer-variables"></a>捕获的外部变量

当某个外部变量被匿名函数引用时，该外部变量被视为已被匿名函数***捕获***。 通常，局部变量的生存期仅限于执行它所关联的块或语句（[局部变量](variables.md#local-variables)）。 但是，已捕获的外部变量的生存期至少会进行扩展，直到从匿名函数创建的委托或表达式树变为可进行垃圾回收的条件。

示例中
```csharp
using System;

delegate int D();

class Test
{
    static D F() {
        int x = 0;
        D result = () => ++x;
        return result;
    }

    static void Main() {
        D d = F();
        Console.WriteLine(d());
        Console.WriteLine(d());
        Console.WriteLine(d());
    }
}
```
局部变量 `x` 由匿名函数捕获，并且 `x` 的生存期至少会进行扩展，直到从 `F` 返回的委托符合垃圾回收条件（这在程序的最末尾之前不会发生）。 由于匿名函数的每个调用都在 `x`的同一个实例上运行，因此该示例的输出为：
```console
1
2
3
```

当匿名函数捕获本地变量或值参数时，本地变量或参数不再被视为固定变量（[固定](unsafe-code.md#fixed-and-moveable-variables)变量），而是被视为可移动的变量。 因此，任何采用已捕获外部变量地址的 `unsafe` 代码都必须首先使用 `fixed` 语句来修复该变量。

请注意，与 uncaptured 变量不同，捕获的局部变量可以同时公开给多个执行线程。

#### <a name="instantiation-of-local-variables"></a>局部变量的实例化

当执行进入变量的作用域时，本地变量被视为被***实例化***。 例如，在调用以下方法时，本地变量 `x` 将实例化并初始化三次，一次针对循环的每次迭代。

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

但是，在循环外移动 `x` 的声明会导致 `x`的单个实例化：
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

如果未捕获，则无法准确观察局部变量的实例化频率（因为实例化的生存期是不连续的），因此每个实例化都可以只使用相同的存储位置。 但是，当匿名函数捕获本地变量时，实例化的效果会变得很明显。

示例
```csharp
using System;

delegate void D();

class Test
{
    static D[] F() {
        D[] result = new D[3];
        for (int i = 0; i < 3; i++) {
            int x = i * 2 + 1;
            result[i] = () => { Console.WriteLine(x); };
        }
        return result;
    }

    static void Main() {
        foreach (D d in F()) d();
    }
}
```
生成输出：
```console
1
3
5
```

但是，当 `x` 的声明移到循环外时：
```csharp
static D[] F() {
    D[] result = new D[3];
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        result[i] = () => { Console.WriteLine(x); };
    }
    return result;
}
```
输出是：
```console
5
5
5
```

如果 for 循环声明迭代变量，则该变量本身被视为在循环外部声明。 因此，如果将该示例更改为捕获迭代变量自身：

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
只捕获迭代变量的一个实例，这将生成输出：
```console
3
3
3
```

匿名函数委托可以共享一些捕获的变量，但其他实例却有单独的实例。 例如，如果 `F` 更改为
```csharp
static D[] F() {
    D[] result = new D[3];
    int x = 0;
    for (int i = 0; i < 3; i++) {
        int y = 0;
        result[i] = () => { Console.WriteLine("{0} {1}", ++x, ++y); };
    }
    return result;
}
```
这三个委托捕获 `x` 的同一实例，但 `y`的单独实例，输出为：
```console
1 1
2 1
3 1
```

单独的匿名函数可以捕获外部变量的同一个实例。 在下面的示例中：
```csharp
using System;

delegate void Setter(int value);

delegate int Getter();

class Test
{
    static void Main() {
        int x = 0;
        Setter s = (int value) => { x = value; };
        Getter g = () => { return x; };
        s(5);
        Console.WriteLine(g());
        s(10);
        Console.WriteLine(g());
    }
}
```
这两个匿名函数 `x`捕获本地变量的同一个实例，因此它们可以通过该变量 "进行通信"。 该示例的输出为：
```console
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a>匿名函数表达式的计算

匿名函数 `F` 必须始终转换为委托类型 `D` 或 `E`（直接或通过执行委托创建表达式 `new D(F)`）的表达式树类型。 此转换确定匿名函数的结果，如[匿名函数转换](conversions.md#anonymous-function-conversions)中所述。

## <a name="query-expressions"></a>查询表达式

***查询表达式***为类似于关系和分层查询语言（如 SQL 和 XQuery）的查询提供了语言集成语法。

```antlr
query_expression
    : from_clause query_body
    ;

from_clause
    : 'from' type? identifier 'in' expression
    ;

query_body
    : query_body_clauses? select_or_group_clause query_continuation?
    ;

query_body_clauses
    : query_body_clause
    | query_body_clauses query_body_clause
    ;

query_body_clause
    : from_clause
    | let_clause
    | where_clause
    | join_clause
    | join_into_clause
    | orderby_clause
    ;

let_clause
    : 'let' identifier '=' expression
    ;

where_clause
    : 'where' boolean_expression
    ;

join_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression
    ;

join_into_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression 'into' identifier
    ;

orderby_clause
    : 'orderby' orderings
    ;

orderings
    : ordering (',' ordering)*
    ;

ordering
    : expression ordering_direction?
    ;

ordering_direction
    : 'ascending'
    | 'descending'
    ;

select_or_group_clause
    : select_clause
    | group_clause
    ;

select_clause
    : 'select' expression
    ;

group_clause
    : 'group' expression 'by' expression
    ;

query_continuation
    : 'into' identifier query_body
    ;
```

查询表达式以 `from` 子句开头，以 `select` 或 `group` 子句结束。 初始 `from` 子句后面可以跟零个或多个 `from`、`let`、`where`、`join` 或 `orderby` 子句。 每个 `from` 子句都是引入一个***范围变量***的生成器，该变量范围是***序列***的元素。 每个 `let` 子句引入一个范围变量，表示通过以前的范围变量计算得出的值。 每个 `where` 子句是一个从结果中排除项的筛选器。 每个 `join` 子句将源序列的指定键与另一个序列的键进行比较，从而生成匹配对。 每个 `orderby` 子句会根据指定的条件对项进行重新排序。Final `select` 或 `group` 子句根据范围变量指定结果的形状。 最后，`into` 子句可用于将一个查询的结果作为一个生成器在后续查询中处理。

### <a name="ambiguities-in-query-expressions"></a>查询表达式中的多义性

查询表达式包含许多 "上下文关键字"，即在给定上下文中具有特殊意义的标识符。 具体来说，这些是 `from`、`where`、`join`、`on`、`equals`、`into`、`let`、`orderby`、`ascending`、`descending`、`select`、`group` 和 `by`。 为了避免因混合使用这些标识符作为关键字或简单名称而导致的查询表达式中出现歧义，在查询表达式中的任何位置出现时，这些标识符都被视为关键字。

为此，查询表达式是以 "`from identifier`" 开头的任何表达式，后跟除 "`;`"、"`=`" 或 "`,`" 之外的任何标记。

为了在查询表达式中使用这些字词作为标识符，可以使用 "`@`" （[标识符](lexical-structure.md#identifiers)）作为前缀。

### <a name="query-expression-translation"></a>查询表达式转换

该C#语言不指定查询表达式的执行语义。 相反，查询表达式被转换为符合*查询表达式模式*（[查询表达式模式](expressions.md#the-query-expression-pattern)）的方法调用。 具体而言，将查询表达式转换为名为 `Where`、`Select`、`SelectMany`、`Join`、`GroupJoin`、`OrderBy`、`OrderByDescending`、`ThenBy`、`ThenByDescending`、`GroupBy`和 `Cast`的方法调用。这些方法应具有特定的签名和结果类型，如[查询表达式模式](expressions.md#the-query-expression-pattern)中所述。 这些方法可以是要查询的对象的实例方法或对象外部的扩展方法，并实现查询的实际执行。

从查询表达式转换为方法调用是在执行任何类型绑定或重载决策之前发生的语法映射。 转换确保语法正确，但不保证产生语义正确C#的代码。 转换查询表达式后，生成的方法调用将作为常规方法调用进行处理，这可能会导致错误（例如，如果方法不存在，如果参数具有错误的类型），或者如果方法是泛型，则类型推理失败。

通过重复应用以下翻译来处理查询表达式，直到不能进一步缩减。 翻译按照应用程序顺序列出：每个部分都假设之前部分中的翻译已进行了详尽的操作，一旦完成，就不会再在处理同一查询表达式时再次使用部分。

不允许在查询表达式中分配范围变量。 但允许C#实现并非始终强制实施此限制，因为有时可能不会使用此处提供的语法转换方案。

某些翻译注入范围变量，其中包含由 `*`表示的透明标识符。 [透明标识符中进一步](expressions.md#transparent-identifiers)讨论了透明标识符的特殊属性。

#### <a name="select-and-groupby-clauses-with-continuations"></a>带有延续的 Select 和 groupby 子句

具有延续的查询表达式
```csharp
from ... into x ...
```
转换为
```csharp
from x in ( from ... ) ...
```

以下各部分中的翻译假设查询没有 `into` 的继续符。

示例
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
转换为
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
的最终转换是
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a>显式范围变量类型

显式指定范围变量类型的 `from` 子句
```csharp
from T x in e
```
转换为
```csharp
from x in ( e ) . Cast < T > ( )
```

显式指定范围变量类型的 `join` 子句
```csharp
join T x in e on k1 equals k2
```
转换为
```csharp
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

以下部分中的翻译假设查询没有显式范围变量类型。

示例
```csharp
from Customer c in customers
where c.City == "London"
select c
```
转换为
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
的最终转换是
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

显式范围变量类型可用于查询实现非泛型 `IEnumerable` 接口的集合，但不能用于泛型 `IEnumerable<T>` 接口。 在上述示例中，如果 `customers` 的类型 `ArrayList`，就会出现这种情况。

#### <a name="degenerate-query-expressions"></a>退化查询表达式

格式为的查询表达式
```csharp
from x in e select x
```
转换为
```csharp
( e ) . Select ( x => x )
```

示例
```csharp
from c in customers
select c
```
转换为
```csharp
customers.Select(c => c)
```

退化查询表达式是完全选择源中的元素的表达式。 转换的更晚阶段会删除其他翻译步骤引入的退化查询，方法是将其替换为其源。 但要确保查询表达式的结果绝不是源对象本身，这一点很重要，因为这会将源的类型和标识显示到查询的客户端。 因此，此步骤通过显式调用源 `Select` 来保护直接在源代码中编写的退化查询。 然后由 `Select` 和其他查询运算符的实施者来确保这些方法从不返回源对象本身。

#### <a name="from-let-where-join-and-orderby-clauses"></a>From、let、where、join 和 orderby 子句

包含第二个 `from` 子句后跟 `select` 子句的查询表达式
```csharp
from x1 in e1
from x2 in e2
select v
```
转换为
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

一个查询表达式，该表达式的第二个 `from` 子句后跟除 `select` 子句之外的其他内容：

```csharp
from x1 in e1
from x2 in e2
...
```
转换为
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

带有 `let` 子句的查询表达式
```csharp
from x in e
let y = f
...
```
转换为
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

带有 `where` 子句的查询表达式
```csharp
from x in e
where f
...
```
转换为
```csharp
from x in ( e ) . Where ( x => f )
...
```

带有 `join` 子句且不带 `into` 的查询表达式，后面跟有 `select` 子句
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
转换为
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

一个查询表达式，其中的 `join` 子句没有 `into`，后面跟有 `select` 子句以外的其他内容
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
转换为
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

带有 `into` 后跟 `select` 子句的 `join` 子句的查询表达式
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
转换为
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

包含 `into` 后跟除 `select` 子句之外的 `join` 子句的查询表达式
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
转换为
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

带有 `orderby` 子句的查询表达式
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
转换为
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

如果 order 子句指定 `descending` 方向指示器，则改为生成 `OrderByDescending` 或 `ThenByDescending` 的调用。

以下翻译假设每个查询表达式中没有 `let`、`where`、`join` 或 `orderby` 子句，且不超过一个初始 `from` 子句。

示例
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
转换为
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

示例
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
转换为
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
的最终转换是
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
其中 `x` 是编译器生成的标识符，否则不可见且不可访问。

示例
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
转换为
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
的最终转换是
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
其中 `x` 是编译器生成的标识符，否则不可见且不可访问。

示例
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
转换为
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

示例
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
转换为
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
的最终转换是
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
其中 `x` 和 `y` 是编译器生成的标识符，否则不可见且不可访问。

示例
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
具有最终翻译
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a>Select 子句

格式为的查询表达式
```csharp
from x in e select v
```
转换为
```csharp
( e ) . Select ( x => v )
```
除了 v 是标识符 x 时，转换只是
```csharp
( e )
```

例如
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
只转换为
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a>Groupby 子句

格式为的查询表达式
```csharp
from x in e group v by k
```
转换为
```csharp
( e ) . GroupBy ( x => k , x => v )
```
除非 v 是标识符 x，否则转换为
```csharp
( e ) . GroupBy ( x => k )
```

示例
```csharp
from c in customers
group c.Name by c.Country
```
转换为
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a>透明标识符

某些翻译注入范围变量，其中包含由 `*`表示的***透明标识符***。 透明标识符不是正确的语言功能;它们仅作为查询表达式转换过程中的一个中间步骤存在。

当查询转换注入透明标识符时，进一步的转换步骤会将透明标识符传播到匿名函数和匿名对象初始值设定项。 在这些上下文中，透明标识符具有以下行为：

*  当透明标识符作为匿名函数中的参数出现时，关联的匿名类型的成员将自动出现在匿名函数主体的范围内。
*  当具有透明标识符的成员位于范围内时，该成员的成员也会在范围内。
*  当透明标识符作为匿名对象初始值设定项中的成员声明符出现时，它会引入一个具有透明标识符的成员。
*  在上面所述的翻译步骤中，透明标识符始终与匿名类型一起引入，目的是将多个范围变量捕获为单个对象的成员。 允许实现C#使用与匿名类型不同的机制将多个范围变量组合在一起。 以下翻译示例假设使用匿名类型，并演示如何将透明标识符转换为。

示例
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
转换为
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

这会进一步转换为
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
当清除透明标识符时，它等效于
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
其中 `x` 是编译器生成的标识符，否则不可见且不可访问。

示例
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
转换为
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
这会进一步减小到
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
的最终转换是
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c, o }).
Join(details, x => x.o.OrderID, d => d.OrderID,
    (x, d) => new { x, d }).
Join(products, y => y.d.ProductID, p => p.ProductID,
    (y, p) => new { y, p }).
Select(z => new { z.y.x.c.Name, z.y.x.o.OrderDate, z.p.ProductName })
```
其中 `x`、`y`和 `z` 是编译器生成的标识符，否则不可见且不可访问。

### <a name="the-query-expression-pattern"></a>查询表达式模式

***查询表达式模式***建立了一种方法，这些方法可实现类型以支持查询表达式。 由于查询表达式是通过语法映射转换为方法调用的，因此在实现查询表达式模式的方式上，类型具有相当大的灵活性。 例如，可以将模式的方法作为实例方法或扩展方法实现，因为这两个方法具有相同的调用语法，并且方法可以请求委托或表达式树，因为匿名函数可同时转换为两者。

下面显示了支持查询表达式模式的泛型类型 `C<T>` 的推荐形状。 使用泛型类型来说明参数和结果类型之间的正确关系，但也可以实现非泛型类型的模式。

```csharp
delegate R Func<T1,R>(T1 arg1);

delegate R Func<T1,T2,R>(T1 arg1, T2 arg2);

class C
{
    public C<T> Cast<T>();
}

class C<T> : C
{
    public C<T> Where(Func<T,bool> predicate);

    public C<U> Select<U>(Func<T,U> selector);

    public C<V> SelectMany<U,V>(Func<T,C<U>> selector,
        Func<T,U,V> resultSelector);

    public C<V> Join<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,U,V> resultSelector);

    public C<V> GroupJoin<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,C<U>,V> resultSelector);

    public O<T> OrderBy<K>(Func<T,K> keySelector);

    public O<T> OrderByDescending<K>(Func<T,K> keySelector);

    public C<G<K,T>> GroupBy<K>(Func<T,K> keySelector);

    public C<G<K,E>> GroupBy<K,E>(Func<T,K> keySelector,
        Func<T,E> elementSelector);
}

class O<T> : C<T>
{
    public O<T> ThenBy<K>(Func<T,K> keySelector);

    public O<T> ThenByDescending<K>(Func<T,K> keySelector);
}

class G<K,T> : C<T>
{
    public K Key { get; }
}
```

上述方法使用 `Func<T1,R>` 和 `Func<T1,T2,R>`的泛型委托类型，但它们同样可以使用参数和结果类型中具有相同关系的其他委托或表达式树类型。

请注意 `C<T>` 和 `O<T>` 之间的推荐关系，这可确保 `ThenBy` 和 `ThenByDescending` 方法仅对 `OrderBy` 或 `OrderByDescending`的结果可用。 另请注意 `GroupBy` 结果的建议形状（序列序列），其中每个内部序列都有附加的 `Key` 属性。

`System.Linq` 命名空间为实现 `System.Collections.Generic.IEnumerable<T>` 接口的任何类型提供查询运算符模式的实现。

## <a name="assignment-operators"></a>赋值运算符

赋值运算符将新值分配给变量、属性、事件或索引器元素。

```antlr
assignment
    : unary_expression assignment_operator expression
    ;

assignment_operator
    : '='
    | '+='
    | '-='
    | '*='
    | '/='
    | '%='
    | '&='
    | '|='
    | '^='
    | '<<='
    | right_shift_assignment
    ;
```

赋值运算的左操作数必须是分类为变量、属性访问、索引器访问或事件访问的表达式。

`=` 运算符称为***简单赋值运算符***。 它将右操作数的值分配给左操作数给定的变量、属性或索引器元素。 简单赋值运算符的左操作数不能是事件访问（如[类似于字段的事件](classes.md#field-like-events)中所述）。 简单赋值运算符在[简单赋值](expressions.md#simple-assignment)中进行了介绍。

除 `=` 运算符以外的赋值运算符称为***复合赋值运算符***。 这些运算符对两个操作数执行指定的运算，然后将结果值分配给左操作数给定的变量、属性或索引器元素。 复合赋值运算符在[复合赋值](expressions.md#compound-assignment)中介绍。

使用事件访问表达式作为左操作数的 `+=` 和 `-=` 运算符称为*事件赋值运算符*。 其他赋值运算符对于作为左操作数的事件访问无效。 事件赋值运算符在[事件分配](expressions.md#event-assignment)中介绍。

赋值运算符是右结合运算符，这意味着运算从右到左分组。 例如，形式 `a = b = c` 的表达式的计算结果为 `a = (b = c)`。

### <a name="simple-assignment"></a>简单赋值

`=` 运算符称为简单赋值运算符。

如果简单赋值的左操作数的形式为 `E.P` 或 `E[Ei]` `E` 具有编译时类型 `dynamic`，则分配是动态绑定的（[动态绑定](expressions.md#dynamic-binding)）。 在这种情况下，赋值表达式的编译时类型是 `dynamic`的，并且基于 `E`的运行时类型，将在运行时进行以下描述的解决方法。

在简单赋值中，右操作数必须是可隐式转换为左操作数类型的表达式。 操作将右操作数的值分配给左操作数给定的变量、属性或索引器元素。

简单赋值表达式的结果是赋给左操作数的值。 结果与左操作数的类型相同，并且始终归类为值。

如果左操作数是属性或索引器访问，则属性或索引器必须具有 `set` 访问器。 如果不是这种情况，则会发生绑定时错误。

对形式 `x = y` 的简单赋值处理的运行时处理包括以下步骤：

*  如果 `x` 归类为变量：
   * 计算 `x` 以产生变量。
   * 计算 `y`，并在需要时通过隐式转换转换为 `x` 的类型（[隐式转换](conversions.md#implicit-conversions)）。
   * 如果 `x` 给定的变量是*reference_type*的数组元素，则执行运行时检查，以确保为 `y` 计算的值与 `x` 为元素的数组实例兼容。 如果 `y` `null`，或者存在从 `y` 引用的实例的实际类型到包含 `x`的数组实例的实际元素类型的隐式引用转换（[隐式引用转换](conversions.md#implicit-reference-conversions)），则检查成功。 否则，将会引发 `System.ArrayTypeMismatchException`。
   * `y` 的计算和转换生成的值将存储到 `x`计算得出的位置。
*  如果 `x` 归类为属性或索引器访问：
   * 实例表达式（如果 `x` 不 `static`）和参数列表（如果 `x` 是索引器访问）与 `x` 关联，并在后续的 `set` 访问器调用中使用结果。
   * 计算 `y`，并在需要时通过隐式转换转换为 `x` 的类型（[隐式转换](conversions.md#implicit-conversions)）。
   * 调用 `x` 的 `set` 访问器时，会将计算的值 `y` 为其 `value` 参数。

如果存在从 `B` 到 `A`的隐式引用转换，数组协方差规则（[数组协方](arrays.md#array-covariance)差）允许 `A[]` 为数组类型 `B[]`的实例的引用。 由于这些规则，对*reference_type*的数组元素赋值需要运行时检查，以确保所赋的值与数组实例兼容。 示例中
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
最后一个分配导致引发 `System.ArrayTypeMismatchException`，因为 `ArrayList` 的实例无法存储在 `string[]`的元素中。

如果在*struct_type*中声明的属性或索引器是赋值目标，则必须将与属性或索引器访问关联的实例表达式归类为变量。 如果实例表达式归类为值，则会发生绑定时错误。 由于[成员访问](expressions.md#member-access)，相同的规则也适用于字段。

给定以下声明：
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int X {
        get { return x; }
        set { x = value; }
    }

    public int Y {
        get { return y; }
        set { y = value; }
    }
}

struct Rectangle
{
    Point a, b;

    public Rectangle(Point a, Point b) {
        this.a = a;
        this.b = b;
    }

    public Point A {
        get { return a; }
        set { a = value; }
    }

    public Point B {
        get { return b; }
        set { b = value; }
    }
}
```
示例中
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
允许分配到 `p.X`、`p.Y`、`r.A`和 `r.B`，因为 `p` 和 `r` 都是变量。 但在示例中
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
分配均无效，因为 `r.A` 和 `r.B` 不是变量。

### <a name="compound-assignment"></a>复合赋值

如果复合赋值的左操作数的形式为 `E.P` 或 `E[Ei]` `E` 具有编译时类型 `dynamic`，则分配是动态绑定的（[动态绑定](expressions.md#dynamic-binding)）。 在这种情况下，赋值表达式的编译时类型是 `dynamic`的，并且基于 `E`的运行时类型，将在运行时进行以下描述的解决方法。

通过应用二元运算符重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）来处理窗体 `x op= y` 的操作，就像将操作写入 `x op y`一样。 那么：

*  如果所选运算符的返回类型可隐式转换为 `x`的类型，则该运算将计算为 `x = x op y`，只不过 `x` 只计算一次。
*  否则，如果所选运算符是预定义的运算符，则如果所选运算符的返回类型可显式转换为 `x`的类型，并且如果 `y` 可隐式转换为 `x` 类型或运算符为移位运算符，则该运算将计算为 `x = (T)(x op y)`，其中 `T` 是 `x`的类型，但 `x` 只计算一次。
*  否则，复合分配无效，并发生绑定时错误。

术语 "仅计算一次" 表示在 `x op y`的计算中，将临时保存 `x` 的所有构成表达式的结果，并在对 `x`执行赋值时重用。 例如，在赋值 `A()[B()] += C()`中，其中 `A` 是返回 `int[]`的方法，并且 `B` 和 `C` 是返回 `int`的方法，这些方法只调用一次，并按顺序 `A``B`、`C`。

当复合赋值的左操作数是属性访问或索引器访问时，属性或索引器必须同时具有 `get` 访问器和 `set` 访问器。 如果不是这种情况，则会发生绑定时错误。

上面的第二个规则允许 `x op= y` 在某些上下文中计算为 `x = (T)(x op y)`。 存在该规则，以便当左操作数的类型为 `sbyte`、`byte`、`short`、`ushort`或 `char`时，预定义运算符可用作复合运算符。 即使两个参数都属于这些类型之一，预定义运算符也会生成 `int`类型的结果，如[二进制数值升级](expressions.md#binary-numeric-promotions)中所述。 因此，在没有强制转换的情况下，不能将结果赋给左操作数。

预定义运算符规则的直观效果只是允许 `x op y` 和 `x = y` 都允许 `x op= y`。 示例中
```csharp
byte b = 0;
char ch = '\0';
int i = 0;

b += 1;             // Ok
b += 1000;          // Error, b = 1000 not permitted
b += i;             // Error, b = i not permitted
b += (byte)i;       // Ok

ch += 1;            // Error, ch = 1 not permitted
ch += (char)1;      // Ok
```
每个错误的直观原因在于，相应的简单分配也会导致错误。

这也意味着复合赋值操作支持提升的操作。 示例中
```csharp
int? i = 0;
i += 1;             // Ok
```
使用提升运算符 `+(int?,int?)`。

### <a name="event-assignment"></a>事件分配

如果 `+=` 或 `-=` 运算符的左操作数分类为事件访问，则表达式的计算方式如下：

*  计算事件访问的实例表达式（如果有）。
*  计算 `+=` 或 `-=` 运算符的右操作数，并在需要时通过隐式转换转换为左操作数的类型（[隐式](conversions.md#implicit-conversions)转换）。
*  调用事件的事件访问器，其中包含右操作数（在计算后，如有必要）转换后的参数列表。 如果运算符是 `+=`的，则调用 `add` 访问器;如果运算符是 `-=`的，则调用 `remove` 访问器。

事件赋值表达式不生成值。 因此，事件分配表达式仅在*statement_expression* （[expression 语句](statements.md#expression-statements)）的上下文中有效。

## <a name="expression"></a>表达式

*表达式*可以是*non_assignment_expression*或*赋值*。

```antlr
expression
    : non_assignment_expression
    | assignment
    ;

non_assignment_expression
    : conditional_expression
    | lambda_expression
    | query_expression
    ;
```

## <a name="constant-expressions"></a>常量表达式

*Constant_expression*是可以在编译时完全计算的表达式。

```antlr
constant_expression
    : expression
    ;
```

常数表达式必须是 `null` 文本或具有以下类型之一的值： `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`、`bool`、`object`、`string`或任何枚举类型。 常量表达式中仅允许使用以下构造：

*  文本（包括 `null` 文本）。
*  对类类型和结构类型 `const` 成员的引用。
*  对枚举类型的成员的引用。
*  对 `const` 参数或局部变量的引用
*  带括号的子表达式，它们本身就是常量表达式。
*  如果目标类型是上面列出的类型之一，则强制转换表达式。
*  `checked` 和 `unchecked` 表达式
*  默认值表达式
*  Nameof 表达式
*  预定义的 `+`、`-`、`!`和 `~` 一元运算符。
*  预定义的 `+`、`-`、`*`、`/`、`%`、`<<`、`>>`、`&`、`|`、`^`、`&&`、`||`、`==`、`!=`、`<`、`>`、`<=`和 `>=` 二进制运算符，前提是每个操作数都是上面列出的类型。
*  `?:` 条件运算符。

常数表达式中允许以下转换：

*  标识转换
*  数值转换
*  枚举转换
*  常数表达式转换
*  隐式和显式引用转换，前提是转换的源是计算结果为 null 值的常量表达式。

不允许在常数表达式中使用其他转换，包括非 null 值的装箱、取消装箱和隐式引用转换。 例如：
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
i 的初始化是错误的，因为需要装箱转换。 Str 的初始化是错误的，因为需要从非 null 值进行隐式引用转换。

只要表达式满足上面列出的要求，就会在编译时计算表达式。 即使表达式是包含非常量构造的更大的表达式的子表达式，也是如此。

常数表达式的编译时计算使用与非常量表达式的运行时计算相同的规则，只不过运行时计算会引发异常，编译时计算会导致发生编译时错误。

除非将常量表达式显式置于 `unchecked` 的上下文中，否则在表达式的编译时计算过程中发生的溢出将始终导致编译时错误（[常量表达式](expressions.md#constant-expressions)）。

常数表达式出现在下面列出的上下文中。 在这些上下文中，如果表达式在编译时无法完全计算，则会发生编译时错误。

*  常量声明（[常量](classes.md#constants)）。
*  枚举成员声明（[枚举成员](enums.md#enum-members)）。
*  形参表的默认参数（[方法参数](classes.md#method-parameters)）
*  `switch` 语句 `case` 标签（[switch 语句](statements.md#the-switch-statement)）。
*  `goto case` 语句（[goto 语句](statements.md#the-goto-statement)）。
*  数组创建表达式中的维度长度（[数组创建表达式](expressions.md#array-creation-expressions)），其中包含初始值设定项。
*  特性（[特性](attributes.md)）。

如果常量表达式的值在目标类型的范围内，则隐式常量表达式转换（[隐式常数表达式转换](conversions.md#implicit-constant-expression-conversions)）允许 `int` 类型的常量表达式转换为 `sbyte`、`byte`、`short`、`ushort`、`uint`或 `ulong`。

## <a name="boolean-expressions"></a>Boolean 表达式

*Boolean_expression*是生成 `bool`类型的结果的表达式;直接或通过在某些上下文中 `operator true` 应用程序，如以下所示。

```antlr
boolean_expression
    : expression
    ;
```

*If_statement* （[if 语句](statements.md#the-if-statement)）、 *while_statement* （[while 语句](statements.md#the-while-statement)）、 *do_statement* （[do 语句](statements.md#the-do-statement)）或*for_statement* （[for 语句](statements.md#the-for-statement)）的控制条件表达式为*boolean_expression*。 `?:` 运算符（[条件运算符](expressions.md#conditional-operator)）的控制条件表达式遵循与*boolean_expression*相同的规则，但由于运算符优先级的原因，将归类为*conditional_or_expression*。

需要*boolean_expression* `E` 才能生成类型 `bool`的值，如下所示：

*  如果 `E` 可隐式转换为 `bool` 则在运行时将应用隐式转换。
*  否则，一元运算符重载决策（[一元运算符重载决策](expressions.md#unary-operator-overload-resolution)）用于查找 `E`上运算符 `true` 的唯一最佳实现，并且该实现在运行时应用。
*  如果找不到这样的运算符，则发生绑定时错误。

[数据库布尔类型](structs.md#database-boolean-type)中 `DBBool` 结构类型提供了实现 `operator true` 和 `operator false`的类型的示例。
