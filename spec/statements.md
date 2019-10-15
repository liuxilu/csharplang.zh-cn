---
ms.openlocfilehash: 7248a91976c479dc1b6b64b799639635617a7bec
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704047"
---
# <a name="statements"></a><span data-ttu-id="efddf-101">语句</span><span class="sxs-lookup"><span data-stu-id="efddf-101">Statements</span></span>

<span data-ttu-id="efddf-102">C#提供各种语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-102">C# provides a variety of statements.</span></span> <span data-ttu-id="efddf-103">对于在 C 和C++中进行了编程的开发人员，这些语句中的大部分将很熟悉。</span><span class="sxs-lookup"><span data-stu-id="efddf-103">Most of these statements will be familiar to developers who have programmed in C and C++.</span></span>

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

<span data-ttu-id="efddf-104">*Embedded_statement*非终止符用于在其他语句中显示的语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-104">The *embedded_statement* nonterminal is used for statements that appear within other statements.</span></span> <span data-ttu-id="efddf-105">使用*embedded_statement*而不是*语句*不包括在这些上下文中声明语句和标记语句的使用。</span><span class="sxs-lookup"><span data-stu-id="efddf-105">The use of *embedded_statement* rather than *statement* excludes the use of declaration statements and labeled statements in these contexts.</span></span> <span data-ttu-id="efddf-106">示例</span><span class="sxs-lookup"><span data-stu-id="efddf-106">The example</span></span>
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
<span data-ttu-id="efddf-107">导致编译时错误，因为 `if` 语句需要*embedded_statement* ，而不是*语句*用于 if 分支。</span><span class="sxs-lookup"><span data-stu-id="efddf-107">results in a compile-time error because an `if` statement requires an *embedded_statement* rather than a *statement* for its if branch.</span></span> <span data-ttu-id="efddf-108">如果允许此代码，则将声明该`i`变量，但绝不能使用它。</span><span class="sxs-lookup"><span data-stu-id="efddf-108">If this code were permitted, then the variable `i` would be declared, but it could never be used.</span></span> <span data-ttu-id="efddf-109">但请注意，通过将的`i`声明放置在块中，该示例是有效的。</span><span class="sxs-lookup"><span data-stu-id="efddf-109">Note, however, that by placing `i`'s declaration in a block, the example is valid.</span></span>

## <a name="end-points-and-reachability"></a><span data-ttu-id="efddf-110">终结点和可访问性</span><span class="sxs-lookup"><span data-stu-id="efddf-110">End points and reachability</span></span>

<span data-ttu-id="efddf-111">每个语句都有一个***终结点***。</span><span class="sxs-lookup"><span data-stu-id="efddf-111">Every statement has an ***end point***.</span></span> <span data-ttu-id="efddf-112">简而言之，语句的结束点是紧跟在语句之后的位置。</span><span class="sxs-lookup"><span data-stu-id="efddf-112">In intuitive terms, the end point of a statement is the location that immediately follows the statement.</span></span> <span data-ttu-id="efddf-113">复合语句的执行规则（包含嵌入语句的语句）指定在控件到达嵌入语句的终结点时所采取的操作。</span><span class="sxs-lookup"><span data-stu-id="efddf-113">The execution rules for composite statements (statements that contain embedded statements) specify the action that is taken when control reaches the end point of an embedded statement.</span></span> <span data-ttu-id="efddf-114">例如，当控件到达块中语句的终点时，控制将转移到块中的下一条语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-114">For example, when control reaches the end point of a statement in a block, control is transferred to the next statement in the block.</span></span>

<span data-ttu-id="efddf-115">如果语句可以通过执行到达，则表明该语句是可***访问***的。</span><span class="sxs-lookup"><span data-stu-id="efddf-115">If a statement can possibly be reached by execution, the statement is said to be ***reachable***.</span></span> <span data-ttu-id="efddf-116">相反，如果不可能执行语句，则认为该语句是无法***访问***的。</span><span class="sxs-lookup"><span data-stu-id="efddf-116">Conversely, if there is no possibility that a statement will be executed, the statement is said to be ***unreachable***.</span></span>

<span data-ttu-id="efddf-117">示例中</span><span class="sxs-lookup"><span data-stu-id="efddf-117">In the example</span></span>
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
<span data-ttu-id="efddf-118">无法访问的`Console.WriteLine`第二个调用，因为不可能执行该语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-118">the second invocation of `Console.WriteLine` is unreachable because there is no possibility that the statement will be executed.</span></span>

<span data-ttu-id="efddf-119">如果编译器确定无法访问某个语句，则会报告警告。</span><span class="sxs-lookup"><span data-stu-id="efddf-119">A warning is reported if the compiler determines that a statement is unreachable.</span></span> <span data-ttu-id="efddf-120">它特别不是错误，无法访问语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-120">It is specifically not an error for a statement to be unreachable.</span></span>

