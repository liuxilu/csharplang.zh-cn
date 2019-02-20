# <a name="expressions"></a><span data-ttu-id="08161-101">表达式</span><span class="sxs-lookup"><span data-stu-id="08161-101">Expressions</span></span>

<span data-ttu-id="08161-102">表达式是一系列运算符和操作数。</span><span class="sxs-lookup"><span data-stu-id="08161-102">An expression is a sequence of operators and operands.</span></span> <span data-ttu-id="08161-103">本章定义的语法，操作数和运算符的求值和表达式的含义的顺序。</span><span class="sxs-lookup"><span data-stu-id="08161-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

## <a name="expression-classifications"></a><span data-ttu-id="08161-104">表达式分类</span><span class="sxs-lookup"><span data-stu-id="08161-104">Expression classifications</span></span>

<span data-ttu-id="08161-105">表达式分类为以下类别之一：</span><span class="sxs-lookup"><span data-stu-id="08161-105">An expression is classified as one of the following:</span></span>

*  <span data-ttu-id="08161-106">一个值。</span><span class="sxs-lookup"><span data-stu-id="08161-106">A value.</span></span> <span data-ttu-id="08161-107">每个值都有关联的类型。</span><span class="sxs-lookup"><span data-stu-id="08161-107">Every value has an associated type.</span></span>
*  <span data-ttu-id="08161-108">一个变量。</span><span class="sxs-lookup"><span data-stu-id="08161-108">A variable.</span></span> <span data-ttu-id="08161-109">每个变量具有关联的类型，即该变量的声明类型。</span><span class="sxs-lookup"><span data-stu-id="08161-109">Every variable has an associated type, namely the declared type of the variable.</span></span>
*  <span data-ttu-id="08161-110">一个命名空间。</span><span class="sxs-lookup"><span data-stu-id="08161-110">A namespace.</span></span> <span data-ttu-id="08161-111">具有此分类的表达式只显示为的左侧*member_access* ([成员访问](expressions.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="08161-111">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="08161-112">在任何其他上下文中，分类为一个命名空间的表达式将导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>
*  <span data-ttu-id="08161-113">一种类型。</span><span class="sxs-lookup"><span data-stu-id="08161-113">A type.</span></span> <span data-ttu-id="08161-114">具有此分类的表达式只显示为的左侧*member_access* ([成员访问](expressions.md#member-access))，或为有关操作数`as`运算符 ([As 运算符](expressions.md#the-as-operator))，则`is`运算符 ([是运算符](expressions.md#the-is-operator))，或`typeof`运算符 ([typeof 运算符](expressions.md#the-typeof-operator))。</span><span class="sxs-lookup"><span data-stu-id="08161-114">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)), or as an operand for the `as` operator ([The as operator](expressions.md#the-as-operator)), the `is` operator ([The is operator](expressions.md#the-is-operator)), or the `typeof` operator ([The typeof operator](expressions.md#the-typeof-operator)).</span></span> <span data-ttu-id="08161-115">在任何其他上下文中，分类为一种类型的表达式将导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>
*  <span data-ttu-id="08161-116">一组重载方法，从而得到从成员查找方法组 ([成员查找](expressions.md#member-lookup))。</span><span class="sxs-lookup"><span data-stu-id="08161-116">A method group, which is a set of overloaded methods resulting from a member lookup ([Member lookup](expressions.md#member-lookup)).</span></span> <span data-ttu-id="08161-117">方法组可能有一个关联的实例表达式和关联的类型参数列表。</span><span class="sxs-lookup"><span data-stu-id="08161-117">A method group may have an associated instance expression and an associated type argument list.</span></span> <span data-ttu-id="08161-118">调用实例方法时，实例表达式的计算结果将成为所表示的实例`this`([此访问权限](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="08161-118">When an instance method is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="08161-119">方法组允许在*invocation_expression* ([调用表达式](expressions.md#invocation-expressions))、 一个*delegate_creation_expression* ([委托创建表达式](expressions.md#delegate-creation-expressions)) 的左侧，以及为运算符，且可以隐式转换为兼容的委托类型 ([方法组转换](conversions.md#method-group-conversions))。</span><span class="sxs-lookup"><span data-stu-id="08161-119">A method group is permitted in an *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)) , a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) and as the left hand side of an is operator, and can be implicitly converted to a compatible delegate type ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="08161-120">在任何其他上下文中，分类为方法组的表达式将导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-120">In any other context, an expression classified as a method group causes a compile-time error.</span></span>
*  <span data-ttu-id="08161-121">Null 文本。</span><span class="sxs-lookup"><span data-stu-id="08161-121">A null literal.</span></span> <span data-ttu-id="08161-122">具有此分类的表达式可以隐式转换为引用类型或可以为 null 的类型。</span><span class="sxs-lookup"><span data-stu-id="08161-122">An expression with this classification can be implicitly converted to a reference type or nullable type.</span></span>
*  <span data-ttu-id="08161-123">匿名函数。</span><span class="sxs-lookup"><span data-stu-id="08161-123">An anonymous function.</span></span> <span data-ttu-id="08161-124">具有此分类的表达式可以隐式转换为兼容的委托类型或表达式树类型。</span><span class="sxs-lookup"><span data-stu-id="08161-124">An expression with this classification can be implicitly converted to a compatible delegate type or expression tree type.</span></span>
*  <span data-ttu-id="08161-125">属性访问。</span><span class="sxs-lookup"><span data-stu-id="08161-125">A property access.</span></span> <span data-ttu-id="08161-126">每个属性访问具有关联的类型，即属性的类型。</span><span class="sxs-lookup"><span data-stu-id="08161-126">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="08161-127">此外，属性访问可以有一个关联的实例表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-127">Furthermore, a property access may have an associated instance expression.</span></span> <span data-ttu-id="08161-128">当一个访问器 (`get`或`set`块) 的实例调用属性访问、 实例表达式的计算结果将成为所表示的实例`this`([此访问权限](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="08161-128">When an accessor (the `get` or `set` block) of an instance property access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="08161-129">事件的访问。</span><span class="sxs-lookup"><span data-stu-id="08161-129">An event access.</span></span> <span data-ttu-id="08161-130">每个事件访问具有关联的类型，即该事件的类型。</span><span class="sxs-lookup"><span data-stu-id="08161-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="08161-131">此外，事件访问可以有一个关联的实例表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-131">Furthermore, an event access may have an associated instance expression.</span></span> <span data-ttu-id="08161-132">事件访问可能显示为左操作数`+=`并`-=`运算符 ([事件分配](expressions.md#event-assignment))。</span><span class="sxs-lookup"><span data-stu-id="08161-132">An event access may appear as the left hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="08161-133">在任何其他上下文中，分类为事件访问的表达式将导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>
*  <span data-ttu-id="08161-134">索引器访问。</span><span class="sxs-lookup"><span data-stu-id="08161-134">An indexer access.</span></span> <span data-ttu-id="08161-135">每个索引器访问具有关联的类型，即元素的索引器的类型。</span><span class="sxs-lookup"><span data-stu-id="08161-135">Every indexer access has an associated type, namely the element type of the indexer.</span></span> <span data-ttu-id="08161-136">此外，索引器访问有一个关联的实例表达式和关联的参数列表。</span><span class="sxs-lookup"><span data-stu-id="08161-136">Furthermore, an indexer access has an associated instance expression and an associated argument list.</span></span> <span data-ttu-id="08161-137">取值函数时 (`get`或`set`块) 的索引器调用访问、 实例表达式的计算结果将成为所表示的实例`this`([此访问权限](expressions.md#this-access))，和的结果计算参数列表将变为调用的参数列表。</span><span class="sxs-lookup"><span data-stu-id="08161-137">When an accessor (the `get` or `set` block) of an indexer access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)), and the result of evaluating the argument list becomes the parameter list of the invocation.</span></span>
*  <span data-ttu-id="08161-138">执行任何操作。</span><span class="sxs-lookup"><span data-stu-id="08161-138">Nothing.</span></span> <span data-ttu-id="08161-139">发生这种情况的一种方法的调用的返回类型与表达式时`void`。</span><span class="sxs-lookup"><span data-stu-id="08161-139">This occurs when the expression is an invocation of a method with a return type of `void`.</span></span> <span data-ttu-id="08161-140">归类为执行任何操作才有效的上下文中的表达式*statement_expression* ([表达式语句](statements.md#expression-statements))。</span><span class="sxs-lookup"><span data-stu-id="08161-140">An expression classified as nothing is only valid in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

<span data-ttu-id="08161-141">命名空间、 类型、 方法组或事件访问，永远不会是一个表达式的最终结果。</span><span class="sxs-lookup"><span data-stu-id="08161-141">The final result of an expression is never a namespace, type, method group, or event access.</span></span> <span data-ttu-id="08161-142">相反，如上文所述，这些类别的表达式是只能在某些上下文中使用的中间构造。</span><span class="sxs-lookup"><span data-stu-id="08161-142">Rather, as noted above, these categories of expressions are intermediate constructs that are only permitted in certain contexts.</span></span>

<span data-ttu-id="08161-143">属性访问或索引器访问始终重新分类为一个值，通过执行调用*get 访问器*或*set 访问器*。</span><span class="sxs-lookup"><span data-stu-id="08161-143">A property access or indexer access is always reclassified as a value by performing an invocation of the *get accessor* or the *set accessor*.</span></span> <span data-ttu-id="08161-144">由属性或索引器访问的上下文确定特定的访问器：如果访问为赋值的目标*set 访问器*调用来分配新值 ([简单赋值](expressions.md#simple-assignment))。</span><span class="sxs-lookup"><span data-stu-id="08161-144">The particular accessor is determined by the context of the property or indexer access: If the access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="08161-145">否则为*get 访问器*调用来获取当前值 ([表达式的值](expressions.md#values-of-expressions))。</span><span class="sxs-lookup"><span data-stu-id="08161-145">Otherwise, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="values-of-expressions"></a><span data-ttu-id="08161-146">表达式的值</span><span class="sxs-lookup"><span data-stu-id="08161-146">Values of expressions</span></span>

<span data-ttu-id="08161-147">大多数构造都表达式最终要求表达式指示***值***。</span><span class="sxs-lookup"><span data-stu-id="08161-147">Most of the constructs that involve an expression ultimately require the expression to denote a ***value***.</span></span> <span data-ttu-id="08161-148">在这种情况下，如果实际表达式指示一个命名空间、 类型、 方法组，或执行任何操作，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-148">In such cases, if the actual expression denotes a namespace, a type, a method group, or nothing, a compile-time error occurs.</span></span> <span data-ttu-id="08161-149">但是，如果表达式指示属性访问、 索引器访问或变量，将隐式替换的属性、 索引器或变量的值：</span><span class="sxs-lookup"><span data-stu-id="08161-149">However, if the expression denotes a property access, an indexer access, or a variable, the value of the property, indexer, or variable is implicitly substituted:</span></span>

*  <span data-ttu-id="08161-150">变量的值就是当前存储在由该变量的存储位置中的值。</span><span class="sxs-lookup"><span data-stu-id="08161-150">The value of a variable is simply the value currently stored in the storage location identified by the variable.</span></span> <span data-ttu-id="08161-151">必须考虑一个变量明确赋值 ([明确赋值](variables.md#definite-assignment)) 可以获取其值，或否则会发生编译时错误之前。</span><span class="sxs-lookup"><span data-stu-id="08161-151">A variable must be considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained, or otherwise a compile-time error occurs.</span></span>
*  <span data-ttu-id="08161-152">属性访问表达式的值通过调用*get 访问器*的属性。</span><span class="sxs-lookup"><span data-stu-id="08161-152">The value of a property access expression is obtained by invoking the *get accessor* of the property.</span></span> <span data-ttu-id="08161-153">如果该属性不具有*get 访问器*，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-153">If the property has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="08161-154">否则为函数成员调用 ([进行编译时检查的动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) 执行，并调用的结果将成为属性访问表达式的值。</span><span class="sxs-lookup"><span data-stu-id="08161-154">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed, and the result of the invocation becomes the value of the property access expression.</span></span>
*  <span data-ttu-id="08161-155">索引器访问表达式的值通过调用*get 访问器*的索引器。</span><span class="sxs-lookup"><span data-stu-id="08161-155">The value of an indexer access expression is obtained by invoking the *get accessor* of the indexer.</span></span> <span data-ttu-id="08161-156">如果索引器没有*get 访问器*，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-156">If the indexer has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="08161-157">否则为函数成员调用 ([进行编译时检查的动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) 执行具有参数列表与索引器访问表达式，并调用的结果将成为值索引器访问表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-157">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed with the argument list associated with the indexer access expression, and the result of the invocation becomes the value of the indexer access expression.</span></span>

## <a name="static-and-dynamic-binding"></a><span data-ttu-id="08161-158">静态和动态绑定</span><span class="sxs-lookup"><span data-stu-id="08161-158">Static and Dynamic Binding</span></span>

<span data-ttu-id="08161-159">确定基于类型或值构成的表达式 （自变量，操作数，接收方） 的操作的含义的过程通常称为***绑定***。</span><span class="sxs-lookup"><span data-stu-id="08161-159">The process of determining the meaning of an operation based on the type or value of constituent expressions (arguments, operands, receivers) is often referred to as ***binding***.</span></span> <span data-ttu-id="08161-160">例如根据接收方和自变量的类型确定方法调用的含义。</span><span class="sxs-lookup"><span data-stu-id="08161-160">For instance the meaning of a method call is determined based on the type of the receiver and arguments.</span></span> <span data-ttu-id="08161-161">运算符的含义是根据其操作数的类型确定的。</span><span class="sxs-lookup"><span data-stu-id="08161-161">The meaning of an operator is determined based on the type of its operands.</span></span>

<span data-ttu-id="08161-162">在 C# 中的操作的含义通常在编译时根据确定其构成表达式的编译时类型。</span><span class="sxs-lookup"><span data-stu-id="08161-162">In C# the meaning of an operation is usually determined at compile-time, based on the compile-time type of its constituent expressions.</span></span> <span data-ttu-id="08161-163">同样，如果表达式包含错误，错误将检测到并将由编译器报告。</span><span class="sxs-lookup"><span data-stu-id="08161-163">Likewise, if an expression contains an error, the error is detected and reported by the compiler.</span></span> <span data-ttu-id="08161-164">这种方法称为***静态绑定***。</span><span class="sxs-lookup"><span data-stu-id="08161-164">This approach is known as ***static binding***.</span></span>

<span data-ttu-id="08161-165">但是，如果表达式是动态表达式 (即具有类型`dynamic`) 这表示，它参与任何绑定应基于其运行时类型 （即它在运行时表示的对象的实际类型） 而不是它在具有的类型编译时。</span><span class="sxs-lookup"><span data-stu-id="08161-165">However, if an expression is a dynamic expression (i.e. has the type `dynamic`) this indicates that any binding that it participates in should be based on its run-time type (i.e. the actual type of the object it denotes at run-time) rather than the type it has at compile-time.</span></span> <span data-ttu-id="08161-166">此类操作的绑定因此推迟到其中的操作是在程序运行期间执行的时间。</span><span class="sxs-lookup"><span data-stu-id="08161-166">The binding of such an operation is therefore deferred until the time where the operation is to be executed during the running of the program.</span></span> <span data-ttu-id="08161-167">这被称为***动态绑定***。</span><span class="sxs-lookup"><span data-stu-id="08161-167">This is referred to as ***dynamic binding***.</span></span>

<span data-ttu-id="08161-168">当动态绑定操作时，由编译器执行很少或没有检查。</span><span class="sxs-lookup"><span data-stu-id="08161-168">When an operation is dynamically bound, little or no checking is performed by the compiler.</span></span> <span data-ttu-id="08161-169">改为如果运行时绑定失败，错误报告为在运行时异常。</span><span class="sxs-lookup"><span data-stu-id="08161-169">Instead if the run-time binding fails, errors are reported as exceptions at run-time.</span></span>

<span data-ttu-id="08161-170">根据绑定是在 C# 中的以下操作：</span><span class="sxs-lookup"><span data-stu-id="08161-170">The following operations in C# are subject to binding:</span></span>

*  <span data-ttu-id="08161-171">成员访问： `e.M`</span><span class="sxs-lookup"><span data-stu-id="08161-171">Member access: `e.M`</span></span>
*  <span data-ttu-id="08161-172">方法调用： `e.M(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="08161-172">Method invocation: `e.M(e1, ..., eN)`</span></span>
*  <span data-ttu-id="08161-173">委托调用：`e(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="08161-173">Delegate invocation:`e(e1, ..., eN)`</span></span>
*  <span data-ttu-id="08161-174">元素的访问权限： `e[e1, ..., eN]`</span><span class="sxs-lookup"><span data-stu-id="08161-174">Element access: `e[e1, ..., eN]`</span></span>
*  <span data-ttu-id="08161-175">创建对象： `new C(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="08161-175">Object creation: `new C(e1, ..., eN)`</span></span>
*  <span data-ttu-id="08161-176">重载一元运算符： `+`， `-`， `!`， `~`， `++`， `--`， `true`， `false`</span><span class="sxs-lookup"><span data-stu-id="08161-176">Overloaded unary operators: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span></span>
*  <span data-ttu-id="08161-177">重载二元运算符： `+`， `-`， `*`， `/`， `%`， `&`， `&&`， `|`， `||`， `??`， `^`， `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span><span class="sxs-lookup"><span data-stu-id="08161-177">Overloaded binary operators: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span></span>
*  <span data-ttu-id="08161-178">赋值运算符： `=`， `+=`， `-=`， `*=`， `/=`， `%=`， `&=`， `|=`， `^=`， `<<=`， `>>=`</span><span class="sxs-lookup"><span data-stu-id="08161-178">Assignment operators: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span></span>
*  <span data-ttu-id="08161-179">隐式和显式转换</span><span class="sxs-lookup"><span data-stu-id="08161-179">Implicit and explicit conversions</span></span>

<span data-ttu-id="08161-180">当不涉及任何动态表达式时，C# 默认为静态绑定，这意味着，在选择过程中使用构成表达式的编译时类型。</span><span class="sxs-lookup"><span data-stu-id="08161-180">When no dynamic expressions are involved, C# defaults to static binding, which means that the compile-time types of constituent expressions are used in the selection process.</span></span> <span data-ttu-id="08161-181">但是，在上面列出的操作构成的表达式之一时动态表达式，该操作改为动态绑定。</span><span class="sxs-lookup"><span data-stu-id="08161-181">However, when one of the constituent expressions in the operations listed above is a dynamic expression, the operation is instead dynamically bound.</span></span>

### <a name="binding-time"></a><span data-ttu-id="08161-182">绑定时间</span><span class="sxs-lookup"><span data-stu-id="08161-182">Binding-time</span></span>

<span data-ttu-id="08161-183">静态绑定都将在编译时进行而动态绑定发生在运行时。</span><span class="sxs-lookup"><span data-stu-id="08161-183">Static binding takes place at compile-time, whereas dynamic binding takes place at run-time.</span></span> <span data-ttu-id="08161-184">在以下部分中，术语***绑定时***指编译时或运行时间，具体取决于绑定发生。</span><span class="sxs-lookup"><span data-stu-id="08161-184">In the following sections, the term ***binding-time*** refers to either compile-time or run-time, depending on when the binding takes place.</span></span>

<span data-ttu-id="08161-185">下面的示例说明了概念和绑定时的静态和动态绑定：</span><span class="sxs-lookup"><span data-stu-id="08161-185">The following example illustrates the notions of static and dynamic binding and of binding-time:</span></span>
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

<span data-ttu-id="08161-186">前两个调用都以静态方式绑定： 的重载`Console.WriteLine`选择基于其自变量的编译时类型。</span><span class="sxs-lookup"><span data-stu-id="08161-186">The first two calls are statically bound: the overload of `Console.WriteLine` is picked based on the compile-time type of their argument.</span></span> <span data-ttu-id="08161-187">因此，绑定时是编译时。</span><span class="sxs-lookup"><span data-stu-id="08161-187">Thus, the binding-time is compile-time.</span></span>

<span data-ttu-id="08161-188">第三个调用动态绑定： 的重载`Console.WriteLine`选择基于其参数的运行时类型。</span><span class="sxs-lookup"><span data-stu-id="08161-188">The third call is dynamically bound: the overload of `Console.WriteLine` is picked based on the run-time type of its argument.</span></span> <span data-ttu-id="08161-189">这是因为自变量是动态表达式--它的编译时类型是`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="08161-189">This happens because the argument is a dynamic expression -- its compile-time type is `dynamic`.</span></span> <span data-ttu-id="08161-190">因此，第三个调用绑定时间是运行时。</span><span class="sxs-lookup"><span data-stu-id="08161-190">Thus, the binding-time for the third call is run-time.</span></span>

### <a name="dynamic-binding"></a><span data-ttu-id="08161-191">动态绑定</span><span class="sxs-lookup"><span data-stu-id="08161-191">Dynamic binding</span></span>

<span data-ttu-id="08161-192">动态绑定的目的是允许 C# 程序可以与之交互***动态对象***，即不遵循 C# 的一般规则的对象类型系统。</span><span class="sxs-lookup"><span data-stu-id="08161-192">The purpose of dynamic binding is to allow C# programs to interact with ***dynamic objects***, i.e. objects that do not follow the normal rules of the C# type system.</span></span> <span data-ttu-id="08161-193">动态对象可能与不同类型系统，其他编程语言中的对象，或者它们可能是以编程方式安装程序以实现自己的绑定语义表示不同的操作的对象。</span><span class="sxs-lookup"><span data-stu-id="08161-193">Dynamic objects may be objects from other programming languages with different types systems, or they may be objects that are programmatically setup to implement their own binding semantics for different operations.</span></span>

<span data-ttu-id="08161-194">依据一个动态对象实现其自己的语义的机制是定义的实现。</span><span class="sxs-lookup"><span data-stu-id="08161-194">The mechanism by which a dynamic object implements its own semantics is implementation defined.</span></span> <span data-ttu-id="08161-195">动态对象，将信号发送到 C# 运行时它们具有特殊的语义由实现给定的接口-再次实现定义。</span><span class="sxs-lookup"><span data-stu-id="08161-195">A given interface -- again implementation defined -- is implemented by dynamic objects to signal to the C# run-time that they have special semantics.</span></span> <span data-ttu-id="08161-196">因此，动态绑定动态对象上的操作，只要它们自己的绑定语义，而不是 C# 中本文档中，指定的接管。</span><span class="sxs-lookup"><span data-stu-id="08161-196">Thus, whenever operations on a dynamic object are dynamically bound, their own binding semantics, rather than those of C# as specified in this document, take over.</span></span>

<span data-ttu-id="08161-197">虽然动态绑定的目的是允许使用动态对象互操作，C# 允许动态绑定上的所有对象，无论它们是动态的还是不。</span><span class="sxs-lookup"><span data-stu-id="08161-197">While the purpose of dynamic binding is to allow interoperation with dynamic objects, C# allows dynamic binding on all objects, whether they are dynamic or not.</span></span> <span data-ttu-id="08161-198">这可以更顺畅地集成的动态对象，对其操作的结果可能不是本身是动态对象，但仍属于未知的程序员在编译时类型。</span><span class="sxs-lookup"><span data-stu-id="08161-198">This allows for a smoother integration of dynamic objects, as the results of operations on them may not themselves be dynamic objects, but are still of a type unknown to the programmer at compile-time.</span></span> <span data-ttu-id="08161-199">此外动态绑定可以帮助消除易出错的基于反射的代码，即使不不涉及任何对象的动态对象。</span><span class="sxs-lookup"><span data-stu-id="08161-199">Also dynamic binding can help eliminate error-prone reflection-based code even when no objects involved are dynamic objects.</span></span>

<span data-ttu-id="08161-200">以下部分介绍每个构造在语言中完全动态绑定应用时，内容编译时检查-如果应用任何-，和何种编译时结果和表达式分类为。</span><span class="sxs-lookup"><span data-stu-id="08161-200">The following sections describe for each construct in the language exactly when dynamic binding is applied, what compile time checking -- if any -- is applied, and what the compile-time result and expression classification is.</span></span>

### <a name="types-of-constituent-expressions"></a><span data-ttu-id="08161-201">构成表达式的类型</span><span class="sxs-lookup"><span data-stu-id="08161-201">Types of constituent expressions</span></span>

<span data-ttu-id="08161-202">当以静态方式绑定操作时，构成的表达式 （例如接收方、 参数、 索引或操作数） 的类型始终被视为可该表达式的编译时类型。</span><span class="sxs-lookup"><span data-stu-id="08161-202">When an operation is statically bound, the type of a constituent expression (e.g. a receiver, an argument, an index or an operand) is always considered to be the compile-time type of that expression.</span></span>

<span data-ttu-id="08161-203">当动态绑定操作时，具体取决于构成表达式的编译时类型不同的方式确定构成表达式的类型：</span><span class="sxs-lookup"><span data-stu-id="08161-203">When an operation is dynamically bound, the type of a constituent expression is determined in different ways depending on the compile-time type of the constituent expression:</span></span>

*  <span data-ttu-id="08161-204">编译时类型的构成表达式`dynamic`被视为具有表达式计算结果为在运行时的实际值的类型</span><span class="sxs-lookup"><span data-stu-id="08161-204">A constituent expression of compile-time type `dynamic` is considered to have the type of the actual value that the expression evaluates to at runtime</span></span>
*  <span data-ttu-id="08161-205">编译时类型为类型参数构成表达式将被视为已在运行时绑定到类型参数的类型</span><span class="sxs-lookup"><span data-stu-id="08161-205">A constituent expression whose compile-time type is a type parameter is considered to have the type which the type parameter is bound to at runtime</span></span>
*  <span data-ttu-id="08161-206">否则构成的表达式被视为具有其编译时类型。</span><span class="sxs-lookup"><span data-stu-id="08161-206">Otherwise the constituent expression is considered to have its compile-time type.</span></span>

## <a name="operators"></a><span data-ttu-id="08161-207">运算符</span><span class="sxs-lookup"><span data-stu-id="08161-207">Operators</span></span>

<span data-ttu-id="08161-208">表达式从构造***操作数***并***运算符***。</span><span class="sxs-lookup"><span data-stu-id="08161-208">Expressions are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="08161-209">表达式的运算符指明了向操作数应用的运算。</span><span class="sxs-lookup"><span data-stu-id="08161-209">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="08161-210">运算符的示例包括 `+`、`-`、`*`、`/` 和 `new`。</span><span class="sxs-lookup"><span data-stu-id="08161-210">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="08161-211">操作数的示例包括文本、字段、局部变量和表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-211">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="08161-212">有三种类型的运算符：</span><span class="sxs-lookup"><span data-stu-id="08161-212">There are three kinds of operators:</span></span>

*  <span data-ttu-id="08161-213">一元运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-213">Unary operators.</span></span> <span data-ttu-id="08161-214">一元运算符接受一个操作数，并使用任一前缀表示法 (例如`--x`) 或后缀表示法 (例如`x++`)。</span><span class="sxs-lookup"><span data-stu-id="08161-214">The unary operators take one operand and use either prefix notation (such as `--x`) or postfix notation (such as `x++`).</span></span>
*  <span data-ttu-id="08161-215">二元运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-215">Binary operators.</span></span> <span data-ttu-id="08161-216">二进制运算符采用两个操作数，并且所有使用中缀表示法 (例如`x + y`)。</span><span class="sxs-lookup"><span data-stu-id="08161-216">The binary operators take two operands and all use infix notation (such as `x + y`).</span></span>
*  <span data-ttu-id="08161-217">三元运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-217">Ternary operator.</span></span> <span data-ttu-id="08161-218">只有一个三元运算符`?:`，存在; 它采用三个操作数，并使用中缀表示法 (`c ? x : y`)。</span><span class="sxs-lookup"><span data-stu-id="08161-218">Only one ternary operator, `?:`, exists; it takes three operands and uses infix notation (`c ? x : y`).</span></span>

<span data-ttu-id="08161-219">在表达式中运算符的计算顺序由***优先***和***关联性***的运算符 ([运算符优先级和结合性](expressions.md#operator-precedence-and-associativity)).</span><span class="sxs-lookup"><span data-stu-id="08161-219">The order of evaluation of operators in an expression is determined by the ***precedence*** and ***associativity*** of the operators ([Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)).</span></span>

<span data-ttu-id="08161-220">从左到右情况下，在表达式中的操作数进行求值。</span><span class="sxs-lookup"><span data-stu-id="08161-220">Operands in an expression are evaluated from left to right.</span></span> <span data-ttu-id="08161-221">例如，在`F(i) + G(i++) * H(i)`，方法`F`使用的旧值调用`i`，然后方法`G`使用的旧值调用`i`，并，最后，方法`H`的新值与名为`i`.</span><span class="sxs-lookup"><span data-stu-id="08161-221">For example, in `F(i) + G(i++) * H(i)`, method `F` is called using the old value of `i`, then method `G` is called with the old value of `i`, and, finally, method `H` is called with the new value of `i`.</span></span> <span data-ttu-id="08161-222">这是运算符优先级独立于和不相关。</span><span class="sxs-lookup"><span data-stu-id="08161-222">This is separate from and unrelated to operator precedence.</span></span>

<span data-ttu-id="08161-223">某些运算符可以是***重载***。</span><span class="sxs-lookup"><span data-stu-id="08161-223">Certain operators can be ***overloaded***.</span></span> <span data-ttu-id="08161-224">运算符重载允许用户定义的运算符实现为操作指定一个或两个操作数均为用户定义的类或结构类型 ([运算符重载](expressions.md#operator-overloading))。</span><span class="sxs-lookup"><span data-stu-id="08161-224">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type ([Operator overloading](expressions.md#operator-overloading)).</span></span>

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="08161-225">运算符优先级和关联性</span><span class="sxs-lookup"><span data-stu-id="08161-225">Operator precedence and associativity</span></span>

<span data-ttu-id="08161-226">如果表达式包含多个运算符，那么运算符的***优先级***决定了各个运算符的计算顺序。</span><span class="sxs-lookup"><span data-stu-id="08161-226">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="08161-227">例如，表达式`x + y * z`评为`x + (y * z)`因为`*`运算符的优先级高于该二进制文件`+`运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-227">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the binary `+` operator.</span></span> <span data-ttu-id="08161-228">运算符的优先顺序建立其关联的语法生产的定义。</span><span class="sxs-lookup"><span data-stu-id="08161-228">The precedence of an operator is established by the definition of its associated grammar production.</span></span> <span data-ttu-id="08161-229">例如， *additive_expression*包含的一系列*multiplicative_expression*s 分隔`+`或`-`运算符，从而为提供`+`和`-`运算符较低的优先级高于`*`， `/`，和`%`运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-229">For example, an *additive_expression* consists of a sequence of *multiplicative_expression*s separated by `+` or `-` operators, thus giving the `+` and `-` operators lower precedence than the `*`, `/`, and `%` operators.</span></span>

<span data-ttu-id="08161-230">下表汇总了所有运算符按优先级从高到低的顺序：</span><span class="sxs-lookup"><span data-stu-id="08161-230">The following table summarizes all operators in order of precedence from highest to lowest:</span></span>

| <span data-ttu-id="08161-231">__节__</span><span class="sxs-lookup"><span data-stu-id="08161-231">__Section__</span></span>                                                                                   | <span data-ttu-id="08161-232">__类别__</span><span class="sxs-lookup"><span data-stu-id="08161-232">__Category__</span></span>                | <span data-ttu-id="08161-233">__运算符__</span><span class="sxs-lookup"><span data-stu-id="08161-233">__Operators__</span></span> | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [<span data-ttu-id="08161-234">主要表达式</span><span class="sxs-lookup"><span data-stu-id="08161-234">Primary expressions</span></span>](expressions.md#primary-expressions)                                     | <span data-ttu-id="08161-235">基本</span><span class="sxs-lookup"><span data-stu-id="08161-235">Primary</span></span>                     | <span data-ttu-id="08161-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span><span class="sxs-lookup"><span data-stu-id="08161-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span></span> | 
| [<span data-ttu-id="08161-237">一元运算符</span><span class="sxs-lookup"><span data-stu-id="08161-237">Unary operators</span></span>](expressions.md#unary-operators)                                             | <span data-ttu-id="08161-238">一元</span><span class="sxs-lookup"><span data-stu-id="08161-238">Unary</span></span>                       | <span data-ttu-id="08161-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span><span class="sxs-lookup"><span data-stu-id="08161-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span></span> | 
| [<span data-ttu-id="08161-240">算术运算符</span><span class="sxs-lookup"><span data-stu-id="08161-240">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="08161-241">乘法</span><span class="sxs-lookup"><span data-stu-id="08161-241">Multiplicative</span></span>              | <span data-ttu-id="08161-242">`*`  `/`  `%`</span><span class="sxs-lookup"><span data-stu-id="08161-242">`*`  `/`  `%`</span></span> | 
| [<span data-ttu-id="08161-243">算术运算符</span><span class="sxs-lookup"><span data-stu-id="08161-243">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="08161-244">加法</span><span class="sxs-lookup"><span data-stu-id="08161-244">Additive</span></span>                    | <span data-ttu-id="08161-245">`+`  `-`</span><span class="sxs-lookup"><span data-stu-id="08161-245">`+`  `-`</span></span>      | 
| [<span data-ttu-id="08161-246">移位运算符</span><span class="sxs-lookup"><span data-stu-id="08161-246">Shift operators</span></span>](expressions.md#shift-operators)                                             | <span data-ttu-id="08161-247">移位</span><span class="sxs-lookup"><span data-stu-id="08161-247">Shift</span></span>                       | <span data-ttu-id="08161-248">`<<`  `>>`</span><span class="sxs-lookup"><span data-stu-id="08161-248">`<<`  `>>`</span></span>    | 
| [<span data-ttu-id="08161-249">关系和类型测试运算符</span><span class="sxs-lookup"><span data-stu-id="08161-249">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="08161-250">关系和类型测试</span><span class="sxs-lookup"><span data-stu-id="08161-250">Relational and type testing</span></span> | <span data-ttu-id="08161-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span><span class="sxs-lookup"><span data-stu-id="08161-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span></span> | 
| [<span data-ttu-id="08161-252">关系和类型测试运算符</span><span class="sxs-lookup"><span data-stu-id="08161-252">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="08161-253">相等</span><span class="sxs-lookup"><span data-stu-id="08161-253">Equality</span></span>                    | <span data-ttu-id="08161-254">`==`  `!=`</span><span class="sxs-lookup"><span data-stu-id="08161-254">`==`  `!=`</span></span>    | 
| [<span data-ttu-id="08161-255">逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="08161-255">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="08161-256">逻辑“与”</span><span class="sxs-lookup"><span data-stu-id="08161-256">Logical AND</span></span>                 | `&`           | 
| [<span data-ttu-id="08161-257">逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="08161-257">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="08161-258">逻辑 XOR</span><span class="sxs-lookup"><span data-stu-id="08161-258">Logical XOR</span></span>                 | `^`           | 
| [<span data-ttu-id="08161-259">逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="08161-259">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="08161-260">逻辑“或”</span><span class="sxs-lookup"><span data-stu-id="08161-260">Logical OR</span></span>                  | <code>&#124;</code>           |
| [<span data-ttu-id="08161-261">条件逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="08161-261">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="08161-262">条件“与”</span><span class="sxs-lookup"><span data-stu-id="08161-262">Conditional AND</span></span>             | `&&`          | 
| [<span data-ttu-id="08161-263">条件逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="08161-263">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="08161-264">条件“或”</span><span class="sxs-lookup"><span data-stu-id="08161-264">Conditional OR</span></span>              | <code>&#124;&#124;</code>          | 
| [<span data-ttu-id="08161-265">Null 合并运算符</span><span class="sxs-lookup"><span data-stu-id="08161-265">The null coalescing operator</span></span>](expressions.md#the-null-coalescing-operator)                   | <span data-ttu-id="08161-266">null 合并</span><span class="sxs-lookup"><span data-stu-id="08161-266">Null coalescing</span></span>             | `??`          | 
| [<span data-ttu-id="08161-267">条件运算符</span><span class="sxs-lookup"><span data-stu-id="08161-267">Conditional operator</span></span>](expressions.md#conditional-operator)                                   | <span data-ttu-id="08161-268">条件运算</span><span class="sxs-lookup"><span data-stu-id="08161-268">Conditional</span></span>                 | `?:`          | 
| <span data-ttu-id="08161-269">[赋值运算符](expressions.md#assignment-operators)，[匿名函数表达式](expressions.md#anonymous-function-expressions)</span><span class="sxs-lookup"><span data-stu-id="08161-269">[Assignment operators](expressions.md#assignment-operators), [Anonymous function expressions](expressions.md#anonymous-function-expressions)</span></span>  | <span data-ttu-id="08161-270">赋值和 lambda 表达式</span><span class="sxs-lookup"><span data-stu-id="08161-270">Assignment and lambda expression</span></span> | <span data-ttu-id="08161-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span><span class="sxs-lookup"><span data-stu-id="08161-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span></span> | 

<span data-ttu-id="08161-272">如果操作数具有相同优先级的两个运算符之间，运算符的关联性控件执行的操作的顺序：</span><span class="sxs-lookup"><span data-stu-id="08161-272">When an operand occurs between two operators with the same precedence, the associativity of the operators controls the order in which the operations are performed:</span></span>

*  <span data-ttu-id="08161-273">除赋值运算符和 null 合并运算符，所有二元运算符均***左结合***，即操作执行从左到右。</span><span class="sxs-lookup"><span data-stu-id="08161-273">Except for the assignment operators and the null coalescing operator, all binary operators are ***left-associative***, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="08161-274">例如，`x + y + z` 将计算为 `(x + y) + z`。</span><span class="sxs-lookup"><span data-stu-id="08161-274">For example, `x + y + z` is evaluated as `(x + y) + z`.</span></span>
*  <span data-ttu-id="08161-275">赋值运算符、 null 合并运算符和条件运算符 (`?:`) 是***右结合运算符***，这意味着从右到左执行运算。</span><span class="sxs-lookup"><span data-stu-id="08161-275">The assignment operators, the null coalescing operator and the conditional operator (`?:`) are ***right-associative***, meaning that operations are performed from right to left.</span></span> <span data-ttu-id="08161-276">例如，`x = y = z` 将计算为 `x = (y = z)`。</span><span class="sxs-lookup"><span data-stu-id="08161-276">For example, `x = y = z` is evaluated as `x = (y = z)`.</span></span>

<span data-ttu-id="08161-277">可以使用括号控制优先级和结合性。</span><span class="sxs-lookup"><span data-stu-id="08161-277">Precedence and associativity can be controlled using parentheses.</span></span> <span data-ttu-id="08161-278">例如，`x + y * z` 先计算 `y` 乘 `z`，并将结果与 `x` 相加，而 `(x + y) * z` 则先计算 `x` 加 `y`，然后将结果与 `z` 相乘。</span><span class="sxs-lookup"><span data-stu-id="08161-278">For example, `x + y * z` first multiplies `y` by `z` and then adds the result to `x`, but `(x + y) * z` first adds `x` and `y` and then multiplies the result by `z`.</span></span>

### <a name="operator-overloading"></a><span data-ttu-id="08161-279">运算符重载</span><span class="sxs-lookup"><span data-stu-id="08161-279">Operator overloading</span></span>

<span data-ttu-id="08161-280">所有的一元和二元运算符具有预定义的任何表达式中会自动提供的实现。</span><span class="sxs-lookup"><span data-stu-id="08161-280">All unary and binary operators have predefined implementations that are automatically available in any expression.</span></span> <span data-ttu-id="08161-281">除了预定义的实现中，用户定义的实现可以引入通过包括`operator`类和结构中的声明 ([运算符](classes.md#operators))。</span><span class="sxs-lookup"><span data-stu-id="08161-281">In addition to the predefined implementations, user-defined implementations can be introduced by including `operator` declarations in classes and structs ([Operators](classes.md#operators)).</span></span> <span data-ttu-id="08161-282">用户定义的运算符实现始终优先于预定义的运算符实现：仅当没有适用的用户定义运算符实现存在将会考虑预定义的运算符实现，如中所述[一元运算符重载决策](expressions.md#unary-operator-overload-resolution)和[二元运算符重载解析](expressions.md#binary-operator-overload-resolution)。</span><span class="sxs-lookup"><span data-stu-id="08161-282">User-defined operator implementations always take precedence over predefined operator implementations: Only when no applicable user-defined operator implementations exist will the predefined operator implementations be considered, as described in [Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution).</span></span>

<span data-ttu-id="08161-283">***可重载的一元运算符***是：</span><span class="sxs-lookup"><span data-stu-id="08161-283">The ***overloadable unary operators*** are:</span></span>
```csharp
+   -   !   ~   ++   --   true   false
```

<span data-ttu-id="08161-284">尽管`true`和`false`不在表达式中显式使用 (并因此不包括的优先顺序表中[运算符优先级和结合性](expressions.md#operator-precedence-and-associativity))，因为它们是，它们将被视为运算符在多个表达式上下文中调用： 布尔表达式 ([布尔表达式](expressions.md#boolean-expressions)) 和包含条件表达式 ([条件运算符](expressions.md#conditional-operator))，和条件逻辑运算符 ([条件逻辑运算符](expressions.md#conditional-logical-operators))。</span><span class="sxs-lookup"><span data-stu-id="08161-284">Although `true` and `false` are not used explicitly in expressions (and therefore are not included in the precedence table in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)), they are considered operators because they are invoked in several expression contexts: boolean expressions ([Boolean expressions](expressions.md#boolean-expressions)) and expressions involving the conditional ([Conditional operator](expressions.md#conditional-operator)), and conditional logical operators ([Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span>

<span data-ttu-id="08161-285">***可重载的二元运算符***是：</span><span class="sxs-lookup"><span data-stu-id="08161-285">The ***overloadable binary operators*** are:</span></span>
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

<span data-ttu-id="08161-286">只有上面列出的运算符可以进行重载。</span><span class="sxs-lookup"><span data-stu-id="08161-286">Only the operators listed above can be overloaded.</span></span> <span data-ttu-id="08161-287">具体而言，不能重载成员访问，方法调用中，或`=`， `&&`， `||`， `??`， `?:`， `=>`， `checked`， `unchecked`， `new`， `typeof`， `default`， `as`，和`is`运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-287">In particular, it is not possible to overload member access, method invocation, or the `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, and `is` operators.</span></span>

<span data-ttu-id="08161-288">重载二元运算符时，也会隐式重载相应的赋值运算符（若有）。</span><span class="sxs-lookup"><span data-stu-id="08161-288">When a binary operator is overloaded, the corresponding assignment operator, if any, is also implicitly overloaded.</span></span> <span data-ttu-id="08161-289">例如，运算符重载`*`也是运算符重载`*=`。</span><span class="sxs-lookup"><span data-stu-id="08161-289">For example, an overload of operator `*` is also an overload of operator `*=`.</span></span> <span data-ttu-id="08161-290">这是中作了进一步介绍[复合赋值](expressions.md#compound-assignment)。</span><span class="sxs-lookup"><span data-stu-id="08161-290">This is described further in [Compound assignment](expressions.md#compound-assignment).</span></span> <span data-ttu-id="08161-291">请注意，赋值运算符本身 (`=`) 不能重载。</span><span class="sxs-lookup"><span data-stu-id="08161-291">Note that the assignment operator itself (`=`) cannot be overloaded.</span></span> <span data-ttu-id="08161-292">始终分配到一个变量执行简单的值的按位复制。</span><span class="sxs-lookup"><span data-stu-id="08161-292">An assignment always performs a simple bit-wise copy of a value into a variable.</span></span>

<span data-ttu-id="08161-293">将强制转换操作，比如`(T)x`，通过提供用户定义的转换重载 ([用户定义的转换](conversions.md#user-defined-conversions))。</span><span class="sxs-lookup"><span data-stu-id="08161-293">Cast operations, such as `(T)x`, are overloaded by providing user-defined conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

<span data-ttu-id="08161-294">元素访问，例如`a[x]`，不被视为输入可重载运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-294">Element access, such as `a[x]`, is not considered an overloadable operator.</span></span> <span data-ttu-id="08161-295">相反，用户定义的索引通过索引器支持 ([索引器](classes.md#indexers))。</span><span class="sxs-lookup"><span data-stu-id="08161-295">Instead, user-defined indexing is supported through indexers ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="08161-296">在表达式中，使用运算符表示法来引用运算符，并在声明中，使用函数表示法来引用运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-296">In expressions, operators are referenced using operator notation, and in declarations, operators are referenced using functional notation.</span></span> <span data-ttu-id="08161-297">下表显示了运算符和函数表示法一元和二元运算符之间的关系。</span><span class="sxs-lookup"><span data-stu-id="08161-297">The following table shows the relationship between operator and functional notations for unary and binary operators.</span></span> <span data-ttu-id="08161-298">中的第一个条目*op*表示任何可重载的一元前缀运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-298">In the first entry, *op* denotes any overloadable unary prefix operator.</span></span> <span data-ttu-id="08161-299">在第二个条目中， *op*表示一元后缀`++`和`--`运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-299">In the second entry, *op* denotes the unary postfix `++` and `--` operators.</span></span> <span data-ttu-id="08161-300">在第三个条目中， *op*表示任何可重载的二元运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-300">In the third entry, *op* denotes any overloadable binary operator.</span></span>


| <span data-ttu-id="08161-301">__运算符表示法__</span><span class="sxs-lookup"><span data-stu-id="08161-301">__Operator notation__</span></span> | <span data-ttu-id="08161-302">__函数表示法__</span><span class="sxs-lookup"><span data-stu-id="08161-302">__Functional notation__</span></span> |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

<span data-ttu-id="08161-303">用户定义的运算符声明始终需要至少一个要包含在运算符声明的类或结构类型的参数。</span><span class="sxs-lookup"><span data-stu-id="08161-303">User-defined operator declarations always require at least one of the parameters to be of the class or struct type that contains the operator declaration.</span></span> <span data-ttu-id="08161-304">因此，不能为用户定义的运算符具有与预定义的运算符相同的签名。</span><span class="sxs-lookup"><span data-stu-id="08161-304">Thus, it is not possible for a user-defined operator to have the same signature as a predefined operator.</span></span>

<span data-ttu-id="08161-305">用户定义的运算符声明不能修改语法、 优先级或运算符的关联性。</span><span class="sxs-lookup"><span data-stu-id="08161-305">User-defined operator declarations cannot modify the syntax, precedence, or associativity of an operator.</span></span> <span data-ttu-id="08161-306">例如，`/`运算符始终是二元运算符始终具有优先级中指定的级别[运算符优先级和结合性](expressions.md#operator-precedence-and-associativity)，并且始终是左结合运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-306">For example, the `/` operator is always a binary operator, always has the precedence level specified in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity), and is always left-associative.</span></span>

<span data-ttu-id="08161-307">用户定义的运算符，可以执行的任何计算它所请求的操作时，生成的结果而非直观地预期的实现都是强烈建议不要使用。</span><span class="sxs-lookup"><span data-stu-id="08161-307">While it is possible for a user-defined operator to perform any computation it pleases, implementations that produce results other than those that are intuitively expected are strongly discouraged.</span></span> <span data-ttu-id="08161-308">例如，实现`operator ==`应比较是否相等的两个操作数，并返回相应`bool`结果。</span><span class="sxs-lookup"><span data-stu-id="08161-308">For example, an implementation of `operator ==` should compare the two operands for equality and return an appropriate `bool` result.</span></span>

<span data-ttu-id="08161-309">中的各个运算符的说明[主表达式](expressions.md#primary-expressions)通过[条件逻辑运算符](expressions.md#conditional-logical-operators)指定的预定义的运算符和任何其他规则适用的实现与每个运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-309">The descriptions of individual operators in [Primary expressions](expressions.md#primary-expressions) through [Conditional logical operators](expressions.md#conditional-logical-operators) specify the predefined implementations of the operators and any additional rules that apply to each operator.</span></span> <span data-ttu-id="08161-310">在这些说明使用的条款***一元运算符重载决策***，***二元运算符重载决策***，并***数值升级***，其中的定义在以下各节中找到。</span><span class="sxs-lookup"><span data-stu-id="08161-310">The descriptions make use of the terms ***unary operator overload resolution***, ***binary operator overload resolution***, and ***numeric promotion***, definitions of which are found in the following sections.</span></span>

### <a name="unary-operator-overload-resolution"></a><span data-ttu-id="08161-311">一元运算符重载决策</span><span class="sxs-lookup"><span data-stu-id="08161-311">Unary operator overload resolution</span></span>

<span data-ttu-id="08161-312">操作的窗体`op x`或`x op`，其中`op`是可重载的一元运算符，和`x`类型的表达式`X`，处理时，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-312">An operation of the form `op x` or `x op`, where `op` is an overloadable unary operator, and `x` is an expression of type `X`, is processed as follows:</span></span>

*  <span data-ttu-id="08161-313">提供的候选用户定义运算符的一套`X`操作`operator op(x)`使用的规则确定[候选用户定义的运算符](expressions.md#candidate-user-defined-operators)。</span><span class="sxs-lookup"><span data-stu-id="08161-313">The set of candidate user-defined operators provided by `X` for the operation `operator op(x)` is determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span>
*  <span data-ttu-id="08161-314">如果候选用户定义的运算符集不为空，则这成为候选操作的运算符集。</span><span class="sxs-lookup"><span data-stu-id="08161-314">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="08161-315">否则为预定义的一元`operator op`实现，包括其提升的窗体，成为候选操作的运算符集。</span><span class="sxs-lookup"><span data-stu-id="08161-315">Otherwise, the predefined unary `operator op` implementations, including their lifted forms, become the set of candidate operators for the operation.</span></span> <span data-ttu-id="08161-316">运算符的说明中指定的预定义的实现给定运算符 ([主表达式](expressions.md#primary-expressions)并[一元运算符](expressions.md#unary-operators))。</span><span class="sxs-lookup"><span data-stu-id="08161-316">The predefined implementations of a given operator are specified in the description of the operator ([Primary expressions](expressions.md#primary-expressions) and [Unary operators](expressions.md#unary-operators)).</span></span>
*  <span data-ttu-id="08161-317">重载决策规则[重载决策](expressions.md#overload-resolution)将应用于的候选运算符来选择关于参数列表的最佳运算符集`(x)`，此运算符将成为该重载的结果解析过程。</span><span class="sxs-lookup"><span data-stu-id="08161-317">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="08161-318">如果选择单个最佳运算符重载决策失败，发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-318">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="binary-operator-overload-resolution"></a><span data-ttu-id="08161-319">二元运算符重载决策</span><span class="sxs-lookup"><span data-stu-id="08161-319">Binary operator overload resolution</span></span>

<span data-ttu-id="08161-320">操作的窗体`x op y`，其中`op`是可重载的二元运算符`x`类型的表达式`X`，和`y`类型的表达式`Y`，处理时，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-320">An operation of the form `x op y`, where `op` is an overloadable binary operator, `x` is an expression of type `X`, and `y` is an expression of type `Y`, is processed as follows:</span></span>

*  <span data-ttu-id="08161-321">提供的候选用户定义运算符的一套`X`并`Y`操作`operator op(x,y)`确定。</span><span class="sxs-lookup"><span data-stu-id="08161-321">The set of candidate user-defined operators provided by `X` and `Y` for the operation `operator op(x,y)` is determined.</span></span> <span data-ttu-id="08161-322">集包括的联合提供的候选运算符`X`和由提供的候选运算符`Y`，每个确定使用的规则[候选用户定义的运算符](expressions.md#candidate-user-defined-operators)。</span><span class="sxs-lookup"><span data-stu-id="08161-322">The set consists of the union of the candidate operators provided by `X` and the candidate operators provided by `Y`, each determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span> <span data-ttu-id="08161-323">如果`X`并`Y`具有相同的类型，或者如果`X`和`Y`派生自公共基类型，然后共享的候选运算符仅在出现的组合集一次。</span><span class="sxs-lookup"><span data-stu-id="08161-323">If `X` and `Y` are the same type, or if `X` and `Y` are derived from a common base type, then shared candidate operators only occur in the combined set once.</span></span>
*  <span data-ttu-id="08161-324">如果候选用户定义的运算符集不为空，则这成为候选操作的运算符集。</span><span class="sxs-lookup"><span data-stu-id="08161-324">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="08161-325">否则为预定义的二进制文件`operator op`实现，包括其提升的窗体，成为候选操作的运算符集。</span><span class="sxs-lookup"><span data-stu-id="08161-325">Otherwise, the predefined binary `operator op` implementations, including their lifted forms,  become the set of candidate operators for the operation.</span></span> <span data-ttu-id="08161-326">运算符的说明中指定的预定义的实现给定运算符 ([算术运算符](expressions.md#arithmetic-operators)通过[条件逻辑运算符](expressions.md#conditional-logical-operators))。</span><span class="sxs-lookup"><span data-stu-id="08161-326">The predefined implementations of a given operator are specified in the description of the operator ([Arithmetic operators](expressions.md#arithmetic-operators) through [Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span> <span data-ttu-id="08161-327">对于预定义的枚举和委托运算符，唯一的运算符被视为是定义的其中一个操作数的绑定时类型的枚举或委托类型。</span><span class="sxs-lookup"><span data-stu-id="08161-327">For predefined enum and delegate operators, the only operators considered are those defined by an enum or delegate type that is the binding-time type of one of the operands.</span></span>
*  <span data-ttu-id="08161-328">重载决策规则[重载决策](expressions.md#overload-resolution)将应用于的候选运算符来选择关于参数列表的最佳运算符集`(x,y)`，此运算符将成为该重载的结果解析过程。</span><span class="sxs-lookup"><span data-stu-id="08161-328">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x,y)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="08161-329">如果选择单个最佳运算符重载决策失败，发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-329">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="candidate-user-defined-operators"></a><span data-ttu-id="08161-330">候选用户定义的运算符</span><span class="sxs-lookup"><span data-stu-id="08161-330">Candidate user-defined operators</span></span>

<span data-ttu-id="08161-331">对于给定的类型`T`和操作`operator op(A)`，其中`op`是可重载运算符和`A`是由用户定义的运算符的参数列表中，组候选`T`为`operator op(A)`确定按如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-331">Given a type `T` and an operation `operator op(A)`, where `op` is an overloadable operator and `A` is an argument list, the set of candidate user-defined operators provided by `T` for `operator op(A)` is determined as follows:</span></span>

*  <span data-ttu-id="08161-332">确定类型`T0`。</span><span class="sxs-lookup"><span data-stu-id="08161-332">Determine the type `T0`.</span></span> <span data-ttu-id="08161-333">如果`T`为 null 的类型，`T0`是其基础类型，否则`T0`等同于`T`。</span><span class="sxs-lookup"><span data-stu-id="08161-333">If `T` is a nullable type, `T0` is its underlying type, otherwise `T0` is equal to `T`.</span></span>
*  <span data-ttu-id="08161-334">为所有`operator op`中的声明`T0`以及所有提升此类运算符的形式，如果至少一个运算符适用 ([适用函数成员](expressions.md#applicable-function-member)) 与参数列表`A`，然后的集候选运算符包含中的所有此类适用运算符`T0`。</span><span class="sxs-lookup"><span data-stu-id="08161-334">For all `operator op` declarations in `T0` and all lifted forms of such operators, if at least one operator is applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the argument list `A`, then the set of candidate operators consists of all such applicable operators in `T0`.</span></span>
*  <span data-ttu-id="08161-335">否则为如果`T0`是`object`，候选运算符集为空。</span><span class="sxs-lookup"><span data-stu-id="08161-335">Otherwise, if `T0` is `object`, the set of candidate operators is empty.</span></span>
*  <span data-ttu-id="08161-336">否则，候选运算符集提供的`T0`是一组提供的直接基类的候选运算符`T0`，或有效的类的基类`T0`如果`T0`是类型参数。</span><span class="sxs-lookup"><span data-stu-id="08161-336">Otherwise, the set of candidate operators provided by `T0` is the set of candidate operators provided by the direct base class of `T0`, or the effective base class of `T0` if `T0` is a type parameter.</span></span>

### <a name="numeric-promotions"></a><span data-ttu-id="08161-337">数值提升</span><span class="sxs-lookup"><span data-stu-id="08161-337">Numeric promotions</span></span>

<span data-ttu-id="08161-338">数值升级包含自动执行某些隐式转换的预定义的一元和二进制数字运算符的操作数。</span><span class="sxs-lookup"><span data-stu-id="08161-338">Numeric promotion consists of automatically performing certain implicit conversions of the operands of the predefined unary and binary numeric operators.</span></span> <span data-ttu-id="08161-339">数值提升不是一种不同机制，而是将应用到预定义的运算符重载决策的影响。</span><span class="sxs-lookup"><span data-stu-id="08161-339">Numeric promotion is not a distinct mechanism, but rather an effect of applying overload resolution to the predefined operators.</span></span> <span data-ttu-id="08161-340">数值升级专门不影响评估用户定义的运算符，尽管可以实现用户定义的运算符来表现类似的效果。</span><span class="sxs-lookup"><span data-stu-id="08161-340">Numeric promotion specifically does not affect evaluation of user-defined operators, although user-defined operators can be implemented to exhibit similar effects.</span></span>

<span data-ttu-id="08161-341">作为数值升级的示例，请考虑将二进制文件的预定义的实现`*`运算符：</span><span class="sxs-lookup"><span data-stu-id="08161-341">As an example of numeric promotion, consider the predefined implementations of the binary `*` operator:</span></span>

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

<span data-ttu-id="08161-342">当重载决策规则 ([重载决策](expressions.md#overload-resolution)) 应用于此集的运算符的效果是以选择存在隐式转换运算符的第一个操作数类型。</span><span class="sxs-lookup"><span data-stu-id="08161-342">When overload resolution rules ([Overload resolution](expressions.md#overload-resolution)) are applied to this set of operators, the effect is to select the first of the operators for which implicit conversions exist from the operand types.</span></span> <span data-ttu-id="08161-343">例如，对于该操作`b * s`，其中`b`是`byte`并`s`是`short`，重载解析选择`operator *(int,int)`为最佳的运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-343">For example, for the operation `b * s`, where `b` is a `byte` and `s` is a `short`, overload resolution selects `operator *(int,int)` as the best operator.</span></span> <span data-ttu-id="08161-344">因此的效果是，`b`并`s`转换为`int`，并且结果的类型为`int`。</span><span class="sxs-lookup"><span data-stu-id="08161-344">Thus, the effect is that `b` and `s` are converted to `int`, and the type of the result is `int`.</span></span> <span data-ttu-id="08161-345">同样，为操作`i * d`，其中`i`是`int`并`d`是`double`，重载解析选择`operator *(double,double)`为最佳的运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-345">Likewise, for the operation `i * d`, where `i` is an `int` and `d` is a `double`, overload resolution selects `operator *(double,double)` as the best operator.</span></span>

#### <a name="unary-numeric-promotions"></a><span data-ttu-id="08161-346">一元数值提升</span><span class="sxs-lookup"><span data-stu-id="08161-346">Unary numeric promotions</span></span>

<span data-ttu-id="08161-347">预定义的操作数进行一元数值升级`+`， `-`，和`~`一元运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-347">Unary numeric promotion occurs for the operands of the predefined `+`, `-`, and `~` unary operators.</span></span> <span data-ttu-id="08161-348">一元数值升级只包含的转换类型的操作数`sbyte`， `byte`， `short`， `ushort`，或`char`以键入`int`。</span><span class="sxs-lookup"><span data-stu-id="08161-348">Unary numeric promotion simply consists of converting operands of type `sbyte`, `byte`, `short`, `ushort`, or `char` to type `int`.</span></span> <span data-ttu-id="08161-349">此外，对于一元`-`运算符，一元数值升级将转换的类型的操作数`uint`键入`long`。</span><span class="sxs-lookup"><span data-stu-id="08161-349">Additionally, for the unary `-` operator, unary numeric promotion converts operands of type `uint` to type `long`.</span></span>

#### <a name="binary-numeric-promotions"></a><span data-ttu-id="08161-350">二进制数值提升</span><span class="sxs-lookup"><span data-stu-id="08161-350">Binary numeric promotions</span></span>

<span data-ttu-id="08161-351">预定义的操作数进行二进制数值升级`+`， `-`， `*`， `/`， `%`， `&`， `|`， `^`， `==`， `!=`，`>`， `<`， `>=`，和`<=`二元运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-351">Binary numeric promotion occurs for the operands of the predefined `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, and `<=` binary operators.</span></span> <span data-ttu-id="08161-352">二进制数值升级隐式转换为通用类型的非关系运算符时也将成为该操作的结果类型的两个操作数。</span><span class="sxs-lookup"><span data-stu-id="08161-352">Binary numeric promotion implicitly converts both operands to a common type which, in case of the non-relational operators, also becomes the result type of the operation.</span></span> <span data-ttu-id="08161-353">二进制数值升级包含将应用以下规则按其在此处显示的顺序：</span><span class="sxs-lookup"><span data-stu-id="08161-353">Binary numeric promotion consists of applying the following rules, in the order they appear here:</span></span>

*  <span data-ttu-id="08161-354">如果其中一个操作数的类型`decimal`，另一个操作数将转换为类型`decimal`，或者如果另一个操作数的类型发生了绑定时间错误`float`或`double`。</span><span class="sxs-lookup"><span data-stu-id="08161-354">If either operand is of type `decimal`, the other operand is converted to type `decimal`, or a binding-time error occurs if the other operand is of type `float` or `double`.</span></span>
*  <span data-ttu-id="08161-355">否则为如果其中一个操作数的类型`double`，另一个操作数将转换为类型`double`。</span><span class="sxs-lookup"><span data-stu-id="08161-355">Otherwise, if either operand is of type `double`, the other operand is converted to type `double`.</span></span>
*  <span data-ttu-id="08161-356">否则为如果其中一个操作数的类型`float`，另一个操作数将转换为类型`float`。</span><span class="sxs-lookup"><span data-stu-id="08161-356">Otherwise, if either operand is of type `float`, the other operand is converted to type `float`.</span></span>
*  <span data-ttu-id="08161-357">否则为如果其中一个操作数的类型`ulong`，另一个操作数将转换为类型`ulong`，或者如果另一个操作数的类型发生了绑定时间错误`sbyte`， `short`， `int`，或`long`。</span><span class="sxs-lookup"><span data-stu-id="08161-357">Otherwise, if either operand is of type `ulong`, the other operand is converted to type `ulong`, or a binding-time error occurs if the other operand is of type `sbyte`, `short`, `int`, or `long`.</span></span>
*  <span data-ttu-id="08161-358">否则为如果其中一个操作数的类型`long`，另一个操作数将转换为类型`long`。</span><span class="sxs-lookup"><span data-stu-id="08161-358">Otherwise, if either operand is of type `long`, the other operand is converted to type `long`.</span></span>
*  <span data-ttu-id="08161-359">否则为如果其中一个操作数的类型`uint`和另一个操作数的类型是`sbyte`， `short`，或`int`，这两个操作数都转换为键入`long`。</span><span class="sxs-lookup"><span data-stu-id="08161-359">Otherwise, if either operand is of type `uint` and the other operand is of type `sbyte`, `short`, or `int`, both operands are converted to type `long`.</span></span>
*  <span data-ttu-id="08161-360">否则为如果其中一个操作数的类型`uint`，另一个操作数将转换为类型`uint`。</span><span class="sxs-lookup"><span data-stu-id="08161-360">Otherwise, if either operand is of type `uint`, the other operand is converted to type `uint`.</span></span>
*  <span data-ttu-id="08161-361">否则，两个操作数都转换为类型`int`。</span><span class="sxs-lookup"><span data-stu-id="08161-361">Otherwise, both operands are converted to type `int`.</span></span>

<span data-ttu-id="08161-362">请注意第一个规则不允许混合使用任何操作`decimal`类型具有`double`和`float`类型。</span><span class="sxs-lookup"><span data-stu-id="08161-362">Note that the first rule disallows any operations that mix the `decimal` type with the `double` and `float` types.</span></span> <span data-ttu-id="08161-363">此规则遵循这一事实之间没有隐式转换从`decimal`类型和`double`和`float`类型。</span><span class="sxs-lookup"><span data-stu-id="08161-363">The rule follows from the fact that there are no implicit conversions between the `decimal` type and the `double` and `float` types.</span></span>

<span data-ttu-id="08161-364">另请注意不能为操作数的类型是`ulong`当另一个操作数属于有符号的整型。</span><span class="sxs-lookup"><span data-stu-id="08161-364">Also note that it is not possible for an operand to be of type `ulong` when the other operand is of a signed integral type.</span></span> <span data-ttu-id="08161-365">原因是任何整型类型存在，可以表示完整范围的`ulong`以及带符号整数类型。</span><span class="sxs-lookup"><span data-stu-id="08161-365">The reason is that no integral type exists that can represent the full range of `ulong` as well as the signed integral types.</span></span>

<span data-ttu-id="08161-366">在这种上述情况下，强制转换表达式可用于显式将一个操作数转换为符合另一个操作数的类型。</span><span class="sxs-lookup"><span data-stu-id="08161-366">In both of the above cases, a cast expression can be used to explicitly convert one operand to a type that is compatible with the other operand.</span></span>

<span data-ttu-id="08161-367">在示例</span><span class="sxs-lookup"><span data-stu-id="08161-367">In the example</span></span>
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
<span data-ttu-id="08161-368">发生绑定时错误的原因`decimal`不能乘以`double`。</span><span class="sxs-lookup"><span data-stu-id="08161-368">a binding-time error occurs because a `decimal` cannot be multiplied by a `double`.</span></span> <span data-ttu-id="08161-369">通过显式转换的第二个操作数解决错误`decimal`，按如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-369">The error is resolved by explicitly converting the second operand to `decimal`, as follows:</span></span>

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a><span data-ttu-id="08161-370">提升的运算符</span><span class="sxs-lookup"><span data-stu-id="08161-370">Lifted operators</span></span>

<span data-ttu-id="08161-371">***提升运算符***允许在不可以为 null 的值类型也可用于这些类型的可以为 null 的窗体运行的预定义和用户定义运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-371">***Lifted operators*** permit predefined and user-defined operators that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="08161-372">提升的运算符构造从预定义和用户定义运算符的满足某些要求，如以下所述：</span><span class="sxs-lookup"><span data-stu-id="08161-372">Lifted operators are constructed from predefined and user-defined operators that meet certain requirements, as described in the following:</span></span>

*   <span data-ttu-id="08161-373">对于一元运算符</span><span class="sxs-lookup"><span data-stu-id="08161-373">For the unary operators</span></span>

    ```csharp
    +  ++  -  --  !  ~
    ```

    <span data-ttu-id="08161-374">如果操作数和结果类型都为这两个不可以为 null 的值类型，运算符的提升的形式存在。</span><span class="sxs-lookup"><span data-stu-id="08161-374">a lifted form of an operator exists if the operand and result types are both non-nullable value types.</span></span> <span data-ttu-id="08161-375">通过将一个构造提升的形式`?`操作数和结果类型的修饰符。</span><span class="sxs-lookup"><span data-stu-id="08161-375">The lifted form is constructed by adding a single `?` modifier to the operand and result types.</span></span> <span data-ttu-id="08161-376">如果操作数为 null，则提升的运算符将产生一个 null 值。</span><span class="sxs-lookup"><span data-stu-id="08161-376">The lifted operator produces a null value if the operand is null.</span></span> <span data-ttu-id="08161-377">否则为提升的运算符对操作数进行解包、 应用基础运算符，并将结果包装。</span><span class="sxs-lookup"><span data-stu-id="08161-377">Otherwise, the lifted operator unwraps the operand, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="08161-378">对于二元运算符</span><span class="sxs-lookup"><span data-stu-id="08161-378">For the binary operators</span></span>

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    <span data-ttu-id="08161-379">如果操作数和结果类型是所有不可为 null 的值类型，将存在运算符的提升的形式。</span><span class="sxs-lookup"><span data-stu-id="08161-379">a lifted form of an operator exists if the operand and result types are all non-nullable value types.</span></span> <span data-ttu-id="08161-380">通过将一个构造提升的形式`?`为每个操作数和结果的类型修饰符。</span><span class="sxs-lookup"><span data-stu-id="08161-380">The lifted form is constructed by adding a single `?` modifier to each operand and result type.</span></span> <span data-ttu-id="08161-381">提升的运算符产生一个 null 值，如果一个或两个操作数均为 null (异常正在`&`并`|`运算符的`bool?`类型，如中所述[布尔逻辑运算符](expressions.md#boolean-logical-operators))。</span><span class="sxs-lookup"><span data-stu-id="08161-381">The lifted operator produces a null value if one or both operands are null (an exception being the `&` and `|` operators of the `bool?` type, as described in [Boolean logical operators](expressions.md#boolean-logical-operators)).</span></span> <span data-ttu-id="08161-382">否则为提升的运算符对操作数进行解包、 应用基础运算符，并将结果包装。</span><span class="sxs-lookup"><span data-stu-id="08161-382">Otherwise, the lifted operator unwraps the operands, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="08161-383">相等运算符</span><span class="sxs-lookup"><span data-stu-id="08161-383">For the equality operators</span></span>

    ```csharp
    ==  !=
    ```

    <span data-ttu-id="08161-384">如果操作数类型不可以为 null 的值类型和结果类型是否存在的运算符的提升的形式`bool`。</span><span class="sxs-lookup"><span data-stu-id="08161-384">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="08161-385">通过将一个构造提升的形式`?`为每个操作数的类型修饰符。</span><span class="sxs-lookup"><span data-stu-id="08161-385">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="08161-386">提升的运算符会将两个 null 值相等，而 null 的值为任何非 null 值不相等。</span><span class="sxs-lookup"><span data-stu-id="08161-386">The lifted operator considers two null values equal, and a null value unequal to any non-null value.</span></span> <span data-ttu-id="08161-387">如果两个操作数都为非空，则提升的运算符对操作数进行解包和应用基础运算符以产生`bool`结果。</span><span class="sxs-lookup"><span data-stu-id="08161-387">If both operands are non-null, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

*   <span data-ttu-id="08161-388">对于关系运算符</span><span class="sxs-lookup"><span data-stu-id="08161-388">For the relational operators</span></span>

    ```csharp
    <  >  <=  >=
    ```

    <span data-ttu-id="08161-389">如果操作数类型不可以为 null 的值类型和结果类型是否存在的运算符的提升的形式`bool`。</span><span class="sxs-lookup"><span data-stu-id="08161-389">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="08161-390">通过将一个构造提升的形式`?`为每个操作数的类型修饰符。</span><span class="sxs-lookup"><span data-stu-id="08161-390">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="08161-391">提升的运算符产生值`false`如果一个或两个操作数都为 null。</span><span class="sxs-lookup"><span data-stu-id="08161-391">The lifted operator produces the value `false` if one or both operands are null.</span></span> <span data-ttu-id="08161-392">否则为提升的运算符对操作数进行解包和应用基础运算符以产生`bool`结果。</span><span class="sxs-lookup"><span data-stu-id="08161-392">Otherwise, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

## <a name="member-lookup"></a><span data-ttu-id="08161-393">成员查找</span><span class="sxs-lookup"><span data-stu-id="08161-393">Member lookup</span></span>

<span data-ttu-id="08161-394">成员查找是，由此确定一种类型的上下文中名称的含义的过程。</span><span class="sxs-lookup"><span data-stu-id="08161-394">A member lookup is the process whereby the meaning of a name in the context of a type is determined.</span></span> <span data-ttu-id="08161-395">成员查找可以作为评估过程的一部分出现*simple_name* ([简单名称](expressions.md#simple-names)) 或*member_access* ([成员访问](expressions.md#member-access)) 中表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-395">A member lookup can occur as part of evaluating a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)) in an expression.</span></span> <span data-ttu-id="08161-396">如果*simple_name*或*member_access*作为发生*primary_expression*的*invocation_expression* ([方法调用](expressions.md#method-invocations))，则称该成员被调用。</span><span class="sxs-lookup"><span data-stu-id="08161-396">If the *simple_name* or *member_access* occurs as the *primary_expression* of an *invocation_expression* ([Method invocations](expressions.md#method-invocations)), the member is said to be invoked.</span></span>

<span data-ttu-id="08161-397">如果成员是方法或事件，或如果它是常量、 字段或委托类型的属性 ([委托](delegates.md)) 或类型`dynamic`([动态类型](types.md#the-dynamic-type))，则称该成员是*invocable*。</span><span class="sxs-lookup"><span data-stu-id="08161-397">If a member is a method or event, or if it is a constant, field or property of either a delegate type ([Delegates](delegates.md)) or the type `dynamic` ([The dynamic type](types.md#the-dynamic-type)), then the member is said to be *invocable*.</span></span>

<span data-ttu-id="08161-398">成员查找会考虑不仅成员还有数成员具有的类型参数和成员是否可访问的名称。</span><span class="sxs-lookup"><span data-stu-id="08161-398">Member lookup considers not only the name of a member but also the number of type parameters the member has and whether the member is accessible.</span></span> <span data-ttu-id="08161-399">为了成员查找，泛型方法和嵌套的泛型类型具有在其各自的声明中所指示的类型参数的数目和所有其他成员都具有零个类型参数。</span><span class="sxs-lookup"><span data-stu-id="08161-399">For the purposes of member lookup, generic methods and nested generic types have the number of type parameters indicated in their respective declarations and all other members have zero type parameters.</span></span>

<span data-ttu-id="08161-400">名称的成员查找 `N`与`K` 类型参数的类型中 `T`进行处理，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-400">A member lookup of a name `N` with `K` type parameters in a type `T` is processed as follows:</span></span>

*  <span data-ttu-id="08161-401">首先，可访问的成员名为一组 `N`确定：</span><span class="sxs-lookup"><span data-stu-id="08161-401">First, a set of accessible members named `N` is determined:</span></span>
    * <span data-ttu-id="08161-402">如果`T`是一个类型参数，则组是一组可访问的成员名为联合 `N`中每个类型指定为主约束或辅助约束 ([类型参数约束](classes.md#type-parameter-constraints)) 为 `T`，以及可访问的成员命名集 `N`中`object`。</span><span class="sxs-lookup"><span data-stu-id="08161-402">If `T` is a type parameter, then the set is the union of the sets of accessible members named `N` in each of the types specified as a primary constraint or secondary constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) for `T`, along with the set of accessible members named `N` in `object`.</span></span>
    * <span data-ttu-id="08161-403">否则，集包括的所有可访问 ([成员访问](basic-concepts.md#member-access)) 成员命名 `N`中 `T`，包括继承的成员和可访问的成员命名 `N`中`object`。</span><span class="sxs-lookup"><span data-stu-id="08161-403">Otherwise, the set consists of all accessible ([Member access](basic-concepts.md#member-access)) members named `N` in `T`, including inherited members and the accessible members named `N` in `object`.</span></span> <span data-ttu-id="08161-404">如果`T`是一个构造的类型的成员集获取，只需替换类型参数，如中所述[构造类型的成员](classes.md#members-of-constructed-types)。</span><span class="sxs-lookup"><span data-stu-id="08161-404">If `T` is a constructed type, the set of members is obtained by substituting type arguments as described in [Members of constructed types](classes.md#members-of-constructed-types).</span></span> <span data-ttu-id="08161-405">包含成员的`override`修饰符从集中排除。</span><span class="sxs-lookup"><span data-stu-id="08161-405">Members that include an `override` modifier are excluded from the set.</span></span>
*  <span data-ttu-id="08161-406">接下来，如果`K`为零，所有嵌套类型声明中包含的类型参数会删除的。</span><span class="sxs-lookup"><span data-stu-id="08161-406">Next, if `K` is zero, all nested types whose declarations include type parameters are removed.</span></span> <span data-ttu-id="08161-407">如果`K`不为零，具有不同数目的类型参数已删除的所有成员。</span><span class="sxs-lookup"><span data-stu-id="08161-407">If `K` is not zero, all members with a different number of type parameters are removed.</span></span> <span data-ttu-id="08161-408">请注意，当`K`零个、 方法具有的类型参数不会删除，因为类型推断过程 ([类型推理](expressions.md#type-inference)) 可能能够推断类型参数。</span><span class="sxs-lookup"><span data-stu-id="08161-408">Note that when `K` is zero, methods having type parameters are not removed, since the type inference process ([Type inference](expressions.md#type-inference)) might be able to infer the type arguments.</span></span>
*  <span data-ttu-id="08161-409">接下来，如果该成员是*调用*，所有非-*invocable*从集中删除成员。</span><span class="sxs-lookup"><span data-stu-id="08161-409">Next, if the member is *invoked*, all non-*invocable* members are removed from the set.</span></span>
*  <span data-ttu-id="08161-410">接下来，从集中移除其他成员隐藏的成员。</span><span class="sxs-lookup"><span data-stu-id="08161-410">Next, members that are hidden by other members are removed from the set.</span></span> <span data-ttu-id="08161-411">每个成员`S.M`在组中，其中`S`是在其中的类型成员 `M`声明时，将应用以下规则：</span><span class="sxs-lookup"><span data-stu-id="08161-411">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied:</span></span>
    * <span data-ttu-id="08161-412">如果`M`为常量、 字段、 属性、 事件或枚举成员，则所有成员的基类型中都声明`S`从集中删除。</span><span class="sxs-lookup"><span data-stu-id="08161-412">If `M` is a constant, field, property, event, or enumeration member, then all members declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="08161-413">如果`M`为的类型声明，则所有非类型的基类型中声明`S`从集合中移除和所有类型声明具有相同数量的类型参数作为`M`的基类型中声明`S`删除从组中。</span><span class="sxs-lookup"><span data-stu-id="08161-413">If `M` is a type declaration, then all non-types declared in a base type of `S` are removed from the set, and all type declarations with the same number of type parameters as `M` declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="08161-414">如果`M`是一种方法，然后在基类型中声明非方法的所有成员`S`从集中删除。</span><span class="sxs-lookup"><span data-stu-id="08161-414">If `M` is a method, then all non-method members declared in a base type of `S` are removed from the set.</span></span>
*  <span data-ttu-id="08161-415">接下来，是从集中移除接口成员的隐藏的类成员。</span><span class="sxs-lookup"><span data-stu-id="08161-415">Next, interface members that are hidden by class members are removed from the set.</span></span> <span data-ttu-id="08161-416">此步骤才有意义，如果`T`是一个类型参数和`T`而不具有这两个的有效基类`object`和设置的非空有效接口 ([类型参数约束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="08161-416">This step only has an effect if `T` is a type parameter and `T` has both an effective base class other than `object` and a non-empty effective interface set ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="08161-417">每个成员`S.M`在组中，其中`S`是在其中的类型成员`M`声明，如果应用以下规则`S`而不是类声明`object`:</span><span class="sxs-lookup"><span data-stu-id="08161-417">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied if `S` is a class declaration other than `object`:</span></span>
    * <span data-ttu-id="08161-418">如果`M`为常量、 字段、 属性、 事件、 枚举成员或类型声明，则从集中删除接口声明中声明的所有成员。</span><span class="sxs-lookup"><span data-stu-id="08161-418">If `M` is a constant, field, property, event, enumeration member, or type declaration, then all members declared in an interface declaration are removed from the set.</span></span>
    * <span data-ttu-id="08161-419">如果`M`是一种方法，则在接口声明中声明的所有非方法成员删除组，以及所有具有相同的签名方法从`M`声明在接口中声明从集合中移除。</span><span class="sxs-lookup"><span data-stu-id="08161-419">If `M` is a method, then all non-method members declared in an interface declaration are removed from the set, and all methods with the same signature as `M` declared in an interface declaration are removed from the set.</span></span>
*  <span data-ttu-id="08161-420">最后，删除隐藏的成员，确定查找的结果：</span><span class="sxs-lookup"><span data-stu-id="08161-420">Finally, having removed hidden members, the result of the lookup is determined:</span></span>
    * <span data-ttu-id="08161-421">如果集包含不是一种方法的单个成员，此成员是查找的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-421">If the set consists of a single member that is not a method, then this member is the result of the lookup.</span></span>
    * <span data-ttu-id="08161-422">否则，如果集包含只有方法，此组的方法是查找的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-422">Otherwise, if the set contains only methods, then this group of methods is the result of the lookup.</span></span>
    * <span data-ttu-id="08161-423">否则为查找不明确的并将绑定时出现错误。</span><span class="sxs-lookup"><span data-stu-id="08161-423">Otherwise, the lookup is ambiguous, and a binding-time error occurs.</span></span>

<span data-ttu-id="08161-424">为非类型参数和接口的类型中的成员查找和严格的单一继承的接口中的成员查找 （继承链中的每个接口都只有零个或一个直接基接口），查找规则的效果是只需派生出来的具有相同名称或签名的成员隐藏基成员。</span><span class="sxs-lookup"><span data-stu-id="08161-424">For member lookups in types other than type parameters and interfaces, and member lookups in interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effect of the lookup rules is simply that derived members hide base members with the same name or signature.</span></span> <span data-ttu-id="08161-425">这种单一继承查找会永远不会引起歧义。</span><span class="sxs-lookup"><span data-stu-id="08161-425">Such single-inheritance lookups are never ambiguous.</span></span> <span data-ttu-id="08161-426">中介绍了多个继承接口中的成员查找可能引起歧义[接口成员访问](interfaces.md#interface-member-access)。</span><span class="sxs-lookup"><span data-stu-id="08161-426">The ambiguities that can possibly arise from member lookups in multiple-inheritance interfaces are described in [Interface member access](interfaces.md#interface-member-access).</span></span>

### <a name="base-types"></a><span data-ttu-id="08161-427">基类型</span><span class="sxs-lookup"><span data-stu-id="08161-427">Base types</span></span>

<span data-ttu-id="08161-428">有关成员查找，一种类型的目的`T`被视为具有以下基本类型：</span><span class="sxs-lookup"><span data-stu-id="08161-428">For purposes of member lookup, a type `T` is considered to have the following base types:</span></span>

*  <span data-ttu-id="08161-429">如果`T`是`object`，然后`T`没有基类型。</span><span class="sxs-lookup"><span data-stu-id="08161-429">If `T` is `object`, then `T` has no base type.</span></span>
*  <span data-ttu-id="08161-430">如果`T`是*enum_type*，类型的基类型`T`是类类型`System.Enum`， `System.ValueType`，和`object`。</span><span class="sxs-lookup"><span data-stu-id="08161-430">If `T` is an *enum_type*, the base types of `T` are the class types `System.Enum`, `System.ValueType`, and `object`.</span></span>
*  <span data-ttu-id="08161-431">如果`T`是*struct_type*，类型的基类型`T`是类类型`System.ValueType`和`object`。</span><span class="sxs-lookup"><span data-stu-id="08161-431">If `T` is a *struct_type*, the base types of `T` are the class types `System.ValueType` and `object`.</span></span>
*  <span data-ttu-id="08161-432">如果`T`是*class_type*，类型的基类型`T`的基类`T`，其中包括类类型`object`。</span><span class="sxs-lookup"><span data-stu-id="08161-432">If `T` is a *class_type*, the base types of `T` are the base classes of `T`, including the class type `object`.</span></span>
*  <span data-ttu-id="08161-433">如果`T`是*interface_type*，类型的基类型`T`是接口的基`T`和类类型`object`。</span><span class="sxs-lookup"><span data-stu-id="08161-433">If `T` is an *interface_type*, the base types of `T` are the base interfaces of `T` and the class type `object`.</span></span>
*  <span data-ttu-id="08161-434">如果`T`是*array_type*，类型的基类型`T`是类类型`System.Array`和`object`。</span><span class="sxs-lookup"><span data-stu-id="08161-434">If `T` is an *array_type*, the base types of `T` are the class types `System.Array` and `object`.</span></span>
*  <span data-ttu-id="08161-435">如果`T`是*delegate_type*，类型的基类型`T`是类类型`System.Delegate`和`object`。</span><span class="sxs-lookup"><span data-stu-id="08161-435">If `T` is a *delegate_type*, the base types of `T` are the class types `System.Delegate` and `object`.</span></span>

## <a name="function-members"></a><span data-ttu-id="08161-436">函数成员</span><span class="sxs-lookup"><span data-stu-id="08161-436">Function members</span></span>

<span data-ttu-id="08161-437">函数成员是包含可执行语句的成员。</span><span class="sxs-lookup"><span data-stu-id="08161-437">Function members are members that contain executable statements.</span></span> <span data-ttu-id="08161-438">函数成员始终是类型的成员，并且不能为命名空间的成员。</span><span class="sxs-lookup"><span data-stu-id="08161-438">Function members are always members of types and cannot be members of namespaces.</span></span> <span data-ttu-id="08161-439">C# 定义函数成员的以下的类别：</span><span class="sxs-lookup"><span data-stu-id="08161-439">C# defines the following categories of function members:</span></span>

*  <span data-ttu-id="08161-440">方法</span><span class="sxs-lookup"><span data-stu-id="08161-440">Methods</span></span>
*  <span data-ttu-id="08161-441">属性</span><span class="sxs-lookup"><span data-stu-id="08161-441">Properties</span></span>
*  <span data-ttu-id="08161-442">事件</span><span class="sxs-lookup"><span data-stu-id="08161-442">Events</span></span>
*  <span data-ttu-id="08161-443">索引器</span><span class="sxs-lookup"><span data-stu-id="08161-443">Indexers</span></span>
*  <span data-ttu-id="08161-444">用户定义的运算符</span><span class="sxs-lookup"><span data-stu-id="08161-444">User-defined operators</span></span>
*  <span data-ttu-id="08161-445">实例构造函数</span><span class="sxs-lookup"><span data-stu-id="08161-445">Instance constructors</span></span>
*  <span data-ttu-id="08161-446">静态构造函数</span><span class="sxs-lookup"><span data-stu-id="08161-446">Static constructors</span></span>
*  <span data-ttu-id="08161-447">析构函数</span><span class="sxs-lookup"><span data-stu-id="08161-447">Destructors</span></span>

<span data-ttu-id="08161-448">除了析构函数和静态构造函数 （它们不能显式调用），函数成员中包含的语句执行通过函数成员调用。</span><span class="sxs-lookup"><span data-stu-id="08161-448">Except for destructors and static constructors (which cannot be invoked explicitly), the statements contained in function members are executed through function member invocations.</span></span> <span data-ttu-id="08161-449">编写一个函数成员调用的实际语法取决于特定函数成员类别。</span><span class="sxs-lookup"><span data-stu-id="08161-449">The actual syntax for writing a function member invocation depends on the particular function member category.</span></span>

<span data-ttu-id="08161-450">参数列表 ([自变量列表](expressions.md#argument-lists)) 的函数成员调用提供了实际值或变量引用的函数成员的参数。</span><span class="sxs-lookup"><span data-stu-id="08161-450">The argument list ([Argument lists](expressions.md#argument-lists)) of a function member invocation provides actual values or variable references for the parameters of the function member.</span></span>

<span data-ttu-id="08161-451">泛型方法的调用可能会使用类型推理来确定要传递给该方法的类型参数的集合。</span><span class="sxs-lookup"><span data-stu-id="08161-451">Invocations of generic methods may employ type inference to determine the set of type arguments to pass to the method.</span></span> <span data-ttu-id="08161-452">此过程所述[类型推理](expressions.md#type-inference)。</span><span class="sxs-lookup"><span data-stu-id="08161-452">This process is described in [Type inference](expressions.md#type-inference).</span></span>

<span data-ttu-id="08161-453">调用的方法、 索引器、 运算符和实例构造函数采用重载解析，以确定要调用的函数成员的候选项集。</span><span class="sxs-lookup"><span data-stu-id="08161-453">Invocations of methods, indexers, operators and instance constructors employ overload resolution to determine which of a candidate set of function members to invoke.</span></span> <span data-ttu-id="08161-454">此过程所述[重载决策](expressions.md#overload-resolution)。</span><span class="sxs-lookup"><span data-stu-id="08161-454">This process is described in [Overload resolution](expressions.md#overload-resolution).</span></span>

<span data-ttu-id="08161-455">一旦确定具体的函数成员在绑定时，可能是通过重载解析调用的函数成员的运行时的实际过程不中所述[编译时检查的动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="08161-455">Once a particular function member has been identified at binding-time, possibly through overload resolution, the actual run-time process of invoking the function member is described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="08161-456">下表总结了涉及可以显式调用的函数成员的六个类别的构造中进行处理。</span><span class="sxs-lookup"><span data-stu-id="08161-456">The following table summarizes the processing that takes place in constructs involving the six categories of function members that can be explicitly invoked.</span></span> <span data-ttu-id="08161-457">在表中， `e`， `x`， `y`，和`value`指示表达式分类为变量或值，`T`指示分类为一个类型的表达式`F`是简单的方法，并名称`P`是一个属性的简单名称。</span><span class="sxs-lookup"><span data-stu-id="08161-457">In the table, `e`, `x`, `y`, and `value` indicate expressions classified as variables or values, `T` indicates an expression classified as a type, `F` is the simple name of a method, and `P` is the simple name of a property.</span></span>


| <span data-ttu-id="08161-458">__构造__</span><span class="sxs-lookup"><span data-stu-id="08161-458">__Construct__</span></span>     | <span data-ttu-id="08161-459">__示例__</span><span class="sxs-lookup"><span data-stu-id="08161-459">__Example__</span></span>    | <span data-ttu-id="08161-460">__说明__</span><span class="sxs-lookup"><span data-stu-id="08161-460">__Description__</span></span> |
|-------------------|----------------|-----------------|
| <span data-ttu-id="08161-461">方法调用</span><span class="sxs-lookup"><span data-stu-id="08161-461">Method invocation</span></span> | `F(x,y)`       | <span data-ttu-id="08161-462">重载决策应用选择最佳方法`F`包含类或结构中。</span><span class="sxs-lookup"><span data-stu-id="08161-462">Overload resolution is applied to select the best method `F` in the containing class or struct.</span></span> <span data-ttu-id="08161-463">与参数列表调用该方法`(x,y)`。</span><span class="sxs-lookup"><span data-stu-id="08161-463">The method is invoked with the argument list `(x,y)`.</span></span> <span data-ttu-id="08161-464">如果此方法不是`static`，实例表达式是`this`。</span><span class="sxs-lookup"><span data-stu-id="08161-464">If the method is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.F(x,y)`     | <span data-ttu-id="08161-465">重载决策应用选择最佳的方法`F`类或结构中`T`。</span><span class="sxs-lookup"><span data-stu-id="08161-465">Overload resolution is applied to select the best method `F` in the class or struct `T`.</span></span> <span data-ttu-id="08161-466">如果该方法不会发生绑定时间错误`static`。</span><span class="sxs-lookup"><span data-stu-id="08161-466">A binding-time error occurs if the method is not `static`.</span></span> <span data-ttu-id="08161-467">与参数列表调用该方法`(x,y)`。</span><span class="sxs-lookup"><span data-stu-id="08161-467">The method is invoked with the argument list `(x,y)`.</span></span> | 
|                   | `e.F(x,y)`     | <span data-ttu-id="08161-468">重载决策被用于类、 结构或接口的类型的给定中选择最佳的方法 F `e`。</span><span class="sxs-lookup"><span data-stu-id="08161-468">Overload resolution is applied to select the best method F in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="08161-469">绑定时错误，则该方法是否`static`。</span><span class="sxs-lookup"><span data-stu-id="08161-469">A binding-time error occurs if the method is `static`.</span></span> <span data-ttu-id="08161-470">该方法调用的实例表达式`e`和参数列表`(x,y)`。</span><span class="sxs-lookup"><span data-stu-id="08161-470">The method is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="08161-471">和</span><span class="sxs-lookup"><span data-stu-id="08161-471">Property access</span></span>   | `P`            | <span data-ttu-id="08161-472">`get`属性访问器`P`调用中包含的类或结构。</span><span class="sxs-lookup"><span data-stu-id="08161-472">The `get` accessor of the property `P` in the containing class or struct is invoked.</span></span> <span data-ttu-id="08161-473">如果会发生编译时错误`P`是只写的。</span><span class="sxs-lookup"><span data-stu-id="08161-473">A compile-time error occurs if `P` is write-only.</span></span> <span data-ttu-id="08161-474">如果`P`不是`static`，实例表达式是`this`。</span><span class="sxs-lookup"><span data-stu-id="08161-474">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `P = value`    | <span data-ttu-id="08161-475">`set`属性访问器`P`包含类或结构中使用参数列表调用`(value)`。</span><span class="sxs-lookup"><span data-stu-id="08161-475">The `set` accessor of the property `P` in the containing class or struct is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="08161-476">如果会发生编译时错误`P`是只读的。</span><span class="sxs-lookup"><span data-stu-id="08161-476">A compile-time error occurs if `P` is read-only.</span></span> <span data-ttu-id="08161-477">如果`P`不是`static`，实例表达式是`this`。</span><span class="sxs-lookup"><span data-stu-id="08161-477">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.P`          | <span data-ttu-id="08161-478">`get`属性访问器`P`类或结构中`T`调用。</span><span class="sxs-lookup"><span data-stu-id="08161-478">The `get` accessor of the property `P` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="08161-479">如果会发生编译时错误`P`不是`static`或者如果`P`是只写的。</span><span class="sxs-lookup"><span data-stu-id="08161-479">A compile-time error occurs if `P` is not `static` or if `P` is write-only.</span></span> | 
|                   | `T.P = value`  | <span data-ttu-id="08161-480">`set`属性访问器`P`类或结构中`T`与参数列表调用`(value)`。</span><span class="sxs-lookup"><span data-stu-id="08161-480">The `set` accessor of the property `P` in the class or struct `T` is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="08161-481">如果会发生编译时错误`P`不是`static`或者如果`P`是只读的。</span><span class="sxs-lookup"><span data-stu-id="08161-481">A compile-time error occurs if `P` is not `static` or if `P` is read-only.</span></span> | 
|                   | `e.P`          | <span data-ttu-id="08161-482">`get`属性访问器`P`在类、 结构或接口的类型的给定`e`调用的实例表达式`e`。</span><span class="sxs-lookup"><span data-stu-id="08161-482">The `get` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="08161-483">如果绑定时间错误会发生`P`是`static`或者如果`P`是只写的。</span><span class="sxs-lookup"><span data-stu-id="08161-483">A binding-time error occurs if `P` is `static` or if `P` is write-only.</span></span> | 
|                   | `e.P = value`  | <span data-ttu-id="08161-484">`set`属性访问器`P`在类、 结构或接口的类型的给定`e`调用的实例表达式`e`和参数列表`(value)`。</span><span class="sxs-lookup"><span data-stu-id="08161-484">The `set` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e` and the argument list `(value)`.</span></span> <span data-ttu-id="08161-485">如果绑定时间错误会发生`P`是`static`或者如果`P`是只读的。</span><span class="sxs-lookup"><span data-stu-id="08161-485">A binding-time error occurs if `P` is `static` or if `P` is read-only.</span></span> | 
| <span data-ttu-id="08161-486">事件访问</span><span class="sxs-lookup"><span data-stu-id="08161-486">Event access</span></span>      | `E += value`   | <span data-ttu-id="08161-487">`add`事件访问器`E`调用中包含的类或结构。</span><span class="sxs-lookup"><span data-stu-id="08161-487">The `add` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="08161-488">如果`E`为非静态实例表达式是`this`。</span><span class="sxs-lookup"><span data-stu-id="08161-488">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `E -= value`   | <span data-ttu-id="08161-489">`remove`事件访问器`E`调用中包含的类或结构。</span><span class="sxs-lookup"><span data-stu-id="08161-489">The `remove` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="08161-490">如果`E`为非静态实例表达式是`this`。</span><span class="sxs-lookup"><span data-stu-id="08161-490">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `T.E += value` | <span data-ttu-id="08161-491">`add`事件访问器`E`类或结构中`T`调用。</span><span class="sxs-lookup"><span data-stu-id="08161-491">The `add` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="08161-492">绑定时错误，则如果`E`不是静态的。</span><span class="sxs-lookup"><span data-stu-id="08161-492">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `T.E -= value` | <span data-ttu-id="08161-493">`remove`事件访问器`E`类或结构中`T`调用。</span><span class="sxs-lookup"><span data-stu-id="08161-493">The `remove` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="08161-494">绑定时错误，则如果`E`不是静态的。</span><span class="sxs-lookup"><span data-stu-id="08161-494">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `e.E += value` | <span data-ttu-id="08161-495">`add`事件访问器`E`在类、 结构或接口的类型的给定`e`调用的实例表达式`e`。</span><span class="sxs-lookup"><span data-stu-id="08161-495">The `add` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="08161-496">绑定时错误，则如果`E`是静态的。</span><span class="sxs-lookup"><span data-stu-id="08161-496">A binding-time error occurs if `E` is static.</span></span> | 
|                   | `e.E -= value` | <span data-ttu-id="08161-497">`remove`事件访问器`E`在类、 结构或接口的类型的给定`e`调用的实例表达式`e`。</span><span class="sxs-lookup"><span data-stu-id="08161-497">The `remove` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="08161-498">绑定时错误，则如果`E`是静态的。</span><span class="sxs-lookup"><span data-stu-id="08161-498">A binding-time error occurs if `E` is static.</span></span> | 
| <span data-ttu-id="08161-499">索引器访问</span><span class="sxs-lookup"><span data-stu-id="08161-499">Indexer access</span></span>    | `e[x,y]`       | <span data-ttu-id="08161-500">重载决策被用于类、 结构或接口由 e 的类型中选择最佳索引器。</span><span class="sxs-lookup"><span data-stu-id="08161-500">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of e.</span></span> <span data-ttu-id="08161-501">`get`访问器的索引器的调用的实例表达式`e`和参数列表`(x,y)`。</span><span class="sxs-lookup"><span data-stu-id="08161-501">The `get` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> <span data-ttu-id="08161-502">如果索引器是只写，发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-502">A binding-time error occurs if the indexer is write-only.</span></span> | 
|                   | `e[x,y] = value` | <span data-ttu-id="08161-503">重载决策应用选择最佳索引器在类、 结构或接口的类型的给定`e`。</span><span class="sxs-lookup"><span data-stu-id="08161-503">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="08161-504">`set`访问器的索引器的调用的实例表达式`e`和参数列表`(x,y,value)`。</span><span class="sxs-lookup"><span data-stu-id="08161-504">The `set` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y,value)`.</span></span> <span data-ttu-id="08161-505">如果索引器是只读的会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-505">A binding-time error occurs if the indexer is read-only.</span></span> | 
| <span data-ttu-id="08161-506">运算符调用</span><span class="sxs-lookup"><span data-stu-id="08161-506">Operator invocation</span></span> | `-x`         | <span data-ttu-id="08161-507">重载决策应用选择最佳的一元运算符中的类或结构的类型的给定`x`。</span><span class="sxs-lookup"><span data-stu-id="08161-507">Overload resolution is applied to select the best unary operator in the class or struct given by the type of `x`.</span></span> <span data-ttu-id="08161-508">使用参数列表调用所选的运算符`(x)`。</span><span class="sxs-lookup"><span data-stu-id="08161-508">The selected operator is invoked with the argument list `(x)`.</span></span> | 
|                     | `x + y`      | <span data-ttu-id="08161-509">重载决策应用选择最佳的二元运算符中的类或结构类型的给定`x`和`y`。</span><span class="sxs-lookup"><span data-stu-id="08161-509">Overload resolution is applied to select the best binary operator in the classes or structs given by the types of `x` and `y`.</span></span> <span data-ttu-id="08161-510">使用参数列表调用所选的运算符`(x,y)`。</span><span class="sxs-lookup"><span data-stu-id="08161-510">The selected operator is invoked with the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="08161-511">实例构造函数调用</span><span class="sxs-lookup"><span data-stu-id="08161-511">Instance constructor invocation</span></span> | `new T(x,y)` | <span data-ttu-id="08161-512">重载决策应用选择最佳的实例构造函数中的类或结构`T`。</span><span class="sxs-lookup"><span data-stu-id="08161-512">Overload resolution is applied to select the best instance constructor in the class or struct `T`.</span></span> <span data-ttu-id="08161-513">实例构造函数调用具有参数列表`(x,y)`。</span><span class="sxs-lookup"><span data-stu-id="08161-513">The instance constructor is invoked with the argument list `(x,y)`.</span></span> | 

### <a name="argument-lists"></a><span data-ttu-id="08161-514">自变量列表</span><span class="sxs-lookup"><span data-stu-id="08161-514">Argument lists</span></span>

<span data-ttu-id="08161-515">每个函数成员和委托调用包括一个自变量列表，其中提供的函数成员的参数的实际值或变量引用。</span><span class="sxs-lookup"><span data-stu-id="08161-515">Every function member and delegate invocation includes an argument list which provides actual values or variable references for the parameters of the function member.</span></span> <span data-ttu-id="08161-516">指定的函数成员调用的参数列表的语法取决于函数成员类别：</span><span class="sxs-lookup"><span data-stu-id="08161-516">The syntax for specifying the argument list of a function member invocation depends on the function member category:</span></span>

*  <span data-ttu-id="08161-517">实例构造函数、 方法、 索引器和委托，则参数被指定为*argument_list*，如下所述。</span><span class="sxs-lookup"><span data-stu-id="08161-517">For instance constructors, methods, indexers and delegates, the arguments are specified as an *argument_list*, as described below.</span></span> <span data-ttu-id="08161-518">对于索引器，在调用时`set`访问器，自变量列表此外还包括为赋值运算符的右操作数指定的表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-518">For indexers, when invoking the `set` accessor, the argument list additionally includes the expression specified as the right operand of the assignment operator.</span></span>
*  <span data-ttu-id="08161-519">对于属性，该参数列表为空时调用`get`访问器，并由调用时指定为赋值运算符的右操作数的表达式组成`set`访问器。</span><span class="sxs-lookup"><span data-stu-id="08161-519">For properties, the argument list is empty when invoking the `get` accessor, and consists of the expression specified as the right operand of the assignment operator when invoking the `set` accessor.</span></span>
*  <span data-ttu-id="08161-520">对于事件，参数列表包含指定为右操作数的表达式`+=`或`-=`运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-520">For events, the argument list consists of the expression specified as the right operand of the `+=` or `-=` operator.</span></span>
*  <span data-ttu-id="08161-521">对于用户定义的运算符，参数列表包含的单个运算符的操作数的一元或二元运算符的两个操作数。</span><span class="sxs-lookup"><span data-stu-id="08161-521">For user-defined operators, the argument list consists of the single operand of the unary operator or the two operands of the binary operator.</span></span>

<span data-ttu-id="08161-522">属性的参数 ([属性](classes.md#properties))，事件 ([事件](classes.md#events))，和用户定义的运算符 ([运算符](classes.md#operators)) 始终作为值参数传递 ([值参数](classes.md#value-parameters))。</span><span class="sxs-lookup"><span data-stu-id="08161-522">The arguments of properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), and user-defined operators ([Operators](classes.md#operators)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)).</span></span> <span data-ttu-id="08161-523">索引器的自变量 ([索引器](classes.md#indexers)) 始终作为值参数传递 ([值参数](classes.md#value-parameters)) 或参数数组 ([参数数组](classes.md#parameter-arrays))。</span><span class="sxs-lookup"><span data-stu-id="08161-523">The arguments of indexers ([Indexers](classes.md#indexers)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)) or parameter arrays ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="08161-524">不支持对这些类别的函数成员的引用和输出参数。</span><span class="sxs-lookup"><span data-stu-id="08161-524">Reference and output parameters are not supported for these categories of function members.</span></span>

<span data-ttu-id="08161-525">实例构造函数、 方法、 索引器或委托调用的参数被指定为*argument_list*:</span><span class="sxs-lookup"><span data-stu-id="08161-525">The arguments of an instance constructor, method, indexer or delegate invocation are specified as an *argument_list*:</span></span>

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

<span data-ttu-id="08161-526">*Argument_list*包含一个或多个*自变量*s，由逗号分隔。</span><span class="sxs-lookup"><span data-stu-id="08161-526">An *argument_list* consists of one or more *argument*s, separated by commas.</span></span> <span data-ttu-id="08161-527">每个自变量组成的可选*argument_name*跟*argument_value*。</span><span class="sxs-lookup"><span data-stu-id="08161-527">Each argument consists of an optional  *argument_name* followed by an *argument_value*.</span></span> <span data-ttu-id="08161-528">*自变量*与*argument_name*称为***命名参数***，而*参数*而无需*argument_name*是***位置自变量***。</span><span class="sxs-lookup"><span data-stu-id="08161-528">An *argument* with an *argument_name* is referred to as a ***named argument***, whereas an *argument* without an *argument_name* is a ***positional argument***.</span></span> <span data-ttu-id="08161-529">它是错误的位置自变量中的命名参数后出现*argument_list*。</span><span class="sxs-lookup"><span data-stu-id="08161-529">It is an error for a positional argument to appear after a named argument in an *argument_list*.</span></span>

<span data-ttu-id="08161-530">*Argument_value*可以采用以下形式之一：</span><span class="sxs-lookup"><span data-stu-id="08161-530">The *argument_value* can take one of the following forms:</span></span>

*  <span data-ttu-id="08161-531">*表达式*，指示作为值参数传递自变量 ([值参数](classes.md#value-parameters))。</span><span class="sxs-lookup"><span data-stu-id="08161-531">An *expression*, indicating that the argument is passed as a value parameter ([Value parameters](classes.md#value-parameters)).</span></span>
*  <span data-ttu-id="08161-532">关键字`ref`跟*variable_reference* ([变量引用](variables.md#variable-references))，指示作为引用参数传递参数 ([引用参数](classes.md#reference-parameters)).</span><span class="sxs-lookup"><span data-stu-id="08161-532">The keyword `ref` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as a reference parameter ([Reference parameters](classes.md#reference-parameters)).</span></span> <span data-ttu-id="08161-533">变量必须明确赋值 ([明确赋值](variables.md#definite-assignment)) 它可以作为引用参数传递之前。</span><span class="sxs-lookup"><span data-stu-id="08161-533">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter.</span></span> <span data-ttu-id="08161-534">关键字`out`跟*variable_reference* ([变量引用](variables.md#variable-references))，指示作为输出参数传递参数 ([输出参数](classes.md#output-parameters)).</span><span class="sxs-lookup"><span data-stu-id="08161-534">The keyword `out` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as an output parameter ([Output parameters](classes.md#output-parameters)).</span></span> <span data-ttu-id="08161-535">被视为一个变量明确赋值 ([明确赋值](variables.md#definite-assignment)) 在该变量传递作为输出参数的函数成员调用之后。</span><span class="sxs-lookup"><span data-stu-id="08161-535">A variable is considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) following a function member invocation in which the variable is passed as an output parameter.</span></span>

#### <a name="corresponding-parameters"></a><span data-ttu-id="08161-536">相应的参数</span><span class="sxs-lookup"><span data-stu-id="08161-536">Corresponding parameters</span></span>

<span data-ttu-id="08161-537">对于每个自变量参数列表中必须有委托调用的函数成员中的相应参数。</span><span class="sxs-lookup"><span data-stu-id="08161-537">For each argument in an argument list there has to be a corresponding parameter in the function member or delegate being invoked.</span></span>

<span data-ttu-id="08161-538">在下面使用的参数列表中，如下所示确定：</span><span class="sxs-lookup"><span data-stu-id="08161-538">The parameter list used in the following is determined as follows:</span></span>

*  <span data-ttu-id="08161-539">对于虚拟方法和类中定义的索引器，参数列表从最具体的声明中选取或重写的函数成员，开头的静态类型的接收方，并通过其基类，这些类搜索。</span><span class="sxs-lookup"><span data-stu-id="08161-539">For virtual methods and indexers defined in classes, the parameter list is picked from the most specific declaration or override of the function member, starting with the static type of the receiver, and searching through its base classes.</span></span>
*  <span data-ttu-id="08161-540">对于接口方法和索引器，选择的参数列表窗体中的成员，从接口类型和搜索的基接口中的最具体的定义。</span><span class="sxs-lookup"><span data-stu-id="08161-540">For interface methods and indexers, the parameter list is picked form the most specific definition of the member, starting with the interface type and searching through the base interfaces.</span></span> <span data-ttu-id="08161-541">如果找到任何唯一的参数列表，构造具有不可访问的名称和任何可选参数的参数列表，以便调用不能使用命名的参数或省略可选参数。</span><span class="sxs-lookup"><span data-stu-id="08161-541">If no unique parameter list is found, a parameter list with inaccessible names and no optional parameters is constructed, so that invocations cannot use named parameters or omit optional arguments.</span></span>
*  <span data-ttu-id="08161-542">对于分部方法，使用定义的分部方法声明的参数列表。</span><span class="sxs-lookup"><span data-stu-id="08161-542">For partial methods, the parameter list of the defining partial method declaration is used.</span></span>
*  <span data-ttu-id="08161-543">对于所有其他函数成员和委托没有仅一个单一参数列表，即使用。</span><span class="sxs-lookup"><span data-stu-id="08161-543">For all other function members and delegates there is only a single parameter list, which is the one used.</span></span>

<span data-ttu-id="08161-544">自变量或参数的位置定义为参数的个数或参数列表或参数列表中前面的参数。</span><span class="sxs-lookup"><span data-stu-id="08161-544">The position of an argument or parameter is defined as the number of arguments or parameters preceding it in the argument list or parameter list.</span></span>

<span data-ttu-id="08161-545">建立函数成员参数的相应参数，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-545">The corresponding parameters for function member arguments are established as follows:</span></span>

*  <span data-ttu-id="08161-546">中的自变量*argument_list*的实例构造函数、 方法、 索引器和委托：</span><span class="sxs-lookup"><span data-stu-id="08161-546">Arguments in the *argument_list* of instance constructors, methods, indexers and delegates:</span></span>
    * <span data-ttu-id="08161-547">固定的参数出现在参数列表中的相同位置的位置自变量对应于该参数。</span><span class="sxs-lookup"><span data-stu-id="08161-547">A positional argument where a fixed parameter occurs at the same position in the parameter list corresponds to that parameter.</span></span>
    * <span data-ttu-id="08161-548">以普通形式调用的参数数组的函数成员的位置自变量对应于参数数组，必须在参数列表中的同一位置。</span><span class="sxs-lookup"><span data-stu-id="08161-548">A positional argument of a function member with a parameter array invoked in its normal form corresponds to the parameter  array, which must occur at the same position in the parameter list.</span></span>
    * <span data-ttu-id="08161-549">带有以扩展形式，其中没有固定的参数出现在参数列表中的同一位置中，调用的参数数组的函数成员的位置自变量对应于参数数组中的元素。</span><span class="sxs-lookup"><span data-stu-id="08161-549">A positional argument of a function member with a parameter array invoked in its expanded form, where no fixed parameter occurs at the same position in the parameter list, corresponds to an element in the parameter array.</span></span>
    * <span data-ttu-id="08161-550">命名的参数对应于参数列表中的相同名称的参数。</span><span class="sxs-lookup"><span data-stu-id="08161-550">A named argument corresponds to the parameter of the same name in the parameter list.</span></span>
    * <span data-ttu-id="08161-551">对于索引器，在调用时`set`访问器，指定为赋值运算符的右操作数对应的隐式表达式`value`参数的`set`访问器声明。</span><span class="sxs-lookup"><span data-stu-id="08161-551">For indexers, when invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="08161-552">对于属性，调用时`get`有访问器都没有自变量。</span><span class="sxs-lookup"><span data-stu-id="08161-552">For properties, when invoking the `get` accessor there are no arguments.</span></span> <span data-ttu-id="08161-553">调用时`set`访问器，指定为赋值运算符的右操作数对应的隐式表达式`value`参数的`set`访问器声明。</span><span class="sxs-lookup"><span data-stu-id="08161-553">When invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="08161-554">对于用户定义的一元运算符 （包括转换），单个操作数与运算符声明的单个参数相对应。</span><span class="sxs-lookup"><span data-stu-id="08161-554">For user-defined unary operators (including conversions), the single operand corresponds to the single parameter of the operator declaration.</span></span>
*  <span data-ttu-id="08161-555">对于用户定义二元运算符，对应于第一个参数的左的操作数和右操作数与运算符声明的第二个参数相对应。</span><span class="sxs-lookup"><span data-stu-id="08161-555">For user-defined binary operators, the left operand corresponds to the first parameter, and the right operand corresponds to the second parameter of the operator declaration.</span></span>

#### <a name="run-time-evaluation-of-argument-lists"></a><span data-ttu-id="08161-556">运行时计算的自变量列表</span><span class="sxs-lookup"><span data-stu-id="08161-556">Run-time evaluation of argument lists</span></span>

<span data-ttu-id="08161-557">函数成员调用的运行时处理过程中 ([进行编译时检查的动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution))，表达式或变量引用自变量列表的顺序从左到右，评估为如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-557">During the run-time processing of a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), the expressions or variable references of an argument list are evaluated in order, from left to right, as follows:</span></span>

*  <span data-ttu-id="08161-558">对于值参数，参数表达式求值的隐式转换和 ([隐式转换](conversions.md#implicit-conversions)) 为相应的参数执行类型。</span><span class="sxs-lookup"><span data-stu-id="08161-558">For a value parameter, the argument expression is evaluated and an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to the corresponding parameter type is performed.</span></span> <span data-ttu-id="08161-559">生成的值将成为该函数成员的调用中的值参数的初始值。</span><span class="sxs-lookup"><span data-stu-id="08161-559">The resulting value becomes the initial value of the value parameter in the function member invocation.</span></span>
*  <span data-ttu-id="08161-560">对于引用或输出参数，计算变量的引用和生成的存储位置将成为中函数成员调用的参数所表示的存储位置。</span><span class="sxs-lookup"><span data-stu-id="08161-560">For a reference or output parameter, the variable reference is evaluated and the resulting storage location becomes the storage location represented by the parameter in the function member invocation.</span></span> <span data-ttu-id="08161-561">如果引用或输出参数中给出的变量引用的数组元素*reference_type*，将运行时检查以确保数组的元素类型与参数的类型相同。</span><span class="sxs-lookup"><span data-stu-id="08161-561">If the variable reference given as a reference or output parameter is an array element of a *reference_type*, a run-time check is performed to ensure that the element type of the array is identical to the type of the parameter.</span></span> <span data-ttu-id="08161-562">如果此检查失败，`System.ArrayTypeMismatchException`引发。</span><span class="sxs-lookup"><span data-stu-id="08161-562">If this check fails, a `System.ArrayTypeMismatchException` is thrown.</span></span>

<span data-ttu-id="08161-563">方法、 索引器和实例构造函数可以声明为一个参数数组，其最右边的参数 ([参数数组](classes.md#parameter-arrays))。</span><span class="sxs-lookup"><span data-stu-id="08161-563">Methods, indexers, and instance constructors may declare their right-most parameter to be a parameter array ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="08161-564">此类函数成员调用其正常形式或以其具体取决于所适用的扩展形式 ([适用函数成员](expressions.md#applicable-function-member)):</span><span class="sxs-lookup"><span data-stu-id="08161-564">Such function members are invoked either in their normal form or in their expanded form depending on which is applicable ([Applicable function member](expressions.md#applicable-function-member)):</span></span>

*  <span data-ttu-id="08161-565">在最普通的形式调用带有参数数组的函数成员时，提供的参数数组的参数必须是一个隐式转换的表达式 ([隐式转换](conversions.md#implicit-conversions)) 为参数数组类型。</span><span class="sxs-lookup"><span data-stu-id="08161-565">When a function member with a parameter array is invoked in its normal form, the argument given for the parameter array must be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="08161-566">在这种情况下，参数数组的作用与值参数完全一样。</span><span class="sxs-lookup"><span data-stu-id="08161-566">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="08161-567">调用以展开形式调用带有参数数组的函数成员时，必须指定参数数组，其中每个参数都是隐式转换的表达式的零个或多个位置自变量 ([隐式转换](conversions.md#implicit-conversions)) 为参数数组的元素类型。</span><span class="sxs-lookup"><span data-stu-id="08161-567">When a function member with a parameter array is invoked in its expanded form, the invocation must specify zero or more positional arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="08161-568">在这种情况下，调用创建具有对应的参数的数目的长度参数数组类型的实例、 初始化具有给定的参数值的数组实例的元素并将新创建的数组实例用作实际自变量。</span><span class="sxs-lookup"><span data-stu-id="08161-568">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="08161-569">写入时的顺序始终计算自变量列表的表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-569">The expressions of an argument list are always evaluated in the order they are written.</span></span> <span data-ttu-id="08161-570">因此，示例</span><span class="sxs-lookup"><span data-stu-id="08161-570">Thus, the example</span></span>
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
<span data-ttu-id="08161-571">生成输出</span><span class="sxs-lookup"><span data-stu-id="08161-571">produces the output</span></span>
```
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

<span data-ttu-id="08161-572">数组协变规则 ([数组协方差](arrays.md#array-covariance)) 允许使用数组类型的值`A[]`是对的数组类型实例的引用`B[]`，则从存在的隐式引用转换`B`到`A`.</span><span class="sxs-lookup"><span data-stu-id="08161-572">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="08161-573">由于这些规则的数组元素时*reference_type*传递作为引用或输出参数，运行时检查是需要确保数组的实际元素类型是相同的参数。</span><span class="sxs-lookup"><span data-stu-id="08161-573">Because of these rules, when an array element of a *reference_type* is passed as a reference or output parameter, a run-time check is required to ensure that the actual element type of the array is identical to that of the parameter.</span></span> <span data-ttu-id="08161-574">在示例</span><span class="sxs-lookup"><span data-stu-id="08161-574">In the example</span></span>
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
<span data-ttu-id="08161-575">第二次调用`F`会导致`System.ArrayTypeMismatchException`因为实际的元素类型的引发`b`是`string`而不`object`。</span><span class="sxs-lookup"><span data-stu-id="08161-575">the second invocation of `F` causes a `System.ArrayTypeMismatchException` to be thrown because the actual element type of `b` is `string` and not `object`.</span></span>

<span data-ttu-id="08161-576">正如使用数组初始值设定项的数组创建表达式以展开形式调用带有参数数组的函数成员时，处理调用 ([数组创建表达式](expressions.md#array-creation-expressions)) 周围插入扩展的参数。</span><span class="sxs-lookup"><span data-stu-id="08161-576">When a function member with a parameter array is invoked in its expanded form, the invocation is processed exactly as if an array creation expression with an array initializer ([Array creation expressions](expressions.md#array-creation-expressions)) was inserted around the expanded parameters.</span></span> <span data-ttu-id="08161-577">例如，给定声明</span><span class="sxs-lookup"><span data-stu-id="08161-577">For example, given the declaration</span></span>
```csharp
void F(int x, int y, params object[] args);
```
<span data-ttu-id="08161-578">该方法的展开形式的以下调用</span><span class="sxs-lookup"><span data-stu-id="08161-578">the following invocations of the expanded form of the method</span></span>
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
<span data-ttu-id="08161-579">完全对应于</span><span class="sxs-lookup"><span data-stu-id="08161-579">correspond exactly to</span></span>
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

<span data-ttu-id="08161-580">具体而言，请注意有零个参数提供的参数数组时，创建一个空数组。</span><span class="sxs-lookup"><span data-stu-id="08161-580">In particular, note that an empty array is created when there are zero arguments given for the parameter array.</span></span>

<span data-ttu-id="08161-581">当从带有相应的可选参数的函数成员省略参数时，隐式传递函数成员声明的默认参数。</span><span class="sxs-lookup"><span data-stu-id="08161-581">When arguments are omitted from a function member with corresponding optional parameters, the default arguments of the function member declaration are implicitly passed.</span></span> <span data-ttu-id="08161-582">这些始终是常量，因为其评估不会影响其余的参数的计算顺序。</span><span class="sxs-lookup"><span data-stu-id="08161-582">Because these are always constant, their evaluation will not impact the evaluation order of the remaining arguments.</span></span>

### <a name="type-inference"></a><span data-ttu-id="08161-583">类型推理</span><span class="sxs-lookup"><span data-stu-id="08161-583">Type inference</span></span>

<span data-ttu-id="08161-584">不指定类型参数的情况下调用泛型方法时***类型推理***进程将尝试推断调用的类型参数。</span><span class="sxs-lookup"><span data-stu-id="08161-584">When a generic method is called without specifying type arguments, a ***type inference*** process attempts to infer type arguments for the call.</span></span> <span data-ttu-id="08161-585">类型推断存在允许更方便的语法，用于调用泛型方法，并允许程序员不必指定冗余类型信息。</span><span class="sxs-lookup"><span data-stu-id="08161-585">The presence of type inference allows a more convenient syntax to be used for calling a generic method, and allows the programmer to avoid specifying redundant type information.</span></span> <span data-ttu-id="08161-586">例如，给定方法声明：</span><span class="sxs-lookup"><span data-stu-id="08161-586">For example, given the method declaration:</span></span>
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
<span data-ttu-id="08161-587">可以调用`Choose`而无需显式指定类型参数的方法：</span><span class="sxs-lookup"><span data-stu-id="08161-587">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

<span data-ttu-id="08161-588">通过类型推断的类型实参`int`和`string`将根据对方法的参数。</span><span class="sxs-lookup"><span data-stu-id="08161-588">Through type inference, the type arguments `int` and `string` are determined from the arguments to the method.</span></span>

<span data-ttu-id="08161-589">作为方法调用的绑定时处理的一部分，会发生类型推理 ([方法调用](expressions.md#method-invocations)) 和调用的重载解决方法步骤之前发生。</span><span class="sxs-lookup"><span data-stu-id="08161-589">Type inference occurs as part of the binding-time processing of a method invocation ([Method invocations](expressions.md#method-invocations)) and takes place before the overload resolution step of the invocation.</span></span> <span data-ttu-id="08161-590">在方法调用中，指定某一特定方法组并作为方法调用的一部分未指定任何类型参数，类型推理被应用于方法组中每个泛型方法。</span><span class="sxs-lookup"><span data-stu-id="08161-590">When a particular method group is specified in a method invocation, and no type arguments are specified as part of the method invocation, type inference is applied to each generic method in the method group.</span></span> <span data-ttu-id="08161-591">如果类型推理成功，然后推断的类型参数用于确定后续重载解决方法的参数的类型。</span><span class="sxs-lookup"><span data-stu-id="08161-591">If type inference succeeds, then the inferred type arguments are used to determine the types of arguments for subsequent overload resolution.</span></span> <span data-ttu-id="08161-592">如果重载决策选择与要调用泛型方法，然后推断的类型参数用作调用的实际类型参数。</span><span class="sxs-lookup"><span data-stu-id="08161-592">If overload resolution chooses a generic method as the one to invoke, then the inferred type arguments are used as the actual type arguments for the invocation.</span></span> <span data-ttu-id="08161-593">如果使用的特定方法的类型推理失败，该方法将不参与重载决策。</span><span class="sxs-lookup"><span data-stu-id="08161-593">If type inference for a particular method fails, that method does not participate in overload resolution.</span></span> <span data-ttu-id="08161-594">失败的类型推断，本身并不会导致绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-594">The failure of type inference, in and of itself, does not cause a binding-time error.</span></span> <span data-ttu-id="08161-595">但是，它通常会导致绑定时错误时重载决策然后未能找到任何适用的方法。</span><span class="sxs-lookup"><span data-stu-id="08161-595">However, it often leads to a binding-time error when overload resolution then fails to find any applicable methods.</span></span>

<span data-ttu-id="08161-596">如果提供的参数数目与方法中的参数的数目不同，则推断立即失败。</span><span class="sxs-lookup"><span data-stu-id="08161-596">If the supplied number of arguments is different than the number of parameters in the method, then inference immediately fails.</span></span> <span data-ttu-id="08161-597">否则，假定该泛型方法具有以下签名：</span><span class="sxs-lookup"><span data-stu-id="08161-597">Otherwise, assume that the generic method has the following signature:</span></span>
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

<span data-ttu-id="08161-598">使用窗体的方法调用`M(E1...Em)`类型推断的任务是查找唯一类型参数`S1...Sn`为每个类型参数`X1...Xn`，以便在调用`M<S1...Sn>(E1...Em)`开始生效。</span><span class="sxs-lookup"><span data-stu-id="08161-598">With a method call of the form `M(E1...Em)` the task of type inference is to find unique type arguments `S1...Sn` for each of the type parameters `X1...Xn` so that the call `M<S1...Sn>(E1...Em)` becomes valid.</span></span>

<span data-ttu-id="08161-599">推理过程中，每个类型参数`Xi`可以是*修复*为特定类型`Si`或*非固定*具有一组关联的*边界*.</span><span class="sxs-lookup"><span data-stu-id="08161-599">During the process of inference each type parameter `Xi` is either *fixed* to a particular type `Si` or *unfixed* with an associated set of *bounds*.</span></span> <span data-ttu-id="08161-600">每个边界都是某种类型`T`。</span><span class="sxs-lookup"><span data-stu-id="08161-600">Each of the bounds is some type `T`.</span></span> <span data-ttu-id="08161-601">最初的每个类型变量`Xi`是未固定的边界为空集。</span><span class="sxs-lookup"><span data-stu-id="08161-601">Initially each type variable `Xi` is unfixed with an empty set of bounds.</span></span>

<span data-ttu-id="08161-602">类型推理分阶段进行。</span><span class="sxs-lookup"><span data-stu-id="08161-602">Type inference takes place in phases.</span></span> <span data-ttu-id="08161-603">每个阶段将尝试推断根据上一阶段发现更多类型的变量的类型参数。</span><span class="sxs-lookup"><span data-stu-id="08161-603">Each phase will try to infer type arguments for more type variables based on the findings of the previous phase.</span></span> <span data-ttu-id="08161-604">第一阶段可指定边界内的某些初始推断，而第二个阶段解决了为特定类型的类型变量并进一步推断边界。</span><span class="sxs-lookup"><span data-stu-id="08161-604">The first phase makes some initial inferences of bounds, whereas the second phase fixes type variables to specific types and infers further bounds.</span></span> <span data-ttu-id="08161-605">第二个阶段都必须重复次数的时间。</span><span class="sxs-lookup"><span data-stu-id="08161-605">The second phase may have to be repeated a number of times.</span></span>

<span data-ttu-id="08161-606">*注意：* 类型推理，将发生不只调用泛型方法。</span><span class="sxs-lookup"><span data-stu-id="08161-606">*Note:* Type inference takes place not only when a generic method is called.</span></span> <span data-ttu-id="08161-607">中介绍了为方法组转换的类型推理[类型推理为方法组转换](expressions.md#type-inference-for-conversion-of-method-groups)和中所述查找一组表达式的最常见类型[查找一组的最常见类型表达式的](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)。</span><span class="sxs-lookup"><span data-stu-id="08161-607">Type inference for conversion of method groups is described in [Type inference for conversion of method groups](expressions.md#type-inference-for-conversion-of-method-groups) and finding the best common type of a set of expressions is described in [Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span></span>

#### <a name="the-first-phase"></a><span data-ttu-id="08161-608">第一阶段</span><span class="sxs-lookup"><span data-stu-id="08161-608">The first phase</span></span>

<span data-ttu-id="08161-609">为每个方法自变量`Ei`:</span><span class="sxs-lookup"><span data-stu-id="08161-609">For each of the method arguments `Ei`:</span></span>

*   <span data-ttu-id="08161-610">如果`Ei`是一个匿名函数*显式参数类型推断*([显式参数类型推断](expressions.md#explicit-parameter-type-inferences)) 从进行`Ei`到 `Ti`</span><span class="sxs-lookup"><span data-stu-id="08161-610">If `Ei` is an anonymous function, an *explicit parameter type inference* ([Explicit parameter type inferences](expressions.md#explicit-parameter-type-inferences)) is made from `Ei` to `Ti`</span></span>
*   <span data-ttu-id="08161-611">否则为如果`Ei`具有类型`U`和`xi`是一个值参数，则*下限推断*由*从* `U` *到*`Ti`.</span><span class="sxs-lookup"><span data-stu-id="08161-611">Otherwise, if `Ei` has a type `U` and `xi` is a value parameter then a *lower-bound inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="08161-612">否则为如果`Ei`具有类型`U`并`xi`是`ref`或`out`参数则*精确推理*由*从* `U`*到* `Ti`。</span><span class="sxs-lookup"><span data-stu-id="08161-612">Otherwise, if `Ei` has a type `U` and `xi` is a `ref` or `out` parameter then an *exact inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="08161-613">否则，不进行推断。 为此参数。</span><span class="sxs-lookup"><span data-stu-id="08161-613">Otherwise, no inference is made for this argument.</span></span>


#### <a name="the-second-phase"></a><span data-ttu-id="08161-614">第二个阶段</span><span class="sxs-lookup"><span data-stu-id="08161-614">The second phase</span></span>

<span data-ttu-id="08161-615">第二个阶段将继续，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-615">The second phase proceeds as follows:</span></span>

*   <span data-ttu-id="08161-616">所有*非固定*类型变量`Xi`哪些则不*依赖于*([依赖](expressions.md#dependence)) 任何`Xj`固定的 ([正在修复](expressions.md#fixing)).</span><span class="sxs-lookup"><span data-stu-id="08161-616">All *unfixed* type variables `Xi` which do not *depend on* ([Dependence](expressions.md#dependence)) any `Xj` are fixed ([Fixing](expressions.md#fixing)).</span></span>
*   <span data-ttu-id="08161-617">如果没有此类类型变量存在，所有*非固定*类型变量`Xi`是*修复*为其所有以下保存：</span><span class="sxs-lookup"><span data-stu-id="08161-617">If no such type variables exist, all *unfixed* type variables `Xi` are *fixed* for which all of the following hold:</span></span>
    *   <span data-ttu-id="08161-618">至少一个类型变量`Xj`这取决于 `Xi`</span><span class="sxs-lookup"><span data-stu-id="08161-618">There is at least one type variable `Xj` that depends on `Xi`</span></span>
    *   <span data-ttu-id="08161-619">`Xi` 具有非空集合的界限</span><span class="sxs-lookup"><span data-stu-id="08161-619">`Xi` has a non-empty set of bounds</span></span>
*   <span data-ttu-id="08161-620">如果存在任何此类类型的变量，并且仍有*非固定*类型变量、 类型推断将失败。</span><span class="sxs-lookup"><span data-stu-id="08161-620">If no such type variables exist and there are still *unfixed* type variables, type inference fails.</span></span>
*   <span data-ttu-id="08161-621">否则为如果无法进一步*非固定*类型变量存在，则类型推理成功。</span><span class="sxs-lookup"><span data-stu-id="08161-621">Otherwise, if no further *unfixed* type variables exist, type inference succeeds.</span></span>
*   <span data-ttu-id="08161-622">否则，为所有参数`Ei`相应的参数类型与`Ti`其中*输出类型*([输出类型](expressions.md#output-types)) 包含*非固定*键入变量`Xj`但*输入类型*([输入类型](expressions.md#input-types)) 不会这样，*输出类型推理*([输出类型推断](expressions.md#output-type-inferences)) 进行*从* `Ei` *到* `Ti`。</span><span class="sxs-lookup"><span data-stu-id="08161-622">Otherwise, for all arguments `Ei` with corresponding parameter type `Ti` where the *output types* ([Output types](expressions.md#output-types)) contain *unfixed* type variables `Xj` but the *input types* ([Input types](expressions.md#input-types)) do not, an *output type inference* ([Output type inferences](expressions.md#output-type-inferences)) is made *from* `Ei` *to* `Ti`.</span></span> <span data-ttu-id="08161-623">然后，重复执行第二个阶段。</span><span class="sxs-lookup"><span data-stu-id="08161-623">Then the second phase is repeated.</span></span>

#### <a name="input-types"></a><span data-ttu-id="08161-624">输入的类型</span><span class="sxs-lookup"><span data-stu-id="08161-624">Input types</span></span>

<span data-ttu-id="08161-625">如果`E`是一个方法组或隐式类型的匿名函数和`T`类型或表达式树类型则的所有参数类型是一个委托`T`是*输入类型*的`E` *具有类型* `T`。</span><span class="sxs-lookup"><span data-stu-id="08161-625">If `E` is a method group or implicitly typed anonymous function and `T` is a delegate type or expression tree type then all the parameter types of `T` are *input types* of `E` *with type* `T`.</span></span>

####  <a name="output-types"></a><span data-ttu-id="08161-626">输出类型</span><span class="sxs-lookup"><span data-stu-id="08161-626">Output types</span></span>

<span data-ttu-id="08161-627">如果`E`是一个方法组或匿名函数和`T`类型或表达式树类型然后的返回类型是一个委托`T`是*输出的类型* `E` *与类型* `T`.</span><span class="sxs-lookup"><span data-stu-id="08161-627">If `E` is a method group or an anonymous function and `T` is a delegate type or expression tree type then the return type of `T` is an *output type of* `E` *with type* `T`.</span></span>

#### <a name="dependence"></a><span data-ttu-id="08161-628">依赖关系</span><span class="sxs-lookup"><span data-stu-id="08161-628">Dependence</span></span>

<span data-ttu-id="08161-629">*非固定*类型的变量`Xi`*直接依赖于*固定的类型变量`Xj`如果某些参数`Ek`类型`Tk` `Xj`中发生*输入类型*的`Ek`与类型`Tk`并`Xi`中发生*输出类型*的`Ek`类型`Tk`。</span><span class="sxs-lookup"><span data-stu-id="08161-629">An *unfixed* type variable `Xi` *depends directly on* an unfixed type variable `Xj` if for some argument `Ek` with type `Tk` `Xj` occurs in an *input type* of `Ek` with type `Tk` and `Xi` occurs in an *output type* of `Ek` with type `Tk`.</span></span>

<span data-ttu-id="08161-630">`Xj` *取决于*`Xi`如果`Xj`*直接依赖于*`Xi`或者如果`Xi`*直接依赖于*`Xk`和`Xk`*取决于* `Xj`。</span><span class="sxs-lookup"><span data-stu-id="08161-630">`Xj` *depends on* `Xi` if `Xj` *depends directly on* `Xi` or if `Xi` *depends directly on* `Xk` and `Xk` *depends on* `Xj`.</span></span> <span data-ttu-id="08161-631">因此，"依赖于"是"直接依赖于"的可传递，但不是反身闭包。</span><span class="sxs-lookup"><span data-stu-id="08161-631">Thus "depends on" is the transitive but not reflexive closure of "depends directly on".</span></span>

#### <a name="output-type-inferences"></a><span data-ttu-id="08161-632">输出类型推断</span><span class="sxs-lookup"><span data-stu-id="08161-632">Output type inferences</span></span>

<span data-ttu-id="08161-633">*输出类型推理*由*从*表达式`E`*到*类型`T`如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-633">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="08161-634">如果`E`是一个匿名函数推断返回类型与`U`([推断返回类型](expressions.md#inferred-return-type)) 和`T`是委托类型或返回类型与表达式目录树类型`Tb`，然后*下限推断*([下限推断](expressions.md#lower-bound-inferences)) 由*从* `U` *到* `Tb`。</span><span class="sxs-lookup"><span data-stu-id="08161-634">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="08161-635">否则为如果`E`是一个方法组和`T`是委托类型或参数类型的表达式目录树类型`T1...Tk`和返回类型`Tb`，和的重载解析`E`与类型`T1...Tk`生成单一方法具有返回类型`U`，然后*下限推断*由*从* `U` *到* `Tb`。</span><span class="sxs-lookup"><span data-stu-id="08161-635">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="08161-636">否则为如果`E`是具有类型的表达式`U`，然后*下限推断*由*从* `U` *到* `T`.</span><span class="sxs-lookup"><span data-stu-id="08161-636">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
*  <span data-ttu-id="08161-637">否则，不进行任何推断。</span><span class="sxs-lookup"><span data-stu-id="08161-637">Otherwise, no inferences are made.</span></span>

#### <a name="explicit-parameter-type-inferences"></a><span data-ttu-id="08161-638">显式参数类型推断</span><span class="sxs-lookup"><span data-stu-id="08161-638">Explicit parameter type inferences</span></span>

<span data-ttu-id="08161-639">*显式参数类型推断*由*从*表达式`E`*到*类型`T`如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-639">An *explicit parameter type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="08161-640">如果`E`是一个显式类型化的匿名函数参数类型与`U1...Uk`并`T`是委托类型或参数类型的表达式目录树类型`V1...Vk`然后对于每个`Ui`*确切推理*([精确推断](expressions.md#exact-inferences)) 进行*从* `Ui` *到*相应`Vi`。</span><span class="sxs-lookup"><span data-stu-id="08161-640">If `E` is an explicitly typed anonymous function with parameter types `U1...Uk` and `T` is a delegate type or expression tree type with parameter types `V1...Vk` then for each `Ui` an *exact inference* ([Exact inferences](expressions.md#exact-inferences)) is made *from* `Ui` *to* the corresponding `Vi`.</span></span>

#### <a name="exact-inferences"></a><span data-ttu-id="08161-641">确切推断</span><span class="sxs-lookup"><span data-stu-id="08161-641">Exact inferences</span></span>

<span data-ttu-id="08161-642">*精确推理\*\*从*类型`U`*到*类型`V`由，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-642">An *exact inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="08161-643">如果`V`是之一*非固定*`Xi`然后`U`添加到的确切边界集`Xi`。</span><span class="sxs-lookup"><span data-stu-id="08161-643">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of exact bounds for `Xi`.</span></span>

*  <span data-ttu-id="08161-644">否则，设置`V1...Vk`和`U1...Uk`由检查是否任何以下情况下适用：</span><span class="sxs-lookup"><span data-stu-id="08161-644">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>

   *  <span data-ttu-id="08161-645">`V` 是一个数组类型`V1[...]`并`U`是一个数组类型`U1[...]`相同的排名</span><span class="sxs-lookup"><span data-stu-id="08161-645">`V` is an array type `V1[...]` and `U` is an array type `U1[...]`  of the same rank</span></span>
   *  <span data-ttu-id="08161-646">`V` 是类型`V1?`和`U`是类型 `U1?`</span><span class="sxs-lookup"><span data-stu-id="08161-646">`V` is the type `V1?` and `U` is the type `U1?`</span></span>
   *  <span data-ttu-id="08161-647">`V` 为构造的类型`C<V1...Vk>`和`U`为构造的类型 `C<U1...Uk>`</span><span class="sxs-lookup"><span data-stu-id="08161-647">`V` is a constructed type `C<V1...Vk>`and `U` is a constructed type `C<U1...Uk>`</span></span>

   <span data-ttu-id="08161-648">如果任何这些情况下然后适用*精确推理*由*从*每个`Ui`*到*相应`Vi`。</span><span class="sxs-lookup"><span data-stu-id="08161-648">If any of these cases apply then an *exact inference* is made *from* each `Ui` *to* the corresponding `Vi`.</span></span>

*  <span data-ttu-id="08161-649">否则不进行任何推断。</span><span class="sxs-lookup"><span data-stu-id="08161-649">Otherwise no inferences are made.</span></span>

#### <a name="lower-bound-inferences"></a><span data-ttu-id="08161-650">下界推断</span><span class="sxs-lookup"><span data-stu-id="08161-650">Lower-bound inferences</span></span>

<span data-ttu-id="08161-651">一个*下限推断\*\*从*类型`U`*到*类型`V`由，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-651">A *lower-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="08161-652">如果`V`是之一*非固定*`Xi`然后`U`添加到的一套为下限`Xi`。</span><span class="sxs-lookup"><span data-stu-id="08161-652">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of lower bounds for `Xi`.</span></span>
*  <span data-ttu-id="08161-653">否则为如果`V`是类型`V1?`并`U`是类型`U1?`则从进行下限推断`U1`到`V1`。</span><span class="sxs-lookup"><span data-stu-id="08161-653">Otherwise, if `V` is the type `V1?`and `U` is the type `U1?` then a lower bound inference is made from `U1` to `V1`.</span></span>
*  <span data-ttu-id="08161-654">否则，设置`U1...Uk`和`V1...Vk`由检查是否任何以下情况下适用：</span><span class="sxs-lookup"><span data-stu-id="08161-654">Otherwise, sets `U1...Uk` and `V1...Vk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="08161-655">`V` 是一个数组类型`V1[...]`并`U`是一个数组类型`U1[...]`(或类型参数的有效基类型是`U1[...]`) 相同的排名</span><span class="sxs-lookup"><span data-stu-id="08161-655">`V` is an array type `V1[...]` and `U` is an array type `U1[...]` (or a type parameter whose effective base type is `U1[...]`) of the same rank</span></span>
   *  <span data-ttu-id="08161-656">`V` 是的一个`IEnumerable<V1>`，`ICollection<V1>`或`IList<V1>`并`U`是一维数组类型`U1[]`(或类型参数的有效基类型是`U1[]`)</span><span class="sxs-lookup"><span data-stu-id="08161-656">`V` is one of `IEnumerable<V1>`, `ICollection<V1>` or `IList<V1>` and `U` is a one-dimensional array type `U1[]`(or a type parameter whose effective base type is `U1[]`)</span></span>
   *  <span data-ttu-id="08161-657">`V` 为构造的类、 结构、 接口或委托类型`C<V1...Vk>`且是唯一的类型`C<U1...Uk>`以便`U`(或者，如果`U`是类型参数、 其有效的基类或其有效的接口集的任何成员) 是要完全相同 （直接或间接） 继承或实现 （直接或间接） `C<U1...Uk>`。</span><span class="sxs-lookup"><span data-stu-id="08161-657">`V` is a constructed class, struct, interface or delegate type `C<V1...Vk>` and there is a unique type `C<U1...Uk>` such that `U` (or, if `U` is a type parameter, its effective base class or any member of its effective interface set) is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) `C<U1...Uk>`.</span></span>

      <span data-ttu-id="08161-658">("唯一性"限制意味着，在大小写的界面`C<T> {} class U: C<X>, C<Y> {}`，则无法推断进行时推断`U`到`C<T>`因为`U1`可能是`X`或`Y`。)</span><span class="sxs-lookup"><span data-stu-id="08161-658">(The "uniqueness" restriction means that in the case interface `C<T> {} class U: C<X>, C<Y> {}`, then no inference is made when inferring from `U` to `C<T>` because `U1` could be `X` or `Y`.)</span></span>

   <span data-ttu-id="08161-659">如果任何这些情况下适用进行推断*从*每个`Ui`*到*相应`Vi`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-659">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>

   *  <span data-ttu-id="08161-660">如果`Ui`未知引用类型，然后*精确推理*由</span><span class="sxs-lookup"><span data-stu-id="08161-660">If `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="08161-661">否则为如果`U`是一个数组类型，然后*下限推断*由</span><span class="sxs-lookup"><span data-stu-id="08161-661">Otherwise, if `U` is an array type then a *lower-bound inference* is made</span></span>
   *  <span data-ttu-id="08161-662">否则为如果`V`是`C<V1...Vk>`推理取决于的第 i 个类型参数，然后`C`:</span><span class="sxs-lookup"><span data-stu-id="08161-662">Otherwise, if `V` is `C<V1...Vk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="08161-663">如果它是协变，然后*下限推断*进行。</span><span class="sxs-lookup"><span data-stu-id="08161-663">If it is covariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="08161-664">如果它是逆变则*上限推断*进行。</span><span class="sxs-lookup"><span data-stu-id="08161-664">If it is contravariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="08161-665">如果它是固定然后*精确推理*进行。</span><span class="sxs-lookup"><span data-stu-id="08161-665">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="08161-666">否则，不进行任何推断。</span><span class="sxs-lookup"><span data-stu-id="08161-666">Otherwise, no inferences are made.</span></span>

#### <a name="upper-bound-inferences"></a><span data-ttu-id="08161-667">上限推断</span><span class="sxs-lookup"><span data-stu-id="08161-667">Upper-bound inferences</span></span>

<span data-ttu-id="08161-668">*上限推断\*\*从*类型`U`*到*类型`V`由，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-668">An *upper-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="08161-669">如果`V`是之一*非固定*`Xi`然后`U`添加到为上限的一套`Xi`。</span><span class="sxs-lookup"><span data-stu-id="08161-669">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of upper bounds for `Xi`.</span></span>
*  <span data-ttu-id="08161-670">否则，设置`V1...Vk`和`U1...Uk`由检查是否任何以下情况下适用：</span><span class="sxs-lookup"><span data-stu-id="08161-670">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="08161-671">`U` 是一个数组类型`U1[...]`并`V`是一个数组类型`V1[...]`相同的排名</span><span class="sxs-lookup"><span data-stu-id="08161-671">`U` is an array type `U1[...]` and `V` is an array type `V1[...]` of the same rank</span></span>
   *  <span data-ttu-id="08161-672">`U` 是的一个`IEnumerable<Ue>`，`ICollection<Ue>`或`IList<Ue>`和`V`是一维数组类型 `Ve[]`</span><span class="sxs-lookup"><span data-stu-id="08161-672">`U` is one of `IEnumerable<Ue>`, `ICollection<Ue>` or `IList<Ue>` and `V` is a one-dimensional array type `Ve[]`</span></span>
   *  <span data-ttu-id="08161-673">`U` 是类型`U1?`和`V`是类型 `V1?`</span><span class="sxs-lookup"><span data-stu-id="08161-673">`U` is the type `U1?` and `V` is the type `V1?`</span></span>
   *  <span data-ttu-id="08161-674">`U` 是构造的类、 结构、 接口或委托类型`C<U1...Uk>`和`V`（直接或间接） 是一个类、 结构、 接口或委托的类型相同，继承自 （直接或间接），该类型或实现一个唯一的类型 `C<V1...Vk>`</span><span class="sxs-lookup"><span data-stu-id="08161-674">`U` is constructed class, struct, interface or delegate type `C<U1...Uk>` and `V` is a class, struct, interface or delegate type which is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) a unique type `C<V1...Vk>`</span></span>

      <span data-ttu-id="08161-675">("唯一性"限制意味着，如果我们有`interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`，则无法推断进行时推断`C<U1>`到`V<Q>`。</span><span class="sxs-lookup"><span data-stu-id="08161-675">(The "uniqueness" restriction means that if we have `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, then no inference is made when inferring from `C<U1>` to `V<Q>`.</span></span> <span data-ttu-id="08161-676">从不进行推断`U1`至任一`X<Q>`或`Y<Q>`。)</span><span class="sxs-lookup"><span data-stu-id="08161-676">Inferences are not made from `U1` to either `X<Q>` or `Y<Q>`.)</span></span>

   <span data-ttu-id="08161-677">如果任何这些情况下适用进行推断*从*每个`Ui`*到*相应`Vi`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-677">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>
   *  <span data-ttu-id="08161-678">如果`Ui`未知引用类型，然后*精确推理*由</span><span class="sxs-lookup"><span data-stu-id="08161-678">If  `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="08161-679">否则为如果`V`是一个数组类型，然后*上限推断*由</span><span class="sxs-lookup"><span data-stu-id="08161-679">Otherwise, if `V` is an array type then an *upper-bound inference* is made</span></span>
   *  <span data-ttu-id="08161-680">否则为如果`U`是`C<U1...Uk>`推理取决于的第 i 个类型参数，然后`C`:</span><span class="sxs-lookup"><span data-stu-id="08161-680">Otherwise, if `U` is `C<U1...Uk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="08161-681">如果它是协变，然后*上限推断*进行。</span><span class="sxs-lookup"><span data-stu-id="08161-681">If it is covariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="08161-682">如果它是逆变则*下限推断*进行。</span><span class="sxs-lookup"><span data-stu-id="08161-682">If it is contravariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="08161-683">如果它是固定然后*精确推理*进行。</span><span class="sxs-lookup"><span data-stu-id="08161-683">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="08161-684">否则，不进行任何推断。</span><span class="sxs-lookup"><span data-stu-id="08161-684">Otherwise, no inferences are made.</span></span>   

#### <a name="fixing"></a><span data-ttu-id="08161-685">修复</span><span class="sxs-lookup"><span data-stu-id="08161-685">Fixing</span></span>

<span data-ttu-id="08161-686">*非固定*类型的变量`Xi`与一组边界*修复*，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-686">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>

*  <span data-ttu-id="08161-687">一套*候选类型*`Uj`边界组中的所有类型的集的起始点`Xi`。</span><span class="sxs-lookup"><span data-stu-id="08161-687">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
*  <span data-ttu-id="08161-688">然后，我们检查的每个绑定`Xi`反过来：对于每个确切绑定`U`的`Xi`所有类型`Uj`不等于`U`从候选集合中移除。</span><span class="sxs-lookup"><span data-stu-id="08161-688">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="08161-689">对于每个下限`U`的`Xi`所有类型`Uj`到另一个的位置是*不*隐式转换`U`从候选集合中移除。</span><span class="sxs-lookup"><span data-stu-id="08161-689">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="08161-690">对于每个上限`U`的`Xi`所有类型`Uj`从其中有*不*隐式转换为`U`从候选集合中移除。</span><span class="sxs-lookup"><span data-stu-id="08161-690">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
*  <span data-ttu-id="08161-691">如果在其余候选类型之间`Uj`唯一类型`V`它们没有隐式转换为所有的其他候选类型，然后从`Xi`被固定为`V`。</span><span class="sxs-lookup"><span data-stu-id="08161-691">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then `Xi` is fixed to `V`.</span></span>
*  <span data-ttu-id="08161-692">否则，类型推理失败。</span><span class="sxs-lookup"><span data-stu-id="08161-692">Otherwise, type inference fails.</span></span>

#### <a name="inferred-return-type"></a><span data-ttu-id="08161-693">推断返回类型</span><span class="sxs-lookup"><span data-stu-id="08161-693">Inferred return type</span></span>

<span data-ttu-id="08161-694">返回一个匿名函数的类型推断出`F`在类型推理和重载解析过程中使用。</span><span class="sxs-lookup"><span data-stu-id="08161-694">The inferred return type of an anonymous function `F` is used during type inference and overload resolution.</span></span> <span data-ttu-id="08161-695">仅可以为所有参数类型已知的或者用户显式获得了，因为通过匿名函数转换提供或推断在上的类型推理封闭泛型过程的其中一个匿名函数确定推断返回类型方法调用。</span><span class="sxs-lookup"><span data-stu-id="08161-695">The inferred return type can only be determined for an anonymous function where all parameter types are known, either because they are explicitly given, provided through an anonymous function conversion or inferred during type inference on an enclosing generic method invocation.</span></span>

<span data-ttu-id="08161-696">***推断出的结果类型***，如下所示确定：</span><span class="sxs-lookup"><span data-stu-id="08161-696">The ***inferred result type*** is determined as follows:</span></span>

*  <span data-ttu-id="08161-697">如果正文`F`是*表达式*具有一个类型，然后推断的结果类型为`F`是该表达式的类型。</span><span class="sxs-lookup"><span data-stu-id="08161-697">If the body of `F` is an *expression* that has a type, then the inferred result type of `F` is the type of that expression.</span></span>
*  <span data-ttu-id="08161-698">如果正文`F`是*块*和表达式块中的一`return`语句具有最佳的通用类型`T`([查找一组表达式的最常见类型](expressions.md#finding-the-best-common-type-of-a-set-of-expressions))，然后推断的结果类型为`F`是`T`。</span><span class="sxs-lookup"><span data-stu-id="08161-698">If the body of `F` is a *block* and the set of expressions in the block's `return` statements has a best common type `T` ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), then the inferred result type of `F` is `T`.</span></span>
*  <span data-ttu-id="08161-699">否则，不能为推断结果类型`F`。</span><span class="sxs-lookup"><span data-stu-id="08161-699">Otherwise, a result type cannot be inferred for `F`.</span></span>

<span data-ttu-id="08161-700">***推断返回类型***，如下所示确定：</span><span class="sxs-lookup"><span data-stu-id="08161-700">The ***inferred return type*** is determined as follows:</span></span>

*  <span data-ttu-id="08161-701">如果`F`是异步和正文`F`是归类为执行任何操作的表达式 ([表达式分类](expressions.md#expression-classifications))，或语句块，其中没有返回语句具有表达式，推断出的返回类型是 `System.Threading.Tasks.Task`</span><span class="sxs-lookup"><span data-stu-id="08161-701">If `F` is async and the body of `F` is either an expression classified as nothing ([Expression classifications](expressions.md#expression-classifications)), or a statement block where no return statements have expressions, the inferred return type is `System.Threading.Tasks.Task`</span></span>
*  <span data-ttu-id="08161-702">如果`F`异步，且是一个推断的结果类型`T`，推断出的返回类型是`System.Threading.Tasks.Task<T>`。</span><span class="sxs-lookup"><span data-stu-id="08161-702">If `F` is async and has an inferred result type `T`, the inferred return type is `System.Threading.Tasks.Task<T>`.</span></span>
*  <span data-ttu-id="08161-703">如果`F`非异步，且是一个推断的结果类型`T`，推断出的返回类型是`T`。</span><span class="sxs-lookup"><span data-stu-id="08161-703">If `F` is non-async and has an inferred result type `T`, the inferred return type is `T`.</span></span>
*  <span data-ttu-id="08161-704">否则，不能为推断返回类型`F`。</span><span class="sxs-lookup"><span data-stu-id="08161-704">Otherwise a return type cannot be inferred for `F`.</span></span>

<span data-ttu-id="08161-705">作为涉及匿名函数的类型推理的示例，请考虑`Select`扩展方法中声明`System.Linq.Enumerable`类：</span><span class="sxs-lookup"><span data-stu-id="08161-705">As an example of type inference involving anonymous functions, consider the `Select` extension method declared in the `System.Linq.Enumerable` class:</span></span>
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

<span data-ttu-id="08161-706">假设`System.Linq`命名空间与已导入`using`子句，并给定一个类`Customer`与`Name`类型的属性`string`，则`Select`方法可用于选择一个客户列表的名称：</span><span class="sxs-lookup"><span data-stu-id="08161-706">Assuming the `System.Linq` namespace was imported with a `using` clause, and given a class `Customer` with a `Name` property of type `string`, the `Select` method can be used to select the names of a list of customers:</span></span>
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

<span data-ttu-id="08161-707">扩展方法调用 ([扩展方法调用](expressions.md#extension-method-invocations)) 的`Select`处理的重写的静态方法调用的调用：</span><span class="sxs-lookup"><span data-stu-id="08161-707">The extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)) of `Select` is processed by rewriting the invocation to a static method invocation:</span></span>
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

<span data-ttu-id="08161-708">由于未显式指定类型参数，使用类型推理来推断类型参数。</span><span class="sxs-lookup"><span data-stu-id="08161-708">Since type arguments were not explicitly specified, type inference is used to infer the type arguments.</span></span> <span data-ttu-id="08161-709">首先，`customers`参数与`source`参数，推断`T`为`Customer`。</span><span class="sxs-lookup"><span data-stu-id="08161-709">First, the `customers` argument is related to the `source` parameter, inferring `T` to be `Customer`.</span></span> <span data-ttu-id="08161-710">然后，使用匿名函数键入推断过程，上面所述`c`给定类型`Customer`，和表达式`c.Name`的返回类型与相关`selector`参数，推断`S`要`string`.</span><span class="sxs-lookup"><span data-stu-id="08161-710">Then, using the anonymous function type inference process described above, `c` is given type `Customer`, and the expression `c.Name` is related to the return type of the `selector` parameter, inferring `S` to be `string`.</span></span> <span data-ttu-id="08161-711">因此，调用等同于</span><span class="sxs-lookup"><span data-stu-id="08161-711">Thus, the invocation is equivalent to</span></span>
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
<span data-ttu-id="08161-712">结果的类型为`IEnumerable<string>`。</span><span class="sxs-lookup"><span data-stu-id="08161-712">and the result is of type `IEnumerable<string>`.</span></span>

<span data-ttu-id="08161-713">下面的示例演示如何匿名函数类型推理允许"流"中的泛型方法调用的自变量之间的类型信息。</span><span class="sxs-lookup"><span data-stu-id="08161-713">The following example demonstrates how anonymous function type inference allows type information to "flow" between arguments in a generic method invocation.</span></span> <span data-ttu-id="08161-714">对于给定的方法：</span><span class="sxs-lookup"><span data-stu-id="08161-714">Given the method:</span></span>
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

<span data-ttu-id="08161-715">调用的类型推断：</span><span class="sxs-lookup"><span data-stu-id="08161-715">Type inference for the invocation:</span></span>
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
<span data-ttu-id="08161-716">将继续，按如下所示：首先，该参数`"1:15:30"`与相关`value`参数，推断`X`为`string`。</span><span class="sxs-lookup"><span data-stu-id="08161-716">proceeds as follows: First, the argument `"1:15:30"` is related to the `value` parameter, inferring `X` to be `string`.</span></span> <span data-ttu-id="08161-717">然后，第一个匿名函数的参数`s`，为给定的推断的类型`string`，和表达式`TimeSpan.Parse(s)`的返回类型与相关`f1`、 推断`Y`为`System.TimeSpan`。</span><span class="sxs-lookup"><span data-stu-id="08161-717">Then, the parameter of the first anonymous function, `s`, is given the inferred type `string`, and the expression `TimeSpan.Parse(s)` is related to the return type of `f1`, inferring `Y` to be `System.TimeSpan`.</span></span> <span data-ttu-id="08161-718">最后，第二个匿名函数的参数`t`，为给定的推断的类型`System.TimeSpan`，和表达式`t.TotalSeconds`的返回类型与相关`f2`、 推断`Z`为`double`。</span><span class="sxs-lookup"><span data-stu-id="08161-718">Finally, the parameter of the second anonymous function, `t`, is given the inferred type `System.TimeSpan`, and the expression `t.TotalSeconds` is related to the return type of `f2`, inferring `Z` to be `double`.</span></span> <span data-ttu-id="08161-719">因此，调用的结果属于类型`double`。</span><span class="sxs-lookup"><span data-stu-id="08161-719">Thus, the result of the invocation is of type `double`.</span></span>

#### <a name="type-inference-for-conversion-of-method-groups"></a><span data-ttu-id="08161-720">为方法组转换的类型推理</span><span class="sxs-lookup"><span data-stu-id="08161-720">Type inference for conversion of method groups</span></span>

<span data-ttu-id="08161-721">与泛型方法的调用类似，类型推断还必须应用时方法组`M`包含泛型方法转换为给定的委托类型`D`([方法组转换](conversions.md#method-group-conversions))。</span><span class="sxs-lookup"><span data-stu-id="08161-721">Similar to calls of generic methods, type inference must also be applied when a method group `M` containing a generic method is converted to a given delegate type `D` ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="08161-722">给定方法</span><span class="sxs-lookup"><span data-stu-id="08161-722">Given a method</span></span>
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
<span data-ttu-id="08161-723">和方法组`M`赋给委托类型`D`的类型推理任务是查找类型实参`S1...Sn`以便表达式：</span><span class="sxs-lookup"><span data-stu-id="08161-723">and the method group `M` being assigned to the delegate type `D` the task of type inference is to find type arguments `S1...Sn` so that the expression:</span></span>
```csharp
M<S1...Sn>
```
<span data-ttu-id="08161-724">将成为兼容 ([委托声明](delegates.md#delegate-declarations)) 与`D`。</span><span class="sxs-lookup"><span data-stu-id="08161-724">becomes compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`.</span></span>

<span data-ttu-id="08161-725">与不同的泛型方法调用的类型推断算法，在这种情况下有仅参数*类型*，没有参数*表达式*。</span><span class="sxs-lookup"><span data-stu-id="08161-725">Unlike the type inference algorithm for generic method calls, in this case there are only argument *types*, no argument *expressions*.</span></span> <span data-ttu-id="08161-726">具体而言，没有匿名函数，因此无需进行推理的多个阶段。</span><span class="sxs-lookup"><span data-stu-id="08161-726">In particular, there are no anonymous functions and hence no need for multiple phases of inference.</span></span>

<span data-ttu-id="08161-727">相反，所有`Xi`被视为*非固定*，和一个*下限推断*由*从*每个参数类型`Uj`的`D`*到*对应的参数类型`Tj`的`M`。</span><span class="sxs-lookup"><span data-stu-id="08161-727">Instead, all `Xi` are considered *unfixed*, and a *lower-bound inference* is made *from* each argument type `Uj` of `D` *to* the corresponding parameter type `Tj` of `M`.</span></span> <span data-ttu-id="08161-728">如果任一`Xi`没有找到，类型推理失败。</span><span class="sxs-lookup"><span data-stu-id="08161-728">If for any of the `Xi` no bounds were found, type inference fails.</span></span> <span data-ttu-id="08161-729">否则为所有`Xi`都*修复*到对应`Si`，这是类型推理的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-729">Otherwise, all `Xi` are *fixed* to corresponding `Si`, which are the result of type inference.</span></span>

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a><span data-ttu-id="08161-730">查找一组表达式的最常见类型</span><span class="sxs-lookup"><span data-stu-id="08161-730">Finding the best common type of a set of expressions</span></span>

<span data-ttu-id="08161-731">在某些情况下，需要为一组表达式推断通用类型。</span><span class="sxs-lookup"><span data-stu-id="08161-731">In some cases, a common type needs to be inferred for a set of expressions.</span></span> <span data-ttu-id="08161-732">在特定、 隐式类型化数组的元素类型和使用匿名函数的返回类型*块*正文在这种方式中找到。</span><span class="sxs-lookup"><span data-stu-id="08161-732">In particular, the element types of implicitly typed arrays and the return types of anonymous functions with *block* bodies are found in this way.</span></span>

<span data-ttu-id="08161-733">直观地说，给定一组表达式`E1...Em`此推断应为等效于调用方法</span><span class="sxs-lookup"><span data-stu-id="08161-733">Intuitively, given a set of expressions `E1...Em` this inference should be equivalent to calling a method</span></span>
```csharp
Tr M<X>(X x1 ... X xm)
```
<span data-ttu-id="08161-734">使用`Ei`作为参数。</span><span class="sxs-lookup"><span data-stu-id="08161-734">with the `Ei` as arguments.</span></span>

<span data-ttu-id="08161-735">更确切地说，推理首先*非固定*类型的变量`X`。</span><span class="sxs-lookup"><span data-stu-id="08161-735">More precisely, the inference starts out with an *unfixed* type variable `X`.</span></span> <span data-ttu-id="08161-736">*输出类型推理*然后做*从*每个`Ei`*到* `X`。</span><span class="sxs-lookup"><span data-stu-id="08161-736">*Output type inferences* are then made *from* each `Ei` *to* `X`.</span></span> <span data-ttu-id="08161-737">最后，`X`是*修复*和，如果成功，则生成键入`S`是表达式的结果的最通用类型。</span><span class="sxs-lookup"><span data-stu-id="08161-737">Finally, `X` is *fixed* and, if successful, the resulting type `S` is the resulting best common type for the expressions.</span></span> <span data-ttu-id="08161-738">如果没有此`S`存在，两个表达式有任何最佳的常用类型。</span><span class="sxs-lookup"><span data-stu-id="08161-738">If no such `S` exists, the expressions have no best common type.</span></span>

### <a name="overload-resolution"></a><span data-ttu-id="08161-739">重载决策</span><span class="sxs-lookup"><span data-stu-id="08161-739">Overload resolution</span></span>

<span data-ttu-id="08161-740">重载解析是绑定时机制，用于选择要调用给定的参数列表和一组候选函数成员的最佳函数成员。</span><span class="sxs-lookup"><span data-stu-id="08161-740">Overload resolution is a binding-time mechanism for selecting the best function member to invoke given an argument list and a set of candidate function members.</span></span> <span data-ttu-id="08161-741">重载决策选择要在 C# 中下列不同的上下文中调用的函数成员：</span><span class="sxs-lookup"><span data-stu-id="08161-741">Overload resolution selects the function member to invoke in the following distinct contexts within C#:</span></span>

*  <span data-ttu-id="08161-742">在名为方法的调用*invocation_expression* ([方法调用](expressions.md#method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="08161-742">Invocation of a method named in an *invocation_expression* ([Method invocations](expressions.md#method-invocations)).</span></span>
*  <span data-ttu-id="08161-743">实例构造函数中名为调用*object_creation_expression* ([对象创建表达式](expressions.md#object-creation-expressions))。</span><span class="sxs-lookup"><span data-stu-id="08161-743">Invocation of an instance constructor named in an *object_creation_expression* ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>
*  <span data-ttu-id="08161-744">索引器取值函数通过调用*element_access* ([元素访问](expressions.md#element-access))。</span><span class="sxs-lookup"><span data-stu-id="08161-744">Invocation of an indexer accessor through an *element_access* ([Element access](expressions.md#element-access)).</span></span>
*  <span data-ttu-id="08161-745">在表达式中引用预定义或用户定义运算符的调用 ([一元运算符重载决策](expressions.md#unary-operator-overload-resolution)并[二元运算符重载决策](expressions.md#binary-operator-overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="08161-745">Invocation of a predefined or user-defined operator referenced in an expression ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)).</span></span>

<span data-ttu-id="08161-746">这些上下文中的每个定义的候选函数成员集的参数列表中和其自己独特的方式，上面列出的章节中的详细信息中所述。</span><span class="sxs-lookup"><span data-stu-id="08161-746">Each of these contexts defines the set of candidate function members and the list of arguments in its own unique way, as described in detail in the sections listed above.</span></span> <span data-ttu-id="08161-747">例如，为方法调用的候选项的集不包括标记的方法`override`([成员查找](expressions.md#member-lookup))，而且在基类中的方法不是候选项，如果派生类中的任何方法适用 ([方法调用](expressions.md#method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="08161-747">For example, the set of candidates for a method invocation does not include methods marked `override` ([Member lookup](expressions.md#member-lookup)), and methods in a base class are not candidates if any method in a derived class is applicable ([Method invocations](expressions.md#method-invocations)).</span></span>

<span data-ttu-id="08161-748">一旦已确定候选函数成员和参数列表，最佳函数成员的选择是在所有情况下相同的：</span><span class="sxs-lookup"><span data-stu-id="08161-748">Once the candidate function members and the argument list have been identified, the selection of the best function member is the same in all cases:</span></span>

*  <span data-ttu-id="08161-749">给定的一套适用的候选函数成员，最佳函数成员的操作，在其中找到。</span><span class="sxs-lookup"><span data-stu-id="08161-749">Given the set of applicable candidate function members, the best function member in that set is located.</span></span> <span data-ttu-id="08161-750">如果集包含只有一个函数成员，则该函数成员为最佳的函数成员。</span><span class="sxs-lookup"><span data-stu-id="08161-750">If the set contains only one function member, then that function member is the best function member.</span></span> <span data-ttu-id="08161-751">否则，最佳函数成员是优于成员对给定的参数列表中，前提是每个函数成员进行比较的使用中的规则的所有其他函数成员的一个函数成员[更好的函数成员](expressions.md#better-function-member)。</span><span class="sxs-lookup"><span data-stu-id="08161-751">Otherwise, the best function member is the one function member that is better than all other function members with respect to the given argument list, provided that each function member is compared to all other function members using the rules in [Better function member](expressions.md#better-function-member).</span></span> <span data-ttu-id="08161-752">如果不完全是一个函数成员优于所有其他函数成员，则该函数成员的调用是不明确和绑定时出错。</span><span class="sxs-lookup"><span data-stu-id="08161-752">If there is not exactly one function member that is better than all other function members, then the function member invocation is ambiguous and a binding-time error occurs.</span></span>

<span data-ttu-id="08161-753">以下几节定义条款的准确含义***适用函数成员***并***更好的函数成员***。</span><span class="sxs-lookup"><span data-stu-id="08161-753">The following sections define the exact meanings of the terms ***applicable function member*** and ***better function member***.</span></span>

#### <a name="applicable-function-member"></a><span data-ttu-id="08161-754">适用函数成员</span><span class="sxs-lookup"><span data-stu-id="08161-754">Applicable function member</span></span>

<span data-ttu-id="08161-755">函数成员称为***适用函数成员***相对于参数列表`A`当所有以下均为 true:</span><span class="sxs-lookup"><span data-stu-id="08161-755">A function member is said to be an ***applicable function member*** with respect to an argument list `A` when all of the following are true:</span></span>

*  <span data-ttu-id="08161-756">在每个自变量`A`对应于函数成员声明中的参数，如中所述[相应参数](expressions.md#corresponding-parameters)，和任何自变量对应的任何参数是一个可选参数。</span><span class="sxs-lookup"><span data-stu-id="08161-756">Each argument in `A` corresponds to a parameter in the function member declaration as described in [Corresponding parameters](expressions.md#corresponding-parameters), and any parameter to which no argument corresponds is an optional parameter.</span></span>
*  <span data-ttu-id="08161-757">中每个参数的`A`、 参数传递的参数的模式 (即，值`ref`，或`out`) 等同于的相应参数的参数传递模式和</span><span class="sxs-lookup"><span data-stu-id="08161-757">For each argument in `A`, the parameter passing mode of the argument (i.e., value, `ref`, or `out`) is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="08161-758">值参数或参数数组的隐式转换 ([隐式转换](conversions.md#implicit-conversions)) 存在从参数到的相应参数的类型或</span><span class="sxs-lookup"><span data-stu-id="08161-758">for a value parameter or a parameter array, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="08161-759">有关`ref`或`out`参数自变量的类型是与对应参数的类型相同。</span><span class="sxs-lookup"><span data-stu-id="08161-759">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span> <span data-ttu-id="08161-760">毕竟`ref`或`out`参数是传递的参数的别名。</span><span class="sxs-lookup"><span data-stu-id="08161-760">After all, a `ref` or `out` parameter is an alias for the argument passed.</span></span>

<span data-ttu-id="08161-761">对于函数成员，包括参数数组，如果通过上述规则适用的函数成员，它则称其适用于其***范式***。</span><span class="sxs-lookup"><span data-stu-id="08161-761">For a function member that includes a parameter array, if the function member is applicable by the above rules, it is said to be applicable in its ***normal form***.</span></span> <span data-ttu-id="08161-762">如果包括参数数组的函数成员，以常规形式适用函数成员而可能适用于其***扩展窗体***:</span><span class="sxs-lookup"><span data-stu-id="08161-762">If a function member that includes a parameter array is not applicable in its normal form, the function member may instead be applicable in its ***expanded form***:</span></span>

*  <span data-ttu-id="08161-763">扩展的形式构造的函数成员声明中的形参数组替换为零或更多值参数的参数的元素类型的数组，如该自变量列表中的参数数目`A`匹配总计参数数目。</span><span class="sxs-lookup"><span data-stu-id="08161-763">The expanded form is constructed by replacing the parameter array in the function member declaration with zero or more value parameters of the element type of the parameter array such that the number of arguments in the argument list `A` matches the total number of parameters.</span></span> <span data-ttu-id="08161-764">如果`A`函数成员声明中具有较少的参数，而固定的参数数目、 的函数成员的展开的形式不能构造，因此不适用。</span><span class="sxs-lookup"><span data-stu-id="08161-764">If `A` has fewer arguments than the number of fixed parameters in the function member declaration, the expanded form of the function member cannot be constructed and is thus not applicable.</span></span>
*  <span data-ttu-id="08161-765">否则，扩展的形式是适用于在每个自变量如果`A`自变量的参数传递模式等同于的相应参数的参数传递模式和</span><span class="sxs-lookup"><span data-stu-id="08161-765">Otherwise, the expanded form is applicable if for each argument in `A` the parameter passing mode of the argument is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="08161-766">固定的值参数或创建扩展的隐式转换的值参数 ([隐式转换](conversions.md#implicit-conversions)) 存在从自变量的类型的相应参数的类型或</span><span class="sxs-lookup"><span data-stu-id="08161-766">for a fixed value parameter or a value parameter created by the expansion, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the type of the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="08161-767">有关`ref`或`out`参数自变量的类型是与对应参数的类型相同。</span><span class="sxs-lookup"><span data-stu-id="08161-767">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span>

#### <a name="better-function-member"></a><span data-ttu-id="08161-768">更好的函数成员</span><span class="sxs-lookup"><span data-stu-id="08161-768">Better function member</span></span>

<span data-ttu-id="08161-769">为了确定更好的函数成员，精简的实参列表 A 构造包含只参数表达式本身原始的参数列表中的显示顺序。</span><span class="sxs-lookup"><span data-stu-id="08161-769">For the purposes of determining the better function member, a stripped-down argument list A is constructed containing just the argument expressions themselves in the order they appear in the original argument list.</span></span>

<span data-ttu-id="08161-770">参数列出了为每个候选函数成员的构造如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-770">Parameter lists for each of the candidate function members are constructed in the following way:</span></span>

*  <span data-ttu-id="08161-771">如果函数成员是只适用于扩展的形式，则使用扩展的形式。</span><span class="sxs-lookup"><span data-stu-id="08161-771">The expanded form is used if the function member was applicable only in the expanded form.</span></span>
*  <span data-ttu-id="08161-772">从参数列表中删除没有相应的自变量的可选参数</span><span class="sxs-lookup"><span data-stu-id="08161-772">Optional parameters with no corresponding arguments are removed from the parameter list</span></span>
*  <span data-ttu-id="08161-773">因此，它们在与相应的自变量参数列表中相同的位置会发生重新排序参数。</span><span class="sxs-lookup"><span data-stu-id="08161-773">The parameters are reordered so that they occur at the same position as the corresponding argument in the argument list.</span></span>

<span data-ttu-id="08161-774">给定的参数列表`A`与一组自变量表达式`{E1, E2, ..., En}`和两个适用的函数成员`Mp`和`Mq`与参数类型`{P1, P2, ..., Pn}`并`{Q1, Q2, ..., Qn}`，`Mp`被定义为***更好的函数成员***比`Mq`如果</span><span class="sxs-lookup"><span data-stu-id="08161-774">Given an argument list `A` with a set of argument expressions `{E1, E2, ..., En}` and two applicable function members `Mp` and `Mq` with parameter types `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}`, `Mp` is defined to be a ***better function member*** than `Mq` if</span></span>

*  <span data-ttu-id="08161-775">对于每个参数，请从隐式转换`Ex`到`Qx`不是从隐式转换优于`Ex`到`Px`，和</span><span class="sxs-lookup"><span data-stu-id="08161-775">for each argument, the implicit conversion from `Ex` to `Qx` is not better than the implicit conversion from `Ex` to `Px`, and</span></span>
*  <span data-ttu-id="08161-776">对于至少一个参数，请从转换`Ex`到`Px`从转换优于`Ex`到`Qx`。</span><span class="sxs-lookup"><span data-stu-id="08161-776">for at least one argument, the conversion from `Ex` to `Px` is better than the conversion from `Ex` to `Qx`.</span></span>

<span data-ttu-id="08161-777">在执行此计算时，如果`Mp`或`Mq`是适用于它的扩展形式，然后`Px`或`Qx`指的是展开形式的参数列表中的参数。</span><span class="sxs-lookup"><span data-stu-id="08161-777">When performing this evaluation, if `Mp` or `Mq` is applicable in its expanded form, then `Px` or `Qx` refers to a parameter in the expanded form of the parameter list.</span></span>

<span data-ttu-id="08161-778">如果参数类型序列 `{P1, P2, ..., Pn}`和`{Q1, Q2, ..., Qn}`是等效的 (即每个`Pi`标识转换为相应`Qi`)，以下的决定性规则，按顺序应用，以确定，性能越好函数成员。</span><span class="sxs-lookup"><span data-stu-id="08161-778">In case the parameter type sequences `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}` are equivalent (i.e. each `Pi` has an identity conversion to the corresponding `Qi`), the following tie-breaking rules are applied, in order, to determine the better function member.</span></span>

*  <span data-ttu-id="08161-779">如果`Mp`是一种非泛型方法和`Mq`是一个泛型方法，则`Mp`优于`Mq`。</span><span class="sxs-lookup"><span data-stu-id="08161-779">If `Mp` is a non-generic method and `Mq` is a generic method, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="08161-780">否则为如果`Mp`适用于其正常的窗体并`Mq`已`params`数组也适用于仅在其扩展形式中，然后`Mp`优于`Mq`。</span><span class="sxs-lookup"><span data-stu-id="08161-780">Otherwise, if `Mp` is applicable in its normal form and `Mq` has a `params` array and is applicable only in its expanded form, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="08161-781">否则为如果`Mp`的详细信息已声明参数比`Mq`，然后`Mp`优于`Mq`。</span><span class="sxs-lookup"><span data-stu-id="08161-781">Otherwise, if `Mp` has more declared parameters than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="08161-782">发生这种情况这两种方法具有`params`数组，并且只适用于它们扩展的形式。</span><span class="sxs-lookup"><span data-stu-id="08161-782">This can occur if both methods have `params` arrays and are applicable only in their expanded forms.</span></span>
*  <span data-ttu-id="08161-783">如果所有参数的`Mp`具有相应参数，而默认自变量需要替换为在至少一个可选参数`Mq`然后`Mp`优于`Mq`。</span><span class="sxs-lookup"><span data-stu-id="08161-783">Otherwise if all parameters of `Mp` have a corresponding argument whereas default arguments need to be substituted for at least one optional parameter in `Mq` then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="08161-784">否则为如果`Mp`具有更具体的参数类型比`Mq`，然后`Mp`优于`Mq`。</span><span class="sxs-lookup"><span data-stu-id="08161-784">Otherwise, if `Mp` has more specific parameter types than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="08161-785">让`{R1, R2, ..., Rn}`并`{S1, S2, ..., Sn}`表示未实例化和非扩展的参数类型的`Mp`和`Mq`。</span><span class="sxs-lookup"><span data-stu-id="08161-785">Let `{R1, R2, ..., Rn}` and `{S1, S2, ..., Sn}` represent the uninstantiated and unexpanded parameter types of `Mp` and `Mq`.</span></span> <span data-ttu-id="08161-786">`Mp`参数类型为比更具体`Mq`的当每个参数，对于`Rx`不是具体`Sx`，和至少一个参数`Rx`比更具体`Sx`:</span><span class="sxs-lookup"><span data-stu-id="08161-786">`Mp`'s parameter types are more specific than `Mq`'s if, for each parameter, `Rx` is not less specific than `Sx`, and, for at least one parameter, `Rx` is more specific than `Sx`:</span></span>
   *  <span data-ttu-id="08161-787">类型参数是更具体的非类型参数。</span><span class="sxs-lookup"><span data-stu-id="08161-787">A type parameter is less specific than a non-type parameter.</span></span>
   *  <span data-ttu-id="08161-788">以递归方式，构造的类型是不是另一个构造的类型 （具有相同的类型参数） 如果至少一个类型实参是更具体且没有类型参数为更具体的相应类型参数中的其他更具体。</span><span class="sxs-lookup"><span data-stu-id="08161-788">Recursively, a constructed type is more specific than another constructed type (with the same number of type arguments) if at least one type argument is more specific and no type argument is less specific than the corresponding type argument in the other.</span></span>
   *  <span data-ttu-id="08161-789">如果第一个元素类型是比第二个元素类型更具体，数组类型是比 （具有相同维数的） 的另一个数组类型更具体。</span><span class="sxs-lookup"><span data-stu-id="08161-789">An array type is more specific than another array type (with the same number of dimensions) if the element type of the first is more specific than the element type of the second.</span></span>
*  <span data-ttu-id="08161-790">否则，如果一个成员是一个非提升运算符，另一个提升的运算符，是更好的非提升一个。</span><span class="sxs-lookup"><span data-stu-id="08161-790">Otherwise if one member is a non-lifted operator and  the other is a lifted operator, the non-lifted one is better.</span></span>
*  <span data-ttu-id="08161-791">否则，两个函数成员是更好。</span><span class="sxs-lookup"><span data-stu-id="08161-791">Otherwise, neither function member is better.</span></span>

#### <a name="better-conversion-from-expression"></a><span data-ttu-id="08161-792">从表达式更好的转换</span><span class="sxs-lookup"><span data-stu-id="08161-792">Better conversion from expression</span></span>

<span data-ttu-id="08161-793">给定的隐式转换`C1`，它将从表达式`E`为某种`T1`，和隐式转换`C2`，它将从表达式`E`为某种`T2`， `C1`是***更好的转换***比`C2`如果`E`不完全匹配`T2`和至少一个以下包含：</span><span class="sxs-lookup"><span data-stu-id="08161-793">Given an implicit conversion `C1` that converts from an expression `E` to a type `T1`, and an implicit conversion `C2` that converts from an expression `E` to a type `T2`, `C1` is a ***better conversion*** than `C2` if `E` does not exactly match `T2` and at least one of the following holds:</span></span>

* <span data-ttu-id="08161-794">`E` 完全匹配`T1`([表达式完全匹配的](expressions.md#exactly-matching-expression))</span><span class="sxs-lookup"><span data-stu-id="08161-794">`E` exactly matches `T1` ([Exactly matching Expression](expressions.md#exactly-matching-expression))</span></span>
* <span data-ttu-id="08161-795">`T1` 是更好的转换目标比`T2`([更好地转换目标](expressions.md#better-conversion-target))</span><span class="sxs-lookup"><span data-stu-id="08161-795">`T1` is a better conversion target than `T2` ([Better conversion target](expressions.md#better-conversion-target))</span></span>

#### <a name="exactly-matching-expression"></a><span data-ttu-id="08161-796">完全匹配的表达式</span><span class="sxs-lookup"><span data-stu-id="08161-796">Exactly matching Expression</span></span>

<span data-ttu-id="08161-797">提供了一个表达式`E`并键入`T`，`E`完全匹配`T`如果下列之一保存：</span><span class="sxs-lookup"><span data-stu-id="08161-797">Given an expression `E` and a type `T`, `E` exactly matches `T` if one of the following holds:</span></span>

*  <span data-ttu-id="08161-798">`E` 具有类型`S`，并且是标识转换存在一个从`S`到 `T`</span><span class="sxs-lookup"><span data-stu-id="08161-798">`E` has a type `S`, and an identity conversion exists from `S` to `T`</span></span>
*  <span data-ttu-id="08161-799">`E` 是一个匿名函数`T`是委托类型`D`或表达式树类型`Expression<D>`和以下之一保存：</span><span class="sxs-lookup"><span data-stu-id="08161-799">`E` is an anonymous function, `T` is either a delegate type `D` or an expression tree type `Expression<D>` and one of the following holds:</span></span>
   *  <span data-ttu-id="08161-800">推断返回类型`X`存在`E`中的参数列表的上下文`D`([推断返回类型](expressions.md#inferred-return-type))，并且是标识转换存在一个从`X`到的返回类型 `D`</span><span class="sxs-lookup"><span data-stu-id="08161-800">An inferred return type `X` exists for `E` in the context of the parameter list of `D` ([Inferred return type](expressions.md#inferred-return-type)), and an identity conversion exists from `X` to the return type of `D`</span></span>
   *  <span data-ttu-id="08161-801">任一`E`为非异步和`D`具有返回类型`Y`或`E`是异步和`D`具有返回类型`Task<Y>`，并包含以下项之一：</span><span class="sxs-lookup"><span data-stu-id="08161-801">Either `E` is non-async and `D` has a return type `Y` or `E` is async and `D` has a return type `Task<Y>`, and one of the following holds:</span></span>
      * <span data-ttu-id="08161-802">正文`E`是完全匹配的表达式 `Y`</span><span class="sxs-lookup"><span data-stu-id="08161-802">The body of `E` is an expression that exactly matches `Y`</span></span>
      * <span data-ttu-id="08161-803">正文`E`是语句块，其中每个 return 语句返回的表达式完全匹配的 `Y`</span><span class="sxs-lookup"><span data-stu-id="08161-803">The body of `E` is a statement block where every return statement returns an expression that exactly matches `Y`</span></span>

#### <a name="better-conversion-target"></a><span data-ttu-id="08161-804">更好地转换目标</span><span class="sxs-lookup"><span data-stu-id="08161-804">Better conversion target</span></span>

<span data-ttu-id="08161-805">提供两种不同类型`T1`并`T2`，`T1`是更好的转换目标比`T2`如果从没有隐式转换`T2`到`T1`存在，并在至少一个以下保留：</span><span class="sxs-lookup"><span data-stu-id="08161-805">Given two different types `T1` and `T2`, `T1` is a better conversion target than `T2` if no implicit conversion from `T2` to `T1` exists, and at least one of the following holds:</span></span>

*  <span data-ttu-id="08161-806">隐式转换`T1`到`T2`存在</span><span class="sxs-lookup"><span data-stu-id="08161-806">An implicit conversion from `T1` to `T2` exists</span></span>
*  <span data-ttu-id="08161-807">`T1` 是委托类型`D1`或表达式树类型`Expression<D1>`，`T2`是委托类型`D2`或表达式树类型`Expression<D2>`，`D1`具有返回类型`S1`以及之一以下包含：</span><span class="sxs-lookup"><span data-stu-id="08161-807">`T1` is either a delegate type `D1` or an expression tree type `Expression<D1>`, `T2` is either a delegate type `D2` or an expression tree type `Expression<D2>`, `D1` has a return type `S1` and one of the following holds:</span></span>
   * <span data-ttu-id="08161-808">`D2` 返回 void</span><span class="sxs-lookup"><span data-stu-id="08161-808">`D2` is void returning</span></span>
   * <span data-ttu-id="08161-809">`D2` 具有返回类型`S2`，和`S1`是比更好地转换目标 `S2`</span><span class="sxs-lookup"><span data-stu-id="08161-809">`D2` has a return type `S2`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="08161-810">`T1` 是`Task<S1>`，`T2`是`Task<S2>`，和`S1`是比更好地转换目标 `S2`</span><span class="sxs-lookup"><span data-stu-id="08161-810">`T1` is `Task<S1>`, `T2` is `Task<S2>`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="08161-811">`T1` 是`S1`或`S1?`其中`S1`是带符号的整型类型，以及`T2`是`S2`或`S2?`其中`S2`是无符号整数类型。</span><span class="sxs-lookup"><span data-stu-id="08161-811">`T1` is `S1` or `S1?` where `S1` is a signed integral type, and `T2` is `S2` or `S2?` where `S2` is an unsigned integral type.</span></span> <span data-ttu-id="08161-812">尤其是在下列情况下：</span><span class="sxs-lookup"><span data-stu-id="08161-812">Specifically:</span></span>
   * <span data-ttu-id="08161-813">`S1` 是`sbyte`并`S2`是`byte`， `ushort`， `uint`，或 `ulong`</span><span class="sxs-lookup"><span data-stu-id="08161-813">`S1` is `sbyte` and `S2` is `byte`, `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="08161-814">`S1` 是`short`并`S2`是`ushort`， `uint`，或 `ulong`</span><span class="sxs-lookup"><span data-stu-id="08161-814">`S1` is `short` and `S2` is `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="08161-815">`S1` 是`int`并`S2`是`uint`，或 `ulong`</span><span class="sxs-lookup"><span data-stu-id="08161-815">`S1` is `int` and `S2` is `uint`, or `ulong`</span></span>
   * <span data-ttu-id="08161-816">`S1` 是`long`和`S2`是 `ulong`</span><span class="sxs-lookup"><span data-stu-id="08161-816">`S1` is `long` and `S2` is `ulong`</span></span>

#### <a name="overloading-in-generic-classes"></a><span data-ttu-id="08161-817">泛型类中重载</span><span class="sxs-lookup"><span data-stu-id="08161-817">Overloading in generic classes</span></span>

<span data-ttu-id="08161-818">声明的签名必须是唯一的而是可能的类型参数替换的会导致完全相同的签名。</span><span class="sxs-lookup"><span data-stu-id="08161-818">While signatures as declared must be unique, it is possible that substitution of type arguments results in identical signatures.</span></span> <span data-ttu-id="08161-819">在这种情况下，更高版本的重载决策的决定性规则将挑选最明确的成员。</span><span class="sxs-lookup"><span data-stu-id="08161-819">In such cases, the tie-breaking rules of overload resolution above will pick the most specific member.</span></span>

<span data-ttu-id="08161-820">以下示例显示有效和无效根据此规则的重载：</span><span class="sxs-lookup"><span data-stu-id="08161-820">The following examples show overloads that are valid and invalid according to this rule:</span></span>

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

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a><span data-ttu-id="08161-821">编译时检查的动态重载决策</span><span class="sxs-lookup"><span data-stu-id="08161-821">Compile-time checking of dynamic overload resolution</span></span>

<span data-ttu-id="08161-822">对于最动态绑定操作的一套用于解析可能的候选项是在编译时未知。</span><span class="sxs-lookup"><span data-stu-id="08161-822">For most dynamically bound operations the set of possible candidates for resolution is unknown at compile-time.</span></span> <span data-ttu-id="08161-823">在某些情况下，但候选集知道在编译时：</span><span class="sxs-lookup"><span data-stu-id="08161-823">In certain cases, however the candidate set is known at compile-time:</span></span>

*  <span data-ttu-id="08161-824">使用动态参数的静态方法调用</span><span class="sxs-lookup"><span data-stu-id="08161-824">Static method calls with dynamic arguments</span></span>
*  <span data-ttu-id="08161-825">接收方不是动态表达式的实例方法调用</span><span class="sxs-lookup"><span data-stu-id="08161-825">Instance method calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="08161-826">索引器调用接收方不是动态表达式</span><span class="sxs-lookup"><span data-stu-id="08161-826">Indexer calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="08161-827">使用动态参数的构造函数调用</span><span class="sxs-lookup"><span data-stu-id="08161-827">Constructor calls with dynamic arguments</span></span>

<span data-ttu-id="08161-828">在这些情况下每个候选，若要查看是否其中任何一个无法可能是应用在运行时执行有限的编译时检查。此检查包括以下步骤：</span><span class="sxs-lookup"><span data-stu-id="08161-828">In these cases a limited compile-time check is performed for each candidate to see if any of them could possibly apply at run-time.This check consists of the following steps:</span></span>

*  <span data-ttu-id="08161-829">分部类型推断：任何类型不依赖于直接或间接的类型参数的实参`dynamic`使用的规则推断[类型推理](expressions.md#type-inference)。</span><span class="sxs-lookup"><span data-stu-id="08161-829">Partial type inference: Any type argument that does not depend directly or indirectly on an argument of type `dynamic` is inferred using the rules of [Type inference](expressions.md#type-inference).</span></span> <span data-ttu-id="08161-830">其余的类型参数是未知的。</span><span class="sxs-lookup"><span data-stu-id="08161-830">The remaining type arguments are unknown.</span></span>
*  <span data-ttu-id="08161-831">部分适用性检查：根据检查适用性[适用函数成员](expressions.md#applicable-function-member)，但忽略其类型是未知的参数。</span><span class="sxs-lookup"><span data-stu-id="08161-831">Partial applicability check: Applicability is checked according to [Applicable function member](expressions.md#applicable-function-member), but ignoring parameters whose types are unknown.</span></span>
*  <span data-ttu-id="08161-832">如果没有候选人通过此测试，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-832">If no candidate passes this test, a compile-time error occurs.</span></span>

### <a name="function-member-invocation"></a><span data-ttu-id="08161-833">函数成员调用</span><span class="sxs-lookup"><span data-stu-id="08161-833">Function member invocation</span></span>

<span data-ttu-id="08161-834">本部分介绍在运行时调用特定函数成员的过程。</span><span class="sxs-lookup"><span data-stu-id="08161-834">This section describes the process that takes place at run-time to invoke a particular function member.</span></span> <span data-ttu-id="08161-835">假定绑定时进程具有已确定要调用的可能采用重载解析到一组候选函数成员的特定成员。</span><span class="sxs-lookup"><span data-stu-id="08161-835">It is assumed that a binding-time process has already determined the particular member to invoke, possibly by applying overload resolution to a set of candidate function members.</span></span>

<span data-ttu-id="08161-836">用于描述在调用进程的目的，函数成员分为两类：</span><span class="sxs-lookup"><span data-stu-id="08161-836">For purposes of describing the invocation process, function members are divided into two categories:</span></span>

*  <span data-ttu-id="08161-837">静态函数成员。</span><span class="sxs-lookup"><span data-stu-id="08161-837">Static function members.</span></span> <span data-ttu-id="08161-838">这些是实例构造函数、 静态方法、 静态属性访问器和用户定义的运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-838">These are instance constructors, static methods, static property accessors, and user-defined operators.</span></span> <span data-ttu-id="08161-839">静态函数成员始终是非虚拟的。</span><span class="sxs-lookup"><span data-stu-id="08161-839">Static function members are always non-virtual.</span></span>
*  <span data-ttu-id="08161-840">实例函数成员。</span><span class="sxs-lookup"><span data-stu-id="08161-840">Instance function members.</span></span> <span data-ttu-id="08161-841">这些是实例方法、 实例属性访问器和索引器访问器。</span><span class="sxs-lookup"><span data-stu-id="08161-841">These are instance methods, instance property accessors, and indexer accessors.</span></span> <span data-ttu-id="08161-842">实例函数成员为非虚拟或虚拟的并且始终调用的特定实例上。</span><span class="sxs-lookup"><span data-stu-id="08161-842">Instance function members are either non-virtual or virtual, and are always invoked on a particular instance.</span></span> <span data-ttu-id="08161-843">实例计算的实例表达式，且可访问作为函数成员内`this`([此访问权限](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="08161-843">The instance is computed by an instance expression, and it becomes accessible within the function member as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="08161-844">函数成员调用的运行时处理包括以下步骤，其中`M`是函数成员，并且在`M`是实例成员，`E`的实例表达式：</span><span class="sxs-lookup"><span data-stu-id="08161-844">The run-time processing of a function member invocation consists of the following steps, where `M` is the function member and, if `M` is an instance member, `E` is the instance expression:</span></span>

*  <span data-ttu-id="08161-845">如果`M`是静态函数成员：</span><span class="sxs-lookup"><span data-stu-id="08161-845">If `M` is a static function member:</span></span>
   * <span data-ttu-id="08161-846">参数列表中所述的顺序进行计算[自变量列表](expressions.md#argument-lists)。</span><span class="sxs-lookup"><span data-stu-id="08161-846">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="08161-847">`M` 调用。</span><span class="sxs-lookup"><span data-stu-id="08161-847">`M` is invoked.</span></span>

*  <span data-ttu-id="08161-848">如果`M`中声明的实例函数成员*value_type*:</span><span class="sxs-lookup"><span data-stu-id="08161-848">If `M` is an instance function member declared in a *value_type*:</span></span>
   * <span data-ttu-id="08161-849">`E` 计算。</span><span class="sxs-lookup"><span data-stu-id="08161-849">`E` is evaluated.</span></span> <span data-ttu-id="08161-850">如果此计算导致了异常，然后不执行任何进一步的操作。</span><span class="sxs-lookup"><span data-stu-id="08161-850">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="08161-851">如果`E`未归类为变量，然后的临时本地变量`E`的创建类型和值`E`分配给该变量。</span><span class="sxs-lookup"><span data-stu-id="08161-851">If `E` is not classified as a variable, then a temporary local variable of `E`'s type is created and the value of `E` is assigned to that variable.</span></span> <span data-ttu-id="08161-852">`E` 然后重新归类为对该临时本地变量的引用。</span><span class="sxs-lookup"><span data-stu-id="08161-852">`E` is then reclassified as a reference to that temporary local variable.</span></span> <span data-ttu-id="08161-853">临时变量是可访问性`this`内`M`，但不是在另一种方法。</span><span class="sxs-lookup"><span data-stu-id="08161-853">The temporary variable is accessible as `this` within `M`, but not in any other way.</span></span> <span data-ttu-id="08161-854">因此，仅当`E`是 true 变量是调用方以观察所做的更改的`M`对`this`。</span><span class="sxs-lookup"><span data-stu-id="08161-854">Thus, only when `E` is a true variable is it possible for the caller to observe the changes that `M` makes to `this`.</span></span>
   * <span data-ttu-id="08161-855">参数列表中所述的顺序进行计算[自变量列表](expressions.md#argument-lists)。</span><span class="sxs-lookup"><span data-stu-id="08161-855">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="08161-856">`M` 调用。</span><span class="sxs-lookup"><span data-stu-id="08161-856">`M` is invoked.</span></span> <span data-ttu-id="08161-857">通过所引用的变量`E`成为引用变量`this`。</span><span class="sxs-lookup"><span data-stu-id="08161-857">The variable referenced by `E` becomes the variable referenced by `this`.</span></span>

*  <span data-ttu-id="08161-858">如果`M`中声明的实例函数成员*reference_type*:</span><span class="sxs-lookup"><span data-stu-id="08161-858">If `M` is an instance function member declared in a *reference_type*:</span></span>
   * <span data-ttu-id="08161-859">`E` 计算。</span><span class="sxs-lookup"><span data-stu-id="08161-859">`E` is evaluated.</span></span> <span data-ttu-id="08161-860">如果此计算导致了异常，然后不执行任何进一步的操作。</span><span class="sxs-lookup"><span data-stu-id="08161-860">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="08161-861">参数列表中所述的顺序进行计算[自变量列表](expressions.md#argument-lists)。</span><span class="sxs-lookup"><span data-stu-id="08161-861">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="08161-862">如果类型`E`是*value_type*，装箱转换 ([装箱转换](types.md#boxing-conversions)) 执行来将转换`E`键入`object`，和`E`被视为为类型`object`中的以下步骤。</span><span class="sxs-lookup"><span data-stu-id="08161-862">If the type of `E` is a *value_type*, a boxing conversion ([Boxing conversions](types.md#boxing-conversions)) is performed to convert `E` to type `object`, and `E` is considered to be of type `object` in the following steps.</span></span> <span data-ttu-id="08161-863">在这种情况下，`M`只能属于`System.Object`。</span><span class="sxs-lookup"><span data-stu-id="08161-863">In this case, `M` could only be a member of `System.Object`.</span></span>
   * <span data-ttu-id="08161-864">值`E`进行检查，以保持有效。</span><span class="sxs-lookup"><span data-stu-id="08161-864">The value of `E` is checked to be valid.</span></span> <span data-ttu-id="08161-865">如果的值`E`是`null`、`System.NullReferenceException`引发并执行任何进一步的操作。</span><span class="sxs-lookup"><span data-stu-id="08161-865">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
   * <span data-ttu-id="08161-866">确定要调用的函数成员实现：</span><span class="sxs-lookup"><span data-stu-id="08161-866">The function member implementation to invoke is determined:</span></span>
     * <span data-ttu-id="08161-867">如果绑定时类型`E`是一个接口，要调用的函数成员是实现`M`提供的引用的实例的运行时类型`E`。</span><span class="sxs-lookup"><span data-stu-id="08161-867">If the binding-time type of `E` is an interface, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="08161-868">通过应用的接口映射规则来确定此函数成员 ([接口映射](interfaces.md#interface-mapping)) 来确定的实现`M`提供的引用的实例的运行时类型`E`。</span><span class="sxs-lookup"><span data-stu-id="08161-868">This function member is determined by applying the interface mapping rules ([Interface mapping](interfaces.md#interface-mapping)) to determine the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="08161-869">否则为如果`M`是虚函数成员，要调用的函数成员是实现`M`提供的引用的实例的运行时类型`E`。</span><span class="sxs-lookup"><span data-stu-id="08161-869">Otherwise, if `M` is a virtual function member, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="08161-870">通过应用用于确定派生程度最高的实现的规则来确定此函数成员 ([虚拟方法](classes.md#virtual-methods)) 的`M`相对于引用的实例的运行时类型`E`。</span><span class="sxs-lookup"><span data-stu-id="08161-870">This function member is determined by applying the rules for determining the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of `M` with respect to the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="08161-871">否则为`M`所在的非虚拟函数，并要调用的函数成员是`M`本身。</span><span class="sxs-lookup"><span data-stu-id="08161-871">Otherwise, `M` is a non-virtual function member, and the function member to invoke is `M` itself.</span></span>
   * <span data-ttu-id="08161-872">调用在上述步骤中确定的函数成员实现。</span><span class="sxs-lookup"><span data-stu-id="08161-872">The function member implementation determined in the step above is invoked.</span></span> <span data-ttu-id="08161-873">所引用的对象`E`所引用的对象将成为`this`。</span><span class="sxs-lookup"><span data-stu-id="08161-873">The object referenced by `E` becomes the object referenced by `this`.</span></span>

#### <a name="invocations-on-boxed-instances"></a><span data-ttu-id="08161-874">装箱的实例上调用</span><span class="sxs-lookup"><span data-stu-id="08161-874">Invocations on boxed instances</span></span>

<span data-ttu-id="08161-875">在中实现的函数成员*value_type*可以通过的已装箱的实例调用*value_type*在以下情况下：</span><span class="sxs-lookup"><span data-stu-id="08161-875">A function member implemented in a *value_type* can be invoked through a boxed instance of that *value_type* in the following situations:</span></span>

*  <span data-ttu-id="08161-876">函数成员何时`override`继承自类型的方法`object`和类型的实例表达式通过调用`object`。</span><span class="sxs-lookup"><span data-stu-id="08161-876">When the function member is an `override` of a method inherited from type `object` and is invoked through an instance expression of type `object`.</span></span>
*  <span data-ttu-id="08161-877">当函数成员是一种实现的接口函数成员和的实例表达式通过调用*interface_type*。</span><span class="sxs-lookup"><span data-stu-id="08161-877">When the function member is an implementation of an interface function member and is invoked through an instance expression of an *interface_type*.</span></span>
*  <span data-ttu-id="08161-878">当通过委托调用的函数成员。</span><span class="sxs-lookup"><span data-stu-id="08161-878">When the function member is invoked through a delegate.</span></span>

<span data-ttu-id="08161-879">在这些情况下，则认为该装箱的实例包含的变量*value_type*，并且此变量成为引用的变量`this`内函数成员调用。</span><span class="sxs-lookup"><span data-stu-id="08161-879">In these situations, the boxed instance is considered to contain a variable of the *value_type*, and this variable becomes the variable referenced by `this` within the function member invocation.</span></span> <span data-ttu-id="08161-880">具体而言，这意味着，当装箱实例上调用的函数成员，很可能要修改的已装箱的实例中包含的值的函数成员。</span><span class="sxs-lookup"><span data-stu-id="08161-880">In particular, this means that when a function member is invoked on a boxed instance, it is possible for the function member to modify the value contained in the boxed instance.</span></span>

## <a name="primary-expressions"></a><span data-ttu-id="08161-881">主表达式</span><span class="sxs-lookup"><span data-stu-id="08161-881">Primary expressions</span></span>

<span data-ttu-id="08161-882">主表达式包含表达式的最简单形式。</span><span class="sxs-lookup"><span data-stu-id="08161-882">Primary expressions include the simplest forms of expressions.</span></span>

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

<span data-ttu-id="08161-883">主表达式分为*array_creation_expression*s 和*primary_no_array_creation_expression*s。</span><span class="sxs-lookup"><span data-stu-id="08161-883">Primary expressions are divided between *array_creation_expression*s and *primary_no_array_creation_expression*s.</span></span> <span data-ttu-id="08161-884">以这种方式处理数组创建表达式，而不是列出的其他简单表达式窗体，以及使语法如禁止可能令人混淆的代码</span><span class="sxs-lookup"><span data-stu-id="08161-884">Treating array-creation-expression in this way, rather than listing it along with the other simple expression forms, enables the grammar to disallow potentially confusing code such as</span></span>
```csharp
object o = new int[3][1];
```
<span data-ttu-id="08161-885">它将否则解释为</span><span class="sxs-lookup"><span data-stu-id="08161-885">which would otherwise be interpreted as</span></span>
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a><span data-ttu-id="08161-886">文本</span><span class="sxs-lookup"><span data-stu-id="08161-886">Literals</span></span>

<span data-ttu-id="08161-887">一个*primary_expression*组成*文字*([文本](lexical-structure.md#literals)) 分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="08161-887">A *primary_expression* that consists of a *literal* ([Literals](lexical-structure.md#literals)) is classified as a value.</span></span>


### <a name="interpolated-strings"></a><span data-ttu-id="08161-888">内插字符串</span><span class="sxs-lookup"><span data-stu-id="08161-888">Interpolated strings</span></span>

<span data-ttu-id="08161-889">*Interpolated_string_expression*组成`$`号后跟正则或原义字符串文本、 漏洞，其中由分隔`{`和`}`、 括起来的表达式和格式设置规范。</span><span class="sxs-lookup"><span data-stu-id="08161-889">An *interpolated_string_expression* consists of a `$` sign followed by a regular or verbatim string literal, wherein holes, delimited by `{` and `}`, enclose expressions and formatting specifications.</span></span> <span data-ttu-id="08161-890">内插的字符串表达式是对*interpolated_string_literal* ，已被拆分成各个单独的标记中所述[内插字符串文本](lexical-structure.md#interpolated-string-literals)。</span><span class="sxs-lookup"><span data-stu-id="08161-890">An interpolated string expression is the result of an *interpolated_string_literal* that has been broken up into individual tokens, as described in [Interpolated string literals](lexical-structure.md#interpolated-string-literals).</span></span>

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

<span data-ttu-id="08161-891">*Constant_expression*内插中必须具有隐式转换为`int`。</span><span class="sxs-lookup"><span data-stu-id="08161-891">The *constant_expression* in an interpolation must have an implicit conversion to `int`.</span></span>

<span data-ttu-id="08161-892">*Interpolated_string_expression*分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="08161-892">An *interpolated_string_expression* is classified as a value.</span></span> <span data-ttu-id="08161-893">如果立即将其转换为`System.IFormattable`或`System.FormattableString`隐式的内插的字符串转换 ([隐式内插字符串转换](conversions.md#implicit-interpolated-string-conversions)) 内, 插的字符串表达式也为该类型。</span><span class="sxs-lookup"><span data-stu-id="08161-893">If it is immediately converted to `System.IFormattable` or `System.FormattableString` with an implicit interpolated string conversion ([Implicit interpolated string conversions](conversions.md#implicit-interpolated-string-conversions)), the interpolated string expression has that type.</span></span> <span data-ttu-id="08161-894">否则，它具有类型`string`。</span><span class="sxs-lookup"><span data-stu-id="08161-894">Otherwise, it has the type `string`.</span></span>

<span data-ttu-id="08161-895">如果内插字符串的类型为`System.IFormattable`或`System.FormattableString`，含义是对的调用`System.Runtime.CompilerServices.FormattableStringFactory.Create`。</span><span class="sxs-lookup"><span data-stu-id="08161-895">If the type of an interpolated string is `System.IFormattable` or `System.FormattableString`, the meaning is a call to `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span></span> <span data-ttu-id="08161-896">如果类型为`string`，该表达式的含义是对的调用`string.Format`。</span><span class="sxs-lookup"><span data-stu-id="08161-896">If the type is `string`, the meaning of the expression is a call to `string.Format`.</span></span> <span data-ttu-id="08161-897">在这两种情况下，调用的参数列表包含格式字符串文本，与每个内插的占位符和对应的占位符的每个表达式的参数。</span><span class="sxs-lookup"><span data-stu-id="08161-897">In both cases, the argument list of the call consists of a format string literal with placeholders for each interpolation, and an argument for each expression corresponding to the place holders.</span></span>

<span data-ttu-id="08161-898">格式字符串文本构造，如下所示，其中`N`是数中的内插*interpolated_string_expression*:</span><span class="sxs-lookup"><span data-stu-id="08161-898">The format string literal is constructed as follows, where `N` is the number of interpolations in the *interpolated_string_expression*:</span></span>

*  <span data-ttu-id="08161-899">如果*interpolated_regular_string_whole*或*interpolated_verbatim_string_whole*遵循`$`登录，则格式字符串文本是该令牌。</span><span class="sxs-lookup"><span data-stu-id="08161-899">If an *interpolated_regular_string_whole* or an *interpolated_verbatim_string_whole* follows the `$` sign, then the format string literal is that token.</span></span>
*  <span data-ttu-id="08161-900">否则，文本的格式字符串组成：</span><span class="sxs-lookup"><span data-stu-id="08161-900">Otherwise, the format string literal consists of:</span></span> 
   *  <span data-ttu-id="08161-901">第一个*interpolated_regular_string_start*或*interpolated_verbatim_string_start*</span><span class="sxs-lookup"><span data-stu-id="08161-901">First the *interpolated_regular_string_start* or *interpolated_verbatim_string_start*</span></span>
   *  <span data-ttu-id="08161-902">然后对于每个数字`I`从`0`到`N-1`:</span><span class="sxs-lookup"><span data-stu-id="08161-902">Then for each number `I` from `0` to `N-1`:</span></span> 
      * <span data-ttu-id="08161-903">十进制表示形式 `I`</span><span class="sxs-lookup"><span data-stu-id="08161-903">The decimal representation of `I`</span></span>
      * <span data-ttu-id="08161-904">然后，如果相应*内插*已*constant_expression*即`,`（逗号） 的值的十进制表示形式后, 跟*constant_expression*</span><span class="sxs-lookup"><span data-stu-id="08161-904">Then, if the corresponding *interpolation* has a *constant_expression*, a `,` (comma) followed by the decimal representation of the value of the *constant_expression*</span></span>
      * <span data-ttu-id="08161-905">然后*interpolated_regular_string_mid*， *interpolated_regular_string_end*， *interpolated_verbatim_string_mid*或*interpolated_verbatim_string_end*紧随相应的内插。</span><span class="sxs-lookup"><span data-stu-id="08161-905">Then the *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* or *interpolated_verbatim_string_end* immediately following the corresponding interpolation.</span></span>

<span data-ttu-id="08161-906">后续参数将只需*表达式*从*内插*（如果有），按顺序。</span><span class="sxs-lookup"><span data-stu-id="08161-906">The subsequent arguments are simply the *expressions* from the *interpolations* (if any), in order.</span></span>

<span data-ttu-id="08161-907">TODO： 示例。</span><span class="sxs-lookup"><span data-stu-id="08161-907">TODO: examples.</span></span>


### <a name="simple-names"></a><span data-ttu-id="08161-908">简单名称</span><span class="sxs-lookup"><span data-stu-id="08161-908">Simple names</span></span>

<span data-ttu-id="08161-909">一个*simple_name*包含的标识符，可以选择后跟类型参数列表：</span><span class="sxs-lookup"><span data-stu-id="08161-909">A *simple_name* consists of an identifier, optionally followed by a type argument list:</span></span>

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

<span data-ttu-id="08161-910">一个*simple_name*是窗体`I`或窗体`I<A1,...,Ak>`，其中`I`是单个标识符和`<A1,...,Ak>`是一个可选*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="08161-910">A *simple_name* is either of the form `I` or of the form `I<A1,...,Ak>`, where `I` is a single identifier and `<A1,...,Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="08161-911">如果未*type_argument_list*是指定，请考虑`K`为零。</span><span class="sxs-lookup"><span data-stu-id="08161-911">When no *type_argument_list* is specified, consider `K` to be zero.</span></span> <span data-ttu-id="08161-912">*Simple_name*计算和分类，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-912">The *simple_name* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="08161-913">如果`K`为零， *simple_name*中将显示*块*如果*块*的 (或封闭*块*的) 本地变量声明空间 ([声明](basic-concepts.md#declarations)) 包含本地变量、 参数或具有名称的常量 `I`，然后*simple_name*引用该本地变量参数或常量和分类为变量或值。</span><span class="sxs-lookup"><span data-stu-id="08161-913">If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>
*  <span data-ttu-id="08161-914">如果`K`为零， *simple_name*出现在泛型方法声明的主体中，如果该声明包含名称的类型参数 `I`，则*simple_name*指的是该类型参数。</span><span class="sxs-lookup"><span data-stu-id="08161-914">If `K` is zero and the *simple_name* appears within the body of a generic method declaration and if that declaration includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
*  <span data-ttu-id="08161-915">否则，为每个实例类型 `T`([的实例类型](classes.md#the-instance-type))，从最近的封闭类型声明的实例类型开始，继续执行的每个封闭类或结构的实例类型声明 （如果有）：</span><span class="sxs-lookup"><span data-stu-id="08161-915">Otherwise, for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of the immediately enclosing type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
   *  <span data-ttu-id="08161-916">如果`K`是零的声明`T`包括具有名称的类型参数 `I`，然后*simple_name*引用该类型参数。</span><span class="sxs-lookup"><span data-stu-id="08161-916">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
   *  <span data-ttu-id="08161-917">否则为如果成员查找 ([成员查找](expressions.md#member-lookup)) 的`I`中`T`与`K` 类型自变量生成匹配项：</span><span class="sxs-lookup"><span data-stu-id="08161-917">Otherwise, if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match:</span></span>
      * <span data-ttu-id="08161-918">如果`T`是否立即封闭类或结构类型的实例类型和查找识别一个或多个方法，结果是方法组，其中的关联的实例表达式`this`。</span><span class="sxs-lookup"><span data-stu-id="08161-918">If `T` is the instance type of the immediately enclosing class or struct type and the lookup identifies one or more methods, the result is a method group with an associated instance expression of `this`.</span></span> <span data-ttu-id="08161-919">如果指定了类型自变量列表，它将在调用泛型方法中使用 ([方法调用](expressions.md#method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="08161-919">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
      * <span data-ttu-id="08161-920">否则为如果`T`如果查找标识一个实例成员，并且引用出现在实例构造函数、 实例方法或实例访问器的主体是最近的封闭类或结构类型的实例类型结果是相同的成员访问 ([成员访问](expressions.md#member-access)) 的窗体`this.I`。</span><span class="sxs-lookup"><span data-stu-id="08161-920">Otherwise, if `T` is the instance type of the immediately enclosing class or struct type, if the lookup identifies an instance member, and if the reference occurs within the body of an instance constructor, an instance method, or an instance accessor, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `this.I`.</span></span> <span data-ttu-id="08161-921">才会发生此时`K`为零。</span><span class="sxs-lookup"><span data-stu-id="08161-921">This can only happen when `K` is zero.</span></span>
      * <span data-ttu-id="08161-922">否则，结果是相同的成员访问 ([成员访问](expressions.md#member-access)) 的窗体`T.I`或`T.I<A1,...,Ak>`。</span><span class="sxs-lookup"><span data-stu-id="08161-922">Otherwise, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `T.I` or `T.I<A1,...,Ak>`.</span></span> <span data-ttu-id="08161-923">在这种情况下，它是的绑定时错误*simple_name*来指代的实例成员。</span><span class="sxs-lookup"><span data-stu-id="08161-923">In this case, it is a binding-time error for the *simple_name* to refer to an instance member.</span></span>

*  <span data-ttu-id="08161-924">否则为每个命名空间 `N`，在其中命名空间从开始*simple_name*发生，继续到每个封闭命名空间 （如果有），结束与全局命名空间，执行以下步骤只有一个实体就是位于才进行计算：</span><span class="sxs-lookup"><span data-stu-id="08161-924">Otherwise, for each namespace `N`, starting with the namespace in which the *simple_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
   *  <span data-ttu-id="08161-925">如果`K`为零并`I`中的命名空间名称 `N`，然后：</span><span class="sxs-lookup"><span data-stu-id="08161-925">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
      * <span data-ttu-id="08161-926">如果位置其中*simple_name*发生的命名空间声明括`N`和命名空间声明中包含*extern_alias_directive*或*using_alias_directive* ，将名称相关联 `I`与命名空间或类型，则*simple_name*不明确，并且发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-926">If the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="08161-927">否则为*simple_name*指的是名为的命名空间`I`中`N`。</span><span class="sxs-lookup"><span data-stu-id="08161-927">Otherwise, the *simple_name* refers to the namespace named `I` in `N`.</span></span>
   *  <span data-ttu-id="08161-928">否则为如果`N`包含可访问类型具有名称 `I`并`K` 类型形参，则：</span><span class="sxs-lookup"><span data-stu-id="08161-928">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
      * <span data-ttu-id="08161-929">如果`K`是零和位置， *simple_name*发生括起来的命名空间声明`N`和命名空间声明中包含*extern_alias_directive*或*using_alias_directive*将关联名称 `I`与命名空间或类型，则*simple_name*不明确，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-929">If `K` is zero and the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="08161-930">否则为*namespace_or_type_name*指构造具有给定的类型参数的类型。</span><span class="sxs-lookup"><span data-stu-id="08161-930">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="08161-931">否则为如果位置其中*simple_name*发生括起来的命名空间声明 `N`:</span><span class="sxs-lookup"><span data-stu-id="08161-931">Otherwise, if the location where the *simple_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
      * <span data-ttu-id="08161-932">如果`K`为零，并且命名空间声明中包含*extern_alias_directive*或*using_alias_directive*将关联名称 `I`与导入的命名空间或类型，则*simple_name*指的是该命名空间或类型。</span><span class="sxs-lookup"><span data-stu-id="08161-932">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *simple_name* refers to that namespace or type.</span></span>
      * <span data-ttu-id="08161-933">否则为如果通过导入的命名空间和类型声明*using_namespace_directive*s 和*using_static_directive*的命名空间声明包含一个可访问类型或具有名称的非扩展静态成员 `I`并`K` 类型参数，则*simple_name*指的是该类型或成员构造具有给定的类型参数。</span><span class="sxs-lookup"><span data-stu-id="08161-933">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_static_directive*s of the namespace declaration contain exactly one accessible type or non-extension static member having name `I` and `K` type parameters, then the *simple_name* refers to that type or member constructed with the given type arguments.</span></span>
      * <span data-ttu-id="08161-934">否则为如果通过导入的命名空间和类型*using_namespace_directive*的命名空间声明包含多个可访问类型或非扩展方法具有名称的静态成员 `I`和`K` 类型参数，则*simple_name*不明确并产生一个错误。</span><span class="sxs-lookup"><span data-stu-id="08161-934">Otherwise, if the namespaces and types imported by the *using_namespace_directive*s of the namespace declaration contain more than one accessible type or non-extension-method static member having name `I` and `K` type parameters, then the *simple_name* is ambiguous and an error occurs.</span></span>

   <span data-ttu-id="08161-935">请注意，此整个步骤中的处理相应的步骤完全并行*namespace_or_type_name* ([Namespace 和类型名称](basic-concepts.md#namespace-and-type-names))。</span><span class="sxs-lookup"><span data-stu-id="08161-935">Note that this entire step is exactly parallel to the corresponding step in the processing of a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span>

*  <span data-ttu-id="08161-936">否则为*simple_name*是未定义，且将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-936">Otherwise, the *simple_name* is undefined and a compile-time error occurs.</span></span>


### <a name="parenthesized-expressions"></a><span data-ttu-id="08161-937">带括号的表达式</span><span class="sxs-lookup"><span data-stu-id="08161-937">Parenthesized expressions</span></span>

<span data-ttu-id="08161-938">一个*parenthesized_expression*组成*表达式*括在括号中。</span><span class="sxs-lookup"><span data-stu-id="08161-938">A *parenthesized_expression* consists of an *expression* enclosed in parentheses.</span></span>

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

<span data-ttu-id="08161-939">一个*parenthesized_expression*通过评估来评估计算*表达式*在括号中。</span><span class="sxs-lookup"><span data-stu-id="08161-939">A *parenthesized_expression* is evaluated by evaluating the *expression* within the parentheses.</span></span> <span data-ttu-id="08161-940">如果*表达式*在括号中表示的命名空间或类型，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-940">If the *expression* within the parentheses denotes a namespace or type, a compile-time error occurs.</span></span> <span data-ttu-id="08161-941">否则为结果*parenthesized_expression*是所包含的计算结果*表达式*。</span><span class="sxs-lookup"><span data-stu-id="08161-941">Otherwise, the result of the *parenthesized_expression* is the result of the evaluation of the contained *expression*.</span></span>

### <a name="member-access"></a><span data-ttu-id="08161-942">成员访问</span><span class="sxs-lookup"><span data-stu-id="08161-942">Member access</span></span>

<span data-ttu-id="08161-943">一个*member_access*组成*primary_expression*即*predefined_type*，或者*qualified_alias_member*后, 跟"`.`"令牌后, 跟*标识符*，后面可跟*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="08161-943">A *member_access* consists of a *primary_expression*, a *predefined_type*, or a *qualified_alias_member*, followed by a "`.`" token, followed by an *identifier*, optionally followed by a *type_argument_list*.</span></span>

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

<span data-ttu-id="08161-944">*Qualified_alias_member*中定义生产[Namespace 别名限定符](namespaces.md#namespace-alias-qualifiers)。</span><span class="sxs-lookup"><span data-stu-id="08161-944">The *qualified_alias_member* production is defined in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span>

<span data-ttu-id="08161-945">一个*member_access*是窗体`E.I`或窗体`E.I<A1, ..., Ak>`，其中`E`是主表达式`I`是单个标识符和`<A1, ..., Ak>`是可选的*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="08161-945">A *member_access* is either of the form `E.I` or of the form `E.I<A1, ..., Ak>`, where `E` is a primary-expression, `I` is a single identifier and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="08161-946">如果未*type_argument_list*是指定，请考虑`K`为零。</span><span class="sxs-lookup"><span data-stu-id="08161-946">When no *type_argument_list* is specified, consider `K` to be zero.</span></span>

<span data-ttu-id="08161-947">一个*member_access*与*primary_expression*类型的`dynamic`动态绑定 ([动态绑定](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="08161-947">A *member_access* with a *primary_expression* of type `dynamic` is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="08161-948">在这种情况下，编译器将成员访问分类为属性访问类型的`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="08161-948">In this case the compiler classifies the member access as a property access of type `dynamic`.</span></span> <span data-ttu-id="08161-949">规则可确定的含义*member_access*然后，在运行时，使用运行时类型而非编译时类型的应用*primary_expression*。</span><span class="sxs-lookup"><span data-stu-id="08161-949">The rules below to determine the meaning of the *member_access* are then applied at run-time, using the run-time type instead of the compile-time type of the *primary_expression*.</span></span> <span data-ttu-id="08161-950">如果此运行时分类的潜在顾客到方法组，则必须是成员访问*primary_expression*的*invocation_expression*。</span><span class="sxs-lookup"><span data-stu-id="08161-950">If this run-time classification leads to a method group, then the member access must be the *primary_expression* of an *invocation_expression*.</span></span>

<span data-ttu-id="08161-951">*Member_access*计算和分类，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-951">The *member_access* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="08161-952">如果`K`为零并`E`是一个命名空间和`E`包含嵌套的命名空间与名称 `I`，则结果为该命名空间。</span><span class="sxs-lookup"><span data-stu-id="08161-952">If `K` is zero and `E` is a namespace and `E` contains a nested namespace with name `I`, then the result is that namespace.</span></span>
*  <span data-ttu-id="08161-953">否则为如果`E`是一个命名空间和`E`包含可访问类型具有名称 `I`并`K` 类型参数，则结果为构造具有给定的类型参数的该类型。</span><span class="sxs-lookup"><span data-stu-id="08161-953">Otherwise, if `E` is a namespace and `E` contains an accessible type having name `I` and `K` type parameters, then the result is that type constructed with the given type arguments.</span></span>
*  <span data-ttu-id="08161-954">如果`E`是*predefined_type*或*primary_expression*归类为一种类型，如果`E`不是类型参数，并且如果成员查找 ([成员查找](expressions.md#member-lookup))`I`中`E`与`K` 类型参数生成匹配项，然后`E.I`计算和分类，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-954">If `E` is a *predefined_type* or a *primary_expression* classified as a type, if `E` is not a type parameter, and if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `E` with `K` type parameters produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="08161-955">如果`I`标识的类型，则结果为构造具有给定的类型参数的该类型。</span><span class="sxs-lookup"><span data-stu-id="08161-955">If `I` identifies a type, then the result is that type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="08161-956">如果`I`标识一个或多个方法，则结果为方法组没有关联的实例表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-956">If `I` identifies one or more methods, then the result is a method group with no associated instance expression.</span></span> <span data-ttu-id="08161-957">如果指定了类型自变量列表，它将在调用泛型方法中使用 ([方法调用](expressions.md#method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="08161-957">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="08161-958">如果`I`标识`static`属性，则结果是不包含关联的实例表达式的属性访问。</span><span class="sxs-lookup"><span data-stu-id="08161-958">If `I` identifies a `static` property, then the result is a property access with no associated instance expression.</span></span>
   *  <span data-ttu-id="08161-959">如果`I`标识`static`字段：</span><span class="sxs-lookup"><span data-stu-id="08161-959">If `I` identifies a `static` field:</span></span>
      * <span data-ttu-id="08161-960">如果此字段为`readonly`和引用发生之外的静态构造函数的类或结构在其中声明该字段，则结果为一个值，即静态字段的值 `I`中 `E`。</span><span class="sxs-lookup"><span data-stu-id="08161-960">If the field is `readonly` and the reference occurs outside the static constructor of the class or struct in which the field is declared, then the result is a value, namely the value of the static field `I` in `E`.</span></span>
      * <span data-ttu-id="08161-961">否则，结果是一个变量，即静态字段 `I`在 `E`。</span><span class="sxs-lookup"><span data-stu-id="08161-961">Otherwise, the result is a variable, namely the static field `I` in `E`.</span></span>
   *  <span data-ttu-id="08161-962">如果`I`标识`static`事件：</span><span class="sxs-lookup"><span data-stu-id="08161-962">If `I` identifies a `static` event:</span></span>
      * <span data-ttu-id="08161-963">如果引用出现在类或结构在其中声明了事件，并声明了事件，而无需*event_accessor_declarations* ([事件](classes.md#events))，然后`E.I`完全处理像`I`已静态字段。</span><span class="sxs-lookup"><span data-stu-id="08161-963">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), then `E.I` is processed exactly as if `I` were a static field.</span></span>
      * <span data-ttu-id="08161-964">否则，结果为无关联的实例表达式的事件访问。</span><span class="sxs-lookup"><span data-stu-id="08161-964">Otherwise, the result is an event access with no associated instance expression.</span></span>
   *  <span data-ttu-id="08161-965">如果`I`标识一个常量，则结果为一个值，即该常量的值。</span><span class="sxs-lookup"><span data-stu-id="08161-965">If `I` identifies a constant, then the result is a value, namely the value of that constant.</span></span>
    * <span data-ttu-id="08161-966">如果`I`标识枚举成员，则结果为一个值，即该枚举成员的值。</span><span class="sxs-lookup"><span data-stu-id="08161-966">If `I` identifies an enumeration member, then the result is a value, namely the value of that enumeration member.</span></span>
    * <span data-ttu-id="08161-967">否则为`E.I`是无效的成员引用，并且将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-967">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="08161-968">如果`E`是属性访问、 索引器访问、 变量或值的类型是 `T`，和成员查找 ([成员查找](expressions.md#member-lookup)) 的`I`中`T`与`K`  类型参数生成匹配项，然后`E.I`计算和分类，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-968">If `E` is a property access, indexer access, variable, or value, the type of which is `T`, and a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="08161-969">首先，如果`E`是一个属性或索引器访问，则属性的值或获取索引器访问 ([表达式的值](expressions.md#values-of-expressions)) 和`E`重新分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="08161-969">First, if `E` is a property or indexer access, then the value of the property or indexer access is obtained ([Values of expressions](expressions.md#values-of-expressions)) and `E` is reclassified as a value.</span></span>
   *  <span data-ttu-id="08161-970">如果`I`标识一个或多个方法，则结果为方法组的关联的实例表达式与`E`。</span><span class="sxs-lookup"><span data-stu-id="08161-970">If `I` identifies one or more methods, then the result is a method group with an associated instance expression of `E`.</span></span> <span data-ttu-id="08161-971">如果指定了类型自变量列表，它将在调用泛型方法中使用 ([方法调用](expressions.md#method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="08161-971">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="08161-972">如果`I`标识实例属性</span><span class="sxs-lookup"><span data-stu-id="08161-972">If `I` identifies an instance property,</span></span>
      * <span data-ttu-id="08161-973">如果`E`是`this`，`I`标识的自动实现的属性 ([自动实现的属性](classes.md#automatically-implemented-properties)) 没有 setter，并且引用出现在实例构造函数类或结构类型`T`，则结果为变量，即为给定的自动属性隐藏的支持字段`I`的实例中`T`由给定`this`。</span><span class="sxs-lookup"><span data-stu-id="08161-973">If `E` is `this`, `I` identifies an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)) without a setter, and the reference occurs within an instance constructor for a class or struct type `T`, then the result is a variable, namely the hidden backing field for the auto-property given by `I` in the instance of `T` given by `this`.</span></span>
      * <span data-ttu-id="08161-974">否则，结果是提供的关联的实例表达式的属性访问 `E`。</span><span class="sxs-lookup"><span data-stu-id="08161-974">Otherwise, the result is a property access with an associated instance expression of `E`.</span></span>
   *  <span data-ttu-id="08161-975">如果`T`是*class_type*并`I`标识的实例字段*class_type*:</span><span class="sxs-lookup"><span data-stu-id="08161-975">If `T` is a *class_type* and `I` identifies an instance field of that *class_type*:</span></span>
      * <span data-ttu-id="08161-976">如果的值`E`是`null`，然后`System.NullReferenceException`引发。</span><span class="sxs-lookup"><span data-stu-id="08161-976">If the value of `E` is `null`, then a `System.NullReferenceException` is thrown.</span></span>
      * <span data-ttu-id="08161-977">否则为如果该字段是`readonly`和发生的外部声明该字段的类的实例构造函数的引用，则结果为一个值，即字段的值 `I`中所引用的对象 `E`。</span><span class="sxs-lookup"><span data-stu-id="08161-977">Otherwise, if the field is `readonly` and the reference occurs outside an instance constructor of the class in which the field is declared, then the result is a value, namely the value of the field `I` in the object referenced by `E`.</span></span>
      * <span data-ttu-id="08161-978">否则，结果是一个变量，即字段 `I`中所引用的对象 `E`。</span><span class="sxs-lookup"><span data-stu-id="08161-978">Otherwise, the result is a variable, namely the field `I` in the object referenced by `E`.</span></span>
   *  <span data-ttu-id="08161-979">如果`T`是*struct_type*并`I`标识的实例字段*struct_type*:</span><span class="sxs-lookup"><span data-stu-id="08161-979">If `T` is a *struct_type* and `I` identifies an instance field of that *struct_type*:</span></span>
      * <span data-ttu-id="08161-980">如果`E`是一个值，或如果字段是`readonly`和引用出现在其中声明该字段，该结构的实例构造函数外，则结果为一个值，即字段的值 `I`中给定的结构实例 `E`.</span><span class="sxs-lookup"><span data-stu-id="08161-980">If `E` is a value, or if the field is `readonly` and the reference occurs outside an instance constructor of the struct in which the field is declared, then the result is a value, namely the value of the field `I` in the struct instance given by `E`.</span></span>
      * <span data-ttu-id="08161-981">否则，结果是一个变量，即字段 `I`中给定的结构实例 `E`。</span><span class="sxs-lookup"><span data-stu-id="08161-981">Otherwise, the result is a variable, namely the field `I` in the struct instance given by `E`.</span></span>
   *  <span data-ttu-id="08161-982">如果`I`标识实例事件：</span><span class="sxs-lookup"><span data-stu-id="08161-982">If `I` identifies an instance event:</span></span>
      * <span data-ttu-id="08161-983">如果引用出现在类或结构在其中声明了事件，并声明了事件，而无需*event_accessor_declarations* ([事件](classes.md#events))，并为不发生引用左上方`+=`或`-=`运算符，然后`E.I`完全处理像`I`是实例字段。</span><span class="sxs-lookup"><span data-stu-id="08161-983">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), and the reference does not occur as the left-hand side of a `+=` or `-=` operator, then `E.I` is processed exactly as if `I` was an instance field.</span></span>
      * <span data-ttu-id="08161-984">否则，会得到的关联的实例表达式的事件访问 `E`。</span><span class="sxs-lookup"><span data-stu-id="08161-984">Otherwise, the result is an event access with an associated instance expression of `E`.</span></span>
*  <span data-ttu-id="08161-985">否则，尝试处理`E.I`扩展方法调用的形式 ([扩展方法调用](expressions.md#extension-method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="08161-985">Otherwise, an attempt is made to process `E.I` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="08161-986">如果此操作失败，`E.I`是无效的成员引用，并且绑定时出错。</span><span class="sxs-lookup"><span data-stu-id="08161-986">If this fails, `E.I` is an invalid member reference, and a binding-time error occurs.</span></span>

#### <a name="identical-simple-names-and-type-names"></a><span data-ttu-id="08161-987">相同的简单名称和类型名称</span><span class="sxs-lookup"><span data-stu-id="08161-987">Identical simple names and type names</span></span>

<span data-ttu-id="08161-988">在窗体的成员访问`E.I`，如果`E`是一个单一的标识符，并且如果的含义`E`作为*simple_name* ([简单名称](expressions.md#simple-names)) 是常量、 字段、 属性，本地变量或参数类型相同的含义`E`作为*type_name* ([Namespace 和类型名称](basic-concepts.md#namespace-and-type-names))，则这两个可能的含义的`E`是允许使用。</span><span class="sxs-lookup"><span data-stu-id="08161-988">In a member access of the form `E.I`, if `E` is a single identifier, and if the meaning of `E` as a *simple_name* ([Simple names](expressions.md#simple-names)) is a constant, field, property, local variable, or parameter with the same type as the meaning of `E` as a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)), then both possible meanings of `E` are permitted.</span></span> <span data-ttu-id="08161-989">两个可能的含义`E.I`永远不会不明确，因为`I`一定必须是类型的成员`E`这两种情况。</span><span class="sxs-lookup"><span data-stu-id="08161-989">The two possible meanings of `E.I` are never ambiguous, since `I` must necessarily be a member of the type `E` in both cases.</span></span> <span data-ttu-id="08161-990">换而言之，此规则只是允许访问静态成员和嵌套的类型`E`本来已发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-990">In other words, the rule simply permits access to the static members and nested types of `E` where a compile-time error would otherwise have occurred.</span></span> <span data-ttu-id="08161-991">例如：</span><span class="sxs-lookup"><span data-stu-id="08161-991">For example:</span></span>
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

#### <a name="grammar-ambiguities"></a><span data-ttu-id="08161-992">语法二义性</span><span class="sxs-lookup"><span data-stu-id="08161-992">Grammar ambiguities</span></span>

<span data-ttu-id="08161-993">对于生产*simple_name* ([简单名称](expressions.md#simple-names)) 和*member_access* ([成员访问](expressions.md#member-access)) 可能导致引发方面的多义性问题表达式的语法。</span><span class="sxs-lookup"><span data-stu-id="08161-993">The productions for *simple_name* ([Simple names](expressions.md#simple-names)) and *member_access* ([Member access](expressions.md#member-access)) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="08161-994">例如，语句：</span><span class="sxs-lookup"><span data-stu-id="08161-994">For example, the statement:</span></span>
```
F(G<A,B>(7));
```
<span data-ttu-id="08161-995">可解释为调用`F`具有两个参数`G < A`和`B > (7)`。</span><span class="sxs-lookup"><span data-stu-id="08161-995">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="08161-996">或者，也可以解释为调用`F`带有一个自变量，这是对泛型方法的调用 `G`使用两个类型参数和一个正则自变量。</span><span class="sxs-lookup"><span data-stu-id="08161-996">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

<span data-ttu-id="08161-997">如果可以为分析 （在上下文中） 的标记序列*simple_name* ([简单名称](expressions.md#simple-names))， *member_access* ([成员访问](expressions.md#member-access))，或*pointer_member_access* ([指针成员访问](unsafe-code.md#pointer-member-access)) 结尾*type_argument_list* ([类型实参](types.md#type-arguments))，则令牌紧跟`>`检查令牌。</span><span class="sxs-lookup"><span data-stu-id="08161-997">If a sequence of tokens can be parsed (in context) as a *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), or *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) ending with a *type_argument_list* ([Type arguments](types.md#type-arguments)), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="08161-998">如果它是一个</span><span class="sxs-lookup"><span data-stu-id="08161-998">If it is one of</span></span>
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
<span data-ttu-id="08161-999">然后*type_argument_list*保留为一部分*simple_name*， *member_access*或者*pointer_member_access*和任何令牌的序列的其他可能分析将被放弃。</span><span class="sxs-lookup"><span data-stu-id="08161-999">then the *type_argument_list* is retained as part of the *simple_name*, *member_access* or *pointer_member_access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="08161-1000">否则为*type_argument_list*不属于被视为*simple_name*， *member_access*或*pointer_member_access*，即使不没有令牌的序列的任何其他可能分析。</span><span class="sxs-lookup"><span data-stu-id="08161-1000">Otherwise, the *type_argument_list* is not considered to be part of the *simple_name*, *member_access* or *pointer_member_access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="08161-1001">请注意，这些规则不应用分析时*type_argument_list*中*namespace_or_type_name* ([Namespace 和类型名称](basic-concepts.md#namespace-and-type-names))。</span><span class="sxs-lookup"><span data-stu-id="08161-1001">Note that these rules are not applied when parsing a *type_argument_list* in a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span> <span data-ttu-id="08161-1002">语句</span><span class="sxs-lookup"><span data-stu-id="08161-1002">The statement</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="08161-1003">将此规则，根据被解释为调用`F`带有一个自变量，这是对泛型方法的调用`G`使用两个类型参数和一个正则自变量。</span><span class="sxs-lookup"><span data-stu-id="08161-1003">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="08161-1004">语句</span><span class="sxs-lookup"><span data-stu-id="08161-1004">The statements</span></span>
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
<span data-ttu-id="08161-1005">将每个被解释为调用`F`具有两个参数。</span><span class="sxs-lookup"><span data-stu-id="08161-1005">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="08161-1006">语句</span><span class="sxs-lookup"><span data-stu-id="08161-1006">The statement</span></span>
```csharp
x = F < A > +y;
```
<span data-ttu-id="08161-1007">将被解释为小于运算符，大于运算符和一元加运算符，该语句必须编写`x = (F < A) > (+y)`，而不是作为*simple_name*与*type_argument_list*后跟二进制加运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-1007">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple_name* with a *type_argument_list* followed by a binary plus operator.</span></span> <span data-ttu-id="08161-1008">在语句中</span><span class="sxs-lookup"><span data-stu-id="08161-1008">In the statement</span></span>
```csharp
x = y is C<T> + z;
```
<span data-ttu-id="08161-1009">令牌`C<T>`解释为*namespace_or_type_name*与*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="08161-1009">the tokens `C<T>` are interpreted as a *namespace_or_type_name* with a *type_argument_list*.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="08161-1010">调用表达式</span><span class="sxs-lookup"><span data-stu-id="08161-1010">Invocation expressions</span></span>

<span data-ttu-id="08161-1011">*Invocation_expression*用于调用的方法。</span><span class="sxs-lookup"><span data-stu-id="08161-1011">An *invocation_expression* is used to invoke a method.</span></span>

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

<span data-ttu-id="08161-1012">*Invocation_expression*动态绑定 ([动态绑定](expressions.md#dynamic-binding)) 如果在至少一个以下保留：</span><span class="sxs-lookup"><span data-stu-id="08161-1012">An *invocation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="08161-1013">*Primary_expression*具有编译时类型`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="08161-1013">The *primary_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="08161-1014">至少一个参数的可选*argument_list*具有编译时类型`dynamic`并且*primary_expression*没有委托类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1014">At least one argument of the optional *argument_list* has compile-time type `dynamic` and the *primary_expression* does not have a delegate type.</span></span>

<span data-ttu-id="08161-1015">在这种情况下，编译器将分类*invocation_expression*类型的值作为`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="08161-1015">In this case the compiler classifies the *invocation_expression* as a value of type `dynamic`.</span></span> <span data-ttu-id="08161-1016">规则可确定的含义*invocation_expression*然后应用在运行时，使用运行时类型，而不其中的编译时类型*primary_expression*和其具有编译时类型的参数`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="08161-1016">The rules below to determine the meaning of the *invocation_expression* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_expression* and arguments which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="08161-1017">如果*primary_expression*没有编译时类型`dynamic`，然后方法调用中所述进行有限的编译时检查[编译时检查的动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="08161-1017">If the *primary_expression* does not have compile-time type `dynamic`, then the method invocation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="08161-1018">*Primary_expression*的*invocation_expression*方法组或的值必须是*delegate_type*。</span><span class="sxs-lookup"><span data-stu-id="08161-1018">The *primary_expression* of an *invocation_expression* must be a method group or a value of a *delegate_type*.</span></span> <span data-ttu-id="08161-1019">如果*primary_expression*是方法组*invocation_expression*是方法调用 ([方法调用](expressions.md#method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="08161-1019">If the *primary_expression* is a method group, the *invocation_expression* is a method invocation ([Method invocations](expressions.md#method-invocations)).</span></span> <span data-ttu-id="08161-1020">如果*primary_expression*的值为*delegate_type*，则*invocation_expression*是委托调用 ([委托调用](expressions.md#delegate-invocations)).</span><span class="sxs-lookup"><span data-stu-id="08161-1020">If the *primary_expression* is a value of a *delegate_type*, the *invocation_expression* is a delegate invocation ([Delegate invocations](expressions.md#delegate-invocations)).</span></span> <span data-ttu-id="08161-1021">如果*primary_expression*是方法组和值都不*delegate_type*，绑定时出错。</span><span class="sxs-lookup"><span data-stu-id="08161-1021">If the *primary_expression* is neither a method group nor a value of a *delegate_type*, a binding-time error occurs.</span></span>

<span data-ttu-id="08161-1022">可选*argument_list* ([自变量列表](expressions.md#argument-lists)) 提供的方法参数的值或变量引用。</span><span class="sxs-lookup"><span data-stu-id="08161-1022">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) provides values or variable references for the parameters of the method.</span></span>

<span data-ttu-id="08161-1023">计算结果*invocation_expression*分类，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-1023">The result of evaluating an *invocation_expression* is classified as follows:</span></span>

*  <span data-ttu-id="08161-1024">如果*invocation_expression*调用方法或委托，可返回`void`，结果是执行任何操作。</span><span class="sxs-lookup"><span data-stu-id="08161-1024">If the *invocation_expression* invokes a method or delegate that returns `void`, the result is nothing.</span></span> <span data-ttu-id="08161-1025">一个表达式，被归类为可执行任何操作的上下文中仅允许*statement_expression* ([表达式语句](statements.md#expression-statements)) 或作为正文*lambda_expression*([匿名函数表达式](expressions.md#anonymous-function-expressions))。</span><span class="sxs-lookup"><span data-stu-id="08161-1025">An expression that is classified as nothing is permitted only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)) or as the body of a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="08161-1026">否则绑定时出错。</span><span class="sxs-lookup"><span data-stu-id="08161-1026">Otherwise a binding-time error occurs.</span></span>
*  <span data-ttu-id="08161-1027">否则，结果为返回的方法或委托的类型的值。</span><span class="sxs-lookup"><span data-stu-id="08161-1027">Otherwise, the result is a value of the type returned by the method or delegate.</span></span>

#### <a name="method-invocations"></a><span data-ttu-id="08161-1028">方法调用</span><span class="sxs-lookup"><span data-stu-id="08161-1028">Method invocations</span></span>

<span data-ttu-id="08161-1029">有关方法调用中， *primary_expression*的*invocation_expression*必须为方法组。</span><span class="sxs-lookup"><span data-stu-id="08161-1029">For a method invocation, the *primary_expression* of the *invocation_expression* must be a method group.</span></span> <span data-ttu-id="08161-1030">方法组标识要调用的一个方法或从中进行选择要调用的特定方法的重载方法的集。</span><span class="sxs-lookup"><span data-stu-id="08161-1030">The method group identifies the one method to invoke or the set of overloaded methods from which to choose a specific method to invoke.</span></span> <span data-ttu-id="08161-1031">在后一种情况下，确定特定的方法来调用基于上下文中的参数的类型提供*argument_list*。</span><span class="sxs-lookup"><span data-stu-id="08161-1031">In the latter case, determination of the specific method to invoke is based on the context provided by the types of the arguments in the *argument_list*.</span></span>

<span data-ttu-id="08161-1032">在窗体的方法调用的处理的绑定时间`M(A)`，其中`M`是方法组 (可能包括*type_argument_list*)，并`A`是一个可选*argument_列表*，包括以下步骤：</span><span class="sxs-lookup"><span data-stu-id="08161-1032">The binding-time processing of a method invocation of the form `M(A)`, where `M` is a method group (possibly including a *type_argument_list*), and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="08161-1033">构造的方法调用的候选方法集。</span><span class="sxs-lookup"><span data-stu-id="08161-1033">The set of candidate methods for the method invocation is constructed.</span></span> <span data-ttu-id="08161-1034">为每个方法`F`与方法组相关联`M`:</span><span class="sxs-lookup"><span data-stu-id="08161-1034">For each method `F` associated with the method group `M`:</span></span>
   *  <span data-ttu-id="08161-1035">如果`F`是非泛型`F`是候选版本时：</span><span class="sxs-lookup"><span data-stu-id="08161-1035">If `F` is non-generic, `F` is a candidate when:</span></span>
      * <span data-ttu-id="08161-1036">`M` 没有类型参数列表，并</span><span class="sxs-lookup"><span data-stu-id="08161-1036">`M` has no type argument list, and</span></span>
      * <span data-ttu-id="08161-1037">`F` 与适用`A`([适用函数成员](expressions.md#applicable-function-member))。</span><span class="sxs-lookup"><span data-stu-id="08161-1037">`F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="08161-1038">如果`F`是泛型方法和`M`没有类型参数列表，`F`是候选版本时：</span><span class="sxs-lookup"><span data-stu-id="08161-1038">If `F` is generic and `M` has no type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="08161-1039">类型推理 ([类型推理](expressions.md#type-inference)) 成功，推断出的调用，类型参数列表和</span><span class="sxs-lookup"><span data-stu-id="08161-1039">Type inference ([Type inference](expressions.md#type-inference)) succeeds, inferring a list of type arguments for the call, and</span></span>
      * <span data-ttu-id="08161-1040">F 中的参数列表的所有构造的类型后的推断的类型参数将替换为相应的方法类型参数，满足其约束 ([满足约束](types.md#satisfying-constraints))，以及的参数列表`F`与适用`A`([适用函数成员](expressions.md#applicable-function-member))。</span><span class="sxs-lookup"><span data-stu-id="08161-1040">Once the inferred type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="08161-1041">如果`F`是泛型方法和`M`包含类型参数列表，`F`是候选版本时：</span><span class="sxs-lookup"><span data-stu-id="08161-1041">If `F` is generic and `M` includes a type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="08161-1042">`F` 具有相同数量的方法类型参数在类型参数列表中，提供和</span><span class="sxs-lookup"><span data-stu-id="08161-1042">`F` has the same number of method type parameters as were supplied in the type argument list, and</span></span>
      * <span data-ttu-id="08161-1043">F 中的参数列表的所有构造的类型后的类型参数将替换为相应的方法类型参数，满足其约束 ([满足约束](types.md#satisfying-constraints))，和参数列表的`F`与适用`A`([适用函数成员](expressions.md#applicable-function-member))。</span><span class="sxs-lookup"><span data-stu-id="08161-1043">Once the type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
*  <span data-ttu-id="08161-1044">候选方法集减少到仅包含从派生程度最高的类型：为每个方法`C.F`在组中，其中`C`是在其中的类型方法`F`的所有方法的基类型中都声明的都声明`C`从集中删除。</span><span class="sxs-lookup"><span data-stu-id="08161-1044">The set of candidate methods is reduced to contain only methods from the most derived types: For each method `C.F` in the set, where `C` is the type in which the method `F` is declared, all methods declared in a base type of `C` are removed from the set.</span></span> <span data-ttu-id="08161-1045">此外，如果`C`而不是类类型`object`，从集中删除接口类型中声明的所有方法。</span><span class="sxs-lookup"><span data-stu-id="08161-1045">Furthermore, if `C` is a class type other than `object`, all methods declared in an interface type are removed from the set.</span></span> <span data-ttu-id="08161-1046">（此后一种规则只能具有影响时该方法组是具有非对象的有效基类和非空有效接口集的类型形参上的成员查找结果。）</span><span class="sxs-lookup"><span data-stu-id="08161-1046">(This latter rule only has affect when the method group was the result of a member lookup on a type parameter having an effective base class other than object and a non-empty effective interface set.)</span></span>
*  <span data-ttu-id="08161-1047">如果生成的候选方法集为空，然后进一步处理沿以下步骤将弃用，并改为尝试处理该调用扩展方法调用的形式 ([扩展方法调用](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="08161-1047">If the resulting set of candidate methods is empty, then further processing along the following steps are abandoned, and instead an attempt is made to process the invocation as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="08161-1048">如果此操作失败，则不适用的方法存在，并将绑定时出现错误。</span><span class="sxs-lookup"><span data-stu-id="08161-1048">If this fails, then no applicable methods exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="08161-1049">候选方法集的最佳方法使用的重载决策规则标识[重载决策](expressions.md#overload-resolution)。</span><span class="sxs-lookup"><span data-stu-id="08161-1049">The best method of the set of candidate methods is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="08161-1050">如果不能确定一个最佳方法，该方法调用是不明确的并绑定时出错。</span><span class="sxs-lookup"><span data-stu-id="08161-1050">If a single best method cannot be identified, the method invocation is ambiguous, and a binding-time error occurs.</span></span> <span data-ttu-id="08161-1051">执行重载决策时，泛型方法的参数替换之后考虑 （提供或推断） 的类型参数的相应的方法类型参数。</span><span class="sxs-lookup"><span data-stu-id="08161-1051">When performing overload resolution, the parameters of a generic method are considered after substituting the type arguments (supplied or inferred) for the corresponding method type parameters.</span></span>
*  <span data-ttu-id="08161-1052">执行最终验证，所选的最佳方法：</span><span class="sxs-lookup"><span data-stu-id="08161-1052">Final validation of the chosen best method is performed:</span></span>
   * <span data-ttu-id="08161-1053">在方法组的上下文中验证方法：如果最佳方法是静态方法，方法组必须已由产生*simple_name*或*member_access*通过一种类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1053">The method is validated in the context of the method group: If the best method is a static method, the method group must have resulted from a *simple_name* or a *member_access* through a type.</span></span> <span data-ttu-id="08161-1054">如果最佳方法是实例方法，方法组必须已由产生*simple_name*即*member_access*通过变量或值，或*base_access*。</span><span class="sxs-lookup"><span data-stu-id="08161-1054">If the best method is an instance method, the method group must have resulted from a *simple_name*, a *member_access* through a variable or value, or a *base_access*.</span></span> <span data-ttu-id="08161-1055">如果这些要求都不为 true，将绑定时出现错误。</span><span class="sxs-lookup"><span data-stu-id="08161-1055">If neither of these requirements is true, a binding-time error occurs.</span></span>
   * <span data-ttu-id="08161-1056">如果最佳方法是泛型方法，根据的约束检查类型实参 （提供或推断出） ([满足约束](types.md#satisfying-constraints)) 声明的泛型方法。</span><span class="sxs-lookup"><span data-stu-id="08161-1056">If the best method is a generic method, the type arguments (supplied or inferred) are checked against the constraints ([Satisfying constraints](types.md#satisfying-constraints)) declared on the generic method.</span></span> <span data-ttu-id="08161-1057">如果任何类型实参不满足类型参数上的对应约束，绑定时出错。</span><span class="sxs-lookup"><span data-stu-id="08161-1057">If any type argument does not satisfy the corresponding constraint(s) on the type parameter, a binding-time error occurs.</span></span>

<span data-ttu-id="08161-1058">一旦选择一种方法并将其绑定时在验证通过上述步骤，根据函数成员调用中所述的规则处理实际的运行时调用[编译时检查的动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="08161-1058">Once a method has been selected and validated at binding-time by the above steps, the actual run-time invocation is processed according to the rules of function member invocation described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="08161-1059">上面所述的解析规则的直观效果是，如下所示：若要查找调用的方法调用的特定方法，启动方法调用所指示的类型，并一直向上继承链，直到找到至少一个适用的、 可访问的、 非重写方法声明。</span><span class="sxs-lookup"><span data-stu-id="08161-1059">The intuitive effect of the resolution rules described above is as follows: To locate the particular method invoked by a method invocation, start with the type indicated by the method invocation and proceed up the inheritance chain until at least one applicable, accessible, non-override method declaration is found.</span></span> <span data-ttu-id="08161-1060">然后执行类型推断和重载决策的一套适用的、 可访问的、 非重写该类型中声明的方法并调用由此选定的方法。</span><span class="sxs-lookup"><span data-stu-id="08161-1060">Then perform type inference and overload resolution on the set of applicable, accessible, non-override methods declared in that type and invoke the method thus selected.</span></span> <span data-ttu-id="08161-1061">如果不找到任何方法，改为尝试处理该调用为扩展方法调用。</span><span class="sxs-lookup"><span data-stu-id="08161-1061">If no method was found, try instead to process the invocation as an extension method invocation.</span></span>

#### <a name="extension-method-invocations"></a><span data-ttu-id="08161-1062">扩展方法调用</span><span class="sxs-lookup"><span data-stu-id="08161-1062">Extension method invocations</span></span>

<span data-ttu-id="08161-1063">在方法调用中 ([装箱实例上的调用](expressions.md#invocations-on-boxed-instances)) 之一的窗体</span><span class="sxs-lookup"><span data-stu-id="08161-1063">In a method invocation ([Invocations on boxed instances](expressions.md#invocations-on-boxed-instances)) of one of the forms</span></span>
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
<span data-ttu-id="08161-1064">如果正常处理的调用不发现任何适用的方法，尝试处理构造作为扩展方法调用。</span><span class="sxs-lookup"><span data-stu-id="08161-1064">if the normal processing of the invocation finds no applicable methods, an attempt is made to process the construct as an extension method invocation.</span></span> <span data-ttu-id="08161-1065">如果*expr*或任一*args*编译时类型`dynamic`，扩展方法将不适用。</span><span class="sxs-lookup"><span data-stu-id="08161-1065">If *expr* or any of the *args* has compile-time type `dynamic`, extension methods will not apply.</span></span>

<span data-ttu-id="08161-1066">目的是找到最*type_name* `C`，以便可以进行相应的静态方法调用：</span><span class="sxs-lookup"><span data-stu-id="08161-1066">The objective is to find the best *type_name* `C`, so that the corresponding static method invocation can take place:</span></span>
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

<span data-ttu-id="08161-1067">扩展方法`Ci.Mj`是***符合条件***如果：</span><span class="sxs-lookup"><span data-stu-id="08161-1067">An extension method `Ci.Mj` is ***eligible*** if:</span></span>

*  <span data-ttu-id="08161-1068">`Ci` 是一个非泛型的非嵌套类</span><span class="sxs-lookup"><span data-stu-id="08161-1068">`Ci` is a non-generic, non-nested class</span></span>
*  <span data-ttu-id="08161-1069">名称`Mj`是*标识符*</span><span class="sxs-lookup"><span data-stu-id="08161-1069">The name of `Mj` is *identifier*</span></span>
*  <span data-ttu-id="08161-1070">`Mj` 可访问，并且适用时应用的自变量作为静态方法，如上所示</span><span class="sxs-lookup"><span data-stu-id="08161-1070">`Mj` is accessible and applicable when applied to the arguments as a static method as shown above</span></span>
*  <span data-ttu-id="08161-1071">存在从的隐式标识、 引用或装箱转换*expr*的第一个参数的类型为`Mj`。</span><span class="sxs-lookup"><span data-stu-id="08161-1071">An implicit identity, reference or boxing conversion exists from *expr* to the type of the first parameter of `Mj`.</span></span>

<span data-ttu-id="08161-1072">搜索`C`继续，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-1072">The search for `C` proceeds as follows:</span></span>

*  <span data-ttu-id="08161-1073">从开始继续执行每个封闭命名空间声明和使用包含的编译单元中，结束的最接近封闭命名空间声明连续尝试进行查找候选组扩展方法：</span><span class="sxs-lookup"><span data-stu-id="08161-1073">Starting with the closest enclosing namespace declaration, continuing with each enclosing namespace declaration, and ending with the containing compilation unit, successive attempts are made to find a candidate set of extension methods:</span></span>
   * <span data-ttu-id="08161-1074">如果给定的命名空间或编译单元直接包含非泛型类型声明`Ci`符合条件的扩展方法与`Mj`，则这些扩展方法的集为候选项集。</span><span class="sxs-lookup"><span data-stu-id="08161-1074">If the given namespace or compilation unit directly contains non-generic type declarations `Ci` with eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
   * <span data-ttu-id="08161-1075">如果类型`Ci`导入*using_static_declarations*并且直接由导入的命名空间中声明*using_namespace_directive*给定命名空间或编译单元中直接 s包含符合条件的扩展方法`Mj`，则这些扩展方法的集为候选项集。</span><span class="sxs-lookup"><span data-stu-id="08161-1075">If types `Ci` imported by *using_static_declarations* and directly declared in namespaces imported by *using_namespace_directive*s in the given namespace or compilation unit directly contain eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
*  <span data-ttu-id="08161-1076">如果任何封闭命名空间声明或编译单元中不找到任何候选集，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-1076">If no candidate set is found in any enclosing namespace declaration or compilation unit, a compile-time error occurs.</span></span>
*  <span data-ttu-id="08161-1077">否则，将重载解析应用于设置中所述的候选项 ([重载决策](expressions.md#overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="08161-1077">Otherwise, overload resolution is applied to the candidate set as described in ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="08161-1078">如果未找到任何单个的最佳方法，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-1078">If no single best method is found, a compile-time error occurs.</span></span>
*  <span data-ttu-id="08161-1079">`C` 是在其中最好的方法声明为扩展方法的类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1079">`C` is the type within which the best method is declared as an extension method.</span></span>

<span data-ttu-id="08161-1080">使用`C`为目标，方法调用然后处理作为静态方法调用 ([进行编译时检查的动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="08161-1080">Using `C` as a target, the method call is then processed as a static method invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="08161-1081">上述规则意味着，实例方法优先扩展方法、 内部命名空间声明中可用的扩展方法，优先于在外部命名空间声明和该扩展中可用的扩展方法直接在命名空间中声明的方法优先于扩展方法导入到该命名空间与 using 命名空间指令。</span><span class="sxs-lookup"><span data-stu-id="08161-1081">The preceding rules mean that instance methods take precedence over extension methods, that extension methods available in inner namespace declarations take precedence over extension methods available in outer namespace declarations, and that extension methods declared directly in a namespace take precedence over extension methods imported into that same namespace with a using namespace directive.</span></span> <span data-ttu-id="08161-1082">例如：</span><span class="sxs-lookup"><span data-stu-id="08161-1082">For example:</span></span>
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

<span data-ttu-id="08161-1083">在示例中，`B`的方法将优先于第一个扩展方法和`C`的方法将优先于这两种扩展方法。</span><span class="sxs-lookup"><span data-stu-id="08161-1083">In the example, `B`'s method takes precedence over the first extension method, and `C`'s method takes precedence over both extension methods.</span></span>

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

<span data-ttu-id="08161-1084">此示例的输出是：</span><span class="sxs-lookup"><span data-stu-id="08161-1084">The output of this example is:</span></span>
```
E.F(1)
D.G(2)
C.H(3)
```
<span data-ttu-id="08161-1085">`D.G` 将优先于`C.G`，并`E.F`优先于同时`D.F`和`C.F`。</span><span class="sxs-lookup"><span data-stu-id="08161-1085">`D.G` takes precedence over `C.G`, and `E.F` takes precedence over both `D.F` and `C.F`.</span></span>

#### <a name="delegate-invocations"></a><span data-ttu-id="08161-1086">委托调用</span><span class="sxs-lookup"><span data-stu-id="08161-1086">Delegate invocations</span></span>

<span data-ttu-id="08161-1087">对于委托调用， *primary_expression*的*invocation_expression*必须是值为*delegate_type*。</span><span class="sxs-lookup"><span data-stu-id="08161-1087">For a delegate invocation, the *primary_expression* of the *invocation_expression* must be a value of a *delegate_type*.</span></span> <span data-ttu-id="08161-1088">此外，考虑*delegate_type*是一个具有相同的参数列表的函数成员*delegate_type*，则*delegate_type*必须为适用 （[适用函数成员](expressions.md#applicable-function-member)) 与*argument_list*的*invocation_expression*。</span><span class="sxs-lookup"><span data-stu-id="08161-1088">Furthermore, considering the *delegate_type* to be a function member with the same parameter list as the *delegate_type*, the *delegate_type* must be applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the *argument_list* of the *invocation_expression*.</span></span>

<span data-ttu-id="08161-1089">委托调用窗体的运行时处理`D(A)`，其中`D`是*primary_expression*的*delegate_type*和`A`是可选*argument_list*，包括以下步骤：</span><span class="sxs-lookup"><span data-stu-id="08161-1089">The run-time processing of a delegate invocation of the form `D(A)`, where `D` is a *primary_expression* of a *delegate_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="08161-1090">`D` 计算。</span><span class="sxs-lookup"><span data-stu-id="08161-1090">`D` is evaluated.</span></span> <span data-ttu-id="08161-1091">如果此计算导致了异常，不执行任何进一步的操作。</span><span class="sxs-lookup"><span data-stu-id="08161-1091">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="08161-1092">值`D`进行检查，以保持有效。</span><span class="sxs-lookup"><span data-stu-id="08161-1092">The value of `D` is checked to be valid.</span></span> <span data-ttu-id="08161-1093">如果的值`D`是`null`、`System.NullReferenceException`引发并执行任何进一步的操作。</span><span class="sxs-lookup"><span data-stu-id="08161-1093">If the value of `D` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="08161-1094">否则为`D`是对委托实例的引用。</span><span class="sxs-lookup"><span data-stu-id="08161-1094">Otherwise, `D` is a reference to a delegate instance.</span></span> <span data-ttu-id="08161-1095">函数成员调用 ([进行编译时检查的动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) 在每个委托的调用列表中的可调用实体上执行。</span><span class="sxs-lookup"><span data-stu-id="08161-1095">Function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) are performed on each of the callable entities in the invocation list of the delegate.</span></span> <span data-ttu-id="08161-1096">对于可调用实体组成的实例和实例方法，调用的实例是可调用实体中包含的实例。</span><span class="sxs-lookup"><span data-stu-id="08161-1096">For callable entities consisting of an instance and instance method, the instance for the invocation is the instance contained in the callable entity.</span></span>

### <a name="element-access"></a><span data-ttu-id="08161-1097">元素访问</span><span class="sxs-lookup"><span data-stu-id="08161-1097">Element access</span></span>

<span data-ttu-id="08161-1098">*Element_access*组成*primary_no_array_creation_expression*后, 跟"`[`"令牌后, 跟*argument_list*后, 跟"`]`"令牌。</span><span class="sxs-lookup"><span data-stu-id="08161-1098">An *element_access* consists of a *primary_no_array_creation_expression*, followed by a "`[`" token, followed by an *argument_list*, followed by a "`]`" token.</span></span> <span data-ttu-id="08161-1099">*Argument_list*包含一个或多个*自变量*s，由逗号分隔。</span><span class="sxs-lookup"><span data-stu-id="08161-1099">The *argument_list* consists of one or more *argument*s, separated by commas.</span></span>

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

<span data-ttu-id="08161-1100">*Argument_list*的*element_access*不允许包含`ref`或`out`参数。</span><span class="sxs-lookup"><span data-stu-id="08161-1100">The *argument_list* of an *element_access* is not allowed to contain `ref` or `out` arguments.</span></span>

<span data-ttu-id="08161-1101">*Element_access*动态绑定 ([动态绑定](expressions.md#dynamic-binding)) 如果在至少一个以下保留：</span><span class="sxs-lookup"><span data-stu-id="08161-1101">An *element_access* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="08161-1102">*Primary_no_array_creation_expression*具有编译时类型`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="08161-1102">The *primary_no_array_creation_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="08161-1103">至少一个表达式的*argument_list*具有编译时类型`dynamic`并且*primary_no_array_creation_expression*不具有数组类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1103">At least one expression of the *argument_list* has compile-time type `dynamic` and the *primary_no_array_creation_expression* does not have an array type.</span></span>

<span data-ttu-id="08161-1104">在这种情况下，编译器将分类*element_access*类型的值作为`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="08161-1104">In this case the compiler classifies the *element_access* as a value of type `dynamic`.</span></span> <span data-ttu-id="08161-1105">规则可确定的含义*element_access*然后应用在运行时，使用运行时类型，而不其中的编译时类型*primary_no_array_creation_expression*并*argument_list*其具有编译时类型的表达式`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="08161-1105">The rules below to determine the meaning of the *element_access* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_no_array_creation_expression* and *argument_list* expressions which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="08161-1106">如果*primary_no_array_creation_expression*没有编译时类型`dynamic`，然后元素访问进行限制的编译时检查，如中所述[编译时检查的动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution)。</span><span class="sxs-lookup"><span data-stu-id="08161-1106">If the *primary_no_array_creation_expression* does not have compile-time type `dynamic`, then the element access undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="08161-1107">如果*primary_no_array_creation_expression*的*element_access*的值为*array_type*，则*element_access*是数组访问 ([数组访问](expressions.md#array-access))。</span><span class="sxs-lookup"><span data-stu-id="08161-1107">If the *primary_no_array_creation_expression* of an *element_access* is a value of an *array_type*, the *element_access* is an array access ([Array access](expressions.md#array-access)).</span></span> <span data-ttu-id="08161-1108">否则为*primary_no_array_creation_expression*必须是变量或值类、 结构或接口类型，在这种情况下具有一个或多个索引器成员*element_access*是索引器访问 ([索引器访问](expressions.md#indexer-access))。</span><span class="sxs-lookup"><span data-stu-id="08161-1108">Otherwise, the *primary_no_array_creation_expression* must be a variable or value of a class, struct, or interface type that has one or more indexer members, in which case the *element_access* is an indexer access ([Indexer access](expressions.md#indexer-access)).</span></span>

#### <a name="array-access"></a><span data-ttu-id="08161-1109">数组访问</span><span class="sxs-lookup"><span data-stu-id="08161-1109">Array access</span></span>

<span data-ttu-id="08161-1110">对于数组访问*primary_no_array_creation_expression*的*element_access*必须是值为*array_type*。</span><span class="sxs-lookup"><span data-stu-id="08161-1110">For an array access, the *primary_no_array_creation_expression* of the *element_access* must be a value of an *array_type*.</span></span> <span data-ttu-id="08161-1111">此外， *argument_list*数组的访问不允许包含命名的参数。表达式的数目*argument_list*必须是相同的秩*array_type*，并且每个表达式必须是类型`int`， `uint`， `long`， `ulong`，或者必须是隐式转换为一个或多个这些类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1111">Furthermore, the *argument_list* of an array access is not allowed to contain named arguments.The number of expressions in the *argument_list* must be the same as the rank of the *array_type*, and each expression must be of type `int`, `uint`, `long`, `ulong`, or must be implicitly convertible to one or more of these types.</span></span>

<span data-ttu-id="08161-1112">评估数组访问的结果为数组，即所选的中的表达式的值的数组元素的元素类型的变量*argument_list*。</span><span class="sxs-lookup"><span data-stu-id="08161-1112">The result of evaluating an array access is a variable of the element type of the array, namely the array element selected by the value(s) of the expression(s) in the *argument_list*.</span></span>

<span data-ttu-id="08161-1113">数组访问窗体的运行时处理`P[A]`，其中`P`是*primary_no_array_creation_expression*的*array_type*和`A`是*argument_list*，包括以下步骤：</span><span class="sxs-lookup"><span data-stu-id="08161-1113">The run-time processing of an array access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of an *array_type* and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="08161-1114">`P` 计算。</span><span class="sxs-lookup"><span data-stu-id="08161-1114">`P` is evaluated.</span></span> <span data-ttu-id="08161-1115">如果此计算导致了异常，不执行任何进一步的操作。</span><span class="sxs-lookup"><span data-stu-id="08161-1115">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="08161-1116">索引表达式*argument_list*按从左到右的顺序计算。</span><span class="sxs-lookup"><span data-stu-id="08161-1116">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="08161-1117">以下每个索引表达式，隐式转换的计算结果 ([隐式转换](conversions.md#implicit-conversions)) 执行以下类型之一： `int`， `uint`， `long`， `ulong`。</span><span class="sxs-lookup"><span data-stu-id="08161-1117">Following evaluation of each index expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="08161-1118">选择在此列表中存在的隐式转换的第一种类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1118">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="08161-1119">例如，如果索引表达式的类型`short`然后隐式转换到`int`执行，因为从隐式转换`short`到`int`以及从`short`到`long`可使用。</span><span class="sxs-lookup"><span data-stu-id="08161-1119">For instance, if the index expression is of type `short` then an implicit conversion to `int` is performed, since implicit conversions from `short` to `int` and from `short` to `long` are possible.</span></span> <span data-ttu-id="08161-1120">索引表达式或后续的隐式转换的计算会导致异常，如果没有更多的索引表达式的计算，并不能进一步执行步骤。</span><span class="sxs-lookup"><span data-stu-id="08161-1120">If evaluation of an index expression or the subsequent implicit conversion causes an exception, then no further index expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="08161-1121">值`P`进行检查，以保持有效。</span><span class="sxs-lookup"><span data-stu-id="08161-1121">The value of `P` is checked to be valid.</span></span> <span data-ttu-id="08161-1122">如果的值`P`是`null`、`System.NullReferenceException`引发并执行任何进一步的操作。</span><span class="sxs-lookup"><span data-stu-id="08161-1122">If the value of `P` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="08161-1123">在每个表达式的值*argument_list*根据每个维度的引用的数组实例的实际边界检查`P`。</span><span class="sxs-lookup"><span data-stu-id="08161-1123">The value of each expression in the *argument_list* is checked against the actual bounds of each dimension of the array instance referenced by `P`.</span></span> <span data-ttu-id="08161-1124">如果一个或多个值超出范围，`System.IndexOutOfRangeException`引发并执行任何进一步的操作。</span><span class="sxs-lookup"><span data-stu-id="08161-1124">If one or more values are out of range, a `System.IndexOutOfRangeException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="08161-1125">计算由索引表达式给定的数组元素的位置，并且此位置将成为数组访问的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-1125">The location of the array element given by the index expression(s) is computed, and this location becomes the result of the array access.</span></span>

#### <a name="indexer-access"></a><span data-ttu-id="08161-1126">索引器访问</span><span class="sxs-lookup"><span data-stu-id="08161-1126">Indexer access</span></span>

<span data-ttu-id="08161-1127">对于索引器访问， *primary_no_array_creation_expression*的*element_access*必须是变量或值类、 结构或接口类型，并且此类型必须实现一个或多个索引器与适用*argument_list*的*element_access*。</span><span class="sxs-lookup"><span data-stu-id="08161-1127">For an indexer access, the *primary_no_array_creation_expression* of the *element_access* must be a variable or value of a class, struct, or interface type, and this type must implement one or more indexers that are applicable with respect to the *argument_list* of the *element_access*.</span></span>

<span data-ttu-id="08161-1128">处理窗体的索引器访问的绑定时间`P[A]`，其中`P`是*primary_no_array_creation_expression*的类、 结构或接口类型`T`，并`A`是*argument_list*，包括以下步骤：</span><span class="sxs-lookup"><span data-stu-id="08161-1128">The binding-time processing of an indexer access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of a class, struct, or interface type `T`, and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="08161-1129">索引器提供的一套`T`构造。</span><span class="sxs-lookup"><span data-stu-id="08161-1129">The set of indexers provided by `T` is constructed.</span></span> <span data-ttu-id="08161-1130">集包括的所有索引器中声明`T`或基类型的`T`不是`override`声明，并可在当前上下文中访问 ([成员访问](basic-concepts.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="08161-1130">The set consists of all indexers declared in `T` or a base type of `T` that are not `override` declarations and are accessible in the current context ([Member access](basic-concepts.md#member-access)).</span></span>
*  <span data-ttu-id="08161-1131">集减少到由其他索引器适用的并且不会隐藏这些索引器。</span><span class="sxs-lookup"><span data-stu-id="08161-1131">The set is reduced to those indexers that are applicable and not hidden by other indexers.</span></span> <span data-ttu-id="08161-1132">以下规则应用于每个索引器`S.I`在组中，其中`S`是在其中的类型索引器`I`声明：</span><span class="sxs-lookup"><span data-stu-id="08161-1132">The following rules are applied to each indexer `S.I` in the set, where `S` is the type in which the indexer `I` is declared:</span></span>
   * <span data-ttu-id="08161-1133">如果`I`不与适用`A`([适用函数成员](expressions.md#applicable-function-member))，然后`I`从集中删除。</span><span class="sxs-lookup"><span data-stu-id="08161-1133">If `I` is not applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then `I` is removed from the set.</span></span>
   * <span data-ttu-id="08161-1134">如果`I`与适用`A`([适用函数成员](expressions.md#applicable-function-member))，所有索引器的基类型中的声明然后`S`从集中删除。</span><span class="sxs-lookup"><span data-stu-id="08161-1134">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then all indexers declared in a base type of `S` are removed from the set.</span></span>
   * <span data-ttu-id="08161-1135">如果`I`与适用`A`([适用函数成员](expressions.md#applicable-function-member)) 和`S`而不是类类型`object`，从集合中移除接口中声明的所有索引器。</span><span class="sxs-lookup"><span data-stu-id="08161-1135">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)) and `S` is a class type other than `object`, all indexers declared in an interface are removed from the set.</span></span>
*  <span data-ttu-id="08161-1136">如果生成候选索引器的集为空，则不适用的索引器存在，并将绑定时出现错误。</span><span class="sxs-lookup"><span data-stu-id="08161-1136">If the resulting set of candidate indexers is empty, then no applicable indexers exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="08161-1137">使用的重载决策规则标识的一套候选索引器的最佳索引器[重载决策](expressions.md#overload-resolution)。</span><span class="sxs-lookup"><span data-stu-id="08161-1137">The best indexer of the set of candidate indexers is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="08161-1138">如果无法识别单个最佳索引器，索引器访问是不明确，并将绑定时出现错误。</span><span class="sxs-lookup"><span data-stu-id="08161-1138">If a single best indexer cannot be identified, the indexer access is ambiguous, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="08161-1139">索引表达式*argument_list*按从左到右的顺序计算。</span><span class="sxs-lookup"><span data-stu-id="08161-1139">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="08161-1140">处理索引器访问的结果是归类为索引器访问的表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-1140">The result of processing the indexer access is an expression classified as an indexer access.</span></span> <span data-ttu-id="08161-1141">索引器访问表达式引用在上述步骤中确定索引器，并具有关联的实例表达式的`P`和关联的参数列表的`A`。</span><span class="sxs-lookup"><span data-stu-id="08161-1141">The indexer access expression references the indexer determined in the step above, and has an associated instance expression of `P` and an associated argument list of `A`.</span></span>

<span data-ttu-id="08161-1142">根据使用它的上下文，索引器访问会导致调用任一*get 访问器*或*set 访问器*的索引器。</span><span class="sxs-lookup"><span data-stu-id="08161-1142">Depending on the context in which it is used, an indexer access causes invocation of either the *get accessor* or the *set accessor* of the indexer.</span></span> <span data-ttu-id="08161-1143">如果索引器访问是作为赋值目标*set 访问器*调用来分配新值 ([简单赋值](expressions.md#simple-assignment))。</span><span class="sxs-lookup"><span data-stu-id="08161-1143">If the indexer access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="08161-1144">在所有其他情况下， *get 访问器*调用来获取当前值 ([表达式的值](expressions.md#values-of-expressions))。</span><span class="sxs-lookup"><span data-stu-id="08161-1144">In all other cases, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="this-access"></a><span data-ttu-id="08161-1145">此访问权限</span><span class="sxs-lookup"><span data-stu-id="08161-1145">This access</span></span>

<span data-ttu-id="08161-1146">一个*this_access*包含保留字`this`。</span><span class="sxs-lookup"><span data-stu-id="08161-1146">A *this_access* consists of the reserved word `this`.</span></span>

```antlr
this_access
    : 'this'
    ;
```

<span data-ttu-id="08161-1147">一个*this_access*仅在允许*块*的实例构造函数、 实例方法或实例访问器。</span><span class="sxs-lookup"><span data-stu-id="08161-1147">A *this_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="08161-1148">它具有以下含义之一：</span><span class="sxs-lookup"><span data-stu-id="08161-1148">It has one of the following meanings:</span></span>

*  <span data-ttu-id="08161-1149">当`this`中使用*primary_expression*内类的实例构造函数，它分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="08161-1149">When `this` is used in a *primary_expression* within an instance constructor of a class, it is classified as a value.</span></span> <span data-ttu-id="08161-1150">值的类型为类型的实例 ([的实例类型](classes.md#the-instance-type)) 的类使用情况发生，并且该值是对所构造的对象的引用。</span><span class="sxs-lookup"><span data-stu-id="08161-1150">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object being constructed.</span></span>
*  <span data-ttu-id="08161-1151">当`this`中使用*primary_expression*内实例方法或类的实例访问器，它分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="08161-1151">When `this` is used in a *primary_expression* within an instance method or instance accessor of a class, it is classified as a value.</span></span> <span data-ttu-id="08161-1152">值的类型为类型的实例 ([的实例类型](classes.md#the-instance-type)) 的类使用情况发生，并且该值是对为其调用方法或访问器的对象的引用。</span><span class="sxs-lookup"><span data-stu-id="08161-1152">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object for which the method or accessor was invoked.</span></span>
*  <span data-ttu-id="08161-1153">当`this`中使用*primary_expression*内结构的实例构造函数，它分类为变量。</span><span class="sxs-lookup"><span data-stu-id="08161-1153">When `this` is used in a *primary_expression* within an instance constructor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="08161-1154">变量的类型是使用实例类型 ([的实例类型](classes.md#the-instance-type)) 的结构在其中使用情况发生，并且该变量表示正在构造的结构。</span><span class="sxs-lookup"><span data-stu-id="08161-1154">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs, and the variable represents the struct being constructed.</span></span> <span data-ttu-id="08161-1155">`this`变量的结构的实例构造函数的行为完全相同`out`结构类型的参数-具体而言，这意味着变量必须明确赋值的实例的每个执行路径中构造函数。</span><span class="sxs-lookup"><span data-stu-id="08161-1155">The `this` variable of an instance constructor of a struct behaves exactly the same as an `out` parameter of the struct type—in particular, this means that the variable must be definitely assigned in every execution path of the instance constructor.</span></span>
*  <span data-ttu-id="08161-1156">当`this`中使用*primary_expression*内实例方法或实例访问器的结构，它分类为变量。</span><span class="sxs-lookup"><span data-stu-id="08161-1156">When `this` is used in a *primary_expression* within an instance method or instance accessor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="08161-1157">变量的类型是使用实例类型 ([的实例类型](classes.md#the-instance-type)) 的出现该表达式的结构。</span><span class="sxs-lookup"><span data-stu-id="08161-1157">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs.</span></span>
   * <span data-ttu-id="08161-1158">如果方法或访问器不是一个迭代器 ([迭代器](classes.md#iterators))，则`this`变量表示为其方法或访问器被调用，并具有完全相同的行为的结构`ref`结构类型的参数。</span><span class="sxs-lookup"><span data-stu-id="08161-1158">If the method or accessor is not an iterator ([Iterators](classes.md#iterators)), the `this` variable represents the struct for which the method or accessor was invoked, and behaves exactly the same as a `ref` parameter of the struct type.</span></span>
   * <span data-ttu-id="08161-1159">如果方法或访问器是一个迭代器，`this`变量表示的结构为其方法或访问器被调用，并具有的行为与值类型参数的结构完全相同副本。</span><span class="sxs-lookup"><span data-stu-id="08161-1159">If the method or accessor is an iterator, the `this` variable represents a copy of the struct for which the method or accessor was invoked, and behaves exactly the same as a value parameter of the struct type.</span></span>

<span data-ttu-id="08161-1160">利用`this`中*primary_expression*上述之外的上下文中是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-1160">Use of `this` in a *primary_expression* in a context other than the ones listed above is a compile-time error.</span></span> <span data-ttu-id="08161-1161">具体而言，不能是指`this`中的静态方法，静态属性访问器，或在*variable_initializer*字段声明。</span><span class="sxs-lookup"><span data-stu-id="08161-1161">In particular, it is not possible to refer to `this` in a static method, a static property accessor, or in a *variable_initializer* of a field declaration.</span></span>

### <a name="base-access"></a><span data-ttu-id="08161-1162">基访问</span><span class="sxs-lookup"><span data-stu-id="08161-1162">Base access</span></span>

<span data-ttu-id="08161-1163">一个*base_access*包含保留字`base`后面有"`.`"标记标识符和一个*argument_list*括在方括号中：</span><span class="sxs-lookup"><span data-stu-id="08161-1163">A *base_access* consists of the reserved word `base` followed by either a "`.`" token and an identifier or an *argument_list* enclosed in square brackets:</span></span>

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

<span data-ttu-id="08161-1164">一个*base_access*用于访问以同样在当前类或结构中命名的成员隐藏基类成员。</span><span class="sxs-lookup"><span data-stu-id="08161-1164">A *base_access* is used to access base class members that are hidden by similarly named members in the current class or struct.</span></span> <span data-ttu-id="08161-1165">一个*base_access*仅在允许*块*的实例构造函数、 实例方法或实例访问器。</span><span class="sxs-lookup"><span data-stu-id="08161-1165">A *base_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="08161-1166">当`base.I`出现在类或结构，`I`必须表示该类或结构的基类的成员。</span><span class="sxs-lookup"><span data-stu-id="08161-1166">When `base.I` occurs in a class or struct, `I` must denote a member of the base class of that class or struct.</span></span> <span data-ttu-id="08161-1167">同样，当`base[E]`出现在类中，适用的索引器必须存在于类的基类。</span><span class="sxs-lookup"><span data-stu-id="08161-1167">Likewise, when `base[E]` occurs in a class, an applicable indexer must exist in the base class.</span></span>

<span data-ttu-id="08161-1168">在绑定时， *base_access*形式的表达式`base.I`和`base[E]`正如它们的编写计算`((B)this).I`并`((B)this)[E]`，其中`B`是类的基类或构造会发生的结构。</span><span class="sxs-lookup"><span data-stu-id="08161-1168">At binding-time, *base_access* expressions of the form `base.I` and `base[E]` are evaluated exactly as if they were written `((B)this).I` and `((B)this)[E]`, where `B` is the base class of the class or struct in which the construct occurs.</span></span> <span data-ttu-id="08161-1169">因此，`base.I`并`base[E]`对应于`this.I`并`this[E]`，除`this`被视为与基的类的实例。</span><span class="sxs-lookup"><span data-stu-id="08161-1169">Thus, `base.I` and `base[E]` correspond to `this.I` and `this[E]`, except `this` is viewed as an instance of the base class.</span></span>

<span data-ttu-id="08161-1170">当*base_access*引用确定该函数在运行时调用的成员的虚函数成员 （方法、 属性或索引器），([编译时检查的动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) 发生更改。</span><span class="sxs-lookup"><span data-stu-id="08161-1170">When a *base_access* references a virtual function member (a method, property, or indexer), the determination of which function member to invoke at run-time ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is changed.</span></span> <span data-ttu-id="08161-1171">通过查找派生程度最高的实现来确定调用的函数成员 ([虚拟方法](classes.md#virtual-methods)) 的函数成员与`B`(而不是相对于的运行时类型`this`，作为是通常在非基本访问权限）。</span><span class="sxs-lookup"><span data-stu-id="08161-1171">The function member that is invoked is determined by finding the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of the function member with respect to `B` (instead of with respect to the run-time type of `this`, as would be usual in a non-base access).</span></span> <span data-ttu-id="08161-1172">因此，在`override`的`virtual`函数成员*base_access*可用于调用的函数成员的继承的实现。</span><span class="sxs-lookup"><span data-stu-id="08161-1172">Thus, within an `override` of a `virtual` function member, a *base_access* can be used to invoke the inherited implementation of the function member.</span></span> <span data-ttu-id="08161-1173">如果引用的函数成员*base_access*是抽象的绑定时出错。</span><span class="sxs-lookup"><span data-stu-id="08161-1173">If the function member referenced by a *base_access* is abstract, a binding-time error occurs.</span></span>

### <a name="postfix-increment-and-decrement-operators"></a><span data-ttu-id="08161-1174">后缀递增和递减运算符</span><span class="sxs-lookup"><span data-stu-id="08161-1174">Postfix increment and decrement operators</span></span>

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

<span data-ttu-id="08161-1175">操作数的后缀递增或递减操作必须是归类为变量、 属性访问或索引器访问的表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-1175">The operand of a postfix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="08161-1176">操作的结果是类型的操作数相同的值。</span><span class="sxs-lookup"><span data-stu-id="08161-1176">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="08161-1177">如果*primary_expression*具有编译时类型`dynamic`运算符动态绑定 ([动态绑定](expressions.md#dynamic-binding))，则*post_increment_expression*或*post_decrement_expression*具有编译时类型`dynamic`并在运行时使用的运行时类型应用以下规则*primary_expression*。</span><span class="sxs-lookup"><span data-stu-id="08161-1177">If the *primary_expression* has the compile-time type `dynamic` then the operator is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), the *post_increment_expression* or *post_decrement_expression* has the compile-time type `dynamic` and the following rules are applied at run-time using the run-time type of the *primary_expression*.</span></span>

<span data-ttu-id="08161-1178">如果操作数的后缀递增或递减操作是一个属性或索引器访问、 属性或索引器必须同时具有`get`和一个`set`访问器。</span><span class="sxs-lookup"><span data-stu-id="08161-1178">If the operand of a postfix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="08161-1179">如果这不是这种情况，绑定时出错。</span><span class="sxs-lookup"><span data-stu-id="08161-1179">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="08161-1180">一元运算符重载决策 ([一元运算符重载决策](expressions.md#unary-operator-overload-resolution)) 应用以选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="08161-1180">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08161-1181">预定义`++`并`--`运算符存在以下类型： `sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char``float`， `double`， `decimal`，和任何枚举类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1181">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="08161-1182">预定义`++`运算符将返回通过将 1 添加到操作数，并在预定义生成的值`--`运算符返回生成的数减去 1 的操作数的值。</span><span class="sxs-lookup"><span data-stu-id="08161-1182">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="08161-1183">在中`checked`情况下，如果此加法或减法的结果是结果类型的范围之外，结果类型是整数类型或枚举类型`System.OverflowException`引发。</span><span class="sxs-lookup"><span data-stu-id="08161-1183">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="08161-1184">运行时处理的后缀递增或递减运算的窗体`x++`或`x--`包括以下步骤：</span><span class="sxs-lookup"><span data-stu-id="08161-1184">The run-time processing of a postfix increment or decrement operation of the form `x++` or `x--` consists of the following steps:</span></span>

*   <span data-ttu-id="08161-1185">如果`x`分类为变量：</span><span class="sxs-lookup"><span data-stu-id="08161-1185">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="08161-1186">`x` 计算为生成变量。</span><span class="sxs-lookup"><span data-stu-id="08161-1186">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="08161-1187">值`x`保存。</span><span class="sxs-lookup"><span data-stu-id="08161-1187">The value of `x` is saved.</span></span>
    * <span data-ttu-id="08161-1188">所选的运算符调用已保存的值为`x`作为其参数。</span><span class="sxs-lookup"><span data-stu-id="08161-1188">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="08161-1189">运算符返回的值存储在给定的评估结果的位置`x`。</span><span class="sxs-lookup"><span data-stu-id="08161-1189">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="08161-1190">保存的值的`x`使其成为操作的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-1190">The saved value of `x` becomes the result of the operation.</span></span>
*   <span data-ttu-id="08161-1191">如果`x`归类为属性或索引器访问：</span><span class="sxs-lookup"><span data-stu-id="08161-1191">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="08161-1192">实例表达式 (如果`x`不是`static`) 和参数列表 (如果`x`的索引器访问) 与关联`x`计算，然后在后续结果`get`和`set`取值函数调用。</span><span class="sxs-lookup"><span data-stu-id="08161-1192">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="08161-1193">`get`访问器的`x`调用并保存返回的值。</span><span class="sxs-lookup"><span data-stu-id="08161-1193">The `get` accessor of `x` is invoked and the returned value is saved.</span></span>
    * <span data-ttu-id="08161-1194">所选的运算符调用已保存的值为`x`作为其参数。</span><span class="sxs-lookup"><span data-stu-id="08161-1194">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="08161-1195">`set`访问器的`x`作为运算符所返回的值调用其`value`参数。</span><span class="sxs-lookup"><span data-stu-id="08161-1195">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="08161-1196">保存的值的`x`使其成为操作的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-1196">The saved value of `x` becomes the result of the operation.</span></span>

<span data-ttu-id="08161-1197">`++`并`--`运算符也支持前缀表示法 ([前缀递增和递减运算符](expressions.md#prefix-increment-and-decrement-operators))。</span><span class="sxs-lookup"><span data-stu-id="08161-1197">The `++` and `--` operators also support prefix notation ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="08161-1198">通常情况下的结果`x++`或`x--`的值`x`前该操作，而的结果`++x`或`--x`的值`x`操作完成后。</span><span class="sxs-lookup"><span data-stu-id="08161-1198">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="08161-1199">在任一情况下，`x`本身完成操作后具有相同的值。</span><span class="sxs-lookup"><span data-stu-id="08161-1199">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="08161-1200">`operator ++`或`operator --`实现可以使用后缀或前缀表示法来调用。</span><span class="sxs-lookup"><span data-stu-id="08161-1200">An `operator ++` or `operator --` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="08161-1201">不能具有这两种表示的运算符的不同实现。</span><span class="sxs-lookup"><span data-stu-id="08161-1201">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="the-new-operator"></a><span data-ttu-id="08161-1202">调用 new 运算符</span><span class="sxs-lookup"><span data-stu-id="08161-1202">The new operator</span></span>

<span data-ttu-id="08161-1203">`new`运算符用于创建类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="08161-1203">The `new` operator is used to create new instances of types.</span></span>

<span data-ttu-id="08161-1204">有三种形式的`new`表达式：</span><span class="sxs-lookup"><span data-stu-id="08161-1204">There are three forms of `new` expressions:</span></span>

*  <span data-ttu-id="08161-1205">对象创建表达式用于创建类类型和值类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="08161-1205">Object creation expressions are used to create new instances of class types and value types.</span></span>
*  <span data-ttu-id="08161-1206">数组创建表达式用于创建数组类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="08161-1206">Array creation expressions are used to create new instances of array types.</span></span>
*  <span data-ttu-id="08161-1207">委托创建表达式用于创建类型的委托的新实例。</span><span class="sxs-lookup"><span data-stu-id="08161-1207">Delegate creation expressions are used to create new instances of delegate types.</span></span>

<span data-ttu-id="08161-1208">`new`运算符表示创建的类型的实例，但不一定意味着动态分配的内存。</span><span class="sxs-lookup"><span data-stu-id="08161-1208">The `new` operator implies creation of an instance of a type, but does not necessarily imply dynamic allocation of memory.</span></span> <span data-ttu-id="08161-1209">具体而言，值类型的实例需要在其中它们驻留，并发生任何动态分配的变量以外没有其他内存时`new`用于创建值类型的实例。</span><span class="sxs-lookup"><span data-stu-id="08161-1209">In particular, instances of value types require no additional memory beyond the variables in which they reside, and no dynamic allocations occur when `new` is used to create instances of value types.</span></span>

#### <a name="object-creation-expressions"></a><span data-ttu-id="08161-1210">对象创建表达式</span><span class="sxs-lookup"><span data-stu-id="08161-1210">Object creation expressions</span></span>

<span data-ttu-id="08161-1211">*Object_creation_expression*用来创建的新实例*class_type*或*value_type*。</span><span class="sxs-lookup"><span data-stu-id="08161-1211">An *object_creation_expression* is used to create a new instance of a *class_type* or a *value_type*.</span></span>

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

<span data-ttu-id="08161-1212">*类型*的*object_creation_expression*必须是*class_type*即*value_type*或*type_parameter*.</span><span class="sxs-lookup"><span data-stu-id="08161-1212">The *type* of an *object_creation_expression* must be a *class_type*, a *value_type* or a *type_parameter*.</span></span> <span data-ttu-id="08161-1213">*类型*不能`abstract` *class_type*。</span><span class="sxs-lookup"><span data-stu-id="08161-1213">The *type* cannot be an `abstract` *class_type*.</span></span>

<span data-ttu-id="08161-1214">可选*argument_list* ([自变量列表](expressions.md#argument-lists))，才允许*类型*是*class_type*或*struct_类型*。</span><span class="sxs-lookup"><span data-stu-id="08161-1214">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) is permitted only if the *type* is a *class_type* or a *struct_type*.</span></span>

<span data-ttu-id="08161-1215">对象创建表达式可以省略构造函数参数列表和封闭括号提供它包括对象初始值设定项或集合初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="08161-1215">An object creation expression can omit the constructor argument list and enclosing parentheses provided it includes an object initializer or collection initializer.</span></span> <span data-ttu-id="08161-1216">省略构造函数参数列表和封闭括号是等效于指定参数列表为空。</span><span class="sxs-lookup"><span data-stu-id="08161-1216">Omitting the constructor argument list and enclosing parentheses is equivalent to specifying an empty argument list.</span></span>

<span data-ttu-id="08161-1217">处理的对象创建表达式，其中包括对象初始值设定项或集合初始值设定项包括的第一次处理的实例构造函数，然后处理成员或元素初始化指定的对象初始值设定项 ([对象初始值设定项](expressions.md#object-initializers)) 或集合初始值设定项 ([集合初始值设定项](expressions.md#collection-initializers))。</span><span class="sxs-lookup"><span data-stu-id="08161-1217">Processing of an object creation expression that includes an object initializer or collection initializer consists of first processing the instance constructor and then processing the member or element initializations specified by the object initializer ([Object initializers](expressions.md#object-initializers)) or collection initializer ([Collection initializers](expressions.md#collection-initializers)).</span></span>

<span data-ttu-id="08161-1218">如果任何可选参数*argument_list*具有编译时类型`dynamic`则*object_creation_expression*动态绑定 ([动态绑定](expressions.md#dynamic-binding))和在运行时使用的这些参数的运行时类型应用以下规则*argument_list*具有编译时类型`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="08161-1218">If any of the arguments in the optional *argument_list* has the compile-time type `dynamic` then the *object_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) and the following rules are applied at run-time using the run-time type of those arguments of the *argument_list* that have the compile time type `dynamic`.</span></span> <span data-ttu-id="08161-1219">但是，对象创建进行有限的编译时检查，如中所述[进行编译时检查的动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution)。</span><span class="sxs-lookup"><span data-stu-id="08161-1219">However, the object creation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="08161-1220">绑定时处理*object_creation_expression*窗体`new T(A)`，其中`T`是*class_type*或者*value_type*和`A`是一个可选*argument_list*，包括以下步骤：</span><span class="sxs-lookup"><span data-stu-id="08161-1220">The binding-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is a *class_type* or a *value_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="08161-1221">如果`T`是*value_type*和`A`不存在：</span><span class="sxs-lookup"><span data-stu-id="08161-1221">If `T` is a *value_type* and `A` is not present:</span></span>
    * <span data-ttu-id="08161-1222">*Object_creation_expression*是默认的构造函数调用。</span><span class="sxs-lookup"><span data-stu-id="08161-1222">The *object_creation_expression* is a default constructor invocation.</span></span> <span data-ttu-id="08161-1223">结果*object_creation_expression*是类型的值`T`，即的默认值为`T`中定义[System.ValueType 类型](types.md#the-systemvaluetype-type)。</span><span class="sxs-lookup"><span data-stu-id="08161-1223">The result of the *object_creation_expression* is a value of type `T`, namely the default value for `T` as defined in [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>
*   <span data-ttu-id="08161-1224">否则为如果`T`是*type_parameter*和`A`不存在：</span><span class="sxs-lookup"><span data-stu-id="08161-1224">Otherwise, if `T` is a *type_parameter* and `A` is not present:</span></span>
    * <span data-ttu-id="08161-1225">如果没有值类型约束或构造函数约束 ([类型参数约束](classes.md#type-parameter-constraints)) 为指定`T`，绑定时出错。</span><span class="sxs-lookup"><span data-stu-id="08161-1225">If no value type constraint or constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) has been specified for `T`, a binding-time error occurs.</span></span>
    * <span data-ttu-id="08161-1226">结果*object_creation_expression*是类型参数已绑定到，运行时类型的值即调用该类型的默认构造函数的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-1226">The result of the *object_creation_expression* is a value of the run-time type that the type parameter has been bound to, namely the result of invoking the default constructor of that type.</span></span> <span data-ttu-id="08161-1227">运行时类型可以是引用类型或值类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1227">The run-time type may be a reference type or a value type.</span></span>
*   <span data-ttu-id="08161-1228">否则为如果`T`是*class_type*或*struct_type*:</span><span class="sxs-lookup"><span data-stu-id="08161-1228">Otherwise, if `T` is a *class_type* or a *struct_type*:</span></span>
    * <span data-ttu-id="08161-1229">如果`T`是`abstract` *class_type*，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-1229">If `T` is an `abstract` *class_type*, a compile-time error occurs.</span></span>
    * <span data-ttu-id="08161-1230">使用的重载决策规则确定要调用的实例构造函数[重载决策](expressions.md#overload-resolution)。</span><span class="sxs-lookup"><span data-stu-id="08161-1230">The instance constructor to invoke is determined using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="08161-1231">候选实例构造函数集包括的所有可访问的实例构造函数中声明`T`这是与适用`A`([适用函数成员](expressions.md#applicable-function-member))。</span><span class="sxs-lookup"><span data-stu-id="08161-1231">The set of candidate instance constructors consists of all accessible instance constructors declared in `T` which are applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="08161-1232">如果候选实例构造函数的集为空，或者如果无法确定一个最佳实例构造函数，绑定时出错。</span><span class="sxs-lookup"><span data-stu-id="08161-1232">If the set of candidate instance constructors is empty, or if a single best instance constructor cannot be identified, a binding-time error occurs.</span></span>
    * <span data-ttu-id="08161-1233">结果*object_creation_expression*是类型的值`T`，即通过调用在上述步骤中确定的实例构造函数生成的值。</span><span class="sxs-lookup"><span data-stu-id="08161-1233">The result of the *object_creation_expression* is a value of type `T`, namely the value produced by invoking the instance constructor determined in the step above.</span></span>
*  <span data-ttu-id="08161-1234">否则为*object_creation_expression*是无效的并将绑定时出现错误。</span><span class="sxs-lookup"><span data-stu-id="08161-1234">Otherwise, the *object_creation_expression* is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="08161-1235">即使*object_creation_expression*是否为动态绑定，编译时类型是否仍`T`。</span><span class="sxs-lookup"><span data-stu-id="08161-1235">Even if the *object_creation_expression* is dynamically bound, the compile-time type is still `T`.</span></span>

<span data-ttu-id="08161-1236">运行时处理*object_creation_expression*窗体`new T(A)`，其中`T`是*class_type*或者*struct_type*和`A`是一个可选*argument_list*，包括以下步骤：</span><span class="sxs-lookup"><span data-stu-id="08161-1236">The run-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is *class_type* or a *struct_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="08161-1237">如果`T`是*class_type*:</span><span class="sxs-lookup"><span data-stu-id="08161-1237">If `T` is a *class_type*:</span></span>
    * <span data-ttu-id="08161-1238">类的新实例`T`分配。</span><span class="sxs-lookup"><span data-stu-id="08161-1238">A new instance of class `T` is allocated.</span></span> <span data-ttu-id="08161-1239">如果没有足够内存可用于分配新实例，`System.OutOfMemoryException`引发并执行任何进一步的操作。</span><span class="sxs-lookup"><span data-stu-id="08161-1239">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="08161-1240">所有字段的新实例都初始化为其默认值 ([默认值](variables.md#default-values))。</span><span class="sxs-lookup"><span data-stu-id="08161-1240">All fields of the new instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
    * <span data-ttu-id="08161-1241">实例构造函数调用的函数成员调用规则根据 ([进行编译时检查的动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="08161-1241">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="08161-1242">对新分配的实例的引用会自动传递到的实例构造函数和该实例可以作为该构造函数中访问从`this`。</span><span class="sxs-lookup"><span data-stu-id="08161-1242">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>
*   <span data-ttu-id="08161-1243">如果`T`是*struct_type*:</span><span class="sxs-lookup"><span data-stu-id="08161-1243">If `T` is a *struct_type*:</span></span>
    * <span data-ttu-id="08161-1244">类型的实例`T`创建通过分配一个临时本地变量。</span><span class="sxs-lookup"><span data-stu-id="08161-1244">An instance of type `T` is created by allocating a temporary local variable.</span></span> <span data-ttu-id="08161-1245">实例构造函数的以来*struct_type*需肯定将值分配给要创建的临时变量未初始化是必需的实例的每个字段。</span><span class="sxs-lookup"><span data-stu-id="08161-1245">Since an instance constructor of a *struct_type* is required to definitely assign a value to each field of the instance being created, no initialization of the temporary variable is necessary.</span></span>
    * <span data-ttu-id="08161-1246">实例构造函数调用的函数成员调用规则根据 ([进行编译时检查的动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="08161-1246">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="08161-1247">对新分配的实例的引用会自动传递到的实例构造函数和该实例可以作为该构造函数中访问从`this`。</span><span class="sxs-lookup"><span data-stu-id="08161-1247">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>

#### <a name="object-initializers"></a><span data-ttu-id="08161-1248">对象初始值设定项</span><span class="sxs-lookup"><span data-stu-id="08161-1248">Object initializers</span></span>

<span data-ttu-id="08161-1249">***对象初始值设定项***指定零个或多个字段、 属性或索引的对象的元素的值。</span><span class="sxs-lookup"><span data-stu-id="08161-1249">An ***object initializer*** specifies values for zero or more fields, properties or indexed elements of an object.</span></span>

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

<span data-ttu-id="08161-1250">对象初始值设定项包含成员初始值设定项，括在一系列`{`和`}`标记并用逗号分隔。</span><span class="sxs-lookup"><span data-stu-id="08161-1250">An object initializer consists of a sequence of member initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="08161-1251">每个*member_initializer*指定初始化的目标。</span><span class="sxs-lookup"><span data-stu-id="08161-1251">Each *member_initializer* designates a target for the initialization.</span></span> <span data-ttu-id="08161-1252">*标识符*必须命名的可访问字段或属性要初始化的对象而*argument_list*括在方括号必须为指定参数的可访问的索引器上要初始化的对象。</span><span class="sxs-lookup"><span data-stu-id="08161-1252">An *identifier* must name an accessible field or property of the object being initialized, whereas an *argument_list* enclosed in square brackets must specify arguments for an accessible indexer on the object being initialized.</span></span> <span data-ttu-id="08161-1253">它是错误的对象初始值设定项包含的相同字段或属性的多个成员初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="08161-1253">It is an error for an object initializer to include more than one member initializer for the same field or property.</span></span>

<span data-ttu-id="08161-1254">每个*initializer_target*后跟一个等号和表达式、 对象初始值设定项或集合初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="08161-1254">Each *initializer_target* is followed by an equals sign and either an expression, an object initializer or a collection initializer.</span></span> <span data-ttu-id="08161-1255">不能为对象初始值设定项来引用它正在初始化新创建的对象中的表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-1255">It is not possible for expressions within the object initializer to refer to the newly created object it is initializing.</span></span>

<span data-ttu-id="08161-1256">指定一个表达式，为分配相同的方式处理等号后的成员初始值设定项 ([简单的赋值](expressions.md#simple-assignment)) 到目标。</span><span class="sxs-lookup"><span data-stu-id="08161-1256">A member initializer that specifies an expression after the equals sign is processed in the same way as an assignment ([Simple assignment](expressions.md#simple-assignment)) to the target.</span></span>

<span data-ttu-id="08161-1257">等号后指定的对象初始值设定项的成员初始值设定项***嵌套的对象初始值设定项***，即，嵌入对象的初始化。</span><span class="sxs-lookup"><span data-stu-id="08161-1257">A member initializer that specifies an object initializer after the equals sign is a ***nested object initializer***, i.e. an initialization of an embedded object.</span></span> <span data-ttu-id="08161-1258">而不是将新值分配到的字段或属性，嵌套的对象初始值设定项中的分配将被视为分配给字段或属性的成员。</span><span class="sxs-lookup"><span data-stu-id="08161-1258">Instead of assigning a new value to the field or property, the assignments in the nested object initializer are treated as assignments to members of the field or property.</span></span> <span data-ttu-id="08161-1259">嵌套的对象初始值设定项不能应用到属性使用值类型，或使用值类型的只读字段。</span><span class="sxs-lookup"><span data-stu-id="08161-1259">Nested object initializers cannot be applied to properties with a value type, or to read-only fields with a value type.</span></span>

<span data-ttu-id="08161-1260">在等号后面指定的集合初始值设定项的成员初始值设定项是嵌入集合的初始化。</span><span class="sxs-lookup"><span data-stu-id="08161-1260">A member initializer that specifies a collection initializer after the equals sign is an initialization of an embedded collection.</span></span> <span data-ttu-id="08161-1261">而不是将一个新集合分配给目标字段、 属性或索引器，初始值设定项中给定元素添加到所引用的目标集合。</span><span class="sxs-lookup"><span data-stu-id="08161-1261">Instead of assigning a new collection to the target field, property or indexer, the elements given in the initializer are added to the collection referenced by the target.</span></span> <span data-ttu-id="08161-1262">目标必须满足的需求中指定的集合类型[集合初始值设定项](expressions.md#collection-initializers)。</span><span class="sxs-lookup"><span data-stu-id="08161-1262">The target must be of a collection type that satisfies the requirements specified in [Collection initializers](expressions.md#collection-initializers).</span></span>

<span data-ttu-id="08161-1263">索引初始值设定项的参数将始终仅一次计算。</span><span class="sxs-lookup"><span data-stu-id="08161-1263">The arguments to an index initializer will always be evaluated exactly once.</span></span> <span data-ttu-id="08161-1264">因此，即使参数最终会永远不会使用 （例如由于空嵌套初始值设定项），它们将为其副作用计算。</span><span class="sxs-lookup"><span data-stu-id="08161-1264">Thus, even if the arguments end up never getting used (e.g. because of an empty nested initializer), they will be evaluated for their side effects.</span></span>

<span data-ttu-id="08161-1265">下面的类表示具有两个坐标的点：</span><span class="sxs-lookup"><span data-stu-id="08161-1265">The following class represents a point with two coordinates:</span></span>
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

<span data-ttu-id="08161-1266">实例`Point`可以创建和初始化，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-1266">An instance of `Point` can be created and initialized as follows:</span></span>
```csharp
Point a = new Point { X = 0, Y = 1 };
```
<span data-ttu-id="08161-1267">具有相同的效果</span><span class="sxs-lookup"><span data-stu-id="08161-1267">which has the same effect as</span></span>
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
<span data-ttu-id="08161-1268">其中`__a`是临时变量，否则为不可见且不可访问。</span><span class="sxs-lookup"><span data-stu-id="08161-1268">where `__a` is an otherwise invisible and inaccessible temporary variable.</span></span> <span data-ttu-id="08161-1269">下面的类表示从两个点创建一个矩形：</span><span class="sxs-lookup"><span data-stu-id="08161-1269">The following class represents a rectangle created from two points:</span></span>
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

<span data-ttu-id="08161-1270">实例`Rectangle`可以创建和初始化，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-1270">An instance of `Rectangle` can be created and initialized as follows:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
<span data-ttu-id="08161-1271">具有相同的效果</span><span class="sxs-lookup"><span data-stu-id="08161-1271">which has the same effect as</span></span>
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
<span data-ttu-id="08161-1272">其中`__r`，`__p1`和`__p2`是临时的变量，否则是不可见且不可访问。</span><span class="sxs-lookup"><span data-stu-id="08161-1272">where `__r`, `__p1` and `__p2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="08161-1273">如果`Rectangle`的构造函数分配两个嵌入`Point`实例</span><span class="sxs-lookup"><span data-stu-id="08161-1273">If `Rectangle`'s constructor allocates the two embedded `Point` instances</span></span>
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
<span data-ttu-id="08161-1274">以下构造可用于初始化嵌入`Point`实例而不是将分配新的实例：</span><span class="sxs-lookup"><span data-stu-id="08161-1274">the following construct can be used to initialize the embedded `Point` instances instead of assigning new instances:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
<span data-ttu-id="08161-1275">具有相同的效果</span><span class="sxs-lookup"><span data-stu-id="08161-1275">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

<span data-ttu-id="08161-1276">假设存在相应的 C，下面的示例：</span><span class="sxs-lookup"><span data-stu-id="08161-1276">Given an appropriate definition of C, the following example:</span></span>
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
<span data-ttu-id="08161-1277">等同于这一系列的分配：</span><span class="sxs-lookup"><span data-stu-id="08161-1277">is equivalent to this series of assignments:</span></span>
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
<span data-ttu-id="08161-1278">其中`__c`等，是不可见且无法访问源代码的生成的变量。</span><span class="sxs-lookup"><span data-stu-id="08161-1278">where `__c`, etc., are generated variables that are invisible and inaccessible to the source code.</span></span> <span data-ttu-id="08161-1279">请注意，为参数`[0,0]`有计算仅执行一次，以及有关参数`[1,2]`只计算一次，即使从不使用它们。</span><span class="sxs-lookup"><span data-stu-id="08161-1279">Note that the arguments for `[0,0]` are evaluated only once, and the arguments for `[1,2]` are evaluated once even though they are never used.</span></span>

#### <a name="collection-initializers"></a><span data-ttu-id="08161-1280">集合初始值设定项</span><span class="sxs-lookup"><span data-stu-id="08161-1280">Collection initializers</span></span>

<span data-ttu-id="08161-1281">集合初始值设定项指定集合中的元素。</span><span class="sxs-lookup"><span data-stu-id="08161-1281">A collection initializer specifies the elements of a collection.</span></span>

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

<span data-ttu-id="08161-1282">集合初始值设定项包含的元素初始值设定项，括在一系列`{`和`}`标记并用逗号分隔。</span><span class="sxs-lookup"><span data-stu-id="08161-1282">A collection initializer consists of a sequence of element initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="08161-1283">每个元素初始值设定项指定要添加到要初始化的集合对象的元素，并包含通过括起来的表达式列表`{`和`}`标记并用逗号分隔。</span><span class="sxs-lookup"><span data-stu-id="08161-1283">Each element initializer specifies an element to be added to the collection object being initialized, and consists of a list of expressions enclosed by `{` and `}` tokens and separated by commas.</span></span>  <span data-ttu-id="08161-1284">单表达式元素初始值设定项可以不使用大括号，编写，但不能是赋值表达式，以避免多义性与成员初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="08161-1284">A single-expression element initializer can be written without braces, but cannot then be an assignment expression, to avoid ambiguity with member initializers.</span></span> <span data-ttu-id="08161-1285">*Non_assignment_expression*中定义生产[表达式](expressions.md#expression)。</span><span class="sxs-lookup"><span data-stu-id="08161-1285">The *non_assignment_expression* production is defined in [Expression](expressions.md#expression).</span></span>

<span data-ttu-id="08161-1286">下面是包含集合初始值设定项的对象创建表达式的示例：</span><span class="sxs-lookup"><span data-stu-id="08161-1286">The following is an example of an object creation expression that includes a collection initializer:</span></span>
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

<span data-ttu-id="08161-1287">集合初始值设定项应用到的集合对象必须实现的类型的`System.Collections.IEnumerable`或者发生了编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-1287">The collection object to which a collection initializer is applied must be of a type that implements `System.Collections.IEnumerable` or a compile-time error occurs.</span></span> <span data-ttu-id="08161-1288">集合初始值设定项为每个指定元素的顺序，调用`Add`方法在目标上的元素初始值设定项表达式列表对象作为自变量列表中，将应用普通成员查找并重载决策的每个调用。</span><span class="sxs-lookup"><span data-stu-id="08161-1288">For each specified element in order, the collection initializer invokes an `Add` method on the target object with the expression list of the element initializer as argument list, applying normal member lookup and overload resolution for each invocation.</span></span> <span data-ttu-id="08161-1289">因此，集合对象必须有一个具有名称的适用实例或扩展方法`Add`对于每个元素初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="08161-1289">Thus, the collection object must have an applicable instance or extension method with the name `Add` for each element initializer.</span></span>

<span data-ttu-id="08161-1290">下面的类表示的联系人的名称和电话号码的列表：</span><span class="sxs-lookup"><span data-stu-id="08161-1290">The following class represents a contact with a name and a list of phone numbers:</span></span>
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

<span data-ttu-id="08161-1291">一个`List<Contact>`可以创建和初始化，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-1291">A `List<Contact>` can be created and initialized as follows:</span></span>
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
<span data-ttu-id="08161-1292">具有相同的效果</span><span class="sxs-lookup"><span data-stu-id="08161-1292">which has the same effect as</span></span>
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
<span data-ttu-id="08161-1293">其中`__clist`，`__c1`和`__c2`是临时的变量，否则是不可见且不可访问。</span><span class="sxs-lookup"><span data-stu-id="08161-1293">where `__clist`, `__c1` and `__c2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="08161-1294">数组创建表达式</span><span class="sxs-lookup"><span data-stu-id="08161-1294">Array creation expressions</span></span>

<span data-ttu-id="08161-1295">*Array_creation_expression*用来创建的新实例*array_type*。</span><span class="sxs-lookup"><span data-stu-id="08161-1295">An *array_creation_expression* is used to create a new instance of an *array_type*.</span></span>

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

<span data-ttu-id="08161-1296">第一个窗体的数组创建表达式分配表达式列表中删除每个单个表达式结果的类型数组的实例。</span><span class="sxs-lookup"><span data-stu-id="08161-1296">An array creation expression of the first form allocates an array instance of the type that results from deleting each of the individual expressions from the expression list.</span></span> <span data-ttu-id="08161-1297">例如，数组创建表达式`new int[10,20]`生成类型的数组实例`int[,]`，和数组创建表达式`new int[10][,]`生成类型的数组`int[][,]`。</span><span class="sxs-lookup"><span data-stu-id="08161-1297">For example, the array creation expression `new int[10,20]` produces an array instance of type `int[,]`, and the array creation expression `new int[10][,]` produces an array of type `int[][,]`.</span></span> <span data-ttu-id="08161-1298">在表达式列表中的每个表达式必须为类型`int`， `uint`， `long`，或`ulong`，或隐式转换为一个或多个这些类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1298">Each expression in the expression list must be of type `int`, `uint`, `long`, or `ulong`, or implicitly convertible to one or more of these types.</span></span> <span data-ttu-id="08161-1299">每个表达式的值确定新分配的数组实例中的相应维度的长度。</span><span class="sxs-lookup"><span data-stu-id="08161-1299">The value of each expression determines the length of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="08161-1300">数组维度的长度必须为非负的因为它是具有导致编译时错误*constant_expression*使用负值表达式列表中。</span><span class="sxs-lookup"><span data-stu-id="08161-1300">Since the length of an array dimension must be nonnegative, it is a compile-time error to have a *constant_expression* with a negative value in the expression list.</span></span>

<span data-ttu-id="08161-1301">不安全的上下文中除外 ([不安全的上下文](unsafe-code.md#unsafe-contexts))，未指定数组的布局。</span><span class="sxs-lookup"><span data-stu-id="08161-1301">Except in an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)), the layout of arrays is unspecified.</span></span>

<span data-ttu-id="08161-1302">如果第一个窗体的数组创建表达式中包含的数组初始值设定项，在表达式列表中的每个表达式必须是常量和指定的表达式列表的秩和维度长度必须匹配数组初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="08161-1302">If an array creation expression of the first form includes an array initializer, each expression in the expression list must be a constant and the rank and dimension lengths specified by the expression list must match those of the array initializer.</span></span>

<span data-ttu-id="08161-1303">在第二个或第三个窗体的数组创建表达式，指定的数组类型或秩说明符的秩必须匹配的数组初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="08161-1303">In an array creation expression of the second or third form, the rank of the specified array type or rank specifier must match that of the array initializer.</span></span> <span data-ttu-id="08161-1304">单独的维度长度推断出的每个相应的嵌套级别的数组初始值设定项中的元素数。</span><span class="sxs-lookup"><span data-stu-id="08161-1304">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the array initializer.</span></span> <span data-ttu-id="08161-1305">因此，表达式</span><span class="sxs-lookup"><span data-stu-id="08161-1305">Thus, the expression</span></span>
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
<span data-ttu-id="08161-1306">完全对应于</span><span class="sxs-lookup"><span data-stu-id="08161-1306">exactly corresponds to</span></span>
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

<span data-ttu-id="08161-1307">第三个窗体的数组创建表达式被称为***隐式类型化数组创建表达式***。</span><span class="sxs-lookup"><span data-stu-id="08161-1307">An array creation expression of the third form is referred to as an ***implicitly typed array creation expression***.</span></span> <span data-ttu-id="08161-1308">它类似于第二个窗体，只是数组的元素类型未显式提供，但确定为最常见类型 ([查找一组表达式的最常见类型](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) 的数组中的表达式集初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="08161-1308">It is similar to the second form, except that the element type of the array is not explicitly given, but determined as the best common type ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) of the set of expressions in the array initializer.</span></span> <span data-ttu-id="08161-1309">对于多维数组，即，其中*rank_specifier*包含至少一个逗号，此组包含所有*表达式*中找到 s 嵌套*array_initializer*s。</span><span class="sxs-lookup"><span data-stu-id="08161-1309">For a multidimensional array, i.e., one where the *rank_specifier* contains at least one comma, this set comprises all *expression*s found in nested *array_initializer*s.</span></span>

<span data-ttu-id="08161-1310">数组初始值设定项中进行了描述进一步[数组初始值设定项](arrays.md#array-initializers)。</span><span class="sxs-lookup"><span data-stu-id="08161-1310">Array initializers are described further in [Array initializers](arrays.md#array-initializers).</span></span>

<span data-ttu-id="08161-1311">数组创建表达式的计算结果被分类为一个值，即对新分配的数组实例的引用。</span><span class="sxs-lookup"><span data-stu-id="08161-1311">The result of evaluating an array creation expression is classified as a value, namely a reference to the newly allocated array instance.</span></span> <span data-ttu-id="08161-1312">数组创建表达式的运行时处理包括以下步骤：</span><span class="sxs-lookup"><span data-stu-id="08161-1312">The run-time processing of an array creation expression consists of the following steps:</span></span>

*  <span data-ttu-id="08161-1313">维度长度表达式的*expression_list*按从左到右的顺序计算。</span><span class="sxs-lookup"><span data-stu-id="08161-1313">The dimension length expressions of the *expression_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="08161-1314">以下每个表达式的隐式转换的计算结果 ([隐式转换](conversions.md#implicit-conversions)) 执行以下类型之一： `int`， `uint`， `long`， `ulong`。</span><span class="sxs-lookup"><span data-stu-id="08161-1314">Following evaluation of each expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="08161-1315">选择在此列表中存在的隐式转换的第一种类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1315">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="08161-1316">如果计算的表达式或后续的隐式转换导致异常，然后计算任何其他表达式，并执行任何进一步的操作。</span><span class="sxs-lookup"><span data-stu-id="08161-1316">If evaluation of an expression or the subsequent implicit conversion causes an exception, then no further expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="08161-1317">维的长度的计算所得的值进行验证，如下所示。</span><span class="sxs-lookup"><span data-stu-id="08161-1317">The computed values for the dimension lengths are validated as follows.</span></span> <span data-ttu-id="08161-1318">如果一个或多个值都小于零，`System.OverflowException`引发并执行任何进一步的操作。</span><span class="sxs-lookup"><span data-stu-id="08161-1318">If one or more of the values are less than zero, a `System.OverflowException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="08161-1319">分配具有给定的维的长度的数组实例。</span><span class="sxs-lookup"><span data-stu-id="08161-1319">An array instance with the given dimension lengths is allocated.</span></span> <span data-ttu-id="08161-1320">如果没有足够内存可用于分配新实例，`System.OutOfMemoryException`引发并执行任何进一步的操作。</span><span class="sxs-lookup"><span data-stu-id="08161-1320">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="08161-1321">新数组实例的所有元素将都初始化为其默认值 ([默认值](variables.md#default-values))。</span><span class="sxs-lookup"><span data-stu-id="08161-1321">All elements of the new array instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
*  <span data-ttu-id="08161-1322">如果数组创建表达式中包含的数组初始值设定项，然后计算数组初始值设定项中的每个表达式的值，并将分配给其对应的数组元素。</span><span class="sxs-lookup"><span data-stu-id="08161-1322">If the array creation expression contains an array initializer, then each expression in the array initializer is evaluated and assigned to its corresponding array element.</span></span> <span data-ttu-id="08161-1323">表达式用数组初始值设定项的顺序执行的计算和分配 — 换句话说，元素将初始化按递增第一次增加最右边的维度的索引顺序。</span><span class="sxs-lookup"><span data-stu-id="08161-1323">The evaluations and assignments are performed in the order the expressions are written in the array initializer—in other words, elements are initialized in increasing index order, with the rightmost dimension increasing first.</span></span> <span data-ttu-id="08161-1324">如果给定的表达式或后续分配给相应的数组元素的计算会导致异常，然后初始化任何其他元素 （和剩余的元素将因此具有其默认值）。</span><span class="sxs-lookup"><span data-stu-id="08161-1324">If evaluation of a given expression or the subsequent assignment to the corresponding array element causes an exception, then no further elements are initialized (and the remaining elements will thus have their default values).</span></span>

<span data-ttu-id="08161-1325">数组创建表达式允许实例化数组的元素的数组类型，但必须手动初始化此类数组的元素。</span><span class="sxs-lookup"><span data-stu-id="08161-1325">An array creation expression permits instantiation of an array with elements of an array type, but the elements of such an array must be manually initialized.</span></span> <span data-ttu-id="08161-1326">例如，语句</span><span class="sxs-lookup"><span data-stu-id="08161-1326">For example, the statement</span></span>
```csharp
int[][] a = new int[100][];
```
<span data-ttu-id="08161-1327">创建具有 100 个元素类型的一维数组`int[]`。</span><span class="sxs-lookup"><span data-stu-id="08161-1327">creates a single-dimensional array with 100 elements of type `int[]`.</span></span> <span data-ttu-id="08161-1328">每个元素的初始值是`null`。</span><span class="sxs-lookup"><span data-stu-id="08161-1328">The initial value of each element is `null`.</span></span> <span data-ttu-id="08161-1329">不能为相同的数组创建表达式以子数组和该语句还实例化</span><span class="sxs-lookup"><span data-stu-id="08161-1329">It is not possible for the same array creation expression to also instantiate the sub-arrays, and the statement</span></span>
```csharp
int[][] a = new int[100][5];        // Error
```
<span data-ttu-id="08161-1330">在编译时错误的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-1330">results in a compile-time error.</span></span> <span data-ttu-id="08161-1331">必须改为手动，如下所示执行的子数组实例化</span><span class="sxs-lookup"><span data-stu-id="08161-1331">Instantiation of the sub-arrays must instead be performed manually, as in</span></span>
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

<span data-ttu-id="08161-1332">当数组的数组具有"矩形"形状时，子数组都具有相同时，是长度的使用多维数组更加高效。</span><span class="sxs-lookup"><span data-stu-id="08161-1332">When an array of arrays has a "rectangular" shape, that is when the sub-arrays are all of the same length, it is more efficient to use a multi-dimensional array.</span></span> <span data-ttu-id="08161-1333">在上面的示例中，实例化的数组的数组创建 101 个对象，一个外部数组和 100 的子数组。</span><span class="sxs-lookup"><span data-stu-id="08161-1333">In the example above, instantiation of the array of arrays creates 101 objects—one outer array and 100 sub-arrays.</span></span> <span data-ttu-id="08161-1334">与此相反，</span><span class="sxs-lookup"><span data-stu-id="08161-1334">In contrast,</span></span>
```csharp
int[,] = new int[100, 5];
```
<span data-ttu-id="08161-1335">只创建单个对象，一个二维数组，并完成单个语句中的分配。</span><span class="sxs-lookup"><span data-stu-id="08161-1335">creates only a single object, a two-dimensional array, and accomplishes the allocation in a single statement.</span></span>

<span data-ttu-id="08161-1336">隐式类型化的数组创建表达式的示例如下：</span><span class="sxs-lookup"><span data-stu-id="08161-1336">The following are examples of implicitly typed array creation expressions:</span></span>
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

<span data-ttu-id="08161-1337">最后一个表达式会导致编译时错误，因为既`int`也不`string`隐式转换为另一个，并那里是不是最常见类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1337">The last expression causes a compile-time error because neither `int` nor `string` is implicitly convertible to the other, and so there is no best common type.</span></span> <span data-ttu-id="08161-1338">显式类型化的数组创建表达式必须使用这种情况下，例如指定的类型为`object[]`。</span><span class="sxs-lookup"><span data-stu-id="08161-1338">An explicitly typed array creation expression must be used in this case, for example specifying the type to be `object[]`.</span></span> <span data-ttu-id="08161-1339">或者，一个元素可以转换为通用的基类型，就会处于推断的元素类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1339">Alternatively, one of the elements can be cast to a common base type, which would then become the inferred element type.</span></span>

<span data-ttu-id="08161-1340">可以与匿名对象初始值设定项结合使用隐式类型化的数组创建表达式 ([匿名对象创建表达式](expressions.md#anonymous-object-creation-expressions)) 以匿名方式创建类型化数据结构。</span><span class="sxs-lookup"><span data-stu-id="08161-1340">Implicitly typed array creation expressions can be combined with anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) to create anonymously typed data structures.</span></span> <span data-ttu-id="08161-1341">例如：</span><span class="sxs-lookup"><span data-stu-id="08161-1341">For example:</span></span>
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

#### <a name="delegate-creation-expressions"></a><span data-ttu-id="08161-1342">委托创建表达式</span><span class="sxs-lookup"><span data-stu-id="08161-1342">Delegate creation expressions</span></span>

<span data-ttu-id="08161-1343">一个*delegate_creation_expression*用来创建的新实例*delegate_type*。</span><span class="sxs-lookup"><span data-stu-id="08161-1343">A *delegate_creation_expression* is used to create a new instance of a *delegate_type*.</span></span>

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

<span data-ttu-id="08161-1344">委托创建表达式的参数必须是方法组、 匿名函数或编译时类型的值`dynamic`或*delegate_type*。</span><span class="sxs-lookup"><span data-stu-id="08161-1344">The argument of a delegate creation expression must be a method group, an anonymous function or a value of either the compile time type `dynamic` or a *delegate_type*.</span></span> <span data-ttu-id="08161-1345">如果参数为方法组，它标识的方法和实例方法，为其创建委托的对象。</span><span class="sxs-lookup"><span data-stu-id="08161-1345">If the argument is a method group, it identifies the method and, for an instance method, the object for which to create a delegate.</span></span> <span data-ttu-id="08161-1346">如果该参数是一个匿名函数，它直接定义的参数和委托目标的方法体。</span><span class="sxs-lookup"><span data-stu-id="08161-1346">If the argument is an anonymous function it directly defines the parameters and method body of the delegate target.</span></span> <span data-ttu-id="08161-1347">如果参数为一个值，它标识要创建副本的一个委托实例。</span><span class="sxs-lookup"><span data-stu-id="08161-1347">If the argument is a value it identifies a delegate instance of which to create a copy.</span></span>

<span data-ttu-id="08161-1348">如果*表达式*具有编译时类型`dynamic`，则*delegate_creation_expression*动态绑定 ([动态绑定](expressions.md#dynamic-binding))，以及下面的规则在运行时使用的运行时类型应用*表达式*。</span><span class="sxs-lookup"><span data-stu-id="08161-1348">If the *expression* has the compile-time type `dynamic`, the *delegate_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), and the rules below are applied at run-time using the run-time type of the *expression*.</span></span> <span data-ttu-id="08161-1349">否则，在编译时应用规则。</span><span class="sxs-lookup"><span data-stu-id="08161-1349">Otherwise the rules are applied at compile-time.</span></span>

<span data-ttu-id="08161-1350">绑定时处理*delegate_creation_expression*窗体`new D(E)`，其中`D`是*delegate_type*并`E`是*表达式*，包括以下步骤：</span><span class="sxs-lookup"><span data-stu-id="08161-1350">The binding-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*  <span data-ttu-id="08161-1351">如果`E`是一个方法组，方法组转换为相同的方式处理委托创建表达式 ([方法组转换](conversions.md#method-group-conversions)) 从`E`到`D`。</span><span class="sxs-lookup"><span data-stu-id="08161-1351">If `E` is a method group, the delegate creation expression is processed in the same way as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="08161-1352">如果`E`是一个匿名函数的匿名函数转换为相同的方式处理委托创建表达式 ([匿名函数转换](conversions.md#anonymous-function-conversions)) 从`E`到`D`。</span><span class="sxs-lookup"><span data-stu-id="08161-1352">If `E` is an anonymous function, the delegate creation expression is processed in the same way as an anonymous function conversion ([Anonymous function conversions](conversions.md#anonymous-function-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="08161-1353">如果`E`是一个值，`E`必须是兼容的 ([委托声明](delegates.md#delegate-declarations)) 与`D`，并且结果是对新创建的委托类型的引用`D`，是指同一调用列为`E`。</span><span class="sxs-lookup"><span data-stu-id="08161-1353">If `E` is a value, `E` must be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`, and the result is a reference to a newly created delegate of type `D` that refers to the same invocation list as `E`.</span></span> <span data-ttu-id="08161-1354">如果`E`与不兼容`D`，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-1354">If `E` is not compatible with `D`, a compile-time error occurs.</span></span>

<span data-ttu-id="08161-1355">运行时处理*delegate_creation_expression*窗体`new D(E)`，其中`D`是*delegate_type*并`E`是*表达式*，包括以下步骤：</span><span class="sxs-lookup"><span data-stu-id="08161-1355">The run-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*   <span data-ttu-id="08161-1356">如果`E`是一个方法组，委托创建表达式计算为方法组转换 ([方法组转换](conversions.md#method-group-conversions)) 从`E`到`D`。</span><span class="sxs-lookup"><span data-stu-id="08161-1356">If `E` is a method group, the delegate creation expression is evaluated as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*   <span data-ttu-id="08161-1357">如果`E`是一个匿名函数委托创建计算作为一种从匿名函数转换`E`到`D`([匿名函数转换](conversions.md#anonymous-function-conversions))。</span><span class="sxs-lookup"><span data-stu-id="08161-1357">If `E` is an anonymous function, the delegate creation is evaluated as an anonymous function conversion from `E` to `D` ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>
*   <span data-ttu-id="08161-1358">如果`E`的值为*delegate_type*:</span><span class="sxs-lookup"><span data-stu-id="08161-1358">If `E` is a value of a *delegate_type*:</span></span>
    * <span data-ttu-id="08161-1359">`E` 计算。</span><span class="sxs-lookup"><span data-stu-id="08161-1359">`E` is evaluated.</span></span> <span data-ttu-id="08161-1360">如果此计算导致了异常，不执行任何进一步的操作。</span><span class="sxs-lookup"><span data-stu-id="08161-1360">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="08161-1361">如果的值`E`是`null`、`System.NullReferenceException`引发并执行任何进一步的操作。</span><span class="sxs-lookup"><span data-stu-id="08161-1361">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="08161-1362">委托类型的新实例`D`分配。</span><span class="sxs-lookup"><span data-stu-id="08161-1362">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="08161-1363">如果没有足够内存可用于分配新实例，`System.OutOfMemoryException`引发并执行任何进一步的操作。</span><span class="sxs-lookup"><span data-stu-id="08161-1363">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="08161-1364">使用给定的委托实例相同的调用列表初始化新的委托实例`E`。</span><span class="sxs-lookup"><span data-stu-id="08161-1364">The new delegate instance is initialized with the same invocation list as the delegate instance given by `E`.</span></span>

<span data-ttu-id="08161-1365">委托实例化和委托的整个生存期内保持不变时确定委托的调用列表。</span><span class="sxs-lookup"><span data-stu-id="08161-1365">The invocation list of a delegate is determined when the delegate is instantiated and then remains constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="08161-1366">换而言之，不能创建它后更改目标可调用实体的委托。</span><span class="sxs-lookup"><span data-stu-id="08161-1366">In other words, it is not possible to change the target callable entities of a delegate once it has been created.</span></span> <span data-ttu-id="08161-1367">当两个委托组合或一个删除从另一个 ([委托声明](delegates.md#delegate-declarations))，新的委托; 没有现有委托具有更改其内容。</span><span class="sxs-lookup"><span data-stu-id="08161-1367">When two delegates are combined or one is removed from another ([Delegate declarations](delegates.md#delegate-declarations)), a new delegate results; no existing delegate has its contents changed.</span></span>

<span data-ttu-id="08161-1368">不能创建引用属性、 索引、 用户定义的运算符、 实例构造函数、 析构函数或静态构造函数的委托。</span><span class="sxs-lookup"><span data-stu-id="08161-1368">It is not possible to create a delegate that refers to a property, indexer, user-defined operator, instance constructor, destructor, or static constructor.</span></span>

<span data-ttu-id="08161-1369">上文所述，当创建委托时从方法组，形式参数列表和委托的返回类型确定哪些要选择的重载方法。</span><span class="sxs-lookup"><span data-stu-id="08161-1369">As described above, when a delegate is created from a method group, the formal parameter list and return type of the delegate determine which of the overloaded methods to select.</span></span> <span data-ttu-id="08161-1370">在示例</span><span class="sxs-lookup"><span data-stu-id="08161-1370">In the example</span></span>
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
<span data-ttu-id="08161-1371">`A.f`字段会初始化与指的是第二个委托`Square`方法因为该方法完全匹配的形参列表和返回类型`DoubleFunc`。</span><span class="sxs-lookup"><span data-stu-id="08161-1371">the `A.f` field is initialized with a delegate that refers to the second `Square` method because that method exactly matches the formal parameter list and return type of `DoubleFunc`.</span></span> <span data-ttu-id="08161-1372">具有第二个`Square`方法尚未存在，会出现编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-1372">Had the second `Square` method not been present, a compile-time error would have occurred.</span></span>

#### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="08161-1373">匿名对象创建表达式</span><span class="sxs-lookup"><span data-stu-id="08161-1373">Anonymous object creation expressions</span></span>

<span data-ttu-id="08161-1374">*Anonymous_object_creation_expression*用于创建匿名类型的对象。</span><span class="sxs-lookup"><span data-stu-id="08161-1374">An *anonymous_object_creation_expression* is used to create an object of an anonymous type.</span></span>

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

<span data-ttu-id="08161-1375">匿名对象初始值设定项声明匿名类型，并返回该类型的实例。</span><span class="sxs-lookup"><span data-stu-id="08161-1375">An anonymous object initializer declares an anonymous type and returns an instance of that type.</span></span> <span data-ttu-id="08161-1376">匿名类型是直接继承自的无名称的类类型`object`。</span><span class="sxs-lookup"><span data-stu-id="08161-1376">An anonymous type is a nameless class type that inherits directly from `object`.</span></span> <span data-ttu-id="08161-1377">匿名类型的成员是一系列从用于创建类型的实例的匿名对象初始值设定项推断出的只读属性。</span><span class="sxs-lookup"><span data-stu-id="08161-1377">The members of an anonymous type are a sequence of read-only properties inferred from the anonymous object initializer used to create an instance of the type.</span></span> <span data-ttu-id="08161-1378">具体而言，匿名对象初始值设定项的窗体</span><span class="sxs-lookup"><span data-stu-id="08161-1378">Specifically, an anonymous object initializer of the form</span></span>
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
<span data-ttu-id="08161-1379">声明匿名类型的窗体</span><span class="sxs-lookup"><span data-stu-id="08161-1379">declares an anonymous type of the form</span></span>
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
<span data-ttu-id="08161-1380">其中每个`Tx`是一种相应表达式`ex`。</span><span class="sxs-lookup"><span data-stu-id="08161-1380">where each `Tx` is the type of the corresponding expression `ex`.</span></span> <span data-ttu-id="08161-1381">中使用的表达式*member_declarator*必须具有类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1381">The expression used in a *member_declarator* must have a type.</span></span> <span data-ttu-id="08161-1382">因此，它是中的表达式的编译时错误*member_declarator*为 null 或匿名函数。</span><span class="sxs-lookup"><span data-stu-id="08161-1382">Thus, it is a compile-time error for an expression in a *member_declarator* to be null or an anonymous function.</span></span> <span data-ttu-id="08161-1383">它也是不安全类型的表达式的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-1383">It is also a compile-time error for the expression to have an unsafe type.</span></span>

<span data-ttu-id="08161-1384">匿名类型和参数的名称及其`Equals`方法由编译器自动生成，不能引用中程序文本。</span><span class="sxs-lookup"><span data-stu-id="08161-1384">The names of an anonymous type and of the parameter to its `Equals` method are automatically generated by the compiler and cannot be referenced in program text.</span></span>

<span data-ttu-id="08161-1385">在同一程序中两个匿名对象初始值设定项的相同顺序指定相同的名称和编译时类型的属性的序列将生成相同匿名类型的实例。</span><span class="sxs-lookup"><span data-stu-id="08161-1385">Within the same program, two anonymous object initializers that specify a sequence of properties of the same names and compile-time types in the same order will produce instances of the same anonymous type.</span></span>

<span data-ttu-id="08161-1386">在示例</span><span class="sxs-lookup"><span data-stu-id="08161-1386">In the example</span></span>
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
<span data-ttu-id="08161-1387">允许在最后一行的分配进行，因为`p1`和`p2`同一匿名类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1387">the assignment on the last line is permitted because `p1` and `p2` are of the same anonymous type.</span></span>

<span data-ttu-id="08161-1388">`Equals`和`GetHashcode`匿名类型的方法重写方法继承自`object`，和定义的`Equals`和`GetHashcode`属性，以便同一匿名类型的两个实例相等，如果且仅当所有属性都都相等。</span><span class="sxs-lookup"><span data-stu-id="08161-1388">The `Equals` and `GetHashcode` methods on anonymous types override the methods inherited from `object`, and are defined in terms of the `Equals` and `GetHashcode` of the properties, so that two instances of the same anonymous type are equal if and only if all their properties are equal.</span></span>

<span data-ttu-id="08161-1389">成员声明符可以缩写为简单名称 ([类型推理](expressions.md#type-inference))，成员访问 ([进行编译时检查的动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution))，基访问 ([基访问](expressions.md#base-access))或 null 条件成员访问 ([Null 条件表达式作为投影初始值设定项](expressions.md#null-conditional-expressions-as-projection-initializers))。</span><span class="sxs-lookup"><span data-stu-id="08161-1389">A member declarator can be abbreviated to a simple name ([Type inference](expressions.md#type-inference)), a member access ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), a base access ([Base access](expressions.md#base-access)) or a null-conditional member access ([Null-conditional expressions as projection initializers](expressions.md#null-conditional-expressions-as-projection-initializers)).</span></span> <span data-ttu-id="08161-1390">这称为***投影初始值设定项***和是分配到具有相同名称的属性和声明的简写。</span><span class="sxs-lookup"><span data-stu-id="08161-1390">This is called a ***projection initializer*** and is shorthand for a declaration of and assignment to a property with the same name.</span></span> <span data-ttu-id="08161-1391">具体而言，成员声明符的窗体</span><span class="sxs-lookup"><span data-stu-id="08161-1391">Specifically, member declarators of the forms</span></span>
```csharp
identifier
expr.identifier
```
<span data-ttu-id="08161-1392">完全等效于以下内容，分别是：</span><span class="sxs-lookup"><span data-stu-id="08161-1392">are precisely equivalent to the following, respectively:</span></span>
```csharp
identifier = identifier
identifier = expr.identifier
```

<span data-ttu-id="08161-1393">因此，在投影初始值设定项*标识符*选择值和字段或属性赋值。</span><span class="sxs-lookup"><span data-stu-id="08161-1393">Thus, in a projection initializer the *identifier* selects both the value and the field or property to which the value is assigned.</span></span> <span data-ttu-id="08161-1394">直观地说，投影初始值设定项项目而不仅仅是一个值，但还的值的名称。</span><span class="sxs-lookup"><span data-stu-id="08161-1394">Intuitively, a projection initializer projects not just a value, but also the name of the value.</span></span>

### <a name="the-typeof-operator"></a><span data-ttu-id="08161-1395">Typeof 运算符</span><span class="sxs-lookup"><span data-stu-id="08161-1395">The typeof operator</span></span>

<span data-ttu-id="08161-1396">`typeof`运算符用于获取`System.Type`类型对象。</span><span class="sxs-lookup"><span data-stu-id="08161-1396">The `typeof` operator is used to obtain the `System.Type` object for a type.</span></span>

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

<span data-ttu-id="08161-1397">第一个窗体*typeof_expression*组成`typeof`关键字后跟用圆括号括起来*类型*。</span><span class="sxs-lookup"><span data-stu-id="08161-1397">The first form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *type*.</span></span> <span data-ttu-id="08161-1398">这种形式的表达式的结果是`System.Type`所指示类型的对象。</span><span class="sxs-lookup"><span data-stu-id="08161-1398">The result of an expression of this form is the `System.Type` object for the indicated type.</span></span> <span data-ttu-id="08161-1399">只有一个`System.Type`对于任何给定类型的对象。</span><span class="sxs-lookup"><span data-stu-id="08161-1399">There is only one `System.Type` object for any given type.</span></span> <span data-ttu-id="08161-1400">这意味着，对于类型 `T`，`typeof(T) == typeof(T)`始终为 true。</span><span class="sxs-lookup"><span data-stu-id="08161-1400">This means that for a type `T`, `typeof(T) == typeof(T)` is always true.</span></span> <span data-ttu-id="08161-1401">*类型*不能为`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="08161-1401">The *type* cannot be `dynamic`.</span></span>

<span data-ttu-id="08161-1402">第二个窗体*typeof_expression*组成`typeof`关键字后跟用圆括号括起来*unbound_type_name*。</span><span class="sxs-lookup"><span data-stu-id="08161-1402">The second form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *unbound_type_name*.</span></span> <span data-ttu-id="08161-1403">*Unbound_type_name*非常类似于*type_name* ([Namespace 和类型名称](basic-concepts.md#namespace-and-type-names))，只不过*unbound_type_name*包含*generic_dimension_specifier*s 其中*type_name*包含*type_argument_list*s。</span><span class="sxs-lookup"><span data-stu-id="08161-1403">An *unbound_type_name* is very similar to a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) except that an *unbound_type_name* contains *generic_dimension_specifier*s where a *type_name* contains *type_argument_list*s.</span></span> <span data-ttu-id="08161-1404">时的操作数*typeof_expression*是满足这两者的语法的标记序列*unbound_type_name*并*type_name*，即当它所包含的位置既不*generic_dimension_specifier*也不是*type_argument_list*，令牌的序列被视为可*type_name*。</span><span class="sxs-lookup"><span data-stu-id="08161-1404">When the operand of a *typeof_expression* is a sequence of tokens that satisfies the grammars of both *unbound_type_name* and *type_name*, namely when it contains neither a *generic_dimension_specifier* nor a *type_argument_list*, the sequence of tokens is considered to be a *type_name*.</span></span> <span data-ttu-id="08161-1405">含义*unbound_type_name* ，如下所示确定：</span><span class="sxs-lookup"><span data-stu-id="08161-1405">The meaning of an *unbound_type_name* is determined as follows:</span></span>

*  <span data-ttu-id="08161-1406">将转换为标记的顺序*type_name*通过替换每个*generic_dimension_specifier*与*type_argument_list*具有相同数量的逗号和关键字`object`为每个*type_argument*。</span><span class="sxs-lookup"><span data-stu-id="08161-1406">Convert the sequence of tokens to a *type_name* by replacing each *generic_dimension_specifier* with a *type_argument_list* having the same number of commas and the keyword `object` as each *type_argument*.</span></span>
*  <span data-ttu-id="08161-1407">评估得到*type_name*，但会忽略所有类型参数约束。</span><span class="sxs-lookup"><span data-stu-id="08161-1407">Evaluate the resulting *type_name*, while ignoring all type parameter constraints.</span></span>
*  <span data-ttu-id="08161-1408">*Unbound_type_name*解析为与生成的构造类型关联的未绑定泛型类型 ([绑定和未绑定类型](types.md#bound-and-unbound-types))。</span><span class="sxs-lookup"><span data-stu-id="08161-1408">The *unbound_type_name* resolves to the unbound generic type associated with the resulting constructed type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span>

<span data-ttu-id="08161-1409">结果*typeof_expression*是`System.Type`对象生成未绑定泛型类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1409">The result of the *typeof_expression* is the `System.Type` object for the resulting unbound generic type.</span></span>

<span data-ttu-id="08161-1410">第三种形式*typeof_expression*组成`typeof`关键字后跟用圆括号括起来`void`关键字。</span><span class="sxs-lookup"><span data-stu-id="08161-1410">The third form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized `void` keyword.</span></span> <span data-ttu-id="08161-1411">这种形式的表达式的结果是`System.Type`表示没有一种类型的对象。</span><span class="sxs-lookup"><span data-stu-id="08161-1411">The result of an expression of this form is the `System.Type` object that represents the absence of a type.</span></span> <span data-ttu-id="08161-1412">返回的类型对象`typeof(void)`不同于为任何类型返回的类型对象。</span><span class="sxs-lookup"><span data-stu-id="08161-1412">The type object returned by `typeof(void)` is distinct from the type object returned for any type.</span></span> <span data-ttu-id="08161-1413">这种特殊类型对象在类库中的反射到方法允许在语言中，这些方法希望有一种方法来表示任何方法，包括具有的实例 void 方法的返回类型很有用`System.Type`。</span><span class="sxs-lookup"><span data-stu-id="08161-1413">This special type object is useful in class libraries that allow reflection onto methods in the language, where those methods wish to have a way to represent the return type of any method, including void methods, with an instance of `System.Type`.</span></span>

<span data-ttu-id="08161-1414">`typeof`运算符可以用于类型参数。</span><span class="sxs-lookup"><span data-stu-id="08161-1414">The `typeof` operator can be used on a type parameter.</span></span> <span data-ttu-id="08161-1415">结果是`System.Type`已绑定到类型参数的运行时类型的对象。</span><span class="sxs-lookup"><span data-stu-id="08161-1415">The result is the `System.Type` object for the run-time type that was bound to the type parameter.</span></span> <span data-ttu-id="08161-1416">`typeof`运算符还可构造的类型或未绑定的泛型类型上 ([绑定和未绑定类型](types.md#bound-and-unbound-types))。</span><span class="sxs-lookup"><span data-stu-id="08161-1416">The `typeof` operator can also be used on a constructed type or an unbound generic type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span> <span data-ttu-id="08161-1417">`System.Type`对象未绑定的泛型类型不是与相同`System.Type`实例类型的对象。</span><span class="sxs-lookup"><span data-stu-id="08161-1417">The `System.Type` object for an unbound generic type is not the same as the `System.Type` object of the instance type.</span></span> <span data-ttu-id="08161-1418">实例类型都在运行时封闭式构造的类型因此其`System.Type`对象取决于在使用中，运行时类型自变量而未绑定的泛型类型不具有任何类型实参。</span><span class="sxs-lookup"><span data-stu-id="08161-1418">The instance type is always a closed constructed type at run-time so its `System.Type` object depends on the run-time type arguments in use, while the unbound generic type has no type arguments.</span></span>

<span data-ttu-id="08161-1419">该示例</span><span class="sxs-lookup"><span data-stu-id="08161-1419">The example</span></span>
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
<span data-ttu-id="08161-1420">生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="08161-1420">produces the following output:</span></span>
```
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

<span data-ttu-id="08161-1421">请注意，`int`和`System.Int32`属于同一类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1421">Note that `int` and `System.Int32` are the same type.</span></span>

<span data-ttu-id="08161-1422">另请注意的结果`typeof(X<>)`不依赖于类型参数，但的结果`typeof(X<T>)`does。</span><span class="sxs-lookup"><span data-stu-id="08161-1422">Also note that the result of `typeof(X<>)` does not depend on the type argument but the result of `typeof(X<T>)` does.</span></span>

### <a name="the-checked-and-unchecked-operators"></a><span data-ttu-id="08161-1423">Checked 和 unchecked 运算符</span><span class="sxs-lookup"><span data-stu-id="08161-1423">The checked and unchecked operators</span></span>

<span data-ttu-id="08161-1424">`checked`并`unchecked`运算符用于控制***溢出检查上下文***整型类型算术运算和转换。</span><span class="sxs-lookup"><span data-stu-id="08161-1424">The `checked` and `unchecked` operators are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

<span data-ttu-id="08161-1425">`checked`运算符计算包含在 checked 上下文中，表达式和`unchecked`运算符会求值在未检查的上下文中包含的表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-1425">The `checked` operator evaluates the contained expression in a checked context, and the `unchecked` operator evaluates the contained expression in an unchecked context.</span></span> <span data-ttu-id="08161-1426">一个*checked_expression*或*unchecked_expression*完全对应于*parenthesized_expression* ([带括号的表达式](expressions.md#parenthesized-expressions))，但包含的表达式计算中给定的溢出检查上下文。</span><span class="sxs-lookup"><span data-stu-id="08161-1426">A *checked_expression* or *unchecked_expression* corresponds exactly to a *parenthesized_expression* ([Parenthesized expressions](expressions.md#parenthesized-expressions)), except that the contained expression is evaluated in the given overflow checking context.</span></span>

<span data-ttu-id="08161-1427">此外可以通过控制溢出检查上下文`checked`并`unchecked`语句 ([checked 和 unchecked 语句](statements.md#the-checked-and-unchecked-statements))。</span><span class="sxs-lookup"><span data-stu-id="08161-1427">The overflow checking context can also be controlled through the `checked` and `unchecked` statements ([The checked and unchecked statements](statements.md#the-checked-and-unchecked-statements)).</span></span>

<span data-ttu-id="08161-1428">以下操作受溢出检查上下文来建立`checked`和`unchecked`运算符和语句：</span><span class="sxs-lookup"><span data-stu-id="08161-1428">The following operations are affected by the overflow checking context established by the `checked` and `unchecked` operators and statements:</span></span>

*  <span data-ttu-id="08161-1429">预定义`++`并`--`一元运算符 ([后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators)并[前缀递增和递减运算符](expressions.md#prefix-increment-and-decrement-operators))、 一个操作数是一个整型的类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1429">The predefined `++` and `--` unary operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="08161-1430">预定义`-`一元运算符 ([一元负运算符](expressions.md#unary-minus-operator))、 一个操作数是一种整型类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1430">The predefined `-` unary operator ([Unary minus operator](expressions.md#unary-minus-operator)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="08161-1431">预定义`+`， `-`， `*`，和`/`二元运算符 ([算术运算符](expressions.md#arithmetic-operators))，当这两个操作数都是整数类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1431">The predefined `+`, `-`, `*`, and `/` binary operators ([Arithmetic operators](expressions.md#arithmetic-operators)), when both operands are of integral types.</span></span>
*  <span data-ttu-id="08161-1432">显式数值转换 ([显式数值转换](conversions.md#explicit-numeric-conversions)) 从一种整型类型到另一个整型类型，或从`float`或`double`为整型类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1432">Explicit numeric conversions ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from one integral type to another integral type, or from `float` or `double` to an integral type.</span></span>

<span data-ttu-id="08161-1433">当上述操作的其中一个生成的结果太大而无法表示目标类型，该操作执行的控件的上下文中产生的行为：</span><span class="sxs-lookup"><span data-stu-id="08161-1433">When one of the above operations produce a result that is too large to represent in the destination type, the context in which the operation is performed controls the resulting behavior:</span></span>

*  <span data-ttu-id="08161-1434">在中`checked`情况下，如果该操作是一个常量表达式 ([常量表达式](expressions.md#constant-expressions))，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-1434">In a `checked` context, if the operation is a constant expression ([Constant expressions](expressions.md#constant-expressions)), a compile-time error occurs.</span></span> <span data-ttu-id="08161-1435">否则为如果在运行时执行该操作`System.OverflowException`引发。</span><span class="sxs-lookup"><span data-stu-id="08161-1435">Otherwise, when the operation is performed at run-time, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="08161-1436">在`unchecked`上下文中，结果被截断，放弃不适合目标类型中的任何高序位。</span><span class="sxs-lookup"><span data-stu-id="08161-1436">In an `unchecked` context, the result is truncated by discarding any high-order bits that do not fit in the destination type.</span></span>

<span data-ttu-id="08161-1437">对非常量表达式 （在运行时计算的表达式），不包含任何`checked`或`unchecked`运算符或语句时，默认值溢出检查上下文是`unchecked`除非外部因素 （如编译器有关交换机和执行环境配置） 调用`checked`评估。</span><span class="sxs-lookup"><span data-stu-id="08161-1437">For non-constant expressions (expressions that are evaluated at run-time) that are not enclosed by any `checked` or `unchecked` operators or statements, the default overflow checking context is `unchecked` unless external factors (such as compiler switches and execution environment configuration) call for `checked` evaluation.</span></span>

<span data-ttu-id="08161-1438">常量表达式 （可以在编译时完全计算的表达式），对于默认溢出检查上下文始终是`checked`。</span><span class="sxs-lookup"><span data-stu-id="08161-1438">For constant expressions (expressions that can be fully evaluated at compile-time), the default overflow checking context is always `checked`.</span></span> <span data-ttu-id="08161-1439">除非常量表达式显式置于`unchecked`上下文中，表达式始终的编译时计算期间发生的溢出会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-1439">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur during the compile-time evaluation of the expression always cause compile-time errors.</span></span>

<span data-ttu-id="08161-1440">匿名函数的主体不会影响`checked`或`unchecked`匿名函数中发生的上下文。</span><span class="sxs-lookup"><span data-stu-id="08161-1440">The body of an anonymous function is not affected by `checked` or `unchecked` contexts in which the anonymous function occurs.</span></span>

<span data-ttu-id="08161-1441">在示例</span><span class="sxs-lookup"><span data-stu-id="08161-1441">In the example</span></span>
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
<span data-ttu-id="08161-1442">由于可以在编译时计算的表达式都不报告任何编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-1442">no compile-time errors are reported since neither of the expressions can be evaluated at compile-time.</span></span> <span data-ttu-id="08161-1443">在运行时`F`方法会抛出`System.OverflowException`，和`G`方法返回-727379968 （超出范围结果的低 32 位）。</span><span class="sxs-lookup"><span data-stu-id="08161-1443">At run-time, the `F` method throws a `System.OverflowException`, and the `G` method returns -727379968 (the lower 32 bits of the out-of-range result).</span></span> <span data-ttu-id="08161-1444">行为`H`方法取决于默认溢出检查上下文在编译中，但它是与相同`F`或相同`G`。</span><span class="sxs-lookup"><span data-stu-id="08161-1444">The behavior of the `H` method depends on the default overflow checking context for the compilation, but it is either the same as `F` or the same as `G`.</span></span>

<span data-ttu-id="08161-1445">在示例</span><span class="sxs-lookup"><span data-stu-id="08161-1445">In the example</span></span>
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
<span data-ttu-id="08161-1446">评估中的常量表达式时发生溢出`F`并`H`导致编译时错误报告，因为在计算这些表达式`checked`上下文。</span><span class="sxs-lookup"><span data-stu-id="08161-1446">the overflows that occur when evaluating the constant expressions in `F` and `H` cause compile-time errors to be reported because the expressions are evaluated in a `checked` context.</span></span> <span data-ttu-id="08161-1447">评估中的常量表达式时，也会发生溢出`G`，但由于计算发生在`unchecked`上下文中，不报告溢出。</span><span class="sxs-lookup"><span data-stu-id="08161-1447">An overflow also occurs when evaluating the constant expression in `G`, but since the evaluation takes place in an `unchecked` context, the overflow is not reported.</span></span>

<span data-ttu-id="08161-1448">`checked`并`unchecked`运算符只会影响溢出检查上下文的那些操作的文本中包含"`(`"和"`)`"令牌。</span><span class="sxs-lookup"><span data-stu-id="08161-1448">The `checked` and `unchecked` operators only affect the overflow checking context for those operations that are textually contained within the "`(`" and "`)`" tokens.</span></span> <span data-ttu-id="08161-1449">运算符具有作为包含的表达式求值结果调用的函数成员没有影响。</span><span class="sxs-lookup"><span data-stu-id="08161-1449">The operators have no effect on function members that are invoked as a result of evaluating the contained expression.</span></span> <span data-ttu-id="08161-1450">在示例</span><span class="sxs-lookup"><span data-stu-id="08161-1450">In the example</span></span>
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
<span data-ttu-id="08161-1451">利用`checked`中`F`不会影响的评估`x * y`中`Multiply`，因此`x * y`默认溢出检查上下文中计算。</span><span class="sxs-lookup"><span data-stu-id="08161-1451">the use of `checked` in `F` does not affect the evaluation of `x * y` in `Multiply`, so `x * y` is evaluated in the default overflow checking context.</span></span>

<span data-ttu-id="08161-1452">`unchecked`运算符时，可以方便十六进制表示法中编写的带符号整型的常数。</span><span class="sxs-lookup"><span data-stu-id="08161-1452">The `unchecked` operator is convenient when writing constants of the signed integral types in hexadecimal notation.</span></span> <span data-ttu-id="08161-1453">例如：</span><span class="sxs-lookup"><span data-stu-id="08161-1453">For example:</span></span>
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

<span data-ttu-id="08161-1454">这两个以上的十六进制常量的类型是`uint`。</span><span class="sxs-lookup"><span data-stu-id="08161-1454">Both of the hexadecimal constants above are of type `uint`.</span></span> <span data-ttu-id="08161-1455">因为这些常量包括外部`int`范围，不带`unchecked`运算符、 强制转换到`int`会生成编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-1455">Because the constants are outside the `int` range, without the `unchecked` operator, the casts to `int` would produce compile-time errors.</span></span>

<span data-ttu-id="08161-1456">`checked`和`unchecked`运算符和语句，程序员可以控制某些数值计算的某些方面。</span><span class="sxs-lookup"><span data-stu-id="08161-1456">The `checked` and `unchecked` operators and statements allow programmers to control certain aspects of some numeric calculations.</span></span> <span data-ttu-id="08161-1457">但是，某些数字运算符的行为取决于其操作数的数据类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1457">However, the behavior of some numeric operators depends on their operands' data types.</span></span> <span data-ttu-id="08161-1458">例如，始终乘以两位小数会导致溢出异常，即使在显式`unchecked`构造。</span><span class="sxs-lookup"><span data-stu-id="08161-1458">For example, multiplying two decimals always results in an exception on overflow even within an explicitly `unchecked` construct.</span></span> <span data-ttu-id="08161-1459">同样，乘以两个浮动永远不会结果中溢出异常，即使在显式`checked`构造。</span><span class="sxs-lookup"><span data-stu-id="08161-1459">Similarly, multiplying two floats never results in an exception on overflow even within an explicitly `checked` construct.</span></span> <span data-ttu-id="08161-1460">此外，其他运算符永远不会受检查的模式，无论是默认或显式。</span><span class="sxs-lookup"><span data-stu-id="08161-1460">In addition, other operators are never affected by the mode of checking, whether default or explicit.</span></span>

### <a name="default-value-expressions"></a><span data-ttu-id="08161-1461">默认值表达式</span><span class="sxs-lookup"><span data-stu-id="08161-1461">Default value expressions</span></span>

<span data-ttu-id="08161-1462">默认值表达式用于获取默认值 ([默认值](variables.md#default-values)) 的类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1462">A default value expression is used to obtain the default value ([Default values](variables.md#default-values)) of a type.</span></span> <span data-ttu-id="08161-1463">通常默认值表达式用于类型参数，因为它可能不知道如果类型参数为值类型或引用类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1463">Typically a default value expression is used for type parameters, since it may not be known if the type parameter is a value type or a reference type.</span></span> <span data-ttu-id="08161-1464">(不存在从转换`null`除非类型参数已知为引用类型的类型参数为文字。)</span><span class="sxs-lookup"><span data-stu-id="08161-1464">(No conversion exists from the `null` literal to a type parameter unless the type parameter is known to be a reference type.)</span></span>

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

<span data-ttu-id="08161-1465">如果*类型*中*default_value_expression*计算结果为引用类型运行时，结果是`null`转换为该类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1465">If the *type* in a *default_value_expression* evaluates at run-time to a reference type, the result is `null` converted to that type.</span></span> <span data-ttu-id="08161-1466">如果*类型*中*default_value_expression*计算结果在运行时对值类型，结果是*value_type*的默认值 ([默认构造函数](types.md#default-constructors))。</span><span class="sxs-lookup"><span data-stu-id="08161-1466">If the *type* in a *default_value_expression* evaluates at run-time to a value type, the result is the *value_type*'s default value ([Default constructors](types.md#default-constructors)).</span></span>

<span data-ttu-id="08161-1467">一个*default_value_expression*是常量表达式 ([常量表达式](expressions.md#constant-expressions)) 如果类型是引用类型或类型参数引用类型已知的 ([类型参数约束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="08161-1467">A *default_value_expression* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) if the type is a reference type or a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="08161-1468">此外， *default_value_expression*是常量表达式，如果类型为以下值类型之一： `sbyte`， `byte`， `short`， `ushort`， `int`， `uint`，`long`， `ulong`， `char`， `float`， `double`， `decimal`， `bool`，或任何枚举类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1468">In addition, a *default_value_expression* is a constant expression if the type is one of the following value types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, or any enumeration type.</span></span>


### <a name="nameof-expressions"></a><span data-ttu-id="08161-1469">Nameof 表达式</span><span class="sxs-lookup"><span data-stu-id="08161-1469">Nameof expressions</span></span>

<span data-ttu-id="08161-1470">一个*nameof_expression*用于获得常量字符串作为程序实体的名称。</span><span class="sxs-lookup"><span data-stu-id="08161-1470">A *nameof_expression* is used to obtain the name of a program entity as a constant string.</span></span>

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

<span data-ttu-id="08161-1471">语法上讲， *named_entity*操作数始终是一个表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-1471">Grammatically speaking, the *named_entity* operand is always an expression.</span></span> <span data-ttu-id="08161-1472">因为`nameof`不是保留的关键字，nameof 表达式始终是语法上使用的简单名称的调用不明确`nameof`。</span><span class="sxs-lookup"><span data-stu-id="08161-1472">Because `nameof` is not a reserved keyword, a nameof expression is always syntactically ambiguous with an invocation of the simple name `nameof`.</span></span> <span data-ttu-id="08161-1473">出于兼容性原因，如果名称查找 ([的简单名称](expressions.md#simple-names)) 的名称`nameof`成功，该表达式将被视为*invocation_expression* -无论是否调用处于法律。</span><span class="sxs-lookup"><span data-stu-id="08161-1473">For compatibility reasons, if a name lookup ([Simple names](expressions.md#simple-names)) of the name `nameof` succeeds, the expression is treated as an *invocation_expression* -- regardless of whether the invocation is legal.</span></span> <span data-ttu-id="08161-1474">否则它是*nameof_expression*。</span><span class="sxs-lookup"><span data-stu-id="08161-1474">Otherwise it is a *nameof_expression*.</span></span>

<span data-ttu-id="08161-1475">含义*named_entity*的*nameof_expression*是表达式，则为它的含义，即以作为*simple_name*、 *base_access*或*member_access*。</span><span class="sxs-lookup"><span data-stu-id="08161-1475">The meaning of the *named_entity* of a *nameof_expression* is the meaning of it as an expression; that is, either as a *simple_name*, a *base_access* or a *member_access*.</span></span> <span data-ttu-id="08161-1476">但是，在其中查找中所述[的简单名称](expressions.md#simple-names)并[成员访问](expressions.md#member-access)会导致出现错误，因为在静态上下文中，发现了实例成员*nameof_expression*会生成任何此类错误。</span><span class="sxs-lookup"><span data-stu-id="08161-1476">However, where the lookup described in [Simple names](expressions.md#simple-names) and [Member access](expressions.md#member-access) results in an error because an instance member was found in a static context, a *nameof_expression* produces no such error.</span></span>

<span data-ttu-id="08161-1477">它是编译时错误*named_entity*指定一个方法组能够*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="08161-1477">It is a compile-time error for a *named_entity* designating a method group to have a *type_argument_list*.</span></span> <span data-ttu-id="08161-1478">它是用于编译时错误*named_entity_target*类型`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="08161-1478">It is a compile time error for a *named_entity_target* to have the type `dynamic`.</span></span>

<span data-ttu-id="08161-1479">一个*nameof_expression*是类型的常量表达式`string`，因此在运行时已失效。</span><span class="sxs-lookup"><span data-stu-id="08161-1479">A *nameof_expression* is a constant expression of type `string`, and has no effect at runtime.</span></span> <span data-ttu-id="08161-1480">具体而言，其*named_entity*则不会评估，并忽略有关进行明确赋值分析 ([简单表达式的一般规则](variables.md#general-rules-for-simple-expressions))。</span><span class="sxs-lookup"><span data-stu-id="08161-1480">Specifically, its *named_entity* is not evaluated, and is ignored for the purposes of definite assignment analysis ([General rules for simple expressions](variables.md#general-rules-for-simple-expressions)).</span></span> <span data-ttu-id="08161-1481">其值是最后一个标识符*named_entity*之前的可选最终*type_argument_list*转换如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-1481">Its value is the last identifier of the *named_entity* before the optional final *type_argument_list*, transformed in the following way:</span></span>

* <span data-ttu-id="08161-1482">前缀"`@`"，如果使用，会删除。</span><span class="sxs-lookup"><span data-stu-id="08161-1482">The prefix "`@`", if used, is removed.</span></span>
* <span data-ttu-id="08161-1483">每个*unicode_escape_sequence*转换为其对应的 Unicode 字符。</span><span class="sxs-lookup"><span data-stu-id="08161-1483">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
* <span data-ttu-id="08161-1484">任何*formatting_characters*删除。</span><span class="sxs-lookup"><span data-stu-id="08161-1484">Any *formatting_characters* are removed.</span></span>

<span data-ttu-id="08161-1485">这些是相同的转换应用于[标识符](lexical-structure.md#identifiers)时测试标识符是否相等。</span><span class="sxs-lookup"><span data-stu-id="08161-1485">These are the same transformations applied in [Identifiers](lexical-structure.md#identifiers) when testing equality between identifiers.</span></span>

<span data-ttu-id="08161-1486">TODO： 示例</span><span class="sxs-lookup"><span data-stu-id="08161-1486">TODO: examples</span></span>

### <a name="anonymous-method-expressions"></a><span data-ttu-id="08161-1487">匿名方法表达式</span><span class="sxs-lookup"><span data-stu-id="08161-1487">Anonymous method expressions</span></span>

<span data-ttu-id="08161-1488">*Anonymous_method_expression*是定义一个匿名函数的两种方式之一。</span><span class="sxs-lookup"><span data-stu-id="08161-1488">An *anonymous_method_expression* is one of two ways of defining an anonymous function.</span></span> <span data-ttu-id="08161-1489">在中进一步说明[匿名函数表达式](expressions.md#anonymous-function-expressions)。</span><span class="sxs-lookup"><span data-stu-id="08161-1489">These are further described in [Anonymous function expressions](expressions.md#anonymous-function-expressions).</span></span>

## <a name="unary-operators"></a><span data-ttu-id="08161-1490">一元运算符</span><span class="sxs-lookup"><span data-stu-id="08161-1490">Unary operators</span></span>

<span data-ttu-id="08161-1491">`?`， `+`， `-`， `!`， `~`， `++`，`--`强制转换，和`await`运算符被称作一元运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-1491">The `?`, `+`, `-`, `!`, `~`, `++`, `--`, cast, and `await` operators are called the unary operators.</span></span>

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

<span data-ttu-id="08161-1492">如果操作数*unary_expression*具有编译时类型`dynamic`，动态绑定 ([动态绑定](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="08161-1492">If the operand of a *unary_expression* has the compile-time type `dynamic`, it is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="08161-1493">在这种情况下的编译时类型*unary_expression*是`dynamic`，如下所述的分辨率将发生在运行时使用的运行时类型操作数的和。</span><span class="sxs-lookup"><span data-stu-id="08161-1493">In this case the compile-time type of the *unary_expression* is `dynamic`, and the resolution described below will take place at run-time using the run-time type of the operand.</span></span>

### <a name="null-conditional-operator"></a><span data-ttu-id="08161-1494">Null 条件运算符</span><span class="sxs-lookup"><span data-stu-id="08161-1494">Null-conditional operator</span></span>

<span data-ttu-id="08161-1495">仅当该操作数为非 null，null 条件运算符适用于其操作数的操作的列表。</span><span class="sxs-lookup"><span data-stu-id="08161-1495">The null-conditional operator applies a list of operations to its operand only if that operand is non-null.</span></span> <span data-ttu-id="08161-1496">否则应用运算符的结果是`null`。</span><span class="sxs-lookup"><span data-stu-id="08161-1496">Otherwise the result of applying the operator is `null`.</span></span>

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

<span data-ttu-id="08161-1497">成员访问和元素访问操作 （它本身可能是 null 条件），以及调用可以包含的操作的列表。</span><span class="sxs-lookup"><span data-stu-id="08161-1497">The list of operations can include member access and element access operations (which may themselves be null-conditional), as well as invocation.</span></span>

<span data-ttu-id="08161-1498">例如，表达式`a.b?[0]?.c()`是*null_conditional_expression*与*primary_expression* `a.b`和*null_conditional_operations* `?[0]` （null 条件元素访问）， `?.c` （null 条件成员访问） 和`()`（调用）。</span><span class="sxs-lookup"><span data-stu-id="08161-1498">For example, the expression `a.b?[0]?.c()` is a *null_conditional_expression* with a *primary_expression* `a.b` and *null_conditional_operations* `?[0]` (null-conditional element access), `?.c` (null-conditional member access) and `()` (invocation).</span></span>

<span data-ttu-id="08161-1499">有关*null_conditional_expression* `E`与*primary_expression* `P`，可让`E0`是通过文本上删除了前导获取表达式`?`从每个*null_conditional_operations*的`E`，有一个。</span><span class="sxs-lookup"><span data-stu-id="08161-1499">For a *null_conditional_expression* `E` with a *primary_expression* `P`, let `E0` be the expression obtained by textually removing the leading `?` from each of the *null_conditional_operations* of `E` that have one.</span></span> <span data-ttu-id="08161-1500">从概念上讲，`E0`无 null 检查所表示的情况下计算的表达式`?`找到了 s `null`。</span><span class="sxs-lookup"><span data-stu-id="08161-1500">Conceptually, `E0` is the expression that will be evaluated if none of the null checks represented by the `?`s do find a `null`.</span></span>

<span data-ttu-id="08161-1501">此外，让`E1`是获取通过文本上删除了前导表达式`?`只需从的第一个*null_conditional_operations*中`E`。</span><span class="sxs-lookup"><span data-stu-id="08161-1501">Also, let `E1` be the expression obtained by textually removing the leading `?` from just the first of the *null_conditional_operations* in `E`.</span></span> <span data-ttu-id="08161-1502">这可能会导致*主表达式*(如果有一个`?`) 或另一个*null_conditional_expression*。</span><span class="sxs-lookup"><span data-stu-id="08161-1502">This may lead to a *primary-expression* (if there was just one `?`) or to another *null_conditional_expression*.</span></span>

<span data-ttu-id="08161-1503">例如，如果`E`的表达式`a.b?[0]?.c()`，然后`E0`表达式`a.b[0].c()`并`E1`的表达式`a.b[0]?.c()`。</span><span class="sxs-lookup"><span data-stu-id="08161-1503">For example, if `E` is the expression `a.b?[0]?.c()`, then `E0` is the expression `a.b[0].c()` and `E1` is the expression `a.b[0]?.c()`.</span></span>

<span data-ttu-id="08161-1504">如果`E0`归类为执行任何操作，然后`E`归类为执行任何操作。</span><span class="sxs-lookup"><span data-stu-id="08161-1504">If `E0` is classified as nothing, then `E` is classified as nothing.</span></span> <span data-ttu-id="08161-1505">否则 E 分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="08161-1505">Otherwise E is classified as a value.</span></span>

<span data-ttu-id="08161-1506">`E0` 并`E1`用于确定的含义`E`:</span><span class="sxs-lookup"><span data-stu-id="08161-1506">`E0` and `E1` are used to determine the meaning of `E`:</span></span>

*  <span data-ttu-id="08161-1507">如果`E`作为发生*statement_expression*的含义`E`等同于该语句</span><span class="sxs-lookup"><span data-stu-id="08161-1507">If `E` occurs as a *statement_expression* the meaning of `E` is the same as the statement</span></span>

   ```csharp
   if ((object)P != null) E1;
   ```

   <span data-ttu-id="08161-1508">除非 P 计算仅一次。</span><span class="sxs-lookup"><span data-stu-id="08161-1508">except that P is evaluated only once.</span></span>

*  <span data-ttu-id="08161-1509">否则为如果`E0`归类为任何内容将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-1509">Otherwise, if `E0` is classified as nothing a compile-time error occurs.</span></span>

*  <span data-ttu-id="08161-1510">否则，让`T0`是类型的`E0`。</span><span class="sxs-lookup"><span data-stu-id="08161-1510">Otherwise, let `T0` be the type of `E0`.</span></span>

   *  <span data-ttu-id="08161-1511">如果`T0`是类型参数都不知道为引用类型或不可以为 null 的值类型，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-1511">If `T0` is a type parameter that is not known to be a reference type or a non-nullable value type, a compile-time error occurs.</span></span>

   *  <span data-ttu-id="08161-1512">如果`T0`为非空值类型，则类型`E`是`T0?`，和的含义`E`相同</span><span class="sxs-lookup"><span data-stu-id="08161-1512">If `T0` is a non-nullable value type, then the type of `E` is `T0?`, and the meaning of `E` is the same as</span></span>

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      <span data-ttu-id="08161-1513">只不过`P`只计算一次。</span><span class="sxs-lookup"><span data-stu-id="08161-1513">except that `P` is evaluated only once.</span></span>

   *  <span data-ttu-id="08161-1514">否则 E 的类型是 T0，和 E 的含义等同于</span><span class="sxs-lookup"><span data-stu-id="08161-1514">Otherwise the type of E is T0, and the meaning of E is the same as</span></span>

      ```csharp
      ((object)P == null) ? null : E1
      ```

      <span data-ttu-id="08161-1515">只不过`P`只计算一次。</span><span class="sxs-lookup"><span data-stu-id="08161-1515">except that `P` is evaluated only once.</span></span>

<span data-ttu-id="08161-1516">如果`E1`本身*null_conditional_expression*，然后这些规则应用同样，嵌套的测试`null`直到不再有`?`的并一直向下减少了表达式主表达式`E0`。</span><span class="sxs-lookup"><span data-stu-id="08161-1516">If `E1` is itself a *null_conditional_expression*, then these rules are applied again, nesting the tests for `null` until there are no further `?`'s, and the expression has been reduced all the way down to the primary-expression `E0`.</span></span>

<span data-ttu-id="08161-1517">例如，如果表达式`a.b?[0]?.c()`发生作为语句的表达式，如在语句中所示：</span><span class="sxs-lookup"><span data-stu-id="08161-1517">For example, if the expression `a.b?[0]?.c()` occurs as a statement-expression, as in the statement:</span></span>
```csharp
a.b?[0]?.c();
```
<span data-ttu-id="08161-1518">它的含义等同于：</span><span class="sxs-lookup"><span data-stu-id="08161-1518">its meaning is equivalent to:</span></span>
```csharp
if (a.b != null) a.b[0]?.c();
```
<span data-ttu-id="08161-1519">这同样是等效于：</span><span class="sxs-lookup"><span data-stu-id="08161-1519">which again is equivalent to:</span></span>
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
<span data-ttu-id="08161-1520">只不过`a.b`和`a.b[0]`计算仅一次。</span><span class="sxs-lookup"><span data-stu-id="08161-1520">Except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

<span data-ttu-id="08161-1521">如果它出现在其中使用它的值，如下所示： 上下文中：</span><span class="sxs-lookup"><span data-stu-id="08161-1521">If it occurs in a context where its value is used, as in:</span></span>
```csharp
var x = a.b?[0]?.c();
```
<span data-ttu-id="08161-1522">并假设的最后一个调用的类型不是不可以为 null 的值类型，其含义等同于：</span><span class="sxs-lookup"><span data-stu-id="08161-1522">and assuming that the type of the final invocation is not a non-nullable value type, its meaning is equivalent to:</span></span>
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
<span data-ttu-id="08161-1523">只不过`a.b`和`a.b[0]`计算仅一次。</span><span class="sxs-lookup"><span data-stu-id="08161-1523">except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

#### <a name="null-conditional-expressions-as-projection-initializers"></a><span data-ttu-id="08161-1524">Null 条件表达式作为投影初始值设定项</span><span class="sxs-lookup"><span data-stu-id="08161-1524">Null-conditional expressions as projection initializers</span></span>

<span data-ttu-id="08161-1525">Null 条件表达式仅允许作为*member_declarator*中*anonymous_object_creation_expression* ([匿名对象创建表达式](expressions.md#anonymous-object-creation-expressions)) 如果它以结尾 （根据需要 null 条件） 成员访问。</span><span class="sxs-lookup"><span data-stu-id="08161-1525">A null-conditional expression is only allowed as a *member_declarator* in an *anonymous_object_creation_expression* ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) if it ends with an (optionally null-conditional) member access.</span></span> <span data-ttu-id="08161-1526">从语法角度而言，此要求可表示为：</span><span class="sxs-lookup"><span data-stu-id="08161-1526">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

<span data-ttu-id="08161-1527">这是一种特殊的语法的情况*null_conditional_expression*上面。</span><span class="sxs-lookup"><span data-stu-id="08161-1527">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="08161-1528">对于生产*member_declarator*中[匿名对象创建表达式](expressions.md#anonymous-object-creation-expressions)只包含*null_conditional_member_access*。</span><span class="sxs-lookup"><span data-stu-id="08161-1528">The production for *member_declarator* in [Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions) then includes only *null_conditional_member_access*.</span></span>

#### <a name="null-conditional-expressions-as-statement-expressions"></a><span data-ttu-id="08161-1529">Null 条件表达式作为语句表达式</span><span class="sxs-lookup"><span data-stu-id="08161-1529">Null-conditional expressions as statement expressions</span></span>

<span data-ttu-id="08161-1530">Null 条件表达式仅允许作为*statement_expression* ([表达式语句](statements.md#expression-statements)) 如果使用的调用结束。</span><span class="sxs-lookup"><span data-stu-id="08161-1530">A null-conditional expression is only allowed as a *statement_expression* ([Expression statements](statements.md#expression-statements)) if it ends with an invocation.</span></span> <span data-ttu-id="08161-1531">从语法角度而言，此要求可表示为：</span><span class="sxs-lookup"><span data-stu-id="08161-1531">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="08161-1532">这是一种特殊的语法的情况*null_conditional_expression*上面。</span><span class="sxs-lookup"><span data-stu-id="08161-1532">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="08161-1533">对于生产*statement_expression*中[表达式语句](statements.md#expression-statements)只包含*null_conditional_invocation_expression*。</span><span class="sxs-lookup"><span data-stu-id="08161-1533">The production for *statement_expression* in [Expression statements](statements.md#expression-statements) then includes only *null_conditional_invocation_expression*.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="08161-1534">一元加运算符</span><span class="sxs-lookup"><span data-stu-id="08161-1534">Unary plus operator</span></span>

<span data-ttu-id="08161-1535">有关操作的窗体`+x`，一元运算符重载决策 ([一元运算符重载决策](expressions.md#unary-operator-overload-resolution)) 应用以选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="08161-1535">For an operation of the form `+x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08161-1536">操作数转换为所选运算符的参数类型和结果的类型是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1536">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="08161-1537">预定义的一元加运算符为：</span><span class="sxs-lookup"><span data-stu-id="08161-1537">The predefined unary plus operators are:</span></span>

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

<span data-ttu-id="08161-1538">对于每个这些运算符，结果就是操作数的值。</span><span class="sxs-lookup"><span data-stu-id="08161-1538">For each of these operators, the result is simply the value of the operand.</span></span>

### <a name="unary-minus-operator"></a><span data-ttu-id="08161-1539">一元负运算符</span><span class="sxs-lookup"><span data-stu-id="08161-1539">Unary minus operator</span></span>

<span data-ttu-id="08161-1540">有关操作的窗体`-x`，一元运算符重载决策 ([一元运算符重载决策](expressions.md#unary-operator-overload-resolution)) 应用以选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="08161-1540">For an operation of the form `-x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08161-1541">操作数转换为所选运算符的参数类型和结果的类型是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1541">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="08161-1542">预定义的求反运算符为：</span><span class="sxs-lookup"><span data-stu-id="08161-1542">The predefined negation operators are:</span></span>

*  <span data-ttu-id="08161-1543">整数求反：</span><span class="sxs-lookup"><span data-stu-id="08161-1543">Integer negation:</span></span>

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   <span data-ttu-id="08161-1544">通过减去计算结果`x`从零开始。</span><span class="sxs-lookup"><span data-stu-id="08161-1544">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="08161-1545">如果的值的`x`是最小的可表示的操作数类型值 (-2 ^31 个`int`或-2 ^63 有关`long`)，然后的算术求反`x`不能显示中的操作数类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1545">If the value of of `x` is the smallest representable value of the operand type (-2^31 for `int` or -2^63 for `long`), then the mathematical negation of `x` is not representable within the operand type.</span></span> <span data-ttu-id="08161-1546">如果发生此情况中`checked`上下文中，`System.OverflowException`引发; 如果它出现在`unchecked`上下文中，结果是操作数的值，并且不报告溢出。</span><span class="sxs-lookup"><span data-stu-id="08161-1546">If this occurs within a `checked` context, a `System.OverflowException` is thrown; if it occurs within an `unchecked` context, the result is the value of the operand and the overflow is not reported.</span></span>

   <span data-ttu-id="08161-1547">求反运算符的操作数是否为类型`uint`，它将转换为类型`long`，并且结果的类型为`long`。</span><span class="sxs-lookup"><span data-stu-id="08161-1547">If the operand of the negation operator is of type `uint`, it is converted to type `long`, and the type of the result is `long`.</span></span> <span data-ttu-id="08161-1548">例外情况是规则，允许`int`值介于-2147483648 (-2 ^31) 写为十进制整数 ([整数文本](lexical-structure.md#integer-literals))。</span><span class="sxs-lookup"><span data-stu-id="08161-1548">An exception is the rule that permits the `int` value -2147483648 (-2^31) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

   <span data-ttu-id="08161-1549">如果求反运算符的操作数是类型`ulong`，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-1549">If the operand of the negation operator is of type `ulong`, a compile-time error occurs.</span></span> <span data-ttu-id="08161-1550">例外情况是规则，允许`long`值-9223372036854775808 (-2 ^63) 写为十进制整数 ([整数文本](lexical-structure.md#integer-literals))。</span><span class="sxs-lookup"><span data-stu-id="08161-1550">An exception is the rule that permits the `long` value -9223372036854775808 (-2^63) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

*  <span data-ttu-id="08161-1551">浮点求反：</span><span class="sxs-lookup"><span data-stu-id="08161-1551">Floating-point negation:</span></span>

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   <span data-ttu-id="08161-1552">结果为的值`x`符号相反的。</span><span class="sxs-lookup"><span data-stu-id="08161-1552">The result is the value of `x` with its sign inverted.</span></span> <span data-ttu-id="08161-1553">如果`x`为 NaN，则结果也为 NaN。</span><span class="sxs-lookup"><span data-stu-id="08161-1553">If `x` is NaN, the result is also NaN.</span></span>

*  <span data-ttu-id="08161-1554">十进制求反：</span><span class="sxs-lookup"><span data-stu-id="08161-1554">Decimal negation:</span></span>

   ```csharp
   decimal operator -(decimal x);
   ```

   <span data-ttu-id="08161-1555">通过减去计算结果`x`从零开始。</span><span class="sxs-lookup"><span data-stu-id="08161-1555">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="08161-1556">十进制求反运算等效于使用一元负运算符类型的`System.Decimal`。</span><span class="sxs-lookup"><span data-stu-id="08161-1556">Decimal negation is equivalent to using the unary minus operator of type `System.Decimal`.</span></span>

### <a name="logical-negation-operator"></a><span data-ttu-id="08161-1557">逻辑求反运算符</span><span class="sxs-lookup"><span data-stu-id="08161-1557">Logical negation operator</span></span>

<span data-ttu-id="08161-1558">有关操作的窗体`!x`，一元运算符重载决策 ([一元运算符重载决策](expressions.md#unary-operator-overload-resolution)) 应用以选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="08161-1558">For an operation of the form `!x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08161-1559">操作数转换为所选运算符的参数类型和结果的类型是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1559">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="08161-1560">只有一个预定义的逻辑求反运算符存在：</span><span class="sxs-lookup"><span data-stu-id="08161-1560">Only one predefined logical negation operator exists:</span></span>
```csharp
bool operator !(bool x);
```

<span data-ttu-id="08161-1561">此运算符将计算操作数的逻辑求反运算：如果操作数是`true`，结果是`false`。</span><span class="sxs-lookup"><span data-stu-id="08161-1561">This operator computes the logical negation of the operand: If the operand is `true`, the result is `false`.</span></span> <span data-ttu-id="08161-1562">如果操作数是`false`，结果是`true`。</span><span class="sxs-lookup"><span data-stu-id="08161-1562">If the operand is `false`, the result is `true`.</span></span>

### <a name="bitwise-complement-operator"></a><span data-ttu-id="08161-1563">按位求补运算符</span><span class="sxs-lookup"><span data-stu-id="08161-1563">Bitwise complement operator</span></span>

<span data-ttu-id="08161-1564">有关操作的窗体`~x`，一元运算符重载决策 ([一元运算符重载决策](expressions.md#unary-operator-overload-resolution)) 应用以选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="08161-1564">For an operation of the form `~x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08161-1565">操作数转换为所选运算符的参数类型和结果的类型是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1565">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="08161-1566">预定义的按位求补运算符为：</span><span class="sxs-lookup"><span data-stu-id="08161-1566">The predefined bitwise complement operators are:</span></span>
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

<span data-ttu-id="08161-1567">对于每个这些运算符，该操作的结果是按位求补`x`。</span><span class="sxs-lookup"><span data-stu-id="08161-1567">For each of these operators, the result of the operation is the bitwise complement of `x`.</span></span>

<span data-ttu-id="08161-1568">每个枚举类型`E`隐式提供了以下的按位求补运算符：</span><span class="sxs-lookup"><span data-stu-id="08161-1568">Every enumeration type `E` implicitly provides the following bitwise complement operator:</span></span>

```csharp
E operator ~(E x);
```

<span data-ttu-id="08161-1569">计算结果`~x`，其中`x`是枚举类型的表达式`E`与基础类型`U`，正是与评估相同`(E)(~(U)x)`，只不过转换为`E`是始终作为执行中的 if`unchecked`上下文 ([checked 和 unchecked 运算符](expressions.md#the-checked-and-unchecked-operators))。</span><span class="sxs-lookup"><span data-stu-id="08161-1569">The result of evaluating `~x`, where `x` is an expression of an enumeration type `E` with an underlying type `U`, is exactly the same as evaluating `(E)(~(U)x)`, except that the conversion to `E` is always performed as if in an `unchecked` context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span>

### <a name="prefix-increment-and-decrement-operators"></a><span data-ttu-id="08161-1570">前缀增量和减量运算符</span><span class="sxs-lookup"><span data-stu-id="08161-1570">Prefix increment and decrement operators</span></span>

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

<span data-ttu-id="08161-1571">操作数的前缀递增或递减操作必须是归类为变量、 属性访问或索引器访问的表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-1571">The operand of a prefix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="08161-1572">操作的结果是类型的操作数相同的值。</span><span class="sxs-lookup"><span data-stu-id="08161-1572">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="08161-1573">如果操作数的前缀递增或递减操作是一个属性或索引器访问、 属性或索引器必须同时具有`get`和一个`set`访问器。</span><span class="sxs-lookup"><span data-stu-id="08161-1573">If the operand of a prefix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="08161-1574">如果这不是这种情况，绑定时出错。</span><span class="sxs-lookup"><span data-stu-id="08161-1574">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="08161-1575">一元运算符重载决策 ([一元运算符重载决策](expressions.md#unary-operator-overload-resolution)) 应用以选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="08161-1575">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08161-1576">预定义`++`并`--`运算符存在以下类型： `sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char``float`， `double`， `decimal`，和任何枚举类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1576">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="08161-1577">预定义`++`运算符将返回通过将 1 添加到操作数，并在预定义生成的值`--`运算符返回生成的数减去 1 的操作数的值。</span><span class="sxs-lookup"><span data-stu-id="08161-1577">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="08161-1578">在中`checked`情况下，如果此加法或减法的结果是结果类型的范围之外，结果类型是整数类型或枚举类型`System.OverflowException`引发。</span><span class="sxs-lookup"><span data-stu-id="08161-1578">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="08161-1579">运行时处理的前缀递增或递减运算的窗体`++x`或`--x`包括以下步骤：</span><span class="sxs-lookup"><span data-stu-id="08161-1579">The run-time processing of a prefix increment or decrement operation of the form `++x` or `--x` consists of the following steps:</span></span>

*   <span data-ttu-id="08161-1580">如果`x`分类为变量：</span><span class="sxs-lookup"><span data-stu-id="08161-1580">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="08161-1581">`x` 计算为生成变量。</span><span class="sxs-lookup"><span data-stu-id="08161-1581">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="08161-1582">所选的运算符调用值为`x`作为其参数。</span><span class="sxs-lookup"><span data-stu-id="08161-1582">The selected operator is invoked with the value of `x` as its argument.</span></span>
    * <span data-ttu-id="08161-1583">运算符返回的值存储在给定的评估结果的位置`x`。</span><span class="sxs-lookup"><span data-stu-id="08161-1583">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="08161-1584">运算符返回的值将成为操作的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-1584">The value returned by the operator becomes the result of the operation.</span></span>
*   <span data-ttu-id="08161-1585">如果`x`归类为属性或索引器访问：</span><span class="sxs-lookup"><span data-stu-id="08161-1585">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="08161-1586">实例表达式 (如果`x`不是`static`) 和参数列表 (如果`x`的索引器访问) 与关联`x`计算，然后在后续结果`get`和`set`取值函数调用。</span><span class="sxs-lookup"><span data-stu-id="08161-1586">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="08161-1587">`get`访问器的`x`调用。</span><span class="sxs-lookup"><span data-stu-id="08161-1587">The `get` accessor of `x` is invoked.</span></span>
    * <span data-ttu-id="08161-1588">使用返回的值调用所选的运算符`get`作为其参数的取值函数。</span><span class="sxs-lookup"><span data-stu-id="08161-1588">The selected operator is invoked with the value returned by the `get` accessor as its argument.</span></span>
    * <span data-ttu-id="08161-1589">`set`访问器的`x`作为运算符所返回的值调用其`value`参数。</span><span class="sxs-lookup"><span data-stu-id="08161-1589">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="08161-1590">运算符返回的值将成为操作的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-1590">The value returned by the operator becomes the result of the operation.</span></span>

<span data-ttu-id="08161-1591">`++`并`--`运算符也支持后缀表示法 ([后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators))。</span><span class="sxs-lookup"><span data-stu-id="08161-1591">The `++` and `--` operators also support postfix notation ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="08161-1592">通常情况下的结果`x++`或`x--`的值`x`前该操作，而的结果`++x`或`--x`的值`x`操作完成后。</span><span class="sxs-lookup"><span data-stu-id="08161-1592">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="08161-1593">在任一情况下，`x`本身完成操作后具有相同的值。</span><span class="sxs-lookup"><span data-stu-id="08161-1593">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="08161-1594">`operator++`或`operator--`实现可以使用后缀或前缀表示法来调用。</span><span class="sxs-lookup"><span data-stu-id="08161-1594">An `operator++` or `operator--` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="08161-1595">不能具有这两种表示的运算符的不同实现。</span><span class="sxs-lookup"><span data-stu-id="08161-1595">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="08161-1596">强制转换表达式</span><span class="sxs-lookup"><span data-stu-id="08161-1596">Cast expressions</span></span>

<span data-ttu-id="08161-1597">一个*cast_expression*用于将表达式显式转换为给定类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1597">A *cast_expression* is used to explicitly convert an expression to a given type.</span></span>

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

<span data-ttu-id="08161-1598">一个*cast_expression*窗体`(T)E`，其中`T`是*类型*并`E`是*unary_expression*，执行显式转换 ([显式转换](conversions.md#explicit-conversions)) 的值的`E`键入`T`。</span><span class="sxs-lookup"><span data-stu-id="08161-1598">A *cast_expression* of the form `(T)E`, where `T` is a *type* and `E` is a *unary_expression*, performs an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) of the value of `E` to type `T`.</span></span> <span data-ttu-id="08161-1599">如果不存在从任何显式转换`E`到`T`，绑定时出错。</span><span class="sxs-lookup"><span data-stu-id="08161-1599">If no explicit conversion exists from `E` to `T`, a binding-time error occurs.</span></span> <span data-ttu-id="08161-1600">否则，结果为显式转换生成的值。</span><span class="sxs-lookup"><span data-stu-id="08161-1600">Otherwise, the result is the value produced by the explicit conversion.</span></span> <span data-ttu-id="08161-1601">结果始终分类为一个值，即使`E`表示的变量。</span><span class="sxs-lookup"><span data-stu-id="08161-1601">The result is always classified as a value, even if `E` denotes a variable.</span></span>

<span data-ttu-id="08161-1602">语法*cast_expression*会导致某些语法多义性。</span><span class="sxs-lookup"><span data-stu-id="08161-1602">The grammar for a *cast_expression* leads to certain syntactic ambiguities.</span></span> <span data-ttu-id="08161-1603">例如，表达式`(x)-y`是可解释为*cast_expression* (的强制转换`-y`键入`x`) 或*additive_expression*与结合使用*parenthesized_expression* (用于计算值`x - y)`。</span><span class="sxs-lookup"><span data-stu-id="08161-1603">For example, the expression `(x)-y` could either be interpreted as a *cast_expression* (a cast of `-y` to type `x`) or as an *additive_expression* combined with a *parenthesized_expression* (which computes the value `x - y)`.</span></span>

<span data-ttu-id="08161-1604">若要解决*cast_expression*存在二义性，以下规则：一个或多个序列*令牌*s ([空白区域](lexical-structure.md#white-space)) 括在括号中被视为开头*cast_expression*仅当至少一个以下条件成立：</span><span class="sxs-lookup"><span data-stu-id="08161-1604">To resolve *cast_expression* ambiguities, the following rule exists: A sequence of one or more *token*s ([White space](lexical-structure.md#white-space)) enclosed in parentheses is considered the start of a *cast_expression* only if at least one of the following are true:</span></span>

*  <span data-ttu-id="08161-1605">标记的顺序是为正确的语法*类型*，而不是*表达式*。</span><span class="sxs-lookup"><span data-stu-id="08161-1605">The sequence of tokens is correct grammar for a *type*, but not for an *expression*.</span></span>
*  <span data-ttu-id="08161-1606">标记的顺序是为正确的语法*类型*，并紧接在右括号是令牌"`~`"，令牌"`!`"，令牌"`(`"、 *标识符*([Unicode 字符转义序列](lexical-structure.md#unicode-character-escape-sequences))、 一个*文字*([文本](lexical-structure.md#literals))，或任何*关键字*([关键字](lexical-structure.md#keywords)) 除外`as`和`is`。</span><span class="sxs-lookup"><span data-stu-id="08161-1606">The sequence of tokens is correct grammar for a *type*, and the token immediately following the closing parentheses is the token "`~`", the token "`!`", the token "`(`", an *identifier* ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)), a *literal* ([Literals](lexical-structure.md#literals)), or any *keyword* ([Keywords](lexical-structure.md#keywords)) except `as` and `is`.</span></span>

<span data-ttu-id="08161-1607">术语"正确的语法"表示仅标记的顺序必须符合特定的语法产生上面。</span><span class="sxs-lookup"><span data-stu-id="08161-1607">The term "correct grammar" above means only that the sequence of tokens must conform to the particular grammatical production.</span></span> <span data-ttu-id="08161-1608">它专门不考虑任何构成的标识符的实际含义。</span><span class="sxs-lookup"><span data-stu-id="08161-1608">It specifically does not consider the actual meaning of any constituent identifiers.</span></span> <span data-ttu-id="08161-1609">例如，如果`x`并`y`是标识符，然后`x.y`是正确的语法，对于类型，即使`x.y`实际并不表示一种类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1609">For example, if `x` and `y` are identifiers, then `x.y` is correct grammar for a type, even if `x.y` doesn't actually denote a type.</span></span>

<span data-ttu-id="08161-1610">从消除歧义规则，它遵循的如果`x`和`y`是标识符，都`(x)y`， `(x)(y)`，并`(x)(-y)`是*cast_expression*s，但`(x)-y`不是，即使`x`标识的类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1610">From the disambiguation rule it follows that, if `x` and `y` are identifiers, `(x)y`, `(x)(y)`, and `(x)(-y)` are *cast_expression*s, but `(x)-y` is not, even if `x` identifies a type.</span></span> <span data-ttu-id="08161-1611">但是，如果`x`是标识预定义的类型的关键字 (如`int`)，则所有四个窗体*cast_expression*s （因为这种关键字不可能是表达式本身）。</span><span class="sxs-lookup"><span data-stu-id="08161-1611">However, if `x` is a keyword that identifies a predefined type (such as `int`), then all four forms are *cast_expression*s (because such a keyword could not possibly be an expression by itself).</span></span>

### <a name="await-expressions"></a><span data-ttu-id="08161-1612">Await 表达式</span><span class="sxs-lookup"><span data-stu-id="08161-1612">Await expressions</span></span>

<span data-ttu-id="08161-1613">Await 运算符用于挂起封闭的异步函数的求值，直到在操作数表示的异步操作完成。</span><span class="sxs-lookup"><span data-stu-id="08161-1613">The await operator is used to suspend evaluation of the enclosing async function until the asynchronous operation represented by the operand has completed.</span></span>

```antlr
await_expression
    : 'await' unary_expression
    ;
```

<span data-ttu-id="08161-1614">*Await_expression*只允许在异步函数的主体 ([迭代器](classes.md#iterators))。</span><span class="sxs-lookup"><span data-stu-id="08161-1614">An *await_expression* is only allowed in the body of an async function ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="08161-1615">在最近的封闭异步函数*await_expression*可能不会出现在这些位置：</span><span class="sxs-lookup"><span data-stu-id="08161-1615">Within the nearest enclosing async function, an *await_expression* may not occur in these places:</span></span>

*  <span data-ttu-id="08161-1616">在嵌套的 （非异步） 匿名函数</span><span class="sxs-lookup"><span data-stu-id="08161-1616">Inside a nested (non-async) anonymous function</span></span>
*  <span data-ttu-id="08161-1617">在的块内*lock_statement*</span><span class="sxs-lookup"><span data-stu-id="08161-1617">Inside the block of a *lock_statement*</span></span>
*  <span data-ttu-id="08161-1618">在不安全的上下文中</span><span class="sxs-lookup"><span data-stu-id="08161-1618">In an unsafe context</span></span>

<span data-ttu-id="08161-1619">请注意， *await_expression*不能出现在大多数位置内*query_expression*，因为这些语法上转换为使用非异步 lambda 表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-1619">Note that an *await_expression* cannot occur in most places within a *query_expression*, because those are syntactically transformed to use non-async lambda expressions.</span></span>

<span data-ttu-id="08161-1620">在异步函数，内部`await`不能用作标识符。</span><span class="sxs-lookup"><span data-stu-id="08161-1620">Inside of an async function, `await` cannot be used as an identifier.</span></span> <span data-ttu-id="08161-1621">因此是 await 表达式和各种表达式涉及标识符之间没有句法歧义。</span><span class="sxs-lookup"><span data-stu-id="08161-1621">There is therefore no syntactic ambiguity between await-expressions and various expressions involving identifiers.</span></span> <span data-ttu-id="08161-1622">异步函数之外`await`充当正常的标识符。</span><span class="sxs-lookup"><span data-stu-id="08161-1622">Outside of async functions, `await` acts as a normal identifier.</span></span>

<span data-ttu-id="08161-1623">操作数*await_expression*称为***任务***。</span><span class="sxs-lookup"><span data-stu-id="08161-1623">The operand of an *await_expression* is called the ***task***.</span></span> <span data-ttu-id="08161-1624">表示异步操作，可能会或可能不完整时*await_expression*进行计算。</span><span class="sxs-lookup"><span data-stu-id="08161-1624">It represents an asynchronous operation that may or may not be complete at the time the *await_expression* is evaluated.</span></span> <span data-ttu-id="08161-1625">Await 运算符的目的是能够挂起封闭的异步函数的执行，直到所等待的任务已完成，然后获取其结果。</span><span class="sxs-lookup"><span data-stu-id="08161-1625">The purpose of the await operator is to suspend execution of the enclosing async function until the awaited task is complete, and then obtain its outcome.</span></span>

#### <a name="awaitable-expressions"></a><span data-ttu-id="08161-1626">可等待操作的表达式</span><span class="sxs-lookup"><span data-stu-id="08161-1626">Awaitable expressions</span></span>

<span data-ttu-id="08161-1627">该任务的 await 表达式必须是***awaitable***。</span><span class="sxs-lookup"><span data-stu-id="08161-1627">The task of an await expression is required to be ***awaitable***.</span></span> <span data-ttu-id="08161-1628">一个表达式`t`是以下之一保存的情况下可等待：</span><span class="sxs-lookup"><span data-stu-id="08161-1628">An expression `t` is awaitable if one of the following holds:</span></span>

*  <span data-ttu-id="08161-1629">`t` 编译时类型 `dynamic`</span><span class="sxs-lookup"><span data-stu-id="08161-1629">`t` is of compile time type `dynamic`</span></span>
*  <span data-ttu-id="08161-1630">`t` 已调用的可访问实例或扩展方法`GetAwaiter`没有参数和没有类型参数和返回类型与`A`为其满足以下所有保存：</span><span class="sxs-lookup"><span data-stu-id="08161-1630">`t` has an accessible instance or extension method called `GetAwaiter` with no parameters and no type parameters, and a return type `A` for which all of the following hold:</span></span>
   * <span data-ttu-id="08161-1631">`A` 实现接口`System.Runtime.CompilerServices.INotifyCompletion`(以后称为`INotifyCompletion`为简洁起见)</span><span class="sxs-lookup"><span data-stu-id="08161-1631">`A` implements the interface `System.Runtime.CompilerServices.INotifyCompletion` (hereafter known as `INotifyCompletion` for brevity)</span></span>
   * <span data-ttu-id="08161-1632">`A` 具有可访问的、 可读的实例属性`IsCompleted`的类型 `bool`</span><span class="sxs-lookup"><span data-stu-id="08161-1632">`A` has an accessible, readable instance property `IsCompleted` of type `bool`</span></span>
   * <span data-ttu-id="08161-1633">`A` 具有可访问的实例方法`GetResult`没有参数，没有类型参数</span><span class="sxs-lookup"><span data-stu-id="08161-1633">`A` has an accessible instance method `GetResult` with no parameters and no type parameters</span></span>

<span data-ttu-id="08161-1634">目的`GetAwaiter`的方法是获取***awaiter***任务。</span><span class="sxs-lookup"><span data-stu-id="08161-1634">The purpose of the `GetAwaiter` method is to obtain an ***awaiter*** for the task.</span></span> <span data-ttu-id="08161-1635">类型`A`称为***awaiter 类型***await 表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-1635">The type `A` is called the ***awaiter type*** for the await expression.</span></span>

<span data-ttu-id="08161-1636">用途`IsCompleted`属性将确定任务是否已完成。</span><span class="sxs-lookup"><span data-stu-id="08161-1636">The purpose of the `IsCompleted` property is to determine if the task is already complete.</span></span> <span data-ttu-id="08161-1637">如果是这样，就不必挂起评估。</span><span class="sxs-lookup"><span data-stu-id="08161-1637">If so, there is no need to suspend evaluation.</span></span>

<span data-ttu-id="08161-1638">目的`INotifyCompletion.OnCompleted`方法是注册到任务;"继续"即委托 (类型的`System.Action`) 任务完成后，将调用的。</span><span class="sxs-lookup"><span data-stu-id="08161-1638">The purpose of the `INotifyCompletion.OnCompleted` method is to sign up a "continuation" to the task; i.e. a delegate (of type `System.Action`) that will be invoked once the task is complete.</span></span>

<span data-ttu-id="08161-1639">用途`GetResult`方法是完成后获得的结果的任务。</span><span class="sxs-lookup"><span data-stu-id="08161-1639">The purpose of the `GetResult` method is to obtain the outcome of the task once it is complete.</span></span> <span data-ttu-id="08161-1640">这种情况可能会成功完成后，可能与结果值，也可能是由引发该异常`GetResult`方法。</span><span class="sxs-lookup"><span data-stu-id="08161-1640">This outcome may be successful completion, possibly with a result value, or it may be an exception which is thrown by the `GetResult` method.</span></span>

#### <a name="classification-of-await-expressions"></a><span data-ttu-id="08161-1641">分类的 await 表达式</span><span class="sxs-lookup"><span data-stu-id="08161-1641">Classification of await expressions</span></span>

<span data-ttu-id="08161-1642">表达式`await t`分类的表达式的相同方式`(t).GetAwaiter().GetResult()`。</span><span class="sxs-lookup"><span data-stu-id="08161-1642">The expression `await t` is classified the same way as the expression `(t).GetAwaiter().GetResult()`.</span></span> <span data-ttu-id="08161-1643">因此，如果的返回类型的`GetResult`是`void`，则*await_expression*归类为执行任何操作。</span><span class="sxs-lookup"><span data-stu-id="08161-1643">Thus, if the return type of `GetResult` is `void`, the *await_expression* is classified as nothing.</span></span> <span data-ttu-id="08161-1644">如果它具有非 void 返回类型`T`，则*await_expression*分类类型的值为`T`。</span><span class="sxs-lookup"><span data-stu-id="08161-1644">If it has a non-void return type `T`, the *await_expression* is classified as a value of type `T`.</span></span>

#### <a name="runtime-evaluation-of-await-expressions"></a><span data-ttu-id="08161-1645">运行时计算的 await 表达式</span><span class="sxs-lookup"><span data-stu-id="08161-1645">Runtime evaluation of await expressions</span></span>

<span data-ttu-id="08161-1646">在运行时，该表达式`await t`的计算方式如下：</span><span class="sxs-lookup"><span data-stu-id="08161-1646">At runtime, the expression `await t` is evaluated as follows:</span></span>

*  <span data-ttu-id="08161-1647">Awaiter`a`通过计算该表达式获得`(t).GetAwaiter()`。</span><span class="sxs-lookup"><span data-stu-id="08161-1647">An awaiter `a` is obtained by evaluating the expression `(t).GetAwaiter()`.</span></span>
*  <span data-ttu-id="08161-1648">一个`bool``b`通过计算该表达式获得`(a).IsCompleted`。</span><span class="sxs-lookup"><span data-stu-id="08161-1648">A `bool` `b` is obtained by evaluating the expression `(a).IsCompleted`.</span></span>
*  <span data-ttu-id="08161-1649">如果`b`是`false`，然后计算取决于是否`a`实现接口`System.Runtime.CompilerServices.ICriticalNotifyCompletion`(以后称为`ICriticalNotifyCompletion`为简洁起见)。</span><span class="sxs-lookup"><span data-stu-id="08161-1649">If `b` is `false` then evaluation depends on whether `a` implements the interface `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (hereafter known as `ICriticalNotifyCompletion` for brevity).</span></span> <span data-ttu-id="08161-1650">在绑定时间; 执行此检查即在运行时如果`a`具有的编译时类型`dynamic`，而是在编译时以其他方式。</span><span class="sxs-lookup"><span data-stu-id="08161-1650">This check is done at binding time; i.e. at runtime if `a` has the compile time type `dynamic`, and at compile time otherwise.</span></span> <span data-ttu-id="08161-1651">让`r`表示恢复委托 ([迭代器](classes.md#iterators)):</span><span class="sxs-lookup"><span data-stu-id="08161-1651">Let `r` denote the resumption delegate ([Iterators](classes.md#iterators)):</span></span>
    * <span data-ttu-id="08161-1652">如果`a`不实现`ICriticalNotifyCompletion`，然后表达式`(a as (INotifyCompletion)).OnCompleted(r)`进行计算。</span><span class="sxs-lookup"><span data-stu-id="08161-1652">If `a` does not implement `ICriticalNotifyCompletion`, then the expression `(a as (INotifyCompletion)).OnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="08161-1653">如果`a`实现`ICriticalNotifyCompletion`，然后表达式`(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)`进行计算。</span><span class="sxs-lookup"><span data-stu-id="08161-1653">If `a` does implement `ICriticalNotifyCompletion`, then the expression `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="08161-1654">然后暂停计算，以及将控制返回给异步函数的当前调用方。</span><span class="sxs-lookup"><span data-stu-id="08161-1654">Evaluation is then suspended, and control is returned to the current caller of the async function.</span></span>
*  <span data-ttu-id="08161-1655">任一后立即 (如果`b`已`true`)，或更高版本调用恢复委托时 (如果`b`已`false`)，该表达式`(a).GetResult()`进行计算。</span><span class="sxs-lookup"><span data-stu-id="08161-1655">Either immediately after (if `b` was `true`), or upon later invocation of the resumption delegate (if `b` was `false`), the expression `(a).GetResult()` is evaluated.</span></span> <span data-ttu-id="08161-1656">如果它返回一个值，该值是对*await_expression*。</span><span class="sxs-lookup"><span data-stu-id="08161-1656">If it returns a value, that value is the result of the *await_expression*.</span></span> <span data-ttu-id="08161-1657">否则结果是执行任何操作。</span><span class="sxs-lookup"><span data-stu-id="08161-1657">Otherwise the result is nothing.</span></span>

<span data-ttu-id="08161-1658">接口方法实现的 awaiter`INotifyCompletion.OnCompleted`和`ICriticalNotifyCompletion.UnsafeOnCompleted`应导致委托`r`最多一次调用。</span><span class="sxs-lookup"><span data-stu-id="08161-1658">An awaiter's implementation of the interface methods `INotifyCompletion.OnCompleted` and `ICriticalNotifyCompletion.UnsafeOnCompleted` should cause the delegate `r` to be invoked at most once.</span></span> <span data-ttu-id="08161-1659">否则，将封闭的异步函数的行为是不确定的。</span><span class="sxs-lookup"><span data-stu-id="08161-1659">Otherwise, the behavior of the enclosing async function is undefined.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="08161-1660">算术运算符</span><span class="sxs-lookup"><span data-stu-id="08161-1660">Arithmetic operators</span></span>

<span data-ttu-id="08161-1661">`*`， `/`， `%`， `+`，和`-`运算符被称作算术运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-1661">The `*`, `/`, `%`, `+`, and `-` operators are called the arithmetic operators.</span></span>

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

<span data-ttu-id="08161-1662">如果操作数的算术运算符的已编译时类型`dynamic`，然后动态绑定表达式 ([动态绑定](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="08161-1662">If an operand of an arithmetic operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="08161-1663">在这种情况下该表达式的编译时类型是`dynamic`，如下所述的分辨率将发生在运行时使用这些具有编译时类型的操作数的运行时类型和`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="08161-1663">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

### <a name="multiplication-operator"></a><span data-ttu-id="08161-1664">乘法运算符</span><span class="sxs-lookup"><span data-stu-id="08161-1664">Multiplication operator</span></span>

<span data-ttu-id="08161-1665">有关操作的窗体`x * y`，二元运算符重载决策 ([二元运算符重载决策](expressions.md#binary-operator-overload-resolution)) 应用以选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="08161-1665">For an operation of the form `x * y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08161-1666">操作数都转换为所选运算符的参数类型和结果的类型是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1666">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="08161-1667">下面列出了预定义的乘法运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-1667">The predefined multiplication operators are listed below.</span></span> <span data-ttu-id="08161-1668">所有运算符将都计算的产品`x`和`y`。</span><span class="sxs-lookup"><span data-stu-id="08161-1668">The operators all compute the product of `x` and `y`.</span></span>

*  <span data-ttu-id="08161-1669">整数乘法：</span><span class="sxs-lookup"><span data-stu-id="08161-1669">Integer multiplication:</span></span>

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   <span data-ttu-id="08161-1670">在中`checked`情况下，该产品已超出范围的结果类型，如果`System.OverflowException`引发。</span><span class="sxs-lookup"><span data-stu-id="08161-1670">In a `checked` context, if the product is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="08161-1671">在`unchecked`上下文，则不报告溢出和范围之外的结果类型的任何重大高顺序位将被丢弃。</span><span class="sxs-lookup"><span data-stu-id="08161-1671">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>


*  <span data-ttu-id="08161-1672">浮点乘法：</span><span class="sxs-lookup"><span data-stu-id="08161-1672">Floating-point multiplication:</span></span>

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   <span data-ttu-id="08161-1673">根据 IEEE 754 算法的规则计算乘积。</span><span class="sxs-lookup"><span data-stu-id="08161-1673">The product is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="08161-1674">下表列出的所有可能组合的非零的有限值、 零、 无穷大和 NaN 的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-1674">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="08161-1675">在表中，`x`和`y`是有限的正值。</span><span class="sxs-lookup"><span data-stu-id="08161-1675">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="08161-1676">`z` 是的结果`x * y`。</span><span class="sxs-lookup"><span data-stu-id="08161-1676">`z` is the result of `x * y`.</span></span> <span data-ttu-id="08161-1677">如果结果为目标类型太大`z`为无穷大。</span><span class="sxs-lookup"><span data-stu-id="08161-1677">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="08161-1678">如果结果为目标类型太小`z`为零。</span><span class="sxs-lookup"><span data-stu-id="08161-1678">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | <span data-ttu-id="08161-1679">+y</span><span class="sxs-lookup"><span data-stu-id="08161-1679">+y</span></span>   | <span data-ttu-id="08161-1680">-y</span><span class="sxs-lookup"><span data-stu-id="08161-1680">-y</span></span>   | <span data-ttu-id="08161-1681">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1681">+0</span></span>  | <span data-ttu-id="08161-1682">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1682">-0</span></span>  | <span data-ttu-id="08161-1683">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1683">+inf</span></span> | <span data-ttu-id="08161-1684">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1684">-inf</span></span> | <span data-ttu-id="08161-1685">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1685">NaN</span></span> | 
   | <span data-ttu-id="08161-1686">+x</span><span class="sxs-lookup"><span data-stu-id="08161-1686">+x</span></span>   | <span data-ttu-id="08161-1687">+z</span><span class="sxs-lookup"><span data-stu-id="08161-1687">+z</span></span>   | <span data-ttu-id="08161-1688">-z</span><span class="sxs-lookup"><span data-stu-id="08161-1688">-z</span></span>   | <span data-ttu-id="08161-1689">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1689">+0</span></span>  | <span data-ttu-id="08161-1690">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1690">-0</span></span>  | <span data-ttu-id="08161-1691">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1691">+inf</span></span> | <span data-ttu-id="08161-1692">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1692">-inf</span></span> | <span data-ttu-id="08161-1693">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1693">NaN</span></span> | 
   | <span data-ttu-id="08161-1694">-x</span><span class="sxs-lookup"><span data-stu-id="08161-1694">-x</span></span>   | <span data-ttu-id="08161-1695">-z</span><span class="sxs-lookup"><span data-stu-id="08161-1695">-z</span></span>   | <span data-ttu-id="08161-1696">+z</span><span class="sxs-lookup"><span data-stu-id="08161-1696">+z</span></span>   | <span data-ttu-id="08161-1697">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1697">-0</span></span>  | <span data-ttu-id="08161-1698">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1698">+0</span></span>  | <span data-ttu-id="08161-1699">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1699">-inf</span></span> | <span data-ttu-id="08161-1700">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1700">+inf</span></span> | <span data-ttu-id="08161-1701">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1701">NaN</span></span> | 
   | <span data-ttu-id="08161-1702">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1702">+0</span></span>   | <span data-ttu-id="08161-1703">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1703">+0</span></span>   | <span data-ttu-id="08161-1704">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1704">-0</span></span>   | <span data-ttu-id="08161-1705">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1705">+0</span></span>  | <span data-ttu-id="08161-1706">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1706">-0</span></span>  | <span data-ttu-id="08161-1707">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1707">NaN</span></span>  | <span data-ttu-id="08161-1708">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1708">NaN</span></span>  | <span data-ttu-id="08161-1709">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1709">NaN</span></span> | 
   | <span data-ttu-id="08161-1710">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1710">-0</span></span>   | <span data-ttu-id="08161-1711">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1711">-0</span></span>   | <span data-ttu-id="08161-1712">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1712">+0</span></span>   | <span data-ttu-id="08161-1713">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1713">-0</span></span>  | <span data-ttu-id="08161-1714">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1714">+0</span></span>  | <span data-ttu-id="08161-1715">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1715">NaN</span></span>  | <span data-ttu-id="08161-1716">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1716">NaN</span></span>  | <span data-ttu-id="08161-1717">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1717">NaN</span></span> | 
   | <span data-ttu-id="08161-1718">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1718">+inf</span></span> | <span data-ttu-id="08161-1719">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1719">+inf</span></span> | <span data-ttu-id="08161-1720">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1720">-inf</span></span> | <span data-ttu-id="08161-1721">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1721">NaN</span></span> | <span data-ttu-id="08161-1722">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1722">NaN</span></span> | <span data-ttu-id="08161-1723">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1723">+inf</span></span> | <span data-ttu-id="08161-1724">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1724">-inf</span></span> | <span data-ttu-id="08161-1725">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1725">NaN</span></span> | 
   | <span data-ttu-id="08161-1726">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1726">-inf</span></span> | <span data-ttu-id="08161-1727">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1727">-inf</span></span> | <span data-ttu-id="08161-1728">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1728">+inf</span></span> | <span data-ttu-id="08161-1729">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1729">NaN</span></span> | <span data-ttu-id="08161-1730">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1730">NaN</span></span> | <span data-ttu-id="08161-1731">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1731">-inf</span></span> | <span data-ttu-id="08161-1732">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1732">+inf</span></span> | <span data-ttu-id="08161-1733">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1733">NaN</span></span> | 
   | <span data-ttu-id="08161-1734">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1734">NaN</span></span>  | <span data-ttu-id="08161-1735">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1735">NaN</span></span>  | <span data-ttu-id="08161-1736">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1736">NaN</span></span>  | <span data-ttu-id="08161-1737">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1737">NaN</span></span> | <span data-ttu-id="08161-1738">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1738">NaN</span></span> | <span data-ttu-id="08161-1739">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1739">NaN</span></span>  | <span data-ttu-id="08161-1740">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1740">NaN</span></span>  | <span data-ttu-id="08161-1741">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1741">NaN</span></span> | 

*  <span data-ttu-id="08161-1742">十进制乘法：</span><span class="sxs-lookup"><span data-stu-id="08161-1742">Decimal multiplication:</span></span>

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   <span data-ttu-id="08161-1743">如果生成的值是太大，无法表示在`decimal`格式，`System.OverflowException`引发。</span><span class="sxs-lookup"><span data-stu-id="08161-1743">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="08161-1744">如果结果值就太小，无法在表示`decimal`格式，则结果为零。</span><span class="sxs-lookup"><span data-stu-id="08161-1744">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="08161-1745">结果，任何舍入之前, 的小数位数是这些标度的两个操作数之和。</span><span class="sxs-lookup"><span data-stu-id="08161-1745">The scale of the result, before any rounding, is the sum of the scales of the two operands.</span></span>

   <span data-ttu-id="08161-1746">十进制乘法相当于使用类型的乘法运算符`System.Decimal`。</span><span class="sxs-lookup"><span data-stu-id="08161-1746">Decimal multiplication is equivalent to using the multiplication operator of type `System.Decimal`.</span></span>


### <a name="division-operator"></a><span data-ttu-id="08161-1747">除法运算符</span><span class="sxs-lookup"><span data-stu-id="08161-1747">Division operator</span></span>

<span data-ttu-id="08161-1748">有关操作的窗体`x / y`，二元运算符重载决策 ([二元运算符重载决策](expressions.md#binary-operator-overload-resolution)) 应用以选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="08161-1748">For an operation of the form `x / y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08161-1749">操作数都转换为所选运算符的参数类型和结果的类型是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1749">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="08161-1750">下面列出了预定义的除法运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-1750">The predefined division operators are listed below.</span></span> <span data-ttu-id="08161-1751">所有运算符将都计算所得的商`x`和`y`。</span><span class="sxs-lookup"><span data-stu-id="08161-1751">The operators all compute the quotient of `x` and `y`.</span></span>

*  <span data-ttu-id="08161-1752">整数除法：</span><span class="sxs-lookup"><span data-stu-id="08161-1752">Integer division:</span></span>

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   <span data-ttu-id="08161-1753">如果右操作数的值为零，`System.DivideByZeroException`引发。</span><span class="sxs-lookup"><span data-stu-id="08161-1753">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="08161-1754">该除法运算将向零结果舍入。</span><span class="sxs-lookup"><span data-stu-id="08161-1754">The division rounds the result towards zero.</span></span> <span data-ttu-id="08161-1755">因此，结果的绝对值是商的小于或等于两个操作数的绝对值的最大可能整数。</span><span class="sxs-lookup"><span data-stu-id="08161-1755">Thus the absolute value of the result is the largest possible integer that is less than or equal to the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="08161-1756">时两个操作数具有相同的登录以及零个或两个操作数具有符号相反时为负，则结果为零或正数。</span><span class="sxs-lookup"><span data-stu-id="08161-1756">The result is zero or positive when the two operands have the same sign and zero or negative when the two operands have opposite signs.</span></span>

   <span data-ttu-id="08161-1757">如果左的操作数是可表示最小`int`或`long`值和右操作数是`-1`，会发生溢出。</span><span class="sxs-lookup"><span data-stu-id="08161-1757">If the left operand is the smallest representable `int` or `long` value and the right operand is `-1`, an overflow occurs.</span></span> <span data-ttu-id="08161-1758">在中`checked`上下文中，这会导致`System.ArithmeticException`（或其中的子类） 引发。</span><span class="sxs-lookup"><span data-stu-id="08161-1758">In a `checked` context, this causes a `System.ArithmeticException` (or a subclass thereof) to be thrown.</span></span> <span data-ttu-id="08161-1759">在`unchecked`上下文中，它是实现定义的还是`System.ArithmeticException`（或其中的子类） 会引发或溢出进入未报告与生成正在的左操作数的值。</span><span class="sxs-lookup"><span data-stu-id="08161-1759">In an `unchecked` context, it is implementation-defined as to whether a `System.ArithmeticException` (or a subclass thereof) is thrown or the overflow goes unreported with the resulting value being that of the left operand.</span></span>

*  <span data-ttu-id="08161-1760">浮点除法运算：</span><span class="sxs-lookup"><span data-stu-id="08161-1760">Floating-point division:</span></span>

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   <span data-ttu-id="08161-1761">根据 IEEE 754 算法的规则计算商。</span><span class="sxs-lookup"><span data-stu-id="08161-1761">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="08161-1762">下表列出的所有可能组合的非零的有限值、 零、 无穷大和 NaN 的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-1762">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="08161-1763">在表中，`x`和`y`是有限的正值。</span><span class="sxs-lookup"><span data-stu-id="08161-1763">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="08161-1764">`z` 是的结果`x / y`。</span><span class="sxs-lookup"><span data-stu-id="08161-1764">`z` is the result of `x / y`.</span></span> <span data-ttu-id="08161-1765">如果结果为目标类型太大`z`为无穷大。</span><span class="sxs-lookup"><span data-stu-id="08161-1765">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="08161-1766">如果结果为目标类型太小`z`为零。</span><span class="sxs-lookup"><span data-stu-id="08161-1766">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="08161-1767">+y</span><span class="sxs-lookup"><span data-stu-id="08161-1767">+y</span></span>   | <span data-ttu-id="08161-1768">-y</span><span class="sxs-lookup"><span data-stu-id="08161-1768">-y</span></span>   | <span data-ttu-id="08161-1769">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1769">+0</span></span>   | <span data-ttu-id="08161-1770">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1770">-0</span></span>   | <span data-ttu-id="08161-1771">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1771">+inf</span></span> | <span data-ttu-id="08161-1772">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1772">-inf</span></span> | <span data-ttu-id="08161-1773">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1773">NaN</span></span>  | 
   | <span data-ttu-id="08161-1774">+x</span><span class="sxs-lookup"><span data-stu-id="08161-1774">+x</span></span>   | <span data-ttu-id="08161-1775">+z</span><span class="sxs-lookup"><span data-stu-id="08161-1775">+z</span></span>   | <span data-ttu-id="08161-1776">-z</span><span class="sxs-lookup"><span data-stu-id="08161-1776">-z</span></span>   | <span data-ttu-id="08161-1777">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1777">+inf</span></span> | <span data-ttu-id="08161-1778">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1778">-inf</span></span> | <span data-ttu-id="08161-1779">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1779">+0</span></span>   | <span data-ttu-id="08161-1780">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1780">-0</span></span>   | <span data-ttu-id="08161-1781">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1781">NaN</span></span>  | 
   | <span data-ttu-id="08161-1782">-x</span><span class="sxs-lookup"><span data-stu-id="08161-1782">-x</span></span>   | <span data-ttu-id="08161-1783">-z</span><span class="sxs-lookup"><span data-stu-id="08161-1783">-z</span></span>   | <span data-ttu-id="08161-1784">+z</span><span class="sxs-lookup"><span data-stu-id="08161-1784">+z</span></span>   | <span data-ttu-id="08161-1785">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1785">-inf</span></span> | <span data-ttu-id="08161-1786">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1786">+inf</span></span> | <span data-ttu-id="08161-1787">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1787">-0</span></span>   | <span data-ttu-id="08161-1788">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1788">+0</span></span>   | <span data-ttu-id="08161-1789">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1789">NaN</span></span>  | 
   | <span data-ttu-id="08161-1790">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1790">+0</span></span>   | <span data-ttu-id="08161-1791">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1791">+0</span></span>   | <span data-ttu-id="08161-1792">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1792">-0</span></span>   | <span data-ttu-id="08161-1793">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1793">NaN</span></span>  | <span data-ttu-id="08161-1794">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1794">NaN</span></span>  | <span data-ttu-id="08161-1795">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1795">+0</span></span>   | <span data-ttu-id="08161-1796">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1796">-0</span></span>   | <span data-ttu-id="08161-1797">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1797">NaN</span></span>  | 
   | <span data-ttu-id="08161-1798">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1798">-0</span></span>   | <span data-ttu-id="08161-1799">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1799">-0</span></span>   | <span data-ttu-id="08161-1800">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1800">+0</span></span>   | <span data-ttu-id="08161-1801">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1801">NaN</span></span>  | <span data-ttu-id="08161-1802">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1802">NaN</span></span>  | <span data-ttu-id="08161-1803">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1803">-0</span></span>   | <span data-ttu-id="08161-1804">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1804">+0</span></span>   | <span data-ttu-id="08161-1805">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1805">NaN</span></span>  | 
   | <span data-ttu-id="08161-1806">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1806">+inf</span></span> | <span data-ttu-id="08161-1807">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1807">+inf</span></span> | <span data-ttu-id="08161-1808">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1808">-inf</span></span> | <span data-ttu-id="08161-1809">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1809">+inf</span></span> | <span data-ttu-id="08161-1810">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1810">-inf</span></span> | <span data-ttu-id="08161-1811">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1811">NaN</span></span>  | <span data-ttu-id="08161-1812">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1812">NaN</span></span>  | <span data-ttu-id="08161-1813">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1813">NaN</span></span>  | 
   | <span data-ttu-id="08161-1814">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1814">-inf</span></span> | <span data-ttu-id="08161-1815">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1815">-inf</span></span> | <span data-ttu-id="08161-1816">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1816">+inf</span></span> | <span data-ttu-id="08161-1817">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1817">-inf</span></span> | <span data-ttu-id="08161-1818">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1818">+inf</span></span> | <span data-ttu-id="08161-1819">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1819">NaN</span></span>  | <span data-ttu-id="08161-1820">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1820">NaN</span></span>  | <span data-ttu-id="08161-1821">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1821">NaN</span></span>  | 
   | <span data-ttu-id="08161-1822">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1822">NaN</span></span>  | <span data-ttu-id="08161-1823">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1823">NaN</span></span>  | <span data-ttu-id="08161-1824">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1824">NaN</span></span>  | <span data-ttu-id="08161-1825">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1825">NaN</span></span>  | <span data-ttu-id="08161-1826">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1826">NaN</span></span>  | <span data-ttu-id="08161-1827">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1827">NaN</span></span>  | <span data-ttu-id="08161-1828">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1828">NaN</span></span>  | <span data-ttu-id="08161-1829">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1829">NaN</span></span>  | 

*  <span data-ttu-id="08161-1830">十进制除法：</span><span class="sxs-lookup"><span data-stu-id="08161-1830">Decimal division:</span></span>

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   <span data-ttu-id="08161-1831">如果右操作数的值为零，`System.DivideByZeroException`引发。</span><span class="sxs-lookup"><span data-stu-id="08161-1831">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="08161-1832">如果生成的值是太大，无法表示在`decimal`格式，`System.OverflowException`引发。</span><span class="sxs-lookup"><span data-stu-id="08161-1832">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="08161-1833">如果结果值就太小，无法在表示`decimal`格式，则结果为零。</span><span class="sxs-lookup"><span data-stu-id="08161-1833">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="08161-1834">结果的小数位数是保留结果等于最小缩放接近的可表示的十进制值为 true 的数学运算结果。</span><span class="sxs-lookup"><span data-stu-id="08161-1834">The scale of the result is the smallest scale that will preserve a result equal to the nearest representable decimal value to the true mathematical result.</span></span>

   <span data-ttu-id="08161-1835">十进制除法相当于使用类型的除法运算符`System.Decimal`。</span><span class="sxs-lookup"><span data-stu-id="08161-1835">Decimal division is equivalent to using the division operator of type `System.Decimal`.</span></span>


### <a name="remainder-operator"></a><span data-ttu-id="08161-1836">余数运算符</span><span class="sxs-lookup"><span data-stu-id="08161-1836">Remainder operator</span></span>

<span data-ttu-id="08161-1837">有关操作的窗体`x % y`，二元运算符重载决策 ([二元运算符重载决策](expressions.md#binary-operator-overload-resolution)) 应用以选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="08161-1837">For an operation of the form `x % y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08161-1838">操作数都转换为所选运算符的参数类型和结果的类型是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1838">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="08161-1839">下面列出了预定义的余数运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-1839">The predefined remainder operators are listed below.</span></span> <span data-ttu-id="08161-1840">所有运算符将都计算之间除法的余数`x`和`y`。</span><span class="sxs-lookup"><span data-stu-id="08161-1840">The operators all compute the remainder of the division between `x` and `y`.</span></span>

*  <span data-ttu-id="08161-1841">整数余数：</span><span class="sxs-lookup"><span data-stu-id="08161-1841">Integer remainder:</span></span>

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   <span data-ttu-id="08161-1842">结果`x % y`由生成的值`x - (x / y) * y`。</span><span class="sxs-lookup"><span data-stu-id="08161-1842">The result of `x % y` is the value produced by `x - (x / y) * y`.</span></span> <span data-ttu-id="08161-1843">如果`y`为零，`System.DivideByZeroException`引发。</span><span class="sxs-lookup"><span data-stu-id="08161-1843">If `y` is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="08161-1844">如果左的操作数是最小`int`或`long`值和右操作数是`-1`、`System.OverflowException`引发。</span><span class="sxs-lookup"><span data-stu-id="08161-1844">If the left operand is the smallest `int` or `long` value and the right operand is `-1`, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="08161-1845">在任何情况下执行`x % y`引发异常位置`x / y`将不会引发异常。</span><span class="sxs-lookup"><span data-stu-id="08161-1845">In no case does `x % y` throw an exception where `x / y` would not throw an exception.</span></span>

*  <span data-ttu-id="08161-1846">浮点余数：</span><span class="sxs-lookup"><span data-stu-id="08161-1846">Floating-point remainder:</span></span>

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   <span data-ttu-id="08161-1847">下表列出的所有可能组合的非零的有限值、 零、 无穷大和 NaN 的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-1847">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="08161-1848">在表中，`x`和`y`是有限的正值。</span><span class="sxs-lookup"><span data-stu-id="08161-1848">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="08161-1849">`z` 是的结果`x % y`，并作为计算出`x - n * y`，其中`n`小于或等于最大可能整数`x / y`。</span><span class="sxs-lookup"><span data-stu-id="08161-1849">`z` is the result of `x % y` and is computed as `x - n * y`, where `n` is the largest possible integer that is less than or equal to `x / y`.</span></span> <span data-ttu-id="08161-1850">计算余数的此方法类似于用于整数操作数，但不同于 IEEE 754 定义 (在其中`n`是最接近的整数`x / y`)。</span><span class="sxs-lookup"><span data-stu-id="08161-1850">This method of computing the remainder is analogous to that used for integer operands, but differs from the IEEE 754 definition (in which `n` is the integer closest to `x / y`).</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="08161-1851">+y</span><span class="sxs-lookup"><span data-stu-id="08161-1851">+y</span></span>   | <span data-ttu-id="08161-1852">-y</span><span class="sxs-lookup"><span data-stu-id="08161-1852">-y</span></span>   | <span data-ttu-id="08161-1853">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1853">+0</span></span>   | <span data-ttu-id="08161-1854">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1854">-0</span></span>   | <span data-ttu-id="08161-1855">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1855">+inf</span></span> | <span data-ttu-id="08161-1856">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1856">-inf</span></span> | <span data-ttu-id="08161-1857">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1857">NaN</span></span>  | 
   | <span data-ttu-id="08161-1858">+x</span><span class="sxs-lookup"><span data-stu-id="08161-1858">+x</span></span>   | <span data-ttu-id="08161-1859">+z</span><span class="sxs-lookup"><span data-stu-id="08161-1859">+z</span></span>   | <span data-ttu-id="08161-1860">+z</span><span class="sxs-lookup"><span data-stu-id="08161-1860">+z</span></span>   | <span data-ttu-id="08161-1861">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1861">NaN</span></span>  | <span data-ttu-id="08161-1862">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1862">NaN</span></span>  | <span data-ttu-id="08161-1863">x</span><span class="sxs-lookup"><span data-stu-id="08161-1863">x</span></span>    | <span data-ttu-id="08161-1864">x</span><span class="sxs-lookup"><span data-stu-id="08161-1864">x</span></span>    | <span data-ttu-id="08161-1865">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1865">NaN</span></span>  | 
   | <span data-ttu-id="08161-1866">-x</span><span class="sxs-lookup"><span data-stu-id="08161-1866">-x</span></span>   | <span data-ttu-id="08161-1867">-z</span><span class="sxs-lookup"><span data-stu-id="08161-1867">-z</span></span>   | <span data-ttu-id="08161-1868">-z</span><span class="sxs-lookup"><span data-stu-id="08161-1868">-z</span></span>   | <span data-ttu-id="08161-1869">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1869">NaN</span></span>  | <span data-ttu-id="08161-1870">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1870">NaN</span></span>  | <span data-ttu-id="08161-1871">-x</span><span class="sxs-lookup"><span data-stu-id="08161-1871">-x</span></span>   | <span data-ttu-id="08161-1872">-x</span><span class="sxs-lookup"><span data-stu-id="08161-1872">-x</span></span>   | <span data-ttu-id="08161-1873">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1873">NaN</span></span>  | 
   | <span data-ttu-id="08161-1874">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1874">+0</span></span>   | <span data-ttu-id="08161-1875">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1875">+0</span></span>   | <span data-ttu-id="08161-1876">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1876">+0</span></span>   | <span data-ttu-id="08161-1877">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1877">NaN</span></span>  | <span data-ttu-id="08161-1878">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1878">NaN</span></span>  | <span data-ttu-id="08161-1879">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1879">+0</span></span>   | <span data-ttu-id="08161-1880">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1880">+0</span></span>   | <span data-ttu-id="08161-1881">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1881">NaN</span></span>  | 
   | <span data-ttu-id="08161-1882">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1882">-0</span></span>   | <span data-ttu-id="08161-1883">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1883">-0</span></span>   | <span data-ttu-id="08161-1884">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1884">-0</span></span>   | <span data-ttu-id="08161-1885">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1885">NaN</span></span>  | <span data-ttu-id="08161-1886">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1886">NaN</span></span>  | <span data-ttu-id="08161-1887">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1887">-0</span></span>   | <span data-ttu-id="08161-1888">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1888">-0</span></span>   | <span data-ttu-id="08161-1889">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1889">NaN</span></span>  | 
   | <span data-ttu-id="08161-1890">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1890">+inf</span></span> | <span data-ttu-id="08161-1891">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1891">NaN</span></span>  | <span data-ttu-id="08161-1892">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1892">NaN</span></span>  | <span data-ttu-id="08161-1893">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1893">NaN</span></span>  | <span data-ttu-id="08161-1894">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1894">NaN</span></span>  | <span data-ttu-id="08161-1895">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1895">NaN</span></span>  | <span data-ttu-id="08161-1896">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1896">NaN</span></span>  | <span data-ttu-id="08161-1897">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1897">NaN</span></span>  | 
   | <span data-ttu-id="08161-1898">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1898">-inf</span></span> | <span data-ttu-id="08161-1899">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1899">NaN</span></span>  | <span data-ttu-id="08161-1900">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1900">NaN</span></span>  | <span data-ttu-id="08161-1901">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1901">NaN</span></span>  | <span data-ttu-id="08161-1902">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1902">NaN</span></span>  | <span data-ttu-id="08161-1903">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1903">NaN</span></span>  | <span data-ttu-id="08161-1904">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1904">NaN</span></span>  | <span data-ttu-id="08161-1905">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1905">NaN</span></span>  | 
   | <span data-ttu-id="08161-1906">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1906">NaN</span></span>  | <span data-ttu-id="08161-1907">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1907">NaN</span></span>  | <span data-ttu-id="08161-1908">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1908">NaN</span></span>  | <span data-ttu-id="08161-1909">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1909">NaN</span></span>  | <span data-ttu-id="08161-1910">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1910">NaN</span></span>  | <span data-ttu-id="08161-1911">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1911">NaN</span></span>  | <span data-ttu-id="08161-1912">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1912">NaN</span></span>  | <span data-ttu-id="08161-1913">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1913">NaN</span></span>  | 

*  <span data-ttu-id="08161-1914">十进制的其余部分：</span><span class="sxs-lookup"><span data-stu-id="08161-1914">Decimal remainder:</span></span>

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   <span data-ttu-id="08161-1915">如果右操作数的值为零，`System.DivideByZeroException`引发。</span><span class="sxs-lookup"><span data-stu-id="08161-1915">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="08161-1916">结果，任何舍入之前, 的小数位数是较大的两个操作数，这些标度和运算的结果，如果非零，则将相同的是`x`。</span><span class="sxs-lookup"><span data-stu-id="08161-1916">The scale of the result, before any rounding, is the larger of the scales of the two operands, and the sign of the result, if non-zero, is the same as that of `x`.</span></span>

   <span data-ttu-id="08161-1917">十进制的其余部分相当于使用余数运算符的类型`System.Decimal`。</span><span class="sxs-lookup"><span data-stu-id="08161-1917">Decimal remainder is equivalent to using the remainder operator of type `System.Decimal`.</span></span>


### <a name="addition-operator"></a><span data-ttu-id="08161-1918">加法运算符</span><span class="sxs-lookup"><span data-stu-id="08161-1918">Addition operator</span></span>

<span data-ttu-id="08161-1919">有关操作的窗体`x + y`，二元运算符重载决策 ([二元运算符重载决策](expressions.md#binary-operator-overload-resolution)) 应用以选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="08161-1919">For an operation of the form `x + y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08161-1920">操作数都转换为所选运算符的参数类型和结果的类型是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="08161-1920">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="08161-1921">下面列出了预定义的加法运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-1921">The predefined addition operators are listed below.</span></span> <span data-ttu-id="08161-1922">对于数值和枚举类型预定义的加法运算符计算两个操作数之和。</span><span class="sxs-lookup"><span data-stu-id="08161-1922">For numeric and enumeration types, the predefined addition operators compute the sum of the two operands.</span></span> <span data-ttu-id="08161-1923">当一个或两个操作数都是字符串类型时，预定义的加法运算符串联操作数的字符串表示形式。</span><span class="sxs-lookup"><span data-stu-id="08161-1923">When one or both operands are of type string, the predefined addition operators concatenate the string representation of the operands.</span></span>

*  <span data-ttu-id="08161-1924">整数添加：</span><span class="sxs-lookup"><span data-stu-id="08161-1924">Integer addition:</span></span>

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   <span data-ttu-id="08161-1925">在中`checked`情况下，如果总和不在范围内的结果类型，`System.OverflowException`引发。</span><span class="sxs-lookup"><span data-stu-id="08161-1925">In a `checked` context, if the sum is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="08161-1926">在`unchecked`上下文，则不报告溢出和范围之外的结果类型的任何重大高顺序位将被丢弃。</span><span class="sxs-lookup"><span data-stu-id="08161-1926">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="08161-1927">浮点加法：</span><span class="sxs-lookup"><span data-stu-id="08161-1927">Floating-point addition:</span></span>

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   <span data-ttu-id="08161-1928">根据 IEEE 754 算法的规则计算总和。</span><span class="sxs-lookup"><span data-stu-id="08161-1928">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="08161-1929">下表列出的所有可能组合的非零的有限值、 零、 无穷大和 NaN 的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-1929">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="08161-1930">在表中，`x`并`y`为非零的有限值，并`z`结果的`x + y`。</span><span class="sxs-lookup"><span data-stu-id="08161-1930">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x + y`.</span></span> <span data-ttu-id="08161-1931">如果`x`并`y`具有相同的量值，但符号相反，`z`为正零。</span><span class="sxs-lookup"><span data-stu-id="08161-1931">If `x` and `y` have the same magnitude but opposite signs, `z` is positive zero.</span></span> <span data-ttu-id="08161-1932">如果`x + y`太大而无法在目标类型，表示`z`是相同的符号与无穷`x + y`。</span><span class="sxs-lookup"><span data-stu-id="08161-1932">If `x + y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x + y`.</span></span>

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="08161-1933">y</span><span class="sxs-lookup"><span data-stu-id="08161-1933">y</span></span>    | <span data-ttu-id="08161-1934">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1934">+0</span></span>   | <span data-ttu-id="08161-1935">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1935">-0</span></span>   | <span data-ttu-id="08161-1936">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1936">+inf</span></span> | <span data-ttu-id="08161-1937">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1937">-inf</span></span> | <span data-ttu-id="08161-1938">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1938">NaN</span></span>  | 
   | <span data-ttu-id="08161-1939">x</span><span class="sxs-lookup"><span data-stu-id="08161-1939">x</span></span>    | <span data-ttu-id="08161-1940">z</span><span class="sxs-lookup"><span data-stu-id="08161-1940">z</span></span>    | <span data-ttu-id="08161-1941">x</span><span class="sxs-lookup"><span data-stu-id="08161-1941">x</span></span>    | <span data-ttu-id="08161-1942">x</span><span class="sxs-lookup"><span data-stu-id="08161-1942">x</span></span>    | <span data-ttu-id="08161-1943">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1943">+inf</span></span> | <span data-ttu-id="08161-1944">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1944">-inf</span></span> | <span data-ttu-id="08161-1945">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1945">NaN</span></span>  | 
   | <span data-ttu-id="08161-1946">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1946">+0</span></span>   | <span data-ttu-id="08161-1947">y</span><span class="sxs-lookup"><span data-stu-id="08161-1947">y</span></span>    | <span data-ttu-id="08161-1948">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1948">+0</span></span>   | <span data-ttu-id="08161-1949">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1949">+0</span></span>   | <span data-ttu-id="08161-1950">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1950">+inf</span></span> | <span data-ttu-id="08161-1951">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1951">-inf</span></span> | <span data-ttu-id="08161-1952">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1952">NaN</span></span>  | 
   | <span data-ttu-id="08161-1953">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1953">-0</span></span>   | <span data-ttu-id="08161-1954">y</span><span class="sxs-lookup"><span data-stu-id="08161-1954">y</span></span>    | <span data-ttu-id="08161-1955">+0</span><span class="sxs-lookup"><span data-stu-id="08161-1955">+0</span></span>   | <span data-ttu-id="08161-1956">-0</span><span class="sxs-lookup"><span data-stu-id="08161-1956">-0</span></span>   | <span data-ttu-id="08161-1957">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1957">+inf</span></span> | <span data-ttu-id="08161-1958">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1958">-inf</span></span> | <span data-ttu-id="08161-1959">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1959">NaN</span></span>  | 
   | <span data-ttu-id="08161-1960">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1960">+inf</span></span> | <span data-ttu-id="08161-1961">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1961">+inf</span></span> | <span data-ttu-id="08161-1962">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1962">+inf</span></span> | <span data-ttu-id="08161-1963">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1963">+inf</span></span> | <span data-ttu-id="08161-1964">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-1964">+inf</span></span> | <span data-ttu-id="08161-1965">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1965">NaN</span></span>  | <span data-ttu-id="08161-1966">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1966">NaN</span></span>  | 
   | <span data-ttu-id="08161-1967">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1967">-inf</span></span> | <span data-ttu-id="08161-1968">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1968">-inf</span></span> | <span data-ttu-id="08161-1969">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1969">-inf</span></span> | <span data-ttu-id="08161-1970">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1970">-inf</span></span> | <span data-ttu-id="08161-1971">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1971">NaN</span></span>  | <span data-ttu-id="08161-1972">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-1972">-inf</span></span> | <span data-ttu-id="08161-1973">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1973">NaN</span></span>  | 
   | <span data-ttu-id="08161-1974">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1974">NaN</span></span>  | <span data-ttu-id="08161-1975">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1975">NaN</span></span>  | <span data-ttu-id="08161-1976">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1976">NaN</span></span>  | <span data-ttu-id="08161-1977">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1977">NaN</span></span>  | <span data-ttu-id="08161-1978">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1978">NaN</span></span>  | <span data-ttu-id="08161-1979">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1979">NaN</span></span>  | <span data-ttu-id="08161-1980">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-1980">NaN</span></span>  | 

*  <span data-ttu-id="08161-1981">十进制添加：</span><span class="sxs-lookup"><span data-stu-id="08161-1981">Decimal addition:</span></span>

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   <span data-ttu-id="08161-1982">如果生成的值是太大，无法表示在`decimal`格式，`System.OverflowException`引发。</span><span class="sxs-lookup"><span data-stu-id="08161-1982">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="08161-1983">结果，任何舍入之前, 的小数位数是较大的两个操作数的刻度。</span><span class="sxs-lookup"><span data-stu-id="08161-1983">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="08161-1984">十进制添加相当于使用加法运算符的类型`System.Decimal`。</span><span class="sxs-lookup"><span data-stu-id="08161-1984">Decimal addition is equivalent to using the addition operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="08161-1985">枚举加法。</span><span class="sxs-lookup"><span data-stu-id="08161-1985">Enumeration addition.</span></span> <span data-ttu-id="08161-1986">每个枚举类型都隐式提供以下预定义的运算符，其中`E`是枚举类型，并`U`是基础类型`E`:</span><span class="sxs-lookup"><span data-stu-id="08161-1986">Every enumeration type implicitly provides the following predefined operators, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   <span data-ttu-id="08161-1987">在运行时这些运算符的计算结果恰好`(E)((U)x + (U)y)`。</span><span class="sxs-lookup"><span data-stu-id="08161-1987">At run-time these operators are evaluated exactly as `(E)((U)x + (U)y)`.</span></span>

*  <span data-ttu-id="08161-1988">字符串串联：</span><span class="sxs-lookup"><span data-stu-id="08161-1988">String concatenation:</span></span>

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   <span data-ttu-id="08161-1989">该二进制文件的这些重载`+`运算符执行字符串串联。</span><span class="sxs-lookup"><span data-stu-id="08161-1989">These overloads of the binary `+` operator perform string concatenation.</span></span> <span data-ttu-id="08161-1990">如果操作数字符串串联为`null`，替换为空字符串。</span><span class="sxs-lookup"><span data-stu-id="08161-1990">If an operand of string concatenation is `null`, an empty string is substituted.</span></span> <span data-ttu-id="08161-1991">否则，任何非字符串参数转换为其字符串表示形式通过调用虚拟`ToString`方法继承自类型`object`。</span><span class="sxs-lookup"><span data-stu-id="08161-1991">Otherwise, any non-string argument is converted to its string representation by invoking the virtual `ToString` method inherited from type `object`.</span></span> <span data-ttu-id="08161-1992">如果`ToString`返回`null`，替换为空字符串。</span><span class="sxs-lookup"><span data-stu-id="08161-1992">If `ToString` returns `null`, an empty string is substituted.</span></span>

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

   字符串串联运算符的结果是一个左操作数的右操作数的字符后跟的字符组成的字符串。 字符串串联运算符永远不会返回`null`值。 <span data-ttu-id="08161-1995">一个`System.OutOfMemoryException`可能会引发，如果没有足够内存可用于分配生成的字符串。</span><span class="sxs-lookup"><span data-stu-id="08161-1995">A `System.OutOfMemoryException` may be thrown if there is not enough memory available to allocate the resulting string.</span></span>

*  <span data-ttu-id="08161-1996">委托组合。</span><span class="sxs-lookup"><span data-stu-id="08161-1996">Delegate combination.</span></span> <span data-ttu-id="08161-1997">每个委托类型都隐式提供以下预定义的运算符，其中`D`是委托类型：</span><span class="sxs-lookup"><span data-stu-id="08161-1997">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator +(D x, D y);
   ```

   <span data-ttu-id="08161-1998">二进制`+`运算符执行委托组合两个操作数属于特定的委托类型时`D`。</span><span class="sxs-lookup"><span data-stu-id="08161-1998">The binary `+` operator performs delegate combination when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="08161-1999">（如果操作数有不同的委托类型绑定时出错。）如果在第一个操作数`null`，该操作的结果是第二个操作数的值 (即使这也是`null`)。</span><span class="sxs-lookup"><span data-stu-id="08161-1999">(If the operands have different delegate types, a binding-time error occurs.) If the first operand is `null`, the result of the operation is the value of the second operand (even if that is also `null`).</span></span> <span data-ttu-id="08161-2000">否则为如果第二个操作数为`null`，则该操作的结果为第一个操作数的值。</span><span class="sxs-lookup"><span data-stu-id="08161-2000">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="08161-2001">否则，该操作的结果是一个新委托实例，该实例时调用，调用第一个操作数，然后调用第二个操作数。</span><span class="sxs-lookup"><span data-stu-id="08161-2001">Otherwise, the result of the operation is a new delegate instance that, when invoked, invokes the first operand and then invokes the second operand.</span></span> <span data-ttu-id="08161-2002">委托组合的示例，请参阅[减法运算符](expressions.md#subtraction-operator)并[委托调用](delegates.md#delegate-invocation)。</span><span class="sxs-lookup"><span data-stu-id="08161-2002">For examples of delegate combination, see [Subtraction operator](expressions.md#subtraction-operator) and [Delegate invocation](delegates.md#delegate-invocation).</span></span> <span data-ttu-id="08161-2003">由于`System.Delegate`不是委托类型， `operator`  `+`不为其定义。</span><span class="sxs-lookup"><span data-stu-id="08161-2003">Since `System.Delegate` is not a delegate type, `operator` `+` is not defined for it.</span></span>

### <a name="subtraction-operator"></a><span data-ttu-id="08161-2004">减法运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2004">Subtraction operator</span></span>

<span data-ttu-id="08161-2005">有关操作的窗体`x - y`，二元运算符重载决策 ([二元运算符重载决策](expressions.md#binary-operator-overload-resolution)) 应用以选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="08161-2005">For an operation of the form `x - y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08161-2006">操作数都转换为所选运算符的参数类型和结果的类型是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2006">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="08161-2007">下面列出了预定义的减法运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-2007">The predefined subtraction operators are listed below.</span></span> <span data-ttu-id="08161-2008">所有减的运算符`y`从`x`。</span><span class="sxs-lookup"><span data-stu-id="08161-2008">The operators all subtract `y` from `x`.</span></span>

*  <span data-ttu-id="08161-2009">整数减法运算：</span><span class="sxs-lookup"><span data-stu-id="08161-2009">Integer subtraction:</span></span>

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   <span data-ttu-id="08161-2010">在中`checked`情况下，如果结果类型的范围之外的区别是`System.OverflowException`引发。</span><span class="sxs-lookup"><span data-stu-id="08161-2010">In a `checked` context, if the difference is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="08161-2011">在`unchecked`上下文，则不报告溢出和范围之外的结果类型的任何重大高顺序位将被丢弃。</span><span class="sxs-lookup"><span data-stu-id="08161-2011">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="08161-2012">浮点减法运算：</span><span class="sxs-lookup"><span data-stu-id="08161-2012">Floating-point subtraction:</span></span>

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   <span data-ttu-id="08161-2013">根据 IEEE 754 算法的规则计算差。</span><span class="sxs-lookup"><span data-stu-id="08161-2013">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="08161-2014">下表列出所有可能组合的非零的有限值、 零、 无穷大和 Nan 的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-2014">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaNs.</span></span> <span data-ttu-id="08161-2015">在表中，`x`并`y`为非零的有限值，并`z`结果的`x - y`。</span><span class="sxs-lookup"><span data-stu-id="08161-2015">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x - y`.</span></span> <span data-ttu-id="08161-2016">如果`x`并`y`相等，`z`为正零。</span><span class="sxs-lookup"><span data-stu-id="08161-2016">If `x` and `y` are equal, `z` is positive zero.</span></span> <span data-ttu-id="08161-2017">如果`x - y`太大而无法在目标类型，表示`z`是相同的符号与无穷`x - y`。</span><span class="sxs-lookup"><span data-stu-id="08161-2017">If `x - y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x - y`.</span></span>

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   | <span data-ttu-id="08161-2018">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-2018">NaN</span></span>  | <span data-ttu-id="08161-2019">y</span><span class="sxs-lookup"><span data-stu-id="08161-2019">y</span></span>    | <span data-ttu-id="08161-2020">+0</span><span class="sxs-lookup"><span data-stu-id="08161-2020">+0</span></span>   | <span data-ttu-id="08161-2021">-0</span><span class="sxs-lookup"><span data-stu-id="08161-2021">-0</span></span>   | <span data-ttu-id="08161-2022">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-2022">+inf</span></span> | <span data-ttu-id="08161-2023">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-2023">-inf</span></span> | <span data-ttu-id="08161-2024">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-2024">NaN</span></span> | 
   | <span data-ttu-id="08161-2025">x</span><span class="sxs-lookup"><span data-stu-id="08161-2025">x</span></span>    | <span data-ttu-id="08161-2026">z</span><span class="sxs-lookup"><span data-stu-id="08161-2026">z</span></span>    | <span data-ttu-id="08161-2027">x</span><span class="sxs-lookup"><span data-stu-id="08161-2027">x</span></span>    | <span data-ttu-id="08161-2028">x</span><span class="sxs-lookup"><span data-stu-id="08161-2028">x</span></span>    | <span data-ttu-id="08161-2029">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-2029">-inf</span></span> | <span data-ttu-id="08161-2030">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-2030">+inf</span></span> | <span data-ttu-id="08161-2031">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-2031">NaN</span></span> | 
   | <span data-ttu-id="08161-2032">+0</span><span class="sxs-lookup"><span data-stu-id="08161-2032">+0</span></span>   | <span data-ttu-id="08161-2033">-y</span><span class="sxs-lookup"><span data-stu-id="08161-2033">-y</span></span>   | <span data-ttu-id="08161-2034">+0</span><span class="sxs-lookup"><span data-stu-id="08161-2034">+0</span></span>   | <span data-ttu-id="08161-2035">+0</span><span class="sxs-lookup"><span data-stu-id="08161-2035">+0</span></span>   | <span data-ttu-id="08161-2036">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-2036">-inf</span></span> | <span data-ttu-id="08161-2037">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-2037">+inf</span></span> | <span data-ttu-id="08161-2038">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-2038">NaN</span></span> | 
   | <span data-ttu-id="08161-2039">-0</span><span class="sxs-lookup"><span data-stu-id="08161-2039">-0</span></span>   | <span data-ttu-id="08161-2040">-y</span><span class="sxs-lookup"><span data-stu-id="08161-2040">-y</span></span>   | <span data-ttu-id="08161-2041">-0</span><span class="sxs-lookup"><span data-stu-id="08161-2041">-0</span></span>   | <span data-ttu-id="08161-2042">+0</span><span class="sxs-lookup"><span data-stu-id="08161-2042">+0</span></span>   | <span data-ttu-id="08161-2043">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-2043">-inf</span></span> | <span data-ttu-id="08161-2044">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-2044">+inf</span></span> | <span data-ttu-id="08161-2045">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-2045">NaN</span></span> | 
   | <span data-ttu-id="08161-2046">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-2046">+inf</span></span> | <span data-ttu-id="08161-2047">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-2047">+inf</span></span> | <span data-ttu-id="08161-2048">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-2048">+inf</span></span> | <span data-ttu-id="08161-2049">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-2049">+inf</span></span> | <span data-ttu-id="08161-2050">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-2050">NaN</span></span>  | <span data-ttu-id="08161-2051">+inf</span><span class="sxs-lookup"><span data-stu-id="08161-2051">+inf</span></span> | <span data-ttu-id="08161-2052">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-2052">NaN</span></span> | 
   | <span data-ttu-id="08161-2053">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-2053">-inf</span></span> | <span data-ttu-id="08161-2054">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-2054">-inf</span></span> | <span data-ttu-id="08161-2055">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-2055">-inf</span></span> | <span data-ttu-id="08161-2056">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-2056">-inf</span></span> | <span data-ttu-id="08161-2057">-inf</span><span class="sxs-lookup"><span data-stu-id="08161-2057">-inf</span></span> | <span data-ttu-id="08161-2058">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-2058">NaN</span></span>  | <span data-ttu-id="08161-2059">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-2059">NaN</span></span> | 
   | <span data-ttu-id="08161-2060">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-2060">NaN</span></span>  | <span data-ttu-id="08161-2061">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-2061">NaN</span></span>  | <span data-ttu-id="08161-2062">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-2062">NaN</span></span>  | <span data-ttu-id="08161-2063">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-2063">NaN</span></span>  | <span data-ttu-id="08161-2064">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-2064">NaN</span></span>  | <span data-ttu-id="08161-2065">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-2065">NaN</span></span>  | <span data-ttu-id="08161-2066">NaN</span><span class="sxs-lookup"><span data-stu-id="08161-2066">NaN</span></span> | 

*  <span data-ttu-id="08161-2067">十进制减法运算：</span><span class="sxs-lookup"><span data-stu-id="08161-2067">Decimal subtraction:</span></span>

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   <span data-ttu-id="08161-2068">如果生成的值是太大，无法表示在`decimal`格式，`System.OverflowException`引发。</span><span class="sxs-lookup"><span data-stu-id="08161-2068">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="08161-2069">结果，任何舍入之前, 的小数位数是较大的两个操作数的刻度。</span><span class="sxs-lookup"><span data-stu-id="08161-2069">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="08161-2070">十进制减法相当于使用类型的减法运算符`System.Decimal`。</span><span class="sxs-lookup"><span data-stu-id="08161-2070">Decimal subtraction is equivalent to using the subtraction operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="08161-2071">枚举减法。</span><span class="sxs-lookup"><span data-stu-id="08161-2071">Enumeration subtraction.</span></span> <span data-ttu-id="08161-2072">每个枚举类型都隐式提供以下预定义的运算符，其中`E`是枚举类型，并`U`是基础类型`E`:</span><span class="sxs-lookup"><span data-stu-id="08161-2072">Every enumeration type implicitly provides the following predefined operator, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   U operator -(E x, E y);
   ```

   <span data-ttu-id="08161-2073">此计算运算符的值严格按照`(U)((U)x - (U)y)`。</span><span class="sxs-lookup"><span data-stu-id="08161-2073">This operator is evaluated exactly as `(U)((U)x - (U)y)`.</span></span> <span data-ttu-id="08161-2074">换而言之，该运算符计算的序号值之间的差异`x`和`y`，并且结果的类型是枚举的基础类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2074">In other words, the operator computes the difference between the ordinal values of `x` and `y`, and the type of the result is the underlying type of the enumeration.</span></span>

   ```csharp
   E operator -(E x, U y);
   ```

   <span data-ttu-id="08161-2075">此计算运算符的值严格按照`(E)((U)x - y)`。</span><span class="sxs-lookup"><span data-stu-id="08161-2075">This operator is evaluated exactly as `(E)((U)x - y)`.</span></span> <span data-ttu-id="08161-2076">换而言之，运算符从减去的值的基础类型的枚举，生成的枚举值。</span><span class="sxs-lookup"><span data-stu-id="08161-2076">In other words, the operator subtracts a value from the underlying type of the enumeration, yielding a value of the enumeration.</span></span>

*  <span data-ttu-id="08161-2077">移除的委托。</span><span class="sxs-lookup"><span data-stu-id="08161-2077">Delegate removal.</span></span> <span data-ttu-id="08161-2078">每个委托类型都隐式提供以下预定义的运算符，其中`D`是委托类型：</span><span class="sxs-lookup"><span data-stu-id="08161-2078">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator -(D x, D y);
   ```

   <span data-ttu-id="08161-2079">二进制`-`运算符执行委托移除两个操作数属于特定的委托类型时`D`。</span><span class="sxs-lookup"><span data-stu-id="08161-2079">The binary `-` operator performs delegate removal when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="08161-2080">如果操作数有不同的委托类型绑定时出错。</span><span class="sxs-lookup"><span data-stu-id="08161-2080">If the operands have different delegate types, a binding-time error occurs.</span></span> <span data-ttu-id="08161-2081">如果在第一个操作数`null`，该操作的结果是`null`。</span><span class="sxs-lookup"><span data-stu-id="08161-2081">If the first operand is `null`, the result of the operation is `null`.</span></span> <span data-ttu-id="08161-2082">否则为如果第二个操作数为`null`，则该操作的结果为第一个操作数的值。</span><span class="sxs-lookup"><span data-stu-id="08161-2082">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="08161-2083">否则，两个操作数表示的调用列表 ([委托声明](delegates.md#delegate-declarations)) 具有一个或多个条目，并将该结果是第一个操作数的列表包括从删除的第二个操作数的条目具有一个新的调用列表它提供第二个操作数的列表是正确的连续子列表的第一个。</span><span class="sxs-lookup"><span data-stu-id="08161-2083">Otherwise, both operands represent invocation lists ([Delegate declarations](delegates.md#delegate-declarations)) having one or more entries, and the result is a new invocation list consisting of the first operand's list with the second operand's entries removed from it, provided the second operand's list is a proper contiguous sublist of the first's.</span></span>     <span data-ttu-id="08161-2084">(若要确定子列表相等，对应项进行比较委托是否相等运算符 ([委托相等运算符](expressions.md#delegate-equality-operators))。)否则，结果为左操作数的值。</span><span class="sxs-lookup"><span data-stu-id="08161-2084">(To determine sublist equality, corresponding entries are compared as for the delegate equality operator ([Delegate equality operators](expressions.md#delegate-equality-operators)).) Otherwise, the result is the value of the left operand.</span></span> <span data-ttu-id="08161-2085">在过程中更改任一操作数的列表。</span><span class="sxs-lookup"><span data-stu-id="08161-2085">Neither of the operands' lists is changed in the process.</span></span> <span data-ttu-id="08161-2086">如果第二个操作数的列表与连续列表中项的第一个操作数的多个子列表相匹配，则删除连续条目最右侧的匹配子列表。</span><span class="sxs-lookup"><span data-stu-id="08161-2086">If the second operand's list matches multiple sublists of contiguous entries in the first operand's list, the right-most matching sublist of contiguous entries is removed.</span></span> <span data-ttu-id="08161-2087">如果删除会导致空列表，则结果是`null`。</span><span class="sxs-lookup"><span data-stu-id="08161-2087">If removal results in an empty list, the result is `null`.</span></span> <span data-ttu-id="08161-2088">例如：</span><span class="sxs-lookup"><span data-stu-id="08161-2088">For example:</span></span>

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

## <a name="shift-operators"></a><span data-ttu-id="08161-2089">移位运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2089">Shift operators</span></span>

<span data-ttu-id="08161-2090">`<<`和`>>`运算符用于执行移位运算。</span><span class="sxs-lookup"><span data-stu-id="08161-2090">The `<<` and `>>` operators are used to perform bit shifting operations.</span></span>

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

<span data-ttu-id="08161-2091">如果操作数的*shift_expression*具有编译时类型`dynamic`，然后表达式动态绑定 ([动态绑定](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="08161-2091">If an operand of a *shift_expression* has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="08161-2092">在这种情况下该表达式的编译时类型是`dynamic`，如下所述的分辨率将发生在运行时使用这些具有编译时类型的操作数的运行时类型和`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="08161-2092">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="08161-2093">有关操作的窗体`x << count`或`x >> count`，二元运算符重载决策 ([二元运算符重载决策](expressions.md#binary-operator-overload-resolution)) 应用以选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="08161-2093">For an operation of the form `x << count` or `x >> count`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08161-2094">操作数都转换为所选运算符的参数类型和结果的类型是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2094">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="08161-2095">当声明重载的移位运算符时，第一个操作数的类型必须始终为类或结构包含运算符声明和第二个操作数的类型必须始终为`int`。</span><span class="sxs-lookup"><span data-stu-id="08161-2095">When declaring an overloaded shift operator, the type of the first operand must always be the class or struct containing the operator declaration, and the type of the second operand must always be `int`.</span></span>

<span data-ttu-id="08161-2096">下面列出了预定义的移位运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-2096">The predefined shift operators are listed below.</span></span>

*  <span data-ttu-id="08161-2097">左移位：</span><span class="sxs-lookup"><span data-stu-id="08161-2097">Shift left:</span></span>

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   <span data-ttu-id="08161-2098">`<<`运算符 shifts`x`左旋转比特数计算如下所述。</span><span class="sxs-lookup"><span data-stu-id="08161-2098">The `<<` operator shifts `x` left by a number of bits computed as described below.</span></span>

   <span data-ttu-id="08161-2099">结果类型的范围之外的高顺序位`x`会被丢弃，剩余的位向左和低序空位位置设置为零。</span><span class="sxs-lookup"><span data-stu-id="08161-2099">The high-order bits outside the range of the result type of `x` are discarded, the remaining bits are shifted left, and the low-order empty bit positions are set to zero.</span></span>

*  <span data-ttu-id="08161-2100">右移：</span><span class="sxs-lookup"><span data-stu-id="08161-2100">Shift right:</span></span>

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   <span data-ttu-id="08161-2101">`>>`运算符 shifts`x`右旋转比特数计算如下所述。</span><span class="sxs-lookup"><span data-stu-id="08161-2101">The `>>` operator shifts `x` right by a number of bits computed as described below.</span></span>

   <span data-ttu-id="08161-2102">时`x`属于类型`int`或`long`的低顺序位`x`会被丢弃，剩余的位向右，位移和高序空位位置设置为零`x`为非负，并且已设置一个用于在`x`为负。</span><span class="sxs-lookup"><span data-stu-id="08161-2102">When `x` is of type `int` or `long`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero if `x` is non-negative and set to one if `x` is negative.</span></span>

   <span data-ttu-id="08161-2103">当`x`属于类型`uint`或`ulong`的低顺序位`x`是被丢弃，剩余的位向右位移，和高序空位位置设置为零。</span><span class="sxs-lookup"><span data-stu-id="08161-2103">When `x` is of type `uint` or `ulong`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero.</span></span>

<span data-ttu-id="08161-2104">对于预定义的运算符，移动的位数，如下所示计算：</span><span class="sxs-lookup"><span data-stu-id="08161-2104">For the predefined operators, the number of bits to shift is computed as follows:</span></span>

*  <span data-ttu-id="08161-2105">时的类型`x`是`int`或`uint`，则移位计数由的低序五位给定`count`。</span><span class="sxs-lookup"><span data-stu-id="08161-2105">When the type of `x` is `int` or `uint`, the shift count is given by the low-order five bits of `count`.</span></span> <span data-ttu-id="08161-2106">换而言之，则移位计数计算得出`count & 0x1F`。</span><span class="sxs-lookup"><span data-stu-id="08161-2106">In other words, the shift count is computed from `count & 0x1F`.</span></span>
*  <span data-ttu-id="08161-2107">时的类型`x`是`long`或`ulong`，则移位计数由的低序六位给定`count`。</span><span class="sxs-lookup"><span data-stu-id="08161-2107">When the type of `x` is `long` or `ulong`, the shift count is given by the low-order six bits of `count`.</span></span> <span data-ttu-id="08161-2108">换而言之，则移位计数计算得出`count & 0x3F`。</span><span class="sxs-lookup"><span data-stu-id="08161-2108">In other words, the shift count is computed from `count & 0x3F`.</span></span>

<span data-ttu-id="08161-2109">如果在生成的 shift 计数为零，则移位运算符只需返回的值`x`。</span><span class="sxs-lookup"><span data-stu-id="08161-2109">If the resulting shift count is zero, the shift operators simply return the value of `x`.</span></span>

<span data-ttu-id="08161-2110">移位操作永远不会导致溢出并产生相同的结果中`checked`和`unchecked`上下文。</span><span class="sxs-lookup"><span data-stu-id="08161-2110">Shift operations never cause overflows and produce the same results in `checked` and `unchecked` contexts.</span></span>

<span data-ttu-id="08161-2111">当左的操作数的`>>`运算符为带符号整型类型时，运算符执行算术右操作数的最高有效位 （符号位） 的值将传播其中到高序空位位置。</span><span class="sxs-lookup"><span data-stu-id="08161-2111">When the left operand of the `>>` operator is of a signed integral type, the operator performs an arithmetic shift right wherein the value of the most significant bit (the sign bit) of the operand is propagated to the high-order empty bit positions.</span></span> <span data-ttu-id="08161-2112">当左的操作数的`>>`运算符是无符号整数类型，该运算符执行逻辑右其中高序空位位置始终设置为零。</span><span class="sxs-lookup"><span data-stu-id="08161-2112">When the left operand of the `>>` operator is of an unsigned integral type, the operator performs a logical shift right wherein high-order empty bit positions are always set to zero.</span></span> <span data-ttu-id="08161-2113">若要执行的操作数类型推断的相反的操作，可以使用显式强制转换。</span><span class="sxs-lookup"><span data-stu-id="08161-2113">To perform the opposite operation of that inferred from the operand type, explicit casts can be used.</span></span> <span data-ttu-id="08161-2114">例如，如果`x`类型的变量`int`，该操作`unchecked((int)((uint)x >> y))`执行的一种逻辑移位`x`。</span><span class="sxs-lookup"><span data-stu-id="08161-2114">For example, if `x` is a variable of type `int`, the operation `unchecked((int)((uint)x >> y))` performs a logical shift right of `x`.</span></span>

## <a name="relational-and-type-testing-operators"></a><span data-ttu-id="08161-2115">关系和类型测试运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2115">Relational and type-testing operators</span></span>

<span data-ttu-id="08161-2116">`==`， `!=`， `<`， `>`， `<=`， `>=`，`is`和`as`运算符被称作关系和类型测试运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-2116">The `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` and `as` operators are called the relational and type-testing operators.</span></span>

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

<span data-ttu-id="08161-2117">`is`运算符中所述[是运算符](expressions.md#the-is-operator)并且`as`运算符中所述[As 运算符](expressions.md#the-as-operator)。</span><span class="sxs-lookup"><span data-stu-id="08161-2117">The `is` operator is described in [The is operator](expressions.md#the-is-operator) and the `as` operator is described in [The as operator](expressions.md#the-as-operator).</span></span>

<span data-ttu-id="08161-2118">`==`， `!=`， `<`， `>`，`<=`并`>=`运算符是***比较运算符***。</span><span class="sxs-lookup"><span data-stu-id="08161-2118">The `==`, `!=`, `<`, `>`, `<=` and `>=` operators are ***comparison operators***.</span></span>

<span data-ttu-id="08161-2119">如果比较运算符的操作数具有编译时类型`dynamic`，然后动态绑定表达式 ([动态绑定](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="08161-2119">If an operand of a comparison operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="08161-2120">在这种情况下该表达式的编译时类型是`dynamic`，如下所述的分辨率将发生在运行时使用这些具有编译时类型的操作数的运行时类型和`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="08161-2120">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="08161-2121">有关操作的窗体`x` *op* `y`，其中*op*是一个比较运算符，重载决策 ([二元运算符重载决策](expressions.md#binary-operator-overload-resolution)) 用于选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="08161-2121">For an operation of the form `x` *op* `y`, where *op* is a comparison operator, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08161-2122">操作数都转换为所选运算符的参数类型和结果的类型是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2122">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="08161-2123">以下各节描述了预定义的比较运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-2123">The predefined comparison operators are described in the following sections.</span></span> <span data-ttu-id="08161-2124">所有预定义的比较运算符将返回类型的结果`bool`下, 表中所述。</span><span class="sxs-lookup"><span data-stu-id="08161-2124">All predefined comparison operators return a result of type `bool`, as described in the following table.</span></span>


| <span data-ttu-id="08161-2125">__Operation__</span><span class="sxs-lookup"><span data-stu-id="08161-2125">__Operation__</span></span> | <span data-ttu-id="08161-2126">__结果__</span><span class="sxs-lookup"><span data-stu-id="08161-2126">__Result__</span></span>                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | <span data-ttu-id="08161-2127">`true` 如果`x`等同于`y`，`false`否则为</span><span class="sxs-lookup"><span data-stu-id="08161-2127">`true` if `x` is equal to `y`, `false` otherwise</span></span>                 | 
| `x != y`      | <span data-ttu-id="08161-2128">`true` 如果`x`不等于`y`，`false`否则为</span><span class="sxs-lookup"><span data-stu-id="08161-2128">`true` if `x` is not equal to `y`, `false` otherwise</span></span>             | 
| `x < y`       | <span data-ttu-id="08161-2129">`true` 如果`x`是小于`y`，`false`否则为</span><span class="sxs-lookup"><span data-stu-id="08161-2129">`true` if `x` is less than `y`, `false` otherwise</span></span>                | 
| `x > y`       | <span data-ttu-id="08161-2130">`true` 如果`x`大于`y`，`false`否则为</span><span class="sxs-lookup"><span data-stu-id="08161-2130">`true` if `x` is greater than `y`, `false` otherwise</span></span>             | 
| `x <= y`      | <span data-ttu-id="08161-2131">`true` 如果`x`小于或等于`y`，`false`否则为</span><span class="sxs-lookup"><span data-stu-id="08161-2131">`true` if `x` is less than or equal to `y`, `false` otherwise</span></span>    | 
| `x >= y`      | <span data-ttu-id="08161-2132">`true` 如果`x`大于或等于`y`，`false`否则为</span><span class="sxs-lookup"><span data-stu-id="08161-2132">`true` if `x` is greater than or equal to `y`, `false` otherwise</span></span> | 

### <a name="integer-comparison-operators"></a><span data-ttu-id="08161-2133">整数比较运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2133">Integer comparison operators</span></span>

<span data-ttu-id="08161-2134">预定义的整数比较运算符为：</span><span class="sxs-lookup"><span data-stu-id="08161-2134">The predefined integer comparison operators are:</span></span>
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

<span data-ttu-id="08161-2135">每个运算符进行比较的两个整数操作数和返回数值`bool`值，该值指示是否将特定关系`true`或`false`。</span><span class="sxs-lookup"><span data-stu-id="08161-2135">Each of these operators compares the numeric values of the two integer operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span>

### <a name="floating-point-comparison-operators"></a><span data-ttu-id="08161-2136">浮点比较运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2136">Floating-point comparison operators</span></span>

<span data-ttu-id="08161-2137">预定义的浮点比较运算符为：</span><span class="sxs-lookup"><span data-stu-id="08161-2137">The predefined floating-point comparison operators are:</span></span>
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

<span data-ttu-id="08161-2138">这些运算符来比较根据 IEEE 754 标准规则操作数：</span><span class="sxs-lookup"><span data-stu-id="08161-2138">The operators compare the operands according to the rules of the IEEE 754 standard:</span></span>

*  <span data-ttu-id="08161-2139">如果其中一个操作数为 NaN，则结果是`false`的所有运算符除外`!=`，则结果为`true`。</span><span class="sxs-lookup"><span data-stu-id="08161-2139">If either operand is NaN, the result is `false` for all operators except `!=`, for which the result is `true`.</span></span> <span data-ttu-id="08161-2140">对于任何两个操作数，`x != y`始终生成相同的结果`!(x == y)`。</span><span class="sxs-lookup"><span data-stu-id="08161-2140">For any two operands, `x != y` always produces the same result as `!(x == y)`.</span></span> <span data-ttu-id="08161-2141">然而，当一个或两个操作数为 NaN `<`， `>`， `<=`，和`>=`运算符不产生与逻辑求反的相反运算符相同的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-2141">However, when one or both operands are NaN, the `<`, `>`, `<=`, and `>=` operators do not produce the same results as the logical negation of the opposite operator.</span></span> <span data-ttu-id="08161-2142">例如，如果任一的`x`并`y`为 NaN，则`x < y`是`false`，但`!(x >= y)`是`true`。</span><span class="sxs-lookup"><span data-stu-id="08161-2142">For example, if either of `x` and `y` is NaN, then `x < y` is `false`, but `!(x >= y)` is `true`.</span></span>
*  <span data-ttu-id="08161-2143">当两个操作数为 NaN 时，这些运算符比较两个浮点操作数与顺序相关的值</span><span class="sxs-lookup"><span data-stu-id="08161-2143">When neither operand is NaN, the operators compare the values of the two floating-point operands with respect to the ordering</span></span>

   ```
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   <span data-ttu-id="08161-2144">其中`min`和`max`是最小和最大正有限值可以以给定的浮点格式表示。</span><span class="sxs-lookup"><span data-stu-id="08161-2144">where `min` and `max` are the smallest and largest positive finite values that can be represented in the given floating-point format.</span></span> <span data-ttu-id="08161-2145">值得注意的这一顺序的影响是：</span><span class="sxs-lookup"><span data-stu-id="08161-2145">Notable effects of this ordering are:</span></span>
   * <span data-ttu-id="08161-2146">负数和正数零被视为相等。</span><span class="sxs-lookup"><span data-stu-id="08161-2146">Negative and positive zeros are considered equal.</span></span>
   * <span data-ttu-id="08161-2147">负无穷大被视为小于所有其他值，但等于另一个负无穷大。</span><span class="sxs-lookup"><span data-stu-id="08161-2147">A negative infinity is considered less than all other values, but equal to another negative infinity.</span></span>
   * <span data-ttu-id="08161-2148">正无穷大被视为大于所有其他值，但等于另一个正无穷大。</span><span class="sxs-lookup"><span data-stu-id="08161-2148">A positive infinity is considered greater than all other values, but equal to another positive infinity.</span></span>

### <a name="decimal-comparison-operators"></a><span data-ttu-id="08161-2149">十进制比较运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2149">Decimal comparison operators</span></span>

<span data-ttu-id="08161-2150">预定义的十进制比较运算符为：</span><span class="sxs-lookup"><span data-stu-id="08161-2150">The predefined decimal comparison operators are:</span></span>
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

<span data-ttu-id="08161-2151">每个运算符进行比较的两个十进制操作数和返回数值`bool`值，该值指示是否将特定关系`true`或`false`。</span><span class="sxs-lookup"><span data-stu-id="08161-2151">Each of these operators compares the numeric values of the two decimal operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span> <span data-ttu-id="08161-2152">每个十进制比较相当于使用的对应关系或类型的相等运算符`System.Decimal`。</span><span class="sxs-lookup"><span data-stu-id="08161-2152">Each decimal comparison is equivalent to using the corresponding relational or equality operator of type `System.Decimal`.</span></span>

### <a name="boolean-equality-operators"></a><span data-ttu-id="08161-2153">布尔值的相等运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2153">Boolean equality operators</span></span>

<span data-ttu-id="08161-2154">预定义的布尔相等运算符为：</span><span class="sxs-lookup"><span data-stu-id="08161-2154">The predefined boolean equality operators are:</span></span>
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

<span data-ttu-id="08161-2155">结果`==`是`true`如果这两种`x`并`y`是`true`或如果这两个`x`并`y`是`false`。</span><span class="sxs-lookup"><span data-stu-id="08161-2155">The result of `==` is `true` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="08161-2156">否则，结果为 `false`。</span><span class="sxs-lookup"><span data-stu-id="08161-2156">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="08161-2157">结果`!=`是`false`如果这两种`x`并`y`是`true`或如果这两个`x`并`y`是`false`。</span><span class="sxs-lookup"><span data-stu-id="08161-2157">The result of `!=` is `false` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="08161-2158">否则，结果为 `true`。</span><span class="sxs-lookup"><span data-stu-id="08161-2158">Otherwise, the result is `true`.</span></span> <span data-ttu-id="08161-2159">如果操作数均为类型`bool`，则`!=`运算符会产生相同的结果`^`运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-2159">When the operands are of type `bool`, the `!=` operator produces the same result as the `^` operator.</span></span>

### <a name="enumeration-comparison-operators"></a><span data-ttu-id="08161-2160">枚举的比较运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2160">Enumeration comparison operators</span></span>

<span data-ttu-id="08161-2161">每个枚举类型隐式提供了以下预定义的比较运算符：</span><span class="sxs-lookup"><span data-stu-id="08161-2161">Every enumeration type implicitly provides the following predefined comparison operators:</span></span>
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

<span data-ttu-id="08161-2162">计算结果`x op y`，其中`x`并`y`是枚举类型的表达式`E`与基础类型`U`，并`op`为其中一个比较运算符，为完全相同评估`((U)x) op ((U)y)`。</span><span class="sxs-lookup"><span data-stu-id="08161-2162">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the comparison operators, is exactly the same as evaluating `((U)x) op ((U)y)`.</span></span> <span data-ttu-id="08161-2163">换而言之，枚举类型比较运算符只需比较两个操作数的基础整数值。</span><span class="sxs-lookup"><span data-stu-id="08161-2163">In other words, the enumeration type comparison operators simply compare the underlying integral values of the two operands.</span></span>

### <a name="reference-type-equality-operators"></a><span data-ttu-id="08161-2164">引用类型相等运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2164">Reference type equality operators</span></span>

<span data-ttu-id="08161-2165">预定义的引用类型的相等运算符为：</span><span class="sxs-lookup"><span data-stu-id="08161-2165">The predefined reference type equality operators are:</span></span>
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

<span data-ttu-id="08161-2166">运算符将返回比较相等或不相等的两个引用的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-2166">The operators return the result of comparing the two references for equality or non-equality.</span></span>

<span data-ttu-id="08161-2167">由于预定义的引用类型的相等运算符接受类型的操作数`object`，它们将应用于所有类型的未声明适用`operator ==`和`operator !=`成员。</span><span class="sxs-lookup"><span data-stu-id="08161-2167">Since the predefined reference type equality operators accept operands of type `object`, they apply to all types that do not declare applicable `operator ==` and `operator !=` members.</span></span> <span data-ttu-id="08161-2168">相反，任何适用的用户定义的相等运算符有效地隐藏预定义的引用类型的相等运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-2168">Conversely, any applicable user-defined equality operators effectively hide the predefined reference type equality operators.</span></span>

<span data-ttu-id="08161-2169">预定义的引用类型的相等运算符需要以下项之一：</span><span class="sxs-lookup"><span data-stu-id="08161-2169">The predefined reference type equality operators require one of the following:</span></span>

*  <span data-ttu-id="08161-2170">这两个操作数都是已知类型的值*reference_type*或文本`null`。</span><span class="sxs-lookup"><span data-stu-id="08161-2170">Both operands are a value of a type known to be a *reference_type* or the literal `null`.</span></span> <span data-ttu-id="08161-2171">此外，显式引用转换 ([显式引用转换](conversions.md#explicit-reference-conversions)) 存在从其中一个操作数的类型到另一个操作数的类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2171">Furthermore, an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from the type of either operand to the type of the other operand.</span></span>
*  <span data-ttu-id="08161-2172">一个操作数的值为类型`T`其中`T`是*type_parameter*和另一个操作数为字面值`null`。</span><span class="sxs-lookup"><span data-stu-id="08161-2172">One operand is a value of type `T` where `T` is a *type_parameter* and the other operand is the literal `null`.</span></span> <span data-ttu-id="08161-2173">此外`T`不具有值类型约束。</span><span class="sxs-lookup"><span data-stu-id="08161-2173">Furthermore `T` does not have the value type constraint.</span></span>

<span data-ttu-id="08161-2174">除非以下条件之一为真，则绑定时间发生错误。</span><span class="sxs-lookup"><span data-stu-id="08161-2174">Unless one of these conditions are true, a binding-time error occurs.</span></span> <span data-ttu-id="08161-2175">值得注意这些规则的含义是：</span><span class="sxs-lookup"><span data-stu-id="08161-2175">Notable implications of these rules are:</span></span>

*  <span data-ttu-id="08161-2176">它是使用预定义的引用类型相等运算符比较两个已知可在绑定时不同的引用的绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-2176">It is a binding-time error to use the predefined reference type equality operators to compare two references that are known to be different at binding-time.</span></span> <span data-ttu-id="08161-2177">例如，如果绑定时间类型的操作数是两个类类型`A`并`B`，和如果既没有`A`也不`B`派生另一个，然后它就不可能的两个操作数引用同一对象。</span><span class="sxs-lookup"><span data-stu-id="08161-2177">For example, if the binding-time types of the operands are two class types `A` and `B`, and if neither `A` nor `B` derives from the other, then it would be impossible for the two operands to reference the same object.</span></span> <span data-ttu-id="08161-2178">因此，该操作被视为绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-2178">Thus, the operation is considered a binding-time error.</span></span>
*  <span data-ttu-id="08161-2179">预定义的引用类型的相等运算符不允许值类型操作数进行比较。</span><span class="sxs-lookup"><span data-stu-id="08161-2179">The predefined reference type equality operators do not permit value type operands to be compared.</span></span> <span data-ttu-id="08161-2180">因此，除非将结构类型声明其自己的相等运算符，不能对该结构类型的值进行比较。</span><span class="sxs-lookup"><span data-stu-id="08161-2180">Therefore, unless a struct type declares its own equality operators, it is not possible to compare values of that struct type.</span></span>
*  <span data-ttu-id="08161-2181">预定义的引用类型的相等运算符永远不会导致对其操作数执行装箱操作。</span><span class="sxs-lookup"><span data-stu-id="08161-2181">The predefined reference type equality operators never cause boxing operations to occur for their operands.</span></span> <span data-ttu-id="08161-2182">将无意义，若要执行此类装箱操作，因为对新分配的装箱实例必将区别于其他所有引用。</span><span class="sxs-lookup"><span data-stu-id="08161-2182">It would be meaningless to perform such boxing operations, since references to the newly allocated boxed instances would necessarily differ from all other references.</span></span>
*  <span data-ttu-id="08161-2183">如果类型参数类型的操作数`T`进行比较的`null`，和的运行时类型`T`是值类型，比较的结果是`false`。</span><span class="sxs-lookup"><span data-stu-id="08161-2183">If an operand of a type parameter type `T` is compared to `null`, and the run-time type of `T` is a value type, the result of the comparison is `false`.</span></span>

<span data-ttu-id="08161-2184">下面的示例检查不受约束的类型参数类型的自变量是否`null`。</span><span class="sxs-lookup"><span data-stu-id="08161-2184">The following example checks whether an argument of an unconstrained type parameter type is `null`.</span></span>
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

<span data-ttu-id="08161-2185">`x == null`即使允许构造`T`可表示值类型，且结果只需定义`false`时`T`是值类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2185">The `x == null` construct is permitted even though `T` could represent a value type, and the result is simply defined to be `false` when `T` is a value type.</span></span>

<span data-ttu-id="08161-2186">有关操作的窗体`x == y`或`x != y`，如果任何适用`operator ==`或`operator !=`存在，运算符重载解析 ([二元运算符重载决策](expressions.md#binary-operator-overload-resolution)) 规则将选择的运算符而不是预定义的引用类型的相等运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-2186">For an operation of the form `x == y` or `x != y`, if any applicable `operator ==` or `operator !=` exists, the operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) rules will select that operator instead of the predefined reference type equality operator.</span></span> <span data-ttu-id="08161-2187">但是，则始终可以选择预定义的引用类型的相等运算符通过一个或两个操作数类型显式转换`object`。</span><span class="sxs-lookup"><span data-stu-id="08161-2187">However, it is always possible to select the predefined reference type equality operator by explicitly casting one or both of the operands to type `object`.</span></span> <span data-ttu-id="08161-2188">该示例</span><span class="sxs-lookup"><span data-stu-id="08161-2188">The example</span></span>
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
<span data-ttu-id="08161-2189">生成输出</span><span class="sxs-lookup"><span data-stu-id="08161-2189">produces the output</span></span>
```
True
False
False
False
```

<span data-ttu-id="08161-2190">`s`并`t`变量引用两个不同`string`实例包含相同字符。</span><span class="sxs-lookup"><span data-stu-id="08161-2190">The `s` and `t` variables refer to two distinct `string` instances containing the same characters.</span></span> <span data-ttu-id="08161-2191">第一次比较输出`True`因为预定义的字符串相等运算符 ([字符串是否相等运算符](expressions.md#string-equality-operators)) 时这两个操作数均为类型选择`string`。</span><span class="sxs-lookup"><span data-stu-id="08161-2191">The first comparison outputs `True` because the predefined string equality operator ([String equality operators](expressions.md#string-equality-operators)) is selected when both operands are of type `string`.</span></span> <span data-ttu-id="08161-2192">输出所有其余的比较`False`因为一个或两个操作数属于类型时选择预定义的引用类型的相等运算符`object`。</span><span class="sxs-lookup"><span data-stu-id="08161-2192">The remaining comparisons all output `False` because the predefined reference type equality operator is selected when one or both of the operands are of type `object`.</span></span>

<span data-ttu-id="08161-2193">请注意，上面的技术不是对值类型有意义。</span><span class="sxs-lookup"><span data-stu-id="08161-2193">Note that the above technique is not meaningful for value types.</span></span> <span data-ttu-id="08161-2194">该示例</span><span class="sxs-lookup"><span data-stu-id="08161-2194">The example</span></span>
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
<span data-ttu-id="08161-2195">输出`False`由于强制转换创建的两个单独实例的引用，因此装箱`int`值。</span><span class="sxs-lookup"><span data-stu-id="08161-2195">outputs `False` because the casts create references to two separate instances of boxed `int` values.</span></span>

### <a name="string-equality-operators"></a><span data-ttu-id="08161-2196">字符串相等运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2196">String equality operators</span></span>

<span data-ttu-id="08161-2197">预定义的字符串相等运算符为：</span><span class="sxs-lookup"><span data-stu-id="08161-2197">The predefined string equality operators are:</span></span>
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

<span data-ttu-id="08161-2198">两个`string`值被视为相等，以下项之一为 true 时：</span><span class="sxs-lookup"><span data-stu-id="08161-2198">Two `string` values are considered equal when one of the following is true:</span></span>

*  <span data-ttu-id="08161-2199">两个值均`null`。</span><span class="sxs-lookup"><span data-stu-id="08161-2199">Both values are `null`.</span></span>
*  <span data-ttu-id="08161-2200">这两个值是对每个字符位置中具有相同长度和字符的字符串实例的非 null 引用。</span><span class="sxs-lookup"><span data-stu-id="08161-2200">Both values are non-null references to string instances that have identical lengths and identical characters in each character position.</span></span>

<span data-ttu-id="08161-2201">字符串相等运算符比较字符串值而不是字符串的引用。</span><span class="sxs-lookup"><span data-stu-id="08161-2201">The string equality operators compare string values rather than string references.</span></span> <span data-ttu-id="08161-2202">当两个单独的字符串实例包含完全相同的字符序列时，字符串的值相等，但不同的引用。</span><span class="sxs-lookup"><span data-stu-id="08161-2202">When two separate string instances contain the exact same sequence of characters, the values of the strings are equal, but the references are different.</span></span> <span data-ttu-id="08161-2203">如中所述[引用类型相等运算符](expressions.md#reference-type-equality-operators)，引用类型的相等运算符可用于比较字符串引用而不是字符串值。</span><span class="sxs-lookup"><span data-stu-id="08161-2203">As described in [Reference type equality operators](expressions.md#reference-type-equality-operators), the reference type equality operators can be used to compare string references instead of string values.</span></span>

### <a name="delegate-equality-operators"></a><span data-ttu-id="08161-2204">委托相等运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2204">Delegate equality operators</span></span>

<span data-ttu-id="08161-2205">每个委托类型隐式提供了以下预定义的比较运算符：</span><span class="sxs-lookup"><span data-stu-id="08161-2205">Every delegate type implicitly provides the following predefined comparison operators:</span></span>

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

<span data-ttu-id="08161-2206">两个委托实例视为相等，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-2206">Two delegate instances are considered equal as follows:</span></span>

*  <span data-ttu-id="08161-2207">如果委托实例之一`null`，它们是否相等的当且仅当两者都是`null`。</span><span class="sxs-lookup"><span data-stu-id="08161-2207">If either of the delegate instances is `null`, they are equal if and only if both are `null`.</span></span>
*  <span data-ttu-id="08161-2208">如果委托具有不同的运行时类型为永远不会相等。</span><span class="sxs-lookup"><span data-stu-id="08161-2208">If the delegates have different run-time type they are never equal.</span></span>
*  <span data-ttu-id="08161-2209">如果这两个委托实例必须调用列表 ([委托声明](delegates.md#delegate-declarations))，当且仅当它们的调用列表相同的长度，并且其中一个的调用列表中每个条目是相等 （如定义如下），这些实例是否相等到顺序，请在对方的调用列表中的相应条目。</span><span class="sxs-lookup"><span data-stu-id="08161-2209">If both of the delegate instances have an invocation list ([Delegate declarations](delegates.md#delegate-declarations)), those instances are equal if and only if their invocation lists are the same length, and each entry in one's invocation list is equal (as defined below) to the corresponding entry, in order, in the other's invocation list.</span></span>

<span data-ttu-id="08161-2210">以下规则控制调用列表项的相等的性：</span><span class="sxs-lookup"><span data-stu-id="08161-2210">The following rules govern the equality of invocation list entries:</span></span>

*  <span data-ttu-id="08161-2211">如果两个调用列表条目这两个都指向相同的静态方法然后条目相等。</span><span class="sxs-lookup"><span data-stu-id="08161-2211">If two invocation list entries both refer to the same static method then the entries are equal.</span></span>
*  <span data-ttu-id="08161-2212">如果两个调用列表条目这两个都指向相同的目标对象上的同一个非静态方法 （如引用相等运算符所定义） 条目相等。</span><span class="sxs-lookup"><span data-stu-id="08161-2212">If two invocation list entries both refer to the same non-static method on the same target object (as defined by the reference equality operators) then the entries are equal.</span></span>
*  <span data-ttu-id="08161-2213">从评估版在语义上完全相同的调用列表项生成*anonymous_method_expression*s 或*lambda_expression*与 （可能为空） 一组相同的捕获的外部变量允许 （但不是必需的） 实例相等。</span><span class="sxs-lookup"><span data-stu-id="08161-2213">Invocation list entries produced from evaluation of semantically identical *anonymous_method_expression*s or *lambda_expression*s with the same (possibly empty) set of captured outer variable instances are permitted (but not required) to be equal.</span></span>

### <a name="equality-operators-and-null"></a><span data-ttu-id="08161-2214">相等运算符和 null</span><span class="sxs-lookup"><span data-stu-id="08161-2214">Equality operators and null</span></span>

<span data-ttu-id="08161-2215">`==`并`!=`运算符允许一个操作数是值为 null 的类型和其他要`null`文字，即使没有预定义或用户定义运算符 （在未提升或提升窗体） 存在的操作。</span><span class="sxs-lookup"><span data-stu-id="08161-2215">The `==` and `!=` operators permit one operand to be a value of a nullable type and the other to be the `null` literal, even if no predefined or user-defined operator (in unlifted or lifted form) exists for the operation.</span></span>

<span data-ttu-id="08161-2216">对于使用某种形式的操作</span><span class="sxs-lookup"><span data-stu-id="08161-2216">For an operation of one of the forms</span></span>
```csharp
x == null
null == x
x != null
null != x
```
<span data-ttu-id="08161-2217">其中`x`运算符重载决策时为 null 的类型的表达式 ([二元运算符重载决策](expressions.md#binary-operator-overload-resolution)) 无法找到适用的运算符，结果而是计算得出`HasValue`属性的`x`。</span><span class="sxs-lookup"><span data-stu-id="08161-2217">where `x` is an expression of a nullable type, if operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) fails to find an applicable operator, the result is instead computed from the `HasValue` property of `x`.</span></span> <span data-ttu-id="08161-2218">具体而言前, 两个窗体将转换成`!x.HasValue`，并最后两个窗体转换为`x.HasValue`。</span><span class="sxs-lookup"><span data-stu-id="08161-2218">Specifically, the first two forms are translated into `!x.HasValue`, and last two forms are translated into `x.HasValue`.</span></span>

### <a name="the-is-operator"></a><span data-ttu-id="08161-2219">Is 运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2219">The is operator</span></span>

<span data-ttu-id="08161-2220">`is`运算符用于动态检查对象的运行时类型是否与给定类型兼容。</span><span class="sxs-lookup"><span data-stu-id="08161-2220">The `is` operator is used to dynamically check if the run-time type of an object is compatible with a given type.</span></span> <span data-ttu-id="08161-2221">操作的结果`E is T`，其中`E`是一个表达式和`T`是一种类型，是一个布尔值，该值指示是否`E`可以成功转换为类型`T`通过引用转换、 装箱转换或取消装箱转换。</span><span class="sxs-lookup"><span data-stu-id="08161-2221">The result of the operation `E is T`, where `E` is an expression and `T` is a type, is a boolean value indicating whether `E` can successfully be converted to type `T` by a reference conversion, a boxing conversion, or an unboxing conversion.</span></span> <span data-ttu-id="08161-2222">类型参数都已替换为所有类型参数后，将按如下所示，计算该操作：</span><span class="sxs-lookup"><span data-stu-id="08161-2222">The operation is evaluated as follows, after type arguments have been substituted for all type parameters:</span></span>

*  <span data-ttu-id="08161-2223">如果`E`是一个匿名函数，则发生编译时错误</span><span class="sxs-lookup"><span data-stu-id="08161-2223">If `E` is an anonymous function, a compile-time error occurs</span></span>
*  <span data-ttu-id="08161-2224">如果`E`是方法组或`null`文字，如果类型`E`是引用类型或可以为 null 的类型和值的`E`是 null，则结果为 false。</span><span class="sxs-lookup"><span data-stu-id="08161-2224">If `E` is a method group or the `null` literal, of if the type of `E` is a reference type or a nullable type and the value of `E` is null, the result is false.</span></span>
*  <span data-ttu-id="08161-2225">否则，让`D`表示的动态类型`E`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-2225">Otherwise, let `D` represent the dynamic type of `E` as follows:</span></span>
   * <span data-ttu-id="08161-2226">如果类型`E`是引用类型，`D`是引用的实例的运行时类型`E`。</span><span class="sxs-lookup"><span data-stu-id="08161-2226">If the type of `E` is a reference type, `D` is the run-time type of the instance reference by `E`.</span></span>
   * <span data-ttu-id="08161-2227">如果类型`E`为 null 的类型，`D`是该可以为 null 的类型的基础类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2227">If the type of `E` is a nullable type, `D` is the underlying type of that nullable type.</span></span>
   * <span data-ttu-id="08161-2228">如果类型`E`是不可以为 null 的值类型，`D`是一种`E`。</span><span class="sxs-lookup"><span data-stu-id="08161-2228">If the type of `E` is a non-nullable value type, `D` is the type of `E`.</span></span>
*  <span data-ttu-id="08161-2229">操作的结果取决于`D`和`T`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-2229">The result of the operation depends on `D` and `T` as follows:</span></span>
   * <span data-ttu-id="08161-2230">如果`T`是引用类型，则结果为 true 如果`D`并`T`是相同的类型，如果`D`是引用类型，从的隐式引用转换`D`到`T`存在，或者如果`D`是值类型和装箱转换从`D`到`T`存在。</span><span class="sxs-lookup"><span data-stu-id="08161-2230">If `T` is a reference type, the result is true if `D` and `T` are the same type, if `D` is a reference type and an implicit reference conversion from `D` to `T` exists, or if `D` is a value type and a boxing conversion from `D` to `T` exists.</span></span>
   * <span data-ttu-id="08161-2231">如果`T`为 null 的类型，则结果为 true 如果`D`是基础类型`T`。</span><span class="sxs-lookup"><span data-stu-id="08161-2231">If `T` is a nullable type, the result is true if `D` is the underlying type of `T`.</span></span>
   * <span data-ttu-id="08161-2232">如果`T`是不可以为 null 的值类型，则结果为 true 如果`D`和`T`属于同一类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2232">If `T` is a non-nullable value type, the result is true if `D` and `T` are the same type.</span></span>
   * <span data-ttu-id="08161-2233">否则，结果为 false。</span><span class="sxs-lookup"><span data-stu-id="08161-2233">Otherwise, the result is false.</span></span>

<span data-ttu-id="08161-2234">请注意，用户定义的转换，不考虑`is`运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-2234">Note that user defined conversions, are not considered by the `is` operator.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="08161-2235">As 运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2235">The as operator</span></span>

<span data-ttu-id="08161-2236">`as`运算符用于将值显式转换为给定的引用类型或可以为 null 的类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2236">The `as` operator is used to explicitly convert a value to a given reference type or nullable type.</span></span> <span data-ttu-id="08161-2237">与不同的强制转换表达式 ([强制转换表达式](expressions.md#cast-expressions))，则`as`运算符永远不会引发异常。</span><span class="sxs-lookup"><span data-stu-id="08161-2237">Unlike a cast expression ([Cast expressions](expressions.md#cast-expressions)), the `as` operator never throws an exception.</span></span> <span data-ttu-id="08161-2238">相反，如果指定的转换不可能，生成的值是`null`。</span><span class="sxs-lookup"><span data-stu-id="08161-2238">Instead, if the indicated conversion is not possible, the resulting value is `null`.</span></span>

<span data-ttu-id="08161-2239">在操作中的窗体`E as T`，`E`必须为的表达式和`T`必须是引用类型，已知为引用类型或为 null 的类型的类型参数。</span><span class="sxs-lookup"><span data-stu-id="08161-2239">In an operation of the form `E as T`, `E` must be an expression and `T` must be a reference type, a type parameter known to be a reference type, or a nullable type.</span></span> <span data-ttu-id="08161-2240">此外，在至少一个以下必须为 true，否则会发生编译时错误：</span><span class="sxs-lookup"><span data-stu-id="08161-2240">Furthermore, at least one of the following must be true, or otherwise a compile-time error occurs:</span></span>

*  <span data-ttu-id="08161-2241">标识 ([标识转换](conversions.md#identity-conversion))、 隐式可以为 null ([可以为 null 的隐式转换](conversions.md#implicit-nullable-conversions))，隐式引用 ([隐式引用转换](conversions.md#implicit-reference-conversions))，装箱 ([装箱转换](conversions.md#boxing-conversions))、 显式可以为 null ([可以为 null 的显式转换](conversions.md#explicit-nullable-conversions))，显式引用 ([显式引用转换](conversions.md#explicit-reference-conversions))，或取消装箱 ([取消装箱转换](conversions.md#unboxing-conversions)) 存在从转换`E`到`T`。</span><span class="sxs-lookup"><span data-stu-id="08161-2241">An identity ([Identity conversion](conversions.md#identity-conversion)), implicit nullable ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)), implicit reference ([Implicit reference conversions](conversions.md#implicit-reference-conversions)), boxing ([Boxing conversions](conversions.md#boxing-conversions)), explicit nullable ([Explicit nullable conversions](conversions.md#explicit-nullable-conversions)), explicit reference ([Explicit reference conversions](conversions.md#explicit-reference-conversions)), or unboxing ([Unboxing conversions](conversions.md#unboxing-conversions)) conversion exists from `E` to `T`.</span></span>
*  <span data-ttu-id="08161-2242">类型`E`或`T`是开放类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2242">The type of `E` or `T` is an open type.</span></span>
*  <span data-ttu-id="08161-2243">`E` 是`null`文本。</span><span class="sxs-lookup"><span data-stu-id="08161-2243">`E` is the `null` literal.</span></span>

<span data-ttu-id="08161-2244">如果编译时类型`E`不是`dynamic`，该操作`E as T`生成相同的结果</span><span class="sxs-lookup"><span data-stu-id="08161-2244">If the compile-time type of `E` is not `dynamic`, the operation `E as T` produces the same result as</span></span>
```csharp
E is T ? (T)(E) : (T)null
```
<span data-ttu-id="08161-2245">不同的是 `E` 只计算一次。</span><span class="sxs-lookup"><span data-stu-id="08161-2245">except that `E` is only evaluated once.</span></span> <span data-ttu-id="08161-2246">编译器需要优化`E as T`执行一个有最多动态类型检查而不是上面的扩展权限隐含的两个动态类型检查。</span><span class="sxs-lookup"><span data-stu-id="08161-2246">The compiler can be expected to optimize `E as T` to perform at most one dynamic type check as opposed to the two dynamic type checks implied by the expansion above.</span></span>

<span data-ttu-id="08161-2247">如果编译时类型`E`是`dynamic`，与强制转换运算符不同`as`运算符未动态绑定 ([动态绑定](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="08161-2247">If the compile-time type of `E` is `dynamic`, unlike the cast operator the `as` operator is not dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="08161-2248">因此扩展在这种情况下是：</span><span class="sxs-lookup"><span data-stu-id="08161-2248">Therefore the expansion in this case is:</span></span>
```csharp
E is T ? (T)(object)(E) : (T)null
```

<span data-ttu-id="08161-2249">请注意某些转换，如用户定义的转换，不可能实现`as`运算符，并且应改为执行使用强制转换表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-2249">Note that some conversions, such as user defined conversions, are not possible with the `as` operator and should instead be performed using cast expressions.</span></span>

<span data-ttu-id="08161-2250">在示例</span><span class="sxs-lookup"><span data-stu-id="08161-2250">In the example</span></span>
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
<span data-ttu-id="08161-2251">类型参数`T`的`G`已知为引用类型，因为它具有类约束。</span><span class="sxs-lookup"><span data-stu-id="08161-2251">the type parameter `T` of `G` is known to be a reference type, because it has the class constraint.</span></span> <span data-ttu-id="08161-2252">类型参数`U`的`H`不但是; 因此使用`as`中的运算符`H`不允许的。</span><span class="sxs-lookup"><span data-stu-id="08161-2252">The type parameter `U` of `H` is not however; hence the use of the `as` operator in `H` is disallowed.</span></span>

## <a name="logical-operators"></a><span data-ttu-id="08161-2253">逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2253">Logical operators</span></span>

<span data-ttu-id="08161-2254">`&`， `^`，和`|`操作员称为逻辑运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-2254">The `&`, `^`, and `|` operators are called the logical operators.</span></span>

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

<span data-ttu-id="08161-2255">如果一个逻辑运算符的操作数具有编译时类型`dynamic`，然后动态绑定表达式 ([动态绑定](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="08161-2255">If an operand of a logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="08161-2256">在这种情况下该表达式的编译时类型是`dynamic`，如下所述的分辨率将发生在运行时使用这些具有编译时类型的操作数的运行时类型和`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="08161-2256">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="08161-2257">有关操作的窗体`x op y`，其中`op`是一个逻辑运算符和重载决策 ([二元运算符重载决策](expressions.md#binary-operator-overload-resolution)) 应用以选择特定的运算符实现。</span><span class="sxs-lookup"><span data-stu-id="08161-2257">For an operation of the form `x op y`, where `op` is one of the logical operators, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="08161-2258">操作数都转换为所选运算符的参数类型和结果的类型是运算符的返回类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2258">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="08161-2259">以下各节描述了预定义的逻辑运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-2259">The predefined logical operators are described in the following sections.</span></span>

### <a name="integer-logical-operators"></a><span data-ttu-id="08161-2260">整数逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2260">Integer logical operators</span></span>

<span data-ttu-id="08161-2261">预定义的整数逻辑运算符为：</span><span class="sxs-lookup"><span data-stu-id="08161-2261">The predefined integer logical operators are:</span></span>
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

<span data-ttu-id="08161-2262">`&`运算符计算的按位逻辑`AND`的两个操作数`|`运算符计算的按位逻辑`OR`的两个操作数和`^`运算符计算的按位逻辑异`OR`的两个操作数。</span><span class="sxs-lookup"><span data-stu-id="08161-2262">The `&` operator computes the bitwise logical `AND` of the two operands, the `|` operator computes the bitwise logical `OR` of the two operands, and the `^` operator computes the bitwise logical exclusive `OR` of the two operands.</span></span> <span data-ttu-id="08161-2263">从这些操作可能不会出现任何溢出。</span><span class="sxs-lookup"><span data-stu-id="08161-2263">No overflows are possible from these operations.</span></span>

### <a name="enumeration-logical-operators"></a><span data-ttu-id="08161-2264">枚举逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2264">Enumeration logical operators</span></span>

<span data-ttu-id="08161-2265">每个枚举类型`E`隐式提供以下预定义的逻辑运算符：</span><span class="sxs-lookup"><span data-stu-id="08161-2265">Every enumeration type `E` implicitly provides the following predefined logical operators:</span></span>

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

<span data-ttu-id="08161-2266">计算结果`x op y`，其中`x`并`y`是枚举类型的表达式`E`与基础类型`U`，并`op`是一个逻辑运算符，是完全相同评估`(E)((U)x op (U)y)`。</span><span class="sxs-lookup"><span data-stu-id="08161-2266">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the logical operators, is exactly the same as evaluating `(E)((U)x op (U)y)`.</span></span> <span data-ttu-id="08161-2267">换而言之，枚举类型的逻辑运算符只需执行两个操作数的基础类型上的逻辑操作。</span><span class="sxs-lookup"><span data-stu-id="08161-2267">In other words, the enumeration type logical operators simply perform the logical operation on the underlying type of the two operands.</span></span>

### <a name="boolean-logical-operators"></a><span data-ttu-id="08161-2268">布尔逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2268">Boolean logical operators</span></span>

<span data-ttu-id="08161-2269">预定义的布尔逻辑运算符为：</span><span class="sxs-lookup"><span data-stu-id="08161-2269">The predefined boolean logical operators are:</span></span>
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

<span data-ttu-id="08161-2270">如果 `x` 和 `y` 都为 `true`，则 `x & y` 的结果为 `true`。</span><span class="sxs-lookup"><span data-stu-id="08161-2270">The result of `x & y` is `true` if both `x` and `y` are `true`.</span></span> <span data-ttu-id="08161-2271">否则，结果为 `false`。</span><span class="sxs-lookup"><span data-stu-id="08161-2271">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="08161-2272">结果`x | y`是`true`如果任一`x`或`y`是`true`。</span><span class="sxs-lookup"><span data-stu-id="08161-2272">The result of `x | y` is `true` if either `x` or `y` is `true`.</span></span> <span data-ttu-id="08161-2273">否则，结果为 `false`。</span><span class="sxs-lookup"><span data-stu-id="08161-2273">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="08161-2274">结果`x ^ y`是`true`如果`x`是`true`并`y`是`false`，或者`x`是`false`和`y`是`true`。</span><span class="sxs-lookup"><span data-stu-id="08161-2274">The result of `x ^ y` is `true` if `x` is `true` and `y` is `false`, or `x` is `false` and `y` is `true`.</span></span> <span data-ttu-id="08161-2275">否则，结果为 `false`。</span><span class="sxs-lookup"><span data-stu-id="08161-2275">Otherwise, the result is `false`.</span></span> <span data-ttu-id="08161-2276">如果操作数均为类型`bool`，则`^`运算符计算相同的结果`!=`运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-2276">When the operands are of type `bool`, the `^` operator computes the same result as the `!=` operator.</span></span>

### <a name="nullable-boolean-logical-operators"></a><span data-ttu-id="08161-2277">可以为 null 的布尔逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2277">Nullable boolean logical operators</span></span>

<span data-ttu-id="08161-2278">可以为 null 的布尔类型`bool?`可以表示三个值`true`， `false`，和`null`，它从概念上讲类似于用于在 SQL 中的布尔表达式的三个值类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2278">The nullable boolean type `bool?` can represent three values, `true`, `false`, and `null`, and is conceptually similar to the three-valued type used for boolean expressions in SQL.</span></span> <span data-ttu-id="08161-2279">若要确保生成的结果`&`并`|`运算符`bool?`操作数都与 SQL 的三值逻辑一致的提供以下预定义的运算符：</span><span class="sxs-lookup"><span data-stu-id="08161-2279">To ensure that the results produced by the `&` and `|` operators for `bool?` operands are consistent with SQL's three-valued logic, the following predefined operators are provided:</span></span>

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

<span data-ttu-id="08161-2280">下表列出了这些运算符时针对所有值的组合所产生的结果`true`， `false`，和`null`。</span><span class="sxs-lookup"><span data-stu-id="08161-2280">The following table lists the results produced by these operators for all combinations of the values `true`, `false`, and `null`.</span></span>

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

## <a name="conditional-logical-operators"></a><span data-ttu-id="08161-2281">条件逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2281">Conditional logical operators</span></span>

<span data-ttu-id="08161-2282">`&&`和`||`操作员称为条件逻辑运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-2282">The `&&` and `||` operators are called the conditional logical operators.</span></span> <span data-ttu-id="08161-2283">它们也称为"短路"逻辑运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-2283">They are also called the "short-circuiting" logical operators.</span></span>

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

<span data-ttu-id="08161-2284">`&&`并`||`运算符是条件新版`&`和`|`运算符：</span><span class="sxs-lookup"><span data-stu-id="08161-2284">The `&&` and `||` operators are conditional versions of the `&` and `|` operators:</span></span>

*  <span data-ttu-id="08161-2285">该操作`x && y`对应于该操作`x & y`，只不过`y`才计算`x`不是`false`。</span><span class="sxs-lookup"><span data-stu-id="08161-2285">The operation `x && y` corresponds to the operation `x & y`, except that `y` is evaluated only if `x` is not `false`.</span></span>
*  <span data-ttu-id="08161-2286">该操作`x || y`对应于该操作`x | y`，只不过`y`才计算`x`不是`true`。</span><span class="sxs-lookup"><span data-stu-id="08161-2286">The operation `x || y` corresponds to the operation `x | y`, except that `y` is evaluated only if `x` is not `true`.</span></span>

<span data-ttu-id="08161-2287">如果条件的逻辑运算符的操作数具有编译时类型`dynamic`，然后动态绑定表达式 ([动态绑定](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="08161-2287">If an operand of a conditional logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="08161-2288">在这种情况下该表达式的编译时类型是`dynamic`，如下所述的分辨率将发生在运行时使用这些具有编译时类型的操作数的运行时类型和`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="08161-2288">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="08161-2289">操作的窗体`x && y`或`x || y`处理通过应用重载解析 ([二元运算符重载决策](expressions.md#binary-operator-overload-resolution)) 编写该操作已`x & y`或`x | y`。</span><span class="sxs-lookup"><span data-stu-id="08161-2289">An operation of the form `x && y` or `x || y` is processed by applying overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x & y` or `x | y`.</span></span> <span data-ttu-id="08161-2290">然后，</span><span class="sxs-lookup"><span data-stu-id="08161-2290">Then,</span></span>

*  <span data-ttu-id="08161-2291">如果重载解决方法未能找到一个最佳运算符，或者如果重载决策选择其中一个预定义的整数逻辑运算符，会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-2291">If overload resolution fails to find a single best operator, or if overload resolution selects one of the predefined integer logical operators, a binding-time error occurs.</span></span>
*  <span data-ttu-id="08161-2292">否则为如果所选的运算符是一个预定义的布尔逻辑运算符 ([布尔逻辑运算符](expressions.md#boolean-logical-operators)) 或可以为 null 的布尔逻辑运算符 ([可以为 Null 的布尔逻辑运算符](expressions.md#nullable-boolean-logical-operators))，则处理操作中所述[布尔条件逻辑运算符](expressions.md#boolean-conditional-logical-operators)。</span><span class="sxs-lookup"><span data-stu-id="08161-2292">Otherwise, if the selected operator is one of the predefined boolean logical operators ([Boolean logical operators](expressions.md#boolean-logical-operators)) or nullable boolean logical operators ([Nullable boolean logical operators](expressions.md#nullable-boolean-logical-operators)), the operation is processed as described in [Boolean conditional logical operators](expressions.md#boolean-conditional-logical-operators).</span></span>
*  <span data-ttu-id="08161-2293">否则为所选的运算符是用户定义的运算符，该操作处理中所述[用户定义的条件逻辑运算符](expressions.md#user-defined-conditional-logical-operators)。</span><span class="sxs-lookup"><span data-stu-id="08161-2293">Otherwise, the selected operator is a user-defined operator, and the operation is processed as described in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

<span data-ttu-id="08161-2294">不能直接重载条件逻辑运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-2294">It is not possible to directly overload the conditional logical operators.</span></span> <span data-ttu-id="08161-2295">但是，因为按常规逻辑运算符计算条件逻辑运算符，重载常规逻辑运算符，在某些限制，也被视为条件逻辑运算符的重载。</span><span class="sxs-lookup"><span data-stu-id="08161-2295">However, because the conditional logical operators are evaluated in terms of the regular logical operators, overloads of the regular logical operators are, with certain restrictions, also considered overloads of the conditional logical operators.</span></span> <span data-ttu-id="08161-2296">这是中作了进一步介绍[用户定义的条件逻辑运算符](expressions.md#user-defined-conditional-logical-operators)。</span><span class="sxs-lookup"><span data-stu-id="08161-2296">This is described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

### <a name="boolean-conditional-logical-operators"></a><span data-ttu-id="08161-2297">布尔条件逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2297">Boolean conditional logical operators</span></span>

<span data-ttu-id="08161-2298">时的操作数`&&`或`||`属于类型`bool`，或未定义适用的类型的操作数时`operator &`或`operator |`，但定义隐式转换为`bool`，操作处理，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-2298">When the operands of `&&` or `||` are of type `bool`, or when the operands are of types that do not define an applicable `operator &` or `operator |`, but do define implicit conversions to `bool`, the operation is processed as follows:</span></span>

*  <span data-ttu-id="08161-2299">该操作`x && y`评为`x ? y : false`。</span><span class="sxs-lookup"><span data-stu-id="08161-2299">The operation `x && y` is evaluated as `x ? y : false`.</span></span> <span data-ttu-id="08161-2300">换而言之，`x`首先计算并将转换为类型`bool`。</span><span class="sxs-lookup"><span data-stu-id="08161-2300">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="08161-2301">然后，如果`x`是`true`，`y`进行计算并转换为类型`bool`，这将成为该操作的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-2301">Then, if `x` is `true`, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span> <span data-ttu-id="08161-2302">否则，该操作的结果是`false`。</span><span class="sxs-lookup"><span data-stu-id="08161-2302">Otherwise, the result of the operation is `false`.</span></span>
*  <span data-ttu-id="08161-2303">该操作`x || y`评为`x ? true : y`。</span><span class="sxs-lookup"><span data-stu-id="08161-2303">The operation `x || y` is evaluated as `x ? true : y`.</span></span> <span data-ttu-id="08161-2304">换而言之，`x`首先计算并将转换为类型`bool`。</span><span class="sxs-lookup"><span data-stu-id="08161-2304">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="08161-2305">然后，如果`x`是`true`，该操作的结果是`true`。</span><span class="sxs-lookup"><span data-stu-id="08161-2305">Then, if `x` is `true`, the result of the operation is `true`.</span></span> <span data-ttu-id="08161-2306">否则为`y`进行计算并转换为类型`bool`，这将成为该操作的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-2306">Otherwise, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span>

### <a name="user-defined-conditional-logical-operators"></a><span data-ttu-id="08161-2307">用户定义的条件逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2307">User-defined conditional logical operators</span></span>

<span data-ttu-id="08161-2308">时的操作数`&&`或`||`是声明适用的类型的用户定义`operator &`或`operator |`，下列两个条件必须为 true，其中`T`是在其中声明所选的运算符的类型：</span><span class="sxs-lookup"><span data-stu-id="08161-2308">When the operands of `&&` or `||` are of types that declare an applicable user-defined `operator &` or `operator |`, both of the following must be true, where `T` is the type in which the selected operator is declared:</span></span>

*  <span data-ttu-id="08161-2309">返回类型和所选运算符的每个参数的类型必须是`T`。</span><span class="sxs-lookup"><span data-stu-id="08161-2309">The return type and the type of each parameter of the selected operator must be `T`.</span></span> <span data-ttu-id="08161-2310">换而言之，操作员必须计算逻辑`AND`或逻辑`OR`的类型的两个操作数`T`，并且必须返回类型的结果`T`。</span><span class="sxs-lookup"><span data-stu-id="08161-2310">In other words, the operator must compute the logical `AND` or the logical `OR` of two operands of type `T`, and must return a result of type `T`.</span></span>
*  <span data-ttu-id="08161-2311">`T` 必须包含的声明`operator true`和`operator false`。</span><span class="sxs-lookup"><span data-stu-id="08161-2311">`T` must contain declarations of `operator true` and `operator false`.</span></span>

<span data-ttu-id="08161-2312">如果不满足任一要求，则会发生绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-2312">A binding-time error occurs if either of these requirements is not satisfied.</span></span> <span data-ttu-id="08161-2313">否则为`&&`或`||`通过组合用户定义运算`operator true`或`operator false`与所选用户定义的运算符：</span><span class="sxs-lookup"><span data-stu-id="08161-2313">Otherwise, the `&&` or `||` operation is evaluated by combining the user-defined `operator true` or `operator false` with the selected user-defined operator:</span></span>

*  <span data-ttu-id="08161-2314">该操作`x && y`评为`T.false(x) ? x : T.&(x, y)`，其中`T.false(x)`的调用`operator false`中声明`T`，和`T.&(x, y)`的所选调用`operator &`。</span><span class="sxs-lookup"><span data-stu-id="08161-2314">The operation `x && y` is evaluated as `T.false(x) ? x : T.&(x, y)`, where `T.false(x)` is an invocation of the `operator false` declared in `T`, and `T.&(x, y)` is an invocation of the selected `operator &`.</span></span> <span data-ttu-id="08161-2315">换而言之，`x`首先计算并`operator false`结果可确定是否调用`x`肯定为假。</span><span class="sxs-lookup"><span data-stu-id="08161-2315">In other words, `x` is first evaluated and `operator false` is invoked on the result to determine if `x` is definitely false.</span></span> <span data-ttu-id="08161-2316">然后，如果`x`肯定为假，该操作的结果是为以前计算的值`x`。</span><span class="sxs-lookup"><span data-stu-id="08161-2316">Then, if `x` is definitely false, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="08161-2317">否则为`y`计算时，并且所选`operator &`为以前计算的值上调用`x`的计算值和`y`生成操作的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-2317">Otherwise, `y` is evaluated, and the selected `operator &` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>
*  <span data-ttu-id="08161-2318">该操作`x || y`评为`T.true(x) ? x : T.|(x, y)`，其中`T.true(x)`的调用`operator true`中声明`T`，和`T.|(x,y)`的所选调用`operator|`。</span><span class="sxs-lookup"><span data-stu-id="08161-2318">The operation `x || y` is evaluated as `T.true(x) ? x : T.|(x, y)`, where `T.true(x)` is an invocation of the `operator true` declared in `T`, and `T.|(x,y)` is an invocation of the selected `operator|`.</span></span> <span data-ttu-id="08161-2319">换而言之，`x`首先计算并`operator true`结果可确定是否调用`x`肯定为真。</span><span class="sxs-lookup"><span data-stu-id="08161-2319">In other words, `x` is first evaluated and `operator true` is invoked on the result to determine if `x` is definitely true.</span></span> <span data-ttu-id="08161-2320">然后，如果`x`则肯定会这样，该操作的结果是为以前计算的值`x`。</span><span class="sxs-lookup"><span data-stu-id="08161-2320">Then, if `x` is definitely true, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="08161-2321">否则为`y`计算时，并且所选`operator |`为以前计算的值上调用`x`的计算值和`y`生成操作的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-2321">Otherwise, `y` is evaluated, and the selected `operator |` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>

<span data-ttu-id="08161-2322">这些操作，提供的表达式之一`x`是只计算一次，并由给定的表达式`y`或者不在评估或仅计算一次。</span><span class="sxs-lookup"><span data-stu-id="08161-2322">In either of these operations, the expression given by `x` is only evaluated once, and the expression given by `y` is either not evaluated or evaluated exactly once.</span></span>

<span data-ttu-id="08161-2323">有关实现的类型的示例`operator true`并`operator false`，请参阅[数据库布尔类型](structs.md#database-boolean-type)。</span><span class="sxs-lookup"><span data-stu-id="08161-2323">For an example of a type that implements `operator true` and `operator false`, see [Database boolean type](structs.md#database-boolean-type).</span></span>

## <a name="the-null-coalescing-operator"></a><span data-ttu-id="08161-2324">Null 合并运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2324">The null coalescing operator</span></span>

<span data-ttu-id="08161-2325">`??`运算符称作 null 合并运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-2325">The `??` operator is called the null coalescing operator.</span></span>

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

<span data-ttu-id="08161-2326">在窗体 null 合并表达式`a ?? b`需要`a`为可以为 null 的类型或引用类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2326">A null coalescing expression of the form `a ?? b` requires `a` to be of a nullable type or reference type.</span></span> <span data-ttu-id="08161-2327">如果`a`为非 null 的结果`a ?? b`是`a`; 否则为结果是`b`。</span><span class="sxs-lookup"><span data-stu-id="08161-2327">If `a` is non-null, the result of `a ?? b` is `a`; otherwise, the result is `b`.</span></span> <span data-ttu-id="08161-2328">操作会评估`b`仅当`a`为 null。</span><span class="sxs-lookup"><span data-stu-id="08161-2328">The operation evaluates `b` only if `a` is null.</span></span>

<span data-ttu-id="08161-2329">Null 合并运算符是右结合运算符，这意味着操作将从右到左分组。</span><span class="sxs-lookup"><span data-stu-id="08161-2329">The null coalescing operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="08161-2330">例如，窗体的表达式`a ?? b ?? c`评为`a ?? (b ?? c)`。</span><span class="sxs-lookup"><span data-stu-id="08161-2330">For example, an expression of the form `a ?? b ?? c` is evaluated as `a ?? (b ?? c)`.</span></span> <span data-ttu-id="08161-2331">一般情况下条款形式的表达式`E1 ?? E2 ?? ... ?? En`返回第一个操作数的非 null 或为 null，如果所有操作数都都为 null。</span><span class="sxs-lookup"><span data-stu-id="08161-2331">In general terms, an expression of the form `E1 ?? E2 ?? ... ?? En` returns the first of the operands that is non-null, or null if all operands are null.</span></span>

<span data-ttu-id="08161-2332">表达式的类型`a ?? b`取决于哪个隐式转换都可通过对操作数。</span><span class="sxs-lookup"><span data-stu-id="08161-2332">The type of the expression `a ?? b` depends on which implicit conversions are available on the operands.</span></span> <span data-ttu-id="08161-2333">首选项的类型的顺序`a ?? b`是`A0`， `A`，或`B`，其中`A`是一种`a`(前提是`a`具有类型)，`B`是一种`b`(前提`b`有一个类型)，并`A0`是基础类型`A`如果`A`是一个可以为 null 的类型，或`A`否则为。</span><span class="sxs-lookup"><span data-stu-id="08161-2333">In order of preference, the type of `a ?? b` is `A0`, `A`, or `B`, where `A` is the type of `a` (provided that `a` has a type), `B` is the type of `b` (provided that `b` has a type), and `A0` is the underlying type of `A` if `A` is a nullable type, or `A` otherwise.</span></span> <span data-ttu-id="08161-2334">具体而言，`a ?? b`进行处理，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-2334">Specifically, `a ?? b` is processed as follows:</span></span>

*  <span data-ttu-id="08161-2335">如果`A`存在且不为 null 的类型或引用类型，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-2335">If `A` exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>
*  <span data-ttu-id="08161-2336">如果`b`是一个动态表达式的结果类型是`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="08161-2336">If `b` is a dynamic expression, the result type is `dynamic`.</span></span> <span data-ttu-id="08161-2337">在运行时`a`首先计算。</span><span class="sxs-lookup"><span data-stu-id="08161-2337">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="08161-2338">如果`a`不为 null，`a`转换为动态，这将成为结果。</span><span class="sxs-lookup"><span data-stu-id="08161-2338">If `a` is not null, `a` is converted to dynamic, and this becomes the result.</span></span> <span data-ttu-id="08161-2339">否则为`b`计算时，这将成为结果。</span><span class="sxs-lookup"><span data-stu-id="08161-2339">Otherwise, `b` is evaluated, and this becomes the result.</span></span>
*  <span data-ttu-id="08161-2340">否则为如果`A`存在和为 null 的类型和隐式转换`b`到`A0`，该结果类型是`A0`。</span><span class="sxs-lookup"><span data-stu-id="08161-2340">Otherwise, if `A` exists and is a nullable type and an implicit conversion exists from `b` to `A0`, the result type is `A0`.</span></span> <span data-ttu-id="08161-2341">在运行时`a`首先计算。</span><span class="sxs-lookup"><span data-stu-id="08161-2341">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="08161-2342">如果`a`不为 null，`a`解包为类型`A0`，这将成为结果。</span><span class="sxs-lookup"><span data-stu-id="08161-2342">If `a` is not null, `a` is unwrapped to type `A0`, and this becomes the result.</span></span> <span data-ttu-id="08161-2343">否则为`b`进行计算并转换为类型`A0`，这将成为结果。</span><span class="sxs-lookup"><span data-stu-id="08161-2343">Otherwise, `b` is evaluated and converted to type `A0`, and this becomes the result.</span></span>
*  <span data-ttu-id="08161-2344">否则为如果`A`存在并且存在一个隐式转换从`b`到`A`，该结果类型是`A`。</span><span class="sxs-lookup"><span data-stu-id="08161-2344">Otherwise, if `A` exists and an implicit conversion exists from `b` to `A`, the result type is `A`.</span></span> <span data-ttu-id="08161-2345">在运行时`a`首先计算。</span><span class="sxs-lookup"><span data-stu-id="08161-2345">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="08161-2346">如果`a`不为 null，`a`使其成为结果。</span><span class="sxs-lookup"><span data-stu-id="08161-2346">If `a` is not null, `a` becomes the result.</span></span> <span data-ttu-id="08161-2347">否则为`b`进行计算并转换为类型`A`，这将成为结果。</span><span class="sxs-lookup"><span data-stu-id="08161-2347">Otherwise, `b` is evaluated and converted to type `A`, and this becomes the result.</span></span>
*  <span data-ttu-id="08161-2348">否则为如果`b`具有类型`B`并且从存在的隐式转换`a`到`B`，结果类型是`B`。</span><span class="sxs-lookup"><span data-stu-id="08161-2348">Otherwise, if `b` has a type `B` and an implicit conversion exists from `a` to `B`, the result type is `B`.</span></span> <span data-ttu-id="08161-2349">在运行时`a`首先计算。</span><span class="sxs-lookup"><span data-stu-id="08161-2349">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="08161-2350">如果`a`不为 null，`a`解包为类型`A0`(如果`A`是否存在以及是否可以为 null) 和转换为键入`B`，这将成为结果。</span><span class="sxs-lookup"><span data-stu-id="08161-2350">If `a` is not null, `a` is unwrapped to type `A0` (if `A` exists and is nullable) and converted to type `B`, and this becomes the result.</span></span> <span data-ttu-id="08161-2351">否则为`b`计算，并使其成为结果。</span><span class="sxs-lookup"><span data-stu-id="08161-2351">Otherwise, `b` is evaluated and becomes the result.</span></span>
*  <span data-ttu-id="08161-2352">否则为`a`和`b`是不兼容，并且编译时错误发生。</span><span class="sxs-lookup"><span data-stu-id="08161-2352">Otherwise, `a` and `b` are incompatible, and a compile-time error occurs.</span></span>

## <a name="conditional-operator"></a><span data-ttu-id="08161-2353">条件运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2353">Conditional operator</span></span>

<span data-ttu-id="08161-2354">`?:`调用条件运算符的运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-2354">The `?:` operator is called the conditional operator.</span></span> <span data-ttu-id="08161-2355">它有时也称为三元运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-2355">It is at times also called the ternary operator.</span></span>

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

<span data-ttu-id="08161-2356">形式的条件表达式`b ? x : y`先计算条件`b`。</span><span class="sxs-lookup"><span data-stu-id="08161-2356">A conditional expression of the form `b ? x : y` first evaluates the condition `b`.</span></span> <span data-ttu-id="08161-2357">然后，如果`b`是`true`，`x`计算，并使其成为操作的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-2357">Then, if `b` is `true`, `x` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="08161-2358">否则为`y`计算，并使其成为操作的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-2358">Otherwise, `y` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="08161-2359">条件的表达式永远不会计算这两`x`和`y`。</span><span class="sxs-lookup"><span data-stu-id="08161-2359">A conditional expression never evaluates both `x` and `y`.</span></span>

<span data-ttu-id="08161-2360">条件运算符是右结合运算符，这意味着操作将从右到左分组。</span><span class="sxs-lookup"><span data-stu-id="08161-2360">The conditional operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="08161-2361">例如，窗体的表达式`a ? b : c ? d : e`评为`a ? b : (c ? d : e)`。</span><span class="sxs-lookup"><span data-stu-id="08161-2361">For example, an expression of the form `a ? b : c ? d : e` is evaluated as `a ? b : (c ? d : e)`.</span></span>

<span data-ttu-id="08161-2362">第一个操作数`?:`运算符必须可以隐式转换为的表达式`bool`，或实现的类型的表达式`operator true`。</span><span class="sxs-lookup"><span data-stu-id="08161-2362">The first operand of the `?:` operator must be an expression that can be implicitly converted to `bool`, or an expression of a type that implements `operator true`.</span></span> <span data-ttu-id="08161-2363">如果满足这些要求都不会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-2363">If neither of these requirements is satisfied, a compile-time error occurs.</span></span>

<span data-ttu-id="08161-2364">第二个和第三个操作数，`x`并`y`的`?:`运算符控制条件表达式的类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2364">The second and third operands, `x` and `y`, of the `?:` operator control the type of the conditional expression.</span></span>

*  <span data-ttu-id="08161-2365">如果`x`具有类型`X`并`y`具有类型`Y`然后</span><span class="sxs-lookup"><span data-stu-id="08161-2365">If `x` has type `X` and `y` has type `Y` then</span></span>
   * <span data-ttu-id="08161-2366">如果隐式转换 ([隐式转换](conversions.md#implicit-conversions)) 中存在`X`到`Y`，但不能从`Y`到`X`，然后`Y`是条件表达式的类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2366">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `X` to `Y`, but not from `Y` to `X`, then `Y` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="08161-2367">如果隐式转换 ([隐式转换](conversions.md#implicit-conversions)) 中存在`Y`到`X`，但不能从`X`到`Y`，然后`X`是条件表达式的类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2367">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `Y` to `X`, but not from `X` to `Y`, then `X` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="08161-2368">否则为可以确定任何表达式类型，并将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-2368">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="08161-2369">如果只有之一`x`并`y`具有一个类型，并且两个`x`和`y`的是隐式转换为该类型，那么它就是条件表达式的类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2369">If only one of `x` and `y` has a type, and both `x` and `y`, of are implicitly convertible to that type, then that is the type of the conditional expression.</span></span>
*  <span data-ttu-id="08161-2370">否则为可以确定任何表达式类型，并将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-2370">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>

<span data-ttu-id="08161-2371">处理窗体的条件表达式的运行时间`b ? x : y`包括以下步骤：</span><span class="sxs-lookup"><span data-stu-id="08161-2371">The run-time processing of a conditional expression of the form `b ? x : y` consists of the following steps:</span></span>

*  <span data-ttu-id="08161-2372">首先，`b`计算，并`bool`的值`b`确定：</span><span class="sxs-lookup"><span data-stu-id="08161-2372">First, `b` is evaluated, and the `bool` value of `b` is determined:</span></span>
   * <span data-ttu-id="08161-2373">如果从的类型的隐式转换`b`到`bool`存在，则执行此隐式转换以生成`bool`值。</span><span class="sxs-lookup"><span data-stu-id="08161-2373">If an implicit conversion from the type of `b` to `bool` exists, then this implicit conversion is performed to produce a `bool` value.</span></span>
   * <span data-ttu-id="08161-2374">否则为`operator true`的类型由定义`b`调用，以生成`bool`值。</span><span class="sxs-lookup"><span data-stu-id="08161-2374">Otherwise, the `operator true` defined by the type of `b` is invoked to produce a `bool` value.</span></span>
*  <span data-ttu-id="08161-2375">如果`bool`由上述步骤生成的值是`true`，然后`x`是计算并转换为的类型的条件表达式，这将成为条件表达式的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-2375">If the `bool` value produced by the step above is `true`, then `x` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>
*  <span data-ttu-id="08161-2376">否则为`y`是计算并转换为的类型的条件表达式，这将成为条件表达式的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-2376">Otherwise, `y` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>

## <a name="anonymous-function-expressions"></a><span data-ttu-id="08161-2377">匿名函数表达式</span><span class="sxs-lookup"><span data-stu-id="08161-2377">Anonymous function expressions</span></span>

<span data-ttu-id="08161-2378">***匿名函数***一个表达式，表示"行中"方法定义。</span><span class="sxs-lookup"><span data-stu-id="08161-2378">An ***anonymous function*** is an expression that represents an "in-line" method definition.</span></span> <span data-ttu-id="08161-2379">匿名函数没有值或类型本身，但转换为兼容的委托或表达式树类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2379">An anonymous function does not have a value or type in and of itself, but is convertible to a compatible delegate or expression tree type.</span></span> <span data-ttu-id="08161-2380">匿名函数转换的计算取决于转换的目标类型：如果它是委托类型，转换计算结果为引用它的匿名函数定义的方法的委托值。</span><span class="sxs-lookup"><span data-stu-id="08161-2380">The evaluation of an anonymous function conversion depends on the target type of the conversion: If it is a delegate type, the conversion evaluates to a delegate value referencing the method which the anonymous function defines.</span></span> <span data-ttu-id="08161-2381">如果表达式目录树类型，转换计算结果为表达式树表示形式的对象结构的方法的结构。</span><span class="sxs-lookup"><span data-stu-id="08161-2381">If it is an expression tree type, the conversion evaluates to an expression tree which represents the structure of the method as an object structure.</span></span>

<span data-ttu-id="08161-2382">由于历史原因有两种语法类型的匿名函数，即*lambda_expression*s 和*anonymous_method_expression*s。</span><span class="sxs-lookup"><span data-stu-id="08161-2382">For historical reasons there are two syntactic flavors of anonymous functions, namely *lambda_expression*s and *anonymous_method_expression*s.</span></span> <span data-ttu-id="08161-2383">对于几乎所有目的*lambda_expression*是更简洁、 更具表现力比*anonymous_method_expression*s，保留的语言为向后兼容性。</span><span class="sxs-lookup"><span data-stu-id="08161-2383">For almost all purposes, *lambda_expression*s are more concise and expressive than *anonymous_method_expression*s, which remain in the language for backwards compatibility.</span></span>

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

<span data-ttu-id="08161-2384">`=>`运算符具有与赋值相同的优先级 (`=`) 并且是右结合运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-2384">The `=>` operator has the same precedence as assignment (`=`) and is right-associative.</span></span>

<span data-ttu-id="08161-2385">使用匿名函数`async`修饰符是异步函数，并遵循中所述的规则[迭代器](classes.md#iterators)。</span><span class="sxs-lookup"><span data-stu-id="08161-2385">An anonymous function with the `async` modifier is an async function and follows the rules described in [Iterators](classes.md#iterators).</span></span>

<span data-ttu-id="08161-2386">中的窗体的匿名函数的参数*lambda_expression*可以显式或隐式类型化。</span><span class="sxs-lookup"><span data-stu-id="08161-2386">The parameters of an anonymous function in the form of a *lambda_expression* can be explicitly or implicitly typed.</span></span> <span data-ttu-id="08161-2387">在显式类型化的参数列表中，显式声明每个参数的类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2387">In an explicitly typed parameter list, the type of each parameter is explicitly stated.</span></span> <span data-ttu-id="08161-2388">在隐式类型化的参数列表中，参数的类型推断匿名函数的上下文从 — 具体来说，匿名函数转换为兼容的委托类型或类型提供的表达式目录树类型参数类型 ([匿名函数转换](conversions.md#anonymous-function-conversions))。</span><span class="sxs-lookup"><span data-stu-id="08161-2388">In an implicitly typed parameter list, the types of the parameters are inferred from the context in which the anonymous function occurs—specifically, when the anonymous function is converted to a compatible delegate type or expression tree type, that type provides the parameter types ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="08161-2389">在单一、 隐式类型化参数使用一个匿名函数，则可能会省略括号从参数列表。</span><span class="sxs-lookup"><span data-stu-id="08161-2389">In an anonymous function with a single, implicitly typed parameter, the parentheses may be omitted from the parameter list.</span></span> <span data-ttu-id="08161-2390">换而言之，窗体的匿名函数</span><span class="sxs-lookup"><span data-stu-id="08161-2390">In other words, an anonymous function of the form</span></span>
```csharp
( param ) => expr
```
<span data-ttu-id="08161-2391">可缩写为</span><span class="sxs-lookup"><span data-stu-id="08161-2391">can be abbreviated to</span></span>
```csharp
param => expr
```

<span data-ttu-id="08161-2392">中的窗体的匿名函数的参数列表*anonymous_method_expression*是可选的。</span><span class="sxs-lookup"><span data-stu-id="08161-2392">The parameter list of an anonymous function in the form of an *anonymous_method_expression* is optional.</span></span> <span data-ttu-id="08161-2393">如果指定，则必须显式键入参数。</span><span class="sxs-lookup"><span data-stu-id="08161-2393">If given, the parameters must be explicitly typed.</span></span> <span data-ttu-id="08161-2394">如果不是，匿名函数转换为一个委托，其任何参数列表不包含`out`参数。</span><span class="sxs-lookup"><span data-stu-id="08161-2394">If not, the anonymous function is convertible to a delegate with any parameter list not containing `out` parameters.</span></span>

<span data-ttu-id="08161-2395">一个*块*匿名函数的主体是可访问 ([终结点和可访问性](statements.md#end-points-and-reachability)) 除非匿名函数发生在无法访问语句内。</span><span class="sxs-lookup"><span data-stu-id="08161-2395">A *block* body of an anonymous function is reachable ([End points and reachability](statements.md#end-points-and-reachability)) unless the anonymous function occurs inside an unreachable statement.</span></span>

<span data-ttu-id="08161-2396">下面是匿名函数的一些示例：</span><span class="sxs-lookup"><span data-stu-id="08161-2396">Some examples of anonymous functions follow below:</span></span>

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

<span data-ttu-id="08161-2397">行为*lambda_expression*s 和*anonymous_method_expression*s 是除以下几点：</span><span class="sxs-lookup"><span data-stu-id="08161-2397">The behavior of *lambda_expression*s and *anonymous_method_expression*s is the same except for the following points:</span></span>

*  <span data-ttu-id="08161-2398">*anonymous_method_expression*s 允许要完全，省略的参数列表生成可转换为委托类型的任何值参数的列表。</span><span class="sxs-lookup"><span data-stu-id="08161-2398">*anonymous_method_expression*s permit the parameter list to be omitted entirely, yielding convertibility to delegate types of any list of value parameters.</span></span>
*  <span data-ttu-id="08161-2399">*lambda_expression*s 允许省略和而这方面的推断参数类型*anonymous_method_expression*需要显式指定的参数类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2399">*lambda_expression*s permit parameter types to be omitted and inferred whereas *anonymous_method_expression*s require parameter types to be explicitly stated.</span></span>
*  <span data-ttu-id="08161-2400">正文*lambda_expression*可以是表达式或语句块而正文*anonymous_method_expression*必须是语句块。</span><span class="sxs-lookup"><span data-stu-id="08161-2400">The body of a *lambda_expression* can be an expression or a statement block whereas the body of an *anonymous_method_expression* must be a statement block.</span></span>
*  <span data-ttu-id="08161-2401">仅*lambda_expression*具有到兼容的表达式树类型的转换 ([表达式树类型](types.md#expression-tree-types))。</span><span class="sxs-lookup"><span data-stu-id="08161-2401">Only *lambda_expression*s have conversions to compatible expression tree types ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-signatures"></a><span data-ttu-id="08161-2402">匿名函数签名</span><span class="sxs-lookup"><span data-stu-id="08161-2402">Anonymous function signatures</span></span>

<span data-ttu-id="08161-2403">可选*anonymous_function_signature*匿名函数的定义名称和 （可选） 的匿名函数的形参的类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2403">The optional *anonymous_function_signature* of an anonymous function defines the names and optionally the types of the formal parameters for the anonymous function.</span></span> <span data-ttu-id="08161-2404">匿名函数的参数的范围是*anonymous_function_body*。</span><span class="sxs-lookup"><span data-stu-id="08161-2404">The scope of the parameters of the anonymous function is the *anonymous_function_body*.</span></span> <span data-ttu-id="08161-2405">([作用域](basic-concepts.md#scopes)) 参数列表 （如果已指定） 一起构成了匿名方法体，声明空间 ([声明](basic-concepts.md#declarations))。</span><span class="sxs-lookup"><span data-stu-id="08161-2405">([Scopes](basic-concepts.md#scopes)) Together with the parameter list (if given) the anonymous-method-body constitutes a declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="08161-2406">因此是要与本地变量、 局部常量或其作用域包括的参数的名称匹配的匿名函数的参数的名称的编译时错误*anonymous_method_expression*或*lambda_表达式*。</span><span class="sxs-lookup"><span data-stu-id="08161-2406">It is thus a compile-time error for the name of a parameter of the anonymous function to match the name of a local variable, local constant or parameter whose scope includes the *anonymous_method_expression* or *lambda_expression*.</span></span>

<span data-ttu-id="08161-2407">如果具有一个匿名函数*explicit_anonymous_function_signature*，则限制为那些相同的顺序中具有相同参数类型和修饰符的兼容的委托类型和表达式树类型集。</span><span class="sxs-lookup"><span data-stu-id="08161-2407">If an anonymous function has an *explicit_anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have the same parameter types and modifiers in the same order.</span></span> <span data-ttu-id="08161-2408">与方法组转换相反 ([方法组转换](conversions.md#method-group-conversions))，不支持匿名函数参数类型的逆变。</span><span class="sxs-lookup"><span data-stu-id="08161-2408">In contrast to method group conversions ([Method group conversions](conversions.md#method-group-conversions)), contra-variance of anonymous function parameter types is not supported.</span></span> <span data-ttu-id="08161-2409">如果匿名函数不具有*anonymous_function_signature*，则限制为那些没有兼容的委托类型和表达式树类型的集`out`参数。</span><span class="sxs-lookup"><span data-stu-id="08161-2409">If an anonymous function does not have an *anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have no `out` parameters.</span></span>

<span data-ttu-id="08161-2410">请注意， *anonymous_function_signature*不能包含属性或参数数组。</span><span class="sxs-lookup"><span data-stu-id="08161-2410">Note that an *anonymous_function_signature* cannot include attributes or a parameter array.</span></span> <span data-ttu-id="08161-2411">不过， *anonymous_function_signature*可能用其参数列表包含参数数组的委托类型兼容。</span><span class="sxs-lookup"><span data-stu-id="08161-2411">Nevertheless, an *anonymous_function_signature* may be compatible with a delegate type whose parameter list contains a parameter array.</span></span>

<span data-ttu-id="08161-2412">另请注意该转换为表达式树类型，即使兼容，可能仍会在编译时失败 ([表达式树类型](types.md#expression-tree-types))。</span><span class="sxs-lookup"><span data-stu-id="08161-2412">Note also that conversion to an expression tree type, even if compatible, may still fail at compile-time ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-bodies"></a><span data-ttu-id="08161-2413">匿名函数体</span><span class="sxs-lookup"><span data-stu-id="08161-2413">Anonymous function bodies</span></span>

<span data-ttu-id="08161-2414">正文 (*表达式*或*块*) 的匿名函数时要遵循以下规则：</span><span class="sxs-lookup"><span data-stu-id="08161-2414">The body (*expression* or *block*) of an anonymous function is subject to the following rules:</span></span>

*  <span data-ttu-id="08161-2415">如果匿名函数包含签名，签名中指定的参数提供了在正文。</span><span class="sxs-lookup"><span data-stu-id="08161-2415">If the anonymous function includes a signature, the parameters specified in the signature are available in the body.</span></span> <span data-ttu-id="08161-2416">如果匿名函数没有签名它可转换为委托类型或具有参数的表达式类型 ([匿名函数转换](conversions.md#anonymous-function-conversions))，但参数不能访问正文中。</span><span class="sxs-lookup"><span data-stu-id="08161-2416">If the anonymous function has no signature it can be converted to a delegate type or expression type having parameters ([Anonymous function conversions](conversions.md#anonymous-function-conversions)), but the parameters cannot be accessed in the body.</span></span>
*  <span data-ttu-id="08161-2417">除`ref`或`out`最近的封闭的签名 （如果有） 中指定的参数匿名函数，它是要访问的正文的编译时错误`ref`或`out`参数。</span><span class="sxs-lookup"><span data-stu-id="08161-2417">Except for `ref` or `out` parameters specified in the signature (if any) of the nearest enclosing anonymous function, it is a compile-time error for the body to access a `ref` or `out` parameter.</span></span>
*  <span data-ttu-id="08161-2418">时的类型`this`是一个结构类型，它是要访问的正文的编译时错误`this`。</span><span class="sxs-lookup"><span data-stu-id="08161-2418">When the type of `this` is a struct type, it is a compile-time error for the body to access `this`.</span></span> <span data-ttu-id="08161-2419">这是 true 的访问是否是显式 (如`this.x`) 或隐式 (如`x`其中`x`是结构的一个实例成员)。</span><span class="sxs-lookup"><span data-stu-id="08161-2419">This is true whether the access is explicit (as in `this.x`) or implicit (as in `x` where `x` is an instance member of the struct).</span></span> <span data-ttu-id="08161-2420">此规则只需禁止此类访问权限并不会影响是否成员查找导致该结构的成员。</span><span class="sxs-lookup"><span data-stu-id="08161-2420">This rule simply prohibits such access and does not affect whether member lookup results in a member of the struct.</span></span>
*  <span data-ttu-id="08161-2421">正文有权访问的外部变量 ([外部变量](expressions.md#outer-variables)) 的匿名函数。</span><span class="sxs-lookup"><span data-stu-id="08161-2421">The body has access to the outer variables ([Outer variables](expressions.md#outer-variables)) of the anonymous function.</span></span> <span data-ttu-id="08161-2422">外部变量的访问将引用的变量时处于活动状态的实例*lambda_expression*或*anonymous_method_expression*计算 ([的评估匿名函数表达式](expressions.md#evaluation-of-anonymous-function-expressions))。</span><span class="sxs-lookup"><span data-stu-id="08161-2422">Access of an outer variable will reference the instance of the variable that is active at the time the *lambda_expression* or *anonymous_method_expression* is evaluated ([Evaluation of anonymous function expressions](expressions.md#evaluation-of-anonymous-function-expressions)).</span></span>
*  <span data-ttu-id="08161-2423">它是正文中所含的编译时错误`goto`语句中，`break`语句，或`continue`其目标为表体外部或包含匿名函数主体内的语句。</span><span class="sxs-lookup"><span data-stu-id="08161-2423">It is a compile-time error for the body to contain a `goto` statement, `break` statement, or `continue` statement whose target is outside the body or within the body of a contained anonymous function.</span></span>
*  <span data-ttu-id="08161-2424">一个`return`在正文中的语句将控制权返回从最近的封闭的调用不是来自封闭函数成员的匿名函数。</span><span class="sxs-lookup"><span data-stu-id="08161-2424">A `return` statement in the body returns control from an invocation of the nearest enclosing anonymous function, not from the enclosing function member.</span></span> <span data-ttu-id="08161-2425">中指定的表达式`return`语句必须是隐式转换为委托类型或为表达式树类型的返回类型的最近的封闭*lambda_expression*或*anonymous_method_expression*转换 ([匿名函数转换](conversions.md#anonymous-function-conversions))。</span><span class="sxs-lookup"><span data-stu-id="08161-2425">An expression specified in a `return` statement must be implicitly convertible to the return type of the delegate type or expression tree type to which the nearest enclosing *lambda_expression* or *anonymous_method_expression* is converted ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="08161-2426">是否没有任何方法来执行通过评估和调用的匿名函数以外的其他块没有显式指定*lambda_expression*或*anonymous_method_expression*.</span><span class="sxs-lookup"><span data-stu-id="08161-2426">It is explicitly unspecified whether there is any way to execute the block of an anonymous function other than through evaluation and invocation of the *lambda_expression* or *anonymous_method_expression*.</span></span> <span data-ttu-id="08161-2427">具体而言，编译器可以选择实现一个匿名函数，通过合成一个或多个命名方法或类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2427">In particular, the compiler may choose to implement an anonymous function by synthesizing one or more named methods or types.</span></span> <span data-ttu-id="08161-2428">合成的任何此类元素的名称必须是保留供编译器使用的窗体。</span><span class="sxs-lookup"><span data-stu-id="08161-2428">The names of any such synthesized elements must be of a form reserved for compiler use.</span></span>

### <a name="overload-resolution-and-anonymous-functions"></a><span data-ttu-id="08161-2429">重载决策和匿名函数</span><span class="sxs-lookup"><span data-stu-id="08161-2429">Overload resolution and anonymous functions</span></span>

<span data-ttu-id="08161-2430">参数列表中的匿名函数参与类型推理和重载决策。</span><span class="sxs-lookup"><span data-stu-id="08161-2430">Anonymous functions in an argument list participate in type inference and overload resolution.</span></span> <span data-ttu-id="08161-2431">请参阅[类型推理](expressions.md#type-inference)并[重载决策](expressions.md#overload-resolution)的确切规则。</span><span class="sxs-lookup"><span data-stu-id="08161-2431">Please refer to [Type inference](expressions.md#type-inference) and [Overload resolution](expressions.md#overload-resolution) for the exact rules.</span></span>

<span data-ttu-id="08161-2432">下面的示例说明了匿名函数对重载决策的影响。</span><span class="sxs-lookup"><span data-stu-id="08161-2432">The following example illustrates the effect of anonymous functions on overload resolution.</span></span>

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

<span data-ttu-id="08161-2433">`ItemList<T>`类具有两个`Sum`方法。</span><span class="sxs-lookup"><span data-stu-id="08161-2433">The `ItemList<T>` class has two `Sum` methods.</span></span> <span data-ttu-id="08161-2434">每个都采用`selector`参数，从提取的值为 sum 通过列表项。</span><span class="sxs-lookup"><span data-stu-id="08161-2434">Each takes a `selector` argument, which extracts the value to sum over from a list item.</span></span> <span data-ttu-id="08161-2435">提取的值可以是`int`或`double`并将生成的和为同样`int`或`double`。</span><span class="sxs-lookup"><span data-stu-id="08161-2435">The extracted value can be either an `int` or a `double` and the resulting sum is likewise either an `int` or a `double`.</span></span>

<span data-ttu-id="08161-2436">`Sum`方法为例无法用于计算的详细信息行的顺序从列表的总和。</span><span class="sxs-lookup"><span data-stu-id="08161-2436">The `Sum` methods could for example be used to compute sums from a list of detail lines in an order.</span></span>

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

<span data-ttu-id="08161-2437">中的第一个调用`orderDetails.Sum`，这两个`Sum`方法都适用因为匿名函数`d => d. UnitCount`两个兼容`Func<Detail,int>`和`Func<Detail,double>`。</span><span class="sxs-lookup"><span data-stu-id="08161-2437">In the first invocation of `orderDetails.Sum`, both `Sum` methods are applicable because the anonymous function `d => d. UnitCount` is compatible with both `Func<Detail,int>` and `Func<Detail,double>`.</span></span> <span data-ttu-id="08161-2438">不过，重载决策会选取第一个`Sum`方法因为转换`Func<Detail,int>`转换为更好`Func<Detail,double>`。</span><span class="sxs-lookup"><span data-stu-id="08161-2438">However, overload resolution picks the first `Sum` method because the conversion to `Func<Detail,int>` is better than the conversion to `Func<Detail,double>`.</span></span>

<span data-ttu-id="08161-2439">在第二个调用中`orderDetails.Sum`，仅第二`Sum`方法是适用因为匿名函数`d => d.UnitPrice * d.UnitCount`产生的值的类型`double`。</span><span class="sxs-lookup"><span data-stu-id="08161-2439">In the second invocation of `orderDetails.Sum`, only the second `Sum` method is applicable because the anonymous function `d => d.UnitPrice * d.UnitCount` produces a value of type `double`.</span></span> <span data-ttu-id="08161-2440">因此，重载解析选取第二个`Sum`对于该调用的方法。</span><span class="sxs-lookup"><span data-stu-id="08161-2440">Thus, overload resolution picks the second `Sum` method for that invocation.</span></span>

### <a name="anonymous-functions-and-dynamic-binding"></a><span data-ttu-id="08161-2441">匿名函数和动态绑定</span><span class="sxs-lookup"><span data-stu-id="08161-2441">Anonymous functions and dynamic binding</span></span>

<span data-ttu-id="08161-2442">接收方、 参数或动态绑定运算的操作数，不能为匿名函数。</span><span class="sxs-lookup"><span data-stu-id="08161-2442">An anonymous function cannot be a receiver, argument or operand of a dynamically bound operation.</span></span>

### <a name="outer-variables"></a><span data-ttu-id="08161-2443">外部变量</span><span class="sxs-lookup"><span data-stu-id="08161-2443">Outer variables</span></span>

<span data-ttu-id="08161-2444">任何本地变量、 值参数或其作用域包括参数数组*lambda_expression*或*anonymous_method_expression*称为***外部变量***匿名函数。</span><span class="sxs-lookup"><span data-stu-id="08161-2444">Any local variable, value parameter, or parameter array whose scope includes the *lambda_expression* or *anonymous_method_expression* is called an ***outer variable*** of the anonymous function.</span></span> <span data-ttu-id="08161-2445">中的一个类，实例函数成员`this`值被视为值参数，并且函数成员内包含任何匿名函数的外部变量。</span><span class="sxs-lookup"><span data-stu-id="08161-2445">In an instance function member of a class, the `this` value is considered a value parameter and is an outer variable of any anonymous function contained within the function member.</span></span>

#### <a name="captured-outer-variables"></a><span data-ttu-id="08161-2446">捕获的外部变量</span><span class="sxs-lookup"><span data-stu-id="08161-2446">Captured outer variables</span></span>

<span data-ttu-id="08161-2447">当匿名函数引用的外部变量时，则称外部变量已被***捕获***匿名函数。</span><span class="sxs-lookup"><span data-stu-id="08161-2447">When an outer variable is referenced by an anonymous function, the outer variable is said to have been ***captured*** by the anonymous function.</span></span> <span data-ttu-id="08161-2448">通常，本地变量的生存期仅限于执行块或与之关联的语句 ([局部变量](variables.md#local-variables))。</span><span class="sxs-lookup"><span data-stu-id="08161-2448">Ordinarily, the lifetime of a local variable is limited to execution of the block or statement with which it is associated ([Local variables](variables.md#local-variables)).</span></span> <span data-ttu-id="08161-2449">但是，捕获的外部变量的生存期至少延期到委托或表达式树中创建的匿名函数变得可以进行垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="08161-2449">However, the lifetime of a captured outer variable is extended at least until the delegate or expression tree created from the anonymous function becomes eligible for garbage collection.</span></span>

<span data-ttu-id="08161-2450">在示例</span><span class="sxs-lookup"><span data-stu-id="08161-2450">In the example</span></span>
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
<span data-ttu-id="08161-2451">本地变量`x`捕获的匿名函数和的生命周期`x`至少从返回的委托扩展`F`变得垃圾回收 （这不会发生的最后才符合条件程序）。</span><span class="sxs-lookup"><span data-stu-id="08161-2451">the local variable `x` is captured by the anonymous function, and the lifetime of `x` is extended at least until the delegate returned from `F` becomes eligible for garbage collection (which doesn't happen until the very end of the program).</span></span> <span data-ttu-id="08161-2452">由于匿名函数的每个调用对在同一个实例`x`，该示例的输出是：</span><span class="sxs-lookup"><span data-stu-id="08161-2452">Since each invocation of the anonymous function operates on the same instance of `x`, the output of the example is:</span></span>
```
1
2
3
```

<span data-ttu-id="08161-2453">当匿名函数捕获本地变量或值参数时，局部变量或参数被不再视为可为固定的变量 ([固定和可移动变量](unsafe-code.md#fixed-and-moveable-variables))，而是被视为可可移动但变量。</span><span class="sxs-lookup"><span data-stu-id="08161-2453">When a local variable or a value parameter is captured by an anonymous function, the local variable or parameter is no longer considered to be a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), but is instead considered to be a moveable variable.</span></span> <span data-ttu-id="08161-2454">因此任何`unsafe`代码，用来捕获的外部变量的地址必须首先使用`fixed`固定该变量的语句。</span><span class="sxs-lookup"><span data-stu-id="08161-2454">Thus any `unsafe` code that takes the address of a captured outer variable must first use the `fixed` statement to fix the variable.</span></span>

<span data-ttu-id="08161-2455">请注意，与 uncaptured 变量不同，可以向多个执行线程同时公开捕获本地变量。</span><span class="sxs-lookup"><span data-stu-id="08161-2455">Note that unlike an uncaptured variable, a captured local variable can be simultaneously exposed to multiple threads of execution.</span></span>

#### <a name="instantiation-of-local-variables"></a><span data-ttu-id="08161-2456">本地变量的实例化</span><span class="sxs-lookup"><span data-stu-id="08161-2456">Instantiation of local variables</span></span>

<span data-ttu-id="08161-2457">本地变量被视为***实例化***时执行进入变量的作用域。</span><span class="sxs-lookup"><span data-stu-id="08161-2457">A local variable is considered to be ***instantiated*** when execution enters the scope of the variable.</span></span> <span data-ttu-id="08161-2458">例如，以下方法调用时，本地变量`x`进行实例化和初始化三次，每个迭代的循环一次。</span><span class="sxs-lookup"><span data-stu-id="08161-2458">For example, when the following method is invoked, the local variable `x` is instantiated and initialized three times—once for each iteration of the loop.</span></span>

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="08161-2459">但是，移动的声明`x`之外的单一实例化此循环导致`x`:</span><span class="sxs-lookup"><span data-stu-id="08161-2459">However, moving the declaration of `x` outside the loop results in a single instantiation of `x`:</span></span>
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="08161-2460">时不会捕获，没有方法来观察完全何种频率进行实例化本地变量，因为实例化生存期是连续的就可以为每个实例化，只需使用相同的存储位置。</span><span class="sxs-lookup"><span data-stu-id="08161-2460">When not captured, there is no way to observe exactly how often a local variable is instantiated—because the lifetimes of the instantiations are disjoint, it is possible for each instantiation to simply use the same storage location.</span></span> <span data-ttu-id="08161-2461">但是，当一个匿名函数捕获本地变量时，实例化的效果很明显。</span><span class="sxs-lookup"><span data-stu-id="08161-2461">However, when an anonymous function captures a local variable, the effects of instantiation become apparent.</span></span>

<span data-ttu-id="08161-2462">该示例</span><span class="sxs-lookup"><span data-stu-id="08161-2462">The example</span></span>
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
<span data-ttu-id="08161-2463">生成输出：</span><span class="sxs-lookup"><span data-stu-id="08161-2463">produces the output:</span></span>
```
1
3
5
```

<span data-ttu-id="08161-2464">但是，当声明`x`移出循环：</span><span class="sxs-lookup"><span data-stu-id="08161-2464">However, when the declaration of `x` is moved outside the loop:</span></span>
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
<span data-ttu-id="08161-2465">输出为：</span><span class="sxs-lookup"><span data-stu-id="08161-2465">the output is:</span></span>
```
5
5
5
```

<span data-ttu-id="08161-2466">如果 for 循环中声明的迭代变量，该变量本身是被视为位于循环外声明。</span><span class="sxs-lookup"><span data-stu-id="08161-2466">If a for-loop declares an iteration variable, that variable itself is considered to be declared outside of the loop.</span></span> <span data-ttu-id="08161-2467">因此，如果该示例更改要捕获的迭代变量本身：</span><span class="sxs-lookup"><span data-stu-id="08161-2467">Thus, if the example is changed to capture the iteration variable itself:</span></span>

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
<span data-ttu-id="08161-2468">捕获的迭代变量只有一个实例，这会生成输出：</span><span class="sxs-lookup"><span data-stu-id="08161-2468">only one instance of the iteration variable is captured, which produces the output:</span></span>
```
3
3
3
```

<span data-ttu-id="08161-2469">它是匿名函数委托可共享一些捕获的变量，但是又有其他人的单独实例。</span><span class="sxs-lookup"><span data-stu-id="08161-2469">It is possible for anonymous function delegates to share some captured variables yet have separate instances of others.</span></span> <span data-ttu-id="08161-2470">例如，如果`F`更改为</span><span class="sxs-lookup"><span data-stu-id="08161-2470">For example, if `F` is changed to</span></span>
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
<span data-ttu-id="08161-2471">三个委托捕获的同一个实例`x`但的单个实例`y`，而输出是：</span><span class="sxs-lookup"><span data-stu-id="08161-2471">the three delegates capture the same instance of `x` but separate instances of `y`, and the output is:</span></span>
```
1 1
2 1
3 1
```

<span data-ttu-id="08161-2472">单独的匿名函数可以捕获一个外部变量的同一个实例。</span><span class="sxs-lookup"><span data-stu-id="08161-2472">Separate anonymous functions can capture the same instance of an outer variable.</span></span> <span data-ttu-id="08161-2473">在下面的示例中：</span><span class="sxs-lookup"><span data-stu-id="08161-2473">In the example:</span></span>
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
<span data-ttu-id="08161-2474">两个匿名函数捕获本地变量的同一实例`x`，并且它们可以因此"通信"通过该变量。</span><span class="sxs-lookup"><span data-stu-id="08161-2474">the two anonymous functions capture the same instance of the local variable `x`, and they can thus "communicate" through that variable.</span></span> <span data-ttu-id="08161-2475">该示例的输出是：</span><span class="sxs-lookup"><span data-stu-id="08161-2475">The output of the example is:</span></span>
```
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a><span data-ttu-id="08161-2476">匿名函数表达式的计算</span><span class="sxs-lookup"><span data-stu-id="08161-2476">Evaluation of anonymous function expressions</span></span>

<span data-ttu-id="08161-2477">匿名函数`F`必须始终转换为委托类型`D`或表达式树类型`E`，直接或通过执行委托创建表达式`new D(F)`。</span><span class="sxs-lookup"><span data-stu-id="08161-2477">An anonymous function `F` must always be converted to a delegate type `D` or an expression tree type `E`, either directly or through the execution of a delegate creation expression `new D(F)`.</span></span> <span data-ttu-id="08161-2478">此转换确定的匿名函数的结果，如中所述[匿名函数转换](conversions.md#anonymous-function-conversions)。</span><span class="sxs-lookup"><span data-stu-id="08161-2478">This conversion determines the result of the anonymous function, as described in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>

## <a name="query-expressions"></a><span data-ttu-id="08161-2479">查询表达式</span><span class="sxs-lookup"><span data-stu-id="08161-2479">Query expressions</span></span>

<span data-ttu-id="08161-2480">***查询表达式***提供语言集成语法的查询，类似于关系和层次查询语言，如 SQL 和 XQuery。</span><span class="sxs-lookup"><span data-stu-id="08161-2480">***Query expressions*** provide a language integrated syntax for queries that is similar to relational and hierarchical query languages such as SQL and XQuery.</span></span>

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

<span data-ttu-id="08161-2481">查询表达式开头`from`子句和任一结尾`select`或`group`子句。</span><span class="sxs-lookup"><span data-stu-id="08161-2481">A query expression begins with a `from` clause and ends with either a `select` or `group` clause.</span></span> <span data-ttu-id="08161-2482">初始`from`子句可以后跟零个或多`from`， `let`， `where`，`join`或`orderby`子句。</span><span class="sxs-lookup"><span data-stu-id="08161-2482">The initial `from` clause can be followed by zero or more `from`, `let`, `where`, `join` or `orderby` clauses.</span></span> <span data-ttu-id="08161-2483">每个`from`子句是一个生成器简介***范围变量***的元素范围***序列***。</span><span class="sxs-lookup"><span data-stu-id="08161-2483">Each `from` clause is a generator introducing a ***range variable*** which ranges over the elements of a ***sequence***.</span></span> <span data-ttu-id="08161-2484">每个`let`子句引入范围变量表示计算通过之前的范围变量的值。</span><span class="sxs-lookup"><span data-stu-id="08161-2484">Each `let` clause introduces a range variable representing a value computed by means of previous range variables.</span></span> <span data-ttu-id="08161-2485">每个`where`子句是从结果中排除项的筛选器。</span><span class="sxs-lookup"><span data-stu-id="08161-2485">Each `where` clause is a filter that excludes items from the result.</span></span> <span data-ttu-id="08161-2486">每个`join`子句将指定的源序列中的另一个序列，无法完成匹配对的键的键进行比较。</span><span class="sxs-lookup"><span data-stu-id="08161-2486">Each `join` clause compares specified keys of the source sequence with keys of another sequence, yielding matching pairs.</span></span> <span data-ttu-id="08161-2487">每个`orderby`子句会根据指定条件的项进行重新排序。最终`select`或`group`子句指定结果的范围变量的形状。</span><span class="sxs-lookup"><span data-stu-id="08161-2487">Each `orderby` clause reorders items according to specified criteria.The final `select` or `group` clause specifies the shape of the result in terms of the range variables.</span></span> <span data-ttu-id="08161-2488">最后，`into`子句可用于"接合"查询通过将视为一个生成器的后续查询中的一个查询的结果。</span><span class="sxs-lookup"><span data-stu-id="08161-2488">Finally, an `into` clause can be used to "splice" queries by treating the results of one query as a generator in a subsequent query.</span></span>

### <a name="ambiguities-in-query-expressions"></a><span data-ttu-id="08161-2489">在查询表达式中的多义性</span><span class="sxs-lookup"><span data-stu-id="08161-2489">Ambiguities in query expressions</span></span>

<span data-ttu-id="08161-2490">查询表达式包含大量的"上下文关键字"，即，在给定上下文中具有特殊含义的标识符。</span><span class="sxs-lookup"><span data-stu-id="08161-2490">Query expressions contain a number of "contextual keywords", i.e., identifiers that have special meaning in a given context.</span></span> <span data-ttu-id="08161-2491">特别是这些`from`， `where`， `join`， `on`， `equals`， `into`， `let`， `orderby`， `ascending`， `descending`， `select`，`group`和`by`.</span><span class="sxs-lookup"><span data-stu-id="08161-2491">Specifically these are `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` and `by`.</span></span> <span data-ttu-id="08161-2492">为了避免在为关键字或简单名称引起混合使用这些标识符的查询表达式中的多义性，这些标识符被视为关键字时查询表达式中任何位置发生。</span><span class="sxs-lookup"><span data-stu-id="08161-2492">In order to avoid ambiguities in query expressions caused by mixed use of these identifiers as keywords or simple names, these identifiers are considered keywords when occurring anywhere within a query expression.</span></span>

<span data-ttu-id="08161-2493">为此，查询表达式是开头的任何表达式"`from identifier`"后跟除外的任何令牌"`;`"，"`=`"或"`,`"。</span><span class="sxs-lookup"><span data-stu-id="08161-2493">For this purpose, a query expression is any expression that starts with "`from identifier`" followed by any token except "`;`", "`=`" or "`,`".</span></span>

<span data-ttu-id="08161-2494">若要使用这些字作为标识符中的查询表达式，它们可以前缀为"`@`"([标识符](lexical-structure.md#identifiers))。</span><span class="sxs-lookup"><span data-stu-id="08161-2494">In order to use these words as identifiers within a query expression, they can be prefixed with "`@`" ([Identifiers](lexical-structure.md#identifiers)).</span></span>

### <a name="query-expression-translation"></a><span data-ttu-id="08161-2495">在查询表达式转换</span><span class="sxs-lookup"><span data-stu-id="08161-2495">Query expression translation</span></span>

<span data-ttu-id="08161-2496">C# 语言不会指定查询表达式的执行语义。</span><span class="sxs-lookup"><span data-stu-id="08161-2496">The C# language does not specify the execution semantics of query expressions.</span></span> <span data-ttu-id="08161-2497">相反，查询表达式转换为调用的方法的遵守*查询表达式模式*([查询表达式模式](expressions.md#the-query-expression-pattern))。</span><span class="sxs-lookup"><span data-stu-id="08161-2497">Rather, query expressions are translated into invocations of methods that adhere to the *query expression pattern* ([The query expression pattern](expressions.md#the-query-expression-pattern)).</span></span> <span data-ttu-id="08161-2498">具体而言，查询表达式转换为调用的方法名为`Where`， `Select`， `SelectMany`， `Join`， `GroupJoin`， `OrderBy`， `OrderByDescending`， `ThenBy`， `ThenByDescending`， `GroupBy`，和`Cast`。这些方法应具有特定签名和结果类型，如中所述[查询表达式模式](expressions.md#the-query-expression-pattern)。</span><span class="sxs-lookup"><span data-stu-id="08161-2498">Specifically, query expressions are translated into invocations of methods named `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, and `Cast`.These methods are expected to have particular signatures and result types, as described in [The query expression pattern](expressions.md#the-query-expression-pattern).</span></span> <span data-ttu-id="08161-2499">这些方法可以是正在查询的对象的实例方法或外部的对象的扩展方法，实现实际执行查询。</span><span class="sxs-lookup"><span data-stu-id="08161-2499">These methods can be instance methods of the object being queried or extension methods that are external to the object, and they implement the actual execution of the query.</span></span>

<span data-ttu-id="08161-2500">从查询表达式转换为方法调用是任何类型绑定之前发生的语法映射或已执行重载决策。</span><span class="sxs-lookup"><span data-stu-id="08161-2500">The translation from query expressions to method invocations is a syntactic mapping that occurs before any type binding or overload resolution has been performed.</span></span> <span data-ttu-id="08161-2501">翻译保证语法正确，但不是保证能够产生在语义上是正确的 C# 代码。</span><span class="sxs-lookup"><span data-stu-id="08161-2501">The translation is guaranteed to be syntactically correct, but it is not guaranteed to produce semantically correct C# code.</span></span> <span data-ttu-id="08161-2502">查询表达式的转换时，生成的方法调用处理为正则方法调用，且这又可能会发现错误，例如，如果方法不存在，如果自变量具有错误类型，或如果方法是泛型和类型推理失败。</span><span class="sxs-lookup"><span data-stu-id="08161-2502">Following translation of query expressions, the resulting method invocations are processed as regular method invocations, and this may in turn uncover errors, for example if the methods do not exist, if arguments have wrong types, or if the methods are generic and type inference fails.</span></span>

<span data-ttu-id="08161-2503">查询表达式由重复应用以下转换，直到没有更多降低是两个可能进行处理。</span><span class="sxs-lookup"><span data-stu-id="08161-2503">A query expression is processed by repeatedly applying the following translations until no further reductions are possible.</span></span> <span data-ttu-id="08161-2504">翻译中应用程序的顺序列出： 每个部分假定已彻底，执行前面部分中的翻译并后已用完，一个部分将不更高版本再次讨论在处理同一个查询表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-2504">The translations are listed in order of application: each section assumes that the translations in the preceding sections have been performed exhaustively, and once exhausted, a section will not later be revisited in the processing of the same query expression.</span></span>

<span data-ttu-id="08161-2505">查询表达式中不允许的范围变量赋值。</span><span class="sxs-lookup"><span data-stu-id="08161-2505">Assignment to range variables is not allowed in query expressions.</span></span> <span data-ttu-id="08161-2506">但是允许的 C# 实现并非始终强制执行这种限制，因为这有时不可能与此处介绍的在语法上转换方案。</span><span class="sxs-lookup"><span data-stu-id="08161-2506">However a C# implementation is permitted to not always enforce this restriction, since this may sometimes not be possible with the syntactic translation scheme presented here.</span></span>

<span data-ttu-id="08161-2507">某些转换为由透明标识符将插入的范围变量`*`。</span><span class="sxs-lookup"><span data-stu-id="08161-2507">Certain translations inject range variables with transparent identifiers denoted by `*`.</span></span> <span data-ttu-id="08161-2508">透明标识符的特殊属性进行讨论中进一步[透明标识符](expressions.md#transparent-identifiers)。</span><span class="sxs-lookup"><span data-stu-id="08161-2508">The special properties of transparent identifiers are discussed further in [Transparent identifiers](expressions.md#transparent-identifiers).</span></span>

#### <a name="select-and-groupby-clauses-with-continuations"></a><span data-ttu-id="08161-2509">使用继续符的选择和 groupby 子句</span><span class="sxs-lookup"><span data-stu-id="08161-2509">Select and groupby clauses with continuations</span></span>

<span data-ttu-id="08161-2510">查询表达式中使用继续符</span><span class="sxs-lookup"><span data-stu-id="08161-2510">A query expression with a continuation</span></span>
```csharp
from ... into x ...
```
<span data-ttu-id="08161-2511">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2511">is translated into</span></span>
```csharp
from x in ( from ... ) ...
```

<span data-ttu-id="08161-2512">以下各节中的翻译假定查询没有任何`into`继续符。</span><span class="sxs-lookup"><span data-stu-id="08161-2512">The translations in the following sections assume that queries have no `into` continuations.</span></span>

<span data-ttu-id="08161-2513">该示例</span><span class="sxs-lookup"><span data-stu-id="08161-2513">The example</span></span>
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="08161-2514">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2514">is translated into</span></span>
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="08161-2515">最后一个转换是</span><span class="sxs-lookup"><span data-stu-id="08161-2515">the final translation of which is</span></span>
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a><span data-ttu-id="08161-2516">显式范围变量类型</span><span class="sxs-lookup"><span data-stu-id="08161-2516">Explicit range variable types</span></span>

<span data-ttu-id="08161-2517">一个`from`子句显式指定范围变量类型</span><span class="sxs-lookup"><span data-stu-id="08161-2517">A `from` clause that explicitly specifies a range variable type</span></span>
```csharp
from T x in e
```
<span data-ttu-id="08161-2518">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2518">is translated into</span></span>
```csharp
from x in ( e ) . Cast < T > ( )
```

<span data-ttu-id="08161-2519">一个`join`子句显式指定范围变量类型</span><span class="sxs-lookup"><span data-stu-id="08161-2519">A `join` clause that explicitly specifies a range variable type</span></span>
```
join T x in e on k1 equals k2
```
<span data-ttu-id="08161-2520">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2520">is translated into</span></span>
```
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

<span data-ttu-id="08161-2521">以下各节中的翻译假定查询有无显式范围变量的类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2521">The translations in the following sections assume that queries have no explicit range variable types.</span></span>

<span data-ttu-id="08161-2522">该示例</span><span class="sxs-lookup"><span data-stu-id="08161-2522">The example</span></span>
```csharp
from Customer c in customers
where c.City == "London"
select c
```
<span data-ttu-id="08161-2523">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2523">is translated into</span></span>
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
<span data-ttu-id="08161-2524">最后一个转换是</span><span class="sxs-lookup"><span data-stu-id="08161-2524">the final translation of which is</span></span>
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

<span data-ttu-id="08161-2525">显式范围变量的类型可用于查询实现非泛型的集合`IEnumerable`接口，但不是通用`IEnumerable<T>`接口。</span><span class="sxs-lookup"><span data-stu-id="08161-2525">Explicit range variable types are useful for querying collections that implement the non-generic `IEnumerable` interface, but not the generic `IEnumerable<T>` interface.</span></span> <span data-ttu-id="08161-2526">在上面的示例中，这是如果`customers`类型的`ArrayList`。</span><span class="sxs-lookup"><span data-stu-id="08161-2526">In the example above, this would be the case if `customers` were of type `ArrayList`.</span></span>

#### <a name="degenerate-query-expressions"></a><span data-ttu-id="08161-2527">退化的查询表达式</span><span class="sxs-lookup"><span data-stu-id="08161-2527">Degenerate query expressions</span></span>

<span data-ttu-id="08161-2528">查询表达式中的窗体</span><span class="sxs-lookup"><span data-stu-id="08161-2528">A query expression of the form</span></span>
```csharp
from x in e select x
```
<span data-ttu-id="08161-2529">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2529">is translated into</span></span>
```csharp
( e ) . Select ( x => x )
```

<span data-ttu-id="08161-2530">该示例</span><span class="sxs-lookup"><span data-stu-id="08161-2530">The example</span></span>
```csharp
from c in customers
select c
```
<span data-ttu-id="08161-2531">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2531">is translated into</span></span>
```csharp
customers.Select(c => c)
```

<span data-ttu-id="08161-2532">退化的查询表达式是指不费力地选择源的元素。</span><span class="sxs-lookup"><span data-stu-id="08161-2532">A degenerate query expression is one that trivially selects the elements of the source.</span></span> <span data-ttu-id="08161-2533">翻译的后面的阶段中删除引入的其他转换步骤通过将替换其源的退化查询。</span><span class="sxs-lookup"><span data-stu-id="08161-2533">A later phase of the translation removes degenerate queries introduced by other translation steps by replacing them with their source.</span></span> <span data-ttu-id="08161-2534">它很重要，确保查询的结果表达式是永远不会是源对象本身，但因为的到客户端的查询显示的类型和源的标识。</span><span class="sxs-lookup"><span data-stu-id="08161-2534">It is important however to ensure that the result of a query expression is never the source object itself, as that would reveal the type and identity of the source to the client of the query.</span></span> <span data-ttu-id="08161-2535">因此，此步骤保护退化通过显式调用编写直接在源代码中的查询`Select`源上。</span><span class="sxs-lookup"><span data-stu-id="08161-2535">Therefore this step protects degenerate queries written directly in source code by explicitly calling `Select` on the source.</span></span> <span data-ttu-id="08161-2536">这样，它将最多的实施者`Select`和其他查询运算符，以确保这些方法永远不会返回的源对象本身。</span><span class="sxs-lookup"><span data-stu-id="08161-2536">It is then up to the implementers of `Select` and other query operators to ensure that these methods never return the source object itself.</span></span>

#### <a name="from-let-where-join-and-orderby-clauses"></a><span data-ttu-id="08161-2537">From、 let、 where、 join 和 orderby 子句</span><span class="sxs-lookup"><span data-stu-id="08161-2537">From, let, where, join and orderby clauses</span></span>

<span data-ttu-id="08161-2538">与第二个查询表达式`from`子句跟`select`子句</span><span class="sxs-lookup"><span data-stu-id="08161-2538">A query expression with a second `from` clause followed by a `select` clause</span></span>
```csharp
from x1 in e1
from x2 in e2
select v
```
<span data-ttu-id="08161-2539">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2539">is translated into</span></span>
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="08161-2540">与第二个查询表达式`from`子句而不遵循由`select`子句：</span><span class="sxs-lookup"><span data-stu-id="08161-2540">A query expression with a second `from` clause followed by something other than a `select` clause:</span></span>

```csharp
from x1 in e1
from x2 in e2
...
```
<span data-ttu-id="08161-2541">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2541">is translated into</span></span>
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

<span data-ttu-id="08161-2542">查询表达式与`let`子句</span><span class="sxs-lookup"><span data-stu-id="08161-2542">A query expression with a `let` clause</span></span>
```csharp
from x in e
let y = f
...
```
<span data-ttu-id="08161-2543">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2543">is translated into</span></span>
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

<span data-ttu-id="08161-2544">查询表达式与`where`子句</span><span class="sxs-lookup"><span data-stu-id="08161-2544">A query expression with a `where` clause</span></span>
```csharp
from x in e
where f
...
```
<span data-ttu-id="08161-2545">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2545">is translated into</span></span>
```csharp
from x in ( e ) . Where ( x => f )
...
```

<span data-ttu-id="08161-2546">使用查询表达式`join`子句而无需`into`跟`select`子句</span><span class="sxs-lookup"><span data-stu-id="08161-2546">A query expression with a `join` clause without an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
<span data-ttu-id="08161-2547">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2547">is translated into</span></span>
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="08161-2548">使用查询表达式`join`子句而无需`into`而不遵循由`select`子句</span><span class="sxs-lookup"><span data-stu-id="08161-2548">A query expression with a `join` clause without an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
<span data-ttu-id="08161-2549">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2549">is translated into</span></span>
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

<span data-ttu-id="08161-2550">使用查询表达式`join`子句`into`跟`select`子句</span><span class="sxs-lookup"><span data-stu-id="08161-2550">A query expression with a `join` clause with an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
<span data-ttu-id="08161-2551">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2551">is translated into</span></span>
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

<span data-ttu-id="08161-2552">使用查询表达式`join`子句`into`而不遵循由`select`子句</span><span class="sxs-lookup"><span data-stu-id="08161-2552">A query expression with a `join` clause with an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
<span data-ttu-id="08161-2553">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2553">is translated into</span></span>
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

<span data-ttu-id="08161-2554">查询表达式与`orderby`子句</span><span class="sxs-lookup"><span data-stu-id="08161-2554">A query expression with an `orderby` clause</span></span>
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
<span data-ttu-id="08161-2555">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2555">is translated into</span></span>
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

<span data-ttu-id="08161-2556">如果进行的排序子句指定`descending`方向指示器中，调用`OrderByDescending`或`ThenByDescending`改为生成。</span><span class="sxs-lookup"><span data-stu-id="08161-2556">If an ordering clause specifies a `descending` direction indicator, an invocation of `OrderByDescending` or `ThenByDescending` is produced instead.</span></span>

<span data-ttu-id="08161-2557">下面的翻译假定没有任何`let`， `where`，`join`或`orderby`子句，并且不能超过一个初始`from`中每个查询表达式子句。</span><span class="sxs-lookup"><span data-stu-id="08161-2557">The following translations assume that there are no `let`, `where`, `join` or `orderby` clauses, and no more than the one initial `from` clause in each query expression.</span></span>

<span data-ttu-id="08161-2558">该示例</span><span class="sxs-lookup"><span data-stu-id="08161-2558">The example</span></span>
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="08161-2559">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2559">is translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

<span data-ttu-id="08161-2560">该示例</span><span class="sxs-lookup"><span data-stu-id="08161-2560">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="08161-2561">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2561">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="08161-2562">最后一个转换是</span><span class="sxs-lookup"><span data-stu-id="08161-2562">the final translation of which is</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
<span data-ttu-id="08161-2563">其中`x`是编译器生成的标识符，否则是不可见且不可访问。</span><span class="sxs-lookup"><span data-stu-id="08161-2563">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="08161-2564">该示例</span><span class="sxs-lookup"><span data-stu-id="08161-2564">The example</span></span>
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
<span data-ttu-id="08161-2565">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2565">is translated into</span></span>
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
<span data-ttu-id="08161-2566">最后一个转换是</span><span class="sxs-lookup"><span data-stu-id="08161-2566">the final translation of which is</span></span>
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
<span data-ttu-id="08161-2567">其中`x`是编译器生成的标识符，否则是不可见且不可访问。</span><span class="sxs-lookup"><span data-stu-id="08161-2567">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="08161-2568">该示例</span><span class="sxs-lookup"><span data-stu-id="08161-2568">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
<span data-ttu-id="08161-2569">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2569">is translated into</span></span>
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

<span data-ttu-id="08161-2570">该示例</span><span class="sxs-lookup"><span data-stu-id="08161-2570">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="08161-2571">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2571">is translated into</span></span>
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="08161-2572">最后一个转换是</span><span class="sxs-lookup"><span data-stu-id="08161-2572">the final translation of which is</span></span>
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
<span data-ttu-id="08161-2573">其中`x`和`y`是编译器生成的标识符，否则是不可见且不可访问。</span><span class="sxs-lookup"><span data-stu-id="08161-2573">where `x` and `y` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="08161-2574">该示例</span><span class="sxs-lookup"><span data-stu-id="08161-2574">The example</span></span>
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
<span data-ttu-id="08161-2575">具有最终的转换</span><span class="sxs-lookup"><span data-stu-id="08161-2575">has the final translation</span></span>
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a><span data-ttu-id="08161-2576">Select 子句</span><span class="sxs-lookup"><span data-stu-id="08161-2576">Select clauses</span></span>

<span data-ttu-id="08161-2577">查询表达式中的窗体</span><span class="sxs-lookup"><span data-stu-id="08161-2577">A query expression of the form</span></span>
```csharp
from x in e select v
```
<span data-ttu-id="08161-2578">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2578">is translated into</span></span>
```csharp
( e ) . Select ( x => v )
```
<span data-ttu-id="08161-2579">除非 v 是标识符 x，该转换是只需</span><span class="sxs-lookup"><span data-stu-id="08161-2579">except when v is the identifier x, the translation is simply</span></span>
```csharp
( e )
```

<span data-ttu-id="08161-2580">例如</span><span class="sxs-lookup"><span data-stu-id="08161-2580">For example</span></span>
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
<span data-ttu-id="08161-2581">只需转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2581">is simply translated into</span></span>
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a><span data-ttu-id="08161-2582">Groupby 子句</span><span class="sxs-lookup"><span data-stu-id="08161-2582">Groupby clauses</span></span>

<span data-ttu-id="08161-2583">查询表达式中的窗体</span><span class="sxs-lookup"><span data-stu-id="08161-2583">A query expression of the form</span></span>
```csharp
from x in e group v by k
```
<span data-ttu-id="08161-2584">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2584">is translated into</span></span>
```csharp
( e ) . GroupBy ( x => k , x => v )
```
<span data-ttu-id="08161-2585">除非 v 是标识符 x，该转换是</span><span class="sxs-lookup"><span data-stu-id="08161-2585">except when v is the identifier x, the translation is</span></span>
```csharp
( e ) . GroupBy ( x => k )
```

<span data-ttu-id="08161-2586">该示例</span><span class="sxs-lookup"><span data-stu-id="08161-2586">The example</span></span>
```csharp
from c in customers
group c.Name by c.Country
```
<span data-ttu-id="08161-2587">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2587">is translated into</span></span>
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a><span data-ttu-id="08161-2588">透明标识符</span><span class="sxs-lookup"><span data-stu-id="08161-2588">Transparent identifiers</span></span>

<span data-ttu-id="08161-2589">某些翻译注入与范围变量***透明标识符***为由`*`。</span><span class="sxs-lookup"><span data-stu-id="08161-2589">Certain translations inject range variables with ***transparent identifiers*** denoted by `*`.</span></span> <span data-ttu-id="08161-2590">透明标识符不是一种适当的语言功能;它们仅作为中间步骤在查询表达式转换过程中存在。</span><span class="sxs-lookup"><span data-stu-id="08161-2590">Transparent identifiers are not a proper language feature; they exist only as an intermediate step in the query expression translation process.</span></span>

<span data-ttu-id="08161-2591">当查询转换注入透明标识符时，进一步转换步骤会将透明标识符传播的匿名函数和匿名对象初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="08161-2591">When a query translation injects a transparent identifier, further translation steps propagate the transparent identifier into anonymous functions and anonymous object initializers.</span></span> <span data-ttu-id="08161-2592">在这些上下文中，透明标识符具有以下行为：</span><span class="sxs-lookup"><span data-stu-id="08161-2592">In those contexts, transparent identifiers have the following behavior:</span></span>

*  <span data-ttu-id="08161-2593">作为匿名函数中的参数的透明标识符时，关联的匿名类型的成员会自动在匿名函数的主体的范围内。</span><span class="sxs-lookup"><span data-stu-id="08161-2593">When a transparent identifier occurs as a parameter in an anonymous function, the members of the associated anonymous type are automatically in scope in the body of the anonymous function.</span></span>
*  <span data-ttu-id="08161-2594">作用域具有透明标识符成员时，该成员的成员处于范围内以及。</span><span class="sxs-lookup"><span data-stu-id="08161-2594">When a member with a transparent identifier is in scope, the members of that member are in scope as well.</span></span>
*  <span data-ttu-id="08161-2595">作为匿名对象初始值设定项中的成员声明符是透明的标识符时，它引入了一个具有透明标识符成员。</span><span class="sxs-lookup"><span data-stu-id="08161-2595">When a transparent identifier occurs as a member declarator in an anonymous object initializer, it introduces a member with a transparent identifier.</span></span>
*  <span data-ttu-id="08161-2596">在上面所述的转换步骤，以及匿名类型，为单个对象的成员捕获多个范围变量的意向始终介绍透明标识符了。</span><span class="sxs-lookup"><span data-stu-id="08161-2596">In the translation steps described above, transparent identifiers are always introduced together with anonymous types, with the intent of capturing multiple range variables as members of a single object.</span></span> <span data-ttu-id="08161-2597">允许使用匿名类型相比，另一种机制来组合在一起的多个范围变量的 C# 实现。</span><span class="sxs-lookup"><span data-stu-id="08161-2597">An implementation of C# is permitted to use a different mechanism than anonymous types to group together multiple range variables.</span></span> <span data-ttu-id="08161-2598">下面的转换示例假定匿名类型使用，并显示如何透明标识符可以立即转换。</span><span class="sxs-lookup"><span data-stu-id="08161-2598">The following translation examples assume that anonymous types are used, and show how transparent identifiers can be translated away.</span></span>

<span data-ttu-id="08161-2599">该示例</span><span class="sxs-lookup"><span data-stu-id="08161-2599">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
<span data-ttu-id="08161-2600">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2600">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

<span data-ttu-id="08161-2601">这是进一步转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2601">which is further translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
<span data-ttu-id="08161-2602">时将删除透明标识符，其中，是等效于</span><span class="sxs-lookup"><span data-stu-id="08161-2602">which, when transparent identifiers are erased, is equivalent to</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
<span data-ttu-id="08161-2603">其中`x`是编译器生成的标识符，否则是不可见且不可访问。</span><span class="sxs-lookup"><span data-stu-id="08161-2603">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="08161-2604">该示例</span><span class="sxs-lookup"><span data-stu-id="08161-2604">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="08161-2605">转换为</span><span class="sxs-lookup"><span data-stu-id="08161-2605">is translated into</span></span>
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="08161-2606">这是进一步简化为</span><span class="sxs-lookup"><span data-stu-id="08161-2606">which is further reduced to</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
<span data-ttu-id="08161-2607">最后一个转换是</span><span class="sxs-lookup"><span data-stu-id="08161-2607">the final translation of which is</span></span>
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
<span data-ttu-id="08161-2608">其中`x`， `y`，和`z`是编译器生成的标识符，否则是不可见且不可访问。</span><span class="sxs-lookup"><span data-stu-id="08161-2608">where `x`, `y`, and `z` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

### <a name="the-query-expression-pattern"></a><span data-ttu-id="08161-2609">查询表达式模式</span><span class="sxs-lookup"><span data-stu-id="08161-2609">The query expression pattern</span></span>

<span data-ttu-id="08161-2610">***查询表达式模式***建立一种模式的类型可以实现以支持查询表达式的方法。</span><span class="sxs-lookup"><span data-stu-id="08161-2610">The ***Query expression pattern*** establishes a pattern of methods that types can implement to support query expressions.</span></span> <span data-ttu-id="08161-2611">通过语法映射情况下，查询表达式转换为方法调用，因为类型具有相当大的灵活性，在查询表达式模式的实现方式。</span><span class="sxs-lookup"><span data-stu-id="08161-2611">Because query expressions are translated to method invocations by means of a syntactic mapping, types have considerable flexibility in how they implement the query expression pattern.</span></span> <span data-ttu-id="08161-2612">例如，模式的方法可以实现作为实例方法或扩展方法这两个具有相同的调用语法，因此方法可以请求委托或表达式树，因为匿名函数都可以转换为两者。</span><span class="sxs-lookup"><span data-stu-id="08161-2612">For example, the methods of the pattern can be implemented as instance methods or as extension methods because the two have the same invocation syntax, and the methods can request delegates or expression trees because anonymous functions are convertible to both.</span></span>

<span data-ttu-id="08161-2613">泛型类型的建议的形状`C<T>`，支持的查询表达式模式如下所示。</span><span class="sxs-lookup"><span data-stu-id="08161-2613">The recommended shape of a generic type `C<T>` that supports the query expression pattern is shown below.</span></span> <span data-ttu-id="08161-2614">为了说明正确的参数和结果类型之间的关系使用的泛型类型，但可以实现非泛型类型的模式。</span><span class="sxs-lookup"><span data-stu-id="08161-2614">A generic type is used in order to illustrate the proper relationships between parameter and result types, but it is possible to implement the pattern for non-generic types as well.</span></span>

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

<span data-ttu-id="08161-2615">上述两种方法使用泛型委托类型`Func<T1,R>`和`Func<T1,T2,R>`，但它们也同样也可以使用其他委托或表达式树类型使用中的参数和结果类型相同的关系。</span><span class="sxs-lookup"><span data-stu-id="08161-2615">The methods above use the generic delegate types `Func<T1,R>` and `Func<T1,T2,R>`, but they could equally well have used other delegate or expression tree types with the same relationships in parameter and result types.</span></span>

<span data-ttu-id="08161-2616">请注意之间的建议的关系`C<T>`并`O<T>`这可确保`ThenBy`并`ThenByDescending`方法是仅适用于的结果`OrderBy`或`OrderByDescending`。</span><span class="sxs-lookup"><span data-stu-id="08161-2616">Notice the recommended relationship between `C<T>` and `O<T>` which ensures that the `ThenBy` and `ThenByDescending` methods are available only on the result of an `OrderBy` or `OrderByDescending`.</span></span> <span data-ttu-id="08161-2617">此外请注意，建议的结果的形状`GroupBy`-一系列序列，其中每个内部序列具有一个额外`Key`属性。</span><span class="sxs-lookup"><span data-stu-id="08161-2617">Also notice the recommended shape of the result of `GroupBy` -- a sequence of sequences, where each inner sequence has an additional `Key` property.</span></span>

<span data-ttu-id="08161-2618">`System.Linq`命名空间提供用于实现的任何类型的查询运算符模式实现`System.Collections.Generic.IEnumerable<T>`接口。</span><span class="sxs-lookup"><span data-stu-id="08161-2618">The `System.Linq` namespace provides an implementation of the query operator pattern for any type that implements the `System.Collections.Generic.IEnumerable<T>` interface.</span></span>

## <a name="assignment-operators"></a><span data-ttu-id="08161-2619">赋值运算符</span><span class="sxs-lookup"><span data-stu-id="08161-2619">Assignment operators</span></span>

<span data-ttu-id="08161-2620">赋值运算符将一个新值分配给变量、 属性、 事件或索引器元素。</span><span class="sxs-lookup"><span data-stu-id="08161-2620">The assignment operators assign a new value to a variable, a property, an event, or an indexer element.</span></span>

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

<span data-ttu-id="08161-2621">赋值的左的操作数必须是归类为变量、 属性访问、 索引器访问或事件访问的表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-2621">The left operand of an assignment must be an expression classified as a variable, a property access, an indexer access, or an event access.</span></span>

<span data-ttu-id="08161-2622">`=`调用运算符***简单赋值运算符***。</span><span class="sxs-lookup"><span data-stu-id="08161-2622">The `=` operator is called the ***simple assignment operator***.</span></span> <span data-ttu-id="08161-2623">它将右操作数的值分配给由左操作数给定的变量、 属性或索引器元素。</span><span class="sxs-lookup"><span data-stu-id="08161-2623">It assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="08161-2624">简单赋值运算符的左的操作数不可能是一个事件访问 (除非中所述[类似字段的事件](classes.md#field-like-events))。</span><span class="sxs-lookup"><span data-stu-id="08161-2624">The left operand of the simple assignment operator may not be an event access (except as described in [Field-like events](classes.md#field-like-events)).</span></span> <span data-ttu-id="08161-2625">简单赋值运算符中所述[简单的赋值](expressions.md#simple-assignment)。</span><span class="sxs-lookup"><span data-stu-id="08161-2625">The simple assignment operator is described in [Simple assignment](expressions.md#simple-assignment).</span></span>

<span data-ttu-id="08161-2626">以外的其他赋值运算符`=`调用的运算符***复合赋值运算符***。</span><span class="sxs-lookup"><span data-stu-id="08161-2626">The assignment operators other than the `=` operator are called the ***compound assignment operators***.</span></span> <span data-ttu-id="08161-2627">这些运算符执行两个操作数，指示的运算，然后将生成的值分配给由左操作数给定的变量、 属性或索引器元素。</span><span class="sxs-lookup"><span data-stu-id="08161-2627">These operators perform the indicated operation on the two operands, and then assign the resulting value to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="08161-2628">中介绍了复合赋值运算符[复合赋值](expressions.md#compound-assignment)。</span><span class="sxs-lookup"><span data-stu-id="08161-2628">The compound assignment operators are described in [Compound assignment](expressions.md#compound-assignment).</span></span>

<span data-ttu-id="08161-2629">`+=`并`-=`事件访问表达式作为左操作数的运算符称为*事件赋值运算符*。</span><span class="sxs-lookup"><span data-stu-id="08161-2629">The `+=` and `-=` operators with an event access expression as the left operand are called the *event assignment operators*.</span></span> <span data-ttu-id="08161-2630">没有其他赋值运算符的左操作数的作为有效事件访问。</span><span class="sxs-lookup"><span data-stu-id="08161-2630">No other assignment operator is valid with an event access as the left operand.</span></span> <span data-ttu-id="08161-2631">中介绍了事件赋值运算符[事件分配](expressions.md#event-assignment)。</span><span class="sxs-lookup"><span data-stu-id="08161-2631">The event assignment operators are described in [Event assignment](expressions.md#event-assignment).</span></span>

<span data-ttu-id="08161-2632">赋值运算符是右结合运算符，这意味着操作将从右到左分组。</span><span class="sxs-lookup"><span data-stu-id="08161-2632">The assignment operators are right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="08161-2633">例如，窗体的表达式`a = b = c`评为`a = (b = c)`。</span><span class="sxs-lookup"><span data-stu-id="08161-2633">For example, an expression of the form `a = b = c` is evaluated as `a = (b = c)`.</span></span>

### <a name="simple-assignment"></a><span data-ttu-id="08161-2634">简单赋值</span><span class="sxs-lookup"><span data-stu-id="08161-2634">Simple assignment</span></span>

<span data-ttu-id="08161-2635">`=`调用运算符是简单赋值运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-2635">The `=` operator is called the simple assignment operator.</span></span>

<span data-ttu-id="08161-2636">简单赋值的左的操作数是否为窗体`E.P`或`E[Ei]`其中`E`具有编译时类型`dynamic`，然后分配动态绑定 ([动态绑定](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="08161-2636">If the left operand of a simple assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="08161-2637">在这种情况下是赋值表达式的编译时类型`dynamic`，如下所述的分辨率将发生在运行时基于的运行时类型和`E`。</span><span class="sxs-lookup"><span data-stu-id="08161-2637">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="08161-2638">在简单赋值中，右操作数必须是隐式转换为左操作数的类型的表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-2638">In a simple assignment, the right operand must be an expression that is implicitly convertible to the type of the left operand.</span></span> <span data-ttu-id="08161-2639">该操作将右操作数的值分配给由左操作数给定的变量、 属性或索引器元素。</span><span class="sxs-lookup"><span data-stu-id="08161-2639">The operation assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span>

<span data-ttu-id="08161-2640">简单的赋值表达式的结果是分配给左操作数的值。</span><span class="sxs-lookup"><span data-stu-id="08161-2640">The result of a simple assignment expression is the value assigned to the left operand.</span></span> <span data-ttu-id="08161-2641">结果具有左操作数为相同的类型，并始终分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="08161-2641">The result has the same type as the left operand and is always classified as a value.</span></span>

<span data-ttu-id="08161-2642">如果左的操作数是属性或索引器访问，必须具有属性或索引器`set`访问器。</span><span class="sxs-lookup"><span data-stu-id="08161-2642">If the left operand is a property or indexer access, the property or indexer must have a `set` accessor.</span></span> <span data-ttu-id="08161-2643">如果这不是这种情况，绑定时出错。</span><span class="sxs-lookup"><span data-stu-id="08161-2643">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="08161-2644">窗体的简单分配的运行时处理`x = y`包括以下步骤：</span><span class="sxs-lookup"><span data-stu-id="08161-2644">The run-time processing of a simple assignment of the form `x = y` consists of the following steps:</span></span>

*  <span data-ttu-id="08161-2645">如果`x`分类为变量：</span><span class="sxs-lookup"><span data-stu-id="08161-2645">If `x` is classified as a variable:</span></span>
   * <span data-ttu-id="08161-2646">`x` 计算为生成变量。</span><span class="sxs-lookup"><span data-stu-id="08161-2646">`x` is evaluated to produce the variable.</span></span>
   * <span data-ttu-id="08161-2647">`y` 是计算并，如有必要，转换为的类型`x`通过隐式转换 ([隐式转换](conversions.md#implicit-conversions))。</span><span class="sxs-lookup"><span data-stu-id="08161-2647">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="08161-2648">如果通过给出的变量`x`是一个数组元素的*reference_type*，将运行时检查以确保计算的值`y`数组实例与兼容`x`是元素。</span><span class="sxs-lookup"><span data-stu-id="08161-2648">If the variable given by `x` is an array element of a *reference_type*, a run-time check is performed to ensure that the value computed for `y` is compatible with the array instance of which `x` is an element.</span></span> <span data-ttu-id="08161-2649">如果箱子`y`是`null`，或如果隐式引用转换 ([隐式引用转换](conversions.md#implicit-reference-conversions)) 存在从引用的实例的实际类型`y`为实际的元素类型数组实例包含`x`。</span><span class="sxs-lookup"><span data-stu-id="08161-2649">The check succeeds if `y` is `null`, or if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the actual type of the instance referenced by `y` to the actual element type of the array instance containing `x`.</span></span> <span data-ttu-id="08161-2650">否则，将会引发 `System.ArrayTypeMismatchException`。</span><span class="sxs-lookup"><span data-stu-id="08161-2650">Otherwise, a `System.ArrayTypeMismatchException` is thrown.</span></span>
   * <span data-ttu-id="08161-2651">从评估和转换所产生的值`y`存储到给定的评估结果的位置`x`。</span><span class="sxs-lookup"><span data-stu-id="08161-2651">The value resulting from the evaluation and conversion of `y` is stored into the location given by the evaluation of `x`.</span></span>
*  <span data-ttu-id="08161-2652">如果`x`归类为属性或索引器访问：</span><span class="sxs-lookup"><span data-stu-id="08161-2652">If `x` is classified as a property or indexer access:</span></span>
   * <span data-ttu-id="08161-2653">实例表达式 (如果`x`不是`static`) 和参数列表 (如果`x`的索引器访问) 与关联`x`计算，然后在后续结果`set`取值函数调用。</span><span class="sxs-lookup"><span data-stu-id="08161-2653">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `set` accessor invocation.</span></span>
   * <span data-ttu-id="08161-2654">`y` 是计算并，如有必要，转换为的类型`x`通过隐式转换 ([隐式转换](conversions.md#implicit-conversions))。</span><span class="sxs-lookup"><span data-stu-id="08161-2654">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="08161-2655">`set`访问器的`x`为计算的值使用调用`y`作为其`value`参数。</span><span class="sxs-lookup"><span data-stu-id="08161-2655">The `set` accessor of `x` is invoked with the value computed for `y` as its `value` argument.</span></span>

<span data-ttu-id="08161-2656">数组协变规则 ([数组协方差](arrays.md#array-covariance)) 允许使用数组类型的值`A[]`是对的数组类型实例的引用`B[]`，则从存在的隐式引用转换`B`到`A`.</span><span class="sxs-lookup"><span data-stu-id="08161-2656">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="08161-2657">由于这些规则，对的数组元素赋值*reference_type*需要运行时检查以确保所赋的值与数组实例兼容。</span><span class="sxs-lookup"><span data-stu-id="08161-2657">Because of these rules, assignment to an array element of a *reference_type* requires a run-time check to ensure that the value being assigned is compatible with the array instance.</span></span> <span data-ttu-id="08161-2658">在示例</span><span class="sxs-lookup"><span data-stu-id="08161-2658">In the example</span></span>
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
<span data-ttu-id="08161-2659">最后一个分配会导致`System.ArrayTypeMismatchException`因为引发的实例`ArrayList`不能存储在一个元素的`string[]`。</span><span class="sxs-lookup"><span data-stu-id="08161-2659">the last assignment causes a `System.ArrayTypeMismatchException` to be thrown because an instance of `ArrayList` cannot be stored in an element of a `string[]`.</span></span>

<span data-ttu-id="08161-2660">当属性或索引器声明中*struct_type*所分配的实例表达式的目标关联的属性或索引器访问必须分类为变量。</span><span class="sxs-lookup"><span data-stu-id="08161-2660">When a property or indexer declared in a *struct_type* is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="08161-2661">如果实例表达式分类为一个值，绑定时出错。</span><span class="sxs-lookup"><span data-stu-id="08161-2661">If the instance expression is classified as a value, a binding-time error occurs.</span></span> <span data-ttu-id="08161-2662">由于[成员访问](expressions.md#member-access)，相同的规则也适用于字段。</span><span class="sxs-lookup"><span data-stu-id="08161-2662">Because of [Member access](expressions.md#member-access), the same rule also applies to fields.</span></span>

<span data-ttu-id="08161-2663">给定下列声明：</span><span class="sxs-lookup"><span data-stu-id="08161-2663">Given the declarations:</span></span>
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
<span data-ttu-id="08161-2664">在示例</span><span class="sxs-lookup"><span data-stu-id="08161-2664">in the example</span></span>
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
<span data-ttu-id="08161-2665">对分配`p.X`， `p.Y`， `r.A`，和`r.B`允许，因为`p`和`r`是变量。</span><span class="sxs-lookup"><span data-stu-id="08161-2665">the assignments to `p.X`, `p.Y`, `r.A`, and `r.B` are permitted because `p` and `r` are variables.</span></span> <span data-ttu-id="08161-2666">但是，在以下示例</span><span class="sxs-lookup"><span data-stu-id="08161-2666">However, in the example</span></span>
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
<span data-ttu-id="08161-2667">分配都无效，因为`r.A`和`r.B`不是变量。</span><span class="sxs-lookup"><span data-stu-id="08161-2667">the assignments are all invalid, since `r.A` and `r.B` are not variables.</span></span>

### <a name="compound-assignment"></a><span data-ttu-id="08161-2668">复合赋值</span><span class="sxs-lookup"><span data-stu-id="08161-2668">Compound assignment</span></span>

<span data-ttu-id="08161-2669">复合赋值的左的操作数是否为窗体`E.P`或`E[Ei]`其中`E`具有编译时类型`dynamic`，然后分配动态绑定 ([动态绑定](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="08161-2669">If the left operand of a compound assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="08161-2670">在这种情况下是赋值表达式的编译时类型`dynamic`，如下所述的分辨率将发生在运行时基于的运行时类型和`E`。</span><span class="sxs-lookup"><span data-stu-id="08161-2670">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="08161-2671">操作的窗体`x op= y`处理的应用的二元运算符重载解析 ([二元运算符重载决策](expressions.md#binary-operator-overload-resolution)) 该操作已编写`x op y`。</span><span class="sxs-lookup"><span data-stu-id="08161-2671">An operation of the form `x op= y` is processed by applying binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x op y`.</span></span> <span data-ttu-id="08161-2672">然后，</span><span class="sxs-lookup"><span data-stu-id="08161-2672">Then,</span></span>

*  <span data-ttu-id="08161-2673">如果所选运算符的返回类型是隐式转换为的类型`x`，该操作将被视为`x = x op y`，只不过`x`只计算一次。</span><span class="sxs-lookup"><span data-stu-id="08161-2673">If the return type of the selected operator is implicitly convertible to the type of `x`, the operation is evaluated as `x = x op y`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="08161-2674">否则为如果所选的运算符是一个预定义的运算符，如果所选运算符的返回类型是显式转换为的类型`x`，并且如果`y`隐式转换为的类型`x`或操作员移位运算符，则该操作将被视为`x = (T)(x op y)`，其中`T`是一种`x`，只不过`x`只计算一次。</span><span class="sxs-lookup"><span data-stu-id="08161-2674">Otherwise, if the selected operator is a predefined operator, if the return type of the selected operator is explicitly convertible to the type of `x`, and if `y` is implicitly convertible to the type of `x` or the operator is a shift operator, then the operation is evaluated as `x = (T)(x op y)`, where `T` is the type of `x`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="08161-2675">否则为复合赋值是无效的并且绑定时出错。</span><span class="sxs-lookup"><span data-stu-id="08161-2675">Otherwise, the compound assignment is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="08161-2676">术语"仅计算一次"意味着在评估`x op y`，任何构成表达式的结果`x`是暂时保存并重用时执行的赋值`x`。</span><span class="sxs-lookup"><span data-stu-id="08161-2676">The term "evaluated only once" means that in the evaluation of `x op y`, the results of any constituent expressions of `x` are temporarily saved and then reused when performing the assignment to `x`.</span></span> <span data-ttu-id="08161-2677">例如，在赋值`A()[B()] += C()`，其中`A`是一种方法返回`int[]`，和`B`并`C`方法返回`int`，按顺序一次，调用这些方法`A`，`B`, `C`.</span><span class="sxs-lookup"><span data-stu-id="08161-2677">For example, in the assignment `A()[B()] += C()`, where `A` is a method returning `int[]`, and `B` and `C` are methods returning `int`, the methods are invoked only once, in the order `A`, `B`, `C`.</span></span>

<span data-ttu-id="08161-2678">属性访问或索引器访问的左的操作数的复合赋值时，属性或索引器必须同时具有`get`访问器和一个`set`访问器。</span><span class="sxs-lookup"><span data-stu-id="08161-2678">When the left operand of a compound assignment is a property access or indexer access, the property or indexer must have both a `get` accessor and a `set` accessor.</span></span> <span data-ttu-id="08161-2679">如果这不是这种情况，绑定时出错。</span><span class="sxs-lookup"><span data-stu-id="08161-2679">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="08161-2680">允许的值之上的第二个段落`x op= y`计算结果为`x = (T)(x op y)`在某些上下文中。</span><span class="sxs-lookup"><span data-stu-id="08161-2680">The second rule above permits `x op= y` to be evaluated as `x = (T)(x op y)` in certain contexts.</span></span> <span data-ttu-id="08161-2681">此规则存在，使预定义的运算符可以用作复合运算符，如果左的操作数的类型`sbyte`， `byte`， `short`， `ushort`，或`char`。</span><span class="sxs-lookup"><span data-stu-id="08161-2681">The rule exists such that the predefined operators can be used as compound operators when the left operand is of type `sbyte`, `byte`, `short`, `ushort`, or `char`.</span></span> <span data-ttu-id="08161-2682">甚至当两个参数都属于这些类型之一时，预定义的运算符生成类型的结果`int`，如中所述[二进制数值提升](expressions.md#binary-numeric-promotions)。</span><span class="sxs-lookup"><span data-stu-id="08161-2682">Even when both arguments are of one of those types, the predefined operators produce a result of type `int`, as described in [Binary numeric promotions](expressions.md#binary-numeric-promotions).</span></span> <span data-ttu-id="08161-2683">因此，不进行强制转换它就不可能要将结果分配到左操作数。</span><span class="sxs-lookup"><span data-stu-id="08161-2683">Thus, without a cast it would not be possible to assign the result to the left operand.</span></span>

<span data-ttu-id="08161-2684">预定义的运算符的规则的直观的效果是只需`x op= y`如果这两个，则允许的`x op y`和`x = y`允许。</span><span class="sxs-lookup"><span data-stu-id="08161-2684">The intuitive effect of the rule for predefined operators is simply that `x op= y` is permitted if both of `x op y` and `x = y` are permitted.</span></span> <span data-ttu-id="08161-2685">在示例</span><span class="sxs-lookup"><span data-stu-id="08161-2685">In the example</span></span>
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
<span data-ttu-id="08161-2686">每个错误的直观原因是，相应的简单分配也已经出现错误。</span><span class="sxs-lookup"><span data-stu-id="08161-2686">the intuitive reason for each error is that a corresponding simple assignment would also have been an error.</span></span>

<span data-ttu-id="08161-2687">这也意味着复合赋值操作支持取消操作。</span><span class="sxs-lookup"><span data-stu-id="08161-2687">This also means that compound assignment operations support lifted operations.</span></span> <span data-ttu-id="08161-2688">在示例</span><span class="sxs-lookup"><span data-stu-id="08161-2688">In the example</span></span>
```csharp
int? i = 0;
i += 1;             // Ok
```
<span data-ttu-id="08161-2689">提升的运算符`+(int?,int?)`使用。</span><span class="sxs-lookup"><span data-stu-id="08161-2689">the lifted operator `+(int?,int?)` is used.</span></span>

### <a name="event-assignment"></a><span data-ttu-id="08161-2690">事件分配</span><span class="sxs-lookup"><span data-stu-id="08161-2690">Event assignment</span></span>

<span data-ttu-id="08161-2691">如果左的操作数`+=`或`-=`运算符属于事件访问，则表达式进行计算，如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-2691">If the left operand of a `+=` or `-=` operator is classified as an event access, then the expression is evaluated as follows:</span></span>

*  <span data-ttu-id="08161-2692">实例，如果有，事件访问的被计算表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-2692">The instance expression, if any, of the event access is evaluated.</span></span>
*  <span data-ttu-id="08161-2693">右操作数`+=`或`-=`运算符要计算，并如有必要，转换为通过隐式转换的左操作数的类型 ([隐式转换](conversions.md#implicit-conversions))。</span><span class="sxs-lookup"><span data-stu-id="08161-2693">The right operand of the `+=` or `-=` operator is evaluated, and, if required, converted to the type of the left operand through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
*  <span data-ttu-id="08161-2694">事件的事件访问器会调用带有参数组成的列表，右操作数，在评估之后，如有必要，转换。</span><span class="sxs-lookup"><span data-stu-id="08161-2694">An event accessor of the event is invoked, with argument list consisting of the right operand, after evaluation and, if necessary, conversion.</span></span> <span data-ttu-id="08161-2695">如果该运算符`+=`，则`add`调用访问器; 如果该运算符`-=`，则`remove`调用访问器。</span><span class="sxs-lookup"><span data-stu-id="08161-2695">If the operator was `+=`, the `add` accessor is invoked; if the operator was `-=`, the `remove` accessor is invoked.</span></span>

<span data-ttu-id="08161-2696">事件赋值表达式不产生值。</span><span class="sxs-lookup"><span data-stu-id="08161-2696">An event assignment expression does not yield a value.</span></span> <span data-ttu-id="08161-2697">因此，事件赋值表达式是有效的上下文中仅*statement_expression* ([表达式语句](statements.md#expression-statements))。</span><span class="sxs-lookup"><span data-stu-id="08161-2697">Thus, an event assignment expression is valid only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

## <a name="expression"></a><span data-ttu-id="08161-2698">表达式</span><span class="sxs-lookup"><span data-stu-id="08161-2698">Expression</span></span>

<span data-ttu-id="08161-2699">*表达式*可以是*non_assignment_expression*或*分配*。</span><span class="sxs-lookup"><span data-stu-id="08161-2699">An *expression* is either a *non_assignment_expression* or an *assignment*.</span></span>

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

## <a name="constant-expressions"></a><span data-ttu-id="08161-2700">常量表达式</span><span class="sxs-lookup"><span data-stu-id="08161-2700">Constant expressions</span></span>

<span data-ttu-id="08161-2701">一个*constant_expression*是可以在编译时完全计算的表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-2701">A *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span>

```antlr
constant_expression
    : expression
    ;
```

<span data-ttu-id="08161-2702">常量表达式必须为`null`文字或具有以下类型之一的值： `sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char``float`， `double`， `decimal`， `bool`， `object`， `string`，或任何枚举类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2702">A constant expression must be the `null` literal or a value with one of  the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`, or any enumeration type.</span></span> <span data-ttu-id="08161-2703">常量表达式中允许仅以下构造：</span><span class="sxs-lookup"><span data-stu-id="08161-2703">Only the following constructs are permitted in constant expressions:</span></span>

*  <span data-ttu-id="08161-2704">文本 (包括`null`文字)。</span><span class="sxs-lookup"><span data-stu-id="08161-2704">Literals (including the `null` literal).</span></span>
*  <span data-ttu-id="08161-2705">对引用`const`类和结构类型的成员。</span><span class="sxs-lookup"><span data-stu-id="08161-2705">References to `const` members of class and struct types.</span></span>
*  <span data-ttu-id="08161-2706">对枚举类型的成员的引用。</span><span class="sxs-lookup"><span data-stu-id="08161-2706">References to members of enumeration types.</span></span>
*  <span data-ttu-id="08161-2707">对引用`const`参数或局部变量</span><span class="sxs-lookup"><span data-stu-id="08161-2707">References to `const` parameters or local variables</span></span>
*  <span data-ttu-id="08161-2708">带括号的子表达式，其自身是常量表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-2708">Parenthesized sub-expressions, which are themselves constant expressions.</span></span>
*  <span data-ttu-id="08161-2709">强制转换表达式，前提是目标类型是上面列出的类型之一。</span><span class="sxs-lookup"><span data-stu-id="08161-2709">Cast expressions, provided the target type is one of the types listed above.</span></span>
*  <span data-ttu-id="08161-2710">`checked` 和`unchecked`表达式</span><span class="sxs-lookup"><span data-stu-id="08161-2710">`checked` and `unchecked` expressions</span></span>
*  <span data-ttu-id="08161-2711">默认值表达式</span><span class="sxs-lookup"><span data-stu-id="08161-2711">Default value expressions</span></span>
*  <span data-ttu-id="08161-2712">Nameof 表达式</span><span class="sxs-lookup"><span data-stu-id="08161-2712">Nameof expressions</span></span>
*  <span data-ttu-id="08161-2713">预定义`+`， `-`， `!`，和`~`一元运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-2713">The predefined `+`, `-`, `!`, and `~` unary operators.</span></span>
*  <span data-ttu-id="08161-2714">预定义`+`， `-`， `*`， `/`， `%`， `<<`， `>>`， `&`， `|`， `^`， `&&`， `||`， `==`， `!=`， `<`， `>`， `<=`，并`>=`二元运算符，提供每个操作数是上面列出的类型。</span><span class="sxs-lookup"><span data-stu-id="08161-2714">The predefined `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, and `>=` binary operators, provided each operand is of a type listed above.</span></span>
*  <span data-ttu-id="08161-2715">`?:`条件运算符。</span><span class="sxs-lookup"><span data-stu-id="08161-2715">The `?:` conditional operator.</span></span>

<span data-ttu-id="08161-2716">在常量表达式中允许以下转换：</span><span class="sxs-lookup"><span data-stu-id="08161-2716">The following conversions are permitted in constant expressions:</span></span>

*  <span data-ttu-id="08161-2717">标识转换</span><span class="sxs-lookup"><span data-stu-id="08161-2717">Identity conversions</span></span>
*  <span data-ttu-id="08161-2718">数值转换</span><span class="sxs-lookup"><span data-stu-id="08161-2718">Numeric conversions</span></span>
*  <span data-ttu-id="08161-2719">枚举的转换</span><span class="sxs-lookup"><span data-stu-id="08161-2719">Enumeration conversions</span></span>
*  <span data-ttu-id="08161-2720">常量表达式转换</span><span class="sxs-lookup"><span data-stu-id="08161-2720">Constant expression conversions</span></span>
*  <span data-ttu-id="08161-2721">提供的转换的源是常量表达式的计算结果为 null 值的隐式和显式引用转换。</span><span class="sxs-lookup"><span data-stu-id="08161-2721">Implicit and explicit reference conversions, provided that the source of the conversions is a constant expression that evaluates to the null value.</span></span>

<span data-ttu-id="08161-2722">其他转换，包括装箱，取消装箱转换和隐式引用转换的非 null 值的常量表达式中不允许。</span><span class="sxs-lookup"><span data-stu-id="08161-2722">Other conversions including boxing, unboxing and implicit reference conversions of non-null values are not permitted in constant expressions.</span></span> <span data-ttu-id="08161-2723">例如：</span><span class="sxs-lookup"><span data-stu-id="08161-2723">For example:</span></span>
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
<span data-ttu-id="08161-2724">初始化 i 是一个错误，因为是必需的装箱转换。</span><span class="sxs-lookup"><span data-stu-id="08161-2724">the initialization of i is an error because a boxing conversion is required.</span></span> <span data-ttu-id="08161-2725">Str 初始化是一个错误，因为从非 null 值的隐式引用转换是必需的。</span><span class="sxs-lookup"><span data-stu-id="08161-2725">The initialization of str is an error because an implicit reference conversion from a non-null value is required.</span></span>

<span data-ttu-id="08161-2726">只要表达式可满足上面列出的要求，是在编译时计算该表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-2726">Whenever an expression fulfills the requirements listed above, the expression is evaluated at compile-time.</span></span> <span data-ttu-id="08161-2727">即使该表达式是包含非常量构造较大表达式的子表达式，这是如此。</span><span class="sxs-lookup"><span data-stu-id="08161-2727">This is true even if the expression is a sub-expression of a larger expression that contains non-constant constructs.</span></span>

<span data-ttu-id="08161-2728">只不过时，运行时计算引发异常，而编译时计算会导致编译时错误发生，常量表达式的编译时计算为非常量表达式的运行时评估使用相同的规则。</span><span class="sxs-lookup"><span data-stu-id="08161-2728">The compile-time evaluation of constant expressions uses the same rules as run-time evaluation of non-constant expressions, except that where run-time evaluation would have thrown an exception, compile-time evaluation causes a compile-time error to occur.</span></span>

<span data-ttu-id="08161-2729">除非常量表达式显式置于`unchecked`上下文，在编译时计算的表达式始终过程发生在整型类型算术运算和转换的溢出会导致编译时错误 ([常量表达式](expressions.md#constant-expressions))。</span><span class="sxs-lookup"><span data-stu-id="08161-2729">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur in integral-type arithmetic operations and conversions during the compile-time evaluation of the expression always cause compile-time errors ([Constant expressions](expressions.md#constant-expressions)).</span></span>

<span data-ttu-id="08161-2730">以下列出的上下文中发生的常量表达式。</span><span class="sxs-lookup"><span data-stu-id="08161-2730">Constant expressions occur in the contexts listed below.</span></span> <span data-ttu-id="08161-2731">在这些上下文中，如果不能在编译时完全计算表达式，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="08161-2731">In these contexts, a compile-time error occurs if an expression cannot be fully evaluated at compile-time.</span></span>

*  <span data-ttu-id="08161-2732">常数声明 ([常量](classes.md#constants))。</span><span class="sxs-lookup"><span data-stu-id="08161-2732">Constant declarations ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="08161-2733">枚举成员声明 ([枚举成员](enums.md#enum-members))。</span><span class="sxs-lookup"><span data-stu-id="08161-2733">Enumeration member declarations ([Enum members](enums.md#enum-members)).</span></span>
*  <span data-ttu-id="08161-2734">默认自变量的形式参数列表 ([方法参数](classes.md#method-parameters))</span><span class="sxs-lookup"><span data-stu-id="08161-2734">Default arguments of formal parameter lists ([Method parameters](classes.md#method-parameters))</span></span>
*  <span data-ttu-id="08161-2735">`case` 标签`switch`语句 ([switch 语句](statements.md#the-switch-statement))。</span><span class="sxs-lookup"><span data-stu-id="08161-2735">`case` labels of a `switch` statement ([The switch statement](statements.md#the-switch-statement)).</span></span>
*  <span data-ttu-id="08161-2736">`goto case` 语句 ([goto 语句](statements.md#the-goto-statement))。</span><span class="sxs-lookup"><span data-stu-id="08161-2736">`goto case` statements ([The goto statement](statements.md#the-goto-statement)).</span></span>
*  <span data-ttu-id="08161-2737">数组创建表达式中的维度长度 ([数组创建表达式](expressions.md#array-creation-expressions))，其中包含初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="08161-2737">Dimension lengths in an array creation expression ([Array creation expressions](expressions.md#array-creation-expressions)) that includes an initializer.</span></span>
*  <span data-ttu-id="08161-2738">属性 ([属性](attributes.md))。</span><span class="sxs-lookup"><span data-stu-id="08161-2738">Attributes ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="08161-2739">常量表达式隐式转换 ([隐式的常量表达式转换](conversions.md#implicit-constant-expression-conversions)) 允许类型的常量表达式`int`转换为`sbyte`， `byte`， `short`， `ushort`， `uint`，或`ulong`，前提是目标类型的范围内的常量表达式的值。</span><span class="sxs-lookup"><span data-stu-id="08161-2739">An implicit constant expression conversion ([Implicit constant expression conversions](conversions.md#implicit-constant-expression-conversions)) permits a constant expression of type `int` to be converted to `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the constant expression is within the range of the destination type.</span></span>

## <a name="boolean-expressions"></a><span data-ttu-id="08161-2740">Boolean 表达式</span><span class="sxs-lookup"><span data-stu-id="08161-2740">Boolean expressions</span></span>

<span data-ttu-id="08161-2741">一个*boolean_expression*是生成类型的结果的表达式`bool`; 或者直接或通过应用程序的`operator true`在某些上下文中指定在下面的示例。</span><span class="sxs-lookup"><span data-stu-id="08161-2741">A *boolean_expression* is an expression that yields a result of type `bool`; either directly or through application of `operator true` in certain contexts as specified in the following.</span></span>

```antlr
boolean_expression
    : expression
    ;
```

<span data-ttu-id="08161-2742">控制条件表达式*if_statement* ([if 语句](statements.md#the-if-statement))， *while_statement* ([while 语句](statements.md#the-while-statement))，*do_statement* ([do 语句](statements.md#the-do-statement))，或*for_statement* ([语句](statements.md#the-for-statement)) 是*boolean_表达式*。</span><span class="sxs-lookup"><span data-stu-id="08161-2742">The controlling conditional expression of an *if_statement* ([The if statement](statements.md#the-if-statement)), *while_statement* ([The while statement](statements.md#the-while-statement)), *do_statement* ([The do statement](statements.md#the-do-statement)), or *for_statement* ([The for statement](statements.md#the-for-statement)) is a *boolean_expression*.</span></span> <span data-ttu-id="08161-2743">控制条件表达式`?:`运算符 ([条件运算符](expressions.md#conditional-operator)) 遵循相同的规则*boolean_expression*，但出于运算符优先级进行分类作为*conditional_or_expression*。</span><span class="sxs-lookup"><span data-stu-id="08161-2743">The controlling conditional expression of the `?:` operator ([Conditional operator](expressions.md#conditional-operator)) follows the same rules as a *boolean_expression*, but for reasons of operator precedence is classified as a *conditional_or_expression*.</span></span>

<span data-ttu-id="08161-2744">一个*boolean_expression* `E`生成类型的值所需`bool`，按如下所示：</span><span class="sxs-lookup"><span data-stu-id="08161-2744">A *boolean_expression* `E` is required to be able to produce a value of type `bool`, as follows:</span></span>

*  <span data-ttu-id="08161-2745">如果`E`隐式转换为`bool`然后该隐式转换将它应用在运行时。</span><span class="sxs-lookup"><span data-stu-id="08161-2745">If `E` is implicitly convertible to `bool` then at runtime that implicit conversion is applied.</span></span>
*  <span data-ttu-id="08161-2746">否则，一元运算符重载决策 ([一元运算符重载决策](expressions.md#unary-operator-overload-resolution)) 用于查找运算符的唯一最佳实现`true`上`E`，并在运行时应用该实现。</span><span class="sxs-lookup"><span data-stu-id="08161-2746">Otherwise, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is used to find a unique best implementation of operator `true` on `E`, and that implementation is applied at runtime.</span></span>
*  <span data-ttu-id="08161-2747">如果不找到任何此类运算符，则绑定时出错。</span><span class="sxs-lookup"><span data-stu-id="08161-2747">If no such operator is found, a binding-time error occurs.</span></span>

<span data-ttu-id="08161-2748">`DBBool`中的结构类型[数据库布尔类型](structs.md#database-boolean-type)提供了实现的类型的一个示例`operator true`和`operator false`。</span><span class="sxs-lookup"><span data-stu-id="08161-2748">The `DBBool` struct type in [Database boolean type](structs.md#database-boolean-type) provides an example of a type that implements `operator true` and `operator false`.</span></span>
