---
ms.openlocfilehash: 08c14d9ef2afe30580f456995066c141653ede92
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229521"
---
# <a name="delegates"></a><span data-ttu-id="ebb00-101">委托</span><span class="sxs-lookup"><span data-stu-id="ebb00-101">Delegates</span></span>

<span data-ttu-id="ebb00-102">委托实现方案的其他语言，如 c + +、 Pascal 和 Modula-已，解决与函数指针。</span><span class="sxs-lookup"><span data-stu-id="ebb00-102">Delegates enable scenarios that other languages—such as C++, Pascal, and Modula -- have addressed with function pointers.</span></span> <span data-ttu-id="ebb00-103">与 c + + 函数指针不同，委托完全是面向对象的而且与 c + + 成员函数的指针，不同的委托封装的对象实例和方法。</span><span class="sxs-lookup"><span data-stu-id="ebb00-103">Unlike C++ function pointers, however, delegates are fully object oriented, and unlike C++ pointers to member functions, delegates encapsulate both an object instance and a method.</span></span>

<span data-ttu-id="ebb00-104">在委托声明定义了从类派生一个类`System.Delegate`。</span><span class="sxs-lookup"><span data-stu-id="ebb00-104">A delegate declaration defines a class that is derived from the class `System.Delegate`.</span></span> <span data-ttu-id="ebb00-105">委托实例封装一个调用列表，这是一个或多个方法，其中每个被称为可调用实体的列表。</span><span class="sxs-lookup"><span data-stu-id="ebb00-105">A delegate instance encapsulates an invocation list, which is a list of one or more methods, each of which is referred to as a callable entity.</span></span> <span data-ttu-id="ebb00-106">对于实例方法，可调用实体实例和包含该实例上的方法。</span><span class="sxs-lookup"><span data-stu-id="ebb00-106">For instance methods, a callable entity consists of an instance and a method on that instance.</span></span> <span data-ttu-id="ebb00-107">对于静态方法，可调用实体包含的只是一种方法。</span><span class="sxs-lookup"><span data-stu-id="ebb00-107">For static methods, a callable entity consists of just a method.</span></span> <span data-ttu-id="ebb00-108">调用具有一组适当的参数的委托实例会导致每个委托的可调用实体使用给定的参数集的调用。</span><span class="sxs-lookup"><span data-stu-id="ebb00-108">Invoking a delegate instance with an appropriate set of arguments causes each of the delegate's callable entities to be invoked with the given set of arguments.</span></span>

