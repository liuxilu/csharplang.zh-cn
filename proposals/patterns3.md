---
ms.openlocfilehash: 57cdb682efd4cb169308e347d63e27c97b9f1b7a
ms.sourcegitcommit: 6901635c383801e4d177085587aaccadaa7b2f11
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641960"
---
# <a name="pattern-matching-changes-for-c-90"></a>C# 9.0 的模式匹配更改

我们正在考虑对 C# 9.0 的模式匹配进行少量增强，这些增强具有天然的协同效应，并很好地解决了许多常见的编程问题：
- https://github.com/dotnet/csharplang/issues/2925类型模式
- https://github.com/dotnet/csharplang/issues/1350括号调整的模式，以强制执行或强调新组合器的优先级
- https://github.com/dotnet/csharplang/issues/1350需要两种不同`and`模式匹配的构成模式;
- https://github.com/dotnet/csharplang/issues/1350需要匹配两种`or`不同模式之一的分离模式;
- https://github.com/dotnet/csharplang/issues/1350需要给定`not`模式*不匹配*的否定模式;和
- https://github.com/dotnet/csharplang/issues/812要求输入值小于、小于或等于给定常量等的关系模式。

## <a name="parenthesized-patterns"></a>括号模式

括号模式允许程序员将括号放在任何模式周围。  这在 C# 8.0 中的现有模式中没有那么有用，但是新的模式组合器引入了程序员可能想要重写的优先级。

```antlr
primary_pattern
    : parenthesized_pattern
    | // all of the existing forms
    ;
parenthesized_pattern
    : '(' pattern ')'
    ;
```

## <a name="type-patterns"></a>类型模式

我们允许类型作为模式：

``` antlr
primary_pattern
    : type-pattern
    | // all of the existing forms
    ;
type_pattern
    : type
    ;
```

这会使现有的*is-type 表达式*作为一个模式模式为*类型模式**的正模式表达式*，尽管我们不会更改编译器生成的语法树。

一个微妙的实现问题是，这个语法是模糊的。  可以将 （`a.b`如）的字符串解析为限定名称（在类型上下文中）或虚线表达式（在表达式上下文中）。  编译器已经能够处理与虚线表达式相同的限定名称，以便处理类似`e is Color.Red`之类的内容。  编译器的语义分析将进一步扩展，以便将（语法）常量模式（例如虚线表达式）绑定为类型，以便将其视为绑定类型模式，以支持此构造。

此更改后，您将能够编写
```csharp
void M(object o1, object o2)
{
    var t = (o1, o2);
    if (t is (int, string)) {} // test if o1 is an int and o2 is a string
    switch (o1) {
        case int: break; // test if o1 is an int
        case System.String: break; // test if o1 is a string
    }
}
```

## <a name="relational-patterns"></a>关系模式

关系模式允许程序员表示输入值必须满足关系约束，而常量值与常量值相比：

``` C#
    public static LifeStage LifeStageAtAge(int age) => age switch
    {
        < 0 =>  LifeStage.Prenatal,
        < 2 =>  LifeStage.Infant,
        < 4 =>  LifeStage.Toddler,
        < 6 =>  LifeStage.EarlyChild,
        < 12 => LifeStage.MiddleChild,
        < 20 => LifeStage.Adolescent,
        < 40 => LifeStage.EarlyAdult,
        < 65 => LifeStage.MiddleAdult,
        _ =>    LifeStage.LateAdult,
    };
```

关系模式`<`支持关系运算符 、`<=`和`>``>=`在所有支持具有相同类型操作数的二进制关系运算符的所有内置类型上。 具体而言，`sbyte`我们支持、、、、、、、、、、、、、`long``ulong``char``float``double``decimal``nint``nuint``byte``short``ushort``int``uint`和 的所有这些关系模式。

```antlr
primary_pattern
    : relational_pattern
    ;
relational_pattern
    : '<' relational_expression
    | '<=' relational_expression
    | '>' relational_expression
    | '>=' relational_expression
    ;
```

表达式需要计算到常量值。  如果常量值为 或`double.NaN``float.NaN`，则为 错误。  如果表达式为空常量，则为错误。

当输入是定义一个合适的内置二进制关系运算符的类型，该运算符适用于输入作为其左操作数，给定常量作为其右操作数，则该运算符的评估被视为关系模式的含义。  否则，我们使用显式可空或取消装箱转换将输入转换为表达式的类型。  如果不存在此类转换，则为编译时错误。  如果转换失败，则认为模式不匹配。  如果转换成功，则模式匹配操作的结果是计算`e OP v`表达式的结果，其中转换`e`的输入是`OP`关系运算符，是`v`常量表达式。

