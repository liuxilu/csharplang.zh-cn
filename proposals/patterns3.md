---
ms.openlocfilehash: 57cdb682efd4cb169308e347d63e27c97b9f1b7a
ms.sourcegitcommit: 6901635c383801e4d177085587aaccadaa7b2f11
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641960"
---
# <a name="pattern-matching-changes-for-c-90"></a><span data-ttu-id="4691b-101">C# 9.0 的模式匹配更改</span><span class="sxs-lookup"><span data-stu-id="4691b-101">Pattern-matching changes for C# 9.0</span></span>

<span data-ttu-id="4691b-102">我们正在考虑对 C# 9.0 的模式匹配进行少量增强，这些增强具有天然的协同效应，并很好地解决了许多常见的编程问题：</span><span class="sxs-lookup"><span data-stu-id="4691b-102">We are considering a small handful of enhancements to pattern-matching for C# 9.0 that have natural synergy and work well to address a number of common programming problems:</span></span>
- <span data-ttu-id="4691b-103">https://github.com/dotnet/csharplang/issues/2925类型模式</span><span class="sxs-lookup"><span data-stu-id="4691b-103">https://github.com/dotnet/csharplang/issues/2925 Type patterns</span></span>
- <span data-ttu-id="4691b-104">https://github.com/dotnet/csharplang/issues/1350括号调整的模式，以强制执行或强调新组合器的优先级</span><span class="sxs-lookup"><span data-stu-id="4691b-104">https://github.com/dotnet/csharplang/issues/1350 Parenthesized patterns to enforce or emphasize precedence of the new combinators</span></span>
- <span data-ttu-id="4691b-105">https://github.com/dotnet/csharplang/issues/1350需要两种不同`and`模式匹配的构成模式;</span><span class="sxs-lookup"><span data-stu-id="4691b-105">https://github.com/dotnet/csharplang/issues/1350 Conjunctive `and` patterns that require both of two different patterns to match;</span></span>
- <span data-ttu-id="4691b-106">https://github.com/dotnet/csharplang/issues/1350需要匹配两种`or`不同模式之一的分离模式;</span><span class="sxs-lookup"><span data-stu-id="4691b-106">https://github.com/dotnet/csharplang/issues/1350 Disjunctive `or` patterns that require either of two different patterns to match;</span></span>
- <span data-ttu-id="4691b-107">https://github.com/dotnet/csharplang/issues/1350需要给定`not`模式*不匹配*的否定模式;和</span><span class="sxs-lookup"><span data-stu-id="4691b-107">https://github.com/dotnet/csharplang/issues/1350 Negated `not` patterns that require a given pattern *not* to match; and</span></span>
- <span data-ttu-id="4691b-108">https://github.com/dotnet/csharplang/issues/812要求输入值小于、小于或等于给定常量等的关系模式。</span><span class="sxs-lookup"><span data-stu-id="4691b-108">https://github.com/dotnet/csharplang/issues/812 Relational patterns that require the input value to be less than, less than or equal to, etc a given constant.</span></span>

## <a name="parenthesized-patterns"></a><span data-ttu-id="4691b-109">括号模式</span><span class="sxs-lookup"><span data-stu-id="4691b-109">Parenthesized Patterns</span></span>

<span data-ttu-id="4691b-110">括号模式允许程序员将括号放在任何模式周围。</span><span class="sxs-lookup"><span data-stu-id="4691b-110">Parenthesized patterns permit the programmer to put parentheses around any pattern.</span></span>  <span data-ttu-id="4691b-111">这在 C# 8.0 中的现有模式中没有那么有用，但是新的模式组合器引入了程序员可能想要重写的优先级。</span><span class="sxs-lookup"><span data-stu-id="4691b-111">This is not so useful with the existing patterns in C# 8.0, however the new pattern combinators introduce a precedence that the programmer may want to override.</span></span>

```antlr
primary_pattern
    : parenthesized_pattern
    | // all of the existing forms
    ;
parenthesized_pattern
    : '(' pattern ')'
    ;
```

## <a name="type-patterns"></a><span data-ttu-id="4691b-112">类型模式</span><span class="sxs-lookup"><span data-stu-id="4691b-112">Type Patterns</span></span>

