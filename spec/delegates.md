---
ms.openlocfilehash: d162d4b6a489032dcdfca9094a39d88fd03d4013
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704097"
---
# <a name="delegates"></a><span data-ttu-id="af7cd-101">委派</span><span class="sxs-lookup"><span data-stu-id="af7cd-101">Delegates</span></span>

<span data-ttu-id="af7cd-102">委托可实现其他语言（例如C++，Pascal 和 Modula）使用函数指针进行寻址的方案。</span><span class="sxs-lookup"><span data-stu-id="af7cd-102">Delegates enable scenarios that other languages—such as C++, Pascal, and Modula -- have addressed with function pointers.</span></span> <span data-ttu-id="af7cd-103">但C++与函数指针不同，委托是完全面向对象的，不同C++于指向成员函数的指针，委托封装对象实例和方法。</span><span class="sxs-lookup"><span data-stu-id="af7cd-103">Unlike C++ function pointers, however, delegates are fully object oriented, and unlike C++ pointers to member functions, delegates encapsulate both an object instance and a method.</span></span>

<span data-ttu-id="af7cd-104">委托声明定义派生自类 `System.Delegate`的类。</span><span class="sxs-lookup"><span data-stu-id="af7cd-104">A delegate declaration defines a class that is derived from the class `System.Delegate`.</span></span> <span data-ttu-id="af7cd-105">委托实例封装一个调用列表，它是一个或多个方法的列表，其中每个方法都称为可调用实体。</span><span class="sxs-lookup"><span data-stu-id="af7cd-105">A delegate instance encapsulates an invocation list, which is a list of one or more methods, each of which is referred to as a callable entity.</span></span> <span data-ttu-id="af7cd-106">对于实例方法，可调用实体包含实例和该实例上的方法。</span><span class="sxs-lookup"><span data-stu-id="af7cd-106">For instance methods, a callable entity consists of an instance and a method on that instance.</span></span> <span data-ttu-id="af7cd-107">对于静态方法，可调用实体仅包含方法。</span><span class="sxs-lookup"><span data-stu-id="af7cd-107">For static methods, a callable entity consists of just a method.</span></span> <span data-ttu-id="af7cd-108">使用适当的参数集调用委托实例会导致使用给定的参数集调用每个委托的可调用实体。</span><span class="sxs-lookup"><span data-stu-id="af7cd-108">Invoking a delegate instance with an appropriate set of arguments causes each of the delegate's callable entities to be invoked with the given set of arguments.</span></span>

