---
ms.openlocfilehash: 8da0f989669c77f724b5369722da3fcc944c348e
ms.sourcegitcommit: ab0873759f86d44adfc5daefb18cb922df8adb8b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/27/2020
ms.locfileid: "82162095"
---
<a name="init-only-setters"></a><span data-ttu-id="3a374-101">仅 Init 资源库</span><span class="sxs-lookup"><span data-stu-id="3a374-101">Init Only Setters</span></span>
=====

## <a name="summary"></a><span data-ttu-id="3a374-102">摘要</span><span class="sxs-lookup"><span data-stu-id="3a374-102">Summary</span></span>
<span data-ttu-id="3a374-103">此建议将仅 init 属性和索引器的概念添加到 c #。</span><span class="sxs-lookup"><span data-stu-id="3a374-103">This proposal adds the concept of init only properties and indexers to C#.</span></span> <span data-ttu-id="3a374-104">这些属性和索引器可以在创建对象时设置，但仅在对象`get`创建完成后才会有效。</span><span class="sxs-lookup"><span data-stu-id="3a374-104">These properties and indexers can be set at the point of object creation but become effectively `get` only once object creation has completed.</span></span>
<span data-ttu-id="3a374-105">这允许在 c # 中使用更灵活的不可变模型。</span><span class="sxs-lookup"><span data-stu-id="3a374-105">This allows for a much more flexible immutable model in C#.</span></span> 

## <a name="motivation"></a><span data-ttu-id="3a374-106">动机</span><span class="sxs-lookup"><span data-stu-id="3a374-106">Motivation</span></span>
<span data-ttu-id="3a374-107">自1.0 以来，用 c # 生成不可变数据的基础机制并未更改。</span><span class="sxs-lookup"><span data-stu-id="3a374-107">The underlying mechanisms for building immutable data in C# haven't changed since 1.0.</span></span> <span data-ttu-id="3a374-108">它们仍保留：</span><span class="sxs-lookup"><span data-stu-id="3a374-108">They remain:</span></span>

1. <span data-ttu-id="3a374-109">将字段声明`readonly`为。</span><span class="sxs-lookup"><span data-stu-id="3a374-109">Declaring fields as `readonly`.</span></span>
1. <span data-ttu-id="3a374-110">声明仅包含`get`访问器的属性。</span><span class="sxs-lookup"><span data-stu-id="3a374-110">Declaring properties that contain only a `get` accessor.</span></span>

<span data-ttu-id="3a374-111">这些机制在允许构造不可变的数据时有效，但它们通过向类型的样板代码添加成本并从对象和集合初始值设定项的功能中选择此类类型来实现此目的。</span><span class="sxs-lookup"><span data-stu-id="3a374-111">These mechanisms are effective at allowing the construction of immutable data but they do so by adding cost to the boilerplate code of types and opting such types out of features like object and collection initializers.</span></span> <span data-ttu-id="3a374-112">这意味着开发人员必须在易用性和不可变性之间进行选择。</span><span class="sxs-lookup"><span data-stu-id="3a374-112">This means developers must choose between ease of use and immutability.</span></span>

<span data-ttu-id="3a374-113">像声明类型一样， `Point`简单的不可变对象（例如）需要两个样板板代码，以支持构造。</span><span class="sxs-lookup"><span data-stu-id="3a374-113">A simple immutable object like `Point` requires twice as much boiler plate code to support construction as it does to declare the type.</span></span> <span data-ttu-id="3a374-114">类型越大，此样板板式的成本就越大：</span><span class="sxs-lookup"><span data-stu-id="3a374-114">The bigger the type the bigger the cost of this boiler plate:</span></span>

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

<span data-ttu-id="3a374-115">通过`init`允许调用方在构造操作过程中改变成员，访问器使不可变对象更具灵活性。</span><span class="sxs-lookup"><span data-stu-id="3a374-115">The `init` accessor makes immutable objects more flexible by allowing the caller to mutate the members during the act of construction.</span></span> <span data-ttu-id="3a374-116">这意味着对象的不可变属性可以参与对象初始值设定项，因此不再需要类型中的所有构造函数样板。</span><span class="sxs-lookup"><span data-stu-id="3a374-116">That means the object's immutable properties can participate in object initializers and thus removes the need for all constructor boilerplate in the type.</span></span> <span data-ttu-id="3a374-117">`Point`类型现在只是：</span><span class="sxs-lookup"><span data-stu-id="3a374-117">The `Point` type is now simply:</span></span>

```cs
struct Point
{
    public int X { get; init; }
    public int Y { get; init; }
}
```

<span data-ttu-id="3a374-118">然后，使用者可以使用对象初始值设定项来创建对象。</span><span class="sxs-lookup"><span data-stu-id="3a374-118">The consumer can then use object initializers to create the object</span></span>

```cs
var p = new Point() { X = 42, Y = 13 };
```

## <a name="detailed-design"></a><span data-ttu-id="3a374-119">详细设计</span><span class="sxs-lookup"><span data-stu-id="3a374-119">Detailed Design</span></span>

### <a name="init-accessors"></a><span data-ttu-id="3a374-120">init 访问器</span><span class="sxs-lookup"><span data-stu-id="3a374-120">init accessors</span></span>
<span data-ttu-id="3a374-121">Init only 属性（或索引器）是使用`init`访问器替代`set`访问器声明的：</span><span class="sxs-lookup"><span data-stu-id="3a374-121">An init only property (or indexer) is declared by using the `init` accessor in place of the `set` accessor:</span></span>

```cs
class Student
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
}
```

<span data-ttu-id="3a374-122">在以下情况下， `init`包含访问器的实例属性被认为是可设置的：</span><span class="sxs-lookup"><span data-stu-id="3a374-122">An instance property containing an `init` accessor is considered settable in the following circumstances:</span></span>

