# <a name="interfaces"></a><span data-ttu-id="22c14-101">接口</span><span class="sxs-lookup"><span data-stu-id="22c14-101">Interfaces</span></span>

<span data-ttu-id="22c14-102">接口定义一个协定。</span><span class="sxs-lookup"><span data-stu-id="22c14-102">An interface defines a contract.</span></span> <span data-ttu-id="22c14-103">类或结构实现的接口必须遵守其协定。</span><span class="sxs-lookup"><span data-stu-id="22c14-103">A class or struct that implements an interface must adhere to its contract.</span></span> <span data-ttu-id="22c14-104">一个接口可能从多个基接口继承，类或结构可以实现多个接口。</span><span class="sxs-lookup"><span data-stu-id="22c14-104">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="22c14-105">接口可以包含方法、 属性、 事件和索引器。</span><span class="sxs-lookup"><span data-stu-id="22c14-105">Interfaces can contain methods, properties, events, and indexers.</span></span> <span data-ttu-id="22c14-106">接口本身不提供实现，它定义的成员。</span><span class="sxs-lookup"><span data-stu-id="22c14-106">The interface itself does not provide implementations for the members that it defines.</span></span> <span data-ttu-id="22c14-107">接口只是指定必须由实现接口的类或结构提供的成员。</span><span class="sxs-lookup"><span data-stu-id="22c14-107">The interface merely specifies the members that must be supplied by classes or structs that implement the interface.</span></span>

## <a name="interface-declarations"></a><span data-ttu-id="22c14-108">接口声明</span><span class="sxs-lookup"><span data-stu-id="22c14-108">Interface declarations</span></span>

