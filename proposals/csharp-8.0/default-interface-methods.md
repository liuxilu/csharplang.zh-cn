---
ms.openlocfilehash: b0d0fa70d90f7493c6c23be576155a77cec36cf8
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/14/2020
ms.locfileid: "79484080"
---
# <a name="default-interface-methods"></a><span data-ttu-id="0b736-101">默认接口方法</span><span class="sxs-lookup"><span data-stu-id="0b736-101">default interface methods</span></span>

* <span data-ttu-id="0b736-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="0b736-102">[x] Proposed</span></span>
* <span data-ttu-id="0b736-103">[] 原型：[正在进行](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)</span><span class="sxs-lookup"><span data-stu-id="0b736-103">[ ] Prototype: [In progress](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)</span></span>
* <span data-ttu-id="0b736-104">[] 实现：无</span><span class="sxs-lookup"><span data-stu-id="0b736-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="0b736-105">[] 规范：正在进行，如下所示</span><span class="sxs-lookup"><span data-stu-id="0b736-105">[ ] Specification: In progress, below</span></span>

## <a name="summary"></a><span data-ttu-id="0b736-106">总结</span><span class="sxs-lookup"><span data-stu-id="0b736-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="0b736-107">添加对_虚拟扩展方法_的支持-具有具体实现的接口中的方法。</span><span class="sxs-lookup"><span data-stu-id="0b736-107">Add support for _virtual extension methods_ - methods in interfaces with concrete implementations.</span></span> <span data-ttu-id="0b736-108">实现此类接口的类或结构需要具有接口方法的单个_特定_实现，该实现由类或结构实现，或从其基类或接口继承。</span><span class="sxs-lookup"><span data-stu-id="0b736-108">A class or struct that implements such an interface is required to have a single _most specific_ implementation for the interface method, either implemented by the class or struct, or inherited from its base classes or interfaces.</span></span> <span data-ttu-id="0b736-109">利用虚拟扩展方法，API 作者可以在未来版本中将方法添加到接口，而不会对该接口的现有实现中断源或二进制文件的兼容性。</span><span class="sxs-lookup"><span data-stu-id="0b736-109">Virtual extension methods enable an API author to add methods to an interface in future versions without breaking source or binary compatibility with existing implementations of that interface.</span></span>

<span data-ttu-id="0b736-110">它们类似于 Java 的["默认方法"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)。</span><span class="sxs-lookup"><span data-stu-id="0b736-110">These are similar to Java's ["Default Methods"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html).</span></span>

<span data-ttu-id="0b736-111">（基于可能的实现方法）此功能需要 CLI/CLR 中的相应支持。</span><span class="sxs-lookup"><span data-stu-id="0b736-111">(Based on the likely implementation technique) this feature requires corresponding support in the CLI/CLR.</span></span> <span data-ttu-id="0b736-112">利用此功能的程序不能在早期版本的平台上运行。</span><span class="sxs-lookup"><span data-stu-id="0b736-112">Programs that take advantage of this feature cannot run on earlier versions of the platform.</span></span>

## <a name="motivation"></a><span data-ttu-id="0b736-113">动机</span><span class="sxs-lookup"><span data-stu-id="0b736-113">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="0b736-114">此功能的主要动机是</span><span class="sxs-lookup"><span data-stu-id="0b736-114">The principal motivations for this feature are</span></span>

- <span data-ttu-id="0b736-115">使用默认接口方法，API 作者可以在未来版本中将方法添加到接口，而不会对该接口的现有实现中断源或二进制文件的兼容性。</span><span class="sxs-lookup"><span data-stu-id="0b736-115">Default interface methods enable an API author to add methods to an interface in future versions without breaking source or binary compatibility with existing implementations of that interface.</span></span>
- <span data-ttu-id="0b736-116">利用此功能C# ，可以与面向[Android （Java）](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)和[IOS （Swift）](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267)的 api 进行互操作，这种功能支持相似的功能。</span><span class="sxs-lookup"><span data-stu-id="0b736-116">The feature enables C# to interoperate with APIs targeting [Android (Java)](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html) and [iOS (Swift)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267), which support similar features.</span></span>
- <span data-ttu-id="0b736-117">事实证明，添加默认接口实现提供 "特征" 语言功能的元素（<https://en.wikipedia.org/wiki/Trait_(computer_programming)>）。</span><span class="sxs-lookup"><span data-stu-id="0b736-117">As it turns out, adding default interface implementations provides the elements of the "traits" language feature (<https://en.wikipedia.org/wiki/Trait_(computer_programming)>).</span></span> <span data-ttu-id="0b736-118">特性证明是一种功能强大的编程技术（<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>）。</span><span class="sxs-lookup"><span data-stu-id="0b736-118">Traits have proven to be a powerful programming technique (<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>).</span></span>

## <a name="detailed-design"></a><span data-ttu-id="0b736-119">详细设计</span><span class="sxs-lookup"><span data-stu-id="0b736-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="0b736-120">接口的语法已扩展为允许</span><span class="sxs-lookup"><span data-stu-id="0b736-120">The syntax for an interface is extended to permit</span></span>

- <span data-ttu-id="0b736-121">声明常量、运算符、静态构造函数和嵌套类型的成员声明;</span><span class="sxs-lookup"><span data-stu-id="0b736-121">member declarations that declare constants, operators, static constructors, and nested types;</span></span>
- <span data-ttu-id="0b736-122">方法或索引器、属性或事件访问器（即 "默认" 实现）的*主体*;</span><span class="sxs-lookup"><span data-stu-id="0b736-122">a *body* for a method or indexer, property, or event accessor (that is, a "default" implementation);</span></span>
- <span data-ttu-id="0b736-123">声明静态字段、方法、属性、索引器和事件的成员声明;</span><span class="sxs-lookup"><span data-stu-id="0b736-123">member declarations that declare static fields, methods, properties, indexers, and events;</span></span>
- <span data-ttu-id="0b736-124">使用显式接口实现语法的成员声明;与</span><span class="sxs-lookup"><span data-stu-id="0b736-124">member declarations using the explicit interface implementation syntax; and</span></span>
- <span data-ttu-id="0b736-125">显式访问修饰符（`public`的默认访问权限）。</span><span class="sxs-lookup"><span data-stu-id="0b736-125">Explicit access modifiers (the default access is `public`).</span></span>

<span data-ttu-id="0b736-126">具有主体的成员允许接口为不提供重写实现的类和结构中的方法提供 "默认" 实现。</span><span class="sxs-lookup"><span data-stu-id="0b736-126">Members with bodies permit the interface to provide a "default" implementation for the method in classes and structs that do not provide an overriding implementation.</span></span>

<span data-ttu-id="0b736-127">接口不能包含实例状态。</span><span class="sxs-lookup"><span data-stu-id="0b736-127">Interfaces may not contain instance state.</span></span> <span data-ttu-id="0b736-128">虽然现在允许使用静态字段，但接口中不允许使用实例字段。</span><span class="sxs-lookup"><span data-stu-id="0b736-128">While static fields are now permitted, instance fields are not permitted in interfaces.</span></span> <span data-ttu-id="0b736-129">接口中不支持实例自动属性，因为它们会隐式声明隐藏的字段。</span><span class="sxs-lookup"><span data-stu-id="0b736-129">Instance auto-properties are not supported in interfaces, as they would implicitly declare a hidden field.</span></span>

<span data-ttu-id="0b736-130">静态方法和私有方法允许使用用于实现接口的公共 API 的代码的实用重构和组织。</span><span class="sxs-lookup"><span data-stu-id="0b736-130">Static and private methods permit useful refactoring and organization of code used to implement the interface's public API.</span></span>

<span data-ttu-id="0b736-131">接口中的方法重写必须使用显式接口实现语法。</span><span class="sxs-lookup"><span data-stu-id="0b736-131">A method override in an interface must use the explicit interface implementation syntax.</span></span>

<span data-ttu-id="0b736-132">在使用*variance_annotation*声明的类型形参的范围内声明类类型、结构类型或枚举类型是错误的。</span><span class="sxs-lookup"><span data-stu-id="0b736-132">It is an error to declare a class type, struct type, or enum type within the scope of a type parameter that was declared with a *variance_annotation*.</span></span>  <span data-ttu-id="0b736-133">例如，下面的 `C` 的声明是错误的。</span><span class="sxs-lookup"><span data-stu-id="0b736-133">For example, the declaration of `C` below is an error.</span></span>

```csharp
interface IOuter<out T>
{
    class C { } // error: class declaration within the scope of variant type parameter 'T'
}
```

### <a name="concrete-methods-in-interfaces"></a><span data-ttu-id="0b736-134">接口中的具体方法</span><span class="sxs-lookup"><span data-stu-id="0b736-134">Concrete methods in interfaces</span></span>

<span data-ttu-id="0b736-135">此功能的最简单形式是在接口中声明*具体方法*，该方法是具有主体的方法。</span><span class="sxs-lookup"><span data-stu-id="0b736-135">The simplest form of this feature is the ability to declare a *concrete method* in an interface, which is a method with a body.</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
```

<span data-ttu-id="0b736-136">实现此接口的类不需要实现其具体方法。</span><span class="sxs-lookup"><span data-stu-id="0b736-136">A class that implements this interface need not implement its concrete method.</span></span>

```csharp
class C : IA { } // OK

