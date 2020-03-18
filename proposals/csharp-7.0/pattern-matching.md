---
ms.openlocfilehash: 3df21c5816be90387a6cd9242e99ba11f43dfd1c
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/14/2020
ms.locfileid: "79484068"
---
# <a name="pattern-matching-for-c-7"></a><span data-ttu-id="ed402-101">7的C#模式匹配</span><span class="sxs-lookup"><span data-stu-id="ed402-101">Pattern Matching for C# 7</span></span>

<span data-ttu-id="ed402-102">模式匹配扩展可C#实现与功能语言的代数数据类型和模式匹配的许多优势，但这种方式与基础语言的外观紧密集成。</span><span class="sxs-lookup"><span data-stu-id="ed402-102">Pattern matching extensions for C# enable many of the benefits of algebraic data types and pattern matching from functional languages, but in a way that smoothly integrates with the feel of the underlying language.</span></span> <span data-ttu-id="ed402-103">基本功能包括：[记录类型](https://github.com/dotnet/csharplang/blob/master/proposals/records.md)，这些类型的语义含义由数据形状描述;和模式匹配，这是一个新的表达式窗体，用于启用这些数据类型的极简洁的多级分解。</span><span class="sxs-lookup"><span data-stu-id="ed402-103">The basic features are: [record types](https://github.com/dotnet/csharplang/blob/master/proposals/records.md), which are types whose semantic meaning is described by the shape of the data; and pattern matching, which is a new expression form that enables extremely concise multilevel decomposition of these data types.</span></span> <span data-ttu-id="ed402-104">此方法的元素通过编程语言[F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "通过轻型语言进行的可扩展模式匹配")和[Scala](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "与模式匹配的对象")中的相关功能来实现。</span><span class="sxs-lookup"><span data-stu-id="ed402-104">Elements of this approach are inspired by related features in the programming languages [F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "Extensible Pattern Matching Via a Lightweight Language") and [Scala](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "Matching Objects With Patterns").</span></span>

## <a name="is-expression"></a><span data-ttu-id="ed402-105">为 expression</span><span class="sxs-lookup"><span data-stu-id="ed402-105">Is expression</span></span>

<span data-ttu-id="ed402-106">扩展 `is` 运算符以针对*模式*测试表达式。</span><span class="sxs-lookup"><span data-stu-id="ed402-106">The `is` operator is extended to test an expression against a *pattern*.</span></span>

```antlr
relational_expression
    : relational_expression 'is' pattern
    ;
```

<span data-ttu-id="ed402-107">这种形式的*relational_expression*除了C#规范中的现有窗体外。</span><span class="sxs-lookup"><span data-stu-id="ed402-107">This form of *relational_expression* is in addition to the existing forms in the C# specification.</span></span> <span data-ttu-id="ed402-108">如果 `is` 标记左侧的*relational_expression*未指定值或没有类型，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ed402-108">It is a compile-time error if the *relational_expression* to the left of the `is` token does not designate a value or does not have a type.</span></span>

<span data-ttu-id="ed402-109">该模式的每个*标识符*都引入了一个新的局部变量，该局部变量在 `is` 运算符 *`true` 后（* 即，*如果为 true，则为明确赋值*）。</span><span class="sxs-lookup"><span data-stu-id="ed402-109">Every *identifier* of the pattern introduces a new local variable that is *definitely assigned* after the `is` operator is `true` (i.e. *definitely assigned when true*).</span></span>

> <span data-ttu-id="ed402-110">注意：在技术上，`is-expression` 和*constant_pattern*中的*类型*之间存在二义性，其中可能是限定标识符的有效分析。</span><span class="sxs-lookup"><span data-stu-id="ed402-110">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="ed402-111">我们尝试将其绑定为类型以与以前版本的语言兼容;只有当这种情况失败时，我们才会将其解析为在其他上下文中执行的操作（必须是常量或类型）。</span><span class="sxs-lookup"><span data-stu-id="ed402-111">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="ed402-112">这种歧义仅出现在 `is` 表达式的右侧。</span><span class="sxs-lookup"><span data-stu-id="ed402-112">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

## <a name="patterns"></a><span data-ttu-id="ed402-113">模式</span><span class="sxs-lookup"><span data-stu-id="ed402-113">Patterns</span></span>

<span data-ttu-id="ed402-114">模式在 `is` 运算符和*switch_statement*中用于表示要比较传入数据的数据的形状。</span><span class="sxs-lookup"><span data-stu-id="ed402-114">Patterns are used in the `is` operator and in a *switch_statement* to express the shape of data against which incoming data is to be compared.</span></span> <span data-ttu-id="ed402-115">模式可以是递归的，以便可以将部分数据与子模式进行匹配。</span><span class="sxs-lookup"><span data-stu-id="ed402-115">Patterns may be recursive so that parts of the data may be matched against sub-patterns.</span></span>

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    ;

declaration_pattern
    : type simple_designation
    ;

constant_pattern
    : shift_expression
    ;

var_pattern
    : 'var' simple_designation
    ;
```

> <span data-ttu-id="ed402-116">注意：在技术上，`is-expression` 和*constant_pattern*中的*类型*之间存在二义性，其中可能是限定标识符的有效分析。</span><span class="sxs-lookup"><span data-stu-id="ed402-116">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="ed402-117">我们尝试将其绑定为类型以与以前版本的语言兼容;只有当这种情况失败时，我们才会将其解析为在其他上下文中执行的操作（必须是常量或类型）。</span><span class="sxs-lookup"><span data-stu-id="ed402-117">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="ed402-118">这种歧义仅出现在 `is` 表达式的右侧。</span><span class="sxs-lookup"><span data-stu-id="ed402-118">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

### <a name="declaration-pattern"></a><span data-ttu-id="ed402-119">声明模式</span><span class="sxs-lookup"><span data-stu-id="ed402-119">Declaration pattern</span></span>

<span data-ttu-id="ed402-120">如果测试成功，则*declaration_pattern*两个测试表达式是否为给定的类型并将其转换为该类型。</span><span class="sxs-lookup"><span data-stu-id="ed402-120">The *declaration_pattern* both tests that an expression is of a given type and casts it to that type if the test succeeds.</span></span> <span data-ttu-id="ed402-121">如果*simple_designation*是一个标识符，则会引入给定标识符的给定类型的局部变量。</span><span class="sxs-lookup"><span data-stu-id="ed402-121">If the *simple_designation* is an identifier, it introduces a local variable of the given type named by the given identifier.</span></span> <span data-ttu-id="ed402-122">当模式匹配操作的结果为 true 时，将*明确分配*该局部变量。</span><span class="sxs-lookup"><span data-stu-id="ed402-122">That local variable is *definitely assigned* when the result of the pattern-matching operation is true.</span></span>

```antlr
declaration_pattern
    : type simple_designation
    ;