## <a name="pattern-combinators"></a>模式组合器

模式*组合器*允许使用`and`匹配两种不同模式（这可以通过重复使用）来扩展到任意数量的模式`and`，或者使用 （ditto） 对两种不同模式`or`的任意一种模式进行`not`*否定*。

组合器的常见用途是成语

``` c#
if (e is not null) ...
```

与当前习惯用法`e is object`相比，该模式的可读性更大，它清楚地表示一个人正在检查一个非空值。

和`and``or`组合器可用于测试值范围

``` c#
bool IsLetter(char c) => c is >= 'a' and <= 'z' or >= 'A' and <= 'Z';
```

此示例说明，`and`解析优先级（即绑定更紧密）将高于`or`。  程序员可以使用*括号模式*来显式优先级：

``` c#
bool IsLetter(char c) => c is (>= 'a' and <= 'z') or (>= 'A' and <= 'Z');
```

与所有模式一样，这些组合器可用于任何预期模式的上下文中，包括嵌套模式、*模式表达式*、*开关表达式*和 switch 语句大小写标签的模式。

```antlr
pattern
    : disjunctive_pattern
    ;
disjunctive_pattern
    : disjunctive_pattern 'or' conjunctive_pattern
    | conjunctive_pattern
    ;
conjunctive_pattern
    : conjunctive_pattern 'and' negated_pattern
    | negated_pattern
    ;
negated_pattern
    : 'not' negated_pattern
    | primary_pattern
    ;
primary_pattern
    : // all of the patterns forms previously defined
    ;
```

## <a name="open-issues-with-proposed-changes"></a>已打开的已建议更改问题

### <a name="syntax-for-relational-operators"></a>关系运算符的语法

是`and` `or`，`not`和 某种上下文关键字？  如果是这样，是否有重大的变化（例如，与声明*模式*中作为指定器的使用相比）。

### <a name="semantics-eg-type-for-relational-operators"></a>关系运算符的语义（例如类型）

我们期望支持使用关系运算符在表达式中比较的所有基元类型。  简单情况下的含义是明确的

``` c#
bool IsValidPercentage(int x) => x is >= 0 and <= 100;
```

但是，当输入不是这种基元类型时，我们尝试将其转换为什么类型？

``` c#
bool IsValidPercentage(object x) => x is >= 0 and <= 100;
```

我们提出，当输入类型已经是一个可比较的基元时，这就是比较的类型。 但是，当输入不是可比较基元时，我们将关系视为包括隐式类型测试的关系右侧常量类型。  如果程序员打算支持多个输入类型，则必须显式完成：

``` c#
bool IsValidPercentage(object x) => x is
    >= 0 and <= 100 or    // integer tests
    >= 0F and <= 100F or  // float tests
    >= 0D and <= 100D;    // double tests
```

### <a name="flowing-type-information-from-the-left-to-the-right-of-and"></a>从左侧到右侧的流式类型信息`and`

有人建议，当您编写`and`组合器时，在左侧学到的有关顶级类型的信息可能会流向右侧。  例如：

```csharp
bool isSmallByte(object o) => o is byte and < 100;
```

在这里，*输入类型*到第二个模式被缩小的类型*缩小*要求左侧`and`的左侧。  我们将定义所有模式的类型缩小语义，如下所示。  模式`P`的*窄缩小到*如下：
1. 如果是`P`类型模式，*则窄缩小类型*是类型模式的类型。
2. 如果是`P`声明模式，*则窄缩小类型*是声明模式的类型。
3. 如果`P`是提供显式类型的递归模式，则*窄范围类型*是该类型。
4. 如果`P`是常量不是空常量的常量模式，并且表达式没有向*输入类型的**常量表达式转换*，则*窄值类型*是常量的类型。
5. 如果`P`是常量表达式没有向*输入类型*转换*常量表达式*的关系模式，则*窄号类型*是常量的类型。
6. 如果是`P``or`模式，则如果存在此类公共类型，*则窄缩小到*子模式的*窄条类型的*通用类型。 为此，通用类型算法只考虑标识、装箱和隐式引用转换，并且它考虑一系列`or`模式的所有子模式（忽略括号模式）。
7. 如果是`P``and`模式，*则窄缩小到*右侧模式的*窄缩小到类型*。 此外，左侧模式的*窄缩小到*右模式的*输入类型*。
8. 否则，窄`P`*缩小的类型*是`P`'的输入类型。

