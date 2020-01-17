---
ms.openlocfilehash: f61039abd6bd557ac0ea625e6aac1c8bafa57b02
ms.sourcegitcommit: e134bb7058e9848120b93b345f96d6ac0cb8c815
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/17/2020
ms.locfileid: "71704079"
---
# <a name="expressions"></a><span data-ttu-id="a1735-101">表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-101">Expressions</span></span>

<span data-ttu-id="a1735-102">表达式是运算符和操作数构成的序列。</span><span class="sxs-lookup"><span data-stu-id="a1735-102">An expression is a sequence of operators and operands.</span></span> <span data-ttu-id="a1735-103">本章定义语法、操作数和运算符的计算顺序，以及表达式的含义。</span><span class="sxs-lookup"><span data-stu-id="a1735-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

## <a name="expression-classifications"></a><span data-ttu-id="a1735-104">表达式分类</span><span class="sxs-lookup"><span data-stu-id="a1735-104">Expression classifications</span></span>

<span data-ttu-id="a1735-105">表达式分类为以下类别之一：</span><span class="sxs-lookup"><span data-stu-id="a1735-105">An expression is classified as one of the following:</span></span>

*  <span data-ttu-id="a1735-106">一个 值。</span><span class="sxs-lookup"><span data-stu-id="a1735-106">A value.</span></span> <span data-ttu-id="a1735-107">每个值都有关联的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-107">Every value has an associated type.</span></span>
*  <span data-ttu-id="a1735-108">一个变量。</span><span class="sxs-lookup"><span data-stu-id="a1735-108">A variable.</span></span> <span data-ttu-id="a1735-109">每个变量都有关联的类型，即变量的声明类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-109">Every variable has an associated type, namely the declared type of the variable.</span></span>
*  <span data-ttu-id="a1735-110">一个命名空间。</span><span class="sxs-lookup"><span data-stu-id="a1735-110">A namespace.</span></span> <span data-ttu-id="a1735-111">具有此分类的表达式只能作为*member_access* （[成员访问](expressions.md#member-access)）的左侧出现。</span><span class="sxs-lookup"><span data-stu-id="a1735-111">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="a1735-112">在任何其他上下文中，归类为命名空间的表达式会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>
*  <span data-ttu-id="a1735-113">一种类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-113">A type.</span></span> <span data-ttu-id="a1735-114">具有此分类的表达式只能作为*member_access* （[成员访问](expressions.md#member-access)）的左侧，或者作为 `as` 运算符（[as 运算符](expressions.md#the-as-operator)）、`is` 运算符（[is 运算符](expressions.md#the-is-operator)）或 `typeof` 运算符（[typeof 运算符](expressions.md#the-typeof-operator)）的操作数出现。</span><span class="sxs-lookup"><span data-stu-id="a1735-114">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)), or as an operand for the `as` operator ([The as operator](expressions.md#the-as-operator)), the `is` operator ([The is operator](expressions.md#the-is-operator)), or the `typeof` operator ([The typeof operator](expressions.md#the-typeof-operator)).</span></span> <span data-ttu-id="a1735-115">在其他任何上下文中，归类为类型的表达式会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>
*  <span data-ttu-id="a1735-116">一个方法组，它是从成员查找（[成员查找](expressions.md#member-lookup)）生成的一组重载方法。</span><span class="sxs-lookup"><span data-stu-id="a1735-116">A method group, which is a set of overloaded methods resulting from a member lookup ([Member lookup](expressions.md#member-lookup)).</span></span> <span data-ttu-id="a1735-117">方法组可以有一个关联的实例表达式和一个关联的类型参数列表。</span><span class="sxs-lookup"><span data-stu-id="a1735-117">A method group may have an associated instance expression and an associated type argument list.</span></span> <span data-ttu-id="a1735-118">调用实例方法时，实例表达式的计算结果将成为由 `this` （[此访问](expressions.md#this-access)）表示的实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-118">When an instance method is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="a1735-119">*Invocation_expression* （[调用表达式](expressions.md#invocation-expressions)）、 *delegate_creation_expression* （[委托创建表达式](expressions.md#delegate-creation-expressions)）和 is 运算符的左侧都允许使用方法组，并可将其隐式转换为兼容的委托类型（[方法组转换](conversions.md#method-group-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-119">A method group is permitted in an *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)) , a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) and as the left hand side of an is operator, and can be implicitly converted to a compatible delegate type ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="a1735-120">在其他任何上下文中，归类为方法组的表达式会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-120">In any other context, an expression classified as a method group causes a compile-time error.</span></span>
*  <span data-ttu-id="a1735-121">空文本。</span><span class="sxs-lookup"><span data-stu-id="a1735-121">A null literal.</span></span> <span data-ttu-id="a1735-122">具有此分类的表达式可隐式转换为引用类型或可以为 null 的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-122">An expression with this classification can be implicitly converted to a reference type or nullable type.</span></span>
*  <span data-ttu-id="a1735-123">匿名函数。</span><span class="sxs-lookup"><span data-stu-id="a1735-123">An anonymous function.</span></span> <span data-ttu-id="a1735-124">具有此分类的表达式可隐式转换为兼容的委托类型或表达式目录树类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-124">An expression with this classification can be implicitly converted to a compatible delegate type or expression tree type.</span></span>
*  <span data-ttu-id="a1735-125">属性访问。</span><span class="sxs-lookup"><span data-stu-id="a1735-125">A property access.</span></span> <span data-ttu-id="a1735-126">每个属性访问都有关联的类型，即属性的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-126">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="a1735-127">而且，属性访问可能具有关联的实例表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-127">Furthermore, a property access may have an associated instance expression.</span></span> <span data-ttu-id="a1735-128">调用实例属性访问的访问器（`get` 或 `set` 块）时，实例表达式的计算结果将成为 `this` （[此访问](expressions.md#this-access)）表示的实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-128">When an accessor (the `get` or `set` block) of an instance property access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="a1735-129">事件访问。</span><span class="sxs-lookup"><span data-stu-id="a1735-129">An event access.</span></span> <span data-ttu-id="a1735-130">每个事件访问都有关联的类型，即事件的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="a1735-131">而且，事件访问可能具有关联的实例表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-131">Furthermore, an event access may have an associated instance expression.</span></span> <span data-ttu-id="a1735-132">事件访问可能显示为 `+=` 和 `-=` 运算符（[事件分配](expressions.md#event-assignment)）的左操作数。</span><span class="sxs-lookup"><span data-stu-id="a1735-132">An event access may appear as the left hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="a1735-133">在其他任何上下文中，归类为事件访问的表达式会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>
*  <span data-ttu-id="a1735-134">索引器访问。</span><span class="sxs-lookup"><span data-stu-id="a1735-134">An indexer access.</span></span> <span data-ttu-id="a1735-135">每个索引器访问都有关联的类型，即索引器的元素类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-135">Every indexer access has an associated type, namely the element type of the indexer.</span></span> <span data-ttu-id="a1735-136">此外，索引器访问具有关联的实例表达式和关联的参数列表。</span><span class="sxs-lookup"><span data-stu-id="a1735-136">Furthermore, an indexer access has an associated instance expression and an associated argument list.</span></span> <span data-ttu-id="a1735-137">调用索引器访问的访问器（`get` 或 `set` 块）时，实例表达式的计算结果将成为 `this` （[此访问](expressions.md#this-access)）表示的实例，而参数列表的计算结果将成为调用的参数列表。</span><span class="sxs-lookup"><span data-stu-id="a1735-137">When an accessor (the `get` or `set` block) of an indexer access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)), and the result of evaluating the argument list becomes the parameter list of the invocation.</span></span>
*  <span data-ttu-id="a1735-138">无变化。</span><span class="sxs-lookup"><span data-stu-id="a1735-138">Nothing.</span></span> <span data-ttu-id="a1735-139">当表达式是 `void`的返回类型的方法的调用时，会发生这种情况。</span><span class="sxs-lookup"><span data-stu-id="a1735-139">This occurs when the expression is an invocation of a method with a return type of `void`.</span></span> <span data-ttu-id="a1735-140">归类为 nothing 的表达式仅在*statement_expression* （[expression 语句](statements.md#expression-statements)）的上下文中有效。</span><span class="sxs-lookup"><span data-stu-id="a1735-140">An expression classified as nothing is only valid in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

<span data-ttu-id="a1735-141">表达式的最终结果绝不会是命名空间、类型、方法组或事件访问。</span><span class="sxs-lookup"><span data-stu-id="a1735-141">The final result of an expression is never a namespace, type, method group, or event access.</span></span> <span data-ttu-id="a1735-142">相反，如上所述，这些类别的表达式是仅在某些上下文中允许的中间构造。</span><span class="sxs-lookup"><span data-stu-id="a1735-142">Rather, as noted above, these categories of expressions are intermediate constructs that are only permitted in certain contexts.</span></span>

<span data-ttu-id="a1735-143">通过执行*get 访问器*或*set 访问器*的调用，始终可以将属性访问或索引器访问重新分类为值。</span><span class="sxs-lookup"><span data-stu-id="a1735-143">A property access or indexer access is always reclassified as a value by performing an invocation of the *get accessor* or the *set accessor*.</span></span> <span data-ttu-id="a1735-144">特定访问器由属性或索引器访问的上下文确定：如果访问是赋值的目标，则调用*set 访问器*来分配新值（[简单赋值](expressions.md#simple-assignment)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-144">The particular accessor is determined by the context of the property or indexer access: If the access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="a1735-145">否则，将调用*get 访问器*以获取当前值（[表达式的值](expressions.md#values-of-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-145">Otherwise, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="values-of-expressions"></a><span data-ttu-id="a1735-146">表达式的值</span><span class="sxs-lookup"><span data-stu-id="a1735-146">Values of expressions</span></span>

<span data-ttu-id="a1735-147">大多数涉及表达式的构造都需要表达式来表示***值***。</span><span class="sxs-lookup"><span data-stu-id="a1735-147">Most of the constructs that involve an expression ultimately require the expression to denote a ***value***.</span></span> <span data-ttu-id="a1735-148">在这种情况下，如果实际表达式表示命名空间、类型、方法组或不执行任何操作，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-148">In such cases, if the actual expression denotes a namespace, a type, a method group, or nothing, a compile-time error occurs.</span></span> <span data-ttu-id="a1735-149">但是，如果表达式表示属性访问、索引器访问或变量，则将隐式替换属性、索引器或变量的值：</span><span class="sxs-lookup"><span data-stu-id="a1735-149">However, if the expression denotes a property access, an indexer access, or a variable, the value of the property, indexer, or variable is implicitly substituted:</span></span>

*  <span data-ttu-id="a1735-150">变量的值只是当前存储在变量标识的存储位置中的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-150">The value of a variable is simply the value currently stored in the storage location identified by the variable.</span></span> <span data-ttu-id="a1735-151">必须先将变量视为明确赋值（[明确赋值](variables.md#definite-assignment)），才能获得其值，否则将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-151">A variable must be considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained, or otherwise a compile-time error occurs.</span></span>
*  <span data-ttu-id="a1735-152">属性访问表达式的值是通过调用属性的*get 访问器*获取的。</span><span class="sxs-lookup"><span data-stu-id="a1735-152">The value of a property access expression is obtained by invoking the *get accessor* of the property.</span></span> <span data-ttu-id="a1735-153">如果该属性没有*get 访问器*，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-153">If the property has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="a1735-154">否则，将执行函数成员调用（对[动态重载决策进行编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)），并且调用的结果将成为属性访问表达式的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-154">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed, and the result of the invocation becomes the value of the property access expression.</span></span>
*  <span data-ttu-id="a1735-155">索引器访问表达式的值是通过调用索引器的*get 访问器*获取的。</span><span class="sxs-lookup"><span data-stu-id="a1735-155">The value of an indexer access expression is obtained by invoking the *get accessor* of the indexer.</span></span> <span data-ttu-id="a1735-156">如果索引器没有*get 访问器*，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-156">If the indexer has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="a1735-157">否则，将使用与索引器访问表达式关联的参数列表来执行函数成员调用（[动态重载决策的编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)），并且调用的结果将成为索引器访问表达式的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-157">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed with the argument list associated with the indexer access expression, and the result of the invocation becomes the value of the indexer access expression.</span></span>

## <a name="static-and-dynamic-binding"></a><span data-ttu-id="a1735-158">静态和动态绑定</span><span class="sxs-lookup"><span data-stu-id="a1735-158">Static and Dynamic Binding</span></span>

<span data-ttu-id="a1735-159">根据构成表达式的类型或值（参数、操作数、接收方）确定操作含义的过程通常称为 "***绑定***"。</span><span class="sxs-lookup"><span data-stu-id="a1735-159">The process of determining the meaning of an operation based on the type or value of constituent expressions (arguments, operands, receivers) is often referred to as ***binding***.</span></span> <span data-ttu-id="a1735-160">例如，方法调用的含义是根据接收方和参数的类型确定的。</span><span class="sxs-lookup"><span data-stu-id="a1735-160">For instance the meaning of a method call is determined based on the type of the receiver and arguments.</span></span> <span data-ttu-id="a1735-161">运算符的含义取决于其操作数的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-161">The meaning of an operator is determined based on the type of its operands.</span></span>

<span data-ttu-id="a1735-162">在C#编译时，通常基于其构成表达式的编译时类型确定操作的含义。</span><span class="sxs-lookup"><span data-stu-id="a1735-162">In C# the meaning of an operation is usually determined at compile-time, based on the compile-time type of its constituent expressions.</span></span> <span data-ttu-id="a1735-163">同样，如果表达式包含错误，编译器将检测并报告错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-163">Likewise, if an expression contains an error, the error is detected and reported by the compiler.</span></span> <span data-ttu-id="a1735-164">此方法称为***静态绑定***。</span><span class="sxs-lookup"><span data-stu-id="a1735-164">This approach is known as ***static binding***.</span></span>

<span data-ttu-id="a1735-165">但是，如果表达式是动态表达式（即类型为 `dynamic`），则表示它参与的任何绑定都应基于其运行时类型（即，它在运行时表示的对象的实际类型），而不是它在编译时所具有的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-165">However, if an expression is a dynamic expression (i.e. has the type `dynamic`) this indicates that any binding that it participates in should be based on its run-time type (i.e. the actual type of the object it denotes at run-time) rather than the type it has at compile-time.</span></span> <span data-ttu-id="a1735-166">这样，此类操作的绑定就会推迟到运行该程序的时间。</span><span class="sxs-lookup"><span data-stu-id="a1735-166">The binding of such an operation is therefore deferred until the time where the operation is to be executed during the running of the program.</span></span> <span data-ttu-id="a1735-167">这称为***动态绑定***。</span><span class="sxs-lookup"><span data-stu-id="a1735-167">This is referred to as ***dynamic binding***.</span></span>

<span data-ttu-id="a1735-168">当动态绑定操作时，编译器不会执行任何检查。</span><span class="sxs-lookup"><span data-stu-id="a1735-168">When an operation is dynamically bound, little or no checking is performed by the compiler.</span></span> <span data-ttu-id="a1735-169">相反，如果运行时绑定失败，则在运行时将错误报告为异常。</span><span class="sxs-lookup"><span data-stu-id="a1735-169">Instead if the run-time binding fails, errors are reported as exceptions at run-time.</span></span>

<span data-ttu-id="a1735-170">中C#的以下操作服从绑定：</span><span class="sxs-lookup"><span data-stu-id="a1735-170">The following operations in C# are subject to binding:</span></span>

*  <span data-ttu-id="a1735-171">成员访问： `e.M`</span><span class="sxs-lookup"><span data-stu-id="a1735-171">Member access: `e.M`</span></span>
*  <span data-ttu-id="a1735-172">方法调用： `e.M(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="a1735-172">Method invocation: `e.M(e1, ..., eN)`</span></span>
*  <span data-ttu-id="a1735-173">委托调用：`e(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="a1735-173">Delegate invocation:`e(e1, ..., eN)`</span></span>
*  <span data-ttu-id="a1735-174">元素访问： `e[e1, ..., eN]`</span><span class="sxs-lookup"><span data-stu-id="a1735-174">Element access: `e[e1, ..., eN]`</span></span>
*  <span data-ttu-id="a1735-175">对象创建： `new C(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="a1735-175">Object creation: `new C(e1, ..., eN)`</span></span>
*  <span data-ttu-id="a1735-176">重载的一元运算符： `+`、`-`、`!`、`~`、`++`、`--`、`true`、`false`</span><span class="sxs-lookup"><span data-stu-id="a1735-176">Overloaded unary operators: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span></span>
*  <span data-ttu-id="a1735-177">重载的二元运算符： `+`、`-`、`*`、`/`、`%`、`&`、`&&`、`|`、`||`、`??`、`^`、`<<`、`>>`、`==`、`!=`、`>`、`<`、`>=`、`<=`</span><span class="sxs-lookup"><span data-stu-id="a1735-177">Overloaded binary operators: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span></span>
*  <span data-ttu-id="a1735-178">赋值运算符： `=`、`+=`、`-=`、`*=`、`/=`、`%=`、`&=`、`|=`、`^=`、`<<=`、`>>=`</span><span class="sxs-lookup"><span data-stu-id="a1735-178">Assignment operators: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span></span>
*  <span data-ttu-id="a1735-179">隐式和显式转换</span><span class="sxs-lookup"><span data-stu-id="a1735-179">Implicit and explicit conversions</span></span>

<span data-ttu-id="a1735-180">如果不涉及动态表达式， C#则默认为静态绑定，这意味着在选择过程中使用构成表达式的编译时类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-180">When no dynamic expressions are involved, C# defaults to static binding, which means that the compile-time types of constituent expressions are used in the selection process.</span></span> <span data-ttu-id="a1735-181">但是，当上面列出的操作中的其中一个构成表达式为动态表达式时，将改为动态绑定该操作。</span><span class="sxs-lookup"><span data-stu-id="a1735-181">However, when one of the constituent expressions in the operations listed above is a dynamic expression, the operation is instead dynamically bound.</span></span>

### <a name="binding-time"></a><span data-ttu-id="a1735-182">绑定时间</span><span class="sxs-lookup"><span data-stu-id="a1735-182">Binding-time</span></span>

<span data-ttu-id="a1735-183">在编译时进行静态绑定，而动态绑定在运行时发生。</span><span class="sxs-lookup"><span data-stu-id="a1735-183">Static binding takes place at compile-time, whereas dynamic binding takes place at run-time.</span></span> <span data-ttu-id="a1735-184">在下面几节中，术语 "***绑定时间***" 指编译时或运行时，具体取决于绑定发生的时间。</span><span class="sxs-lookup"><span data-stu-id="a1735-184">In the following sections, the term ***binding-time*** refers to either compile-time or run-time, depending on when the binding takes place.</span></span>

<span data-ttu-id="a1735-185">下面的示例演示了静态和动态绑定的概念以及绑定时间：</span><span class="sxs-lookup"><span data-stu-id="a1735-185">The following example illustrates the notions of static and dynamic binding and of binding-time:</span></span>
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

<span data-ttu-id="a1735-186">前两个调用是静态绑定的：根据参数的编译时类型选取 `Console.WriteLine` 的重载。</span><span class="sxs-lookup"><span data-stu-id="a1735-186">The first two calls are statically bound: the overload of `Console.WriteLine` is picked based on the compile-time type of their argument.</span></span> <span data-ttu-id="a1735-187">因此，绑定时间为编译时。</span><span class="sxs-lookup"><span data-stu-id="a1735-187">Thus, the binding-time is compile-time.</span></span>

<span data-ttu-id="a1735-188">第三个调用是动态绑定的：根据参数的运行时类型选取 `Console.WriteLine` 的重载。</span><span class="sxs-lookup"><span data-stu-id="a1735-188">The third call is dynamically bound: the overload of `Console.WriteLine` is picked based on the run-time type of its argument.</span></span> <span data-ttu-id="a1735-189">出现这种情况的原因是，参数是动态表达式--它的编译时类型是 `dynamic`。</span><span class="sxs-lookup"><span data-stu-id="a1735-189">This happens because the argument is a dynamic expression -- its compile-time type is `dynamic`.</span></span> <span data-ttu-id="a1735-190">因此，第三次调用的绑定时间是运行时。</span><span class="sxs-lookup"><span data-stu-id="a1735-190">Thus, the binding-time for the third call is run-time.</span></span>

### <a name="dynamic-binding"></a><span data-ttu-id="a1735-191">动态绑定</span><span class="sxs-lookup"><span data-stu-id="a1735-191">Dynamic binding</span></span>

<span data-ttu-id="a1735-192">动态绑定的目的是允许C#程序与***动态对象***交互，即不遵循C#类型系统的常规规则的对象。</span><span class="sxs-lookup"><span data-stu-id="a1735-192">The purpose of dynamic binding is to allow C# programs to interact with ***dynamic objects***, i.e. objects that do not follow the normal rules of the C# type system.</span></span> <span data-ttu-id="a1735-193">动态对象可以是具有不同类型系统的其他编程语言中的对象，也可以是以编程方式设置以实现不同操作的绑定语义的对象。</span><span class="sxs-lookup"><span data-stu-id="a1735-193">Dynamic objects may be objects from other programming languages with different types systems, or they may be objects that are programmatically setup to implement their own binding semantics for different operations.</span></span>

<span data-ttu-id="a1735-194">动态对象实现其自身语义的机制是定义的实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-194">The mechanism by which a dynamic object implements its own semantics is implementation defined.</span></span> <span data-ttu-id="a1735-195">定义的给定接口再次实现--由动态对象实现，以向C#运行时发出信号，指示它们具有特殊语义。</span><span class="sxs-lookup"><span data-stu-id="a1735-195">A given interface -- again implementation defined -- is implemented by dynamic objects to signal to the C# run-time that they have special semantics.</span></span> <span data-ttu-id="a1735-196">因此，无论动态对象上的操作是动态绑定的，无论是在该文档中指定的C# ，而不是在本文档中指定的语义，都需要接管。</span><span class="sxs-lookup"><span data-stu-id="a1735-196">Thus, whenever operations on a dynamic object are dynamically bound, their own binding semantics, rather than those of C# as specified in this document, take over.</span></span>

<span data-ttu-id="a1735-197">尽管动态绑定的目的是允许与动态对象进行互操作， C#但允许动态绑定所有对象，无论它们是否为动态的。</span><span class="sxs-lookup"><span data-stu-id="a1735-197">While the purpose of dynamic binding is to allow interoperation with dynamic objects, C# allows dynamic binding on all objects, whether they are dynamic or not.</span></span> <span data-ttu-id="a1735-198">这允许动态对象的更平滑集成，因为它们的操作结果可能不是动态对象，而是在编译时对程序员来说是未知类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-198">This allows for a smoother integration of dynamic objects, as the results of operations on them may not themselves be dynamic objects, but are still of a type unknown to the programmer at compile-time.</span></span> <span data-ttu-id="a1735-199">此外，动态绑定还有助于消除容易出错的基于反射的代码，即使在没有任何对象是动态对象时也是如此。</span><span class="sxs-lookup"><span data-stu-id="a1735-199">Also dynamic binding can help eliminate error-prone reflection-based code even when no objects involved are dynamic objects.</span></span>

<span data-ttu-id="a1735-200">以下各节描述了在应用动态绑定时与语言中的每个构造完全相同的内容、所应用的编译时检查（如果有），以及编译时结果和表达式分类的定义。</span><span class="sxs-lookup"><span data-stu-id="a1735-200">The following sections describe for each construct in the language exactly when dynamic binding is applied, what compile time checking -- if any -- is applied, and what the compile-time result and expression classification is.</span></span>

### <a name="types-of-constituent-expressions"></a><span data-ttu-id="a1735-201">构成表达式的类型</span><span class="sxs-lookup"><span data-stu-id="a1735-201">Types of constituent expressions</span></span>

<span data-ttu-id="a1735-202">静态绑定操作时，构成表达式的类型（例如，接收方、参数、索引或操作数）始终被认为是该表达式的编译时类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-202">When an operation is statically bound, the type of a constituent expression (e.g. a receiver, an argument, an index or an operand) is always considered to be the compile-time type of that expression.</span></span>

<span data-ttu-id="a1735-203">动态绑定操作时，将根据构成表达式的编译时类型以不同的方式确定构成表达式的类型：</span><span class="sxs-lookup"><span data-stu-id="a1735-203">When an operation is dynamically bound, the type of a constituent expression is determined in different ways depending on the compile-time type of the constituent expression:</span></span>

*  <span data-ttu-id="a1735-204">编译时类型 `dynamic` 的构成表达式被视为在运行时使用表达式计算结果的实际值类型</span><span class="sxs-lookup"><span data-stu-id="a1735-204">A constituent expression of compile-time type `dynamic` is considered to have the type of the actual value that the expression evaluates to at runtime</span></span>
*  <span data-ttu-id="a1735-205">在运行时将其编译时类型为类型参数的构成表达式视为具有类型参数绑定到的类型</span><span class="sxs-lookup"><span data-stu-id="a1735-205">A constituent expression whose compile-time type is a type parameter is considered to have the type which the type parameter is bound to at runtime</span></span>
*  <span data-ttu-id="a1735-206">否则，构成表达式被视为具有其编译时类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-206">Otherwise the constituent expression is considered to have its compile-time type.</span></span>

## <a name="operators"></a><span data-ttu-id="a1735-207">运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-207">Operators</span></span>

<span data-ttu-id="a1735-208">表达式是从***操作数***和***运算符***构造的。</span><span class="sxs-lookup"><span data-stu-id="a1735-208">Expressions are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="a1735-209">表达式的运算符指明了向操作数应用的运算。</span><span class="sxs-lookup"><span data-stu-id="a1735-209">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="a1735-210">运算符的示例包括 `+`、`-`、`*`、`/` 和 `new`。</span><span class="sxs-lookup"><span data-stu-id="a1735-210">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="a1735-211">操作数的示例包括文本、字段、局部变量和表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-211">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="a1735-212">有三种类型的运算符：</span><span class="sxs-lookup"><span data-stu-id="a1735-212">There are three kinds of operators:</span></span>

*  <span data-ttu-id="a1735-213">一元运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-213">Unary operators.</span></span> <span data-ttu-id="a1735-214">一元运算符采用一个操作数，并使用前缀表示法（如 `--x`）或后缀表示法（如 `x++`）。</span><span class="sxs-lookup"><span data-stu-id="a1735-214">The unary operators take one operand and use either prefix notation (such as `--x`) or postfix notation (such as `x++`).</span></span>
*  <span data-ttu-id="a1735-215">二元运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-215">Binary operators.</span></span> <span data-ttu-id="a1735-216">二元运算符采用两个操作数，并使用中缀表示法（如 `x + y`）。</span><span class="sxs-lookup"><span data-stu-id="a1735-216">The binary operators take two operands and all use infix notation (such as `x + y`).</span></span>
*  <span data-ttu-id="a1735-217">三元运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-217">Ternary operator.</span></span> <span data-ttu-id="a1735-218">只存在一个三元运算符 `?:`;它采用三个操作数，并使用中缀表示法（`c ? x : y`）。</span><span class="sxs-lookup"><span data-stu-id="a1735-218">Only one ternary operator, `?:`, exists; it takes three operands and uses infix notation (`c ? x : y`).</span></span>

<span data-ttu-id="a1735-219">表达式中运算符的计算顺序由运算符的***优先级***和***关联***性（[运算符优先级和结合](expressions.md#operator-precedence-and-associativity)性）决定。</span><span class="sxs-lookup"><span data-stu-id="a1735-219">The order of evaluation of operators in an expression is determined by the ***precedence*** and ***associativity*** of the operators ([Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)).</span></span>

<span data-ttu-id="a1735-220">表达式中的操作数从左到右进行计算。</span><span class="sxs-lookup"><span data-stu-id="a1735-220">Operands in an expression are evaluated from left to right.</span></span> <span data-ttu-id="a1735-221">例如，在 `F(i) + G(i++) * H(i)`中，使用旧值 `i`调用方法 `F`，然后使用旧值 `i`调用方法 `G`，最后，将使用新值 `H` 调用方法 `i`。</span><span class="sxs-lookup"><span data-stu-id="a1735-221">For example, in `F(i) + G(i++) * H(i)`, method `F` is called using the old value of `i`, then method `G` is called with the old value of `i`, and, finally, method `H` is called with the new value of `i`.</span></span> <span data-ttu-id="a1735-222">这与运算符优先级不同。</span><span class="sxs-lookup"><span data-stu-id="a1735-222">This is separate from and unrelated to operator precedence.</span></span>

<span data-ttu-id="a1735-223">特定的运算符可***重载***。</span><span class="sxs-lookup"><span data-stu-id="a1735-223">Certain operators can be ***overloaded***.</span></span> <span data-ttu-id="a1735-224">运算符重载允许为操作指定用户定义的运算符实现，其中一个或两个操作数属于用户定义的类或结构类型（[运算符重载](expressions.md#operator-overloading)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-224">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type ([Operator overloading](expressions.md#operator-overloading)).</span></span>

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="a1735-225">运算符优先级和关联性</span><span class="sxs-lookup"><span data-stu-id="a1735-225">Operator precedence and associativity</span></span>

<span data-ttu-id="a1735-226">如果表达式包含多个运算符，那么运算符的***优先级***决定了各个运算符的计算顺序。</span><span class="sxs-lookup"><span data-stu-id="a1735-226">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="a1735-227">例如，表达式 `x + y * z` 的计算结果为 `x + (y * z)`，因为 `*` 运算符的优先级高于 binary `+` 运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-227">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the binary `+` operator.</span></span> <span data-ttu-id="a1735-228">运算符的优先级由其关联的语法生产的定义来确定。</span><span class="sxs-lookup"><span data-stu-id="a1735-228">The precedence of an operator is established by the definition of its associated grammar production.</span></span> <span data-ttu-id="a1735-229">例如， *additive_expression*由 `+` 或 `-` 运算符分隔的一系列*multiplicative_expression*组成，从而使 `+` 和 `-` 运算符的优先级低于 `*`、`/`和 `%` 运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-229">For example, an *additive_expression* consists of a sequence of *multiplicative_expression*s separated by `+` or `-` operators, thus giving the `+` and `-` operators lower precedence than the `*`, `/`, and `%` operators.</span></span>

<span data-ttu-id="a1735-230">下表按优先级从高到低的顺序汇总了所有运算符：</span><span class="sxs-lookup"><span data-stu-id="a1735-230">The following table summarizes all operators in order of precedence from highest to lowest:</span></span>

| <span data-ttu-id="a1735-231">__Section__</span><span class="sxs-lookup"><span data-stu-id="a1735-231">__Section__</span></span>                                                                                   | <span data-ttu-id="a1735-232">__类别__</span><span class="sxs-lookup"><span data-stu-id="a1735-232">__Category__</span></span>                | <span data-ttu-id="a1735-233">__运算符__</span><span class="sxs-lookup"><span data-stu-id="a1735-233">__Operators__</span></span> | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [<span data-ttu-id="a1735-234">主要表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-234">Primary expressions</span></span>](expressions.md#primary-expressions)                                     | <span data-ttu-id="a1735-235">Primary</span><span class="sxs-lookup"><span data-stu-id="a1735-235">Primary</span></span>                     | <span data-ttu-id="a1735-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span><span class="sxs-lookup"><span data-stu-id="a1735-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span></span> | 
| [<span data-ttu-id="a1735-237">一元运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-237">Unary operators</span></span>](expressions.md#unary-operators)                                             | <span data-ttu-id="a1735-238">一元</span><span class="sxs-lookup"><span data-stu-id="a1735-238">Unary</span></span>                       | <span data-ttu-id="a1735-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span><span class="sxs-lookup"><span data-stu-id="a1735-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span></span> | 
| [<span data-ttu-id="a1735-240">算术运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-240">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="a1735-241">乘法</span><span class="sxs-lookup"><span data-stu-id="a1735-241">Multiplicative</span></span>              | <span data-ttu-id="a1735-242">`*`  `/`  `%`</span><span class="sxs-lookup"><span data-stu-id="a1735-242">`*`  `/`  `%`</span></span> | 
| [<span data-ttu-id="a1735-243">算术运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-243">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="a1735-244">加法</span><span class="sxs-lookup"><span data-stu-id="a1735-244">Additive</span></span>                    | <span data-ttu-id="a1735-245">`+`  `-`</span><span class="sxs-lookup"><span data-stu-id="a1735-245">`+`  `-`</span></span>      | 
| [<span data-ttu-id="a1735-246">移位运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-246">Shift operators</span></span>](expressions.md#shift-operators)                                             | <span data-ttu-id="a1735-247">移位</span><span class="sxs-lookup"><span data-stu-id="a1735-247">Shift</span></span>                       | <span data-ttu-id="a1735-248">`<<`  `>>`</span><span class="sxs-lookup"><span data-stu-id="a1735-248">`<<`  `>>`</span></span>    | 
| [<span data-ttu-id="a1735-249">关系和类型测试运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-249">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="a1735-250">关系和类型测试</span><span class="sxs-lookup"><span data-stu-id="a1735-250">Relational and type testing</span></span> | <span data-ttu-id="a1735-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span><span class="sxs-lookup"><span data-stu-id="a1735-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span></span> | 
| [<span data-ttu-id="a1735-252">关系和类型测试运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-252">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="a1735-253">相等</span><span class="sxs-lookup"><span data-stu-id="a1735-253">Equality</span></span>                    | <span data-ttu-id="a1735-254">`==`  `!=`</span><span class="sxs-lookup"><span data-stu-id="a1735-254">`==`  `!=`</span></span>    | 
| [<span data-ttu-id="a1735-255">逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-255">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="a1735-256">逻辑“与”</span><span class="sxs-lookup"><span data-stu-id="a1735-256">Logical AND</span></span>                 | `&`           | 
| [<span data-ttu-id="a1735-257">逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-257">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="a1735-258">逻辑 XOR</span><span class="sxs-lookup"><span data-stu-id="a1735-258">Logical XOR</span></span>                 | `^`           | 
| [<span data-ttu-id="a1735-259">逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-259">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="a1735-260">逻辑“或”</span><span class="sxs-lookup"><span data-stu-id="a1735-260">Logical OR</span></span>                  | <code>&#124;</code>           |
| [<span data-ttu-id="a1735-261">条件逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-261">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="a1735-262">条件“与”</span><span class="sxs-lookup"><span data-stu-id="a1735-262">Conditional AND</span></span>             | `&&`          | 
| [<span data-ttu-id="a1735-263">条件逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-263">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="a1735-264">条件“或”</span><span class="sxs-lookup"><span data-stu-id="a1735-264">Conditional OR</span></span>              | <code>&#124;&#124;</code>          | 
| [<span data-ttu-id="a1735-265">Null 合并运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-265">The null coalescing operator</span></span>](expressions.md#the-null-coalescing-operator)                   | <span data-ttu-id="a1735-266">null 合并</span><span class="sxs-lookup"><span data-stu-id="a1735-266">Null coalescing</span></span>             | `??`          | 
| [<span data-ttu-id="a1735-267">条件运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-267">Conditional operator</span></span>](expressions.md#conditional-operator)                                   | <span data-ttu-id="a1735-268">条件</span><span class="sxs-lookup"><span data-stu-id="a1735-268">Conditional</span></span>                 | `?:`          | 
| <span data-ttu-id="a1735-269">[赋值运算符](expressions.md#assignment-operators)，[匿名函数表达式](expressions.md#anonymous-function-expressions)</span><span class="sxs-lookup"><span data-stu-id="a1735-269">[Assignment operators](expressions.md#assignment-operators), [Anonymous function expressions](expressions.md#anonymous-function-expressions)</span></span>  | <span data-ttu-id="a1735-270">赋值和 lambda 表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-270">Assignment and lambda expression</span></span> | <span data-ttu-id="a1735-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span><span class="sxs-lookup"><span data-stu-id="a1735-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span></span> | 

<span data-ttu-id="a1735-272">如果两个运算符在优先级相同的两个运算符之间出现，则运算符的关联性将控制运算的执行顺序：</span><span class="sxs-lookup"><span data-stu-id="a1735-272">When an operand occurs between two operators with the same precedence, the associativity of the operators controls the order in which the operations are performed:</span></span>

*  <span data-ttu-id="a1735-273">除了赋值运算符和 null 合并运算符外，所有二元运算符都是***左结合***运算符，这意味着运算从左至右执行。</span><span class="sxs-lookup"><span data-stu-id="a1735-273">Except for the assignment operators and the null coalescing operator, all binary operators are ***left-associative***, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="a1735-274">例如， `x + y + z` 将计算为 `(x + y) + z`。</span><span class="sxs-lookup"><span data-stu-id="a1735-274">For example, `x + y + z` is evaluated as `(x + y) + z`.</span></span>
*  <span data-ttu-id="a1735-275">赋值运算符、null 合并运算符和条件运算符（`?:`）是***右结合***运算符，这意味着运算从右到左执行。</span><span class="sxs-lookup"><span data-stu-id="a1735-275">The assignment operators, the null coalescing operator and the conditional operator (`?:`) are ***right-associative***, meaning that operations are performed from right to left.</span></span> <span data-ttu-id="a1735-276">例如， `x = y = z` 将计算为 `x = (y = z)`。</span><span class="sxs-lookup"><span data-stu-id="a1735-276">For example, `x = y = z` is evaluated as `x = (y = z)`.</span></span>

<span data-ttu-id="a1735-277">可以使用括号控制优先级和结合性。</span><span class="sxs-lookup"><span data-stu-id="a1735-277">Precedence and associativity can be controlled using parentheses.</span></span> <span data-ttu-id="a1735-278">例如，`x + y * z` 先计算 `y` 乘 `z`，并将结果与 `x` 相加，而 `(x + y) * z` 则先计算 `x` 加 `y`，然后将结果与 `z` 相乘。</span><span class="sxs-lookup"><span data-stu-id="a1735-278">For example, `x + y * z` first multiplies `y` by `z` and then adds the result to `x`, but `(x + y) * z` first adds `x` and `y` and then multiplies the result by `z`.</span></span>

### <a name="operator-overloading"></a><span data-ttu-id="a1735-279">运算符重载</span><span class="sxs-lookup"><span data-stu-id="a1735-279">Operator overloading</span></span>

<span data-ttu-id="a1735-280">所有一元运算符和二元运算符都具有在任何表达式中自动可用的预定义实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-280">All unary and binary operators have predefined implementations that are automatically available in any expression.</span></span> <span data-ttu-id="a1735-281">除了预定义的实现外，还可以通过在类和结构（[运算符](classes.md#operators)）中包括 `operator` 声明来引入用户定义的实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-281">In addition to the predefined implementations, user-defined implementations can be introduced by including `operator` declarations in classes and structs ([Operators](classes.md#operators)).</span></span> <span data-ttu-id="a1735-282">用户定义的运算符实现始终优先于预定义运算符实现：只有当不存在适用的用户定义运算符实现时，才会考虑预定义运算符实现，如[一元运算符重载决策](expressions.md#unary-operator-overload-resolution)和[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)中所述。</span><span class="sxs-lookup"><span data-stu-id="a1735-282">User-defined operator implementations always take precedence over predefined operator implementations: Only when no applicable user-defined operator implementations exist will the predefined operator implementations be considered, as described in [Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution).</span></span>

<span data-ttu-id="a1735-283">可***重载的一元运算符***是：</span><span class="sxs-lookup"><span data-stu-id="a1735-283">The ***overloadable unary operators*** are:</span></span>
```csharp
+   -   !   ~   ++   --   true   false
```

<span data-ttu-id="a1735-284">尽管 `true` 和 `false` 并不显式在表达式中使用（因此它们不会以[运算符优先级和相关性](expressions.md#operator-precedence-and-associativity)的优先级表的顺序包括在内），但是它们被视为运算符，因为它们是在多个表达式上下文中调用的：布尔表达式（[布尔表达式](expressions.md#boolean-expressions)）和涉及条件[运算符](expressions.md#conditional-operator)的表达式（条件[逻辑](expressions.md#conditional-logical-operators)运算符）。</span><span class="sxs-lookup"><span data-stu-id="a1735-284">Although `true` and `false` are not used explicitly in expressions (and therefore are not included in the precedence table in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)), they are considered operators because they are invoked in several expression contexts: boolean expressions ([Boolean expressions](expressions.md#boolean-expressions)) and expressions involving the conditional ([Conditional operator](expressions.md#conditional-operator)), and conditional logical operators ([Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span>

<span data-ttu-id="a1735-285">可***重载的二元运算符***是：</span><span class="sxs-lookup"><span data-stu-id="a1735-285">The ***overloadable binary operators*** are:</span></span>
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

<span data-ttu-id="a1735-286">只能重载上面列出的运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-286">Only the operators listed above can be overloaded.</span></span> <span data-ttu-id="a1735-287">具体而言，无法重载成员访问、方法调用或 `=`、`&&`、`||`、`??`、`?:`、`=>`、`checked`、`unchecked`、`new`、`typeof`、`default`、`as`和 `is` 运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-287">In particular, it is not possible to overload member access, method invocation, or the `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, and `is` operators.</span></span>

<span data-ttu-id="a1735-288">重载二元运算符时，也会隐式重载相应的赋值运算符（若有）。</span><span class="sxs-lookup"><span data-stu-id="a1735-288">When a binary operator is overloaded, the corresponding assignment operator, if any, is also implicitly overloaded.</span></span> <span data-ttu-id="a1735-289">例如，运算符 `*` 的重载也是运算符 `*=`的重载。</span><span class="sxs-lookup"><span data-stu-id="a1735-289">For example, an overload of operator `*` is also an overload of operator `*=`.</span></span> <span data-ttu-id="a1735-290">这将在[复合赋值](expressions.md#compound-assignment)中进一步说明。</span><span class="sxs-lookup"><span data-stu-id="a1735-290">This is described further in [Compound assignment](expressions.md#compound-assignment).</span></span> <span data-ttu-id="a1735-291">请注意，赋值运算符本身（`=`）无法重载。</span><span class="sxs-lookup"><span data-stu-id="a1735-291">Note that the assignment operator itself (`=`) cannot be overloaded.</span></span> <span data-ttu-id="a1735-292">赋值始终将值的简单的逐位副本用于变量。</span><span class="sxs-lookup"><span data-stu-id="a1735-292">An assignment always performs a simple bit-wise copy of a value into a variable.</span></span>

<span data-ttu-id="a1735-293">强制转换操作（如 `(T)x`）是通过提供用户定义的转换（[用户定义的转换](conversions.md#user-defined-conversions)）而重载的。</span><span class="sxs-lookup"><span data-stu-id="a1735-293">Cast operations, such as `(T)x`, are overloaded by providing user-defined conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

<span data-ttu-id="a1735-294">元素访问（如 `a[x]`）不被视为可重载的运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-294">Element access, such as `a[x]`, is not considered an overloadable operator.</span></span> <span data-ttu-id="a1735-295">而是通过索引器（[索引器](classes.md#indexers)）支持用户定义的索引。</span><span class="sxs-lookup"><span data-stu-id="a1735-295">Instead, user-defined indexing is supported through indexers ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="a1735-296">在表达式中，运算符是使用运算符表示法引用的，而在声明中，使用函数表示法引用运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-296">In expressions, operators are referenced using operator notation, and in declarations, operators are referenced using functional notation.</span></span> <span data-ttu-id="a1735-297">下表显示了一元运算符和二元运算符的运算符和函数表示法之间的关系。</span><span class="sxs-lookup"><span data-stu-id="a1735-297">The following table shows the relationship between operator and functional notations for unary and binary operators.</span></span> <span data-ttu-id="a1735-298">在第一个条目中， *op*表示任何可重载的一元前缀运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-298">In the first entry, *op* denotes any overloadable unary prefix operator.</span></span> <span data-ttu-id="a1735-299">在第二个条目中， *op*表示一元后缀 `++` 和 `--` 运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-299">In the second entry, *op* denotes the unary postfix `++` and `--` operators.</span></span> <span data-ttu-id="a1735-300">在第三个条目中， *op*表示任何可重载的二元运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-300">In the third entry, *op* denotes any overloadable binary operator.</span></span>


| <span data-ttu-id="a1735-301">__运算符表示法__</span><span class="sxs-lookup"><span data-stu-id="a1735-301">__Operator notation__</span></span> | <span data-ttu-id="a1735-302">__函数表示法__</span><span class="sxs-lookup"><span data-stu-id="a1735-302">__Functional notation__</span></span> |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

<span data-ttu-id="a1735-303">用户定义的运算符声明始终需要至少一个参数成为包含运算符声明的类或结构类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-303">User-defined operator declarations always require at least one of the parameters to be of the class or struct type that contains the operator declaration.</span></span> <span data-ttu-id="a1735-304">因此，用户定义的运算符不能与预定义的运算符具有相同的签名。</span><span class="sxs-lookup"><span data-stu-id="a1735-304">Thus, it is not possible for a user-defined operator to have the same signature as a predefined operator.</span></span>

<span data-ttu-id="a1735-305">用户定义的运算符声明不能修改运算符的语法、优先级或关联性。</span><span class="sxs-lookup"><span data-stu-id="a1735-305">User-defined operator declarations cannot modify the syntax, precedence, or associativity of an operator.</span></span> <span data-ttu-id="a1735-306">例如，`/` 运算符始终为二元运算符，始终具有在[运算符优先级和关联](expressions.md#operator-precedence-and-associativity)性中指定的优先级别，并且始终为左结合。</span><span class="sxs-lookup"><span data-stu-id="a1735-306">For example, the `/` operator is always a binary operator, always has the precedence level specified in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity), and is always left-associative.</span></span>

<span data-ttu-id="a1735-307">尽管用户定义的运算符可以执行 pleases 的任何计算，但强烈不建议使用生成的实现，而不是所需的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-307">While it is possible for a user-defined operator to perform any computation it pleases, implementations that produce results other than those that are intuitively expected are strongly discouraged.</span></span> <span data-ttu-id="a1735-308">例如，`operator ==` 的实现应比较两个操作数的相等性并返回相应的 `bool` 结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-308">For example, an implementation of `operator ==` should compare the two operands for equality and return an appropriate `bool` result.</span></span>

<span data-ttu-id="a1735-309">通过[条件逻辑运算符](expressions.md#conditional-logical-operators)对[主要表达式](expressions.md#primary-expressions)中的单个运算符进行的说明指定运算符的预定义实现以及适用于每个运算符的任何其他规则。</span><span class="sxs-lookup"><span data-stu-id="a1735-309">The descriptions of individual operators in [Primary expressions](expressions.md#primary-expressions) through [Conditional logical operators](expressions.md#conditional-logical-operators) specify the predefined implementations of the operators and any additional rules that apply to each operator.</span></span> <span data-ttu-id="a1735-310">这些说明使用术语 "***一元运算符重载解析***"、"***二元运算符重载解析***" 和 "***数值升级***"，其中包括以下各节。</span><span class="sxs-lookup"><span data-stu-id="a1735-310">The descriptions make use of the terms ***unary operator overload resolution***, ***binary operator overload resolution***, and ***numeric promotion***, definitions of which are found in the following sections.</span></span>

### <a name="unary-operator-overload-resolution"></a><span data-ttu-id="a1735-311">一元运算符重载决策</span><span class="sxs-lookup"><span data-stu-id="a1735-311">Unary operator overload resolution</span></span>

<span data-ttu-id="a1735-312">`op x` 或 `x op`形式的操作（其中 `op` 是一个可重载的一元运算符），并且 `x` 是 `X`类型的表达式，则按以下方式处理：</span><span class="sxs-lookup"><span data-stu-id="a1735-312">An operation of the form `op x` or `x op`, where `op` is an overloadable unary operator, and `x` is an expression of type `X`, is processed as follows:</span></span>

*  <span data-ttu-id="a1735-313">`X` 为操作 `operator op(x)` 提供的候选用户定义运算符集是使用[候选用户定义运算符](expressions.md#candidate-user-defined-operators)的规则确定的。</span><span class="sxs-lookup"><span data-stu-id="a1735-313">The set of candidate user-defined operators provided by `X` for the operation `operator op(x)` is determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span>
*  <span data-ttu-id="a1735-314">如果候选用户定义的运算符集不为空，则这将成为操作的候选运算符集。</span><span class="sxs-lookup"><span data-stu-id="a1735-314">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="a1735-315">否则，预定义的一元 `operator op` 实现（包括其提升形式）会成为操作的候选运算符集。</span><span class="sxs-lookup"><span data-stu-id="a1735-315">Otherwise, the predefined unary `operator op` implementations, including their lifted forms, become the set of candidate operators for the operation.</span></span> <span data-ttu-id="a1735-316">给定运算符的预定义实现在运算符的说明（[主表达式](expressions.md#primary-expressions)和[一元运算符](expressions.md#unary-operators)）中指定。</span><span class="sxs-lookup"><span data-stu-id="a1735-316">The predefined implementations of a given operator are specified in the description of the operator ([Primary expressions](expressions.md#primary-expressions) and [Unary operators](expressions.md#unary-operators)).</span></span>
*  <span data-ttu-id="a1735-317">[重载](expressions.md#overload-resolution)决策的重载决策规则应用于候选运算符集，以选择与 `(x)`参数列表相关的最佳运算符，而此运算符将成为重载决策过程的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-317">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="a1735-318">如果重载决策未能选择单个最佳运算符，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-318">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="binary-operator-overload-resolution"></a><span data-ttu-id="a1735-319">二元运算符重载决策</span><span class="sxs-lookup"><span data-stu-id="a1735-319">Binary operator overload resolution</span></span>

<span data-ttu-id="a1735-320">`x op y`格式的操作，其中 `op` 是可重载的二元运算符，`x` 是 `X`类型的表达式，而 `y` 是类型 `Y`的表达式，则按如下方式进行处理：</span><span class="sxs-lookup"><span data-stu-id="a1735-320">An operation of the form `x op y`, where `op` is an overloadable binary operator, `x` is an expression of type `X`, and `y` is an expression of type `Y`, is processed as follows:</span></span>

*  <span data-ttu-id="a1735-321">确定操作 `operator op(x,y)` `X` 和 `Y` 提供的候选用户定义运算符集。</span><span class="sxs-lookup"><span data-stu-id="a1735-321">The set of candidate user-defined operators provided by `X` and `Y` for the operation `operator op(x,y)` is determined.</span></span> <span data-ttu-id="a1735-322">该集由 `X` 提供的候选运算符与 `Y`提供的候选运算符联合组成，每个运算符都是使用[候选用户定义的运算符](expressions.md#candidate-user-defined-operators)的规则确定的。</span><span class="sxs-lookup"><span data-stu-id="a1735-322">The set consists of the union of the candidate operators provided by `X` and the candidate operators provided by `Y`, each determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span> <span data-ttu-id="a1735-323">如果 `X` 和 `Y` 属于同一类型，或者如果 `X` 和 `Y` 派生自一个公共的基类型，则仅在组合集内发生一次共享候选运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-323">If `X` and `Y` are the same type, or if `X` and `Y` are derived from a common base type, then shared candidate operators only occur in the combined set once.</span></span>
*  <span data-ttu-id="a1735-324">如果候选用户定义的运算符集不为空，则这将成为操作的候选运算符集。</span><span class="sxs-lookup"><span data-stu-id="a1735-324">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="a1735-325">否则，预定义的二进制 `operator op` 实现（包括其提升形式）会成为操作的候选运算符集。</span><span class="sxs-lookup"><span data-stu-id="a1735-325">Otherwise, the predefined binary `operator op` implementations, including their lifted forms,  become the set of candidate operators for the operation.</span></span> <span data-ttu-id="a1735-326">给定运算符的预定义实现是在运算符的说明（通过[条件逻辑运算符](expressions.md#conditional-logical-operators)进行[算术运算符](expressions.md#arithmetic-operators)）中指定的。</span><span class="sxs-lookup"><span data-stu-id="a1735-326">The predefined implementations of a given operator are specified in the description of the operator ([Arithmetic operators](expressions.md#arithmetic-operators) through [Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span> <span data-ttu-id="a1735-327">对于预定义的枚举和委托运算符，唯一认为的运算符是由作为一个操作数的绑定时类型的枚举或委托类型定义的运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-327">For predefined enum and delegate operators, the only operators considered are those defined by an enum or delegate type that is the binding-time type of one of the operands.</span></span>
*  <span data-ttu-id="a1735-328">[重载](expressions.md#overload-resolution)决策的重载决策规则应用于候选运算符集，以选择与 `(x,y)`参数列表相关的最佳运算符，而此运算符将成为重载决策过程的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-328">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x,y)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="a1735-329">如果重载决策未能选择单个最佳运算符，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-329">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="candidate-user-defined-operators"></a><span data-ttu-id="a1735-330">候选用户定义的运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-330">Candidate user-defined operators</span></span>

<span data-ttu-id="a1735-331">给定一个类型 `T` 和一个操作 `operator op(A)`，其中 `op` 是一个可重载的运算符，并且 `A` 是一个参数列表，则按如下方式确定 `T` 为 `operator op(A)` 提供的候选用户定义运算符集：</span><span class="sxs-lookup"><span data-stu-id="a1735-331">Given a type `T` and an operation `operator op(A)`, where `op` is an overloadable operator and `A` is an argument list, the set of candidate user-defined operators provided by `T` for `operator op(A)` is determined as follows:</span></span>

*  <span data-ttu-id="a1735-332">确定 `T0`类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-332">Determine the type `T0`.</span></span> <span data-ttu-id="a1735-333">如果 `T` 是可以为 null 的类型，则 `T0` 是其基础类型，否则 `T0` 等于 `T`。</span><span class="sxs-lookup"><span data-stu-id="a1735-333">If `T` is a nullable type, `T0` is its underlying type, otherwise `T0` is equal to `T`.</span></span>
*  <span data-ttu-id="a1735-334">对于 `T0` 中的所有 `operator op` 声明以及此类运算符的所有提升形式，如果至少有一个[运算符适用于](expressions.md#applicable-function-member)参数列表 `A`，则候选运算符集包含 `T0`中的所有此类适用运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-334">For all `operator op` declarations in `T0` and all lifted forms of such operators, if at least one operator is applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the argument list `A`, then the set of candidate operators consists of all such applicable operators in `T0`.</span></span>
*  <span data-ttu-id="a1735-335">否则，如果 `object``T0`，则候选运算符集为空。</span><span class="sxs-lookup"><span data-stu-id="a1735-335">Otherwise, if `T0` is `object`, the set of candidate operators is empty.</span></span>
*  <span data-ttu-id="a1735-336">否则，`T0` 提供的候选运算符集是 `T0`的直接基类提供的候选运算符集，如果 `T0` 为类型参数，则为 `T0` 的有效基类。</span><span class="sxs-lookup"><span data-stu-id="a1735-336">Otherwise, the set of candidate operators provided by `T0` is the set of candidate operators provided by the direct base class of `T0`, or the effective base class of `T0` if `T0` is a type parameter.</span></span>

### <a name="numeric-promotions"></a><span data-ttu-id="a1735-337">数值提升</span><span class="sxs-lookup"><span data-stu-id="a1735-337">Numeric promotions</span></span>

<span data-ttu-id="a1735-338">数值提升包括自动执行预定义的一元运算符和二元数值运算符的操作数的某些隐式转换。</span><span class="sxs-lookup"><span data-stu-id="a1735-338">Numeric promotion consists of automatically performing certain implicit conversions of the operands of the predefined unary and binary numeric operators.</span></span> <span data-ttu-id="a1735-339">数值升级不是一种不同的机制，而是将重载决策应用于预定义运算符的效果。</span><span class="sxs-lookup"><span data-stu-id="a1735-339">Numeric promotion is not a distinct mechanism, but rather an effect of applying overload resolution to the predefined operators.</span></span> <span data-ttu-id="a1735-340">尽管可以实现用户定义的运算符来表现出类似的效果，但具体的数值升级却不会影响用户定义的运算符的计算。</span><span class="sxs-lookup"><span data-stu-id="a1735-340">Numeric promotion specifically does not affect evaluation of user-defined operators, although user-defined operators can be implemented to exhibit similar effects.</span></span>

<span data-ttu-id="a1735-341">作为数值升级的一个示例，请考虑二元 `*` 运算符的预定义实现：</span><span class="sxs-lookup"><span data-stu-id="a1735-341">As an example of numeric promotion, consider the predefined implementations of the binary `*` operator:</span></span>

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

<span data-ttu-id="a1735-342">当重载决策规则（[重载决策](expressions.md#overload-resolution)）应用于这组运算符时，其效果是选择在操作数类型中存在隐式转换的第一个运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-342">When overload resolution rules ([Overload resolution](expressions.md#overload-resolution)) are applied to this set of operators, the effect is to select the first of the operators for which implicit conversions exist from the operand types.</span></span> <span data-ttu-id="a1735-343">例如，对于操作 `b * s`，其中 `b` 为 `byte`，而 `s` 为 `short`，则重载决策将 `operator *(int,int)` 选为最佳运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-343">For example, for the operation `b * s`, where `b` is a `byte` and `s` is a `short`, overload resolution selects `operator *(int,int)` as the best operator.</span></span> <span data-ttu-id="a1735-344">因此，其效果是将 `b` 和 `s` 转换为 `int`，并 `int`结果的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-344">Thus, the effect is that `b` and `s` are converted to `int`, and the type of the result is `int`.</span></span> <span data-ttu-id="a1735-345">同样，对于操作 `i * d`（其中，`i` 是一个 `int` 并且 `d` 为 `double`），重载决策选择 `operator *(double,double)` 作为最佳运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-345">Likewise, for the operation `i * d`, where `i` is an `int` and `d` is a `double`, overload resolution selects `operator *(double,double)` as the best operator.</span></span>

#### <a name="unary-numeric-promotions"></a><span data-ttu-id="a1735-346">一元数值促销</span><span class="sxs-lookup"><span data-stu-id="a1735-346">Unary numeric promotions</span></span>

<span data-ttu-id="a1735-347">一元数值升级发生于预定义 `+`、`-`和 `~` 一元运算符的操作数。</span><span class="sxs-lookup"><span data-stu-id="a1735-347">Unary numeric promotion occurs for the operands of the predefined `+`, `-`, and `~` unary operators.</span></span> <span data-ttu-id="a1735-348">一元数值提升只包含将类型 `sbyte`、`byte`、`short`、`ushort`或 `char` 的操作数转换为类型 `int`。</span><span class="sxs-lookup"><span data-stu-id="a1735-348">Unary numeric promotion simply consists of converting operands of type `sbyte`, `byte`, `short`, `ushort`, or `char` to type `int`.</span></span> <span data-ttu-id="a1735-349">此外，对于一元 `-` 运算符，一元数值提升将类型 `uint` 的操作数转换为类型 `long`。</span><span class="sxs-lookup"><span data-stu-id="a1735-349">Additionally, for the unary `-` operator, unary numeric promotion converts operands of type `uint` to type `long`.</span></span>

#### <a name="binary-numeric-promotions"></a><span data-ttu-id="a1735-350">二进制数值升级</span><span class="sxs-lookup"><span data-stu-id="a1735-350">Binary numeric promotions</span></span>

<span data-ttu-id="a1735-351">二元数值升级发生在预定义 `+`、`-`、`*`、`/`、`%`、`&`、`|`、`^`、`==`、`!=`、`>`、`<`、`>=`和 `<=` 二元运算符的操作数。</span><span class="sxs-lookup"><span data-stu-id="a1735-351">Binary numeric promotion occurs for the operands of the predefined `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, and `<=` binary operators.</span></span> <span data-ttu-id="a1735-352">二进制数值升级会隐式地将两个操作数转换为通用类型，在非关系运算符的情况下，也会成为运算的结果类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-352">Binary numeric promotion implicitly converts both operands to a common type which, in case of the non-relational operators, also becomes the result type of the operation.</span></span> <span data-ttu-id="a1735-353">二进制数值升级包括应用下列规则，顺序如下：</span><span class="sxs-lookup"><span data-stu-id="a1735-353">Binary numeric promotion consists of applying the following rules, in the order they appear here:</span></span>

*  <span data-ttu-id="a1735-354">如果任一操作数为类型 `decimal`，则将另一个操作数转换为类型 `decimal`，如果另一个操作数的类型为 `float` 或 `double`，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-354">If either operand is of type `decimal`, the other operand is converted to type `decimal`, or a binding-time error occurs if the other operand is of type `float` or `double`.</span></span>
*  <span data-ttu-id="a1735-355">否则，如果其中一个操作数为 `double`类型，则另一个操作数将转换为类型 `double`。</span><span class="sxs-lookup"><span data-stu-id="a1735-355">Otherwise, if either operand is of type `double`, the other operand is converted to type `double`.</span></span>
*  <span data-ttu-id="a1735-356">否则，如果其中一个操作数为 `float`类型，则另一个操作数将转换为类型 `float`。</span><span class="sxs-lookup"><span data-stu-id="a1735-356">Otherwise, if either operand is of type `float`, the other operand is converted to type `float`.</span></span>
*  <span data-ttu-id="a1735-357">否则，如果其中一个操作数为 `ulong`类型，则另一个操作数将转换为类型 `ulong`，如果另一个操作数的类型为 `sbyte`、`short`、`int`或 `long`，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-357">Otherwise, if either operand is of type `ulong`, the other operand is converted to type `ulong`, or a binding-time error occurs if the other operand is of type `sbyte`, `short`, `int`, or `long`.</span></span>
*  <span data-ttu-id="a1735-358">否则，如果其中一个操作数为 `long`类型，则另一个操作数将转换为类型 `long`。</span><span class="sxs-lookup"><span data-stu-id="a1735-358">Otherwise, if either operand is of type `long`, the other operand is converted to type `long`.</span></span>
*  <span data-ttu-id="a1735-359">否则，如果其中一个操作数的类型为 `uint`，另一个操作数的类型为 `sbyte`、`short`或 `int`，则这两个操作数都将转换为类型 `long`。</span><span class="sxs-lookup"><span data-stu-id="a1735-359">Otherwise, if either operand is of type `uint` and the other operand is of type `sbyte`, `short`, or `int`, both operands are converted to type `long`.</span></span>
*  <span data-ttu-id="a1735-360">否则，如果其中一个操作数为 `uint`类型，则另一个操作数将转换为类型 `uint`。</span><span class="sxs-lookup"><span data-stu-id="a1735-360">Otherwise, if either operand is of type `uint`, the other operand is converted to type `uint`.</span></span>
*  <span data-ttu-id="a1735-361">否则，两个操作数都将转换为 `int`类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-361">Otherwise, both operands are converted to type `int`.</span></span>

<span data-ttu-id="a1735-362">请注意，第一个规则禁止将 `decimal` 类型与 `double` 和 `float` 类型一起使用的任何操作。</span><span class="sxs-lookup"><span data-stu-id="a1735-362">Note that the first rule disallows any operations that mix the `decimal` type with the `double` and `float` types.</span></span> <span data-ttu-id="a1735-363">规则如下所示，这是因为 `decimal` 类型与 `double` 和 `float` 类型之间没有隐式转换。</span><span class="sxs-lookup"><span data-stu-id="a1735-363">The rule follows from the fact that there are no implicit conversions between the `decimal` type and the `double` and `float` types.</span></span>

<span data-ttu-id="a1735-364">另请注意，如果另一个操作数为有符号整数类型，则操作数不能 `ulong` 类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-364">Also note that it is not possible for an operand to be of type `ulong` when the other operand is of a signed integral type.</span></span> <span data-ttu-id="a1735-365">原因在于，不存在可以表示全部 `ulong` 范围和有符号整数类型的整数类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-365">The reason is that no integral type exists that can represent the full range of `ulong` as well as the signed integral types.</span></span>

<span data-ttu-id="a1735-366">在上述两种情况下，强制转换表达式可用于将一个操作数显式转换为与另一个操作数兼容的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-366">In both of the above cases, a cast expression can be used to explicitly convert one operand to a type that is compatible with the other operand.</span></span>

<span data-ttu-id="a1735-367">示例中</span><span class="sxs-lookup"><span data-stu-id="a1735-367">In the example</span></span>
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
<span data-ttu-id="a1735-368">发生绑定时错误，因为 `decimal` 不能与 `double`相乘。</span><span class="sxs-lookup"><span data-stu-id="a1735-368">a binding-time error occurs because a `decimal` cannot be multiplied by a `double`.</span></span> <span data-ttu-id="a1735-369">通过将第二个操作数显式转换为 `decimal`来解决此错误，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a1735-369">The error is resolved by explicitly converting the second operand to `decimal`, as follows:</span></span>

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a><span data-ttu-id="a1735-370">提升的运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-370">Lifted operators</span></span>

<span data-ttu-id="a1735-371">***提升运算符***允许对不可以为 null 的值类型进行运算的预定义运算符和用户定义的运算符也可用于这些类型的可以为 null 的形式。</span><span class="sxs-lookup"><span data-stu-id="a1735-371">***Lifted operators*** permit predefined and user-defined operators that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="a1735-372">提升运算符是根据满足某些要求的预定义运算符和用户定义的运算符构造的，如下所述：</span><span class="sxs-lookup"><span data-stu-id="a1735-372">Lifted operators are constructed from predefined and user-defined operators that meet certain requirements, as described in the following:</span></span>

*   <span data-ttu-id="a1735-373">对于一元运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-373">For the unary operators</span></span>

    ```csharp
    +  ++  -  --  !  ~
    ```

    <span data-ttu-id="a1735-374">如果操作数和结果类型都是不可为 null 的值类型，则会存在运算符的提升形式。</span><span class="sxs-lookup"><span data-stu-id="a1735-374">a lifted form of an operator exists if the operand and result types are both non-nullable value types.</span></span> <span data-ttu-id="a1735-375">提升形式是通过向操作数和结果类型添加单个 `?` 修饰符来构造的。</span><span class="sxs-lookup"><span data-stu-id="a1735-375">The lifted form is constructed by adding a single `?` modifier to the operand and result types.</span></span> <span data-ttu-id="a1735-376">如果操作数为 null，则提升运算符将生成 null 值。</span><span class="sxs-lookup"><span data-stu-id="a1735-376">The lifted operator produces a null value if the operand is null.</span></span> <span data-ttu-id="a1735-377">否则，提升运算符将对操作数进行解包，应用基础运算符并包装结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-377">Otherwise, the lifted operator unwraps the operand, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="a1735-378">对于二元运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-378">For the binary operators</span></span>

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    <span data-ttu-id="a1735-379">如果操作数和结果类型都是不可以为 null 的值类型，则该运算符的提升形式存在。</span><span class="sxs-lookup"><span data-stu-id="a1735-379">a lifted form of an operator exists if the operand and result types are all non-nullable value types.</span></span> <span data-ttu-id="a1735-380">提升形式是通过向每个操作数和结果类型添加单个 `?` 修饰符来构造的。</span><span class="sxs-lookup"><span data-stu-id="a1735-380">The lifted form is constructed by adding a single `?` modifier to each operand and result type.</span></span> <span data-ttu-id="a1735-381">如果有一个或两个操作数为空（`bool?` 类型的 `&` 和 `|` 运算符的异常，如[布尔逻辑运算符](expressions.md#boolean-logical-operators)中所述），则提升的运算符将生成 null 值。</span><span class="sxs-lookup"><span data-stu-id="a1735-381">The lifted operator produces a null value if one or both operands are null (an exception being the `&` and `|` operators of the `bool?` type, as described in [Boolean logical operators](expressions.md#boolean-logical-operators)).</span></span> <span data-ttu-id="a1735-382">否则，提升运算符将对操作数进行解包，应用基础运算符并包装结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-382">Otherwise, the lifted operator unwraps the operands, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="a1735-383">对于相等运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-383">For the equality operators</span></span>

    ```csharp
    ==  !=
    ```

    <span data-ttu-id="a1735-384">如果操作数类型既是不可为 null 的值类型，也不是 `bool`的结果类型，则运算符的提升形式存在。</span><span class="sxs-lookup"><span data-stu-id="a1735-384">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="a1735-385">提升形式是通过向每个操作数类型添加单个 `?` 修饰符来构造的。</span><span class="sxs-lookup"><span data-stu-id="a1735-385">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="a1735-386">提升的运算符将两个 null 值视为相等，并将 null 值视为非 null 值。</span><span class="sxs-lookup"><span data-stu-id="a1735-386">The lifted operator considers two null values equal, and a null value unequal to any non-null value.</span></span> <span data-ttu-id="a1735-387">如果两个操作数都非空，则提升运算符将对操作数进行解包，并应用基础运算符来生成 `bool` 结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-387">If both operands are non-null, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

*   <span data-ttu-id="a1735-388">对于关系运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-388">For the relational operators</span></span>

    ```csharp
    <  >  <=  >=
    ```

    <span data-ttu-id="a1735-389">如果操作数类型既是不可为 null 的值类型，也不是 `bool`的结果类型，则运算符的提升形式存在。</span><span class="sxs-lookup"><span data-stu-id="a1735-389">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="a1735-390">提升形式是通过向每个操作数类型添加单个 `?` 修饰符来构造的。</span><span class="sxs-lookup"><span data-stu-id="a1735-390">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="a1735-391">如果一个或两个操作数为 null，则提升运算符将生成值 `false`。</span><span class="sxs-lookup"><span data-stu-id="a1735-391">The lifted operator produces the value `false` if one or both operands are null.</span></span> <span data-ttu-id="a1735-392">否则，提升运算符将对操作数进行解包，并应用基础运算符来产生 `bool` 结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-392">Otherwise, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

## <a name="member-lookup"></a><span data-ttu-id="a1735-393">成员查找</span><span class="sxs-lookup"><span data-stu-id="a1735-393">Member lookup</span></span>

<span data-ttu-id="a1735-394">成员查找是指确定类型上下文中名称含义的进程。</span><span class="sxs-lookup"><span data-stu-id="a1735-394">A member lookup is the process whereby the meaning of a name in the context of a type is determined.</span></span> <span data-ttu-id="a1735-395">成员查找在计算表达式中的*simple_name* （[简单名称](expressions.md#simple-names)）或*member_access* （[成员访问](expressions.md#member-access)）的过程中可能发生。</span><span class="sxs-lookup"><span data-stu-id="a1735-395">A member lookup can occur as part of evaluating a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)) in an expression.</span></span> <span data-ttu-id="a1735-396">如果*simple_name*或*member_access*作为*invocation_expression* （[方法调用](expressions.md#method-invocations)）的*primary_expression*出现，则称成员被调用。</span><span class="sxs-lookup"><span data-stu-id="a1735-396">If the *simple_name* or *member_access* occurs as the *primary_expression* of an *invocation_expression* ([Method invocations](expressions.md#method-invocations)), the member is said to be invoked.</span></span>

<span data-ttu-id="a1735-397">如果成员是方法或事件，或者它是委托类型（[委托](delegates.md)）或类型 `dynamic` （[动态类型](types.md#the-dynamic-type)）的常量、字段或属性，则称成员是*invocable*。</span><span class="sxs-lookup"><span data-stu-id="a1735-397">If a member is a method or event, or if it is a constant, field or property of either a delegate type ([Delegates](delegates.md)) or the type `dynamic` ([The dynamic type](types.md#the-dynamic-type)), then the member is said to be *invocable*.</span></span>

<span data-ttu-id="a1735-398">成员查找不仅考虑了成员的名称，还考虑了成员具有的类型参数的数目以及该成员是否可访问。</span><span class="sxs-lookup"><span data-stu-id="a1735-398">Member lookup considers not only the name of a member but also the number of type parameters the member has and whether the member is accessible.</span></span> <span data-ttu-id="a1735-399">出于成员查找的目的，泛型方法和嵌套泛型类型具有其各自声明中指示的类型参数的数目，并且所有其他成员都有零个类型参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-399">For the purposes of member lookup, generic methods and nested generic types have the number of type parameters indicated in their respective declarations and all other members have zero type parameters.</span></span>

<span data-ttu-id="a1735-400">类型 `T` 的 `K` 类型参数 `N` 名称的成员查找按如下方式进行处理：</span><span class="sxs-lookup"><span data-stu-id="a1735-400">A member lookup of a name `N` with `K` type parameters in a type `T` is processed as follows:</span></span>

*  <span data-ttu-id="a1735-401">首先，确定一组名为 `N` 的可访问成员：</span><span class="sxs-lookup"><span data-stu-id="a1735-401">First, a set of accessible members named `N` is determined:</span></span>
    * <span data-ttu-id="a1735-402">如果 `T` 是类型参数，则该集是在每个指定为 `object` `N` `T`的主约束或辅助约束（[类型参数约束](classes.md#type-parameter-constraints)）的每个类型中名为 `N` 的可访问成员集的并集。</span><span class="sxs-lookup"><span data-stu-id="a1735-402">If `T` is a type parameter, then the set is the union of the sets of accessible members named `N` in each of the types specified as a primary constraint or secondary constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) for `T`, along with the set of accessible members named `N` in `object`.</span></span>
    * <span data-ttu-id="a1735-403">否则，该集由 `T`中名为 `N` 的所有可访问（[成员访问](basic-concepts.md#member-access)）成员组成，其中包括 `object`中名为 `N` 的继承成员和可访问成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-403">Otherwise, the set consists of all accessible ([Member access](basic-concepts.md#member-access)) members named `N` in `T`, including inherited members and the accessible members named `N` in `object`.</span></span> <span data-ttu-id="a1735-404">如果 `T` 是构造类型，则通过替换类型参数来获取成员集，如[构造类型成员](classes.md#members-of-constructed-types)中所述。</span><span class="sxs-lookup"><span data-stu-id="a1735-404">If `T` is a constructed type, the set of members is obtained by substituting type arguments as described in [Members of constructed types](classes.md#members-of-constructed-types).</span></span> <span data-ttu-id="a1735-405">包含 `override` 修饰符的成员将从集合中排除。</span><span class="sxs-lookup"><span data-stu-id="a1735-405">Members that include an `override` modifier are excluded from the set.</span></span>
*  <span data-ttu-id="a1735-406">接下来，如果 `K` 为零，则将删除所有声明都包含类型参数的嵌套类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-406">Next, if `K` is zero, all nested types whose declarations include type parameters are removed.</span></span> <span data-ttu-id="a1735-407">如果 `K` 不为零，则删除所有具有不同数量的类型参数的成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-407">If `K` is not zero, all members with a different number of type parameters are removed.</span></span> <span data-ttu-id="a1735-408">请注意，当 `K` 为零时，不会删除具有类型参数的方法，因为类型推理过程（[类型推理](expressions.md#type-inference)）可能能够推断类型参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-408">Note that when `K` is zero, methods having type parameters are not removed, since the type inference process ([Type inference](expressions.md#type-inference)) might be able to infer the type arguments.</span></span>
*  <span data-ttu-id="a1735-409">接下来，如果*调用*成员，则将从集合中删除所有非*invocable*成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-409">Next, if the member is *invoked*, all non-*invocable* members are removed from the set.</span></span>
*  <span data-ttu-id="a1735-410">接下来，将从集合中删除其他成员隐藏的成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-410">Next, members that are hidden by other members are removed from the set.</span></span> <span data-ttu-id="a1735-411">对于集中的每个成员 `S.M` （其中 `S` 是声明成员 `M` 的类型），将应用以下规则：</span><span class="sxs-lookup"><span data-stu-id="a1735-411">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied:</span></span>
    * <span data-ttu-id="a1735-412">如果 `M` 是常量、字段、属性、事件或枚举成员，则将从集合中删除在 `S` 的基类型中声明的所有成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-412">If `M` is a constant, field, property, event, or enumeration member, then all members declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="a1735-413">如果 `M` 是类型声明，则从该集中删除所有在 `S` 的基类型中声明的非类型，并且与在 `S` 的基类型中声明的 `M` 类型参数相同的所有类型声明都将从集合中删除。</span><span class="sxs-lookup"><span data-stu-id="a1735-413">If `M` is a type declaration, then all non-types declared in a base type of `S` are removed from the set, and all type declarations with the same number of type parameters as `M` declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="a1735-414">如果 `M` 是一种方法，则会从该集内删除在 `S` 的基类型中声明的所有非方法成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-414">If `M` is a method, then all non-method members declared in a base type of `S` are removed from the set.</span></span>
*  <span data-ttu-id="a1735-415">接下来，将从集合中删除类成员隐藏的接口成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-415">Next, interface members that are hidden by class members are removed from the set.</span></span> <span data-ttu-id="a1735-416">仅当 `T` 是类型参数并且 `T` 既具有 `object` 的有效基类，也具有非空的有效接口集（[类型参数约束](classes.md#type-parameter-constraints)）时，此步骤才会生效。</span><span class="sxs-lookup"><span data-stu-id="a1735-416">This step only has an effect if `T` is a type parameter and `T` has both an effective base class other than `object` and a non-empty effective interface set ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="a1735-417">对于集合中的每个成员 `S.M` （其中 `S` 是声明成员 `M` 的类型），如果 `S` 为除 `object`之外的类声明，则应用以下规则：</span><span class="sxs-lookup"><span data-stu-id="a1735-417">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied if `S` is a class declaration other than `object`:</span></span>
    * <span data-ttu-id="a1735-418">如果 `M` 是常量、字段、属性、事件、枚举成员或类型声明，则会从集中删除在接口声明中声明的所有成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-418">If `M` is a constant, field, property, event, enumeration member, or type declaration, then all members declared in an interface declaration are removed from the set.</span></span>
    * <span data-ttu-id="a1735-419">如果 `M` 是方法，则将从集合中删除在接口声明中声明的所有非方法成员，并且将从该集内删除与在接口声明中声明的 `M` 具有相同签名的所有方法。</span><span class="sxs-lookup"><span data-stu-id="a1735-419">If `M` is a method, then all non-method members declared in an interface declaration are removed from the set, and all methods with the same signature as `M` declared in an interface declaration are removed from the set.</span></span>
*  <span data-ttu-id="a1735-420">最后，删除隐藏成员后，将确定查找的结果：</span><span class="sxs-lookup"><span data-stu-id="a1735-420">Finally, having removed hidden members, the result of the lookup is determined:</span></span>
    * <span data-ttu-id="a1735-421">如果集由不是方法的单个成员组成，则此成员是查找的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-421">If the set consists of a single member that is not a method, then this member is the result of the lookup.</span></span>
    * <span data-ttu-id="a1735-422">否则，如果该集仅包含方法，则此方法组是查找的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-422">Otherwise, if the set contains only methods, then this group of methods is the result of the lookup.</span></span>
    * <span data-ttu-id="a1735-423">否则，查找将不明确，并发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-423">Otherwise, the lookup is ambiguous, and a binding-time error occurs.</span></span>

<span data-ttu-id="a1735-424">对于类型参数和接口以外的类型中的成员查找以及严格为单一继承的接口中的成员查找（继承链中的每个接口都具有完全零或一个直接基接口），查找规则的效果是只是派生成员将隐藏具有相同名称或签名的基成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-424">For member lookups in types other than type parameters and interfaces, and member lookups in interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effect of the lookup rules is simply that derived members hide base members with the same name or signature.</span></span> <span data-ttu-id="a1735-425">这种单一继承查找从来都不明确。</span><span class="sxs-lookup"><span data-stu-id="a1735-425">Such single-inheritance lookups are never ambiguous.</span></span> <span data-ttu-id="a1735-426">[接口成员访问](interfaces.md#interface-member-access)中介绍了可能从多继承接口中的成员查找产生的歧义。</span><span class="sxs-lookup"><span data-stu-id="a1735-426">The ambiguities that can possibly arise from member lookups in multiple-inheritance interfaces are described in [Interface member access](interfaces.md#interface-member-access).</span></span>

### <a name="base-types"></a><span data-ttu-id="a1735-427">基类型</span><span class="sxs-lookup"><span data-stu-id="a1735-427">Base types</span></span>

<span data-ttu-id="a1735-428">出于成员查找的目的，类型 `T` 被视为具有以下基本类型：</span><span class="sxs-lookup"><span data-stu-id="a1735-428">For purposes of member lookup, a type `T` is considered to have the following base types:</span></span>

*  <span data-ttu-id="a1735-429">如果 `object``T`，则 `T` 没有基类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-429">If `T` is `object`, then `T` has no base type.</span></span>
*  <span data-ttu-id="a1735-430">如果 `T` 是*enum_type*，则 `T` 的基类型是类类型 `System.Enum`、`System.ValueType`和 `object`。</span><span class="sxs-lookup"><span data-stu-id="a1735-430">If `T` is an *enum_type*, the base types of `T` are the class types `System.Enum`, `System.ValueType`, and `object`.</span></span>
*  <span data-ttu-id="a1735-431">如果 `T` 是*struct_type*，则 `T` 的基类型是类类型 `System.ValueType` 和 `object`。</span><span class="sxs-lookup"><span data-stu-id="a1735-431">If `T` is a *struct_type*, the base types of `T` are the class types `System.ValueType` and `object`.</span></span>
*  <span data-ttu-id="a1735-432">如果 `T` 是*class_type*，则 `T` 的基类型是 `T`的基类，包括类类型 `object`。</span><span class="sxs-lookup"><span data-stu-id="a1735-432">If `T` is a *class_type*, the base types of `T` are the base classes of `T`, including the class type `object`.</span></span>
*  <span data-ttu-id="a1735-433">如果 `T` 是*interface_type*，则 `T` 的基类型是 `T` 和类类型 `object`的基接口。</span><span class="sxs-lookup"><span data-stu-id="a1735-433">If `T` is an *interface_type*, the base types of `T` are the base interfaces of `T` and the class type `object`.</span></span>
*  <span data-ttu-id="a1735-434">如果 `T` 是*array_type*，则 `T` 的基类型是类类型 `System.Array` 和 `object`。</span><span class="sxs-lookup"><span data-stu-id="a1735-434">If `T` is an *array_type*, the base types of `T` are the class types `System.Array` and `object`.</span></span>
*  <span data-ttu-id="a1735-435">如果 `T` 是*delegate_type*，则 `T` 的基类型是类类型 `System.Delegate` 和 `object`。</span><span class="sxs-lookup"><span data-stu-id="a1735-435">If `T` is a *delegate_type*, the base types of `T` are the class types `System.Delegate` and `object`.</span></span>

## <a name="function-members"></a><span data-ttu-id="a1735-436">函数成员</span><span class="sxs-lookup"><span data-stu-id="a1735-436">Function members</span></span>

<span data-ttu-id="a1735-437">函数成员是包含可执行语句的成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-437">Function members are members that contain executable statements.</span></span> <span data-ttu-id="a1735-438">函数成员始终是类型的成员，不能是命名空间的成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-438">Function members are always members of types and cannot be members of namespaces.</span></span> <span data-ttu-id="a1735-439">C#定义以下类别的函数成员：</span><span class="sxs-lookup"><span data-stu-id="a1735-439">C# defines the following categories of function members:</span></span>

*  <span data-ttu-id="a1735-440">方法</span><span class="sxs-lookup"><span data-stu-id="a1735-440">Methods</span></span>
*  <span data-ttu-id="a1735-441">属性</span><span class="sxs-lookup"><span data-stu-id="a1735-441">Properties</span></span>
*  <span data-ttu-id="a1735-442">Events</span><span class="sxs-lookup"><span data-stu-id="a1735-442">Events</span></span>
*  <span data-ttu-id="a1735-443">索引器</span><span class="sxs-lookup"><span data-stu-id="a1735-443">Indexers</span></span>
*  <span data-ttu-id="a1735-444">用户定义的运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-444">User-defined operators</span></span>
*  <span data-ttu-id="a1735-445">实例构造函数</span><span class="sxs-lookup"><span data-stu-id="a1735-445">Instance constructors</span></span>
*  <span data-ttu-id="a1735-446">静态构造函数</span><span class="sxs-lookup"><span data-stu-id="a1735-446">Static constructors</span></span>
*  <span data-ttu-id="a1735-447">析构函数。</span><span class="sxs-lookup"><span data-stu-id="a1735-447">Destructors</span></span>

<span data-ttu-id="a1735-448">除析构函数和静态构造函数（无法显式调用）以外，函数成员中包含的语句是通过函数成员调用来执行的。</span><span class="sxs-lookup"><span data-stu-id="a1735-448">Except for destructors and static constructors (which cannot be invoked explicitly), the statements contained in function members are executed through function member invocations.</span></span> <span data-ttu-id="a1735-449">用于编写函数成员调用的实际语法取决于特定的函数成员类别。</span><span class="sxs-lookup"><span data-stu-id="a1735-449">The actual syntax for writing a function member invocation depends on the particular function member category.</span></span>

<span data-ttu-id="a1735-450">函数成员调用的参数列表（[参数](expressions.md#argument-lists)列表）为函数成员的参数提供实际值或变量引用。</span><span class="sxs-lookup"><span data-stu-id="a1735-450">The argument list ([Argument lists](expressions.md#argument-lists)) of a function member invocation provides actual values or variable references for the parameters of the function member.</span></span>

<span data-ttu-id="a1735-451">调用泛型方法时，可以使用类型推理来确定要传递给方法的类型参数集。</span><span class="sxs-lookup"><span data-stu-id="a1735-451">Invocations of generic methods may employ type inference to determine the set of type arguments to pass to the method.</span></span> <span data-ttu-id="a1735-452">此过程在[类型推理](expressions.md#type-inference)中进行了介绍。</span><span class="sxs-lookup"><span data-stu-id="a1735-452">This process is described in [Type inference](expressions.md#type-inference).</span></span>

<span data-ttu-id="a1735-453">调用方法、索引器、运算符和实例构造函数会使用重载决策来确定要调用的候选函数成员集中哪一组。</span><span class="sxs-lookup"><span data-stu-id="a1735-453">Invocations of methods, indexers, operators and instance constructors employ overload resolution to determine which of a candidate set of function members to invoke.</span></span> <span data-ttu-id="a1735-454">[重载决策](expressions.md#overload-resolution)中介绍了此过程。</span><span class="sxs-lookup"><span data-stu-id="a1735-454">This process is described in [Overload resolution](expressions.md#overload-resolution).</span></span>

<span data-ttu-id="a1735-455">在绑定时确定了特定函数成员（可能通过重载决策）后，调用函数成员的实际运行时过程将在[编译时检查动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution)中进行说明。</span><span class="sxs-lookup"><span data-stu-id="a1735-455">Once a particular function member has been identified at binding-time, possibly through overload resolution, the actual run-time process of invoking the function member is described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="a1735-456">下表总结了构造中发生的处理，涉及到可显式调用的六类函数成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-456">The following table summarizes the processing that takes place in constructs involving the six categories of function members that can be explicitly invoked.</span></span> <span data-ttu-id="a1735-457">在表中，`e`、`x`、`y`和 `value` 指示归类为变量或值的表达式，`T` 指示归类为类型的表达式，`F` 是方法的简单名称，`P` 是属性的简单名称。</span><span class="sxs-lookup"><span data-stu-id="a1735-457">In the table, `e`, `x`, `y`, and `value` indicate expressions classified as variables or values, `T` indicates an expression classified as a type, `F` is the simple name of a method, and `P` is the simple name of a property.</span></span>


| <span data-ttu-id="a1735-458">__构造__</span><span class="sxs-lookup"><span data-stu-id="a1735-458">__Construct__</span></span>     | <span data-ttu-id="a1735-459">__示例__</span><span class="sxs-lookup"><span data-stu-id="a1735-459">__Example__</span></span>    | <span data-ttu-id="a1735-460">__描述__</span><span class="sxs-lookup"><span data-stu-id="a1735-460">__Description__</span></span> |
|-------------------|----------------|-----------------|
| <span data-ttu-id="a1735-461">方法调用</span><span class="sxs-lookup"><span data-stu-id="a1735-461">Method invocation</span></span> | `F(x,y)`       | <span data-ttu-id="a1735-462">应用重载决策，以选择包含类或结构中 `F` 的最佳方法。</span><span class="sxs-lookup"><span data-stu-id="a1735-462">Overload resolution is applied to select the best method `F` in the containing class or struct.</span></span> <span data-ttu-id="a1735-463">方法是通过参数列表 `(x,y)`调用的。</span><span class="sxs-lookup"><span data-stu-id="a1735-463">The method is invoked with the argument list `(x,y)`.</span></span> <span data-ttu-id="a1735-464">如果未 `static`方法，则 `this`实例表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-464">If the method is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.F(x,y)`     | <span data-ttu-id="a1735-465">应用重载决策，以选择类或结构 `T`中 `F` 的最佳方法。</span><span class="sxs-lookup"><span data-stu-id="a1735-465">Overload resolution is applied to select the best method `F` in the class or struct `T`.</span></span> <span data-ttu-id="a1735-466">如果未 `static`方法，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-466">A binding-time error occurs if the method is not `static`.</span></span> <span data-ttu-id="a1735-467">方法是通过参数列表 `(x,y)`调用的。</span><span class="sxs-lookup"><span data-stu-id="a1735-467">The method is invoked with the argument list `(x,y)`.</span></span> | 
|                   | `e.F(x,y)`     | <span data-ttu-id="a1735-468">重载决策适用于在类、结构或接口中选择由 `e`类型给定的最佳方法 F。</span><span class="sxs-lookup"><span data-stu-id="a1735-468">Overload resolution is applied to select the best method F in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="a1735-469">如果 `static`方法，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-469">A binding-time error occurs if the method is `static`.</span></span> <span data-ttu-id="a1735-470">方法是通过实例表达式 `e` 调用的，而参数列表 `(x,y)`。</span><span class="sxs-lookup"><span data-stu-id="a1735-470">The method is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="a1735-471">和</span><span class="sxs-lookup"><span data-stu-id="a1735-471">Property access</span></span>   | `P`            | <span data-ttu-id="a1735-472">调用包含类或结构中的属性 `P` 的 `get` 访问器。</span><span class="sxs-lookup"><span data-stu-id="a1735-472">The `get` accessor of the property `P` in the containing class or struct is invoked.</span></span> <span data-ttu-id="a1735-473">如果 `P` 是只写的，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-473">A compile-time error occurs if `P` is write-only.</span></span> <span data-ttu-id="a1735-474">如果未 `static``P`，则 `this`实例表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-474">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `P = value`    | <span data-ttu-id="a1735-475">包含类或结构中的属性 `P` 的 `set` 访问器是通过参数列表 `(value)`调用的。</span><span class="sxs-lookup"><span data-stu-id="a1735-475">The `set` accessor of the property `P` in the containing class or struct is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="a1735-476">如果 `P` 是只读的，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-476">A compile-time error occurs if `P` is read-only.</span></span> <span data-ttu-id="a1735-477">如果未 `static``P`，则 `this`实例表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-477">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.P`          | <span data-ttu-id="a1735-478">调用类或结构 `T` 中 `P` 属性的 `get` 访问器。</span><span class="sxs-lookup"><span data-stu-id="a1735-478">The `get` accessor of the property `P` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="a1735-479">如果 `P` 未 `static` 或 `P` 是只写的，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-479">A compile-time error occurs if `P` is not `static` or if `P` is write-only.</span></span> | 
|                   | `T.P = value`  | <span data-ttu-id="a1735-480">类或结构 `T` 中 `P` 属性的 `set` 访问器是通过参数列表 `(value)`调用的。</span><span class="sxs-lookup"><span data-stu-id="a1735-480">The `set` accessor of the property `P` in the class or struct `T` is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="a1735-481">如果 `P` 未 `static` 或 `P` 为只读，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-481">A compile-time error occurs if `P` is not `static` or if `P` is read-only.</span></span> | 
|                   | `e.P`          | <span data-ttu-id="a1735-482">`e` 类型指定的类、结构或接口中的属性 `P` 的 `get` 访问器在实例表达式 `e`中调用。</span><span class="sxs-lookup"><span data-stu-id="a1735-482">The `get` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="a1735-483">如果 `P` `static` 或 `P` 是只写的，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-483">A binding-time error occurs if `P` is `static` or if `P` is write-only.</span></span> | 
|                   | `e.P = value`  | <span data-ttu-id="a1735-484">`e` 类型给定的类、结构或接口中的属性 `P` 的 `set` 访问器是使用实例表达式 `e` 和参数列表 `(value)`调用的。</span><span class="sxs-lookup"><span data-stu-id="a1735-484">The `set` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e` and the argument list `(value)`.</span></span> <span data-ttu-id="a1735-485">如果 `P` `static` 或 `P` 为只读，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-485">A binding-time error occurs if `P` is `static` or if `P` is read-only.</span></span> | 
| <span data-ttu-id="a1735-486">事件访问</span><span class="sxs-lookup"><span data-stu-id="a1735-486">Event access</span></span>      | `E += value`   | <span data-ttu-id="a1735-487">调用包含类或结构中的事件 `E` 的 `add` 访问器。</span><span class="sxs-lookup"><span data-stu-id="a1735-487">The `add` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="a1735-488">如果 `E` 不是静态的，则 `this`实例表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-488">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `E -= value`   | <span data-ttu-id="a1735-489">调用包含类或结构中的事件 `E` 的 `remove` 访问器。</span><span class="sxs-lookup"><span data-stu-id="a1735-489">The `remove` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="a1735-490">如果 `E` 不是静态的，则 `this`实例表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-490">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `T.E += value` | <span data-ttu-id="a1735-491">调用类或结构 `T` 中 `E` 事件的 `add` 访问器。</span><span class="sxs-lookup"><span data-stu-id="a1735-491">The `add` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="a1735-492">如果 `E` 不是静态的，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-492">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `T.E -= value` | <span data-ttu-id="a1735-493">调用类或结构 `T` 中 `E` 事件的 `remove` 访问器。</span><span class="sxs-lookup"><span data-stu-id="a1735-493">The `remove` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="a1735-494">如果 `E` 不是静态的，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-494">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `e.E += value` | <span data-ttu-id="a1735-495">`e` 类型给定的类、结构或接口中的事件 `E` 的 `add` 访问器在实例表达式 `e`中调用。</span><span class="sxs-lookup"><span data-stu-id="a1735-495">The `add` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="a1735-496">如果 `E` 是静态的，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-496">A binding-time error occurs if `E` is static.</span></span> | 
|                   | `e.E -= value` | <span data-ttu-id="a1735-497">`e` 类型给定的类、结构或接口中的事件 `E` 的 `remove` 访问器在实例表达式 `e`中调用。</span><span class="sxs-lookup"><span data-stu-id="a1735-497">The `remove` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="a1735-498">如果 `E` 是静态的，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-498">A binding-time error occurs if `E` is static.</span></span> | 
| <span data-ttu-id="a1735-499">索引器访问</span><span class="sxs-lookup"><span data-stu-id="a1735-499">Indexer access</span></span>    | `e[x,y]`       | <span data-ttu-id="a1735-500">重载决策适用于在由 e 类型给定的类、结构或接口中选择最佳索引器。</span><span class="sxs-lookup"><span data-stu-id="a1735-500">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of e.</span></span> <span data-ttu-id="a1735-501">索引器的 `get` 访问器在实例表达式 `e` 和参数列表 `(x,y)`调用。</span><span class="sxs-lookup"><span data-stu-id="a1735-501">The `get` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> <span data-ttu-id="a1735-502">如果索引器是只写的，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-502">A binding-time error occurs if the indexer is write-only.</span></span> | 
|                   | `e[x,y] = value` | <span data-ttu-id="a1735-503">重载决策适用于在类、结构或接口中选择由 `e`类型给定的最佳索引器。</span><span class="sxs-lookup"><span data-stu-id="a1735-503">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="a1735-504">索引器的 `set` 访问器在实例表达式 `e` 和参数列表 `(x,y,value)`调用。</span><span class="sxs-lookup"><span data-stu-id="a1735-504">The `set` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y,value)`.</span></span> <span data-ttu-id="a1735-505">如果索引器是只读的，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-505">A binding-time error occurs if the indexer is read-only.</span></span> | 
| <span data-ttu-id="a1735-506">运算符调用</span><span class="sxs-lookup"><span data-stu-id="a1735-506">Operator invocation</span></span> | `-x`         | <span data-ttu-id="a1735-507">应用重载决策，以选择类或结构中由 `x`类型给定的最佳一元运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-507">Overload resolution is applied to select the best unary operator in the class or struct given by the type of `x`.</span></span> <span data-ttu-id="a1735-508">用参数列表 `(x)`调用所选运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-508">The selected operator is invoked with the argument list `(x)`.</span></span> | 
|                     | `x + y`      | <span data-ttu-id="a1735-509">重载决策适用于在由 `x` 类型或 `y`类型给定的类或结构中选择最佳的二进制运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-509">Overload resolution is applied to select the best binary operator in the classes or structs given by the types of `x` and `y`.</span></span> <span data-ttu-id="a1735-510">用参数列表 `(x,y)`调用所选运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-510">The selected operator is invoked with the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="a1735-511">实例构造函数调用</span><span class="sxs-lookup"><span data-stu-id="a1735-511">Instance constructor invocation</span></span> | `new T(x,y)` | <span data-ttu-id="a1735-512">重载决策适用于在类或结构 `T`中选择最佳实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="a1735-512">Overload resolution is applied to select the best instance constructor in the class or struct `T`.</span></span> <span data-ttu-id="a1735-513">实例构造函数用参数列表 `(x,y)`调用。</span><span class="sxs-lookup"><span data-stu-id="a1735-513">The instance constructor is invoked with the argument list `(x,y)`.</span></span> | 

### <a name="argument-lists"></a><span data-ttu-id="a1735-514">参数列表</span><span class="sxs-lookup"><span data-stu-id="a1735-514">Argument lists</span></span>

<span data-ttu-id="a1735-515">每个函数成员和委托调用都包含一个参数列表，该列表为函数成员的参数提供实际值或变量引用。</span><span class="sxs-lookup"><span data-stu-id="a1735-515">Every function member and delegate invocation includes an argument list which provides actual values or variable references for the parameters of the function member.</span></span> <span data-ttu-id="a1735-516">指定函数成员调用的参数列表的语法取决于函数成员类别：</span><span class="sxs-lookup"><span data-stu-id="a1735-516">The syntax for specifying the argument list of a function member invocation depends on the function member category:</span></span>

*  <span data-ttu-id="a1735-517">对于实例构造函数、方法、索引器和委托，参数被指定为*argument_list*，如下所述。</span><span class="sxs-lookup"><span data-stu-id="a1735-517">For instance constructors, methods, indexers and delegates, the arguments are specified as an *argument_list*, as described below.</span></span> <span data-ttu-id="a1735-518">对于索引器，调用 `set` 访问器时，自变量列表还包括指定为赋值运算符的右操作数的表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-518">For indexers, when invoking the `set` accessor, the argument list additionally includes the expression specified as the right operand of the assignment operator.</span></span>
*  <span data-ttu-id="a1735-519">对于属性，调用 `get` 访问器时，参数列表是空的，并且包含在调用 `set` 访问器时指定为赋值运算符的右操作数的表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-519">For properties, the argument list is empty when invoking the `get` accessor, and consists of the expression specified as the right operand of the assignment operator when invoking the `set` accessor.</span></span>
*  <span data-ttu-id="a1735-520">对于事件，参数列表包含指定为 `+=` 或 `-=` 运算符的右操作数的表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-520">For events, the argument list consists of the expression specified as the right operand of the `+=` or `-=` operator.</span></span>
*  <span data-ttu-id="a1735-521">对于用户定义的运算符，参数列表包含一元运算符的单个操作数或二元运算符的两个操作数。</span><span class="sxs-lookup"><span data-stu-id="a1735-521">For user-defined operators, the argument list consists of the single operand of the unary operator or the two operands of the binary operator.</span></span>

<span data-ttu-id="a1735-522">属性（[属性](classes.md#properties)）、事件（[事件](classes.md#events)）和用户定义的运算符（[运算符](classes.md#operators)）的参数始终作为值参数传递（[值参数](classes.md#value-parameters)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-522">The arguments of properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), and user-defined operators ([Operators](classes.md#operators)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)).</span></span> <span data-ttu-id="a1735-523">索引器（[索引器](classes.md#indexers)）的参数始终作为值参数（[值参数](classes.md#value-parameters)）或参数数组（[参数数组](classes.md#parameter-arrays)）传递。</span><span class="sxs-lookup"><span data-stu-id="a1735-523">The arguments of indexers ([Indexers](classes.md#indexers)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)) or parameter arrays ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="a1735-524">这些类别的函数成员不支持 Reference 和 output 参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-524">Reference and output parameters are not supported for these categories of function members.</span></span>

<span data-ttu-id="a1735-525">实例构造函数、方法、索引器或委托调用的参数指定为*argument_list*：</span><span class="sxs-lookup"><span data-stu-id="a1735-525">The arguments of an instance constructor, method, indexer or delegate invocation are specified as an *argument_list*:</span></span>

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

<span data-ttu-id="a1735-526">*Argument_list*由一个或多个由逗号分隔的*参数*组成。</span><span class="sxs-lookup"><span data-stu-id="a1735-526">An *argument_list* consists of one or more *argument*s, separated by commas.</span></span> <span data-ttu-id="a1735-527">每个自变量都包含一个可选的*argument_name*后跟一个*argument_value*。</span><span class="sxs-lookup"><span data-stu-id="a1735-527">Each argument consists of an optional  *argument_name* followed by an *argument_value*.</span></span> <span data-ttu-id="a1735-528">带有*argument_name*的*参数*称为***命名参数***，而没有*argument_name*的*参数*是***位置参数***。</span><span class="sxs-lookup"><span data-stu-id="a1735-528">An *argument* with an *argument_name* is referred to as a ***named argument***, whereas an *argument* without an *argument_name* is a ***positional argument***.</span></span> <span data-ttu-id="a1735-529">在*argument_list*中的命名参数后，位置参数出现错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-529">It is an error for a positional argument to appear after a named argument in an *argument_list*.</span></span>

<span data-ttu-id="a1735-530">*Argument_value*可以采用以下形式之一：</span><span class="sxs-lookup"><span data-stu-id="a1735-530">The *argument_value* can take one of the following forms:</span></span>

*  <span data-ttu-id="a1735-531">一个*表达式*，指示参数作为值参数传递（[值参数](classes.md#value-parameters)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-531">An *expression*, indicating that the argument is passed as a value parameter ([Value parameters](classes.md#value-parameters)).</span></span>
*  <span data-ttu-id="a1735-532">关键字 `ref` 后跟*variable_reference* （[变量引用](variables.md#variable-references)），指示参数作为引用参数（[引用参数](classes.md#reference-parameters)）传递。</span><span class="sxs-lookup"><span data-stu-id="a1735-532">The keyword `ref` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as a reference parameter ([Reference parameters](classes.md#reference-parameters)).</span></span> <span data-ttu-id="a1735-533">在将变量作为引用参数传递之前，必须对其进行明确赋值（[明确赋值](variables.md#definite-assignment)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-533">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter.</span></span> <span data-ttu-id="a1735-534">关键字 `out` 后跟*variable_reference* （[变量引用](variables.md#variable-references)），指示参数作为输出参数传递（[输出参数](classes.md#output-parameters)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-534">The keyword `out` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as an output parameter ([Output parameters](classes.md#output-parameters)).</span></span> <span data-ttu-id="a1735-535">在将变量作为 output 参数传递的函数成员调用之后，变量被视为明确赋值（[明确赋值](variables.md#definite-assignment)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-535">A variable is considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) following a function member invocation in which the variable is passed as an output parameter.</span></span>

#### <a name="corresponding-parameters"></a><span data-ttu-id="a1735-536">对应的参数</span><span class="sxs-lookup"><span data-stu-id="a1735-536">Corresponding parameters</span></span>

<span data-ttu-id="a1735-537">对于参数列表中的每个参数，必须在要调用的函数成员或委托中包含相应的参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-537">For each argument in an argument list there has to be a corresponding parameter in the function member or delegate being invoked.</span></span>

<span data-ttu-id="a1735-538">以下内容中使用的参数列表的确定方式如下：</span><span class="sxs-lookup"><span data-stu-id="a1735-538">The parameter list used in the following is determined as follows:</span></span>

*  <span data-ttu-id="a1735-539">对于在类中定义的虚拟方法和索引器，将从函数成员的最特定声明或重写中选取参数列表，从接收方的静态类型开始，然后搜索其基类。</span><span class="sxs-lookup"><span data-stu-id="a1735-539">For virtual methods and indexers defined in classes, the parameter list is picked from the most specific declaration or override of the function member, starting with the static type of the receiver, and searching through its base classes.</span></span>
*  <span data-ttu-id="a1735-540">对于接口方法和索引器，从接口类型和搜索基接口开始，选取参数列表作为成员的最明确定义。</span><span class="sxs-lookup"><span data-stu-id="a1735-540">For interface methods and indexers, the parameter list is picked form the most specific definition of the member, starting with the interface type and searching through the base interfaces.</span></span> <span data-ttu-id="a1735-541">如果找不到唯一的参数列表，则将构造一个具有不可访问的名称且不带可选参数的参数列表，以便调用不能使用命名参数或省略可选参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-541">If no unique parameter list is found, a parameter list with inaccessible names and no optional parameters is constructed, so that invocations cannot use named parameters or omit optional arguments.</span></span>
*  <span data-ttu-id="a1735-542">对于分部方法，使用定义分部方法声明的参数列表。</span><span class="sxs-lookup"><span data-stu-id="a1735-542">For partial methods, the parameter list of the defining partial method declaration is used.</span></span>
*  <span data-ttu-id="a1735-543">对于所有其他函数成员和委托，只有一个参数列表，该列表是使用的。</span><span class="sxs-lookup"><span data-stu-id="a1735-543">For all other function members and delegates there is only a single parameter list, which is the one used.</span></span>

<span data-ttu-id="a1735-544">参数或参数的位置定义为参数列表或参数列表中其前面的参数或参数的数目。</span><span class="sxs-lookup"><span data-stu-id="a1735-544">The position of an argument or parameter is defined as the number of arguments or parameters preceding it in the argument list or parameter list.</span></span>

<span data-ttu-id="a1735-545">按如下所示建立函数成员参数的对应参数：</span><span class="sxs-lookup"><span data-stu-id="a1735-545">The corresponding parameters for function member arguments are established as follows:</span></span>

*  <span data-ttu-id="a1735-546">实例构造函数、方法、索引器和委托的*argument_list*中的参数：</span><span class="sxs-lookup"><span data-stu-id="a1735-546">Arguments in the *argument_list* of instance constructors, methods, indexers and delegates:</span></span>
    * <span data-ttu-id="a1735-547">一个位置参数，其中固定参数出现在参数列表中的同一位置，该参数对应于该参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-547">A positional argument where a fixed parameter occurs at the same position in the parameter list corresponds to that parameter.</span></span>
    * <span data-ttu-id="a1735-548">函数成员的位置自变量，其正常形式中调用的参数数组对应于参数数组，该参数数组必须出现在参数列表中的同一位置。</span><span class="sxs-lookup"><span data-stu-id="a1735-548">A positional argument of a function member with a parameter array invoked in its normal form corresponds to the parameter  array, which must occur at the same position in the parameter list.</span></span>
    * <span data-ttu-id="a1735-549">函数成员的位置自变量，其中的参数数组以其展开形式调用，其中没有固定参数出现在参数列表中的同一位置，它对应于参数数组中的一个元素。</span><span class="sxs-lookup"><span data-stu-id="a1735-549">A positional argument of a function member with a parameter array invoked in its expanded form, where no fixed parameter occurs at the same position in the parameter list, corresponds to an element in the parameter array.</span></span>
    * <span data-ttu-id="a1735-550">命名参数对应于参数列表中具有相同名称的参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-550">A named argument corresponds to the parameter of the same name in the parameter list.</span></span>
    * <span data-ttu-id="a1735-551">对于索引器，调用 `set` 访问器时，指定为赋值运算符的右操作数的表达式对应于 `set` 访问器声明的隐式 `value` 参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-551">For indexers, when invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="a1735-552">对于属性，当调用 `get` 访问器时，没有参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-552">For properties, when invoking the `get` accessor there are no arguments.</span></span> <span data-ttu-id="a1735-553">调用 `set` 访问器时，指定为赋值运算符的右操作数的表达式对应于 `set` 访问器声明的隐式 `value` 参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-553">When invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="a1735-554">对于用户定义的一元运算符（包括转换），单个操作数对应于运算符声明的单个参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-554">For user-defined unary operators (including conversions), the single operand corresponds to the single parameter of the operator declaration.</span></span>
*  <span data-ttu-id="a1735-555">对于用户定义的二进制运算符，左操作数对应于第一个参数，右侧操作数对应于运算符声明的第二个参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-555">For user-defined binary operators, the left operand corresponds to the first parameter, and the right operand corresponds to the second parameter of the operator declaration.</span></span>

#### <a name="run-time-evaluation-of-argument-lists"></a><span data-ttu-id="a1735-556">参数列表的运行时计算</span><span class="sxs-lookup"><span data-stu-id="a1735-556">Run-time evaluation of argument lists</span></span>

<span data-ttu-id="a1735-557">在运行时处理函数成员调用期间（对[动态重载决策进行编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)），将按从左至右的顺序计算参数列表的表达式或变量引用，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a1735-557">During the run-time processing of a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), the expressions or variable references of an argument list are evaluated in order, from left to right, as follows:</span></span>

*  <span data-ttu-id="a1735-558">对于值参数，将计算参数表达式，并执行到相应参数类型的隐式转换（[隐式](conversions.md#implicit-conversions)转换）。</span><span class="sxs-lookup"><span data-stu-id="a1735-558">For a value parameter, the argument expression is evaluated and an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to the corresponding parameter type is performed.</span></span> <span data-ttu-id="a1735-559">生成的值将成为函数成员调用中 value 参数的初始值。</span><span class="sxs-lookup"><span data-stu-id="a1735-559">The resulting value becomes the initial value of the value parameter in the function member invocation.</span></span>
*  <span data-ttu-id="a1735-560">对于引用参数或输出参数，将对变量引用进行计算，并将生成的存储位置成为函数成员调用中的参数所表示的存储位置。</span><span class="sxs-lookup"><span data-stu-id="a1735-560">For a reference or output parameter, the variable reference is evaluated and the resulting storage location becomes the storage location represented by the parameter in the function member invocation.</span></span> <span data-ttu-id="a1735-561">如果作为引用或输出参数提供的变量引用是*reference_type*的数组元素，则执行运行时检查以确保数组的元素类型与参数的类型相同。</span><span class="sxs-lookup"><span data-stu-id="a1735-561">If the variable reference given as a reference or output parameter is an array element of a *reference_type*, a run-time check is performed to ensure that the element type of the array is identical to the type of the parameter.</span></span> <span data-ttu-id="a1735-562">如果此检查失败，则会引发 `System.ArrayTypeMismatchException`。</span><span class="sxs-lookup"><span data-stu-id="a1735-562">If this check fails, a `System.ArrayTypeMismatchException` is thrown.</span></span>

<span data-ttu-id="a1735-563">方法、索引器和实例构造函数可以将其最右侧的参数声明为参数数组（[参数](classes.md#parameter-arrays)数组）。</span><span class="sxs-lookup"><span data-stu-id="a1735-563">Methods, indexers, and instance constructors may declare their right-most parameter to be a parameter array ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="a1735-564">此类函数成员的调用方式可以是普通形式，也可以是其展开形式，具体取决于适用的（[适用的函数成员](expressions.md#applicable-function-member)）：</span><span class="sxs-lookup"><span data-stu-id="a1735-564">Such function members are invoked either in their normal form or in their expanded form depending on which is applicable ([Applicable function member](expressions.md#applicable-function-member)):</span></span>

*  <span data-ttu-id="a1735-565">当使用参数数组的函数成员以其正常形式调用时，为参数数组提供的参数必须是可隐[式转换为](conversions.md#implicit-conversions)参数数组类型的单个表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-565">When a function member with a parameter array is invoked in its normal form, the argument given for the parameter array must be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="a1735-566">在这种情况下，参数数组的作用与值参数完全相同。</span><span class="sxs-lookup"><span data-stu-id="a1735-566">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="a1735-567">当具有参数数组的函数成员以其展开形式调用时，该调用必须为参数数组指定零个或多个位置参数，其中每个参数都是可隐[式转换为](conversions.md#implicit-conversions)参数数组元素类型的表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-567">When a function member with a parameter array is invoked in its expanded form, the invocation must specify zero or more positional arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="a1735-568">在这种情况下，调用会创建一个参数数组类型的实例，该实例的长度与参数的数量相对应，使用给定的参数值初始化数组实例的元素，并使用新创建的数组实例作为实际实际.</span><span class="sxs-lookup"><span data-stu-id="a1735-568">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="a1735-569">自变量列表的表达式始终按写入它们的顺序进行计算。</span><span class="sxs-lookup"><span data-stu-id="a1735-569">The expressions of an argument list are always evaluated in the order they are written.</span></span> <span data-ttu-id="a1735-570">因此，示例</span><span class="sxs-lookup"><span data-stu-id="a1735-570">Thus, the example</span></span>
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
<span data-ttu-id="a1735-571">生成输出</span><span class="sxs-lookup"><span data-stu-id="a1735-571">produces the output</span></span>
```console
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

<span data-ttu-id="a1735-572">如果存在从 `B` 到 `A`的隐式引用转换，数组协方差规则（[数组协方](arrays.md#array-covariance)差）允许 `A[]` 为数组类型 `B[]`的实例的引用。</span><span class="sxs-lookup"><span data-stu-id="a1735-572">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="a1735-573">由于这些规则，当*reference_type*的数组元素作为引用或输出参数传递时，需要运行时检查以确保数组的实际元素类型与参数的类型相同。</span><span class="sxs-lookup"><span data-stu-id="a1735-573">Because of these rules, when an array element of a *reference_type* is passed as a reference or output parameter, a run-time check is required to ensure that the actual element type of the array is identical to that of the parameter.</span></span> <span data-ttu-id="a1735-574">示例中</span><span class="sxs-lookup"><span data-stu-id="a1735-574">In the example</span></span>
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
<span data-ttu-id="a1735-575">`F` 的第二次调用导致引发 `System.ArrayTypeMismatchException`，因为 `b` 的实际元素类型为 `string` 而不是 `object`。</span><span class="sxs-lookup"><span data-stu-id="a1735-575">the second invocation of `F` causes a `System.ArrayTypeMismatchException` to be thrown because the actual element type of `b` is `string` and not `object`.</span></span>

<span data-ttu-id="a1735-576">当具有参数数组的函数成员以其展开形式调用时，该调用的处理方式与使用数组初始值设定项（[数组创建表达式](expressions.md#array-creation-expressions)）的数组创建表达式在展开的参数周围插入的方式完全相同。</span><span class="sxs-lookup"><span data-stu-id="a1735-576">When a function member with a parameter array is invoked in its expanded form, the invocation is processed exactly as if an array creation expression with an array initializer ([Array creation expressions](expressions.md#array-creation-expressions)) was inserted around the expanded parameters.</span></span> <span data-ttu-id="a1735-577">例如，给定声明</span><span class="sxs-lookup"><span data-stu-id="a1735-577">For example, given the declaration</span></span>
```csharp
void F(int x, int y, params object[] args);
```
<span data-ttu-id="a1735-578">对方法的展开形式的以下调用</span><span class="sxs-lookup"><span data-stu-id="a1735-578">the following invocations of the expanded form of the method</span></span>
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
<span data-ttu-id="a1735-579">完全对应于</span><span class="sxs-lookup"><span data-stu-id="a1735-579">correspond exactly to</span></span>
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

<span data-ttu-id="a1735-580">特别要注意的是，如果参数数组提供的参数为零，则将创建一个空数组。</span><span class="sxs-lookup"><span data-stu-id="a1735-580">In particular, note that an empty array is created when there are zero arguments given for the parameter array.</span></span>

<span data-ttu-id="a1735-581">当使用相应的可选参数从函数成员中省略参数时，将隐式传递函数成员声明的默认参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-581">When arguments are omitted from a function member with corresponding optional parameters, the default arguments of the function member declaration are implicitly passed.</span></span> <span data-ttu-id="a1735-582">因为这些始终是常量，所以其计算不会影响剩余参数的计算顺序。</span><span class="sxs-lookup"><span data-stu-id="a1735-582">Because these are always constant, their evaluation will not impact the evaluation order of the remaining arguments.</span></span>

### <a name="type-inference"></a><span data-ttu-id="a1735-583">类型推理</span><span class="sxs-lookup"><span data-stu-id="a1735-583">Type inference</span></span>

<span data-ttu-id="a1735-584">如果在未指定类型实参的情况下调用泛型方法，则***类型推理***进程将尝试推断调用的类型参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-584">When a generic method is called without specifying type arguments, a ***type inference*** process attempts to infer type arguments for the call.</span></span> <span data-ttu-id="a1735-585">类型推理的存在允许使用更方便的语法来调用泛型方法，并允许程序员避免指定冗余类型信息。</span><span class="sxs-lookup"><span data-stu-id="a1735-585">The presence of type inference allows a more convenient syntax to be used for calling a generic method, and allows the programmer to avoid specifying redundant type information.</span></span> <span data-ttu-id="a1735-586">例如，给定方法声明：</span><span class="sxs-lookup"><span data-stu-id="a1735-586">For example, given the method declaration:</span></span>
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
<span data-ttu-id="a1735-587">无需显式指定类型参数即可调用 `Choose` 方法：</span><span class="sxs-lookup"><span data-stu-id="a1735-587">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

<span data-ttu-id="a1735-588">通过类型推理，`int` 和 `string` 的类型参数由方法的自变量确定。</span><span class="sxs-lookup"><span data-stu-id="a1735-588">Through type inference, the type arguments `int` and `string` are determined from the arguments to the method.</span></span>

<span data-ttu-id="a1735-589">类型推理作为方法调用（[方法调用](expressions.md#method-invocations)）的绑定时处理的一部分发生，并在调用的重载解析步骤之前发生。</span><span class="sxs-lookup"><span data-stu-id="a1735-589">Type inference occurs as part of the binding-time processing of a method invocation ([Method invocations](expressions.md#method-invocations)) and takes place before the overload resolution step of the invocation.</span></span> <span data-ttu-id="a1735-590">如果在方法调用中指定了特定方法组，但没有将任何类型参数指定为方法调用的一部分，则会将类型推理应用于方法组中的每个泛型方法。</span><span class="sxs-lookup"><span data-stu-id="a1735-590">When a particular method group is specified in a method invocation, and no type arguments are specified as part of the method invocation, type inference is applied to each generic method in the method group.</span></span> <span data-ttu-id="a1735-591">如果类型推理成功，则使用推断出的类型参数来确定参数的类型，以供后续重载决策使用。</span><span class="sxs-lookup"><span data-stu-id="a1735-591">If type inference succeeds, then the inferred type arguments are used to determine the types of arguments for subsequent overload resolution.</span></span> <span data-ttu-id="a1735-592">如果重载决策选择泛型方法作为要调用的方法，则会将推断的类型参数用作调用的实际类型参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-592">If overload resolution chooses a generic method as the one to invoke, then the inferred type arguments are used as the actual type arguments for the invocation.</span></span> <span data-ttu-id="a1735-593">如果特定方法的类型推理失败，则该方法将不参与重载决策。</span><span class="sxs-lookup"><span data-stu-id="a1735-593">If type inference for a particular method fails, that method does not participate in overload resolution.</span></span> <span data-ttu-id="a1735-594">类型推理的失败本身不会导致绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-594">The failure of type inference, in and of itself, does not cause a binding-time error.</span></span> <span data-ttu-id="a1735-595">但是，当重载决策失败并找不到任何适用的方法时，通常会导致绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-595">However, it often leads to a binding-time error when overload resolution then fails to find any applicable methods.</span></span>

<span data-ttu-id="a1735-596">如果所提供的参数数目不同于方法中的参数数量，则推理会立即失败。</span><span class="sxs-lookup"><span data-stu-id="a1735-596">If the supplied number of arguments is different than the number of parameters in the method, then inference immediately fails.</span></span> <span data-ttu-id="a1735-597">否则，假定泛型方法具有以下签名：</span><span class="sxs-lookup"><span data-stu-id="a1735-597">Otherwise, assume that the generic method has the following signature:</span></span>
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

<span data-ttu-id="a1735-598">使用窗体的方法调用 `M(E1...Em)` 类型推理的任务是查找 `S1...Sn` 的每个类型参数的唯一类型参数 `X1...Xn` 以便调用 `M<S1...Sn>(E1...Em)` 变为有效。</span><span class="sxs-lookup"><span data-stu-id="a1735-598">With a method call of the form `M(E1...Em)` the task of type inference is to find unique type arguments `S1...Sn` for each of the type parameters `X1...Xn` so that the call `M<S1...Sn>(E1...Em)` becomes valid.</span></span>

<span data-ttu-id="a1735-599">在推断过程中，每个类型参数 `Xi` 都*固定*到特定类型 `Si` 或未*使用关联*的*界限*集进行固定。</span><span class="sxs-lookup"><span data-stu-id="a1735-599">During the process of inference each type parameter `Xi` is either *fixed* to a particular type `Si` or *unfixed* with an associated set of *bounds*.</span></span> <span data-ttu-id="a1735-600">每个边界都是 `T`的一种类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-600">Each of the bounds is some type `T`.</span></span> <span data-ttu-id="a1735-601">最初，每个类型变量 `Xi` 都不包含一组空的界限。</span><span class="sxs-lookup"><span data-stu-id="a1735-601">Initially each type variable `Xi` is unfixed with an empty set of bounds.</span></span>

<span data-ttu-id="a1735-602">类型推理分阶段发生。</span><span class="sxs-lookup"><span data-stu-id="a1735-602">Type inference takes place in phases.</span></span> <span data-ttu-id="a1735-603">每个阶段都将尝试根据上一阶段的发现来推导更多类型变量的类型参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-603">Each phase will try to infer type arguments for more type variables based on the findings of the previous phase.</span></span> <span data-ttu-id="a1735-604">第一阶段会对边界进行一些初始推断，而第二阶段则将类型变量修复为特定类型，并推导出进一步的边界。</span><span class="sxs-lookup"><span data-stu-id="a1735-604">The first phase makes some initial inferences of bounds, whereas the second phase fixes type variables to specific types and infers further bounds.</span></span> <span data-ttu-id="a1735-605">第二阶段可能需要重复一次。</span><span class="sxs-lookup"><span data-stu-id="a1735-605">The second phase may have to be repeated a number of times.</span></span>

<span data-ttu-id="a1735-606">*注意：* 仅当调用泛型方法时，才会发生类型推理。</span><span class="sxs-lookup"><span data-stu-id="a1735-606">*Note:* Type inference takes place not only when a generic method is called.</span></span> <span data-ttu-id="a1735-607">用于转换方法组的类型推理中介绍了用于转换[方法](expressions.md#type-inference-for-conversion-of-method-groups)组的类型推理和查找一组表达式的最佳通用[类型中介绍的方法。](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)</span><span class="sxs-lookup"><span data-stu-id="a1735-607">Type inference for conversion of method groups is described in [Type inference for conversion of method groups](expressions.md#type-inference-for-conversion-of-method-groups) and finding the best common type of a set of expressions is described in [Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span></span>

#### <a name="the-first-phase"></a><span data-ttu-id="a1735-608">第一阶段</span><span class="sxs-lookup"><span data-stu-id="a1735-608">The first phase</span></span>

<span data-ttu-id="a1735-609">对于每个方法参数 `Ei`：</span><span class="sxs-lookup"><span data-stu-id="a1735-609">For each of the method arguments `Ei`:</span></span>

*   <span data-ttu-id="a1735-610">如果 `Ei` 是匿名函数，则*显式参数类型推理*（[显式参数类型推断](expressions.md#explicit-parameter-type-inferences)）是从 `Ei` 到 `Ti`</span><span class="sxs-lookup"><span data-stu-id="a1735-610">If `Ei` is an anonymous function, an *explicit parameter type inference* ([Explicit parameter type inferences](expressions.md#explicit-parameter-type-inferences)) is made from `Ei` to `Ti`</span></span>
*   <span data-ttu-id="a1735-611">否则，如果 `Ei` 具有类型 `U` 并且 `xi` 是值参数，则将*从*`U`*到*`Ti`进行*下限推理*。</span><span class="sxs-lookup"><span data-stu-id="a1735-611">Otherwise, if `Ei` has a type `U` and `xi` is a value parameter then a *lower-bound inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="a1735-612">否则，如果 `Ei` 具有类型 `U` 并且 `xi` 是 `ref` 或 `out` 参数，则将*从*`U`*到*`Ti`进行*精确推断*。</span><span class="sxs-lookup"><span data-stu-id="a1735-612">Otherwise, if `Ei` has a type `U` and `xi` is a `ref` or `out` parameter then an *exact inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="a1735-613">否则，不会对此参数进行推理。</span><span class="sxs-lookup"><span data-stu-id="a1735-613">Otherwise, no inference is made for this argument.</span></span>


#### <a name="the-second-phase"></a><span data-ttu-id="a1735-614">第二阶段</span><span class="sxs-lookup"><span data-stu-id="a1735-614">The second phase</span></span>

<span data-ttu-id="a1735-615">第二个阶段按如下方式进行：</span><span class="sxs-lookup"><span data-stu-id="a1735-615">The second phase proceeds as follows:</span></span>

*   <span data-ttu-id="a1735-616">不*依赖于*（[依赖](expressions.md#dependence)）任何 `Xj`*的所有未固定的*类型 `Xi` 变量都是固定的（正在[修复](expressions.md#fixing)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-616">All *unfixed* type variables `Xi` which do not *depend on* ([Dependence](expressions.md#dependence)) any `Xj` are fixed ([Fixing](expressions.md#fixing)).</span></span>
*   <span data-ttu-id="a1735-617">如果不存在这样的类型变量，则所有*未固定的类型变量*`Xi` 都是*固定*的，其中所有的保留都是：</span><span class="sxs-lookup"><span data-stu-id="a1735-617">If no such type variables exist, all *unfixed* type variables `Xi` are *fixed* for which all of the following hold:</span></span>
    *   <span data-ttu-id="a1735-618">至少有一个类型变量 `Xj` 依赖于 `Xi`</span><span class="sxs-lookup"><span data-stu-id="a1735-618">There is at least one type variable `Xj` that depends on `Xi`</span></span>
    *   <span data-ttu-id="a1735-619">`Xi` 具有非空的界限集</span><span class="sxs-lookup"><span data-stu-id="a1735-619">`Xi` has a non-empty set of bounds</span></span>
*   <span data-ttu-id="a1735-620">如果不存在这样的类型变量，并且仍有未*固定*的类型变量，则类型推理将失败。</span><span class="sxs-lookup"><span data-stu-id="a1735-620">If no such type variables exist and there are still *unfixed* type variables, type inference fails.</span></span>
*   <span data-ttu-id="a1735-621">否则，如果不存在进一步的未*固定*类型变量，则类型推理将成功。</span><span class="sxs-lookup"><span data-stu-id="a1735-621">Otherwise, if no further *unfixed* type variables exist, type inference succeeds.</span></span>
*   <span data-ttu-id="a1735-622">否则，对于具有相应参数类型的所有参数 `Ei` `Ti` 其中*输出类型*（[输出类型](expressions.md#output-types)）包含未*固定*的类型变量 `Xj` 但*输入类型*（[输入类型](expressions.md#input-types)）不存在，则将*从*`Ei`[Output type inferences](expressions.md#output-type-inferences)*到*`Ti`进行*输出类型推理*。</span><span class="sxs-lookup"><span data-stu-id="a1735-622">Otherwise, for all arguments `Ei` with corresponding parameter type `Ti` where the *output types* ([Output types](expressions.md#output-types)) contain *unfixed* type variables `Xj` but the *input types* ([Input types](expressions.md#input-types)) do not, an *output type inference* ([Output type inferences](expressions.md#output-type-inferences)) is made *from* `Ei` *to* `Ti`.</span></span> <span data-ttu-id="a1735-623">然后，重复第二阶段。</span><span class="sxs-lookup"><span data-stu-id="a1735-623">Then the second phase is repeated.</span></span>

#### <a name="input-types"></a><span data-ttu-id="a1735-624">输入类型</span><span class="sxs-lookup"><span data-stu-id="a1735-624">Input types</span></span>

<span data-ttu-id="a1735-625">如果 `E` 是方法组或隐式类型的匿名函数，并且 `T` 为委托类型或表达式树类型，则 `T` 的所有参数类型均为*类型*为 `T`的 `E` 的*输入类型*。</span><span class="sxs-lookup"><span data-stu-id="a1735-625">If `E` is a method group or implicitly typed anonymous function and `T` is a delegate type or expression tree type then all the parameter types of `T` are *input types* of `E` *with type* `T`.</span></span>

####  <a name="output-types"></a><span data-ttu-id="a1735-626">输出类型</span><span class="sxs-lookup"><span data-stu-id="a1735-626">Output types</span></span>

<span data-ttu-id="a1735-627">如果 `E` 为方法组或匿名函数，并且 `T` 为委托类型或表达式树类型，则 `T` 的返回类型为*类型*为 `T`的 `E` 的*输出类型*。</span><span class="sxs-lookup"><span data-stu-id="a1735-627">If `E` is a method group or an anonymous function and `T` is a delegate type or expression tree type then the return type of `T` is an *output type of* `E` *with type* `T`.</span></span>

#### <a name="dependence"></a><span data-ttu-id="a1735-628">依赖关系</span><span class="sxs-lookup"><span data-stu-id="a1735-628">Dependence</span></span>

<span data-ttu-id="a1735-629">未*固定的类型*变量 `Xi`*直接依赖于*未固定的类型变量 `Xj` 如果对于具有类型 `Xj` 的某些参数 `Tk` `Ek`，`Ek`*在类型为*`Tk` 的输出类型为 `Xi` 的*输出类型*中发生。</span><span class="sxs-lookup"><span data-stu-id="a1735-629">An *unfixed* type variable `Xi` *depends directly on* an unfixed type variable `Xj` if for some argument `Ek` with type `Tk` `Xj` occurs in an *input type* of `Ek` with type `Tk` and `Xi` occurs in an *output type* of `Ek` with type `Tk`.</span></span>

<span data-ttu-id="a1735-630">如果 `Xj`*直接依赖于*`Xi` 或者 `Xi`*直接*依赖于 `Xk` 并且 `Xk`*依赖于*`Xj`，则 `Xj`*依赖于*`Xi`。</span><span class="sxs-lookup"><span data-stu-id="a1735-630">`Xj` *depends on* `Xi` if `Xj` *depends directly on* `Xi` or if `Xi` *depends directly on* `Xk` and `Xk` *depends on* `Xj`.</span></span> <span data-ttu-id="a1735-631">因此 "依赖于" 是传递的，而不是 "直接依赖于" 的反身闭包。</span><span class="sxs-lookup"><span data-stu-id="a1735-631">Thus "depends on" is the transitive but not reflexive closure of "depends directly on".</span></span>

#### <a name="output-type-inferences"></a><span data-ttu-id="a1735-632">输出类型推断</span><span class="sxs-lookup"><span data-stu-id="a1735-632">Output type inferences</span></span>

<span data-ttu-id="a1735-633">*输出类型推断*是通过以下方式*从*`E`*到*类型 `T` 的表达式进行的：</span><span class="sxs-lookup"><span data-stu-id="a1735-633">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="a1735-634">如果 `E` 是 `U` （[推断返回类型](expressions.md#inferred-return-type)）且 `T` 为委托类型或具有返回类型 `Tb`的表达式树类型的匿名函数，则将*从*`U`[Lower-bound inferences](expressions.md#lower-bound-inferences)*到*`Tb`进行*下限推理*。</span><span class="sxs-lookup"><span data-stu-id="a1735-634">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="a1735-635">否则，如果 `E` 为方法组，`T` 是参数类型为 `T1...Tk` 和返回类型 `Tb`的委托类型或表达式树类型，并且 `E` 生成一个返回类型 `T1...Tk` 的单个方法，则*从*`U`*到*`U`*生成一个方法*。</span><span class="sxs-lookup"><span data-stu-id="a1735-635">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="a1735-636">否则，如果 `E` 是 `U`类型的表达式，则将*从*`U`*到*`T`进行*下限推理*。</span><span class="sxs-lookup"><span data-stu-id="a1735-636">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
*  <span data-ttu-id="a1735-637">否则，不进行推断。</span><span class="sxs-lookup"><span data-stu-id="a1735-637">Otherwise, no inferences are made.</span></span>

#### <a name="explicit-parameter-type-inferences"></a><span data-ttu-id="a1735-638">显式参数类型推断</span><span class="sxs-lookup"><span data-stu-id="a1735-638">Explicit parameter type inferences</span></span>

<span data-ttu-id="a1735-639">*显式参数类型推理*是通过以下方式*从*`E`*到*类型 `T` 的表达式进行的：</span><span class="sxs-lookup"><span data-stu-id="a1735-639">An *explicit parameter type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="a1735-640">如果 `E`@no__t 为 @no__t 的参数类型`U1...Uk`为-1 的显式类型匿名函数，`T` 是具有参数类型 `V1...Vk` 的委托类型或表达式树类型，则对每个 `Ui` 进行精确推理（[精确推断](expressions.md#exact-inferences)）从到对应的 0。`Ui``Vi`</span><span class="sxs-lookup"><span data-stu-id="a1735-640">If `E` is an explicitly typed anonymous function with parameter types `U1...Uk` and `T` is a delegate type or expression tree type with parameter types `V1...Vk` then for each `Ui` an *exact inference* ([Exact inferences](expressions.md#exact-inferences)) is made *from* `Ui` *to* the corresponding `Vi`.</span></span>

#### <a name="exact-inferences"></a><span data-ttu-id="a1735-641">精确推断</span><span class="sxs-lookup"><span data-stu-id="a1735-641">Exact inferences</span></span>

<span data-ttu-id="a1735-642">*从*类型 `U`*到*类型 `V` 的*精确推理*如下所示：</span><span class="sxs-lookup"><span data-stu-id="a1735-642">An *exact inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="a1735-643">如果 `V` 是未*固定*的 `Xi` 之一，则 `U` 添加到 `Xi`的完全限定集。</span><span class="sxs-lookup"><span data-stu-id="a1735-643">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of exact bounds for `Xi`.</span></span>

*  <span data-ttu-id="a1735-644">否则，将通过检查是否存在以下任何情况来确定 `V1...Vk` 和 `U1...Uk`：</span><span class="sxs-lookup"><span data-stu-id="a1735-644">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>

   *  <span data-ttu-id="a1735-645">`V` 是 `V1[...]` 数组类型，`U` 是同一排名 `U1[...]` 的数组类型</span><span class="sxs-lookup"><span data-stu-id="a1735-645">`V` is an array type `V1[...]` and `U` is an array type `U1[...]`  of the same rank</span></span>
   *  <span data-ttu-id="a1735-646">`V` 是 `V1?` 类型，`U` 为类型 `U1?`</span><span class="sxs-lookup"><span data-stu-id="a1735-646">`V` is the type `V1?` and `U` is the type `U1?`</span></span>
   *  <span data-ttu-id="a1735-647">`V` 是构造类型 `C<V1...Vk>`并且 `U` 为构造类型 `C<U1...Uk>`</span><span class="sxs-lookup"><span data-stu-id="a1735-647">`V` is a constructed type `C<V1...Vk>`and `U` is a constructed type `C<U1...Uk>`</span></span>

   <span data-ttu-id="a1735-648">如果上述任何情况都适用，则将*从*每个 `Ui`*到*相应的 `Vi`进行*确切的推理*。</span><span class="sxs-lookup"><span data-stu-id="a1735-648">If any of these cases apply then an *exact inference* is made *from* each `Ui` *to* the corresponding `Vi`.</span></span>

*  <span data-ttu-id="a1735-649">否则，不进行推断。</span><span class="sxs-lookup"><span data-stu-id="a1735-649">Otherwise no inferences are made.</span></span>

#### <a name="lower-bound-inferences"></a><span data-ttu-id="a1735-650">下限推理</span><span class="sxs-lookup"><span data-stu-id="a1735-650">Lower-bound inferences</span></span>

<span data-ttu-id="a1735-651">*从*类型 `U`*到*类型 `V` 的*下限推理*如下所示：</span><span class="sxs-lookup"><span data-stu-id="a1735-651">A *lower-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="a1735-652">如果 `V` 是未*固定*的 `Xi` 之一，则 `U` 添加到 `Xi`的下限集。</span><span class="sxs-lookup"><span data-stu-id="a1735-652">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of lower bounds for `Xi`.</span></span>
*  <span data-ttu-id="a1735-653">否则，如果 `V` 是类型 `V1?`并且 `U` 为类型 `U1?` 则从 `U1` 到 `V1`进行下限推理。</span><span class="sxs-lookup"><span data-stu-id="a1735-653">Otherwise, if `V` is the type `V1?`and `U` is the type `U1?` then a lower bound inference is made from `U1` to `V1`.</span></span>
*  <span data-ttu-id="a1735-654">否则，将通过检查是否存在以下任何情况来确定 `U1...Uk` 和 `V1...Vk`：</span><span class="sxs-lookup"><span data-stu-id="a1735-654">Otherwise, sets `U1...Uk` and `V1...Vk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="a1735-655">`V` 是 `V1[...]` 数组类型，`U` 是同一排名 `U1[...]` （或其有效基类型为 `U1[...]`的类型参数）的数组类型</span><span class="sxs-lookup"><span data-stu-id="a1735-655">`V` is an array type `V1[...]` and `U` is an array type `U1[...]` (or a type parameter whose effective base type is `U1[...]`) of the same rank</span></span>
   *  <span data-ttu-id="a1735-656">`V` 是 `IEnumerable<V1>`、`ICollection<V1>` 或 `IList<V1>` 中的一种，`U` 是一维数组类型 `U1[]`（或其有效基类型为 `U1[]`的类型参数）</span><span class="sxs-lookup"><span data-stu-id="a1735-656">`V` is one of `IEnumerable<V1>`, `ICollection<V1>` or `IList<V1>` and `U` is a one-dimensional array type `U1[]`(or a type parameter whose effective base type is `U1[]`)</span></span>
   *  <span data-ttu-id="a1735-657">`V` 是 `C<V1...Vk>` 构造的类、结构、接口或委托类型，并且有一个唯一类型 `C<U1...Uk>` 以便 `U` （或者，如果 `U` 是一个类型参数，其有效的基类或其有效接口集的任何成员）等同于、从（直接或间接） `C<U1...Uk>`继承。</span><span class="sxs-lookup"><span data-stu-id="a1735-657">`V` is a constructed class, struct, interface or delegate type `C<V1...Vk>` and there is a unique type `C<U1...Uk>` such that `U` (or, if `U` is a type parameter, its effective base class or any member of its effective interface set) is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) `C<U1...Uk>`.</span></span>

      <span data-ttu-id="a1735-658">（"唯一性" 限制意味着在 case 接口 `C<T> {} class U: C<X>, C<Y> {}`中，从 `U` 推断到 `C<T>` 时不进行推理，因为 `U1` 可以 `X` 或 `Y`。）</span><span class="sxs-lookup"><span data-stu-id="a1735-658">(The "uniqueness" restriction means that in the case interface `C<T> {} class U: C<X>, C<Y> {}`, then no inference is made when inferring from `U` to `C<T>` because `U1` could be `X` or `Y`.)</span></span>

   <span data-ttu-id="a1735-659">如果这些情况中的任何一种都适用，则将*从*每*个 `Ui` 推导为相应*的 `Vi`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a1735-659">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>

   *  <span data-ttu-id="a1735-660">如果 `Ui` 已知为引用类型，则进行*精确推理*</span><span class="sxs-lookup"><span data-stu-id="a1735-660">If `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="a1735-661">否则，如果 `U` 是数组类型，则进行*下限推理*</span><span class="sxs-lookup"><span data-stu-id="a1735-661">Otherwise, if `U` is an array type then a *lower-bound inference* is made</span></span>
   *  <span data-ttu-id="a1735-662">否则，如果 `C<V1...Vk>` `V`，则推理依赖于 `C`的第 i 个类型参数：</span><span class="sxs-lookup"><span data-stu-id="a1735-662">Otherwise, if `V` is `C<V1...Vk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="a1735-663">如果它是协变的，则进行*下限推理*。</span><span class="sxs-lookup"><span data-stu-id="a1735-663">If it is covariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="a1735-664">如果它是逆变的，则进行*上限推断*。</span><span class="sxs-lookup"><span data-stu-id="a1735-664">If it is contravariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="a1735-665">如果它是固定的，则进行*精确推理*。</span><span class="sxs-lookup"><span data-stu-id="a1735-665">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="a1735-666">否则，不进行推断。</span><span class="sxs-lookup"><span data-stu-id="a1735-666">Otherwise, no inferences are made.</span></span>

#### <a name="upper-bound-inferences"></a><span data-ttu-id="a1735-667">上限推理</span><span class="sxs-lookup"><span data-stu-id="a1735-667">Upper-bound inferences</span></span>

<span data-ttu-id="a1735-668">*从*类型 `U`*到*类型 `V` 的*上限推理*如下所示：</span><span class="sxs-lookup"><span data-stu-id="a1735-668">An *upper-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="a1735-669">如果 `V` 是未*固定*的 `Xi` 之一，则 `U` 添加到 `Xi`的上限。</span><span class="sxs-lookup"><span data-stu-id="a1735-669">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of upper bounds for `Xi`.</span></span>
*  <span data-ttu-id="a1735-670">否则，将通过检查是否存在以下任何情况来确定 `V1...Vk` 和 `U1...Uk`：</span><span class="sxs-lookup"><span data-stu-id="a1735-670">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="a1735-671">`U` 是 `U1[...]` 数组类型，`V` 是同一排名 `V1[...]` 的数组类型</span><span class="sxs-lookup"><span data-stu-id="a1735-671">`U` is an array type `U1[...]` and `V` is an array type `V1[...]` of the same rank</span></span>
   *  <span data-ttu-id="a1735-672">`U` 是 `IEnumerable<Ue>`、`ICollection<Ue>` 或 `IList<Ue>`，`V` 是一维数组类型 `Ve[]`</span><span class="sxs-lookup"><span data-stu-id="a1735-672">`U` is one of `IEnumerable<Ue>`, `ICollection<Ue>` or `IList<Ue>` and `V` is a one-dimensional array type `Ve[]`</span></span>
   *  <span data-ttu-id="a1735-673">`U` 是 `U1?` 类型，`V` 为类型 `V1?`</span><span class="sxs-lookup"><span data-stu-id="a1735-673">`U` is the type `U1?` and `V` is the type `V1?`</span></span>
   *  <span data-ttu-id="a1735-674">`U` 是构造的类、结构、接口或委托类型 `C<U1...Uk>` 并且 `V` 是一个与相同的类、结构、接口或委托类型，继承自（直接或间接），或实现（直接或间接）唯一类型 `C<V1...Vk>`</span><span class="sxs-lookup"><span data-stu-id="a1735-674">`U` is constructed class, struct, interface or delegate type `C<U1...Uk>` and `V` is a class, struct, interface or delegate type which is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) a unique type `C<V1...Vk>`</span></span>

      <span data-ttu-id="a1735-675">（"唯一性" 限制意味着如果有 `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`，则从 `C<U1>` 推断到 `V<Q>`时不进行推理。</span><span class="sxs-lookup"><span data-stu-id="a1735-675">(The "uniqueness" restriction means that if we have `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, then no inference is made when inferring from `C<U1>` to `V<Q>`.</span></span> <span data-ttu-id="a1735-676">不从 `U1` 到 `X<Q>` 或 `Y<Q>`进行推断。）</span><span class="sxs-lookup"><span data-stu-id="a1735-676">Inferences are not made from `U1` to either `X<Q>` or `Y<Q>`.)</span></span>

   <span data-ttu-id="a1735-677">如果这些情况中的任何一种都适用，则将*从*每*个 `Ui` 推导为相应*的 `Vi`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a1735-677">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>
   *  <span data-ttu-id="a1735-678">如果 `Ui` 已知为引用类型，则进行*精确推理*</span><span class="sxs-lookup"><span data-stu-id="a1735-678">If  `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="a1735-679">否则，如果 `V` 是数组类型，则进行*上限推理*</span><span class="sxs-lookup"><span data-stu-id="a1735-679">Otherwise, if `V` is an array type then an *upper-bound inference* is made</span></span>
   *  <span data-ttu-id="a1735-680">否则，如果 `C<U1...Uk>` `U`，则推理依赖于 `C`的第 i 个类型参数：</span><span class="sxs-lookup"><span data-stu-id="a1735-680">Otherwise, if `U` is `C<U1...Uk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="a1735-681">如果它是协变的，则进行*上限推理*。</span><span class="sxs-lookup"><span data-stu-id="a1735-681">If it is covariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="a1735-682">如果它是逆变的，则进行*下限推理*。</span><span class="sxs-lookup"><span data-stu-id="a1735-682">If it is contravariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="a1735-683">如果它是固定的，则进行*精确推理*。</span><span class="sxs-lookup"><span data-stu-id="a1735-683">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="a1735-684">否则，不进行推断。</span><span class="sxs-lookup"><span data-stu-id="a1735-684">Otherwise, no inferences are made.</span></span>   

#### <a name="fixing"></a><span data-ttu-id="a1735-685">更正</span><span class="sxs-lookup"><span data-stu-id="a1735-685">Fixing</span></span>

<span data-ttu-id="a1735-686">使用一组界限 `Xi` 未*固定*的类型变量是*固定*的，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a1735-686">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>

*  <span data-ttu-id="a1735-687">*候选类型*集 `Uj` 以 `Xi`的边界集中的所有类型的集合开头。</span><span class="sxs-lookup"><span data-stu-id="a1735-687">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
*  <span data-ttu-id="a1735-688">然后，我们依次检查每个绑定 for `Xi`：对于每个完全绑定 `U`，`Xi` `Uj` 的所有类型，这些类型与从候选集删除 `U` 不完全相同。</span><span class="sxs-lookup"><span data-stu-id="a1735-688">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="a1735-689">对于 `Xi` 的每个下限 `U`，从候选集中将*不*会从 `U` 中删除任何 `Uj` 类型的隐式转换。</span><span class="sxs-lookup"><span data-stu-id="a1735-689">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="a1735-690">对于 `Xi` 的每个上限 *`U`，`Uj`* 将从候选集删除没有隐式转换到 `U` 的所有类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-690">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
*  <span data-ttu-id="a1735-691">如果其余的候选类型 `Uj` 有一个唯一的类型 `V` 从该类型中有一个到所有其他候选类型的隐式转换，则 `Xi` 固定到 `V`。</span><span class="sxs-lookup"><span data-stu-id="a1735-691">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then `Xi` is fixed to `V`.</span></span>
*  <span data-ttu-id="a1735-692">否则，类型推理将失败。</span><span class="sxs-lookup"><span data-stu-id="a1735-692">Otherwise, type inference fails.</span></span>

#### <a name="inferred-return-type"></a><span data-ttu-id="a1735-693">推断出的返回类型</span><span class="sxs-lookup"><span data-stu-id="a1735-693">Inferred return type</span></span>

<span data-ttu-id="a1735-694">推断出的匿名函数的返回类型 `F` 在类型推理和重载决策过程中使用。</span><span class="sxs-lookup"><span data-stu-id="a1735-694">The inferred return type of an anonymous function `F` is used during type inference and overload resolution.</span></span> <span data-ttu-id="a1735-695">只能为所有参数类型已知的匿名函数确定推理出的返回类型，因为这些参数类型是显式给定的，它是通过匿名函数转换提供的，或者是在对封闭泛型的类型推理期间推断的方法调用。</span><span class="sxs-lookup"><span data-stu-id="a1735-695">The inferred return type can only be determined for an anonymous function where all parameter types are known, either because they are explicitly given, provided through an anonymous function conversion or inferred during type inference on an enclosing generic method invocation.</span></span>

<span data-ttu-id="a1735-696">按如下方式确定***推断结果类型***：</span><span class="sxs-lookup"><span data-stu-id="a1735-696">The ***inferred result type*** is determined as follows:</span></span>

*  <span data-ttu-id="a1735-697">如果 `F` 的主体是具有类型的*表达式*，则推断出的结果类型 `F` 是该表达式的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-697">If the body of `F` is an *expression* that has a type, then the inferred result type of `F` is the type of that expression.</span></span>
*  <span data-ttu-id="a1735-698">如果 `F` 的主体是一个*块*，块的 `return` 语句中的表达式集有一个最常见的类型 `T` （[查找一组表达式的最佳通用类型](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)），则将 `T`推理出的结果 `F` 类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-698">If the body of `F` is a *block* and the set of expressions in the block's `return` statements has a best common type `T` ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), then the inferred result type of `F` is `T`.</span></span>
*  <span data-ttu-id="a1735-699">否则，无法推断 `F`的结果类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-699">Otherwise, a result type cannot be inferred for `F`.</span></span>

<span data-ttu-id="a1735-700">按如下方式确定***推断出的返回类型***：</span><span class="sxs-lookup"><span data-stu-id="a1735-700">The ***inferred return type*** is determined as follows:</span></span>

*  <span data-ttu-id="a1735-701">如果 `F` 是异步的，并且 `F` 的主体是分类为 nothing （[表达式分类](expressions.md#expression-classifications)）的表达式，或者是没有 return 语句的语句块包含表达式，则推断出的返回类型为 `System.Threading.Tasks.Task`</span><span class="sxs-lookup"><span data-stu-id="a1735-701">If `F` is async and the body of `F` is either an expression classified as nothing ([Expression classifications](expressions.md#expression-classifications)), or a statement block where no return statements have expressions, the inferred return type is `System.Threading.Tasks.Task`</span></span>
*  <span data-ttu-id="a1735-702">如果 `F` 为 async 并且 `T`推理结果类型，则推断出的返回类型为 `System.Threading.Tasks.Task<T>`。</span><span class="sxs-lookup"><span data-stu-id="a1735-702">If `F` is async and has an inferred result type `T`, the inferred return type is `System.Threading.Tasks.Task<T>`.</span></span>
*  <span data-ttu-id="a1735-703">如果 `F` 不是异步的，并且 `T`推断结果类型，则推断出的返回类型为 `T`。</span><span class="sxs-lookup"><span data-stu-id="a1735-703">If `F` is non-async and has an inferred result type `T`, the inferred return type is `T`.</span></span>
*  <span data-ttu-id="a1735-704">否则，将无法为 `F`推断返回类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-704">Otherwise a return type cannot be inferred for `F`.</span></span>

<span data-ttu-id="a1735-705">作为涉及匿名函数的类型推理的示例，请考虑在 `System.Linq.Enumerable` 类中声明 `Select` 扩展方法：</span><span class="sxs-lookup"><span data-stu-id="a1735-705">As an example of type inference involving anonymous functions, consider the `Select` extension method declared in the `System.Linq.Enumerable` class:</span></span>
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

<span data-ttu-id="a1735-706">假定 `System.Linq` 命名空间是使用 `using` 子句导入的，并且给定的类 `Customer` 具有类型 `string`的 `Name` 属性，则可以使用 `Select` 方法选择客户列表的名称：</span><span class="sxs-lookup"><span data-stu-id="a1735-706">Assuming the `System.Linq` namespace was imported with a `using` clause, and given a class `Customer` with a `Name` property of type `string`, the `Select` method can be used to select the names of a list of customers:</span></span>
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

<span data-ttu-id="a1735-707">通过将调用重写为静态方法调用来处理 `Select` 的扩展方法调用（[扩展方法](expressions.md#extension-method-invocations)调用）：</span><span class="sxs-lookup"><span data-stu-id="a1735-707">The extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)) of `Select` is processed by rewriting the invocation to a static method invocation:</span></span>
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

<span data-ttu-id="a1735-708">由于未显式指定类型参数，因此将使用类型推理来推断类型参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-708">Since type arguments were not explicitly specified, type inference is used to infer the type arguments.</span></span> <span data-ttu-id="a1735-709">首先，`customers` 自变量与 `source` 参数相关，推理 `T` 为 `Customer`。</span><span class="sxs-lookup"><span data-stu-id="a1735-709">First, the `customers` argument is related to the `source` parameter, inferring `T` to be `Customer`.</span></span> <span data-ttu-id="a1735-710">然后，使用上面所述的匿名函数类型推理过程，`c` 给定类型 `Customer`，并且表达式 `c.Name` 与 `selector` 参数的返回类型相关，并推断 `S` 为 `string`。</span><span class="sxs-lookup"><span data-stu-id="a1735-710">Then, using the anonymous function type inference process described above, `c` is given type `Customer`, and the expression `c.Name` is related to the return type of the `selector` parameter, inferring `S` to be `string`.</span></span> <span data-ttu-id="a1735-711">因此，调用等效于</span><span class="sxs-lookup"><span data-stu-id="a1735-711">Thus, the invocation is equivalent to</span></span>
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
<span data-ttu-id="a1735-712">结果的类型为 `IEnumerable<string>`。</span><span class="sxs-lookup"><span data-stu-id="a1735-712">and the result is of type `IEnumerable<string>`.</span></span>

<span data-ttu-id="a1735-713">下面的示例演示匿名函数类型推理如何允许类型信息在泛型方法调用中的参数之间 "流"。</span><span class="sxs-lookup"><span data-stu-id="a1735-713">The following example demonstrates how anonymous function type inference allows type information to "flow" between arguments in a generic method invocation.</span></span> <span data-ttu-id="a1735-714">给定方法：</span><span class="sxs-lookup"><span data-stu-id="a1735-714">Given the method:</span></span>
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

<span data-ttu-id="a1735-715">调用的类型推理：</span><span class="sxs-lookup"><span data-stu-id="a1735-715">Type inference for the invocation:</span></span>
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
<span data-ttu-id="a1735-716">按如下所示进行操作：首先，参数 `"1:15:30"` 与 `value` 参数相关，推理 `X` 为 `string`。</span><span class="sxs-lookup"><span data-stu-id="a1735-716">proceeds as follows: First, the argument `"1:15:30"` is related to the `value` parameter, inferring `X` to be `string`.</span></span> <span data-ttu-id="a1735-717">然后，将 `s`第一个匿名函数的参数 `string`，并将表达式 `TimeSpan.Parse(s)` 与 `f1`的返回类型相关联，推断 `Y` 为 `System.TimeSpan`。</span><span class="sxs-lookup"><span data-stu-id="a1735-717">Then, the parameter of the first anonymous function, `s`, is given the inferred type `string`, and the expression `TimeSpan.Parse(s)` is related to the return type of `f1`, inferring `Y` to be `System.TimeSpan`.</span></span> <span data-ttu-id="a1735-718">最后，为第二个匿名函数 `t`的参数赋予 `System.TimeSpan`推断出的类型，并且表达式 `t.TotalSeconds` 与 `f2`的返回类型相关，推断 `Z` 为 `double`。</span><span class="sxs-lookup"><span data-stu-id="a1735-718">Finally, the parameter of the second anonymous function, `t`, is given the inferred type `System.TimeSpan`, and the expression `t.TotalSeconds` is related to the return type of `f2`, inferring `Z` to be `double`.</span></span> <span data-ttu-id="a1735-719">因此，调用的结果为 `double`类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-719">Thus, the result of the invocation is of type `double`.</span></span>

#### <a name="type-inference-for-conversion-of-method-groups"></a><span data-ttu-id="a1735-720">用于转换方法组的类型推理</span><span class="sxs-lookup"><span data-stu-id="a1735-720">Type inference for conversion of method groups</span></span>

<span data-ttu-id="a1735-721">与泛型方法的调用类似，当包含泛型方法的方法组 `M` 转换为给定委托类型 `D` （[方法组转换](conversions.md#method-group-conversions)）时，也必须应用类型推理。</span><span class="sxs-lookup"><span data-stu-id="a1735-721">Similar to calls of generic methods, type inference must also be applied when a method group `M` containing a generic method is converted to a given delegate type `D` ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="a1735-722">给定方法</span><span class="sxs-lookup"><span data-stu-id="a1735-722">Given a method</span></span>
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
<span data-ttu-id="a1735-723">并且方法组 `M` 分配给委托类型 `D` 推断类型的任务是查找类型参数 `S1...Sn` 以便表达式：</span><span class="sxs-lookup"><span data-stu-id="a1735-723">and the method group `M` being assigned to the delegate type `D` the task of type inference is to find type arguments `S1...Sn` so that the expression:</span></span>
```csharp
M<S1...Sn>
```
<span data-ttu-id="a1735-724">与 `D`成为兼容的（[委托声明](delegates.md#delegate-declarations)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-724">becomes compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`.</span></span>

<span data-ttu-id="a1735-725">与泛型方法调用的类型推理算法不同，在本例中，只有参数*类型*，没有参数*表达式*。</span><span class="sxs-lookup"><span data-stu-id="a1735-725">Unlike the type inference algorithm for generic method calls, in this case there are only argument *types*, no argument *expressions*.</span></span> <span data-ttu-id="a1735-726">具体而言，不存在任何匿名函数，因此不需要进行多个推理阶段。</span><span class="sxs-lookup"><span data-stu-id="a1735-726">In particular, there are no anonymous functions and hence no need for multiple phases of inference.</span></span>

<span data-ttu-id="a1735-727">相反，所有 `Xi` 都被视为未*固定*的，并且从 `D` 的每个参数类型*到*`M`的相应参数 `Tj` 类型，将*从*每个参数类型 `Uj` 进行*下限推理*。</span><span class="sxs-lookup"><span data-stu-id="a1735-727">Instead, all `Xi` are considered *unfixed*, and a *lower-bound inference* is made *from* each argument type `Uj` of `D` *to* the corresponding parameter type `Tj` of `M`.</span></span> <span data-ttu-id="a1735-728">如果没有找到任何 `Xi` 的边界，则类型推理将失败。</span><span class="sxs-lookup"><span data-stu-id="a1735-728">If for any of the `Xi` no bounds were found, type inference fails.</span></span> <span data-ttu-id="a1735-729">否则，所有 `Xi` 都*固定*到相应的 `Si`，这是类型推理的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-729">Otherwise, all `Xi` are *fixed* to corresponding `Si`, which are the result of type inference.</span></span>

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a><span data-ttu-id="a1735-730">查找一组表达式的最佳通用类型</span><span class="sxs-lookup"><span data-stu-id="a1735-730">Finding the best common type of a set of expressions</span></span>

<span data-ttu-id="a1735-731">在某些情况下，需要为一组表达式推断通用类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-731">In some cases, a common type needs to be inferred for a set of expressions.</span></span> <span data-ttu-id="a1735-732">特别是，隐式类型化数组的元素类型和带*块*体的匿名函数的返回类型是通过这种方式找到的。</span><span class="sxs-lookup"><span data-stu-id="a1735-732">In particular, the element types of implicitly typed arrays and the return types of anonymous functions with *block* bodies are found in this way.</span></span>

<span data-ttu-id="a1735-733">直观地说，给定一组表达式 `E1...Em` 此推理应等效于调用方法</span><span class="sxs-lookup"><span data-stu-id="a1735-733">Intuitively, given a set of expressions `E1...Em` this inference should be equivalent to calling a method</span></span>
```csharp
Tr M<X>(X x1 ... X xm)
```
<span data-ttu-id="a1735-734">作为参数的 `Ei`。</span><span class="sxs-lookup"><span data-stu-id="a1735-734">with the `Ei` as arguments.</span></span>

<span data-ttu-id="a1735-735">更准确地说，推理最初使用未*固定*的类型变量 `X`。</span><span class="sxs-lookup"><span data-stu-id="a1735-735">More precisely, the inference starts out with an *unfixed* type variable `X`.</span></span> <span data-ttu-id="a1735-736">然后*从*每个 `Ei`*到*`X`进行*输出类型推断*。</span><span class="sxs-lookup"><span data-stu-id="a1735-736">*Output type inferences* are then made *from* each `Ei` *to* `X`.</span></span> <span data-ttu-id="a1735-737">最后，`X` 是*固定*的，如果成功，则结果类型 `S` 是生成的表达式的最佳通用类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-737">Finally, `X` is *fixed* and, if successful, the resulting type `S` is the resulting best common type for the expressions.</span></span> <span data-ttu-id="a1735-738">如果不存在这样的 `S`，则表达式没有最常见的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-738">If no such `S` exists, the expressions have no best common type.</span></span>

### <a name="overload-resolution"></a><span data-ttu-id="a1735-739">重载决策</span><span class="sxs-lookup"><span data-stu-id="a1735-739">Overload resolution</span></span>

<span data-ttu-id="a1735-740">重载决策是用于选择在给定参数列表和一组候选函数成员时调用的最佳函数成员的绑定时机制。</span><span class="sxs-lookup"><span data-stu-id="a1735-740">Overload resolution is a binding-time mechanism for selecting the best function member to invoke given an argument list and a set of candidate function members.</span></span> <span data-ttu-id="a1735-741">重载决策选择要在中C#的下列不同上下文中调用的函数成员：</span><span class="sxs-lookup"><span data-stu-id="a1735-741">Overload resolution selects the function member to invoke in the following distinct contexts within C#:</span></span>

*  <span data-ttu-id="a1735-742">调用*invocation_expression*中名为的方法（[方法调用](expressions.md#method-invocations)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-742">Invocation of a method named in an *invocation_expression* ([Method invocations](expressions.md#method-invocations)).</span></span>
*  <span data-ttu-id="a1735-743">调用*object_creation_expression*中名为的实例构造函数（[对象创建表达式](expressions.md#object-creation-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-743">Invocation of an instance constructor named in an *object_creation_expression* ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>
*  <span data-ttu-id="a1735-744">通过*element_access* （[元素访问](expressions.md#element-access)）调用索引器访问器。</span><span class="sxs-lookup"><span data-stu-id="a1735-744">Invocation of an indexer accessor through an *element_access* ([Element access](expressions.md#element-access)).</span></span>
*  <span data-ttu-id="a1735-745">表达式中引用的预定义运算符或用户定义的运算符的调用（[一元运算符重载决策](expressions.md#unary-operator-overload-resolution)和[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-745">Invocation of a predefined or user-defined operator referenced in an expression ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)).</span></span>

<span data-ttu-id="a1735-746">其中每个上下文都定义候选函数成员的集合和参数列表，其独特方式为，如以上所列部分中所述。</span><span class="sxs-lookup"><span data-stu-id="a1735-746">Each of these contexts defines the set of candidate function members and the list of arguments in its own unique way, as described in detail in the sections listed above.</span></span> <span data-ttu-id="a1735-747">例如，方法调用的候选项不包括标记为 `override` （[成员查找](expressions.md#member-lookup)）的方法，并且派生类中的任何方法都适用时，基类中的方法不是候选项（[方法调用](expressions.md#method-invocations)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-747">For example, the set of candidates for a method invocation does not include methods marked `override` ([Member lookup](expressions.md#member-lookup)), and methods in a base class are not candidates if any method in a derived class is applicable ([Method invocations](expressions.md#method-invocations)).</span></span>

<span data-ttu-id="a1735-748">确定候选函数成员和参数列表后，在所有情况下，最佳函数成员的选择是相同的：</span><span class="sxs-lookup"><span data-stu-id="a1735-748">Once the candidate function members and the argument list have been identified, the selection of the best function member is the same in all cases:</span></span>

*  <span data-ttu-id="a1735-749">给定一组适用的候选函数成员后，该集合中的最佳函数成员就是。</span><span class="sxs-lookup"><span data-stu-id="a1735-749">Given the set of applicable candidate function members, the best function member in that set is located.</span></span> <span data-ttu-id="a1735-750">如果集只包含一个函数成员，则该函数成员是最佳函数成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-750">If the set contains only one function member, then that function member is the best function member.</span></span> <span data-ttu-id="a1735-751">否则，最佳函数成员就是比给定自变量列表更好的所有其他函数成员的一个函数成员，前提是将每个函数成员与在[更好的函数成员](expressions.md#better-function-member)中使用规则的所有其他函数成员进行比较。</span><span class="sxs-lookup"><span data-stu-id="a1735-751">Otherwise, the best function member is the one function member that is better than all other function members with respect to the given argument list, provided that each function member is compared to all other function members using the rules in [Better function member](expressions.md#better-function-member).</span></span> <span data-ttu-id="a1735-752">如果不是正好有一个比所有其他函数成员更好的函数成员，则函数成员调用是不明确的，并发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-752">If there is not exactly one function member that is better than all other function members, then the function member invocation is ambiguous and a binding-time error occurs.</span></span>

<span data-ttu-id="a1735-753">以下部分定义了术语***适用的函数成员***和***更好的函数成员***的精确含义。</span><span class="sxs-lookup"><span data-stu-id="a1735-753">The following sections define the exact meanings of the terms ***applicable function member*** and ***better function member***.</span></span>

#### <a name="applicable-function-member"></a><span data-ttu-id="a1735-754">适用的函数成员</span><span class="sxs-lookup"><span data-stu-id="a1735-754">Applicable function member</span></span>

<span data-ttu-id="a1735-755">当满足以下所有条件时，函数成员被称为适用于自变量列表的***函数成员***`A`：</span><span class="sxs-lookup"><span data-stu-id="a1735-755">A function member is said to be an ***applicable function member*** with respect to an argument list `A` when all of the following are true:</span></span>

*  <span data-ttu-id="a1735-756">`A` 中的每个参数都对应于函数成员声明中的一个参数（如[相应的参数](expressions.md#corresponding-parameters)中所述），并且没有参数对应的任何参数都是可选参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-756">Each argument in `A` corresponds to a parameter in the function member declaration as described in [Corresponding parameters](expressions.md#corresponding-parameters), and any parameter to which no argument corresponds is an optional parameter.</span></span>
*  <span data-ttu-id="a1735-757">对于 `A`中的每个自变量，参数的参数传递模式（即值、`ref`或 `out`）与相应参数的参数传递模式相同，并且</span><span class="sxs-lookup"><span data-stu-id="a1735-757">For each argument in `A`, the parameter passing mode of the argument (i.e., value, `ref`, or `out`) is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="a1735-758">对于值参数或参数数组，存在从参数到相应参数类型的隐式转换（[隐式转换](conversions.md#implicit-conversions)）或</span><span class="sxs-lookup"><span data-stu-id="a1735-758">for a value parameter or a parameter array, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="a1735-759">对于 `ref` 或 `out` 参数，参数的类型与对应参数的类型相同。</span><span class="sxs-lookup"><span data-stu-id="a1735-759">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span> <span data-ttu-id="a1735-760">毕竟，`ref` 或 `out` 参数是传递的参数的别名。</span><span class="sxs-lookup"><span data-stu-id="a1735-760">After all, a `ref` or `out` parameter is an alias for the argument passed.</span></span>

<span data-ttu-id="a1735-761">对于包含参数数组的函数成员，如果函数成员适用于上述规则，则称它适用于其***正常形式***。</span><span class="sxs-lookup"><span data-stu-id="a1735-761">For a function member that includes a parameter array, if the function member is applicable by the above rules, it is said to be applicable in its ***normal form***.</span></span> <span data-ttu-id="a1735-762">如果包含参数数组的函数成员在其正常形式下不适用，则函数成员可以采用其***展开形式***：</span><span class="sxs-lookup"><span data-stu-id="a1735-762">If a function member that includes a parameter array is not applicable in its normal form, the function member may instead be applicable in its ***expanded form***:</span></span>

*  <span data-ttu-id="a1735-763">通过使用参数数组元素类型的零个或多个值参数替换函数成员声明中的参数数组，使参数列表中的参数数目与参数的总数 `A` 匹配，来构造展开后的形式。</span><span class="sxs-lookup"><span data-stu-id="a1735-763">The expanded form is constructed by replacing the parameter array in the function member declaration with zero or more value parameters of the element type of the parameter array such that the number of arguments in the argument list `A` matches the total number of parameters.</span></span> <span data-ttu-id="a1735-764">如果 `A` 的参数少于函数成员声明中的固定参数数目，则无法构造函数成员的展开形式，因此不适用。</span><span class="sxs-lookup"><span data-stu-id="a1735-764">If `A` has fewer arguments than the number of fixed parameters in the function member declaration, the expanded form of the function member cannot be constructed and is thus not applicable.</span></span>
*  <span data-ttu-id="a1735-765">否则，如果为中的每个参数 `A` 参数传递模式，则展开形式适用，参数的参数传递模式与相应参数的参数传递模式相同，并且</span><span class="sxs-lookup"><span data-stu-id="a1735-765">Otherwise, the expanded form is applicable if for each argument in `A` the parameter passing mode of the argument is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="a1735-766">对于固定值参数或扩展创建的值参数，存在从自变量的类型到相应参数类型的隐式转换（[隐式转换](conversions.md#implicit-conversions)）或</span><span class="sxs-lookup"><span data-stu-id="a1735-766">for a fixed value parameter or a value parameter created by the expansion, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the type of the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="a1735-767">对于 `ref` 或 `out` 参数，参数的类型与对应参数的类型相同。</span><span class="sxs-lookup"><span data-stu-id="a1735-767">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span>

#### <a name="better-function-member"></a><span data-ttu-id="a1735-768">更好的函数成员</span><span class="sxs-lookup"><span data-stu-id="a1735-768">Better function member</span></span>

<span data-ttu-id="a1735-769">出于确定更好的函数成员的目的，一个被去除的自变量列表 A 构造，它以其在原始参数列表中出现的顺序仅包含自变量表达式本身。</span><span class="sxs-lookup"><span data-stu-id="a1735-769">For the purposes of determining the better function member, a stripped-down argument list A is constructed containing just the argument expressions themselves in the order they appear in the original argument list.</span></span>

<span data-ttu-id="a1735-770">按以下方式构造每个候选函数成员的参数列表：</span><span class="sxs-lookup"><span data-stu-id="a1735-770">Parameter lists for each of the candidate function members are constructed in the following way:</span></span>

*  <span data-ttu-id="a1735-771">如果函数成员仅适用于扩展窗体，则使用展开的窗体。</span><span class="sxs-lookup"><span data-stu-id="a1735-771">The expanded form is used if the function member was applicable only in the expanded form.</span></span>
*  <span data-ttu-id="a1735-772">将从参数列表中删除不具有相应参数的可选参数</span><span class="sxs-lookup"><span data-stu-id="a1735-772">Optional parameters with no corresponding arguments are removed from the parameter list</span></span>
*  <span data-ttu-id="a1735-773">参数会进行重新排序，以便它们在与参数列表中的相应参数相同的位置上发生。</span><span class="sxs-lookup"><span data-stu-id="a1735-773">The parameters are reordered so that they occur at the same position as the corresponding argument in the argument list.</span></span>

<span data-ttu-id="a1735-774">给定一个参数列表 `A`，其中包含一组参数表达式 `{E1, E2, ..., En}` 和两个适用的函数成员 `Mp`，并 `Mq` 参数类型 `{P1, P2, ..., Pn}` 和 `{Q1, Q2, ..., Qn}`，则 `Mp` 定义为比 `Mq`***更好的函数成员***</span><span class="sxs-lookup"><span data-stu-id="a1735-774">Given an argument list `A` with a set of argument expressions `{E1, E2, ..., En}` and two applicable function members `Mp` and `Mq` with parameter types `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}`, `Mp` is defined to be a ***better function member*** than `Mq` if</span></span>

*  <span data-ttu-id="a1735-775">对于每个参数，从 `Ex` 到 `Qx` 的隐式转换并不优于从 `Ex` 到 `Px`的隐式转换和</span><span class="sxs-lookup"><span data-stu-id="a1735-775">for each argument, the implicit conversion from `Ex` to `Qx` is not better than the implicit conversion from `Ex` to `Px`, and</span></span>
*  <span data-ttu-id="a1735-776">对于至少一个参数，从 `Ex` 到 `Px` 的转换优于从 `Ex` 到 `Qx`的转换。</span><span class="sxs-lookup"><span data-stu-id="a1735-776">for at least one argument, the conversion from `Ex` to `Px` is better than the conversion from `Ex` to `Qx`.</span></span>

<span data-ttu-id="a1735-777">当执行此计算时，如果 `Mp` 或 `Mq` 适用于其展开形式，则 `Px` 或 `Qx` 引用参数列表的展开形式的参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-777">When performing this evaluation, if `Mp` or `Mq` is applicable in its expanded form, then `Px` or `Qx` refers to a parameter in the expanded form of the parameter list.</span></span>

<span data-ttu-id="a1735-778">如果参数类型序列 `{P1, P2, ..., Pn}` 和 `{Q1, Q2, ..., Qn}` 等效（即每个 `Pi` 的标识转换为相应的 `Qi`），则将按顺序应用以下附加中断规则，以确定更好的函数成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-778">In case the parameter type sequences `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}` are equivalent (i.e. each `Pi` has an identity conversion to the corresponding `Qi`), the following tie-breaking rules are applied, in order, to determine the better function member.</span></span>

*  <span data-ttu-id="a1735-779">如果 `Mp` 为非泛型方法并且 `Mq` 为泛型方法，则 `Mp` 比 `Mq`更好。</span><span class="sxs-lookup"><span data-stu-id="a1735-779">If `Mp` is a non-generic method and `Mq` is a generic method, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="a1735-780">否则，如果 `Mp` 适用于其正常窗体并且 `Mq` 具有 `params` 数组，并且仅适用于其展开形式，则 `Mp` 比 `Mq`更好。</span><span class="sxs-lookup"><span data-stu-id="a1735-780">Otherwise, if `Mp` is applicable in its normal form and `Mq` has a `params` array and is applicable only in its expanded form, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="a1735-781">否则，如果 `Mp` 具有比 `Mq`更多的声明参数，则 `Mp` 比 `Mq`更好。</span><span class="sxs-lookup"><span data-stu-id="a1735-781">Otherwise, if `Mp` has more declared parameters than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="a1735-782">如果两种方法都有 `params` 数组，并且仅适用于其展开的形式，则会发生这种情况。</span><span class="sxs-lookup"><span data-stu-id="a1735-782">This can occur if both methods have `params` arrays and are applicable only in their expanded forms.</span></span>
*  <span data-ttu-id="a1735-783">否则，如果 `Mp` 的所有参数都有相应的参数，而默认参数需要替换为 `Mq` 中的至少一个可选参数，则 `Mp` 比 `Mq`更好。</span><span class="sxs-lookup"><span data-stu-id="a1735-783">Otherwise if all parameters of `Mp` have a corresponding argument whereas default arguments need to be substituted for at least one optional parameter in `Mq` then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="a1735-784">否则，如果 `Mp` 具有比 `Mq`更具体的参数类型，则 `Mp` 比 `Mq`更好。</span><span class="sxs-lookup"><span data-stu-id="a1735-784">Otherwise, if `Mp` has more specific parameter types than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="a1735-785">让 `{R1, R2, ..., Rn}` 和 `{S1, S2, ..., Sn}` 表示 `Mp` 和 `Mq`的未实例化和未扩展的参数类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-785">Let `{R1, R2, ..., Rn}` and `{S1, S2, ..., Sn}` represent the uninstantiated and unexpanded parameter types of `Mp` and `Mq`.</span></span> <span data-ttu-id="a1735-786">`Mp`的参数类型比 `Mq`的更具体，因为对于每个参数，`Rx` 不太具体于 `Sx`，并且对于至少一个参数，`Rx` 比 `Sx`更为具体：</span><span class="sxs-lookup"><span data-stu-id="a1735-786">`Mp`'s parameter types are more specific than `Mq`'s if, for each parameter, `Rx` is not less specific than `Sx`, and, for at least one parameter, `Rx` is more specific than `Sx`:</span></span>
   *  <span data-ttu-id="a1735-787">类型参数不如非类型参数的特定类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-787">A type parameter is less specific than a non-type parameter.</span></span>
   *  <span data-ttu-id="a1735-788">如果至少有一个类型自变量是更具体的类型，并且没有类型参数的特定于其他中的相应类型参数，则递归方式比另一个构造类型（具有相同数量的类型参数）更为具体。</span><span class="sxs-lookup"><span data-stu-id="a1735-788">Recursively, a constructed type is more specific than another constructed type (with the same number of type arguments) if at least one type argument is more specific and no type argument is less specific than the corresponding type argument in the other.</span></span>
   *  <span data-ttu-id="a1735-789">如果第一个数组的元素类型比第二个元素的类型更具体，则数组类型比另一个数组类型更具体（具有相同维数的数组）。</span><span class="sxs-lookup"><span data-stu-id="a1735-789">An array type is more specific than another array type (with the same number of dimensions) if the element type of the first is more specific than the element type of the second.</span></span>
*  <span data-ttu-id="a1735-790">否则，如果一个成员是非提升运算符，而另一个成员是提升运算符，则不提升的运算符更好。</span><span class="sxs-lookup"><span data-stu-id="a1735-790">Otherwise if one member is a non-lifted operator and  the other is a lifted operator, the non-lifted one is better.</span></span>
*  <span data-ttu-id="a1735-791">否则，两个函数成员都不会更好。</span><span class="sxs-lookup"><span data-stu-id="a1735-791">Otherwise, neither function member is better.</span></span>

#### <a name="better-conversion-from-expression"></a><span data-ttu-id="a1735-792">表达式的更好转换</span><span class="sxs-lookup"><span data-stu-id="a1735-792">Better conversion from expression</span></span>

<span data-ttu-id="a1735-793">给定一个隐式转换 `C1`，该转换将 `E` 类型的表达式转换为类型 `T1`，并从表达式 `E` 转换为类型 `T2`的隐式转换 `C2`，`C1` 是比 `C2`***更好的转换***（如果 `E` 并不完全匹配 `T2` 和至少一个保留项）：</span><span class="sxs-lookup"><span data-stu-id="a1735-793">Given an implicit conversion `C1` that converts from an expression `E` to a type `T1`, and an implicit conversion `C2` that converts from an expression `E` to a type `T2`, `C1` is a ***better conversion*** than `C2` if `E` does not exactly match `T2` and at least one of the following holds:</span></span>

* <span data-ttu-id="a1735-794">`E` 完全匹配 `T1` （[完全匹配的表达式](expressions.md#exactly-matching-expression)）</span><span class="sxs-lookup"><span data-stu-id="a1735-794">`E` exactly matches `T1` ([Exactly matching Expression](expressions.md#exactly-matching-expression))</span></span>
* <span data-ttu-id="a1735-795">`T1` 是比 `T2` 更好的转换目标（[更好的转换目标](expressions.md#better-conversion-target)）</span><span class="sxs-lookup"><span data-stu-id="a1735-795">`T1` is a better conversion target than `T2` ([Better conversion target](expressions.md#better-conversion-target))</span></span>

#### <a name="exactly-matching-expression"></a><span data-ttu-id="a1735-796">完全匹配的表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-796">Exactly matching Expression</span></span>

<span data-ttu-id="a1735-797">给定表达式 `E` 和类型 `T`，如果下列其中一项保留，`E` 与 `T` 完全匹配：</span><span class="sxs-lookup"><span data-stu-id="a1735-797">Given an expression `E` and a type `T`, `E` exactly matches `T` if one of the following holds:</span></span>

*  <span data-ttu-id="a1735-798">`E` 具有类型 `S`，并且存在从 `S` 到 `T` 的标识转换</span><span class="sxs-lookup"><span data-stu-id="a1735-798">`E` has a type `S`, and an identity conversion exists from `S` to `T`</span></span>
*  <span data-ttu-id="a1735-799">`E` 是一种匿名函数，`T` 为委托类型 `D` 或表达式目录树类型 `Expression<D>` 和以下保留之一：</span><span class="sxs-lookup"><span data-stu-id="a1735-799">`E` is an anonymous function, `T` is either a delegate type `D` or an expression tree type `Expression<D>` and one of the following holds:</span></span>
   *  <span data-ttu-id="a1735-800">`E` 在 `D` 参数列表的上下文中存在的推断返回类型 `X` （[推断返回类型](expressions.md#inferred-return-type)），并且存在从 `X` 到返回类型的标识转换 `D`</span><span class="sxs-lookup"><span data-stu-id="a1735-800">An inferred return type `X` exists for `E` in the context of the parameter list of `D` ([Inferred return type](expressions.md#inferred-return-type)), and an identity conversion exists from `X` to the return type of `D`</span></span>
   *  <span data-ttu-id="a1735-801">`E` 为非异步的，并且 `D` 具有返回类型 `Y` 或者 `E` 为 async 并且 `D` 具有返回类型 `Task<Y>`，以及以下保留项之一：</span><span class="sxs-lookup"><span data-stu-id="a1735-801">Either `E` is non-async and `D` has a return type `Y` or `E` is async and `D` has a return type `Task<Y>`, and one of the following holds:</span></span>
      * <span data-ttu-id="a1735-802">`E` 的主体是一个完全匹配的表达式 `Y`</span><span class="sxs-lookup"><span data-stu-id="a1735-802">The body of `E` is an expression that exactly matches `Y`</span></span>
      * <span data-ttu-id="a1735-803">`E` 的主体是一个语句块，其中每个 return 语句返回一个完全匹配的表达式 `Y`</span><span class="sxs-lookup"><span data-stu-id="a1735-803">The body of `E` is a statement block where every return statement returns an expression that exactly matches `Y`</span></span>

#### <a name="better-conversion-target"></a><span data-ttu-id="a1735-804">更好的转换目标</span><span class="sxs-lookup"><span data-stu-id="a1735-804">Better conversion target</span></span>

<span data-ttu-id="a1735-805">如果 `T1` 和 `T2`两种不同的类型，`T1` 是比 `T2` 更好的转换目标，前提是不存在从 `T2` 到 `T1` 的隐式转换，并且至少包含以下其中一项：</span><span class="sxs-lookup"><span data-stu-id="a1735-805">Given two different types `T1` and `T2`, `T1` is a better conversion target than `T2` if no implicit conversion from `T2` to `T1` exists, and at least one of the following holds:</span></span>

*  <span data-ttu-id="a1735-806">从 `T1` 到 `T2` 存在的隐式转换</span><span class="sxs-lookup"><span data-stu-id="a1735-806">An implicit conversion from `T1` to `T2` exists</span></span>
*  <span data-ttu-id="a1735-807">`T1` 为委托类型 `D1` 或表达式树类型 `Expression<D1>`，`T2` 为委托类型 `D2` 或表达式树类型 `Expression<D2>`，`D1` 具有返回类型 `S1` 和以下保留之一：</span><span class="sxs-lookup"><span data-stu-id="a1735-807">`T1` is either a delegate type `D1` or an expression tree type `Expression<D1>`, `T2` is either a delegate type `D2` or an expression tree type `Expression<D2>`, `D1` has a return type `S1` and one of the following holds:</span></span>
   * <span data-ttu-id="a1735-808">`D2` 返回 void</span><span class="sxs-lookup"><span data-stu-id="a1735-808">`D2` is void returning</span></span>
   * <span data-ttu-id="a1735-809">`D2` 具有 `S2`的返回类型，`S1` 是比 `S2` 更好的转换目标</span><span class="sxs-lookup"><span data-stu-id="a1735-809">`D2` has a return type `S2`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="a1735-810">`T1` `Task<S1>`，`T2` `Task<S2>`，`S1` 是比 `S2` 更好的转换目标</span><span class="sxs-lookup"><span data-stu-id="a1735-810">`T1` is `Task<S1>`, `T2` is `Task<S2>`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="a1735-811">`T1` `S1` 或 `S1?`，其中 `S1` 是有符号整数类型，`T2` `S2` 或 `S2?`，其中 `S2` 是无符号整数类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-811">`T1` is `S1` or `S1?` where `S1` is a signed integral type, and `T2` is `S2` or `S2?` where `S2` is an unsigned integral type.</span></span> <span data-ttu-id="a1735-812">具体而言：</span><span class="sxs-lookup"><span data-stu-id="a1735-812">Specifically:</span></span>
   * <span data-ttu-id="a1735-813">`S1` 是 `sbyte` 的，`S2` 为 `byte`、`ushort`、`uint`或 `ulong`</span><span class="sxs-lookup"><span data-stu-id="a1735-813">`S1` is `sbyte` and `S2` is `byte`, `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="a1735-814">`S1` 是 `short` 并且 `S2` 为 `ushort`、`uint`或 `ulong`</span><span class="sxs-lookup"><span data-stu-id="a1735-814">`S1` is `short` and `S2` is `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="a1735-815">`S1` 是 `int` 并且 `S2` 为 `uint`或 `ulong`</span><span class="sxs-lookup"><span data-stu-id="a1735-815">`S1` is `int` and `S2` is `uint`, or `ulong`</span></span>
   * <span data-ttu-id="a1735-816">`S1` 是 `long` 并且 `S2` 为 `ulong`</span><span class="sxs-lookup"><span data-stu-id="a1735-816">`S1` is `long` and `S2` is `ulong`</span></span>

#### <a name="overloading-in-generic-classes"></a><span data-ttu-id="a1735-817">泛型类中的重载</span><span class="sxs-lookup"><span data-stu-id="a1735-817">Overloading in generic classes</span></span>

<span data-ttu-id="a1735-818">尽管声明的签名必须是唯一的，但类型参数的替换可能导致相同的签名。</span><span class="sxs-lookup"><span data-stu-id="a1735-818">While signatures as declared must be unique, it is possible that substitution of type arguments results in identical signatures.</span></span> <span data-ttu-id="a1735-819">在这种情况下，以上重载解决方法的分解规则将选取最特定的成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-819">In such cases, the tie-breaking rules of overload resolution above will pick the most specific member.</span></span>

<span data-ttu-id="a1735-820">下面的示例显示了根据此规则有效和无效的重载：</span><span class="sxs-lookup"><span data-stu-id="a1735-820">The following examples show overloads that are valid and invalid according to this rule:</span></span>

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

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a><span data-ttu-id="a1735-821">动态重载解析的编译时检查</span><span class="sxs-lookup"><span data-stu-id="a1735-821">Compile-time checking of dynamic overload resolution</span></span>

<span data-ttu-id="a1735-822">对于大多数动态绑定操作，解析的可能候选项集在编译时是未知的。</span><span class="sxs-lookup"><span data-stu-id="a1735-822">For most dynamically bound operations the set of possible candidates for resolution is unknown at compile-time.</span></span> <span data-ttu-id="a1735-823">但在某些情况下，在编译时，候选集是已知的：</span><span class="sxs-lookup"><span data-stu-id="a1735-823">In certain cases, however the candidate set is known at compile-time:</span></span>

*  <span data-ttu-id="a1735-824">具有动态参数的静态方法调用</span><span class="sxs-lookup"><span data-stu-id="a1735-824">Static method calls with dynamic arguments</span></span>
*  <span data-ttu-id="a1735-825">接收方不是动态表达式的实例方法调用</span><span class="sxs-lookup"><span data-stu-id="a1735-825">Instance method calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="a1735-826">接收方不是动态表达式的索引器调用</span><span class="sxs-lookup"><span data-stu-id="a1735-826">Indexer calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="a1735-827">具有动态参数的构造函数调用</span><span class="sxs-lookup"><span data-stu-id="a1735-827">Constructor calls with dynamic arguments</span></span>

<span data-ttu-id="a1735-828">在这些情况下，将对每个候选项执行有限的编译时检查，以查看是否有可能在运行时应用它们。此检查包括以下步骤：</span><span class="sxs-lookup"><span data-stu-id="a1735-828">In these cases a limited compile-time check is performed for each candidate to see if any of them could possibly apply at run-time.This check consists of the following steps:</span></span>

*  <span data-ttu-id="a1735-829">分部类型推理：使用[类型推理](expressions.md#type-inference)的规则推断不直接或间接依赖于类型 `dynamic` 的参数的任何类型参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-829">Partial type inference: Any type argument that does not depend directly or indirectly on an argument of type `dynamic` is inferred using the rules of [Type inference](expressions.md#type-inference).</span></span> <span data-ttu-id="a1735-830">其余类型参数是未知的。</span><span class="sxs-lookup"><span data-stu-id="a1735-830">The remaining type arguments are unknown.</span></span>
*  <span data-ttu-id="a1735-831">部分适用性检查：根据[适用的函数成员](expressions.md#applicable-function-member)检查适用性，但忽略类型未知的参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-831">Partial applicability check: Applicability is checked according to [Applicable function member](expressions.md#applicable-function-member), but ignoring parameters whose types are unknown.</span></span>
*  <span data-ttu-id="a1735-832">如果没有候选项通过此测试，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-832">If no candidate passes this test, a compile-time error occurs.</span></span>

### <a name="function-member-invocation"></a><span data-ttu-id="a1735-833">函数成员调用</span><span class="sxs-lookup"><span data-stu-id="a1735-833">Function member invocation</span></span>

<span data-ttu-id="a1735-834">本节介绍在运行时调用特定函数成员的过程。</span><span class="sxs-lookup"><span data-stu-id="a1735-834">This section describes the process that takes place at run-time to invoke a particular function member.</span></span> <span data-ttu-id="a1735-835">假定绑定时间进程已确定要调用的特定成员（可能通过将重载决策应用于一组候选函数成员）。</span><span class="sxs-lookup"><span data-stu-id="a1735-835">It is assumed that a binding-time process has already determined the particular member to invoke, possibly by applying overload resolution to a set of candidate function members.</span></span>

<span data-ttu-id="a1735-836">出于描述调用过程的目的，函数成员分为两类：</span><span class="sxs-lookup"><span data-stu-id="a1735-836">For purposes of describing the invocation process, function members are divided into two categories:</span></span>

*  <span data-ttu-id="a1735-837">静态函数成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-837">Static function members.</span></span> <span data-ttu-id="a1735-838">这些是实例构造函数、静态方法、静态属性访问器和用户定义的运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-838">These are instance constructors, static methods, static property accessors, and user-defined operators.</span></span> <span data-ttu-id="a1735-839">静态函数成员始终是非虚拟的。</span><span class="sxs-lookup"><span data-stu-id="a1735-839">Static function members are always non-virtual.</span></span>
*  <span data-ttu-id="a1735-840">实例函数成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-840">Instance function members.</span></span> <span data-ttu-id="a1735-841">这些是实例方法、实例属性访问器和索引器访问器。</span><span class="sxs-lookup"><span data-stu-id="a1735-841">These are instance methods, instance property accessors, and indexer accessors.</span></span> <span data-ttu-id="a1735-842">实例函数成员既不是虚拟的也不是虚拟的，始终在特定的实例上调用。</span><span class="sxs-lookup"><span data-stu-id="a1735-842">Instance function members are either non-virtual or virtual, and are always invoked on a particular instance.</span></span> <span data-ttu-id="a1735-843">实例由实例表达式计算，它在函数成员内可作为 `this` （[此访问](expressions.md#this-access)）进行访问。</span><span class="sxs-lookup"><span data-stu-id="a1735-843">The instance is computed by an instance expression, and it becomes accessible within the function member as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="a1735-844">函数成员调用的运行时处理包括以下步骤，其中 `M` 是函数成员，如果 `M` 为实例成员，`E` 为实例表达式：</span><span class="sxs-lookup"><span data-stu-id="a1735-844">The run-time processing of a function member invocation consists of the following steps, where `M` is the function member and, if `M` is an instance member, `E` is the instance expression:</span></span>

*  <span data-ttu-id="a1735-845">如果 `M` 是静态函数成员：</span><span class="sxs-lookup"><span data-stu-id="a1735-845">If `M` is a static function member:</span></span>
   * <span data-ttu-id="a1735-846">参数列表按[参数](expressions.md#argument-lists)列表中所述进行计算。</span><span class="sxs-lookup"><span data-stu-id="a1735-846">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="a1735-847">调用 `M`。</span><span class="sxs-lookup"><span data-stu-id="a1735-847">`M` is invoked.</span></span>

*  <span data-ttu-id="a1735-848">如果 `M` 是在*value_type*中声明的实例函数成员：</span><span class="sxs-lookup"><span data-stu-id="a1735-848">If `M` is an instance function member declared in a *value_type*:</span></span>
   * <span data-ttu-id="a1735-849">计算 `E`。</span><span class="sxs-lookup"><span data-stu-id="a1735-849">`E` is evaluated.</span></span> <span data-ttu-id="a1735-850">如果此计算导致异常，则不执行其他步骤。</span><span class="sxs-lookup"><span data-stu-id="a1735-850">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="a1735-851">如果 `E` 未分类为变量，则创建 `E`类型的临时本地变量，并将 `E` 的值分配给该变量。</span><span class="sxs-lookup"><span data-stu-id="a1735-851">If `E` is not classified as a variable, then a temporary local variable of `E`'s type is created and the value of `E` is assigned to that variable.</span></span> <span data-ttu-id="a1735-852">然后，将 `E` 重新分类为对该临时本地变量的引用。</span><span class="sxs-lookup"><span data-stu-id="a1735-852">`E` is then reclassified as a reference to that temporary local variable.</span></span> <span data-ttu-id="a1735-853">临时变量在 `M`内作为 `this` 访问，但不能以其他方式访问。</span><span class="sxs-lookup"><span data-stu-id="a1735-853">The temporary variable is accessible as `this` within `M`, but not in any other way.</span></span> <span data-ttu-id="a1735-854">因此，只有在 `E` 为真正的变量时，调用方才能观察 `M` 对 `this`所做的更改。</span><span class="sxs-lookup"><span data-stu-id="a1735-854">Thus, only when `E` is a true variable is it possible for the caller to observe the changes that `M` makes to `this`.</span></span>
   * <span data-ttu-id="a1735-855">参数列表按[参数](expressions.md#argument-lists)列表中所述进行计算。</span><span class="sxs-lookup"><span data-stu-id="a1735-855">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="a1735-856">调用 `M`。</span><span class="sxs-lookup"><span data-stu-id="a1735-856">`M` is invoked.</span></span> <span data-ttu-id="a1735-857">`E` 引用的变量成为 `this`引用的变量。</span><span class="sxs-lookup"><span data-stu-id="a1735-857">The variable referenced by `E` becomes the variable referenced by `this`.</span></span>

*  <span data-ttu-id="a1735-858">如果 `M` 是在*reference_type*中声明的实例函数成员：</span><span class="sxs-lookup"><span data-stu-id="a1735-858">If `M` is an instance function member declared in a *reference_type*:</span></span>
   * <span data-ttu-id="a1735-859">计算 `E`。</span><span class="sxs-lookup"><span data-stu-id="a1735-859">`E` is evaluated.</span></span> <span data-ttu-id="a1735-860">如果此计算导致异常，则不执行其他步骤。</span><span class="sxs-lookup"><span data-stu-id="a1735-860">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="a1735-861">参数列表按[参数](expressions.md#argument-lists)列表中所述进行计算。</span><span class="sxs-lookup"><span data-stu-id="a1735-861">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="a1735-862">如果 `E` 的类型为*value_type*，则执行装箱转换（[装箱转换](types.md#boxing-conversions)）以将 `E` 转换为类型 `object`，并在以下步骤中将 `E` 视为类型 `object`。</span><span class="sxs-lookup"><span data-stu-id="a1735-862">If the type of `E` is a *value_type*, a boxing conversion ([Boxing conversions](types.md#boxing-conversions)) is performed to convert `E` to type `object`, and `E` is considered to be of type `object` in the following steps.</span></span> <span data-ttu-id="a1735-863">在这种情况下，`M` 只能是 `System.Object`的成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-863">In this case, `M` could only be a member of `System.Object`.</span></span>
   * <span data-ttu-id="a1735-864">检查 `E` 的值是否有效。</span><span class="sxs-lookup"><span data-stu-id="a1735-864">The value of `E` is checked to be valid.</span></span> <span data-ttu-id="a1735-865">如果 `null``E` 的值，则将引发 `System.NullReferenceException`，并且不执行任何其他步骤。</span><span class="sxs-lookup"><span data-stu-id="a1735-865">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
   * <span data-ttu-id="a1735-866">确定要调用的函数成员实现：</span><span class="sxs-lookup"><span data-stu-id="a1735-866">The function member implementation to invoke is determined:</span></span>
     * <span data-ttu-id="a1735-867">如果 `E` 的绑定时类型是接口，则调用的函数成员是由 `E`引用的实例的运行时类型提供的 `M` 的实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-867">If the binding-time type of `E` is an interface, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="a1735-868">此函数成员是通过以下方式确定的：应用接口映射规则（[接口映射](interfaces.md#interface-mapping)），以确定 `E`所引用的实例的运行时类型提供的 `M` 实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-868">This function member is determined by applying the interface mapping rules ([Interface mapping](interfaces.md#interface-mapping)) to determine the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="a1735-869">否则，如果 `M` 为虚拟函数成员，则调用的函数成员是由 `E`引用的实例的运行时类型提供的 `M` 的实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-869">Otherwise, if `M` is a virtual function member, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="a1735-870">此函数成员是通过应用规则来确定的，这些规则用于确定 `E`所引用的实例的运行时类型 `M` 的派生程度最高的实现（[虚拟方法](classes.md#virtual-methods)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-870">This function member is determined by applying the rules for determining the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of `M` with respect to the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="a1735-871">否则，`M` 为非虚拟函数成员，调用的函数成员 `M` 本身。</span><span class="sxs-lookup"><span data-stu-id="a1735-871">Otherwise, `M` is a non-virtual function member, and the function member to invoke is `M` itself.</span></span>
   * <span data-ttu-id="a1735-872">调用上述步骤中确定的函数成员实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-872">The function member implementation determined in the step above is invoked.</span></span> <span data-ttu-id="a1735-873">`E` 引用的对象成为 `this`引用的对象。</span><span class="sxs-lookup"><span data-stu-id="a1735-873">The object referenced by `E` becomes the object referenced by `this`.</span></span>

#### <a name="invocations-on-boxed-instances"></a><span data-ttu-id="a1735-874">在装箱实例上调用</span><span class="sxs-lookup"><span data-stu-id="a1735-874">Invocations on boxed instances</span></span>

<span data-ttu-id="a1735-875">在*value_type*中实现的函数成员可在以下情况下通过*value_type*的装箱实例进行调用：</span><span class="sxs-lookup"><span data-stu-id="a1735-875">A function member implemented in a *value_type* can be invoked through a boxed instance of that *value_type* in the following situations:</span></span>

*  <span data-ttu-id="a1735-876">当函数成员是从类型 `object` 继承的方法的 `override` 时，将通过类型 `object`的实例表达式调用。</span><span class="sxs-lookup"><span data-stu-id="a1735-876">When the function member is an `override` of a method inherited from type `object` and is invoked through an instance expression of type `object`.</span></span>
*  <span data-ttu-id="a1735-877">当函数成员是接口函数成员的实现并通过*interface_type*的实例表达式调用时。</span><span class="sxs-lookup"><span data-stu-id="a1735-877">When the function member is an implementation of an interface function member and is invoked through an instance expression of an *interface_type*.</span></span>
*  <span data-ttu-id="a1735-878">函数成员通过委托调用时。</span><span class="sxs-lookup"><span data-stu-id="a1735-878">When the function member is invoked through a delegate.</span></span>

<span data-ttu-id="a1735-879">在这些情况下，已装箱的实例被视为包含*value_type*的变量，此变量将成为函数成员调用中 `this` 所引用的变量。</span><span class="sxs-lookup"><span data-stu-id="a1735-879">In these situations, the boxed instance is considered to contain a variable of the *value_type*, and this variable becomes the variable referenced by `this` within the function member invocation.</span></span> <span data-ttu-id="a1735-880">具体而言，这意味着当对装箱实例调用函数成员时，该函数成员可以修改装箱实例中包含的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-880">In particular, this means that when a function member is invoked on a boxed instance, it is possible for the function member to modify the value contained in the boxed instance.</span></span>

## <a name="primary-expressions"></a><span data-ttu-id="a1735-881">主要表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-881">Primary expressions</span></span>

<span data-ttu-id="a1735-882">主要表达式包括表达式的最简单形式。</span><span class="sxs-lookup"><span data-stu-id="a1735-882">Primary expressions include the simplest forms of expressions.</span></span>

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

<span data-ttu-id="a1735-883">主表达式划分*array_creation_expression*s 和*primary_no_array_creation_expression*之间。</span><span class="sxs-lookup"><span data-stu-id="a1735-883">Primary expressions are divided between *array_creation_expression*s and *primary_no_array_creation_expression*s.</span></span> <span data-ttu-id="a1735-884">以这种方式对待数组创建表达式，而不是将其与其他简单的表达式窗体一起列出，而是允许语法禁止可能的代码混乱，如</span><span class="sxs-lookup"><span data-stu-id="a1735-884">Treating array-creation-expression in this way, rather than listing it along with the other simple expression forms, enables the grammar to disallow potentially confusing code such as</span></span>
```csharp
object o = new int[3][1];
```
<span data-ttu-id="a1735-885">否则，会将其解释为</span><span class="sxs-lookup"><span data-stu-id="a1735-885">which would otherwise be interpreted as</span></span>
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a><span data-ttu-id="a1735-886">文本</span><span class="sxs-lookup"><span data-stu-id="a1735-886">Literals</span></span>

<span data-ttu-id="a1735-887">由*文本*（[文本](lexical-structure.md#literals)）组成的*primary_expression*分类为值。</span><span class="sxs-lookup"><span data-stu-id="a1735-887">A *primary_expression* that consists of a *literal* ([Literals](lexical-structure.md#literals)) is classified as a value.</span></span>


### <a name="interpolated-strings"></a><span data-ttu-id="a1735-888">内插字符串</span><span class="sxs-lookup"><span data-stu-id="a1735-888">Interpolated strings</span></span>

<span data-ttu-id="a1735-889">*Interpolated_string_expression*包含一个 `$` 符号后跟一个常规或逐字字符串文本，其中的洞由 `{` 和 `}`分隔，其中包含表达式和格式规范。</span><span class="sxs-lookup"><span data-stu-id="a1735-889">An *interpolated_string_expression* consists of a `$` sign followed by a regular or verbatim string literal, wherein holes, delimited by `{` and `}`, enclose expressions and formatting specifications.</span></span> <span data-ttu-id="a1735-890">内插字符串表达式是已分解为单独标记的*interpolated_string_literal*的结果，如内[插字符串文本](lexical-structure.md#interpolated-string-literals)中所述。</span><span class="sxs-lookup"><span data-stu-id="a1735-890">An interpolated string expression is the result of an *interpolated_string_literal* that has been broken up into individual tokens, as described in [Interpolated string literals](lexical-structure.md#interpolated-string-literals).</span></span>

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

<span data-ttu-id="a1735-891">内插中的*constant_expression*必须具有到 `int`的隐式转换。</span><span class="sxs-lookup"><span data-stu-id="a1735-891">The *constant_expression* in an interpolation must have an implicit conversion to `int`.</span></span>

<span data-ttu-id="a1735-892">*Interpolated_string_expression*分类为值。</span><span class="sxs-lookup"><span data-stu-id="a1735-892">An *interpolated_string_expression* is classified as a value.</span></span> <span data-ttu-id="a1735-893">如果它立即转换为 `System.IFormattable` 或使用隐式内插字符串转换（[隐式内插](conversions.md#implicit-interpolated-string-conversions)的字符串转换） `System.FormattableString`，则内插字符串表达式具有该类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-893">If it is immediately converted to `System.IFormattable` or `System.FormattableString` with an implicit interpolated string conversion ([Implicit interpolated string conversions](conversions.md#implicit-interpolated-string-conversions)), the interpolated string expression has that type.</span></span> <span data-ttu-id="a1735-894">否则，它的类型为 `string`。</span><span class="sxs-lookup"><span data-stu-id="a1735-894">Otherwise, it has the type `string`.</span></span>

<span data-ttu-id="a1735-895">如果内插字符串的类型是 `System.IFormattable` 或 `System.FormattableString`，则表示对 `System.Runtime.CompilerServices.FormattableStringFactory.Create`的调用。</span><span class="sxs-lookup"><span data-stu-id="a1735-895">If the type of an interpolated string is `System.IFormattable` or `System.FormattableString`, the meaning is a call to `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span></span> <span data-ttu-id="a1735-896">如果类型 `string`，则表达式的含义是对 `string.Format`的调用。</span><span class="sxs-lookup"><span data-stu-id="a1735-896">If the type is `string`, the meaning of the expression is a call to `string.Format`.</span></span> <span data-ttu-id="a1735-897">在这两种情况下，调用的参数列表都包含格式字符串文本和每个内插的占位符，以及与占位符相对应的每个表达式的参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-897">In both cases, the argument list of the call consists of a format string literal with placeholders for each interpolation, and an argument for each expression corresponding to the place holders.</span></span>

<span data-ttu-id="a1735-898">格式字符串文字按如下方式构造，其中 `N` 是*interpolated_string_expression*中的内插数：</span><span class="sxs-lookup"><span data-stu-id="a1735-898">The format string literal is constructed as follows, where `N` is the number of interpolations in the *interpolated_string_expression*:</span></span>

*  <span data-ttu-id="a1735-899">如果*interpolated_regular_string_whole*或*interpolated_verbatim_string_whole*后跟 `$` 符号，则格式字符串文本就是该标记。</span><span class="sxs-lookup"><span data-stu-id="a1735-899">If an *interpolated_regular_string_whole* or an *interpolated_verbatim_string_whole* follows the `$` sign, then the format string literal is that token.</span></span>
*  <span data-ttu-id="a1735-900">否则，格式字符串文本包含：</span><span class="sxs-lookup"><span data-stu-id="a1735-900">Otherwise, the format string literal consists of:</span></span> 
   *  <span data-ttu-id="a1735-901">首先*interpolated_regular_string_start*或*interpolated_verbatim_string_start*</span><span class="sxs-lookup"><span data-stu-id="a1735-901">First the *interpolated_regular_string_start* or *interpolated_verbatim_string_start*</span></span>
   *  <span data-ttu-id="a1735-902">然后，将每个数字从 `0` `I` 到 `N-1`：</span><span class="sxs-lookup"><span data-stu-id="a1735-902">Then for each number `I` from `0` to `N-1`:</span></span> 
      * <span data-ttu-id="a1735-903">`I` 的十进制表示形式</span><span class="sxs-lookup"><span data-stu-id="a1735-903">The decimal representation of `I`</span></span>
      * <span data-ttu-id="a1735-904">然后，如果相应的*内插*具有*constant_expression*，则 `,` （逗号），后跟的值的十进制表示形式*constant_expression*</span><span class="sxs-lookup"><span data-stu-id="a1735-904">Then, if the corresponding *interpolation* has a *constant_expression*, a `,` (comma) followed by the decimal representation of the value of the *constant_expression*</span></span>
      * <span data-ttu-id="a1735-905">然后， *interpolated_regular_string_mid*、 *interpolated_regular_string_end*、 *interpolated_verbatim_string_mid*或*interpolated_verbatim_string_end*后跟相应的内插。</span><span class="sxs-lookup"><span data-stu-id="a1735-905">Then the *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* or *interpolated_verbatim_string_end* immediately following the corresponding interpolation.</span></span>

<span data-ttu-id="a1735-906">后续参数只是*内插*中的*表达式*（如果有），按顺序排列。</span><span class="sxs-lookup"><span data-stu-id="a1735-906">The subsequent arguments are simply the *expressions* from the *interpolations* (if any), in order.</span></span>

<span data-ttu-id="a1735-907">TODO：示例。</span><span class="sxs-lookup"><span data-stu-id="a1735-907">TODO: examples.</span></span>


### <a name="simple-names"></a><span data-ttu-id="a1735-908">简单名称</span><span class="sxs-lookup"><span data-stu-id="a1735-908">Simple names</span></span>

<span data-ttu-id="a1735-909">*Simple_name*包含标识符，可选择后跟类型参数列表：</span><span class="sxs-lookup"><span data-stu-id="a1735-909">A *simple_name* consists of an identifier, optionally followed by a type argument list:</span></span>

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

<span data-ttu-id="a1735-910">*Simple_name*格式为 `I` 或格式为 `I<A1,...,Ak>`，其中 `I` 是单个标识符，`<A1,...,Ak>` 是可选*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="a1735-910">A *simple_name* is either of the form `I` or of the form `I<A1,...,Ak>`, where `I` is a single identifier and `<A1,...,Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="a1735-911">当未指定*type_argument_list*时，请考虑 `K` 为零。</span><span class="sxs-lookup"><span data-stu-id="a1735-911">When no *type_argument_list* is specified, consider `K` to be zero.</span></span> <span data-ttu-id="a1735-912">将按如下所示对*simple_name*进行评估和分类：</span><span class="sxs-lookup"><span data-stu-id="a1735-912">The *simple_name* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="a1735-913">如果 `K` 为零且*simple_name*出现在*块*中，并且*块*的（或封闭*块*的）局部变量声明空间（[声明](basic-concepts.md#declarations)）包含名称 `I`的局部变量、参数或常量，则*simple_name*引用该局部变量、参数或常量，并归类为变量或值。</span><span class="sxs-lookup"><span data-stu-id="a1735-913">If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>
*  <span data-ttu-id="a1735-914">如果 `K` 为零，且*simple_name*出现在泛型方法声明的主体中，并且该声明包含名称为 `I`的类型参数，则*simple_name*引用该类型参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-914">If `K` is zero and the *simple_name* appears within the body of a generic method declaration and if that declaration includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
*  <span data-ttu-id="a1735-915">否则，对于每个实例类型 `T` （[实例类型](classes.md#the-instance-type)），从立即封闭类型声明的实例类型开始，并继续执行每个封闭类或结构声明的实例类型（如果有）：</span><span class="sxs-lookup"><span data-stu-id="a1735-915">Otherwise, for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of the immediately enclosing type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
   *  <span data-ttu-id="a1735-916">如果 `K` 为零，且 `T` 的声明包含名称为 `I`的类型参数，则*simple_name*将引用该类型参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-916">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
   *  <span data-ttu-id="a1735-917">否则，如果 `T` 中的 `I` 的成员查找（[成员查找](expressions.md#member-lookup)） `K` 类型参数生成匹配：</span><span class="sxs-lookup"><span data-stu-id="a1735-917">Otherwise, if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match:</span></span>
      * <span data-ttu-id="a1735-918">如果 `T` 是直接封闭类或结构类型的实例类型，并且查找标识了一个或多个方法，则结果为具有 `this`的关联实例表达式的方法组。</span><span class="sxs-lookup"><span data-stu-id="a1735-918">If `T` is the instance type of the immediately enclosing class or struct type and the lookup identifies one or more methods, the result is a method group with an associated instance expression of `this`.</span></span> <span data-ttu-id="a1735-919">如果指定了类型参数列表，它将用于调用泛型方法（[方法调用](expressions.md#method-invocations)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-919">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
      * <span data-ttu-id="a1735-920">否则，如果 `T` 是直接封闭类或结构类型的实例类型，如果查找标识实例成员，并且引用出现在实例构造函数、实例方法或实例访问器的主体中，则结果与 `this.I`窗体的成员访问（[成员访问](expressions.md#member-access)）相同。</span><span class="sxs-lookup"><span data-stu-id="a1735-920">Otherwise, if `T` is the instance type of the immediately enclosing class or struct type, if the lookup identifies an instance member, and if the reference occurs within the body of an instance constructor, an instance method, or an instance accessor, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `this.I`.</span></span> <span data-ttu-id="a1735-921">仅当 `K` 为零时，才会发生这种情况。</span><span class="sxs-lookup"><span data-stu-id="a1735-921">This can only happen when `K` is zero.</span></span>
      * <span data-ttu-id="a1735-922">否则，结果将与表单的成员访问（[成员访问](expressions.md#member-access)） `T.I` 或 `T.I<A1,...,Ak>`相同。</span><span class="sxs-lookup"><span data-stu-id="a1735-922">Otherwise, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `T.I` or `T.I<A1,...,Ak>`.</span></span> <span data-ttu-id="a1735-923">在这种情况下， *simple_name*引用实例成员，则为绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-923">In this case, it is a binding-time error for the *simple_name* to refer to an instance member.</span></span>

*  <span data-ttu-id="a1735-924">否则，对于每个命名空间 `N`，从发生*simple_name*的命名空间开始，继续每个封闭命名空间（如果有），并以全局命名空间结束，将计算以下步骤直到找到实体：</span><span class="sxs-lookup"><span data-stu-id="a1735-924">Otherwise, for each namespace `N`, starting with the namespace in which the *simple_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
   *  <span data-ttu-id="a1735-925">如果 `K` 为零且 `I` 是 `N`中的命名空间的名称，则：</span><span class="sxs-lookup"><span data-stu-id="a1735-925">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
      * <span data-ttu-id="a1735-926">如果*simple_name*发生的位置由 `N` 的命名空间声明括起来，并且命名空间声明包含将名称 `I` 与命名空间或类型相关联的*extern_alias_directive*或*using_alias_directive* ，则*simple_name*为不明确，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-926">If the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="a1735-927">否则， *simple_name*引用 `N`中名为 `I` 的命名空间。</span><span class="sxs-lookup"><span data-stu-id="a1735-927">Otherwise, the *simple_name* refers to the namespace named `I` in `N`.</span></span>
   *  <span data-ttu-id="a1735-928">否则，如果 `N` 包含名称 `I` 且 `K` 类型参数的可访问类型，则：</span><span class="sxs-lookup"><span data-stu-id="a1735-928">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
      * <span data-ttu-id="a1735-929">如果 `K` 为零，且*simple_name*的位置由 `N` 的命名空间声明括起来，并且命名空间声明包含将名称 using_alias_directive 与命名空间或类型相关联的*extern_alias_directive*或 * `I`* ，则*simple_name*是不明确的，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-929">If `K` is zero and the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="a1735-930">否则， *namespace_or_type_name*引用用给定类型参数构造的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-930">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="a1735-931">否则，如果*simple_name*发生的位置由 `N`的命名空间声明括起来：</span><span class="sxs-lookup"><span data-stu-id="a1735-931">Otherwise, if the location where the *simple_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
      * <span data-ttu-id="a1735-932">如果 `K` 为零，且命名空间声明包含将名称 `I` 与导入的命名空间或类型相关联的*extern_alias_directive*或*using_alias_directive* ，则*simple_name*引用该命名空间或类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-932">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *simple_name* refers to that namespace or type.</span></span>
      * <span data-ttu-id="a1735-933">否则，如果由*using_namespace_directive*s 和*using_static_directive*s 命名空间声明导入的命名空间和类型声明包含一个名称 `I` 和 `K` 类型参数的可访问类型或非扩展静态成员，则*simple_name*引用使用给定类型参数构造的该类型或成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-933">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_static_directive*s of the namespace declaration contain exactly one accessible type or non-extension static member having name `I` and `K` type parameters, then the *simple_name* refers to that type or member constructed with the given type arguments.</span></span>
      * <span data-ttu-id="a1735-934">否则，如果命名空间声明中*using_namespace_directive*s 所导入的命名空间和类型包含多个具有名称 `I` 和 `K` 类型参数的可访问的类型或非扩展方法静态成员，则*simple_name*不明确，并发生错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-934">Otherwise, if the namespaces and types imported by the *using_namespace_directive*s of the namespace declaration contain more than one accessible type or non-extension-method static member having name `I` and `K` type parameters, then the *simple_name* is ambiguous and an error occurs.</span></span>

   <span data-ttu-id="a1735-935">请注意，整个步骤与处理*namespace_or_type_name* （[命名空间和类型名称](basic-concepts.md#namespace-and-type-names)）的相应步骤完全相同。</span><span class="sxs-lookup"><span data-stu-id="a1735-935">Note that this entire step is exactly parallel to the corresponding step in the processing of a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span>

*  <span data-ttu-id="a1735-936">否则， *simple_name*是未定义的，并且发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-936">Otherwise, the *simple_name* is undefined and a compile-time error occurs.</span></span>


### <a name="parenthesized-expressions"></a><span data-ttu-id="a1735-937">带括号的表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-937">Parenthesized expressions</span></span>

<span data-ttu-id="a1735-938">*Parenthesized_expression*包含用括号括起来的*表达式*。</span><span class="sxs-lookup"><span data-stu-id="a1735-938">A *parenthesized_expression* consists of an *expression* enclosed in parentheses.</span></span>

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

<span data-ttu-id="a1735-939">通过计算括号内的*表达式*来计算*parenthesized_expression* 。</span><span class="sxs-lookup"><span data-stu-id="a1735-939">A *parenthesized_expression* is evaluated by evaluating the *expression* within the parentheses.</span></span> <span data-ttu-id="a1735-940">如果括号内的*表达式*表示命名空间或类型，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-940">If the *expression* within the parentheses denotes a namespace or type, a compile-time error occurs.</span></span> <span data-ttu-id="a1735-941">否则， *parenthesized_expression*的结果是包含的*表达式*的计算结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-941">Otherwise, the result of the *parenthesized_expression* is the result of the evaluation of the contained *expression*.</span></span>

### <a name="member-access"></a><span data-ttu-id="a1735-942">成员访问</span><span class="sxs-lookup"><span data-stu-id="a1735-942">Member access</span></span>

<span data-ttu-id="a1735-943">*Member_access*包含*primary_expression*、 *predefined_type*或*qualified_alias_member*，后跟一个 "`.`" 标记，后跟一个*标识符*，可选择后跟*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="a1735-943">A *member_access* consists of a *primary_expression*, a *predefined_type*, or a *qualified_alias_member*, followed by a "`.`" token, followed by an *identifier*, optionally followed by a *type_argument_list*.</span></span>

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

<span data-ttu-id="a1735-944">*Qualified_alias_member*生产是在[命名空间别名限定符](namespaces.md#namespace-alias-qualifiers)中定义的。</span><span class="sxs-lookup"><span data-stu-id="a1735-944">The *qualified_alias_member* production is defined in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span>

<span data-ttu-id="a1735-945">*Member_access*的格式为 `E.I` 或格式 `E.I<A1, ..., Ak>`，其中 `E` 是主表达式，`I` 是单个标识符，`<A1, ..., Ak>` 是可选*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="a1735-945">A *member_access* is either of the form `E.I` or of the form `E.I<A1, ..., Ak>`, where `E` is a primary-expression, `I` is a single identifier and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="a1735-946">当未指定*type_argument_list*时，请考虑 `K` 为零。</span><span class="sxs-lookup"><span data-stu-id="a1735-946">When no *type_argument_list* is specified, consider `K` to be zero.</span></span>

<span data-ttu-id="a1735-947">*Primary_expression*类型 `dynamic` 的*member_access*是动态绑定的（[动态绑定](expressions.md#dynamic-binding)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-947">A *member_access* with a *primary_expression* of type `dynamic` is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="a1735-948">在这种情况下，编译器将成员访问分类为 `dynamic`类型的属性访问。</span><span class="sxs-lookup"><span data-stu-id="a1735-948">In this case the compiler classifies the member access as a property access of type `dynamic`.</span></span> <span data-ttu-id="a1735-949">下面的规则使用运行时类型（而不是*primary_expression*的编译时类型）在运行时应用*member_access*的含义。</span><span class="sxs-lookup"><span data-stu-id="a1735-949">The rules below to determine the meaning of the *member_access* are then applied at run-time, using the run-time type instead of the compile-time type of the *primary_expression*.</span></span> <span data-ttu-id="a1735-950">如果此运行时分类导致了方法组，则成员访问必须是*invocation_expression*的*primary_expression* 。</span><span class="sxs-lookup"><span data-stu-id="a1735-950">If this run-time classification leads to a method group, then the member access must be the *primary_expression* of an *invocation_expression*.</span></span>

<span data-ttu-id="a1735-951">将按如下所示对*member_access*进行评估和分类：</span><span class="sxs-lookup"><span data-stu-id="a1735-951">The *member_access* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="a1735-952">如果 `K` 为零且 `E` 为命名空间并且 `E` 包含名称 `I`的嵌套命名空间，则结果为该命名空间。</span><span class="sxs-lookup"><span data-stu-id="a1735-952">If `K` is zero and `E` is a namespace and `E` contains a nested namespace with name `I`, then the result is that namespace.</span></span>
*  <span data-ttu-id="a1735-953">否则，如果 `E` 是一个命名空间，并且 `E` 包含名称 `I` 和 `K` 类型参数的可访问类型，则结果为使用给定类型参数构造的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-953">Otherwise, if `E` is a namespace and `E` contains an accessible type having name `I` and `K` type parameters, then the result is that type constructed with the given type arguments.</span></span>
*  <span data-ttu-id="a1735-954">如果 `E` 是*predefined_type*或归类为类型的*primary_expression* ，则如果 `E` 不是类型形参，并且 `I` 中 `E` 的成员查找（[成员查找](expressions.md#member-lookup)）使用 `K`，则类型参数将生成匹配项，然后按如下方式计算  和分类：</span><span class="sxs-lookup"><span data-stu-id="a1735-954">If `E` is a *predefined_type* or a *primary_expression* classified as a type, if `E` is not a type parameter, and if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `E` with `K` type parameters produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="a1735-955">如果 `I` 标识一个类型，则结果是使用给定的类型参数构造的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-955">If `I` identifies a type, then the result is that type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="a1735-956">如果 `I` 标识一个或多个方法，则结果是没有关联的实例表达式的方法组。</span><span class="sxs-lookup"><span data-stu-id="a1735-956">If `I` identifies one or more methods, then the result is a method group with no associated instance expression.</span></span> <span data-ttu-id="a1735-957">如果指定了类型参数列表，它将用于调用泛型方法（[方法调用](expressions.md#method-invocations)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-957">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="a1735-958">如果 `I` 确定 `static` 属性，则结果为无关联的实例表达式的属性访问。</span><span class="sxs-lookup"><span data-stu-id="a1735-958">If `I` identifies a `static` property, then the result is a property access with no associated instance expression.</span></span>
   *  <span data-ttu-id="a1735-959">如果 `I` 确定 `static` 字段：</span><span class="sxs-lookup"><span data-stu-id="a1735-959">If `I` identifies a `static` field:</span></span>
      * <span data-ttu-id="a1735-960">如果字段为 `readonly` 并且引用出现在声明该字段的类或结构的静态构造函数之外，则结果为值，即 `E`中的静态字段 `I` 的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-960">If the field is `readonly` and the reference occurs outside the static constructor of the class or struct in which the field is declared, then the result is a value, namely the value of the static field `I` in `E`.</span></span>
      * <span data-ttu-id="a1735-961">否则，结果为变量，即 `E`中 `I` 的静态字段。</span><span class="sxs-lookup"><span data-stu-id="a1735-961">Otherwise, the result is a variable, namely the static field `I` in `E`.</span></span>
   *  <span data-ttu-id="a1735-962">如果 `I` 标识 `static` 事件：</span><span class="sxs-lookup"><span data-stu-id="a1735-962">If `I` identifies a `static` event:</span></span>
      * <span data-ttu-id="a1735-963">如果引用出现在声明事件的类或结构中，并且声明事件时没有*event_accessor_declarations* （[事件](classes.md#events)），则 `E.I` 的处理方式与 `I` 为静态字段的方式完全相同。</span><span class="sxs-lookup"><span data-stu-id="a1735-963">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), then `E.I` is processed exactly as if `I` were a static field.</span></span>
      * <span data-ttu-id="a1735-964">否则，结果是没有关联的实例表达式的事件访问。</span><span class="sxs-lookup"><span data-stu-id="a1735-964">Otherwise, the result is an event access with no associated instance expression.</span></span>
   *  <span data-ttu-id="a1735-965">如果 `I` 标识一个常量，则结果为值，即该常量的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-965">If `I` identifies a constant, then the result is a value, namely the value of that constant.</span></span>
    * <span data-ttu-id="a1735-966">如果 `I` 标识枚举成员，则结果为值，即枚举成员的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-966">If `I` identifies an enumeration member, then the result is a value, namely the value of that enumeration member.</span></span>
    * <span data-ttu-id="a1735-967">否则，`E.I` 是无效的成员引用，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-967">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="a1735-968">如果 `E` 是属性访问、索引器访问、变量或值、 `T`的类型，并且在 `T` 中包含 `K`的 `I` 的成员查找（[成员查找](expressions.md#member-lookup)）， 类型参数会生成匹配项，然后按如下所示对 `E.I` 进行计算和分类：</span><span class="sxs-lookup"><span data-stu-id="a1735-968">If `E` is a property access, indexer access, variable, or value, the type of which is `T`, and a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="a1735-969">首先，如果 `E` 是属性或索引器访问，则获取属性或索引器访问的值（[表达式的值](expressions.md#values-of-expressions)），并将 `E` 重新分类为值。</span><span class="sxs-lookup"><span data-stu-id="a1735-969">First, if `E` is a property or indexer access, then the value of the property or indexer access is obtained ([Values of expressions](expressions.md#values-of-expressions)) and `E` is reclassified as a value.</span></span>
   *  <span data-ttu-id="a1735-970">如果 `I` 标识了一个或多个方法，则结果将是具有 `E`的关联实例表达式的方法组。</span><span class="sxs-lookup"><span data-stu-id="a1735-970">If `I` identifies one or more methods, then the result is a method group with an associated instance expression of `E`.</span></span> <span data-ttu-id="a1735-971">如果指定了类型参数列表，它将用于调用泛型方法（[方法调用](expressions.md#method-invocations)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-971">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="a1735-972">如果 `I` 标识实例属性，</span><span class="sxs-lookup"><span data-stu-id="a1735-972">If `I` identifies an instance property,</span></span>
      * <span data-ttu-id="a1735-973">如果 `E` `this`，`I` 将标识一个自动实现的属性（[自动实现的属性](classes.md#automatically-implemented-properties)），而不使用 setter，并且引用出现在类或结构类型 `T`的实例构造函数中，则结果是一个变量，即 `I` 给定的 `T` 的实例中的 `this`所给定的自动属性的隐藏支持字段。</span><span class="sxs-lookup"><span data-stu-id="a1735-973">If `E` is `this`, `I` identifies an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)) without a setter, and the reference occurs within an instance constructor for a class or struct type `T`, then the result is a variable, namely the hidden backing field for the auto-property given by `I` in the instance of `T` given by `this`.</span></span>
      * <span data-ttu-id="a1735-974">否则，结果为具有 `E`的关联实例表达式的属性访问。</span><span class="sxs-lookup"><span data-stu-id="a1735-974">Otherwise, the result is a property access with an associated instance expression of `E`.</span></span>
   *  <span data-ttu-id="a1735-975">如果 `T` 是*class_type*并且 `I` 标识该*class_type*的实例字段：</span><span class="sxs-lookup"><span data-stu-id="a1735-975">If `T` is a *class_type* and `I` identifies an instance field of that *class_type*:</span></span>
      * <span data-ttu-id="a1735-976">如果 `null``E` 的值，则将引发 `System.NullReferenceException`。</span><span class="sxs-lookup"><span data-stu-id="a1735-976">If the value of `E` is `null`, then a `System.NullReferenceException` is thrown.</span></span>
      * <span data-ttu-id="a1735-977">否则，如果字段 `readonly` 并且引用出现在声明该字段的类的实例构造函数之外，则结果为值，即 `E`引用的对象中的字段的值 `I`。</span><span class="sxs-lookup"><span data-stu-id="a1735-977">Otherwise, if the field is `readonly` and the reference occurs outside an instance constructor of the class in which the field is declared, then the result is a value, namely the value of the field `I` in the object referenced by `E`.</span></span>
      * <span data-ttu-id="a1735-978">否则，结果为变量，即 `E`所引用的对象中 `I` 字段。</span><span class="sxs-lookup"><span data-stu-id="a1735-978">Otherwise, the result is a variable, namely the field `I` in the object referenced by `E`.</span></span>
   *  <span data-ttu-id="a1735-979">如果 `T` 是*struct_type*并且 `I` 标识该*struct_type*的实例字段：</span><span class="sxs-lookup"><span data-stu-id="a1735-979">If `T` is a *struct_type* and `I` identifies an instance field of that *struct_type*:</span></span>
      * <span data-ttu-id="a1735-980">如果 `E` 是一个值，或如果该字段 `readonly` 并且引用出现在声明该字段的结构的实例构造函数之外，则结果是一个值，即 `E`指定的结构实例中的字段的值 `I`。</span><span class="sxs-lookup"><span data-stu-id="a1735-980">If `E` is a value, or if the field is `readonly` and the reference occurs outside an instance constructor of the struct in which the field is declared, then the result is a value, namely the value of the field `I` in the struct instance given by `E`.</span></span>
      * <span data-ttu-id="a1735-981">否则，结果为变量，即 `E`指定的结构实例中的字段 `I`。</span><span class="sxs-lookup"><span data-stu-id="a1735-981">Otherwise, the result is a variable, namely the field `I` in the struct instance given by `E`.</span></span>
   *  <span data-ttu-id="a1735-982">如果 `I` 标识实例事件：</span><span class="sxs-lookup"><span data-stu-id="a1735-982">If `I` identifies an instance event:</span></span>
      * <span data-ttu-id="a1735-983">如果引用出现在声明了事件的类或结构中，并且事件是在没有*event_accessor_declarations* （[事件](classes.md#events)）的情况下声明的，并且引用不是作为 `+=` 或 `-=` 运算符的左侧出现，则 `E.I` 的处理方式与 `I` 为实例字段的方式完全相同。</span><span class="sxs-lookup"><span data-stu-id="a1735-983">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), and the reference does not occur as the left-hand side of a `+=` or `-=` operator, then `E.I` is processed exactly as if `I` was an instance field.</span></span>
      * <span data-ttu-id="a1735-984">否则，结果为具有 `E`的关联实例表达式的事件访问。</span><span class="sxs-lookup"><span data-stu-id="a1735-984">Otherwise, the result is an event access with an associated instance expression of `E`.</span></span>
*  <span data-ttu-id="a1735-985">否则，会尝试将 `E.I` 处理为扩展方法调用（[扩展方法](expressions.md#extension-method-invocations)调用）。</span><span class="sxs-lookup"><span data-stu-id="a1735-985">Otherwise, an attempt is made to process `E.I` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="a1735-986">如果此操作失败，`E.I` 是无效的成员引用，并发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-986">If this fails, `E.I` is an invalid member reference, and a binding-time error occurs.</span></span>

#### <a name="identical-simple-names-and-type-names"></a><span data-ttu-id="a1735-987">相同的简单名称和类型名称</span><span class="sxs-lookup"><span data-stu-id="a1735-987">Identical simple names and type names</span></span>

<span data-ttu-id="a1735-988">在窗体 `E.I`的成员访问中，如果 `E` 是单个标识符，并且 `E` 为*simple_name* （[简单名称](expressions.md#simple-names)）是与 *`E` （* [命名空间和类型名称](basic-concepts.md#namespace-and-type-names)）具有相同类型的常数、字段、属性、局部变量或参数，则允许 type_name 的两种可能含义。</span><span class="sxs-lookup"><span data-stu-id="a1735-988">In a member access of the form `E.I`, if `E` is a single identifier, and if the meaning of `E` as a *simple_name* ([Simple names](expressions.md#simple-names)) is a constant, field, property, local variable, or parameter with the same type as the meaning of `E` as a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)), then both possible meanings of `E` are permitted.</span></span> <span data-ttu-id="a1735-989">`E.I` 的两个可能含义不明确，因为 `I` 必须是这两种情况下 `E` 类型的成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-989">The two possible meanings of `E.I` are never ambiguous, since `I` must necessarily be a member of the type `E` in both cases.</span></span> <span data-ttu-id="a1735-990">换言之，该规则只允许访问静态成员和嵌套类型的 `E`，在这种情况下，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-990">In other words, the rule simply permits access to the static members and nested types of `E` where a compile-time error would otherwise have occurred.</span></span> <span data-ttu-id="a1735-991">例如：</span><span class="sxs-lookup"><span data-stu-id="a1735-991">For example:</span></span>
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

#### <a name="grammar-ambiguities"></a><span data-ttu-id="a1735-992">语法多义性</span><span class="sxs-lookup"><span data-stu-id="a1735-992">Grammar ambiguities</span></span>

<span data-ttu-id="a1735-993">*Simple_name*的生产（[简单名称](expressions.md#simple-names)）和*member_access* （[成员访问](expressions.md#member-access)）可以在表达式的语法中带来歧义。</span><span class="sxs-lookup"><span data-stu-id="a1735-993">The productions for *simple_name* ([Simple names](expressions.md#simple-names)) and *member_access* ([Member access](expressions.md#member-access)) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="a1735-994">例如，语句：</span><span class="sxs-lookup"><span data-stu-id="a1735-994">For example, the statement:</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="a1735-995">可以解释为对具有两个自变量的 `F` 的调用 `G < A` 和 `B > (7)`。</span><span class="sxs-lookup"><span data-stu-id="a1735-995">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="a1735-996">或者，它可以解释为对 `F` 的调用，该参数是一个参数，它是对具有两个类型参数和一个正则参数的泛型方法的调用 `G`。</span><span class="sxs-lookup"><span data-stu-id="a1735-996">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

<span data-ttu-id="a1735-997">如果可以将标记序列（在上下文中）作为*simple_name* （[简单名称](expressions.md#simple-names)）、 *member_access* （[成员访问](expressions.md#member-access)）或*pointer_member_access* （[指针成员访问](unsafe-code.md#pointer-member-access)）以*type_argument_list* （[类型参数](types.md#type-arguments)）结尾，则将检查紧跟 `>` 结束标记的标记。</span><span class="sxs-lookup"><span data-stu-id="a1735-997">If a sequence of tokens can be parsed (in context) as a *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), or *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) ending with a *type_argument_list* ([Type arguments](types.md#type-arguments)), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="a1735-998">如果是</span><span class="sxs-lookup"><span data-stu-id="a1735-998">If it is one of</span></span>
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
<span data-ttu-id="a1735-999">然后， *type_argument_list*将作为*simple_name*的一部分保留， *member_access*或*pointer_member_access* ，并放弃标记序列的任何其他可能分析。</span><span class="sxs-lookup"><span data-stu-id="a1735-999">then the *type_argument_list* is retained as part of the *simple_name*, *member_access* or *pointer_member_access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="a1735-1000">否则，不会将*type_argument_list*视为*simple_name*、 *member_access*或*pointer_member_access*的一部分，即使没有其他可能的标记序列分析也是如此。</span><span class="sxs-lookup"><span data-stu-id="a1735-1000">Otherwise, the *type_argument_list* is not considered to be part of the *simple_name*, *member_access* or *pointer_member_access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="a1735-1001">请注意，分析*namespace_or_type_name*中的*type_argument_list* （[命名空间和类型名称](basic-concepts.md#namespace-and-type-names)）时，不会应用这些规则。</span><span class="sxs-lookup"><span data-stu-id="a1735-1001">Note that these rules are not applied when parsing a *type_argument_list* in a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span> <span data-ttu-id="a1735-1002">语句</span><span class="sxs-lookup"><span data-stu-id="a1735-1002">The statement</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="a1735-1003">根据此规则，将解释为对 `F` 的调用，该参数是一个参数，它是对具有两个类型参数和一个正则参数的泛型方法的调用 `G`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1003">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="a1735-1004">语句</span><span class="sxs-lookup"><span data-stu-id="a1735-1004">The statements</span></span>
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
<span data-ttu-id="a1735-1005">每个都将解释为对具有两个自变量的 `F` 的调用。</span><span class="sxs-lookup"><span data-stu-id="a1735-1005">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="a1735-1006">语句</span><span class="sxs-lookup"><span data-stu-id="a1735-1006">The statement</span></span>
```csharp
x = F < A > +y;
```
<span data-ttu-id="a1735-1007">将解释为小于运算符、大于运算符和一元正运算符，就好像语句已 `x = (F < A) > (+y)`编写，而不是以*type_argument_list*后跟二元加运算符的*simple_name* 。</span><span class="sxs-lookup"><span data-stu-id="a1735-1007">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple_name* with a *type_argument_list* followed by a binary plus operator.</span></span> <span data-ttu-id="a1735-1008">在语句中</span><span class="sxs-lookup"><span data-stu-id="a1735-1008">In the statement</span></span>
```csharp
x = y is C<T> + z;
```
<span data-ttu-id="a1735-1009">标记 `C<T>` 被解释为具有*type_argument_list*的*namespace_or_type_name* 。</span><span class="sxs-lookup"><span data-stu-id="a1735-1009">the tokens `C<T>` are interpreted as a *namespace_or_type_name* with a *type_argument_list*.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="a1735-1010">调用表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-1010">Invocation expressions</span></span>

<span data-ttu-id="a1735-1011">用于调用方法的*invocation_expression* 。</span><span class="sxs-lookup"><span data-stu-id="a1735-1011">An *invocation_expression* is used to invoke a method.</span></span>

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

<span data-ttu-id="a1735-1012">如果以下至少一项保留，则*invocation_expression*是动态绑定的（[动态绑定](expressions.md#dynamic-binding)）：</span><span class="sxs-lookup"><span data-stu-id="a1735-1012">An *invocation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="a1735-1013">*Primary_expression*具有 `dynamic`的编译时类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1013">The *primary_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="a1735-1014">至少一个可选*argument_list*的参数 `dynamic` 编译时类型，而*primary_expression*没有委托类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1014">At least one argument of the optional *argument_list* has compile-time type `dynamic` and the *primary_expression* does not have a delegate type.</span></span>

<span data-ttu-id="a1735-1015">在这种情况下，编译器将*invocation_expression*分类为 `dynamic`类型的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1015">In this case the compiler classifies the *invocation_expression* as a value of type `dynamic`.</span></span> <span data-ttu-id="a1735-1016">下面的规则用于确定*invocation_expression*的含义，然后使用运行时类型（而不是*primary_expression*的参数和编译时类型 `dynamic`的编译时类型）在运行时应用。</span><span class="sxs-lookup"><span data-stu-id="a1735-1016">The rules below to determine the meaning of the *invocation_expression* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_expression* and arguments which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="a1735-1017">如果*primary_expression*没有 `dynamic`的编译时类型，则方法调用会经历有限的编译时检查，如[编译时检查动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution)中所述。</span><span class="sxs-lookup"><span data-stu-id="a1735-1017">If the *primary_expression* does not have compile-time type `dynamic`, then the method invocation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="a1735-1018">*Invocation_expression*的*primary_expression*必须为方法组或*delegate_type*的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1018">The *primary_expression* of an *invocation_expression* must be a method group or a value of a *delegate_type*.</span></span> <span data-ttu-id="a1735-1019">如果*primary_expression*为方法组，则*invocation_expression*为方法调用（[方法调用](expressions.md#method-invocations)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1019">If the *primary_expression* is a method group, the *invocation_expression* is a method invocation ([Method invocations](expressions.md#method-invocations)).</span></span> <span data-ttu-id="a1735-1020">如果*primary_expression*是*delegate_type*的值，则*Invocation_expression*为委托调用（[委托调用](expressions.md#delegate-invocations)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1020">If the *primary_expression* is a value of a *delegate_type*, the *invocation_expression* is a delegate invocation ([Delegate invocations](expressions.md#delegate-invocations)).</span></span> <span data-ttu-id="a1735-1021">如果*primary_expression*既不是方法组也不是*delegate_type*的值，则发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1021">If the *primary_expression* is neither a method group nor a value of a *delegate_type*, a binding-time error occurs.</span></span>

<span data-ttu-id="a1735-1022">可选*argument_list* （[参数列表](expressions.md#argument-lists)）为方法的参数提供值或变量引用。</span><span class="sxs-lookup"><span data-stu-id="a1735-1022">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) provides values or variable references for the parameters of the method.</span></span>

<span data-ttu-id="a1735-1023">评估*invocation_expression*的结果归类如下：</span><span class="sxs-lookup"><span data-stu-id="a1735-1023">The result of evaluating an *invocation_expression* is classified as follows:</span></span>

*  <span data-ttu-id="a1735-1024">如果*invocation_expression*调用返回 `void`的方法或委托，则结果为 nothing。</span><span class="sxs-lookup"><span data-stu-id="a1735-1024">If the *invocation_expression* invokes a method or delegate that returns `void`, the result is nothing.</span></span> <span data-ttu-id="a1735-1025">仅在*statement_expression* （[expression 语句](statements.md#expression-statements)）的上下文中或作为*lambda_expression* （[匿名函数表达式](expressions.md#anonymous-function-expressions)）的主体时，才允许将表达式归类为 nothing。</span><span class="sxs-lookup"><span data-stu-id="a1735-1025">An expression that is classified as nothing is permitted only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)) or as the body of a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="a1735-1026">否则，会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1026">Otherwise a binding-time error occurs.</span></span>
*  <span data-ttu-id="a1735-1027">否则，结果为方法或委托返回的类型的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1027">Otherwise, the result is a value of the type returned by the method or delegate.</span></span>

#### <a name="method-invocations"></a><span data-ttu-id="a1735-1028">方法调用</span><span class="sxs-lookup"><span data-stu-id="a1735-1028">Method invocations</span></span>

<span data-ttu-id="a1735-1029">对于方法调用， *invocation_expression*的*primary_expression*必须为方法组。</span><span class="sxs-lookup"><span data-stu-id="a1735-1029">For a method invocation, the *primary_expression* of the *invocation_expression* must be a method group.</span></span> <span data-ttu-id="a1735-1030">方法组标识一种要调用的方法或用于从中选择要调用的特定方法的重载方法集。</span><span class="sxs-lookup"><span data-stu-id="a1735-1030">The method group identifies the one method to invoke or the set of overloaded methods from which to choose a specific method to invoke.</span></span> <span data-ttu-id="a1735-1031">在后一种情况下，确定要调用的特定方法的方式取决于*argument_list*中的参数类型所提供的上下文。</span><span class="sxs-lookup"><span data-stu-id="a1735-1031">In the latter case, determination of the specific method to invoke is based on the context provided by the types of the arguments in the *argument_list*.</span></span>

<span data-ttu-id="a1735-1032">对形式的方法调用的绑定时处理 `M(A)`，其中 `M` 是一个方法组（可能包括一个*type_argument_list*），而 `A` 是一个可选*argument_list*，其中包含以下步骤：</span><span class="sxs-lookup"><span data-stu-id="a1735-1032">The binding-time processing of a method invocation of the form `M(A)`, where `M` is a method group (possibly including a *type_argument_list*), and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="a1735-1033">构造方法调用的候选方法集。</span><span class="sxs-lookup"><span data-stu-id="a1735-1033">The set of candidate methods for the method invocation is constructed.</span></span> <span data-ttu-id="a1735-1034">对于每个与方法组相关联 `F` 方法 `M`：</span><span class="sxs-lookup"><span data-stu-id="a1735-1034">For each method `F` associated with the method group `M`:</span></span>
   *  <span data-ttu-id="a1735-1035">如果 `F` 不是泛型的，则在以下情况下 `F` 为候选项：</span><span class="sxs-lookup"><span data-stu-id="a1735-1035">If `F` is non-generic, `F` is a candidate when:</span></span>
      * <span data-ttu-id="a1735-1036">`M` 没有类型参数列表，并且</span><span class="sxs-lookup"><span data-stu-id="a1735-1036">`M` has no type argument list, and</span></span>
      * <span data-ttu-id="a1735-1037">`F` 适用于 `A` （[适用的函数成员](expressions.md#applicable-function-member)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1037">`F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="a1735-1038">如果 `F` 是泛型且 `M` 没有类型参数列表，则在以下情况下 `F` 是候选项：</span><span class="sxs-lookup"><span data-stu-id="a1735-1038">If `F` is generic and `M` has no type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="a1735-1039">类型推理（[类型推理](expressions.md#type-inference)）成功，推断调用的类型参数列表，</span><span class="sxs-lookup"><span data-stu-id="a1735-1039">Type inference ([Type inference](expressions.md#type-inference)) succeeds, inferring a list of type arguments for the call, and</span></span>
      * <span data-ttu-id="a1735-1040">将推断类型参数替换为相应的方法类型参数后，F 的参数列表中的所有构造类型都满足其约束（[满足约束](types.md#satisfying-constraints)），`F` 的参数列表适用于 `A` （[适用的函数成员](expressions.md#applicable-function-member)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1040">Once the inferred type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="a1735-1041">如果 `F` 是泛型并且 `M` 包含类型参数列表，则在以下情况下 `F` 是候选项：</span><span class="sxs-lookup"><span data-stu-id="a1735-1041">If `F` is generic and `M` includes a type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="a1735-1042">`F` 具有与类型参数列表中提供的相同数量的方法类型参数，并且</span><span class="sxs-lookup"><span data-stu-id="a1735-1042">`F` has the same number of method type parameters as were supplied in the type argument list, and</span></span>
      * <span data-ttu-id="a1735-1043">将类型参数替换为相应的方法类型参数后，F 的参数列表中的所有构造类型都满足其约束（[满足约束](types.md#satisfying-constraints)），并且 `F` 的参数列表适用于 `A` （[适用的函数成员](expressions.md#applicable-function-member)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1043">Once the type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
*  <span data-ttu-id="a1735-1044">候选方法集减少为仅包含派生程度最高的类型中的方法：对于集中 `C.F` 的每个方法，其中 `C` 是声明方法 `F` 的类型，而在 `C` 的基类型中声明的所有方法将从集合中删除。</span><span class="sxs-lookup"><span data-stu-id="a1735-1044">The set of candidate methods is reduced to contain only methods from the most derived types: For each method `C.F` in the set, where `C` is the type in which the method `F` is declared, all methods declared in a base type of `C` are removed from the set.</span></span> <span data-ttu-id="a1735-1045">此外，如果 `C` 是 `object`以外的类类型，则将从集合中删除在接口类型中声明的所有方法。</span><span class="sxs-lookup"><span data-stu-id="a1735-1045">Furthermore, if `C` is a class type other than `object`, all methods declared in an interface type are removed from the set.</span></span> <span data-ttu-id="a1735-1046">（仅当方法组是具有非 object 和非空有效接口集的有效基类的类型形参上的成员查找结果时，这一规则才会生效。）</span><span class="sxs-lookup"><span data-stu-id="a1735-1046">(This latter rule only has affect when the method group was the result of a member lookup on a type parameter having an effective base class other than object and a non-empty effective interface set.)</span></span>
*  <span data-ttu-id="a1735-1047">如果生成的候选方法集为空，则将放弃按照以下步骤进行的进一步处理，而是尝试将调用作为扩展方法调用（[扩展方法调用](expressions.md#extension-method-invocations)）进行处理。</span><span class="sxs-lookup"><span data-stu-id="a1735-1047">If the resulting set of candidate methods is empty, then further processing along the following steps are abandoned, and instead an attempt is made to process the invocation as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="a1735-1048">如果此操作失败，则不存在适用的方法，并且发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1048">If this fails, then no applicable methods exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="a1735-1049">使用[重载决策](expressions.md#overload-resolution)的重载决策规则标识候选方法集的最佳方法。</span><span class="sxs-lookup"><span data-stu-id="a1735-1049">The best method of the set of candidate methods is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="a1735-1050">如果无法识别单个最佳方法，则方法调用是不明确的，并发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1050">If a single best method cannot be identified, the method invocation is ambiguous, and a binding-time error occurs.</span></span> <span data-ttu-id="a1735-1051">在执行重载决策时，将在替换相应方法类型参数的类型参数（提供或推断）后考虑泛型方法的参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-1051">When performing overload resolution, the parameters of a generic method are considered after substituting the type arguments (supplied or inferred) for the corresponding method type parameters.</span></span>
*  <span data-ttu-id="a1735-1052">执行所选最佳方法的最终验证：</span><span class="sxs-lookup"><span data-stu-id="a1735-1052">Final validation of the chosen best method is performed:</span></span>
   * <span data-ttu-id="a1735-1053">方法在方法组的上下文中进行验证：如果最佳方法为静态方法，则方法组必须通过类型*simple_name*或*member_access* 。</span><span class="sxs-lookup"><span data-stu-id="a1735-1053">The method is validated in the context of the method group: If the best method is a static method, the method group must have resulted from a *simple_name* or a *member_access* through a type.</span></span> <span data-ttu-id="a1735-1054">如果最佳方法是实例方法，则方法组必须由*simple_name*、 *member_access*通过变量或值或*base_access*导致。</span><span class="sxs-lookup"><span data-stu-id="a1735-1054">If the best method is an instance method, the method group must have resulted from a *simple_name*, a *member_access* through a variable or value, or a *base_access*.</span></span> <span data-ttu-id="a1735-1055">如果这两个要求都不成立，则发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1055">If neither of these requirements is true, a binding-time error occurs.</span></span>
   * <span data-ttu-id="a1735-1056">如果最佳方法是泛型方法，则会对照泛型方法上声明的约束（[满足约束](types.md#satisfying-constraints)）检查类型参数（提供或推断）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1056">If the best method is a generic method, the type arguments (supplied or inferred) are checked against the constraints ([Satisfying constraints](types.md#satisfying-constraints)) declared on the generic method.</span></span> <span data-ttu-id="a1735-1057">如果任何类型自变量不满足类型参数上的相应约束，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1057">If any type argument does not satisfy the corresponding constraint(s) on the type parameter, a binding-time error occurs.</span></span>

<span data-ttu-id="a1735-1058">按照上述步骤在绑定时选择并验证方法后，将根据[动态重载解析的编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)中所述的函数成员调用规则来处理实际的运行时调用。</span><span class="sxs-lookup"><span data-stu-id="a1735-1058">Once a method has been selected and validated at binding-time by the above steps, the actual run-time invocation is processed according to the rules of function member invocation described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="a1735-1059">上述解决方法的直观效果如下所示：若要查找由方法调用调用的特定方法，请从方法调用指示的类型开始，然后继续继承链，直到至少有一个适用的。可访问，但却找到了非替代方法声明。</span><span class="sxs-lookup"><span data-stu-id="a1735-1059">The intuitive effect of the resolution rules described above is as follows: To locate the particular method invoked by a method invocation, start with the type indicated by the method invocation and proceed up the inheritance chain until at least one applicable, accessible, non-override method declaration is found.</span></span> <span data-ttu-id="a1735-1060">然后，对该类型中声明的适用的、可访问的、非重写方法集执行类型推理和重载决策，并调用此方法。</span><span class="sxs-lookup"><span data-stu-id="a1735-1060">Then perform type inference and overload resolution on the set of applicable, accessible, non-override methods declared in that type and invoke the method thus selected.</span></span> <span data-ttu-id="a1735-1061">如果未找到方法，请改为尝试将调用作为扩展方法调用来处理。</span><span class="sxs-lookup"><span data-stu-id="a1735-1061">If no method was found, try instead to process the invocation as an extension method invocation.</span></span>

#### <a name="extension-method-invocations"></a><span data-ttu-id="a1735-1062">扩展方法调用</span><span class="sxs-lookup"><span data-stu-id="a1735-1062">Extension method invocations</span></span>

<span data-ttu-id="a1735-1063">在方法调用中（[装箱实例上的调用](expressions.md#invocations-on-boxed-instances)），其中一个窗体</span><span class="sxs-lookup"><span data-stu-id="a1735-1063">In a method invocation ([Invocations on boxed instances](expressions.md#invocations-on-boxed-instances)) of one of the forms</span></span>
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
<span data-ttu-id="a1735-1064">如果调用的正常处理找不到适用的方法，则会尝试将构造作为扩展方法调用来处理。</span><span class="sxs-lookup"><span data-stu-id="a1735-1064">if the normal processing of the invocation finds no applicable methods, an attempt is made to process the construct as an extension method invocation.</span></span> <span data-ttu-id="a1735-1065">如果*expr*或任意*参数*的编译时类型 `dynamic`，则扩展方法将不适用。</span><span class="sxs-lookup"><span data-stu-id="a1735-1065">If *expr* or any of the *args* has compile-time type `dynamic`, extension methods will not apply.</span></span>

<span data-ttu-id="a1735-1066">目标是查找最佳*type_name* `C`，以便可以进行相应的静态方法调用：</span><span class="sxs-lookup"><span data-stu-id="a1735-1066">The objective is to find the best *type_name* `C`, so that the corresponding static method invocation can take place:</span></span>
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

<span data-ttu-id="a1735-1067">如果满足以下条件，扩展方法 `Ci.Mj`***适用***：</span><span class="sxs-lookup"><span data-stu-id="a1735-1067">An extension method `Ci.Mj` is ***eligible*** if:</span></span>

*  <span data-ttu-id="a1735-1068">`Ci` 是非泛型非泛型类</span><span class="sxs-lookup"><span data-stu-id="a1735-1068">`Ci` is a non-generic, non-nested class</span></span>
*  <span data-ttu-id="a1735-1069">`Mj` 的名称是*标识符*</span><span class="sxs-lookup"><span data-stu-id="a1735-1069">The name of `Mj` is *identifier*</span></span>
*  <span data-ttu-id="a1735-1070">当作为如下所示的静态方法应用于参数时，`Mj` 是可访问和适用的</span><span class="sxs-lookup"><span data-stu-id="a1735-1070">`Mj` is accessible and applicable when applied to the arguments as a static method as shown above</span></span>
*  <span data-ttu-id="a1735-1071">存在从*expr*到 `Mj`的第一个参数类型的隐式标识、引用或装箱转换。</span><span class="sxs-lookup"><span data-stu-id="a1735-1071">An implicit identity, reference or boxing conversion exists from *expr* to the type of the first parameter of `Mj`.</span></span>

<span data-ttu-id="a1735-1072">搜索 `C` 按如下方式进行：</span><span class="sxs-lookup"><span data-stu-id="a1735-1072">The search for `C` proceeds as follows:</span></span>

*  <span data-ttu-id="a1735-1073">从最接近的封闭命名空间声明开始，继续每个封闭命名空间声明，并以包含编译单元结束，接下来尝试查找一组候选的扩展方法：</span><span class="sxs-lookup"><span data-stu-id="a1735-1073">Starting with the closest enclosing namespace declaration, continuing with each enclosing namespace declaration, and ending with the containing compilation unit, successive attempts are made to find a candidate set of extension methods:</span></span>
   * <span data-ttu-id="a1735-1074">如果给定的命名空间或编译单元直接包含 `Ci` 具有符合条件的扩展 `Mj`方法的非泛型类型声明，则这些扩展方法集是候选集。</span><span class="sxs-lookup"><span data-stu-id="a1735-1074">If the given namespace or compilation unit directly contains non-generic type declarations `Ci` with eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
   * <span data-ttu-id="a1735-1075">如果在给定的命名空间或编译单元中*using_namespace_directive*s 导入的命名空间中 `Ci` 导入*using_static_declarations*并直接声明的类型 `Mj`，则这些扩展方法集是候选集。</span><span class="sxs-lookup"><span data-stu-id="a1735-1075">If types `Ci` imported by *using_static_declarations* and directly declared in namespaces imported by *using_namespace_directive*s in the given namespace or compilation unit directly contain eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
*  <span data-ttu-id="a1735-1076">如果在任何封闭命名空间声明或编译单元中均未找到候选集，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1076">If no candidate set is found in any enclosing namespace declaration or compilation unit, a compile-time error occurs.</span></span>
*  <span data-ttu-id="a1735-1077">否则，将重载决策应用于候选集，如中所述（[重载决策](expressions.md#overload-resolution)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1077">Otherwise, overload resolution is applied to the candidate set as described in ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="a1735-1078">如果未找到单个最佳方法，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1078">If no single best method is found, a compile-time error occurs.</span></span>
*  <span data-ttu-id="a1735-1079">`C` 是将最佳方法声明为扩展方法的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1079">`C` is the type within which the best method is declared as an extension method.</span></span>

<span data-ttu-id="a1735-1080">将 `C` 用作目标，然后将方法调用处理为静态方法调用（对[动态重载决策进行编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1080">Using `C` as a target, the method call is then processed as a static method invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="a1735-1081">前面的规则意味着实例方法优先于扩展方法，内部命名空间声明中提供的扩展方法优先于外部命名空间声明中提供的扩展方法和该扩展直接在命名空间中声明的方法优先于通过使用命名空间指令导入到同一命名空间中的扩展方法。</span><span class="sxs-lookup"><span data-stu-id="a1735-1081">The preceding rules mean that instance methods take precedence over extension methods, that extension methods available in inner namespace declarations take precedence over extension methods available in outer namespace declarations, and that extension methods declared directly in a namespace take precedence over extension methods imported into that same namespace with a using namespace directive.</span></span> <span data-ttu-id="a1735-1082">例如：</span><span class="sxs-lookup"><span data-stu-id="a1735-1082">For example:</span></span>
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

<span data-ttu-id="a1735-1083">在此示例中，`B`的方法优先于第一个扩展方法，`C`的方法优先于这两个扩展方法。</span><span class="sxs-lookup"><span data-stu-id="a1735-1083">In the example, `B`'s method takes precedence over the first extension method, and `C`'s method takes precedence over both extension methods.</span></span>

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

<span data-ttu-id="a1735-1084">此示例的输出为：</span><span class="sxs-lookup"><span data-stu-id="a1735-1084">The output of this example is:</span></span>
```console
E.F(1)
D.G(2)
C.H(3)
```
<span data-ttu-id="a1735-1085">`D.G` 优先于 `C.G`，`E.F` 优先于 `D.F` 和 `C.F`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1085">`D.G` takes precedence over `C.G`, and `E.F` takes precedence over both `D.F` and `C.F`.</span></span>

#### <a name="delegate-invocations"></a><span data-ttu-id="a1735-1086">委托调用</span><span class="sxs-lookup"><span data-stu-id="a1735-1086">Delegate invocations</span></span>

<span data-ttu-id="a1735-1087">对于委托调用， *invocation_expression*的*primary_expression*必须是*delegate_type*的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1087">For a delegate invocation, the *primary_expression* of the *invocation_expression* must be a value of a *delegate_type*.</span></span> <span data-ttu-id="a1735-1088">此外，将*delegate_type*视为与*delegate_type*具有相同参数列表的函数成员， *delegate_type*必须适用于与*invocation_expression*的*argument_list*相关的（[适用的函数成员](expressions.md#applicable-function-member)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1088">Furthermore, considering the *delegate_type* to be a function member with the same parameter list as the *delegate_type*, the *delegate_type* must be applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the *argument_list* of the *invocation_expression*.</span></span>

<span data-ttu-id="a1735-1089">`D(A)`格式的委托调用的运行时处理，其中 `D` 是*delegate_type*的*primary_expression* ，`A` 是一个可选的*argument_list*，其中包含以下步骤：</span><span class="sxs-lookup"><span data-stu-id="a1735-1089">The run-time processing of a delegate invocation of the form `D(A)`, where `D` is a *primary_expression* of a *delegate_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="a1735-1090">计算 `D`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1090">`D` is evaluated.</span></span> <span data-ttu-id="a1735-1091">如果此计算导致异常，则不执行其他步骤。</span><span class="sxs-lookup"><span data-stu-id="a1735-1091">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="a1735-1092">检查 `D` 的值是否有效。</span><span class="sxs-lookup"><span data-stu-id="a1735-1092">The value of `D` is checked to be valid.</span></span> <span data-ttu-id="a1735-1093">如果 `null``D` 的值，则将引发 `System.NullReferenceException`，并且不执行任何其他步骤。</span><span class="sxs-lookup"><span data-stu-id="a1735-1093">If the value of `D` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="a1735-1094">否则，`D` 是对委托实例的引用。</span><span class="sxs-lookup"><span data-stu-id="a1735-1094">Otherwise, `D` is a reference to a delegate instance.</span></span> <span data-ttu-id="a1735-1095">对委托调用列表中的每个可调用实体执行函数成员调用（对[动态重载决策进行编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1095">Function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) are performed on each of the callable entities in the invocation list of the delegate.</span></span> <span data-ttu-id="a1735-1096">对于包含实例和实例方法的可调用实体，调用实例是包含在可调用实体中的实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-1096">For callable entities consisting of an instance and instance method, the instance for the invocation is the instance contained in the callable entity.</span></span>

### <a name="element-access"></a><span data-ttu-id="a1735-1097">元素访问</span><span class="sxs-lookup"><span data-stu-id="a1735-1097">Element access</span></span>

<span data-ttu-id="a1735-1098">*Element_access*包含一个*primary_no_array_creation_expression*，后跟一个 "`[`" 标记，后跟一个*argument_list*，后跟一个 "`]`" 标记。</span><span class="sxs-lookup"><span data-stu-id="a1735-1098">An *element_access* consists of a *primary_no_array_creation_expression*, followed by a "`[`" token, followed by an *argument_list*, followed by a "`]`" token.</span></span> <span data-ttu-id="a1735-1099">*Argument_list*由一个或多个由逗号分隔的*参数*组成。</span><span class="sxs-lookup"><span data-stu-id="a1735-1099">The *argument_list* consists of one or more *argument*s, separated by commas.</span></span>

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

<span data-ttu-id="a1735-1100">不允许*element_access*的*argument_list*包含 `ref` 或 `out` 参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-1100">The *argument_list* of an *element_access* is not allowed to contain `ref` or `out` arguments.</span></span>

<span data-ttu-id="a1735-1101">如果以下至少一项保留，则*element_access*是动态绑定的（[动态绑定](expressions.md#dynamic-binding)）：</span><span class="sxs-lookup"><span data-stu-id="a1735-1101">An *element_access* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="a1735-1102">*Primary_no_array_creation_expression*具有 `dynamic`的编译时类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1102">The *primary_no_array_creation_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="a1735-1103">至少一个*argument_list*的表达式 `dynamic` 有编译时类型，而*primary_no_array_creation_expression*没有数组类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1103">At least one expression of the *argument_list* has compile-time type `dynamic` and the *primary_no_array_creation_expression* does not have an array type.</span></span>

<span data-ttu-id="a1735-1104">在这种情况下，编译器将*element_access*分类为 `dynamic`类型的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1104">In this case the compiler classifies the *element_access* as a value of type `dynamic`.</span></span> <span data-ttu-id="a1735-1105">下面的规则用于确定*element_access*的含义，然后使用运行时类型（而不是*primary_no_array_creation_expression*和*argument_list*具有编译时类型 `dynamic`的表达式的编译时类型）在运行时应用。</span><span class="sxs-lookup"><span data-stu-id="a1735-1105">The rules below to determine the meaning of the *element_access* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_no_array_creation_expression* and *argument_list* expressions which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="a1735-1106">如果*primary_no_array_creation_expression*没有 `dynamic`的编译时类型，则元素访问会经历有限的编译时检查，如对[动态重载决策进行编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)中所述。</span><span class="sxs-lookup"><span data-stu-id="a1735-1106">If the *primary_no_array_creation_expression* does not have compile-time type `dynamic`, then the element access undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="a1735-1107">如果*element_access*的*primary_no_array_creation_expression*是*array_type*的值，则*element_access*是数组访问（[数组访问](expressions.md#array-access)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1107">If the *primary_no_array_creation_expression* of an *element_access* is a value of an *array_type*, the *element_access* is an array access ([Array access](expressions.md#array-access)).</span></span> <span data-ttu-id="a1735-1108">否则， *primary_no_array_creation_expression*必须是具有一个或多个索引器成员的类、结构或接口类型的变量或值，在这种情况下， *element_access*是索引器访问（[索引器访问](expressions.md#indexer-access)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1108">Otherwise, the *primary_no_array_creation_expression* must be a variable or value of a class, struct, or interface type that has one or more indexer members, in which case the *element_access* is an indexer access ([Indexer access](expressions.md#indexer-access)).</span></span>

#### <a name="array-access"></a><span data-ttu-id="a1735-1109">数组访问</span><span class="sxs-lookup"><span data-stu-id="a1735-1109">Array access</span></span>

<span data-ttu-id="a1735-1110">对于数组访问， *element_access*的*primary_no_array_creation_expression*必须是*array_type*的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1110">For an array access, the *primary_no_array_creation_expression* of the *element_access* must be a value of an *array_type*.</span></span> <span data-ttu-id="a1735-1111">而且，不允许数组访问的*argument_list*包含命名参数。*Argument_list*中的表达式的数目必须与*array_type*的排名相同，并且每个表达式的类型必须为 `int`、`uint`、`long`、`ulong`，或者必须能够隐式转换为这些类型中的一个或多个。</span><span class="sxs-lookup"><span data-stu-id="a1735-1111">Furthermore, the *argument_list* of an array access is not allowed to contain named arguments.The number of expressions in the *argument_list* must be the same as the rank of the *array_type*, and each expression must be of type `int`, `uint`, `long`, `ulong`, or must be implicitly convertible to one or more of these types.</span></span>

<span data-ttu-id="a1735-1112">计算数组访问的结果是数组的元素类型的一个变量，即由*argument_list*中的表达式的值所选择的数组元素。</span><span class="sxs-lookup"><span data-stu-id="a1735-1112">The result of evaluating an array access is a variable of the element type of the array, namely the array element selected by the value(s) of the expression(s) in the *argument_list*.</span></span>

<span data-ttu-id="a1735-1113">`P[A]`格式的数组访问的运行时处理，其中 `P` 是*array_type*的*primary_no_array_creation_expression* ，`A` 是*argument_list*，其中包含以下步骤：</span><span class="sxs-lookup"><span data-stu-id="a1735-1113">The run-time processing of an array access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of an *array_type* and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="a1735-1114">计算 `P`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1114">`P` is evaluated.</span></span> <span data-ttu-id="a1735-1115">如果此计算导致异常，则不执行其他步骤。</span><span class="sxs-lookup"><span data-stu-id="a1735-1115">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="a1735-1116">将按从左到右的顺序计算*argument_list*的索引表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-1116">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="a1735-1117">对于每个索引表达式的求值，将执行隐式转换（[隐式](conversions.md#implicit-conversions)转换）为以下类型之一： `int`、`uint`、`long``ulong`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1117">Following evaluation of each index expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="a1735-1118">在此列表中选择了隐式转换的第一种类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1118">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="a1735-1119">例如，如果索引表达式的类型为 `short` 则执行到 `int` 的隐式转换，因为从 `short` 到 `int` 的隐式转换和从 `short` 到 `long` 都是可能的。</span><span class="sxs-lookup"><span data-stu-id="a1735-1119">For instance, if the index expression is of type `short` then an implicit conversion to `int` is performed, since implicit conversions from `short` to `int` and from `short` to `long` are possible.</span></span> <span data-ttu-id="a1735-1120">如果索引表达式的计算或后续的隐式转换导致异常，则不会再计算索引表达式，也不会执行任何其他步骤。</span><span class="sxs-lookup"><span data-stu-id="a1735-1120">If evaluation of an index expression or the subsequent implicit conversion causes an exception, then no further index expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="a1735-1121">检查 `P` 的值是否有效。</span><span class="sxs-lookup"><span data-stu-id="a1735-1121">The value of `P` is checked to be valid.</span></span> <span data-ttu-id="a1735-1122">如果 `null``P` 的值，则将引发 `System.NullReferenceException`，并且不执行任何其他步骤。</span><span class="sxs-lookup"><span data-stu-id="a1735-1122">If the value of `P` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="a1735-1123">根据 `P`引用的数组实例的每个维度的实际界限检查*argument_list*中每个表达式的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1123">The value of each expression in the *argument_list* is checked against the actual bounds of each dimension of the array instance referenced by `P`.</span></span> <span data-ttu-id="a1735-1124">如果一个或多个值超出范围，则会引发 `System.IndexOutOfRangeException`，而不执行进一步的步骤。</span><span class="sxs-lookup"><span data-stu-id="a1735-1124">If one or more values are out of range, a `System.IndexOutOfRangeException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="a1735-1125">计算索引表达式给定的数组元素的位置，此位置成为数组访问的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1125">The location of the array element given by the index expression(s) is computed, and this location becomes the result of the array access.</span></span>

#### <a name="indexer-access"></a><span data-ttu-id="a1735-1126">索引器访问</span><span class="sxs-lookup"><span data-stu-id="a1735-1126">Indexer access</span></span>

<span data-ttu-id="a1735-1127">对于索引器访问， *element_access*的*primary_no_array_creation_expression*必须是类、结构或接口类型的变量或值，并且此类型必须实现适用于*element_access*的*argument_list*的一个或多个索引器。</span><span class="sxs-lookup"><span data-stu-id="a1735-1127">For an indexer access, the *primary_no_array_creation_expression* of the *element_access* must be a variable or value of a class, struct, or interface type, and this type must implement one or more indexers that are applicable with respect to the *argument_list* of the *element_access*.</span></span>

<span data-ttu-id="a1735-1128">对窗体 `P[A]`的索引器访问的绑定时处理，其中 `P` 是类、结构或接口类型 `T`的*primary_no_array_creation_expression* ，`A` 是*argument_list*，其中包含以下步骤：</span><span class="sxs-lookup"><span data-stu-id="a1735-1128">The binding-time processing of an indexer access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of a class, struct, or interface type `T`, and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="a1735-1129">构造由 `T` 提供的一组索引器。</span><span class="sxs-lookup"><span data-stu-id="a1735-1129">The set of indexers provided by `T` is constructed.</span></span> <span data-ttu-id="a1735-1130">该集包含在 `T` 中声明的所有索引器，或不 `override` 声明并且可在当前上下文（[成员访问](basic-concepts.md#member-access)）中访问的 `T` 基类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1130">The set consists of all indexers declared in `T` or a base type of `T` that are not `override` declarations and are accessible in the current context ([Member access](basic-concepts.md#member-access)).</span></span>
*  <span data-ttu-id="a1735-1131">该集被简化为那些适用且不被其他索引器隐藏的索引器。</span><span class="sxs-lookup"><span data-stu-id="a1735-1131">The set is reduced to those indexers that are applicable and not hidden by other indexers.</span></span> <span data-ttu-id="a1735-1132">以下规则应用于集中的每个索引器 `S.I`，其中 `S` 是声明索引器 `I` 的类型：</span><span class="sxs-lookup"><span data-stu-id="a1735-1132">The following rules are applied to each indexer `S.I` in the set, where `S` is the type in which the indexer `I` is declared:</span></span>
   * <span data-ttu-id="a1735-1133">如果 `I` 不适用于 `A` （[适用的函数成员](expressions.md#applicable-function-member)），则从集中删除 `I`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1133">If `I` is not applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then `I` is removed from the set.</span></span>
   * <span data-ttu-id="a1735-1134">如果 `I` 适用于 `A` （[适用的函数成员](expressions.md#applicable-function-member)），则将从集合中删除在 `S` 的基类型中声明的所有索引器。</span><span class="sxs-lookup"><span data-stu-id="a1735-1134">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then all indexers declared in a base type of `S` are removed from the set.</span></span>
   * <span data-ttu-id="a1735-1135">如果 `I` 适用于 `A` （[适用的函数成员](expressions.md#applicable-function-member)），而 `S` 是 `object`以外的类类型，则将从集合中删除在接口中声明的所有索引器。</span><span class="sxs-lookup"><span data-stu-id="a1735-1135">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)) and `S` is a class type other than `object`, all indexers declared in an interface are removed from the set.</span></span>
*  <span data-ttu-id="a1735-1136">如果生成的候选索引器集为空，则不存在适用的索引器，并且发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1136">If the resulting set of candidate indexers is empty, then no applicable indexers exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="a1735-1137">使用[重载决策](expressions.md#overload-resolution)的重载决策规则识别候选索引器集的最佳索引器。</span><span class="sxs-lookup"><span data-stu-id="a1735-1137">The best indexer of the set of candidate indexers is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="a1735-1138">如果无法识别单个最佳索引器，索引器访问不明确，并发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1138">If a single best indexer cannot be identified, the indexer access is ambiguous, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="a1735-1139">将按从左到右的顺序计算*argument_list*的索引表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-1139">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="a1735-1140">处理索引器访问的结果是分类为索引器访问的表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-1140">The result of processing the indexer access is an expression classified as an indexer access.</span></span> <span data-ttu-id="a1735-1141">索引器访问表达式引用前面步骤中确定的索引器，并且具有 `P` 的关联实例表达式和 `A`的关联自变量列表。</span><span class="sxs-lookup"><span data-stu-id="a1735-1141">The indexer access expression references the indexer determined in the step above, and has an associated instance expression of `P` and an associated argument list of `A`.</span></span>

<span data-ttu-id="a1735-1142">索引器访问会导致索引器的*get*访问器或*set 访问*器的调用，具体取决于使用它的上下文。</span><span class="sxs-lookup"><span data-stu-id="a1735-1142">Depending on the context in which it is used, an indexer access causes invocation of either the *get accessor* or the *set accessor* of the indexer.</span></span> <span data-ttu-id="a1735-1143">如果索引器访问是赋值的目标，则调用*set 访问器*来分配新值（[简单赋值](expressions.md#simple-assignment)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1143">If the indexer access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="a1735-1144">在所有其他情况下，将调用*get 访问器*以获取当前值（[表达式的值](expressions.md#values-of-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1144">In all other cases, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="this-access"></a><span data-ttu-id="a1735-1145">此访问</span><span class="sxs-lookup"><span data-stu-id="a1735-1145">This access</span></span>

<span data-ttu-id="a1735-1146">*This_access*包含保留字 `this`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1146">A *this_access* consists of the reserved word `this`.</span></span>

```antlr
this_access
    : 'this'
    ;
```

<span data-ttu-id="a1735-1147">只能在实例构造函数、实例方法或实例访问器的*块*中使用*this_access* 。</span><span class="sxs-lookup"><span data-stu-id="a1735-1147">A *this_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="a1735-1148">它具有下列含义之一：</span><span class="sxs-lookup"><span data-stu-id="a1735-1148">It has one of the following meanings:</span></span>

*  <span data-ttu-id="a1735-1149">当在类的实例构造函数中的*primary_expression*中使用 `this` 时，它将分类为值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1149">When `this` is used in a *primary_expression* within an instance constructor of a class, it is classified as a value.</span></span> <span data-ttu-id="a1735-1150">值的类型为在其中发生使用的类的实例类型（[实例类型](classes.md#the-instance-type)），值是对正在构造的对象的引用。</span><span class="sxs-lookup"><span data-stu-id="a1735-1150">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object being constructed.</span></span>
*  <span data-ttu-id="a1735-1151">当在类的实例方法或实例访问器中的*primary_expression*中使用 `this` 时，它将分类为值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1151">When `this` is used in a *primary_expression* within an instance method or instance accessor of a class, it is classified as a value.</span></span> <span data-ttu-id="a1735-1152">值的类型为在其中发生使用的类的实例类型（[实例类型](classes.md#the-instance-type)），值是对为其调用方法或访问器的对象的引用。</span><span class="sxs-lookup"><span data-stu-id="a1735-1152">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object for which the method or accessor was invoked.</span></span>
*  <span data-ttu-id="a1735-1153">如果在结构的实例构造函数中的*primary_expression*中使用 `this`，则将其归类为变量。</span><span class="sxs-lookup"><span data-stu-id="a1735-1153">When `this` is used in a *primary_expression* within an instance constructor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="a1735-1154">变量的类型是在其中发生使用的结构的实例类型（[实例类型](classes.md#the-instance-type)），而变量表示正在构造的结构。</span><span class="sxs-lookup"><span data-stu-id="a1735-1154">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs, and the variable represents the struct being constructed.</span></span> <span data-ttu-id="a1735-1155">结构的实例构造函数的 `this` 变量的行为与结构类型的 `out` 参数完全相同，具体而言，这意味着必须在实例构造函数的每个执行路径中明确分配变量。</span><span class="sxs-lookup"><span data-stu-id="a1735-1155">The `this` variable of an instance constructor of a struct behaves exactly the same as an `out` parameter of the struct type—in particular, this means that the variable must be definitely assigned in every execution path of the instance constructor.</span></span>
*  <span data-ttu-id="a1735-1156">如果在结构的实例方法或实例访问器中的*primary_expression*中使用 `this`，则将其归类为变量。</span><span class="sxs-lookup"><span data-stu-id="a1735-1156">When `this` is used in a *primary_expression* within an instance method or instance accessor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="a1735-1157">变量的类型是发生使用的结构的实例类型（[实例类型](classes.md#the-instance-type)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1157">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs.</span></span>
   * <span data-ttu-id="a1735-1158">如果方法或访问器不是迭代器（[迭代](classes.md#iterators)器），则 `this` 变量表示为其调用方法或访问器的结构，其行为与结构类型的 `ref` 参数完全相同。</span><span class="sxs-lookup"><span data-stu-id="a1735-1158">If the method or accessor is not an iterator ([Iterators](classes.md#iterators)), the `this` variable represents the struct for which the method or accessor was invoked, and behaves exactly the same as a `ref` parameter of the struct type.</span></span>
   * <span data-ttu-id="a1735-1159">如果方法或访问器是迭代器，则 `this` 变量表示为其调用了方法或访问器的结构的副本，其行为与结构类型的值参数完全相同。</span><span class="sxs-lookup"><span data-stu-id="a1735-1159">If the method or accessor is an iterator, the `this` variable represents a copy of the struct for which the method or accessor was invoked, and behaves exactly the same as a value parameter of the struct type.</span></span>

<span data-ttu-id="a1735-1160">如果在不是上面列出的上下文中的*primary_expression*中使用 `this`，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1160">Use of `this` in a *primary_expression* in a context other than the ones listed above is a compile-time error.</span></span> <span data-ttu-id="a1735-1161">具体而言，不能引用静态方法、静态属性访问器或字段声明的*variable_initializer*中的 `this`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1161">In particular, it is not possible to refer to `this` in a static method, a static property accessor, or in a *variable_initializer* of a field declaration.</span></span>

### <a name="base-access"></a><span data-ttu-id="a1735-1162">基本访问权限</span><span class="sxs-lookup"><span data-stu-id="a1735-1162">Base access</span></span>

<span data-ttu-id="a1735-1163">*Base_access*包含保留字 `base` 后跟 "`.`" 标记、标识符或用方括号括起来的*argument_list* ：</span><span class="sxs-lookup"><span data-stu-id="a1735-1163">A *base_access* consists of the reserved word `base` followed by either a "`.`" token and an identifier or an *argument_list* enclosed in square brackets:</span></span>

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

<span data-ttu-id="a1735-1164">*Base_access*用于访问当前类或结构中名称类似的成员隐藏的基类成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-1164">A *base_access* is used to access base class members that are hidden by similarly named members in the current class or struct.</span></span> <span data-ttu-id="a1735-1165">只能在实例构造函数、实例方法或实例访问器的*块*中使用*base_access* 。</span><span class="sxs-lookup"><span data-stu-id="a1735-1165">A *base_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="a1735-1166">当在类或结构中发生 `base.I` 时，`I` 必须表示该类或结构的基类的成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-1166">When `base.I` occurs in a class or struct, `I` must denote a member of the base class of that class or struct.</span></span> <span data-ttu-id="a1735-1167">同样，在类中发生 `base[E]` 时，基类中必须存在适用的索引器。</span><span class="sxs-lookup"><span data-stu-id="a1735-1167">Likewise, when `base[E]` occurs in a class, an applicable indexer must exist in the base class.</span></span>

<span data-ttu-id="a1735-1168">在绑定时，`base.I` 和 `base[E]` 形式的*base_access*表达式的计算方式与 `((B)this).I` 和 `((B)this)[E]`编写时完全一样，其中 `B` 是出现构造的类或结构的基类。</span><span class="sxs-lookup"><span data-stu-id="a1735-1168">At binding-time, *base_access* expressions of the form `base.I` and `base[E]` are evaluated exactly as if they were written `((B)this).I` and `((B)this)[E]`, where `B` is the base class of the class or struct in which the construct occurs.</span></span> <span data-ttu-id="a1735-1169">因此，`base.I` 和 `base[E]` 对应于 `this.I` 和 `this[E]`，只不过 `this` 被视为基类的实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-1169">Thus, `base.I` and `base[E]` correspond to `this.I` and `this[E]`, except `this` is viewed as an instance of the base class.</span></span>

<span data-ttu-id="a1735-1170">当*base_access*引用虚拟函数成员（方法、属性或索引器）时，将更改在运行时调用哪个函数成员（[动态重载决策的编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1170">When a *base_access* references a virtual function member (a method, property, or indexer), the determination of which function member to invoke at run-time ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is changed.</span></span> <span data-ttu-id="a1735-1171">调用的函数成员是通过查找与 `B` 相关的函数成员的派生程度最高（[虚拟方法](classes.md#virtual-methods)）来确定的，而不是与 `this`的运行时类型相关，而在非基本访问中通常是如此。</span><span class="sxs-lookup"><span data-stu-id="a1735-1171">The function member that is invoked is determined by finding the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of the function member with respect to `B` (instead of with respect to the run-time type of `this`, as would be usual in a non-base access).</span></span> <span data-ttu-id="a1735-1172">因此，在 `virtual` 函数成员的 `override` 中， *base_access*可用于调用函数成员的继承实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-1172">Thus, within an `override` of a `virtual` function member, a *base_access* can be used to invoke the inherited implementation of the function member.</span></span> <span data-ttu-id="a1735-1173">如果*base_access*引用的函数成员是抽象的，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1173">If the function member referenced by a *base_access* is abstract, a binding-time error occurs.</span></span>

### <a name="postfix-increment-and-decrement-operators"></a><span data-ttu-id="a1735-1174">后缀增量和减量运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-1174">Postfix increment and decrement operators</span></span>

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

<span data-ttu-id="a1735-1175">后缀递增或递减运算的操作数必须是分类为变量、属性访问或索引器访问的表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-1175">The operand of a postfix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="a1735-1176">操作的结果是与操作数类型相同的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1176">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="a1735-1177">如果*primary_expression*具有编译时类型 `dynamic` 则运算符是动态绑定的（[动态绑定](expressions.md#dynamic-binding)）， *post_increment_expression*或*post_decrement_expression*具有编译时类型 `dynamic` 并且以下规则在运行时使用*primary_expression*的运行时类型应用。</span><span class="sxs-lookup"><span data-stu-id="a1735-1177">If the *primary_expression* has the compile-time type `dynamic` then the operator is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), the *post_increment_expression* or *post_decrement_expression* has the compile-time type `dynamic` and the following rules are applied at run-time using the run-time type of the *primary_expression*.</span></span>

<span data-ttu-id="a1735-1178">如果后缀递增或递减运算的操作数是属性或索引器访问，则属性或索引器必须同时具有 `get` 和 `set` 访问器。</span><span class="sxs-lookup"><span data-stu-id="a1735-1178">If the operand of a postfix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="a1735-1179">如果不是这种情况，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1179">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="a1735-1180">一元运算符重载决策（[一元运算符重载决策](expressions.md#unary-operator-overload-resolution)）应用于选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-1180">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a1735-1181">为以下类型存在预定义的 `++` 和 `--` 运算符： `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`和任何枚举类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1181">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="a1735-1182">预定义的 `++` 运算符返回通过向操作数添加1而生成的值，预定义的 `--` 运算符返回通过从操作数中减去1而生成的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1182">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="a1735-1183">在 `checked` 上下文中，如果此加法或减法的结果超出了结果类型的范围，且结果类型为整型类型或枚举类型，则将引发 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1183">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="a1735-1184">`x++` 或 `x--` 形式的后缀递增或递减运算的运行时处理包括以下步骤：</span><span class="sxs-lookup"><span data-stu-id="a1735-1184">The run-time processing of a postfix increment or decrement operation of the form `x++` or `x--` consists of the following steps:</span></span>

*   <span data-ttu-id="a1735-1185">如果 `x` 归类为变量：</span><span class="sxs-lookup"><span data-stu-id="a1735-1185">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="a1735-1186">计算 `x` 以产生变量。</span><span class="sxs-lookup"><span data-stu-id="a1735-1186">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="a1735-1187">保存 `x` 的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1187">The value of `x` is saved.</span></span>
    * <span data-ttu-id="a1735-1188">将调用选定的运算符，并将 `x` 的保存值作为其参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-1188">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="a1735-1189">运算符返回的值存储在 `x`的计算给定的位置。</span><span class="sxs-lookup"><span data-stu-id="a1735-1189">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="a1735-1190">的保存值 `x` 成为操作的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1190">The saved value of `x` becomes the result of the operation.</span></span>
*   <span data-ttu-id="a1735-1191">如果 `x` 归类为属性或索引器访问：</span><span class="sxs-lookup"><span data-stu-id="a1735-1191">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="a1735-1192">实例表达式（如果 `x` 不 `static`）和参数列表（如果 `x` 是索引器访问）与 `x` 关联，并在后续的 `get` 和 `set` 访问器调用中使用结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1192">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="a1735-1193">调用 `x` 的 `get` 访问器，并保存返回值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1193">The `get` accessor of `x` is invoked and the returned value is saved.</span></span>
    * <span data-ttu-id="a1735-1194">将调用选定的运算符，并将 `x` 的保存值作为其参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-1194">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="a1735-1195">使用运算符作为其 `value` 参数返回的值调用 `x` 的 `set` 访问器。</span><span class="sxs-lookup"><span data-stu-id="a1735-1195">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="a1735-1196">的保存值 `x` 成为操作的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1196">The saved value of `x` becomes the result of the operation.</span></span>

<span data-ttu-id="a1735-1197">`++` 和 `--` 运算符还支持前缀表示法（[前缀增量和减量运算符](expressions.md#prefix-increment-and-decrement-operators)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1197">The `++` and `--` operators also support prefix notation ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="a1735-1198">通常，`x++` 或 `x--` 的结果是操作前 `x` 的值，而 `++x` 或 `--x` 的结果是操作后 `x` 的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1198">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="a1735-1199">在任一情况下，操作后 `x` 本身具有相同的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1199">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="a1735-1200">可以使用后缀或前缀表示法来调用 `operator ++` 或 `operator --` 实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-1200">An `operator ++` or `operator --` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="a1735-1201">对于这两个表示法，不能有单独的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-1201">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="the-new-operator"></a><span data-ttu-id="a1735-1202">调用 new 运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-1202">The new operator</span></span>

<span data-ttu-id="a1735-1203">`new` 运算符用于创建类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-1203">The `new` operator is used to create new instances of types.</span></span>

<span data-ttu-id="a1735-1204">有三种形式的 `new` 表达式：</span><span class="sxs-lookup"><span data-stu-id="a1735-1204">There are three forms of `new` expressions:</span></span>

*  <span data-ttu-id="a1735-1205">对象创建表达式用于创建类类型和值类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-1205">Object creation expressions are used to create new instances of class types and value types.</span></span>
*  <span data-ttu-id="a1735-1206">数组创建表达式用于创建数组类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-1206">Array creation expressions are used to create new instances of array types.</span></span>
*  <span data-ttu-id="a1735-1207">委托创建表达式用于创建委托类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-1207">Delegate creation expressions are used to create new instances of delegate types.</span></span>

<span data-ttu-id="a1735-1208">`new` 运算符表示创建类型的实例，但不一定表示动态分配内存。</span><span class="sxs-lookup"><span data-stu-id="a1735-1208">The `new` operator implies creation of an instance of a type, but does not necessarily imply dynamic allocation of memory.</span></span> <span data-ttu-id="a1735-1209">具体而言，值类型的实例不需要超出它们所在的变量的额外内存，并且当 `new` 用于创建值类型的实例时，不会发生动态分配。</span><span class="sxs-lookup"><span data-stu-id="a1735-1209">In particular, instances of value types require no additional memory beyond the variables in which they reside, and no dynamic allocations occur when `new` is used to create instances of value types.</span></span>

#### <a name="object-creation-expressions"></a><span data-ttu-id="a1735-1210">对象创建表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-1210">Object creation expressions</span></span>

<span data-ttu-id="a1735-1211">*Object_creation_expression*用于创建*class_type*或*value_type*的新实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-1211">An *object_creation_expression* is used to create a new instance of a *class_type* or a *value_type*.</span></span>

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

<span data-ttu-id="a1735-1212">*Object_creation_expression*的*类型*必须是*class_type*、 *value_type*或*type_parameter*。</span><span class="sxs-lookup"><span data-stu-id="a1735-1212">The *type* of an *object_creation_expression* must be a *class_type*, a *value_type* or a *type_parameter*.</span></span> <span data-ttu-id="a1735-1213">*类型*不能是 `abstract` *class_type*。</span><span class="sxs-lookup"><span data-stu-id="a1735-1213">The *type* cannot be an `abstract` *class_type*.</span></span>

<span data-ttu-id="a1735-1214">仅当*类型*为*class_type*或*struct_type*时，才允许使用可选*argument_list* （[参数列表](expressions.md#argument-lists)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1214">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) is permitted only if the *type* is a *class_type* or a *struct_type*.</span></span>

<span data-ttu-id="a1735-1215">如果对象创建表达式包括对象初始值设定项或集合初始值设定项，则它可以省略构造函数参数列表和外括号。</span><span class="sxs-lookup"><span data-stu-id="a1735-1215">An object creation expression can omit the constructor argument list and enclosing parentheses provided it includes an object initializer or collection initializer.</span></span> <span data-ttu-id="a1735-1216">省略构造函数参数列表和外括号等效于指定空参数列表。</span><span class="sxs-lookup"><span data-stu-id="a1735-1216">Omitting the constructor argument list and enclosing parentheses is equivalent to specifying an empty argument list.</span></span>

<span data-ttu-id="a1735-1217">处理包括对象初始值设定项或集合初始值设定项的对象创建表达式包括首先处理实例构造函数，然后处理对象初始值设定项（[对象初始值设定](expressions.md#object-initializers)项）或集合初始值设定项（[集合](expressions.md#collection-initializers)初始值设定项）指定的成员或元素初始化。</span><span class="sxs-lookup"><span data-stu-id="a1735-1217">Processing of an object creation expression that includes an object initializer or collection initializer consists of first processing the instance constructor and then processing the member or element initializations specified by the object initializer ([Object initializers](expressions.md#object-initializers)) or collection initializer ([Collection initializers](expressions.md#collection-initializers)).</span></span>

<span data-ttu-id="a1735-1218">如果可选*argument_list*中的任何参数具有编译时类型 `dynamic` 则*object_creation_expression*为动态绑定（[动态绑定](expressions.md#dynamic-binding)），并且以下规则在运行时使用编译时类型为 `dynamic`的*argument_list*的参数的运行时类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1218">If any of the arguments in the optional *argument_list* has the compile-time type `dynamic` then the *object_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) and the following rules are applied at run-time using the run-time type of those arguments of the *argument_list* that have the compile time type `dynamic`.</span></span> <span data-ttu-id="a1735-1219">但是，对象创建会经历有限的编译时检查，如对[动态重载决策进行编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)中所述。</span><span class="sxs-lookup"><span data-stu-id="a1735-1219">However, the object creation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="a1735-1220">格式为 `new T(A)`的*object_creation_expression*的绑定时处理，其中 `T` 是*class_type*或*value_type* ，`A` 是可选的*argument_list*，其中包含以下步骤：</span><span class="sxs-lookup"><span data-stu-id="a1735-1220">The binding-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is a *class_type* or a *value_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="a1735-1221">如果 `T` 是*value_type*并且 `A` 不存在：</span><span class="sxs-lookup"><span data-stu-id="a1735-1221">If `T` is a *value_type* and `A` is not present:</span></span>
    * <span data-ttu-id="a1735-1222">*Object_creation_expression*是默认的构造函数调用。</span><span class="sxs-lookup"><span data-stu-id="a1735-1222">The *object_creation_expression* is a default constructor invocation.</span></span> <span data-ttu-id="a1735-1223">*Object_creation_expression*的结果是 `T`类型的值，即在 system.string[类型](types.md#the-systemvaluetype-type)中定义 `T` 的默认值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1223">The result of the *object_creation_expression* is a value of type `T`, namely the default value for `T` as defined in [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>
*   <span data-ttu-id="a1735-1224">否则，如果 `T` 是*type_parameter*并且 `A` 不存在：</span><span class="sxs-lookup"><span data-stu-id="a1735-1224">Otherwise, if `T` is a *type_parameter* and `A` is not present:</span></span>
    * <span data-ttu-id="a1735-1225">如果没有为 `T`指定任何值类型约束或构造函数约束（[类型参数约束](classes.md#type-parameter-constraints)），将发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1225">If no value type constraint or constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) has been specified for `T`, a binding-time error occurs.</span></span>
    * <span data-ttu-id="a1735-1226">*Object_creation_expression*的结果是类型参数已绑定到的运行时类型的值，即调用该类型的默认构造函数的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1226">The result of the *object_creation_expression* is a value of the run-time type that the type parameter has been bound to, namely the result of invoking the default constructor of that type.</span></span> <span data-ttu-id="a1735-1227">运行时类型可以是引用类型或值类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1227">The run-time type may be a reference type or a value type.</span></span>
*   <span data-ttu-id="a1735-1228">否则，如果 `T` 是*class_type*或*struct_type*：</span><span class="sxs-lookup"><span data-stu-id="a1735-1228">Otherwise, if `T` is a *class_type* or a *struct_type*:</span></span>
    * <span data-ttu-id="a1735-1229">如果 `T` 是 `abstract` *class_type*，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1229">If `T` is an `abstract` *class_type*, a compile-time error occurs.</span></span>
    * <span data-ttu-id="a1735-1230">使用[重载决策](expressions.md#overload-resolution)的重载决策规则确定要调用的实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="a1735-1230">The instance constructor to invoke is determined using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="a1735-1231">候选实例构造函数集由 `T` 中声明的所有可访问实例构造函数组成，它们适用于 `A` （[适用的函数成员](expressions.md#applicable-function-member)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1231">The set of candidate instance constructors consists of all accessible instance constructors declared in `T` which are applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="a1735-1232">如果候选实例构造函数集为空，或者无法识别单个最佳实例构造函数，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1232">If the set of candidate instance constructors is empty, or if a single best instance constructor cannot be identified, a binding-time error occurs.</span></span>
    * <span data-ttu-id="a1735-1233">*Object_creation_expression*的结果是 `T`类型的值，即通过调用上述步骤中确定的实例构造函数生成的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1233">The result of the *object_creation_expression* is a value of type `T`, namely the value produced by invoking the instance constructor determined in the step above.</span></span>
*  <span data-ttu-id="a1735-1234">否则， *object_creation_expression*无效，并发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1234">Otherwise, the *object_creation_expression* is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="a1735-1235">即使*object_creation_expression*是动态绑定的，编译时类型仍会 `T`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1235">Even if the *object_creation_expression* is dynamically bound, the compile-time type is still `T`.</span></span>

<span data-ttu-id="a1735-1236">格式为 `new T(A)`的*object_creation_expression*的运行时处理，其中 `T` 是*class_type*或*struct_type* ，`A` 是可选的*argument_list*，其中包含以下步骤：</span><span class="sxs-lookup"><span data-stu-id="a1735-1236">The run-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is *class_type* or a *struct_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="a1735-1237">如果 `T` 是*class_type*：</span><span class="sxs-lookup"><span data-stu-id="a1735-1237">If `T` is a *class_type*:</span></span>
    * <span data-ttu-id="a1735-1238">分配 `T` 类的新实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-1238">A new instance of class `T` is allocated.</span></span> <span data-ttu-id="a1735-1239">如果内存不足，无法分配新的实例，则会引发 `System.OutOfMemoryException`，而不执行进一步的步骤。</span><span class="sxs-lookup"><span data-stu-id="a1735-1239">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="a1735-1240">新实例的所有字段都初始化为其默认值（[默认值](variables.md#default-values)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1240">All fields of the new instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
    * <span data-ttu-id="a1735-1241">实例构造函数根据函数成员调用的规则进行调用（对[动态重载决策进行编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1241">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="a1735-1242">对新分配的实例的引用会自动传递到实例构造函数，并且可以从该构造函数内作为 `this`访问该实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-1242">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>
*   <span data-ttu-id="a1735-1243">如果 `T` 是*struct_type*：</span><span class="sxs-lookup"><span data-stu-id="a1735-1243">If `T` is a *struct_type*:</span></span>
    * <span data-ttu-id="a1735-1244">`T` 类型的实例是通过分配临时本地变量创建的。</span><span class="sxs-lookup"><span data-stu-id="a1735-1244">An instance of type `T` is created by allocating a temporary local variable.</span></span> <span data-ttu-id="a1735-1245">由于需要*struct_type*的实例构造函数来明确地为要创建的实例的每个字段赋值，因此不需要初始化临时变量。</span><span class="sxs-lookup"><span data-stu-id="a1735-1245">Since an instance constructor of a *struct_type* is required to definitely assign a value to each field of the instance being created, no initialization of the temporary variable is necessary.</span></span>
    * <span data-ttu-id="a1735-1246">实例构造函数根据函数成员调用的规则进行调用（对[动态重载决策进行编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1246">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="a1735-1247">对新分配的实例的引用会自动传递到实例构造函数，并且可以从该构造函数内作为 `this`访问该实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-1247">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>

#### <a name="object-initializers"></a><span data-ttu-id="a1735-1248">对象初始值设定项</span><span class="sxs-lookup"><span data-stu-id="a1735-1248">Object initializers</span></span>

<span data-ttu-id="a1735-1249">***对象初始值设定项***为零个或多个字段、属性或对象的索引元素指定值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1249">An ***object initializer*** specifies values for zero or more fields, properties or indexed elements of an object.</span></span>

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

<span data-ttu-id="a1735-1250">对象初始值设定项由成员初始值设定项的序列组成，括在 `{` 和 `}` 标记中，并用逗号分隔。</span><span class="sxs-lookup"><span data-stu-id="a1735-1250">An object initializer consists of a sequence of member initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="a1735-1251">每个*member_initializer*为初始化指定一个目标。</span><span class="sxs-lookup"><span data-stu-id="a1735-1251">Each *member_initializer* designates a target for the initialization.</span></span> <span data-ttu-id="a1735-1252">*标识符*必须命名要初始化的对象的可访问字段或属性，而用方括号括起来的*argument_list*必须在要初始化的对象上指定可访问索引器的参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-1252">An *identifier* must name an accessible field or property of the object being initialized, whereas an *argument_list* enclosed in square brackets must specify arguments for an accessible indexer on the object being initialized.</span></span> <span data-ttu-id="a1735-1253">对象初始值设定项为同一个字段或属性包含多个成员初始值设定项是错误的。</span><span class="sxs-lookup"><span data-stu-id="a1735-1253">It is an error for an object initializer to include more than one member initializer for the same field or property.</span></span>

<span data-ttu-id="a1735-1254">每个*initializer_target*后跟一个等号和一个表达式、一个对象初始值设定项或集合初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="a1735-1254">Each *initializer_target* is followed by an equals sign and either an expression, an object initializer or a collection initializer.</span></span> <span data-ttu-id="a1735-1255">对象初始值设定项中的表达式不能引用它要初始化的新创建的对象。</span><span class="sxs-lookup"><span data-stu-id="a1735-1255">It is not possible for expressions within the object initializer to refer to the newly created object it is initializing.</span></span>

<span data-ttu-id="a1735-1256">在按与目标的赋值（[简单赋值](expressions.md#simple-assignment)）相同的方式处理等号后，指定表达式的成员初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="a1735-1256">A member initializer that specifies an expression after the equals sign is processed in the same way as an assignment ([Simple assignment](expressions.md#simple-assignment)) to the target.</span></span>

<span data-ttu-id="a1735-1257">在等号后指定对象初始值设定项的成员初始值设定项是***嵌套的对象初始值设定***项，即嵌入对象的初始化。</span><span class="sxs-lookup"><span data-stu-id="a1735-1257">A member initializer that specifies an object initializer after the equals sign is a ***nested object initializer***, i.e. an initialization of an embedded object.</span></span> <span data-ttu-id="a1735-1258">嵌套的对象初始值设定项中的赋值被视为对该字段或属性的成员的赋值，而不是为字段或属性赋值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1258">Instead of assigning a new value to the field or property, the assignments in the nested object initializer are treated as assignments to members of the field or property.</span></span> <span data-ttu-id="a1735-1259">嵌套的对象初始值设定项不能应用于值类型的属性，也不能应用于具有值类型的只读字段。</span><span class="sxs-lookup"><span data-stu-id="a1735-1259">Nested object initializers cannot be applied to properties with a value type, or to read-only fields with a value type.</span></span>

<span data-ttu-id="a1735-1260">在等号后指定集合初始值设定项的成员初始值设定项是嵌入集合的初始化。</span><span class="sxs-lookup"><span data-stu-id="a1735-1260">A member initializer that specifies a collection initializer after the equals sign is an initialization of an embedded collection.</span></span> <span data-ttu-id="a1735-1261">将初始值设定项中给定的元素添加到目标所引用的集合，而不是将新集合分配给目标字段、属性或索引器。</span><span class="sxs-lookup"><span data-stu-id="a1735-1261">Instead of assigning a new collection to the target field, property or indexer, the elements given in the initializer are added to the collection referenced by the target.</span></span> <span data-ttu-id="a1735-1262">目标必须为满足[集合初始值设定项](expressions.md#collection-initializers)中指定的要求的集合类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1262">The target must be of a collection type that satisfies the requirements specified in [Collection initializers](expressions.md#collection-initializers).</span></span>

<span data-ttu-id="a1735-1263">索引初始值设定项的参数将始终只计算一次。</span><span class="sxs-lookup"><span data-stu-id="a1735-1263">The arguments to an index initializer will always be evaluated exactly once.</span></span> <span data-ttu-id="a1735-1264">因此，即使参数不会使用（例如，由于空的嵌套初始值设定项），也会对其副作用进行评估。</span><span class="sxs-lookup"><span data-stu-id="a1735-1264">Thus, even if the arguments end up never getting used (e.g. because of an empty nested initializer), they will be evaluated for their side effects.</span></span>

<span data-ttu-id="a1735-1265">下面的类表示一个具有两个坐标的点：</span><span class="sxs-lookup"><span data-stu-id="a1735-1265">The following class represents a point with two coordinates:</span></span>
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

<span data-ttu-id="a1735-1266">可以创建和初始化 `Point` 的实例，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a1735-1266">An instance of `Point` can be created and initialized as follows:</span></span>
```csharp
Point a = new Point { X = 0, Y = 1 };
```
<span data-ttu-id="a1735-1267">这与</span><span class="sxs-lookup"><span data-stu-id="a1735-1267">which has the same effect as</span></span>
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
<span data-ttu-id="a1735-1268">其中 `__a` 是其他不可见且无法访问的临时变量。</span><span class="sxs-lookup"><span data-stu-id="a1735-1268">where `__a` is an otherwise invisible and inaccessible temporary variable.</span></span> <span data-ttu-id="a1735-1269">下面的类表示从两个点创建的矩形：</span><span class="sxs-lookup"><span data-stu-id="a1735-1269">The following class represents a rectangle created from two points:</span></span>
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

<span data-ttu-id="a1735-1270">可以创建和初始化 `Rectangle` 的实例，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a1735-1270">An instance of `Rectangle` can be created and initialized as follows:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
<span data-ttu-id="a1735-1271">这与</span><span class="sxs-lookup"><span data-stu-id="a1735-1271">which has the same effect as</span></span>
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
<span data-ttu-id="a1735-1272">其中 `__r`、`__p1` 和 `__p2` 是临时变量，这些变量不可见且不可访问。</span><span class="sxs-lookup"><span data-stu-id="a1735-1272">where `__r`, `__p1` and `__p2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="a1735-1273">如果 `Rectangle`的构造函数分配两个嵌入的 `Point` 实例</span><span class="sxs-lookup"><span data-stu-id="a1735-1273">If `Rectangle`'s constructor allocates the two embedded `Point` instances</span></span>
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
<span data-ttu-id="a1735-1274">以下构造可用于初始化嵌入的 `Point` 实例，而不是分配新实例：</span><span class="sxs-lookup"><span data-stu-id="a1735-1274">the following construct can be used to initialize the embedded `Point` instances instead of assigning new instances:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
<span data-ttu-id="a1735-1275">这与</span><span class="sxs-lookup"><span data-stu-id="a1735-1275">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

<span data-ttu-id="a1735-1276">给定 C 的适当定义，以下示例：</span><span class="sxs-lookup"><span data-stu-id="a1735-1276">Given an appropriate definition of C, the following example:</span></span>
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
<span data-ttu-id="a1735-1277">等效于这一系列的赋值：</span><span class="sxs-lookup"><span data-stu-id="a1735-1277">is equivalent to this series of assignments:</span></span>
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
<span data-ttu-id="a1735-1278">其中 `__c`（等等）生成的变量不可见且无法访问源代码。</span><span class="sxs-lookup"><span data-stu-id="a1735-1278">where `__c`, etc., are generated variables that are invisible and inaccessible to the source code.</span></span> <span data-ttu-id="a1735-1279">请注意，`[0,0]` 的参数只计算一次，即使从未使用过，也会对 `[1,2]` 的参数进行一次计算。</span><span class="sxs-lookup"><span data-stu-id="a1735-1279">Note that the arguments for `[0,0]` are evaluated only once, and the arguments for `[1,2]` are evaluated once even though they are never used.</span></span>

#### <a name="collection-initializers"></a><span data-ttu-id="a1735-1280">集合初始值设定项</span><span class="sxs-lookup"><span data-stu-id="a1735-1280">Collection initializers</span></span>

<span data-ttu-id="a1735-1281">集合初始值设定项指定集合的元素。</span><span class="sxs-lookup"><span data-stu-id="a1735-1281">A collection initializer specifies the elements of a collection.</span></span>

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

<span data-ttu-id="a1735-1282">集合初始值设定项由元素初始值设定项序列组成，括在 `{` 和 `}` 标记之间，并用逗号分隔。</span><span class="sxs-lookup"><span data-stu-id="a1735-1282">A collection initializer consists of a sequence of element initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="a1735-1283">每个元素初始值设定项指定一个元素，该元素将被添加到要初始化的集合对象，并由 `{` 和 `}` 令牌中包含的表达式列表以及用逗号分隔。</span><span class="sxs-lookup"><span data-stu-id="a1735-1283">Each element initializer specifies an element to be added to the collection object being initialized, and consists of a list of expressions enclosed by `{` and `}` tokens and separated by commas.</span></span>  <span data-ttu-id="a1735-1284">单表达式元素初始值设定项可以不带大括号编写，但是不能是赋值表达式，以避免成员初始值设定项出现歧义。</span><span class="sxs-lookup"><span data-stu-id="a1735-1284">A single-expression element initializer can be written without braces, but cannot then be an assignment expression, to avoid ambiguity with member initializers.</span></span> <span data-ttu-id="a1735-1285">*Non_assignment_expression*生产是在[expression](expressions.md#expression)中定义的。</span><span class="sxs-lookup"><span data-stu-id="a1735-1285">The *non_assignment_expression* production is defined in [Expression](expressions.md#expression).</span></span>

<span data-ttu-id="a1735-1286">下面是包含集合初始值设定项的对象创建表达式的示例：</span><span class="sxs-lookup"><span data-stu-id="a1735-1286">The following is an example of an object creation expression that includes a collection initializer:</span></span>
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

<span data-ttu-id="a1735-1287">集合初始值设定项应用于的集合对象必须是实现 `System.Collections.IEnumerable` 或发生编译时错误的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1287">The collection object to which a collection initializer is applied must be of a type that implements `System.Collections.IEnumerable` or a compile-time error occurs.</span></span> <span data-ttu-id="a1735-1288">对于按顺序排列的每个指定元素，集合初始值设定项使用元素初始值设定项的表达式列表作为参数列表调用目标对象上的 `Add` 方法，并对每个调用应用常规成员查找和重载解析。</span><span class="sxs-lookup"><span data-stu-id="a1735-1288">For each specified element in order, the collection initializer invokes an `Add` method on the target object with the expression list of the element initializer as argument list, applying normal member lookup and overload resolution for each invocation.</span></span> <span data-ttu-id="a1735-1289">因此，集合对象必须具有适用于每个元素初始值设定项 `Add` 名称的相应实例或扩展方法。</span><span class="sxs-lookup"><span data-stu-id="a1735-1289">Thus, the collection object must have an applicable instance or extension method with the name `Add` for each element initializer.</span></span>

<span data-ttu-id="a1735-1290">下面的类表示一个联系人，其中包含名称和电话号码的列表：</span><span class="sxs-lookup"><span data-stu-id="a1735-1290">The following class represents a contact with a name and a list of phone numbers:</span></span>
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

<span data-ttu-id="a1735-1291">可以按如下所示创建和初始化 `List<Contact>`：</span><span class="sxs-lookup"><span data-stu-id="a1735-1291">A `List<Contact>` can be created and initialized as follows:</span></span>
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
<span data-ttu-id="a1735-1292">这与</span><span class="sxs-lookup"><span data-stu-id="a1735-1292">which has the same effect as</span></span>
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
<span data-ttu-id="a1735-1293">其中 `__clist`、`__c1` 和 `__c2` 是临时变量，这些变量不可见且不可访问。</span><span class="sxs-lookup"><span data-stu-id="a1735-1293">where `__clist`, `__c1` and `__c2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="a1735-1294">数组创建表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-1294">Array creation expressions</span></span>

<span data-ttu-id="a1735-1295">*Array_creation_expression*用于创建*array_type*的新实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-1295">An *array_creation_expression* is used to create a new instance of an *array_type*.</span></span>

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

<span data-ttu-id="a1735-1296">第一种形式的数组创建表达式将分配从表达式列表中删除每个表达式所得到的类型的数组实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-1296">An array creation expression of the first form allocates an array instance of the type that results from deleting each of the individual expressions from the expression list.</span></span> <span data-ttu-id="a1735-1297">例如，数组创建表达式 `new int[10,20]` 生成 `int[,]`类型的数组实例，而数组创建表达式 `new int[10][,]` 生成类型 `int[][,]`的数组。</span><span class="sxs-lookup"><span data-stu-id="a1735-1297">For example, the array creation expression `new int[10,20]` produces an array instance of type `int[,]`, and the array creation expression `new int[10][,]` produces an array of type `int[][,]`.</span></span> <span data-ttu-id="a1735-1298">表达式列表中的每个表达式的类型必须为 `int`、`uint`、`long`或 `ulong`，或可隐式转换为这些类型中的一个或多个。</span><span class="sxs-lookup"><span data-stu-id="a1735-1298">Each expression in the expression list must be of type `int`, `uint`, `long`, or `ulong`, or implicitly convertible to one or more of these types.</span></span> <span data-ttu-id="a1735-1299">每个表达式的值决定了新分配的数组实例中相应维度的长度。</span><span class="sxs-lookup"><span data-stu-id="a1735-1299">The value of each expression determines the length of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="a1735-1300">由于数组维度的长度必须为非负，因此在表达式列表中有一个具有负值的*constant_expression*编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1300">Since the length of an array dimension must be nonnegative, it is a compile-time error to have a *constant_expression* with a negative value in the expression list.</span></span>

<span data-ttu-id="a1735-1301">除非在不安全的上下文中（[不安全](unsafe-code.md#unsafe-contexts)上下文），否则不指定数组的布局。</span><span class="sxs-lookup"><span data-stu-id="a1735-1301">Except in an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)), the layout of arrays is unspecified.</span></span>

<span data-ttu-id="a1735-1302">如果第一种形式的数组创建表达式包含数组初始值设定项，则表达式列表中的每个表达式都必须是常量，并且表达式列表指定的秩和维度长度必须与数组初始值设定项的匹配项和维度长度匹配。</span><span class="sxs-lookup"><span data-stu-id="a1735-1302">If an array creation expression of the first form includes an array initializer, each expression in the expression list must be a constant and the rank and dimension lengths specified by the expression list must match those of the array initializer.</span></span>

<span data-ttu-id="a1735-1303">在第二个或第三个窗体的数组创建表达式中，指定数组类型或秩说明符的秩必须与数组初始值设定项的秩匹配。</span><span class="sxs-lookup"><span data-stu-id="a1735-1303">In an array creation expression of the second or third form, the rank of the specified array type or rank specifier must match that of the array initializer.</span></span> <span data-ttu-id="a1735-1304">各个维度长度是从数组初始值设定项的每个相应嵌套级别中的元素数推断出来的。</span><span class="sxs-lookup"><span data-stu-id="a1735-1304">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the array initializer.</span></span> <span data-ttu-id="a1735-1305">因此，表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-1305">Thus, the expression</span></span>
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
<span data-ttu-id="a1735-1306">完全对应于</span><span class="sxs-lookup"><span data-stu-id="a1735-1306">exactly corresponds to</span></span>
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

<span data-ttu-id="a1735-1307">第三种形式的数组创建表达式称为***隐式类型化数组创建表达式***。</span><span class="sxs-lookup"><span data-stu-id="a1735-1307">An array creation expression of the third form is referred to as an ***implicitly typed array creation expression***.</span></span> <span data-ttu-id="a1735-1308">它类似于第二个窗体，只不过数组的元素类型不是显式给定的，而是确定为数组初始值设定项中的一组表达式的最佳通用类型（[查找一组表达式的最佳通用类型](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1308">It is similar to the second form, except that the element type of the array is not explicitly given, but determined as the best common type ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) of the set of expressions in the array initializer.</span></span> <span data-ttu-id="a1735-1309">对于多维数组（即，其中*rank_specifier*至少包含一个逗号），此集包含在嵌套*array_initializer*中找到的所有*表达式*。</span><span class="sxs-lookup"><span data-stu-id="a1735-1309">For a multidimensional array, i.e., one where the *rank_specifier* contains at least one comma, this set comprises all *expression*s found in nested *array_initializer*s.</span></span>

<span data-ttu-id="a1735-1310">数组[初始值设定项中进一步](arrays.md#array-initializers)介绍了数组初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="a1735-1310">Array initializers are described further in [Array initializers](arrays.md#array-initializers).</span></span>

<span data-ttu-id="a1735-1311">计算数组创建表达式的结果是一个值，即对新分配的数组实例的引用。</span><span class="sxs-lookup"><span data-stu-id="a1735-1311">The result of evaluating an array creation expression is classified as a value, namely a reference to the newly allocated array instance.</span></span> <span data-ttu-id="a1735-1312">数组创建表达式的运行时处理包括以下步骤：</span><span class="sxs-lookup"><span data-stu-id="a1735-1312">The run-time processing of an array creation expression consists of the following steps:</span></span>

*  <span data-ttu-id="a1735-1313">将按从左到右的顺序计算*expression_list*的维度长度表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-1313">The dimension length expressions of the *expression_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="a1735-1314">对每个表达式进行以下计算后，将执行隐式转换（[隐式](conversions.md#implicit-conversions)转换）为以下类型之一： `int`、`uint`、`long``ulong`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1314">Following evaluation of each expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="a1735-1315">在此列表中选择了隐式转换的第一种类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1315">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="a1735-1316">如果表达式的计算或后续的隐式转换导致异常，则不会计算进一步的表达式，也不会执行任何其他步骤。</span><span class="sxs-lookup"><span data-stu-id="a1735-1316">If evaluation of an expression or the subsequent implicit conversion causes an exception, then no further expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="a1735-1317">按如下所示验证维度长度的计算值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1317">The computed values for the dimension lengths are validated as follows.</span></span> <span data-ttu-id="a1735-1318">如果一个或多个值小于零，则会引发 `System.OverflowException`，而不执行进一步的步骤。</span><span class="sxs-lookup"><span data-stu-id="a1735-1318">If one or more of the values are less than zero, a `System.OverflowException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="a1735-1319">分配具有给定维度长度的数组实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-1319">An array instance with the given dimension lengths is allocated.</span></span> <span data-ttu-id="a1735-1320">如果内存不足，无法分配新的实例，则会引发 `System.OutOfMemoryException`，而不执行进一步的步骤。</span><span class="sxs-lookup"><span data-stu-id="a1735-1320">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="a1735-1321">新数组实例的所有元素都初始化为其默认值（[默认值](variables.md#default-values)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1321">All elements of the new array instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
*  <span data-ttu-id="a1735-1322">如果数组创建表达式包含数组初始值设定项，则计算数组初始值设定项中的每个表达式并将其分配给其相应的数组元素。</span><span class="sxs-lookup"><span data-stu-id="a1735-1322">If the array creation expression contains an array initializer, then each expression in the array initializer is evaluated and assigned to its corresponding array element.</span></span> <span data-ttu-id="a1735-1323">计算和分配按表达式在数组初始值设定项中写入的顺序执行，换言之，元素按递增的索引顺序进行初始化，最右边的维度首先增长。</span><span class="sxs-lookup"><span data-stu-id="a1735-1323">The evaluations and assignments are performed in the order the expressions are written in the array initializer—in other words, elements are initialized in increasing index order, with the rightmost dimension increasing first.</span></span> <span data-ttu-id="a1735-1324">如果给定表达式的计算或对相应数组元素的后续赋值导致异常，则不会初始化其他元素（因此剩余的元素将具有其默认值）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1324">If evaluation of a given expression or the subsequent assignment to the corresponding array element causes an exception, then no further elements are initialized (and the remaining elements will thus have their default values).</span></span>

<span data-ttu-id="a1735-1325">数组创建表达式允许实例化包含数组类型的元素的数组，但此类数组的元素必须手动初始化。</span><span class="sxs-lookup"><span data-stu-id="a1735-1325">An array creation expression permits instantiation of an array with elements of an array type, but the elements of such an array must be manually initialized.</span></span> <span data-ttu-id="a1735-1326">例如，语句</span><span class="sxs-lookup"><span data-stu-id="a1735-1326">For example, the statement</span></span>
```csharp
int[][] a = new int[100][];
```
<span data-ttu-id="a1735-1327">创建具有100类型 `int[]`元素的一维数组。</span><span class="sxs-lookup"><span data-stu-id="a1735-1327">creates a single-dimensional array with 100 elements of type `int[]`.</span></span> <span data-ttu-id="a1735-1328">每个元素的初始值都是 `null`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1328">The initial value of each element is `null`.</span></span> <span data-ttu-id="a1735-1329">同一个数组创建表达式不可能同时实例化子数组和语句</span><span class="sxs-lookup"><span data-stu-id="a1735-1329">It is not possible for the same array creation expression to also instantiate the sub-arrays, and the statement</span></span>
```csharp
int[][] a = new int[100][5];        // Error
```
<span data-ttu-id="a1735-1330">导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1330">results in a compile-time error.</span></span> <span data-ttu-id="a1735-1331">子数组的实例化必须改为手动执行，如下所示</span><span class="sxs-lookup"><span data-stu-id="a1735-1331">Instantiation of the sub-arrays must instead be performed manually, as in</span></span>
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

<span data-ttu-id="a1735-1332">当数组的数组具有 "矩形" 形状时，即当子数组的长度完全相同时，使用多维数组更为有效。</span><span class="sxs-lookup"><span data-stu-id="a1735-1332">When an array of arrays has a "rectangular" shape, that is when the sub-arrays are all of the same length, it is more efficient to use a multi-dimensional array.</span></span> <span data-ttu-id="a1735-1333">在上面的示例中，数组数组的实例化将创建101对象，即一个外部数组和100子数组。</span><span class="sxs-lookup"><span data-stu-id="a1735-1333">In the example above, instantiation of the array of arrays creates 101 objects—one outer array and 100 sub-arrays.</span></span> <span data-ttu-id="a1735-1334">相反，</span><span class="sxs-lookup"><span data-stu-id="a1735-1334">In contrast,</span></span>
```csharp
int[,] = new int[100, 5];
```
<span data-ttu-id="a1735-1335">仅创建单个对象和二维数组，并在单个语句中完成分配。</span><span class="sxs-lookup"><span data-stu-id="a1735-1335">creates only a single object, a two-dimensional array, and accomplishes the allocation in a single statement.</span></span>

<span data-ttu-id="a1735-1336">下面是隐式类型的数组创建表达式的示例：</span><span class="sxs-lookup"><span data-stu-id="a1735-1336">The following are examples of implicitly typed array creation expressions:</span></span>
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

<span data-ttu-id="a1735-1337">最后一个表达式会导致编译时错误，因为 `int` 和 `string` 都不能隐式转换为另一种类型，因此没有最常见的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1337">The last expression causes a compile-time error because neither `int` nor `string` is implicitly convertible to the other, and so there is no best common type.</span></span> <span data-ttu-id="a1735-1338">在这种情况下，必须使用显式类型化的数组创建表达式，例如指定要 `object[]`的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1338">An explicitly typed array creation expression must be used in this case, for example specifying the type to be `object[]`.</span></span> <span data-ttu-id="a1735-1339">或者，可以将其中一个元素强制转换为公共基类型，后者随后将成为推断元素类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1339">Alternatively, one of the elements can be cast to a common base type, which would then become the inferred element type.</span></span>

<span data-ttu-id="a1735-1340">隐式类型的数组创建表达式可与匿名对象初始值设定项（[匿名对象创建表达式](expressions.md#anonymous-object-creation-expressions)）结合，以创建匿名类型的数据结构。</span><span class="sxs-lookup"><span data-stu-id="a1735-1340">Implicitly typed array creation expressions can be combined with anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) to create anonymously typed data structures.</span></span> <span data-ttu-id="a1735-1341">例如：</span><span class="sxs-lookup"><span data-stu-id="a1735-1341">For example:</span></span>
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

#### <a name="delegate-creation-expressions"></a><span data-ttu-id="a1735-1342">委托创建表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-1342">Delegate creation expressions</span></span>

<span data-ttu-id="a1735-1343">*Delegate_creation_expression*用于创建*delegate_type*的新实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-1343">A *delegate_creation_expression* is used to create a new instance of a *delegate_type*.</span></span>

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

<span data-ttu-id="a1735-1344">委托创建表达式的参数必须是方法组、匿名函数或编译时类型的值 `dynamic` 或*delegate_type*。</span><span class="sxs-lookup"><span data-stu-id="a1735-1344">The argument of a delegate creation expression must be a method group, an anonymous function or a value of either the compile time type `dynamic` or a *delegate_type*.</span></span> <span data-ttu-id="a1735-1345">如果参数为方法组，则它将标识方法，并为实例方法标识要为其创建委托的对象。</span><span class="sxs-lookup"><span data-stu-id="a1735-1345">If the argument is a method group, it identifies the method and, for an instance method, the object for which to create a delegate.</span></span> <span data-ttu-id="a1735-1346">如果参数是匿名函数，它将直接定义委托目标的参数和方法主体。</span><span class="sxs-lookup"><span data-stu-id="a1735-1346">If the argument is an anonymous function it directly defines the parameters and method body of the delegate target.</span></span> <span data-ttu-id="a1735-1347">如果参数是一个值，则它标识要创建副本的委托实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-1347">If the argument is a value it identifies a delegate instance of which to create a copy.</span></span>

<span data-ttu-id="a1735-1348">如果*表达式*具有编译时类型 `dynamic`，则*delegate_creation_expression*是动态绑定的（[动态绑定](expressions.md#dynamic-binding)），下面的规则将使用*表达式*的运行时类型在运行时应用。</span><span class="sxs-lookup"><span data-stu-id="a1735-1348">If the *expression* has the compile-time type `dynamic`, the *delegate_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), and the rules below are applied at run-time using the run-time type of the *expression*.</span></span> <span data-ttu-id="a1735-1349">否则，规则将在编译时应用。</span><span class="sxs-lookup"><span data-stu-id="a1735-1349">Otherwise the rules are applied at compile-time.</span></span>

<span data-ttu-id="a1735-1350">格式为 `new D(E)`的*delegate_creation_expression*的绑定时处理，其中 `D` 是一个*delegate_type* ，`E` 是一个*表达式*，包含以下步骤：</span><span class="sxs-lookup"><span data-stu-id="a1735-1350">The binding-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*  <span data-ttu-id="a1735-1351">如果 `E` 是方法组，则将使用与从 `E` 到 `D`的方法[组转换相同](conversions.md#method-group-conversions)的方式来处理委托创建表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-1351">If `E` is a method group, the delegate creation expression is processed in the same way as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="a1735-1352">如果 `E` 是匿名函数，则以与从 `E` 到 `D`的匿名[函数转换相同](conversions.md#anonymous-function-conversions)的方式处理委托创建表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-1352">If `E` is an anonymous function, the delegate creation expression is processed in the same way as an anonymous function conversion ([Anonymous function conversions](conversions.md#anonymous-function-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="a1735-1353">如果 `E` 是值，则 `E` 必须与 `D`兼容（[委托声明](delegates.md#delegate-declarations)），并且结果是对类型 `D` 的新创建委托的引用，该委托引用与 `E`相同的调用列表。</span><span class="sxs-lookup"><span data-stu-id="a1735-1353">If `E` is a value, `E` must be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`, and the result is a reference to a newly created delegate of type `D` that refers to the same invocation list as `E`.</span></span> <span data-ttu-id="a1735-1354">如果 `E` 与 `D`不兼容，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1354">If `E` is not compatible with `D`, a compile-time error occurs.</span></span>

<span data-ttu-id="a1735-1355">格式为 `new D(E)`的*delegate_creation_expression*的运行时处理，其中 `D` 是一个*delegate_type* ，`E` 是一个*表达式*，它包含以下步骤：</span><span class="sxs-lookup"><span data-stu-id="a1735-1355">The run-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*   <span data-ttu-id="a1735-1356">如果 `E` 是方法组，则将委托创建表达式作为方法组转换（[方法组](conversions.md#method-group-conversions)转换）作为 `E` `D`进行计算。</span><span class="sxs-lookup"><span data-stu-id="a1735-1356">If `E` is a method group, the delegate creation expression is evaluated as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*   <span data-ttu-id="a1735-1357">如果 `E` 是匿名函数，则委托创建将作为从 `E` 到 `D` （[匿名函数转换](conversions.md#anonymous-function-conversions)）的匿名函数转换进行计算。</span><span class="sxs-lookup"><span data-stu-id="a1735-1357">If `E` is an anonymous function, the delegate creation is evaluated as an anonymous function conversion from `E` to `D` ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>
*   <span data-ttu-id="a1735-1358">如果 `E` 是*delegate_type*的值：</span><span class="sxs-lookup"><span data-stu-id="a1735-1358">If `E` is a value of a *delegate_type*:</span></span>
    * <span data-ttu-id="a1735-1359">计算 `E`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1359">`E` is evaluated.</span></span> <span data-ttu-id="a1735-1360">如果此计算导致异常，则不执行其他步骤。</span><span class="sxs-lookup"><span data-stu-id="a1735-1360">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="a1735-1361">如果 `null``E` 的值，则将引发 `System.NullReferenceException`，并且不执行任何其他步骤。</span><span class="sxs-lookup"><span data-stu-id="a1735-1361">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="a1735-1362">分配 `D` 委托类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-1362">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="a1735-1363">如果内存不足，无法分配新的实例，则会引发 `System.OutOfMemoryException`，而不执行进一步的步骤。</span><span class="sxs-lookup"><span data-stu-id="a1735-1363">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="a1735-1364">使用与 `E`给定的委托实例相同的调用列表来初始化新的委托实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-1364">The new delegate instance is initialized with the same invocation list as the delegate instance given by `E`.</span></span>

<span data-ttu-id="a1735-1365">委托的调用列表是在对委托进行实例化时确定的，然后在该委托的整个生存期内保持不变。</span><span class="sxs-lookup"><span data-stu-id="a1735-1365">The invocation list of a delegate is determined when the delegate is instantiated and then remains constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="a1735-1366">换句话说，创建委托后，不能更改其目标可调用的实体。</span><span class="sxs-lookup"><span data-stu-id="a1735-1366">In other words, it is not possible to change the target callable entities of a delegate once it has been created.</span></span> <span data-ttu-id="a1735-1367">合并两个委托或从另一个委托（[委托声明](delegates.md#delegate-declarations)）中删除一个委托时，将生成一个新委托;现有委托的内容未发生更改。</span><span class="sxs-lookup"><span data-stu-id="a1735-1367">When two delegates are combined or one is removed from another ([Delegate declarations](delegates.md#delegate-declarations)), a new delegate results; no existing delegate has its contents changed.</span></span>

<span data-ttu-id="a1735-1368">不能创建引用属性、索引器、用户定义的运算符、实例构造函数、析构函数或静态构造函数的委托。</span><span class="sxs-lookup"><span data-stu-id="a1735-1368">It is not possible to create a delegate that refers to a property, indexer, user-defined operator, instance constructor, destructor, or static constructor.</span></span>

<span data-ttu-id="a1735-1369">如上所述，从方法组创建委托时，委托的形参列表和返回类型确定要选择的重载方法。</span><span class="sxs-lookup"><span data-stu-id="a1735-1369">As described above, when a delegate is created from a method group, the formal parameter list and return type of the delegate determine which of the overloaded methods to select.</span></span> <span data-ttu-id="a1735-1370">示例中</span><span class="sxs-lookup"><span data-stu-id="a1735-1370">In the example</span></span>
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
<span data-ttu-id="a1735-1371">使用引用第二个 `Square` 方法的委托初始化 `A.f` 字段，因为该方法与 `DoubleFunc`的形参列表和返回类型完全匹配。</span><span class="sxs-lookup"><span data-stu-id="a1735-1371">the `A.f` field is initialized with a delegate that refers to the second `Square` method because that method exactly matches the formal parameter list and return type of `DoubleFunc`.</span></span> <span data-ttu-id="a1735-1372">如果第二个 `Square` 方法不存在，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1372">Had the second `Square` method not been present, a compile-time error would have occurred.</span></span>

#### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="a1735-1373">匿名对象创建表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-1373">Anonymous object creation expressions</span></span>

<span data-ttu-id="a1735-1374">*Anonymous_object_creation_expression*用于创建匿名类型的对象。</span><span class="sxs-lookup"><span data-stu-id="a1735-1374">An *anonymous_object_creation_expression* is used to create an object of an anonymous type.</span></span>

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

<span data-ttu-id="a1735-1375">匿名对象初始值设定项声明匿名类型并返回该类型的实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-1375">An anonymous object initializer declares an anonymous type and returns an instance of that type.</span></span> <span data-ttu-id="a1735-1376">匿名类型是直接从 `object`继承的无类类类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1376">An anonymous type is a nameless class type that inherits directly from `object`.</span></span> <span data-ttu-id="a1735-1377">匿名类型的成员是从用于创建该类型实例的匿名对象初始值设定项中推断出的只读属性的序列。</span><span class="sxs-lookup"><span data-stu-id="a1735-1377">The members of an anonymous type are a sequence of read-only properties inferred from the anonymous object initializer used to create an instance of the type.</span></span> <span data-ttu-id="a1735-1378">具体而言，是窗体的匿名对象初始值设定项</span><span class="sxs-lookup"><span data-stu-id="a1735-1378">Specifically, an anonymous object initializer of the form</span></span>
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
<span data-ttu-id="a1735-1379">声明窗体的匿名类型</span><span class="sxs-lookup"><span data-stu-id="a1735-1379">declares an anonymous type of the form</span></span>
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
<span data-ttu-id="a1735-1380">其中每个 `Tx` 都是对应的表达式 `ex`的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1380">where each `Tx` is the type of the corresponding expression `ex`.</span></span> <span data-ttu-id="a1735-1381">*Member_declarator*中使用的表达式的类型必须为。</span><span class="sxs-lookup"><span data-stu-id="a1735-1381">The expression used in a *member_declarator* must have a type.</span></span> <span data-ttu-id="a1735-1382">因此， *member_declarator*中的表达式为 null 或匿名函数是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1382">Thus, it is a compile-time error for an expression in a *member_declarator* to be null or an anonymous function.</span></span> <span data-ttu-id="a1735-1383">这也是表达式具有不安全类型的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1383">It is also a compile-time error for the expression to have an unsafe type.</span></span>

<span data-ttu-id="a1735-1384">匿名类型及其 `Equals` 方法的参数的名称由编译器自动生成，不能在程序文本中引用。</span><span class="sxs-lookup"><span data-stu-id="a1735-1384">The names of an anonymous type and of the parameter to its `Equals` method are automatically generated by the compiler and cannot be referenced in program text.</span></span>

<span data-ttu-id="a1735-1385">在同一程序中，两个用相同顺序指定相同名称和编译时类型的属性序列的匿名对象初始值设定项将生成相同的匿名类型的实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-1385">Within the same program, two anonymous object initializers that specify a sequence of properties of the same names and compile-time types in the same order will produce instances of the same anonymous type.</span></span>

<span data-ttu-id="a1735-1386">示例中</span><span class="sxs-lookup"><span data-stu-id="a1735-1386">In the example</span></span>
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
<span data-ttu-id="a1735-1387">允许在最后一行上进行赋值，因为 `p1` 和 `p2` 是相同的匿名类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1387">the assignment on the last line is permitted because `p1` and `p2` are of the same anonymous type.</span></span>

<span data-ttu-id="a1735-1388">匿名类型上的 `Equals` 和 `GetHashcode` 方法会重写从 `object`继承的方法，并在属性的 `Equals` 和 `GetHashcode` 中定义，因此，当且仅当所有属性都相等时，相同匿名类型的两个实例才相等。</span><span class="sxs-lookup"><span data-stu-id="a1735-1388">The `Equals` and `GetHashcode` methods on anonymous types override the methods inherited from `object`, and are defined in terms of the `Equals` and `GetHashcode` of the properties, so that two instances of the same anonymous type are equal if and only if all their properties are equal.</span></span>

<span data-ttu-id="a1735-1389">成员声明符可以缩写为简单名称（[类型推理](expressions.md#type-inference)）、成员访问（[动态重载决策的编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）、基访问（[基本访问](expressions.md#base-access)）或 null 条件成员访问（[null 条件表达式作为投影初始值设定项](expressions.md#null-conditional-expressions-as-projection-initializers)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1389">A member declarator can be abbreviated to a simple name ([Type inference](expressions.md#type-inference)), a member access ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), a base access ([Base access](expressions.md#base-access)) or a null-conditional member access ([Null-conditional expressions as projection initializers](expressions.md#null-conditional-expressions-as-projection-initializers)).</span></span> <span data-ttu-id="a1735-1390">这称为***投影初始值设定项***，是的声明和对具有相同名称的属性赋值的简写形式。</span><span class="sxs-lookup"><span data-stu-id="a1735-1390">This is called a ***projection initializer*** and is shorthand for a declaration of and assignment to a property with the same name.</span></span> <span data-ttu-id="a1735-1391">具体而言，是窗体的成员声明符</span><span class="sxs-lookup"><span data-stu-id="a1735-1391">Specifically, member declarators of the forms</span></span>
```csharp
identifier
expr.identifier
```
<span data-ttu-id="a1735-1392">与以下各项完全等效：</span><span class="sxs-lookup"><span data-stu-id="a1735-1392">are precisely equivalent to the following, respectively:</span></span>
```csharp
identifier = identifier
identifier = expr.identifier
```

<span data-ttu-id="a1735-1393">因此，在投影初始值设定项中，*标识符*既选择值，又选择要向其分配值的字段或属性。</span><span class="sxs-lookup"><span data-stu-id="a1735-1393">Thus, in a projection initializer the *identifier* selects both the value and the field or property to which the value is assigned.</span></span> <span data-ttu-id="a1735-1394">直观而言，投影初始值设定项不只是一个值，而是值的名称。</span><span class="sxs-lookup"><span data-stu-id="a1735-1394">Intuitively, a projection initializer projects not just a value, but also the name of the value.</span></span>

### <a name="the-typeof-operator"></a><span data-ttu-id="a1735-1395">Typeof 运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-1395">The typeof operator</span></span>

<span data-ttu-id="a1735-1396">`typeof` 运算符用于获取类型的 `System.Type` 对象。</span><span class="sxs-lookup"><span data-stu-id="a1735-1396">The `typeof` operator is used to obtain the `System.Type` object for a type.</span></span>

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

<span data-ttu-id="a1735-1397">第一种形式的*typeof_expression*由 `typeof` 关键字后跟带括号的*类型*组成。</span><span class="sxs-lookup"><span data-stu-id="a1735-1397">The first form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *type*.</span></span> <span data-ttu-id="a1735-1398">此窗体的表达式的结果是指定类型的 `System.Type` 对象。</span><span class="sxs-lookup"><span data-stu-id="a1735-1398">The result of an expression of this form is the `System.Type` object for the indicated type.</span></span> <span data-ttu-id="a1735-1399">对于任何给定类型，只有一个 `System.Type` 对象。</span><span class="sxs-lookup"><span data-stu-id="a1735-1399">There is only one `System.Type` object for any given type.</span></span> <span data-ttu-id="a1735-1400">这意味着对于类型 `T`，`typeof(T) == typeof(T)` 始终为 true。</span><span class="sxs-lookup"><span data-stu-id="a1735-1400">This means that for a type `T`, `typeof(T) == typeof(T)` is always true.</span></span> <span data-ttu-id="a1735-1401">无法 `dynamic`*类型*。</span><span class="sxs-lookup"><span data-stu-id="a1735-1401">The *type* cannot be `dynamic`.</span></span>

<span data-ttu-id="a1735-1402">第二种形式的*typeof_expression*包含一个 `typeof` 关键字，后跟一个带圆括号的*unbound_type_name*。</span><span class="sxs-lookup"><span data-stu-id="a1735-1402">The second form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *unbound_type_name*.</span></span> <span data-ttu-id="a1735-1403">*Unbound_type_name*与*type_name* （[命名空间和类型名称](basic-concepts.md#namespace-and-type-names)）非常相似，不同之处在于*unbound_type_name*包含*type_name*包含*type_argument_list*的*generic_dimension_specifier*。</span><span class="sxs-lookup"><span data-stu-id="a1735-1403">An *unbound_type_name* is very similar to a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) except that an *unbound_type_name* contains *generic_dimension_specifier*s where a *type_name* contains *type_argument_list*s.</span></span> <span data-ttu-id="a1735-1404">当*typeof_expression*的操作数为满足*unbound_type_name*和*type_name*语法的标记序列时，即当它既不包含*generic_dimension_specifier*也不包含*type_argument_list*时，将标记序列视为*type_name*。</span><span class="sxs-lookup"><span data-stu-id="a1735-1404">When the operand of a *typeof_expression* is a sequence of tokens that satisfies the grammars of both *unbound_type_name* and *type_name*, namely when it contains neither a *generic_dimension_specifier* nor a *type_argument_list*, the sequence of tokens is considered to be a *type_name*.</span></span> <span data-ttu-id="a1735-1405">确定*unbound_type_name*的含义，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a1735-1405">The meaning of an *unbound_type_name* is determined as follows:</span></span>

*  <span data-ttu-id="a1735-1406">将标记序列替换为*type_name* ，方法是将每个*generic_dimension_specifier*替换为具有相同的逗号数的*type_argument_list* ，并且关键字 `object` 每个*type_argument*。</span><span class="sxs-lookup"><span data-stu-id="a1735-1406">Convert the sequence of tokens to a *type_name* by replacing each *generic_dimension_specifier* with a *type_argument_list* having the same number of commas and the keyword `object` as each *type_argument*.</span></span>
*  <span data-ttu-id="a1735-1407">计算生成的*type_name*，同时忽略所有类型参数约束。</span><span class="sxs-lookup"><span data-stu-id="a1735-1407">Evaluate the resulting *type_name*, while ignoring all type parameter constraints.</span></span>
*  <span data-ttu-id="a1735-1408">*Unbound_type_name*解析为与生成的构造类型（[绑定的和未绑定](types.md#bound-and-unbound-types)的类型）关联的未绑定的泛型类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1408">The *unbound_type_name* resolves to the unbound generic type associated with the resulting constructed type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span>

<span data-ttu-id="a1735-1409">*Typeof_expression*的结果是生成的未绑定泛型类型的 `System.Type` 对象。</span><span class="sxs-lookup"><span data-stu-id="a1735-1409">The result of the *typeof_expression* is the `System.Type` object for the resulting unbound generic type.</span></span>

<span data-ttu-id="a1735-1410">第三种形式的*typeof_expression*包含 `typeof` 关键字，后跟带括号的 `void` 关键字。</span><span class="sxs-lookup"><span data-stu-id="a1735-1410">The third form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized `void` keyword.</span></span> <span data-ttu-id="a1735-1411">此形式的表达式的结果是表示缺少类型的 `System.Type` 对象。</span><span class="sxs-lookup"><span data-stu-id="a1735-1411">The result of an expression of this form is the `System.Type` object that represents the absence of a type.</span></span> <span data-ttu-id="a1735-1412">`typeof(void)` 返回的类型对象不同于为任何类型返回的类型对象。</span><span class="sxs-lookup"><span data-stu-id="a1735-1412">The type object returned by `typeof(void)` is distinct from the type object returned for any type.</span></span> <span data-ttu-id="a1735-1413">此特殊类型对象在允许反射到语言中的方法的类库中很有用，在这种情况下，这些方法希望有一种方法来表示任何方法（包括 void 方法）的返回类型以及 `System.Type`的实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-1413">This special type object is useful in class libraries that allow reflection onto methods in the language, where those methods wish to have a way to represent the return type of any method, including void methods, with an instance of `System.Type`.</span></span>

<span data-ttu-id="a1735-1414">可在类型参数上使用 `typeof` 运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-1414">The `typeof` operator can be used on a type parameter.</span></span> <span data-ttu-id="a1735-1415">结果是绑定到类型参数的运行时类型的 `System.Type` 对象。</span><span class="sxs-lookup"><span data-stu-id="a1735-1415">The result is the `System.Type` object for the run-time type that was bound to the type parameter.</span></span> <span data-ttu-id="a1735-1416">`typeof` 运算符还可用于构造类型或未绑定的泛型类型（[绑定类型和未绑定类型](types.md#bound-and-unbound-types)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1416">The `typeof` operator can also be used on a constructed type or an unbound generic type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span> <span data-ttu-id="a1735-1417">未绑定的泛型类型的 `System.Type` 对象与实例类型的 `System.Type` 对象不同。</span><span class="sxs-lookup"><span data-stu-id="a1735-1417">The `System.Type` object for an unbound generic type is not the same as the `System.Type` object of the instance type.</span></span> <span data-ttu-id="a1735-1418">实例类型在运行时始终是封闭式构造类型，因此它的 `System.Type` 对象取决于正在使用的运行时类型参数，而未绑定的泛型类型没有类型参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-1418">The instance type is always a closed constructed type at run-time so its `System.Type` object depends on the run-time type arguments in use, while the unbound generic type has no type arguments.</span></span>

<span data-ttu-id="a1735-1419">示例</span><span class="sxs-lookup"><span data-stu-id="a1735-1419">The example</span></span>
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
<span data-ttu-id="a1735-1420">生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="a1735-1420">produces the following output:</span></span>
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

<span data-ttu-id="a1735-1421">请注意，`int` 和 `System.Int32` 是相同的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1421">Note that `int` and `System.Int32` are the same type.</span></span>

<span data-ttu-id="a1735-1422">另请注意，`typeof(X<>)` 的结果不取决于类型参数，但 `typeof(X<T>)` 的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1422">Also note that the result of `typeof(X<>)` does not depend on the type argument but the result of `typeof(X<T>)` does.</span></span>

### <a name="the-checked-and-unchecked-operators"></a><span data-ttu-id="a1735-1423">checked 和 unchecked 运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-1423">The checked and unchecked operators</span></span>

<span data-ttu-id="a1735-1424">`checked` 和 `unchecked` 运算符用于控制整型算术运算和转换的***溢出检查上下文***。</span><span class="sxs-lookup"><span data-stu-id="a1735-1424">The `checked` and `unchecked` operators are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

<span data-ttu-id="a1735-1425">`checked` 运算符在已检查的上下文中计算包含的表达式，而 `unchecked` 运算符在未检查的上下文中计算包含的表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-1425">The `checked` operator evaluates the contained expression in a checked context, and the `unchecked` operator evaluates the contained expression in an unchecked context.</span></span> <span data-ttu-id="a1735-1426">*Checked_expression*或*unchecked_expression*完全对应于*parenthesized_expression* （[带括号的表达式](expressions.md#parenthesized-expressions)），只是在给定溢出检查上下文中计算包含的表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-1426">A *checked_expression* or *unchecked_expression* corresponds exactly to a *parenthesized_expression* ([Parenthesized expressions](expressions.md#parenthesized-expressions)), except that the contained expression is evaluated in the given overflow checking context.</span></span>

<span data-ttu-id="a1735-1427">还可以通过 `checked` 和 `unchecked` 语句（[checked 和 unchecked 语句](statements.md#the-checked-and-unchecked-statements)）控制溢出检查上下文。</span><span class="sxs-lookup"><span data-stu-id="a1735-1427">The overflow checking context can also be controlled through the `checked` and `unchecked` statements ([The checked and unchecked statements](statements.md#the-checked-and-unchecked-statements)).</span></span>

<span data-ttu-id="a1735-1428">以下操作受 `checked` 和 `unchecked` 运算符和语句所建立的溢出检查上下文的影响：</span><span class="sxs-lookup"><span data-stu-id="a1735-1428">The following operations are affected by the overflow checking context established by the `checked` and `unchecked` operators and statements:</span></span>

*  <span data-ttu-id="a1735-1429">当操作数为整数类型时，预定义的 `++` 和 `--` 一元运算符（[后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators)以及[前缀增量和减量运算符](expressions.md#prefix-increment-and-decrement-operators)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1429">The predefined `++` and `--` unary operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="a1735-1430">当操作数为整数类型时，预定义的 `-` 一元运算符（[一元负运算符](expressions.md#unary-minus-operator)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1430">The predefined `-` unary operator ([Unary minus operator](expressions.md#unary-minus-operator)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="a1735-1431">预定义的 `+`、`-`、`*`和 `/` 二元运算符（[算术运算符](expressions.md#arithmetic-operators)），这两个操作数都是整数类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1431">The predefined `+`, `-`, `*`, and `/` binary operators ([Arithmetic operators](expressions.md#arithmetic-operators)), when both operands are of integral types.</span></span>
*  <span data-ttu-id="a1735-1432">显式数值转换（[显式数字转换](conversions.md#explicit-numeric-conversions)）从一个整型类型转换为另一个整型类型，或从 `float` 或 `double` 转换为整型类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1432">Explicit numeric conversions ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from one integral type to another integral type, or from `float` or `double` to an integral type.</span></span>

<span data-ttu-id="a1735-1433">当上述操作之一产生的结果太大而无法在目标类型中表示时，执行该操作的上下文控制产生的行为：</span><span class="sxs-lookup"><span data-stu-id="a1735-1433">When one of the above operations produce a result that is too large to represent in the destination type, the context in which the operation is performed controls the resulting behavior:</span></span>

*  <span data-ttu-id="a1735-1434">在 `checked` 上下文中，如果操作是常量表达式（[常量表达式](expressions.md#constant-expressions)），则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1434">In a `checked` context, if the operation is a constant expression ([Constant expressions](expressions.md#constant-expressions)), a compile-time error occurs.</span></span> <span data-ttu-id="a1735-1435">否则，在运行时执行操作时，将引发 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1435">Otherwise, when the operation is performed at run-time, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="a1735-1436">在 `unchecked` 上下文中，将放弃不适合目标类型的任何高序位，从而截断结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1436">In an `unchecked` context, the result is truncated by discarding any high-order bits that do not fit in the destination type.</span></span>

<span data-ttu-id="a1735-1437">对于非常量表达式（在运行时计算的表达式），该表达式未由任何 `checked` 或 `unchecked` 运算符或语句括起来，则默认溢出检查上下文将 `unchecked`，除非 `checked` 计算的外部因素（如编译器开关和执行环境配置）调用。</span><span class="sxs-lookup"><span data-stu-id="a1735-1437">For non-constant expressions (expressions that are evaluated at run-time) that are not enclosed by any `checked` or `unchecked` operators or statements, the default overflow checking context is `unchecked` unless external factors (such as compiler switches and execution environment configuration) call for `checked` evaluation.</span></span>

<span data-ttu-id="a1735-1438">对于常量表达式（可在编译时完全计算的表达式），默认溢出检查上下文始终 `checked`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1438">For constant expressions (expressions that can be fully evaluated at compile-time), the default overflow checking context is always `checked`.</span></span> <span data-ttu-id="a1735-1439">除非在 `unchecked` 上下文中显式放置了一个常数表达式，否则在该表达式的编译时计算期间发生的溢出将始终导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1439">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur during the compile-time evaluation of the expression always cause compile-time errors.</span></span>

<span data-ttu-id="a1735-1440">匿名函数的主体不受 `checked` 或发生匿名函数 `unchecked` 上下文的影响。</span><span class="sxs-lookup"><span data-stu-id="a1735-1440">The body of an anonymous function is not affected by `checked` or `unchecked` contexts in which the anonymous function occurs.</span></span>

<span data-ttu-id="a1735-1441">示例中</span><span class="sxs-lookup"><span data-stu-id="a1735-1441">In the example</span></span>
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
<span data-ttu-id="a1735-1442">不会报告编译时错误，因为在编译时无法计算两个表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-1442">no compile-time errors are reported since neither of the expressions can be evaluated at compile-time.</span></span> <span data-ttu-id="a1735-1443">在运行时，`F` 方法引发 `System.OverflowException`，`G` 方法返回-727379968 （超出范围的结果的32位）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1443">At run-time, the `F` method throws a `System.OverflowException`, and the `G` method returns -727379968 (the lower 32 bits of the out-of-range result).</span></span> <span data-ttu-id="a1735-1444">`H` 方法的行为取决于编译的默认溢出检查上下文，但它与 `F` 或与 `G`相同。</span><span class="sxs-lookup"><span data-stu-id="a1735-1444">The behavior of the `H` method depends on the default overflow checking context for the compilation, but it is either the same as `F` or the same as `G`.</span></span>

<span data-ttu-id="a1735-1445">示例中</span><span class="sxs-lookup"><span data-stu-id="a1735-1445">In the example</span></span>
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
<span data-ttu-id="a1735-1446">在对 `F` 和 `H` 中的常量表达式进行计算时所发生的溢出会导致报告编译时错误，因为在 `checked` 上下文中对表达式进行求值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1446">the overflows that occur when evaluating the constant expressions in `F` and `H` cause compile-time errors to be reported because the expressions are evaluated in a `checked` context.</span></span> <span data-ttu-id="a1735-1447">在 `G`中计算常数表达式时也会发生溢出，但由于在 `unchecked` 上下文中发生了计算，因此不会报告溢出。</span><span class="sxs-lookup"><span data-stu-id="a1735-1447">An overflow also occurs when evaluating the constant expression in `G`, but since the evaluation takes place in an `unchecked` context, the overflow is not reported.</span></span>

<span data-ttu-id="a1735-1448">`checked` 和 `unchecked` 运算符仅对那些在 "`(`" 和 "`)`" 标记中包含的按</span><span class="sxs-lookup"><span data-stu-id="a1735-1448">The `checked` and `unchecked` operators only affect the overflow checking context for those operations that are textually contained within the "`(`" and "`)`" tokens.</span></span> <span data-ttu-id="a1735-1449">运算符对在计算包含的表达式时被调用的函数成员不起作用。</span><span class="sxs-lookup"><span data-stu-id="a1735-1449">The operators have no effect on function members that are invoked as a result of evaluating the contained expression.</span></span> <span data-ttu-id="a1735-1450">示例中</span><span class="sxs-lookup"><span data-stu-id="a1735-1450">In the example</span></span>
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
<span data-ttu-id="a1735-1451">在 `F` 中使用 `checked` 不会影响 `Multiply`中的 `x * y` 计算，因此在默认溢出检查上下文中对 `x * y` 进行求值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1451">the use of `checked` in `F` does not affect the evaluation of `x * y` in `Multiply`, so `x * y` is evaluated in the default overflow checking context.</span></span>

<span data-ttu-id="a1735-1452">用十六进制表示法编写带符号整数类型的常量时，`unchecked` 运算符非常方便。</span><span class="sxs-lookup"><span data-stu-id="a1735-1452">The `unchecked` operator is convenient when writing constants of the signed integral types in hexadecimal notation.</span></span> <span data-ttu-id="a1735-1453">例如：</span><span class="sxs-lookup"><span data-stu-id="a1735-1453">For example:</span></span>
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

<span data-ttu-id="a1735-1454">上述两个十六进制常量均为类型 `uint`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1454">Both of the hexadecimal constants above are of type `uint`.</span></span> <span data-ttu-id="a1735-1455">由于常量不在 `int` 范围内（没有 `unchecked` 运算符），因此强制转换为 `int` 会产生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1455">Because the constants are outside the `int` range, without the `unchecked` operator, the casts to `int` would produce compile-time errors.</span></span>

<span data-ttu-id="a1735-1456">`checked` 和 `unchecked` 运算符和语句使程序员能够控制某些数值计算的某些方面。</span><span class="sxs-lookup"><span data-stu-id="a1735-1456">The `checked` and `unchecked` operators and statements allow programmers to control certain aspects of some numeric calculations.</span></span> <span data-ttu-id="a1735-1457">但是，某些数值运算符的行为取决于其操作数的数据类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1457">However, the behavior of some numeric operators depends on their operands' data types.</span></span> <span data-ttu-id="a1735-1458">例如，两个小数相乘始终导致溢出异常，即使在显式 `unchecked` 的构造中也是如此。</span><span class="sxs-lookup"><span data-stu-id="a1735-1458">For example, multiplying two decimals always results in an exception on overflow even within an explicitly `unchecked` construct.</span></span> <span data-ttu-id="a1735-1459">同样，如果在显式 `checked` 的构造中，将两个浮点数相乘会导致溢出异常。</span><span class="sxs-lookup"><span data-stu-id="a1735-1459">Similarly, multiplying two floats never results in an exception on overflow even within an explicitly `checked` construct.</span></span> <span data-ttu-id="a1735-1460">此外，其他运算符永远不会受检查模式（无论是默认的还是显式的）的影响。</span><span class="sxs-lookup"><span data-stu-id="a1735-1460">In addition, other operators are never affected by the mode of checking, whether default or explicit.</span></span>

### <a name="default-value-expressions"></a><span data-ttu-id="a1735-1461">默认值表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-1461">Default value expressions</span></span>

<span data-ttu-id="a1735-1462">默认值表达式用于获取类型的默认值（[默认值](variables.md#default-values)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1462">A default value expression is used to obtain the default value ([Default values](variables.md#default-values)) of a type.</span></span> <span data-ttu-id="a1735-1463">通常，默认值表达式用于类型参数，因为如果类型参数是值类型或引用类型，则它可能不是已知的。</span><span class="sxs-lookup"><span data-stu-id="a1735-1463">Typically a default value expression is used for type parameters, since it may not be known if the type parameter is a value type or a reference type.</span></span> <span data-ttu-id="a1735-1464">（除非已知类型参数是引用类型，否则不存在从 `null` 文本到类型参数的转换。）</span><span class="sxs-lookup"><span data-stu-id="a1735-1464">(No conversion exists from the `null` literal to a type parameter unless the type parameter is known to be a reference type.)</span></span>

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

<span data-ttu-id="a1735-1465">如果*default_value_expression*中的*类型*在运行时计算为引用类型，则结果将 `null` 转换为该类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1465">If the *type* in a *default_value_expression* evaluates at run-time to a reference type, the result is `null` converted to that type.</span></span> <span data-ttu-id="a1735-1466">如果*default_value_expression*中的*类型*在运行时计算为值类型，则结果为*Value_type*的默认值（[默认构造函数](types.md#default-constructors)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1466">If the *type* in a *default_value_expression* evaluates at run-time to a value type, the result is the *value_type*'s default value ([Default constructors](types.md#default-constructors)).</span></span>

<span data-ttu-id="a1735-1467">如果类型是引用类型或已知为引用类型的类型参数（[类型参数约束](classes.md#type-parameter-constraints)），则*default_value_expression*是常量表达式（[常量表达式](expressions.md#constant-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1467">A *default_value_expression* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) if the type is a reference type or a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="a1735-1468">此外，如果该类型为以下值类型之一，则*default_value_expression*为常量表达式： `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`、`bool`或任意枚举类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1468">In addition, a *default_value_expression* is a constant expression if the type is one of the following value types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, or any enumeration type.</span></span>


### <a name="nameof-expressions"></a><span data-ttu-id="a1735-1469">Nameof 表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-1469">Nameof expressions</span></span>

<span data-ttu-id="a1735-1470">*Nameof_expression*用于获取作为常量字符串的程序实体的名称。</span><span class="sxs-lookup"><span data-stu-id="a1735-1470">A *nameof_expression* is used to obtain the name of a program entity as a constant string.</span></span>

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

<span data-ttu-id="a1735-1471">语法上说， *named_entity*操作数始终是表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-1471">Grammatically speaking, the *named_entity* operand is always an expression.</span></span> <span data-ttu-id="a1735-1472">由于 `nameof` 不是保留关键字，因此，nameof 表达式在语法上始终不明确，因为调用简单名称 `nameof`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1472">Because `nameof` is not a reserved keyword, a nameof expression is always syntactically ambiguous with an invocation of the simple name `nameof`.</span></span> <span data-ttu-id="a1735-1473">出于兼容性原因，如果名称的名称查找（[简单名称](expressions.md#simple-names)） `nameof` 成功，则无论调用是否合法，都将表达式视为*invocation_expression* 。</span><span class="sxs-lookup"><span data-stu-id="a1735-1473">For compatibility reasons, if a name lookup ([Simple names](expressions.md#simple-names)) of the name `nameof` succeeds, the expression is treated as an *invocation_expression* -- regardless of whether the invocation is legal.</span></span> <span data-ttu-id="a1735-1474">否则为*nameof_expression*。</span><span class="sxs-lookup"><span data-stu-id="a1735-1474">Otherwise it is a *nameof_expression*.</span></span>

<span data-ttu-id="a1735-1475">*Nameof_expression*的*named_entity*的含义是表达式的含义;即，作为*simple_name*、 *base_access*或*member_access*。</span><span class="sxs-lookup"><span data-stu-id="a1735-1475">The meaning of the *named_entity* of a *nameof_expression* is the meaning of it as an expression; that is, either as a *simple_name*, a *base_access* or a *member_access*.</span></span> <span data-ttu-id="a1735-1476">但是，在[简单名称](expressions.md#simple-names)和[成员访问](expressions.md#member-access)中描述的查找会导致错误，因为实例成员是在静态上下文中找到的，所以*nameof_expression*不产生这样的错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1476">However, where the lookup described in [Simple names](expressions.md#simple-names) and [Member access](expressions.md#member-access) results in an error because an instance member was found in a static context, a *nameof_expression* produces no such error.</span></span>

<span data-ttu-id="a1735-1477">如果*named_entity*指定方法组具有*type_argument_list*，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1477">It is a compile-time error for a *named_entity* designating a method group to have a *type_argument_list*.</span></span> <span data-ttu-id="a1735-1478">*Named_entity_target*的类型 `dynamic`为编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1478">It is a compile time error for a *named_entity_target* to have the type `dynamic`.</span></span>

<span data-ttu-id="a1735-1479">*Nameof_expression*是 `string`类型的常量表达式，在运行时不起作用。</span><span class="sxs-lookup"><span data-stu-id="a1735-1479">A *nameof_expression* is a constant expression of type `string`, and has no effect at runtime.</span></span> <span data-ttu-id="a1735-1480">具体而言，它的*named_entity*不会进行评估，出于明确的赋值分析目的将被忽略（[简单表达式的一般规则](variables.md#general-rules-for-simple-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1480">Specifically, its *named_entity* is not evaluated, and is ignored for the purposes of definite assignment analysis ([General rules for simple expressions](variables.md#general-rules-for-simple-expressions)).</span></span> <span data-ttu-id="a1735-1481">它的值是可选的最终*type_argument_list*之前*named_entity*的最后一个标识符，转换方式如下：</span><span class="sxs-lookup"><span data-stu-id="a1735-1481">Its value is the last identifier of the *named_entity* before the optional final *type_argument_list*, transformed in the following way:</span></span>

* <span data-ttu-id="a1735-1482">如果使用前缀 "`@`"，则将其删除。</span><span class="sxs-lookup"><span data-stu-id="a1735-1482">The prefix "`@`", if used, is removed.</span></span>
* <span data-ttu-id="a1735-1483">每个*unicode_escape_sequence*都转换为其对应的 unicode 字符。</span><span class="sxs-lookup"><span data-stu-id="a1735-1483">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
* <span data-ttu-id="a1735-1484">删除任何*formatting_characters* 。</span><span class="sxs-lookup"><span data-stu-id="a1735-1484">Any *formatting_characters* are removed.</span></span>

<span data-ttu-id="a1735-1485">当在标识符之间测试相等性时，[标识符](lexical-structure.md#identifiers)中会应用相同的转换。</span><span class="sxs-lookup"><span data-stu-id="a1735-1485">These are the same transformations applied in [Identifiers](lexical-structure.md#identifiers) when testing equality between identifiers.</span></span>

<span data-ttu-id="a1735-1486">TODO：示例</span><span class="sxs-lookup"><span data-stu-id="a1735-1486">TODO: examples</span></span>

### <a name="anonymous-method-expressions"></a><span data-ttu-id="a1735-1487">匿名方法表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-1487">Anonymous method expressions</span></span>

<span data-ttu-id="a1735-1488">*Anonymous_method_expression*是定义匿名函数的两种方法之一。</span><span class="sxs-lookup"><span data-stu-id="a1735-1488">An *anonymous_method_expression* is one of two ways of defining an anonymous function.</span></span> <span data-ttu-id="a1735-1489">[匿名函数表达式](expressions.md#anonymous-function-expressions)中进一步介绍了这些功能。</span><span class="sxs-lookup"><span data-stu-id="a1735-1489">These are further described in [Anonymous function expressions](expressions.md#anonymous-function-expressions).</span></span>

## <a name="unary-operators"></a><span data-ttu-id="a1735-1490">一元运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-1490">Unary operators</span></span>

<span data-ttu-id="a1735-1491">`?`、`+`、`-`、`!`、`~`、`++`、`--`、强制转换和 `await` 运算符称为一元运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-1491">The `?`, `+`, `-`, `!`, `~`, `++`, `--`, cast, and `await` operators are called the unary operators.</span></span>

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

<span data-ttu-id="a1735-1492">如果*unary_expression*的操作数 `dynamic`编译时类型，则它是动态绑定的（[动态绑定](expressions.md#dynamic-binding)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1492">If the operand of a *unary_expression* has the compile-time type `dynamic`, it is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="a1735-1493">在这种情况下，将 `dynamic`*unary_expression*的编译时类型，并将使用操作数的运行时类型在运行时进行以下描述的解决方法。</span><span class="sxs-lookup"><span data-stu-id="a1735-1493">In this case the compile-time type of the *unary_expression* is `dynamic`, and the resolution described below will take place at run-time using the run-time type of the operand.</span></span>

### <a name="null-conditional-operator"></a><span data-ttu-id="a1735-1494">Null 条件运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-1494">Null-conditional operator</span></span>

<span data-ttu-id="a1735-1495">仅当操作数为非 null 时，null 条件运算符才将操作列表应用于其操作数。</span><span class="sxs-lookup"><span data-stu-id="a1735-1495">The null-conditional operator applies a list of operations to its operand only if that operand is non-null.</span></span> <span data-ttu-id="a1735-1496">否则，将 `null`应用运算符的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1496">Otherwise the result of applying the operator is `null`.</span></span>

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

<span data-ttu-id="a1735-1497">操作列表可以包括成员访问和元素访问操作（可能自身为空条件）以及调用。</span><span class="sxs-lookup"><span data-stu-id="a1735-1497">The list of operations can include member access and element access operations (which may themselves be null-conditional), as well as invocation.</span></span>

<span data-ttu-id="a1735-1498">例如，表达式 `a.b?[0]?.c()` 是一种具有*primary_expression* `a.b` 和*null_conditional_operations* `?[0]` （null 条件元素访问）、`?.c` （null 条件成员访问）和 `()` （调用）的*null_conditional_expression* 。</span><span class="sxs-lookup"><span data-stu-id="a1735-1498">For example, the expression `a.b?[0]?.c()` is a *null_conditional_expression* with a *primary_expression* `a.b` and *null_conditional_operations* `?[0]` (null-conditional element access), `?.c` (null-conditional member access) and `()` (invocation).</span></span>

<span data-ttu-id="a1735-1499">对于具有*primary_expression* `P`的*null_conditional_expression* `E`，让 `E0` 是通过从每个包含一个的 `?` *null_conditional_operations*中删除前导 `E` 而获得的表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-1499">For a *null_conditional_expression* `E` with a *primary_expression* `P`, let `E0` be the expression obtained by textually removing the leading `?` from each of the *null_conditional_operations* of `E` that have one.</span></span> <span data-ttu-id="a1735-1500">从概念上讲，`E0` 是将计算的表达式，如果 `?`所表示的 null 检查没有找到 `null`，则会计算该表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-1500">Conceptually, `E0` is the expression that will be evaluated if none of the null checks represented by the `?`s do find a `null`.</span></span>

<span data-ttu-id="a1735-1501">此外，让 `E1` 是通过从 `E`中的第一个*null_conditional_operations*只删除前导 `?` 来获取的表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-1501">Also, let `E1` be the expression obtained by textually removing the leading `?` from just the first of the *null_conditional_operations* in `E`.</span></span> <span data-ttu-id="a1735-1502">这可能会导致*主表达式*（如果只有一个 `?`）或另一个*null_conditional_expression*。</span><span class="sxs-lookup"><span data-stu-id="a1735-1502">This may lead to a *primary-expression* (if there was just one `?`) or to another *null_conditional_expression*.</span></span>

<span data-ttu-id="a1735-1503">例如，如果 `E` 是表达式 `a.b?[0]?.c()`，则 `E0` 为表达式 `a.b[0].c()`，`E1` 为表达式 `a.b[0]?.c()`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1503">For example, if `E` is the expression `a.b?[0]?.c()`, then `E0` is the expression `a.b[0].c()` and `E1` is the expression `a.b[0]?.c()`.</span></span>

<span data-ttu-id="a1735-1504">如果 `E0` 分类为 "无"，则 `E` 分类为 "无"。</span><span class="sxs-lookup"><span data-stu-id="a1735-1504">If `E0` is classified as nothing, then `E` is classified as nothing.</span></span> <span data-ttu-id="a1735-1505">否则，将 E 分类为值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1505">Otherwise E is classified as a value.</span></span>

<span data-ttu-id="a1735-1506">`E0` 和 `E1` 用于确定 `E`的含义：</span><span class="sxs-lookup"><span data-stu-id="a1735-1506">`E0` and `E1` are used to determine the meaning of `E`:</span></span>

*  <span data-ttu-id="a1735-1507">如果 `E` 作为*statement_expression* `E` 的含义与语句相同</span><span class="sxs-lookup"><span data-stu-id="a1735-1507">If `E` occurs as a *statement_expression* the meaning of `E` is the same as the statement</span></span>

   ```csharp
   if ((object)P != null) E1;
   ```

   <span data-ttu-id="a1735-1508">但 P 只计算一次。</span><span class="sxs-lookup"><span data-stu-id="a1735-1508">except that P is evaluated only once.</span></span>

*  <span data-ttu-id="a1735-1509">否则，如果 `E0` 归类为 nothing，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1509">Otherwise, if `E0` is classified as nothing a compile-time error occurs.</span></span>

*  <span data-ttu-id="a1735-1510">否则，让 `T0` 成为 `E0`的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1510">Otherwise, let `T0` be the type of `E0`.</span></span>

   *  <span data-ttu-id="a1735-1511">如果 `T0` 是未知类型参数，或者是不可以为 null 的值类型，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1511">If `T0` is a type parameter that is not known to be a reference type or a non-nullable value type, a compile-time error occurs.</span></span>

   *  <span data-ttu-id="a1735-1512">如果 `T0` 是不可以为 null 的值类型，则 `E` 的类型 `T0?`，并且 `E` 的含义与</span><span class="sxs-lookup"><span data-stu-id="a1735-1512">If `T0` is a non-nullable value type, then the type of `E` is `T0?`, and the meaning of `E` is the same as</span></span>

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      <span data-ttu-id="a1735-1513">但 `P` 只计算一次。</span><span class="sxs-lookup"><span data-stu-id="a1735-1513">except that `P` is evaluated only once.</span></span>

   *  <span data-ttu-id="a1735-1514">否则，E 的类型为 T0，E 的含义与</span><span class="sxs-lookup"><span data-stu-id="a1735-1514">Otherwise the type of E is T0, and the meaning of E is the same as</span></span>

      ```csharp
      ((object)P == null) ? null : E1
      ```

      <span data-ttu-id="a1735-1515">但 `P` 只计算一次。</span><span class="sxs-lookup"><span data-stu-id="a1735-1515">except that `P` is evaluated only once.</span></span>

<span data-ttu-id="a1735-1516">如果 `E1` 本身就是*null_conditional_expression*，则将再次应用这些规则，并为 `null` 嵌套测试，直到没有更多的 `?`，并且表达式已缩小到主-表达式 `E0`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1516">If `E1` is itself a *null_conditional_expression*, then these rules are applied again, nesting the tests for `null` until there are no further `?`'s, and the expression has been reduced all the way down to the primary-expression `E0`.</span></span>

<span data-ttu-id="a1735-1517">例如，如果表达式 `a.b?[0]?.c()` 作为语句表达式出现，如语句中所示：</span><span class="sxs-lookup"><span data-stu-id="a1735-1517">For example, if the expression `a.b?[0]?.c()` occurs as a statement-expression, as in the statement:</span></span>
```csharp
a.b?[0]?.c();
```
<span data-ttu-id="a1735-1518">其含义等效于：</span><span class="sxs-lookup"><span data-stu-id="a1735-1518">its meaning is equivalent to:</span></span>
```csharp
if (a.b != null) a.b[0]?.c();
```
<span data-ttu-id="a1735-1519">这同样等效于：</span><span class="sxs-lookup"><span data-stu-id="a1735-1519">which again is equivalent to:</span></span>
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
<span data-ttu-id="a1735-1520">除了 `a.b` 和 `a.b[0]` 只计算一次。</span><span class="sxs-lookup"><span data-stu-id="a1735-1520">Except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

<span data-ttu-id="a1735-1521">如果它发生在使用其值的上下文中，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a1735-1521">If it occurs in a context where its value is used, as in:</span></span>
```csharp
var x = a.b?[0]?.c();
```
<span data-ttu-id="a1735-1522">并且假设最终调用的类型不是可以为 null 的值类型，其含义等效于：</span><span class="sxs-lookup"><span data-stu-id="a1735-1522">and assuming that the type of the final invocation is not a non-nullable value type, its meaning is equivalent to:</span></span>
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
<span data-ttu-id="a1735-1523">除了 `a.b` 和 `a.b[0]` 只计算一次。</span><span class="sxs-lookup"><span data-stu-id="a1735-1523">except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

#### <a name="null-conditional-expressions-as-projection-initializers"></a><span data-ttu-id="a1735-1524">空条件表达式作为投影初始值设定项</span><span class="sxs-lookup"><span data-stu-id="a1735-1524">Null-conditional expressions as projection initializers</span></span>

<span data-ttu-id="a1735-1525">如果在*anonymous_object_creation_expression* （[匿名对象创建表达式](expressions.md#anonymous-object-creation-expressions)）结束时，只允许使用 null 条件表达式作为*member_declarator* （也可以是 null 条件）成员访问。</span><span class="sxs-lookup"><span data-stu-id="a1735-1525">A null-conditional expression is only allowed as a *member_declarator* in an *anonymous_object_creation_expression* ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) if it ends with an (optionally null-conditional) member access.</span></span> <span data-ttu-id="a1735-1526">语法上，此要求可表示为：</span><span class="sxs-lookup"><span data-stu-id="a1735-1526">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

<span data-ttu-id="a1735-1527">这是上述*null_conditional_expression*语法的一种特殊情况。</span><span class="sxs-lookup"><span data-stu-id="a1735-1527">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="a1735-1528">在[匿名对象创建表达式](expressions.md#anonymous-object-creation-expressions)中*member_declarator*的生产仅包括*null_conditional_member_access*。</span><span class="sxs-lookup"><span data-stu-id="a1735-1528">The production for *member_declarator* in [Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions) then includes only *null_conditional_member_access*.</span></span>

#### <a name="null-conditional-expressions-as-statement-expressions"></a><span data-ttu-id="a1735-1529">空条件表达式作为语句表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-1529">Null-conditional expressions as statement expressions</span></span>

<span data-ttu-id="a1735-1530">如果以 null 条件表达式结束，则仅允许将其作为*statement_expression* （[expression 语句](statements.md#expression-statements)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1530">A null-conditional expression is only allowed as a *statement_expression* ([Expression statements](statements.md#expression-statements)) if it ends with an invocation.</span></span> <span data-ttu-id="a1735-1531">语法上，此要求可表示为：</span><span class="sxs-lookup"><span data-stu-id="a1735-1531">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="a1735-1532">这是上述*null_conditional_expression*语法的一种特殊情况。</span><span class="sxs-lookup"><span data-stu-id="a1735-1532">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="a1735-1533">然后，[表达式语句](statements.md#expression-statements)中*statement_expression*的生产仅包括*null_conditional_invocation_expression*。</span><span class="sxs-lookup"><span data-stu-id="a1735-1533">The production for *statement_expression* in [Expression statements](statements.md#expression-statements) then includes only *null_conditional_invocation_expression*.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="a1735-1534">一元加运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-1534">Unary plus operator</span></span>

<span data-ttu-id="a1735-1535">对于 `+x`形式的操作，将应用一元运算符重载决策（[一元运算符重载决策](expressions.md#unary-operator-overload-resolution)）来选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-1535">For an operation of the form `+x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a1735-1536">操作数将转换为所选运算符的参数类型，结果的类型是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1536">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="a1735-1537">预定义的一元加法运算符是：</span><span class="sxs-lookup"><span data-stu-id="a1735-1537">The predefined unary plus operators are:</span></span>

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

<span data-ttu-id="a1735-1538">对于每个运算符，结果只是操作数的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1538">For each of these operators, the result is simply the value of the operand.</span></span>

### <a name="unary-minus-operator"></a><span data-ttu-id="a1735-1539">一元减法运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-1539">Unary minus operator</span></span>

<span data-ttu-id="a1735-1540">对于 `-x`形式的操作，将应用一元运算符重载决策（[一元运算符重载决策](expressions.md#unary-operator-overload-resolution)）来选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-1540">For an operation of the form `-x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a1735-1541">操作数将转换为所选运算符的参数类型，结果的类型是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1541">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="a1735-1542">预定义的求反运算符是：</span><span class="sxs-lookup"><span data-stu-id="a1735-1542">The predefined negation operators are:</span></span>

*  <span data-ttu-id="a1735-1543">整数取反：</span><span class="sxs-lookup"><span data-stu-id="a1735-1543">Integer negation:</span></span>

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   <span data-ttu-id="a1735-1544">通过从零中减去 `x` 来计算结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1544">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="a1735-1545">如果 `x` 的值是操作数类型的最小可表示的值（对于 `int` 为-2 ^ 31，对于 `long`，则为-2 ^ 63），则不能在操作数类型内表示 `x` 的数学求反。</span><span class="sxs-lookup"><span data-stu-id="a1735-1545">If the value of `x` is the smallest representable value of the operand type (-2^31 for `int` or -2^63 for `long`), then the mathematical negation of `x` is not representable within the operand type.</span></span> <span data-ttu-id="a1735-1546">如果在 `checked` 上下文中发生此情况，则会引发 `System.OverflowException`;如果它出现在 `unchecked` 上下文内，则结果为操作数的值，并且不报告溢出。</span><span class="sxs-lookup"><span data-stu-id="a1735-1546">If this occurs within a `checked` context, a `System.OverflowException` is thrown; if it occurs within an `unchecked` context, the result is the value of the operand and the overflow is not reported.</span></span>

   <span data-ttu-id="a1735-1547">如果求反运算符的操作数为类型 `uint`，则将其转换为类型 `long`，并 `long`结果的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1547">If the operand of the negation operator is of type `uint`, it is converted to type `long`, and the type of the result is `long`.</span></span> <span data-ttu-id="a1735-1548">例外情况是允许将 `int` 值-2147483648 （-2 ^ 31）编写为十进制整数文本（[整数文本](lexical-structure.md#integer-literals)）的规则。</span><span class="sxs-lookup"><span data-stu-id="a1735-1548">An exception is the rule that permits the `int` value -2147483648 (-2^31) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

   <span data-ttu-id="a1735-1549">如果求反运算符的操作数为类型 `ulong`，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1549">If the operand of the negation operator is of type `ulong`, a compile-time error occurs.</span></span> <span data-ttu-id="a1735-1550">例外情况是允许将 `long` 值-9223372036854775808 （-2 ^ 63）写入为十进制整数文本（[整数文本](lexical-structure.md#integer-literals)）的规则。</span><span class="sxs-lookup"><span data-stu-id="a1735-1550">An exception is the rule that permits the `long` value -9223372036854775808 (-2^63) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

*  <span data-ttu-id="a1735-1551">浮点反：</span><span class="sxs-lookup"><span data-stu-id="a1735-1551">Floating-point negation:</span></span>

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   <span data-ttu-id="a1735-1552">结果为其符号反转 `x` 的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1552">The result is the value of `x` with its sign inverted.</span></span> <span data-ttu-id="a1735-1553">如果 `x` 为 NaN，则结果也为 NaN。</span><span class="sxs-lookup"><span data-stu-id="a1735-1553">If `x` is NaN, the result is also NaN.</span></span>

*  <span data-ttu-id="a1735-1554">小数求反：</span><span class="sxs-lookup"><span data-stu-id="a1735-1554">Decimal negation:</span></span>

   ```csharp
   decimal operator -(decimal x);
   ```

   <span data-ttu-id="a1735-1555">通过从零中减去 `x` 来计算结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1555">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="a1735-1556">Decimal 求反等效于使用 `System.Decimal`类型的一元减号运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-1556">Decimal negation is equivalent to using the unary minus operator of type `System.Decimal`.</span></span>

### <a name="logical-negation-operator"></a><span data-ttu-id="a1735-1557">逻辑非运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-1557">Logical negation operator</span></span>

<span data-ttu-id="a1735-1558">对于 `!x`形式的操作，将应用一元运算符重载决策（[一元运算符重载决策](expressions.md#unary-operator-overload-resolution)）来选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-1558">For an operation of the form `!x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a1735-1559">操作数将转换为所选运算符的参数类型，结果的类型是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1559">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="a1735-1560">只存在一个预定义的逻辑求反运算符：</span><span class="sxs-lookup"><span data-stu-id="a1735-1560">Only one predefined logical negation operator exists:</span></span>
```csharp
bool operator !(bool x);
```

<span data-ttu-id="a1735-1561">此运算符计算操作数的逻辑非运算：如果操作数为 `true`，则结果为 `false`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1561">This operator computes the logical negation of the operand: If the operand is `true`, the result is `false`.</span></span> <span data-ttu-id="a1735-1562">如果操作数为 `false`，则结果为 `true`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1562">If the operand is `false`, the result is `true`.</span></span>

### <a name="bitwise-complement-operator"></a><span data-ttu-id="a1735-1563">按位求补运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-1563">Bitwise complement operator</span></span>

<span data-ttu-id="a1735-1564">对于 `~x`形式的操作，将应用一元运算符重载决策（[一元运算符重载决策](expressions.md#unary-operator-overload-resolution)）来选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-1564">For an operation of the form `~x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a1735-1565">操作数将转换为所选运算符的参数类型，结果的类型是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1565">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="a1735-1566">预定义的按位求补运算符是：</span><span class="sxs-lookup"><span data-stu-id="a1735-1566">The predefined bitwise complement operators are:</span></span>
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

<span data-ttu-id="a1735-1567">对于每个运算符，操作的结果是 `x`的按位求补。</span><span class="sxs-lookup"><span data-stu-id="a1735-1567">For each of these operators, the result of the operation is the bitwise complement of `x`.</span></span>

<span data-ttu-id="a1735-1568">每个枚举类型 `E` 隐式提供以下按位求补运算符：</span><span class="sxs-lookup"><span data-stu-id="a1735-1568">Every enumeration type `E` implicitly provides the following bitwise complement operator:</span></span>

```csharp
E operator ~(E x);
```

<span data-ttu-id="a1735-1569">计算 `~x`的结果（其中，`x` 是 `E` 具有基础类型 `U`的枚举类型的表达式）与计算 `(E)(~(U)x)`完全相同，不同之处在于，在 `E` 上下文（[检查的和未](expressions.md#the-checked-and-unchecked-operators)检查的运算符）中始终执行到 `unchecked` 的转换。</span><span class="sxs-lookup"><span data-stu-id="a1735-1569">The result of evaluating `~x`, where `x` is an expression of an enumeration type `E` with an underlying type `U`, is exactly the same as evaluating `(E)(~(U)x)`, except that the conversion to `E` is always performed as if in an `unchecked` context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span>

### <a name="prefix-increment-and-decrement-operators"></a><span data-ttu-id="a1735-1570">前缀增量和减量运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-1570">Prefix increment and decrement operators</span></span>

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

<span data-ttu-id="a1735-1571">前缀递增或递减运算的操作数必须是分类为变量、属性访问或索引器访问的表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-1571">The operand of a prefix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="a1735-1572">操作的结果是与操作数类型相同的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1572">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="a1735-1573">如果前缀递增或递减运算的操作数是属性或索引器访问，则属性或索引器必须同时具有 `get` 和 `set` 访问器。</span><span class="sxs-lookup"><span data-stu-id="a1735-1573">If the operand of a prefix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="a1735-1574">如果不是这种情况，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1574">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="a1735-1575">一元运算符重载决策（[一元运算符重载决策](expressions.md#unary-operator-overload-resolution)）应用于选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-1575">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a1735-1576">为以下类型存在预定义的 `++` 和 `--` 运算符： `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`和任何枚举类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1576">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="a1735-1577">预定义的 `++` 运算符返回通过向操作数添加1而生成的值，预定义的 `--` 运算符返回通过从操作数中减去1而生成的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1577">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="a1735-1578">在 `checked` 上下文中，如果此加法或减法的结果超出了结果类型的范围，且结果类型为整型类型或枚举类型，则将引发 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1578">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="a1735-1579">`++x` 或 `--x` 形式的前缀递增或递减操作的运行时处理包括以下步骤：</span><span class="sxs-lookup"><span data-stu-id="a1735-1579">The run-time processing of a prefix increment or decrement operation of the form `++x` or `--x` consists of the following steps:</span></span>

*   <span data-ttu-id="a1735-1580">如果 `x` 归类为变量：</span><span class="sxs-lookup"><span data-stu-id="a1735-1580">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="a1735-1581">计算 `x` 以产生变量。</span><span class="sxs-lookup"><span data-stu-id="a1735-1581">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="a1735-1582">将 `x` 所选运算符作为其参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-1582">The selected operator is invoked with the value of `x` as its argument.</span></span>
    * <span data-ttu-id="a1735-1583">运算符返回的值存储在 `x`的计算给定的位置。</span><span class="sxs-lookup"><span data-stu-id="a1735-1583">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="a1735-1584">运算符返回的值成为操作的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1584">The value returned by the operator becomes the result of the operation.</span></span>
*   <span data-ttu-id="a1735-1585">如果 `x` 归类为属性或索引器访问：</span><span class="sxs-lookup"><span data-stu-id="a1735-1585">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="a1735-1586">实例表达式（如果 `x` 不 `static`）和参数列表（如果 `x` 是索引器访问）与 `x` 关联，并在后续的 `get` 和 `set` 访问器调用中使用结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1586">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="a1735-1587">调用 `x` 的 `get` 访问器。</span><span class="sxs-lookup"><span data-stu-id="a1735-1587">The `get` accessor of `x` is invoked.</span></span>
    * <span data-ttu-id="a1735-1588">使用 `get` 访问器返回的值作为其参数调用所选运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-1588">The selected operator is invoked with the value returned by the `get` accessor as its argument.</span></span>
    * <span data-ttu-id="a1735-1589">使用运算符作为其 `value` 参数返回的值调用 `x` 的 `set` 访问器。</span><span class="sxs-lookup"><span data-stu-id="a1735-1589">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="a1735-1590">运算符返回的值成为操作的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1590">The value returned by the operator becomes the result of the operation.</span></span>

<span data-ttu-id="a1735-1591">`++` 和 `--` 运算符还支持后缀表示法（[后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1591">The `++` and `--` operators also support postfix notation ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="a1735-1592">通常，`x++` 或 `x--` 的结果是操作前 `x` 的值，而 `++x` 或 `--x` 的结果是操作后 `x` 的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1592">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="a1735-1593">在任一情况下，操作后 `x` 本身具有相同的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1593">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="a1735-1594">可以使用后缀或前缀表示法来调用 `operator++` 或 `operator--` 实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-1594">An `operator++` or `operator--` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="a1735-1595">对于这两个表示法，不能有单独的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-1595">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="a1735-1596">强制转换表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-1596">Cast expressions</span></span>

<span data-ttu-id="a1735-1597">*Cast_expression*用于将表达式显式转换为给定类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1597">A *cast_expression* is used to explicitly convert an expression to a given type.</span></span>

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

<span data-ttu-id="a1735-1598">格式为 `(T)E`的*cast_expression* ，其中 `T` 是一*种类型*，`E` 为*unary_expression*，则执行 `E` 的值到类型 `T`的显式转换（[显式转换](conversions.md#explicit-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1598">A *cast_expression* of the form `(T)E`, where `T` is a *type* and `E` is a *unary_expression*, performs an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) of the value of `E` to type `T`.</span></span> <span data-ttu-id="a1735-1599">如果不存在从 `E` 到 `T`的显式转换，则发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-1599">If no explicit conversion exists from `E` to `T`, a binding-time error occurs.</span></span> <span data-ttu-id="a1735-1600">否则，结果为显式转换生成的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1600">Otherwise, the result is the value produced by the explicit conversion.</span></span> <span data-ttu-id="a1735-1601">即使 `E` 表示变量，结果也始终归类为值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1601">The result is always classified as a value, even if `E` denotes a variable.</span></span>

<span data-ttu-id="a1735-1602">*Cast_expression*的语法导致某些语义歧义。</span><span class="sxs-lookup"><span data-stu-id="a1735-1602">The grammar for a *cast_expression* leads to certain syntactic ambiguities.</span></span> <span data-ttu-id="a1735-1603">例如，表达式 `(x)-y` 可以解释为*cast_expression* （将 `-y` 强制转换为类型 `x`），或作为*additive_expression*与*parenthesized_expression*组合（这将计算该值 `x - y)`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1603">For example, the expression `(x)-y` could either be interpreted as a *cast_expression* (a cast of `-y` to type `x`) or as an *additive_expression* combined with a *parenthesized_expression* (which computes the value `x - y)`.</span></span>

<span data-ttu-id="a1735-1604">若要解决*cast_expression*歧义，请注意以下规则：仅当满足以下条件之一时，括在括号中的一个或多个*标记*为的序列（[空格](lexical-structure.md#white-space)）才被视为*cast_expression*的开头：</span><span class="sxs-lookup"><span data-stu-id="a1735-1604">To resolve *cast_expression* ambiguities, the following rule exists: A sequence of one or more *token*s ([White space](lexical-structure.md#white-space)) enclosed in parentheses is considered the start of a *cast_expression* only if at least one of the following are true:</span></span>

*  <span data-ttu-id="a1735-1605">标记序列对于*类型*是正确的语法，而不是*表达式*的语法。</span><span class="sxs-lookup"><span data-stu-id="a1735-1605">The sequence of tokens is correct grammar for a *type*, but not for an *expression*.</span></span>
*  <span data-ttu-id="a1735-1606">标记序列对于*类型*是正确的语法，紧跟在右括号后面的标记是标记 "`~`"、标记 "`!`"、令牌 "`(`"、*标识符*（[Unicode 字符转义序列](lexical-structure.md#unicode-character-escape-sequences)）、*文本*（[文本](lexical-structure.md#literals)）或除 `as` 和 `is`之外的任何*关键字*（[关键字](lexical-structure.md#keywords)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1606">The sequence of tokens is correct grammar for a *type*, and the token immediately following the closing parentheses is the token "`~`", the token "`!`", the token "`(`", an *identifier* ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)), a *literal* ([Literals](lexical-structure.md#literals)), or any *keyword* ([Keywords](lexical-structure.md#keywords)) except `as` and `is`.</span></span>

<span data-ttu-id="a1735-1607">上面的 "更正语法" 一词只是指标记序列必须符合特定的语法生产。</span><span class="sxs-lookup"><span data-stu-id="a1735-1607">The term "correct grammar" above means only that the sequence of tokens must conform to the particular grammatical production.</span></span> <span data-ttu-id="a1735-1608">具体而言，它不考虑任何构成标识符的实际含义。</span><span class="sxs-lookup"><span data-stu-id="a1735-1608">It specifically does not consider the actual meaning of any constituent identifiers.</span></span> <span data-ttu-id="a1735-1609">例如，如果 `x` 和 `y` 是标识符，则 `x.y` 是类型的正确语法，即使 `x.y` 实际上不表示类型也是如此。</span><span class="sxs-lookup"><span data-stu-id="a1735-1609">For example, if `x` and `y` are identifiers, then `x.y` is correct grammar for a type, even if `x.y` doesn't actually denote a type.</span></span>

<span data-ttu-id="a1735-1610">从消除歧义规则开始，如果 `x` 和 `y` 为标识符、`(x)y`、`(x)(y)`和 `(x)(-y)`，则 cast_expression 为 *`(x)-y`* ，即使 `x` 标识一个类型，也是如此。</span><span class="sxs-lookup"><span data-stu-id="a1735-1610">From the disambiguation rule it follows that, if `x` and `y` are identifiers, `(x)y`, `(x)(y)`, and `(x)(-y)` are *cast_expression*s, but `(x)-y` is not, even if `x` identifies a type.</span></span> <span data-ttu-id="a1735-1611">但是，如果 `x` 是标识预定义类型（如 `int`）的关键字，则所有四个窗体*cast_expression*s （因为这种关键字本身不可能是表达式）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1611">However, if `x` is a keyword that identifies a predefined type (such as `int`), then all four forms are *cast_expression*s (because such a keyword could not possibly be an expression by itself).</span></span>

### <a name="await-expressions"></a><span data-ttu-id="a1735-1612">Await 表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-1612">Await expressions</span></span>

<span data-ttu-id="a1735-1613">Await 运算符用于暂停封闭异步函数的计算，直到操作数表示的异步操作完成。</span><span class="sxs-lookup"><span data-stu-id="a1735-1613">The await operator is used to suspend evaluation of the enclosing async function until the asynchronous operation represented by the operand has completed.</span></span>

```antlr
await_expression
    : 'await' unary_expression
    ;
```

<span data-ttu-id="a1735-1614">只允许在异步函数（[迭代](classes.md#iterators)器）的主体中使用*await_expression* 。</span><span class="sxs-lookup"><span data-stu-id="a1735-1614">An *await_expression* is only allowed in the body of an async function ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="a1735-1615">在最近的封闭 async 函数中， *await_expression*可能不会出现在以下位置：</span><span class="sxs-lookup"><span data-stu-id="a1735-1615">Within the nearest enclosing async function, an *await_expression* may not occur in these places:</span></span>

*  <span data-ttu-id="a1735-1616">嵌套（非异步）匿名函数内</span><span class="sxs-lookup"><span data-stu-id="a1735-1616">Inside a nested (non-async) anonymous function</span></span>
*  <span data-ttu-id="a1735-1617">在*lock_statement*块内</span><span class="sxs-lookup"><span data-stu-id="a1735-1617">Inside the block of a *lock_statement*</span></span>
*  <span data-ttu-id="a1735-1618">在不安全的上下文中</span><span class="sxs-lookup"><span data-stu-id="a1735-1618">In an unsafe context</span></span>

<span data-ttu-id="a1735-1619">请注意， *await_expression*不能出现在*query_expression*内的大多数位置，因为这些是在语法上转换为使用非 async lambda 表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-1619">Note that an *await_expression* cannot occur in most places within a *query_expression*, because those are syntactically transformed to use non-async lambda expressions.</span></span>

<span data-ttu-id="a1735-1620">在异步函数内部，不能将 `await` 用作标识符。</span><span class="sxs-lookup"><span data-stu-id="a1735-1620">Inside of an async function, `await` cannot be used as an identifier.</span></span> <span data-ttu-id="a1735-1621">因此，await 表达式和涉及标识符的各种表达式之间没有语法多义性。</span><span class="sxs-lookup"><span data-stu-id="a1735-1621">There is therefore no syntactic ambiguity between await-expressions and various expressions involving identifiers.</span></span> <span data-ttu-id="a1735-1622">在异步函数的外部，`await` 充当普通标识符。</span><span class="sxs-lookup"><span data-stu-id="a1735-1622">Outside of async functions, `await` acts as a normal identifier.</span></span>

<span data-ttu-id="a1735-1623">*Await_expression*的操作数称为***任务***。</span><span class="sxs-lookup"><span data-stu-id="a1735-1623">The operand of an *await_expression* is called the ***task***.</span></span> <span data-ttu-id="a1735-1624">它表示一个异步操作，该操作在计算*await_expression*时可能会也可能不会完成。</span><span class="sxs-lookup"><span data-stu-id="a1735-1624">It represents an asynchronous operation that may or may not be complete at the time the *await_expression* is evaluated.</span></span> <span data-ttu-id="a1735-1625">Await 运算符的用途是在等待的任务完成之前挂起封闭异步函数的执行，然后获取其结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1625">The purpose of the await operator is to suspend execution of the enclosing async function until the awaited task is complete, and then obtain its outcome.</span></span>

#### <a name="awaitable-expressions"></a><span data-ttu-id="a1735-1626">可等待表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-1626">Awaitable expressions</span></span>

<span data-ttu-id="a1735-1627">Await 表达式的任务需要***可等待***。</span><span class="sxs-lookup"><span data-stu-id="a1735-1627">The task of an await expression is required to be ***awaitable***.</span></span> <span data-ttu-id="a1735-1628">如果以下包含其中一项，则表达式 `t` 为可等待：</span><span class="sxs-lookup"><span data-stu-id="a1735-1628">An expression `t` is awaitable if one of the following holds:</span></span>

*  <span data-ttu-id="a1735-1629">`t` 的编译时类型为 `dynamic`</span><span class="sxs-lookup"><span data-stu-id="a1735-1629">`t` is of compile time type `dynamic`</span></span>
*  <span data-ttu-id="a1735-1630">`t` 具有一个名为 `GetAwaiter` 的可访问实例或扩展方法，其中没有任何参数和类型参数，以及以下所有保留 `A` 的返回类型：</span><span class="sxs-lookup"><span data-stu-id="a1735-1630">`t` has an accessible instance or extension method called `GetAwaiter` with no parameters and no type parameters, and a return type `A` for which all of the following hold:</span></span>
   * <span data-ttu-id="a1735-1631">`A` 实现接口 `System.Runtime.CompilerServices.INotifyCompletion` （对于简洁起见，为 `INotifyCompletion`）</span><span class="sxs-lookup"><span data-stu-id="a1735-1631">`A` implements the interface `System.Runtime.CompilerServices.INotifyCompletion` (hereafter known as `INotifyCompletion` for brevity)</span></span>
   * <span data-ttu-id="a1735-1632">`A` 具有类型的可访问的可读实例属性 `IsCompleted` `bool`</span><span class="sxs-lookup"><span data-stu-id="a1735-1632">`A` has an accessible, readable instance property `IsCompleted` of type `bool`</span></span>
   * <span data-ttu-id="a1735-1633">`A` 具有可访问的实例方法 `GetResult`，无任何参数和类型参数</span><span class="sxs-lookup"><span data-stu-id="a1735-1633">`A` has an accessible instance method `GetResult` with no parameters and no type parameters</span></span>

<span data-ttu-id="a1735-1634">`GetAwaiter` 方法的目的是获取任务的***awaiter*** 。</span><span class="sxs-lookup"><span data-stu-id="a1735-1634">The purpose of the `GetAwaiter` method is to obtain an ***awaiter*** for the task.</span></span> <span data-ttu-id="a1735-1635">类型 `A` 称为 await 表达式的***awaiter 类型***。</span><span class="sxs-lookup"><span data-stu-id="a1735-1635">The type `A` is called the ***awaiter type*** for the await expression.</span></span>

<span data-ttu-id="a1735-1636">`IsCompleted` 属性的目的是确定任务是否已完成。</span><span class="sxs-lookup"><span data-stu-id="a1735-1636">The purpose of the `IsCompleted` property is to determine if the task is already complete.</span></span> <span data-ttu-id="a1735-1637">如果是这样，则无需暂停评估。</span><span class="sxs-lookup"><span data-stu-id="a1735-1637">If so, there is no need to suspend evaluation.</span></span>

<span data-ttu-id="a1735-1638">`INotifyCompletion.OnCompleted` 方法的目的是将 "延续" 注册到任务;也就是说，将在任务完成后调用的委托（类型为 `System.Action`）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1638">The purpose of the `INotifyCompletion.OnCompleted` method is to sign up a "continuation" to the task; i.e. a delegate (of type `System.Action`) that will be invoked once the task is complete.</span></span>

<span data-ttu-id="a1735-1639">`GetResult` 方法的目的是在任务完成后获取任务的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1639">The purpose of the `GetResult` method is to obtain the outcome of the task once it is complete.</span></span> <span data-ttu-id="a1735-1640">此结果可能会成功完成（可能包含结果值），也可能是由 `GetResult` 方法引发的异常。</span><span class="sxs-lookup"><span data-stu-id="a1735-1640">This outcome may be successful completion, possibly with a result value, or it may be an exception which is thrown by the `GetResult` method.</span></span>

#### <a name="classification-of-await-expressions"></a><span data-ttu-id="a1735-1641">Await 表达式的分类</span><span class="sxs-lookup"><span data-stu-id="a1735-1641">Classification of await expressions</span></span>

<span data-ttu-id="a1735-1642">表达式 `await t` 的分类方式与 `(t).GetAwaiter().GetResult()`表达式的方式相同。</span><span class="sxs-lookup"><span data-stu-id="a1735-1642">The expression `await t` is classified the same way as the expression `(t).GetAwaiter().GetResult()`.</span></span> <span data-ttu-id="a1735-1643">因此，如果 `GetResult` 的返回类型为 `void`，则*await_expression*分类为 nothing。</span><span class="sxs-lookup"><span data-stu-id="a1735-1643">Thus, if the return type of `GetResult` is `void`, the *await_expression* is classified as nothing.</span></span> <span data-ttu-id="a1735-1644">如果 `T`，则将*await_expression*分类为类型 `T`的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1644">If it has a non-void return type `T`, the *await_expression* is classified as a value of type `T`.</span></span>

#### <a name="runtime-evaluation-of-await-expressions"></a><span data-ttu-id="a1735-1645">Await 表达式的运行时计算</span><span class="sxs-lookup"><span data-stu-id="a1735-1645">Runtime evaluation of await expressions</span></span>

<span data-ttu-id="a1735-1646">在运行时，表达式 `await t` 的计算方法如下：</span><span class="sxs-lookup"><span data-stu-id="a1735-1646">At runtime, the expression `await t` is evaluated as follows:</span></span>

*  <span data-ttu-id="a1735-1647">通过 `(t).GetAwaiter()`计算表达式来获取 awaiter `a`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1647">An awaiter `a` is obtained by evaluating the expression `(t).GetAwaiter()`.</span></span>
*  <span data-ttu-id="a1735-1648">通过计算表达式 `(a).IsCompleted`获取 `bool` `b`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1648">A `bool` `b` is obtained by evaluating the expression `(a).IsCompleted`.</span></span>
*  <span data-ttu-id="a1735-1649">如果 `false` `b`，则评估将取决于 `a` 是否实现了接口 `System.Runtime.CompilerServices.ICriticalNotifyCompletion` （对于简洁起见，也称为 `ICriticalNotifyCompletion`）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1649">If `b` is `false` then evaluation depends on whether `a` implements the interface `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (hereafter known as `ICriticalNotifyCompletion` for brevity).</span></span> <span data-ttu-id="a1735-1650">此检查是在绑定时进行的;例如，如果 `a` 的编译时间类型 `dynamic`，则在运行时，否则为。</span><span class="sxs-lookup"><span data-stu-id="a1735-1650">This check is done at binding time; i.e. at runtime if `a` has the compile time type `dynamic`, and at compile time otherwise.</span></span> <span data-ttu-id="a1735-1651">让 `r` 表示恢复委托（[迭代](classes.md#iterators)器）：</span><span class="sxs-lookup"><span data-stu-id="a1735-1651">Let `r` denote the resumption delegate ([Iterators](classes.md#iterators)):</span></span>
    * <span data-ttu-id="a1735-1652">如果 `a` 未实现 `ICriticalNotifyCompletion`，则计算 `(a as (INotifyCompletion)).OnCompleted(r)` 表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-1652">If `a` does not implement `ICriticalNotifyCompletion`, then the expression `(a as (INotifyCompletion)).OnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="a1735-1653">如果 `a` 实现 `ICriticalNotifyCompletion`，则计算 `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` 表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-1653">If `a` does implement `ICriticalNotifyCompletion`, then the expression `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="a1735-1654">然后，将挂起计算，并将控制权返回给 async 函数的当前调用方。</span><span class="sxs-lookup"><span data-stu-id="a1735-1654">Evaluation is then suspended, and control is returned to the current caller of the async function.</span></span>
*  <span data-ttu-id="a1735-1655">紧随之后（如果 `true``b`），或在以后调用恢复委托（如果 `b` `false`），将计算表达式 `(a).GetResult()`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1655">Either immediately after (if `b` was `true`), or upon later invocation of the resumption delegate (if `b` was `false`), the expression `(a).GetResult()` is evaluated.</span></span> <span data-ttu-id="a1735-1656">如果返回值，则该值是*await_expression*的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1656">If it returns a value, that value is the result of the *await_expression*.</span></span> <span data-ttu-id="a1735-1657">否则，结果为 nothing。</span><span class="sxs-lookup"><span data-stu-id="a1735-1657">Otherwise the result is nothing.</span></span>

<span data-ttu-id="a1735-1658">Awaiter 实现的接口方法 `INotifyCompletion.OnCompleted` 和 `ICriticalNotifyCompletion.UnsafeOnCompleted` 应导致委托 `r` 最多调用一次。</span><span class="sxs-lookup"><span data-stu-id="a1735-1658">An awaiter's implementation of the interface methods `INotifyCompletion.OnCompleted` and `ICriticalNotifyCompletion.UnsafeOnCompleted` should cause the delegate `r` to be invoked at most once.</span></span> <span data-ttu-id="a1735-1659">否则，封闭异步函数的行为是不确定的。</span><span class="sxs-lookup"><span data-stu-id="a1735-1659">Otherwise, the behavior of the enclosing async function is undefined.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="a1735-1660">算术运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-1660">Arithmetic operators</span></span>

<span data-ttu-id="a1735-1661">`*`、`/`、`%`、`+`和 `-` 运算符称为算术运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-1661">The `*`, `/`, `%`, `+`, and `-` operators are called the arithmetic operators.</span></span>

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

<span data-ttu-id="a1735-1662">如果算术运算符的操作数具有编译时类型 `dynamic`，则该表达式将动态绑定（[动态绑定](expressions.md#dynamic-binding)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1662">If an operand of an arithmetic operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="a1735-1663">在这种情况下，将 `dynamic`表达式的编译时类型，并将使用编译时类型为 `dynamic`的操作数的运行时类型在运行时执行下面描述的解决方法。</span><span class="sxs-lookup"><span data-stu-id="a1735-1663">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

### <a name="multiplication-operator"></a><span data-ttu-id="a1735-1664">乘法运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-1664">Multiplication operator</span></span>

<span data-ttu-id="a1735-1665">对于 `x * y`形式的操作，将应用二元运算符重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）来选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-1665">For an operation of the form `x * y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a1735-1666">操作数将转换为所选运算符的参数类型，结果的类型就是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1666">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="a1735-1667">下面列出了预定义的乘法运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-1667">The predefined multiplication operators are listed below.</span></span> <span data-ttu-id="a1735-1668">运算符都计算 `x` 和 `y`的产品。</span><span class="sxs-lookup"><span data-stu-id="a1735-1668">The operators all compute the product of `x` and `y`.</span></span>

*  <span data-ttu-id="a1735-1669">整数乘法：</span><span class="sxs-lookup"><span data-stu-id="a1735-1669">Integer multiplication:</span></span>

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   <span data-ttu-id="a1735-1670">在 `checked` 上下文中，如果该产品超出了结果类型的范围，则会引发 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1670">In a `checked` context, if the product is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="a1735-1671">在 `unchecked` 上下文中，不会报告溢出，并且放弃结果类型范围外的任何重要高序位。</span><span class="sxs-lookup"><span data-stu-id="a1735-1671">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>


*  <span data-ttu-id="a1735-1672">浮点乘法：</span><span class="sxs-lookup"><span data-stu-id="a1735-1672">Floating-point multiplication:</span></span>

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   <span data-ttu-id="a1735-1673">根据 IEEE 754 算法的规则计算产品。</span><span class="sxs-lookup"><span data-stu-id="a1735-1673">The product is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="a1735-1674">下表列出了非零有限值、零、无穷大和 NaN 的所有可能组合的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1674">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="a1735-1675">在表中，`x` 和 `y` 为正有限值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1675">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="a1735-1676">`z` 是 `x * y`的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1676">`z` is the result of `x * y`.</span></span> <span data-ttu-id="a1735-1677">如果结果对于目标类型来说太大，则 `z` 无限大。</span><span class="sxs-lookup"><span data-stu-id="a1735-1677">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="a1735-1678">如果结果对于目标类型来说太小，则 `z` 为零。</span><span class="sxs-lookup"><span data-stu-id="a1735-1678">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | <span data-ttu-id="a1735-1679">+y</span><span class="sxs-lookup"><span data-stu-id="a1735-1679">+y</span></span>   | <span data-ttu-id="a1735-1680">-y</span><span class="sxs-lookup"><span data-stu-id="a1735-1680">-y</span></span>   | <span data-ttu-id="a1735-1681">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1681">+0</span></span>  | <span data-ttu-id="a1735-1682">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1682">-0</span></span>  | <span data-ttu-id="a1735-1683">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1683">+inf</span></span> | <span data-ttu-id="a1735-1684">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1684">-inf</span></span> | <span data-ttu-id="a1735-1685">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1685">NaN</span></span> | 
   | <span data-ttu-id="a1735-1686">+x</span><span class="sxs-lookup"><span data-stu-id="a1735-1686">+x</span></span>   | <span data-ttu-id="a1735-1687">+z</span><span class="sxs-lookup"><span data-stu-id="a1735-1687">+z</span></span>   | <span data-ttu-id="a1735-1688">-z</span><span class="sxs-lookup"><span data-stu-id="a1735-1688">-z</span></span>   | <span data-ttu-id="a1735-1689">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1689">+0</span></span>  | <span data-ttu-id="a1735-1690">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1690">-0</span></span>  | <span data-ttu-id="a1735-1691">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1691">+inf</span></span> | <span data-ttu-id="a1735-1692">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1692">-inf</span></span> | <span data-ttu-id="a1735-1693">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1693">NaN</span></span> | 
   | <span data-ttu-id="a1735-1694">-x</span><span class="sxs-lookup"><span data-stu-id="a1735-1694">-x</span></span>   | <span data-ttu-id="a1735-1695">-z</span><span class="sxs-lookup"><span data-stu-id="a1735-1695">-z</span></span>   | <span data-ttu-id="a1735-1696">+z</span><span class="sxs-lookup"><span data-stu-id="a1735-1696">+z</span></span>   | <span data-ttu-id="a1735-1697">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1697">-0</span></span>  | <span data-ttu-id="a1735-1698">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1698">+0</span></span>  | <span data-ttu-id="a1735-1699">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1699">-inf</span></span> | <span data-ttu-id="a1735-1700">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1700">+inf</span></span> | <span data-ttu-id="a1735-1701">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1701">NaN</span></span> | 
   | <span data-ttu-id="a1735-1702">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1702">+0</span></span>   | <span data-ttu-id="a1735-1703">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1703">+0</span></span>   | <span data-ttu-id="a1735-1704">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1704">-0</span></span>   | <span data-ttu-id="a1735-1705">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1705">+0</span></span>  | <span data-ttu-id="a1735-1706">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1706">-0</span></span>  | <span data-ttu-id="a1735-1707">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1707">NaN</span></span>  | <span data-ttu-id="a1735-1708">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1708">NaN</span></span>  | <span data-ttu-id="a1735-1709">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1709">NaN</span></span> | 
   | <span data-ttu-id="a1735-1710">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1710">-0</span></span>   | <span data-ttu-id="a1735-1711">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1711">-0</span></span>   | <span data-ttu-id="a1735-1712">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1712">+0</span></span>   | <span data-ttu-id="a1735-1713">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1713">-0</span></span>  | <span data-ttu-id="a1735-1714">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1714">+0</span></span>  | <span data-ttu-id="a1735-1715">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1715">NaN</span></span>  | <span data-ttu-id="a1735-1716">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1716">NaN</span></span>  | <span data-ttu-id="a1735-1717">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1717">NaN</span></span> | 
   | <span data-ttu-id="a1735-1718">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1718">+inf</span></span> | <span data-ttu-id="a1735-1719">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1719">+inf</span></span> | <span data-ttu-id="a1735-1720">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1720">-inf</span></span> | <span data-ttu-id="a1735-1721">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1721">NaN</span></span> | <span data-ttu-id="a1735-1722">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1722">NaN</span></span> | <span data-ttu-id="a1735-1723">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1723">+inf</span></span> | <span data-ttu-id="a1735-1724">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1724">-inf</span></span> | <span data-ttu-id="a1735-1725">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1725">NaN</span></span> | 
   | <span data-ttu-id="a1735-1726">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1726">-inf</span></span> | <span data-ttu-id="a1735-1727">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1727">-inf</span></span> | <span data-ttu-id="a1735-1728">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1728">+inf</span></span> | <span data-ttu-id="a1735-1729">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1729">NaN</span></span> | <span data-ttu-id="a1735-1730">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1730">NaN</span></span> | <span data-ttu-id="a1735-1731">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1731">-inf</span></span> | <span data-ttu-id="a1735-1732">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1732">+inf</span></span> | <span data-ttu-id="a1735-1733">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1733">NaN</span></span> | 
   | <span data-ttu-id="a1735-1734">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1734">NaN</span></span>  | <span data-ttu-id="a1735-1735">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1735">NaN</span></span>  | <span data-ttu-id="a1735-1736">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1736">NaN</span></span>  | <span data-ttu-id="a1735-1737">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1737">NaN</span></span> | <span data-ttu-id="a1735-1738">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1738">NaN</span></span> | <span data-ttu-id="a1735-1739">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1739">NaN</span></span>  | <span data-ttu-id="a1735-1740">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1740">NaN</span></span>  | <span data-ttu-id="a1735-1741">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1741">NaN</span></span> | 

*  <span data-ttu-id="a1735-1742">小数乘法：</span><span class="sxs-lookup"><span data-stu-id="a1735-1742">Decimal multiplication:</span></span>

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   <span data-ttu-id="a1735-1743">如果生成的值太大而无法表示为 `decimal` 格式，则会引发 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1743">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="a1735-1744">如果结果值太小而无法表示为 `decimal` 格式，则结果为零。</span><span class="sxs-lookup"><span data-stu-id="a1735-1744">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="a1735-1745">在进行任何舍入之前，结果的小数位数是两个操作数的刻度之和。</span><span class="sxs-lookup"><span data-stu-id="a1735-1745">The scale of the result, before any rounding, is the sum of the scales of the two operands.</span></span>

   <span data-ttu-id="a1735-1746">Decimal 等效于使用 `System.Decimal`类型的乘法运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-1746">Decimal multiplication is equivalent to using the multiplication operator of type `System.Decimal`.</span></span>


### <a name="division-operator"></a><span data-ttu-id="a1735-1747">除法运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-1747">Division operator</span></span>

<span data-ttu-id="a1735-1748">对于 `x / y`形式的操作，将应用二元运算符重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）来选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-1748">For an operation of the form `x / y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a1735-1749">操作数将转换为所选运算符的参数类型，结果的类型就是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1749">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="a1735-1750">下面列出了预定义的除法运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-1750">The predefined division operators are listed below.</span></span> <span data-ttu-id="a1735-1751">运算符都计算 `x` 和 `y`的商。</span><span class="sxs-lookup"><span data-stu-id="a1735-1751">The operators all compute the quotient of `x` and `y`.</span></span>

*  <span data-ttu-id="a1735-1752">整数相除：</span><span class="sxs-lookup"><span data-stu-id="a1735-1752">Integer division:</span></span>

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   <span data-ttu-id="a1735-1753">如果右操作数的值为零，则会引发 `System.DivideByZeroException`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1753">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="a1735-1754">相除将结果舍入到零。</span><span class="sxs-lookup"><span data-stu-id="a1735-1754">The division rounds the result towards zero.</span></span> <span data-ttu-id="a1735-1755">因此，结果的绝对值是小于或等于两个操作数的商的绝对值的最大整数。</span><span class="sxs-lookup"><span data-stu-id="a1735-1755">Thus the absolute value of the result is the largest possible integer that is less than or equal to the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="a1735-1756">如果两个操作数具有相同的符号，则结果为零或正; 如果两个操作数具有相反的符号，则结果为零或负。</span><span class="sxs-lookup"><span data-stu-id="a1735-1756">The result is zero or positive when the two operands have the same sign and zero or negative when the two operands have opposite signs.</span></span>

   <span data-ttu-id="a1735-1757">如果左操作数是 `int` 或 `long` 值的最小表示，且右操作数 `-1`，则会发生溢出。</span><span class="sxs-lookup"><span data-stu-id="a1735-1757">If the left operand is the smallest representable `int` or `long` value and the right operand is `-1`, an overflow occurs.</span></span> <span data-ttu-id="a1735-1758">在 `checked` 上下文中，这将导致引发 `System.ArithmeticException` （或其子类）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1758">In a `checked` context, this causes a `System.ArithmeticException` (or a subclass thereof) to be thrown.</span></span> <span data-ttu-id="a1735-1759">在 `unchecked` 上下文中，它的实现定义为：是否引发了 `System.ArithmeticException` （或其中的子类），或者溢出与左操作数的结果值无报告。</span><span class="sxs-lookup"><span data-stu-id="a1735-1759">In an `unchecked` context, it is implementation-defined as to whether a `System.ArithmeticException` (or a subclass thereof) is thrown or the overflow goes unreported with the resulting value being that of the left operand.</span></span>

*  <span data-ttu-id="a1735-1760">浮点除法：</span><span class="sxs-lookup"><span data-stu-id="a1735-1760">Floating-point division:</span></span>

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   <span data-ttu-id="a1735-1761">根据 IEEE 754 算法的规则计算商。</span><span class="sxs-lookup"><span data-stu-id="a1735-1761">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="a1735-1762">下表列出了非零有限值、零、无穷大和 NaN 的所有可能组合的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1762">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="a1735-1763">在表中，`x` 和 `y` 为正有限值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1763">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="a1735-1764">`z` 是 `x / y`的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1764">`z` is the result of `x / y`.</span></span> <span data-ttu-id="a1735-1765">如果结果对于目标类型来说太大，则 `z` 无限大。</span><span class="sxs-lookup"><span data-stu-id="a1735-1765">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="a1735-1766">如果结果对于目标类型来说太小，则 `z` 为零。</span><span class="sxs-lookup"><span data-stu-id="a1735-1766">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="a1735-1767">+y</span><span class="sxs-lookup"><span data-stu-id="a1735-1767">+y</span></span>   | <span data-ttu-id="a1735-1768">-y</span><span class="sxs-lookup"><span data-stu-id="a1735-1768">-y</span></span>   | <span data-ttu-id="a1735-1769">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1769">+0</span></span>   | <span data-ttu-id="a1735-1770">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1770">-0</span></span>   | <span data-ttu-id="a1735-1771">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1771">+inf</span></span> | <span data-ttu-id="a1735-1772">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1772">-inf</span></span> | <span data-ttu-id="a1735-1773">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1773">NaN</span></span>  | 
   | <span data-ttu-id="a1735-1774">+x</span><span class="sxs-lookup"><span data-stu-id="a1735-1774">+x</span></span>   | <span data-ttu-id="a1735-1775">+z</span><span class="sxs-lookup"><span data-stu-id="a1735-1775">+z</span></span>   | <span data-ttu-id="a1735-1776">-z</span><span class="sxs-lookup"><span data-stu-id="a1735-1776">-z</span></span>   | <span data-ttu-id="a1735-1777">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1777">+inf</span></span> | <span data-ttu-id="a1735-1778">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1778">-inf</span></span> | <span data-ttu-id="a1735-1779">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1779">+0</span></span>   | <span data-ttu-id="a1735-1780">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1780">-0</span></span>   | <span data-ttu-id="a1735-1781">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1781">NaN</span></span>  | 
   | <span data-ttu-id="a1735-1782">-x</span><span class="sxs-lookup"><span data-stu-id="a1735-1782">-x</span></span>   | <span data-ttu-id="a1735-1783">-z</span><span class="sxs-lookup"><span data-stu-id="a1735-1783">-z</span></span>   | <span data-ttu-id="a1735-1784">+z</span><span class="sxs-lookup"><span data-stu-id="a1735-1784">+z</span></span>   | <span data-ttu-id="a1735-1785">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1785">-inf</span></span> | <span data-ttu-id="a1735-1786">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1786">+inf</span></span> | <span data-ttu-id="a1735-1787">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1787">-0</span></span>   | <span data-ttu-id="a1735-1788">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1788">+0</span></span>   | <span data-ttu-id="a1735-1789">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1789">NaN</span></span>  | 
   | <span data-ttu-id="a1735-1790">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1790">+0</span></span>   | <span data-ttu-id="a1735-1791">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1791">+0</span></span>   | <span data-ttu-id="a1735-1792">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1792">-0</span></span>   | <span data-ttu-id="a1735-1793">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1793">NaN</span></span>  | <span data-ttu-id="a1735-1794">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1794">NaN</span></span>  | <span data-ttu-id="a1735-1795">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1795">+0</span></span>   | <span data-ttu-id="a1735-1796">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1796">-0</span></span>   | <span data-ttu-id="a1735-1797">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1797">NaN</span></span>  | 
   | <span data-ttu-id="a1735-1798">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1798">-0</span></span>   | <span data-ttu-id="a1735-1799">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1799">-0</span></span>   | <span data-ttu-id="a1735-1800">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1800">+0</span></span>   | <span data-ttu-id="a1735-1801">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1801">NaN</span></span>  | <span data-ttu-id="a1735-1802">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1802">NaN</span></span>  | <span data-ttu-id="a1735-1803">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1803">-0</span></span>   | <span data-ttu-id="a1735-1804">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1804">+0</span></span>   | <span data-ttu-id="a1735-1805">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1805">NaN</span></span>  | 
   | <span data-ttu-id="a1735-1806">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1806">+inf</span></span> | <span data-ttu-id="a1735-1807">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1807">+inf</span></span> | <span data-ttu-id="a1735-1808">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1808">-inf</span></span> | <span data-ttu-id="a1735-1809">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1809">+inf</span></span> | <span data-ttu-id="a1735-1810">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1810">-inf</span></span> | <span data-ttu-id="a1735-1811">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1811">NaN</span></span>  | <span data-ttu-id="a1735-1812">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1812">NaN</span></span>  | <span data-ttu-id="a1735-1813">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1813">NaN</span></span>  | 
   | <span data-ttu-id="a1735-1814">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1814">-inf</span></span> | <span data-ttu-id="a1735-1815">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1815">-inf</span></span> | <span data-ttu-id="a1735-1816">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1816">+inf</span></span> | <span data-ttu-id="a1735-1817">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1817">-inf</span></span> | <span data-ttu-id="a1735-1818">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1818">+inf</span></span> | <span data-ttu-id="a1735-1819">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1819">NaN</span></span>  | <span data-ttu-id="a1735-1820">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1820">NaN</span></span>  | <span data-ttu-id="a1735-1821">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1821">NaN</span></span>  | 
   | <span data-ttu-id="a1735-1822">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1822">NaN</span></span>  | <span data-ttu-id="a1735-1823">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1823">NaN</span></span>  | <span data-ttu-id="a1735-1824">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1824">NaN</span></span>  | <span data-ttu-id="a1735-1825">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1825">NaN</span></span>  | <span data-ttu-id="a1735-1826">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1826">NaN</span></span>  | <span data-ttu-id="a1735-1827">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1827">NaN</span></span>  | <span data-ttu-id="a1735-1828">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1828">NaN</span></span>  | <span data-ttu-id="a1735-1829">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1829">NaN</span></span>  | 

*  <span data-ttu-id="a1735-1830">小数部分：</span><span class="sxs-lookup"><span data-stu-id="a1735-1830">Decimal division:</span></span>

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   <span data-ttu-id="a1735-1831">如果右操作数的值为零，则会引发 `System.DivideByZeroException`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1831">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="a1735-1832">如果生成的值太大而无法表示为 `decimal` 格式，则会引发 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1832">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="a1735-1833">如果结果值太小而无法表示为 `decimal` 格式，则结果为零。</span><span class="sxs-lookup"><span data-stu-id="a1735-1833">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="a1735-1834">结果的小数位数是最小的小数位数，将结果等于最接近的可表示的十进制值到真正的数学结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1834">The scale of the result is the smallest scale that will preserve a result equal to the nearest representable decimal value to the true mathematical result.</span></span>

   <span data-ttu-id="a1735-1835">小数部分等效于使用 `System.Decimal`类型的除法运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-1835">Decimal division is equivalent to using the division operator of type `System.Decimal`.</span></span>


### <a name="remainder-operator"></a><span data-ttu-id="a1735-1836">余数运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-1836">Remainder operator</span></span>

<span data-ttu-id="a1735-1837">对于 `x % y`形式的操作，将应用二元运算符重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）来选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-1837">For an operation of the form `x % y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a1735-1838">操作数将转换为所选运算符的参数类型，结果的类型就是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1838">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="a1735-1839">下面列出了预定义的余数运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-1839">The predefined remainder operators are listed below.</span></span> <span data-ttu-id="a1735-1840">运算符都计算 `x` 和 `y`之间的相除余数。</span><span class="sxs-lookup"><span data-stu-id="a1735-1840">The operators all compute the remainder of the division between `x` and `y`.</span></span>

*  <span data-ttu-id="a1735-1841">整数余数：</span><span class="sxs-lookup"><span data-stu-id="a1735-1841">Integer remainder:</span></span>

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   <span data-ttu-id="a1735-1842">`x % y` 的结果是 `x - (x / y) * y`生成的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1842">The result of `x % y` is the value produced by `x - (x / y) * y`.</span></span> <span data-ttu-id="a1735-1843">如果 `y` 为零，则将引发 `System.DivideByZeroException`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1843">If `y` is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="a1735-1844">如果左操作数是 `int` 或 `long` 值的最小值，且右操作数 `-1`，则将引发 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1844">If the left operand is the smallest `int` or `long` value and the right operand is `-1`, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="a1735-1845">在任何情况下，`x % y` 引发异常，`x / y` 不会引发异常。</span><span class="sxs-lookup"><span data-stu-id="a1735-1845">In no case does `x % y` throw an exception where `x / y` would not throw an exception.</span></span>

*  <span data-ttu-id="a1735-1846">浮点余数：</span><span class="sxs-lookup"><span data-stu-id="a1735-1846">Floating-point remainder:</span></span>

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   <span data-ttu-id="a1735-1847">下表列出了非零有限值、零、无穷大和 NaN 的所有可能组合的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1847">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="a1735-1848">在表中，`x` 和 `y` 为正有限值。</span><span class="sxs-lookup"><span data-stu-id="a1735-1848">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="a1735-1849">`z` 是 `x % y` 的结果，并按 `x - n * y`计算，其中 `n` 是小于或等于 `x / y`的最大整数。</span><span class="sxs-lookup"><span data-stu-id="a1735-1849">`z` is the result of `x % y` and is computed as `x - n * y`, where `n` is the largest possible integer that is less than or equal to `x / y`.</span></span> <span data-ttu-id="a1735-1850">计算余数的这种方法类似于用于整数操作数，但不同于 IEEE 754 定义（其中 `n` 是最接近 `x / y`的整数）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1850">This method of computing the remainder is analogous to that used for integer operands, but differs from the IEEE 754 definition (in which `n` is the integer closest to `x / y`).</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="a1735-1851">+y</span><span class="sxs-lookup"><span data-stu-id="a1735-1851">+y</span></span>   | <span data-ttu-id="a1735-1852">-y</span><span class="sxs-lookup"><span data-stu-id="a1735-1852">-y</span></span>   | <span data-ttu-id="a1735-1853">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1853">+0</span></span>   | <span data-ttu-id="a1735-1854">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1854">-0</span></span>   | <span data-ttu-id="a1735-1855">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1855">+inf</span></span> | <span data-ttu-id="a1735-1856">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1856">-inf</span></span> | <span data-ttu-id="a1735-1857">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1857">NaN</span></span>  | 
   | <span data-ttu-id="a1735-1858">+x</span><span class="sxs-lookup"><span data-stu-id="a1735-1858">+x</span></span>   | <span data-ttu-id="a1735-1859">+z</span><span class="sxs-lookup"><span data-stu-id="a1735-1859">+z</span></span>   | <span data-ttu-id="a1735-1860">+z</span><span class="sxs-lookup"><span data-stu-id="a1735-1860">+z</span></span>   | <span data-ttu-id="a1735-1861">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1861">NaN</span></span>  | <span data-ttu-id="a1735-1862">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1862">NaN</span></span>  | <span data-ttu-id="a1735-1863">x</span><span class="sxs-lookup"><span data-stu-id="a1735-1863">x</span></span>    | <span data-ttu-id="a1735-1864">x</span><span class="sxs-lookup"><span data-stu-id="a1735-1864">x</span></span>    | <span data-ttu-id="a1735-1865">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1865">NaN</span></span>  | 
   | <span data-ttu-id="a1735-1866">-x</span><span class="sxs-lookup"><span data-stu-id="a1735-1866">-x</span></span>   | <span data-ttu-id="a1735-1867">-z</span><span class="sxs-lookup"><span data-stu-id="a1735-1867">-z</span></span>   | <span data-ttu-id="a1735-1868">-z</span><span class="sxs-lookup"><span data-stu-id="a1735-1868">-z</span></span>   | <span data-ttu-id="a1735-1869">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1869">NaN</span></span>  | <span data-ttu-id="a1735-1870">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1870">NaN</span></span>  | <span data-ttu-id="a1735-1871">-x</span><span class="sxs-lookup"><span data-stu-id="a1735-1871">-x</span></span>   | <span data-ttu-id="a1735-1872">-x</span><span class="sxs-lookup"><span data-stu-id="a1735-1872">-x</span></span>   | <span data-ttu-id="a1735-1873">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1873">NaN</span></span>  | 
   | <span data-ttu-id="a1735-1874">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1874">+0</span></span>   | <span data-ttu-id="a1735-1875">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1875">+0</span></span>   | <span data-ttu-id="a1735-1876">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1876">+0</span></span>   | <span data-ttu-id="a1735-1877">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1877">NaN</span></span>  | <span data-ttu-id="a1735-1878">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1878">NaN</span></span>  | <span data-ttu-id="a1735-1879">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1879">+0</span></span>   | <span data-ttu-id="a1735-1880">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1880">+0</span></span>   | <span data-ttu-id="a1735-1881">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1881">NaN</span></span>  | 
   | <span data-ttu-id="a1735-1882">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1882">-0</span></span>   | <span data-ttu-id="a1735-1883">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1883">-0</span></span>   | <span data-ttu-id="a1735-1884">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1884">-0</span></span>   | <span data-ttu-id="a1735-1885">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1885">NaN</span></span>  | <span data-ttu-id="a1735-1886">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1886">NaN</span></span>  | <span data-ttu-id="a1735-1887">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1887">-0</span></span>   | <span data-ttu-id="a1735-1888">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1888">-0</span></span>   | <span data-ttu-id="a1735-1889">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1889">NaN</span></span>  | 
   | <span data-ttu-id="a1735-1890">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1890">+inf</span></span> | <span data-ttu-id="a1735-1891">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1891">NaN</span></span>  | <span data-ttu-id="a1735-1892">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1892">NaN</span></span>  | <span data-ttu-id="a1735-1893">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1893">NaN</span></span>  | <span data-ttu-id="a1735-1894">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1894">NaN</span></span>  | <span data-ttu-id="a1735-1895">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1895">NaN</span></span>  | <span data-ttu-id="a1735-1896">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1896">NaN</span></span>  | <span data-ttu-id="a1735-1897">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1897">NaN</span></span>  | 
   | <span data-ttu-id="a1735-1898">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1898">-inf</span></span> | <span data-ttu-id="a1735-1899">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1899">NaN</span></span>  | <span data-ttu-id="a1735-1900">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1900">NaN</span></span>  | <span data-ttu-id="a1735-1901">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1901">NaN</span></span>  | <span data-ttu-id="a1735-1902">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1902">NaN</span></span>  | <span data-ttu-id="a1735-1903">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1903">NaN</span></span>  | <span data-ttu-id="a1735-1904">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1904">NaN</span></span>  | <span data-ttu-id="a1735-1905">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1905">NaN</span></span>  | 
   | <span data-ttu-id="a1735-1906">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1906">NaN</span></span>  | <span data-ttu-id="a1735-1907">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1907">NaN</span></span>  | <span data-ttu-id="a1735-1908">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1908">NaN</span></span>  | <span data-ttu-id="a1735-1909">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1909">NaN</span></span>  | <span data-ttu-id="a1735-1910">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1910">NaN</span></span>  | <span data-ttu-id="a1735-1911">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1911">NaN</span></span>  | <span data-ttu-id="a1735-1912">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1912">NaN</span></span>  | <span data-ttu-id="a1735-1913">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1913">NaN</span></span>  | 

*  <span data-ttu-id="a1735-1914">小数余数：</span><span class="sxs-lookup"><span data-stu-id="a1735-1914">Decimal remainder:</span></span>

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   <span data-ttu-id="a1735-1915">如果右操作数的值为零，则会引发 `System.DivideByZeroException`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1915">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="a1735-1916">在进行舍入之前，结果的小数位数是两个操作数的刻度中较大的值，并且如果非零，则结果的符号与 `x`相同。</span><span class="sxs-lookup"><span data-stu-id="a1735-1916">The scale of the result, before any rounding, is the larger of the scales of the two operands, and the sign of the result, if non-zero, is the same as that of `x`.</span></span>

   <span data-ttu-id="a1735-1917">小数余数等效于使用 `System.Decimal`类型的余数运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-1917">Decimal remainder is equivalent to using the remainder operator of type `System.Decimal`.</span></span>


### <a name="addition-operator"></a><span data-ttu-id="a1735-1918">加法运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-1918">Addition operator</span></span>

<span data-ttu-id="a1735-1919">对于 `x + y`形式的操作，将应用二元运算符重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）来选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-1919">For an operation of the form `x + y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a1735-1920">操作数将转换为所选运算符的参数类型，结果的类型就是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-1920">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="a1735-1921">下面列出了预定义的加法运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-1921">The predefined addition operators are listed below.</span></span> <span data-ttu-id="a1735-1922">对于数值类型和枚举类型，预定义的加法运算符计算两个操作数之和。</span><span class="sxs-lookup"><span data-stu-id="a1735-1922">For numeric and enumeration types, the predefined addition operators compute the sum of the two operands.</span></span> <span data-ttu-id="a1735-1923">当一个或两个操作数为字符串类型时，预定义的加法运算符将连接操作数的字符串表示形式。</span><span class="sxs-lookup"><span data-stu-id="a1735-1923">When one or both operands are of type string, the predefined addition operators concatenate the string representation of the operands.</span></span>

*  <span data-ttu-id="a1735-1924">整数加法：</span><span class="sxs-lookup"><span data-stu-id="a1735-1924">Integer addition:</span></span>

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   <span data-ttu-id="a1735-1925">在 `checked` 上下文中，如果求和超出了结果类型的范围，则会引发 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1925">In a `checked` context, if the sum is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="a1735-1926">在 `unchecked` 上下文中，不会报告溢出，并且放弃结果类型范围外的任何重要高序位。</span><span class="sxs-lookup"><span data-stu-id="a1735-1926">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="a1735-1927">浮点加法：</span><span class="sxs-lookup"><span data-stu-id="a1735-1927">Floating-point addition:</span></span>

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   <span data-ttu-id="a1735-1928">Sum 根据 IEEE 754 算法的规则进行计算。</span><span class="sxs-lookup"><span data-stu-id="a1735-1928">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="a1735-1929">下表列出了非零有限值、零、无穷大和 NaN 的所有可能组合的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1929">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="a1735-1930">在表中，`x` 和 `y` 是非零的有限值，`z` 是 `x + y`的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-1930">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x + y`.</span></span> <span data-ttu-id="a1735-1931">如果 `x` 和 `y` 的量相同但符号相反，`z` 为正零。</span><span class="sxs-lookup"><span data-stu-id="a1735-1931">If `x` and `y` have the same magnitude but opposite signs, `z` is positive zero.</span></span> <span data-ttu-id="a1735-1932">如果 `x + y` 太大而无法在目标类型中表示，则 `z` 使用与 `x + y`相同的无穷。</span><span class="sxs-lookup"><span data-stu-id="a1735-1932">If `x + y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x + y`.</span></span>

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="a1735-1933">y</span><span class="sxs-lookup"><span data-stu-id="a1735-1933">y</span></span>    | <span data-ttu-id="a1735-1934">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1934">+0</span></span>   | <span data-ttu-id="a1735-1935">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1935">-0</span></span>   | <span data-ttu-id="a1735-1936">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1936">+inf</span></span> | <span data-ttu-id="a1735-1937">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1937">-inf</span></span> | <span data-ttu-id="a1735-1938">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1938">NaN</span></span>  | 
   | <span data-ttu-id="a1735-1939">x</span><span class="sxs-lookup"><span data-stu-id="a1735-1939">x</span></span>    | <span data-ttu-id="a1735-1940">z</span><span class="sxs-lookup"><span data-stu-id="a1735-1940">z</span></span>    | <span data-ttu-id="a1735-1941">x</span><span class="sxs-lookup"><span data-stu-id="a1735-1941">x</span></span>    | <span data-ttu-id="a1735-1942">x</span><span class="sxs-lookup"><span data-stu-id="a1735-1942">x</span></span>    | <span data-ttu-id="a1735-1943">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1943">+inf</span></span> | <span data-ttu-id="a1735-1944">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1944">-inf</span></span> | <span data-ttu-id="a1735-1945">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1945">NaN</span></span>  | 
   | <span data-ttu-id="a1735-1946">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1946">+0</span></span>   | <span data-ttu-id="a1735-1947">y</span><span class="sxs-lookup"><span data-stu-id="a1735-1947">y</span></span>    | <span data-ttu-id="a1735-1948">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1948">+0</span></span>   | <span data-ttu-id="a1735-1949">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1949">+0</span></span>   | <span data-ttu-id="a1735-1950">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1950">+inf</span></span> | <span data-ttu-id="a1735-1951">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1951">-inf</span></span> | <span data-ttu-id="a1735-1952">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1952">NaN</span></span>  | 
   | <span data-ttu-id="a1735-1953">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1953">-0</span></span>   | <span data-ttu-id="a1735-1954">y</span><span class="sxs-lookup"><span data-stu-id="a1735-1954">y</span></span>    | <span data-ttu-id="a1735-1955">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-1955">+0</span></span>   | <span data-ttu-id="a1735-1956">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-1956">-0</span></span>   | <span data-ttu-id="a1735-1957">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1957">+inf</span></span> | <span data-ttu-id="a1735-1958">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1958">-inf</span></span> | <span data-ttu-id="a1735-1959">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1959">NaN</span></span>  | 
   | <span data-ttu-id="a1735-1960">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1960">+inf</span></span> | <span data-ttu-id="a1735-1961">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1961">+inf</span></span> | <span data-ttu-id="a1735-1962">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1962">+inf</span></span> | <span data-ttu-id="a1735-1963">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1963">+inf</span></span> | <span data-ttu-id="a1735-1964">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1964">+inf</span></span> | <span data-ttu-id="a1735-1965">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1965">NaN</span></span>  | <span data-ttu-id="a1735-1966">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1966">NaN</span></span>  | 
   | <span data-ttu-id="a1735-1967">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1967">-inf</span></span> | <span data-ttu-id="a1735-1968">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1968">-inf</span></span> | <span data-ttu-id="a1735-1969">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1969">-inf</span></span> | <span data-ttu-id="a1735-1970">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1970">-inf</span></span> | <span data-ttu-id="a1735-1971">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1971">NaN</span></span>  | <span data-ttu-id="a1735-1972">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-1972">-inf</span></span> | <span data-ttu-id="a1735-1973">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1973">NaN</span></span>  | 
   | <span data-ttu-id="a1735-1974">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1974">NaN</span></span>  | <span data-ttu-id="a1735-1975">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1975">NaN</span></span>  | <span data-ttu-id="a1735-1976">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1976">NaN</span></span>  | <span data-ttu-id="a1735-1977">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1977">NaN</span></span>  | <span data-ttu-id="a1735-1978">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1978">NaN</span></span>  | <span data-ttu-id="a1735-1979">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1979">NaN</span></span>  | <span data-ttu-id="a1735-1980">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-1980">NaN</span></span>  | 

*  <span data-ttu-id="a1735-1981">小数加：</span><span class="sxs-lookup"><span data-stu-id="a1735-1981">Decimal addition:</span></span>

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   <span data-ttu-id="a1735-1982">如果生成的值太大而无法表示为 `decimal` 格式，则会引发 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1982">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="a1735-1983">在进行舍入之前，结果的小数位数是两个操作数的刻度中较大的一个。</span><span class="sxs-lookup"><span data-stu-id="a1735-1983">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="a1735-1984">Decimal 加法等效于使用 `System.Decimal`类型的加法运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-1984">Decimal addition is equivalent to using the addition operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="a1735-1985">枚举加法。</span><span class="sxs-lookup"><span data-stu-id="a1735-1985">Enumeration addition.</span></span> <span data-ttu-id="a1735-1986">每个枚举类型都隐式提供以下预定义运算符，其中 `E` 是枚举类型，`U` 是 `E`的基础类型：</span><span class="sxs-lookup"><span data-stu-id="a1735-1986">Every enumeration type implicitly provides the following predefined operators, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   <span data-ttu-id="a1735-1987">在运行时，这些运算符的计算结果与 `(E)((U)x + (U)y)`完全相同。</span><span class="sxs-lookup"><span data-stu-id="a1735-1987">At run-time these operators are evaluated exactly as `(E)((U)x + (U)y)`.</span></span>

*  <span data-ttu-id="a1735-1988">字符串串联：</span><span class="sxs-lookup"><span data-stu-id="a1735-1988">String concatenation:</span></span>

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   <span data-ttu-id="a1735-1989">二元 `+` 运算符的这些重载执行字符串串联。</span><span class="sxs-lookup"><span data-stu-id="a1735-1989">These overloads of the binary `+` operator perform string concatenation.</span></span> <span data-ttu-id="a1735-1990">如果 `null`字符串串联的操作数，则将替换空字符串。</span><span class="sxs-lookup"><span data-stu-id="a1735-1990">If an operand of string concatenation is `null`, an empty string is substituted.</span></span> <span data-ttu-id="a1735-1991">否则，通过调用从类型 `object`继承的虚拟 `ToString` 方法，将任何非字符串参数转换为其字符串表示形式。</span><span class="sxs-lookup"><span data-stu-id="a1735-1991">Otherwise, any non-string argument is converted to its string representation by invoking the virtual `ToString` method inherited from type `object`.</span></span> <span data-ttu-id="a1735-1992">如果 `ToString` 返回 `null`，则将替换空字符串。</span><span class="sxs-lookup"><span data-stu-id="a1735-1992">If `ToString` returns `null`, an empty string is substituted.</span></span>

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

   字符串串联运算符的结果是一个字符串，该字符串由左操作数的字符后跟右操作数的字符组成。 字符串串联运算符从不返回 `null` 值。 <span data-ttu-id="a1735-1995">如果没有足够的内存可用于分配生成的字符串，可能会引发 `System.OutOfMemoryException`。</span><span class="sxs-lookup"><span data-stu-id="a1735-1995">A `System.OutOfMemoryException` may be thrown if there is not enough memory available to allocate the resulting string.</span></span>

*  <span data-ttu-id="a1735-1996">委托组合。</span><span class="sxs-lookup"><span data-stu-id="a1735-1996">Delegate combination.</span></span> <span data-ttu-id="a1735-1997">每个委托类型都隐式提供以下预定义运算符，其中 `D` 为委托类型：</span><span class="sxs-lookup"><span data-stu-id="a1735-1997">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator +(D x, D y);
   ```

   <span data-ttu-id="a1735-1998">二元 `+` 运算符在两个操作数均为某个委托类型 `D`时执行委托组合。</span><span class="sxs-lookup"><span data-stu-id="a1735-1998">The binary `+` operator performs delegate combination when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="a1735-1999">（如果操作数具有不同的委托类型，则会发生绑定时错误。）如果 `null`第一个操作数，则操作的结果为第二个操作数的值（即使还 `null`）。</span><span class="sxs-lookup"><span data-stu-id="a1735-1999">(If the operands have different delegate types, a binding-time error occurs.) If the first operand is `null`, the result of the operation is the value of the second operand (even if that is also `null`).</span></span> <span data-ttu-id="a1735-2000">否则，如果 `null`第二个操作数，则运算结果为第一个操作数的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2000">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="a1735-2001">否则，该操作的结果是一个新的委托实例，它在调用时调用第一个操作数，然后调用第二个操作数。</span><span class="sxs-lookup"><span data-stu-id="a1735-2001">Otherwise, the result of the operation is a new delegate instance that, when invoked, invokes the first operand and then invokes the second operand.</span></span> <span data-ttu-id="a1735-2002">有关委托组合的示例，请参阅[减法运算符](expressions.md#subtraction-operator)和[委托调用](delegates.md#delegate-invocation)。</span><span class="sxs-lookup"><span data-stu-id="a1735-2002">For examples of delegate combination, see [Subtraction operator](expressions.md#subtraction-operator) and [Delegate invocation](delegates.md#delegate-invocation).</span></span> <span data-ttu-id="a1735-2003">由于 `System.Delegate` 不是委托类型，因此不会为其定义 `operator` `+`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2003">Since `System.Delegate` is not a delegate type, `operator` `+` is not defined for it.</span></span>

### <a name="subtraction-operator"></a><span data-ttu-id="a1735-2004">减法运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2004">Subtraction operator</span></span>

<span data-ttu-id="a1735-2005">对于 `x - y`形式的操作，将应用二元运算符重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）来选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-2005">For an operation of the form `x - y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a1735-2006">操作数将转换为所选运算符的参数类型，结果的类型就是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2006">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="a1735-2007">下面列出了预定义的减法运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2007">The predefined subtraction operators are listed below.</span></span> <span data-ttu-id="a1735-2008">运算符都从 `x`中减去 `y`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2008">The operators all subtract `y` from `x`.</span></span>

*  <span data-ttu-id="a1735-2009">整数减法：</span><span class="sxs-lookup"><span data-stu-id="a1735-2009">Integer subtraction:</span></span>

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   <span data-ttu-id="a1735-2010">在 `checked` 上下文中，如果差异超出了结果类型的范围，则会引发 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2010">In a `checked` context, if the difference is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="a1735-2011">在 `unchecked` 上下文中，不会报告溢出，并且放弃结果类型范围外的任何重要高序位。</span><span class="sxs-lookup"><span data-stu-id="a1735-2011">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="a1735-2012">浮点减法：</span><span class="sxs-lookup"><span data-stu-id="a1735-2012">Floating-point subtraction:</span></span>

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   <span data-ttu-id="a1735-2013">根据 IEEE 754 算法的规则计算差异。</span><span class="sxs-lookup"><span data-stu-id="a1735-2013">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="a1735-2014">下表列出了非零有限值、零、无穷大和 Nan 的所有可能组合的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2014">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaNs.</span></span> <span data-ttu-id="a1735-2015">在表中，`x` 和 `y` 是非零的有限值，`z` 是 `x - y`的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2015">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x - y`.</span></span> <span data-ttu-id="a1735-2016">如果 `x` 和 `y` 相等，`z` 为正零。</span><span class="sxs-lookup"><span data-stu-id="a1735-2016">If `x` and `y` are equal, `z` is positive zero.</span></span> <span data-ttu-id="a1735-2017">如果 `x - y` 太大而无法在目标类型中表示，则 `z` 使用与 `x - y`相同的无穷。</span><span class="sxs-lookup"><span data-stu-id="a1735-2017">If `x - y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x - y`.</span></span>

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | <span data-ttu-id="a1735-2018">y</span><span class="sxs-lookup"><span data-stu-id="a1735-2018">y</span></span>    | <span data-ttu-id="a1735-2019">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-2019">+0</span></span>   | <span data-ttu-id="a1735-2020">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-2020">-0</span></span>   | <span data-ttu-id="a1735-2021">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-2021">+inf</span></span> | <span data-ttu-id="a1735-2022">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-2022">-inf</span></span> | <span data-ttu-id="a1735-2023">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-2023">NaN</span></span> | 
   | <span data-ttu-id="a1735-2024">x</span><span class="sxs-lookup"><span data-stu-id="a1735-2024">x</span></span>    | <span data-ttu-id="a1735-2025">z</span><span class="sxs-lookup"><span data-stu-id="a1735-2025">z</span></span>    | <span data-ttu-id="a1735-2026">x</span><span class="sxs-lookup"><span data-stu-id="a1735-2026">x</span></span>    | <span data-ttu-id="a1735-2027">x</span><span class="sxs-lookup"><span data-stu-id="a1735-2027">x</span></span>    | <span data-ttu-id="a1735-2028">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-2028">-inf</span></span> | <span data-ttu-id="a1735-2029">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-2029">+inf</span></span> | <span data-ttu-id="a1735-2030">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-2030">NaN</span></span> | 
   | <span data-ttu-id="a1735-2031">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-2031">+0</span></span>   | <span data-ttu-id="a1735-2032">-y</span><span class="sxs-lookup"><span data-stu-id="a1735-2032">-y</span></span>   | <span data-ttu-id="a1735-2033">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-2033">+0</span></span>   | <span data-ttu-id="a1735-2034">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-2034">+0</span></span>   | <span data-ttu-id="a1735-2035">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-2035">-inf</span></span> | <span data-ttu-id="a1735-2036">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-2036">+inf</span></span> | <span data-ttu-id="a1735-2037">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-2037">NaN</span></span> | 
   | <span data-ttu-id="a1735-2038">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-2038">-0</span></span>   | <span data-ttu-id="a1735-2039">-y</span><span class="sxs-lookup"><span data-stu-id="a1735-2039">-y</span></span>   | <span data-ttu-id="a1735-2040">-0</span><span class="sxs-lookup"><span data-stu-id="a1735-2040">-0</span></span>   | <span data-ttu-id="a1735-2041">+0</span><span class="sxs-lookup"><span data-stu-id="a1735-2041">+0</span></span>   | <span data-ttu-id="a1735-2042">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-2042">-inf</span></span> | <span data-ttu-id="a1735-2043">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-2043">+inf</span></span> | <span data-ttu-id="a1735-2044">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-2044">NaN</span></span> | 
   | <span data-ttu-id="a1735-2045">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-2045">+inf</span></span> | <span data-ttu-id="a1735-2046">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-2046">+inf</span></span> | <span data-ttu-id="a1735-2047">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-2047">+inf</span></span> | <span data-ttu-id="a1735-2048">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-2048">+inf</span></span> | <span data-ttu-id="a1735-2049">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-2049">NaN</span></span>  | <span data-ttu-id="a1735-2050">+inf</span><span class="sxs-lookup"><span data-stu-id="a1735-2050">+inf</span></span> | <span data-ttu-id="a1735-2051">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-2051">NaN</span></span> | 
   | <span data-ttu-id="a1735-2052">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-2052">-inf</span></span> | <span data-ttu-id="a1735-2053">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-2053">-inf</span></span> | <span data-ttu-id="a1735-2054">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-2054">-inf</span></span> | <span data-ttu-id="a1735-2055">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-2055">-inf</span></span> | <span data-ttu-id="a1735-2056">-inf</span><span class="sxs-lookup"><span data-stu-id="a1735-2056">-inf</span></span> | <span data-ttu-id="a1735-2057">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-2057">NaN</span></span>  | <span data-ttu-id="a1735-2058">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-2058">NaN</span></span> | 
   | <span data-ttu-id="a1735-2059">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-2059">NaN</span></span>  | <span data-ttu-id="a1735-2060">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-2060">NaN</span></span>  | <span data-ttu-id="a1735-2061">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-2061">NaN</span></span>  | <span data-ttu-id="a1735-2062">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-2062">NaN</span></span>  | <span data-ttu-id="a1735-2063">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-2063">NaN</span></span>  | <span data-ttu-id="a1735-2064">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-2064">NaN</span></span>  | <span data-ttu-id="a1735-2065">NaN</span><span class="sxs-lookup"><span data-stu-id="a1735-2065">NaN</span></span> | 

*  <span data-ttu-id="a1735-2066">小数减：</span><span class="sxs-lookup"><span data-stu-id="a1735-2066">Decimal subtraction:</span></span>

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   <span data-ttu-id="a1735-2067">如果生成的值太大而无法表示为 `decimal` 格式，则会引发 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2067">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="a1735-2068">在进行舍入之前，结果的小数位数是两个操作数的刻度中较大的一个。</span><span class="sxs-lookup"><span data-stu-id="a1735-2068">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="a1735-2069">Decimal 等效于使用 `System.Decimal`类型的减法运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2069">Decimal subtraction is equivalent to using the subtraction operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="a1735-2070">枚举减法。</span><span class="sxs-lookup"><span data-stu-id="a1735-2070">Enumeration subtraction.</span></span> <span data-ttu-id="a1735-2071">每个枚举类型都隐式提供以下预定义运算符，其中 `E` 是枚举类型，`U` 是 `E`的基础类型：</span><span class="sxs-lookup"><span data-stu-id="a1735-2071">Every enumeration type implicitly provides the following predefined operator, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   U operator -(E x, E y);
   ```

   <span data-ttu-id="a1735-2072">此运算符的计算结果与 `(U)((U)x - (U)y)`完全相同。</span><span class="sxs-lookup"><span data-stu-id="a1735-2072">This operator is evaluated exactly as `(U)((U)x - (U)y)`.</span></span> <span data-ttu-id="a1735-2073">换言之，运算符计算 `x` 和 `y`的序号值之间的差异，结果的类型为枚举的基础类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2073">In other words, the operator computes the difference between the ordinal values of `x` and `y`, and the type of the result is the underlying type of the enumeration.</span></span>

   ```csharp
   E operator -(E x, U y);
   ```

   <span data-ttu-id="a1735-2074">此运算符的计算结果与 `(E)((U)x - y)`完全相同。</span><span class="sxs-lookup"><span data-stu-id="a1735-2074">This operator is evaluated exactly as `(E)((U)x - y)`.</span></span> <span data-ttu-id="a1735-2075">换言之，运算符从枚举的基础类型中减去一个值，从而生成一个枚举值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2075">In other words, the operator subtracts a value from the underlying type of the enumeration, yielding a value of the enumeration.</span></span>

*  <span data-ttu-id="a1735-2076">委托删除。</span><span class="sxs-lookup"><span data-stu-id="a1735-2076">Delegate removal.</span></span> <span data-ttu-id="a1735-2077">每个委托类型都隐式提供以下预定义运算符，其中 `D` 为委托类型：</span><span class="sxs-lookup"><span data-stu-id="a1735-2077">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator -(D x, D y);
   ```

   <span data-ttu-id="a1735-2078">二元 `-` 运算符在两个操作数均为某个委托类型 `D`时执行委托删除。</span><span class="sxs-lookup"><span data-stu-id="a1735-2078">The binary `-` operator performs delegate removal when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="a1735-2079">如果操作数具有不同的委托类型，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-2079">If the operands have different delegate types, a binding-time error occurs.</span></span> <span data-ttu-id="a1735-2080">如果第一个操作数为 `null`，则操作结果为 `null`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2080">If the first operand is `null`, the result of the operation is `null`.</span></span> <span data-ttu-id="a1735-2081">否则，如果 `null`第二个操作数，则运算结果为第一个操作数的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2081">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="a1735-2082">否则，两个操作数都表示具有一个或多个项的调用列表（[委托声明](delegates.md#delegate-declarations)），并且结果是一个新的调用列表，其中包含第一个操作数的列表（从中删除第二个操作数的条目），前提是第二个操作数的列表是第一个操作数的正确连续子列表。</span><span class="sxs-lookup"><span data-stu-id="a1735-2082">Otherwise, both operands represent invocation lists ([Delegate declarations](delegates.md#delegate-declarations)) having one or more entries, and the result is a new invocation list consisting of the first operand's list with the second operand's entries removed from it, provided the second operand's list is a proper contiguous sublist of the first's.</span></span>     <span data-ttu-id="a1735-2083">（若要确定子列表相等性，请将对应的项与委托相等运算符（[委托相等运算符](expressions.md#delegate-equality-operators)）进行比较。）否则，结果为左操作数的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2083">(To determine sublist equality, corresponding entries are compared as for the delegate equality operator ([Delegate equality operators](expressions.md#delegate-equality-operators)).) Otherwise, the result is the value of the left operand.</span></span> <span data-ttu-id="a1735-2084">进程中的两个操作数都不是更改的。</span><span class="sxs-lookup"><span data-stu-id="a1735-2084">Neither of the operands' lists is changed in the process.</span></span> <span data-ttu-id="a1735-2085">如果第二个操作数的列表与第一个操作数列表中连续条目的多个子列表匹配，则会删除最右侧匹配的子列表。</span><span class="sxs-lookup"><span data-stu-id="a1735-2085">If the second operand's list matches multiple sublists of contiguous entries in the first operand's list, the right-most matching sublist of contiguous entries is removed.</span></span> <span data-ttu-id="a1735-2086">如果删除行为导致出现空列表，则结果为 `null`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2086">If removal results in an empty list, the result is `null`.</span></span> <span data-ttu-id="a1735-2087">例如：</span><span class="sxs-lookup"><span data-stu-id="a1735-2087">For example:</span></span>

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

## <a name="shift-operators"></a><span data-ttu-id="a1735-2088">移位运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2088">Shift operators</span></span>

<span data-ttu-id="a1735-2089">`<<` 和 `>>` 运算符用于执行移位运算。</span><span class="sxs-lookup"><span data-stu-id="a1735-2089">The `<<` and `>>` operators are used to perform bit shifting operations.</span></span>

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

<span data-ttu-id="a1735-2090">如果*shift_expression*的操作数 `dynamic`编译时类型，则该表达式将动态绑定（[动态绑定](expressions.md#dynamic-binding)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2090">If an operand of a *shift_expression* has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="a1735-2091">在这种情况下，将 `dynamic`表达式的编译时类型，并将使用编译时类型为 `dynamic`的操作数的运行时类型在运行时执行下面描述的解决方法。</span><span class="sxs-lookup"><span data-stu-id="a1735-2091">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="a1735-2092">对于 `x << count` 或 `x >> count`形式的操作，将应用二进制运算符重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）以选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-2092">For an operation of the form `x << count` or `x >> count`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a1735-2093">操作数将转换为所选运算符的参数类型，结果的类型就是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2093">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="a1735-2094">在声明重载移位运算符时，第一个操作数的类型必须始终为包含运算符声明的类或结构，并且第二个操作数的类型必须始终 `int`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2094">When declaring an overloaded shift operator, the type of the first operand must always be the class or struct containing the operator declaration, and the type of the second operand must always be `int`.</span></span>

<span data-ttu-id="a1735-2095">下面列出了预定义的移位运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2095">The predefined shift operators are listed below.</span></span>

*  <span data-ttu-id="a1735-2096">左移：</span><span class="sxs-lookup"><span data-stu-id="a1735-2096">Shift left:</span></span>

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   <span data-ttu-id="a1735-2097">`<<` 运算符将 `x` 按如下所述计算的位数向左移动。</span><span class="sxs-lookup"><span data-stu-id="a1735-2097">The `<<` operator shifts `x` left by a number of bits computed as described below.</span></span>

   <span data-ttu-id="a1735-2098">丢弃 `x` 的结果类型范围外的高序位，剩余位将向左移动，并将低序位空位位置设置为零。</span><span class="sxs-lookup"><span data-stu-id="a1735-2098">The high-order bits outside the range of the result type of `x` are discarded, the remaining bits are shifted left, and the low-order empty bit positions are set to zero.</span></span>

*  <span data-ttu-id="a1735-2099">右移：</span><span class="sxs-lookup"><span data-stu-id="a1735-2099">Shift right:</span></span>

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   <span data-ttu-id="a1735-2100">`>>` 运算符将 `x` 向右移位按下面所述计算的位数。</span><span class="sxs-lookup"><span data-stu-id="a1735-2100">The `>>` operator shifts `x` right by a number of bits computed as described below.</span></span>

   <span data-ttu-id="a1735-2101">当 `x` 的类型为 `int` 或 `long`时，将放弃 `x` 的低序位，并向右移动剩余的位，并将高顺序空位位置设置为零（如果 `x` 为非负值，则设置为1）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2101">When `x` is of type `int` or `long`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero if `x` is non-negative and set to one if `x` is negative.</span></span>

   <span data-ttu-id="a1735-2102">当 `x` 的类型为 `uint` 或 `ulong`时，将丢弃 `x` 的低序位，其余位将向右移动，而高阶空位位置设置为零。</span><span class="sxs-lookup"><span data-stu-id="a1735-2102">When `x` is of type `uint` or `ulong`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero.</span></span>

<span data-ttu-id="a1735-2103">对于预定义的运算符，将计算要移位的位数，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a1735-2103">For the predefined operators, the number of bits to shift is computed as follows:</span></span>

*  <span data-ttu-id="a1735-2104">当 `x` 的类型 `int` 或 `uint`时，移位计数由 `count`的低序位5位提供。</span><span class="sxs-lookup"><span data-stu-id="a1735-2104">When the type of `x` is `int` or `uint`, the shift count is given by the low-order five bits of `count`.</span></span> <span data-ttu-id="a1735-2105">换句话说，移位计数由 `count & 0x1F`计算得出。</span><span class="sxs-lookup"><span data-stu-id="a1735-2105">In other words, the shift count is computed from `count & 0x1F`.</span></span>
*  <span data-ttu-id="a1735-2106">当 `x` 的类型 `long` 或 `ulong`时，移位计数由 `count`的低序位六位给定。</span><span class="sxs-lookup"><span data-stu-id="a1735-2106">When the type of `x` is `long` or `ulong`, the shift count is given by the low-order six bits of `count`.</span></span> <span data-ttu-id="a1735-2107">换句话说，移位计数由 `count & 0x3F`计算得出。</span><span class="sxs-lookup"><span data-stu-id="a1735-2107">In other words, the shift count is computed from `count & 0x3F`.</span></span>

<span data-ttu-id="a1735-2108">如果生成的移位计数为零，则移位运算符只返回 `x`的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2108">If the resulting shift count is zero, the shift operators simply return the value of `x`.</span></span>

<span data-ttu-id="a1735-2109">移位运算从不导致溢出，并在 `checked` 和 `unchecked` 上下文中产生相同的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2109">Shift operations never cause overflows and produce the same results in `checked` and `unchecked` contexts.</span></span>

<span data-ttu-id="a1735-2110">当 `>>` 运算符的左操作数为有符号整数类型时，运算符将执行算术右移，其中操作数的最高有效位（符号位）的值将传播到高阶空位位置。</span><span class="sxs-lookup"><span data-stu-id="a1735-2110">When the left operand of the `>>` operator is of a signed integral type, the operator performs an arithmetic shift right wherein the value of the most significant bit (the sign bit) of the operand is propagated to the high-order empty bit positions.</span></span> <span data-ttu-id="a1735-2111">当 `>>` 运算符的左操作数是无符号整数类型时，运算符将执行逻辑移位，其中高阶空位位置始终设置为零。</span><span class="sxs-lookup"><span data-stu-id="a1735-2111">When the left operand of the `>>` operator is of an unsigned integral type, the operator performs a logical shift right wherein high-order empty bit positions are always set to zero.</span></span> <span data-ttu-id="a1735-2112">若要执行从操作数类型推断得出的相反运算，可以使用显式强制转换。</span><span class="sxs-lookup"><span data-stu-id="a1735-2112">To perform the opposite operation of that inferred from the operand type, explicit casts can be used.</span></span> <span data-ttu-id="a1735-2113">例如，如果 `x` 是 `int`类型的变量，则运算 `unchecked((int)((uint)x >> y))` 执行 `x`的逻辑向右移位。</span><span class="sxs-lookup"><span data-stu-id="a1735-2113">For example, if `x` is a variable of type `int`, the operation `unchecked((int)((uint)x >> y))` performs a logical shift right of `x`.</span></span>

## <a name="relational-and-type-testing-operators"></a><span data-ttu-id="a1735-2114">关系和类型测试运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2114">Relational and type-testing operators</span></span>

<span data-ttu-id="a1735-2115">`==`、`!=`、`<`、`>`、`<=`、`>=`、`is` 和 `as` 运算符称为关系和类型测试运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2115">The `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` and `as` operators are called the relational and type-testing operators.</span></span>

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

<span data-ttu-id="a1735-2116">[Is](expressions.md#the-is-operator)运算符中介绍了 `is` 运算符，并在[as 运算符](expressions.md#the-as-operator)中描述了 `as` 运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2116">The `is` operator is described in [The is operator](expressions.md#the-is-operator) and the `as` operator is described in [The as operator](expressions.md#the-as-operator).</span></span>

<span data-ttu-id="a1735-2117">`==`、`!=`、`<`、`>`、`<=` 和 `>=` 运算符是***比较运算符***。</span><span class="sxs-lookup"><span data-stu-id="a1735-2117">The `==`, `!=`, `<`, `>`, `<=` and `>=` operators are ***comparison operators***.</span></span>

<span data-ttu-id="a1735-2118">如果比较运算符的操作数 `dynamic`编译时类型，则该表达式将动态绑定（[动态绑定](expressions.md#dynamic-binding)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2118">If an operand of a comparison operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="a1735-2119">在这种情况下，将 `dynamic`表达式的编译时类型，并将使用编译时类型为 `dynamic`的操作数的运行时类型在运行时执行下面描述的解决方法。</span><span class="sxs-lookup"><span data-stu-id="a1735-2119">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="a1735-2120">对于 `x` *op* `y`形式的操作，其中*op*是比较运算符，将应用重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）以选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-2120">For an operation of the form `x` *op* `y`, where *op* is a comparison operator, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a1735-2121">操作数将转换为所选运算符的参数类型，结果的类型就是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2121">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="a1735-2122">以下各节介绍了预定义的比较运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2122">The predefined comparison operators are described in the following sections.</span></span> <span data-ttu-id="a1735-2123">所有预定义的比较运算符都返回 `bool`类型的结果，如下表所述。</span><span class="sxs-lookup"><span data-stu-id="a1735-2123">All predefined comparison operators return a result of type `bool`, as described in the following table.</span></span>


| <span data-ttu-id="a1735-2124">__Operation__</span><span class="sxs-lookup"><span data-stu-id="a1735-2124">__Operation__</span></span> | <span data-ttu-id="a1735-2125">__结果__</span><span class="sxs-lookup"><span data-stu-id="a1735-2125">__Result__</span></span>                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | <span data-ttu-id="a1735-2126">`true` 如果 `x` 等于 `y``false`，则为; 否则为。</span><span class="sxs-lookup"><span data-stu-id="a1735-2126">`true` if `x` is equal to `y`, `false` otherwise</span></span>                 | 
| `x != y`      | <span data-ttu-id="a1735-2127">如果 `x` 不等于 `y``false`，则 `true`; 否则为。</span><span class="sxs-lookup"><span data-stu-id="a1735-2127">`true` if `x` is not equal to `y`, `false` otherwise</span></span>             | 
| `x < y`       | <span data-ttu-id="a1735-2128">如果 `x` 小于 `y``false`，则 `true`; 否则为。</span><span class="sxs-lookup"><span data-stu-id="a1735-2128">`true` if `x` is less than `y`, `false` otherwise</span></span>                | 
| `x > y`       | <span data-ttu-id="a1735-2129">如果 `x` 大于 `y``false`，则 `true`; 否则为。</span><span class="sxs-lookup"><span data-stu-id="a1735-2129">`true` if `x` is greater than `y`, `false` otherwise</span></span>             | 
| `x <= y`      | <span data-ttu-id="a1735-2130">如果 `x` 小于或等于 `y``false`，则 `true`; 否则为。</span><span class="sxs-lookup"><span data-stu-id="a1735-2130">`true` if `x` is less than or equal to `y`, `false` otherwise</span></span>    | 
| `x >= y`      | <span data-ttu-id="a1735-2131">如果 `x` 大于或等于 `y``false`，则 `true`; 否则为。</span><span class="sxs-lookup"><span data-stu-id="a1735-2131">`true` if `x` is greater than or equal to `y`, `false` otherwise</span></span> | 

### <a name="integer-comparison-operators"></a><span data-ttu-id="a1735-2132">整数比较运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2132">Integer comparison operators</span></span>

<span data-ttu-id="a1735-2133">预定义的整数比较运算符是：</span><span class="sxs-lookup"><span data-stu-id="a1735-2133">The predefined integer comparison operators are:</span></span>
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

<span data-ttu-id="a1735-2134">其中每个运算符比较两个整数操作数的数值，并返回一个 `bool` 值，该值指示是否 `true` 或 `false`特定关系。</span><span class="sxs-lookup"><span data-stu-id="a1735-2134">Each of these operators compares the numeric values of the two integer operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span>

### <a name="floating-point-comparison-operators"></a><span data-ttu-id="a1735-2135">浮点比较运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2135">Floating-point comparison operators</span></span>

<span data-ttu-id="a1735-2136">预定义的浮点比较运算符是：</span><span class="sxs-lookup"><span data-stu-id="a1735-2136">The predefined floating-point comparison operators are:</span></span>
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

<span data-ttu-id="a1735-2137">运算符根据 IEEE 754 标准的规则对操作数进行比较：</span><span class="sxs-lookup"><span data-stu-id="a1735-2137">The operators compare the operands according to the rules of the IEEE 754 standard:</span></span>

*  <span data-ttu-id="a1735-2138">如果其中一个操作数为 NaN，则结果为 `true`的所有运算符（`!=`除外） `false`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2138">If either operand is NaN, the result is `false` for all operators except `!=`, for which the result is `true`.</span></span> <span data-ttu-id="a1735-2139">对于任意两个操作数，`x != y` 始终产生与 `!(x == y)`相同的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2139">For any two operands, `x != y` always produces the same result as `!(x == y)`.</span></span> <span data-ttu-id="a1735-2140">但是，当一个或两个操作数为 NaN 时，`<`、`>`、`<=`和 `>=` 运算符不会生成与相对运算符逻辑求反的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2140">However, when one or both operands are NaN, the `<`, `>`, `<=`, and `>=` operators do not produce the same results as the logical negation of the opposite operator.</span></span> <span data-ttu-id="a1735-2141">例如，如果 `x` 和 `y` 均为 NaN，则 `false``x < y`，但 `!(x >= y)` `true`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2141">For example, if either of `x` and `y` is NaN, then `x < y` is `false`, but `!(x >= y)` is `true`.</span></span>
*  <span data-ttu-id="a1735-2142">当两个操作数均为 NaN 时，运算符比较两个浮点操作数的值与排序相关</span><span class="sxs-lookup"><span data-stu-id="a1735-2142">When neither operand is NaN, the operators compare the values of the two floating-point operands with respect to the ordering</span></span>

   ```csharp
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   <span data-ttu-id="a1735-2143">其中 `min` 和 `max` 是可以用给定浮点格式表示的最小和最大的正有限值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2143">where `min` and `max` are the smallest and largest positive finite values that can be represented in the given floating-point format.</span></span> <span data-ttu-id="a1735-2144">此顺序的明显效果如下：</span><span class="sxs-lookup"><span data-stu-id="a1735-2144">Notable effects of this ordering are:</span></span>
   * <span data-ttu-id="a1735-2145">负零和正零被视为相等。</span><span class="sxs-lookup"><span data-stu-id="a1735-2145">Negative and positive zeros are considered equal.</span></span>
   * <span data-ttu-id="a1735-2146">负无穷被视为小于所有其他值，但等于其他负无穷。</span><span class="sxs-lookup"><span data-stu-id="a1735-2146">A negative infinity is considered less than all other values, but equal to another negative infinity.</span></span>
   * <span data-ttu-id="a1735-2147">正无穷视为大于所有其他值，但等于其他正无穷。</span><span class="sxs-lookup"><span data-stu-id="a1735-2147">A positive infinity is considered greater than all other values, but equal to another positive infinity.</span></span>

### <a name="decimal-comparison-operators"></a><span data-ttu-id="a1735-2148">小数比较运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2148">Decimal comparison operators</span></span>

<span data-ttu-id="a1735-2149">预定义的小数比较运算符是：</span><span class="sxs-lookup"><span data-stu-id="a1735-2149">The predefined decimal comparison operators are:</span></span>
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

<span data-ttu-id="a1735-2150">其中每个运算符比较两个 decimal 操作数的数值，并返回一个 `bool` 值，该值指示是否 `true` 或 `false`特定关系。</span><span class="sxs-lookup"><span data-stu-id="a1735-2150">Each of these operators compares the numeric values of the two decimal operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span> <span data-ttu-id="a1735-2151">每个 decimal 比较等效于使用 `System.Decimal`类型的对应关系或相等运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2151">Each decimal comparison is equivalent to using the corresponding relational or equality operator of type `System.Decimal`.</span></span>

### <a name="boolean-equality-operators"></a><span data-ttu-id="a1735-2152">布尔相等运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2152">Boolean equality operators</span></span>

<span data-ttu-id="a1735-2153">预定义的布尔相等运算符是：</span><span class="sxs-lookup"><span data-stu-id="a1735-2153">The predefined boolean equality operators are:</span></span>
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

<span data-ttu-id="a1735-2154">如果 `x` 和 `y` 均 `true`，则 `true` `==` 的结果，或者 `x` 和 `y` 都 `false`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2154">The result of `==` is `true` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="a1735-2155">否则，结果为 `false`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2155">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="a1735-2156">如果 `x` 和 `y` 均 `true`，则 `false` `!=` 的结果，或者 `x` 和 `y` 都 `false`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2156">The result of `!=` is `false` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="a1735-2157">否则，结果为 `true`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2157">Otherwise, the result is `true`.</span></span> <span data-ttu-id="a1735-2158">当操作数的类型为 `bool`时，`!=` 运算符产生与 `^` 运算符相同的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2158">When the operands are of type `bool`, the `!=` operator produces the same result as the `^` operator.</span></span>

### <a name="enumeration-comparison-operators"></a><span data-ttu-id="a1735-2159">枚举比较运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2159">Enumeration comparison operators</span></span>

<span data-ttu-id="a1735-2160">每个枚举类型都隐式提供以下预定义的比较运算符：</span><span class="sxs-lookup"><span data-stu-id="a1735-2160">Every enumeration type implicitly provides the following predefined comparison operators:</span></span>
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

<span data-ttu-id="a1735-2161">计算 `x op y`的结果，其中 `x` 和 `y` 是 `E` 基础类型为的枚举类型的表达式，`U`是比较运算符之一，与评估 `op` 完全相同。</span><span class="sxs-lookup"><span data-stu-id="a1735-2161">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the comparison operators, is exactly the same as evaluating `((U)x) op ((U)y)`.</span></span> <span data-ttu-id="a1735-2162">换言之，枚举类型比较运算符只是比较两个操作数的基础整数值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2162">In other words, the enumeration type comparison operators simply compare the underlying integral values of the two operands.</span></span>

### <a name="reference-type-equality-operators"></a><span data-ttu-id="a1735-2163">引用类型相等运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2163">Reference type equality operators</span></span>

<span data-ttu-id="a1735-2164">预定义的引用类型相等运算符是：</span><span class="sxs-lookup"><span data-stu-id="a1735-2164">The predefined reference type equality operators are:</span></span>
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

<span data-ttu-id="a1735-2165">运算符返回比较两个引用是否相等或不相等的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2165">The operators return the result of comparing the two references for equality or non-equality.</span></span>

<span data-ttu-id="a1735-2166">由于预定义的引用类型相等运算符接受 `object`类型的操作数，因此它们适用于未声明适用的 `operator ==` 和 `operator !=` 成员的所有类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2166">Since the predefined reference type equality operators accept operands of type `object`, they apply to all types that do not declare applicable `operator ==` and `operator !=` members.</span></span> <span data-ttu-id="a1735-2167">相反，任何适用的用户定义的相等运算符都可以有效地隐藏预定义的引用类型相等运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2167">Conversely, any applicable user-defined equality operators effectively hide the predefined reference type equality operators.</span></span>

<span data-ttu-id="a1735-2168">预定义的引用类型相等运算符需要以下项之一：</span><span class="sxs-lookup"><span data-stu-id="a1735-2168">The predefined reference type equality operators require one of the following:</span></span>

*  <span data-ttu-id="a1735-2169">两个操作数都是已知为*reference_type*或文本 `null`的类型的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2169">Both operands are a value of a type known to be a *reference_type* or the literal `null`.</span></span> <span data-ttu-id="a1735-2170">而且，显式引用转换（[显式引用转换](conversions.md#explicit-reference-conversions)）是从任一操作数的类型到另一个操作数的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2170">Furthermore, an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from the type of either operand to the type of the other operand.</span></span>
*  <span data-ttu-id="a1735-2171">一个操作数是类型 `T` 的值，其中 `T` 为*type_parameter* ，另一个操作数为文本 `null`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2171">One operand is a value of type `T` where `T` is a *type_parameter* and the other operand is the literal `null`.</span></span> <span data-ttu-id="a1735-2172">此外 `T` 没有值类型约束。</span><span class="sxs-lookup"><span data-stu-id="a1735-2172">Furthermore `T` does not have the value type constraint.</span></span>

<span data-ttu-id="a1735-2173">除非满足这些条件之一，否则将发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-2173">Unless one of these conditions are true, a binding-time error occurs.</span></span> <span data-ttu-id="a1735-2174">这些规则的明显含义是：</span><span class="sxs-lookup"><span data-stu-id="a1735-2174">Notable implications of these rules are:</span></span>

*  <span data-ttu-id="a1735-2175">使用预定义的引用类型相等运算符比较已知在绑定时不同的两个引用是绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-2175">It is a binding-time error to use the predefined reference type equality operators to compare two references that are known to be different at binding-time.</span></span> <span data-ttu-id="a1735-2176">例如，如果操作数的绑定时类型是 `A` 和 `B`的两个类类型，并且 `A` 和 `B` 都不是派生自另一个，则两个操作数不可能引用同一个对象。</span><span class="sxs-lookup"><span data-stu-id="a1735-2176">For example, if the binding-time types of the operands are two class types `A` and `B`, and if neither `A` nor `B` derives from the other, then it would be impossible for the two operands to reference the same object.</span></span> <span data-ttu-id="a1735-2177">因此，操作被视为绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-2177">Thus, the operation is considered a binding-time error.</span></span>
*  <span data-ttu-id="a1735-2178">预定义的引用类型相等运算符不允许比较值类型操作数。</span><span class="sxs-lookup"><span data-stu-id="a1735-2178">The predefined reference type equality operators do not permit value type operands to be compared.</span></span> <span data-ttu-id="a1735-2179">因此，除非结构类型声明自己的相等运算符，否则不能比较该结构类型的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2179">Therefore, unless a struct type declares its own equality operators, it is not possible to compare values of that struct type.</span></span>
*  <span data-ttu-id="a1735-2180">预定义的引用类型相等运算符永远不会导致其操作数发生装箱操作。</span><span class="sxs-lookup"><span data-stu-id="a1735-2180">The predefined reference type equality operators never cause boxing operations to occur for their operands.</span></span> <span data-ttu-id="a1735-2181">执行此类装箱操作会毫无意义，因为对新分配的装箱实例的引用必须与所有其他引用不同。</span><span class="sxs-lookup"><span data-stu-id="a1735-2181">It would be meaningless to perform such boxing operations, since references to the newly allocated boxed instances would necessarily differ from all other references.</span></span>
*  <span data-ttu-id="a1735-2182">如果将类型参数类型 `T` 的操作数与 `null`进行比较，并且 `T` 的运行时类型是值类型，则将 `false`比较的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2182">If an operand of a type parameter type `T` is compared to `null`, and the run-time type of `T` is a value type, the result of the comparison is `false`.</span></span>

<span data-ttu-id="a1735-2183">下面的示例检查是否 `null`不受约束的类型参数类型的参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-2183">The following example checks whether an argument of an unconstrained type parameter type is `null`.</span></span>
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

<span data-ttu-id="a1735-2184">即使 `T` 可以表示一个值类型，也允许 `x == null` 构造，并且仅当 `T` 为值类型时，才将结果定义为 `false`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2184">The `x == null` construct is permitted even though `T` could represent a value type, and the result is simply defined to be `false` when `T` is a value type.</span></span>

<span data-ttu-id="a1735-2185">对于 `x == y` 或 `x != y`形式的操作，如果存在任何适用的 `operator ==` 或 `operator !=`，运算符重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）规则将选择该运算符而不是预定义的引用类型相等运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2185">For an operation of the form `x == y` or `x != y`, if any applicable `operator ==` or `operator !=` exists, the operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) rules will select that operator instead of the predefined reference type equality operator.</span></span> <span data-ttu-id="a1735-2186">但是，始终可以通过将一个或两个操作数显式强制转换为类型 `object`来选择预定义的引用类型相等运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2186">However, it is always possible to select the predefined reference type equality operator by explicitly casting one or both of the operands to type `object`.</span></span> <span data-ttu-id="a1735-2187">示例</span><span class="sxs-lookup"><span data-stu-id="a1735-2187">The example</span></span>
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
<span data-ttu-id="a1735-2188">生成输出</span><span class="sxs-lookup"><span data-stu-id="a1735-2188">produces the output</span></span>
```console
True
False
False
False
```

<span data-ttu-id="a1735-2189">`s` 和 `t` 变量引用包含相同字符的两个不同的 `string` 实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-2189">The `s` and `t` variables refer to two distinct `string` instances containing the same characters.</span></span> <span data-ttu-id="a1735-2190">第一个比较输出 `True`，因为当两个操作数为类型 `string`时，将选择预定义的字符串相等运算符（[字符串相等运算符](expressions.md#string-equality-operators)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2190">The first comparison outputs `True` because the predefined string equality operator ([String equality operators](expressions.md#string-equality-operators)) is selected when both operands are of type `string`.</span></span> <span data-ttu-id="a1735-2191">其余的比较 `False` 输出，因为当两个操作数为类型 `object`时，将选择预定义的引用类型相等运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2191">The remaining comparisons all output `False` because the predefined reference type equality operator is selected when one or both of the operands are of type `object`.</span></span>

<span data-ttu-id="a1735-2192">请注意，上述方法对于值类型没有意义。</span><span class="sxs-lookup"><span data-stu-id="a1735-2192">Note that the above technique is not meaningful for value types.</span></span> <span data-ttu-id="a1735-2193">示例</span><span class="sxs-lookup"><span data-stu-id="a1735-2193">The example</span></span>
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
<span data-ttu-id="a1735-2194">输出 `False` 因为强制转换会创建对两个不同的装箱 `int` 值实例的引用。</span><span class="sxs-lookup"><span data-stu-id="a1735-2194">outputs `False` because the casts create references to two separate instances of boxed `int` values.</span></span>

### <a name="string-equality-operators"></a><span data-ttu-id="a1735-2195">字符串相等运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2195">String equality operators</span></span>

<span data-ttu-id="a1735-2196">预定义的字符串相等运算符是：</span><span class="sxs-lookup"><span data-stu-id="a1735-2196">The predefined string equality operators are:</span></span>
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

<span data-ttu-id="a1735-2197">如果满足以下任一条件，则将两个 `string` 值视为相等：</span><span class="sxs-lookup"><span data-stu-id="a1735-2197">Two `string` values are considered equal when one of the following is true:</span></span>

*  <span data-ttu-id="a1735-2198">这两个值都是 `null`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2198">Both values are `null`.</span></span>
*  <span data-ttu-id="a1735-2199">对于在每个字符位置都具有相同长度和相同字符的字符串实例，这两个值都是非空引用。</span><span class="sxs-lookup"><span data-stu-id="a1735-2199">Both values are non-null references to string instances that have identical lengths and identical characters in each character position.</span></span>

<span data-ttu-id="a1735-2200">字符串相等运算符比较字符串值而不是字符串引用。</span><span class="sxs-lookup"><span data-stu-id="a1735-2200">The string equality operators compare string values rather than string references.</span></span> <span data-ttu-id="a1735-2201">当两个单独的字符串实例包含完全相同的字符序列时，字符串的值相等，但引用不同。</span><span class="sxs-lookup"><span data-stu-id="a1735-2201">When two separate string instances contain the exact same sequence of characters, the values of the strings are equal, but the references are different.</span></span> <span data-ttu-id="a1735-2202">如[引用类型相等运算符](expressions.md#reference-type-equality-operators)中所述，引用类型相等运算符可用于比较字符串引用而不是字符串值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2202">As described in [Reference type equality operators](expressions.md#reference-type-equality-operators), the reference type equality operators can be used to compare string references instead of string values.</span></span>

### <a name="delegate-equality-operators"></a><span data-ttu-id="a1735-2203">委托相等运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2203">Delegate equality operators</span></span>

<span data-ttu-id="a1735-2204">每个委托类型都隐式提供以下预定义的比较运算符：</span><span class="sxs-lookup"><span data-stu-id="a1735-2204">Every delegate type implicitly provides the following predefined comparison operators:</span></span>

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

<span data-ttu-id="a1735-2205">两个委托实例被视为相等，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a1735-2205">Two delegate instances are considered equal as follows:</span></span>

*  <span data-ttu-id="a1735-2206">如果 `null`的任何一个委托实例，则当且仅当两者都 `null`时，它们才相等。</span><span class="sxs-lookup"><span data-stu-id="a1735-2206">If either of the delegate instances is `null`, they are equal if and only if both are `null`.</span></span>
*  <span data-ttu-id="a1735-2207">如果委托具有不同的运行时类型，则它们永远不相等。</span><span class="sxs-lookup"><span data-stu-id="a1735-2207">If the delegates have different run-time type they are never equal.</span></span>
*  <span data-ttu-id="a1735-2208">如果两个委托实例都具有调用列表（[委托声明](delegates.md#delegate-declarations)），则当且仅当其调用列表具有相同的长度，并且一个调用列表中的每一项都等于（如下所定义）到另一个调用列表中的相应项时，这些实例相等。</span><span class="sxs-lookup"><span data-stu-id="a1735-2208">If both of the delegate instances have an invocation list ([Delegate declarations](delegates.md#delegate-declarations)), those instances are equal if and only if their invocation lists are the same length, and each entry in one's invocation list is equal (as defined below) to the corresponding entry, in order, in the other's invocation list.</span></span>

<span data-ttu-id="a1735-2209">以下规则控制调用列表项的相等性：</span><span class="sxs-lookup"><span data-stu-id="a1735-2209">The following rules govern the equality of invocation list entries:</span></span>

*  <span data-ttu-id="a1735-2210">如果两个调用列表项引用相同的静态方法，则这些项相等。</span><span class="sxs-lookup"><span data-stu-id="a1735-2210">If two invocation list entries both refer to the same static method then the entries are equal.</span></span>
*  <span data-ttu-id="a1735-2211">如果两个调用列表项引用相同的目标对象上的相同非静态方法（由引用相等运算符定义），则这些项相等。</span><span class="sxs-lookup"><span data-stu-id="a1735-2211">If two invocation list entries both refer to the same non-static method on the same target object (as defined by the reference equality operators) then the entries are equal.</span></span>
*  <span data-ttu-id="a1735-2212">在语义上完全相同的*anonymous_method_expression*s 或*lambda_expression*s 的计算中，允许（但不要求）具有相同（可能为空）一组捕获的外部变量实例的调用列表项是相等的。</span><span class="sxs-lookup"><span data-stu-id="a1735-2212">Invocation list entries produced from evaluation of semantically identical *anonymous_method_expression*s or *lambda_expression*s with the same (possibly empty) set of captured outer variable instances are permitted (but not required) to be equal.</span></span>

### <a name="equality-operators-and-null"></a><span data-ttu-id="a1735-2213">相等运算符和 null</span><span class="sxs-lookup"><span data-stu-id="a1735-2213">Equality operators and null</span></span>

<span data-ttu-id="a1735-2214">`==` 和 `!=` 运算符允许一个操作数是可以为 null 的类型的值，另一个操作数是 `null` 文本，即使该操作没有预定义或用户定义的运算符（在 unlifted 或提升形式）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2214">The `==` and `!=` operators permit one operand to be a value of a nullable type and the other to be the `null` literal, even if no predefined or user-defined operator (in unlifted or lifted form) exists for the operation.</span></span>

<span data-ttu-id="a1735-2215">对于某个窗体的操作</span><span class="sxs-lookup"><span data-stu-id="a1735-2215">For an operation of one of the forms</span></span>
```csharp
x == null
null == x
x != null
null != x
```
<span data-ttu-id="a1735-2216">其中 `x` 是可以为 null 的类型的表达式，如果运算符重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）未能找到适用的运算符，则结果将改为从 `x`的 `HasValue` 属性进行计算。</span><span class="sxs-lookup"><span data-stu-id="a1735-2216">where `x` is an expression of a nullable type, if operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) fails to find an applicable operator, the result is instead computed from the `HasValue` property of `x`.</span></span> <span data-ttu-id="a1735-2217">具体而言，前两个窗体转换为 `!x.HasValue`，最后两个窗体转换为 `x.HasValue`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2217">Specifically, the first two forms are translated into `!x.HasValue`, and last two forms are translated into `x.HasValue`.</span></span>

### <a name="the-is-operator"></a><span data-ttu-id="a1735-2218">Is 运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2218">The is operator</span></span>

<span data-ttu-id="a1735-2219">`is` 运算符用于动态检查对象的运行时类型与给定类型是否兼容。</span><span class="sxs-lookup"><span data-stu-id="a1735-2219">The `is` operator is used to dynamically check if the run-time type of an object is compatible with a given type.</span></span> <span data-ttu-id="a1735-2220">操作的结果 `E is T`，其中 `E` 是一个表达式，而 `T` 是一个类型，它是一个布尔值，指示是否可以通过引用转换、装箱转换或取消装箱转换将 `E` 成功转换为类型 `T`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2220">The result of the operation `E is T`, where `E` is an expression and `T` is a type, is a boolean value indicating whether `E` can successfully be converted to type `T` by a reference conversion, a boxing conversion, or an unboxing conversion.</span></span> <span data-ttu-id="a1735-2221">在将类型参数替换为所有类型参数后，按如下所示计算运算：</span><span class="sxs-lookup"><span data-stu-id="a1735-2221">The operation is evaluated as follows, after type arguments have been substituted for all type parameters:</span></span>

*  <span data-ttu-id="a1735-2222">如果 `E` 是匿名函数，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-2222">If `E` is an anonymous function, a compile-time error occurs</span></span>
*  <span data-ttu-id="a1735-2223">如果 `E` 为方法组或 `null` 文本，则如果 `E` 的类型为引用类型或可以为 null 的类型并且 `E` 的值为 null，则结果为 false。</span><span class="sxs-lookup"><span data-stu-id="a1735-2223">If `E` is a method group or the `null` literal, of if the type of `E` is a reference type or a nullable type and the value of `E` is null, the result is false.</span></span>
*  <span data-ttu-id="a1735-2224">否则，让 `D` 表示 `E` 的动态类型，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a1735-2224">Otherwise, let `D` represent the dynamic type of `E` as follows:</span></span>
   * <span data-ttu-id="a1735-2225">如果 `E` 的类型为引用类型，则 `D` 是 `E`的实例引用的运行时类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2225">If the type of `E` is a reference type, `D` is the run-time type of the instance reference by `E`.</span></span>
   * <span data-ttu-id="a1735-2226">如果 `E` 的类型是可以为 null 的类型，则 `D` 是可以为 null 的类型的基础类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2226">If the type of `E` is a nullable type, `D` is the underlying type of that nullable type.</span></span>
   * <span data-ttu-id="a1735-2227">如果 `E` 的类型是不可以为 null 的值类型，则 `D` 是 `E`的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2227">If the type of `E` is a non-nullable value type, `D` is the type of `E`.</span></span>
*  <span data-ttu-id="a1735-2228">操作的结果取决于 `D` 和 `T`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a1735-2228">The result of the operation depends on `D` and `T` as follows:</span></span>
   * <span data-ttu-id="a1735-2229">如果 `T` 是引用类型，如果 `D` 和 `T` 为同一类型，则结果为 true; 如果 `D` 是引用类型，并且存在从 `D` 到 `T` 的隐式引用转换，或者 `D` 是一个值类型，并且 `D` 存在从 `T` 到的装箱转换。</span><span class="sxs-lookup"><span data-stu-id="a1735-2229">If `T` is a reference type, the result is true if `D` and `T` are the same type, if `D` is a reference type and an implicit reference conversion from `D` to `T` exists, or if `D` is a value type and a boxing conversion from `D` to `T` exists.</span></span>
   * <span data-ttu-id="a1735-2230">如果 `T` 是可以为 null 的类型，则如果 `D` 是 `T`的基础类型，则结果为 true。</span><span class="sxs-lookup"><span data-stu-id="a1735-2230">If `T` is a nullable type, the result is true if `D` is the underlying type of `T`.</span></span>
   * <span data-ttu-id="a1735-2231">如果 `T` 是不可以为 null 的值类型，则如果 `D` 和 `T` 为同一类型，则结果为 true。</span><span class="sxs-lookup"><span data-stu-id="a1735-2231">If `T` is a non-nullable value type, the result is true if `D` and `T` are the same type.</span></span>
   * <span data-ttu-id="a1735-2232">否则，结果为 false。</span><span class="sxs-lookup"><span data-stu-id="a1735-2232">Otherwise, the result is false.</span></span>

<span data-ttu-id="a1735-2233">请注意，`is` 运算符不考虑用户定义的转换。</span><span class="sxs-lookup"><span data-stu-id="a1735-2233">Note that user defined conversions, are not considered by the `is` operator.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="a1735-2234">As 运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2234">The as operator</span></span>

<span data-ttu-id="a1735-2235">`as` 运算符用于将值显式转换为给定引用类型或可以为 null 的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2235">The `as` operator is used to explicitly convert a value to a given reference type or nullable type.</span></span> <span data-ttu-id="a1735-2236">与强制转换表达式（[强制转换](expressions.md#cast-expressions)表达式）不同，`as` 运算符永远不会引发异常。</span><span class="sxs-lookup"><span data-stu-id="a1735-2236">Unlike a cast expression ([Cast expressions](expressions.md#cast-expressions)), the `as` operator never throws an exception.</span></span> <span data-ttu-id="a1735-2237">相反，如果不可能指定的转换，则将 `null`结果值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2237">Instead, if the indicated conversion is not possible, the resulting value is `null`.</span></span>

<span data-ttu-id="a1735-2238">在 `E as T`形式的操作中，`E` 必须是表达式，并且 `T` 必须是引用类型、已知为引用类型的类型参数或可以为 null 的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2238">In an operation of the form `E as T`, `E` must be an expression and `T` must be a reference type, a type parameter known to be a reference type, or a nullable type.</span></span> <span data-ttu-id="a1735-2239">此外，必须至少满足以下条件之一，否则将发生编译时错误：</span><span class="sxs-lookup"><span data-stu-id="a1735-2239">Furthermore, at least one of the following must be true, or otherwise a compile-time error occurs:</span></span>

*  <span data-ttu-id="a1735-2240">标识（[标识转换](conversions.md#identity-conversion)）、隐式引用（[隐式](conversions.md#implicit-nullable-conversions)[引用](conversions.md#implicit-reference-conversions)转换）、装箱（[装箱转换](conversions.md#boxing-conversions)）、显式可为 null （[显式](conversions.md#explicit-nullable-conversions)[引用](conversions.md#explicit-reference-conversions)转换）或取消装箱（[取消装箱转换](conversions.md#unboxing-conversions)）转换 `E` 到 `T`中。</span><span class="sxs-lookup"><span data-stu-id="a1735-2240">An identity ([Identity conversion](conversions.md#identity-conversion)), implicit nullable ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)), implicit reference ([Implicit reference conversions](conversions.md#implicit-reference-conversions)), boxing ([Boxing conversions](conversions.md#boxing-conversions)), explicit nullable ([Explicit nullable conversions](conversions.md#explicit-nullable-conversions)), explicit reference ([Explicit reference conversions](conversions.md#explicit-reference-conversions)), or unboxing ([Unboxing conversions](conversions.md#unboxing-conversions)) conversion exists from `E` to `T`.</span></span>
*  <span data-ttu-id="a1735-2241">`E` 类型或 `T` 是开放类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2241">The type of `E` or `T` is an open type.</span></span>
*  <span data-ttu-id="a1735-2242">`E` 是 `null` 文本。</span><span class="sxs-lookup"><span data-stu-id="a1735-2242">`E` is the `null` literal.</span></span>

<span data-ttu-id="a1735-2243">如果未 `dynamic``E` 的编译时类型，则操作 `E as T` 生成的结果与</span><span class="sxs-lookup"><span data-stu-id="a1735-2243">If the compile-time type of `E` is not `dynamic`, the operation `E as T` produces the same result as</span></span>
```csharp
E is T ? (T)(E) : (T)null
```
<span data-ttu-id="a1735-2244">不同的是 `E` 只计算一次。</span><span class="sxs-lookup"><span data-stu-id="a1735-2244">except that `E` is only evaluated once.</span></span> <span data-ttu-id="a1735-2245">编译器可以优化 `E as T` 最多执行一次动态类型检查，而不是上述扩展隐含的两种动态类型检查。</span><span class="sxs-lookup"><span data-stu-id="a1735-2245">The compiler can be expected to optimize `E as T` to perform at most one dynamic type check as opposed to the two dynamic type checks implied by the expansion above.</span></span>

<span data-ttu-id="a1735-2246">如果 `dynamic``E` 的编译时类型，则与强制转换运算符不同，`as` 运算符不会动态绑定（[动态绑定](expressions.md#dynamic-binding)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2246">If the compile-time type of `E` is `dynamic`, unlike the cast operator the `as` operator is not dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="a1735-2247">因此，在这种情况下，展开是：</span><span class="sxs-lookup"><span data-stu-id="a1735-2247">Therefore the expansion in this case is:</span></span>
```csharp
E is T ? (T)(object)(E) : (T)null
```

<span data-ttu-id="a1735-2248">请注意，某些转换（例如用户定义的转换）无法与 `as` 运算符一起使用，应改为使用强制转换表达式执行。</span><span class="sxs-lookup"><span data-stu-id="a1735-2248">Note that some conversions, such as user defined conversions, are not possible with the `as` operator and should instead be performed using cast expressions.</span></span>

<span data-ttu-id="a1735-2249">示例中</span><span class="sxs-lookup"><span data-stu-id="a1735-2249">In the example</span></span>
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
<span data-ttu-id="a1735-2250">`G` 的类型参数 `T` 称为引用类型，因为它具有类约束。</span><span class="sxs-lookup"><span data-stu-id="a1735-2250">the type parameter `T` of `G` is known to be a reference type, because it has the class constraint.</span></span> <span data-ttu-id="a1735-2251">但 `H` 的类型参数 `U` 不是;因此，不允许在 `H` 中使用 `as` 运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2251">The type parameter `U` of `H` is not however; hence the use of the `as` operator in `H` is disallowed.</span></span>

## <a name="logical-operators"></a><span data-ttu-id="a1735-2252">逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2252">Logical operators</span></span>

<span data-ttu-id="a1735-2253">`&`、`^`和 `|` 运算符称为逻辑运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2253">The `&`, `^`, and `|` operators are called the logical operators.</span></span>

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

<span data-ttu-id="a1735-2254">如果逻辑运算符的操作数具有编译时类型 `dynamic`，则该表达式将动态绑定（[动态绑定](expressions.md#dynamic-binding)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2254">If an operand of a logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="a1735-2255">在这种情况下，将 `dynamic`表达式的编译时类型，并将使用编译时类型为 `dynamic`的操作数的运行时类型在运行时执行下面描述的解决方法。</span><span class="sxs-lookup"><span data-stu-id="a1735-2255">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="a1735-2256">对于 `x op y`形式的操作，其中 `op` 为逻辑运算符之一，将应用重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）来选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-2256">For an operation of the form `x op y`, where `op` is one of the logical operators, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="a1735-2257">操作数将转换为所选运算符的参数类型，结果的类型就是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2257">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="a1735-2258">以下各节介绍了预定义的逻辑运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2258">The predefined logical operators are described in the following sections.</span></span>

### <a name="integer-logical-operators"></a><span data-ttu-id="a1735-2259">整数逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2259">Integer logical operators</span></span>

<span data-ttu-id="a1735-2260">预定义的整数逻辑运算符为：</span><span class="sxs-lookup"><span data-stu-id="a1735-2260">The predefined integer logical operators are:</span></span>
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

<span data-ttu-id="a1735-2261">`&` 运算符计算两个操作数的按位逻辑 `AND`，`|` 运算符计算两个操作数的按位逻辑 `OR`，而 `^` 运算符计算两个操作数的按位逻辑独占 `OR`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2261">The `&` operator computes the bitwise logical `AND` of the two operands, the `|` operator computes the bitwise logical `OR` of the two operands, and the `^` operator computes the bitwise logical exclusive `OR` of the two operands.</span></span> <span data-ttu-id="a1735-2262">这些操作不能有溢出。</span><span class="sxs-lookup"><span data-stu-id="a1735-2262">No overflows are possible from these operations.</span></span>

### <a name="enumeration-logical-operators"></a><span data-ttu-id="a1735-2263">枚举逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2263">Enumeration logical operators</span></span>

<span data-ttu-id="a1735-2264">每个枚举类型 `E` 隐式提供以下预定义的逻辑运算符：</span><span class="sxs-lookup"><span data-stu-id="a1735-2264">Every enumeration type `E` implicitly provides the following predefined logical operators:</span></span>

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

<span data-ttu-id="a1735-2265">计算 `x op y`的结果，其中 `x` 和 `y` 是 `E` 基础类型为的枚举类型的表达式，`U`是逻辑运算符之一，与评估 `op` 完全相同。</span><span class="sxs-lookup"><span data-stu-id="a1735-2265">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the logical operators, is exactly the same as evaluating `(E)((U)x op (U)y)`.</span></span> <span data-ttu-id="a1735-2266">换言之，枚举类型逻辑运算符只对两个操作数的基础类型执行逻辑运算。</span><span class="sxs-lookup"><span data-stu-id="a1735-2266">In other words, the enumeration type logical operators simply perform the logical operation on the underlying type of the two operands.</span></span>

### <a name="boolean-logical-operators"></a><span data-ttu-id="a1735-2267">Boolean 逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2267">Boolean logical operators</span></span>

<span data-ttu-id="a1735-2268">预定义的布尔逻辑运算符为：</span><span class="sxs-lookup"><span data-stu-id="a1735-2268">The predefined boolean logical operators are:</span></span>
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

<span data-ttu-id="a1735-2269">如果 `x` 和 `y` 都为 `true`，则 `x & y` 的结果为 `true`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2269">The result of `x & y` is `true` if both `x` and `y` are `true`.</span></span> <span data-ttu-id="a1735-2270">否则，结果为 `false`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2270">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="a1735-2271">如果 `x` 或 `y` 为 `true`，则 `true` `x | y` 的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2271">The result of `x | y` is `true` if either `x` or `y` is `true`.</span></span> <span data-ttu-id="a1735-2272">否则，结果为 `false`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2272">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="a1735-2273">如果 `x` 为 `true`，`y` 为 `false`，或 `x` `false` `y` `true`，则会 `true` `x ^ y` 的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2273">The result of `x ^ y` is `true` if `x` is `true` and `y` is `false`, or `x` is `false` and `y` is `true`.</span></span> <span data-ttu-id="a1735-2274">否则，结果为 `false`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2274">Otherwise, the result is `false`.</span></span> <span data-ttu-id="a1735-2275">当操作数为类型 `bool`时，`^` 运算符将计算与 `!=` 运算符相同的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2275">When the operands are of type `bool`, the `^` operator computes the same result as the `!=` operator.</span></span>

### <a name="nullable-boolean-logical-operators"></a><span data-ttu-id="a1735-2276">可以为 null 的布尔逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2276">Nullable boolean logical operators</span></span>

<span data-ttu-id="a1735-2277">可以为 null 的布尔类型 `bool?` 可以表示三个值： `true`、`false`和 `null`，在概念上类似于用于 SQL 中的布尔表达式的三值类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2277">The nullable boolean type `bool?` can represent three values, `true`, `false`, and `null`, and is conceptually similar to the three-valued type used for boolean expressions in SQL.</span></span> <span data-ttu-id="a1735-2278">为了确保 `bool?` 操作数的 `&` 和 `|` 运算符产生的结果与 SQL 的三值逻辑一致，提供了以下预定义运算符：</span><span class="sxs-lookup"><span data-stu-id="a1735-2278">To ensure that the results produced by the `&` and `|` operators for `bool?` operands are consistent with SQL's three-valued logic, the following predefined operators are provided:</span></span>

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

<span data-ttu-id="a1735-2279">下表列出了这些运算符为 `true`、`false`和 `null`的所有值组合所产生的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2279">The following table lists the results produced by these operators for all combinations of the values `true`, `false`, and `null`.</span></span>

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

## <a name="conditional-logical-operators"></a><span data-ttu-id="a1735-2280">条件逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2280">Conditional logical operators</span></span>

<span data-ttu-id="a1735-2281">`&&` 和 `||` 运算符称为条件逻辑运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2281">The `&&` and `||` operators are called the conditional logical operators.</span></span> <span data-ttu-id="a1735-2282">它们也称为 "短路" 逻辑运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2282">They are also called the "short-circuiting" logical operators.</span></span>

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

<span data-ttu-id="a1735-2283">`&&` 和 `||` 运算符是 `&` 和 `|` 运算符的条件版本：</span><span class="sxs-lookup"><span data-stu-id="a1735-2283">The `&&` and `||` operators are conditional versions of the `&` and `|` operators:</span></span>

*  <span data-ttu-id="a1735-2284">操作 `x && y` 对应于操作 `x & y`，只不过仅当 `x` 未 `false`时才会计算 `y`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2284">The operation `x && y` corresponds to the operation `x & y`, except that `y` is evaluated only if `x` is not `false`.</span></span>
*  <span data-ttu-id="a1735-2285">操作 `x || y` 对应于操作 `x | y`，只不过仅当 `x` 未 `true`时才会计算 `y`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2285">The operation `x || y` corresponds to the operation `x | y`, except that `y` is evaluated only if `x` is not `true`.</span></span>

<span data-ttu-id="a1735-2286">如果条件逻辑运算符的操作数具有编译时类型 `dynamic`，则该表达式将动态绑定（[动态绑定](expressions.md#dynamic-binding)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2286">If an operand of a conditional logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="a1735-2287">在这种情况下，将 `dynamic`表达式的编译时类型，并将使用编译时类型为 `dynamic`的操作数的运行时类型在运行时执行下面描述的解决方法。</span><span class="sxs-lookup"><span data-stu-id="a1735-2287">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="a1735-2288">`x && y` 或 `x || y` 形式的操作通过应用重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）来处理，就像该操作是 `x & y` 还是 `x | y`编写的。</span><span class="sxs-lookup"><span data-stu-id="a1735-2288">An operation of the form `x && y` or `x || y` is processed by applying overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x & y` or `x | y`.</span></span> <span data-ttu-id="a1735-2289">那么：</span><span class="sxs-lookup"><span data-stu-id="a1735-2289">Then,</span></span>

*  <span data-ttu-id="a1735-2290">如果重载决策未能找到单个最佳运算符，或者重载决策选择预定义的整数逻辑运算符之一，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-2290">If overload resolution fails to find a single best operator, or if overload resolution selects one of the predefined integer logical operators, a binding-time error occurs.</span></span>
*  <span data-ttu-id="a1735-2291">否则，如果所选运算符是预定义的布尔逻辑运算符之一（[布尔逻辑运算符](expressions.md#boolean-logical-operators)）或可以为 null 的布尔逻辑运算符（[可为 null](expressions.md#nullable-boolean-logical-operators)的布尔逻辑运算符），则按照[布尔条件逻辑运算符](expressions.md#boolean-conditional-logical-operators)中所述处理操作。</span><span class="sxs-lookup"><span data-stu-id="a1735-2291">Otherwise, if the selected operator is one of the predefined boolean logical operators ([Boolean logical operators](expressions.md#boolean-logical-operators)) or nullable boolean logical operators ([Nullable boolean logical operators](expressions.md#nullable-boolean-logical-operators)), the operation is processed as described in [Boolean conditional logical operators](expressions.md#boolean-conditional-logical-operators).</span></span>
*  <span data-ttu-id="a1735-2292">否则，所选运算符是用户定义的运算符，并且按照[用户定义的条件逻辑运算符](expressions.md#user-defined-conditional-logical-operators)中的描述处理操作。</span><span class="sxs-lookup"><span data-stu-id="a1735-2292">Otherwise, the selected operator is a user-defined operator, and the operation is processed as described in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

<span data-ttu-id="a1735-2293">不能直接重载条件逻辑运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2293">It is not possible to directly overload the conditional logical operators.</span></span> <span data-ttu-id="a1735-2294">但是，因为条件逻辑运算符是按常规逻辑运算符计算的，所以常规逻辑运算符的重载在某些限制条件下也被视为条件逻辑运算符的重载。</span><span class="sxs-lookup"><span data-stu-id="a1735-2294">However, because the conditional logical operators are evaluated in terms of the regular logical operators, overloads of the regular logical operators are, with certain restrictions, also considered overloads of the conditional logical operators.</span></span> <span data-ttu-id="a1735-2295">[用户定义的条件逻辑运算符](expressions.md#user-defined-conditional-logical-operators)中对此进行了进一步说明。</span><span class="sxs-lookup"><span data-stu-id="a1735-2295">This is described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

### <a name="boolean-conditional-logical-operators"></a><span data-ttu-id="a1735-2296">布尔条件逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2296">Boolean conditional logical operators</span></span>

<span data-ttu-id="a1735-2297">如果 `&&` 或 `||` 的操作数的类型为 `bool`，或者当操作数的类型没有定义适用的 `operator &` 或 `operator |`，但确实定义了到 `bool`的隐式转换时，将按如下所示处理操作：</span><span class="sxs-lookup"><span data-stu-id="a1735-2297">When the operands of `&&` or `||` are of type `bool`, or when the operands are of types that do not define an applicable `operator &` or `operator |`, but do define implicit conversions to `bool`, the operation is processed as follows:</span></span>

*  <span data-ttu-id="a1735-2298">运算 `x && y` 的计算结果为 `x ? y : false`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2298">The operation `x && y` is evaluated as `x ? y : false`.</span></span> <span data-ttu-id="a1735-2299">换句话说，`x` 首先计算并转换为类型 `bool`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2299">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="a1735-2300">然后，如果 `true``x`，则计算 `y`，并将其转换为类型 `bool`，这就成为了运算结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2300">Then, if `x` is `true`, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span> <span data-ttu-id="a1735-2301">否则，将 `false`操作的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2301">Otherwise, the result of the operation is `false`.</span></span>
*  <span data-ttu-id="a1735-2302">运算 `x || y` 的计算结果为 `x ? true : y`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2302">The operation `x || y` is evaluated as `x ? true : y`.</span></span> <span data-ttu-id="a1735-2303">换句话说，`x` 首先计算并转换为类型 `bool`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2303">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="a1735-2304">然后，如果 `true``x`，则将 `true`操作的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2304">Then, if `x` is `true`, the result of the operation is `true`.</span></span> <span data-ttu-id="a1735-2305">否则，将计算 `y`，并将其转换为类型 `bool`，这就成为了运算结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2305">Otherwise, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span>

### <a name="user-defined-conditional-logical-operators"></a><span data-ttu-id="a1735-2306">用户定义的条件逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2306">User-defined conditional logical operators</span></span>

<span data-ttu-id="a1735-2307">如果 `&&` 或 `||` 的操作数是声明适用的用户定义 `operator &` 或 `operator |`的类型，则以下两项都必须为 true，其中 `T` 是声明所选运算符的类型：</span><span class="sxs-lookup"><span data-stu-id="a1735-2307">When the operands of `&&` or `||` are of types that declare an applicable user-defined `operator &` or `operator |`, both of the following must be true, where `T` is the type in which the selected operator is declared:</span></span>

*  <span data-ttu-id="a1735-2308">所选运算符的每个参数的返回类型和类型必须是 `T`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2308">The return type and the type of each parameter of the selected operator must be `T`.</span></span> <span data-ttu-id="a1735-2309">换言之，运算符必须计算 `T`类型的两个操作数的逻辑 `AND` 或逻辑 `OR`，并且必须返回 `T`类型的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2309">In other words, the operator must compute the logical `AND` or the logical `OR` of two operands of type `T`, and must return a result of type `T`.</span></span>
*  <span data-ttu-id="a1735-2310">`T` 必须包含 `operator true` 和 `operator false`的声明。</span><span class="sxs-lookup"><span data-stu-id="a1735-2310">`T` must contain declarations of `operator true` and `operator false`.</span></span>

<span data-ttu-id="a1735-2311">如果不满足上述任一要求，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-2311">A binding-time error occurs if either of these requirements is not satisfied.</span></span> <span data-ttu-id="a1735-2312">否则，通过将用户定义的 `operator true` 或 `operator false` 与选定的用户定义运算符相结合来计算 `&&` 或 `||` 操作：</span><span class="sxs-lookup"><span data-stu-id="a1735-2312">Otherwise, the `&&` or `||` operation is evaluated by combining the user-defined `operator true` or `operator false` with the selected user-defined operator:</span></span>

*  <span data-ttu-id="a1735-2313">运算 `x && y` 的计算结果为 `T.false(x) ? x : T.&(x, y)`，其中 `T.false(x)` 是在 `T`中声明的 `operator false` 的调用，`T.&(x, y)` 是所选 `operator &`的调用。</span><span class="sxs-lookup"><span data-stu-id="a1735-2313">The operation `x && y` is evaluated as `T.false(x) ? x : T.&(x, y)`, where `T.false(x)` is an invocation of the `operator false` declared in `T`, and `T.&(x, y)` is an invocation of the selected `operator &`.</span></span> <span data-ttu-id="a1735-2314">换而言之，首先计算 `x`，并对结果调用 `operator false`，以确定 `x` 是否确实为 false。</span><span class="sxs-lookup"><span data-stu-id="a1735-2314">In other words, `x` is first evaluated and `operator false` is invoked on the result to determine if `x` is definitely false.</span></span> <span data-ttu-id="a1735-2315">然后，如果 `x` 肯定为 false，则操作的结果是之前为 `x`计算的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2315">Then, if `x` is definitely false, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="a1735-2316">否则，将对 `y` 进行计算，并对之前为 `x` 计算的值调用所选 `operator &`，并为 `y` 生成操作结果的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2316">Otherwise, `y` is evaluated, and the selected `operator &` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>
*  <span data-ttu-id="a1735-2317">运算 `x || y` 的计算结果为 `T.true(x) ? x : T.|(x, y)`，其中 `T.true(x)` 是在 `T`中声明的 `operator true` 的调用，`T.|(x,y)` 是所选 `operator|`的调用。</span><span class="sxs-lookup"><span data-stu-id="a1735-2317">The operation `x || y` is evaluated as `T.true(x) ? x : T.|(x, y)`, where `T.true(x)` is an invocation of the `operator true` declared in `T`, and `T.|(x,y)` is an invocation of the selected `operator|`.</span></span> <span data-ttu-id="a1735-2318">换言之，首先计算 `x` 并对结果调用 `operator true`，以确定 `x` 是否确实为 true。</span><span class="sxs-lookup"><span data-stu-id="a1735-2318">In other words, `x` is first evaluated and `operator true` is invoked on the result to determine if `x` is definitely true.</span></span> <span data-ttu-id="a1735-2319">然后，如果 `x` 确实为 true，则操作的结果是之前为 `x`计算的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2319">Then, if `x` is definitely true, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="a1735-2320">否则，将对 `y` 进行计算，并对之前为 `x` 计算的值调用所选 `operator |`，并为 `y` 生成操作结果的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2320">Otherwise, `y` is evaluated, and the selected `operator |` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>

<span data-ttu-id="a1735-2321">在上述任一操作中，由 `x` 给定的表达式只计算一次，并且 `y` 指定的表达式不会进行任何计算或只计算一次。</span><span class="sxs-lookup"><span data-stu-id="a1735-2321">In either of these operations, the expression given by `x` is only evaluated once, and the expression given by `y` is either not evaluated or evaluated exactly once.</span></span>

<span data-ttu-id="a1735-2322">有关实现 `operator true` 和 `operator false`的类型的示例，请参阅[数据库布尔类型](structs.md#database-boolean-type)。</span><span class="sxs-lookup"><span data-stu-id="a1735-2322">For an example of a type that implements `operator true` and `operator false`, see [Database boolean type](structs.md#database-boolean-type).</span></span>

## <a name="the-null-coalescing-operator"></a><span data-ttu-id="a1735-2323">Null 合并运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2323">The null coalescing operator</span></span>

<span data-ttu-id="a1735-2324">`??` 运算符称为 null 合并运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2324">The `??` operator is called the null coalescing operator.</span></span>

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

<span data-ttu-id="a1735-2325">格式 `a ?? b` 的 null 合并表达式需要 `a` 为可以为 null 的类型或引用类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2325">A null coalescing expression of the form `a ?? b` requires `a` to be of a nullable type or reference type.</span></span> <span data-ttu-id="a1735-2326">如果 `a` 为非 null，则 `a ?? b` 的结果 `a`;否则，结果为 `b`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2326">If `a` is non-null, the result of `a ?? b` is `a`; otherwise, the result is `b`.</span></span> <span data-ttu-id="a1735-2327">仅当 `a` 为 null 时，操作才计算 `b`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2327">The operation evaluates `b` only if `a` is null.</span></span>

<span data-ttu-id="a1735-2328">Null 合并运算符是右结合运算符，这意味着运算从右到左分组。</span><span class="sxs-lookup"><span data-stu-id="a1735-2328">The null coalescing operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="a1735-2329">例如，形式 `a ?? b ?? c` 的表达式的计算结果为 `a ?? (b ?? c)`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2329">For example, an expression of the form `a ?? b ?? c` is evaluated as `a ?? (b ?? c)`.</span></span> <span data-ttu-id="a1735-2330">一般来说，形式 `E1 ?? E2 ?? ... ?? En` 的表达式返回非 null 的第一个操作数，如果所有操作数为 null，则返回 null。</span><span class="sxs-lookup"><span data-stu-id="a1735-2330">In general terms, an expression of the form `E1 ?? E2 ?? ... ?? En` returns the first of the operands that is non-null, or null if all operands are null.</span></span>

<span data-ttu-id="a1735-2331">`a ?? b` 表达式的类型取决于在操作数上可用的隐式转换。</span><span class="sxs-lookup"><span data-stu-id="a1735-2331">The type of the expression `a ?? b` depends on which implicit conversions are available on the operands.</span></span> <span data-ttu-id="a1735-2332">按照优先顺序，`a ?? b` 的类型为 `A0`、`A`或 `B`，其中 `A` 是 `a` 的类型（前提是 `a` 具有类型），`B` 是 `b` 的类型（前提是 `b` 有一个类型），`A0` 是 `A` 的基础类型（如果 `A` 是可以为 null 的类型），或者 `A` 为。</span><span class="sxs-lookup"><span data-stu-id="a1735-2332">In order of preference, the type of `a ?? b` is `A0`, `A`, or `B`, where `A` is the type of `a` (provided that `a` has a type), `B` is the type of `b` (provided that `b` has a type), and `A0` is the underlying type of `A` if `A` is a nullable type, or `A` otherwise.</span></span> <span data-ttu-id="a1735-2333">具体而言，按如下所示处理 `a ?? b`：</span><span class="sxs-lookup"><span data-stu-id="a1735-2333">Specifically, `a ?? b` is processed as follows:</span></span>

*  <span data-ttu-id="a1735-2334">如果 `A` 存在，并且不是可以为 null 的类型或引用类型，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-2334">If `A` exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>
*  <span data-ttu-id="a1735-2335">如果 `b` 是动态表达式，则结果类型为 `dynamic`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2335">If `b` is a dynamic expression, the result type is `dynamic`.</span></span> <span data-ttu-id="a1735-2336">在运行时，首先计算 `a`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2336">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="a1735-2337">如果 `a` 不为 null，则 `a` 转换为动态，这将成为结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2337">If `a` is not null, `a` is converted to dynamic, and this becomes the result.</span></span> <span data-ttu-id="a1735-2338">否则，将计算 `b`，这就是结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2338">Otherwise, `b` is evaluated, and this becomes the result.</span></span>
*  <span data-ttu-id="a1735-2339">否则，如果 `A` 存在并且是可以为 null 的类型并且存在从 `b` 到 `A0`的隐式转换，则结果类型为 `A0`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2339">Otherwise, if `A` exists and is a nullable type and an implicit conversion exists from `b` to `A0`, the result type is `A0`.</span></span> <span data-ttu-id="a1735-2340">在运行时，首先计算 `a`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2340">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="a1735-2341">如果 `a` 不为 null，`a` 将解包为类型 `A0`，这就成为了结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2341">If `a` is not null, `a` is unwrapped to type `A0`, and this becomes the result.</span></span> <span data-ttu-id="a1735-2342">否则，将计算 `b`，并将其转换为类型 `A0`，这就成为了结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2342">Otherwise, `b` is evaluated and converted to type `A0`, and this becomes the result.</span></span>
*  <span data-ttu-id="a1735-2343">否则，如果 `A` 存在，而从 `b` 到 `A`存在隐式转换，则结果类型为 `A`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2343">Otherwise, if `A` exists and an implicit conversion exists from `b` to `A`, the result type is `A`.</span></span> <span data-ttu-id="a1735-2344">在运行时，首先计算 `a`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2344">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="a1735-2345">如果 `a` 不为 null，则 `a` 会成为结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2345">If `a` is not null, `a` becomes the result.</span></span> <span data-ttu-id="a1735-2346">否则，将计算 `b`，并将其转换为类型 `A`，这就成为了结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2346">Otherwise, `b` is evaluated and converted to type `A`, and this becomes the result.</span></span>
*  <span data-ttu-id="a1735-2347">否则，如果 `b` 具有类型 `B` 并且存在从 `a` 到 `B`的隐式转换，则结果类型为 `B`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2347">Otherwise, if `b` has a type `B` and an implicit conversion exists from `a` to `B`, the result type is `B`.</span></span> <span data-ttu-id="a1735-2348">在运行时，首先计算 `a`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2348">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="a1735-2349">如果 `a` 不为 null，则 `a` 将解包到类型 `A0` （如果 `A` 存在并且可为 null），并转换为类型 `B`，这就成为了结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2349">If `a` is not null, `a` is unwrapped to type `A0` (if `A` exists and is nullable) and converted to type `B`, and this becomes the result.</span></span> <span data-ttu-id="a1735-2350">否则，将计算 `b` 并使其成为结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2350">Otherwise, `b` is evaluated and becomes the result.</span></span>
*  <span data-ttu-id="a1735-2351">否则，`a` 和 `b` 不兼容，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-2351">Otherwise, `a` and `b` are incompatible, and a compile-time error occurs.</span></span>

## <a name="conditional-operator"></a><span data-ttu-id="a1735-2352">条件运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2352">Conditional operator</span></span>

<span data-ttu-id="a1735-2353">`?:` 运算符称为条件运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2353">The `?:` operator is called the conditional operator.</span></span> <span data-ttu-id="a1735-2354">有时也称为三元运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2354">It is at times also called the ternary operator.</span></span>

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

<span data-ttu-id="a1735-2355">`b ? x : y` 首先计算 `b`的条件表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-2355">A conditional expression of the form `b ? x : y` first evaluates the condition `b`.</span></span> <span data-ttu-id="a1735-2356">然后，如果 `true``b`，则将计算 `x`，并成为操作的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2356">Then, if `b` is `true`, `x` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="a1735-2357">否则，将计算 `y`，并成为操作的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2357">Otherwise, `y` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="a1735-2358">条件表达式从不同时计算 `x` 和 `y`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2358">A conditional expression never evaluates both `x` and `y`.</span></span>

<span data-ttu-id="a1735-2359">条件运算符是右结合运算符，这意味着运算从右到左分组。</span><span class="sxs-lookup"><span data-stu-id="a1735-2359">The conditional operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="a1735-2360">例如，形式 `a ? b : c ? d : e` 的表达式的计算结果为 `a ? b : (c ? d : e)`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2360">For example, an expression of the form `a ? b : c ? d : e` is evaluated as `a ? b : (c ? d : e)`.</span></span>

<span data-ttu-id="a1735-2361">`?:` 运算符的第一个操作数必须是可以隐式转换为 `bool`的表达式，或者是实现 `operator true`的类型的表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-2361">The first operand of the `?:` operator must be an expression that can be implicitly converted to `bool`, or an expression of a type that implements `operator true`.</span></span> <span data-ttu-id="a1735-2362">如果满足这两个要求，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-2362">If neither of these requirements is satisfied, a compile-time error occurs.</span></span>

<span data-ttu-id="a1735-2363">`?:` 运算符的第二个和第三个操作数 `x` 和 `y`控制条件表达式的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2363">The second and third operands, `x` and `y`, of the `?:` operator control the type of the conditional expression.</span></span>

*  <span data-ttu-id="a1735-2364">如果 `x` 具有类型 `X` 并且 `y` 具有类型 `Y` 则</span><span class="sxs-lookup"><span data-stu-id="a1735-2364">If `x` has type `X` and `y` has type `Y` then</span></span>
   * <span data-ttu-id="a1735-2365">如果 `X` 中存在隐式转换（[隐式转换](conversions.md#implicit-conversions)）到 `Y`，但不是从 `Y` 到 `X`，则 `Y` 是条件表达式的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2365">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `X` to `Y`, but not from `Y` to `X`, then `Y` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="a1735-2366">如果 `Y` 中存在隐式转换（[隐式转换](conversions.md#implicit-conversions)）到 `X`，但不是从 `X` 到 `Y`，则 `X` 是条件表达式的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2366">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `Y` to `X`, but not from `X` to `Y`, then `X` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="a1735-2367">否则，不能确定表达式类型，并且会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-2367">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="a1735-2368">如果 `x` 和 `y` 中只有一个具有类型，并且的 `x` 和 `y`都可以隐式转换为该类型，则这是条件表达式的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2368">If only one of `x` and `y` has a type, and both `x` and `y`, of are implicitly convertible to that type, then that is the type of the conditional expression.</span></span>
*  <span data-ttu-id="a1735-2369">否则，不能确定表达式类型，并且会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-2369">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>

<span data-ttu-id="a1735-2370">格式 `b ? x : y` 的条件表达式的运行时处理包括以下步骤：</span><span class="sxs-lookup"><span data-stu-id="a1735-2370">The run-time processing of a conditional expression of the form `b ? x : y` consists of the following steps:</span></span>

*  <span data-ttu-id="a1735-2371">首先，将计算 `b`，并确定 `b` 的 `bool` 值：</span><span class="sxs-lookup"><span data-stu-id="a1735-2371">First, `b` is evaluated, and the `bool` value of `b` is determined:</span></span>
   * <span data-ttu-id="a1735-2372">如果从 `b` 类型到 `bool` 存在隐式转换，则执行此隐式转换以生成 `bool` 值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2372">If an implicit conversion from the type of `b` to `bool` exists, then this implicit conversion is performed to produce a `bool` value.</span></span>
   * <span data-ttu-id="a1735-2373">否则，将调用 `b` 类型定义的 `operator true` 以生成 `bool` 值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2373">Otherwise, the `operator true` defined by the type of `b` is invoked to produce a `bool` value.</span></span>
*  <span data-ttu-id="a1735-2374">如果上面的步骤生成的 `bool` 值为 `true`，则计算 `x`，并将其转换为条件表达式的类型，这就成为条件表达式的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2374">If the `bool` value produced by the step above is `true`, then `x` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>
*  <span data-ttu-id="a1735-2375">否则，将计算 `y`，并将其转换为条件表达式的类型，这将成为条件表达式的结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2375">Otherwise, `y` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>

## <a name="anonymous-function-expressions"></a><span data-ttu-id="a1735-2376">匿名函数表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-2376">Anonymous function expressions</span></span>

<span data-ttu-id="a1735-2377">***匿名函数***是表示 "内联" 方法定义的表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-2377">An ***anonymous function*** is an expression that represents an "in-line" method definition.</span></span> <span data-ttu-id="a1735-2378">匿名函数本身不具有值或类型，但可以转换为兼容的委托或表达式树类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2378">An anonymous function does not have a value or type in and of itself, but is convertible to a compatible delegate or expression tree type.</span></span> <span data-ttu-id="a1735-2379">匿名函数转换的计算取决于转换的目标类型：如果该类型为委托类型，则转换将计算为引用匿名函数定义的方法的委托值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2379">The evaluation of an anonymous function conversion depends on the target type of the conversion: If it is a delegate type, the conversion evaluates to a delegate value referencing the method which the anonymous function defines.</span></span> <span data-ttu-id="a1735-2380">如果该类型为表达式树类型，则转换为表达式树，该树将方法的结构表示为对象结构。</span><span class="sxs-lookup"><span data-stu-id="a1735-2380">If it is an expression tree type, the conversion evaluates to an expression tree which represents the structure of the method as an object structure.</span></span>

<span data-ttu-id="a1735-2381">由于历史原因，匿名函数有两种语法风格，即*lambda_expression*s 和*anonymous_method_expression*s。</span><span class="sxs-lookup"><span data-stu-id="a1735-2381">For historical reasons there are two syntactic flavors of anonymous functions, namely *lambda_expression*s and *anonymous_method_expression*s.</span></span> <span data-ttu-id="a1735-2382">几乎所有情况下， *lambda_expression*比*anonymous_method_expression*的更简洁、更具表现力，这仍然是用于向后兼容的语言。</span><span class="sxs-lookup"><span data-stu-id="a1735-2382">For almost all purposes, *lambda_expression*s are more concise and expressive than *anonymous_method_expression*s, which remain in the language for backwards compatibility.</span></span>

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

<span data-ttu-id="a1735-2383">`=>` 运算符具有与赋值（`=`）相同的优先级，并且是右结合运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2383">The `=>` operator has the same precedence as assignment (`=`) and is right-associative.</span></span>

<span data-ttu-id="a1735-2384">带有 `async` 修饰符的匿名函数是一个异步函数，并遵循[迭代](classes.md#iterators)器中所述的规则。</span><span class="sxs-lookup"><span data-stu-id="a1735-2384">An anonymous function with the `async` modifier is an async function and follows the rules described in [Iterators](classes.md#iterators).</span></span>

<span data-ttu-id="a1735-2385">可以显式或隐式类型地*lambda_expression*形式的匿名函数的参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-2385">The parameters of an anonymous function in the form of a *lambda_expression* can be explicitly or implicitly typed.</span></span> <span data-ttu-id="a1735-2386">在显式类型化参数列表中，每个参数的类型都是显式声明的。</span><span class="sxs-lookup"><span data-stu-id="a1735-2386">In an explicitly typed parameter list, the type of each parameter is explicitly stated.</span></span> <span data-ttu-id="a1735-2387">在隐式类型的参数列表中，参数的类型是从发生匿名函数的上下文中推断出来的：具体而言，当匿名函数转换为兼容的委托类型或表达式目录树类型时，该类型提供参数类型（[匿名函数转换](conversions.md#anonymous-function-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2387">In an implicitly typed parameter list, the types of the parameters are inferred from the context in which the anonymous function occurs—specifically, when the anonymous function is converted to a compatible delegate type or expression tree type, that type provides the parameter types ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="a1735-2388">在具有单个隐式类型化参数的匿名函数中，可以从参数列表中省略括号。</span><span class="sxs-lookup"><span data-stu-id="a1735-2388">In an anonymous function with a single, implicitly typed parameter, the parentheses may be omitted from the parameter list.</span></span> <span data-ttu-id="a1735-2389">换句话说，形式的匿名函数</span><span class="sxs-lookup"><span data-stu-id="a1735-2389">In other words, an anonymous function of the form</span></span>
```csharp
( param ) => expr
```
<span data-ttu-id="a1735-2390">可以缩写为</span><span class="sxs-lookup"><span data-stu-id="a1735-2390">can be abbreviated to</span></span>
```csharp
param => expr
```

<span data-ttu-id="a1735-2391">*Anonymous_method_expression*格式的匿名函数的参数列表是可选的。</span><span class="sxs-lookup"><span data-stu-id="a1735-2391">The parameter list of an anonymous function in the form of an *anonymous_method_expression* is optional.</span></span> <span data-ttu-id="a1735-2392">如果给定，则必须显式键入参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-2392">If given, the parameters must be explicitly typed.</span></span> <span data-ttu-id="a1735-2393">如果不是，则该匿名函数可转换为包含任何参数列表（不包含 `out` 参数）的委托。</span><span class="sxs-lookup"><span data-stu-id="a1735-2393">If not, the anonymous function is convertible to a delegate with any parameter list not containing `out` parameters.</span></span>

<span data-ttu-id="a1735-2394">匿名函数的*块*体是可访问的（[终结点和可访问](statements.md#end-points-and-reachability)性），除非匿名函数出现在无法访问的语句中。</span><span class="sxs-lookup"><span data-stu-id="a1735-2394">A *block* body of an anonymous function is reachable ([End points and reachability](statements.md#end-points-and-reachability)) unless the anonymous function occurs inside an unreachable statement.</span></span>

<span data-ttu-id="a1735-2395">下面是匿名函数的一些示例：</span><span class="sxs-lookup"><span data-stu-id="a1735-2395">Some examples of anonymous functions follow below:</span></span>

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

<span data-ttu-id="a1735-2396">*Lambda_expression*和*anonymous_method_expression*s 的行为是相同的，但以下情况除外：</span><span class="sxs-lookup"><span data-stu-id="a1735-2396">The behavior of *lambda_expression*s and *anonymous_method_expression*s is the same except for the following points:</span></span>

*  <span data-ttu-id="a1735-2397">*anonymous_method_expression*允许完全省略参数列表，则可以 convertibility 委托任何值参数列表的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2397">*anonymous_method_expression*s permit the parameter list to be omitted entirely, yielding convertibility to delegate types of any list of value parameters.</span></span>
*  <span data-ttu-id="a1735-2398">*lambda_expression*允许省略和推断参数类型，而*anonymous_method_expression*则需要显式声明参数类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2398">*lambda_expression*s permit parameter types to be omitted and inferred whereas *anonymous_method_expression*s require parameter types to be explicitly stated.</span></span>
*  <span data-ttu-id="a1735-2399">*Lambda_expression*的主体可以是表达式或语句块，而*anonymous_method_expression*的主体必须是语句块。</span><span class="sxs-lookup"><span data-stu-id="a1735-2399">The body of a *lambda_expression* can be an expression or a statement block whereas the body of an *anonymous_method_expression* must be a statement block.</span></span>
*  <span data-ttu-id="a1735-2400">只有*lambda_expression*s 具有转换为兼容的表达式树类型（[表达式树类型](types.md#expression-tree-types)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2400">Only *lambda_expression*s have conversions to compatible expression tree types ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-signatures"></a><span data-ttu-id="a1735-2401">匿名函数签名</span><span class="sxs-lookup"><span data-stu-id="a1735-2401">Anonymous function signatures</span></span>

<span data-ttu-id="a1735-2402">匿名函数的可选*anonymous_function_signature*定义匿名函数的名称和可选的形参类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2402">The optional *anonymous_function_signature* of an anonymous function defines the names and optionally the types of the formal parameters for the anonymous function.</span></span> <span data-ttu-id="a1735-2403">匿名函数的参数的作用域是*anonymous_function_body*。</span><span class="sxs-lookup"><span data-stu-id="a1735-2403">The scope of the parameters of the anonymous function is the *anonymous_function_body*.</span></span> <span data-ttu-id="a1735-2404">（[范围](basic-concepts.md#scopes)）与参数列表（如果给定）一起构成了声明空间（[声明](basic-concepts.md#declarations)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2404">([Scopes](basic-concepts.md#scopes)) Together with the parameter list (if given) the anonymous-method-body constitutes a declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="a1735-2405">因此，匿名函数的参数名称的编译时错误与本地变量、本地常量或参数的名称（其范围包含*anonymous_method_expression*或*lambda_expression*）相匹配。</span><span class="sxs-lookup"><span data-stu-id="a1735-2405">It is thus a compile-time error for the name of a parameter of the anonymous function to match the name of a local variable, local constant or parameter whose scope includes the *anonymous_method_expression* or *lambda_expression*.</span></span>

<span data-ttu-id="a1735-2406">如果匿名函数有*explicit_anonymous_function_signature*，则兼容的委托类型集和表达式树类型将被限制为具有相同顺序的相同参数类型和修饰符的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2406">If an anonymous function has an *explicit_anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have the same parameter types and modifiers in the same order.</span></span> <span data-ttu-id="a1735-2407">与方法组转换（[方法组转换](conversions.md#method-group-conversions)）相比，匿名函数参数类型的差异不受支持。</span><span class="sxs-lookup"><span data-stu-id="a1735-2407">In contrast to method group conversions ([Method group conversions](conversions.md#method-group-conversions)), contra-variance of anonymous function parameter types is not supported.</span></span> <span data-ttu-id="a1735-2408">如果匿名函数没有*anonymous_function_signature*，则兼容的委托类型集和表达式树类型将被限制为没有 `out` 参数的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2408">If an anonymous function does not have an *anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have no `out` parameters.</span></span>

<span data-ttu-id="a1735-2409">请注意， *anonymous_function_signature*不能包含特性或参数数组。</span><span class="sxs-lookup"><span data-stu-id="a1735-2409">Note that an *anonymous_function_signature* cannot include attributes or a parameter array.</span></span> <span data-ttu-id="a1735-2410">尽管如此， *anonymous_function_signature*可能与其参数列表包含参数数组的委托类型兼容。</span><span class="sxs-lookup"><span data-stu-id="a1735-2410">Nevertheless, an *anonymous_function_signature* may be compatible with a delegate type whose parameter list contains a parameter array.</span></span>

<span data-ttu-id="a1735-2411">另请注意，即使兼容，转换为表达式树类型也可能仍会在编译时（[表达式树类型](types.md#expression-tree-types)）失败。</span><span class="sxs-lookup"><span data-stu-id="a1735-2411">Note also that conversion to an expression tree type, even if compatible, may still fail at compile-time ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-bodies"></a><span data-ttu-id="a1735-2412">匿名函数体</span><span class="sxs-lookup"><span data-stu-id="a1735-2412">Anonymous function bodies</span></span>

<span data-ttu-id="a1735-2413">匿名函数的主体（*表达式*或*块*）将遵循以下规则：</span><span class="sxs-lookup"><span data-stu-id="a1735-2413">The body (*expression* or *block*) of an anonymous function is subject to the following rules:</span></span>

*  <span data-ttu-id="a1735-2414">如果匿名函数包含签名，则在签名中指定的参数在正文中可用。</span><span class="sxs-lookup"><span data-stu-id="a1735-2414">If the anonymous function includes a signature, the parameters specified in the signature are available in the body.</span></span> <span data-ttu-id="a1735-2415">如果匿名函数没有签名，则可以将其转换为具有参数的委托类型或表达式类型（[匿名函数转换](conversions.md#anonymous-function-conversions)），但不能在正文中访问这些参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-2415">If the anonymous function has no signature it can be converted to a delegate type or expression type having parameters ([Anonymous function conversions](conversions.md#anonymous-function-conversions)), but the parameters cannot be accessed in the body.</span></span>
*  <span data-ttu-id="a1735-2416">除了在最近的封闭匿名函数的签名（如果有）中指定的 `ref` 或 `out` 参数外，主体访问 `ref` 或 `out` 参数的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-2416">Except for `ref` or `out` parameters specified in the signature (if any) of the nearest enclosing anonymous function, it is a compile-time error for the body to access a `ref` or `out` parameter.</span></span>
*  <span data-ttu-id="a1735-2417">当 `this` 的类型为结构类型时，主体访问 `this`时会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-2417">When the type of `this` is a struct type, it is a compile-time error for the body to access `this`.</span></span> <span data-ttu-id="a1735-2418">无论访问是显式的（如 `this.x`）还是隐式的（如在 `x` （其中 `x` 是结构的实例成员），都是如此。</span><span class="sxs-lookup"><span data-stu-id="a1735-2418">This is true whether the access is explicit (as in `this.x`) or implicit (as in `x` where `x` is an instance member of the struct).</span></span> <span data-ttu-id="a1735-2419">此规则只是禁止此类访问，不会影响成员查找是否会导致结构的成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-2419">This rule simply prohibits such access and does not affect whether member lookup results in a member of the struct.</span></span>
*  <span data-ttu-id="a1735-2420">主体有权访问匿名函数的外部变量（[外部变量](expressions.md#outer-variables)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2420">The body has access to the outer variables ([Outer variables](expressions.md#outer-variables)) of the anonymous function.</span></span> <span data-ttu-id="a1735-2421">外部变量的访问将引用在计算*lambda_expression*或*anonymous_method_expression*时处于活动状态的变量实例（[匿名函数表达式的计算](expressions.md#evaluation-of-anonymous-function-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2421">Access of an outer variable will reference the instance of the variable that is active at the time the *lambda_expression* or *anonymous_method_expression* is evaluated ([Evaluation of anonymous function expressions](expressions.md#evaluation-of-anonymous-function-expressions)).</span></span>
*  <span data-ttu-id="a1735-2422">如果主体包含的 `goto` 语句、`break` 语句或 `continue` 语句的目标在正文外或包含的匿名函数的主体中，则它是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-2422">It is a compile-time error for the body to contain a `goto` statement, `break` statement, or `continue` statement whose target is outside the body or within the body of a contained anonymous function.</span></span>
*  <span data-ttu-id="a1735-2423">正文中的 `return` 语句从最近的封闭匿名函数（而不是来自封闭函数成员）的调用返回控制权。</span><span class="sxs-lookup"><span data-stu-id="a1735-2423">A `return` statement in the body returns control from an invocation of the nearest enclosing anonymous function, not from the enclosing function member.</span></span> <span data-ttu-id="a1735-2424">`return` 语句中指定的表达式必须可隐式转换为最接近*lambda_expression*或*anonymous_method_expression*转换为的委托类型或表达式树类型的返回类型（[匿名函数转换](conversions.md#anonymous-function-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2424">An expression specified in a `return` statement must be implicitly convertible to the return type of the delegate type or expression tree type to which the nearest enclosing *lambda_expression* or *anonymous_method_expression* is converted ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="a1735-2425">明确指出，无论是否有任何方法可以执行匿名函数的块，而不是通过计算和调用*lambda_expression*或*anonymous_method_expression*。</span><span class="sxs-lookup"><span data-stu-id="a1735-2425">It is explicitly unspecified whether there is any way to execute the block of an anonymous function other than through evaluation and invocation of the *lambda_expression* or *anonymous_method_expression*.</span></span> <span data-ttu-id="a1735-2426">特别是，编译器可以通过综合一个或多个命名方法或类型来选择实现匿名函数。</span><span class="sxs-lookup"><span data-stu-id="a1735-2426">In particular, the compiler may choose to implement an anonymous function by synthesizing one or more named methods or types.</span></span> <span data-ttu-id="a1735-2427">任何此类合成元素的名称必须是保留供编译器使用的形式。</span><span class="sxs-lookup"><span data-stu-id="a1735-2427">The names of any such synthesized elements must be of a form reserved for compiler use.</span></span>

### <a name="overload-resolution-and-anonymous-functions"></a><span data-ttu-id="a1735-2428">重载决策和匿名函数</span><span class="sxs-lookup"><span data-stu-id="a1735-2428">Overload resolution and anonymous functions</span></span>

<span data-ttu-id="a1735-2429">自变量列表中的匿名函数参与类型推理和重载解析。</span><span class="sxs-lookup"><span data-stu-id="a1735-2429">Anonymous functions in an argument list participate in type inference and overload resolution.</span></span> <span data-ttu-id="a1735-2430">请参阅[类型推理](expressions.md#type-inference)和[重载决策](expressions.md#overload-resolution)，了解确切的规则。</span><span class="sxs-lookup"><span data-stu-id="a1735-2430">Please refer to [Type inference](expressions.md#type-inference) and [Overload resolution](expressions.md#overload-resolution) for the exact rules.</span></span>

<span data-ttu-id="a1735-2431">下面的示例演示匿名函数对重载决策的影响。</span><span class="sxs-lookup"><span data-stu-id="a1735-2431">The following example illustrates the effect of anonymous functions on overload resolution.</span></span>

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

<span data-ttu-id="a1735-2432">`ItemList<T>` 类有两个 `Sum` 方法。</span><span class="sxs-lookup"><span data-stu-id="a1735-2432">The `ItemList<T>` class has two `Sum` methods.</span></span> <span data-ttu-id="a1735-2433">每个都采用 `selector` 参数，该参数从列表项中提取要求和的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2433">Each takes a `selector` argument, which extracts the value to sum over from a list item.</span></span> <span data-ttu-id="a1735-2434">提取的值可以是 `int` 或 `double`，所得的总和也可以是 `int` 或 `double`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2434">The extracted value can be either an `int` or a `double` and the resulting sum is likewise either an `int` or a `double`.</span></span>

<span data-ttu-id="a1735-2435">例如，`Sum` 方法可用于计算按订单列出的详细信息行的总和。</span><span class="sxs-lookup"><span data-stu-id="a1735-2435">The `Sum` methods could for example be used to compute sums from a list of detail lines in an order.</span></span>

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

<span data-ttu-id="a1735-2436">在第一次调用 `orderDetails.Sum`中，这两个 `Sum` 方法都适用，因为匿名函数 `d => d. UnitCount` 与 `Func<Detail,int>` 和 `Func<Detail,double>`都兼容。</span><span class="sxs-lookup"><span data-stu-id="a1735-2436">In the first invocation of `orderDetails.Sum`, both `Sum` methods are applicable because the anonymous function `d => d. UnitCount` is compatible with both `Func<Detail,int>` and `Func<Detail,double>`.</span></span> <span data-ttu-id="a1735-2437">但重载决策将选取第一个 `Sum` 方法，因为转换到 `Func<Detail,int>` 比转换到 `Func<Detail,double>`好。</span><span class="sxs-lookup"><span data-stu-id="a1735-2437">However, overload resolution picks the first `Sum` method because the conversion to `Func<Detail,int>` is better than the conversion to `Func<Detail,double>`.</span></span>

<span data-ttu-id="a1735-2438">在第二次调用 `orderDetails.Sum`中，只会使用第二个 `Sum` 方法，因为匿名函数 `d => d.UnitPrice * d.UnitCount` 生成类型 `double`的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2438">In the second invocation of `orderDetails.Sum`, only the second `Sum` method is applicable because the anonymous function `d => d.UnitPrice * d.UnitCount` produces a value of type `double`.</span></span> <span data-ttu-id="a1735-2439">因此，重载决策为该调用选择第二个 `Sum` 方法。</span><span class="sxs-lookup"><span data-stu-id="a1735-2439">Thus, overload resolution picks the second `Sum` method for that invocation.</span></span>

### <a name="anonymous-functions-and-dynamic-binding"></a><span data-ttu-id="a1735-2440">匿名函数和动态绑定</span><span class="sxs-lookup"><span data-stu-id="a1735-2440">Anonymous functions and dynamic binding</span></span>

<span data-ttu-id="a1735-2441">匿名函数不能是动态绑定操作的接收方、参数或操作数。</span><span class="sxs-lookup"><span data-stu-id="a1735-2441">An anonymous function cannot be a receiver, argument or operand of a dynamically bound operation.</span></span>

### <a name="outer-variables"></a><span data-ttu-id="a1735-2442">外部变量</span><span class="sxs-lookup"><span data-stu-id="a1735-2442">Outer variables</span></span>

<span data-ttu-id="a1735-2443">任何本地变量、值参数或其范围包含*lambda_expression*或*anonymous_method_expression*的参数数组称为匿名函数的***外部变量***。</span><span class="sxs-lookup"><span data-stu-id="a1735-2443">Any local variable, value parameter, or parameter array whose scope includes the *lambda_expression* or *anonymous_method_expression* is called an ***outer variable*** of the anonymous function.</span></span> <span data-ttu-id="a1735-2444">在类的实例函数成员中，`this` 值被视为值参数，并且是函数成员内包含的任何匿名函数的外部变量。</span><span class="sxs-lookup"><span data-stu-id="a1735-2444">In an instance function member of a class, the `this` value is considered a value parameter and is an outer variable of any anonymous function contained within the function member.</span></span>

#### <a name="captured-outer-variables"></a><span data-ttu-id="a1735-2445">捕获的外部变量</span><span class="sxs-lookup"><span data-stu-id="a1735-2445">Captured outer variables</span></span>

<span data-ttu-id="a1735-2446">当某个外部变量被匿名函数引用时，该外部变量被视为已被匿名函数***捕获***。</span><span class="sxs-lookup"><span data-stu-id="a1735-2446">When an outer variable is referenced by an anonymous function, the outer variable is said to have been ***captured*** by the anonymous function.</span></span> <span data-ttu-id="a1735-2447">通常，局部变量的生存期仅限于执行它所关联的块或语句（[局部变量](variables.md#local-variables)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2447">Ordinarily, the lifetime of a local variable is limited to execution of the block or statement with which it is associated ([Local variables](variables.md#local-variables)).</span></span> <span data-ttu-id="a1735-2448">但是，已捕获的外部变量的生存期至少会进行扩展，直到从匿名函数创建的委托或表达式树变为可进行垃圾回收的条件。</span><span class="sxs-lookup"><span data-stu-id="a1735-2448">However, the lifetime of a captured outer variable is extended at least until the delegate or expression tree created from the anonymous function becomes eligible for garbage collection.</span></span>

<span data-ttu-id="a1735-2449">示例中</span><span class="sxs-lookup"><span data-stu-id="a1735-2449">In the example</span></span>
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
<span data-ttu-id="a1735-2450">局部变量 `x` 由匿名函数捕获，并且 `x` 的生存期至少会进行扩展，直到从 `F` 返回的委托符合垃圾回收条件（这在程序的最末尾之前不会发生）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2450">the local variable `x` is captured by the anonymous function, and the lifetime of `x` is extended at least until the delegate returned from `F` becomes eligible for garbage collection (which doesn't happen until the very end of the program).</span></span> <span data-ttu-id="a1735-2451">由于匿名函数的每个调用都在 `x`的同一个实例上运行，因此该示例的输出为：</span><span class="sxs-lookup"><span data-stu-id="a1735-2451">Since each invocation of the anonymous function operates on the same instance of `x`, the output of the example is:</span></span>
```console
1
2
3
```

<span data-ttu-id="a1735-2452">当匿名函数捕获本地变量或值参数时，本地变量或参数不再被视为固定变量（[固定](unsafe-code.md#fixed-and-moveable-variables)变量），而是被视为可移动的变量。</span><span class="sxs-lookup"><span data-stu-id="a1735-2452">When a local variable or a value parameter is captured by an anonymous function, the local variable or parameter is no longer considered to be a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), but is instead considered to be a moveable variable.</span></span> <span data-ttu-id="a1735-2453">因此，任何采用已捕获外部变量地址的 `unsafe` 代码都必须首先使用 `fixed` 语句来修复该变量。</span><span class="sxs-lookup"><span data-stu-id="a1735-2453">Thus any `unsafe` code that takes the address of a captured outer variable must first use the `fixed` statement to fix the variable.</span></span>

<span data-ttu-id="a1735-2454">请注意，与 uncaptured 变量不同，捕获的局部变量可以同时公开给多个执行线程。</span><span class="sxs-lookup"><span data-stu-id="a1735-2454">Note that unlike an uncaptured variable, a captured local variable can be simultaneously exposed to multiple threads of execution.</span></span>

#### <a name="instantiation-of-local-variables"></a><span data-ttu-id="a1735-2455">局部变量的实例化</span><span class="sxs-lookup"><span data-stu-id="a1735-2455">Instantiation of local variables</span></span>

<span data-ttu-id="a1735-2456">当执行进入变量的作用域时，本地变量被视为被***实例化***。</span><span class="sxs-lookup"><span data-stu-id="a1735-2456">A local variable is considered to be ***instantiated*** when execution enters the scope of the variable.</span></span> <span data-ttu-id="a1735-2457">例如，在调用以下方法时，本地变量 `x` 将实例化并初始化三次，一次针对循环的每次迭代。</span><span class="sxs-lookup"><span data-stu-id="a1735-2457">For example, when the following method is invoked, the local variable `x` is instantiated and initialized three times—once for each iteration of the loop.</span></span>

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="a1735-2458">但是，在循环外移动 `x` 的声明会导致 `x`的单个实例化：</span><span class="sxs-lookup"><span data-stu-id="a1735-2458">However, moving the declaration of `x` outside the loop results in a single instantiation of `x`:</span></span>
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="a1735-2459">如果未捕获，则无法准确观察局部变量的实例化频率（因为实例化的生存期是不连续的），因此每个实例化都可以只使用相同的存储位置。</span><span class="sxs-lookup"><span data-stu-id="a1735-2459">When not captured, there is no way to observe exactly how often a local variable is instantiated—because the lifetimes of the instantiations are disjoint, it is possible for each instantiation to simply use the same storage location.</span></span> <span data-ttu-id="a1735-2460">但是，当匿名函数捕获本地变量时，实例化的效果会变得很明显。</span><span class="sxs-lookup"><span data-stu-id="a1735-2460">However, when an anonymous function captures a local variable, the effects of instantiation become apparent.</span></span>

<span data-ttu-id="a1735-2461">示例</span><span class="sxs-lookup"><span data-stu-id="a1735-2461">The example</span></span>
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
<span data-ttu-id="a1735-2462">生成输出：</span><span class="sxs-lookup"><span data-stu-id="a1735-2462">produces the output:</span></span>
```console
1
3
5
```

<span data-ttu-id="a1735-2463">但是，当 `x` 的声明移到循环外时：</span><span class="sxs-lookup"><span data-stu-id="a1735-2463">However, when the declaration of `x` is moved outside the loop:</span></span>
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
<span data-ttu-id="a1735-2464">输出是：</span><span class="sxs-lookup"><span data-stu-id="a1735-2464">the output is:</span></span>
```console
5
5
5
```

<span data-ttu-id="a1735-2465">如果 for 循环声明迭代变量，则该变量本身被视为在循环外部声明。</span><span class="sxs-lookup"><span data-stu-id="a1735-2465">If a for-loop declares an iteration variable, that variable itself is considered to be declared outside of the loop.</span></span> <span data-ttu-id="a1735-2466">因此，如果将该示例更改为捕获迭代变量自身：</span><span class="sxs-lookup"><span data-stu-id="a1735-2466">Thus, if the example is changed to capture the iteration variable itself:</span></span>

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
<span data-ttu-id="a1735-2467">只捕获迭代变量的一个实例，这将生成输出：</span><span class="sxs-lookup"><span data-stu-id="a1735-2467">only one instance of the iteration variable is captured, which produces the output:</span></span>
```console
3
3
3
```

<span data-ttu-id="a1735-2468">匿名函数委托可以共享一些捕获的变量，但其他实例却有单独的实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-2468">It is possible for anonymous function delegates to share some captured variables yet have separate instances of others.</span></span> <span data-ttu-id="a1735-2469">例如，如果 `F` 更改为</span><span class="sxs-lookup"><span data-stu-id="a1735-2469">For example, if `F` is changed to</span></span>
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
<span data-ttu-id="a1735-2470">这三个委托捕获 `x` 的同一实例，但 `y`的单独实例，输出为：</span><span class="sxs-lookup"><span data-stu-id="a1735-2470">the three delegates capture the same instance of `x` but separate instances of `y`, and the output is:</span></span>
```console
1 1
2 1
3 1
```

<span data-ttu-id="a1735-2471">单独的匿名函数可以捕获外部变量的同一个实例。</span><span class="sxs-lookup"><span data-stu-id="a1735-2471">Separate anonymous functions can capture the same instance of an outer variable.</span></span> <span data-ttu-id="a1735-2472">在下面的示例中：</span><span class="sxs-lookup"><span data-stu-id="a1735-2472">In the example:</span></span>
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
<span data-ttu-id="a1735-2473">这两个匿名函数 `x`捕获本地变量的同一个实例，因此它们可以通过该变量 "进行通信"。</span><span class="sxs-lookup"><span data-stu-id="a1735-2473">the two anonymous functions capture the same instance of the local variable `x`, and they can thus "communicate" through that variable.</span></span> <span data-ttu-id="a1735-2474">该示例的输出为：</span><span class="sxs-lookup"><span data-stu-id="a1735-2474">The output of the example is:</span></span>
```console
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a><span data-ttu-id="a1735-2475">匿名函数表达式的计算</span><span class="sxs-lookup"><span data-stu-id="a1735-2475">Evaluation of anonymous function expressions</span></span>

<span data-ttu-id="a1735-2476">匿名函数 `F` 必须始终转换为委托类型 `D` 或 `E`（直接或通过执行委托创建表达式 `new D(F)`）的表达式树类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2476">An anonymous function `F` must always be converted to a delegate type `D` or an expression tree type `E`, either directly or through the execution of a delegate creation expression `new D(F)`.</span></span> <span data-ttu-id="a1735-2477">此转换确定匿名函数的结果，如[匿名函数转换](conversions.md#anonymous-function-conversions)中所述。</span><span class="sxs-lookup"><span data-stu-id="a1735-2477">This conversion determines the result of the anonymous function, as described in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>

## <a name="query-expressions"></a><span data-ttu-id="a1735-2478">查询表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-2478">Query expressions</span></span>

<span data-ttu-id="a1735-2479">***查询表达式***为类似于关系和分层查询语言（如 SQL 和 XQuery）的查询提供了语言集成语法。</span><span class="sxs-lookup"><span data-stu-id="a1735-2479">***Query expressions*** provide a language integrated syntax for queries that is similar to relational and hierarchical query languages such as SQL and XQuery.</span></span>

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

<span data-ttu-id="a1735-2480">查询表达式以 `from` 子句开头，以 `select` 或 `group` 子句结束。</span><span class="sxs-lookup"><span data-stu-id="a1735-2480">A query expression begins with a `from` clause and ends with either a `select` or `group` clause.</span></span> <span data-ttu-id="a1735-2481">初始 `from` 子句后面可以跟零个或多个 `from`、`let`、`where`、`join` 或 `orderby` 子句。</span><span class="sxs-lookup"><span data-stu-id="a1735-2481">The initial `from` clause can be followed by zero or more `from`, `let`, `where`, `join` or `orderby` clauses.</span></span> <span data-ttu-id="a1735-2482">每个 `from` 子句都是引入一个***范围变量***的生成器，该变量范围是***序列***的元素。</span><span class="sxs-lookup"><span data-stu-id="a1735-2482">Each `from` clause is a generator introducing a ***range variable*** which ranges over the elements of a ***sequence***.</span></span> <span data-ttu-id="a1735-2483">每个 `let` 子句引入一个范围变量，表示通过以前的范围变量计算得出的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2483">Each `let` clause introduces a range variable representing a value computed by means of previous range variables.</span></span> <span data-ttu-id="a1735-2484">每个 `where` 子句是一个从结果中排除项的筛选器。</span><span class="sxs-lookup"><span data-stu-id="a1735-2484">Each `where` clause is a filter that excludes items from the result.</span></span> <span data-ttu-id="a1735-2485">每个 `join` 子句将源序列的指定键与另一个序列的键进行比较，从而生成匹配对。</span><span class="sxs-lookup"><span data-stu-id="a1735-2485">Each `join` clause compares specified keys of the source sequence with keys of another sequence, yielding matching pairs.</span></span> <span data-ttu-id="a1735-2486">每个 `orderby` 子句会根据指定的条件对项进行重新排序。Final `select` 或 `group` 子句根据范围变量指定结果的形状。</span><span class="sxs-lookup"><span data-stu-id="a1735-2486">Each `orderby` clause reorders items according to specified criteria.The final `select` or `group` clause specifies the shape of the result in terms of the range variables.</span></span> <span data-ttu-id="a1735-2487">最后，`into` 子句可用于将一个查询的结果作为一个生成器在后续查询中处理。</span><span class="sxs-lookup"><span data-stu-id="a1735-2487">Finally, an `into` clause can be used to "splice" queries by treating the results of one query as a generator in a subsequent query.</span></span>

### <a name="ambiguities-in-query-expressions"></a><span data-ttu-id="a1735-2488">查询表达式中的多义性</span><span class="sxs-lookup"><span data-stu-id="a1735-2488">Ambiguities in query expressions</span></span>

<span data-ttu-id="a1735-2489">查询表达式包含许多 "上下文关键字"，即在给定上下文中具有特殊意义的标识符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2489">Query expressions contain a number of "contextual keywords", i.e., identifiers that have special meaning in a given context.</span></span> <span data-ttu-id="a1735-2490">具体来说，这些是 `from`、`where`、`join`、`on`、`equals`、`into`、`let`、`orderby`、`ascending`、`descending`、`select`、`group` 和 `by`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2490">Specifically these are `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` and `by`.</span></span> <span data-ttu-id="a1735-2491">为了避免因混合使用这些标识符作为关键字或简单名称而导致的查询表达式中出现歧义，在查询表达式中的任何位置出现时，这些标识符都被视为关键字。</span><span class="sxs-lookup"><span data-stu-id="a1735-2491">In order to avoid ambiguities in query expressions caused by mixed use of these identifiers as keywords or simple names, these identifiers are considered keywords when occurring anywhere within a query expression.</span></span>

<span data-ttu-id="a1735-2492">为此，查询表达式是以 "`from identifier`" 开头的任何表达式，后跟除 "`;`"、"`=`" 或 "`,`" 之外的任何标记。</span><span class="sxs-lookup"><span data-stu-id="a1735-2492">For this purpose, a query expression is any expression that starts with "`from identifier`" followed by any token except "`;`", "`=`" or "`,`".</span></span>

<span data-ttu-id="a1735-2493">为了在查询表达式中使用这些字词作为标识符，可以使用 "`@`" （[标识符](lexical-structure.md#identifiers)）作为前缀。</span><span class="sxs-lookup"><span data-stu-id="a1735-2493">In order to use these words as identifiers within a query expression, they can be prefixed with "`@`" ([Identifiers](lexical-structure.md#identifiers)).</span></span>

### <a name="query-expression-translation"></a><span data-ttu-id="a1735-2494">查询表达式转换</span><span class="sxs-lookup"><span data-stu-id="a1735-2494">Query expression translation</span></span>

<span data-ttu-id="a1735-2495">该C#语言不指定查询表达式的执行语义。</span><span class="sxs-lookup"><span data-stu-id="a1735-2495">The C# language does not specify the execution semantics of query expressions.</span></span> <span data-ttu-id="a1735-2496">相反，查询表达式被转换为符合*查询表达式模式*（[查询表达式模式](expressions.md#the-query-expression-pattern)）的方法调用。</span><span class="sxs-lookup"><span data-stu-id="a1735-2496">Rather, query expressions are translated into invocations of methods that adhere to the *query expression pattern* ([The query expression pattern](expressions.md#the-query-expression-pattern)).</span></span> <span data-ttu-id="a1735-2497">具体而言，将查询表达式转换为名为 `Where`、`Select`、`SelectMany`、`Join`、`GroupJoin`、`OrderBy`、`OrderByDescending`、`ThenBy`、`ThenByDescending`、`GroupBy`和 `Cast`的方法调用。这些方法应具有特定的签名和结果类型，如[查询表达式模式](expressions.md#the-query-expression-pattern)中所述。</span><span class="sxs-lookup"><span data-stu-id="a1735-2497">Specifically, query expressions are translated into invocations of methods named `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, and `Cast`.These methods are expected to have particular signatures and result types, as described in [The query expression pattern](expressions.md#the-query-expression-pattern).</span></span> <span data-ttu-id="a1735-2498">这些方法可以是要查询的对象的实例方法或对象外部的扩展方法，并实现查询的实际执行。</span><span class="sxs-lookup"><span data-stu-id="a1735-2498">These methods can be instance methods of the object being queried or extension methods that are external to the object, and they implement the actual execution of the query.</span></span>

<span data-ttu-id="a1735-2499">从查询表达式转换为方法调用是在执行任何类型绑定或重载决策之前发生的语法映射。</span><span class="sxs-lookup"><span data-stu-id="a1735-2499">The translation from query expressions to method invocations is a syntactic mapping that occurs before any type binding or overload resolution has been performed.</span></span> <span data-ttu-id="a1735-2500">转换确保语法正确，但不保证产生语义正确C#的代码。</span><span class="sxs-lookup"><span data-stu-id="a1735-2500">The translation is guaranteed to be syntactically correct, but it is not guaranteed to produce semantically correct C# code.</span></span> <span data-ttu-id="a1735-2501">转换查询表达式后，生成的方法调用将作为常规方法调用进行处理，这可能会导致错误（例如，如果方法不存在，如果参数具有错误的类型），或者如果方法是泛型，则类型推理失败。</span><span class="sxs-lookup"><span data-stu-id="a1735-2501">Following translation of query expressions, the resulting method invocations are processed as regular method invocations, and this may in turn uncover errors, for example if the methods do not exist, if arguments have wrong types, or if the methods are generic and type inference fails.</span></span>

<span data-ttu-id="a1735-2502">通过重复应用以下翻译来处理查询表达式，直到不能进一步缩减。</span><span class="sxs-lookup"><span data-stu-id="a1735-2502">A query expression is processed by repeatedly applying the following translations until no further reductions are possible.</span></span> <span data-ttu-id="a1735-2503">翻译按照应用程序顺序列出：每个部分都假设之前部分中的翻译已进行了详尽的操作，一旦完成，就不会再在处理同一查询表达式时再次使用部分。</span><span class="sxs-lookup"><span data-stu-id="a1735-2503">The translations are listed in order of application: each section assumes that the translations in the preceding sections have been performed exhaustively, and once exhausted, a section will not later be revisited in the processing of the same query expression.</span></span>

<span data-ttu-id="a1735-2504">不允许在查询表达式中分配范围变量。</span><span class="sxs-lookup"><span data-stu-id="a1735-2504">Assignment to range variables is not allowed in query expressions.</span></span> <span data-ttu-id="a1735-2505">但允许C#实现并非始终强制实施此限制，因为有时可能不会使用此处提供的语法转换方案。</span><span class="sxs-lookup"><span data-stu-id="a1735-2505">However a C# implementation is permitted to not always enforce this restriction, since this may sometimes not be possible with the syntactic translation scheme presented here.</span></span>

<span data-ttu-id="a1735-2506">某些翻译注入范围变量，其中包含由 `*`表示的透明标识符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2506">Certain translations inject range variables with transparent identifiers denoted by `*`.</span></span> <span data-ttu-id="a1735-2507">[透明标识符中进一步](expressions.md#transparent-identifiers)讨论了透明标识符的特殊属性。</span><span class="sxs-lookup"><span data-stu-id="a1735-2507">The special properties of transparent identifiers are discussed further in [Transparent identifiers](expressions.md#transparent-identifiers).</span></span>

#### <a name="select-and-groupby-clauses-with-continuations"></a><span data-ttu-id="a1735-2508">带有延续的 Select 和 groupby 子句</span><span class="sxs-lookup"><span data-stu-id="a1735-2508">Select and groupby clauses with continuations</span></span>

<span data-ttu-id="a1735-2509">具有延续的查询表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-2509">A query expression with a continuation</span></span>
```csharp
from ... into x ...
```
<span data-ttu-id="a1735-2510">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2510">is translated into</span></span>
```csharp
from x in ( from ... ) ...
```

<span data-ttu-id="a1735-2511">以下各部分中的翻译假设查询没有 `into` 的继续符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2511">The translations in the following sections assume that queries have no `into` continuations.</span></span>

<span data-ttu-id="a1735-2512">示例</span><span class="sxs-lookup"><span data-stu-id="a1735-2512">The example</span></span>
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="a1735-2513">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2513">is translated into</span></span>
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="a1735-2514">的最终转换是</span><span class="sxs-lookup"><span data-stu-id="a1735-2514">the final translation of which is</span></span>
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a><span data-ttu-id="a1735-2515">显式范围变量类型</span><span class="sxs-lookup"><span data-stu-id="a1735-2515">Explicit range variable types</span></span>

<span data-ttu-id="a1735-2516">显式指定范围变量类型的 `from` 子句</span><span class="sxs-lookup"><span data-stu-id="a1735-2516">A `from` clause that explicitly specifies a range variable type</span></span>
```csharp
from T x in e
```
<span data-ttu-id="a1735-2517">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2517">is translated into</span></span>
```csharp
from x in ( e ) . Cast < T > ( )
```

<span data-ttu-id="a1735-2518">显式指定范围变量类型的 `join` 子句</span><span class="sxs-lookup"><span data-stu-id="a1735-2518">A `join` clause that explicitly specifies a range variable type</span></span>
```csharp
join T x in e on k1 equals k2
```
<span data-ttu-id="a1735-2519">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2519">is translated into</span></span>
```csharp
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

<span data-ttu-id="a1735-2520">以下部分中的翻译假设查询没有显式范围变量类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2520">The translations in the following sections assume that queries have no explicit range variable types.</span></span>

<span data-ttu-id="a1735-2521">示例</span><span class="sxs-lookup"><span data-stu-id="a1735-2521">The example</span></span>
```csharp
from Customer c in customers
where c.City == "London"
select c
```
<span data-ttu-id="a1735-2522">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2522">is translated into</span></span>
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
<span data-ttu-id="a1735-2523">的最终转换是</span><span class="sxs-lookup"><span data-stu-id="a1735-2523">the final translation of which is</span></span>
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

<span data-ttu-id="a1735-2524">显式范围变量类型可用于查询实现非泛型 `IEnumerable` 接口的集合，但不能用于泛型 `IEnumerable<T>` 接口。</span><span class="sxs-lookup"><span data-stu-id="a1735-2524">Explicit range variable types are useful for querying collections that implement the non-generic `IEnumerable` interface, but not the generic `IEnumerable<T>` interface.</span></span> <span data-ttu-id="a1735-2525">在上述示例中，如果 `customers` 的类型 `ArrayList`，就会出现这种情况。</span><span class="sxs-lookup"><span data-stu-id="a1735-2525">In the example above, this would be the case if `customers` were of type `ArrayList`.</span></span>

#### <a name="degenerate-query-expressions"></a><span data-ttu-id="a1735-2526">退化查询表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-2526">Degenerate query expressions</span></span>

<span data-ttu-id="a1735-2527">格式为的查询表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-2527">A query expression of the form</span></span>
```csharp
from x in e select x
```
<span data-ttu-id="a1735-2528">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2528">is translated into</span></span>
```csharp
( e ) . Select ( x => x )
```

<span data-ttu-id="a1735-2529">示例</span><span class="sxs-lookup"><span data-stu-id="a1735-2529">The example</span></span>
```csharp
from c in customers
select c
```
<span data-ttu-id="a1735-2530">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2530">is translated into</span></span>
```csharp
customers.Select(c => c)
```

<span data-ttu-id="a1735-2531">退化查询表达式是完全选择源中的元素的表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-2531">A degenerate query expression is one that trivially selects the elements of the source.</span></span> <span data-ttu-id="a1735-2532">转换的更晚阶段会删除其他翻译步骤引入的退化查询，方法是将其替换为其源。</span><span class="sxs-lookup"><span data-stu-id="a1735-2532">A later phase of the translation removes degenerate queries introduced by other translation steps by replacing them with their source.</span></span> <span data-ttu-id="a1735-2533">但要确保查询表达式的结果绝不是源对象本身，这一点很重要，因为这会将源的类型和标识显示到查询的客户端。</span><span class="sxs-lookup"><span data-stu-id="a1735-2533">It is important however to ensure that the result of a query expression is never the source object itself, as that would reveal the type and identity of the source to the client of the query.</span></span> <span data-ttu-id="a1735-2534">因此，此步骤通过显式调用源 `Select` 来保护直接在源代码中编写的退化查询。</span><span class="sxs-lookup"><span data-stu-id="a1735-2534">Therefore this step protects degenerate queries written directly in source code by explicitly calling `Select` on the source.</span></span> <span data-ttu-id="a1735-2535">然后由 `Select` 和其他查询运算符的实施者来确保这些方法从不返回源对象本身。</span><span class="sxs-lookup"><span data-stu-id="a1735-2535">It is then up to the implementers of `Select` and other query operators to ensure that these methods never return the source object itself.</span></span>

#### <a name="from-let-where-join-and-orderby-clauses"></a><span data-ttu-id="a1735-2536">From、let、where、join 和 orderby 子句</span><span class="sxs-lookup"><span data-stu-id="a1735-2536">From, let, where, join and orderby clauses</span></span>

<span data-ttu-id="a1735-2537">包含第二个 `from` 子句后跟 `select` 子句的查询表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-2537">A query expression with a second `from` clause followed by a `select` clause</span></span>
```csharp
from x1 in e1
from x2 in e2
select v
```
<span data-ttu-id="a1735-2538">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2538">is translated into</span></span>
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="a1735-2539">一个查询表达式，该表达式的第二个 `from` 子句后跟除 `select` 子句之外的其他内容：</span><span class="sxs-lookup"><span data-stu-id="a1735-2539">A query expression with a second `from` clause followed by something other than a `select` clause:</span></span>

```csharp
from x1 in e1
from x2 in e2
...
```
<span data-ttu-id="a1735-2540">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2540">is translated into</span></span>
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

<span data-ttu-id="a1735-2541">带有 `let` 子句的查询表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-2541">A query expression with a `let` clause</span></span>
```csharp
from x in e
let y = f
...
```
<span data-ttu-id="a1735-2542">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2542">is translated into</span></span>
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

<span data-ttu-id="a1735-2543">带有 `where` 子句的查询表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-2543">A query expression with a `where` clause</span></span>
```csharp
from x in e
where f
...
```
<span data-ttu-id="a1735-2544">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2544">is translated into</span></span>
```csharp
from x in ( e ) . Where ( x => f )
...
```

<span data-ttu-id="a1735-2545">带有 `join` 子句且不带 `into` 的查询表达式，后面跟有 `select` 子句</span><span class="sxs-lookup"><span data-stu-id="a1735-2545">A query expression with a `join` clause without an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
<span data-ttu-id="a1735-2546">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2546">is translated into</span></span>
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="a1735-2547">一个查询表达式，其中的 `join` 子句没有 `into`，后面跟有 `select` 子句以外的其他内容</span><span class="sxs-lookup"><span data-stu-id="a1735-2547">A query expression with a `join` clause without an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
<span data-ttu-id="a1735-2548">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2548">is translated into</span></span>
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

<span data-ttu-id="a1735-2549">带有 `into` 后跟 `select` 子句的 `join` 子句的查询表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-2549">A query expression with a `join` clause with an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
<span data-ttu-id="a1735-2550">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2550">is translated into</span></span>
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

<span data-ttu-id="a1735-2551">包含 `into` 后跟除 `select` 子句之外的 `join` 子句的查询表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-2551">A query expression with a `join` clause with an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
<span data-ttu-id="a1735-2552">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2552">is translated into</span></span>
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

<span data-ttu-id="a1735-2553">带有 `orderby` 子句的查询表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-2553">A query expression with an `orderby` clause</span></span>
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
<span data-ttu-id="a1735-2554">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2554">is translated into</span></span>
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

<span data-ttu-id="a1735-2555">如果 order 子句指定 `descending` 方向指示器，则改为生成 `OrderByDescending` 或 `ThenByDescending` 的调用。</span><span class="sxs-lookup"><span data-stu-id="a1735-2555">If an ordering clause specifies a `descending` direction indicator, an invocation of `OrderByDescending` or `ThenByDescending` is produced instead.</span></span>

<span data-ttu-id="a1735-2556">以下翻译假设每个查询表达式中没有 `let`、`where`、`join` 或 `orderby` 子句，且不超过一个初始 `from` 子句。</span><span class="sxs-lookup"><span data-stu-id="a1735-2556">The following translations assume that there are no `let`, `where`, `join` or `orderby` clauses, and no more than the one initial `from` clause in each query expression.</span></span>

<span data-ttu-id="a1735-2557">示例</span><span class="sxs-lookup"><span data-stu-id="a1735-2557">The example</span></span>
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="a1735-2558">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2558">is translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

<span data-ttu-id="a1735-2559">示例</span><span class="sxs-lookup"><span data-stu-id="a1735-2559">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="a1735-2560">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2560">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="a1735-2561">的最终转换是</span><span class="sxs-lookup"><span data-stu-id="a1735-2561">the final translation of which is</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
<span data-ttu-id="a1735-2562">其中 `x` 是编译器生成的标识符，否则不可见且不可访问。</span><span class="sxs-lookup"><span data-stu-id="a1735-2562">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="a1735-2563">示例</span><span class="sxs-lookup"><span data-stu-id="a1735-2563">The example</span></span>
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
<span data-ttu-id="a1735-2564">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2564">is translated into</span></span>
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
<span data-ttu-id="a1735-2565">的最终转换是</span><span class="sxs-lookup"><span data-stu-id="a1735-2565">the final translation of which is</span></span>
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
<span data-ttu-id="a1735-2566">其中 `x` 是编译器生成的标识符，否则不可见且不可访问。</span><span class="sxs-lookup"><span data-stu-id="a1735-2566">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="a1735-2567">示例</span><span class="sxs-lookup"><span data-stu-id="a1735-2567">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
<span data-ttu-id="a1735-2568">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2568">is translated into</span></span>
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

<span data-ttu-id="a1735-2569">示例</span><span class="sxs-lookup"><span data-stu-id="a1735-2569">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="a1735-2570">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2570">is translated into</span></span>
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="a1735-2571">的最终转换是</span><span class="sxs-lookup"><span data-stu-id="a1735-2571">the final translation of which is</span></span>
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
<span data-ttu-id="a1735-2572">其中 `x` 和 `y` 是编译器生成的标识符，否则不可见且不可访问。</span><span class="sxs-lookup"><span data-stu-id="a1735-2572">where `x` and `y` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="a1735-2573">示例</span><span class="sxs-lookup"><span data-stu-id="a1735-2573">The example</span></span>
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
<span data-ttu-id="a1735-2574">具有最终翻译</span><span class="sxs-lookup"><span data-stu-id="a1735-2574">has the final translation</span></span>
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a><span data-ttu-id="a1735-2575">Select 子句</span><span class="sxs-lookup"><span data-stu-id="a1735-2575">Select clauses</span></span>

<span data-ttu-id="a1735-2576">格式为的查询表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-2576">A query expression of the form</span></span>
```csharp
from x in e select v
```
<span data-ttu-id="a1735-2577">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2577">is translated into</span></span>
```csharp
( e ) . Select ( x => v )
```
<span data-ttu-id="a1735-2578">除了 v 是标识符 x 时，转换只是</span><span class="sxs-lookup"><span data-stu-id="a1735-2578">except when v is the identifier x, the translation is simply</span></span>
```csharp
( e )
```

<span data-ttu-id="a1735-2579">例如</span><span class="sxs-lookup"><span data-stu-id="a1735-2579">For example</span></span>
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
<span data-ttu-id="a1735-2580">只转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2580">is simply translated into</span></span>
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a><span data-ttu-id="a1735-2581">Groupby 子句</span><span class="sxs-lookup"><span data-stu-id="a1735-2581">Groupby clauses</span></span>

<span data-ttu-id="a1735-2582">格式为的查询表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-2582">A query expression of the form</span></span>
```csharp
from x in e group v by k
```
<span data-ttu-id="a1735-2583">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2583">is translated into</span></span>
```csharp
( e ) . GroupBy ( x => k , x => v )
```
<span data-ttu-id="a1735-2584">除非 v 是标识符 x，否则转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2584">except when v is the identifier x, the translation is</span></span>
```csharp
( e ) . GroupBy ( x => k )
```

<span data-ttu-id="a1735-2585">示例</span><span class="sxs-lookup"><span data-stu-id="a1735-2585">The example</span></span>
```csharp
from c in customers
group c.Name by c.Country
```
<span data-ttu-id="a1735-2586">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2586">is translated into</span></span>
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a><span data-ttu-id="a1735-2587">透明标识符</span><span class="sxs-lookup"><span data-stu-id="a1735-2587">Transparent identifiers</span></span>

<span data-ttu-id="a1735-2588">某些翻译注入范围变量，其中包含由 `*`表示的***透明标识符***。</span><span class="sxs-lookup"><span data-stu-id="a1735-2588">Certain translations inject range variables with ***transparent identifiers*** denoted by `*`.</span></span> <span data-ttu-id="a1735-2589">透明标识符不是正确的语言功能;它们仅作为查询表达式转换过程中的一个中间步骤存在。</span><span class="sxs-lookup"><span data-stu-id="a1735-2589">Transparent identifiers are not a proper language feature; they exist only as an intermediate step in the query expression translation process.</span></span>

<span data-ttu-id="a1735-2590">当查询转换注入透明标识符时，进一步的转换步骤会将透明标识符传播到匿名函数和匿名对象初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="a1735-2590">When a query translation injects a transparent identifier, further translation steps propagate the transparent identifier into anonymous functions and anonymous object initializers.</span></span> <span data-ttu-id="a1735-2591">在这些上下文中，透明标识符具有以下行为：</span><span class="sxs-lookup"><span data-stu-id="a1735-2591">In those contexts, transparent identifiers have the following behavior:</span></span>

*  <span data-ttu-id="a1735-2592">当透明标识符作为匿名函数中的参数出现时，关联的匿名类型的成员将自动出现在匿名函数主体的范围内。</span><span class="sxs-lookup"><span data-stu-id="a1735-2592">When a transparent identifier occurs as a parameter in an anonymous function, the members of the associated anonymous type are automatically in scope in the body of the anonymous function.</span></span>
*  <span data-ttu-id="a1735-2593">当具有透明标识符的成员位于范围内时，该成员的成员也会在范围内。</span><span class="sxs-lookup"><span data-stu-id="a1735-2593">When a member with a transparent identifier is in scope, the members of that member are in scope as well.</span></span>
*  <span data-ttu-id="a1735-2594">当透明标识符作为匿名对象初始值设定项中的成员声明符出现时，它会引入一个具有透明标识符的成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-2594">When a transparent identifier occurs as a member declarator in an anonymous object initializer, it introduces a member with a transparent identifier.</span></span>
*  <span data-ttu-id="a1735-2595">在上面所述的翻译步骤中，透明标识符始终与匿名类型一起引入，目的是将多个范围变量捕获为单个对象的成员。</span><span class="sxs-lookup"><span data-stu-id="a1735-2595">In the translation steps described above, transparent identifiers are always introduced together with anonymous types, with the intent of capturing multiple range variables as members of a single object.</span></span> <span data-ttu-id="a1735-2596">允许实现C#使用与匿名类型不同的机制将多个范围变量组合在一起。</span><span class="sxs-lookup"><span data-stu-id="a1735-2596">An implementation of C# is permitted to use a different mechanism than anonymous types to group together multiple range variables.</span></span> <span data-ttu-id="a1735-2597">以下翻译示例假设使用匿名类型，并演示如何将透明标识符转换为。</span><span class="sxs-lookup"><span data-stu-id="a1735-2597">The following translation examples assume that anonymous types are used, and show how transparent identifiers can be translated away.</span></span>

<span data-ttu-id="a1735-2598">示例</span><span class="sxs-lookup"><span data-stu-id="a1735-2598">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
<span data-ttu-id="a1735-2599">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2599">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

<span data-ttu-id="a1735-2600">这会进一步转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2600">which is further translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
<span data-ttu-id="a1735-2601">当清除透明标识符时，它等效于</span><span class="sxs-lookup"><span data-stu-id="a1735-2601">which, when transparent identifiers are erased, is equivalent to</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
<span data-ttu-id="a1735-2602">其中 `x` 是编译器生成的标识符，否则不可见且不可访问。</span><span class="sxs-lookup"><span data-stu-id="a1735-2602">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="a1735-2603">示例</span><span class="sxs-lookup"><span data-stu-id="a1735-2603">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="a1735-2604">转换为</span><span class="sxs-lookup"><span data-stu-id="a1735-2604">is translated into</span></span>
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="a1735-2605">这会进一步减小到</span><span class="sxs-lookup"><span data-stu-id="a1735-2605">which is further reduced to</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
<span data-ttu-id="a1735-2606">的最终转换是</span><span class="sxs-lookup"><span data-stu-id="a1735-2606">the final translation of which is</span></span>
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
<span data-ttu-id="a1735-2607">其中 `x`、`y`和 `z` 是编译器生成的标识符，否则不可见且不可访问。</span><span class="sxs-lookup"><span data-stu-id="a1735-2607">where `x`, `y`, and `z` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

### <a name="the-query-expression-pattern"></a><span data-ttu-id="a1735-2608">查询表达式模式</span><span class="sxs-lookup"><span data-stu-id="a1735-2608">The query expression pattern</span></span>

<span data-ttu-id="a1735-2609">***查询表达式模式***建立了一种方法，这些方法可实现类型以支持查询表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-2609">The ***Query expression pattern*** establishes a pattern of methods that types can implement to support query expressions.</span></span> <span data-ttu-id="a1735-2610">由于查询表达式是通过语法映射转换为方法调用的，因此在实现查询表达式模式的方式上，类型具有相当大的灵活性。</span><span class="sxs-lookup"><span data-stu-id="a1735-2610">Because query expressions are translated to method invocations by means of a syntactic mapping, types have considerable flexibility in how they implement the query expression pattern.</span></span> <span data-ttu-id="a1735-2611">例如，可以将模式的方法作为实例方法或扩展方法实现，因为这两个方法具有相同的调用语法，并且方法可以请求委托或表达式树，因为匿名函数可同时转换为两者。</span><span class="sxs-lookup"><span data-stu-id="a1735-2611">For example, the methods of the pattern can be implemented as instance methods or as extension methods because the two have the same invocation syntax, and the methods can request delegates or expression trees because anonymous functions are convertible to both.</span></span>

<span data-ttu-id="a1735-2612">下面显示了支持查询表达式模式的泛型类型 `C<T>` 的推荐形状。</span><span class="sxs-lookup"><span data-stu-id="a1735-2612">The recommended shape of a generic type `C<T>` that supports the query expression pattern is shown below.</span></span> <span data-ttu-id="a1735-2613">使用泛型类型来说明参数和结果类型之间的正确关系，但也可以实现非泛型类型的模式。</span><span class="sxs-lookup"><span data-stu-id="a1735-2613">A generic type is used in order to illustrate the proper relationships between parameter and result types, but it is possible to implement the pattern for non-generic types as well.</span></span>

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

<span data-ttu-id="a1735-2614">上述方法使用 `Func<T1,R>` 和 `Func<T1,T2,R>`的泛型委托类型，但它们同样可以使用参数和结果类型中具有相同关系的其他委托或表达式树类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2614">The methods above use the generic delegate types `Func<T1,R>` and `Func<T1,T2,R>`, but they could equally well have used other delegate or expression tree types with the same relationships in parameter and result types.</span></span>

<span data-ttu-id="a1735-2615">请注意 `C<T>` 和 `O<T>` 之间的推荐关系，这可确保 `ThenBy` 和 `ThenByDescending` 方法仅对 `OrderBy` 或 `OrderByDescending`的结果可用。</span><span class="sxs-lookup"><span data-stu-id="a1735-2615">Notice the recommended relationship between `C<T>` and `O<T>` which ensures that the `ThenBy` and `ThenByDescending` methods are available only on the result of an `OrderBy` or `OrderByDescending`.</span></span> <span data-ttu-id="a1735-2616">另请注意 `GroupBy` 结果的建议形状（序列序列），其中每个内部序列都有附加的 `Key` 属性。</span><span class="sxs-lookup"><span data-stu-id="a1735-2616">Also notice the recommended shape of the result of `GroupBy` -- a sequence of sequences, where each inner sequence has an additional `Key` property.</span></span>

<span data-ttu-id="a1735-2617">`System.Linq` 命名空间为实现 `System.Collections.Generic.IEnumerable<T>` 接口的任何类型提供查询运算符模式的实现。</span><span class="sxs-lookup"><span data-stu-id="a1735-2617">The `System.Linq` namespace provides an implementation of the query operator pattern for any type that implements the `System.Collections.Generic.IEnumerable<T>` interface.</span></span>

## <a name="assignment-operators"></a><span data-ttu-id="a1735-2618">赋值运算符</span><span class="sxs-lookup"><span data-stu-id="a1735-2618">Assignment operators</span></span>

<span data-ttu-id="a1735-2619">赋值运算符将新值分配给变量、属性、事件或索引器元素。</span><span class="sxs-lookup"><span data-stu-id="a1735-2619">The assignment operators assign a new value to a variable, a property, an event, or an indexer element.</span></span>

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

<span data-ttu-id="a1735-2620">赋值运算的左操作数必须是分类为变量、属性访问、索引器访问或事件访问的表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-2620">The left operand of an assignment must be an expression classified as a variable, a property access, an indexer access, or an event access.</span></span>

<span data-ttu-id="a1735-2621">`=` 运算符称为***简单赋值运算符***。</span><span class="sxs-lookup"><span data-stu-id="a1735-2621">The `=` operator is called the ***simple assignment operator***.</span></span> <span data-ttu-id="a1735-2622">它将右操作数的值分配给左操作数给定的变量、属性或索引器元素。</span><span class="sxs-lookup"><span data-stu-id="a1735-2622">It assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="a1735-2623">简单赋值运算符的左操作数不能是事件访问（如[类似于字段的事件](classes.md#field-like-events)中所述）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2623">The left operand of the simple assignment operator may not be an event access (except as described in [Field-like events](classes.md#field-like-events)).</span></span> <span data-ttu-id="a1735-2624">简单赋值运算符在[简单赋值](expressions.md#simple-assignment)中进行了介绍。</span><span class="sxs-lookup"><span data-stu-id="a1735-2624">The simple assignment operator is described in [Simple assignment](expressions.md#simple-assignment).</span></span>

<span data-ttu-id="a1735-2625">除 `=` 运算符以外的赋值运算符称为***复合赋值运算符***。</span><span class="sxs-lookup"><span data-stu-id="a1735-2625">The assignment operators other than the `=` operator are called the ***compound assignment operators***.</span></span> <span data-ttu-id="a1735-2626">这些运算符对两个操作数执行指定的运算，然后将结果值分配给左操作数给定的变量、属性或索引器元素。</span><span class="sxs-lookup"><span data-stu-id="a1735-2626">These operators perform the indicated operation on the two operands, and then assign the resulting value to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="a1735-2627">复合赋值运算符在[复合赋值](expressions.md#compound-assignment)中介绍。</span><span class="sxs-lookup"><span data-stu-id="a1735-2627">The compound assignment operators are described in [Compound assignment](expressions.md#compound-assignment).</span></span>

<span data-ttu-id="a1735-2628">使用事件访问表达式作为左操作数的 `+=` 和 `-=` 运算符称为*事件赋值运算符*。</span><span class="sxs-lookup"><span data-stu-id="a1735-2628">The `+=` and `-=` operators with an event access expression as the left operand are called the *event assignment operators*.</span></span> <span data-ttu-id="a1735-2629">其他赋值运算符对于作为左操作数的事件访问无效。</span><span class="sxs-lookup"><span data-stu-id="a1735-2629">No other assignment operator is valid with an event access as the left operand.</span></span> <span data-ttu-id="a1735-2630">事件赋值运算符在[事件分配](expressions.md#event-assignment)中介绍。</span><span class="sxs-lookup"><span data-stu-id="a1735-2630">The event assignment operators are described in [Event assignment](expressions.md#event-assignment).</span></span>

<span data-ttu-id="a1735-2631">赋值运算符是右结合运算符，这意味着运算从右到左分组。</span><span class="sxs-lookup"><span data-stu-id="a1735-2631">The assignment operators are right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="a1735-2632">例如，形式 `a = b = c` 的表达式的计算结果为 `a = (b = c)`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2632">For example, an expression of the form `a = b = c` is evaluated as `a = (b = c)`.</span></span>

### <a name="simple-assignment"></a><span data-ttu-id="a1735-2633">简单赋值</span><span class="sxs-lookup"><span data-stu-id="a1735-2633">Simple assignment</span></span>

<span data-ttu-id="a1735-2634">`=` 运算符称为简单赋值运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2634">The `=` operator is called the simple assignment operator.</span></span>

<span data-ttu-id="a1735-2635">如果简单赋值的左操作数的形式为 `E.P` 或 `E[Ei]` `E` 具有编译时类型 `dynamic`，则分配是动态绑定的（[动态绑定](expressions.md#dynamic-binding)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2635">If the left operand of a simple assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="a1735-2636">在这种情况下，赋值表达式的编译时类型是 `dynamic`的，并且基于 `E`的运行时类型，将在运行时进行以下描述的解决方法。</span><span class="sxs-lookup"><span data-stu-id="a1735-2636">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="a1735-2637">在简单赋值中，右操作数必须是可隐式转换为左操作数类型的表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-2637">In a simple assignment, the right operand must be an expression that is implicitly convertible to the type of the left operand.</span></span> <span data-ttu-id="a1735-2638">操作将右操作数的值分配给左操作数给定的变量、属性或索引器元素。</span><span class="sxs-lookup"><span data-stu-id="a1735-2638">The operation assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span>

<span data-ttu-id="a1735-2639">简单赋值表达式的结果是赋给左操作数的值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2639">The result of a simple assignment expression is the value assigned to the left operand.</span></span> <span data-ttu-id="a1735-2640">结果与左操作数的类型相同，并且始终归类为值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2640">The result has the same type as the left operand and is always classified as a value.</span></span>

<span data-ttu-id="a1735-2641">如果左操作数是属性或索引器访问，则属性或索引器必须具有 `set` 访问器。</span><span class="sxs-lookup"><span data-stu-id="a1735-2641">If the left operand is a property or indexer access, the property or indexer must have a `set` accessor.</span></span> <span data-ttu-id="a1735-2642">如果不是这种情况，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-2642">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="a1735-2643">对形式 `x = y` 的简单赋值处理的运行时处理包括以下步骤：</span><span class="sxs-lookup"><span data-stu-id="a1735-2643">The run-time processing of a simple assignment of the form `x = y` consists of the following steps:</span></span>

*  <span data-ttu-id="a1735-2644">如果 `x` 归类为变量：</span><span class="sxs-lookup"><span data-stu-id="a1735-2644">If `x` is classified as a variable:</span></span>
   * <span data-ttu-id="a1735-2645">计算 `x` 以产生变量。</span><span class="sxs-lookup"><span data-stu-id="a1735-2645">`x` is evaluated to produce the variable.</span></span>
   * <span data-ttu-id="a1735-2646">计算 `y`，并在需要时通过隐式转换转换为 `x` 的类型（[隐式转换](conversions.md#implicit-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2646">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="a1735-2647">如果 `x` 给定的变量是*reference_type*的数组元素，则执行运行时检查，以确保为 `y` 计算的值与 `x` 为元素的数组实例兼容。</span><span class="sxs-lookup"><span data-stu-id="a1735-2647">If the variable given by `x` is an array element of a *reference_type*, a run-time check is performed to ensure that the value computed for `y` is compatible with the array instance of which `x` is an element.</span></span> <span data-ttu-id="a1735-2648">如果 `y` `null`，或者存在从 `y` 引用的实例的实际类型到包含 `x`的数组实例的实际元素类型的隐式引用转换（[隐式引用转换](conversions.md#implicit-reference-conversions)），则检查成功。</span><span class="sxs-lookup"><span data-stu-id="a1735-2648">The check succeeds if `y` is `null`, or if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the actual type of the instance referenced by `y` to the actual element type of the array instance containing `x`.</span></span> <span data-ttu-id="a1735-2649">否则，将会引发 `System.ArrayTypeMismatchException`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2649">Otherwise, a `System.ArrayTypeMismatchException` is thrown.</span></span>
   * <span data-ttu-id="a1735-2650">`y` 的计算和转换生成的值将存储到 `x`计算得出的位置。</span><span class="sxs-lookup"><span data-stu-id="a1735-2650">The value resulting from the evaluation and conversion of `y` is stored into the location given by the evaluation of `x`.</span></span>
*  <span data-ttu-id="a1735-2651">如果 `x` 归类为属性或索引器访问：</span><span class="sxs-lookup"><span data-stu-id="a1735-2651">If `x` is classified as a property or indexer access:</span></span>
   * <span data-ttu-id="a1735-2652">实例表达式（如果 `x` 不 `static`）和参数列表（如果 `x` 是索引器访问）与 `x` 关联，并在后续的 `set` 访问器调用中使用结果。</span><span class="sxs-lookup"><span data-stu-id="a1735-2652">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `set` accessor invocation.</span></span>
   * <span data-ttu-id="a1735-2653">计算 `y`，并在需要时通过隐式转换转换为 `x` 的类型（[隐式转换](conversions.md#implicit-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2653">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="a1735-2654">调用 `x` 的 `set` 访问器时，会将计算的值 `y` 为其 `value` 参数。</span><span class="sxs-lookup"><span data-stu-id="a1735-2654">The `set` accessor of `x` is invoked with the value computed for `y` as its `value` argument.</span></span>

<span data-ttu-id="a1735-2655">如果存在从 `B` 到 `A`的隐式引用转换，数组协方差规则（[数组协方](arrays.md#array-covariance)差）允许 `A[]` 为数组类型 `B[]`的实例的引用。</span><span class="sxs-lookup"><span data-stu-id="a1735-2655">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="a1735-2656">由于这些规则，对*reference_type*的数组元素赋值需要运行时检查，以确保所赋的值与数组实例兼容。</span><span class="sxs-lookup"><span data-stu-id="a1735-2656">Because of these rules, assignment to an array element of a *reference_type* requires a run-time check to ensure that the value being assigned is compatible with the array instance.</span></span> <span data-ttu-id="a1735-2657">示例中</span><span class="sxs-lookup"><span data-stu-id="a1735-2657">In the example</span></span>
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
<span data-ttu-id="a1735-2658">最后一个分配导致引发 `System.ArrayTypeMismatchException`，因为 `ArrayList` 的实例无法存储在 `string[]`的元素中。</span><span class="sxs-lookup"><span data-stu-id="a1735-2658">the last assignment causes a `System.ArrayTypeMismatchException` to be thrown because an instance of `ArrayList` cannot be stored in an element of a `string[]`.</span></span>

<span data-ttu-id="a1735-2659">如果在*struct_type*中声明的属性或索引器是赋值目标，则必须将与属性或索引器访问关联的实例表达式归类为变量。</span><span class="sxs-lookup"><span data-stu-id="a1735-2659">When a property or indexer declared in a *struct_type* is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="a1735-2660">如果实例表达式归类为值，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-2660">If the instance expression is classified as a value, a binding-time error occurs.</span></span> <span data-ttu-id="a1735-2661">由于[成员访问](expressions.md#member-access)，相同的规则也适用于字段。</span><span class="sxs-lookup"><span data-stu-id="a1735-2661">Because of [Member access](expressions.md#member-access), the same rule also applies to fields.</span></span>

<span data-ttu-id="a1735-2662">给定以下声明：</span><span class="sxs-lookup"><span data-stu-id="a1735-2662">Given the declarations:</span></span>
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
<span data-ttu-id="a1735-2663">示例中</span><span class="sxs-lookup"><span data-stu-id="a1735-2663">in the example</span></span>
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
<span data-ttu-id="a1735-2664">允许分配到 `p.X`、`p.Y`、`r.A`和 `r.B`，因为 `p` 和 `r` 都是变量。</span><span class="sxs-lookup"><span data-stu-id="a1735-2664">the assignments to `p.X`, `p.Y`, `r.A`, and `r.B` are permitted because `p` and `r` are variables.</span></span> <span data-ttu-id="a1735-2665">但在示例中</span><span class="sxs-lookup"><span data-stu-id="a1735-2665">However, in the example</span></span>
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
<span data-ttu-id="a1735-2666">分配均无效，因为 `r.A` 和 `r.B` 不是变量。</span><span class="sxs-lookup"><span data-stu-id="a1735-2666">the assignments are all invalid, since `r.A` and `r.B` are not variables.</span></span>

### <a name="compound-assignment"></a><span data-ttu-id="a1735-2667">复合赋值</span><span class="sxs-lookup"><span data-stu-id="a1735-2667">Compound assignment</span></span>

<span data-ttu-id="a1735-2668">如果复合赋值的左操作数的形式为 `E.P` 或 `E[Ei]` `E` 具有编译时类型 `dynamic`，则分配是动态绑定的（[动态绑定](expressions.md#dynamic-binding)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2668">If the left operand of a compound assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="a1735-2669">在这种情况下，赋值表达式的编译时类型是 `dynamic`的，并且基于 `E`的运行时类型，将在运行时进行以下描述的解决方法。</span><span class="sxs-lookup"><span data-stu-id="a1735-2669">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="a1735-2670">通过应用二元运算符重载决策（[二元运算符重载决策](expressions.md#binary-operator-overload-resolution)）来处理窗体 `x op= y` 的操作，就像将操作写入 `x op y`一样。</span><span class="sxs-lookup"><span data-stu-id="a1735-2670">An operation of the form `x op= y` is processed by applying binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x op y`.</span></span> <span data-ttu-id="a1735-2671">那么：</span><span class="sxs-lookup"><span data-stu-id="a1735-2671">Then,</span></span>

*  <span data-ttu-id="a1735-2672">如果所选运算符的返回类型可隐式转换为 `x`的类型，则该运算将计算为 `x = x op y`，只不过 `x` 只计算一次。</span><span class="sxs-lookup"><span data-stu-id="a1735-2672">If the return type of the selected operator is implicitly convertible to the type of `x`, the operation is evaluated as `x = x op y`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="a1735-2673">否则，如果所选运算符是预定义的运算符，则如果所选运算符的返回类型可显式转换为 `x`的类型，并且如果 `y` 可隐式转换为 `x` 类型或运算符为移位运算符，则该运算将计算为 `x = (T)(x op y)`，其中 `T` 是 `x`的类型，但 `x` 只计算一次。</span><span class="sxs-lookup"><span data-stu-id="a1735-2673">Otherwise, if the selected operator is a predefined operator, if the return type of the selected operator is explicitly convertible to the type of `x`, and if `y` is implicitly convertible to the type of `x` or the operator is a shift operator, then the operation is evaluated as `x = (T)(x op y)`, where `T` is the type of `x`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="a1735-2674">否则，复合分配无效，并发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-2674">Otherwise, the compound assignment is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="a1735-2675">术语 "仅计算一次" 表示在 `x op y`的计算中，将临时保存 `x` 的所有构成表达式的结果，并在对 `x`执行赋值时重用。</span><span class="sxs-lookup"><span data-stu-id="a1735-2675">The term "evaluated only once" means that in the evaluation of `x op y`, the results of any constituent expressions of `x` are temporarily saved and then reused when performing the assignment to `x`.</span></span> <span data-ttu-id="a1735-2676">例如，在赋值 `A()[B()] += C()`中，其中 `A` 是返回 `int[]`的方法，并且 `B` 和 `C` 是返回 `int`的方法，这些方法只调用一次，并按顺序 `A``B`、`C`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2676">For example, in the assignment `A()[B()] += C()`, where `A` is a method returning `int[]`, and `B` and `C` are methods returning `int`, the methods are invoked only once, in the order `A`, `B`, `C`.</span></span>

<span data-ttu-id="a1735-2677">当复合赋值的左操作数是属性访问或索引器访问时，属性或索引器必须同时具有 `get` 访问器和 `set` 访问器。</span><span class="sxs-lookup"><span data-stu-id="a1735-2677">When the left operand of a compound assignment is a property access or indexer access, the property or indexer must have both a `get` accessor and a `set` accessor.</span></span> <span data-ttu-id="a1735-2678">如果不是这种情况，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-2678">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="a1735-2679">上面的第二个规则允许 `x op= y` 在某些上下文中计算为 `x = (T)(x op y)`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2679">The second rule above permits `x op= y` to be evaluated as `x = (T)(x op y)` in certain contexts.</span></span> <span data-ttu-id="a1735-2680">存在该规则，以便当左操作数的类型为 `sbyte`、`byte`、`short`、`ushort`或 `char`时，预定义运算符可用作复合运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2680">The rule exists such that the predefined operators can be used as compound operators when the left operand is of type `sbyte`, `byte`, `short`, `ushort`, or `char`.</span></span> <span data-ttu-id="a1735-2681">即使两个参数都属于这些类型之一，预定义运算符也会生成 `int`类型的结果，如[二进制数值升级](expressions.md#binary-numeric-promotions)中所述。</span><span class="sxs-lookup"><span data-stu-id="a1735-2681">Even when both arguments are of one of those types, the predefined operators produce a result of type `int`, as described in [Binary numeric promotions](expressions.md#binary-numeric-promotions).</span></span> <span data-ttu-id="a1735-2682">因此，在没有强制转换的情况下，不能将结果赋给左操作数。</span><span class="sxs-lookup"><span data-stu-id="a1735-2682">Thus, without a cast it would not be possible to assign the result to the left operand.</span></span>

<span data-ttu-id="a1735-2683">预定义运算符规则的直观效果只是允许 `x op y` 和 `x = y` 都允许 `x op= y`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2683">The intuitive effect of the rule for predefined operators is simply that `x op= y` is permitted if both of `x op y` and `x = y` are permitted.</span></span> <span data-ttu-id="a1735-2684">示例中</span><span class="sxs-lookup"><span data-stu-id="a1735-2684">In the example</span></span>
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
<span data-ttu-id="a1735-2685">每个错误的直观原因在于，相应的简单分配也会导致错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-2685">the intuitive reason for each error is that a corresponding simple assignment would also have been an error.</span></span>

<span data-ttu-id="a1735-2686">这也意味着复合赋值操作支持提升的操作。</span><span class="sxs-lookup"><span data-stu-id="a1735-2686">This also means that compound assignment operations support lifted operations.</span></span> <span data-ttu-id="a1735-2687">示例中</span><span class="sxs-lookup"><span data-stu-id="a1735-2687">In the example</span></span>
```csharp
int? i = 0;
i += 1;             // Ok
```
<span data-ttu-id="a1735-2688">使用提升运算符 `+(int?,int?)`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2688">the lifted operator `+(int?,int?)` is used.</span></span>

### <a name="event-assignment"></a><span data-ttu-id="a1735-2689">事件分配</span><span class="sxs-lookup"><span data-stu-id="a1735-2689">Event assignment</span></span>

<span data-ttu-id="a1735-2690">如果 `+=` 或 `-=` 运算符的左操作数分类为事件访问，则表达式的计算方式如下：</span><span class="sxs-lookup"><span data-stu-id="a1735-2690">If the left operand of a `+=` or `-=` operator is classified as an event access, then the expression is evaluated as follows:</span></span>

*  <span data-ttu-id="a1735-2691">计算事件访问的实例表达式（如果有）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2691">The instance expression, if any, of the event access is evaluated.</span></span>
*  <span data-ttu-id="a1735-2692">计算 `+=` 或 `-=` 运算符的右操作数，并在需要时通过隐式转换转换为左操作数的类型（[隐式](conversions.md#implicit-conversions)转换）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2692">The right operand of the `+=` or `-=` operator is evaluated, and, if required, converted to the type of the left operand through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
*  <span data-ttu-id="a1735-2693">调用事件的事件访问器，其中包含右操作数（在计算后，如有必要）转换后的参数列表。</span><span class="sxs-lookup"><span data-stu-id="a1735-2693">An event accessor of the event is invoked, with argument list consisting of the right operand, after evaluation and, if necessary, conversion.</span></span> <span data-ttu-id="a1735-2694">如果运算符是 `+=`的，则调用 `add` 访问器;如果运算符是 `-=`的，则调用 `remove` 访问器。</span><span class="sxs-lookup"><span data-stu-id="a1735-2694">If the operator was `+=`, the `add` accessor is invoked; if the operator was `-=`, the `remove` accessor is invoked.</span></span>

<span data-ttu-id="a1735-2695">事件赋值表达式不生成值。</span><span class="sxs-lookup"><span data-stu-id="a1735-2695">An event assignment expression does not yield a value.</span></span> <span data-ttu-id="a1735-2696">因此，事件分配表达式仅在*statement_expression* （[expression 语句](statements.md#expression-statements)）的上下文中有效。</span><span class="sxs-lookup"><span data-stu-id="a1735-2696">Thus, an event assignment expression is valid only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

## <a name="expression"></a><span data-ttu-id="a1735-2697">表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-2697">Expression</span></span>

<span data-ttu-id="a1735-2698">*表达式*可以是*non_assignment_expression*或*赋值*。</span><span class="sxs-lookup"><span data-stu-id="a1735-2698">An *expression* is either a *non_assignment_expression* or an *assignment*.</span></span>

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

## <a name="constant-expressions"></a><span data-ttu-id="a1735-2699">常量表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-2699">Constant expressions</span></span>

<span data-ttu-id="a1735-2700">*Constant_expression*是可以在编译时完全计算的表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-2700">A *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span>

```antlr
constant_expression
    : expression
    ;
```

<span data-ttu-id="a1735-2701">常数表达式必须是 `null` 文本或具有以下类型之一的值： `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`、`bool`、`object`、`string`或任何枚举类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2701">A constant expression must be the `null` literal or a value with one of  the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`, or any enumeration type.</span></span> <span data-ttu-id="a1735-2702">常量表达式中仅允许使用以下构造：</span><span class="sxs-lookup"><span data-stu-id="a1735-2702">Only the following constructs are permitted in constant expressions:</span></span>

*  <span data-ttu-id="a1735-2703">文本（包括 `null` 文本）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2703">Literals (including the `null` literal).</span></span>
*  <span data-ttu-id="a1735-2704">对类类型和结构类型 `const` 成员的引用。</span><span class="sxs-lookup"><span data-stu-id="a1735-2704">References to `const` members of class and struct types.</span></span>
*  <span data-ttu-id="a1735-2705">对枚举类型的成员的引用。</span><span class="sxs-lookup"><span data-stu-id="a1735-2705">References to members of enumeration types.</span></span>
*  <span data-ttu-id="a1735-2706">对 `const` 参数或局部变量的引用</span><span class="sxs-lookup"><span data-stu-id="a1735-2706">References to `const` parameters or local variables</span></span>
*  <span data-ttu-id="a1735-2707">带括号的子表达式，它们本身就是常量表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-2707">Parenthesized sub-expressions, which are themselves constant expressions.</span></span>
*  <span data-ttu-id="a1735-2708">如果目标类型是上面列出的类型之一，则强制转换表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-2708">Cast expressions, provided the target type is one of the types listed above.</span></span>
*  <span data-ttu-id="a1735-2709">`checked` 和 `unchecked` 表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-2709">`checked` and `unchecked` expressions</span></span>
*  <span data-ttu-id="a1735-2710">默认值表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-2710">Default value expressions</span></span>
*  <span data-ttu-id="a1735-2711">Nameof 表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-2711">Nameof expressions</span></span>
*  <span data-ttu-id="a1735-2712">预定义的 `+`、`-`、`!`和 `~` 一元运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2712">The predefined `+`, `-`, `!`, and `~` unary operators.</span></span>
*  <span data-ttu-id="a1735-2713">预定义的 `+`、`-`、`*`、`/`、`%`、`<<`、`>>`、`&`、`|`、`^`、`&&`、`||`、`==`、`!=`、`<`、`>`、`<=`和 `>=` 二进制运算符，前提是每个操作数都是上面列出的类型。</span><span class="sxs-lookup"><span data-stu-id="a1735-2713">The predefined `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, and `>=` binary operators, provided each operand is of a type listed above.</span></span>
*  <span data-ttu-id="a1735-2714">`?:` 条件运算符。</span><span class="sxs-lookup"><span data-stu-id="a1735-2714">The `?:` conditional operator.</span></span>

<span data-ttu-id="a1735-2715">常数表达式中允许以下转换：</span><span class="sxs-lookup"><span data-stu-id="a1735-2715">The following conversions are permitted in constant expressions:</span></span>

*  <span data-ttu-id="a1735-2716">标识转换</span><span class="sxs-lookup"><span data-stu-id="a1735-2716">Identity conversions</span></span>
*  <span data-ttu-id="a1735-2717">数值转换</span><span class="sxs-lookup"><span data-stu-id="a1735-2717">Numeric conversions</span></span>
*  <span data-ttu-id="a1735-2718">枚举转换</span><span class="sxs-lookup"><span data-stu-id="a1735-2718">Enumeration conversions</span></span>
*  <span data-ttu-id="a1735-2719">常数表达式转换</span><span class="sxs-lookup"><span data-stu-id="a1735-2719">Constant expression conversions</span></span>
*  <span data-ttu-id="a1735-2720">隐式和显式引用转换，前提是转换的源是计算结果为 null 值的常量表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-2720">Implicit and explicit reference conversions, provided that the source of the conversions is a constant expression that evaluates to the null value.</span></span>

<span data-ttu-id="a1735-2721">不允许在常数表达式中使用其他转换，包括非 null 值的装箱、取消装箱和隐式引用转换。</span><span class="sxs-lookup"><span data-stu-id="a1735-2721">Other conversions including boxing, unboxing and implicit reference conversions of non-null values are not permitted in constant expressions.</span></span> <span data-ttu-id="a1735-2722">例如：</span><span class="sxs-lookup"><span data-stu-id="a1735-2722">For example:</span></span>
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
<span data-ttu-id="a1735-2723">i 的初始化是错误的，因为需要装箱转换。</span><span class="sxs-lookup"><span data-stu-id="a1735-2723">the initialization of i is an error because a boxing conversion is required.</span></span> <span data-ttu-id="a1735-2724">Str 的初始化是错误的，因为需要从非 null 值进行隐式引用转换。</span><span class="sxs-lookup"><span data-stu-id="a1735-2724">The initialization of str is an error because an implicit reference conversion from a non-null value is required.</span></span>

<span data-ttu-id="a1735-2725">只要表达式满足上面列出的要求，就会在编译时计算表达式。</span><span class="sxs-lookup"><span data-stu-id="a1735-2725">Whenever an expression fulfills the requirements listed above, the expression is evaluated at compile-time.</span></span> <span data-ttu-id="a1735-2726">即使表达式是包含非常量构造的更大的表达式的子表达式，也是如此。</span><span class="sxs-lookup"><span data-stu-id="a1735-2726">This is true even if the expression is a sub-expression of a larger expression that contains non-constant constructs.</span></span>

<span data-ttu-id="a1735-2727">常数表达式的编译时计算使用与非常量表达式的运行时计算相同的规则，只不过运行时计算会引发异常，编译时计算会导致发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-2727">The compile-time evaluation of constant expressions uses the same rules as run-time evaluation of non-constant expressions, except that where run-time evaluation would have thrown an exception, compile-time evaluation causes a compile-time error to occur.</span></span>

<span data-ttu-id="a1735-2728">除非将常量表达式显式置于 `unchecked` 的上下文中，否则在表达式的编译时计算过程中发生的溢出将始终导致编译时错误（[常量表达式](expressions.md#constant-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2728">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur in integral-type arithmetic operations and conversions during the compile-time evaluation of the expression always cause compile-time errors ([Constant expressions](expressions.md#constant-expressions)).</span></span>

<span data-ttu-id="a1735-2729">常数表达式出现在下面列出的上下文中。</span><span class="sxs-lookup"><span data-stu-id="a1735-2729">Constant expressions occur in the contexts listed below.</span></span> <span data-ttu-id="a1735-2730">在这些上下文中，如果表达式在编译时无法完全计算，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-2730">In these contexts, a compile-time error occurs if an expression cannot be fully evaluated at compile-time.</span></span>

*  <span data-ttu-id="a1735-2731">常量声明（[常量](classes.md#constants)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2731">Constant declarations ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="a1735-2732">枚举成员声明（[枚举成员](enums.md#enum-members)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2732">Enumeration member declarations ([Enum members](enums.md#enum-members)).</span></span>
*  <span data-ttu-id="a1735-2733">形参表的默认参数（[方法参数](classes.md#method-parameters)）</span><span class="sxs-lookup"><span data-stu-id="a1735-2733">Default arguments of formal parameter lists ([Method parameters](classes.md#method-parameters))</span></span>
*  <span data-ttu-id="a1735-2734">`switch` 语句 `case` 标签（[switch 语句](statements.md#the-switch-statement)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2734">`case` labels of a `switch` statement ([The switch statement](statements.md#the-switch-statement)).</span></span>
*  <span data-ttu-id="a1735-2735">`goto case` 语句（[goto 语句](statements.md#the-goto-statement)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2735">`goto case` statements ([The goto statement](statements.md#the-goto-statement)).</span></span>
*  <span data-ttu-id="a1735-2736">数组创建表达式中的维度长度（[数组创建表达式](expressions.md#array-creation-expressions)），其中包含初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="a1735-2736">Dimension lengths in an array creation expression ([Array creation expressions](expressions.md#array-creation-expressions)) that includes an initializer.</span></span>
*  <span data-ttu-id="a1735-2737">特性（[特性](attributes.md)）。</span><span class="sxs-lookup"><span data-stu-id="a1735-2737">Attributes ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="a1735-2738">如果常量表达式的值在目标类型的范围内，则隐式常量表达式转换（[隐式常数表达式转换](conversions.md#implicit-constant-expression-conversions)）允许 `int` 类型的常量表达式转换为 `sbyte`、`byte`、`short`、`ushort`、`uint`或 `ulong`。</span><span class="sxs-lookup"><span data-stu-id="a1735-2738">An implicit constant expression conversion ([Implicit constant expression conversions](conversions.md#implicit-constant-expression-conversions)) permits a constant expression of type `int` to be converted to `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the constant expression is within the range of the destination type.</span></span>

## <a name="boolean-expressions"></a><span data-ttu-id="a1735-2739">Boolean 表达式</span><span class="sxs-lookup"><span data-stu-id="a1735-2739">Boolean expressions</span></span>

<span data-ttu-id="a1735-2740">*Boolean_expression*是生成 `bool`类型的结果的表达式;直接或通过在某些上下文中 `operator true` 应用程序，如以下所示。</span><span class="sxs-lookup"><span data-stu-id="a1735-2740">A *boolean_expression* is an expression that yields a result of type `bool`; either directly or through application of `operator true` in certain contexts as specified in the following.</span></span>

```antlr
boolean_expression
    : expression
    ;
```

<span data-ttu-id="a1735-2741">*If_statement* （[if 语句](statements.md#the-if-statement)）、 *while_statement* （[while 语句](statements.md#the-while-statement)）、 *do_statement* （[do 语句](statements.md#the-do-statement)）或*for_statement* （[for 语句](statements.md#the-for-statement)）的控制条件表达式为*boolean_expression*。</span><span class="sxs-lookup"><span data-stu-id="a1735-2741">The controlling conditional expression of an *if_statement* ([The if statement](statements.md#the-if-statement)), *while_statement* ([The while statement](statements.md#the-while-statement)), *do_statement* ([The do statement](statements.md#the-do-statement)), or *for_statement* ([The for statement](statements.md#the-for-statement)) is a *boolean_expression*.</span></span> <span data-ttu-id="a1735-2742">`?:` 运算符（[条件运算符](expressions.md#conditional-operator)）的控制条件表达式遵循与*boolean_expression*相同的规则，但由于运算符优先级的原因，将归类为*conditional_or_expression*。</span><span class="sxs-lookup"><span data-stu-id="a1735-2742">The controlling conditional expression of the `?:` operator ([Conditional operator](expressions.md#conditional-operator)) follows the same rules as a *boolean_expression*, but for reasons of operator precedence is classified as a *conditional_or_expression*.</span></span>

<span data-ttu-id="a1735-2743">需要*boolean_expression* `E` 才能生成类型 `bool`的值，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a1735-2743">A *boolean_expression* `E` is required to be able to produce a value of type `bool`, as follows:</span></span>

*  <span data-ttu-id="a1735-2744">如果 `E` 可隐式转换为 `bool` 则在运行时将应用隐式转换。</span><span class="sxs-lookup"><span data-stu-id="a1735-2744">If `E` is implicitly convertible to `bool` then at runtime that implicit conversion is applied.</span></span>
*  <span data-ttu-id="a1735-2745">否则，一元运算符重载决策（[一元运算符重载决策](expressions.md#unary-operator-overload-resolution)）用于查找 `E`上运算符 `true` 的唯一最佳实现，并且该实现在运行时应用。</span><span class="sxs-lookup"><span data-stu-id="a1735-2745">Otherwise, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is used to find a unique best implementation of operator `true` on `E`, and that implementation is applied at runtime.</span></span>
*  <span data-ttu-id="a1735-2746">如果找不到这样的运算符，则发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="a1735-2746">If no such operator is found, a binding-time error occurs.</span></span>

<span data-ttu-id="a1735-2747">[数据库布尔类型](structs.md#database-boolean-type)中 `DBBool` 结构类型提供了实现 `operator true` 和 `operator false`的类型的示例。</span><span class="sxs-lookup"><span data-stu-id="a1735-2747">The `DBBool` struct type in [Database boolean type](structs.md#database-boolean-type) provides an example of a type that implements `operator true` and `operator false`.</span></span>