```

<span data-ttu-id="ed402-123">此表达式的运行时语义是根据模式中的*类型*测试左侧*relational_expression*操作数的运行时类型。</span><span class="sxs-lookup"><span data-stu-id="ed402-123">The runtime semantic of this expression is that it tests the runtime type of the left-hand *relational_expression* operand against the *type* in the pattern.</span></span> <span data-ttu-id="ed402-124">如果它属于此运行时类型（或某些子类型），则 `true``is operator` 的结果。</span><span class="sxs-lookup"><span data-stu-id="ed402-124">If it is of that runtime type (or some subtype), the result of the `is operator` is `true`.</span></span> <span data-ttu-id="ed402-125">它声明一个由标识符命名的新局部变量，该*标识符*在结果 `true`时赋给左操作数的值。</span><span class="sxs-lookup"><span data-stu-id="ed402-125">It declares a new local variable named by the *identifier* that is assigned the value of the left-hand operand when the result is `true`.</span></span>

<span data-ttu-id="ed402-126">左侧和给定类型的静态类型的某些组合被视为不兼容并导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ed402-126">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="ed402-127">如果存在标识转换、隐式引用转换、装箱转换、显式引用转换或从 `E` 到 `T`的取消装箱转换，则将静态类型 `E` 的值称为*模式 `T` 兼容*。</span><span class="sxs-lookup"><span data-stu-id="ed402-127">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`.</span></span> <span data-ttu-id="ed402-128">如果类型 `E` 的表达式不与它所匹配的类型模式中的类型兼容，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ed402-128">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