IA i = new C();
i.M(); // prints "IA.M"
```

<span data-ttu-id="0b736-137">类 `C` 中 `IA.M` 的最终替代是 `M` 在 `IA`中声明的具体方法。</span><span class="sxs-lookup"><span data-stu-id="0b736-137">The final override for `IA.M` in class `C` is the concrete method `M` declared in `IA`.</span></span> <span data-ttu-id="0b736-138">请注意，类不会从其接口继承成员;这不会被此功能更改：</span><span class="sxs-lookup"><span data-stu-id="0b736-138">Note that a class does not inherit members from its interfaces; that is not changed by this feature:</span></span>

```csharp
new C().M(); // error: class 'C' does not contain a member 'M'
```

<span data-ttu-id="0b736-139">在接口的实例成员内，`this` 具有封闭接口的类型。</span><span class="sxs-lookup"><span data-stu-id="0b736-139">Within an instance member of an interface, `this` has the type of the enclosing interface.</span></span>

### <a name="modifiers-in-interfaces"></a><span data-ttu-id="0b736-140">接口中的修饰符</span><span class="sxs-lookup"><span data-stu-id="0b736-140">Modifiers in interfaces</span></span>

<span data-ttu-id="0b736-141">接口的语法非常宽松，允许其成员具有修饰符。</span><span class="sxs-lookup"><span data-stu-id="0b736-141">The syntax for an interface is relaxed to permit modifiers on its members.</span></span> <span data-ttu-id="0b736-142">允许以下内容： `private`、`protected`、`internal`、`public`、`virtual`、`abstract`、`sealed`、`static`、`extern`和 `partial`。</span><span class="sxs-lookup"><span data-stu-id="0b736-142">The following are permitted: `private`, `protected`, `internal`, `public`, `virtual`, `abstract`, `sealed`, `static`, `extern`, and `partial`.</span></span>

> <span data-ttu-id="0b736-143">***TODO***：检查其他修饰符是否存在。</span><span class="sxs-lookup"><span data-stu-id="0b736-143">***TODO***: check what other modifiers exist.</span></span>

<span data-ttu-id="0b736-144">除非使用 `sealed` 或 `private` 修饰符，否则，其声明中包含正文的接口成员是 `virtual` 成员。</span><span class="sxs-lookup"><span data-stu-id="0b736-144">An interface member whose declaration includes a body is a `virtual` member unless the `sealed` or `private` modifier is used.</span></span> <span data-ttu-id="0b736-145">`virtual` 修饰符可用于本来将隐式 `virtual`的函数成员。</span><span class="sxs-lookup"><span data-stu-id="0b736-145">The `virtual` modifier may be used on a function member that would otherwise be implicitly `virtual`.</span></span> <span data-ttu-id="0b736-146">同样，尽管 `abstract` 在没有主体的接口成员上是默认值，但也可以显式指定修饰符。</span><span class="sxs-lookup"><span data-stu-id="0b736-146">Similarly, although `abstract` is the default on interface members without bodies, that modifier may be given explicitly.</span></span> <span data-ttu-id="0b736-147">可以使用 `sealed` 关键字声明非虚拟成员。</span><span class="sxs-lookup"><span data-stu-id="0b736-147">A non-virtual member may be declared using the `sealed` keyword.</span></span>

<span data-ttu-id="0b736-148">如果接口的 `private` 或 `sealed` 函数成员没有正文，则是错误的。</span><span class="sxs-lookup"><span data-stu-id="0b736-148">It is an error for a `private` or `sealed` function member of an interface to have no body.</span></span> <span data-ttu-id="0b736-149">`private` 函数成员不能 `sealed`修饰符。</span><span class="sxs-lookup"><span data-stu-id="0b736-149">A `private` function member may not have the modifier `sealed`.</span></span>

<span data-ttu-id="0b736-150">访问修饰符可用于允许的所有种类成员的接口成员。</span><span class="sxs-lookup"><span data-stu-id="0b736-150">Access modifiers may be used on interface members of all kinds of members that are permitted.</span></span> <span data-ttu-id="0b736-151">访问级别 `public` 是默认值，但它可以显式给定。</span><span class="sxs-lookup"><span data-stu-id="0b736-151">The access level `public` is the default but it may be given explicitly.</span></span>

> <span data-ttu-id="0b736-152">***打开问题：*** 需要指定访问修饰符（如 `protected` 和 `internal`）的精确含义，以及哪些声明执行和不重写它们（在派生接口中）或实现它们（在实现接口的类中）。</span><span class="sxs-lookup"><span data-stu-id="0b736-152">***Open Issue:*** We need to specify the precise meaning of the access modifiers such as `protected` and `internal`, and which declarations do and do not override them (in a derived interface) or implement them (in a class that implements the interface).</span></span>

<span data-ttu-id="0b736-153">接口可以声明 `static` 成员，包括嵌套类型、方法、索引器、属性、事件和静态构造函数。</span><span class="sxs-lookup"><span data-stu-id="0b736-153">Interfaces may declare `static` members, including nested types, methods, indexers, properties, events, and static constructors.</span></span> <span data-ttu-id="0b736-154">所有接口成员的默认访问级别为 `public`。</span><span class="sxs-lookup"><span data-stu-id="0b736-154">The default access level for all interface members is `public`.</span></span>

<span data-ttu-id="0b736-155">接口不能声明实例构造函数、析构函数或字段。</span><span class="sxs-lookup"><span data-stu-id="0b736-155">Interfaces may not declare instance constructors, destructors, or fields.</span></span>

> <span data-ttu-id="0b736-156">***已关闭问题：*** 是否允许在接口中声明运算符？</span><span class="sxs-lookup"><span data-stu-id="0b736-156">***Closed Issue:*** Should operator declarations be permitted in an interface?</span></span> <span data-ttu-id="0b736-157">可能不是转换运算符，但其他运算符怎么样呢？</span><span class="sxs-lookup"><span data-stu-id="0b736-157">Probably not conversion operators, but what about others?</span></span> <span data-ttu-id="0b736-158">***决策***：除转换、相等和不等运算符*之外*，允许使用运算符。</span><span class="sxs-lookup"><span data-stu-id="0b736-158">***Decision***: Operators are permitted *except* for conversion, equality, and inequality operators.</span></span>

> <span data-ttu-id="0b736-159">***已关闭问题：*** 对于隐藏基接口中的成员的接口成员声明，是否应允许 `new`？</span><span class="sxs-lookup"><span data-stu-id="0b736-159">***Closed Issue:*** Should `new` be permitted on interface member declarations that hide members from base interfaces?</span></span> <span data-ttu-id="0b736-160">***决策***：是。</span><span class="sxs-lookup"><span data-stu-id="0b736-160">***Decision***: Yes.</span></span>

> <span data-ttu-id="0b736-161">***已关闭问题：*** 目前不允许对某个接口或其成员执行 `partial`。</span><span class="sxs-lookup"><span data-stu-id="0b736-161">***Closed Issue:*** We do not currently permit `partial` on an interface or its members.</span></span> <span data-ttu-id="0b736-162">这需要单独建议。</span><span class="sxs-lookup"><span data-stu-id="0b736-162">That would require a separate proposal.</span></span> <span data-ttu-id="0b736-163">***决策***：是。</span><span class="sxs-lookup"><span data-stu-id="0b736-163">***Decision***: Yes.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>

### <a name="overrides-in-interfaces"></a><span data-ttu-id="0b736-164">接口中的替代</span><span class="sxs-lookup"><span data-stu-id="0b736-164">Overrides in interfaces</span></span>

<span data-ttu-id="0b736-165">重写声明（即包含 `override` 修饰符的声明）允许编程人员在编译器或运行时不会找到一个的接口中提供最具体的虚拟成员实现。</span><span class="sxs-lookup"><span data-stu-id="0b736-165">Override declarations (i.e. those containing the `override` modifier) allow the programmer to provide a most specific implementation of a virtual member in an interface where the compiler or runtime would not otherwise find one.</span></span> <span data-ttu-id="0b736-166">它还允许将抽象成员从超级接口转换为派生接口中的默认成员。</span><span class="sxs-lookup"><span data-stu-id="0b736-166">It also allows turning an abstract member from a super-interface into a default member in a derived interface.</span></span> <span data-ttu-id="0b736-167">允许重写声明通过使用接口名称（在这种情况下不允许使用访问修饰符）来限定声明，从而*显式*重写特定基接口方法。</span><span class="sxs-lookup"><span data-stu-id="0b736-167">An override declaration is permitted to *explicitly* override a particular base interface method by qualifying the declaration with the interface name (no access modifier is permitted in this case).</span></span> <span data-ttu-id="0b736-168">不允许使用隐式重写。</span><span class="sxs-lookup"><span data-stu-id="0b736-168">Implicit overrides are not permitted.</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); } // explicitly named
}
interface IC : IA
{
    override void M() { WriteLine("IC.M"); } // implicitly named
}
```

<span data-ttu-id="0b736-169">接口中的重写声明不能 `sealed`声明。</span><span class="sxs-lookup"><span data-stu-id="0b736-169">Override declarations in interfaces may not be declared `sealed`.</span></span>

<span data-ttu-id="0b736-170">接口中的公共 `virtual` 函数成员可以在派生接口中显式重写（通过使用最初声明方法的接口类型在重写声明中限定名称，并省略访问修饰符）。</span><span class="sxs-lookup"><span data-stu-id="0b736-170">Public `virtual` function members in an interface may be overridden in a derived interface explicitly (by qualifying the name in the override declaration with the interface type that originally declared the method, and omitting an access modifier).</span></span>

<span data-ttu-id="0b736-171">接口中 `virtual` 函数成员只能在派生接口中显式重写（而非隐式），并且未 `public` 的成员只能在类或结构中显式实现（而不是隐式）。</span><span class="sxs-lookup"><span data-stu-id="0b736-171">`virtual` function members in an interface may only be overridden explicitly (not implicitly) in derived interfaces, and members that are not `public` may only be implemented in a class or struct explicitly (not implicitly).</span></span> <span data-ttu-id="0b736-172">在任一情况下，重写或实现的成员都必须可在重写时*访问*。</span><span class="sxs-lookup"><span data-stu-id="0b736-172">In either case, the overridden or implemented member must be *accessible* where it is overridden.</span></span>

### <a name="reabstraction"></a><span data-ttu-id="0b736-173">Reabstraction</span><span class="sxs-lookup"><span data-stu-id="0b736-173">Reabstraction</span></span>

<span data-ttu-id="0b736-174">在接口中声明的虚拟（具体）方法可能会被重写为在派生接口中是抽象的</span><span class="sxs-lookup"><span data-stu-id="0b736-174">A virtual (concrete) method declared in an interface may be overridden to be abstract in a derived interface</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    abstract void IA.M();
}
class C : IB { } // error: class 'C' does not implement 'IA.M'.
```

<span data-ttu-id="0b736-175">`IB.M` （这是接口中的默认值）的声明中不需要 `abstract` 修饰符，但最好是在重写声明中显式操作。</span><span class="sxs-lookup"><span data-stu-id="0b736-175">The `abstract` modifier is not required in the declaration of `IB.M` (that is the default in interfaces), but it is probably good practice to be explicit in an override declaration.</span></span>

<span data-ttu-id="0b736-176">这适用于方法的默认实现不恰当并且应通过实现类提供更合适的实现的派生接口。</span><span class="sxs-lookup"><span data-stu-id="0b736-176">This is useful in derived interfaces where the default implementation of a method is inappropriate and a more appropriate implementation should be provided by implementing classes.</span></span>

> <span data-ttu-id="0b736-177">***打开问题：*** 是否应允许 reabstraction？</span><span class="sxs-lookup"><span data-stu-id="0b736-177">***Open Issue:*** Should reabstraction be permitted?</span></span>

### <a name="the-most-specific-override-rule"></a><span data-ttu-id="0b736-178">最特定的替代规则</span><span class="sxs-lookup"><span data-stu-id="0b736-178">The most specific override rule</span></span>

<span data-ttu-id="0b736-179">我们要求每个接口和类在类型或其直接和间接接口中出现的重写中的每个虚拟成员都有一个*最特定的重写*。</span><span class="sxs-lookup"><span data-stu-id="0b736-179">We require that every interface and class have a *most specific override* for every virtual member among the overrides appearing in the type or its direct and indirect interfaces.</span></span> <span data-ttu-id="0b736-180">*最具体的替代*是比每个其他重写更具体的替代。</span><span class="sxs-lookup"><span data-stu-id="0b736-180">The *most specific override* is a unique override that is more specific than every other override.</span></span> <span data-ttu-id="0b736-181">如果没有替代，则将成员本身视为最特定的重写。</span><span class="sxs-lookup"><span data-stu-id="0b736-181">If there is no override, the member itself is considered the most specific override.</span></span>

<span data-ttu-id="0b736-182">如果在类型 `T1`上声明了 `M1`，则将一个重写 `M1` 视为比另一个重写 `M2`*更为具体*，`M2` 在类型 `T2`上声明，并在</span><span class="sxs-lookup"><span data-stu-id="0b736-182">One override `M1` is considered *more specific* than another override `M2` if `M1` is declared on type `T1`, `M2` is declared on type `T2`, and either</span></span>

1. <span data-ttu-id="0b736-183">`T1` 在其直接或间接接口之间包含 `T2`，或</span><span class="sxs-lookup"><span data-stu-id="0b736-183">`T1` contains `T2` among its direct or indirect interfaces, or</span></span>
2. <span data-ttu-id="0b736-184">`T2` 是接口类型，但 `T1` 不是接口类型。</span><span class="sxs-lookup"><span data-stu-id="0b736-184">`T2` is an interface type but `T1` is not an interface type.</span></span>

<span data-ttu-id="0b736-185">例如：</span><span class="sxs-lookup"><span data-stu-id="0b736-185">For example:</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    void IA.M() { WriteLine("IC.M"); }
}
interface ID : IB, IC { } // error: no most specific override for 'IA.M'
abstract class C : IB, IC { } // error: no most specific override for 'IA.M'
abstract class D : IA, IB, IC // ok
{
    public abstract void M();
}

```

<span data-ttu-id="0b736-186">最具体的替代规则确保程序员在发生冲突的情况下显式解析冲突（即，由菱形继承引起的歧义）。</span><span class="sxs-lookup"><span data-stu-id="0b736-186">The most specific override rule ensures that a conflict (i.e. an ambiguity arising from diamond inheritance) is resolved explicitly by the programmer at the point where the conflict arises.</span></span>

<span data-ttu-id="0b736-187">由于我们在接口中支持显式抽象替代，因此也可以在类中执行此操作</span><span class="sxs-lookup"><span data-stu-id="0b736-187">Because we support explicit abstract overrides in interfaces, we could do so in classes as well</span></span>

```csharp
abstract class E : IA, IB, IC // ok
{
    abstract void IA.M();
}
```

