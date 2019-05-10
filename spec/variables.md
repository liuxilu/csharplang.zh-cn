---
ms.openlocfilehash: b7bb7dd575d9e2e6d5dd85bdd3e535411e29fcf4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488867"
---
# <a name="variables"></a><span data-ttu-id="19395-101">变量</span><span class="sxs-lookup"><span data-stu-id="19395-101">Variables</span></span>

<span data-ttu-id="19395-102">变量表示存储位置。</span><span class="sxs-lookup"><span data-stu-id="19395-102">Variables represent storage locations.</span></span> <span data-ttu-id="19395-103">每个变量具有类型，用于确定值可以是存储在变量中。</span><span class="sxs-lookup"><span data-stu-id="19395-103">Every variable has a type that determines what values can be stored in the variable.</span></span> <span data-ttu-id="19395-104">C# 是类型安全语言和 C# 编译器保证在将存储在变量中的值始终是适当的类型。</span><span class="sxs-lookup"><span data-stu-id="19395-104">C# is a type-safe language, and the C# compiler guarantees that values stored in variables are always of the appropriate type.</span></span> <span data-ttu-id="19395-105">可以更改变量的值，通过分配或通过使用`++`和`--`运算符。</span><span class="sxs-lookup"><span data-stu-id="19395-105">The value of a variable can be changed through assignment or through use of the `++` and `--` operators.</span></span>

