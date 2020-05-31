---
ms.openlocfilehash: 49720d62c496ff904eacacb31ae29ef97a5aefaa
ms.sourcegitcommit: ae114131069ca76e4a1ec8149b7bab81fce8965c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/30/2020
ms.locfileid: "84228189"
---
# <a name="records"></a><span data-ttu-id="2ddd1-101">records</span><span class="sxs-lookup"><span data-stu-id="2ddd1-101">records</span></span>

* <span data-ttu-id="2ddd1-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="2ddd1-102">[x] Proposed</span></span>
* <span data-ttu-id="2ddd1-103">[] 原型：[完成](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)</span><span class="sxs-lookup"><span data-stu-id="2ddd1-103">[ ] Prototype: [Complete](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)</span></span>
* <span data-ttu-id="2ddd1-104">[] 实现：[正在进行](https://github.com/dotnet/roslyn/BRANCH_NAME)</span><span class="sxs-lookup"><span data-stu-id="2ddd1-104">[ ] Implementation: [In Progress](https://github.com/dotnet/roslyn/BRANCH_NAME)</span></span>
* <span data-ttu-id="2ddd1-105">[] 规范：包含的草稿规范</span><span class="sxs-lookup"><span data-stu-id="2ddd1-105">[ ] Specification: Draft specification enclosed</span></span>

## <a name="summary"></a><span data-ttu-id="2ddd1-106">总结</span><span class="sxs-lookup"><span data-stu-id="2ddd1-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="2ddd1-107">记录是 c # 类和结构类型的一个新的简化声明窗体，它们结合了许多更简单的功能的优点。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-107">Records are a new, simplified declaration form for C# class and struct types that combine the benefits of a number of simpler features.</span></span> <span data-ttu-id="2ddd1-108">我们介绍了新功能（调用方-接收方参数和*带有表达式*的），为记录声明提供语法和语义，并提供一些示例。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-108">We describe the new features (caller-receiver parameters and *with-expression*s), give the syntax and semantics for record declarations, and then provide some examples.</span></span>


## <a name="motivation"></a><span data-ttu-id="2ddd1-109">动机</span><span class="sxs-lookup"><span data-stu-id="2ddd1-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="2ddd1-110">在 c # 中，类型声明的数量很少比类型化数据的聚合集合少。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-110">A significant number of type declarations in C# are little more than aggregate collections of typed data.</span></span> <span data-ttu-id="2ddd1-111">遗憾的是，声明此类类型需要大量的样板代码。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-111">Unfortunately, declaring such types requires a great deal of boilerplate code.</span></span> <span data-ttu-id="2ddd1-112">*记录*提供了一种机制，用于通过描述聚合的成员以及其他代码或与常用样本（如果有）的偏差来声明数据类型。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-112">*Records* provide a mechanism for declaring a datatype by describing the members of the aggregate along with additional code or deviations from the usual boilerplate, if any.</span></span>

<span data-ttu-id="2ddd1-113">请参阅下面的[示例](#examples)。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-113">See [Examples](#examples), below.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="2ddd1-114">详细设计</span><span class="sxs-lookup"><span data-stu-id="2ddd1-114">Detailed design</span></span>
[design]: #detailed-design

### <a name="caller-receiver-parameters"></a><span data-ttu-id="2ddd1-115">调用方-接收方参数</span><span class="sxs-lookup"><span data-stu-id="2ddd1-115">caller-receiver parameters</span></span>

<span data-ttu-id="2ddd1-116">当前方法参数的*默认参数*必须是</span><span class="sxs-lookup"><span data-stu-id="2ddd1-116">Currently a method parameter's *default-argument* must be</span></span> 
- <span data-ttu-id="2ddd1-117">*常数表达式*;或</span><span class="sxs-lookup"><span data-stu-id="2ddd1-117">a *constant-expression*; or</span></span>
- <span data-ttu-id="2ddd1-118">格式为的表达式， `new S()` 其中 `S` 是值类型; 或</span><span class="sxs-lookup"><span data-stu-id="2ddd1-118">an expression of the form `new S()` where `S` is a value type; or</span></span>
- <span data-ttu-id="2ddd1-119">格式为的表达式， `default(S)` 其中 `S` 是值类型</span><span class="sxs-lookup"><span data-stu-id="2ddd1-119">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="2ddd1-120">我们对此进行了扩展，以添加以下</span><span class="sxs-lookup"><span data-stu-id="2ddd1-120">We extend this to add the following</span></span>
- <span data-ttu-id="2ddd1-121">格式为的表达式`this.Identifier`</span><span class="sxs-lookup"><span data-stu-id="2ddd1-121">an expression of the form `this.Identifier`</span></span>

<span data-ttu-id="2ddd1-122">此新格式称为*调用方-接收方默认参数*，仅当满足以下所有条件时才允许使用</span><span class="sxs-lookup"><span data-stu-id="2ddd1-122">This new form is called a *caller-receiver default-argument*, and is allowed only if all of the following are satisfied</span></span>
- <span data-ttu-id="2ddd1-123">它在其中出现的方法是实例方法;与</span><span class="sxs-lookup"><span data-stu-id="2ddd1-123">The method in which it appears is an instance method; and</span></span>
- <span data-ttu-id="2ddd1-124">表达式 `this.Identifier` 绑定到封闭类型的实例成员，该成员必须是字段或属性; 并且</span><span class="sxs-lookup"><span data-stu-id="2ddd1-124">The expression `this.Identifier` binds to an instance member of the enclosing type, which must be either a field or a property; and</span></span>
- <span data-ttu-id="2ddd1-125">它绑定到的成员（ `get` 如果它是属性，则为访问器）至少可作为方法访问; 和</span><span class="sxs-lookup"><span data-stu-id="2ddd1-125">The member to which it binds (and the `get` accessor, if it is a property) is at least as accessible as the method; and</span></span>
- <span data-ttu-id="2ddd1-126">的类型可 `this.Identifier` 通过标识或可为 null 的转换隐式转换为参数的类型（这是对*默认参数*的现有约束）。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-126">The type of `this.Identifier` is implicitly convertible by an identity or nullable conversion to the type of the parameter (this is an existing constraint on *default-argument*).</span></span>

<span data-ttu-id="2ddd1-127">当使用*调用方-接收方默认参数*的对应可选参数的函数成员调用省略参数时，将隐式传递接收方成员的值。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-127">When an argument is omitted from an invocation of a function member for a corresponding optional parameter with a *caller-receiver default-argument*, the value of the receiver's member is implicitly passed.</span></span> 

> <span data-ttu-id="2ddd1-128">**设计说明**：调用方参数的主要原因是为了支持*with 表达式*。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-128">**Design Notes**: the main reason for the caller-receiver parameter is to support the *with-expression*.</span></span> <span data-ttu-id="2ddd1-129">其思路是可以声明如下所示的方法</span><span class="sxs-lookup"><span data-stu-id="2ddd1-129">The idea is that you can declare a method like this</span></span>
> ```cs
> class Point
> {
>     public readonly int X;
>     public readonly int Y;
>     public Point With(int x = this.X, int y = this.Y) => new Point(x, y);
>     // etc
> }
> ```
> <span data-ttu-id="2ddd1-130">然后按如下所示使用它</span><span class="sxs-lookup"><span data-stu-id="2ddd1-130">and then use it like this</span></span>
> ```cs
>     Point p = new Point(3, 4);
>     p = p.With(x: 1);
> ```
> <span data-ttu-id="2ddd1-131">如果为，则创建一个新的， `Point` `Point` 它与已更改的值相同 `X` 。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-131">To create a new `Point` just like an existing `Point` but with the value of `X` changed.</span></span>
> 
> <span data-ttu-id="2ddd1-132">这是一个打开的问题，无论是否值得添加*表达式*的语法形式的调用方-接收方参数，都可以执行此操作，*而*不是*in addition to* *使用 with 表达式*。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-132">It is an open question whether or not the syntactic form of the *with-expression* is worth adding once we have support for caller-receiver parameters, so it is possible we would do this *instead of* rather than *in addition to* the *with-expression*.</span></span>

- <span data-ttu-id="2ddd1-133">[]**打开问题**：计算*调用方-接收方默认参数*的顺序与其他参数的计算顺序是什么？</span><span class="sxs-lookup"><span data-stu-id="2ddd1-133">[ ] **Open issue**: What is the order in which a *caller-receiver default-argument* is evaluated with respect to other arguments?</span></span> <span data-ttu-id="2ddd1-134">是否应指出它未指定？</span><span class="sxs-lookup"><span data-stu-id="2ddd1-134">Should we say that it is unspecified?</span></span>

### <a name="with-expressions"></a><span data-ttu-id="2ddd1-135">with-表达式</span><span class="sxs-lookup"><span data-stu-id="2ddd1-135">with-expressions</span></span>

<span data-ttu-id="2ddd1-136">建议使用新的表达式格式：</span><span class="sxs-lookup"><span data-stu-id="2ddd1-136">A new expression form is proposed:</span></span>

```antlr
primary_expression
    : with_expression
    ;

with_expression
    : primary_expression 'with' '{' with_initializer_list '}'
    ;

with_initializer_list
    : with_initializer
    | with_initializer ',' with_initializer_list
    ;

with_initializer
    : identifier '=' expression
    ;
```

<span data-ttu-id="2ddd1-137">标记 `with` 是一个新的上下文相关的关键字。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-137">The token `with` is a new context-sensitive keyword.</span></span>

<span data-ttu-id="2ddd1-138">*With_initializer*左侧的每个*标识符*都必须绑定到*with_expression*的*primary_expression*类型的可访问实例字段或属性。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-138">Each *identifier* on the left of a *with_initializer* must bind to an accessible instance field or property of the type of the *primary_expression* of the *with_expression*.</span></span> <span data-ttu-id="2ddd1-139">给定*with_expression*的这些标识符中不能有重复的名称。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-139">There may be no duplicated name among these identifiers of a given *with_expression*.</span></span>

<span data-ttu-id="2ddd1-140">窗体的*with_expression*</span><span class="sxs-lookup"><span data-stu-id="2ddd1-140">A *with_expression* of the form</span></span>

> <span data-ttu-id="2ddd1-141">*e1* `with``{`*标识符*  =  *e2*，.。。`}`</span><span class="sxs-lookup"><span data-stu-id="2ddd1-141">*e1* `with` `{` *identifier* = *e2*, ... `}`</span></span>

<span data-ttu-id="2ddd1-142">被视为对窗体的调用</span><span class="sxs-lookup"><span data-stu-id="2ddd1-142">is treated as an invocation of the form</span></span>

> <span data-ttu-id="2ddd1-143">*e1* `.With(`*identifier2* `:`*e2*，.。。`)`</span><span class="sxs-lookup"><span data-stu-id="2ddd1-143">*e1*`.With(`*identifier2*`:` *e2*, ...`)`</span></span>

<span data-ttu-id="2ddd1-144">其中，对于名为 `With` *e1*的可访问实例成员的每个方法，我们选择*identifier2*作为该方法中第一个参数的名称，该方法具有与实例字段或绑定到*标识符*的属性相同的成员。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-144">Where, for each method named `With` that is an accessible instance member of *e1*, we select *identifier2* as the name of the first parameter in that method that has a caller-receiver parameter that is the same member as the instance field or property bound to *identifier*.</span></span> <span data-ttu-id="2ddd1-145">如果无法识别此类参数，则不需要考虑方法。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-145">If no such parameter can be identified that method is eliminated from consideration.</span></span> <span data-ttu-id="2ddd1-146">要调用的方法是从其余候选项的重载决策中选择的。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-146">The method to be invoked is selected from among the remaining candidates by overload resolution.</span></span>

> <span data-ttu-id="2ddd1-147">**设计说明**：如果调用方是接收方参数，则无需此特殊语法形式即可使用*带有表达式*的许多优点。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-147">**Design Notes**: Given caller-receiver parameters, many of the benefits of the *with-expression* are available without this special syntax form.</span></span> <span data-ttu-id="2ddd1-148">因此，我们考虑是否需要此方法。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-148">We are therefore considering whether or not it is needed.</span></span> <span data-ttu-id="2ddd1-149">它的主要好处是允许在字段和属性的名称（而不是参数名称）中进行编程。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-149">Its main benefit is allowing one to program in terms of the names of fields and properties, rather than in terms of the names of parameters.</span></span> <span data-ttu-id="2ddd1-150">这样，我们就可以提高可读性和工具质量（例如，对*with_expression*的标识符进行的定义将导航到属性而不是方法参数）。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-150">In this way we improve both readability and the quality of tooling (e.g. go-to-definition on the identifier of a *with_expression* would navigate to the property rather than to a method parameter).</span></span>

- <span data-ttu-id="2ddd1-151">[]**打开问题**：应修改此描述以支持扩展方法。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-151">[ ] **Open issue**: This description should be modified to support extension methods.</span></span>

### <a name="pattern-matching"></a><span data-ttu-id="2ddd1-152">模式匹配</span><span class="sxs-lookup"><span data-stu-id="2ddd1-152">pattern-matching</span></span>

<span data-ttu-id="2ddd1-153">查看规范的[模式匹配规范](csharp-8.0/patterns.md#positional-pattern) `Deconstruct` 及其与模式匹配的关系。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-153">See the [Pattern Matching Specification](csharp-8.0/patterns.md#positional-pattern) for a specification of `Deconstruct` and its relationship to pattern-matching.</span></span>

> <span data-ttu-id="2ddd1-154">**设计说明**：根据此处指定的编译器生成的 `Deconstruct` ，以及用于模式匹配的规范，记录声明</span><span class="sxs-lookup"><span data-stu-id="2ddd1-154">**Design Notes**: By virtue of the compiler-generated `Deconstruct` as specified herein, and the specification for pattern-matching, a record declaration</span></span>
> ```cs
> public class Point(int X, int Y);
> ```
> <span data-ttu-id="2ddd1-155">将支持位置模式匹配，如下所示</span><span class="sxs-lookup"><span data-stu-id="2ddd1-155">will support positional pattern-matching as follows</span></span>
> ```cs
> Point p = new Point(3, 4);
> if (p is Point(3, var y)) { // if X is 3
>     Console.WriteLine(y);   // print Y
> }
> ```

### <a name="record-type-declarations"></a><span data-ttu-id="2ddd1-156">记录类型声明</span><span class="sxs-lookup"><span data-stu-id="2ddd1-156">record type declarations</span></span>

<span data-ttu-id="2ddd1-157">或声明的语法 `class` `struct` 已扩展为支持值参数; 参数将变为类型为的属性：</span><span class="sxs-lookup"><span data-stu-id="2ddd1-157">The syntax for a `class` or `struct` declaration is extended to support value parameters; the parameters become properties of the type:</span></span>

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      record_parameters? record_class_base? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      record_parameters? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

record_class_base
    : class_type record_base_arguments?
    | interface_type_list
    | class_type record_base_arguments? ',' interface_type_list
    ;

record_base_arguments
    : '(' argument_list? ')'
    ;

record_parameters
    : '(' record_parameter_list? ')'
    ;

record_parameter_list
    : record_parameter
    | record_parameter ',' record_parameter_list
    ;

record_parameter
    : attributes? type identifier record_property_name? default_argument?
    ;

record_property_name
    : ':' identifier
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

> <span data-ttu-id="2ddd1-158">**设计说明**：由于在不需要在类体中显式声明任何成员的情况下，记录类型通常很有用，因此我们修改声明的语法，以允许主体只是一个分号。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-158">**Design Notes**: Because record types are often useful without the need for any members explicitly declared in a class-body, we modify the syntax of the declaration to allow a body to be simply a semicolon.</span></span>

<span data-ttu-id="2ddd1-159">使用*记录参数*声明的类（结构）称为*记录类*（*record struct*），其中的任何一个是*记录类型*。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-159">A class (struct) declared with the *record-parameters* is called a *record class* (*record struct*), either of which is a *record type*.</span></span>

- <span data-ttu-id="2ddd1-160">[]**打开问题**：我们需要将*primary_constructor_body*包含在语法中，以便它可以出现在记录类型声明中。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-160">[ ] **Open issue**: We need to include *primary_constructor_body* in the grammar so that it can appear inside a record type declaration.</span></span>
- <span data-ttu-id="2ddd1-161">[]**打开问题**：参数名称的名称冲突规则是什么？</span><span class="sxs-lookup"><span data-stu-id="2ddd1-161">[ ] **Open issue**: What are the name conflict rules for the parameter names?</span></span> <span data-ttu-id="2ddd1-162">可能不允许一个类型参数或另一个*记录参数*冲突。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-162">Presumably one is not allowed to conflict with a type parameter or another *record-parameter*.</span></span>
- <span data-ttu-id="2ddd1-163">[]**打开问题**：需要指定记录参数的作用域。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-163">[ ] **Open issue**: We need to specify the scope of the record-parameters.</span></span> <span data-ttu-id="2ddd1-164">在哪里可以使用它们？</span><span class="sxs-lookup"><span data-stu-id="2ddd1-164">Where can they be used?</span></span> <span data-ttu-id="2ddd1-165">可能在实例字段初始值设定项和*primary_constructor_body*至少。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-165">Presumably within instance field initializers and *primary_constructor_body* at least.</span></span>
- <span data-ttu-id="2ddd1-166">[]**打开问题**：记录类型声明是否是分部的？</span><span class="sxs-lookup"><span data-stu-id="2ddd1-166">[ ] **Open issue**: Can a record type declaration be partial?</span></span> <span data-ttu-id="2ddd1-167">如果是这样，每个部分都必须重复参数吗？</span><span class="sxs-lookup"><span data-stu-id="2ddd1-167">If so, must the parameters be repeated on each part?</span></span>

#### <a name="members-of-a-record-type"></a><span data-ttu-id="2ddd1-168">记录类型的成员</span><span class="sxs-lookup"><span data-stu-id="2ddd1-168">Members of a record type</span></span>

<span data-ttu-id="2ddd1-169">除了在*类体*中声明的成员以外，记录类型还具有下列附加成员：</span><span class="sxs-lookup"><span data-stu-id="2ddd1-169">In addition to the members declared in the *class-body*, a record type has the following additional members:</span></span>

##### <a name="primary-constructor"></a><span data-ttu-id="2ddd1-170">主构造函数</span><span class="sxs-lookup"><span data-stu-id="2ddd1-170">Primary Constructor</span></span>

<span data-ttu-id="2ddd1-171">记录类型具有一个 `public` 构造函数，该构造函数的签名对应于类型声明的值参数。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-171">A record type has a `public` constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="2ddd1-172">这称为类型的*主构造函数*，并导致禁止显示隐式声明的*默认构造函数*。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-172">This is called the *primary constructor* for the type, and causes the implicitly declared *default constructor* to be suppressed.</span></span>

<span data-ttu-id="2ddd1-173">在运行时，主构造函数</span><span class="sxs-lookup"><span data-stu-id="2ddd1-173">At runtime the primary constructor</span></span>

* <span data-ttu-id="2ddd1-174">为对应于值参数的属性初始化编译器生成的支持字段（如果这些属性是编译器提供的）;[请参阅 1.1.2](#1.1.2)）;接着</span><span class="sxs-lookup"><span data-stu-id="2ddd1-174">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; [see 1.1.2](#1.1.2)); then</span></span>
* <span data-ttu-id="2ddd1-175">执行出现在*类体*中的实例字段初始值设定项;然后</span><span class="sxs-lookup"><span data-stu-id="2ddd1-175">executes the instance field initializers appearing in the *class-body*; and then</span></span>
* <span data-ttu-id="2ddd1-176">调用基类构造函数：</span><span class="sxs-lookup"><span data-stu-id="2ddd1-176">invokes a base class constructor:</span></span>
    * <span data-ttu-id="2ddd1-177">如果*record_base_arguments*中有参数，则调用由重载解析选择的具有这些参数的基构造函数;</span><span class="sxs-lookup"><span data-stu-id="2ddd1-177">If there are arguments in the *record_base_arguments*, a base constructor selected by overload resolution with these arguments is invoked;</span></span>
    * <span data-ttu-id="2ddd1-178">否则，将调用不带参数的基构造函数。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-178">Otherwise a base constructor is invoked with no arguments.</span></span>
* <span data-ttu-id="2ddd1-179">按源顺序执行每个*primary_constructor_body*（如果有）的正文。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-179">executes the body of each *primary_constructor_body*, if any, in source order.</span></span>

- <span data-ttu-id="2ddd1-180">[]**打开问题**：需要指定顺序，尤其是在分区的编译单元中。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-180">[ ] **Open issue**: We need to specify that order, particularly across compilation units for partials.</span></span>
- <span data-ttu-id="2ddd1-181">[]**打开问题**：需要指定每个显式声明的构造函数必须链接到主构造函数。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-181">[ ] **Open Issue**: We need to specify that every explicitly declared constructor must chain to the primary constructor.</span></span>
- <span data-ttu-id="2ddd1-182">[]**打开问题**：是否允许更改主构造函数的访问修饰符？</span><span class="sxs-lookup"><span data-stu-id="2ddd1-182">[ ] **Open issue**: Should it be allowed to change the access modifier on the primary constructor?</span></span>
- <span data-ttu-id="2ddd1-183">[]**打开问题**：在记录结构中，不存在任何记录参数，这是一个错误？</span><span class="sxs-lookup"><span data-stu-id="2ddd1-183">[ ] **Open issue**: In a record struct, it is an error for there to be no record parameters?</span></span>

##### <a name="primary-constructor-body"></a><span data-ttu-id="2ddd1-184">主构造函数正文</span><span class="sxs-lookup"><span data-stu-id="2ddd1-184">Primary constructor body</span></span>

```antlr
primary_constructor_body
    : attributes? constructor_modifiers? identifier block
    ;
```

<span data-ttu-id="2ddd1-185">*Primary_constructor_body*只能在记录类型声明内使用。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-185">A *primary_constructor_body* may only be used within a record type declaration.</span></span> <span data-ttu-id="2ddd1-186">*Primary_constructor_body*的*标识符*应命名声明它的记录类型。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-186">The *identifier* of a *primary_constructor_body* shall name the record type in which it is declared.</span></span>

<span data-ttu-id="2ddd1-187">*Primary_constructor_body*不会自行声明成员，但它是一种为程序员提供属性的方法，并指定了记录类型的主构造函数的访问权限。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-187">The *primary_constructor_body* does not declare a member on its own, but is a way for the programmer to provide attributes for, and specify the access of, a record type's primary constructor.</span></span> <span data-ttu-id="2ddd1-188">它还使程序员能够提供在构造记录类型的实例时要执行的附加代码。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-188">It also enables the programmer to provide additional code that will be executed when an instance of the record type is constructed.</span></span>

- <span data-ttu-id="2ddd1-189">[]**打开问题**：应注意，结构默认构造函数会绕过这种情况。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-189">[ ] **Open issue**: We should note that a struct default constructor bypasses this.</span></span>
- <span data-ttu-id="2ddd1-190">[]**打开问题**：应指定初始化的执行顺序。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-190">[ ] **Open issue**: We should specify the execution order of initialization.</span></span>
- <span data-ttu-id="2ddd1-191">[]**打开问题**：我们是否应在非记录类型声明中允许诸如*primary_constructor_body* （可能没有属性和修饰符）之类的内容，并将其视为实例字段初始值设定项的代码？</span><span class="sxs-lookup"><span data-stu-id="2ddd1-191">[ ] **Open issue**: Should we allow something like a *primary_constructor_body* (presumably without attributes and modifiers) in a non-record type declaration, and treat it like we would the code of an instance field initializer?</span></span>

##### <a name="properties"></a><span data-ttu-id="2ddd1-192">属性</span><span class="sxs-lookup"><span data-stu-id="2ddd1-192">Properties</span></span>

<span data-ttu-id="2ddd1-193">对于记录类型声明的每个记录参数，都有一个对应的 `public` 属性成员，其名称和类型取自值参数声明。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-193">For each record parameter of a record type declaration there is a corresponding `public` property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="2ddd1-194">其名称是*record_property_name*的*标识符*（如果存在）或*record_parameter*的*标识符*，否则为。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-194">Its name is the *identifier* of the *record_property_name*, if present, or the *identifier* of the *record_parameter* otherwise.</span></span> <span data-ttu-id="2ddd1-195">如果没有 `get` 显式声明或继承具有访问器且具有此名称和类型的具体（即非抽象）公共属性，则编译器将生成，如下所示：</span><span class="sxs-lookup"><span data-stu-id="2ddd1-195">If no concrete (i.e. non-abstract) public property with a `get` accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

* <span data-ttu-id="2ddd1-196">对于*记录结构*或 `sealed` *记录类*：</span><span class="sxs-lookup"><span data-stu-id="2ddd1-196">For a *record struct* or a `sealed` *record class*:</span></span>
 * <span data-ttu-id="2ddd1-197">`private` `readonly` 字段作为属性的支持字段生成 `readonly` 。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-197">A `private` `readonly` field is produced as a backing field for a `readonly` property.</span></span> <span data-ttu-id="2ddd1-198">在构造过程中，它的值将与相应的主构造函数参数的值一起初始化。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-198">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span>
 * <span data-ttu-id="2ddd1-199">实现该属性的 `get` 访问器以返回支持字段的值。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-199">The property's `get` accessor is implemented to return the value of the backing field.</span></span>
 * <span data-ttu-id="2ddd1-200">将重写每个 "匹配" 继承的虚拟属性的 `get` 访问器。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-200">Each "matching" inherited virtual property's `get` accessor is overridden.</span></span>

> <span data-ttu-id="2ddd1-201">**设计说明**：换言之，如果扩展基类或实现一个接口，该接口声明了具有与 record 参数相同的名称和类型的公共抽象属性，则会重写或实现该属性。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-201">**Design notes**: In other words, if you extend a base class or implement an interface that declares a public abstract property with the same name and type as a record parameter, that property is overridden or implemented.</span></span>

- <span data-ttu-id="2ddd1-202">[]**打开问题**：当显式声明某个属性时，是否可以更改该属性的访问修饰符？</span><span class="sxs-lookup"><span data-stu-id="2ddd1-202">[ ] **Open issue**: Should it be possible to change the access modifier on a property when it is explicitly declared?</span></span>
- <span data-ttu-id="2ddd1-203">[]**打开问题**：是否可以用属性替换某个字段？</span><span class="sxs-lookup"><span data-stu-id="2ddd1-203">[ ] **Open issue**: Should it be possible to substitute a field for a property?</span></span>

##### <a name="object-methods"></a><span data-ttu-id="2ddd1-204">对象方法</span><span class="sxs-lookup"><span data-stu-id="2ddd1-204">Object Methods</span></span>

<span data-ttu-id="2ddd1-205">对于*记录结构*或 `sealed` *记录类*，方法的实现 `object.GetHashCode()` 和 `object.Equals(object)` 由编译器生成，除非用户提供。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-205">For a *record struct* or a `sealed` *record class*, implementations of the methods `object.GetHashCode()` and `object.Equals(object)` are produced by the compiler unless provided by the user.</span></span>

- <span data-ttu-id="2ddd1-206">[]**打开问题**：我们应该精确指定其实现。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-206">[ ] **Open issue**: We should precisely specify their implementation.</span></span>
- <span data-ttu-id="2ddd1-207">[]**打开问题**：我们还应 `IEquatable<T>` 为记录类型添加接口，并指定提供实现。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-207">[ ] **Open issue**: We should also add the interface `IEquatable<T>` for the record type and specify that implementations are provided.</span></span>
- <span data-ttu-id="2ddd1-208">[]**打开问题**：我们还应指定实现每个 `IEquatable<T>.Equals` 。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-208">[ ] **Open issue**: We should also specify that we implement every `IEquatable<T>.Equals`.</span></span>
- <span data-ttu-id="2ddd1-209">[]**打开问题**：我们应确切地指定我们在记录继承面对的问题的解决方法：具体而言，我们如何生成相等方法，以使它们具有对称、可传递、反身等。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-209">[ ] **Open issue**: We should specify precisely how we solve the problem of Equals in the face of record inheritance: specifically how we generate equality methods such that they are symmetric, transitive, reflexive, etc.</span></span>
- <span data-ttu-id="2ddd1-210">[]**打开问题**：已建议实施 `operator ==` 和 `operator !=` 记录类型。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-210">[ ] **Open issue**: It has been proposed that we implement `operator ==` and `operator !=` for record types.</span></span>
- <span data-ttu-id="2ddd1-211">[]**打开问题**：是否应自动生成的实现 `object.ToString` ？</span><span class="sxs-lookup"><span data-stu-id="2ddd1-211">[ ] **Open issue**: Should we auto-generate an implementation of `object.ToString`?</span></span>

##### `Deconstruct`

<span data-ttu-id="2ddd1-212">记录类型具有编译器生成的方法， `public` `void Deconstruct` 除非用户提供一个具有任何签名的方法。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-212">A record type has a compiler-generated `public` method `void Deconstruct` unless one with any signature is provided by the user.</span></span> <span data-ttu-id="2ddd1-213">每个参数都是与该 `out` 记录类型的相应参数具有相同名称和类型的参数。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-213">Each parameter is an `out` parameter of the same name and type as the corresponding parameter of the record type.</span></span> <span data-ttu-id="2ddd1-214">此方法的编译器提供的实现应为每个 `out` 参数分配相应属性的值。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-214">The compiler-provided implementation of this method shall assign each `out` parameter with the value of the corresponding property.</span></span>

<span data-ttu-id="2ddd1-215">请参阅的语义的[模式匹配规范](csharp-8.0/patterns.md#positional-pattern) `Deconstruct` 。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-215">See [the pattern-matching specification](csharp-8.0/patterns.md#positional-pattern) for the semantics of `Deconstruct`.</span></span>

##### <a name="with-method"></a><span data-ttu-id="2ddd1-216">`With` 方法</span><span class="sxs-lookup"><span data-stu-id="2ddd1-216">`With` method</span></span>

<span data-ttu-id="2ddd1-217">除非存在名为 "已声明" 的用户声明的成员 `With` ，否则记录类型具有一个名为的编译器提供的方法 `With` ，该方法的返回类型为记录类型本身，并包含与每个*记录参数*相对应的值参数，这些参数在记录类型声明中出现的顺序相同。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-217">Unless there is a user-declared member named `With` declared, a record type has a compiler-provided method named `With` whose return type is the record type itself, and containing one value parameter corresponding to each *record-parameter* in the same order that these parameters appear in the record type declaration.</span></span> <span data-ttu-id="2ddd1-218">每个参数都应具有相应属性的*调用方-接收方默认参数*。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-218">Each parameter shall have a *caller-receiver default-argument* of the corresponding property.</span></span>

<span data-ttu-id="2ddd1-219">在 `abstract` record 类中，编译器提供的 `With` 方法是抽象的。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-219">In an `abstract` record class, the compiler-provided `With` method is abstract.</span></span> <span data-ttu-id="2ddd1-220">在记录结构或密封记录类中，编译器提供的 `With` 方法是 `sealed` 。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-220">In a record struct, or a sealed record class, the compiler-provided `With` method is `sealed`.</span></span> <span data-ttu-id="2ddd1-221">否则，编译器提供的 `With` 方法是 "虚拟的，它的实现应返回一个新实例，该实例通过将参数作为参数来调用主构造函数以从参数创建新实例，并返回该新实例。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-221">Otherwise the compiler-provided `With` method is \`virtual and its implementation shall return a new instance produced by invoking the primary constructor with the parameters as arguments to create a new instance from the parameters, and return that new instance.</span></span>

- <span data-ttu-id="2ddd1-222">[]**打开问题**：我们还应指定在什么条件下重写或实现 `With` `With` 从实现的接口继承的虚方法或方法。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-222">[ ] **Open issue**: We should also specify under what conditions we override or implement inherited virtual `With` methods or `With` methods from implemented interfaces.</span></span>
- <span data-ttu-id="2ddd1-223">[]**打开问题**：我们应指出当继承非虚拟方法时会发生什么情况 `With` 。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-223">[ ] **Open issue**: We should say what happens when we inherit a non-virtual `With` method.</span></span>

> <span data-ttu-id="2ddd1-224">**设计说明**：由于记录类型在默认情况下是不可变的，因此该 `With` 方法提供了一种方法来创建新的实例，该实例与现有实例相同，但所选属性为新值。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-224">**Design notes**: Because record types are by default immutable, the `With` method provides a way of creating a new instance that is the same as an existing instance but with selected properties given new values.</span></span> <span data-ttu-id="2ddd1-225">例如，给定</span><span class="sxs-lookup"><span data-stu-id="2ddd1-225">For example, given</span></span>
> ```cs
> public class Point(int X, int Y);
> ```
> <span data-ttu-id="2ddd1-226">存在编译器提供的成员</span><span class="sxs-lookup"><span data-stu-id="2ddd1-226">there is a compiler-provided member</span></span>
> ```cs
>     public virtual Point With(int X = this.X, int Y = this.Y) => new Point(X, Y);
> ```
> <span data-ttu-id="2ddd1-227">这将启用记录类型的变量</span><span class="sxs-lookup"><span data-stu-id="2ddd1-227">Which enables an variable of the record type</span></span>
> ```cs
> var p = new Point(3, 4);
> ```
> <span data-ttu-id="2ddd1-228">替换为具有一个或多个属性的实例</span><span class="sxs-lookup"><span data-stu-id="2ddd1-228">to be replaced with an instance that has one or more properties different</span></span>
> ```cs
>     p = p.With(X: 5);
> ```
> <span data-ttu-id="2ddd1-229">这还可以使用*with_expression*来表示：</span><span class="sxs-lookup"><span data-stu-id="2ddd1-229">This can also be expressed using the *with_expression*:</span></span>
> ```cs
>     p = p with { X = 5 };
> ```

### <a name="examples"></a><span data-ttu-id="2ddd1-230">示例</span><span class="sxs-lookup"><span data-stu-id="2ddd1-230">Examples</span></span>

#### <a name="compatibility-of-record-types"></a><span data-ttu-id="2ddd1-231">记录类型的兼容性</span><span class="sxs-lookup"><span data-stu-id="2ddd1-231">Compatibility of record types</span></span>

<span data-ttu-id="2ddd1-232">由于程序员可以将成员添加到记录类型声明中，因此通常可以更改记录元素的集合，而不会影响现有的客户端。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-232">Because the programmer can add members to a record type declaration, it is often possible to change the set of record elements without affecting existing clients.</span></span> <span data-ttu-id="2ddd1-233">例如，给定记录类型的初始版本</span><span class="sxs-lookup"><span data-stu-id="2ddd1-233">For example, given an initial version of a record type</span></span>

```cs
// v1
public class Person(string Name, DateTime DateOfBirth);
```

<span data-ttu-id="2ddd1-234">记录类型的新元素可以兼容性添加到该类型的下一版本中，而不会影响二进制或源兼容性：</span><span class="sxs-lookup"><span data-stu-id="2ddd1-234">A new element of the record type can be compatibly added in the next revision of the type without affecting binary or source compatibility:</span></span>

```cs
// v2
public class Person(string Name, DateTime DateOfBirth, string HomeTown)
{
    // Note: below operations added to retain binary compatibility with v1
    public Person(string Name, DateTime DateOfBirth) : this(Name, DateOfBirth, string.Empty) {}
    public static void operator is(Person self, out string Name, out DateTime DateOfBirth)
        { Name = self.Name; DateOfBirth = self.DateOfBirth; }
    public Person With(string Name, DateTime DateOfBirth) => new Person(Name, DateOfBirth);
}
```

#### <a name="record-struct-example"></a><span data-ttu-id="2ddd1-235">record 结构示例</span><span class="sxs-lookup"><span data-stu-id="2ddd1-235">record struct example</span></span>

<span data-ttu-id="2ddd1-236">此记录结构</span><span class="sxs-lookup"><span data-stu-id="2ddd1-236">This record struct</span></span>

```cs
public struct Pair(object First, object Second);
```

<span data-ttu-id="2ddd1-237">转换为此代码</span><span class="sxs-lookup"><span data-stu-id="2ddd1-237">is translated to this code</span></span>

```cs
public struct Pair : IEquatable<Pair>
{
    public object First { get; }
    public object Second { get; }
    public Pair(object First, object Second)
    {
        this.First = First;
        this.Second = Second;
    }
    public bool Equals(Pair other) // for IEquatable<Pair>
    {
        return Equals(First, other.First) && Equals(Second, other.Second);
    }
    public override bool Equals(object other)
    {
        return (other as Pair)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (First?.GetHashCode()*17 + Second?.GetHashCode()).GetValueOrDefault();
    }
    public Pair With(object First = this.First, object Second = this.Second) => new Pair(First, Second);
    public void Deconstruct(out object First, out object Second)
    {
        First = self.First;
        Second = self.Second;
    }
}
```

- <span data-ttu-id="2ddd1-238">[]**打开问题**： Equals 的实现（对等）是否是对的公共成员？</span><span class="sxs-lookup"><span data-stu-id="2ddd1-238">[ ] **Open issue**: should the implementation of Equals(Pair other) be a public member of Pair?</span></span>
- <span data-ttu-id="2ddd1-239">[]**打开问题**：此实现 `Equals` 在面对继承时不对称。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-239">[ ] **Open issue**: This implementation of `Equals` is not symmetric in the face of inheritance.</span></span>
- <span data-ttu-id="2ddd1-240">[]**打开问题**：记录是否应声明 `operator ==` 和 `operator !=` ？</span><span class="sxs-lookup"><span data-stu-id="2ddd1-240">[ ] **Open issue**: Should a record declare `operator ==` and `operator !=`?</span></span>

> <span data-ttu-id="2ddd1-241">**设计说明**：由于一种记录类型可以从另一种记录类型继承，因此 `Equals` 在这种情况下，此实现不是对称的，这是不正确的。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-241">**Design notes**: Because one record type can inherit from another, and this implementation of `Equals` would not be symmetric in that case, it is not correct.</span></span> <span data-ttu-id="2ddd1-242">建议以这种方式实现相等性：</span><span class="sxs-lookup"><span data-stu-id="2ddd1-242">We propose to implement equality this way:</span></span>
> ```cs
>     public bool Equals(Pair other) // for IEquatable<Pair>
>     {
>         return other != null && EqualityContract == other.EqualityContract &&
>             Equals(First, other.First) && Equals(Second, other.Second);
>     }
>     protected virtual Type EqualityContract => typeof(Pair);
> ```
> <span data-ttu-id="2ddd1-243">派生记录是 `override EqualityContract` 。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-243">Derived records would `override EqualityContract`.</span></span> <span data-ttu-id="2ddd1-244">更具吸引力的替代方法是限制继承。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-244">The less attractive alternative is to restrict inheritance.</span></span>

#### <a name="sealed-record-example"></a><span data-ttu-id="2ddd1-245">密封记录示例</span><span class="sxs-lookup"><span data-stu-id="2ddd1-245">sealed record example</span></span>

<span data-ttu-id="2ddd1-246">此密封记录类</span><span class="sxs-lookup"><span data-stu-id="2ddd1-246">This sealed record class</span></span>

```cs
public sealed class Student(string Name, decimal Gpa);
```

<span data-ttu-id="2ddd1-247">转换为此代码</span><span class="sxs-lookup"><span data-stu-id="2ddd1-247">is translated into this code</span></span>

```cs
public sealed class Student : IEquatable<Student>
{
    public string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public bool Equals(Student other) // for IEquatable<Student>
    {
        return other != null && Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public override bool Equals(object other)
    {
        return this.Equals(other as Student);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa?.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public void Deconstruct(out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

#### <a name="abstract-record-class-example"></a><span data-ttu-id="2ddd1-248">abstract 记录类示例</span><span class="sxs-lookup"><span data-stu-id="2ddd1-248">abstract record class example</span></span>

<span data-ttu-id="2ddd1-249">此抽象记录类</span><span class="sxs-lookup"><span data-stu-id="2ddd1-249">This abstract record class</span></span>

```cs
public abstract class Person(string Name);
```

<span data-ttu-id="2ddd1-250">转换为此代码</span><span class="sxs-lookup"><span data-stu-id="2ddd1-250">is translated into this code</span></span>

```cs
public abstract class Person : IEquatable<Person>
{
    public string Name { get; }
    public Person(string Name)
    {
        this.Name = Name;
    }
    public bool Equals(Person other)
    {
        return other != null && Equals(Name, other.Name);
    }
    public override bool Equals(object other)
    {
        return Equals(other as Person);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()).GetValueOrDefault();
    }
    public abstract Person With(string Name = this.Name);
    public void Deconstruct(Person self, out string Name)
    {
        Name = self.Name;
    }
}
```

#### <a name="combining-abstract-and-sealed-records"></a><span data-ttu-id="2ddd1-251">合并抽象记录和密封记录</span><span class="sxs-lookup"><span data-stu-id="2ddd1-251">combining abstract and sealed records</span></span>

<span data-ttu-id="2ddd1-252">给定上述抽象记录类 `Person` ，此密封记录类</span><span class="sxs-lookup"><span data-stu-id="2ddd1-252">Given the abstract record class `Person` above, this sealed record class</span></span>

```cs
public sealed class Student(string Name, decimal Gpa) : Person(Name);
```

<span data-ttu-id="2ddd1-253">转换为此代码</span><span class="sxs-lookup"><span data-stu-id="2ddd1-253">is translated into this code</span></span>

```cs
public sealed class Student : Person, IEquatable<Student>
{
    public override string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa) : base(Name)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public override bool Equals(Student other) // for IEquatable<Student>
    {
        return Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public bool Equals(Person other) // for IEquatable<Person>
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override bool Equals(object other)
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public override Person With(string Name = this.Name) => new Student(Name, Gpa);
    public void Deconstruct(Student self, out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

## <a name="drawbacks"></a><span data-ttu-id="2ddd1-254">缺点</span><span class="sxs-lookup"><span data-stu-id="2ddd1-254">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="2ddd1-255">与任何语言功能一样，我们必须回答对 c # 程序正文提供的更清晰的偿还，以使其受益于该功能。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-255">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="2ddd1-256">备选项</span><span class="sxs-lookup"><span data-stu-id="2ddd1-256">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="2ddd1-257">在 c # 6 中，我们考虑添加*主构造函数*。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-257">We considered adding *primary constructors* in C# 6.</span></span> <span data-ttu-id="2ddd1-258">尽管它们与此建议具有相同的语法图面，但我们发现它们的优点低于记录所提供的优势。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-258">Although they occupy the same syntactic surface as this proposal, we found that they fell short of the advantages offered by records.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="2ddd1-259">未解决的问题</span><span class="sxs-lookup"><span data-stu-id="2ddd1-259">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="2ddd1-260">建议的正文中出现打开的问题。</span><span class="sxs-lookup"><span data-stu-id="2ddd1-260">Open questions appear in the body of the proposal.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="2ddd1-261">设计会议</span><span class="sxs-lookup"><span data-stu-id="2ddd1-261">Design meetings</span></span>

<span data-ttu-id="2ddd1-262">TBD</span><span class="sxs-lookup"><span data-stu-id="2ddd1-262">TBD</span></span>