### <a name="variable-definitions-and-definite-assignment"></a>变量定义和明确赋值

添加`or`和`not`模式围绕模式变量和明确分配创建了一些有趣的新问题。  由于变量通常最多可以声明一`or`次，因此当模式匹配时，在模式的一侧声明的任何模式变量似乎不会明确分配。  同样，当模式匹配时，`not`预计不会绝对分配在模式内声明的变量。  解决这个问题的最简单方法是禁止在这些上下文中声明模式变量。  但是，这可能限制性太强。  还有其他方法需要考虑。

值得考虑的一种情况是

``` csharp
if (e is not int i) return;
M(i); // is i definitely assigned here?
```

这在今天不起作用，因为对于 is*模式表达式*，模式变量仅在*模式表达式*为 true（"当为 true 时明确分配"）时才*被视为绝对分配*。

支持这一点（从程序员的角度来看）比添加对否定条件`if`语句的支持更简单。  即使我们添加这种支持，程序员也会想知道为什么上面的代码段不起作用。  另一方面，在 中`switch`相同的方案不太有意义，因为程序中没有相应的点，其中*明确分配时为 false*有意义。  我们是否允许在*是模式表达式*中，但在允许模式的其他上下文中允许这样做？  这似乎不规则。

与此相关的是*分词模式*中明确分配的问题。

```csharp
if (e is 0 or int i)
{
    M(i); // is i definitely assigned here?
}
```

我们只希望在`i`输入不是零时被明确分配。  但是，由于我们不知道模块内的输入是否为零，`i`因此未明确分配。  但是，如果我们允许`i`以不同的互斥模式声明，该怎么办？

```csharp
if ((e1, e2) is (0, int i) or (int i, 0))
{
    M(i);
}
```

在这里，变量`i`肯定在块内分配，并在找到零元素时从元组的其他元素获取该变量的值。

还有人建议允许在案例块的每个情况下定义变量（乘以）：

```csharp
    case (0, int x):
    case (int x, 0):
        Console.WriteLine(x);
```

为了进行任何这样的工作，我们必须仔细定义允许此类多个定义的位置，以及在什么条件下，此类变量被视为绝对分配的变量。

如果我们选择推迟这样的工作，直到后来（我建议），我们可以说在C#9
- 或`not``or`下 不能声明 模式变量。

然后，我们将有时间发展一些经验，提供洞察可能的价值放松以后。

### <a name="diagnostics-subsumption-and-exhaustiveness"></a>诊断、资助和详尽无遗

这些新模式形式为可诊断的程序员错误引入了许多新的机会。  我们需要决定我们将诊断哪些类型的错误，以及如何诊断错误。  下面是一些示例：

``` csharp
case >= 0 and <= 100D:
```

此情况永远不能匹配（因为输入不能同时是 和`int`。 `double`  当我们检测到无法匹配的情况时，我们已经出现了错误，但其措辞（"交换机案例已由以前的案例处理"和"模式已由交换机表达式的前一分支处理"）可能会在新的方案中产生误导。  我们可能不得不修改措辞，只是说模式永远不会与输入匹配。

``` csharp
case 1 and 2:
```

同样，这将是一个错误，因为值不能兼`1`和`2`。

``` csharp
case 1 or 2 or 3 or 1:
```

此情况可以匹配，`or 1`但末尾的模式没有增加任何意义。  我建议，当复合模式的某些连结或分离不定义模式变量或影响匹配值集时，我们应该尝试产生错误。

``` csharp
case < 2: break;
case 0 or 1 or 2 or 3 or 4 or 5: break;
```

此处，`0 or 1 or`不会向第二个案例添加任何内容，因为这些值将由第一个案例处理。  这也应该是一个错误。

``` csharp
byte b = ...;
int x = b switch { <100 => 0, 100 => 1, 101 => 2, >101 => 3 };
```

此类开关表达式应视为*详尽无遗*（它处理所有可能的输入值）。

在 C# 8.0 中，具有类型`byte`输入的开关表达式只有在包含其模式与一切（*丢弃模式*或 var*模式*）匹配的最终臂时才被视为详尽无遗。  在 C# 8 中，即使具有`byte`每个不同值臂的开关表达式也不被视为详尽无遗。  为了正确处理关系模式的详尽无遗，我们也必须处理这个案子。  从技术上讲，这将是一个重大的变化，但任何用户都不可能注意到。