<span data-ttu-id="4691b-113">我们允许类型作为模式：</span><span class="sxs-lookup"><span data-stu-id="4691b-113">We permit a type as a pattern:</span></span>

``` antlr
primary_pattern
    : type-pattern
    | // all of the existing forms
    ;
type_pattern
    : type
    ;
```

<span data-ttu-id="4691b-114">这会使现有的*is-type 表达式*作为一个模式模式为*类型模式\*\*的正模式表达式*，尽管我们不会更改编译器生成的语法树。</span><span class="sxs-lookup"><span data-stu-id="4691b-114">This retcons the existing *is-type-expression* to be an *is-pattern-expression* in which the pattern is a *type-pattern*, though we would not change the syntax tree produced by the compiler.</span></span>

<span data-ttu-id="4691b-115">一个微妙的实现问题是，这个语法是模糊的。</span><span class="sxs-lookup"><span data-stu-id="4691b-115">One subtle implementation issue is that this grammar is ambiguous.</span></span>  <span data-ttu-id="4691b-116">可以将 （`a.b`如）的字符串解析为限定名称（在类型上下文中）或虚线表达式（在表达式上下文中）。</span><span class="sxs-lookup"><span data-stu-id="4691b-116">A string such as `a.b` can be parsed either as a qualified name (in a type context) or a dotted expression (in an expression context).</span></span>  <span data-ttu-id="4691b-117">编译器已经能够处理与虚线表达式相同的限定名称，以便处理类似`e is Color.Red`之类的内容。</span><span class="sxs-lookup"><span data-stu-id="4691b-117">The compiler is already capable of treating a qualified name the same as a dotted expression in order to handle something like `e is Color.Red`.</span></span>  <span data-ttu-id="4691b-118">编译器的语义分析将进一步扩展，以便将（语法）常量模式（例如虚线表达式）绑定为类型，以便将其视为绑定类型模式，以支持此构造。</span><span class="sxs-lookup"><span data-stu-id="4691b-118">The compiler's semantic analysis would be further extended to be capable of binding a (syntactic) constant pattern (e.g. a dotted expression) as a type in order to treat it as a bound type pattern in order to support this construct.</span></span>

<span data-ttu-id="4691b-119">此更改后，您将能够编写</span><span class="sxs-lookup"><span data-stu-id="4691b-119">After this change, you would be able to write</span></span>
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

## <a name="relational-patterns"></a><span data-ttu-id="4691b-120">关系模式</span><span class="sxs-lookup"><span data-stu-id="4691b-120">Relational Patterns</span></span>

<span data-ttu-id="4691b-121">关系模式允许程序员表示输入值必须满足关系约束，而常量值与常量值相比：</span><span class="sxs-lookup"><span data-stu-id="4691b-121">Relational patterns permit the programmer to express that an input value must satisfy a relational constraint when compared to a constant value:</span></span>

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