> <span data-ttu-id="0b736-188">***打开问题***：我们是否应在类中支持显式接口抽象替代？</span><span class="sxs-lookup"><span data-stu-id="0b736-188">***Open issue***: should we support explicit interface abstract overrides in classes?</span></span>

<span data-ttu-id="0b736-189">此外，如果在类声明中，某些接口方法的最特定重写是在接口中声明的抽象重写，则是错误的。</span><span class="sxs-lookup"><span data-stu-id="0b736-189">In addition, it is an error if in a class declaration the most specific override of some interface method is an abstract override that was declared in an interface.</span></span> <span data-ttu-id="0b736-190">这是生效首先使用新术语的现有规则。</span><span class="sxs-lookup"><span data-stu-id="0b736-190">This is an existing rule restated using the new terminology.</span></span>

```csharp
interface IF
{
    void M();
}
abstract class F : IF { } // error: 'F' does not implement 'IF.M'
```

<span data-ttu-id="0b736-191">在接口中声明的虚拟属性有可能在一个接口中具有对其 `get` 访问器的最特定的重写，并且在另一个接口中具有对其 `set` 访问器的最特定的重写。</span><span class="sxs-lookup"><span data-stu-id="0b736-191">It is possible for a virtual property declared in an interface to have a most specific override for its `get` accessor in one interface and a most specific override for its `set` accessor in a different interface.</span></span> <span data-ttu-id="0b736-192">这被视为违反*最具体的替代*规则。</span><span class="sxs-lookup"><span data-stu-id="0b736-192">This is considered a violation of the *most specific override* rule.</span></span>

### <a name="static-and-private-methods"></a><span data-ttu-id="0b736-193">`static` 和 `private` 方法</span><span class="sxs-lookup"><span data-stu-id="0b736-193">`static` and `private` methods</span></span>

<span data-ttu-id="0b736-194">由于接口现在可以包含可执行代码，因此将常见代码抽象到私有和静态方法非常有用。</span><span class="sxs-lookup"><span data-stu-id="0b736-194">Because interfaces may now contain executable code, it is useful to abstract common code into private and static methods.</span></span> <span data-ttu-id="0b736-195">现在，我们在接口中允许这些。</span><span class="sxs-lookup"><span data-stu-id="0b736-195">We now permit these in interfaces.</span></span>

> <span data-ttu-id="0b736-196">***已关闭问题***：是否应支持私有方法？</span><span class="sxs-lookup"><span data-stu-id="0b736-196">***Closed issue***: Should we support private methods?</span></span> <span data-ttu-id="0b736-197">是否应支持静态方法？</span><span class="sxs-lookup"><span data-stu-id="0b736-197">Should we support static methods?</span></span> <span data-ttu-id="0b736-198">**决策：是**</span><span class="sxs-lookup"><span data-stu-id="0b736-198">**Decision: YES**</span></span>

> <span data-ttu-id="0b736-199">***打开问题***：是否应允许接口方法 `protected` 或 `internal` 或其他访问权限？</span><span class="sxs-lookup"><span data-stu-id="0b736-199">***Open issue***: should we permit interface methods to be `protected` or `internal` or other access?</span></span> <span data-ttu-id="0b736-200">如果是，语义是什么？</span><span class="sxs-lookup"><span data-stu-id="0b736-200">If so, what are the semantics?</span></span> <span data-ttu-id="0b736-201">它们是否默认 `virtual`？</span><span class="sxs-lookup"><span data-stu-id="0b736-201">Are they `virtual` by default?</span></span> <span data-ttu-id="0b736-202">如果是，有没有办法使它们成为非虚拟的？</span><span class="sxs-lookup"><span data-stu-id="0b736-202">If so, is there a way to make them non-virtual?</span></span>

> <span data-ttu-id="0b736-203">***打开问题***：如果我们支持静态方法，是否应支持（静态）运算符？</span><span class="sxs-lookup"><span data-stu-id="0b736-203">***Open issue***: If we support static methods, should we support (static) operators?</span></span>

### <a name="base-interface-invocations"></a><span data-ttu-id="0b736-204">基接口调用</span><span class="sxs-lookup"><span data-stu-id="0b736-204">Base interface invocations</span></span>

<span data-ttu-id="0b736-205">使用默认方法从接口派生的类型中的代码可以显式调用该接口的 "base" 实现。</span><span class="sxs-lookup"><span data-stu-id="0b736-205">Code in a type that derives from an interface with a default method can explicitly invoke that interface's "base" implementation.</span></span>

```csharp
interface I0
{
   void M() { Console.WriteLine("I0"); }
}
interface I1 : I0
{
   override void M() { Console.WriteLine("I1"); }
}
interface I2 : I0
{
   override void M() { Console.WriteLine("I2"); }
}
interface I3 : I1, I2
{
   // an explicit override that invoke's a base interface's default method
   void I0.M() { I2.base.M(); }
}

```

<span data-ttu-id="0b736-206">实例（非静态）方法允许通过使用语法 `base(Type).M`来命名直接基接口 nonvirtually 中可访问实例方法的实现。</span><span class="sxs-lookup"><span data-stu-id="0b736-206">An instance (nonstatic) method is permitted to invoke the implementation of an accessible instance method in a direct base interface nonvirtually by naming it using the syntax `base(Type).M`.</span></span> <span data-ttu-id="0b736-207">此方法在以下情况下很有用：通过委托给某个特定基实现来解析由于菱形继承而需要提供的重写。</span><span class="sxs-lookup"><span data-stu-id="0b736-207">This is useful when an override that is required to be provided due to diamond inheritance is resolved by delegating to one particular base implementation.</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    override void IA.M() { WriteLine("IC.M"); }
}

class D : IA, IB, IC
{
    void IA.M() { base(IB).M(); }
}
```

<span data-ttu-id="0b736-208">当使用语法 `base(Type).M`访问 `virtual` 或 `abstract` 成员时，`Type` 包含 `M`的唯一*特定的重写*。</span><span class="sxs-lookup"><span data-stu-id="0b736-208">When a `virtual` or `abstract` member is accessed using the syntax `base(Type).M`, it is required that `Type` contains a unique *most specific override* for `M`.</span></span>

### <a name="binding-base-clauses"></a><span data-ttu-id="0b736-209">绑定基子句</span><span class="sxs-lookup"><span data-stu-id="0b736-209">Binding base clauses</span></span>

<span data-ttu-id="0b736-210">接口现在包含类型。</span><span class="sxs-lookup"><span data-stu-id="0b736-210">Interfaces now contain types.</span></span>  <span data-ttu-id="0b736-211">这些类型可以在基子句中用作基接口。</span><span class="sxs-lookup"><span data-stu-id="0b736-211">These types may be used in the base clause as base interfaces.</span></span>  <span data-ttu-id="0b736-212">绑定基子句时，可能需要知道要绑定这些类型的基接口集（例如，在这些类型中查找和解析受保护的访问）。</span><span class="sxs-lookup"><span data-stu-id="0b736-212">When binding a base clause, we may need to know the set of base interfaces to bind those types (e.g. to lookup in them and to resolve protected access).</span></span>  <span data-ttu-id="0b736-213">因此，会循环定义接口的基子句的含义。</span><span class="sxs-lookup"><span data-stu-id="0b736-213">The meaning of an interface's base clause is thus circularly defined.</span></span>  <span data-ttu-id="0b736-214">为了打破循环，我们添加了与类中已有的类似规则相对应的新语言规则。</span><span class="sxs-lookup"><span data-stu-id="0b736-214">To break the cycle, we add a new language rules corresponding to a similar rule already in place for classes.</span></span>

<span data-ttu-id="0b736-215">在确定接口的*interface_base*含义时，将暂时假设基接口为空。</span><span class="sxs-lookup"><span data-stu-id="0b736-215">While determining the meaning of the *interface_base* of an interface, the base interfaces are temporarily assumed to be empty.</span></span> <span data-ttu-id="0b736-216">这可以确保基本子句的含义无法以递归方式依赖于自身。</span><span class="sxs-lookup"><span data-stu-id="0b736-216">Intuitively this ensures that the meaning of a base clause cannot recursively depend on itself.</span></span> 

<span data-ttu-id="0b736-217">**我们使用以下规则：**</span><span class="sxs-lookup"><span data-stu-id="0b736-217">**We used to have the following rules:**</span></span>

<span data-ttu-id="0b736-218">"当类 B 从类 A 派生时，它是依赖于 B 的编译时错误。类**直接依赖于**其直接基类（如果有），并且**直接依赖于**直接嵌套它的~~**类**~~（如果有）。</span><span class="sxs-lookup"><span data-stu-id="0b736-218">"When a class B derives from a class A, it is a compile-time error for A to depend on B. A class **directly depends on** its direct base class (if any) and **directly depends on** the ~~**class**~~ within which it is immediately nested (if any).</span></span> <span data-ttu-id="0b736-219">根据此定义，类所依赖的完整~~**类**~~集是**直接依赖**关系的反身和可传递闭包。</span><span class="sxs-lookup"><span data-stu-id="0b736-219">Given this definition, the complete set of ~~**classes**~~ upon which a class depends is the reflexive and transitive closure of the **directly depends on** relationship."</span></span>

<span data-ttu-id="0b736-220">接口直接或间接从自身继承时，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="0b736-220">It is a compile-time error for an interface to directly or indirectly inherit from itself.</span></span>
<span data-ttu-id="0b736-221">接口的**基接口**是显式基接口及其基接口。</span><span class="sxs-lookup"><span data-stu-id="0b736-221">The **base interfaces** of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="0b736-222">换言之，基接口集是显式基接口的完全可传递的闭包、其显式基接口等。</span><span class="sxs-lookup"><span data-stu-id="0b736-222">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span>

<span data-ttu-id="0b736-223">**我们将调整它们，如下所示：**</span><span class="sxs-lookup"><span data-stu-id="0b736-223">**We are adjusting them as follows:**</span></span>

<span data-ttu-id="0b736-224">当类 B 从类 A 派生时，它是依赖于 B 的编译时错误。类**直接依赖于**其直接基类（如果有），并且**直接依赖于**直接嵌套它的 _**类型**_ （如果有）。</span><span class="sxs-lookup"><span data-stu-id="0b736-224">When a class B derives from a class A, it is a compile-time error for A to depend on B. A class **directly depends on** its direct base class (if any) and **directly depends on** the _**type**_ within which it is immediately nested (if any).</span></span>

<span data-ttu-id="0b736-225">接口 IB 扩展接口 IA 时，IA 依赖于 IB 会出现编译时错误。</span><span class="sxs-lookup"><span data-stu-id="0b736-225">When an interface IB extends an interface IA, it is a compile-time error for IA to depend on IB.</span></span> <span data-ttu-id="0b736-226">接口**直接依赖于**其直接基接口（如果有），并且**直接依赖于**直接嵌套它的类型（如果有）。</span><span class="sxs-lookup"><span data-stu-id="0b736-226">An interface **directly depends on** its direct base interfaces (if any) and **directly depends on** the type within which it is immediately nested (if any).</span></span>

<span data-ttu-id="0b736-227">考虑到这些定义，类型所依赖的完整**类型**集是**直接依赖**关系的反身和可传递闭包。</span><span class="sxs-lookup"><span data-stu-id="0b736-227">Given these definitions, the complete set of **types** upon which a type depends is the reflexive and transitive closure of the **directly depends on** relationship.</span></span>

### <a name="effect-on-existing-programs"></a><span data-ttu-id="0b736-228">对现有程序的影响</span><span class="sxs-lookup"><span data-stu-id="0b736-228">Effect on existing programs</span></span>

<span data-ttu-id="0b736-229">此处介绍的规则不会影响现有程序的含义。</span><span class="sxs-lookup"><span data-stu-id="0b736-229">The rules presented here are intended to have no effect on the meaning of existing programs.</span></span>

<span data-ttu-id="0b736-230">示例 1：</span><span class="sxs-lookup"><span data-stu-id="0b736-230">Example 1:</span></span>

```csharp
interface IA
{
    void M();
}
class C: IA // Error: IA.M has no concrete most specific override in C
{
    public static void M() { } // method unrelated to 'IA.M' because static
}
```

<span data-ttu-id="0b736-231">示例 2：</span><span class="sxs-lookup"><span data-stu-id="0b736-231">Example 2:</span></span>

```csharp
interface IA
{
    void M();
}
class Base: IA
{
    void IA.M() { }
}
class Derived: Base, IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

