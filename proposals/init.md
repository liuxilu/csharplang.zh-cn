---
ms.openlocfilehash: ebbdab6d121f3001ac34a953b3de09768cda6344
ms.sourcegitcommit: 3f177e90b12e39d4d28f8bb1064df81a8e5912ca
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "81726052"
---
<a name="init-only-members"></a><span data-ttu-id="54569-101">仅 Init 会员</span><span class="sxs-lookup"><span data-stu-id="54569-101">Init Only Members</span></span>
=====

## <a name="summary"></a><span data-ttu-id="54569-102">总结</span><span class="sxs-lookup"><span data-stu-id="54569-102">Summary</span></span>
<span data-ttu-id="54569-103">此建议将仅 init 属性的概念添加到 C# 中。</span><span class="sxs-lookup"><span data-stu-id="54569-103">This proposal adds the concept of init only properties to C#.</span></span> <span data-ttu-id="54569-104">这些属性可以在对象创建点设置，但仅在对象创建完成后才`get`有效。</span><span class="sxs-lookup"><span data-stu-id="54569-104">These properties can be set at the point of object creation but become effectively `get` only once object creation has completed.</span></span> <span data-ttu-id="54569-105">这允许在 C# 中建立更灵活的不可变模型。</span><span class="sxs-lookup"><span data-stu-id="54569-105">This allows for a much more flexible immutable model in C#.</span></span> 

## <a name="motivation"></a><span data-ttu-id="54569-106">动机</span><span class="sxs-lookup"><span data-stu-id="54569-106">Motivation</span></span>
<span data-ttu-id="54569-107">自 1.0 以来，在 C# 中构建不可变数据的基本机制一直没有改变。</span><span class="sxs-lookup"><span data-stu-id="54569-107">The underlying mechanisms for building immutable data in C# haven't changed since 1.0.</span></span> <span data-ttu-id="54569-108">它们仍然存在：</span><span class="sxs-lookup"><span data-stu-id="54569-108">They remain:</span></span>

1. <span data-ttu-id="54569-109">将字段声明`readonly`为 。</span><span class="sxs-lookup"><span data-stu-id="54569-109">Declaring fields as `readonly`.</span></span>
1. <span data-ttu-id="54569-110">声明仅包含`get`访问器的属性。</span><span class="sxs-lookup"><span data-stu-id="54569-110">Declaring properties that contain only a `get` accessor.</span></span>

<span data-ttu-id="54569-111">这些机制有效地允许构造不可变数据，但它们通过增加类型样板代码的成本，并选择此类类型，使其退出对象和集合初始化器等功能。</span><span class="sxs-lookup"><span data-stu-id="54569-111">These mechanisms are effective at allowing the construction of immutable data but they do so by adding cost to the boilerplate code of types and opting such types out of features like object and collection initializers.</span></span> <span data-ttu-id="54569-112">这意味着开发人员必须在易用性和不变性之间做出选择。</span><span class="sxs-lookup"><span data-stu-id="54569-112">This means developers must choose between ease of use and immutability.</span></span>

<span data-ttu-id="54569-113">简单的不可变对象（如）`Point`需要两倍于声明类型的锅炉板代码来支持构造。</span><span class="sxs-lookup"><span data-stu-id="54569-113">A simple immutable object like `Point` requires twice as much boiler plate code to support construction as it does to declare the type.</span></span> <span data-ttu-id="54569-114">类型越大，锅炉板的成本越大：</span><span class="sxs-lookup"><span data-stu-id="54569-114">The bigger the type the bigger the cost of this boiler plate:</span></span>

```cs
struct Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int X, int Y)
    {
        this.X = x;
        this.Y = y;
    }
}
```

<span data-ttu-id="54569-115">访问`init`器允许调用方在构造过程中更改成员，从而使不可变对象更加灵活。</span><span class="sxs-lookup"><span data-stu-id="54569-115">The `init` accessor makes immutable objects more flexible by allowing the caller to mutate the members during the act of construction.</span></span> <span data-ttu-id="54569-116">这意味着对象的不可变属性可以参与对象初始化，从而不再需要类型中的所有构造函数样板。</span><span class="sxs-lookup"><span data-stu-id="54569-116">That means the object's immutable properties can participate in object initializers and thus removes the need for all constructor boilerplate in the type.</span></span> <span data-ttu-id="54569-117">该`Point`类型现在很简单：</span><span class="sxs-lookup"><span data-stu-id="54569-117">The `Point` type is now simply:</span></span>

```cs
struct Point
{
    public int X { get; init; }
    public int Y { get; init; }
}
```

<span data-ttu-id="54569-118">然后，使用者可以使用对象初始化程序创建对象</span><span class="sxs-lookup"><span data-stu-id="54569-118">The consumer can then use object initializers to create the object</span></span>

```cs
var p = new Point() { X = 42, Y = 13 };
```

## <a name="detailed-design"></a><span data-ttu-id="54569-119">详细设计</span><span class="sxs-lookup"><span data-stu-id="54569-119">Detailed Design</span></span>

### <a name="init-members"></a><span data-ttu-id="54569-120">init 成员</span><span class="sxs-lookup"><span data-stu-id="54569-120">init members</span></span>
<span data-ttu-id="54569-121">仅使用`init`访问器代替`set`访问器声明 init 属性：</span><span class="sxs-lookup"><span data-stu-id="54569-121">An init only property is declared by using the `init` accessor in place of the `set` accessor:</span></span>

```cs
class Student
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
}
```

<span data-ttu-id="54569-122">在以下情况下，包含`init`访问器的实例属性被视为可设置的：</span><span class="sxs-lookup"><span data-stu-id="54569-122">An instance property containing an `init` accessor is considered settable in the following circumstances:</span></span>