- <span data-ttu-id="3a374-123">在对象初始值设定项期间</span><span class="sxs-lookup"><span data-stu-id="3a374-123">During an object initializer</span></span>
- <span data-ttu-id="3a374-124">`with`表达式初始值设定项期间</span><span class="sxs-lookup"><span data-stu-id="3a374-124">During a `with` expression initializer</span></span>
- <span data-ttu-id="3a374-125">在或上`this`包含类型或派生类型的实例构造函数中`base`</span><span class="sxs-lookup"><span data-stu-id="3a374-125">Inside an instance constructor of the containing or derived type, on `this` or `base`</span></span>
- <span data-ttu-id="3a374-126">在任何`init`属性的访问器中、 `this`在或上`base`</span><span class="sxs-lookup"><span data-stu-id="3a374-126">Inside the `init` accessor of any property, on `this` or `base`</span></span>

<span data-ttu-id="3a374-127">在此文档中，可`init`设置访问器的时间统称为对象的构造阶段。</span><span class="sxs-lookup"><span data-stu-id="3a374-127">The times above in which the `init` accessors are settable are collectively referred to in this document as the construction phase of the object.</span></span>

<span data-ttu-id="3a374-128">这意味着， `Student`可以通过以下方式使用类：</span><span class="sxs-lookup"><span data-stu-id="3a374-128">This means the `Student` class can be used in the following ways:</span></span>

```cs
var s = new Student()
{
    FirstName = "Jared",
    LastName = "Parosns",
};
s.LastName = "Parsons"; // Error: LastName is not settable
```

<span data-ttu-id="3a374-129">`init`访问器可设置的规则围绕类型层次结构扩展。</span><span class="sxs-lookup"><span data-stu-id="3a374-129">The rules around when `init` accessors are settable extend across type hierarchies.</span></span> <span data-ttu-id="3a374-130">如果该成员是可访问的，并且已知该对象处于构造阶段，则该成员是可设置的。</span><span class="sxs-lookup"><span data-stu-id="3a374-130">If the member is accessible and the object is known to be in the construction phase then the member is settable.</span></span> <span data-ttu-id="3a374-131">这就是：</span><span class="sxs-lookup"><span data-stu-id="3a374-131">That specifically allows for the following:</span></span>

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

<span data-ttu-id="3a374-132">在调用`init`访问器时，实例已知处于开放构造阶段。</span><span class="sxs-lookup"><span data-stu-id="3a374-132">At the point a `init` accessor is invoked the instance is known to be in the open construction phase.</span></span> <span data-ttu-id="3a374-133">因此， `init`除了一般`set`访问器可以执行的操作外，还允许访问器执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="3a374-133">Hence an `init` accessor is allowed to take the following actions in addition to what a normal `set` accessor can do:</span></span>

1. <span data-ttu-id="3a374-134">调用通过`init` `this`或提供的其他访问器`base`</span><span class="sxs-lookup"><span data-stu-id="3a374-134">Call other `init` accessors available through `this` or `base`</span></span>
1. <span data-ttu-id="3a374-135">通过`readonly`指定在同一类型中声明的字段`this`</span><span class="sxs-lookup"><span data-stu-id="3a374-135">Assign `readonly` fields declared on the same type through `this`</span></span>

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

<span data-ttu-id="3a374-136">将字段从`init`访问`readonly`器分配的能力限制为与访问器在同一类型上声明的那些字段。</span><span class="sxs-lookup"><span data-stu-id="3a374-136">The ability to assign `readonly` fields from an `init` accessor is limited to those fields declared on the same type as the accessor.</span></span> <span data-ttu-id="3a374-137">它不能用于分配`readonly`基类型中的字段。</span><span class="sxs-lookup"><span data-stu-id="3a374-137">It cannot be used to assign `readonly` fields in a base type.</span></span> <span data-ttu-id="3a374-138">此规则可确保类型作者保持对其类型的可变性行为的控制。</span><span class="sxs-lookup"><span data-stu-id="3a374-138">This rule ensures that type authors remain in control over the mutability behavior of their type.</span></span> <span data-ttu-id="3a374-139">不希望利用`init`的开发人员不会受到其他类型的影响，因为选择这样做：</span><span class="sxs-lookup"><span data-stu-id="3a374-139">Developers who do not wish to utilize `init` cannot be impacted from other types choosing to do so:</span></span>

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

<span data-ttu-id="3a374-140">当`init`在虚拟属性中使用时，所有重写也必须标记为`init`。</span><span class="sxs-lookup"><span data-stu-id="3a374-140">When `init` is used in a virtual property then all the overrides must also be marked as `init`.</span></span> <span data-ttu-id="3a374-141">同样，不能使用`set` `init`重写的简单。</span><span class="sxs-lookup"><span data-stu-id="3a374-141">Likewise it is not possible to override a simple `set` with `init`.</span></span>

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

<span data-ttu-id="3a374-142">`interface`声明还可以通过以下模式`init`参与样式初始化：</span><span class="sxs-lookup"><span data-stu-id="3a374-142">An `interface` declaration can also participate in `init` style initialization via the following pattern:</span></span>

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

<span data-ttu-id="3a374-143">此功能的限制：</span><span class="sxs-lookup"><span data-stu-id="3a374-143">Restrictions of this feature:</span></span>
- <span data-ttu-id="3a374-144">`init`访问器只能用于实例属性</span><span class="sxs-lookup"><span data-stu-id="3a374-144">The `init` accessor can only be used on instance properties</span></span>
- <span data-ttu-id="3a374-145">属性不能同时包含`init`和`set`访问器</span><span class="sxs-lookup"><span data-stu-id="3a374-145">A property cannot contain both an `init` and `set` accessor</span></span>
- <span data-ttu-id="3a374-146">如果基具有`init`，则属性的`init`所有重写都必须具有。</span><span class="sxs-lookup"><span data-stu-id="3a374-146">All overrides of a property must have `init` if the base had `init`.</span></span> <span data-ttu-id="3a374-147">此规则也适用于接口实现。</span><span class="sxs-lookup"><span data-stu-id="3a374-147">This rule also applies to interface implementation.</span></span>