<span data-ttu-id="0b736-232">对于涉及默认接口方法的类似情况，相同的规则将产生类似的结果：</span><span class="sxs-lookup"><span data-stu-id="0b736-232">The same rules give similar results to the analogous situation involving default interface methods:</span></span>

```csharp
interface IA
{
    void M() { }
}
class Derived: IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

> <span data-ttu-id="0b736-233">***已关闭问题***：确认这是规范的预期结果。</span><span class="sxs-lookup"><span data-stu-id="0b736-233">***Closed issue***: confirm that this is an intended consequence of the specification.</span></span> <span data-ttu-id="0b736-234">**决策：是**</span><span class="sxs-lookup"><span data-stu-id="0b736-234">**Decision: YES**</span></span>

### <a name="runtime-method-resolution"></a><span data-ttu-id="0b736-235">运行时方法解析</span><span class="sxs-lookup"><span data-stu-id="0b736-235">Runtime method resolution</span></span>

> <span data-ttu-id="0b736-236">***已关闭问题：*** 规范应描述接口默认方法的运行时方法解析算法。</span><span class="sxs-lookup"><span data-stu-id="0b736-236">***Closed Issue:*** The spec should describe the runtime method resolution algorithm in the face of interface default methods.</span></span> <span data-ttu-id="0b736-237">我们需要确保语义与语言语义一致，例如，已声明的方法的执行操作，并且不会重写或实现 `internal` 方法。</span><span class="sxs-lookup"><span data-stu-id="0b736-237">We need to ensure that the semantics are consistent with the language semantics, e.g. which declared methods do and do not override or implement an `internal` method.</span></span>

### <a name="clr-support-api"></a><span data-ttu-id="0b736-238">CLR 支持 API</span><span class="sxs-lookup"><span data-stu-id="0b736-238">CLR support API</span></span>

<span data-ttu-id="0b736-239">为了使编译器能够在为支持此功能的运行时进行检测，将修改此类运行时的库，以便通过 <https://github.com/dotnet/corefx/issues/17116>中所述的 API 播发该事实。</span><span class="sxs-lookup"><span data-stu-id="0b736-239">In order for compilers to detect when they are compiling for a runtime that supports this feature, libraries for such runtimes are modified to advertise that fact through the API discussed in <https://github.com/dotnet/corefx/issues/17116>.</span></span> <span data-ttu-id="0b736-240">我们添加</span><span class="sxs-lookup"><span data-stu-id="0b736-240">We add</span></span>

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeFeature
    {
        // Presence of the field indicates runtime support
        public const string DefaultInterfaceImplementation = nameof(DefaultInterfaceImplementation);
    }
}
```

> <span data-ttu-id="0b736-241">***打开问题***： *CLR*功能的最佳名称是吗？</span><span class="sxs-lookup"><span data-stu-id="0b736-241">***Open issue***: Is that the best name for the *CLR* feature?</span></span> <span data-ttu-id="0b736-242">CLR 功能不仅仅是这样（例如放宽保护约束、支持接口中的重写等）。</span><span class="sxs-lookup"><span data-stu-id="0b736-242">The CLR feature does much more than just that (e.g. relaxes protection constraints, supports overrides in interfaces, etc).</span></span> <span data-ttu-id="0b736-243">它可能称为 "接口中的具体方法" 或 "特征" 之类的内容？</span><span class="sxs-lookup"><span data-stu-id="0b736-243">Perhaps it should be called something like "concrete methods in interfaces", or "traits"?</span></span>

### <a name="further-areas-to-be-specified"></a><span data-ttu-id="0b736-244">要指定的其他区域</span><span class="sxs-lookup"><span data-stu-id="0b736-244">Further areas to be specified</span></span>

- <span data-ttu-id="0b736-245">[] 通过向现有接口添加默认的接口方法和替代，对导致源和二进制兼容性的各种影响进行编目是非常有用的。</span><span class="sxs-lookup"><span data-stu-id="0b736-245">[ ] It would be useful to catalog the kinds of source and binary compatibility effects caused by adding default interface methods and overrides to existing interfaces.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="0b736-246">缺点</span><span class="sxs-lookup"><span data-stu-id="0b736-246">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="0b736-247">此建议需要对 CLR 规范进行协调更新（以支持接口和方法解析中的具体方法）。</span><span class="sxs-lookup"><span data-stu-id="0b736-247">This proposal requires a coordinated update to the CLR specification (to support concrete methods in interfaces and method resolution).</span></span> <span data-ttu-id="0b736-248">因此，这种情况相当 "昂贵"，可能需要将其与其他可能需要 CLR 更改的功能结合在一起。</span><span class="sxs-lookup"><span data-stu-id="0b736-248">It is therefore fairly "expensive" and it may be worth doing in combination with other features that we also anticipate would require CLR changes.</span></span>

## <a name="alternatives"></a><span data-ttu-id="0b736-249">备选项</span><span class="sxs-lookup"><span data-stu-id="0b736-249">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="0b736-250">无。</span><span class="sxs-lookup"><span data-stu-id="0b736-250">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="0b736-251">未解决的问题</span><span class="sxs-lookup"><span data-stu-id="0b736-251">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="0b736-252">在上面的方案中，我们提出了开放式问题。</span><span class="sxs-lookup"><span data-stu-id="0b736-252">Open questions are called out throughout the proposal, above.</span></span>
- <span data-ttu-id="0b736-253">有关打开的问题的列表，另请参阅 <https://github.com/dotnet/csharplang/issues/406>。</span><span class="sxs-lookup"><span data-stu-id="0b736-253">See also <https://github.com/dotnet/csharplang/issues/406> for a list of open questions.</span></span>
- <span data-ttu-id="0b736-254">详细规范必须描述运行时用于选择要调用的精确方法的解析机制。</span><span class="sxs-lookup"><span data-stu-id="0b736-254">The detailed specification must describe the resolution mechanism used at runtime to select the precise method to be invoked.</span></span>
- <span data-ttu-id="0b736-255">新编译器所生成的元数据与旧编译器使用的元数据交互需要进行详细处理。</span><span class="sxs-lookup"><span data-stu-id="0b736-255">The interaction of metadata produced by new compilers and consumed by older compilers needs to be worked out in detail.</span></span> <span data-ttu-id="0b736-256">例如，我们需要确保使用的元数据表示形式不会导致接口中添加默认实现，以在由较旧的编译器编译时破坏实现该接口的现有类。</span><span class="sxs-lookup"><span data-stu-id="0b736-256">For example, we need to ensure that the metadata representation that we use does not cause the addition of a default implementation in an interface to break an existing class that implements that interface when compiled by an older compiler.</span></span> <span data-ttu-id="0b736-257">这可能会影响可使用的元数据表示形式。</span><span class="sxs-lookup"><span data-stu-id="0b736-257">This may affect the metadata representation that we can use.</span></span>
- <span data-ttu-id="0b736-258">设计必须考虑与其他语言的互操作性和其他语言的现有编译器。</span><span class="sxs-lookup"><span data-stu-id="0b736-258">The design must consider interoperation with other languages and existing compilers for other languages.</span></span>

## <a name="resolved-questions"></a><span data-ttu-id="0b736-259">解决的问题</span><span class="sxs-lookup"><span data-stu-id="0b736-259">Resolved Questions</span></span>

### <a name="abstract-override"></a><span data-ttu-id="0b736-260">Abstract 重写</span><span class="sxs-lookup"><span data-stu-id="0b736-260">Abstract Override</span></span>