- <span data-ttu-id="54569-123">在对象初始化过程中</span><span class="sxs-lookup"><span data-stu-id="54569-123">During an object initializer</span></span>
- <span data-ttu-id="54569-124">在包含或派生类型的实例构造函数中，在`this`或`base`</span><span class="sxs-lookup"><span data-stu-id="54569-124">Inside an instance constructor of the containing or derived type, on `this` or `base`</span></span>
- <span data-ttu-id="54569-125">在任何属性`init`的检修器内，在`this`或`base`</span><span class="sxs-lookup"><span data-stu-id="54569-125">Inside the `init` accessor of any property, on `this` or `base`</span></span>

<span data-ttu-id="54569-126">本文档将`init`访问器设置为可设置的时间统称为对象的构造阶段。</span><span class="sxs-lookup"><span data-stu-id="54569-126">The times above in which the `init` accessors are settable are collectively referred to in this document as the construction phase of the object.</span></span>

<span data-ttu-id="54569-127">这意味着类`Student`可以通过以下方式使用：</span><span class="sxs-lookup"><span data-stu-id="54569-127">This means the `Student` class can be used in the following ways:</span></span>

```cs
var s = new Student()
{
    FirstName = "Jared",
    LastName = "Parosns",
};
s.LastName = "Parsons"; // Error: LastName is not settable
```

<span data-ttu-id="54569-128">访问器可设置`init`时的规则跨类型层次结构扩展。</span><span class="sxs-lookup"><span data-stu-id="54569-128">The rules around when `init` accessors are settable extend across type hierarchies.</span></span> <span data-ttu-id="54569-129">如果成员是可访问的，并且已知对象处于构造阶段，则该成员是可设置的。</span><span class="sxs-lookup"><span data-stu-id="54569-129">If the member is accessible and the object is known to be in the construction phase then the member is settable.</span></span> <span data-ttu-id="54569-130">这特别允许以下事项：</span><span class="sxs-lookup"><span data-stu-id="54569-130">That specifically allows for the following:</span></span>

```cs
class Base
{
    public bool Value { get; init; }
}

class Derived : Base
{
    Derived()
    {
        // Not allowed with get only properties but allowed with init
        Value = true;
    }
}

class Consumption
{
    void Example()
    {
        var d = new Derived() { Value = true; };
    }
}

```

<span data-ttu-id="54569-131">在调用`init`访问器时，已知实例处于打开构造阶段。</span><span class="sxs-lookup"><span data-stu-id="54569-131">At the point a `init` accessor is invoked the instance is known to be in the open construction phase.</span></span> <span data-ttu-id="54569-132">因此，`init`除了普通`set`访问器可以执行的操作外，允许访问器执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="54569-132">Hence an `init` accessor is allowed to take the following actions in addition to what a normal `set` accessor can do:</span></span>

1. <span data-ttu-id="54569-133">通过`init``this`或 调用其他访问器`base`</span><span class="sxs-lookup"><span data-stu-id="54569-133">Call other `init` accessors available through `this` or `base`</span></span>
1. <span data-ttu-id="54569-134">分配`readonly`在同一类型上声明的字段</span><span class="sxs-lookup"><span data-stu-id="54569-134">Assign `readonly` fields declared on the same type</span></span>

```cs
class Complex
{
    readonly int Field1;
    int Field2;
    int Prop1 { get; init ; }
    int Prop2
    {
        get => 42;
        init
        {
            Field1 = 13; // okay
            Field2 = 13; // okay
            Prop1 = 13; // okay
        }
    }
}
```

<span data-ttu-id="54569-135">从`init`访问器分配`readonly`字段的能力仅限于与访问器类型相同的字段。</span><span class="sxs-lookup"><span data-stu-id="54569-135">The ability to assign `readonly` fields from an `init` accessor is limited to those fields declared on the same type as the accessor.</span></span> <span data-ttu-id="54569-136">它不能用于分配`readonly`基类型的字段。</span><span class="sxs-lookup"><span data-stu-id="54569-136">It cannot be used to assign `readonly` fields in a base type.</span></span> <span data-ttu-id="54569-137">此规则可确保类型作者始终控制其类型的可变性行为。</span><span class="sxs-lookup"><span data-stu-id="54569-137">This rule ensures that type authors remain in control over the mutability behavior of their type.</span></span> <span data-ttu-id="54569-138">不希望利用`init`的开发人员不能受到选择这样做的其他类型的影响：</span><span class="sxs-lookup"><span data-stu-id="54569-138">Developers who do not wish to utilize `init` cannot be impacted from other types choosing to do so:</span></span>

```cs
class Base
{
    internal readonly int Field;
    internal int Property
    {
        get => Field;
        init => Field = value; // Okay
    }

    internal int OtherProperty { get; init; }
}

class Derived : Base
{
    internal readonly int DerivedField;
    internal int DerivedProperty
    {
        get => DerivedField;
        init
        {
            DerivedField = 42;  // Okay
            Property = 0;       // Okay
            Field = 13;         // Error Field is readonly
        }
    }

    public Derived()
    {
        Property = 42;  // Okay 
        Field = 13;     // Error Field is readonly
    }
}
```

<span data-ttu-id="54569-139">在`init`虚拟属性中使用时，还必须将所有重写标记为`init`。</span><span class="sxs-lookup"><span data-stu-id="54569-139">When `init` is used in a virtual property then all the overrides must also be marked as `init`.</span></span> <span data-ttu-id="54569-140">同样，不可能用 重写简单`set`。 `init`</span><span class="sxs-lookup"><span data-stu-id="54569-140">Likewise it is not possible to override a simple `set` with `init`.</span></span>

```cs
class Base
{
    public virtual int Property { get; init; }
}

class C1 : Base
{
    public override int Property { get; init; }
}

class C2 : Base
{
    // Error: Property must have init to override Base.Property
    public override int Property { get; set; }
}
```

<span data-ttu-id="54569-141">声明`interface`还可以通过以下模式参与`init`样式初始化：</span><span class="sxs-lookup"><span data-stu-id="54569-141">An `interface` declaration can also participate in `init` style initialization via the following pattern:</span></span>