> <span data-ttu-id="ed402-129">注意：[在C# 7.1 中，我们会扩展此项](../csharp-7.1/generics-pattern-match.md)，以便在输入类型或类型 `T` 是开放类型时允许模式匹配操作。</span><span class="sxs-lookup"><span data-stu-id="ed402-129">Note: [In C# 7.1 we extend this](../csharp-7.1/generics-pattern-match.md) to permit a pattern-matching operation if either the input type or the type `T` is an open type.</span></span> <span data-ttu-id="ed402-130">此段落替换为以下内容：</span><span class="sxs-lookup"><span data-stu-id="ed402-130">This paragraph is replaced by the following:</span></span>
> 
> <span data-ttu-id="ed402-131">左侧和给定类型的静态类型的某些组合被视为不兼容并导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ed402-131">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="ed402-132">如果存在标识转换、隐式引用转换、装箱转换、显式引用转换或从 `E` 到 `T`的取消装箱转换，**或者 `E` 或 `T` 是开放类型**，则将静态类型 `E` 的值视为与类型 `T`*模式兼容*。</span><span class="sxs-lookup"><span data-stu-id="ed402-132">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`, **or if either `E` or `T` is an open type**.</span></span> <span data-ttu-id="ed402-133">如果类型 `E` 的表达式不与它所匹配的类型模式中的类型兼容，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ed402-133">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

<span data-ttu-id="ed402-134">声明模式对于执行引用类型的运行时类型测试非常有用，并且替换了方法</span><span class="sxs-lookup"><span data-stu-id="ed402-134">The declaration pattern is useful for performing run-time type tests of reference types, and replaces the idiom</span></span>

```csharp
var v = expr as Type;
if (v != null) { // code using v }
```

<span data-ttu-id="ed402-135">稍微简单一些</span><span class="sxs-lookup"><span data-stu-id="ed402-135">With the slightly more concise</span></span>

```csharp
if (expr is Type v) { // code using v }
```

<span data-ttu-id="ed402-136">如果*类型*是可以为 null 的值类型，则是错误的。</span><span class="sxs-lookup"><span data-stu-id="ed402-136">It is an error if *type* is a nullable value type.</span></span>

<span data-ttu-id="ed402-137">声明模式可用于测试可为 null 的类型的值：类型为 `Nullable<T>` （或装箱 `T`）的值与类型模式匹配 `T2 id` 如果该值不为 null，并且 `T2` 的类型为 `T`或 `T`的某些基类型或接口。</span><span class="sxs-lookup"><span data-stu-id="ed402-137">The declaration pattern can be used to test values of nullable types: a value of type `Nullable<T>` (or a boxed `T`) matches a type pattern `T2 id` if the value is non-null and the type of `T2` is `T`, or some base type or interface of `T`.</span></span> <span data-ttu-id="ed402-138">例如，在代码片段中</span><span class="sxs-lookup"><span data-stu-id="ed402-138">For example, in the code fragment</span></span>

```csharp
int? x = 3;
if (x is int v) { // code using v }
```

<span data-ttu-id="ed402-139">在运行时 `true` `if` 语句的条件，`v` 在块内保存类型 `int` 的值 `3`。</span><span class="sxs-lookup"><span data-stu-id="ed402-139">The condition of the `if` statement is `true` at runtime and the variable `v` holds the value `3` of type `int` inside the block.</span></span>

### <a name="constant-pattern"></a><span data-ttu-id="ed402-140">常量模式</span><span class="sxs-lookup"><span data-stu-id="ed402-140">Constant pattern</span></span>

```antlr
constant_pattern
    : shift_expression
    ;
```

<span data-ttu-id="ed402-141">常数模式根据常数值测试表达式的值。</span><span class="sxs-lookup"><span data-stu-id="ed402-141">A constant pattern tests the value of an expression against a constant value.</span></span> <span data-ttu-id="ed402-142">常数可以是任意常量表达式，如文本、声明的 `const` 变量的名称、枚举常量或 `typeof` 表达式。</span><span class="sxs-lookup"><span data-stu-id="ed402-142">The constant may be any constant expression, such as a literal, the name of a declared `const` variable, or an enumeration constant, or a `typeof` expression.</span></span>

<span data-ttu-id="ed402-143">如果*e*和*c*均为整型类型，则当 `true`表达式 `e == c` 的结果时，模式将被视为匹配。</span><span class="sxs-lookup"><span data-stu-id="ed402-143">If both *e* and *c* are of integral types, the pattern is considered matched if the result of the expression `e == c` is `true`.</span></span>

<span data-ttu-id="ed402-144">否则，如果 `object.Equals(e, c)` 返回 `true`模式，则视为匹配。</span><span class="sxs-lookup"><span data-stu-id="ed402-144">Otherwise the pattern is considered matching if `object.Equals(e, c)` returns `true`.</span></span> <span data-ttu-id="ed402-145">在这种情况下，如果*e*的静态类型与常量的类型不*兼容*，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ed402-145">In this case it is a compile-time error if the static type of *e* is not *pattern compatible* with the type of the constant.</span></span>

### <a name="var-pattern"></a><span data-ttu-id="ed402-146">Var 模式</span><span class="sxs-lookup"><span data-stu-id="ed402-146">Var pattern</span></span>

```antlr
var_pattern
    : 'var' simple_designation
    ;
```

<span data-ttu-id="ed402-147">表达式*e*与*var_pattern*总是匹配。</span><span class="sxs-lookup"><span data-stu-id="ed402-147">An expression *e* matches a *var_pattern* always.</span></span> <span data-ttu-id="ed402-148">换言之，与*var 模式*匹配始终会成功。</span><span class="sxs-lookup"><span data-stu-id="ed402-148">In other words, a match to a *var pattern* always succeeds.</span></span> <span data-ttu-id="ed402-149">如果*simple_designation*是标识符，则在运行时， *e*的值将绑定到新引入的局部变量。</span><span class="sxs-lookup"><span data-stu-id="ed402-149">If the *simple_designation* is an identifier, then at runtime the value of *e* is bound to a newly introduced local variable.</span></span> <span data-ttu-id="ed402-150">局部变量的类型为*e*的静态类型。</span><span class="sxs-lookup"><span data-stu-id="ed402-150">The type of the local variable is the static type of *e*.</span></span>

<span data-ttu-id="ed402-151">如果名称 `var` 绑定到类型，则是错误的。</span><span class="sxs-lookup"><span data-stu-id="ed402-151">It is an error if the name `var` binds to a type.</span></span>

## <a name="switch-statement"></a><span data-ttu-id="ed402-152">Switch 语句</span><span class="sxs-lookup"><span data-stu-id="ed402-152">Switch statement</span></span>

<span data-ttu-id="ed402-153">扩展了 `switch` 语句，以选择执行第一个具有与*switch 表达式*相匹配的模式的块。</span><span class="sxs-lookup"><span data-stu-id="ed402-153">The `switch` statement is extended to select for execution the first block having an associated pattern that matches the *switch expression*.</span></span>

```antlr
switch_label
    : 'case' complex_pattern case_guard? ':'
    | 'case' constant_expression case_guard? ':'
    | 'default' ':'
    ;

case_guard
    : 'when' expression
    ;
```

<span data-ttu-id="ed402-154">未定义模式匹配的顺序。</span><span class="sxs-lookup"><span data-stu-id="ed402-154">The order in which patterns are matched is not defined.</span></span> <span data-ttu-id="ed402-155">允许编译器按顺序匹配模式，并重复使用已匹配的模式的结果来计算其他模式的匹配结果。</span><span class="sxs-lookup"><span data-stu-id="ed402-155">A compiler is permitted to match patterns out of order, and to reuse the results of already matched patterns to compute the result of matching of other patterns.</span></span>

<span data-ttu-id="ed402-156">如果存在*case 保护*，则其表达式的类型为 `bool`。</span><span class="sxs-lookup"><span data-stu-id="ed402-156">If a *case-guard* is present, its expression is of type `bool`.</span></span> <span data-ttu-id="ed402-157">它作为附加条件进行评估，此条件必须满足，才能认为需要满足此条件。</span><span class="sxs-lookup"><span data-stu-id="ed402-157">It is evaluated as an additional condition that must be satisfied for the case to be considered satisfied.</span></span>

<span data-ttu-id="ed402-158">如果*switch_label*在运行时不起作用，则是错误的，因为在以前的情况下，其模式归入。</span><span class="sxs-lookup"><span data-stu-id="ed402-158">It is an error if a *switch_label* can have no effect at runtime because its pattern is subsumed by previous cases.</span></span> <span data-ttu-id="ed402-159">[TODO：我们应该更精确地了解编译器要求使用哪种方法才能达到这一判断。]</span><span class="sxs-lookup"><span data-stu-id="ed402-159">[TODO: We should be more precise about the techniques the compiler is required to use to reach this judgment.]</span></span>

<span data-ttu-id="ed402-160">当且仅当该事例块精确包含一个*switch_label*时，在*switch_label*中声明的模式变量将在其 case 块中明确赋值。</span><span class="sxs-lookup"><span data-stu-id="ed402-160">A pattern variable declared in a *switch_label* is definitely assigned in its case block if and only if that case block contains precisely one *switch_label*.</span></span>

<span data-ttu-id="ed402-161">[TODO：应指定何时可以访问*switch 块*。]</span><span class="sxs-lookup"><span data-stu-id="ed402-161">[TODO: We should specify when a *switch block* is reachable.]</span></span>

### <a name="scope-of-pattern-variables"></a><span data-ttu-id="ed402-162">模式变量的范围</span><span class="sxs-lookup"><span data-stu-id="ed402-162">Scope of pattern variables</span></span>

<span data-ttu-id="ed402-163">在模式中声明的变量的作用域如下：</span><span class="sxs-lookup"><span data-stu-id="ed402-163">The scope of a variable declared in a pattern is as follows:</span></span>

- <span data-ttu-id="ed402-164">如果模式为 case 标签，则该变量的作用域为*事例块*。</span><span class="sxs-lookup"><span data-stu-id="ed402-164">If the pattern is a case label, then the scope of the variable is the *case block*.</span></span>

<span data-ttu-id="ed402-165">否则，将在*is_pattern*表达式中声明变量，并且其作用域基于直接包含*is_pattern*表达式的表达式的构造，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ed402-165">Otherwise the variable is declared in an *is_pattern* expression, and its scope is based on the construct immediately enclosing the expression containing the *is_pattern* expression as follows:</span></span>

- <span data-ttu-id="ed402-166">如果表达式位于 expression-bodied lambda 表达式中，则其作用域为 lambda 的主体。</span><span class="sxs-lookup"><span data-stu-id="ed402-166">If the expression is in an expression-bodied lambda, its scope is the body of the lambda.</span></span>
- <span data-ttu-id="ed402-167">如果表达式在 expression-bodied 方法或属性中，则它的作用域是方法或属性的主体。</span><span class="sxs-lookup"><span data-stu-id="ed402-167">If the expression is in an expression-bodied method or property, its scope is the body of the method or property.</span></span>
- <span data-ttu-id="ed402-168">如果表达式位于 `catch` 子句的 `when` 子句中，则该表达式的作用域是 `catch` 的子句。</span><span class="sxs-lookup"><span data-stu-id="ed402-168">If the expression is in a `when` clause of a `catch` clause, its scope is that `catch` clause.</span></span>
- <span data-ttu-id="ed402-169">如果表达式位于*iteration_statement*中，则它的作用域只是该语句。</span><span class="sxs-lookup"><span data-stu-id="ed402-169">If the expression is in an *iteration_statement*, its scope is just that statement.</span></span>
- <span data-ttu-id="ed402-170">否则，如果表达式是在其他语句形式中，则它的作用域是包含语句的作用域。</span><span class="sxs-lookup"><span data-stu-id="ed402-170">Otherwise if the expression is in some other statement form, its scope is the scope containing the statement.</span></span>

<span data-ttu-id="ed402-171">为了确定作用域， *embedded_statement*被视为位于其自身的作用域中。</span><span class="sxs-lookup"><span data-stu-id="ed402-171">For the purpose of determining the scope, an *embedded_statement* is considered to be in its own scope.</span></span> <span data-ttu-id="ed402-172">例如， *if_statement*的语法为</span><span class="sxs-lookup"><span data-stu-id="ed402-172">For example, the grammar for an *if_statement* is</span></span>

``` antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="ed402-173">因此，如果*if_statement*的受控语句声明一个模式变量，则其作用域限制为该*embedded_statement*：</span><span class="sxs-lookup"><span data-stu-id="ed402-173">So if the controlled statement of an *if_statement* declares a pattern variable, its scope is restricted to that *embedded_statement*:</span></span>

```csharp
if (x) M(y is var z);
```

<span data-ttu-id="ed402-174">在这种情况下，`z` 的作用域为嵌入的语句 `M(y is var z);`。</span><span class="sxs-lookup"><span data-stu-id="ed402-174">In this case the scope of `z` is the embedded statement `M(y is var z);`.</span></span>

<span data-ttu-id="ed402-175">其他情况是出于其他原因（例如，在参数的默认值或属性中）错误，这两者都是错误，因为这些上下文需要常量表达式。</span><span class="sxs-lookup"><span data-stu-id="ed402-175">Other cases are errors for other reasons (e.g. in a parameter's default value or an attribute, both of which are an error because those contexts require a constant expression).</span></span>

> <span data-ttu-id="ed402-176">[在C# 7.3 中，我们添加了以下上下文](../csharp-7.3/expression-variables-in-initializers.md)，可在其中声明模式变量：</span><span class="sxs-lookup"><span data-stu-id="ed402-176">[In C# 7.3 we added the following contexts](../csharp-7.3/expression-variables-in-initializers.md) in which a pattern variable may be declared:</span></span>
> - <span data-ttu-id="ed402-177">如果表达式在*构造函数初始值设定项*中，则其作用域为*构造函数初始值设定项*和构造函数的主体。</span><span class="sxs-lookup"><span data-stu-id="ed402-177">If the expression is in a *constructor initializer*, its scope is the *constructor initializer* and the constructor's body.</span></span>
> - <span data-ttu-id="ed402-178">如果表达式在字段初始值设定项中，其作用域就是它出现的*equals_value_clause* 。</span><span class="sxs-lookup"><span data-stu-id="ed402-178">If the expression is in a field initializer, its scope is the *equals_value_clause* in which it appears.</span></span>
> - <span data-ttu-id="ed402-179">如果表达式所在的查询子句所指定的值将转换为 lambda 体，则该表达式的作用域只是该表达式。</span><span class="sxs-lookup"><span data-stu-id="ed402-179">If the expression is in a query clause that is specified to be translated into the body of a lambda, its scope is just that expression.</span></span>

## <a name="changes-to-syntactic-disambiguation"></a><span data-ttu-id="ed402-180">对句法歧义的更改</span><span class="sxs-lookup"><span data-stu-id="ed402-180">Changes to syntactic disambiguation</span></span>

<span data-ttu-id="ed402-181">在某些情况下，出现语法C#不明确的情况，语言规范说明了如何解决这些歧义：</span><span class="sxs-lookup"><span data-stu-id="ed402-181">There are situations involving generics where the C# grammar is ambiguous, and the language spec says how to resolve those ambiguities:</span></span>

> #### <a name="7652-grammar-ambiguities"></a><span data-ttu-id="ed402-182">7.6.5.2 语法多义性</span><span class="sxs-lookup"><span data-stu-id="ed402-182">7.6.5.2 Grammar ambiguities</span></span>
> <span data-ttu-id="ed402-183">*简单名称*（第7.6.3）和*成员访问*（第7.6.5）的生产可以在表达式的语法中产生歧义。</span><span class="sxs-lookup"><span data-stu-id="ed402-183">The productions for *simple-name* (§7.6.3) and *member-access* (§7.6.5) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="ed402-184">例如，语句：</span><span class="sxs-lookup"><span data-stu-id="ed402-184">For example, the statement:</span></span>
> ```csharp
> F(G<A,B>(7));
> ```
> <span data-ttu-id="ed402-185">可以解释为对具有两个自变量的 `F` 的调用 `G < A` 和 `B > (7)`。</span><span class="sxs-lookup"><span data-stu-id="ed402-185">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="ed402-186">或者，它可以解释为对 `F` 的调用，该参数是一个参数，它是对具有两个类型参数和一个正则参数的泛型方法的调用 `G`。</span><span class="sxs-lookup"><span data-stu-id="ed402-186">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

> <span data-ttu-id="ed402-187">如果可以将标记序列（在上下文中）作为*简单名称*（第7.6.3）、*成员访问*（第7.6.5）或*指针-成员访问*（第18.5.2）以*类型参数列表*（第4.4.1）结尾，则将检查紧跟在结束 `>` 标记后面的标记。</span><span class="sxs-lookup"><span data-stu-id="ed402-187">If a sequence of tokens can be parsed (in context) as a *simple-name* (§7.6.3), *member-access* (§7.6.5), or *pointer-member-access* (§18.5.2) ending with a *type-argument-list* (§4.4.1), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="ed402-188">如果是</span><span class="sxs-lookup"><span data-stu-id="ed402-188">If it is one of</span></span>
> ```none
> (  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
> ```
> <span data-ttu-id="ed402-189">然后，将*类型参数列表*保留为*简单名称*、*成员访问*或*指针成员访问*的一部分，并且会丢弃标记序列的任何其他可能分析。</span><span class="sxs-lookup"><span data-stu-id="ed402-189">then the *type-argument-list* is retained as part of the *simple-name*, *member-access* or *pointer-member-access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="ed402-190">否则，*类型参数列表*不会被视为*简单名称*、*成员访问*或 >*指针成员访问*的一部分，即使没有其他可能的标记序列分析也是如此。</span><span class="sxs-lookup"><span data-stu-id="ed402-190">Otherwise, the *type-argument-list* is not considered to be part of the *simple-name*, *member-access* or > *pointer-member-access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="ed402-191">请注意，当在*命名空间或类型名称*中分析*类型参数列表*时，不应用这些规则（第3.8 节）。</span><span class="sxs-lookup"><span data-stu-id="ed402-191">Note that these rules are not applied when parsing a *type-argument-list* in a *namespace-or-type-name* (§3.8).</span></span> <span data-ttu-id="ed402-192">语句</span><span class="sxs-lookup"><span data-stu-id="ed402-192">The statement</span></span>
> ```csharp
> F(G<A,B>(7));
> ```
> <span data-ttu-id="ed402-193">根据此规则，将解释为对 `F` 的调用，该参数是一个参数，它是对具有两个类型参数和一个正则参数的泛型方法的调用 `G`。</span><span class="sxs-lookup"><span data-stu-id="ed402-193">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="ed402-194">语句</span><span class="sxs-lookup"><span data-stu-id="ed402-194">The statements</span></span>
> ```csharp
> F(G < A, B > 7);
> F(G < A, B >> 7);
> ```
> <span data-ttu-id="ed402-195">每个都将解释为对具有两个自变量的 `F` 的调用。</span><span class="sxs-lookup"><span data-stu-id="ed402-195">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="ed402-196">语句</span><span class="sxs-lookup"><span data-stu-id="ed402-196">The statement</span></span>
> ```csharp
> x = F < A > +y;
> ```
> <span data-ttu-id="ed402-197">将被解释为小于运算符、大于运算符和一元正运算符，就如同语句已写入 `x = (F < A) > (+y)`，而不是作为带有后跟二元加法运算符的*类型参数列表*的*简单名称*。</span><span class="sxs-lookup"><span data-stu-id="ed402-197">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple-name* with a *type-argument-list* followed by a binary plus operator.</span></span> <span data-ttu-id="ed402-198">在语句中</span><span class="sxs-lookup"><span data-stu-id="ed402-198">In the statement</span></span>
> ```csharp
> x = y is C<T> + z;
> ```
> <span data-ttu-id="ed402-199">标记 `C<T>` 被解释为带有*类型参数列表*的*命名空间或类型名称*。</span><span class="sxs-lookup"><span data-stu-id="ed402-199">the tokens `C<T>` are interpreted as a *namespace-or-type-name* with a *type-argument-list*.</span></span>

<span data-ttu-id="ed402-200">7中C#引入了许多更改，这使得这些歧义消除规则不再足以处理语言的复杂性。</span><span class="sxs-lookup"><span data-stu-id="ed402-200">There are a number of changes being introduced in C# 7 that make these disambiguation rules no longer sufficient to handle the complexity of the language.</span></span>

### <a name="out-variable-declarations"></a><span data-ttu-id="ed402-201">Out 变量声明</span><span class="sxs-lookup"><span data-stu-id="ed402-201">Out variable declarations</span></span>

<span data-ttu-id="ed402-202">现在可以在 out 参数中声明变量：</span><span class="sxs-lookup"><span data-stu-id="ed402-202">It is now possible to declare a variable in an out argument:</span></span>

```csharp
M(out Type name);
```

<span data-ttu-id="ed402-203">但类型可以是泛型：</span><span class="sxs-lookup"><span data-stu-id="ed402-203">However, the type may be generic:</span></span> 

```csharp
M(out A<B> name);
```

<span data-ttu-id="ed402-204">由于参数的语言语法使用*expression*，因此此上下文服从消除规则。</span><span class="sxs-lookup"><span data-stu-id="ed402-204">Since the language grammar for the argument uses *expression*, this context is subject to the disambiguation rule.</span></span> <span data-ttu-id="ed402-205">在这种情况下，关闭 `>` 后跟一个标识符，该*标识符*不是允许它被视为*类型参数列表*的标记之一。</span><span class="sxs-lookup"><span data-stu-id="ed402-205">In this case the closing `>` is followed by an *identifier*, which is not one of the tokens that permits it to be treated as a *type-argument-list*.</span></span> <span data-ttu-id="ed402-206">因此，我建议**将*标识符*添加到用于触发*类型参数列表*的消除歧义的令牌集。**</span><span class="sxs-lookup"><span data-stu-id="ed402-206">I therefore propose to **add *identifier* to the set of tokens that triggers the disambiguation to a *type-argument-list*.**</span></span>

### <a name="tuples-and-deconstruction-declarations"></a><span data-ttu-id="ed402-207">元组和析构声明</span><span class="sxs-lookup"><span data-stu-id="ed402-207">Tuples and deconstruction declarations</span></span>

<span data-ttu-id="ed402-208">元组文本运行完全相同的问题。</span><span class="sxs-lookup"><span data-stu-id="ed402-208">A tuple literal runs into exactly the same issue.</span></span> <span data-ttu-id="ed402-209">考虑元组表达式</span><span class="sxs-lookup"><span data-stu-id="ed402-209">Consider the tuple expression</span></span>

```csharp
(A < B, C > D, E < F, G > H)
```

<span data-ttu-id="ed402-210">在用于分析C#参数列表的旧6个规则下，这会分析为具有四个元素的元组，从 `A < B` 开始。</span><span class="sxs-lookup"><span data-stu-id="ed402-210">Under the old C# 6 rules for parsing an argument list, this would parse as a tuple with four elements, starting with `A < B` as the first.</span></span> <span data-ttu-id="ed402-211">但是，如果在析构的左侧出现这种情况，我们需要由*标识符*标记触发的歧义消除，如上所述：</span><span class="sxs-lookup"><span data-stu-id="ed402-211">However, when this appears on the left of a deconstruction, we want the disambiguation triggered by the *identifier* token as described above:</span></span>

```csharp
(A<B,C> D, E<F,G> H) = e;
```

<span data-ttu-id="ed402-212">这是声明两个变量的析构声明，其中第一个变量的类型为 `A<B,C>` 和命名 `D`。</span><span class="sxs-lookup"><span data-stu-id="ed402-212">This is a deconstruction declaration which declares two variables, the first of which is of type `A<B,C>` and named `D`.</span></span> <span data-ttu-id="ed402-213">换言之，元组文本包含两个表达式，其中每个表达式都是一个声明表达式。</span><span class="sxs-lookup"><span data-stu-id="ed402-213">In other words, the tuple literal contains two expressions, each of which is a declaration expression.</span></span>

<span data-ttu-id="ed402-214">为了简化规范和编译器，我建议将此元组文本分析为双元素元组（无论它是否显示在赋值的左侧）中。</span><span class="sxs-lookup"><span data-stu-id="ed402-214">For simplicity of the specification and compiler, I propose that this tuple literal be parsed as a two-element tuple wherever it appears (whether or not it appears on the left-hand-side of an assignment).</span></span> <span data-ttu-id="ed402-215">这是上一节中所述歧义消除的自然结果。</span><span class="sxs-lookup"><span data-stu-id="ed402-215">That would be a natural result of the disambiguation described in the previous section.</span></span>

### <a name="pattern-matching"></a><span data-ttu-id="ed402-216">模式匹配</span><span class="sxs-lookup"><span data-stu-id="ed402-216">Pattern-matching</span></span>

<span data-ttu-id="ed402-217">模式匹配会引入一个新的上下文，在此上下文中会出现表达式类型的多义性。</span><span class="sxs-lookup"><span data-stu-id="ed402-217">Pattern matching introduces a new context where the expression-type ambiguity arises.</span></span> <span data-ttu-id="ed402-218">之前，`is` 运算符的右边是一个类型。</span><span class="sxs-lookup"><span data-stu-id="ed402-218">Previously the right-hand-side of an `is` operator was a type.</span></span> <span data-ttu-id="ed402-219">现在，它可以是类型或表达式，如果它是类型，则可以后跟标识符。</span><span class="sxs-lookup"><span data-stu-id="ed402-219">Now it can be a type or expression, and if it is a type it may be followed by an identifier.</span></span> <span data-ttu-id="ed402-220">从技术上讲，可以更改现有代码的含义：</span><span class="sxs-lookup"><span data-stu-id="ed402-220">This can, technically, change the meaning of existing code:</span></span>

```csharp
var x = e is T < A > B;
```

<span data-ttu-id="ed402-221">可在 c # 6 规则中将其分析为</span><span class="sxs-lookup"><span data-stu-id="ed402-221">This could be parsed under C#6 rules as</span></span>

```csharp
var x = ((e is T) < A) > B;
```

<span data-ttu-id="ed402-222">但在 c # 7 规则下（上面建议的歧义消除）将被解析为</span><span class="sxs-lookup"><span data-stu-id="ed402-222">but under under C#7 rules (with the disambiguation proposed above) would be parsed as</span></span>

```csharp
var x = e is T<A> B;
```

<span data-ttu-id="ed402-223">`T<A>`类型的变量 `B` 声明。</span><span class="sxs-lookup"><span data-stu-id="ed402-223">which declares a variable `B` of type `T<A>`.</span></span> <span data-ttu-id="ed402-224">幸运的是，本机编译器和 Roslyn 编译器都有 bug，因此它们给出了 c # 6 代码的语法错误。</span><span class="sxs-lookup"><span data-stu-id="ed402-224">Fortunately, the native and Roslyn compilers have a bug whereby they give a syntax error on the C#6 code.</span></span> <span data-ttu-id="ed402-225">因此，不需要考虑这种特殊的重大更改。</span><span class="sxs-lookup"><span data-stu-id="ed402-225">Therefore this particular breaking change is not a concern.</span></span>

<span data-ttu-id="ed402-226">模式匹配引入了其他标记，这些标记应促进选择类型的多义性解析。</span><span class="sxs-lookup"><span data-stu-id="ed402-226">Pattern-matching introduces additional tokens that should drive the ambiguity resolution toward selecting a type.</span></span> <span data-ttu-id="ed402-227">下面的现有有效 c # 6 代码的示例将中断，无需额外消除规则：</span><span class="sxs-lookup"><span data-stu-id="ed402-227">The following examples of existing valid C#6 code would be broken without additional disambiguation rules:</span></span>

```csharp
var x = e is A<B> && f;            // &&
var x = e is A<B> || f;            // ||
var x = e is A<B> & f;             // &
var x = e is A<B>[];               // [
```

### <a name="proposed-change-to-the-disambiguation-rule"></a><span data-ttu-id="ed402-228">消除歧义规则的建议更改</span><span class="sxs-lookup"><span data-stu-id="ed402-228">Proposed change to the disambiguation rule</span></span>

<span data-ttu-id="ed402-229">我建议修订规范，以更改歧义令牌的列表</span><span class="sxs-lookup"><span data-stu-id="ed402-229">I propose to revise the specification to change the list of disambiguating tokens from</span></span>

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```

<span data-ttu-id="ed402-230">to</span><span class="sxs-lookup"><span data-stu-id="ed402-230">to</span></span>

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [
```

<span data-ttu-id="ed402-231">在某些上下文中，我们将*标识符*视为歧义标记。</span><span class="sxs-lookup"><span data-stu-id="ed402-231">And, in certain contexts, we treat *identifier* as a disambiguating token.</span></span> <span data-ttu-id="ed402-232">这些上下文包括：要消除的标记的序列紧靠某个关键字 `is`、`case`或 `out`，或者分析元组文本的第一个元素（在这种情况下，令牌前面是 `(` 或 `:`，标识符后跟 `,`）或元组文本的后续元素。</span><span class="sxs-lookup"><span data-stu-id="ed402-232">Those contexts are where the sequence of tokens being disambiguated is immediately preceded by one of the keywords `is`, `case`, or `out`, or arises while parsing the first element of a tuple literal (in which case the tokens are preceded by `(` or `:` and the identifier is followed by a `,`) or a subsequent element of a tuple literal.</span></span>

### <a name="modified-disambiguation-rule"></a><span data-ttu-id="ed402-233">修改后消除规则</span><span class="sxs-lookup"><span data-stu-id="ed402-233">Modified disambiguation rule</span></span>

<span data-ttu-id="ed402-234">修改后的消除歧义规则如下所示</span><span class="sxs-lookup"><span data-stu-id="ed402-234">The revised disambiguation rule would be something like this</span></span>

> <span data-ttu-id="ed402-235">如果可以将标记序列（在上下文中）作为*简单名称*（第7.6.3）、*成员访问*（第7.6.5）或*指针-成员访问*（第18.5.2）以*类型参数列表*（第4.4.1）结尾，则将检查紧跟在结束 `>` 标记后面的标记，以查看它是否为</span><span class="sxs-lookup"><span data-stu-id="ed402-235">If a sequence of tokens can be parsed (in context) as a *simple-name* (§7.6.3), *member-access* (§7.6.5), or *pointer-member-access* (§18.5.2) ending with a *type-argument-list* (§4.4.1), the token immediately following the closing `>` token is examined, to see if it is</span></span>
> - <span data-ttu-id="ed402-236">`(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`之一;或</span><span class="sxs-lookup"><span data-stu-id="ed402-236">One of `(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`; or</span></span>
> - <span data-ttu-id="ed402-237">其中一个关系运算符 `<  >  <=  >=  is as`;或</span><span class="sxs-lookup"><span data-stu-id="ed402-237">One of the relational operators `<  >  <=  >=  is as`; or</span></span>
> - <span data-ttu-id="ed402-238">上下文查询关键字显示在查询表达式中;或</span><span class="sxs-lookup"><span data-stu-id="ed402-238">A contextual query keyword appearing inside a query expression; or</span></span>
> - <span data-ttu-id="ed402-239">在某些上下文中，我们将*标识符*视为歧义标记。</span><span class="sxs-lookup"><span data-stu-id="ed402-239">In certain contexts, we treat *identifier* as a disambiguating token.</span></span> <span data-ttu-id="ed402-240">这些上下文包括：要消除的标记的序列紧靠在分析元组文本的第一个元素时 `is`、`case` 或 `out`，或在分析元组文本的第一个元素时（在这种情况下，令牌前面是 `(` 或 `:`，标识符后跟 `,`）或元组文本的后续元素。</span><span class="sxs-lookup"><span data-stu-id="ed402-240">Those contexts are where the sequence of tokens being disambiguated is immediately preceded by one of the keywords `is`, `case` or `out`, or arises while parsing the first element of a tuple literal (in which case the tokens are preceded by `(` or `:` and the identifier is followed by a `,`) or a subsequent element of a tuple literal.</span></span>
> 
> <span data-ttu-id="ed402-241">如果以下标记在此列表中，或在此类上下文中为标识符，则会将*类型参数列表*保留为*简单名称*、*成员访问*或*指针成员访问*的一部分，并且会丢弃标记序列的任何其他可能分析。</span><span class="sxs-lookup"><span data-stu-id="ed402-241">If the following token is among this list, or an identifier in such a context, then the *type-argument-list* is retained as part of the *simple-name*, *member-access* or  *pointer-member-access* and any other possible parse of the sequence of tokens is discarded.</span></span>  <span data-ttu-id="ed402-242">否则，*类型参数列表*不被视为*简单名称*、*成员访问*或*指针访问权限*的一部分，即使没有其他可能的标记序列分析也是如此。</span><span class="sxs-lookup"><span data-stu-id="ed402-242">Otherwise, the *type-argument-list* is not considered to be part of the *simple-name*, *member-access* or *pointer-member-access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="ed402-243">请注意，当在*命名空间或类型名称*中分析*类型参数列表*时，不应用这些规则（第3.8 节）。</span><span class="sxs-lookup"><span data-stu-id="ed402-243">Note that these rules are not applied when parsing a *type-argument-list* in a *namespace-or-type-name* (§3.8).</span></span>

### <a name="breaking-changes-due-to-this-proposal"></a><span data-ttu-id="ed402-244">由于此建议造成的重大更改</span><span class="sxs-lookup"><span data-stu-id="ed402-244">Breaking changes due to this proposal</span></span>

<span data-ttu-id="ed402-245">由于此建议的消除歧义规则，不能识别重大更改。</span><span class="sxs-lookup"><span data-stu-id="ed402-245">No breaking changes are known due to this proposed disambiguation rule.</span></span>

### <a name="interesting-examples"></a><span data-ttu-id="ed402-246">有趣的示例</span><span class="sxs-lookup"><span data-stu-id="ed402-246">Interesting examples</span></span>

<span data-ttu-id="ed402-247">下面是这些歧义消除规则的一些有趣的结果：</span><span class="sxs-lookup"><span data-stu-id="ed402-247">Here are some interesting results of these disambiguation rules:</span></span>

<span data-ttu-id="ed402-248">表达式 `(A < B, C > D)` 是具有两个元素的元组，每个元素都是比较。</span><span class="sxs-lookup"><span data-stu-id="ed402-248">The expression `(A < B, C > D)` is a tuple with two elements, each a comparison.</span></span>

<span data-ttu-id="ed402-249">表达式 `(A<B,C> D, E)` 是具有两个元素的元组，其中第一个元素是声明表达式。</span><span class="sxs-lookup"><span data-stu-id="ed402-249">The expression `(A<B,C> D, E)` is a tuple with two elements, the first of which is a declaration expression.</span></span>

<span data-ttu-id="ed402-250">调用 `M(A < B, C > D, E)` 具有三个参数。</span><span class="sxs-lookup"><span data-stu-id="ed402-250">The invocation `M(A < B, C > D, E)` has three arguments.</span></span>

<span data-ttu-id="ed402-251">调用 `M(out A<B,C> D, E)` 有两个参数，第一个参数是 `out` 声明。</span><span class="sxs-lookup"><span data-stu-id="ed402-251">The invocation `M(out A<B,C> D, E)` has two arguments, the first of which is an `out` declaration.</span></span>

<span data-ttu-id="ed402-252">`e is A<B> C` 使用声明表达式的表达式。</span><span class="sxs-lookup"><span data-stu-id="ed402-252">The expression `e is A<B> C` uses a declaration expression.</span></span>

<span data-ttu-id="ed402-253">Case 标签 `case A<B> C:` 使用声明表达式。</span><span class="sxs-lookup"><span data-stu-id="ed402-253">The case label `case A<B> C:` uses a declaration expression.</span></span>

## <a name="some-examples-of-pattern-matching"></a><span data-ttu-id="ed402-254">模式匹配的一些示例</span><span class="sxs-lookup"><span data-stu-id="ed402-254">Some examples of pattern matching</span></span>

### <a name="is-as"></a><span data-ttu-id="ed402-255">为-As</span><span class="sxs-lookup"><span data-stu-id="ed402-255">Is-As</span></span>

<span data-ttu-id="ed402-256">我们可以替换用法</span><span class="sxs-lookup"><span data-stu-id="ed402-256">We can replace the idiom</span></span>

```csharp
var v = expr as Type;   
if (v != null) {
    // code using v
}
```

<span data-ttu-id="ed402-257">稍微简单、直接</span><span class="sxs-lookup"><span data-stu-id="ed402-257">With the slightly more concise and direct</span></span>

```csharp
if (expr is Type v) {
    // code using v
}
```

### <a name="testing-nullable"></a><span data-ttu-id="ed402-258">测试可为 null</span><span class="sxs-lookup"><span data-stu-id="ed402-258">Testing nullable</span></span>

<span data-ttu-id="ed402-259">我们可以替换用法</span><span class="sxs-lookup"><span data-stu-id="ed402-259">We can replace the idiom</span></span>

```csharp
Type? v = x?.y?.z;
if (v.HasValue) {
    var value = v.GetValueOrDefault();
    // code using value
}
```

<span data-ttu-id="ed402-260">稍微简单、直接</span><span class="sxs-lookup"><span data-stu-id="ed402-260">With the slightly more concise and direct</span></span>

```csharp
if (x?.y?.z is Type value) {
    // code using value
}
```

### <a name="arithmetic-simplification"></a><span data-ttu-id="ed402-261">算术简化</span><span class="sxs-lookup"><span data-stu-id="ed402-261">Arithmetic simplification</span></span>

<span data-ttu-id="ed402-262">假设我们定义一组递归类型来表示表达式（每个单独的提议）：</span><span class="sxs-lookup"><span data-stu-id="ed402-262">Suppose we define a set of recursive types to represent expressions (per a separate proposal):</span></span>

```csharp
abstract class Expr;
class X() : Expr;
class Const(double Value) : Expr;
class Add(Expr Left, Expr Right) : Expr;
class Mult(Expr Left, Expr Right) : Expr;
class Neg(Expr Value) : Expr;
```

<span data-ttu-id="ed402-263">现在，我们可以定义函数来计算表达式的（unreduced）导数：</span><span class="sxs-lookup"><span data-stu-id="ed402-263">Now we can define a function to compute the (unreduced) derivative of an expression:</span></span>

```csharp
Expr Deriv(Expr e)
{
  switch (e) {
    case X(): return Const(1);
    case Const(*): return Const(0);
    case Add(var Left, var Right):
      return Add(Deriv(Left), Deriv(Right));
    case Mult(var Left, var Right):
      return Add(Mult(Deriv(Left), Right), Mult(Left, Deriv(Right)));
    case Neg(var Value):
      return Neg(Deriv(Value));
  }
}
```

<span data-ttu-id="ed402-264">表达式 simplifier 演示位置模式：</span><span class="sxs-lookup"><span data-stu-id="ed402-264">An expression simplifier demonstrates positional patterns:</span></span>

```csharp
Expr Simplify(Expr e)
{
  switch (e) {
    case Mult(Const(0), *): return Const(0);
    case Mult(*, Const(0)): return Const(0);
    case Mult(Const(1), var x): return Simplify(x);
    case Mult(var x, Const(1)): return Simplify(x);
    case Mult(Const(var l), Const(var r)): return Const(l*r);
    case Add(Const(0), var x): return Simplify(x);
    case Add(var x, Const(0)): return Simplify(x);
    case Add(Const(var l), Const(var r)): return Const(l+r);
    case Neg(Const(var k)): return Const(-k);
    default: return e;
  }
}
```