### <a name="metadata-encoding"></a><span data-ttu-id="3a374-148">元数据编码</span><span class="sxs-lookup"><span data-stu-id="3a374-148">Metadata encoding</span></span> 
<span data-ttu-id="3a374-149">属性`init`访问器将作为标准`set`访问器发出，并使用标记有 modreq 的`IsExternalInit`返回类型。</span><span class="sxs-lookup"><span data-stu-id="3a374-149">Property `init` accessors will be emitted as a standard `set` accessor with the return type marked with a modreq of `IsExternalInit`.</span></span> <span data-ttu-id="3a374-150">这是一种新类型，它具有以下定义：</span><span class="sxs-lookup"><span data-stu-id="3a374-150">This is a new type which will have the following definition:</span></span>

```cs
namespace System.Runtime.CompilerServices
{
    public sealed class IsExternalInit
    {
    }
}
```

<span data-ttu-id="3a374-151">编译器将按全名匹配类型。</span><span class="sxs-lookup"><span data-stu-id="3a374-151">The compiler will match the type by full name.</span></span> <span data-ttu-id="3a374-152">它不会在核心库中显示。</span><span class="sxs-lookup"><span data-stu-id="3a374-152">There is no requirement that it appear in the core library.</span></span> <span data-ttu-id="3a374-153">如果有多个使用此名称的类型，则编译器将按以下顺序关联中断：</span><span class="sxs-lookup"><span data-stu-id="3a374-153">If there are multiple types by this name then the compiler will tie break in the following order:</span></span>

1. <span data-ttu-id="3a374-154">正在编译的项目中定义的项</span><span class="sxs-lookup"><span data-stu-id="3a374-154">The one defined in the project being compiled</span></span>
1. <span data-ttu-id="3a374-155">System.private.corelib 中定义的</span><span class="sxs-lookup"><span data-stu-id="3a374-155">The one defined in corelib</span></span>

<span data-ttu-id="3a374-156">如果这些都不存在，则将发出类型多义性错误。</span><span class="sxs-lookup"><span data-stu-id="3a374-156">If neither of these exist then a type ambiguity error will be issued.</span></span>