```cs
interface IPerson
{
    string Name { get; init; }
}

class Init
{
    void M<T>() where T : IPerson, new()
    {
        var local = new T()
        {
            Name = "Jared"
        };
        local.Name = "Jraed"; // Error
    }
}
```

<span data-ttu-id="54569-142">此功能的限制：</span><span class="sxs-lookup"><span data-stu-id="54569-142">Restrictions of this feature:</span></span>
- <span data-ttu-id="54569-143">`init`访问器只能在实例属性上使用</span><span class="sxs-lookup"><span data-stu-id="54569-143">The `init` accessor can only be used on instance properties</span></span>
- <span data-ttu-id="54569-144">属性不能同时包含 和`init``set`访问器</span><span class="sxs-lookup"><span data-stu-id="54569-144">A property cannot contain both an `init` and `set` accessor</span></span>
- <span data-ttu-id="54569-145">如果基础具有 ，则属性的所有`init`重写都必须具有`init`。</span><span class="sxs-lookup"><span data-stu-id="54569-145">All overrides of a property must have `init` if the base had `init`.</span></span> <span data-ttu-id="54569-146">此规则也适用于接口实现。</span><span class="sxs-lookup"><span data-stu-id="54569-146">This rule also applies to interface implementation.</span></span>

### <a name="metadata-encoding"></a><span data-ttu-id="54569-147">元数据编码</span><span class="sxs-lookup"><span data-stu-id="54569-147">Metadata encoding</span></span> 
<span data-ttu-id="54569-148">属性`init`访问器将作为标准`set`访问器发出，返回类型标有 modreq `IsInitOnly`。</span><span class="sxs-lookup"><span data-stu-id="54569-148">Property `init` accessors will be emitted as a standard `set` accessor with the return type marked with a modreq of `IsInitOnly`.</span></span> <span data-ttu-id="54569-149">这是一种具有以下定义的新类型：</span><span class="sxs-lookup"><span data-stu-id="54569-149">This is a new type which will have the following definition:</span></span>

```cs
namespace System.Runtime.CompilerServices
{
    public sealed class IsInitOnly
    {
    }
}
```

<span data-ttu-id="54569-150">编译器将按全名匹配类型。</span><span class="sxs-lookup"><span data-stu-id="54569-150">The compiler will match the type by full name.</span></span> <span data-ttu-id="54569-151">没有要求它出现在核心库中。</span><span class="sxs-lookup"><span data-stu-id="54569-151">There is no requirement that it appear in the core library.</span></span> <span data-ttu-id="54569-152">如果此名称有多个类型，则编译器将按以下顺序绑定中断：</span><span class="sxs-lookup"><span data-stu-id="54569-152">If there are multiple types by this name then the compiler will tie break in the following order:</span></span>

1. <span data-ttu-id="54569-153">正在编译的项目中定义的一个</span><span class="sxs-lookup"><span data-stu-id="54569-153">The one defined in the project being compiled</span></span>
1. <span data-ttu-id="54569-154">在核心利库中定义的那个</span><span class="sxs-lookup"><span data-stu-id="54569-154">The one defined in corelib</span></span>

<span data-ttu-id="54569-155">如果两者都不存在，则将发出类型歧义错误。</span><span class="sxs-lookup"><span data-stu-id="54569-155">If neither of these exist then a type ambiguity error will be issued.</span></span>

