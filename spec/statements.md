---
ms.openlocfilehash: 8f9551b9e7f70379836c23a60f0d37dc02f8e18e
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488817"
---
# <a name="statements"></a><span data-ttu-id="82992-101">语句</span><span class="sxs-lookup"><span data-stu-id="82992-101">Statements</span></span>

<span data-ttu-id="82992-102">C# 提供了各种不同的语句。</span><span class="sxs-lookup"><span data-stu-id="82992-102">C# provides a variety of statements.</span></span> <span data-ttu-id="82992-103">将熟悉的开发人员可以在 C 中是否具有编程经验的大部分这些语句和C++。</span><span class="sxs-lookup"><span data-stu-id="82992-103">Most of these statements will be familiar to developers who have programmed in C and C++.</span></span>

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

<span data-ttu-id="82992-104">*Embedded_statement*非终止符出现在其他语句的语句的使用。</span><span class="sxs-lookup"><span data-stu-id="82992-104">The *embedded_statement* nonterminal is used for statements that appear within other statements.</span></span> <span data-ttu-id="82992-105">利用*embedded_statement*而非*语句*不包括的声明语句和标记的语句下面的上下文中使用。</span><span class="sxs-lookup"><span data-stu-id="82992-105">The use of *embedded_statement* rather than *statement* excludes the use of declaration statements and labeled statements in these contexts.</span></span> <span data-ttu-id="82992-106">该示例</span><span class="sxs-lookup"><span data-stu-id="82992-106">The example</span></span>
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
<span data-ttu-id="82992-107">导致编译时错误，因为`if`语句需要*embedded_statement*而非*语句*的 if 分支。</span><span class="sxs-lookup"><span data-stu-id="82992-107">results in a compile-time error because an `if` statement requires an *embedded_statement* rather than a *statement* for its if branch.</span></span> <span data-ttu-id="82992-108">如果允许这种代码，然后在变量`i`将声明，但可能永远不会使用它。</span><span class="sxs-lookup"><span data-stu-id="82992-108">If this code were permitted, then the variable `i` would be declared, but it could never be used.</span></span> <span data-ttu-id="82992-109">但请注意，通过将放置`i`的块中的声明，该示例是有效的。</span><span class="sxs-lookup"><span data-stu-id="82992-109">Note, however, that by placing `i`'s declaration in a block, the example is valid.</span></span>

## <a name="end-points-and-reachability"></a><span data-ttu-id="82992-110">终结点和可访问性</span><span class="sxs-lookup"><span data-stu-id="82992-110">End points and reachability</span></span>

<span data-ttu-id="82992-111">每个语句具有***终结点***。</span><span class="sxs-lookup"><span data-stu-id="82992-111">Every statement has an ***end point***.</span></span> <span data-ttu-id="82992-112">直观地讲，在语句的结束点是紧跟在语句的位置。</span><span class="sxs-lookup"><span data-stu-id="82992-112">In intuitive terms, the end point of a statement is the location that immediately follows the statement.</span></span> <span data-ttu-id="82992-113">复合语句 （包含嵌入的语句的语句） 执行的规则指定当控件到达终结点的嵌入的语句时执行的操作。</span><span class="sxs-lookup"><span data-stu-id="82992-113">The execution rules for composite statements (statements that contain embedded statements) specify the action that is taken when control reaches the end point of an embedded statement.</span></span> <span data-ttu-id="82992-114">例如，当控件到达终结点的块中的语句，控制转到下一个语句块中。</span><span class="sxs-lookup"><span data-stu-id="82992-114">For example, when control reaches the end point of a statement in a block, control is transferred to the next statement in the block.</span></span>

<span data-ttu-id="82992-115">如果一个语句可能是可通过执行访问，该语句称***可访问***。</span><span class="sxs-lookup"><span data-stu-id="82992-115">If a statement can possibly be reached by execution, the statement is said to be ***reachable***.</span></span> <span data-ttu-id="82992-116">相反，如果没有要执行的语句不可能，该语句称***无法访问***。</span><span class="sxs-lookup"><span data-stu-id="82992-116">Conversely, if there is no possibility that a statement will be executed, the statement is said to be ***unreachable***.</span></span>

<span data-ttu-id="82992-117">在示例</span><span class="sxs-lookup"><span data-stu-id="82992-117">In the example</span></span>
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
<span data-ttu-id="82992-118">第二个调用`Console.WriteLine`是无法访问，因为不可能将执行该语句。</span><span class="sxs-lookup"><span data-stu-id="82992-118">the second invocation of `Console.WriteLine` is unreachable because there is no possibility that the statement will be executed.</span></span>

<span data-ttu-id="82992-119">如果编译器确定语句是无法访问，将报告警告。</span><span class="sxs-lookup"><span data-stu-id="82992-119">A warning is reported if the compiler determines that a statement is unreachable.</span></span> <span data-ttu-id="82992-120">具体而言不是错误的语句无法访问它。</span><span class="sxs-lookup"><span data-stu-id="82992-120">It is specifically not an error for a statement to be unreachable.</span></span>