<span data-ttu-id="efddf-121">若要确定特定的语句或终结点是否可访问，编译器将根据为每个语句定义的可访问性规则执行流分析。</span><span class="sxs-lookup"><span data-stu-id="efddf-121">To determine whether a particular statement or end point is reachable, the compiler performs flow analysis according to the reachability rules defined for each statement.</span></span> <span data-ttu-id="efddf-122">流分析考虑了控制语句行为的常量表达式（[常量表达式](expressions.md#constant-expressions)）的值，但不考虑非常量表达式的可能值。</span><span class="sxs-lookup"><span data-stu-id="efddf-122">The flow analysis takes into account the values of constant expressions ([Constant expressions](expressions.md#constant-expressions)) that control the behavior of statements, but the possible values of non-constant expressions are not considered.</span></span> <span data-ttu-id="efddf-123">换句话说，出于控制流分析的目的，给定类型的非常量表达式被视为具有该类型的任何可能值。</span><span class="sxs-lookup"><span data-stu-id="efddf-123">In other words, for purposes of control flow analysis, a non-constant expression of a given type is considered to have any possible value of that type.</span></span>

<span data-ttu-id="efddf-124">示例中</span><span class="sxs-lookup"><span data-stu-id="efddf-124">In the example</span></span>
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
<span data-ttu-id="efddf-125">`if`语句的布尔表达式是常量表达式，因为`==`运算符的两个操作数都是常量。</span><span class="sxs-lookup"><span data-stu-id="efddf-125">the boolean expression of the `if` statement is a constant expression because both operands of the `==` operator are constants.</span></span> <span data-ttu-id="efddf-126">在编译时计算常量表达式，生成值`false`时`Console.WriteLine` ，会将调用视为不可访问。</span><span class="sxs-lookup"><span data-stu-id="efddf-126">As the constant expression is evaluated at compile-time, producing the value `false`, the `Console.WriteLine` invocation is considered unreachable.</span></span> <span data-ttu-id="efddf-127">但是，如果`i`将更改为本地变量</span><span class="sxs-lookup"><span data-stu-id="efddf-127">However, if `i` is changed to be a local variable</span></span>
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
<span data-ttu-id="efddf-128">`Console.WriteLine`调用被视为可访问，即使实际上它将永远不会执行。</span><span class="sxs-lookup"><span data-stu-id="efddf-128">the `Console.WriteLine` invocation is considered reachable, even though, in reality, it will never be executed.</span></span>

<span data-ttu-id="efddf-129">函数成员的*块*始终被认为是可访问的。</span><span class="sxs-lookup"><span data-stu-id="efddf-129">The *block* of a function member is always considered reachable.</span></span> <span data-ttu-id="efddf-130">通过依次计算块中每个语句的可访问性规则，可以确定任何给定语句的可访问性。</span><span class="sxs-lookup"><span data-stu-id="efddf-130">By successively evaluating the reachability rules of each statement in a block, the reachability of any given statement can be determined.</span></span>

<span data-ttu-id="efddf-131">示例中</span><span class="sxs-lookup"><span data-stu-id="efddf-131">In the example</span></span>
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
<span data-ttu-id="efddf-132">第二个`Console.WriteLine`的可访问性确定如下：</span><span class="sxs-lookup"><span data-stu-id="efddf-132">the reachability of the second `Console.WriteLine` is determined as follows:</span></span>

*  <span data-ttu-id="efddf-133">第一个`Console.WriteLine`表达式语句是可访问的，因为该`F`方法的块是可访问的。</span><span class="sxs-lookup"><span data-stu-id="efddf-133">The first `Console.WriteLine` expression statement is reachable because the block of the `F` method is reachable.</span></span>
*  <span data-ttu-id="efddf-134">第一个`Console.WriteLine`表达式语句的结束点是可访问的，因为该语句是可访问的。</span><span class="sxs-lookup"><span data-stu-id="efddf-134">The end point of the first `Console.WriteLine` expression statement is reachable because that statement is reachable.</span></span>
*  <span data-ttu-id="efddf-135">由于`if`第一个`Console.WriteLine` expression 语句的结束点是可访问的，因此该语句是可访问的。</span><span class="sxs-lookup"><span data-stu-id="efddf-135">The `if` statement is reachable because the end point of the first `Console.WriteLine` expression statement is reachable.</span></span>
*  <span data-ttu-id="efddf-136">可访问`Console.WriteLine`第二个 expression 语句，因为该`if`语句的布尔表达式没有常数值`false`。</span><span class="sxs-lookup"><span data-stu-id="efddf-136">The second `Console.WriteLine` expression statement is reachable because the boolean expression of the `if` statement does not have the constant value `false`.</span></span>

<span data-ttu-id="efddf-137">在以下两种情况下，可能会发生编译时错误，导致语句的终结点可到达：</span><span class="sxs-lookup"><span data-stu-id="efddf-137">There are two situations in which it is a compile-time error for the end point of a statement to be reachable:</span></span>

*  <span data-ttu-id="efddf-138">`switch`由于语句不允许 switch 节 "贯穿" 到下一个开关部分，因此在开关部分的语句列表的终结点可以访问时，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="efddf-138">Because the `switch` statement does not permit a switch section to "fall through" to the next switch section, it is a compile-time error for the end point of the statement list of a switch section to be reachable.</span></span> <span data-ttu-id="efddf-139">如果发生此错误，则通常指示`break`缺少语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-139">If this error occurs, it is typically an indication that a `break` statement is missing.</span></span>
*  <span data-ttu-id="efddf-140">对于计算要访问的值的函数成员块，它是一个编译时错误。</span><span class="sxs-lookup"><span data-stu-id="efddf-140">It is a compile-time error for the end point of the block of a function member that computes a value to be reachable.</span></span> <span data-ttu-id="efddf-141">如果发生此错误，则通常指示`return`缺少语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-141">If this error occurs, it typically is an indication that a `return` statement is missing.</span></span>

## <a name="blocks"></a><span data-ttu-id="efddf-142">Blocks</span><span class="sxs-lookup"><span data-stu-id="efddf-142">Blocks</span></span>

<span data-ttu-id="efddf-143">使用*代码块*，可以在允许编写一个语句的上下文中编写多个语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-143">A *block* permits multiple statements to be written in contexts where a single statement is allowed.</span></span>

```antlr
block
    : '{' statement_list? '}'
    ;
```

<span data-ttu-id="efddf-144">*块*包含一个可选的*statement_list* （[语句列表](statements.md#statement-lists)），括在大括号中。</span><span class="sxs-lookup"><span data-stu-id="efddf-144">A *block* consists of an optional *statement_list* ([Statement lists](statements.md#statement-lists)), enclosed in braces.</span></span> <span data-ttu-id="efddf-145">如果省略了语句列表，则称块为空。</span><span class="sxs-lookup"><span data-stu-id="efddf-145">If the statement list is omitted, the block is said to be empty.</span></span>

<span data-ttu-id="efddf-146">块可以包含声明语句（[声明语句](statements.md#declaration-statements)）。</span><span class="sxs-lookup"><span data-stu-id="efddf-146">A block may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="efddf-147">块中声明的局部变量或常量的范围为块。</span><span class="sxs-lookup"><span data-stu-id="efddf-147">The scope of a local variable or constant declared in a block is the block.</span></span>

<span data-ttu-id="efddf-148">块的执行方式如下：</span><span class="sxs-lookup"><span data-stu-id="efddf-148">A block is executed as follows:</span></span>

*  <span data-ttu-id="efddf-149">如果块为空，控制将被传输到块的终结点。</span><span class="sxs-lookup"><span data-stu-id="efddf-149">If the block is empty, control is transferred to the end point of the block.</span></span>
*  <span data-ttu-id="efddf-150">如果块不为空，则将控制转移到语句列表。</span><span class="sxs-lookup"><span data-stu-id="efddf-150">If the block is not empty, control is transferred to the statement list.</span></span> <span data-ttu-id="efddf-151">当和如果控件到达语句列表的终点时，控制将被传输到块的终结点。</span><span class="sxs-lookup"><span data-stu-id="efddf-151">When and if control reaches the end point of the statement list, control is transferred to the end point of the block.</span></span>

<span data-ttu-id="efddf-152">如果块本身是可访问的，则块的语句列表是可访问的。</span><span class="sxs-lookup"><span data-stu-id="efddf-152">The statement list of a block is reachable if the block itself is reachable.</span></span>

<span data-ttu-id="efddf-153">如果块为空或语句列表的终结点是可访问的，则块的终结点是可访问的。</span><span class="sxs-lookup"><span data-stu-id="efddf-153">The end point of a block is reachable if the block is empty or if the end point of the statement list is reachable.</span></span>

<span data-ttu-id="efddf-154">包含一个或多个`yield`语句（[yield 语句](statements.md#the-yield-statement)）的块称为迭代器块。</span><span class="sxs-lookup"><span data-stu-id="efddf-154">A *block* that contains one or more `yield` statements ([The yield statement](statements.md#the-yield-statement)) is called an iterator block.</span></span> <span data-ttu-id="efddf-155">迭代器块用于将函数成员作为迭代器（[迭代](classes.md#iterators)器）实现。</span><span class="sxs-lookup"><span data-stu-id="efddf-155">Iterator blocks are used to implement function members as iterators ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="efddf-156">迭代器块适用于一些附加限制：</span><span class="sxs-lookup"><span data-stu-id="efddf-156">Some additional restrictions apply to iterator blocks:</span></span>

*  <span data-ttu-id="efddf-157">`return`语句出现在迭代器块中是编译时错误（但`yield return`允许使用语句）。</span><span class="sxs-lookup"><span data-stu-id="efddf-157">It is a compile-time error for a `return` statement to appear in an iterator block (but `yield return` statements are permitted).</span></span>
*  <span data-ttu-id="efddf-158">迭代器块包含不安全的上下文（[不安全](unsafe-code.md#unsafe-contexts)上下文）是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="efddf-158">It is a compile-time error for an iterator block to contain an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="efddf-159">迭代器块始终定义安全上下文，即使其声明嵌套在不安全的上下文中也是如此。</span><span class="sxs-lookup"><span data-stu-id="efddf-159">An iterator block always defines a safe context, even when its declaration is nested in an unsafe context.</span></span>

### <a name="statement-lists"></a><span data-ttu-id="efddf-160">语句列表</span><span class="sxs-lookup"><span data-stu-id="efddf-160">Statement lists</span></span>

<span data-ttu-id="efddf-161">***语句列表***包含一个或多个按顺序编写的语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-161">A ***statement list*** consists of one or more statements written in sequence.</span></span> <span data-ttu-id="efddf-162">语句列表出现在*块*s （[块](statements.md#blocks)）和*switch_block*s （[switch 语句](statements.md#the-switch-statement)）中。</span><span class="sxs-lookup"><span data-stu-id="efddf-162">Statement lists occur in *block*s ([Blocks](statements.md#blocks)) and in *switch_block*s ([The switch statement](statements.md#the-switch-statement)).</span></span>

```antlr
statement_list
    : statement+
    ;
```

<span data-ttu-id="efddf-163">语句列表通过将控制转移到第一条语句来执行。</span><span class="sxs-lookup"><span data-stu-id="efddf-163">A statement list is executed by transferring control to the first statement.</span></span> <span data-ttu-id="efddf-164">当和如果控件到达语句的结束点时，控制将转移到下一个语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-164">When and if control reaches the end point of a statement, control is transferred to the next statement.</span></span> <span data-ttu-id="efddf-165">当和如果控件到达最后一个语句的终点时，控件将被传输到语句列表的终结点。</span><span class="sxs-lookup"><span data-stu-id="efddf-165">When and if control reaches the end point of the last statement, control is transferred to the end point of the statement list.</span></span>

<span data-ttu-id="efddf-166">如果以下至少一个条件为 true，则语句列表中的语句是可访问的：</span><span class="sxs-lookup"><span data-stu-id="efddf-166">A statement in a statement list is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="efddf-167">语句是第一条语句，语句列表本身是可访问的。</span><span class="sxs-lookup"><span data-stu-id="efddf-167">The statement is the first statement and the statement list itself is reachable.</span></span>
*  <span data-ttu-id="efddf-168">可以访问前面语句的终点。</span><span class="sxs-lookup"><span data-stu-id="efddf-168">The end point of the preceding statement is reachable.</span></span>
*  <span data-ttu-id="efddf-169">语句是标记的语句，并且标签由可访问`goto`的语句引用。</span><span class="sxs-lookup"><span data-stu-id="efddf-169">The statement is a labeled statement and the label is referenced by a reachable `goto` statement.</span></span>

<span data-ttu-id="efddf-170">如果列表中最后一个语句的结束点是可访问的，则该语句列表的终结点是可访问的。</span><span class="sxs-lookup"><span data-stu-id="efddf-170">The end point of a statement list is reachable if the end point of the last statement in the list is reachable.</span></span>

## <a name="the-empty-statement"></a><span data-ttu-id="efddf-171">空语句</span><span class="sxs-lookup"><span data-stu-id="efddf-171">The empty statement</span></span>

<span data-ttu-id="efddf-172">*Empty_statement*不执行任何操作。</span><span class="sxs-lookup"><span data-stu-id="efddf-172">An *empty_statement* does nothing.</span></span>

```antlr
empty_statement
    : ';'
    ;
```

<span data-ttu-id="efddf-173">在需要语句的上下文中没有要执行的操作时，将使用空语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-173">An empty statement is used when there are no operations to perform in a context where a statement is required.</span></span>

<span data-ttu-id="efddf-174">空语句的执行只是将控制转移到语句的终结点。</span><span class="sxs-lookup"><span data-stu-id="efddf-174">Execution of an empty statement simply transfers control to the end point of the statement.</span></span> <span data-ttu-id="efddf-175">因此，如果可以访问空语句，则可以访问空语句的结束点。</span><span class="sxs-lookup"><span data-stu-id="efddf-175">Thus, the end point of an empty statement is reachable if the empty statement is reachable.</span></span>

<span data-ttu-id="efddf-176">使用空的正文编写`while`语句时，可以使用空语句：</span><span class="sxs-lookup"><span data-stu-id="efddf-176">An empty statement can be used when writing a `while` statement with a null body:</span></span>
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

<span data-ttu-id="efddf-177">此外，空语句还可用于在块的结束 "`}`" 之前声明标签：</span><span class="sxs-lookup"><span data-stu-id="efddf-177">Also, an empty statement can be used to declare a label just before the closing "`}`" of a block:</span></span>
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a><span data-ttu-id="efddf-178">带标签的语句</span><span class="sxs-lookup"><span data-stu-id="efddf-178">Labeled statements</span></span>

<span data-ttu-id="efddf-179">*Labeled_statement*允许将语句作为标签的前缀。</span><span class="sxs-lookup"><span data-stu-id="efddf-179">A *labeled_statement* permits a statement to be prefixed by a label.</span></span> <span data-ttu-id="efddf-180">标记语句允许出现在块中，但不允许作为嵌入语句使用。</span><span class="sxs-lookup"><span data-stu-id="efddf-180">Labeled statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

<span data-ttu-id="efddf-181">标记语句使用*标识符*给定的名称声明标签。</span><span class="sxs-lookup"><span data-stu-id="efddf-181">A labeled statement declares a label with the name given by the *identifier*.</span></span> <span data-ttu-id="efddf-182">标签的作用域是在其中声明标签的整个块，包括任何嵌套块。</span><span class="sxs-lookup"><span data-stu-id="efddf-182">The scope of a label is the whole block in which the label is declared, including any nested blocks.</span></span> <span data-ttu-id="efddf-183">对于具有相同名称的两个同名标签，它是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="efddf-183">It is a compile-time error for two labels with the same name to have overlapping scopes.</span></span>

<span data-ttu-id="efddf-184">可以在标签范围内从`goto`语句（[goto 语句](statements.md#the-goto-statement)）引用标签。</span><span class="sxs-lookup"><span data-stu-id="efddf-184">A label can be referenced from `goto` statements ([The goto statement](statements.md#the-goto-statement)) within the scope of the label.</span></span> <span data-ttu-id="efddf-185">这意味着`goto`语句可以将控制转移到块中，而不是块中的块。</span><span class="sxs-lookup"><span data-stu-id="efddf-185">This means that `goto` statements can transfer control within blocks and out of blocks, but never into blocks.</span></span>

<span data-ttu-id="efddf-186">标签具有自己的声明空间，不会干扰其他标识符。</span><span class="sxs-lookup"><span data-stu-id="efddf-186">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="efddf-187">示例</span><span class="sxs-lookup"><span data-stu-id="efddf-187">The example</span></span>
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
<span data-ttu-id="efddf-188">有效并且使用名称`x`作为参数和标签。</span><span class="sxs-lookup"><span data-stu-id="efddf-188">is valid and uses the name `x` as both a parameter and a label.</span></span>

<span data-ttu-id="efddf-189">标记语句的执行与标签后面的语句的执行完全对应。</span><span class="sxs-lookup"><span data-stu-id="efddf-189">Execution of a labeled statement corresponds exactly to execution of the statement following the label.</span></span>

<span data-ttu-id="efddf-190">除了正常的控制流提供的可访问性外，如果标签由可访问`goto`的语句引用，则可以访问标记的语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-190">In addition to the reachability provided by normal flow of control, a labeled statement is reachable if the label is referenced by a reachable `goto` statement.</span></span> <span data-ttu-id="efddf-191">异常`try` `try`如果语句位于`finally`包含`finally`块的中，并且标记的语句在之外，并且无法到达块的终结点，则无法从`goto`该`goto`语句。）</span><span class="sxs-lookup"><span data-stu-id="efddf-191">(Exception: If a `goto` statement is inside a `try` that includes a `finally` block, and the labeled statement is outside the `try`, and the end point of the `finally` block is unreachable, then the labeled statement is not reachable from that `goto` statement.)</span></span>

## <a name="declaration-statements"></a><span data-ttu-id="efddf-192">声明语句</span><span class="sxs-lookup"><span data-stu-id="efddf-192">Declaration statements</span></span>

<span data-ttu-id="efddf-193">*Declaration_statement*声明局部变量或常量。</span><span class="sxs-lookup"><span data-stu-id="efddf-193">A *declaration_statement* declares a local variable or constant.</span></span> <span data-ttu-id="efddf-194">声明语句在块中是允许的，但不允许作为嵌入语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-194">Declaration statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a><span data-ttu-id="efddf-195">局部变量声明</span><span class="sxs-lookup"><span data-stu-id="efddf-195">Local variable declarations</span></span>

<span data-ttu-id="efddf-196">*Local_variable_declaration*声明一个或多个局部变量。</span><span class="sxs-lookup"><span data-stu-id="efddf-196">A *local_variable_declaration* declares one or more local variables.</span></span>

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

<span data-ttu-id="efddf-197">*Local_variable_declaration*的*local_variable_type*直接指定声明引入的变量类型，或使用标识符 `var` 来指示应基于初始值设定项推断类型。</span><span class="sxs-lookup"><span data-stu-id="efddf-197">The *local_variable_type* of a *local_variable_declaration* either directly specifies the type of the variables introduced by the declaration, or indicates with the identifier `var` that the type should be inferred based on an initializer.</span></span> <span data-ttu-id="efddf-198">该类型后跟一个*local_variable_declarator*s 列表，其中每个都引入一个新变量。</span><span class="sxs-lookup"><span data-stu-id="efddf-198">The type is followed by a list of *local_variable_declarator*s, each of which introduces a new variable.</span></span> <span data-ttu-id="efddf-199">*Local_variable_declarator*包含命名变量的*标识符*，可选择后跟 "`=`" 标记和提供变量初始值的*local_variable_initializer* 。</span><span class="sxs-lookup"><span data-stu-id="efddf-199">A *local_variable_declarator* consists of an *identifier* that names the variable, optionally followed by an "`=`" token and a *local_variable_initializer* that gives the initial value of the variable.</span></span>

<span data-ttu-id="efddf-200">在局部变量声明的上下文中，标识符 var 用作上下文关键字（[关键字](lexical-structure.md#keywords)）。如果将*local_variable_type*指定为 `var` 并且在范围内没有名为 @no__t 的类型，则声明是一个***隐式类型的局部变量声明***，其类型是从关联的初始值设定项表达式的类型推断而来的。</span><span class="sxs-lookup"><span data-stu-id="efddf-200">In the context of a local variable declaration, the identifier var acts as a contextual keyword ([Keywords](lexical-structure.md#keywords)).When the *local_variable_type* is specified as `var` and no type named `var` is in scope, the declaration is an ***implicitly typed local variable declaration***, whose type is inferred from the type of the associated initializer expression.</span></span> <span data-ttu-id="efddf-201">隐式类型的局部变量声明受到以下限制：</span><span class="sxs-lookup"><span data-stu-id="efddf-201">Implicitly typed local variable declarations are subject to the following restrictions:</span></span>

*  <span data-ttu-id="efddf-202">*Local_variable_declaration*不能包含多个*local_variable_declarator*。</span><span class="sxs-lookup"><span data-stu-id="efddf-202">The *local_variable_declaration* cannot include multiple *local_variable_declarator*s.</span></span>
*  <span data-ttu-id="efddf-203">*Local_variable_declarator*必须包括*local_variable_initializer*。</span><span class="sxs-lookup"><span data-stu-id="efddf-203">The *local_variable_declarator* must include a *local_variable_initializer*.</span></span>
*  <span data-ttu-id="efddf-204">*Local_variable_initializer*必须是一个*表达式*。</span><span class="sxs-lookup"><span data-stu-id="efddf-204">The *local_variable_initializer* must be an *expression*.</span></span>
*  <span data-ttu-id="efddf-205">初始值设定项*表达式*必须具有编译时类型。</span><span class="sxs-lookup"><span data-stu-id="efddf-205">The initializer *expression* must have a compile-time type.</span></span>
*  <span data-ttu-id="efddf-206">初始值设定项*表达式*不能引用声明的变量本身</span><span class="sxs-lookup"><span data-stu-id="efddf-206">The initializer *expression* cannot refer to the declared variable itself</span></span>

<span data-ttu-id="efddf-207">下面是错误的隐式类型化局部变量声明的示例：</span><span class="sxs-lookup"><span data-stu-id="efddf-207">The following are examples of incorrect implicitly typed local variable declarations:</span></span>

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

<span data-ttu-id="efddf-208">局部变量的值是在使用*simple_name* （[简单名称](expressions.md#simple-names)）的表达式中获取的，使用*赋值*（[赋值运算符](expressions.md#assignment-operators)）修改本地变量的值。</span><span class="sxs-lookup"><span data-stu-id="efddf-208">The value of a local variable is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)), and the value of a local variable is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="efddf-209">局部变量在获取其值的每个位置都必须明确赋值（[明确赋值](variables.md#definite-assignment)）。</span><span class="sxs-lookup"><span data-stu-id="efddf-209">A local variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at each location where its value is obtained.</span></span>

<span data-ttu-id="efddf-210">在*local_variable_declaration*中声明的局部变量的作用域是在其中进行声明的块。</span><span class="sxs-lookup"><span data-stu-id="efddf-210">The scope of a local variable declared in a *local_variable_declaration* is the block in which the declaration occurs.</span></span> <span data-ttu-id="efddf-211">在本地变量的*local_variable_declarator*之前的文本位置引用本地变量是错误的。</span><span class="sxs-lookup"><span data-stu-id="efddf-211">It is an error to refer to a local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="efddf-212">在局部变量的作用域内，使用相同的名称声明另一个局部变量或常量时，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="efddf-212">Within the scope of a local variable, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="efddf-213">声明多个变量的局部变量声明等效于多个具有相同类型的单个变量的声明。</span><span class="sxs-lookup"><span data-stu-id="efddf-213">A local variable declaration that declares multiple variables is equivalent to multiple declarations of single variables with the same type.</span></span> <span data-ttu-id="efddf-214">此外，局部变量声明中的变量初始值设定项完全对应于紧接在声明后插入的赋值语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-214">Furthermore, a variable initializer in a local variable declaration corresponds exactly to an assignment statement that is inserted immediately after the declaration.</span></span>

<span data-ttu-id="efddf-215">示例</span><span class="sxs-lookup"><span data-stu-id="efddf-215">The example</span></span>
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
<span data-ttu-id="efddf-216">完全对应于</span><span class="sxs-lookup"><span data-stu-id="efddf-216">corresponds exactly to</span></span>
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

<span data-ttu-id="efddf-217">在隐式类型的局部变量声明中，声明的局部变量的类型将被视为与用于初始化变量的表达式的类型相同。</span><span class="sxs-lookup"><span data-stu-id="efddf-217">In an implicitly typed local variable declaration, the type of the local variable being declared is taken to be the same as the type of the expression used to initialize the variable.</span></span> <span data-ttu-id="efddf-218">例如：</span><span class="sxs-lookup"><span data-stu-id="efddf-218">For example:</span></span>
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

<span data-ttu-id="efddf-219">上面的隐式类型局部变量声明完全等效于以下显式类型化声明：</span><span class="sxs-lookup"><span data-stu-id="efddf-219">The implicitly typed local variable declarations above are precisely equivalent to the following explicitly typed declarations:</span></span>
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a><span data-ttu-id="efddf-220">局部常量声明</span><span class="sxs-lookup"><span data-stu-id="efddf-220">Local constant declarations</span></span>

<span data-ttu-id="efddf-221">*Local_constant_declaration*声明一个或多个本地常量。</span><span class="sxs-lookup"><span data-stu-id="efddf-221">A *local_constant_declaration* declares one or more local constants.</span></span>

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

<span data-ttu-id="efddf-222">*Local_constant_declaration*的*类型*指定声明引入的常量的类型。</span><span class="sxs-lookup"><span data-stu-id="efddf-222">The *type* of a *local_constant_declaration* specifies the type of the constants introduced by the declaration.</span></span> <span data-ttu-id="efddf-223">该类型后跟一个*constant_declarator*s 列表，其中每个都引入一个新常量。</span><span class="sxs-lookup"><span data-stu-id="efddf-223">The type is followed by a list of *constant_declarator*s, each of which introduces a new constant.</span></span> <span data-ttu-id="efddf-224">*Constant_declarator*包含命名常量的*标识符*，后跟 "`=`" 标记，后跟*constant_expression* （[常数表达式](expressions.md#constant-expressions)），提供常量的值。</span><span class="sxs-lookup"><span data-stu-id="efddf-224">A *constant_declarator* consists of an *identifier* that names the constant, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the constant.</span></span>

<span data-ttu-id="efddf-225">局部常量声明的*类型*和*constant_expression*必须遵循与常量成员声明（[常量](classes.md#constants)）相同的规则。</span><span class="sxs-lookup"><span data-stu-id="efddf-225">The *type* and *constant_expression* of a local constant declaration must follow the same rules as those of a constant member declaration ([Constants](classes.md#constants)).</span></span>

<span data-ttu-id="efddf-226">本地常量的值是使用*simple_name* （[简单名称](expressions.md#simple-names)）在表达式中获取的。</span><span class="sxs-lookup"><span data-stu-id="efddf-226">The value of a local constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="efddf-227">局部常数的作用域是在其中进行声明的块。</span><span class="sxs-lookup"><span data-stu-id="efddf-227">The scope of a local constant is the block in which the declaration occurs.</span></span> <span data-ttu-id="efddf-228">在其*constant_declarator*之前的文本位置引用本地常量是错误的。</span><span class="sxs-lookup"><span data-stu-id="efddf-228">It is an error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span> <span data-ttu-id="efddf-229">在局部常数的范围内，使用相同的名称声明另一个局部变量或常量时，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="efddf-229">Within the scope of a local constant, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="efddf-230">声明多个常量的局部常量声明等效于多个具有相同类型的单个常量的声明。</span><span class="sxs-lookup"><span data-stu-id="efddf-230">A local constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same type.</span></span>

## <a name="expression-statements"></a><span data-ttu-id="efddf-231">表达式语句</span><span class="sxs-lookup"><span data-stu-id="efddf-231">Expression statements</span></span>

<span data-ttu-id="efddf-232">*Expression_statement*计算给定的表达式。</span><span class="sxs-lookup"><span data-stu-id="efddf-232">An *expression_statement* evaluates a given expression.</span></span> <span data-ttu-id="efddf-233">由表达式计算的值（如果有）将被丢弃。</span><span class="sxs-lookup"><span data-stu-id="efddf-233">The value computed by the expression, if any, is discarded.</span></span>

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

<span data-ttu-id="efddf-234">并非所有表达式都允许作为语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-234">Not all expressions are permitted as statements.</span></span> <span data-ttu-id="efddf-235">特别是，不允许将`x + y`仅`x == 1`计算值的表达式（将丢弃此值）作为语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-235">In particular, expressions such as `x + y` and `x == 1` that merely compute a value (which will be discarded), are not permitted as statements.</span></span>

<span data-ttu-id="efddf-236">执行*expression_statement*将计算包含的表达式，然后将控制转移到*expression_statement*的终结点。</span><span class="sxs-lookup"><span data-stu-id="efddf-236">Execution of an *expression_statement* evaluates the contained expression and then transfers control to the end point of the *expression_statement*.</span></span> <span data-ttu-id="efddf-237">如果*expression_statement*可访问，则可访问*expression_statement*的终结点。</span><span class="sxs-lookup"><span data-stu-id="efddf-237">The end point of an *expression_statement* is reachable if that *expression_statement* is reachable.</span></span>

## <a name="selection-statements"></a><span data-ttu-id="efddf-238">选择语句</span><span class="sxs-lookup"><span data-stu-id="efddf-238">Selection statements</span></span>

<span data-ttu-id="efddf-239">选择语句根据某个表达式的值为执行选择多个可能的语句之一。</span><span class="sxs-lookup"><span data-stu-id="efddf-239">Selection statements select one of a number of possible statements for execution based on the value of some expression.</span></span>

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a><span data-ttu-id="efddf-240">If 语句</span><span class="sxs-lookup"><span data-stu-id="efddf-240">The if statement</span></span>

<span data-ttu-id="efddf-241">`if`语句基于布尔表达式的值来选择要执行的语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-241">The `if` statement selects a statement for execution based on the value of a boolean expression.</span></span>

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="efddf-242">部件与语法允许的上一个词法上`if`最近的语句相关联。 `else`</span><span class="sxs-lookup"><span data-stu-id="efddf-242">An `else` part is associated with the lexically nearest preceding `if` that is allowed by the syntax.</span></span> <span data-ttu-id="efddf-243">因此， `if`形式的语句</span><span class="sxs-lookup"><span data-stu-id="efddf-243">Thus, an `if` statement of the form</span></span>
```csharp
if (x) if (y) F(); else G();
```
<span data-ttu-id="efddf-244">等效于</span><span class="sxs-lookup"><span data-stu-id="efddf-244">is equivalent to</span></span>
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

<span data-ttu-id="efddf-245">`if`语句的执行方式如下：</span><span class="sxs-lookup"><span data-stu-id="efddf-245">An `if` statement is executed as follows:</span></span>

*  <span data-ttu-id="efddf-246">计算*boolean_expression* （[布尔表达式](expressions.md#boolean-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="efddf-246">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="efddf-247">如果布尔表达式产生`true`了，则将控制转移到第一个嵌入语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-247">If the boolean expression yields `true`, control is transferred to the first embedded statement.</span></span> <span data-ttu-id="efddf-248">当和如果控件到达该语句的终点时，控制将被传输到`if`语句的终结点。</span><span class="sxs-lookup"><span data-stu-id="efddf-248">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="efddf-249">如果布尔表达式的结果`false`为，并且`else`存在某个部分，则将控制转移到第二个嵌入语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-249">If the boolean expression yields `false` and if an `else` part is present, control is transferred to the second embedded statement.</span></span> <span data-ttu-id="efddf-250">当和如果控件到达该语句的终点时，控制将被传输到`if`语句的终结点。</span><span class="sxs-lookup"><span data-stu-id="efddf-250">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="efddf-251">如果布尔表达式产生`false`并且如果某个`else`部分不存在，则将控制转移到该`if`语句的结束点。</span><span class="sxs-lookup"><span data-stu-id="efddf-251">If the boolean expression yields `false` and if an `else` part is not present, control is transferred to the end point of the `if` statement.</span></span>

<span data-ttu-id="efddf-252">如果语句是可访问的`if` `if`并且布尔表达式不具有常数值`false`，则可以访问语句的第一个嵌入语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-252">The first embedded statement of an `if` statement is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="efddf-253">如果语句是可访问的`if` `if`并且布尔表达式不具有常数值`true`，则语句的第二个嵌入语句（如果有）是可访问的。</span><span class="sxs-lookup"><span data-stu-id="efddf-253">The second embedded statement of an `if` statement, if present, is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

<span data-ttu-id="efddf-254">如果至少有一个嵌入`if`语句的结束点可访问，则该语句的结束点是可访问的。</span><span class="sxs-lookup"><span data-stu-id="efddf-254">The end point of an `if` statement is reachable if the end point of at least one of its embedded statements is reachable.</span></span> <span data-ttu-id="efddf-255">此外，如果语句是可访问的`if` `if`并且布尔表达式`else`不具有常数值`true`，则可访问没有任何部分的语句的结束点。</span><span class="sxs-lookup"><span data-stu-id="efddf-255">In addition, the end point of an `if` statement with no `else` part is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-switch-statement"></a><span data-ttu-id="efddf-256">Switch 语句</span><span class="sxs-lookup"><span data-stu-id="efddf-256">The switch statement</span></span>

<span data-ttu-id="efddf-257">Switch 语句为执行选择一个语句列表，其中包含与 switch 表达式的值相对应的关联 switch 标签。</span><span class="sxs-lookup"><span data-stu-id="efddf-257">The switch statement selects for execution a statement list having an associated switch label that corresponds to the value of the switch expression.</span></span>

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

<span data-ttu-id="efddf-258">*Switch_statement*包含关键字 `switch`，后跟带括号的表达式（称为 switch 表达式），后跟一个*switch_block*。</span><span class="sxs-lookup"><span data-stu-id="efddf-258">A *switch_statement* consists of the keyword `switch`, followed by a parenthesized expression (called the switch expression), followed by a *switch_block*.</span></span> <span data-ttu-id="efddf-259">*Switch_block*由零个或多个*switch_section*组成，括在大括号中。</span><span class="sxs-lookup"><span data-stu-id="efddf-259">The *switch_block* consists of zero or more *switch_section*s, enclosed in braces.</span></span> <span data-ttu-id="efddf-260">每个*switch_section*都包含一个或多个*switch_label*，后跟一个*statement_list* （[语句列表](statements.md#statement-lists)）。</span><span class="sxs-lookup"><span data-stu-id="efddf-260">Each *switch_section* consists of one or more *switch_label*s followed by a *statement_list* ([Statement lists](statements.md#statement-lists)).</span></span>

<span data-ttu-id="efddf-261">`switch`语句的***管理类型***由 switch 表达式建立。</span><span class="sxs-lookup"><span data-stu-id="efddf-261">The ***governing type*** of a `switch` statement is established by the switch expression.</span></span>

*  <span data-ttu-id="efddf-262">如果 switch 表达式的类型为 `sbyte`，`byte`，`short`，`ushort`，`int`，`uint`，`long`，`ulong`，`bool`，`char`，0 或*为*，或者如果它是对应于其中一种类型的可以为 null 的类型，则为。，则这是 2 语句的管理类型。</span><span class="sxs-lookup"><span data-stu-id="efddf-262">If the type of the switch expression is `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, or an *enum_type*, or if it is the nullable type corresponding to one of these types, then that is the governing type of the `switch` statement.</span></span>
*  <span data-ttu-id="efddf-263">否则，必须有一个用户定义的隐式转换（[用户定义的转换](conversions.md#user-defined-conversions)） `sbyte`从 switch 表达式的类型到以下可能的管理类型之一：、 `byte`、、 `ushort` `short`、 `int` 、`uint`、 、`string`、 、`char`或，这是与这些类型之一对应的可以为 null 的类型。 `ulong` `long`</span><span class="sxs-lookup"><span data-stu-id="efddf-263">Otherwise, exactly one user-defined implicit conversion ([User-defined conversions](conversions.md#user-defined-conversions)) must exist from the type of the switch expression to one of the following possible governing types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, or,  a nullable type corresponding to one of those types.</span></span>
*  <span data-ttu-id="efddf-264">否则，如果不存在这样的隐式转换，或者存在多个这样的隐式转换，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="efddf-264">Otherwise, if no such implicit conversion exists, or if more than one such implicit conversion exists, a compile-time error occurs.</span></span>

<span data-ttu-id="efddf-265">每个`case`标签的常量表达式必须表示一个可隐式转换（[隐式转换](conversions.md#implicit-conversions)）为`switch`语句的管理类型的值。</span><span class="sxs-lookup"><span data-stu-id="efddf-265">The constant expression of each `case` label must denote a value that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the `switch` statement.</span></span> <span data-ttu-id="efddf-266">如果同一`case` `switch`语句中的两个或更多标签指定同一常数值，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="efddf-266">A compile-time error occurs if two or more `case` labels in the same `switch` statement specify the same constant value.</span></span>

<span data-ttu-id="efddf-267">Switch 语句中最多只能`default`有一个标签。</span><span class="sxs-lookup"><span data-stu-id="efddf-267">There can be at most one `default` label in a switch statement.</span></span>

<span data-ttu-id="efddf-268">`switch`语句的执行方式如下：</span><span class="sxs-lookup"><span data-stu-id="efddf-268">A `switch` statement is executed as follows:</span></span>

*  <span data-ttu-id="efddf-269">将计算 switch 表达式并将其转换为管理类型。</span><span class="sxs-lookup"><span data-stu-id="efddf-269">The switch expression is evaluated and converted to the governing type.</span></span>
*  <span data-ttu-id="efddf-270">如果在同一`case` `switch`语句中的标签中指定的一个常量等于 switch 表达式的值，则控制将转移到匹配`case`的标签后面的语句列表中。</span><span class="sxs-lookup"><span data-stu-id="efddf-270">If one of the constants specified in a `case` label in the same `switch` statement is equal to the value of the switch expression, control is transferred to the statement list following the matched `case` label.</span></span>
*  <span data-ttu-id="efddf-271">如果`case`在同一`switch`语句的标签中指定的常量均不等于 switch `default`表达式的值，并且如果存在标签，则将控制转移到后面`default`的语句列表。标识.</span><span class="sxs-lookup"><span data-stu-id="efddf-271">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if a `default` label is present, control is transferred to the statement list following the `default` label.</span></span>
*  <span data-ttu-id="efddf-272">如果`case`在同一`switch`语句的标签中指定的常量均不等于 switch 表达式的值，并且如果不存在任何`default`标签，则`switch`会将控制转移到语句的终点。</span><span class="sxs-lookup"><span data-stu-id="efddf-272">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if no `default` label is present, control is transferred to the end point of the `switch` statement.</span></span>

<span data-ttu-id="efddf-273">如果开关部分的语句列表的结束点是可访问的，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="efddf-273">If the end point of the statement list of a switch section is reachable, a compile-time error occurs.</span></span> <span data-ttu-id="efddf-274">这称为 "不贯穿" 规则。</span><span class="sxs-lookup"><span data-stu-id="efddf-274">This is known as the "no fall through" rule.</span></span> <span data-ttu-id="efddf-275">示例</span><span class="sxs-lookup"><span data-stu-id="efddf-275">The example</span></span>
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
<span data-ttu-id="efddf-276">有效，因为没有开关部分具有可访问的终结点。</span><span class="sxs-lookup"><span data-stu-id="efddf-276">is valid because no switch section has a reachable end point.</span></span> <span data-ttu-id="efddf-277">与 C 和C++不同，switch 节的执行不允许 "贯穿" 到下一个开关部分，示例</span><span class="sxs-lookup"><span data-stu-id="efddf-277">Unlike C and C++, execution of a switch section is not permitted to "fall through" to the next switch section, and the example</span></span>
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
<span data-ttu-id="efddf-278">导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="efddf-278">results in a compile-time error.</span></span> <span data-ttu-id="efddf-279">当执行 switch 节后，若要执行另一个 switch 节，必须使用显式`goto case`或`goto default`语句：</span><span class="sxs-lookup"><span data-stu-id="efddf-279">When execution of a switch section is to be followed by execution of another switch section, an explicit `goto case` or `goto default` statement must be used:</span></span>
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

<span data-ttu-id="efddf-280">*Switch_section*允许使用多个标签。</span><span class="sxs-lookup"><span data-stu-id="efddf-280">Multiple labels are permitted in a *switch_section*.</span></span> <span data-ttu-id="efddf-281">示例</span><span class="sxs-lookup"><span data-stu-id="efddf-281">The example</span></span>
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
<span data-ttu-id="efddf-282">有效。</span><span class="sxs-lookup"><span data-stu-id="efddf-282">is valid.</span></span> <span data-ttu-id="efddf-283">该示例不违反 "no 贯穿" 规则，因为标签 `case 2:`，`default:` 是同一*switch_section*的一部分。</span><span class="sxs-lookup"><span data-stu-id="efddf-283">The example does not violate the "no fall through" rule because the labels `case 2:` and `default:` are part of the same *switch_section*.</span></span>

<span data-ttu-id="efddf-284">"不贯穿" 规则可防止 C 中发生的常见错误类，以及C++意外省略语句`break`的情况。</span><span class="sxs-lookup"><span data-stu-id="efddf-284">The "no fall through" rule prevents a common class of bugs that occur in C and C++ when `break` statements are accidentally omitted.</span></span> <span data-ttu-id="efddf-285">此外，由于此规则，可以任意重新排列`switch`语句的 switch 部分，而不会影响语句的行为。</span><span class="sxs-lookup"><span data-stu-id="efddf-285">In addition, because of this rule, the switch sections of a `switch` statement can be arbitrarily rearranged without affecting the behavior of the statement.</span></span> <span data-ttu-id="efddf-286">例如，上述`switch`语句的各个部分可以反转，而不会影响语句的行为：</span><span class="sxs-lookup"><span data-stu-id="efddf-286">For example, the sections of the `switch` statement above can be reversed without affecting the behavior of the statement:</span></span>
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

<span data-ttu-id="efddf-287">Switch 部分的语句列表通常以`break`、 `goto case`或`goto default`语句结束，但允许任何不能访问语句列表的终结点的构造。</span><span class="sxs-lookup"><span data-stu-id="efddf-287">The statement list of a switch section typically ends in a `break`, `goto case`, or `goto default` statement, but any construct that renders the end point of the statement list unreachable is permitted.</span></span> <span data-ttu-id="efddf-288">例如， `while`已知由布尔表达式`true`控制的语句永远不会到达其终结点。</span><span class="sxs-lookup"><span data-stu-id="efddf-288">For example, a `while` statement controlled by the boolean expression `true` is known to never reach its end point.</span></span> <span data-ttu-id="efddf-289">同样，或`throw` `return`语句始终将控制转移到其他位置，而永远不会到达终结点。</span><span class="sxs-lookup"><span data-stu-id="efddf-289">Likewise, a `throw` or `return` statement always transfers control elsewhere and never reaches its end point.</span></span> <span data-ttu-id="efddf-290">因此，下面的示例是有效的：</span><span class="sxs-lookup"><span data-stu-id="efddf-290">Thus, the following example is valid:</span></span>
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

<span data-ttu-id="efddf-291">`switch`语句的管理类型可以是类型`string`。</span><span class="sxs-lookup"><span data-stu-id="efddf-291">The governing type of a `switch` statement may be the type `string`.</span></span> <span data-ttu-id="efddf-292">例如：</span><span class="sxs-lookup"><span data-stu-id="efddf-292">For example:</span></span>
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

<span data-ttu-id="efddf-293">与字符串相等运算符（[字符串相等运算符](expressions.md#string-equality-operators)）一样，该`switch`语句区分大小写，并且仅当 switch 表达式`case`字符串与标签常量完全匹配时，才会执行给定的 switch 节。</span><span class="sxs-lookup"><span data-stu-id="efddf-293">Like the string equality operators ([String equality operators](expressions.md#string-equality-operators)), the `switch` statement is case sensitive and will execute a given switch section only if the switch expression string exactly matches a `case` label constant.</span></span>

<span data-ttu-id="efddf-294">当`switch`语句的管理类型为`string`时，允许值`null`作为 case 标签常量。</span><span class="sxs-lookup"><span data-stu-id="efddf-294">When the governing type of a `switch` statement is `string`, the value `null` is permitted as a case label constant.</span></span>

<span data-ttu-id="efddf-295">*Switch_block*的*statement_list*可以包含声明语句（[声明语句](statements.md#declaration-statements)）。</span><span class="sxs-lookup"><span data-stu-id="efddf-295">The *statement_list*s of a *switch_block* may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="efddf-296">Switch 块中声明的局部变量或常量的范围为 switch 块。</span><span class="sxs-lookup"><span data-stu-id="efddf-296">The scope of a local variable or constant declared in a switch block is the switch block.</span></span>

<span data-ttu-id="efddf-297">如果可以访问该`switch`语句，并且至少满足以下条件之一，则可到达给定开关部分的语句列表：</span><span class="sxs-lookup"><span data-stu-id="efddf-297">The statement list of a given switch section is reachable if the `switch` statement is reachable and at least one of the following is true:</span></span>

*  <span data-ttu-id="efddf-298">Switch 表达式是一个非常量的值。</span><span class="sxs-lookup"><span data-stu-id="efddf-298">The switch expression is a non-constant value.</span></span>
*  <span data-ttu-id="efddf-299">Switch 表达式是一个与开关部分中的`case`标签匹配的常量值。</span><span class="sxs-lookup"><span data-stu-id="efddf-299">The switch expression is a constant value that matches a `case` label in the switch section.</span></span>
*  <span data-ttu-id="efddf-300">Switch 表达式是不与任何`case`标签匹配的常数值，并且开关部分`default`包含标签。</span><span class="sxs-lookup"><span data-stu-id="efddf-300">The switch expression is a constant value that doesn't match any `case` label, and the switch section contains the `default` label.</span></span>
*  <span data-ttu-id="efddf-301">开关部分的开关标签由可访问`goto case`的或`goto default`语句引用。</span><span class="sxs-lookup"><span data-stu-id="efddf-301">A switch label of the switch section is referenced by a reachable `goto case` or `goto default` statement.</span></span>

<span data-ttu-id="efddf-302">如果以下至少一个条件`switch`为 true，则可以访问语句的结束点：</span><span class="sxs-lookup"><span data-stu-id="efddf-302">The end point of a `switch` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="efddf-303">语句包含可访问`break`语句，该语句可`switch`退出语句。 `switch`</span><span class="sxs-lookup"><span data-stu-id="efddf-303">The `switch` statement contains a reachable `break` statement that exits the `switch` statement.</span></span>
*  <span data-ttu-id="efddf-304">该`switch`语句可访问，switch 表达式为非常量值，并且不存在任何`default`标签。</span><span class="sxs-lookup"><span data-stu-id="efddf-304">The `switch` statement is reachable, the switch expression is a non-constant value, and no `default` label is present.</span></span>
*  <span data-ttu-id="efddf-305">该`switch`语句是可访问的，switch 表达式是不与任何`case`标签匹配的常数值，并且`default`不存在任何标签。</span><span class="sxs-lookup"><span data-stu-id="efddf-305">The `switch` statement is reachable, the switch expression is a constant value that doesn't match any `case` label, and no `default` label is present.</span></span>

## <a name="iteration-statements"></a><span data-ttu-id="efddf-306">迭代语句</span><span class="sxs-lookup"><span data-stu-id="efddf-306">Iteration statements</span></span>

<span data-ttu-id="efddf-307">迭代语句重复执行嵌入语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-307">Iteration statements repeatedly execute an embedded statement.</span></span>

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a><span data-ttu-id="efddf-308">While 语句</span><span class="sxs-lookup"><span data-stu-id="efddf-308">The while statement</span></span>

<span data-ttu-id="efddf-309">`while`语句有条件地执行一个嵌入语句零次或多次。</span><span class="sxs-lookup"><span data-stu-id="efddf-309">The `while` statement conditionally executes an embedded statement zero or more times.</span></span>

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

<span data-ttu-id="efddf-310">`while`语句的执行方式如下：</span><span class="sxs-lookup"><span data-stu-id="efddf-310">A `while` statement is executed as follows:</span></span>

*  <span data-ttu-id="efddf-311">计算*boolean_expression* （[布尔表达式](expressions.md#boolean-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="efddf-311">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="efddf-312">如果布尔表达式产生`true`了，则将控制转移到嵌入语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-312">If the boolean expression yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="efddf-313">当和如果控件到达嵌入语句的结束点（可能是执行`continue`语句时），则将控制转移到`while`语句的开头。</span><span class="sxs-lookup"><span data-stu-id="efddf-313">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), control is transferred to the beginning of the `while` statement.</span></span>
*  <span data-ttu-id="efddf-314">如果布尔表达式产生`false`，控制将被传输到`while`语句的结束点。</span><span class="sxs-lookup"><span data-stu-id="efddf-314">If the boolean expression yields `false`, control is transferred to the end point of the `while` statement.</span></span>

<span data-ttu-id="efddf-315">在`while`语句的嵌入语句中`break` ，语句（[break 语句](statements.md#the-break-statement)）可用于将`while`控制转移到语句的终结点（从而结束嵌入语句的迭代）和`continue`语句（[continue 语句](statements.md#the-continue-statement)）可用于将控制转移到嵌入语句的终结点（从而执行`while`语句的其他迭代）。</span><span class="sxs-lookup"><span data-stu-id="efddf-315">Within the embedded statement of a `while` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `while` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus performing another iteration of the `while` statement).</span></span>

<span data-ttu-id="efddf-316">如果语句是可访问`while`的`while`并且布尔表达式不具有常数值`false`，则可以访问语句的嵌入语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-316">The embedded statement of a `while` statement is reachable if the `while` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="efddf-317">如果以下至少一个条件`while`为 true，则可以访问语句的结束点：</span><span class="sxs-lookup"><span data-stu-id="efddf-317">The end point of a `while` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="efddf-318">语句包含可访问`break`语句，该语句可`while`退出语句。 `while`</span><span class="sxs-lookup"><span data-stu-id="efddf-318">The `while` statement contains a reachable `break` statement that exits the `while` statement.</span></span>
*  <span data-ttu-id="efddf-319">该`while`语句可访问，且布尔表达式没有常数值`true`。</span><span class="sxs-lookup"><span data-stu-id="efddf-319">The `while` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-do-statement"></a><span data-ttu-id="efddf-320">Do 语句</span><span class="sxs-lookup"><span data-stu-id="efddf-320">The do statement</span></span>

<span data-ttu-id="efddf-321">`do`语句有条件地执行一次或多次嵌入式语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-321">The `do` statement conditionally executes an embedded statement one or more times.</span></span>

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

<span data-ttu-id="efddf-322">`do`语句的执行方式如下：</span><span class="sxs-lookup"><span data-stu-id="efddf-322">A `do` statement is executed as follows:</span></span>

*  <span data-ttu-id="efddf-323">控件将被传输到嵌入语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-323">Control is transferred to the embedded statement.</span></span>
*  <span data-ttu-id="efddf-324">当和如果控件到达嵌入语句的终结点（可能是执行 `continue` 语句时），将计算*boolean_expression* （[布尔值表达式](expressions.md#boolean-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="efddf-324">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span> <span data-ttu-id="efddf-325">如果布尔表达式产生`true`，控制将被传输到`do`语句的开头。</span><span class="sxs-lookup"><span data-stu-id="efddf-325">If the boolean expression yields `true`, control is transferred to the beginning of the `do` statement.</span></span> <span data-ttu-id="efddf-326">否则，控制将转移到`do`语句的结束点。</span><span class="sxs-lookup"><span data-stu-id="efddf-326">Otherwise, control is transferred to the end point of the `do` statement.</span></span>

<span data-ttu-id="efddf-327">在`do`语句的嵌入语句中`break` ，语句（[break 语句](statements.md#the-break-statement)）可用于将`do`控制转移到语句的终结点（从而结束嵌入语句的迭代）和`continue`语句（[continue 语句](statements.md#the-continue-statement)）可用于将控制转移到嵌入语句的终结点。</span><span class="sxs-lookup"><span data-stu-id="efddf-327">Within the embedded statement of a `do` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `do` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement.</span></span>

<span data-ttu-id="efddf-328">如果语句可访问， `do`则可以访问语句的嵌入语句。 `do`</span><span class="sxs-lookup"><span data-stu-id="efddf-328">The embedded statement of a `do` statement is reachable if the `do` statement is reachable.</span></span>

<span data-ttu-id="efddf-329">如果以下至少一个条件`do`为 true，则可以访问语句的结束点：</span><span class="sxs-lookup"><span data-stu-id="efddf-329">The end point of a `do` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="efddf-330">语句包含可访问`break`语句，该语句可`do`退出语句。 `do`</span><span class="sxs-lookup"><span data-stu-id="efddf-330">The `do` statement contains a reachable `break` statement that exits the `do` statement.</span></span>
*  <span data-ttu-id="efddf-331">嵌入语句的结束点是可访问的，并且布尔表达式没有常数值`true`。</span><span class="sxs-lookup"><span data-stu-id="efddf-331">The end point of the embedded statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-for-statement"></a><span data-ttu-id="efddf-332">For 语句</span><span class="sxs-lookup"><span data-stu-id="efddf-332">The for statement</span></span>

<span data-ttu-id="efddf-333">该`for`语句计算一系列的初始化表达式，然后，当条件为 true 时，重复执行嵌入语句并计算迭代表达式的序列。</span><span class="sxs-lookup"><span data-stu-id="efddf-333">The `for` statement evaluates a sequence of initialization expressions and then, while a condition is true, repeatedly executes an embedded statement and evaluates a sequence of iteration expressions.</span></span>

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

<span data-ttu-id="efddf-334">*For_initializer*（如果存在）由*local_variable_declaration* （[局部变量声明](statements.md#local-variable-declarations)）或以逗号分隔的*statement_expression*s （[expression 语句](statements.md#expression-statements)）列表组成。</span><span class="sxs-lookup"><span data-stu-id="efddf-334">The *for_initializer*, if present, consists of either a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) or a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span> <span data-ttu-id="efddf-335">*For_initializer*声明的局部变量的作用域从变量的*local_variable_declarator*开始，并延伸到嵌入语句的末尾。</span><span class="sxs-lookup"><span data-stu-id="efddf-335">The scope of a local variable declared by a *for_initializer* starts at the *local_variable_declarator* for the variable and extends to the end of the embedded statement.</span></span> <span data-ttu-id="efddf-336">范围包括*for_condition*和*for_iterator*。</span><span class="sxs-lookup"><span data-stu-id="efddf-336">The scope includes the *for_condition* and the *for_iterator*.</span></span>

<span data-ttu-id="efddf-337">*For_condition*（如果存在）必须是*boolean_expression* （[布尔表达式](expressions.md#boolean-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="efddf-337">The *for_condition*, if present, must be a *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)).</span></span>

<span data-ttu-id="efddf-338">*For_iterator*（如果存在）由以逗号分隔的*Statement_expression*s （[expression 语句](statements.md#expression-statements)）列表组成。</span><span class="sxs-lookup"><span data-stu-id="efddf-338">The *for_iterator*, if present, consists of a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span>

<span data-ttu-id="efddf-339">For 语句的执行方式如下：</span><span class="sxs-lookup"><span data-stu-id="efddf-339">A for statement is executed as follows:</span></span>

*  <span data-ttu-id="efddf-340">如果存在*for_initializer* ，变量初始值设定项或语句表达式将按照它们的写入顺序执行。</span><span class="sxs-lookup"><span data-stu-id="efddf-340">If a *for_initializer* is present, the variable initializers or statement expressions are executed in the order they are written.</span></span> <span data-ttu-id="efddf-341">此步骤只执行一次。</span><span class="sxs-lookup"><span data-stu-id="efddf-341">This step is only performed once.</span></span>
*  <span data-ttu-id="efddf-342">如果存在*for_condition* ，则对其进行评估。</span><span class="sxs-lookup"><span data-stu-id="efddf-342">If a *for_condition* is present, it is evaluated.</span></span>
*  <span data-ttu-id="efddf-343">如果*for_condition*不存在，或者如果计算结果为 `true`，则将控件传输到嵌入语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-343">If the *for_condition* is not present or if the evaluation yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="efddf-344">当和如果控件到达嵌入语句的终结点（可能是执行 `continue` 语句）时， *for_iterator*的表达式（如果有）将按顺序进行计算，然后执行另一次迭代，从计算上述步骤中的*for_condition* 。</span><span class="sxs-lookup"><span data-stu-id="efddf-344">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the expressions of the *for_iterator*, if any, are evaluated in sequence, and then another iteration is performed, starting with evaluation of the *for_condition* in the step above.</span></span>
*  <span data-ttu-id="efddf-345">如果*for_condition*存在并且计算产生 `false`，则将控制权转移到 @no__t 2 语句的终点。</span><span class="sxs-lookup"><span data-stu-id="efddf-345">If the *for_condition* is present and the evaluation yields `false`, control is transferred to the end point of the `for` statement.</span></span>

<span data-ttu-id="efddf-346">在 `for` 语句的嵌入语句内，@no__t 语句（[break 语句](statements.md#the-break-statement)）可用于将控制转移到第 3 @no__t 语句的终点（从而结束嵌入语句的迭代）和 @no__t 4 语句（[Continue 语句](statements.md#the-continue-statement)）可用于将控制转移到嵌入语句的终结点（从而执行*for_iterator* ，并从*for_condition*开始执行 @no__t 7 语句的另一次迭代）。</span><span class="sxs-lookup"><span data-stu-id="efddf-346">Within the embedded statement of a `for` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `for` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus executing the *for_iterator* and performing another iteration of the `for` statement, starting with the *for_condition*).</span></span>

<span data-ttu-id="efddf-347">如果满足以下条件之一`for` ，则可以访问语句的嵌入语句：</span><span class="sxs-lookup"><span data-stu-id="efddf-347">The embedded statement of a `for` statement is reachable if one of the following is true:</span></span>

*  <span data-ttu-id="efddf-348">@No__t-0 语句是可访问的，并且不存在*for_condition* 。</span><span class="sxs-lookup"><span data-stu-id="efddf-348">The `for` statement is reachable and no *for_condition* is present.</span></span>
*  <span data-ttu-id="efddf-349">@No__t-0 语句是可访问的，并且存在*for_condition* ，并且没有 `false` 的常量值。</span><span class="sxs-lookup"><span data-stu-id="efddf-349">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `false`.</span></span>

<span data-ttu-id="efddf-350">如果以下至少一个条件`for`为 true，则可以访问语句的结束点：</span><span class="sxs-lookup"><span data-stu-id="efddf-350">The end point of a `for` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="efddf-351">语句包含可访问`break`语句，该语句可`for`退出语句。 `for`</span><span class="sxs-lookup"><span data-stu-id="efddf-351">The `for` statement contains a reachable `break` statement that exits the `for` statement.</span></span>
*  <span data-ttu-id="efddf-352">@No__t-0 语句是可访问的，并且存在*for_condition* ，并且没有 `true` 的常量值。</span><span class="sxs-lookup"><span data-stu-id="efddf-352">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `true`.</span></span>

### <a name="the-foreach-statement"></a><span data-ttu-id="efddf-353">Foreach 语句</span><span class="sxs-lookup"><span data-stu-id="efddf-353">The foreach statement</span></span>

<span data-ttu-id="efddf-354">`foreach`语句枚举集合中的元素，并对集合中的每个元素执行嵌入语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-354">The `foreach` statement enumerates the elements of a collection, executing an embedded statement for each element of the collection.</span></span>

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

<span data-ttu-id="efddf-355">`foreach`语句的*类型*和*标识符*声明语句的***迭代变量***。</span><span class="sxs-lookup"><span data-stu-id="efddf-355">The *type* and *identifier* of a `foreach` statement declare the ***iteration variable*** of the statement.</span></span> <span data-ttu-id="efddf-356">如果 `var` 标识符被指定为*local_variable_type*，并且没有任何名为 `var` 的类型在范围内，则将迭代变量称为***隐式类型的迭代变量***，并将其类型视为 @no__t 的元素类型。语句，如下所示。</span><span class="sxs-lookup"><span data-stu-id="efddf-356">If the `var` identifier is given as the *local_variable_type*, and no type named `var` is in scope, the iteration variable is said to be an ***implicitly typed iteration variable***, and its type is taken to be the element type of the `foreach` statement, as specified below.</span></span> <span data-ttu-id="efddf-357">迭代变量对应于一个只读局部变量，该局部变量具有扩展到嵌入语句的范围。</span><span class="sxs-lookup"><span data-stu-id="efddf-357">The iteration variable corresponds to a read-only local variable with a scope that extends over the embedded statement.</span></span> <span data-ttu-id="efddf-358">在`foreach`语句执行过程中，迭代变量表示当前正在为其执行迭代的集合元素。</span><span class="sxs-lookup"><span data-stu-id="efddf-358">During execution of a `foreach` statement, the iteration variable represents the collection element for which an iteration is currently being performed.</span></span> <span data-ttu-id="efddf-359">如果嵌入的语句尝试修改迭代变量（通过赋值`++`或和`--`运算符），或将迭代变量作为`ref`或`out`参数传递，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="efddf-359">A compile-time error occurs if the embedded statement attempts to modify the iteration variable (via assignment or the `++` and `--` operators) or pass the iteration variable as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="efddf-360">在下面的中，为简洁`IEnumerable`起见`IEnumerator`， `IEnumerable<T>` 、 `IEnumerator<T>`和引用命名空间`System.Collections`和`System.Collections.Generic`中的相应类型。</span><span class="sxs-lookup"><span data-stu-id="efddf-360">In the following, for brevity, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` and `IEnumerator<T>` refer to the corresponding types in the namespaces `System.Collections` and `System.Collections.Generic`.</span></span>

<span data-ttu-id="efddf-361">Foreach 语句的编译时处理首先确定表达式的***集合类型***、***枚举器类型***和***元素类型***。</span><span class="sxs-lookup"><span data-stu-id="efddf-361">The compile-time processing of a foreach statement first determines the ***collection type***, ***enumerator type*** and ***element type*** of the expression.</span></span> <span data-ttu-id="efddf-362">这种决定将按如下方式进行：</span><span class="sxs-lookup"><span data-stu-id="efddf-362">This determination proceeds as follows:</span></span>

*  <span data-ttu-id="efddf-363">如果表达式的`X`类型是数组类型，则存在从`X`到`IEnumerable`接口的隐式引用转换（因为`System.Array`实现了此接口）。</span><span class="sxs-lookup"><span data-stu-id="efddf-363">If the type `X` of *expression* is an array type then there is an implicit reference conversion from `X` to the `IEnumerable` interface (since `System.Array` implements this interface).</span></span> <span data-ttu-id="efddf-364">***集合类型***是`IEnumerable`接口，***枚举器类型***为`IEnumerator`接口，***元素类型***是数组类型`X`的元素类型。</span><span class="sxs-lookup"><span data-stu-id="efddf-364">The ***collection type*** is the `IEnumerable` interface, the ***enumerator type*** is the `IEnumerator` interface and the ***element type*** is the element type of the array type `X`.</span></span>
*  <span data-ttu-id="efddf-365">如果表达式的`X`类型为， `dynamic`则从*expression*到`IEnumerable` interface （[隐式动态转换](conversions.md#implicit-dynamic-conversions)）的隐式转换。</span><span class="sxs-lookup"><span data-stu-id="efddf-365">If the type `X` of *expression* is `dynamic` then there is an implicit conversion from *expression* to the `IEnumerable` interface ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)).</span></span> <span data-ttu-id="efddf-366">***集合类型***是`IEnumerable`接口，而***枚举器类型***是`IEnumerator`接口。</span><span class="sxs-lookup"><span data-stu-id="efddf-366">The ***collection type*** is the `IEnumerable` interface and the ***enumerator type*** is the `IEnumerator` interface.</span></span> <span data-ttu-id="efddf-367">如果将 @no__t 0 标识符指定为*local_variable_type* ，则***元素类型***为 `dynamic`，否则为 @no__t。</span><span class="sxs-lookup"><span data-stu-id="efddf-367">If the `var` identifier is given as the *local_variable_type* then the ***element type*** is `dynamic`, otherwise it is `object`.</span></span>
*  <span data-ttu-id="efddf-368">否则，请确定该类型`X`是否具有适当`GetEnumerator`的方法：</span><span class="sxs-lookup"><span data-stu-id="efddf-368">Otherwise, determine whether the type `X` has an appropriate `GetEnumerator` method:</span></span>
   * <span data-ttu-id="efddf-369">对具有标识符`GetEnumerator`且无类型`X`参数的类型执行成员查找。</span><span class="sxs-lookup"><span data-stu-id="efddf-369">Perform member lookup on the type `X` with identifier `GetEnumerator` and no type arguments.</span></span> <span data-ttu-id="efddf-370">如果成员查找不会生成匹配项，或者它产生了多义性，或者产生了不是方法组的匹配项，请检查可枚举的接口，如下所述。</span><span class="sxs-lookup"><span data-stu-id="efddf-370">If the member lookup does not produce a match, or it produces an ambiguity, or produces a match that is not a method group, check for an enumerable interface as described below.</span></span> <span data-ttu-id="efddf-371">如果成员查找产生了除方法组以外的任何内容，则建议发出警告。</span><span class="sxs-lookup"><span data-stu-id="efddf-371">It is recommended that a warning be issued if member lookup produces anything except a method group or no match.</span></span>
   * <span data-ttu-id="efddf-372">使用生成的方法组和空参数列表执行重载决策。</span><span class="sxs-lookup"><span data-stu-id="efddf-372">Perform overload resolution using the resulting method group and an empty argument list.</span></span> <span data-ttu-id="efddf-373">如果重载决策导致没有适用的方法、导致歧义或产生单个最佳方法，但该方法是静态的或非公共的，请按如下所述检查可枚举的接口。</span><span class="sxs-lookup"><span data-stu-id="efddf-373">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, check for an enumerable interface as described below.</span></span> <span data-ttu-id="efddf-374">如果重载决策产生了除明确的公共实例方法之外的任何内容，或者没有适用方法，则建议发出警告。</span><span class="sxs-lookup"><span data-stu-id="efddf-374">It is recommended that a warning be issued if overload resolution produces anything except an unambiguous public instance method or no applicable methods.</span></span>
   * <span data-ttu-id="efddf-375">如果该`GetEnumerator`方法的`E`返回类型不是类、结构或接口类型，则会生成错误，并且不会执行任何其他步骤。</span><span class="sxs-lookup"><span data-stu-id="efddf-375">If the return type `E` of the `GetEnumerator` method is not a class, struct or interface type, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="efddf-376">成员查找是在上`E`用标识符`Current`而不是类型参数执行的。</span><span class="sxs-lookup"><span data-stu-id="efddf-376">Member lookup is performed on `E` with the identifier `Current` and no type arguments.</span></span> <span data-ttu-id="efddf-377">如果成员查找没有生成任何匹配项，则结果为错误，或结果为除允许读取的公共实例属性之外的任何内容，并不执行任何其他步骤。</span><span class="sxs-lookup"><span data-stu-id="efddf-377">If the member lookup produces no match, the result is an error, or the result is anything except a public instance property that permits reading, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="efddf-378">成员查找是在上`E`用标识符`MoveNext`而不是类型参数执行的。</span><span class="sxs-lookup"><span data-stu-id="efddf-378">Member lookup is performed on `E` with the identifier `MoveNext` and no type arguments.</span></span> <span data-ttu-id="efddf-379">如果成员查找没有生成任何匹配项，则结果为错误，或结果为除方法组外的任何内容，并不执行任何其他步骤。</span><span class="sxs-lookup"><span data-stu-id="efddf-379">If the member lookup produces no match, the result is an error, or the result is anything except a method group, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="efddf-380">使用空参数列表对方法组执行重载决策。</span><span class="sxs-lookup"><span data-stu-id="efddf-380">Overload resolution is performed on the method group with an empty argument list.</span></span> <span data-ttu-id="efddf-381">如果重载决策导致没有适用的方法，导致不确定性，或者导致单个最佳方法，但该方法是静态的或非公共的，或者它的返回类型不`bool`是，则会生成错误，并且不会执行任何其他步骤。</span><span class="sxs-lookup"><span data-stu-id="efddf-381">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, or its return type is not `bool`, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="efddf-382">***集合类型***为`X`，***枚举器类型***为`E`，***元素类型***为`Current`属性的类型。</span><span class="sxs-lookup"><span data-stu-id="efddf-382">The ***collection type*** is `X`, the ***enumerator type*** is `E`, and the ***element type*** is the type of the `Current` property.</span></span>

*  <span data-ttu-id="efddf-383">否则，请检查可枚举的接口：</span><span class="sxs-lookup"><span data-stu-id="efddf-383">Otherwise, check for an enumerable interface:</span></span>
   * <span data-ttu-id="efddf-384">`Ti`如果存在从`dynamic` `T` `Ti` `T`到的隐式转换的所有类型，则有一种独特的类型，这种类型不是，对于所有其他类型， `IEnumerable<Ti>` `X`从`IEnumerable<T>`到`IEnumerator<T>` `IEnumerable<T>`的隐式转换，则集合类型为接口，枚举器类型为接口，元素类型为`IEnumerable<Ti>` `T`.</span><span class="sxs-lookup"><span data-stu-id="efddf-384">If among all the types `Ti` for which there is an implicit conversion from `X` to `IEnumerable<Ti>`, there is a unique type `T` such that `T` is not `dynamic` and for all the other `Ti` there is an implicit conversion from `IEnumerable<T>` to `IEnumerable<Ti>`, then the ***collection type*** is the interface `IEnumerable<T>`, the ***enumerator type*** is the interface `IEnumerator<T>`, and the ***element type*** is `T`.</span></span>
   * <span data-ttu-id="efddf-385">否则，如果有多个这样的类型`T`，则会生成错误，并且不会执行任何其他步骤。</span><span class="sxs-lookup"><span data-stu-id="efddf-385">Otherwise, if there is more than one such type `T`, then an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="efddf-386">否则，如果存在`X`从`System.Collections.IEnumerable`到接口的隐式转换，则***集合类型***为此接口，***枚举器类型***为接口`System.Collections.IEnumerator`，***元素类型***为`object`.</span><span class="sxs-lookup"><span data-stu-id="efddf-386">Otherwise, if there is an implicit conversion from `X` to the `System.Collections.IEnumerable` interface, then the ***collection type*** is this interface, the ***enumerator type*** is the interface `System.Collections.IEnumerator`, and the ***element type*** is `object`.</span></span>
   * <span data-ttu-id="efddf-387">否则，将生成错误，并且不执行任何其他步骤。</span><span class="sxs-lookup"><span data-stu-id="efddf-387">Otherwise, an error is produced and no further steps are taken.</span></span>

<span data-ttu-id="efddf-388">如果成功，上述步骤会明确产生集合类型`C`、枚举器类型`E`和元素类型`T`。</span><span class="sxs-lookup"><span data-stu-id="efddf-388">The above steps, if successful, unambiguously produce a collection type `C`, enumerator type `E` and element type `T`.</span></span> <span data-ttu-id="efddf-389">窗体的 foreach 语句</span><span class="sxs-lookup"><span data-stu-id="efddf-389">A foreach statement of the form</span></span>
```csharp
foreach (V v in x) embedded_statement
```
<span data-ttu-id="efddf-390">然后扩展为：</span><span class="sxs-lookup"><span data-stu-id="efddf-390">is then expanded to:</span></span>
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

<span data-ttu-id="efddf-391">变量`e`对表达式`x`或嵌入语句或程序的任何其他源代码都不可见或不可访问。</span><span class="sxs-lookup"><span data-stu-id="efddf-391">The variable `e` is not visible to or accessible to the expression `x` or the embedded statement or any other source code of the program.</span></span> <span data-ttu-id="efddf-392">此变量`v`在嵌入语句中是只读的。</span><span class="sxs-lookup"><span data-stu-id="efddf-392">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="efddf-393">如果没有从 `T` （元素类型）到 `V` （foreach 语句中的*local_variable_type* ）的显式转换（[显式](conversions.md#explicit-conversions)转换），则会生成错误，并且不会执行任何其他步骤。</span><span class="sxs-lookup"><span data-stu-id="efddf-393">If there is not an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) from `T` (the element type) to `V` (the *local_variable_type* in the foreach statement), an error is produced and no further steps are taken.</span></span> <span data-ttu-id="efddf-394">如果`x`具有值`null`， `System.NullReferenceException`则会在运行时引发。</span><span class="sxs-lookup"><span data-stu-id="efddf-394">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

<span data-ttu-id="efddf-395">允许实现以不同的方式实现给定的 foreach 语句（例如出于性能原因），前提是该行为与上述扩展一致。</span><span class="sxs-lookup"><span data-stu-id="efddf-395">An implementation is permitted to implement a given foreach-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="efddf-396">While 循环内 `v` 的位置对于*embedded_statement*中发生的任何匿名函数的捕获是非常重要的。</span><span class="sxs-lookup"><span data-stu-id="efddf-396">The placement of `v` inside the while loop is important for how it is captured by any anonymous function occurring in the *embedded_statement*.</span></span>

<span data-ttu-id="efddf-397">例如：</span><span class="sxs-lookup"><span data-stu-id="efddf-397">For example:</span></span>
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
<span data-ttu-id="efddf-398">如果`v`是在 while 循环之外声明的，则会在所有迭代中共享该循环，而 for 循环之后它的值将是最终`13`值，这`f`是对的调用将打印到的值。</span><span class="sxs-lookup"><span data-stu-id="efddf-398">If `v` was declared outside of the while loop, it would be shared among all iterations, and its value after the for loop would be the final value, `13`, which is what the invocation of `f` would print.</span></span> <span data-ttu-id="efddf-399">相反，因为每个迭代都有其`v`自己的变量，所以`f` ，第一次迭代中捕获的将继续`7`保存值，这就是要打印的值。</span><span class="sxs-lookup"><span data-stu-id="efddf-399">Instead, because each iteration has its own variable `v`, the one captured by `f` in the first iteration will continue to hold the value `7`, which is what will be printed.</span></span> <span data-ttu-id="efddf-400">（注意： while 循环外部C#声明`v`的早期版本。）</span><span class="sxs-lookup"><span data-stu-id="efddf-400">(Note: earlier versions of C# declared `v` outside of the while loop.)</span></span>

<span data-ttu-id="efddf-401">Finally 块的主体按照以下步骤进行构造：</span><span class="sxs-lookup"><span data-stu-id="efddf-401">The body of the finally block is constructed according to the following steps:</span></span>

*  <span data-ttu-id="efddf-402">如果存在从`E` `System.IDisposable`到接口的隐式转换，则</span><span class="sxs-lookup"><span data-stu-id="efddf-402">If there is an implicit conversion from `E` to the `System.IDisposable` interface, then</span></span>
   *  <span data-ttu-id="efddf-403">如果`E`是不可以为 null 的值类型，则将 finally 子句扩展为语义等效项：</span><span class="sxs-lookup"><span data-stu-id="efddf-403">If `E` is a non-nullable value type then the finally clause is expanded to the semantic equivalent  of:</span></span>

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  <span data-ttu-id="efddf-404">否则，finally 子句将扩展到语义等效项：</span><span class="sxs-lookup"><span data-stu-id="efddf-404">Otherwise the finally clause is expanded to the semantic equivalent of:</span></span>

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   <span data-ttu-id="efddf-405">除了如果`E`是值类型或实例化为值类型的类型参数，的`e`强制转换`System.IDisposable`不会导致装箱发生。</span><span class="sxs-lookup"><span data-stu-id="efddf-405">except that if `E` is a value type, or a type parameter instantiated to a value type, then the cast of `e` to `System.IDisposable` will not cause boxing to occur.</span></span>

*  <span data-ttu-id="efddf-406">否则，如果`E`是密封类型，则将 finally 子句扩展为空块：</span><span class="sxs-lookup"><span data-stu-id="efddf-406">Otherwise, if `E` is a sealed type, the finally clause is expanded to an empty block:</span></span>

   ```csharp
   finally {
   }
   ```

*  <span data-ttu-id="efddf-407">否则，finally 子句将扩展为：</span><span class="sxs-lookup"><span data-stu-id="efddf-407">Otherwise, the finally clause is expanded to:</span></span>

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   <span data-ttu-id="efddf-408">本地变量`d`对任何用户代码都不可见或不可访问。</span><span class="sxs-lookup"><span data-stu-id="efddf-408">The local variable `d` is not visible to or accessible to any user code.</span></span> <span data-ttu-id="efddf-409">具体而言，它不会与范围包含 finally 块的任何其他变量发生冲突。</span><span class="sxs-lookup"><span data-stu-id="efddf-409">In particular, it does not conflict with any other variable whose scope includes the finally block.</span></span>

<span data-ttu-id="efddf-410">`foreach`遍历数组元素的顺序如下所示：对于一维数组元素，按递增的索引顺序遍历，从 index `0`开始，以 index `Length - 1`结束。</span><span class="sxs-lookup"><span data-stu-id="efddf-410">The order in which `foreach` traverses the elements of an array, is as follows: For single-dimensional arrays elements are traversed in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="efddf-411">对于多维数组，会遍历元素，以便先增加最右侧维度的索引，然后再增加下一个左侧维度，依此类推。</span><span class="sxs-lookup"><span data-stu-id="efddf-411">For multi-dimensional arrays, elements are traversed such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span>

<span data-ttu-id="efddf-412">下面的示例按元素顺序输出二维数组中的每个值：</span><span class="sxs-lookup"><span data-stu-id="efddf-412">The following example prints out each value in a two-dimensional array, in element order:</span></span>
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
<span data-ttu-id="efddf-413">生成的输出如下所示：</span><span class="sxs-lookup"><span data-stu-id="efddf-413">The output produced is as follows:</span></span>
```console
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

<span data-ttu-id="efddf-414">示例中</span><span class="sxs-lookup"><span data-stu-id="efddf-414">In the example</span></span>
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
<span data-ttu-id="efddf-415">的类型`n`被推断`int`为`numbers`，元素类型为。</span><span class="sxs-lookup"><span data-stu-id="efddf-415">the type of `n` is inferred to be `int`, the element type of `numbers`.</span></span>

## <a name="jump-statements"></a><span data-ttu-id="efddf-416">跳转语句</span><span class="sxs-lookup"><span data-stu-id="efddf-416">Jump statements</span></span>

<span data-ttu-id="efddf-417">跳转语句无条件传输控制。</span><span class="sxs-lookup"><span data-stu-id="efddf-417">Jump statements unconditionally transfer control.</span></span>

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

<span data-ttu-id="efddf-418">跳转语句将控制转移到的位置称为跳转语句的***目标***。</span><span class="sxs-lookup"><span data-stu-id="efddf-418">The location to which a jump statement transfers control is called the ***target*** of the jump statement.</span></span>

<span data-ttu-id="efddf-419">如果跳转语句发生在块中，并且该跳转语句的目标在该块外，则可以使用跳转语句***退出***块。</span><span class="sxs-lookup"><span data-stu-id="efddf-419">When a jump statement occurs within a block, and the target of that jump statement is outside that block, the jump statement is said to ***exit*** the block.</span></span> <span data-ttu-id="efddf-420">尽管一个跳转语句可以将控制转移到块外，但它永远不能将控制转移到块中。</span><span class="sxs-lookup"><span data-stu-id="efddf-420">While a jump statement may transfer control out of a block, it can never transfer control into a block.</span></span>

<span data-ttu-id="efddf-421">由于存在干预`try`语句，跳转语句的执行很复杂。</span><span class="sxs-lookup"><span data-stu-id="efddf-421">Execution of jump statements is complicated by the presence of intervening `try` statements.</span></span> <span data-ttu-id="efddf-422">在缺少此类`try`语句的情况下，跳转语句会无条件地将控制从跳转语句转移到其目标。</span><span class="sxs-lookup"><span data-stu-id="efddf-422">In the absence of such `try` statements, a jump statement unconditionally transfers control from the jump statement to its target.</span></span> <span data-ttu-id="efddf-423">如果存在此类干预`try`语句，执行将更复杂。</span><span class="sxs-lookup"><span data-stu-id="efddf-423">In the presence of such intervening `try` statements, execution is more complex.</span></span> <span data-ttu-id="efddf-424">如果跳转语句退出一个或多`try`个具有关联`finally`块的块， `finally`则控件最初会传输到最内层`try`语句的块。</span><span class="sxs-lookup"><span data-stu-id="efddf-424">If the jump statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="efddf-425">当和如果控件到达`finally`块的终点时，控制将被传输`finally`到下一个封闭`try`语句的块。</span><span class="sxs-lookup"><span data-stu-id="efddf-425">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="efddf-426">此过程将重复进行， `finally`直到执行了所有`try`干预语句的块。</span><span class="sxs-lookup"><span data-stu-id="efddf-426">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>

<span data-ttu-id="efddf-427">示例中</span><span class="sxs-lookup"><span data-stu-id="efddf-427">In the example</span></span>
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
<span data-ttu-id="efddf-428">在`finally`控件传输到跳`try`转语句的目标之前，将执行与两个语句关联的块。</span><span class="sxs-lookup"><span data-stu-id="efddf-428">the `finally` blocks associated with two `try` statements are executed before control is transferred to the target of the jump statement.</span></span>

<span data-ttu-id="efddf-429">生成的输出如下所示：</span><span class="sxs-lookup"><span data-stu-id="efddf-429">The output produced is as follows:</span></span>
```console
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a><span data-ttu-id="efddf-430">Break 语句</span><span class="sxs-lookup"><span data-stu-id="efddf-430">The break statement</span></span>

<span data-ttu-id="efddf-431">`switch` `while` `do` `for`语句退出最近的包含、、、或`foreach`语句。 `break`</span><span class="sxs-lookup"><span data-stu-id="efddf-431">The `break` statement exits the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
break_statement
    : 'break' ';'
    ;
```

<span data-ttu-id="efddf-432">`break`语句的目标是最近的封闭`switch`、 `while` `do`、、 `for`或`foreach`语句的终点。</span><span class="sxs-lookup"><span data-stu-id="efddf-432">The target of a `break` statement is the end point of the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="efddf-433">`switch`如果语句未`while`由、 、`do` 、或`foreach`语句括起来，则会发生编译时错误。 `for` `break`</span><span class="sxs-lookup"><span data-stu-id="efddf-433">If a `break` statement is not enclosed by a `switch`, `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="efddf-434">当多`switch`个`while`、 `do`、 、`for`或`foreach`语句彼此嵌套时， `break`语句仅适用于最内层的语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-434">When multiple `switch`, `while`, `do`, `for`, or `foreach` statements are nested within each other, a `break` statement applies only to the innermost statement.</span></span> <span data-ttu-id="efddf-435">若要跨多个嵌套级别传输控制`goto` ，必须使用语句（[goto 语句](statements.md#the-goto-statement)）。</span><span class="sxs-lookup"><span data-stu-id="efddf-435">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="efddf-436">语句不能`finally`退出块（[try 语句](statements.md#the-try-statement)）。 `break`</span><span class="sxs-lookup"><span data-stu-id="efddf-436">A `break` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="efddf-437">当语句在块中出现时`break` ，语句的目标必须在同一`finally`块内; 否则，将发生编译时错误。 `finally` `break`</span><span class="sxs-lookup"><span data-stu-id="efddf-437">When a `break` statement occurs within a `finally` block, the target of the `break` statement must be within the same `finally` block; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="efddf-438">`break`语句的执行方式如下：</span><span class="sxs-lookup"><span data-stu-id="efddf-438">A `break` statement is executed as follows:</span></span>

*  <span data-ttu-id="efddf-439">`try` `finally` `finally` `try`如果语句退出一个或多个具有关联块的块，则控件最初会传输到最内层语句的块。 `break`</span><span class="sxs-lookup"><span data-stu-id="efddf-439">If the `break` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="efddf-440">当和如果控件到达`finally`块的终点时，控制将被传输`finally`到下一个封闭`try`语句的块。</span><span class="sxs-lookup"><span data-stu-id="efddf-440">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="efddf-441">此过程将重复进行， `finally`直到执行了所有`try`干预语句的块。</span><span class="sxs-lookup"><span data-stu-id="efddf-441">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="efddf-442">控制将转移到`break`语句的目标。</span><span class="sxs-lookup"><span data-stu-id="efddf-442">Control is transferred to the target of the `break` statement.</span></span>

<span data-ttu-id="efddf-443">由于语句无条件地将控制转移到其他位置，因此无法`break`访问语句的结束点。 `break`</span><span class="sxs-lookup"><span data-stu-id="efddf-443">Because a `break` statement unconditionally transfers control elsewhere, the end point of a `break` statement is never reachable.</span></span>

### <a name="the-continue-statement"></a><span data-ttu-id="efddf-444">Continue 语句</span><span class="sxs-lookup"><span data-stu-id="efddf-444">The continue statement</span></span>

<span data-ttu-id="efddf-445">`while` `do` `for`语句启动最近的封闭、、或`foreach`语句的新迭代。 `continue`</span><span class="sxs-lookup"><span data-stu-id="efddf-445">The `continue` statement starts a new iteration of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
continue_statement
    : 'continue' ';'
    ;
```

<span data-ttu-id="efddf-446">`continue`语句的目标是最近的封闭`while`、 `do`、 `for`或`foreach`语句的嵌入语句的终点。</span><span class="sxs-lookup"><span data-stu-id="efddf-446">The target of a `continue` statement is the end point of the embedded statement of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="efddf-447">`while` `do`如果语句未由、 、`for`或`foreach`语句括起来，则会发生编译时错误。 `continue`</span><span class="sxs-lookup"><span data-stu-id="efddf-447">If a `continue` statement is not enclosed by a `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="efddf-448">当多`while`个`do`、 `for`、 `continue`或`foreach`语句彼此嵌套时，语句仅适用于最内层的语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-448">When multiple `while`, `do`, `for`, or `foreach` statements are nested within each other, a `continue` statement applies only to the innermost statement.</span></span> <span data-ttu-id="efddf-449">若要跨多个嵌套级别传输控制`goto` ，必须使用语句（[goto 语句](statements.md#the-goto-statement)）。</span><span class="sxs-lookup"><span data-stu-id="efddf-449">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="efddf-450">语句不能`finally`退出块（[try 语句](statements.md#the-try-statement)）。 `continue`</span><span class="sxs-lookup"><span data-stu-id="efddf-450">A `continue` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="efddf-451">当语句在块中出现时`continue` ，语句的目标必须在同一`finally`块内; 否则，将发生编译时错误。 `finally` `continue`</span><span class="sxs-lookup"><span data-stu-id="efddf-451">When a `continue` statement occurs within a `finally` block, the target of the `continue` statement must be within the same `finally` block; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="efddf-452">`continue`语句的执行方式如下：</span><span class="sxs-lookup"><span data-stu-id="efddf-452">A `continue` statement is executed as follows:</span></span>

*  <span data-ttu-id="efddf-453">`try` `finally` `finally` `try`如果语句退出一个或多个具有关联块的块，则控件最初会传输到最内层语句的块。 `continue`</span><span class="sxs-lookup"><span data-stu-id="efddf-453">If the `continue` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="efddf-454">当和如果控件到达`finally`块的终点时，控制将被传输`finally`到下一个封闭`try`语句的块。</span><span class="sxs-lookup"><span data-stu-id="efddf-454">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="efddf-455">此过程将重复进行， `finally`直到执行了所有`try`干预语句的块。</span><span class="sxs-lookup"><span data-stu-id="efddf-455">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="efddf-456">控制将转移到`continue`语句的目标。</span><span class="sxs-lookup"><span data-stu-id="efddf-456">Control is transferred to the target of the `continue` statement.</span></span>

<span data-ttu-id="efddf-457">由于语句无条件地将控制转移到其他位置，因此无法`continue`访问语句的结束点。 `continue`</span><span class="sxs-lookup"><span data-stu-id="efddf-457">Because a `continue` statement unconditionally transfers control elsewhere, the end point of a `continue` statement is never reachable.</span></span>

### <a name="the-goto-statement"></a><span data-ttu-id="efddf-458">goto 语句</span><span class="sxs-lookup"><span data-stu-id="efddf-458">The goto statement</span></span>

<span data-ttu-id="efddf-459">`goto`语句将控制转移到由标签标记的语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-459">The `goto` statement transfers control to a statement that is marked by a label.</span></span>

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

<span data-ttu-id="efddf-460">*标识符*语句的目标`goto`是带有给定标签的标记的语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-460">The target of a `goto` *identifier* statement is the labeled statement with the given label.</span></span> <span data-ttu-id="efddf-461">如果当前函数成员中不存在具有给定名称的标签，或者如果该`goto`语句不在标签范围内，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="efddf-461">If a label with the given name does not exist in the current function member, or if the `goto` statement is not within the scope of the label, a compile-time error occurs.</span></span> <span data-ttu-id="efddf-462">此规则允许使用`goto`语句将控制转移出嵌套作用域，而不是嵌套作用域。</span><span class="sxs-lookup"><span data-stu-id="efddf-462">This rule permits the use of a `goto` statement to transfer control out of a nested scope, but not into a nested scope.</span></span> <span data-ttu-id="efddf-463">示例中</span><span class="sxs-lookup"><span data-stu-id="efddf-463">In the example</span></span>
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
<span data-ttu-id="efddf-464">`goto`语句用于将控制转移出嵌套作用域。</span><span class="sxs-lookup"><span data-stu-id="efddf-464">a `goto` statement is used to transfer control out of a nested scope.</span></span>

<span data-ttu-id="efddf-465">`goto case`语句的目标是直接封闭`switch`语句（[switch](statements.md#the-switch-statement) `case`语句）中的语句列表，其中包含具有给定常数值的标签。</span><span class="sxs-lookup"><span data-stu-id="efddf-465">The target of a `goto case` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `case` label with the given constant value.</span></span> <span data-ttu-id="efddf-466">如果 `goto case` 语句未括在 @no__t 1 语句中，则如果*constant_expression*不能隐式转换（[隐式转换](conversions.md#implicit-conversions)）到最近封闭的 `switch` 语句的管理类型，或者最近的封闭`switch` 语句不包含具有给定常数值的 @no__t 6 标签，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="efddf-466">If the `goto case` statement is not enclosed by a `switch` statement, if the *constant_expression* is not implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the nearest enclosing `switch` statement, or if the nearest enclosing `switch` statement does not contain a `case` label with the given constant value, a compile-time error occurs.</span></span>

<span data-ttu-id="efddf-467">`goto default`语句的目标是直接封闭`switch`语句（[switch 语句](statements.md#the-switch-statement)）中的语句列表，其中包含一个`default`标签。</span><span class="sxs-lookup"><span data-stu-id="efddf-467">The target of a `goto default` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `default` label.</span></span> <span data-ttu-id="efddf-468">如果语句未`switch`包含在语句内，或者最近的封闭`switch`语句不包含`default`标签，则会发生编译时错误。 `goto default`</span><span class="sxs-lookup"><span data-stu-id="efddf-468">If the `goto default` statement is not enclosed by a `switch` statement, or if the nearest enclosing `switch` statement does not contain a `default` label, a compile-time error occurs.</span></span>

<span data-ttu-id="efddf-469">语句不能`finally`退出块（[try 语句](statements.md#the-try-statement)）。 `goto`</span><span class="sxs-lookup"><span data-stu-id="efddf-469">A `goto` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="efddf-470">当语句在块中出现时`goto` ，语句的目标必须在同一`finally`块中，否则将发生编译时错误。 `finally` `goto`</span><span class="sxs-lookup"><span data-stu-id="efddf-470">When a `goto` statement occurs within a `finally` block, the target of the `goto` statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="efddf-471">`goto`语句的执行方式如下：</span><span class="sxs-lookup"><span data-stu-id="efddf-471">A `goto` statement is executed as follows:</span></span>

*  <span data-ttu-id="efddf-472">`try` `finally` `finally` `try`如果语句退出一个或多个具有关联块的块，则控件最初会传输到最内层语句的块。 `goto`</span><span class="sxs-lookup"><span data-stu-id="efddf-472">If the `goto` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="efddf-473">当和如果控件到达`finally`块的终点时，控制将被传输`finally`到下一个封闭`try`语句的块。</span><span class="sxs-lookup"><span data-stu-id="efddf-473">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="efddf-474">此过程将重复进行， `finally`直到执行了所有`try`干预语句的块。</span><span class="sxs-lookup"><span data-stu-id="efddf-474">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="efddf-475">控制将转移到`goto`语句的目标。</span><span class="sxs-lookup"><span data-stu-id="efddf-475">Control is transferred to the target of the `goto` statement.</span></span>

<span data-ttu-id="efddf-476">由于语句无条件地将控制转移到其他位置，因此无法`goto`访问语句的结束点。 `goto`</span><span class="sxs-lookup"><span data-stu-id="efddf-476">Because a `goto` statement unconditionally transfers control elsewhere, the end point of a `goto` statement is never reachable.</span></span>

### <a name="the-return-statement"></a><span data-ttu-id="efddf-477">Return 语句</span><span class="sxs-lookup"><span data-stu-id="efddf-477">The return statement</span></span>

<span data-ttu-id="efddf-478">语句将控制权返回给出现该`return`语句的函数的当前调用方。 `return`</span><span class="sxs-lookup"><span data-stu-id="efddf-478">The `return` statement returns control to the current caller of the function in which the `return` statement appears.</span></span>

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

<span data-ttu-id="efddf-479">`set` `add` `void` `return`不带 expression 的语句只能在不计算值的函数成员中使用，即，具有结果类型（[方法主体](classes.md#method-body)）的方法、属性或索引器的访问器，事件`remove` 、实例构造函数、静态构造函数或析构函数的和访问器。</span><span class="sxs-lookup"><span data-stu-id="efddf-479">A `return` statement with no expression can be used only in a function member that does not compute a value, that is, a method with the result type ([Method body](classes.md#method-body)) `void`, the `set` accessor of a property or indexer, the `add` and `remove` accessors of an event, an instance constructor, a static constructor, or a destructor.</span></span>

<span data-ttu-id="efddf-480">带有表达式的`get` 语句只能在计算值的函数成员中使用，即，具有非void结果类型的方法、属性或索引器的访问器或用户`return`定义的运算符。</span><span class="sxs-lookup"><span data-stu-id="efddf-480">A `return` statement with an expression can only be used in a function member that computes a value, that is, a method with a non-void result type, the `get` accessor of a property or indexer, or a user-defined operator.</span></span> <span data-ttu-id="efddf-481">隐式转换（[隐式](conversions.md#implicit-conversions)转换）必须存在于表达式的类型到包含函数成员的返回类型。</span><span class="sxs-lookup"><span data-stu-id="efddf-481">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression to the return type of the containing function member.</span></span>

<span data-ttu-id="efddf-482">Return 语句还可用于匿名函数表达式（[匿名函数表达式](expressions.md#anonymous-function-expressions)）的主体中，并参与确定哪些转换存在这些函数。</span><span class="sxs-lookup"><span data-stu-id="efddf-482">Return statements can also be used in the body of anonymous function expressions ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), and participate in determining which conversions exist for those functions.</span></span>

<span data-ttu-id="efddf-483">`return`语句出现`finally`在块中（[try 语句](statements.md#the-try-statement)）是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="efddf-483">It is a compile-time error for a `return` statement to appear in a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="efddf-484">`return`语句的执行方式如下：</span><span class="sxs-lookup"><span data-stu-id="efddf-484">A `return` statement is executed as follows:</span></span>

*  <span data-ttu-id="efddf-485">如果该`return`语句指定一个表达式，将计算该表达式，并通过隐式转换将生成的值转换为包含函数的返回类型。</span><span class="sxs-lookup"><span data-stu-id="efddf-485">If the `return` statement specifies an expression, the expression is evaluated and the resulting value is converted to the return type of the containing function by an implicit conversion.</span></span> <span data-ttu-id="efddf-486">转换的结果成为函数生成的结果值。</span><span class="sxs-lookup"><span data-stu-id="efddf-486">The result of the conversion becomes the result value produced by the function.</span></span>
*  <span data-ttu-id="efddf-487">`finally` `catch` `try` `finally`如果语句由一个或多个具有关联块的或块括起来，则控件最初将传输到最内层`try`语句的块。 `return`</span><span class="sxs-lookup"><span data-stu-id="efddf-487">If the `return` statement is enclosed by one or more `try` or `catch` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="efddf-488">当和如果控件到达`finally`块的终点时，控制将被传输`finally`到下一个封闭`try`语句的块。</span><span class="sxs-lookup"><span data-stu-id="efddf-488">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="efddf-489">此过程将重复进行， `finally`直至所有封闭`try`语句的块都已执行完毕。</span><span class="sxs-lookup"><span data-stu-id="efddf-489">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="efddf-490">如果包含函数不是异步函数，则会将控件返回到包含函数的调用方，同时返回结果值（如果有）。</span><span class="sxs-lookup"><span data-stu-id="efddf-490">If the containing function is not an async function, control is returned to the caller of the containing function along with the result value, if any.</span></span>
*  <span data-ttu-id="efddf-491">如果包含函数是一个异步函数，则将控制权返回给当前调用方，并在返回任务中记录结果值（如果有），如（[枚举器接口](classes.md#enumerator-interfaces)）中所述。</span><span class="sxs-lookup"><span data-stu-id="efddf-491">If the containing function is an async function, control is returned to the current caller, and the result value, if any, is recorded in the return task as described in ([Enumerator interfaces](classes.md#enumerator-interfaces)).</span></span>

<span data-ttu-id="efddf-492">由于语句无条件地将控制转移到其他位置，因此无法`return`访问语句的结束点。 `return`</span><span class="sxs-lookup"><span data-stu-id="efddf-492">Because a `return` statement unconditionally transfers control elsewhere, the end point of a `return` statement is never reachable.</span></span>

### <a name="the-throw-statement"></a><span data-ttu-id="efddf-493">Throw 语句</span><span class="sxs-lookup"><span data-stu-id="efddf-493">The throw statement</span></span>

<span data-ttu-id="efddf-494">`throw`语句引发异常。</span><span class="sxs-lookup"><span data-stu-id="efddf-494">The `throw` statement throws an exception.</span></span>

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

<span data-ttu-id="efddf-495">带有`throw`表达式的语句将引发通过计算表达式生成的值。</span><span class="sxs-lookup"><span data-stu-id="efddf-495">A `throw` statement with an expression throws the value produced by evaluating the expression.</span></span> <span data-ttu-id="efddf-496">表达式必须表示从派生的`System.Exception` `System.Exception`类类型的类类型的值，或具有`System.Exception`作为其有效基类的类型参数类型的类型参数类型的值。</span><span class="sxs-lookup"><span data-stu-id="efddf-496">The expression must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="efddf-497">如果表达式`null`的计算结果为`System.NullReferenceException` ，则改为引发。</span><span class="sxs-lookup"><span data-stu-id="efddf-497">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="efddf-498">不带 expression 的`catch` `catch`语句只能用在块中，在这种情况下，该语句会重新引发该块当前正在处理的异常。 `throw`</span><span class="sxs-lookup"><span data-stu-id="efddf-498">A `throw` statement with no expression can be used only in a `catch` block, in which case that statement re-throws the exception that is currently being handled by that `catch` block.</span></span>

<span data-ttu-id="efddf-499">由于语句无条件地将控制转移到其他位置，因此无法`throw`访问语句的结束点。 `throw`</span><span class="sxs-lookup"><span data-stu-id="efddf-499">Because a `throw` statement unconditionally transfers control elsewhere, the end point of a `throw` statement is never reachable.</span></span>

<span data-ttu-id="efddf-500">当引发异常时，控制将被传输到可处理`catch`异常的封闭`try`语句中的第一个子句。</span><span class="sxs-lookup"><span data-stu-id="efddf-500">When an exception is thrown, control is transferred to the first `catch` clause in an enclosing `try` statement that can handle the exception.</span></span> <span data-ttu-id="efddf-501">从引发的异常点到将控件传输到适当的异常处理程序的时间点的过程称为 "***异常传播***"。</span><span class="sxs-lookup"><span data-stu-id="efddf-501">The process that takes place from the point of the exception being thrown to the point of transferring control to a suitable exception handler is known as ***exception propagation***.</span></span> <span data-ttu-id="efddf-502">异常的传播包括重复计算以下步骤，直到`catch`找到匹配该异常的子句。</span><span class="sxs-lookup"><span data-stu-id="efddf-502">Propagation of an exception consists of repeatedly evaluating the following steps until a `catch` clause that matches the exception is found.</span></span> <span data-ttu-id="efddf-503">在此说明中，引发异常的位置最初为***引发点***的位置。</span><span class="sxs-lookup"><span data-stu-id="efddf-503">In this description, the ***throw point*** is initially the location at which the exception is thrown.</span></span>

*  <span data-ttu-id="efddf-504">在当前函数成员中，将`try`检查包含引发点的每个语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-504">In the current function member, each `try` statement that encloses the throw point is examined.</span></span> <span data-ttu-id="efddf-505">对于每个`S`语句，从最内层`try`的语句开始，到最`try`外面的语句结束，计算以下步骤：</span><span class="sxs-lookup"><span data-stu-id="efddf-505">For each statement `S`, starting with the innermost `try` statement and ending with the outermost `try` statement, the following steps are evaluated:</span></span>

   * <span data-ttu-id="efddf-506">`catch`如果的`try` `S`块包含一个`catch`或多个子句，则将按照外观顺序检查子句，以根据中指定的规则查找适用于异常的处理程序部分[。](statements.md#the-try-statement)</span><span class="sxs-lookup"><span data-stu-id="efddf-506">If the `try` block of `S` encloses the throw point and if S has one or more `catch` clauses, the `catch` clauses are examined in order of appearance to locate a suitable handler for the exception, according to the rules specified in Section [The try statement](statements.md#the-try-statement).</span></span> <span data-ttu-id="efddf-507">如果找到了`catch`匹配子句，则通过将控制转移到该`catch`子句的块来完成异常传播。</span><span class="sxs-lookup"><span data-stu-id="efddf-507">If a matching `catch` clause is located, the exception propagation is completed by transferring control to the block of that `catch` clause.</span></span>

   * <span data-ttu-id="efddf-508">`try`否则，如果块`catch`或块`S` `S` 封闭了引发点`finally` ，并且如果有块，则将控制转移到块。`finally`</span><span class="sxs-lookup"><span data-stu-id="efddf-508">Otherwise, if the `try` block or a `catch` block of `S` encloses the throw point and if `S` has a `finally` block, control is transferred to the `finally` block.</span></span> <span data-ttu-id="efddf-509">`finally`如果该块引发另一个异常，则终止当前异常的处理。</span><span class="sxs-lookup"><span data-stu-id="efddf-509">If the `finally` block throws another exception, processing of the current exception is terminated.</span></span> <span data-ttu-id="efddf-510">否则，当控件到达`finally`块的终点时，将继续处理当前异常。</span><span class="sxs-lookup"><span data-stu-id="efddf-510">Otherwise, when control reaches the end point of the `finally` block, processing of the current exception is continued.</span></span>

*  <span data-ttu-id="efddf-511">如果在当前函数调用中未找到异常处理程序，则终止函数调用，并发生以下情况之一：</span><span class="sxs-lookup"><span data-stu-id="efddf-511">If an exception handler was not located in the current function invocation, the function invocation is terminated, and one of the following occurs:</span></span>

   * <span data-ttu-id="efddf-512">如果当前函数为非异步，则会为函数调用方重复上述步骤，并将引发点与调用函数成员的语句对应。</span><span class="sxs-lookup"><span data-stu-id="efddf-512">If the current function is non-async, the steps above are repeated for the caller of the function with a throw point corresponding to the statement from which the function member was invoked.</span></span>

   * <span data-ttu-id="efddf-513">如果当前函数为 async 并返回任务，则会在返回任务中记录异常，该异常将被置于 "[枚举器接口](classes.md#enumerator-interfaces)" 中所述的 "出错" 或 "已取消" 状态。</span><span class="sxs-lookup"><span data-stu-id="efddf-513">If the current function is async and task-returning, the exception is recorded in the return task, which is put into a faulted or cancelled state as described in [Enumerator interfaces](classes.md#enumerator-interfaces).</span></span>

   * <span data-ttu-id="efddf-514">如果当前函数为 async 和 void 返回，则会通知当前线程的同步上下文，如可[枚举接口](classes.md#enumerable-interfaces)中所述。</span><span class="sxs-lookup"><span data-stu-id="efddf-514">If the current function is async and void-returning, the synchronization context of the current thread is notified as described in [Enumerable interfaces](classes.md#enumerable-interfaces).</span></span>

*  <span data-ttu-id="efddf-515">如果异常处理终止当前线程中的所有函数成员调用，指示该线程没有异常的处理程序，则该线程本身将终止。</span><span class="sxs-lookup"><span data-stu-id="efddf-515">If the exception processing terminates all function member invocations in the current thread, indicating that the thread has no handler for the exception, then the thread is itself terminated.</span></span> <span data-ttu-id="efddf-516">此类终止的影响是由实现定义的。</span><span class="sxs-lookup"><span data-stu-id="efddf-516">The impact of such termination is implementation-defined.</span></span>

## <a name="the-try-statement"></a><span data-ttu-id="efddf-517">Try 语句</span><span class="sxs-lookup"><span data-stu-id="efddf-517">The try statement</span></span>

<span data-ttu-id="efddf-518">`try`语句提供了一种机制，用于捕获在执行块期间发生的异常。</span><span class="sxs-lookup"><span data-stu-id="efddf-518">The `try` statement provides a mechanism for catching exceptions that occur during execution of a block.</span></span> <span data-ttu-id="efddf-519">而且， `try`语句提供了指定在控制`try`离开语句时始终执行的代码块的能力。</span><span class="sxs-lookup"><span data-stu-id="efddf-519">Furthermore, the `try` statement provides the ability to specify a block of code that is always executed when control leaves the `try` statement.</span></span>

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

<span data-ttu-id="efddf-520">有三种可能的`try`语句形式：</span><span class="sxs-lookup"><span data-stu-id="efddf-520">There are three possible forms of `try` statements:</span></span>

*  <span data-ttu-id="efddf-521">后跟一个或多个`catch`块的块。`try`</span><span class="sxs-lookup"><span data-stu-id="efddf-521">A `try` block followed by one or more `catch` blocks.</span></span>
*  <span data-ttu-id="efddf-522">后跟`finally`块的块。`try`</span><span class="sxs-lookup"><span data-stu-id="efddf-522">A `try` block followed by a `finally` block.</span></span>
*  <span data-ttu-id="efddf-523">后跟一个或多个`catch`块后跟`finally`一个块的块。`try`</span><span class="sxs-lookup"><span data-stu-id="efddf-523">A `try` block followed by one or more `catch` blocks followed by a `finally` block.</span></span>

<span data-ttu-id="efddf-524">当 `catch` 子句指定*exception_specifier*时，类型必须为 `System.Exception`、从 `System.Exception` 派生的类型或具有作为其有效基类的 @no__t 4 （或子类）的类型参数类型。</span><span class="sxs-lookup"><span data-stu-id="efddf-524">When a `catch` clause specifies an *exception_specifier*, the type must be `System.Exception`, a type that derives from `System.Exception` or a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span>

<span data-ttu-id="efddf-525">当 `catch` 子句同时指定具有*标识符*的*exception_specifier*时，将声明一个具有给定名称和类型的***异常变量***。</span><span class="sxs-lookup"><span data-stu-id="efddf-525">When a `catch` clause specifies both an *exception_specifier* with an *identifier*, an ***exception variable*** of the given name and type is declared.</span></span> <span data-ttu-id="efddf-526">异常变量对应于具有扩展`catch`子句的作用域的局部变量。</span><span class="sxs-lookup"><span data-stu-id="efddf-526">The exception variable corresponds to a local variable with a scope that extends over the `catch` clause.</span></span> <span data-ttu-id="efddf-527">在*exception_filter*和*块*的执行期间，异常变量表示当前正在处理的异常。</span><span class="sxs-lookup"><span data-stu-id="efddf-527">During execution of the *exception_filter* and *block*, the exception variable represents the exception currently being handled.</span></span> <span data-ttu-id="efddf-528">出于明确赋值检查的目的，在其整个范围内将异常变量视为明确赋值。</span><span class="sxs-lookup"><span data-stu-id="efddf-528">For purposes of definite assignment checking, the exception variable is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="efddf-529">除非子句包含异常变量名称，否则无法访问筛选器和`catch`块中的异常对象。 `catch`</span><span class="sxs-lookup"><span data-stu-id="efddf-529">Unless a `catch` clause includes an exception variable name, it is impossible to access the exception object in the filter and `catch` block.</span></span>

<span data-ttu-id="efddf-530">不指定*exception_specifier*的 @no__t 0 子句称为一般 @no__t 2 子句。</span><span class="sxs-lookup"><span data-stu-id="efddf-530">A `catch` clause that does not specify an *exception_specifier* is called a general `catch` clause.</span></span>

<span data-ttu-id="efddf-531">某些编程语言可能会支持不能表示为派生自`System.Exception`的对象的异常，但C#代码不会生成此类异常。</span><span class="sxs-lookup"><span data-stu-id="efddf-531">Some programming languages may support exceptions that are not representable as an object derived from `System.Exception`, although such exceptions could never be generated by C# code.</span></span> <span data-ttu-id="efddf-532">可以使用`catch`常规子句来捕获此类异常。</span><span class="sxs-lookup"><span data-stu-id="efddf-532">A general `catch` clause may be used to catch such exceptions.</span></span> <span data-ttu-id="efddf-533">因此，一般`catch`子句在语义上不同于指定类型`System.Exception`的规则，在这种情况下，前者还可以从其他语言中捕获异常。</span><span class="sxs-lookup"><span data-stu-id="efddf-533">Thus, a general `catch` clause is semantically different from one that specifies the type `System.Exception`, in that the former may also catch exceptions from other languages.</span></span>

<span data-ttu-id="efddf-534">为了定位异常的处理程序， `catch`按词法顺序检查子句。</span><span class="sxs-lookup"><span data-stu-id="efddf-534">In order to locate a handler for an exception, `catch` clauses are examined in lexical order.</span></span> <span data-ttu-id="efddf-535">如果子句指定一个类型，但没有指定异常筛选器，则在同一`try`语句中，后面`catch`的子句会出现编译时错误，以指定与该类型相同或派生自该类型的类型。 `catch`</span><span class="sxs-lookup"><span data-stu-id="efddf-535">If a `catch` clause specifies a type but no exception filter, it is a compile-time error for a later `catch` clause in the same `try` statement to specify a type that is the same as, or is derived from, that type.</span></span> <span data-ttu-id="efddf-536">如果子句未指定任何类型并且没有筛选器，则它必须是`catch`该`try`语句的最后一个子句。 `catch`</span><span class="sxs-lookup"><span data-stu-id="efddf-536">If a `catch` clause specifies no type and no filter, it must be the last `catch` clause for that `try` statement.</span></span>

<span data-ttu-id="efddf-537">在块中，无`throw`表达式的语句（[throw 语句](statements.md#the-throw-statement)）可用于重新引发`catch`块捕获的异常。 `catch`</span><span class="sxs-lookup"><span data-stu-id="efddf-537">Within a `catch` block, a `throw` statement ([The throw statement](statements.md#the-throw-statement)) with no expression can be used to re-throw the exception that was caught by the `catch` block.</span></span> <span data-ttu-id="efddf-538">对异常变量的赋值不会改变重新引发的异常。</span><span class="sxs-lookup"><span data-stu-id="efddf-538">Assignments to an exception variable do not alter the exception that is re-thrown.</span></span>

<span data-ttu-id="efddf-539">示例中</span><span class="sxs-lookup"><span data-stu-id="efddf-539">In the example</span></span>
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
<span data-ttu-id="efddf-540">方法`F`捕获异常，将一些诊断信息写入控制台，更改异常变量，并重新引发异常。</span><span class="sxs-lookup"><span data-stu-id="efddf-540">the method `F` catches an exception, writes some diagnostic information to the console, alters the exception variable, and re-throws the exception.</span></span> <span data-ttu-id="efddf-541">重新引发的异常是原始异常，因此生成的输出为：</span><span class="sxs-lookup"><span data-stu-id="efddf-541">The exception that is re-thrown is the original exception, so the output produced is:</span></span>
```console
Exception in F: G
Exception in Main: G
```

<span data-ttu-id="efddf-542">如果第一个 catch 块引发`e`了而不是重新引发当前异常，则生成的输出将如下所示：</span><span class="sxs-lookup"><span data-stu-id="efddf-542">If the first catch block had thrown `e` instead of rethrowing the current exception, the output produced would be as follows:</span></span>
```console
Exception in F: G
Exception in Main: F
```

<span data-ttu-id="efddf-543">`break` `finally` 、或`continue`语句将控制传输到块`goto`的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="efddf-543">It is a compile-time error for a `break`, `continue`, or `goto` statement to transfer control out of a `finally` block.</span></span> <span data-ttu-id="efddf-544">`continue` `goto` `finally`当在块`finally`中发生、或语句时，语句的目标必须在同一块中，否则将发生编译时错误。 `break`</span><span class="sxs-lookup"><span data-stu-id="efddf-544">When a `break`, `continue`, or `goto` statement occurs in a `finally` block, the target of the statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="efddf-545">`return`语句出现`finally`在块中是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="efddf-545">It is a compile-time error for a `return` statement to occur in a `finally` block.</span></span>

<span data-ttu-id="efddf-546">`try`语句的执行方式如下：</span><span class="sxs-lookup"><span data-stu-id="efddf-546">A `try` statement is executed as follows:</span></span>

*  <span data-ttu-id="efddf-547">控制传输到`try`块。</span><span class="sxs-lookup"><span data-stu-id="efddf-547">Control is transferred to the `try` block.</span></span>
*  <span data-ttu-id="efddf-548">当和如果控件到达`try`块的终结点时：</span><span class="sxs-lookup"><span data-stu-id="efddf-548">When and if control reaches the end point of the `try` block:</span></span>
   *  <span data-ttu-id="efddf-549">如果语句有块，则执行`finally`块。 `finally` `try`</span><span class="sxs-lookup"><span data-stu-id="efddf-549">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
   *  <span data-ttu-id="efddf-550">控制将转移到`try`语句的终点。</span><span class="sxs-lookup"><span data-stu-id="efddf-550">Control is transferred to the end point of the `try` statement.</span></span>

*  <span data-ttu-id="efddf-551">如果在执行`try`块期间将异常`try`传播到语句：</span><span class="sxs-lookup"><span data-stu-id="efddf-551">If an exception is propagated to the `try` statement during execution of the `try` block:</span></span>
   *  <span data-ttu-id="efddf-552">`catch`子句（如果有）将按外观的顺序进行检查，以查找适用于异常的处理程序。</span><span class="sxs-lookup"><span data-stu-id="efddf-552">The `catch` clauses, if any, are examined in order of appearance to locate a suitable handler for the exception.</span></span> <span data-ttu-id="efddf-553">`catch`如果子句未指定类型，或指定异常类型或异常类型的基类型，则为; 否则为。</span><span class="sxs-lookup"><span data-stu-id="efddf-553">If a `catch` clause does not specify a type, or specifies the exception type or a base type of the exception type:</span></span>
      *  <span data-ttu-id="efddf-554">`catch`如果子句声明了异常变量，则会将异常对象分配给异常变量。</span><span class="sxs-lookup"><span data-stu-id="efddf-554">If the `catch` clause declares an exception variable, the exception object is assigned to the exception variable.</span></span>
      *  <span data-ttu-id="efddf-555">`catch`如果子句声明了异常筛选器，则会计算筛选器。</span><span class="sxs-lookup"><span data-stu-id="efddf-555">If the `catch` clause declares an exception filter, the filter is evaluated.</span></span> <span data-ttu-id="efddf-556">如果其计算结果`false`为，则 catch 子句不是匹配项，并且搜索将继续经过适用`catch`于适当处理程序的任何后续子句。</span><span class="sxs-lookup"><span data-stu-id="efddf-556">If it evaluates to `false`, the catch clause is not a match, and the search continues through any subsequent `catch` clauses for a suitable handler.</span></span>
      *  <span data-ttu-id="efddf-557">否则， `catch`子句被视为匹配，并将控制转移到匹配`catch`的块。</span><span class="sxs-lookup"><span data-stu-id="efddf-557">Otherwise, the `catch` clause is considered a match, and control is transferred to the matching `catch` block.</span></span>
      *  <span data-ttu-id="efddf-558">当和如果控件到达`catch`块的终结点时：</span><span class="sxs-lookup"><span data-stu-id="efddf-558">When and if control reaches the end point of the `catch` block:</span></span>
         * <span data-ttu-id="efddf-559">如果语句有块，则执行`finally`块。 `finally` `try`</span><span class="sxs-lookup"><span data-stu-id="efddf-559">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         * <span data-ttu-id="efddf-560">控制将转移到`try`语句的终点。</span><span class="sxs-lookup"><span data-stu-id="efddf-560">Control is transferred to the end point of the `try` statement.</span></span>
      *  <span data-ttu-id="efddf-561">如果在执行`catch`块期间将异常`try`传播到语句：</span><span class="sxs-lookup"><span data-stu-id="efddf-561">If an exception is propagated to the `try` statement during execution of the `catch` block:</span></span>
         *  <span data-ttu-id="efddf-562">如果语句有块，则执行`finally`块。 `finally` `try`</span><span class="sxs-lookup"><span data-stu-id="efddf-562">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         *  <span data-ttu-id="efddf-563">异常将传播到下一个封闭`try`语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-563">The exception is propagated to the next enclosing `try` statement.</span></span>
   *  <span data-ttu-id="efddf-564">如果语句没有子句，或者如果没有`catch`子句与异常匹配，则为： `catch` `try`</span><span class="sxs-lookup"><span data-stu-id="efddf-564">If the `try` statement has no `catch` clauses or if no `catch` clause matches the exception:</span></span>
      *  <span data-ttu-id="efddf-565">如果语句有块，则执行`finally`块。 `finally` `try`</span><span class="sxs-lookup"><span data-stu-id="efddf-565">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
      *  <span data-ttu-id="efddf-566">异常将传播到下一个封闭`try`语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-566">The exception is propagated to the next enclosing `try` statement.</span></span>

<span data-ttu-id="efddf-567">当控制`finally` `try`离开语句时，始终会执行块的语句。</span><span class="sxs-lookup"><span data-stu-id="efddf-567">The statements of a `finally` block are always executed when control leaves a `try` statement.</span></span> <span data-ttu-id="efddf-568">无论是由于执行了`break`正常执行，还是由于执行、 `continue`、 `goto`或`return`语句而导致控件传输，或是由于将异常传播到`try`损益.</span><span class="sxs-lookup"><span data-stu-id="efddf-568">This is true whether the control transfer occurs as a result of normal execution, as a result of executing a `break`, `continue`, `goto`, or `return` statement, or as a result of propagating an exception out of the `try` statement.</span></span>

<span data-ttu-id="efddf-569">如果在执行`finally`块期间引发了异常，并且未在同一个 finally 块中捕获该异常，则该异常将传播到下一个`try`封闭语句中。</span><span class="sxs-lookup"><span data-stu-id="efddf-569">If an exception is thrown during execution of a `finally` block, and is not caught within the same finally block, the exception is propagated to the next enclosing `try` statement.</span></span> <span data-ttu-id="efddf-570">如果正在传播其他异常，则该异常将丢失。</span><span class="sxs-lookup"><span data-stu-id="efddf-570">If another exception was in the process of being propagated, that exception is lost.</span></span> <span data-ttu-id="efddf-571">传播异常的过程在`throw`语句的说明（[throw 语句](statements.md#the-throw-statement)）中进一步讨论。</span><span class="sxs-lookup"><span data-stu-id="efddf-571">The process of propagating an exception is discussed further in the description of the `throw` statement ([The throw statement](statements.md#the-throw-statement)).</span></span>

<span data-ttu-id="efddf-572">如果语句可访问`try` ，则可以访问`try` `try`语句块。</span><span class="sxs-lookup"><span data-stu-id="efddf-572">The `try` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="efddf-573">如果语句可访问`try` ，则可以访问`catch` `try`语句块。</span><span class="sxs-lookup"><span data-stu-id="efddf-573">A `catch` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="efddf-574">如果语句可访问`try` ，则可以访问`finally` `try`语句块。</span><span class="sxs-lookup"><span data-stu-id="efddf-574">The `finally` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="efddf-575">如果以下两个条件`try`均为 true，则可以访问语句的终结点：</span><span class="sxs-lookup"><span data-stu-id="efddf-575">The end point of a `try` statement is reachable if both of the following are true:</span></span>

*  <span data-ttu-id="efddf-576">`try`块的终结点是可访问的，或者至少有一个`catch`块可访问的终结点。</span><span class="sxs-lookup"><span data-stu-id="efddf-576">The end point of the `try` block is reachable or the end point of at least one `catch` block is reachable.</span></span>
*  <span data-ttu-id="efddf-577">如果存在`finally`块，则可到达块的终结点。 `finally`</span><span class="sxs-lookup"><span data-stu-id="efddf-577">If a `finally` block is present, the end point of the `finally` block is reachable.</span></span>

## <a name="the-checked-and-unchecked-statements"></a><span data-ttu-id="efddf-578">Checked 和 unchecked 语句</span><span class="sxs-lookup"><span data-stu-id="efddf-578">The checked and unchecked statements</span></span>

<span data-ttu-id="efddf-579">和语句用于控制整型算术运算和转换的***溢出检查上下文。*** `checked` `unchecked`</span><span class="sxs-lookup"><span data-stu-id="efddf-579">The `checked` and `unchecked` statements are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

<span data-ttu-id="efddf-580">语句导致在已检查的`unchecked`上下文中计算*块*中的所有表达式，语句导致在未检查的上下文中计算*块*中的所有表达式。 `checked`</span><span class="sxs-lookup"><span data-stu-id="efddf-580">The `checked` statement causes all expressions in the *block* to be evaluated in a checked context, and the `unchecked` statement causes all expressions in the *block* to be evaluated in an unchecked context.</span></span>

<span data-ttu-id="efddf-581">`checked` 和 `unchecked` 语句完全等效于`checked`和`unchecked`运算符（[checked 和 unchecked 运算符](expressions.md#the-checked-and-unchecked-operators)），只不过它们在块而不是表达式上操作。</span><span class="sxs-lookup"><span data-stu-id="efddf-581">The `checked` and `unchecked` statements are precisely equivalent to the `checked` and `unchecked` operators ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), except that they operate on blocks instead of expressions.</span></span>

## <a name="the-lock-statement"></a><span data-ttu-id="efddf-582">Lock 语句</span><span class="sxs-lookup"><span data-stu-id="efddf-582">The lock statement</span></span>

<span data-ttu-id="efddf-583">`lock`语句获取给定对象的互斥锁，执行语句，然后释放该锁。</span><span class="sxs-lookup"><span data-stu-id="efddf-583">The `lock` statement obtains the mutual-exclusion lock for a given object, executes a statement, and then releases the lock.</span></span>

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

<span data-ttu-id="efddf-584">@No__t-0 语句的表达式必须表示已知为*reference_type*的类型的值。</span><span class="sxs-lookup"><span data-stu-id="efddf-584">The expression of a `lock` statement must denote a value of a type known to be a *reference_type*.</span></span> <span data-ttu-id="efddf-585">对于 `lock` 语句的表达式，不会执行任何隐式装箱转换（[装箱转换](conversions.md#boxing-conversions)），因此，表达式会出现编译时错误，以表示*value_type*的值。</span><span class="sxs-lookup"><span data-stu-id="efddf-585">No implicit boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)) is ever performed for the expression of a `lock` statement, and thus it is a compile-time error for the expression to denote a value of a *value_type*.</span></span>

<span data-ttu-id="efddf-586">窗体的语句`lock`</span><span class="sxs-lookup"><span data-stu-id="efddf-586">A `lock` statement of the form</span></span>
```csharp
lock (x) ...
```
<span data-ttu-id="efddf-587">其中 `x` 是*reference_type*的表达式，它完全等效于</span><span class="sxs-lookup"><span data-stu-id="efddf-587">where `x` is an expression of a *reference_type*, is precisely equivalent to</span></span>
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
<span data-ttu-id="efddf-588">不同的是 `x` 只计算一次。</span><span class="sxs-lookup"><span data-stu-id="efddf-588">except that `x` is only evaluated once.</span></span>

<span data-ttu-id="efddf-589">持有互斥锁时，在同一执行线程中执行的代码也可以获取和释放锁。</span><span class="sxs-lookup"><span data-stu-id="efddf-589">While a mutual-exclusion lock is held, code executing in the same execution thread can also obtain and release the lock.</span></span> <span data-ttu-id="efddf-590">但是，在其他线程中执行的代码被阻止获取锁定，直到锁定被释放。</span><span class="sxs-lookup"><span data-stu-id="efddf-590">However, code executing in other threads is blocked from obtaining the lock until the lock is released.</span></span>

<span data-ttu-id="efddf-591">不`System.Type`建议锁定对象以同步对静态数据的访问。</span><span class="sxs-lookup"><span data-stu-id="efddf-591">Locking `System.Type` objects in order to synchronize access to static data is not recommended.</span></span> <span data-ttu-id="efddf-592">其他代码可能会锁定同一类型，这可能会导致死锁。</span><span class="sxs-lookup"><span data-stu-id="efddf-592">Other code might lock on the same type, which can result in deadlock.</span></span> <span data-ttu-id="efddf-593">更好的方法是通过锁定专用静态对象来同步对静态数据的访问。</span><span class="sxs-lookup"><span data-stu-id="efddf-593">A better approach is to synchronize access to static data by locking a private static object.</span></span> <span data-ttu-id="efddf-594">例如：</span><span class="sxs-lookup"><span data-stu-id="efddf-594">For example:</span></span>
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

## <a name="the-using-statement"></a><span data-ttu-id="efddf-595">using 语句</span><span class="sxs-lookup"><span data-stu-id="efddf-595">The using statement</span></span>

<span data-ttu-id="efddf-596">`using`语句获取一个或多个资源，执行语句，然后释放资源。</span><span class="sxs-lookup"><span data-stu-id="efddf-596">The `using` statement obtains one or more resources, executes a statement, and then disposes of the resource.</span></span>

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

<span data-ttu-id="efddf-597">***资源***是实现`System.IDisposable`的类或结构，其中包括一个名为`Dispose`的无参数方法。</span><span class="sxs-lookup"><span data-stu-id="efddf-597">A ***resource*** is a class or struct that implements `System.IDisposable`, which includes a single parameterless method named `Dispose`.</span></span> <span data-ttu-id="efddf-598">使用资源的代码可以调用`Dispose`来指示不再需要资源。</span><span class="sxs-lookup"><span data-stu-id="efddf-598">Code that is using a resource can call `Dispose` to indicate that the resource is no longer needed.</span></span> <span data-ttu-id="efddf-599">如果`Dispose`未调用，则最终将作为垃圾回收的结果发生。</span><span class="sxs-lookup"><span data-stu-id="efddf-599">If `Dispose` is not called, then automatic disposal eventually occurs as a consequence of garbage collection.</span></span>

<span data-ttu-id="efddf-600">如果*resource_acquisition*的形式为*local_variable_declaration* ，则*local_variable_declaration*的类型必须是 @no__t 3，或者是可以隐式转换为 `System.IDisposable` 的类型。</span><span class="sxs-lookup"><span data-stu-id="efddf-600">If the form of *resource_acquisition* is *local_variable_declaration* then the type of the *local_variable_declaration* must be either `dynamic` or a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="efddf-601">如果*resource_acquisition*的形式为*expression* ，则此表达式必须可隐式转换为 `System.IDisposable`。</span><span class="sxs-lookup"><span data-stu-id="efddf-601">If the form of *resource_acquisition* is *expression* then this expression must be implicitly convertible to `System.IDisposable`.</span></span>

<span data-ttu-id="efddf-602">在*resource_acquisition*中声明的局部变量是只读的，并且必须包含初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="efddf-602">Local variables declared in a *resource_acquisition* are read-only, and must include an initializer.</span></span> <span data-ttu-id="efddf-603">如果嵌入的语句尝试修改这些本地变量（通过赋值`++`或和`--`运算符），则会发生编译时错误，请获取它们的地址，或将它们作为`ref`或`out`参数传递。</span><span class="sxs-lookup"><span data-stu-id="efddf-603">A compile-time error occurs if the embedded statement attempts to modify these local variables (via assignment or the `++` and `--` operators) , take the address of them, or pass them as `ref` or `out` parameters.</span></span>

<span data-ttu-id="efddf-604">`using`语句转换为三个部分：获取、使用和处理。</span><span class="sxs-lookup"><span data-stu-id="efddf-604">A `using` statement is translated into three parts: acquisition, usage, and disposal.</span></span> <span data-ttu-id="efddf-605">资源的使用隐式包含在`try` `finally`包含子句的语句中。</span><span class="sxs-lookup"><span data-stu-id="efddf-605">Usage of the resource is implicitly enclosed in a `try` statement that includes a `finally` clause.</span></span> <span data-ttu-id="efddf-606">此`finally`子句处置资源。</span><span class="sxs-lookup"><span data-stu-id="efddf-606">This `finally` clause disposes of the resource.</span></span> <span data-ttu-id="efddf-607">如果已获取`Dispose` `null`资源，则不会对进行任何调用，也不会引发异常。</span><span class="sxs-lookup"><span data-stu-id="efddf-607">If a `null` resource is acquired, then no call to `Dispose` is made, and no exception is thrown.</span></span> <span data-ttu-id="efddf-608">如果资源的类型`dynamic`为，则它会在获取过程中通过隐式动态转换（[隐式动态](conversions.md#implicit-dynamic-conversions)转换）动态转换为`IDisposable` ，以确保在使用和之前转换成功最后.</span><span class="sxs-lookup"><span data-stu-id="efddf-608">If the resource is of type `dynamic` it is dynamically converted through an implicit dynamic conversion ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)) to `IDisposable` during acquisition in order to ensure that the conversion is successful before the usage and disposal.</span></span>

<span data-ttu-id="efddf-609">窗体的语句`using`</span><span class="sxs-lookup"><span data-stu-id="efddf-609">A `using` statement of the form</span></span>
```csharp
using (ResourceType resource = expression) statement
```
<span data-ttu-id="efddf-610">对应于三个可能的扩展中的一个。</span><span class="sxs-lookup"><span data-stu-id="efddf-610">corresponds to one of three possible expansions.</span></span> <span data-ttu-id="efddf-611">如果`ResourceType`是不可以为 null 的值类型，则扩展为</span><span class="sxs-lookup"><span data-stu-id="efddf-611">When `ResourceType` is a non-nullable value type, the expansion is</span></span>
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

<span data-ttu-id="efddf-612">否则，如果`ResourceType`是可以为 null 的值类型或引用类型`dynamic`（而不是），则扩展为</span><span class="sxs-lookup"><span data-stu-id="efddf-612">Otherwise, when `ResourceType` is a nullable value type or a reference type other than `dynamic`, the expansion is</span></span>
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

<span data-ttu-id="efddf-613">否则，如果`ResourceType`为`dynamic`，则扩展为</span><span class="sxs-lookup"><span data-stu-id="efddf-613">Otherwise, when `ResourceType` is `dynamic`, the expansion is</span></span>
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

<span data-ttu-id="efddf-614">在任一扩展中， `resource`变量在嵌入语句中是只读的， `d`并且该变量在嵌入的语句中不可访问，也不可见。</span><span class="sxs-lookup"><span data-stu-id="efddf-614">In either expansion, the `resource` variable is read-only in the embedded statement, and the `d` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="efddf-615">允许实现以不同方式实现给定的 using 语句（例如出于性能原因），前提是该行为与上述扩展一致。</span><span class="sxs-lookup"><span data-stu-id="efddf-615">An implementation is permitted to implement a given using-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="efddf-616">窗体的语句`using`</span><span class="sxs-lookup"><span data-stu-id="efddf-616">A `using` statement of the form</span></span>
```csharp
using (expression) statement
```
<span data-ttu-id="efddf-617">具有相同的三个可能的扩展。</span><span class="sxs-lookup"><span data-stu-id="efddf-617">has the same three possible expansions.</span></span> <span data-ttu-id="efddf-618">在这种`ResourceType`情况下`expression`，将隐式的编译时类型（如果有）。</span><span class="sxs-lookup"><span data-stu-id="efddf-618">In this case `ResourceType` is implicitly the compile-time type of the `expression`, if it has one.</span></span> <span data-ttu-id="efddf-619">否则，接口`IDisposable`本身将`ResourceType`用作。</span><span class="sxs-lookup"><span data-stu-id="efddf-619">Otherwise the interface `IDisposable` itself is used as the `ResourceType`.</span></span> <span data-ttu-id="efddf-620">此`resource`变量在嵌入的语句中不可访问，也不可见。</span><span class="sxs-lookup"><span data-stu-id="efddf-620">The `resource` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="efddf-621">当*resource_acquisition*采用*local_variable_declaration*的形式时，可以获取给定类型的多个资源。</span><span class="sxs-lookup"><span data-stu-id="efddf-621">When a *resource_acquisition* takes the form of a *local_variable_declaration*, it is possible to acquire multiple resources of a given type.</span></span> <span data-ttu-id="efddf-622">窗体的语句`using`</span><span class="sxs-lookup"><span data-stu-id="efddf-622">A `using` statement of the form</span></span>
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
<span data-ttu-id="efddf-623">完全等效于一系列嵌套`using`语句：</span><span class="sxs-lookup"><span data-stu-id="efddf-623">is precisely equivalent to a sequence of nested `using` statements:</span></span>
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

<span data-ttu-id="efddf-624">下面的示例创建一个名为`log.txt`的文件，并向该文件写入两行文本。</span><span class="sxs-lookup"><span data-stu-id="efddf-624">The example below creates a file named `log.txt` and writes two lines of text to the file.</span></span> <span data-ttu-id="efddf-625">然后，该示例将打开该文件以进行读取，并将包含的文本行复制到控制台。</span><span class="sxs-lookup"><span data-stu-id="efddf-625">The example then opens that same file for reading and copies the contained lines of text to the console.</span></span>
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

<span data-ttu-id="efddf-626">`TextWriter`由于和`TextReader`类`using`实现`IDisposable`接口，因此该示例可以使用语句来确保在写入或读取操作后正确关闭基础文件。</span><span class="sxs-lookup"><span data-stu-id="efddf-626">Since the `TextWriter` and `TextReader` classes implement the `IDisposable` interface, the example can use `using` statements to ensure that the underlying file is properly closed following the write or read operations.</span></span>

## <a name="the-yield-statement"></a><span data-ttu-id="efddf-627">Yield 语句</span><span class="sxs-lookup"><span data-stu-id="efddf-627">The yield statement</span></span>

<span data-ttu-id="efddf-628">在`yield`迭代器块（[块](statements.md#blocks)）中使用该语句来向迭代器的枚举器对象（[枚举器对象](classes.md#enumerator-objects)）或可枚举对象（可[枚举](classes.md#enumerable-objects)对象）生成值或表示迭代结束。</span><span class="sxs-lookup"><span data-stu-id="efddf-628">The `yield` statement is used in an iterator block ([Blocks](statements.md#blocks)) to yield a value to the enumerator object ([Enumerator objects](classes.md#enumerator-objects)) or enumerable object ([Enumerable objects](classes.md#enumerable-objects)) of an iterator or to signal the end of the iteration.</span></span>

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

<span data-ttu-id="efddf-629">`yield`不是保留字;仅当紧靠在`return`或`break`关键字之前使用时，它才具有特殊意义。</span><span class="sxs-lookup"><span data-stu-id="efddf-629">`yield` is not a reserved word; it has special meaning only when used immediately before a `return` or `break` keyword.</span></span> <span data-ttu-id="efddf-630">在其他上下文中`yield` ，可用作标识符。</span><span class="sxs-lookup"><span data-stu-id="efddf-630">In other contexts, `yield` can be used as an identifier.</span></span>

<span data-ttu-id="efddf-631">`yield`语句可以出现的位置有多个限制，如下所述。</span><span class="sxs-lookup"><span data-stu-id="efddf-631">There are several restrictions on where a `yield` statement can appear, as described in the following.</span></span>

*  <span data-ttu-id="efddf-632">对于*method_body*、 *operator_body*或*accessor_body*之外的 @no__t 0 语句，这是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="efddf-632">It is a compile-time error for a `yield` statement (of either form) to appear outside a *method_body*, *operator_body* or *accessor_body*</span></span>
*  <span data-ttu-id="efddf-633">在匿名函数内显示`yield`语句时，它是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="efddf-633">It is a compile-time error for a `yield` statement (of either form) to appear inside an anonymous function.</span></span>
*  <span data-ttu-id="efddf-634">语句的`yield` `finally`子句中将出现编译时错误，语句如下所示。`try`</span><span class="sxs-lookup"><span data-stu-id="efddf-634">It is a compile-time error for a `yield` statement (of either form) to appear in the `finally` clause of a `try` statement.</span></span>
*  <span data-ttu-id="efddf-635">`yield return`语句要出现`try`在包含任何`catch`子句的语句中的任何位置的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="efddf-635">It is a compile-time error for a `yield return` statement to appear anywhere in a `try` statement that contains any `catch` clauses.</span></span>

<span data-ttu-id="efddf-636">下面的示例显示了语句的一些有效和`yield`无效的用法。</span><span class="sxs-lookup"><span data-stu-id="efddf-636">The following example shows some valid and invalid uses of `yield` statements.</span></span>

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

<span data-ttu-id="efddf-637">必须从`yield return`语句中表达式的类型到迭代器的 yield 类型（[yield 类型](classes.md#yield-type)）存在隐式转换（[隐式](conversions.md#implicit-conversions)转换）。</span><span class="sxs-lookup"><span data-stu-id="efddf-637">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression in the `yield return` statement to the yield type ([Yield type](classes.md#yield-type)) of the iterator.</span></span>

<span data-ttu-id="efddf-638">`yield return`语句的执行方式如下：</span><span class="sxs-lookup"><span data-stu-id="efddf-638">A `yield return` statement is executed as follows:</span></span>

*  <span data-ttu-id="efddf-639">计算语句中给定的表达式，将其隐式转换为 yield 类型，并将其分配`Current`给枚举器对象的属性。</span><span class="sxs-lookup"><span data-stu-id="efddf-639">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
*  <span data-ttu-id="efddf-640">迭代器块的执行被挂起。</span><span class="sxs-lookup"><span data-stu-id="efddf-640">Execution of the iterator block is suspended.</span></span> <span data-ttu-id="efddf-641">如果语句在一个或多个`try`块中，则不`finally`会执行关联的块。 `yield return`</span><span class="sxs-lookup"><span data-stu-id="efddf-641">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
*  <span data-ttu-id="efddf-642">枚举`MoveNext`器对象的方法返回`true`到其调用方，指示枚举数对象已成功地前进到下一项。</span><span class="sxs-lookup"><span data-stu-id="efddf-642">The `MoveNext` method of the enumerator object returns `true` to its caller, indicating that the enumerator object successfully advanced to the next item.</span></span>

<span data-ttu-id="efddf-643">对枚举器对象的`MoveNext`方法的下一次调用会从其上次挂起的位置继续执行迭代器块。</span><span class="sxs-lookup"><span data-stu-id="efddf-643">The next call to the enumerator object's `MoveNext` method resumes execution of the iterator block from where it was last suspended.</span></span>

<span data-ttu-id="efddf-644">`yield break`语句的执行方式如下：</span><span class="sxs-lookup"><span data-stu-id="efddf-644">A `yield break` statement is executed as follows:</span></span>

*  <span data-ttu-id="efddf-645">`try` `finally` `finally` `try`如果语句由一个或多个具有关联块的块括起来，则控件最初会传输到最内层语句的块。 `yield break`</span><span class="sxs-lookup"><span data-stu-id="efddf-645">If the `yield break` statement is enclosed by one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="efddf-646">当和如果控件到达`finally`块的终点时，控制将被传输`finally`到下一个封闭`try`语句的块。</span><span class="sxs-lookup"><span data-stu-id="efddf-646">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="efddf-647">此过程将重复进行， `finally`直至所有封闭`try`语句的块都已执行完毕。</span><span class="sxs-lookup"><span data-stu-id="efddf-647">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="efddf-648">控制权将返回给迭代器块的调用方。</span><span class="sxs-lookup"><span data-stu-id="efddf-648">Control is returned to the caller of the iterator block.</span></span> <span data-ttu-id="efddf-649">这是枚举器`MoveNext`对象的`Dispose`方法或方法。</span><span class="sxs-lookup"><span data-stu-id="efddf-649">This is either the `MoveNext` method or `Dispose` method of the enumerator object.</span></span>

<span data-ttu-id="efddf-650">由于语句无条件地将控制转移到其他位置，因此无法`yield break`访问语句的结束点。 `yield break`</span><span class="sxs-lookup"><span data-stu-id="efddf-650">Because a `yield break` statement unconditionally transfers control elsewhere, the end point of a `yield break` statement is never reachable.</span></span>