<span data-ttu-id="22c14-109">*Interface_declaration*是*type_declaration* ([类型声明](namespaces.md#type-declarations))，其用于声明新的接口类型。</span><span class="sxs-lookup"><span data-stu-id="22c14-109">An *interface_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new interface type.</span></span>

```antlr
interface_declaration
    : attributes? interface_modifier* 'partial'? 'interface'
      identifier variant_type_parameter_list? interface_base?
      type_parameter_constraints_clause* interface_body ';'?
    ;
```

<span data-ttu-id="22c14-110">*Interface_declaration*包含一组可选*特性*([特性](attributes.md)) 后, 跟一组可选的*interface_modifier*s ([接口修饰符](interfaces.md#interface-modifiers)) 后, 跟一个可选`partial`修饰符，关键字后跟`interface`和一个*标识符*命名接口，跟一个可选*variant_type_parameter_list*规范 ([变体类型的参数列表](interfaces.md#variant-type-parameter-lists)) 后, 跟一个可选*interface_base*规范 ([的基接口](interfaces.md#base-interfaces)) 后, 跟一个可选*type_parameter_constraints_clause*s 规范 ([类型参数约束](classes.md#type-parameter-constraints))后, 跟*interface_body* ([接口正文](interfaces.md#interface-body)) 后, 跟分号 （可选）。</span><span class="sxs-lookup"><span data-stu-id="22c14-110">An *interface_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *interface_modifier*s ([Interface modifiers](interfaces.md#interface-modifiers)), followed by an optional `partial` modifier, followed by the keyword `interface` and an *identifier* that names the interface, followed by an optional *variant_type_parameter_list* specification ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)), followed by an optional *interface_base* specification ([Base interfaces](interfaces.md#base-interfaces)), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by an *interface_body* ([Interface body](interfaces.md#interface-body)), optionally followed by a semicolon.</span></span>

### <a name="interface-modifiers"></a><span data-ttu-id="22c14-111">接口修饰符</span><span class="sxs-lookup"><span data-stu-id="22c14-111">Interface modifiers</span></span>

<span data-ttu-id="22c14-112">*Interface_declaration*可以根据需要包含一系列接口修饰符：</span><span class="sxs-lookup"><span data-stu-id="22c14-112">An *interface_declaration* may optionally include a sequence of interface modifiers:</span></span>

```antlr
interface_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | interface_modifier_unsafe
    ;
```

<span data-ttu-id="22c14-113">它是同一修饰符在接口声明中出现多次的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="22c14-113">It is a compile-time error for the same modifier to appear multiple times in an interface declaration.</span></span>

<span data-ttu-id="22c14-114">`new`修饰符仅允许在类中定义的接口上。</span><span class="sxs-lookup"><span data-stu-id="22c14-114">The `new` modifier is only permitted on interfaces defined within a class.</span></span> <span data-ttu-id="22c14-115">它指定该接口通过相同的名称，隐藏继承的成员中所述[的新修饰符](classes.md#the-new-modifier)。</span><span class="sxs-lookup"><span data-stu-id="22c14-115">It specifies that the interface hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="22c14-116">`public`， `protected`， `internal`，和`private`修饰符控制接口的可访问性。</span><span class="sxs-lookup"><span data-stu-id="22c14-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the interface.</span></span> <span data-ttu-id="22c14-117">具体取决于接口声明发生的上下文，可能会允许仅在部分这些修饰符 ([声明可访问性](basic-concepts.md#declared-accessibility))。</span><span class="sxs-lookup"><span data-stu-id="22c14-117">Depending on the context in which the interface declaration occurs, only some of these modifiers may be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="22c14-118">Partial 修饰符</span><span class="sxs-lookup"><span data-stu-id="22c14-118">Partial modifier</span></span>

<span data-ttu-id="22c14-119">`partial`修饰符指示这*interface_declaration*是分部类型声明。</span><span class="sxs-lookup"><span data-stu-id="22c14-119">The `partial` modifier indicates that this *interface_declaration* is a partial type declaration.</span></span> <span data-ttu-id="22c14-120">具有相同名称的封闭命名空间或类型声明中的多个分部接口声明组合到窗体一个接口声明，遵循的规则中指定[分部类型](classes.md#partial-types)。</span><span class="sxs-lookup"><span data-stu-id="22c14-120">Multiple partial interface declarations with the same name within an enclosing namespace or type declaration combine to form one interface declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="variant-type-parameter-lists"></a><span data-ttu-id="22c14-121">变体类型参数列表</span><span class="sxs-lookup"><span data-stu-id="22c14-121">Variant type parameter lists</span></span>

<span data-ttu-id="22c14-122">变体类型参数列表只能出现在接口和委托类型。</span><span class="sxs-lookup"><span data-stu-id="22c14-122">Variant type parameter lists can only occur on interface and delegate types.</span></span> <span data-ttu-id="22c14-123">与普通的差别*type_parameter_list*是可选的 s *variance_annotation*上每个类型参数。</span><span class="sxs-lookup"><span data-stu-id="22c14-123">The difference from ordinary *type_parameter_list*s is the optional *variance_annotation* on each type parameter.</span></span>

```antlr
variant_type_parameter_list
    : '<' variant_type_parameters '>'
    ;

variant_type_parameters
    : attributes? variance_annotation? type_parameter
    | variant_type_parameters ',' attributes? variance_annotation? type_parameter
    ;

variance_annotation
    : 'in'
    | 'out'
    ;
```

<span data-ttu-id="22c14-124">如果方差批注`out`，类型参数称为***协变***。</span><span class="sxs-lookup"><span data-stu-id="22c14-124">If the variance annotation is `out`, the type parameter is said to be ***covariant***.</span></span> <span data-ttu-id="22c14-125">如果方差批注`in`，类型参数称为***逆变***。</span><span class="sxs-lookup"><span data-stu-id="22c14-125">If the variance annotation is `in`, the type parameter is said to be ***contravariant***.</span></span> <span data-ttu-id="22c14-126">如果没有任何变化批注，类型参数称***固定***。</span><span class="sxs-lookup"><span data-stu-id="22c14-126">If there is no variance annotation, the type parameter is said to be ***invariant***.</span></span>

<span data-ttu-id="22c14-127">在示例</span><span class="sxs-lookup"><span data-stu-id="22c14-127">In the example</span></span>
```csharp
interface C<out X, in Y, Z> 
{
  X M(Y y);
  Z P { get; set; }
}
```
<span data-ttu-id="22c14-128">`X` 为协变`Y`是逆变和`Z`是固定不变。</span><span class="sxs-lookup"><span data-stu-id="22c14-128">`X` is covariant, `Y` is contravariant and `Z` is invariant.</span></span>

#### <a name="variance-safety"></a><span data-ttu-id="22c14-129">变体安全</span><span class="sxs-lookup"><span data-stu-id="22c14-129">Variance safety</span></span>

<span data-ttu-id="22c14-130">方差批注类型形参列表中的一种类型的匹配项将限制类型的类型声明中可能发生的位置的位置。</span><span class="sxs-lookup"><span data-stu-id="22c14-130">The occurrence of variance annotations in the type parameter list of a type restricts the places where types can occur within the type declaration.</span></span>

<span data-ttu-id="22c14-131">一种类型`T`是***输出不安全***如果下列之一保存：</span><span class="sxs-lookup"><span data-stu-id="22c14-131">A type `T` is ***output-unsafe*** if one of the following holds:</span></span>

*  <span data-ttu-id="22c14-132">`T` 是逆变类型参数</span><span class="sxs-lookup"><span data-stu-id="22c14-132">`T` is a contravariant type parameter</span></span>
*  <span data-ttu-id="22c14-133">`T` 为具有输出不安全的元素类型的数组类型</span><span class="sxs-lookup"><span data-stu-id="22c14-133">`T` is an array type with an output-unsafe element type</span></span>
*  <span data-ttu-id="22c14-134">`T` 是一个接口或委托类型`S<A1,...,Ak>`从泛型类型构造`S<X1,...,Xk>`至少一个 where`Ai`以下之一保存：</span><span class="sxs-lookup"><span data-stu-id="22c14-134">`T` is an interface or delegate type `S<A1,...,Ak>` constructed from a generic type `S<X1,...,Xk>` where for at least one `Ai` one of the following holds:</span></span>
   * <span data-ttu-id="22c14-135">`Xi` 协变或固定和`Ai`是不安全的输出。</span><span class="sxs-lookup"><span data-stu-id="22c14-135">`Xi` is covariant or invariant and `Ai` is output-unsafe.</span></span>
   * <span data-ttu-id="22c14-136">`Xi` 是逆变或固定条件和`Ai`是输入安全的。</span><span class="sxs-lookup"><span data-stu-id="22c14-136">`Xi` is contravariant or invariant and `Ai` is input-safe.</span></span>
   
<span data-ttu-id="22c14-137">一种类型`T`是***输入不安全***如果下列之一保存：</span><span class="sxs-lookup"><span data-stu-id="22c14-137">A type `T` is ***input-unsafe*** if one of the following holds:</span></span>

*  <span data-ttu-id="22c14-138">`T` 是协变类型参数</span><span class="sxs-lookup"><span data-stu-id="22c14-138">`T` is a covariant type parameter</span></span>
*  <span data-ttu-id="22c14-139">`T` 是输入不安全的元素类型的数组类型</span><span class="sxs-lookup"><span data-stu-id="22c14-139">`T` is an array type with an input-unsafe element type</span></span>
*  <span data-ttu-id="22c14-140">`T` 是一个接口或委托类型`S<A1,...,Ak>`从泛型类型构造`S<X1,...,Xk>`至少一个 where`Ai`以下之一保存：</span><span class="sxs-lookup"><span data-stu-id="22c14-140">`T` is an interface or delegate type `S<A1,...,Ak>` constructed from a generic type `S<X1,...,Xk>` where for at least one `Ai` one of the following holds:</span></span>
   * <span data-ttu-id="22c14-141">`Xi` 协变或固定和`Ai`是不安全的输入。</span><span class="sxs-lookup"><span data-stu-id="22c14-141">`Xi` is covariant or invariant and `Ai` is input-unsafe.</span></span>
   * <span data-ttu-id="22c14-142">`Xi` 是逆变或固定条件和`Ai`是不安全的输出。</span><span class="sxs-lookup"><span data-stu-id="22c14-142">`Xi` is contravariant or invariant and `Ai` is output-unsafe.</span></span>

<span data-ttu-id="22c14-143">直观地说，输出不安全类型禁止的中的输出位置，并在输入位置禁止的输入不安全类型。</span><span class="sxs-lookup"><span data-stu-id="22c14-143">Intuitively, an output-unsafe type is prohibited in an output position, and an input-unsafe type is prohibited in an input position.</span></span>

<span data-ttu-id="22c14-144">一种类型是***输出安全***如果不是输出不安全，并***输入安全***如果不输入不安全。</span><span class="sxs-lookup"><span data-stu-id="22c14-144">A type is ***output-safe*** if it is not output-unsafe, and ***input-safe*** if it is not input-unsafe.</span></span>

#### <a name="variance-conversion"></a><span data-ttu-id="22c14-145">变化转换</span><span class="sxs-lookup"><span data-stu-id="22c14-145">Variance conversion</span></span>

<span data-ttu-id="22c14-146">方差批注的目的是提供更宽松 （但仍类型安全的） 转换成接口和委托类型。</span><span class="sxs-lookup"><span data-stu-id="22c14-146">The purpose of variance annotations is to provide for more lenient (but still type safe) conversions to interface and delegate types.</span></span> <span data-ttu-id="22c14-147">到此结束的隐式定义 ([隐式转换](conversions.md#implicit-conversions)) 和显式转换 ([显式转换](conversions.md#explicit-conversions)) 进行可转换，按如下所示定义这一概念的使用：</span><span class="sxs-lookup"><span data-stu-id="22c14-147">To this end the definitions of implicit ([Implicit conversions](conversions.md#implicit-conversions)) and explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) make use of the notion of variance-convertibility, which is defined as follows:</span></span>

<span data-ttu-id="22c14-148">一种类型`T<A1,...,An>`可变化转换为某种`T<B1,...,Bn>`如果`T`接口或委托类型使用变体类型参数声明`T<X1,...,Xn>`，并为每个变体类型参数`Xi`以下项之一包含：</span><span class="sxs-lookup"><span data-stu-id="22c14-148">A type `T<A1,...,An>` is variance-convertible to a type `T<B1,...,Bn>` if `T` is either an interface or a delegate type declared with the variant type parameters `T<X1,...,Xn>`, and for each variant type parameter `Xi` one of the following holds:</span></span>

*  <span data-ttu-id="22c14-149">`Xi` 是协变和隐式引用或标识转换存在从`Ai`到 `Bi`</span><span class="sxs-lookup"><span data-stu-id="22c14-149">`Xi` is covariant and an implicit reference or identity conversion exists from `Ai` to `Bi`</span></span>
*  <span data-ttu-id="22c14-150">`Xi` 是逆变和隐式引用或标识转换存在从`Bi`到 `Ai`</span><span class="sxs-lookup"><span data-stu-id="22c14-150">`Xi` is contravariant and an implicit reference or identity conversion exists from `Bi` to `Ai`</span></span>
*  <span data-ttu-id="22c14-151">`Xi` 是固定条件和标识转换存在从`Ai`到 `Bi`</span><span class="sxs-lookup"><span data-stu-id="22c14-151">`Xi` is invariant and an identity conversion exists from `Ai` to `Bi`</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="22c14-152">基接口</span><span class="sxs-lookup"><span data-stu-id="22c14-152">Base interfaces</span></span>

<span data-ttu-id="22c14-153">接口可以继承自零个或多个接口类型，称为***显式基接口***的接口。</span><span class="sxs-lookup"><span data-stu-id="22c14-153">An interface can inherit from zero or more interface types, which are called the ***explicit base interfaces*** of the interface.</span></span> <span data-ttu-id="22c14-154">时接口具有一个或多个显式基接口，然后在该接口的声明中的接口标识符后跟冒号和逗号分隔的基接口类型的列表。</span><span class="sxs-lookup"><span data-stu-id="22c14-154">When an interface has one or more explicit base interfaces, then in the declaration of that interface, the interface identifier is followed by a colon and a comma separated list of base interface types.</span></span>

```antlr
interface_base
    : ':' interface_type_list
    ;
```

<span data-ttu-id="22c14-155">对于构造的接口类型，显式基接口构成方法为泛型类型声明中，对其执行显式基接口声明替换为每个*type_parameter*中的基接口相应的声明*type_argument*构造类型。</span><span class="sxs-lookup"><span data-stu-id="22c14-155">For a constructed interface type, the explicit base interfaces are formed by taking the explicit base interface declarations on the generic type declaration, and substituting, for each *type_parameter* in the base interface declaration, the corresponding *type_argument* of the constructed type.</span></span>

<span data-ttu-id="22c14-156">接口的显式基接口必须至少与接口本身相同的可访问性 ([可访问性约束](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="22c14-156">The explicit base interfaces of an interface must be at least as accessible as the interface itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span> <span data-ttu-id="22c14-157">例如，它是指定的编译时错误`private`或`internal`接口中*interface_base*的`public`接口。</span><span class="sxs-lookup"><span data-stu-id="22c14-157">For example, it is a compile-time error to specify a `private` or `internal` interface in the *interface_base* of a `public` interface.</span></span>

<span data-ttu-id="22c14-158">它是一个接口以直接或间接从自身继承的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="22c14-158">It is a compile-time error for an interface to directly or indirectly inherit from itself.</span></span>

<span data-ttu-id="22c14-159">***的基接口***已显式基接口并其基接口的接口。</span><span class="sxs-lookup"><span data-stu-id="22c14-159">The ***base interfaces*** of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="22c14-160">换而言之，组的基接口是完全的显式基接口，其显式基接口，诸如此类的可传递闭包。</span><span class="sxs-lookup"><span data-stu-id="22c14-160">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span> <span data-ttu-id="22c14-161">接口继承其基接口的所有成员。</span><span class="sxs-lookup"><span data-stu-id="22c14-161">An interface inherits all members of its base interfaces.</span></span> <span data-ttu-id="22c14-162">在示例</span><span class="sxs-lookup"><span data-stu-id="22c14-162">In the example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
<span data-ttu-id="22c14-163">接口的基`IComboBox`都`IControl`， `ITextBox`，和`IListBox`。</span><span class="sxs-lookup"><span data-stu-id="22c14-163">the base interfaces of `IComboBox` are `IControl`, `ITextBox`, and `IListBox`.</span></span>

<span data-ttu-id="22c14-164">换而言之，`IComboBox`上述接口继承的成员`SetText`并`SetItems`以及`Paint`。</span><span class="sxs-lookup"><span data-stu-id="22c14-164">In other words, the `IComboBox` interface above inherits members `SetText` and `SetItems` as well as `Paint`.</span></span>

<span data-ttu-id="22c14-165">每个基接口的接口必须是安全的输出 ([变体安全](interfaces.md#variance-safety))。</span><span class="sxs-lookup"><span data-stu-id="22c14-165">Every base interface of an interface must be output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span> <span data-ttu-id="22c14-166">类或结构还将隐式实现接口实现的所有接口的基接口。</span><span class="sxs-lookup"><span data-stu-id="22c14-166">A class or struct that implements an interface also implicitly implements all of the interface's base interfaces.</span></span>

### <a name="interface-body"></a><span data-ttu-id="22c14-167">接口体内</span><span class="sxs-lookup"><span data-stu-id="22c14-167">Interface body</span></span>

<span data-ttu-id="22c14-168">*Interface_body*接口的定义接口的成员。</span><span class="sxs-lookup"><span data-stu-id="22c14-168">The *interface_body* of an interface defines the members of the interface.</span></span>

```antlr
interface_body
    : '{' interface_member_declaration* '}'
    ;
```

## <a name="interface-members"></a><span data-ttu-id="22c14-169">接口成员</span><span class="sxs-lookup"><span data-stu-id="22c14-169">Interface members</span></span>

<span data-ttu-id="22c14-170">接口的成员是继承自基接口的成员和成员声明由接口本身。</span><span class="sxs-lookup"><span data-stu-id="22c14-170">The members of an interface are the members inherited from the base interfaces and the members declared by the interface itself.</span></span>

```antlr
interface_member_declaration
    : interface_method_declaration
    | interface_property_declaration
    | interface_event_declaration
    | interface_indexer_declaration
    ;
```

<span data-ttu-id="22c14-171">接口声明可以声明零个或多个成员。</span><span class="sxs-lookup"><span data-stu-id="22c14-171">An interface declaration may declare zero or more members.</span></span> <span data-ttu-id="22c14-172">接口成员必须是方法、 属性、 事件或索引器。</span><span class="sxs-lookup"><span data-stu-id="22c14-172">The members of an interface must be methods, properties, events, or indexers.</span></span> <span data-ttu-id="22c14-173">接口不能包含常量、 字段、 运算符、 实例构造函数、 析构函数或类型，也不能包含任何类型的静态成员。</span><span class="sxs-lookup"><span data-stu-id="22c14-173">An interface cannot contain constants, fields, operators, instance constructors, destructors, or types, nor can an interface contain static members of any kind.</span></span>

<span data-ttu-id="22c14-174">所有接口成员隐式都具有公共访问权限。</span><span class="sxs-lookup"><span data-stu-id="22c14-174">All interface members implicitly have public access.</span></span> <span data-ttu-id="22c14-175">它是接口成员声明中包含任何修饰符的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="22c14-175">It is a compile-time error for interface member declarations to include any modifiers.</span></span> <span data-ttu-id="22c14-176">具体而言，不能具有修饰符声明接口成员`abstract`， `public`， `protected`， `internal`， `private`， `virtual`， `override`，或`static`。</span><span class="sxs-lookup"><span data-stu-id="22c14-176">In particular, interfaces members cannot be declared with the modifiers `abstract`, `public`, `protected`, `internal`, `private`, `virtual`, `override`, or `static`.</span></span>

<span data-ttu-id="22c14-177">该示例</span><span class="sxs-lookup"><span data-stu-id="22c14-177">The example</span></span>
```csharp
public delegate void StringListEvent(IStringList sender);

public interface IStringList
{
    void Add(string s);
    int Count { get; }
    event StringListEvent Changed;
    string this[int index] { get; set; }
}
```
<span data-ttu-id="22c14-178">声明接口包含之一的每个可能类型的成员： 方法、 属性、 事件和索引器。</span><span class="sxs-lookup"><span data-stu-id="22c14-178">declares an interface that contains one each of the possible kinds of members: A method, a property, an event, and an indexer.</span></span>

<span data-ttu-id="22c14-179">*Interface_declaration*创建一个新的声明空间 ([声明](basic-concepts.md#declarations))，并且*interface_member_declaration*s 立即包含*interface_declaration*引入此声明空间的新成员。</span><span class="sxs-lookup"><span data-stu-id="22c14-179">An *interface_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *interface_member_declaration*s immediately contained by the *interface_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="22c14-180">以下规则适用于*interface_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="22c14-180">The following rules apply to *interface_member_declaration*s:</span></span>

*  <span data-ttu-id="22c14-181">方法的名称必须不同于所有属性和相同的接口中声明的事件的名称。</span><span class="sxs-lookup"><span data-stu-id="22c14-181">The name of a method must differ from the names of all properties and events declared in the same interface.</span></span> <span data-ttu-id="22c14-182">此外，签名 ([签名和超载](basic-concepts.md#signatures-and-overloading)) 的一种方法必须不同于同一接口中声明的所有其他方法的签名和签名不能在同一个接口中声明的两种方法，完全由不同`ref`和`out`。</span><span class="sxs-lookup"><span data-stu-id="22c14-182">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same interface, and two methods declared in the same interface may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="22c14-183">属性或事件的名称必须不同于在相同的接口中声明的所有其他成员的名称。</span><span class="sxs-lookup"><span data-stu-id="22c14-183">The name of a property or event must differ from the names of all other members declared in the same interface.</span></span>
*  <span data-ttu-id="22c14-184">索引器的签名必须不同于同一接口中声明的所有其他索引器的签名。</span><span class="sxs-lookup"><span data-stu-id="22c14-184">The signature of an indexer must differ from the signatures of all other indexers declared in the same interface.</span></span>

<span data-ttu-id="22c14-185">接口的继承的成员是专门不属于该接口声明空间。</span><span class="sxs-lookup"><span data-stu-id="22c14-185">The inherited members of an interface are specifically not part of the declaration space of the interface.</span></span> <span data-ttu-id="22c14-186">因此，接口可以声明一个具有相同名称或签名为继承的成员的成员。</span><span class="sxs-lookup"><span data-stu-id="22c14-186">Thus, an interface is allowed to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="22c14-187">当发生这种情况时，则称该派生的接口成员，若要隐藏基接口成员。</span><span class="sxs-lookup"><span data-stu-id="22c14-187">When this occurs, the derived interface member is said to hide the base interface member.</span></span> <span data-ttu-id="22c14-188">隐藏继承的成员不被视为错误，但它确实会导致编译器发出警告。</span><span class="sxs-lookup"><span data-stu-id="22c14-188">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="22c14-189">若要禁止显示警告，请在派生的接口成员的声明必须包括`new`修饰符以指示派生的成员用于隐藏基类成员。</span><span class="sxs-lookup"><span data-stu-id="22c14-189">To suppress the warning, the declaration of the derived interface member must include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="22c14-190">本主题讨论中进一步[通过继承隐藏](basic-concepts.md#hiding-through-inheritance)。</span><span class="sxs-lookup"><span data-stu-id="22c14-190">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="22c14-191">如果`new`修饰符包含在不隐藏继承的成员的声明，该结果发出警告。</span><span class="sxs-lookup"><span data-stu-id="22c14-191">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning is issued to that effect.</span></span> <span data-ttu-id="22c14-192">通过删除禁止显示此警告`new`修饰符。</span><span class="sxs-lookup"><span data-stu-id="22c14-192">This warning is suppressed by removing the `new` modifier.</span></span>

<span data-ttu-id="22c14-193">请注意，在类成员`object`不严格地讲，任何接口的成员 ([接口成员](interfaces.md#interface-members))。</span><span class="sxs-lookup"><span data-stu-id="22c14-193">Note that the members in class `object` are not, strictly speaking, members of any interface ([Interface members](interfaces.md#interface-members)).</span></span> <span data-ttu-id="22c14-194">但是，在类成员`object`可通过成员查找中任何接口类型 ([成员查找](expressions.md#member-lookup))。</span><span class="sxs-lookup"><span data-stu-id="22c14-194">However, the members in class `object` are available via member lookup in any interface type ([Member lookup](expressions.md#member-lookup)).</span></span>

### <a name="interface-methods"></a><span data-ttu-id="22c14-195">接口方法</span><span class="sxs-lookup"><span data-stu-id="22c14-195">Interface methods</span></span>

<span data-ttu-id="22c14-196">使用声明接口方法*interface_method_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="22c14-196">Interface methods are declared using *interface_method_declaration*s:</span></span>

```antlr
interface_method_declaration
    : attributes? 'new'? return_type identifier type_parameter_list
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;
```

<span data-ttu-id="22c14-197">*特性*， *return_type*，*标识符*，以及*formal_parameter_list*的接口方法声明具有相同这意味着与方法声明在类中的 ([方法](classes.md#methods))。</span><span class="sxs-lookup"><span data-stu-id="22c14-197">The *attributes*, *return_type*, *identifier*, and *formal_parameter_list* of an interface method declaration have the same meaning as those of a method declaration in a class ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="22c14-198">接口方法声明不允许指定方法体中，并声明因此始终以分号结尾。</span><span class="sxs-lookup"><span data-stu-id="22c14-198">An interface method declaration is not permitted to specify a method body, and the declaration therefore always ends with a semicolon.</span></span>

<span data-ttu-id="22c14-199">每种形式参数类型的接口方法必须是输入安全 ([变体安全](interfaces.md#variance-safety))，并返回类型必须为`void`或输出安全。</span><span class="sxs-lookup"><span data-stu-id="22c14-199">Each formal parameter type of an interface method must be input-safe ([Variance safety](interfaces.md#variance-safety)), and the return type must be either `void` or output-safe.</span></span> <span data-ttu-id="22c14-200">此外，每个类类型约束、 接口类型约束和类型参数约束上方法的任何类型参数必须是输入安全的。</span><span class="sxs-lookup"><span data-stu-id="22c14-200">Furthermore, each class type constraint, interface type constraint and type parameter constraint on any type parameter of the method must be input-safe.</span></span>

<span data-ttu-id="22c14-201">这些规则可确保任何协变或逆变的接口的使用情况保持类型安全。</span><span class="sxs-lookup"><span data-stu-id="22c14-201">These rules ensure that any covariant or contravariant usage of the interface remains type-safe.</span></span> <span data-ttu-id="22c14-202">例如，应用于对象的</span><span class="sxs-lookup"><span data-stu-id="22c14-202">For example,</span></span>
```csharp
interface I<out T> { void M<U>() where U : T; }
```
<span data-ttu-id="22c14-203">是非法的因为的使用情况`T`用作类型参数约束上`U`不是输入安全的。</span><span class="sxs-lookup"><span data-stu-id="22c14-203">is illegal because the usage of `T` as a type parameter constraint on `U` is not input-safe.</span></span>

<span data-ttu-id="22c14-204">已不存在此限制可能会违反类型安全方式如下：</span><span class="sxs-lookup"><span data-stu-id="22c14-204">Were this restriction not in place it would be possible to violate type safety in the following manner:</span></span>
```csharp
class B {}
class D : B{}
class E : B {}
class C : I<D> { public void M<U>() {...} }
...
I<B> b = new C();
b.M<E>();
```
<span data-ttu-id="22c14-205">这是实际调用`C.M<E>`。</span><span class="sxs-lookup"><span data-stu-id="22c14-205">This is actually a call to `C.M<E>`.</span></span> <span data-ttu-id="22c14-206">但该调用需要`E`派生自`D`，因此此处违反类型安全。</span><span class="sxs-lookup"><span data-stu-id="22c14-206">But that call requires that `E` derive from `D`, so type safety would be violated here.</span></span>

### <a name="interface-properties"></a><span data-ttu-id="22c14-207">接口属性</span><span class="sxs-lookup"><span data-stu-id="22c14-207">Interface properties</span></span>

<span data-ttu-id="22c14-208">接口属性的声明使用*interface_property_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="22c14-208">Interface properties are declared using *interface_property_declaration*s:</span></span>

```antlr
interface_property_declaration
    : attributes? 'new'? type identifier '{' interface_accessors '}'
    ;

interface_accessors
    : attributes? 'get' ';'
    | attributes? 'set' ';'
    | attributes? 'get' ';' attributes? 'set' ';'
    | attributes? 'set' ';' attributes? 'get' ';'
    ;
```

<span data-ttu-id="22c14-209">*特性*，*类型*，并*标识符*接口属性声明的类中具有相同含义的属性声明 ([属性](classes.md#properties))。</span><span class="sxs-lookup"><span data-stu-id="22c14-209">The *attributes*, *type*, and *identifier* of an interface property declaration have the same meaning as those of a property declaration in a class ([Properties](classes.md#properties)).</span></span>

<span data-ttu-id="22c14-210">接口属性声明的访问器对应于类属性声明的访问器 ([访问器](classes.md#accessors))，只不过访问器正文必须始终是一个分号。</span><span class="sxs-lookup"><span data-stu-id="22c14-210">The accessors of an interface property declaration correspond to the accessors of a class property declaration ([Accessors](classes.md#accessors)), except that the accessor body must always be a semicolon.</span></span> <span data-ttu-id="22c14-211">因此，访问器只需指示属性是读写、 只读的或只写。</span><span class="sxs-lookup"><span data-stu-id="22c14-211">Thus, the accessors simply indicate whether the property is read-write, read-only, or write-only.</span></span>

<span data-ttu-id="22c14-212">接口属性的类型必须是输出安全是否存在 get 访问器，并且必须是输入安全如果没有 set 访问器。</span><span class="sxs-lookup"><span data-stu-id="22c14-212">The type of an interface property must be output-safe if there is a get accessor, and must be input-safe if there is a set accessor.</span></span>

### <a name="interface-events"></a><span data-ttu-id="22c14-213">接口事件</span><span class="sxs-lookup"><span data-stu-id="22c14-213">Interface events</span></span>

<span data-ttu-id="22c14-214">使用声明接口事件*interface_event_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="22c14-214">Interface events are declared using *interface_event_declaration*s:</span></span>

```antlr
interface_event_declaration
    : attributes? 'new'? 'event' type identifier ';'
    ;
```

<span data-ttu-id="22c14-215">*特性*，*类型*，并*标识符*接口事件声明的类中具有相同的事件声明的含义 ([事件](classes.md#events)).</span><span class="sxs-lookup"><span data-stu-id="22c14-215">The *attributes*, *type*, and *identifier* of an interface event declaration have the same meaning as those of an event declaration in a class ([Events](classes.md#events)).</span></span>

<span data-ttu-id="22c14-216">接口事件的类型必须是输入安全。</span><span class="sxs-lookup"><span data-stu-id="22c14-216">The type of an interface event must be input-safe.</span></span>

### <a name="interface-indexers"></a><span data-ttu-id="22c14-217">接口索引器</span><span class="sxs-lookup"><span data-stu-id="22c14-217">Interface indexers</span></span>

<span data-ttu-id="22c14-218">使用声明接口索引器*interface_indexer_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="22c14-218">Interface indexers are declared using *interface_indexer_declaration*s:</span></span>

```antlr
interface_indexer_declaration
    : attributes? 'new'? type 'this' '[' formal_parameter_list ']' '{' interface_accessors '}'
    ;
```

<span data-ttu-id="22c14-219">*特性*，*类型*，并*formal_parameter_list*接口索引器声明的类 (中具有与索引器声明相同的含义[索引器](classes.md#indexers))。</span><span class="sxs-lookup"><span data-stu-id="22c14-219">The *attributes*, *type*, and *formal_parameter_list* of an interface indexer declaration have the same meaning as those of an indexer declaration in a class ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="22c14-220">接口索引器声明的访问器对应于类索引器声明的访问器 ([索引器](classes.md#indexers))，只不过访问器正文必须始终是一个分号。</span><span class="sxs-lookup"><span data-stu-id="22c14-220">The accessors of an interface indexer declaration correspond to the accessors of a class indexer declaration ([Indexers](classes.md#indexers)), except that the accessor body must always be a semicolon.</span></span> <span data-ttu-id="22c14-221">因此，访问器只需指示索引器为读写、 只读的或只写。</span><span class="sxs-lookup"><span data-stu-id="22c14-221">Thus, the accessors simply indicate whether the indexer is read-write, read-only, or write-only.</span></span>

<span data-ttu-id="22c14-222">接口索引器的所有形式参数类型必须是输入安全。</span><span class="sxs-lookup"><span data-stu-id="22c14-222">All the formal parameter types of an interface indexer must be input-safe .</span></span> <span data-ttu-id="22c14-223">此外，任何`out`或`ref`形参类型还必须是安全的输出。</span><span class="sxs-lookup"><span data-stu-id="22c14-223">In addition, any `out` or `ref` formal parameter types must also be output-safe.</span></span> <span data-ttu-id="22c14-224">请注意，甚至`out`参数需是输入安全的由于基础执行平台的限制。</span><span class="sxs-lookup"><span data-stu-id="22c14-224">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="22c14-225">接口索引器的类型必须是输出安全是否存在 get 访问器，并且必须是输入安全如果没有 set 访问器。</span><span class="sxs-lookup"><span data-stu-id="22c14-225">The type of an interface indexer must be output-safe if there is a get accessor, and must be input-safe if there is a set accessor.</span></span>

### <a name="interface-member-access"></a><span data-ttu-id="22c14-226">接口成员访问</span><span class="sxs-lookup"><span data-stu-id="22c14-226">Interface member access</span></span>

<span data-ttu-id="22c14-227">成员访问通过访问接口成员 ([成员访问](expressions.md#member-access)) 和索引器访问 ([索引器访问](expressions.md#indexer-access)) 形式的表达式`I.M`并`I[A]`，其中`I`是一个接口类型，`M`是方法、 属性或事件的接口类型，和`A`是索引器参数列表。</span><span class="sxs-lookup"><span data-stu-id="22c14-227">Interface members are accessed through member access ([Member access](expressions.md#member-access)) and indexer access ([Indexer access](expressions.md#indexer-access)) expressions of the form `I.M` and `I[A]`, where `I` is an interface type, `M` is a method, property, or event of that interface type, and `A` is an indexer argument list.</span></span>

<span data-ttu-id="22c14-228">接口的仅限单一继承 （继承链中的每个接口都恰好零个或一个直接的基接口）、 成员查找的效果 ([成员查找](expressions.md#member-lookup))，方法调用 ([方法调用](expressions.md#method-invocations))，和索引器访问 ([索引器访问](expressions.md#indexer-access)) 规则是完全与类和结构相同： 小于派生成员具有相同名称或签名的派生程度更大的成员隐藏。</span><span class="sxs-lookup"><span data-stu-id="22c14-228">For interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effects of the member lookup ([Member lookup](expressions.md#member-lookup)), method invocation ([Method invocations](expressions.md#method-invocations)), and indexer access ([Indexer access](expressions.md#indexer-access)) rules are exactly the same as for classes and structs: More derived members hide less derived members with the same name or signature.</span></span> <span data-ttu-id="22c14-229">但是，对于多个继承的接口，二义性可能发生，当两个或多个不相关的基接口声明具有相同名称或签名的成员。</span><span class="sxs-lookup"><span data-stu-id="22c14-229">However, for multiple-inheritance interfaces, ambiguities can occur when two or more unrelated base interfaces declare members with the same name or signature.</span></span> <span data-ttu-id="22c14-230">本部分介绍这种情况下的几个示例。</span><span class="sxs-lookup"><span data-stu-id="22c14-230">This section shows several examples of such situations.</span></span> <span data-ttu-id="22c14-231">在所有情况下，可以使用显式强制转换来解决歧义。</span><span class="sxs-lookup"><span data-stu-id="22c14-231">In all cases, explicit casts can be used to resolve the ambiguities.</span></span>

<span data-ttu-id="22c14-232">在示例</span><span class="sxs-lookup"><span data-stu-id="22c14-232">In the example</span></span>
```csharp
interface IList
{
    int Count { get; set; }
}

interface ICounter
{
    void Count(int i);
}

interface IListCounter: IList, ICounter {}

class C
{
    void Test(IListCounter x) {
        x.Count(1);                  // Error
        x.Count = 1;                 // Error
        ((IList)x).Count = 1;        // Ok, invokes IList.Count.set
        ((ICounter)x).Count(1);      // Ok, invokes ICounter.Count
    }
}
```
<span data-ttu-id="22c14-233">前两个语句会导致编译时错误，因为成员查找 ([成员查找](expressions.md#member-lookup)) 的`Count`中`IListCounter`不明确。</span><span class="sxs-lookup"><span data-stu-id="22c14-233">the first two statements cause compile-time errors because the member lookup ([Member lookup](expressions.md#member-lookup)) of `Count` in `IListCounter` is ambiguous.</span></span> <span data-ttu-id="22c14-234">如示例所示，通过强制转换来解决二义性`x`为适当的基接口类型。</span><span class="sxs-lookup"><span data-stu-id="22c14-234">As illustrated by the example, the ambiguity is resolved by casting `x` to the appropriate base interface type.</span></span> <span data-ttu-id="22c14-235">这种转换具有不运行时成本 — 它们只是包含的作为派生程度更小类型在编译时查看的实例。</span><span class="sxs-lookup"><span data-stu-id="22c14-235">Such casts have no run-time costs—they merely consist of viewing the instance as a less derived type at compile-time.</span></span>

<span data-ttu-id="22c14-236">在示例</span><span class="sxs-lookup"><span data-stu-id="22c14-236">In the example</span></span>
```csharp
interface IInteger
{
    void Add(int i);
}

interface IDouble
{
    void Add(double d);
}

interface INumber: IInteger, IDouble {}

class C
{
    void Test(INumber n) {
        n.Add(1);                // Invokes IInteger.Add
        n.Add(1.0);              // Only IDouble.Add is applicable
        ((IInteger)n).Add(1);    // Only IInteger.Add is a candidate
        ((IDouble)n).Add(1);     // Only IDouble.Add is a candidate
    }
}
```
<span data-ttu-id="22c14-237">在调用`n.Add(1)`中选择`IInteger.Add`通过应用的重载决策规则[重载决策](expressions.md#overload-resolution)。</span><span class="sxs-lookup"><span data-stu-id="22c14-237">the invocation `n.Add(1)` selects `IInteger.Add` by applying the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="22c14-238">同样的调用`n.Add(1.0)`选择`IDouble.Add`。</span><span class="sxs-lookup"><span data-stu-id="22c14-238">Similarly the invocation `n.Add(1.0)` selects `IDouble.Add`.</span></span> <span data-ttu-id="22c14-239">插入了显式强制转换，就只有一个候选方法，因此没有歧义。</span><span class="sxs-lookup"><span data-stu-id="22c14-239">When explicit casts are inserted, there is only one candidate method, and thus no ambiguity.</span></span>

<span data-ttu-id="22c14-240">在示例</span><span class="sxs-lookup"><span data-stu-id="22c14-240">In the example</span></span>
```csharp
interface IBase
{
    void F(int i);
}

interface ILeft: IBase
{
    new void F(int i);
}

interface IRight: IBase
{
    void G();
}

interface IDerived: ILeft, IRight {}

class A
{
    void Test(IDerived d) {
        d.F(1);                 // Invokes ILeft.F
        ((IBase)d).F(1);        // Invokes IBase.F
        ((ILeft)d).F(1);        // Invokes ILeft.F
        ((IRight)d).F(1);       // Invokes IBase.F
    }
}
```
<span data-ttu-id="22c14-241">`IBase.F`情况下隐藏成员`ILeft.F`成员。</span><span class="sxs-lookup"><span data-stu-id="22c14-241">the `IBase.F` member is hidden by the `ILeft.F` member.</span></span> <span data-ttu-id="22c14-242">在调用`d.F(1)`因此选择`ILeft.F`，即使`IBase.F`似乎未隐藏，引导完成的访问路径中`IRight`。</span><span class="sxs-lookup"><span data-stu-id="22c14-242">The invocation `d.F(1)` thus selects `ILeft.F`, even though `IBase.F` appears to not be hidden in the access path that leads through `IRight`.</span></span>

<span data-ttu-id="22c14-243">多个继承接口中的隐藏的直观规则非常简单，如下： 如果在任何一个访问路径，则隐藏该成员，它隐藏在所有的访问路径中。</span><span class="sxs-lookup"><span data-stu-id="22c14-243">The intuitive rule for hiding in multiple-inheritance interfaces is simply this: If a member is hidden in any access path, it is hidden in all access paths.</span></span> <span data-ttu-id="22c14-244">因为从的访问路径`IDerived`到`ILeft`到`IBase`隐藏`IBase.F`，从访问路径中还隐藏该成员`IDerived`到`IRight`到`IBase`。</span><span class="sxs-lookup"><span data-stu-id="22c14-244">Because the access path from `IDerived` to `ILeft` to `IBase` hides `IBase.F`, the member is also hidden in the access path from `IDerived` to `IRight` to `IBase`.</span></span>

## <a name="fully-qualified-interface-member-names"></a><span data-ttu-id="22c14-245">完全限定的接口成员名称</span><span class="sxs-lookup"><span data-stu-id="22c14-245">Fully qualified interface member names</span></span>

<span data-ttu-id="22c14-246">接口成员有时会通过其***完全限定的名称***。</span><span class="sxs-lookup"><span data-stu-id="22c14-246">An interface member is sometimes referred to by its ***fully qualified name***.</span></span> <span data-ttu-id="22c14-247">接口成员的完全限定的名包含的名称的接口中的成员是声明，跟一个点，然后是的成员的名称。</span><span class="sxs-lookup"><span data-stu-id="22c14-247">The fully qualified name of an interface member consists of the name of the interface in which the member is declared, followed by a dot, followed by the name of the member.</span></span> <span data-ttu-id="22c14-248">成员的完全限定的名称引用在其中声明成员的接口。</span><span class="sxs-lookup"><span data-stu-id="22c14-248">The fully qualified name of a member references the interface in which the member is declared.</span></span> <span data-ttu-id="22c14-249">例如，给定的声明</span><span class="sxs-lookup"><span data-stu-id="22c14-249">For example, given the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}
```
<span data-ttu-id="22c14-250">完全限定的名称`Paint`是`IControl.Paint`和完全限定的名称`SetText`是`ITextBox.SetText`。</span><span class="sxs-lookup"><span data-stu-id="22c14-250">the fully qualified name of `Paint` is `IControl.Paint` and the fully qualified name of `SetText` is `ITextBox.SetText`.</span></span>

<span data-ttu-id="22c14-251">在上面的示例中，它不能是指`Paint`作为`ITextBox.Paint`。</span><span class="sxs-lookup"><span data-stu-id="22c14-251">In the example above, it is not possible to refer to `Paint` as `ITextBox.Paint`.</span></span>

<span data-ttu-id="22c14-252">如果接口是命名空间的一部分，接口成员的完全限定的名包括命名空间名称。</span><span class="sxs-lookup"><span data-stu-id="22c14-252">When an interface is part of a namespace, the fully qualified name of an interface member includes the namespace name.</span></span> <span data-ttu-id="22c14-253">例如</span><span class="sxs-lookup"><span data-stu-id="22c14-253">For example</span></span>
```csharp
namespace System
{
    public interface ICloneable
    {
        object Clone();
    }
}
```

<span data-ttu-id="22c14-254">此处，完全限定的名称`Clone`方法是`System.ICloneable.Clone`。</span><span class="sxs-lookup"><span data-stu-id="22c14-254">Here, the fully qualified name of the `Clone` method is `System.ICloneable.Clone`.</span></span>

## <a name="interface-implementations"></a><span data-ttu-id="22c14-255">接口实现</span><span class="sxs-lookup"><span data-stu-id="22c14-255">Interface implementations</span></span>

<span data-ttu-id="22c14-256">可能由类和结构实现接口。</span><span class="sxs-lookup"><span data-stu-id="22c14-256">Interfaces may be implemented by classes and structs.</span></span> <span data-ttu-id="22c14-257">若要指示类或结构直接实现一个接口，类或结构的基列表中包含的接口标识符。</span><span class="sxs-lookup"><span data-stu-id="22c14-257">To indicate that a class or struct directly implements an interface, the interface identifier is included in the base class list of the class or struct.</span></span> <span data-ttu-id="22c14-258">例如：</span><span class="sxs-lookup"><span data-stu-id="22c14-258">For example:</span></span>
```csharp
interface ICloneable
{
    object Clone();
}

interface IComparable
{
    int CompareTo(object other);
}

class ListEntry: ICloneable, IComparable
{
    public object Clone() {...}
    public int CompareTo(object other) {...}
}
```

<span data-ttu-id="22c14-259">类或结构直接实现的接口直接隐式实现的所有接口的基接口。</span><span class="sxs-lookup"><span data-stu-id="22c14-259">A class or struct that directly implements an interface also directly implements all of the interface's base interfaces implicitly.</span></span> <span data-ttu-id="22c14-260">即使类或结构不会显式列出基类列表中的所有基接口，这是如此。</span><span class="sxs-lookup"><span data-stu-id="22c14-260">This is true even if the class or struct doesn't explicitly list all base interfaces in the base class list.</span></span> <span data-ttu-id="22c14-261">例如：</span><span class="sxs-lookup"><span data-stu-id="22c14-261">For example:</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    public void Paint() {...}
    public void SetText(string text) {...}
}
```

<span data-ttu-id="22c14-262">在这里，类`TextBox`同时实现`IControl`和`ITextBox`。</span><span class="sxs-lookup"><span data-stu-id="22c14-262">Here, class `TextBox` implements both `IControl` and `ITextBox`.</span></span>

<span data-ttu-id="22c14-263">当类`C`直接实现一个接口，从 C 派生的所有类还隐式实现接口。</span><span class="sxs-lookup"><span data-stu-id="22c14-263">When a class `C` directly implements an interface, all classes derived from C also implement the interface implicitly.</span></span> <span data-ttu-id="22c14-264">在类声明中指定的基接口可以是构造的接口类型 ([构造类型](types.md#constructed-types))。</span><span class="sxs-lookup"><span data-stu-id="22c14-264">The base interfaces specified in a class declaration can be constructed interface types ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="22c14-265">基接口不能是自身、 类型参数，但它可能涉及到作用域中的类型参数。</span><span class="sxs-lookup"><span data-stu-id="22c14-265">A base interface cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span> <span data-ttu-id="22c14-266">下面的代码演示如何对一个类可以实现和扩展构造的类型：</span><span class="sxs-lookup"><span data-stu-id="22c14-266">The following code illustrates how a class can implement and extend constructed types:</span></span>
```csharp
class C<U,V> {}

interface I1<V> {}

class D: C<string,int>, I1<string> {}

class E<T>: C<int,T>, I1<T> {}
```

<span data-ttu-id="22c14-267">泛型类声明的基接口必须满足唯一性规则中所述[实现的接口的唯一性](interfaces.md#uniqueness-of-implemented-interfaces)。</span><span class="sxs-lookup"><span data-stu-id="22c14-267">The base interfaces of a generic class declaration must satisfy the uniqueness rule described in [Uniqueness of implemented interfaces](interfaces.md#uniqueness-of-implemented-interfaces).</span></span>

### <a name="explicit-interface-member-implementations"></a><span data-ttu-id="22c14-268">显式接口成员实现代码</span><span class="sxs-lookup"><span data-stu-id="22c14-268">Explicit interface member implementations</span></span>

<span data-ttu-id="22c14-269">为了实现接口，类或结构可以声明***显式接口成员实现代码***。</span><span class="sxs-lookup"><span data-stu-id="22c14-269">For purposes of implementing interfaces, a class or struct may declare ***explicit interface member implementations***.</span></span> <span data-ttu-id="22c14-270">显式接口成员实现是引用完全限定的接口成员名称的方法、 属性、 事件或索引器声明。</span><span class="sxs-lookup"><span data-stu-id="22c14-270">An explicit interface member implementation is a method, property, event, or indexer declaration that references a fully qualified interface member name.</span></span> <span data-ttu-id="22c14-271">例如</span><span class="sxs-lookup"><span data-stu-id="22c14-271">For example</span></span>
```csharp
interface IList<T>
{
    T[] GetElements();
}

interface IDictionary<K,V>
{
    V this[K key];
    void Add(K key, V value);
}

class List<T>: IList<T>, IDictionary<int,T>
{
    T[] IList<T>.GetElements() {...}
    T IDictionary<int,T>.this[int index] {...}
    void IDictionary<int,T>.Add(int index, T value) {...}
}
```

<span data-ttu-id="22c14-272">这里`IDictionary<int,T>.this`和`IDictionary<int,T>.Add`是显式接口成员实现。</span><span class="sxs-lookup"><span data-stu-id="22c14-272">Here `IDictionary<int,T>.this` and `IDictionary<int,T>.Add` are explicit interface member implementations.</span></span>

<span data-ttu-id="22c14-273">在某些情况下，接口成员的名称可能不是适用于实现的类，将在其中用例的接口成员可能会实现使用显式接口成员实现。</span><span class="sxs-lookup"><span data-stu-id="22c14-273">In some cases, the name of an interface member may not be appropriate for the implementing class, in which case the interface member may be implemented using explicit interface member implementation.</span></span> <span data-ttu-id="22c14-274">类实现文件抽象，例如，可能会实现`Close`成员函数，释放文件资源的影响并实现了`Dispose`方法的`IDisposable`接口使用显式接口成员的实现：</span><span class="sxs-lookup"><span data-stu-id="22c14-274">A class implementing a file abstraction, for example, would likely implement a `Close` member function that has the effect of releasing the file resource, and implement the `Dispose` method of the `IDisposable` interface using explicit interface member implementation:</span></span>
```csharp
interface IDisposable
{
    void Dispose();
}

class MyFile: IDisposable
{
    void IDisposable.Dispose() {
        Close();
    }

    public void Close() {
        // Do what's necessary to close the file
        System.GC.SuppressFinalize(this);
    }
}
```

<span data-ttu-id="22c14-275">不能通过在方法调用、 属性访问或索引器访问其完全限定名称访问的显式接口成员实现。</span><span class="sxs-lookup"><span data-stu-id="22c14-275">It is not possible to access an explicit interface member implementation through its fully qualified name in a method invocation, property access, or indexer access.</span></span> <span data-ttu-id="22c14-276">显式接口成员实现只能通过接口实例访问，并且在这种情况下引用只需通过其成员名称。</span><span class="sxs-lookup"><span data-stu-id="22c14-276">An explicit interface member implementation can only be accessed through an interface instance, and is in that case referenced simply by its member name.</span></span>

<span data-ttu-id="22c14-277">它是显式接口成员实现包括访问修饰符的编译时错误，它会导致编译时错误包含修饰符`abstract`， `virtual`， `override`，或`static`。</span><span class="sxs-lookup"><span data-stu-id="22c14-277">It is a compile-time error for an explicit interface member implementation to include access modifiers, and it is a compile-time error to include the modifiers `abstract`, `virtual`, `override`, or `static`.</span></span>

<span data-ttu-id="22c14-278">显式接口成员实现具有不同的可访问性特征与其他成员。</span><span class="sxs-lookup"><span data-stu-id="22c14-278">Explicit interface member implementations have different accessibility characteristics than other members.</span></span> <span data-ttu-id="22c14-279">由于显式接口成员实现代码永远不能通过在方法调用或属性访问其完全限定名称，它们是在专用的意义上。</span><span class="sxs-lookup"><span data-stu-id="22c14-279">Because explicit interface member implementations are never accessible through their fully qualified name in a method invocation or a property access, they are in a sense private.</span></span> <span data-ttu-id="22c14-280">但是，由于它们可以通过将接口实例访问，因此可以在某种意义上也是公共。</span><span class="sxs-lookup"><span data-stu-id="22c14-280">However, since they can be accessed through an interface instance, they are in a sense also public.</span></span>

<span data-ttu-id="22c14-281">显式接口成员实现代码有两个主要用途：</span><span class="sxs-lookup"><span data-stu-id="22c14-281">Explicit interface member implementations serve two primary purposes:</span></span>

*  <span data-ttu-id="22c14-282">显式接口成员实现代码不是通过类或结构的实例可访问的因为它们允许接口实现，若要从公共接口的类或结构中排除。</span><span class="sxs-lookup"><span data-stu-id="22c14-282">Because explicit interface member implementations are not accessible through class or struct instances, they allow interface implementations to be excluded from the public interface of a class or struct.</span></span> <span data-ttu-id="22c14-283">此功能特别有用，当类或结构实现此类或结构的使用者不感兴趣的内部接口。</span><span class="sxs-lookup"><span data-stu-id="22c14-283">This is particularly useful when a class or struct implements an internal interface that is of no interest to a consumer of that class or struct.</span></span>
*  <span data-ttu-id="22c14-284">显式接口成员实现允许消除具有相同签名的接口成员的歧义。</span><span class="sxs-lookup"><span data-stu-id="22c14-284">Explicit interface member implementations allow disambiguation of interface members with the same signature.</span></span> <span data-ttu-id="22c14-285">没有显式接口成员实现代码是类或结构不可能具有不同的实现的接口具有相同签名的成员，并作为返回类型，它是类或结构不可能具有任何实现在所有接口成员具有相同的签名，但使用不同的返回类型。</span><span class="sxs-lookup"><span data-stu-id="22c14-285">Without explicit interface member implementations it would be impossible for a class or struct to have different implementations of interface members with the same signature and return type, as would it be impossible for a class or struct to have any implementation at all of interface members with the same signature but with different return types.</span></span>

<span data-ttu-id="22c14-286">有效的显式接口成员实现，类或结构必须命名其基类列表中包含其完全限定的名称、 类型和参数类型完全匹配的显式接口成员的成员的接口实现。</span><span class="sxs-lookup"><span data-stu-id="22c14-286">For an explicit interface member implementation to be valid, the class or struct must name an interface in its base class list that contains a member whose fully qualified name, type, and parameter types exactly match those of the explicit interface member implementation.</span></span> <span data-ttu-id="22c14-287">因此，在下面的类</span><span class="sxs-lookup"><span data-stu-id="22c14-287">Thus, in the following class</span></span>
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
    int IComparable.CompareTo(object other) {...}    // invalid
}
```
<span data-ttu-id="22c14-288">声明`IComparable.CompareTo`会导致编译时错误，因为`IComparable`的基类列表中未列出`Shape`并不是基接口的`ICloneable`。</span><span class="sxs-lookup"><span data-stu-id="22c14-288">the declaration of `IComparable.CompareTo` results in a compile-time error because `IComparable` is not listed in the base class list of `Shape` and is not a base interface of `ICloneable`.</span></span> <span data-ttu-id="22c14-289">同样，在声明中</span><span class="sxs-lookup"><span data-stu-id="22c14-289">Likewise, in the declarations</span></span>
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
}

class Ellipse: Shape
{
    object ICloneable.Clone() {...}    // invalid
}
```
<span data-ttu-id="22c14-290">声明`ICloneable.Clone`中`Ellipse`导致编译时错误，因为`ICloneable`的基类列表中未显式列出`Ellipse`。</span><span class="sxs-lookup"><span data-stu-id="22c14-290">the declaration of `ICloneable.Clone` in `Ellipse` results in a compile-time error because `ICloneable` is not explicitly listed in the base class list of `Ellipse`.</span></span>

<span data-ttu-id="22c14-291">接口成员的完全限定的名称必须引用在其中声明成员的接口。</span><span class="sxs-lookup"><span data-stu-id="22c14-291">The fully qualified name of an interface member must reference the interface in which the member was declared.</span></span> <span data-ttu-id="22c14-292">因此，在下面的声明</span><span class="sxs-lookup"><span data-stu-id="22c14-292">Thus, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
}
```
<span data-ttu-id="22c14-293">显式接口成员实现的`Paint`必须编写为`IControl.Paint`。</span><span class="sxs-lookup"><span data-stu-id="22c14-293">the explicit interface member implementation of `Paint` must be written as `IControl.Paint`.</span></span>

### <a name="uniqueness-of-implemented-interfaces"></a><span data-ttu-id="22c14-294">实现的接口的唯一性</span><span class="sxs-lookup"><span data-stu-id="22c14-294">Uniqueness of implemented interfaces</span></span>

<span data-ttu-id="22c14-295">实现泛型类型声明的接口必须保持唯一的所有可能的构造类型。</span><span class="sxs-lookup"><span data-stu-id="22c14-295">The interfaces implemented by a generic type declaration must remain unique for all possible constructed types.</span></span> <span data-ttu-id="22c14-296">如果没有此规则，将无法确定要调用的某些构造的类型的正确方法。</span><span class="sxs-lookup"><span data-stu-id="22c14-296">Without this rule, it would be impossible to determine the correct method to call for certain constructed types.</span></span> <span data-ttu-id="22c14-297">例如，假设一个泛型类声明了允许按以下方式编写：</span><span class="sxs-lookup"><span data-stu-id="22c14-297">For example, suppose a generic class declaration were permitted to be written as follows:</span></span>
```csharp
interface I<T>
{
    void F();
}

class X<U,V>: I<U>, I<V>                    // Error: I<U> and I<V> conflict
{
    void I<U>.F() {...}
    void I<V>.F() {...}
}
```

<span data-ttu-id="22c14-298">如果允许这样，它无法确定要在以下情况下执行的代码：</span><span class="sxs-lookup"><span data-stu-id="22c14-298">Were this permitted, it would be impossible to determine which code to execute in the following case:</span></span>
```csharp
I<int> x = new X<int,int>();
x.F();
```

<span data-ttu-id="22c14-299">若要确定泛型类型声明的接口列表是否有效，请执行以下步骤：</span><span class="sxs-lookup"><span data-stu-id="22c14-299">To determine if the interface list of a generic type declaration is valid, the following steps are performed:</span></span>

*  <span data-ttu-id="22c14-300">让`L`是直接在泛型类、 结构或接口声明中指定的接口的列表`C`。</span><span class="sxs-lookup"><span data-stu-id="22c14-300">Let `L` be the list of interfaces directly specified in a generic class, struct, or interface declaration `C`.</span></span>
*  <span data-ttu-id="22c14-301">将添加到`L`任何基接口的接口已在`L`。</span><span class="sxs-lookup"><span data-stu-id="22c14-301">Add to `L` any base interfaces of the interfaces already in `L`.</span></span>
*  <span data-ttu-id="22c14-302">删除任何重复项`L`。</span><span class="sxs-lookup"><span data-stu-id="22c14-302">Remove any duplicates from `L`.</span></span>
*  <span data-ttu-id="22c14-303">如果任何可能构造从创建类型`C`将类型参数会代入后`L`，会导致在两个接口`L`完全相同，然后的声明`C`无效。</span><span class="sxs-lookup"><span data-stu-id="22c14-303">If any possible constructed type created from `C` would, after type arguments are substituted into `L`, cause two interfaces in `L` to be identical, then the declaration of `C` is invalid.</span></span> <span data-ttu-id="22c14-304">确定所有可能的构造的类型时，不会考虑约束声明。</span><span class="sxs-lookup"><span data-stu-id="22c14-304">Constraint declarations are not considered when determining all possible constructed types.</span></span>

<span data-ttu-id="22c14-305">在类声明中`X`接口列表上方`L`组成`I<U>`和`I<V>`。</span><span class="sxs-lookup"><span data-stu-id="22c14-305">In the class declaration `X` above, the interface list `L` consists of `I<U>` and `I<V>`.</span></span> <span data-ttu-id="22c14-306">声明无效，因为任何构造具有类型`U`和`V`正在相同的类型会导致这两个接口是相同的类型。</span><span class="sxs-lookup"><span data-stu-id="22c14-306">The declaration is invalid because any constructed type with `U` and `V` being the same type would cause these two interfaces to be identical types.</span></span>

<span data-ttu-id="22c14-307">此外，可以在不同的继承级别统一上指定的接口：</span><span class="sxs-lookup"><span data-stu-id="22c14-307">It is possible for interfaces specified at different inheritance levels to unify:</span></span>
```csharp
interface I<T>
{
    void F();
}

class Base<U>: I<U>
{
    void I<U>.F() {...}
}

class Derived<U,V>: Base<U>, I<V>    // Ok
{
    void I<V>.F() {...}
}
```

<span data-ttu-id="22c14-308">此代码是有效的即使`Derived<U,V>`同时实现`I<U>`和`I<V>`。</span><span class="sxs-lookup"><span data-stu-id="22c14-308">This code is valid even though `Derived<U,V>` implements both `I<U>` and `I<V>`.</span></span> <span data-ttu-id="22c14-309">代码</span><span class="sxs-lookup"><span data-stu-id="22c14-309">The code</span></span>
```csharp
I<int> x = new Derived<int,int>();
x.F();
```
<span data-ttu-id="22c14-310">调用中的方法`Derived`，因为`Derived<int,int>`有效地重新实现`I<int>`([接口重新实现](interfaces.md#interface-re-implementation))。</span><span class="sxs-lookup"><span data-stu-id="22c14-310">invokes the method in `Derived`, since `Derived<int,int>` effectively re-implements `I<int>` ([Interface re-implementation](interfaces.md#interface-re-implementation)).</span></span>

### <a name="implementation-of-generic-methods"></a><span data-ttu-id="22c14-311">泛型方法的实现</span><span class="sxs-lookup"><span data-stu-id="22c14-311">Implementation of generic methods</span></span>

<span data-ttu-id="22c14-312">当泛型方法隐式实现接口方法，提供每个方法类型参数必须是两个声明中的等效 （在任何接口类型参数替换为适当的类型参数），其中的约束方法类型参数进行标识的序号位置，从左到右。</span><span class="sxs-lookup"><span data-stu-id="22c14-312">When a generic method implicitly implements an interface method, the constraints given for each method type parameter must be equivalent in both declarations (after any interface type parameters are replaced with the appropriate type arguments), where method type parameters are identified by ordinal positions, left to right.</span></span>

<span data-ttu-id="22c14-313">当泛型方法显式实现接口方法时，但是，实现方法上不允许使用任何约束。</span><span class="sxs-lookup"><span data-stu-id="22c14-313">When a generic method explicitly implements an interface method, however, no constraints are allowed on the implementing method.</span></span> <span data-ttu-id="22c14-314">相反，约束被继承的接口方法</span><span class="sxs-lookup"><span data-stu-id="22c14-314">Instead, the constraints are inherited from the interface method</span></span>

```csharp
interface I<A,B,C>
{
    void F<T>(T t) where T: A;
    void G<T>(T t) where T: B;
    void H<T>(T t) where T: C;
}

class C: I<object,C,string>
{
    public void F<T>(T t) {...}                    // Ok
    public void G<T>(T t) where T: C {...}         // Ok
    public void H<T>(T t) where T: string {...}    // Error
}
```

<span data-ttu-id="22c14-315">该方法`C.F<T>`隐式实现`I<object,C,string>.F<T>`。</span><span class="sxs-lookup"><span data-stu-id="22c14-315">The method `C.F<T>` implicitly implements `I<object,C,string>.F<T>`.</span></span> <span data-ttu-id="22c14-316">在这种情况下，`C.F<T>`不是需要 （也不允许） 来指定该约束`T:object`由于`object`是隐式所有类型参数约束。</span><span class="sxs-lookup"><span data-stu-id="22c14-316">In this case, `C.F<T>` is not required (nor permitted) to specify the constraint `T:object` since `object` is an implicit constraint on all type parameters.</span></span> <span data-ttu-id="22c14-317">该方法`C.G<T>`隐式实现`I<object,C,string>.G<T>`由于约束匹配那些在界面中，在接口类型参数替换为相应类型参数之后。</span><span class="sxs-lookup"><span data-stu-id="22c14-317">The method `C.G<T>` implicitly implements `I<object,C,string>.G<T>` because the constraints match those in the interface, after the interface type parameters are replaced with the corresponding type arguments.</span></span> <span data-ttu-id="22c14-318">方法的约束`C.H<T>`是一个错误因为密封类型 (`string`这种情况下) 不能用作约束。</span><span class="sxs-lookup"><span data-stu-id="22c14-318">The constraint for method `C.H<T>` is an error because sealed types (`string` in this case) cannot be used as constraints.</span></span> <span data-ttu-id="22c14-319">由于隐式接口方法实现的约束需要匹配省略该约束也将是一个错误。</span><span class="sxs-lookup"><span data-stu-id="22c14-319">Omitting the constraint would also be an error since constraints of implicit interface method implementations are required to match.</span></span> <span data-ttu-id="22c14-320">因此，就无法隐式实现`I<object,C,string>.H<T>`。</span><span class="sxs-lookup"><span data-stu-id="22c14-320">Thus, it is impossible to implicitly implement `I<object,C,string>.H<T>`.</span></span> <span data-ttu-id="22c14-321">仅可以使用显式接口成员实现来实现此接口方法：</span><span class="sxs-lookup"><span data-stu-id="22c14-321">This interface method can only be implemented using an explicit interface member implementation:</span></span>
```csharp
class C: I<object,C,string>
{
    ...

    public void H<U>(U u) where U: class {...}

    void I<object,C,string>.H<T>(T t) {
        string s = t;    // Ok
        H<T>(t);
    }
}
```

<span data-ttu-id="22c14-322">在此示例中，显式接口成员实现调用具有更严格弱约束的公共方法。</span><span class="sxs-lookup"><span data-stu-id="22c14-322">In this example, the explicit interface member implementation invokes a public method having strictly weaker constraints.</span></span> <span data-ttu-id="22c14-323">请注意，从赋值`t`到`s`无效，因为`T`继承的约束`T:string`，即使此约束不可表示源代码中。</span><span class="sxs-lookup"><span data-stu-id="22c14-323">Note that the assignment from `t` to `s` is valid since `T` inherits a constraint of `T:string`, even though this constraint is not expressible in source code.</span></span>

### <a name="interface-mapping"></a><span data-ttu-id="22c14-324">接口映射</span><span class="sxs-lookup"><span data-stu-id="22c14-324">Interface mapping</span></span>

<span data-ttu-id="22c14-325">类或结构必须提供的接口的类或结构的基列表中列出的所有成员的实现。</span><span class="sxs-lookup"><span data-stu-id="22c14-325">A class or struct must provide implementations of all members of the interfaces that are listed in the base class list of the class or struct.</span></span> <span data-ttu-id="22c14-326">查找实现接口成员的实现的类或结构中的过程被称为***接口映射***。</span><span class="sxs-lookup"><span data-stu-id="22c14-326">The process of locating implementations of interface members in an implementing class or struct is known as ***interface mapping***.</span></span>

<span data-ttu-id="22c14-327">类或结构的接口映射`C`为每个接口的基的类列表中指定的每个成员定位实现`C`。</span><span class="sxs-lookup"><span data-stu-id="22c14-327">Interface mapping for a class or struct `C` locates an implementation for each member of each interface specified in the base class list of `C`.</span></span> <span data-ttu-id="22c14-328">特定的接口成员的实现`I.M`，其中`I`是在其中的接口成员`M`声明，确定通过检查每个类或结构`S`，从开始`C`和为每个后续基类重复`C`，直到找到匹配项：</span><span class="sxs-lookup"><span data-stu-id="22c14-328">The implementation of a particular interface member `I.M`, where `I` is the interface in which the member `M` is declared, is determined by examining each class or struct `S`, starting with `C` and repeating for each successive base class of `C`, until a match is located:</span></span>

*  <span data-ttu-id="22c14-329">如果`S`包含声明的匹配的显式接口成员实现`I`并`M`，则此成员是实现`I.M`。</span><span class="sxs-lookup"><span data-stu-id="22c14-329">If `S` contains a declaration of an explicit interface member implementation that matches `I` and `M`, then this member is the implementation of `I.M`.</span></span>
*  <span data-ttu-id="22c14-330">否则为如果`S`包含非静态公共成员相匹配的声明`M`，则此成员是实现`I.M`。</span><span class="sxs-lookup"><span data-stu-id="22c14-330">Otherwise, if `S` contains a declaration of a non-static public member that matches `M`, then this member is the implementation of `I.M`.</span></span> <span data-ttu-id="22c14-331">如果多个匹配项有一个成员，它是未指定哪个成员是实现`I.M`。</span><span class="sxs-lookup"><span data-stu-id="22c14-331">If more than one member matches, it is unspecified which member is the implementation of `I.M`.</span></span> <span data-ttu-id="22c14-332">这种情况下只能如果`S`为构造的类型，其中泛型类型中声明的两个成员具有不同的签名，但类型参数可以让它们的签名完全相同。</span><span class="sxs-lookup"><span data-stu-id="22c14-332">This situation can only occur if `S` is a constructed type where the two members as declared in the generic type have different signatures, but the type arguments make their signatures identical.</span></span>

<span data-ttu-id="22c14-333">如果实现找不到指定的基类列表中的所有接口的所有成员会发生编译时错误`C`。</span><span class="sxs-lookup"><span data-stu-id="22c14-333">A compile-time error occurs if implementations cannot be located for all members of all interfaces specified in the base class list of `C`.</span></span> <span data-ttu-id="22c14-334">请注意接口的成员，包括那些继承自基接口的成员。</span><span class="sxs-lookup"><span data-stu-id="22c14-334">Note that the members of an interface include those members that are inherited from base interfaces.</span></span>

<span data-ttu-id="22c14-335">有关接口映射，类成员的含义`A`与接口成员匹配`B`时：</span><span class="sxs-lookup"><span data-stu-id="22c14-335">For purposes of interface mapping, a class member `A` matches an interface member `B` when:</span></span>

*  <span data-ttu-id="22c14-336">`A` 并`B`方法，和名称、 类型和形参表的`A`和`B`完全相同。</span><span class="sxs-lookup"><span data-stu-id="22c14-336">`A` and `B` are methods, and the name, type, and formal parameter lists of `A` and `B` are identical.</span></span>
*  <span data-ttu-id="22c14-337">`A` 和`B`是属性、 名称和类型的`A`和`B`是相同的并且`A`具有相同的访问器作为`B`(`A`允许具有其他访问器，如果它不是显式接口成员实现）。</span><span class="sxs-lookup"><span data-stu-id="22c14-337">`A` and `B` are properties, the name and type of `A` and `B` are identical, and `A` has the same accessors as `B` (`A` is permitted to have additional accessors if it is not an explicit interface member implementation).</span></span>
*  <span data-ttu-id="22c14-338">`A` 并`B`是事件，以及名称和类型的`A`和`B`完全相同。</span><span class="sxs-lookup"><span data-stu-id="22c14-338">`A` and `B` are events, and the name and type of `A` and `B` are identical.</span></span>
*  <span data-ttu-id="22c14-339">`A` 和`B`是索引器、 类型和形参列表`A`并`B`是相同的和`A`具有相同的访问器作为`B`(`A`允许具有其他访问器，如果不是显式接口成员实现）。</span><span class="sxs-lookup"><span data-stu-id="22c14-339">`A` and `B` are indexers, the type and formal parameter lists of `A` and `B` are identical, and `A` has the same accessors as `B` (`A` is permitted to have additional accessors if it is not an explicit interface member implementation).</span></span>

<span data-ttu-id="22c14-340">值得注意的接口映射算法含义是：</span><span class="sxs-lookup"><span data-stu-id="22c14-340">Notable implications of the interface mapping algorithm are:</span></span>

*  <span data-ttu-id="22c14-341">确定实现接口成员的类或结构成员时，显式接口成员实现代码优先于同一类或结构中的其他成员。</span><span class="sxs-lookup"><span data-stu-id="22c14-341">Explicit interface member implementations take precedence over other members in the same class or struct when determining the class or struct member that implements an interface member.</span></span>
*  <span data-ttu-id="22c14-342">非公共既不静态成员参与接口映射。</span><span class="sxs-lookup"><span data-stu-id="22c14-342">Neither non-public nor static members participate in interface mapping.</span></span>

<span data-ttu-id="22c14-343">在示例</span><span class="sxs-lookup"><span data-stu-id="22c14-343">In the example</span></span>
```csharp
interface ICloneable
{
    object Clone();
}

class C: ICloneable
{
    object ICloneable.Clone() {...}
    public object Clone() {...}
}
```
<span data-ttu-id="22c14-344">`ICloneable.Clone`的成员`C`的实现变得`Clone`中`ICloneable`因为显式接口成员实现代码优先于其他成员。</span><span class="sxs-lookup"><span data-stu-id="22c14-344">the `ICloneable.Clone` member of `C` becomes the implementation of `Clone` in `ICloneable` because explicit interface member implementations take precedence over other members.</span></span>

<span data-ttu-id="22c14-345">如果类或结构实现两个或多个接口包含具有相同的名称、 类型和参数类型的成员，则可以将每个到单个类或结构成员上这些接口成员。</span><span class="sxs-lookup"><span data-stu-id="22c14-345">If a class or struct implements two or more interfaces containing a member with the same name, type, and parameter types, it is possible to map each of those interface members onto a single class or struct member.</span></span> <span data-ttu-id="22c14-346">例如</span><span class="sxs-lookup"><span data-stu-id="22c14-346">For example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface IForm
{
    void Paint();
}

class Page: IControl, IForm
{
    public void Paint() {...}
}
```

<span data-ttu-id="22c14-347">在这里，`Paint`两个方法`IControl`并`IForm`映射到`Paint`中的方法`Page`。</span><span class="sxs-lookup"><span data-stu-id="22c14-347">Here, the `Paint` methods of both `IControl` and `IForm` are mapped onto the `Paint` method in `Page`.</span></span> <span data-ttu-id="22c14-348">当然也很可能有两个方法的单独的显式接口成员实现代码。</span><span class="sxs-lookup"><span data-stu-id="22c14-348">It is of course also possible to have separate explicit interface member implementations for the two methods.</span></span>

<span data-ttu-id="22c14-349">如果类或结构实现包含隐藏的成员的接口，然后某些成员必须通过显式接口成员实现来实现。</span><span class="sxs-lookup"><span data-stu-id="22c14-349">If a class or struct implements an interface that contains hidden members, then some members must necessarily be implemented through explicit interface member implementations.</span></span> <span data-ttu-id="22c14-350">例如</span><span class="sxs-lookup"><span data-stu-id="22c14-350">For example</span></span>
```csharp
interface IBase
{
    int P { get; }
}

interface IDerived: IBase
{
    new int P();
}
```

<span data-ttu-id="22c14-351">此接口的实现将需要至少一个显式接口成员实现，并将采用以下形式之一</span><span class="sxs-lookup"><span data-stu-id="22c14-351">An implementation of this interface would require at least one explicit interface member implementation, and would take one of the following forms</span></span>
```csharp
class C: IDerived
{
    int IBase.P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    public int P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    int IBase.P { get {...} }
    public int P() {...}
}
```

<span data-ttu-id="22c14-352">当一个类实现多个具有相同的基接口的接口时，可以只有一个基接口的实现。</span><span class="sxs-lookup"><span data-stu-id="22c14-352">When a class implements multiple interfaces that have the same base interface, there can be only one implementation of the base interface.</span></span> <span data-ttu-id="22c14-353">在示例</span><span class="sxs-lookup"><span data-stu-id="22c14-353">In the example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

class ComboBox: IControl, ITextBox, IListBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
    void IListBox.SetItems(string[] items) {...}
}
```
<span data-ttu-id="22c14-354">不能具有单独实现`IControl`在基的类列表中，名为`IControl`由继承`ITextBox`，和`IControl`由继承`IListBox`。</span><span class="sxs-lookup"><span data-stu-id="22c14-354">it is not possible to have separate implementations for the `IControl` named in the base class list, the `IControl` inherited by `ITextBox`, and the `IControl` inherited by `IListBox`.</span></span> <span data-ttu-id="22c14-355">实际上，是标识的没有不同，这些接口的概念。</span><span class="sxs-lookup"><span data-stu-id="22c14-355">Indeed, there is no notion of a separate identity for these interfaces.</span></span> <span data-ttu-id="22c14-356">相反，实现`ITextBox`和`IListBox`共享的相同实现`IControl`，和`ComboBox`只需实现三个接口，被视为`IControl`， `ITextBox`，和`IListBox`。</span><span class="sxs-lookup"><span data-stu-id="22c14-356">Rather, the implementations of `ITextBox` and `IListBox` share the same implementation of `IControl`, and `ComboBox` is simply considered to implement three interfaces, `IControl`, `ITextBox`, and `IListBox`.</span></span>

<span data-ttu-id="22c14-357">基类的成员参与接口映射。</span><span class="sxs-lookup"><span data-stu-id="22c14-357">The members of a base class participate in interface mapping.</span></span> <span data-ttu-id="22c14-358">在示例</span><span class="sxs-lookup"><span data-stu-id="22c14-358">In the example</span></span>
```csharp
interface Interface1
{
    void F();
}

class Class1
{
    public void F() {}
    public void G() {}
}

class Class2: Class1, Interface1
{
    new public void G() {}
}
```
<span data-ttu-id="22c14-359">该方法`F`中`Class1`中使用`Class2`的实现`Interface1`。</span><span class="sxs-lookup"><span data-stu-id="22c14-359">the method `F` in `Class1` is used in `Class2`'s implementation of `Interface1`.</span></span>

### <a name="interface-implementation-inheritance"></a><span data-ttu-id="22c14-360">接口实现继承</span><span class="sxs-lookup"><span data-stu-id="22c14-360">Interface implementation inheritance</span></span>

<span data-ttu-id="22c14-361">一个类继承其基类所提供的所有接口实现。</span><span class="sxs-lookup"><span data-stu-id="22c14-361">A class inherits all interface implementations provided by its base classes.</span></span>

<span data-ttu-id="22c14-362">无需显式***重新实现***接口，派生的类不能以任何方式更改它从其基类继承的接口映射。</span><span class="sxs-lookup"><span data-stu-id="22c14-362">Without explicitly ***re-implementing*** an interface, a derived class cannot in any way alter the interface mappings it inherits from its base classes.</span></span> <span data-ttu-id="22c14-363">例如，在声明中</span><span class="sxs-lookup"><span data-stu-id="22c14-363">For example, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public void Paint() {...}
}

class TextBox: Control
{
    new public void Paint() {...}
}
```
<span data-ttu-id="22c14-364">`Paint`中的方法`TextBox`隐藏`Paint`中的方法`Control`，但它不会更改的映射`Control.Paint`拖动到`IControl.Paint`，以及对调用`Paint`通过类实例和接口实例将具有以下效果</span><span class="sxs-lookup"><span data-stu-id="22c14-364">the `Paint` method in `TextBox` hides the `Paint` method in `Control`, but it does not alter the mapping of `Control.Paint` onto `IControl.Paint`, and calls to `Paint` through class instances and interface instances will have the following effects</span></span>
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes Control.Paint();
```

<span data-ttu-id="22c14-365">但是，当接口方法映射到类中的虚方法时，就可以为派生类重写虚拟方法并更改接口的实现。</span><span class="sxs-lookup"><span data-stu-id="22c14-365">However, when an interface method is mapped onto a virtual method in a class, it is possible for derived classes to override the virtual method and alter the implementation of the interface.</span></span> <span data-ttu-id="22c14-366">例如，重写到上面的声明</span><span class="sxs-lookup"><span data-stu-id="22c14-366">For example, rewriting the declarations above to</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public virtual void Paint() {...}
}

class TextBox: Control
{
    public override void Paint() {...}
}
```
<span data-ttu-id="22c14-367">现在观察到以下影响</span><span class="sxs-lookup"><span data-stu-id="22c14-367">the following effects will now be observed</span></span>
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes TextBox.Paint();
```

<span data-ttu-id="22c14-368">显式接口成员实现代码不能声明为虚拟函数，因为它不能重写显式接口成员实现。</span><span class="sxs-lookup"><span data-stu-id="22c14-368">Since explicit interface member implementations cannot be declared virtual, it is not possible to override an explicit interface member implementation.</span></span> <span data-ttu-id="22c14-369">但是，它是完全有效的显式接口成员实现调用另一个方法，并且，其他方法可以声明为虚拟函数以允许派生类重写它。</span><span class="sxs-lookup"><span data-stu-id="22c14-369">However, it is perfectly valid for an explicit interface member implementation to call another method, and that other method can be declared virtual to allow derived classes to override it.</span></span> <span data-ttu-id="22c14-370">例如</span><span class="sxs-lookup"><span data-stu-id="22c14-370">For example</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() { PaintControl(); }
    protected virtual void PaintControl() {...}
}

class TextBox: Control
{
    protected override void PaintControl() {...}
}
```

<span data-ttu-id="22c14-371">在这里，类派生自`Control`可以专用化的实现`IControl.Paint`通过重写`PaintControl`方法。</span><span class="sxs-lookup"><span data-stu-id="22c14-371">Here, classes derived from `Control` can specialize the implementation of `IControl.Paint` by overriding the `PaintControl` method.</span></span>

### <a name="interface-re-implementation"></a><span data-ttu-id="22c14-372">接口重新实现</span><span class="sxs-lookup"><span data-stu-id="22c14-372">Interface re-implementation</span></span>

<span data-ttu-id="22c14-373">继承的接口实现的类允许***重新实现***通过基类列表中包含的接口。</span><span class="sxs-lookup"><span data-stu-id="22c14-373">A class that inherits an interface implementation is permitted to ***re-implement*** the interface by including it in the base class list.</span></span>

<span data-ttu-id="22c14-374">接口重新实现遵循完全相同接口映射的规则的初始实现的接口。</span><span class="sxs-lookup"><span data-stu-id="22c14-374">A re-implementation of an interface follows exactly the same interface mapping rules as an initial implementation of an interface.</span></span> <span data-ttu-id="22c14-375">因此，继承的接口映射不起作用的接口映射上建立的重新实现的接口。</span><span class="sxs-lookup"><span data-stu-id="22c14-375">Thus, the inherited interface mapping has no effect whatsoever on the interface mapping established for the re-implementation of the interface.</span></span> <span data-ttu-id="22c14-376">例如，在声明中</span><span class="sxs-lookup"><span data-stu-id="22c14-376">For example, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() {...}
}

class MyControl: Control, IControl
{
    public void Paint() {}
}
```
<span data-ttu-id="22c14-377">这一事实，`Control`映射`IControl.Paint`拖动到`Control.IControl.Paint`不会影响在重新实现`MyControl`，映射`IControl.Paint`拖到`MyControl.Paint`。</span><span class="sxs-lookup"><span data-stu-id="22c14-377">the fact that `Control` maps `IControl.Paint` onto `Control.IControl.Paint` doesn't affect the re-implementation in `MyControl`, which maps `IControl.Paint` onto `MyControl.Paint`.</span></span>

<span data-ttu-id="22c14-378">继承的公共成员声明和继承的显式接口成员声明参与重新实现的接口的接口映射过程。</span><span class="sxs-lookup"><span data-stu-id="22c14-378">Inherited public member declarations and inherited explicit interface member declarations participate in the interface mapping process for re-implemented interfaces.</span></span> <span data-ttu-id="22c14-379">例如</span><span class="sxs-lookup"><span data-stu-id="22c14-379">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
    void H();
    void I();
}

class Base: IMethods
{
    void IMethods.F() {}
    void IMethods.G() {}
    public void H() {}
    public void I() {}
}

class Derived: Base, IMethods
{
    public void F() {}
    void IMethods.H() {}
}
```

<span data-ttu-id="22c14-380">此处，实现`IMethods`中`Derived`映射到的接口方法`Derived.F`， `Base.IMethods.G`， `Derived.IMethods.H`，并`Base.I`。</span><span class="sxs-lookup"><span data-stu-id="22c14-380">Here, the implementation of `IMethods` in `Derived` maps the interface methods onto `Derived.F`, `Base.IMethods.G`, `Derived.IMethods.H`, and `Base.I`.</span></span>

<span data-ttu-id="22c14-381">当类实现一个接口，它隐式还实现了所有该接口的基接口。</span><span class="sxs-lookup"><span data-stu-id="22c14-381">When a class implements an interface, it implicitly also implements all of that interface's base interfaces.</span></span> <span data-ttu-id="22c14-382">同样，一个接口重新实现还隐式是重新实现的所有接口的基接口。</span><span class="sxs-lookup"><span data-stu-id="22c14-382">Likewise, a re-implementation of an interface is also implicitly a re-implementation of all of the interface's base interfaces.</span></span> <span data-ttu-id="22c14-383">例如</span><span class="sxs-lookup"><span data-stu-id="22c14-383">For example</span></span>
```csharp
interface IBase
{
    void F();
}

interface IDerived: IBase
{
    void G();
}

class C: IDerived
{
    void IBase.F() {...}
    void IDerived.G() {...}
}

class D: C, IDerived
{
    public void F() {...}
    public void G() {...}
}
```

<span data-ttu-id="22c14-384">此处，重新实现`IDerived`还重新实现`IBase`映射`IBase.F`拖动到`D.F`。</span><span class="sxs-lookup"><span data-stu-id="22c14-384">Here, the re-implementation of `IDerived` also re-implements `IBase`, mapping `IBase.F` onto `D.F`.</span></span>

### <a name="abstract-classes-and-interfaces"></a><span data-ttu-id="22c14-385">抽象类和接口</span><span class="sxs-lookup"><span data-stu-id="22c14-385">Abstract classes and interfaces</span></span>

<span data-ttu-id="22c14-386">就像一个非抽象类，抽象类必须提供的接口的类的基类列表中列出的所有成员的实现。</span><span class="sxs-lookup"><span data-stu-id="22c14-386">Like a non-abstract class, an abstract class must provide implementations of all members of the interfaces that are listed in the base class list of the class.</span></span> <span data-ttu-id="22c14-387">但是，一个抽象类被允许接口方法映射到抽象方法。</span><span class="sxs-lookup"><span data-stu-id="22c14-387">However, an abstract class is permitted to map interface methods onto abstract methods.</span></span> <span data-ttu-id="22c14-388">例如</span><span class="sxs-lookup"><span data-stu-id="22c14-388">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    public abstract void F();
    public abstract void G();
}
```

<span data-ttu-id="22c14-389">此处，实现`IMethods`映射`F`并`G`到抽象方法，其必须重写的派生自非抽象类中`C`。</span><span class="sxs-lookup"><span data-stu-id="22c14-389">Here, the implementation of `IMethods` maps `F` and `G` onto abstract methods, which must be overridden in non-abstract classes that derive from `C`.</span></span>

<span data-ttu-id="22c14-390">请注意，显式接口成员实现代码不能为抽象的但当然允许显式接口成员实现调用抽象方法。</span><span class="sxs-lookup"><span data-stu-id="22c14-390">Note that explicit interface member implementations cannot be abstract, but explicit interface member implementations are of course permitted to call abstract methods.</span></span> <span data-ttu-id="22c14-391">例如</span><span class="sxs-lookup"><span data-stu-id="22c14-391">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    void IMethods.F() { FF(); }
    void IMethods.G() { GG(); }
    protected abstract void FF();
    protected abstract void GG();
}
```

<span data-ttu-id="22c14-392">此处，派生自非抽象类`C`所需重写`FF`并`GG`，从而提供的实际实现`IMethods`。</span><span class="sxs-lookup"><span data-stu-id="22c14-392">Here, non-abstract classes that derive from `C` would be required to override `FF` and `GG`, thus providing the actual implementation of `IMethods`.</span></span>
