---
ms.openlocfilehash: a01cf9387b8dc47de036bf0bd1496c19a441d81c
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876793"
---
# <a name="variables"></a><span data-ttu-id="4d3a5-101">变量</span><span class="sxs-lookup"><span data-stu-id="4d3a5-101">Variables</span></span>

<span data-ttu-id="4d3a5-102">变量表示存储位置。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-102">Variables represent storage locations.</span></span> <span data-ttu-id="4d3a5-103">每个变量都有一个类型，用于确定可以在变量中存储的值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-103">Every variable has a type that determines what values can be stored in the variable.</span></span> <span data-ttu-id="4d3a5-104">C#是一种类型安全的语言， C#编译器保证变量中存储的值始终为适当的类型。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-104">C# is a type-safe language, and the C# compiler guarantees that values stored in variables are always of the appropriate type.</span></span> <span data-ttu-id="4d3a5-105">可以通过赋值或使用`++`和`--`运算符来更改变量的值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-105">The value of a variable can be changed through assignment or through use of the `++` and `--` operators.</span></span>

<span data-ttu-id="4d3a5-106">在可以获取变量的值之前，必须对其进行***明确***赋值（[明确赋值](variables.md#definite-assignment)）。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-106">A variable must be ***definitely assigned*** ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained.</span></span>

<span data-ttu-id="4d3a5-107">如以下各节所述，变量***最初已赋值***或***最初未分配***。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-107">As described in the following sections, variables are either ***initially assigned*** or ***initially unassigned***.</span></span> <span data-ttu-id="4d3a5-108">最初分配的变量具有定义完善的初始值，并始终被视为明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-108">An initially assigned variable has a well-defined initial value and is always considered definitely assigned.</span></span> <span data-ttu-id="4d3a5-109">初始未赋值的变量没有初始值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-109">An initially unassigned variable has no initial value.</span></span> <span data-ttu-id="4d3a5-110">对于在某个特定位置被视为明确赋值的初始未赋值的变量，对该变量的赋值必须出现在通向该位置的每个可能的执行路径中。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-110">For an initially unassigned variable to be considered definitely assigned at a certain location, an assignment to the variable must occur in every possible execution path leading to that location.</span></span>

## <a name="variable-categories"></a><span data-ttu-id="4d3a5-111">变量类别</span><span class="sxs-lookup"><span data-stu-id="4d3a5-111">Variable categories</span></span>

<span data-ttu-id="4d3a5-112">C#定义七类变量：静态变量、实例变量、数组元素、值参数、引用参数、输出参数和局部变量。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-112">C# defines seven categories of variables: static variables, instance variables, array elements, value parameters, reference parameters, output parameters, and local variables.</span></span> <span data-ttu-id="4d3a5-113">以下各节介绍了其中的每个类别。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-113">The sections that follow describe each of these categories.</span></span>

<span data-ttu-id="4d3a5-114">示例中</span><span class="sxs-lookup"><span data-stu-id="4d3a5-114">In the example</span></span>
```csharp
class A
{
    public static int x;
    int y;

    void F(int[] v, int a, ref int b, out int c) {
        int i = 1;
        c = a + b++;
    }
}
```
<span data-ttu-id="4d3a5-115">`x`是一个静态变量， `y`它是一个实例变量`v[0]` ，是一个数组元素`a` ，是一个`c`值参数`b` ，是一个引用参数，是一个输出参数`i` ，是一个本地变量.</span><span class="sxs-lookup"><span data-stu-id="4d3a5-115">`x` is a static variable, `y` is an instance variable, `v[0]` is an array element, `a` is a value parameter, `b` is a reference parameter, `c` is an output parameter, and `i` is a local variable.</span></span>

### <a name="static-variables"></a><span data-ttu-id="4d3a5-116">静态变量</span><span class="sxs-lookup"><span data-stu-id="4d3a5-116">Static variables</span></span>

<span data-ttu-id="4d3a5-117">使用`static`修饰符声明的字段称为***静态变量***。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-117">A field declared with the `static` modifier is called a ***static variable***.</span></span> <span data-ttu-id="4d3a5-118">静态变量在为其包含类型的静态构造函数（[静态构造函数](classes.md#static-constructors)）的执行之前就已存在，如果关联的应用程序域停止存在，则不再存在。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-118">A static variable comes into existence before execution of the static constructor ([Static constructors](classes.md#static-constructors)) for its containing type, and ceases to exist when the associated application domain ceases to exist.</span></span>

<span data-ttu-id="4d3a5-119">静态变量的初始值是变量类型的默认值（[默认](variables.md#default-values)值）。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-119">The initial value of a static variable is the default value ([Default values](variables.md#default-values)) of the variable's type.</span></span>

<span data-ttu-id="4d3a5-120">出于明确赋值检查的目的，静态变量被视为初始赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-120">For purposes of definite assignment checking, a static variable is considered initially assigned.</span></span>

### <a name="instance-variables"></a><span data-ttu-id="4d3a5-121">实例变量</span><span class="sxs-lookup"><span data-stu-id="4d3a5-121">Instance variables</span></span>

<span data-ttu-id="4d3a5-122">不使用`static`修饰符声明的字段称为***实例变量***。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-122">A field declared without the `static` modifier is called an ***instance variable***.</span></span>

#### <a name="instance-variables-in-classes"></a><span data-ttu-id="4d3a5-123">类中的实例变量</span><span class="sxs-lookup"><span data-stu-id="4d3a5-123">Instance variables in classes</span></span>

<span data-ttu-id="4d3a5-124">当创建该类的新实例时，该类的实例变量就会成为存在，如果没有对该实例的引用，并且已执行了该实例的析构函数（如果有），就不再存在。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-124">An instance variable of a class comes into existence when a new instance of that class is created, and ceases to exist when there are no references to that instance and the instance's destructor (if any) has executed.</span></span>

<span data-ttu-id="4d3a5-125">类的实例变量的初始值是变量类型的默认值（[默认](variables.md#default-values)值）。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-125">The initial value of an instance variable of a class is the default value ([Default values](variables.md#default-values)) of the variable's type.</span></span>

<span data-ttu-id="4d3a5-126">出于明确赋值检查的目的，类的实例变量被视为初始赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-126">For the purpose of definite assignment checking, an instance variable of a class is considered initially assigned.</span></span>

#### <a name="instance-variables-in-structs"></a><span data-ttu-id="4d3a5-127">结构中的实例变量</span><span class="sxs-lookup"><span data-stu-id="4d3a5-127">Instance variables in structs</span></span>

<span data-ttu-id="4d3a5-128">结构的实例变量与它所属的结构变量的生存期完全相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-128">An instance variable of a struct has exactly the same lifetime as the struct variable to which it belongs.</span></span> <span data-ttu-id="4d3a5-129">换而言之，当结构类型的变量变为存在或不再存在时，该结构的实例变量也是如此。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-129">In other words, when a variable of a struct type comes into existence or ceases to exist, so too do the instance variables of the struct.</span></span>

<span data-ttu-id="4d3a5-130">结构的实例变量的初始赋值状态与包含结构变量的初始赋值状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-130">The initial assignment state of an instance variable of a struct is the same as that of the containing struct variable.</span></span> <span data-ttu-id="4d3a5-131">换言之，当结构变量被视为初始已赋值，因此它的实例变量也被视为未赋值时，它的实例变量会同样取消分配。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-131">In other words, when a struct variable is considered initially assigned, so too are its instance variables, and when a struct variable is considered initially unassigned, its instance variables are likewise unassigned.</span></span>

### <a name="array-elements"></a><span data-ttu-id="4d3a5-132">数组元素</span><span class="sxs-lookup"><span data-stu-id="4d3a5-132">Array elements</span></span>

<span data-ttu-id="4d3a5-133">当创建数组实例时，数组的元素便会存在，并且在没有对该数组实例的引用时停止存在。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-133">The elements of an array come into existence when an array instance is created, and cease to exist when there are no references to that array instance.</span></span>

<span data-ttu-id="4d3a5-134">数组中每个元素的初始值为数组元素类型的默认值（[默认](variables.md#default-values)值）。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-134">The initial value of each of the elements of an array is the default value ([Default values](variables.md#default-values)) of the type of the array elements.</span></span>

<span data-ttu-id="4d3a5-135">出于明确赋值检查的目的，数组元素被视为初始已赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-135">For the purpose of definite assignment checking, an array element is considered initially assigned.</span></span>

### <a name="value-parameters"></a><span data-ttu-id="4d3a5-136">值参数</span><span class="sxs-lookup"><span data-stu-id="4d3a5-136">Value parameters</span></span>

<span data-ttu-id="4d3a5-137">不带`ref`或`out`修饰符声明的参数是一个***值参数***。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-137">A parameter declared without a `ref` or `out` modifier is a ***value parameter***.</span></span>

<span data-ttu-id="4d3a5-138">值参数会在调用该参数所属的函数成员（方法、实例构造函数、访问器或运算符）或匿名函数时成为存在，并使用调用中给定的自变量的值进行初始化。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-138">A value parameter comes into existence upon invocation of the function member (method, instance constructor, accessor, or operator) or anonymous function to which the parameter belongs, and is initialized with the value of the argument given in the invocation.</span></span> <span data-ttu-id="4d3a5-139">当函数成员或匿名函数返回时，值参数通常不存在。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-139">A value parameter normally ceases to exist upon return of the function member or anonymous function.</span></span> <span data-ttu-id="4d3a5-140">但是，如果值参数由匿名函数（[匿名函数表达式](expressions.md#anonymous-function-expressions)）捕获，则其生存期将至少扩展到从该匿名函数创建的委托或表达式树符合垃圾回收的条件。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-140">However, if the value parameter is captured by an anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), its life time extends at least until the delegate or expression tree created from that anonymous function is eligible for garbage collection.</span></span>

<span data-ttu-id="4d3a5-141">出于明确赋值检查的目的，值参数被视为初始已赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-141">For the purpose of definite assignment checking, a value parameter is considered initially assigned.</span></span>

### <a name="reference-parameters"></a><span data-ttu-id="4d3a5-142">引用参数</span><span class="sxs-lookup"><span data-stu-id="4d3a5-142">Reference parameters</span></span>

<span data-ttu-id="4d3a5-143">使用`ref`修饰符声明的参数是一个***引用参数***。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-143">A parameter declared with a `ref` modifier is a ***reference parameter***.</span></span>

<span data-ttu-id="4d3a5-144">引用参数不会创建新的存储位置。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-144">A reference parameter does not create a new storage location.</span></span> <span data-ttu-id="4d3a5-145">相反，引用参数与给定作为函数成员或匿名函数调用中的参数的变量表示相同的存储位置。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-145">Instead, a reference parameter represents the same storage location as the variable given as the argument in the function member or anonymous function invocation.</span></span> <span data-ttu-id="4d3a5-146">因此，引用参数的值始终与基础变量相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-146">Thus, the value of a reference parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="4d3a5-147">以下明确赋值规则适用于引用参数。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-147">The following definite assignment rules apply to reference parameters.</span></span> <span data-ttu-id="4d3a5-148">请注意[输出参数](variables.md#output-parameters)中所述的输出参数的不同规则。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-148">Note the different rules for output parameters described in [Output parameters](variables.md#output-parameters).</span></span>

*  <span data-ttu-id="4d3a5-149">在函数成员或委托调用中将变量作为引用参数传递之前，必须对其进行明确赋值（[明确赋值](variables.md#definite-assignment)）。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-149">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter in a function member or delegate invocation.</span></span>
*  <span data-ttu-id="4d3a5-150">在函数成员或匿名函数中，引用参数被视为初始赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-150">Within a function member or anonymous function, a reference parameter is considered initially assigned.</span></span>

<span data-ttu-id="4d3a5-151">在结构类型的实例方法或实例访问器中，关键字`this`的行为与结构类型（[此访问](expressions.md#this-access)）的引用参数完全相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-151">Within an instance method or instance accessor of a struct type, the `this` keyword behaves exactly as a reference parameter of the struct type ([This access](expressions.md#this-access)).</span></span>

### <a name="output-parameters"></a><span data-ttu-id="4d3a5-152">输出参数</span><span class="sxs-lookup"><span data-stu-id="4d3a5-152">Output parameters</span></span>

<span data-ttu-id="4d3a5-153">使用`out`修饰符声明的参数是一个***输出参数***。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-153">A parameter declared with an `out` modifier is an ***output parameter***.</span></span>

<span data-ttu-id="4d3a5-154">Output 参数不会创建新的存储位置。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-154">An output parameter does not create a new storage location.</span></span> <span data-ttu-id="4d3a5-155">相反，output 参数表示作为函数成员或委托调用中的参数提供的相同存储位置。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-155">Instead, an output parameter represents the same storage location as the variable given as the argument in the function member or delegate invocation.</span></span> <span data-ttu-id="4d3a5-156">因此，output 参数的值始终与基础变量相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-156">Thus, the value of an output parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="4d3a5-157">以下明确的赋值规则适用于 output 参数。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-157">The following definite assignment rules apply to output parameters.</span></span> <span data-ttu-id="4d3a5-158">请注意[引用参数](variables.md#reference-parameters)中所述的引用参数的不同规则。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-158">Note the different rules for reference parameters described in [Reference parameters](variables.md#reference-parameters).</span></span>

*  <span data-ttu-id="4d3a5-159">在函数成员或委托调用中将变量作为 output 参数传递之前，不需要明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-159">A variable need not be definitely assigned before it can be passed as an output parameter in a function member or delegate invocation.</span></span>
*  <span data-ttu-id="4d3a5-160">完成函数成员或委托调用的正常完成后，作为输出参数传递的每个变量都被视为在该执行路径中分配。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-160">Following the normal completion of a function member or delegate invocation, each variable that was passed as an output parameter is considered assigned in that execution path.</span></span>
*  <span data-ttu-id="4d3a5-161">在函数成员或匿名函数中，output 参数被视为初始未赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-161">Within a function member or anonymous function, an output parameter is considered initially unassigned.</span></span>
*  <span data-ttu-id="4d3a5-162">函数成员或匿名函数的每个输出参数都必须在函数成员或匿名函数正常返回之前明确赋值（[明确赋值](variables.md#definite-assignment)）。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-162">Every output parameter of a function member or anonymous function must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before the function member or anonymous function returns normally.</span></span>

<span data-ttu-id="4d3a5-163">在结构类型的实例构造函数中，关键字`this`的行为与结构类型的输出参数（[此访问](expressions.md#this-access)）完全相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-163">Within an instance constructor of a struct type, the `this` keyword behaves exactly as an output parameter of the struct type ([This access](expressions.md#this-access)).</span></span>

### <a name="local-variables"></a><span data-ttu-id="4d3a5-164">局部变量</span><span class="sxs-lookup"><span data-stu-id="4d3a5-164">Local variables</span></span>

<span data-ttu-id="4d3a5-165">***局部变量***由*local_variable_declaration*声明，该变量可能出现在*块*、 *for_statement*、 *switch_statement*或*using_statement*中。或*try_statement*的*foreach_statement*或*specific_catch_clause* 。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-165">A ***local variable*** is declared by a *local_variable_declaration*, which may occur in a *block*, a *for_statement*, a *switch_statement* or a *using_statement*; or by a *foreach_statement* or a *specific_catch_clause* for a *try_statement*.</span></span>

<span data-ttu-id="4d3a5-166">局部变量的生存期是程序执行的一部分，在该过程中，将保证存储的存储空间。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-166">The lifetime of a local variable is the portion of program execution during which storage is guaranteed to be reserved for it.</span></span> <span data-ttu-id="4d3a5-167">此生存期至少会将输入内容扩展到*块*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*或与其关联的*specific_catch_clause* ，直到*该块*的执行、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*或*specific_catch_clause*以任何方式结束。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-167">This lifetime extends at least from entry into the *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* with which it is associated, until execution of that *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* ends in any way.</span></span> <span data-ttu-id="4d3a5-168">（输入封闭的*块*或调用方法会挂起，但不会结束、执行当前*块*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*或*specific_catch_clause*。）如果通过匿名函数（[捕获的外部变量](expressions.md#captured-outer-variables)）捕获本地变量，则其生存期至少将一直扩展到从匿名函数创建的委托或表达式树以及引用捕获的变量适用于垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-168">(Entering an enclosed *block* or calling a method suspends, but does not end, execution of the current *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause*.) If the local variable is captured by an anonymous function ([Captured outer variables](expressions.md#captured-outer-variables)), its lifetime extends at least until the delegate or expression tree created from the anonymous function, along with any other objects that come to reference the captured variable, are eligible for garbage collection.</span></span>

<span data-ttu-id="4d3a5-169">如果以递归方式进入父*block*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*或*specific_catch_clause* ，则将创建一个新的本地变量实例，每次计算时间及其*local_variable_initializer*（如果有）。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-169">If the parent *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* is entered recursively, a new instance of the local variable is created each time, and its *local_variable_initializer*, if any, is evaluated each time.</span></span>

<span data-ttu-id="4d3a5-170">*Local_variable_declaration*引入的本地变量不会自动初始化，因此没有默认值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-170">A local variable introduced by a *local_variable_declaration* is not automatically initialized and thus has no default value.</span></span> <span data-ttu-id="4d3a5-171">出于明确赋值检查的目的， *local_variable_declaration*引入的局部变量被视为初始未分配。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-171">For the purpose of definite assignment checking, a local variable introduced by a *local_variable_declaration* is considered initially unassigned.</span></span> <span data-ttu-id="4d3a5-172">*Local_variable_declaration*可以包括*local_variable_initializer*，在这种情况下，仅在初始化表达式（[声明语句](variables.md#declaration-statements)）后才将变量视为明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-172">A *local_variable_declaration* may include a *local_variable_initializer*, in which case the variable is considered definitely assigned only after the initializing expression ([Declaration statements](variables.md#declaration-statements)).</span></span>

<span data-ttu-id="4d3a5-173">在*local_variable_declaration*引入的局部变量范围内，在其*local_variable_declarator*之前的文本位置引用该局部变量是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-173">Within the scope of a local variable introduced by a *local_variable_declaration*, it is a compile-time error to refer to that local variable in a textual position that precedes its *local_variable_declarator*.</span></span> <span data-ttu-id="4d3a5-174">如果局部变量声明是隐式的（[局部变量声明](statements.md#local-variable-declarations)），则在其*local_variable_declarator*中引用该变量也是错误的。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-174">If the local variable declaration is implicit ([Local variable declarations](statements.md#local-variable-declarations)), it is also an error to refer to the variable within its *local_variable_declarator*.</span></span>

<span data-ttu-id="4d3a5-175">*Foreach_statement*或*specific_catch_clause*引入的局部变量在整个范围内被视为已明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-175">A local variable introduced by a *foreach_statement* or a *specific_catch_clause* is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="4d3a5-176">局部变量的实际生存期取决于实现。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-176">The actual lifetime of a local variable is implementation-dependent.</span></span> <span data-ttu-id="4d3a5-177">例如，编译器可能会以静态方式确定块中的某个局部变量仅用于该块的一小部分。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-177">For example, a compiler might statically determine that a local variable in a block is only used for a small portion of that block.</span></span> <span data-ttu-id="4d3a5-178">使用此分析，编译器可以生成代码，使变量的存储具有比包含块更短的生存期。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-178">Using this analysis, the compiler could generate code that results in the variable's storage having a shorter lifetime than its containing block.</span></span>

<span data-ttu-id="4d3a5-179">局部引用变量所引用的存储将独立于该本地引用变量的生存期（[自动内存管理](basic-concepts.md#automatic-memory-management)）进行回收。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-179">The storage referred to by a local reference variable is reclaimed independently of the lifetime of that local reference variable ([Automatic memory management](basic-concepts.md#automatic-memory-management)).</span></span>

## <a name="default-values"></a><span data-ttu-id="4d3a5-180">默认值</span><span class="sxs-lookup"><span data-stu-id="4d3a5-180">Default values</span></span>

<span data-ttu-id="4d3a5-181">以下类别的变量会自动初始化为其默认值：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-181">The following categories of variables are automatically initialized to their default values:</span></span>

*  <span data-ttu-id="4d3a5-182">静态变量。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-182">Static variables.</span></span>
*  <span data-ttu-id="4d3a5-183">类实例的实例变量。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-183">Instance variables of class instances.</span></span>
*  <span data-ttu-id="4d3a5-184">数组元素。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-184">Array elements.</span></span>

<span data-ttu-id="4d3a5-185">变量的默认值取决于变量的类型，并按如下所示确定：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-185">The default value of a variable depends on the type of the variable and is determined as follows:</span></span>

*  <span data-ttu-id="4d3a5-186">对于*value_type*的变量，默认值与*value_type*的默认构造函数（[默认构造函数](types.md#default-constructors)）所计算的值相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-186">For a variable of a *value_type*, the default value is the same as the value computed by the *value_type*'s default constructor ([Default constructors](types.md#default-constructors)).</span></span>
*  <span data-ttu-id="4d3a5-187">对于*reference_type*的变量，默认值为`null`。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-187">For a variable of a *reference_type*, the default value is `null`.</span></span>

<span data-ttu-id="4d3a5-188">默认值的初始化通常是通过使内存管理器或垃圾回收器将内存初始化为全部位零来完成的，然后再将其分配给使用。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-188">Initialization to default values is typically done by having the memory manager or garbage collector initialize memory to all-bits-zero before it is allocated for use.</span></span> <span data-ttu-id="4d3a5-189">出于此原因，可以方便地使用所有位数表示空引用。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-189">For this reason, it is convenient to use all-bits-zero to represent the null reference.</span></span>

## <a name="definite-assignment"></a><span data-ttu-id="4d3a5-190">明确赋值</span><span class="sxs-lookup"><span data-stu-id="4d3a5-190">Definite assignment</span></span>

<span data-ttu-id="4d3a5-191">在函数成员的可执行代码中的给定位置 ***，如果编译器***可以通过特定的静态流分析（[确定明确赋值的精确规则](variables.md#precise-rules-for-determining-definite-assignment)）来证明变量已自动初始化或已成为至少一个赋值的目标。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-191">At a given location in the executable code of a function member, a variable is said to be ***definitely assigned*** if the compiler can prove, by a particular static flow analysis ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)), that the variable has been automatically initialized or has been the target of at least one assignment.</span></span> <span data-ttu-id="4d3a5-192">非正式地说，明确赋值的规则如下：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-192">Informally stated, the rules of definite assignment are:</span></span>

*  <span data-ttu-id="4d3a5-193">最初分配的变量（[最初分配的变量](variables.md#initially-assigned-variables)）始终被视为明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-193">An initially assigned variable ([Initially assigned variables](variables.md#initially-assigned-variables)) is always considered definitely assigned.</span></span>
*  <span data-ttu-id="4d3a5-194">如果指向该位置的所有可能的执行路径至少包含以下内容之一，则初始未分配的变量（[最初未分配](variables.md#initially-unassigned-variables)的变量）将被视为在给定位置明确赋值：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-194">An initially unassigned variable ([Initially unassigned variables](variables.md#initially-unassigned-variables)) is considered definitely assigned at a given location if all possible execution paths leading to that location contain at least one of the following:</span></span>
    * <span data-ttu-id="4d3a5-195">简单赋值（[简单赋值](expressions.md#simple-assignment)），其中变量是左操作数。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-195">A simple assignment ([Simple assignment](expressions.md#simple-assignment)) in which the variable is the left operand.</span></span>
    * <span data-ttu-id="4d3a5-196">将变量作为输出参数传递的调用表达式（[调用表达式](expressions.md#invocation-expressions)）或对象创建表达式（[对象创建表达式](expressions.md#object-creation-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-196">An invocation expression ([Invocation expressions](expressions.md#invocation-expressions)) or object creation expression ([Object creation expressions](expressions.md#object-creation-expressions)) that passes the variable as an output parameter.</span></span>
    * <span data-ttu-id="4d3a5-197">对于本地变量，则为包含变量初始值设定项的局部变量声明（[局部变量声明](statements.md#local-variable-declarations)）。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-197">For a local variable, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) that includes a variable initializer.</span></span>

<span data-ttu-id="4d3a5-198">[最初分配的变量](variables.md#initially-assigned-variables)、[最初未分配的变量](variables.md#initially-unassigned-variables)和[用于确定明确赋值的精确规则](variables.md#precise-rules-for-determining-definite-assignment)中介绍了上述非正式规则基础的正式规范。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-198">The formal specification underlying the above informal rules is described in [Initially assigned variables](variables.md#initially-assigned-variables), [Initially unassigned variables](variables.md#initially-unassigned-variables), and [Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment).</span></span>

<span data-ttu-id="4d3a5-199">*Struct_type*变量的实例变量的明确赋值状态是单独跟踪的。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-199">The definite assignment states of instance variables of a *struct_type* variable are tracked individually as well as collectively.</span></span> <span data-ttu-id="4d3a5-200">除了以上规则，以下规则适用于*struct_type*变量及其实例变量：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-200">In addition to the rules above, the following rules apply to *struct_type* variables and their instance variables:</span></span>

*  <span data-ttu-id="4d3a5-201">如果实例变量的包含*struct_type*变量被视为已明确赋值，则将其视为明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-201">An instance variable is considered definitely assigned if its containing *struct_type* variable is considered definitely assigned.</span></span>
*  <span data-ttu-id="4d3a5-202">如果将*struct_type*变量的每个实例变量都视为明确赋值，则该变量将被视为明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-202">A *struct_type* variable is considered definitely assigned if each of its instance variables is considered definitely assigned.</span></span>

<span data-ttu-id="4d3a5-203">在以下上下文中，明确赋值是必需的：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-203">Definite assignment is a requirement in the following contexts:</span></span>

*  <span data-ttu-id="4d3a5-204">变量必须在获取其值的每个位置明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-204">A variable must be definitely assigned at each location where its value is obtained.</span></span> <span data-ttu-id="4d3a5-205">这可确保永远不会出现未定义的值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-205">This ensures that undefined values never occur.</span></span> <span data-ttu-id="4d3a5-206">表达式中的变量的出现位置被视为获取该变量的值，但在</span><span class="sxs-lookup"><span data-stu-id="4d3a5-206">The occurrence of a variable in an expression is considered to obtain the value of the variable, except when</span></span>
    * <span data-ttu-id="4d3a5-207">变量是简单赋值的左操作数，</span><span class="sxs-lookup"><span data-stu-id="4d3a5-207">the variable is the left operand of a simple assignment,</span></span>
    * <span data-ttu-id="4d3a5-208">变量作为 output 参数传递，或</span><span class="sxs-lookup"><span data-stu-id="4d3a5-208">the variable is passed as an output parameter, or</span></span>
    * <span data-ttu-id="4d3a5-209">变量是*struct_type*变量，作为成员访问的左操作数出现。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-209">the variable is a *struct_type* variable and occurs as the left operand of a member access.</span></span>
*  <span data-ttu-id="4d3a5-210">变量必须在作为引用参数传递的每个位置上进行明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-210">A variable must be definitely assigned at each location where it is passed as a reference parameter.</span></span> <span data-ttu-id="4d3a5-211">这可确保被调用的函数成员可以考虑最初分配的引用参数。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-211">This ensures that the function member being invoked can consider the reference parameter initially assigned.</span></span>
*  <span data-ttu-id="4d3a5-212">函数成员的所有输出参数都必须在函数成员返回的每个位置明确赋值（通过`return`语句或通过执行到达函数成员主体的末尾）。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-212">All output parameters of a function member must be definitely assigned at each location where the function member returns (through a `return` statement or through execution reaching the end of the function member body).</span></span> <span data-ttu-id="4d3a5-213">这可确保函数成员不在输出参数中返回未定义的值，从而使编译器可以考虑使用变量作为输出参数的函数成员调用，该参数等效于对变量赋值的输出参数。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-213">This ensures that function members do not return undefined values in output parameters, thus enabling the compiler to consider a function member invocation that takes a variable as an output parameter equivalent to an assignment to the variable.</span></span>
*  <span data-ttu-id="4d3a5-214">必须在该实例构造函数返回的每个位置明确分配*struct_type*实例构造函数的变量。`this`</span><span class="sxs-lookup"><span data-stu-id="4d3a5-214">The `this` variable of a *struct_type* instance constructor must be definitely assigned at each location where that instance constructor returns.</span></span>

### <a name="initially-assigned-variables"></a><span data-ttu-id="4d3a5-215">最初分配的变量</span><span class="sxs-lookup"><span data-stu-id="4d3a5-215">Initially assigned variables</span></span>

<span data-ttu-id="4d3a5-216">以下类别的变量被分类为初始分配：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-216">The following categories of variables are classified as initially assigned:</span></span>

*  <span data-ttu-id="4d3a5-217">静态变量。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-217">Static variables.</span></span>
*  <span data-ttu-id="4d3a5-218">类实例的实例变量。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-218">Instance variables of class instances.</span></span>
*  <span data-ttu-id="4d3a5-219">最初分配的结构变量的实例变量。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-219">Instance variables of initially assigned struct variables.</span></span>
*  <span data-ttu-id="4d3a5-220">数组元素。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-220">Array elements.</span></span>
*  <span data-ttu-id="4d3a5-221">值参数。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-221">Value parameters.</span></span>
*  <span data-ttu-id="4d3a5-222">引用参数。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-222">Reference parameters.</span></span>
*  <span data-ttu-id="4d3a5-223">`catch` 子句`foreach`或语句中声明的变量。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-223">Variables declared in a `catch` clause or a `foreach` statement.</span></span>

### <a name="initially-unassigned-variables"></a><span data-ttu-id="4d3a5-224">初始未赋值变量</span><span class="sxs-lookup"><span data-stu-id="4d3a5-224">Initially unassigned variables</span></span>

<span data-ttu-id="4d3a5-225">以下类别的变量被分类为初始未赋值：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-225">The following categories of variables are classified as initially unassigned:</span></span>

*  <span data-ttu-id="4d3a5-226">初始未分配的结构变量的实例变量。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-226">Instance variables of initially unassigned struct variables.</span></span>
*  <span data-ttu-id="4d3a5-227">输出参数，包括`this`结构实例构造函数的变量。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-227">Output parameters, including the `this` variable of struct instance constructors.</span></span>
*  <span data-ttu-id="4d3a5-228">局部变量（ `catch`子句`foreach`或语句中声明的变量除外）。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-228">Local variables, except those declared in a `catch` clause or a `foreach` statement.</span></span>

### <a name="precise-rules-for-determining-definite-assignment"></a><span data-ttu-id="4d3a5-229">确定明确赋值的精确规则</span><span class="sxs-lookup"><span data-stu-id="4d3a5-229">Precise rules for determining definite assignment</span></span>

<span data-ttu-id="4d3a5-230">为了确定每个已使用的变量是否已明确赋值，编译器必须使用与本部分中所述等效的进程。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-230">In order to determine that each used variable is definitely assigned, the compiler must use a process that is equivalent to the one described in this section.</span></span>

<span data-ttu-id="4d3a5-231">编译器处理每个具有一个或多个初始未赋值变量的函数成员的正文。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-231">The compiler processes the body of each function member that has one or more initially unassigned variables.</span></span> <span data-ttu-id="4d3a5-232">对于每个初始未赋值的变量*v*，编译器会在函数成员的以下各点处确定*v*的***明确赋值状态***：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-232">For each initially unassigned variable *v*, the compiler determines a ***definite assignment state*** for *v* at each of the following points in the function member:</span></span>

*  <span data-ttu-id="4d3a5-233">在每个语句的开头</span><span class="sxs-lookup"><span data-stu-id="4d3a5-233">At the beginning of each statement</span></span>
*  <span data-ttu-id="4d3a5-234">每个语句的终结点（[终结点和可访问](statements.md#end-points-and-reachability)性）</span><span class="sxs-lookup"><span data-stu-id="4d3a5-234">At the end point ([End points and reachability](statements.md#end-points-and-reachability)) of each statement</span></span>
*  <span data-ttu-id="4d3a5-235">在将控制转移到另一条语句或语句结束点的每个弧线上</span><span class="sxs-lookup"><span data-stu-id="4d3a5-235">On each arc which transfers control to another statement or to the end point of a statement</span></span>
*  <span data-ttu-id="4d3a5-236">在每个表达式的开头</span><span class="sxs-lookup"><span data-stu-id="4d3a5-236">At the beginning of each expression</span></span>
*  <span data-ttu-id="4d3a5-237">在每个表达式的末尾</span><span class="sxs-lookup"><span data-stu-id="4d3a5-237">At the end of each expression</span></span>

<span data-ttu-id="4d3a5-238">*V*的明确赋值状态可以是：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-238">The definite assignment state of *v* can be either:</span></span>

*  <span data-ttu-id="4d3a5-239">明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-239">Definitely assigned.</span></span> <span data-ttu-id="4d3a5-240">这表明，在所有可能的控制流到目前为止， *v*已分配有一个值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-240">This indicates that on all possible control flows to this point, *v* has been assigned a value.</span></span>
*  <span data-ttu-id="4d3a5-241">未明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-241">Not definitely assigned.</span></span> <span data-ttu-id="4d3a5-242">对于类型`bool`为的表达式末尾的变量状态，未明确赋值的变量的状态可能（但不一定）属于以下子状态之一：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-242">For the state of a variable at the end of an expression of type `bool`, the state of a variable that isn't definitely assigned may (but doesn't necessarily) fall into one of the following sub-states:</span></span>
    * <span data-ttu-id="4d3a5-243">在 true 表达式后明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-243">Definitely assigned after true expression.</span></span> <span data-ttu-id="4d3a5-244">此状态表明，如果布尔表达式的计算结果为 true，则会明确赋值*v* ; 如果布尔表达式的计算结果为 false，则不一定赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-244">This state indicates that *v* is definitely assigned if the boolean expression evaluated as true, but is not necessarily assigned if the boolean expression evaluated as false.</span></span>
    * <span data-ttu-id="4d3a5-245">在 false 表达式后明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-245">Definitely assigned after false expression.</span></span> <span data-ttu-id="4d3a5-246">此状态表明，如果布尔表达式的计算结果为 false，则会明确赋值*v* ; 如果布尔表达式的计算结果为 true，则不一定赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-246">This state indicates that *v* is definitely assigned if the boolean expression evaluated as false, but is not necessarily assigned if the boolean expression evaluated as true.</span></span>

<span data-ttu-id="4d3a5-247">以下规则控制如何在每个位置确定变量*v*的状态。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-247">The following rules govern how the state of a variable *v* is determined at each location.</span></span>

#### <a name="general-rules-for-statements"></a><span data-ttu-id="4d3a5-248">语句的一般规则</span><span class="sxs-lookup"><span data-stu-id="4d3a5-248">General rules for statements</span></span>

*  <span data-ttu-id="4d3a5-249">在函数成员主体的开头， *v*未明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-249">*v* is not definitely assigned at the beginning of a function member body.</span></span>
*  <span data-ttu-id="4d3a5-250">在任何无法访问的语句开始时，均明确赋值*v* 。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-250">*v* is definitely assigned at the beginning of any unreachable statement.</span></span>
*  <span data-ttu-id="4d3a5-251">*V*在任何其他语句开始时的明确赋值状态是通过检查*v*在该语句开头的所有控制流传输上的明确赋值状态。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-251">The definite assignment state of *v* at the beginning of any other statement is determined by checking the definite assignment state of *v* on all control flow transfers that target the beginning of that statement.</span></span> <span data-ttu-id="4d3a5-252">如果在所有此类控制流传输上明确分配了（且仅当） *v* ，则在语句的开头明确赋值*v* 。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-252">If (and only if) *v* is definitely assigned on all such control flow transfers, then *v* is definitely assigned at the beginning of the statement.</span></span> <span data-ttu-id="4d3a5-253">可能的控制流传输集的确定方式与检查语句可访问性（[终结点和可访问](statements.md#end-points-and-reachability)性）相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-253">The set of possible control flow transfers is determined in the same way as for checking statement reachability ([End points and reachability](statements.md#end-points-and-reachability)).</span></span>
*  <span data-ttu-id="4d3a5-254">*V*在`checked`块、 `do` `unchecked` 、、`if`、、、、、、或的终结点上的明确赋值状态`while` `for` `foreach` `lock` `using` `switch`语句是通过检查以该语句的终结点为目标的所有控制流传输上的*v*的明确赋值状态来确定的。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-254">The definite assignment state of *v* at the end point of a block, `checked`, `unchecked`, `if`, `while`, `do`, `for`, `foreach`, `lock`, `using`, or `switch` statement is determined by checking the definite assignment state of *v* on all control flow transfers that target the end point of that statement.</span></span> <span data-ttu-id="4d3a5-255">如果对所有此类控制流传输明确赋值*v* ，则*v*在语句的终结点上是明确赋值的。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-255">If *v* is definitely assigned on all such control flow transfers, then *v* is definitely assigned at the end point of the statement.</span></span> <span data-ttu-id="4d3a5-256">本来在语句的结束点上不明确赋值*v* 。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-256">Otherwise; *v* is not definitely assigned at the end point of the statement.</span></span> <span data-ttu-id="4d3a5-257">可能的控制流传输集的确定方式与检查语句可访问性（[终结点和可访问](statements.md#end-points-and-reachability)性）相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-257">The set of possible control flow transfers is determined in the same way as for checking statement reachability ([End points and reachability](statements.md#end-points-and-reachability)).</span></span>

#### <a name="block-statements-checked-and-unchecked-statements"></a><span data-ttu-id="4d3a5-258">Block 语句、checked 和 unchecked 语句</span><span class="sxs-lookup"><span data-stu-id="4d3a5-258">Block statements, checked, and unchecked statements</span></span>

<span data-ttu-id="4d3a5-259">如果控件传输到块中语句列表的第一条语句（如果语句列表为空，则为到块的终结点），则控件上的*v*的明确赋值状态与块之前的*v*的明确赋值语句相同。、 `checked`或`unchecked`语句。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-259">The definite assignment state of *v* on the control transfer to the first statement of the statement list in the block (or to the end point of the block, if the statement list is empty) is the same as the definite assignment statement of *v* before the block, `checked`, or `unchecked` statement.</span></span>

#### <a name="expression-statements"></a><span data-ttu-id="4d3a5-260">表达式语句</span><span class="sxs-lookup"><span data-stu-id="4d3a5-260">Expression statements</span></span>

<span data-ttu-id="4d3a5-261">对于包含表达式*expr*的表达式语句*stmt* ：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-261">For an expression statement *stmt* that consists of the expression *expr*:</span></span>

*  <span data-ttu-id="4d3a5-262">在*stmt*开始时， *v*在*expr*开头具有相同的明确赋值状态。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-262">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="4d3a5-263">如果在*expr*的末尾指定了 " *v* "，则它在*stmt*的终结点上进行明确赋值;本来它不是在*stmt*终结点明确赋值的。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-263">If *v* if definitely assigned at the end of *expr*, it is definitely assigned at the end point of *stmt*; otherwise; it is not definitely assigned at the end point of *stmt*.</span></span>

#### <a name="declaration-statements"></a><span data-ttu-id="4d3a5-264">声明语句</span><span class="sxs-lookup"><span data-stu-id="4d3a5-264">Declaration statements</span></span>

*  <span data-ttu-id="4d3a5-265">如果*stmt*是没有初始值设定项的声明语句，则*v*在*stmt*的终结点上的明确赋值状态与*stmt*的开头相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-265">If *stmt* is a declaration statement without initializers, then *v* has the same definite assignment state at the end point of *stmt* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="4d3a5-266">如果*stmt*是带有初始值设定项的声明语句，则*v*的明确赋值状态被确定为 ，但对于带有初始值设定项的每个声明使用一个赋值语句（顺序为声明）。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-266">If *stmt* is a declaration statement with initializers, then the definite assignment state for *v* is determined as if *stmt* were a statement list, with one assignment statement for each declaration with an initializer (in the order of declaration).</span></span>

#### <a name="if-statements"></a><span data-ttu-id="4d3a5-267">If 语句</span><span class="sxs-lookup"><span data-stu-id="4d3a5-267">If statements</span></span>

<span data-ttu-id="4d3a5-268">对于窗体的语句`if` stmt：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-268">For an `if` statement *stmt* of the form:</span></span>
```csharp
if ( expr ) then_stmt else else_stmt
```

*  <span data-ttu-id="4d3a5-269">在*stmt*开始时， *v*在*expr*开头具有相同的明确赋值状态。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-269">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="4d3a5-270">如果在*expr*结束时， *v*是明确赋值的，则在到*then_stmt*的控制流传输上，并将其指定为*else_stmt* ，如果没有 else 子句，则将其分配给*stmt*的终结点。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-270">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to *then_stmt* and to either *else_stmt* or to the end-point of *stmt* if there is no else clause.</span></span>
*  <span data-ttu-id="4d3a5-271">如果在*expr*的末尾， *v*的状态为 "的确赋值为 true expression"，则它在控制流到*then_stmt*的传输上明确赋值，而不是在控制流传输上明确分配到任一*else_* 如果没有 else 子句，则为 stmt 或*stmt*的终结点。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-271">If *v* has the state "definitely assigned after true expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *then_stmt*, and not definitely assigned on the control flow transfer to either *else_stmt* or to the end-point of *stmt* if there is no else clause.</span></span>
*  <span data-ttu-id="4d3a5-272">如果*v*在*expr*的末尾具有状态 "在假表达式后明确赋值"，则它在控制流到*else_stmt*的传输上明确赋值，在控制流到 then_stmt 的传输上未明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-272">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *else_stmt*, and not definitely assigned on the control flow transfer to *then_stmt*.</span></span> <span data-ttu-id="4d3a5-273">当且仅当在*then_stmt*的终结点明确赋值时，它才会在*stmt*的终结点上进行明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-273">It is definitely assigned at the end-point of *stmt* if and only if it is definitely assigned at the end-point of *then_stmt*.</span></span>
*  <span data-ttu-id="4d3a5-274">否则，在对*then_stmt*或*else_stmt*的控制流传输上， *v*被视为未明确分配，如果不存在 else 子句，则被视为*stmt*的终结点。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-274">Otherwise, *v* is considered not definitely assigned on the control flow transfer to either the *then_stmt* or *else_stmt*, or to the end-point of *stmt* if there is no else clause.</span></span>

#### <a name="switch-statements"></a><span data-ttu-id="4d3a5-275">Switch 语句</span><span class="sxs-lookup"><span data-stu-id="4d3a5-275">Switch statements</span></span>

<span data-ttu-id="4d3a5-276">使用控制表达式 *expr*的语句`switch` stmt：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-276">In a `switch` statement *stmt* with a controlling expression *expr*:</span></span>

*  <span data-ttu-id="4d3a5-277">*Expr*开头的*v*的明确赋值状态与*stmt*开头的*v*的状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-277">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="4d3a5-278">到可访问的 switch 块语句列表的控制流传输上， *v*的明确分配状态与*expr*末尾的*v*的明确赋值状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-278">The definite assignment state of *v* on the control flow transfer to a reachable switch block statement list is the same as the definite assignment state of *v* at the end of *expr*.</span></span>

#### <a name="while-statements"></a><span data-ttu-id="4d3a5-279">While 语句</span><span class="sxs-lookup"><span data-stu-id="4d3a5-279">While statements</span></span>

<span data-ttu-id="4d3a5-280">对于窗体的语句`while` stmt：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-280">For a `while` statement *stmt* of the form:</span></span>
```csharp
while ( expr ) while_body
```

*  <span data-ttu-id="4d3a5-281">在*stmt*开始时， *v*在*expr*开头具有相同的明确赋值状态。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-281">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="4d3a5-282">如果在*expr*结束时， *v*是明确赋值的，则它在控制流到*while_body*的传递和到*stmt*的结束点上都是明确赋值的。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-282">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to *while_body* and to the end point of *stmt*.</span></span>
*  <span data-ttu-id="4d3a5-283">如果在*expr*的末尾， *v*的状态为 "明确分配给 true 表达式"，则它在控制流到*while_body*的传输上明确赋值，但不是在*stmt*终结点明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-283">If *v* has the state "definitely assigned after true expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *while_body*, but not definitely assigned at the end-point of *stmt*.</span></span>
*  <span data-ttu-id="4d3a5-284">如果在*expr*的末尾， *v*的状态为 "指定为 false expression 后明确赋值"，则它在控制流到*stmt*终点的传输上明确赋值，但并不是在控制流传输时明确赋值。 *_body*。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-284">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*, but not definitely assigned on the control flow transfer to *while_body*.</span></span>

#### <a name="do-statements"></a><span data-ttu-id="4d3a5-285">Do 语句</span><span class="sxs-lookup"><span data-stu-id="4d3a5-285">Do statements</span></span>

<span data-ttu-id="4d3a5-286">对于窗体的语句`do` stmt：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-286">For a `do` statement *stmt* of the form:</span></span>
```csharp
do do_body while ( expr ) ;
```

*  <span data-ttu-id="4d3a5-287">*v*在从*stmt*开始到*do_body*的控制流传输上具有相同的明确赋值状态，就像在*stmt*的开头。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-287">*v* has the same definite assignment state on the control flow transfer from the beginning of *stmt* to *do_body* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="4d3a5-288">对于位于*do_body*的终结点的*expr* ， *v*具有相同的明确赋值状态。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-288">*v* has the same definite assignment state at the beginning of *expr* as at the end point of *do_body*.</span></span>
*  <span data-ttu-id="4d3a5-289">如果在*expr*结束时， *v*是明确赋值的，则它在控制流到*stmt*终点的传输上明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-289">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*.</span></span>
*  <span data-ttu-id="4d3a5-290">如果在*expr*的末尾， *v*的状态为 "在假表达式后明确赋值"，则它在控制流到*stmt*终点的传输上明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-290">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*.</span></span>

#### <a name="for-statements"></a><span data-ttu-id="4d3a5-291">For 语句</span><span class="sxs-lookup"><span data-stu-id="4d3a5-291">For statements</span></span>

<span data-ttu-id="4d3a5-292">对以下形式的`for`语句的明确赋值检查：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-292">Definite assignment checking for a `for` statement of the form:</span></span>
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
<span data-ttu-id="4d3a5-293">像编写语句一样完成：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-293">is done as if the statement were written:</span></span>
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

<span data-ttu-id="4d3a5-294">如果`for`语句中省略*for_condition* ，则对明确赋值的计算将继续执行，就像上述扩展`true`中已将*for_condition*替换为。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-294">If the *for_condition* is omitted from the `for` statement, then evaluation of definite assignment proceeds as if *for_condition* were replaced with `true` in the above expansion.</span></span>

#### <a name="break-continue-and-goto-statements"></a><span data-ttu-id="4d3a5-295">Break、continue 和 goto 语句</span><span class="sxs-lookup"><span data-stu-id="4d3a5-295">Break, continue, and goto statements</span></span>

<span data-ttu-id="4d3a5-296">由 `break`、或语句`goto`导致的控制流传输上的 v 的明确赋值状态与语句开头的 v 的明确赋值状态相同。 `continue`</span><span class="sxs-lookup"><span data-stu-id="4d3a5-296">The definite assignment state of *v* on the control flow transfer caused by a `break`, `continue`, or `goto` statement is the same as the definite assignment state of *v* at the beginning of the statement.</span></span>

#### <a name="throw-statements"></a><span data-ttu-id="4d3a5-297">Throw 语句</span><span class="sxs-lookup"><span data-stu-id="4d3a5-297">Throw statements</span></span>

<span data-ttu-id="4d3a5-298">对于窗体的语句*stmt*</span><span class="sxs-lookup"><span data-stu-id="4d3a5-298">For a statement *stmt* of the form</span></span>
```csharp
throw expr ;
```

<span data-ttu-id="4d3a5-299">*Expr*开头的*v*的明确赋值状态与*stmt*开头的*v*的明确赋值状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-299">The definite assignment state of *v* at the beginning of *expr* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>

#### <a name="return-statements"></a><span data-ttu-id="4d3a5-300">Return 语句</span><span class="sxs-lookup"><span data-stu-id="4d3a5-300">Return statements</span></span>

<span data-ttu-id="4d3a5-301">对于窗体的语句*stmt*</span><span class="sxs-lookup"><span data-stu-id="4d3a5-301">For a statement *stmt* of the form</span></span>
```csharp
return expr ;
```

*  <span data-ttu-id="4d3a5-302">*Expr*开头的*v*的明确赋值状态与*stmt*开头的*v*的明确赋值状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-302">The definite assignment state of *v* at the beginning of *expr* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="4d3a5-303">如果*v*是 output 参数，则必须为其赋值：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-303">If *v* is an output parameter, then it must be definitely assigned either:</span></span>
    * <span data-ttu-id="4d3a5-304">*expr*之后</span><span class="sxs-lookup"><span data-stu-id="4d3a5-304">after *expr*</span></span>
    * <span data-ttu-id="4d3a5-305">或`finally`包含语句`return`的块`try` - `finally` 的结尾- 。 `try` `catch` - `finally`</span><span class="sxs-lookup"><span data-stu-id="4d3a5-305">or at the end of the `finally` block of a `try`-`finally` or `try`-`catch`-`finally` that encloses the `return` statement.</span></span>

<span data-ttu-id="4d3a5-306">对于窗体的语句 stmt：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-306">For a statement stmt of the form:</span></span>
```csharp
return ;
```

*  <span data-ttu-id="4d3a5-307">如果*v*是 output 参数，则必须为其赋值：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-307">If *v* is an output parameter, then it must be definitely assigned either:</span></span>
    * <span data-ttu-id="4d3a5-308">*stmt*之前</span><span class="sxs-lookup"><span data-stu-id="4d3a5-308">before *stmt*</span></span>
    * <span data-ttu-id="4d3a5-309">或`finally`包含语句`return`的块`try` - `finally` 的结尾- 。 `try` `catch` - `finally`</span><span class="sxs-lookup"><span data-stu-id="4d3a5-309">or at the end of the `finally` block of a `try`-`finally` or `try`-`catch`-`finally` that encloses the `return` statement.</span></span>

#### <a name="try-catch-statements"></a><span data-ttu-id="4d3a5-310">Try-catch 语句</span><span class="sxs-lookup"><span data-stu-id="4d3a5-310">Try-catch statements</span></span>

<span data-ttu-id="4d3a5-311">对于窗体的语句*stmt* ：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-311">For a statement *stmt* of the form:</span></span>
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  <span data-ttu-id="4d3a5-312">在*try_block*开始时， *v*的明确赋值状态与*stmt*开头的*v*的明确赋值状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-312">The definite assignment state of *v* at the beginning of *try_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="4d3a5-313">*Catch_block_i*开头的*v*的明确分配状态（适用于任何*i*）与*stmt*开头的*v*的明确赋值状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-313">The definite assignment state of *v* at the beginning of *catch_block_i* (for any *i*) is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="4d3a5-314">如果在*try_block*的终结点和每个 catch_block_i （*对于每个* *，则为*1 到 n 的每个 ）明确分配*v*时，).</span><span class="sxs-lookup"><span data-stu-id="4d3a5-314">The definite assignment state of *v* at the end-point of *stmt* is definitely assigned if (and only if) *v* is definitely assigned at the end-point of *try_block* and every *catch_block_i* (for every *i* from 1 to *n*).</span></span>

#### <a name="try-finally-statements"></a><span data-ttu-id="4d3a5-315">Try-catch 语句</span><span class="sxs-lookup"><span data-stu-id="4d3a5-315">Try-finally statements</span></span>

<span data-ttu-id="4d3a5-316">对于窗体的语句`try` stmt：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-316">For a `try` statement *stmt* of the form:</span></span>
```csharp
try try_block finally finally_block
```

*  <span data-ttu-id="4d3a5-317">在*try_block*开始时， *v*的明确赋值状态与*stmt*开头的*v*的明确赋值状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-317">The definite assignment state of *v* at the beginning of *try_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="4d3a5-318">在*finally_block*开始时， *v*的明确赋值状态与*stmt*开头的*v*的明确赋值状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-318">The definite assignment state of *v* at the beginning of *finally_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="4d3a5-319">如果（且仅当）满足以下条件之一，则会明确分配*v*在*stmt*终结点上的明确赋值状态：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-319">The definite assignment state of *v* at the end-point of *stmt* is definitely assigned if (and only if) at least one of the following is true:</span></span>
    * <span data-ttu-id="4d3a5-320">在*try_block*的终结点明确赋值*v*</span><span class="sxs-lookup"><span data-stu-id="4d3a5-320">*v* is definitely assigned at the end-point of *try_block*</span></span>
    * <span data-ttu-id="4d3a5-321">在*finally_block*的终结点明确赋值*v*</span><span class="sxs-lookup"><span data-stu-id="4d3a5-321">*v* is definitely assigned at the end-point of *finally_block*</span></span>

<span data-ttu-id="4d3a5-322">如果控制`goto`流传输（例如，语句）是在*try_block*中开始，并在*try_block*之外结束，则在该控制流传输上， *v*也被视为明确赋值（如果*v*是在*finally_block*的终结点明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-322">If a control flow transfer (for example, a `goto` statement) is made that begins within *try_block*, and ends outside of *try_block*, then *v* is also considered definitely assigned on that control flow transfer if *v* is definitely assigned at the end-point of *finally_block*.</span></span> <span data-ttu-id="4d3a5-323">（这并不是唯一的情况，如果对此控制流传输的另一个原因明确指定*v* ，则仍将其视为明确分配。）</span><span class="sxs-lookup"><span data-stu-id="4d3a5-323">(This is not an only if—if *v* is definitely assigned for another reason on this control flow transfer, then it is still considered definitely assigned.)</span></span>

#### <a name="try-catch-finally-statements"></a><span data-ttu-id="4d3a5-324">Try-catch-finally 语句</span><span class="sxs-lookup"><span data-stu-id="4d3a5-324">Try-catch-finally statements</span></span>

<span data-ttu-id="4d3a5-325">对`try` 以下形式`catch`的语句的`finally`明确赋值分析： - -</span><span class="sxs-lookup"><span data-stu-id="4d3a5-325">Definite assignment analysis for a `try`-`catch`-`finally` statement of the form:</span></span>
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
<span data-ttu-id="4d3a5-326">如`try`语句-是包含语句的`try`语句一样完成： `finally` - `catch`</span><span class="sxs-lookup"><span data-stu-id="4d3a5-326">is done as if the statement were a `try`-`finally` statement enclosing a `try`-`catch` statement:</span></span>
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

<span data-ttu-id="4d3a5-327">下面的示例演示`try`语句的不同块（[try 语句](statements.md#the-try-statement)）如何影响明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-327">The following example demonstrates how the different blocks of a `try` statement ([The try statement](statements.md#the-try-statement)) affect definite assignment.</span></span>
```csharp
class A
{
    static void F() {
        int i, j;
        try {
            goto LABEL;
            // neither i nor j definitely assigned
            i = 1;
            // i definitely assigned
        }

        catch {
            // neither i nor j definitely assigned
            i = 3;
            // i definitely assigned
        }

        finally {
            // neither i nor j definitely assigned
            j = 5;
            // j definitely assigned
            }
        // i and j definitely assigned
        LABEL:;
        // j definitely assigned

    }
}
```

#### <a name="foreach-statements"></a><span data-ttu-id="4d3a5-328">Foreach 语句</span><span class="sxs-lookup"><span data-stu-id="4d3a5-328">Foreach statements</span></span>

<span data-ttu-id="4d3a5-329">对于窗体的语句`foreach` stmt：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-329">For a `foreach` statement *stmt* of the form:</span></span>
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  <span data-ttu-id="4d3a5-330">*Expr*开头的*v*的明确赋值状态与*stmt*开头的*v*的状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-330">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="4d3a5-331">将控制流传输到*embedded_statement*或*stmt*终结点时， *v*的明确分配状态与*expr*末尾处的*v*状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-331">The definite assignment state of *v* on the control flow transfer to *embedded_statement* or to the end point of *stmt* is the same as the state of *v* at the end of *expr*.</span></span>

#### <a name="using-statements"></a><span data-ttu-id="4d3a5-332">Using 语句</span><span class="sxs-lookup"><span data-stu-id="4d3a5-332">Using statements</span></span>

<span data-ttu-id="4d3a5-333">对于窗体的语句`using` stmt：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-333">For a `using` statement *stmt* of the form:</span></span>
```csharp
using ( resource_acquisition ) embedded_statement
```

*  <span data-ttu-id="4d3a5-334">在*resource_acquisition*开始时， *v*的明确赋值状态与*stmt*开头的*v*的状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-334">The definite assignment state of *v* at the beginning of *resource_acquisition* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="4d3a5-335">控制流传输到*embedded_statement*的*v*的明确分配状态与*resource_acquisition*末尾的*v*状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-335">The definite assignment state of *v* on the control flow transfer to *embedded_statement* is the same as the state of *v* at the end of *resource_acquisition*.</span></span>

#### <a name="lock-statements"></a><span data-ttu-id="4d3a5-336">Lock 语句</span><span class="sxs-lookup"><span data-stu-id="4d3a5-336">Lock statements</span></span>

<span data-ttu-id="4d3a5-337">对于窗体的语句`lock` stmt：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-337">For a `lock` statement *stmt* of the form:</span></span>
```csharp
lock ( expr ) embedded_statement
```

*  <span data-ttu-id="4d3a5-338">*Expr*开头的*v*的明确赋值状态与*stmt*开头的*v*的状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-338">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="4d3a5-339">控制流传输到*embedded_statement*的*v*的明确分配状态与*expr*末尾处的*v*状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-339">The definite assignment state of *v* on the control flow transfer to *embedded_statement* is the same as the state of *v* at the end of *expr*.</span></span>

#### <a name="yield-statements"></a><span data-ttu-id="4d3a5-340">Yield 语句</span><span class="sxs-lookup"><span data-stu-id="4d3a5-340">Yield statements</span></span>

<span data-ttu-id="4d3a5-341">对于窗体的语句`yield return` stmt：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-341">For a `yield return` statement *stmt* of the form:</span></span>
```csharp
yield return expr ;
```

*  <span data-ttu-id="4d3a5-342">*Expr*开头的*v*的明确赋值状态与*stmt*开头的*v*的状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-342">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="4d3a5-343">在*stmt*结束时， *v*的明确赋值状态与*expr*末尾的*v*状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-343">The definite assignment state of *v* at the end of *stmt* is the same as the state of *v* at the end of *expr*.</span></span>
*  <span data-ttu-id="4d3a5-344">`yield break`语句对明确赋值状态没有影响。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-344">A `yield break` statement has no effect on the definite assignment state.</span></span>

#### <a name="general-rules-for-simple-expressions"></a><span data-ttu-id="4d3a5-345">简单表达式的一般规则</span><span class="sxs-lookup"><span data-stu-id="4d3a5-345">General rules for simple expressions</span></span>

<span data-ttu-id="4d3a5-346">以下规则适用于这些类型的表达式：文本（[文本](expressions.md#literals)）、简单名称（[简单名称](expressions.md#simple-names)）、成员访问表达式（[成员访问](expressions.md#member-access)）、非索引的基本访问表达式（[基本访问](expressions.md#base-access)）、 `typeof`表达式（[typeof 运算符](expressions.md#the-typeof-operator)）、默认值表达式（[默认值表达式](expressions.md#default-value-expressions)）和`nameof`表达式（[Nameof 表达式](expressions.md#nameof-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-346">The following rule applies to these kinds of expressions: literals ([Literals](expressions.md#literals)), simple names ([Simple names](expressions.md#simple-names)), member access expressions ([Member access](expressions.md#member-access)), non-indexed base access expressions ([Base access](expressions.md#base-access)), `typeof` expressions ([The typeof operator](expressions.md#the-typeof-operator)), default value expressions ([Default value expressions](expressions.md#default-value-expressions)) and `nameof` expressions ([Nameof expressions](expressions.md#nameof-expressions)).</span></span>

*  <span data-ttu-id="4d3a5-347">此类表达式结束时， *v*的明确赋值状态与表达式开头*v*的明确赋值状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-347">The definite assignment state of *v* at the end of such an expression is the same as the definite assignment state of *v* at the beginning of the expression.</span></span>

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a><span data-ttu-id="4d3a5-348">带有嵌入式表达式的表达式的一般规则</span><span class="sxs-lookup"><span data-stu-id="4d3a5-348">General rules for expressions with embedded expressions</span></span>

<span data-ttu-id="4d3a5-349">以下规则适用于这些类型的表达式：带圆括号的表达式（[带括号的表达式](expressions.md#parenthesized-expressions)）、元素访问表达式（[元素访问](expressions.md#element-access)）、带有索引的基本访问表达式（[基本访问](expressions.md#base-access)）、增量和减量表达式（[后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators)，[前缀增量和减量运算符](expressions.md#prefix-increment-and-decrement-operators)），强制转换表达式（[强制转换表达式](expressions.md#cast-expressions)） `+`， `-`一元`~`，，， `*`表达式、二进制`+` `-` 、`*` 、`/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`, `|`, `^` 表达式 （[算术运算符](expressions.md#arithmetic-operators)、[移位运算符](expressions.md#shift-operators)、[关系和类型测试运算符](expressions.md#relational-and-type-testing-operators)、[逻辑运算符](expressions.md#logical-operators)）、复合赋值表达式（[复合](expressions.md#compound-assignment) `checked`赋值）和`unchecked`表达式（[checked 和 unchecked 运算符](expressions.md#the-checked-and-unchecked-operators)）以及数组和委托创建表达式（[new 运算符](expressions.md#the-new-operator)）。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-349">The following rules apply to these kinds of expressions: parenthesized expressions ([Parenthesized expressions](expressions.md#parenthesized-expressions)), element access expressions ([Element access](expressions.md#element-access)), base access expressions with indexing ([Base access](expressions.md#base-access)), increment and decrement expressions ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), cast expressions ([Cast expressions](expressions.md#cast-expressions)), unary `+`, `-`, `~`, `*` expressions, binary `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`, `|`, `^` expressions ([Arithmetic operators](expressions.md#arithmetic-operators), [Shift operators](expressions.md#shift-operators), [Relational and type-testing operators](expressions.md#relational-and-type-testing-operators), [Logical operators](expressions.md#logical-operators)), compound assignment expressions ([Compound assignment](expressions.md#compound-assignment)), `checked` and `unchecked` expressions ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), plus array and delegate creation expressions ([The new operator](expressions.md#the-new-operator)).</span></span>

<span data-ttu-id="4d3a5-350">其中每个表达式都具有一个或多个按固定顺序无条件计算的子表达式。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-350">Each of these expressions has one or more sub-expressions that are unconditionally evaluated in a fixed order.</span></span> <span data-ttu-id="4d3a5-351">例如，二元`%`运算符计算运算符的左侧，然后计算右侧。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-351">For example, the binary `%` operator evaluates the left hand side of the operator, then the right hand side.</span></span> <span data-ttu-id="4d3a5-352">索引操作将计算索引表达式的值，然后按从左至右的顺序计算每个索引表达式。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-352">An indexing operation evaluates the indexed expression, and then evaluates each of the index expressions, in order from left to right.</span></span> <span data-ttu-id="4d3a5-353">对于具有子表达式*e1、e2、...、eN*的表达式*expr*，按以下顺序计算：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-353">For an expression *expr*, which has sub-expressions *e1, e2, ..., eN*, evaluated in that order:</span></span>

*  <span data-ttu-id="4d3a5-354">在*e1*开始时， *v*的明确赋值状态与*expr*开头的明确赋值状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-354">The definite assignment state of *v* at the beginning of *e1* is the same as the definite assignment state at the beginning of *expr*.</span></span>
*  <span data-ttu-id="4d3a5-355">*Ei* （*i*大于1）开头的*v*的明确赋值状态与上一个子表达式末尾的明确赋值状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-355">The definite assignment state of *v* at the beginning of *ei* (*i* greater than one) is the same as the definite assignment state at the end of the previous sub-expression.</span></span>
*  <span data-ttu-id="4d3a5-356">在*expr*结束时， *v*的明确赋值状态与*eN*末尾的明确赋值状态相同</span><span class="sxs-lookup"><span data-stu-id="4d3a5-356">The definite assignment state of *v* at the end of *expr* is the same as the definite assignment state at the end of *eN*</span></span>

#### <a name="invocation-expressions-and-object-creation-expressions"></a><span data-ttu-id="4d3a5-357">调用表达式和对象创建表达式</span><span class="sxs-lookup"><span data-stu-id="4d3a5-357">Invocation expressions and object creation expressions</span></span>

<span data-ttu-id="4d3a5-358">对于以下形式的调用表达式*expr* ：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-358">For an invocation expression *expr* of the form:</span></span>
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
<span data-ttu-id="4d3a5-359">或形式的对象创建表达式：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-359">or an object creation expression of the form:</span></span>
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  <span data-ttu-id="4d3a5-360">对于调用表达式， *v*之前的*primary_expression*的明确赋值状态与*expr*前面的*v*的状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-360">For an invocation expression, the definite assignment state of *v* before *primary_expression* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="4d3a5-361">对于调用表达式，v 之前的*v*的明确赋值状态*与* *primary_expression*后*v*的状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-361">For an invocation expression, the definite assignment state of *v* before *arg1* is the same as the state of *v* after *primary_expression*.</span></span>
*  <span data-ttu-id="4d3a5-362">对于对象创建表达式，v 之前的*v*的明确赋值状态*与* *expr*前面的*v*的状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-362">For an object creation expression, the definite assignment state of *v* before *arg1* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="4d3a5-363">对于每个*参数 argi*， *argi*之后的`ref` *v*的明确赋值状态由标准表达式规则确定，忽略任何或`out`修饰符。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-363">For each argument *argi*, the definite assignment state of *v* after *argi* is determined by the normal expression rules, ignoring any `ref` or `out` modifiers.</span></span>
*  <span data-ttu-id="4d3a5-364">对于每*个大于一*的参数*argi* ， *argi*之前的*v*的明确赋值状态与上一个*arg*后*v*的状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-364">For each argument *argi* for any *i* greater than one, the definite assignment state of *v* before *argi* is the same as the state of *v* after the previous *arg*.</span></span>
*  <span data-ttu-id="4d3a5-365">如果在任何参数中将变量 v `out`作为参数（即， `out v`形式的参数）传递，则*expr*后的*v*状态将明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-365">If the variable *v* is passed as an `out` argument (i.e., an argument of the form `out v`) in any of the arguments, then the state of *v* after *expr* is definitely assigned.</span></span> <span data-ttu-id="4d3a5-366">本来*expr*后*v*的状态与*argN*后*v*的状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-366">Otherwise; the state of *v* after *expr* is the same as the state of *v* after *argN*.</span></span>
*  <span data-ttu-id="4d3a5-367">For 数组初始值设定项（[数组创建表达式](expressions.md#array-creation-expressions)）、对象初始值设定项（[对象初始值设定项](expressions.md#object-initializers)）、集合初始值设定项（[集合初始值设定项](expressions.md#collection-initializers)）和匿名对象初始值设定项（[匿名对象创建表达式](expressions.md#anonymous-object-creation-expressions)），明确的赋值状态由定义这些构造的扩展确定。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-367">For array initializers ([Array creation expressions](expressions.md#array-creation-expressions)), object initializers ([Object initializers](expressions.md#object-initializers)), collection initializers ([Collection initializers](expressions.md#collection-initializers)) and anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)), the definite assignment state is determined by the expansion that these constructs are defined in terms of.</span></span>

#### <a name="simple-assignment-expressions"></a><span data-ttu-id="4d3a5-368">简单赋值表达式</span><span class="sxs-lookup"><span data-stu-id="4d3a5-368">Simple assignment expressions</span></span>

<span data-ttu-id="4d3a5-369">对于以下`w = expr_rhs`形式*的表达式表达式*：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-369">For an expression *expr* of the form `w = expr_rhs`:</span></span>

*  <span data-ttu-id="4d3a5-370">*Expr_rhs*之前的*v*的明确赋值状态与*expr*之前的*v*的明确赋值状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-370">The definite assignment state of *v* before *expr_rhs* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="4d3a5-371">*Expr*后*v*的明确赋值状态由确定：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-371">The definite assignment state of *v* after *expr* is determined by:</span></span>
   * <span data-ttu-id="4d3a5-372">如果*w*是与*v*相同的变量，则*expr*后*v*的明确赋值状态是明确赋值的。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-372">If *w* is the same variable as *v*, then the definite assignment state of *v* after *expr* is definitely assigned.</span></span>
   * <span data-ttu-id="4d3a5-373">否则，如果在结构类型的实例构造函数中发生分配，则如果*w*是属性访问，它指定了正在构造的实例上自动实现的属性*P* ，而*v*是的隐藏支持字段*P*，则*expr*后*v*的明确赋值状态为明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-373">Otherwise, if the assignment occurs within the instance constructor of a struct type, if *w* is a property access designating an automatically implemented property *P* on the instance being constructed and *v* is the hidden backing field of *P*, then the definite assignment state of *v* after *expr* is definitely assigned.</span></span>
   * <span data-ttu-id="4d3a5-374">否则，在*expr*后， *v*的明确赋值状态与*expr_rhs*后*v*的明确赋值状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-374">Otherwise, the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_rhs*.</span></span>

#### <a name="-conditional-and-expressions"></a><span data-ttu-id="4d3a5-375">& & （条件和）表达式</span><span class="sxs-lookup"><span data-stu-id="4d3a5-375">&& (conditional AND) expressions</span></span>

<span data-ttu-id="4d3a5-376">对于以下`expr_first && expr_second`形式*的表达式表达式*：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-376">For an expression *expr* of the form `expr_first && expr_second`:</span></span>

*  <span data-ttu-id="4d3a5-377">*Expr_first*之前的*v*的明确赋值状态与*expr*之前的*v*的明确赋值状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-377">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="4d3a5-378">如果*expr_first*后*v*的状态为明确赋值或 "true expression 之后明确赋值"，则*expr_second*之前*v*的明确赋值状态为明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-378">The definite assignment state of *v* before *expr_second* is definitely assigned if the state of *v* after *expr_first* is either definitely assigned or "definitely assigned after true expression".</span></span> <span data-ttu-id="4d3a5-379">否则，不会明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-379">Otherwise, it is not definitely assigned.</span></span>
*  <span data-ttu-id="4d3a5-380">*Expr*后*v*的明确赋值状态由确定：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-380">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="4d3a5-381">如果*expr_first*是一个具有值`false`的常量表达式，则在*expr*后*v*的明确赋值状态与*expr_first*后*v*的明确赋值状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-381">If *expr_first* is a constant expression with the value `false`, then the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
    * <span data-ttu-id="4d3a5-382">否则，如果*expr_first*后*v*的状态为明确赋值，则*expr*后*v*的状态将明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-382">Otherwise, if the state of *v* after *expr_first* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="4d3a5-383">否则，如果*expr_second*后*v*的状态为 "已明确分配"，而 " *expr_first* *" 的状态*为 "在 false 表达式后明确赋值"，则*expr*后*v*的状态肯定是已.</span><span class="sxs-lookup"><span data-stu-id="4d3a5-383">Otherwise, if the state of *v* after *expr_second* is definitely assigned, and the state of *v* after *expr_first* is "definitely assigned after false expression", then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="4d3a5-384">否则，如果*expr_second*后*v*的状态为 "明确赋值" 或 "true expression 之后明确赋值"，则*expr*后*v*的状态为 "在 true 表达式后明确赋值"。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-384">Otherwise, if the state of *v* after *expr_second* is definitely assigned or "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="4d3a5-385">否则，如果*expr_first*后*v*的状态为 "在假表达式后明确赋值"，并且*expr_second*之后的*v*状态是 *"在 false*表达式后明确赋值"，则*expr*是 "在假表达式后明确赋值"。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-385">Otherwise, if the state of *v* after *expr_first* is "definitely assigned after false expression", and the state of *v* after *expr_second* is "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>
    * <span data-ttu-id="4d3a5-386">否则，不明确赋值*expr*后的*v*状态。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-386">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

<span data-ttu-id="4d3a5-387">示例中</span><span class="sxs-lookup"><span data-stu-id="4d3a5-387">In the example</span></span>
```csharp
class A
{
    static void F(int x, int y) {
        int i;
        if (x >= 0 && (i = y) >= 0) {
            // i definitely assigned
        }
        else {
            // i not definitely assigned
        }
        // i not definitely assigned
    }
}
```
<span data-ttu-id="4d3a5-388">在`if`语句`i`的一个嵌入语句中（而不是在另一个语句中），该变量被视为已明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-388">the variable `i` is considered definitely assigned in one of the embedded statements of an `if` statement but not in the other.</span></span> <span data-ttu-id="4d3a5-389">在方法`if` `F`的语句中，变量`i`在第一个嵌入语句中是明确赋值的，因为在执行`(i = y)`此嵌入语句之前，表达式的执行始终为。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-389">In the `if` statement in method `F`, the variable `i` is definitely assigned in the first embedded statement because execution of the expression `(i = y)` always precedes execution of this embedded statement.</span></span> <span data-ttu-id="4d3a5-390">与此相反，第`i`二个嵌入语句中的变量并不是明确`x >= 0`赋值的，因为可能已对 false 进行`i`了测试，导致变量未赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-390">In contrast, the variable `i` is not definitely assigned in the second embedded statement, since `x >= 0` might have tested false, resulting in the variable `i` being unassigned.</span></span>

#### <a name="-conditional-or-expressions"></a><span data-ttu-id="4d3a5-391">||（条件或）表达式</span><span class="sxs-lookup"><span data-stu-id="4d3a5-391">|| (conditional OR) expressions</span></span>

<span data-ttu-id="4d3a5-392">对于以下`expr_first || expr_second`形式*的表达式表达式*：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-392">For an expression *expr* of the form `expr_first || expr_second`:</span></span>

*  <span data-ttu-id="4d3a5-393">*Expr_first*之前的*v*的明确赋值状态与*expr*之前的*v*的明确赋值状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-393">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="4d3a5-394">如果*expr_first*后*v*的状态为明确赋值或 "在 false 表达式后明确赋值"，则*expr_second*之前*v*的明确赋值状态为明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-394">The definite assignment state of *v* before *expr_second* is definitely assigned if the state of *v* after *expr_first* is either definitely assigned or "definitely assigned after false expression".</span></span> <span data-ttu-id="4d3a5-395">否则，不会明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-395">Otherwise, it is not definitely assigned.</span></span>
*  <span data-ttu-id="4d3a5-396">*Expr*后的*v*的明确赋值语句由确定：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-396">The definite assignment statement of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="4d3a5-397">如果*expr_first*是一个具有值`true`的常量表达式，则在*expr*后*v*的明确赋值状态与*expr_first*后*v*的明确赋值状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-397">If *expr_first* is a constant expression with the value `true`, then the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
    * <span data-ttu-id="4d3a5-398">否则，如果*expr_first*后*v*的状态为明确赋值，则*expr*后*v*的状态将明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-398">Otherwise, if the state of *v* after *expr_first* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="4d3a5-399">否则，如果*expr_second*后*v*的状态是明确赋值的，并且*expr_first*之后的*v*状态是 "在真正表达式后明确赋值"，则*expr*后*v*的状态肯定是已.</span><span class="sxs-lookup"><span data-stu-id="4d3a5-399">Otherwise, if the state of *v* after *expr_second* is definitely assigned, and the state of *v* after *expr_first* is "definitely assigned after true expression", then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="4d3a5-400">否则，如果*expr_second*后*v*的状态是明确赋值的或 "在假表达式后明确赋值"，则*expr*后*v*的状态为 "在 false 表达式后明确赋值"。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-400">Otherwise, if the state of *v* after *expr_second* is definitely assigned or "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>
    * <span data-ttu-id="4d3a5-401">否则，如果*expr_first*后*v*的状态为 "在 true 表达式后明确赋值"，并且*expr_second*之后的*v*状态是 "true expression 之后明确赋值"，则在 expr 后的*v*状态为 "true 表达式后明确赋值"。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-401">Otherwise, if the state of *v* after *expr_first* is "definitely assigned after true expression", and the state of *v* after *expr_second* is "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="4d3a5-402">否则，不明确赋值*expr*后的*v*状态。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-402">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

<span data-ttu-id="4d3a5-403">示例中</span><span class="sxs-lookup"><span data-stu-id="4d3a5-403">In the example</span></span>
```csharp
class A
{
    static void G(int x, int y) {
        int i;
        if (x >= 0 || (i = y) >= 0) {
            // i not definitely assigned
        }
        else {
            // i definitely assigned
        }
        // i not definitely assigned
    }
}
```
<span data-ttu-id="4d3a5-404">在`if`语句`i`的一个嵌入语句中（而不是在另一个语句中），该变量被视为已明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-404">the variable `i` is considered definitely assigned in one of the embedded statements of an `if` statement but not in the other.</span></span> <span data-ttu-id="4d3a5-405">在方法`if` `G`的语句中，变量`i`在第二个嵌入语句中是明确赋值的，因为在`(i = y)`执行此嵌入语句之前，表达式的执行始终为。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-405">In the `if` statement in method `G`, the variable `i` is definitely assigned in the second embedded statement because execution of the expression `(i = y)` always precedes execution of this embedded statement.</span></span> <span data-ttu-id="4d3a5-406">与此相反，第`i`一个嵌入语句中的变量不是明确赋值的`x >= 0` ，因为可能测试了 true，导致变量`i`被分配。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-406">In contrast, the variable `i` is not definitely assigned in the first embedded statement, since `x >= 0` might have tested true, resulting in the variable `i` being unassigned.</span></span>

#### <a name="-logical-negation-expressions"></a><span data-ttu-id="4d3a5-407">!</span><span class="sxs-lookup"><span data-stu-id="4d3a5-407">!</span></span> <span data-ttu-id="4d3a5-408">（逻辑非）表达式</span><span class="sxs-lookup"><span data-stu-id="4d3a5-408">(logical negation) expressions</span></span>

<span data-ttu-id="4d3a5-409">对于以下`! expr_operand`形式*的表达式表达式*：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-409">For an expression *expr* of the form `! expr_operand`:</span></span>

*  <span data-ttu-id="4d3a5-410">*Expr_operand*之前的*v*的明确赋值状态与*expr*之前的*v*的明确赋值状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-410">The definite assignment state of *v* before *expr_operand* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="4d3a5-411">*Expr*后*v*的明确赋值状态由确定：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-411">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="4d3a5-412">如果在 \* expr_operand \* 之后的*v*状态为明确赋值，则*expr*后*v*的状态将明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-412">If the state of *v* after \*expr_operand \*is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="4d3a5-413">如果未明确赋值 \* expr_operand \* 之后的*v*状态，则不明确赋值*expr*后的*v*状态。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-413">If the state of *v* after \*expr_operand \*is not definitely assigned, then the state of *v* after *expr* is not definitely assigned.</span></span>
    * <span data-ttu-id="4d3a5-414">如果在 \* expr_operand \* 之后的*v*状态是 "在假表达式后明确赋值"，则*expr*后面的*v*状态是 "在真正表达式后明确赋值"。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-414">If the state of *v* after \*expr_operand \*is "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="4d3a5-415">如果在 \* expr_operand \* 之后的*v*状态是 "在 true 表达式后明确赋值"，则*expr*后*v*的状态为 "在假表达式后明确赋值"。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-415">If the state of *v* after \*expr_operand \*is "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>

#### <a name="-null-coalescing-expressions"></a><span data-ttu-id="4d3a5-416">??</span><span class="sxs-lookup"><span data-stu-id="4d3a5-416">??</span></span> <span data-ttu-id="4d3a5-417">（null 合并）表达式</span><span class="sxs-lookup"><span data-stu-id="4d3a5-417">(null coalescing) expressions</span></span>

<span data-ttu-id="4d3a5-418">对于以下`expr_first ?? expr_second`形式*的表达式表达式*：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-418">For an expression *expr* of the form `expr_first ?? expr_second`:</span></span>

*  <span data-ttu-id="4d3a5-419">*Expr_first*之前的*v*的明确赋值状态与*expr*之前的*v*的明确赋值状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-419">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="4d3a5-420">*Expr_second*之前的*v*的明确赋值状态与*expr_first*后*v*的明确赋值状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-420">The definite assignment state of *v* before *expr_second* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
*  <span data-ttu-id="4d3a5-421">*Expr*后的*v*的明确赋值语句由确定：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-421">The definite assignment statement of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="4d3a5-422">如果*expr_first*是一个值为 null 的常量表达式（[常数表达式](expressions.md#constant-expressions)），则在*expr*后， *v*的状态与*expr_second*后*v*的状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-422">If *expr_first* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value null, then the state of *v* after *expr* is the same as the state of *v* after *expr_second*.</span></span>
*  <span data-ttu-id="4d3a5-423">否则，在*expr*后， *v*的状态与*expr_first*后*v*的明确赋值状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-423">Otherwise, the state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>

#### <a name="-conditional-expressions"></a><span data-ttu-id="4d3a5-424">？：（条件）表达式</span><span class="sxs-lookup"><span data-stu-id="4d3a5-424">?: (conditional) expressions</span></span>

<span data-ttu-id="4d3a5-425">对于以下`expr_cond ? expr_true : expr_false`形式*的表达式表达式*：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-425">For an expression *expr* of the form `expr_cond ? expr_true : expr_false`:</span></span>

*  <span data-ttu-id="4d3a5-426">*Expr_cond*之前的*v*的明确赋值状态与*expr*前面的*v*的状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-426">The definite assignment state of *v* before *expr_cond* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="4d3a5-427">当且仅当以下其中一项保留时， *v 前 v*的明确赋值*状态才是*明确赋值的：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-427">The definite assignment state of *v* before *expr_true* is definitely assigned if and only if one of the following holds:</span></span>
    * <span data-ttu-id="4d3a5-428">*expr_cond*是具有值的常量表达式`false`</span><span class="sxs-lookup"><span data-stu-id="4d3a5-428">*expr_cond* is a constant expression with the value `false`</span></span>
    * <span data-ttu-id="4d3a5-429">*expr_cond*后*v*的状态是明确赋值的，或者是 "true 表达式后明确赋值"。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-429">the state of *v* after *expr_cond* is definitely assigned or "definitely assigned after true expression".</span></span>
*  <span data-ttu-id="4d3a5-430">当且仅当以下其中一项保留时， *v 前 v*的明确赋值*状态才是*明确赋值的：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-430">The definite assignment state of *v* before *expr_false* is definitely assigned if and only if one of the following holds:</span></span>
    * <span data-ttu-id="4d3a5-431">*expr_cond*是具有值的常量表达式`true`</span><span class="sxs-lookup"><span data-stu-id="4d3a5-431">*expr_cond* is a constant expression with the value `true`</span></span>
*  <span data-ttu-id="4d3a5-432">*expr_cond*之后的*v*状态是明确赋值的，或者是 "在假表达式后明确赋值"。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-432">the state of *v* after *expr_cond* is definitely assigned or "definitely assigned after false expression".</span></span>
*  <span data-ttu-id="4d3a5-433">*Expr*后*v*的明确赋值状态由确定：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-433">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="4d3a5-434">如果*expr_cond*是`true`具有值的常量表达式（[常数表达式](expressions.md#constant-expressions)），则*expr*后*v*的状态与*expr_true*后*v*的状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-434">If *expr_cond* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value `true` then the state of *v* after *expr* is the same as the state of *v* after *expr_true*.</span></span>
    * <span data-ttu-id="4d3a5-435">否则，如果*expr_cond*是`false`具有值的常量表达式（[常数表达式](expressions.md#constant-expressions)），则*expr*后*v*的状态与*expr_false*后*v*的状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-435">Otherwise, if *expr_cond* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value `false` then the state of *v* after *expr* is the same as the state of *v* after *expr_false*.</span></span>
    * <span data-ttu-id="4d3a5-436">否则，如果*expr_true*后*v*的状态为 "已明确分配" 并且 expr_false*的*状态为 "已明确分配"，则*expr*后*v*的状态将明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-436">Otherwise, if the state of *v* after *expr_true* is definitely assigned and the state of *v* after *expr_false* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="4d3a5-437">否则，不明确赋值*expr*后的*v*状态。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-437">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

#### <a name="anonymous-functions"></a><span data-ttu-id="4d3a5-438">匿名函数</span><span class="sxs-lookup"><span data-stu-id="4d3a5-438">Anonymous functions</span></span>

<span data-ttu-id="4d3a5-439">对于带有主体（*块*或*表达式*）*正文*的*lambda_expression*或*anonymous_method_expression* *expr* ：</span><span class="sxs-lookup"><span data-stu-id="4d3a5-439">For a *lambda_expression* or *anonymous_method_expression* *expr* with a body (either *block* or *expression*) *body*:</span></span>

*  <span data-ttu-id="4d3a5-440">在*body*之前，外部变量*v*的明确赋值状态与*expr*前面的*v*的状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-440">The definite assignment state of an outer variable *v* before *body* is the same as the state of *v* before *expr*.</span></span> <span data-ttu-id="4d3a5-441">也就是说，外部变量的明确赋值状态是从匿名函数的上下文继承而来的。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-441">That is, definite assignment state of outer variables is inherited from the context of the anonymous function.</span></span>
*  <span data-ttu-id="4d3a5-442">*Expr*后面的外部变量*v*的明确赋值状态与*expr*前面的*v*的状态相同。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-442">The definite assignment state of an outer variable *v* after *expr* is the same as the state of *v* before *expr*.</span></span>

<span data-ttu-id="4d3a5-443">示例</span><span class="sxs-lookup"><span data-stu-id="4d3a5-443">The example</span></span>
```csharp
delegate bool Filter(int i);

void F() {
    int max;

    // Error, max is not definitely assigned
    Filter f = (int n) => n < max;

    max = 5;
    DoWork(f);
}
```
<span data-ttu-id="4d3a5-444">生成编译时错误，因为`max`在声明匿名函数的位置未明确赋值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-444">generates a compile-time error since `max` is not definitely assigned where the anonymous function is declared.</span></span> <span data-ttu-id="4d3a5-445">示例</span><span class="sxs-lookup"><span data-stu-id="4d3a5-445">The example</span></span>
```csharp
delegate void D();

void F() {
    int n;
    D d = () => { n = 1; };

    d();

    // Error, n is not definitely assigned
    Console.WriteLine(n);
}
```
<span data-ttu-id="4d3a5-446">还会生成编译时错误，因为对匿名函数`n`的赋值不会影响匿名函数`n`之外的明确赋值状态。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-446">also generates a compile-time error since the assignment to `n` in the anonymous function has no affect on the definite assignment state of `n` outside the anonymous function.</span></span>

## <a name="variable-references"></a><span data-ttu-id="4d3a5-447">变量引用</span><span class="sxs-lookup"><span data-stu-id="4d3a5-447">Variable references</span></span>

<span data-ttu-id="4d3a5-448">*Variable_reference*是分类为变量的*表达式*。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-448">A *variable_reference* is an *expression* that is classified as a variable.</span></span> <span data-ttu-id="4d3a5-449">*Variable_reference*表示可访问的存储位置以提取当前值并存储新值。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-449">A *variable_reference* denotes a storage location that can be accessed both to fetch the current value and to store a new value.</span></span>

```antlr
variable_reference
    : expression
    ;
```

<span data-ttu-id="4d3a5-450">在 C 和C++中， *variable_reference*称为*lvalue*。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-450">In C and C++, a *variable_reference* is known as an *lvalue*.</span></span>

## <a name="atomicity-of-variable-references"></a><span data-ttu-id="4d3a5-451">变量引用的原子性</span><span class="sxs-lookup"><span data-stu-id="4d3a5-451">Atomicity of variable references</span></span>

<span data-ttu-id="4d3a5-452">以下数据类型的读取和写入是原子的： `bool`、 `char` `short` `ushort` `byte` `sbyte`、、 `uint` 、、`int`、、、和引用类型。 `float`</span><span class="sxs-lookup"><span data-stu-id="4d3a5-452">Reads and writes of the following data types are atomic: `bool`, `char`, `byte`, `sbyte`, `short`, `ushort`, `uint`, `int`, `float`, and reference types.</span></span> <span data-ttu-id="4d3a5-453">此外，在前面的列表中，具有基础类型的枚举类型的读取和写入也是原子的。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-453">In addition, reads and writes of enum types with an underlying type in the previous list are also atomic.</span></span> <span data-ttu-id="4d3a5-454">其他类型（ `long`包括、 `ulong`、 `double`、以及`decimal`用户定义类型）的读取和写入不能保证为原子性。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-454">Reads and writes of other types, including `long`, `ulong`, `double`, and `decimal`, as well as user-defined types, are not guaranteed to be atomic.</span></span> <span data-ttu-id="4d3a5-455">除了为此目的而设计的库函数外，不保证原子读修改写入，如递增或递减。</span><span class="sxs-lookup"><span data-stu-id="4d3a5-455">Aside from the library functions designed for that purpose, there is no guarantee of atomic read-modify-write, such as in the case of increment or decrement.</span></span>