<span data-ttu-id="3a374-157">[此问题](https://github.com/dotnet/runtime/issues/34978)中`IsExternalInit`进一步介绍了的设计</span><span class="sxs-lookup"><span data-stu-id="3a374-157">The design for `IsExternalInit` is futher covered in [this issue](https://github.com/dotnet/runtime/issues/34978)</span></span>

## <a name="questions"></a><span data-ttu-id="3a374-158">问题</span><span class="sxs-lookup"><span data-stu-id="3a374-158">Questions</span></span>

### <a name="breaking-changes"></a><span data-ttu-id="3a374-159">中断性变更</span><span class="sxs-lookup"><span data-stu-id="3a374-159">Breaking changes</span></span>
<span data-ttu-id="3a374-160">编码此功能的方式的主要透视点之一将会导致以下问题：</span><span class="sxs-lookup"><span data-stu-id="3a374-160">One of the main pivot points in how this feature is encoded will come down to the following question:</span></span> 

> <span data-ttu-id="3a374-161">它是否是要替换`init`的`set`二进制重大更改？</span><span class="sxs-lookup"><span data-stu-id="3a374-161">Is it a binary breaking change to replace `init` with `set`?</span></span>

<span data-ttu-id="3a374-162">将`init`替换`set`为，并使属性完全可写，而不会对非虚拟属性进行源重大更改。</span><span class="sxs-lookup"><span data-stu-id="3a374-162">Replacing `init` with `set` and thus making a property fully writable is never a source breaking change on a non-virtual property.</span></span> <span data-ttu-id="3a374-163">它只是扩展可写入属性的方案集。</span><span class="sxs-lookup"><span data-stu-id="3a374-163">It simply expands the set of scenarios where the property can be written.</span></span> <span data-ttu-id="3a374-164">相关的唯一行为是，这是否仍是二进制的重大更改。</span><span class="sxs-lookup"><span data-stu-id="3a374-164">The only behavior in question is whether or not this remains a binary breaking change.</span></span>

<span data-ttu-id="3a374-165">如果我们想要将更改`init`为`set`源和二进制兼容的更改，则会强制我们在下面的 modreq 与属性决策上做出选择，因为它会将 modreqs 排除为 soulution。</span><span class="sxs-lookup"><span data-stu-id="3a374-165">If we want to make the change of `init` to `set` a source and binary compatible change then it will force our hand on the modreq vs. attributes decision below because it will rule out modreqs as a soulution.</span></span> <span data-ttu-id="3a374-166">另一方面，这会被视为一种不感兴趣的做法，这将使 modreq 与属性决策更少有影响力。</span><span class="sxs-lookup"><span data-stu-id="3a374-166">If on the other hand this is seen as a non-interesting then this will make the modreq vs. attribute decision less impactful.</span></span>

<span data-ttu-id="3a374-167">**解决方法**LDM 不会出现这种情况。</span><span class="sxs-lookup"><span data-stu-id="3a374-167">**Resolution** This scenario is not seen as compelling by LDM.</span></span>

### <a name="modreqs-vs-attributes"></a><span data-ttu-id="3a374-168">Modreqs 与属性</span><span class="sxs-lookup"><span data-stu-id="3a374-168">Modreqs vs. attributes</span></span>
<span data-ttu-id="3a374-169">在元数据过程`init`中发出时，属性访问器的发出策略必须在使用属性或 modreqs 之间进行选择。</span><span class="sxs-lookup"><span data-stu-id="3a374-169">The emit strategy for `init` property accessors must choose between using attributes or modreqs when emitting during metadata.</span></span> <span data-ttu-id="3a374-170">它们具有不同的权衡要求。</span><span class="sxs-lookup"><span data-stu-id="3a374-170">These have different trade offs that need to be considered.</span></span>

<span data-ttu-id="3a374-171">使用 modreq 声明为属性集访问器批注意味着 CLI 兼容编译器将忽略访问器，除非它了解 modreq。</span><span class="sxs-lookup"><span data-stu-id="3a374-171">Annotating a property set accessor with a modreq declaration means CLI compliant compilers will ignore the accessor unless it understands the modreq.</span></span> <span data-ttu-id="3a374-172">这意味着仅识别的`init`编译器将读取成员。</span><span class="sxs-lookup"><span data-stu-id="3a374-172">That means only compilers aware of `init` will read the member.</span></span> <span data-ttu-id="3a374-173">不识别的`init`编译器会忽略`set`访问器，因此不会意外地将属性视为读/写。</span><span class="sxs-lookup"><span data-stu-id="3a374-173">Compilers unaware of `init` will ignore the `set` accessor and hence will not accidentally treat the property as read / write.</span></span> 

<span data-ttu-id="3a374-174">Modreq 的缺点`init`成为`set`访问器的二进制签名的一部分。</span><span class="sxs-lookup"><span data-stu-id="3a374-174">The downside of modreq is `init` becomes a part of the binary signature of the `set` accessor.</span></span> <span data-ttu-id="3a374-175">添加或删除`init`会破坏应用程序的二进制 compatbility。</span><span class="sxs-lookup"><span data-stu-id="3a374-175">Adding or removing `init` will break binary compatbility of the application.</span></span>

<span data-ttu-id="3a374-176">使用特性来批注`set`访问器意味着只有了解该属性的编译器知道它会限制对它的访问。</span><span class="sxs-lookup"><span data-stu-id="3a374-176">Using attributes to annotate the `set` accessor means that only compilers which understand the attribute will know to limit access to it.</span></span> <span data-ttu-id="3a374-177">不知道的`init`编译器会将其视为简单的读/写属性，并允许访问。</span><span class="sxs-lookup"><span data-stu-id="3a374-177">A compiler unaware of `init` will see it as a simple read / write property and allow access.</span></span>

<span data-ttu-id="3a374-178">这似乎表明，这一决定是在额外安全方面做出的选择，但代价是在二进制兼容性的代价。</span><span class="sxs-lookup"><span data-stu-id="3a374-178">This would seemingly mean this decision is a choice between extra safety at the expense of binary compatibility.</span></span> <span data-ttu-id="3a374-179">深入，额外的安全性并不是它的真正含义。</span><span class="sxs-lookup"><span data-stu-id="3a374-179">Digging in a bit the extra safety is not exactly what it seems.</span></span> <span data-ttu-id="3a374-180">它不会在以下情况下受到保护：</span><span class="sxs-lookup"><span data-stu-id="3a374-180">It will not protect against the following circumstances:</span></span>

1. <span data-ttu-id="3a374-181">`public`成员反射</span><span class="sxs-lookup"><span data-stu-id="3a374-181">Reflection over `public` members</span></span>
1. <span data-ttu-id="3a374-182">使用`dynamic`</span><span class="sxs-lookup"><span data-stu-id="3a374-182">The use of `dynamic`</span></span> 
1. <span data-ttu-id="3a374-183">不识别 modreqs 的编译器</span><span class="sxs-lookup"><span data-stu-id="3a374-183">Compilers that don't recognize modreqs</span></span>

<span data-ttu-id="3a374-184">还应考虑到，当我们完成 .NET 5 的 IL 验证规则时， `init`它将是这些规则之一。</span><span class="sxs-lookup"><span data-stu-id="3a374-184">It should also be considered that, when we complete the IL verification rules for .NET 5, `init` will be one of those rules.</span></span> <span data-ttu-id="3a374-185">这意味着只需验证发出可验证 IL 的编译器即可获得额外的强制。</span><span class="sxs-lookup"><span data-stu-id="3a374-185">That means extra enforcement will be gained from simply verifying compilers emitting verifiable IL.</span></span>

<span data-ttu-id="3a374-186">.NET 的主要语言（c #、F # 和 VB）都将更新，以识别这些`init`访问器。</span><span class="sxs-lookup"><span data-stu-id="3a374-186">The primary languages for .NET (C#, F# and VB) will all be updated to recognize these `init` accessors.</span></span> <span data-ttu-id="3a374-187">因此，这种情况下唯一现实的方案是： c `init` # 9 编译器发出属性，这些属性由较旧的工具集（如 c # 8、VB 15 等）发现。C # 8。</span><span class="sxs-lookup"><span data-stu-id="3a374-187">Hence the only realistic scenario here is when a C# 9 compiler emits `init` properties and they are seen by an older toolset such as C# 8, VB 15, etc ... C# 8.</span></span> <span data-ttu-id="3a374-188">这就是要考虑和权衡二进制兼容性的权衡。</span><span class="sxs-lookup"><span data-stu-id="3a374-188">That is the trade off to consider and weigh against binary compatibility.</span></span>

<span data-ttu-id="3a374-189">**注意**本讨论主要适用于成员，不适用于字段。</span><span class="sxs-lookup"><span data-stu-id="3a374-189">**Note** This discussion primarily applies to members only, not to fields.</span></span> <span data-ttu-id="3a374-190">尽管`init`字段被 LDM 拒绝，但对于 modreq 与属性讨论仍有兴趣。</span><span class="sxs-lookup"><span data-stu-id="3a374-190">While `init` fields were rejected by LDM they are still interesting to consider for the modreq vs. attribute discussion.</span></span> <span data-ttu-id="3a374-191">字段`init`的功能是现有限制的 relaxation `readonly`。</span><span class="sxs-lookup"><span data-stu-id="3a374-191">The `init` feature for fields is a relaxation of the existing restriction of `readonly`.</span></span> <span data-ttu-id="3a374-192">这意味着，如果将这些字段作为`readonly` + a 属性发出，则较早的编译器不会出现错误，因为它们已被识别`readonly`。</span><span class="sxs-lookup"><span data-stu-id="3a374-192">That means if we emit the fields as `readonly` + an attribute there is no risk of older compilers mis-using the field because they would already recognize `readonly`.</span></span> <span data-ttu-id="3a374-193">因此，在这里使用 modreq 不会添加任何额外的保护。</span><span class="sxs-lookup"><span data-stu-id="3a374-193">Hence using a modreq here doesn't add any extra protection.</span></span>

<span data-ttu-id="3a374-194">**解决方法**此功能将使用 modreq 对属性`init` setter 进行编码。</span><span class="sxs-lookup"><span data-stu-id="3a374-194">**Resolution** The feature will use a modreq to encode the property `init` setter.</span></span> <span data-ttu-id="3a374-195">有说服力的因素是（无特定顺序）：</span><span class="sxs-lookup"><span data-stu-id="3a374-195">The compelling factors were (in no particular order):</span></span>

* <span data-ttu-id="3a374-196">希望防止旧的编译器违反`init`语义</span><span class="sxs-lookup"><span data-stu-id="3a374-196">Desire to discourage older compilers from violating `init` semantics</span></span>
* <span data-ttu-id="3a374-197">希望在`init` `virtual`声明中添加或删除，或者`interface`同时执行源和二进制的重大更改。</span><span class="sxs-lookup"><span data-stu-id="3a374-197">Desire to make adding or removing `init` in a `virtual` declaration or `interface` both a source and binary breaking change.</span></span>

<span data-ttu-id="3a374-198">如果对删除`init`是二进制兼容更改，也不会有重要支持，则可以选择直接使用 modreq。</span><span class="sxs-lookup"><span data-stu-id="3a374-198">Given there was also no significant support for removing `init` to be a binary compatible change it made the choice of using modreq straight forward.</span></span>

### <a name="init-vs-initonly"></a><span data-ttu-id="3a374-199">init 与 initonly</span><span class="sxs-lookup"><span data-stu-id="3a374-199">init vs. initonly</span></span>
<span data-ttu-id="3a374-200">有三种语法形式，可在我们的 LDM 会议中获得重要的注意事项：</span><span class="sxs-lookup"><span data-stu-id="3a374-200">There were three syntax forms which got significant consideration during our LDM meeting:</span></span>

```cs
// 1. Use init 
int Option1 { get; init; }
// 2. Use init set
int Option2 { get; init set; }
// 3. Use initonly
int Option3 { get; initonly; }
```

<span data-ttu-id="3a374-201">**解决方法**LDM 中没有充分的语法。</span><span class="sxs-lookup"><span data-stu-id="3a374-201">**Resolution** There was no syntax which was overwhelmingly favored in LDM.</span></span>

<span data-ttu-id="3a374-202">需要注意的一点是，对语法的选择如何影响我们在将来作为一般功能`init`执行成员的能力。</span><span class="sxs-lookup"><span data-stu-id="3a374-202">One point which got significant attention was how the choice of syntax would impact our ability to do `init` members as a general feature in the future.</span></span>
<span data-ttu-id="3a374-203">如果选择选项1，则意味着在将来定义具有`init`样式`get`方法的属性是很困难的。</span><span class="sxs-lookup"><span data-stu-id="3a374-203">Choosing option 1 would mean that it would be difficult to define a property which had a `init` style `get` method in the future.</span></span> <span data-ttu-id="3a374-204">最终，如果我们决定以后继续使用常规`init`成员，我们就可以允许`init`在属性访问器列表中提供修饰符，为提供`init set`一个简短的修饰符。</span><span class="sxs-lookup"><span data-stu-id="3a374-204">Eventually it was decided that if we decided to go forward with general `init` members in future we could allow `init` to be a modifier in the property accessor list as well as a short hand for `init set`.</span></span> <span data-ttu-id="3a374-205">实质上，下面两个声明是相同的。</span><span class="sxs-lookup"><span data-stu-id="3a374-205">Essentially the following two declarations would be identical.</span></span>

```cs
int Property1 { get; init; }
int Property1 { get; init set; }
```

<span data-ttu-id="3a374-206">在属性访问器列表中，决定`init`以独立访问器的形式继续。</span><span class="sxs-lookup"><span data-stu-id="3a374-206">The decision was made to move forward with `init` as a standalone accessor in the property accessor list.</span></span>

### <a name="warn-on-failed-init"></a><span data-ttu-id="3a374-207">初始化失败时发出警告</span><span class="sxs-lookup"><span data-stu-id="3a374-207">Warn on failed init</span></span>
<span data-ttu-id="3a374-208">请考虑以下场景。</span><span class="sxs-lookup"><span data-stu-id="3a374-208">Consider the following scenario.</span></span> <span data-ttu-id="3a374-209">类型声明的`init`唯一成员未在构造函数中设置。</span><span class="sxs-lookup"><span data-stu-id="3a374-209">A type declares an `init` only member which is not set in the constructor.</span></span> <span data-ttu-id="3a374-210">如果构造对象的代码未能初始化值，是否应收到警告？</span><span class="sxs-lookup"><span data-stu-id="3a374-210">Should the code which constructs the object get a warning if they failed to initialize the value?</span></span>

<span data-ttu-id="3a374-211">此时，该字段将永远不会被设置，因此，可能会有许多相似之处和有关无法初始化`private`数据的警告。</span><span class="sxs-lookup"><span data-stu-id="3a374-211">At that point it is clear the field will never be set and hence has a lot of similarities with the warning around failing to initialize `private` data.</span></span> <span data-ttu-id="3a374-212">那么，在这种情况下面会出现一个警告，</span><span class="sxs-lookup"><span data-stu-id="3a374-212">Hence a warning would seemingly have some value here?</span></span>

<span data-ttu-id="3a374-213">不过，此警告有重大缺点：</span><span class="sxs-lookup"><span data-stu-id="3a374-213">There are significant downsides to this warning though:</span></span>
1. <span data-ttu-id="3a374-214">它会使更改`readonly`到`init`的兼容性故事复杂化。</span><span class="sxs-lookup"><span data-stu-id="3a374-214">It complicates the compatibility story of changing `readonly` to `init`.</span></span> 
1. <span data-ttu-id="3a374-215">它需要额外的元数据来表示需要由调用方初始化的成员。</span><span class="sxs-lookup"><span data-stu-id="3a374-215">It requires carrying additional metadata around to denote the members which are required to be initialized by the caller.</span></span>

<span data-ttu-id="3a374-216">此外，如果我们认为在这种情况下，我们认为在这种情况下，强制对象创建者收到警告/错误应对特定字段存在价值，这可能是一项常见功能。</span><span class="sxs-lookup"><span data-stu-id="3a374-216">Further if we believe there is value here in the overall scenario of forcing object creators to be warned / error'd about specific fields then this likely makes sense as a general feature.</span></span> <span data-ttu-id="3a374-217">没有理由只`init`应限制为成员。</span><span class="sxs-lookup"><span data-stu-id="3a374-217">There is no reason it should be limited to just `init` members.</span></span>

<span data-ttu-id="3a374-218">**解决方法**字段和属性的`init`使用不会出现警告。</span><span class="sxs-lookup"><span data-stu-id="3a374-218">**Resolution** There will be no warning on consumption of `init` fields and properties.</span></span>

<span data-ttu-id="3a374-219">LDM 希望更广泛地讨论必填字段和属性的概念。</span><span class="sxs-lookup"><span data-stu-id="3a374-219">LDM wants to have a broader discussion on the idea of required fields and properties.</span></span> <span data-ttu-id="3a374-220">这可能会导致我们返回并重新考虑我们的`init`成员和验证的位置。</span><span class="sxs-lookup"><span data-stu-id="3a374-220">That may cause us to come back and reconsider our position on `init` members and validation.</span></span>

## <a name="allow-init-as-a-field-modifier"></a><span data-ttu-id="3a374-221">允许 init 作为字段修饰符</span><span class="sxs-lookup"><span data-stu-id="3a374-221">Allow init as a field modifier</span></span>
<span data-ttu-id="3a374-222">同样，也`init`可以作为属性访问器，它也可以作为字段的指定，将其作为`init`属性提供。</span><span class="sxs-lookup"><span data-stu-id="3a374-222">In the same way `init` can serve as a property accessor it could also serve as a designation on fields to give them similar behaviors as `init` properties.</span></span>
<span data-ttu-id="3a374-223">这将允许在类型、派生类型或对象初始值设定项完成构造之前，对字段进行赋值。</span><span class="sxs-lookup"><span data-stu-id="3a374-223">That would allow for the field to be assigned before construction was complete by the type, derived types, or object initializers.</span></span>

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

<span data-ttu-id="3a374-224">在元数据中，这些字段将以与`readonly`字段相同的方式进行标记，但使用附加的属性或 modreq `init`指示它们是样式字段。</span><span class="sxs-lookup"><span data-stu-id="3a374-224">In metadata these fields would be marked in the same way as `readonly` fields but with an additional attribute or modreq to indicate they are `init` style fields.</span></span> 

<span data-ttu-id="3a374-225">**解决方法**LDM 同意此建议是听起来的，但是总体情况下，这种情况并不完全相同。</span><span class="sxs-lookup"><span data-stu-id="3a374-225">**Resolution** LDM agrees this proposal is sound but overall the scenario felt disjoint from properties.</span></span> <span data-ttu-id="3a374-226">这里的决策是只继续进行`init`属性。</span><span class="sxs-lookup"><span data-stu-id="3a374-226">The decision was to proceed only with `init` properties for now.</span></span> <span data-ttu-id="3a374-227">由于`init`属性可以在属性的声明类型上改变`readonly`字段，因此它具有适当的灵活性级别。</span><span class="sxs-lookup"><span data-stu-id="3a374-227">This has a suitable level of flexibility as an `init` property can mutate a `readonly` field on the declaring type of the property.</span></span> <span data-ttu-id="3a374-228">如果有大量客户反馈来表明方案，则会很少见。</span><span class="sxs-lookup"><span data-stu-id="3a374-228">This will be reconsidered if there is significant customer feedback that justifies the scenario.</span></span>

### <a name="allow-init-as-a-type-modifier"></a><span data-ttu-id="3a374-229">允许 init 作为类型修饰符</span><span class="sxs-lookup"><span data-stu-id="3a374-229">Allow init as a type modifier</span></span>
<span data-ttu-id="3a374-230">在中，可以将`readonly`修饰符应用于`struct`来自动将所有字段声明`readonly`为， `init` `struct`也`class`可以在上声明唯一修饰符，并将所有字段自动标记为。 `init`</span><span class="sxs-lookup"><span data-stu-id="3a374-230">In the same way the `readonly` modifier can be applied to a `struct` to automatically declare all fields as `readonly`, the `init` only modifier can be declared on a `struct` or `class` to automatically mark all fields as `init`.</span></span>
<span data-ttu-id="3a374-231">这意味着以下两个类型声明是等效的：</span><span class="sxs-lookup"><span data-stu-id="3a374-231">This means the following two type declarations are equivalent:</span></span>

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

<span data-ttu-id="3a374-232">**解决方法**此功能在此处过于*刻意*，并与它`readonly struct`所基于的功能冲突。</span><span class="sxs-lookup"><span data-stu-id="3a374-232">**Resolution** This feature is too *cute* here and conflicts with the `readonly struct` feature on which it is based.</span></span> <span data-ttu-id="3a374-233">此`readonly struct`功能很简单，因为它适用`readonly`于所有成员：字段、方法等。此`init struct`功能仅适用于属性。</span><span class="sxs-lookup"><span data-stu-id="3a374-233">The `readonly struct` feature is simple in that it applies `readonly` to all members: fields, methods, etc ... The `init struct` feature would only apply to properties.</span></span> <span data-ttu-id="3a374-234">这实际上会使用户感到困惑。</span><span class="sxs-lookup"><span data-stu-id="3a374-234">This actually ends up making it confusing for users.</span></span> 

<span data-ttu-id="3a374-235">假设只`init`对某个类型的某些方面有效，我们拒绝将其作为类型修饰符。</span><span class="sxs-lookup"><span data-stu-id="3a374-235">Given that `init` is only valid on certain aspects of a type we rejected the idea of having it as a type modifier.</span></span>

## <a name="considerations"></a><span data-ttu-id="3a374-236">注意事项</span><span class="sxs-lookup"><span data-stu-id="3a374-236">Considerations</span></span>

### <a name="compatibility"></a><span data-ttu-id="3a374-237">兼容性</span><span class="sxs-lookup"><span data-stu-id="3a374-237">Compatibility</span></span>
<span data-ttu-id="3a374-238">此`init`功能旨在与现有`get`的属性兼容。</span><span class="sxs-lookup"><span data-stu-id="3a374-238">The `init` feature is designed to be compatible with existing `get` only properties.</span></span> <span data-ttu-id="3a374-239">具体而言，它只是一个属性的完全累加性更改，该`get`属性仅在今天，但需要更多的 flexbile 对象创建语义。</span><span class="sxs-lookup"><span data-stu-id="3a374-239">Specifically it is meant to be a completely additive change for a property which is `get` only today but desires more flexbile object creation semantics.</span></span>

<span data-ttu-id="3a374-240">例如，考虑以下类型：</span><span class="sxs-lookup"><span data-stu-id="3a374-240">For example consider the following type:</span></span>

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

<span data-ttu-id="3a374-241">添加`init`到这些属性中是非重大更改：</span><span class="sxs-lookup"><span data-stu-id="3a374-241">Adding `init` to these properties is a non-breaking change:</span></span>

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

### <a name="il-verification"></a><span data-ttu-id="3a374-242">IL 验证</span><span class="sxs-lookup"><span data-stu-id="3a374-242">IL verification</span></span>
<span data-ttu-id="3a374-243">当 .NET Core 决定重新实现 IL 验证时，需要对规则进行调整，以便为成员提供`init`帐户。</span><span class="sxs-lookup"><span data-stu-id="3a374-243">When .NET Core decides to re-implement IL verification the rules will need to be adjusted to account for `init` members.</span></span> <span data-ttu-id="3a374-244">这将需要包含在规则更改中，以便对`readonly`数据进行非转变性的更改。</span><span class="sxs-lookup"><span data-stu-id="3a374-244">This will need to be included in the rule changes for non-mutating acess to `readonly` data.</span></span>

<span data-ttu-id="3a374-245">IL 验证规则需要分为两部分：</span><span class="sxs-lookup"><span data-stu-id="3a374-245">The IL verification rules will need to be broken into two parts:</span></span> 

1. <span data-ttu-id="3a374-246">允许`init`成员设置`readonly`字段。</span><span class="sxs-lookup"><span data-stu-id="3a374-246">Allowing `init` members to set a `readonly` field.</span></span>
1. <span data-ttu-id="3a374-247">确定何时可以`init`合法地调用成员。</span><span class="sxs-lookup"><span data-stu-id="3a374-247">Determining when an `init` member can be legally called.</span></span>

<span data-ttu-id="3a374-248">第一种是对现有规则的简单调整。</span><span class="sxs-lookup"><span data-stu-id="3a374-248">The first is a simple adjustment to the existing rules.</span></span> <span data-ttu-id="3a374-249">可以学会使用 IL 验证程序来识别`init`成员，而只需考虑在此类成员`readonly`上`this`可设置的字段。</span><span class="sxs-lookup"><span data-stu-id="3a374-249">The IL verifier can be taught to recognize `init` members and from there it just needs to consider a `readonly` field to be settable on `this` in such a member.</span></span>

<span data-ttu-id="3a374-250">第二个规则更复杂。</span><span class="sxs-lookup"><span data-stu-id="3a374-250">The second rule is more complicated.</span></span> <span data-ttu-id="3a374-251">在对象初始值设定项的简单情况下，该规则是直接的。</span><span class="sxs-lookup"><span data-stu-id="3a374-251">In the simple case of object initializers the rule is straight forward.</span></span> <span data-ttu-id="3a374-252">当`new`表达式的结果仍在`init`堆栈上时，调用成员应合法。</span><span class="sxs-lookup"><span data-stu-id="3a374-252">It should be legal to call `init` members when the result of a `new` expression is still on the stack.</span></span> <span data-ttu-id="3a374-253">也就是说，在将值存储在本地数组元素或字段中，或将其作为参数传递给另一种方法时，它仍合法调用`init`成员。</span><span class="sxs-lookup"><span data-stu-id="3a374-253">That is until the value has been stored in a local, array element or field or passed as an argument to another method it will still be legal to call `init` members.</span></span> <span data-ttu-id="3a374-254">这可确保将`new`表达式的结果发布到命名标识符（而不是`this`）后，它将不再合法调用`init`成员。</span><span class="sxs-lookup"><span data-stu-id="3a374-254">This ensures that once the result of the `new` expression is published to a named identifier (other than `this`) then it will no longer be legal to call `init` members.</span></span> 

<span data-ttu-id="3a374-255">尽管我们混合`init`了成员、对象初始值设定项和`await`，但这种情况更复杂。</span><span class="sxs-lookup"><span data-stu-id="3a374-255">The more complicated case though is when we mix `init` members, object initializers and `await`.</span></span> <span data-ttu-id="3a374-256">这可能会导致新创建的对象暂时提升到状态机中，因而放入字段中。</span><span class="sxs-lookup"><span data-stu-id="3a374-256">That can cause the newly created object to be temporarily hoisted into a state machine and hence put into a field.</span></span>

```cs
var student = new Student() 
{
    Name = await SomeMethod()
};
```

<span data-ttu-id="3a374-257">在此之前`Name` ， `new Student()`的结果将作为字段 hoised 到状态机中。</span><span class="sxs-lookup"><span data-stu-id="3a374-257">Here the result of `new Student()` will be hoised into a state machine as a field before the set of `Name` occurs.</span></span> <span data-ttu-id="3a374-258">编译器需要将此类已提升的字段标记为 IL 验证程序理解它们不是用户可访问的方式，因此不违反的预期语义`init`。</span><span class="sxs-lookup"><span data-stu-id="3a374-258">The compiler will need to mark such hoisted fields in a way that the IL verifier understands they're not user accessible and hence doesn't violate the intended semantics of `init`.</span></span>

### <a name="init-members"></a><span data-ttu-id="3a374-259">init 成员</span><span class="sxs-lookup"><span data-stu-id="3a374-259">init members</span></span>
<span data-ttu-id="3a374-260">`init`修饰符可以扩展以应用于所有实例成员。</span><span class="sxs-lookup"><span data-stu-id="3a374-260">The `init` modifier could be extended to apply to all instance members.</span></span> <span data-ttu-id="3a374-261">这会在对象构造`init`过程中通用化概念，并允许类型声明可在构造过程中 partipate 以初始化`init`字段和属性的帮助器方法。</span><span class="sxs-lookup"><span data-stu-id="3a374-261">This would generalize the concept of `init` during object construction and allow types to declare helper methods that could partipate in the construction process to initialize `init` fields and properties.</span></span>

<span data-ttu-id="3a374-262">此类成员将具有`init`访问器在此设计中执行的所有 restricions。</span><span class="sxs-lookup"><span data-stu-id="3a374-262">Such members would have all the restricions that an `init` accessor does in this design.</span></span> <span data-ttu-id="3a374-263">不过，这种需要是值得怀疑的，而这可以通过一种兼容的方式安全地添加在语言的未来版本中。</span><span class="sxs-lookup"><span data-stu-id="3a374-263">The need is questionable though and this can be safely added in a future version of the language in a compatible manner.</span></span>

### <a name="generate-three-accessors"></a><span data-ttu-id="3a374-264">生成三个访问器</span><span class="sxs-lookup"><span data-stu-id="3a374-264">Generate three accessors</span></span>
<span data-ttu-id="3a374-265">属性的`init`一个可能实现是`init`完全独立于。 `set`</span><span class="sxs-lookup"><span data-stu-id="3a374-265">One potential implementation of `init` properties is to make `init` completely separate from `set`.</span></span> <span data-ttu-id="3a374-266">这意味着，一个属性可能具有三个不同的访问`get`器`set` ： `init`、和。</span><span class="sxs-lookup"><span data-stu-id="3a374-266">That means that a property can potentially have three different accessors: `get`, `set` and `init`.</span></span>

<span data-ttu-id="3a374-267">这可能会带来的优势是，允许使用 modreq 来强制实现正确性，同时保持二进制兼容性。</span><span class="sxs-lookup"><span data-stu-id="3a374-267">This has the potential advantage of allowing the use of modreq to enforce correctness while maintaining binary compatibility.</span></span> <span data-ttu-id="3a374-268">实现大致如下所示：</span><span class="sxs-lookup"><span data-stu-id="3a374-268">The implementation would roughly be the following:</span></span>

1. <span data-ttu-id="3a374-269">如果`init`有，则始终发出访问器`set`。</span><span class="sxs-lookup"><span data-stu-id="3a374-269">An `init` accessor is always emitted if there is a `set`.</span></span> <span data-ttu-id="3a374-270">当开发人员未定义时，它只是对的`set`引用。</span><span class="sxs-lookup"><span data-stu-id="3a374-270">When not defined by the developer it is simply a reference to `set`.</span></span> 
1. <span data-ttu-id="3a374-271">对象初始值设定项中的属性集将始终使用`init` （如果存在），但`set`如果缺少，则返回。</span><span class="sxs-lookup"><span data-stu-id="3a374-271">The set of a property in an object initializer will always use `init` if present but fall back to `set` if it's missing.</span></span>

<span data-ttu-id="3a374-272">这意味着开发人员始终可以从属性中`init`安全地删除。</span><span class="sxs-lookup"><span data-stu-id="3a374-272">This means that a developer can always safely delete `init` from a property.</span></span> 

<span data-ttu-id="3a374-273">这种设计的缺点是，仅当在出现`init`时**始终**发出时才有用`set`。</span><span class="sxs-lookup"><span data-stu-id="3a374-273">The downside of this design is that is only useful if `init` is **always** emitted when there is a `set`.</span></span> <span data-ttu-id="3a374-274">如果`init`以前删除了该语言，则它必须假定它是，因此`init`必须始终发出。</span><span class="sxs-lookup"><span data-stu-id="3a374-274">The language can't know if `init` was deleted in the past, it has to assume it was and hence the `init` must always be emitted.</span></span> <span data-ttu-id="3a374-275">这会导致一种重要的元数据扩展，而不是在此处提供兼容性的代价。</span><span class="sxs-lookup"><span data-stu-id="3a374-275">That would cause a significant metadata expansion and is simply not worth the cost of the compatibility here.</span></span>