<span data-ttu-id="0b736-261">早期草案规范包含了 "reabstract" 继承方法的能力：</span><span class="sxs-lookup"><span data-stu-id="0b736-261">The earlier draft spec contained the ability to "reabstract" an inherited method:</span></span>

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { }
}
interface IC : IB
{
    override void M(); // make it abstract again
}
```

<span data-ttu-id="0b736-262">我的说明2017-03-20 显示我们决定不允许这样做。</span><span class="sxs-lookup"><span data-stu-id="0b736-262">My notes for 2017-03-20 showed that we decided not to allow this.</span></span> <span data-ttu-id="0b736-263">但是，其中至少有两个用例：</span><span class="sxs-lookup"><span data-stu-id="0b736-263">However, there are at least two use cases for it:</span></span>

1. <span data-ttu-id="0b736-264">Java Api，此功能的某些用户希望进行互操作，这取决于此功能。</span><span class="sxs-lookup"><span data-stu-id="0b736-264">The Java APIs, with which some users of this feature hope to interoperate, depend on this facility.</span></span>
2. <span data-ttu-id="0b736-265">利用这*一优势来*编程。</span><span class="sxs-lookup"><span data-stu-id="0b736-265">Programming with *traits* benefits from this.</span></span> <span data-ttu-id="0b736-266">Reabstraction 是 "特征" 语言功能（ https://en.wikipedia.org/wiki/Trait_(computer_programming))的元素之一。</span><span class="sxs-lookup"><span data-stu-id="0b736-266">Reabstraction is one of the elements of the "traits" language feature (https://en.wikipedia.org/wiki/Trait_(computer_programming)).</span></span> <span data-ttu-id="0b736-267">以下类允许使用类：</span><span class="sxs-lookup"><span data-stu-id="0b736-267">The following is permitted with classes:</span></span>

```csharp
public abstract class Base
{
    public abstract void M();
}
public abstract class A : Base
{
    public override void M() { }
}
public abstract class B : A
{
    public override abstract void M(); // reabstract Base.M
}
```

<span data-ttu-id="0b736-268">遗憾的是，除非允许这样做，否则不能将此代码重构为一组接口（特征）。</span><span class="sxs-lookup"><span data-stu-id="0b736-268">Unfortunately this code cannot be refactored as a set of interfaces (traits) unless this is permitted.</span></span> <span data-ttu-id="0b736-269">贪婪的*Jared 原则*应允许这样做。</span><span class="sxs-lookup"><span data-stu-id="0b736-269">By the *Jared principle of greed*, it should be permitted.</span></span>

> <span data-ttu-id="0b736-270">***已关闭问题：*** 是否应允许 reabstraction？</span><span class="sxs-lookup"><span data-stu-id="0b736-270">***Closed issue:*** Should reabstraction be permitted?</span></span> <span data-ttu-id="0b736-271">[我的笔记出错了。</span><span class="sxs-lookup"><span data-stu-id="0b736-271">[YES] My notes were wrong.</span></span> <span data-ttu-id="0b736-272">[LDM 说明](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)指出接口中允许 reabstraction。</span><span class="sxs-lookup"><span data-stu-id="0b736-272">The [LDM notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) say that reabstraction is permitted in an interface.</span></span> <span data-ttu-id="0b736-273">不在类中。</span><span class="sxs-lookup"><span data-stu-id="0b736-273">Not in a class.</span></span>

### <a name="virtual-modifier-vs-sealed-modifier"></a><span data-ttu-id="0b736-274">虚拟修饰符与密封修饰符</span><span class="sxs-lookup"><span data-stu-id="0b736-274">Virtual Modifier vs Sealed Modifier</span></span>

<span data-ttu-id="0b736-275">From [Aleksey Tsingauz](https://github.com/AlekseyTs)：</span><span class="sxs-lookup"><span data-stu-id="0b736-275">From [Aleksey Tsingauz](https://github.com/AlekseyTs):</span></span>

> <span data-ttu-id="0b736-276">我们决定允许在接口成员上显式声明修饰符，除非有理由禁止这样做。</span><span class="sxs-lookup"><span data-stu-id="0b736-276">We decided to allow modifiers explicitly stated on interface members, unless there is a reason to disallow some of them.</span></span> <span data-ttu-id="0b736-277">这就为虚拟修饰符带来了一个有趣的问题。</span><span class="sxs-lookup"><span data-stu-id="0b736-277">This brings an interesting question around virtual modifier.</span></span> <span data-ttu-id="0b736-278">是否应在具有默认实现的成员上需要？</span><span class="sxs-lookup"><span data-stu-id="0b736-278">Should it be required on members with default implementation?</span></span>
>
> <span data-ttu-id="0b736-279">我们可能会说：</span><span class="sxs-lookup"><span data-stu-id="0b736-279">We could say that:</span></span>
>
> - <span data-ttu-id="0b736-280">如果没有任何实现，也没有指定虚拟和密封，则假定该成员是抽象成员。</span><span class="sxs-lookup"><span data-stu-id="0b736-280">if there is no implementation and neither virtual, nor sealed are specified, we assume the member is abstract.</span></span>
> - <span data-ttu-id="0b736-281">如果有一个实现并且未指定任何抽象和密封，则假定该成员是虚拟的。</span><span class="sxs-lookup"><span data-stu-id="0b736-281">if there is an implementation and neither abstract, nor sealed are specified, we assume the member is virtual.</span></span>
> - <span data-ttu-id="0b736-282">要使方法既不是虚拟的也不是抽象的，必须使用 sealed 修饰符。</span><span class="sxs-lookup"><span data-stu-id="0b736-282">sealed modifier is required to make a method neither virtual, nor abstract.</span></span>
>
> <span data-ttu-id="0b736-283">另外，我们也可能会说虚拟修饰符对于虚拟成员是必需的。</span><span class="sxs-lookup"><span data-stu-id="0b736-283">Alternatively, we could say that virtual modifier is required for a virtual member.</span></span> <span data-ttu-id="0b736-284">例如，如果存在未使用虚拟修饰符显式标记的实现的成员，则它既不是虚拟的也不是抽象的。</span><span class="sxs-lookup"><span data-stu-id="0b736-284">I.e, if there is a member with implementation not explicitly marked with virtual modifier, it is neither virtual, nor abstract.</span></span> <span data-ttu-id="0b736-285">当方法从类移到接口时，此方法可能会提供更好的体验：</span><span class="sxs-lookup"><span data-stu-id="0b736-285">This approach might provide better experience when a method is moved from a class to an interface:</span></span>
>
> - <span data-ttu-id="0b736-286">抽象方法保持抽象。</span><span class="sxs-lookup"><span data-stu-id="0b736-286">an abstract method stays abstract.</span></span>
> - <span data-ttu-id="0b736-287">虚方法保持为虚方法。</span><span class="sxs-lookup"><span data-stu-id="0b736-287">a virtual method stays virtual.</span></span>
> - <span data-ttu-id="0b736-288">没有任何修饰符的方法将保持不变，也不会成为抽象方法。</span><span class="sxs-lookup"><span data-stu-id="0b736-288">a method without any modifier stays neither virtual, nor abstract.</span></span>
> - <span data-ttu-id="0b736-289">密封修饰符不能应用于不是重写的方法。</span><span class="sxs-lookup"><span data-stu-id="0b736-289">sealed modifier cannot be applied to a method that is not an override.</span></span>
>
> <span data-ttu-id="0b736-290">你觉得怎么样？</span><span class="sxs-lookup"><span data-stu-id="0b736-290">What do you think?</span></span>

> <span data-ttu-id="0b736-291">***已关闭问题：*** 是否应隐式地 `virtual`具体的方法（具有实现）？</span><span class="sxs-lookup"><span data-stu-id="0b736-291">***Closed Issue:*** Should a concrete method (with implementation) be implicitly `virtual`?</span></span> <span data-ttu-id="0b736-292">[</span><span class="sxs-lookup"><span data-stu-id="0b736-292">[YES]</span></span>

<span data-ttu-id="0b736-293">***决策：*** 在 LDM 2017-04-05 中创建：</span><span class="sxs-lookup"><span data-stu-id="0b736-293">***Decisions:*** Made in the LDM 2017-04-05:</span></span>

1. <span data-ttu-id="0b736-294">非虚拟应通过 `sealed` 或 `private`显式表达。</span><span class="sxs-lookup"><span data-stu-id="0b736-294">non-virtual should be explicitly expressed through `sealed` or `private`.</span></span>
2. <span data-ttu-id="0b736-295">`sealed` 是使接口实例成员具有非虚拟主体的关键字</span><span class="sxs-lookup"><span data-stu-id="0b736-295">`sealed` is the keyword to make interface instance members with bodies non-virtual</span></span>
3. <span data-ttu-id="0b736-296">我们希望允许接口中的所有修饰符</span><span class="sxs-lookup"><span data-stu-id="0b736-296">We want to allow all modifiers in interfaces</span></span>  
4. <span data-ttu-id="0b736-297">接口成员的默认可访问性是公共的，其中包括嵌套类型</span><span class="sxs-lookup"><span data-stu-id="0b736-297">Default accessibility for interface members is public, including nested types</span></span>
5. <span data-ttu-id="0b736-298">接口中的私有函数成员是隐式密封的，不允许在这些成员上使用 `sealed`。</span><span class="sxs-lookup"><span data-stu-id="0b736-298">private function members in interfaces are implicitly sealed, and `sealed` is not permitted on them.</span></span>
6. <span data-ttu-id="0b736-299">允许使用私有类（接口中的），可以将其密封，这意味着密封在密封类的类中。</span><span class="sxs-lookup"><span data-stu-id="0b736-299">Private classes (in interfaces) are permitted and can be sealed, and that means sealed in the class sense of sealed.</span></span>
7. <span data-ttu-id="0b736-300">缺少良好的建议，不允许在接口或其成员上使用 partial。</span><span class="sxs-lookup"><span data-stu-id="0b736-300">Absent a good proposal, partial is still not allowed on interfaces or their members.</span></span>

### <a name="binary-compatibility-1"></a><span data-ttu-id="0b736-301">二进制兼容性1</span><span class="sxs-lookup"><span data-stu-id="0b736-301">Binary Compatibility 1</span></span>

<span data-ttu-id="0b736-302">当库提供默认实现时</span><span class="sxs-lookup"><span data-stu-id="0b736-302">When a library provides a default implementation</span></span>

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
}
class C : I2
{
}
```

<span data-ttu-id="0b736-303">我们了解到 `C` 中的 `I1.M` 实现是 `I1.M`的。</span><span class="sxs-lookup"><span data-stu-id="0b736-303">We understand that the implementation of `I1.M` in `C` is `I1.M`.</span></span> <span data-ttu-id="0b736-304">如果包含 `I2` 的程序集按如下方式更改并重新编译，会发生什么情况呢？</span><span class="sxs-lookup"><span data-stu-id="0b736-304">What if the assembly containing `I2` is changed as follows and recompiled</span></span>

```csharp
interface I2 : I1
{
    override void M() { Impl2 }
}
```

<span data-ttu-id="0b736-305">但不会重新编译 `C`。</span><span class="sxs-lookup"><span data-stu-id="0b736-305">but `C` is not recompiled.</span></span> <span data-ttu-id="0b736-306">程序运行时会发生什么情况？</span><span class="sxs-lookup"><span data-stu-id="0b736-306">What happens when the program is run?</span></span> <span data-ttu-id="0b736-307">调用 `(C as I1).M()`</span><span class="sxs-lookup"><span data-stu-id="0b736-307">An invocation of `(C as I1).M()`</span></span>

1. <span data-ttu-id="0b736-308">运行 `I1.M`</span><span class="sxs-lookup"><span data-stu-id="0b736-308">Runs `I1.M`</span></span>
2. <span data-ttu-id="0b736-309">运行 `I2.M`</span><span class="sxs-lookup"><span data-stu-id="0b736-309">Runs `I2.M`</span></span>
3. <span data-ttu-id="0b736-310">引发某种类型的运行时错误</span><span class="sxs-lookup"><span data-stu-id="0b736-310">Throws some kind of runtime error</span></span>

<span data-ttu-id="0b736-311">***决策：*** 进行2017-04-11：运行 `I2.M`，这是运行时明确的最具体替代。</span><span class="sxs-lookup"><span data-stu-id="0b736-311">***Decision:*** Made 2017-04-11: Runs `I2.M`, which is the unambiguously most specific override at runtime.</span></span>

### <a name="event-accessors-closed"></a><span data-ttu-id="0b736-312">事件访问器（关闭）</span><span class="sxs-lookup"><span data-stu-id="0b736-312">Event accessors (closed)</span></span>

> <span data-ttu-id="0b736-313">***已关闭问题：*** 事件是否可以重写 "分段"？</span><span class="sxs-lookup"><span data-stu-id="0b736-313">***Closed Issue:*** Can an event be overridden "piecewise"?</span></span>

<span data-ttu-id="0b736-314">请考虑这种情况：</span><span class="sxs-lookup"><span data-stu-id="0b736-314">Consider this case:</span></span>

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        // error: "remove" accessor missing
    }
}
```

<span data-ttu-id="0b736-315">不允许使用事件的 "部分" 实现，因为在类中，事件声明的语法不允许只有一个访问器;必须同时提供两个（或两个）。</span><span class="sxs-lookup"><span data-stu-id="0b736-315">This "partial" implementation of the event is not permitted because, as in a class, the syntax for an event declaration does not permit only one accessor; both (or neither) must be provided.</span></span> <span data-ttu-id="0b736-316">通过允许语法中的 abstract remove 访问器在缺少正文时隐式抽象，可以实现相同的目的：</span><span class="sxs-lookup"><span data-stu-id="0b736-316">You could accomplish the same thing by permitting the abstract remove accessor in the syntax to be implicitly abstract by the absence of a body:</span></span>

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        remove; // implicitly abstract
    }
}
```

<span data-ttu-id="0b736-317">请注意，*这是一个新的（建议的）语法*。</span><span class="sxs-lookup"><span data-stu-id="0b736-317">Note that *this is a new (proposed) syntax*.</span></span> <span data-ttu-id="0b736-318">在当前语法中，事件访问器具有强制正文。</span><span class="sxs-lookup"><span data-stu-id="0b736-318">In the current grammar, event accessors have a mandatory body.</span></span>

> <span data-ttu-id="0b736-319">***已关闭问题：*** 事件访问器可以通过省略主体来确定（隐式）抽象，类似于通过省略主体来确定接口和属性访问器中的方法（隐式）抽象的方法。</span><span class="sxs-lookup"><span data-stu-id="0b736-319">***Closed Issue:*** Can an event accessor be (implicitly) abstract by the omission of a body, similarly to the way that methods in interfaces and property accessors are (implicitly) abstract by the omission of a body?</span></span>

<span data-ttu-id="0b736-320">***决策：*** （2017-04-18）否，事件声明需要两个具体的访问器（或均不需要）。</span><span class="sxs-lookup"><span data-stu-id="0b736-320">***Decision:*** (2017-04-18) No, event declarations require both concrete accessors (or neither).</span></span>

### <a name="reabstraction-in-a-class-closed"></a><span data-ttu-id="0b736-321">类中的 Reabstraction （已关闭）</span><span class="sxs-lookup"><span data-stu-id="0b736-321">Reabstraction in a Class (closed)</span></span>

<span data-ttu-id="0b736-322">***已关闭问题：*** 我们应确认允许这样做（否则添加默认实现将是一项重大更改）：</span><span class="sxs-lookup"><span data-stu-id="0b736-322">***Closed Issue:*** We should confirm that this is permitted (otherwise adding a default implementation would be a breaking change):</span></span>

```csharp
interface I1
{
    void M() { }
}
abstract class C : I1
{
    public abstract void M(); // implement I1.M with an abstract method in C
}
```

<span data-ttu-id="0b736-323">***决策：*** （2017-04-18）是，将主体添加到接口成员声明不应中断 C。</span><span class="sxs-lookup"><span data-stu-id="0b736-323">***Decision:*** (2017-04-18) Yes, adding a body to an interface member declaration shouldn't break C.</span></span>

### <a name="sealed-override-closed"></a><span data-ttu-id="0b736-324">密封的替代（已关闭）</span><span class="sxs-lookup"><span data-stu-id="0b736-324">Sealed Override (closed)</span></span>