<span data-ttu-id="4691b-122">关系模式`<`支持关系运算符 、`<=`和`>``>=`在所有支持具有相同类型操作数的二进制关系运算符的所有内置类型上。</span><span class="sxs-lookup"><span data-stu-id="4691b-122">Relational patterns support the relational operators `<`, `<=`, `>`, and `>=` on all of the built-in types that support such binary relational operators with two operands of the same type in an expression.</span></span> <span data-ttu-id="4691b-123">具体而言，`sbyte`我们支持、、、、、、、、、、、、、`long``ulong``char``float``double``decimal``nint``nuint``byte``short``ushort``int``uint`和 的所有这些关系模式。</span><span class="sxs-lookup"><span data-stu-id="4691b-123">Specifically, we support all of these relational patterns for `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `nint`, and `nuint`.</span></span>

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

<span data-ttu-id="4691b-124">表达式需要计算到常量值。</span><span class="sxs-lookup"><span data-stu-id="4691b-124">The expression is required to evaluate to a constant value.</span></span>  <span data-ttu-id="4691b-125">如果常量值为 或`double.NaN``float.NaN`，则为 错误。</span><span class="sxs-lookup"><span data-stu-id="4691b-125">It is an error if that constant value is `double.NaN` or `float.NaN`.</span></span>  <span data-ttu-id="4691b-126">如果表达式为空常量，则为错误。</span><span class="sxs-lookup"><span data-stu-id="4691b-126">It is an error if the expression is a null constant.</span></span>

<span data-ttu-id="4691b-127">当输入是定义一个合适的内置二进制关系运算符的类型，该运算符适用于输入作为其左操作数，给定常量作为其右操作数，则该运算符的评估被视为关系模式的含义。</span><span class="sxs-lookup"><span data-stu-id="4691b-127">When the input is a type for which a suitable built-in binary relational operator is defined that is applicable with the input as its left operand and the given constant as its right operand, the evaluation of that operator is taken as the meaning of the relational pattern.</span></span>  <span data-ttu-id="4691b-128">否则，我们使用显式可空或取消装箱转换将输入转换为表达式的类型。</span><span class="sxs-lookup"><span data-stu-id="4691b-128">Otherwise we convert the input to the type of the expression using an explicit nullable or unboxing conversion.</span></span>  <span data-ttu-id="4691b-129">如果不存在此类转换，则为编译时错误。</span><span class="sxs-lookup"><span data-stu-id="4691b-129">It is a compile-time error if no such conversion exists.</span></span>  <span data-ttu-id="4691b-130">如果转换失败，则认为模式不匹配。</span><span class="sxs-lookup"><span data-stu-id="4691b-130">The pattern is considered not to match if the conversion fails.</span></span>  <span data-ttu-id="4691b-131">如果转换成功，则模式匹配操作的结果是计算`e OP v`表达式的结果，其中转换`e`的输入是`OP`关系运算符，是`v`常量表达式。</span><span class="sxs-lookup"><span data-stu-id="4691b-131">If the conversion succeeds then the result of the pattern-matching operation is the result of evaluating the expression `e OP v` where `e` is the converted input, `OP` is the relational operator, and `v` is the constant expression.</span></span>

## <a name="pattern-combinators"></a><span data-ttu-id="4691b-132">模式组合器</span><span class="sxs-lookup"><span data-stu-id="4691b-132">Pattern Combinators</span></span>

<span data-ttu-id="4691b-133">模式*组合器*允许使用`and`匹配两种不同模式（这可以通过重复使用）来扩展到任意数量的模式`and`，或者使用 （ditto） 对两种不同模式`or`的任意一种模式进行`not`*否定*。</span><span class="sxs-lookup"><span data-stu-id="4691b-133">Pattern *combinators* permit matching both of two different patterns using `and` (this can be extended to any number of patterns by the repeated use of `and`), either of two different patterns using `or` (ditto), or the *negation* of a pattern using `not`.</span></span>

<span data-ttu-id="4691b-134">组合器的常见用途是成语</span><span class="sxs-lookup"><span data-stu-id="4691b-134">A common use of a combinator will be the idiom</span></span>

``` c#
if (e is not null) ...
```

<span data-ttu-id="4691b-135">与当前习惯用法`e is object`相比，该模式的可读性更大，它清楚地表示一个人正在检查一个非空值。</span><span class="sxs-lookup"><span data-stu-id="4691b-135">More readable than the current idiom `e is object`, this pattern clearly expresses that one is checking for a non-null value.</span></span>

<span data-ttu-id="4691b-136">和`and``or`组合器可用于测试值范围</span><span class="sxs-lookup"><span data-stu-id="4691b-136">The `and` and `or` combinators will be useful for testing ranges of values</span></span>

``` c#
bool IsLetter(char c) => c is >= 'a' and <= 'z' or >= 'A' and <= 'Z';
```

<span data-ttu-id="4691b-137">此示例说明，`and`解析优先级（即绑定更紧密）将高于`or`。</span><span class="sxs-lookup"><span data-stu-id="4691b-137">This example illustrates that `and` will have a higher parsing priority (i.e. will bind more closely) than `or`.</span></span>  <span data-ttu-id="4691b-138">程序员可以使用*括号模式*来显式优先级：</span><span class="sxs-lookup"><span data-stu-id="4691b-138">The programmer can use the *parenthesized pattern* to make the precedence explicit:</span></span>

``` c#
bool IsLetter(char c) => c is (>= 'a' and <= 'z') or (>= 'A' and <= 'Z');
```

<span data-ttu-id="4691b-139">与所有模式一样，这些组合器可用于任何预期模式的上下文中，包括嵌套模式、*模式表达式*、*开关表达式*和 switch 语句大小写标签的模式。</span><span class="sxs-lookup"><span data-stu-id="4691b-139">Like all patterns, these combinators can be used in any context in which a pattern is expected, including nested patterns, the *is-pattern-expression*, the *switch-expression*, and the pattern of a switch statement's case label.</span></span>

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

## <a name="open-issues-with-proposed-changes"></a><span data-ttu-id="4691b-140">已打开的已建议更改问题</span><span class="sxs-lookup"><span data-stu-id="4691b-140">Open Issues with Proposed Changes</span></span>

### <a name="syntax-for-relational-operators"></a><span data-ttu-id="4691b-141">关系运算符的语法</span><span class="sxs-lookup"><span data-stu-id="4691b-141">Syntax for relational operators</span></span>

<span data-ttu-id="4691b-142">是`and` `or`，`not`和 某种上下文关键字？</span><span class="sxs-lookup"><span data-stu-id="4691b-142">Are `and`, `or`, and `not` some kind of contextual keyword?</span></span>  <span data-ttu-id="4691b-143">如果是这样，是否有重大的变化（例如，与声明*模式*中作为指定器的使用相比）。</span><span class="sxs-lookup"><span data-stu-id="4691b-143">If so, is there a breaking change (e.g. compared to their use as a designator in a *declaration-pattern*).</span></span>

### <a name="semantics-eg-type-for-relational-operators"></a><span data-ttu-id="4691b-144">关系运算符的语义（例如类型）</span><span class="sxs-lookup"><span data-stu-id="4691b-144">Semantics (e.g. type) for relational operators</span></span>

<span data-ttu-id="4691b-145">我们期望支持使用关系运算符在表达式中比较的所有基元类型。</span><span class="sxs-lookup"><span data-stu-id="4691b-145">We expect to support all of the primitive types that can be compared in an expression using a relational operator.</span></span>  <span data-ttu-id="4691b-146">简单情况下的含义是明确的</span><span class="sxs-lookup"><span data-stu-id="4691b-146">The meaning in simple cases is clear</span></span>

``` c#
bool IsValidPercentage(int x) => x is >= 0 and <= 100;
```

<span data-ttu-id="4691b-147">但是，当输入不是这种基元类型时，我们尝试将其转换为什么类型？</span><span class="sxs-lookup"><span data-stu-id="4691b-147">But when the input is not such a primitive type, what type do we attempt to convert it to?</span></span>

``` c#
bool IsValidPercentage(object x) => x is >= 0 and <= 100;
```

<span data-ttu-id="4691b-148">我们提出，当输入类型已经是一个可比较的基元时，这就是比较的类型。</span><span class="sxs-lookup"><span data-stu-id="4691b-148">We have proposed that when the input type is already a comparable primitive, that is the type of the comparison.</span></span> <span data-ttu-id="4691b-149">但是，当输入不是可比较基元时，我们将关系视为包括隐式类型测试的关系右侧常量类型。</span><span class="sxs-lookup"><span data-stu-id="4691b-149">However, when the input is not a comparable primitive, we treat the relational as including an implicit type test to the type of the constant on the right-hand-side of the relational.</span></span>  <span data-ttu-id="4691b-150">如果程序员打算支持多个输入类型，则必须显式完成：</span><span class="sxs-lookup"><span data-stu-id="4691b-150">If the programmer intends to support more than one input type, that must be done explicitly:</span></span>

``` c#
bool IsValidPercentage(object x) => x is
    >= 0 and <= 100 or    // integer tests
    >= 0F and <= 100F or  // float tests
    >= 0D and <= 100D;    // double tests