<span data-ttu-id="ebb00-109">委托实例的一个有趣且有用的属性是它不会知道或关心封装; 方法的类最重要的就是这些方法为兼容 ([委托声明](delegates.md#delegate-declarations)) 与委托的类型。</span><span class="sxs-lookup"><span data-stu-id="ebb00-109">An interesting and useful property of a delegate instance is that it does not know or care about the classes of the methods it encapsulates; all that matters is that those methods be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with the delegate's type.</span></span> <span data-ttu-id="ebb00-110">这样，非常适合于"匿名"调用的委托。</span><span class="sxs-lookup"><span data-stu-id="ebb00-110">This makes delegates perfectly suited for "anonymous" invocation.</span></span>

## <a name="delegate-declarations"></a><span data-ttu-id="ebb00-111">委托声明</span><span class="sxs-lookup"><span data-stu-id="ebb00-111">Delegate declarations</span></span>

<span data-ttu-id="ebb00-112">一个*delegate_declaration*是*type_declaration* ([类型声明](namespaces.md#type-declarations))，其用于声明新的委托类型。</span><span class="sxs-lookup"><span data-stu-id="ebb00-112">A *delegate_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new delegate type.</span></span>

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

<span data-ttu-id="ebb00-113">它是在委托声明中多次出现同一修饰符的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="ebb00-113">It is a compile-time error for the same modifier to appear multiple times in a delegate declaration.</span></span>

<span data-ttu-id="ebb00-114">`new`修饰符仅允许在委托声明中另一种类型，它指定此类委托在这种情况下隐藏继承的成员通过相同的名称，如中所述[的新修饰符](classes.md#the-new-modifier)。</span><span class="sxs-lookup"><span data-stu-id="ebb00-114">The `new` modifier is only permitted on delegates declared within another type, in which case it specifies that such a delegate hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="ebb00-115">`public`， `protected`， `internal`，和`private`修饰符控制委托类型的可访问性。</span><span class="sxs-lookup"><span data-stu-id="ebb00-115">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the delegate type.</span></span> <span data-ttu-id="ebb00-116">具体取决于与委托声明发生的上下文，其中某些修饰符可能不允许使用 ([声明可访问性](basic-concepts.md#declared-accessibility))。</span><span class="sxs-lookup"><span data-stu-id="ebb00-116">Depending on the context in which the delegate declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="ebb00-117">委托的类型名称是*标识符*。</span><span class="sxs-lookup"><span data-stu-id="ebb00-117">The delegate's type name is *identifier*.</span></span>

<span data-ttu-id="ebb00-118">可选*formal_parameter_list*指定的参数的委托，并*return_type*指示委托的返回类型。</span><span class="sxs-lookup"><span data-stu-id="ebb00-118">The optional *formal_parameter_list* specifies the parameters of the delegate, and *return_type* indicates the return type of the delegate.</span></span>

<span data-ttu-id="ebb00-119">可选*variant_type_parameter_list* ([变体类型的参数列表](interfaces.md#variant-type-parameter-lists)) 指定自身的委托的类型参数。</span><span class="sxs-lookup"><span data-stu-id="ebb00-119">The optional *variant_type_parameter_list* ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)) specifies the type parameters to the delegate itself.</span></span>

<span data-ttu-id="ebb00-120">委托类型的返回类型必须是`void`，或输出安全 ([变体安全](interfaces.md#variance-safety))。</span><span class="sxs-lookup"><span data-stu-id="ebb00-120">The return type of a delegate type must be either `void`, or output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span>

<span data-ttu-id="ebb00-121">所有委托类型的正式参数类型必须都是输入安全。</span><span class="sxs-lookup"><span data-stu-id="ebb00-121">All the formal parameter types of a delegate type must be input-safe.</span></span> <span data-ttu-id="ebb00-122">此外，如果`out`或`ref`参数类型还必须是安全的输出。</span><span class="sxs-lookup"><span data-stu-id="ebb00-122">Additionally, any `out` or `ref` parameter types must also be output-safe.</span></span> <span data-ttu-id="ebb00-123">请注意，甚至`out`参数需是输入安全的由于基础执行平台的限制。</span><span class="sxs-lookup"><span data-stu-id="ebb00-123">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="ebb00-124">等效的而不是结构等效的 C# 中的委托类型是名称。</span><span class="sxs-lookup"><span data-stu-id="ebb00-124">Delegate types in C# are name equivalent, not structurally equivalent.</span></span> <span data-ttu-id="ebb00-125">具体而言，具有相同的参数的两种不同的委托类型列出并返回类型被视为不同的委托类型。</span><span class="sxs-lookup"><span data-stu-id="ebb00-125">Specifically, two different delegate types that have the same parameter lists and return type are considered different delegate types.</span></span> <span data-ttu-id="ebb00-126">但是，两个不同但结构等效的委托类型的实例可能会比较结果相等 ([委托相等运算符](expressions.md#delegate-equality-operators))。</span><span class="sxs-lookup"><span data-stu-id="ebb00-126">However, instances of two distinct but structurally equivalent delegate types may compare as equal ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="ebb00-127">例如：</span><span class="sxs-lookup"><span data-stu-id="ebb00-127">For example:</span></span>

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

<span data-ttu-id="ebb00-128">方法`A.M1`并`B.M1 `与委托类型兼容`D1`和`D2`，因为它们具有相同的返回类型和参数列表; 但是，这些委托类型是两种不同类型，因此它们不是可互换。</span><span class="sxs-lookup"><span data-stu-id="ebb00-128">The methods `A.M1` and `B.M1 `are compatible with both the delegate types `D1` and `D2` , since they have the same return type and parameter list; however, these delegate types are two different types, so they are not interchangeable.</span></span> <span data-ttu-id="ebb00-129">方法`B.M2`， `B.M3`，并`B.M4`与委托类型不兼容`D1`和`D2`，因为它们具有不同的返回类型或参数列表。</span><span class="sxs-lookup"><span data-stu-id="ebb00-129">The methods `B.M2`, `B.M3`, and `B.M4` are incompatible with the delegate types `D1` and `D2`, since they have different return types or parameter lists.</span></span>

<span data-ttu-id="ebb00-130">如其他泛型类型声明，必须提供类型参数以创建构造的委托类型。</span><span class="sxs-lookup"><span data-stu-id="ebb00-130">Like other generic type declarations, type arguments must be given to create a constructed delegate type.</span></span> <span data-ttu-id="ebb00-131">通过将替换为在委托声明中，每个类型参数的构造的委托类型的相应类型参数创建的参数类型和构造的委托类型的返回类型。</span><span class="sxs-lookup"><span data-stu-id="ebb00-131">The parameter types and return type of a constructed delegate type are created by substituting, for each type parameter in the delegate declaration, the corresponding type argument of the constructed delegate type.</span></span> <span data-ttu-id="ebb00-132">生成的返回类型和参数类型用于确定使用哪些方法将与构造的委托类型兼容。</span><span class="sxs-lookup"><span data-stu-id="ebb00-132">The resulting return type and parameter types are used in determining what methods are compatible with a constructed delegate type.</span></span> <span data-ttu-id="ebb00-133">例如：</span><span class="sxs-lookup"><span data-stu-id="ebb00-133">For example:</span></span>

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

<span data-ttu-id="ebb00-134">该方法`X.F`委托类型与兼容`Predicate<int>`和方法`X.G`与委托类型兼容`Predicate<string>`。</span><span class="sxs-lookup"><span data-stu-id="ebb00-134">The method `X.F` is compatible with the delegate type `Predicate<int>` and the method `X.G` is compatible with the delegate type `Predicate<string>` .</span></span>

<span data-ttu-id="ebb00-135">声明委托类型的唯一方法是通过*delegate_declaration*。</span><span class="sxs-lookup"><span data-stu-id="ebb00-135">The only way to declare a delegate type is via a *delegate_declaration*.</span></span> <span data-ttu-id="ebb00-136">委托类型是派生自的类类型`System.Delegate`。</span><span class="sxs-lookup"><span data-stu-id="ebb00-136">A delegate type is a class type that is derived from `System.Delegate`.</span></span> <span data-ttu-id="ebb00-137">委托类型是隐式`sealed`，因此不允许从委托类型派生的任何类型。</span><span class="sxs-lookup"><span data-stu-id="ebb00-137">Delegate types are implicitly `sealed`, so it is not permissible to derive any type from a delegate type.</span></span> <span data-ttu-id="ebb00-138">此外，也不允许派生类非委托类型从`System.Delegate`。</span><span class="sxs-lookup"><span data-stu-id="ebb00-138">It is also not permissible to derive a non-delegate class type from `System.Delegate`.</span></span> <span data-ttu-id="ebb00-139">请注意，`System.Delegate`是委托类型本身不; 它是所有委托类型都派生的类类型。</span><span class="sxs-lookup"><span data-stu-id="ebb00-139">Note that `System.Delegate` is not itself a delegate type; it is a class type from which all delegate types are derived.</span></span>

<span data-ttu-id="ebb00-140">C# 提供特殊语法委托实例化和调用。</span><span class="sxs-lookup"><span data-stu-id="ebb00-140">C# provides special syntax for delegate instantiation and invocation.</span></span> <span data-ttu-id="ebb00-141">实例化，除了可以应用于类或类实例的任何操作可以也应用于委托类或实例，分别。</span><span class="sxs-lookup"><span data-stu-id="ebb00-141">Except for instantiation, any operation that can be applied to a class or class instance can also be applied to a delegate class or instance, respectively.</span></span> <span data-ttu-id="ebb00-142">具体而言，就可以访问的成员`System.Delegate`通过常用的成员访问语法类型。</span><span class="sxs-lookup"><span data-stu-id="ebb00-142">In particular, it is possible to access members of the `System.Delegate` type via the usual member access syntax.</span></span>

<span data-ttu-id="ebb00-143">委托实例封装的方法集称为一个调用列表。</span><span class="sxs-lookup"><span data-stu-id="ebb00-143">The set of methods encapsulated by a delegate instance is called an invocation list.</span></span> <span data-ttu-id="ebb00-144">创建委托实例时 ([委托兼容性](delegates.md#delegate-compatibility)) 的单个方法，它封装此方法，并且其调用列表中包含只有一个条目。</span><span class="sxs-lookup"><span data-stu-id="ebb00-144">When a delegate instance is created ([Delegate compatibility](delegates.md#delegate-compatibility)) from a single method, it encapsulates that method, and its invocation list contains only one entry.</span></span> <span data-ttu-id="ebb00-145">但是，当组合两个非 null 委托实例时，它们的调用列表串联起来，-顺序从左操作数，则右操作数-以形成新的调用列表，其中包含两个或多个条目。</span><span class="sxs-lookup"><span data-stu-id="ebb00-145">However, when two non-null delegate instances are combined, their invocation lists are concatenated -- in the order left operand then right operand -- to form a new invocation list, which contains two or more entries.</span></span>

<span data-ttu-id="ebb00-146">使用二进制组合委托`+`([加法运算符](expressions.md#addition-operator)) 和`+=`运算符 ([复合赋值](expressions.md#compound-assignment))。</span><span class="sxs-lookup"><span data-stu-id="ebb00-146">Delegates are combined using the binary `+` ([Addition operator](expressions.md#addition-operator)) and `+=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="ebb00-147">委托可以从委托，使用二进制文件的组合`-`([减法运算符](expressions.md#subtraction-operator)) 和`-=`运算符 ([复合赋值](expressions.md#compound-assignment))。</span><span class="sxs-lookup"><span data-stu-id="ebb00-147">A delegate can be removed from a combination of delegates, using the binary `-` ([Subtraction operator](expressions.md#subtraction-operator)) and `-=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="ebb00-148">委托可以比较它们是否相等 ([委托相等运算符](expressions.md#delegate-equality-operators))。</span><span class="sxs-lookup"><span data-stu-id="ebb00-148">Delegates can be compared for equality ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="ebb00-149">下面的示例演示实例化多个委托，并列出其对应的调用：</span><span class="sxs-lookup"><span data-stu-id="ebb00-149">The following example shows the instantiation of a number of delegates, and their corresponding invocation lists:</span></span>

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

<span data-ttu-id="ebb00-150">当`cd1`和`cd2`是实例化，它们分别封装一个方法。</span><span class="sxs-lookup"><span data-stu-id="ebb00-150">When `cd1` and `cd2` are instantiated, they each encapsulate one method.</span></span> <span data-ttu-id="ebb00-151">当`cd3`是实例化，它具有两种方法的调用列表`M1`和`M2`按顺序。</span><span class="sxs-lookup"><span data-stu-id="ebb00-151">When `cd3` is instantiated, it has an invocation list of two methods, `M1` and `M2`, in that order.</span></span> <span data-ttu-id="ebb00-152">`cd4`调用列表包含`M1`， `M2`，和`M1`按顺序。</span><span class="sxs-lookup"><span data-stu-id="ebb00-152">`cd4`'s invocation list contains `M1`, `M2`, and `M1`, in that order.</span></span> <span data-ttu-id="ebb00-153">最后，`cd5`的调用列表包含`M1`， `M2`， `M1`， `M1`，并`M2`按顺序。</span><span class="sxs-lookup"><span data-stu-id="ebb00-153">Finally, `cd5`'s invocation list contains `M1`, `M2`, `M1`, `M1`, and `M2`, in that order.</span></span> <span data-ttu-id="ebb00-154">组合 （也为删除） 委托的更多示例，请参阅[委托调用](delegates.md#delegate-invocation)。</span><span class="sxs-lookup"><span data-stu-id="ebb00-154">For more examples of combining (as well as removing) delegates, see [Delegate invocation](delegates.md#delegate-invocation).</span></span>

## <a name="delegate-compatibility"></a><span data-ttu-id="ebb00-155">委托兼容性</span><span class="sxs-lookup"><span data-stu-id="ebb00-155">Delegate compatibility</span></span>

<span data-ttu-id="ebb00-156">方法或委托`M`是***兼容***与委托类型`D`如果满足所有以下：</span><span class="sxs-lookup"><span data-stu-id="ebb00-156">A method or delegate `M` is ***compatible*** with a delegate type `D` if all of the following are true:</span></span>

*  <span data-ttu-id="ebb00-157">`D` 并`M`具有相同数目的参数，以及在每个参数`D`具有相同`ref`或`out`与中的相应参数的修饰符`M`。</span><span class="sxs-lookup"><span data-stu-id="ebb00-157">`D` and `M` have the same number of parameters, and each parameter in `D` has the same `ref` or `out` modifiers as the corresponding parameter in `M`.</span></span>
*  <span data-ttu-id="ebb00-158">对于每个值参数 (不带参数`ref`或`out`修饰符)，标识转换 ([标识转换](conversions.md#identity-conversion)) 或隐式引用转换 ([隐式引用转换](conversions.md#implicit-reference-conversions))存在从中的参数类型`D`中的相应参数类型为`M`。</span><span class="sxs-lookup"><span data-stu-id="ebb00-158">For each value parameter (a parameter with no `ref` or `out` modifier), an identity conversion ([Identity conversion](conversions.md#identity-conversion)) or implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the parameter type in `D` to the corresponding parameter type in `M`.</span></span>
*  <span data-ttu-id="ebb00-159">每个`ref`或`out`中的参数，参数类型`D`中的参数类型与相同`M`。</span><span class="sxs-lookup"><span data-stu-id="ebb00-159">For each `ref` or `out` parameter, the parameter type in `D` is the same as the parameter type in `M`.</span></span>
*  <span data-ttu-id="ebb00-160">存在从的返回类型的标识或隐式引用转换`M`的返回类型为`D`。</span><span class="sxs-lookup"><span data-stu-id="ebb00-160">An identity or implicit reference conversion exists from the return type of `M` to the return type of `D`.</span></span>

## <a name="delegate-instantiation"></a><span data-ttu-id="ebb00-161">委托实例化</span><span class="sxs-lookup"><span data-stu-id="ebb00-161">Delegate instantiation</span></span>

<span data-ttu-id="ebb00-162">通过创建委托的实例*delegate_creation_expression* ([委托创建表达式](expressions.md#delegate-creation-expressions)) 或转换为委托类型。</span><span class="sxs-lookup"><span data-stu-id="ebb00-162">An instance of a delegate is created by a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) or a conversion to a delegate type.</span></span> <span data-ttu-id="ebb00-163">为然后引用新创建的委托实例：</span><span class="sxs-lookup"><span data-stu-id="ebb00-163">The newly created delegate instance then refers to either:</span></span>

*  <span data-ttu-id="ebb00-164">中引用的静态方法*delegate_creation_expression*，或</span><span class="sxs-lookup"><span data-stu-id="ebb00-164">The static method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="ebb00-165">目标对象 (这不能`null`) 和实例方法中引用*delegate_creation_expression*，或</span><span class="sxs-lookup"><span data-stu-id="ebb00-165">The target object (which cannot be `null`) and instance method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="ebb00-166">另一个委托。</span><span class="sxs-lookup"><span data-stu-id="ebb00-166">Another delegate.</span></span>

<span data-ttu-id="ebb00-167">例如：</span><span class="sxs-lookup"><span data-stu-id="ebb00-167">For example:</span></span>

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

<span data-ttu-id="ebb00-168">实例化后，委托实例始终引用相同的目标对象和方法。</span><span class="sxs-lookup"><span data-stu-id="ebb00-168">Once instantiated, delegate instances always refer to the same target object and method.</span></span> <span data-ttu-id="ebb00-169">请记住，当结合使用两个委托，或一个已从另一种，使用它自己的调用列表; 的新委托结果合并或移除的委托的调用列表将保持不变。</span><span class="sxs-lookup"><span data-stu-id="ebb00-169">Remember, when two delegates are combined, or one is removed from another, a new delegate results with its own invocation list; the invocation lists of the delegates combined or removed remain unchanged.</span></span>

## <a name="delegate-invocation"></a><span data-ttu-id="ebb00-170">委托调用</span><span class="sxs-lookup"><span data-stu-id="ebb00-170">Delegate invocation</span></span>

<span data-ttu-id="ebb00-171">C# 提供特殊的语法调用委托。</span><span class="sxs-lookup"><span data-stu-id="ebb00-171">C# provides special syntax for invoking a delegate.</span></span> <span data-ttu-id="ebb00-172">当调用其调用列表包含一项的非 null 委托实例时，它调用具有它提供，并返回相同的值均与相同的参数的方法到方法。</span><span class="sxs-lookup"><span data-stu-id="ebb00-172">When a non-null delegate instance whose invocation list contains one entry is invoked, it invokes the one method with the same arguments it was given, and returns the same value as the referred to method.</span></span> <span data-ttu-id="ebb00-173">(请参阅[委托调用](expressions.md#delegate-invocations)的委托调用的详细信息。)如果此类委托的调用期间发生异常，并且未调用的方法中捕获该异常，就像已直接调用该方法，该异常的 catch 子句的搜索将继续在调用该委托的方法方法的委托到的引用。</span><span class="sxs-lookup"><span data-stu-id="ebb00-173">(See [Delegate invocations](expressions.md#delegate-invocations) for detailed information on delegate invocation.) If an exception occurs during the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, as if that method had directly called the method to which that delegate referred.</span></span>

<span data-ttu-id="ebb00-174">通过调用每个调用列表中的方法以同步方式，按顺序将继续对其调用列表包含多个条目的委托实例的调用。</span><span class="sxs-lookup"><span data-stu-id="ebb00-174">Invocation of a delegate instance whose invocation list contains multiple entries proceeds by invoking each of the methods in the invocation list, synchronously, in order.</span></span> <span data-ttu-id="ebb00-175">所谓的每个方法传递一组相同的参数与提供给委托实例。</span><span class="sxs-lookup"><span data-stu-id="ebb00-175">Each method so called is passed the same set of arguments as was given to the delegate instance.</span></span> <span data-ttu-id="ebb00-176">如果此类委托调用包括引用参数 ([引用参数](classes.md#reference-parameters))，每个方法调用都将使用相同的变量的引用; 将更改该变量的调用列表中的另一种方法到后面的方法调用列表中向下可见。</span><span class="sxs-lookup"><span data-stu-id="ebb00-176">If such a delegate invocation includes reference parameters ([Reference parameters](classes.md#reference-parameters)), each method invocation will occur with a reference to the same variable; changes to that variable by one method in the invocation list will be visible to methods further down the invocation list.</span></span> <span data-ttu-id="ebb00-177">如果委托调用包含输出参数或返回值，其最终值将来自列表中的最后一个委托的调用。</span><span class="sxs-lookup"><span data-stu-id="ebb00-177">If the delegate invocation includes output parameters or a return value, their final value will come from the invocation of the last delegate in the list.</span></span>

<span data-ttu-id="ebb00-178">如果在处理此类委托的调用期间发生异常，并且未调用的方法中捕获该异常，该异常的 catch 子句的搜索将继续中调用该委托的方法和任何方法的后面部分不会调用调用列表。</span><span class="sxs-lookup"><span data-stu-id="ebb00-178">If an exception occurs during processing of the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, and any methods further down the invocation list are not invoked.</span></span>

<span data-ttu-id="ebb00-179">尝试调用的委托实例，其值为 null 类型的异常将导致`System.NullReferenceException`。</span><span class="sxs-lookup"><span data-stu-id="ebb00-179">Attempting to invoke a delegate instance whose value is null results in an exception of type `System.NullReferenceException`.</span></span>

<span data-ttu-id="ebb00-180">下面的示例演示如何实例化、 合并、 删除和调用委托：</span><span class="sxs-lookup"><span data-stu-id="ebb00-180">The following example shows how to instantiate, combine, remove, and invoke delegates:</span></span>

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

<span data-ttu-id="ebb00-181">该语句中所示`cd3 += cd1;`，多个时间时，可调用列表中出现一个委托。</span><span class="sxs-lookup"><span data-stu-id="ebb00-181">As shown in the statement `cd3 += cd1;`, a delegate can be present in an invocation list multiple times.</span></span> <span data-ttu-id="ebb00-182">在这种情况下，它是只需调用一次每个匹配项。</span><span class="sxs-lookup"><span data-stu-id="ebb00-182">In this case, it is simply invoked once per occurrence.</span></span> <span data-ttu-id="ebb00-183">在这样的调用列表，删除该委托，调用列表中的最后一个匹配项是实际删除。</span><span class="sxs-lookup"><span data-stu-id="ebb00-183">In an invocation list such as this, when that delegate is removed, the last occurrence in the invocation list is the one actually removed.</span></span>

<span data-ttu-id="ebb00-184">最后一条语句执行之前立即`cd3 -= cd1;`，该委托`cd3`引用了一个空的调用列表。</span><span class="sxs-lookup"><span data-stu-id="ebb00-184">Immediately prior to the execution of the final statement, `cd3 -= cd1;`, the delegate `cd3` refers to an empty invocation list.</span></span> <span data-ttu-id="ebb00-185">尝试从空白列表删除委托 （或从非空列表中删除不存在委托） 不是错误。</span><span class="sxs-lookup"><span data-stu-id="ebb00-185">Attempting to remove a delegate from an empty list (or to remove a non-existent delegate from a non-empty list) is not an error.</span></span>

<span data-ttu-id="ebb00-186">生成的输出是：</span><span class="sxs-lookup"><span data-stu-id="ebb00-186">The output produced is:</span></span>

```
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