<span data-ttu-id="0b736-325">前面的问题隐式假定 `sealed` 修饰符可应用于接口中的 `override`。</span><span class="sxs-lookup"><span data-stu-id="0b736-325">The previous question implicitly assumes that the `sealed` modifier can be applied to an `override` in an interface.</span></span> <span data-ttu-id="0b736-326">这与草案规范相抵触。</span><span class="sxs-lookup"><span data-stu-id="0b736-326">This contradicts the draft specification.</span></span> <span data-ttu-id="0b736-327">是否允许密封替代？</span><span class="sxs-lookup"><span data-stu-id="0b736-327">Do we want to permit sealing an override?</span></span> <span data-ttu-id="0b736-328">应考虑密封的源和二进制兼容性影响。</span><span class="sxs-lookup"><span data-stu-id="0b736-328">Source and binary compatibility effects of sealing should be considered.</span></span>

> <span data-ttu-id="0b736-329">***已关闭问题：*** 是否应允许密封替代？</span><span class="sxs-lookup"><span data-stu-id="0b736-329">***Closed Issue:*** Should we permit sealing an override?</span></span>

<span data-ttu-id="0b736-330">***决策：*** （2017-04-18）不允许在接口中的替代 `sealed`。</span><span class="sxs-lookup"><span data-stu-id="0b736-330">***Decision:*** (2017-04-18) Let's not allowed `sealed` on overrides in interfaces.</span></span> <span data-ttu-id="0b736-331">对接口成员的 `sealed` 唯一使用是在其初始声明中使其成为非虚拟的。</span><span class="sxs-lookup"><span data-stu-id="0b736-331">The only use of `sealed` on interface members is to make them non-virtual in their initial declaration.</span></span>

### <a name="diamond-inheritance-and-classes-closed"></a><span data-ttu-id="0b736-332">菱形继承和类（闭合）</span><span class="sxs-lookup"><span data-stu-id="0b736-332">Diamond inheritance and classes (closed)</span></span>

<span data-ttu-id="0b736-333">提议的草稿在菱形继承方案中，类优先于接口替代：</span><span class="sxs-lookup"><span data-stu-id="0b736-333">The draft of the proposal prefers class overrides to interface overrides in diamond inheritance scenarios:</span></span>

> <span data-ttu-id="0b736-334">我们要求每个接口和类在类型或其直接和间接接口中出现的重写中都有对每个接口方法的*最特定的重写*。</span><span class="sxs-lookup"><span data-stu-id="0b736-334">We require that every interface and class have a *most specific override* for every interface method among the overrides appearing in the type or its direct and indirect interfaces.</span></span> <span data-ttu-id="0b736-335">*最具体的替代*是比每个其他重写更具体的替代。</span><span class="sxs-lookup"><span data-stu-id="0b736-335">The *most specific override* is a unique override that is more specific than every other override.</span></span> <span data-ttu-id="0b736-336">如果没有替代，则将方法本身视为最特定的重写。</span><span class="sxs-lookup"><span data-stu-id="0b736-336">If there is no override, the method itself is considered the most specific override.</span></span>
>
> <span data-ttu-id="0b736-337">如果在类型 `T1`上声明了 `M1`，则将一个重写 `M1` 视为比另一个重写 `M2`*更为具体*，`M2` 在类型 `T2`上声明，并在</span><span class="sxs-lookup"><span data-stu-id="0b736-337">One override `M1` is considered *more specific* than another override `M2` if `M1` is declared on type `T1`, `M2` is declared on type `T2`, and either</span></span>
>
> 1. <span data-ttu-id="0b736-338">`T1` 在其直接或间接接口之间包含 `T2`，或</span><span class="sxs-lookup"><span data-stu-id="0b736-338">`T1` contains `T2` among its direct or indirect interfaces, or</span></span>
> 2. <span data-ttu-id="0b736-339">`T2` 是接口类型，但 `T1` 不是接口类型。</span><span class="sxs-lookup"><span data-stu-id="0b736-339">`T2` is an interface type but `T1` is not an interface type.</span></span>

<span data-ttu-id="0b736-340">此方案是</span><span class="sxs-lookup"><span data-stu-id="0b736-340">The scenario is this</span></span>

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { WriteLine("IB"); }
}
class Base : IA
{
    void IA.M() { WriteLine("Base"); }
}
class Derived : Base, IB // allowed?
{
    static void Main()
    {
        Ia a = new Derived();
        a.M();           // what does it do?
    }
}
```

<span data-ttu-id="0b736-341">我们应确认此行为（或自行决定）</span><span class="sxs-lookup"><span data-stu-id="0b736-341">We should confirm this behavior (or decide otherwise)</span></span>

> <span data-ttu-id="0b736-342">***已关闭问题：*** 在上面的草案规范适用于混合类和接口（类优先于接口）的情况下，针对*大多数特定的替代*，进行确认。</span><span class="sxs-lookup"><span data-stu-id="0b736-342">***Closed Issue:*** Confirm the draft spec, above, for *most specific override* as it applies to mixed classes and interfaces (a class takes priority over an interface).</span></span> <span data-ttu-id="0b736-343">请参阅 <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>。</span><span class="sxs-lookup"><span data-stu-id="0b736-343">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>.</span></span>

### <a name="interface-methods-vs-structs-closed"></a><span data-ttu-id="0b736-344">接口方法 vs 结构（闭合）</span><span class="sxs-lookup"><span data-stu-id="0b736-344">Interface methods vs structs (closed)</span></span>

<span data-ttu-id="0b736-345">默认的接口方法和结构之间存在一些不幸的交互。</span><span class="sxs-lookup"><span data-stu-id="0b736-345">There are some unfortunate interactions between default interface methods and structs.</span></span>

```csharp
interface IA
{
    public void M() { }
}
struct S : IA
{
}
```

<span data-ttu-id="0b736-346">请注意，不会继承接口成员：</span><span class="sxs-lookup"><span data-stu-id="0b736-346">Note that interface members are not inherited:</span></span>

```csharp
var s = default(S);
s.M(); // error: 'S' does not contain a member 'M'
```

<span data-ttu-id="0b736-347">因此，客户端必须对该结构进行方框才能调用接口方法</span><span class="sxs-lookup"><span data-stu-id="0b736-347">Consequently, the client must box the struct to invoke interface methods</span></span>

```csharp
IA s = default(S); // an S, boxed
s.M(); // ok
```

<span data-ttu-id="0b736-348">采用这种方式进行装箱将违背 `struct` 类型的主要优点。</span><span class="sxs-lookup"><span data-stu-id="0b736-348">Boxing in this way defeats the principal benefits of a `struct` type.</span></span> <span data-ttu-id="0b736-349">而且，任何变化方法都不会有明显的影响，因为它们在结构的已*装箱副本*上操作：</span><span class="sxs-lookup"><span data-stu-id="0b736-349">Moreover, any mutation methods will have no apparent effect, because they are operating on a *boxed copy* of the struct:</span></span>

```csharp
interface IB
{
    public void Increment() { P += 1; }
    public int P { get; set; }
}
struct T : IB
{
    public int P { get; set; } // auto-property
}

T t = default(T);
Console.WriteLine(t.P); // prints 0
(t as IB).Increment();
Console.WriteLine(t.P); // prints 0
```

> <span data-ttu-id="0b736-350">***已关闭问题：*** 我们可以对此做些什么：</span><span class="sxs-lookup"><span data-stu-id="0b736-350">***Closed Issue:*** What can we do about this:</span></span>
>
> 1. <span data-ttu-id="0b736-351">禁止 `struct` 继承默认实现。</span><span class="sxs-lookup"><span data-stu-id="0b736-351">Forbid a `struct` from inheriting a default implementation.</span></span> <span data-ttu-id="0b736-352">在 `struct`中，所有接口方法均被视为抽象方法。</span><span class="sxs-lookup"><span data-stu-id="0b736-352">All interface methods would be treated as abstract in a `struct`.</span></span> <span data-ttu-id="0b736-353">稍后，我们可能需要花费一段时间来决定如何使其更好地工作。</span><span class="sxs-lookup"><span data-stu-id="0b736-353">Then we may take time later to decide how to make it work better.</span></span>
> 2. <span data-ttu-id="0b736-354">提供了一种可避免装箱的代码生成策略。</span><span class="sxs-lookup"><span data-stu-id="0b736-354">Come up with some kind of code generation strategy that avoids boxing.</span></span> <span data-ttu-id="0b736-355">在 `IB.Increment`之类的方法中，`this` 的类型可能类似于类型参数，该类型参数被约束为 `IB`。</span><span class="sxs-lookup"><span data-stu-id="0b736-355">Inside a method like `IB.Increment`, the type of `this` would perhaps be akin to a type parameter constrained to `IB`.</span></span> <span data-ttu-id="0b736-356">与此相结合，若要避免调用方中的装箱，将从接口继承非抽象方法。</span><span class="sxs-lookup"><span data-stu-id="0b736-356">In conjunction with that, to avoid boxing in the caller, non-abstract methods would be inherited from interfaces.</span></span> <span data-ttu-id="0b736-357">这会显著提高编译器和 CLR 实现的工作效果。</span><span class="sxs-lookup"><span data-stu-id="0b736-357">This may increase compiler and CLR implementation work substantially.</span></span>
> 3. <span data-ttu-id="0b736-358">不用担心，只是将其保留为 wart。</span><span class="sxs-lookup"><span data-stu-id="0b736-358">Not worry about it and just leave it as a wart.</span></span>
> 4. <span data-ttu-id="0b736-359">其他想法？</span><span class="sxs-lookup"><span data-stu-id="0b736-359">Other ideas?</span></span>

<span data-ttu-id="0b736-360">***决策：*** 不用担心，只是将其保留为 wart。</span><span class="sxs-lookup"><span data-stu-id="0b736-360">***Decision:*** Not worry about it and just leave it as a wart.</span></span> <span data-ttu-id="0b736-361">请参阅 <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>。</span><span class="sxs-lookup"><span data-stu-id="0b736-361">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>.</span></span>

### <a name="base-interface-invocations-closed"></a><span data-ttu-id="0b736-362">基接口调用（已关闭）</span><span class="sxs-lookup"><span data-stu-id="0b736-362">Base interface invocations (closed)</span></span>

<span data-ttu-id="0b736-363">草案规范建议使用 Java 的基本接口调用的语法： `Interface.base.M()`。</span><span class="sxs-lookup"><span data-stu-id="0b736-363">The draft spec suggests a syntax for base interface invocations inspired by Java: `Interface.base.M()`.</span></span> <span data-ttu-id="0b736-364">我们需要至少为初始原型选择一种语法。</span><span class="sxs-lookup"><span data-stu-id="0b736-364">We need to select a syntax, at least for the initial prototype.</span></span> <span data-ttu-id="0b736-365">我喜欢 `base<Interface>.M()`。</span><span class="sxs-lookup"><span data-stu-id="0b736-365">My favorite is `base<Interface>.M()`.</span></span>

> <span data-ttu-id="0b736-366">***已关闭问题：*** 基本成员调用的语法是什么？</span><span class="sxs-lookup"><span data-stu-id="0b736-366">***Closed Issue:*** What is the syntax for a base member invocation?</span></span>

<span data-ttu-id="0b736-367">***决策：*** 语法为 `base(Interface).M()`。</span><span class="sxs-lookup"><span data-stu-id="0b736-367">***Decision:*** The syntax is `base(Interface).M()`.</span></span> <span data-ttu-id="0b736-368">请参阅 <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>。</span><span class="sxs-lookup"><span data-stu-id="0b736-368">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>.</span></span> <span data-ttu-id="0b736-369">接口如此命名必须是基接口，但不需要是直接基接口。</span><span class="sxs-lookup"><span data-stu-id="0b736-369">The interface so named must be a base interface, but does not need to be a direct base interface.</span></span>

> <span data-ttu-id="0b736-370">***打开问题：*** 是否应允许在类成员中调用基接口？</span><span class="sxs-lookup"><span data-stu-id="0b736-370">***Open Issue:*** Should base interface invocations be permitted in class members?</span></span>

<span data-ttu-id="0b736-371">***决策***：是。</span><span class="sxs-lookup"><span data-stu-id="0b736-371">***Decision***: Yes.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>

### <a name="overriding-non-public-interface-members-closed"></a><span data-ttu-id="0b736-372">重写非公共接口成员（已关闭）</span><span class="sxs-lookup"><span data-stu-id="0b736-372">Overriding non-public interface members (closed)</span></span>

<span data-ttu-id="0b736-373">在接口中，通过使用 `override` 修饰符重写基接口中的非公共成员。</span><span class="sxs-lookup"><span data-stu-id="0b736-373">In an interface, non-public members from base interfaces are overridden using the `override` modifier.</span></span> <span data-ttu-id="0b736-374">如果它是命名包含成员的接口的 "显式" 重写，则省略访问修饰符。</span><span class="sxs-lookup"><span data-stu-id="0b736-374">If it is an "explicit" override that names the interface containing the member, the access modifier is omitted.</span></span>

> <span data-ttu-id="0b736-375">***已关闭问题：*** 如果它是一个不命名接口的 "隐式" 重写，则访问修饰符是否必须匹配？</span><span class="sxs-lookup"><span data-stu-id="0b736-375">***Closed Issue:*** If it is an "implicit" override that does not name the interface, does the access modifier have to match?</span></span>

<span data-ttu-id="0b736-376">***决策：*** 仅公共成员可以隐式重写，并且访问必须匹配。</span><span class="sxs-lookup"><span data-stu-id="0b736-376">***Decision:*** Only public members may be implicitly overridden, and the access must match.</span></span> <span data-ttu-id="0b736-377">请参阅 <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>。</span><span class="sxs-lookup"><span data-stu-id="0b736-377">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span></span>

> <span data-ttu-id="0b736-378">***打开问题：*** 访问修饰符是必需的还是可选的，或在显式重写（如 `override void IB.M() {}`）上省略。</span><span class="sxs-lookup"><span data-stu-id="0b736-378">***Open Issue:*** Is the access modifier required, optional, or omitted on an explicit override such as `override void IB.M() {}`?</span></span>

> <span data-ttu-id="0b736-379">***打开问题：*** 在显式重写（如 `void IB.M() {}`）上 `override` required、optional 还是省略？</span><span class="sxs-lookup"><span data-stu-id="0b736-379">***Open Issue:*** Is `override` required, optional, or omitted on an explicit override such as `void IB.M() {}`?</span></span>

<span data-ttu-id="0b736-380">如何实现类中的非公共接口成员？</span><span class="sxs-lookup"><span data-stu-id="0b736-380">How does one implement a non-public interface member in a class?</span></span> <span data-ttu-id="0b736-381">这可能是必须显式完成的吗？</span><span class="sxs-lookup"><span data-stu-id="0b736-381">Perhaps it must be done explicitly?</span></span>

```csharp
interface IA
{
    internal void MI();
    protected void MP();
}
class C : IA
{
    // are these implementations?
    internal void MI() {}
    protected void MP() {}
}
```

> <span data-ttu-id="0b736-382">***已关闭问题：*** 如何实现类中的非公共接口成员？</span><span class="sxs-lookup"><span data-stu-id="0b736-382">***Closed Issue:*** How does one implement a non-public interface member in a class?</span></span>

<span data-ttu-id="0b736-383">***决策：*** 您只能显式实现非公共接口成员。</span><span class="sxs-lookup"><span data-stu-id="0b736-383">***Decision:*** You can only implement non-public interface members explicitly.</span></span> <span data-ttu-id="0b736-384">请参阅 <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>。</span><span class="sxs-lookup"><span data-stu-id="0b736-384">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span></span>

<span data-ttu-id="0b736-385">***决策***：不允许在接口成员上使用 `override` 关键字。</span><span class="sxs-lookup"><span data-stu-id="0b736-385">***Decision***: No `override` keyword permitted on interface members.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>

### <a name="binary-compatibility-2-closed"></a><span data-ttu-id="0b736-386">二进制兼容性2（关闭）</span><span class="sxs-lookup"><span data-stu-id="0b736-386">Binary Compatibility 2 (closed)</span></span>

<span data-ttu-id="0b736-387">请考虑以下代码，其中每个类型都在单独的程序集中</span><span class="sxs-lookup"><span data-stu-id="0b736-387">Consider the following code in which each type is in a separate assembly</span></span>

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
    override void M() { Impl2 }
}
interface I3 : I1
{
}
class C : I2, I3
{
}
```