```

### <a name="flowing-type-information-from-the-left-to-the-right-of-and"></a><span data-ttu-id="4691b-151">从左侧到右侧的流式类型信息`and`</span><span class="sxs-lookup"><span data-stu-id="4691b-151">Flowing type information from the left to the right of `and`</span></span>

<span data-ttu-id="4691b-152">有人建议，当您编写`and`组合器时，在左侧学到的有关顶级类型的信息可能会流向右侧。</span><span class="sxs-lookup"><span data-stu-id="4691b-152">It has been suggested that when you write an `and` combinator, type information learned on the left about the top-level type could flow to the right.</span></span>  <span data-ttu-id="4691b-153">例如：</span><span class="sxs-lookup"><span data-stu-id="4691b-153">For example</span></span>

```csharp
bool isSmallByte(object o) => o is byte and < 100;
```

<span data-ttu-id="4691b-154">在这里，*输入类型*到第二个模式被缩小的类型*缩小*要求左侧`and`的左侧。</span><span class="sxs-lookup"><span data-stu-id="4691b-154">Here, the *input type* to the second pattern is narrowed by the *type narrowing* requirements of left of the `and`.</span></span>  <span data-ttu-id="4691b-155">我们将定义所有模式的类型缩小语义，如下所示。</span><span class="sxs-lookup"><span data-stu-id="4691b-155">We would define type narrowing semantics for all patterns as follows.</span></span>  <span data-ttu-id="4691b-156">模式`P`的*窄缩小到*如下：</span><span class="sxs-lookup"><span data-stu-id="4691b-156">The *narrowed type* of a pattern `P` is defined as follows:</span></span>
1. <span data-ttu-id="4691b-157">如果是`P`类型模式，*则窄缩小类型*是类型模式的类型。</span><span class="sxs-lookup"><span data-stu-id="4691b-157">If `P` is a type pattern, the *narrowed type* is the type of the type pattern's type.</span></span>
2. <span data-ttu-id="4691b-158">如果是`P`声明模式，*则窄缩小类型*是声明模式的类型。</span><span class="sxs-lookup"><span data-stu-id="4691b-158">If `P` is a declaration pattern, the *narrowed type* is the type of the declaration pattern's type.</span></span>
3. <span data-ttu-id="4691b-159">如果`P`是提供显式类型的递归模式，则*窄范围类型*是该类型。</span><span class="sxs-lookup"><span data-stu-id="4691b-159">If `P` is a recursive pattern that gives an explicit type, the *narrowed type* is that type.</span></span>
4. <span data-ttu-id="4691b-160">如果`P`是常量不是空常量的常量模式，并且表达式没有向*输入类型的\*\*常量表达式转换*，则*窄值类型*是常量的类型。</span><span class="sxs-lookup"><span data-stu-id="4691b-160">If `P` is a constant pattern where the constant is not the null constant and where the expression has no *constant expression conversion* to the *input type*, the *narrowed type* is the type of the constant.</span></span>
5. <span data-ttu-id="4691b-161">如果`P`是常量表达式没有向*输入类型*转换*常量表达式*的关系模式，则*窄号类型*是常量的类型。</span><span class="sxs-lookup"><span data-stu-id="4691b-161">If `P` is a relational pattern where the constant expression has no *constant expression conversion* to the *input type*, the *narrowed type* is the type of the constant.</span></span>
6. <span data-ttu-id="4691b-162">如果是`P``or`模式，则如果存在此类公共类型，*则窄缩小到*子模式的*窄条类型的*通用类型。</span><span class="sxs-lookup"><span data-stu-id="4691b-162">If `P` is an `or` pattern, the *narrowed type* is the common type of the *narrowed type* of the subpatterns if such a common type exists.</span></span> <span data-ttu-id="4691b-163">为此，通用类型算法只考虑标识、装箱和隐式引用转换，并且它考虑一系列`or`模式的所有子模式（忽略括号模式）。</span><span class="sxs-lookup"><span data-stu-id="4691b-163">For this purpose, the common type algorithm considers only identity, boxing, and implicit reference conversions, and it considers all subpatterns of a sequence of `or` patterns (ignoring parenthesized patterns).</span></span>
7. <span data-ttu-id="4691b-164">如果是`P``and`模式，*则窄缩小到*右侧模式的*窄缩小到类型*。</span><span class="sxs-lookup"><span data-stu-id="4691b-164">If `P` is an `and` pattern, the *narrowed type* is the *narrowed type* of the right pattern.</span></span> <span data-ttu-id="4691b-165">此外，左侧模式的*窄缩小到*右模式的*输入类型*。</span><span class="sxs-lookup"><span data-stu-id="4691b-165">Moreover, the *narrowed type* of the left pattern is the *input type* of the right pattern.</span></span>
8. <span data-ttu-id="4691b-166">否则，窄`P`*缩小的类型*是`P`'的输入类型。</span><span class="sxs-lookup"><span data-stu-id="4691b-166">Otherwise the *narrowed type* of `P` is `P`'s input type.</span></span>

### <a name="variable-definitions-and-definite-assignment"></a><span data-ttu-id="4691b-167">变量定义和明确赋值</span><span class="sxs-lookup"><span data-stu-id="4691b-167">Variable definitions and definite assignment</span></span>

<span data-ttu-id="4691b-168">添加`or`和`not`模式围绕模式变量和明确分配创建了一些有趣的新问题。</span><span class="sxs-lookup"><span data-stu-id="4691b-168">The addition of `or` and `not` patterns creates some interesting new problems around pattern variables and definite assignment.</span></span>  <span data-ttu-id="4691b-169">由于变量通常最多可以声明一`or`次，因此当模式匹配时，在模式的一侧声明的任何模式变量似乎不会明确分配。</span><span class="sxs-lookup"><span data-stu-id="4691b-169">Since variables can normally be declared at most once, it would seem any pattern variable declared on one side of an `or` pattern would not be definitely assigned when the pattern matches.</span></span>  <span data-ttu-id="4691b-170">同样，当模式匹配时，`not`预计不会绝对分配在模式内声明的变量。</span><span class="sxs-lookup"><span data-stu-id="4691b-170">Similarly, a variable declared inside a `not` pattern would not be expected to be definitely assigned when the pattern matches.</span></span>  <span data-ttu-id="4691b-171">解决这个问题的最简单方法是禁止在这些上下文中声明模式变量。</span><span class="sxs-lookup"><span data-stu-id="4691b-171">The simplest way to address this is to forbid declaring pattern variables in these contexts.</span></span>  <span data-ttu-id="4691b-172">但是，这可能限制性太强。</span><span class="sxs-lookup"><span data-stu-id="4691b-172">However, this may be too restrictive.</span></span>  <span data-ttu-id="4691b-173">还有其他方法需要考虑。</span><span class="sxs-lookup"><span data-stu-id="4691b-173">There are other approaches to consider.</span></span>

<span data-ttu-id="4691b-174">值得考虑的一种情况是</span><span class="sxs-lookup"><span data-stu-id="4691b-174">One scenario that is worth considering is this</span></span>

``` csharp
if (e is not int i) return;
M(i); // is i definitely assigned here?
```

<span data-ttu-id="4691b-175">这在今天不起作用，因为对于 is*模式表达式*，模式变量仅在*模式表达式*为 true（"当为 true 时明确分配"）时才*被视为绝对分配*。</span><span class="sxs-lookup"><span data-stu-id="4691b-175">This does not work today because, for an *is-pattern-expression*, the pattern variables are considered *definitely assigned* only where the *is-pattern-expression* is true ("definitely assigned when true").</span></span>

<span data-ttu-id="4691b-176">支持这一点（从程序员的角度来看）比添加对否定条件`if`语句的支持更简单。</span><span class="sxs-lookup"><span data-stu-id="4691b-176">Supporting this would be simpler (from the programmer's perspective) than also adding support for a negated-condition `if` statement.</span></span>  <span data-ttu-id="4691b-177">即使我们添加这种支持，程序员也会想知道为什么上面的代码段不起作用。</span><span class="sxs-lookup"><span data-stu-id="4691b-177">Even if we add such support, programmers would wonder why the above snippet does not work.</span></span>  <span data-ttu-id="4691b-178">另一方面，在 中`switch`相同的方案不太有意义，因为程序中没有相应的点，其中*明确分配时为 false*有意义。</span><span class="sxs-lookup"><span data-stu-id="4691b-178">On the other hand, the same scenario in a `switch` makes less sense, as there is no corresponding point in the program where *definitely assigned when false* would be meaningful.</span></span>  <span data-ttu-id="4691b-179">我们是否允许在*是模式表达式*中，但在允许模式的其他上下文中允许这样做？</span><span class="sxs-lookup"><span data-stu-id="4691b-179">Would we permit this in an *is-pattern-expression* but not in other contexts where patterns are permitted?</span></span>  <span data-ttu-id="4691b-180">这似乎不规则。</span><span class="sxs-lookup"><span data-stu-id="4691b-180">That seems irregular.</span></span>

<span data-ttu-id="4691b-181">与此相关的是*分词模式*中明确分配的问题。</span><span class="sxs-lookup"><span data-stu-id="4691b-181">Related to this is the problem of definite assignment in a *disjunctive-pattern*.</span></span>

```csharp
if (e is 0 or int i)
{
    M(i); // is i definitely assigned here?
}
```

<span data-ttu-id="4691b-182">我们只希望在`i`输入不是零时被明确分配。</span><span class="sxs-lookup"><span data-stu-id="4691b-182">We would only expect `i` to be definitely assigned when the input is not zero.</span></span>  <span data-ttu-id="4691b-183">但是，由于我们不知道模块内的输入是否为零，`i`因此未明确分配。</span><span class="sxs-lookup"><span data-stu-id="4691b-183">But since we don't know whether the input is zero or not inside the block, `i` is not definitely assigned.</span></span>  <span data-ttu-id="4691b-184">但是，如果我们允许`i`以不同的互斥模式声明，该怎么办？</span><span class="sxs-lookup"><span data-stu-id="4691b-184">However, what if we permit `i` to be declared in different mutually exclusive patterns?</span></span>

```csharp
if ((e1, e2) is (0, int i) or (int i, 0))
{
    M(i);
}
```

<span data-ttu-id="4691b-185">在这里，变量`i`肯定在块内分配，并在找到零元素时从元组的其他元素获取该变量的值。</span><span class="sxs-lookup"><span data-stu-id="4691b-185">Here, the variable `i` is definitely assigned inside the block, and takes it value from the other element of the tuple when a zero element is found.</span></span>

<span data-ttu-id="4691b-186">还有人建议允许在案例块的每个情况下定义变量（乘以）：</span><span class="sxs-lookup"><span data-stu-id="4691b-186">It has also been suggested to permit variables to be (multiply) defined in every case of a case block:</span></span>

```csharp
    case (0, int x):
    case (int x, 0):
        Console.WriteLine(x);
