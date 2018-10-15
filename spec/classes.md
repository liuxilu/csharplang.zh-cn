# <a name="classes"></a><span data-ttu-id="6f0ea-101">类</span><span class="sxs-lookup"><span data-stu-id="6f0ea-101">Classes</span></span>

<span data-ttu-id="6f0ea-102">类是可以包含数据成员 （常量和字段），函数成员 （方法、 属性、 事件、 索引器、 运算符、 实例构造函数、 析构函数和静态构造函数） 以及嵌套的类型的数据结构。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-102">A class is a data structure that may contain data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="6f0ea-103">类类型支持继承，因此派生的类可以扩展和专门针对基类的机制。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-103">Class types support inheritance, a mechanism whereby a derived class can extend and specialize a base class.</span></span>

## <a name="class-declarations"></a><span data-ttu-id="6f0ea-104">类声明</span><span class="sxs-lookup"><span data-stu-id="6f0ea-104">Class declarations</span></span>

<span data-ttu-id="6f0ea-105">一个*class_declaration*是*type_declaration* ([类型声明](namespaces.md#type-declarations))，其用于声明新类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-105">A *class_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new class.</span></span>

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

<span data-ttu-id="6f0ea-106">一个*class_declaration*包含一组可选*特性*([特性](attributes.md)) 后, 跟一组可选的*class_modifier*s ([类修饰符](classes.md#class-modifiers)) 后, 跟可选`partial`修饰符，关键字后跟`class`和一个*标识符*命名的类后, 跟可选*type_parameter_list* ([类型参数](classes.md#type-parameters)) 后, 跟一个可选*class_base*规范 ([类 base规范](classes.md#class-base-specification)) 后, 跟一组可选*type_parameter_constraints_clause*s ([类型参数约束](classes.md#type-parameter-constraints)) 后, 跟*class_body* ([类正文](classes.md#class-body)) 后, 跟分号 （可选）。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-106">A *class_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *class_modifier*s ([Class modifiers](classes.md#class-modifiers)), followed by an optional `partial` modifier, followed by the keyword `class` and an *identifier* that names the class, followed by an optional *type_parameter_list* ([Type parameters](classes.md#type-parameters)), followed by an optional *class_base* specification ([Class base specification](classes.md#class-base-specification)) , followed by an optional set of *type_parameter_constraints_clause*s ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *class_body* ([Class body](classes.md#class-body)), optionally followed by a semicolon.</span></span>

<span data-ttu-id="6f0ea-107">类声明不能提供*type_parameter_constraints_clause*s 除非还提供*type_parameter_list*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-107">A class declaration cannot supply *type_parameter_constraints_clause*s unless it also supplies a *type_parameter_list*.</span></span>

<span data-ttu-id="6f0ea-108">提供的类声明*type_parameter_list*是***泛型类声明***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-108">A class declaration that supplies a *type_parameter_list* is a ***generic class declaration***.</span></span> <span data-ttu-id="6f0ea-109">此外，嵌套在泛型类声明或泛型结构声明中的任何类本身就是泛型类声明，因为必须提供包含类型的类型参数创建构造的类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-109">Additionally, any class nested inside a generic class declaration or a generic struct declaration is itself a generic class declaration, since type parameters for the containing type must be supplied to create a constructed type.</span></span>

### <a name="class-modifiers"></a><span data-ttu-id="6f0ea-110">类修饰符</span><span class="sxs-lookup"><span data-stu-id="6f0ea-110">Class modifiers</span></span>

<span data-ttu-id="6f0ea-111">一个*class_declaration*可以根据需要包含一系列类修饰符：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-111">A *class_declaration* may optionally include a sequence of class modifiers:</span></span>

```antlr
class_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'abstract'
    | 'sealed'
    | 'static'
    | class_modifier_unsafe
    ;
```

<span data-ttu-id="6f0ea-112">它是在类声明中多次出现同一修饰符的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-112">It is a compile-time error for the same modifier to appear multiple times in a class declaration.</span></span>

<span data-ttu-id="6f0ea-113">`new`嵌套类上允许使用修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-113">The `new` modifier is permitted on nested classes.</span></span> <span data-ttu-id="6f0ea-114">它指定类隐藏继承的成员相同的名称，如中所述[的新修饰符](classes.md#the-new-modifier)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-114">It specifies that the class hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span> <span data-ttu-id="6f0ea-115">它是编译时错误`new`修饰符可以出现在类声明不是嵌套的类声明中。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-115">It is a compile-time error for the `new` modifier to appear on a class declaration that is not a nested class declaration.</span></span>

<span data-ttu-id="6f0ea-116">`public`， `protected`， `internal`，和`private`修饰符控制类的可访问性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the class.</span></span> <span data-ttu-id="6f0ea-117">具体取决于类声明发生的上下文，其中某些修饰符可能不允许使用 ([声明可访问性](basic-concepts.md#declared-accessibility))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-117">Depending on the context in which the class declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="6f0ea-118">`abstract`，`sealed`和`static`修饰符将在以下各节中讨论。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-118">The `abstract`, `sealed` and `static` modifiers are discussed in the following sections.</span></span>

#### <a name="abstract-classes"></a><span data-ttu-id="6f0ea-119">抽象类</span><span class="sxs-lookup"><span data-stu-id="6f0ea-119">Abstract classes</span></span>

<span data-ttu-id="6f0ea-120">`abstract`修饰符用于指示类是不完整，它旨在用于只能用作基类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-120">The `abstract` modifier is used to indicate that a class is incomplete and that it is intended to be used only as a base class.</span></span> <span data-ttu-id="6f0ea-121">一个抽象类不同于非抽象类通过以下方式：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-121">An abstract class differs from a non-abstract class in the following ways:</span></span>

*  <span data-ttu-id="6f0ea-122">不能直接实例化抽象类，它会导致编译时错误使用`new`的抽象类上的运算符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-122">An abstract class cannot be instantiated directly, and it is a compile-time error to use the `new` operator on an abstract class.</span></span> <span data-ttu-id="6f0ea-123">虽然可能有变量和其编译时类型是抽象的值，但此类变量和值将一定是`null`或包含对派生自抽象类型的非抽象类的实例的引用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-123">While it is possible to have variables and values whose compile-time types are abstract, such variables and values will necessarily either be `null` or contain references to instances of non-abstract classes derived from the abstract types.</span></span>
*  <span data-ttu-id="6f0ea-124">允许 （但不是必需的） 的抽象类包含抽象成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-124">An abstract class is permitted (but not required) to contain abstract members.</span></span>
*  <span data-ttu-id="6f0ea-125">无法密封的抽象类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-125">An abstract class cannot be sealed.</span></span>

<span data-ttu-id="6f0ea-126">当非抽象类派生自抽象类时，非抽象类必须包括所有继承的抽象成员，从而重写这些抽象成员的实际实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-126">When a non-abstract class is derived from an abstract class, the non-abstract class must include actual implementations of all inherited abstract members, thereby overriding those abstract members.</span></span> <span data-ttu-id="6f0ea-127">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-127">In the example</span></span>
```csharp
abstract class A
{
    public abstract void F();
}

abstract class B: A
{
    public void G() {}
}

class C: B
{
    public override void F() {
        // actual implementation of F
    }
}
```
<span data-ttu-id="6f0ea-128">抽象类`A`引入了一个抽象方法`F`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-128">the abstract class `A` introduces an abstract method `F`.</span></span> <span data-ttu-id="6f0ea-129">类`B`引入了其他方法`G`，但由于它不提供的实现`F`，`B`也必须声明为抽象。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-129">Class `B` introduces an additional method `G`, but since it doesn't provide an implementation of `F`, `B` must also be declared abstract.</span></span> <span data-ttu-id="6f0ea-130">类`C`重写`F`并提供实际实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-130">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="6f0ea-131">由于没有抽象成员在`C`，`C`是允许 （但不是必需的） 为非抽象。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-131">Since there are no abstract members in `C`, `C` is permitted (but not required) to be non-abstract.</span></span>

#### <a name="sealed-classes"></a><span data-ttu-id="6f0ea-132">密封的类</span><span class="sxs-lookup"><span data-stu-id="6f0ea-132">Sealed classes</span></span>

<span data-ttu-id="6f0ea-133">`sealed`修饰符用于防止从类派生。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-133">The `sealed` modifier is used to prevent derivation from a class.</span></span> <span data-ttu-id="6f0ea-134">如果密封的类指定为另一个类的基类，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-134">A compile-time error occurs if a sealed class is specified as the base class of another class.</span></span>

<span data-ttu-id="6f0ea-135">密封的类也不能为抽象类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-135">A sealed class cannot also be an abstract class.</span></span>

<span data-ttu-id="6f0ea-136">`sealed`修饰符主要用于防止无意的派生，但它还使某些运行时优化。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-136">The `sealed` modifier is primarily used to prevent unintended derivation, but it also enables certain run-time optimizations.</span></span> <span data-ttu-id="6f0ea-137">具体而言，由于密封的类永远不会有任何派生的类，则可以将密封的类实例上的虚拟函数成员调用转换为非虚拟调用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-137">In particular, because a sealed class is known to never have any derived classes, it is possible to transform virtual function member invocations on sealed class instances into non-virtual invocations.</span></span>

#### <a name="static-classes"></a><span data-ttu-id="6f0ea-138">静态类</span><span class="sxs-lookup"><span data-stu-id="6f0ea-138">Static classes</span></span>

<span data-ttu-id="6f0ea-139">`static`修饰符用于声明为类标记***静态类***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-139">The `static` modifier is used to mark the class being declared as a ***static class***.</span></span> <span data-ttu-id="6f0ea-140">静态类不能实例化，不能用作类型只能包含静态成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-140">A static class cannot be instantiated, cannot be used as a type and can contain only static members.</span></span> <span data-ttu-id="6f0ea-141">只有一个静态类可以包含扩展方法的声明 ([扩展方法](classes.md#extension-methods))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-141">Only a static class can contain declarations of extension methods ([Extension methods](classes.md#extension-methods)).</span></span>

<span data-ttu-id="6f0ea-142">静态类声明会受到以下限制：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-142">A static class declaration is subject to the following restrictions:</span></span>

*  <span data-ttu-id="6f0ea-143">可能不包括静态类`sealed`或`abstract`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-143">A static class may not include a `sealed` or `abstract` modifier.</span></span> <span data-ttu-id="6f0ea-144">但请注意，因为静态类无法实例化或派生自，其行为就像它已密封和抽象。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-144">Note, however, that since a static class cannot be instantiated or derived from, it behaves as if it was both sealed and abstract.</span></span>
*  <span data-ttu-id="6f0ea-145">可能不包括静态类*class_base*规范 ([类基规范](classes.md#class-base-specification)) 和不能显式指定的基类或实现的接口的列表。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-145">A static class may not include a *class_base* specification ([Class base specification](classes.md#class-base-specification)) and cannot explicitly specify a base class or a list of implemented interfaces.</span></span> <span data-ttu-id="6f0ea-146">静态类隐式继承自类型`object`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-146">A static class implicitly inherits from type `object`.</span></span>
*  <span data-ttu-id="6f0ea-147">静态类只能包含静态成员 ([静态和实例成员](classes.md#static-and-instance-members))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-147">A static class can only contain static members ([Static and instance members](classes.md#static-and-instance-members)).</span></span> <span data-ttu-id="6f0ea-148">请注意，常量和嵌套的类型归为静态成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-148">Note that constants and nested types are classified as static members.</span></span>
*  <span data-ttu-id="6f0ea-149">静态类不能具有与成员`protected`或`protected internal`声明可访问性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-149">A static class cannot have members with `protected` or `protected internal` declared accessibility.</span></span>

<span data-ttu-id="6f0ea-150">它是一个编译时错误违反任何这些限制。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-150">It is a compile-time error to violate any of these restrictions.</span></span>

<span data-ttu-id="6f0ea-151">静态类有实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-151">A static class has no instance constructors.</span></span> <span data-ttu-id="6f0ea-152">不能在静态类中的实例构造函数，没有默认实例构造函数声明 ([默认构造函数](classes.md#default-constructors)) 提供静态的类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-152">It is not possible to declare an instance constructor in a static class, and no default instance constructor ([Default constructors](classes.md#default-constructors)) is provided for a static class.</span></span>

<span data-ttu-id="6f0ea-153">静态类的成员不是自动静态的并且必须显式包含成员声明`static`（除外常量和嵌套的类型） 的修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-153">The members of a static class are not automatically static, and the member declarations must explicitly include a `static` modifier (except for constants and nested types).</span></span> <span data-ttu-id="6f0ea-154">当一个类嵌套在外部静态类时，嵌套的类不是静态类除非显式包括`static`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-154">When a class is nested within a static outer class, the nested class is not a static class unless it explicitly includes a `static` modifier.</span></span>

<span data-ttu-id="6f0ea-155">__引用静态类类型__</span><span class="sxs-lookup"><span data-stu-id="6f0ea-155">__Referencing static class types__</span></span>

<span data-ttu-id="6f0ea-156">一个*namespace_or_type_name* ([Namespace 和类型名称](basic-concepts.md#namespace-and-type-names)) 允许以引用静态类，如果</span><span class="sxs-lookup"><span data-stu-id="6f0ea-156">A *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="6f0ea-157">*Namespace_or_type_name*是`T`中*namespace_or_type_name*窗体的`T.I`，或</span><span class="sxs-lookup"><span data-stu-id="6f0ea-157">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="6f0ea-158">*Namespace_or_type_name*是`T`中*typeof_expression* ([自变量列表](expressions.md#argument-lists)1) 的窗体`typeof(T)`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-158">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

<span data-ttu-id="6f0ea-159">一个*primary_expression* ([函数成员](expressions.md#function-members)) 允许以引用静态类，如果</span><span class="sxs-lookup"><span data-stu-id="6f0ea-159">A *primary_expression* ([Function members](expressions.md#function-members)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="6f0ea-160">*Primary_expression*是`E`中*member_access* ([编译时检查的动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) 的窗体`E.I`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-160">The *primary_expression* is the `E` in a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) of the form `E.I`.</span></span>

<span data-ttu-id="6f0ea-161">在任何其他上下文中是编译时错误，以引用静态类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-161">In any other context it is a compile-time error to reference a static class.</span></span> <span data-ttu-id="6f0ea-162">例如，它是错误的一个静态类用作基类，构成类型 ([嵌套类型](classes.md#nested-types)) 的成员、 泛型类型参数或类型参数约束。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-162">For example, it is an error for a static class to be used as a base class, a constituent type ([Nested types](classes.md#nested-types)) of a member, a generic type argument, or a type parameter constraint.</span></span> <span data-ttu-id="6f0ea-163">同样，在数组类型、 指针类型，不能使用静态类`new`表达式、 强制转换表达式，`is`表达式`as`表达式，`sizeof`表达式或默认值表达式。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-163">Likewise, a static class cannot be used in an array type, a pointer type, a `new` expression, a cast expression, an `is` expression, an `as` expression, a `sizeof` expression, or a default value expression.</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="6f0ea-164">Partial 修饰符</span><span class="sxs-lookup"><span data-stu-id="6f0ea-164">Partial modifier</span></span>

<span data-ttu-id="6f0ea-165">`partial`修饰符用于指示此*class_declaration*是分部类型声明。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-165">The `partial` modifier is used to indicate that this *class_declaration* is a partial type declaration.</span></span> <span data-ttu-id="6f0ea-166">具有相同名称的封闭命名空间或类型声明中的多个分部类型声明组合到窗体的一种类型声明，遵循的规则中指定[分部类型](classes.md#partial-types)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-166">Multiple partial type declarations with the same name within an enclosing namespace or type declaration combine to form one type declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

<span data-ttu-id="6f0ea-167">如果生产或在不同的上下文中维护这些段具有分布在单独的程序文本段的类的声明可以很有用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-167">Having the declaration of a class distributed over separate segments of program text can be useful if these segments are produced or maintained in different contexts.</span></span> <span data-ttu-id="6f0ea-168">例如，类声明的一部分可能是由计算机生成，而手动编写的其他。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-168">For instance, one part of a class declaration may be machine generated, whereas the other is manually authored.</span></span> <span data-ttu-id="6f0ea-169">两个文本分隔可以防止用一个与更新冲突由其他更新。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-169">Textual separation of the two prevents updates by one from conflicting with updates by the other.</span></span>

### <a name="type-parameters"></a><span data-ttu-id="6f0ea-170">类型参数</span><span class="sxs-lookup"><span data-stu-id="6f0ea-170">Type parameters</span></span>

<span data-ttu-id="6f0ea-171">类型参数是一个简单标识符，表示所提供的用于创建构造的类型的类型参数的占位符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-171">A type parameter is a simple identifier that denotes a placeholder for a type argument supplied to create a constructed type.</span></span> <span data-ttu-id="6f0ea-172">类型参数是将更高版本提供的类型的正式占位符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-172">A type parameter is a formal placeholder for a type that will be supplied later.</span></span> <span data-ttu-id="6f0ea-173">与之相反，类型参数 ([类型参数](types.md#type-arguments)) 是创建构造的类型时替换为类型参数的实际类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-173">By contrast, a type argument ([Type arguments](types.md#type-arguments)) is the actual type that is substituted for the type parameter when a constructed type is created.</span></span>

```antlr
type_parameter_list
    : '<' type_parameters '>'
    ;

type_parameters
    : attributes? type_parameter
    | type_parameters ',' attributes? type_parameter
    ;

type_parameter
    : identifier
    ;
```

<span data-ttu-id="6f0ea-174">在类声明中的每个类型参数声明空间中定义一个名称 ([声明](basic-concepts.md#declarations)) 的类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-174">Each type parameter in a class declaration defines a name in the declaration space ([Declarations](basic-concepts.md#declarations)) of that class.</span></span> <span data-ttu-id="6f0ea-175">因此，它不能与另一个类型参数相同的名称或成员在该类中声明。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-175">Thus, it cannot have the same name as another type parameter or a member declared in that class.</span></span> <span data-ttu-id="6f0ea-176">类型参数不能具有类型本身相同的名称。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-176">A type parameter cannot have the same name as the type itself.</span></span>

### <a name="class-base-specification"></a><span data-ttu-id="6f0ea-177">类的基规范</span><span class="sxs-lookup"><span data-stu-id="6f0ea-177">Class base specification</span></span>

<span data-ttu-id="6f0ea-178">类声明可能包括*class_base*规格，它定义类和接口的类的直接基类 ([接口](interfaces.md)) 直接由类实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-178">A class declaration may include a *class_base* specification, which defines the direct base class of the class and the interfaces ([Interfaces](interfaces.md)) directly implemented by the class.</span></span>

```antlr
class_base
    : ':' class_type
    | ':' interface_type_list
    | ':' class_type ',' interface_type_list
    ;

interface_type_list
    : interface_type (',' interface_type)*
    ;
```

<span data-ttu-id="6f0ea-179">在类声明中指定的基类可以是构造的类类型 ([构造类型](types.md#constructed-types))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-179">The base class specified in a class declaration can be a constructed class type ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="6f0ea-180">基类不能是自身、 类型参数，但它可能涉及到作用域中的类型参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-180">A base class cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span>

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a><span data-ttu-id="6f0ea-181">基类</span><span class="sxs-lookup"><span data-stu-id="6f0ea-181">Base classes</span></span>

<span data-ttu-id="6f0ea-182">当*class_type*中包含*class_base*，它指定要声明的类的直接基类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-182">When a *class_type* is included in the *class_base*, it specifies the direct base class of the class being declared.</span></span> <span data-ttu-id="6f0ea-183">如果类声明没有*class_base*，或者如果*class_base*列表只有接口类型、 直接基类被假定为`object`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-183">If a class declaration has no *class_base*, or if the *class_base* lists only interface types, the direct base class is assumed to be `object`.</span></span> <span data-ttu-id="6f0ea-184">类从其直接基类继承成员，如中所述[继承](classes.md#inheritance)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-184">A class inherits members from its direct base class, as described in [Inheritance](classes.md#inheritance).</span></span>

<span data-ttu-id="6f0ea-185">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-185">In the example</span></span>
```csharp
class A {}

class B: A {}
```
<span data-ttu-id="6f0ea-186">类`A`称其直接基类`B`，并`B`称为派生自`A`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-186">class `A` is said to be the direct base class of `B`, and `B` is said to be derived from `A`.</span></span> <span data-ttu-id="6f0ea-187">由于`A`does 未显式指定直接基类，其直接基类是隐式`object`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-187">Since `A` does not explicitly specify a direct base class, its direct base class is implicitly `object`.</span></span>

<span data-ttu-id="6f0ea-188">对于构造的类类型，如果在泛型类声明中，指定基类的构造类型的基类通过替换为每个*type_parameter*中相应的基类声明*type_argument*构造类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-188">For a constructed class type, if a base class is specified in the generic class declaration, the base class of the constructed type is obtained by substituting, for each *type_parameter* in the base class declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="6f0ea-189">给定泛型类声明</span><span class="sxs-lookup"><span data-stu-id="6f0ea-189">Given the generic class declarations</span></span>
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
<span data-ttu-id="6f0ea-190">构造类型的基类`G<int>`将为`B<string,int[]>`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-190">the base class of the constructed type `G<int>` would be `B<string,int[]>`.</span></span>

<span data-ttu-id="6f0ea-191">类类型的直接基类必须至少与类类型本身相同的可访问性 ([可访问性域](basic-concepts.md#accessibility-domains))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-191">The direct base class of a class type must be at least as accessible as the class type itself ([Accessibility domains](basic-concepts.md#accessibility-domains)).</span></span> <span data-ttu-id="6f0ea-192">例如，它是用于编译时错误`public`类进行派生`private`或`internal`类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-192">For example, it is a compile-time error for a `public` class to derive from a `private` or `internal` class.</span></span>

<span data-ttu-id="6f0ea-193">类类型的直接基类必须不能是任何以下类型： `System.Array`， `System.Delegate`， `System.MulticastDelegate`， `System.Enum`，或`System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-193">The direct base class of a class type must not be any of the following types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="6f0ea-194">此外，不能使用泛型类声明`System.Attribute`作为直接或间接的基类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-194">Furthermore, a generic class declaration cannot use `System.Attribute` as a direct or indirect base class.</span></span>

<span data-ttu-id="6f0ea-195">确定直接基类规范的含义时`A`类的`B`，直接基类`B`暂时被假定为`object`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-195">While determining the meaning of the direct base class specification `A` of a class `B`, the direct base class of `B` is temporarily assumed to be `object`.</span></span> <span data-ttu-id="6f0ea-196">直观地这可确保基类规范的含义，不能以递归方式依赖于自身。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-196">Intuitively this ensures that the meaning of a base class specification cannot recursively depend on itself.</span></span> <span data-ttu-id="6f0ea-197">下面的示例：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-197">The example:</span></span>
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
<span data-ttu-id="6f0ea-198">基类规范中自出错`A<C.B>`的直接基类`C`被视为可`object`，，因此 (通过的规则[Namespace 和类型名称](basic-concepts.md#namespace-and-type-names))`C`不被视为到有一个成员`B`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-198">is in error since in the base class specification `A<C.B>` the direct base class of `C` is considered to be `object`, and hence (by the rules of [Namespace and type names](basic-concepts.md#namespace-and-type-names))  `C` is not considered to have a member `B`.</span></span>

<span data-ttu-id="6f0ea-199">类类型的基类都是直接基类和其基类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-199">The base classes of a class type are the direct base class and its base classes.</span></span> <span data-ttu-id="6f0ea-200">换而言之，组的基本类是直接基类关系的传递闭包。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-200">In other words, the set of base classes is the transitive closure of the direct base class relationship.</span></span> <span data-ttu-id="6f0ea-201">以下示例中，基类的引用`B`都`A`和`object`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-201">Referring to the example above, the base classes of `B` are `A` and `object`.</span></span> <span data-ttu-id="6f0ea-202">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-202">In the example</span></span>
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
<span data-ttu-id="6f0ea-203">类的基类`D<int>`都`C<int[]>`， `B<IComparable<int[]>>`， `A`，和`object`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-203">the base classes of `D<int>` are `C<int[]>`, `B<IComparable<int[]>>`, `A`, and `object`.</span></span>

<span data-ttu-id="6f0ea-204">除了类`object`，每个类类型都有一个直接基类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-204">Except for class `object`, every class type has exactly one direct base class.</span></span> <span data-ttu-id="6f0ea-205">`object`类没有直接的基类，并且所有其他类的最终基类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-205">The `object` class has no direct base class and is the ultimate base class of all other classes.</span></span>

<span data-ttu-id="6f0ea-206">当类`B`派生自类`A`，它是编译时错误`A`依赖于`B`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-206">When a class `B` derives from a class `A`, it is a compile-time error for `A` to depend on `B`.</span></span> <span data-ttu-id="6f0ea-207">一个类***直接依赖于***它 （如果有） 的直接基类并***直接依赖于***在其中它立即嵌套 （如果有） 的类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-207">A class ***directly depends on*** its direct base class (if any) and ***directly depends on*** the class within which it is immediately nested (if any).</span></span> <span data-ttu-id="6f0ea-208">根据此定义，完整的类所依赖的类集是反身和可传递闭包***直接依赖于***关系。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-208">Given this definition, the complete set of classes upon which a class depends is the reflexive and transitive closure of the ***directly depends on*** relationship.</span></span>

<span data-ttu-id="6f0ea-209">该示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-209">The example</span></span>
```csharp
class A: A {}
```
<span data-ttu-id="6f0ea-210">因为此类依赖于本身是错误的。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-210">is erroneous because the class depends on itself.</span></span> <span data-ttu-id="6f0ea-211">同样，示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-211">Likewise, the example</span></span>
```csharp
class A: B {}
class B: C {}
class C: A {}
```
<span data-ttu-id="6f0ea-212">错误是因为类之间循环依赖。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-212">is in error because the classes circularly depend on themselves.</span></span> <span data-ttu-id="6f0ea-213">最后，示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-213">Finally, the example</span></span>
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
<span data-ttu-id="6f0ea-214">导致编译时错误，因为`A`取决于`B.C`（它的直接基类），取决于`B`（其最近的封闭类），该循环依赖于`A`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-214">results in a compile-time error because `A` depends on `B.C` (its direct base class), which depends on `B` (its immediately enclosing class), which circularly depends on `A`.</span></span>

<span data-ttu-id="6f0ea-215">请注意类不依赖于嵌套在它的类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-215">Note that a class does not depend on the classes that are nested within it.</span></span> <span data-ttu-id="6f0ea-216">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-216">In the example</span></span>
```csharp
class A
{
    class B: A {}
}
```
<span data-ttu-id="6f0ea-217">`B` 取决于`A`(因为`A`是其直接基类和其最近的封闭类)，但`A`不依赖于`B`(因为`B`既不的基类，也不是封闭类的`A`).</span><span class="sxs-lookup"><span data-stu-id="6f0ea-217">`B` depends on `A` (because `A` is both its direct base class and its immediately enclosing class), but `A` does not depend on `B` (since `B` is neither a base class nor an enclosing class of `A`).</span></span> <span data-ttu-id="6f0ea-218">因此，此示例是有效的。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-218">Thus, the example is valid.</span></span>

<span data-ttu-id="6f0ea-219">不能从派生`sealed`类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-219">It is not possible to derive from a `sealed` class.</span></span> <span data-ttu-id="6f0ea-220">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-220">In the example</span></span>
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
<span data-ttu-id="6f0ea-221">类`B`是错误，因为它会尝试派生自`sealed`类`A`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-221">class `B` is in error because it attempts to derive from the `sealed` class `A`.</span></span>

#### <a name="interface-implementations"></a><span data-ttu-id="6f0ea-222">接口实现</span><span class="sxs-lookup"><span data-stu-id="6f0ea-222">Interface implementations</span></span>

<span data-ttu-id="6f0ea-223">一个*class_base*规范可能包括接口类型，认为字母大写类直接实现给定的接口类型的列表。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-223">A *class_base* specification may include a list of interface types, in which case the class is said to directly implement the given interface types.</span></span> <span data-ttu-id="6f0ea-224">讨论了接口实现中进一步[接口实现代码](interfaces.md#interface-implementations)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-224">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="type-parameter-constraints"></a><span data-ttu-id="6f0ea-225">类型参数约束</span><span class="sxs-lookup"><span data-stu-id="6f0ea-225">Type parameter constraints</span></span>

<span data-ttu-id="6f0ea-226">泛型类型和方法声明可以选择通过包含指定类型参数约束*type_parameter_constraints_clause*s。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-226">Generic type and method declarations can optionally specify type parameter constraints by including *type_parameter_constraints_clause*s.</span></span>

```antlr
type_parameter_constraints_clause
    : 'where' type_parameter ':' type_parameter_constraints
    ;

type_parameter_constraints
    : primary_constraint
    | secondary_constraints
    | constructor_constraint
    | primary_constraint ',' secondary_constraints
    | primary_constraint ',' constructor_constraint
    | secondary_constraints ',' constructor_constraint
    | primary_constraint ',' secondary_constraints ',' constructor_constraint
    ;

primary_constraint
    : class_type
    | 'class'
    | 'struct'
    ;

secondary_constraints
    : interface_type
    | type_parameter
    | secondary_constraints ',' interface_type
    | secondary_constraints ',' type_parameter
    ;

constructor_constraint
    : 'new' '(' ')'
    ;
```

<span data-ttu-id="6f0ea-227">每个*type_parameter_constraints_clause*包含令牌`where`后, 跟类型参数的名称后, 跟一个冒号和为该类型形参的约束的列表。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-227">Each *type_parameter_constraints_clause* consists of the token `where`, followed by the name of a type parameter, followed by a colon and the list of constraints for that type parameter.</span></span> <span data-ttu-id="6f0ea-228">可以是最多有`where`子句为每个类型参数，和`where`子句可以任何顺序列出。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-228">There can be at most one `where` clause for each type parameter, and the `where` clauses can be listed in any order.</span></span> <span data-ttu-id="6f0ea-229">像`get`并`set`属性访问器中的令牌`where`令牌不是一个关键字。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-229">Like the `get` and `set` tokens in a property accessor, the `where` token is not a keyword.</span></span>

<span data-ttu-id="6f0ea-230">给出的约束列表`where`子句可以按以下顺序包含任何以下组件： 单个主约束、 一个或多个辅助约束和构造函数约束`new()`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-230">The list of constraints given in a `where` clause can include any of the following components, in this order: a single primary constraint, one or more secondary constraints, and the constructor constraint, `new()`.</span></span>

<span data-ttu-id="6f0ea-231">主键约束可以是类类型或***引用类型约束***`class`或***值类型约束*** `struct`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-231">A primary constraint can be a class type or the ***reference type constraint*** `class` or the ***value type constraint*** `struct`.</span></span> <span data-ttu-id="6f0ea-232">辅助约束可以是*type_parameter*或*interface_type*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-232">A secondary constraint can be a *type_parameter* or *interface_type*.</span></span>

<span data-ttu-id="6f0ea-233">引用类型约束指定类型参数所用的类型实参必须是引用类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-233">The reference type constraint specifies that a type argument used for the type parameter must be a reference type.</span></span> <span data-ttu-id="6f0ea-234">所有类类型、 接口类型、 委托类型、 数组类型和已知为引用类型 （如定义如下） 的类型参数都满足此约束。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-234">All class types, interface types, delegate types, array types, and type parameters known to be a reference type (as defined below) satisfy this constraint.</span></span>

<span data-ttu-id="6f0ea-235">值类型约束指定的类型参数使用的类型实参必须是不可以为 null 的值类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-235">The value type constraint specifies that a type argument used for the type parameter must be a non-nullable value type.</span></span> <span data-ttu-id="6f0ea-236">所有不可为 null 的结构类型、 枚举类型和值类型约束的类型参数满足此约束。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-236">All non-nullable struct types, enum types, and type parameters having the value type constraint satisfy this constraint.</span></span> <span data-ttu-id="6f0ea-237">请注意，尽管分类为值类型，可以为 null 的类型 ([为 Null 的类型](types.md#nullable-types)) 不满足的值类型约束。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-237">Note that although classified as a value type, a nullable type ([Nullable types](types.md#nullable-types)) does not satisfy the value type constraint.</span></span> <span data-ttu-id="6f0ea-238">具有值类型约束的类型形参不能同时具有*constructor_constraint*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-238">A type parameter having the value type constraint cannot also have the *constructor_constraint*.</span></span>

<span data-ttu-id="6f0ea-239">指针类型从不允许作为类型参数，并且不会考虑以满足引用类型或值类型约束。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-239">Pointer types are never allowed to be type arguments and are not considered to satisfy either the reference type or value type constraints.</span></span>

<span data-ttu-id="6f0ea-240">如果约束是类类型、 接口类型或类型参数，该类型指定的最小"基类型"为该类型形参使用每个类型实参必须支持。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-240">If a constraint is a class type, an interface type, or a type parameter, that type specifies a minimal "base type" that every type argument used for that type parameter must support.</span></span> <span data-ttu-id="6f0ea-241">只要使用的构造的类型或泛型方法，将针对在编译时类型参数的约束检查的类型参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-241">Whenever a constructed type or generic method is used, the type argument is checked against the constraints on the type parameter at compile-time.</span></span> <span data-ttu-id="6f0ea-242">提供的类型实参必须满足的条件中所述[满足约束](types.md#satisfying-constraints)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-242">The type argument supplied must satisfy the conditions described in [Satisfying constraints](types.md#satisfying-constraints).</span></span>

<span data-ttu-id="6f0ea-243">一个*class_type*约束必须满足以下规则：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-243">A *class_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="6f0ea-244">类型必须是类类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-244">The type must be a class type.</span></span>
*  <span data-ttu-id="6f0ea-245">类型不能`sealed`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-245">The type must not be `sealed`.</span></span>
*  <span data-ttu-id="6f0ea-246">类型不能属于以下类型之一： `System.Array`， `System.Delegate`， `System.Enum`，或`System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-246">The type must not be one of the following types: `System.Array`, `System.Delegate`, `System.Enum`, or `System.ValueType`.</span></span>
*  <span data-ttu-id="6f0ea-247">类型不能`object`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-247">The type must not be `object`.</span></span> <span data-ttu-id="6f0ea-248">由于所有类型都派生`object`，如果已允许此类约束会产生任何影响。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-248">Because all types derive from `object`, such a constraint would have no effect if it were permitted.</span></span>
*  <span data-ttu-id="6f0ea-249">最多一个约束为给定的类型参数可以是类类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-249">At most one constraint for a given type parameter can be a class type.</span></span>

<span data-ttu-id="6f0ea-250">为指定的类型*interface_type*约束必须满足以下规则：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-250">A type specified as an *interface_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="6f0ea-251">类型必须是接口类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-251">The type must be an interface type.</span></span>
*  <span data-ttu-id="6f0ea-252">必须未指定类型不止一次在给定`where`子句。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-252">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="6f0ea-253">在任一情况下，该约束可以包括任何关联的类型或方法声明的构造类型一部分的类型参数，也可以包含要声明的类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-253">In either case, the constraint can involve any of the type parameters of the associated type or method declaration as part of a constructed type, and can involve the type being declared.</span></span>

<span data-ttu-id="6f0ea-254">指定类型参数约束必须至少与可访问性为任何类或接口类型 ([可访问性约束](basic-concepts.md#accessibility-constraints)) 作为泛型类型或所声明的方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-254">Any class or interface type specified as a type parameter constraint must be at least as accessible ([Accessibility constraints](basic-concepts.md#accessibility-constraints)) as the generic type or method being declared.</span></span>

<span data-ttu-id="6f0ea-255">为指定的类型*type_parameter*约束必须满足以下规则：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-255">A type specified as a *type_parameter* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="6f0ea-256">类型必须是类型参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-256">The type must be a type parameter.</span></span>
*  <span data-ttu-id="6f0ea-257">必须未指定类型不止一次在给定`where`子句。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-257">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="6f0ea-258">此外必须没有循环中的类型参数，其中依赖项是可传递关系定义的依赖项关系图：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-258">In addition there must be no cycles in the dependency graph of type parameters, where dependency is a transitive relation defined by:</span></span>

*  <span data-ttu-id="6f0ea-259">如果类型形参`T`用作类型参数约束`S`然后`S`***取决于*** `T`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-259">If a type parameter `T` is used as a constraint for type parameter `S` then `S` ***depends on*** `T`.</span></span>
*  <span data-ttu-id="6f0ea-260">如果类型形参`S`取决于类型参数`T`并`T`取决于类型参数`U`然后`S`***取决于*** `U`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-260">If a type parameter `S` depends on a type parameter `T` and `T` depends on a type parameter `U` then `S` ***depends on*** `U`.</span></span>

<span data-ttu-id="6f0ea-261">根据此关系，则会用于依赖于自身 （直接或间接） 的类型参数的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-261">Given this relation, it is a compile-time error for a type parameter to depend on itself (directly or indirectly).</span></span>

<span data-ttu-id="6f0ea-262">任何约束必须依赖类型参数之间保持一致。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-262">Any constraints must be consistent among dependent type parameters.</span></span> <span data-ttu-id="6f0ea-263">如果类型形参`S`取决于类型参数`T`然后：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-263">If type parameter `S` depends on type parameter `T` then:</span></span>

*  <span data-ttu-id="6f0ea-264">`T` 必须具有值类型约束。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-264">`T` must not have the value type constraint.</span></span> <span data-ttu-id="6f0ea-265">否则为`T`有效地密封的这样`S`将被强制为相同的类型为`T`，无需两个类型参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-265">Otherwise, `T` is effectively sealed so `S` would be forced to be the same type as `T`, eliminating the need for two type parameters.</span></span>
*  <span data-ttu-id="6f0ea-266">如果`S`具有值类型约束，然后`T`不能*class_type*约束。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-266">If `S` has the value type constraint then `T` must not have a *class_type* constraint.</span></span>
*  <span data-ttu-id="6f0ea-267">如果`S`已*class_type*约束`A`并`T`具有*class_type*约束`B`则必须是标识转换或隐式引用转换从`A`到`B`或从隐式引用转换`B`到`A`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-267">If `S` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>
*  <span data-ttu-id="6f0ea-268">如果`S`还取决于类型参数`U`并`U`具有*class_type*约束`A`并`T`具有*class_type*约束`B`则必须为标识转换或从隐式引用转换`A`到`B`或从隐式引用转换`B`到`A`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-268">If `S` also depends on type parameter `U` and `U` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>

<span data-ttu-id="6f0ea-269">它是有效的`S`具有值类型约束和`T`具有引用类型约束。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-269">It is valid for `S` to have the value type constraint and `T` to have the reference type constraint.</span></span> <span data-ttu-id="6f0ea-270">这将有效地限制`T`到类型`System.Object`， `System.ValueType`， `System.Enum`，和任何接口类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-270">Effectively this limits `T` to the types `System.Object`, `System.ValueType`, `System.Enum`, and any interface type.</span></span>

<span data-ttu-id="6f0ea-271">如果`where`的类型参数的子句包括构造函数约束 (其中包含窗体`new()`)，则可以使用`new`运算符来创建该类型的实例 ([对象创建表达式](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="6f0ea-271">If the `where` clause for a type parameter includes a constructor constraint (which has the form `new()`), it is possible to use the `new` operator to create instances of the type ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span> <span data-ttu-id="6f0ea-272">任何类型使用参数的构造函数约束的类型参数必须具有公共的无参数构造函数 （此构造函数隐式存在任何值类型） 也是具有值类型约束或构造函数约束 （请参阅的类型形参[类型参数约束](classes.md#type-parameter-constraints)有关详细信息)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-272">Any type argument used for a type parameter with a constructor constraint must have a public parameterless constructor (this constructor implicitly exists for any value type) or be a type parameter having the value type constraint or constructor constraint (see [Type parameter constraints](classes.md#type-parameter-constraints) for details).</span></span>

<span data-ttu-id="6f0ea-273">约束的示例如下：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-273">The following are examples of constraints:</span></span>
```csharp
interface IPrintable
{
    void Print();
}

interface IComparable<T>
{
    int CompareTo(T value);
}

interface IKeyProvider<T>
{
    T GetKey();
}

class Printer<T> where T: IPrintable {...}

class SortedList<T> where T: IComparable<T> {...}

class Dictionary<K,V>
    where K: IComparable<K>
    where V: IPrintable, IKeyProvider<K>, new()
{
    ...
}
```

<span data-ttu-id="6f0ea-274">下面的示例是错误的因为它将导致发生循环中的类型参数的依赖项关系图：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-274">The following example is in error because it causes a circularity in the dependency graph of the type parameters:</span></span>
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

<span data-ttu-id="6f0ea-275">下面的示例演示了一些无效的情况：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-275">The following examples illustrate additional invalid situations:</span></span>
```csharp
class Sealed<S,T>
    where S: T
    where T: struct        // Error, T is sealed
{
    ...
}

class A {...}

class B {...}

class Incompat<S,T>
    where S: A, T
    where T: B                // Error, incompatible class-type constraints
{
    ...
}

class StructWithClass<S,T,U>
    where S: struct, T
    where T: U
    where U: A                // Error, A incompatible with struct
{
    ...
}
```

<span data-ttu-id="6f0ea-276">***有效基类***的类型参数`T`定义，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-276">The ***effective base class*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="6f0ea-277">如果`T`没有主约束或类型参数约束，其有效的基类是`object`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-277">If `T` has no primary constraints or type parameter constraints, its effective base class is `object`.</span></span>
*  <span data-ttu-id="6f0ea-278">如果`T`具有值类型约束，其有效的基类是`System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-278">If `T` has the value type constraint, its effective base class is `System.ValueType`.</span></span>
*  <span data-ttu-id="6f0ea-279">如果`T`已*class_type*约束`C`但没有*type_parameter*约束，其有效的基类是`C`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-279">If `T` has a *class_type* constraint `C` but no *type_parameter* constraints, its effective base class is `C`.</span></span>
*  <span data-ttu-id="6f0ea-280">如果`T`不具有*class_type*约束但具有一个或多个*type_parameter*约束，其有效的基本类是包含程度最大的类型 ([提升转换运算符](conversions.md#lifted-conversion-operators)) 中的一套有效的基类及其*type_parameter*约束。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-280">If `T` has no *class_type* constraint but has one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set of effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="6f0ea-281">一致性规则可确保不存在此类包含程度最大的类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-281">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="6f0ea-282">如果`T`同时具有*class_type*约束和一个或多*type_parameter*约束，其有效的基本类是包含程度最大的类型 ([提升转换运算符](conversions.md#lifted-conversion-operators)) 组成的集合中*class_type*约束`T`和有效的基类的其*type_parameter*约束。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-282">If `T` has both a *class_type* constraint and one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set consisting of the *class_type* constraint of `T` and the effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="6f0ea-283">一致性规则可确保不存在此类包含程度最大的类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-283">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="6f0ea-284">如果`T`具有引用类型约束但不会*class_type*约束，其有效的基类是`object`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-284">If `T` has the reference type constraint but no *class_type* constraints, its effective base class is `object`.</span></span>

<span data-ttu-id="6f0ea-285">在这些规则，如果 T 具有约束`V`也就是说*value_type*，改为使用的最具体的基类型`V`即*class_type*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-285">For the purpose of these rules, if T has a constraint `V` that is a *value_type*, use instead the most specific base type of `V` that is a *class_type*.</span></span> <span data-ttu-id="6f0ea-286">这可以永远不会发生中显式给定的约束，但隐式继承通过重写方法声明或显式实现的接口方法的泛型方法的约束时可能会发生。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-286">This can never happen in an explicitly given constraint, but may occur when the constraints of a generic method are implicitly inherited by an overriding method declaration or an explicit implementation of an interface method.</span></span>

<span data-ttu-id="6f0ea-287">这些规则可确保始终是有效基类*class_type*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-287">These rules ensure that the effective base class is always a *class_type*.</span></span>

<span data-ttu-id="6f0ea-288">***有效接口集中***的类型参数`T`定义，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-288">The ***effective interface set*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="6f0ea-289">如果`T`不具有*secondary_constraints*，其有效的接口集为空。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-289">If `T` has no *secondary_constraints*, its effective interface set is empty.</span></span>
*  <span data-ttu-id="6f0ea-290">如果`T`已*interface_type*约束，但没有*type_parameter*约束，其有效的接口集是其一套*interface_type*约束。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-290">If `T` has *interface_type* constraints but no *type_parameter* constraints, its effective interface set is its set of *interface_type* constraints.</span></span>
*  <span data-ttu-id="6f0ea-291">如果`T`不具有*interface_type*约束，但具有*type_parameter*约束，其有效的接口集是有效的接口集的并集及其*得以参数*约束。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-291">If `T` has no *interface_type* constraints but has *type_parameter* constraints, its effective interface set is the union of the effective interface sets of its *type_parameter* constraints.</span></span>
*  <span data-ttu-id="6f0ea-292">如果`T`同时具有*interface_type*约束和*type_parameter*约束，其有效的接口集是其一组联合*interface_type*约束和有效的接口集及其*type_parameter*约束。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-292">If `T` has both *interface_type* constraints and *type_parameter* constraints, its effective interface set is the union of its set of *interface_type* constraints and the effective interface sets of its *type_parameter* constraints.</span></span>

<span data-ttu-id="6f0ea-293">类型参数是***已知为引用类型***如果它具有引用类型约束，或者不是其有效基类`object`或`System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-293">A type parameter is ***known to be a reference type*** if it has the reference type constraint or its effective base class is not `object` or `System.ValueType`.</span></span>

<span data-ttu-id="6f0ea-294">受约束的类型参数类型的值可以用于访问实例成员权限隐含的约束。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-294">Values of a constrained type parameter type can be used to access the instance members implied by the constraints.</span></span> <span data-ttu-id="6f0ea-295">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-295">In the example</span></span>
```csharp
interface IPrintable
{
    void Print();
}

class Printer<T> where T: IPrintable
{
    void PrintOne(T x) {
        x.Print();
    }
}
```
<span data-ttu-id="6f0ea-296">方法`IPrintable`可以直接上调用`x`因为`T`被约束为始终实现`IPrintable`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-296">the methods of `IPrintable` can be invoked directly on `x` because `T` is constrained to always implement `IPrintable`.</span></span>

### <a name="class-body"></a><span data-ttu-id="6f0ea-297">类的主体</span><span class="sxs-lookup"><span data-stu-id="6f0ea-297">Class body</span></span>

<span data-ttu-id="6f0ea-298">*Class_body*的类定义了该类的成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-298">The *class_body* of a class defines the members of that class.</span></span>

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a><span data-ttu-id="6f0ea-299">分部类型</span><span class="sxs-lookup"><span data-stu-id="6f0ea-299">Partial types</span></span>

<span data-ttu-id="6f0ea-300">类型声明可以跨多个拆分***分部类型声明***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-300">A type declaration can be split across multiple ***partial type declarations***.</span></span> <span data-ttu-id="6f0ea-301">类型声明构造从其各个部分按照规则在此部分中，先将其视为编译时和运行时处理程序的其余部分在单个声明。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-301">The type declaration is constructed from its parts by following the rules in this section, whereupon it is treated as a single declaration during the remainder of the compile-time and run-time processing of the program.</span></span>

<span data-ttu-id="6f0ea-302">一个*class_declaration*， *struct_declaration*或*interface_declaration*表示分部类型声明，如果它包含`partial`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-302">A *class_declaration*, *struct_declaration* or *interface_declaration* represents a partial type declaration if it includes a `partial` modifier.</span></span> <span data-ttu-id="6f0ea-303">`partial` 不是一个关键字，并仅充当一个修饰符，如果后面紧接着关键字之一`class`，`struct`或`interface`在类型声明中，或在类型前面`void`方法声明中。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-303">`partial` is not a keyword, and only acts as a modifier if it appears immediately before one of the keywords `class`, `struct` or `interface` in a type declaration, or before the type `void` in a method declaration.</span></span> <span data-ttu-id="6f0ea-304">在其他上下文中它可以用作常规标识符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-304">In other contexts it can be used as a normal identifier.</span></span>

<span data-ttu-id="6f0ea-305">分部类型声明的每个部分必须包含`partial`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-305">Each part of a partial type declaration must include a `partial` modifier.</span></span> <span data-ttu-id="6f0ea-306">它必须具有相同的名称，并在相同的命名空间或作为其他部件的类型声明中声明。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-306">It must have the same name  and be declared in the same namespace or type declaration as the other parts.</span></span> <span data-ttu-id="6f0ea-307">`partial`修饰符表示类型声明的其他部分可能存在其他位置，但此类中的其他部件存在并不是一项要求; 它是与单个声明类型，包括有效的`partial`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-307">The `partial` modifier indicates that additional parts of the type declaration may exist elsewhere, but the existence of such additional parts is not a requirement; it is valid for a type with a single declaration to include the `partial` modifier.</span></span>

<span data-ttu-id="6f0ea-308">分部类型的所有部分都必须一起都编译，以便这些部分可为单个类型声明在编译时合并。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-308">All parts of a partial type must be compiled together such that the parts can be merged at compile-time into a single type declaration.</span></span> <span data-ttu-id="6f0ea-309">特别是分部类型不允许已编译的类型进行扩展。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-309">Partial types specifically do not allow already compiled types to be extended.</span></span>

<span data-ttu-id="6f0ea-310">嵌套的类型可能会在多个部分中声明使用`partial`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-310">Nested types may be declared in multiple parts by using the `partial` modifier.</span></span> <span data-ttu-id="6f0ea-311">通常情况下，使用声明包含类型`partial`，并且每个部分的嵌套类型声明中包含的类型的不同部分。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-311">Typically, the containing type is declared using `partial` as well, and each part of the nested type is declared in a different part of the containing type.</span></span>

<span data-ttu-id="6f0ea-312">`partial`对委托或枚举声明不允许使用修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-312">The `partial` modifier is not permitted on delegate or enum declarations.</span></span>

### <a name="attributes"></a><span data-ttu-id="6f0ea-313">特性</span><span class="sxs-lookup"><span data-stu-id="6f0ea-313">Attributes</span></span>

<span data-ttu-id="6f0ea-314">未指定顺序组合的每个组成部分的属性取决于在分部类型的属性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-314">The attributes of a partial type are determined by combining, in an unspecified order, the attributes of each of the parts.</span></span> <span data-ttu-id="6f0ea-315">如果特性被放置在多个部分，相当于在类型上多次指定该属性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-315">If an attribute is placed on multiple parts, it is equivalent to specifying the attribute multiple times on the type.</span></span> <span data-ttu-id="6f0ea-316">例如，两个部分：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-316">For example, the two parts:</span></span>

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
<span data-ttu-id="6f0ea-317">如是等效于声明的：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-317">are equivalent to a declaration such as:</span></span>
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

<span data-ttu-id="6f0ea-318">类型参数的属性以类似方式进行组合。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-318">Attributes on type parameters combine in a similar fashion.</span></span>

### <a name="modifiers"></a><span data-ttu-id="6f0ea-319">修饰符</span><span class="sxs-lookup"><span data-stu-id="6f0ea-319">Modifiers</span></span>

<span data-ttu-id="6f0ea-320">如果分部类型声明包含的可访问性规范 ( `public`， `protected`， `internal`，和`private`修饰符) 与所有包含的可访问性规范的其他部分，它必须一致。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-320">When a partial type declaration includes an accessibility specification (the `public`, `protected`, `internal`, and `private` modifiers) it must agree with all other parts that include an accessibility specification.</span></span> <span data-ttu-id="6f0ea-321">如果在分部类型的任何部分不包含的可访问性规范，该类型将获得适当的默认可访问性 ([声明可访问性](basic-concepts.md#declared-accessibility))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-321">If no part of a partial type includes an accessibility specification, the type is given the appropriate default accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="6f0ea-322">如果一个或多个分部声明的嵌套类型包括`new`修饰符，如果嵌套的类型会隐藏继承的成员，则会报告任何警告 ([通过继承隐藏](basic-concepts.md#hiding-through-inheritance))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-322">If one or more partial declarations of a nested type include a `new` modifier, no warning is reported if the nested type hides an inherited member ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

<span data-ttu-id="6f0ea-323">如果一个或多个分部声明的类包括`abstract`修饰符，类被视为抽象 ([抽象类](classes.md#abstract-classes))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-323">If one or more partial declarations of a class include an `abstract` modifier, the class is considered abstract ([Abstract classes](classes.md#abstract-classes)).</span></span> <span data-ttu-id="6f0ea-324">否则，此类被视为非抽象类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-324">Otherwise, the class is considered non-abstract.</span></span>

<span data-ttu-id="6f0ea-325">如果一个或多个分部声明的类包括`sealed`修饰符，该类都被视为密封 ([密封类](classes.md#sealed-classes))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-325">If one or more partial declarations of a class include a `sealed` modifier, the class is considered sealed ([Sealed classes](classes.md#sealed-classes)).</span></span> <span data-ttu-id="6f0ea-326">否则，视为类未密封。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-326">Otherwise, the class is considered unsealed.</span></span>

<span data-ttu-id="6f0ea-327">请注意，一个类不能为抽象和密封。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-327">Note that a class cannot be both abstract and sealed.</span></span>

<span data-ttu-id="6f0ea-328">当`unsafe`修饰符用于在分部类型声明，只有该特定部分被视为不安全的上下文 ([不安全的上下文](unsafe-code.md#unsafe-contexts))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-328">When the `unsafe` modifier is used on a partial type declaration, only that particular part is considered an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

### <a name="type-parameters-and-constraints"></a><span data-ttu-id="6f0ea-329">类型参数和约束</span><span class="sxs-lookup"><span data-stu-id="6f0ea-329">Type parameters and constraints</span></span>

<span data-ttu-id="6f0ea-330">如果在多个部分中声明一个泛型类型，每个部分都必须声明的类型参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-330">If a generic type is declared in multiple parts, each part must state the type parameters.</span></span> <span data-ttu-id="6f0ea-331">每个部分必须具有顺序相同数量的类型参数和每个类型形参，相同的名称。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-331">Each part must have the same number of type parameters, and the same name for each type parameter, in order.</span></span>

<span data-ttu-id="6f0ea-332">当部分泛型类型声明包括约束 (`where`子句)，约束必须同意包含约束的所有其他部分。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-332">When a partial generic type declaration includes constraints (`where` clauses), the constraints must agree with all other parts that include constraints.</span></span> <span data-ttu-id="6f0ea-333">具体而言，每个包含约束的一部分必须为同一组的类型参数的约束，并且为每个类型参数设置的主要，辅助数据库，并且构造函数约束必须等效。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-333">Specifically, each part that includes constraints must have constraints for the same set of type parameters, and for each type parameter the sets of primary, secondary, and constructor constraints must be equivalent.</span></span> <span data-ttu-id="6f0ea-334">如果它们包含相同的成员的约束的两个集合相等。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-334">Two sets of constraints are equivalent if they contain the same members.</span></span> <span data-ttu-id="6f0ea-335">如果部分泛型类型的任何部分指定类型参数约束的类型参数被视为不受约束。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-335">If no part of a partial generic type specifies type parameter constraints, the type parameters are considered unconstrained.</span></span>

<span data-ttu-id="6f0ea-336">该示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-336">The example</span></span>
```csharp
partial class Dictionary<K,V>
    where K: IComparable<K>
    where V: IKeyProvider<K>, IPersistable
{
    ...
}

partial class Dictionary<K,V>
    where V: IPersistable, IKeyProvider<K>
    where K: IComparable<K>
{
    ...
}

partial class Dictionary<K,V>
{
    ...
}
```
<span data-ttu-id="6f0ea-337">是正确的因为有效地包含约束 （前两个） 的这些部分指定的相同设置的主要，辅助，和为同一组的类型参数的构造函数约束分别。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-337">is correct because those parts that include constraints (the first two) effectively specify the same set of primary, secondary, and constructor constraints for the same set of type parameters, respectively.</span></span>

### <a name="base-class"></a><span data-ttu-id="6f0ea-338">基类</span><span class="sxs-lookup"><span data-stu-id="6f0ea-338">Base class</span></span>

<span data-ttu-id="6f0ea-339">分部类声明包含基类规范时，它必须与包含基类规范的所有其他部分一致。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-339">When a partial class declaration includes a base class specification it must agree with all other parts that include a base class specification.</span></span> <span data-ttu-id="6f0ea-340">如果分部类中的任何部分不包含基类规范，则基类将成为`System.Object`([基类](classes.md#base-classes))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-340">If no part of a partial class includes a base class specification, the base class becomes `System.Object` ([Base classes](classes.md#base-classes)).</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="6f0ea-341">基接口</span><span class="sxs-lookup"><span data-stu-id="6f0ea-341">Base interfaces</span></span>

<span data-ttu-id="6f0ea-342">基接口在多个部分中声明的类型集是指定每个部分的基接口的并集。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-342">The set of base interfaces for a type declared in multiple parts is the union of the base interfaces specified on each part.</span></span> <span data-ttu-id="6f0ea-343">特定的基接口中只能指定一次每个部分，但它允许多个部分来命名相同的基接口。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-343">A particular base interface may only be named once on each part, but it is permitted for multiple parts to name the same base interface(s).</span></span> <span data-ttu-id="6f0ea-344">必须仅有的任何给定的基接口成员的一种实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-344">There must only be one implementation of the members of any given base interface.</span></span>

<span data-ttu-id="6f0ea-345">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-345">In the example</span></span>
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
<span data-ttu-id="6f0ea-346">基接口的类的一套`C`是`IA`， `IB`，和`IC`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-346">the set of base interfaces for class `C` is `IA`, `IB`, and `IC`.</span></span>

<span data-ttu-id="6f0ea-347">通常情况下，每个部分提供声明在该部分; 上的接口的实现但是，这不是一项要求。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-347">Typically, each part provides an implementation of the interface(s) declared on that part; however, this is not a requirement.</span></span> <span data-ttu-id="6f0ea-348">一个部件可能会提供声明的其他部分上的接口的实现：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-348">A part may provide the implementation for an interface declared on a different part:</span></span>
```csharp
partial class X
{
    int IComparable.CompareTo(object o) {...}
}

partial class X: IComparable
{
    ...
}
```

### <a name="members"></a><span data-ttu-id="6f0ea-349">成员</span><span class="sxs-lookup"><span data-stu-id="6f0ea-349">Members</span></span>

<span data-ttu-id="6f0ea-350">分部方法除外 ([分部方法](classes.md#partial-methods))，在多个部分中声明类型的成员的集是只需在每个部分中声明成员的集的并集。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-350">With the exception of partial methods ([Partial methods](classes.md#partial-methods)), the set of members of a type declared in multiple parts is simply the union of the set of members declared in each part.</span></span> <span data-ttu-id="6f0ea-351">正文类型声明的所有部件共享同一声明空间 ([声明](basic-concepts.md#declarations))，以及每个成员的范围 ([作用域](basic-concepts.md#scopes)) 扩展到所有部件的正文。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-351">The bodies of all parts of the type declaration share the same declaration space ([Declarations](basic-concepts.md#declarations)), and the scope of each member ([Scopes](basic-concepts.md#scopes)) extends to the bodies of all the parts.</span></span> <span data-ttu-id="6f0ea-352">任何成员的可访问域始终包括封闭类型; 所有的部分`private`成员声明为一个部分是从另一个部分可免费访问。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-352">The accessibility domain of any member always includes all the parts of the enclosing type; a `private` member declared in one part is freely accessible from another part.</span></span> <span data-ttu-id="6f0ea-353">它是要声明多个类型的部分中的同一成员的编译时错误，除非该成员的类型`partial`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-353">It is a compile-time error to declare the same member in more than one part of the type, unless that member is a type with the `partial` modifier.</span></span>

```csharp
partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int y;
    }
}

partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int z;
    }
}
```

<span data-ttu-id="6f0ea-354">在类型成员的排序到 C# 代码中，很少有效，但与其他语言和环境交互时可能很大。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-354">The ordering of members within a type is rarely significant to C# code, but may be significant when interfacing with other languages and environments.</span></span> <span data-ttu-id="6f0ea-355">在这些情况下，在多个部分中声明的类型中的成员的排序是未定义。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-355">In these cases, the ordering of members within a type declared in multiple parts is undefined.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="6f0ea-356">分部方法</span><span class="sxs-lookup"><span data-stu-id="6f0ea-356">Partial methods</span></span>

<span data-ttu-id="6f0ea-357">可以定义的类型声明的一部分中并在另一个中实现分部方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-357">Partial methods can be defined in one part of a type declaration and implemented in another.</span></span> <span data-ttu-id="6f0ea-358">实现是可选的;如果任何部分不实现分部方法，从生成的各个部分组合的类型声明中删除分部方法声明和对它的所有调用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-358">The implementation is optional; if no part implements the partial method, the partial method declaration and all calls to it are removed from the type declaration resulting from the combination of the parts.</span></span>

<span data-ttu-id="6f0ea-359">分部方法不能定义访问修饰符，但为隐式`private`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-359">Partial methods cannot define access modifiers, but are implicitly `private`.</span></span> <span data-ttu-id="6f0ea-360">它们的返回类型必须是`void`，并且其参数不能具有`out`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-360">Their return type must be `void`, and their parameters cannot have the `out` modifier.</span></span> <span data-ttu-id="6f0ea-361">标识符`partial`仅当它出现之前被识别为一个方法声明中的特殊关键字`void`类型; 否则可以使用它作为普通标识符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-361">The identifier `partial` is recognized as a special keyword in a method declaration only if it appears right before the `void` type; otherwise it can be used as a normal identifier.</span></span> <span data-ttu-id="6f0ea-362">分部方法不能显式实现接口方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-362">A partial method cannot explicitly implement interface methods.</span></span>

<span data-ttu-id="6f0ea-363">有两种类型的分部方法声明： 如果在方法声明的正文是分号，声明称***定义分部方法声明***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-363">There are two kinds of partial method declarations: If the body of the method declaration is a semicolon, the declaration is said to be a ***defining partial method declaration***.</span></span> <span data-ttu-id="6f0ea-364">如果在正文中指定为*块*，在声明称为***实现分部方法声明***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-364">If the body is given as a *block*, the declaration is said to be an ***implementing partial method declaration***.</span></span> <span data-ttu-id="6f0ea-365">在类型声明的部分之间可以有只有一个定义分部方法声明具有给定签名，并且可能只有一个实现分部方法声明具有给定签名。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-365">Across the parts of a type declaration there can be only one defining partial method declaration with a given signature, and there can be only one implementing partial method declaration with a given signature.</span></span> <span data-ttu-id="6f0ea-366">如果给定实现分部方法声明中，相应定义分部方法声明必须存在，并且声明必须匹配指定所示：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-366">If an implementing partial method declaration is given, a corresponding defining partial method declaration must exist, and the declarations must match as specified in the following:</span></span>

* <span data-ttu-id="6f0ea-367">声明必须具有相同的修饰符 （尽管不一定按相同顺序），方法名称、 类型参数的数目和数量的参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-367">The declarations must have the same modifiers (although not necessarily in the same order), method name, number of type parameters and number of parameters.</span></span>
* <span data-ttu-id="6f0ea-368">在声明中的相应参数必须具有相同的修饰符 （尽管不一定按相同顺序） 和 （取模的类型参数名称的差异） 相同的类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-368">Corresponding parameters in the declarations must have the same modifiers (although not necessarily in the same order) and the same types (modulo differences in type parameter names).</span></span>
* <span data-ttu-id="6f0ea-369">在声明中的相应类型参数必须具有相同的约束 （取模的类型参数名称的差异）。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-369">Corresponding type parameters in the declarations must have the same constraints (modulo differences in type parameter names).</span></span>

<span data-ttu-id="6f0ea-370">实现分部方法声明可以出现在为相应定义分部方法声明的相同部分。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-370">An implementing partial method declaration can appear in the same part as the corresponding defining partial method declaration.</span></span>

<span data-ttu-id="6f0ea-371">定义分部方法参与重载决策。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-371">Only a defining partial method participates in overload resolution.</span></span> <span data-ttu-id="6f0ea-372">因此，无论给定实现声明，则调用表达式可能会解析为分部方法的调用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-372">Thus, whether or not an implementing declaration is given, invocation expressions may resolve to invocations of the partial method.</span></span> <span data-ttu-id="6f0ea-373">因为分部方法始终返回`void`，此类调用表达式将始终为表达式语句。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-373">Because a partial method always returns `void`, such invocation expressions will always be expression statements.</span></span> <span data-ttu-id="6f0ea-374">此外，因为分部方法是隐式`private`，此类语句将总是出现在分部方法声明中的类型声明的部分之一。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-374">Furthermore, because a partial method is implicitly `private`, such statements will always occur within one of the parts of the type declaration within which the partial method is declared.</span></span>

<span data-ttu-id="6f0ea-375">如果分部类型声明的任何部分不包含有关给定的分部方法实现声明，只需从组合的类型声明中删除调用它的任何表达式语句。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-375">If no part of a partial type declaration contains an implementing declaration for a given partial method, any expression statement invoking it is simply removed from the combined type declaration.</span></span> <span data-ttu-id="6f0ea-376">因此调用表达式，包括任何构成的表达式，在运行时没有任何影响。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-376">Thus the invocation expression, including any constituent expressions, has no effect at run-time.</span></span> <span data-ttu-id="6f0ea-377">分部方法本身也会删除并不是组合的类型声明的成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-377">The partial method itself is also removed and will not be a member of the combined type declaration.</span></span>

<span data-ttu-id="6f0ea-378">如果实现声明存在给定的分部方法，将保留分部方法的调用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-378">If an implementing declaration exist for a given partial method, the invocations of the partial methods are retained.</span></span> <span data-ttu-id="6f0ea-379">分部方法导致在方法声明类似于实现的分部方法声明，但以下字符串除外：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-379">The partial method gives rise to a method declaration similar to the implementing partial method declaration except for the following:</span></span>

* <span data-ttu-id="6f0ea-380">`partial`修饰符不包含</span><span class="sxs-lookup"><span data-stu-id="6f0ea-380">The `partial` modifier is not included</span></span>
* <span data-ttu-id="6f0ea-381">生成的方法声明中的属性中未指定的顺序是组合定义和实现的分部方法声明的属性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-381">The attributes in the resulting method declaration are the combined attributes of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="6f0ea-382">不删除重复项。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-382">Duplicates are not removed.</span></span>
* <span data-ttu-id="6f0ea-383">生成的方法声明的参数的属性中未指定的顺序是特性的相应参数的定义和实现的分部方法声明的结合。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-383">The attributes on the parameters of the resulting method declaration are the combined attributes of the corresponding parameters of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="6f0ea-384">不删除重复项。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-384">Duplicates are not removed.</span></span>

<span data-ttu-id="6f0ea-385">如果定义声明，但不是实现声明为分部方法 M 指定，则以下限制适用：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-385">If a defining declaration but not an implementing declaration is given for a partial method M, the following restrictions apply:</span></span>

* <span data-ttu-id="6f0ea-386">它会导致编译时错误创建方法的委托 ([委托创建表达式](expressions.md#delegate-creation-expressions))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-386">It is a compile-time error to create a delegate to method ([Delegate creation expressions](expressions.md#delegate-creation-expressions)).</span></span>
* <span data-ttu-id="6f0ea-387">它会导致编译时错误是指`M`内部转换为表达式目录树类型的匿名函数 ([匿名函数转换为表达式树类型的计算](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-387">It is a compile-time error to refer to `M` inside an anonymous function that is converted to an expression tree type ([Evaluation of anonymous function conversions to expression tree types](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span></span>
* <span data-ttu-id="6f0ea-388">表达式一部分的调用发生`M`不会影响明确赋值状态 ([明确赋值](variables.md#definite-assignment))，这可能会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-388">Expressions occurring as part of an invocation of `M` do not affect the definite assignment state ([Definite assignment](variables.md#definite-assignment)), which can potentially lead to compile-time errors.</span></span>
* <span data-ttu-id="6f0ea-389">`M` 不能为应用程序的入口点 ([应用程序启动](basic-concepts.md#application-startup))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-389">`M` cannot be the entry point for an application ([Application Startup](basic-concepts.md#application-startup)).</span></span>

<span data-ttu-id="6f0ea-390">分部方法可用于允许自定义的另一个部件，例如，一个由工具生成的行为的类型声明的一部分。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-390">Partial methods are useful for allowing one part of a type declaration to customize the behavior of another part, e.g., one that is generated by a tool.</span></span> <span data-ttu-id="6f0ea-391">请考虑下面的分部类声明：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-391">Consider the following partial class declaration:</span></span>
```csharp
partial class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    partial void OnNameChanging(string newName);

    partial void OnNameChanged();
}
```

<span data-ttu-id="6f0ea-392">如果没有任何其他部分的情况下编译此类后，定义分部方法声明和其调用将被删除，并将等效于以下的结果组合的类声明：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-392">If this class is compiled without any other parts, the defining partial method declarations and their invocations will be removed, and the resulting combined class declaration will be equivalent to the following:</span></span>
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set { name = value; }
    }
}
```

<span data-ttu-id="6f0ea-393">假定，另一个部分给出，但是，其中提供了实现的分部方法声明：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-393">Assume that another part is given, however, which provides implementing declarations of the partial methods:</span></span>
```csharp
partial class Customer
{
    partial void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    partial void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

<span data-ttu-id="6f0ea-394">则生成的组合的类声明将等效于以下：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-394">Then the resulting combined class declaration will be equivalent to the following:</span></span>
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

### <a name="name-binding"></a><span data-ttu-id="6f0ea-395">名称绑定</span><span class="sxs-lookup"><span data-stu-id="6f0ea-395">Name binding</span></span>

<span data-ttu-id="6f0ea-396">虽然必须在相同的命名空间中声明的可扩展类型的每个部分，通常在不同的命名空间声明内编写部分。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-396">Although each part of an extensible type must be declared within the same namespace, the parts are typically written within different namespace declarations.</span></span> <span data-ttu-id="6f0ea-397">因此，不同`using`指令 ([Using 指令](namespaces.md#using-directives)) 可能会显示每个部分。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-397">Thus, different `using` directives ([Using directives](namespaces.md#using-directives)) may be present for each part.</span></span> <span data-ttu-id="6f0ea-398">解释的简单名称时 ([类型推理](expressions.md#type-inference)) 中一个部分，仅`using`被视为封闭该部分的命名空间定义的指令。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-398">When interpreting simple names ([Type inference](expressions.md#type-inference)) within one part, only the `using` directives of the namespace declaration(s) enclosing that part are considered.</span></span> <span data-ttu-id="6f0ea-399">这可能会导致相同的标识符不同部分中具有不同的含义：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-399">This may result in the same identifier having different meanings in different parts:</span></span>
```csharp
namespace N
{
    using List = System.Collections.ArrayList;

    partial class A
    {
        List x;                // x has type System.Collections.ArrayList
    }
}

namespace N
{
    using List = Widgets.LinkedList;

    partial class A
    {
        List y;                // y has type Widgets.LinkedList
    }
}
```

## <a name="class-members"></a><span data-ttu-id="6f0ea-400">类成员</span><span class="sxs-lookup"><span data-stu-id="6f0ea-400">Class members</span></span>

<span data-ttu-id="6f0ea-401">类的成员包含引入的成员及其*class_member_declaration*从直接基类和成员继承。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-401">The members of a class consist of the members introduced by its *class_member_declaration*s and the members inherited from the direct base class.</span></span>

```antlr
class_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | destructor_declaration
    | static_constructor_declaration
    | type_declaration
    ;
```

<span data-ttu-id="6f0ea-402">类类型的成员分为以下类别：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-402">The members of a class type are divided into the following categories:</span></span>

*  <span data-ttu-id="6f0ea-403">常量，表示与类关联的常量值 ([常量](classes.md#constants))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-403">Constants, which represent constant values associated with the class ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="6f0ea-404">字段，它们将类的变量 ([字段](classes.md#fields))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-404">Fields, which are the variables of the class ([Fields](classes.md#fields)).</span></span>
*  <span data-ttu-id="6f0ea-405">方法，用于实现计算和可以由类执行的操作 ([方法](classes.md#methods))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-405">Methods, which implement the computations and actions that can be performed by the class ([Methods](classes.md#methods)).</span></span>
*  <span data-ttu-id="6f0ea-406">属性，用于定义名为特征和与读取和写入这些特性关联的操作 ([属性](classes.md#properties))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-406">Properties, which define named characteristics and the actions associated with reading and writing those characteristics ([Properties](classes.md#properties)).</span></span>
*  <span data-ttu-id="6f0ea-407">定义可以由类生成的通知的事件 ([事件](classes.md#events))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-407">Events, which define notifications that can be generated by the class ([Events](classes.md#events)).</span></span>
*  <span data-ttu-id="6f0ea-408">索引器，使相同的方式要编制索引的类的实例 （语法） 作为数组 ([索引器](classes.md#indexers))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-408">Indexers, which permit instances of the class to be indexed in the same way (syntactically) as arrays ([Indexers](classes.md#indexers)).</span></span>
*  <span data-ttu-id="6f0ea-409">运算符定义可以应用于类的实例的表达式运算符 ([运算符](classes.md#operators))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-409">Operators, which define the expression operators that can be applied to instances of the class ([Operators](classes.md#operators)).</span></span>
*  <span data-ttu-id="6f0ea-410">实例构造函数，实现初始化类的实例所需的操作 ([实例构造函数](classes.md#instance-constructors))</span><span class="sxs-lookup"><span data-stu-id="6f0ea-410">Instance constructors, which implement the actions required to initialize instances of the class ([Instance constructors](classes.md#instance-constructors))</span></span>
*  <span data-ttu-id="6f0ea-411">析构函数，实现永久放弃类的实例之前要执行的操作 ([析构函数](classes.md#destructors))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-411">Destructors, which implement the actions to be performed before instances of the class are permanently discarded ([Destructors](classes.md#destructors)).</span></span>
*  <span data-ttu-id="6f0ea-412">静态构造函数，用于实现初始化类本身所需的操作 ([静态构造函数](classes.md#static-constructors))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-412">Static constructors, which implement the actions required to initialize the class itself ([Static constructors](classes.md#static-constructors)).</span></span>
*  <span data-ttu-id="6f0ea-413">表示类型的本地类的类型 ([嵌套类型](classes.md#nested-types))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-413">Types, which represent the types that are local to the class ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="6f0ea-414">可以包含可执行代码的成员统称为*函数成员*类类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-414">Members that can contain executable code are collectively known as the *function members* of the class type.</span></span> <span data-ttu-id="6f0ea-415">类类型的函数成员是方法、 属性、 事件、 索引器、 运算符、 实例构造函数、 析构函数和类类型的静态构造函数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-415">The function members of a class type are the methods, properties, events, indexers, operators, instance constructors,  destructors, and static constructors of that class type.</span></span>

<span data-ttu-id="6f0ea-416">一个*class_declaration*创建一个新的声明空间 ([声明](basic-concepts.md#declarations))，并且*class_member_declaration*s 立即所包含的*类_declaration*引入此声明空间的新成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-416">A *class_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *class_member_declaration*s immediately contained by the *class_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="6f0ea-417">以下规则适用于*class_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="6f0ea-417">The following rules apply to *class_member_declaration*s:</span></span>

*  <span data-ttu-id="6f0ea-418">实例构造函数、 析构函数和静态构造函数必须具有与最近的封闭类同名。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-418">Instance constructors, destructors and static constructors must have the same name as the immediately enclosing class.</span></span> <span data-ttu-id="6f0ea-419">所有其他成员必须具有不同于最近的封闭类的名称的名称。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-419">All other members must have names that differ from the name of the immediately enclosing class.</span></span>
*  <span data-ttu-id="6f0ea-420">常量、 字段、 属性、 事件或类型的名称必须不同于在同一类中声明的所有其他成员的名称。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-420">The name of a constant, field, property, event, or type must differ from the names of all other members declared in the same class.</span></span>
*  <span data-ttu-id="6f0ea-421">方法的名称必须不同于其他所有非-中声明的方法相同的类的名称。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-421">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="6f0ea-422">此外，签名 ([签名和超载](basic-concepts.md#signatures-and-overloading)) 的一种方法必须不同于同一类中声明的所有其他方法的签名和完全不同的签名不能在同一个类中声明的两种方法通过`ref`和`out`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-422">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="6f0ea-423">实例构造函数的签名必须不同于在同一类中声明所有其他实例构造函数的签名，并且完全由不同的签名不能在同一个类中声明的两个构造函数`ref`和`out`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-423">The signature of an instance constructor must differ from the signatures of all other instance constructors declared in the same class, and two constructors declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="6f0ea-424">索引器的签名必须不同于在同一类中声明的所有其他索引器的签名。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-424">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>
*  <span data-ttu-id="6f0ea-425">运算符的签名必须不同于在同一类中声明的所有其他运算符的签名。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-425">The signature of an operator must differ from the signatures of all other operators declared in the same class.</span></span>

<span data-ttu-id="6f0ea-426">类类型的继承的成员 ([继承](classes.md#inheritance)) 不是一个类的声明空间的一部分。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-426">The inherited members of a class type ([Inheritance](classes.md#inheritance)) are not part of the declaration space of a class.</span></span> <span data-ttu-id="6f0ea-427">因此，允许派生的类声明一个具有相同名称或签名为继承的成员 （这实质上隐藏继承的成员） 的成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-427">Thus, a derived class is allowed to declare a member with the same name or signature as an inherited member (which in effect hides the inherited member).</span></span>

### <a name="the-instance-type"></a><span data-ttu-id="6f0ea-428">实例类型</span><span class="sxs-lookup"><span data-stu-id="6f0ea-428">The instance type</span></span>

<span data-ttu-id="6f0ea-429">每个类声明有关联的绑定的类型 ([绑定，并且未绑定的类型](types.md#bound-and-unbound-types))，则***实例类型***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-429">Each class declaration has an associated bound type ([Bound and unbound types](types.md#bound-and-unbound-types)), the ***instance type***.</span></span> <span data-ttu-id="6f0ea-430">泛型类声明，该实例类型形成通过创建构造的类型 ([构造类型](types.md#constructed-types)) 从类型声明中，与每个所提供的类型自变量的相应类型参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-430">For a generic class declaration, the instance type is formed by creating a constructed type ([Constructed types](types.md#constructed-types)) from the type declaration, with each of the supplied type arguments being the corresponding type parameter.</span></span> <span data-ttu-id="6f0ea-431">由于实例类型使用的类型参数，它只能在类型参数的位置是在范围内;也就是说，在类声明中。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-431">Since the instance type uses the type parameters, it can only be used where the type parameters are in scope; that is, inside the class declaration.</span></span> <span data-ttu-id="6f0ea-432">实例类型是一种`this`在类声明的内部编写的代码。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-432">The instance type is the type of `this` for code written inside the class declaration.</span></span> <span data-ttu-id="6f0ea-433">对于非泛型类的实例类型是只需声明的类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-433">For non-generic classes, the instance type is simply the declared class.</span></span> <span data-ttu-id="6f0ea-434">下面显示了多个类声明，以及其实例类型：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-434">The following shows several class declarations along with their instance types:</span></span> 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a><span data-ttu-id="6f0ea-435">构造类型的成员</span><span class="sxs-lookup"><span data-stu-id="6f0ea-435">Members of constructed types</span></span>

<span data-ttu-id="6f0ea-436">构造类型的非继承成员获取，只需替换为每个*type_parameter*在成员声明中，相应*type_argument*构造类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-436">The non-inherited members of a constructed type are obtained by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="6f0ea-437">替换进程取决于语义含义的类型声明，并不是只是文本替换。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-437">The substitution process is based on the semantic meaning of type declarations, and is not simply textual substitution.</span></span>

<span data-ttu-id="6f0ea-438">例如，给定泛型类声明</span><span class="sxs-lookup"><span data-stu-id="6f0ea-438">For example, given the generic class declaration</span></span>
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
<span data-ttu-id="6f0ea-439">构造的类型`Gen<int[],IComparable<string>>`具有下列成员：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-439">the constructed type `Gen<int[],IComparable<string>>` has the following members:</span></span>
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

<span data-ttu-id="6f0ea-440">成员类型`a`泛型类声明中`Gen`是"的二维数组`T`"，因此该成员的类型`a`在上面的构造类型是"二维数组的一维数组`int`"，或`int[,][]`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-440">The type of the member `a` in the generic class declaration `Gen` is "two-dimensional array of `T`", so the type of the member `a` in the constructed type above is "two-dimensional array of one-dimensional array of `int`", or `int[,][]`.</span></span>

<span data-ttu-id="6f0ea-441">中的类型的实例函数成员`this`是使用实例类型 ([的实例类型](classes.md#the-instance-type)) 包含声明。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-441">Within instance function members, the type of `this` is the instance type ([The instance type](classes.md#the-instance-type)) of the containing declaration.</span></span>

<span data-ttu-id="6f0ea-442">泛型类的所有成员可以都使用任何封闭类中的类型参数直接或作为构造类型的一部分。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-442">All members of a generic class can use type parameters from any enclosing class, either directly or as part of a constructed type.</span></span> <span data-ttu-id="6f0ea-443">当关闭特定构造类型 ([开放类型和封闭类型](types.md#open-and-closed-types)) 使用在运行时，每次使用类型参数替换为提供给构造的类型的实际类型实参。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-443">When a particular closed constructed type ([Open and closed types](types.md#open-and-closed-types)) is used at run-time, each use of a type parameter is replaced with the actual type argument supplied to the constructed type.</span></span> <span data-ttu-id="6f0ea-444">例如：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-444">For example:</span></span>
```csharp
class C<V>
{
    public V f1;
    public C<V> f2 = null;

    public C(V x) {
        this.f1 = x;
        this.f2 = this;
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>(1);
        Console.WriteLine(x1.f1);        // Prints 1

        C<double> x2 = new C<double>(3.1415);
        Console.WriteLine(x2.f1);        // Prints 3.1415
    }
}
```

### <a name="inheritance"></a><span data-ttu-id="6f0ea-445">继承</span><span class="sxs-lookup"><span data-stu-id="6f0ea-445">Inheritance</span></span>

<span data-ttu-id="6f0ea-446">一个类***继承***其直接基类类型的成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-446">A class ***inherits*** the members of its direct base class type.</span></span> <span data-ttu-id="6f0ea-447">继承是指一个类隐式包含其直接基类类型，除实例构造函数、 析构函数和静态构造函数的类的基类的所有成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-447">Inheritance means that a class implicitly contains all members of its direct base class type, except for the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="6f0ea-448">继承的一些重要方面是：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-448">Some important aspects of inheritance are:</span></span>

*  <span data-ttu-id="6f0ea-449">继承是可传递的。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-449">Inheritance is transitive.</span></span> <span data-ttu-id="6f0ea-450">如果`C`派生自`B`，并`B`派生自`A`，然后`C`继承中声明成员`B`中声明的成员以及`A`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-450">If `C` is derived from `B`, and `B` is derived from `A`, then `C` inherits the members declared in `B` as well as the members declared in `A`.</span></span>
*  <span data-ttu-id="6f0ea-451">在派生的类扩展了其直接基类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-451">A derived class extends its direct base class.</span></span> <span data-ttu-id="6f0ea-452">派生类可以其继承的类添加新成员，但无法删除继承成员的定义。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-452">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span>
*  <span data-ttu-id="6f0ea-453">实例构造函数、 析构函数和静态构造函数不会继承，但所有其他成员，而不考虑其声明的可访问性 ([成员访问](basic-concepts.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-453">Instance constructors, destructors, and static constructors are not inherited, but all other members are, regardless of their declared accessibility ([Member access](basic-concepts.md#member-access)).</span></span> <span data-ttu-id="6f0ea-454">但是，具体取决于其声明的可访问性，继承的成员可能无法访问派生类中。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-454">However, depending on their declared accessibility, inherited members might not be accessible in a derived class.</span></span>
*  <span data-ttu-id="6f0ea-455">在派生的类可以***隐藏***([通过继承隐藏](basic-concepts.md#hiding-through-inheritance)) 通过声明具有相同名称或签名的新成员的继承成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-455">A derived class can ***hide*** ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)) inherited members by declaring new members with the same name or signature.</span></span> <span data-ttu-id="6f0ea-456">请注意但是隐藏继承的成员不会删除该成员 — 它只是使该成员无法直接通过派生类访问。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-456">Note however that hiding an inherited member does not remove that member—it merely makes that member inaccessible directly through the derived class.</span></span>
*  <span data-ttu-id="6f0ea-457">类的实例包含的一组声明中的类以及其基本类和隐式转换的所有实例字段 ([隐式引用转换](conversions.md#implicit-reference-conversions)) 存在从派生的类类型到任何基类类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-457">An instance of a class contains a set of all instance fields declared in the class and its base classes, and an implicit conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from a derived class type to any of its base class types.</span></span> <span data-ttu-id="6f0ea-458">因此，对某些派生类的实例的引用可视为对任何其基类，这些类的实例的引用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-458">Thus, a reference to an instance of some derived class can be treated as a reference to an instance of any of its base classes.</span></span>
*  <span data-ttu-id="6f0ea-459">虚方法、 属性和索引器，可以声明一个类和派生的类可以重写这些函数成员的实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-459">A class can declare virtual methods, properties, and indexers, and derived classes can override the implementation of these function members.</span></span> <span data-ttu-id="6f0ea-460">这可使类显示其中一个函数成员调用执行的操作而异通过其调用该函数成员的实例的运行时类型的多态行为。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-460">This enables classes to exhibit polymorphic behavior wherein the actions performed by a function member invocation varies depending on the run-time type of the instance through which that function member is invoked.</span></span>

<span data-ttu-id="6f0ea-461">构造的类类型的继承的成员是直接基类类型的成员 ([基类](classes.md#base-classes))，它出现，只需替换相应类型的每个匹配项的类型参数的构造类型中的参数*class_base*规范。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-461">The inherited member of a constructed class type are the members of the immediate base class type ([Base classes](classes.md#base-classes)), which is found by substituting the type arguments of the constructed type for each occurrence of the corresponding type parameters in the *class_base* specification.</span></span> <span data-ttu-id="6f0ea-462">反过来，这些成员进行转换，只需替换为每个*type_parameter*在成员声明中，相应*type_argument*的*class_base*规范。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-462">These members, in turn, are transformed by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the *class_base* specification.</span></span>

```csharp
class B<U>
{
    public U F(long index) {...}
}

class D<T>: B<T[]>
{
    public T G(string s) {...}
}
```

<span data-ttu-id="6f0ea-463">在上面的示例中，构造类型`D<int>`有一个非继承的成员`public int G(string s)`通过替换类型参数来获取`int`类型参数`T`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-463">In the above example, the constructed type `D<int>` has a non-inherited member `public int G(string s)` obtained by substituting the type argument `int` for the type parameter `T`.</span></span> <span data-ttu-id="6f0ea-464">`D<int>` 还有类声明的继承的成员`B`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-464">`D<int>` also has an inherited member from the class declaration `B`.</span></span> <span data-ttu-id="6f0ea-465">确定此继承的成员时，首先确定基类类型`B<int[]>`的`D<int>`通过替换来`int`有关`T`基类规范中`B<T[]>`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-465">This inherited member is determined by first determining the base class type `B<int[]>` of `D<int>` by substituting `int` for `T` in the base class specification `B<T[]>`.</span></span> <span data-ttu-id="6f0ea-466">然后，作为类型参数`B`，`int[]`替换为`U`中`public U F(long index)`，无法完成继承的成员`public int[] F(long index)`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-466">Then, as a type argument to `B`, `int[]` is substituted for `U` in `public U F(long index)`, yielding the inherited member `public int[] F(long index)`.</span></span>

### <a name="the-new-modifier"></a><span data-ttu-id="6f0ea-467">New 修饰符</span><span class="sxs-lookup"><span data-stu-id="6f0ea-467">The new modifier</span></span>

<span data-ttu-id="6f0ea-468">一个*class_member_declaration*允许声明一个具有相同名称或签名为继承的成员的成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-468">A *class_member_declaration* is permitted to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="6f0ea-469">派生的类成员时出现这种情况，称***隐藏***基类成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-469">When this occurs, the derived class member is said to ***hide*** the base class member.</span></span> <span data-ttu-id="6f0ea-470">隐藏继承的成员不被视为错误，但它确实会导致编译器发出警告。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-470">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="6f0ea-471">若要禁止显示警告，请在派生的类成员的声明可以包括`new`修饰符以指示派生的成员用于隐藏基类成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-471">To suppress the warning, the declaration of the derived class member can include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="6f0ea-472">本主题讨论中进一步[通过继承隐藏](basic-concepts.md#hiding-through-inheritance)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-472">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="6f0ea-473">如果`new`修饰符包含在不隐藏继承的成员的声明，该结果会发出警告。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-473">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning to that effect is issued.</span></span> <span data-ttu-id="6f0ea-474">通过删除禁止显示此警告`new`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-474">This warning is suppressed by removing the `new` modifier.</span></span>

### <a name="access-modifiers"></a><span data-ttu-id="6f0ea-475">访问修饰符</span><span class="sxs-lookup"><span data-stu-id="6f0ea-475">Access modifiers</span></span>

<span data-ttu-id="6f0ea-476">一个*class_member_declaration*可以具有下列任一五个的可能类型的声明可访问性 ([声明可访问性](basic-concepts.md#declared-accessibility)): `public`， `protected internal`， `protected`， `internal`或`private`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-476">A *class_member_declaration* can have any one of the five possible kinds of declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal`, or `private`.</span></span> <span data-ttu-id="6f0ea-477">除`protected internal`组合，它会导致编译时错误指定多个访问修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-477">Except for the `protected internal` combination, it is a compile-time error to specify more than one access modifier.</span></span> <span data-ttu-id="6f0ea-478">当*class_member_declaration*不包括任何访问修饰符，`private`假定。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-478">When a *class_member_declaration* does not include any access modifiers, `private` is assumed.</span></span>

### <a name="constituent-types"></a><span data-ttu-id="6f0ea-479">构成类型</span><span class="sxs-lookup"><span data-stu-id="6f0ea-479">Constituent types</span></span>

<span data-ttu-id="6f0ea-480">成员的声明中使用的类型称为构成该成员的类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-480">Types that are used in the declaration of a member are called the constituent types of that member.</span></span> <span data-ttu-id="6f0ea-481">可能的构成类型是一个常量、 字段、 属性、 事件或索引器的类型、 方法或运算符的返回类型和方法、 索引器、 运算符或实例构造函数的参数类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-481">Possible constituent types are the type of a constant, field, property, event, or indexer, the return type of a method or operator, and the parameter types of a method, indexer, operator, or instance constructor.</span></span> <span data-ttu-id="6f0ea-482">成员的构成类型必须至少与该成员本身具有同样的可访问性 ([可访问性约束](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-482">The constituent types of a member must be at least as accessible as that member itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

### <a name="static-and-instance-members"></a><span data-ttu-id="6f0ea-483">静态和实例成员</span><span class="sxs-lookup"><span data-stu-id="6f0ea-483">Static and instance members</span></span>

<span data-ttu-id="6f0ea-484">类的成员为***静态成员***或***实例成员***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-484">Members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="6f0ea-485">通常情况下，最好考虑为属于类类型和实例成员为属于对象 （类类型的实例） 的静态成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-485">Generally speaking, it is useful to think of static members as belonging to class types and instance members as belonging to objects (instances of class types).</span></span>

<span data-ttu-id="6f0ea-486">当字段、 方法、 属性、 事件、 运算符或构造函数声明包括`static`修饰符，它声明了静态成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-486">When a field, method, property, event, operator, or constructor declaration includes a `static` modifier, it declares a static member.</span></span> <span data-ttu-id="6f0ea-487">此外，常量或类型声明隐式声明的静态成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-487">In addition, a constant or type declaration implicitly declares a static member.</span></span> <span data-ttu-id="6f0ea-488">静态类成员具有以下特征：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-488">Static members have the following characteristics:</span></span>

*  <span data-ttu-id="6f0ea-489">当静态成员`M`中引用*member_access* ([成员访问](expressions.md#member-access)) 的窗体`E.M`，`E`必须表示一个类型，其中包含`M`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-489">When a static member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote a type containing `M`.</span></span> <span data-ttu-id="6f0ea-490">它是编译时错误`E`来表示实例。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-490">It is a compile-time error for `E` to denote an instance.</span></span>
*  <span data-ttu-id="6f0ea-491">静态字段用于标识由给定已关闭的类类型的所有实例共享一个存储位置。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-491">A static field identifies exactly one storage location to be shared by all instances of a given closed class type.</span></span> <span data-ttu-id="6f0ea-492">无论创建多少个实例的已关闭的给定的类类型，是的静态字段只有一个副本。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-492">No matter how many instances of a given closed class type are created, there is only ever one copy of a static field.</span></span>
*  <span data-ttu-id="6f0ea-493">静态函数成员 （方法、 属性、 事件、 运算符或构造函数） 不会进行特定的实例、 操作，并且它会导致编译时错误是指`this`中这样的函数成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-493">A static function member (method, property, event, operator, or constructor) does not operate on a specific instance, and it is a compile-time error to refer to `this` in such a function member.</span></span>

<span data-ttu-id="6f0ea-494">当字段、 方法、 属性、 事件、 索引器、 构造函数或析构函数声明中不包括`static`修饰符，它声明实例成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-494">When a field, method, property, event, indexer, constructor, or destructor declaration does not include a `static` modifier, it declares an instance member.</span></span> <span data-ttu-id="6f0ea-495">（实例成员有时称为非静态成员。）实例成员具有以下特征：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-495">(An instance member is sometimes called a non-static member.) Instance members have the following characteristics:</span></span>

*  <span data-ttu-id="6f0ea-496">当实例成员`M`中引用*member_access* ([成员访问](expressions.md#member-access)) 的窗体`E.M`，`E`必须表示一个包含类型的一个实例`M`.</span><span class="sxs-lookup"><span data-stu-id="6f0ea-496">When an instance member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote an instance of a type containing `M`.</span></span> <span data-ttu-id="6f0ea-497">它是为绑定时错误`E`来表示一种类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-497">It is a binding-time error for `E` to denote a type.</span></span>
*  <span data-ttu-id="6f0ea-498">每个实例的类包含一组单独的类的所有实例字段。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-498">Every instance of a class contains a separate set of all instance fields of the class.</span></span>
*  <span data-ttu-id="6f0ea-499">类的给定实例上运行的实例函数成员 （方法、 属性、 索引器、 实例构造函数或析构函数） 和此实例可作为访问`this`([此访问权限](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-499">An instance function member (method, property, indexer, instance constructor, or destructor) operates on a given instance of the class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="6f0ea-500">下面的示例说明了用于访问静态的规则和实例成员：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-500">The following example illustrates the rules for accessing static and instance members:</span></span>
```csharp
class Test
{
    int x;
    static int y;

    void F() {
        x = 1;            // Ok, same as this.x = 1
        y = 1;            // Ok, same as Test.y = 1
    }

    static void G() {
        x = 1;            // Error, cannot access this.x
        y = 1;            // Ok, same as Test.y = 1
    }

    static void Main() {
        Test t = new Test();
        t.x = 1;          // Ok
        t.y = 1;          // Error, cannot access static member through instance
        Test.x = 1;       // Error, cannot access instance member through type
        Test.y = 1;       // Ok
    }
}
```

<span data-ttu-id="6f0ea-501">`F`方法演示的实例函数成员，在*simple_name* ([简单名称](expressions.md#simple-names)) 可用来访问实例成员和静态成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-501">The `F` method shows that in an instance function member, a *simple_name* ([Simple names](expressions.md#simple-names)) can be used to access both instance members and static members.</span></span> <span data-ttu-id="6f0ea-502">`G`方法显示在静态函数成员中，它是编译时错误，若要访问通过实例成员*simple_name*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-502">The `G` method shows that in a static function member, it is a compile-time error to access an instance member through a *simple_name*.</span></span> <span data-ttu-id="6f0ea-503">`Main`方法显示在*member_access* ([成员访问](expressions.md#member-access))，必须通过情况下，访问实例成员，并且必须通过类型来访问静态成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-503">The `Main` method shows that in a *member_access* ([Member access](expressions.md#member-access)), instance members must be accessed through instances, and static members must be accessed through types.</span></span>

### <a name="nested-types"></a><span data-ttu-id="6f0ea-504">嵌套类型</span><span class="sxs-lookup"><span data-stu-id="6f0ea-504">Nested types</span></span>

<span data-ttu-id="6f0ea-505">在类或结构声明中声明的类型称为***嵌套类型***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-505">A type declared within a class or struct declaration is called a ***nested type***.</span></span> <span data-ttu-id="6f0ea-506">编译单元或命名空间中声明的类型称为***非嵌套类型***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-506">A type that is declared within a compilation unit or namespace is called a ***non-nested type***.</span></span>

<span data-ttu-id="6f0ea-507">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-507">In the example</span></span>
```csharp
using System;

class A
{
    class B
    {
        static void F() {
            Console.WriteLine("A.B.F");
        }
    }
}
```
<span data-ttu-id="6f0ea-508">类`B`是嵌套的类型，因为它在类内声明`A`，和类`A`是非嵌套类型，因为它被声明为编译单元中。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-508">class `B` is a nested type because it is declared within class `A`, and class `A` is a non-nested type because it is declared within a compilation unit.</span></span>

#### <a name="fully-qualified-name"></a><span data-ttu-id="6f0ea-509">完全限定的名称</span><span class="sxs-lookup"><span data-stu-id="6f0ea-509">Fully qualified name</span></span>

<span data-ttu-id="6f0ea-510">完全限定的名称 ([完全限定名](basic-concepts.md#fully-qualified-names)) 为嵌套的类型`S.N`其中`S`是哪种类型中的类型的完全限定的名称`N`声明。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-510">The fully qualified name ([Fully qualified names](basic-concepts.md#fully-qualified-names)) for a nested type is `S.N` where `S` is the fully qualified name of the type in which type `N` is declared.</span></span>

#### <a name="declared-accessibility"></a><span data-ttu-id="6f0ea-511">声明的可访问性</span><span class="sxs-lookup"><span data-stu-id="6f0ea-511">Declared accessibility</span></span>

<span data-ttu-id="6f0ea-512">非嵌套类型可以具有`public`或`internal`声明可访问性，而且有`internal`默认情况下声明可访问性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-512">Non-nested types can have `public` or `internal` declared accessibility and have `internal` declared accessibility by default.</span></span> <span data-ttu-id="6f0ea-513">嵌套的类型可以有这些形式的声明可访问性，以及一个或多个其他形式的声明可访问性，具体取决于包含类型是类或结构：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-513">Nested types can have these forms of declared accessibility too, plus one or more additional forms of declared accessibility, depending on whether the containing type is a class or struct:</span></span>

*  <span data-ttu-id="6f0ea-514">类中声明的嵌套的类型可以具有任何声明可访问性的五个窗体 (`public`， `protected internal`， `protected`， `internal`，或`private`) 和其他类成员，如默认为`private`声明可访问性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-514">A nested type that is declared in a class can have any of five forms of declared accessibility (`public`, `protected internal`, `protected`, `internal`, or `private`) and, like other class members, defaults to `private` declared accessibility.</span></span>
*  <span data-ttu-id="6f0ea-515">在结构中声明的嵌套的类型可以具有任何三种形式的声明可访问性 (`public`， `internal`，或`private`) 和其他结构与成员一样，默认为`private`声明可访问性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-515">A nested type that is declared in a struct can have any of three forms of declared accessibility (`public`, `internal`, or `private`) and, like other struct members, defaults to `private` declared accessibility.</span></span>

<span data-ttu-id="6f0ea-516">该示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-516">The example</span></span>
```csharp
public class List
{
    // Private data structure
    private class Node
    { 
        public object Data;
        public Node Next;

        public Node(object data, Node next) {
            this.Data = data;
            this.Next = next;
        }
    }

    private Node first = null;
    private Node last = null;

    // Public interface
    public void AddToFront(object o) {...}
    public void AddToBack(object o) {...}
    public object RemoveFromFront() {...}
    public object RemoveFromBack() {...}
    public int Count { get {...} }
}
```
<span data-ttu-id="6f0ea-517">声明的私有嵌套的类`Node`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-517">declares a private nested class `Node`.</span></span>

#### <a name="hiding"></a><span data-ttu-id="6f0ea-518">隐藏</span><span class="sxs-lookup"><span data-stu-id="6f0ea-518">Hiding</span></span>

<span data-ttu-id="6f0ea-519">嵌套的类型可能会隐藏 ([名称隐藏](basic-concepts.md#name-hiding)) 基成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-519">A nested type may hide ([Name hiding](basic-concepts.md#name-hiding)) a base member.</span></span> <span data-ttu-id="6f0ea-520">`new`允许使用修饰符对嵌套的类型声明，以便明确表示隐藏。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-520">The `new` modifier is permitted on nested type declarations so that hiding can be expressed explicitly.</span></span> <span data-ttu-id="6f0ea-521">该示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-521">The example</span></span>
```csharp
using System;

class Base
{
    public static void M() {
        Console.WriteLine("Base.M");
    }
}

class Derived: Base 
{
    new public class M 
    {
        public static void F() {
            Console.WriteLine("Derived.M.F");
        }
    }
}

class Test 
{
    static void Main() {
        Derived.M.F();
    }
}
```
<span data-ttu-id="6f0ea-522">显示了嵌套的类`M`隐藏的方法`M`中定义`Base`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-522">shows a nested class `M` that hides the method `M` defined in `Base`.</span></span>

#### <a name="this-access"></a><span data-ttu-id="6f0ea-523">此访问权限</span><span class="sxs-lookup"><span data-stu-id="6f0ea-523">this access</span></span>

<span data-ttu-id="6f0ea-524">嵌套的类型和它的包含类型没有与具有特殊关系*this_access* ([此访问权限](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-524">A nested type and its containing type do not have a special relationship with regard to *this_access* ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="6f0ea-525">具体而言，`this`内嵌套类型不能用于引用包含类型的实例成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-525">Specifically, `this` within a nested type cannot be used to refer to instance members of the containing type.</span></span> <span data-ttu-id="6f0ea-526">在其中的嵌套的类型需要对它的包含类型的实例成员访问的情况下，可以通过提供提供访问`this`包含作为嵌套类型的构造函数参数类型的实例。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-526">In cases where a nested type needs access to the instance members of its containing type, access can be provided by providing the `this` for the instance of the containing type as a constructor argument for the nested type.</span></span> <span data-ttu-id="6f0ea-527">下面的示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-527">The following example</span></span>
```csharp
using System;

class C
{
    int i = 123;

    public void F() {
        Nested n = new Nested(this);
        n.G();
    }

    public class Nested
    {
        C this_c;

        public Nested(C c) {
            this_c = c;
        }

        public void G() {
            Console.WriteLine(this_c.i);
        }
    }
}

class Test
{
    static void Main() {
        C c = new C();
        c.F();
    }
}
```
<span data-ttu-id="6f0ea-528">显示了这个方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-528">shows this technique.</span></span> <span data-ttu-id="6f0ea-529">实例`C`创建的实例`Nested`，并将传递其自身`this`到`Nested`的构造函数，提供对后续访问`C`的实例成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-529">An instance of `C` creates an instance of `Nested` and passes its own `this` to `Nested`'s constructor in order to provide subsequent access to `C`'s instance members.</span></span>

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a><span data-ttu-id="6f0ea-530">对包含类型的私有和受保护成员的访问</span><span class="sxs-lookup"><span data-stu-id="6f0ea-530">Access to private and protected members of the containing type</span></span>

<span data-ttu-id="6f0ea-531">嵌套的类型有权访问它的包含类型，包括具有包含类型的成员可以访问的成员的所有`private`和`protected`声明可访问性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-531">A nested type has access to all of the members that are accessible to its containing type, including members of the containing type that have `private` and `protected` declared accessibility.</span></span> <span data-ttu-id="6f0ea-532">该示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-532">The example</span></span>
```csharp
using System;

class C 
{
    private static void F() {
        Console.WriteLine("C.F");
    }

    public class Nested 
    {
        public static void G() {
            F();
        }
    }
}

class Test 
{
    static void Main() {
        C.Nested.G();
    }
}
```
<span data-ttu-id="6f0ea-533">显示了一个类`C`，其中包含嵌套的类`Nested`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-533">shows a class `C` that contains a nested class `Nested`.</span></span> <span data-ttu-id="6f0ea-534">内`Nested`，该方法`G`调用静态方法`F`中定义`C`，和`F`具有私有声明可访问性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-534">Within `Nested`, the method `G` calls the static method `F` defined in `C`, and `F` has private declared accessibility.</span></span>

<span data-ttu-id="6f0ea-535">嵌套的类型还可以访问它的包含类型的基类型中定义的受保护的成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-535">A nested type also may access protected members defined in a base type of its containing type.</span></span> <span data-ttu-id="6f0ea-536">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-536">In the example</span></span>
```csharp
using System;

class Base 
{
    protected void F() {
        Console.WriteLine("Base.F");
    }
}

class Derived: Base 
{
    public class Nested 
    {
        public void G() {
            Derived d = new Derived();
            d.F();        // ok
        }
    }
}

class Test 
{
    static void Main() {
        Derived.Nested n = new Derived.Nested();
        n.G();
    }
}
```
<span data-ttu-id="6f0ea-537">嵌套的类`Derived.Nested`访问受保护的方法`F`中定义`Derived`的基类， `Base`，也可由的实例通过调用`Derived`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-537">the nested class `Derived.Nested` accesses the protected method `F` defined in `Derived`'s base class, `Base`, by calling through an instance of `Derived`.</span></span>

#### <a name="nested-types-in-generic-classes"></a><span data-ttu-id="6f0ea-538">泛型类中的嵌套的类型</span><span class="sxs-lookup"><span data-stu-id="6f0ea-538">Nested types in generic classes</span></span>

<span data-ttu-id="6f0ea-539">泛型类声明可以包含嵌套的类型声明。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-539">A generic class declaration can contain nested type declarations.</span></span> <span data-ttu-id="6f0ea-540">在嵌套类型，可以使用封闭类的类型参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-540">The type parameters of the enclosing class can be used within the nested types.</span></span> <span data-ttu-id="6f0ea-541">嵌套的类型声明可以包含其他类型参数仅适用于嵌套的类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-541">A nested type declaration can contain additional type parameters that apply only to the nested type.</span></span>

<span data-ttu-id="6f0ea-542">包含在泛型类声明中的每个类型声明是隐式泛型类型声明。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-542">Every type declaration contained within a generic class declaration is implicitly a generic type declaration.</span></span> <span data-ttu-id="6f0ea-543">当编写类型的引用嵌套在泛型类型时，包含构造的类型，包括其类型参数，必须具有名称。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-543">When writing a reference to a type nested within a generic type, the containing constructed type, including its type arguments, must be named.</span></span> <span data-ttu-id="6f0ea-544">但是，从外部类中的嵌套的类型可以使用而无需限定;构造的嵌套的类型时，可以隐式使用外部类的实例类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-544">However, from within the outer class, the nested type can be used without qualification; the instance type of the outer class can be implicitly used when constructing the nested type.</span></span> <span data-ttu-id="6f0ea-545">下面的示例演示三种不同的正确方式来指从创建一个构造类型`Inner`; 前两个等效：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-545">The following example shows three different correct ways to refer to a constructed type created from `Inner`; the first two are equivalent:</span></span>
```csharp
class Outer<T>
{
    class Inner<U>
    {
        public static void F(T t, U u) {...}
    }

    static void F(T t) {
        Outer<T>.Inner<string>.F(t, "abc");      // These two statements have
        Inner<string>.F(t, "abc");               // the same effect

        Outer<int>.Inner<string>.F(3, "abc");    // This type is different

        Outer.Inner<string>.F(t, "abc");         // Error, Outer needs type arg
    }
}
```

<span data-ttu-id="6f0ea-546">尽管错误编程风格，嵌套类型中的类型参数可以隐藏成员或类型参数中的外部类型声明：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-546">Although it is bad programming style, a type parameter in a nested type can hide a member or type parameter declared in the outer type:</span></span>
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a><span data-ttu-id="6f0ea-547">保留的成员名称</span><span class="sxs-lookup"><span data-stu-id="6f0ea-547">Reserved member names</span></span>

<span data-ttu-id="6f0ea-548">为了促进的基础 C# 运行时实现，为每个源成员声明中，为属性、 事件或索引器，该实现必须保留基于的类型成员声明中，其名称，以及其类型的两个方法签名。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-548">To facilitate the underlying C# run-time implementation, for each source member declaration that is a property, event, or indexer, the implementation must reserve two method signatures based on the kind of the member declaration, its name, and its type.</span></span> <span data-ttu-id="6f0ea-549">它是一个程序来声明其签名与匹配其中一种成员保留签名，即使基础运行时实现不会进行编译时错误使用这些保留。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-549">It is a compile-time error for a program to declare a member whose signature matches one of these reserved signatures, even if the underlying run-time implementation does not make use of these reservations.</span></span>

<span data-ttu-id="6f0ea-550">保留的名称不会引入声明，因此它们不参与成员查找。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-550">The reserved names do not introduce declarations, thus they do not participate in member lookup.</span></span> <span data-ttu-id="6f0ea-551">但是，在声明关联的签名是否参与继承的保留的方法 ([继承](classes.md#inheritance))，并可以使用隐藏起来`new`修饰符 ([的新修饰符](classes.md#the-new-modifier))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-551">However, a declaration's associated reserved method signatures do participate in inheritance ([Inheritance](classes.md#inheritance)), and can be hidden with the `new` modifier ([The new modifier](classes.md#the-new-modifier)).</span></span>

<span data-ttu-id="6f0ea-552">这些名称的预订有三种用途：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-552">The reservation of these names serves three purposes:</span></span>

*  <span data-ttu-id="6f0ea-553">若要允许使用一个普通的标识符作为 get 方法名称或访问权限设置为 C# 语言功能的基础实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-553">To allow the underlying implementation to use an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="6f0ea-554">若要允许其他语言进行互操作作为 get 方法名称中使用一个普通的标识符或设置对 C# 语言功能的访问。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-554">To allow other languages to interoperate using an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="6f0ea-555">若要帮助确保由一个符合的编译器接受的源接受由另一个，使细节保留成员的名称一致在所有 C# 实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-555">To help ensure that the source accepted by one conforming compiler is accepted by another, by making the specifics of reserved member names consistent across all C# implementations.</span></span>

<span data-ttu-id="6f0ea-556">析构函数声明 ([析构函数](classes.md#destructors)) 也会导致签名被保留 ([析构函数为成员名称](classes.md#member-names-reserved-for-destructors))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-556">The declaration of a destructor ([Destructors](classes.md#destructors)) also causes a signature to be reserved ([Member names reserved for destructors](classes.md#member-names-reserved-for-destructors)).</span></span>

#### <a name="member-names-reserved-for-properties"></a><span data-ttu-id="6f0ea-557">属性为保留的成员名称</span><span class="sxs-lookup"><span data-stu-id="6f0ea-557">Member names reserved for properties</span></span>

<span data-ttu-id="6f0ea-558">为属性`P`([属性](classes.md#properties)) 的类型`T`，保留了以下签名：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-558">For a property `P` ([Properties](classes.md#properties)) of type `T`, the following signatures are reserved:</span></span>

```csharp
T get_P();
void set_P(T value);
```

<span data-ttu-id="6f0ea-559">保留这两个签名，即使该属性是只读的还是只写。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-559">Both signatures are reserved, even if the property is read-only or write-only.</span></span>

<span data-ttu-id="6f0ea-560">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-560">In the example</span></span>
```csharp
using System;

class A
{
    public int P {
        get { return 123; }
    }
}

class B: A
{
    new public int get_P() {
        return 456;
    }

    new public void set_P(int value) {
    }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        Console.WriteLine(a.P);
        Console.WriteLine(b.P);
        Console.WriteLine(b.get_P());
    }
}
```
<span data-ttu-id="6f0ea-561">一个类`A`定义了一个只读属性`P`，因此保留签名`get_P`和`set_P`方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-561">a class `A` defines a read-only property `P`, thus reserving signatures for `get_P` and `set_P` methods.</span></span> <span data-ttu-id="6f0ea-562">一个类`B`派生自`A`并隐藏这两个保留的签名。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-562">A class `B` derives from `A` and hides both of these reserved signatures.</span></span> <span data-ttu-id="6f0ea-563">该示例生成输出：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-563">The example produces the output:</span></span>
```
123
123
456
```

#### <a name="member-names-reserved-for-events"></a><span data-ttu-id="6f0ea-564">事件保留的成员名称</span><span class="sxs-lookup"><span data-stu-id="6f0ea-564">Member names reserved for events</span></span>

<span data-ttu-id="6f0ea-565">事件`E`([事件](classes.md#events)) 的委托类型`T`，保留了以下签名：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-565">For an event `E` ([Events](classes.md#events)) of delegate type `T`, the following signatures are reserved:</span></span>
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a><span data-ttu-id="6f0ea-566">索引器的保留的成员名称</span><span class="sxs-lookup"><span data-stu-id="6f0ea-566">Member names reserved for indexers</span></span>

<span data-ttu-id="6f0ea-567">索引器 ([索引器](classes.md#indexers)) 的类型`T`与参数列表`L`，保留了以下签名：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-567">For an indexer ([Indexers](classes.md#indexers)) of type `T` with parameter-list `L`, the following signatures are reserved:</span></span>
```csharp
T get_Item(L);
void set_Item(L, T value);
```

<span data-ttu-id="6f0ea-568">保留这两个签名，即使索引器是只读或只写。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-568">Both signatures are reserved, even if the indexer is read-only or write-only.</span></span>

<span data-ttu-id="6f0ea-569">此外成员名称`Item`被保留。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-569">Furthermore the member name `Item` is reserved.</span></span>

#### <a name="member-names-reserved-for-destructors"></a><span data-ttu-id="6f0ea-570">成员名称保留为析构函数</span><span class="sxs-lookup"><span data-stu-id="6f0ea-570">Member names reserved for destructors</span></span>

<span data-ttu-id="6f0ea-571">类包含析构函数 ([析构函数](classes.md#destructors))，保留以下签名：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-571">For a class containing a destructor ([Destructors](classes.md#destructors)), the following signature is reserved:</span></span>
```csharp
void Finalize();
```

## <a name="constants"></a><span data-ttu-id="6f0ea-572">常量</span><span class="sxs-lookup"><span data-stu-id="6f0ea-572">Constants</span></span>

<span data-ttu-id="6f0ea-573">一个***常量***是类成员，它表示常量值： 可以在编译时计算的值。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-573">A ***constant*** is a class member that represents a constant value: a value that can be computed at compile-time.</span></span> <span data-ttu-id="6f0ea-574">一个*constant_declaration*引入了给定类型的一个或多个常量。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-574">A *constant_declaration* introduces one or more constants of a given type.</span></span>

```antlr
constant_declaration
    : attributes? constant_modifier* 'const' type constant_declarators ';'
    ;

constant_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

<span data-ttu-id="6f0ea-575">一个*constant_declaration*可能包括一套*特性*([属性](attributes.md))，`new`修饰符 ([的新修饰符](classes.md#the-new-modifier))，和有效组合的四种访问修饰符 ([访问修饰符](classes.md#access-modifiers))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-575">A *constant_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span> <span data-ttu-id="6f0ea-576">特性和修饰符应用于所有由声明的成员*constant_declaration*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-576">The attributes and modifiers apply to all of the members declared by the *constant_declaration*.</span></span> <span data-ttu-id="6f0ea-577">即使常量被视为静态成员，也是如此*constant_declaration*既不需要也不允许`static`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-577">Even though constants are considered static members, a *constant_declaration* neither requires nor allows a `static` modifier.</span></span> <span data-ttu-id="6f0ea-578">它是同一修饰符在常量声明中多次出现的错误的。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-578">It is an error for the same modifier to appear multiple times in a constant declaration.</span></span>

<span data-ttu-id="6f0ea-579">*类型*的*constant_declaration*指定该声明引入的成员的类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-579">The *type* of a *constant_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="6f0ea-580">键入后跟一系列*constant_declarator*s，其中每个引入了新的成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-580">The type is followed by a list of *constant_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="6f0ea-581">一个*constant_declarator*组成*标识符*命名的成员后, 跟"`=`"令牌后, 跟*constant_expression* ([常量表达式](expressions.md#constant-expressions)) 提供的成员的值。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-581">A *constant_declarator* consists of an *identifier* that names the member, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the member.</span></span>

<span data-ttu-id="6f0ea-582">*类型*常量中指定声明必须是`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`，`float`， `double`， `decimal`， `bool`， `string`、 一个*enum_type*，或*reference_type*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-582">The *type* specified in a constant declaration must be `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, an *enum_type*, or a *reference_type*.</span></span> <span data-ttu-id="6f0ea-583">每个*constant_expression*必须生成的目标类型或可以隐式转换为目标类型的类型的值 ([隐式转换](conversions.md#implicit-conversions))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-583">Each *constant_expression* must yield a value of the target type or of a type that can be converted to the target type by an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>

<span data-ttu-id="6f0ea-584">*类型*的一个常量必须至少与常量本身相同的可访问性 ([可访问性约束](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-584">The *type* of a constant must be at least as accessible as the constant itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6f0ea-585">在表达式中使用获取的值的常量*simple_name* ([简单名称](expressions.md#simple-names)) 或*member_access* ([成员访问](expressions.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-585">The value of a constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="6f0ea-586">一个常量本身可以参与*constant_expression*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-586">A constant can itself participate in a *constant_expression*.</span></span> <span data-ttu-id="6f0ea-587">因此，可在任何需要的构造函数中使用常量*constant_expression*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-587">Thus, a constant may be used in any construct that requires a *constant_expression*.</span></span> <span data-ttu-id="6f0ea-588">此类构造的示例包括`case`标签`goto case`语句，`enum`成员声明、 属性和其他常量声明。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-588">Examples of such constructs include `case` labels, `goto case` statements, `enum` member declarations, attributes, and other constant declarations.</span></span>

<span data-ttu-id="6f0ea-589">如中所述[常量表达式](expressions.md#constant-expressions)即*constant_expression*是可以在编译时完全计算的表达式。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-589">As described in [Constant expressions](expressions.md#constant-expressions), a *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span> <span data-ttu-id="6f0ea-590">以来创建的非 null 值的唯一办法*reference_type*而不`string`是应用`new`运算符，并且`new`运算符中不允许使用*constant_表达式*，唯一可能的值的常量*reference_type*以外的其他 s`string`是`null`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-590">Since the only way to create a non-null value of a *reference_type* other than `string` is to apply the `new` operator, and since the `new` operator is not permitted in a *constant_expression*, the only possible value for constants of *reference_type*s other than `string` is `null`.</span></span>

<span data-ttu-id="6f0ea-591">时需要的常量值的符号名称，但该值的类型时不允许在常量声明中，或当不能在通过编译时计算的值*constant_expression*、`readonly`字段 （[只读字段](classes.md#readonly-fields)) 可以改为使用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-591">When a symbolic name for a constant value is desired, but when the type of that value is not permitted in a constant declaration, or when the value cannot be computed at compile-time by a *constant_expression*, a `readonly` field ([Readonly fields](classes.md#readonly-fields)) may be used instead.</span></span>

<span data-ttu-id="6f0ea-592">声明多个常数在常量声明是等效于单个常量与相同的属性、 修饰符和类型的多个声明。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-592">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="6f0ea-593">例如</span><span class="sxs-lookup"><span data-stu-id="6f0ea-593">For example</span></span>
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
<span data-ttu-id="6f0ea-594">等效于</span><span class="sxs-lookup"><span data-stu-id="6f0ea-594">is equivalent to</span></span>
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

<span data-ttu-id="6f0ea-595">常量可以，只要依赖项不是循环依赖于在同一程序中其他常量。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-595">Constants are permitted to depend on other constants within the same program as long as the dependencies are not of a circular nature.</span></span> <span data-ttu-id="6f0ea-596">编译器自动排列要评估的常量声明中的相应顺序。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-596">The compiler automatically arranges to evaluate the constant declarations in the appropriate order.</span></span> <span data-ttu-id="6f0ea-597">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-597">In the example</span></span>
```csharp
class A
{
    public const int X = B.Z + 1;
    public const int Y = 10;
}

class B
{
    public const int Z = A.Y + 1;
}
```
<span data-ttu-id="6f0ea-598">编译器将首先计算`A.Y`，然后计算结果`B.Z`，并最后评估`A.X`，生成值`10`， `11`，并`12`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-598">the compiler first evaluates `A.Y`, then evaluates `B.Z`, and finally evaluates `A.X`, producing the values `10`, `11`, and `12`.</span></span> <span data-ttu-id="6f0ea-599">常量声明可能取决于常量从其他程序，但只能按一个方向可能此类依赖项。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-599">Constant declarations may depend on constants from other programs, but such dependencies are only possible in one direction.</span></span> <span data-ttu-id="6f0ea-600">上述示例中，如果引用`A`和`B`声明，因此在单独程序中，将有可能`A.X`依赖`B.Z`，但`B.Z`无法则不能同时取决于`A.Y`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-600">Referring to the example above, if `A` and `B` were declared in separate programs, it would be possible for `A.X` to depend on `B.Z`, but `B.Z` could then not simultaneously depend on `A.Y`.</span></span>

## <a name="fields"></a><span data-ttu-id="6f0ea-601">字段</span><span class="sxs-lookup"><span data-stu-id="6f0ea-601">Fields</span></span>

<span data-ttu-id="6f0ea-602">一个***字段***是表示与对象或类关联的变量的成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-602">A ***field*** is a member that represents a variable associated with an object or class.</span></span> <span data-ttu-id="6f0ea-603">一个*field_declaration*引入了给定类型的一个或多个字段。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-603">A *field_declaration* introduces one or more fields of a given type.</span></span>

```antlr
field_declaration
    : attributes? field_modifier* type variable_declarators ';'
    ;

field_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'readonly'
    | 'volatile'
    | field_modifier_unsafe
    ;

variable_declarators
    : variable_declarator (',' variable_declarator)*
    ;

variable_declarator
    : identifier ('=' variable_initializer)?
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

<span data-ttu-id="6f0ea-604">一个*field_declaration*可能包括一套*特性*([属性](attributes.md))，`new`修饰符 ([new 修饰符](classes.md#the-new-modifier))、有效组合的四种访问修饰符 ([访问修饰符](classes.md#access-modifiers))，和一个`static`修饰符 ([静态和实例字段](classes.md#static-and-instance-fields))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-604">A *field_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and a `static` modifier ([Static and instance fields](classes.md#static-and-instance-fields)).</span></span> <span data-ttu-id="6f0ea-605">此外， *field_declaration*可能包括`readonly`修饰符 ([只读字段](classes.md#readonly-fields)) 或`volatile`修饰符 ([可变字段](classes.md#volatile-fields))，但不可同时使用两者。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-605">In addition, a *field_declaration* may include a `readonly` modifier ([Readonly fields](classes.md#readonly-fields)) or a `volatile` modifier ([Volatile fields](classes.md#volatile-fields)) but not both.</span></span> <span data-ttu-id="6f0ea-606">特性和修饰符应用于所有由声明的成员*field_declaration*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-606">The attributes and modifiers apply to all of the members declared by the *field_declaration*.</span></span> <span data-ttu-id="6f0ea-607">它是相同的修饰符，若要在字段声明中多次出现的错误。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-607">It is an error for the same modifier to appear multiple times in a field declaration.</span></span>

<span data-ttu-id="6f0ea-608">*类型*的*field_declaration*指定该声明引入的成员的类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-608">The *type* of a *field_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="6f0ea-609">键入后跟一系列*variable_declarator*s，其中每个引入了新的成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-609">The type is followed by a list of *variable_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="6f0ea-610">一个*variable_declarator*组成*标识符*命名该成员，可以选择后跟"`=`"令牌和一个*variable_initializer* ([变量初始值设定项](classes.md#variable-initializers)) 使该成员的初始值。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-610">A *variable_declarator* consists of an *identifier* that names that member, optionally followed by an "`=`" token and a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)) that gives the initial value of that member.</span></span>

<span data-ttu-id="6f0ea-611">*类型*的字段必须至少与字段本身相同的可访问性 ([可访问性约束](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-611">The *type* of a field must be at least as accessible as the field itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6f0ea-612">在中使用表达式获取字段的值*simple_name* ([简单名称](expressions.md#simple-names)) 或*member_access* ([成员访问](expressions.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-612">The value of a field is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="6f0ea-613">使用修改非只读字段的值*赋值*([赋值运算符](expressions.md#assignment-operators))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-613">The value of a non-readonly field is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="6f0ea-614">非只读字段的值可以是获取和修改使用后缀递增和递减运算符 ([后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators)) 和前缀递增和递减运算符 ([前缀递增和递减运算符](expressions.md#prefix-increment-and-decrement-operators))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-614">The value of a non-readonly field can be both obtained and modified using postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)) and prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="6f0ea-615">声明多个字段的字段声明是等效于多个具有相同的属性、 修饰符和类型的单个字段的声明。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-615">A field declaration that declares multiple fields is equivalent to multiple declarations of single fields with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="6f0ea-616">例如</span><span class="sxs-lookup"><span data-stu-id="6f0ea-616">For example</span></span>
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
<span data-ttu-id="6f0ea-617">等效于</span><span class="sxs-lookup"><span data-stu-id="6f0ea-617">is equivalent to</span></span>
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a><span data-ttu-id="6f0ea-618">静态和实例字段</span><span class="sxs-lookup"><span data-stu-id="6f0ea-618">Static and instance fields</span></span>

<span data-ttu-id="6f0ea-619">当字段声明包括`static`修饰符，该声明引入的字段是***静态字段***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-619">When a field declaration includes a `static` modifier, the fields introduced by the declaration are ***static fields***.</span></span> <span data-ttu-id="6f0ea-620">如果未`static`修饰符存在，该声明引入的字段***实例字段***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-620">When no `static` modifier is present, the fields introduced by the declaration are ***instance fields***.</span></span> <span data-ttu-id="6f0ea-621">静态字段和实例字段是两个有几种变量 ([变量](variables.md)) 和支持 C# 中，有时它们被称为***静态变量***和***的实例变量***分别。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-621">Static fields and instance fields are two of the several kinds of variables ([Variables](variables.md)) supported by C#, and at times they are referred to as ***static variables*** and ***instance variables***, respectively.</span></span>

<span data-ttu-id="6f0ea-622">静态字段不是实例的特定; 的一部分相反，它均共享已关闭类型的所有实例 ([打开和关闭类型](types.md#open-and-closed-types))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-622">A static field is not part of a specific instance; instead, it is shared amongst all instances of a closed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="6f0ea-623">无论创建多少个实例的已关闭的类类型，没有关联的应用程序域的静态字段只有一个副本。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-623">No matter how many instances of a closed class type are created, there is only ever one copy of a static field for the associated application domain.</span></span>

<span data-ttu-id="6f0ea-624">例如：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-624">For example:</span></span>
```csharp
class C<V>
{
    static int count = 0;

    public C() {
        count++;
    }

    public static int Count {
        get { return count; }
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<double> x2 = new C<double>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<int> x3 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 2
    }
}
```

<span data-ttu-id="6f0ea-625">实例所属的实例字段。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-625">An instance field belongs to an instance.</span></span> <span data-ttu-id="6f0ea-626">具体而言，每个实例的类包含一组单独的该类的所有实例字段。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-626">Specifically, every instance of a class contains a separate set of all the instance fields of that class.</span></span>

<span data-ttu-id="6f0ea-627">字段中的引用时*member_access* ([成员访问](expressions.md#member-access)) 的窗体`E.M`，如果`M`是一个静态字段`E`必须表示一个类型，其中包含`M`如果`M`为实例字段，则 E 必须表示一个类型，其中包含的实例`M`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-627">When a field is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static field, `E` must denote a type containing `M`, and if `M` is an instance field, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="6f0ea-628">静态之间的差异和实例成员进行讨论中进一步[静态和实例成员](classes.md#static-and-instance-members)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-628">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="readonly-fields"></a><span data-ttu-id="6f0ea-629">只读字段</span><span class="sxs-lookup"><span data-stu-id="6f0ea-629">Readonly fields</span></span>

<span data-ttu-id="6f0ea-630">当*field_declaration*包括`readonly`修饰符，该声明引入的字段不***只读字段***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-630">When a *field_declaration* includes a `readonly` modifier, the fields introduced by the declaration are ***readonly fields***.</span></span> <span data-ttu-id="6f0ea-631">Readonly 字段的直接赋值只能作为的一部分，该声明或在实例构造函数或同一个类中的静态构造函数中发生。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-631">Direct assignments to readonly fields can only occur as part of that declaration or in an instance constructor or static constructor in the same class.</span></span> <span data-ttu-id="6f0ea-632">（只读字段可以被分配给多个时间在这些上下文中。）具体而言，直接分配到`readonly`字段允许仅在以下上下文中：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-632">(A readonly field can be assigned to multiple times in these contexts.) Specifically, direct assignments to a `readonly` field are permitted only in the following contexts:</span></span>

*  <span data-ttu-id="6f0ea-633">在*variable_declarator*这就带来了字段 (包括*variable_initializer*声明中)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-633">In the *variable_declarator* that introduces the field (by including a *variable_initializer* in the declaration).</span></span>
*  <span data-ttu-id="6f0ea-634">对于实例字段，在包含字段声明; 的类的实例构造函数包含字段声明的类的静态构造函数中的静态字段。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-634">For an instance field, in the instance constructors of the class that contains the field declaration; for a static field, in the static constructor of the class that contains the field declaration.</span></span> <span data-ttu-id="6f0ea-635">这些事件也是它是有效地传递的唯一上下文`readonly`字段作为`out`或`ref`参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-635">These are also the only contexts in which it is valid to pass a `readonly` field as an `out` or `ref` parameter.</span></span>

<span data-ttu-id="6f0ea-636">尝试将分配给`readonly`字段，或将其作为传递`out`或`ref`在任何其他上下文中的参数是一个编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-636">Attempting to assign to a `readonly` field or pass it as an `out` or `ref` parameter in any other context is a compile-time error.</span></span>

#### <a name="using-static-readonly-fields-for-constants"></a><span data-ttu-id="6f0ea-637">使用常量的静态只读字段</span><span class="sxs-lookup"><span data-stu-id="6f0ea-637">Using static readonly fields for constants</span></span>

<span data-ttu-id="6f0ea-638">一个`static readonly`时需要的常量值的符号名称，但当中不允许的值的类型，字段很有用`const`声明，或当不能在编译时计算的值。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-638">A `static readonly` field is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a `const` declaration, or when the value cannot be computed at compile-time.</span></span> <span data-ttu-id="6f0ea-639">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-639">In the example</span></span>
```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);

    private byte red, green, blue;

    public Color(byte r, byte g, byte b) {
        red = r;
        green = g;
        blue = b;
    }
}
```
<span data-ttu-id="6f0ea-640">`Black`， `White`， `Red`， `Green`，并`Blue`成员不能声明为`const`成员由于不能在编译时计算其值。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-640">the `Black`, `White`, `Red`, `Green`, and `Blue` members cannot be declared as `const` members because their values cannot be computed at compile-time.</span></span> <span data-ttu-id="6f0ea-641">但是，声明它们`static readonly`改为具有更相同的效果。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-641">However, declaring them `static readonly` instead has much the same effect.</span></span>

#### <a name="versioning-of-constants-and-static-readonly-fields"></a><span data-ttu-id="6f0ea-642">常量和静态只读字段的版本控制</span><span class="sxs-lookup"><span data-stu-id="6f0ea-642">Versioning of constants and static readonly fields</span></span>

<span data-ttu-id="6f0ea-643">常量和只读字段具有不同的二进制版本控制语义。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-643">Constants and readonly fields have different binary versioning semantics.</span></span> <span data-ttu-id="6f0ea-644">当表达式引用了一个常量时，在编译时，获取常量的值，但当表达式引用只读字段，直到运行时才获取字段的值。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-644">When an expression references a constant, the value of the constant is obtained at compile-time, but when an expression references a readonly field, the value of the field is not obtained until run-time.</span></span> <span data-ttu-id="6f0ea-645">请考虑包含两个单独的程序的应用程序：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-645">Consider an application that consists of two separate programs:</span></span>
```csharp
using System;

namespace Program1
{
    public class Utils
    {
        public static readonly int X = 1;
    }
}

namespace Program2
{
    class Test
    {
        static void Main() {
            Console.WriteLine(Program1.Utils.X);
        }
    }
}
```

<span data-ttu-id="6f0ea-646">`Program1`和`Program2`命名空间表示两个单独编译的程序。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-646">The `Program1` and `Program2` namespaces denote two programs that are compiled separately.</span></span> <span data-ttu-id="6f0ea-647">因为`Program1.Utils.X`被声明为静态只读字段，输出的值`Console.WriteLine`语句不在编译时已知，但而不是在运行时获取。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-647">Because `Program1.Utils.X` is declared as a static readonly field, the value output by the `Console.WriteLine` statement is not known at compile-time, but rather is obtained at run-time.</span></span> <span data-ttu-id="6f0ea-648">因此，如果的值`X`更改和`Program1`重新编写`Console.WriteLine`语句会输出新值即使`Program2`不重新编译。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-648">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` isn't recompiled.</span></span> <span data-ttu-id="6f0ea-649">但是，有`X`已常量的值`X`将时获取`Program2`编译，并且将保持不受更改影响`Program1`直到`Program2`重新编译。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-649">However, had `X` been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would remain unaffected by changes in `Program1` until `Program2` is recompiled.</span></span>

### <a name="volatile-fields"></a><span data-ttu-id="6f0ea-650">可变字段</span><span class="sxs-lookup"><span data-stu-id="6f0ea-650">Volatile fields</span></span>

<span data-ttu-id="6f0ea-651">当*field_declaration*包括`volatile`修饰符，该声明引入的字段不***可变字段***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-651">When a *field_declaration* includes a `volatile` modifier, the fields introduced by that declaration are ***volatile fields***.</span></span>

<span data-ttu-id="6f0ea-652">对于非易失性字段，对指令重新排序的优化技术可能会导致意外的且不可预测结果中访问而不进行同步，例如提供的字段的多线程程序*lock_statement* ([Lock 语句](statements.md#the-lock-statement))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-652">For non-volatile fields, optimization techniques that reorder instructions can lead to unexpected and unpredictable results in multi-threaded programs that access fields without synchronization such as that provided by the *lock_statement* ([The lock statement](statements.md#the-lock-statement)).</span></span> <span data-ttu-id="6f0ea-653">由编译器、 运行时系统或硬件，可以执行这些优化。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-653">These optimizations can be performed by the compiler, by the run-time system, or by hardware.</span></span> <span data-ttu-id="6f0ea-654">对于可变字段，此类重新排序优化受到以下限制：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-654">For volatile fields, such reordering optimizations are restricted:</span></span>

*  <span data-ttu-id="6f0ea-655">名为可变字段读取***易失读取***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-655">A read of a volatile field is called a ***volatile read***.</span></span> <span data-ttu-id="6f0ea-656">易失读取具有"获取语义;"也就是说，它保证任何对内存的引用后它对指令序列中出现之前发生。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-656">A volatile read has "acquire semantics"; that is, it is guaranteed to occur prior to any references to memory that occur after it in the instruction sequence.</span></span>
*  <span data-ttu-id="6f0ea-657">可变字段的写入称为***可变写入***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-657">A write of a volatile field is called a ***volatile write***.</span></span> <span data-ttu-id="6f0ea-658">可变写入具有"发布语义;"也就是说，它保证操作之后在对指令序列中的写入指令之前的任何内存引用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-658">A volatile write has "release semantics"; that is, it is guaranteed to happen after any memory references prior to the write instruction in the instruction sequence.</span></span>

<span data-ttu-id="6f0ea-659">这些限制确保所有线程观察易失性写入操作（由任何其他线程执行）时的观察顺序与写入操作的执行顺序一致。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-659">These restrictions ensure that all threads will observe volatile writes performed by any other thread in the order in which they were performed.</span></span> <span data-ttu-id="6f0ea-660">符合标准的实现不需要提供的单一总排序的所有线程的执行中显示的易失性写入。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-660">A conforming implementation is not required to provide a single total ordering of volatile writes as seen from all threads of execution.</span></span> <span data-ttu-id="6f0ea-661">可变字段的类型必须是以下值之一：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-661">The type of a volatile field must be one of the following:</span></span>

*  <span data-ttu-id="6f0ea-662">一个*reference_type*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-662">A *reference_type*.</span></span>
*  <span data-ttu-id="6f0ea-663">类型`byte`， `sbyte`， `short`， `ushort`， `int`， `uint`， `char`， `float`， `bool`， `System.IntPtr`，或` System.UIntPtr`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-663">The type `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, or` System.UIntPtr`.</span></span>
*  <span data-ttu-id="6f0ea-664">*Enum_type*具有枚举基类型的`byte`， `sbyte`， `short`， `ushort`， `int`，或`uint`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-664">An *enum_type* having an enum base type of `byte`, `sbyte`, `short`, `ushort`, `int`, or `uint`.</span></span>

<span data-ttu-id="6f0ea-665">该示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-665">The example</span></span>
```csharp
using System;
using System.Threading;

class Test
{
    public static int result;   
    public static volatile bool finished;

    static void Thread2() {
        result = 143;    
        finished = true; 
    }

    static void Main() {
        finished = false;

        // Run Thread2() in a new thread
        new Thread(new ThreadStart(Thread2)).Start();

        // Wait for Thread2 to signal that it has a result by setting
        // finished to true.
        for (;;) {
            if (finished) {
                Console.WriteLine("result = {0}", result);
                return;
            }
        }
    }
}
```
<span data-ttu-id="6f0ea-666">生成输出：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-666">produces the output:</span></span>
```
result = 143
```

<span data-ttu-id="6f0ea-667">在此示例中，该方法`Main`可以启动一个新线程的运行方法`Thread2`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-667">In this example, the method `Main` starts a new thread that runs the method `Thread2`.</span></span> <span data-ttu-id="6f0ea-668">此方法将值存储到一个名为的非易失性字段`result`，然后将存储`true`可变字段中`finished`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-668">This method stores a value into a non-volatile field called `result`, then stores `true` in the volatile field `finished`.</span></span> <span data-ttu-id="6f0ea-669">主线程等待字段`finished`设置为`true`，然后读取字段`result`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-669">The main thread waits for the field `finished` to be set to `true`, then reads the field `result`.</span></span> <span data-ttu-id="6f0ea-670">由于`finished`已声明`volatile`，在主线程必须读取的值`143`字段中`result`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-670">Since `finished` has been declared `volatile`, the main thread must read the value `143` from the field `result`.</span></span> <span data-ttu-id="6f0ea-671">如果该字段`finished`尚未声明`volatile`，则将允许到应用商店`result`后的存储为对主线程可见`finished`，，因此读取值的主线程`0`从字段`result`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-671">If the field `finished` had not been declared `volatile`, then it would be permissible for the store to `result` to be visible to the main thread after the store to `finished`, and hence for the main thread to read the value `0` from the field `result`.</span></span> <span data-ttu-id="6f0ea-672">声明`finished`作为`volatile`字段会阻止任何此类不一致。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-672">Declaring `finished` as a `volatile` field prevents any such inconsistency.</span></span>

### <a name="field-initialization"></a><span data-ttu-id="6f0ea-673">字段初始化</span><span class="sxs-lookup"><span data-stu-id="6f0ea-673">Field initialization</span></span>

<span data-ttu-id="6f0ea-674">字段的初始值的静态字段或实例字段，无论是默认值 ([默认值](variables.md#default-values)) 的字段的类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-674">The initial value of a field, whether it be a static field or an instance field, is the default value ([Default values](variables.md#default-values)) of the field's type.</span></span> <span data-ttu-id="6f0ea-675">不能观察到的字段值之前此默认值初始化已发生，并且字段未因此永远不会"初始化"。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-675">It is not possible to observe the value of a field before this default initialization has occurred, and a field is thus never "uninitialized".</span></span> <span data-ttu-id="6f0ea-676">该示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-676">The example</span></span>
```csharp
using System;

class Test
{
    static bool b;
    int i;

    static void Main() {
        Test t = new Test();
        Console.WriteLine("b = {0}, i = {1}", b, t.i);
    }
}
```
<span data-ttu-id="6f0ea-677">生成输出</span><span class="sxs-lookup"><span data-stu-id="6f0ea-677">produces the output</span></span>
```
b = False, i = 0
```
<span data-ttu-id="6f0ea-678">因为`b`和`i`都被自动初始化为默认值。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-678">because `b` and `i` are both automatically initialized to default values.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="6f0ea-679">变量初始值设定项</span><span class="sxs-lookup"><span data-stu-id="6f0ea-679">Variable initializers</span></span>

<span data-ttu-id="6f0ea-680">字段声明可能包括*variable_initializer*s。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-680">Field declarations may include *variable_initializer*s.</span></span> <span data-ttu-id="6f0ea-681">对于静态字段，变量初始值设定项对应于类初始化过程中执行赋值语句。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-681">For static fields, variable initializers correspond to assignment statements that are executed during class initialization.</span></span> <span data-ttu-id="6f0ea-682">对于实例字段，变量初始值设定项对应于在创建类的实例时执行的赋值语句。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-682">For instance fields, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span>

<span data-ttu-id="6f0ea-683">该示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-683">The example</span></span>
```csharp
using System;

class Test
{
    static double x = Math.Sqrt(2.0);
    int i = 100;
    string s = "Hello";

    static void Main() {
        Test a = new Test();
        Console.WriteLine("x = {0}, i = {1}, s = {2}", x, a.i, a.s);
    }
}
```
<span data-ttu-id="6f0ea-684">生成输出</span><span class="sxs-lookup"><span data-stu-id="6f0ea-684">produces the output</span></span>
```
x = 1.4142135623731, i = 100, s = Hello
```
<span data-ttu-id="6f0ea-685">因为向赋值`x`发生时执行静态字段初始值设定项和分配到`i`和`s`实例字段初始值设定项执行时发生。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-685">because an assignment to `x` occurs when static field initializers execute and assignments to `i` and `s` occur when the instance field initializers execute.</span></span>

<span data-ttu-id="6f0ea-686">中所述的默认值初始化[字段初始化](classes.md#field-initialization)出现的所有字段，包括具有变量初始值设定项的字段。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-686">The default value initialization described in [Field initialization](classes.md#field-initialization) occurs for all fields, including fields that have variable initializers.</span></span> <span data-ttu-id="6f0ea-687">因此，初始化类时，首先将该类中的所有静态字段初始化为其默认值，并且然后按文本顺序执行的静态字段初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-687">Thus, when a class is initialized, all static fields in that class are first initialized to their default values, and then the static field initializers are executed in textual order.</span></span> <span data-ttu-id="6f0ea-688">同样，创建一个类的实例后，首先将该实例中的所有实例字段都初始化为其默认值，而且然后按文本顺序执行的实例字段初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-688">Likewise, when an instance of a class is created, all instance fields in that instance are first initialized to their default values, and then the instance field initializers are executed in textual order.</span></span>

<span data-ttu-id="6f0ea-689">很可能具有变量初始值设定项在其默认值状态中观察到的静态字段。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-689">It is possible for static fields with variable initializers to be observed in their default value state.</span></span> <span data-ttu-id="6f0ea-690">但是，这是作为一种样式，强烈建议不要使用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-690">However, this is strongly discouraged as a matter of style.</span></span> <span data-ttu-id="6f0ea-691">该示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-691">The example</span></span>
```csharp
using System;

class Test
{
    static int a = b + 1;
    static int b = a + 1;

    static void Main() {
        Console.WriteLine("a = {0}, b = {1}", a, b);
    }
}
```
<span data-ttu-id="6f0ea-692">演示这一行为。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-692">exhibits this behavior.</span></span> <span data-ttu-id="6f0ea-693">尽管的循环定义，而 b，该程序是有效。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-693">Despite the circular definitions of a and b, the program is valid.</span></span> <span data-ttu-id="6f0ea-694">这会导致输出</span><span class="sxs-lookup"><span data-stu-id="6f0ea-694">It results in the output</span></span>
```
a = 1, b = 2
```
<span data-ttu-id="6f0ea-695">因为静态字段`a`并`b`初始化为`0`(的默认值为`int`) 执行其初始值设定项之前。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-695">because the static fields `a` and `b` are initialized to `0` (the default value for `int`) before their initializers are executed.</span></span> <span data-ttu-id="6f0ea-696">时的初始值设定项`a`运行时，值`b`为零，因此`a`初始化为`1`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-696">When the initializer for `a` runs, the value of `b` is zero, and so `a` is initialized to `1`.</span></span> <span data-ttu-id="6f0ea-697">时的初始值设定项`b`运行时，值`a`已经`1`，，因此`b`初始化为`2`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-697">When the initializer for `b` runs, the value of `a` is already `1`, and so `b` is initialized to `2`.</span></span>

#### <a name="static-field-initialization"></a><span data-ttu-id="6f0ea-698">静态字段初始化</span><span class="sxs-lookup"><span data-stu-id="6f0ea-698">Static field initialization</span></span>

<span data-ttu-id="6f0ea-699">静态字段变量初始值设定项类的对应于一系列类声明中显示的文本顺序执行的分配。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-699">The static field variable initializers of a class correspond to a sequence of assignments that are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="6f0ea-700">如果静态构造函数 ([静态构造函数](classes.md#static-constructors)) 存在在类中，执行静态字段初始值设定项在执行该静态构造函数之前立即发生。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-700">If a static constructor ([Static constructors](classes.md#static-constructors)) exists in the class, execution of the static field initializers occurs immediately prior to executing that static constructor.</span></span> <span data-ttu-id="6f0ea-701">否则，在首次使用该类的静态字段之前实现相关时所执行的静态字段初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-701">Otherwise, the static field initializers are executed at an implementation-dependent time prior to the first use of a static field of that class.</span></span> <span data-ttu-id="6f0ea-702">该示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-702">The example</span></span>
```csharp
using System;

class Test 
{ 
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    public static int X = Test.F("Init A");
}

class B
{
    public static int Y = Test.F("Init B");
}
```
<span data-ttu-id="6f0ea-703">可能会产生下列输出：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-703">might produce either the output:</span></span>
```
Init A
Init B
1 1
```
<span data-ttu-id="6f0ea-704">或输出：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-704">or the output:</span></span>
```
Init B
Init A
1 1
```
<span data-ttu-id="6f0ea-705">因为在执行`X`的初始值设定项和`Y`的初始值设定项可能会发生上述两种顺序; 它们仅限制发生后才对这些字段的引用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-705">because the execution of `X`'s initializer and `Y`'s initializer could occur in either order; they are only constrained to occur before the references to those fields.</span></span> <span data-ttu-id="6f0ea-706">但是，在下面的示例：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-706">However, in the example:</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    static A() {}

    public static int X = Test.F("Init A");
}

class B
{
    static B() {}

    public static int Y = Test.F("Init B");
}
```
<span data-ttu-id="6f0ea-707">必须是输出：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-707">the output must be:</span></span>
```
Init B
Init A
1 1
```
<span data-ttu-id="6f0ea-708">因为当静态构造函数执行的规则 (中定义[静态构造函数](classes.md#static-constructors)) 提供该`B`的静态构造函数 (并因此`B`的静态字段初始值设定项)之前，必须运行`A`的静态构造函数和字段初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-708">because the rules for when static constructors execute (as defined in [Static constructors](classes.md#static-constructors)) provide that `B`'s static constructor (and hence `B`'s static field initializers) must run before `A`'s static constructor and field initializers.</span></span>

#### <a name="instance-field-initialization"></a><span data-ttu-id="6f0ea-709">实例字段的初始化</span><span class="sxs-lookup"><span data-stu-id="6f0ea-709">Instance field initialization</span></span>

<span data-ttu-id="6f0ea-710">一个类的实例字段变量初始值设定项对应于分配到任何一个实例构造函数在输入时立即执行的一系列 ([构造函数初始值设定项](classes.md#constructor-initializers)) 的类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-710">The instance field variable initializers of a class correspond to a sequence of assignments that are executed immediately upon entry to any one of the instance constructors ([Constructor initializers](classes.md#constructor-initializers)) of that class.</span></span> <span data-ttu-id="6f0ea-711">变量的初始值设定项类声明中显示的文本顺序执行。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-711">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="6f0ea-712">类实例创建和初始化过程进行了进一步说明了[实例构造函数](classes.md#instance-constructors)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-712">The class instance creation and initialization process is described further in [Instance constructors](classes.md#instance-constructors).</span></span>

<span data-ttu-id="6f0ea-713">变量的初始值设定项，对于实例字段不能引用要创建的实例。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-713">A variable initializer for an instance field cannot reference the instance being created.</span></span> <span data-ttu-id="6f0ea-714">因此，它是一个编译时错误，以引用`this`中变量的初始值设定项，因为它是变量的初始值设定项来通过引用任何实例成员的编译时错误*simple_name*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-714">Thus, it is a compile-time error to reference `this` in a variable initializer, as it is a compile-time error for a variable initializer to reference any instance member through a *simple_name*.</span></span> <span data-ttu-id="6f0ea-715">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-715">In the example</span></span>
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
<span data-ttu-id="6f0ea-716">有关变量的初始值设定项`y`会导致编译时错误，因为它引用了要创建的实例的成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-716">the variable initializer for `y` results in a compile-time error because it references a member of the instance being created.</span></span>

## <a name="methods"></a><span data-ttu-id="6f0ea-717">方法</span><span class="sxs-lookup"><span data-stu-id="6f0ea-717">Methods</span></span>

<span data-ttu-id="6f0ea-718">***方法***是实现对象或类可执行的计算或操作的成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-718">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="6f0ea-719">方法使用声明*method_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="6f0ea-719">Methods are declared using *method_declaration*s:</span></span>

```antlr
method_declaration
    : method_header method_body
    ;

method_header
    : attributes? method_modifier* 'partial'? return_type member_name type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause*
    ;

method_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | 'async'
    | method_modifier_unsafe
    ;

return_type
    : type
    | 'void'
    ;

member_name
    : identifier
    | interface_type '.' identifier
    ;

method_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

<span data-ttu-id="6f0ea-720">一个*method_declaration*可能包括一套*特性*([特性](attributes.md)) 和四种访问修饰符的有效组合 ([访问修饰符](classes.md#access-modifiers))，则`new`([的新修饰符](classes.md#the-new-modifier))， `static` ([静态和实例方法](classes.md#static-and-instance-methods))， `virtual` ([虚拟方法](classes.md#virtual-methods))， `override` ([重写方法](classes.md#override-methods))， `sealed` ([密封方法](classes.md#sealed-methods))， `abstract` ([抽象方法](classes.md#abstract-methods))，并`extern` ([外部方法](classes.md#external-methods)) 修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-720">A *method_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="6f0ea-721">如果满足以下所有的声明就具有修饰符的有效组合：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-721">A declaration has a valid combination of modifiers if all of the following are true:</span></span>

*  <span data-ttu-id="6f0ea-722">声明包含一个有效的访问修饰符组合 ([访问修饰符](classes.md#access-modifiers))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-722">The declaration includes a valid combination of access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span>
*  <span data-ttu-id="6f0ea-723">声明不包括相同的修饰符多次。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-723">The declaration does not include the same modifier multiple times.</span></span>
*  <span data-ttu-id="6f0ea-724">声明包含最多一个的下列修饰符： `static`， `virtual`，和`override`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-724">The declaration includes at most one of the following modifiers: `static`, `virtual`, and `override`.</span></span>
*  <span data-ttu-id="6f0ea-725">声明包含最多一个的下列修饰符：`new`和`override`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-725">The declaration includes at most one of the following modifiers: `new` and `override`.</span></span>
*  <span data-ttu-id="6f0ea-726">如果声明包含`abstract`修饰符，然后声明不包含任何下列修饰符： `static`， `virtual`，`sealed`或`extern`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-726">If the declaration includes the `abstract` modifier, then the declaration does not include any of the following modifiers: `static`, `virtual`, `sealed` or `extern`.</span></span>
*  <span data-ttu-id="6f0ea-727">如果声明包含`private`修饰符，然后声明不包含任何下列修饰符： `virtual`， `override`，或`abstract`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-727">If the declaration includes the `private` modifier, then the declaration does not include any of the following modifiers: `virtual`, `override`, or `abstract`.</span></span>
*  <span data-ttu-id="6f0ea-728">如果声明包含`sealed`修饰符，然后声明还包括`override`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-728">If the declaration includes the `sealed` modifier, then the declaration also includes the `override` modifier.</span></span>
*  <span data-ttu-id="6f0ea-729">如果声明包含`partial`修饰符，则它不包含任何下列修饰符： `new`， `public`， `protected`， `internal`， `private`， `virtual`， `sealed`， `override``abstract`，或`extern`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-729">If the declaration includes the `partial` modifier, then it does not include any of the following modifiers: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`, or `extern`.</span></span>

<span data-ttu-id="6f0ea-730">一种具有方法`async`修饰符是异步函数，并遵循中所述的规则[异步函数](classes.md#async-functions)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-730">A method that has the `async` modifier is an async function and follows the rules described in [Async functions](classes.md#async-functions).</span></span>

<span data-ttu-id="6f0ea-731">*Return_type*方法的声明指定的计算和方法返回的值的类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-731">The *return_type* of a method declaration specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="6f0ea-732">*Return_type*是`void`如果方法不返回值。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-732">The *return_type* is `void` if the method does not return a value.</span></span> <span data-ttu-id="6f0ea-733">如果声明包含`partial`修饰符，则返回类型必须可`void`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-733">If the declaration includes the `partial` modifier, then the return type must be `void`.</span></span>

<span data-ttu-id="6f0ea-734">*Member_name*指定方法的名称。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-734">The *member_name* specifies the name of the method.</span></span> <span data-ttu-id="6f0ea-735">除非该方法是显式接口成员实现 ([显式接口成员实现代码](interfaces.md#explicit-interface-member-implementations))，则*member_name*是只需*标识符*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-735">Unless the method is an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="6f0ea-736">有关显式接口成员实现， *member_name*组成*interface_type*跟"`.`"和一个*标识符*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-736">For an explicit interface member implementation, the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="6f0ea-737">可选*type_parameter_list*指定的类型参数的方法 ([类型参数](classes.md#type-parameters))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-737">The optional *type_parameter_list* specifies the type parameters of the method ([Type parameters](classes.md#type-parameters)).</span></span> <span data-ttu-id="6f0ea-738">如果*type_parameter_list*的方法指定***泛型方法***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-738">If a *type_parameter_list* is specified the method is a ***generic method***.</span></span> <span data-ttu-id="6f0ea-739">如果该方法具有`extern`修饰符， *type_parameter_list*不能指定。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-739">If the method has an `extern` modifier, a *type_parameter_list* cannot be specified.</span></span>

<span data-ttu-id="6f0ea-740">可选*formal_parameter_list*指定的参数的方法 ([方法参数](classes.md#method-parameters))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-740">The optional *formal_parameter_list* specifies the parameters of the method ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="6f0ea-741">可选*type_parameter_constraints_clause*指定单个类型参数的约束 ([类型参数约束](classes.md#type-parameter-constraints))，并且如果可能仅指定*type_parameter_列表*还提供，并且该方法没有`override`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-741">The optional *type_parameter_constraints_clause*s specify constraints on individual type parameters ([Type parameter constraints](classes.md#type-parameter-constraints)) and may only be specified if a *type_parameter_list* is also supplied, and the method does not have an `override` modifier.</span></span>

<span data-ttu-id="6f0ea-742">*Return_type*和每种类型中引用*formal_parameter_list*的一种方法必须是至少与该方法本身相同的可访问性 ([可访问性约束](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="6f0ea-742">The *return_type* and each of the types referenced in the *formal_parameter_list* of a method must be at least as accessible as the method itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6f0ea-743">*Method_body*是一个分号***语句体***或***表达式主体***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-743">The *method_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="6f0ea-744">语句体组成*块*，它指定要调用的方法时执行的语句。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-744">A statement body consists of a *block*, which specifies the statements to execute when the method is invoked.</span></span> <span data-ttu-id="6f0ea-745">表达式主体组成`=>`跟*表达式*和一个分号，表示单个表达式执行时调用的方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-745">An expression body consists of `=>` followed by an *expression* and a semicolon, and denotes a single expression to perform when the method is invoked.</span></span> 

<span data-ttu-id="6f0ea-746">有关`abstract`并`extern`方法， *method_body*只需包含一个分号。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-746">For `abstract` and `extern` methods, the *method_body* consists simply of a semicolon.</span></span> <span data-ttu-id="6f0ea-747">有关`partial`方法*method_body*可能包含分号、 块正文或正文为表达式。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-747">For `partial` methods the *method_body* may consist of either a semicolon, a block body or an expression body.</span></span> <span data-ttu-id="6f0ea-748">对于所有其他方法， *method_body*块正文或正文为表达式。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-748">For all other methods, the *method_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="6f0ea-749">如果*method_body*包含一个分号，则可能不包括在声明`async`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-749">If the *method_body* consists of a semicolon, then the declaration may not include the `async` modifier.</span></span>

<span data-ttu-id="6f0ea-750">名称、 类型参数列表和形参列表的方法定义的签名 ([签名和超载](basic-concepts.md#signatures-and-overloading)) 的方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-750">The name, the type parameter list and the formal parameter list of a method define the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the method.</span></span> <span data-ttu-id="6f0ea-751">具体而言，一种方法的签名由其名称、 类型参数的数量、 修饰符和其形参类型的数组成。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-751">Specifically, the signature of a method consists of its name, the number of type parameters and the number, modifiers, and types of its formal parameters.</span></span> <span data-ttu-id="6f0ea-752">出于这些目的，不是按其名称，但该方法的类型实参列表中其序号位置通过标识的方法的形参的类型中发生的任何类型参数。返回类型不是方法的签名的一部分，也不是类型形参或正式参数的名称。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-752">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.The return type is not part of a method's signature, nor are the names of the type parameters or the formal parameters.</span></span>

<span data-ttu-id="6f0ea-753">方法的名称必须不同于其他所有非-中声明的方法相同的类的名称。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-753">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="6f0ea-754">此外，一种方法的签名必须不同于同一类中声明的所有其他方法的签名并完全由不同的签名不能在同一个类中声明的两种方法`ref`和`out`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-754">In addition, the signature of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>

<span data-ttu-id="6f0ea-755">该方法的*type_parameter*s 处于范围内整个*method_declaration*，并可用于对整个该作用域中的窗体类型*return_type*， *method_body*，并*type_parameter_constraints_clause*s 但不能在*属性*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-755">The method's *type_parameter*s are in scope throughout the *method_declaration*, and can be used to form types throughout that scope in *return_type*, *method_body*, and *type_parameter_constraints_clause*s but not in *attributes*.</span></span>

<span data-ttu-id="6f0ea-756">所有形参和类型参数必须都具有不同的名称。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-756">All formal parameters and type parameters must have different names.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="6f0ea-757">方法参数</span><span class="sxs-lookup"><span data-stu-id="6f0ea-757">Method parameters</span></span>

<span data-ttu-id="6f0ea-758">参数的方法，如果有，声明的方法的*formal_parameter_list*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-758">The parameters of a method, if any, are declared by the method's *formal_parameter_list*.</span></span>

```antlr
formal_parameter_list
    : fixed_parameters
    | fixed_parameters ',' parameter_array
    | parameter_array
    ;

fixed_parameters
    : fixed_parameter (',' fixed_parameter)*
    ;

fixed_parameter
    : attributes? parameter_modifier? type identifier default_argument?
    ;

default_argument
    : '=' expression
    ;

parameter_modifier
    : 'ref'
    | 'out'
    | 'this'
    ;

parameter_array
    : attributes? 'params' array_type identifier
    ;
```

<span data-ttu-id="6f0ea-759">形参列表包含的一个或多个以逗号分隔的参数其中只有最后一个可能*parameter_array*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-759">The formal parameter list consists of one or more comma-separated parameters of which only the last may be a *parameter_array*.</span></span>

<span data-ttu-id="6f0ea-760">一个*fixed_parameter*包含一组可选*特性*([属性](attributes.md))，一个可选`ref`，`out`或`this`修饰符，*类型*，则*标识符*和可选*default_argument*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-760">A *fixed_parameter* consists of an optional set of *attributes* ([Attributes](attributes.md)), an optional `ref`, `out` or `this` modifier, a *type*, an *identifier* and an optional *default_argument*.</span></span> <span data-ttu-id="6f0ea-761">每个*fixed_parameter*声明了具有给定名称的给定类型的参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-761">Each *fixed_parameter* declares a parameter of the given type with the given name.</span></span> <span data-ttu-id="6f0ea-762">`this`修饰符将方法指定为扩展方法，并只允许一个静态方法的第一个参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-762">The `this` modifier designates the method as an extension method and is only allowed on the first parameter of a static method.</span></span> <span data-ttu-id="6f0ea-763">中进一步介绍扩展方法[扩展方法](classes.md#extension-methods)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-763">Extension methods are further described in [Extension methods](classes.md#extension-methods).</span></span>

<span data-ttu-id="6f0ea-764">一个*fixed_parameter*与*default_argument*称为***可选参数***，而*fixed_parameter*而无需*default_argument*是***所需的参数***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-764">A *fixed_parameter* with a *default_argument* is known as an ***optional parameter***, whereas a *fixed_parameter* without a *default_argument* is a ***required parameter***.</span></span> <span data-ttu-id="6f0ea-765">所需的参数可能不会显示在中的可选参数后*formal_parameter_list*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-765">A required parameter may not appear after an optional parameter in a *formal_parameter_list*.</span></span>

<span data-ttu-id="6f0ea-766">一个`ref`或`out`参数不能具有*default_argument*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-766">A `ref` or `out` parameter cannot have a *default_argument*.</span></span> <span data-ttu-id="6f0ea-767">*表达式*中*default_argument*必须是以下值之一：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-767">The *expression* in a *default_argument* must be one of the following:</span></span>

*  <span data-ttu-id="6f0ea-768">*constant_expression*</span><span class="sxs-lookup"><span data-stu-id="6f0ea-768">a *constant_expression*</span></span>
*  <span data-ttu-id="6f0ea-769">形式的表达式`new S()`其中`S`是值类型</span><span class="sxs-lookup"><span data-stu-id="6f0ea-769">an expression of the form `new S()` where `S` is a value type</span></span>
*  <span data-ttu-id="6f0ea-770">形式的表达式`default(S)`其中`S`是值类型</span><span class="sxs-lookup"><span data-stu-id="6f0ea-770">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="6f0ea-771">*表达式*必须可由标识或可以为 null 转换为参数的类型隐式转换。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-771">The *expression* must be implicitly convertible by an identity or nullable conversion to the type of the parameter.</span></span>

<span data-ttu-id="6f0ea-772">如果在实现分部方法声明中出现的可选参数 ([分部方法](classes.md#partial-methods))，显式接口成员实现 ([显式接口成员实现代码](interfaces.md#explicit-interface-member-implementations)) 中或在单参数索引器声明 ([索引器](classes.md#indexers)) 编译器应发出警告，因为这些成员可以永远不会调用允许省略的参数的方式。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-772">If optional parameters occur in an implementing partial method declaration ([Partial methods](classes.md#partial-methods)) , an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)) or in a single-parameter indexer declaration ([Indexers](classes.md#indexers)) the compiler should give a warning, since these members can never be invoked in a way that permits arguments to be omitted.</span></span>

<span data-ttu-id="6f0ea-773">一个*parameter_array*包含一组可选*特性*([特性](attributes.md))，`params`修饰符， *array_type*，和一个*标识符*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-773">A *parameter_array* consists of an optional set of *attributes* ([Attributes](attributes.md)), a `params` modifier, an *array_type*, and an *identifier*.</span></span> <span data-ttu-id="6f0ea-774">参数数组声明了具有给定名称的给定的数组类型的单个参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-774">A parameter array declares a single parameter of the given array type with the given name.</span></span> <span data-ttu-id="6f0ea-775">*Array_type*的参数数组必须是一个一维数组类型 ([数组类型](arrays.md#array-types))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-775">The *array_type* of a parameter array must be a single-dimensional array type ([Array types](arrays.md#array-types)).</span></span> <span data-ttu-id="6f0ea-776">在方法调用中，参数数组允许任一单个参数的给定的数组类型来指定，或要指定的数组元素类型的零个或多个参数，它允许。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-776">In a method invocation, a parameter array permits either a single argument of the given array type to be specified, or it permits zero or more arguments of the array element type to be specified.</span></span> <span data-ttu-id="6f0ea-777">参数数组中进行了描述进一步[参数数组](classes.md#parameter-arrays)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-777">Parameter arrays are described further in [Parameter arrays](classes.md#parameter-arrays).</span></span>

<span data-ttu-id="6f0ea-778">一个*parameter_array*可能是后一个可选参数，但不能有默认值--参数省略*parameter_array*改为将导致创建一个空数组。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-778">A *parameter_array* may occur after an optional parameter, but cannot have a default value -- the omission of arguments for a *parameter_array* would instead result in the creation of an empty array.</span></span>

<span data-ttu-id="6f0ea-779">下面的示例说明了不同类型的参数：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-779">The following example illustrates different kinds of parameters:</span></span>
```csharp
public void M(
    ref int      i,
    decimal      d,
    bool         b = false,
    bool?        n = false,
    string       s = "Hello",
    object       o = null,
    T            t = default(T),
    params int[] a
) { }
```

<span data-ttu-id="6f0ea-780">中*formal_parameter_list*有关`M`，`i`是必需的 ref 参数，而`d`是必需的值参数，而`b`， `s`，`o`和`t`可选值的参数和`a`是参数数组。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-780">In the *formal_parameter_list* for `M`, `i` is a required ref parameter, `d` is a required value parameter, `b`, `s`, `o` and `t` are optional value parameters and `a` is a parameter array.</span></span>

<span data-ttu-id="6f0ea-781">方法声明为创建一个单独的声明空间参数、 类型参数和本地变量。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-781">A method declaration creates a separate declaration space for parameters, type parameters and local variables.</span></span> <span data-ttu-id="6f0ea-782">名称引入此声明空间通过类型参数列表和形参列表的方法以及局部变量声明中*块*的方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-782">Names are introduced into this declaration space by the type parameter list and the formal parameter list of the method and by local variable declarations in the *block* of the method.</span></span> <span data-ttu-id="6f0ea-783">它是错误的方法声明空间的两个成员具有相同的名称。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-783">It is an error for two members of a method declaration space to have the same name.</span></span> <span data-ttu-id="6f0ea-784">它是错误的方法声明空间和嵌套的声明空间以包含具有相同名称的元素的本地变量声明空间。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-784">It is an error for the method declaration space and the local variable declaration space of a nested declaration space to contain elements with the same name.</span></span>

<span data-ttu-id="6f0ea-785">方法调用 ([方法调用](expressions.md#method-invocations)) 创建特定于该调用的副本，形参和局部变量的方法和调用的参数列表的值或变量引用赋给新创建的正式参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-785">A method invocation ([Method invocations](expressions.md#method-invocations)) creates a copy, specific to that invocation, of the formal parameters and local variables of the method, and the argument list of the invocation assigns values or variable references to the newly created formal parameters.</span></span> <span data-ttu-id="6f0ea-786">内*块*的一种方法，可以通过在其标识符引用形参*simple_name*表达式 ([简单名称](expressions.md#simple-names))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-786">Within the *block* of a method, formal parameters can be referenced by their identifiers in *simple_name* expressions ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="6f0ea-787">有四种类型的正式参数：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-787">There are four kinds of formal parameters:</span></span>

*  <span data-ttu-id="6f0ea-788">值参数，而无需任何修饰符声明。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-788">Value parameters, which are declared without any modifiers.</span></span>
*  <span data-ttu-id="6f0ea-789">引用参数，使用声明`ref`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-789">Reference parameters, which are declared with the `ref` modifier.</span></span>
*  <span data-ttu-id="6f0ea-790">输出参数，使用声明`out`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-790">Output parameters, which are declared with the `out` modifier.</span></span>
*  <span data-ttu-id="6f0ea-791">参数的数组，使用声明`params`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-791">Parameter arrays, which are declared with the `params` modifier.</span></span>

<span data-ttu-id="6f0ea-792">如中所述[签名和超载](basic-concepts.md#signatures-and-overloading)，则`ref`并`out`修饰符是方法的签名的一部分，但`params`修饰符不是。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-792">As described in [Signatures and overloading](basic-concepts.md#signatures-and-overloading), the `ref` and `out` modifiers are part of a method's signature, but the `params` modifier is not.</span></span>

#### <a name="value-parameters"></a><span data-ttu-id="6f0ea-793">值参数</span><span class="sxs-lookup"><span data-stu-id="6f0ea-793">Value parameters</span></span>

<span data-ttu-id="6f0ea-794">用任何修饰符声明的参数是值参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-794">A parameter declared with no modifiers is a value parameter.</span></span> <span data-ttu-id="6f0ea-795">值参数对应于从方法调用中提供的相应参数中获取其初始值的局部变量。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-795">A value parameter corresponds to a local variable that gets its initial value from the corresponding argument supplied in the method invocation.</span></span>

<span data-ttu-id="6f0ea-796">方法调用中的相应参数的值参数的形参时，必须是隐式转换的表达式 ([隐式转换](conversions.md#implicit-conversions)) 到形参类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-796">When a formal parameter is a value parameter, the corresponding argument in a method invocation must be an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the formal parameter type.</span></span>

<span data-ttu-id="6f0ea-797">一种方法被允许将新值分配给 value 参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-797">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="6f0ea-798">此类分配只会影响由值参数表示的本地存储位置 — 它们不会影响在方法调用中给出的实参。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-798">Such assignments only affect the local storage location represented by the value parameter—they have no effect on the actual argument given in the method invocation.</span></span>

#### <a name="reference-parameters"></a><span data-ttu-id="6f0ea-799">引用参数</span><span class="sxs-lookup"><span data-stu-id="6f0ea-799">Reference parameters</span></span>

<span data-ttu-id="6f0ea-800">与声明的参数`ref`修饰符是一个引用参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-800">A parameter declared with a `ref` modifier is a reference parameter.</span></span> <span data-ttu-id="6f0ea-801">值参数与引用参数不会创建新的存储位置。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-801">Unlike a value parameter, a reference parameter does not create a new storage location.</span></span> <span data-ttu-id="6f0ea-802">相反，引用参数表示相同的存储位置与作为自变量在方法调用中的变量。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-802">Instead, a reference parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="6f0ea-803">在方法调用中的相应参数的形参时引用参数，必须包含关键字`ref`跟*variable_reference* ([精确规则，用于确定明确赋值](variables.md#precise-rules-for-determining-definite-assignment)) 的形式参数的类型相同。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-803">When a formal parameter is a reference parameter, the corresponding argument in a method invocation must consist of the keyword `ref` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="6f0ea-804">它可以作为引用参数传递之前，必须明确赋值变量。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-804">A variable must be definitely assigned before it can be passed as a reference parameter.</span></span>

<span data-ttu-id="6f0ea-805">在方法中，引用参数始终被视为明确赋值。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-805">Within a method, a reference parameter is always considered definitely assigned.</span></span>

<span data-ttu-id="6f0ea-806">一个方法声明为一个迭代器 ([迭代器](classes.md#iterators)) 不能具有引用参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-806">A method declared as an iterator ([Iterators](classes.md#iterators)) cannot have reference parameters.</span></span>

<span data-ttu-id="6f0ea-807">该示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-807">The example</span></span>
```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("i = {0}, j = {1}", i, j);
    }
}
```
<span data-ttu-id="6f0ea-808">生成输出</span><span class="sxs-lookup"><span data-stu-id="6f0ea-808">produces the output</span></span>
```
i = 2, j = 1
```

<span data-ttu-id="6f0ea-809">调用`Swap`中`Main`，`x`表示`i`并`y`表示`j`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-809">For the invocation of `Swap` in `Main`, `x` represents `i` and `y` represents `j`.</span></span> <span data-ttu-id="6f0ea-810">因此，调用具有交换的值的效果`i`和`j`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-810">Thus, the invocation has the effect of swapping the values of `i` and `j`.</span></span>

<span data-ttu-id="6f0ea-811">在采用的方法，可以为多个名称来表示相同的存储位置的引用参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-811">In a method that takes reference parameters it is possible for multiple names to represent the same storage location.</span></span> <span data-ttu-id="6f0ea-812">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-812">In the example</span></span>
```csharp
class A
{
    string s;

    void F(ref string a, ref string b) {
        s = "One";
        a = "Two";
        b = "Three";
    }

    void G() {
        F(ref s, ref s);
    }
}
```
<span data-ttu-id="6f0ea-813">在调用`F`中`G`将传递到引用`s`两个`a`和`b`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-813">the invocation of `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="6f0ea-814">因此，对于该调用，名称`s`， `a`，并`b`所有引用相同的存储位置，并且所有三个赋值修改实例字段`s`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-814">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance field `s`.</span></span>

#### <a name="output-parameters"></a><span data-ttu-id="6f0ea-815">输出参数</span><span class="sxs-lookup"><span data-stu-id="6f0ea-815">Output parameters</span></span>

<span data-ttu-id="6f0ea-816">与声明的参数`out`修饰符是一个 output 参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-816">A parameter declared with an `out` modifier is an output parameter.</span></span> <span data-ttu-id="6f0ea-817">与引用参数类似，输出参数不会创建新的存储位置。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-817">Similar to a reference parameter, an output parameter does not create a new storage location.</span></span> <span data-ttu-id="6f0ea-818">而是输出参数表示相同的存储位置与作为自变量在方法调用中的变量。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-818">Instead, an output parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="6f0ea-819">输出参数的形参时，在方法调用中的相应参数必须包含关键字`out`跟*variable_reference* ([精确规则，用于确定明确赋值](variables.md#precise-rules-for-determining-definite-assignment)) 的形式参数的类型相同。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-819">When a formal parameter is an output parameter, the corresponding argument in a method invocation must consist of the keyword `out` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="6f0ea-820">变量在之前它可以传递作为输出参数，但以下其中一个变量作为输出参数传递的调用，该变量明确赋值被视为不需要明确赋值。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-820">A variable need not be definitely assigned before it can be passed as an output parameter, but following an invocation where a variable was passed as an output parameter, the variable is considered definitely assigned.</span></span>

<span data-ttu-id="6f0ea-821">在方法中，就像本地变量时，输出参数最初被认为是未分配和使用它的值之前必须明确赋值。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-821">Within a method, just like a local variable, an output parameter is initially considered unassigned and must be definitely assigned before its value is used.</span></span>

<span data-ttu-id="6f0ea-822">该方法返回之前，必须明确赋值方法的每个输出参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-822">Every output parameter of a method must be definitely assigned before the method returns.</span></span>

<span data-ttu-id="6f0ea-823">为分部方法声明的方法 ([分部方法](classes.md#partial-methods)) 或迭代器 ([迭代器](classes.md#iterators)) 不能具有输出参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-823">A method declared as a partial method ([Partial methods](classes.md#partial-methods)) or an iterator ([Iterators](classes.md#iterators)) cannot have output parameters.</span></span>

<span data-ttu-id="6f0ea-824">生成多个返回值的方法中通常使用输出参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-824">Output parameters are typically used in methods that produce multiple return values.</span></span> <span data-ttu-id="6f0ea-825">例如：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-825">For example:</span></span>
```csharp
using System;

class Test
{
    static void SplitPath(string path, out string dir, out string name) {
        int i = path.Length;
        while (i > 0) {
            char ch = path[i - 1];
            if (ch == '\\' || ch == '/' || ch == ':') break;
            i--;
        }
        dir = path.Substring(0, i);
        name = path.Substring(i);
    }

    static void Main() {
        string dir, name;
        SplitPath("c:\\Windows\\System\\hello.txt", out dir, out name);
        Console.WriteLine(dir);
        Console.WriteLine(name);
    }
}
```

<span data-ttu-id="6f0ea-826">该示例生成输出：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-826">The example produces the output:</span></span>
```
c:\Windows\System\
hello.txt
```

<span data-ttu-id="6f0ea-827">请注意，`dir`并`name`传递到之前的变量可以是未分配`SplitPath`，它们被看作明确赋值调用之后。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-827">Note that the `dir` and `name` variables can be unassigned before they are passed to `SplitPath`, and that they are considered definitely assigned following the call.</span></span>

#### <a name="parameter-arrays"></a><span data-ttu-id="6f0ea-828">参数数组</span><span class="sxs-lookup"><span data-stu-id="6f0ea-828">Parameter arrays</span></span>

<span data-ttu-id="6f0ea-829">与声明的参数`params`修饰符是参数数组。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-829">A parameter declared with a `params` modifier is a parameter array.</span></span> <span data-ttu-id="6f0ea-830">如果形参列表中包含的参数数组，它必须在列表中的最后一个参数，并且它必须是一维数组类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-830">If a formal parameter list includes a parameter array, it must be the last parameter in the list and it must be of a single-dimensional array type.</span></span> <span data-ttu-id="6f0ea-831">例如，类型`string[]`并`string[][]`可以用作参数数组的类型，但类型`string[,]`不可以。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-831">For example, the types `string[]` and `string[][]` can be used as the type of a parameter array, but the type `string[,]` can not.</span></span> <span data-ttu-id="6f0ea-832">不能合并`params`修饰符和修饰符`ref`和`out`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-832">It is not possible to combine the `params` modifier with the modifiers `ref` and `out`.</span></span>

<span data-ttu-id="6f0ea-833">参数数组允许在方法调用中的两种方式之一中指定的参数：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-833">A parameter array permits arguments to be specified in one of two ways in a method invocation:</span></span>

*  <span data-ttu-id="6f0ea-834">给定的参数数组可以是一个隐式转换的表达式的参数 ([隐式转换](conversions.md#implicit-conversions)) 为参数数组类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-834">The argument given for a parameter array can be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="6f0ea-835">在这种情况下，参数数组的作用与值参数完全一样。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-835">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="6f0ea-836">或者，此调用可以指定为参数数组，其中每个参数都是隐式转换的表达式的零个或多个参数 ([隐式转换](conversions.md#implicit-conversions)) 为参数数组的元素类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-836">Alternatively, the invocation can specify zero or more arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="6f0ea-837">在这种情况下，调用创建具有对应的参数的数目的长度参数数组类型的实例、 初始化具有给定的参数值的数组实例的元素并将新创建的数组实例用作实际自变量。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-837">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="6f0ea-838">除了允许数目可变的参数调用中，参数数组是恰好等同于值参数 ([值参数](classes.md#value-parameters)) 的相同的类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-838">Except for allowing a variable number of arguments in an invocation, a parameter array is precisely equivalent to a value parameter ([Value parameters](classes.md#value-parameters)) of the same type.</span></span>

<span data-ttu-id="6f0ea-839">该示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-839">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params int[] args) {
        Console.Write("Array contains {0} elements:", args.Length);
        foreach (int i in args) 
            Console.Write(" {0}", i);
        Console.WriteLine();
    }

    static void Main() {
        int[] arr = {1, 2, 3};
        F(arr);
        F(10, 20, 30, 40);
        F();
    }
}
```
<span data-ttu-id="6f0ea-840">生成输出</span><span class="sxs-lookup"><span data-stu-id="6f0ea-840">produces the output</span></span>
```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="6f0ea-841">第一个调用`F`只需将该数组传递`a`作为值参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-841">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="6f0ea-842">第二次调用`F`会自动创建四个元素`int[]`与给定的元素的值并将该数组实例作为值参数传递。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-842">The second invocation of `F` automatically creates a four-element `int[]` with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="6f0ea-843">同样，第三个调用的`F`创建一个零元素`int[]`并将该实例作为值参数传递。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-843">Likewise, the third invocation of `F` creates a zero-element `int[]` and passes that instance as a value parameter.</span></span> <span data-ttu-id="6f0ea-844">第二个和第三个调用都完全等效于编写：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-844">The second and third invocations are precisely equivalent to writing:</span></span>
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

<span data-ttu-id="6f0ea-845">具有参数数组的方法执行重载决策时，可能在其正常形式或以其扩展形式适用 ([适用函数成员](expressions.md#applicable-function-member))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-845">When performing overload resolution, a method with a parameter array may be applicable either in its normal form or in its expanded form ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="6f0ea-846">仅当该方法的标准形式不适用并且仅当具有相同签名的扩展形式适用的方法未声明相同的类型中，展开的形式的一种方法才可用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-846">The expanded form of a method is available only if the normal form of the method is not applicable and only if an applicable method with the same signature as the expanded form is not already declared in the same type.</span></span>

<span data-ttu-id="6f0ea-847">该示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-847">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params object[] a) {
        Console.WriteLine("F(object[])");
    }

    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object a0, object a1) {
        Console.WriteLine("F(object,object)");
    }

    static void Main() {
        F();
        F(1);
        F(1, 2);
        F(1, 2, 3);
        F(1, 2, 3, 4);
    }
}
```
<span data-ttu-id="6f0ea-848">生成输出</span><span class="sxs-lookup"><span data-stu-id="6f0ea-848">produces the output</span></span>
```
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

<span data-ttu-id="6f0ea-849">在示例中，两个可能的扩展方法使用参数数组形式的已包含在类中作为常规方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-849">In the example, two of the possible expanded forms of the method with a parameter array are already included in the class as regular methods.</span></span> <span data-ttu-id="6f0ea-850">因此执行重载决策时不考虑这些扩展的形式和第一个和第三个方法调用将因此选择常规方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-850">These expanded forms are therefore not considered when performing overload resolution, and the first and third method invocations thus select the regular methods.</span></span> <span data-ttu-id="6f0ea-851">当类声明具有参数数组的方法时，它不少见还包含一些扩展形式作为常规方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-851">When a class declares a method with a parameter array, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="6f0ea-852">通过这样做可以避免的数组分配将调用具有参数数组的方法的扩展形式出现的实例。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-852">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a parameter array is invoked.</span></span>

<span data-ttu-id="6f0ea-853">当参数数组的类型是`object[]`，该方法的标准形式和单个的扩展的形式之间产生潜在的多义性`object`参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-853">When the type of a parameter array is `object[]`, a potential ambiguity arises between the normal form of the method and the expended form for a single `object` parameter.</span></span> <span data-ttu-id="6f0ea-854">不明确的原因在于`object[]`本身就是隐式转换为键入`object`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-854">The reason for the ambiguity is that an `object[]` is itself implicitly convertible to type `object`.</span></span> <span data-ttu-id="6f0ea-855">二义性造成任何问题，但是，因为它可通过插入一个强制转换，如果需要解决。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-855">The ambiguity presents no problem, however, since it can be resolved by inserting a cast if needed.</span></span>

<span data-ttu-id="6f0ea-856">该示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-856">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params object[] args) {
        foreach (object o in args) {
            Console.Write(o.GetType().FullName);
            Console.Write(" ");
        }
        Console.WriteLine();
    }

    static void Main() {
        object[] a = {1, "Hello", 123.456};
        object o = a;
        F(a);
        F((object)a);
        F(o);
        F((object[])o);
    }
}
```
<span data-ttu-id="6f0ea-857">生成输出</span><span class="sxs-lookup"><span data-stu-id="6f0ea-857">produces the output</span></span>
```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="6f0ea-858">中的第一个和最后一个调用`F`的范式`F`是适用的因为存在从实参类型到形参类型隐式转换 (两个均为类型`object[]`)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-858">In the first and last invocations of `F`, the normal form of `F` is applicable because an implicit conversion exists from the argument type to the parameter type (both are of type `object[]`).</span></span> <span data-ttu-id="6f0ea-859">因此，重载决策选择的标准形式`F`，并作为常规的值参数传递自变量。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-859">Thus, overload resolution selects the normal form of `F`, and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="6f0ea-860">在第二个和第三个调用的范式`F`因为隐式转换存在从实参类型到形参类型不适用 (类型`object`不能隐式转换为类型`object[]`)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-860">In the second and third invocations, the normal form of `F` is not applicable because no implicit conversion exists from the argument type to the parameter type (type `object` cannot be implicitly converted to type `object[]`).</span></span> <span data-ttu-id="6f0ea-861">但是的展开的形式`F`是适用，因此重载决策选择。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-861">However, the expanded form of `F` is applicable, so it is selected by overload resolution.</span></span> <span data-ttu-id="6f0ea-862">因此，一个元素`object[]`创建的调用，并使用给定的参数值初始化数组的单个元素 (其本身是对的引用`object[]`)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-862">As a result, a one-element `object[]` is created by the invocation, and the single element of the array is initialized with the given argument value (which itself is a reference to an `object[]`).</span></span>

### <a name="static-and-instance-methods"></a><span data-ttu-id="6f0ea-863">静态和实例方法</span><span class="sxs-lookup"><span data-stu-id="6f0ea-863">Static and instance methods</span></span>

<span data-ttu-id="6f0ea-864">当一个方法声明包括`static`修饰符，称方法为静态方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-864">When a method declaration includes a `static` modifier, that method is said to be a static method.</span></span> <span data-ttu-id="6f0ea-865">如果未`static`修饰符存在，则称该方法为实例方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-865">When no `static` modifier is present, the method is said to be an instance method.</span></span>

<span data-ttu-id="6f0ea-866">静态方法不对特定实例进行操作，它会导致编译时错误是指`this`中的静态方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-866">A static method does not operate on a specific instance, and it is a compile-time error to refer to `this` in a static method.</span></span>

<span data-ttu-id="6f0ea-867">实例方法作用于给定类的实例，并且该实例可作为访问`this`([此访问权限](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-867">An instance method operates on a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="6f0ea-868">当中引用的方法*member_access* ([成员访问](expressions.md#member-access)) 的窗体`E.M`，如果`M`是静态方法，`E`必须表示一个类型，包含`M`，并且如果`M`是实例方法，`E`必须表示一个类型，其中包含的一个实例`M`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-868">When a method is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static method, `E` must denote a type containing `M`, and if `M` is an instance method, `E` must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="6f0ea-869">静态之间的差异和实例成员进行讨论中进一步[静态和实例成员](classes.md#static-and-instance-members)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-869">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-methods"></a><span data-ttu-id="6f0ea-870">虚拟方法</span><span class="sxs-lookup"><span data-stu-id="6f0ea-870">Virtual methods</span></span>

<span data-ttu-id="6f0ea-871">如果实例方法声明包含`virtual`修饰符，称方法为虚方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-871">When an instance method declaration includes a `virtual` modifier, that method is said to be a virtual method.</span></span> <span data-ttu-id="6f0ea-872">如果未`virtual`修饰符存在，则称该方法为非虚拟方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-872">When no `virtual` modifier is present, the method is said to be a non-virtual method.</span></span>

<span data-ttu-id="6f0ea-873">非虚方法的实现是不变： 实现都是相同的方法调用中的类的实例上是否已声明的或派生类的实例。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-873">The implementation of a non-virtual method is invariant: The implementation is the same whether the method is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="6f0ea-874">与此相反，虚拟方法的实现可以由派生类所取代。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-874">In contrast, the implementation of a virtual method can be superseded by derived classes.</span></span> <span data-ttu-id="6f0ea-875">取代继承的虚方法的实现的过程被称为***重写***该方法 ([重写方法](classes.md#override-methods))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-875">The process of superseding the implementation of an inherited virtual method is known as ***overriding*** that method ([Override methods](classes.md#override-methods)).</span></span>

<span data-ttu-id="6f0ea-876">在虚拟方法调用中，***运行时类型***为其调用方法的实例的位置确定要调用的实际方法实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-876">In a virtual method invocation, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="6f0ea-877">在非虚拟方法调用中，***编译时类型***的实例是决定性因素。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-877">In a non-virtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span> <span data-ttu-id="6f0ea-878">准确地说，当名为一种方法中`N`使用的参数列表调用`A`编译时类型的实例上`C`和运行时类型`R`(其中`R`是`C`或派生的类从`C`)，其调用处理过程，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-878">In precise terms, when a method named `N` is invoked with an argument list `A` on an instance with a compile-time type `C` and a run-time type `R` (where `R` is either `C` or a class derived from `C`), the invocation is processed as follows:</span></span>

*  <span data-ttu-id="6f0ea-879">首先，重载决策应用于`C`， `N`，并`A`以选择特定方法`M`从一组的方法中声明并由继承`C`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-879">First, overload resolution is applied to `C`, `N`, and `A`, to select a specific method `M` from the set of methods declared in and inherited by `C`.</span></span> <span data-ttu-id="6f0ea-880">这中所述[方法调用](expressions.md#method-invocations)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-880">This is described in [Method invocations](expressions.md#method-invocations).</span></span>
*  <span data-ttu-id="6f0ea-881">然后，如果`M`是一个非虚拟方法，`M`调用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-881">Then, if `M` is a non-virtual method, `M` is invoked.</span></span>
*  <span data-ttu-id="6f0ea-882">否则为`M`是虚拟方法的派生程度最高的实现`M`与`R`调用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-882">Otherwise, `M` is a virtual method, and the most derived implementation of `M` with respect to `R` is invoked.</span></span>

<span data-ttu-id="6f0ea-883">对于每个虚拟方法中声明或继承的类，都存在***实现的派生程度最***与该类相关的方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-883">For every virtual method declared in or inherited by a class, there exists a ***most derived implementation*** of the method with respect to that class.</span></span> <span data-ttu-id="6f0ea-884">虚拟方法的派生程度最高的实现`M`相对于一个类`R`，如下所示确定：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-884">The most derived implementation of a virtual method `M` with respect to a class `R` is determined as follows:</span></span>

*  <span data-ttu-id="6f0ea-885">如果`R`包含引入`virtual`的声明`M`，则此操作的派生程度最高的实现`M`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-885">If `R` contains the introducing `virtual` declaration of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="6f0ea-886">否则为如果`R`包含`override`的`M`，则此操作的派生程度最高的实现`M`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-886">Otherwise, if `R` contains an `override` of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="6f0ea-887">否则，大多数派生的实现`M`相对于`R`的派生程度最高实现相同`M`方面的直接基类`R`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-887">Otherwise, the most derived implementation of `M` with respect to `R` is the same as the most derived implementation of `M` with respect to the direct base class of `R`.</span></span>

<span data-ttu-id="6f0ea-888">下面的示例说明了虚拟和非虚拟方法之间的差异：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-888">The following example illustrates the differences between virtual and non-virtual methods:</span></span>
```csharp
using System;

class A
{
    public void F() { Console.WriteLine("A.F"); }

    public virtual void G() { Console.WriteLine("A.G"); }
}

class B: A
{
    new public void F() { Console.WriteLine("B.F"); }

    public override void G() { Console.WriteLine("B.G"); }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        a.F();
        b.F();
        a.G();
        b.G();
    }
}
```

<span data-ttu-id="6f0ea-889">在示例中，`A`引入了非虚方法`F`和虚拟方法`G`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-889">In the example, `A` introduces a non-virtual method `F` and a virtual method `G`.</span></span> <span data-ttu-id="6f0ea-890">该类`B`引入了新的非虚拟方法`F`，从而隐藏继承`F`，并且还重写继承的方法`G`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-890">The class `B` introduces a new non-virtual method `F`, thus hiding the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="6f0ea-891">该示例生成输出：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-891">The example produces the output:</span></span>
```
A.F
B.F
B.G
B.G
```

<span data-ttu-id="6f0ea-892">请注意，该语句`a.G()`调用`B.G`，而不`A.G`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-892">Notice that the statement `a.G()` invokes `B.G`, not `A.G`.</span></span> <span data-ttu-id="6f0ea-893">这是因为运行时类型的实例 (即`B`)，不是实例的编译时类型 (即`A`)，确定要调用的实际方法实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-893">This is because the run-time type of the instance (which is `B`), not the compile-time type of the instance (which is `A`), determines the actual method implementation to invoke.</span></span>

<span data-ttu-id="6f0ea-894">方法允许隐藏继承的方法，因为很可能包含多个具有相同签名的虚拟方法的类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-894">Because methods are allowed to hide inherited methods, it is possible for a class to contain several virtual methods with the same signature.</span></span> <span data-ttu-id="6f0ea-895">这不会造成多义性问题，因为除了派生程度最高的方法的所有隐藏。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-895">This does not present an ambiguity problem, since all but the most derived method are hidden.</span></span> <span data-ttu-id="6f0ea-896">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-896">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() { Console.WriteLine("A.F"); }
}

class B: A
{
    public override void F() { Console.WriteLine("B.F"); }
}

class C: B
{
    new public virtual void F() { Console.WriteLine("C.F"); }
}

class D: C
{
    public override void F() { Console.WriteLine("D.F"); }
}

class Test
{
    static void Main() {
        D d = new D();
        A a = d;
        B b = d;
        C c = d;
        a.F();
        b.F();
        c.F();
        d.F();
    }
}
```
<span data-ttu-id="6f0ea-897">`C`和`D`类包含两个具有相同的签名的虚拟方法： 一个引入`A`引入的一个`C`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-897">the `C` and `D` classes contain two virtual methods with the same signature: The one introduced by `A` and the one introduced by `C`.</span></span> <span data-ttu-id="6f0ea-898">引入的方法`C`隐藏继承的方法`A`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-898">The method introduced by `C` hides the method inherited from `A`.</span></span> <span data-ttu-id="6f0ea-899">因此，重写中的声明`D`重写方法，通过引入`C`，并不适用于`D`若要重写方法由引入`A`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-899">Thus, the override declaration in `D` overrides the method introduced by `C`, and it is not possible for `D` to override the method introduced by `A`.</span></span> <span data-ttu-id="6f0ea-900">该示例生成输出：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-900">The example produces the output:</span></span>
```
B.F
B.F
D.F
D.F
```

<span data-ttu-id="6f0ea-901">请注意，它是可以通过访问的实例调用隐藏虚方法`D`小于通过派生的类型中的方法不会隐藏。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-901">Note that it is possible to invoke the hidden virtual method by accessing an instance of `D` through a less derived type in which the method is not hidden.</span></span>

### <a name="override-methods"></a><span data-ttu-id="6f0ea-902">重写方法</span><span class="sxs-lookup"><span data-stu-id="6f0ea-902">Override methods</span></span>

<span data-ttu-id="6f0ea-903">如果实例方法声明包含`override`修饰符，该方法称为***重写方法***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-903">When an instance method declaration includes an `override` modifier, the method is said to be an ***override method***.</span></span> <span data-ttu-id="6f0ea-904">重写方法重写继承的虚方法具有相同的签名。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-904">An override method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="6f0ea-905">但如果虚方法声明中引入新方法，重写方法声明通过提供相应方法的新实现代码，专门针对现有的继承虚方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-905">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="6f0ea-906">通过重写的方法`override`声明被称为***重写基方法***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-906">The method overridden by an `override` declaration is known as the ***overridden base method***.</span></span> <span data-ttu-id="6f0ea-907">用于重写方法`M`类中声明`C`，重写基方法通过检查确定的每个基类类型`C`开头的直接基类类型`C`并继续每个连续直接基类类型，直到至少一个可访问的方法是在给定的基类类型中位于其具有相同的签名`M`后替换的类型参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-907">For an override method `M` declared in a class `C`, the overridden base method is determined by examining each base class type of `C`, starting with the direct base class type of `C` and continuing with each successive direct base class type, until in a given base class type at least one accessible method is located which has the same signature as `M` after substitution of type arguments.</span></span> <span data-ttu-id="6f0ea-908">为了查找重写基方法，方法考虑它是否可访问`public`，如果它是`protected`，则`protected internal`，或者它是否`internal`作为在同一程序中声明和`C`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-908">For the purposes of locating the overridden base method, a method is considered accessible if it is `public`, if it is `protected`, if it is `protected internal`, or if it is `internal` and declared in the same program as `C`.</span></span>

<span data-ttu-id="6f0ea-909">除非以下条件都适用于重写声明，否则，将发生编译时错误：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-909">A compile-time error occurs unless all of the following are true for an override declaration:</span></span>

*  <span data-ttu-id="6f0ea-910">重写基方法可位于上文所述。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-910">An overridden base method can be located as described above.</span></span>
*  <span data-ttu-id="6f0ea-911">没有一个此类重写基方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-911">There is exactly one such overridden base method.</span></span> <span data-ttu-id="6f0ea-912">基类类型是构造的类型，其中类型参数替换使两个方法的签名相同，此限制就会起作用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-912">This restriction has effect only if the base class type is a constructed type where the substitution of type arguments makes the signature of two methods the same.</span></span>
*  <span data-ttu-id="6f0ea-913">重写基方法是虚拟的抽象，或重写方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-913">The overridden base method is a virtual, abstract, or override method.</span></span> <span data-ttu-id="6f0ea-914">换而言之，重写基方法不能为静态或非虚拟。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-914">In other words, the overridden base method cannot be static or non-virtual.</span></span>
*  <span data-ttu-id="6f0ea-915">重写基方法不是密封的方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-915">The overridden base method is not a sealed method.</span></span>
*  <span data-ttu-id="6f0ea-916">重写方法和重写基方法具有相同的返回类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-916">The override method and the overridden base method have the same return type.</span></span>
*  <span data-ttu-id="6f0ea-917">重写声明和重写基方法具有相同的声明可访问性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-917">The override declaration and the overridden base method have the same declared accessibility.</span></span> <span data-ttu-id="6f0ea-918">换而言之，重写声明不能更改虚拟方法的可访问性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-918">In other words, an override declaration cannot change the accessibility of the virtual method.</span></span> <span data-ttu-id="6f0ea-919">但是，如果重写基方法受保护的内部并不是声明为包含的重写方法，然后重写方法的程序集在不同的程序集中声明可访问性必须受到保护。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-919">However, if the overridden base method is protected internal and it is declared in a different assembly than the assembly containing the override method then the override method's declared accessibility must be protected.</span></span>
*  <span data-ttu-id="6f0ea-920">重写声明未指定类型参数约束子句。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-920">The override declaration does not specify type-parameter-constraints-clauses.</span></span> <span data-ttu-id="6f0ea-921">而是约束均继承于重写基方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-921">Instead the constraints are inherited from the overridden base method.</span></span> <span data-ttu-id="6f0ea-922">请注意，可能会通过继承约束中的类型参数替换中重写的方法的类型形参的约束。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-922">Note that constraints that are type parameters in the overridden method may be replaced by type arguments in the inherited constraint.</span></span> <span data-ttu-id="6f0ea-923">这可能会导致不是合法时显式指定，如值类型或密封的类型的约束。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-923">This can lead to constraints that are not legal when explicitly specified, such as value types or sealed types.</span></span>

<span data-ttu-id="6f0ea-924">下面的示例演示如何重写规则适用于泛型类：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-924">The following example demonstrates how the overriding rules work for generic classes:</span></span>
```csharp
abstract class C<T>
{
    public virtual T F() {...}
    public virtual C<T> G() {...}
    public virtual void H(C<T> x) {...}
}

class D: C<string>
{
    public override string F() {...}            // Ok
    public override C<string> G() {...}         // Ok
    public override void H(C<T> x) {...}        // Error, should be C<string>
}

class E<T,U>: C<U>
{
    public override U F() {...}                 // Ok
    public override C<U> G() {...}              // Ok
    public override void H(C<T> x) {...}        // Error, should be C<U>
}
```

<span data-ttu-id="6f0ea-925">重写声明可以重写基方法使用访问*base_access* ([基访问](expressions.md#base-access))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-925">An override declaration can access the overridden base method using a *base_access* ([Base access](expressions.md#base-access)).</span></span> <span data-ttu-id="6f0ea-926">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-926">In the example</span></span>
```csharp
class A
{
    int x;

    public virtual void PrintFields() {
        Console.WriteLine("x = {0}", x);
    }
}

class B: A
{
    int y;

    public override void PrintFields() {
        base.PrintFields();
        Console.WriteLine("y = {0}", y);
    }
}
```
<span data-ttu-id="6f0ea-927">`base.PrintFields()`中的调用`B`调用`PrintFields`方法中声明`A`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-927">the `base.PrintFields()` invocation in `B` invokes the `PrintFields` method declared in `A`.</span></span> <span data-ttu-id="6f0ea-928">一个*base_access*禁用虚调用机制，并只需将视为非虚方法的基方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-928">A *base_access* disables the virtual invocation mechanism and simply treats the base method as a non-virtual method.</span></span> <span data-ttu-id="6f0ea-929">中有调用`B`已写入`((A)this).PrintFields()`，它会以递归方式调用`PrintFields`方法中声明`B`中, 声明一个不`A`，因为`PrintFields`是虚拟运行时类型和`((A)this)`是`B`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-929">Had the invocation in `B` been written `((A)this).PrintFields()`, it would recursively invoke the `PrintFields` method declared in `B`, not the one declared in `A`, since `PrintFields` is virtual and the run-time type of `((A)this)` is `B`.</span></span>

<span data-ttu-id="6f0ea-930">仅通过包括`override`修饰符可以一种方法重写另一种方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-930">Only by including an `override` modifier can a method override another method.</span></span> <span data-ttu-id="6f0ea-931">在所有其他情况下，具有与继承的方法相同的签名的方法只是隐藏继承的方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-931">In all other cases, a method with the same signature as an inherited method simply hides the inherited method.</span></span> <span data-ttu-id="6f0ea-932">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-932">In the example</span></span>
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    public virtual void F() {}        // Warning, hiding inherited F()
}
```
<span data-ttu-id="6f0ea-933">`F`中的方法`B`不包括`override`修饰符，因此不会覆盖`F`中的方法`A`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-933">the `F` method in `B` does not include an `override` modifier and therefore does not override the `F` method in `A`.</span></span> <span data-ttu-id="6f0ea-934">而是`F`中的方法`B`中的方法将隐藏`A`，并将报告警告，因为在声明中不包含`new`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-934">Rather, the `F` method in `B` hides the method in `A`, and a warning is reported because the declaration does not include a `new` modifier.</span></span>

<span data-ttu-id="6f0ea-935">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-935">In the example</span></span>
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    new private void F() {}        // Hides A.F within body of B
}

class C: B
{
    public override void F() {}    // Ok, overrides A.F
}
```
<span data-ttu-id="6f0ea-936">`F`中的方法`B`隐藏虚拟`F`方法继承自`A`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-936">the `F` method in `B` hides the virtual `F` method inherited from `A`.</span></span> <span data-ttu-id="6f0ea-937">因为新`F`中`B`具有私有访问权限，其作用域仅包含的类的主体`B`并不会扩展到`C`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-937">Since the new `F` in `B` has private access, its scope only includes the class body of `B` and does not extend to `C`.</span></span> <span data-ttu-id="6f0ea-938">因此，声明`F`中`C`允许重写`F`继承自`A`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-938">Therefore, the declaration of `F` in `C` is permitted to override the `F` inherited from `A`.</span></span>

### <a name="sealed-methods"></a><span data-ttu-id="6f0ea-939">密封的方法</span><span class="sxs-lookup"><span data-stu-id="6f0ea-939">Sealed methods</span></span>

<span data-ttu-id="6f0ea-940">如果实例方法声明包含`sealed`修饰符、 说方法是可***密封方法***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-940">When an instance method declaration includes a `sealed` modifier, that method is said to be a ***sealed method***.</span></span> <span data-ttu-id="6f0ea-941">如果实例方法声明`sealed`修饰符，它还必须包括`override`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-941">If an instance method declaration includes the  `sealed` modifier, it must also include the `override` modifier.</span></span> <span data-ttu-id="6f0ea-942">使用`sealed`修饰符可防止派生的类进一步重写方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-942">Use of the `sealed` modifier prevents a derived class from further overriding the method.</span></span>

<span data-ttu-id="6f0ea-943">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-943">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }

    public virtual void G() {
        Console.WriteLine("A.G");
    }
}

class B: A
{
    sealed override public void F() {
        Console.WriteLine("B.F");
    } 

    override public void G() {
        Console.WriteLine("B.G");
    } 
}

class C: B
{
    override public void G() {
        Console.WriteLine("C.G");
    } 
}
```
<span data-ttu-id="6f0ea-944">类`B`提供了两个重写方法：`F`方法具有`sealed`修饰符和`G`不的方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-944">the class `B` provides two override methods: an `F` method that has the `sealed` modifier and a `G` method that does not.</span></span> <span data-ttu-id="6f0ea-945">`B`使用密封`modifier`可防止`C`进一步重写`F`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-945">`B`'s use of the sealed `modifier` prevents `C` from further overriding `F`.</span></span>

### <a name="abstract-methods"></a><span data-ttu-id="6f0ea-946">抽象方法</span><span class="sxs-lookup"><span data-stu-id="6f0ea-946">Abstract methods</span></span>

<span data-ttu-id="6f0ea-947">如果实例方法声明包含`abstract`修饰符、 说方法是可***抽象方法***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-947">When an instance method declaration includes an `abstract` modifier, that method is said to be an ***abstract method***.</span></span> <span data-ttu-id="6f0ea-948">尽管一个抽象方法隐式也是一个虚拟方法，但它不能有修饰符`virtual`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-948">Although an abstract method is implicitly also a virtual method, it cannot have the modifier `virtual`.</span></span>

<span data-ttu-id="6f0ea-949">抽象方法声明引入了新的虚拟方法，但不提供该方法的实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-949">An abstract method declaration introduces a new virtual method but does not provide an implementation of that method.</span></span> <span data-ttu-id="6f0ea-950">相反，非抽象派生的类所需重写该方法提供其自己的实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-950">Instead, non-abstract derived classes are required to provide their own implementation by overriding that method.</span></span> <span data-ttu-id="6f0ea-951">一个抽象方法不提供任何实际的实现，因为*method_body*抽象方法的只包含一个分号。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-951">Because an abstract method provides no actual implementation, the *method_body* of an abstract method simply consists of a semicolon.</span></span>

<span data-ttu-id="6f0ea-952">抽象类中才允许抽象方法声明 ([抽象类](classes.md#abstract-classes))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-952">Abstract method declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="6f0ea-953">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-953">In the example</span></span>
```csharp
public abstract class Shape
{
    public abstract void Paint(Graphics g, Rectangle r);
}

public class Ellipse: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawEllipse(r);
    }
}

public class Box: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawRect(r);
    }
}
```
<span data-ttu-id="6f0ea-954">`Shape`类定义本身就可以绘制一个几何形状对象的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-954">the `Shape` class defines the abstract notion of a geometrical shape object that can paint itself.</span></span> <span data-ttu-id="6f0ea-955">`Paint`方法是抽象的因为没有有意义的默认实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-955">The `Paint` method is abstract because there is no meaningful default implementation.</span></span> <span data-ttu-id="6f0ea-956">`Ellipse`并`Box`类是具体`Shape`实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-956">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="6f0ea-957">由于这些类非抽象的他们需要重写`Paint`方法，并提供实际实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-957">Because these classes are non-abstract, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="6f0ea-958">它是编译时错误*base_access* ([基访问](expressions.md#base-access)) 来引用一个抽象方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-958">It is a compile-time error for a *base_access* ([Base access](expressions.md#base-access)) to reference an abstract method.</span></span> <span data-ttu-id="6f0ea-959">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-959">In the example</span></span>
```csharp
abstract class A
{
    public abstract void F();
}

class B: A
{
    public override void F() {
        base.F();                        // Error, base.F is abstract
    }
}
```
<span data-ttu-id="6f0ea-960">编译时错误报告为`base.F()`调用因为它引用了一个抽象方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-960">a compile-time error is reported for the `base.F()` invocation because it references an abstract method.</span></span>

<span data-ttu-id="6f0ea-961">抽象方法声明允许重写虚拟方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-961">An abstract method declaration is permitted to override a virtual method.</span></span> <span data-ttu-id="6f0ea-962">这允许用于强制重新实现派生类中方法的抽象类，并使该方法的原始实现不可用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-962">This allows an abstract class to force re-implementation of the method in derived classes, and makes the original implementation of the method unavailable.</span></span> <span data-ttu-id="6f0ea-963">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-963">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }
}

abstract class B: A
{
    public abstract override void F();
}

class C: B
{
    public override void F() {
        Console.WriteLine("C.F");
    }
}
```
<span data-ttu-id="6f0ea-964">类`A`声明虚拟方法，类`B`重写此方法与抽象方法和类`C`重写抽象方法以提供其自己的实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-964">class `A` declares a virtual method, class `B` overrides this method with an abstract method, and class `C` overrides the abstract method to provide its own implementation.</span></span>

### <a name="external-methods"></a><span data-ttu-id="6f0ea-965">外部方法</span><span class="sxs-lookup"><span data-stu-id="6f0ea-965">External methods</span></span>

<span data-ttu-id="6f0ea-966">当一个方法声明包括`extern`修饰符、 说方法是可***外部方法***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-966">When a method declaration includes an `extern` modifier, that method is said to be an ***external method***.</span></span> <span data-ttu-id="6f0ea-967">外部方法是在外部实现，通常使用 C# 以外的语言。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-967">External methods are implemented externally, typically using a language other than C#.</span></span> <span data-ttu-id="6f0ea-968">外部方法声明不提供任何实际的实现，因为*method_body*外部方法的只包含一个分号。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-968">Because an external method declaration provides no actual implementation, the *method_body* of an external method simply consists of a semicolon.</span></span> <span data-ttu-id="6f0ea-969">外部方法可能不是泛型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-969">An external method may not be generic.</span></span>

<span data-ttu-id="6f0ea-970">`extern`修饰符通常与结合使用`DllImport`属性 ([与 COM 和 Win32 组件互操作](attributes.md#interoperation-with-com-and-win32-components))，从而允许外部方法由 Dll （动态链接库） 来实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-970">The `extern` modifier is typically used in conjunction with a `DllImport` attribute ([Interoperation with COM and Win32 components](attributes.md#interoperation-with-com-and-win32-components)), allowing external methods to be implemented by DLLs (Dynamic Link Libraries).</span></span> <span data-ttu-id="6f0ea-971">执行环境可以支持外部方法的实现可以来提供其他机制。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-971">The execution environment may support other mechanisms whereby implementations of external methods can be provided.</span></span>

<span data-ttu-id="6f0ea-972">当外部方法中包括`DllImport`属性，还必须包括在方法声明`static`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-972">When an external method includes a `DllImport` attribute, the method declaration must also include a `static` modifier.</span></span> <span data-ttu-id="6f0ea-973">此示例演示如何使用`extern`修饰符和`DllImport`属性：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-973">This example demonstrates the use of the `extern` modifier and the `DllImport` attribute:</span></span>
```csharp
using System.Text;
using System.Security.Permissions;
using System.Runtime.InteropServices;

class Path
{
    [DllImport("kernel32", SetLastError=true)]
    static extern bool CreateDirectory(string name, SecurityAttribute sa);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool RemoveDirectory(string name);

    [DllImport("kernel32", SetLastError=true)]
    static extern int GetCurrentDirectory(int bufSize, StringBuilder buf);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool SetCurrentDirectory(string name);
}
```

### <a name="partial-methods-recap"></a><span data-ttu-id="6f0ea-974">分部方法 （回顾）</span><span class="sxs-lookup"><span data-stu-id="6f0ea-974">Partial methods (recap)</span></span>

<span data-ttu-id="6f0ea-975">当一个方法声明包括`partial`修饰符、 说方法是可***分部方法***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-975">When a method declaration includes a `partial` modifier, that method is said to be a ***partial method***.</span></span> <span data-ttu-id="6f0ea-976">分部方法只能声明为分部类型的成员 ([分部类型](classes.md#partial-types))，并受到的限制数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-976">Partial methods can only be declared as members of partial types ([Partial types](classes.md#partial-types)), and are subject to a number of restrictions.</span></span> <span data-ttu-id="6f0ea-977">分部方法将进一步描述中[分部方法](classes.md#partial-methods)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-977">Partial methods are further described in [Partial methods](classes.md#partial-methods).</span></span>

### <a name="extension-methods"></a><span data-ttu-id="6f0ea-978">扩展方法</span><span class="sxs-lookup"><span data-stu-id="6f0ea-978">Extension methods</span></span>

<span data-ttu-id="6f0ea-979">当方法的第一个参数包括`this`修饰符、 说方法是可***扩展方法***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-979">When the first parameter of a method includes the `this` modifier, that method is said to be an ***extension method***.</span></span> <span data-ttu-id="6f0ea-980">只能在非泛型的非嵌套静态类中声明的扩展方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-980">Extension methods can only be declared in non-generic, non-nested static classes.</span></span> <span data-ttu-id="6f0ea-981">扩展方法的第一个参数而不可以使用任何修饰符`this`，且参数类型不能为指针类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-981">The first parameter of an extension method can have no modifiers other than `this`, and the parameter type cannot be a pointer type.</span></span>

<span data-ttu-id="6f0ea-982">下面是一个静态类，声明两个扩展方法的示例：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-982">The following is an example of a static class that declares two extension methods:</span></span>
```csharp
public static class Extensions
{
    public static int ToInt32(this string s) {
        return Int32.Parse(s);
    }

    public static T[] Slice<T>(this T[] source, int index, int count) {
        if (index < 0 || count < 0 || source.Length - index < count)
            throw new ArgumentException();
        T[] result = new T[count];
        Array.Copy(source, index, result, 0, count);
        return result;
    }
}
```

<span data-ttu-id="6f0ea-983">扩展方法是常规的静态方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-983">An extension method is a regular static method.</span></span> <span data-ttu-id="6f0ea-984">此外，其中它包含静态类是在作用域中，扩展方法可以使用来调用实例方法调用语法 ([扩展方法调用](expressions.md#extension-method-invocations))，作为第一个参数使用接收方的表达式。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-984">In addition, where its enclosing static class is in scope, an extension method can be invoked using instance method invocation syntax ([Extension method invocations](expressions.md#extension-method-invocations)), using the receiver expression as the first argument.</span></span>

<span data-ttu-id="6f0ea-985">以下程序使用上面已声明的扩展方法：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-985">The following program uses the extension methods declared above:</span></span>
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in strings.Slice(1, 2)) {
            Console.WriteLine(s.ToInt32());
        }
    }
}
```

<span data-ttu-id="6f0ea-986">`Slice`方法是在可用`string[]`，并`ToInt32`方法位于`string`，因为它们具有已声明为扩展方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-986">The `Slice` method is available on the `string[]`, and the `ToInt32` method is available on `string`, because they have been declared as extension methods.</span></span> <span data-ttu-id="6f0ea-987">该程序的含义等同于以下，并使用普通静态方法调用：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-987">The meaning of the program is the same as the following, using ordinary static method calls:</span></span>
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in Extensions.Slice(strings, 1, 2)) {
            Console.WriteLine(Extensions.ToInt32(s));
        }
    }
}
```

### <a name="method-body"></a><span data-ttu-id="6f0ea-988">方法主体</span><span class="sxs-lookup"><span data-stu-id="6f0ea-988">Method body</span></span>

<span data-ttu-id="6f0ea-989">*Method_body*方法的声明包含的块主体中，表达式主体或分号。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-989">The *method_body* of a method declaration consists of either a block body, an expression body or a semicolon.</span></span>

<span data-ttu-id="6f0ea-990">***结果类型***的一种方法是`void`如果返回类型为`void`，或如果的方法是异步和返回类型是`System.Threading.Tasks.Task`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-990">The ***result type*** of a method is `void` if the return type is `void`, or if the method is async and the return type is `System.Threading.Tasks.Task`.</span></span> <span data-ttu-id="6f0ea-991">否则，非异步方法的结果类型是其返回类型和异步方法的结果类型，返回类型`System.Threading.Tasks.Task<T>`是`T`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-991">Otherwise, the result type of a non-async method is its return type, and the result type of an async method with return type `System.Threading.Tasks.Task<T>` is `T`.</span></span>

<span data-ttu-id="6f0ea-992">当某个方法具有`void`导致类型和块正文`return`语句 ([return 语句](statements.md#the-return-statement)) 在块中不允许指定的表达式。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-992">When a method has a `void` result type and a block body, `return` statements ([The return statement](statements.md#the-return-statement)) in the block are not permitted to specify an expression.</span></span> <span data-ttu-id="6f0ea-993">如果块的 void 方法的执行正常完成 （也就是说，控制流末尾的方法主体），方法只需返回到其当前的调用方。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-993">If execution of the block of a void method completes normally (that is, control flows off the end of the method body), that method simply returns to its current caller.</span></span>
    
<span data-ttu-id="6f0ea-994">当某个方法具有`void`结果和表达式主体，表达式`E`必须是*statement_expression*，且正文是完全相当于窗体的块主体`{ E; }`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-994">When a method has a `void` result and an expression body, the expression `E` must be a *statement_expression*, and the body is exactly equivalent to a block body of the form `{ E; }`.</span></span>
    
<span data-ttu-id="6f0ea-995">当某个方法含有非空结果类型和一个块的正文，每个`return`块中的语句必须指定隐式转换为结果类型的表达式。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-995">When a method has a non-void result type and a block body, each `return` statement in the block must specify an expression that is implicitly convertible to the result type.</span></span> <span data-ttu-id="6f0ea-996">返回值的方法的块主体的终结点不能访问。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-996">The endpoint of a block body of a value-returning method must not be reachable.</span></span> <span data-ttu-id="6f0ea-997">换而言之，在具有块主体的返回值的方法，不是允许控制流的方法正文末尾。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-997">In other words, in a value-returning method with a block body, control is not permitted to flow off the end of the method body.</span></span>
    
<span data-ttu-id="6f0ea-998">当某个方法具有非 void 结果类型，表达式主体，该表达式必须是隐式转换为结果类型，且正文是完全相当于窗体的块主体`{ return E; }`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-998">When a method has a non-void result type and an expression body, the expression must be implicitly convertible to the result type, and the body is exactly equivalent to a block body of the form `{ return E; }`.</span></span>
    
<span data-ttu-id="6f0ea-999">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-999">In the example</span></span>
```csharp
class A
{
    public int F() {}            // Error, return value required

    public int G() {
        return 1;
    }

    public int H(bool b) {
        if (b) {
            return 1;
        }
        else {
            return 0;
        }
    }

    public int I(bool b) => b ? 1 : 0;
}
```
<span data-ttu-id="6f0ea-1000">返回值的`F`方法导致在编译时错误，因为控制可以超出方法正文末尾。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1000">the value-returning `F` method results in a compile-time error because control can flow off the end of the method body.</span></span> <span data-ttu-id="6f0ea-1001">`G`和`H`方法是否正确，因为所有可能的执行路径中指定一个返回值的 return 语句结束。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1001">The `G` and `H` methods are correct because all possible execution paths end in a return statement that specifies a return value.</span></span> <span data-ttu-id="6f0ea-1002">`I`方法是否正确，因为其主体等效于只是单个的返回语句中包含的语句块。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1002">The `I` method is correct, because its body is equivalent to a statement block with just a single return statement in it.</span></span>

### <a name="method-overloading"></a><span data-ttu-id="6f0ea-1003">方法重载</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1003">Method overloading</span></span>

<span data-ttu-id="6f0ea-1004">方法重载决策规则所述[类型推理](expressions.md#type-inference)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1004">The method overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="properties"></a><span data-ttu-id="6f0ea-1005">属性</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1005">Properties</span></span>

<span data-ttu-id="6f0ea-1006">一个***属性***成员提供访问权限的对象或类的特征。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1006">A ***property*** is a member that provides access to a characteristic of an object or a class.</span></span> <span data-ttu-id="6f0ea-1007">属性的示例包括字符串的长度，一种字体，窗口中，客户的名称的标题的大小等等。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1007">Examples of properties include the length of a string, the size of a font, the caption of a window, the name of a customer, and so on.</span></span> <span data-ttu-id="6f0ea-1008">属性是字段的自然扩展，都命名的成员与相关的类型，并且访问字段和属性的语法相同。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1008">Properties are a natural extension of fields—both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="6f0ea-1009">不过，与字段不同的是，属性不指明存储位置。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1009">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="6f0ea-1010">相反，属性包含***访问器***，用于指定在读取或写入属性值时要执行的语句。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1010">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span> <span data-ttu-id="6f0ea-1011">因此，属性提供用于将操作与读取和写入对象的属性，则相关联的机制此外，不允许此类属性，将进行计算。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1011">Properties thus provide a mechanism for associating actions with the reading and writing of an object's attributes; furthermore, they permit such attributes to be computed.</span></span>

<span data-ttu-id="6f0ea-1012">属性使用声明*property_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1012">Properties are declared using *property_declaration*s:</span></span>

```antlr
property_declaration
    : attributes? property_modifier* type member_name property_body
    ;

property_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | property_modifier_unsafe
    ;

property_body
    : '{' accessor_declarations '}' property_initializer?
    | '=>' expression ';'
    ;

property_initializer
    : '=' variable_initializer ';'
    ;
```

<span data-ttu-id="6f0ea-1013">一个*property_declaration*可能包括一套*特性*([特性](attributes.md)) 和四种访问修饰符的有效组合 ([访问修饰符](classes.md#access-modifiers))，则`new`([的新修饰符](classes.md#the-new-modifier))， `static` ([静态和实例方法](classes.md#static-and-instance-methods))， `virtual` ([虚拟方法](classes.md#virtual-methods))， `override` ([重写方法](classes.md#override-methods))， `sealed` ([密封方法](classes.md#sealed-methods))， `abstract` ([抽象方法](classes.md#abstract-methods))，并`extern` ([外部方法](classes.md#external-methods)) 修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1013">A *property_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="6f0ea-1014">属性声明可能会有所与方法声明相同的规则 ([方法](classes.md#methods)) 关于修饰符的有效组合。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1014">Property declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="6f0ea-1015">*类型*声明的属性指定该声明引入的属性的类型和*member_name*指定属性的名称。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1015">The *type* of a property declaration specifies the type of the property introduced by the declaration, and the *member_name* specifies the name of the property.</span></span> <span data-ttu-id="6f0ea-1016">该属性是显式接口成员的实现，除非*member_name*是只需*标识符*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1016">Unless the property is an explicit interface member implementation, the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="6f0ea-1017">为了使显式接口成员实现 ([显式接口成员实现代码](interfaces.md#explicit-interface-member-implementations))，则*member_name*组成*interface_type*跟"`.`"和一个*标识符*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1017">For an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="6f0ea-1018">*类型*的属性必须至少与该属性本身具有同样的可访问性 ([可访问性约束](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1018">The *type* of a property must be at least as accessible as the property itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6f0ea-1019">一个*property_body*可能是组成***访问器正文***或***表达式主体***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1019">A *property_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="6f0ea-1020">在访问器正文中， *accessor_declarations*，必须在用"`{`"和"`}`"令牌，声明访问器 ([访问器](classes.md#accessors)) 的属性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1020">In an accessor body,  *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="6f0ea-1021">访问器指定与读取和写入属性相关联的可执行语句。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1021">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="6f0ea-1022">表达式主体组成`=>`跟*表达式*`E`分号，完全等效于在语句体`{ get { return E; } }`，并因此仅可指定仅定义了 getter属性的 getter 的结果由单个表达式。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1022">An expression body consisting of `=>` followed by an *expression* `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only properties where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="6f0ea-1023">一个*property_initializer*可能仅被授予以自动实现属性 ([自动实现的属性](classes.md#automatically-implemented-properties))，并导致此类的基础字段的初始化具有给定值的属性*表达式*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1023">A *property_initializer* may only be given for an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)), and causes the initialization of the underlying field of such properties with the value given by the *expression*.</span></span>

<span data-ttu-id="6f0ea-1024">即使访问属性的语法是相同的字段，一个属性是未归类为变量。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1024">Even though the syntax for accessing a property is the same as that for a field, a property is not classified as a variable.</span></span> <span data-ttu-id="6f0ea-1025">因此，不能将属性作为`ref`或`out`参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1025">Thus, it is not possible to pass a property as a `ref` or `out` argument.</span></span>

<span data-ttu-id="6f0ea-1026">如果属性声明包含`extern`修饰符，该属性称为***外部属性***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1026">When a property declaration includes an `extern` modifier, the property is said to be an ***external property***.</span></span> <span data-ttu-id="6f0ea-1027">因为外部属性声明不提供任何实际的实现，每个其*accessor_declarations*包含一个分号。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1027">Because an external property declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

### <a name="static-and-instance-properties"></a><span data-ttu-id="6f0ea-1028">静态和实例属性</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1028">Static and instance properties</span></span>

<span data-ttu-id="6f0ea-1029">如果属性声明包含`static`修饰符，该属性称为***静态属性***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1029">When a property declaration includes a `static` modifier, the property is said to be a ***static property***.</span></span> <span data-ttu-id="6f0ea-1030">如果未`static`修饰符存在，该属性称为***实例属性***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1030">When no `static` modifier is present, the property is said to be an ***instance property***.</span></span>

<span data-ttu-id="6f0ea-1031">静态属性不与特定实例相关联，它是指导致编译时错误`this`的静态属性访问器中。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1031">A static property is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static property.</span></span>

<span data-ttu-id="6f0ea-1032">实例属性与给定类的实例相关联，并且该实例可作为访问`this`([此访问权限](expressions.md#this-access)) 中的该属性访问器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1032">An instance property is associated with a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that property.</span></span>

<span data-ttu-id="6f0ea-1033">中引用属性时*member_access* ([成员访问](expressions.md#member-access)) 的窗体`E.M`，如果`M`是一个静态属性，`E`必须表示一个类型，包含`M`，并且如果`M`是实例属性，则 E 必须表示一个类型，其中包含的实例`M`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1033">When a property is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static property, `E` must denote a type containing `M`, and if `M` is an instance property, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="6f0ea-1034">静态之间的差异和实例成员进行讨论中进一步[静态和实例成员](classes.md#static-and-instance-members)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1034">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="accessors"></a><span data-ttu-id="6f0ea-1035">访问器</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1035">Accessors</span></span>

<span data-ttu-id="6f0ea-1036">*Accessor_declarations*属性指定与读取和写入该属性相关联的可执行语句。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1036">The *accessor_declarations* of a property specify the executable statements associated with reading and writing that property.</span></span>

```antlr
accessor_declarations
    : get_accessor_declaration set_accessor_declaration?
    | set_accessor_declaration get_accessor_declaration?
    ;

get_accessor_declaration
    : attributes? accessor_modifier? 'get' accessor_body
    ;

set_accessor_declaration
    : attributes? accessor_modifier? 'set' accessor_body
    ;

accessor_modifier
    : 'protected'
    | 'internal'
    | 'private'
    | 'protected' 'internal'
    | 'internal' 'protected'
    ;

accessor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="6f0ea-1037">访问器声明组成*get_accessor_declaration*即*set_accessor_declaration*，和 / 或。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1037">The accessor declarations consist of a *get_accessor_declaration*, a *set_accessor_declaration*, or both.</span></span> <span data-ttu-id="6f0ea-1038">每个访问器声明包含令牌`get`或`set`跟一个可选*accessor_modifier*和一个*accessor_body*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1038">Each accessor declaration consists of the token `get` or `set` followed by an optional *accessor_modifier* and an *accessor_body*.</span></span>

<span data-ttu-id="6f0ea-1039">利用*accessor_modifier*s 受到以下限制：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1039">The use of *accessor_modifier*s is governed by the following restrictions:</span></span>

*  <span data-ttu-id="6f0ea-1040">*Accessor_modifier*不能在接口或显式接口成员实现中使用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1040">An *accessor_modifier* may not be used in an interface or in an explicit interface member implementation.</span></span>
*  <span data-ttu-id="6f0ea-1041">属性或索引器没有`override`修饰符， *accessor_modifier*仅当属性或索引器同时具有允许`get`和`set`访问器，然后允许仅在其中一个访问器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1041">For a property or indexer that has no `override` modifier, an *accessor_modifier* is permitted only if the property or indexer has both a `get` and `set` accessor, and then is permitted only on one of those accessors.</span></span>
*  <span data-ttu-id="6f0ea-1042">属性或索引器，包括`override`修饰符，访问器必须匹配*accessor_modifier*(如果有） 的访问器被重写。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1042">For a property or indexer that includes an `override` modifier, an accessor must match the *accessor_modifier*, if any, of the accessor being overridden.</span></span>
*  <span data-ttu-id="6f0ea-1043">*Accessor_modifier*必须声明一个严格限制性比属性或索引器本身的声明可访问性的可访问性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1043">The *accessor_modifier* must declare an accessibility that is strictly more restrictive than the declared accessibility of the property or indexer itself.</span></span> <span data-ttu-id="6f0ea-1044">若要确切地说：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1044">To be precise:</span></span>
   * <span data-ttu-id="6f0ea-1045">如果属性或索引器已声明可访问性`public`，则*accessor_modifier*可能是`protected internal`， `internal`， `protected`，或`private`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1045">If the property or indexer has a declared accessibility of `public`, the *accessor_modifier* may be either `protected internal`, `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="6f0ea-1046">如果属性或索引器已声明可访问性`protected internal`，则*accessor_modifier*可能是`internal`， `protected`，或`private`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1046">If the property or indexer has a declared accessibility of `protected internal`, the *accessor_modifier* may be either `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="6f0ea-1047">如果属性或索引器已声明可访问性`internal`或`protected`，则*accessor_modifier*必须是`private`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1047">If the property or indexer has a declared accessibility of `internal` or `protected`, the *accessor_modifier* must be `private`.</span></span>
   * <span data-ttu-id="6f0ea-1048">如果属性或索引器已声明可访问性`private`，无*accessor_modifier*可能会使用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1048">If the property or indexer has a declared accessibility of `private`, no *accessor_modifier* may be used.</span></span>

<span data-ttu-id="6f0ea-1049">有关`abstract`并`extern`属性， *accessor_body*指定每个访问器是只需一个分号。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1049">For `abstract` and `extern` properties, the *accessor_body* for each accessor specified is simply a semicolon.</span></span> <span data-ttu-id="6f0ea-1050">一个非抽象、 非外部属性可以具有每个*accessor_body*是一个分号，在这种情况下很***自动实现的属性***([自动实现的属性](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1050">A non-abstract, non-extern property may have each *accessor_body* be a semicolon, in which case it is an ***automatically implemented property*** ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="6f0ea-1051">自动实现的属性必须具有至少一个 get 访问器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1051">An automatically implemented property must have at least a get accessor.</span></span> <span data-ttu-id="6f0ea-1052">任何其他非抽象、 非外部属性，访问器的*accessor_body*是*块*它指定要调用的相应的访问器时要执行的语句。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1052">For the accessors of any other non-abstract, non-extern property, the *accessor_body* is a *block* which specifies the statements to be executed when the corresponding accessor is invoked.</span></span>

<span data-ttu-id="6f0ea-1053">一个`get`访问器对应于具有返回值的属性类型的无参数方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1053">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="6f0ea-1054">当在表达式中，引用属性除了作为赋值目标`get`属性访问器调用以计算属性的值 ([表达式的值](expressions.md#values-of-expressions))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1054">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property ([Values of expressions](expressions.md#values-of-expressions)).</span></span> <span data-ttu-id="6f0ea-1055">正文`get`访问器必须符合有关返回值的规则中所述方法[方法体](classes.md#method-body)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1055">The body of a `get` accessor must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="6f0ea-1056">具体而言，所有`return`语句的正文中`get`访问器必须指定隐式转换为属性类型的表达式。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1056">In particular, all `return` statements in the body of a `get` accessor must specify an expression that is implicitly convertible to the property type.</span></span> <span data-ttu-id="6f0ea-1057">此外，终结点的`get`访问器不能访问。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1057">Furthermore, the endpoint of a `get` accessor must not be reachable.</span></span>

<span data-ttu-id="6f0ea-1058">一个`set`访问器对应于具有单个值的属性类型参数的方法和一个`void`返回类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1058">A `set` accessor corresponds to a method with a single value parameter of the property type and a `void` return type.</span></span> <span data-ttu-id="6f0ea-1059">隐式参数`set`访问器始终命名为`value`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1059">The implicit parameter of a `set` accessor is always named `value`.</span></span> <span data-ttu-id="6f0ea-1060">将属性作为赋值的目标的引用时 ([赋值运算符](expressions.md#assignment-operators))，或为操作数`++`或`--`([后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators)， [前缀递增和递减运算符](expressions.md#prefix-increment-and-decrement-operators))，则`set`用的参数调用访问器 (其值为赋值的右侧或的操作数`++`或`--`运算符)，提供新值 ([简单的赋值](expressions.md#simple-assignment))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1060">When a property is referenced as the target of an assignment ([Assignment operators](expressions.md#assignment-operators)), or as the operand of `++` or `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), the `set` accessor is invoked with an argument (whose value is that of the right-hand side of the assignment or the operand of the `++` or `--` operator) that provides the new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="6f0ea-1061">正文`set`访问器必须遵守的规则`void`中所述方法[方法主体](classes.md#method-body)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1061">The body of a `set` accessor must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="6f0ea-1062">具体而言，`return`中的语句`set`访问器正文不允许指定的表达式。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1062">In particular, `return` statements in the `set` accessor body are not permitted to specify an expression.</span></span> <span data-ttu-id="6f0ea-1063">由于`set`访问器隐式具有一个名为参数`value`，它是本地变量或常量声明中的编译时错误`set`访问器具有该名称。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1063">Since a `set` accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declaration in a `set` accessor to have that name.</span></span>

<span data-ttu-id="6f0ea-1064">基于是否存在或缺少`get`和`set`访问器属性分类，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1064">Based on the presence or absence of the `get` and `set` accessors, a property is classified as follows:</span></span>

*  <span data-ttu-id="6f0ea-1065">一个属性，包括这两个`get`访问器和一个`set`访问器称为***读写***属性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1065">A property that includes both a `get` accessor and a `set` accessor is said to be a ***read-write*** property.</span></span>
*  <span data-ttu-id="6f0ea-1066">仅具有一个属性`get`访问器称为***只读***属性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1066">A property that has only a `get` accessor is said to be a ***read-only*** property.</span></span> <span data-ttu-id="6f0ea-1067">它是只读的属性赋值的目标的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1067">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>
*  <span data-ttu-id="6f0ea-1068">仅具有一个属性`set`访问器称为***只写***属性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1068">A property that has only a `set` accessor is said to be a ***write-only*** property.</span></span> <span data-ttu-id="6f0ea-1069">除了作为赋值的目标，它是以引用在表达式中的只写属性的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1069">Except as the target of an assignment, it is a compile-time error to reference a write-only property in an expression.</span></span>

<span data-ttu-id="6f0ea-1070">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1070">In the example</span></span>
```csharp
public class Button: Control
{
    private string caption;

    public string Caption {
        get {
            return caption;
        }
        set {
            if (caption != value) {
                caption = value;
                Repaint();
            }
        }
    }

    public override void Paint(Graphics g, Rectangle r) {
        // Painting code goes here
    }
}
```
<span data-ttu-id="6f0ea-1071">`Button`控制声明一个公共`Caption`属性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1071">the `Button` control declares a public `Caption` property.</span></span> <span data-ttu-id="6f0ea-1072">`get`访问器的`Caption`属性将返回字符串存储在私有`caption`字段。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1072">The `get` accessor of the `Caption` property returns the string stored in the private `caption` field.</span></span> <span data-ttu-id="6f0ea-1073">`set`访问器会检查是否不同于当前值，新值，如果是这样，它存储的新值并重新绘制控件。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1073">The `set` accessor checks if the new value is different from the current value, and if so, it stores the new value and repaints the control.</span></span> <span data-ttu-id="6f0ea-1074">属性通常遵循上面所示的模式：`get`访问器仅返回存储的私有字段中的值和`set`访问器修改该私有字段，然后执行完全更新的状态所需的任何其他操作对象。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1074">Properties often follow the pattern shown above: The `get` accessor simply returns a value stored in a private field, and the `set` accessor modifies that private field and then performs any additional actions required to fully update the state of the object.</span></span>

<span data-ttu-id="6f0ea-1075">给定`Button`上面的类，以下是使用的示例`Caption`属性：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1075">Given the `Button` class above, the following is an example of use of the `Caption` property:</span></span>
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

<span data-ttu-id="6f0ea-1076">在这里，`set`通过将值分配给属性，调用访问器和`get`通过引用在表达式中的属性来调用访问器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1076">Here, the `set` accessor is invoked by assigning a value to the property, and the `get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="6f0ea-1077">`get`和`set`属性访问器不是不同的成员，并且不能单独声明属性访问器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1077">The `get` and `set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="6f0ea-1078">在这种情况下，不能使读写属性的两个访问器具有不同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1078">As such, it is not possible for the two accessors of a read-write property to have different accessibility.</span></span> <span data-ttu-id="6f0ea-1079">该示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1079">The example</span></span>
```csharp
class A
{
    private string name;

    public string Name {                // Error, duplicate member name
        get { return name; }
    }

    public string Name {                // Error, duplicate member name
        set { name = value; }
    }
}
```
<span data-ttu-id="6f0ea-1080">未声明的单一的读-写属性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1080">does not declare a single read-write property.</span></span> <span data-ttu-id="6f0ea-1081">相反，它声明了两个具有相同的名称，其中一个只读属性，另一个只写。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1081">Rather, it declares two properties with the same name, one read-only and one write-only.</span></span> <span data-ttu-id="6f0ea-1082">由于在同一类中声明的两个成员不能具有相同的名称，则示例将导致编译时错误发生。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1082">Since two members declared in the same class cannot have the same name, the example causes a compile-time error to occur.</span></span>

<span data-ttu-id="6f0ea-1083">当在派生的类继承的属性与相同的名称声明一个属性时, 派生的属性将会隐藏继承的属性相对于读取和写入。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1083">When a derived class declares a property by the same name as an inherited property, the derived property hides the inherited property with respect to both reading and writing.</span></span> <span data-ttu-id="6f0ea-1084">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1084">In the example</span></span>
```csharp
class A
{
    public int P {
        set {...}
    }
}

class B: A
{
    new public int P {
        get {...}
    }
}
```
<span data-ttu-id="6f0ea-1085">`P`中的属性`B`隐藏`P`中的属性`A`相对于读取和写入。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1085">the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing.</span></span> <span data-ttu-id="6f0ea-1086">因此，在语句中</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1086">Thus, in the statements</span></span>
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
<span data-ttu-id="6f0ea-1087">分配给`b.P`会导致编译时错误报告，因为只读`P`属性中的`B`隐藏只写`P`中的属性`A`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1087">the assignment to `b.P` causes a compile-time error to be reported, since the read-only `P` property in `B` hides the write-only `P` property in `A`.</span></span> <span data-ttu-id="6f0ea-1088">但请注意，强制转换可用于访问隐藏`P`属性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1088">Note, however, that a cast can be used to access the hidden `P` property.</span></span>

<span data-ttu-id="6f0ea-1089">与公共字段、 属性提供对象的内部状态和它的公共接口之间的分隔。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1089">Unlike public fields, properties provide a separation between an object's internal state and its public interface.</span></span> <span data-ttu-id="6f0ea-1090">为例：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1090">Consider the example:</span></span>
```csharp
class Label
{
    private int x, y;
    private string caption;

    public Label(int x, int y, string caption) {
        this.x = x;
        this.y = y;
        this.caption = caption;
    }

    public int X {
        get { return x; }
    }

    public int Y {
        get { return y; }
    }

    public Point Location {
        get { return new Point(x, y); }
    }

    public string Caption {
        get { return caption; }
    }
}
```

<span data-ttu-id="6f0ea-1091">在这里，`Label`类使用两个`int`字段中，`x`和`y`，以存储其位置。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1091">Here, the `Label` class uses two `int` fields, `x` and `y`, to store its location.</span></span> <span data-ttu-id="6f0ea-1092">公开的位置是公开既用作`X`和一个`Y`属性和 as`Location`类型的属性`Point`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1092">The location is publicly exposed both as an `X` and a `Y` property and as a `Location` property of type `Point`.</span></span> <span data-ttu-id="6f0ea-1093">如果在未来版本的`Label`，将变得更方便地存储所在的位置`Point`可以在内部，而不会影响类的公共接口进行更改：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1093">If, in a future version of `Label`, it becomes more convenient to store the location as a `Point` internally, the change can be made without affecting the public interface of the class:</span></span>
```csharp
class Label
{
    private Point location;
    private string caption;

    public Label(int x, int y, string caption) {
        this.location = new Point(x, y);
        this.caption = caption;
    }

    public int X {
        get { return location.x; }
    }

    public int Y {
        get { return location.y; }
    }

    public Point Location {
        get { return location; }
    }

    public string Caption {
        get { return caption; }
    }
}
```

<span data-ttu-id="6f0ea-1094">有`x`并`y`改为已`public readonly`字段中，重要的是不可能进行这样的更改到`Label`类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1094">Had `x` and `y` instead been `public readonly` fields, it would have been impossible to make such a change to the `Label` class.</span></span>

<span data-ttu-id="6f0ea-1095">通过属性公开状态不一定比直接公开字段效率低。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1095">Exposing state through properties is not necessarily any less efficient than exposing fields directly.</span></span> <span data-ttu-id="6f0ea-1096">具体而言，当属性为非虚拟的且包含只有少量的代码，执行环境可能对访问器的调用将替换为实际代码的访问器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1096">In particular, when a property is non-virtual and contains only a small amount of code, the execution environment may replace calls to accessors with the actual code of the accessors.</span></span> <span data-ttu-id="6f0ea-1097">此过程被称为***内联***，和它，使属性访问不如字段访问，同时还保留属性的更高的灵活性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1097">This process is known as ***inlining***, and it makes property access as efficient as field access, yet preserves the increased flexibility of properties.</span></span>

<span data-ttu-id="6f0ea-1098">由于调用`get`访问器，从概念上讲相当于读取字段的值，它被认为是好的编程风格的`get`访问器具有明显的副作用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1098">Since invoking a `get` accessor is conceptually equivalent to reading the value of a field, it is considered bad programming style for `get` accessors to have observable side-effects.</span></span> <span data-ttu-id="6f0ea-1099">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1099">In the example</span></span>
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
<span data-ttu-id="6f0ea-1100">值`Next`属性依赖于以前访问的属性的次数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1100">the value of the `Next` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="6f0ea-1101">因此，访问该属性会产生明显负面影响，并且此属性应作为一种方法实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1101">Thus, accessing the property produces an observable side-effect, and the property should be implemented as a method instead.</span></span>

<span data-ttu-id="6f0ea-1102">为"无其他作用"约定`get`访问器并不意味着`get`访问器始终应编写为只返回字段中存储的值。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1102">The "no side-effects" convention for `get` accessors doesn't mean that `get` accessors should always be written to simply return values stored in fields.</span></span> <span data-ttu-id="6f0ea-1103">实际上，`get`访问器通常通过访问多个字段或调用方法计算属性的值。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1103">Indeed, `get` accessors often compute the value of a property by accessing multiple fields or invoking methods.</span></span> <span data-ttu-id="6f0ea-1104">但是，在正确设计`get`访问器执行的任何操作都导致中对象的状态的可观察的更改。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1104">However, a properly designed `get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="6f0ea-1105">属性可用于资源直到首次引用的时的初始化延迟。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1105">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="6f0ea-1106">例如：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1106">For example:</span></span>
```csharp
using System.IO;

public class Console
{
    private static TextReader reader;
    private static TextWriter writer;
    private static TextWriter error;

    public static TextReader In {
        get {
            if (reader == null) {
                reader = new StreamReader(Console.OpenStandardInput());
            }
            return reader;
        }
    }

    public static TextWriter Out {
        get {
            if (writer == null) {
                writer = new StreamWriter(Console.OpenStandardOutput());
            }
            return writer;
        }
    }

    public static TextWriter Error {
        get {
            if (error == null) {
                error = new StreamWriter(Console.OpenStandardError());
            }
            return error;
        }
    }
}
```

<span data-ttu-id="6f0ea-1107">`Console`类包含三个属性`In`， `Out`，和`Error`，分别表示标准输入、 输出和错误设备。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1107">The `Console` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="6f0ea-1108">通过公开为属性，这些成员`Console`类可以延迟其初始化，直到它们被实际使用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1108">By exposing these members as properties, the `Console` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="6f0ea-1109">例如，在第一次引用时`Out`属性，如下所示</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1109">For example, upon first referencing the `Out` property, as in</span></span>
```csharp
Console.Out.WriteLine("hello, world");
```
<span data-ttu-id="6f0ea-1110">基础`TextWriter`创建输出设备。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1110">the underlying `TextWriter` for the output device is created.</span></span> <span data-ttu-id="6f0ea-1111">但如果应用程序不引用`In`和`Error`为这些设备创建属性，则任何对象。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1111">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="6f0ea-1112">自动实现的属性</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1112">Automatically implemented properties</span></span>

<span data-ttu-id="6f0ea-1113">自动实现的属性 (或***自动属性***简称)，是具有仅限分号的访问器正文的非抽象非外部属性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1113">An automatically implemented property (or ***auto-property*** for short), is a non-abstract non-extern property with semicolon-only accessor bodies.</span></span> <span data-ttu-id="6f0ea-1114">自动属性必须具有 get 访问器，并可根据需要 set 访问器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1114">Auto-properties must have a get accessor and can optionally have a set accessor.</span></span>

<span data-ttu-id="6f0ea-1115">属性指定为自动实现的属性，对于属性，自动提供一个隐藏的支持字段，并且访问器的实现是为了读取和写入到该支持字段。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1115">When a property is specified as an automatically implemented property, a hidden backing field is automatically available for the property, and the accessors are implemented to read from and write to that backing field.</span></span> <span data-ttu-id="6f0ea-1116">如果自动属性没有 set 访问器，支持字段被视为`readonly`([只读字段](classes.md#readonly-fields))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1116">If the auto-property has no set accessor, the backing field is considered `readonly` ([Readonly fields](classes.md#readonly-fields)).</span></span> <span data-ttu-id="6f0ea-1117">就像`readonly`字段中，仅定义了 getter 的自动属性还中可以分配到封闭类的构造函数的正文。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1117">Just like a `readonly` field, a getter-only auto-property can also be assigned to in the body of a constructor of the enclosing class.</span></span> <span data-ttu-id="6f0ea-1118">此类赋值将直接分配到只读支持字段的属性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1118">Such an assignment assigns directly to the readonly backing field of the property.</span></span>

<span data-ttu-id="6f0ea-1119">自动属性还可以具有*property_initializer*，这将直接应用到为支持字段*variable_initializer* ([变量初始值设定项](classes.md#variable-initializers)).</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1119">An auto-property may optionally have a *property_initializer*, which is applied directly to the backing field as a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)).</span></span>

<span data-ttu-id="6f0ea-1120">下面的示例：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1120">The following example:</span></span>
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
<span data-ttu-id="6f0ea-1121">等效于以下声明：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1121">is equivalent to the following declaration:</span></span>
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

<span data-ttu-id="6f0ea-1122">下面的示例：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1122">The following example:</span></span>
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
<span data-ttu-id="6f0ea-1123">等效于以下声明：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1123">is equivalent to the following declaration:</span></span>
```csharp
public class ReadOnlyPoint
{
    private readonly int __x;
    private readonly int __y;
    public int X { get { return __x; } }
    public int Y { get { return __y; } }
    public ReadOnlyPoint(int x, int y) { __x = x; __y = y; }
}
```

<span data-ttu-id="6f0ea-1124">请注意，分配到只读字段合法的因为它们构造函数内发生。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1124">Notice that the assignments to the readonly field are legal, because they occur within the constructor.</span></span>


### <a name="accessibility"></a><span data-ttu-id="6f0ea-1125">可访问性</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1125">Accessibility</span></span>

<span data-ttu-id="6f0ea-1126">如果访问器具有*accessor_modifier*，可访问域 ([可访问性域](basic-concepts.md#accessibility-domains)) 的访问器确定使用的声明可访问性*accessor_modifier*.</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1126">If an accessor has an *accessor_modifier*, the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the accessor is determined using the declared accessibility of the *accessor_modifier*.</span></span> <span data-ttu-id="6f0ea-1127">如果访问器没有*accessor_modifier*，访问器的可访问性域由属性或索引器的声明可访问性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1127">If an accessor does not have an *accessor_modifier*, the accessibility domain of the accessor is determined from the declared accessibility of the property or indexer.</span></span>

<span data-ttu-id="6f0ea-1128">是否存在*accessor_modifier*永远不会影响成员查找 ([运算符](expressions.md#operators)) 或重载决策 ([重载决策](expressions.md#overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1128">The presence of an *accessor_modifier* never affects member lookup ([Operators](expressions.md#operators)) or overload resolution ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="6f0ea-1129">属性或索引器上的修饰符始终确定哪些属性或索引器绑定到，而不考虑访问的上下文。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1129">The modifiers on the property or indexer always determine which property or indexer is bound to, regardless of the context of the access.</span></span>

<span data-ttu-id="6f0ea-1130">一旦选择了特定属性或索引器，所涉及的特定访问器的可访问性域用于确定使用情况是否有效：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1130">Once a particular property or indexer has been selected, the accessibility domains of the specific accessors involved are used to determine if that usage is valid:</span></span>

*  <span data-ttu-id="6f0ea-1131">如果用法是作为一个值 ([表达式的值](expressions.md#values-of-expressions))，则`get`访问器必须存在并且可访问。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1131">If the usage is as a value ([Values of expressions](expressions.md#values-of-expressions)), the `get` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="6f0ea-1132">如果用法是作为简单赋值的目标 ([简单的赋值](expressions.md#simple-assignment))，则`set`访问器必须存在并且可访问。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1132">If the usage is as the target of a simple assignment ([Simple assignment](expressions.md#simple-assignment)), the `set` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="6f0ea-1133">使用情况是否为目标的复合赋值 ([复合赋值](expressions.md#compound-assignment))，或为目标的`++`或`--`运算符 ([函数成员](expressions.md#function-members).9， [调用表达式](expressions.md#invocation-expressions))，这两个`get`访问器和`set`访问器必须存在并且可访问。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1133">If the usage is as the target of compound assignment ([Compound assignment](expressions.md#compound-assignment)), or as the target of the `++` or `--` operators ([Function members](expressions.md#function-members).9, [Invocation expressions](expressions.md#invocation-expressions)), both the `get` accessors and the `set` accessor must exist and be accessible.</span></span>

<span data-ttu-id="6f0ea-1134">在下面的示例中，该属性`A.Text`由属性隐藏`B.Text`，在仅在上下文中甚至`set`调用访问器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1134">In the following example, the property `A.Text` is hidden by the property `B.Text`, even in contexts where only the `set` accessor is called.</span></span> <span data-ttu-id="6f0ea-1135">与之相反，该属性`B.Count`不能访问类`M`，因此可访问属性`A.Count`改为使用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1135">In contrast, the property `B.Count` is not accessible to class `M`, so the accessible property `A.Count` is used instead.</span></span>

```csharp
class A
{
    public string Text {
        get { return "hello"; }
        set { }
    }

    public int Count {
        get { return 5; }
        set { }
    }
}

class B: A
{
    private string text = "goodbye"; 
    private int count = 0;

    new public string Text {
        get { return text; }
        protected set { text = value; }
    }

    new protected int Count { 
        get { return count; }
        set { count = value; }
    }
}

class M
{
    static void Main() {
        B b = new B();
        b.Count = 12;             // Calls A.Count set accessor
        int i = b.Count;          // Calls A.Count get accessor
        b.Text = "howdy";         // Error, B.Text set accessor not accessible
        string s = b.Text;        // Calls B.Text get accessor
    }
}
```

<span data-ttu-id="6f0ea-1136">用于实现接口的取值函数可能没有*accessor_modifier*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1136">An accessor that is used to implement an interface may not have an *accessor_modifier*.</span></span> <span data-ttu-id="6f0ea-1137">如果只有一个访问器用于实现一个接口，可以使用声明其他访问器*accessor_modifier*:</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1137">If only one accessor is used to implement an interface, the other accessor may be declared with an *accessor_modifier*:</span></span>
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a><span data-ttu-id="6f0ea-1138">虚拟，密封的重写方法和抽象属性访问器</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1138">Virtual, sealed, override, and abstract property accessors</span></span>

<span data-ttu-id="6f0ea-1139">一个`virtual`属性声明指定的属性访问器是虚拟。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1139">A `virtual` property declaration specifies that the accessors of the property are virtual.</span></span> <span data-ttu-id="6f0ea-1140">`virtual`修饰符将应用于这两个访问器的读写属性，不可能只有一个访问器的读写属性必须是虚拟的。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1140">The `virtual` modifier applies to both accessors of a read-write property—it is not possible for only one accessor of a read-write property to be virtual.</span></span>

<span data-ttu-id="6f0ea-1141">`abstract`属性声明指定的属性访问器是虚拟的但不提供访问器的实际实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1141">An `abstract` property declaration specifies that the accessors of the property are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="6f0ea-1142">相反，非抽象派生的类所需通过重写该属性提供自己的访问器的实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1142">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the property.</span></span> <span data-ttu-id="6f0ea-1143">因为抽象属性声明的访问器不提供任何实际的实现，其*accessor_body*只包含一个分号。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1143">Because an accessor for an abstract property declaration provides no actual implementation, its *accessor_body* simply consists of a semicolon.</span></span>

<span data-ttu-id="6f0ea-1144">属性声明，包括这两个`abstract`和`override`修饰符指定该属性是抽象的重写基属性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1144">A property declaration that includes both the `abstract` and `override` modifiers specifies that the property is abstract and overrides a base property.</span></span> <span data-ttu-id="6f0ea-1145">此类属性访问器也是抽象的。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1145">The accessors of such a property are also abstract.</span></span>

<span data-ttu-id="6f0ea-1146">抽象类中才允许抽象属性声明 ([抽象类](classes.md#abstract-classes))。继承的虚属性访问器可以重写派生类中通过包括指定的属性声明`override`指令。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1146">Abstract property declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).The accessors of an inherited virtual property can be overridden in a derived class by including a property declaration that specifies an `override` directive.</span></span> <span data-ttu-id="6f0ea-1147">这称为***重写属性声明***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1147">This is known as an ***overriding property declaration***.</span></span> <span data-ttu-id="6f0ea-1148">重写属性声明不声明一个新的属性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1148">An overriding property declaration does not declare a new property.</span></span> <span data-ttu-id="6f0ea-1149">相反，它只是专用化现有虚拟属性的访问器的实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1149">Instead, it simply specializes the implementations of the accessors of an existing virtual property.</span></span>

<span data-ttu-id="6f0ea-1150">重写属性声明必须指定与继承的属性完全相同的可访问性修饰符、 类型和名称。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1150">An overriding property declaration must specify the exact same accessibility modifiers, type, and name as the inherited property.</span></span> <span data-ttu-id="6f0ea-1151">如果继承的属性具有单个访问器 （例如，如果该属性是只读或只写），则重写属性必须只包含该访问器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1151">If the inherited property has only a single accessor (i.e., if the inherited property is read-only or write-only), the overriding property must include only that accessor.</span></span> <span data-ttu-id="6f0ea-1152">如果继承的属性包含这两个访问器 （即，如果该属性是读写），则重写属性可以包含单个访问器或这两个访问器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1152">If the inherited property includes both accessors (i.e., if the inherited property is read-write), the overriding property can include either a single accessor or both accessors.</span></span>

<span data-ttu-id="6f0ea-1153">重写属性声明可能包括`sealed`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1153">An overriding property declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="6f0ea-1154">使用此修饰符可以防止进一步重写该属性派生的类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1154">Use of this modifier prevents a derived class from further overriding the property.</span></span> <span data-ttu-id="6f0ea-1155">密封属性的访问器也被密封。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1155">The accessors of a sealed property are also sealed.</span></span>

<span data-ttu-id="6f0ea-1156">声明和调用之间的差异除外语法、 虚拟、 密封的重写和抽象访问器的行为与虚拟、 密封的重写和抽象方法完全相同。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1156">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="6f0ea-1157">具体而言，这些规则中所述[虚拟方法](classes.md#virtual-methods)，[重写方法](classes.md#override-methods)，[密封方法](classes.md#sealed-methods)，并[抽象方法](classes.md#abstract-methods)应用像访问器是相应窗体的方法：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1157">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form:</span></span>

*  <span data-ttu-id="6f0ea-1158">一个`get`访问器对应于具有返回值的属性类型和包含的属性与相同的修饰符的无参数方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1158">A `get` accessor corresponds to a parameterless method with a return value of the property type and the same modifiers as the containing property.</span></span>
*  <span data-ttu-id="6f0ea-1159">一个`set`访问器对应于具有单个值的属性类型参数的方法`void`返回类型，以及与包含属性相同的修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1159">A `set` accessor corresponds to a method with a single value parameter of the property type, a `void` return type, and the same modifiers as the containing property.</span></span>

<span data-ttu-id="6f0ea-1160">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1160">In the example</span></span>
```csharp
abstract class A
{
    int y;

    public virtual int X {
        get { return 0; }
    }

    public virtual int Y {
        get { return y; }
        set { y = value; }
    }

    public abstract int Z { get; set; }
}
```
<span data-ttu-id="6f0ea-1161">`X` 是虚拟的只读属性，`Y`是虚拟的读写属性，并`Z`是抽象的读写属性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1161">`X` is a virtual read-only property, `Y` is a virtual read-write property, and `Z` is an abstract read-write property.</span></span> <span data-ttu-id="6f0ea-1162">因为`Z`是抽象的包含类`A`也必须声明为抽象。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1162">Because `Z` is abstract, the containing class `A` must also be declared abstract.</span></span>

<span data-ttu-id="6f0ea-1163">从派生的类`A`下面显示了：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1163">A class that derives from `A` is show below:</span></span>
```csharp
class B: A
{
    int z;

    public override int X {
        get { return base.X + 1; }
    }

    public override int Y {
        set { base.Y = value < 0? 0: value; }
    }

    public override int Z {
        get { return z; }
        set { z = value; }
    }
}
```

<span data-ttu-id="6f0ea-1164">此处的声明`X`， `Y`，和`Z`将重写属性声明。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1164">Here, the declarations of `X`, `Y`, and `Z` are overriding property declarations.</span></span> <span data-ttu-id="6f0ea-1165">每个属性声明完全匹配可访问性修饰符、 类型和相应的继承属性的名称。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1165">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="6f0ea-1166">`get`访问器的`X`并`set`访问器`Y`使用`base`关键字访问继承的访问器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1166">The `get` accessor of `X` and the `set` accessor of `Y` use the `base` keyword to access the inherited accessors.</span></span> <span data-ttu-id="6f0ea-1167">声明`Z`重写这两个抽象访问器，因此，没有在抽象的函数成员`B`，和`B`允许为非抽象类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1167">The declaration of `Z` overrides both abstract accessors—thus, there are no outstanding abstract function members in `B`, and `B` is permitted to be a non-abstract class.</span></span>

<span data-ttu-id="6f0ea-1168">当某个属性声明为`override`，任何重写的访问器必须可供重写代码。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1168">When a property is declared as an `override`, any overridden accessors must be accessible to the overriding code.</span></span> <span data-ttu-id="6f0ea-1169">此外，声明可访问性的属性或索引器本身，和的访问器，必须与匹配的重写的成员和访问器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1169">In addition, the declared accessibility of both the property or indexer itself, and of the accessors, must match that of the overridden member and accessors.</span></span> <span data-ttu-id="6f0ea-1170">例如：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1170">For example:</span></span>
```csharp
public class B
{
    public virtual int P {
        protected set {...}
        get {...}
    }
}

public class D: B
{
    public override int P {
        protected set {...}            // Must specify protected here
        get {...}                      // Must not have a modifier here
    }
}
```

## <a name="events"></a><span data-ttu-id="6f0ea-1171">事件</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1171">Events</span></span>

<span data-ttu-id="6f0ea-1172">***事件***是使对象或类能够提供通知的成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1172">An ***event*** is a member that enables an object or class to provide notifications.</span></span> <span data-ttu-id="6f0ea-1173">客户端可以通过提供附加事件的可执行代码***事件处理程序***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1173">Clients can attach executable code for events by supplying ***event handlers***.</span></span>

<span data-ttu-id="6f0ea-1174">使用声明事件*event_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1174">Events are declared using *event_declaration*s:</span></span>

```antlr
event_declaration
    : attributes? event_modifier* 'event' type variable_declarators ';'
    | attributes? event_modifier* 'event' type member_name '{' event_accessor_declarations '}'
    ;

event_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | event_modifier_unsafe
    ;

event_accessor_declarations
    : add_accessor_declaration remove_accessor_declaration
    | remove_accessor_declaration add_accessor_declaration
    ;

add_accessor_declaration
    : attributes? 'add' block
    ;

remove_accessor_declaration
    : attributes? 'remove' block
    ;
```

<span data-ttu-id="6f0ea-1175">*Event_declaration*可能包括一套*特性*([特性](attributes.md)) 和四种访问修饰符的有效组合 ([访问修饰符](classes.md#access-modifiers))，则`new`([的新修饰符](classes.md#the-new-modifier))， `static` ([静态和实例方法](classes.md#static-and-instance-methods))， `virtual` ([虚拟方法](classes.md#virtual-methods))， `override` ([重写方法](classes.md#override-methods))， `sealed` ([密封方法](classes.md#sealed-methods))， `abstract` ([抽象方法](classes.md#abstract-methods))，并`extern` ([外部方法](classes.md#external-methods)) 修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1175">An *event_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="6f0ea-1176">事件声明可能会有所与方法声明相同的规则 ([方法](classes.md#methods)) 关于修饰符的有效组合。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1176">Event declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="6f0ea-1177">*类型*事件的声明必须*delegate_type* ([引用类型](types.md#reference-types))，并且*delegate_type*必须至少为事件本身的可访问性 ([可访问性约束](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1177">The *type* of an event declaration must be a *delegate_type* ([Reference types](types.md#reference-types)), and that *delegate_type* must be at least as accessible as the event itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6f0ea-1178">事件声明可能包括*event_accessor_declarations*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1178">An event declaration may include *event_accessor_declarations*.</span></span> <span data-ttu-id="6f0ea-1179">但是，如果不是，请为非 extern 非抽象事件，编译器会自动提供它们 ([类似字段的事件](classes.md#field-like-events)); 对于外部事件，从外部提供访问器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1179">However, if it does not, for non-extern, non-abstract events, the compiler supplies them automatically ([Field-like events](classes.md#field-like-events)); for extern events, the accessors are provided externally.</span></span>

<span data-ttu-id="6f0ea-1180">事件声明省略*event_accessor_declarations*定义了一个或多个事件，每个*variable_declarator*s。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1180">An event declaration that omits *event_accessor_declarations* defines one or more events—one for each of the *variable_declarator*s.</span></span> <span data-ttu-id="6f0ea-1181">特性和修饰符应用于所有通过此类声明的成员*event_declaration*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1181">The attributes and modifiers apply to all of the members declared by such an *event_declaration*.</span></span>

<span data-ttu-id="6f0ea-1182">它是编译时错误*event_declaration*包含这两`abstract`修饰符，大括号分隔*event_accessor_declarations*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1182">It is a compile-time error for an *event_declaration* to include both the `abstract` modifier and brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="6f0ea-1183">当事件声明包括`extern`修饰符，该事件则称***外部事件***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1183">When an event declaration includes an `extern` modifier, the event is said to be an ***external event***.</span></span> <span data-ttu-id="6f0ea-1184">因为外部事件声明不提供任何实际的实现，则返回错误，以便同时包含`extern`修饰符并*event_accessor_declarations*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1184">Because an external event declaration provides no actual implementation, it is an error for it to include both the `extern` modifier and *event_accessor_declarations*.</span></span>

<span data-ttu-id="6f0ea-1185">它是编译时错误*variable_declarator*的使用的事件声明`abstract`或`external`修饰符以包括*variable_initializer*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1185">It is a compile-time error for a *variable_declarator* of an event declaration with an `abstract` or `external` modifier to include a *variable_initializer*.</span></span>

<span data-ttu-id="6f0ea-1186">事件可以用作左操作数的`+=`并`-=`运算符 ([事件分配](expressions.md#event-assignment))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1186">An event can be used as the left-hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="6f0ea-1187">这些运算符的使用分别，附加到事件处理程序或删除事件处理程序从事件，以及事件的访问修饰符控制在其中执行此类操作的上下文。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1187">These operators are used, respectively, to attach event handlers to or to remove event handlers from an event, and the access modifiers of the event control the contexts in which such operations are permitted.</span></span>

<span data-ttu-id="6f0ea-1188">由于`+=`和`-=`是之外声明的事件、 外部代码的类型的事件允许的唯一操作可以添加和移除处理程序的事件，但不能在另一种方法获取或修改事件的基础列表处理程序。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1188">Since `+=` and `-=` are the only operations that are permitted on an event outside the type that declares the event, external code can add and remove handlers for an event, but cannot in any other way obtain or modify the underlying list of event handlers.</span></span>

<span data-ttu-id="6f0ea-1189">在窗体的操作中`x += y`或`x -= y`，当`x`是一个事件，该引用包含的声明的类型的外部发生`x`，该操作的结果具有类型`void`（而不是类型`x`，值为`x`赋值后)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1189">In an operation of the form `x += y` or `x -= y`, when `x` is an event and the reference takes place outside the type that contains the declaration of `x`, the result of the operation has type `void` (as opposed to having the type of `x`, with the value of `x` after the assignment).</span></span> <span data-ttu-id="6f0ea-1190">此规则禁止从间接检查事件的基础委托的外部代码。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1190">This rule prohibits external code from indirectly examining the underlying delegate of an event.</span></span>

<span data-ttu-id="6f0ea-1191">下面的示例演示如何将事件处理程序附加到的实例`Button`类：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1191">The following example shows how event handlers are attached to instances of the `Button` class:</span></span>
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;
}

public class LoginDialog: Form
{
    Button OkButton;
    Button CancelButton;

    public LoginDialog() {
        OkButton = new Button(...);
        OkButton.Click += new EventHandler(OkButtonClick);
        CancelButton = new Button(...);
        CancelButton.Click += new EventHandler(CancelButtonClick);
    }

    void OkButtonClick(object sender, EventArgs e) {
        // Handle OkButton.Click event
    }

    void CancelButtonClick(object sender, EventArgs e) {
        // Handle CancelButton.Click event
    }
}
```

<span data-ttu-id="6f0ea-1192">在这里，`LoginDialog`实例构造函数创建两个`Button`实例，并将附加到事件处理程序`Click`事件。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1192">Here, the `LoginDialog` instance constructor creates two `Button` instances and attaches event handlers to the `Click` events.</span></span>

### <a name="field-like-events"></a><span data-ttu-id="6f0ea-1193">类似字段的事件</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1193">Field-like events</span></span>

<span data-ttu-id="6f0ea-1194">中的类或结构包含一个事件声明的程序文本，可以像字段一样使用某些事件。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1194">Within the program text of the class or struct that contains the declaration of an event, certain events can be used like fields.</span></span> <span data-ttu-id="6f0ea-1195">若要使用这种方式，事件不能`abstract`或`extern`，并且必须显式包括*event_accessor_declarations*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1195">To be used in this way, an event must not be `abstract` or `extern`, and must not explicitly include *event_accessor_declarations*.</span></span> <span data-ttu-id="6f0ea-1196">可以在任何允许使用字段的上下文中使用此类事件。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1196">Such an event can be used in any context that permits a field.</span></span> <span data-ttu-id="6f0ea-1197">该字段将包含一个委托 ([委托](delegates.md)) 这是指已添加到该事件的事件处理程序的列表。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1197">The field contains a delegate ([Delegates](delegates.md)) which refers to the list of event handlers that have been added to the event.</span></span> <span data-ttu-id="6f0ea-1198">如果尚未添加任何事件处理程序，该字段包含`null`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1198">If no event handlers have been added, the field contains `null`.</span></span>

<span data-ttu-id="6f0ea-1199">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1199">In the example</span></span>
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;

    protected void OnClick(EventArgs e) {
        if (Click != null) Click(this, e);
    }

    public void Reset() {
        Click = null;
    }
}
```
<span data-ttu-id="6f0ea-1200">`Click` 用作字段内`Button`类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1200">`Click` is used as a field within the `Button` class.</span></span> <span data-ttu-id="6f0ea-1201">如示例所示，该字段可以检查、 修改和使用委托调用表达式中。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1201">As the example demonstrates, the field can be examined, modified, and used in delegate invocation expressions.</span></span> <span data-ttu-id="6f0ea-1202">`OnClick`中的方法`Button`类"引发"`Click`事件。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1202">The `OnClick` method in the `Button` class "raises" the `Click` event.</span></span> <span data-ttu-id="6f0ea-1203">引发事件的概念恰恰等同于调用由事件表示的委托，因此，没有用于引发事件的特殊语言构造。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1203">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span> <span data-ttu-id="6f0ea-1204">请注意检查，以确保委托为非 null 的前面有委托调用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1204">Note that the delegate invocation is preceded by a check that ensures the delegate is non-null.</span></span>

<span data-ttu-id="6f0ea-1205">声明的外部`Button`类，`Click`成员只能上的左侧使用`+=`和`-=`运算符，如下所示</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1205">Outside the declaration of the `Button` class, the `Click` member can only be used on the left-hand side of the `+=` and `-=` operators, as in</span></span>
```csharp
b.Click += new EventHandler(...);
```
<span data-ttu-id="6f0ea-1206">这将委托附加到的调用列表`Click`事件，并</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1206">which appends a delegate to the invocation list of the `Click` event, and</span></span>
```csharp
b.Click -= new EventHandler(...);
```
<span data-ttu-id="6f0ea-1207">它从调用列表中删除委托`Click`事件。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1207">which removes a delegate from the invocation list of the `Click` event.</span></span>

<span data-ttu-id="6f0ea-1208">当编译类似字段的事件时，编译器将自动创建一个存储区来存放委托，并创建事件访问器的添加或删除事件处理程序委托字段。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1208">When compiling a field-like event, the compiler automatically creates storage to hold the delegate, and creates accessors for the event that add or remove event handlers to the delegate field.</span></span> <span data-ttu-id="6f0ea-1209">添加和删除操作都是线程安全的并可能 （但都不需要） 将完成的时持有锁 ([lock 语句](statements.md#the-lock-statement)) 对实例事件时，包含对象或类型对象 ([匿名对象创建表达式](expressions.md#anonymous-object-creation-expressions)) 的静态事件。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1209">The addition and removal operations are thread safe, and may (but are not required to) be done while holding the lock ([The lock statement](statements.md#the-lock-statement)) on the containing object for an instance event, or the type object ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) for a static event.</span></span>

<span data-ttu-id="6f0ea-1210">因此，下列窗体的实例事件声明：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1210">Thus, an instance event declaration of the form:</span></span>
```csharp
class X
{
    public event D Ev;
}
```
<span data-ttu-id="6f0ea-1211">将编译为如下语句：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1211">will be compiled to something equivalent to:</span></span>
```csharp
class X
{
    private D __Ev;  // field to hold the delegate

    public event D Ev {
        add {
            /* add the delegate in a thread safe way */
        }

        remove {
            /* remove the delegate in a thread safe way */
        }
    }
}
```
<span data-ttu-id="6f0ea-1212">在类中`X`，对引用`Ev`的左侧`+=`和`-=`运算符导致 add 和 remove 访问器要调用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1212">Within the class `X`, references to `Ev` on the left-hand side of the `+=` and `-=` operators cause the add and remove accessors to be invoked.</span></span> <span data-ttu-id="6f0ea-1213">对所有其他引用`Ev`进行编译以引用该隐藏的字段`__Ev`改为 ([成员访问](expressions.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1213">All other references to `Ev` are compiled to reference the hidden field `__Ev` instead ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="6f0ea-1214">名称"`__Ev`"是任意; 隐藏的字段可以在所有具有任何名称或无名称。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1214">The name "`__Ev`" is arbitrary; the hidden field could have any name or no name at all.</span></span>

### <a name="event-accessors"></a><span data-ttu-id="6f0ea-1215">事件访问器</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1215">Event accessors</span></span>

<span data-ttu-id="6f0ea-1216">事件声明通常省略*event_accessor_declarations*，如下所示：`Button`上面示例中。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1216">Event declarations typically omit *event_accessor_declarations*, as in the `Button` example above.</span></span> <span data-ttu-id="6f0ea-1217">执行此操作的一种情况时，需要在其中每个事件的一个字段的存储成本是不可接受的情况。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1217">One situation for doing so involves the case in which the storage cost of one field per event is not acceptable.</span></span> <span data-ttu-id="6f0ea-1218">在这种情况下，一个类可以包含*event_accessor_declarations*和使用私有机制来存储事件处理程序的列表。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1218">In such cases, a class can include *event_accessor_declarations* and use a private mechanism for storing the list of event handlers.</span></span>

<span data-ttu-id="6f0ea-1219">*Event_accessor_declarations*事件指定与添加和移除事件处理程序相关联的可执行语句。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1219">The *event_accessor_declarations* of an event specify the executable statements associated with adding and removing event handlers.</span></span>

<span data-ttu-id="6f0ea-1220">访问器声明组成*add_accessor_declaration*和一个*remove_accessor_declaration*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1220">The accessor declarations consist of an *add_accessor_declaration* and a *remove_accessor_declaration*.</span></span> <span data-ttu-id="6f0ea-1221">每个访问器声明包含令牌`add`或`remove`跟*块*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1221">Each accessor declaration consists of the token `add` or `remove` followed by a *block*.</span></span> <span data-ttu-id="6f0ea-1222">*块*与关联*add_accessor_declaration*指定时添加事件处理程序，要执行的语句和*块*与相关联*remove_accessor_declaration*指定要删除的事件处理程序时执行的语句。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1222">The *block* associated with an *add_accessor_declaration* specifies the statements to execute when an event handler is added, and the *block* associated with a *remove_accessor_declaration* specifies the statements to execute when an event handler is removed.</span></span>

<span data-ttu-id="6f0ea-1223">每个*add_accessor_declaration*并*remove_accessor_declaration*对应于具有事件类型的单值参数的方法和一个`void`返回类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1223">Each *add_accessor_declaration* and *remove_accessor_declaration* corresponds to a method with a single value parameter of the event type and a `void` return type.</span></span> <span data-ttu-id="6f0ea-1224">事件访问器的隐式参数名为`value`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1224">The implicit parameter of an event accessor is named `value`.</span></span> <span data-ttu-id="6f0ea-1225">当事件分配中使用事件时，使用相应的事件访问器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1225">When an event is used in an event assignment, the appropriate event accessor is used.</span></span> <span data-ttu-id="6f0ea-1226">具体而言，如果赋值运算符是`+=`使用的 add 访问器，然后赋值运算符是`-=`则使用 remove 访问器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1226">Specifically, if the assignment operator is `+=` then the add accessor is used, and if the assignment operator is `-=` then the remove accessor is used.</span></span> <span data-ttu-id="6f0ea-1227">在任一情况下，赋值运算符的右侧操作数用作事件访问器的参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1227">In either case, the right-hand operand of the assignment operator is used as the argument to the event accessor.</span></span> <span data-ttu-id="6f0ea-1228">块*add_accessor_declaration*或*remove_accessor_declaration*必须符合有关规则`void`中所述方法[方法体](classes.md#method-body)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1228">The block of an *add_accessor_declaration* or a *remove_accessor_declaration* must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="6f0ea-1229">具体而言，`return`不允许此类块中的语句指定的表达式。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1229">In particular, `return` statements in such a block are not permitted to specify an expression.</span></span>

<span data-ttu-id="6f0ea-1230">由于事件访问器隐式具有一个名为参数`value`，它是一个编译时错误的事件访问器具有该名称中声明本地变量或常量。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1230">Since an event accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declared in an event accessor to have that name.</span></span>

<span data-ttu-id="6f0ea-1231">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1231">In the example</span></span>
```csharp
class Control: Component
{
    // Unique keys for events
    static readonly object mouseDownEventKey = new object();
    static readonly object mouseUpEventKey = new object();

    // Return event handler associated with key
    protected Delegate GetEventHandler(object key) {...}

    // Add event handler associated with key
    protected void AddEventHandler(object key, Delegate handler) {...}

    // Remove event handler associated with key
    protected void RemoveEventHandler(object key, Delegate handler) {...}

    // MouseDown event
    public event MouseEventHandler MouseDown {
        add { AddEventHandler(mouseDownEventKey, value); }
        remove { RemoveEventHandler(mouseDownEventKey, value); }
    }

    // MouseUp event
    public event MouseEventHandler MouseUp {
        add { AddEventHandler(mouseUpEventKey, value); }
        remove { RemoveEventHandler(mouseUpEventKey, value); }
    }

    // Invoke the MouseUp event
    protected void OnMouseUp(MouseEventArgs args) {
        MouseEventHandler handler; 
        handler = (MouseEventHandler)GetEventHandler(mouseUpEventKey);
        if (handler != null)
            handler(this, args);
    }
}
```
<span data-ttu-id="6f0ea-1232">`Control`类实现事件的内部存储机制。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1232">the `Control` class implements an internal storage mechanism for events.</span></span> <span data-ttu-id="6f0ea-1233">`AddEventHandler`方法将委托值与键相关联`GetEventHandler`方法返回当前与键关联的委托和`RemoveEventHandler`方法将委托移除与指定的事件的事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1233">The `AddEventHandler` method associates a delegate value with a key, the `GetEventHandler` method returns the delegate currently associated with a key, and the `RemoveEventHandler` method removes a delegate as an event handler for the specified event.</span></span> <span data-ttu-id="6f0ea-1234">据估计，却设计的基础存储机制为： 没有为关联的成本`null`委托值的密钥，并因此在未处理的事件使用任何存储。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1234">Presumably, the underlying storage mechanism is designed such that there is no cost for associating a `null` delegate value with a key, and thus unhandled events consume no storage.</span></span>

### <a name="static-and-instance-events"></a><span data-ttu-id="6f0ea-1235">静态和实例事件</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1235">Static and instance events</span></span>

<span data-ttu-id="6f0ea-1236">当事件声明包括`static`修饰符，该事件则称***静态事件***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1236">When an event declaration includes a `static` modifier, the event is said to be a ***static event***.</span></span> <span data-ttu-id="6f0ea-1237">如果未`static`修饰符存在，该事件则称***实例事件***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1237">When no `static` modifier is present, the event is said to be an ***instance event***.</span></span>

<span data-ttu-id="6f0ea-1238">静态事件不与特定实例相关联，它是指导致编译时错误`this`的静态事件访问器中。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1238">A static event is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static event.</span></span>

<span data-ttu-id="6f0ea-1239">实例事件与给定类的实例相关联，并且可作为访问此实例`this`([此访问权限](expressions.md#this-access)) 中的该事件访问器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1239">An instance event is associated with a given instance of a class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that event.</span></span>

<span data-ttu-id="6f0ea-1240">事件中的引用时*member_access* ([成员访问](expressions.md#member-access)) 的窗体`E.M`，如果`M`是静态事件，`E`必须表示一个类型，包含`M`，并且如果`M`是实例事件，则 E 必须表示一个类型，其中包含的实例`M`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1240">When an event is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static event, `E` must denote a type containing `M`, and if `M` is an instance event, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="6f0ea-1241">静态之间的差异和实例成员进行讨论中进一步[静态和实例成员](classes.md#static-and-instance-members)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1241">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a><span data-ttu-id="6f0ea-1242">虚拟，密封的重写方法和抽象事件访问器</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1242">Virtual, sealed, override, and abstract event accessors</span></span>

<span data-ttu-id="6f0ea-1243">一个`virtual`事件声明指定该事件的访问器是虚拟。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1243">A `virtual` event declaration specifies that the accessors of that event are virtual.</span></span> <span data-ttu-id="6f0ea-1244">`virtual`修饰符将应用于事件的两个访问器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1244">The `virtual` modifier applies to both accessors of an event.</span></span>

<span data-ttu-id="6f0ea-1245">`abstract`事件声明指定事件的访问器是虚拟的但不提供访问器的实际实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1245">An `abstract` event declaration specifies that the accessors of the event are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="6f0ea-1246">相反，非抽象派生的类所需的重写事件提供自己的访问器的实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1246">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the event.</span></span> <span data-ttu-id="6f0ea-1247">由于抽象事件声明不提供任何实际的实现，因此它不能提供大括号分隔*event_accessor_declarations*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1247">Because an abstract event declaration provides no actual implementation, it cannot provide brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="6f0ea-1248">事件声明同时包含`abstract`和`override`修饰符指定该事件是抽象的重写基事件。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1248">An event declaration that includes both the `abstract` and `override` modifiers specifies that the event is abstract and overrides a base event.</span></span> <span data-ttu-id="6f0ea-1249">此类事件的访问器也是抽象的。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1249">The accessors of such an event are also abstract.</span></span>

<span data-ttu-id="6f0ea-1250">抽象类中才允许抽象事件声明 ([抽象类](classes.md#abstract-classes))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1250">Abstract event declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="6f0ea-1251">可以通过包含指定的事件声明派生类中替代继承虚拟事件访问器`override`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1251">The accessors of an inherited virtual event can be overridden in a derived class by including an event declaration that specifies an `override` modifier.</span></span> <span data-ttu-id="6f0ea-1252">这称为***重写事件声明***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1252">This is known as an ***overriding event declaration***.</span></span> <span data-ttu-id="6f0ea-1253">重写事件声明不声明新的事件。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1253">An overriding event declaration does not declare a new event.</span></span> <span data-ttu-id="6f0ea-1254">相反，它只是专用化的现有虚拟事件访问器的实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1254">Instead, it simply specializes the implementations of the accessors of an existing virtual event.</span></span>

<span data-ttu-id="6f0ea-1255">重写事件声明必须指定为重写事件完全相同的可访问性修饰符、 类型和名称。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1255">An overriding event declaration must specify the exact same accessibility modifiers, type, and name as the overridden event.</span></span>

<span data-ttu-id="6f0ea-1256">重写事件声明可能包括`sealed`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1256">An overriding event declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="6f0ea-1257">使用此修饰符可防止进一步重写事件派生的类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1257">Use of this modifier prevents a derived class from further overriding the event.</span></span> <span data-ttu-id="6f0ea-1258">密封的事件的访问器也被密封。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1258">The accessors of a sealed event are also sealed.</span></span>

<span data-ttu-id="6f0ea-1259">它是重写事件声明中包含的编译时错误`new`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1259">It is a compile-time error for an overriding event declaration to include a `new` modifier.</span></span>

<span data-ttu-id="6f0ea-1260">声明和调用之间的差异除外语法、 虚拟、 密封的重写和抽象访问器的行为与虚拟、 密封的重写和抽象方法完全相同。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1260">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="6f0ea-1261">具体而言，这些规则中所述[虚拟方法](classes.md#virtual-methods)，[重写方法](classes.md#override-methods)，[密封方法](classes.md#sealed-methods)，并[抽象方法](classes.md#abstract-methods)应用像访问器是相应窗体的方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1261">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form.</span></span> <span data-ttu-id="6f0ea-1262">每个访问器对应于具有事件类型的单值参数的方法`void`返回类型，以及与包含事件相同的修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1262">Each accessor corresponds to a method with a single value parameter of the event type, a `void` return type, and the same modifiers as the containing event.</span></span>

## <a name="indexers"></a><span data-ttu-id="6f0ea-1263">索引器</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1263">Indexers</span></span>

<span data-ttu-id="6f0ea-1264">***索引器***是使对象可以为数组相同的方式进行索引的成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1264">An ***indexer*** is a member that enables an object to be indexed in the same way as an array.</span></span> <span data-ttu-id="6f0ea-1265">使用声明索引器*indexer_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1265">Indexers are declared using *indexer_declaration*s:</span></span>

```antlr
indexer_declaration
    : attributes? indexer_modifier* indexer_declarator indexer_body
    ;

indexer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | indexer_modifier_unsafe
    ;

indexer_declarator
    : type 'this' '[' formal_parameter_list ']'
    | type interface_type '.' 'this' '[' formal_parameter_list ']'
    ;

indexer_body
    : '{' accessor_declarations '}' 
    | '=>' expression ';'
    ;
```

<span data-ttu-id="6f0ea-1266">*Indexer_declaration*可能包括一套*特性*([特性](attributes.md)) 和四种访问修饰符的有效组合 ([访问修饰符](classes.md#access-modifiers))，则`new`([的新修饰符](classes.md#the-new-modifier))， `virtual` ([虚拟方法](classes.md#virtual-methods))， `override` ([重写方法](classes.md#override-methods))， `sealed` ([密封方法](classes.md#sealed-methods))， `abstract` ([抽象方法](classes.md#abstract-methods))，以及`extern`([外部方法](classes.md#external-methods)) 修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1266">An *indexer_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="6f0ea-1267">索引器声明可能会有所与方法声明相同的规则 ([方法](classes.md#methods)) 方面的修饰符的有效组合，有一个例外是，静态修饰符不允许在索引器声明。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1267">Indexer declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers, with the one exception being that the static modifier is not permitted on an indexer declaration.</span></span>

<span data-ttu-id="6f0ea-1268">修饰符`virtual`， `override`，和`abstract`是一种情况下除外互相排斥。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1268">The modifiers `virtual`, `override`, and `abstract` are mutually exclusive except in one case.</span></span> <span data-ttu-id="6f0ea-1269">`abstract`和`override`修饰符可能以便抽象索引器可以重写虚拟一起使用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1269">The `abstract` and `override` modifiers may be used together so that an abstract indexer can override a virtual one.</span></span>

<span data-ttu-id="6f0ea-1270">*类型*声明索引器的指定索引器声明引入的元素类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1270">The *type* of an indexer declaration specifies the element type of the indexer introduced by the declaration.</span></span> <span data-ttu-id="6f0ea-1271">索引器是显式接口成员的实现，除非*类型*跟关键字`this`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1271">Unless the indexer is an explicit interface member implementation, the *type* is followed by the keyword `this`.</span></span> <span data-ttu-id="6f0ea-1272">有关显式接口成员实现，*类型*后跟*interface_type*、"`.`"，和关键字`this`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1272">For an explicit interface member implementation, the *type* is followed by an *interface_type*, a "`.`", and the keyword `this`.</span></span> <span data-ttu-id="6f0ea-1273">与其他成员不同索引器不具有用户定义的名称。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1273">Unlike other members, indexers do not have user-defined names.</span></span>

<span data-ttu-id="6f0ea-1274">*Formal_parameter_list*指定的索引器的参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1274">The *formal_parameter_list* specifies the parameters of the indexer.</span></span> <span data-ttu-id="6f0ea-1275">索引器的形参列表对应于一种方法 ([方法参数](classes.md#method-parameters))，但必须指定至少一个参数，并且`ref`和`out`不允许参数修饰符.</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1275">The formal parameter list of an indexer corresponds to that of a method ([Method parameters](classes.md#method-parameters)), except that at least one parameter must be specified, and that the `ref` and `out` parameter modifiers are not permitted.</span></span>

<span data-ttu-id="6f0ea-1276">*类型*的索引器和每种类型中引用*formal_parameter_list*必须至少与索引器本身相同的可访问性 ([可访问性约束](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1276">The *type* of an indexer and each of the types referenced in the *formal_parameter_list* must be at least as accessible as the indexer itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6f0ea-1277">*Indexer_body*可能是组成***访问器正文***或***表达式主体***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1277">An *indexer_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="6f0ea-1278">在访问器正文中， *accessor_declarations*，必须在用"`{`"和"`}`"令牌，声明访问器 ([访问器](classes.md#accessors)) 的属性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1278">In an accessor body, *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="6f0ea-1279">访问器指定与读取和写入属性相关联的可执行语句。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1279">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="6f0ea-1280">包含的表达式主体"`=>`"表达式后跟`E`分号，完全等效于在语句体`{ get { return E; } }`，并因此仅可指定仅定义了 getter 的索引器的 getter 的结果由单个表达式给定。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1280">An expression body consisting of "`=>`" followed by an expression `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only indexers where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="6f0ea-1281">即使访问索引器元素的语法是相同的数组元素，索引器元素是未归类为变量。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1281">Even though the syntax for accessing an indexer element is the same as that for an array element, an indexer element is not classified as a variable.</span></span> <span data-ttu-id="6f0ea-1282">因此，不能作为索引器元素传递`ref`或`out`参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1282">Thus, it is not possible to pass an indexer element as a `ref` or `out` argument.</span></span>

<span data-ttu-id="6f0ea-1283">索引器的形参列表定义签名 ([签名和超载](basic-concepts.md#signatures-and-overloading)) 的索引器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1283">The formal parameter list of an indexer defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the indexer.</span></span> <span data-ttu-id="6f0ea-1284">具体而言，索引器的签名组成的数量和其形参的类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1284">Specifically, the signature of an indexer consists of the number and types of its formal parameters.</span></span> <span data-ttu-id="6f0ea-1285">元素类型和正式参数的名称不是签名的索引器的一部分。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1285">The element type and names of the formal parameters are not part of an indexer's signature.</span></span>

<span data-ttu-id="6f0ea-1286">索引器的签名必须不同于在同一类中声明的所有其他索引器的签名。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1286">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>

<span data-ttu-id="6f0ea-1287">索引器和属性的概念，非常类似，但在以下方面有所不同：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1287">Indexers and properties are very similar in concept, but differ in the following ways:</span></span>

*  <span data-ttu-id="6f0ea-1288">属性是由其名称标识，而索引器由其签名。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1288">A property is identified by its name, whereas an indexer is identified by its signature.</span></span>
*  <span data-ttu-id="6f0ea-1289">通过访问属性*simple_name* ([简单名称](expressions.md#simple-names)) 或*member_access* ([成员访问](expressions.md#member-access))，而索引器通过访问元素*element_access* ([索引器访问](expressions.md#indexer-access))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1289">A property is accessed through a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)), whereas an indexer element is accessed through an *element_access* ([Indexer access](expressions.md#indexer-access)).</span></span>
*  <span data-ttu-id="6f0ea-1290">属性可以是`static`成员，而索引器始终是一个实例成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1290">A property can be a `static` member, whereas an indexer is always an instance member.</span></span>
*  <span data-ttu-id="6f0ea-1291">一个`get`的属性访问器对应于不带任何参数的方法而`get`索引器访问器对应于具有索引器相同的形参列表的方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1291">A `get` accessor of a property corresponds to a method with no parameters, whereas a `get` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer.</span></span>
*  <span data-ttu-id="6f0ea-1292">一个`set`的属性访问器对应于一种方法具有一个名为的单个参数`value`，而`set`索引器访问器对应于具有索引器，以及一个附加参数相同的形参列表的方法名为`value`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1292">A `set` accessor of a property corresponds to a method with a single parameter named `value`, whereas a `set` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer, plus an additional parameter named `value`.</span></span>
*  <span data-ttu-id="6f0ea-1293">它是索引器访问器声明索引器参数与同名的局部变量的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1293">It is a compile-time error for an indexer accessor to declare a local variable with the same name as an indexer parameter.</span></span>
*  <span data-ttu-id="6f0ea-1294">在重写属性声明中，使用语法访问继承的属性`base.P`，其中`P`是属性名称。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1294">In an overriding property declaration, the inherited property is accessed using the syntax `base.P`, where `P` is the property name.</span></span> <span data-ttu-id="6f0ea-1295">在重写索引器声明中，使用语法访问继承的索引器`base[E]`，其中`E`是表达式的以逗号分隔列表。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1295">In an overriding indexer declaration, the inherited indexer is accessed using the syntax `base[E]`, where `E` is a comma separated list of expressions.</span></span>
*  <span data-ttu-id="6f0ea-1296">不存在"自动实现索引器"的概念。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1296">There is no concept of an "automatically implemented indexer".</span></span> <span data-ttu-id="6f0ea-1297">它是错误的非抽象、 非外部索引器中的，使用分号访问器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1297">It is an error to have a non-abstract, non-external indexer with semicolon accessors.</span></span>

<span data-ttu-id="6f0ea-1298">除了这些差异中定义的所有规则[访问器](classes.md#accessors)并[自动实现的属性](classes.md#automatically-implemented-properties)应用于索引器访问器以及属性访问器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1298">Aside from these differences, all rules defined in [Accessors](classes.md#accessors) and [Automatically implemented properties](classes.md#automatically-implemented-properties) apply to indexer accessors as well as to property accessors.</span></span>

<span data-ttu-id="6f0ea-1299">当索引器声明包括`extern`修饰符，索引器称为***外部索引器***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1299">When an indexer declaration includes an `extern` modifier, the indexer is said to be an ***external indexer***.</span></span> <span data-ttu-id="6f0ea-1300">因为外部索引器声明不提供任何实际的实现，每个其*accessor_declarations*包含一个分号。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1300">Because an external indexer declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

<span data-ttu-id="6f0ea-1301">下面的示例声明`BitArray`类，该类实现索引器，以便访问中的位数组的单个位。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1301">The example below declares a `BitArray` class that implements an indexer for accessing the individual bits in the bit array.</span></span>
```csharp
using System;

class BitArray
{
    int[] bits;
    int length;

    public BitArray(int length) {
        if (length < 0) throw new ArgumentException();
        bits = new int[((length - 1) >> 5) + 1];
        this.length = length;
    }

    public int Length {
        get { return length; }
    }

    public bool this[int index] {
        get {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            return (bits[index >> 5] & 1 << index) != 0;
        }
        set {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            if (value) {
                bits[index >> 5] |= 1 << index;
            }
            else {
                bits[index >> 5] &= ~(1 << index);
            }
        }
    }
}
```

<span data-ttu-id="6f0ea-1302">实例`BitArray`类会占用内存远少于相应`bool[]`（因为前者的每个值所占的仅有一位而不是后者的一个字节），但与相同的操作，它允许`bool[]`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1302">An instance of the `BitArray` class consumes substantially less memory than a corresponding `bool[]` (since each value of the former occupies only one bit instead of the latter's one byte), but it permits the same operations as a `bool[]`.</span></span>

<span data-ttu-id="6f0ea-1303">以下`CountPrimes`类使用`BitArray`和传统的"埃拉托色"算法，来计算的 1 到给定的最大值之间的质数：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1303">The following `CountPrimes` class uses a `BitArray` and the classical "sieve" algorithm to compute the number of primes between 1 and a given maximum:</span></span>
```csharp
class CountPrimes
{
    static int Count(int max) {
        BitArray flags = new BitArray(max + 1);
        int count = 1;
        for (int i = 2; i <= max; i++) {
            if (!flags[i]) {
                for (int j = i * 2; j <= max; j += i) flags[j] = true;
                count++;
            }
        }
        return count;
    }

    static void Main(string[] args) {
        int max = int.Parse(args[0]);
        int count = Count(max);
        Console.WriteLine("Found {0} primes between 1 and {1}", count, max);
    }
}
```

<span data-ttu-id="6f0ea-1304">请注意，用于访问的元素的语法`BitArray`准确地说是相同`bool[]`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1304">Note that the syntax for accessing elements of the `BitArray` is precisely the same as for a `bool[]`.</span></span>

<span data-ttu-id="6f0ea-1305">下面的示例演示具有两个参数使用一个索引器的 26 \* 10 网格类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1305">The following example shows a 26 \* 10 grid class that has an indexer with two parameters.</span></span> <span data-ttu-id="6f0ea-1306">第一个参数必须是大写或小写字母 A-Z 范围内，第二个需是范围 0-9 内的整数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1306">The first parameter is required to be an upper- or lowercase letter in the range A-Z, and the second is required to be an integer in the range 0-9.</span></span>

```csharp
using System;

class Grid
{
    const int NumRows = 26;
    const int NumCols = 10;

    int[,] cells = new int[NumRows, NumCols];

    public int this[char c, int col] {
        get {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            return cells[c - 'A', col];
        }

        set {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            cells[c - 'A', col] = value;
        }
    }
}
```

### <a name="indexer-overloading"></a><span data-ttu-id="6f0ea-1307">索引器重载</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1307">Indexer overloading</span></span>

<span data-ttu-id="6f0ea-1308">索引器重载决策规则所述[类型推理](expressions.md#type-inference)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1308">The indexer overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="operators"></a><span data-ttu-id="6f0ea-1309">运算符</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1309">Operators</span></span>

<span data-ttu-id="6f0ea-1310">***运算符***是定义可应用于类的实例的表达式运算符的含义的成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1310">An ***operator*** is a member that defines the meaning of an expression operator that can be applied to instances of the class.</span></span> <span data-ttu-id="6f0ea-1311">使用声明运算符*operator_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1311">Operators are declared using *operator_declaration*s:</span></span>

```antlr
operator_declaration
    : attributes? operator_modifier+ operator_declarator operator_body
    ;

operator_modifier
    : 'public'
    | 'static'
    | 'extern'
    | operator_modifier_unsafe
    ;

operator_declarator
    : unary_operator_declarator
    | binary_operator_declarator
    | conversion_operator_declarator
    ;

unary_operator_declarator
    : type 'operator' overloadable_unary_operator '(' type identifier ')'
    ;

overloadable_unary_operator
    : '+' | '-' | '!' | '~' | '++' | '--' | 'true' | 'false'
    ;

binary_operator_declarator
    : type 'operator' overloadable_binary_operator '(' type identifier ',' type identifier ')'
    ;

overloadable_binary_operator
    : '+'   | '-'   | '*'   | '/'   | '%'   | '&'   | '|'   | '^'   | '<<'
    | 'right_shift' | '=='  | '!='  | '>'   | '<'   | '>='  | '<='
    ;

conversion_operator_declarator
    : 'implicit' 'operator' type '(' type identifier ')'
    | 'explicit' 'operator' type '(' type identifier ')'
    ;

operator_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

<span data-ttu-id="6f0ea-1312">有三个类别的可重载运算符： 一元运算符 ([一元运算符](classes.md#unary-operators))，二元运算符 ([二元运算符](classes.md#binary-operators))，和转换运算符 ([转换运算符](classes.md#conversion-operators)).</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1312">There are three categories of overloadable operators: Unary operators ([Unary operators](classes.md#unary-operators)), binary operators ([Binary operators](classes.md#binary-operators)), and conversion operators ([Conversion operators](classes.md#conversion-operators)).</span></span>

<span data-ttu-id="6f0ea-1313">*Operator_body*是一个分号***语句体***或***表达式主体***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1313">The *operator_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="6f0ea-1314">语句体组成*块*，它指定要执行时调用运算符的语句。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1314">A statement body consists of a *block*, which specifies the statements to execute when the operator is invoked.</span></span> <span data-ttu-id="6f0ea-1315">*块*必须符合有关返回值的规则中所述方法[方法主体](classes.md#method-body)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1315">The *block* must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="6f0ea-1316">表达式主体组成`=>`表达式和一个分号后, 跟，表示要执行时调用运算符的单个表达式。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1316">An expression body consists of `=>` followed by an expression and a semicolon, and denotes a single expression to perform when the operator is invoked.</span></span>

<span data-ttu-id="6f0ea-1317">有关`extern`运算符*operator_body*只需包含一个分号。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1317">For `extern` operators, the *operator_body* consists simply of a semicolon.</span></span> <span data-ttu-id="6f0ea-1318">对于所有其他运算符， *operator_body*块正文或正文为表达式。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1318">For all other operators, the *operator_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="6f0ea-1319">以下规则适用于所有运算符声明：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1319">The following rules apply to all operator declarations:</span></span>

*  <span data-ttu-id="6f0ea-1320">在运算符声明必须同时包含`public`和一个`static`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1320">An operator declaration must include both a `public` and a `static` modifier.</span></span>
*  <span data-ttu-id="6f0ea-1321">运算符的参数必须为值参数 ([值参数](variables.md#value-parameters))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1321">The parameter(s) of an operator must be value parameters ([Value parameters](variables.md#value-parameters)).</span></span> <span data-ttu-id="6f0ea-1322">它是在运算符声明中指定的编译时错误`ref`或`out`参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1322">It is a compile-time error for an operator declaration to specify `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="6f0ea-1323">运算符的签名 ([一元运算符](classes.md#unary-operators)，[二元运算符](classes.md#binary-operators)，[转换运算符](classes.md#conversion-operators)) 必须不同于声明中的所有其他运算符的签名同一个类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1323">The signature of an operator ([Unary operators](classes.md#unary-operators), [Binary operators](classes.md#binary-operators), [Conversion operators](classes.md#conversion-operators)) must differ from the signatures of all other operators declared in the same class.</span></span>
*  <span data-ttu-id="6f0ea-1324">在运算符声明中引用的所有类型都必须至少与运算符本身相同的可访问性 ([可访问性约束](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1324">All types referenced in an operator declaration must be at least as accessible as the operator itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>
*  <span data-ttu-id="6f0ea-1325">它是同一修饰符在运算符声明中多次出现的错误的。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1325">It is an error for the same modifier to appear multiple times in an operator declaration.</span></span>

<span data-ttu-id="6f0ea-1326">每个运算符类别都有其他限制，如以下各节中所述。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1326">Each operator category imposes additional restrictions, as described in the following sections.</span></span>

<span data-ttu-id="6f0ea-1327">与其他成员一样在基类中声明的运算符由派生类继承。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1327">Like other members, operators declared in a base class are inherited by derived classes.</span></span> <span data-ttu-id="6f0ea-1328">运算符声明始终需要的类或结构声明运算符参与运算符的签名，因为它不能在派生类中声明的运算符，若要隐藏基类中声明的运算符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1328">Because operator declarations always require the class or struct in which the operator is declared to participate in the signature of the operator, it is not possible for an operator declared in a derived class to hide an operator declared in a base class.</span></span> <span data-ttu-id="6f0ea-1329">因此，`new`修饰符永远不会是必需的并因此永远不会允许，在运算符声明中。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1329">Thus, the `new` modifier is never required, and therefore never permitted, in an operator declaration.</span></span>

<span data-ttu-id="6f0ea-1330">一元和二元运算符的其他信息可在[运算符](expressions.md#operators)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1330">Additional information on unary and binary operators can be found in [Operators](expressions.md#operators).</span></span>

<span data-ttu-id="6f0ea-1331">转换运算符的其他信息可在[用户定义的转换](conversions.md#user-defined-conversions)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1331">Additional information on conversion operators can be found in [User-defined conversions](conversions.md#user-defined-conversions).</span></span>

### <a name="unary-operators"></a><span data-ttu-id="6f0ea-1332">一元运算符</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1332">Unary operators</span></span>

<span data-ttu-id="6f0ea-1333">以下规则适用于一元运算符声明，其中`T`表示类或结构都包含在运算符声明的实例类型：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1333">The following rules apply to unary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="6f0ea-1334">一元`+`， `-`， `!`，或`~`运算符必须采用一个参数的类型`T`或`T?`和可以返回任何类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1334">A unary `+`, `-`, `!`, or `~` operator must take a single parameter of type `T` or `T?` and can return any type.</span></span>
*  <span data-ttu-id="6f0ea-1335">一元`++`或`--`运算符必须将单个类型的参数`T`或`T?`并且必须返回相同类型派生自它。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1335">A unary `++` or `--` operator must take a single parameter of type `T` or `T?` and must return that same type or a type derived from it.</span></span>
*  <span data-ttu-id="6f0ea-1336">一元`true`或`false`运算符必须将单个类型的参数`T`或`T?`，并且必须返回类型`bool`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1336">A unary `true` or `false` operator must take a single parameter of type `T` or `T?` and must return type `bool`.</span></span>

<span data-ttu-id="6f0ea-1337">一元运算符的签名包含的运算符标记 (`+`， `-`， `!`， `~`， `++`， `--`， `true`，或`false`) 和单个规范参数的类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1337">The signature of a unary operator consists of the operator token (`+`, `-`, `!`, `~`, `++`, `--`, `true`, or `false`) and the type of the single formal parameter.</span></span> <span data-ttu-id="6f0ea-1338">返回类型不是一个一元运算符的签名的一部分，也不是正式参数的名称。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1338">The return type is not part of a unary operator's signature, nor is the name of the formal parameter.</span></span>

<span data-ttu-id="6f0ea-1339">`true`和`false`一元运算符要求成对的声明。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1339">The `true` and `false` unary operators require pair-wise declaration.</span></span> <span data-ttu-id="6f0ea-1340">如果一个类中声明以下运算符之一而没有声明其他，发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1340">A compile-time error occurs if a class declares one of these operators without also declaring the other.</span></span> <span data-ttu-id="6f0ea-1341">`true`并`false`运算符是中作了进一步介绍[用户定义的条件逻辑运算符](expressions.md#user-defined-conditional-logical-operators)并[布尔表达式](expressions.md#boolean-expressions)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1341">The `true` and `false` operators are described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators) and [Boolean expressions](expressions.md#boolean-expressions).</span></span>

<span data-ttu-id="6f0ea-1342">下面的示例演示一个实现和后续使用`operator ++`对整数向量类：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1342">The following example shows an implementation and subsequent usage of `operator ++` for an integer vector class:</span></span>
```csharp
public class IntVector
{
    public IntVector(int length) {...}

    public int Length {...}                 // read-only property

    public int this[int index] {...}        // read-write indexer

    public static IntVector operator ++(IntVector iv) {
        IntVector temp = new IntVector(iv.Length);
        for (int i = 0; i < iv.Length; i++)
            temp[i] = iv[i] + 1;
        return temp;
    }
}

class Test
{
    static void Main() {
        IntVector iv1 = new IntVector(4);    // vector of 4 x 0
        IntVector iv2;

        iv2 = iv1++;    // iv2 contains 4 x 0, iv1 contains 4 x 1
        iv2 = ++iv1;    // iv2 contains 4 x 2, iv1 contains 4 x 2
    }
}
```

<span data-ttu-id="6f0ea-1343">请注意如何运算符方法将返回通过将 1 添加到的操作数，就像后缀递增而产生的值和递减运算符 ([后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators))，和前缀递增和递减运算符 ([前缀递增和递减运算符](expressions.md#prefix-increment-and-decrement-operators))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1343">Note how the operator method returns the value produced by adding 1 to the operand, just like the  postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)), and the prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="6f0ea-1344">不同于在 c + +，此方法需要不修改其操作数的值直接。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1344">Unlike in C++, this method need not modify the value of its operand directly.</span></span> <span data-ttu-id="6f0ea-1345">事实上，修改操作数的值违反了后缀递增运算符的标准语义。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1345">In fact, modifying the operand value would violate the standard semantics of the postfix increment operator.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="6f0ea-1346">二元运算符</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1346">Binary operators</span></span>

<span data-ttu-id="6f0ea-1347">以下规则适用于二元运算符的声明，其中`T`表示类或结构都包含在运算符声明的实例类型：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1347">The following rules apply to binary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="6f0ea-1348">二进制非移位运算符必须采用两个参数，在至少一个必须具有类型`T`或`T?`，并且可以返回任何类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1348">A binary non-shift operator must take two parameters, at least one of which must have type `T` or `T?`, and can return any type.</span></span>
*  <span data-ttu-id="6f0ea-1349">二进制`<<`或`>>`运算符必须将两个参数，其中第一个类型必须`T`或`T?`，其中第二个必须具有类型`int`或`int?`，并且可以返回任何类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1349">A binary `<<` or `>>` operator must take two parameters, the first of which must have type `T` or `T?` and the second of which must have type `int` or `int?`, and can return any type.</span></span>

<span data-ttu-id="6f0ea-1350">二元运算符的签名包含的运算符标记 (`+`， `-`， `*`， `/`， `%`， `&`， `|`， `^`， `<<`， `>>`，`==`， `!=`， `>`， `<`， `>=`，或`<=`) 和两个形参的类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1350">The signature of a binary operator consists of the operator token (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, or `<=`) and the types of the two formal parameters.</span></span> <span data-ttu-id="6f0ea-1351">返回类型和形参的名称不是签名的二进制运算符的一部分。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1351">The return type and the names of the formal parameters are not part of a binary operator's signature.</span></span>

<span data-ttu-id="6f0ea-1352">某些二元运算符要求成对的声明。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1352">Certain binary operators require pair-wise declaration.</span></span> <span data-ttu-id="6f0ea-1353">对于一对任一运算符的每个声明，必须是对的匹配中的其他运算符的声明。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1353">For every declaration of either operator of a pair, there must be a matching declaration of the other operator of the pair.</span></span> <span data-ttu-id="6f0ea-1354">两个运算符声明匹配时它们具有相同的返回类型和每个参数的同一类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1354">Two operator declarations match when they have the same return type and the same type for each parameter.</span></span> <span data-ttu-id="6f0ea-1355">以下运算符要求成对的声明：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1355">The following operators require pair-wise declaration:</span></span>

*  <span data-ttu-id="6f0ea-1356">`operator ==` 和 `operator !=`</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1356">`operator ==` and `operator !=`</span></span>
*  <span data-ttu-id="6f0ea-1357">`operator >` 和 `operator <`</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1357">`operator >` and `operator <`</span></span>
*  <span data-ttu-id="6f0ea-1358">`operator >=` 和 `operator <=`</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1358">`operator >=` and `operator <=`</span></span>

### <a name="conversion-operators"></a><span data-ttu-id="6f0ea-1359">转换运算符</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1359">Conversion operators</span></span>

<span data-ttu-id="6f0ea-1360">转换运算符声明引入***用户定义的转换***([用户定义的转换](conversions.md#user-defined-conversions)) 的增强的预定义隐式和显式转换。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1360">A conversion operator declaration introduces a ***user-defined conversion*** ([User-defined conversions](conversions.md#user-defined-conversions)) which augments the pre-defined implicit and explicit conversions.</span></span>

<span data-ttu-id="6f0ea-1361">包括的转换运算符声明`implicit`关键字引入的用户定义的隐式转换。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1361">A conversion operator declaration that includes the `implicit` keyword introduces a user-defined implicit conversion.</span></span> <span data-ttu-id="6f0ea-1362">隐式转换可以在各种情况下，包括函数成员调用、 强制转换表达式和赋值发生。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1362">Implicit conversions can occur in a variety of situations, including function member invocations, cast expressions, and assignments.</span></span> <span data-ttu-id="6f0ea-1363">这是中作了进一步介绍[隐式转换](conversions.md#implicit-conversions)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1363">This is described further in [Implicit conversions](conversions.md#implicit-conversions).</span></span>

<span data-ttu-id="6f0ea-1364">包括的转换运算符声明`explicit`关键字引入的用户定义的显式转换。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1364">A conversion operator declaration that includes the `explicit` keyword introduces a user-defined explicit conversion.</span></span> <span data-ttu-id="6f0ea-1365">显式转换进一步说明了，并且在强制转换表达式可能会发生[显式转换](conversions.md#explicit-conversions)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1365">Explicit conversions can occur in cast expressions, and are described further in [Explicit conversions](conversions.md#explicit-conversions).</span></span>

<span data-ttu-id="6f0ea-1366">转换运算符将源类型，即转换运算符，为目标类型，指示转换运算符的返回类型的参数类型转换。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1366">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span>

<span data-ttu-id="6f0ea-1367">给定的源类型`S`和目标类型`T`，如果`S`或`T`是可以为 null 的类型，让`S0`并`T0`指其基础类型，否则`S0`和`T0`是等于`S`和`T`分别。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1367">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="6f0ea-1368">类或结构允许以声明的源类型转换`S`为目标类型`T`只有在满足以下所有时：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1368">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="6f0ea-1369">`S0` 和`T0`是不同的类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1369">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="6f0ea-1370">要么`S0`或`T0`是在运算符声明发生的类或结构类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1370">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="6f0ea-1371">既不`S0`也不`T0`是*interface_type*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1371">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="6f0ea-1372">除用户定义的转换，转换不存在从`S`到`T`或从`T`到`S`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1372">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="6f0ea-1373">出于这些规则，任何类型与关联的参数`S`或`T`被视为具有不与其他类型的继承关系和参数将被忽略这些类型的任何约束的唯一类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1373">For the purposes of these rules, any type parameters associated with `S` or `T` are considered to be unique types that have no inheritance relationship with other types, and any constraints on those type parameters are ignored.</span></span>

<span data-ttu-id="6f0ea-1374">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1374">In the example</span></span>
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
<span data-ttu-id="6f0ea-1375">第一个的两个运算符声明允许的因为出于[索引器](classes.md#indexers).3，`T`并`int`和`string`分别被视为与任何关系的唯一类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1375">the first two operator declarations are permitted because, for the purposes of [Indexers](classes.md#indexers).3, `T` and `int` and `string` respectively are considered unique types with no relationship.</span></span> <span data-ttu-id="6f0ea-1376">但是，第三个运算符是一个错误，因为`C<T>`是类的基类`D<T>`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1376">However, the third operator is an error because `C<T>` is the base class of `D<T>`.</span></span>

<span data-ttu-id="6f0ea-1377">从第二条规则，它遵循的转换运算符必须转换到或从在其中声明该运算符的类或结构类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1377">From the second rule it follows that a conversion operator must convert either to or from the class or struct type in which the operator is declared.</span></span> <span data-ttu-id="6f0ea-1378">例如，就可以为类或结构类型`C`定义的转换从`C`到`int`并从`int`到`C`，但不能从`int`到`bool`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1378">For example, it is possible for a class or struct type `C` to define a conversion from `C` to `int` and from `int` to `C`, but not from `int` to `bool`.</span></span>

<span data-ttu-id="6f0ea-1379">不能直接重新定义的预定义的转换。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1379">It is not possible to directly redefine a pre-defined conversion.</span></span> <span data-ttu-id="6f0ea-1380">因此，转换运算符不允许将转换来自或发往`object`因为之间已存在隐式和显式转换`object`和所有其他类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1380">Thus, conversion operators are not allowed to convert from or to `object` because implicit and explicit conversions already exist between `object` and all other types.</span></span> <span data-ttu-id="6f0ea-1381">同样，在源和目标类型都不可以是转换的基类型的另一个，因为已经存在这样的转换。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1381">Likewise, neither the source nor the target types of a conversion can be a base type of the other, since a conversion would then already exist.</span></span>

<span data-ttu-id="6f0ea-1382">但是，就可以在泛型类型，对于特定类型的参数，指定的转换已存在的预定义的转换中声明运算符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1382">However, it is possible to declare operators on generic types that, for particular type arguments, specify conversions that already exist as pre-defined conversions.</span></span> <span data-ttu-id="6f0ea-1383">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1383">In the example</span></span>
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
<span data-ttu-id="6f0ea-1384">类型时`object`指定为的类型参数`T`，第二个运算符声明已存在的转换 (隐式的因此还显式存在从任何类型的转换`object`)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1384">when type `object` is specified as a type argument for `T`, the second operator declares a conversion that already exists (an implicit, and therefore also an explicit, conversion exists from any type to type `object`).</span></span>

<span data-ttu-id="6f0ea-1385">在其中两个类型之间存在的预定义的转换的情况下，将忽略这些类型之间的任何用户定义转换。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1385">In cases where a pre-defined conversion exists between two types, any user-defined conversions between those types are ignored.</span></span> <span data-ttu-id="6f0ea-1386">尤其是在下列情况下：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1386">Specifically:</span></span>

*  <span data-ttu-id="6f0ea-1387">如果预定义隐式转换 ([隐式转换](conversions.md#implicit-conversions)) 存在从类型`S`键入`T`，所有用户都定义的转换 （隐式或显式） 从`S`到`T`将被忽略。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1387">If a pre-defined implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from type `S` to type `T`, all user-defined conversions (implicit or explicit) from `S` to `T` are ignored.</span></span>
*  <span data-ttu-id="6f0ea-1388">如果预定义的显式转换 ([显式转换](conversions.md#explicit-conversions)) 存在从类型`S`键入`T`，从任何用户定义的显式转换`S`到`T`将被忽略。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1388">If a pre-defined explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) exists from type `S` to type `T`, any user-defined explicit conversions from `S` to `T` are ignored.</span></span> <span data-ttu-id="6f0ea-1389">此外：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1389">Furthermore:</span></span>

<span data-ttu-id="6f0ea-1390">如果`T`是一个接口类型，从用户定义的隐式转换`S`到`T`将被忽略。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1390">If `T` is an interface type, user-defined implicit conversions from `S` to `T` are ignored.</span></span>

<span data-ttu-id="6f0ea-1391">否则为用户定义的隐式转换从`S`到`T`仍被视为。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1391">Otherwise, user-defined implicit conversions from `S` to `T` are still considered.</span></span>

<span data-ttu-id="6f0ea-1392">对于所有类型，但`object`，运算符声明为通过`Convertible<T>`与预定义的转换不冲突类型更高版本。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1392">For all types but `object`, the operators declared by the `Convertible<T>` type above do not conflict with pre-defined conversions.</span></span> <span data-ttu-id="6f0ea-1393">例如：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1393">For example:</span></span>
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

<span data-ttu-id="6f0ea-1394">但是，对于类型`object`，预定义的转换隐藏在所有情况下，但一个用户定义的转换：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1394">However, for type `object`, pre-defined conversions hide the user-defined conversions in all cases but one:</span></span>

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

<span data-ttu-id="6f0ea-1395">用户定义的转换不允许将转换来自或发往*interface_type*s。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1395">User-defined conversions are not allowed to convert from or to *interface_type*s.</span></span> <span data-ttu-id="6f0ea-1396">具体而言，此限制可确保没有用户定义的转换发生时将转换为*interface_type*，并且转换为*interface_type*时才会成功对象要转换成实际实现指定*interface_type*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1396">In particular, this restriction ensures that no user-defined transformations occur when converting to an *interface_type*, and that a conversion to an *interface_type* succeeds only if the object being converted actually implements the specified *interface_type*.</span></span>

<span data-ttu-id="6f0ea-1397">转换运算符的签名组成的源类型和目标类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1397">The signature of a conversion operator consists of the source type and the target type.</span></span> <span data-ttu-id="6f0ea-1398">（请注意这是在签名中参与的返回类型的成员的唯一形式。）`implicit`或`explicit`分类的转换运算符不是运算符的签名的一部分。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1398">(Note that this is the only form of member for which the return type participates in the signature.) The `implicit` or `explicit` classification of a conversion operator is not part of the operator's signature.</span></span> <span data-ttu-id="6f0ea-1399">因此，类或结构不能声明两者`implicit`和一个`explicit`具有相同的源和目标类型的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1399">Thus, a class or struct cannot declare both an `implicit` and an `explicit` conversion operator with the same source and target types.</span></span>

<span data-ttu-id="6f0ea-1400">通常情况下，应设计用户定义的隐式转换，永远不会引发异常，并且永远不会丢失信息。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1400">In general, user-defined implicit conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="6f0ea-1401">如果用户定义的转换可能导致引发异常 （例如，因为源参数不在范围内） 或丢失的信息 （如放弃高顺序位），则该转换应定义为显式转换。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1401">If a user-defined conversion can give rise to exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as an explicit conversion.</span></span>

<span data-ttu-id="6f0ea-1402">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1402">In the example</span></span>
```csharp
using System;

public struct Digit
{
    byte value;

    public Digit(byte value) {
        if (value < 0 || value > 9) throw new ArgumentException();
        this.value = value;
    }

    public static implicit operator byte(Digit d) {
        return d.value;
    }

    public static explicit operator Digit(byte b) {
        return new Digit(b);
    }
}
```
<span data-ttu-id="6f0ea-1403">从转换`Digit`到`byte`因为它永远不会引发异常或丢失信息，但从转换是隐式`byte`到`Digit`是因为显式`Digit`只能表示可能的子集值`byte`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1403">the conversion from `Digit` to `byte` is implicit because it never throws exceptions or loses information, but the conversion from `byte` to `Digit` is explicit since `Digit` can only represent a subset of the possible values of a `byte`.</span></span>

## <a name="instance-constructors"></a><span data-ttu-id="6f0ea-1404">实例构造函数</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1404">Instance constructors</span></span>

<span data-ttu-id="6f0ea-1405">***实例构造函数***是实现初始化类实例所需执行的操作的成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1405">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="6f0ea-1406">使用声明实例构造函数*constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1406">Instance constructors are declared using *constructor_declaration*s:</span></span>

```antlr
constructor_declaration
    : attributes? constructor_modifier* constructor_declarator constructor_body
    ;

constructor_modifier
    : 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'extern'
    | constructor_modifier_unsafe
    ;

constructor_declarator
    : identifier '(' formal_parameter_list? ')' constructor_initializer?
    ;

constructor_initializer
    : ':' 'base' '(' argument_list? ')'
    | ':' 'this' '(' argument_list? ')'
    ;

constructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="6f0ea-1407">一个*constructor_declaration*可能包括一套*特性*([特性](attributes.md))，四种访问修饰符的有效组合 ([访问修饰符](classes.md#access-modifiers))，和一个`extern`([外部方法](classes.md#external-methods)) 修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1407">A *constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and an `extern` ([External methods](classes.md#external-methods)) modifier.</span></span> <span data-ttu-id="6f0ea-1408">构造函数声明不被允许多次包含同一修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1408">A constructor declaration is not permitted to include the same modifier multiple times.</span></span>

<span data-ttu-id="6f0ea-1409">*标识符*的*constructor_declarator*必须命名在其中声明的实例构造函数的类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1409">The *identifier* of a *constructor_declarator* must name the class in which the instance constructor is declared.</span></span> <span data-ttu-id="6f0ea-1410">如果指定的任何其他名称，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1410">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="6f0ea-1411">可选*formal_parameter_list*的实例构造函数受到相同的规则*formal_parameter_list*的一种方法 ([方法](classes.md#methods))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1411">The optional *formal_parameter_list* of an instance constructor is subject to the same rules as the *formal_parameter_list* of a method ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="6f0ea-1412">形参列表定义签名 ([签名和超载](basic-concepts.md#signatures-and-overloading)) 的实例构造函数，并且凭此控制过程重载决策 ([类型推理](expressions.md#type-inference)) 选择特定实例构造函数调用中。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1412">The formal parameter list defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of an instance constructor and governs the process whereby overload resolution ([Type inference](expressions.md#type-inference)) selects a particular instance constructor in an invocation.</span></span>

<span data-ttu-id="6f0ea-1413">每种类型中引用*formal_parameter_list*的实例构造函数必须至少与构造函数本身相同的可访问性 ([可访问性约束](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1413">Each of the types referenced in the *formal_parameter_list* of an instance constructor must be at least as accessible as the constructor itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6f0ea-1414">可选*constructor_initializer*指定另一个实例构造函数调用中给定的语句执行之前*constructor_body*的此实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1414">The optional *constructor_initializer* specifies another instance constructor to invoke before executing the statements given in the *constructor_body* of this instance constructor.</span></span> <span data-ttu-id="6f0ea-1415">这是中作了进一步介绍[构造函数初始值设定项](classes.md#constructor-initializers)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1415">This is described further in [Constructor initializers](classes.md#constructor-initializers).</span></span>

<span data-ttu-id="6f0ea-1416">当构造函数声明包括`extern`修饰符，构造函数称为***外部构造函数***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1416">When a constructor declaration includes an `extern` modifier, the constructor is said to be an ***external constructor***.</span></span> <span data-ttu-id="6f0ea-1417">因为外部构造函数声明不提供任何实际的实现，其*constructor_body*包含一个分号。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1417">Because an external constructor declaration provides no actual implementation, its *constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="6f0ea-1418">对于所有其他构造函数， *constructor_body*组成*块*它指定要初始化类的新实例的语句。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1418">For all other constructors, the *constructor_body* consists of a *block* which specifies the statements to initialize a new instance of the class.</span></span> <span data-ttu-id="6f0ea-1419">这完全对应*块*的实例方法使用`void`返回类型 ([方法体](classes.md#method-body))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1419">This corresponds exactly to the *block* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="6f0ea-1420">实例构造函数不会继承。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1420">Instance constructors are not inherited.</span></span> <span data-ttu-id="6f0ea-1421">因此，类具有实例构造函数而非实际的类中声明。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1421">Thus, a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="6f0ea-1422">如果类不包含任何实例构造函数声明，会自动提供默认实例构造函数 ([默认构造函数](classes.md#default-constructors))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1422">If a class contains no instance constructor declarations, a default instance constructor is automatically provided ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="6f0ea-1423">实例构造函数调用由*object_creation_expression*s ([对象创建表达式](expressions.md#object-creation-expressions)) 并通过*constructor_initializer*s。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1423">Instance constructors are invoked by *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)) and through *constructor_initializer*s.</span></span>

### <a name="constructor-initializers"></a><span data-ttu-id="6f0ea-1424">构造函数初始值设定项</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1424">Constructor initializers</span></span>

<span data-ttu-id="6f0ea-1425">所有实例构造函数 (除类`object`) 隐式包括另一个实例构造函数的调用后面紧跟*constructor_body*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1425">All instance constructors (except those for class `object`) implicitly include an invocation of another instance constructor immediately before the *constructor_body*.</span></span> <span data-ttu-id="6f0ea-1426">构造函数隐式调用由*constructor_initializer*:</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1426">The constructor to implicitly invoke is determined by the *constructor_initializer*:</span></span>

*  <span data-ttu-id="6f0ea-1427">实例构造函数初始值设定项的窗体`base(argument_list)`或`base()`导致从要调用的直接基类的实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1427">An instance constructor initializer of the form `base(argument_list)` or `base()` causes an instance constructor from the direct base class to be invoked.</span></span> <span data-ttu-id="6f0ea-1428">使用选择该构造函数*argument_list*如果存在且的重载决策规则[重载决策](expressions.md#overload-resolution)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1428">That constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="6f0ea-1429">候选实例构造函数集包括的直接基类中包含的所有可访问的实例构造函数或默认构造函数 ([默认构造函数](classes.md#default-constructors))，如果没有实例的构造函数中声明直接基类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1429">The set of candidate instance constructors consists of all accessible instance constructors contained in the direct base class, or the default constructor ([Default constructors](classes.md#default-constructors)), if no instance constructors are declared in the direct base class.</span></span> <span data-ttu-id="6f0ea-1430">如果此集为空，或者无法识别单个最佳实例构造函数，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1430">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span>
*  <span data-ttu-id="6f0ea-1431">实例构造函数初始值设定项的窗体`this(argument-list)`或`this()`导致从本身要调用的类的实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1431">An instance constructor initializer of the form `this(argument-list)` or `this()` causes an instance constructor from the class itself to be invoked.</span></span> <span data-ttu-id="6f0ea-1432">使用选择构造函数*argument_list*如果存在且的重载决策规则[重载决策](expressions.md#overload-resolution)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1432">The constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="6f0ea-1433">候选实例构造函数集包括的所有可访问的实例构造函数在类本身中声明。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1433">The set of candidate instance constructors consists of all accessible instance constructors declared in the class itself.</span></span> <span data-ttu-id="6f0ea-1434">如果此集为空，或者无法识别单个最佳实例构造函数，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1434">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span> <span data-ttu-id="6f0ea-1435">如果某个实例构造函数声明包括调用构造函数本身的构造函数初始值设定项，发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1435">If an instance constructor declaration includes a constructor initializer that invokes the constructor itself, a compile-time error occurs.</span></span>

<span data-ttu-id="6f0ea-1436">如果实例构造函数具有没有构造函数初始值设定项，在窗体的构造函数初始值设定项`base()`隐式提供。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1436">If an instance constructor has no constructor initializer, a constructor initializer of the form `base()` is implicitly provided.</span></span> <span data-ttu-id="6f0ea-1437">因此，实例构造函数声明的窗体</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1437">Thus, an instance constructor declaration of the form</span></span>
```csharp
C(...) {...}
```
<span data-ttu-id="6f0ea-1438">完全相当于</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1438">is exactly equivalent to</span></span>
```csharp
C(...): base() {...}
```

<span data-ttu-id="6f0ea-1439">通过给定的参数的作用域*formal_parameter_list*的实例构造函数声明中包含该声明的构造函数初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1439">The scope of the parameters given by the *formal_parameter_list* of an instance constructor declaration includes the constructor initializer of that declaration.</span></span> <span data-ttu-id="6f0ea-1440">因此，构造函数初始值设定项允许访问的构造函数的参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1440">Thus, a constructor initializer is permitted to access the parameters of the constructor.</span></span> <span data-ttu-id="6f0ea-1441">例如：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1441">For example:</span></span>
```csharp
class A
{
    public A(int x, int y) {}
}

class B: A
{
    public B(int x, int y): base(x + y, x - y) {}
}
```

<span data-ttu-id="6f0ea-1442">实例构造函数初始值设定项无法访问要创建的实例。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1442">An instance constructor initializer cannot access the instance being created.</span></span> <span data-ttu-id="6f0ea-1443">因此它会导致编译时错误引用`this`中参数的构造函数初始值设定项表达式，因为是它要通过引用任何实例成员的自变量表达式的编译时错误*simple_name*.</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1443">Therefore it is a compile-time error to reference `this` in an argument expression of the constructor initializer, as is it a compile-time error for an argument expression to reference any instance member through a *simple_name*.</span></span>

### <a name="instance-variable-initializers"></a><span data-ttu-id="6f0ea-1444">实例变量的初始值设定项</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1444">Instance variable initializers</span></span>

<span data-ttu-id="6f0ea-1445">当实例构造函数具有没有构造函数初始值设定项，或它具有窗体的构造函数初始值设定项`base(...)`，该构造函数隐式执行指定的初始化*variable_initializer*的 s在其类中声明的实例字段。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1445">When an instance constructor has no constructor initializer, or it has a constructor initializer of the form `base(...)`, that constructor implicitly performs the initializations specified by the *variable_initializer*s of the instance fields declared in its class.</span></span> <span data-ttu-id="6f0ea-1446">这对应于分配给构造函数和直接基类构造函数隐式调用之前在输入时立即执行的一系列。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1446">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor and before the implicit invocation of the direct base class constructor.</span></span> <span data-ttu-id="6f0ea-1447">变量的初始值设定项类声明中显示的文本顺序执行。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1447">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span>

### <a name="constructor-execution"></a><span data-ttu-id="6f0ea-1448">构造函数执行</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1448">Constructor execution</span></span>

<span data-ttu-id="6f0ea-1449">变量初始值设定项被转换为赋值语句，而这些语句执行之前基类实例构造函数的调用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1449">Variable initializers are transformed into assignment statements, and these assignment statements are executed before the invocation of the base class instance constructor.</span></span> <span data-ttu-id="6f0ea-1450">此顺序可确保有权访问该实例的任何语句执行之前，所有实例字段进行都初始化其变量初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1450">This ordering ensures that all instance fields are initialized by their variable initializers before any statements that have access to that instance are executed.</span></span>

<span data-ttu-id="6f0ea-1451">给定示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1451">Given the example</span></span>
```csharp
using System;

class A
{
    public A() {
        PrintFields();
    }

    public virtual void PrintFields() {}
}

class B: A
{
    int x = 1;
    int y;

    public B() {
        y = -1;
    }

    public override void PrintFields() {
        Console.WriteLine("x = {0}, y = {1}", x, y);
    }
}
```
<span data-ttu-id="6f0ea-1452">当`new B()`用于创建实例`B`，将生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1452">when `new B()` is used to create an instance of `B`, the following output is produced:</span></span>
```
x = 1, y = 0
```

<span data-ttu-id="6f0ea-1453">值`x`为 1，因为在调用基类实例构造函数之前执行变量初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1453">The value of `x` is 1 because the variable initializer is executed before the base class instance constructor is invoked.</span></span> <span data-ttu-id="6f0ea-1454">但是的值`y`为 0 (默认值`int`) 因为分配到`y`基类构造函数返回之后直到才执行。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1454">However, the value of `y` is 0 (the default value of an `int`) because the assignment to `y` is not executed until after the base class constructor returns.</span></span>

<span data-ttu-id="6f0ea-1455">可用于将实例变量的初始值设定项和构造函数初始值设定项之前自动插入语句视为*constructor_body*。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1455">It is useful to think of instance variable initializers and constructor initializers as statements that are automatically inserted before the *constructor_body*.</span></span> <span data-ttu-id="6f0ea-1456">该示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1456">The example</span></span>
```csharp
using System;
using System.Collections;

class A
{
    int x = 1, y = -1, count;

    public A() {
        count = 0;
    }

    public A(int n) {
        count = n;
    }
}

class B: A
{
    double sqrt2 = Math.Sqrt(2.0);
    ArrayList items = new ArrayList(100);
    int max;

    public B(): this(100) {
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        max = n;
    }
}
```
<span data-ttu-id="6f0ea-1457">包含多个变量的初始值设定项;它还包含两个窗体的构造函数初始值设定项 (`base`和`this`)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1457">contains several variable initializers; it also contains constructor initializers of both forms (`base` and `this`).</span></span> <span data-ttu-id="6f0ea-1458">此示例对应于代码如下所示，其中每个注释指示的自动插入的语句 （用于自动插入的构造函数调用的语法无效，但只是用来演示了机制）。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1458">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement (the syntax used for the automatically inserted constructor invocations isn't valid, but merely serves to illustrate the mechanism).</span></span>

```csharp
using System.Collections;

class A
{
    int x, y, count;

    public A() {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = 0;
    }

    public A(int n) {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = n;
    }
}

class B: A
{
    double sqrt2;
    ArrayList items;
    int max;

    public B(): this(100) {
        B(100);                      // Invoke B(int) constructor
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        sqrt2 = Math.Sqrt(2.0);      // Variable initializer
        items = new ArrayList(100);  // Variable initializer
        A(n - 1);                    // Invoke A(int) constructor
        max = n;
    }
}
```

### <a name="default-constructors"></a><span data-ttu-id="6f0ea-1459">默认构造函数</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1459">Default constructors</span></span>

<span data-ttu-id="6f0ea-1460">如果类不包含任何实例构造函数声明，会自动提供默认实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1460">If a class contains no instance constructor declarations, a default instance constructor is automatically provided.</span></span> <span data-ttu-id="6f0ea-1461">该默认构造函数只是调用的直接基类的无参数构造函数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1461">That default constructor simply invokes the parameterless constructor of the direct base class.</span></span> <span data-ttu-id="6f0ea-1462">如果此类为抽象的默认构造函数声明可访问性是受保护。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1462">If the class is abstract then the declared accessibility for the default constructor is protected.</span></span> <span data-ttu-id="6f0ea-1463">否则，默认构造函数声明可访问性是公共的。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1463">Otherwise, the declared accessibility for the default constructor is public.</span></span> <span data-ttu-id="6f0ea-1464">默认构造函数因此，始终是窗体</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1464">Thus, the default constructor is always of the form</span></span>

```csharp
protected C(): base() {}
```
<span data-ttu-id="6f0ea-1465">或</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1465">or</span></span>
```csharp
public C(): base() {}
```
<span data-ttu-id="6f0ea-1466">其中`C`是类的名称。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1466">where `C` is the name of the class.</span></span> <span data-ttu-id="6f0ea-1467">如果重载解析是无法确定基类构造函数初始值设定项的唯一最佳候选项，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1467">If overload resolution is unable to determine a unique best candidate for the base class constructor initializer then a compile-time error occurs.</span></span>

<span data-ttu-id="6f0ea-1468">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1468">In the example</span></span>
```csharp
class Message
{
    object sender;
    string text;
}
```
<span data-ttu-id="6f0ea-1469">提供默认构造函数，因为此类不包含任何实例构造函数声明。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1469">a default constructor is provided because the class contains no instance constructor declarations.</span></span> <span data-ttu-id="6f0ea-1470">因此，此示例是恰好等同于</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1470">Thus, the example is precisely equivalent to</span></span>
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a><span data-ttu-id="6f0ea-1471">私有构造函数</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1471">Private constructors</span></span>

<span data-ttu-id="6f0ea-1472">当类`T`声明仅专用实例构造函数，不能为类外部的程序文本`T`为派生`T`直接创建的实例`T`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1472">When a class `T` declares only private instance constructors, it is not possible for classes outside the program text of `T` to derive from `T` or to directly create instances of `T`.</span></span> <span data-ttu-id="6f0ea-1473">因此，如果一个类只包含静态成员，并且不能被实例化，则添加一个空的私有实例构造函数将阻止实例化。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1473">Thus, if a class contains only static members and isn't intended to be instantiated, adding an empty private instance constructor will prevent instantiation.</span></span> <span data-ttu-id="6f0ea-1474">例如：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1474">For example:</span></span>
```csharp
public class Trig
{
    private Trig() {}        // Prevent instantiation

    public const double PI = 3.14159265358979323846;

    public static double Sin(double x) {...}
    public static double Cos(double x) {...}
    public static double Tan(double x) {...}
}
```

<span data-ttu-id="6f0ea-1475">`Trig`类组相关的方法和常量，但不是要实例化。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1475">The `Trig` class groups related methods and constants, but is not intended to be instantiated.</span></span> <span data-ttu-id="6f0ea-1476">因此它会声明一个空的私有实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1476">Therefore it declares a single empty private instance constructor.</span></span> <span data-ttu-id="6f0ea-1477">必须声明至少一个实例构造函数来取消自动生成的默认构造函数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1477">At least one instance constructor must be declared to suppress the automatic generation of a default constructor.</span></span>

### <a name="optional-instance-constructor-parameters"></a><span data-ttu-id="6f0ea-1478">可选的实例构造函数参数</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1478">Optional instance constructor parameters</span></span>

<span data-ttu-id="6f0ea-1479">`this(...)`窗体的构造函数初始值设定项通常用于结合重载实现可选的实例构造函数参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1479">The `this(...)` form of constructor initializer is commonly used in conjunction with overloading to implement optional instance constructor parameters.</span></span> <span data-ttu-id="6f0ea-1480">在示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1480">In the example</span></span>
```csharp
class Text
{
    public Text(): this(0, 0, null) {}

    public Text(int x, int y): this(x, y, null) {}

    public Text(int x, int y, string s) {
        // Actual constructor implementation
    }
}
```
<span data-ttu-id="6f0ea-1481">前两个实例构造函数只是为缺少自变量提供的默认值。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1481">the first two instance constructors merely provide the default values for the missing arguments.</span></span> <span data-ttu-id="6f0ea-1482">两者都使用`this(...)`构造函数初始值设定项来调用第三个实例构造函数，它会真正初始化新实例的工作。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1482">Both use a `this(...)` constructor initializer to invoke the third instance constructor, which actually does the work of initializing the new instance.</span></span> <span data-ttu-id="6f0ea-1483">效果是，可选的构造函数参数：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1483">The effect is that of optional constructor parameters:</span></span>
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a><span data-ttu-id="6f0ea-1484">静态构造函数</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1484">Static constructors</span></span>

<span data-ttu-id="6f0ea-1485">一个***静态构造函数***是实现初始化已关闭的类类型所需的操作的成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1485">A ***static constructor*** is a member that implements the actions required to initialize a closed class type.</span></span> <span data-ttu-id="6f0ea-1486">使用声明静态构造函数*static_constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1486">Static constructors are declared using *static_constructor_declaration*s:</span></span>

```antlr
static_constructor_declaration
    : attributes? static_constructor_modifiers identifier '(' ')' static_constructor_body
    ;

static_constructor_modifiers
    : 'extern'? 'static'
    | 'static' 'extern'?
    | static_constructor_modifiers_unsafe
    ;

static_constructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="6f0ea-1487">一个*static_constructor_declaration*可能包括一套*特性*([特性](attributes.md)) 和一个`extern`修饰符 ([外部方法](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1487">A *static_constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and an `extern` modifier ([External methods](classes.md#external-methods)).</span></span>

<span data-ttu-id="6f0ea-1488">*标识符*的*static_constructor_declaration*必须命名在其中声明静态构造函数的类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1488">The *identifier* of a *static_constructor_declaration* must name the class in which the static constructor is declared.</span></span> <span data-ttu-id="6f0ea-1489">如果指定的任何其他名称，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1489">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="6f0ea-1490">如果静态构造函数声明包含`extern`修饰符，静态构造函数称为***外部静态构造函数***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1490">When a static constructor declaration includes an `extern` modifier, the static constructor is said to be an ***external static constructor***.</span></span> <span data-ttu-id="6f0ea-1491">因为外部静态构造函数声明不提供任何实际的实现，其*static_constructor_body*包含一个分号。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1491">Because an external static constructor declaration provides no actual implementation, its *static_constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="6f0ea-1492">所有其他静态构造函数声明*static_constructor_body*组成*块*它指定要执行，以初始化类的语句。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1492">For all other static constructor declarations, the *static_constructor_body* consists of a *block* which specifies the statements to execute in order to initialize the class.</span></span> <span data-ttu-id="6f0ea-1493">这完全对应*method_body*的静态方法替换`void`返回类型 ([方法体](classes.md#method-body))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1493">This corresponds exactly to the *method_body* of a static method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="6f0ea-1494">静态构造函数不会继承，而且不能直接调用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1494">Static constructors are not inherited, and cannot be called directly.</span></span>

<span data-ttu-id="6f0ea-1495">已关闭的类类型的静态构造函数在给定的应用程序域中执行最多一次。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1495">The static constructor for a closed class type executes at most once in a given application domain.</span></span> <span data-ttu-id="6f0ea-1496">静态构造函数的执行由第一次以下事件发生在应用程序域内触发：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1496">The execution of a static constructor is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="6f0ea-1497">创建类类型的实例。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1497">An instance of the class type is created.</span></span>
*  <span data-ttu-id="6f0ea-1498">引用任何类类型的静态成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1498">Any of the static members of the class type are referenced.</span></span>

<span data-ttu-id="6f0ea-1499">如果类包含`Main`方法 ([应用程序启动](basic-concepts.md#application-startup)) 中的执行开始时，静态构造函数之前执行此类为`Main`调用方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1499">If a class contains the `Main` method ([Application Startup](basic-concepts.md#application-startup)) in which execution begins, the static constructor for that class executes before the `Main` method is called.</span></span>

<span data-ttu-id="6f0ea-1500">若要初始化新的已关闭的类类型，第一次一组新的静态字段 ([静态和实例字段](classes.md#static-and-instance-fields)) 创建该特定的封闭的类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1500">To initialize a new closed class type, first a new set of static fields ([Static and instance fields](classes.md#static-and-instance-fields)) for that particular closed type is created.</span></span> <span data-ttu-id="6f0ea-1501">每个静态字段初始化为其默认值 ([默认值](variables.md#default-values))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1501">Each of the static fields is initialized to its default value ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="6f0ea-1502">接下来，静态字段初始值设定项 ([静态字段初始化](classes.md#static-field-initialization)) 执行这些静态字段。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1502">Next, the static field initializers ([Static field initialization](classes.md#static-field-initialization)) are executed for those static fields.</span></span> <span data-ttu-id="6f0ea-1503">最后，执行静态构造函数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1503">Finally, the static constructor is executed.</span></span>

<span data-ttu-id="6f0ea-1504">该示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1504">The example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        A.F();
        B.F();
    }
}

class A
{
    static A() {
        Console.WriteLine("Init A");
    }
    public static void F() {
        Console.WriteLine("A.F");
    }
}

class B
{
    static B() {
        Console.WriteLine("Init B");
    }
    public static void F() {
        Console.WriteLine("B.F");
    }
}
```
<span data-ttu-id="6f0ea-1505">必须生成的输出：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1505">must produce the output:</span></span>
```
Init A
A.F
Init B
B.F
```
<span data-ttu-id="6f0ea-1506">因为执行`A`的静态构造函数通过调用触发`A.F`，和的执行`B`的静态构造函数通过调用触发`B.F`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1506">because the execution of `A`'s static constructor is triggered by the call to `A.F`, and the execution of `B`'s static constructor is triggered by the call to `B.F`.</span></span>

<span data-ttu-id="6f0ea-1507">就可以构造允许使用变量初始值设定项在其默认值状态中观察到的静态字段的循环依赖项。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1507">It is possible to construct circular dependencies that allow static fields with variable initializers to be observed in their default value state.</span></span>

<span data-ttu-id="6f0ea-1508">该示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1508">The example</span></span>
```csharp
using System;

class A
{
    public static int X;

    static A() {
        X = B.Y + 1;
    }
}

class B
{
    public static int Y = A.X + 1;

    static B() {}

    static void Main() {
        Console.WriteLine("X = {0}, Y = {1}", A.X, B.Y);
    }
}
```
<span data-ttu-id="6f0ea-1509">生成输出</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1509">produces the output</span></span>
```
X = 1, Y = 2
```

<span data-ttu-id="6f0ea-1510">若要执行`Main`方法，在系统首次运行的初始值设定项`B.Y`前类`B`的静态构造函数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1510">To execute the `Main` method, the system first runs the initializer for `B.Y`, prior to class `B`'s static constructor.</span></span> <span data-ttu-id="6f0ea-1511">`Y`初始值设定项导致`A`的静态构造函数来运行，因为值`A.X`引用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1511">`Y`'s initializer causes `A`'s static constructor to be run because the value of `A.X` is referenced.</span></span> <span data-ttu-id="6f0ea-1512">静态构造函数`A`又继续计算的值`X`，并在此过程中，提取操作的默认值`Y`，这是零。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1512">The static constructor of `A` in turn proceeds to compute the value of `X`, and in doing so fetches the default value of `Y`, which is zero.</span></span> <span data-ttu-id="6f0ea-1513">`A.X` 因此会初始化为 1。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1513">`A.X` is thus initialized to 1.</span></span> <span data-ttu-id="6f0ea-1514">正在运行的进程`A`的静态字段初始值设定项和静态构造函数，然后完成后，返回到初始值的计算`Y`，结果将变得 2。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1514">The process of running `A`'s static field initializers and static constructor then completes, returning to the calculation of the initial value of `Y`, the result of which becomes 2.</span></span>

<span data-ttu-id="6f0ea-1515">为每个封闭式构造的类类型，一次执行静态构造函数，因为它是很方便地强制执行不能在编译时通过约束检查的类型参数的运行时检查 ([类型参数约束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1515">Because the static constructor is executed exactly once for each closed constructed class type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="6f0ea-1516">例如，以下类型使用静态构造函数来强制实施的类型参数是枚举：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1516">For example, the following type uses a static constructor to enforce that the type argument is an enum:</span></span>
```csharp
class Gen<T> where T: struct
{
    static Gen() {
        if (!typeof(T).IsEnum) {
            throw new ArgumentException("T must be an enum");
        }
    }
}
```

## <a name="destructors"></a><span data-ttu-id="6f0ea-1517">析构函数</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1517">Destructors</span></span>

<span data-ttu-id="6f0ea-1518">一个***析构函数***是实现析构类的实例所必需的操作的成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1518">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="6f0ea-1519">使用声明析构函数*destructor_declaration*:</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1519">A destructor is declared using a *destructor_declaration*:</span></span>

```antlr
destructor_declaration
    : attributes? 'extern'? '~' identifier '(' ')' destructor_body
    | destructor_declaration_unsafe
    ;

destructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="6f0ea-1520">一个*destructor_declaration*可能包括一套*特性*([属性](attributes.md))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1520">A *destructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="6f0ea-1521">*标识符*的*destructor_declaration*必须命名在其中声明析构函数的类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1521">The *identifier* of a *destructor_declaration* must name the class in which the destructor is declared.</span></span> <span data-ttu-id="6f0ea-1522">如果指定的任何其他名称，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1522">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="6f0ea-1523">如果析构函数声明包含`extern`修饰符，析构函数称为***外部析构函数***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1523">When a destructor declaration includes an `extern` modifier, the destructor is said to be an ***external destructor***.</span></span> <span data-ttu-id="6f0ea-1524">因为外部析构函数声明不提供任何实际的实现，其*destructor_body*包含一个分号。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1524">Because an external destructor declaration provides no actual implementation, its *destructor_body* consists of a semicolon.</span></span> <span data-ttu-id="6f0ea-1525">对于所有其他析构函数， *destructor_body*组成*块*指定为了析构类的实例执行的语句。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1525">For all other destructors, the *destructor_body* consists of a *block* which specifies the statements to execute in order to destruct an instance of the class.</span></span> <span data-ttu-id="6f0ea-1526">一个*destructor_body*完全对应*method_body*的实例方法具有`void`返回类型 ([方法体](classes.md#method-body))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1526">A *destructor_body* corresponds exactly to the *method_body* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="6f0ea-1527">析构函数不会继承。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1527">Destructors are not inherited.</span></span> <span data-ttu-id="6f0ea-1528">因此，类具有不析构函数不是可能在该类中声明。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1528">Thus, a class has no destructors other than the one which may be declared in that class.</span></span>

<span data-ttu-id="6f0ea-1529">析构函数需要不具有任何参数，因为它不能为重载，因此一个类可以有，最多一个析构函数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1529">Since a destructor is required to have no parameters, it cannot be overloaded, so a class can have, at most, one destructor.</span></span>

<span data-ttu-id="6f0ea-1530">析构函数自动调用和不能显式调用。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1530">Destructors are invoked automatically, and cannot be invoked explicitly.</span></span> <span data-ttu-id="6f0ea-1531">实例将成为符合销毁条件时不再可以为任何代码即可使用该实例。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1531">An instance becomes eligible for destruction when it is no longer possible for any code to use that instance.</span></span> <span data-ttu-id="6f0ea-1532">执行实例的析构函数可能会发生在实例变得符合销毁条件之后任何时间。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1532">Execution of the destructor for the instance may occur at any time after the instance becomes eligible for destruction.</span></span> <span data-ttu-id="6f0ea-1533">当销毁实例时，该实例的继承链中的析构函数，按顺序调用，从大多数派生程度高到最低派生。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1533">When an instance is destructed, the destructors in that instance's inheritance chain are called, in order, from most derived to least derived.</span></span> <span data-ttu-id="6f0ea-1534">析构函数可能在任何线程上执行。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1534">A destructor may be executed on any thread.</span></span> <span data-ttu-id="6f0ea-1535">用于控制何时以及如何执行析构函数的规则的进一步讨论，请参阅[自动内存管理](basic-concepts.md#automatic-memory-management)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1535">For further discussion of the rules that govern when and how a destructor is executed, see [Automatic memory management](basic-concepts.md#automatic-memory-management).</span></span>

<span data-ttu-id="6f0ea-1536">该示例的输出</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1536">The output of the example</span></span>
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("A's destructor");
    }
}

class B: A
{
    ~B() {
        Console.WriteLine("B's destructor");
    }
}

class Test
{
   static void Main() {
        B b = new B();
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
   }
}
```
<span data-ttu-id="6f0ea-1537">is</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1537">is</span></span>
```
B's destructor
A's destructor
```
<span data-ttu-id="6f0ea-1538">由于继承链中的析构函数的调用顺序情况下，从大多数派生程度高到最低派生。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1538">since destructors in an inheritance chain are called in order, from most derived to least derived.</span></span>

<span data-ttu-id="6f0ea-1539">析构函数通过重写虚拟方法来实现`Finalize`上`System.Object`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1539">Destructors are implemented by overriding the virtual method `Finalize` on `System.Object`.</span></span> <span data-ttu-id="6f0ea-1540">C# 程序中不允许重写此方法或调用它 （或它的重写） 直接。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1540">C# programs are not permitted to override this method or call it (or overrides of it) directly.</span></span> <span data-ttu-id="6f0ea-1541">例如，下列程序</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1541">For instance, the program</span></span>
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
<span data-ttu-id="6f0ea-1542">包含两个错误。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1542">contains two errors.</span></span>

<span data-ttu-id="6f0ea-1543">编译器行为就像此方法，并替代，根本不存在。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1543">The compiler behaves as if this method, and overrides of it, do not exist at all.</span></span> <span data-ttu-id="6f0ea-1544">因此，此程序：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1544">Thus, this program:</span></span>
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
<span data-ttu-id="6f0ea-1545">有效，并显示隐藏的方法`System.Object`的`Finalize`方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1545">is valid, and the method shown hides `System.Object`'s `Finalize` method.</span></span>

<span data-ttu-id="6f0ea-1546">有关的行为的讨论时从析构函数引发异常，请参阅[如何处理异常](exceptions.md#how-exceptions-are-handled)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1546">For a discussion of the behavior when an exception is thrown from a destructor, see [How exceptions are handled](exceptions.md#how-exceptions-are-handled).</span></span>

## <a name="iterators"></a><span data-ttu-id="6f0ea-1547">Iterators</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1547">Iterators</span></span>

<span data-ttu-id="6f0ea-1548">函数成员 ([函数成员](expressions.md#function-members)) 使用迭代器块实现 ([块](statements.md#blocks)) 称为***迭代器***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1548">A function member ([Function members](expressions.md#function-members)) implemented using an iterator block ([Blocks](statements.md#blocks)) is called an ***iterator***.</span></span>

<span data-ttu-id="6f0ea-1549">迭代器块可能用作函数成员的正文，前提是相应的函数成员的返回类型是一个枚举器接口 ([枚举器接口](classes.md#enumerator-interfaces)) 或一个可枚举接口 ([可枚举接口](classes.md#enumerable-interfaces))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1549">An iterator block may be used as the body of a function member as long as the return type of the corresponding function member is one of the enumerator interfaces ([Enumerator interfaces](classes.md#enumerator-interfaces)) or one of the enumerable interfaces ([Enumerable interfaces](classes.md#enumerable-interfaces)).</span></span> <span data-ttu-id="6f0ea-1550">它可能会出现*method_body*， *operator_body*或*accessor_body*，而不能为事件、 实例构造函数、 静态构造函数和析构函数迭代器作为实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1550">It can occur as a *method_body*, *operator_body* or *accessor_body*, whereas events, instance constructors, static constructors and destructors cannot be implemented as iterators.</span></span>

<span data-ttu-id="6f0ea-1551">使用迭代器块实现函数成员时，它是形参列表的函数成员指定任何的编译时错误`ref`或`out`参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1551">When a function member is implemented using an iterator block, it is a compile-time error for the formal parameter list of the function member to specify any `ref` or `out` parameters.</span></span>

### <a name="enumerator-interfaces"></a><span data-ttu-id="6f0ea-1552">枚举器接口</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1552">Enumerator interfaces</span></span>

<span data-ttu-id="6f0ea-1553">***枚举器接口***是非泛型接口`System.Collections.IEnumerator`和泛型接口的所有实例化`System.Collections.Generic.IEnumerator<T>`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1553">The ***enumerator interfaces*** are the non-generic interface `System.Collections.IEnumerator` and all instantiations of the generic interface `System.Collections.Generic.IEnumerator<T>`.</span></span> <span data-ttu-id="6f0ea-1554">为了简洁起见，在这一章中这些接口称为`IEnumerator`和`IEnumerator<T>`分别。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1554">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerator` and `IEnumerator<T>`, respectively.</span></span>

### <a name="enumerable-interfaces"></a><span data-ttu-id="6f0ea-1555">可枚举接口</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1555">Enumerable interfaces</span></span>

<span data-ttu-id="6f0ea-1556">***可枚举接口***是非泛型接口`System.Collections.IEnumerable`和泛型接口的所有实例化`System.Collections.Generic.IEnumerable<T>`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1556">The ***enumerable interfaces*** are the non-generic interface `System.Collections.IEnumerable` and all instantiations of the generic interface `System.Collections.Generic.IEnumerable<T>`.</span></span> <span data-ttu-id="6f0ea-1557">为了简洁起见，在这一章中这些接口称为`IEnumerable`和`IEnumerable<T>`分别。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1557">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerable` and `IEnumerable<T>`, respectively.</span></span>

### <a name="yield-type"></a><span data-ttu-id="6f0ea-1558">Yield 类型</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1558">Yield type</span></span>

<span data-ttu-id="6f0ea-1559">一个迭代器，将生成一系列值，所有相同的类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1559">An iterator produces a sequence of values, all of the same type.</span></span> <span data-ttu-id="6f0ea-1560">这种类型称为***产生类型***的迭代器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1560">This type is called the ***yield type*** of the iterator.</span></span>

*  <span data-ttu-id="6f0ea-1561">返回一个迭代器的 yield 类型`IEnumerator`或`IEnumerable`是`object`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1561">The yield type of an iterator that returns `IEnumerator` or `IEnumerable` is `object`.</span></span>
*  <span data-ttu-id="6f0ea-1562">返回一个迭代器的 yield 类型`IEnumerator<T>`或`IEnumerable<T>`是`T`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1562">The yield type of an iterator that returns `IEnumerator<T>` or `IEnumerable<T>` is `T`.</span></span>

### <a name="enumerator-objects"></a><span data-ttu-id="6f0ea-1563">枚举器对象</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1563">Enumerator objects</span></span>

<span data-ttu-id="6f0ea-1564">使用迭代器块实现返回一个枚举器接口类型的函数成员时，调用的函数成员不会不立即执行的代码在迭代器块中。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1564">When a function member returning an enumerator interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="6f0ea-1565">相反，***枚举器对象***创建并返回。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1565">Instead, an ***enumerator object*** is created and returned.</span></span> <span data-ttu-id="6f0ea-1566">此对象将封装在迭代器块中，指定的代码和执行的迭代器块中的代码发生时枚举器对象的`MoveNext`调用方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1566">This object encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="6f0ea-1567">枚举器对象具有以下特征：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1567">An enumerator object has the following characteristics:</span></span>

*  <span data-ttu-id="6f0ea-1568">它实现`IEnumerator`并`IEnumerator<T>`，其中`T`是迭代器的 yield 类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1568">It implements `IEnumerator` and `IEnumerator<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="6f0ea-1569">它实现 `System.IDisposable`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1569">It implements `System.IDisposable`.</span></span>
*  <span data-ttu-id="6f0ea-1570">（如果有） 的参数值的副本初始化和实例值传递给函数成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1570">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>
*  <span data-ttu-id="6f0ea-1571">它有四个可能的状态，***之前***，***运行***，***挂起***，以及***后***，并最初处于***之前***状态。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1571">It has four potential states, ***before***, ***running***, ***suspended***, and ***after***, and is initially in the ***before*** state.</span></span>

<span data-ttu-id="6f0ea-1572">枚举器对象通常是一个封装迭代器块中的代码并实现枚举器接口的编译器生成的枚举器类的实例，但也可能实现的其他方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1572">An enumerator object is typically an instance of a compiler-generated enumerator class that encapsulates the code in the iterator block and implements the enumerator interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="6f0ea-1573">如果枚举器类由编译器生成的将嵌套的类、 直接或间接地，在类中包含的函数成员，它将具有私有可访问性，和它将具有名称保留供编译器使用 ([标识符](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1573">If an enumerator class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="6f0ea-1574">枚举器对象可以实现比上面指定的多个接口。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1574">An enumerator object may implement more interfaces than those specified above.</span></span>

<span data-ttu-id="6f0ea-1575">以下各节介绍的具体行为`MoveNext`， `Current`，并`Dispose`的成员`IEnumerable`和`IEnumerable<T>`接口提供的枚举器对象的实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1575">The following sections describe the exact behavior of the `MoveNext`, `Current`, and `Dispose` members of the `IEnumerable` and `IEnumerable<T>` interface implementations provided by an enumerator object.</span></span>

<span data-ttu-id="6f0ea-1576">请注意，不支持枚举器对象`IEnumerator.Reset`方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1576">Note that enumerator objects do not support the `IEnumerator.Reset` method.</span></span> <span data-ttu-id="6f0ea-1577">调用此方法会导致`System.NotSupportedException`引发。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1577">Invoking this method causes a `System.NotSupportedException` to be thrown.</span></span>

#### <a name="the-movenext-method"></a><span data-ttu-id="6f0ea-1578">MoveNext 方法</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1578">The MoveNext method</span></span>

<span data-ttu-id="6f0ea-1579">`MoveNext`方法的枚举器对象封装的迭代器块的代码。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1579">The `MoveNext` method of an enumerator object encapsulates the code of an iterator block.</span></span> <span data-ttu-id="6f0ea-1580">调用`MoveNext`方法中的迭代器块和集执行代码`Current`形式相应的枚举器对象的属性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1580">Invoking the `MoveNext` method executes code in the iterator block and sets the `Current` property of the enumerator object as appropriate.</span></span> <span data-ttu-id="6f0ea-1581">精确所执行的操作`MoveNext`取决于枚举器对象的状态时`MoveNext`调用：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1581">The precise action performed by `MoveNext` depends on the state of the enumerator object when `MoveNext` is invoked:</span></span>

*  <span data-ttu-id="6f0ea-1582">如果枚举数对象的状态为***之前***，则调用`MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1582">If the state of the enumerator object is ***before***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="6f0ea-1583">将状态更改为***运行***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1583">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="6f0ea-1584">初始化参数 (包括`this`) 的迭代器块的自变量值和保存时枚举器对象已初始化的实例值。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1584">Initializes the parameters (including `this`) of the iterator block to the argument values and instance value saved when the enumerator object was initialized.</span></span>
   * <span data-ttu-id="6f0ea-1585">从一开始执行迭代器块，直到执行被中断 （如下所述）。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1585">Executes the iterator block from the beginning until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="6f0ea-1586">如果枚举数对象的状态为***运行***，调用的结果`MoveNext`未指定。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1586">If the state of the enumerator object is ***running***, the result of invoking `MoveNext` is unspecified.</span></span>
*  <span data-ttu-id="6f0ea-1587">如果枚举数对象的状态为***挂起***，则调用`MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1587">If the state of the enumerator object is ***suspended***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="6f0ea-1588">将状态更改为***运行***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1588">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="6f0ea-1589">将所有本地变量和参数 （包括这） 的值还原为上次挂起的迭代器块执行时保存的值。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1589">Restores the values of all local variables and parameters (including this) to the values saved when execution of the iterator block was last suspended.</span></span> <span data-ttu-id="6f0ea-1590">请注意，引用这些变量的任何对象的内容可能已更改自上一个调用 MoveNext。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1590">Note that the contents of any objects referenced by these variables may have changed since the previous call to MoveNext.</span></span>
   * <span data-ttu-id="6f0ea-1591">恢复执行迭代器块紧跟`yield return`语句导致的挂起执行，并将继续，直到执行被中断 （如下所述）。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1591">Resumes execution of the iterator block immediately following the `yield return` statement that caused the suspension of execution and continues until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="6f0ea-1592">如果枚举数对象的状态为***后***，则调用`MoveNext`返回`false`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1592">If the state of the enumerator object is ***after***, invoking `MoveNext` returns `false`.</span></span>


<span data-ttu-id="6f0ea-1593">时`MoveNext`执行迭代器块中，可以中断执行四种方式： 通过`yield return`语句，也可由`yield break`语句由遇到迭代器块的末尾和异常被引发和传播出去迭代器块。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1593">When `MoveNext` executes the iterator block, execution can be interrupted in four ways: By a `yield return` statement, by a `yield break` statement, by encountering the end of the iterator block, and by an exception being thrown and propagated out of the iterator block.</span></span>

*  <span data-ttu-id="6f0ea-1594">当`yield return`遇到语句 ([yield 语句](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1594">When a `yield return` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="6f0ea-1595">在语句中给定的表达式是计算、 隐式转换为 yield 类型和分配给`Current`枚举器对象的属性。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1595">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
   * <span data-ttu-id="6f0ea-1596">挂起的迭代器体的执行。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1596">Execution of the iterator body is suspended.</span></span> <span data-ttu-id="6f0ea-1597">所有本地变量和参数的值 (包括`this`) 保存，因为此位置`yield return`语句。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1597">The values of all local variables and parameters (including `this`) are saved, as is the location of this `yield return` statement.</span></span> <span data-ttu-id="6f0ea-1598">如果`yield return`语句是在一个或多个`try`阻止，关联`finally`块不会在这一次执行。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1598">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
   * <span data-ttu-id="6f0ea-1599">枚举器对象的状态将变为***挂起***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1599">The state of the enumerator object is changed to ***suspended***.</span></span>
   * <span data-ttu-id="6f0ea-1600">`MoveNext`方法将返回`true`给迭代成功地推进到下一个值，该值指示调用方。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1600">The `MoveNext` method returns `true` to its caller, indicating that the iteration successfully advanced to the next value.</span></span>
*  <span data-ttu-id="6f0ea-1601">当`yield break`遇到语句 ([yield 语句](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1601">When a `yield break` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="6f0ea-1602">如果`yield break`语句是在一个或多个`try`阻止，关联`finally`块被执行。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1602">If the `yield break` statement is within one or more `try` blocks, the associated `finally` blocks are executed.</span></span>
   * <span data-ttu-id="6f0ea-1603">枚举器对象的状态将变为***后***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1603">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="6f0ea-1604">`MoveNext`方法将返回`false`到其调用方，指示迭代已完成。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1604">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="6f0ea-1605">遇到迭代器体的末尾时：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1605">When the end of the iterator body is encountered:</span></span>
   * <span data-ttu-id="6f0ea-1606">枚举器对象的状态将变为***后***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1606">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="6f0ea-1607">`MoveNext`方法将返回`false`到其调用方，指示迭代已完成。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1607">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="6f0ea-1608">当引发了异常并传播出去迭代器块：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1608">When an exception is thrown and propagated out of the iterator block:</span></span>
   * <span data-ttu-id="6f0ea-1609">相应`finally`迭代器体中的块将执行已的异常传播。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1609">Appropriate `finally` blocks in the iterator body will have been executed by the exception propagation.</span></span>
   * <span data-ttu-id="6f0ea-1610">枚举器对象的状态将变为***后***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1610">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="6f0ea-1611">异常传播的调用方继续`MoveNext`方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1611">The exception propagation continues to the caller of the `MoveNext` method.</span></span>

#### <a name="the-current-property"></a><span data-ttu-id="6f0ea-1612">Current 属性</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1612">The Current property</span></span>

<span data-ttu-id="6f0ea-1613">枚举器对象的`Current`属性受`yield return`迭代器块中的语句。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1613">An enumerator object's `Current` property is affected by `yield return` statements in the iterator block.</span></span>

<span data-ttu-id="6f0ea-1614">当枚举器对象处于***挂起***状态的值`Current`是通过以前调用设置的值`MoveNext`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1614">When an enumerator object is in the ***suspended*** state, the value of `Current` is the value set by the previous call to `MoveNext`.</span></span> <span data-ttu-id="6f0ea-1615">当枚举器对象处于***之前***，***运行***，或***后***状态的访问结果`Current`未指定。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1615">When an enumerator object is in the ***before***, ***running***, or ***after*** states, the result of accessing `Current` is unspecified.</span></span>

<span data-ttu-id="6f0ea-1616">使用 yield 的迭代器键入以外`object`，访问的结果`Current`通过枚举器对象的`IEnumerable`实现对应于访问`Current`通过枚举器对象的`IEnumerator<T>`实现并将结果转换为`object`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1616">For an iterator with a yield type other than `object`, the result of accessing `Current` through the enumerator object's `IEnumerable` implementation corresponds to accessing `Current` through the enumerator object's `IEnumerator<T>` implementation and casting the result to `object`.</span></span>

#### <a name="the-dispose-method"></a><span data-ttu-id="6f0ea-1617">Dispose 方法</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1617">The Dispose method</span></span>

<span data-ttu-id="6f0ea-1618">`Dispose`方法用于通过引入枚举器对象来清理迭代***后***状态。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1618">The `Dispose` method is used to clean up the iteration by bringing the enumerator object to the ***after*** state.</span></span>

*  <span data-ttu-id="6f0ea-1619">如果枚举数对象的状态为***之前***，则调用`Dispose`将状态更改为***后***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1619">If the state of the enumerator object is ***before***, invoking `Dispose` changes the state to ***after***.</span></span>
*  <span data-ttu-id="6f0ea-1620">如果枚举数对象的状态为***运行***，调用的结果`Dispose`未指定。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1620">If the state of the enumerator object is ***running***, the result of invoking `Dispose` is unspecified.</span></span>
*  <span data-ttu-id="6f0ea-1621">如果枚举数对象的状态为***挂起***，则调用`Dispose`:</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1621">If the state of the enumerator object is ***suspended***, invoking `Dispose`:</span></span>
   * <span data-ttu-id="6f0ea-1622">将状态更改为***运行***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1622">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="6f0ea-1623">执行任何最后块就像上次执行的`yield return`语句已`yield break`语句。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1623">Executes any finally blocks as if the last executed `yield return` statement were a `yield break` statement.</span></span> <span data-ttu-id="6f0ea-1624">如果此操作导致引发和传播出去迭代器体引发异常，枚举器对象的状态设置为***后***的调用方传播异常和`Dispose`方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1624">If this causes an exception to be thrown and propagated out of the iterator body, the state of the enumerator object is set to ***after*** and the exception is propagated to the caller of the `Dispose` method.</span></span>
   * <span data-ttu-id="6f0ea-1625">将状态更改为***后***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1625">Changes the state to ***after***.</span></span>
*  <span data-ttu-id="6f0ea-1626">如果枚举数对象的状态为***后***，则调用`Dispose`不产生任何影响。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1626">If the state of the enumerator object is ***after***, invoking `Dispose` has no affect.</span></span>

### <a name="enumerable-objects"></a><span data-ttu-id="6f0ea-1627">可枚举对象</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1627">Enumerable objects</span></span>

<span data-ttu-id="6f0ea-1628">使用迭代器块实现返回一个可枚举接口类型的函数成员时，调用的函数成员不会不立即执行的代码在迭代器块中。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1628">When a function member returning an enumerable interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="6f0ea-1629">相反，***的可枚举对象***创建并返回。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1629">Instead, an ***enumerable object*** is created and returned.</span></span> <span data-ttu-id="6f0ea-1630">可枚举对象的`GetEnumerator`方法返回在迭代器块中，指定枚举器对象封装代码和执行的迭代器块中的代码发生时枚举器对象的`MoveNext`调用方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1630">The enumerable object's `GetEnumerator` method returns an enumerator object that encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="6f0ea-1631">一个可枚举对象具有以下特征：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1631">An enumerable object has the following characteristics:</span></span>

*  <span data-ttu-id="6f0ea-1632">它实现`IEnumerable`并`IEnumerable<T>`，其中`T`是迭代器的 yield 类型。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1632">It implements `IEnumerable` and `IEnumerable<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="6f0ea-1633">（如果有） 的参数值的副本初始化和实例值传递给函数成员。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1633">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>

<span data-ttu-id="6f0ea-1634">一个可枚举对象通常是编译器生成可枚举类封装迭代器块中的代码和实现的可枚举接口的实例，但也可能实现的其他方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1634">An enumerable object is typically an instance of a compiler-generated enumerable class that encapsulates the code in the iterator block and implements the enumerable interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="6f0ea-1635">如果由编译器生成的可枚举类，则将嵌套的类、 直接或间接地，在类中包含的函数成员，它将具有私有可访问性，和它将具有名称保留供编译器使用 ([标识符](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1635">If an enumerable class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="6f0ea-1636">一个可枚举对象可以实现比上面指定的多个接口。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1636">An enumerable object may implement more interfaces than those specified above.</span></span> <span data-ttu-id="6f0ea-1637">具体而言，一个可枚举对象，还可以实施`IEnumerator`和`IEnumerator<T>`，使其能够充当可枚举值和一个枚举器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1637">In particular, an enumerable object may also implement `IEnumerator` and `IEnumerator<T>`, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="6f0ea-1638">在实现该类型，第一次一个可枚举对象`GetEnumerator`调用方法时，返回可枚举对象本身。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1638">In that type of implementation, the first time an enumerable object's `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="6f0ea-1639">可枚举对象的后续调用`GetEnumerator`，如果任何，返回可枚举对象的副本。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1639">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="6f0ea-1640">因此，每个返回的枚举器都具有其自己的状态和一个枚举器中的更改将不会影响另一个。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1640">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span>

#### <a name="the-getenumerator-method"></a><span data-ttu-id="6f0ea-1641">GetEnumerator 方法</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1641">The GetEnumerator method</span></span>

<span data-ttu-id="6f0ea-1642">一个可枚举对象提供的实现`GetEnumerator`的方法`IEnumerable`和`IEnumerable<T>`接口。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1642">An enumerable object provides an implementation of the `GetEnumerator` methods of the `IEnumerable` and `IEnumerable<T>` interfaces.</span></span> <span data-ttu-id="6f0ea-1643">这两个`GetEnumerator`方法共享通用的实现，用于获取并返回的可用枚举器对象。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1643">The two `GetEnumerator` methods share a common implementation that acquires and returns an available enumerator object.</span></span> <span data-ttu-id="6f0ea-1644">枚举器对象使用的参数值初始化和实例值的可枚举对象时初始化，但其他情况下保存枚举器对象的函数中所述[枚举器对象](classes.md#enumerator-objects)。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1644">The enumerator object is initialized with the argument values and instance value saved when the enumerable object was initialized, but otherwise the enumerator object functions as described in [Enumerator objects](classes.md#enumerator-objects).</span></span>

### <a name="implementation-example"></a><span data-ttu-id="6f0ea-1645">实现示例</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1645">Implementation example</span></span>

<span data-ttu-id="6f0ea-1646">本部分介绍的迭代器在标准 C# 构造方面的一个可能实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1646">This section describes a possible implementation of iterators in terms of standard C# constructs.</span></span> <span data-ttu-id="6f0ea-1647">此处所述的实现基于 Microsoft C# 编译器所使用的相同的原则，但它不是强制性的实现或只有一个可能的。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1647">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation or the only one possible.</span></span>

<span data-ttu-id="6f0ea-1648">以下`Stack<T>`类实现其`GetEnumerator`使用迭代器方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1648">The following `Stack<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="6f0ea-1649">迭代器，用于枚举自上而下的顺序中的堆栈中的元素。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1649">The iterator enumerates the elements of the stack in top to bottom order.</span></span>

```csharp
using System;
using System.Collections;
using System.Collections.Generic;

class Stack<T>: IEnumerable<T>
{
    T[] items;
    int count;

    public void Push(T item) {
        if (items == null) {
            items = new T[4];
        }
        else if (items.Length == count) {
            T[] newItems = new T[count * 2];
            Array.Copy(items, 0, newItems, 0, count);
            items = newItems;
        }
        items[count++] = item;
    }

    public T Pop() {
        T result = items[--count];
        items[count] = default(T);
        return result;
    }

    public IEnumerator<T> GetEnumerator() {
        for (int i = count - 1; i >= 0; --i) yield return items[i];
    }
}
```

<span data-ttu-id="6f0ea-1650">`GetEnumerator`方法可以转换成封装在迭代器块中，代码的编译器生成的枚举器类的实例化，在下面的示例所示。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1650">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
class Stack<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1: IEnumerator<T>, IEnumerator
    {
        int __state;
        T __current;
        Stack<T> __this;
        int i;

        public __Enumerator1(Stack<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
                case 1: goto __state1;
                case 2: goto __state2;
            }
            i = __this.count - 1;
        __loop:
            if (i < 0) goto __state2;
            __current = __this.items[i];
            __state = 1;
            return true;
        __state1:
            --i;
            goto __loop;
        __state2:
            __state = 2;
            return false;
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="6f0ea-1651">在前面的转换，转换为状态机并放入迭代器块中的代码`MoveNext`枚举器类的方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1651">In the preceding translation, the code in the iterator block is turned into a state machine and placed in the `MoveNext` method of the enumerator class.</span></span> <span data-ttu-id="6f0ea-1652">此外，本地变量`i`以便它可以继续存在于调用到枚举器对象中的字段启用`MoveNext`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1652">Furthermore, the local variable `i` is turned into a field in the enumerator object so it can continue to exist across invocations of `MoveNext`.</span></span>

<span data-ttu-id="6f0ea-1653">以下示例输出的整数 1 到 10 中的一个简单的乘法表。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1653">The following example prints a simple multiplication table of the integers 1 through 10.</span></span> <span data-ttu-id="6f0ea-1654">`FromTo`在示例中的方法将返回一个可枚举对象并使用迭代器实现。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1654">The `FromTo` method in the example returns an enumerable object and is implemented using an iterator.</span></span>

```csharp
using System;
using System.Collections.Generic;

class Test
{
    static IEnumerable<int> FromTo(int from, int to) {
        while (from <= to) yield return from++;
    }

    static void Main() {
        IEnumerable<int> e = FromTo(1, 10);
        foreach (int x in e) {
            foreach (int y in e) {
                Console.Write("{0,3} ", x * y);
            }
            Console.WriteLine();
        }
    }
}
```

<span data-ttu-id="6f0ea-1655">`FromTo`方法可以转换成封装在迭代器块中，代码的编译器生成的可枚举类的实例化，在下面的示例所示。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1655">The `FromTo` method can be translated into an instantiation of a compiler-generated enumerable class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
using System;
using System.Threading;
using System.Collections;
using System.Collections.Generic;

class Test
{
    ...

    static IEnumerable<int> FromTo(int from, int to) {
        return new __Enumerable1(from, to);
    }

    class __Enumerable1:
        IEnumerable<int>, IEnumerable,
        IEnumerator<int>, IEnumerator
    {
        int __state;
        int __current;
        int __from;
        int from;
        int to;
        int i;

        public __Enumerable1(int __from, int to) {
            this.__from = __from;
            this.to = to;
        }

        public IEnumerator<int> GetEnumerator() {
            __Enumerable1 result = this;
            if (Interlocked.CompareExchange(ref __state, 1, 0) != 0) {
                result = new __Enumerable1(__from, to);
                result.__state = 1;
            }
            result.from = result.__from;
            return result;
        }

        IEnumerator IEnumerable.GetEnumerator() {
            return (IEnumerator)GetEnumerator();
        }

        public int Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
            case 1:
                if (from > to) goto case 2;
                __current = from++;
                __state = 1;
                return true;
            case 2:
                __state = 2;
                return false;
            default:
                throw new InvalidOperationException();
            }
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="6f0ea-1656">Enumerable 类实现的可枚举接口和枚举器接口，使其能够充当可枚举值和一个枚举器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1656">The enumerable class implements both the enumerable interfaces and the enumerator interfaces, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="6f0ea-1657">第一次`GetEnumerator`调用方法时，返回可枚举对象本身。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1657">The first time the `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="6f0ea-1658">可枚举对象的后续调用`GetEnumerator`，如果任何，返回可枚举对象的副本。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1658">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="6f0ea-1659">因此，每个返回的枚举器都具有其自己的状态和一个枚举器中的更改将不会影响另一个。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1659">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span> <span data-ttu-id="6f0ea-1660">`Interlocked.CompareExchange`方法用于确保线程安全操作。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1660">The `Interlocked.CompareExchange` method is used to ensure thread-safe operation.</span></span>

<span data-ttu-id="6f0ea-1661">`from`和`to`参数将转换为可枚举类中的字段。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1661">The `from` and `to` parameters are turned into fields in the enumerable class.</span></span> <span data-ttu-id="6f0ea-1662">因为`from`在附加的迭代器块中修改`__from`字段引入的用于保存到给定的初始值`from`中每个枚举器。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1662">Because `from` is modified in the iterator block, an additional `__from` field is introduced to hold the initial value given to `from` in each enumerator.</span></span>

<span data-ttu-id="6f0ea-1663">`MoveNext`方法会抛出`InvalidOperationException`如果它时，将调用`__state`是`0`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1663">The `MoveNext` method throws an `InvalidOperationException` if it is called when `__state` is `0`.</span></span> <span data-ttu-id="6f0ea-1664">这可避免使用的可枚举对象的枚举器对象，而无需第一个调用作为`GetEnumerator`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1664">This protects against use of the enumerable object as an enumerator object without first calling `GetEnumerator`.</span></span>

<span data-ttu-id="6f0ea-1665">下面的示例演示一个简单的树类。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1665">The following example shows a simple tree class.</span></span> <span data-ttu-id="6f0ea-1666">`Tree<T>`类实现其`GetEnumerator`使用迭代器方法。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1666">The `Tree<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="6f0ea-1667">迭代器枚举顺序中缀的树中的元素。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1667">The iterator enumerates the elements of the tree in infix order.</span></span>

```csharp
using System;
using System.Collections.Generic;

class Tree<T>: IEnumerable<T>
{
    T value;
    Tree<T> left;
    Tree<T> right;

    public Tree(T value, Tree<T> left, Tree<T> right) {
        this.value = value;
        this.left = left;
        this.right = right;
    }

    public IEnumerator<T> GetEnumerator() {
        if (left != null) foreach (T x in left) yield x;
        yield value;
        if (right != null) foreach (T x in right) yield x;
    }
}

class Program
{
    static Tree<T> MakeTree<T>(T[] items, int left, int right) {
        if (left > right) return null;
        int i = (left + right) / 2;
        return new Tree<T>(items[i], 
            MakeTree(items, left, i - 1),
            MakeTree(items, i + 1, right));
    }

    static Tree<T> MakeTree<T>(params T[] items) {
        return MakeTree(items, 0, items.Length - 1);
    }

    // The output of the program is:
    // 1 2 3 4 5 6 7 8 9
    // Mon Tue Wed Thu Fri Sat Sun

    static void Main() {
        Tree<int> ints = MakeTree(1, 2, 3, 4, 5, 6, 7, 8, 9);
        foreach (int i in ints) Console.Write("{0} ", i);
        Console.WriteLine();

        Tree<string> strings = MakeTree(
            "Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun");
        foreach (string s in strings) Console.Write("{0} ", s);
        Console.WriteLine();
    }
}
```

<span data-ttu-id="6f0ea-1668">`GetEnumerator`方法可以转换成封装在迭代器块中，代码的编译器生成的枚举器类的实例化，在下面的示例所示。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1668">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
class Tree<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1 : IEnumerator<T>, IEnumerator
    {
        Node<T> __this;
        IEnumerator<T> __left, __right;
        int __state;
        T __current;

        public __Enumerator1(Node<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            try {
                switch (__state) {

                case 0:
                    __state = -1;
                    if (__this.left == null) goto __yield_value;
                    __left = __this.left.GetEnumerator();
                    goto case 1;

                case 1:
                    __state = -2;
                    if (!__left.MoveNext()) goto __left_dispose;
                    __current = __left.Current;
                    __state = 1;
                    return true;

                __left_dispose:
                    __state = -1;
                    __left.Dispose();

                __yield_value:
                    __current = __this.value;
                    __state = 2;
                    return true;

                case 2:
                    __state = -1;
                    if (__this.right == null) goto __end;
                    __right = __this.right.GetEnumerator();
                    goto case 3;

                case 3:
                    __state = -3;
                    if (!__right.MoveNext()) goto __right_dispose;
                    __current = __right.Current;
                    __state = 3;
                    return true;

                __right_dispose:
                    __state = -1;
                    __right.Dispose();

                __end:
                    __state = 4;
                    break;

                }
            }
            finally {
                if (__state < 0) Dispose();
            }
            return false;
        }

        public void Dispose() {
            try {
                switch (__state) {

                case 1:
                case -2:
                    __left.Dispose();
                    break;

                case 3:
                case -3:
                    __right.Dispose();
                    break;

                }
            }
            finally {
                __state = 4;
            }
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="6f0ea-1669">在中使用编译器生成的临时内存`foreach`语句提升到`__left`和`__right`枚举器对象的字段。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1669">The compiler generated temporaries used in the `foreach` statements are lifted into the `__left` and `__right` fields of the enumerator object.</span></span> <span data-ttu-id="6f0ea-1670">`__state`仔细更新枚举器对象的字段，以便正确`Dispose()`方法将正确调用，如果引发异常。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1670">The `__state` field of the enumerator object is carefully updated so that the correct `Dispose()` method will be called correctly if an exception is thrown.</span></span> <span data-ttu-id="6f0ea-1671">请注意，不能编写转换后的代码具有简单`foreach`语句。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1671">Note that it is not possible to write the translated code with simple `foreach` statements.</span></span>

## <a name="async-functions"></a><span data-ttu-id="6f0ea-1672">异步函数</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1672">Async functions</span></span>

<span data-ttu-id="6f0ea-1673">一种方法 ([方法](classes.md#methods)) 或匿名函数 ([匿名函数表达式](expressions.md#anonymous-function-expressions)) 与`async`修饰符称为***异步函数***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1673">A method ([Methods](classes.md#methods)) or anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) with the `async` modifier is called an ***async function***.</span></span> <span data-ttu-id="6f0ea-1674">一般情况下，术语***异步***用于描述任何类型的函数具有`async`修饰符。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1674">In general, the term ***async*** is used to describe any kind of function that has the `async` modifier.</span></span>

<span data-ttu-id="6f0ea-1675">它是指定任何异步函数的形参列表的编译时错误`ref`或`out`参数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1675">It is a compile-time error for the formal parameter list of an async function to specify any `ref` or `out` parameters.</span></span>

<span data-ttu-id="6f0ea-1676">*Return_type*的异步方法必须是`void`或***任务类型***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1676">The *return_type* of an async method must be either `void` or a ***task type***.</span></span> <span data-ttu-id="6f0ea-1677">任务类型：`System.Threading.Tasks.Task`和类型从构造`System.Threading.Tasks.Task<T>`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1677">The task types are `System.Threading.Tasks.Task` and types constructed from `System.Threading.Tasks.Task<T>`.</span></span> <span data-ttu-id="6f0ea-1678">为了简洁起见，在这一章中引用这些类型作为`Task`和`Task<T>`分别。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1678">For the sake of brevity, in this chapter these types are referenced as `Task` and `Task<T>`, respectively.</span></span> <span data-ttu-id="6f0ea-1679">异步方法返回任务类型称为是返回任务的。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1679">An async method returning a task type is said to be task-returning.</span></span>

<span data-ttu-id="6f0ea-1680">任务类型的精确定义是实现定义，但从语言的角度来看任务类型是中一种不完整，状态是成功还是出错。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1680">The exact definition of the task types is implementation defined, but from the language's point of view a task type is in one of the states incomplete, succeeded or faulted.</span></span> <span data-ttu-id="6f0ea-1681">出错的任务记录相关的异常。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1681">A faulted task records a pertinent exception.</span></span> <span data-ttu-id="6f0ea-1682">已成功`Task<T>`记录类型的结果`T`。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1682">A succeeded `Task<T>` records a result of type `T`.</span></span> <span data-ttu-id="6f0ea-1683">任务类型都可等待，并且因此可的操作数 await 表达式 ([Await 表达式](expressions.md#await-expressions))。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1683">Task types are awaitable, and can therefore be the operands of await expressions ([Await expressions](expressions.md#await-expressions)).</span></span>

<span data-ttu-id="6f0ea-1684">异步函数调用具有能够挂起评估通过 await 表达式 ([Await 表达式](expressions.md#await-expressions)) 在其主体中。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1684">An async function invocation has the ability to suspend evaluation by means of await expressions ([Await expressions](expressions.md#await-expressions)) in its body.</span></span> <span data-ttu-id="6f0ea-1685">评估可能更高版本会恢复点处挂起 await 表达式通过***恢复委托***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1685">Evaluation may later be resumed at the point of the suspending await expression by means of a ***resumption delegate***.</span></span> <span data-ttu-id="6f0ea-1686">恢复委托的类型是`System.Action`，和异步函数调用的计算调用它时，将从 await 表达式从中断处恢复。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1686">The resumption delegate is of type `System.Action`, and when it is invoked, evaluation of the async function invocation will resume from the await expression where it left off.</span></span> <span data-ttu-id="6f0ea-1687">***当前调用方***异步函数调用原始调用方函数调用永远不会被挂起，如果是也就是最新的恢复委托调用方否则。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1687">The ***current caller*** of an async function invocation is the original caller if the function invocation has never been suspended, or the most recent caller of the resumption delegate otherwise.</span></span>

### <a name="evaluation-of-a-task-returning-async-function"></a><span data-ttu-id="6f0ea-1688">返回任务的异步函数的求值</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1688">Evaluation of a task-returning async function</span></span>

<span data-ttu-id="6f0ea-1689">返回任务的异步函数的调用会导致返回的任务类型为其生成的实例。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1689">Invocation of a task-returning async function causes an instance of the returned task type to be generated.</span></span> <span data-ttu-id="6f0ea-1690">这称为***返回任务***的异步函数。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1690">This is called the ***return task*** of the async function.</span></span> <span data-ttu-id="6f0ea-1691">该任务是最初处于未完成状态。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1691">The task is initially in an incomplete state.</span></span>

<span data-ttu-id="6f0ea-1692">异步函数体然后评估直到它 （通过到达 await 表达式） 挂起或终止，点控制返回给调用方，以及返回的任务。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1692">The async function body is then evaluated until it is either suspended (by reaching an await expression) or terminates, at which point control is returned to the caller, along with the return task.</span></span>

<span data-ttu-id="6f0ea-1693">异步函数的主体在终止时，返回的任务被移出未完成状态：</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1693">When the body of the async function terminates, the return task is moved out of the incomplete state:</span></span>

*  <span data-ttu-id="6f0ea-1694">如果在函数体终止作为到达返回语句或正文的末尾的结果，返回 task，放入成功状态中记录任何结果值。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1694">If the function body terminates as the result of reaching a return statement or the end of the body, any result value is recorded in the return task, which is put into a succeeded state.</span></span>
*  <span data-ttu-id="6f0ea-1695">如果在函数体将终止作为未捕获的异常的结果 ([throw 语句](statements.md#the-throw-statement)) 将进入错误状态的返回任务中记录异常。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1695">If the function body terminates as the result of an uncaught exception ([The throw statement](statements.md#the-throw-statement)) the exception is recorded in the return task which is put into a faulted state.</span></span>

### <a name="evaluation-of-a-void-returning-async-function"></a><span data-ttu-id="6f0ea-1696">返回 void 的异步函数的求值</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1696">Evaluation of a void-returning async function</span></span>

<span data-ttu-id="6f0ea-1697">如果异步函数的返回类型为`void`，评估不同于上述按以下方式： 因为没有任务返回，该函数改为通信完成和当前线程的例外情况***同步上下文***。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1697">If the return type of the async function is `void`, evaluation differs from the above in the following way: Because no task is returned, the function instead communicates completion and exceptions to the current thread's ***synchronization context***.</span></span> <span data-ttu-id="6f0ea-1698">同步上下文的精确定义取决于实现，但是表示形式的当前线程正在运行"where"。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1698">The exact definition of synchronization context is implementation-dependent, but is a representation of "where" the current thread is running.</span></span> <span data-ttu-id="6f0ea-1699">返回 void 的异步函数的求值开始、 成功完成，或会导致引发未捕获的异常时，通知同步上下文。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1699">The synchronization context is notified when evaluation of a void-returning async function commences, completes successfully, or causes an uncaught exception to be thrown.</span></span>

<span data-ttu-id="6f0ea-1700">这样，来跟踪在其下运行多少返回 void 的异步函数，并决定如何传播异常来自它们的上下文。</span><span class="sxs-lookup"><span data-stu-id="6f0ea-1700">This allows the context to keep track of how many void-returning async functions are running under it, and to decide how to propagate exceptions coming out of them.</span></span>
