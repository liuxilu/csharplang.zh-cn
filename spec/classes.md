---
ms.openlocfilehash: e0def754174ab8646f9b849abe86d2c375c958b6
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703980"
---
# <a name="classes"></a><span data-ttu-id="b24c2-101">类</span><span class="sxs-lookup"><span data-stu-id="b24c2-101">Classes</span></span>

<span data-ttu-id="b24c2-102">类是一种数据结构，它可以包含数据成员（常量和字段）、函数成员（方法、属性、事件、索引器、运算符、实例构造函数、析构函数和静态构造函数）和嵌套类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-102">A class is a data structure that may contain data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="b24c2-103">类类型支持继承，这是一个派生类可以扩展和专用化基类的机制。</span><span class="sxs-lookup"><span data-stu-id="b24c2-103">Class types support inheritance, a mechanism whereby a derived class can extend and specialize a base class.</span></span>

## <a name="class-declarations"></a><span data-ttu-id="b24c2-104">类声明</span><span class="sxs-lookup"><span data-stu-id="b24c2-104">Class declarations</span></span>

<span data-ttu-id="b24c2-105">*Class_declaration*是声明新类的*type_declaration* （[类型声明](namespaces.md#type-declarations)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-105">A *class_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new class.</span></span>

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

<span data-ttu-id="b24c2-106">*Class_declaration*包含一组可选的*特性*（[特性](attributes.md)），后跟一组可选的*class_modifier*s （[类修饰符](classes.md#class-modifiers)），后跟一个可选的 `partial` 修饰符 `class`，然后是一个可选的修饰符，后面跟一个可选的修饰符 *，然后是*一个可选的*type_parameter_list* （[类型参数](classes.md#type-parameters)），后跟一个*可选的 class_base 规范（* [类基本规范](classes.md#class-base-specification)），后跟通过一组可选的*type_parameter_constraints_clause*s （[类型参数约束](classes.md#type-parameter-constraints)），后跟一个*class_body* （[类体](classes.md#class-body)），后面可以跟一个分号。</span><span class="sxs-lookup"><span data-stu-id="b24c2-106">A *class_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *class_modifier*s ([Class modifiers](classes.md#class-modifiers)), followed by an optional `partial` modifier, followed by the keyword `class` and an *identifier* that names the class, followed by an optional *type_parameter_list* ([Type parameters](classes.md#type-parameters)), followed by an optional *class_base* specification ([Class base specification](classes.md#class-base-specification)) , followed by an optional set of *type_parameter_constraints_clause*s ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *class_body* ([Class body](classes.md#class-body)), optionally followed by a semicolon.</span></span>

<span data-ttu-id="b24c2-107">类声明无法提供*type_parameter_constraints_clause*s，除非它还提供了*type_parameter_list*。</span><span class="sxs-lookup"><span data-stu-id="b24c2-107">A class declaration cannot supply *type_parameter_constraints_clause*s unless it also supplies a *type_parameter_list*.</span></span>

<span data-ttu-id="b24c2-108">提供*type_parameter_list*的类声明是***泛型类声明***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-108">A class declaration that supplies a *type_parameter_list* is a ***generic class declaration***.</span></span> <span data-ttu-id="b24c2-109">此外，嵌套在泛型类声明或泛型结构声明中的任何类本身都是一个泛型类声明，因为必须提供包含类型的类型参数才能创建构造类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-109">Additionally, any class nested inside a generic class declaration or a generic struct declaration is itself a generic class declaration, since type parameters for the containing type must be supplied to create a constructed type.</span></span>

### <a name="class-modifiers"></a><span data-ttu-id="b24c2-110">类修饰符</span><span class="sxs-lookup"><span data-stu-id="b24c2-110">Class modifiers</span></span>

<span data-ttu-id="b24c2-111">*Class_declaration*可以选择性地包含一系列类修饰符：</span><span class="sxs-lookup"><span data-stu-id="b24c2-111">A *class_declaration* may optionally include a sequence of class modifiers:</span></span>

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

<span data-ttu-id="b24c2-112">同一修饰符在类声明中多次出现会出现编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-112">It is a compile-time error for the same modifier to appear multiple times in a class declaration.</span></span>

<span data-ttu-id="b24c2-113">在嵌套类上允许 `new` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-113">The `new` modifier is permitted on nested classes.</span></span> <span data-ttu-id="b24c2-114">它指定类按[新修饰符](classes.md#the-new-modifier)中所述隐藏同名的继承成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-114">It specifies that the class hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span> <span data-ttu-id="b24c2-115">`new` 修饰符出现在不是嵌套类声明的类声明中是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-115">It is a compile-time error for the `new` modifier to appear on a class declaration that is not a nested class declaration.</span></span>

<span data-ttu-id="b24c2-116">`public`、`protected`、`internal`和 `private` 修饰符控制类的可访问性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the class.</span></span> <span data-ttu-id="b24c2-117">根据出现类声明的上下文，可能不允许使用其中某些修饰符（[声明的可访问性](basic-concepts.md#declared-accessibility)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-117">Depending on the context in which the class declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="b24c2-118">以下各节将讨论 `abstract`、`sealed` 和 `static` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-118">The `abstract`, `sealed` and `static` modifiers are discussed in the following sections.</span></span>

#### <a name="abstract-classes"></a><span data-ttu-id="b24c2-119">抽象类</span><span class="sxs-lookup"><span data-stu-id="b24c2-119">Abstract classes</span></span>

<span data-ttu-id="b24c2-120">`abstract` 修饰符用于指示某个类不完整，并且它将仅用作一个基类。</span><span class="sxs-lookup"><span data-stu-id="b24c2-120">The `abstract` modifier is used to indicate that a class is incomplete and that it is intended to be used only as a base class.</span></span> <span data-ttu-id="b24c2-121">抽象类与非抽象类在以下方面有所不同：</span><span class="sxs-lookup"><span data-stu-id="b24c2-121">An abstract class differs from a non-abstract class in the following ways:</span></span>

*  <span data-ttu-id="b24c2-122">抽象类不能直接实例化，而是在抽象类上使用 `new` 运算符的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-122">An abstract class cannot be instantiated directly, and it is a compile-time error to use the `new` operator on an abstract class.</span></span> <span data-ttu-id="b24c2-123">尽管可以具有编译时类型为抽象的变量和值，但这类变量和值必须 `null` 或包含对从抽象类型派生的非抽象类的实例的引用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-123">While it is possible to have variables and values whose compile-time types are abstract, such variables and values will necessarily either be `null` or contain references to instances of non-abstract classes derived from the abstract types.</span></span>
*  <span data-ttu-id="b24c2-124">允许（但不要求）抽象类包含抽象成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-124">An abstract class is permitted (but not required) to contain abstract members.</span></span>
*  <span data-ttu-id="b24c2-125">抽象类不能是密封的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-125">An abstract class cannot be sealed.</span></span>

<span data-ttu-id="b24c2-126">如果非抽象类是从抽象类派生的，则非抽象类必须包含所有继承的抽象成员的实际实现，从而重写这些抽象成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-126">When a non-abstract class is derived from an abstract class, the non-abstract class must include actual implementations of all inherited abstract members, thereby overriding those abstract members.</span></span> <span data-ttu-id="b24c2-127">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-127">In the example</span></span>
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
<span data-ttu-id="b24c2-128">抽象类 `A` 引入了 `F`抽象方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-128">the abstract class `A` introduces an abstract method `F`.</span></span> <span data-ttu-id="b24c2-129">类 `B` 引入了附加方法 `G`，但由于该方法不提供 `F`的实现，因此 `B` 还必须声明为抽象。</span><span class="sxs-lookup"><span data-stu-id="b24c2-129">Class `B` introduces an additional method `G`, but since it doesn't provide an implementation of `F`, `B` must also be declared abstract.</span></span> <span data-ttu-id="b24c2-130">类 `C` 重写 `F` 并提供实际实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-130">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="b24c2-131">由于 `C`中没有抽象成员，因此允许（但不要求） `C` 为非抽象。</span><span class="sxs-lookup"><span data-stu-id="b24c2-131">Since there are no abstract members in `C`, `C` is permitted (but not required) to be non-abstract.</span></span>

#### <a name="sealed-classes"></a><span data-ttu-id="b24c2-132">密封类</span><span class="sxs-lookup"><span data-stu-id="b24c2-132">Sealed classes</span></span>

<span data-ttu-id="b24c2-133">`sealed` 修饰符用于阻止从类派生。</span><span class="sxs-lookup"><span data-stu-id="b24c2-133">The `sealed` modifier is used to prevent derivation from a class.</span></span> <span data-ttu-id="b24c2-134">如果将密封类指定为其他类的基类，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-134">A compile-time error occurs if a sealed class is specified as the base class of another class.</span></span>

<span data-ttu-id="b24c2-135">密封类不能也是抽象类。</span><span class="sxs-lookup"><span data-stu-id="b24c2-135">A sealed class cannot also be an abstract class.</span></span>

<span data-ttu-id="b24c2-136">`sealed` 修饰符主要用于防止意外派生，但它还支持某些运行时优化。</span><span class="sxs-lookup"><span data-stu-id="b24c2-136">The `sealed` modifier is primarily used to prevent unintended derivation, but it also enables certain run-time optimizations.</span></span> <span data-ttu-id="b24c2-137">特别是，由于知道密封类绝不会有任何派生类，因此可以将密封类实例上的虚函数成员调用转换为非虚拟调用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-137">In particular, because a sealed class is known to never have any derived classes, it is possible to transform virtual function member invocations on sealed class instances into non-virtual invocations.</span></span>

#### <a name="static-classes"></a><span data-ttu-id="b24c2-138">静态类</span><span class="sxs-lookup"><span data-stu-id="b24c2-138">Static classes</span></span>

<span data-ttu-id="b24c2-139">`static` 修饰符用于标记声明为***静态类***的类。</span><span class="sxs-lookup"><span data-stu-id="b24c2-139">The `static` modifier is used to mark the class being declared as a ***static class***.</span></span> <span data-ttu-id="b24c2-140">静态类不能被实例化，不能用作类型并且只能包含静态成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-140">A static class cannot be instantiated, cannot be used as a type and can contain only static members.</span></span> <span data-ttu-id="b24c2-141">只有静态类可以包含扩展方法的声明（[扩展方法](classes.md#extension-methods)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-141">Only a static class can contain declarations of extension methods ([Extension methods](classes.md#extension-methods)).</span></span>

<span data-ttu-id="b24c2-142">静态类声明受到下列限制：</span><span class="sxs-lookup"><span data-stu-id="b24c2-142">A static class declaration is subject to the following restrictions:</span></span>

*  <span data-ttu-id="b24c2-143">静态类不能包含 `sealed` 或 `abstract` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-143">A static class may not include a `sealed` or `abstract` modifier.</span></span> <span data-ttu-id="b24c2-144">但请注意，由于无法对静态类进行实例化或派生，因此它的行为就像它既是密封的又是抽象的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-144">Note, however, that since a static class cannot be instantiated or derived from, it behaves as if it was both sealed and abstract.</span></span>
*  <span data-ttu-id="b24c2-145">静态类不能包含*class_base*规范（[类基规范](classes.md#class-base-specification)），并且无法显式指定基类或实现的接口列表。</span><span class="sxs-lookup"><span data-stu-id="b24c2-145">A static class may not include a *class_base* specification ([Class base specification](classes.md#class-base-specification)) and cannot explicitly specify a base class or a list of implemented interfaces.</span></span> <span data-ttu-id="b24c2-146">静态类隐式继承自类型 `object`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-146">A static class implicitly inherits from type `object`.</span></span>
*  <span data-ttu-id="b24c2-147">静态类只能包含静态成员（[静态成员和实例成员](classes.md#static-and-instance-members)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-147">A static class can only contain static members ([Static and instance members](classes.md#static-and-instance-members)).</span></span> <span data-ttu-id="b24c2-148">请注意，常量和嵌套类型归类为静态成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-148">Note that constants and nested types are classified as static members.</span></span>
*  <span data-ttu-id="b24c2-149">静态类不能包含具有 `protected` 或声明可访问性 `protected internal` 的成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-149">A static class cannot have members with `protected` or `protected internal` declared accessibility.</span></span>

<span data-ttu-id="b24c2-150">这是一种编译时错误，违反其中的任何限制。</span><span class="sxs-lookup"><span data-stu-id="b24c2-150">It is a compile-time error to violate any of these restrictions.</span></span>

<span data-ttu-id="b24c2-151">静态类没有实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-151">A static class has no instance constructors.</span></span> <span data-ttu-id="b24c2-152">不能在静态类中声明实例构造函数，也不会为静态类提供默认的实例构造函数（[默认构造函数](classes.md#default-constructors)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-152">It is not possible to declare an instance constructor in a static class, and no default instance constructor ([Default constructors](classes.md#default-constructors)) is provided for a static class.</span></span>

<span data-ttu-id="b24c2-153">静态类的成员不是自动静态的，成员声明必须显式包含 `static` 修饰符（常量和嵌套类型除外）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-153">The members of a static class are not automatically static, and the member declarations must explicitly include a `static` modifier (except for constants and nested types).</span></span> <span data-ttu-id="b24c2-154">当类嵌套在静态外部类中时，嵌套类不是静态类，除非它显式包含 `static` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-154">When a class is nested within a static outer class, the nested class is not a static class unless it explicitly includes a `static` modifier.</span></span>

<span data-ttu-id="b24c2-155">__引用静态类类型__</span><span class="sxs-lookup"><span data-stu-id="b24c2-155">__Referencing static class types__</span></span>

<span data-ttu-id="b24c2-156">如果有*namespace_or_type_name* （[命名空间和类型名称](basic-concepts.md#namespace-and-type-names)），则允许引用静态类</span><span class="sxs-lookup"><span data-stu-id="b24c2-156">A *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="b24c2-157">*Namespace_or_type_name*是窗体 `T.I`*namespace_or_type_name*的 `T`，或</span><span class="sxs-lookup"><span data-stu-id="b24c2-157">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="b24c2-158">*Namespace_or_type_name*是窗体 `typeof(T)`的*typeof_expression* （[参数列表](expressions.md#argument-lists)1）中的 `T`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-158">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

<span data-ttu-id="b24c2-159">如果*primary_expression* （[函数成员](expressions.md#function-members)），则允许引用静态类</span><span class="sxs-lookup"><span data-stu-id="b24c2-159">A *primary_expression* ([Function members](expressions.md#function-members)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="b24c2-160">*Primary_expression*是窗体 `E.I`的*member_access*中的 `E` （对[动态重载决策进行编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-160">The *primary_expression* is the `E` in a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) of the form `E.I`.</span></span>

<span data-ttu-id="b24c2-161">在其他任何上下文中，引用静态类会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-161">In any other context it is a compile-time error to reference a static class.</span></span> <span data-ttu-id="b24c2-162">例如，将静态类用作基类、成员的构成类型（[嵌套类型](classes.md#nested-types)）、泛型类型参数或类型参数约束时，将会出现错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-162">For example, it is an error for a static class to be used as a base class, a constituent type ([Nested types](classes.md#nested-types)) of a member, a generic type argument, or a type parameter constraint.</span></span> <span data-ttu-id="b24c2-163">同样，静态类不能用于数组类型、指针类型、`new` 表达式、强制转换表达式、`is` 表达式、`as` 表达式、`sizeof` 表达式或默认值表达式。</span><span class="sxs-lookup"><span data-stu-id="b24c2-163">Likewise, a static class cannot be used in an array type, a pointer type, a `new` expression, a cast expression, an `is` expression, an `as` expression, a `sizeof` expression, or a default value expression.</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="b24c2-164">Partial 修饰符</span><span class="sxs-lookup"><span data-stu-id="b24c2-164">Partial modifier</span></span>

<span data-ttu-id="b24c2-165">`partial` 修饰符用于指示此*class_declaration*为分部类型声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-165">The `partial` modifier is used to indicate that this *class_declaration* is a partial type declaration.</span></span> <span data-ttu-id="b24c2-166">具有相同名称的多个分部类型声明在封闭命名空间或类型声明中组合在一起，并遵循在[部分类型](classes.md#partial-types)中指定的规则组成一个类型声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-166">Multiple partial type declarations with the same name within an enclosing namespace or type declaration combine to form one type declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

<span data-ttu-id="b24c2-167">如果在不同的上下文中生成或维护这些段，则通过单独的程序文本段来声明的类的声明会很有用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-167">Having the declaration of a class distributed over separate segments of program text can be useful if these segments are produced or maintained in different contexts.</span></span> <span data-ttu-id="b24c2-168">例如，类声明的一部分可以是计算机生成的，而另一个是手动编写的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-168">For instance, one part of a class declaration may be machine generated, whereas the other is manually authored.</span></span> <span data-ttu-id="b24c2-169">这两者的文本分离可防止更新中的一个发生更新。</span><span class="sxs-lookup"><span data-stu-id="b24c2-169">Textual separation of the two prevents updates by one from conflicting with updates by the other.</span></span>

### <a name="type-parameters"></a><span data-ttu-id="b24c2-170">类型参数</span><span class="sxs-lookup"><span data-stu-id="b24c2-170">Type parameters</span></span>

<span data-ttu-id="b24c2-171">类型参数是一个简单标识符，它表示为创建构造类型而提供的类型参数的占位符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-171">A type parameter is a simple identifier that denotes a placeholder for a type argument supplied to create a constructed type.</span></span> <span data-ttu-id="b24c2-172">类型参数是将稍后提供的类型的正式占位符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-172">A type parameter is a formal placeholder for a type that will be supplied later.</span></span> <span data-ttu-id="b24c2-173">与此相反，类型参数（[类型参数](types.md#type-arguments)）是在创建构造类型时替换类型参数的实际类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-173">By contrast, a type argument ([Type arguments](types.md#type-arguments)) is the actual type that is substituted for the type parameter when a constructed type is created.</span></span>

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

<span data-ttu-id="b24c2-174">类声明中的每个类型参数在该类的声明空间（[声明](basic-concepts.md#declarations)）中定义一个名称。</span><span class="sxs-lookup"><span data-stu-id="b24c2-174">Each type parameter in a class declaration defines a name in the declaration space ([Declarations](basic-concepts.md#declarations)) of that class.</span></span> <span data-ttu-id="b24c2-175">因此，它不能与另一个类型参数或该类中声明的成员同名。</span><span class="sxs-lookup"><span data-stu-id="b24c2-175">Thus, it cannot have the same name as another type parameter or a member declared in that class.</span></span> <span data-ttu-id="b24c2-176">类型参数不能与类型本身具有相同的名称。</span><span class="sxs-lookup"><span data-stu-id="b24c2-176">A type parameter cannot have the same name as the type itself.</span></span>

### <a name="class-base-specification"></a><span data-ttu-id="b24c2-177">类基规范</span><span class="sxs-lookup"><span data-stu-id="b24c2-177">Class base specification</span></span>

<span data-ttu-id="b24c2-178">类声明可以包含*class_base*规范，该规范定义类的直接基类和类直接实现的接口（[接口](interfaces.md)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-178">A class declaration may include a *class_base* specification, which defines the direct base class of the class and the interfaces ([Interfaces](interfaces.md)) directly implemented by the class.</span></span>

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

<span data-ttu-id="b24c2-179">类声明中指定的基类可以是构造类类型（[构造类型](types.md#constructed-types)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-179">The base class specified in a class declaration can be a constructed class type ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="b24c2-180">基类本身不能是类型参数，但它可以涉及范围内的类型参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-180">A base class cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span>

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a><span data-ttu-id="b24c2-181">基类</span><span class="sxs-lookup"><span data-stu-id="b24c2-181">Base classes</span></span>

<span data-ttu-id="b24c2-182">*Class_type*包含在*class_base*中时，它将指定所声明的类的直接基类。</span><span class="sxs-lookup"><span data-stu-id="b24c2-182">When a *class_type* is included in the *class_base*, it specifies the direct base class of the class being declared.</span></span> <span data-ttu-id="b24c2-183">如果类声明没有*class_base*，或者*class_base*只列出接口类型，则假定直接基类是 `object`的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-183">If a class declaration has no *class_base*, or if the *class_base* lists only interface types, the direct base class is assumed to be `object`.</span></span> <span data-ttu-id="b24c2-184">类从其直接基类继承成员，如[继承](classes.md#inheritance)中所述。</span><span class="sxs-lookup"><span data-stu-id="b24c2-184">A class inherits members from its direct base class, as described in [Inheritance](classes.md#inheritance).</span></span>

<span data-ttu-id="b24c2-185">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-185">In the example</span></span>
```csharp
class A {}

class B: A {}
```
<span data-ttu-id="b24c2-186">类 `A` 称为 `B`的直接基类，`B` 称为从 `A`派生。</span><span class="sxs-lookup"><span data-stu-id="b24c2-186">class `A` is said to be the direct base class of `B`, and `B` is said to be derived from `A`.</span></span> <span data-ttu-id="b24c2-187">由于 `A` 未显式指定直接基类，因此它的直接基类被隐式 `object`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-187">Since `A` does not explicitly specify a direct base class, its direct base class is implicitly `object`.</span></span>

<span data-ttu-id="b24c2-188">对于构造类类型，如果在泛型类声明中指定了基类，则会通过将基类声明中的每个*type_parameter*替换为构造的类型的相应*type_argument*来获取构造类型的基类。</span><span class="sxs-lookup"><span data-stu-id="b24c2-188">For a constructed class type, if a base class is specified in the generic class declaration, the base class of the constructed type is obtained by substituting, for each *type_parameter* in the base class declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="b24c2-189">给定泛型类声明</span><span class="sxs-lookup"><span data-stu-id="b24c2-189">Given the generic class declarations</span></span>
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
<span data-ttu-id="b24c2-190">构造类型的基类 `G<int>` 将 `B<string,int[]>`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-190">the base class of the constructed type `G<int>` would be `B<string,int[]>`.</span></span>

<span data-ttu-id="b24c2-191">类类型的直接基类必须至少具有与类类型本身相同的可访问性（[可访问域](basic-concepts.md#accessibility-domains)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-191">The direct base class of a class type must be at least as accessible as the class type itself ([Accessibility domains](basic-concepts.md#accessibility-domains)).</span></span> <span data-ttu-id="b24c2-192">例如，`public` 类派生自 `private` 或 `internal` 类的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-192">For example, it is a compile-time error for a `public` class to derive from a `private` or `internal` class.</span></span>

<span data-ttu-id="b24c2-193">类类型的直接基类不得为以下类型之一： `System.Array`、`System.Delegate`、`System.MulticastDelegate`、`System.Enum`或 `System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-193">The direct base class of a class type must not be any of the following types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="b24c2-194">此外，泛型类声明不能将 `System.Attribute` 用作直接或间接的基类。</span><span class="sxs-lookup"><span data-stu-id="b24c2-194">Furthermore, a generic class declaration cannot use `System.Attribute` as a direct or indirect base class.</span></span>

<span data-ttu-id="b24c2-195">确定类 `B`的直接基类规范 `A` 的含义时，`B` 的直接基类暂时假设为 `object`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-195">While determining the meaning of the direct base class specification `A` of a class `B`, the direct base class of `B` is temporarily assumed to be `object`.</span></span> <span data-ttu-id="b24c2-196">这可以确保基类规范的含义无法以递归方式依赖于自身。</span><span class="sxs-lookup"><span data-stu-id="b24c2-196">Intuitively this ensures that the meaning of a base class specification cannot recursively depend on itself.</span></span> <span data-ttu-id="b24c2-197">示例：</span><span class="sxs-lookup"><span data-stu-id="b24c2-197">The example:</span></span>
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
<span data-ttu-id="b24c2-198">存在错误，因为在基类规范中 `A<C.B>` `C` 的直接基类被视为 `object`，因此（由[命名空间和类型名称](basic-concepts.md#namespace-and-type-names)的规则） `C` 不被视为具有成员 `B`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-198">is in error since in the base class specification `A<C.B>` the direct base class of `C` is considered to be `object`, and hence (by the rules of [Namespace and type names](basic-concepts.md#namespace-and-type-names))  `C` is not considered to have a member `B`.</span></span>

<span data-ttu-id="b24c2-199">类类型的基类是直接基类及其基类。</span><span class="sxs-lookup"><span data-stu-id="b24c2-199">The base classes of a class type are the direct base class and its base classes.</span></span> <span data-ttu-id="b24c2-200">换言之，基类集是直接基类关系的传递闭包。</span><span class="sxs-lookup"><span data-stu-id="b24c2-200">In other words, the set of base classes is the transitive closure of the direct base class relationship.</span></span> <span data-ttu-id="b24c2-201">参考上面的示例，`B` 的基类 `A` 并 `object`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-201">Referring to the example above, the base classes of `B` are `A` and `object`.</span></span> <span data-ttu-id="b24c2-202">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-202">In the example</span></span>
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
<span data-ttu-id="b24c2-203">`D<int>` 的基类 `C<int[]>`、`B<IComparable<int[]>>`、`A`和 `object`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-203">the base classes of `D<int>` are `C<int[]>`, `B<IComparable<int[]>>`, `A`, and `object`.</span></span>

<span data-ttu-id="b24c2-204">除了类 `object`，每个类类型都有一个直接基类。</span><span class="sxs-lookup"><span data-stu-id="b24c2-204">Except for class `object`, every class type has exactly one direct base class.</span></span> <span data-ttu-id="b24c2-205">`object` 类没有直接基类，是其他所有类的最终基类。</span><span class="sxs-lookup"><span data-stu-id="b24c2-205">The `object` class has no direct base class and is the ultimate base class of all other classes.</span></span>

<span data-ttu-id="b24c2-206">当类 `B` 从类 `A`派生时，`A` 依赖于 `B`，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-206">When a class `B` derives from a class `A`, it is a compile-time error for `A` to depend on `B`.</span></span> <span data-ttu-id="b24c2-207">类***直接依赖于***其直接基类（如果有），并且***直接依赖于***直接嵌套它的类（如果有）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-207">A class ***directly depends on*** its direct base class (if any) and ***directly depends on*** the class within which it is immediately nested (if any).</span></span> <span data-ttu-id="b24c2-208">根据此定义，类所依赖的完整类集是***直接依赖***关系的反身和可传递闭包。</span><span class="sxs-lookup"><span data-stu-id="b24c2-208">Given this definition, the complete set of classes upon which a class depends is the reflexive and transitive closure of the ***directly depends on*** relationship.</span></span>

<span data-ttu-id="b24c2-209">示例</span><span class="sxs-lookup"><span data-stu-id="b24c2-209">The example</span></span>
```csharp
class A: A {}
```
<span data-ttu-id="b24c2-210">是错误的，因为类依赖于自身。</span><span class="sxs-lookup"><span data-stu-id="b24c2-210">is erroneous because the class depends on itself.</span></span> <span data-ttu-id="b24c2-211">同样，示例</span><span class="sxs-lookup"><span data-stu-id="b24c2-211">Likewise, the example</span></span>
```csharp
class A: B {}
class B: C {}
class C: A {}
```
<span data-ttu-id="b24c2-212">出现错误，因为类循环依赖于自身。</span><span class="sxs-lookup"><span data-stu-id="b24c2-212">is in error because the classes circularly depend on themselves.</span></span> <span data-ttu-id="b24c2-213">最后，示例</span><span class="sxs-lookup"><span data-stu-id="b24c2-213">Finally, the example</span></span>
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
<span data-ttu-id="b24c2-214">导致编译时错误，这是因为 `A` 依赖于 `B.C` （其直接基类），这取决于 `B` （它的直接封闭类），后者循环依赖于 `A`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-214">results in a compile-time error because `A` depends on `B.C` (its direct base class), which depends on `B` (its immediately enclosing class), which circularly depends on `A`.</span></span>

<span data-ttu-id="b24c2-215">请注意，类不依赖于嵌套在其中的类。</span><span class="sxs-lookup"><span data-stu-id="b24c2-215">Note that a class does not depend on the classes that are nested within it.</span></span> <span data-ttu-id="b24c2-216">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-216">In the example</span></span>
```csharp
class A
{
    class B: A {}
}
```
<span data-ttu-id="b24c2-217">`B` 依赖于 `A` （因为 `A` 既是它的直接基类，也是它的直接封闭类），但 `A` 不依赖于 `B` （因为 `B` 既不是基类，也不是 `A`的封闭类）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-217">`B` depends on `A` (because `A` is both its direct base class and its immediately enclosing class), but `A` does not depend on `B` (since `B` is neither a base class nor an enclosing class of `A`).</span></span> <span data-ttu-id="b24c2-218">因此，该示例是有效的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-218">Thus, the example is valid.</span></span>

<span data-ttu-id="b24c2-219">不能从 `sealed` 类派生。</span><span class="sxs-lookup"><span data-stu-id="b24c2-219">It is not possible to derive from a `sealed` class.</span></span> <span data-ttu-id="b24c2-220">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-220">In the example</span></span>
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
<span data-ttu-id="b24c2-221">类 `B` 出错，因为它尝试从 `sealed` 类 `A`派生。</span><span class="sxs-lookup"><span data-stu-id="b24c2-221">class `B` is in error because it attempts to derive from the `sealed` class `A`.</span></span>

#### <a name="interface-implementations"></a><span data-ttu-id="b24c2-222">接口实现</span><span class="sxs-lookup"><span data-stu-id="b24c2-222">Interface implementations</span></span>

<span data-ttu-id="b24c2-223">*Class_base*规范可能包含一系列接口类型，在这种情况下，类可以直接实现给定的接口类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-223">A *class_base* specification may include a list of interface types, in which case the class is said to directly implement the given interface types.</span></span> <span data-ttu-id="b24c2-224">[接口实现中进一步](interfaces.md#interface-implementations)讨论了接口实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-224">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="type-parameter-constraints"></a><span data-ttu-id="b24c2-225">类型参数约束</span><span class="sxs-lookup"><span data-stu-id="b24c2-225">Type parameter constraints</span></span>

<span data-ttu-id="b24c2-226">泛型类型和方法声明可以选择通过包含*type_parameter_constraints_clause*来指定类型参数约束。</span><span class="sxs-lookup"><span data-stu-id="b24c2-226">Generic type and method declarations can optionally specify type parameter constraints by including *type_parameter_constraints_clause*s.</span></span>

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

<span data-ttu-id="b24c2-227">每个*type_parameter_constraints_clause*都包含标记 `where`，后跟类型参数的名称，后跟一个冒号和该类型参数的约束列表。</span><span class="sxs-lookup"><span data-stu-id="b24c2-227">Each *type_parameter_constraints_clause* consists of the token `where`, followed by the name of a type parameter, followed by a colon and the list of constraints for that type parameter.</span></span> <span data-ttu-id="b24c2-228">每个类型参数最多只能有一个 `where` 子句，`where` 子句可以按任意顺序列出。</span><span class="sxs-lookup"><span data-stu-id="b24c2-228">There can be at most one `where` clause for each type parameter, and the `where` clauses can be listed in any order.</span></span> <span data-ttu-id="b24c2-229">与 `get` 并 `set` 属性访问器中的标记一样，`where` 标记不是关键字。</span><span class="sxs-lookup"><span data-stu-id="b24c2-229">Like the `get` and `set` tokens in a property accessor, the `where` token is not a keyword.</span></span>

<span data-ttu-id="b24c2-230">`where` 子句中给定的约束列表可以包括以下任何组件（按以下顺序）：单个主约束、一个或多个辅助约束以及构造函数约束 `new()`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-230">The list of constraints given in a `where` clause can include any of the following components, in this order: a single primary constraint, one or more secondary constraints, and the constructor constraint, `new()`.</span></span>

<span data-ttu-id="b24c2-231">主约束可以是类类型或***引用类型约束***`class` 或 `struct`***值类型约束***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-231">A primary constraint can be a class type or the ***reference type constraint*** `class` or the ***value type constraint*** `struct`.</span></span> <span data-ttu-id="b24c2-232">辅助约束可以是*type_parameter*或*interface_type*。</span><span class="sxs-lookup"><span data-stu-id="b24c2-232">A secondary constraint can be a *type_parameter* or *interface_type*.</span></span>

<span data-ttu-id="b24c2-233">引用类型约束指定用于类型参数的类型参数必须是引用类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-233">The reference type constraint specifies that a type argument used for the type parameter must be a reference type.</span></span> <span data-ttu-id="b24c2-234">已知为引用类型的所有类类型、接口类型、委托类型、数组类型和类型参数均满足此约束。</span><span class="sxs-lookup"><span data-stu-id="b24c2-234">All class types, interface types, delegate types, array types, and type parameters known to be a reference type (as defined below) satisfy this constraint.</span></span>

<span data-ttu-id="b24c2-235">值类型约束指定用于类型参数的类型参数必须是不可以为 null 的值类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-235">The value type constraint specifies that a type argument used for the type parameter must be a non-nullable value type.</span></span> <span data-ttu-id="b24c2-236">所有不可以为 null 的结构类型、枚举类型和具有值类型约束的类型参数均满足此约束。</span><span class="sxs-lookup"><span data-stu-id="b24c2-236">All non-nullable struct types, enum types, and type parameters having the value type constraint satisfy this constraint.</span></span> <span data-ttu-id="b24c2-237">请注意，尽管归类为值类型，但可以为 null 的类型（[可为 null 的类型](types.md#nullable-types)）不满足值类型约束。</span><span class="sxs-lookup"><span data-stu-id="b24c2-237">Note that although classified as a value type, a nullable type ([Nullable types](types.md#nullable-types)) does not satisfy the value type constraint.</span></span> <span data-ttu-id="b24c2-238">具有值类型约束的类型形参也不能具有*constructor_constraint*。</span><span class="sxs-lookup"><span data-stu-id="b24c2-238">A type parameter having the value type constraint cannot also have the *constructor_constraint*.</span></span>

<span data-ttu-id="b24c2-239">指针类型永远不允许为类型参数，并且不被视为满足引用类型或值类型约束。</span><span class="sxs-lookup"><span data-stu-id="b24c2-239">Pointer types are never allowed to be type arguments and are not considered to satisfy either the reference type or value type constraints.</span></span>

<span data-ttu-id="b24c2-240">如果约束是类类型、接口类型或类型参数，则该类型指定用于该类型参数的每个类型参数都必须支持的最小 "基类型"。</span><span class="sxs-lookup"><span data-stu-id="b24c2-240">If a constraint is a class type, an interface type, or a type parameter, that type specifies a minimal "base type" that every type argument used for that type parameter must support.</span></span> <span data-ttu-id="b24c2-241">当使用构造类型或泛型方法时，将在编译时对照类型参数上的约束检查类型参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-241">Whenever a constructed type or generic method is used, the type argument is checked against the constraints on the type parameter at compile-time.</span></span> <span data-ttu-id="b24c2-242">提供的类型参数必须满足[满足约束](types.md#satisfying-constraints)中所述的条件。</span><span class="sxs-lookup"><span data-stu-id="b24c2-242">The type argument supplied must satisfy the conditions described in [Satisfying constraints](types.md#satisfying-constraints).</span></span>

<span data-ttu-id="b24c2-243">*Class_type*约束必须满足以下规则：</span><span class="sxs-lookup"><span data-stu-id="b24c2-243">A *class_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="b24c2-244">类型必须是类类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-244">The type must be a class type.</span></span>
*  <span data-ttu-id="b24c2-245">类型不得 `sealed`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-245">The type must not be `sealed`.</span></span>
*  <span data-ttu-id="b24c2-246">类型不得为以下类型之一： `System.Array`、`System.Delegate`、`System.Enum`或 `System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-246">The type must not be one of the following types: `System.Array`, `System.Delegate`, `System.Enum`, or `System.ValueType`.</span></span>
*  <span data-ttu-id="b24c2-247">类型不得 `object`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-247">The type must not be `object`.</span></span> <span data-ttu-id="b24c2-248">由于所有类型都派生自 `object`，因此如果允许，此类约束将不起作用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-248">Because all types derive from `object`, such a constraint would have no effect if it were permitted.</span></span>
*  <span data-ttu-id="b24c2-249">给定类型参数最多只能有一个约束为类类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-249">At most one constraint for a given type parameter can be a class type.</span></span>

<span data-ttu-id="b24c2-250">指定为*interface_type*约束的类型必须满足以下规则：</span><span class="sxs-lookup"><span data-stu-id="b24c2-250">A type specified as an *interface_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="b24c2-251">类型必须是接口类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-251">The type must be an interface type.</span></span>
*  <span data-ttu-id="b24c2-252">在给定的 `where` 子句中不能多次指定一个类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-252">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="b24c2-253">在任一情况下，约束都可以涉及关联的类型或方法声明的任何类型参数作为构造类型的一部分，并且可以涉及所声明的类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-253">In either case, the constraint can involve any of the type parameters of the associated type or method declaration as part of a constructed type, and can involve the type being declared.</span></span>

<span data-ttu-id="b24c2-254">指定为类型参数约束的任何类或接口类型必须至少具有与声明的泛型类型或方法相同的可访问性（[辅助功能约束](basic-concepts.md#accessibility-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-254">Any class or interface type specified as a type parameter constraint must be at least as accessible ([Accessibility constraints](basic-concepts.md#accessibility-constraints)) as the generic type or method being declared.</span></span>

<span data-ttu-id="b24c2-255">指定为*type_parameter*约束的类型必须满足以下规则：</span><span class="sxs-lookup"><span data-stu-id="b24c2-255">A type specified as a *type_parameter* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="b24c2-256">类型必须为类型参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-256">The type must be a type parameter.</span></span>
*  <span data-ttu-id="b24c2-257">在给定的 `where` 子句中不能多次指定一个类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-257">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="b24c2-258">此外，类型参数的依赖项关系图中必须没有循环，其中依赖项是由定义的传递关系：</span><span class="sxs-lookup"><span data-stu-id="b24c2-258">In addition there must be no cycles in the dependency graph of type parameters, where dependency is a transitive relation defined by:</span></span>

*  <span data-ttu-id="b24c2-259">如果类型参数 `T` 用作类型参数的约束 `S` 则 `S`***依赖于***`T`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-259">If a type parameter `T` is used as a constraint for type parameter `S` then `S` ***depends on*** `T`.</span></span>
*  <span data-ttu-id="b24c2-260">如果类型参数 `S` 依赖于类型参数 `T` 并且 `T` 依赖于类型 `U` 参数，则 `S`***取决于***`U`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-260">If a type parameter `S` depends on a type parameter `T` and `T` depends on a type parameter `U` then `S` ***depends on*** `U`.</span></span>

<span data-ttu-id="b24c2-261">在给定此关系的情况下，如果类型参数依赖于自身（直接或间接），则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-261">Given this relation, it is a compile-time error for a type parameter to depend on itself (directly or indirectly).</span></span>

<span data-ttu-id="b24c2-262">所有约束必须在依赖类型参数之间保持一致。</span><span class="sxs-lookup"><span data-stu-id="b24c2-262">Any constraints must be consistent among dependent type parameters.</span></span> <span data-ttu-id="b24c2-263">如果类型参数 `S` 依赖于类型参数 `T` 则：</span><span class="sxs-lookup"><span data-stu-id="b24c2-263">If type parameter `S` depends on type parameter `T` then:</span></span>

*  <span data-ttu-id="b24c2-264">`T` 不得具有值类型约束。</span><span class="sxs-lookup"><span data-stu-id="b24c2-264">`T` must not have the value type constraint.</span></span> <span data-ttu-id="b24c2-265">否则，`T` 将被有效密封，因此 `S` 强制与 `T`的类型相同，从而无需两个类型参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-265">Otherwise, `T` is effectively sealed so `S` would be forced to be the same type as `T`, eliminating the need for two type parameters.</span></span>
*  <span data-ttu-id="b24c2-266">如果 `S` 具有值类型约束，则 `T` 不能具有*class_type*约束。</span><span class="sxs-lookup"><span data-stu-id="b24c2-266">If `S` has the value type constraint then `T` must not have a *class_type* constraint.</span></span>
*  <span data-ttu-id="b24c2-267">如果 `S` 具有*class_type*约束 `A` 并且 `T` 具有*class_type*约束，则必须存在从 `B` 到 `A` 或从 `B` 到 `B` 的隐式引用转换。`A`</span><span class="sxs-lookup"><span data-stu-id="b24c2-267">If `S` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>
*  <span data-ttu-id="b24c2-268">如果 `S` 还依赖于类型参数 `U` 并且 `U` 具有*class_type*约束 `A` 并且 `T` 具有*class_type*的约束，则必须存在从 `B` 到 `A` 或从 `B` 到 `B` 的隐式引用转换。`A`</span><span class="sxs-lookup"><span data-stu-id="b24c2-268">If `S` also depends on type parameter `U` and `U` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>

<span data-ttu-id="b24c2-269">`S` 具有值类型约束，并且 `T` 具有引用类型约束，则有效。</span><span class="sxs-lookup"><span data-stu-id="b24c2-269">It is valid for `S` to have the value type constraint and `T` to have the reference type constraint.</span></span> <span data-ttu-id="b24c2-270">有效地限制 `T` `System.Object`、`System.ValueType`、`System.Enum`和任何接口类型的类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-270">Effectively this limits `T` to the types `System.Object`, `System.ValueType`, `System.Enum`, and any interface type.</span></span>

<span data-ttu-id="b24c2-271">如果类型参数的 `where` 子句包含构造函数约束（具有 `new()`格式），则可以使用 `new` 运算符来创建类型的实例（[对象创建表达式](expressions.md#object-creation-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-271">If the `where` clause for a type parameter includes a constructor constraint (which has the form `new()`), it is possible to use the `new` operator to create instances of the type ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span> <span data-ttu-id="b24c2-272">用于具有构造函数约束的类型参数的任何类型参数都必须具有一个公共的无参数构造函数（此构造函数对于任何值类型都是隐式的），或是具有值类型约束或构造函数约束的类型参数（有关详细信息，请参阅[类型参数约束](classes.md#type-parameter-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-272">Any type argument used for a type parameter with a constructor constraint must have a public parameterless constructor (this constructor implicitly exists for any value type) or be a type parameter having the value type constraint or constructor constraint (see [Type parameter constraints](classes.md#type-parameter-constraints) for details).</span></span>

<span data-ttu-id="b24c2-273">下面是约束的示例：</span><span class="sxs-lookup"><span data-stu-id="b24c2-273">The following are examples of constraints:</span></span>
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

<span data-ttu-id="b24c2-274">下面的示例出错，因为它在类型参数的依赖项关系图中导致循环：</span><span class="sxs-lookup"><span data-stu-id="b24c2-274">The following example is in error because it causes a circularity in the dependency graph of the type parameters:</span></span>
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

<span data-ttu-id="b24c2-275">下面的示例演示了其他无效情况：</span><span class="sxs-lookup"><span data-stu-id="b24c2-275">The following examples illustrate additional invalid situations:</span></span>
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

<span data-ttu-id="b24c2-276">类型参数 `T` 的***有效基类***定义如下：</span><span class="sxs-lookup"><span data-stu-id="b24c2-276">The ***effective base class*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="b24c2-277">如果 `T` 没有主约束或类型参数约束，则其有效基类将 `object`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-277">If `T` has no primary constraints or type parameter constraints, its effective base class is `object`.</span></span>
*  <span data-ttu-id="b24c2-278">如果 `T` 具有值类型约束，则其有效基类将 `System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-278">If `T` has the value type constraint, its effective base class is `System.ValueType`.</span></span>
*  <span data-ttu-id="b24c2-279">如果 `T` *class_type*约束 `C` 但没有*type_parameter*约束，则它的有效基类是 `C`的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-279">If `T` has a *class_type* constraint `C` but no *type_parameter* constraints, its effective base class is `C`.</span></span>
*  <span data-ttu-id="b24c2-280">如果 `T` 没有*class_type*约束，但具有一个或多个*type_parameter*约束，则它的有效基类是其*type_parameter*约束的一组有效基类中包含程度最高的类型（[提升转换运算符](conversions.md#lifted-conversion-operators)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-280">If `T` has no *class_type* constraint but has one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set of effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="b24c2-281">一致性规则确保存在这种包含最多的类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-281">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="b24c2-282">如果 `T` 同时具有*class_type*约束和一个或多个*type_parameter*约束，则它的有效基类是包含 `T` 的*class_type*约束和其*type_parameter*约束的有效基类的集中包含程度最高的类型（[提升转换运算符](conversions.md#lifted-conversion-operators)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-282">If `T` has both a *class_type* constraint and one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set consisting of the *class_type* constraint of `T` and the effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="b24c2-283">一致性规则确保存在这种包含最多的类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-283">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="b24c2-284">如果 `T` 具有引用类型约束但没有*class_type*约束，则它的有效基类是 `object`的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-284">If `T` has the reference type constraint but no *class_type* constraints, its effective base class is `object`.</span></span>

<span data-ttu-id="b24c2-285">对于这些规则，如果 T 的约束 `V` 是*value_type*，请改用*class_type*的最特定类型 `V` 的基类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-285">For the purpose of these rules, if T has a constraint `V` that is a *value_type*, use instead the most specific base type of `V` that is a *class_type*.</span></span> <span data-ttu-id="b24c2-286">此操作永远不会在显式给定的约束中发生，但当通过重写方法声明或接口方法的显式实现隐式继承泛型方法时，可能会出现这种情况。</span><span class="sxs-lookup"><span data-stu-id="b24c2-286">This can never happen in an explicitly given constraint, but may occur when the constraints of a generic method are implicitly inherited by an overriding method declaration or an explicit implementation of an interface method.</span></span>

<span data-ttu-id="b24c2-287">这些规则确保有效的基类始终是*class_type*。</span><span class="sxs-lookup"><span data-stu-id="b24c2-287">These rules ensure that the effective base class is always a *class_type*.</span></span>

<span data-ttu-id="b24c2-288">类型参数 `T` 的***有效接口集***定义如下：</span><span class="sxs-lookup"><span data-stu-id="b24c2-288">The ***effective interface set*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="b24c2-289">如果 `T` 没有*secondary_constraints*，则其有效接口集为空。</span><span class="sxs-lookup"><span data-stu-id="b24c2-289">If `T` has no *secondary_constraints*, its effective interface set is empty.</span></span>
*  <span data-ttu-id="b24c2-290">如果 `T` 具有*interface_type*约束但没有*type_parameter*约束，则其有效接口集为其*interface_type*约束的集合。</span><span class="sxs-lookup"><span data-stu-id="b24c2-290">If `T` has *interface_type* constraints but no *type_parameter* constraints, its effective interface set is its set of *interface_type* constraints.</span></span>
*  <span data-ttu-id="b24c2-291">如果 `T` 没有*interface_type*约束但具有*type_parameter*约束，则其有效接口集是其*type_parameter*约束的有效接口集的并集。</span><span class="sxs-lookup"><span data-stu-id="b24c2-291">If `T` has no *interface_type* constraints but has *type_parameter* constraints, its effective interface set is the union of the effective interface sets of its *type_parameter* constraints.</span></span>
*  <span data-ttu-id="b24c2-292">如果 `T` 同时具有*interface_type*约束和*type_parameter*约束，则其有效接口集是其一组*interface_type*约束和其*type_parameter*约束的有效接口集的并集。</span><span class="sxs-lookup"><span data-stu-id="b24c2-292">If `T` has both *interface_type* constraints and *type_parameter* constraints, its effective interface set is the union of its set of *interface_type* constraints and the effective interface sets of its *type_parameter* constraints.</span></span>

<span data-ttu-id="b24c2-293">如果类型参数具有引用类型约束，或者其有效的基类不是 `object` 或 `System.ValueType`，则已知该类型参数是***引用类型***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-293">A type parameter is ***known to be a reference type*** if it has the reference type constraint or its effective base class is not `object` or `System.ValueType`.</span></span>

<span data-ttu-id="b24c2-294">受约束的类型参数类型的值可用于访问约束隐含的实例成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-294">Values of a constrained type parameter type can be used to access the instance members implied by the constraints.</span></span> <span data-ttu-id="b24c2-295">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-295">In the example</span></span>
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
<span data-ttu-id="b24c2-296">可以在 `x` 上直接调用 `IPrintable` 方法，因为 `T` 被限制为始终实现 `IPrintable`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-296">the methods of `IPrintable` can be invoked directly on `x` because `T` is constrained to always implement `IPrintable`.</span></span>

### <a name="class-body"></a><span data-ttu-id="b24c2-297">类体</span><span class="sxs-lookup"><span data-stu-id="b24c2-297">Class body</span></span>

<span data-ttu-id="b24c2-298">类的*class_body*定义该类的成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-298">The *class_body* of a class defines the members of that class.</span></span>

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a><span data-ttu-id="b24c2-299">分部类型</span><span class="sxs-lookup"><span data-stu-id="b24c2-299">Partial types</span></span>

<span data-ttu-id="b24c2-300">类型声明可跨多个***分部类型声明***拆分。</span><span class="sxs-lookup"><span data-stu-id="b24c2-300">A type declaration can be split across multiple ***partial type declarations***.</span></span> <span data-ttu-id="b24c2-301">类型声明是根据此部分中的规则从其部分构造的，因此在程序的编译时和运行时处理的其余部分，将它视为单个声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-301">The type declaration is constructed from its parts by following the rules in this section, whereupon it is treated as a single declaration during the remainder of the compile-time and run-time processing of the program.</span></span>

<span data-ttu-id="b24c2-302">如果*class_declaration*、 *struct_declaration*或*interface_declaration*表示包含 `partial` 修饰符的分部类型声明，则为。</span><span class="sxs-lookup"><span data-stu-id="b24c2-302">A *class_declaration*, *struct_declaration* or *interface_declaration* represents a partial type declaration if it includes a `partial` modifier.</span></span> <span data-ttu-id="b24c2-303">`partial` 不是关键字，并且仅在以下情况下充当修饰符： `class`、`struct` 或 `interface` 在类型声明中，或者在方法声明中的类型 `void` 之前出现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-303">`partial` is not a keyword, and only acts as a modifier if it appears immediately before one of the keywords `class`, `struct` or `interface` in a type declaration, or before the type `void` in a method declaration.</span></span> <span data-ttu-id="b24c2-304">在其他上下文中，可以将其用作常规标识符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-304">In other contexts it can be used as a normal identifier.</span></span>

<span data-ttu-id="b24c2-305">分部类型声明的每个部分都必须包含一个 `partial` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-305">Each part of a partial type declaration must include a `partial` modifier.</span></span> <span data-ttu-id="b24c2-306">它必须具有相同的名称，并在与其他部分相同的命名空间或类型声明中声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-306">It must have the same name  and be declared in the same namespace or type declaration as the other parts.</span></span> <span data-ttu-id="b24c2-307">`partial` 修饰符指示类型声明的其他部分可能存在于其他位置，但不要求存在此类附加部分;它对于具有单个声明的类型是有效的，以包含 `partial` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-307">The `partial` modifier indicates that additional parts of the type declaration may exist elsewhere, but the existence of such additional parts is not a requirement; it is valid for a type with a single declaration to include the `partial` modifier.</span></span>

<span data-ttu-id="b24c2-308">分部类型的所有部分都必须一起编译，以便可以在编译时将这些部分合并为单个类型声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-308">All parts of a partial type must be compiled together such that the parts can be merged at compile-time into a single type declaration.</span></span> <span data-ttu-id="b24c2-309">具体而言，分部类型不允许扩展已编译的类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-309">Partial types specifically do not allow already compiled types to be extended.</span></span>

<span data-ttu-id="b24c2-310">嵌套类型可以在多个部分中通过使用 `partial` 修饰符来声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-310">Nested types may be declared in multiple parts by using the `partial` modifier.</span></span> <span data-ttu-id="b24c2-311">通常，包含类型是使用 `partial` 进行声明的，并且嵌套类型的每个部分都是在包含类型的不同部分中声明的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-311">Typically, the containing type is declared using `partial` as well, and each part of the nested type is declared in a different part of the containing type.</span></span>

<span data-ttu-id="b24c2-312">委托或枚举声明中不允许使用 `partial` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-312">The `partial` modifier is not permitted on delegate or enum declarations.</span></span>

### <a name="attributes"></a><span data-ttu-id="b24c2-313">Attributes</span><span class="sxs-lookup"><span data-stu-id="b24c2-313">Attributes</span></span>

<span data-ttu-id="b24c2-314">分部类型的属性是通过将每个部件的属性按未指定顺序组合来确定的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-314">The attributes of a partial type are determined by combining, in an unspecified order, the attributes of each of the parts.</span></span> <span data-ttu-id="b24c2-315">如果特性放置在多个部件上，则等效于在类型上多次指定属性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-315">If an attribute is placed on multiple parts, it is equivalent to specifying the attribute multiple times on the type.</span></span> <span data-ttu-id="b24c2-316">例如，这两个部分：</span><span class="sxs-lookup"><span data-stu-id="b24c2-316">For example, the two parts:</span></span>

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
<span data-ttu-id="b24c2-317">等效于如下所示的声明：</span><span class="sxs-lookup"><span data-stu-id="b24c2-317">are equivalent to a declaration such as:</span></span>
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

<span data-ttu-id="b24c2-318">类型参数上的属性以类似的方式进行组合。</span><span class="sxs-lookup"><span data-stu-id="b24c2-318">Attributes on type parameters combine in a similar fashion.</span></span>

### <a name="modifiers"></a><span data-ttu-id="b24c2-319">修饰符</span><span class="sxs-lookup"><span data-stu-id="b24c2-319">Modifiers</span></span>

<span data-ttu-id="b24c2-320">当分部类型声明包括辅助功能规范（`public`、`protected`、`internal`和 `private` 修饰符）时，它必须与包括辅助功能规范的所有其他部分协商。</span><span class="sxs-lookup"><span data-stu-id="b24c2-320">When a partial type declaration includes an accessibility specification (the `public`, `protected`, `internal`, and `private` modifiers) it must agree with all other parts that include an accessibility specification.</span></span> <span data-ttu-id="b24c2-321">如果分部类型的任何部分都不包括辅助功能规范，则会为该类型提供适当的默认可访问性（已[声明的可访问](basic-concepts.md#declared-accessibility)性）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-321">If no part of a partial type includes an accessibility specification, the type is given the appropriate default accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="b24c2-322">如果嵌套类型的一个或多个分部声明包含 `new` 修饰符，则在嵌套类型隐藏继承成员（[通过继承隐藏](basic-concepts.md#hiding-through-inheritance)）时不会报告警告。</span><span class="sxs-lookup"><span data-stu-id="b24c2-322">If one or more partial declarations of a nested type include a `new` modifier, no warning is reported if the nested type hides an inherited member ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

<span data-ttu-id="b24c2-323">如果某个类的一个或多个分部声明包含 `abstract` 修饰符，则将此类视为抽象类（[抽象类](classes.md#abstract-classes)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-323">If one or more partial declarations of a class include an `abstract` modifier, the class is considered abstract ([Abstract classes](classes.md#abstract-classes)).</span></span> <span data-ttu-id="b24c2-324">否则，类被视为非抽象类。</span><span class="sxs-lookup"><span data-stu-id="b24c2-324">Otherwise, the class is considered non-abstract.</span></span>

<span data-ttu-id="b24c2-325">如果某个类的一个或多个分部声明包含 `sealed` 修饰符，则将此类视为 sealed （[密封类](classes.md#sealed-classes)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-325">If one or more partial declarations of a class include a `sealed` modifier, the class is considered sealed ([Sealed classes](classes.md#sealed-classes)).</span></span> <span data-ttu-id="b24c2-326">否则，类被视为未密封。</span><span class="sxs-lookup"><span data-stu-id="b24c2-326">Otherwise, the class is considered unsealed.</span></span>

<span data-ttu-id="b24c2-327">请注意，类不能既是抽象的又是密封的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-327">Note that a class cannot be both abstract and sealed.</span></span>

<span data-ttu-id="b24c2-328">当对分部类型声明使用 `unsafe` 修饰符时，只有该特定部分被视为不安全的上下文（[不安全](unsafe-code.md#unsafe-contexts)上下文）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-328">When the `unsafe` modifier is used on a partial type declaration, only that particular part is considered an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

### <a name="type-parameters-and-constraints"></a><span data-ttu-id="b24c2-329">类型参数和约束</span><span class="sxs-lookup"><span data-stu-id="b24c2-329">Type parameters and constraints</span></span>

<span data-ttu-id="b24c2-330">如果泛型类型在多个部分中声明，则每个部分都必须声明类型参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-330">If a generic type is declared in multiple parts, each part must state the type parameters.</span></span> <span data-ttu-id="b24c2-331">每个部分必须具有相同数量的类型参数，并且每个类型参数的名称必须相同。</span><span class="sxs-lookup"><span data-stu-id="b24c2-331">Each part must have the same number of type parameters, and the same name for each type parameter, in order.</span></span>

<span data-ttu-id="b24c2-332">当部分泛型类型声明包含约束（`where` 子句）时，约束必须与包含约束的所有其他部分一致。</span><span class="sxs-lookup"><span data-stu-id="b24c2-332">When a partial generic type declaration includes constraints (`where` clauses), the constraints must agree with all other parts that include constraints.</span></span> <span data-ttu-id="b24c2-333">具体而言，包括约束的每个部分都必须具有对同一组类型参数的约束，并且对于每个类型参数，主要、次要和构造函数约束的集必须是等效的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-333">Specifically, each part that includes constraints must have constraints for the same set of type parameters, and for each type parameter the sets of primary, secondary, and constructor constraints must be equivalent.</span></span> <span data-ttu-id="b24c2-334">如果两组约束包含相同的成员，则它们是等效的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-334">Two sets of constraints are equivalent if they contain the same members.</span></span> <span data-ttu-id="b24c2-335">如果部分泛型类型的任何部分都不指定类型参数约束，则类型参数被视为不受约束。</span><span class="sxs-lookup"><span data-stu-id="b24c2-335">If no part of a partial generic type specifies type parameter constraints, the type parameters are considered unconstrained.</span></span>

<span data-ttu-id="b24c2-336">示例</span><span class="sxs-lookup"><span data-stu-id="b24c2-336">The example</span></span>
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
<span data-ttu-id="b24c2-337">是正确的，因为包含约束的部分（前两个）为同一组类型参数有效地指定了相同的一组主、辅助和构造函数约束。</span><span class="sxs-lookup"><span data-stu-id="b24c2-337">is correct because those parts that include constraints (the first two) effectively specify the same set of primary, secondary, and constructor constraints for the same set of type parameters, respectively.</span></span>

### <a name="base-class"></a><span data-ttu-id="b24c2-338">基类</span><span class="sxs-lookup"><span data-stu-id="b24c2-338">Base class</span></span>

<span data-ttu-id="b24c2-339">当分部类声明包含基类规范时，它必须与包含基类规范的所有其他部分协商。</span><span class="sxs-lookup"><span data-stu-id="b24c2-339">When a partial class declaration includes a base class specification it must agree with all other parts that include a base class specification.</span></span> <span data-ttu-id="b24c2-340">如果分部类的任何部分都不包含基类规范，则基类成为 `System.Object` （[基类](classes.md#base-classes)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-340">If no part of a partial class includes a base class specification, the base class becomes `System.Object` ([Base classes](classes.md#base-classes)).</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="b24c2-341">基接口</span><span class="sxs-lookup"><span data-stu-id="b24c2-341">Base interfaces</span></span>

<span data-ttu-id="b24c2-342">在多个部分中声明的类型的基接口集是在每个部件上指定的基本接口的联合。</span><span class="sxs-lookup"><span data-stu-id="b24c2-342">The set of base interfaces for a type declared in multiple parts is the union of the base interfaces specified on each part.</span></span> <span data-ttu-id="b24c2-343">特定基接口的每个部分只能命名一次，但允许多个部件命名相同的基接口。</span><span class="sxs-lookup"><span data-stu-id="b24c2-343">A particular base interface may only be named once on each part, but it is permitted for multiple parts to name the same base interface(s).</span></span> <span data-ttu-id="b24c2-344">任何给定基接口的成员只能有一个实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-344">There must only be one implementation of the members of any given base interface.</span></span>

<span data-ttu-id="b24c2-345">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-345">In the example</span></span>
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
<span data-ttu-id="b24c2-346">类 `C` 的基接口集是 `IA`、`IB`和 `IC`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-346">the set of base interfaces for class `C` is `IA`, `IB`, and `IC`.</span></span>

<span data-ttu-id="b24c2-347">通常，每个部分都提供对该部件声明的接口的实现;但这并不是必需的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-347">Typically, each part provides an implementation of the interface(s) declared on that part; however, this is not a requirement.</span></span> <span data-ttu-id="b24c2-348">部件可以为在不同的部分声明的接口提供实现：</span><span class="sxs-lookup"><span data-stu-id="b24c2-348">A part may provide the implementation for an interface declared on a different part:</span></span>
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

### <a name="members"></a><span data-ttu-id="b24c2-349">Members</span><span class="sxs-lookup"><span data-stu-id="b24c2-349">Members</span></span>

<span data-ttu-id="b24c2-350">除了分部方法（[分部方法](classes.md#partial-methods)）以外，在多个部分中声明的类型的成员集只是每个部分中声明的成员集的并集。</span><span class="sxs-lookup"><span data-stu-id="b24c2-350">With the exception of partial methods ([Partial methods](classes.md#partial-methods)), the set of members of a type declared in multiple parts is simply the union of the set of members declared in each part.</span></span> <span data-ttu-id="b24c2-351">类型声明的所有部分的主体共享同一声明空间（[声明](basic-concepts.md#declarations)），并且每个成员（[范围](basic-concepts.md#scopes)）的作用域都扩展到所有部分的主体。</span><span class="sxs-lookup"><span data-stu-id="b24c2-351">The bodies of all parts of the type declaration share the same declaration space ([Declarations](basic-concepts.md#declarations)), and the scope of each member ([Scopes](basic-concepts.md#scopes)) extends to the bodies of all the parts.</span></span> <span data-ttu-id="b24c2-352">任何成员的可访问域始终包括封闭类型的所有部分;在一个部分中声明的 `private` 成员可从另一个部件自由地访问。</span><span class="sxs-lookup"><span data-stu-id="b24c2-352">The accessibility domain of any member always includes all the parts of the enclosing type; a `private` member declared in one part is freely accessible from another part.</span></span> <span data-ttu-id="b24c2-353">在该类型的多个部分中声明同一成员是编译时错误，除非该成员是带有 `partial` 修饰符的类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-353">It is a compile-time error to declare the same member in more than one part of the type, unless that member is a type with the `partial` modifier.</span></span>

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

<span data-ttu-id="b24c2-354">类型中成员的顺序对C#代码的排序很少，但在与其他语言和环境交互时可能很重要。</span><span class="sxs-lookup"><span data-stu-id="b24c2-354">The ordering of members within a type is rarely significant to C# code, but may be significant when interfacing with other languages and environments.</span></span> <span data-ttu-id="b24c2-355">在这些情况下，在多个部分中声明的类型中的成员顺序是不确定的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-355">In these cases, the ordering of members within a type declared in multiple parts is undefined.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="b24c2-356">分部方法</span><span class="sxs-lookup"><span data-stu-id="b24c2-356">Partial methods</span></span>

<span data-ttu-id="b24c2-357">分部方法可在类型声明的一部分中定义，并在另一个中实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-357">Partial methods can be defined in one part of a type declaration and implemented in another.</span></span> <span data-ttu-id="b24c2-358">实现是可选的;如果没有部分实现分部方法，则将从部分组合生成的类型声明中删除分部方法声明和对它的所有调用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-358">The implementation is optional; if no part implements the partial method, the partial method declaration and all calls to it are removed from the type declaration resulting from the combination of the parts.</span></span>

<span data-ttu-id="b24c2-359">分部方法不能定义访问修饰符，而是隐式 `private`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-359">Partial methods cannot define access modifiers, but are implicitly `private`.</span></span> <span data-ttu-id="b24c2-360">它们的返回类型必须是 `void`的，并且其参数不能具有 `out` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-360">Their return type must be `void`, and their parameters cannot have the `out` modifier.</span></span> <span data-ttu-id="b24c2-361">仅当标识符 `partial` 在 `void` 类型之前出现时，才会被识别为方法声明中的特殊关键字;否则，可将其用作常规标识符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-361">The identifier `partial` is recognized as a special keyword in a method declaration only if it appears right before the `void` type; otherwise it can be used as a normal identifier.</span></span> <span data-ttu-id="b24c2-362">分部方法不能显式实现接口方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-362">A partial method cannot explicitly implement interface methods.</span></span>

<span data-ttu-id="b24c2-363">存在两种类型的分部方法声明：如果方法声明的主体为分号，则声明声明为***定义分部方法声明***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-363">There are two kinds of partial method declarations: If the body of the method declaration is a semicolon, the declaration is said to be a ***defining partial method declaration***.</span></span> <span data-ttu-id="b24c2-364">如果将正文作为*块*提供，则声明声明为***实现分部方法声明***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-364">If the body is given as a *block*, the declaration is said to be an ***implementing partial method declaration***.</span></span> <span data-ttu-id="b24c2-365">在类型声明的各个部分中，只能有一个用给定的签名定义分部方法声明，只能有一个实现具有给定签名的分部方法声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-365">Across the parts of a type declaration there can be only one defining partial method declaration with a given signature, and there can be only one implementing partial method declaration with a given signature.</span></span> <span data-ttu-id="b24c2-366">如果给定了实现分部方法声明，则必须存在相应的定义分部方法声明，并且声明必须与下面指定的声明匹配：</span><span class="sxs-lookup"><span data-stu-id="b24c2-366">If an implementing partial method declaration is given, a corresponding defining partial method declaration must exist, and the declarations must match as specified in the following:</span></span>

* <span data-ttu-id="b24c2-367">声明必须具有相同的修饰符（尽管不必按相同顺序）、方法名称、类型参数的数目和参数的数目。</span><span class="sxs-lookup"><span data-stu-id="b24c2-367">The declarations must have the same modifiers (although not necessarily in the same order), method name, number of type parameters and number of parameters.</span></span>
* <span data-ttu-id="b24c2-368">声明中的相应参数必须具有相同的修饰符（尽管不必按相同顺序）和相同的类型（在类型参数名称中取模差异）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-368">Corresponding parameters in the declarations must have the same modifiers (although not necessarily in the same order) and the same types (modulo differences in type parameter names).</span></span>
* <span data-ttu-id="b24c2-369">声明中的相应类型参数必须具有相同的约束（在类型参数名称中取模不同）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-369">Corresponding type parameters in the declarations must have the same constraints (modulo differences in type parameter names).</span></span>

<span data-ttu-id="b24c2-370">实现分部方法声明可以与相应的定义分部方法声明出现在同一部分中。</span><span class="sxs-lookup"><span data-stu-id="b24c2-370">An implementing partial method declaration can appear in the same part as the corresponding defining partial method declaration.</span></span>

<span data-ttu-id="b24c2-371">只有定义分部方法参与重载决策。</span><span class="sxs-lookup"><span data-stu-id="b24c2-371">Only a defining partial method participates in overload resolution.</span></span> <span data-ttu-id="b24c2-372">因此，无论是否给定了实现声明，调用表达式都可以解析为分部方法的调用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-372">Thus, whether or not an implementing declaration is given, invocation expressions may resolve to invocations of the partial method.</span></span> <span data-ttu-id="b24c2-373">由于分部方法始终返回 `void`，因此此类调用表达式将始终为 expression 语句。</span><span class="sxs-lookup"><span data-stu-id="b24c2-373">Because a partial method always returns `void`, such invocation expressions will always be expression statements.</span></span> <span data-ttu-id="b24c2-374">此外，由于分部方法是隐式 `private`的，因此，此类语句将始终出现在声明分部方法的类型声明的某个部分中。</span><span class="sxs-lookup"><span data-stu-id="b24c2-374">Furthermore, because a partial method is implicitly `private`, such statements will always occur within one of the parts of the type declaration within which the partial method is declared.</span></span>

<span data-ttu-id="b24c2-375">如果分部类型声明的任何部分都不包含给定分部方法的实现声明，则从组合类型声明中只会删除调用它的任何表达式语句。</span><span class="sxs-lookup"><span data-stu-id="b24c2-375">If no part of a partial type declaration contains an implementing declaration for a given partial method, any expression statement invoking it is simply removed from the combined type declaration.</span></span> <span data-ttu-id="b24c2-376">因此，调用表达式（包括任何构成表达式）在运行时不起作用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-376">Thus the invocation expression, including any constituent expressions, has no effect at run-time.</span></span> <span data-ttu-id="b24c2-377">分部方法本身也会被移除，并且不是组合类型声明的成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-377">The partial method itself is also removed and will not be a member of the combined type declaration.</span></span>

<span data-ttu-id="b24c2-378">如果给定分部方法存在实现声明，则保留分部方法的调用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-378">If an implementing declaration exist for a given partial method, the invocations of the partial methods are retained.</span></span> <span data-ttu-id="b24c2-379">分部方法提供与实现分部方法声明类似的方法声明，但以下情况除外：</span><span class="sxs-lookup"><span data-stu-id="b24c2-379">The partial method gives rise to a method declaration similar to the implementing partial method declaration except for the following:</span></span>

* <span data-ttu-id="b24c2-380">不包含 `partial` 修饰符</span><span class="sxs-lookup"><span data-stu-id="b24c2-380">The `partial` modifier is not included</span></span>
* <span data-ttu-id="b24c2-381">生成的方法声明中的特性是定义的组合特性和实现分部方法声明的未指定顺序。</span><span class="sxs-lookup"><span data-stu-id="b24c2-381">The attributes in the resulting method declaration are the combined attributes of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="b24c2-382">不会删除重复项。</span><span class="sxs-lookup"><span data-stu-id="b24c2-382">Duplicates are not removed.</span></span>
* <span data-ttu-id="b24c2-383">生成的方法声明的参数上的属性为定义的相应参数的组合特性，并按未指定的顺序进行实现分部方法声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-383">The attributes on the parameters of the resulting method declaration are the combined attributes of the corresponding parameters of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="b24c2-384">不会删除重复项。</span><span class="sxs-lookup"><span data-stu-id="b24c2-384">Duplicates are not removed.</span></span>

<span data-ttu-id="b24c2-385">如果为分部方法 M 提供定义声明，而不是实现声明，则以下限制适用：</span><span class="sxs-lookup"><span data-stu-id="b24c2-385">If a defining declaration but not an implementing declaration is given for a partial method M, the following restrictions apply:</span></span>

* <span data-ttu-id="b24c2-386">创建委托到方法（[委托创建表达式](expressions.md#delegate-creation-expressions)）时出现编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-386">It is a compile-time error to create a delegate to method ([Delegate creation expressions](expressions.md#delegate-creation-expressions)).</span></span>
* <span data-ttu-id="b24c2-387">在转换为表达式树类型的匿名函数内引用 `M` 是编译时错误（[对表达式树类型的匿名函数转换的计算](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-387">It is a compile-time error to refer to `M` inside an anonymous function that is converted to an expression tree type ([Evaluation of anonymous function conversions to expression tree types](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span></span>
* <span data-ttu-id="b24c2-388">作为 `M` 调用的一部分出现的表达式不影响明确的赋值状态（[明确赋值](variables.md#definite-assignment)），这可能会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-388">Expressions occurring as part of an invocation of `M` do not affect the definite assignment state ([Definite assignment](variables.md#definite-assignment)), which can potentially lead to compile-time errors.</span></span>
* <span data-ttu-id="b24c2-389">`M` 不能是应用程序的入口点（[应用程序启动](basic-concepts.md#application-startup)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-389">`M` cannot be the entry point for an application ([Application Startup](basic-concepts.md#application-startup)).</span></span>

<span data-ttu-id="b24c2-390">分部方法有助于允许类型声明的一部分自定义另一个部件的行为，例如，由工具生成的部分。</span><span class="sxs-lookup"><span data-stu-id="b24c2-390">Partial methods are useful for allowing one part of a type declaration to customize the behavior of another part, e.g., one that is generated by a tool.</span></span> <span data-ttu-id="b24c2-391">请考虑以下分部类声明：</span><span class="sxs-lookup"><span data-stu-id="b24c2-391">Consider the following partial class declaration:</span></span>
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

<span data-ttu-id="b24c2-392">如果此类是在没有任何其他部分的情况下编译的，则将删除定义分部方法声明及其调用，并且生成的组合类声明将等效于以下内容：</span><span class="sxs-lookup"><span data-stu-id="b24c2-392">If this class is compiled without any other parts, the defining partial method declarations and their invocations will be removed, and the resulting combined class declaration will be equivalent to the following:</span></span>
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

<span data-ttu-id="b24c2-393">假定另外提供了一个部分，后者提供了分部方法的实现声明：</span><span class="sxs-lookup"><span data-stu-id="b24c2-393">Assume that another part is given, however, which provides implementing declarations of the partial methods:</span></span>
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

<span data-ttu-id="b24c2-394">然后，生成的组合类声明将等效于以下内容：</span><span class="sxs-lookup"><span data-stu-id="b24c2-394">Then the resulting combined class declaration will be equivalent to the following:</span></span>
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

### <a name="name-binding"></a><span data-ttu-id="b24c2-395">名称绑定</span><span class="sxs-lookup"><span data-stu-id="b24c2-395">Name binding</span></span>

<span data-ttu-id="b24c2-396">尽管可扩展类型的每个部分都必须在同一个命名空间内声明，但这些部分通常是在不同的命名空间声明中编写的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-396">Although each part of an extensible type must be declared within the same namespace, the parts are typically written within different namespace declarations.</span></span> <span data-ttu-id="b24c2-397">因此，每个部件可能都有不同的 `using` 指令（[使用指令](namespaces.md#using-directives)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-397">Thus, different `using` directives ([Using directives](namespaces.md#using-directives)) may be present for each part.</span></span> <span data-ttu-id="b24c2-398">解释一个部分中的简单名称（[类型推理](expressions.md#type-inference)）时，仅考虑包含该部分的命名空间声明的 `using` 指令。</span><span class="sxs-lookup"><span data-stu-id="b24c2-398">When interpreting simple names ([Type inference](expressions.md#type-inference)) within one part, only the `using` directives of the namespace declaration(s) enclosing that part are considered.</span></span> <span data-ttu-id="b24c2-399">这可能导致同一标识符在不同的部分中具有不同的含义：</span><span class="sxs-lookup"><span data-stu-id="b24c2-399">This may result in the same identifier having different meanings in different parts:</span></span>
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

## <a name="class-members"></a><span data-ttu-id="b24c2-400">类成员</span><span class="sxs-lookup"><span data-stu-id="b24c2-400">Class members</span></span>

<span data-ttu-id="b24c2-401">类的成员由其*class_member_declaration*引入的成员和从直接基类继承的成员组成。</span><span class="sxs-lookup"><span data-stu-id="b24c2-401">The members of a class consist of the members introduced by its *class_member_declaration*s and the members inherited from the direct base class.</span></span>

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

<span data-ttu-id="b24c2-402">类类型的成员分为以下几个类别：</span><span class="sxs-lookup"><span data-stu-id="b24c2-402">The members of a class type are divided into the following categories:</span></span>

*  <span data-ttu-id="b24c2-403">常量，表示与类（[常量](classes.md#constants)）关联的常量值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-403">Constants, which represent constant values associated with the class ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="b24c2-404">字段，它们是类（[字段](classes.md#fields)）的变量。</span><span class="sxs-lookup"><span data-stu-id="b24c2-404">Fields, which are the variables of the class ([Fields](classes.md#fields)).</span></span>
*  <span data-ttu-id="b24c2-405">方法，可实现类（[方法](classes.md#methods)）可以执行的计算和操作。</span><span class="sxs-lookup"><span data-stu-id="b24c2-405">Methods, which implement the computations and actions that can be performed by the class ([Methods](classes.md#methods)).</span></span>
*  <span data-ttu-id="b24c2-406">属性，定义命名特性以及与读取和写入这些特性相关联的操作。[属性](classes.md#properties)</span><span class="sxs-lookup"><span data-stu-id="b24c2-406">Properties, which define named characteristics and the actions associated with reading and writing those characteristics ([Properties](classes.md#properties)).</span></span>
*  <span data-ttu-id="b24c2-407">事件，定义可由类生成的通知（[事件](classes.md#events)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-407">Events, which define notifications that can be generated by the class ([Events](classes.md#events)).</span></span>
*  <span data-ttu-id="b24c2-408">索引器，允许以相同方式（语法为）将类的实例索引为数组（[索引器](classes.md#indexers)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-408">Indexers, which permit instances of the class to be indexed in the same way (syntactically) as arrays ([Indexers](classes.md#indexers)).</span></span>
*  <span data-ttu-id="b24c2-409">运算符，用于定义可应用于类的实例（[运算符](classes.md#operators)）的表达式运算符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-409">Operators, which define the expression operators that can be applied to instances of the class ([Operators](classes.md#operators)).</span></span>
*  <span data-ttu-id="b24c2-410">实例构造函数，用于实现初始化类的实例所需的操作（[实例构造函数](classes.md#instance-constructors)）</span><span class="sxs-lookup"><span data-stu-id="b24c2-410">Instance constructors, which implement the actions required to initialize instances of the class ([Instance constructors](classes.md#instance-constructors))</span></span>
*  <span data-ttu-id="b24c2-411">析构函数，用于实现在永久丢弃类的实例之前要执行的操作（[析构函数](classes.md#destructors)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-411">Destructors, which implement the actions to be performed before instances of the class are permanently discarded ([Destructors](classes.md#destructors)).</span></span>
*  <span data-ttu-id="b24c2-412">静态构造函数，用于实现初始化类本身所需的操作（[静态构造函数](classes.md#static-constructors)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-412">Static constructors, which implement the actions required to initialize the class itself ([Static constructors](classes.md#static-constructors)).</span></span>
*  <span data-ttu-id="b24c2-413">类型，表示类的本地类型（[嵌套类型](classes.md#nested-types)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-413">Types, which represent the types that are local to the class ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="b24c2-414">可以包含可执行代码的成员统称为类类型的*函数成员*。</span><span class="sxs-lookup"><span data-stu-id="b24c2-414">Members that can contain executable code are collectively known as the *function members* of the class type.</span></span> <span data-ttu-id="b24c2-415">类类型的函数成员是类类型的方法、属性、事件、索引器、运算符、实例构造函数、析构函数和静态构造函数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-415">The function members of a class type are the methods, properties, events, indexers, operators, instance constructors,  destructors, and static constructors of that class type.</span></span>

<span data-ttu-id="b24c2-416">*Class_declaration*创建新的声明空间（[声明](basic-concepts.md#declarations)），并且*class_declaration*立即包含的*class_member_declaration*会将新成员引入此声明空间。</span><span class="sxs-lookup"><span data-stu-id="b24c2-416">A *class_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *class_member_declaration*s immediately contained by the *class_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="b24c2-417">以下规则适用于*class_member_declaration*：</span><span class="sxs-lookup"><span data-stu-id="b24c2-417">The following rules apply to *class_member_declaration*s:</span></span>

*  <span data-ttu-id="b24c2-418">实例构造函数、析构函数和静态构造函数必须具有与直接封闭类相同的名称。</span><span class="sxs-lookup"><span data-stu-id="b24c2-418">Instance constructors, destructors and static constructors must have the same name as the immediately enclosing class.</span></span> <span data-ttu-id="b24c2-419">所有其他成员的名称必须不同于立即封闭的类的名称。</span><span class="sxs-lookup"><span data-stu-id="b24c2-419">All other members must have names that differ from the name of the immediately enclosing class.</span></span>
*  <span data-ttu-id="b24c2-420">常数、字段、属性、事件或类型的名称必须不同于同一个类中声明的所有其他成员的名称。</span><span class="sxs-lookup"><span data-stu-id="b24c2-420">The name of a constant, field, property, event, or type must differ from the names of all other members declared in the same class.</span></span>
*  <span data-ttu-id="b24c2-421">方法的名称必须不同于同一个类中声明的所有其他非方法的名称。</span><span class="sxs-lookup"><span data-stu-id="b24c2-421">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="b24c2-422">此外，方法的签名（[签名和重载](basic-concepts.md#signatures-and-overloading)）必须不同于同一个类中声明的所有其他方法的签名，同一类中声明的两个方法的签名可能不只是 `ref` 和 `out`不同。</span><span class="sxs-lookup"><span data-stu-id="b24c2-422">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="b24c2-423">实例构造函数的签名必须不同于同一个类中声明的所有其他实例构造函数的签名，同一类中声明的两个构造函数的签名可能不只是 `ref` 和 `out`不同。</span><span class="sxs-lookup"><span data-stu-id="b24c2-423">The signature of an instance constructor must differ from the signatures of all other instance constructors declared in the same class, and two constructors declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="b24c2-424">索引器的签名必须与同一类中声明的所有其他索引器的签名不同。</span><span class="sxs-lookup"><span data-stu-id="b24c2-424">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>
*  <span data-ttu-id="b24c2-425">运算符的签名必须与同一类中声明的所有其他运算符的签名不同。</span><span class="sxs-lookup"><span data-stu-id="b24c2-425">The signature of an operator must differ from the signatures of all other operators declared in the same class.</span></span>

<span data-ttu-id="b24c2-426">类类型的继承成员（[继承](classes.md#inheritance)）不属于类的声明空间。</span><span class="sxs-lookup"><span data-stu-id="b24c2-426">The inherited members of a class type ([Inheritance](classes.md#inheritance)) are not part of the declaration space of a class.</span></span> <span data-ttu-id="b24c2-427">因此，允许派生类声明与继承成员具有相同名称或签名的成员（实际上隐藏了继承成员）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-427">Thus, a derived class is allowed to declare a member with the same name or signature as an inherited member (which in effect hides the inherited member).</span></span>

### <a name="the-instance-type"></a><span data-ttu-id="b24c2-428">实例类型</span><span class="sxs-lookup"><span data-stu-id="b24c2-428">The instance type</span></span>

<span data-ttu-id="b24c2-429">每个类声明都具有关联的绑定类型（[绑定类型和未绑定类型](types.md#bound-and-unbound-types)）和***实例类型***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-429">Each class declaration has an associated bound type ([Bound and unbound types](types.md#bound-and-unbound-types)), the ***instance type***.</span></span> <span data-ttu-id="b24c2-430">对于泛型类声明，实例类型通过从类型声明创建构造类型（[构造类型](types.md#constructed-types)）来形成，其中每个提供的类型参数都是对应的类型参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-430">For a generic class declaration, the instance type is formed by creating a constructed type ([Constructed types](types.md#constructed-types)) from the type declaration, with each of the supplied type arguments being the corresponding type parameter.</span></span> <span data-ttu-id="b24c2-431">由于实例类型使用类型参数，因此只能在类型参数位于范围内时使用。也就是说，在类声明中。</span><span class="sxs-lookup"><span data-stu-id="b24c2-431">Since the instance type uses the type parameters, it can only be used where the type parameters are in scope; that is, inside the class declaration.</span></span> <span data-ttu-id="b24c2-432">实例类型是在类声明中编写的代码的 `this` 的类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-432">The instance type is the type of `this` for code written inside the class declaration.</span></span> <span data-ttu-id="b24c2-433">对于非泛型类，实例类型只是声明的类。</span><span class="sxs-lookup"><span data-stu-id="b24c2-433">For non-generic classes, the instance type is simply the declared class.</span></span> <span data-ttu-id="b24c2-434">下面显示了多个类声明及其实例类型：</span><span class="sxs-lookup"><span data-stu-id="b24c2-434">The following shows several class declarations along with their instance types:</span></span> 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a><span data-ttu-id="b24c2-435">构造类型的成员</span><span class="sxs-lookup"><span data-stu-id="b24c2-435">Members of constructed types</span></span>

<span data-ttu-id="b24c2-436">构造类型的非继承成员是通过将成员声明中的每个*type_parameter*替换为构造类型的对应*type_argument*来获取的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-436">The non-inherited members of a constructed type are obtained by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="b24c2-437">替换过程基于类型声明的语义含义，而不只是文本替换。</span><span class="sxs-lookup"><span data-stu-id="b24c2-437">The substitution process is based on the semantic meaning of type declarations, and is not simply textual substitution.</span></span>

<span data-ttu-id="b24c2-438">例如，给定泛型类声明</span><span class="sxs-lookup"><span data-stu-id="b24c2-438">For example, given the generic class declaration</span></span>
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
<span data-ttu-id="b24c2-439">构造类型 `Gen<int[],IComparable<string>>` 具有以下成员：</span><span class="sxs-lookup"><span data-stu-id="b24c2-439">the constructed type `Gen<int[],IComparable<string>>` has the following members:</span></span>
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

<span data-ttu-id="b24c2-440">泛型类声明中的成员 `a` 的类型 `Gen` 为 "二维数组" `T`，因此以上构造类型中的成员 `a` 类型为 "一维数组的二维数组，即 `int`" 或 "`int[,][]`"。</span><span class="sxs-lookup"><span data-stu-id="b24c2-440">The type of the member `a` in the generic class declaration `Gen` is "two-dimensional array of `T`", so the type of the member `a` in the constructed type above is "two-dimensional array of one-dimensional array of `int`", or `int[,][]`.</span></span>

<span data-ttu-id="b24c2-441">在实例函数成员中，`this` 的类型是包含声明的实例类型（[实例类型](classes.md#the-instance-type)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-441">Within instance function members, the type of `this` is the instance type ([The instance type](classes.md#the-instance-type)) of the containing declaration.</span></span>

<span data-ttu-id="b24c2-442">泛型类的所有成员都可以直接或作为构造类型的一部分，使用任何封闭类中的类型参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-442">All members of a generic class can use type parameters from any enclosing class, either directly or as part of a constructed type.</span></span> <span data-ttu-id="b24c2-443">当在运行时使用特定的封闭式构造类型（[开放和闭合类型](types.md#open-and-closed-types)）时，会将类型形参的每个使用替换为提供给构造类型的实际类型实参。</span><span class="sxs-lookup"><span data-stu-id="b24c2-443">When a particular closed constructed type ([Open and closed types](types.md#open-and-closed-types)) is used at run-time, each use of a type parameter is replaced with the actual type argument supplied to the constructed type.</span></span> <span data-ttu-id="b24c2-444">例如：</span><span class="sxs-lookup"><span data-stu-id="b24c2-444">For example:</span></span>
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

### <a name="inheritance"></a><span data-ttu-id="b24c2-445">继承</span><span class="sxs-lookup"><span data-stu-id="b24c2-445">Inheritance</span></span>

<span data-ttu-id="b24c2-446">类***继承***其直接基类类型的成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-446">A class ***inherits*** the members of its direct base class type.</span></span> <span data-ttu-id="b24c2-447">继承意味着类隐式包含其直接基类类型的所有成员，基类的实例构造函数、析构函数和静态构造函数除外。</span><span class="sxs-lookup"><span data-stu-id="b24c2-447">Inheritance means that a class implicitly contains all members of its direct base class type, except for the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="b24c2-448">继承的一些重要方面包括：</span><span class="sxs-lookup"><span data-stu-id="b24c2-448">Some important aspects of inheritance are:</span></span>

*  <span data-ttu-id="b24c2-449">继承是可传递的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-449">Inheritance is transitive.</span></span> <span data-ttu-id="b24c2-450">如果 `C` 派生自 `B`，并且 `B` 派生自 `A`，则 `C` 将继承在 `B` 中声明的成员以及在 `A`中声明的成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-450">If `C` is derived from `B`, and `B` is derived from `A`, then `C` inherits the members declared in `B` as well as the members declared in `A`.</span></span>
*  <span data-ttu-id="b24c2-451">派生类扩展其直接基类。</span><span class="sxs-lookup"><span data-stu-id="b24c2-451">A derived class extends its direct base class.</span></span> <span data-ttu-id="b24c2-452">派生类可以其继承的类添加新成员，但无法删除继承成员的定义。</span><span class="sxs-lookup"><span data-stu-id="b24c2-452">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span>
*  <span data-ttu-id="b24c2-453">实例构造函数、析构函数和静态构造函数不能继承，但所有其他成员均为，而不考虑它们的声明可访问性（[成员访问](basic-concepts.md#member-access)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-453">Instance constructors, destructors, and static constructors are not inherited, but all other members are, regardless of their declared accessibility ([Member access](basic-concepts.md#member-access)).</span></span> <span data-ttu-id="b24c2-454">但是，根据其声明的可访问性，继承成员可能无法在派生类中访问。</span><span class="sxs-lookup"><span data-stu-id="b24c2-454">However, depending on their declared accessibility, inherited members might not be accessible in a derived class.</span></span>
*  <span data-ttu-id="b24c2-455">派生类可以通过使用相同的名称或签名声明新成员，***隐藏***（[通过继承](basic-concepts.md#hiding-through-inheritance)隐藏）继承成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-455">A derived class can ***hide*** ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)) inherited members by declaring new members with the same name or signature.</span></span> <span data-ttu-id="b24c2-456">但请注意，隐藏继承成员不会删除该成员，只是使该成员直接通过派生类无法访问。</span><span class="sxs-lookup"><span data-stu-id="b24c2-456">Note however that hiding an inherited member does not remove that member—it merely makes that member inaccessible directly through the derived class.</span></span>
*  <span data-ttu-id="b24c2-457">类的实例包含在类及其基类中声明的所有实例字段的集合，以及从派生类类型到其任何基类类型的隐式转换（[隐式引用转换](conversions.md#implicit-reference-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-457">An instance of a class contains a set of all instance fields declared in the class and its base classes, and an implicit conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from a derived class type to any of its base class types.</span></span> <span data-ttu-id="b24c2-458">因此，可以将对某个派生类的实例的引用视为对其任何基类的实例的引用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-458">Thus, a reference to an instance of some derived class can be treated as a reference to an instance of any of its base classes.</span></span>
*  <span data-ttu-id="b24c2-459">类可以声明虚拟方法、属性和索引器，派生类可以重写这些函数成员的实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-459">A class can declare virtual methods, properties, and indexers, and derived classes can override the implementation of these function members.</span></span> <span data-ttu-id="b24c2-460">这使类能够显示多态行为，其中函数成员调用执行的操作取决于通过其调用该函数成员的实例的运行时类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-460">This enables classes to exhibit polymorphic behavior wherein the actions performed by a function member invocation varies depending on the run-time type of the instance through which that function member is invoked.</span></span>

<span data-ttu-id="b24c2-461">构造类类型的继承成员是直接基类类型（[基类](classes.md#base-classes)）的成员，可通过使用构造类型的类型参数替换*class_base*规范中每个对应类型参数的匹配项。</span><span class="sxs-lookup"><span data-stu-id="b24c2-461">The inherited member of a constructed class type are the members of the immediate base class type ([Base classes](classes.md#base-classes)), which is found by substituting the type arguments of the constructed type for each occurrence of the corresponding type parameters in the *class_base* specification.</span></span> <span data-ttu-id="b24c2-462">这些成员又通过替换成员声明中的每个*type_parameter*来转换*class_base*规范的相应*type_argument* 。</span><span class="sxs-lookup"><span data-stu-id="b24c2-462">These members, in turn, are transformed by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the *class_base* specification.</span></span>

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

<span data-ttu-id="b24c2-463">在上面的示例中，构造类型 `D<int>` 具有一个非继承成员 `public int G(string s)` 通过将类型参数 `int` 替换为类型参数 `T`来获得。</span><span class="sxs-lookup"><span data-stu-id="b24c2-463">In the above example, the constructed type `D<int>` has a non-inherited member `public int G(string s)` obtained by substituting the type argument `int` for the type parameter `T`.</span></span> <span data-ttu-id="b24c2-464">`D<int>` 还具有类声明 `B`的继承成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-464">`D<int>` also has an inherited member from the class declaration `B`.</span></span> <span data-ttu-id="b24c2-465">此继承成员是通过以下方式确定的：通过在基类规范 `B<T[]>`中替换 `T` `int` 来确定 `D<int>` 的基类类型 `B<int[]>`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-465">This inherited member is determined by first determining the base class type `B<int[]>` of `D<int>` by substituting `int` for `T` in the base class specification `B<T[]>`.</span></span> <span data-ttu-id="b24c2-466">然后，作为 `B`的类型参数，`int[]` 替换 `public U F(long index)`中的 `U`，从而生成继承成员 `public int[] F(long index)`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-466">Then, as a type argument to `B`, `int[]` is substituted for `U` in `public U F(long index)`, yielding the inherited member `public int[] F(long index)`.</span></span>

### <a name="the-new-modifier"></a><span data-ttu-id="b24c2-467">New 修饰符</span><span class="sxs-lookup"><span data-stu-id="b24c2-467">The new modifier</span></span>

<span data-ttu-id="b24c2-468">允许*class_member_declaration*声明与继承成员具有相同名称或签名的成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-468">A *class_member_declaration* is permitted to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="b24c2-469">出现这种情况时，会说派生类成员***隐藏***基类成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-469">When this occurs, the derived class member is said to ***hide*** the base class member.</span></span> <span data-ttu-id="b24c2-470">隐藏继承成员不被视为错误，但会导致编译器发出警告。</span><span class="sxs-lookup"><span data-stu-id="b24c2-470">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="b24c2-471">若要禁止显示该警告，派生类成员的声明可以包含一个 `new` 修饰符，以指示该派生成员用于隐藏基成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-471">To suppress the warning, the declaration of the derived class member can include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="b24c2-472">本主题将在[通过继承隐藏](basic-concepts.md#hiding-through-inheritance)中进一步进行讨论。</span><span class="sxs-lookup"><span data-stu-id="b24c2-472">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="b24c2-473">如果在不隐藏继承成员的声明中包括了 `new` 修饰符，则会发出对该结果的警告。</span><span class="sxs-lookup"><span data-stu-id="b24c2-473">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning to that effect is issued.</span></span> <span data-ttu-id="b24c2-474">删除 `new` 修饰符后，就会禁止显示此警告。</span><span class="sxs-lookup"><span data-stu-id="b24c2-474">This warning is suppressed by removing the `new` modifier.</span></span>

### <a name="access-modifiers"></a><span data-ttu-id="b24c2-475">访问修饰符</span><span class="sxs-lookup"><span data-stu-id="b24c2-475">Access modifiers</span></span>

<span data-ttu-id="b24c2-476">*Class_member_declaration*可以具有五种可能类型的声明的可访问性（已[声明的可访问性](basic-concepts.md#declared-accessibility)）之一： `public`、`protected internal`、`protected`、`internal`或 `private`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-476">A *class_member_declaration* can have any one of the five possible kinds of declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal`, or `private`.</span></span> <span data-ttu-id="b24c2-477">除了 `protected internal` 组合以外，它是一种编译时错误，用于指定多个访问修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-477">Except for the `protected internal` combination, it is a compile-time error to specify more than one access modifier.</span></span> <span data-ttu-id="b24c2-478">如果*class_member_declaration*不包含任何访问修饰符，则采用 `private`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-478">When a *class_member_declaration* does not include any access modifiers, `private` is assumed.</span></span>

### <a name="constituent-types"></a><span data-ttu-id="b24c2-479">构成类型</span><span class="sxs-lookup"><span data-stu-id="b24c2-479">Constituent types</span></span>

<span data-ttu-id="b24c2-480">在成员的声明中使用的类型称为成员的构成类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-480">Types that are used in the declaration of a member are called the constituent types of that member.</span></span> <span data-ttu-id="b24c2-481">可能的构成类型为常量、字段、属性、事件或索引器的类型、方法或运算符的返回类型，以及方法、索引器、运算符或实例构造函数的参数类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-481">Possible constituent types are the type of a constant, field, property, event, or indexer, the return type of a method or operator, and the parameter types of a method, indexer, operator, or instance constructor.</span></span> <span data-ttu-id="b24c2-482">成员的构成类型必须至少与该成员本身具有相同的可访问性（[辅助功能约束](basic-concepts.md#accessibility-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-482">The constituent types of a member must be at least as accessible as that member itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

### <a name="static-and-instance-members"></a><span data-ttu-id="b24c2-483">静态成员和实例成员</span><span class="sxs-lookup"><span data-stu-id="b24c2-483">Static and instance members</span></span>

<span data-ttu-id="b24c2-484">类的成员为***静态成员***或***实例成员***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-484">Members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="b24c2-485">一般来说，将静态成员视为属于类类型和属于对象的实例成员（类类型的实例）非常有用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-485">Generally speaking, it is useful to think of static members as belonging to class types and instance members as belonging to objects (instances of class types).</span></span>

<span data-ttu-id="b24c2-486">当字段、方法、属性、事件、运算符或构造函数声明包含 `static` 修饰符时，它将声明一个静态成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-486">When a field, method, property, event, operator, or constructor declaration includes a `static` modifier, it declares a static member.</span></span> <span data-ttu-id="b24c2-487">此外，常数或类型声明隐式声明静态成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-487">In addition, a constant or type declaration implicitly declares a static member.</span></span> <span data-ttu-id="b24c2-488">静态成员具有以下特征：</span><span class="sxs-lookup"><span data-stu-id="b24c2-488">Static members have the following characteristics:</span></span>

*  <span data-ttu-id="b24c2-489">在 `E.M`形式的*member_access* （[成员访问](expressions.md#member-access)）中引用静态成员 `M` 时，`E` 必须表示包含 `M`的类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-489">When a static member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote a type containing `M`.</span></span> <span data-ttu-id="b24c2-490">`E` 表示实例，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-490">It is a compile-time error for `E` to denote an instance.</span></span>
*  <span data-ttu-id="b24c2-491">静态字段仅标识给定封闭式类类型的所有实例共享的一个存储位置。</span><span class="sxs-lookup"><span data-stu-id="b24c2-491">A static field identifies exactly one storage location to be shared by all instances of a given closed class type.</span></span> <span data-ttu-id="b24c2-492">无论给定封闭式类类型创建了多少个实例，都只有一个静态字段副本。</span><span class="sxs-lookup"><span data-stu-id="b24c2-492">No matter how many instances of a given closed class type are created, there is only ever one copy of a static field.</span></span>
*  <span data-ttu-id="b24c2-493">静态函数成员（方法、属性、事件、运算符或构造函数）对特定实例不起作用，并且在此类函数成员中引用 `this` 是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-493">A static function member (method, property, event, operator, or constructor) does not operate on a specific instance, and it is a compile-time error to refer to `this` in such a function member.</span></span>

<span data-ttu-id="b24c2-494">当字段、方法、属性、事件、索引器、构造函数或析构函数声明不包含 `static` 修饰符时，它将声明一个实例成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-494">When a field, method, property, event, indexer, constructor, or destructor declaration does not include a `static` modifier, it declares an instance member.</span></span> <span data-ttu-id="b24c2-495">（实例成员有时称为非静态成员。）实例成员具有以下特征：</span><span class="sxs-lookup"><span data-stu-id="b24c2-495">(An instance member is sometimes called a non-static member.) Instance members have the following characteristics:</span></span>

*  <span data-ttu-id="b24c2-496">在 `E.M`形式的*member_access* （[成员访问](expressions.md#member-access)）中引用实例成员 `M` 时，`E` 必须表示包含 `M`的类型的实例。</span><span class="sxs-lookup"><span data-stu-id="b24c2-496">When an instance member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote an instance of a type containing `M`.</span></span> <span data-ttu-id="b24c2-497">`E` 表示类型，则是绑定时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-497">It is a binding-time error for `E` to denote a type.</span></span>
*  <span data-ttu-id="b24c2-498">类的每个实例都包含该类的所有实例字段的单独集。</span><span class="sxs-lookup"><span data-stu-id="b24c2-498">Every instance of a class contains a separate set of all instance fields of the class.</span></span>
*  <span data-ttu-id="b24c2-499">实例函数成员（方法、属性、索引器、实例构造函数或析构函数）对类的给定实例进行操作，可以将此实例作为 `this` （[此访问](expressions.md#this-access)）进行访问。</span><span class="sxs-lookup"><span data-stu-id="b24c2-499">An instance function member (method, property, indexer, instance constructor, or destructor) operates on a given instance of the class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="b24c2-500">下面的示例演示用于访问静态成员和实例成员的规则：</span><span class="sxs-lookup"><span data-stu-id="b24c2-500">The following example illustrates the rules for accessing static and instance members:</span></span>
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

<span data-ttu-id="b24c2-501">`F` 方法表明，在实例函数成员中，可使用*simple_name* （[简单名称](expressions.md#simple-names)）来访问实例成员和静态成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-501">The `F` method shows that in an instance function member, a *simple_name* ([Simple names](expressions.md#simple-names)) can be used to access both instance members and static members.</span></span> <span data-ttu-id="b24c2-502">`G` 方法表明，在静态函数成员中，通过*simple_name*访问实例成员是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-502">The `G` method shows that in a static function member, it is a compile-time error to access an instance member through a *simple_name*.</span></span> <span data-ttu-id="b24c2-503">`Main` 方法表明，在*member_access* （[成员访问](expressions.md#member-access)）中，必须通过实例访问实例成员，并且必须通过类型访问静态成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-503">The `Main` method shows that in a *member_access* ([Member access](expressions.md#member-access)), instance members must be accessed through instances, and static members must be accessed through types.</span></span>

### <a name="nested-types"></a><span data-ttu-id="b24c2-504">嵌套类型</span><span class="sxs-lookup"><span data-stu-id="b24c2-504">Nested types</span></span>

<span data-ttu-id="b24c2-505">在类或结构声明中声明的类型称为***嵌套类型***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-505">A type declared within a class or struct declaration is called a ***nested type***.</span></span> <span data-ttu-id="b24c2-506">在编译单元或命名空间内声明的类型称为***非嵌套类型***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-506">A type that is declared within a compilation unit or namespace is called a ***non-nested type***.</span></span>

<span data-ttu-id="b24c2-507">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-507">In the example</span></span>
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
<span data-ttu-id="b24c2-508">类 `B` 是嵌套类型，因为它是在类 `A`中声明的，而类 `A` 是一个非嵌套类型，因为它是在编译单元中声明的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-508">class `B` is a nested type because it is declared within class `A`, and class `A` is a non-nested type because it is declared within a compilation unit.</span></span>

#### <a name="fully-qualified-name"></a><span data-ttu-id="b24c2-509">完全限定的名称</span><span class="sxs-lookup"><span data-stu-id="b24c2-509">Fully qualified name</span></span>

<span data-ttu-id="b24c2-510">嵌套类型的完全限定名（[完全限定](basic-concepts.md#fully-qualified-names)名）为 `S.N`，其中 `S` 是声明类型 `N` 的类型的完全限定名称。</span><span class="sxs-lookup"><span data-stu-id="b24c2-510">The fully qualified name ([Fully qualified names](basic-concepts.md#fully-qualified-names)) for a nested type is `S.N` where `S` is the fully qualified name of the type in which type `N` is declared.</span></span>

#### <a name="declared-accessibility"></a><span data-ttu-id="b24c2-511">声明的可访问性</span><span class="sxs-lookup"><span data-stu-id="b24c2-511">Declared accessibility</span></span>

<span data-ttu-id="b24c2-512">非嵌套类型可以具有 `public` 或 `internal` 声明的可访问性，并且默认情况下 `internal` 声明为可访问性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-512">Non-nested types can have `public` or `internal` declared accessibility and have `internal` declared accessibility by default.</span></span> <span data-ttu-id="b24c2-513">嵌套类型也可以具有这些形式的声明的可访问性，还可以包含一个或多个声明的可访问性的其他形式，具体取决于包含类型是否为类或结构：</span><span class="sxs-lookup"><span data-stu-id="b24c2-513">Nested types can have these forms of declared accessibility too, plus one or more additional forms of declared accessibility, depending on whether the containing type is a class or struct:</span></span>

*  <span data-ttu-id="b24c2-514">在类中声明的嵌套类型可以具有五种形式的声明的可访问性（`public`、`protected internal`、`protected`、`internal`或 `private`），与其他类成员一样，默认为 `private` 声明的可访问性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-514">A nested type that is declared in a class can have any of five forms of declared accessibility (`public`, `protected internal`, `protected`, `internal`, or `private`) and, like other class members, defaults to `private` declared accessibility.</span></span>
*  <span data-ttu-id="b24c2-515">在结构中声明的嵌套类型可以有三种形式的声明的可访问性（`public`、`internal`或 `private`），与其他结构成员一样，默认情况下 `private` 声明的可访问性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-515">A nested type that is declared in a struct can have any of three forms of declared accessibility (`public`, `internal`, or `private`) and, like other struct members, defaults to `private` declared accessibility.</span></span>

<span data-ttu-id="b24c2-516">示例</span><span class="sxs-lookup"><span data-stu-id="b24c2-516">The example</span></span>
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
<span data-ttu-id="b24c2-517">声明一个私有嵌套类 `Node`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-517">declares a private nested class `Node`.</span></span>

#### <a name="hiding"></a><span data-ttu-id="b24c2-518">遮盖</span><span class="sxs-lookup"><span data-stu-id="b24c2-518">Hiding</span></span>

<span data-ttu-id="b24c2-519">嵌套类型可以隐藏基成员（[命名隐藏](basic-concepts.md#name-hiding)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-519">A nested type may hide ([Name hiding](basic-concepts.md#name-hiding)) a base member.</span></span> <span data-ttu-id="b24c2-520">在嵌套类型声明中允许使用 `new` 修饰符，以便可以显式表达隐藏。</span><span class="sxs-lookup"><span data-stu-id="b24c2-520">The `new` modifier is permitted on nested type declarations so that hiding can be expressed explicitly.</span></span> <span data-ttu-id="b24c2-521">示例</span><span class="sxs-lookup"><span data-stu-id="b24c2-521">The example</span></span>
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
<span data-ttu-id="b24c2-522">显示隐藏 `Base`中定义的方法 `M` 的嵌套类 `M`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-522">shows a nested class `M` that hides the method `M` defined in `Base`.</span></span>

#### <a name="this-access"></a><span data-ttu-id="b24c2-523">此访问</span><span class="sxs-lookup"><span data-stu-id="b24c2-523">this access</span></span>

<span data-ttu-id="b24c2-524">嵌套类型及其包含类型与*this_access* （[此访问](expressions.md#this-access)）没有特殊关系。</span><span class="sxs-lookup"><span data-stu-id="b24c2-524">A nested type and its containing type do not have a special relationship with regard to *this_access* ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="b24c2-525">具体来说，不能使用嵌套类型中 `this` 引用包含类型的实例成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-525">Specifically, `this` within a nested type cannot be used to refer to instance members of the containing type.</span></span> <span data-ttu-id="b24c2-526">如果嵌套类型需要访问其包含类型的实例成员，则可以通过为包含类型的实例提供作为嵌套类型的构造函数参数的 `this` 来提供访问权限。</span><span class="sxs-lookup"><span data-stu-id="b24c2-526">In cases where a nested type needs access to the instance members of its containing type, access can be provided by providing the `this` for the instance of the containing type as a constructor argument for the nested type.</span></span> <span data-ttu-id="b24c2-527">下面的示例</span><span class="sxs-lookup"><span data-stu-id="b24c2-527">The following example</span></span>
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
<span data-ttu-id="b24c2-528">显示此方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-528">shows this technique.</span></span> <span data-ttu-id="b24c2-529">实例 `C` 创建 `Nested` 的实例，并将其自己的 `this` 传递到 `Nested`的构造函数，以提供对 `C`实例成员的后续访问。</span><span class="sxs-lookup"><span data-stu-id="b24c2-529">An instance of `C` creates an instance of `Nested` and passes its own `this` to `Nested`'s constructor in order to provide subsequent access to `C`'s instance members.</span></span>

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a><span data-ttu-id="b24c2-530">访问包含类型的私有和受保护成员</span><span class="sxs-lookup"><span data-stu-id="b24c2-530">Access to private and protected members of the containing type</span></span>

<span data-ttu-id="b24c2-531">嵌套类型可以访问其包含类型可以访问的所有成员，包括包含类型的成员，这些成员具有 `private` 和 `protected` 声明可访问性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-531">A nested type has access to all of the members that are accessible to its containing type, including members of the containing type that have `private` and `protected` declared accessibility.</span></span> <span data-ttu-id="b24c2-532">示例</span><span class="sxs-lookup"><span data-stu-id="b24c2-532">The example</span></span>
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
<span data-ttu-id="b24c2-533">显示 `C` 包含嵌套类 `Nested`的类。</span><span class="sxs-lookup"><span data-stu-id="b24c2-533">shows a class `C` that contains a nested class `Nested`.</span></span> <span data-ttu-id="b24c2-534">在 `Nested`中，方法 `G` 调用 `C`中定义 `F` 静态方法，`F` 具有私有声明的可访问性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-534">Within `Nested`, the method `G` calls the static method `F` defined in `C`, and `F` has private declared accessibility.</span></span>

<span data-ttu-id="b24c2-535">嵌套类型还可以访问在其包含类型的基类型中定义的受保护成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-535">A nested type also may access protected members defined in a base type of its containing type.</span></span> <span data-ttu-id="b24c2-536">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-536">In the example</span></span>
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
<span data-ttu-id="b24c2-537">嵌套类 `Derived.Nested` 通过调用 `Derived`的实例来访问 `Derived`的 `Base`基类中定义的受保护的方法 `F`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-537">the nested class `Derived.Nested` accesses the protected method `F` defined in `Derived`'s base class, `Base`, by calling through an instance of `Derived`.</span></span>

#### <a name="nested-types-in-generic-classes"></a><span data-ttu-id="b24c2-538">泛型类中的嵌套类型</span><span class="sxs-lookup"><span data-stu-id="b24c2-538">Nested types in generic classes</span></span>

<span data-ttu-id="b24c2-539">泛型类声明可以包含嵌套类型声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-539">A generic class declaration can contain nested type declarations.</span></span> <span data-ttu-id="b24c2-540">可以在嵌套类型中使用封闭类的类型参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-540">The type parameters of the enclosing class can be used within the nested types.</span></span> <span data-ttu-id="b24c2-541">嵌套类型声明可以包含其他仅适用于嵌套类型的类型参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-541">A nested type declaration can contain additional type parameters that apply only to the nested type.</span></span>

<span data-ttu-id="b24c2-542">泛型类声明中包含的每个类型声明都是隐式的泛型类型声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-542">Every type declaration contained within a generic class declaration is implicitly a generic type declaration.</span></span> <span data-ttu-id="b24c2-543">写入嵌套在泛型类型中的类型的引用时，必须将包含构造类型（包括其类型参数）命名为。</span><span class="sxs-lookup"><span data-stu-id="b24c2-543">When writing a reference to a type nested within a generic type, the containing constructed type, including its type arguments, must be named.</span></span> <span data-ttu-id="b24c2-544">但是，从外部类中可以使用嵌套类型而无需进行限定;构造嵌套类型时，可以隐式使用外部类的实例类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-544">However, from within the outer class, the nested type can be used without qualification; the instance type of the outer class can be implicitly used when constructing the nested type.</span></span> <span data-ttu-id="b24c2-545">下面的示例演示了三种不同的方法来引用从 `Inner`创建的构造类型;前两个等效项：</span><span class="sxs-lookup"><span data-stu-id="b24c2-545">The following example shows three different correct ways to refer to a constructed type created from `Inner`; the first two are equivalent:</span></span>
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

<span data-ttu-id="b24c2-546">尽管编程样式不正确，但嵌套类型中的类型参数可以隐藏在外部类型中声明的成员或类型参数：</span><span class="sxs-lookup"><span data-stu-id="b24c2-546">Although it is bad programming style, a type parameter in a nested type can hide a member or type parameter declared in the outer type:</span></span>
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a><span data-ttu-id="b24c2-547">保留的成员名称</span><span class="sxs-lookup"><span data-stu-id="b24c2-547">Reserved member names</span></span>

<span data-ttu-id="b24c2-548">为了便于基础C#运行时实现，对于作为属性、事件或索引器的每个源成员声明，实现必须根据成员声明的类型、名称和类型保留两个方法签名。</span><span class="sxs-lookup"><span data-stu-id="b24c2-548">To facilitate the underlying C# run-time implementation, for each source member declaration that is a property, event, or indexer, the implementation must reserve two method signatures based on the kind of the member declaration, its name, and its type.</span></span> <span data-ttu-id="b24c2-549">如果程序声明的成员的签名与这些保留签名之一匹配，则这是编译时错误，即使基础运行时实现不使用这些保留。</span><span class="sxs-lookup"><span data-stu-id="b24c2-549">It is a compile-time error for a program to declare a member whose signature matches one of these reserved signatures, even if the underlying run-time implementation does not make use of these reservations.</span></span>

<span data-ttu-id="b24c2-550">保留名称不会引入声明，因此它们不参与成员查找。</span><span class="sxs-lookup"><span data-stu-id="b24c2-550">The reserved names do not introduce declarations, thus they do not participate in member lookup.</span></span> <span data-ttu-id="b24c2-551">但是，声明的关联的保留方法签名确实参与了继承（[继承](classes.md#inheritance)），并且可使用 `new` 修饰符（[新修饰符](classes.md#the-new-modifier)）隐藏。</span><span class="sxs-lookup"><span data-stu-id="b24c2-551">However, a declaration's associated reserved method signatures do participate in inheritance ([Inheritance](classes.md#inheritance)), and can be hidden with the `new` modifier ([The new modifier](classes.md#the-new-modifier)).</span></span>

<span data-ttu-id="b24c2-552">保留这些名称有三个用途：</span><span class="sxs-lookup"><span data-stu-id="b24c2-552">The reservation of these names serves three purposes:</span></span>

*  <span data-ttu-id="b24c2-553">如果为，则允许基础实现使用普通标识符作为方法名称，以获取或设置对C#语言功能的访问权限。</span><span class="sxs-lookup"><span data-stu-id="b24c2-553">To allow the underlying implementation to use an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="b24c2-554">允许其他语言使用普通标识符作为方法名称进行互操作，以获取或设置对C#语言功能的访问权限。</span><span class="sxs-lookup"><span data-stu-id="b24c2-554">To allow other languages to interoperate using an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="b24c2-555">为了帮助确保由一个符合的编译器接受的源接受另一个，通过使保留成员名称的细节在所有C#实现中保持一致。</span><span class="sxs-lookup"><span data-stu-id="b24c2-555">To help ensure that the source accepted by one conforming compiler is accepted by another, by making the specifics of reserved member names consistent across all C# implementations.</span></span>

<span data-ttu-id="b24c2-556">析构函数的声明（[析构函数](classes.md#destructors)）还会导致保留签名（[为析构函数保留的成员名称](classes.md#member-names-reserved-for-destructors)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-556">The declaration of a destructor ([Destructors](classes.md#destructors)) also causes a signature to be reserved ([Member names reserved for destructors](classes.md#member-names-reserved-for-destructors)).</span></span>

#### <a name="member-names-reserved-for-properties"></a><span data-ttu-id="b24c2-557">为属性保留的成员名称</span><span class="sxs-lookup"><span data-stu-id="b24c2-557">Member names reserved for properties</span></span>

<span data-ttu-id="b24c2-558">对于类型 `P` 的属性[（](classes.md#properties)属性`T`），将保留以下签名：</span><span class="sxs-lookup"><span data-stu-id="b24c2-558">For a property `P` ([Properties](classes.md#properties)) of type `T`, the following signatures are reserved:</span></span>

```csharp
T get_P();
void set_P(T value);
```

<span data-ttu-id="b24c2-559">即使属性是只读的或只写的，都保留两个签名。</span><span class="sxs-lookup"><span data-stu-id="b24c2-559">Both signatures are reserved, even if the property is read-only or write-only.</span></span>

<span data-ttu-id="b24c2-560">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-560">In the example</span></span>
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
<span data-ttu-id="b24c2-561">类 `A` 定义只读属性 `P`，从而保留 `get_P` 和 `set_P` 方法的签名。</span><span class="sxs-lookup"><span data-stu-id="b24c2-561">a class `A` defines a read-only property `P`, thus reserving signatures for `get_P` and `set_P` methods.</span></span> <span data-ttu-id="b24c2-562">类 `B` 从 `A` 派生，并隐藏这两个保留的签名。</span><span class="sxs-lookup"><span data-stu-id="b24c2-562">A class `B` derives from `A` and hides both of these reserved signatures.</span></span> <span data-ttu-id="b24c2-563">该示例生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="b24c2-563">The example produces the output:</span></span>
```console
123
123
456
```

#### <a name="member-names-reserved-for-events"></a><span data-ttu-id="b24c2-564">为事件保留的成员名称</span><span class="sxs-lookup"><span data-stu-id="b24c2-564">Member names reserved for events</span></span>

<span data-ttu-id="b24c2-565">对于 `T`委托类型的事件 `E` （[事件](classes.md#events)），将保留以下签名：</span><span class="sxs-lookup"><span data-stu-id="b24c2-565">For an event `E` ([Events](classes.md#events)) of delegate type `T`, the following signatures are reserved:</span></span>
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a><span data-ttu-id="b24c2-566">为索引器保留的成员名称</span><span class="sxs-lookup"><span data-stu-id="b24c2-566">Member names reserved for indexers</span></span>

<span data-ttu-id="b24c2-567">对于类型 `T` `L`的索引器（[索引器](classes.md#indexers)），将保留以下签名：</span><span class="sxs-lookup"><span data-stu-id="b24c2-567">For an indexer ([Indexers](classes.md#indexers)) of type `T` with parameter-list `L`, the following signatures are reserved:</span></span>
```csharp
T get_Item(L);
void set_Item(L, T value);
```

<span data-ttu-id="b24c2-568">即使索引器是只读的或只写的，也会保留两个签名。</span><span class="sxs-lookup"><span data-stu-id="b24c2-568">Both signatures are reserved, even if the indexer is read-only or write-only.</span></span>

<span data-ttu-id="b24c2-569">而且，成员名称 `Item` 保留。</span><span class="sxs-lookup"><span data-stu-id="b24c2-569">Furthermore the member name `Item` is reserved.</span></span>

#### <a name="member-names-reserved-for-destructors"></a><span data-ttu-id="b24c2-570">为析构函数保留的成员名称</span><span class="sxs-lookup"><span data-stu-id="b24c2-570">Member names reserved for destructors</span></span>

<span data-ttu-id="b24c2-571">对于包含析构函数（[析构函数](classes.md#destructors)）的类，将保留以下签名：</span><span class="sxs-lookup"><span data-stu-id="b24c2-571">For a class containing a destructor ([Destructors](classes.md#destructors)), the following signature is reserved:</span></span>
```csharp
void Finalize();
```

## <a name="constants"></a><span data-ttu-id="b24c2-572">常量</span><span class="sxs-lookup"><span data-stu-id="b24c2-572">Constants</span></span>

<span data-ttu-id="b24c2-573">***常量***是表示常量值的类成员：可在编译时计算的值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-573">A ***constant*** is a class member that represents a constant value: a value that can be computed at compile-time.</span></span> <span data-ttu-id="b24c2-574">*Constant_declaration*引入了给定类型的一个或多个常量。</span><span class="sxs-lookup"><span data-stu-id="b24c2-574">A *constant_declaration* introduces one or more constants of a given type.</span></span>

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

<span data-ttu-id="b24c2-575">*Constant_declaration*可以包含一组*特性*（[特性](attributes.md)）、一个 `new` 修饰符（[新修饰符](classes.md#the-new-modifier)）和四个访问修饰符（[访问修饰符](classes.md#access-modifiers)）的有效组合。</span><span class="sxs-lookup"><span data-stu-id="b24c2-575">A *constant_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span> <span data-ttu-id="b24c2-576">特性和修饰符适用于*constant_declaration*声明的所有成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-576">The attributes and modifiers apply to all of the members declared by the *constant_declaration*.</span></span> <span data-ttu-id="b24c2-577">即使常量被视为静态成员， *constant_declaration*也既不需要也不允许 `static` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-577">Even though constants are considered static members, a *constant_declaration* neither requires nor allows a `static` modifier.</span></span> <span data-ttu-id="b24c2-578">同一修饰符在常数声明中多次出现是错误的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-578">It is an error for the same modifier to appear multiple times in a constant declaration.</span></span>

<span data-ttu-id="b24c2-579">*Constant_declaration*的*类型*指定声明引入的成员类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-579">The *type* of a *constant_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="b24c2-580">该类型后跟一个*constant_declarator*s 列表，其中每个都引入一个新成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-580">The type is followed by a list of *constant_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="b24c2-581">*Constant_declarator*由命名成员的*标识符*组成，后跟一个 "`=`" 标记，后跟一个提供成员值*constant_expression* （[常数表达式](expressions.md#constant-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-581">A *constant_declarator* consists of an *identifier* that names the member, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the member.</span></span>

<span data-ttu-id="b24c2-582">在常量声明中指定的*类型*必须是 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`、`bool`、`string`、 *enum_type*或*reference_type*。</span><span class="sxs-lookup"><span data-stu-id="b24c2-582">The *type* specified in a constant declaration must be `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, an *enum_type*, or a *reference_type*.</span></span> <span data-ttu-id="b24c2-583">每个*constant_expression*必须生成一个目标类型的值，或一个可通过隐式转换转换为目标类型的类型（[隐式转换](conversions.md#implicit-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-583">Each *constant_expression* must yield a value of the target type or of a type that can be converted to the target type by an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>

<span data-ttu-id="b24c2-584">常数的*类型*必须至少具有与常量本身相同的可访问性（[辅助功能约束](basic-concepts.md#accessibility-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-584">The *type* of a constant must be at least as accessible as the constant itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="b24c2-585">常量的值是使用*simple_name* （[简单名称](expressions.md#simple-names)）或*member_access* （[成员访问](expressions.md#member-access)）在表达式中获取的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-585">The value of a constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="b24c2-586">常数本身可以参与*constant_expression*。</span><span class="sxs-lookup"><span data-stu-id="b24c2-586">A constant can itself participate in a *constant_expression*.</span></span> <span data-ttu-id="b24c2-587">因此，在需要*constant_expression*的任何构造中都可以使用常量。</span><span class="sxs-lookup"><span data-stu-id="b24c2-587">Thus, a constant may be used in any construct that requires a *constant_expression*.</span></span> <span data-ttu-id="b24c2-588">此类构造的示例包括 `case` 标签、`goto case` 语句、`enum` 成员声明、特性和其他常量声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-588">Examples of such constructs include `case` labels, `goto case` statements, `enum` member declarations, attributes, and other constant declarations.</span></span>

<span data-ttu-id="b24c2-589">如[常数表达式](expressions.md#constant-expressions)中所述， *constant_expression*是可在编译时完全计算的表达式。</span><span class="sxs-lookup"><span data-stu-id="b24c2-589">As described in [Constant expressions](expressions.md#constant-expressions), a *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span> <span data-ttu-id="b24c2-590">由于不 `string` 的*reference_type*的非 null 值的唯一创建方法是应用 `new` 运算符，因此，由于*constant_expression*不允许使用 `new` 运算符，因此 reference_type 之外*的 `string` 的*常量的唯一可能值为 `null`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-590">Since the only way to create a non-null value of a *reference_type* other than `string` is to apply the `new` operator, and since the `new` operator is not permitted in a *constant_expression*, the only possible value for constants of *reference_type*s other than `string` is `null`.</span></span>

<span data-ttu-id="b24c2-591">如果需要常数值的符号名称，但在常数声明中不允许该值的类型，或者无法在*constant_expression*编译时计算该值，则可以改用 `readonly` 字段（[Readonly 字段](classes.md#readonly-fields)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-591">When a symbolic name for a constant value is desired, but when the type of that value is not permitted in a constant declaration, or when the value cannot be computed at compile-time by a *constant_expression*, a `readonly` field ([Readonly fields](classes.md#readonly-fields)) may be used instead.</span></span>

<span data-ttu-id="b24c2-592">声明多个常量的常量声明等效于多个具有相同属性、修饰符和类型的单个常量声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-592">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="b24c2-593">例如</span><span class="sxs-lookup"><span data-stu-id="b24c2-593">For example</span></span>
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
<span data-ttu-id="b24c2-594">等效于</span><span class="sxs-lookup"><span data-stu-id="b24c2-594">is equivalent to</span></span>
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

<span data-ttu-id="b24c2-595">只要依赖项不属于循环本质，就允许常量依赖于同一个程序中的其他常量。</span><span class="sxs-lookup"><span data-stu-id="b24c2-595">Constants are permitted to depend on other constants within the same program as long as the dependencies are not of a circular nature.</span></span> <span data-ttu-id="b24c2-596">编译器会自动排列，以适当的顺序计算常量声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-596">The compiler automatically arranges to evaluate the constant declarations in the appropriate order.</span></span> <span data-ttu-id="b24c2-597">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-597">In the example</span></span>
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
<span data-ttu-id="b24c2-598">编译器首先计算 `A.Y`，然后计算 `B.Z`，最后计算 `A.X`，并 `10`、`11`和 `12`生成值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-598">the compiler first evaluates `A.Y`, then evaluates `B.Z`, and finally evaluates `A.X`, producing the values `10`, `11`, and `12`.</span></span> <span data-ttu-id="b24c2-599">常数声明可能依赖于其他程序中的常量，但这种依赖关系只能在一个方向上进行。</span><span class="sxs-lookup"><span data-stu-id="b24c2-599">Constant declarations may depend on constants from other programs, but such dependencies are only possible in one direction.</span></span> <span data-ttu-id="b24c2-600">参考上面的示例，如果在不同的程序中声明了 `A` 和 `B`，则 `A.X` 可能依赖于 `B.Z`，但 `B.Z` 随后不会同时依赖于 `A.Y`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-600">Referring to the example above, if `A` and `B` were declared in separate programs, it would be possible for `A.X` to depend on `B.Z`, but `B.Z` could then not simultaneously depend on `A.Y`.</span></span>

## <a name="fields"></a><span data-ttu-id="b24c2-601">字段</span><span class="sxs-lookup"><span data-stu-id="b24c2-601">Fields</span></span>

<span data-ttu-id="b24c2-602">***字段***是表示与对象或类关联的变量的成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-602">A ***field*** is a member that represents a variable associated with an object or class.</span></span> <span data-ttu-id="b24c2-603">*Field_declaration*引入了给定类型的一个或多个字段。</span><span class="sxs-lookup"><span data-stu-id="b24c2-603">A *field_declaration* introduces one or more fields of a given type.</span></span>

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

<span data-ttu-id="b24c2-604">*Field_declaration*可以包含一组*属性*（[特性](attributes.md)）、一个 `new` 修饰符（[新修饰符](classes.md#the-new-modifier)）、四个访问修饰符（[访问修饰符](classes.md#access-modifiers)）和一个 `static` 修饰符（[静态和实例字段](classes.md#static-and-instance-fields)）的有效组合。</span><span class="sxs-lookup"><span data-stu-id="b24c2-604">A *field_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and a `static` modifier ([Static and instance fields](classes.md#static-and-instance-fields)).</span></span> <span data-ttu-id="b24c2-605">此外， *field_declaration*可以包括 `readonly` 修饰符（[Readonly 字段](classes.md#readonly-fields)）或 `volatile` 修饰符（[可变字段](classes.md#volatile-fields)），但不能同时包含两者。</span><span class="sxs-lookup"><span data-stu-id="b24c2-605">In addition, a *field_declaration* may include a `readonly` modifier ([Readonly fields](classes.md#readonly-fields)) or a `volatile` modifier ([Volatile fields](classes.md#volatile-fields)) but not both.</span></span> <span data-ttu-id="b24c2-606">特性和修饰符适用于*field_declaration*声明的所有成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-606">The attributes and modifiers apply to all of the members declared by the *field_declaration*.</span></span> <span data-ttu-id="b24c2-607">同一修饰符在字段声明中多次出现是错误的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-607">It is an error for the same modifier to appear multiple times in a field declaration.</span></span>

<span data-ttu-id="b24c2-608">*Field_declaration*的*类型*指定声明引入的成员类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-608">The *type* of a *field_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="b24c2-609">该类型后跟一个*variable_declarator*s 列表，其中每个都引入一个新成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-609">The type is followed by a list of *variable_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="b24c2-610">*Variable_declarator*包含一个标识符，*该标识符*对成员进行命名，可选择后跟 "`=`" 标记和提供该成员的初始值的*variable_initializer* （[变量初始值设定项](classes.md#variable-initializers)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-610">A *variable_declarator* consists of an *identifier* that names that member, optionally followed by an "`=`" token and a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)) that gives the initial value of that member.</span></span>

<span data-ttu-id="b24c2-611">字段的*类型*必须至少与字段本身具有相同的可访问性（[辅助功能约束](basic-concepts.md#accessibility-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-611">The *type* of a field must be at least as accessible as the field itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="b24c2-612">字段的值是在表达式中使用*simple_name* （[简单名称](expressions.md#simple-names)）或*member_access* （[成员访问](expressions.md#member-access)）获取的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-612">The value of a field is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="b24c2-613">使用*赋值*（[赋值运算符](expressions.md#assignment-operators)）修改非只读字段的值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-613">The value of a non-readonly field is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="b24c2-614">非只读字段的值可以使用后缀增量和减量运算符（[后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators)）以及前缀增量和减量运算符（[前缀增量和减量运算符](expressions.md#prefix-increment-and-decrement-operators)）来获取和修改。</span><span class="sxs-lookup"><span data-stu-id="b24c2-614">The value of a non-readonly field can be both obtained and modified using postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)) and prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="b24c2-615">声明多个字段的字段声明等效于具有相同属性、修饰符和类型的单个字段的多个声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-615">A field declaration that declares multiple fields is equivalent to multiple declarations of single fields with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="b24c2-616">例如</span><span class="sxs-lookup"><span data-stu-id="b24c2-616">For example</span></span>
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
<span data-ttu-id="b24c2-617">等效于</span><span class="sxs-lookup"><span data-stu-id="b24c2-617">is equivalent to</span></span>
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a><span data-ttu-id="b24c2-618">静态和实例字段</span><span class="sxs-lookup"><span data-stu-id="b24c2-618">Static and instance fields</span></span>

<span data-ttu-id="b24c2-619">当字段声明包括 `static` 修饰符时，该声明引入的字段是***静态字段***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-619">When a field declaration includes a `static` modifier, the fields introduced by the declaration are ***static fields***.</span></span> <span data-ttu-id="b24c2-620">如果不存在 `static` 修饰符，则声明引入的字段为***实例字段***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-620">When no `static` modifier is present, the fields introduced by the declaration are ***instance fields***.</span></span> <span data-ttu-id="b24c2-621">静态字段和实例字段是受支持的几种变量（[变量](variables.md)） C#中的两种，有时它们分别被称为***静态变量***和***实例变量***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-621">Static fields and instance fields are two of the several kinds of variables ([Variables](variables.md)) supported by C#, and at times they are referred to as ***static variables*** and ***instance variables***, respectively.</span></span>

<span data-ttu-id="b24c2-622">静态字段不是特定实例的一部分;而是在已关闭类型的所有实例（[开放式类型和已关闭类型](types.md#open-and-closed-types)）之间共享。</span><span class="sxs-lookup"><span data-stu-id="b24c2-622">A static field is not part of a specific instance; instead, it is shared amongst all instances of a closed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="b24c2-623">无论已创建已关闭类类型的多少实例，都只有一个静态字段副本用于关联的应用程序域。</span><span class="sxs-lookup"><span data-stu-id="b24c2-623">No matter how many instances of a closed class type are created, there is only ever one copy of a static field for the associated application domain.</span></span>

<span data-ttu-id="b24c2-624">例如：</span><span class="sxs-lookup"><span data-stu-id="b24c2-624">For example:</span></span>
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

<span data-ttu-id="b24c2-625">实例字段属于实例。</span><span class="sxs-lookup"><span data-stu-id="b24c2-625">An instance field belongs to an instance.</span></span> <span data-ttu-id="b24c2-626">具体而言，类的每个实例都包含该类的所有实例字段的单独集。</span><span class="sxs-lookup"><span data-stu-id="b24c2-626">Specifically, every instance of a class contains a separate set of all the instance fields of that class.</span></span>

<span data-ttu-id="b24c2-627">如果在窗体 `E.M`的*member_access* （[成员访问](expressions.md#member-access)）中引用字段，`M` 为静态字段，则 `E` 必须表示包含 `M`的类型，如果 `M` 是实例字段，则 E 必须表示包含 `M`的类型的实例。</span><span class="sxs-lookup"><span data-stu-id="b24c2-627">When a field is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static field, `E` must denote a type containing `M`, and if `M` is an instance field, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="b24c2-628">静态成员和实例成员之间的差异在[静态成员和实例成员](classes.md#static-and-instance-members)中进行了进一步讨论。</span><span class="sxs-lookup"><span data-stu-id="b24c2-628">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="readonly-fields"></a><span data-ttu-id="b24c2-629">只读字段</span><span class="sxs-lookup"><span data-stu-id="b24c2-629">Readonly fields</span></span>

<span data-ttu-id="b24c2-630">当*field_declaration*包含 `readonly` 修饰符时，由声明引入的字段为***只读字段***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-630">When a *field_declaration* includes a `readonly` modifier, the fields introduced by the declaration are ***readonly fields***.</span></span> <span data-ttu-id="b24c2-631">向 readonly 字段的直接赋值只能作为声明的一部分出现，或者出现在同一类的实例构造函数或静态构造函数中。</span><span class="sxs-lookup"><span data-stu-id="b24c2-631">Direct assignments to readonly fields can only occur as part of that declaration or in an instance constructor or static constructor in the same class.</span></span> <span data-ttu-id="b24c2-632">（Readonly 字段可以在这些上下文中指定多次。）具体而言，仅允许在以下上下文中对 `readonly` 字段进行直接赋值：</span><span class="sxs-lookup"><span data-stu-id="b24c2-632">(A readonly field can be assigned to multiple times in these contexts.) Specifically, direct assignments to a `readonly` field are permitted only in the following contexts:</span></span>

*  <span data-ttu-id="b24c2-633">在引入字段的*variable_declarator*中（通过在声明中包含*variable_initializer* ）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-633">In the *variable_declarator* that introduces the field (by including a *variable_initializer* in the declaration).</span></span>
*  <span data-ttu-id="b24c2-634">对于实例字段，在包含字段声明的类的实例构造函数中;对于静态字段，在包含字段声明的类的静态构造函数中。</span><span class="sxs-lookup"><span data-stu-id="b24c2-634">For an instance field, in the instance constructors of the class that contains the field declaration; for a static field, in the static constructor of the class that contains the field declaration.</span></span> <span data-ttu-id="b24c2-635">它们也是将 `readonly` 字段作为 `out` 或 `ref` 参数传递的唯一上下文。</span><span class="sxs-lookup"><span data-stu-id="b24c2-635">These are also the only contexts in which it is valid to pass a `readonly` field as an `out` or `ref` parameter.</span></span>

<span data-ttu-id="b24c2-636">尝试分配到 `readonly` 字段，或在任何其他上下文中将其作为 `out` 或 `ref` 参数传递是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-636">Attempting to assign to a `readonly` field or pass it as an `out` or `ref` parameter in any other context is a compile-time error.</span></span>

#### <a name="using-static-readonly-fields-for-constants"></a><span data-ttu-id="b24c2-637">使用常量的静态 readonly 字段</span><span class="sxs-lookup"><span data-stu-id="b24c2-637">Using static readonly fields for constants</span></span>

<span data-ttu-id="b24c2-638">如果需要常数值的符号名称，但在 `const` 声明中不允许使用值的类型，或者在编译时无法计算该值时，`static readonly` 字段非常有用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-638">A `static readonly` field is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a `const` declaration, or when the value cannot be computed at compile-time.</span></span> <span data-ttu-id="b24c2-639">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-639">In the example</span></span>
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
<span data-ttu-id="b24c2-640">`Black`、`White`、`Red`、`Green`和 `Blue` 成员不能声明为 `const` 成员，因为它们的值不能在编译时进行计算。</span><span class="sxs-lookup"><span data-stu-id="b24c2-640">the `Black`, `White`, `Red`, `Green`, and `Blue` members cannot be declared as `const` members because their values cannot be computed at compile-time.</span></span> <span data-ttu-id="b24c2-641">但是，将它们声明 `static readonly` 的效果大致相同。</span><span class="sxs-lookup"><span data-stu-id="b24c2-641">However, declaring them `static readonly` instead has much the same effect.</span></span>

#### <a name="versioning-of-constants-and-static-readonly-fields"></a><span data-ttu-id="b24c2-642">常量和静态 readonly 字段的版本控制</span><span class="sxs-lookup"><span data-stu-id="b24c2-642">Versioning of constants and static readonly fields</span></span>

<span data-ttu-id="b24c2-643">常量和 readonly 字段的二进制版本控制语义不同。</span><span class="sxs-lookup"><span data-stu-id="b24c2-643">Constants and readonly fields have different binary versioning semantics.</span></span> <span data-ttu-id="b24c2-644">当表达式引用常量时，将在编译时获取常量的值，但当表达式引用 readonly 字段时，不会在运行时获取字段的值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-644">When an expression references a constant, the value of the constant is obtained at compile-time, but when an expression references a readonly field, the value of the field is not obtained until run-time.</span></span> <span data-ttu-id="b24c2-645">假设应用程序包含两个单独的程序：</span><span class="sxs-lookup"><span data-stu-id="b24c2-645">Consider an application that consists of two separate programs:</span></span>
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

<span data-ttu-id="b24c2-646">`Program1` 和 `Program2` 命名空间表示单独编译的两个程序。</span><span class="sxs-lookup"><span data-stu-id="b24c2-646">The `Program1` and `Program2` namespaces denote two programs that are compiled separately.</span></span> <span data-ttu-id="b24c2-647">由于 `Program1.Utils.X` 声明为静态只读字段，因此 `Console.WriteLine` 语句输出的值在编译时是未知的，而是在运行时获取。</span><span class="sxs-lookup"><span data-stu-id="b24c2-647">Because `Program1.Utils.X` is declared as a static readonly field, the value output by the `Console.WriteLine` statement is not known at compile-time, but rather is obtained at run-time.</span></span> <span data-ttu-id="b24c2-648">因此，如果更改了 `X` 的值并重新编译了 `Program1`，则 `Console.WriteLine` 语句将输出新值，即使未重新编译 `Program2` 也是如此。</span><span class="sxs-lookup"><span data-stu-id="b24c2-648">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` isn't recompiled.</span></span> <span data-ttu-id="b24c2-649">但是，`X` 是一个常量，则 `X` 的值将在编译 `Program2` 时获得，并将不受 `Program1` 中的更改的影响，直到重新编译 `Program2` 为止。</span><span class="sxs-lookup"><span data-stu-id="b24c2-649">However, had `X` been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would remain unaffected by changes in `Program1` until `Program2` is recompiled.</span></span>

### <a name="volatile-fields"></a><span data-ttu-id="b24c2-650">可变字段</span><span class="sxs-lookup"><span data-stu-id="b24c2-650">Volatile fields</span></span>

<span data-ttu-id="b24c2-651">当*field_declaration*包含 `volatile` 修饰符时，该声明引入的字段是***可变字段***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-651">When a *field_declaration* includes a `volatile` modifier, the fields introduced by that declaration are ***volatile fields***.</span></span>

<span data-ttu-id="b24c2-652">对于非易失性字段，重新排序指令的优化方法可能导致意外且不可预知的结果，这会导致多线程程序在没有同步的情况下（如*lock_statement*提供的字段）访问字段（[lock 语句](statements.md#the-lock-statement)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-652">For non-volatile fields, optimization techniques that reorder instructions can lead to unexpected and unpredictable results in multi-threaded programs that access fields without synchronization such as that provided by the *lock_statement* ([The lock statement](statements.md#the-lock-statement)).</span></span> <span data-ttu-id="b24c2-653">这些优化可以由编译器、运行时系统或硬件执行。</span><span class="sxs-lookup"><span data-stu-id="b24c2-653">These optimizations can be performed by the compiler, by the run-time system, or by hardware.</span></span> <span data-ttu-id="b24c2-654">对于可变字段，此类重新排序优化将受到限制：</span><span class="sxs-lookup"><span data-stu-id="b24c2-654">For volatile fields, such reordering optimizations are restricted:</span></span>

*  <span data-ttu-id="b24c2-655">读取可变字段称为***易失性读取***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-655">A read of a volatile field is called a ***volatile read***.</span></span> <span data-ttu-id="b24c2-656">可变读取具有 "获取语义";也就是说，保证在对指令序列中出现其后的内存进行引用之前发生。</span><span class="sxs-lookup"><span data-stu-id="b24c2-656">A volatile read has "acquire semantics"; that is, it is guaranteed to occur prior to any references to memory that occur after it in the instruction sequence.</span></span>
*  <span data-ttu-id="b24c2-657">可变字段的写入称为***可变写入***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-657">A write of a volatile field is called a ***volatile write***.</span></span> <span data-ttu-id="b24c2-658">可变写入具有 "发布语义";也就是说，保证在指令序列中写入指令之前的任何内存引用后发生。</span><span class="sxs-lookup"><span data-stu-id="b24c2-658">A volatile write has "release semantics"; that is, it is guaranteed to happen after any memory references prior to the write instruction in the instruction sequence.</span></span>

<span data-ttu-id="b24c2-659">这些限制确保所有线程观察易失性写入操作（由任何其他线程执行）时的观察顺序与写入操作的执行顺序一致。</span><span class="sxs-lookup"><span data-stu-id="b24c2-659">These restrictions ensure that all threads will observe volatile writes performed by any other thread in the order in which they were performed.</span></span> <span data-ttu-id="b24c2-660">一致性实现不需要提供可变写入的单个总体顺序，如所有执行线程中所示。</span><span class="sxs-lookup"><span data-stu-id="b24c2-660">A conforming implementation is not required to provide a single total ordering of volatile writes as seen from all threads of execution.</span></span> <span data-ttu-id="b24c2-661">可变字段的类型必须是以下类型之一：</span><span class="sxs-lookup"><span data-stu-id="b24c2-661">The type of a volatile field must be one of the following:</span></span>

*  <span data-ttu-id="b24c2-662">一个*reference_type*。</span><span class="sxs-lookup"><span data-stu-id="b24c2-662">A *reference_type*.</span></span>
*  <span data-ttu-id="b24c2-663">类型 `byte`、`sbyte`、`short`、`ushort`、`int`、`uint`、`char`、`float`、`bool`、`System.IntPtr`或 `System.UIntPtr`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-663">The type `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, or `System.UIntPtr`.</span></span>
*  <span data-ttu-id="b24c2-664">具有 `byte`、`sbyte`、`short`、`ushort`、`int`或 `uint`的枚举基类型的*enum_type* 。</span><span class="sxs-lookup"><span data-stu-id="b24c2-664">An *enum_type* having an enum base type of `byte`, `sbyte`, `short`, `ushort`, `int`, or `uint`.</span></span>

<span data-ttu-id="b24c2-665">示例</span><span class="sxs-lookup"><span data-stu-id="b24c2-665">The example</span></span>
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
<span data-ttu-id="b24c2-666">生成输出：</span><span class="sxs-lookup"><span data-stu-id="b24c2-666">produces the output:</span></span>
```console
result = 143
```

<span data-ttu-id="b24c2-667">在此示例中，方法 `Main` 启动一个新线程，该线程运行 `Thread2`的方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-667">In this example, the method `Main` starts a new thread that runs the method `Thread2`.</span></span> <span data-ttu-id="b24c2-668">此方法将值存储到名为 `result`的非易失性字段中，然后将 `true` 存储在可变字段 `finished`中。</span><span class="sxs-lookup"><span data-stu-id="b24c2-668">This method stores a value into a non-volatile field called `result`, then stores `true` in the volatile field `finished`.</span></span> <span data-ttu-id="b24c2-669">主线程等待字段 `finished` 设置为 `true`，然后读取 `result`的字段。</span><span class="sxs-lookup"><span data-stu-id="b24c2-669">The main thread waits for the field `finished` to be set to `true`, then reads the field `result`.</span></span> <span data-ttu-id="b24c2-670">由于已 `volatile`声明 `finished`，因此主线程必须从字段 `result`读取值 `143`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-670">Since `finished` has been declared `volatile`, the main thread must read the value `143` from the field `result`.</span></span> <span data-ttu-id="b24c2-671">如果未 `volatile`声明字段 `finished`，则允许存储区 `result` 在存储到 `finished`后对主线程可见，因此，主线程将从该字段 `0` 中读取值 `result`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-671">If the field `finished` had not been declared `volatile`, then it would be permissible for the store to `result` to be visible to the main thread after the store to `finished`, and hence for the main thread to read the value `0` from the field `result`.</span></span> <span data-ttu-id="b24c2-672">将 `finished` 声明为 `volatile` 字段可防止任何此类不一致。</span><span class="sxs-lookup"><span data-stu-id="b24c2-672">Declaring `finished` as a `volatile` field prevents any such inconsistency.</span></span>

### <a name="field-initialization"></a><span data-ttu-id="b24c2-673">字段初始化</span><span class="sxs-lookup"><span data-stu-id="b24c2-673">Field initialization</span></span>

<span data-ttu-id="b24c2-674">字段的初始值，无论是静态字段还是实例字段，都是字段的类型的默认值（[默认值](variables.md#default-values)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-674">The initial value of a field, whether it be a static field or an instance field, is the default value ([Default values](variables.md#default-values)) of the field's type.</span></span> <span data-ttu-id="b24c2-675">在发生此默认初始化之前，不可能观察字段的值，并且字段永远不会 "未初始化"。</span><span class="sxs-lookup"><span data-stu-id="b24c2-675">It is not possible to observe the value of a field before this default initialization has occurred, and a field is thus never "uninitialized".</span></span> <span data-ttu-id="b24c2-676">示例</span><span class="sxs-lookup"><span data-stu-id="b24c2-676">The example</span></span>
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
<span data-ttu-id="b24c2-677">生成输出</span><span class="sxs-lookup"><span data-stu-id="b24c2-677">produces the output</span></span>
```console
b = False, i = 0
```
<span data-ttu-id="b24c2-678">因为 `b` 和 `i` 都自动初始化为默认值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-678">because `b` and `i` are both automatically initialized to default values.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="b24c2-679">变量初始值设定项</span><span class="sxs-lookup"><span data-stu-id="b24c2-679">Variable initializers</span></span>

<span data-ttu-id="b24c2-680">字段声明可能包括*variable_initializer*。</span><span class="sxs-lookup"><span data-stu-id="b24c2-680">Field declarations may include *variable_initializer*s.</span></span> <span data-ttu-id="b24c2-681">对于静态字段，变量初始值设定项对应于在类初始化过程中执行的赋值语句。</span><span class="sxs-lookup"><span data-stu-id="b24c2-681">For static fields, variable initializers correspond to assignment statements that are executed during class initialization.</span></span> <span data-ttu-id="b24c2-682">对于实例字段，变量初始值设定项对应于创建类的实例时执行的赋值语句。</span><span class="sxs-lookup"><span data-stu-id="b24c2-682">For instance fields, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span>

<span data-ttu-id="b24c2-683">示例</span><span class="sxs-lookup"><span data-stu-id="b24c2-683">The example</span></span>
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
<span data-ttu-id="b24c2-684">生成输出</span><span class="sxs-lookup"><span data-stu-id="b24c2-684">produces the output</span></span>
```console
x = 1.4142135623731, i = 100, s = Hello
```
<span data-ttu-id="b24c2-685">由于在执行实例字段初始值设定项时，会执行静态字段初始值设定项并分配给 `i` 并 `s`，因此，对 `x` 的赋值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-685">because an assignment to `x` occurs when static field initializers execute and assignments to `i` and `s` occur when the instance field initializers execute.</span></span>

<span data-ttu-id="b24c2-686">所有字段都将发生[字段初始化](classes.md#field-initialization)中所述的默认值初始化，其中包括具有变量初始值设定项的字段。</span><span class="sxs-lookup"><span data-stu-id="b24c2-686">The default value initialization described in [Field initialization](classes.md#field-initialization) occurs for all fields, including fields that have variable initializers.</span></span> <span data-ttu-id="b24c2-687">因此，初始化某个类时，该类中的所有静态字段都首先初始化为其默认值，然后以文本顺序执行静态字段初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="b24c2-687">Thus, when a class is initialized, all static fields in that class are first initialized to their default values, and then the static field initializers are executed in textual order.</span></span> <span data-ttu-id="b24c2-688">同样，在创建类的实例时，该实例中的所有实例字段都将首先初始化为其默认值，然后以文本顺序执行实例字段初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="b24c2-688">Likewise, when an instance of a class is created, all instance fields in that instance are first initialized to their default values, and then the instance field initializers are executed in textual order.</span></span>

<span data-ttu-id="b24c2-689">具有变量初始值设定项的静态字段可以在其默认值状态中观察到。</span><span class="sxs-lookup"><span data-stu-id="b24c2-689">It is possible for static fields with variable initializers to be observed in their default value state.</span></span> <span data-ttu-id="b24c2-690">不过，强烈建议不要使用这种样式。</span><span class="sxs-lookup"><span data-stu-id="b24c2-690">However, this is strongly discouraged as a matter of style.</span></span> <span data-ttu-id="b24c2-691">示例</span><span class="sxs-lookup"><span data-stu-id="b24c2-691">The example</span></span>
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
<span data-ttu-id="b24c2-692">展示了这种行为。</span><span class="sxs-lookup"><span data-stu-id="b24c2-692">exhibits this behavior.</span></span> <span data-ttu-id="b24c2-693">尽管 a 和 b 的循环定义，但程序有效。</span><span class="sxs-lookup"><span data-stu-id="b24c2-693">Despite the circular definitions of a and b, the program is valid.</span></span> <span data-ttu-id="b24c2-694">这会导致输出</span><span class="sxs-lookup"><span data-stu-id="b24c2-694">It results in the output</span></span>
```console
a = 1, b = 2
```
<span data-ttu-id="b24c2-695">由于静态字段 `a` 和 `b` 会在其初始值设定项执行之前初始化为 `0` （`int`的默认值）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-695">because the static fields `a` and `b` are initialized to `0` (the default value for `int`) before their initializers are executed.</span></span> <span data-ttu-id="b24c2-696">当 `a` 的初始值设定项运行时，`b` 的值为零，因此 `a` 初始化为 `1`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-696">When the initializer for `a` runs, the value of `b` is zero, and so `a` is initialized to `1`.</span></span> <span data-ttu-id="b24c2-697">当 `b` 的初始值设定项运行时，`a` 的值已 `1`，因此 `b` 初始化为 `2`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-697">When the initializer for `b` runs, the value of `a` is already `1`, and so `b` is initialized to `2`.</span></span>

#### <a name="static-field-initialization"></a><span data-ttu-id="b24c2-698">静态字段初始化</span><span class="sxs-lookup"><span data-stu-id="b24c2-698">Static field initialization</span></span>

<span data-ttu-id="b24c2-699">类的静态字段变量初始值设定项对应于按其在类声明中出现的文本顺序执行的一系列赋值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-699">The static field variable initializers of a class correspond to a sequence of assignments that are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="b24c2-700">如果类中存在静态构造函数（[静态](classes.md#static-constructors)构造函数），则在执行该静态构造函数之前，会立即执行静态字段初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="b24c2-700">If a static constructor ([Static constructors](classes.md#static-constructors)) exists in the class, execution of the static field initializers occurs immediately prior to executing that static constructor.</span></span> <span data-ttu-id="b24c2-701">否则，在首次使用该类的静态字段之前，将在依赖于实现的时间执行静态字段初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="b24c2-701">Otherwise, the static field initializers are executed at an implementation-dependent time prior to the first use of a static field of that class.</span></span> <span data-ttu-id="b24c2-702">示例</span><span class="sxs-lookup"><span data-stu-id="b24c2-702">The example</span></span>
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
<span data-ttu-id="b24c2-703">可能产生以下输出：</span><span class="sxs-lookup"><span data-stu-id="b24c2-703">might produce either the output:</span></span>
```console
Init A
Init B
1 1
```
<span data-ttu-id="b24c2-704">或输出：</span><span class="sxs-lookup"><span data-stu-id="b24c2-704">or the output:</span></span>
```console
Init B
Init A
1 1
```
<span data-ttu-id="b24c2-705">由于 `X`的初始值设定项和 `Y`的初始值设定项的执行可能会按以下顺序发生：它们只会在对这些字段的引用之前发生。</span><span class="sxs-lookup"><span data-stu-id="b24c2-705">because the execution of `X`'s initializer and `Y`'s initializer could occur in either order; they are only constrained to occur before the references to those fields.</span></span> <span data-ttu-id="b24c2-706">但在示例中：</span><span class="sxs-lookup"><span data-stu-id="b24c2-706">However, in the example:</span></span>
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
<span data-ttu-id="b24c2-707">输出必须为：</span><span class="sxs-lookup"><span data-stu-id="b24c2-707">the output must be:</span></span>
```console
Init B
Init A
1 1
```
<span data-ttu-id="b24c2-708">由于静态构造函数的执行规则（在[静态构造函数](classes.md#static-constructors)中定义），因此必须在 `A`的静态构造函数和字段初始值设定项之前运行 `B`的静态构造函数（因此 `B`的静态字段初始值设定项）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-708">because the rules for when static constructors execute (as defined in [Static constructors](classes.md#static-constructors)) provide that `B`'s static constructor (and hence `B`'s static field initializers) must run before `A`'s static constructor and field initializers.</span></span>

#### <a name="instance-field-initialization"></a><span data-ttu-id="b24c2-709">实例字段初始化</span><span class="sxs-lookup"><span data-stu-id="b24c2-709">Instance field initialization</span></span>

<span data-ttu-id="b24c2-710">类的实例字段变量初始值设定项对应于进入该类的任何一个实例构造函数（[构造函数初始值设定](classes.md#constructor-initializers)项）后立即执行的一系列赋值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-710">The instance field variable initializers of a class correspond to a sequence of assignments that are executed immediately upon entry to any one of the instance constructors ([Constructor initializers](classes.md#constructor-initializers)) of that class.</span></span> <span data-ttu-id="b24c2-711">变量初始值设定项将按照它们在类声明中的显示顺序执行。</span><span class="sxs-lookup"><span data-stu-id="b24c2-711">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="b24c2-712">[实例构造函数](classes.md#instance-constructors)中对类实例创建和初始化过程进行了进一步说明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-712">The class instance creation and initialization process is described further in [Instance constructors](classes.md#instance-constructors).</span></span>

<span data-ttu-id="b24c2-713">实例字段的变量初始值设定项不能引用正在创建的实例。</span><span class="sxs-lookup"><span data-stu-id="b24c2-713">A variable initializer for an instance field cannot reference the instance being created.</span></span> <span data-ttu-id="b24c2-714">因此，在变量初始值设定项中引用 `this` 是编译时错误，因为变量初始值设定项通过*simple_name*引用任何实例成员时出现编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-714">Thus, it is a compile-time error to reference `this` in a variable initializer, as it is a compile-time error for a variable initializer to reference any instance member through a *simple_name*.</span></span> <span data-ttu-id="b24c2-715">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-715">In the example</span></span>
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
<span data-ttu-id="b24c2-716">`y` 的变量初始值设定项会导致编译时错误，因为它引用正在创建的实例的成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-716">the variable initializer for `y` results in a compile-time error because it references a member of the instance being created.</span></span>

## <a name="methods"></a><span data-ttu-id="b24c2-717">方法</span><span class="sxs-lookup"><span data-stu-id="b24c2-717">Methods</span></span>

<span data-ttu-id="b24c2-718">***方法***是实现对象或类可执行的计算或操作的成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-718">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="b24c2-719">方法使用*method_declaration*s 来声明：</span><span class="sxs-lookup"><span data-stu-id="b24c2-719">Methods are declared using *method_declaration*s:</span></span>

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

<span data-ttu-id="b24c2-720">*Method_declaration*可以包含一组*属性*（[特性](attributes.md)）和四个访问修饰符（[访问修饰符](classes.md#access-modifiers)）、`new` （[新修饰符](classes.md#the-new-modifier)）、`static` （[静态和实例方法](classes.md#static-and-instance-methods)）、`virtual` （[虚拟方法](classes.md#virtual-methods)）、`override` （[重写方法](classes.md#override-methods)）、`sealed` （[密封方法](classes.md#sealed-methods)）、`abstract` （[抽象方法](classes.md#abstract-methods)）和 `extern` （[外部方法](classes.md#external-methods)）修饰符的有效组合。</span><span class="sxs-lookup"><span data-stu-id="b24c2-720">A *method_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="b24c2-721">如果满足以下所有条件，则声明具有有效的修饰符组合：</span><span class="sxs-lookup"><span data-stu-id="b24c2-721">A declaration has a valid combination of modifiers if all of the following are true:</span></span>

*  <span data-ttu-id="b24c2-722">声明中包含有效的访问修饰符（[访问修饰符](classes.md#access-modifiers)）组合。</span><span class="sxs-lookup"><span data-stu-id="b24c2-722">The declaration includes a valid combination of access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span>
*  <span data-ttu-id="b24c2-723">声明不包含同一修饰符多次。</span><span class="sxs-lookup"><span data-stu-id="b24c2-723">The declaration does not include the same modifier multiple times.</span></span>
*  <span data-ttu-id="b24c2-724">声明最多包含以下修饰符之一： `static`、`virtual`和 `override`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-724">The declaration includes at most one of the following modifiers: `static`, `virtual`, and `override`.</span></span>
*  <span data-ttu-id="b24c2-725">声明最多包含以下修饰符之一： `new` 和 `override`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-725">The declaration includes at most one of the following modifiers: `new` and `override`.</span></span>
*  <span data-ttu-id="b24c2-726">如果声明包含 `abstract` 修饰符，则声明不包括以下任何修饰符： `static`、`virtual`、`sealed` 或 `extern`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-726">If the declaration includes the `abstract` modifier, then the declaration does not include any of the following modifiers: `static`, `virtual`, `sealed` or `extern`.</span></span>
*  <span data-ttu-id="b24c2-727">如果声明包含 `private` 修饰符，则声明不包括以下任何修饰符： `virtual`、`override`或 `abstract`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-727">If the declaration includes the `private` modifier, then the declaration does not include any of the following modifiers: `virtual`, `override`, or `abstract`.</span></span>
*  <span data-ttu-id="b24c2-728">如果声明包含 `sealed` 修饰符，则声明还包括 `override` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-728">If the declaration includes the `sealed` modifier, then the declaration also includes the `override` modifier.</span></span>
*  <span data-ttu-id="b24c2-729">如果声明包含 `partial` 修饰符，则不包含以下任何修饰符： `new`、`public`、`protected`、`internal`、`private`、`virtual`、`sealed`、`override`、`abstract`或 `extern`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-729">If the declaration includes the `partial` modifier, then it does not include any of the following modifiers: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`, or `extern`.</span></span>

<span data-ttu-id="b24c2-730">具有 `async` 修饰符的方法是一个异步函数，并遵循[异步函数](classes.md#async-functions)中所述的规则。</span><span class="sxs-lookup"><span data-stu-id="b24c2-730">A method that has the `async` modifier is an async function and follows the rules described in [Async functions](classes.md#async-functions).</span></span>

<span data-ttu-id="b24c2-731">方法声明的*return_type*指定由方法计算并返回的值的类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-731">The *return_type* of a method declaration specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="b24c2-732">如果方法不返回值，则 `void` *return_type* 。</span><span class="sxs-lookup"><span data-stu-id="b24c2-732">The *return_type* is `void` if the method does not return a value.</span></span> <span data-ttu-id="b24c2-733">如果声明包含 `partial` 修饰符，则返回类型必须为 `void`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-733">If the declaration includes the `partial` modifier, then the return type must be `void`.</span></span>

<span data-ttu-id="b24c2-734">*Member_name*指定方法的名称。</span><span class="sxs-lookup"><span data-stu-id="b24c2-734">The *member_name* specifies the name of the method.</span></span> <span data-ttu-id="b24c2-735">除非此方法是显式接口成员实现（[显式接口成员实现](interfaces.md#explicit-interface-member-implementations)），否则*member_name*只是一个*标识符*。</span><span class="sxs-lookup"><span data-stu-id="b24c2-735">Unless the method is an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="b24c2-736">对于显式接口成员实现， *member_name*由*interface_type*后跟 "`.`" 和*标识符*组成。</span><span class="sxs-lookup"><span data-stu-id="b24c2-736">For an explicit interface member implementation, the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="b24c2-737">可选*type_parameter_list*指定方法的类型参数（[类型参数](classes.md#type-parameters)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-737">The optional *type_parameter_list* specifies the type parameters of the method ([Type parameters](classes.md#type-parameters)).</span></span> <span data-ttu-id="b24c2-738">如果指定了*type_parameter_list* ，则该方法是***泛型方法***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-738">If a *type_parameter_list* is specified the method is a ***generic method***.</span></span> <span data-ttu-id="b24c2-739">如果该方法具有 `extern` 修饰符，则无法指定*type_parameter_list* 。</span><span class="sxs-lookup"><span data-stu-id="b24c2-739">If the method has an `extern` modifier, a *type_parameter_list* cannot be specified.</span></span>

<span data-ttu-id="b24c2-740">可选*formal_parameter_list*指定方法的参数（[方法参数](classes.md#method-parameters)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-740">The optional *formal_parameter_list* specifies the parameters of the method ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="b24c2-741">可选*type_parameter_constraints_clause*指定对单个类型参数的约束（[类型参数约束](classes.md#type-parameter-constraints)），并且仅当还提供了*type_parameter_list*并且方法没有 `override` 修饰符时，才可以指定。</span><span class="sxs-lookup"><span data-stu-id="b24c2-741">The optional *type_parameter_constraints_clause*s specify constraints on individual type parameters ([Type parameter constraints](classes.md#type-parameter-constraints)) and may only be specified if a *type_parameter_list* is also supplied, and the method does not have an `override` modifier.</span></span>

<span data-ttu-id="b24c2-742">在方法的*formal_parameter_list*中引用的*return_type*和每个类型必须至少具有与方法本身相同的可访问性（[辅助功能约束](basic-concepts.md#accessibility-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-742">The *return_type* and each of the types referenced in the *formal_parameter_list* of a method must be at least as accessible as the method itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="b24c2-743">*Method_body*为分号、***语句体***或***表达式主体***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-743">The *method_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="b24c2-744">语句体由*块*组成，它指定在调用方法时要执行的语句。</span><span class="sxs-lookup"><span data-stu-id="b24c2-744">A statement body consists of a *block*, which specifies the statements to execute when the method is invoked.</span></span> <span data-ttu-id="b24c2-745">表达式主体由 `=>` 后跟*表达式*和分号组成，并表示在调用方法时要执行的单个表达式。</span><span class="sxs-lookup"><span data-stu-id="b24c2-745">An expression body consists of `=>` followed by an *expression* and a semicolon, and denotes a single expression to perform when the method is invoked.</span></span> 

<span data-ttu-id="b24c2-746">对于 `abstract` 和 `extern` 方法， *method_body*只包含一个分号。</span><span class="sxs-lookup"><span data-stu-id="b24c2-746">For `abstract` and `extern` methods, the *method_body* consists simply of a semicolon.</span></span> <span data-ttu-id="b24c2-747">对于 `partial` 方法， *method_body*可能包含分号、块体或表达式主体。</span><span class="sxs-lookup"><span data-stu-id="b24c2-747">For `partial` methods the *method_body* may consist of either a semicolon, a block body or an expression body.</span></span> <span data-ttu-id="b24c2-748">对于所有其他方法， *method_body*为块体或表达式主体。</span><span class="sxs-lookup"><span data-stu-id="b24c2-748">For all other methods, the *method_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="b24c2-749">如果*method_body*包含分号，则声明可能不包括 `async` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-749">If the *method_body* consists of a semicolon, then the declaration may not include the `async` modifier.</span></span>

<span data-ttu-id="b24c2-750">方法的名称、类型参数列表和形参列表定义方法的签名（[签名和重载](basic-concepts.md#signatures-and-overloading)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-750">The name, the type parameter list and the formal parameter list of a method define the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the method.</span></span> <span data-ttu-id="b24c2-751">具体而言，方法的签名由其名称、类型参数的数目以及其形参的数量、修饰符和类型组成。</span><span class="sxs-lookup"><span data-stu-id="b24c2-751">Specifically, the signature of a method consists of its name, the number of type parameters and the number, modifiers, and types of its formal parameters.</span></span> <span data-ttu-id="b24c2-752">出于这些目的，形参的类型中出现的方法的任何类型形参均由其名称标识，但其在方法的类型实参列表中的序号位置。返回类型不是方法签名的一部分，也不是类型参数或形参的名称。</span><span class="sxs-lookup"><span data-stu-id="b24c2-752">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.The return type is not part of a method's signature, nor are the names of the type parameters or the formal parameters.</span></span>

<span data-ttu-id="b24c2-753">方法的名称必须不同于同一个类中声明的所有其他非方法的名称。</span><span class="sxs-lookup"><span data-stu-id="b24c2-753">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="b24c2-754">此外，方法的签名必须不同于同一个类中声明的所有其他方法的签名，同一类中声明的两个方法的签名可能不只是 `ref` 和 `out`不同。</span><span class="sxs-lookup"><span data-stu-id="b24c2-754">In addition, the signature of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>

<span data-ttu-id="b24c2-755">此方法的*type_parameter*在整个*method_declaration*的范围内，可用于在*return_type*、 *method_body*和*type_parameter_constraints_clause*中的整个范围内形成类型，而不是在*属性*中。</span><span class="sxs-lookup"><span data-stu-id="b24c2-755">The method's *type_parameter*s are in scope throughout the *method_declaration*, and can be used to form types throughout that scope in *return_type*, *method_body*, and *type_parameter_constraints_clause*s but not in *attributes*.</span></span>

<span data-ttu-id="b24c2-756">所有形参和类型形参必须具有不同的名称。</span><span class="sxs-lookup"><span data-stu-id="b24c2-756">All formal parameters and type parameters must have different names.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="b24c2-757">方法参数</span><span class="sxs-lookup"><span data-stu-id="b24c2-757">Method parameters</span></span>

<span data-ttu-id="b24c2-758">方法的参数（如果有）由方法的*formal_parameter_list*声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-758">The parameters of a method, if any, are declared by the method's *formal_parameter_list*.</span></span>

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

<span data-ttu-id="b24c2-759">形参列表包含一个或多个以逗号分隔的参数，其中只有最后一个参数可以是*parameter_array*。</span><span class="sxs-lookup"><span data-stu-id="b24c2-759">The formal parameter list consists of one or more comma-separated parameters of which only the last may be a *parameter_array*.</span></span>

<span data-ttu-id="b24c2-760">*Fixed_parameter*包含一组可选的*特性*（[特性](attributes.md)）、可选的 `ref`、`out` 或 `this` 修饰符、*类型*、*标识符*和可选的*default_argument*。</span><span class="sxs-lookup"><span data-stu-id="b24c2-760">A *fixed_parameter* consists of an optional set of *attributes* ([Attributes](attributes.md)), an optional `ref`, `out` or `this` modifier, a *type*, an *identifier* and an optional *default_argument*.</span></span> <span data-ttu-id="b24c2-761">每个*fixed_parameter*都声明具有给定名称的给定类型的参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-761">Each *fixed_parameter* declares a parameter of the given type with the given name.</span></span> <span data-ttu-id="b24c2-762">`this` 修饰符将方法指定为扩展方法，并且只允许在静态方法的第一个参数上使用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-762">The `this` modifier designates the method as an extension method and is only allowed on the first parameter of a static method.</span></span> <span data-ttu-id="b24c2-763">[扩展方法中进一步](classes.md#extension-methods)介绍了扩展方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-763">Extension methods are further described in [Extension methods](classes.md#extension-methods).</span></span>

<span data-ttu-id="b24c2-764">使用*default_argument*的*fixed_parameter*称为***可选参数***，而没有*default_argument*的*fixed_parameter*是***必需参数***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-764">A *fixed_parameter* with a *default_argument* is known as an ***optional parameter***, whereas a *fixed_parameter* without a *default_argument* is a ***required parameter***.</span></span> <span data-ttu-id="b24c2-765">必需的参数不能出现在*formal_parameter_list*中的可选参数之后。</span><span class="sxs-lookup"><span data-stu-id="b24c2-765">A required parameter may not appear after an optional parameter in a *formal_parameter_list*.</span></span>

<span data-ttu-id="b24c2-766">`ref` 或 `out` 参数不能具有*default_argument*。</span><span class="sxs-lookup"><span data-stu-id="b24c2-766">A `ref` or `out` parameter cannot have a *default_argument*.</span></span> <span data-ttu-id="b24c2-767">*Default_argument*中的*表达式*必须是下列其中一项：</span><span class="sxs-lookup"><span data-stu-id="b24c2-767">The *expression* in a *default_argument* must be one of the following:</span></span>

*  <span data-ttu-id="b24c2-768">一个*constant_expression*</span><span class="sxs-lookup"><span data-stu-id="b24c2-768">a *constant_expression*</span></span>
*  <span data-ttu-id="b24c2-769">格式 `new S()` 其中 `S` 为值类型的表达式</span><span class="sxs-lookup"><span data-stu-id="b24c2-769">an expression of the form `new S()` where `S` is a value type</span></span>
*  <span data-ttu-id="b24c2-770">格式 `default(S)` 其中 `S` 为值类型的表达式</span><span class="sxs-lookup"><span data-stu-id="b24c2-770">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="b24c2-771">*表达式*必须可通过标识或可为 null 的类型转换为参数的类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-771">The *expression* must be implicitly convertible by an identity or nullable conversion to the type of the parameter.</span></span>

<span data-ttu-id="b24c2-772">如果在实现分部方法声明（[分部方法](classes.md#partial-methods)）中出现可选参数，则显式接口成员实现（[显式接口成员实现](interfaces.md#explicit-interface-member-implementations)），或在单参数索引器声明（[索引器](classes.md#indexers)）中出现警告，因为这些成员永远无法通过允许省略参数的方式进行调用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-772">If optional parameters occur in an implementing partial method declaration ([Partial methods](classes.md#partial-methods)) , an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)) or in a single-parameter indexer declaration ([Indexers](classes.md#indexers)) the compiler should give a warning, since these members can never be invoked in a way that permits arguments to be omitted.</span></span>

<span data-ttu-id="b24c2-773">*Parameter_array*包含一组可选的*特性*（[特性](attributes.md)）、`params` 修饰符、 *array_type*和*标识符*。</span><span class="sxs-lookup"><span data-stu-id="b24c2-773">A *parameter_array* consists of an optional set of *attributes* ([Attributes](attributes.md)), a `params` modifier, an *array_type*, and an *identifier*.</span></span> <span data-ttu-id="b24c2-774">参数数组声明具有给定名称的给定数组类型的单一参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-774">A parameter array declares a single parameter of the given array type with the given name.</span></span> <span data-ttu-id="b24c2-775">参数数组的*array_type*必须为一维数组类型（[数组类型](arrays.md#array-types)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-775">The *array_type* of a parameter array must be a single-dimensional array type ([Array types](arrays.md#array-types)).</span></span> <span data-ttu-id="b24c2-776">在方法调用中，参数数组允许指定给定数组类型的单个自变量，或允许指定数组元素类型的零个或多个参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-776">In a method invocation, a parameter array permits either a single argument of the given array type to be specified, or it permits zero or more arguments of the array element type to be specified.</span></span> <span data-ttu-id="b24c2-777">参数数组中进一步介绍了参数[数组。](classes.md#parameter-arrays)</span><span class="sxs-lookup"><span data-stu-id="b24c2-777">Parameter arrays are described further in [Parameter arrays](classes.md#parameter-arrays).</span></span>

<span data-ttu-id="b24c2-778">*Parameter_array*可能会在可选参数之后发生，但不能具有默认值，而是省略*parameter_array*的参数会导致创建一个空数组。</span><span class="sxs-lookup"><span data-stu-id="b24c2-778">A *parameter_array* may occur after an optional parameter, but cannot have a default value -- the omission of arguments for a *parameter_array* would instead result in the creation of an empty array.</span></span>

<span data-ttu-id="b24c2-779">下面的示例演示了不同类型的参数：</span><span class="sxs-lookup"><span data-stu-id="b24c2-779">The following example illustrates different kinds of parameters:</span></span>
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

<span data-ttu-id="b24c2-780">在 `M`的*formal_parameter_list*中，`i` 是必需的 ref 参数，`d` 是必需的值参数，`b`、`s`、`o` 和 `t` 是可选值参数，`a` 是参数数组。</span><span class="sxs-lookup"><span data-stu-id="b24c2-780">In the *formal_parameter_list* for `M`, `i` is a required ref parameter, `d` is a required value parameter, `b`, `s`, `o` and `t` are optional value parameters and `a` is a parameter array.</span></span>

<span data-ttu-id="b24c2-781">方法声明为参数、类型参数和局部变量创建了单独的声明空间。</span><span class="sxs-lookup"><span data-stu-id="b24c2-781">A method declaration creates a separate declaration space for parameters, type parameters and local variables.</span></span> <span data-ttu-id="b24c2-782">名称通过方法的类型形参列表和形参列表以及方法*块*中的局部变量声明引入此声明空间。</span><span class="sxs-lookup"><span data-stu-id="b24c2-782">Names are introduced into this declaration space by the type parameter list and the formal parameter list of the method and by local variable declarations in the *block* of the method.</span></span> <span data-ttu-id="b24c2-783">方法声明空间的两个成员具有相同的名称是错误的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-783">It is an error for two members of a method declaration space to have the same name.</span></span> <span data-ttu-id="b24c2-784">如果嵌套声明空间的方法声明空间和局部变量声明空间包含具有相同名称的元素，则是错误的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-784">It is an error for the method declaration space and the local variable declaration space of a nested declaration space to contain elements with the same name.</span></span>

<span data-ttu-id="b24c2-785">方法调用（[方法调用](expressions.md#method-invocations)）创建特定于该方法的形参和局部变量的副本，并且调用的参数列表将值或变量引用分配给新创建的形参。</span><span class="sxs-lookup"><span data-stu-id="b24c2-785">A method invocation ([Method invocations](expressions.md#method-invocations)) creates a copy, specific to that invocation, of the formal parameters and local variables of the method, and the argument list of the invocation assigns values or variable references to the newly created formal parameters.</span></span> <span data-ttu-id="b24c2-786">在方法的*块*中，可以通过参数*Simple_name*表达式（[简单名称](expressions.md#simple-names)）中的标识符引用形参。</span><span class="sxs-lookup"><span data-stu-id="b24c2-786">Within the *block* of a method, formal parameters can be referenced by their identifiers in *simple_name* expressions ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="b24c2-787">有四种形参：</span><span class="sxs-lookup"><span data-stu-id="b24c2-787">There are four kinds of formal parameters:</span></span>

*  <span data-ttu-id="b24c2-788">值参数，在没有任何修饰符的情况下声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-788">Value parameters, which are declared without any modifiers.</span></span>
*  <span data-ttu-id="b24c2-789">引用参数，它们是用 `ref` 修饰符声明的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-789">Reference parameters, which are declared with the `ref` modifier.</span></span>
*  <span data-ttu-id="b24c2-790">输出参数，它们是用 `out` 修饰符声明的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-790">Output parameters, which are declared with the `out` modifier.</span></span>
*  <span data-ttu-id="b24c2-791">参数数组，用 `params` 修饰符声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-791">Parameter arrays, which are declared with the `params` modifier.</span></span>

<span data-ttu-id="b24c2-792">如[签名和重载](basic-concepts.md#signatures-and-overloading)中所述，`ref` 和 `out` 修饰符是方法签名的一部分，但是 `params` 修饰符不是。</span><span class="sxs-lookup"><span data-stu-id="b24c2-792">As described in [Signatures and overloading](basic-concepts.md#signatures-and-overloading), the `ref` and `out` modifiers are part of a method's signature, but the `params` modifier is not.</span></span>

#### <a name="value-parameters"></a><span data-ttu-id="b24c2-793">值参数</span><span class="sxs-lookup"><span data-stu-id="b24c2-793">Value parameters</span></span>

<span data-ttu-id="b24c2-794">使用不带修饰符声明的参数是一个值参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-794">A parameter declared with no modifiers is a value parameter.</span></span> <span data-ttu-id="b24c2-795">值参数对应于一个局部变量，该局部变量从方法调用中提供的相应参数获取其初始值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-795">A value parameter corresponds to a local variable that gets its initial value from the corresponding argument supplied in the method invocation.</span></span>

<span data-ttu-id="b24c2-796">如果形参为值形参，则方法调用中的相应参数必须是可隐[式转换为](conversions.md#implicit-conversions)形参类型的表达式。</span><span class="sxs-lookup"><span data-stu-id="b24c2-796">When a formal parameter is a value parameter, the corresponding argument in a method invocation must be an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the formal parameter type.</span></span>

<span data-ttu-id="b24c2-797">允许方法为值参数赋值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-797">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="b24c2-798">此类分配只会影响 value 参数所表示的本地存储位置，它们对方法调用中给定的实参不起作用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-798">Such assignments only affect the local storage location represented by the value parameter—they have no effect on the actual argument given in the method invocation.</span></span>

#### <a name="reference-parameters"></a><span data-ttu-id="b24c2-799">引用参数</span><span class="sxs-lookup"><span data-stu-id="b24c2-799">Reference parameters</span></span>

<span data-ttu-id="b24c2-800">使用 `ref` 修饰符声明的参数是一个引用参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-800">A parameter declared with a `ref` modifier is a reference parameter.</span></span> <span data-ttu-id="b24c2-801">与值参数不同，引用参数不会创建新的存储位置。</span><span class="sxs-lookup"><span data-stu-id="b24c2-801">Unlike a value parameter, a reference parameter does not create a new storage location.</span></span> <span data-ttu-id="b24c2-802">相反，引用参数表示作为方法调用中的自变量提供的变量所在的存储位置。</span><span class="sxs-lookup"><span data-stu-id="b24c2-802">Instead, a reference parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="b24c2-803">如果形参是引用参数，则方法调用中的相应参数必须包含关键字 `ref` 后跟与形参相同的类型的*variable_reference* （[确定明确赋值的确切规则](variables.md#precise-rules-for-determining-definite-assignment)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-803">When a formal parameter is a reference parameter, the corresponding argument in a method invocation must consist of the keyword `ref` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="b24c2-804">在将变量作为引用参数传递之前，必须对其进行明确赋值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-804">A variable must be definitely assigned before it can be passed as a reference parameter.</span></span>

<span data-ttu-id="b24c2-805">在方法中，引用参数始终被视为明确赋值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-805">Within a method, a reference parameter is always considered definitely assigned.</span></span>

<span data-ttu-id="b24c2-806">声明为迭代器（[迭代](classes.md#iterators)器）的方法不能有引用参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-806">A method declared as an iterator ([Iterators](classes.md#iterators)) cannot have reference parameters.</span></span>

<span data-ttu-id="b24c2-807">示例</span><span class="sxs-lookup"><span data-stu-id="b24c2-807">The example</span></span>
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
<span data-ttu-id="b24c2-808">生成输出</span><span class="sxs-lookup"><span data-stu-id="b24c2-808">produces the output</span></span>
```console
i = 2, j = 1
```

<span data-ttu-id="b24c2-809">对于 `Main`中 `Swap` 的调用，`x` 表示 `i`，`y` 表示 `j`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-809">For the invocation of `Swap` in `Main`, `x` represents `i` and `y` represents `j`.</span></span> <span data-ttu-id="b24c2-810">因此，调用具有交换 `i` 和 `j`的值的效果。</span><span class="sxs-lookup"><span data-stu-id="b24c2-810">Thus, the invocation has the effect of swapping the values of `i` and `j`.</span></span>

<span data-ttu-id="b24c2-811">在采用引用参数的方法中，多个名称可以表示相同的存储位置。</span><span class="sxs-lookup"><span data-stu-id="b24c2-811">In a method that takes reference parameters it is possible for multiple names to represent the same storage location.</span></span> <span data-ttu-id="b24c2-812">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-812">In the example</span></span>
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
<span data-ttu-id="b24c2-813">`G` 中的 `F` 的调用同时为 `a` 和 `b`传递了对 `s` 的引用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-813">the invocation of `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="b24c2-814">因此，对于该调用，名称 `s`、`a`和 `b` 都引用相同的存储位置，三个分配都将修改实例字段 `s`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-814">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance field `s`.</span></span>

#### <a name="output-parameters"></a><span data-ttu-id="b24c2-815">输出参数</span><span class="sxs-lookup"><span data-stu-id="b24c2-815">Output parameters</span></span>

<span data-ttu-id="b24c2-816">使用 `out` 修饰符声明的参数是一个输出参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-816">A parameter declared with an `out` modifier is an output parameter.</span></span> <span data-ttu-id="b24c2-817">与引用参数类似，output 参数不会创建新的存储位置。</span><span class="sxs-lookup"><span data-stu-id="b24c2-817">Similar to a reference parameter, an output parameter does not create a new storage location.</span></span> <span data-ttu-id="b24c2-818">相反，output 参数表示作为方法调用中的自变量提供的变量所在的存储位置。</span><span class="sxs-lookup"><span data-stu-id="b24c2-818">Instead, an output parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="b24c2-819">如果形参为 output 参数，则方法调用中的相应参数必须包含关键字 `out` 后跟与形参相同的类型的*variable_reference* （[确定明确赋值的确切规则](variables.md#precise-rules-for-determining-definite-assignment)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-819">When a formal parameter is an output parameter, the corresponding argument in a method invocation must consist of the keyword `out` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="b24c2-820">在将变量作为 output 参数传递之前，不需要明确赋值，但在将变量作为 output 参数传递的调用之后，该变量被视为明确赋值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-820">A variable need not be definitely assigned before it can be passed as an output parameter, but following an invocation where a variable was passed as an output parameter, the variable is considered definitely assigned.</span></span>

<span data-ttu-id="b24c2-821">在方法中，就像局部变量一样，output 参数最初被视为未分配，在使用其值之前必须进行明确赋值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-821">Within a method, just like a local variable, an output parameter is initially considered unassigned and must be definitely assigned before its value is used.</span></span>

<span data-ttu-id="b24c2-822">方法返回之前，必须为方法的每个输出参数进行明确赋值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-822">Every output parameter of a method must be definitely assigned before the method returns.</span></span>

<span data-ttu-id="b24c2-823">声明为分部方法（[分部方法](classes.md#partial-methods)）或迭代器（[迭代](classes.md#iterators)器）的方法不能有输出参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-823">A method declared as a partial method ([Partial methods](classes.md#partial-methods)) or an iterator ([Iterators](classes.md#iterators)) cannot have output parameters.</span></span>

<span data-ttu-id="b24c2-824">输出参数通常用于生成多个返回值的方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-824">Output parameters are typically used in methods that produce multiple return values.</span></span> <span data-ttu-id="b24c2-825">例如：</span><span class="sxs-lookup"><span data-stu-id="b24c2-825">For example:</span></span>
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

<span data-ttu-id="b24c2-826">该示例生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="b24c2-826">The example produces the output:</span></span>
```console
c:\Windows\System\
hello.txt
```

<span data-ttu-id="b24c2-827">请注意，可以在将 `dir` 和 `name` 变量传递到 `SplitPath`之前取消对它们的分配，并且这些变量在调用后被视为明确赋值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-827">Note that the `dir` and `name` variables can be unassigned before they are passed to `SplitPath`, and that they are considered definitely assigned following the call.</span></span>

#### <a name="parameter-arrays"></a><span data-ttu-id="b24c2-828">参数数组</span><span class="sxs-lookup"><span data-stu-id="b24c2-828">Parameter arrays</span></span>

<span data-ttu-id="b24c2-829">使用 `params` 修饰符声明的参数是一个参数数组。</span><span class="sxs-lookup"><span data-stu-id="b24c2-829">A parameter declared with a `params` modifier is a parameter array.</span></span> <span data-ttu-id="b24c2-830">如果形参表包含参数数组，则它必须是列表中的最后一个形参，并且必须是一维数组类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-830">If a formal parameter list includes a parameter array, it must be the last parameter in the list and it must be of a single-dimensional array type.</span></span> <span data-ttu-id="b24c2-831">例如，`string[]` 和 `string[][]` 的类型可用作参数数组的类型，但类型 `string[,]` 不能。</span><span class="sxs-lookup"><span data-stu-id="b24c2-831">For example, the types `string[]` and `string[][]` can be used as the type of a parameter array, but the type `string[,]` can not.</span></span> <span data-ttu-id="b24c2-832">不能将 `params` 修饰符与修饰符结合 `ref` 和 `out`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-832">It is not possible to combine the `params` modifier with the modifiers `ref` and `out`.</span></span>

<span data-ttu-id="b24c2-833">参数数组允许在方法调用中使用以下两种方法之一指定参数：</span><span class="sxs-lookup"><span data-stu-id="b24c2-833">A parameter array permits arguments to be specified in one of two ways in a method invocation:</span></span>

*  <span data-ttu-id="b24c2-834">为参数数组提供的参数可以是可隐[式转换为](conversions.md#implicit-conversions)参数数组类型的单个表达式。</span><span class="sxs-lookup"><span data-stu-id="b24c2-834">The argument given for a parameter array can be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="b24c2-835">在这种情况下，参数数组的作用与值参数完全相同。</span><span class="sxs-lookup"><span data-stu-id="b24c2-835">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="b24c2-836">此外，调用可以为参数数组指定零个或多个参数，其中每个参数都是可隐[式转换为](conversions.md#implicit-conversions)参数数组元素类型的表达式。</span><span class="sxs-lookup"><span data-stu-id="b24c2-836">Alternatively, the invocation can specify zero or more arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="b24c2-837">在这种情况下，调用会创建一个参数数组类型的实例，该实例的长度与参数的数量相对应，使用给定的参数值初始化数组实例的元素，并使用新创建的数组实例作为实际实际.</span><span class="sxs-lookup"><span data-stu-id="b24c2-837">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="b24c2-838">除了允许在调用中使用可变数量的自变量，参数数组完全等效于同一类型的值参数（[值参数](classes.md#value-parameters)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-838">Except for allowing a variable number of arguments in an invocation, a parameter array is precisely equivalent to a value parameter ([Value parameters](classes.md#value-parameters)) of the same type.</span></span>

<span data-ttu-id="b24c2-839">示例</span><span class="sxs-lookup"><span data-stu-id="b24c2-839">The example</span></span>
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
<span data-ttu-id="b24c2-840">生成输出</span><span class="sxs-lookup"><span data-stu-id="b24c2-840">produces the output</span></span>
```console
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="b24c2-841">`F` 的第一次调用只是将数组作为值参数传递 `a`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-841">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="b24c2-842">第二次调用 `F` 会自动创建包含给定元素值的四元素 `int[]` 并将该数组实例作为值参数进行传递。</span><span class="sxs-lookup"><span data-stu-id="b24c2-842">The second invocation of `F` automatically creates a four-element `int[]` with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="b24c2-843">同样，第三次调用 `F` 会创建一个零元素 `int[]` 并将该实例作为值参数进行传递。</span><span class="sxs-lookup"><span data-stu-id="b24c2-843">Likewise, the third invocation of `F` creates a zero-element `int[]` and passes that instance as a value parameter.</span></span> <span data-ttu-id="b24c2-844">第二次和第三次调用完全等效于写入：</span><span class="sxs-lookup"><span data-stu-id="b24c2-844">The second and third invocations are precisely equivalent to writing:</span></span>
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

<span data-ttu-id="b24c2-845">在执行重载决策时，具有参数数组的方法可能以其普通形式或其展开形式（[适用的函数成员](expressions.md#applicable-function-member)）的形式应用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-845">When performing overload resolution, a method with a parameter array may be applicable either in its normal form or in its expanded form ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="b24c2-846">仅当方法的正常形式不适用，并且仅当与展开的窗体具有相同签名的适用方法未在同一类型中声明时，方法的展开形式才可用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-846">The expanded form of a method is available only if the normal form of the method is not applicable and only if an applicable method with the same signature as the expanded form is not already declared in the same type.</span></span>

<span data-ttu-id="b24c2-847">示例</span><span class="sxs-lookup"><span data-stu-id="b24c2-847">The example</span></span>
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
<span data-ttu-id="b24c2-848">生成输出</span><span class="sxs-lookup"><span data-stu-id="b24c2-848">produces the output</span></span>
```console
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

<span data-ttu-id="b24c2-849">在此示例中，具有参数数组的方法的两个可能的展开形式都已作为常规方法包含在类中。</span><span class="sxs-lookup"><span data-stu-id="b24c2-849">In the example, two of the possible expanded forms of the method with a parameter array are already included in the class as regular methods.</span></span> <span data-ttu-id="b24c2-850">因此，在执行重载决策时不考虑这些扩展窗体，第一个和第三个方法调用会选择常规方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-850">These expanded forms are therefore not considered when performing overload resolution, and the first and third method invocations thus select the regular methods.</span></span> <span data-ttu-id="b24c2-851">当类声明具有参数数组的方法时，也不太常见，还应将一些展开形式包含为常规方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-851">When a class declares a method with a parameter array, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="b24c2-852">这样做可以避免在调用具有参数数组的方法的扩展形式时，分配发生的数组实例。</span><span class="sxs-lookup"><span data-stu-id="b24c2-852">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a parameter array is invoked.</span></span>

<span data-ttu-id="b24c2-853">当参数数组的类型 `object[]`时，方法的正常形式和单个 `object` 参数的所花费形式之间可能存在歧义。</span><span class="sxs-lookup"><span data-stu-id="b24c2-853">When the type of a parameter array is `object[]`, a potential ambiguity arises between the normal form of the method and the expended form for a single `object` parameter.</span></span> <span data-ttu-id="b24c2-854">歧义的原因是，`object[]` 本身可隐式转换为类型 `object`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-854">The reason for the ambiguity is that an `object[]` is itself implicitly convertible to type `object`.</span></span> <span data-ttu-id="b24c2-855">但这种歧义不会带来任何问题，因为它可以通过在需要时插入强制转换来解决。</span><span class="sxs-lookup"><span data-stu-id="b24c2-855">The ambiguity presents no problem, however, since it can be resolved by inserting a cast if needed.</span></span>

<span data-ttu-id="b24c2-856">示例</span><span class="sxs-lookup"><span data-stu-id="b24c2-856">The example</span></span>
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
<span data-ttu-id="b24c2-857">生成输出</span><span class="sxs-lookup"><span data-stu-id="b24c2-857">produces the output</span></span>
```console
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="b24c2-858">在 `F`的第一次和最后一次调用中，`F` 的正常形式适用，因为存在从实参类型到形参类型的隐式转换（两者都是 `object[]`类型）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-858">In the first and last invocations of `F`, the normal form of `F` is applicable because an implicit conversion exists from the argument type to the parameter type (both are of type `object[]`).</span></span> <span data-ttu-id="b24c2-859">因此，重载决策选择 `F`的正常形式，并将参数作为常规值参数进行传递。</span><span class="sxs-lookup"><span data-stu-id="b24c2-859">Thus, overload resolution selects the normal form of `F`, and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="b24c2-860">在第二次和第三次调用中，`F` 的正常形式不适用，因为不存在从实参类型到形参类型的隐式转换（类型 `object` 无法隐式转换为类型 `object[]`）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-860">In the second and third invocations, the normal form of `F` is not applicable because no implicit conversion exists from the argument type to the parameter type (type `object` cannot be implicitly converted to type `object[]`).</span></span> <span data-ttu-id="b24c2-861">但 `F` 的展开形式适用，因此重载决策选择它。</span><span class="sxs-lookup"><span data-stu-id="b24c2-861">However, the expanded form of `F` is applicable, so it is selected by overload resolution.</span></span> <span data-ttu-id="b24c2-862">因此，调用会创建一个单元素 `object[]`，并使用给定的参数值（其本身是对 `object[]`的引用）初始化数组的单个元素。</span><span class="sxs-lookup"><span data-stu-id="b24c2-862">As a result, a one-element `object[]` is created by the invocation, and the single element of the array is initialized with the given argument value (which itself is a reference to an `object[]`).</span></span>

### <a name="static-and-instance-methods"></a><span data-ttu-id="b24c2-863">静态和实例方法</span><span class="sxs-lookup"><span data-stu-id="b24c2-863">Static and instance methods</span></span>

<span data-ttu-id="b24c2-864">当方法声明包含 `static` 修饰符时，该方法被称为静态方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-864">When a method declaration includes a `static` modifier, that method is said to be a static method.</span></span> <span data-ttu-id="b24c2-865">如果不存在 `static` 修饰符，则称该方法为实例方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-865">When no `static` modifier is present, the method is said to be an instance method.</span></span>

<span data-ttu-id="b24c2-866">静态方法不在特定实例上操作，并且在静态方法中引用 `this` 是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-866">A static method does not operate on a specific instance, and it is a compile-time error to refer to `this` in a static method.</span></span>

<span data-ttu-id="b24c2-867">实例方法对类的给定实例进行操作，并且该实例可以作为 `this` （[此访问](expressions.md#this-access)）进行访问。</span><span class="sxs-lookup"><span data-stu-id="b24c2-867">An instance method operates on a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="b24c2-868">当 `E.M`形式的*member_access* （[成员访问](expressions.md#member-access)）中引用方法时，如果 `M` 为静态方法，则 `E` 必须表示包含 `M`的类型，如果 `M` 是实例方法，则 `E` 必须表示包含 `M`的类型的实例。</span><span class="sxs-lookup"><span data-stu-id="b24c2-868">When a method is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static method, `E` must denote a type containing `M`, and if `M` is an instance method, `E` must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="b24c2-869">静态成员和实例成员之间的差异在[静态成员和实例成员](classes.md#static-and-instance-members)中进行了进一步讨论。</span><span class="sxs-lookup"><span data-stu-id="b24c2-869">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-methods"></a><span data-ttu-id="b24c2-870">虚方法</span><span class="sxs-lookup"><span data-stu-id="b24c2-870">Virtual methods</span></span>

<span data-ttu-id="b24c2-871">当实例方法声明包含 `virtual` 修饰符时，该方法被称为虚拟方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-871">When an instance method declaration includes a `virtual` modifier, that method is said to be a virtual method.</span></span> <span data-ttu-id="b24c2-872">如果不存在 `virtual` 修饰符，则称该方法为非虚拟方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-872">When no `virtual` modifier is present, the method is said to be a non-virtual method.</span></span>

<span data-ttu-id="b24c2-873">非虚方法的实现是不变的，无论是在声明方法的类的实例上调用方法，还是在派生类的实例中调用方法，实现都是相同的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-873">The implementation of a non-virtual method is invariant: The implementation is the same whether the method is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="b24c2-874">相反，虚方法的实现可由派生类取代。</span><span class="sxs-lookup"><span data-stu-id="b24c2-874">In contrast, the implementation of a virtual method can be superseded by derived classes.</span></span> <span data-ttu-id="b24c2-875">取代继承的虚方法的实现的过程称为***重写***该方法（[重写方法](classes.md#override-methods)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-875">The process of superseding the implementation of an inherited virtual method is known as ***overriding*** that method ([Override methods](classes.md#override-methods)).</span></span>

<span data-ttu-id="b24c2-876">在虚方法调用中，调用所针对的实例的***运行时类型***将确定要调用的实际方法实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-876">In a virtual method invocation, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="b24c2-877">在非虚拟方法调用中，实例的***编译时类型***是确定因素。</span><span class="sxs-lookup"><span data-stu-id="b24c2-877">In a non-virtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span> <span data-ttu-id="b24c2-878">确切地说，当使用参数 `A` 列表调用名为 `N` 的方法时，在编译时类型 `C` 和运行时类型 `R` （其中 `R` 是 `C` 或派生自 `C`的类）时，将按如下所示处理调用：</span><span class="sxs-lookup"><span data-stu-id="b24c2-878">In precise terms, when a method named `N` is invoked with an argument list `A` on an instance with a compile-time type `C` and a run-time type `R` (where `R` is either `C` or a class derived from `C`), the invocation is processed as follows:</span></span>

*  <span data-ttu-id="b24c2-879">首先，将重载决策应用于 `C`、`N`和 `A`，以便从在中声明的方法集中选择特定方法 `M`，并通过 `C`继承。</span><span class="sxs-lookup"><span data-stu-id="b24c2-879">First, overload resolution is applied to `C`, `N`, and `A`, to select a specific method `M` from the set of methods declared in and inherited by `C`.</span></span> <span data-ttu-id="b24c2-880">[方法调用](expressions.md#method-invocations)中对此进行了说明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-880">This is described in [Method invocations](expressions.md#method-invocations).</span></span>
*  <span data-ttu-id="b24c2-881">如果 `M` 为非虚拟方法，则调用 `M`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-881">Then, if `M` is a non-virtual method, `M` is invoked.</span></span>
*  <span data-ttu-id="b24c2-882">否则，`M` 是一个虚拟方法，将调用与 `R` 有关的 `M` 的派生程度最高的实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-882">Otherwise, `M` is a virtual method, and the most derived implementation of `M` with respect to `R` is invoked.</span></span>

<span data-ttu-id="b24c2-883">对于在类中声明或继承的每个虚方法，存在与该类相关的方法的***最大派生实现***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-883">For every virtual method declared in or inherited by a class, there exists a ***most derived implementation*** of the method with respect to that class.</span></span> <span data-ttu-id="b24c2-884">与类 `R` 相关 `M` 的虚拟方法的派生程度最高的实现如下所示：</span><span class="sxs-lookup"><span data-stu-id="b24c2-884">The most derived implementation of a virtual method `M` with respect to a class `R` is determined as follows:</span></span>

*  <span data-ttu-id="b24c2-885">如果 `R` 包含 `M`的声明引入 `virtual`，则这是 `M`的派生程度最高的实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-885">If `R` contains the introducing `virtual` declaration of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="b24c2-886">否则，如果 `R` 包含 `M`的 `override`，则这是 `M`的派生程度最高的实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-886">Otherwise, if `R` contains an `override` of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="b24c2-887">否则，与 `R` 的 `M` 的派生程度最高的实现与 `R`的直接基类的 `M` 的派生程度相同。</span><span class="sxs-lookup"><span data-stu-id="b24c2-887">Otherwise, the most derived implementation of `M` with respect to `R` is the same as the most derived implementation of `M` with respect to the direct base class of `R`.</span></span>

<span data-ttu-id="b24c2-888">下面的示例阐释了虚方法和非虚方法之间的差异：</span><span class="sxs-lookup"><span data-stu-id="b24c2-888">The following example illustrates the differences between virtual and non-virtual methods:</span></span>
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

<span data-ttu-id="b24c2-889">在此示例中，`A` 引入了一个非虚方法 `F` 和一个虚方法 `G`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-889">In the example, `A` introduces a non-virtual method `F` and a virtual method `G`.</span></span> <span data-ttu-id="b24c2-890">类 `B` 引入了新的非虚方法 `F`，从而隐藏了继承的 `F`，并且还会重写继承的方法 `G`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-890">The class `B` introduces a new non-virtual method `F`, thus hiding the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="b24c2-891">该示例生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="b24c2-891">The example produces the output:</span></span>
```console
A.F
B.F
B.G
B.G
```

<span data-ttu-id="b24c2-892">请注意，语句 `a.G()` 调用 `B.G`，而不是 `A.G`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-892">Notice that the statement `a.G()` invokes `B.G`, not `A.G`.</span></span> <span data-ttu-id="b24c2-893">这是因为实例的运行时类型（`B`），而不是实例的编译时类型（即 `A`）确定要调用的实际方法实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-893">This is because the run-time type of the instance (which is `B`), not the compile-time type of the instance (which is `A`), determines the actual method implementation to invoke.</span></span>

<span data-ttu-id="b24c2-894">由于允许方法隐藏继承方法，因此类可能包含多个具有相同签名的虚拟方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-894">Because methods are allowed to hide inherited methods, it is possible for a class to contain several virtual methods with the same signature.</span></span> <span data-ttu-id="b24c2-895">这并不会造成多义性问题，因为所有派生方法都是隐藏的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-895">This does not present an ambiguity problem, since all but the most derived method are hidden.</span></span> <span data-ttu-id="b24c2-896">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-896">In the example</span></span>
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
<span data-ttu-id="b24c2-897">`C` 和 `D` 类包含两个具有相同签名的虚拟方法： `A` 引入的两个虚拟方法，以及 `C`引入的两种方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-897">the `C` and `D` classes contain two virtual methods with the same signature: The one introduced by `A` and the one introduced by `C`.</span></span> <span data-ttu-id="b24c2-898">`C` 引入的方法隐藏了继承自 `A`的方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-898">The method introduced by `C` hides the method inherited from `A`.</span></span> <span data-ttu-id="b24c2-899">因此，`D` 中的重写声明会重写 `C`引入的方法，并且 `D` 重写 `A`引入的方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-899">Thus, the override declaration in `D` overrides the method introduced by `C`, and it is not possible for `D` to override the method introduced by `A`.</span></span> <span data-ttu-id="b24c2-900">该示例生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="b24c2-900">The example produces the output:</span></span>
```console
B.F
B.F
D.F
D.F
```

<span data-ttu-id="b24c2-901">请注意，可以通过不隐藏方法的派生程度较低的类型访问 `D` 实例，从而调用隐藏的虚拟方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-901">Note that it is possible to invoke the hidden virtual method by accessing an instance of `D` through a less derived type in which the method is not hidden.</span></span>

### <a name="override-methods"></a><span data-ttu-id="b24c2-902">重写方法</span><span class="sxs-lookup"><span data-stu-id="b24c2-902">Override methods</span></span>

<span data-ttu-id="b24c2-903">当实例方法声明包含 `override` 修饰符时，该方法被称为***替代方法***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-903">When an instance method declaration includes an `override` modifier, the method is said to be an ***override method***.</span></span> <span data-ttu-id="b24c2-904">重写方法使用相同的签名重写继承的虚方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-904">An override method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="b24c2-905">但如果虚方法声明中引入新方法，重写方法声明通过提供相应方法的新实现代码，专门针对现有的继承虚方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-905">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="b24c2-906">`override` 声明重写的方法称为***重写基方法***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-906">The method overridden by an `override` declaration is known as the ***overridden base method***.</span></span> <span data-ttu-id="b24c2-907">对于在类 `C`中声明 `M` 重写方法，重写的基方法是通过检查 `C`的每个基类类型（从 `C` 的直接基类类型开始，并继续每个连续的直接基类类型）确定的，直到至少有一个可访问方法与替换类型参数后，该方法的签名与 `M` 具有相同的签名。</span><span class="sxs-lookup"><span data-stu-id="b24c2-907">For an override method `M` declared in a class `C`, the overridden base method is determined by examining each base class type of `C`, starting with the direct base class type of `C` and continuing with each successive direct base class type, until in a given base class type at least one accessible method is located which has the same signature as `M` after substitution of type arguments.</span></span> <span data-ttu-id="b24c2-908">为了定位重写的基方法，如果 `protected``public`，则会认为该方法是可访问的（如果它是 `protected internal`的）; 或者，如果它是在 `C`的同一程序中 `internal` 并声明的，则认为该方法是可访问的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-908">For the purposes of locating the overridden base method, a method is considered accessible if it is `public`, if it is `protected`, if it is `protected internal`, or if it is `internal` and declared in the same program as `C`.</span></span>

<span data-ttu-id="b24c2-909">除非以下所有条件都适用于重写声明，否则将发生编译时错误：</span><span class="sxs-lookup"><span data-stu-id="b24c2-909">A compile-time error occurs unless all of the following are true for an override declaration:</span></span>

*  <span data-ttu-id="b24c2-910">重写的基方法可以按上文所述进行定位。</span><span class="sxs-lookup"><span data-stu-id="b24c2-910">An overridden base method can be located as described above.</span></span>
*  <span data-ttu-id="b24c2-911">正好有一个这种重写基方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-911">There is exactly one such overridden base method.</span></span> <span data-ttu-id="b24c2-912">仅当基类类型为构造类型时，此限制才会生效，在这种情况下，类型参数的替换会使两个方法的签名相同。</span><span class="sxs-lookup"><span data-stu-id="b24c2-912">This restriction has effect only if the base class type is a constructed type where the substitution of type arguments makes the signature of two methods the same.</span></span>
*  <span data-ttu-id="b24c2-913">重写的基方法为虚拟、抽象或重写方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-913">The overridden base method is a virtual, abstract, or override method.</span></span> <span data-ttu-id="b24c2-914">换言之，重写的基方法不能是静态的或非虚的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-914">In other words, the overridden base method cannot be static or non-virtual.</span></span>
*  <span data-ttu-id="b24c2-915">重写的基方法不是密封的方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-915">The overridden base method is not a sealed method.</span></span>
*  <span data-ttu-id="b24c2-916">重写方法和重写的基方法具有相同的返回类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-916">The override method and the overridden base method have the same return type.</span></span>
*  <span data-ttu-id="b24c2-917">重写声明和重写的基方法具有相同的声明的可访问性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-917">The override declaration and the overridden base method have the same declared accessibility.</span></span> <span data-ttu-id="b24c2-918">换言之，重写声明不能更改虚拟方法的可访问性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-918">In other words, an override declaration cannot change the accessibility of the virtual method.</span></span> <span data-ttu-id="b24c2-919">但是，如果重写的基方法是受保护的内部方法，并且在与包含 override 方法的程序集不同的程序集中声明，则必须保护 override 方法的声明的可访问性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-919">However, if the overridden base method is protected internal and it is declared in a different assembly than the assembly containing the override method then the override method's declared accessibility must be protected.</span></span>
*  <span data-ttu-id="b24c2-920">重写声明不指定类型参数约束子句。</span><span class="sxs-lookup"><span data-stu-id="b24c2-920">The override declaration does not specify type-parameter-constraints-clauses.</span></span> <span data-ttu-id="b24c2-921">相反，约束是从基方法继承而来的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-921">Instead the constraints are inherited from the overridden base method.</span></span> <span data-ttu-id="b24c2-922">请注意，在已重写的方法中属于类型参数的约束可能会替换为继承约束中的类型参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-922">Note that constraints that are type parameters in the overridden method may be replaced by type arguments in the inherited constraint.</span></span> <span data-ttu-id="b24c2-923">当显式指定时，这可能导致约束不合法，如值类型或密封类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-923">This can lead to constraints that are not legal when explicitly specified, such as value types or sealed types.</span></span>

<span data-ttu-id="b24c2-924">下面的示例演示重写规则如何适用于泛型类：</span><span class="sxs-lookup"><span data-stu-id="b24c2-924">The following example demonstrates how the overriding rules work for generic classes:</span></span>
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

<span data-ttu-id="b24c2-925">重写声明可使用*base_access* （[基本访问](expressions.md#base-access)）访问重写基方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-925">An override declaration can access the overridden base method using a *base_access* ([Base access](expressions.md#base-access)).</span></span> <span data-ttu-id="b24c2-926">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-926">In the example</span></span>
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
<span data-ttu-id="b24c2-927">`B` 中的 `base.PrintFields()` 调用调用在 `A`中声明的 `PrintFields` 方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-927">the `base.PrintFields()` invocation in `B` invokes the `PrintFields` method declared in `A`.</span></span> <span data-ttu-id="b24c2-928">*Base_access*禁用虚拟调用机制，只是将基方法视为非虚拟方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-928">A *base_access* disables the virtual invocation mechanism and simply treats the base method as a non-virtual method.</span></span> <span data-ttu-id="b24c2-929">在 `B` 中编写调用后 `((A)this).PrintFields()`，它将以递归方式调用在 `B`中声明的 `PrintFields` 方法，而不是在 `A`中声明的方法，因为 `PrintFields` 是虚拟的，`((A)this)` 的运行时类型 `B`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-929">Had the invocation in `B` been written `((A)this).PrintFields()`, it would recursively invoke the `PrintFields` method declared in `B`, not the one declared in `A`, since `PrintFields` is virtual and the run-time type of `((A)this)` is `B`.</span></span>

<span data-ttu-id="b24c2-930">只有通过包含 `override` 修饰符，方法才能重写另一个方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-930">Only by including an `override` modifier can a method override another method.</span></span> <span data-ttu-id="b24c2-931">在所有其他情况下，具有与继承方法相同的签名的方法只是隐藏了继承方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-931">In all other cases, a method with the same signature as an inherited method simply hides the inherited method.</span></span> <span data-ttu-id="b24c2-932">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-932">In the example</span></span>
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
<span data-ttu-id="b24c2-933">`B` 中的 `F` 方法不包括 `override` 修饰符，因此不会重写 `A`中的 `F` 方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-933">the `F` method in `B` does not include an `override` modifier and therefore does not override the `F` method in `A`.</span></span> <span data-ttu-id="b24c2-934">相反，`B` 中的 `F` 方法隐藏 `A`中的方法，并且会报告警告，因为声明不包括 `new` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-934">Rather, the `F` method in `B` hides the method in `A`, and a warning is reported because the declaration does not include a `new` modifier.</span></span>

<span data-ttu-id="b24c2-935">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-935">In the example</span></span>
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
<span data-ttu-id="b24c2-936">`B` 中的 `F` 方法隐藏了从 `A`继承的虚拟 `F` 方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-936">the `F` method in `B` hides the virtual `F` method inherited from `A`.</span></span> <span data-ttu-id="b24c2-937">由于 `B` 中的新 `F` 具有私有访问权限，因此其范围仅包括 `B` 的类体，不会扩展到 `C`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-937">Since the new `F` in `B` has private access, its scope only includes the class body of `B` and does not extend to `C`.</span></span> <span data-ttu-id="b24c2-938">因此，允许在 `C` 中 `F` 的声明重写继承自 `A`的 `F`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-938">Therefore, the declaration of `F` in `C` is permitted to override the `F` inherited from `A`.</span></span>

### <a name="sealed-methods"></a><span data-ttu-id="b24c2-939">密封方法</span><span class="sxs-lookup"><span data-stu-id="b24c2-939">Sealed methods</span></span>

<span data-ttu-id="b24c2-940">当实例方法声明包含 `sealed` 修饰符时，该方法被称为***密封方法***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-940">When an instance method declaration includes a `sealed` modifier, that method is said to be a ***sealed method***.</span></span> <span data-ttu-id="b24c2-941">如果实例方法声明包含 `sealed` 修饰符，则它还必须包括 `override` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-941">If an instance method declaration includes the  `sealed` modifier, it must also include the `override` modifier.</span></span> <span data-ttu-id="b24c2-942">使用 `sealed` 修饰符可防止派生类进一步重写方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-942">Use of the `sealed` modifier prevents a derived class from further overriding the method.</span></span>

<span data-ttu-id="b24c2-943">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-943">In the example</span></span>
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
<span data-ttu-id="b24c2-944">类 `B` 提供两个重写方法：具有 `sealed` 修饰符的 `F` 方法和不具有的 `G` 方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-944">the class `B` provides two override methods: an `F` method that has the `sealed` modifier and a `G` method that does not.</span></span> <span data-ttu-id="b24c2-945">`B`使用密封的 `modifier` 可防止 `C` 进一步重写 `F`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-945">`B`'s use of the sealed `modifier` prevents `C` from further overriding `F`.</span></span>

### <a name="abstract-methods"></a><span data-ttu-id="b24c2-946">抽象方法</span><span class="sxs-lookup"><span data-stu-id="b24c2-946">Abstract methods</span></span>

<span data-ttu-id="b24c2-947">当实例方法声明包含 `abstract` 修饰符时，该方法被称为***抽象方法***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-947">When an instance method declaration includes an `abstract` modifier, that method is said to be an ***abstract method***.</span></span> <span data-ttu-id="b24c2-948">尽管抽象方法还隐式成为虚方法，但它不能具有修饰符 `virtual`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-948">Although an abstract method is implicitly also a virtual method, it cannot have the modifier `virtual`.</span></span>

<span data-ttu-id="b24c2-949">抽象方法声明引入新的虚方法，但不提供该方法的实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-949">An abstract method declaration introduces a new virtual method but does not provide an implementation of that method.</span></span> <span data-ttu-id="b24c2-950">相反，非抽象派生类需要通过重写方法来提供自己的实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-950">Instead, non-abstract derived classes are required to provide their own implementation by overriding that method.</span></span> <span data-ttu-id="b24c2-951">因为抽象方法不提供实际的实现，所以抽象方法的*method_body*只包含一个分号。</span><span class="sxs-lookup"><span data-stu-id="b24c2-951">Because an abstract method provides no actual implementation, the *method_body* of an abstract method simply consists of a semicolon.</span></span>

<span data-ttu-id="b24c2-952">抽象方法声明仅允许在抽象类（[抽象类](classes.md#abstract-classes)）中使用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-952">Abstract method declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="b24c2-953">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-953">In the example</span></span>
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
<span data-ttu-id="b24c2-954">`Shape` 类定义可自行绘制的几何形状对象的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="b24c2-954">the `Shape` class defines the abstract notion of a geometrical shape object that can paint itself.</span></span> <span data-ttu-id="b24c2-955">`Paint` 方法是抽象的，因为没有有意义的默认实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-955">The `Paint` method is abstract because there is no meaningful default implementation.</span></span> <span data-ttu-id="b24c2-956">`Ellipse` 和 `Box` 类是具体的 `Shape` 实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-956">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="b24c2-957">由于这些类是非抽象类，因此需要重写 `Paint` 方法，并提供实际的实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-957">Because these classes are non-abstract, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="b24c2-958">*Base_access* （[基本访问](expressions.md#base-access)）引用抽象方法的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-958">It is a compile-time error for a *base_access* ([Base access](expressions.md#base-access)) to reference an abstract method.</span></span> <span data-ttu-id="b24c2-959">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-959">In the example</span></span>
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
<span data-ttu-id="b24c2-960">为 `base.F()` 调用报告编译时错误，因为它引用了抽象方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-960">a compile-time error is reported for the `base.F()` invocation because it references an abstract method.</span></span>

<span data-ttu-id="b24c2-961">允许抽象方法声明重写虚拟方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-961">An abstract method declaration is permitted to override a virtual method.</span></span> <span data-ttu-id="b24c2-962">这允许抽象类强制在派生类中重新实现方法，并使方法的原始实现不可用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-962">This allows an abstract class to force re-implementation of the method in derived classes, and makes the original implementation of the method unavailable.</span></span> <span data-ttu-id="b24c2-963">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-963">In the example</span></span>
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
<span data-ttu-id="b24c2-964">类 `A` 声明虚方法，类 `B` 使用抽象方法重写此方法，类 `C` 重写抽象方法以提供其自己的实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-964">class `A` declares a virtual method, class `B` overrides this method with an abstract method, and class `C` overrides the abstract method to provide its own implementation.</span></span>

### <a name="external-methods"></a><span data-ttu-id="b24c2-965">外部方法</span><span class="sxs-lookup"><span data-stu-id="b24c2-965">External methods</span></span>

<span data-ttu-id="b24c2-966">当方法声明包含 `extern` 修饰符时，该方法被称为***外部方法***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-966">When a method declaration includes an `extern` modifier, that method is said to be an ***external method***.</span></span> <span data-ttu-id="b24c2-967">外部方法在外部实现，通常使用以外的语言C#。</span><span class="sxs-lookup"><span data-stu-id="b24c2-967">External methods are implemented externally, typically using a language other than C#.</span></span> <span data-ttu-id="b24c2-968">由于外部方法声明不提供实际的实现，因此外部方法的*method_body*只包含一个分号。</span><span class="sxs-lookup"><span data-stu-id="b24c2-968">Because an external method declaration provides no actual implementation, the *method_body* of an external method simply consists of a semicolon.</span></span> <span data-ttu-id="b24c2-969">外部方法不能是泛型的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-969">An external method may not be generic.</span></span>

<span data-ttu-id="b24c2-970">`extern` 修饰符通常与 `DllImport` 特性结合使用（[与 COM 和 Win32 组件互操作](attributes.md#interoperation-with-com-and-win32-components)），允许 Dll （动态链接库）实现外部方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-970">The `extern` modifier is typically used in conjunction with a `DllImport` attribute ([Interoperation with COM and Win32 components](attributes.md#interoperation-with-com-and-win32-components)), allowing external methods to be implemented by DLLs (Dynamic Link Libraries).</span></span> <span data-ttu-id="b24c2-971">执行环境可能支持可提供外部方法实现的其他机制。</span><span class="sxs-lookup"><span data-stu-id="b24c2-971">The execution environment may support other mechanisms whereby implementations of external methods can be provided.</span></span>

<span data-ttu-id="b24c2-972">当外部方法包含 `DllImport` 特性时，该方法声明还必须包含 `static` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-972">When an external method includes a `DllImport` attribute, the method declaration must also include a `static` modifier.</span></span> <span data-ttu-id="b24c2-973">此示例演示了 `extern` 修饰符和 `DllImport` 属性的用法：</span><span class="sxs-lookup"><span data-stu-id="b24c2-973">This example demonstrates the use of the `extern` modifier and the `DllImport` attribute:</span></span>
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

### <a name="partial-methods-recap"></a><span data-ttu-id="b24c2-974">分部方法（概述）</span><span class="sxs-lookup"><span data-stu-id="b24c2-974">Partial methods (recap)</span></span>

<span data-ttu-id="b24c2-975">当方法声明包含 `partial` 修饰符时，该方法被称为***分部方法***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-975">When a method declaration includes a `partial` modifier, that method is said to be a ***partial method***.</span></span> <span data-ttu-id="b24c2-976">分部方法只能声明为分部类型的成员（[分部类型](classes.md#partial-types)），并且受到多种限制。</span><span class="sxs-lookup"><span data-stu-id="b24c2-976">Partial methods can only be declared as members of partial types ([Partial types](classes.md#partial-types)), and are subject to a number of restrictions.</span></span> <span data-ttu-id="b24c2-977">[分部方法中进一步](classes.md#partial-methods)介绍了分部方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-977">Partial methods are further described in [Partial methods](classes.md#partial-methods).</span></span>

### <a name="extension-methods"></a><span data-ttu-id="b24c2-978">扩展方法</span><span class="sxs-lookup"><span data-stu-id="b24c2-978">Extension methods</span></span>

<span data-ttu-id="b24c2-979">如果方法的第一个参数包含 `this` 修饰符，则称该方法是***扩展方法***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-979">When the first parameter of a method includes the `this` modifier, that method is said to be an ***extension method***.</span></span> <span data-ttu-id="b24c2-980">扩展方法只能在非泛型非嵌套静态类中声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-980">Extension methods can only be declared in non-generic, non-nested static classes.</span></span> <span data-ttu-id="b24c2-981">扩展方法的第一个参数不能包含除 `this`以外的任何修饰符，并且参数类型不能是指针类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-981">The first parameter of an extension method can have no modifiers other than `this`, and the parameter type cannot be a pointer type.</span></span>

<span data-ttu-id="b24c2-982">下面是一个声明两个扩展方法的静态类示例：</span><span class="sxs-lookup"><span data-stu-id="b24c2-982">The following is an example of a static class that declares two extension methods:</span></span>
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

<span data-ttu-id="b24c2-983">扩展方法是一个常规静态方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-983">An extension method is a regular static method.</span></span> <span data-ttu-id="b24c2-984">此外，在其封闭静态类处于范围内时，可以使用实例方法调用语法（[扩展方法调用](expressions.md#extension-method-invocations)）来调用扩展方法，使用接收方表达式作为第一个参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-984">In addition, where its enclosing static class is in scope, an extension method can be invoked using instance method invocation syntax ([Extension method invocations](expressions.md#extension-method-invocations)), using the receiver expression as the first argument.</span></span>

<span data-ttu-id="b24c2-985">下面的程序使用上面声明的扩展方法：</span><span class="sxs-lookup"><span data-stu-id="b24c2-985">The following program uses the extension methods declared above:</span></span>
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

<span data-ttu-id="b24c2-986">`Slice` 方法在 `string[]`上可用，`ToInt32` 方法可在 `string`上使用，因为它们已被声明为扩展方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-986">The `Slice` method is available on the `string[]`, and the `ToInt32` method is available on `string`, because they have been declared as extension methods.</span></span> <span data-ttu-id="b24c2-987">使用普通的静态方法调用，程序的含义与下面相同：</span><span class="sxs-lookup"><span data-stu-id="b24c2-987">The meaning of the program is the same as the following, using ordinary static method calls:</span></span>
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

### <a name="method-body"></a><span data-ttu-id="b24c2-988">方法主体</span><span class="sxs-lookup"><span data-stu-id="b24c2-988">Method body</span></span>

<span data-ttu-id="b24c2-989">方法声明的*method_body*包含一个块体、一个表达式体或一个分号。</span><span class="sxs-lookup"><span data-stu-id="b24c2-989">The *method_body* of a method declaration consists of either a block body, an expression body or a semicolon.</span></span>

<span data-ttu-id="b24c2-990">如果返回类型为 `void`，则 `void` 方法的***结果类型***; 如果该方法是异步的并且返回类型为 `System.Threading.Tasks.Task`，则为。</span><span class="sxs-lookup"><span data-stu-id="b24c2-990">The ***result type*** of a method is `void` if the return type is `void`, or if the method is async and the return type is `System.Threading.Tasks.Task`.</span></span> <span data-ttu-id="b24c2-991">否则，非异步方法的结果类型是其返回类型，并且 `T`具有返回类型 `System.Threading.Tasks.Task<T>` 的异步方法的结果类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-991">Otherwise, the result type of a non-async method is its return type, and the result type of an async method with return type `System.Threading.Tasks.Task<T>` is `T`.</span></span>

<span data-ttu-id="b24c2-992">当方法具有 `void` 结果类型和块体时，不允许在块中使用 `return` 语句（[return 语句](statements.md#the-return-statement)）来指定表达式。</span><span class="sxs-lookup"><span data-stu-id="b24c2-992">When a method has a `void` result type and a block body, `return` statements ([The return statement](statements.md#the-return-statement)) in the block are not permitted to specify an expression.</span></span> <span data-ttu-id="b24c2-993">如果 void 方法的执行操作正常完成（即，控制流从方法体的结尾处），该方法只会返回到当前调用方。</span><span class="sxs-lookup"><span data-stu-id="b24c2-993">If execution of the block of a void method completes normally (that is, control flows off the end of the method body), that method simply returns to its current caller.</span></span>
    
<span data-ttu-id="b24c2-994">如果方法具有 `void` 结果和表达式体，则表达式 `E` 必须是*statement_expression*，并且正文完全等同于窗体 `{ E; }`的块体。</span><span class="sxs-lookup"><span data-stu-id="b24c2-994">When a method has a `void` result and an expression body, the expression `E` must be a *statement_expression*, and the body is exactly equivalent to a block body of the form `{ E; }`.</span></span>
    
<span data-ttu-id="b24c2-995">当方法具有非 void 结果类型和块体时，块中的每个 `return` 语句必须指定一个可隐式转换为结果类型的表达式。</span><span class="sxs-lookup"><span data-stu-id="b24c2-995">When a method has a non-void result type and a block body, each `return` statement in the block must specify an expression that is implicitly convertible to the result type.</span></span> <span data-ttu-id="b24c2-996">返回值的方法的块体的终结点必须是无法访问的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-996">The endpoint of a block body of a value-returning method must not be reachable.</span></span> <span data-ttu-id="b24c2-997">换言之，在带有块体的值返回方法中，不允许控件流过方法体的末尾。</span><span class="sxs-lookup"><span data-stu-id="b24c2-997">In other words, in a value-returning method with a block body, control is not permitted to flow off the end of the method body.</span></span>
    
<span data-ttu-id="b24c2-998">如果方法具有非 void 结果类型和表达式体，则表达式必须可隐式转换为结果类型，并且正文完全等效于窗体 `{ return E; }`的块体。</span><span class="sxs-lookup"><span data-stu-id="b24c2-998">When a method has a non-void result type and an expression body, the expression must be implicitly convertible to the result type, and the body is exactly equivalent to a block body of the form `{ return E; }`.</span></span>
    
<span data-ttu-id="b24c2-999">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-999">In the example</span></span>
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
<span data-ttu-id="b24c2-1000">返回 `F` 方法返回的值会导致编译时错误，因为控件可以流过方法体的末尾。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1000">the value-returning `F` method results in a compile-time error because control can flow off the end of the method body.</span></span> <span data-ttu-id="b24c2-1001">`G` 和 `H` 方法正确，因为所有可能的执行路径都以指定返回值的 return 语句结尾。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1001">The `G` and `H` methods are correct because all possible execution paths end in a return statement that specifies a return value.</span></span> <span data-ttu-id="b24c2-1002">`I` 方法是正确的，因为它的主体等效于其中只包含一个返回语句的语句块。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1002">The `I` method is correct, because its body is equivalent to a statement block with just a single return statement in it.</span></span>

### <a name="method-overloading"></a><span data-ttu-id="b24c2-1003">方法重载</span><span class="sxs-lookup"><span data-stu-id="b24c2-1003">Method overloading</span></span>

<span data-ttu-id="b24c2-1004">[类型推理](expressions.md#type-inference)中介绍了方法重载决策规则。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1004">The method overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="properties"></a><span data-ttu-id="b24c2-1005">属性</span><span class="sxs-lookup"><span data-stu-id="b24c2-1005">Properties</span></span>

<span data-ttu-id="b24c2-1006">***属性***是提供对对象或类的特征的访问的成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1006">A ***property*** is a member that provides access to a characteristic of an object or a class.</span></span> <span data-ttu-id="b24c2-1007">属性的示例包括字符串的长度、字体的大小、窗口的标题、客户的名称，等等。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1007">Examples of properties include the length of a string, the size of a font, the caption of a window, the name of a customer, and so on.</span></span> <span data-ttu-id="b24c2-1008">属性是字段的自然扩展，它们都是具有关联类型的命名成员，并且用于访问字段和属性的语法相同。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1008">Properties are a natural extension of fields—both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="b24c2-1009">不过，与字段不同的是，属性不指明存储位置。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1009">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="b24c2-1010">相反，属性包含***访问器***，用于指定在读取或写入属性值时要执行的语句。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1010">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span> <span data-ttu-id="b24c2-1011">因此，属性提供了一种机制，用于将操作与对象的属性的读取和写入操作相关联;而且，它们允许计算此类属性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1011">Properties thus provide a mechanism for associating actions with the reading and writing of an object's attributes; furthermore, they permit such attributes to be computed.</span></span>

<span data-ttu-id="b24c2-1012">属性使用*property_declaration*s 来声明：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1012">Properties are declared using *property_declaration*s:</span></span>

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

<span data-ttu-id="b24c2-1013">*Property_declaration*可以包含一组*属性*（[特性](attributes.md)）和四个访问修饰符（[访问修饰符](classes.md#access-modifiers)）、`new` （[新修饰符](classes.md#the-new-modifier)）、`static` （[静态和实例方法](classes.md#static-and-instance-methods)）、`virtual` （[虚拟方法](classes.md#virtual-methods)）、`override` （[重写方法](classes.md#override-methods)）、`sealed` （[密封方法](classes.md#sealed-methods)）、`abstract` （[抽象方法](classes.md#abstract-methods)）和 `extern` （[外部方法](classes.md#external-methods)）修饰符的有效组合。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1013">A *property_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="b24c2-1014">属性声明服从与方法声明（[方法](classes.md#methods)）相同的规则，与修饰符的有效组合有关。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1014">Property declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="b24c2-1015">属性声明的*类型*指定声明引入的属性的类型， *member_name*指定属性的名称。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1015">The *type* of a property declaration specifies the type of the property introduced by the declaration, and the *member_name* specifies the name of the property.</span></span> <span data-ttu-id="b24c2-1016">除非该属性是显式接口成员实现，否则*member_name*只是一个*标识符*。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1016">Unless the property is an explicit interface member implementation, the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="b24c2-1017">对于显式接口成员实现（[显式接口成员实现](interfaces.md#explicit-interface-member-implementations)）， *member_name*由*interface_type*后跟 "`.`" 和*标识符*组成。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1017">For an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="b24c2-1018">属性的*类型*必须至少具有与属性本身相同的可访问性（[辅助功能约束](basic-concepts.md#accessibility-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1018">The *type* of a property must be at least as accessible as the property itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="b24c2-1019">*Property_body*可以包含***访问器体***或***表达式主体***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1019">A *property_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="b24c2-1020">在访问器主体中， *accessor_declarations*（必须将其括在 "`{`" 和 "`}`" 标记中），请声明属性的访问器（[访问](classes.md#accessors)器）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1020">In an accessor body,  *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="b24c2-1021">访问器指定与读取和写入属性相关联的可执行语句。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1021">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="b24c2-1022">由 `=>` 后跟*表达式*`E` 并且分号完全等效于语句体 `{ get { return E; } }`的表达式体，因此仅可用于指定仅限 getter 的属性，其中，getter 的结果由单个表达式提供。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1022">An expression body consisting of `=>` followed by an *expression* `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only properties where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="b24c2-1023">只能为自动实现的属性（[自动实现的属性](classes.md#automatically-implemented-properties)）提供*property_initializer* ，并导致使用*表达式*提供的值初始化此类属性的基础字段。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1023">A *property_initializer* may only be given for an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)), and causes the initialization of the underlying field of such properties with the value given by the *expression*.</span></span>

<span data-ttu-id="b24c2-1024">尽管用于访问属性的语法与字段的语法相同，但属性未分类为变量。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1024">Even though the syntax for accessing a property is the same as that for a field, a property is not classified as a variable.</span></span> <span data-ttu-id="b24c2-1025">因此，不能将属性作为 `ref` 或 `out` 参数进行传递。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1025">Thus, it is not possible to pass a property as a `ref` or `out` argument.</span></span>

<span data-ttu-id="b24c2-1026">如果属性声明包含 `extern` 修饰符，则该属性被称为***外部属性***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1026">When a property declaration includes an `extern` modifier, the property is said to be an ***external property***.</span></span> <span data-ttu-id="b24c2-1027">由于外部属性声明不提供实际的实现，因此它的每个*accessor_declarations*都包含一个分号。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1027">Because an external property declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

### <a name="static-and-instance-properties"></a><span data-ttu-id="b24c2-1028">静态属性和实例属性</span><span class="sxs-lookup"><span data-stu-id="b24c2-1028">Static and instance properties</span></span>

<span data-ttu-id="b24c2-1029">如果属性声明包含 `static` 修饰符，则该属性被称为***静态属性***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1029">When a property declaration includes a `static` modifier, the property is said to be a ***static property***.</span></span> <span data-ttu-id="b24c2-1030">如果不存在 `static` 修饰符，则将属性称为***实例属性***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1030">When no `static` modifier is present, the property is said to be an ***instance property***.</span></span>

<span data-ttu-id="b24c2-1031">静态属性不与特定实例相关联，并且是在静态属性的访问器中引用 `this` 的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1031">A static property is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static property.</span></span>

<span data-ttu-id="b24c2-1032">实例属性与类的给定实例相关联，并且该实例可以作为该属性的访问器中 `this` （[此访问](expressions.md#this-access)）进行访问。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1032">An instance property is associated with a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that property.</span></span>

<span data-ttu-id="b24c2-1033">当 `E.M`格式的*member_access* （[成员访问](expressions.md#member-access)）中引用属性时，如果 `M` 为静态属性，则 `E` 必须表示包含 `M`的类型，如果 `M` 是实例属性，则 E 必须表示包含 `M`的类型的实例。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1033">When a property is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static property, `E` must denote a type containing `M`, and if `M` is an instance property, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="b24c2-1034">静态成员和实例成员之间的差异在[静态成员和实例成员](classes.md#static-and-instance-members)中进行了进一步讨论。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1034">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="accessors"></a><span data-ttu-id="b24c2-1035">Remove</span><span class="sxs-lookup"><span data-stu-id="b24c2-1035">Accessors</span></span>

<span data-ttu-id="b24c2-1036">属性的*accessor_declarations*指定与读取和写入属性相关联的可执行语句。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1036">The *accessor_declarations* of a property specify the executable statements associated with reading and writing that property.</span></span>

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

<span data-ttu-id="b24c2-1037">访问器声明由*get_accessor_declaration*和/或*set_accessor_declaration*组成。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1037">The accessor declarations consist of a *get_accessor_declaration*, a *set_accessor_declaration*, or both.</span></span> <span data-ttu-id="b24c2-1038">每个访问器声明都包含标记 `get` 或 `set` 后跟可选的*accessor_modifier*和*accessor_body*。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1038">Each accessor declaration consists of the token `get` or `set` followed by an optional *accessor_modifier* and an *accessor_body*.</span></span>

<span data-ttu-id="b24c2-1039">*Accessor_modifier*的使用受以下限制的制约：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1039">The use of *accessor_modifier*s is governed by the following restrictions:</span></span>

*  <span data-ttu-id="b24c2-1040">*Accessor_modifier*不能在接口或显式接口成员实现中使用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1040">An *accessor_modifier* may not be used in an interface or in an explicit interface member implementation.</span></span>
*  <span data-ttu-id="b24c2-1041">对于没有 `override` 修饰符的属性或索引器，仅当属性或索引器同时具有 `get` 和 `set` 访问器时，才允许*accessor_modifier* ，然后只允许在其中一种访问器上使用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1041">For a property or indexer that has no `override` modifier, an *accessor_modifier* is permitted only if the property or indexer has both a `get` and `set` accessor, and then is permitted only on one of those accessors.</span></span>
*  <span data-ttu-id="b24c2-1042">对于包含 `override` 修饰符的属性或索引器，访问器必须与要重写的访问器的*accessor_modifier*（如果有）匹配。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1042">For a property or indexer that includes an `override` modifier, an accessor must match the *accessor_modifier*, if any, of the accessor being overridden.</span></span>
*  <span data-ttu-id="b24c2-1043">*Accessor_modifier*必须声明一个可访问性，严格比属性或索引器本身的声明的可访问性更严格。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1043">The *accessor_modifier* must declare an accessibility that is strictly more restrictive than the declared accessibility of the property or indexer itself.</span></span> <span data-ttu-id="b24c2-1044">精确：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1044">To be precise:</span></span>
   * <span data-ttu-id="b24c2-1045">如果属性或索引器具有 `public`的声明的可访问性，则*accessor_modifier*可能是 `protected internal`、`internal`、`protected`或 `private`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1045">If the property or indexer has a declared accessibility of `public`, the *accessor_modifier* may be either `protected internal`, `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="b24c2-1046">如果属性或索引器具有 `protected internal`的声明的可访问性，则*accessor_modifier*可能是 `internal`、`protected`或 `private`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1046">If the property or indexer has a declared accessibility of `protected internal`, the *accessor_modifier* may be either `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="b24c2-1047">如果属性或索引器具有 `internal` 或 `protected`的声明的可访问性，则*accessor_modifier*必须 `private`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1047">If the property or indexer has a declared accessibility of `internal` or `protected`, the *accessor_modifier* must be `private`.</span></span>
   * <span data-ttu-id="b24c2-1048">如果属性或索引器具有 `private`的声明的可访问性，则不能使用任何*accessor_modifier* 。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1048">If the property or indexer has a declared accessibility of `private`, no *accessor_modifier* may be used.</span></span>

<span data-ttu-id="b24c2-1049">对于 `abstract` 和 `extern` 属性，所指定的每个访问器的*accessor_body*只是一个分号。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1049">For `abstract` and `extern` properties, the *accessor_body* for each accessor specified is simply a semicolon.</span></span> <span data-ttu-id="b24c2-1050">非抽象的非 extern 属性可能具有每个*accessor_body*都是一个分号，在这种情况下，它是一个***自动实现的属性***（[自动实现的属性](classes.md#automatically-implemented-properties)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1050">A non-abstract, non-extern property may have each *accessor_body* be a semicolon, in which case it is an ***automatically implemented property*** ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="b24c2-1051">自动实现的属性必须至少具有 get 访问器。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1051">An automatically implemented property must have at least a get accessor.</span></span> <span data-ttu-id="b24c2-1052">对于任何其他非抽象非 extern 属性的访问器， *accessor_body*是一个*块*，它指定在调用相应的访问器时要执行的语句。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1052">For the accessors of any other non-abstract, non-extern property, the *accessor_body* is a *block* which specifies the statements to be executed when the corresponding accessor is invoked.</span></span>

<span data-ttu-id="b24c2-1053">`get` 访问器与具有属性类型返回值的无参数方法相对应。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1053">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="b24c2-1054">除了作为赋值的目标以外，在表达式中引用属性时，将调用属性的 `get` 访问器来计算属性的值（[表达式的值](expressions.md#values-of-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1054">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property ([Values of expressions](expressions.md#values-of-expressions)).</span></span> <span data-ttu-id="b24c2-1055">`get` 访问器的主体必须符合[方法体](classes.md#method-body)中所述的返回值的方法的规则。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1055">The body of a `get` accessor must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="b24c2-1056">特别是，`get` 访问器体中的所有 `return` 语句都必须指定一个可隐式转换为属性类型的表达式。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1056">In particular, all `return` statements in the body of a `get` accessor must specify an expression that is implicitly convertible to the property type.</span></span> <span data-ttu-id="b24c2-1057">此外，`get` 访问器的终结点必须是无法访问的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1057">Furthermore, the endpoint of a `get` accessor must not be reachable.</span></span>

<span data-ttu-id="b24c2-1058">`set` 访问器对应于一个方法，该方法具有属性类型的单个值参数和一个 `void` 返回类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1058">A `set` accessor corresponds to a method with a single value parameter of the property type and a `void` return type.</span></span> <span data-ttu-id="b24c2-1059">`set` 访问器的隐式参数始终命名为 `value`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1059">The implicit parameter of a `set` accessor is always named `value`.</span></span> <span data-ttu-id="b24c2-1060">当属性作为赋值的目标（[赋值运算符](expressions.md#assignment-operators)）进行引用时，或者作为 `++` 或 `--` （[后缀增量和减量运算符](expressions.md#postfix-increment-and-decrement-operators)、[前缀增量和减量运算符](expressions.md#prefix-increment-and-decrement-operators)）的操作数，使用参数（其值是赋值的右侧或 `++` 或 `--` 运算符的操作数）提供新值（[简单赋值](expressions.md#simple-assignment)）来调用 `set` 访问器。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1060">When a property is referenced as the target of an assignment ([Assignment operators](expressions.md#assignment-operators)), or as the operand of `++` or `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), the `set` accessor is invoked with an argument (whose value is that of the right-hand side of the assignment or the operand of the `++` or `--` operator) that provides the new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="b24c2-1061">`set` 访问器的主体必须符合[方法体](classes.md#method-body)中所述的 `void` 方法的规则。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1061">The body of a `set` accessor must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="b24c2-1062">特别是，不允许 `set` 访问器正文中的 `return` 语句指定表达式。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1062">In particular, `return` statements in the `set` accessor body are not permitted to specify an expression.</span></span> <span data-ttu-id="b24c2-1063">由于 `set` 访问器隐式包含一个名为 `value`的参数，因此，`set` 访问器中的局部变量或常量声明的编译时错误是具有该名称。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1063">Since a `set` accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declaration in a `set` accessor to have that name.</span></span>

<span data-ttu-id="b24c2-1064">根据 `get` 和 `set` 访问器的存在情况，将按如下方式对属性进行分类：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1064">Based on the presence or absence of the `get` and `set` accessors, a property is classified as follows:</span></span>

*  <span data-ttu-id="b24c2-1065">同时包含 `get` 访问器和 `set` 访问器的属性称为***读写***属性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1065">A property that includes both a `get` accessor and a `set` accessor is said to be a ***read-write*** property.</span></span>
*  <span data-ttu-id="b24c2-1066">仅具有 `get` 访问器的属性称为***只读***属性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1066">A property that has only a `get` accessor is said to be a ***read-only*** property.</span></span> <span data-ttu-id="b24c2-1067">只读属性是赋值的目标时，这是一个编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1067">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>
*  <span data-ttu-id="b24c2-1068">仅具有 `set` 访问器的属性称为***只写***属性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1068">A property that has only a `set` accessor is said to be a ***write-only*** property.</span></span> <span data-ttu-id="b24c2-1069">除非作为赋值的目标，否则在表达式中引用只写属性是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1069">Except as the target of an assignment, it is a compile-time error to reference a write-only property in an expression.</span></span>

<span data-ttu-id="b24c2-1070">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-1070">In the example</span></span>
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
<span data-ttu-id="b24c2-1071">`Button` 控件声明公共 `Caption` 属性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1071">the `Button` control declares a public `Caption` property.</span></span> <span data-ttu-id="b24c2-1072">`Caption` 属性的 `get` 访问器返回私有 `caption` 字段中存储的字符串。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1072">The `get` accessor of the `Caption` property returns the string stored in the private `caption` field.</span></span> <span data-ttu-id="b24c2-1073">`set` 访问器检查新值是否不同于当前值，如果是，则它存储新值并重新绘制控件。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1073">The `set` accessor checks if the new value is different from the current value, and if so, it stores the new value and repaints the control.</span></span> <span data-ttu-id="b24c2-1074">属性通常遵循上面所示的模式： `get` 访问器只返回一个存储在私有字段中的值，而 `set` 访问器则修改该私有字段，然后执行完全更新对象状态所需的任何其他操作。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1074">Properties often follow the pattern shown above: The `get` accessor simply returns a value stored in a private field, and the `set` accessor modifies that private field and then performs any additional actions required to fully update the state of the object.</span></span>

<span data-ttu-id="b24c2-1075">假设上述 `Button` 类，以下是使用 `Caption` 属性的示例：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1075">Given the `Button` class above, the following is an example of use of the `Caption` property:</span></span>
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

<span data-ttu-id="b24c2-1076">此处，通过将值分配给属性来调用 `set` 访问器，通过引用表达式中的属性来调用 `get` 访问器。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1076">Here, the `set` accessor is invoked by assigning a value to the property, and the `get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="b24c2-1077">属性的 `get` 和 `set` 访问器不是不同的成员，并且不能单独声明属性的访问器。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1077">The `get` and `set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="b24c2-1078">因此，读写属性的两个访问器不可能具有不同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1078">As such, it is not possible for the two accessors of a read-write property to have different accessibility.</span></span> <span data-ttu-id="b24c2-1079">示例</span><span class="sxs-lookup"><span data-stu-id="b24c2-1079">The example</span></span>
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
<span data-ttu-id="b24c2-1080">不声明单个读写属性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1080">does not declare a single read-write property.</span></span> <span data-ttu-id="b24c2-1081">相反，它声明了两个具有相同名称的属性，一个为只读，一个只写。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1081">Rather, it declares two properties with the same name, one read-only and one write-only.</span></span> <span data-ttu-id="b24c2-1082">由于在同一类中声明的两个成员不能具有相同的名称，因此该示例将导致发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1082">Since two members declared in the same class cannot have the same name, the example causes a compile-time error to occur.</span></span>

<span data-ttu-id="b24c2-1083">当派生类用与继承属性相同的名称声明属性时，派生属性将隐藏与读取和写入相关的继承属性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1083">When a derived class declares a property by the same name as an inherited property, the derived property hides the inherited property with respect to both reading and writing.</span></span> <span data-ttu-id="b24c2-1084">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-1084">In the example</span></span>
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
<span data-ttu-id="b24c2-1085">`B` 中的 `P` 属性隐藏 `A` 中的 `P` 属性，与读取和写入有关。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1085">the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing.</span></span> <span data-ttu-id="b24c2-1086">因此，在语句中</span><span class="sxs-lookup"><span data-stu-id="b24c2-1086">Thus, in the statements</span></span>
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
<span data-ttu-id="b24c2-1087">分配到 `b.P` 会导致报告编译时错误，因为 `B` 中的只读 `P` 属性将隐藏 `A`中的只写 `P` 属性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1087">the assignment to `b.P` causes a compile-time error to be reported, since the read-only `P` property in `B` hides the write-only `P` property in `A`.</span></span> <span data-ttu-id="b24c2-1088">但请注意，强制转换可用于访问隐藏的 `P` 属性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1088">Note, however, that a cast can be used to access the hidden `P` property.</span></span>

<span data-ttu-id="b24c2-1089">与公共字段不同，属性可在对象的内部状态和其公共接口之间提供分隔。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1089">Unlike public fields, properties provide a separation between an object's internal state and its public interface.</span></span> <span data-ttu-id="b24c2-1090">请看下面的示例：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1090">Consider the example:</span></span>
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

<span data-ttu-id="b24c2-1091">此处，`Label` 类使用两个 `int` 字段 `x` 和 `y`来存储其位置。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1091">Here, the `Label` class uses two `int` fields, `x` and `y`, to store its location.</span></span> <span data-ttu-id="b24c2-1092">该位置公开为 `X` 和 `Y` 属性，并作为 `Point`类型的 `Location` 属性公开。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1092">The location is publicly exposed both as an `X` and a `Y` property and as a `Location` property of type `Point`.</span></span> <span data-ttu-id="b24c2-1093">如果在 `Label`的未来版本中，在内部将位置存储为 `Point` 会更方便，而不会影响类的公共接口，则可以进行更改：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1093">If, in a future version of `Label`, it becomes more convenient to store the location as a `Point` internally, the change can be made without affecting the public interface of the class:</span></span>
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

<span data-ttu-id="b24c2-1094">`x` 和 `y` `public readonly` 字段，则不可能对 `Label` 类进行此类更改。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1094">Had `x` and `y` instead been `public readonly` fields, it would have been impossible to make such a change to the `Label` class.</span></span>

<span data-ttu-id="b24c2-1095">通过属性公开状态不一定比直接公开字段的效率低。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1095">Exposing state through properties is not necessarily any less efficient than exposing fields directly.</span></span> <span data-ttu-id="b24c2-1096">特别是，当某个属性是非虚拟的并且只包含少量的代码时，执行环境可能会将对访问器的调用替换为访问器的实际代码。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1096">In particular, when a property is non-virtual and contains only a small amount of code, the execution environment may replace calls to accessors with the actual code of the accessors.</span></span> <span data-ttu-id="b24c2-1097">此过程称为***内联***，它使属性访问与字段访问一样高效，同时还保留了属性的灵活性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1097">This process is known as ***inlining***, and it makes property access as efficient as field access, yet preserves the increased flexibility of properties.</span></span>

<span data-ttu-id="b24c2-1098">由于调用 `get` 访问器在概念上等同于读取字段的值，因此 `get` 访问器的编程样式被视为不正确的编程样式，使其具有可观察的副作用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1098">Since invoking a `get` accessor is conceptually equivalent to reading the value of a field, it is considered bad programming style for `get` accessors to have observable side-effects.</span></span> <span data-ttu-id="b24c2-1099">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-1099">In the example</span></span>
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
<span data-ttu-id="b24c2-1100">`Next` 属性的值取决于属性以前被访问的次数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1100">the value of the `Next` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="b24c2-1101">因此，访问属性会产生一个可观察到的副作用，而属性应作为方法实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1101">Thus, accessing the property produces an observable side-effect, and the property should be implemented as a method instead.</span></span>

<span data-ttu-id="b24c2-1102">`get` 访问器的 "无副作用" 约定并不意味着应始终将 `get` 访问器编写为仅返回存储在字段中的值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1102">The "no side-effects" convention for `get` accessors doesn't mean that `get` accessors should always be written to simply return values stored in fields.</span></span> <span data-ttu-id="b24c2-1103">事实上，`get` 访问器通常通过访问多个字段或调用方法来计算属性的值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1103">Indeed, `get` accessors often compute the value of a property by accessing multiple fields or invoking methods.</span></span> <span data-ttu-id="b24c2-1104">但是，正确设计的 `get` 访问器不会执行任何操作，从而导致对象的状态发生变化。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1104">However, a properly designed `get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="b24c2-1105">在第一次引用资源之前，可以使用属性来延迟资源的初始化。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1105">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="b24c2-1106">例如：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1106">For example:</span></span>
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

<span data-ttu-id="b24c2-1107">`Console` 类包含三个属性： `In`、`Out`和 `Error`，分别表示标准输入、输出和错误设备。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1107">The `Console` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="b24c2-1108">通过将这些成员作为属性公开，`Console` 类可以延迟其初始化，直到实际使用它们。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1108">By exposing these members as properties, the `Console` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="b24c2-1109">例如，在第一次引用 `Out` 属性时，如下所示</span><span class="sxs-lookup"><span data-stu-id="b24c2-1109">For example, upon first referencing the `Out` property, as in</span></span>
```csharp
Console.Out.WriteLine("hello, world");
```
<span data-ttu-id="b24c2-1110">将创建输出设备的基础 `TextWriter`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1110">the underlying `TextWriter` for the output device is created.</span></span> <span data-ttu-id="b24c2-1111">但如果应用程序不引用 `In` 和 `Error` 属性，则不会为这些设备创建任何对象。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1111">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="b24c2-1112">自动实现的属性</span><span class="sxs-lookup"><span data-stu-id="b24c2-1112">Automatically implemented properties</span></span>

<span data-ttu-id="b24c2-1113">自动实现的属性（short 的***自动属性***）是具有仅包含分号的访问器正文的非抽象非 extern 属性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1113">An automatically implemented property (or ***auto-property*** for short), is a non-abstract non-extern property with semicolon-only accessor bodies.</span></span> <span data-ttu-id="b24c2-1114">自动属性必须具有 get 访问器，并且可以选择包含 set 访问器。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1114">Auto-properties must have a get accessor and can optionally have a set accessor.</span></span>

<span data-ttu-id="b24c2-1115">当属性指定为自动实现的属性时，隐藏的支持字段将自动提供给该属性，并实现访问器来读取和写入该支持字段。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1115">When a property is specified as an automatically implemented property, a hidden backing field is automatically available for the property, and the accessors are implemented to read from and write to that backing field.</span></span> <span data-ttu-id="b24c2-1116">如果 auto 属性没有 set 访问器，则会将支持字段视为 `readonly` （[Readonly 字段](classes.md#readonly-fields)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1116">If the auto-property has no set accessor, the backing field is considered `readonly` ([Readonly fields](classes.md#readonly-fields)).</span></span> <span data-ttu-id="b24c2-1117">与 `readonly` 字段一样，仅 getter 自动属性也可在封闭类的构造函数的主体中分配给。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1117">Just like a `readonly` field, a getter-only auto-property can also be assigned to in the body of a constructor of the enclosing class.</span></span> <span data-ttu-id="b24c2-1118">此类赋值直接分配给属性的 readonly 支持字段。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1118">Such an assignment assigns directly to the readonly backing field of the property.</span></span>

<span data-ttu-id="b24c2-1119">自动属性可以有选择地具有*property_initializer*，该属性作为*variable_initializer* （[变量初始值设定项](classes.md#variable-initializers)）直接应用于支持字段。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1119">An auto-property may optionally have a *property_initializer*, which is applied directly to the backing field as a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)).</span></span>

<span data-ttu-id="b24c2-1120">如下示例中：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1120">The following example:</span></span>
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
<span data-ttu-id="b24c2-1121">等效于以下声明：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1121">is equivalent to the following declaration:</span></span>
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

<span data-ttu-id="b24c2-1122">如下示例中：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1122">The following example:</span></span>
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
<span data-ttu-id="b24c2-1123">等效于以下声明：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1123">is equivalent to the following declaration:</span></span>
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

<span data-ttu-id="b24c2-1124">请注意，只读字段的赋值是合法的，因为它们发生在构造函数中。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1124">Notice that the assignments to the readonly field are legal, because they occur within the constructor.</span></span>


### <a name="accessibility"></a><span data-ttu-id="b24c2-1125">辅助功能</span><span class="sxs-lookup"><span data-stu-id="b24c2-1125">Accessibility</span></span>

<span data-ttu-id="b24c2-1126">如果访问器具有*accessor_modifier*，则访问器的可访问性域（[可访问](basic-concepts.md#accessibility-domains)域）是使用*accessor_modifier*的声明的可访问性确定的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1126">If an accessor has an *accessor_modifier*, the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the accessor is determined using the declared accessibility of the *accessor_modifier*.</span></span> <span data-ttu-id="b24c2-1127">如果访问器没有*accessor_modifier*，则访问器的可访问性域由属性或索引器的声明的可访问性决定。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1127">If an accessor does not have an *accessor_modifier*, the accessibility domain of the accessor is determined from the declared accessibility of the property or indexer.</span></span>

<span data-ttu-id="b24c2-1128">*Accessor_modifier*的存在从不影响成员查找（[运算符](expressions.md#operators)）或重载决策（[重载决策](expressions.md#overload-resolution)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1128">The presence of an *accessor_modifier* never affects member lookup ([Operators](expressions.md#operators)) or overload resolution ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="b24c2-1129">属性或索引器的修饰符始终决定绑定到的属性或索引器，而不考虑访问的上下文。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1129">The modifiers on the property or indexer always determine which property or indexer is bound to, regardless of the context of the access.</span></span>

<span data-ttu-id="b24c2-1130">选择特定属性或索引器后，所涉及的特定访问器的可访问域将用于确定此使用是否有效：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1130">Once a particular property or indexer has been selected, the accessibility domains of the specific accessors involved are used to determine if that usage is valid:</span></span>

*  <span data-ttu-id="b24c2-1131">如果用法为值（[表达式的值](expressions.md#values-of-expressions)），则 `get` 访问器必须存在并且可访问。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1131">If the usage is as a value ([Values of expressions](expressions.md#values-of-expressions)), the `get` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="b24c2-1132">如果使用情况是作为简单赋值的目标（[简单分配](expressions.md#simple-assignment)），则 `set` 访问器必须存在并且可访问。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1132">If the usage is as the target of a simple assignment ([Simple assignment](expressions.md#simple-assignment)), the `set` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="b24c2-1133">如果使用情况是作为复合赋值（[复合赋值](expressions.md#compound-assignment)）的目标，或者作为 `++` 或 `--` 运算符（[函数成员](expressions.md#function-members).9、[调用表达式](expressions.md#invocation-expressions)）的目标，则 `get` 访问器和 `set` 访问器都必须存在并且可访问。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1133">If the usage is as the target of compound assignment ([Compound assignment](expressions.md#compound-assignment)), or as the target of the `++` or `--` operators ([Function members](expressions.md#function-members).9, [Invocation expressions](expressions.md#invocation-expressions)), both the `get` accessors and the `set` accessor must exist and be accessible.</span></span>

<span data-ttu-id="b24c2-1134">在下面的示例中，属性 `B.Text``A.Text` 隐藏属性，即使仅在调用 `set` 访问器的上下文中也是如此。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1134">In the following example, the property `A.Text` is hidden by the property `B.Text`, even in contexts where only the `set` accessor is called.</span></span> <span data-ttu-id="b24c2-1135">相反，类 `M`无法访问属性 `B.Count`，因此改用 `A.Count` 的可访问属性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1135">In contrast, the property `B.Count` is not accessible to class `M`, so the accessible property `A.Count` is used instead.</span></span>

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

<span data-ttu-id="b24c2-1136">用于实现接口的访问器可能没有*accessor_modifier*。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1136">An accessor that is used to implement an interface may not have an *accessor_modifier*.</span></span> <span data-ttu-id="b24c2-1137">如果只使用一个访问器来实现接口，则可以使用*accessor_modifier*声明另一个访问器：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1137">If only one accessor is used to implement an interface, the other accessor may be declared with an *accessor_modifier*:</span></span>
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public string Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a><span data-ttu-id="b24c2-1138">虚拟、密封、重写和抽象属性访问器</span><span class="sxs-lookup"><span data-stu-id="b24c2-1138">Virtual, sealed, override, and abstract property accessors</span></span>

<span data-ttu-id="b24c2-1139">`virtual` 属性声明指定属性的访问器是虚拟的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1139">A `virtual` property declaration specifies that the accessors of the property are virtual.</span></span> <span data-ttu-id="b24c2-1140">`virtual` 修饰符适用于读写属性的这两个访问器，即读写属性的一个取值函数是虚拟的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1140">The `virtual` modifier applies to both accessors of a read-write property—it is not possible for only one accessor of a read-write property to be virtual.</span></span>

<span data-ttu-id="b24c2-1141">`abstract` 属性声明指定属性的访问器是虚拟的，但是不提供访问器的实际实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1141">An `abstract` property declaration specifies that the accessors of the property are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="b24c2-1142">相反，非抽象派生类需要通过重写属性来为访问器提供自己的实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1142">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the property.</span></span> <span data-ttu-id="b24c2-1143">因为抽象属性声明的访问器不提供实际的实现，所以其*accessor_body*只包含一个分号。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1143">Because an accessor for an abstract property declaration provides no actual implementation, its *accessor_body* simply consists of a semicolon.</span></span>

<span data-ttu-id="b24c2-1144">同时包含 `abstract` 和 `override` 修饰符的属性声明指定属性是抽象的并且会重写基属性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1144">A property declaration that includes both the `abstract` and `override` modifiers specifies that the property is abstract and overrides a base property.</span></span> <span data-ttu-id="b24c2-1145">此类属性的访问器也是抽象的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1145">The accessors of such a property are also abstract.</span></span>

<span data-ttu-id="b24c2-1146">仅在抽象类（[抽象类](classes.md#abstract-classes)）中允许抽象属性声明。通过包括指定 `override` 指令的属性声明，可在派生类中重写继承的虚拟属性的访问器。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1146">Abstract property declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).The accessors of an inherited virtual property can be overridden in a derived class by including a property declaration that specifies an `override` directive.</span></span> <span data-ttu-id="b24c2-1147">这称为 "***重写" 属性声明***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1147">This is known as an ***overriding property declaration***.</span></span> <span data-ttu-id="b24c2-1148">重写属性声明未声明新的属性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1148">An overriding property declaration does not declare a new property.</span></span> <span data-ttu-id="b24c2-1149">相反，它只是专用化现有虚拟属性的访问器的实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1149">Instead, it simply specializes the implementations of the accessors of an existing virtual property.</span></span>

<span data-ttu-id="b24c2-1150">重写的属性声明必须将完全相同的可访问性修饰符、类型和名称指定为继承的属性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1150">An overriding property declaration must specify the exact same accessibility modifiers, type, and name as the inherited property.</span></span> <span data-ttu-id="b24c2-1151">如果继承的属性只有一个访问器（即，如果继承的属性为只读或只写），则重写属性必须仅包含该访问器。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1151">If the inherited property has only a single accessor (i.e., if the inherited property is read-only or write-only), the overriding property must include only that accessor.</span></span> <span data-ttu-id="b24c2-1152">如果继承的属性包含两个访问器（即，如果继承的属性是读写的），则重写属性可以包含单个访问器或两个访问器。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1152">If the inherited property includes both accessors (i.e., if the inherited property is read-write), the overriding property can include either a single accessor or both accessors.</span></span>

<span data-ttu-id="b24c2-1153">重写的属性声明可以包括 `sealed` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1153">An overriding property declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="b24c2-1154">使用此修饰符可防止派生类进一步重写属性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1154">Use of this modifier prevents a derived class from further overriding the property.</span></span> <span data-ttu-id="b24c2-1155">密封属性的访问器也是密封的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1155">The accessors of a sealed property are also sealed.</span></span>

<span data-ttu-id="b24c2-1156">除了声明和调用语法中的差异外，virtual、sealed、override 和 abstract 访问器的行为与虚拟、密封、重写和抽象方法完全相同。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1156">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="b24c2-1157">具体而言，在[虚拟方法](classes.md#virtual-methods)、[替代方法](classes.md#override-methods)、[密封方法](classes.md#sealed-methods)和[抽象方法](classes.md#abstract-methods)中描述的规则将应用为：访问器是相应形式的方法：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1157">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form:</span></span>

*  <span data-ttu-id="b24c2-1158">`get` 访问器对应于一个无参数方法，该方法具有属性类型的返回值以及与包含属性相同的修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1158">A `get` accessor corresponds to a parameterless method with a return value of the property type and the same modifiers as the containing property.</span></span>
*  <span data-ttu-id="b24c2-1159">`set` 访问器对应于一个方法，该方法具有属性类型的单个值参数、一个 `void` 返回类型，以及与包含属性相同的修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1159">A `set` accessor corresponds to a method with a single value parameter of the property type, a `void` return type, and the same modifiers as the containing property.</span></span>

<span data-ttu-id="b24c2-1160">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-1160">In the example</span></span>
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
<span data-ttu-id="b24c2-1161">`X` 是虚拟的只读属性，`Y` 为虚拟读写属性，`Z` 是抽象的读写属性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1161">`X` is a virtual read-only property, `Y` is a virtual read-write property, and `Z` is an abstract read-write property.</span></span> <span data-ttu-id="b24c2-1162">由于 `Z` 是抽象的，因此，包含类 `A` 也必须声明为抽象类。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1162">Because `Z` is abstract, the containing class `A` must also be declared abstract.</span></span>

<span data-ttu-id="b24c2-1163">下面显示了一个派生自 `A` 的类：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1163">A class that derives from `A` is show below:</span></span>
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

<span data-ttu-id="b24c2-1164">此处，`X`、`Y`和 `Z` 的声明都是重写属性声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1164">Here, the declarations of `X`, `Y`, and `Z` are overriding property declarations.</span></span> <span data-ttu-id="b24c2-1165">每个属性声明都与相应的继承属性的可访问性修饰符、类型和名称完全匹配。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1165">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="b24c2-1166">`X` 的 `get` 访问器和 `Y` 的 `set` 访问器使用 `base` 关键字访问继承的访问器。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1166">The `get` accessor of `X` and the `set` accessor of `Y` use the `base` keyword to access the inherited accessors.</span></span> <span data-ttu-id="b24c2-1167">`Z` 的声明会重写抽象访问器，因此，`B`中没有未完成的抽象函数成员，并且允许 `B` 为非抽象类。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1167">The declaration of `Z` overrides both abstract accessors—thus, there are no outstanding abstract function members in `B`, and `B` is permitted to be a non-abstract class.</span></span>

<span data-ttu-id="b24c2-1168">当某个属性声明为 `override`时，重写的代码必须能够访问所有重写的访问器。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1168">When a property is declared as an `override`, any overridden accessors must be accessible to the overriding code.</span></span> <span data-ttu-id="b24c2-1169">此外，属性或索引器本身以及访问器的声明的可访问性必须与重写的成员和访问器的可访问性匹配。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1169">In addition, the declared accessibility of both the property or indexer itself, and of the accessors, must match that of the overridden member and accessors.</span></span> <span data-ttu-id="b24c2-1170">例如：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1170">For example:</span></span>
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

## <a name="events"></a><span data-ttu-id="b24c2-1171">事件</span><span class="sxs-lookup"><span data-stu-id="b24c2-1171">Events</span></span>

<span data-ttu-id="b24c2-1172">***事件***是允许对象或类提供通知的成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1172">An ***event*** is a member that enables an object or class to provide notifications.</span></span> <span data-ttu-id="b24c2-1173">客户端可以通过提供***事件处理程序***来附加事件的可执行代码。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1173">Clients can attach executable code for events by supplying ***event handlers***.</span></span>

<span data-ttu-id="b24c2-1174">使用*event_declaration*s 声明事件：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1174">Events are declared using *event_declaration*s:</span></span>

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

<span data-ttu-id="b24c2-1175">*Event_declaration*可能包括一组*属性*（[特性](attributes.md)）和四个访问修饰符（[访问修饰符](classes.md#access-modifiers)）、`new` （[新修饰符](classes.md#the-new-modifier)）、`static` （[静态和实例方法](classes.md#static-and-instance-methods)）、`virtual` （[虚拟方法](classes.md#virtual-methods)）、`override` （[重写方法](classes.md#override-methods)）、`sealed` （[密封方法](classes.md#sealed-methods)）、`abstract` （[抽象方法](classes.md#abstract-methods)）和 `extern` （[外部方法](classes.md#external-methods)）修饰符的有效组合。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1175">An *event_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="b24c2-1176">事件声明遵循与方法声明（[方法](classes.md#methods)）相同的规则，与修饰符的有效组合有关。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1176">Event declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="b24c2-1177">事件声明的*类型*必须是*delegate_type* （[引用类型](types.md#reference-types)），并且*delegate_type*必须至少与事件本身具有相同的可访问性（[可访问性约束](basic-concepts.md#accessibility-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1177">The *type* of an event declaration must be a *delegate_type* ([Reference types](types.md#reference-types)), and that *delegate_type* must be at least as accessible as the event itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="b24c2-1178">事件声明可能包括*event_accessor_declarations*。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1178">An event declaration may include *event_accessor_declarations*.</span></span> <span data-ttu-id="b24c2-1179">但是，如果不是，则对于非 extern 非抽象事件，编译器会自动提供这些事件（[类似于字段的事件](classes.md#field-like-events)）;对于 extern 事件，访问器是在外部提供的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1179">However, if it does not, for non-extern, non-abstract events, the compiler supplies them automatically ([Field-like events](classes.md#field-like-events)); for extern events, the accessors are provided externally.</span></span>

<span data-ttu-id="b24c2-1180">*Event_accessor_declarations*省略的事件声明定义了一个或多个事件，每个*variable_declarator*一个事件。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1180">An event declaration that omits *event_accessor_declarations* defines one or more events—one for each of the *variable_declarator*s.</span></span> <span data-ttu-id="b24c2-1181">特性和修饰符适用于由此类*event_declaration*声明的所有成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1181">The attributes and modifiers apply to all of the members declared by such an *event_declaration*.</span></span>

<span data-ttu-id="b24c2-1182">*Event_declaration*包括 `abstract` 修饰符和用大括号分隔的*event_accessor_declarations*，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1182">It is a compile-time error for an *event_declaration* to include both the `abstract` modifier and brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="b24c2-1183">当事件声明包含 `extern` 修饰符时，该事件被称为***外部事件***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1183">When an event declaration includes an `extern` modifier, the event is said to be an ***external event***.</span></span> <span data-ttu-id="b24c2-1184">由于外部事件声明不提供实际的实现，因此它可以同时包含 `extern` 修饰符和*event_accessor_declarations*。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1184">Because an external event declaration provides no actual implementation, it is an error for it to include both the `extern` modifier and *event_accessor_declarations*.</span></span>

<span data-ttu-id="b24c2-1185">使用 `abstract` 或 `external` 修饰符来包含*variable_initializer*时，事件声明的*variable_declarator*会出现编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1185">It is a compile-time error for a *variable_declarator* of an event declaration with an `abstract` or `external` modifier to include a *variable_initializer*.</span></span>

<span data-ttu-id="b24c2-1186">事件可用作 `+=` 和 `-=` 运算符（[事件分配](expressions.md#event-assignment)）的左操作数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1186">An event can be used as the left-hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="b24c2-1187">这些运算符分别用于将事件处理程序附加到事件处理程序或从事件中删除事件处理程序，事件的访问修饰符控制允许此类操作的上下文。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1187">These operators are used, respectively, to attach event handlers to or to remove event handlers from an event, and the access modifiers of the event control the contexts in which such operations are permitted.</span></span>

<span data-ttu-id="b24c2-1188">由于 `+=` 和 `-=` 是对声明事件的类型之外的事件允许的唯一操作，因此外部代码可以添加和移除事件的处理程序，但不能以任何其他方式获取或修改事件处理程序的基础列表。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1188">Since `+=` and `-=` are the only operations that are permitted on an event outside the type that declares the event, external code can add and remove handlers for an event, but cannot in any other way obtain or modify the underlying list of event handlers.</span></span>

<span data-ttu-id="b24c2-1189">在 `x += y` 或 `x -= y`形式的操作中，当 `x` 为事件并且引用出现在包含 `x`声明的类型之外时，操作的结果将具有类型 `void` （与具有 `x`的类型相反，而在赋值后，`x` 的值为。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1189">In an operation of the form `x += y` or `x -= y`, when `x` is an event and the reference takes place outside the type that contains the declaration of `x`, the result of the operation has type `void` (as opposed to having the type of `x`, with the value of `x` after the assignment).</span></span> <span data-ttu-id="b24c2-1190">此规则禁止外部代码间接检查事件的基础委托。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1190">This rule prohibits external code from indirectly examining the underlying delegate of an event.</span></span>

<span data-ttu-id="b24c2-1191">下面的示例演示如何将事件处理程序附加到 `Button` 类的实例：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1191">The following example shows how event handlers are attached to instances of the `Button` class:</span></span>
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

<span data-ttu-id="b24c2-1192">此处，`LoginDialog` 实例构造函数创建两个 `Button` 实例，并将事件处理程序附加到 `Click` 事件。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1192">Here, the `LoginDialog` instance constructor creates two `Button` instances and attaches event handlers to the `Click` events.</span></span>

### <a name="field-like-events"></a><span data-ttu-id="b24c2-1193">类似字段的事件</span><span class="sxs-lookup"><span data-stu-id="b24c2-1193">Field-like events</span></span>

<span data-ttu-id="b24c2-1194">在包含事件声明的类或结构的程序文本中，某些事件可以像字段一样使用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1194">Within the program text of the class or struct that contains the declaration of an event, certain events can be used like fields.</span></span> <span data-ttu-id="b24c2-1195">若要以这种方式使用，事件不得 `abstract` 或 `extern`，而且不能显式包含*event_accessor_declarations*。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1195">To be used in this way, an event must not be `abstract` or `extern`, and must not explicitly include *event_accessor_declarations*.</span></span> <span data-ttu-id="b24c2-1196">此类事件可用于任何允许字段的上下文中。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1196">Such an event can be used in any context that permits a field.</span></span> <span data-ttu-id="b24c2-1197">该字段包含一个委托（[委托](delegates.md)），表示已添加到事件的事件处理程序的列表。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1197">The field contains a delegate ([Delegates](delegates.md)) which refers to the list of event handlers that have been added to the event.</span></span> <span data-ttu-id="b24c2-1198">如果未添加任何事件处理程序，则字段将包含 `null`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1198">If no event handlers have been added, the field contains `null`.</span></span>

<span data-ttu-id="b24c2-1199">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-1199">In the example</span></span>
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
<span data-ttu-id="b24c2-1200">`Click` 用作 `Button` 类中的字段。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1200">`Click` is used as a field within the `Button` class.</span></span> <span data-ttu-id="b24c2-1201">如示例所示，可以检查、修改字段，并将其用于委托调用表达式。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1201">As the example demonstrates, the field can be examined, modified, and used in delegate invocation expressions.</span></span> <span data-ttu-id="b24c2-1202">`Button` 类中的 `OnClick` 方法 "引发" `Click` 事件。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1202">The `OnClick` method in the `Button` class "raises" the `Click` event.</span></span> <span data-ttu-id="b24c2-1203">引发事件的概念恰恰等同于调用由事件表示的委托，因此，没有用于引发事件的特殊语言构造。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1203">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span> <span data-ttu-id="b24c2-1204">请注意，委托调用前面有一个检查，该检查可确保委托为非 null。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1204">Note that the delegate invocation is preceded by a check that ensures the delegate is non-null.</span></span>

<span data-ttu-id="b24c2-1205">在 `Button` 类的声明外部，`Click` 成员只能在 `+=` 和 `-=` 运算符的左侧使用，如下所示</span><span class="sxs-lookup"><span data-stu-id="b24c2-1205">Outside the declaration of the `Button` class, the `Click` member can only be used on the left-hand side of the `+=` and `-=` operators, as in</span></span>
```csharp
b.Click += new EventHandler(...);
```
<span data-ttu-id="b24c2-1206">它将一个委托追加到 `Click` 事件的调用列表中，并</span><span class="sxs-lookup"><span data-stu-id="b24c2-1206">which appends a delegate to the invocation list of the `Click` event, and</span></span>
```csharp
b.Click -= new EventHandler(...);
```
<span data-ttu-id="b24c2-1207">这将从 `Click` 事件的调用列表中移除委托。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1207">which removes a delegate from the invocation list of the `Click` event.</span></span>

<span data-ttu-id="b24c2-1208">在编译类似于字段的事件时，编译器会自动创建存储来保存委托，并为事件创建访问器，以便在委托字段中添加或删除事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1208">When compiling a field-like event, the compiler automatically creates storage to hold the delegate, and creates accessors for the event that add or remove event handlers to the delegate field.</span></span> <span data-ttu-id="b24c2-1209">添加和移除操作是线程安全的，并且在持有实例事件的包含对象的锁（[lock 语句](statements.md#the-lock-statement)）或静态事件的类型对象（[匿名对象创建表达式](expressions.md#anonymous-object-creation-expressions)）的情况下，可能（但不需要）执行此操作。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1209">The addition and removal operations are thread safe, and may (but are not required to) be done while holding the lock ([The lock statement](statements.md#the-lock-statement)) on the containing object for an instance event, or the type object ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) for a static event.</span></span>

<span data-ttu-id="b24c2-1210">因此，以下形式的实例事件声明：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1210">Thus, an instance event declaration of the form:</span></span>
```csharp
class X
{
    public event D Ev;
}
```
<span data-ttu-id="b24c2-1211">将编译为与等效的对象：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1211">will be compiled to something equivalent to:</span></span>
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
<span data-ttu-id="b24c2-1212">在类 `X`中，对 `+=` 和 `-=` 运算符左侧 `Ev` 的引用会导致调用 add 和 remove 访问器。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1212">Within the class `X`, references to `Ev` on the left-hand side of the `+=` and `-=` operators cause the add and remove accessors to be invoked.</span></span> <span data-ttu-id="b24c2-1213">对 `Ev` 的所有其他引用将被编译为引用隐藏字段（[成员访问](expressions.md#member-access)） `__Ev`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1213">All other references to `Ev` are compiled to reference the hidden field `__Ev` instead ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="b24c2-1214">名称 "`__Ev`" 是任意的;隐藏的字段可能有任何名称，或者根本没有名称。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1214">The name "`__Ev`" is arbitrary; the hidden field could have any name or no name at all.</span></span>

### <a name="event-accessors"></a><span data-ttu-id="b24c2-1215">事件访问器</span><span class="sxs-lookup"><span data-stu-id="b24c2-1215">Event accessors</span></span>

<span data-ttu-id="b24c2-1216">事件声明通常会忽略*event_accessor_declarations*，如上面的 `Button` 示例中所示。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1216">Event declarations typically omit *event_accessor_declarations*, as in the `Button` example above.</span></span> <span data-ttu-id="b24c2-1217">执行此操作的一种情况是，这种情况涉及到每个事件的一个字段的存储成本是不可接受的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1217">One situation for doing so involves the case in which the storage cost of one field per event is not acceptable.</span></span> <span data-ttu-id="b24c2-1218">在这种情况下，类可以包括*event_accessor_declarations* ，并使用专用机制来存储事件处理程序列表。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1218">In such cases, a class can include *event_accessor_declarations* and use a private mechanism for storing the list of event handlers.</span></span>

<span data-ttu-id="b24c2-1219">事件的*event_accessor_declarations*指定与添加和移除事件处理程序相关联的可执行语句。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1219">The *event_accessor_declarations* of an event specify the executable statements associated with adding and removing event handlers.</span></span>

<span data-ttu-id="b24c2-1220">访问器声明由*add_accessor_declaration*和*remove_accessor_declaration*组成。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1220">The accessor declarations consist of an *add_accessor_declaration* and a *remove_accessor_declaration*.</span></span> <span data-ttu-id="b24c2-1221">每个访问器声明都包含标记 `add` 或 `remove` 后跟一个*块*。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1221">Each accessor declaration consists of the token `add` or `remove` followed by a *block*.</span></span> <span data-ttu-id="b24c2-1222">与*add_accessor_declaration*关联的*块*指定在添加事件处理程序时要执行的语句，与*remove_accessor_declaration*关联的*块*指定在删除事件处理程序时要执行的语句。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1222">The *block* associated with an *add_accessor_declaration* specifies the statements to execute when an event handler is added, and the *block* associated with a *remove_accessor_declaration* specifies the statements to execute when an event handler is removed.</span></span>

<span data-ttu-id="b24c2-1223">每个*add_accessor_declaration*和*remove_accessor_declaration*对应于一个方法，其中包含事件类型的单个值参数和一个 `void` 返回类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1223">Each *add_accessor_declaration* and *remove_accessor_declaration* corresponds to a method with a single value parameter of the event type and a `void` return type.</span></span> <span data-ttu-id="b24c2-1224">事件访问器的隐式参数名为 `value`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1224">The implicit parameter of an event accessor is named `value`.</span></span> <span data-ttu-id="b24c2-1225">在事件分配中使用事件时，将使用适当的事件访问器。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1225">When an event is used in an event assignment, the appropriate event accessor is used.</span></span> <span data-ttu-id="b24c2-1226">具体而言，如果 `+=` 赋值运算符，则使用 add 访问器，如果赋值运算符 `-=`，则使用 remove 访问器。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1226">Specifically, if the assignment operator is `+=` then the add accessor is used, and if the assignment operator is `-=` then the remove accessor is used.</span></span> <span data-ttu-id="b24c2-1227">在任一情况下，赋值运算符的右操作数用作事件访问器的参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1227">In either case, the right-hand operand of the assignment operator is used as the argument to the event accessor.</span></span> <span data-ttu-id="b24c2-1228">*Add_accessor_declaration*或*remove_accessor_declaration*的块必须符合[方法体](classes.md#method-body)中所述的 `void` 方法的规则。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1228">The block of an *add_accessor_declaration* or a *remove_accessor_declaration* must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="b24c2-1229">特别是，不允许此类块中的 `return` 语句指定表达式。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1229">In particular, `return` statements in such a block are not permitted to specify an expression.</span></span>

<span data-ttu-id="b24c2-1230">由于事件访问器隐式具有名为 `value`的参数，因此在事件访问器中声明的局部变量或常量具有该名称是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1230">Since an event accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declared in an event accessor to have that name.</span></span>

<span data-ttu-id="b24c2-1231">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-1231">In the example</span></span>
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
<span data-ttu-id="b24c2-1232">`Control` 类实现事件的内部存储机制。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1232">the `Control` class implements an internal storage mechanism for events.</span></span> <span data-ttu-id="b24c2-1233">`AddEventHandler` 方法将委托值与某个键相关联，`GetEventHandler` 方法返回当前与某个键相关联的委托，并且 `RemoveEventHandler` 方法将委托作为指定事件的事件处理程序移除。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1233">The `AddEventHandler` method associates a delegate value with a key, the `GetEventHandler` method returns the delegate currently associated with a key, and the `RemoveEventHandler` method removes a delegate as an event handler for the specified event.</span></span> <span data-ttu-id="b24c2-1234">可能的基础存储机制的设计使 `null` 委托值与密钥关联不会产生费用，因此未处理的事件不会消耗任何存储。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1234">Presumably, the underlying storage mechanism is designed such that there is no cost for associating a `null` delegate value with a key, and thus unhandled events consume no storage.</span></span>

### <a name="static-and-instance-events"></a><span data-ttu-id="b24c2-1235">静态和实例事件</span><span class="sxs-lookup"><span data-stu-id="b24c2-1235">Static and instance events</span></span>

<span data-ttu-id="b24c2-1236">当事件声明包含 `static` 修饰符时，该事件被称为***静态事件***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1236">When an event declaration includes a `static` modifier, the event is said to be a ***static event***.</span></span> <span data-ttu-id="b24c2-1237">如果不存在任何 `static` 修饰符，则该事件被称为***实例事件***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1237">When no `static` modifier is present, the event is said to be an ***instance event***.</span></span>

<span data-ttu-id="b24c2-1238">静态事件不与特定实例相关联，并且是在静态事件的访问器中引用 `this` 的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1238">A static event is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static event.</span></span>

<span data-ttu-id="b24c2-1239">实例事件与类的给定实例相关联，并且可以在该事件的访问器中将此实例作为 `this` （[此访问](expressions.md#this-access)）进行访问。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1239">An instance event is associated with a given instance of a class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that event.</span></span>

<span data-ttu-id="b24c2-1240">当 `E.M`形式的*member_access* （[成员访问](expressions.md#member-access)）中引用事件时，如果 `M` 为静态事件，则 `E` 必须表示包含 `M`的类型，如果 `M` 是实例事件，则 E 必须表示包含 `M`的类型的实例。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1240">When an event is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static event, `E` must denote a type containing `M`, and if `M` is an instance event, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="b24c2-1241">静态成员和实例成员之间的差异在[静态成员和实例成员](classes.md#static-and-instance-members)中进行了进一步讨论。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1241">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a><span data-ttu-id="b24c2-1242">虚拟、密封、重写和抽象事件访问器</span><span class="sxs-lookup"><span data-stu-id="b24c2-1242">Virtual, sealed, override, and abstract event accessors</span></span>

<span data-ttu-id="b24c2-1243">`virtual` 事件声明指定该事件的访问器是虚拟的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1243">A `virtual` event declaration specifies that the accessors of that event are virtual.</span></span> <span data-ttu-id="b24c2-1244">`virtual` 修饰符适用于事件的两个访问器。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1244">The `virtual` modifier applies to both accessors of an event.</span></span>

<span data-ttu-id="b24c2-1245">`abstract` 事件声明指定事件的访问器是虚拟的，但不提供访问器的实际实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1245">An `abstract` event declaration specifies that the accessors of the event are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="b24c2-1246">相反，非抽象派生类需要通过重写事件为访问器提供自己的实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1246">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the event.</span></span> <span data-ttu-id="b24c2-1247">因为抽象事件声明不提供实际的实现，所以它不能提供用括号分隔的*event_accessor_declarations*。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1247">Because an abstract event declaration provides no actual implementation, it cannot provide brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="b24c2-1248">同时包含 `abstract` 和 `override` 修饰符的事件声明指定事件是抽象的并且会重写基事件。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1248">An event declaration that includes both the `abstract` and `override` modifiers specifies that the event is abstract and overrides a base event.</span></span> <span data-ttu-id="b24c2-1249">此类事件的访问器也是抽象的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1249">The accessors of such an event are also abstract.</span></span>

<span data-ttu-id="b24c2-1250">仅在抽象类（[抽象类](classes.md#abstract-classes)）中允许抽象事件声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1250">Abstract event declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="b24c2-1251">通过包括指定 `override` 修饰符的事件声明，可在派生类中重写继承的虚拟事件的访问器。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1251">The accessors of an inherited virtual event can be overridden in a derived class by including an event declaration that specifies an `override` modifier.</span></span> <span data-ttu-id="b24c2-1252">这称为***重写事件声明***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1252">This is known as an ***overriding event declaration***.</span></span> <span data-ttu-id="b24c2-1253">重写事件声明未声明新事件。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1253">An overriding event declaration does not declare a new event.</span></span> <span data-ttu-id="b24c2-1254">相反，它只是专用化现有虚拟事件的访问器的实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1254">Instead, it simply specializes the implementations of the accessors of an existing virtual event.</span></span>

<span data-ttu-id="b24c2-1255">重写事件声明必须指定与重写事件完全相同的可访问性修饰符、类型和名称。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1255">An overriding event declaration must specify the exact same accessibility modifiers, type, and name as the overridden event.</span></span>

<span data-ttu-id="b24c2-1256">重写事件声明可能包括 `sealed` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1256">An overriding event declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="b24c2-1257">使用此修饰符可防止派生类进一步重写事件。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1257">Use of this modifier prevents a derived class from further overriding the event.</span></span> <span data-ttu-id="b24c2-1258">密封事件的访问器也是密封的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1258">The accessors of a sealed event are also sealed.</span></span>

<span data-ttu-id="b24c2-1259">重写事件声明包含 `new` 修饰符是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1259">It is a compile-time error for an overriding event declaration to include a `new` modifier.</span></span>

<span data-ttu-id="b24c2-1260">除了声明和调用语法中的差异外，virtual、sealed、override 和 abstract 访问器的行为与虚拟、密封、重写和抽象方法完全相同。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1260">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="b24c2-1261">具体而言，在[虚拟方法](classes.md#virtual-methods)、[重写方法](classes.md#override-methods)、[密封方法](classes.md#sealed-methods)和[抽象方法](classes.md#abstract-methods)中描述的规则就像访问器是相应窗体的方法一样。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1261">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form.</span></span> <span data-ttu-id="b24c2-1262">每个访问器都对应于一个方法，该方法具有事件类型的单个值参数、一个 `void` 返回类型，以及与包含事件相同的修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1262">Each accessor corresponds to a method with a single value parameter of the event type, a `void` return type, and the same modifiers as the containing event.</span></span>

## <a name="indexers"></a><span data-ttu-id="b24c2-1263">索引器</span><span class="sxs-lookup"><span data-stu-id="b24c2-1263">Indexers</span></span>

<span data-ttu-id="b24c2-1264">***索引器***是一个成员，使对象能够以与数组相同的方式进行索引。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1264">An ***indexer*** is a member that enables an object to be indexed in the same way as an array.</span></span> <span data-ttu-id="b24c2-1265">使用*indexer_declaration*s 声明索引器：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1265">Indexers are declared using *indexer_declaration*s:</span></span>

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

<span data-ttu-id="b24c2-1266">*Indexer_declaration*可能包括一组*特性*（[特性](attributes.md)）和四个访问修饰符（[访问修饰符](classes.md#access-modifiers)）、`new` （[新修饰符](classes.md#the-new-modifier)）、`virtual` （[虚拟方法](classes.md#virtual-methods)）、`override` （[重写方法](classes.md#override-methods)）、`sealed` （[密封方法](classes.md#sealed-methods)）、`abstract` （[抽象方法](classes.md#abstract-methods)）和 `extern` （[外部方法](classes.md#external-methods)）修饰符的有效组合。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1266">An *indexer_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="b24c2-1267">索引器声明遵循与方法声明（[方法](classes.md#methods)）相同的规则，这些修饰符与修饰符的有效组合有关，但有一个例外，即索引器声明上不允许使用 static 修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1267">Indexer declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers, with the one exception being that the static modifier is not permitted on an indexer declaration.</span></span>

<span data-ttu-id="b24c2-1268">除了一种情况外，修饰符 `virtual`、`override`和 `abstract` 都是互斥的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1268">The modifiers `virtual`, `override`, and `abstract` are mutually exclusive except in one case.</span></span> <span data-ttu-id="b24c2-1269">`abstract` 和 `override` 修饰符可一起使用，以便抽象索引器可以重写虚拟一个。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1269">The `abstract` and `override` modifiers may be used together so that an abstract indexer can override a virtual one.</span></span>

<span data-ttu-id="b24c2-1270">索引器声明的*类型*指定声明引入的索引器的元素类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1270">The *type* of an indexer declaration specifies the element type of the indexer introduced by the declaration.</span></span> <span data-ttu-id="b24c2-1271">除非索引器是显式接口成员实现，否则，该*类型*后跟关键字 `this`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1271">Unless the indexer is an explicit interface member implementation, the *type* is followed by the keyword `this`.</span></span> <span data-ttu-id="b24c2-1272">对于显式接口成员实现，该*类型*后跟一个*interface_type*、"`.`" 和关键字 "`this`"。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1272">For an explicit interface member implementation, the *type* is followed by an *interface_type*, a "`.`", and the keyword `this`.</span></span> <span data-ttu-id="b24c2-1273">与其他成员不同，索引器没有用户定义的名称。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1273">Unlike other members, indexers do not have user-defined names.</span></span>

<span data-ttu-id="b24c2-1274">*Formal_parameter_list*指定索引器的参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1274">The *formal_parameter_list* specifies the parameters of the indexer.</span></span> <span data-ttu-id="b24c2-1275">索引器的形参列表与方法（[方法参数](classes.md#method-parameters)）的形参列表相同，不同之处在于至少必须指定一个形参，并且不允许 `ref` 和 `out` 参数修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1275">The formal parameter list of an indexer corresponds to that of a method ([Method parameters](classes.md#method-parameters)), except that at least one parameter must be specified, and that the `ref` and `out` parameter modifiers are not permitted.</span></span>

<span data-ttu-id="b24c2-1276">索引器的*类型*和*formal_parameter_list*中引用的每个类型必须至少与索引器本身相同（[可访问性约束](basic-concepts.md#accessibility-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1276">The *type* of an indexer and each of the types referenced in the *formal_parameter_list* must be at least as accessible as the indexer itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="b24c2-1277">*Indexer_body*可以包含***访问器体***或***表达式主体***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1277">An *indexer_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="b24c2-1278">在访问器主体中， *accessor_declarations*（必须将其括在 "`{`" 和 "`}`" 标记中），请声明属性的访问器（[访问](classes.md#accessors)器）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1278">In an accessor body, *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="b24c2-1279">访问器指定与读取和写入属性相关联的可执行语句。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1279">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="b24c2-1280">由 "`=>`" 后跟表达式 `E` 并且分号完全等效于语句体 `{ get { return E; } }`的表达式体，因此仅用于指定仅限 getter 的索引器，其中，getter 的结果由单个表达式提供。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1280">An expression body consisting of "`=>`" followed by an expression `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only indexers where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="b24c2-1281">尽管访问索引器元素的语法与数组元素的语法相同，但不会将索引器元素分类为变量。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1281">Even though the syntax for accessing an indexer element is the same as that for an array element, an indexer element is not classified as a variable.</span></span> <span data-ttu-id="b24c2-1282">因此，不能将索引器元素作为 `ref` 或 `out` 参数进行传递。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1282">Thus, it is not possible to pass an indexer element as a `ref` or `out` argument.</span></span>

<span data-ttu-id="b24c2-1283">索引器的形参列表定义索引器的签名（[签名和重载](basic-concepts.md#signatures-and-overloading)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1283">The formal parameter list of an indexer defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the indexer.</span></span> <span data-ttu-id="b24c2-1284">具体而言，索引器的签名由其形参的数量和类型组成。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1284">Specifically, the signature of an indexer consists of the number and types of its formal parameters.</span></span> <span data-ttu-id="b24c2-1285">形参的元素类型和名称不属于索引器的签名。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1285">The element type and names of the formal parameters are not part of an indexer's signature.</span></span>

<span data-ttu-id="b24c2-1286">索引器的签名必须与同一类中声明的所有其他索引器的签名不同。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1286">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>

<span data-ttu-id="b24c2-1287">索引器和属性在概念上非常类似，但在以下方面有所不同：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1287">Indexers and properties are very similar in concept, but differ in the following ways:</span></span>

*  <span data-ttu-id="b24c2-1288">属性由其名称标识，而索引器由其签名标识。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1288">A property is identified by its name, whereas an indexer is identified by its signature.</span></span>
*  <span data-ttu-id="b24c2-1289">属性可通过*simple_name* （[简单名称](expressions.md#simple-names)）或*member_access* （[成员访问](expressions.md#member-access)）访问，而索引器元素则通过*element_access* （[索引器访问](expressions.md#indexer-access)）访问。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1289">A property is accessed through a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)), whereas an indexer element is accessed through an *element_access* ([Indexer access](expressions.md#indexer-access)).</span></span>
*  <span data-ttu-id="b24c2-1290">属性可以是 `static` 成员，而索引器始终是实例成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1290">A property can be a `static` member, whereas an indexer is always an instance member.</span></span>
*  <span data-ttu-id="b24c2-1291">属性的 `get` 访问器对应于不带参数的方法，而索引器的 `get` 访问器对应于具有与索引器相同的形参列表的方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1291">A `get` accessor of a property corresponds to a method with no parameters, whereas a `get` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer.</span></span>
*  <span data-ttu-id="b24c2-1292">属性的 `set` 访问器对应于一个方法，该方法具有一个名为 `value`的参数，而索引器的 `set` 访问器对应于一个方法，该方法具有与索引器相同的形参列表，另外还有一个名为 `value`的附加形参。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1292">A `set` accessor of a property corresponds to a method with a single parameter named `value`, whereas a `set` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer, plus an additional parameter named `value`.</span></span>
*  <span data-ttu-id="b24c2-1293">索引器访问器使用与索引器参数相同的名称声明局部变量是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1293">It is a compile-time error for an indexer accessor to declare a local variable with the same name as an indexer parameter.</span></span>
*  <span data-ttu-id="b24c2-1294">在重写属性声明中，使用 `base.P`语法访问继承属性，其中 `P` 是属性名称。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1294">In an overriding property declaration, the inherited property is accessed using the syntax `base.P`, where `P` is the property name.</span></span> <span data-ttu-id="b24c2-1295">在重写索引器声明中，继承的索引器是使用语法 `base[E]`访问的，其中 `E` 是逗号分隔的表达式列表。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1295">In an overriding indexer declaration, the inherited indexer is accessed using the syntax `base[E]`, where `E` is a comma separated list of expressions.</span></span>
*  <span data-ttu-id="b24c2-1296">没有 "自动实现的索引器" 的概念。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1296">There is no concept of an "automatically implemented indexer".</span></span> <span data-ttu-id="b24c2-1297">使用具有分号访问器的非抽象非外部索引器是错误的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1297">It is an error to have a non-abstract, non-external indexer with semicolon accessors.</span></span>

<span data-ttu-id="b24c2-1298">除了这些差异以外，在[访问器](classes.md#accessors)和[自动实现的属性](classes.md#automatically-implemented-properties)中定义的所有规则都适用于索引器访问器以及属性访问器。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1298">Aside from these differences, all rules defined in [Accessors](classes.md#accessors) and [Automatically implemented properties](classes.md#automatically-implemented-properties) apply to indexer accessors as well as to property accessors.</span></span>

<span data-ttu-id="b24c2-1299">当索引器声明包含 `extern` 修饰符时，该索引器被称为***外部索引***器。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1299">When an indexer declaration includes an `extern` modifier, the indexer is said to be an ***external indexer***.</span></span> <span data-ttu-id="b24c2-1300">由于外部索引器声明不提供实际的实现，因此它的每个*accessor_declarations*都包含一个分号。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1300">Because an external indexer declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

<span data-ttu-id="b24c2-1301">下面的示例声明一个 `BitArray` 类，该类实现索引器以访问位数组中的单个位。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1301">The example below declares a `BitArray` class that implements an indexer for accessing the individual bits in the bit array.</span></span>
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

<span data-ttu-id="b24c2-1302">`BitArray` 类的实例使用的内存量明显小于相应的 `bool[]` （因为前者的每个值仅占用一位而不是后者的一个字节），但它允许与 `bool[]`相同的操作。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1302">An instance of the `BitArray` class consumes substantially less memory than a corresponding `bool[]` (since each value of the former occupies only one bit instead of the latter's one byte), but it permits the same operations as a `bool[]`.</span></span>

<span data-ttu-id="b24c2-1303">以下 `CountPrimes` 类使用 `BitArray` 和传统的 "埃拉托色" 算法来计算介于1和 a 之间的 primes：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1303">The following `CountPrimes` class uses a `BitArray` and the classical "sieve" algorithm to compute the number of primes between 1 and a given maximum:</span></span>
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

<span data-ttu-id="b24c2-1304">请注意，用于访问 `BitArray` 的元素的语法与 `bool[]`的语法完全相同。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1304">Note that the syntax for accessing elements of the `BitArray` is precisely the same as for a `bool[]`.</span></span>

<span data-ttu-id="b24c2-1305">下面的示例显示一个 26 \* 10 网格类，该类具有一个具有两个参数的索引器。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1305">The following example shows a 26 \* 10 grid class that has an indexer with two parameters.</span></span> <span data-ttu-id="b24c2-1306">第一个参数必须是 A-z 范围内的大写或小写字母，第二个参数必须是0-9 范围内的整数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1306">The first parameter is required to be an upper- or lowercase letter in the range A-Z, and the second is required to be an integer in the range 0-9.</span></span>

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

### <a name="indexer-overloading"></a><span data-ttu-id="b24c2-1307">索引器重载</span><span class="sxs-lookup"><span data-stu-id="b24c2-1307">Indexer overloading</span></span>

<span data-ttu-id="b24c2-1308">[类型推理](expressions.md#type-inference)中介绍了索引器重载决策规则。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1308">The indexer overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="operators"></a><span data-ttu-id="b24c2-1309">运算符</span><span class="sxs-lookup"><span data-stu-id="b24c2-1309">Operators</span></span>

<span data-ttu-id="b24c2-1310">***运算符***是一个成员，用于定义可应用于类的实例的表达式运算符的含义。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1310">An ***operator*** is a member that defines the meaning of an expression operator that can be applied to instances of the class.</span></span> <span data-ttu-id="b24c2-1311">使用*operator_declaration*s 声明运算符：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1311">Operators are declared using *operator_declaration*s:</span></span>

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
    | right_shift | '=='  | '!='  | '>'   | '<'   | '>='  | '<='
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

<span data-ttu-id="b24c2-1312">有三种类型的可重载运算符：一元运算符（[一元运算符](classes.md#unary-operators)）、二元运算符（[二元运算符](classes.md#binary-operators)）和转换运算符（[转换运算符](classes.md#conversion-operators)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1312">There are three categories of overloadable operators: Unary operators ([Unary operators](classes.md#unary-operators)), binary operators ([Binary operators](classes.md#binary-operators)), and conversion operators ([Conversion operators](classes.md#conversion-operators)).</span></span>

<span data-ttu-id="b24c2-1313">*Operator_body*为分号、***语句体***或***表达式主体***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1313">The *operator_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="b24c2-1314">语句体由*块*组成，它指定在调用运算符时要执行的语句。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1314">A statement body consists of a *block*, which specifies the statements to execute when the operator is invoked.</span></span> <span data-ttu-id="b24c2-1315">*块*必须符合[方法体](classes.md#method-body)中所述的返回值的方法的规则。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1315">The *block* must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="b24c2-1316">表达式主体由 `=>` 后跟表达式和分号组成，表示在调用运算符时要执行的单个表达式。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1316">An expression body consists of `=>` followed by an expression and a semicolon, and denotes a single expression to perform when the operator is invoked.</span></span>

<span data-ttu-id="b24c2-1317">对于 `extern` 运算符， *operator_body*只包含一个分号。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1317">For `extern` operators, the *operator_body* consists simply of a semicolon.</span></span> <span data-ttu-id="b24c2-1318">对于所有其他运算符， *operator_body*为块体或表达式主体。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1318">For all other operators, the *operator_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="b24c2-1319">以下规则适用于所有运算符声明：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1319">The following rules apply to all operator declarations:</span></span>

*  <span data-ttu-id="b24c2-1320">运算符声明必须同时包含 `public` 和 `static` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1320">An operator declaration must include both a `public` and a `static` modifier.</span></span>
*  <span data-ttu-id="b24c2-1321">运算符的参数必须是值参数（[值参数](variables.md#value-parameters)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1321">The parameter(s) of an operator must be value parameters ([Value parameters](variables.md#value-parameters)).</span></span> <span data-ttu-id="b24c2-1322">运算符声明指定 `ref` 或 `out` 参数是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1322">It is a compile-time error for an operator declaration to specify `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="b24c2-1323">运算符（[一元运算符](classes.md#unary-operators)、[二元运算符](classes.md#binary-operators)、[转换运算符](classes.md#conversion-operators)）的签名必须不同于同一个类中声明的所有其他运算符的签名。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1323">The signature of an operator ([Unary operators](classes.md#unary-operators), [Binary operators](classes.md#binary-operators), [Conversion operators](classes.md#conversion-operators)) must differ from the signatures of all other operators declared in the same class.</span></span>
*  <span data-ttu-id="b24c2-1324">运算符声明中引用的所有类型必须至少具有与运算符本身相同的可访问性（[辅助功能约束](basic-concepts.md#accessibility-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1324">All types referenced in an operator declaration must be at least as accessible as the operator itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>
*  <span data-ttu-id="b24c2-1325">同一修饰符在运算符声明中多次出现是错误的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1325">It is an error for the same modifier to appear multiple times in an operator declaration.</span></span>

<span data-ttu-id="b24c2-1326">每个运算符类别都有其他限制，如以下各节所述。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1326">Each operator category imposes additional restrictions, as described in the following sections.</span></span>

<span data-ttu-id="b24c2-1327">与其他成员一样，基类中声明的运算符由派生类继承。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1327">Like other members, operators declared in a base class are inherited by derived classes.</span></span> <span data-ttu-id="b24c2-1328">由于运算符声明始终需要声明运算符的类或结构参与运算符的签名，因此，派生类中声明的运算符不能隐藏在基类中声明的运算符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1328">Because operator declarations always require the class or struct in which the operator is declared to participate in the signature of the operator, it is not possible for an operator declared in a derived class to hide an operator declared in a base class.</span></span> <span data-ttu-id="b24c2-1329">因此，在运算符声明中绝不需要并且不允许使用 `new` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1329">Thus, the `new` modifier is never required, and therefore never permitted, in an operator declaration.</span></span>

<span data-ttu-id="b24c2-1330">有关一元运算符和二元运算符的其他信息，请参阅[运算符](expressions.md#operators)。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1330">Additional information on unary and binary operators can be found in [Operators](expressions.md#operators).</span></span>

<span data-ttu-id="b24c2-1331">有关转换运算符的其他信息，请参阅[用户定义的转换](conversions.md#user-defined-conversions)。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1331">Additional information on conversion operators can be found in [User-defined conversions](conversions.md#user-defined-conversions).</span></span>

### <a name="unary-operators"></a><span data-ttu-id="b24c2-1332">一元运算符</span><span class="sxs-lookup"><span data-stu-id="b24c2-1332">Unary operators</span></span>

<span data-ttu-id="b24c2-1333">以下规则适用于一元运算符声明，其中 `T` 表示包含运算符声明的类或结构的实例类型：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1333">The following rules apply to unary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="b24c2-1334">一元 `+`、`-`、`!`或 `~` 运算符必须采用类型 `T` 或 `T?` 的单个参数，并可以返回任何类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1334">A unary `+`, `-`, `!`, or `~` operator must take a single parameter of type `T` or `T?` and can return any type.</span></span>
*  <span data-ttu-id="b24c2-1335">一元 `++` 或 `--` 运算符必须采用 `T` 或 `T?` 类型的单个参数，并且必须返回该类型或从其派生的类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1335">A unary `++` or `--` operator must take a single parameter of type `T` or `T?` and must return that same type or a type derived from it.</span></span>
*  <span data-ttu-id="b24c2-1336">一元 `true` 或 `false` 运算符必须采用 `T` 或 `T?` 类型的单个参数，并且必须返回类型 `bool`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1336">A unary `true` or `false` operator must take a single parameter of type `T` or `T?` and must return type `bool`.</span></span>

<span data-ttu-id="b24c2-1337">一元运算符的签名由运算符标记（`+`、`-`、`!`、`~`、`++`、`--`、`true`或 `false`）和单个形参的类型组成。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1337">The signature of a unary operator consists of the operator token (`+`, `-`, `!`, `~`, `++`, `--`, `true`, or `false`) and the type of the single formal parameter.</span></span> <span data-ttu-id="b24c2-1338">返回类型不是一元运算符的签名的一部分，也不是形参的名称。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1338">The return type is not part of a unary operator's signature, nor is the name of the formal parameter.</span></span>

<span data-ttu-id="b24c2-1339">`true` 和 `false` 一元运算符要求成对声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1339">The `true` and `false` unary operators require pair-wise declaration.</span></span> <span data-ttu-id="b24c2-1340">如果类声明了这些运算符中的一个，而没有同时声明其他运算符，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1340">A compile-time error occurs if a class declares one of these operators without also declaring the other.</span></span> <span data-ttu-id="b24c2-1341">[用户定义的条件逻辑运算符](expressions.md#user-defined-conditional-logical-operators)和[布尔表达式](expressions.md#boolean-expressions)中进一步介绍了 `true` 和 `false` 运算符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1341">The `true` and `false` operators are described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators) and [Boolean expressions](expressions.md#boolean-expressions).</span></span>

<span data-ttu-id="b24c2-1342">下面的示例演示了一个实现，并为整数向量类 `operator ++` 了后续用法：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1342">The following example shows an implementation and subsequent usage of `operator ++` for an integer vector class:</span></span>
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

<span data-ttu-id="b24c2-1343">请注意，operator 方法如何返回通过将1添加到操作数而生成的值，就像后缀增量和减量运算符（[后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators)）以及前缀增量和减量运算符（[前缀增量和减量运算符](expressions.md#prefix-increment-and-decrement-operators)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1343">Note how the operator method returns the value produced by adding 1 to the operand, just like the  postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)), and the prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="b24c2-1344">与在C++中不同的是，此方法不需要直接修改其操作数的值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1344">Unlike in C++, this method need not modify the value of its operand directly.</span></span> <span data-ttu-id="b24c2-1345">事实上，修改操作数值将违反后缀增量运算符的标准语义。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1345">In fact, modifying the operand value would violate the standard semantics of the postfix increment operator.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="b24c2-1346">二元运算符</span><span class="sxs-lookup"><span data-stu-id="b24c2-1346">Binary operators</span></span>

<span data-ttu-id="b24c2-1347">以下规则适用于二元运算符声明，其中 `T` 表示包含运算符声明的类或结构的实例类型：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1347">The following rules apply to binary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="b24c2-1348">二元非移位运算符必须采用两个参数，其中至少一个参数必须具有类型 `T` 或 `T?`，并且可以返回任何类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1348">A binary non-shift operator must take two parameters, at least one of which must have type `T` or `T?`, and can return any type.</span></span>
*  <span data-ttu-id="b24c2-1349">二进制 `<<` 或 `>>` 运算符必须采用两个参数，其中第一个参数必须具有类型 `T` 或 `T?`，而第二个参数必须具有类型 `int` 或 `int?`，并且可以返回任何类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1349">A binary `<<` or `>>` operator must take two parameters, the first of which must have type `T` or `T?` and the second of which must have type `int` or `int?`, and can return any type.</span></span>

<span data-ttu-id="b24c2-1350">二元运算符的签名由运算符标记（`+`、`-`、`*`、`/`、`%`、`&`、`|`、`^`、`<<`、`>>`、`==`、`!=`、`>`、`<`、`>=`或 `<=`）和两个形参的类型组成。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1350">The signature of a binary operator consists of the operator token (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, or `<=`) and the types of the two formal parameters.</span></span> <span data-ttu-id="b24c2-1351">返回类型和形参的名称不是二元运算符的签名的一部分。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1351">The return type and the names of the formal parameters are not part of a binary operator's signature.</span></span>

<span data-ttu-id="b24c2-1352">某些二元运算符要求成对声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1352">Certain binary operators require pair-wise declaration.</span></span> <span data-ttu-id="b24c2-1353">对于每个运算符的每个声明，都必须有一个对的另一个运算符的匹配声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1353">For every declaration of either operator of a pair, there must be a matching declaration of the other operator of the pair.</span></span> <span data-ttu-id="b24c2-1354">当两个运算符声明具有相同的返回类型，并且每个参数具有相同的类型时，它们将匹配。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1354">Two operator declarations match when they have the same return type and the same type for each parameter.</span></span> <span data-ttu-id="b24c2-1355">以下运算符要求成对声明：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1355">The following operators require pair-wise declaration:</span></span>

*  <span data-ttu-id="b24c2-1356">`operator ==` 和 `operator !=`</span><span class="sxs-lookup"><span data-stu-id="b24c2-1356">`operator ==` and `operator !=`</span></span>
*  <span data-ttu-id="b24c2-1357">`operator >` 和 `operator <`</span><span class="sxs-lookup"><span data-stu-id="b24c2-1357">`operator >` and `operator <`</span></span>
*  <span data-ttu-id="b24c2-1358">`operator >=` 和 `operator <=`</span><span class="sxs-lookup"><span data-stu-id="b24c2-1358">`operator >=` and `operator <=`</span></span>

### <a name="conversion-operators"></a><span data-ttu-id="b24c2-1359">转换运算符</span><span class="sxs-lookup"><span data-stu-id="b24c2-1359">Conversion operators</span></span>

<span data-ttu-id="b24c2-1360">转换运算符声明引入了***用户定义的转换***（[用户定义](conversions.md#user-defined-conversions)的转换），以补充预定义的隐式和显式转换。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1360">A conversion operator declaration introduces a ***user-defined conversion*** ([User-defined conversions](conversions.md#user-defined-conversions)) which augments the pre-defined implicit and explicit conversions.</span></span>

<span data-ttu-id="b24c2-1361">包含 `implicit` 关键字的转换运算符声明引入了用户定义的隐式转换。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1361">A conversion operator declaration that includes the `implicit` keyword introduces a user-defined implicit conversion.</span></span> <span data-ttu-id="b24c2-1362">隐式转换可能会在多种情况下发生，包括函数成员调用、强制转换表达式和赋值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1362">Implicit conversions can occur in a variety of situations, including function member invocations, cast expressions, and assignments.</span></span> <span data-ttu-id="b24c2-1363">[隐式转换](conversions.md#implicit-conversions)中对此进行了进一步说明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1363">This is described further in [Implicit conversions](conversions.md#implicit-conversions).</span></span>

<span data-ttu-id="b24c2-1364">包含 `explicit` 关键字的转换运算符声明引入了用户定义的显式转换。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1364">A conversion operator declaration that includes the `explicit` keyword introduces a user-defined explicit conversion.</span></span> <span data-ttu-id="b24c2-1365">显式转换可在强制转换表达式中发生，并在[显式转换](conversions.md#explicit-conversions)中进一步说明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1365">Explicit conversions can occur in cast expressions, and are described further in [Explicit conversions](conversions.md#explicit-conversions).</span></span>

<span data-ttu-id="b24c2-1366">转换运算符从转换运算符的参数类型指示的源类型转换为目标类型，由转换运算符的返回类型指示。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1366">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span>

<span data-ttu-id="b24c2-1367">对于给定的源类型 `S` 和目标类型 `T`，如果 `S` 或 `T` 是可以为 null 的类型，则允许 `S0` 和 `T0` 引用它们的基础类型，否则 `S0` 和 `T0` 分别等于 `S` 和 `T`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1367">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="b24c2-1368">仅当满足以下所有条件时，才允许类或结构声明从源类型 `S` 转换为目标类型 `T`：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1368">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="b24c2-1369">`S0` 和 `T0` 属于不同类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1369">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="b24c2-1370">`S0` 或 `T0` 是发生运算符声明的类或结构类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1370">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="b24c2-1371">`S0` 和 `T0` 都不是*interface_type*的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1371">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="b24c2-1372">如果不包括用户定义的转换，则从 `S` 到 `T` 或从 `T` 到 `S`的转换不存在。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1372">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="b24c2-1373">对于这些规则，与 `S` 或 `T` 关联的任何类型参数都视为唯一的类型，这些类型与其他类型没有继承关系，并忽略这些类型参数上的任何约束。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1373">For the purposes of these rules, any type parameters associated with `S` or `T` are considered to be unique types that have no inheritance relationship with other types, and any constraints on those type parameters are ignored.</span></span>

<span data-ttu-id="b24c2-1374">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-1374">In the example</span></span>
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
<span data-ttu-id="b24c2-1375">允许使用前两个运算符声明，因为对于[索引器](classes.md#indexers).3，分别 `T` 和 `int` 和 `string` 被视为唯一的类型，而无任何关系。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1375">the first two operator declarations are permitted because, for the purposes of [Indexers](classes.md#indexers).3, `T` and `int` and `string` respectively are considered unique types with no relationship.</span></span> <span data-ttu-id="b24c2-1376">但第三个运算符是错误，因为 `C<T>` 是 `D<T>`的基类。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1376">However, the third operator is an error because `C<T>` is the base class of `D<T>`.</span></span>

<span data-ttu-id="b24c2-1377">从第二个规则开始，转换运算符必须在声明运算符的类或结构类型中转换为或。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1377">From the second rule it follows that a conversion operator must convert either to or from the class or struct type in which the operator is declared.</span></span> <span data-ttu-id="b24c2-1378">例如，类或结构类型 `C` 可以定义从 `C` 到 `int` 之间的转换和从 `int` 到 `C`的转换，但不能定义从 `int` 到 `bool`的转换。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1378">For example, it is possible for a class or struct type `C` to define a conversion from `C` to `int` and from `int` to `C`, but not from `int` to `bool`.</span></span>

<span data-ttu-id="b24c2-1379">不能直接重新定义预定义的转换。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1379">It is not possible to directly redefine a pre-defined conversion.</span></span> <span data-ttu-id="b24c2-1380">因此，不允许转换运算符从或转换为 `object`，因为 `object` 和所有其他类型之间已存在隐式和显式转换。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1380">Thus, conversion operators are not allowed to convert from or to `object` because implicit and explicit conversions already exist between `object` and all other types.</span></span> <span data-ttu-id="b24c2-1381">同样，转换的源类型和目标类型都不能是另一类型的基类型，因为转换已经存在。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1381">Likewise, neither the source nor the target types of a conversion can be a base type of the other, since a conversion would then already exist.</span></span>

<span data-ttu-id="b24c2-1382">但是，可以在针对特定类型参数的泛型类型上声明运算符，指定已作为预定义转换存在的转换。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1382">However, it is possible to declare operators on generic types that, for particular type arguments, specify conversions that already exist as pre-defined conversions.</span></span> <span data-ttu-id="b24c2-1383">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-1383">In the example</span></span>
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
<span data-ttu-id="b24c2-1384">当类型 `object` 指定为 `T`的类型参数时，第二个运算符将声明已存在的转换（隐式，因此也是从任何类型到类型 `object`的显式转换）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1384">when type `object` is specified as a type argument for `T`, the second operator declares a conversion that already exists (an implicit, and therefore also an explicit, conversion exists from any type to type `object`).</span></span>

<span data-ttu-id="b24c2-1385">如果两种类型之间存在预定义的转换，则将忽略这些类型之间的任何用户定义的转换。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1385">In cases where a pre-defined conversion exists between two types, any user-defined conversions between those types are ignored.</span></span> <span data-ttu-id="b24c2-1386">尤其是在下列情况下：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1386">Specifically:</span></span>

*  <span data-ttu-id="b24c2-1387">如果存在从类型 `S` 到类型 `T`的预定义隐式转换（[隐式](conversions.md#implicit-conversions)转换），则将忽略从 `S` 到 `T` 的所有用户定义的转换（隐式或显式转换）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1387">If a pre-defined implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from type `S` to type `T`, all user-defined conversions (implicit or explicit) from `S` to `T` are ignored.</span></span>
*  <span data-ttu-id="b24c2-1388">如果从类型 `S` 中存在预定义的显式转换（[显式转换](conversions.md#explicit-conversions)）到类型 `T`，则将忽略从 `S` 到 `T` 的任何用户定义的显式转换。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1388">If a pre-defined explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) exists from type `S` to type `T`, any user-defined explicit conversions from `S` to `T` are ignored.</span></span> <span data-ttu-id="b24c2-1389">此外：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1389">Furthermore:</span></span>

<span data-ttu-id="b24c2-1390">如果 `T` 是接口类型，则将忽略从 `S` 到 `T` 的用户定义隐式转换。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1390">If `T` is an interface type, user-defined implicit conversions from `S` to `T` are ignored.</span></span>

<span data-ttu-id="b24c2-1391">否则，仍会考虑从 `S` 到 `T` 的用户定义隐式转换。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1391">Otherwise, user-defined implicit conversions from `S` to `T` are still considered.</span></span>

<span data-ttu-id="b24c2-1392">对于所有类型但 `object`，以上 `Convertible<T>` 类型声明的运算符不会与预定义转换冲突。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1392">For all types but `object`, the operators declared by the `Convertible<T>` type above do not conflict with pre-defined conversions.</span></span> <span data-ttu-id="b24c2-1393">例如：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1393">For example:</span></span>
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

<span data-ttu-id="b24c2-1394">但是，对于类型 `object`，预定义的转换在所有情况下都会隐藏用户定义的转换，但会隐藏其中一项：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1394">However, for type `object`, pre-defined conversions hide the user-defined conversions in all cases but one:</span></span>

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

<span data-ttu-id="b24c2-1395">不允许用户定义的转换从或转换到*interface_type*。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1395">User-defined conversions are not allowed to convert from or to *interface_type*s.</span></span> <span data-ttu-id="b24c2-1396">特别是，此限制可确保在转换为*interface_type*时不会发生用户定义的转换，并且仅当被转换的对象真正实现指定的*interface_type*时，才能成功转换为*interface_type* 。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1396">In particular, this restriction ensures that no user-defined transformations occur when converting to an *interface_type*, and that a conversion to an *interface_type* succeeds only if the object being converted actually implements the specified *interface_type*.</span></span>

<span data-ttu-id="b24c2-1397">转换运算符的签名包含源类型和目标类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1397">The signature of a conversion operator consists of the source type and the target type.</span></span> <span data-ttu-id="b24c2-1398">（请注意，这是返回类型参与签名的唯一成员形式。）转换运算符的 `implicit` 或 `explicit` 分类不是运算符签名的一部分。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1398">(Note that this is the only form of member for which the return type participates in the signature.) The `implicit` or `explicit` classification of a conversion operator is not part of the operator's signature.</span></span> <span data-ttu-id="b24c2-1399">因此，类或结构不能声明具有相同源和目标类型的 `implicit` 和 `explicit` 转换运算符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1399">Thus, a class or struct cannot declare both an `implicit` and an `explicit` conversion operator with the same source and target types.</span></span>

<span data-ttu-id="b24c2-1400">通常，用户定义的隐式转换应设计为从不引发异常，并且永远不会丢失信息。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1400">In general, user-defined implicit conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="b24c2-1401">如果用户定义的转换可以增加异常（例如，由于源参数超出范围）或丢失信息（如丢弃高序位），则应将该转换定义为显式转换。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1401">If a user-defined conversion can give rise to exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as an explicit conversion.</span></span>

<span data-ttu-id="b24c2-1402">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-1402">In the example</span></span>
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
<span data-ttu-id="b24c2-1403">从 `Digit` 到 `byte` 的转换是隐式的，因为它从不引发异常或丢失信息，但从 `byte` 到 `Digit` 的转换是显式的，因为 `Digit` 只能表示 `byte`的可能值的子集。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1403">the conversion from `Digit` to `byte` is implicit because it never throws exceptions or loses information, but the conversion from `byte` to `Digit` is explicit since `Digit` can only represent a subset of the possible values of a `byte`.</span></span>

## <a name="instance-constructors"></a><span data-ttu-id="b24c2-1404">实例构造函数</span><span class="sxs-lookup"><span data-stu-id="b24c2-1404">Instance constructors</span></span>

<span data-ttu-id="b24c2-1405">***实例构造函数***是实现初始化类实例所需执行的操作的成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1405">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="b24c2-1406">实例构造函数是使用*constructor_declaration*s 声明的：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1406">Instance constructors are declared using *constructor_declaration*s:</span></span>

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

<span data-ttu-id="b24c2-1407">*Constructor_declaration*可以包含一组*特性*（[特性](attributes.md)）、四个访问修饰符（[访问修饰符](classes.md#access-modifiers)）和 `extern` （[外部方法](classes.md#external-methods)）修饰符的有效组合。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1407">A *constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and an `extern` ([External methods](classes.md#external-methods)) modifier.</span></span> <span data-ttu-id="b24c2-1408">不允许构造函数声明多次包含同一修饰符。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1408">A constructor declaration is not permitted to include the same modifier multiple times.</span></span>

<span data-ttu-id="b24c2-1409">*Constructor_declarator*的*标识符*必须命名声明实例构造函数的类。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1409">The *identifier* of a *constructor_declarator* must name the class in which the instance constructor is declared.</span></span> <span data-ttu-id="b24c2-1410">如果指定了其他名称，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1410">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="b24c2-1411">实例构造函数的可选*formal_parameter_list*遵从与方法（[方法](classes.md#methods)） *formal_parameter_list*相同的规则。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1411">The optional *formal_parameter_list* of an instance constructor is subject to the same rules as the *formal_parameter_list* of a method ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="b24c2-1412">形参表定义实例构造函数的签名（[签名和重载](basic-concepts.md#signatures-and-overloading)），并控制重载决策（[类型推理](expressions.md#type-inference)）在调用中选择特定实例构造函数的过程。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1412">The formal parameter list defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of an instance constructor and governs the process whereby overload resolution ([Type inference](expressions.md#type-inference)) selects a particular instance constructor in an invocation.</span></span>

<span data-ttu-id="b24c2-1413">实例构造函数的*formal_parameter_list*中引用的每个类型必须至少具有与构造函数本身相同的可访问性（[辅助功能约束](basic-concepts.md#accessibility-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1413">Each of the types referenced in the *formal_parameter_list* of an instance constructor must be at least as accessible as the constructor itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="b24c2-1414">可选*constructor_initializer*指定要在执行此实例构造函数的*constructor_body*中给定的语句之前调用的另一个实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1414">The optional *constructor_initializer* specifies another instance constructor to invoke before executing the statements given in the *constructor_body* of this instance constructor.</span></span> <span data-ttu-id="b24c2-1415">[构造函数初始值设定项](classes.md#constructor-initializers)中对此进行了进一步说明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1415">This is described further in [Constructor initializers](classes.md#constructor-initializers).</span></span>

<span data-ttu-id="b24c2-1416">当构造函数声明包含 `extern` 修饰符时，构造函数被称为***外部构造函数***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1416">When a constructor declaration includes an `extern` modifier, the constructor is said to be an ***external constructor***.</span></span> <span data-ttu-id="b24c2-1417">由于外部构造函数声明不提供实际的实现，因此它的*constructor_body*由分号组成。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1417">Because an external constructor declaration provides no actual implementation, its *constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="b24c2-1418">对于所有其他构造函数， *constructor_body*包含一个*块*，它指定用于初始化类的新实例的语句。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1418">For all other constructors, the *constructor_body* consists of a *block* which specifies the statements to initialize a new instance of the class.</span></span> <span data-ttu-id="b24c2-1419">这与实例方法的*块*完全对应，具有 `void` 返回类型（[方法体](classes.md#method-body)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1419">This corresponds exactly to the *block* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="b24c2-1420">不继承实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1420">Instance constructors are not inherited.</span></span> <span data-ttu-id="b24c2-1421">因此，类没有实例构造函数，而不是类中实际声明的构造函数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1421">Thus, a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="b24c2-1422">如果类不包含实例构造函数声明，则会自动提供默认实例构造函数（[默认构造](classes.md#default-constructors)函数）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1422">If a class contains no instance constructor declarations, a default instance constructor is automatically provided ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="b24c2-1423">实例构造函数由*object_creation_expression*（[对象创建表达式](expressions.md#object-creation-expressions)）和*constructor_initializer*来调用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1423">Instance constructors are invoked by *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)) and through *constructor_initializer*s.</span></span>

### <a name="constructor-initializers"></a><span data-ttu-id="b24c2-1424">构造函数初始值设定项</span><span class="sxs-lookup"><span data-stu-id="b24c2-1424">Constructor initializers</span></span>

<span data-ttu-id="b24c2-1425">所有实例构造函数（类 `object`的类构造函数除外）都隐式包含对*constructor_body*之前的其他实例构造函数的调用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1425">All instance constructors (except those for class `object`) implicitly include an invocation of another instance constructor immediately before the *constructor_body*.</span></span> <span data-ttu-id="b24c2-1426">隐式调用的构造函数由*constructor_initializer*确定：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1426">The constructor to implicitly invoke is determined by the *constructor_initializer*:</span></span>

*  <span data-ttu-id="b24c2-1427">`base(argument_list)` 或 `base()` 形式的实例构造函数初始值设定项将导致调用直接基类的实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1427">An instance constructor initializer of the form `base(argument_list)` or `base()` causes an instance constructor from the direct base class to be invoked.</span></span> <span data-ttu-id="b24c2-1428">使用*argument_list*如果存在，则选择该构造函数，并[使用重载决策](expressions.md#overload-resolution)的重载决策规则。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1428">That constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="b24c2-1429">如果未在直接基类中声明任何实例构造函数，候选实例构造函数集则包含直接基类中包含的所有可访问实例构造函数，或者默认构造函数（[默认构造函数](classes.md#default-constructors)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1429">The set of candidate instance constructors consists of all accessible instance constructors contained in the direct base class, or the default constructor ([Default constructors](classes.md#default-constructors)), if no instance constructors are declared in the direct base class.</span></span> <span data-ttu-id="b24c2-1430">如果此集为空，或者无法识别单个最佳实例构造函数，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1430">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span>
*  <span data-ttu-id="b24c2-1431">`this(argument-list)` 或 `this()` 形式的实例构造函数初始值设定项会导致调用类自身的实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1431">An instance constructor initializer of the form `this(argument-list)` or `this()` causes an instance constructor from the class itself to be invoked.</span></span> <span data-ttu-id="b24c2-1432">如果存在，则使用*argument_list* ，并使用[重载](expressions.md#overload-resolution)决策的重载决策规则来选择构造函数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1432">The constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="b24c2-1433">候选实例构造函数集包含类本身中声明的所有可访问实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1433">The set of candidate instance constructors consists of all accessible instance constructors declared in the class itself.</span></span> <span data-ttu-id="b24c2-1434">如果此集为空，或者无法识别单个最佳实例构造函数，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1434">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span> <span data-ttu-id="b24c2-1435">如果实例构造函数声明包含调用构造函数本身的构造函数初始值设定项，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1435">If an instance constructor declaration includes a constructor initializer that invokes the constructor itself, a compile-time error occurs.</span></span>

<span data-ttu-id="b24c2-1436">如果实例构造函数没有构造函数初始值设定项，则会隐式提供形式 `base()` 的构造函数初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1436">If an instance constructor has no constructor initializer, a constructor initializer of the form `base()` is implicitly provided.</span></span> <span data-ttu-id="b24c2-1437">因此，形式的实例构造函数声明</span><span class="sxs-lookup"><span data-stu-id="b24c2-1437">Thus, an instance constructor declaration of the form</span></span>
```csharp
C(...) {...}
```
<span data-ttu-id="b24c2-1438">完全等效于</span><span class="sxs-lookup"><span data-stu-id="b24c2-1438">is exactly equivalent to</span></span>
```csharp
C(...): base() {...}
```

<span data-ttu-id="b24c2-1439">实例构造函数声明的*formal_parameter_list*给定的参数的范围包含该声明的构造函数初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1439">The scope of the parameters given by the *formal_parameter_list* of an instance constructor declaration includes the constructor initializer of that declaration.</span></span> <span data-ttu-id="b24c2-1440">因此，可以使用构造函数初始值设定项来访问构造函数的参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1440">Thus, a constructor initializer is permitted to access the parameters of the constructor.</span></span> <span data-ttu-id="b24c2-1441">例如：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1441">For example:</span></span>
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

<span data-ttu-id="b24c2-1442">实例构造函数初始值设定项无法访问正在创建的实例。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1442">An instance constructor initializer cannot access the instance being created.</span></span> <span data-ttu-id="b24c2-1443">因此，在构造函数初始值设定项的参数表达式中引用 `this` 是编译时错误，这是因为自变量表达式的编译时错误通过*simple_name*引用任何实例成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1443">Therefore it is a compile-time error to reference `this` in an argument expression of the constructor initializer, as is it a compile-time error for an argument expression to reference any instance member through a *simple_name*.</span></span>

### <a name="instance-variable-initializers"></a><span data-ttu-id="b24c2-1444">实例变量初始值设定项</span><span class="sxs-lookup"><span data-stu-id="b24c2-1444">Instance variable initializers</span></span>

<span data-ttu-id="b24c2-1445">如果实例构造函数不具有构造函数初始值设定项，或者它具有形式 `base(...)`形式的构造函数初始值设定项，则该构造函数将隐式执行由其类中声明的实例字段的*variable_initializer*指定的初始化。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1445">When an instance constructor has no constructor initializer, or it has a constructor initializer of the form `base(...)`, that constructor implicitly performs the initializations specified by the *variable_initializer*s of the instance fields declared in its class.</span></span> <span data-ttu-id="b24c2-1446">这对应于在进入构造函数之后、直接调用直接基类构造函数之前立即执行的一系列赋值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1446">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor and before the implicit invocation of the direct base class constructor.</span></span> <span data-ttu-id="b24c2-1447">变量初始值设定项将按照它们在类声明中的显示顺序执行。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1447">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span>

### <a name="constructor-execution"></a><span data-ttu-id="b24c2-1448">构造函数执行</span><span class="sxs-lookup"><span data-stu-id="b24c2-1448">Constructor execution</span></span>

<span data-ttu-id="b24c2-1449">变量初始值设定项转换为赋值语句，并在调用基类实例构造函数之前执行这些赋值语句。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1449">Variable initializers are transformed into assignment statements, and these assignment statements are executed before the invocation of the base class instance constructor.</span></span> <span data-ttu-id="b24c2-1450">此顺序可确保在执行有权访问该实例的任何语句之前，通过其变量初始值设定项来初始化所有实例字段。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1450">This ordering ensures that all instance fields are initialized by their variable initializers before any statements that have access to that instance are executed.</span></span>

<span data-ttu-id="b24c2-1451">给定示例</span><span class="sxs-lookup"><span data-stu-id="b24c2-1451">Given the example</span></span>
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
<span data-ttu-id="b24c2-1452">`new B()` 用于创建 `B`的实例时，将生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1452">when `new B()` is used to create an instance of `B`, the following output is produced:</span></span>
```console
x = 1, y = 0
```

<span data-ttu-id="b24c2-1453">`x` 的值为1，因为在调用基类实例构造函数之前将执行变量初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1453">The value of `x` is 1 because the variable initializer is executed before the base class instance constructor is invoked.</span></span> <span data-ttu-id="b24c2-1454">但 `y` 的值为0（`int`的默认值），因为在基类构造函数返回后才会执行 `y` 的赋值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1454">However, the value of `y` is 0 (the default value of an `int`) because the assignment to `y` is not executed until after the base class constructor returns.</span></span>

<span data-ttu-id="b24c2-1455">将实例变量初始值设定项和构造函数初始值设定项视为自动插入*constructor_body*前面的语句是非常有用的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1455">It is useful to think of instance variable initializers and constructor initializers as statements that are automatically inserted before the *constructor_body*.</span></span> <span data-ttu-id="b24c2-1456">示例</span><span class="sxs-lookup"><span data-stu-id="b24c2-1456">The example</span></span>
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
<span data-ttu-id="b24c2-1457">包含若干变量初始值设定项;它还包含两个窗体的构造函数初始值设定项（`base` 和 `this`）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1457">contains several variable initializers; it also contains constructor initializers of both forms (`base` and `this`).</span></span> <span data-ttu-id="b24c2-1458">该示例与下面显示的代码相对应，其中每个注释指示一个自动插入的语句（用于自动插入的构造函数调用的语法无效，但仅用于说明这种机制）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1458">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement (the syntax used for the automatically inserted constructor invocations isn't valid, but merely serves to illustrate the mechanism).</span></span>

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

### <a name="default-constructors"></a><span data-ttu-id="b24c2-1459">默认构造函数</span><span class="sxs-lookup"><span data-stu-id="b24c2-1459">Default constructors</span></span>

<span data-ttu-id="b24c2-1460">如果类不包含实例构造函数声明，则会自动提供一个默认实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1460">If a class contains no instance constructor declarations, a default instance constructor is automatically provided.</span></span> <span data-ttu-id="b24c2-1461">该默认构造函数只调用直接基类的无参数构造函数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1461">That default constructor simply invokes the parameterless constructor of the direct base class.</span></span> <span data-ttu-id="b24c2-1462">如果类是抽象类，则对默认构造函数的声明的可访问性是受保护的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1462">If the class is abstract then the declared accessibility for the default constructor is protected.</span></span> <span data-ttu-id="b24c2-1463">否则，默认构造函数的声明的可访问性是公共的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1463">Otherwise, the declared accessibility for the default constructor is public.</span></span> <span data-ttu-id="b24c2-1464">因此，默认构造函数始终为形式</span><span class="sxs-lookup"><span data-stu-id="b24c2-1464">Thus, the default constructor is always of the form</span></span>

```csharp
protected C(): base() {}
```
<span data-ttu-id="b24c2-1465">或</span><span class="sxs-lookup"><span data-stu-id="b24c2-1465">or</span></span>
```csharp
public C(): base() {}
```
<span data-ttu-id="b24c2-1466">其中 `C` 是类的名称。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1466">where `C` is the name of the class.</span></span> <span data-ttu-id="b24c2-1467">如果重载决策无法确定基类构造函数初始值设定项的唯一最佳候选项，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1467">If overload resolution is unable to determine a unique best candidate for the base class constructor initializer then a compile-time error occurs.</span></span>

<span data-ttu-id="b24c2-1468">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-1468">In the example</span></span>
```csharp
class Message
{
    object sender;
    string text;
}
```
<span data-ttu-id="b24c2-1469">由于类不包含实例构造函数声明，因此提供了默认的构造函数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1469">a default constructor is provided because the class contains no instance constructor declarations.</span></span> <span data-ttu-id="b24c2-1470">因此，该示例完全等效于</span><span class="sxs-lookup"><span data-stu-id="b24c2-1470">Thus, the example is precisely equivalent to</span></span>
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a><span data-ttu-id="b24c2-1471">私有构造函数</span><span class="sxs-lookup"><span data-stu-id="b24c2-1471">Private constructors</span></span>

<span data-ttu-id="b24c2-1472">当类 `T` 仅声明私有实例构造函数时，不能 `T` 的程序文本外的类从 `T` 派生或直接创建 `T`的实例。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1472">When a class `T` declares only private instance constructors, it is not possible for classes outside the program text of `T` to derive from `T` or to directly create instances of `T`.</span></span> <span data-ttu-id="b24c2-1473">因此，如果某个类只包含静态成员，并且不应实例化，则添加一个空的私有实例构造函数会阻止实例化。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1473">Thus, if a class contains only static members and isn't intended to be instantiated, adding an empty private instance constructor will prevent instantiation.</span></span> <span data-ttu-id="b24c2-1474">例如：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1474">For example:</span></span>
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

<span data-ttu-id="b24c2-1475">`Trig` 类将相关方法和常量分组，但不能进行实例化。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1475">The `Trig` class groups related methods and constants, but is not intended to be instantiated.</span></span> <span data-ttu-id="b24c2-1476">因此，它声明了一个空的私有实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1476">Therefore it declares a single empty private instance constructor.</span></span> <span data-ttu-id="b24c2-1477">至少必须将一个实例构造函数声明为禁止自动生成默认构造函数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1477">At least one instance constructor must be declared to suppress the automatic generation of a default constructor.</span></span>

### <a name="optional-instance-constructor-parameters"></a><span data-ttu-id="b24c2-1478">可选实例构造函数参数</span><span class="sxs-lookup"><span data-stu-id="b24c2-1478">Optional instance constructor parameters</span></span>

<span data-ttu-id="b24c2-1479">构造函数初始值设定项的 `this(...)` 形式通常与重载一起使用，以实现可选的实例构造函数参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1479">The `this(...)` form of constructor initializer is commonly used in conjunction with overloading to implement optional instance constructor parameters.</span></span> <span data-ttu-id="b24c2-1480">示例中</span><span class="sxs-lookup"><span data-stu-id="b24c2-1480">In the example</span></span>
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
<span data-ttu-id="b24c2-1481">前两个实例构造函数只为缺少的参数提供默认值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1481">the first two instance constructors merely provide the default values for the missing arguments.</span></span> <span data-ttu-id="b24c2-1482">这两种方法都使用 `this(...)` 构造函数初始值设定项调用第三个实例构造函数，该构造函数实际上用于初始化新实例。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1482">Both use a `this(...)` constructor initializer to invoke the third instance constructor, which actually does the work of initializing the new instance.</span></span> <span data-ttu-id="b24c2-1483">其结果是可选的构造函数参数：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1483">The effect is that of optional constructor parameters:</span></span>
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a><span data-ttu-id="b24c2-1484">静态构造函数</span><span class="sxs-lookup"><span data-stu-id="b24c2-1484">Static constructors</span></span>

<span data-ttu-id="b24c2-1485">***静态构造函数***是实现初始化封闭式类类型所需的操作的成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1485">A ***static constructor*** is a member that implements the actions required to initialize a closed class type.</span></span> <span data-ttu-id="b24c2-1486">静态构造函数使用*static_constructor_declaration*来声明：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1486">Static constructors are declared using *static_constructor_declaration*s:</span></span>

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

<span data-ttu-id="b24c2-1487">*Static_constructor_declaration*可以包含一组*特性*（[特性](attributes.md)）和一个 `extern` 修饰符（[外部方法](classes.md#external-methods)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1487">A *static_constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and an `extern` modifier ([External methods](classes.md#external-methods)).</span></span>

<span data-ttu-id="b24c2-1488">*Static_constructor_declaration*的*标识符*必须命名声明静态构造函数的类。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1488">The *identifier* of a *static_constructor_declaration* must name the class in which the static constructor is declared.</span></span> <span data-ttu-id="b24c2-1489">如果指定了其他名称，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1489">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="b24c2-1490">如果静态构造函数声明包含 `extern` 修饰符，则将静态构造函数称为***外部静态构造函数***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1490">When a static constructor declaration includes an `extern` modifier, the static constructor is said to be an ***external static constructor***.</span></span> <span data-ttu-id="b24c2-1491">由于外部静态构造函数声明不提供实际的实现，因此它的*static_constructor_body*由分号组成。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1491">Because an external static constructor declaration provides no actual implementation, its *static_constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="b24c2-1492">对于所有其他静态构造函数声明， *static_constructor_body*包含一个*块*，它指定要执行的语句，以便初始化类。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1492">For all other static constructor declarations, the *static_constructor_body* consists of a *block* which specifies the statements to execute in order to initialize the class.</span></span> <span data-ttu-id="b24c2-1493">这与 `void` 返回类型（[方法体](classes.md#method-body)）的静态方法的*method_body*完全对应。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1493">This corresponds exactly to the *method_body* of a static method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="b24c2-1494">静态构造函数不是继承的，不能直接调用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1494">Static constructors are not inherited, and cannot be called directly.</span></span>

<span data-ttu-id="b24c2-1495">封闭式类类型的静态构造函数在给定的应用程序域中最多执行一次。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1495">The static constructor for a closed class type executes at most once in a given application domain.</span></span> <span data-ttu-id="b24c2-1496">在应用程序域中，以下事件的第一个事件会触发静态构造函数的执行：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1496">The execution of a static constructor is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="b24c2-1497">创建类类型的实例。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1497">An instance of the class type is created.</span></span>
*  <span data-ttu-id="b24c2-1498">引用类类型的任何静态成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1498">Any of the static members of the class type are referenced.</span></span>

<span data-ttu-id="b24c2-1499">如果类包含执行开始时所处的 `Main` 方法（[应用程序启动](basic-concepts.md#application-startup)），则在调用 `Main` 方法之前，将执行该类的静态构造函数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1499">If a class contains the `Main` method ([Application Startup](basic-concepts.md#application-startup)) in which execution begins, the static constructor for that class executes before the `Main` method is called.</span></span>

<span data-ttu-id="b24c2-1500">若要初始化新的封闭式类类型，请先创建一组新的已关闭类型的静态字段（[静态字段和实例字段](classes.md#static-and-instance-fields)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1500">To initialize a new closed class type, first a new set of static fields ([Static and instance fields](classes.md#static-and-instance-fields)) for that particular closed type is created.</span></span> <span data-ttu-id="b24c2-1501">每个静态字段都初始化为其默认值（[默认值](variables.md#default-values)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1501">Each of the static fields is initialized to its default value ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="b24c2-1502">接下来，对这些静态字段执行静态字段初始值设定项（[静态字段初始化](classes.md#static-field-initialization)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1502">Next, the static field initializers ([Static field initialization](classes.md#static-field-initialization)) are executed for those static fields.</span></span> <span data-ttu-id="b24c2-1503">最后，执行静态构造函数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1503">Finally, the static constructor is executed.</span></span>

<span data-ttu-id="b24c2-1504">示例</span><span class="sxs-lookup"><span data-stu-id="b24c2-1504">The example</span></span>
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
<span data-ttu-id="b24c2-1505">必须生成输出：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1505">must produce the output:</span></span>
```console
Init A
A.F
Init B
B.F
```
<span data-ttu-id="b24c2-1506">因为对 `A.F`的调用触发了 `A`的静态构造函数的执行，并且 `B`的静态构造函数的执行由对 `B.F`的调用触发。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1506">because the execution of `A`'s static constructor is triggered by the call to `A.F`, and the execution of `B`'s static constructor is triggered by the call to `B.F`.</span></span>

<span data-ttu-id="b24c2-1507">可以构造循环依赖关系，以便在其默认值状态中观察包含变量初始值设定项的静态字段。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1507">It is possible to construct circular dependencies that allow static fields with variable initializers to be observed in their default value state.</span></span>

<span data-ttu-id="b24c2-1508">示例</span><span class="sxs-lookup"><span data-stu-id="b24c2-1508">The example</span></span>
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
<span data-ttu-id="b24c2-1509">生成输出</span><span class="sxs-lookup"><span data-stu-id="b24c2-1509">produces the output</span></span>
```console
X = 1, Y = 2
```

<span data-ttu-id="b24c2-1510">若要执行 `Main` 方法，系统首先在类 `B`的静态构造函数之前运行 `B.Y`的初始化表达式。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1510">To execute the `Main` method, the system first runs the initializer for `B.Y`, prior to class `B`'s static constructor.</span></span> <span data-ttu-id="b24c2-1511">`Y`初始值设定项会导致运行 `A`的静态构造函数，因为引用 `A.X` 的值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1511">`Y`'s initializer causes `A`'s static constructor to be run because the value of `A.X` is referenced.</span></span> <span data-ttu-id="b24c2-1512"> `A` 的静态构造函数将继续计算 `X`的值，并且在执行此操作时，将获取 `Y`的默认值，即零。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1512">The static constructor of `A` in turn proceeds to compute the value of `X`, and in doing so fetches the default value of `Y`, which is zero.</span></span> <span data-ttu-id="b24c2-1513">`A.X` 将初始化为1。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1513">`A.X` is thus initialized to 1.</span></span> <span data-ttu-id="b24c2-1514">运行 `A`的静态字段初始值设定项和静态构造函数的过程随后完成，返回 `Y`初始值的计算结果为2。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1514">The process of running `A`'s static field initializers and static constructor then completes, returning to the calculation of the initial value of `Y`, the result of which becomes 2.</span></span>

<span data-ttu-id="b24c2-1515">由于静态构造函数仅对每个封闭式构造类类型执行一次，因此，在不能通过约束（[类型参数约束](classes.md#type-parameter-constraints)）在编译时无法检查的类型参数上强制执行运行时检查。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1515">Because the static constructor is executed exactly once for each closed constructed class type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="b24c2-1516">例如，下面的类型使用静态构造函数来强制类型参数是一个枚举：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1516">For example, the following type uses a static constructor to enforce that the type argument is an enum:</span></span>
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

## <a name="destructors"></a><span data-ttu-id="b24c2-1517">析构函数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1517">Destructors</span></span>

<span data-ttu-id="b24c2-1518">***析构函数***是实现析构类的实例所需的操作的成员。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1518">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="b24c2-1519">使用*destructor_declaration*声明析构函数：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1519">A destructor is declared using a *destructor_declaration*:</span></span>

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

<span data-ttu-id="b24c2-1520">*Destructor_declaration*可以包含一组*属性*（[特性](attributes.md)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1520">A *destructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="b24c2-1521">*Destructor_declaration*的*标识符*必须命名声明析构函数的类。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1521">The *identifier* of a *destructor_declaration* must name the class in which the destructor is declared.</span></span> <span data-ttu-id="b24c2-1522">如果指定了其他名称，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1522">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="b24c2-1523">当析构函数声明包含 `extern` 修饰符时，析构函数被称为***外部析构函数***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1523">When a destructor declaration includes an `extern` modifier, the destructor is said to be an ***external destructor***.</span></span> <span data-ttu-id="b24c2-1524">由于外部析构函数声明不提供实际的实现，因此它的*destructor_body*由分号组成。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1524">Because an external destructor declaration provides no actual implementation, its *destructor_body* consists of a semicolon.</span></span> <span data-ttu-id="b24c2-1525">对于所有其他析构函数， *destructor_body*包含一个*块*，它指定要执行的语句，以便销毁类的实例。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1525">For all other destructors, the *destructor_body* consists of a *block* which specifies the statements to execute in order to destruct an instance of the class.</span></span> <span data-ttu-id="b24c2-1526">*Destructor_body*完全对应于具有 `void` 返回类型（[方法主体](classes.md#method-body)）的实例方法的*method_body* 。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1526">A *destructor_body* corresponds exactly to the *method_body* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="b24c2-1527">析构函数不是继承的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1527">Destructors are not inherited.</span></span> <span data-ttu-id="b24c2-1528">因此，一个类不包含析构函数，而析构函数可能在该类中声明。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1528">Thus, a class has no destructors other than the one which may be declared in that class.</span></span>

<span data-ttu-id="b24c2-1529">由于析构函数不需要任何参数，因此不能重载，因此一个类最多只能有一个析构函数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1529">Since a destructor is required to have no parameters, it cannot be overloaded, so a class can have, at most, one destructor.</span></span>

<span data-ttu-id="b24c2-1530">析构函数是自动调用的，不能被显式调用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1530">Destructors are invoked automatically, and cannot be invoked explicitly.</span></span> <span data-ttu-id="b24c2-1531">当某个实例对于任何代码都无法使用该实例时，该实例将可用于销毁。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1531">An instance becomes eligible for destruction when it is no longer possible for any code to use that instance.</span></span> <span data-ttu-id="b24c2-1532">在实例符合析构条件后，可能会在任何时间执行实例的析构函数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1532">Execution of the destructor for the instance may occur at any time after the instance becomes eligible for destruction.</span></span> <span data-ttu-id="b24c2-1533">当实例销毁时，该实例的继承链中的析构函数将按顺序调用，从最常派生到最小派生。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1533">When an instance is destructed, the destructors in that instance's inheritance chain are called, in order, from most derived to least derived.</span></span> <span data-ttu-id="b24c2-1534">析构函数可以在任何线程上执行。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1534">A destructor may be executed on any thread.</span></span> <span data-ttu-id="b24c2-1535">若要进一步讨论控制何时以及如何执行析构函数，请参阅[自动内存管理](basic-concepts.md#automatic-memory-management)。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1535">For further discussion of the rules that govern when and how a destructor is executed, see [Automatic memory management](basic-concepts.md#automatic-memory-management).</span></span>

<span data-ttu-id="b24c2-1536">示例的输出</span><span class="sxs-lookup"><span data-stu-id="b24c2-1536">The output of the example</span></span>
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
<span data-ttu-id="b24c2-1537">is</span><span class="sxs-lookup"><span data-stu-id="b24c2-1537">is</span></span>
```
B's destructor
A's destructor
```
<span data-ttu-id="b24c2-1538">由于继承链中的析构函数按顺序调用，从最常派生到最小派生。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1538">since destructors in an inheritance chain are called in order, from most derived to least derived.</span></span>

<span data-ttu-id="b24c2-1539">析构函数是通过重写 `System.Object`上的虚拟方法 `Finalize` 来实现的。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1539">Destructors are implemented by overriding the virtual method `Finalize` on `System.Object`.</span></span> <span data-ttu-id="b24c2-1540">C#不允许程序重写此方法或直接对其进行调用（或重写）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1540">C# programs are not permitted to override this method or call it (or overrides of it) directly.</span></span> <span data-ttu-id="b24c2-1541">例如，程序</span><span class="sxs-lookup"><span data-stu-id="b24c2-1541">For instance, the program</span></span>
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
<span data-ttu-id="b24c2-1542">包含两个错误。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1542">contains two errors.</span></span>

<span data-ttu-id="b24c2-1543">编译器的行为就像此方法和它的替代一样，根本就不存在。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1543">The compiler behaves as if this method, and overrides of it, do not exist at all.</span></span> <span data-ttu-id="b24c2-1544">因此，此程序：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1544">Thus, this program:</span></span>
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
<span data-ttu-id="b24c2-1545">有效，并显示的方法隐藏 `System.Object`的 `Finalize` 方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1545">is valid, and the method shown hides `System.Object`'s `Finalize` method.</span></span>

<span data-ttu-id="b24c2-1546">有关从析构函数引发异常时的行为的讨论，请参阅[如何处理异常](exceptions.md#how-exceptions-are-handled)。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1546">For a discussion of the behavior when an exception is thrown from a destructor, see [How exceptions are handled](exceptions.md#how-exceptions-are-handled).</span></span>

## <a name="iterators"></a><span data-ttu-id="b24c2-1547">迭代器</span><span class="sxs-lookup"><span data-stu-id="b24c2-1547">Iterators</span></span>

<span data-ttu-id="b24c2-1548">使用迭代器块（[块](statements.md#blocks)）实现的函数成员（[函数成员](expressions.md#function-members)）称为***迭代器***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1548">A function member ([Function members](expressions.md#function-members)) implemented using an iterator block ([Blocks](statements.md#blocks)) is called an ***iterator***.</span></span>

<span data-ttu-id="b24c2-1549">迭代器块可以用作函数成员的主体，前提是相应函数成员的返回类型是一个枚举器接口（[枚举器接口](classes.md#enumerator-interfaces)），或者是一个可枚举接口（可[枚举](classes.md#enumerable-interfaces)接口）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1549">An iterator block may be used as the body of a function member as long as the return type of the corresponding function member is one of the enumerator interfaces ([Enumerator interfaces](classes.md#enumerator-interfaces)) or one of the enumerable interfaces ([Enumerable interfaces](classes.md#enumerable-interfaces)).</span></span> <span data-ttu-id="b24c2-1550">它可以作为*method_body*、 *operator_body*或*accessor_body*出现，而事件、实例构造函数、静态构造函数和析构函数不能作为迭代器实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1550">It can occur as a *method_body*, *operator_body* or *accessor_body*, whereas events, instance constructors, static constructors and destructors cannot be implemented as iterators.</span></span>

<span data-ttu-id="b24c2-1551">使用迭代器块实现函数成员时，函数成员的形参表的编译时错误是指定任何 `ref` 或 `out` 参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1551">When a function member is implemented using an iterator block, it is a compile-time error for the formal parameter list of the function member to specify any `ref` or `out` parameters.</span></span>

### <a name="enumerator-interfaces"></a><span data-ttu-id="b24c2-1552">枚举器接口</span><span class="sxs-lookup"><span data-stu-id="b24c2-1552">Enumerator interfaces</span></span>

<span data-ttu-id="b24c2-1553">***枚举器接口***是 `System.Collections.IEnumerator` 的非泛型接口和泛型接口的所有实例化 `System.Collections.Generic.IEnumerator<T>`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1553">The ***enumerator interfaces*** are the non-generic interface `System.Collections.IEnumerator` and all instantiations of the generic interface `System.Collections.Generic.IEnumerator<T>`.</span></span> <span data-ttu-id="b24c2-1554">为了简单起见，在本章中，这些接口分别作为 `IEnumerator` 和 `IEnumerator<T>`进行引用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1554">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerator` and `IEnumerator<T>`, respectively.</span></span>

### <a name="enumerable-interfaces"></a><span data-ttu-id="b24c2-1555">可枚举接口</span><span class="sxs-lookup"><span data-stu-id="b24c2-1555">Enumerable interfaces</span></span>

<span data-ttu-id="b24c2-1556">可***枚举接口***是 `System.Collections.IEnumerable` 的非泛型接口和泛型接口的所有实例化 `System.Collections.Generic.IEnumerable<T>`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1556">The ***enumerable interfaces*** are the non-generic interface `System.Collections.IEnumerable` and all instantiations of the generic interface `System.Collections.Generic.IEnumerable<T>`.</span></span> <span data-ttu-id="b24c2-1557">为了简单起见，在本章中，这些接口分别作为 `IEnumerable` 和 `IEnumerable<T>`进行引用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1557">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerable` and `IEnumerable<T>`, respectively.</span></span>

### <a name="yield-type"></a><span data-ttu-id="b24c2-1558">Yield 类型</span><span class="sxs-lookup"><span data-stu-id="b24c2-1558">Yield type</span></span>

<span data-ttu-id="b24c2-1559">迭代器生成一个值序列，该序列具有相同的类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1559">An iterator produces a sequence of values, all of the same type.</span></span> <span data-ttu-id="b24c2-1560">此类型称为迭代器的***yield 类型***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1560">This type is called the ***yield type*** of the iterator.</span></span>

*  <span data-ttu-id="b24c2-1561">返回 `IEnumerator` 或 `IEnumerable` 的迭代器的 yield 类型 `object`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1561">The yield type of an iterator that returns `IEnumerator` or `IEnumerable` is `object`.</span></span>
*  <span data-ttu-id="b24c2-1562">返回 `IEnumerator<T>` 或 `IEnumerable<T>` 的迭代器的 yield 类型 `T`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1562">The yield type of an iterator that returns `IEnumerator<T>` or `IEnumerable<T>` is `T`.</span></span>

### <a name="enumerator-objects"></a><span data-ttu-id="b24c2-1563">枚举器对象</span><span class="sxs-lookup"><span data-stu-id="b24c2-1563">Enumerator objects</span></span>

<span data-ttu-id="b24c2-1564">当使用迭代器块实现返回枚举器接口类型的函数成员时，调用函数成员不会立即执行迭代器块中的代码。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1564">When a function member returning an enumerator interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="b24c2-1565">相反，将创建并返回一个***枚举器对象***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1565">Instead, an ***enumerator object*** is created and returned.</span></span> <span data-ttu-id="b24c2-1566">此对象封装在迭代器块中指定的代码，并在调用枚举器对象的 `MoveNext` 方法时，执行迭代器块中的代码。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1566">This object encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="b24c2-1567">枚举器对象具有以下特征：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1567">An enumerator object has the following characteristics:</span></span>

*  <span data-ttu-id="b24c2-1568">它实现 `IEnumerator` 和 `IEnumerator<T>`，其中 `T` 是迭代器的 yield 类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1568">It implements `IEnumerator` and `IEnumerator<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="b24c2-1569">它实现 `System.IDisposable`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1569">It implements `System.IDisposable`.</span></span>
*  <span data-ttu-id="b24c2-1570">使用传递到函数成员的参数值（如果有）和实例值的副本对其进行初始化。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1570">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>
*  <span data-ttu-id="b24c2-1571">它有四种可能的状态： "***之前***"、"***正在运行***"、"已***暂停***" 和 "***之后***"，最初处于***之前***的状态。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1571">It has four potential states, ***before***, ***running***, ***suspended***, and ***after***, and is initially in the ***before*** state.</span></span>

<span data-ttu-id="b24c2-1572">枚举器对象通常是编译器生成的枚举器类的一个实例，它封装迭代器块中的代码并实现枚举器接口，但也可以实现其他方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1572">An enumerator object is typically an instance of a compiler-generated enumerator class that encapsulates the code in the iterator block and implements the enumerator interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="b24c2-1573">如果枚举器类由编译器生成，则该类将直接或间接嵌套在包含函数成员的类中，它将具有私有可访问性，并且它将具有为编译器使用而保留的名称（[标识符](lexical-structure.md#identifiers)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1573">If an enumerator class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="b24c2-1574">枚举器对象可以实现比上面指定的接口更多的接口。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1574">An enumerator object may implement more interfaces than those specified above.</span></span>

<span data-ttu-id="b24c2-1575">以下各节描述了枚举器对象提供的 `IEnumerable` 和 `IEnumerable<T>` 接口实现的 `MoveNext`、`Current`和 `Dispose` 成员的确切行为。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1575">The following sections describe the exact behavior of the `MoveNext`, `Current`, and `Dispose` members of the `IEnumerable` and `IEnumerable<T>` interface implementations provided by an enumerator object.</span></span>

<span data-ttu-id="b24c2-1576">请注意，枚举器对象不支持 `IEnumerator.Reset` 方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1576">Note that enumerator objects do not support the `IEnumerator.Reset` method.</span></span> <span data-ttu-id="b24c2-1577">调用此方法将导致引发 `System.NotSupportedException`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1577">Invoking this method causes a `System.NotSupportedException` to be thrown.</span></span>

#### <a name="the-movenext-method"></a><span data-ttu-id="b24c2-1578">MoveNext 方法</span><span class="sxs-lookup"><span data-stu-id="b24c2-1578">The MoveNext method</span></span>

<span data-ttu-id="b24c2-1579">枚举器对象的 `MoveNext` 方法封装迭代器块的代码。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1579">The `MoveNext` method of an enumerator object encapsulates the code of an iterator block.</span></span> <span data-ttu-id="b24c2-1580">调用 `MoveNext` 方法将执行迭代器块中的代码，并根据需要设置枚举器对象的 `Current` 属性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1580">Invoking the `MoveNext` method executes code in the iterator block and sets the `Current` property of the enumerator object as appropriate.</span></span> <span data-ttu-id="b24c2-1581">`MoveNext` 执行的精确操作取决于调用 `MoveNext` 时枚举器对象的状态：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1581">The precise action performed by `MoveNext` depends on the state of the enumerator object when `MoveNext` is invoked:</span></span>

*  <span data-ttu-id="b24c2-1582">如果枚举器对象的状态在***之前***，则调用 `MoveNext`：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1582">If the state of the enumerator object is ***before***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="b24c2-1583">将状态更改为***正在运行***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1583">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="b24c2-1584">将迭代器块的参数（包括 `this`）初始化到参数值和初始化枚举器对象时保存的实例值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1584">Initializes the parameters (including `this`) of the iterator block to the argument values and instance value saved when the enumerator object was initialized.</span></span>
   * <span data-ttu-id="b24c2-1585">从一开始就执行迭代器块，直到中断执行（如下所述）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1585">Executes the iterator block from the beginning until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="b24c2-1586">如果枚举器对象的状态为***正在运行***，则不指定调用 `MoveNext` 的结果。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1586">If the state of the enumerator object is ***running***, the result of invoking `MoveNext` is unspecified.</span></span>
*  <span data-ttu-id="b24c2-1587">如果枚举器对象的状态为 "已***挂起***"，则调用 `MoveNext`：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1587">If the state of the enumerator object is ***suspended***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="b24c2-1588">将状态更改为***正在运行***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1588">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="b24c2-1589">将所有局部变量和参数的值（包括此值）还原到上次挂起执行迭代器块时保存的值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1589">Restores the values of all local variables and parameters (including this) to the values saved when execution of the iterator block was last suspended.</span></span> <span data-ttu-id="b24c2-1590">请注意，这些变量所引用的任何对象的内容可能自上一次调用 MoveNext 后发生了更改。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1590">Note that the contents of any objects referenced by these variables may have changed since the previous call to MoveNext.</span></span>
   * <span data-ttu-id="b24c2-1591">在导致执行挂起的 `yield return` 语句之后立即继续执行迭代器块，并继续执行，直到中断执行（如下所述）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1591">Resumes execution of the iterator block immediately following the `yield return` statement that caused the suspension of execution and continues until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="b24c2-1592">如果枚举数对象的状态为***after***，则调用 `MoveNext` 将返回 `false`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1592">If the state of the enumerator object is ***after***, invoking `MoveNext` returns `false`.</span></span>


<span data-ttu-id="b24c2-1593">当 `MoveNext` 执行迭代器块时，可以通过以下四种方式中断执行：通过 `yield return` 语句、`yield break` 语句、遇到迭代器块的末尾以及引发的异常并传播到迭代器块中。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1593">When `MoveNext` executes the iterator block, execution can be interrupted in four ways: By a `yield return` statement, by a `yield break` statement, by encountering the end of the iterator block, and by an exception being thrown and propagated out of the iterator block.</span></span>

*  <span data-ttu-id="b24c2-1594">遇到 `yield return` 语句时（[yield 语句](statements.md#the-yield-statement)）：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1594">When a `yield return` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="b24c2-1595">计算语句中给定的表达式，将其隐式转换为 yield 类型，并将其分配给枚举器对象的 `Current` 属性。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1595">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
   * <span data-ttu-id="b24c2-1596">迭代器主体的执行将被挂起。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1596">Execution of the iterator body is suspended.</span></span> <span data-ttu-id="b24c2-1597">保存所有局部变量和参数的值（包括 `this`），这与此 `yield return` 语句的位置相同。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1597">The values of all local variables and parameters (including `this`) are saved, as is the location of this `yield return` statement.</span></span> <span data-ttu-id="b24c2-1598">如果 `yield return` 语句位于一个或多个 `try` 块中，则不会执行关联的 `finally` 块。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1598">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
   * <span data-ttu-id="b24c2-1599">枚举器对象的状态将更改为 "已***挂起***"。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1599">The state of the enumerator object is changed to ***suspended***.</span></span>
   * <span data-ttu-id="b24c2-1600">`MoveNext` 方法将 `true` 返回到其调用方，这表示迭代已成功地推进到下一个值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1600">The `MoveNext` method returns `true` to its caller, indicating that the iteration successfully advanced to the next value.</span></span>
*  <span data-ttu-id="b24c2-1601">遇到 `yield break` 语句时（[yield 语句](statements.md#the-yield-statement)）：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1601">When a `yield break` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="b24c2-1602">如果 `yield break` 语句在一个或多个 `try` 块中，则将执行关联的 `finally` 块。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1602">If the `yield break` statement is within one or more `try` blocks, the associated `finally` blocks are executed.</span></span>
   * <span data-ttu-id="b24c2-1603">枚举器对象的状态将更改为***after***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1603">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="b24c2-1604">`MoveNext` 方法将 `false` 返回到其调用方，指示迭代已完成。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1604">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="b24c2-1605">当遇到迭代器主体的末尾时：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1605">When the end of the iterator body is encountered:</span></span>
   * <span data-ttu-id="b24c2-1606">枚举器对象的状态将更改为***after***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1606">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="b24c2-1607">`MoveNext` 方法将 `false` 返回到其调用方，指示迭代已完成。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1607">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="b24c2-1608">当引发异常并将其传播到迭代器块外时：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1608">When an exception is thrown and propagated out of the iterator block:</span></span>
   * <span data-ttu-id="b24c2-1609">迭代器正文中的相应 `finally` 块将由异常传播执行。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1609">Appropriate `finally` blocks in the iterator body will have been executed by the exception propagation.</span></span>
   * <span data-ttu-id="b24c2-1610">枚举器对象的状态将更改为***after***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1610">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="b24c2-1611">异常传播继续到 `MoveNext` 方法的调用方。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1611">The exception propagation continues to the caller of the `MoveNext` method.</span></span>

#### <a name="the-current-property"></a><span data-ttu-id="b24c2-1612">当前属性</span><span class="sxs-lookup"><span data-stu-id="b24c2-1612">The Current property</span></span>

<span data-ttu-id="b24c2-1613">枚举器对象的 `Current` 属性受迭代器块中 `yield return` 语句的影响。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1613">An enumerator object's `Current` property is affected by `yield return` statements in the iterator block.</span></span>

<span data-ttu-id="b24c2-1614">当枚举器对象处于***挂起***状态时，`Current` 的值为之前对 `MoveNext`所设置的值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1614">When an enumerator object is in the ***suspended*** state, the value of `Current` is the value set by the previous call to `MoveNext`.</span></span> <span data-ttu-id="b24c2-1615">如果枚举器对象位于 "***之前***"、"***正在运行***" 或 "***之后***" 状态，则不指定访问 `Current` 的结果。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1615">When an enumerator object is in the ***before***, ***running***, or ***after*** states, the result of accessing `Current` is unspecified.</span></span>

<span data-ttu-id="b24c2-1616">对于 yield 类型不是 `object`的迭代器，通过枚举器对象的 `IEnumerable` 实现访问 `Current` 的结果对应于通过枚举器对象的 `IEnumerator<T>` 实现访问 `Current`，并将结果强制转换为 `object`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1616">For an iterator with a yield type other than `object`, the result of accessing `Current` through the enumerator object's `IEnumerable` implementation corresponds to accessing `Current` through the enumerator object's `IEnumerator<T>` implementation and casting the result to `object`.</span></span>

#### <a name="the-dispose-method"></a><span data-ttu-id="b24c2-1617">Dispose 方法</span><span class="sxs-lookup"><span data-stu-id="b24c2-1617">The Dispose method</span></span>

<span data-ttu-id="b24c2-1618">`Dispose` 方法用于通过将枚举器对象的状态设置为***after***来清理迭代。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1618">The `Dispose` method is used to clean up the iteration by bringing the enumerator object to the ***after*** state.</span></span>

*  <span data-ttu-id="b24c2-1619">如果枚举数对象的状态在***之前***，则调用 `Dispose` 会将状态更改为***after***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1619">If the state of the enumerator object is ***before***, invoking `Dispose` changes the state to ***after***.</span></span>
*  <span data-ttu-id="b24c2-1620">如果枚举器对象的状态为***正在运行***，则不指定调用 `Dispose` 的结果。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1620">If the state of the enumerator object is ***running***, the result of invoking `Dispose` is unspecified.</span></span>
*  <span data-ttu-id="b24c2-1621">如果枚举器对象的状态为 "已***挂起***"，则调用 `Dispose`：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1621">If the state of the enumerator object is ***suspended***, invoking `Dispose`:</span></span>
   * <span data-ttu-id="b24c2-1622">将状态更改为***正在运行***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1622">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="b24c2-1623">执行所有 finally 块，就好像最后执行的 `yield return` 语句是 `yield break` 语句一样。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1623">Executes any finally blocks as if the last executed `yield return` statement were a `yield break` statement.</span></span> <span data-ttu-id="b24c2-1624">如果这导致引发异常并将其传播到迭代器主体外，则枚举器对象的状态将设置为***after*** ，并将异常传播到 `Dispose` 方法的调用方。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1624">If this causes an exception to be thrown and propagated out of the iterator body, the state of the enumerator object is set to ***after*** and the exception is propagated to the caller of the `Dispose` method.</span></span>
   * <span data-ttu-id="b24c2-1625">将状态更改为***after***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1625">Changes the state to ***after***.</span></span>
*  <span data-ttu-id="b24c2-1626">如果枚举数对象的状态为***after***，则调用 `Dispose` 不会有任何影响。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1626">If the state of the enumerator object is ***after***, invoking `Dispose` has no affect.</span></span>

### <a name="enumerable-objects"></a><span data-ttu-id="b24c2-1627">可枚举对象</span><span class="sxs-lookup"><span data-stu-id="b24c2-1627">Enumerable objects</span></span>

<span data-ttu-id="b24c2-1628">当使用迭代器块实现返回可枚举接口类型的函数成员时，调用函数成员不会立即执行迭代器块中的代码。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1628">When a function member returning an enumerable interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="b24c2-1629">相反，将创建并返回一个可***枚举对象***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1629">Instead, an ***enumerable object*** is created and returned.</span></span> <span data-ttu-id="b24c2-1630">可枚举对象的 `GetEnumerator` 方法返回一个枚举器对象，该对象封装迭代器块中指定的代码，并在调用枚举器对象的 `MoveNext` 方法时，执行迭代器块中的代码。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1630">The enumerable object's `GetEnumerator` method returns an enumerator object that encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="b24c2-1631">可枚举对象具有以下特征：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1631">An enumerable object has the following characteristics:</span></span>

*  <span data-ttu-id="b24c2-1632">它实现 `IEnumerable` 和 `IEnumerable<T>`，其中 `T` 是迭代器的 yield 类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1632">It implements `IEnumerable` and `IEnumerable<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="b24c2-1633">使用传递到函数成员的参数值（如果有）和实例值的副本对其进行初始化。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1633">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>

<span data-ttu-id="b24c2-1634">可枚举对象通常是编译器生成的可枚举类的实例，它封装迭代器块中的代码并实现可枚举接口，但也可以实现其他方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1634">An enumerable object is typically an instance of a compiler-generated enumerable class that encapsulates the code in the iterator block and implements the enumerable interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="b24c2-1635">如果可枚举的类由编译器生成，则该类将直接或间接嵌套在包含函数成员的类中，它将具有私有可访问性，并且它将具有为编译器使用而保留的名称（[标识符](lexical-structure.md#identifiers)）。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1635">If an enumerable class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="b24c2-1636">可枚举对象可以实现比上面指定的接口更多的接口。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1636">An enumerable object may implement more interfaces than those specified above.</span></span> <span data-ttu-id="b24c2-1637">具体而言，可枚举对象还可以实现 `IEnumerator` 和 `IEnumerator<T>`，从而使其既可用作可枚举的，也可用作枚举器。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1637">In particular, an enumerable object may also implement `IEnumerator` and `IEnumerator<T>`, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="b24c2-1638">在该类型的实现中，第一次调用可枚举对象的 `GetEnumerator` 方法时，将返回可枚举对象本身。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1638">In that type of implementation, the first time an enumerable object's `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="b24c2-1639">对可枚举对象的 `GetEnumerator`（如果有）的后续调用会返回可枚举对象的副本。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1639">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="b24c2-1640">因此，每个返回的枚举器都有其自己的状态，并且一个枚举器中的更改不会影响另一个枚举</span><span class="sxs-lookup"><span data-stu-id="b24c2-1640">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span>

#### <a name="the-getenumerator-method"></a><span data-ttu-id="b24c2-1641">GetEnumerator 方法</span><span class="sxs-lookup"><span data-stu-id="b24c2-1641">The GetEnumerator method</span></span>

<span data-ttu-id="b24c2-1642">可枚举对象提供 `IEnumerable` 和 `IEnumerable<T>` 接口的 `GetEnumerator` 方法的实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1642">An enumerable object provides an implementation of the `GetEnumerator` methods of the `IEnumerable` and `IEnumerable<T>` interfaces.</span></span> <span data-ttu-id="b24c2-1643">这两个 `GetEnumerator` 方法共享一个公共实现，该实现获取并返回可用的枚举器对象。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1643">The two `GetEnumerator` methods share a common implementation that acquires and returns an available enumerator object.</span></span> <span data-ttu-id="b24c2-1644">初始化枚举器对象时，将在初始化可枚举对象时保存的参数值和实例值进行初始化，否则枚举器对象的功能如[枚举器对象](classes.md#enumerator-objects)中所述。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1644">The enumerator object is initialized with the argument values and instance value saved when the enumerable object was initialized, but otherwise the enumerator object functions as described in [Enumerator objects](classes.md#enumerator-objects).</span></span>

### <a name="implementation-example"></a><span data-ttu-id="b24c2-1645">实现示例</span><span class="sxs-lookup"><span data-stu-id="b24c2-1645">Implementation example</span></span>

<span data-ttu-id="b24c2-1646">本部分介绍了在标准C#构造中可能的迭代器实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1646">This section describes a possible implementation of iterators in terms of standard C# constructs.</span></span> <span data-ttu-id="b24c2-1647">此处所述的实现基于 Microsoft C#编译器使用的相同原则，但它并不意味着强制实施或唯一可能。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1647">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation or the only one possible.</span></span>

<span data-ttu-id="b24c2-1648">以下 `Stack<T>` 类使用迭代器来实现其 `GetEnumerator` 方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1648">The following `Stack<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="b24c2-1649">迭代器按从上到下的顺序枚举堆栈中的元素。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1649">The iterator enumerates the elements of the stack in top to bottom order.</span></span>

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

<span data-ttu-id="b24c2-1650">可以将 `GetEnumerator` 方法转换为编译器生成的枚举器类的实例化，该类封装迭代器块中的代码，如下所示。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1650">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="b24c2-1651">在前面的转换中，迭代器块中的代码将转换为状态机并放置在枚举器类的 `MoveNext` 方法中。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1651">In the preceding translation, the code in the iterator block is turned into a state machine and placed in the `MoveNext` method of the enumerator class.</span></span> <span data-ttu-id="b24c2-1652">此外，局部变量 `i` 会转换为枚举器对象中的字段，因此它可以在 `MoveNext`的调用中继续存在。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1652">Furthermore, the local variable `i` is turned into a field in the enumerator object so it can continue to exist across invocations of `MoveNext`.</span></span>

<span data-ttu-id="b24c2-1653">下面的示例将打印整数1到10的简单乘法表。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1653">The following example prints a simple multiplication table of the integers 1 through 10.</span></span> <span data-ttu-id="b24c2-1654">示例中的 `FromTo` 方法返回一个可枚举对象，并使用迭代器实现。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1654">The `FromTo` method in the example returns an enumerable object and is implemented using an iterator.</span></span>

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

<span data-ttu-id="b24c2-1655">可以将 `FromTo` 方法转换为编译器生成的可枚举类的实例化，该枚举类封装迭代器块中的代码，如下所示。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1655">The `FromTo` method can be translated into an instantiation of a compiler-generated enumerable class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="b24c2-1656">可枚举的类既实现可枚举接口又实现枚举器接口，使其既可用作可枚举的，也可用作枚举器。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1656">The enumerable class implements both the enumerable interfaces and the enumerator interfaces, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="b24c2-1657">第一次调用 `GetEnumerator` 方法时，将返回可枚举对象本身。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1657">The first time the `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="b24c2-1658">对可枚举对象的 `GetEnumerator`（如果有）的后续调用会返回可枚举对象的副本。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1658">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="b24c2-1659">因此，每个返回的枚举器都有其自己的状态，并且一个枚举器中的更改不会影响另一个枚举</span><span class="sxs-lookup"><span data-stu-id="b24c2-1659">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span> <span data-ttu-id="b24c2-1660">`Interlocked.CompareExchange` 方法用于确保线程安全的操作。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1660">The `Interlocked.CompareExchange` method is used to ensure thread-safe operation.</span></span>

<span data-ttu-id="b24c2-1661">`from` 和 `to` 参数会转换为可枚举类中的字段。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1661">The `from` and `to` parameters are turned into fields in the enumerable class.</span></span> <span data-ttu-id="b24c2-1662">因为在迭代器块中修改了 `from`，所以引入了一个附加的 `__from` 字段来保存提供给每个枚举器中的 `from` 的初始值。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1662">Because `from` is modified in the iterator block, an additional `__from` field is introduced to hold the initial value given to `from` in each enumerator.</span></span>

<span data-ttu-id="b24c2-1663">如果 `0``__state`，则 `MoveNext` 方法会引发 `InvalidOperationException`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1663">The `MoveNext` method throws an `InvalidOperationException` if it is called when `__state` is `0`.</span></span> <span data-ttu-id="b24c2-1664">这可以防止将可枚举对象用作枚举器对象，而无需先调用 `GetEnumerator`。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1664">This protects against use of the enumerable object as an enumerator object without first calling `GetEnumerator`.</span></span>

<span data-ttu-id="b24c2-1665">下面的示例演示一个简单的 tree 类。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1665">The following example shows a simple tree class.</span></span> <span data-ttu-id="b24c2-1666">`Tree<T>` 类使用迭代器来实现其 `GetEnumerator` 方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1666">The `Tree<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="b24c2-1667">迭代器按中缀顺序枚举树的元素。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1667">The iterator enumerates the elements of the tree in infix order.</span></span>

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

<span data-ttu-id="b24c2-1668">可以将 `GetEnumerator` 方法转换为编译器生成的枚举器类的实例化，该类封装迭代器块中的代码，如下所示。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1668">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="b24c2-1669">将 `foreach` 语句中使用的编译器生成的临时内存提升为枚举器对象的 `__left` 和 `__right` 字段。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1669">The compiler generated temporaries used in the `foreach` statements are lifted into the `__left` and `__right` fields of the enumerator object.</span></span> <span data-ttu-id="b24c2-1670">将仔细更新枚举器对象的 `__state` 字段，以便在引发异常时正确调用正确的 `Dispose()` 方法。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1670">The `__state` field of the enumerator object is carefully updated so that the correct `Dispose()` method will be called correctly if an exception is thrown.</span></span> <span data-ttu-id="b24c2-1671">请注意，不能用简单的 `foreach` 语句来编写翻译后的代码。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1671">Note that it is not possible to write the translated code with simple `foreach` statements.</span></span>

## <a name="async-functions"></a><span data-ttu-id="b24c2-1672">异步函数</span><span class="sxs-lookup"><span data-stu-id="b24c2-1672">Async functions</span></span>

<span data-ttu-id="b24c2-1673">带有 `async` 修饰符的方法（[方法](classes.md#methods)）或匿名函数（[匿名函数表达式](expressions.md#anonymous-function-expressions)）称为***异步函数***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1673">A method ([Methods](classes.md#methods)) or anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) with the `async` modifier is called an ***async function***.</span></span> <span data-ttu-id="b24c2-1674">通常，术语***async***用于描述具有 `async` 修饰符的任何类型的函数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1674">In general, the term ***async*** is used to describe any kind of function that has the `async` modifier.</span></span>

<span data-ttu-id="b24c2-1675">异步函数的形参列表的编译时错误是指定任何 `ref` 或 `out` 参数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1675">It is a compile-time error for the formal parameter list of an async function to specify any `ref` or `out` parameters.</span></span>

<span data-ttu-id="b24c2-1676">异步方法的*return_type*必须是 `void` 或***任务类型***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1676">The *return_type* of an async method must be either `void` or a ***task type***.</span></span> <span data-ttu-id="b24c2-1677">任务类型是 `System.Threading.Tasks.Task` 的，并且是从 `System.Threading.Tasks.Task<T>`构造的类型。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1677">The task types are `System.Threading.Tasks.Task` and types constructed from `System.Threading.Tasks.Task<T>`.</span></span> <span data-ttu-id="b24c2-1678">为了简单起见，在本章中，这些类型分别作为 `Task` 和 `Task<T>`进行引用。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1678">For the sake of brevity, in this chapter these types are referenced as `Task` and `Task<T>`, respectively.</span></span> <span data-ttu-id="b24c2-1679">返回任务类型的异步方法称作任务返回。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1679">An async method returning a task type is said to be task-returning.</span></span>

<span data-ttu-id="b24c2-1680">任务类型的确切定义是定义的实现，但从语言的角度来看，任务类型处于 "未完成"、"成功" 或 "出错" 状态之一。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1680">The exact definition of the task types is implementation defined, but from the language's point of view a task type is in one of the states incomplete, succeeded or faulted.</span></span> <span data-ttu-id="b24c2-1681">出错的任务记录相关的异常。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1681">A faulted task records a pertinent exception.</span></span> <span data-ttu-id="b24c2-1682">成功的 `Task<T>` 记录 `T`类型的结果。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1682">A succeeded `Task<T>` records a result of type `T`.</span></span> <span data-ttu-id="b24c2-1683">任务类型是可等待的，因此可以是 await 表达式（[await 表达式](expressions.md#await-expressions)）的操作数。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1683">Task types are awaitable, and can therefore be the operands of await expressions ([Await expressions](expressions.md#await-expressions)).</span></span>

<span data-ttu-id="b24c2-1684">异步函数调用能够通过其主体中的 await 表达式（[await 表达式](expressions.md#await-expressions)）来挂起计算。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1684">An async function invocation has the ability to suspend evaluation by means of await expressions ([Await expressions](expressions.md#await-expressions)) in its body.</span></span> <span data-ttu-id="b24c2-1685">稍后可以通过***恢复委托***在挂起 await 表达式时恢复计算。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1685">Evaluation may later be resumed at the point of the suspending await expression by means of a ***resumption delegate***.</span></span> <span data-ttu-id="b24c2-1686">恢复委托的类型为 `System.Action`，当调用该函数时，异步函数调用的计算将从其停止的等待表达式中继续。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1686">The resumption delegate is of type `System.Action`, and when it is invoked, evaluation of the async function invocation will resume from the await expression where it left off.</span></span> <span data-ttu-id="b24c2-1687">如果从未挂起函数调用，则异步函数调用的***当前调用方***是原始调用方，否则为恢复委托的最新调用方。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1687">The ***current caller*** of an async function invocation is the original caller if the function invocation has never been suspended, or the most recent caller of the resumption delegate otherwise.</span></span>

### <a name="evaluation-of-a-task-returning-async-function"></a><span data-ttu-id="b24c2-1688">任务返回的异步函数的计算</span><span class="sxs-lookup"><span data-stu-id="b24c2-1688">Evaluation of a task-returning async function</span></span>

<span data-ttu-id="b24c2-1689">调用任务返回的 async 函数会导致生成返回的任务类型的实例。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1689">Invocation of a task-returning async function causes an instance of the returned task type to be generated.</span></span> <span data-ttu-id="b24c2-1690">这称为 async 函数的***返回任务***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1690">This is called the ***return task*** of the async function.</span></span> <span data-ttu-id="b24c2-1691">任务最初处于 "未完成" 状态。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1691">The task is initially in an incomplete state.</span></span>

<span data-ttu-id="b24c2-1692">然后，将计算异步函数主体，直到它被挂起（通过等待表达式）或终止，此时，将控件返回给调用方，同时返回 task。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1692">The async function body is then evaluated until it is either suspended (by reaching an await expression) or terminates, at which point control is returned to the caller, along with the return task.</span></span>

<span data-ttu-id="b24c2-1693">当 async 函数的主体终止时，返回的任务将不再处于 "未完成" 状态：</span><span class="sxs-lookup"><span data-stu-id="b24c2-1693">When the body of the async function terminates, the return task is moved out of the incomplete state:</span></span>

*  <span data-ttu-id="b24c2-1694">如果函数体因到达 return 语句或正文末尾而终止，则返回的任务中会记录任何结果值，这会置于成功状态。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1694">If the function body terminates as the result of reaching a return statement or the end of the body, any result value is recorded in the return task, which is put into a succeeded state.</span></span>
*  <span data-ttu-id="b24c2-1695">如果函数体由于未捕获的异常而终止（[throw 语句](statements.md#the-throw-statement)），则会在返回任务中记录异常，并将其置于出错状态。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1695">If the function body terminates as the result of an uncaught exception ([The throw statement](statements.md#the-throw-statement)) the exception is recorded in the return task which is put into a faulted state.</span></span>

### <a name="evaluation-of-a-void-returning-async-function"></a><span data-ttu-id="b24c2-1696">Void 返回的 async 函数的计算</span><span class="sxs-lookup"><span data-stu-id="b24c2-1696">Evaluation of a void-returning async function</span></span>

<span data-ttu-id="b24c2-1697">如果异步函数的返回类型为 `void`，则计算方式与上述方法不同：由于没有返回任何任务，因此函数会将完成和异常传达给当前线程的***同步上下文***。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1697">If the return type of the async function is `void`, evaluation differs from the above in the following way: Because no task is returned, the function instead communicates completion and exceptions to the current thread's ***synchronization context***.</span></span> <span data-ttu-id="b24c2-1698">同步上下文的确切定义是依赖于实现的，但它表示当前线程正在运行的 "where"。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1698">The exact definition of synchronization context is implementation-dependent, but is a representation of "where" the current thread is running.</span></span> <span data-ttu-id="b24c2-1699">当对返回 void 的异步函数开始、成功完成或导致引发未捕获的异常时，会通知同步上下文。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1699">The synchronization context is notified when evaluation of a void-returning async function commences, completes successfully, or causes an uncaught exception to be thrown.</span></span>

<span data-ttu-id="b24c2-1700">这样，上下文就可以跟踪正在其下运行的空返回异步函数的数量，并决定如何传播它们发出的异常。</span><span class="sxs-lookup"><span data-stu-id="b24c2-1700">This allows the context to keep track of how many void-returning async functions are running under it, and to decide how to propagate exceptions coming out of them.</span></span>