<span data-ttu-id="19395-106">变量必须是***明确赋值***([明确赋值](variables.md#definite-assignment)) 可以获取其值之前。</span><span class="sxs-lookup"><span data-stu-id="19395-106">A variable must be ***definitely assigned*** ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained.</span></span>

<span data-ttu-id="19395-107">如以下各节中所述，变量是***最初分配***或***最初未分配***。</span><span class="sxs-lookup"><span data-stu-id="19395-107">As described in the following sections, variables are either ***initially assigned*** or ***initially unassigned***.</span></span> <span data-ttu-id="19395-108">最初分配的变量具有定义完善的初始值，并始终被视为已明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-108">An initially assigned variable has a well-defined initial value and is always considered definitely assigned.</span></span> <span data-ttu-id="19395-109">最初未赋值的变量具有无初始值。</span><span class="sxs-lookup"><span data-stu-id="19395-109">An initially unassigned variable has no initial value.</span></span> <span data-ttu-id="19395-110">若要在某个位置被视为明确赋值最初未赋值的变量，向变量赋值必须指向该位置的每个可能的执行路径中。</span><span class="sxs-lookup"><span data-stu-id="19395-110">For an initially unassigned variable to be considered definitely assigned at a certain location, an assignment to the variable must occur in every possible execution path leading to that location.</span></span>

## <a name="variable-categories"></a><span data-ttu-id="19395-111">变量类别</span><span class="sxs-lookup"><span data-stu-id="19395-111">Variable categories</span></span>

<span data-ttu-id="19395-112">C# 定义的变量的七种类别： 静态变量、 实例变量、 数组元素、 值参数、 引用参数、 输出参数和局部变量。</span><span class="sxs-lookup"><span data-stu-id="19395-112">C# defines seven categories of variables: static variables, instance variables, array elements, value parameters, reference parameters, output parameters, and local variables.</span></span> <span data-ttu-id="19395-113">以下各节介绍了每个类别。</span><span class="sxs-lookup"><span data-stu-id="19395-113">The sections that follow describe each of these categories.</span></span>

<span data-ttu-id="19395-114">在示例</span><span class="sxs-lookup"><span data-stu-id="19395-114">In the example</span></span>
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
<span data-ttu-id="19395-115">`x` 是一个静态变量，`y`是一个实例变量，`v[0]`是一个数组元素，`a`是值参数，而`b`是一个引用参数，`c`是一个 output 参数，和`i`是一个本地变量。</span><span class="sxs-lookup"><span data-stu-id="19395-115">`x` is a static variable, `y` is an instance variable, `v[0]` is an array element, `a` is a value parameter, `b` is a reference parameter, `c` is an output parameter, and `i` is a local variable.</span></span>

### <a name="static-variables"></a><span data-ttu-id="19395-116">静态变量</span><span class="sxs-lookup"><span data-stu-id="19395-116">Static variables</span></span>

<span data-ttu-id="19395-117">使用字段声明`static`调用修饰符***静态变量***。</span><span class="sxs-lookup"><span data-stu-id="19395-117">A field declared with the `static` modifier is called a ***static variable***.</span></span> <span data-ttu-id="19395-118">静态变量之前就存在了静态构造函数的执行 ([静态构造函数](classes.md#static-constructors)) 为其包含类型和不再存在时关联的应用程序域将不再存在。</span><span class="sxs-lookup"><span data-stu-id="19395-118">A static variable comes into existence before execution of the static constructor ([Static constructors](classes.md#static-constructors)) for its containing type, and ceases to exist when the associated application domain ceases to exist.</span></span>

<span data-ttu-id="19395-119">静态变量的初始值是默认值 ([默认值](variables.md#default-values)) 的变量的类型。</span><span class="sxs-lookup"><span data-stu-id="19395-119">The initial value of a static variable is the default value ([Default values](variables.md#default-values)) of the variable's type.</span></span>

<span data-ttu-id="19395-120">为了明确赋值检查，静态变量被视为最初分配。</span><span class="sxs-lookup"><span data-stu-id="19395-120">For purposes of definite assignment checking, a static variable is considered initially assigned.</span></span>

### <a name="instance-variables"></a><span data-ttu-id="19395-121">实例变量</span><span class="sxs-lookup"><span data-stu-id="19395-121">Instance variables</span></span>

<span data-ttu-id="19395-122">字段声明而无需`static`调用修饰符***实例变量***。</span><span class="sxs-lookup"><span data-stu-id="19395-122">A field declared without the `static` modifier is called an ***instance variable***.</span></span>

#### <a name="instance-variables-in-classes"></a><span data-ttu-id="19395-123">中的类的实例变量</span><span class="sxs-lookup"><span data-stu-id="19395-123">Instance variables in classes</span></span>

<span data-ttu-id="19395-124">类的一个实例变量时，存在到该类的新实例创建，并且将不再存在时对该实例的引用并且已执行实例的析构函数 （如果有）。</span><span class="sxs-lookup"><span data-stu-id="19395-124">An instance variable of a class comes into existence when a new instance of that class is created, and ceases to exist when there are no references to that instance and the instance's destructor (if any) has executed.</span></span>

<span data-ttu-id="19395-125">类的一个实例变量的初始值是默认值 ([默认值](variables.md#default-values)) 的变量的类型。</span><span class="sxs-lookup"><span data-stu-id="19395-125">The initial value of an instance variable of a class is the default value ([Default values](variables.md#default-values)) of the variable's type.</span></span>

<span data-ttu-id="19395-126">为了明确赋值检查，类的一个实例变量被视为最初分配。</span><span class="sxs-lookup"><span data-stu-id="19395-126">For the purpose of definite assignment checking, an instance variable of a class is considered initially assigned.</span></span>

#### <a name="instance-variables-in-structs"></a><span data-ttu-id="19395-127">结构中的实例变量</span><span class="sxs-lookup"><span data-stu-id="19395-127">Instance variables in structs</span></span>

<span data-ttu-id="19395-128">结构的一个实例变量具有其所属的结构变量的生存期完全相同。</span><span class="sxs-lookup"><span data-stu-id="19395-128">An instance variable of a struct has exactly the same lifetime as the struct variable to which it belongs.</span></span> <span data-ttu-id="19395-129">换而言之，当结构类型的变量进入是否存在或不再存在，因此也执行该结构的实例变量。</span><span class="sxs-lookup"><span data-stu-id="19395-129">In other words, when a variable of a struct type comes into existence or ceases to exist, so too do the instance variables of the struct.</span></span>

<span data-ttu-id="19395-130">包含结构变量相同的结构实例变量的初始分配状态。</span><span class="sxs-lookup"><span data-stu-id="19395-130">The initial assignment state of an instance variable of a struct is the same as that of the containing struct variable.</span></span> <span data-ttu-id="19395-131">换而言之，何时结构变量被视为最初分配的因此也是它的实例变量和结构变量被视为最初未赋值，它的实例变量已赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-131">In other words, when a struct variable is considered initially assigned, so too are its instance variables, and when a struct variable is considered initially unassigned, its instance variables are likewise unassigned.</span></span>

### <a name="array-elements"></a><span data-ttu-id="19395-132">数组元素</span><span class="sxs-lookup"><span data-stu-id="19395-132">Array elements</span></span>

<span data-ttu-id="19395-133">数组的元素创建数组实例后，就应该考虑存在并在没有对该数组实例的引用时停止存在。</span><span class="sxs-lookup"><span data-stu-id="19395-133">The elements of an array come into existence when an array instance is created, and cease to exist when there are no references to that array instance.</span></span>

<span data-ttu-id="19395-134">每个数组的元素的初始值是默认值 ([默认值](variables.md#default-values)) 的数组元素的类型。</span><span class="sxs-lookup"><span data-stu-id="19395-134">The initial value of each of the elements of an array is the default value ([Default values](variables.md#default-values)) of the type of the array elements.</span></span>

<span data-ttu-id="19395-135">为了明确赋值检查，数组元素被视为最初分配。</span><span class="sxs-lookup"><span data-stu-id="19395-135">For the purpose of definite assignment checking, an array element is considered initially assigned.</span></span>

### <a name="value-parameters"></a><span data-ttu-id="19395-136">值参数</span><span class="sxs-lookup"><span data-stu-id="19395-136">Value parameters</span></span>

<span data-ttu-id="19395-137">无需声明的参数`ref`或`out`修饰符***value 参数***。</span><span class="sxs-lookup"><span data-stu-id="19395-137">A parameter declared without a `ref` or `out` modifier is a ***value parameter***.</span></span>

<span data-ttu-id="19395-138">Value 参数开始的函数成员 （方法、 实例构造函数、 访问器或运算符） 或匿名函数的调用时存在于该参数属于和初始化的调用中给定的参数值。</span><span class="sxs-lookup"><span data-stu-id="19395-138">A value parameter comes into existence upon invocation of the function member (method, instance constructor, accessor, or operator) or anonymous function to which the parameter belongs, and is initialized with the value of the argument given in the invocation.</span></span> <span data-ttu-id="19395-139">值参数通常将不再存在时返回的匿名函数的函数成员。</span><span class="sxs-lookup"><span data-stu-id="19395-139">A value parameter normally ceases to exist upon return of the function member or anonymous function.</span></span> <span data-ttu-id="19395-140">但是，如果值参数所捕获的匿名函数 ([匿名函数表达式](expressions.md#anonymous-function-expressions))、 其生存时间将至少延长到委托或从该匿名函数创建表达式树不符合条件垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="19395-140">However, if the value parameter is captured by an anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), its life time extends at least until the delegate or expression tree created from that anonymous function is eligible for garbage collection.</span></span>

<span data-ttu-id="19395-141">为了明确赋值检查，value 参数被视为最初分配。</span><span class="sxs-lookup"><span data-stu-id="19395-141">For the purpose of definite assignment checking, a value parameter is considered initially assigned.</span></span>

### <a name="reference-parameters"></a><span data-ttu-id="19395-142">引用参数</span><span class="sxs-lookup"><span data-stu-id="19395-142">Reference parameters</span></span>

<span data-ttu-id="19395-143">与声明的参数`ref`修饰符***引用参数***。</span><span class="sxs-lookup"><span data-stu-id="19395-143">A parameter declared with a `ref` modifier is a ***reference parameter***.</span></span>

<span data-ttu-id="19395-144">引用参数不会创建新的存储位置。</span><span class="sxs-lookup"><span data-stu-id="19395-144">A reference parameter does not create a new storage location.</span></span> <span data-ttu-id="19395-145">相反，引用参数表示相同的存储位置与在匿名函数调用的函数成员作为参数给出的变量。</span><span class="sxs-lookup"><span data-stu-id="19395-145">Instead, a reference parameter represents the same storage location as the variable given as the argument in the function member or anonymous function invocation.</span></span> <span data-ttu-id="19395-146">因此，引用参数的值始终是相同的基础变量。</span><span class="sxs-lookup"><span data-stu-id="19395-146">Thus, the value of a reference parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="19395-147">以下的明确赋值规则适用于引用参数。</span><span class="sxs-lookup"><span data-stu-id="19395-147">The following definite assignment rules apply to reference parameters.</span></span> <span data-ttu-id="19395-148">请注意输出参数中所述的不同规则[输出参数](variables.md#output-parameters)。</span><span class="sxs-lookup"><span data-stu-id="19395-148">Note the different rules for output parameters described in [Output parameters](variables.md#output-parameters).</span></span>

*  <span data-ttu-id="19395-149">变量必须明确赋值 ([明确赋值](variables.md#definite-assignment)) 它可以作为引用参数的函数成员或委托调用中传递之前。</span><span class="sxs-lookup"><span data-stu-id="19395-149">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter in a function member or delegate invocation.</span></span>
*  <span data-ttu-id="19395-150">在内部函数成员或匿名函数，引用参数被视为最初分配。</span><span class="sxs-lookup"><span data-stu-id="19395-150">Within a function member or anonymous function, a reference parameter is considered initially assigned.</span></span>

<span data-ttu-id="19395-151">实例方法或结构类型的实例访问器内`this`关键字的行为与完全相同的结构类型的引用参数 ([此访问权限](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="19395-151">Within an instance method or instance accessor of a struct type, the `this` keyword behaves exactly as a reference parameter of the struct type ([This access](expressions.md#this-access)).</span></span>

### <a name="output-parameters"></a><span data-ttu-id="19395-152">输出参数</span><span class="sxs-lookup"><span data-stu-id="19395-152">Output parameters</span></span>

<span data-ttu-id="19395-153">与声明的参数`out`修饰符***输出参数***。</span><span class="sxs-lookup"><span data-stu-id="19395-153">A parameter declared with an `out` modifier is an ***output parameter***.</span></span>

<span data-ttu-id="19395-154">输出参数不会创建新的存储位置。</span><span class="sxs-lookup"><span data-stu-id="19395-154">An output parameter does not create a new storage location.</span></span> <span data-ttu-id="19395-155">而是输出参数表示相同的存储位置与作为函数成员或委托调用中的参数给出的变量。</span><span class="sxs-lookup"><span data-stu-id="19395-155">Instead, an output parameter represents the same storage location as the variable given as the argument in the function member or delegate invocation.</span></span> <span data-ttu-id="19395-156">因此，输出参数的值始终是相同的基础变量。</span><span class="sxs-lookup"><span data-stu-id="19395-156">Thus, the value of an output parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="19395-157">以下的明确赋值规则适用于输出参数。</span><span class="sxs-lookup"><span data-stu-id="19395-157">The following definite assignment rules apply to output parameters.</span></span> <span data-ttu-id="19395-158">请注意引用参数中所述的不同规则[引用参数](variables.md#reference-parameters)。</span><span class="sxs-lookup"><span data-stu-id="19395-158">Note the different rules for reference parameters described in [Reference parameters](variables.md#reference-parameters).</span></span>

*  <span data-ttu-id="19395-159">变量在其可以作为函数成员中的输出参数传递或委托调用之前不需要明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-159">A variable need not be definitely assigned before it can be passed as an output parameter in a function member or delegate invocation.</span></span>
*  <span data-ttu-id="19395-160">在函数成员或委托调用正常完成，因为被认为是 output 参数传递的每个变量赋值的执行路径中。</span><span class="sxs-lookup"><span data-stu-id="19395-160">Following the normal completion of a function member or delegate invocation, each variable that was passed as an output parameter is considered assigned in that execution path.</span></span>
*  <span data-ttu-id="19395-161">在函数成员或匿名函数，输出参数被视为最初未赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-161">Within a function member or anonymous function, an output parameter is considered initially unassigned.</span></span>
*  <span data-ttu-id="19395-162">必须明确赋值函数成员或匿名函数的每个输出参数 ([明确赋值](variables.md#definite-assignment)) 之前该函数成员或匿名函数正常返回。</span><span class="sxs-lookup"><span data-stu-id="19395-162">Every output parameter of a function member or anonymous function must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before the function member or anonymous function returns normally.</span></span>

<span data-ttu-id="19395-163">结构类型的实例构造函数内`this`关键字的行为与完全相同的输出参数的结构类型 ([此访问权限](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="19395-163">Within an instance constructor of a struct type, the `this` keyword behaves exactly as an output parameter of the struct type ([This access](expressions.md#this-access)).</span></span>

### <a name="local-variables"></a><span data-ttu-id="19395-164">局部变量</span><span class="sxs-lookup"><span data-stu-id="19395-164">Local variables</span></span>

<span data-ttu-id="19395-165">一个***局部变量***来声明*local_variable_declaration*，这可能是在*块*、 *for_statement*， *switch_statement*或*using_statement*; 或通过*foreach_statement*或者*specific_catch_clause*为*try_statement*。</span><span class="sxs-lookup"><span data-stu-id="19395-165">A ***local variable*** is declared by a *local_variable_declaration*, which may occur in a *block*, a *for_statement*, a *switch_statement* or a *using_statement*; or by a *foreach_statement* or a *specific_catch_clause* for a *try_statement*.</span></span>

<span data-ttu-id="19395-166">本地变量的生存期是在此期间保证存储可为其保留的程序执行的部分。</span><span class="sxs-lookup"><span data-stu-id="19395-166">The lifetime of a local variable is the portion of program execution during which storage is guaranteed to be reserved for it.</span></span> <span data-ttu-id="19395-167">此生命周期中的条目从至少持续*块*， *for_statement*， *switch_statement*， *using_statement*， *foreach_statement*，或*specific_catch_clause*与它关联，执行，直到*块*， *for_statement*， *switch_statement*， *using_statement*， *foreach_statement*，或*specific_catch_clause*以任何方式结束。</span><span class="sxs-lookup"><span data-stu-id="19395-167">This lifetime extends at least from entry into the *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* with which it is associated, until execution of that *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* ends in any way.</span></span> <span data-ttu-id="19395-168">(输入括住*块*或调用方法将挂起，但不结束当前的执行*块*， *for_statement*， *switch_statement*， *using_statement*， *foreach_statement*，或*specific_catch_clause*。)如果匿名函数捕获本地变量 ([捕获外部变量](expressions.md#captured-outer-variables))，其生存期内将至少延长到从匿名函数，以及到出现的任何其他对象创建的委托或表达式树引用捕获的变量，可以进行垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="19395-168">(Entering an enclosed *block* or calling a method suspends, but does not end, execution of the current *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause*.) If the local variable is captured by an anonymous function ([Captured outer variables](expressions.md#captured-outer-variables)), its lifetime extends at least until the delegate or expression tree created from the anonymous function, along with any other objects that come to reference the captured variable, are eligible for garbage collection.</span></span>

<span data-ttu-id="19395-169">如果父级*块*， *for_statement*， *switch_statement*， *using_statement*， *foreach_statement*，或*specific_catch_clause*输入以递归方式，每次创建是有本地变量的新实例并将其*local_variable_initializer*，如果计算每次。</span><span class="sxs-lookup"><span data-stu-id="19395-169">If the parent *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* is entered recursively, a new instance of the local variable is created each time, and its *local_variable_initializer*, if any, is evaluated each time.</span></span>

<span data-ttu-id="19395-170">引入局部变量*local_variable_declaration*不会自动初始化，因此没有默认值。</span><span class="sxs-lookup"><span data-stu-id="19395-170">A local variable introduced by a *local_variable_declaration* is not automatically initialized and thus has no default value.</span></span> <span data-ttu-id="19395-171">为了明确赋值检查，本地变量引入*local_variable_declaration*被视为最初未赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-171">For the purpose of definite assignment checking, a local variable introduced by a *local_variable_declaration* is considered initially unassigned.</span></span> <span data-ttu-id="19395-172">一个*local_variable_declaration*可能包括*local_variable_initializer*，在这种情况下该变量明确赋值被视为仅在初始化表达式后 ([声明语句](variables.md#declaration-statements))。</span><span class="sxs-lookup"><span data-stu-id="19395-172">A *local_variable_declaration* may include a *local_variable_initializer*, in which case the variable is considered definitely assigned only after the initializing expression ([Declaration statements](variables.md#declaration-statements)).</span></span>

<span data-ttu-id="19395-173">引入本地变量的作用域内*local_variable_declaration*，它会导致编译时错误到该本地变量的文本的位置之前，请参阅其*local_variable_declarator*.</span><span class="sxs-lookup"><span data-stu-id="19395-173">Within the scope of a local variable introduced by a *local_variable_declaration*, it is a compile-time error to refer to that local variable in a textual position that precedes its *local_variable_declarator*.</span></span> <span data-ttu-id="19395-174">如果本地变量声明是隐式 ([局部变量声明](statements.md#local-variable-declarations))，它也是错误引用中的变量及其*local_variable_declarator*。</span><span class="sxs-lookup"><span data-stu-id="19395-174">If the local variable declaration is implicit ([Local variable declarations](statements.md#local-variable-declarations)), it is also an error to refer to the variable within its *local_variable_declarator*.</span></span>

<span data-ttu-id="19395-175">引入局部变量*foreach_statement*或*specific_catch_clause*在其整个范围内被视为明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-175">A local variable introduced by a *foreach_statement* or a *specific_catch_clause* is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="19395-176">本地变量的实际生存期是依赖于实现的。</span><span class="sxs-lookup"><span data-stu-id="19395-176">The actual lifetime of a local variable is implementation-dependent.</span></span> <span data-ttu-id="19395-177">例如，编译器可能会以静态方式确定块中的本地变量仅用于该块的一小部分。</span><span class="sxs-lookup"><span data-stu-id="19395-177">For example, a compiler might statically determine that a local variable in a block is only used for a small portion of that block.</span></span> <span data-ttu-id="19395-178">使用此分析，编译器无法生成结果变量的生存期短于包含的块的存储中的代码。</span><span class="sxs-lookup"><span data-stu-id="19395-178">Using this analysis, the compiler could generate code that results in the variable's storage having a shorter lifetime than its containing block.</span></span>

<span data-ttu-id="19395-179">由局部引用变量引用的存储与该局部引用变量的生存期无关的回收 ([自动内存管理](basic-concepts.md#automatic-memory-management))。</span><span class="sxs-lookup"><span data-stu-id="19395-179">The storage referred to by a local reference variable is reclaimed independently of the lifetime of that local reference variable ([Automatic memory management](basic-concepts.md#automatic-memory-management)).</span></span>

## <a name="default-values"></a><span data-ttu-id="19395-180">默认值</span><span class="sxs-lookup"><span data-stu-id="19395-180">Default values</span></span>

<span data-ttu-id="19395-181">以下类别的变量将自动初始化为其默认值：</span><span class="sxs-lookup"><span data-stu-id="19395-181">The following categories of variables are automatically initialized to their default values:</span></span>

*  <span data-ttu-id="19395-182">静态变量。</span><span class="sxs-lookup"><span data-stu-id="19395-182">Static variables.</span></span>
*  <span data-ttu-id="19395-183">类实例的实例变量。</span><span class="sxs-lookup"><span data-stu-id="19395-183">Instance variables of class instances.</span></span>
*  <span data-ttu-id="19395-184">数组元素。</span><span class="sxs-lookup"><span data-stu-id="19395-184">Array elements.</span></span>

<span data-ttu-id="19395-185">变量的默认值取决于变量的类型，并按如下所示确定：</span><span class="sxs-lookup"><span data-stu-id="19395-185">The default value of a variable depends on the type of the variable and is determined as follows:</span></span>

*  <span data-ttu-id="19395-186">变量的*value_type*，默认值是通过计算出来的值与相同*value_type*的默认构造函数 ([默认构造函数](types.md#default-constructors))。</span><span class="sxs-lookup"><span data-stu-id="19395-186">For a variable of a *value_type*, the default value is the same as the value computed by the *value_type*'s default constructor ([Default constructors](types.md#default-constructors)).</span></span>
*  <span data-ttu-id="19395-187">变量的*reference_type*，默认值是`null`。</span><span class="sxs-lookup"><span data-stu-id="19395-187">For a variable of a *reference_type*, the default value is `null`.</span></span>

<span data-ttu-id="19395-188">初始化为默认值通常是通过让内存管理器或垃圾回收器内存初始化为所有位归零之前已分配供使用。</span><span class="sxs-lookup"><span data-stu-id="19395-188">Initialization to default values is typically done by having the memory manager or garbage collector initialize memory to all-bits-zero before it is allocated for use.</span></span> <span data-ttu-id="19395-189">出于此原因，是可以方便地使用所有位归零来表示 null 引用。</span><span class="sxs-lookup"><span data-stu-id="19395-189">For this reason, it is convenient to use all-bits-zero to represent the null reference.</span></span>

## <a name="definite-assignment"></a><span data-ttu-id="19395-190">明确赋值</span><span class="sxs-lookup"><span data-stu-id="19395-190">Definite assignment</span></span>

<span data-ttu-id="19395-191">在给定位置中的可执行代码的函数成员，变量称为***明确赋值***可以证明编译器，由特定的静态流分析 ([精确规则，用于确定明确分配](variables.md#precise-rules-for-determining-definite-assignment))，该变量会自动初始化或已至少一个赋值的目标。</span><span class="sxs-lookup"><span data-stu-id="19395-191">At a given location in the executable code of a function member, a variable is said to be ***definitely assigned*** if the compiler can prove, by a particular static flow analysis ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)), that the variable has been automatically initialized or has been the target of at least one assignment.</span></span> <span data-ttu-id="19395-192">非正式地讲，是赋值的明确的规则：</span><span class="sxs-lookup"><span data-stu-id="19395-192">Informally stated, the rules of definite assignment are:</span></span>

*  <span data-ttu-id="19395-193">最初分配的变量 ([最初分配变量](variables.md#initially-assigned-variables)) 始终被视为明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-193">An initially assigned variable ([Initially assigned variables](variables.md#initially-assigned-variables)) is always considered definitely assigned.</span></span>
*  <span data-ttu-id="19395-194">最初未赋值的变量 ([最初未分配的变量](variables.md#initially-unassigned-variables)) 如果被视为明确赋值在给定位置指向该位置的所有可能的执行路径包含至少一个以下：</span><span class="sxs-lookup"><span data-stu-id="19395-194">An initially unassigned variable ([Initially unassigned variables](variables.md#initially-unassigned-variables)) is considered definitely assigned at a given location if all possible execution paths leading to that location contain at least one of the following:</span></span>
    * <span data-ttu-id="19395-195">简单赋值 ([简单的赋值](expressions.md#simple-assignment)) 中的变量是左的操作数。</span><span class="sxs-lookup"><span data-stu-id="19395-195">A simple assignment ([Simple assignment](expressions.md#simple-assignment)) in which the variable is the left operand.</span></span>
    * <span data-ttu-id="19395-196">调用表达式 ([调用表达式](expressions.md#invocation-expressions)) 或对象创建表达式 ([对象创建表达式](expressions.md#object-creation-expressions)) 将变量传递作为输出参数。</span><span class="sxs-lookup"><span data-stu-id="19395-196">An invocation expression ([Invocation expressions](expressions.md#invocation-expressions)) or object creation expression ([Object creation expressions](expressions.md#object-creation-expressions)) that passes the variable as an output parameter.</span></span>
    * <span data-ttu-id="19395-197">对于本地变量，本地变量声明 ([局部变量声明](statements.md#local-variable-declarations))，其中包含变量的初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="19395-197">For a local variable, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) that includes a variable initializer.</span></span>

<span data-ttu-id="19395-198">基础上述非正式规则的正式规范中所述[最初分配变量](variables.md#initially-assigned-variables)，[最初未分配的变量](variables.md#initially-unassigned-variables)，和[精确规则，用于确定明确赋值](variables.md#precise-rules-for-determining-definite-assignment)。</span><span class="sxs-lookup"><span data-stu-id="19395-198">The formal specification underlying the above informal rules is described in [Initially assigned variables](variables.md#initially-assigned-variables), [Initially unassigned variables](variables.md#initially-unassigned-variables), and [Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment).</span></span>

<span data-ttu-id="19395-199">实例变量明确赋值状态*struct_type*单独以及统一跟踪的变量。</span><span class="sxs-lookup"><span data-stu-id="19395-199">The definite assignment states of instance variables of a *struct_type* variable are tracked individually as well as collectively.</span></span> <span data-ttu-id="19395-200">在其他规则更高版本，以下规则应用于*struct_type*变量和其实例的变量：</span><span class="sxs-lookup"><span data-stu-id="19395-200">In additional to the rules above, the following rules apply to *struct_type* variables and their instance variables:</span></span>

*  <span data-ttu-id="19395-201">一个实例变量被视为明确赋值，如果其包含*struct_type*变量被视为已明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-201">An instance variable is considered definitely assigned if its containing *struct_type* variable is considered definitely assigned.</span></span>
*  <span data-ttu-id="19395-202">一个*struct_type*变量被视为明确赋值，如果它的实例变量的每个被视为明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-202">A *struct_type* variable is considered definitely assigned if each of its instance variables is considered definitely assigned.</span></span>

<span data-ttu-id="19395-203">明确赋值是一项要求以下上下文中：</span><span class="sxs-lookup"><span data-stu-id="19395-203">Definite assignment is a requirement in the following contexts:</span></span>

*  <span data-ttu-id="19395-204">变量必须在其值获取其中每个位置中明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-204">A variable must be definitely assigned at each location where its value is obtained.</span></span> <span data-ttu-id="19395-205">这可确保永远不会发生未定义的值。</span><span class="sxs-lookup"><span data-stu-id="19395-205">This ensures that undefined values never occur.</span></span> <span data-ttu-id="19395-206">在表达式中的变量的匹配项被视为要获取变量的值，除非当</span><span class="sxs-lookup"><span data-stu-id="19395-206">The occurrence of a variable in an expression is considered to obtain the value of the variable, except when</span></span>
    * <span data-ttu-id="19395-207">该变量是分配的一个简单的左的操作数</span><span class="sxs-lookup"><span data-stu-id="19395-207">the variable is the left operand of a simple assignment,</span></span>
    * <span data-ttu-id="19395-208">该变量传递作为输出参数，或</span><span class="sxs-lookup"><span data-stu-id="19395-208">the variable is passed as an output parameter, or</span></span>
    * <span data-ttu-id="19395-209">该变量是*struct_type*变量，并且发生作为成员访问的左操作数。</span><span class="sxs-lookup"><span data-stu-id="19395-209">the variable is a *struct_type* variable and occurs as the left operand of a member access.</span></span>
*  <span data-ttu-id="19395-210">变量必须在其作为引用参数传递的每个位置中明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-210">A variable must be definitely assigned at each location where it is passed as a reference parameter.</span></span> <span data-ttu-id="19395-211">这可确保所调用的函数成员可以考虑最初分配的引用参数。</span><span class="sxs-lookup"><span data-stu-id="19395-211">This ensures that the function member being invoked can consider the reference parameter initially assigned.</span></span>
*  <span data-ttu-id="19395-212">必须在其中的函数成员返回每个位置明确赋值的函数成员的所有输出参数 (通过`return`语句，或者通过执行的函数成员正文结束)。</span><span class="sxs-lookup"><span data-stu-id="19395-212">All output parameters of a function member must be definitely assigned at each location where the function member returns (through a `return` statement or through execution reaching the end of the function member body).</span></span> <span data-ttu-id="19395-213">这可确保，成员函数不返回未定义的值在输出参数中，从而使编译器可以考虑将作为输出参数的变量等效于向变量赋值的函数成员调用。</span><span class="sxs-lookup"><span data-stu-id="19395-213">This ensures that function members do not return undefined values in output parameters, thus enabling the compiler to consider a function member invocation that takes a variable as an output parameter equivalent to an assignment to the variable.</span></span>
*  <span data-ttu-id="19395-214">`this`的变量*struct_type*实例构造函数都必须在该实例构造函数将返回其中每个位置明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-214">The `this` variable of a *struct_type* instance constructor must be definitely assigned at each location where that instance constructor returns.</span></span>

### <a name="initially-assigned-variables"></a><span data-ttu-id="19395-215">最初已分配的变量</span><span class="sxs-lookup"><span data-stu-id="19395-215">Initially assigned variables</span></span>

<span data-ttu-id="19395-216">最初分配分类变量的以下类别：</span><span class="sxs-lookup"><span data-stu-id="19395-216">The following categories of variables are classified as initially assigned:</span></span>

*  <span data-ttu-id="19395-217">静态变量。</span><span class="sxs-lookup"><span data-stu-id="19395-217">Static variables.</span></span>
*  <span data-ttu-id="19395-218">类实例的实例变量。</span><span class="sxs-lookup"><span data-stu-id="19395-218">Instance variables of class instances.</span></span>
*  <span data-ttu-id="19395-219">最初分配的结构变量的实例变量。</span><span class="sxs-lookup"><span data-stu-id="19395-219">Instance variables of initially assigned struct variables.</span></span>
*  <span data-ttu-id="19395-220">数组元素。</span><span class="sxs-lookup"><span data-stu-id="19395-220">Array elements.</span></span>
*  <span data-ttu-id="19395-221">值的参数。</span><span class="sxs-lookup"><span data-stu-id="19395-221">Value parameters.</span></span>
*  <span data-ttu-id="19395-222">引用参数。</span><span class="sxs-lookup"><span data-stu-id="19395-222">Reference parameters.</span></span>
*  <span data-ttu-id="19395-223">中声明的变量`catch`子句或`foreach`语句。</span><span class="sxs-lookup"><span data-stu-id="19395-223">Variables declared in a `catch` clause or a `foreach` statement.</span></span>

### <a name="initially-unassigned-variables"></a><span data-ttu-id="19395-224">最初未赋值的变量</span><span class="sxs-lookup"><span data-stu-id="19395-224">Initially unassigned variables</span></span>

<span data-ttu-id="19395-225">以下类别的变量被归类为最初未分配：</span><span class="sxs-lookup"><span data-stu-id="19395-225">The following categories of variables are classified as initially unassigned:</span></span>

*  <span data-ttu-id="19395-226">变量最初未分配的结构变量的实例。</span><span class="sxs-lookup"><span data-stu-id="19395-226">Instance variables of initially unassigned struct variables.</span></span>
*  <span data-ttu-id="19395-227">输出参数，包括`this`结构实例构造函数的变量。</span><span class="sxs-lookup"><span data-stu-id="19395-227">Output parameters, including the `this` variable of struct instance constructors.</span></span>
*  <span data-ttu-id="19395-228">本地变量，除了那些中声明`catch`子句或`foreach`语句。</span><span class="sxs-lookup"><span data-stu-id="19395-228">Local variables, except those declared in a `catch` clause or a `foreach` statement.</span></span>

### <a name="precise-rules-for-determining-definite-assignment"></a><span data-ttu-id="19395-229">精确规则，用于确定明确的赋值</span><span class="sxs-lookup"><span data-stu-id="19395-229">Precise rules for determining definite assignment</span></span>

<span data-ttu-id="19395-230">若要确定每个使用的变量明确赋值，编译器必须使用等效于在本部分中所述的过程。</span><span class="sxs-lookup"><span data-stu-id="19395-230">In order to determine that each used variable is definitely assigned, the compiler must use a process that is equivalent to the one described in this section.</span></span>

<span data-ttu-id="19395-231">编译器在处理每个具有一个或多个最初未赋值的变量的函数成员的正文。</span><span class="sxs-lookup"><span data-stu-id="19395-231">The compiler processes the body of each function member that has one or more initially unassigned variables.</span></span> <span data-ttu-id="19395-232">对于每个最初未赋值变量*v*，编译器确定***明确赋值状态***有关*v*在每个函数成员中的以下几点：</span><span class="sxs-lookup"><span data-stu-id="19395-232">For each initially unassigned variable *v*, the compiler determines a ***definite assignment state*** for *v* at each of the following points in the function member:</span></span>

*  <span data-ttu-id="19395-233">每个语句的开头</span><span class="sxs-lookup"><span data-stu-id="19395-233">At the beginning of each statement</span></span>
*  <span data-ttu-id="19395-234">在终结点 ([终结点和可访问性](statements.md#end-points-and-reachability)) 的每个语句</span><span class="sxs-lookup"><span data-stu-id="19395-234">At the end point ([End points and reachability](statements.md#end-points-and-reachability)) of each statement</span></span>
*  <span data-ttu-id="19395-235">在每个弧线上将控制转移到另一个语句或语句的终结点</span><span class="sxs-lookup"><span data-stu-id="19395-235">On each arc which transfers control to another statement or to the end point of a statement</span></span>
*  <span data-ttu-id="19395-236">在每个表达式的开头</span><span class="sxs-lookup"><span data-stu-id="19395-236">At the beginning of each expression</span></span>
*  <span data-ttu-id="19395-237">每个表达式的末尾</span><span class="sxs-lookup"><span data-stu-id="19395-237">At the end of each expression</span></span>

<span data-ttu-id="19395-238">明确赋值国*v*可以是：</span><span class="sxs-lookup"><span data-stu-id="19395-238">The definite assignment state of *v* can be either:</span></span>

*  <span data-ttu-id="19395-239">明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-239">Definitely assigned.</span></span> <span data-ttu-id="19395-240">这指示在到目前为止，所有可能的控制流上*v*已分配有一个值。</span><span class="sxs-lookup"><span data-stu-id="19395-240">This indicates that on all possible control flows to this point, *v* has been assigned a value.</span></span>
*  <span data-ttu-id="19395-241">不明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-241">Not definitely assigned.</span></span> <span data-ttu-id="19395-242">变量类型的表达式结尾处的状态`bool`，未明确赋值可能 （但不一定） 的变量的状态划分为以下子状态之一：</span><span class="sxs-lookup"><span data-stu-id="19395-242">For the state of a variable at the end of an expression of type `bool`, the state of a variable that isn't definitely assigned may (but doesn't necessarily) fall into one of the following sub-states:</span></span>
    * <span data-ttu-id="19395-243">明确赋值后，则返回 true 的表达式。</span><span class="sxs-lookup"><span data-stu-id="19395-243">Definitely assigned after true expression.</span></span> <span data-ttu-id="19395-244">此状态表明*v*如果布尔表达式计算为 true，但如果布尔表达式的计算结果为 false 则不一定要分配明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-244">This state indicates that *v* is definitely assigned if the boolean expression evaluated as true, but is not necessarily assigned if the boolean expression evaluated as false.</span></span>
    * <span data-ttu-id="19395-245">明确赋值后 false 的表达式。</span><span class="sxs-lookup"><span data-stu-id="19395-245">Definitely assigned after false expression.</span></span> <span data-ttu-id="19395-246">此状态表明*v*如果布尔表达式计算结果为 false，但如果布尔表达式计算为 true 则不一定要分配明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-246">This state indicates that *v* is definitely assigned if the boolean expression evaluated as false, but is not necessarily assigned if the boolean expression evaluated as true.</span></span>

<span data-ttu-id="19395-247">以下规则控制如何将变量的状态*v*在每个位置确定。</span><span class="sxs-lookup"><span data-stu-id="19395-247">The following rules govern how the state of a variable *v* is determined at each location.</span></span>

#### <a name="general-rules-for-statements"></a><span data-ttu-id="19395-248">语句的一般规则</span><span class="sxs-lookup"><span data-stu-id="19395-248">General rules for statements</span></span>

*  <span data-ttu-id="19395-249">*v*开头的函数成员正文中未明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-249">*v* is not definitely assigned at the beginning of a function member body.</span></span>
*  <span data-ttu-id="19395-250">*v*明确赋值无法访问的任何语句开头。</span><span class="sxs-lookup"><span data-stu-id="19395-250">*v* is definitely assigned at the beginning of any unreachable statement.</span></span>
*  <span data-ttu-id="19395-251">明确赋值国*v*任何其他语句开始处通过检查的明确赋值状态来确定*v*上为目标的开头的所有控制流传输语句。</span><span class="sxs-lookup"><span data-stu-id="19395-251">The definite assignment state of *v* at the beginning of any other statement is determined by checking the definite assignment state of *v* on all control flow transfers that target the beginning of that statement.</span></span> <span data-ttu-id="19395-252">当 （且仅当） *v*明确赋值的所有这些控制流传输，然后*v*明确赋值语句的开始处。</span><span class="sxs-lookup"><span data-stu-id="19395-252">If (and only if) *v* is definitely assigned on all such control flow transfers, then *v* is definitely assigned at the beginning of the statement.</span></span> <span data-ttu-id="19395-253">用于检查语句可访问性的相同方式确定可能的控制流传输的集 ([终结点和可访问性](statements.md#end-points-and-reachability))。</span><span class="sxs-lookup"><span data-stu-id="19395-253">The set of possible control flow transfers is determined in the same way as for checking statement reachability ([End points and reachability](statements.md#end-points-and-reachability)).</span></span>
*  <span data-ttu-id="19395-254">明确赋值国*v*在块中，终结点`checked`， `unchecked`， `if`， `while`， `do`， `for`， `foreach`， `lock`， `using`，或`switch`通过检查的明确赋值状态来确定语句*v*面向该语句的结束点的所有控制流传输。</span><span class="sxs-lookup"><span data-stu-id="19395-254">The definite assignment state of *v* at the end point of a block, `checked`, `unchecked`, `if`, `while`, `do`, `for`, `foreach`, `lock`, `using`, or `switch` statement is determined by checking the definite assignment state of *v* on all control flow transfers that target the end point of that statement.</span></span> <span data-ttu-id="19395-255">如果*v*明确赋值的所有这些控制流传输，然后*v*明确赋值语句的结束点。</span><span class="sxs-lookup"><span data-stu-id="19395-255">If *v* is definitely assigned on all such control flow transfers, then *v* is definitely assigned at the end point of the statement.</span></span> <span data-ttu-id="19395-256">否则为*v*在终结点的语句不明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-256">Otherwise; *v* is not definitely assigned at the end point of the statement.</span></span> <span data-ttu-id="19395-257">用于检查语句可访问性的相同方式确定可能的控制流传输的集 ([终结点和可访问性](statements.md#end-points-and-reachability))。</span><span class="sxs-lookup"><span data-stu-id="19395-257">The set of possible control flow transfers is determined in the same way as for checking statement reachability ([End points and reachability](statements.md#end-points-and-reachability)).</span></span>

#### <a name="block-statements-checked-and-unchecked-statements"></a><span data-ttu-id="19395-258">选中之后，块语句和 unchecked 的语句</span><span class="sxs-lookup"><span data-stu-id="19395-258">Block statements, checked, and unchecked statements</span></span>

<span data-ttu-id="19395-259">明确赋值国*v*在控件上传输到第一个语句块中的语句列表 （或块，如果语句列表为空的终结点） 等同于明确的赋值语句*v*块的前面`checked`，或`unchecked`语句。</span><span class="sxs-lookup"><span data-stu-id="19395-259">The definite assignment state of *v* on the control transfer to the first statement of the statement list in the block (or to the end point of the block, if the statement list is empty) is the same as the definite assignment statement of *v* before the block, `checked`, or `unchecked` statement.</span></span>

#### <a name="expression-statements"></a><span data-ttu-id="19395-260">表达式语句</span><span class="sxs-lookup"><span data-stu-id="19395-260">Expression statements</span></span>

<span data-ttu-id="19395-261">表达式语句*stmt*表达式组成*expr*:</span><span class="sxs-lookup"><span data-stu-id="19395-261">For an expression statement *stmt* that consists of the expression *expr*:</span></span>

*  <span data-ttu-id="19395-262">*v*具有的开始处的同一个明确的赋值状态*expr*作为开头*stmt*。</span><span class="sxs-lookup"><span data-stu-id="19395-262">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="19395-263">如果*v*如果明确赋值的结尾处*expr*，在终结点的明确赋值*stmt*; 否则为未明确赋值在终结点*stmt*。</span><span class="sxs-lookup"><span data-stu-id="19395-263">If *v* if definitely assigned at the end of *expr*, it is definitely assigned at the end point of *stmt*; otherwise; it is not definitely assigned at the end point of *stmt*.</span></span>

#### <a name="declaration-statements"></a><span data-ttu-id="19395-264">声明语句</span><span class="sxs-lookup"><span data-stu-id="19395-264">Declaration statements</span></span>

*  <span data-ttu-id="19395-265">如果*stmt*是声明语句没有初始值设定项，然后*v*具有在的终结点的同一个明确的赋值状态*stmt*作为开头*stmt*。</span><span class="sxs-lookup"><span data-stu-id="19395-265">If *stmt* is a declaration statement without initializers, then *v* has the same definite assignment state at the end point of *stmt* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="19395-266">如果*stmt*是明确的赋值状态的声明语句具有初始值设定项，然后*v*决定好像*stmt*是一个语句列表，其中一个分配对于每个声明使用初始值设定项 （按顺序列出声明） 的语句。</span><span class="sxs-lookup"><span data-stu-id="19395-266">If *stmt* is a declaration statement with initializers, then the definite assignment state for *v* is determined as if *stmt* were a statement list, with one assignment statement for each declaration with an initializer (in the order of declaration).</span></span>

#### <a name="if-statements"></a><span data-ttu-id="19395-267">如果语句</span><span class="sxs-lookup"><span data-stu-id="19395-267">If statements</span></span>

<span data-ttu-id="19395-268">有关`if`语句*stmt*窗体：</span><span class="sxs-lookup"><span data-stu-id="19395-268">For an `if` statement *stmt* of the form:</span></span>
```csharp
if ( expr ) then_stmt else else_stmt
```

*  <span data-ttu-id="19395-269">*v*具有的开始处的同一个明确的赋值状态*expr*作为开头*stmt*。</span><span class="sxs-lookup"><span data-stu-id="19395-269">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="19395-270">如果*v*末尾的明确赋值*expr*，然后它指向的控制流转移上明确赋值*then_stmt* ，并*else_stmt*或到的终结点*stmt*如果没有其他子句。</span><span class="sxs-lookup"><span data-stu-id="19395-270">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to *then_stmt* and to either *else_stmt* or to the end-point of *stmt* if there is no else clause.</span></span>
*  <span data-ttu-id="19395-271">如果*v*末尾处具有"明确赋值后，则返回 true 的表达式"的状态*expr*，然后它指向的控制流转移上明确赋值*then_stmt*，而不在控制流传输到任何一个上明确赋值*else_stmt*或到的终结点*stmt*如果没有其他子句。</span><span class="sxs-lookup"><span data-stu-id="19395-271">If *v* has the state "definitely assigned after true expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *then_stmt*, and not definitely assigned on the control flow transfer to either *else_stmt* or to the end-point of *stmt* if there is no else clause.</span></span>
*  <span data-ttu-id="19395-272">如果*v*末尾处具有"明确赋值后 false 的表达式"的状态*expr*，然后它指向的控制流转移上明确赋值*else_stmt*，而不在控制流传输到上明确赋值*then_stmt*。</span><span class="sxs-lookup"><span data-stu-id="19395-272">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *else_stmt*, and not definitely assigned on the control flow transfer to *then_stmt*.</span></span> <span data-ttu-id="19395-273">在终结点的明确赋值*stmt*当且仅当在终结点的明确赋值*then_stmt*。</span><span class="sxs-lookup"><span data-stu-id="19395-273">It is definitely assigned at the end-point of *stmt* if and only if it is definitely assigned at the end-point of *then_stmt*.</span></span>
*  <span data-ttu-id="19395-274">否则为*v*被视为不在控制流传输到任何一个上明确赋值*then_stmt*或*else_stmt*，或向终结点的*stmt*如果没有其他子句。</span><span class="sxs-lookup"><span data-stu-id="19395-274">Otherwise, *v* is considered not definitely assigned on the control flow transfer to either the *then_stmt* or *else_stmt*, or to the end-point of *stmt* if there is no else clause.</span></span>

#### <a name="switch-statements"></a><span data-ttu-id="19395-275">Switch 语句</span><span class="sxs-lookup"><span data-stu-id="19395-275">Switch statements</span></span>

<span data-ttu-id="19395-276">在中`switch`语句*stmt*与控制表达式*expr*:</span><span class="sxs-lookup"><span data-stu-id="19395-276">In a `switch` statement *stmt* with a controlling expression *expr*:</span></span>

*  <span data-ttu-id="19395-277">明确赋值状态*v*的开头*expr*的状态相同*v*开头*stmt*。</span><span class="sxs-lookup"><span data-stu-id="19395-277">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="19395-278">明确赋值国*v*上的控制流传输到可访问的 switch 块语句列表是明确的赋值状态的相同*v*末尾的*expr*.</span><span class="sxs-lookup"><span data-stu-id="19395-278">The definite assignment state of *v* on the control flow transfer to a reachable switch block statement list is the same as the definite assignment state of *v* at the end of *expr*.</span></span>

#### <a name="while-statements"></a><span data-ttu-id="19395-279">While 语句</span><span class="sxs-lookup"><span data-stu-id="19395-279">While statements</span></span>

<span data-ttu-id="19395-280">有关`while`语句*stmt*窗体：</span><span class="sxs-lookup"><span data-stu-id="19395-280">For a `while` statement *stmt* of the form:</span></span>
```csharp
while ( expr ) while_body
```

*  <span data-ttu-id="19395-281">*v*具有的开始处的同一个明确的赋值状态*expr*作为开头*stmt*。</span><span class="sxs-lookup"><span data-stu-id="19395-281">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="19395-282">如果*v*末尾的明确赋值*expr*，然后它指向的控制流转移上明确赋值*while_body*和终结点到*stmt*。</span><span class="sxs-lookup"><span data-stu-id="19395-282">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to *while_body* and to the end point of *stmt*.</span></span>
*  <span data-ttu-id="19395-283">如果*v*末尾处具有"明确赋值后，则返回 true 的表达式"的状态*expr*，然后它指向的控制流转移上明确赋值*while_body*，但不是在终结点的明确赋值*stmt*。</span><span class="sxs-lookup"><span data-stu-id="19395-283">If *v* has the state "definitely assigned after true expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *while_body*, but not definitely assigned at the end-point of *stmt*.</span></span>
*  <span data-ttu-id="19395-284">如果*v*末尾处具有"明确赋值后 false 的表达式"的状态*expr*，则它控制流传输到的终结点上明确赋值*stmt*但不是在控制流传输到明确赋值*while_body*。</span><span class="sxs-lookup"><span data-stu-id="19395-284">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*, but not definitely assigned on the control flow transfer to *while_body*.</span></span>

#### <a name="do-statements"></a><span data-ttu-id="19395-285">执行语句</span><span class="sxs-lookup"><span data-stu-id="19395-285">Do statements</span></span>

<span data-ttu-id="19395-286">有关`do`语句*stmt*窗体：</span><span class="sxs-lookup"><span data-stu-id="19395-286">For a `do` statement *stmt* of the form:</span></span>
```csharp
do do_body while ( expr ) ;
```

*  <span data-ttu-id="19395-287">*v*已从开始处的控制流转移上的同一个明确的赋值状态*stmt*到*do_body*一样的开始处*stmt*。</span><span class="sxs-lookup"><span data-stu-id="19395-287">*v* has the same definite assignment state on the control flow transfer from the beginning of *stmt* to *do_body* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="19395-288">*v*具有的开始处的同一个明确的赋值状态*expr*为终结点*do_body*。</span><span class="sxs-lookup"><span data-stu-id="19395-288">*v* has the same definite assignment state at the beginning of *expr* as at the end point of *do_body*.</span></span>
*  <span data-ttu-id="19395-289">如果*v*末尾的明确赋值*expr*，则它控制流传输到的终结点上明确赋值*stmt*。</span><span class="sxs-lookup"><span data-stu-id="19395-289">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*.</span></span>
*  <span data-ttu-id="19395-290">如果*v*末尾处具有"明确赋值后 false 的表达式"的状态*expr*，则它控制流传输到的终结点上明确赋值*stmt*.</span><span class="sxs-lookup"><span data-stu-id="19395-290">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*.</span></span>

#### <a name="for-statements"></a><span data-ttu-id="19395-291">语句</span><span class="sxs-lookup"><span data-stu-id="19395-291">For statements</span></span>

<span data-ttu-id="19395-292">正在检查的明确赋值`for`窗体的语句：</span><span class="sxs-lookup"><span data-stu-id="19395-292">Definite assignment checking for a `for` statement of the form:</span></span>
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
<span data-ttu-id="19395-293">编写该语句已完成：</span><span class="sxs-lookup"><span data-stu-id="19395-293">is done as if the statement were written:</span></span>
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

<span data-ttu-id="19395-294">如果*for_condition*中省略`for`语句，然后评估明确赋值将继续像*for_condition*已替换为`true`上述扩展中.</span><span class="sxs-lookup"><span data-stu-id="19395-294">If the *for_condition* is omitted from the `for` statement, then evaluation of definite assignment proceeds as if *for_condition* were replaced with `true` in the above expansion.</span></span>

#### <a name="break-continue-and-goto-statements"></a><span data-ttu-id="19395-295">中断、 继续，请和 goto 语句</span><span class="sxs-lookup"><span data-stu-id="19395-295">Break, continue, and goto statements</span></span>

<span data-ttu-id="19395-296">明确赋值国*v*上引起的控制流转移`break`， `continue`，或`goto`语句是明确的赋值状态的相同*v*在该语句的起始处。</span><span class="sxs-lookup"><span data-stu-id="19395-296">The definite assignment state of *v* on the control flow transfer caused by a `break`, `continue`, or `goto` statement is the same as the definite assignment state of *v* at the beginning of the statement.</span></span>

#### <a name="throw-statements"></a><span data-ttu-id="19395-297">Throw 语句</span><span class="sxs-lookup"><span data-stu-id="19395-297">Throw statements</span></span>

<span data-ttu-id="19395-298">语句*stmt*窗体</span><span class="sxs-lookup"><span data-stu-id="19395-298">For a statement *stmt* of the form</span></span>
```csharp
throw expr ;
```

<span data-ttu-id="19395-299">明确赋值状态*v*的开头*expr*是明确的赋值状态的相同*v*开头*stmt*.</span><span class="sxs-lookup"><span data-stu-id="19395-299">The definite assignment state of *v* at the beginning of *expr* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>

#### <a name="return-statements"></a><span data-ttu-id="19395-300">Return 语句</span><span class="sxs-lookup"><span data-stu-id="19395-300">Return statements</span></span>

<span data-ttu-id="19395-301">语句*stmt*窗体</span><span class="sxs-lookup"><span data-stu-id="19395-301">For a statement *stmt* of the form</span></span>
```csharp
return expr ;
```

*  <span data-ttu-id="19395-302">明确赋值状态*v*的开头*expr*是明确的赋值状态的相同*v*开头*stmt*.</span><span class="sxs-lookup"><span data-stu-id="19395-302">The definite assignment state of *v* at the beginning of *expr* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="19395-303">如果*v*为 output 参数，则它必须绝对分配：</span><span class="sxs-lookup"><span data-stu-id="19395-303">If *v* is an output parameter, then it must be definitely assigned either:</span></span>
    * <span data-ttu-id="19395-304">后*expr*</span><span class="sxs-lookup"><span data-stu-id="19395-304">after *expr*</span></span>
    * <span data-ttu-id="19395-305">或末尾处`finally`块`try` - `finally`或`try` - `catch` - `finally`包含`return`语句。</span><span class="sxs-lookup"><span data-stu-id="19395-305">or at the end of the `finally` block of a `try`-`finally` or `try`-`catch`-`finally` that encloses the `return` statement.</span></span>

<span data-ttu-id="19395-306">为窗体语句 stmt:</span><span class="sxs-lookup"><span data-stu-id="19395-306">For a statement stmt of the form:</span></span>
```csharp
return ;
```

*  <span data-ttu-id="19395-307">如果*v*为 output 参数，则它必须绝对分配：</span><span class="sxs-lookup"><span data-stu-id="19395-307">If *v* is an output parameter, then it must be definitely assigned either:</span></span>
    * <span data-ttu-id="19395-308">before *stmt*</span><span class="sxs-lookup"><span data-stu-id="19395-308">before *stmt*</span></span>
    * <span data-ttu-id="19395-309">或末尾处`finally`块`try` - `finally`或`try` - `catch` - `finally`包含`return`语句。</span><span class="sxs-lookup"><span data-stu-id="19395-309">or at the end of the `finally` block of a `try`-`finally` or `try`-`catch`-`finally` that encloses the `return` statement.</span></span>

#### <a name="try-catch-statements"></a><span data-ttu-id="19395-310">Try catch 语句</span><span class="sxs-lookup"><span data-stu-id="19395-310">Try-catch statements</span></span>

<span data-ttu-id="19395-311">语句*stmt*窗体：</span><span class="sxs-lookup"><span data-stu-id="19395-311">For a statement *stmt* of the form:</span></span>
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  <span data-ttu-id="19395-312">明确赋值状态*v*的开头*try_block*是明确的赋值状态的相同*v*开头*stmt*.</span><span class="sxs-lookup"><span data-stu-id="19395-312">The definite assignment state of *v* at the beginning of *try_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="19395-313">明确赋值国*v*的开头*catch_block_i* (任何*我*) 是明确的赋值状态的相同*v*的开头*stmt*。</span><span class="sxs-lookup"><span data-stu-id="19395-313">The definite assignment state of *v* at the beginning of *catch_block_i* (for any *i*) is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="19395-314">明确赋值国*v*处的终结点*stmt*是明确赋值的当 （且仅当） *v*明确赋值的结束点处*try_block*和每*catch_block_i* (为每个*我*从 1 到*n*)。</span><span class="sxs-lookup"><span data-stu-id="19395-314">The definite assignment state of *v* at the end-point of *stmt* is definitely assigned if (and only if) *v* is definitely assigned at the end-point of *try_block* and every *catch_block_i* (for every *i* from 1 to *n*).</span></span>

#### <a name="try-finally-statements"></a><span data-ttu-id="19395-315">Try finally 语句</span><span class="sxs-lookup"><span data-stu-id="19395-315">Try-finally statements</span></span>

<span data-ttu-id="19395-316">有关`try`语句*stmt*窗体：</span><span class="sxs-lookup"><span data-stu-id="19395-316">For a `try` statement *stmt* of the form:</span></span>
```csharp
try try_block finally finally_block
```

*  <span data-ttu-id="19395-317">明确赋值状态*v*的开头*try_block*是明确的赋值状态的相同*v*开头*stmt*.</span><span class="sxs-lookup"><span data-stu-id="19395-317">The definite assignment state of *v* at the beginning of *try_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="19395-318">明确赋值状态*v*的开头*finally_block*是明确的赋值状态的相同*v*开头*stmt*.</span><span class="sxs-lookup"><span data-stu-id="19395-318">The definite assignment state of *v* at the beginning of *finally_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="19395-319">明确赋值国*v*处的终结点*stmt*是明确赋值的当 （且仅当） 至少下列任一条件为 true:</span><span class="sxs-lookup"><span data-stu-id="19395-319">The definite assignment state of *v* at the end-point of *stmt* is definitely assigned if (and only if) at least one of the following is true:</span></span>
    * <span data-ttu-id="19395-320">*v*明确赋值的结束点处*try_block*</span><span class="sxs-lookup"><span data-stu-id="19395-320">*v* is definitely assigned at the end-point of *try_block*</span></span>
    * <span data-ttu-id="19395-321">*v*明确赋值的结束点处*finally_block*</span><span class="sxs-lookup"><span data-stu-id="19395-321">*v* is definitely assigned at the end-point of *finally_block*</span></span>

<span data-ttu-id="19395-322">如果控制流传输 (例如，`goto`语句) 进行了，则在开始*try_block*，和外部的结束*try_block*，然后*v*也是如果认为是该控制流转移上明确赋值*v*明确赋值的结束点处*finally_block*。</span><span class="sxs-lookup"><span data-stu-id="19395-322">If a control flow transfer (for example, a `goto` statement) is made that begins within *try_block*, and ends outside of *try_block*, then *v* is also considered definitely assigned on that control flow transfer if *v* is definitely assigned at the end-point of *finally_block*.</span></span> <span data-ttu-id="19395-323">(这不是仅当 — 如果*v*则它仍被视为明确赋值明确赋值上此控制流传输，另一个原因。)</span><span class="sxs-lookup"><span data-stu-id="19395-323">(This is not an only if—if *v* is definitely assigned for another reason on this control flow transfer, then it is still considered definitely assigned.)</span></span>

#### <a name="try-catch-finally-statements"></a><span data-ttu-id="19395-324">Try – catch – finally 语句</span><span class="sxs-lookup"><span data-stu-id="19395-324">Try-catch-finally statements</span></span>

<span data-ttu-id="19395-325">为明确的赋值 analysis `try` - `catch` - `finally`窗体的语句：</span><span class="sxs-lookup"><span data-stu-id="19395-325">Definite assignment analysis for a `try`-`catch`-`finally` statement of the form:</span></span>
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
<span data-ttu-id="19395-326">像该语句已完成`try` - `finally`语句封闭`try` - `catch`语句：</span><span class="sxs-lookup"><span data-stu-id="19395-326">is done as if the statement were a `try`-`finally` statement enclosing a `try`-`catch` statement:</span></span>
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

<span data-ttu-id="19395-327">下面的示例演示如何不同的块`try`语句 ([try 语句](statements.md#the-try-statement)) 会影响明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-327">The following example demonstrates how the different blocks of a `try` statement ([The try statement](statements.md#the-try-statement)) affect definite assignment.</span></span>
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

#### <a name="foreach-statements"></a><span data-ttu-id="19395-328">Foreach 语句</span><span class="sxs-lookup"><span data-stu-id="19395-328">Foreach statements</span></span>

<span data-ttu-id="19395-329">有关`foreach`语句*stmt*窗体：</span><span class="sxs-lookup"><span data-stu-id="19395-329">For a `foreach` statement *stmt* of the form:</span></span>
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  <span data-ttu-id="19395-330">明确赋值状态*v*的开头*expr*的状态相同*v*开头*stmt*。</span><span class="sxs-lookup"><span data-stu-id="19395-330">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="19395-331">明确赋值国*v*在控制流传输到*embedded_statement*或终结点的*stmt*的状态相同*v*的末尾*expr*。</span><span class="sxs-lookup"><span data-stu-id="19395-331">The definite assignment state of *v* on the control flow transfer to *embedded_statement* or to the end point of *stmt* is the same as the state of *v* at the end of *expr*.</span></span>

#### <a name="using-statements"></a><span data-ttu-id="19395-332">Using 语句</span><span class="sxs-lookup"><span data-stu-id="19395-332">Using statements</span></span>

<span data-ttu-id="19395-333">有关`using`语句*stmt*窗体：</span><span class="sxs-lookup"><span data-stu-id="19395-333">For a `using` statement *stmt* of the form:</span></span>
```csharp
using ( resource_acquisition ) embedded_statement
```

*  <span data-ttu-id="19395-334">明确赋值状态*v*的开头*resource_acquisition*的状态相同*v*开头*stmt*.</span><span class="sxs-lookup"><span data-stu-id="19395-334">The definite assignment state of *v* at the beginning of *resource_acquisition* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="19395-335">明确赋值国*v*在控制流传输到*embedded_statement*的状态相同*v*末尾的*resource_获取*。</span><span class="sxs-lookup"><span data-stu-id="19395-335">The definite assignment state of *v* on the control flow transfer to *embedded_statement* is the same as the state of *v* at the end of *resource_acquisition*.</span></span>

#### <a name="lock-statements"></a><span data-ttu-id="19395-336">Lock 语句</span><span class="sxs-lookup"><span data-stu-id="19395-336">Lock statements</span></span>

<span data-ttu-id="19395-337">有关`lock`语句*stmt*窗体：</span><span class="sxs-lookup"><span data-stu-id="19395-337">For a `lock` statement *stmt* of the form:</span></span>
```csharp
lock ( expr ) embedded_statement
```

*  <span data-ttu-id="19395-338">明确赋值状态*v*的开头*expr*的状态相同*v*开头*stmt*。</span><span class="sxs-lookup"><span data-stu-id="19395-338">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="19395-339">明确赋值国*v*在控制流传输到*embedded_statement*的状态相同*v*末尾的*expr*.</span><span class="sxs-lookup"><span data-stu-id="19395-339">The definite assignment state of *v* on the control flow transfer to *embedded_statement* is the same as the state of *v* at the end of *expr*.</span></span>

#### <a name="yield-statements"></a><span data-ttu-id="19395-340">Yield 语句</span><span class="sxs-lookup"><span data-stu-id="19395-340">Yield statements</span></span>

<span data-ttu-id="19395-341">有关`yield return`语句*stmt*窗体：</span><span class="sxs-lookup"><span data-stu-id="19395-341">For a `yield return` statement *stmt* of the form:</span></span>
```csharp
yield return expr ;
```

*  <span data-ttu-id="19395-342">明确赋值状态*v*的开头*expr*的状态相同*v*开头*stmt*。</span><span class="sxs-lookup"><span data-stu-id="19395-342">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="19395-343">明确赋值状态*v*末尾处*stmt*的状态相同*v*末尾*expr*。</span><span class="sxs-lookup"><span data-stu-id="19395-343">The definite assignment state of *v* at the end of *stmt* is the same as the state of *v* at the end of *expr*.</span></span>
*  <span data-ttu-id="19395-344">一个`yield break`语句具有明确的赋值状态没有影响。</span><span class="sxs-lookup"><span data-stu-id="19395-344">A `yield break` statement has no effect on the definite assignment state.</span></span>

#### <a name="general-rules-for-simple-expressions"></a><span data-ttu-id="19395-345">简单表达式的一般规则</span><span class="sxs-lookup"><span data-stu-id="19395-345">General rules for simple expressions</span></span>

<span data-ttu-id="19395-346">以下规则适用于这些类型的表达式： 文本 ([文字](expressions.md#literals))，简单名称 ([简单名称](expressions.md#simple-names))，成员访问表达式 ([成员访问](expressions.md#member-access))，非索引基访问表达式 ([基本访问权限](expressions.md#base-access))，`typeof`表达式 ([typeof 运算符](expressions.md#the-typeof-operator))，默认值表达式 ([默认值表达式](expressions.md#default-value-expressions)) 和`nameof`表达式 ([Nameof 表达式](expressions.md#nameof-expressions))。</span><span class="sxs-lookup"><span data-stu-id="19395-346">The following rule applies to these kinds of expressions: literals ([Literals](expressions.md#literals)), simple names ([Simple names](expressions.md#simple-names)), member access expressions ([Member access](expressions.md#member-access)), non-indexed base access expressions ([Base access](expressions.md#base-access)), `typeof` expressions ([The typeof operator](expressions.md#the-typeof-operator)), default value expressions ([Default value expressions](expressions.md#default-value-expressions)) and `nameof` expressions ([Nameof expressions](expressions.md#nameof-expressions)).</span></span>

*  <span data-ttu-id="19395-347">明确赋值国*v*末尾的此类表达式是明确的赋值状态的相同*v*表达式的开头。</span><span class="sxs-lookup"><span data-stu-id="19395-347">The definite assignment state of *v* at the end of such an expression is the same as the definite assignment state of *v* at the beginning of the expression.</span></span>

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a><span data-ttu-id="19395-348">使用嵌入式表达式的表达式的一般规则</span><span class="sxs-lookup"><span data-stu-id="19395-348">General rules for expressions with embedded expressions</span></span>

<span data-ttu-id="19395-349">以下规则适用于这些类型的表达式： 带括号的表达式 ([带括号的表达式](expressions.md#parenthesized-expressions))，元素访问表达式 ([元素访问](expressions.md#element-access))、 基访问表达式索引 ([基本访问权限](expressions.md#base-access))、 递增和递减表达式 ([后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators)，[前缀递增和递减运算符](expressions.md#prefix-increment-and-decrement-operators))，强制转换表达式 ([强制转换表达式](expressions.md#cast-expressions))，一元`+`， `-`， `~`，`*`表达式、 二进制`+`， `-`， `*`，`/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`， `|`，`^`表达式 ([算术运算符](expressions.md#arithmetic-operators)，[移位运算符](expressions.md#shift-operators)，[关系和类型测试运算符](expressions.md#relational-and-type-testing-operators)[逻辑运算符](expressions.md#logical-operators))，复合赋值表达式 ([复合赋值](expressions.md#compound-assignment))，`checked`并`unchecked`表达式 ([checked 和 unchecked运算符](expressions.md#the-checked-and-unchecked-operators))，以及数组和委托创建表达式 ([new 运算符](expressions.md#the-new-operator))。</span><span class="sxs-lookup"><span data-stu-id="19395-349">The following rules apply to these kinds of expressions: parenthesized expressions ([Parenthesized expressions](expressions.md#parenthesized-expressions)), element access expressions ([Element access](expressions.md#element-access)), base access expressions with indexing ([Base access](expressions.md#base-access)), increment and decrement expressions ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), cast expressions ([Cast expressions](expressions.md#cast-expressions)), unary `+`, `-`, `~`, `*` expressions, binary `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`, `|`, `^` expressions ([Arithmetic operators](expressions.md#arithmetic-operators), [Shift operators](expressions.md#shift-operators), [Relational and type-testing operators](expressions.md#relational-and-type-testing-operators), [Logical operators](expressions.md#logical-operators)), compound assignment expressions ([Compound assignment](expressions.md#compound-assignment)), `checked` and `unchecked` expressions ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), plus array and delegate creation expressions ([The new operator](expressions.md#the-new-operator)).</span></span>

<span data-ttu-id="19395-350">每两个表达式都有一个或多个无条件地按固定顺序计算的子表达式。</span><span class="sxs-lookup"><span data-stu-id="19395-350">Each of these expressions has one or more sub-expressions that are unconditionally evaluated in a fixed order.</span></span> <span data-ttu-id="19395-351">例如，二进制`%`运算符会求值的左侧的运算符，然后在右侧。</span><span class="sxs-lookup"><span data-stu-id="19395-351">For example, the binary `%` operator evaluates the left hand side of the operator, then the right hand side.</span></span> <span data-ttu-id="19395-352">索引操作的索引的表达式的计算结果，然后计算每个索引表达式，从左到右的顺序。</span><span class="sxs-lookup"><span data-stu-id="19395-352">An indexing operation evaluates the indexed expression, and then evaluates each of the index expressions, in order from left to right.</span></span> <span data-ttu-id="19395-353">表达式*expr*，其中包含的子表达式*e1、 e2，...，eN*、 按该顺序计算：</span><span class="sxs-lookup"><span data-stu-id="19395-353">For an expression *expr*, which has sub-expressions *e1, e2, ..., eN*, evaluated in that order:</span></span>

*  <span data-ttu-id="19395-354">明确赋值国*v*的开头*e1*相同的开始处的明确赋值状态*expr*。</span><span class="sxs-lookup"><span data-stu-id="19395-354">The definite assignment state of *v* at the beginning of *e1* is the same as the definite assignment state at the beginning of *expr*.</span></span>
*  <span data-ttu-id="19395-355">明确赋值国*v*的开头*ei* (*我*大于 1) 等同于上一个子表达式的末尾的明确赋值状态。</span><span class="sxs-lookup"><span data-stu-id="19395-355">The definite assignment state of *v* at the beginning of *ei* (*i* greater than one) is the same as the definite assignment state at the end of the previous sub-expression.</span></span>
*  <span data-ttu-id="19395-356">明确赋值国*v*末尾处*expr*等同于结尾处的明确赋值状态*eN*</span><span class="sxs-lookup"><span data-stu-id="19395-356">The definite assignment state of *v* at the end of *expr* is the same as the definite assignment state at the end of *eN*</span></span>

#### <a name="invocation-expressions-and-object-creation-expressions"></a><span data-ttu-id="19395-357">调用表达式和对象创建表达式</span><span class="sxs-lookup"><span data-stu-id="19395-357">Invocation expressions and object creation expressions</span></span>

<span data-ttu-id="19395-358">调用表达式*expr*窗体：</span><span class="sxs-lookup"><span data-stu-id="19395-358">For an invocation expression *expr* of the form:</span></span>
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
<span data-ttu-id="19395-359">或在窗体的对象创建表达式：</span><span class="sxs-lookup"><span data-stu-id="19395-359">or an object creation expression of the form:</span></span>
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  <span data-ttu-id="19395-360">调用表达式，明确赋值国*v*之前*primary_expression*的状态相同*v*之前*expr*.</span><span class="sxs-lookup"><span data-stu-id="19395-360">For an invocation expression, the definite assignment state of *v* before *primary_expression* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="19395-361">调用表达式，明确赋值国*v*之前*arg1*的状态相同*v*后*primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="19395-361">For an invocation expression, the definite assignment state of *v* before *arg1* is the same as the state of *v* after *primary_expression*.</span></span>
*  <span data-ttu-id="19395-362">为对象创建表达式，明确赋值国*v*之前*arg1*的状态相同*v*之前*expr*。</span><span class="sxs-lookup"><span data-stu-id="19395-362">For an object creation expression, the definite assignment state of *v* before *arg1* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="19395-363">对于每个自变量*argi*，明确赋值国*v*后*argi*由正则表达式规则，忽略任何`ref`或`out`修饰符。</span><span class="sxs-lookup"><span data-stu-id="19395-363">For each argument *argi*, the definite assignment state of *v* after *argi* is determined by the normal expression rules, ignoring any `ref` or `out` modifiers.</span></span>
*  <span data-ttu-id="19395-364">对于每个自变量*argi*任何*我*大于 1 的明确赋值状态*v*之前*argi*的状态相同*v*之后的上一个*arg*。</span><span class="sxs-lookup"><span data-stu-id="19395-364">For each argument *argi* for any *i* greater than one, the definite assignment state of *v* before *argi* is the same as the state of *v* after the previous *arg*.</span></span>
*  <span data-ttu-id="19395-365">如果将变量*v*作为传递`out`自变量 (即，在窗体的自变量`out v`) 中的参数，则状态的任何*v*后*expr*明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-365">If the variable *v* is passed as an `out` argument (i.e., an argument of the form `out v`) in any of the arguments, then the state of *v* after *expr* is definitely assigned.</span></span> <span data-ttu-id="19395-366">否则为状态*v*后*expr*的状态相同*v*后*argN*。</span><span class="sxs-lookup"><span data-stu-id="19395-366">Otherwise; the state of *v* after *expr* is the same as the state of *v* after *argN*.</span></span>
*  <span data-ttu-id="19395-367">有关数组初始值设定项 ([数组创建表达式](expressions.md#array-creation-expressions))，对象初始值设定项 ([对象初始值设定项](expressions.md#object-initializers))，集合初始值设定项 ([集合初始值设定项](expressions.md#collection-initializers)) 和匿名对象初始值设定项 ([匿名对象创建表达式](expressions.md#anonymous-object-creation-expressions))，明确赋值状态由这些构造定义的扩展。</span><span class="sxs-lookup"><span data-stu-id="19395-367">For array initializers ([Array creation expressions](expressions.md#array-creation-expressions)), object initializers ([Object initializers](expressions.md#object-initializers)), collection initializers ([Collection initializers](expressions.md#collection-initializers)) and anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)), the definite assignment state is determined by the expansion that these constructs are defined in terms of.</span></span>

#### <a name="simple-assignment-expressions"></a><span data-ttu-id="19395-368">简单的赋值表达式</span><span class="sxs-lookup"><span data-stu-id="19395-368">Simple assignment expressions</span></span>

<span data-ttu-id="19395-369">表达式*expr*窗体的`w = expr_rhs`:</span><span class="sxs-lookup"><span data-stu-id="19395-369">For an expression *expr* of the form `w = expr_rhs`:</span></span>

*  <span data-ttu-id="19395-370">明确赋值国*v*之前*expr_rhs*是明确的赋值状态的相同*v*之前*expr*。</span><span class="sxs-lookup"><span data-stu-id="19395-370">The definite assignment state of *v* before *expr_rhs* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="19395-371">明确赋值国*v*后*expr*由：</span><span class="sxs-lookup"><span data-stu-id="19395-371">The definite assignment state of *v* after *expr* is determined by:</span></span>
   * <span data-ttu-id="19395-372">如果*w*是相同的变量作为*v*，然后的明确赋值状态*v*后*expr*明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-372">If *w* is the same variable as *v*, then the definite assignment state of *v* after *expr* is definitely assigned.</span></span>
   * <span data-ttu-id="19395-373">否则为如果进行结构类型的实例构造函数内分配*w*是指定一个自动实现的属性的属性访问*P*正在构造的实例上并*v*是隐藏的支持字段*P*，然后的明确赋值状态*v*后*expr*绝对分配。</span><span class="sxs-lookup"><span data-stu-id="19395-373">Otherwise, if the assignment occurs within the instance constructor of a struct type, if *w* is a property access designating an automatically implemented property *P* on the instance being constructed and *v* is the hidden backing field of *P*, then the definite assignment state of *v* after *expr* is definitely assigned.</span></span>
   * <span data-ttu-id="19395-374">否则为明确赋值国*v*后*expr*是明确的赋值状态的相同*v*后*expr_rhs*。</span><span class="sxs-lookup"><span data-stu-id="19395-374">Otherwise, the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_rhs*.</span></span>

#### <a name="-conditional-and-expressions"></a><span data-ttu-id="19395-375">& & (AND 条件) 表达式</span><span class="sxs-lookup"><span data-stu-id="19395-375">&& (conditional AND) expressions</span></span>

<span data-ttu-id="19395-376">表达式*expr*窗体的`expr_first && expr_second`:</span><span class="sxs-lookup"><span data-stu-id="19395-376">For an expression *expr* of the form `expr_first && expr_second`:</span></span>

*  <span data-ttu-id="19395-377">明确赋值国*v*之前*expr_first*是明确的赋值状态的相同*v*之前*expr*。</span><span class="sxs-lookup"><span data-stu-id="19395-377">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="19395-378">明确赋值国*v*之前*expr_second*如果明确赋值的状态*v*后*expr_first*是明确赋值或者"明确赋值后，则返回 true 的表达式"。</span><span class="sxs-lookup"><span data-stu-id="19395-378">The definite assignment state of *v* before *expr_second* is definitely assigned if the state of *v* after *expr_first* is either definitely assigned or "definitely assigned after true expression".</span></span> <span data-ttu-id="19395-379">否则，它就不是明确赋值的。</span><span class="sxs-lookup"><span data-stu-id="19395-379">Otherwise, it is not definitely assigned.</span></span>
*  <span data-ttu-id="19395-380">明确赋值国*v*后*expr*由：</span><span class="sxs-lookup"><span data-stu-id="19395-380">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="19395-381">如果*expr_first*是常量表达式的值`false`，然后明确赋值的状态*v*后*expr*等同于明确赋值状态*v*后*expr_first*。</span><span class="sxs-lookup"><span data-stu-id="19395-381">If *expr_first* is a constant expression with the value `false`, then the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
    * <span data-ttu-id="19395-382">否则为如果的状态*v*后*expr_first*未明确赋值，然后的状态*v*后*expr*明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-382">Otherwise, if the state of *v* after *expr_first* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="19395-383">否则为如果的状态*v*后*expr_second*明确赋值，和的状态*v*后*expr_first* "肯定是分配后 false 的表达式"，然后的状态*v*后*expr*明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-383">Otherwise, if the state of *v* after *expr_second* is definitely assigned, and the state of *v* after *expr_first* is "definitely assigned after false expression", then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="19395-384">否则为如果的状态*v*后*expr_second*明确赋值或"后，则返回 true 的表达式明确赋值"，然后的状态*v*后*expr* "肯定后被分配，则返回 true 的表达式"。</span><span class="sxs-lookup"><span data-stu-id="19395-384">Otherwise, if the state of *v* after *expr_second* is definitely assigned or "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="19395-385">否则为如果的状态*v*后*expr_first*为"false 表达式之后明确赋值"，和状态*v*后*expr_second*为"false 表达式之后明确赋值"，则状态*v*后*expr* "肯定后被分配 false 的表达式"。</span><span class="sxs-lookup"><span data-stu-id="19395-385">Otherwise, if the state of *v* after *expr_first* is "definitely assigned after false expression", and the state of *v* after *expr_second* is "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>
    * <span data-ttu-id="19395-386">否则为状态*v*后*expr*未明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-386">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

<span data-ttu-id="19395-387">在示例</span><span class="sxs-lookup"><span data-stu-id="19395-387">In the example</span></span>
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
<span data-ttu-id="19395-388">在变量`i`被视为一个嵌入的语句中明确赋值`if`语句但不是在其他。</span><span class="sxs-lookup"><span data-stu-id="19395-388">the variable `i` is considered definitely assigned in one of the embedded statements of an `if` statement but not in the other.</span></span> <span data-ttu-id="19395-389">在`if`方法中的语句`F`，该变量`i`明确赋值中第一条嵌入式语句由于表达式的执行`(i = y)`始终执行前执行此嵌入式语句。</span><span class="sxs-lookup"><span data-stu-id="19395-389">In the `if` statement in method `F`, the variable `i` is definitely assigned in the first embedded statement because execution of the expression `(i = y)` always precedes execution of this embedded statement.</span></span> <span data-ttu-id="19395-390">与之相反，变量`i`不明确赋值在第二个嵌入的语句中，由于`x >= 0`可能已测试为 false，从而在变量中`i`未分配。</span><span class="sxs-lookup"><span data-stu-id="19395-390">In contrast, the variable `i` is not definitely assigned in the second embedded statement, since `x >= 0` might have tested false, resulting in the variable `i` being unassigned.</span></span>

#### <a name="-conditional-or-expressions"></a><span data-ttu-id="19395-391">||（条件或） 表达式</span><span class="sxs-lookup"><span data-stu-id="19395-391">|| (conditional OR) expressions</span></span>

<span data-ttu-id="19395-392">表达式*expr*窗体的`expr_first || expr_second`:</span><span class="sxs-lookup"><span data-stu-id="19395-392">For an expression *expr* of the form `expr_first || expr_second`:</span></span>

*  <span data-ttu-id="19395-393">明确赋值国*v*之前*expr_first*是明确的赋值状态的相同*v*之前*expr*。</span><span class="sxs-lookup"><span data-stu-id="19395-393">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="19395-394">明确赋值国*v*之前*expr_second*如果明确赋值的状态*v*后*expr_first*是明确赋值或者"明确赋值后 false 的表达式"。</span><span class="sxs-lookup"><span data-stu-id="19395-394">The definite assignment state of *v* before *expr_second* is definitely assigned if the state of *v* after *expr_first* is either definitely assigned or "definitely assigned after false expression".</span></span> <span data-ttu-id="19395-395">否则，它就不是明确赋值的。</span><span class="sxs-lookup"><span data-stu-id="19395-395">Otherwise, it is not definitely assigned.</span></span>
*  <span data-ttu-id="19395-396">明确的赋值语句*v*后*expr*由：</span><span class="sxs-lookup"><span data-stu-id="19395-396">The definite assignment statement of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="19395-397">如果*expr_first*是常量表达式的值`true`，然后明确赋值的状态*v*后*expr*等同于明确赋值状态*v*后*expr_first*。</span><span class="sxs-lookup"><span data-stu-id="19395-397">If *expr_first* is a constant expression with the value `true`, then the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
    * <span data-ttu-id="19395-398">否则为如果的状态*v*后*expr_first*未明确赋值，然后的状态*v*后*expr*明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-398">Otherwise, if the state of *v* after *expr_first* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="19395-399">否则为如果的状态*v*后*expr_second*明确赋值，和的状态*v*后*expr_first* "肯定是分配的则返回 true 表达式之后"，然后的状态*v*后*expr*明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-399">Otherwise, if the state of *v* after *expr_second* is definitely assigned, and the state of *v* after *expr_first* is "definitely assigned after true expression", then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="19395-400">否则为如果的状态*v*后*expr_second*明确赋值或"false 表达式之后明确赋值"，然后的状态*v*后*expr* "肯定后被分配 false 的表达式"。</span><span class="sxs-lookup"><span data-stu-id="19395-400">Otherwise, if the state of *v* after *expr_second* is definitely assigned or "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>
    * <span data-ttu-id="19395-401">否则为如果的状态*v*后*expr_first*是"，则返回 true 表达式之后明确赋值"，和状态*v*后*expr_second*为"true 表达式之后明确赋值"，则状态*v*后*expr* "肯定后被分配，则返回 true 的表达式"。</span><span class="sxs-lookup"><span data-stu-id="19395-401">Otherwise, if the state of *v* after *expr_first* is "definitely assigned after true expression", and the state of *v* after *expr_second* is "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="19395-402">否则为状态*v*后*expr*未明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-402">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

<span data-ttu-id="19395-403">在示例</span><span class="sxs-lookup"><span data-stu-id="19395-403">In the example</span></span>
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
<span data-ttu-id="19395-404">在变量`i`被视为一个嵌入的语句中明确赋值`if`语句但不是在其他。</span><span class="sxs-lookup"><span data-stu-id="19395-404">the variable `i` is considered definitely assigned in one of the embedded statements of an `if` statement but not in the other.</span></span> <span data-ttu-id="19395-405">在`if`方法中的语句`G`，该变量`i`因为明确赋值在第二个嵌入的语句中表达式的执行`(i = y)`始终执行前执行此嵌入式语句。</span><span class="sxs-lookup"><span data-stu-id="19395-405">In the `if` statement in method `G`, the variable `i` is definitely assigned in the second embedded statement because execution of the expression `(i = y)` always precedes execution of this embedded statement.</span></span> <span data-ttu-id="19395-406">与之相反，变量`i`肯定中未分配的第一个嵌入语句，因为`x >= 0`可能已测试为 true，从而导致变量`i`未分配。</span><span class="sxs-lookup"><span data-stu-id="19395-406">In contrast, the variable `i` is not definitely assigned in the first embedded statement, since `x >= 0` might have tested true, resulting in the variable `i` being unassigned.</span></span>

#### <a name="-logical-negation-expressions"></a><span data-ttu-id="19395-407">!</span><span class="sxs-lookup"><span data-stu-id="19395-407">!</span></span> <span data-ttu-id="19395-408">（逻辑非运算） 表达式</span><span class="sxs-lookup"><span data-stu-id="19395-408">(logical negation) expressions</span></span>

<span data-ttu-id="19395-409">表达式*expr*窗体的`! expr_operand`:</span><span class="sxs-lookup"><span data-stu-id="19395-409">For an expression *expr* of the form `! expr_operand`:</span></span>

*  <span data-ttu-id="19395-410">明确赋值国*v*之前*expr_operand*是明确的赋值状态的相同*v*之前*expr*。</span><span class="sxs-lookup"><span data-stu-id="19395-410">The definite assignment state of *v* before *expr_operand* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="19395-411">明确赋值国*v*后*expr*由：</span><span class="sxs-lookup"><span data-stu-id="19395-411">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="19395-412">如果状态*v*后 \* expr_operand \* 未明确赋值，然后的状态*v*后*expr*明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-412">If the state of *v* after \*expr_operand \*is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="19395-413">如果状态*v*后 \* expr_operand \* 不明确赋值，然后的状态*v*后*expr*未明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-413">If the state of *v* after \*expr_operand \*is not definitely assigned, then the state of *v* after *expr* is not definitely assigned.</span></span>
    * <span data-ttu-id="19395-414">如果状态*v*后 \* expr_operand \* 为"false 表达式之后明确赋值"，则状态*v*后*expr* "明确赋值后 true表达式"。</span><span class="sxs-lookup"><span data-stu-id="19395-414">If the state of *v* after \*expr_operand \*is "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="19395-415">如果状态*v*后 \* expr_operand \* 为"true 表达式之后明确赋值"，则状态*v*后*expr* "明确赋值后 false表达式"。</span><span class="sxs-lookup"><span data-stu-id="19395-415">If the state of *v* after \*expr_operand \*is "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>

#### <a name="-null-coalescing-expressions"></a><span data-ttu-id="19395-416">??</span><span class="sxs-lookup"><span data-stu-id="19395-416">??</span></span> <span data-ttu-id="19395-417">（null 合并） 表达式</span><span class="sxs-lookup"><span data-stu-id="19395-417">(null coalescing) expressions</span></span>

<span data-ttu-id="19395-418">表达式*expr*窗体的`expr_first ?? expr_second`:</span><span class="sxs-lookup"><span data-stu-id="19395-418">For an expression *expr* of the form `expr_first ?? expr_second`:</span></span>

*  <span data-ttu-id="19395-419">明确赋值国*v*之前*expr_first*是明确的赋值状态的相同*v*之前*expr*。</span><span class="sxs-lookup"><span data-stu-id="19395-419">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="19395-420">明确赋值国*v*之前*expr_second*是明确的赋值状态的相同*v*后*expr_first*。</span><span class="sxs-lookup"><span data-stu-id="19395-420">The definite assignment state of *v* before *expr_second* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
*  <span data-ttu-id="19395-421">明确的赋值语句*v*后*expr*由：</span><span class="sxs-lookup"><span data-stu-id="19395-421">The definite assignment statement of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="19395-422">如果*expr_first*是常量表达式 ([常量表达式](expressions.md#constant-expressions)) 具有值为 null，则状态*v*后*expr*相同状态*v*后*expr_second*。</span><span class="sxs-lookup"><span data-stu-id="19395-422">If *expr_first* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value null, then the the state of *v* after *expr* is the same as the state of *v* after *expr_second*.</span></span>
*  <span data-ttu-id="19395-423">否则为状态*v*后*expr*是明确的赋值状态的相同*v*后*expr_first*。</span><span class="sxs-lookup"><span data-stu-id="19395-423">Otherwise, the state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>

#### <a name="-conditional-expressions"></a><span data-ttu-id="19395-424">？: （条件） 表达式</span><span class="sxs-lookup"><span data-stu-id="19395-424">?: (conditional) expressions</span></span>

<span data-ttu-id="19395-425">表达式*expr*窗体的`expr_cond ? expr_true : expr_false`:</span><span class="sxs-lookup"><span data-stu-id="19395-425">For an expression *expr* of the form `expr_cond ? expr_true : expr_false`:</span></span>

*  <span data-ttu-id="19395-426">明确赋值国*v*之前*expr_cond*的状态相同*v*之前*expr*。</span><span class="sxs-lookup"><span data-stu-id="19395-426">The definite assignment state of *v* before *expr_cond* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="19395-427">明确赋值国*v*之前*expr_true*明确赋值的当且仅当以下之一保存：</span><span class="sxs-lookup"><span data-stu-id="19395-427">The definite assignment state of *v* before *expr_true* is definitely assigned if and only if one of the following holds:</span></span>
    * <span data-ttu-id="19395-428">*expr_cond*是具有值的常量表达式 `false`</span><span class="sxs-lookup"><span data-stu-id="19395-428">*expr_cond* is a constant expression with the value `false`</span></span>
    * <span data-ttu-id="19395-429">状态*v*后*expr_cond*明确赋值或"明确赋值后，则返回 true 的表达式"。</span><span class="sxs-lookup"><span data-stu-id="19395-429">the state of *v* after *expr_cond* is definitely assigned or "definitely assigned after true expression".</span></span>
*  <span data-ttu-id="19395-430">明确赋值国*v*之前*expr_false*明确赋值的当且仅当以下之一保存：</span><span class="sxs-lookup"><span data-stu-id="19395-430">The definite assignment state of *v* before *expr_false* is definitely assigned if and only if one of the following holds:</span></span>
    * <span data-ttu-id="19395-431">*expr_cond*是具有值的常量表达式 `true`</span><span class="sxs-lookup"><span data-stu-id="19395-431">*expr_cond* is a constant expression with the value `true`</span></span>
*  <span data-ttu-id="19395-432">状态*v*后*expr_cond*明确赋值或"明确赋值后 false 的表达式"。</span><span class="sxs-lookup"><span data-stu-id="19395-432">the state of *v* after *expr_cond* is definitely assigned or "definitely assigned after false expression".</span></span>
*  <span data-ttu-id="19395-433">明确赋值国*v*后*expr*由：</span><span class="sxs-lookup"><span data-stu-id="19395-433">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="19395-434">如果*expr_cond*是常量表达式 ([常量表达式](expressions.md#constant-expressions)) 值`true`然后的状态*v*后*expr*状态相同*v*后*expr_true*。</span><span class="sxs-lookup"><span data-stu-id="19395-434">If *expr_cond* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value `true` then the state of *v* after *expr* is the same as the state of *v* after *expr_true*.</span></span>
    * <span data-ttu-id="19395-435">否则为如果*expr_cond*是常量表达式 ([常量表达式](expressions.md#constant-expressions)) 值`false`然后的状态*v*后*expr*的状态相同*v*后*expr_false*。</span><span class="sxs-lookup"><span data-stu-id="19395-435">Otherwise, if *expr_cond* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value `false` then the state of *v* after *expr* is the same as the state of *v* after *expr_false*.</span></span>
    * <span data-ttu-id="19395-436">否则为如果的状态*v*后*expr_true*明确赋值和状态*v*后*expr_false*绝对分配，则状态*v*后*expr*明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-436">Otherwise, if the state of *v* after *expr_true* is definitely assigned and the state of *v* after *expr_false* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="19395-437">否则为状态*v*后*expr*未明确赋值。</span><span class="sxs-lookup"><span data-stu-id="19395-437">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

#### <a name="anonymous-functions"></a><span data-ttu-id="19395-438">匿名函数</span><span class="sxs-lookup"><span data-stu-id="19395-438">Anonymous functions</span></span>

<span data-ttu-id="19395-439">有关*lambda_expression*或*anonymous_method_expression* *expr*正文 (任一*块*或*表达式*)*正文*:</span><span class="sxs-lookup"><span data-stu-id="19395-439">For a *lambda_expression* or *anonymous_method_expression* *expr* with a body (either *block* or *expression*) *body*:</span></span>

*  <span data-ttu-id="19395-440">外部变量的明确赋值状态*v*之前*正文*的状态相同*v*之前*expr*。</span><span class="sxs-lookup"><span data-stu-id="19395-440">The definite assignment state of an outer variable *v* before *body* is the same as the state of *v* before *expr*.</span></span> <span data-ttu-id="19395-441">也就是说的外部变量明确赋值状态继承自的匿名函数的上下文。</span><span class="sxs-lookup"><span data-stu-id="19395-441">That is, definite assignment state of outer variables is inherited from the context of the anonymous function.</span></span>
*  <span data-ttu-id="19395-442">外部变量的明确赋值状态*v*后*expr*的状态相同*v*之前*expr*。</span><span class="sxs-lookup"><span data-stu-id="19395-442">The definite assignment state of an outer variable *v* after *expr* is the same as the state of *v* before *expr*.</span></span>

<span data-ttu-id="19395-443">该示例</span><span class="sxs-lookup"><span data-stu-id="19395-443">The example</span></span>
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
<span data-ttu-id="19395-444">生成编译时错误，因为`max`未明确赋值的匿名函数的声明位置。</span><span class="sxs-lookup"><span data-stu-id="19395-444">generates a compile-time error since `max` is not definitely assigned where the anonymous function is declared.</span></span> <span data-ttu-id="19395-445">该示例</span><span class="sxs-lookup"><span data-stu-id="19395-445">The example</span></span>
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
<span data-ttu-id="19395-446">分配到以来还会生成编译时错误`n`匿名函数中产生任何影响明确分配状态的`n`匿名函数的外部。</span><span class="sxs-lookup"><span data-stu-id="19395-446">also generates a compile-time error since the assignment to `n` in the anonymous function has no affect on the definite assignment state of `n` outside the anonymous function.</span></span>

## <a name="variable-references"></a><span data-ttu-id="19395-447">变量引用</span><span class="sxs-lookup"><span data-stu-id="19395-447">Variable references</span></span>

<span data-ttu-id="19395-448">一个*variable_reference*是*表达式*的分类为变量。</span><span class="sxs-lookup"><span data-stu-id="19395-448">A *variable_reference* is an *expression* that is classified as a variable.</span></span> <span data-ttu-id="19395-449">一个*variable_reference*表示来提取当前值并存储一个新值可以访问的存储位置。</span><span class="sxs-lookup"><span data-stu-id="19395-449">A *variable_reference* denotes a storage location that can be accessed both to fetch the current value and to store a new value.</span></span>

```antlr
variable_reference
    : expression
    ;
```

<span data-ttu-id="19395-450">在 C 和C++、 一个*variable_reference*称为*左值*。</span><span class="sxs-lookup"><span data-stu-id="19395-450">In C and C++, a *variable_reference* is known as an *lvalue*.</span></span>

## <a name="atomicity-of-variable-references"></a><span data-ttu-id="19395-451">变量引用的原子性</span><span class="sxs-lookup"><span data-stu-id="19395-451">Atomicity of variable references</span></span>

<span data-ttu-id="19395-452">是原子操作读取和写入以下数据类型： `bool`， `char`， `byte`， `sbyte`， `short`， `ushort`， `uint`， `int`， `float`，和引用类型。</span><span class="sxs-lookup"><span data-stu-id="19395-452">Reads and writes of the following data types are atomic: `bool`, `char`, `byte`, `sbyte`, `short`, `ushort`, `uint`, `int`, `float`, and reference types.</span></span> <span data-ttu-id="19395-453">此外，读取和写入与前面的列表中的基础类型的枚举类型也是原子。</span><span class="sxs-lookup"><span data-stu-id="19395-453">In addition, reads and writes of enum types with an underlying type in the previous list are also atomic.</span></span> <span data-ttu-id="19395-454">读取和写入的其他类型，包括`long`， `ulong`， `double`，和`decimal`，以及用户定义类型，不保证是原子性。</span><span class="sxs-lookup"><span data-stu-id="19395-454">Reads and writes of other types, including `long`, `ulong`, `double`, and `decimal`, as well as user-defined types, are not guaranteed to be atomic.</span></span> <span data-ttu-id="19395-455">除了为该目的而设计的库函数，则无法保证的原子读取-修改-写入，例如在递增或递减的情况下。</span><span class="sxs-lookup"><span data-stu-id="19395-455">Aside from the library functions designed for that purpose, there is no guarantee of atomic read-modify-write, such as in the case of increment or decrement.</span></span>