<span data-ttu-id="82992-121">若要确定是否可到达特定语句或终结点，编译器会执行流分析根据为每个语句定义的可访问性规则。</span><span class="sxs-lookup"><span data-stu-id="82992-121">To determine whether a particular statement or end point is reachable, the compiler performs flow analysis according to the reachability rules defined for each statement.</span></span> <span data-ttu-id="82992-122">流分析会考虑常量表达式的值 ([常量表达式](expressions.md#constant-expressions)) 的控制行为的语句，但并不认为非常量表达式的可能值。</span><span class="sxs-lookup"><span data-stu-id="82992-122">The flow analysis takes into account the values of constant expressions ([Constant expressions](expressions.md#constant-expressions)) that control the behavior of statements, but the possible values of non-constant expressions are not considered.</span></span> <span data-ttu-id="82992-123">换而言之，用于控制流分析的目的，给定类型的非常量表达式被视为具有该类型的任何可能的值。</span><span class="sxs-lookup"><span data-stu-id="82992-123">In other words, for purposes of control flow analysis, a non-constant expression of a given type is considered to have any possible value of that type.</span></span>

<span data-ttu-id="82992-124">在示例</span><span class="sxs-lookup"><span data-stu-id="82992-124">In the example</span></span>
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
<span data-ttu-id="82992-125">布尔表达式`if`语句是一个常量表达式，因为这两个操作数的`==`运算符是常量。</span><span class="sxs-lookup"><span data-stu-id="82992-125">the boolean expression of the `if` statement is a constant expression because both operands of the `==` operator are constants.</span></span> <span data-ttu-id="82992-126">常量表达式将在编译时计算，生成值`false`，则`Console.WriteLine`调用被视为无法访问。</span><span class="sxs-lookup"><span data-stu-id="82992-126">As the constant expression is evaluated at compile-time, producing the value `false`, the `Console.WriteLine` invocation is considered unreachable.</span></span> <span data-ttu-id="82992-127">但是，如果`i`更改为本地变量</span><span class="sxs-lookup"><span data-stu-id="82992-127">However, if `i` is changed to be a local variable</span></span>
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
<span data-ttu-id="82992-128">`Console.WriteLine`调用被视为可访问，即使在现实中，它将永远不会执行。</span><span class="sxs-lookup"><span data-stu-id="82992-128">the `Console.WriteLine` invocation is considered reachable, even though, in reality, it will never be executed.</span></span>

<span data-ttu-id="82992-129">*块*函数的成员始终被认为是可访问。</span><span class="sxs-lookup"><span data-stu-id="82992-129">The *block* of a function member is always considered reachable.</span></span> <span data-ttu-id="82992-130">通过依次计算每个语句块中的可访问性规则，可确定任何给定的语句的可访问性。</span><span class="sxs-lookup"><span data-stu-id="82992-130">By successively evaluating the reachability rules of each statement in a block, the reachability of any given statement can be determined.</span></span>

<span data-ttu-id="82992-131">在示例</span><span class="sxs-lookup"><span data-stu-id="82992-131">In the example</span></span>
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
<span data-ttu-id="82992-132">第二个可访问性`Console.WriteLine`，如下所示确定：</span><span class="sxs-lookup"><span data-stu-id="82992-132">the reachability of the second `Console.WriteLine` is determined as follows:</span></span>

*  <span data-ttu-id="82992-133">第一个`Console.WriteLine`表达式是可到达因为的块`F`方法是可访问。</span><span class="sxs-lookup"><span data-stu-id="82992-133">The first `Console.WriteLine` expression statement is reachable because the block of the `F` method is reachable.</span></span>
*  <span data-ttu-id="82992-134">第一个终结点`Console.WriteLine`表达式语句是可访问，因为该语句是可访问。</span><span class="sxs-lookup"><span data-stu-id="82992-134">The end point of the first `Console.WriteLine` expression statement is reachable because that statement is reachable.</span></span>
*  <span data-ttu-id="82992-135">`if`语句是可访问，因为终结点的第一个`Console.WriteLine`表达式语句是可访问。</span><span class="sxs-lookup"><span data-stu-id="82992-135">The `if` statement is reachable because the end point of the first `Console.WriteLine` expression statement is reachable.</span></span>
*  <span data-ttu-id="82992-136">第二个`Console.WriteLine`表达式是可到达因为的布尔表达式`if`语句不具有常量值`false`。</span><span class="sxs-lookup"><span data-stu-id="82992-136">The second `Console.WriteLine` expression statement is reachable because the boolean expression of the `if` statement does not have the constant value `false`.</span></span>

<span data-ttu-id="82992-137">有两种情况下，在其中它是语句可到达的终结点的编译时错误：</span><span class="sxs-lookup"><span data-stu-id="82992-137">There are two situations in which it is a compile-time error for the end point of a statement to be reachable:</span></span>

*  <span data-ttu-id="82992-138">因为`switch`语句不允许在下一步的开关部分"贯穿"的开关部分，它是开关部分可到达语句列表的终结点的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="82992-138">Because the `switch` statement does not permit a switch section to "fall through" to the next switch section, it is a compile-time error for the end point of the statement list of a switch section to be reachable.</span></span> <span data-ttu-id="82992-139">如果发生此错误，它通常是一个指示，指明的`break`语句缺少。</span><span class="sxs-lookup"><span data-stu-id="82992-139">If this error occurs, it is typically an indication that a `break` statement is missing.</span></span>
*  <span data-ttu-id="82992-140">它是终结点的块的计算值为可访问的函数成员的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="82992-140">It is a compile-time error for the end point of the block of a function member that computes a value to be reachable.</span></span> <span data-ttu-id="82992-141">如果发生此错误，它通常是一个指示，指明的`return`语句缺少。</span><span class="sxs-lookup"><span data-stu-id="82992-141">If this error occurs, it typically is an indication that a `return` statement is missing.</span></span>

## <a name="blocks"></a><span data-ttu-id="82992-142">Blocks</span><span class="sxs-lookup"><span data-stu-id="82992-142">Blocks</span></span>

<span data-ttu-id="82992-143">使用*代码块*，可以在允许编写一个语句的上下文中编写多个语句。</span><span class="sxs-lookup"><span data-stu-id="82992-143">A *block* permits multiple statements to be written in contexts where a single statement is allowed.</span></span>

```antlr
block
    : '{' statement_list? '}'
    ;
```

<span data-ttu-id="82992-144">一个*块*组成的可选*statement_list* ([语句列出了](statements.md#statement-lists))、 大括号内。</span><span class="sxs-lookup"><span data-stu-id="82992-144">A *block* consists of an optional *statement_list* ([Statement lists](statements.md#statement-lists)), enclosed in braces.</span></span> <span data-ttu-id="82992-145">如果省略语句列表，则称块是为空。</span><span class="sxs-lookup"><span data-stu-id="82992-145">If the statement list is omitted, the block is said to be empty.</span></span>

<span data-ttu-id="82992-146">块可以包含声明语句 ([声明语句](statements.md#declaration-statements))。</span><span class="sxs-lookup"><span data-stu-id="82992-146">A block may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="82992-147">本地变量或常量的作用域在块中声明是块。</span><span class="sxs-lookup"><span data-stu-id="82992-147">The scope of a local variable or constant declared in a block is the block.</span></span>

<span data-ttu-id="82992-148">块的执行方式，如下所示：</span><span class="sxs-lookup"><span data-stu-id="82992-148">A block is executed as follows:</span></span>

*  <span data-ttu-id="82992-149">如果块为空，控制传输到块的结束点。</span><span class="sxs-lookup"><span data-stu-id="82992-149">If the block is empty, control is transferred to the end point of the block.</span></span>
*  <span data-ttu-id="82992-150">如果块不为空，控制转到语句列表。</span><span class="sxs-lookup"><span data-stu-id="82992-150">If the block is not empty, control is transferred to the statement list.</span></span> <span data-ttu-id="82992-151">当且控制到达语句列表的终结点，则将控制转移到其中的块的终结点。</span><span class="sxs-lookup"><span data-stu-id="82992-151">When and if control reaches the end point of the statement list, control is transferred to the end point of the block.</span></span>

<span data-ttu-id="82992-152">如果块本身是可访问的块的语句列表。</span><span class="sxs-lookup"><span data-stu-id="82992-152">The statement list of a block is reachable if the block itself is reachable.</span></span>

<span data-ttu-id="82992-153">如果块为空或语句列表的结束点是可访问的块的终结点访问。</span><span class="sxs-lookup"><span data-stu-id="82992-153">The end point of a block is reachable if the block is empty or if the end point of the statement list is reachable.</span></span>

<span data-ttu-id="82992-154">一个*块*，其中包含一个或多个`yield`语句 ([yield 语句](statements.md#the-yield-statement)) 调用迭代器块。</span><span class="sxs-lookup"><span data-stu-id="82992-154">A *block* that contains one or more `yield` statements ([The yield statement](statements.md#the-yield-statement)) is called an iterator block.</span></span> <span data-ttu-id="82992-155">使用迭代器块实现迭代器作为函数成员 ([迭代器](classes.md#iterators))。</span><span class="sxs-lookup"><span data-stu-id="82992-155">Iterator blocks are used to implement function members as iterators ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="82992-156">某些其他限制适用于迭代器块：</span><span class="sxs-lookup"><span data-stu-id="82992-156">Some additional restrictions apply to iterator blocks:</span></span>

*  <span data-ttu-id="82992-157">它是编译时错误`return`语句出现在迭代器块中 (但`yield return`允许语句)。</span><span class="sxs-lookup"><span data-stu-id="82992-157">It is a compile-time error for a `return` statement to appear in an iterator block (but `yield return` statements are permitted).</span></span>
*  <span data-ttu-id="82992-158">它是一个迭代器块，以包含不安全的上下文的编译时错误 ([不安全的上下文](unsafe-code.md#unsafe-contexts))。</span><span class="sxs-lookup"><span data-stu-id="82992-158">It is a compile-time error for an iterator block to contain an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="82992-159">迭代器块始终定义的安全上下文，即使其声明嵌套在不安全的上下文中。</span><span class="sxs-lookup"><span data-stu-id="82992-159">An iterator block always defines a safe context, even when its declaration is nested in an unsafe context.</span></span>

### <a name="statement-lists"></a><span data-ttu-id="82992-160">语句列表</span><span class="sxs-lookup"><span data-stu-id="82992-160">Statement lists</span></span>

<span data-ttu-id="82992-161">一个***语句列表***包含一个或多个序列中编写的语句。</span><span class="sxs-lookup"><span data-stu-id="82992-161">A ***statement list*** consists of one or more statements written in sequence.</span></span> <span data-ttu-id="82992-162">语句列表中出现*块*s ([块](statements.md#blocks)) 并在*switch_block*s ([switch 语句](statements.md#the-switch-statement))。</span><span class="sxs-lookup"><span data-stu-id="82992-162">Statement lists occur in *block*s ([Blocks](statements.md#blocks)) and in *switch_block*s ([The switch statement](statements.md#the-switch-statement)).</span></span>

```antlr
statement_list
    : statement+
    ;
```

<span data-ttu-id="82992-163">通过将控制转移到第一个语句执行语句列表。</span><span class="sxs-lookup"><span data-stu-id="82992-163">A statement list is executed by transferring control to the first statement.</span></span> <span data-ttu-id="82992-164">当和控制到达终结点的语句，控制传输到下一个语句。</span><span class="sxs-lookup"><span data-stu-id="82992-164">When and if control reaches the end point of a statement, control is transferred to the next statement.</span></span> <span data-ttu-id="82992-165">当和控制到达最后一个语句的结束点，则将控制转移到其中的语句列表的终结点。</span><span class="sxs-lookup"><span data-stu-id="82992-165">When and if control reaches the end point of the last statement, control is transferred to the end point of the statement list.</span></span>

<span data-ttu-id="82992-166">如果至少一个以下为 true，则语句列表中的语句不可用：</span><span class="sxs-lookup"><span data-stu-id="82992-166">A statement in a statement list is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="82992-167">该语句是第一条语句和语句列表本身是可访问。</span><span class="sxs-lookup"><span data-stu-id="82992-167">The statement is the first statement and the statement list itself is reachable.</span></span>
*  <span data-ttu-id="82992-168">访问终结点的前面的语句。</span><span class="sxs-lookup"><span data-stu-id="82992-168">The end point of the preceding statement is reachable.</span></span>
*  <span data-ttu-id="82992-169">该语句是带标签的语句和标签引用的可访问`goto`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-169">The statement is a labeled statement and the label is referenced by a reachable `goto` statement.</span></span>

<span data-ttu-id="82992-170">如果在列表中的最后一个语句的结束点是可访问终结点语句列表的访问。</span><span class="sxs-lookup"><span data-stu-id="82992-170">The end point of a statement list is reachable if the end point of the last statement in the list is reachable.</span></span>

## <a name="the-empty-statement"></a><span data-ttu-id="82992-171">空语句</span><span class="sxs-lookup"><span data-stu-id="82992-171">The empty statement</span></span>

<span data-ttu-id="82992-172">*Empty_statement*不执行任何操作。</span><span class="sxs-lookup"><span data-stu-id="82992-172">An *empty_statement* does nothing.</span></span>

```antlr
empty_statement
    : ';'
    ;
```

<span data-ttu-id="82992-173">时没有任何操作的上下文中执行语句的所需使用一个空语句。</span><span class="sxs-lookup"><span data-stu-id="82992-173">An empty statement is used when there are no operations to perform in a context where a statement is required.</span></span>

<span data-ttu-id="82992-174">执行一个空语句只需将控制转移到该语句的结束点。</span><span class="sxs-lookup"><span data-stu-id="82992-174">Execution of an empty statement simply transfers control to the end point of the statement.</span></span> <span data-ttu-id="82992-175">因此，一个空语句终结点是空语句是可访问的情况下可访问。</span><span class="sxs-lookup"><span data-stu-id="82992-175">Thus, the end point of an empty statement is reachable if the empty statement is reachable.</span></span>

<span data-ttu-id="82992-176">写入时，可以使用一个空语句`while`语句体为空：</span><span class="sxs-lookup"><span data-stu-id="82992-176">An empty statement can be used when writing a `while` statement with a null body:</span></span>
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

<span data-ttu-id="82992-177">此外，一个空语句可用于声明只是在关闭前的一个标签"`}`"块的：</span><span class="sxs-lookup"><span data-stu-id="82992-177">Also, an empty statement can be used to declare a label just before the closing "`}`" of a block:</span></span>
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a><span data-ttu-id="82992-178">带标签的语句</span><span class="sxs-lookup"><span data-stu-id="82992-178">Labeled statements</span></span>

<span data-ttu-id="82992-179">一个*labeled_statement*允许一个语句，以标签作为前缀。</span><span class="sxs-lookup"><span data-stu-id="82992-179">A *labeled_statement* permits a statement to be prefixed by a label.</span></span> <span data-ttu-id="82992-180">标记的语句出现在块中，但不是能作为嵌入的语句。</span><span class="sxs-lookup"><span data-stu-id="82992-180">Labeled statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

<span data-ttu-id="82992-181">使用给定的名称的标记的语句声明了一个标签*标识符*。</span><span class="sxs-lookup"><span data-stu-id="82992-181">A labeled statement declares a label with the name given by the *identifier*.</span></span> <span data-ttu-id="82992-182">标签的范围是整个块在其中声明了标签，包括任何嵌套块。</span><span class="sxs-lookup"><span data-stu-id="82992-182">The scope of a label is the whole block in which the label is declared, including any nested blocks.</span></span> <span data-ttu-id="82992-183">它是具有相同的名称，具有重叠范围的两个标签的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="82992-183">It is a compile-time error for two labels with the same name to have overlapping scopes.</span></span>

<span data-ttu-id="82992-184">可以从引用的标签`goto`语句 ([goto 语句](statements.md#the-goto-statement)) 标签的范围内。</span><span class="sxs-lookup"><span data-stu-id="82992-184">A label can be referenced from `goto` statements ([The goto statement](statements.md#the-goto-statement)) within the scope of the label.</span></span> <span data-ttu-id="82992-185">这意味着，`goto`语句可将控制块内和外部，但永远不会分解成块传输。</span><span class="sxs-lookup"><span data-stu-id="82992-185">This means that `goto` statements can transfer control within blocks and out of blocks, but never into blocks.</span></span>

<span data-ttu-id="82992-186">标签具有其自己的声明空间并不会干扰其他标识符。</span><span class="sxs-lookup"><span data-stu-id="82992-186">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="82992-187">该示例</span><span class="sxs-lookup"><span data-stu-id="82992-187">The example</span></span>
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
<span data-ttu-id="82992-188">有效，并使用名称`x`用作参数和一个标签。</span><span class="sxs-lookup"><span data-stu-id="82992-188">is valid and uses the name `x` as both a parameter and a label.</span></span>

<span data-ttu-id="82992-189">标记语句的执行完全对应于该标签后的语句的执行。</span><span class="sxs-lookup"><span data-stu-id="82992-189">Execution of a labeled statement corresponds exactly to execution of the statement following the label.</span></span>

<span data-ttu-id="82992-190">除了由正常控制流提供的可访问性，带标签的语句是标签引用的可访问的情况下可访问`goto`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-190">In addition to the reachability provided by normal flow of control, a labeled statement is reachable if the label is referenced by a reachable `goto` statement.</span></span> <span data-ttu-id="82992-191">(异常：如果`goto`语句位于`try`，其中包含`finally`块中和标记的语句是外部`try`，和的终点`finally`块是无法访问，则标记的语句不是可从`goto`语句。)</span><span class="sxs-lookup"><span data-stu-id="82992-191">(Exception: If a `goto` statement is inside a `try` that includes a `finally` block, and the labeled statement is outside the `try`, and the end point of the `finally` block is unreachable, then the labeled statement is not reachable from that `goto` statement.)</span></span>

## <a name="declaration-statements"></a><span data-ttu-id="82992-192">声明语句</span><span class="sxs-lookup"><span data-stu-id="82992-192">Declaration statements</span></span>

<span data-ttu-id="82992-193">一个*declaration_statement*声明本地变量或常量。</span><span class="sxs-lookup"><span data-stu-id="82992-193">A *declaration_statement* declares a local variable or constant.</span></span> <span data-ttu-id="82992-194">声明语句出现在块中，但不是能作为嵌入的语句。</span><span class="sxs-lookup"><span data-stu-id="82992-194">Declaration statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a><span data-ttu-id="82992-195">本地变量声明</span><span class="sxs-lookup"><span data-stu-id="82992-195">Local variable declarations</span></span>

<span data-ttu-id="82992-196">一个*local_variable_declaration*声明一个或多个本地变量。</span><span class="sxs-lookup"><span data-stu-id="82992-196">A *local_variable_declaration* declares one or more local variables.</span></span>

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

<span data-ttu-id="82992-197">*Local_variable_type*的*local_variable_declaration*直接指定该声明引入的变量的类型或具有标识符指示`var`，应根据初始值设定项推断的类型。</span><span class="sxs-lookup"><span data-stu-id="82992-197">The *local_variable_type* of a *local_variable_declaration* either directly specifies the type of the variables introduced by the declaration, or indicates with the identifier `var` that the type should be inferred based on an initializer.</span></span> <span data-ttu-id="82992-198">键入后跟一系列*local_variable_declarator*s，其中每个引入了新的变量。</span><span class="sxs-lookup"><span data-stu-id="82992-198">The type is followed by a list of *local_variable_declarator*s, each of which introduces a new variable.</span></span> <span data-ttu-id="82992-199">一个*local_variable_declarator*组成*标识符*命名变量，可以选择后跟"`=`"令牌和一个*local_variable_initializer* ，它给出的变量的初始值。</span><span class="sxs-lookup"><span data-stu-id="82992-199">A *local_variable_declarator* consists of an *identifier* that names the variable, optionally followed by an "`=`" token and a *local_variable_initializer* that gives the initial value of the variable.</span></span>

<span data-ttu-id="82992-200">在本地变量声明的上下文中，标识符 var 充当上下文关键字 ([关键字](lexical-structure.md#keywords))。当*local_variable_type*指定为`var`和名为任何类型`var`是在范围内，声明是***隐式类型化局部变量声明***，其类型是从关联的初始值设定项表达式的类型推断。</span><span class="sxs-lookup"><span data-stu-id="82992-200">In the context of a local variable declaration, the identifier var acts as a contextual keyword ([Keywords](lexical-structure.md#keywords)).When the *local_variable_type* is specified as `var` and no type named `var` is in scope, the declaration is an ***implicitly typed local variable declaration***, whose type is inferred from the type of the associated initializer expression.</span></span> <span data-ttu-id="82992-201">隐式类型化局部变量声明受到以下限制：</span><span class="sxs-lookup"><span data-stu-id="82992-201">Implicitly typed local variable declarations are subject to the following restrictions:</span></span>

*  <span data-ttu-id="82992-202">*Local_variable_declaration*不能包含多个*local_variable_declarator*s。</span><span class="sxs-lookup"><span data-stu-id="82992-202">The *local_variable_declaration* cannot include multiple *local_variable_declarator*s.</span></span>
*  <span data-ttu-id="82992-203">*Local_variable_declarator*必须包括*local_variable_initializer*。</span><span class="sxs-lookup"><span data-stu-id="82992-203">The *local_variable_declarator* must include a *local_variable_initializer*.</span></span>
*  <span data-ttu-id="82992-204">*Local_variable_initializer*必须是*表达式*。</span><span class="sxs-lookup"><span data-stu-id="82992-204">The *local_variable_initializer* must be an *expression*.</span></span>
*  <span data-ttu-id="82992-205">初始值设定项*表达式*必须编译时类型。</span><span class="sxs-lookup"><span data-stu-id="82992-205">The initializer *expression* must have a compile-time type.</span></span>
*  <span data-ttu-id="82992-206">初始值设定项*表达式*声明的变量本身不能引用</span><span class="sxs-lookup"><span data-stu-id="82992-206">The initializer *expression* cannot refer to the declared variable itself</span></span>

<span data-ttu-id="82992-207">不正确的隐式类型本地变量声明的示例如下：</span><span class="sxs-lookup"><span data-stu-id="82992-207">The following are examples of incorrect implicitly typed local variable declarations:</span></span>

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

<span data-ttu-id="82992-208">在中使用表达式获取本地变量的值*simple_name* ([简单名称](expressions.md#simple-names))，并使用本地变量的值进行修改*分配*([赋值运算符](expressions.md#assignment-operators))。</span><span class="sxs-lookup"><span data-stu-id="82992-208">The value of a local variable is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)), and the value of a local variable is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="82992-209">本地变量必须明确赋值 ([明确赋值](variables.md#definite-assignment)) 在每个位置获取其值。</span><span class="sxs-lookup"><span data-stu-id="82992-209">A local variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at each location where its value is obtained.</span></span>

<span data-ttu-id="82992-210">在中声明本地变量的作用域*local_variable_declaration*是块中进行声明。</span><span class="sxs-lookup"><span data-stu-id="82992-210">The scope of a local variable declared in a *local_variable_declaration* is the block in which the declaration occurs.</span></span> <span data-ttu-id="82992-211">它是错误之前的文本位置中的本地变量是指*local_variable_declarator*的本地变量。</span><span class="sxs-lookup"><span data-stu-id="82992-211">It is an error to refer to a local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="82992-212">在本地变量的范围内，它是编译时错误，若要声明另一个本地变量或常量具有相同的名称。</span><span class="sxs-lookup"><span data-stu-id="82992-212">Within the scope of a local variable, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="82992-213">声明多个变量的本地变量声明是等效于具有相同类型的单个变量的多个声明。</span><span class="sxs-lookup"><span data-stu-id="82992-213">A local variable declaration that declares multiple variables is equivalent to multiple declarations of single variables with the same type.</span></span> <span data-ttu-id="82992-214">此外，在本地变量声明中的变量初始值设定项与赋值语句声明后立即插入完全对应。</span><span class="sxs-lookup"><span data-stu-id="82992-214">Furthermore, a variable initializer in a local variable declaration corresponds exactly to an assignment statement that is inserted immediately after the declaration.</span></span>

<span data-ttu-id="82992-215">该示例</span><span class="sxs-lookup"><span data-stu-id="82992-215">The example</span></span>
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
<span data-ttu-id="82992-216">设置完全对应</span><span class="sxs-lookup"><span data-stu-id="82992-216">corresponds exactly to</span></span>
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

<span data-ttu-id="82992-217">在隐式类型本地变量声明中，声明的本地变量的类型执行要用来初始化该变量的表达式的类型相同。</span><span class="sxs-lookup"><span data-stu-id="82992-217">In an implicitly typed local variable declaration, the type of the local variable being declared is taken to be the same as the type of the expression used to initialize the variable.</span></span> <span data-ttu-id="82992-218">例如：</span><span class="sxs-lookup"><span data-stu-id="82992-218">For example:</span></span>
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

<span data-ttu-id="82992-219">隐式类型化局部变量声明上面是完全等效于以下显式类型声明：</span><span class="sxs-lookup"><span data-stu-id="82992-219">The implicitly typed local variable declarations above are precisely equivalent to the following explicitly typed declarations:</span></span>
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a><span data-ttu-id="82992-220">局部常量声明</span><span class="sxs-lookup"><span data-stu-id="82992-220">Local constant declarations</span></span>

<span data-ttu-id="82992-221">一个*local_constant_declaration*声明一个或多个本地常量。</span><span class="sxs-lookup"><span data-stu-id="82992-221">A *local_constant_declaration* declares one or more local constants.</span></span>

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

<span data-ttu-id="82992-222">*类型*的*local_constant_declaration*指定该声明引入的常量的类型。</span><span class="sxs-lookup"><span data-stu-id="82992-222">The *type* of a *local_constant_declaration* specifies the type of the constants introduced by the declaration.</span></span> <span data-ttu-id="82992-223">键入后跟一系列*constant_declarator*s，其中每个引入了新的常量。</span><span class="sxs-lookup"><span data-stu-id="82992-223">The type is followed by a list of *constant_declarator*s, each of which introduces a new constant.</span></span> <span data-ttu-id="82992-224">一个*constant_declarator*组成*标识符*该名称的常量后, 跟"`=`"令牌后, 跟*constant_expression* ([常量表达式](expressions.md#constant-expressions)) 提供的常量的值。</span><span class="sxs-lookup"><span data-stu-id="82992-224">A *constant_declarator* consists of an *identifier* that names the constant, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the constant.</span></span>

<span data-ttu-id="82992-225">*类型*并*constant_expression*的局部常量声明必须遵循相同的规则与常量成员声明 ([常量](classes.md#constants))。</span><span class="sxs-lookup"><span data-stu-id="82992-225">The *type* and *constant_expression* of a local constant declaration must follow the same rules as those of a constant member declaration ([Constants](classes.md#constants)).</span></span>

<span data-ttu-id="82992-226">在表达式中使用获取的值的局部常量*simple_name* ([简单名称](expressions.md#simple-names))。</span><span class="sxs-lookup"><span data-stu-id="82992-226">The value of a local constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="82992-227">局部常量的作用域是在块中进行声明。</span><span class="sxs-lookup"><span data-stu-id="82992-227">The scope of a local constant is the block in which the declaration occurs.</span></span> <span data-ttu-id="82992-228">它是错误的文本的位置之前的局部常量是指其*constant_declarator*。</span><span class="sxs-lookup"><span data-stu-id="82992-228">It is an error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span> <span data-ttu-id="82992-229">范围内的局部常量，它是变量或常量具有相同名称声明另一个本地导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="82992-229">Within the scope of a local constant, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="82992-230">声明多个常数在局部常量声明是等效于单个常量的具有相同类型的多个声明。</span><span class="sxs-lookup"><span data-stu-id="82992-230">A local constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same type.</span></span>

## <a name="expression-statements"></a><span data-ttu-id="82992-231">表达式语句</span><span class="sxs-lookup"><span data-stu-id="82992-231">Expression statements</span></span>

<span data-ttu-id="82992-232">*Expression_statement*计算给定的表达式的值。</span><span class="sxs-lookup"><span data-stu-id="82992-232">An *expression_statement* evaluates a given expression.</span></span> <span data-ttu-id="82992-233">值计算的表达式，如果有，被丢弃。</span><span class="sxs-lookup"><span data-stu-id="82992-233">The value computed by the expression, if any, is discarded.</span></span>

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

<span data-ttu-id="82992-234">并非所有表达式都允许作为语句。</span><span class="sxs-lookup"><span data-stu-id="82992-234">Not all expressions are permitted as statements.</span></span> <span data-ttu-id="82992-235">特别地，表达式中`x + y`和`x == 1`的只是计算一个值，（该值将被放弃），不能作为语句。</span><span class="sxs-lookup"><span data-stu-id="82992-235">In particular, expressions such as `x + y` and `x == 1` that merely compute a value (which will be discarded), are not permitted as statements.</span></span>

<span data-ttu-id="82992-236">执行*expression_statement*包含的表达式的计算结果，然后将控制转移到的终结点*expression_statement*。</span><span class="sxs-lookup"><span data-stu-id="82992-236">Execution of an *expression_statement* evaluates the contained expression and then transfers control to the end point of the *expression_statement*.</span></span> <span data-ttu-id="82992-237">终结点*expression_statement*可访问，如果该*expression_statement*可访问。</span><span class="sxs-lookup"><span data-stu-id="82992-237">The end point of an *expression_statement* is reachable if that *expression_statement* is reachable.</span></span>

## <a name="selection-statements"></a><span data-ttu-id="82992-238">选择语句</span><span class="sxs-lookup"><span data-stu-id="82992-238">Selection statements</span></span>

<span data-ttu-id="82992-239">选择语句选择其中一个的可能根据一些表达式的值执行的语句数。</span><span class="sxs-lookup"><span data-stu-id="82992-239">Selection statements select one of a number of possible statements for execution based on the value of some expression.</span></span>

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a><span data-ttu-id="82992-240">If 语句</span><span class="sxs-lookup"><span data-stu-id="82992-240">The if statement</span></span>

<span data-ttu-id="82992-241">`if`语句选择语句用于执行基于布尔表达式的值。</span><span class="sxs-lookup"><span data-stu-id="82992-241">The `if` statement selects a statement for execution based on the value of a boolean expression.</span></span>

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="82992-242">`else`部分是与前面从词法上最近`if`的允许的语法。</span><span class="sxs-lookup"><span data-stu-id="82992-242">An `else` part is associated with the lexically nearest preceding `if` that is allowed by the syntax.</span></span> <span data-ttu-id="82992-243">因此，`if`窗体的语句</span><span class="sxs-lookup"><span data-stu-id="82992-243">Thus, an `if` statement of the form</span></span>
```csharp
if (x) if (y) F(); else G();
```
<span data-ttu-id="82992-244">等效于</span><span class="sxs-lookup"><span data-stu-id="82992-244">is equivalent to</span></span>
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

<span data-ttu-id="82992-245">`if`执行语句，如下所示：</span><span class="sxs-lookup"><span data-stu-id="82992-245">An `if` statement is executed as follows:</span></span>

*  <span data-ttu-id="82992-246">*Boolean_expression* ([布尔表达式](expressions.md#boolean-expressions)) 进行计算。</span><span class="sxs-lookup"><span data-stu-id="82992-246">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="82992-247">如果布尔表达式生成`true`，控制权转交给第一条嵌入式语句。</span><span class="sxs-lookup"><span data-stu-id="82992-247">If the boolean expression yields `true`, control is transferred to the first embedded statement.</span></span> <span data-ttu-id="82992-248">当且控制到达该语句的结束点，将控制转移到其中的终结点`if`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-248">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="82992-249">如果布尔表达式会生成`false`; 如果`else`一部分存在，控制权转交给第二个嵌入的语句。</span><span class="sxs-lookup"><span data-stu-id="82992-249">If the boolean expression yields `false` and if an `else` part is present, control is transferred to the second embedded statement.</span></span> <span data-ttu-id="82992-250">当且控制到达该语句的结束点，将控制转移到其中的终结点`if`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-250">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="82992-251">如果布尔表达式会生成`false`; 如果`else`一部分不存在，控制转移到其中的终结点`if`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-251">If the boolean expression yields `false` and if an `else` part is not present, control is transferred to the end point of the `if` statement.</span></span>

<span data-ttu-id="82992-252">第一个嵌入的语句`if`语句是可访问如果`if`语句可访问且布尔表达式不具有常量值`false`。</span><span class="sxs-lookup"><span data-stu-id="82992-252">The first embedded statement of an `if` statement is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="82992-253">第二个嵌入的语句`if`语句中，如果存在，是可访问如果`if`语句可访问且布尔表达式不具有常量值`true`。</span><span class="sxs-lookup"><span data-stu-id="82992-253">The second embedded statement of an `if` statement, if present, is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

<span data-ttu-id="82992-254">终结点的`if`语句是至少一个嵌入的语句的结束点是可访问的情况下可访问。</span><span class="sxs-lookup"><span data-stu-id="82992-254">The end point of an `if` statement is reachable if the end point of at least one of its embedded statements is reachable.</span></span> <span data-ttu-id="82992-255">此外，终结点的`if`没有语句`else`部分是可访问如果`if`语句是可访问和布尔表达式不具有常量值`true`。</span><span class="sxs-lookup"><span data-stu-id="82992-255">In addition, the end point of an `if` statement with no `else` part is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-switch-statement"></a><span data-ttu-id="82992-256">Switch 语句</span><span class="sxs-lookup"><span data-stu-id="82992-256">The switch statement</span></span>

<span data-ttu-id="82992-257">Switch 语句执行具有与 switch 表达式的值相对应的关联的交换机标签的语句列表中选择。</span><span class="sxs-lookup"><span data-stu-id="82992-257">The switch statement selects for execution a statement list having an associated switch label that corresponds to the value of the switch expression.</span></span>

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

<span data-ttu-id="82992-258">一个*switch_statement*包含关键字`switch`后, 跟带括号的表达式 （称为 switch 表达式） 后, 跟*switch_block*。</span><span class="sxs-lookup"><span data-stu-id="82992-258">A *switch_statement* consists of the keyword `switch`, followed by a parenthesized expression (called the switch expression), followed by a *switch_block*.</span></span> <span data-ttu-id="82992-259">*Switch_block*由零个或多个组成*switch_section*s，括在大括号。</span><span class="sxs-lookup"><span data-stu-id="82992-259">The *switch_block* consists of zero or more *switch_section*s, enclosed in braces.</span></span> <span data-ttu-id="82992-260">每个*switch_section*包含一个或多个*switch_label*s 跟*statement_list* ([语句列出了](statements.md#statement-lists))。</span><span class="sxs-lookup"><span data-stu-id="82992-260">Each *switch_section* consists of one or more *switch_label*s followed by a *statement_list* ([Statement lists](statements.md#statement-lists)).</span></span>

<span data-ttu-id="82992-261">***用于控制类型***的`switch`语句建立的 switch 表达式。</span><span class="sxs-lookup"><span data-stu-id="82992-261">The ***governing type*** of a `switch` statement is established by the switch expression.</span></span>

*  <span data-ttu-id="82992-262">Switch 表达式的类型是否`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `bool`， `char`， `string`，或*enum_type*，或如果它是可以为 null 的类型对应于其中一种类型，则这是用于管理类型的`switch`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-262">If the type of the switch expression is `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, or an *enum_type*, or if it is the nullable type corresponding to one of these types, then that is the governing type of the `switch` statement.</span></span>
*  <span data-ttu-id="82992-263">否则，只有一个用户定义的隐式转换 ([用户定义的转换](conversions.md#user-defined-conversions)) 必须存在从 switch 表达式的类型为以下可能用于控制类型之一： `sbyte`， `byte`， `short``ushort`， `int`， `uint`， `long`， `ulong`， `char`， `string`，或为 null 的类型对应于这些类型的一个。</span><span class="sxs-lookup"><span data-stu-id="82992-263">Otherwise, exactly one user-defined implicit conversion ([User-defined conversions](conversions.md#user-defined-conversions)) must exist from the type of the switch expression to one of the following possible governing types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, or,  a nullable type corresponding to one of those types.</span></span>
*  <span data-ttu-id="82992-264">否则，如果不存在此类隐式转换，或者如果有多个存在一个此类隐式转换，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="82992-264">Otherwise, if no such implicit conversion exists, or if more than one such implicit conversion exists, a compile-time error occurs.</span></span>

<span data-ttu-id="82992-265">每个常量表达式`case`标签必须表示为隐式转换的值 ([隐式转换](conversions.md#implicit-conversions)) 到用于控制类型的`switch`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-265">The constant expression of each `case` label must denote a value that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the `switch` statement.</span></span> <span data-ttu-id="82992-266">如果两个或多个会发生编译时错误`case`中相同的标签`switch`语句指定相同的常量值。</span><span class="sxs-lookup"><span data-stu-id="82992-266">A compile-time error occurs if two or more `case` labels in the same `switch` statement specify the same constant value.</span></span>

<span data-ttu-id="82992-267">可以是最多有`default`switch 语句中的标签。</span><span class="sxs-lookup"><span data-stu-id="82992-267">There can be at most one `default` label in a switch statement.</span></span>

<span data-ttu-id="82992-268">一个`switch`执行语句，如下所示：</span><span class="sxs-lookup"><span data-stu-id="82992-268">A `switch` statement is executed as follows:</span></span>

*  <span data-ttu-id="82992-269">Switch 表达式计算并将其转换为用于控制类型。</span><span class="sxs-lookup"><span data-stu-id="82992-269">The switch expression is evaluated and converted to the governing type.</span></span>
*  <span data-ttu-id="82992-270">如果在指定某一常量`case`中相同的标签`switch`语句是 switch 表达式的值相等，控制权转至以下匹配的语句列表`case`标签。</span><span class="sxs-lookup"><span data-stu-id="82992-270">If one of the constants specified in a `case` label in the same `switch` statement is equal to the value of the switch expression, control is transferred to the statement list following the matched `case` label.</span></span>
*  <span data-ttu-id="82992-271">如果没有在指定的常量，则`case`中相同的标签`switch`语句等同于 switch 表达式的值; 如果`default`标签不存在，控制传输到后面的语句列表`default`标签。</span><span class="sxs-lookup"><span data-stu-id="82992-271">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if a `default` label is present, control is transferred to the statement list following the `default` label.</span></span>
*  <span data-ttu-id="82992-272">如果没有在指定的常量，则`case`中相同的标签`switch`语句是 switch 表达式的值相等，如果没有`default`标签不存在、 控制转移到其中的终结点`switch`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-272">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if no `default` label is present, control is transferred to the end point of the `switch` statement.</span></span>

<span data-ttu-id="82992-273">如果可访问的开关部分语句列表的终结点，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="82992-273">If the end point of the statement list of a switch section is reachable, a compile-time error occurs.</span></span> <span data-ttu-id="82992-274">这称为"无贯穿"规则。</span><span class="sxs-lookup"><span data-stu-id="82992-274">This is known as the "no fall through" rule.</span></span> <span data-ttu-id="82992-275">该示例</span><span class="sxs-lookup"><span data-stu-id="82992-275">The example</span></span>
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
<span data-ttu-id="82992-276">无效，因为没有开关部分具有可访问的终结点。</span><span class="sxs-lookup"><span data-stu-id="82992-276">is valid because no switch section has a reachable end point.</span></span> <span data-ttu-id="82992-277">与 C 不同， C++，执行的开关部分不允许使用以"贯穿"到下一步的开关部分和示例</span><span class="sxs-lookup"><span data-stu-id="82992-277">Unlike C and C++, execution of a switch section is not permitted to "fall through" to the next switch section, and the example</span></span>
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
<span data-ttu-id="82992-278">在编译时错误的结果。</span><span class="sxs-lookup"><span data-stu-id="82992-278">results in a compile-time error.</span></span> <span data-ttu-id="82992-279">执行的开关部分时的另一个开关部分，显式执行后接`goto case`或`goto default`必须使用语句：</span><span class="sxs-lookup"><span data-stu-id="82992-279">When execution of a switch section is to be followed by execution of another switch section, an explicit `goto case` or `goto default` statement must be used:</span></span>
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

<span data-ttu-id="82992-280">在允许多个标签*switch_section*。</span><span class="sxs-lookup"><span data-stu-id="82992-280">Multiple labels are permitted in a *switch_section*.</span></span> <span data-ttu-id="82992-281">该示例</span><span class="sxs-lookup"><span data-stu-id="82992-281">The example</span></span>
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
<span data-ttu-id="82992-282">是有效的。</span><span class="sxs-lookup"><span data-stu-id="82992-282">is valid.</span></span> <span data-ttu-id="82992-283">该示例不违反"无贯穿"规则，因为标签`case 2:`并`default:`是属于同一*switch_section*。</span><span class="sxs-lookup"><span data-stu-id="82992-283">The example does not violate the "no fall through" rule because the labels `case 2:` and `default:` are part of the same *switch_section*.</span></span>

<span data-ttu-id="82992-284">"无贯穿"规则将禁止在 C 中出现的 bug 的公共类和C++时`break`语句会无意中遗漏了。</span><span class="sxs-lookup"><span data-stu-id="82992-284">The "no fall through" rule prevents a common class of bugs that occur in C and C++ when `break` statements are accidentally omitted.</span></span> <span data-ttu-id="82992-285">此外，由于此规则的开关部分`switch`语句可以随意重新排列而不会影响该语句的行为。</span><span class="sxs-lookup"><span data-stu-id="82992-285">In addition, because of this rule, the switch sections of a `switch` statement can be arbitrarily rearranged without affecting the behavior of the statement.</span></span> <span data-ttu-id="82992-286">例如，部分`switch`上述语句可反转而不会影响该语句的行为：</span><span class="sxs-lookup"><span data-stu-id="82992-286">For example, the sections of the `switch` statement above can be reversed without affecting the behavior of the statement:</span></span>
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

<span data-ttu-id="82992-287">开关部分的语句列表通常以结尾`break`， `goto case`，或`goto default`允许语句，但呈现的语句列表的终结点无法访问任何构造。</span><span class="sxs-lookup"><span data-stu-id="82992-287">The statement list of a switch section typically ends in a `break`, `goto case`, or `goto default` statement, but any construct that renders the end point of the statement list unreachable is permitted.</span></span> <span data-ttu-id="82992-288">例如，`while`控制的布尔表达式的语句`true`知道永远不会覆盖其终结点。</span><span class="sxs-lookup"><span data-stu-id="82992-288">For example, a `while` statement controlled by the boolean expression `true` is known to never reach its end point.</span></span> <span data-ttu-id="82992-289">同样，`throw`或`return`语句始终会传输到其他位置的控件，永远不会到达其终结点。</span><span class="sxs-lookup"><span data-stu-id="82992-289">Likewise, a `throw` or `return` statement always transfers control elsewhere and never reaches its end point.</span></span> <span data-ttu-id="82992-290">因此，下面的示例是有效的：</span><span class="sxs-lookup"><span data-stu-id="82992-290">Thus, the following example is valid:</span></span>
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

<span data-ttu-id="82992-291">用于控制的类型`switch`语句可以是类型`string`。</span><span class="sxs-lookup"><span data-stu-id="82992-291">The governing type of a `switch` statement may be the type `string`.</span></span> <span data-ttu-id="82992-292">例如：</span><span class="sxs-lookup"><span data-stu-id="82992-292">For example:</span></span>
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

<span data-ttu-id="82992-293">像字符串相等运算符 ([字符串是否相等运算符](expressions.md#string-equality-operators))，则`switch`语句是区分大小写和 switch 表达式字符串完全匹配才会执行给定的开关部分`case`标签常量。</span><span class="sxs-lookup"><span data-stu-id="82992-293">Like the string equality operators ([String equality operators](expressions.md#string-equality-operators)), the `switch` statement is case sensitive and will execute a given switch section only if the switch expression string exactly matches a `case` label constant.</span></span>

<span data-ttu-id="82992-294">用于管理的类型时`switch`语句是`string`，值`null`允许为 case 标签常量。</span><span class="sxs-lookup"><span data-stu-id="82992-294">When the governing type of a `switch` statement is `string`, the value `null` is permitted as a case label constant.</span></span>

<span data-ttu-id="82992-295">*Statement_list*的 s *switch_block*可能包含声明语句 ([声明语句](statements.md#declaration-statements))。</span><span class="sxs-lookup"><span data-stu-id="82992-295">The *statement_list*s of a *switch_block* may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="82992-296">本地变量或常量的作用域中的 switch 块声明是 switch 块。</span><span class="sxs-lookup"><span data-stu-id="82992-296">The scope of a local variable or constant declared in a switch block is the switch block.</span></span>

<span data-ttu-id="82992-297">给定的开关部分的语句列表是如果`switch`语句为可访问且至少一个以下为 true:</span><span class="sxs-lookup"><span data-stu-id="82992-297">The statement list of a given switch section is reachable if the `switch` statement is reachable and at least one of the following is true:</span></span>

*  <span data-ttu-id="82992-298">Switch 表达式是一个非常量值。</span><span class="sxs-lookup"><span data-stu-id="82992-298">The switch expression is a non-constant value.</span></span>
*  <span data-ttu-id="82992-299">Switch 表达式是与匹配的常量值`case`开关部分中的标签。</span><span class="sxs-lookup"><span data-stu-id="82992-299">The switch expression is a constant value that matches a `case` label in the switch section.</span></span>
*  <span data-ttu-id="82992-300">Switch 表达式是与任何不匹配的常数值`case`标签和开关部分包含`default`标签。</span><span class="sxs-lookup"><span data-stu-id="82992-300">The switch expression is a constant value that doesn't match any `case` label, and the switch section contains the `default` label.</span></span>
*  <span data-ttu-id="82992-301">Switch 标签的开关部分引用的可访问`goto case`或`goto default`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-301">A switch label of the switch section is referenced by a reachable `goto case` or `goto default` statement.</span></span>

<span data-ttu-id="82992-302">终结点的`switch`语句是可访问，如果在至少一个以下条件：</span><span class="sxs-lookup"><span data-stu-id="82992-302">The end point of a `switch` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="82992-303">`switch`语句包含可访问`break`语句可退出`switch`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-303">The `switch` statement contains a reachable `break` statement that exits the `switch` statement.</span></span>
*  <span data-ttu-id="82992-304">`switch`语句是可访问，switch 表达式是一个非常量值，但没有`default`标签不存在。</span><span class="sxs-lookup"><span data-stu-id="82992-304">The `switch` statement is reachable, the switch expression is a non-constant value, and no `default` label is present.</span></span>
*  <span data-ttu-id="82992-305">`switch`语句是可访问，switch 表达式是与任何不匹配的常数值`case`标签，但没有`default`标签不存在。</span><span class="sxs-lookup"><span data-stu-id="82992-305">The `switch` statement is reachable, the switch expression is a constant value that doesn't match any `case` label, and no `default` label is present.</span></span>

## <a name="iteration-statements"></a><span data-ttu-id="82992-306">迭代语句</span><span class="sxs-lookup"><span data-stu-id="82992-306">Iteration statements</span></span>

<span data-ttu-id="82992-307">迭代语句重复执行嵌入的语句。</span><span class="sxs-lookup"><span data-stu-id="82992-307">Iteration statements repeatedly execute an embedded statement.</span></span>

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a><span data-ttu-id="82992-308">While 语句</span><span class="sxs-lookup"><span data-stu-id="82992-308">The while statement</span></span>

<span data-ttu-id="82992-309">`while`语句有条件地执行嵌入的语句零次或多次。</span><span class="sxs-lookup"><span data-stu-id="82992-309">The `while` statement conditionally executes an embedded statement zero or more times.</span></span>

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

<span data-ttu-id="82992-310">一个`while`执行语句，如下所示：</span><span class="sxs-lookup"><span data-stu-id="82992-310">A `while` statement is executed as follows:</span></span>

*  <span data-ttu-id="82992-311">*Boolean_expression* ([布尔表达式](expressions.md#boolean-expressions)) 进行计算。</span><span class="sxs-lookup"><span data-stu-id="82992-311">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="82992-312">如果布尔表达式生成`true`，控制传输到嵌入的语句。</span><span class="sxs-lookup"><span data-stu-id="82992-312">If the boolean expression yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="82992-313">如果控制到达嵌入的语句的结束点 (可能来自执行`continue`语句)，控制转移到其中的开头`while`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-313">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), control is transferred to the beginning of the `while` statement.</span></span>
*  <span data-ttu-id="82992-314">如果布尔表达式会生成`false`，控制转移到其中的终结点`while`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-314">If the boolean expression yields `false`, control is transferred to the end point of the `while` statement.</span></span>

<span data-ttu-id="82992-315">中的嵌入的语句`while`语句中，`break`语句 ([break 语句](statements.md#the-break-statement)) 可用于将控件传输到的终结点`while`语句 （因此结束迭代的嵌入语句），和一个`continue`语句 ([continue 语句](statements.md#the-continue-statement)) 可用于将控件传输到嵌入的语句结束点 (从而执行的另一个迭代`while`语句)。</span><span class="sxs-lookup"><span data-stu-id="82992-315">Within the embedded statement of a `while` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `while` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus performing another iteration of the `while` statement).</span></span>

<span data-ttu-id="82992-316">嵌入的语句`while`语句是可访问如果`while`语句可访问且布尔表达式不具有常量值`false`。</span><span class="sxs-lookup"><span data-stu-id="82992-316">The embedded statement of a `while` statement is reachable if the `while` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="82992-317">终结点的`while`语句是可访问，如果在至少一个以下条件：</span><span class="sxs-lookup"><span data-stu-id="82992-317">The end point of a `while` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="82992-318">`while`语句包含可访问`break`语句可退出`while`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-318">The `while` statement contains a reachable `break` statement that exits the `while` statement.</span></span>
*  <span data-ttu-id="82992-319">`while`语句可访问且布尔表达式不具有常量值`true`。</span><span class="sxs-lookup"><span data-stu-id="82992-319">The `while` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-do-statement"></a><span data-ttu-id="82992-320">Do 语句</span><span class="sxs-lookup"><span data-stu-id="82992-320">The do statement</span></span>

<span data-ttu-id="82992-321">`do`语句有条件地执行嵌入的语句的一个或多个时间。</span><span class="sxs-lookup"><span data-stu-id="82992-321">The `do` statement conditionally executes an embedded statement one or more times.</span></span>

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

<span data-ttu-id="82992-322">一个`do`执行语句，如下所示：</span><span class="sxs-lookup"><span data-stu-id="82992-322">A `do` statement is executed as follows:</span></span>

*  <span data-ttu-id="82992-323">控制传输到嵌入的语句。</span><span class="sxs-lookup"><span data-stu-id="82992-323">Control is transferred to the embedded statement.</span></span>
*  <span data-ttu-id="82992-324">当和控制到达嵌入的语句的结束点 (可能来自执行`continue`语句)，则*boolean_expression* ([布尔表达式](expressions.md#boolean-expressions)) 进行计算。</span><span class="sxs-lookup"><span data-stu-id="82992-324">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span> <span data-ttu-id="82992-325">如果布尔表达式会生成`true`，控制转移到其中的开头`do`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-325">If the boolean expression yields `true`, control is transferred to the beginning of the `do` statement.</span></span> <span data-ttu-id="82992-326">否则，将控制转移到其中的终结点`do`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-326">Otherwise, control is transferred to the end point of the `do` statement.</span></span>

<span data-ttu-id="82992-327">中的嵌入的语句`do`语句中，`break`语句 ([break 语句](statements.md#the-break-statement)) 可用于将控件传输到的终结点`do`语句 （因此结束迭代的嵌入语句），和一个`continue`语句 ([continue 语句](statements.md#the-continue-statement)) 可用于将控件传输到嵌入的语句结束点。</span><span class="sxs-lookup"><span data-stu-id="82992-327">Within the embedded statement of a `do` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `do` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement.</span></span>

<span data-ttu-id="82992-328">嵌入的语句`do`语句是可访问如果`do`语句是可访问。</span><span class="sxs-lookup"><span data-stu-id="82992-328">The embedded statement of a `do` statement is reachable if the `do` statement is reachable.</span></span>

<span data-ttu-id="82992-329">终结点的`do`语句是可访问，如果在至少一个以下条件：</span><span class="sxs-lookup"><span data-stu-id="82992-329">The end point of a `do` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="82992-330">`do`语句包含可访问`break`语句可退出`do`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-330">The `do` statement contains a reachable `break` statement that exits the `do` statement.</span></span>
*  <span data-ttu-id="82992-331">嵌入的语句的结束点是可访问和布尔表达式不具有常量值`true`。</span><span class="sxs-lookup"><span data-stu-id="82992-331">The end point of the embedded statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-for-statement"></a><span data-ttu-id="82992-332">For 语句</span><span class="sxs-lookup"><span data-stu-id="82992-332">The for statement</span></span>

<span data-ttu-id="82992-333">`for`语句计算初始化表达式的一个序列，然后，当条件为真时，重复执行嵌入的语句和一系列迭代表达式的计算结果。</span><span class="sxs-lookup"><span data-stu-id="82992-333">The `for` statement evaluates a sequence of initialization expressions and then, while a condition is true, repeatedly executes an embedded statement and evaluates a sequence of iteration expressions.</span></span>

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

<span data-ttu-id="82992-334">*For_initializer*(如果有） 由*local_variable_declaration* ([局部变量声明](statements.md#local-variable-declarations)) 或一系列*statement_表达式*s ([表达式语句](statements.md#expression-statements)) 以逗号隔开。</span><span class="sxs-lookup"><span data-stu-id="82992-334">The *for_initializer*, if present, consists of either a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) or a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span> <span data-ttu-id="82992-335">声明的局部变量的作用域*for_initializer*开始*local_variable_declarator*变量并延伸到嵌入的语句结束。</span><span class="sxs-lookup"><span data-stu-id="82992-335">The scope of a local variable declared by a *for_initializer* starts at the *local_variable_declarator* for the variable and extends to the end of the embedded statement.</span></span> <span data-ttu-id="82992-336">作用域包括*for_condition*并*for_iterator*。</span><span class="sxs-lookup"><span data-stu-id="82992-336">The scope includes the *for_condition* and the *for_iterator*.</span></span>

<span data-ttu-id="82992-337">*For_condition*(如果有） 必须*boolean_expression* ([布尔表达式](expressions.md#boolean-expressions))。</span><span class="sxs-lookup"><span data-stu-id="82992-337">The *for_condition*, if present, must be a *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)).</span></span>

<span data-ttu-id="82992-338">*For_iterator*(如果有） 的列表组成*statement_expression*s ([表达式语句](statements.md#expression-statements)) 以逗号隔开。</span><span class="sxs-lookup"><span data-stu-id="82992-338">The *for_iterator*, if present, consists of a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span>

<span data-ttu-id="82992-339">FOR 语句执行，如下所示：</span><span class="sxs-lookup"><span data-stu-id="82992-339">A for statement is executed as follows:</span></span>

*  <span data-ttu-id="82992-340">如果*for_initializer*显示变量的初始值设定项或写入时的顺序执行语句表达式。</span><span class="sxs-lookup"><span data-stu-id="82992-340">If a *for_initializer* is present, the variable initializers or statement expressions are executed in the order they are written.</span></span> <span data-ttu-id="82992-341">此步骤将仅执行一次。</span><span class="sxs-lookup"><span data-stu-id="82992-341">This step is only performed once.</span></span>
*  <span data-ttu-id="82992-342">如果*for_condition*存在，则它将进行计算。</span><span class="sxs-lookup"><span data-stu-id="82992-342">If a *for_condition* is present, it is evaluated.</span></span>
*  <span data-ttu-id="82992-343">如果*for_condition*不存在或如果评估结果`true`，控制传输到嵌入的语句。</span><span class="sxs-lookup"><span data-stu-id="82992-343">If the *for_condition* is not present or if the evaluation yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="82992-344">时和控制到达嵌入的语句的结束点 (可能来自执行`continue`语句)，表达式*for_iterator*，如果任何，计算在序列中，并且另一个迭代，则执行开始计算*for_condition*在前面步骤中。</span><span class="sxs-lookup"><span data-stu-id="82992-344">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the expressions of the *for_iterator*, if any, are evaluated in sequence, and then another iteration is performed, starting with evaluation of the *for_condition* in the step above.</span></span>
*  <span data-ttu-id="82992-345">如果*for_condition*存在而计算产生`false`，控制转移到其中的终结点`for`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-345">If the *for_condition* is present and the evaluation yields `false`, control is transferred to the end point of the `for` statement.</span></span>

<span data-ttu-id="82992-346">中的嵌入的语句`for`语句中，`break`语句 ([break 语句](statements.md#the-break-statement)) 可用于将控件传输到的终结点`for`语句 （因此结束迭代的嵌入语句），和一个`continue`语句 ([continue 语句](statements.md#the-continue-statement)) 可用于将控件传输到嵌入的语句结束点 (从而执行*for_iterator*和执行的另一个迭代`for`语句，从开始*for_condition*)。</span><span class="sxs-lookup"><span data-stu-id="82992-346">Within the embedded statement of a `for` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `for` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus executing the *for_iterator* and performing another iteration of the `for` statement, starting with the *for_condition*).</span></span>

<span data-ttu-id="82992-347">嵌入的语句的`for`语句是可访问，如果以下项之一为 true:</span><span class="sxs-lookup"><span data-stu-id="82992-347">The embedded statement of a `for` statement is reachable if one of the following is true:</span></span>

*  <span data-ttu-id="82992-348">`for`是可到达和无*for_condition*存在。</span><span class="sxs-lookup"><span data-stu-id="82992-348">The `for` statement is reachable and no *for_condition* is present.</span></span>
*  <span data-ttu-id="82992-349">`for`语句是可访问和一个*for_condition*存在且不具有常量值`false`。</span><span class="sxs-lookup"><span data-stu-id="82992-349">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `false`.</span></span>

<span data-ttu-id="82992-350">终结点的`for`语句是可访问，如果在至少一个以下条件：</span><span class="sxs-lookup"><span data-stu-id="82992-350">The end point of a `for` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="82992-351">`for`语句包含可访问`break`语句可退出`for`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-351">The `for` statement contains a reachable `break` statement that exits the `for` statement.</span></span>
*  <span data-ttu-id="82992-352">`for`语句是可访问和一个*for_condition*存在且不具有常量值`true`。</span><span class="sxs-lookup"><span data-stu-id="82992-352">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `true`.</span></span>

### <a name="the-foreach-statement"></a><span data-ttu-id="82992-353">Foreach 语句</span><span class="sxs-lookup"><span data-stu-id="82992-353">The foreach statement</span></span>

<span data-ttu-id="82992-354">`foreach`语句枚举集合，执行嵌入的语句的每个元素的集合中的元素。</span><span class="sxs-lookup"><span data-stu-id="82992-354">The `foreach` statement enumerates the elements of a collection, executing an embedded statement for each element of the collection.</span></span>

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

<span data-ttu-id="82992-355">*类型*并*标识符*的`foreach`语句声明***迭代变量***的语句。</span><span class="sxs-lookup"><span data-stu-id="82992-355">The *type* and *identifier* of a `foreach` statement declare the ***iteration variable*** of the statement.</span></span> <span data-ttu-id="82992-356">如果`var`标识符中指定为*local_variable_type*，并没有名为的类型`var`是在范围内，迭代变量称为***隐式类型化的迭代变量***，和类型的元素类型将被当成`foreach`语句，如下所示。</span><span class="sxs-lookup"><span data-stu-id="82992-356">If the `var` identifier is given as the *local_variable_type*, and no type named `var` is in scope, the iteration variable is said to be an ***implicitly typed iteration variable***, and its type is taken to be the element type of the `foreach` statement, as specified below.</span></span> <span data-ttu-id="82992-357">迭代变量对应于只读本地变量具有通过嵌入的语句进行了扩展作用域。</span><span class="sxs-lookup"><span data-stu-id="82992-357">The iteration variable corresponds to a read-only local variable with a scope that extends over the embedded statement.</span></span> <span data-ttu-id="82992-358">执行期间`foreach`语句，该迭代变量表示当前正在为其执行迭代的集合元素。</span><span class="sxs-lookup"><span data-stu-id="82992-358">During execution of a `foreach` statement, the iteration variable represents the collection element for which an iteration is currently being performed.</span></span> <span data-ttu-id="82992-359">如果尝试修改的迭代变量嵌入的语句会发生编译时错误 (通过分配或`++`并`--`运算符) 或传递作为的迭代变量`ref`或`out`参数。</span><span class="sxs-lookup"><span data-stu-id="82992-359">A compile-time error occurs if the embedded statement attempts to modify the iteration variable (via assignment or the `++` and `--` operators) or pass the iteration variable as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="82992-360">在下面的示例，为简洁起见， `IEnumerable`， `IEnumerator`，`IEnumerable<T>`并`IEnumerator<T>`命名空间中的相应类型，请参阅`System.Collections`和`System.Collections.Generic`。</span><span class="sxs-lookup"><span data-stu-id="82992-360">In the following, for brevity, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` and `IEnumerator<T>` refer to the corresponding types in the namespaces `System.Collections` and `System.Collections.Generic`.</span></span>

<span data-ttu-id="82992-361">Foreach 语句的编译时处理首先确定***集合类型***，***枚举器类型***并***元素类型***的表达式。</span><span class="sxs-lookup"><span data-stu-id="82992-361">The compile-time processing of a foreach statement first determines the ***collection type***, ***enumerator type*** and ***element type*** of the expression.</span></span> <span data-ttu-id="82992-362">此决定继续，如下所示：</span><span class="sxs-lookup"><span data-stu-id="82992-362">This determination proceeds as follows:</span></span>

*  <span data-ttu-id="82992-363">如果类型`X`的*表达式*为数组类型，则不从隐式引用转换`X`到`IEnumerable`接口 (因为`System.Array`实现此接口)。</span><span class="sxs-lookup"><span data-stu-id="82992-363">If the type `X` of *expression* is an array type then there is an implicit reference conversion from `X` to the `IEnumerable` interface (since `System.Array` implements this interface).</span></span> <span data-ttu-id="82992-364">***集合类型***是`IEnumerable`接口，***枚举器类型***是`IEnumerator`接口和***元素类型***是元素类型数组类型`X`。</span><span class="sxs-lookup"><span data-stu-id="82992-364">The ***collection type*** is the `IEnumerable` interface, the ***enumerator type*** is the `IEnumerator` interface and the ***element type*** is the element type of the array type `X`.</span></span>
*  <span data-ttu-id="82992-365">如果类型`X`的*表达式*是`dynamic`隐式转换则*表达式*到`IEnumerable`接口 ([隐式动态转换](conversions.md#implicit-dynamic-conversions))。</span><span class="sxs-lookup"><span data-stu-id="82992-365">If the type `X` of *expression* is `dynamic` then there is an implicit conversion from *expression* to the `IEnumerable` interface ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)).</span></span> <span data-ttu-id="82992-366">***集合类型***是`IEnumerable`接口并***枚举器类型***是`IEnumerator`接口。</span><span class="sxs-lookup"><span data-stu-id="82992-366">The ***collection type*** is the `IEnumerable` interface and the ***enumerator type*** is the `IEnumerator` interface.</span></span> <span data-ttu-id="82992-367">如果`var`标识符中指定为*local_variable_type*则***元素类型***是`dynamic`，否则它是`object`。</span><span class="sxs-lookup"><span data-stu-id="82992-367">If the `var` identifier is given as the *local_variable_type* then the ***element type*** is `dynamic`, otherwise it is `object`.</span></span>
*  <span data-ttu-id="82992-368">否则，确定是否类型`X`具有相应`GetEnumerator`方法：</span><span class="sxs-lookup"><span data-stu-id="82992-368">Otherwise, determine whether the type `X` has an appropriate `GetEnumerator` method:</span></span>
   * <span data-ttu-id="82992-369">对类型执行成员查找`X`具有标识符`GetEnumerator`且不使用类型参数。</span><span class="sxs-lookup"><span data-stu-id="82992-369">Perform member lookup on the type `X` with identifier `GetEnumerator` and no type arguments.</span></span> <span data-ttu-id="82992-370">如果成员查找不会生成匹配项，或者它会造成多义性，或生成不是方法组的匹配项，检查可枚举接口如下所述。</span><span class="sxs-lookup"><span data-stu-id="82992-370">If the member lookup does not produce a match, or it produces an ambiguity, or produces a match that is not a method group, check for an enumerable interface as described below.</span></span> <span data-ttu-id="82992-371">建议如果成员查找生成的任何内容除外方法组或没有匹配项的情况下发出警告。</span><span class="sxs-lookup"><span data-stu-id="82992-371">It is recommended that a warning be issued if member lookup produces anything except a method group or no match.</span></span>
   * <span data-ttu-id="82992-372">执行重载决策使用生成的方法组和参数列表为空。</span><span class="sxs-lookup"><span data-stu-id="82992-372">Perform overload resolution using the resulting method group and an empty argument list.</span></span> <span data-ttu-id="82992-373">如果没有适用的方法的重载解析结果，导致二义性，或导致单个的最佳方法，但该方法是静态或非公共，检查的可枚举接口，如下所述。</span><span class="sxs-lookup"><span data-stu-id="82992-373">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, check for an enumerable interface as described below.</span></span> <span data-ttu-id="82992-374">建议如果除明确的公共实例方法或不适用的方法的重载解析生成任何内容的情况下发出警告。</span><span class="sxs-lookup"><span data-stu-id="82992-374">It is recommended that a warning be issued if overload resolution produces anything except an unambiguous public instance method or no applicable methods.</span></span>
   * <span data-ttu-id="82992-375">如果的返回类型`E`的`GetEnumerator`方法不是类、 结构或接口类型，错误会生成和采取任何进一步的步骤。</span><span class="sxs-lookup"><span data-stu-id="82992-375">If the return type `E` of the `GetEnumerator` method is not a class, struct or interface type, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="82992-376">在执行成员查找`E`具有标识符`Current`且不使用类型参数。</span><span class="sxs-lookup"><span data-stu-id="82992-376">Member lookup is performed on `E` with the identifier `Current` and no type arguments.</span></span> <span data-ttu-id="82992-377">如果成员查找不产生任何匹配，结果是一个错误，或者会得到一个公共实例属性，允许读取以外，将会生成错误并采取任何进一步的步骤。</span><span class="sxs-lookup"><span data-stu-id="82992-377">If the member lookup produces no match, the result is an error, or the result is anything except a public instance property that permits reading, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="82992-378">在执行成员查找`E`具有标识符`MoveNext`且不使用类型参数。</span><span class="sxs-lookup"><span data-stu-id="82992-378">Member lookup is performed on `E` with the identifier `MoveNext` and no type arguments.</span></span> <span data-ttu-id="82992-379">如果成员查找不产生任何匹配，结果是一个错误，或者会得到一个方法组以外，将会生成错误并采取任何进一步的步骤。</span><span class="sxs-lookup"><span data-stu-id="82992-379">If the member lookup produces no match, the result is an error, or the result is anything except a method group, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="82992-380">重载解析执行方法在组上使用参数列表为空。</span><span class="sxs-lookup"><span data-stu-id="82992-380">Overload resolution is performed on the method group with an empty argument list.</span></span> <span data-ttu-id="82992-381">如果没有适用的方法、 导致二义性或在单个的最佳方法，但该方法的结果中的重载解析结果是静态或非公共，或其返回类型不是`bool`，将会生成错误并采取任何进一步的步骤。</span><span class="sxs-lookup"><span data-stu-id="82992-381">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, or its return type is not `bool`, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="82992-382">***集合类型***是`X`，则***枚举器类型***是`E`，以及***元素类型***是一种`Current`属性。</span><span class="sxs-lookup"><span data-stu-id="82992-382">The ***collection type*** is `X`, the ***enumerator type*** is `E`, and the ***element type*** is the type of the `Current` property.</span></span>

*  <span data-ttu-id="82992-383">否则，检查为可枚举接口：</span><span class="sxs-lookup"><span data-stu-id="82992-383">Otherwise, check for an enumerable interface:</span></span>
   * <span data-ttu-id="82992-384">如果在所有类型`Ti`它们没有隐式转换`X`到`IEnumerable<Ti>`，没有唯一类型`T`以便`T`不是`dynamic`以及所有其他`Ti`没有从隐式转换`IEnumerable<T>`到`IEnumerable<Ti>`，然后***集合类型***是接口`IEnumerable<T>`，则***枚举器类型***是接口`IEnumerator<T>`，并***元素类型***是`T`。</span><span class="sxs-lookup"><span data-stu-id="82992-384">If among all the types `Ti` for which there is an implicit conversion from `X` to `IEnumerable<Ti>`, there is a unique type `T` such that `T` is not `dynamic` and for all the other `Ti` there is an implicit conversion from `IEnumerable<T>` to `IEnumerable<Ti>`, then the ***collection type*** is the interface `IEnumerable<T>`, the ***enumerator type*** is the interface `IEnumerator<T>`, and the ***element type*** is `T`.</span></span>
   * <span data-ttu-id="82992-385">否则为如果多个此类类型`T`，然后将会生成错误并采取任何进一步的步骤。</span><span class="sxs-lookup"><span data-stu-id="82992-385">Otherwise, if there is more than one such type `T`, then an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="82992-386">否则为如果隐式转换`X`到`System.Collections.IEnumerable`接口，则***集合类型***是此接口***枚举器类型***是接口`System.Collections.IEnumerator`，并***元素类型***是`object`。</span><span class="sxs-lookup"><span data-stu-id="82992-386">Otherwise, if there is an implicit conversion from `X` to the `System.Collections.IEnumerable` interface, then the ***collection type*** is this interface, the ***enumerator type*** is the interface `System.Collections.IEnumerator`, and the ***element type*** is `object`.</span></span>
   * <span data-ttu-id="82992-387">否则为将会生成错误并采取任何进一步的步骤。</span><span class="sxs-lookup"><span data-stu-id="82992-387">Otherwise, an error is produced and no further steps are taken.</span></span>

<span data-ttu-id="82992-388">上述步骤中，如果成功，明确地生成集合类型`C`，枚举器类型`E`和元素类型`T`。</span><span class="sxs-lookup"><span data-stu-id="82992-388">The above steps, if successful, unambiguously produce a collection type `C`, enumerator type `E` and element type `T`.</span></span> <span data-ttu-id="82992-389">窗体的 foreach 语句</span><span class="sxs-lookup"><span data-stu-id="82992-389">A foreach statement of the form</span></span>
```csharp
foreach (V v in x) embedded_statement
```
<span data-ttu-id="82992-390">然后扩展到：</span><span class="sxs-lookup"><span data-stu-id="82992-390">is then expanded to:</span></span>
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

<span data-ttu-id="82992-391">在变量`e`不可见或供表达式`x`或嵌入的语句或程序的任何其他源代码。</span><span class="sxs-lookup"><span data-stu-id="82992-391">The variable `e` is not visible to or accessible to the expression `x` or the embedded statement or any other source code of the program.</span></span> <span data-ttu-id="82992-392">该变量`v`是嵌入的语句中以只读的。</span><span class="sxs-lookup"><span data-stu-id="82992-392">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="82992-393">如果不存在的显式转换 ([显式转换](conversions.md#explicit-conversions)) 从`T`（元素类型） 对`V`( *local_variable_type* foreach 语句中)，将会生成错误并采取任何进一步的步骤。</span><span class="sxs-lookup"><span data-stu-id="82992-393">If there is not an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) from `T` (the element type) to `V` (the *local_variable_type* in the foreach statement), an error is produced and no further steps are taken.</span></span> <span data-ttu-id="82992-394">如果`x`具有值`null`、`System.NullReferenceException`在运行时引发。</span><span class="sxs-lookup"><span data-stu-id="82992-394">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

<span data-ttu-id="82992-395">实现允许以实现给定的 foreach 语句不同，例如出于性能原因，只要行为是一致具有更高版本的扩展。</span><span class="sxs-lookup"><span data-stu-id="82992-395">An implementation is permitted to implement a given foreach-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="82992-396">位置`v`在 while 循环是有关如何捕获由任何匿名函数中发生重要*embedded_statement*。</span><span class="sxs-lookup"><span data-stu-id="82992-396">The placement of `v` inside the while loop is important for how it is captured by any anonymous function occurring in the *embedded_statement*.</span></span>

<span data-ttu-id="82992-397">例如：</span><span class="sxs-lookup"><span data-stu-id="82992-397">For example:</span></span>
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
<span data-ttu-id="82992-398">如果`v`声明外部 while 循环中，它会在所有迭代和后的其值间共享循环将是最后一个值， `13`，哪些调用属于`f`将打印。</span><span class="sxs-lookup"><span data-stu-id="82992-398">If `v` was declared outside of the while loop, it would be shared among all iterations, and its value after the for loop would be the final value, `13`, which is what the invocation of `f` would print.</span></span> <span data-ttu-id="82992-399">相反，因为每个迭代都有其自己的变量`v`，通过捕获的一个`f`第一次迭代将继续保存值`7`，这是将打印的内容。</span><span class="sxs-lookup"><span data-stu-id="82992-399">Instead, because each iteration has its own variable `v`, the one captured by `f` in the first iteration will continue to hold the value `7`, which is what will be printed.</span></span> <span data-ttu-id="82992-400">(注意： 早期版本的 C# 声明`v`外部的 while 循环。)</span><span class="sxs-lookup"><span data-stu-id="82992-400">(Note: earlier versions of C# declared `v` outside of the while loop.)</span></span>

<span data-ttu-id="82992-401">正文最后块构造根据以下步骤：</span><span class="sxs-lookup"><span data-stu-id="82992-401">The body of the finally block is constructed according to the following steps:</span></span>

*  <span data-ttu-id="82992-402">如果没有隐式转换`E`到`System.IDisposable`接口，则</span><span class="sxs-lookup"><span data-stu-id="82992-402">If there is an implicit conversion from `E` to the `System.IDisposable` interface, then</span></span>
   *  <span data-ttu-id="82992-403">如果`E`是不可以为 null 的值类型则 finally 子句扩展为的语义等效项：</span><span class="sxs-lookup"><span data-stu-id="82992-403">If `E` is a non-nullable value type then the finally clause is expanded to the semantic equivalent  of:</span></span>

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  <span data-ttu-id="82992-404">否则为 finally 子句扩展为的语义等效项：</span><span class="sxs-lookup"><span data-stu-id="82992-404">Otherwise the finally clause is expanded to the semantic equivalent of:</span></span>

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   <span data-ttu-id="82992-405">除非，如果`E`是值类型或类型参数为值类型，则强制转换的实例化`e`到`System.IDisposable`不会导致装箱发生。</span><span class="sxs-lookup"><span data-stu-id="82992-405">except that if `E` is a value type, or a type parameter instantiated to a value type, then the cast of `e` to `System.IDisposable` will not cause boxing to occur.</span></span>

*  <span data-ttu-id="82992-406">否则为如果`E`是密封的类型，最后将子句扩展为一个空白块：</span><span class="sxs-lookup"><span data-stu-id="82992-406">Otherwise, if `E` is a sealed type, the finally clause is expanded to an empty block:</span></span>

   ```csharp
   finally {
   }
   ```

*  <span data-ttu-id="82992-407">否则为最后子句扩展为：</span><span class="sxs-lookup"><span data-stu-id="82992-407">Otherwise, the finally clause is expanded to:</span></span>

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   <span data-ttu-id="82992-408">本地变量`d`不可见或任何用户代码可以访问。</span><span class="sxs-lookup"><span data-stu-id="82992-408">The local variable `d` is not visible to or accessible to any user code.</span></span> <span data-ttu-id="82992-409">具体而言，不是冲突与其作用域包括的任何其他变量 finally 块。</span><span class="sxs-lookup"><span data-stu-id="82992-409">In particular, it does not conflict with any other variable whose scope includes the finally block.</span></span>

<span data-ttu-id="82992-410">依据的顺序`foreach`遍历数组的元素，如下所示：为升序索引顺序遍历一维数组元素，从开始索引 `0`，以索引结束`Length - 1`。</span><span class="sxs-lookup"><span data-stu-id="82992-410">The order in which `foreach` traverses the elements of an array, is as follows: For single-dimensional arrays elements are traversed in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="82992-411">对于多维数组，以便最右边的维度的索引是提高第一个，然后左边紧邻的维度，等向左遍历元素。</span><span class="sxs-lookup"><span data-stu-id="82992-411">For multi-dimensional arrays, elements are traversed such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span>

<span data-ttu-id="82992-412">下面的示例输出中元素顺序的二维数组中每个值：</span><span class="sxs-lookup"><span data-stu-id="82992-412">The following example prints out each value in a two-dimensional array, in element order:</span></span>
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
<span data-ttu-id="82992-413">生成的输出如下所示：</span><span class="sxs-lookup"><span data-stu-id="82992-413">The output produced is as follows:</span></span>
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

<span data-ttu-id="82992-414">在示例</span><span class="sxs-lookup"><span data-stu-id="82992-414">In the example</span></span>
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
<span data-ttu-id="82992-415">类型`n`被推断为`int`的元素类型`numbers`。</span><span class="sxs-lookup"><span data-stu-id="82992-415">the type of `n` is inferred to be `int`, the element type of `numbers`.</span></span>

## <a name="jump-statements"></a><span data-ttu-id="82992-416">跳转语句</span><span class="sxs-lookup"><span data-stu-id="82992-416">Jump statements</span></span>

<span data-ttu-id="82992-417">跳转语句无条件地将控制权。</span><span class="sxs-lookup"><span data-stu-id="82992-417">Jump statements unconditionally transfer control.</span></span>

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

<span data-ttu-id="82992-418">跳转语句将控制转移到的位置称为***目标***的跳转语句。</span><span class="sxs-lookup"><span data-stu-id="82992-418">The location to which a jump statement transfers control is called the ***target*** of the jump statement.</span></span>

<span data-ttu-id="82992-419">时跳转语句发生在块中，并且该跳转语句的目标是该块外部，称跳转语句***退出***块。</span><span class="sxs-lookup"><span data-stu-id="82992-419">When a jump statement occurs within a block, and the target of that jump statement is outside that block, the jump statement is said to ***exit*** the block.</span></span> <span data-ttu-id="82992-420">虽然跳转语句可能会将控件转移出一个块，它可永远不会将控制转移到一个块。</span><span class="sxs-lookup"><span data-stu-id="82992-420">While a jump statement may transfer control out of a block, it can never transfer control into a block.</span></span>

<span data-ttu-id="82992-421">跳转语句的执行十分复杂，是否存在干预性`try`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-421">Execution of jump statements is complicated by the presence of intervening `try` statements.</span></span> <span data-ttu-id="82992-422">如果没有此类`try`语句，跳转语句无条件将控制从跳转语句到其目标。</span><span class="sxs-lookup"><span data-stu-id="82992-422">In the absence of such `try` statements, a jump statement unconditionally transfers control from the jump statement to its target.</span></span> <span data-ttu-id="82992-423">出现这种干预性的情况下`try`语句，执行是更复杂。</span><span class="sxs-lookup"><span data-stu-id="82992-423">In the presence of such intervening `try` statements, execution is more complex.</span></span> <span data-ttu-id="82992-424">如果跳转语句退出一个或多个`try`相关联的块`finally`块，控制最初为传输`finally`块的最内层`try`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-424">If the jump statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="82992-425">如果控制到达的终结点`finally`块中，控制传输到`finally`的下一步封闭块`try`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-425">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="82992-426">重复此过程，直到`finally`的所有块干预`try`已执行语句。</span><span class="sxs-lookup"><span data-stu-id="82992-426">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>

<span data-ttu-id="82992-427">在示例</span><span class="sxs-lookup"><span data-stu-id="82992-427">In the example</span></span>
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
<span data-ttu-id="82992-428">`finally`与两个相关联的块`try`语句执行之前将控制权转交给跳转语句的目标。</span><span class="sxs-lookup"><span data-stu-id="82992-428">the `finally` blocks associated with two `try` statements are executed before control is transferred to the target of the jump statement.</span></span>

<span data-ttu-id="82992-429">生成的输出如下所示：</span><span class="sxs-lookup"><span data-stu-id="82992-429">The output produced is as follows:</span></span>
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a><span data-ttu-id="82992-430">Break 语句</span><span class="sxs-lookup"><span data-stu-id="82992-430">The break statement</span></span>

<span data-ttu-id="82992-431">`break`语句将退出的最近的封闭`switch`， `while`， `do`， `for`，或`foreach`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-431">The `break` statement exits the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
break_statement
    : 'break' ';'
    ;
```

<span data-ttu-id="82992-432">目标`break`语句是终结点的最近的封闭`switch`， `while`， `do`， `for`，或`foreach`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-432">The target of a `break` statement is the end point of the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="82992-433">如果`break`语句不括`switch`， `while`， `do`， `for`，或`foreach`语句中，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="82992-433">If a `break` statement is not enclosed by a `switch`, `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="82992-434">当多个`switch`， `while`， `do`， `for`，或`foreach`语句嵌套在彼此之上，`break`语句仅适用于最内部的语句。</span><span class="sxs-lookup"><span data-stu-id="82992-434">When multiple `switch`, `while`, `do`, `for`, or `foreach` statements are nested within each other, a `break` statement applies only to the innermost statement.</span></span> <span data-ttu-id="82992-435">若要跨多个嵌套级别将控制权`goto`语句 ([goto 语句](statements.md#the-goto-statement)) 必须使用。</span><span class="sxs-lookup"><span data-stu-id="82992-435">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="82992-436">一个`break`语句无法退出`finally`块 ([try 语句](statements.md#the-try-statement))。</span><span class="sxs-lookup"><span data-stu-id="82992-436">A `break` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="82992-437">当`break`语句出现在`finally`block 的目标`break`语句必须是在同一个`finally`阻止; 否则，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="82992-437">When a `break` statement occurs within a `finally` block, the target of the `break` statement must be within the same `finally` block; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="82992-438">一个`break`执行语句，如下所示：</span><span class="sxs-lookup"><span data-stu-id="82992-438">A `break` statement is executed as follows:</span></span>

*  <span data-ttu-id="82992-439">如果`break`语句退出一个或多个`try`相关联的块`finally`块，控制最初为传输`finally`块的最内层`try`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-439">If the `break` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="82992-440">如果控制到达的终结点`finally`块中，控制传输到`finally`的下一步封闭块`try`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-440">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="82992-441">重复此过程，直到`finally`的所有块干预`try`已执行语句。</span><span class="sxs-lookup"><span data-stu-id="82992-441">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="82992-442">控件被转移到的目标`break`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-442">Control is transferred to the target of the `break` statement.</span></span>

<span data-ttu-id="82992-443">因为`break`语句无条件将控制转移到其他位置，终结点的`break`语句永远不会是可访问。</span><span class="sxs-lookup"><span data-stu-id="82992-443">Because a `break` statement unconditionally transfers control elsewhere, the end point of a `break` statement is never reachable.</span></span>

### <a name="the-continue-statement"></a><span data-ttu-id="82992-444">Continue 语句</span><span class="sxs-lookup"><span data-stu-id="82992-444">The continue statement</span></span>

<span data-ttu-id="82992-445">`continue`语句将启动最近的封闭的新迭代`while`， `do`， `for`，或`foreach`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-445">The `continue` statement starts a new iteration of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
continue_statement
    : 'continue' ';'
    ;
```

<span data-ttu-id="82992-446">目标`continue`语句是终结点的最近的封闭的嵌入式语句`while`， `do`， `for`，或`foreach`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-446">The target of a `continue` statement is the end point of the embedded statement of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="82992-447">如果`continue`语句不括`while`， `do`， `for`，或`foreach`语句中，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="82992-447">If a `continue` statement is not enclosed by a `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="82992-448">当多个`while`， `do`， `for`，或`foreach`语句嵌套在彼此之上，`continue`语句仅适用于最内部的语句。</span><span class="sxs-lookup"><span data-stu-id="82992-448">When multiple `while`, `do`, `for`, or `foreach` statements are nested within each other, a `continue` statement applies only to the innermost statement.</span></span> <span data-ttu-id="82992-449">若要跨多个嵌套级别将控制权`goto`语句 ([goto 语句](statements.md#the-goto-statement)) 必须使用。</span><span class="sxs-lookup"><span data-stu-id="82992-449">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="82992-450">一个`continue`语句无法退出`finally`块 ([try 语句](statements.md#the-try-statement))。</span><span class="sxs-lookup"><span data-stu-id="82992-450">A `continue` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="82992-451">当`continue`语句出现在`finally`block 的目标`continue`语句必须是在同一个`finally`阻止; 否则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="82992-451">When a `continue` statement occurs within a `finally` block, the target of the `continue` statement must be within the same `finally` block; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="82992-452">一个`continue`执行语句，如下所示：</span><span class="sxs-lookup"><span data-stu-id="82992-452">A `continue` statement is executed as follows:</span></span>

*  <span data-ttu-id="82992-453">如果`continue`语句退出一个或多个`try`相关联的块`finally`块，控制最初为传输`finally`块的最内层`try`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-453">If the `continue` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="82992-454">如果控制到达的终结点`finally`块中，控制传输到`finally`的下一步封闭块`try`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-454">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="82992-455">重复此过程，直到`finally`的所有块干预`try`已执行语句。</span><span class="sxs-lookup"><span data-stu-id="82992-455">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="82992-456">控件被转移到的目标`continue`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-456">Control is transferred to the target of the `continue` statement.</span></span>

<span data-ttu-id="82992-457">因为`continue`语句无条件将控制转移到其他位置，终结点的`continue`语句永远不会是可访问。</span><span class="sxs-lookup"><span data-stu-id="82992-457">Because a `continue` statement unconditionally transfers control elsewhere, the end point of a `continue` statement is never reachable.</span></span>

### <a name="the-goto-statement"></a><span data-ttu-id="82992-458">goto 语句</span><span class="sxs-lookup"><span data-stu-id="82992-458">The goto statement</span></span>

<span data-ttu-id="82992-459">`goto`语句将控制转移到由标签标记的语句。</span><span class="sxs-lookup"><span data-stu-id="82992-459">The `goto` statement transfers control to a statement that is marked by a label.</span></span>

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

<span data-ttu-id="82992-460">目标`goto`*标识符*语句是用给定标签标记的语句。</span><span class="sxs-lookup"><span data-stu-id="82992-460">The target of a `goto` *identifier* statement is the labeled statement with the given label.</span></span> <span data-ttu-id="82992-461">如果当前的函数成员中不存在具有给定名称的标签或`goto`语句不是标签，作用域内将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="82992-461">If a label with the given name does not exist in the current function member, or if the `goto` statement is not within the scope of the label, a compile-time error occurs.</span></span> <span data-ttu-id="82992-462">此规则将允许使用`goto`语句来移交控制权超出嵌套作用域，而不是嵌套的作用域。</span><span class="sxs-lookup"><span data-stu-id="82992-462">This rule permits the use of a `goto` statement to transfer control out of a nested scope, but not into a nested scope.</span></span> <span data-ttu-id="82992-463">在示例</span><span class="sxs-lookup"><span data-stu-id="82992-463">In the example</span></span>
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
<span data-ttu-id="82992-464">`goto`语句用于将控件转移出嵌套的作用域。</span><span class="sxs-lookup"><span data-stu-id="82992-464">a `goto` statement is used to transfer control out of a nested scope.</span></span>

<span data-ttu-id="82992-465">目标`goto case`语句是语句列表中立即封闭`switch`语句 ([switch 语句](statements.md#the-switch-statement))，其中包含`case`使用给定的常量值的标签。</span><span class="sxs-lookup"><span data-stu-id="82992-465">The target of a `goto case` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `case` label with the given constant value.</span></span> <span data-ttu-id="82992-466">如果`goto case`语句不括`switch`语句中，如果*constant_expression*不能隐式转换 ([隐式转换](conversions.md#implicit-conversions)) 用于控制的类型最近的封闭`switch`语句，或者，如果最近的封闭`switch`语句不包含`case`标签使用给定的常量值，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="82992-466">If the `goto case` statement is not enclosed by a `switch` statement, if the *constant_expression* is not implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the nearest enclosing `switch` statement, or if the nearest enclosing `switch` statement does not contain a `case` label with the given constant value, a compile-time error occurs.</span></span>

<span data-ttu-id="82992-467">目标`goto default`语句是语句列表中立即封闭`switch`语句 ([switch 语句](statements.md#the-switch-statement))，其中包含`default`标签。</span><span class="sxs-lookup"><span data-stu-id="82992-467">The target of a `goto default` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `default` label.</span></span> <span data-ttu-id="82992-468">如果`goto default`语句不括`switch`语句，或者，如果最近的封闭`switch`语句不包含`default`标签，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="82992-468">If the `goto default` statement is not enclosed by a `switch` statement, or if the nearest enclosing `switch` statement does not contain a `default` label, a compile-time error occurs.</span></span>

<span data-ttu-id="82992-469">一个`goto`语句无法退出`finally`块 ([try 语句](statements.md#the-try-statement))。</span><span class="sxs-lookup"><span data-stu-id="82992-469">A `goto` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="82992-470">当`goto`语句出现在`finally`block 的目标`goto`语句必须是在同一个`finally`块中，否则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="82992-470">When a `goto` statement occurs within a `finally` block, the target of the `goto` statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="82992-471">一个`goto`执行语句，如下所示：</span><span class="sxs-lookup"><span data-stu-id="82992-471">A `goto` statement is executed as follows:</span></span>

*  <span data-ttu-id="82992-472">如果`goto`语句退出一个或多个`try`相关联的块`finally`块，控制最初为传输`finally`块的最内层`try`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-472">If the `goto` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="82992-473">如果控制到达的终结点`finally`块中，控制传输到`finally`的下一步封闭块`try`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-473">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="82992-474">重复此过程，直到`finally`的所有块干预`try`已执行语句。</span><span class="sxs-lookup"><span data-stu-id="82992-474">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="82992-475">控件被转移到的目标`goto`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-475">Control is transferred to the target of the `goto` statement.</span></span>

<span data-ttu-id="82992-476">因为`goto`语句无条件将控制转移到其他位置，终结点的`goto`语句永远不会是可访问。</span><span class="sxs-lookup"><span data-stu-id="82992-476">Because a `goto` statement unconditionally transfers control elsewhere, the end point of a `goto` statement is never reachable.</span></span>

### <a name="the-return-statement"></a><span data-ttu-id="82992-477">Return 语句</span><span class="sxs-lookup"><span data-stu-id="82992-477">The return statement</span></span>

<span data-ttu-id="82992-478">`return`语句将控制权返回到在其中函数的当前调用方`return`语句将出现。</span><span class="sxs-lookup"><span data-stu-id="82992-478">The `return` statement returns control to the current caller of the function in which the `return` statement appears.</span></span>

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

<span data-ttu-id="82992-479">一个`return`仅在不会计算一个值，即，具有结果类型的方法的函数成员中可以使用不包含表达式的语句 ([方法主体](classes.md#method-body)) `void`，则`set`的属性访问器或索引器`add`和`remove`事件、 实例构造函数、 静态构造函数或析构函数的访问器。</span><span class="sxs-lookup"><span data-stu-id="82992-479">A `return` statement with no expression can be used only in a function member that does not compute a value, that is, a method with the result type ([Method body](classes.md#method-body)) `void`, the `set` accessor of a property or indexer, the `add` and `remove` accessors of an event, an instance constructor, a static constructor, or a destructor.</span></span>

<span data-ttu-id="82992-480">一个`return`使用的表达式的语句只能在计算一个值，它是具有非 void 结果类型的方法的函数成员中使用`get`访问器的属性或索引器或用户定义的运算符。</span><span class="sxs-lookup"><span data-stu-id="82992-480">A `return` statement with an expression can only be used in a function member that computes a value, that is, a method with a non-void result type, the `get` accessor of a property or indexer, or a user-defined operator.</span></span> <span data-ttu-id="82992-481">隐式转换 ([隐式转换](conversions.md#implicit-conversions)) 到包含的函数成员的返回类型必须存在从表达式的类型。</span><span class="sxs-lookup"><span data-stu-id="82992-481">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression to the return type of the containing function member.</span></span>

<span data-ttu-id="82992-482">语句还可以使用匿名函数表达式的正文中返回 ([匿名函数表达式](expressions.md#anonymous-function-expressions))，并参与确定这些函数的转换。</span><span class="sxs-lookup"><span data-stu-id="82992-482">Return statements can also be used in the body of anonymous function expressions ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), and participate in determining which conversions exist for those functions.</span></span>

<span data-ttu-id="82992-483">它是编译时错误`return`语句出现在`finally`块 ([try 语句](statements.md#the-try-statement))。</span><span class="sxs-lookup"><span data-stu-id="82992-483">It is a compile-time error for a `return` statement to appear in a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="82992-484">一个`return`执行语句，如下所示：</span><span class="sxs-lookup"><span data-stu-id="82992-484">A `return` statement is executed as follows:</span></span>

*  <span data-ttu-id="82992-485">如果`return`语句指定一个表达式，计算该表达式并生成的值转换为包含函数的返回类型的隐式转换。</span><span class="sxs-lookup"><span data-stu-id="82992-485">If the `return` statement specifies an expression, the expression is evaluated and the resulting value is converted to the return type of the containing function by an implicit conversion.</span></span> <span data-ttu-id="82992-486">转换的结果将成为函数生成的结果值。</span><span class="sxs-lookup"><span data-stu-id="82992-486">The result of the conversion becomes the result value produced by the function.</span></span>
*  <span data-ttu-id="82992-487">如果`return`语句包含一个或多个`try`或`catch`相关联的块`finally`块，控制最初为传输`finally`块的最内层`try`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-487">If the `return` statement is enclosed by one or more `try` or `catch` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="82992-488">如果控制到达的终结点`finally`块中，控制传输到`finally`的下一步封闭块`try`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-488">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="82992-489">重复此过程，直到`finally`所有封闭的块`try`已执行语句。</span><span class="sxs-lookup"><span data-stu-id="82992-489">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="82992-490">如果包含的函数不是异步函数，如果任何控件被返回到结果值，以及包含函数的调用方中。</span><span class="sxs-lookup"><span data-stu-id="82992-490">If the containing function is not an async function, control is returned to the caller of the containing function along with the result value, if any.</span></span>
*  <span data-ttu-id="82992-491">如果包含的函数是异步函数，控制权返回给当前调用方，并且结果记录的值，如果有的话，是在返回的任务中所述 ([枚举器接口](classes.md#enumerator-interfaces))。</span><span class="sxs-lookup"><span data-stu-id="82992-491">If the containing function is an async function, control is returned to the current caller, and the result value, if any, is recorded in the return task as described in ([Enumerator interfaces](classes.md#enumerator-interfaces)).</span></span>

<span data-ttu-id="82992-492">因为`return`语句无条件将控制转移到其他位置，终结点的`return`语句永远不会是可访问。</span><span class="sxs-lookup"><span data-stu-id="82992-492">Because a `return` statement unconditionally transfers control elsewhere, the end point of a `return` statement is never reachable.</span></span>

### <a name="the-throw-statement"></a><span data-ttu-id="82992-493">Throw 语句</span><span class="sxs-lookup"><span data-stu-id="82992-493">The throw statement</span></span>

<span data-ttu-id="82992-494">`throw`语句将引发异常。</span><span class="sxs-lookup"><span data-stu-id="82992-494">The `throw` statement throws an exception.</span></span>

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

<span data-ttu-id="82992-495">一个`throw`的表达式的语句将引发计算该表达式生成的值。</span><span class="sxs-lookup"><span data-stu-id="82992-495">A `throw` statement with an expression throws the value produced by evaluating the expression.</span></span> <span data-ttu-id="82992-496">表达式必须表示类类型的值`System.Exception`，派生的类类型的`System.Exception`类型或类型参数类型具有`System.Exception`（或其中的子类） 用作其有效的基类。</span><span class="sxs-lookup"><span data-stu-id="82992-496">The expression must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="82992-497">如果该表达式的计算将生成`null`、`System.NullReferenceException`改为引发。</span><span class="sxs-lookup"><span data-stu-id="82992-497">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="82992-498">一个`throw`可以仅在使用不包含表达式的语句`catch`阻止，在这种情况下该语句重新引发的异常当前正在处理的`catch`块。</span><span class="sxs-lookup"><span data-stu-id="82992-498">A `throw` statement with no expression can be used only in a `catch` block, in which case that statement re-throws the exception that is currently being handled by that `catch` block.</span></span>

<span data-ttu-id="82992-499">因为`throw`语句无条件将控制转移到其他位置，终结点的`throw`语句永远不会是可访问。</span><span class="sxs-lookup"><span data-stu-id="82992-499">Because a `throw` statement unconditionally transfers control elsewhere, the end point of a `throw` statement is never reachable.</span></span>

<span data-ttu-id="82992-500">当引发异常时，将控制转移到第一个`catch`子句中封闭`try`能够处理该异常的语句。</span><span class="sxs-lookup"><span data-stu-id="82992-500">When an exception is thrown, control is transferred to the first `catch` clause in an enclosing `try` statement that can handle the exception.</span></span> <span data-ttu-id="82992-501">从点到点将控制转移到合适的异常处理程序所引发的异常发生过程被称为***异常传播***。</span><span class="sxs-lookup"><span data-stu-id="82992-501">The process that takes place from the point of the exception being thrown to the point of transferring control to a suitable exception handler is known as ***exception propagation***.</span></span> <span data-ttu-id="82992-502">传播异常包含的重复计算以下步骤，直到`catch`找到匹配异常的子句。</span><span class="sxs-lookup"><span data-stu-id="82992-502">Propagation of an exception consists of repeatedly evaluating the following steps until a `catch` clause that matches the exception is found.</span></span> <span data-ttu-id="82992-503">在此描述中，***引发点***是最初引发异常时的位置。</span><span class="sxs-lookup"><span data-stu-id="82992-503">In this description, the ***throw point*** is initially the location at which the exception is thrown.</span></span>

*  <span data-ttu-id="82992-504">在当前的函数成员，每个`try`检查封闭引发点的语句。</span><span class="sxs-lookup"><span data-stu-id="82992-504">In the current function member, each `try` statement that encloses the throw point is examined.</span></span> <span data-ttu-id="82992-505">为每个语句`S`，从最里层开始`try`语句和结束的最外层`try`语句中，按以下步骤进行计算：</span><span class="sxs-lookup"><span data-stu-id="82992-505">For each statement `S`, starting with the innermost `try` statement and ending with the outermost `try` statement, the following steps are evaluated:</span></span>

   * <span data-ttu-id="82992-506">如果`try`块`S`封闭引发点，并且 S 有一个或多个`catch`子句，`catch`子句进行检查的出现顺序来查找异常，根据规则中指定一个合适的处理程序部分[try 语句](statements.md#the-try-statement)。</span><span class="sxs-lookup"><span data-stu-id="82992-506">If the `try` block of `S` encloses the throw point and if S has one or more `catch` clauses, the `catch` clauses are examined in order of appearance to locate a suitable handler for the exception, according to the rules specified in Section [The try statement](statements.md#the-try-statement).</span></span> <span data-ttu-id="82992-507">如果匹配`catch`所在的子句，可以通过将控制转移到块的完成异常传播`catch`子句。</span><span class="sxs-lookup"><span data-stu-id="82992-507">If a matching `catch` clause is located, the exception propagation is completed by transferring control to the block of that `catch` clause.</span></span>

   * <span data-ttu-id="82992-508">否则为如果`try`块或`catch`块`S`封闭引发点; 如果`S`具有`finally`块中，控制传输到`finally`块。</span><span class="sxs-lookup"><span data-stu-id="82992-508">Otherwise, if the `try` block or a `catch` block of `S` encloses the throw point and if `S` has a `finally` block, control is transferred to the `finally` block.</span></span> <span data-ttu-id="82992-509">如果`finally`块引发另一个异常，终止当前异常的处理。</span><span class="sxs-lookup"><span data-stu-id="82992-509">If the `finally` block throws another exception, processing of the current exception is terminated.</span></span> <span data-ttu-id="82992-510">否则为当控件到达的终结点时`finally`块中，继续执行当前异常的处理。</span><span class="sxs-lookup"><span data-stu-id="82992-510">Otherwise, when control reaches the end point of the `finally` block, processing of the current exception is continued.</span></span>

*  <span data-ttu-id="82992-511">如果在当前函数调用中未找到异常处理程序函数调用会终止，并出现以下情况之一：</span><span class="sxs-lookup"><span data-stu-id="82992-511">If an exception handler was not located in the current function invocation, the function invocation is terminated, and one of the following occurs:</span></span>

   * <span data-ttu-id="82992-512">如果当前函数，非异步上面是重复执行步骤函数的调用方使用对应于从中调用的函数成员的语句的引发点。</span><span class="sxs-lookup"><span data-stu-id="82992-512">If the current function is non-async, the steps above are repeated for the caller of the function with a throw point corresponding to the statement from which the function member was invoked.</span></span>

   * <span data-ttu-id="82992-513">如果当前函数，async 和返回任务的返回任务，如中所述将置于出现故障或已取消状态中记录异常[枚举器接口](classes.md#enumerator-interfaces)。</span><span class="sxs-lookup"><span data-stu-id="82992-513">If the current function is async and task-returning, the exception is recorded in the return task, which is put into a faulted or cancelled state as described in [Enumerator interfaces](classes.md#enumerator-interfaces).</span></span>

   * <span data-ttu-id="82992-514">Async 和返回 void 的当前函数时，当前线程的同步上下文收到通知，如中所述[可枚举接口](classes.md#enumerable-interfaces)。</span><span class="sxs-lookup"><span data-stu-id="82992-514">If the current function is async and void-returning, the synchronization context of the current thread is notified as described in [Enumerable interfaces](classes.md#enumerable-interfaces).</span></span>

*  <span data-ttu-id="82992-515">如果异常处理终止当前线程中的所有函数成员调用，表明线程没有处理程序异常，然后在线程本身就是终止。</span><span class="sxs-lookup"><span data-stu-id="82992-515">If the exception processing terminates all function member invocations in the current thread, indicating that the thread has no handler for the exception, then the thread is itself terminated.</span></span> <span data-ttu-id="82992-516">此类终止的影响是实现定义的。</span><span class="sxs-lookup"><span data-stu-id="82992-516">The impact of such termination is implementation-defined.</span></span>

## <a name="the-try-statement"></a><span data-ttu-id="82992-517">Try 语句</span><span class="sxs-lookup"><span data-stu-id="82992-517">The try statement</span></span>

<span data-ttu-id="82992-518">`try`语句提供了一种机制，用于捕获块的执行过程中发生的异常。</span><span class="sxs-lookup"><span data-stu-id="82992-518">The `try` statement provides a mechanism for catching exceptions that occur during execution of a block.</span></span> <span data-ttu-id="82992-519">此外，`try`语句提供了指定的块失去控制权时始终执行的代码的能力`try`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-519">Furthermore, the `try` statement provides the ability to specify a block of code that is always executed when control leaves the `try` statement.</span></span>

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

<span data-ttu-id="82992-520">有三个可能的形式`try`语句：</span><span class="sxs-lookup"><span data-stu-id="82992-520">There are three possible forms of `try` statements:</span></span>

*  <span data-ttu-id="82992-521">一个`try`块后跟一个或多个`catch`块。</span><span class="sxs-lookup"><span data-stu-id="82992-521">A `try` block followed by one or more `catch` blocks.</span></span>
*  <span data-ttu-id="82992-522">一个`try`块后跟`finally`块。</span><span class="sxs-lookup"><span data-stu-id="82992-522">A `try` block followed by a `finally` block.</span></span>
*  <span data-ttu-id="82992-523">一个`try`块后跟一个或多个`catch`块跟`finally`块。</span><span class="sxs-lookup"><span data-stu-id="82992-523">A `try` block followed by one or more `catch` blocks followed by a `finally` block.</span></span>

<span data-ttu-id="82992-524">当`catch`子句指定*exception_specifier*，类型必须是`System.Exception`，派生的类型`System.Exception`或具有的类型参数类型`System.Exception`（或其中的子类） 作为其有效基本类。</span><span class="sxs-lookup"><span data-stu-id="82992-524">When a `catch` clause specifies an *exception_specifier*, the type must be `System.Exception`, a type that derives from `System.Exception` or a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span>

<span data-ttu-id="82992-525">时`catch`子句同时指定*exception_specifier*与*标识符*、 一个***异常变量***声明的给定名称和类型。</span><span class="sxs-lookup"><span data-stu-id="82992-525">When a `catch` clause specifies both an *exception_specifier* with an *identifier*, an ***exception variable*** of the given name and type is declared.</span></span> <span data-ttu-id="82992-526">异常变量对应于局部变量，通过进行了扩展作用域`catch`子句。</span><span class="sxs-lookup"><span data-stu-id="82992-526">The exception variable corresponds to a local variable with a scope that extends over the `catch` clause.</span></span> <span data-ttu-id="82992-527">执行期间*exception_filter*并*块*，异常变量表示当前正在处理的异常。</span><span class="sxs-lookup"><span data-stu-id="82992-527">During execution of the *exception_filter* and *block*, the exception variable represents the exception currently being handled.</span></span> <span data-ttu-id="82992-528">为了明确赋值检查，异常变量被视为在其整个范围内明确赋值。</span><span class="sxs-lookup"><span data-stu-id="82992-528">For purposes of definite assignment checking, the exception variable is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="82992-529">除非`catch`子句包括了异常的变量名称，就无法访问在筛选器中的异常对象和`catch`块。</span><span class="sxs-lookup"><span data-stu-id="82992-529">Unless a `catch` clause includes an exception variable name, it is impossible to access the exception object in the filter and `catch` block.</span></span>

<span data-ttu-id="82992-530">一个`catch`未指定子句*exception_specifier*称为常规`catch`子句。</span><span class="sxs-lookup"><span data-stu-id="82992-530">A `catch` clause that does not specify an *exception_specifier* is called a general `catch` clause.</span></span>

<span data-ttu-id="82992-531">某些编程语言可能支持不是可表示为一个派生自的异常`System.Exception`，尽管 C# 代码可能永远不会生成此类异常。</span><span class="sxs-lookup"><span data-stu-id="82992-531">Some programming languages may support exceptions that are not representable as an object derived from `System.Exception`, although such exceptions could never be generated by C# code.</span></span> <span data-ttu-id="82992-532">一种通用`catch`子句可用于捕获此类异常。</span><span class="sxs-lookup"><span data-stu-id="82992-532">A general `catch` clause may be used to catch such exceptions.</span></span> <span data-ttu-id="82992-533">因此，一种通用`catch`子句是从一个指定的类型在语义上不同`System.Exception`、 在于，前者可能也会捕获其他语言中的异常。</span><span class="sxs-lookup"><span data-stu-id="82992-533">Thus, a general `catch` clause is semantically different from one that specifies the type `System.Exception`, in that the former may also catch exceptions from other languages.</span></span>

<span data-ttu-id="82992-534">若要查找的处理程序异常，`catch`子句词法顺序进行检查。</span><span class="sxs-lookup"><span data-stu-id="82992-534">In order to locate a handler for an exception, `catch` clauses are examined in lexical order.</span></span> <span data-ttu-id="82992-535">如果`catch`子句指定某个类型，但没有异常筛选器，为更高版本是编译时错误`catch`在同一子句`try`语句指定的类型相同，或派生自的类型。</span><span class="sxs-lookup"><span data-stu-id="82992-535">If a `catch` clause specifies a type but no exception filter, it is a compile-time error for a later `catch` clause in the same `try` statement to specify a type that is the same as, or is derived from, that type.</span></span> <span data-ttu-id="82992-536">如果`catch`子句指定没有类型，并且任何筛选器，它必须是最后一个`catch`子句的`try`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-536">If a `catch` clause specifies no type and no filter, it must be the last `catch` clause for that `try` statement.</span></span>

<span data-ttu-id="82992-537">内`catch`块中，`throw`语句 ([throw 语句](statements.md#the-throw-statement)) 不包含表达式可用于重新引发异常的捕获了`catch`块。</span><span class="sxs-lookup"><span data-stu-id="82992-537">Within a `catch` block, a `throw` statement ([The throw statement](statements.md#the-throw-statement)) with no expression can be used to re-throw the exception that was caught by the `catch` block.</span></span> <span data-ttu-id="82992-538">对异常变量的赋值不会更改被重新引发的异常。</span><span class="sxs-lookup"><span data-stu-id="82992-538">Assignments to an exception variable do not alter the exception that is re-thrown.</span></span>

<span data-ttu-id="82992-539">在示例</span><span class="sxs-lookup"><span data-stu-id="82992-539">In the example</span></span>
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
<span data-ttu-id="82992-540">该方法`F`捕获异常、 一些诊断信息写入控制台，更改异常变量和重新引发的异常。</span><span class="sxs-lookup"><span data-stu-id="82992-540">the method `F` catches an exception, writes some diagnostic information to the console, alters the exception variable, and re-throws the exception.</span></span> <span data-ttu-id="82992-541">将重新引发的异常是原始异常，因此生成的输出为：</span><span class="sxs-lookup"><span data-stu-id="82992-541">The exception that is re-thrown is the original exception, so the output produced is:</span></span>
```
Exception in F: G
Exception in Main: G
```

<span data-ttu-id="82992-542">如果第一个 catch 块必须引发`e`而不是重新引发当前异常，生成的输出将如下：</span><span class="sxs-lookup"><span data-stu-id="82992-542">If the first catch block had thrown `e` instead of rethrowing the current exception, the output produced would be as follows:</span></span>
```csharp
Exception in F: G
Exception in Main: F
```

<span data-ttu-id="82992-543">它是编译时错误`break`， `continue`，或`goto`语句将控制转移出`finally`块。</span><span class="sxs-lookup"><span data-stu-id="82992-543">It is a compile-time error for a `break`, `continue`, or `goto` statement to transfer control out of a `finally` block.</span></span> <span data-ttu-id="82992-544">当`break`， `continue`，或`goto`语句中出现`finally`块中，语句的目标必须是在同一个`finally`块，否则会发生编译时错误或。</span><span class="sxs-lookup"><span data-stu-id="82992-544">When a `break`, `continue`, or `goto` statement occurs in a `finally` block, the target of the statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="82992-545">它是编译时错误`return`语句出现在`finally`块。</span><span class="sxs-lookup"><span data-stu-id="82992-545">It is a compile-time error for a `return` statement to occur in a `finally` block.</span></span>

<span data-ttu-id="82992-546">一个`try`执行语句，如下所示：</span><span class="sxs-lookup"><span data-stu-id="82992-546">A `try` statement is executed as follows:</span></span>

*  <span data-ttu-id="82992-547">控制转移到其中`try`块。</span><span class="sxs-lookup"><span data-stu-id="82992-547">Control is transferred to the `try` block.</span></span>
*  <span data-ttu-id="82992-548">如果控制到达的终结点`try`块：</span><span class="sxs-lookup"><span data-stu-id="82992-548">When and if control reaches the end point of the `try` block:</span></span>
   *  <span data-ttu-id="82992-549">如果`try`语句具有`finally`块中，`finally`执行块。</span><span class="sxs-lookup"><span data-stu-id="82992-549">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
   *  <span data-ttu-id="82992-550">控制转移到其中的终结点`try`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-550">Control is transferred to the end point of the `try` statement.</span></span>

*  <span data-ttu-id="82992-551">如果异常传播到`try`语句的执行期间`try`块：</span><span class="sxs-lookup"><span data-stu-id="82992-551">If an exception is propagated to the `try` statement during execution of the `try` block:</span></span>
   *  <span data-ttu-id="82992-552">`catch`子句，如果有的话，会按顺序检查的外观以找到合适的异常的处理程序。</span><span class="sxs-lookup"><span data-stu-id="82992-552">The `catch` clauses, if any, are examined in order of appearance to locate a suitable handler for the exception.</span></span> <span data-ttu-id="82992-553">如果`catch`子句未指定类型，或指定异常类型或基类型的异常类型：</span><span class="sxs-lookup"><span data-stu-id="82992-553">If a `catch` clause does not specify a type, or specifies the exception type or a base type of the exception type:</span></span>
      *  <span data-ttu-id="82992-554">如果`catch`子句声明了一个异常变量，异常对象分配到的异常变量。</span><span class="sxs-lookup"><span data-stu-id="82992-554">If the `catch` clause declares an exception variable, the exception object is assigned to the exception variable.</span></span>
      *  <span data-ttu-id="82992-555">如果`catch`子句声明了一个异常筛选器，则计算筛选器。</span><span class="sxs-lookup"><span data-stu-id="82992-555">If the `catch` clause declares an exception filter, the filter is evaluated.</span></span> <span data-ttu-id="82992-556">如果其计算结果为`false`，catch 子句没有匹配项，并且搜索工作一直持续到任何后续`catch`合适的处理程序的子句。</span><span class="sxs-lookup"><span data-stu-id="82992-556">If it evaluates to `false`, the catch clause is not a match, and the search continues through any subsequent `catch` clauses for a suitable handler.</span></span>
      *  <span data-ttu-id="82992-557">否则为`catch`子句被认为是匹配项，并控制转移到其中的匹配`catch`块。</span><span class="sxs-lookup"><span data-stu-id="82992-557">Otherwise, the `catch` clause is considered a match, and control is transferred to the matching `catch` block.</span></span>
      *  <span data-ttu-id="82992-558">如果控制到达的终结点`catch`块：</span><span class="sxs-lookup"><span data-stu-id="82992-558">When and if control reaches the end point of the `catch` block:</span></span>
         * <span data-ttu-id="82992-559">如果`try`语句具有`finally`块中，`finally`执行块。</span><span class="sxs-lookup"><span data-stu-id="82992-559">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         * <span data-ttu-id="82992-560">控制转移到其中的终结点`try`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-560">Control is transferred to the end point of the `try` statement.</span></span>
      *  <span data-ttu-id="82992-561">如果异常传播到`try`语句的执行期间`catch`块：</span><span class="sxs-lookup"><span data-stu-id="82992-561">If an exception is propagated to the `try` statement during execution of the `catch` block:</span></span>
         *  <span data-ttu-id="82992-562">如果`try`语句具有`finally`块中，`finally`执行块。</span><span class="sxs-lookup"><span data-stu-id="82992-562">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         *  <span data-ttu-id="82992-563">该异常传播到下一步封闭`try`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-563">The exception is propagated to the next enclosing `try` statement.</span></span>
   *  <span data-ttu-id="82992-564">如果`try`语句没有`catch`子句或如果没有`catch`子句与异常匹配：</span><span class="sxs-lookup"><span data-stu-id="82992-564">If the `try` statement has no `catch` clauses or if no `catch` clause matches the exception:</span></span>
      *  <span data-ttu-id="82992-565">如果`try`语句具有`finally`块中，`finally`执行块。</span><span class="sxs-lookup"><span data-stu-id="82992-565">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
      *  <span data-ttu-id="82992-566">该异常传播到下一步封闭`try`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-566">The exception is propagated to the next enclosing `try` statement.</span></span>

<span data-ttu-id="82992-567">语句`finally`失去控制权时始终执行块`try`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-567">The statements of a `finally` block are always executed when control leaves a `try` statement.</span></span> <span data-ttu-id="82992-568">这是 true 的控制转移是否由于正常执行，因为执行而发生`break`， `continue`， `goto`，或`return`语句，或由于传播出的异常而`try`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-568">This is true whether the control transfer occurs as a result of normal execution, as a result of executing a `break`, `continue`, `goto`, or `return` statement, or as a result of propagating an exception out of the `try` statement.</span></span>

<span data-ttu-id="82992-569">如果在执行期间引发异常`finally`阻止，且未被捕获在同一个 finally 块，该异常传播到下一步封闭`try`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-569">If an exception is thrown during execution of a `finally` block, and is not caught within the same finally block, the exception is propagated to the next enclosing `try` statement.</span></span> <span data-ttu-id="82992-570">如果另一个异常传播的过程中，该异常将丢失。</span><span class="sxs-lookup"><span data-stu-id="82992-570">If another exception was in the process of being propagated, that exception is lost.</span></span> <span data-ttu-id="82992-571">讨论传播异常的过程中的说明进一步`throw`语句 ([throw 语句](statements.md#the-throw-statement))。</span><span class="sxs-lookup"><span data-stu-id="82992-571">The process of propagating an exception is discussed further in the description of the `throw` statement ([The throw statement](statements.md#the-throw-statement)).</span></span>

<span data-ttu-id="82992-572">`try`块`try`是可到达的如果`try`语句是可访问。</span><span class="sxs-lookup"><span data-stu-id="82992-572">The `try` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="82992-573">一个`catch`块`try`是可到达的如果`try`语句是可访问。</span><span class="sxs-lookup"><span data-stu-id="82992-573">A `catch` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="82992-574">`finally`块`try`是可到达的如果`try`语句是可访问。</span><span class="sxs-lookup"><span data-stu-id="82992-574">The `finally` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="82992-575">终结点的`try`语句是可访问，如果下列两个条件成立：</span><span class="sxs-lookup"><span data-stu-id="82992-575">The end point of a `try` statement is reachable if both of the following are true:</span></span>

*  <span data-ttu-id="82992-576">终结点`try`块是可访问或结束点的至少一个`catch`块是可访问。</span><span class="sxs-lookup"><span data-stu-id="82992-576">The end point of the `try` block is reachable or the end point of at least one `catch` block is reachable.</span></span>
*  <span data-ttu-id="82992-577">如果`finally`块是否存在、 终结点的`finally`块是可访问。</span><span class="sxs-lookup"><span data-stu-id="82992-577">If a `finally` block is present, the end point of the `finally` block is reachable.</span></span>

## <a name="the-checked-and-unchecked-statements"></a><span data-ttu-id="82992-578">Checked 和 unchecked 语句</span><span class="sxs-lookup"><span data-stu-id="82992-578">The checked and unchecked statements</span></span>

<span data-ttu-id="82992-579">`checked`并`unchecked`语句用于控制***溢出检查上下文***整型类型算术运算和转换。</span><span class="sxs-lookup"><span data-stu-id="82992-579">The `checked` and `unchecked` statements are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

<span data-ttu-id="82992-580">`checked`语句将导致中的所有表达式*块*若要在 checked 上下文中计算和`unchecked`语句都导致中的所有表达式*块*中要计算未检查的上下文。</span><span class="sxs-lookup"><span data-stu-id="82992-580">The `checked` statement causes all expressions in the *block* to be evaluated in a checked context, and the `unchecked` statement causes all expressions in the *block* to be evaluated in an unchecked context.</span></span>

<span data-ttu-id="82992-581">`checked`并`unchecked`语句是恰好等同于`checked`并`unchecked`运算符 ([checked 和 unchecked 运算符](expressions.md#the-checked-and-unchecked-operators))，只不过它们对块而不是表达式进行操作.</span><span class="sxs-lookup"><span data-stu-id="82992-581">The `checked` and `unchecked` statements are precisely equivalent to the `checked` and `unchecked` operators ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), except that they operate on blocks instead of expressions.</span></span>

## <a name="the-lock-statement"></a><span data-ttu-id="82992-582">Lock 语句</span><span class="sxs-lookup"><span data-stu-id="82992-582">The lock statement</span></span>

<span data-ttu-id="82992-583">`lock`语句获取给定对象的互斥锁，执行语句，然后释放该锁。</span><span class="sxs-lookup"><span data-stu-id="82992-583">The `lock` statement obtains the mutual-exclusion lock for a given object, executes a statement, and then releases the lock.</span></span>

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

<span data-ttu-id="82992-584">表达式`lock`语句的已知类型的值必须属于*reference_type*。</span><span class="sxs-lookup"><span data-stu-id="82992-584">The expression of a `lock` statement must denote a value of a type known to be a *reference_type*.</span></span> <span data-ttu-id="82992-585">任何隐式装箱的转换 ([装箱转换](conversions.md#boxing-conversions)) 在单次执行的表达式执行`lock`语句，因此它是要表示的值的表达式的编译时错误*value_type*.</span><span class="sxs-lookup"><span data-stu-id="82992-585">No implicit boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)) is ever performed for the expression of a `lock` statement, and thus it is a compile-time error for the expression to denote a value of a *value_type*.</span></span>

<span data-ttu-id="82992-586">一个`lock`窗体的语句</span><span class="sxs-lookup"><span data-stu-id="82992-586">A `lock` statement of the form</span></span>
```csharp
lock (x) ...
```
<span data-ttu-id="82992-587">其中`x`的表达式*reference_type*，恰好等同于</span><span class="sxs-lookup"><span data-stu-id="82992-587">where `x` is an expression of a *reference_type*, is precisely equivalent to</span></span>
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
<span data-ttu-id="82992-588">不同的是 `x` 只计算一次。</span><span class="sxs-lookup"><span data-stu-id="82992-588">except that `x` is only evaluated once.</span></span>

<span data-ttu-id="82992-589">互斥锁被保留，而在同一执行线程中执行的代码还可以获取和释放锁。</span><span class="sxs-lookup"><span data-stu-id="82992-589">While a mutual-exclusion lock is held, code executing in the same execution thread can also obtain and release the lock.</span></span> <span data-ttu-id="82992-590">但是，在其他线程中执行的代码被无法获取锁，直到锁被释放。</span><span class="sxs-lookup"><span data-stu-id="82992-590">However, code executing in other threads is blocked from obtaining the lock until the lock is released.</span></span>

<span data-ttu-id="82992-591">锁定`System.Type`不建议以同步对静态数据的访问的对象。</span><span class="sxs-lookup"><span data-stu-id="82992-591">Locking `System.Type` objects in order to synchronize access to static data is not recommended.</span></span> <span data-ttu-id="82992-592">其他代码可能会锁定同一类型，可能会导致死锁。</span><span class="sxs-lookup"><span data-stu-id="82992-592">Other code might lock on the same type, which can result in deadlock.</span></span> <span data-ttu-id="82992-593">更好的方法是通过锁定私有静态对象同步对静态数据的访问。</span><span class="sxs-lookup"><span data-stu-id="82992-593">A better approach is to synchronize access to static data by locking a private static object.</span></span> <span data-ttu-id="82992-594">例如：</span><span class="sxs-lookup"><span data-stu-id="82992-594">For example:</span></span>
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

## <a name="the-using-statement"></a><span data-ttu-id="82992-595">using 语句</span><span class="sxs-lookup"><span data-stu-id="82992-595">The using statement</span></span>

<span data-ttu-id="82992-596">`using`语句获得一个或多个资源、 执行语句，然后释放资源。</span><span class="sxs-lookup"><span data-stu-id="82992-596">The `using` statement obtains one or more resources, executes a statement, and then disposes of the resource.</span></span>

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

<span data-ttu-id="82992-597">一个***资源***是类或结构实现`System.IDisposable`，其中包括一个名为的无参数方法`Dispose`。</span><span class="sxs-lookup"><span data-stu-id="82992-597">A ***resource*** is a class or struct that implements `System.IDisposable`, which includes a single parameterless method named `Dispose`.</span></span> <span data-ttu-id="82992-598">使用资源的代码可以调用`Dispose`来指示不再需要该资源。</span><span class="sxs-lookup"><span data-stu-id="82992-598">Code that is using a resource can call `Dispose` to indicate that the resource is no longer needed.</span></span> <span data-ttu-id="82992-599">如果`Dispose`未调用，则自动可供使用最终会出现由于垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="82992-599">If `Dispose` is not called, then automatic disposal eventually occurs as a consequence of garbage collection.</span></span>

<span data-ttu-id="82992-600">如果的窗体*resource_acquisition*是*local_variable_declaration*的类型*local_variable_declaration*必须是`dynamic`或类型可以隐式转换为`System.IDisposable`。</span><span class="sxs-lookup"><span data-stu-id="82992-600">If the form of *resource_acquisition* is *local_variable_declaration* then the type of the *local_variable_declaration* must be either `dynamic` or a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="82992-601">如果的窗体*resource_acquisition*是*表达式*则此表达式必须是隐式转换为`System.IDisposable`。</span><span class="sxs-lookup"><span data-stu-id="82992-601">If the form of *resource_acquisition* is *expression* then this expression must be implicitly convertible to `System.IDisposable`.</span></span>

<span data-ttu-id="82992-602">在中声明的局部变量*resource_acquisition*是只读的并且必须包含初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="82992-602">Local variables declared in a *resource_acquisition* are read-only, and must include an initializer.</span></span> <span data-ttu-id="82992-603">如果尝试修改这些本地变量嵌入的语句会发生编译时错误 (通过分配或`++`并`--`运算符)，需要它们的地址，或将它们作为传递`ref`或`out`参数。</span><span class="sxs-lookup"><span data-stu-id="82992-603">A compile-time error occurs if the embedded statement attempts to modify these local variables (via assignment or the `++` and `--` operators) , take the address of them, or pass them as `ref` or `out` parameters.</span></span>

<span data-ttu-id="82992-604">一个`using`语句转换为三个部分： 获取、 使用情况和可供使用。</span><span class="sxs-lookup"><span data-stu-id="82992-604">A `using` statement is translated into three parts: acquisition, usage, and disposal.</span></span> <span data-ttu-id="82992-605">对资源的使用隐式括`try`包含语句`finally`子句。</span><span class="sxs-lookup"><span data-stu-id="82992-605">Usage of the resource is implicitly enclosed in a `try` statement that includes a `finally` clause.</span></span> <span data-ttu-id="82992-606">这`finally`子句释放的资源。</span><span class="sxs-lookup"><span data-stu-id="82992-606">This `finally` clause disposes of the resource.</span></span> <span data-ttu-id="82992-607">如果`null`获取资源，则没有调用`Dispose`进行，并且不会引发异常。</span><span class="sxs-lookup"><span data-stu-id="82992-607">If a `null` resource is acquired, then no call to `Dispose` is made, and no exception is thrown.</span></span> <span data-ttu-id="82992-608">如果该资源的类型`dynamic`它动态地转换在隐式的动态转换 ([隐式动态转换](conversions.md#implicit-dynamic-conversions)) 到`IDisposable`期间为了确保转换的采集成功执行在使用情况和可供使用。</span><span class="sxs-lookup"><span data-stu-id="82992-608">If the resource is of type `dynamic` it is dynamically converted through an implicit dynamic conversion ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)) to `IDisposable` during acquisition in order to ensure that the conversion is successful before the usage and disposal.</span></span>

<span data-ttu-id="82992-609">一个`using`窗体的语句</span><span class="sxs-lookup"><span data-stu-id="82992-609">A `using` statement of the form</span></span>
```csharp
using (ResourceType resource = expression) statement
```
<span data-ttu-id="82992-610">对应于一个三个可能的扩展。</span><span class="sxs-lookup"><span data-stu-id="82992-610">corresponds to one of three possible expansions.</span></span> <span data-ttu-id="82992-611">当`ResourceType`是不可以为 null 的值类型，在扩展</span><span class="sxs-lookup"><span data-stu-id="82992-611">When `ResourceType` is a non-nullable value type, the expansion is</span></span>
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

<span data-ttu-id="82992-612">否则为当`ResourceType`而不是可以为 null 值类型还是引用类型`dynamic`，扩展</span><span class="sxs-lookup"><span data-stu-id="82992-612">Otherwise, when `ResourceType` is a nullable value type or a reference type other than `dynamic`, the expansion is</span></span>
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

<span data-ttu-id="82992-613">否则为当`ResourceType`是`dynamic`，扩展</span><span class="sxs-lookup"><span data-stu-id="82992-613">Otherwise, when `ResourceType` is `dynamic`, the expansion is</span></span>
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

<span data-ttu-id="82992-614">在任一扩展`resource`变量是只读中嵌入的语句和`d`变量到嵌入的语句是在中，不可访问，并且不可见。</span><span class="sxs-lookup"><span data-stu-id="82992-614">In either expansion, the `resource` variable is read-only in the embedded statement, and the `d` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="82992-615">实现允许以不同的方式，例如出于性能原因，实现给定的 using 语句，前提是与上述展开一致的行为。</span><span class="sxs-lookup"><span data-stu-id="82992-615">An implementation is permitted to implement a given using-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="82992-616">一个`using`窗体的语句</span><span class="sxs-lookup"><span data-stu-id="82992-616">A `using` statement of the form</span></span>
```csharp
using (expression) statement
```
<span data-ttu-id="82992-617">具有相同的三个可能的扩展。</span><span class="sxs-lookup"><span data-stu-id="82992-617">has the same three possible expansions.</span></span> <span data-ttu-id="82992-618">在这种情况下`ResourceType`是隐式的编译时类型`expression`，如果有的话。</span><span class="sxs-lookup"><span data-stu-id="82992-618">In this case `ResourceType` is implicitly the compile-time type of the `expression`, if it has one.</span></span> <span data-ttu-id="82992-619">否则为接口`IDisposable`本身用作`ResourceType`。</span><span class="sxs-lookup"><span data-stu-id="82992-619">Otherwise the interface `IDisposable` itself is used as the `ResourceType`.</span></span> <span data-ttu-id="82992-620">`resource`变量到嵌入的语句是在中，不可访问，并且不可见。</span><span class="sxs-lookup"><span data-stu-id="82992-620">The `resource` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="82992-621">当*resource_acquisition*采用的形式*local_variable_declaration*，可以获取给定类型的多个资源。</span><span class="sxs-lookup"><span data-stu-id="82992-621">When a *resource_acquisition* takes the form of a *local_variable_declaration*, it is possible to acquire multiple resources of a given type.</span></span> <span data-ttu-id="82992-622">一个`using`窗体的语句</span><span class="sxs-lookup"><span data-stu-id="82992-622">A `using` statement of the form</span></span>
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
<span data-ttu-id="82992-623">精确地等效于一系列嵌套`using`语句：</span><span class="sxs-lookup"><span data-stu-id="82992-623">is precisely equivalent to a sequence of nested `using` statements:</span></span>
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

<span data-ttu-id="82992-624">下面的示例创建名为的文件`log.txt`和两行文本写入文件。</span><span class="sxs-lookup"><span data-stu-id="82992-624">The example below creates a file named `log.txt` and writes two lines of text to the file.</span></span> <span data-ttu-id="82992-625">该示例然后打开该文件进行读取，并将所包含的文本行复制到控制台。</span><span class="sxs-lookup"><span data-stu-id="82992-625">The example then opens that same file for reading and copies the contained lines of text to the console.</span></span>
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

<span data-ttu-id="82992-626">由于`TextWriter`并`TextReader`类将实现`IDisposable`接口，该示例可以使用`using`语句，以确保基础文件正确关闭以下写入或读取操作。</span><span class="sxs-lookup"><span data-stu-id="82992-626">Since the `TextWriter` and `TextReader` classes implement the `IDisposable` interface, the example can use `using` statements to ensure that the underlying file is properly closed following the write or read operations.</span></span>

## <a name="the-yield-statement"></a><span data-ttu-id="82992-627">Yield 语句</span><span class="sxs-lookup"><span data-stu-id="82992-627">The yield statement</span></span>

<span data-ttu-id="82992-628">`yield`迭代器块中使用语句 ([块](statements.md#blocks)) 以生成枚举数对象值 ([枚举器对象](classes.md#enumerator-objects)) 或可枚举对象 ([可枚举对象](classes.md#enumerable-objects))迭代器，或在迭代结束的信号。</span><span class="sxs-lookup"><span data-stu-id="82992-628">The `yield` statement is used in an iterator block ([Blocks](statements.md#blocks)) to yield a value to the enumerator object ([Enumerator objects](classes.md#enumerator-objects)) or enumerable object ([Enumerable objects](classes.md#enumerable-objects)) of an iterator or to signal the end of the iteration.</span></span>

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

<span data-ttu-id="82992-629">`yield` 不是保留的字;它具有特殊含义仅当使用紧靠`return`或`break`关键字。</span><span class="sxs-lookup"><span data-stu-id="82992-629">`yield` is not a reserved word; it has special meaning only when used immediately before a `return` or `break` keyword.</span></span> <span data-ttu-id="82992-630">在其他上下文中`yield`可以用作标识符。</span><span class="sxs-lookup"><span data-stu-id="82992-630">In other contexts, `yield` can be used as an identifier.</span></span>

<span data-ttu-id="82992-631">在哪里有多个限制`yield`出现语句，如以下所述。</span><span class="sxs-lookup"><span data-stu-id="82992-631">There are several restrictions on where a `yield` statement can appear, as described in the following.</span></span>

*  <span data-ttu-id="82992-632">它是编译时错误`yield`语句 （包括两种形式） 显示之外*method_body*， *operator_body*或*accessor_body*</span><span class="sxs-lookup"><span data-stu-id="82992-632">It is a compile-time error for a `yield` statement (of either form) to appear outside a *method_body*, *operator_body* or *accessor_body*</span></span>
*  <span data-ttu-id="82992-633">它是编译时错误`yield`（的两种形式） 的语句才会出现在匿名函数内。</span><span class="sxs-lookup"><span data-stu-id="82992-633">It is a compile-time error for a `yield` statement (of either form) to appear inside an anonymous function.</span></span>
*  <span data-ttu-id="82992-634">它是编译时错误`yield`语句 （包括两种形式） 出现在`finally`子句的`try`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-634">It is a compile-time error for a `yield` statement (of either form) to appear in the `finally` clause of a `try` statement.</span></span>
*  <span data-ttu-id="82992-635">它是编译时错误`yield return`语句出现在任何位置`try`包含任何语句`catch`子句。</span><span class="sxs-lookup"><span data-stu-id="82992-635">It is a compile-time error for a `yield return` statement to appear anywhere in a `try` statement that contains any `catch` clauses.</span></span>

<span data-ttu-id="82992-636">下面的示例显示了一些有效和无效的用法`yield`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-636">The following example shows some valid and invalid uses of `yield` statements.</span></span>

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

<span data-ttu-id="82992-637">隐式转换 ([隐式转换](conversions.md#implicit-conversions)) 必须存在从类型中的表达式`yield return`语句产生类型 ([产生类型](classes.md#yield-type)) 的迭代器。</span><span class="sxs-lookup"><span data-stu-id="82992-637">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression in the `yield return` statement to the yield type ([Yield type](classes.md#yield-type)) of the iterator.</span></span>

<span data-ttu-id="82992-638">一个`yield return`执行语句，如下所示：</span><span class="sxs-lookup"><span data-stu-id="82992-638">A `yield return` statement is executed as follows:</span></span>

*  <span data-ttu-id="82992-639">在语句中给定的表达式是计算、 隐式转换为 yield 类型和分配给`Current`枚举器对象的属性。</span><span class="sxs-lookup"><span data-stu-id="82992-639">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
*  <span data-ttu-id="82992-640">挂起的迭代器块的执行。</span><span class="sxs-lookup"><span data-stu-id="82992-640">Execution of the iterator block is suspended.</span></span> <span data-ttu-id="82992-641">如果`yield return`语句是在一个或多个`try`阻止，关联`finally`块不会在这一次执行。</span><span class="sxs-lookup"><span data-stu-id="82992-641">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
*  <span data-ttu-id="82992-642">`MoveNext`的枚举器对象的方法将返回`true`，该值指示枚举器对象成功地推进到下一项，其调用方。</span><span class="sxs-lookup"><span data-stu-id="82992-642">The `MoveNext` method of the enumerator object returns `true` to its caller, indicating that the enumerator object successfully advanced to the next item.</span></span>

<span data-ttu-id="82992-643">枚举数对象的下一步调用`MoveNext`方法恢复执行迭代器块从上次已挂起。</span><span class="sxs-lookup"><span data-stu-id="82992-643">The next call to the enumerator object's `MoveNext` method resumes execution of the iterator block from where it was last suspended.</span></span>

<span data-ttu-id="82992-644">一个`yield break`执行语句，如下所示：</span><span class="sxs-lookup"><span data-stu-id="82992-644">A `yield break` statement is executed as follows:</span></span>

*  <span data-ttu-id="82992-645">如果`yield break`语句包含一个或多个`try`相关联的块`finally`块，控制最初为传输`finally`块的最内层`try`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-645">If the `yield break` statement is enclosed by one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="82992-646">如果控制到达的终结点`finally`块中，控制传输到`finally`的下一步封闭块`try`语句。</span><span class="sxs-lookup"><span data-stu-id="82992-646">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="82992-647">重复此过程，直到`finally`所有封闭的块`try`已执行语句。</span><span class="sxs-lookup"><span data-stu-id="82992-647">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="82992-648">控制权返回给调用方的迭代器块。</span><span class="sxs-lookup"><span data-stu-id="82992-648">Control is returned to the caller of the iterator block.</span></span> <span data-ttu-id="82992-649">这可以是`MoveNext`方法或`Dispose`枚举器对象的方法。</span><span class="sxs-lookup"><span data-stu-id="82992-649">This is either the `MoveNext` method or `Dispose` method of the enumerator object.</span></span>

<span data-ttu-id="82992-650">因为`yield break`语句无条件将控制转移到其他位置，终结点的`yield break`语句永远不会是可访问。</span><span class="sxs-lookup"><span data-stu-id="82992-650">Because a `yield break` statement unconditionally transfers control elsewhere, the end point of a `yield break` statement is never reachable.</span></span>