```

<span data-ttu-id="4691b-187">为了进行任何这样的工作，我们必须仔细定义允许此类多个定义的位置，以及在什么条件下，此类变量被视为绝对分配的变量。</span><span class="sxs-lookup"><span data-stu-id="4691b-187">To make any of this work, we would have to carefully define where such multiple definitions are permitted and under what conditions such a variable is considered definitely assigned.</span></span>

<span data-ttu-id="4691b-188">如果我们选择推迟这样的工作，直到后来（我建议），我们可以说在C#9</span><span class="sxs-lookup"><span data-stu-id="4691b-188">Should we elect to defer such work until later (which I advise), we could say in C# 9</span></span>
- <span data-ttu-id="4691b-189">或`not``or`下 不能声明 模式变量。</span><span class="sxs-lookup"><span data-stu-id="4691b-189">beneath a `not` or `or`, pattern variables may not be declared.</span></span>

<span data-ttu-id="4691b-190">然后，我们将有时间发展一些经验，提供洞察可能的价值放松以后。</span><span class="sxs-lookup"><span data-stu-id="4691b-190">Then, we would have time to develop some experience that would provide insight into the possible value of relaxing that later.</span></span>

### <a name="diagnostics-subsumption-and-exhaustiveness"></a><span data-ttu-id="4691b-191">诊断、资助和详尽无遗</span><span class="sxs-lookup"><span data-stu-id="4691b-191">Diagnostics, subsumption, and exhaustiveness</span></span>

<span data-ttu-id="4691b-192">这些新模式形式为可诊断的程序员错误引入了许多新的机会。</span><span class="sxs-lookup"><span data-stu-id="4691b-192">These new pattern forms introduce many new opportunities for diagnosable programmer error.</span></span>  <span data-ttu-id="4691b-193">我们需要决定我们将诊断哪些类型的错误，以及如何诊断错误。</span><span class="sxs-lookup"><span data-stu-id="4691b-193">We will need to decide what kinds of errors we will diagnose, and how to do so.</span></span>  <span data-ttu-id="4691b-194">下面是一些示例：</span><span class="sxs-lookup"><span data-stu-id="4691b-194">Here are some examples:</span></span>

``` csharp
case >= 0 and <= 100D:
```

<span data-ttu-id="4691b-195">此情况永远不能匹配（因为输入不能同时是 和`int`。 `double`</span><span class="sxs-lookup"><span data-stu-id="4691b-195">This case can never match (because the input cannot be both an `int` and a `double`).</span></span>  <span data-ttu-id="4691b-196">当我们检测到无法匹配的情况时，我们已经出现了错误，但其措辞（"交换机案例已由以前的案例处理"和"模式已由交换机表达式的前一分支处理"）可能会在新的方案中产生误导。</span><span class="sxs-lookup"><span data-stu-id="4691b-196">We already have an error when we detect a case that can never match, but its wording ("The switch case has already been handled by a previous case" and "The pattern has already been handled by a previous arm of the switch expression") may be misleading in new scenarios.</span></span>  <span data-ttu-id="4691b-197">我们可能不得不修改措辞，只是说模式永远不会与输入匹配。</span><span class="sxs-lookup"><span data-stu-id="4691b-197">We may have to modify the wording to just say that the pattern will never match the input.</span></span>

``` csharp
case 1 and 2:
```

<span data-ttu-id="4691b-198">同样，这将是一个错误，因为值不能兼`1`和`2`。</span><span class="sxs-lookup"><span data-stu-id="4691b-198">Similarly, this would be an error because a value cannot be both `1` and `2`.</span></span>

``` csharp
case 1 or 2 or 3 or 1:
```

<span data-ttu-id="4691b-199">此情况可以匹配，`or 1`但末尾的模式没有增加任何意义。</span><span class="sxs-lookup"><span data-stu-id="4691b-199">This case is possible to match, but the `or 1` at the end adds no meaning to the pattern.</span></span>  <span data-ttu-id="4691b-200">我建议，当复合模式的某些连结或分离不定义模式变量或影响匹配值集时，我们应该尝试产生错误。</span><span class="sxs-lookup"><span data-stu-id="4691b-200">I suggest we should aim to produce an error whenever some conjunct or disjunct of a compound pattern does not either define a pattern variable or affect the set of matched values.</span></span>

``` csharp
case < 2: break;
case 0 or 1 or 2 or 3 or 4 or 5: break;
```

<span data-ttu-id="4691b-201">此处，`0 or 1 or`不会向第二个案例添加任何内容，因为这些值将由第一个案例处理。</span><span class="sxs-lookup"><span data-stu-id="4691b-201">Here, `0 or 1 or` adds nothing to the second case, as those values would have been handled by the first case.</span></span>  <span data-ttu-id="4691b-202">这也应该是一个错误。</span><span class="sxs-lookup"><span data-stu-id="4691b-202">This too deserves an error.</span></span>

``` csharp
byte b = ...;
int x = b switch { <100 => 0, 100 => 1, 101 => 2, >101 => 3 };
```

<span data-ttu-id="4691b-203">此类开关表达式应视为*详尽无遗*（它处理所有可能的输入值）。</span><span class="sxs-lookup"><span data-stu-id="4691b-203">A switch expression such as this should be considered *exhaustive* (it handles all possible input values).</span></span>

<span data-ttu-id="4691b-204">在 C# 8.0 中，具有类型`byte`输入的开关表达式只有在包含其模式与一切（*丢弃模式*或 var*模式*）匹配的最终臂时才被视为详尽无遗。</span><span class="sxs-lookup"><span data-stu-id="4691b-204">In C# 8.0, a switch expression with an input of type `byte` is only considered exhaustive if it contains a final arm whose pattern matches everything (a *discard-pattern* or *var-pattern*).</span></span>  <span data-ttu-id="4691b-205">在 C# 8 中，即使具有`byte`每个不同值臂的开关表达式也不被视为详尽无遗。</span><span class="sxs-lookup"><span data-stu-id="4691b-205">Even a switch expression that has an arm for every distinct `byte` value is not considered exhaustive in C# 8.</span></span>  <span data-ttu-id="4691b-206">为了正确处理关系模式的详尽无遗，我们也必须处理这个案子。</span><span class="sxs-lookup"><span data-stu-id="4691b-206">In order to properly handle exhaustiveness of relational patterns, we will have to handle this case too.</span></span>  <span data-ttu-id="4691b-207">从技术上讲，这将是一个重大的变化，但任何用户都不可能注意到。</span><span class="sxs-lookup"><span data-stu-id="4691b-207">This will technically be a breaking change, but no user is likely to notice.</span></span>