<span data-ttu-id="0b736-388">我们了解到 `C` 中的 `I1.M` 实现是 `I2.M`的。</span><span class="sxs-lookup"><span data-stu-id="0b736-388">We understand that the implementation of `I1.M` in `C` is `I2.M`.</span></span> <span data-ttu-id="0b736-389">如果包含 `I3` 的程序集按如下方式更改并重新编译，会发生什么情况呢？</span><span class="sxs-lookup"><span data-stu-id="0b736-389">What if the assembly containing `I3` is changed as follows and recompiled</span></span>

```csharp
interface I3 : I1
{
    override void M() { Impl3 }
}
```

<span data-ttu-id="0b736-390">但不会重新编译 `C`。</span><span class="sxs-lookup"><span data-stu-id="0b736-390">but `C` is not recompiled.</span></span> <span data-ttu-id="0b736-391">程序运行时会发生什么情况？</span><span class="sxs-lookup"><span data-stu-id="0b736-391">What happens when the program is run?</span></span> <span data-ttu-id="0b736-392">调用 `(C as I1).M()`</span><span class="sxs-lookup"><span data-stu-id="0b736-392">An invocation of `(C as I1).M()`</span></span>

1. <span data-ttu-id="0b736-393">运行 `I1.M`</span><span class="sxs-lookup"><span data-stu-id="0b736-393">Runs `I1.M`</span></span>
2. <span data-ttu-id="0b736-394">运行 `I2.M`</span><span class="sxs-lookup"><span data-stu-id="0b736-394">Runs `I2.M`</span></span>
3. <span data-ttu-id="0b736-395">运行 `I3.M`</span><span class="sxs-lookup"><span data-stu-id="0b736-395">Runs `I3.M`</span></span>
4. <span data-ttu-id="0b736-396">2或3，确定性</span><span class="sxs-lookup"><span data-stu-id="0b736-396">Either 2 or 3, deterministically</span></span>
5. <span data-ttu-id="0b736-397">引发某种类型的运行时异常</span><span class="sxs-lookup"><span data-stu-id="0b736-397">Throws some kind of runtime exception</span></span>

<span data-ttu-id="0b736-398">***决策***：引发异常（5）。</span><span class="sxs-lookup"><span data-stu-id="0b736-398">***Decision***: Throw an exception (5).</span></span> <span data-ttu-id="0b736-399">请参阅 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>。</span><span class="sxs-lookup"><span data-stu-id="0b736-399">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>.</span></span>

### <a name="permit-partial-in-interface-closed"></a><span data-ttu-id="0b736-400">是否允许在接口中 `partial`？</span><span class="sxs-lookup"><span data-stu-id="0b736-400">Permit `partial` in interface?</span></span> <span data-ttu-id="0b736-401">闭</span><span class="sxs-lookup"><span data-stu-id="0b736-401">(closed)</span></span>

<span data-ttu-id="0b736-402">如果接口的使用方式类似于使用抽象类的方式，则将它们声明 `partial`可能会很有用。</span><span class="sxs-lookup"><span data-stu-id="0b736-402">Given that interfaces may be used in ways analogous to the way abstract classes are used, it may be useful to declare them `partial`.</span></span> <span data-ttu-id="0b736-403">这对于生成器而言特别有用。</span><span class="sxs-lookup"><span data-stu-id="0b736-403">This would be particularly useful in the face of generators.</span></span>

> <span data-ttu-id="0b736-404">***建议：*** 删除接口和接口成员不能 `partial`声明的语言限制。</span><span class="sxs-lookup"><span data-stu-id="0b736-404">***Proposal:*** Remove the language restriction that interfaces and members of interfaces may not be declared `partial`.</span></span>

<span data-ttu-id="0b736-405">***决策***：是。</span><span class="sxs-lookup"><span data-stu-id="0b736-405">***Decision***: Yes.</span></span> <span data-ttu-id="0b736-406">请参阅 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>。</span><span class="sxs-lookup"><span data-stu-id="0b736-406">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>.</span></span>

### <a name="main-in-an-interface-closed"></a><span data-ttu-id="0b736-407">在接口中 `Main`？</span><span class="sxs-lookup"><span data-stu-id="0b736-407">`Main` in an interface?</span></span> <span data-ttu-id="0b736-408">闭</span><span class="sxs-lookup"><span data-stu-id="0b736-408">(closed)</span></span>

> <span data-ttu-id="0b736-409">***打开问题：*** 接口中的 `static Main` 方法是程序入口点的候选项吗？</span><span class="sxs-lookup"><span data-stu-id="0b736-409">***Open Issue:*** Is a `static Main` method in an interface a candidate to be the program's entry point?</span></span>

<span data-ttu-id="0b736-410">***决策***：是。</span><span class="sxs-lookup"><span data-stu-id="0b736-410">***Decision***: Yes.</span></span> <span data-ttu-id="0b736-411">请参阅 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>。</span><span class="sxs-lookup"><span data-stu-id="0b736-411">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>.</span></span>

### <a name="confirm-intent-to-support-public-non-virtual-methods-closed"></a><span data-ttu-id="0b736-412">确认意图以支持公共非虚方法（已关闭）</span><span class="sxs-lookup"><span data-stu-id="0b736-412">Confirm intent to support public non-virtual methods (closed)</span></span>

<span data-ttu-id="0b736-413">我们是否可以确认（或反转）我们决定是否允许接口中的非虚拟公共方法？</span><span class="sxs-lookup"><span data-stu-id="0b736-413">Can we please confirm (or reverse) our decision to permit non-virtual public methods in an interface?</span></span>

```csharp
interface IA
{
    public sealed void M() { }
}
```

> <span data-ttu-id="0b736-414">***半闭问题：*** （2017-04-18）我们认为它将有用，但会返回到它。</span><span class="sxs-lookup"><span data-stu-id="0b736-414">***Semi-Closed Issue:*** (2017-04-18) We think it is going to be useful, but will come back to it.</span></span> <span data-ttu-id="0b736-415">这是一种精神模型的工作块。</span><span class="sxs-lookup"><span data-stu-id="0b736-415">This is a mental model tripping block.</span></span>

<span data-ttu-id="0b736-416">***决策***：是。</span><span class="sxs-lookup"><span data-stu-id="0b736-416">***Decision***: Yes.</span></span> <span data-ttu-id="0b736-417"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods> 列中的一个值匹配。</span><span class="sxs-lookup"><span data-stu-id="0b736-417"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>.</span></span>

### <a name="does-an-override-in-an-interface-introduce-a-new-member-closed"></a><span data-ttu-id="0b736-418">接口中的 `override` 是否引入了新成员？</span><span class="sxs-lookup"><span data-stu-id="0b736-418">Does an `override` in an interface introduce a new member?</span></span> <span data-ttu-id="0b736-419">闭</span><span class="sxs-lookup"><span data-stu-id="0b736-419">(closed)</span></span>

<span data-ttu-id="0b736-420">有几种方法可以观察重写声明是否引入了新成员。</span><span class="sxs-lookup"><span data-stu-id="0b736-420">There are a few ways to observe whether an override declaration introduces a new member or not.</span></span>

```csharp
interface IA
{
    void M(int x) { }
}
interface IB : IA
{
    override void M(int y) { }
}
interface IC : IB
{
    static void M2()
    {
        M(y: 3); // permitted?
    }
    override void IB.M(int z) { } // permitted? What does it override?
}
```