<span data-ttu-id="54569-156">本期[中](https://github.com/dotnet/runtime/issues/34978)包括`IsInitOnly`的"设计"</span><span class="sxs-lookup"><span data-stu-id="54569-156">The design for `IsInitOnly` is futher covered in [this issue](https://github.com/dotnet/runtime/issues/34978)</span></span>

## <a name="questions"></a><span data-ttu-id="54569-157">问题</span><span class="sxs-lookup"><span data-stu-id="54569-157">Questions</span></span>

### <a name="breaking-changes"></a><span data-ttu-id="54569-158">重大更改</span><span class="sxs-lookup"><span data-stu-id="54569-158">Breaking changes</span></span>
<span data-ttu-id="54569-159">如何编码此功能的主要枢轴点之一将归结为以下问题：</span><span class="sxs-lookup"><span data-stu-id="54569-159">One of the main pivot points in how this feature is encoded will come down to the following question:</span></span> 

> <span data-ttu-id="54569-160">替换为二进制分解更改`init``set`吗？</span><span class="sxs-lookup"><span data-stu-id="54569-160">Is it a binary breaking change to replace `init` with `set`?</span></span>

<span data-ttu-id="54569-161">替换`init``set`属性并使属性完全可写，从来都不是对非虚拟属性的开源更改。</span><span class="sxs-lookup"><span data-stu-id="54569-161">Replacing `init` with `set` and thus making a property fully writable is never a source breaking change on a non-virtual property.</span></span> <span data-ttu-id="54569-162">它只是展开可以写入属性的方案集。</span><span class="sxs-lookup"><span data-stu-id="54569-162">It simply expands the set of scenarios where the property can be written.</span></span> <span data-ttu-id="54569-163">唯一的行为是，这是否仍然是二进制突破更改。</span><span class="sxs-lookup"><span data-stu-id="54569-163">The only behavior in question is whether or not this remains a binary breaking change.</span></span>

<span data-ttu-id="54569-164">如果我们想要对源和二进制兼容`init`更改`set`进行更改，那么它将迫使我们手在 modreq 与下面的属性决策，因为它将排除 modreqs 作为灵魂。</span><span class="sxs-lookup"><span data-stu-id="54569-164">If we want to make the change of `init` to `set` a source and binary compatible change then it will force our hand on the modreq vs. attributes decision below because it will rule out modreqs as a soulution.</span></span> <span data-ttu-id="54569-165">另一方面，如果这被视为一个不感兴趣，那么这将使modreq与属性决策的影响较小。</span><span class="sxs-lookup"><span data-stu-id="54569-165">If on the other hand this is seen as a non-interesting then this will make the modreq vs. attribute decision less impactful.</span></span>

<span data-ttu-id="54569-166">**分辨率**LDM 认为此方案并不令人信服。</span><span class="sxs-lookup"><span data-stu-id="54569-166">**Resolution** This scenario is not seen as compelling by LDM.</span></span>

### <a name="modreqs-vs-attributes"></a><span data-ttu-id="54569-167">Modreqs vs. 属性</span><span class="sxs-lookup"><span data-stu-id="54569-167">Modreqs vs. attributes</span></span>
<span data-ttu-id="54569-168">属性访问器的`init`发出策略必须在在元数据期间发出时使用属性或 modreq 之间进行选择。</span><span class="sxs-lookup"><span data-stu-id="54569-168">The emit strategy for `init` property accessors must choose between using attributes or modreqs when emitting during metadata.</span></span> <span data-ttu-id="54569-169">这些有不同的权衡，需要考虑。</span><span class="sxs-lookup"><span data-stu-id="54569-169">These have different trade offs that need to be considered.</span></span>

<span data-ttu-id="54569-170">使用 modreq 声明对属性集访问器进行说明意味着符合 CLI 的编译器将忽略访问器，除非它了解 modreq。</span><span class="sxs-lookup"><span data-stu-id="54569-170">Annotating a property set accessor with a modreq declaration means CLI compliant compilers will ignore the accessor unless it understands the modreq.</span></span> <span data-ttu-id="54569-171">这意味着只有知道的`init`编译器才会读取成员。</span><span class="sxs-lookup"><span data-stu-id="54569-171">That means only compilers aware of `init` will read the member.</span></span> <span data-ttu-id="54569-172">不知道会忽略该`init`成员的编译器`set`，因此不会意外地将属性视为读/写。</span><span class="sxs-lookup"><span data-stu-id="54569-172">Compilers unaware of `init` will ignore the `set` member and hence will not accidentally treat the property as read / write.</span></span> 

<span data-ttu-id="54569-173">modreq 的缺点是`init`成为`set`访问器的二进制签名的一部分。</span><span class="sxs-lookup"><span data-stu-id="54569-173">The downside of modreq is `init` becomes a part of the binary signature of the `set` accessor.</span></span> <span data-ttu-id="54569-174">添加或删除`init`将破坏应用程序的二进制兼容性。</span><span class="sxs-lookup"><span data-stu-id="54569-174">Adding or removing `init` will break binary compatbility of the application.</span></span>

<span data-ttu-id="54569-175">使用属性对`set`访问器进行说明意味着只有了解该属性的编译器才能知道限制对该属性的访问。</span><span class="sxs-lookup"><span data-stu-id="54569-175">Using attributes to annotate the `set` accessor means that only compilers which understand the attribute will know to limit access to it.</span></span> <span data-ttu-id="54569-176">不知道的`init`编译器会将其视为一个简单的读取/写入属性并允许访问。</span><span class="sxs-lookup"><span data-stu-id="54569-176">A compiler unaware of `init` will see it as a simple read / write property and allow access.</span></span>

<span data-ttu-id="54569-177">这似乎意味着这个决定是在以牺牲二进制兼容性为代价的额外安全性之间做出选择。</span><span class="sxs-lookup"><span data-stu-id="54569-177">This would seemingly mean this decision is a choice between extra safety at the expense of binary compatibility.</span></span> <span data-ttu-id="54569-178">挖掘一点额外的安全性不完全是它看起来。</span><span class="sxs-lookup"><span data-stu-id="54569-178">Digging in a bit the extra safety is not exactly what it seems.</span></span> <span data-ttu-id="54569-179">它无法防止出现以下情况：</span><span class="sxs-lookup"><span data-stu-id="54569-179">It will not protect against the following circumstances:</span></span>

1. <span data-ttu-id="54569-180">对成员`public`的反思</span><span class="sxs-lookup"><span data-stu-id="54569-180">Reflection over `public` members</span></span>
1. <span data-ttu-id="54569-181">使用`dynamic`</span><span class="sxs-lookup"><span data-stu-id="54569-181">The use of `dynamic`</span></span> 
1. <span data-ttu-id="54569-182">无法识别 modreq 的编译器</span><span class="sxs-lookup"><span data-stu-id="54569-182">Compilers that don't recognize modreqs</span></span>

<span data-ttu-id="54569-183">还应考虑，当我们完成 .NET 5 的 IL 验证规则时，`init`将是这些规则之一。</span><span class="sxs-lookup"><span data-stu-id="54569-183">It should also be considered that, when we complete the IL verification rules for .NET 5, `init` will be one of those rules.</span></span> <span data-ttu-id="54569-184">这意味着只需验证发出可验证 IL 的编译器，就会获得额外的强制。</span><span class="sxs-lookup"><span data-stu-id="54569-184">That means extra enforcement will be gained from simply verifying compilers emitting verifiable IL.</span></span>

<span data-ttu-id="54569-185">将更新 .NET（C#、F# 和 VB）的主要语言以识别这些`init`访问器。</span><span class="sxs-lookup"><span data-stu-id="54569-185">The primary languages for .NET (C#, F# and VB) will all be updated to recognize these `init` accessors.</span></span> <span data-ttu-id="54569-186">因此，这里唯一现实的情况是，当 C# 9`init`编译器发出属性，并且它们被较旧的工具集（如 C# 8、VB 15 等）看到时...C# 8.</span><span class="sxs-lookup"><span data-stu-id="54569-186">Hence the only realistic scenario here is when a C# 9 compiler emits `init` properties and they are seen by an older toolset such as C# 8, VB 15, etc ... C# 8.</span></span> <span data-ttu-id="54569-187">这是考虑和权衡二进制兼容性的权衡。</span><span class="sxs-lookup"><span data-stu-id="54569-187">That is the trade off to consider and weigh against binary compatibility.</span></span>

<span data-ttu-id="54569-188">**注意**此讨论主要适用于成员，不适用于字段。</span><span class="sxs-lookup"><span data-stu-id="54569-188">**Note** This discussion primarily applies to members only, not to fields.</span></span> <span data-ttu-id="54569-189">虽然`init`字段被 LDM 拒绝，但它们在 modreq 与属性讨论中仍值得考虑。</span><span class="sxs-lookup"><span data-stu-id="54569-189">While `init` fields were rejected by LDM they are still interesting to consider for the modreq vs. attribute discussion.</span></span> <span data-ttu-id="54569-190">字段`init`的特征是放宽了现有的限制`readonly`。</span><span class="sxs-lookup"><span data-stu-id="54569-190">The `init` feature for fields is a relaxation of the existing restriction of `readonly`.</span></span> <span data-ttu-id="54569-191">这意味着，如果我们将字段作为`readonly`属性发出，则没有使用该字段的旧编译器的风险，因为它们已经识别`readonly`。</span><span class="sxs-lookup"><span data-stu-id="54569-191">That means if we emit the fields as `readonly` + an attribute there is no risk of older compilers mis-using the field because they would already recognize `readonly`.</span></span> <span data-ttu-id="54569-192">因此，在此处使用 modreq 不会添加任何额外的保护。</span><span class="sxs-lookup"><span data-stu-id="54569-192">Hence using a modreq here doesn't add any extra protection.</span></span>

<span data-ttu-id="54569-193">**分辨率**该功能将使用 modreq 对属性`init`设置器进行编码。</span><span class="sxs-lookup"><span data-stu-id="54569-193">**Resolution** The feature will use a modreq to encode the property `init` setter.</span></span> <span data-ttu-id="54569-194">引人注目的因素是（没有特别的顺序）：</span><span class="sxs-lookup"><span data-stu-id="54569-194">The compelling factors were (in no particular order):</span></span>

* <span data-ttu-id="54569-195">希望阻止较旧的编译器违反`init`语义</span><span class="sxs-lookup"><span data-stu-id="54569-195">Desire to discourage older compilers from violating `init` semantics</span></span>
* <span data-ttu-id="54569-196">希望在`virtual`声明中或`interface`同时添加或删除`init`源和二进制中断更改。</span><span class="sxs-lookup"><span data-stu-id="54569-196">Desire to make adding or removing `init` in a `virtual` declaration or `interface` both a source and binary breaking change.</span></span>

<span data-ttu-id="54569-197">鉴于也没有显著的支持删除`init`为二进制兼容的更改，它作出了使用modreq直接前进的选择。</span><span class="sxs-lookup"><span data-stu-id="54569-197">Given there was also no significant support for removing `init` to be a binary compatible change it made the choice of using modreq straight forward.</span></span>

### <a name="init-vs-initonly"></a><span data-ttu-id="54569-198">init vs. 只</span><span class="sxs-lookup"><span data-stu-id="54569-198">init vs. initonly</span></span>
<span data-ttu-id="54569-199">在我们的 LDM 会议上，有三种语法形式得到了重要的考虑：</span><span class="sxs-lookup"><span data-stu-id="54569-199">There were three syntax forms which got significant consideration during our LDM meeting:</span></span>

```cs
// 1. Use init 
int Option1 { get; init; }
// 2. Use init set
int Option2 { get; init set; }
// 3. Use initonly
int Option3 { get; initonly; }
```

<span data-ttu-id="54569-200">**分辨率**在 LDM 中，没有压倒性地青睐的语法。</span><span class="sxs-lookup"><span data-stu-id="54569-200">**Resolution** There was no syntax which was overwhelmingly favored in LDM.</span></span>

<span data-ttu-id="54569-201">备受关注的一点是，语法的选择将如何影响我们将来将成员作为一项常规功能`init`的能力。</span><span class="sxs-lookup"><span data-stu-id="54569-201">One point which got significant attention was how the choice of syntax would impact our ability to do `init` members as a general feature in the future.</span></span>
<span data-ttu-id="54569-202">选择选项 1 意味着将来很难定义具有`init`样式`get`方法的属性。</span><span class="sxs-lookup"><span data-stu-id="54569-202">Choosing option 1 would mean that it would be difficult to define a property which had a `init` style `get` method in the future.</span></span> <span data-ttu-id="54569-203">最终决定，如果我们决定在未来与一般`init`成员一起前进，我们可以允许`init`成为属性访问者列表中的修改者，以及 . `init set`</span><span class="sxs-lookup"><span data-stu-id="54569-203">Eventually it was decided that if we decided to go forward with general `init` members in future we could allow `init` to be a modifier in the property accessor list as well as a short hand for `init set`.</span></span> <span data-ttu-id="54569-204">基本上，以下两个声明将是相同的。</span><span class="sxs-lookup"><span data-stu-id="54569-204">Essentially the following two declarations would be identical.</span></span>

```cs
int Property1 { get; init; }
int Property1 { get; init set; }
```

<span data-ttu-id="54569-205">决定作为属性访问器列表中的独立访问`init`器前进。</span><span class="sxs-lookup"><span data-stu-id="54569-205">The decision was made to move forward with `init` as a standalone accessor in the property accessor list.</span></span>

### <a name="warn-on-failed-init"></a><span data-ttu-id="54569-206">警告失败</span><span class="sxs-lookup"><span data-stu-id="54569-206">Warn on failed init</span></span>
<span data-ttu-id="54569-207">请考虑以下场景。</span><span class="sxs-lookup"><span data-stu-id="54569-207">Consider the following scenario.</span></span> <span data-ttu-id="54569-208">类型声明构造函数中`init`未设置的唯一成员。</span><span class="sxs-lookup"><span data-stu-id="54569-208">A type declares an `init` only member which is not set in the constructor.</span></span> <span data-ttu-id="54569-209">构造对象的代码是否应收到警告，如果他们未能初始化该值？</span><span class="sxs-lookup"><span data-stu-id="54569-209">Should the code which constructs the object get a warning if they failed to initialize the value?</span></span>

<span data-ttu-id="54569-210">此时，很明显，该字段永远不会被设置，因此与有关未能初始化`private`数据的警告有许多相似之处。</span><span class="sxs-lookup"><span data-stu-id="54569-210">At that point it is clear the field will never be set and hence has a lot of similarities with the warning around failing to initialize `private` data.</span></span> <span data-ttu-id="54569-211">因此，警告似乎在这里会有一些价值？</span><span class="sxs-lookup"><span data-stu-id="54569-211">Hence a warning would seemingly have some value here?</span></span>

<span data-ttu-id="54569-212">不过，此警告有显著的缺点：</span><span class="sxs-lookup"><span data-stu-id="54569-212">There are significant downsides to this warning though:</span></span>
1. <span data-ttu-id="54569-213">它使更改为`readonly`的兼容性故事复杂化`init`。</span><span class="sxs-lookup"><span data-stu-id="54569-213">It complicates the compatibility story of changing `readonly` to `init`.</span></span> 
1. <span data-ttu-id="54569-214">它要求携带其他元数据来表示调用方需要初始化的成员。</span><span class="sxs-lookup"><span data-stu-id="54569-214">It requires carrying additional metadata around to denote the members which are required to be initialized by the caller.</span></span>

<span data-ttu-id="54569-215">此外，如果我们认为在迫使对象创建者被警告/错误有关特定字段的总体方案中有价值，那么这很可能作为一般功能有意义。</span><span class="sxs-lookup"><span data-stu-id="54569-215">Further if we believe there is value here in the overall scenario of forcing object creators to be warned / error'd about specific fields then this likely makes sense as a general feature.</span></span> <span data-ttu-id="54569-216">没有理由只局限于`init`成员。</span><span class="sxs-lookup"><span data-stu-id="54569-216">There is no reason it should be limited to just `init` members.</span></span>

<span data-ttu-id="54569-217">**分辨率**字段和属性的`init`消耗不会发出警告。</span><span class="sxs-lookup"><span data-stu-id="54569-217">**Resolution** There will be no warning on consumption of `init` fields and properties.</span></span>

<span data-ttu-id="54569-218">LDM 希望就所需字段和属性的概念进行更广泛的讨论。</span><span class="sxs-lookup"><span data-stu-id="54569-218">LDM wants to have a broader discussion on the idea of required fields and properties.</span></span> <span data-ttu-id="54569-219">这可能导致我们回来重新考虑我们对`init`成员和验证的立场。</span><span class="sxs-lookup"><span data-stu-id="54569-219">That may cause us to come back and reconsider our position on `init` members and validation.</span></span>

## <a name="allow-init-as-a-field-modifier"></a><span data-ttu-id="54569-220">允许以"以天为名"修改器</span><span class="sxs-lookup"><span data-stu-id="54569-220">Allow init as a field modifier</span></span>
<span data-ttu-id="54569-221">以同样的方式`init`也可以作为属性访问者，它也可以作为字段的指定，以赋予它们与`init`属性类似的行为。</span><span class="sxs-lookup"><span data-stu-id="54569-221">In the same way `init` can serve as a property accessor it could also serve as a designation on fields to give them similar behaviors as `init` properties.</span></span>
<span data-ttu-id="54569-222">这将允许在构造完成之前按类型、派生类型或对象初始化器分配字段。</span><span class="sxs-lookup"><span data-stu-id="54569-222">That would allow for the field to be assigned before construction was complete by the type, derived types, or object initializers.</span></span>

```cs
class Student
{
    public init string FirstName;
    public init string LastName;
}

var s = new Student()
{
    FirstName = "Jarde",
    LastName = "Parsons",
}

s.FirstName = "Jared"; // Error FirstName is readonly
```

<span data-ttu-id="54569-223">在元数据中，这些字段的标记方式与`readonly`字段相同，但使用附加属性或 modreq 来指示它们是`init`样式字段。</span><span class="sxs-lookup"><span data-stu-id="54569-223">In metadata these fields would be marked in the same way as `readonly` fields but with an additional attribute or modreq to indicate they are `init` style fields.</span></span> 

<span data-ttu-id="54569-224">**分辨率**LDM同意这一建议是健全的，但总体而言，情况感觉与属性脱节。</span><span class="sxs-lookup"><span data-stu-id="54569-224">**Resolution** LDM agrees this proposal is sound but overall the scenario felt disjoint from properties.</span></span> <span data-ttu-id="54569-225">决定目前只处理`init`属性。</span><span class="sxs-lookup"><span data-stu-id="54569-225">The decision was to proceed only with `init` properties for now.</span></span> <span data-ttu-id="54569-226">这具有适当的灵活性级别，`init`因为属性可以在属性的声明类型上更改`readonly`字段。</span><span class="sxs-lookup"><span data-stu-id="54569-226">This has a suitable level of flexibility as an `init` property can mutate a `readonly` field on the declaring type of the property.</span></span> <span data-ttu-id="54569-227">如果有重要的客户反馈证明方案是否有效，将重新考虑这一点。</span><span class="sxs-lookup"><span data-stu-id="54569-227">This will be reconsidered if there is significant customer feedback that justifies the scenario.</span></span>

### <a name="allow-init-as-a-type-modifier"></a><span data-ttu-id="54569-228">允许以类型修改器表示 init</span><span class="sxs-lookup"><span data-stu-id="54569-228">Allow init as a type modifier</span></span>
<span data-ttu-id="54569-229">`readonly`同样，`struct`修改器可以应用于 自动声明所有字段`readonly`为 ，唯一`init``struct`的修改器可以在 上声明或`class`自动将所有字段标记为 。 `init`</span><span class="sxs-lookup"><span data-stu-id="54569-229">In the same way the `readonly` modifier can be applied to a `struct` to automatically declare all fields as `readonly`, the `init` only modifier can be declared on a `struct` or `class` to automatically mark all fields as `init`.</span></span>
<span data-ttu-id="54569-230">这意味着以下两种类型声明等效：</span><span class="sxs-lookup"><span data-stu-id="54569-230">This means the following two type declarations are equivalent:</span></span>

```cs
struct Point
{
    public init int X;
    public init int Y;
}

// vs. 

init struct Point
{
    public int X;
    public int Y;
}
```

<span data-ttu-id="54569-231">**分辨率**此功能在这里太*可爱*了，`readonly struct`并且与它所基于的功能相冲突。</span><span class="sxs-lookup"><span data-stu-id="54569-231">**Resolution** This feature is too *cute* here and conflicts with the `readonly struct` feature on which it is based.</span></span> <span data-ttu-id="54569-232">该`readonly struct`功能很简单，因为它适用于`readonly`所有成员：字段、方法等...该`init struct`功能仅适用于属性。</span><span class="sxs-lookup"><span data-stu-id="54569-232">The `readonly struct` feature is simple in that it applies `readonly` to all members: fields, methods, etc ... The `init struct` feature would only apply to properties.</span></span> <span data-ttu-id="54569-233">这实际上最终使用户感到困惑。</span><span class="sxs-lookup"><span data-stu-id="54569-233">This actually ends up making it confusing for users.</span></span> 

<span data-ttu-id="54569-234">鉴于这`init`仅在类型的某些方面有效，我们拒绝了将其用作类型修饰符的想法。</span><span class="sxs-lookup"><span data-stu-id="54569-234">Given that `init` is only valid on certain aspects of a type we rejected the idea of having it as a type modifier.</span></span>

## <a name="considerations"></a><span data-ttu-id="54569-235">注意事项</span><span class="sxs-lookup"><span data-stu-id="54569-235">Considerations</span></span>

### <a name="compatibility"></a><span data-ttu-id="54569-236">兼容性</span><span class="sxs-lookup"><span data-stu-id="54569-236">Compatibility</span></span>
<span data-ttu-id="54569-237">该`init`功能设计为与现有的仅属性`get`兼容。</span><span class="sxs-lookup"><span data-stu-id="54569-237">The `init` feature is designed to be compatible with existing `get` only properties.</span></span> <span data-ttu-id="54569-238">具体来说，它意味着一个完全附加的更改属性，这是`get`今天，但希望更多的弹性对象创建语义。</span><span class="sxs-lookup"><span data-stu-id="54569-238">Specifically it is meant to be a completely additive change for a property which is `get` only today but desires more flexbile object creation semantics.</span></span>

<span data-ttu-id="54569-239">例如，请考虑以下类型：</span><span class="sxs-lookup"><span data-stu-id="54569-239">For example consider the following type:</span></span>

```cs
class Name
{
    public string First { get; }
    public string Last { get; }

    public Name(string first, string last)
    {
        First = first;
        Last = last;
    }
}
```

<span data-ttu-id="54569-240">添加到`init`这些属性是一个非突破性的更改：</span><span class="sxs-lookup"><span data-stu-id="54569-240">Adding `init` to these properties is a non-breaking change:</span></span>

```cs
class Name
{
    public string First { get; init; }
    public string Last { get; init; }

    public Name(string first, string last)
    {
        First = first;
        Last = last;
    }
}
```

### <a name="il-verification"></a><span data-ttu-id="54569-241">IL 验证</span><span class="sxs-lookup"><span data-stu-id="54569-241">IL verification</span></span>
<span data-ttu-id="54569-242">当 .NET Core 决定重新实施 IL 验证时，需要调整规则以考虑`init`成员。</span><span class="sxs-lookup"><span data-stu-id="54569-242">When .NET Core decides to re-implement IL verification the rules will need to be adjusted to account for `init` members.</span></span> <span data-ttu-id="54569-243">这将需要包含在规则更改中，以便非对`readonly`数据的更改。</span><span class="sxs-lookup"><span data-stu-id="54569-243">This will need to be included in the rule changes for non-mutating acess to `readonly` data.</span></span>

<span data-ttu-id="54569-244">IL 验证规则需要分为两部分：</span><span class="sxs-lookup"><span data-stu-id="54569-244">The IL verification rules will need to be broken into two parts:</span></span> 

1. <span data-ttu-id="54569-245">允许`init`成员设置`readonly`字段。</span><span class="sxs-lookup"><span data-stu-id="54569-245">Allowing `init` members to set a `readonly` field.</span></span>
1. <span data-ttu-id="54569-246">确定何时可以`init`合法地调用成员。</span><span class="sxs-lookup"><span data-stu-id="54569-246">Determining when an `init` member can be legally called.</span></span>

<span data-ttu-id="54569-247">第一种是对现有规则的简单调整。</span><span class="sxs-lookup"><span data-stu-id="54569-247">The first is a simple adjustment to the existing rules.</span></span> <span data-ttu-id="54569-248">IL 验证器可以被教导识别`init`成员，从那里它只需要考虑在这样的成员`readonly``this`中设置一个字段。</span><span class="sxs-lookup"><span data-stu-id="54569-248">The IL verifier can be taught to recognize `init` members and from there it just needs to consider a `readonly` field to be settable on `this` in such a member.</span></span>

<span data-ttu-id="54569-249">第二个规则更为复杂。</span><span class="sxs-lookup"><span data-stu-id="54569-249">The second rule is more complicated.</span></span> <span data-ttu-id="54569-250">在对象初始化的简单情况下，规则是直截了当的。</span><span class="sxs-lookup"><span data-stu-id="54569-250">In the simple case of object initializers the rule is straight forward.</span></span> <span data-ttu-id="54569-251">当表达式的结果仍在堆栈`init`上时，`new`调用成员应该是合法的。</span><span class="sxs-lookup"><span data-stu-id="54569-251">It should be legal to call `init` members when the result of a `new` expression is still on the stack.</span></span> <span data-ttu-id="54569-252">也就是说，直到该值存储在本地、数组元素或字段中，或作为参数传递给另一种方法，调用`init`成员仍然是合法的。</span><span class="sxs-lookup"><span data-stu-id="54569-252">That is until the value has been stored in a local, array element or field or passed as an argument to another method it will still be legal to call `init` members.</span></span> <span data-ttu-id="54569-253">这可确保一旦`new`表达式的结果发布到命名标识符（而不是`this`），则调用`init`成员将不再合法。</span><span class="sxs-lookup"><span data-stu-id="54569-253">This ensures that once the result of the `new` expression is published to a named identifier (other than `this`) then it will no longer be legal to call `init` members.</span></span> 

<span data-ttu-id="54569-254">但更复杂的情况是，当我们混合`init`成员、对象初始化器和`await`时。</span><span class="sxs-lookup"><span data-stu-id="54569-254">The more complicated case though is when we mix `init` members, object initializers and `await`.</span></span> <span data-ttu-id="54569-255">这可能会导致新创建的对象暂时被吊入状态机，从而放入字段中。</span><span class="sxs-lookup"><span data-stu-id="54569-255">That can cause the newly created object to be temporarily hoisted into a state machine and hence put into a field.</span></span>

```cs
var student = new Student() 
{
    Name = await SomeMethod()
};
```

<span data-ttu-id="54569-256">在这里，结果`new Student()`将作为字段在`Name`发生之前被分到状态机中。</span><span class="sxs-lookup"><span data-stu-id="54569-256">Here the result of `new Student()` will be hoised into a state machine as a field before the set of `Name` occurs.</span></span> <span data-ttu-id="54569-257">编译器需要以 IL 验证器了解它们不可访问的方式标记这些提升字段，因此不违反`init`的预期语义。</span><span class="sxs-lookup"><span data-stu-id="54569-257">The compiler will need to mark such hoisted fields in a way that the IL verifier understands they're not user accessible and hence doesn't violate the intended semantics of `init`.</span></span>

### <a name="init-members"></a><span data-ttu-id="54569-258">init 成员</span><span class="sxs-lookup"><span data-stu-id="54569-258">init members</span></span>
<span data-ttu-id="54569-259">修改`init`器可以扩展到所有实例成员。</span><span class="sxs-lookup"><span data-stu-id="54569-259">The `init` modifier could be extended to apply to all instance members.</span></span> <span data-ttu-id="54569-260">这将概括对象构造期间的概`init`念，并允许类型声明可在构造过程中分段的帮助器方法初始化`init`字段和属性。</span><span class="sxs-lookup"><span data-stu-id="54569-260">This would generalize the concept of `init` during object construction and allow types to declare helper methods that could partipate in the construction process to initialize `init` fields and properties.</span></span>

<span data-ttu-id="54569-261">此类成员将具有`init`访问器在此设计中所做的所有休息。</span><span class="sxs-lookup"><span data-stu-id="54569-261">Such members would have all the restricions that an `init` accessor does in this design.</span></span> <span data-ttu-id="54569-262">需求是值得怀疑的，但是，这可以安全地添加到未来的语言版本，以兼容的方式。</span><span class="sxs-lookup"><span data-stu-id="54569-262">The need is questionable though and this can be safely added in a future version of the language in a compatible manner.</span></span>

### <a name="generate-three-accessors"></a><span data-ttu-id="54569-263">生成三个访问器</span><span class="sxs-lookup"><span data-stu-id="54569-263">Generate three accessors</span></span>
<span data-ttu-id="54569-264">属性的一`init`个潜在实现是完全`init`独立于`set`。</span><span class="sxs-lookup"><span data-stu-id="54569-264">One potential implementation of `init` properties is to make `init` completely separate from `set`.</span></span> <span data-ttu-id="54569-265">这意味着属性可能具有三个不同的访问器：`get`和`set``init`。</span><span class="sxs-lookup"><span data-stu-id="54569-265">That means that a property can potentially have three different accessors: `get`, `set` and `init`.</span></span>

<span data-ttu-id="54569-266">这具有允许使用 modreq 来强制正确性，同时保持二进制兼容性的潜在优势。</span><span class="sxs-lookup"><span data-stu-id="54569-266">This has the potential advantage of allowing the use of modreq to enforce correctness while maintaining binary compatibility.</span></span> <span data-ttu-id="54569-267">实现大致如下：</span><span class="sxs-lookup"><span data-stu-id="54569-267">The implementation would roughly be the following:</span></span>

1. <span data-ttu-id="54569-268">如果存在`init` `set`，则始终发出访问器。</span><span class="sxs-lookup"><span data-stu-id="54569-268">An `init` accessor is always emitted if there is a `set`.</span></span> <span data-ttu-id="54569-269">当开发人员未定义时，它只是对`set`的引用。</span><span class="sxs-lookup"><span data-stu-id="54569-269">When not defined by the developer it is simply a reference to `set`.</span></span> 
1. <span data-ttu-id="54569-270">对象初始化器中的属性集将始终使用`init`（如果存在，但如果丢失则回退）。 `set`</span><span class="sxs-lookup"><span data-stu-id="54569-270">The set of a property in an object initializer will always use `init` if present but fall back to `set` if it's missing.</span></span>

<span data-ttu-id="54569-271">这意味着开发人员始终可以安全地从属性中删除`init`。</span><span class="sxs-lookup"><span data-stu-id="54569-271">This means that a developer can always safely delete `init` from a property.</span></span> 

<span data-ttu-id="54569-272">此设计的缺点是，只有在`init`始终发出时，才有用。 **always** `set`</span><span class="sxs-lookup"><span data-stu-id="54569-272">The downside of this design is that is only useful if `init` is **always** emitted when there is a `set`.</span></span> <span data-ttu-id="54569-273">语言不能知道是否`init`删除了过去，它必须假设它是，`init`因此必须始终发出。</span><span class="sxs-lookup"><span data-stu-id="54569-273">The language can't know if `init` was deleted in the past, it has to assume it was and hence the `init` must always be emitted.</span></span> <span data-ttu-id="54569-274">这将导致大量的元数据扩展，并且根本不值得在这里的兼容性成本。</span><span class="sxs-lookup"><span data-stu-id="54569-274">That would cause a significant metadata expansion and is simply not worth the cost of the compatibility here.</span></span>