<span data-ttu-id="af7cd-109">委托实例的一个有趣且有用的属性是它不知道或关心它所封装的方法的类;重要的是，这些方法与委托的类型兼容（[委托声明](delegates.md#delegate-declarations)）。</span><span class="sxs-lookup"><span data-stu-id="af7cd-109">An interesting and useful property of a delegate instance is that it does not know or care about the classes of the methods it encapsulates; all that matters is that those methods be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with the delegate's type.</span></span> <span data-ttu-id="af7cd-110">这使委托非常适合用于 "匿名" 调用。</span><span class="sxs-lookup"><span data-stu-id="af7cd-110">This makes delegates perfectly suited for "anonymous" invocation.</span></span>

## <a name="delegate-declarations"></a><span data-ttu-id="af7cd-111">委托声明</span><span class="sxs-lookup"><span data-stu-id="af7cd-111">Delegate declarations</span></span>

<span data-ttu-id="af7cd-112">*Delegate_declaration*是声明新委托类型的*type_declaration* （[类型声明](namespaces.md#type-declarations)）。</span><span class="sxs-lookup"><span data-stu-id="af7cd-112">A *delegate_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new delegate type.</span></span>

```antlr
delegate_declaration
    : attributes? delegate_modifier* 'delegate' return_type
      identifier variant_type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;

delegate_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | delegate_modifier_unsafe
    ;
```

<span data-ttu-id="af7cd-113">同一修饰符在一个委托声明中多次出现是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="af7cd-113">It is a compile-time error for the same modifier to appear multiple times in a delegate declaration.</span></span>

<span data-ttu-id="af7cd-114">仅允许在另一种类型中声明的委托上使用 `new` 修饰符，在这种情况下，它指定此类委托隐藏同名的继承成员，如[new 修饰符](classes.md#the-new-modifier)中所述。</span><span class="sxs-lookup"><span data-stu-id="af7cd-114">The `new` modifier is only permitted on delegates declared within another type, in which case it specifies that such a delegate hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="af7cd-115">`public`、`protected`、`internal`和 `private` 修饰符控制委托类型的可访问性。</span><span class="sxs-lookup"><span data-stu-id="af7cd-115">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the delegate type.</span></span> <span data-ttu-id="af7cd-116">根据委托声明发生的上下文，可能无法使用其中某些修饰符（[声明的可访问性](basic-concepts.md#declared-accessibility)）。</span><span class="sxs-lookup"><span data-stu-id="af7cd-116">Depending on the context in which the delegate declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="af7cd-117">委托的类型名称为*标识符*。</span><span class="sxs-lookup"><span data-stu-id="af7cd-117">The delegate's type name is *identifier*.</span></span>

<span data-ttu-id="af7cd-118">可选*formal_parameter_list*指定委托的参数， *return_type*指示委托的返回类型。</span><span class="sxs-lookup"><span data-stu-id="af7cd-118">The optional *formal_parameter_list* specifies the parameters of the delegate, and *return_type* indicates the return type of the delegate.</span></span>

<span data-ttu-id="af7cd-119">可选*variant_type_parameter_list* （[变体类型参数列表](interfaces.md#variant-type-parameter-lists)）指定委托本身的类型参数。</span><span class="sxs-lookup"><span data-stu-id="af7cd-119">The optional *variant_type_parameter_list* ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)) specifies the type parameters to the delegate itself.</span></span>

<span data-ttu-id="af7cd-120">委托类型的返回类型必须是 `void`或输出安全的（[差异安全](interfaces.md#variance-safety)）。</span><span class="sxs-lookup"><span data-stu-id="af7cd-120">The return type of a delegate type must be either `void`, or output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span>

<span data-ttu-id="af7cd-121">委托类型的所有形参类型都必须为输入安全类型。</span><span class="sxs-lookup"><span data-stu-id="af7cd-121">All the formal parameter types of a delegate type must be input-safe.</span></span> <span data-ttu-id="af7cd-122">此外，任何 `out` 或 `ref` 参数类型也必须是输出安全的。</span><span class="sxs-lookup"><span data-stu-id="af7cd-122">Additionally, any `out` or `ref` parameter types must also be output-safe.</span></span> <span data-ttu-id="af7cd-123">请注意，由于基础执行平台的限制，甚至 `out` 参数都需要是输入安全的参数。</span><span class="sxs-lookup"><span data-stu-id="af7cd-123">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="af7cd-124">中的C#委托类型是等效的名称，而不是结构上等效的。</span><span class="sxs-lookup"><span data-stu-id="af7cd-124">Delegate types in C# are name equivalent, not structurally equivalent.</span></span> <span data-ttu-id="af7cd-125">具体而言，具有相同参数列表和返回类型的两个不同的委托类型被视为不同的委托类型。</span><span class="sxs-lookup"><span data-stu-id="af7cd-125">Specifically, two different delegate types that have the same parameter lists and return type are considered different delegate types.</span></span> <span data-ttu-id="af7cd-126">不过，两个不同但结构等效的委托类型的实例可以比较为等于（[委托相等运算符](expressions.md#delegate-equality-operators)）。</span><span class="sxs-lookup"><span data-stu-id="af7cd-126">However, instances of two distinct but structurally equivalent delegate types may compare as equal ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="af7cd-127">例如：</span><span class="sxs-lookup"><span data-stu-id="af7cd-127">For example:</span></span>

```csharp
delegate int D1(int i, double d);

class A
{
    public static int M1(int a, double b) {...}
}

class B
{
    delegate int D2(int c, double d);
    public static int M1(int f, double g) {...}
    public static void M2(int k, double l) {...}
    public static int M3(int g) {...}
    public static void M4(int g) {...}
}
```

<span data-ttu-id="af7cd-128">`A.M1` 和 `B.M1` 的方法与委托类型 `D1` 和 `D2` 兼容，因为它们具有相同的返回类型和参数列表;不过，这些委托类型是两种不同的类型，因此它们不能互换。</span><span class="sxs-lookup"><span data-stu-id="af7cd-128">The methods `A.M1` and `B.M1` are compatible with both the delegate types `D1` and `D2` , since they have the same return type and parameter list; however, these delegate types are two different types, so they are not interchangeable.</span></span> <span data-ttu-id="af7cd-129">`B.M2`、`B.M3`和 `B.M4` 方法与委托类型 `D1` 和 `D2`不兼容，因为它们具有不同的返回类型或参数列表。</span><span class="sxs-lookup"><span data-stu-id="af7cd-129">The methods `B.M2`, `B.M3`, and `B.M4` are incompatible with the delegate types `D1` and `D2`, since they have different return types or parameter lists.</span></span>

<span data-ttu-id="af7cd-130">与其他泛型类型声明一样，必须提供类型参数来创建构造的委托类型。</span><span class="sxs-lookup"><span data-stu-id="af7cd-130">Like other generic type declarations, type arguments must be given to create a constructed delegate type.</span></span> <span data-ttu-id="af7cd-131">构造委托类型的参数类型和返回类型是通过将委托声明中的每个类型参数替换为构造委托类型的相应类型参数来创建的。</span><span class="sxs-lookup"><span data-stu-id="af7cd-131">The parameter types and return type of a constructed delegate type are created by substituting, for each type parameter in the delegate declaration, the corresponding type argument of the constructed delegate type.</span></span> <span data-ttu-id="af7cd-132">生成的返回类型和参数类型用于确定哪些方法与构造的委托类型兼容。</span><span class="sxs-lookup"><span data-stu-id="af7cd-132">The resulting return type and parameter types are used in determining what methods are compatible with a constructed delegate type.</span></span> <span data-ttu-id="af7cd-133">例如：</span><span class="sxs-lookup"><span data-stu-id="af7cd-133">For example:</span></span>

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

<span data-ttu-id="af7cd-134">方法 `X.F` 与委托类型兼容 `Predicate<int>` 并且方法 `X.G` 与委托类型 `Predicate<string>` 兼容。</span><span class="sxs-lookup"><span data-stu-id="af7cd-134">The method `X.F` is compatible with the delegate type `Predicate<int>` and the method `X.G` is compatible with the delegate type `Predicate<string>` .</span></span>

<span data-ttu-id="af7cd-135">声明委托类型的唯一方法是通过*delegate_declaration*。</span><span class="sxs-lookup"><span data-stu-id="af7cd-135">The only way to declare a delegate type is via a *delegate_declaration*.</span></span> <span data-ttu-id="af7cd-136">委托类型是派生自 `System.Delegate`的类类型。</span><span class="sxs-lookup"><span data-stu-id="af7cd-136">A delegate type is a class type that is derived from `System.Delegate`.</span></span> <span data-ttu-id="af7cd-137">委托类型是隐式 `sealed`的，因此不允许从委托类型派生任何类型。</span><span class="sxs-lookup"><span data-stu-id="af7cd-137">Delegate types are implicitly `sealed`, so it is not permissible to derive any type from a delegate type.</span></span> <span data-ttu-id="af7cd-138">也不允许从 `System.Delegate`派生非委托类类型。</span><span class="sxs-lookup"><span data-stu-id="af7cd-138">It is also not permissible to derive a non-delegate class type from `System.Delegate`.</span></span> <span data-ttu-id="af7cd-139">请注意，`System.Delegate` 自身不是委托类型;它是派生所有委托类型的类类型。</span><span class="sxs-lookup"><span data-stu-id="af7cd-139">Note that `System.Delegate` is not itself a delegate type; it is a class type from which all delegate types are derived.</span></span>

<span data-ttu-id="af7cd-140">C#提供委托实例化和调用的特殊语法。</span><span class="sxs-lookup"><span data-stu-id="af7cd-140">C# provides special syntax for delegate instantiation and invocation.</span></span> <span data-ttu-id="af7cd-141">除实例化外，可以应用于类或类实例的任何操作也可以分别应用于委托类或实例。</span><span class="sxs-lookup"><span data-stu-id="af7cd-141">Except for instantiation, any operation that can be applied to a class or class instance can also be applied to a delegate class or instance, respectively.</span></span> <span data-ttu-id="af7cd-142">具体而言，可以通过常规成员访问语法来访问 `System.Delegate` 类型的成员。</span><span class="sxs-lookup"><span data-stu-id="af7cd-142">In particular, it is possible to access members of the `System.Delegate` type via the usual member access syntax.</span></span>

<span data-ttu-id="af7cd-143">委托实例封装的方法集称为调用列表。</span><span class="sxs-lookup"><span data-stu-id="af7cd-143">The set of methods encapsulated by a delegate instance is called an invocation list.</span></span> <span data-ttu-id="af7cd-144">当从单个方法创建委托实例（[委托兼容性](delegates.md#delegate-compatibility)）时，它会封装该方法，并且它的调用列表只包含一个条目。</span><span class="sxs-lookup"><span data-stu-id="af7cd-144">When a delegate instance is created ([Delegate compatibility](delegates.md#delegate-compatibility)) from a single method, it encapsulates that method, and its invocation list contains only one entry.</span></span> <span data-ttu-id="af7cd-145">但是，如果两个非 null 委托实例组合在一起，则会将其调用列表串联在顺序左操作数和右操作数之间，以形成包含两个或多个条目的新调用列表。</span><span class="sxs-lookup"><span data-stu-id="af7cd-145">However, when two non-null delegate instances are combined, their invocation lists are concatenated -- in the order left operand then right operand -- to form a new invocation list, which contains two or more entries.</span></span>

<span data-ttu-id="af7cd-146">使用二进制 `+` （[加法运算符](expressions.md#addition-operator)）和 `+=` 运算符（[复合赋值](expressions.md#compound-assignment)）组合委托。</span><span class="sxs-lookup"><span data-stu-id="af7cd-146">Delegates are combined using the binary `+` ([Addition operator](expressions.md#addition-operator)) and `+=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="af7cd-147">可以使用二进制 `-` （[减法运算符](expressions.md#subtraction-operator)）和 `-=` 运算符（[复合赋值](expressions.md#compound-assignment)）从委托组合中删除委托。</span><span class="sxs-lookup"><span data-stu-id="af7cd-147">A delegate can be removed from a combination of delegates, using the binary `-` ([Subtraction operator](expressions.md#subtraction-operator)) and `-=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="af7cd-148">可以比较委托是否相等（[委托相等运算符](expressions.md#delegate-equality-operators)）。</span><span class="sxs-lookup"><span data-stu-id="af7cd-148">Delegates can be compared for equality ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="af7cd-149">下面的示例演示了多个委托的实例化及其相应的调用列表：</span><span class="sxs-lookup"><span data-stu-id="af7cd-149">The following example shows the instantiation of a number of delegates, and their corresponding invocation lists:</span></span>

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public static void M2(int i) {...}

}

class Test
{
    static void Main() {
        D cd1 = new D(C.M1);      // M1
        D cd2 = new D(C.M2);      // M2
        D cd3 = cd1 + cd2;        // M1 + M2
        D cd4 = cd3 + cd1;        // M1 + M2 + M1
        D cd5 = cd4 + cd3;        // M1 + M2 + M1 + M1 + M2
    }

}
```

<span data-ttu-id="af7cd-150">实例化 `cd1` 和 `cd2` 时，它们将封装一个方法。</span><span class="sxs-lookup"><span data-stu-id="af7cd-150">When `cd1` and `cd2` are instantiated, they each encapsulate one method.</span></span> <span data-ttu-id="af7cd-151">当 `cd3` 实例化时，它具有两个方法的调用列表，按该顺序 `M1` 和 `M2`。</span><span class="sxs-lookup"><span data-stu-id="af7cd-151">When `cd3` is instantiated, it has an invocation list of two methods, `M1` and `M2`, in that order.</span></span> <span data-ttu-id="af7cd-152">`cd4`的调用列表按该顺序包含 `M1`、`M2`和 `M1`。</span><span class="sxs-lookup"><span data-stu-id="af7cd-152">`cd4`'s invocation list contains `M1`, `M2`, and `M1`, in that order.</span></span> <span data-ttu-id="af7cd-153">最后，`cd5`的调用列表按该顺序包含 `M1`、`M2`、`M1`、`M1`和 `M2`。</span><span class="sxs-lookup"><span data-stu-id="af7cd-153">Finally, `cd5`'s invocation list contains `M1`, `M2`, `M1`, `M1`, and `M2`, in that order.</span></span> <span data-ttu-id="af7cd-154">有关组合（以及删除）委托的更多示例，请参阅[委托调用](delegates.md#delegate-invocation)。</span><span class="sxs-lookup"><span data-stu-id="af7cd-154">For more examples of combining (as well as removing) delegates, see [Delegate invocation](delegates.md#delegate-invocation).</span></span>

## <a name="delegate-compatibility"></a><span data-ttu-id="af7cd-155">委托兼容性</span><span class="sxs-lookup"><span data-stu-id="af7cd-155">Delegate compatibility</span></span>

<span data-ttu-id="af7cd-156">如果满足以下所有条件，则方法或委托 `M` 与委托类型 `D`***兼容***：</span><span class="sxs-lookup"><span data-stu-id="af7cd-156">A method or delegate `M` is ***compatible*** with a delegate type `D` if all of the following are true:</span></span>

*  <span data-ttu-id="af7cd-157">`D` 和 `M` 具有相同数量的参数，并且 `D` 中的每个参数都具有与 `out` 中的相应参数相同的 `ref` 或 `M`修饰符。</span><span class="sxs-lookup"><span data-stu-id="af7cd-157">`D` and `M` have the same number of parameters, and each parameter in `D` has the same `ref` or `out` modifiers as the corresponding parameter in `M`.</span></span>
*  <span data-ttu-id="af7cd-158">对于每个值参数（不带 `ref` 或 `out` 修饰符的参数），都存在从 `D` 中的参数类型到 `M`中的相应参数类型的标识转换（[标识转换](conversions.md#identity-conversion)）或隐式引用转换（[隐式引用](conversions.md#implicit-reference-conversions)转换）。</span><span class="sxs-lookup"><span data-stu-id="af7cd-158">For each value parameter (a parameter with no `ref` or `out` modifier), an identity conversion ([Identity conversion](conversions.md#identity-conversion)) or implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the parameter type in `D` to the corresponding parameter type in `M`.</span></span>
*  <span data-ttu-id="af7cd-159">对于每个 `ref` 或 `out` 参数，`D` 中的参数类型与 `M`中的参数类型相同。</span><span class="sxs-lookup"><span data-stu-id="af7cd-159">For each `ref` or `out` parameter, the parameter type in `D` is the same as the parameter type in `M`.</span></span>
*  <span data-ttu-id="af7cd-160">存在从 `M` 的返回类型到 `D`的返回类型的标识或隐式引用转换。</span><span class="sxs-lookup"><span data-stu-id="af7cd-160">An identity or implicit reference conversion exists from the return type of `M` to the return type of `D`.</span></span>

## <a name="delegate-instantiation"></a><span data-ttu-id="af7cd-161">委托实例化</span><span class="sxs-lookup"><span data-stu-id="af7cd-161">Delegate instantiation</span></span>

<span data-ttu-id="af7cd-162">委托的实例是由*delegate_creation_expression* （[委托创建表达式](expressions.md#delegate-creation-expressions)）或到委托类型的转换创建的。</span><span class="sxs-lookup"><span data-stu-id="af7cd-162">An instance of a delegate is created by a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) or a conversion to a delegate type.</span></span> <span data-ttu-id="af7cd-163">然后，新创建的委托实例指的是：</span><span class="sxs-lookup"><span data-stu-id="af7cd-163">The newly created delegate instance then refers to either:</span></span>

*  <span data-ttu-id="af7cd-164">*Delegate_creation_expression*中引用的静态方法，或</span><span class="sxs-lookup"><span data-stu-id="af7cd-164">The static method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="af7cd-165">*Delegate_creation_expression*或中引用的目标对象（不能是 `null`）和实例方法</span><span class="sxs-lookup"><span data-stu-id="af7cd-165">The target object (which cannot be `null`) and instance method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="af7cd-166">其他委托。</span><span class="sxs-lookup"><span data-stu-id="af7cd-166">Another delegate.</span></span>

<span data-ttu-id="af7cd-167">例如：</span><span class="sxs-lookup"><span data-stu-id="af7cd-167">For example:</span></span>

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public void M2(int i) {...}
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);        // static method
        C t = new C();
        D cd2 = new D(t.M2);        // instance method
        D cd3 = new D(cd2);        // another delegate
    }
}
```

<span data-ttu-id="af7cd-168">实例化后，委托实例始终引用相同的目标对象和方法。</span><span class="sxs-lookup"><span data-stu-id="af7cd-168">Once instantiated, delegate instances always refer to the same target object and method.</span></span> <span data-ttu-id="af7cd-169">请记住，当合并两个委托或从另一个委托移除时，将使用其自己的调用列表生成新的委托结果;合并或移除的委托的调用列表保持不变。</span><span class="sxs-lookup"><span data-stu-id="af7cd-169">Remember, when two delegates are combined, or one is removed from another, a new delegate results with its own invocation list; the invocation lists of the delegates combined or removed remain unchanged.</span></span>

## <a name="delegate-invocation"></a><span data-ttu-id="af7cd-170">委托调用</span><span class="sxs-lookup"><span data-stu-id="af7cd-170">Delegate invocation</span></span>

<span data-ttu-id="af7cd-171">C#提供调用委托的特殊语法。</span><span class="sxs-lookup"><span data-stu-id="af7cd-171">C# provides special syntax for invoking a delegate.</span></span> <span data-ttu-id="af7cd-172">当调用列表包含一个项的非 null 委托实例调用时，它将调用一个方法，该方法具有给定的参数，并返回与被引用方法相同的值。</span><span class="sxs-lookup"><span data-stu-id="af7cd-172">When a non-null delegate instance whose invocation list contains one entry is invoked, it invokes the one method with the same arguments it was given, and returns the same value as the referred to method.</span></span> <span data-ttu-id="af7cd-173">（有关委托调用的详细信息，请参阅[委托调用](expressions.md#delegate-invocations)。）如果在调用此类委托的过程中发生异常，且未在调用的方法中捕获该异常，则会在调用委托的方法中继续搜索异常 catch 子句，就好像该方法直接调用了委托引用的方法一样。</span><span class="sxs-lookup"><span data-stu-id="af7cd-173">(See [Delegate invocations](expressions.md#delegate-invocations) for detailed information on delegate invocation.) If an exception occurs during the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, as if that method had directly called the method to which that delegate referred.</span></span>

<span data-ttu-id="af7cd-174">调用列表中包含多个项的委托实例的调用将按顺序同步调用调用列表中的每个方法。</span><span class="sxs-lookup"><span data-stu-id="af7cd-174">Invocation of a delegate instance whose invocation list contains multiple entries proceeds by invoking each of the methods in the invocation list, synchronously, in order.</span></span> <span data-ttu-id="af7cd-175">调用的每个方法都被传递给为委托实例指定的相同的一组参数。</span><span class="sxs-lookup"><span data-stu-id="af7cd-175">Each method so called is passed the same set of arguments as was given to the delegate instance.</span></span> <span data-ttu-id="af7cd-176">如果此类委托调用包括引用参数（[引用参数](classes.md#reference-parameters)），则将发生每个方法调用，同时引用同一变量;调用列表中的方法对该变量所做的更改将对调用列表中的方法可见。</span><span class="sxs-lookup"><span data-stu-id="af7cd-176">If such a delegate invocation includes reference parameters ([Reference parameters](classes.md#reference-parameters)), each method invocation will occur with a reference to the same variable; changes to that variable by one method in the invocation list will be visible to methods further down the invocation list.</span></span> <span data-ttu-id="af7cd-177">如果委托调用包含输出参数或返回值，则其最终值将来自列表中最后一个委托的调用。</span><span class="sxs-lookup"><span data-stu-id="af7cd-177">If the delegate invocation includes output parameters or a return value, their final value will come from the invocation of the last delegate in the list.</span></span>

<span data-ttu-id="af7cd-178">如果在处理此类委托的调用过程中发生异常，且未在调用的方法中捕获该异常，则会在调用委托的方法中继续执行异常 catch 子句的搜索，并使任何方法不会调用调用列表。</span><span class="sxs-lookup"><span data-stu-id="af7cd-178">If an exception occurs during processing of the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, and any methods further down the invocation list are not invoked.</span></span>

<span data-ttu-id="af7cd-179">尝试调用值为 null 的委托实例会导致类型 `System.NullReferenceException`的异常。</span><span class="sxs-lookup"><span data-stu-id="af7cd-179">Attempting to invoke a delegate instance whose value is null results in an exception of type `System.NullReferenceException`.</span></span>

<span data-ttu-id="af7cd-180">下面的示例演示如何实例化、组合、移除和调用委托：</span><span class="sxs-lookup"><span data-stu-id="af7cd-180">The following example shows how to instantiate, combine, remove, and invoke delegates:</span></span>

```csharp
using System;

delegate void D(int x);

class C
{
    public static void M1(int i) {
        Console.WriteLine("C.M1: " + i);
    }

    public static void M2(int i) {
        Console.WriteLine("C.M2: " + i);
    }

    public void M3(int i) {
        Console.WriteLine("C.M3: " + i);
    }
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);
        cd1(-1);                // call M1

        D cd2 = new D(C.M2);
        cd2(-2);                // call M2

        D cd3 = cd1 + cd2;
        cd3(10);                // call M1 then M2

        cd3 += cd1;
        cd3(20);                // call M1, M2, then M1

        C c = new C();
        D cd4 = new D(c.M3);
        cd3 += cd4;
        cd3(30);                // call M1, M2, M1, then M3

        cd3 -= cd1;             // remove last M1
        cd3(40);                // call M1, M2, then M3

        cd3 -= cd4;
        cd3(50);                // call M1 then M2

        cd3 -= cd2;
        cd3(60);                // call M1

        cd3 -= cd2;             // impossible removal is benign
        cd3(60);                // call M1

        cd3 -= cd1;             // invocation list is empty so cd3 is null

        cd3(70);                // System.NullReferenceException thrown

        cd3 -= cd1;             // impossible removal is benign
    }
}
```

<span data-ttu-id="af7cd-181">如语句 `cd3 += cd1;`中所示，一个委托可以在调用列表中多次出现。</span><span class="sxs-lookup"><span data-stu-id="af7cd-181">As shown in the statement `cd3 += cd1;`, a delegate can be present in an invocation list multiple times.</span></span> <span data-ttu-id="af7cd-182">在这种情况下，它只会在每次出现时调用一次。</span><span class="sxs-lookup"><span data-stu-id="af7cd-182">In this case, it is simply invoked once per occurrence.</span></span> <span data-ttu-id="af7cd-183">在此类调用列表中，移除该委托时，调用列表中的最后一个匹配项将被实际删除。</span><span class="sxs-lookup"><span data-stu-id="af7cd-183">In an invocation list such as this, when that delegate is removed, the last occurrence in the invocation list is the one actually removed.</span></span>

<span data-ttu-id="af7cd-184">在执行最后一个语句之前，`cd3 -= cd1;`，委托 `cd3` 引用一个空的调用列表。</span><span class="sxs-lookup"><span data-stu-id="af7cd-184">Immediately prior to the execution of the final statement, `cd3 -= cd1;`, the delegate `cd3` refers to an empty invocation list.</span></span> <span data-ttu-id="af7cd-185">尝试从空列表中删除委托（或从非空列表中删除不存在的委托）不是错误。</span><span class="sxs-lookup"><span data-stu-id="af7cd-185">Attempting to remove a delegate from an empty list (or to remove a non-existent delegate from a non-empty list) is not an error.</span></span>

<span data-ttu-id="af7cd-186">生成的输出为：</span><span class="sxs-lookup"><span data-stu-id="af7cd-186">The output produced is:</span></span>

```console
C.M1: -1
C.M2: -2
C.M1: 10
C.M2: 10
C.M1: 20
C.M2: 20
C.M1: 20
C.M1: 30
C.M2: 30
C.M1: 30
C.M3: 30
C.M1: 40
C.M2: 40
C.M3: 40
C.M1: 50
C.M2: 50
C.M1: 60
C.M1: 60
```