> <span data-ttu-id="0b736-421">***打开问题：*** 接口中的重写声明是否引入了新成员？</span><span class="sxs-lookup"><span data-stu-id="0b736-421">***Open Issue:*** Does an override declaration in an interface introduce a new member?</span></span> <span data-ttu-id="0b736-422">闭</span><span class="sxs-lookup"><span data-stu-id="0b736-422">(closed)</span></span>

<span data-ttu-id="0b736-423">在类中，重写方法在某种方式上为 "visible"。</span><span class="sxs-lookup"><span data-stu-id="0b736-423">In a class, an overriding method is "visible" in some senses.</span></span> <span data-ttu-id="0b736-424">例如，其参数的名称优先于重写方法中的参数的名称。</span><span class="sxs-lookup"><span data-stu-id="0b736-424">For example, the names of its parameters take precedence over the names of parameters in the overridden method.</span></span> <span data-ttu-id="0b736-425">可以在接口中重复该行为，因为始终存在最特定的重写。</span><span class="sxs-lookup"><span data-stu-id="0b736-425">It may be possible to duplicate that behavior in interfaces, as there is always a most specific override.</span></span> <span data-ttu-id="0b736-426">但我们想要复制该行为吗？</span><span class="sxs-lookup"><span data-stu-id="0b736-426">But do we want to duplicate that behavior?</span></span>

<span data-ttu-id="0b736-427">此外，还可以 "重写" 重写方法呢？</span><span class="sxs-lookup"><span data-stu-id="0b736-427">Also, it is possible to "override" an override method?</span></span> <span data-ttu-id="0b736-428">差异</span><span class="sxs-lookup"><span data-stu-id="0b736-428">[Moot]</span></span>

<span data-ttu-id="0b736-429">***决策***：不允许在接口成员上使用 `override` 关键字。</span><span class="sxs-lookup"><span data-stu-id="0b736-429">***Decision***: No `override` keyword permitted on interface members.</span></span> <span data-ttu-id="0b736-430"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member> 列中的一个值匹配。</span><span class="sxs-lookup"><span data-stu-id="0b736-430"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>.</span></span>

### <a name="properties-with-a-private-accessor-closed"></a><span data-ttu-id="0b736-431">具有专用访问器的属性（关闭）</span><span class="sxs-lookup"><span data-stu-id="0b736-431">Properties with a private accessor (closed)</span></span>

<span data-ttu-id="0b736-432">我们说，private 成员不是虚拟的，不允许结合虚拟和私有。</span><span class="sxs-lookup"><span data-stu-id="0b736-432">We say that private members are not virtual, and the combination of virtual and private is disallowed.</span></span> <span data-ttu-id="0b736-433">但对于具有专用访问器的属性怎么办呢？</span><span class="sxs-lookup"><span data-stu-id="0b736-433">But what about a property with a private accessor?</span></span>

```csharp
interface IA
{
    public virtual int P
    {
        get => 3;
        private set => { }
    }
}
```

<span data-ttu-id="0b736-434">是否允许这样做？</span><span class="sxs-lookup"><span data-stu-id="0b736-434">Is this allowed?</span></span> <span data-ttu-id="0b736-435">`set` 访问器是否 `virtual`？</span><span class="sxs-lookup"><span data-stu-id="0b736-435">Is the `set` accessor here `virtual` or not?</span></span> <span data-ttu-id="0b736-436">它可以在可访问的位置重写吗？</span><span class="sxs-lookup"><span data-stu-id="0b736-436">Can it be overridden where it is accessible?</span></span> <span data-ttu-id="0b736-437">下面是否隐式仅实现 `get` 访问器？</span><span class="sxs-lookup"><span data-stu-id="0b736-437">Does the following implicitly implement only the `get` accessor?</span></span>

```csharp
class C : IA
{
    public int P
    {
        get => 4;
        set { }
    }
}
```

<span data-ttu-id="0b736-438">由于 IA，以下原因可能会导致错误。设置不是虚拟的，也可能是因为它无法访问？</span><span class="sxs-lookup"><span data-stu-id="0b736-438">Is the following presumably an error because IA.P.set isn't virtual and also because it isn't accessible?</span></span>

```csharp
class C : IA
{
    int IA.P
    {
        get => 4;
        set { }
    }
}
```

<span data-ttu-id="0b736-439">***决策***：第一个示例看起来有效，而最后一个示例则无效。</span><span class="sxs-lookup"><span data-stu-id="0b736-439">***Decision***: The first example looks valid, while the last does not.</span></span> <span data-ttu-id="0b736-440">这会被解析为类似在中的C#工作方式。</span><span class="sxs-lookup"><span data-stu-id="0b736-440">This is resolved analogously to how it already works in C#.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#properties-with-a-private-accessor>

### <a name="base-interface-invocations-round-2-closed"></a><span data-ttu-id="0b736-441">基接口调用，舍入2（关闭）</span><span class="sxs-lookup"><span data-stu-id="0b736-441">Base Interface Invocations, round 2 (closed)</span></span>

<span data-ttu-id="0b736-442">前面的 "解决方法" 是如何处理基本调用，实际上不提供足够的表现力。</span><span class="sxs-lookup"><span data-stu-id="0b736-442">Our previous "resolution" to how to handle base invocations doesn't actually provide sufficient expressiveness.</span></span> <span data-ttu-id="0b736-443">事实证明，在和C# CLR 中，与 Java 不同，您需要指定包含方法声明的接口以及要调用的实现的位置。</span><span class="sxs-lookup"><span data-stu-id="0b736-443">It turns out that in C# and the CLR, unlike Java, you need to specify both the interface containing the method declaration and the location of the implementation you want to invoke.</span></span>

<span data-ttu-id="0b736-444">我建议将以下语法用于接口中的基本调用。</span><span class="sxs-lookup"><span data-stu-id="0b736-444">I propose the following syntax for base calls in interfaces.</span></span> <span data-ttu-id="0b736-445">我不喜欢它，但它说明了什么语法必须能够表达：</span><span class="sxs-lookup"><span data-stu-id="0b736-445">I’m not in love with it, but it illustrates what any syntax must be able to express:</span></span>

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I4 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>(I1).M(); // calls I3's implementation of I1.M
        base<I4>(I1).M(); // calls I4's implementation of I1.M
    }
    void I2.M()
    {
        base<I3>(I2).M(); // calls I3's implementation of I2.M
        base<I4>(I2).M(); // calls I4's implementation of I2.M
    }
}
```

<span data-ttu-id="0b736-446">如果没有歧义，可以更简单地编写</span><span class="sxs-lookup"><span data-stu-id="0b736-446">If there is no ambiguity, you can write it more simply</span></span>

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I4 : I1 { void I1.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>.M(); // calls I3's implementation of I1.M
        base<I4>.M(); // calls I4's implementation of I1.M
    }
}
```

<span data-ttu-id="0b736-447">或</span><span class="sxs-lookup"><span data-stu-id="0b736-447">Or</span></span>

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base(I1).M(); // calls I3's implementation of I1.M
    }
    void I2.M()
    {
        base(I2).M(); // calls I3's implementation of I2.M
    }
}
```

<span data-ttu-id="0b736-448">或</span><span class="sxs-lookup"><span data-stu-id="0b736-448">Or</span></span>

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base.M(); // calls I3's implementation of I1.M
    }
}
```

<span data-ttu-id="0b736-449">***决策***：决定 `base(N.I1<T>).M(s)`conceding，如果有调用绑定，稍后会出现问题。</span><span class="sxs-lookup"><span data-stu-id="0b736-449">***Decision***: Decided on `base(N.I1<T>).M(s)`, conceding that if we have an invocation binding there may be problem here later on.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md#default-interface-implementations>

### <a name="warning-for-struct-not-implementing-default-method-closed"></a><span data-ttu-id="0b736-450">结构未实现默认方法时的警告？</span><span class="sxs-lookup"><span data-stu-id="0b736-450">Warning for struct not implementing default method?</span></span> <span data-ttu-id="0b736-451">闭</span><span class="sxs-lookup"><span data-stu-id="0b736-451">(closed)</span></span>

<span data-ttu-id="0b736-452">@vancem 断言，如果值类型声明未能重写某些接口方法，则应认真考虑生成警告，即使它会从接口继承该方法的实现。</span><span class="sxs-lookup"><span data-stu-id="0b736-452">@vancem asserts that we should seriously consider producing a warning if a value type declaration fails to override some interface method, even if it would inherit an implementation of that method from an interface.</span></span> <span data-ttu-id="0b736-453">因为这会导致装箱和会破坏约束调用。</span><span class="sxs-lookup"><span data-stu-id="0b736-453">Because it causes boxing and undermines constrained calls.</span></span>

<span data-ttu-id="0b736-454">***决策***：看起来就像是更适合分析器的内容。</span><span class="sxs-lookup"><span data-stu-id="0b736-454">***Decision***: This seems like something more suited for an analyzer.</span></span> <span data-ttu-id="0b736-455">这似乎会干扰此警告，因为即使从未调用默认接口方法，也不会出现任何装箱。</span><span class="sxs-lookup"><span data-stu-id="0b736-455">It also seems like this warning could be noisy, since it would fire even if the default interface method is never called and no boxing will ever occur.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#warning-for-struct-not-implementing-default-method>

### <a name="interface-static-constructors-closed"></a><span data-ttu-id="0b736-456">接口静态构造函数（已关闭）</span><span class="sxs-lookup"><span data-stu-id="0b736-456">Interface static constructors (closed)</span></span>

<span data-ttu-id="0b736-457">何时运行接口静态构造函数？</span><span class="sxs-lookup"><span data-stu-id="0b736-457">When are interface static constructors run?</span></span>  <span data-ttu-id="0b736-458">当前 CLI 草稿建议在访问第一个静态方法或字段时发生。</span><span class="sxs-lookup"><span data-stu-id="0b736-458">The current CLI draft proposes that it occurs when the first static method or field is accessed.</span></span> <span data-ttu-id="0b736-459">如果没有这样做，则可能永远不会运行它？</span><span class="sxs-lookup"><span data-stu-id="0b736-459">If there are neither of those then it might never be run??</span></span>

<span data-ttu-id="0b736-460">[2018-10-09 CLR 团队建议 "转而对具有进行镜像（对每个实例方法的访问进行 .cctor 检查）"]</span><span class="sxs-lookup"><span data-stu-id="0b736-460">[2018-10-09 The CLR team proposes "Going to mirror what we do for valuetypes (cctor check on access to each instance method)"]</span></span>

<span data-ttu-id="0b736-461">***决策***：如果未 `beforefieldinit`静态构造函数，则静态构造函数也会在输入到实例方法时运行，在这种情况下，将在访问第一个静态字段之前运行静态构造函数。</span><span class="sxs-lookup"><span data-stu-id="0b736-461">***Decision***: Static constructors are also run on entry to instance methods, if the static constructor was not `beforefieldinit`, in which case static constructors are run before access to the first static field.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#when-are-interface-static-constructors-run>

## <a name="design-meetings"></a><span data-ttu-id="0b736-462">设计会议</span><span class="sxs-lookup"><span data-stu-id="0b736-462">Design meetings</span></span>

<span data-ttu-id="0b736-463">[2017-03-08 Ldm 会议说明](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md)
[2017-03-21 ldm 会议说明](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)
[2017-03-23 会议 "默认接口方法的 CLR 行为"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)
[2017-04-05 LDM 会议](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md)笔记
[2017-04-11 ldm 会议](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md)笔记
2017-04-18 [ldm](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md)会议笔记
2017-04-19 [ldm](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md)会议笔记
2017-05-17 ldm[会议笔记](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md)
[2017-05-31 ldm](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md) [会议说明](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md)
2018-10-17 [2018-11-14](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)
的[ldm 会议笔记](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
</span><span class="sxs-lookup"><span data-stu-id="0b736-463">[2017-03-08 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md)
[2017-03-21 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)
[2017-03-23 meeting "CLR Behavior for Default Interface Methods"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)
[2017-04-05 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md)
[2017-04-11 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md)
[2017-04-18 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md)
[2017-04-19 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md)
[2017-05-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md)
[2017-05-31 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md)
[2017-06-14 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md)
[2018-10-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
[2018-11-14 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)</span></span>
