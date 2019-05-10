---
ms.openlocfilehash: 72d17175dfb8ef284dce6cf7e5837420fa06f16a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488880"
---
# <a name="structs"></a><span data-ttu-id="47fe9-101">结构</span><span class="sxs-lookup"><span data-stu-id="47fe9-101">Structs</span></span>

<span data-ttu-id="47fe9-102">结构与类很相似，因为它们表示可以包含数据成员和函数成员的数据结构。</span><span class="sxs-lookup"><span data-stu-id="47fe9-102">Structs are similar to classes in that they represent data structures that can contain data members and function members.</span></span> <span data-ttu-id="47fe9-103">但是，与类不同，结构是值类型，不需要堆分配。</span><span class="sxs-lookup"><span data-stu-id="47fe9-103">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="47fe9-104">结构类型的变量直接包含的数据结构，而类类型的变量包含数据后, 一种称为对象的引用。</span><span class="sxs-lookup"><span data-stu-id="47fe9-104">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span>

<span data-ttu-id="47fe9-105">结构对包含值语义的小型数据结构特别有用。</span><span class="sxs-lookup"><span data-stu-id="47fe9-105">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="47fe9-106">复数、坐标系中的点或字典中的键值对都是结构的典型示例。</span><span class="sxs-lookup"><span data-stu-id="47fe9-106">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="47fe9-107">这些数据结构的关键在于它们具有少量数据成员，它们不需要使用继承或引用的标识，而且，它们可以方便地使用实现值语义分配复制而不是引用的值的位置。</span><span class="sxs-lookup"><span data-stu-id="47fe9-107">Key to these data structures is that they have few data members, that they do not require use of inheritance or referential identity, and that they can be conveniently implemented using value semantics where assignment copies the value instead of the reference.</span></span>

<span data-ttu-id="47fe9-108">如中所述[简单类型](types.md#simple-types)，如由 C# 中，提供的简单类型`int`， `double`，和`bool`，实际上是所有结构类型。</span><span class="sxs-lookup"><span data-stu-id="47fe9-108">As described in [Simple types](types.md#simple-types), the simple types provided by C#, such as `int`, `double`, and `bool`, are in fact all struct types.</span></span> <span data-ttu-id="47fe9-109">就像这些预定义的类型是结构，它还有可能要使用的结构和运算符重载在 C# 语言中实现新的"基元"类型。</span><span class="sxs-lookup"><span data-stu-id="47fe9-109">Just as these predefined types are structs, it is also possible to use structs and operator overloading to implement new "primitive" types in the C# language.</span></span> <span data-ttu-id="47fe9-110">在本章末尾提供这种类型的两个示例 ([结构示例](structs.md#struct-examples))。</span><span class="sxs-lookup"><span data-stu-id="47fe9-110">Two examples of such types are given at the end of this chapter ([Struct examples](structs.md#struct-examples)).</span></span>

## <a name="struct-declarations"></a><span data-ttu-id="47fe9-111">结构声明</span><span class="sxs-lookup"><span data-stu-id="47fe9-111">Struct declarations</span></span>

<span data-ttu-id="47fe9-112">一个*struct_declaration*是*type_declaration* ([类型声明](namespaces.md#type-declarations)) 的声明的新结构：</span><span class="sxs-lookup"><span data-stu-id="47fe9-112">A *struct_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new struct:</span></span>

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

<span data-ttu-id="47fe9-113">一个*struct_declaration*包含一组可选*特性*([特性](attributes.md)) 后, 跟一组可选的*struct_modifier*s ([结构修饰符](structs.md#struct-modifiers)) 后, 跟可选`partial`修饰符，关键字后跟`struct`和一个*标识符*命名结构后, 跟可选*type_parameter_list*规范 ([类型参数](classes.md#type-parameters)) 后, 跟一个可选*struct_interfaces*规范 ([Partial 修饰符](structs.md#partial-modifier))) 后, 跟一个可选*type_parameter_constraints_clause*s 规范 ([类型参数约束](classes.md#type-parameter-constraints)) 后, 跟*struct_body* ([结构体](structs.md#struct-body)) 后, 跟分号 （可选）。</span><span class="sxs-lookup"><span data-stu-id="47fe9-113">A *struct_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *struct_modifier*s ([Struct modifiers](structs.md#struct-modifiers)), followed by an optional `partial` modifier, followed by the keyword `struct` and an *identifier* that names the struct, followed by an optional *type_parameter_list* specification ([Type parameters](classes.md#type-parameters)), followed by an optional *struct_interfaces* specification ([Partial modifier](structs.md#partial-modifier)) ), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *struct_body* ([Struct body](structs.md#struct-body)), optionally followed by a semicolon.</span></span>

### <a name="struct-modifiers"></a><span data-ttu-id="47fe9-114">结构修饰符</span><span class="sxs-lookup"><span data-stu-id="47fe9-114">Struct modifiers</span></span>

<span data-ttu-id="47fe9-115">一个*struct_declaration*可以根据需要包含一系列结构修饰符：</span><span class="sxs-lookup"><span data-stu-id="47fe9-115">A *struct_declaration* may optionally include a sequence of struct modifiers:</span></span>

```antlr
struct_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | struct_modifier_unsafe
    ;
```

<span data-ttu-id="47fe9-116">它是要在结构声明中多次出现的相同修饰符的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="47fe9-116">It is a compile-time error for the same modifier to appear multiple times in a struct declaration.</span></span>

<span data-ttu-id="47fe9-117">结构声明中的修饰符具有相同的含义的类声明 ([类声明](classes.md#class-declarations))。</span><span class="sxs-lookup"><span data-stu-id="47fe9-117">The modifiers of a struct declaration have the same meaning as those of a class declaration ([Class declarations](classes.md#class-declarations)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="47fe9-118">Partial 修饰符</span><span class="sxs-lookup"><span data-stu-id="47fe9-118">Partial modifier</span></span>

<span data-ttu-id="47fe9-119">`partial`修饰符指示这*struct_declaration*是分部类型声明。</span><span class="sxs-lookup"><span data-stu-id="47fe9-119">The `partial` modifier indicates that this *struct_declaration* is a partial type declaration.</span></span> <span data-ttu-id="47fe9-120">具有相同名称的封闭命名空间或类型声明中的多个分部结构声明结合在一起形成一个结构声明中，遵循的规则中指定[分部类型](classes.md#partial-types)。</span><span class="sxs-lookup"><span data-stu-id="47fe9-120">Multiple partial struct declarations with the same name within an enclosing namespace or type declaration combine to form one struct declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="struct-interfaces"></a><span data-ttu-id="47fe9-121">结构接口</span><span class="sxs-lookup"><span data-stu-id="47fe9-121">Struct interfaces</span></span>

<span data-ttu-id="47fe9-122">结构声明可能包括*struct_interfaces*规范，说用例结构直接实现给定的接口类型。</span><span class="sxs-lookup"><span data-stu-id="47fe9-122">A struct declaration may include a *struct_interfaces* specification, in which case the struct is said to directly implement the given interface types.</span></span>

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

<span data-ttu-id="47fe9-123">讨论了接口实现中进一步[接口实现代码](interfaces.md#interface-implementations)。</span><span class="sxs-lookup"><span data-stu-id="47fe9-123">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="struct-body"></a><span data-ttu-id="47fe9-124">在结构体</span><span class="sxs-lookup"><span data-stu-id="47fe9-124">Struct body</span></span>

<span data-ttu-id="47fe9-125">*Struct_body*结构的定义结构的成员。</span><span class="sxs-lookup"><span data-stu-id="47fe9-125">The *struct_body* of a struct defines the members of the struct.</span></span>

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a><span data-ttu-id="47fe9-126">结构成员</span><span class="sxs-lookup"><span data-stu-id="47fe9-126">Struct members</span></span>

<span data-ttu-id="47fe9-127">结构的成员包括引入的成员及其*struct_member_declaration*s 和成员继承自类型`System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="47fe9-127">The members of a struct consist of the members introduced by its *struct_member_declaration*s and the members inherited from the type `System.ValueType`.</span></span>

```antlr
struct_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | static_constructor_declaration
    | type_declaration
    | struct_member_declaration_unsafe
    ;
```

<span data-ttu-id="47fe9-128">除了中记下的区别之外[类和结构的区别](structs.md#class-and-struct-differences)，在提供的类成员的说明[类成员](classes.md#class-members)通过[迭代器](classes.md#iterators)适用于结构以及成员。</span><span class="sxs-lookup"><span data-stu-id="47fe9-128">Except for the differences noted in [Class and struct differences](structs.md#class-and-struct-differences), the descriptions of class members provided in [Class members](classes.md#class-members) through [Iterators](classes.md#iterators) apply to struct members as well.</span></span>

## <a name="class-and-struct-differences"></a><span data-ttu-id="47fe9-129">类和结构的区别</span><span class="sxs-lookup"><span data-stu-id="47fe9-129">Class and struct differences</span></span>

<span data-ttu-id="47fe9-130">结构与类不同，在几个重要方面：</span><span class="sxs-lookup"><span data-stu-id="47fe9-130">Structs differ from classes in several important ways:</span></span>

*  <span data-ttu-id="47fe9-131">结构是值类型 ([值的语义](structs.md#value-semantics))。</span><span class="sxs-lookup"><span data-stu-id="47fe9-131">Structs are value types ([Value semantics](structs.md#value-semantics)).</span></span>
*  <span data-ttu-id="47fe9-132">所有结构类型隐式都继承自类`System.ValueType`([继承](structs.md#inheritance))。</span><span class="sxs-lookup"><span data-stu-id="47fe9-132">All struct types implicitly inherit from the class `System.ValueType` ([Inheritance](structs.md#inheritance)).</span></span>
*  <span data-ttu-id="47fe9-133">结构类型的变量进行赋值会创建一份所赋的值 ([分配](structs.md#assignment))。</span><span class="sxs-lookup"><span data-stu-id="47fe9-133">Assignment to a variable of a struct type creates a copy of the value being assigned ([Assignment](structs.md#assignment)).</span></span>
*  <span data-ttu-id="47fe9-134">结构的默认值是生成通过将所有值类型字段都设置为其默认值和所有引用类型字段的值`null`([默认值](structs.md#default-values))。</span><span class="sxs-lookup"><span data-stu-id="47fe9-134">The default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null` ([Default values](structs.md#default-values)).</span></span>
*  <span data-ttu-id="47fe9-135">装箱和取消装箱操作用于将结构类型之间进行转换并`object`([装箱和取消装箱](structs.md#boxing-and-unboxing))。</span><span class="sxs-lookup"><span data-stu-id="47fe9-135">Boxing and unboxing operations are used to convert between a struct type and `object` ([Boxing and unboxing](structs.md#boxing-and-unboxing)).</span></span>
*  <span data-ttu-id="47fe9-136">含义`this`是不同的结构 ([此访问权限](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="47fe9-136">The meaning of `this` is different for structs ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="47fe9-137">不允许在结构的实例字段声明包括变量的初始值设定项 ([字段初始值设定项](structs.md#field-initializers))。</span><span class="sxs-lookup"><span data-stu-id="47fe9-137">Instance field declarations for a struct are not permitted to include variable initializers ([Field initializers](structs.md#field-initializers)).</span></span>
*  <span data-ttu-id="47fe9-138">结构不允许的无参数实例构造函数声明 ([构造函数](structs.md#constructors))。</span><span class="sxs-lookup"><span data-stu-id="47fe9-138">A struct is not permitted to declare a parameterless instance constructor ([Constructors](structs.md#constructors)).</span></span>
*  <span data-ttu-id="47fe9-139">结构不允许声明析构函数 ([析构函数](structs.md#destructors))。</span><span class="sxs-lookup"><span data-stu-id="47fe9-139">A struct is not permitted to declare a destructor ([Destructors](structs.md#destructors)).</span></span>

### <a name="value-semantics"></a><span data-ttu-id="47fe9-140">值语义</span><span class="sxs-lookup"><span data-stu-id="47fe9-140">Value semantics</span></span>

<span data-ttu-id="47fe9-141">结构是值类型 ([值类型](types.md#value-types)) 并被视为具有值语义。</span><span class="sxs-lookup"><span data-stu-id="47fe9-141">Structs are value types ([Value types](types.md#value-types)) and are said to have value semantics.</span></span> <span data-ttu-id="47fe9-142">类，但是，是引用类型 ([引用类型](types.md#reference-types)) 并被视为具有引用语义。</span><span class="sxs-lookup"><span data-stu-id="47fe9-142">Classes, on the other hand, are reference types ([Reference types](types.md#reference-types)) and are said to have reference semantics.</span></span>

<span data-ttu-id="47fe9-143">结构类型的变量直接包含的数据结构，而类类型的变量包含数据后, 一种称为对象的引用。</span><span class="sxs-lookup"><span data-stu-id="47fe9-143">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span> <span data-ttu-id="47fe9-144">当结构`B`包含类型的实例字段`A`和`A`是一个结构类型，是编译时错误`A`依赖于`B`或从构造类型`B`。</span><span class="sxs-lookup"><span data-stu-id="47fe9-144">When a struct `B` contains an instance field of type `A` and `A` is a struct type, it is a compile-time error for `A` to depend on `B` or a type constructed from `B`.</span></span> <span data-ttu-id="47fe9-145">结构`X`***直接依赖于***结构`Y`如果`X`包含类型的实例字段`Y`。</span><span class="sxs-lookup"><span data-stu-id="47fe9-145">A struct `X` ***directly depends on*** a struct `Y` if `X` contains an instance field of type `Y`.</span></span> <span data-ttu-id="47fe9-146">根据此定义，结构所依赖的结构的完整集合是可传递闭包的***直接依赖于***关系。</span><span class="sxs-lookup"><span data-stu-id="47fe9-146">Given this definition, the complete set of structs upon which a struct depends is the transitive closure of the ***directly depends on*** relationship.</span></span>  <span data-ttu-id="47fe9-147">例如</span><span class="sxs-lookup"><span data-stu-id="47fe9-147">For example</span></span>
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
<span data-ttu-id="47fe9-148">是一个错误因为`Node`包含其自己的类型的实例字段。</span><span class="sxs-lookup"><span data-stu-id="47fe9-148">is an error because `Node` contains an instance field of its own type.</span></span>  <span data-ttu-id="47fe9-149">另一个示例</span><span class="sxs-lookup"><span data-stu-id="47fe9-149">Another example</span></span>
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
<span data-ttu-id="47fe9-150">是一个错误，因为每个类型`A`， `B`，和`C`彼此依赖。</span><span class="sxs-lookup"><span data-stu-id="47fe9-150">is an error because each of the types `A`, `B`, and `C` depend on each other.</span></span>

<span data-ttu-id="47fe9-151">借助类，它是两个变量来引用同一对象，并因此可能会对一个变量以影响其他变量所引用的对象的操作。</span><span class="sxs-lookup"><span data-stu-id="47fe9-151">With classes, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="47fe9-152">对于结构，每个变量都具有其自己的数据副本 (的情况下除外`ref`和`out`参数变量)，则可能会影响另一个操作。</span><span class="sxs-lookup"><span data-stu-id="47fe9-152">With structs, the variables each have their own copy of the data (except in the case of `ref` and `out` parameter variables), and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="47fe9-153">此外，由于结构不是引用类型，不能为结构类型的值的`null`。</span><span class="sxs-lookup"><span data-stu-id="47fe9-153">Furthermore, because structs are not reference types, it is not possible for values of a struct type to be `null`.</span></span>

<span data-ttu-id="47fe9-154">已知的声明</span><span class="sxs-lookup"><span data-stu-id="47fe9-154">Given the declaration</span></span>
```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
<span data-ttu-id="47fe9-155">代码片段</span><span class="sxs-lookup"><span data-stu-id="47fe9-155">the code fragment</span></span>
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
<span data-ttu-id="47fe9-156">输出值`10`。</span><span class="sxs-lookup"><span data-stu-id="47fe9-156">outputs the value `10`.</span></span> <span data-ttu-id="47fe9-157">分配`a`到`b`会创建一份值，并`b`因此不受分配到`a.x`。</span><span class="sxs-lookup"><span data-stu-id="47fe9-157">The assignment of `a` to `b` creates a copy of the value, and `b` is thus unaffected by the assignment to `a.x`.</span></span> <span data-ttu-id="47fe9-158">有`Point`改为已声明为一个类，则输出将为`100`因为`a`和`b`将引用同一对象。</span><span class="sxs-lookup"><span data-stu-id="47fe9-158">Had `Point` instead been declared as a class, the output would be `100` because `a` and `b` would reference the same object.</span></span>

### <a name="inheritance"></a><span data-ttu-id="47fe9-159">继承</span><span class="sxs-lookup"><span data-stu-id="47fe9-159">Inheritance</span></span>

<span data-ttu-id="47fe9-160">所有结构类型隐式都继承自类`System.ValueType`，而后者又继承自类`object`。</span><span class="sxs-lookup"><span data-stu-id="47fe9-160">All struct types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="47fe9-161">结构声明可以指定一组实现的接口，但不能为结构声明中指定的基类。</span><span class="sxs-lookup"><span data-stu-id="47fe9-161">A struct declaration may specify a list of implemented interfaces, but it is not possible for a struct declaration to specify a base class.</span></span>

<span data-ttu-id="47fe9-162">结构类型永远不会是抽象的始终隐式密封。</span><span class="sxs-lookup"><span data-stu-id="47fe9-162">Struct types are never abstract and are always implicitly sealed.</span></span> <span data-ttu-id="47fe9-163">`abstract`和`sealed`修饰符因此不允许在结构声明中。</span><span class="sxs-lookup"><span data-stu-id="47fe9-163">The `abstract` and `sealed` modifiers are therefore not permitted in a struct declaration.</span></span>

<span data-ttu-id="47fe9-164">由于结构不支持继承，因此不能为结构成员的声明可访问性`protected`或`protected internal`。</span><span class="sxs-lookup"><span data-stu-id="47fe9-164">Since inheritance isn't supported for structs, the declared accessibility of a struct member cannot be `protected` or `protected internal`.</span></span>

<span data-ttu-id="47fe9-165">结构中的函数成员不能`abstract`或`virtual`，并`override`修饰符仅可以重写方法继承自`System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="47fe9-165">Function members in a struct cannot be `abstract` or `virtual`, and the `override` modifier is allowed only to override methods inherited from `System.ValueType`.</span></span>

### <a name="assignment"></a><span data-ttu-id="47fe9-166">赋值</span><span class="sxs-lookup"><span data-stu-id="47fe9-166">Assignment</span></span>

<span data-ttu-id="47fe9-167">结构类型的变量进行赋值会创建一份所赋的值。</span><span class="sxs-lookup"><span data-stu-id="47fe9-167">Assignment to a variable of a struct type creates a copy of the value being assigned.</span></span> <span data-ttu-id="47fe9-168">这不同于分配给变量的类类型，复制引用而不是由引用标识的对象。</span><span class="sxs-lookup"><span data-stu-id="47fe9-168">This differs from assignment to a variable of a class type, which copies the reference but not the object identified by the reference.</span></span>

<span data-ttu-id="47fe9-169">类似于分配，将结构作为值参数传递或者作为函数成员的结果返回时，该结构的副本创建。</span><span class="sxs-lookup"><span data-stu-id="47fe9-169">Similar to an assignment, when a struct is passed as a value parameter or returned as the result of a function member, a copy of the struct is created.</span></span> <span data-ttu-id="47fe9-170">可以通过对使用函数成员的引用传递结构`ref`或`out`参数。</span><span class="sxs-lookup"><span data-stu-id="47fe9-170">A struct may be passed by reference to a function member using a `ref` or `out` parameter.</span></span>

<span data-ttu-id="47fe9-171">当属性或索引器的结构作为赋值目标时，与属性或索引器访问关联的实例表达式必须分类为变量中。</span><span class="sxs-lookup"><span data-stu-id="47fe9-171">When a property or indexer of a struct is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="47fe9-172">如果实例表达式分类为一个值，发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="47fe9-172">If the instance expression is classified as a value, a compile-time error occurs.</span></span> <span data-ttu-id="47fe9-173">进一步详细介绍[简单的赋值](expressions.md#simple-assignment)。</span><span class="sxs-lookup"><span data-stu-id="47fe9-173">This is described in further detail in [Simple assignment](expressions.md#simple-assignment).</span></span>

### <a name="default-values"></a><span data-ttu-id="47fe9-174">默认值</span><span class="sxs-lookup"><span data-stu-id="47fe9-174">Default values</span></span>

<span data-ttu-id="47fe9-175">如中所述[默认值](variables.md#default-values)，在创建时，有几种变量将自动初始化为其默认值。</span><span class="sxs-lookup"><span data-stu-id="47fe9-175">As described in [Default values](variables.md#default-values), several kinds of variables are automatically initialized to their default value when they are created.</span></span> <span data-ttu-id="47fe9-176">对于类类型和其他引用类型的变量，此默认值是`null`。</span><span class="sxs-lookup"><span data-stu-id="47fe9-176">For variables of class types and other reference types, this default value is `null`.</span></span> <span data-ttu-id="47fe9-177">但是，由于结构是值类型，不能`null`，默认值的结构是通过将所有值类型字段都设置为其默认值和所有引用类型字段生成的值`null`。</span><span class="sxs-lookup"><span data-stu-id="47fe9-177">However, since structs are value types that cannot be `null`, the default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="47fe9-178">指`Point`结构声明该示例上方</span><span class="sxs-lookup"><span data-stu-id="47fe9-178">Referring to the `Point` struct declared above, the example</span></span>
```csharp
Point[] a = new Point[100];
```
<span data-ttu-id="47fe9-179">初始化每个`Point`中通过如下设置生成的值数组`x`和`y`为零的字段。</span><span class="sxs-lookup"><span data-stu-id="47fe9-179">initializes each `Point` in the array to the value produced by setting the `x` and `y` fields to zero.</span></span>

<span data-ttu-id="47fe9-180">结构的默认值对应于该结构的默认构造函数返回的值 ([默认构造函数](types.md#default-constructors))。</span><span class="sxs-lookup"><span data-stu-id="47fe9-180">The default value of a struct corresponds to the value returned by the default constructor of the struct ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="47fe9-181">与类不同，结构不允许声明一个无参数实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="47fe9-181">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="47fe9-182">相反，每个结构都隐式具有一个无参数实例构造函数，它始终返回的值设置为其默认值和所有引用类型字段的所有值类型字段而得出的`null`。</span><span class="sxs-lookup"><span data-stu-id="47fe9-182">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="47fe9-183">结构应旨在有效状态，请考虑默认值初始化状态。</span><span class="sxs-lookup"><span data-stu-id="47fe9-183">Structs should be designed to consider the default initialization state a valid state.</span></span> <span data-ttu-id="47fe9-184">在示例</span><span class="sxs-lookup"><span data-stu-id="47fe9-184">In the example</span></span>
```csharp
using System;

struct KeyValuePair
{
    string key;
    string value;

    public KeyValuePair(string key, string value) {
        if (key == null || value == null) throw new ArgumentException();
        this.key = key;
        this.value = value;
    }
}
```
<span data-ttu-id="47fe9-185">用户定义的实例构造函数可防止 null 值仅在显式调用。</span><span class="sxs-lookup"><span data-stu-id="47fe9-185">the user-defined instance constructor protects against null values only where it is explicitly called.</span></span> <span data-ttu-id="47fe9-186">在情况下，`KeyValuePair`变量可能会发生默认值初始化`key`和`value`字段将为 null，且该结构必须准备好处理此状态。</span><span class="sxs-lookup"><span data-stu-id="47fe9-186">In cases where a `KeyValuePair` variable is subject to default value initialization, the `key` and `value` fields will be null, and the struct must be prepared to handle this state.</span></span>

### <a name="boxing-and-unboxing"></a><span data-ttu-id="47fe9-187">装箱和取消装箱</span><span class="sxs-lookup"><span data-stu-id="47fe9-187">Boxing and unboxing</span></span>

<span data-ttu-id="47fe9-188">类类型的值可以转换为类型`object`或只需通过将引用视为另一种类型在编译时由类实现的接口类型。</span><span class="sxs-lookup"><span data-stu-id="47fe9-188">A value of a class type can be converted to type `object` or to an interface type that is implemented by the class simply by treating the reference as another type at compile-time.</span></span> <span data-ttu-id="47fe9-189">同样，类型的值`object`或接口类型的值可以无需更改的引用转换回类类型 （但当然运行时类型检查是必需的这种情况下）。</span><span class="sxs-lookup"><span data-stu-id="47fe9-189">Likewise, a value of type `object` or a value of an interface type can be converted back to a class type without changing the reference (but of course a run-time type check is required in this case).</span></span>

<span data-ttu-id="47fe9-190">由于结构不是引用类型，这些操作以不同方式实现对结构类型。</span><span class="sxs-lookup"><span data-stu-id="47fe9-190">Since structs are not reference types, these operations are implemented differently for struct types.</span></span> <span data-ttu-id="47fe9-191">当结构类型的值转换为类型`object`或由该结构实现的接口类型，装箱操作发生。</span><span class="sxs-lookup"><span data-stu-id="47fe9-191">When a value of a struct type is converted to type `object` or to an interface type that is implemented by the struct, a boxing operation takes place.</span></span> <span data-ttu-id="47fe9-192">同样，当类型的值时`object`或接口类型的值转换为结构类型，取消装箱操作会发生。</span><span class="sxs-lookup"><span data-stu-id="47fe9-192">Likewise, when a value of type `object` or a value of an interface type is converted back to a struct type, an unboxing operation takes place.</span></span> <span data-ttu-id="47fe9-193">从类类型相同的操作的主要差异是，装箱和取消装箱结构将值复制传入或传出的已装箱的实例。</span><span class="sxs-lookup"><span data-stu-id="47fe9-193">A key difference from the same operations on class types is that boxing and unboxing copies the struct value either into or out of the boxed instance.</span></span> <span data-ttu-id="47fe9-194">因此，在装箱或取消装箱操作后，对未装箱结构所做更改将不反映在装箱的结构。</span><span class="sxs-lookup"><span data-stu-id="47fe9-194">Thus, following a boxing or unboxing operation, changes made to the unboxed struct are not reflected in the boxed struct.</span></span>

<span data-ttu-id="47fe9-195">当结构类型重写继承的虚方法`System.Object`(如`Equals`， `GetHashCode`，或`ToString`)，通过结构类型的实例的虚拟方法调用不会导致装箱发生。</span><span class="sxs-lookup"><span data-stu-id="47fe9-195">When a struct type overrides a virtual method inherited from `System.Object` (such as `Equals`, `GetHashCode`, or `ToString`), invocation of the virtual method through an instance of the struct type does not cause boxing to occur.</span></span> <span data-ttu-id="47fe9-196">即使在该结构用作类型参数，并且调用发生通过类型参数类型的实例时，这是，则返回 true。</span><span class="sxs-lookup"><span data-stu-id="47fe9-196">This is true even when the struct is used as a type parameter and the invocation occurs through an instance of the type parameter type.</span></span> <span data-ttu-id="47fe9-197">例如：</span><span class="sxs-lookup"><span data-stu-id="47fe9-197">For example:</span></span>
```csharp
using System;

struct Counter
{
    int value;

    public override string ToString() {
        value++;
        return value.ToString();
    }
}

class Program
{
    static void Test<T>() where T: new() {
        T x = new T();
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
    }

    static void Main() {
        Test<Counter>();
    }
}
```

<span data-ttu-id="47fe9-198">该程序的输出为：</span><span class="sxs-lookup"><span data-stu-id="47fe9-198">The output of the program is:</span></span>
```
1
2
3
```

<span data-ttu-id="47fe9-199">尽管不是好的风格`ToString`有副作用，示例演示了针对第三次调用中的发生没有值类型装箱`x.ToString()`。</span><span class="sxs-lookup"><span data-stu-id="47fe9-199">Although it is bad style for `ToString` to have side effects, the example demonstrates that no boxing occurred for the three invocations of `x.ToString()`.</span></span>

<span data-ttu-id="47fe9-200">类似地，永远不会隐式进行装箱时访问受约束的类型参数上的成员。</span><span class="sxs-lookup"><span data-stu-id="47fe9-200">Similarly, boxing never implicitly occurs when accessing a member on a constrained type parameter.</span></span> <span data-ttu-id="47fe9-201">例如，假设一个接口`ICounter`包含一个方法，`Increment`这可用于修改值。</span><span class="sxs-lookup"><span data-stu-id="47fe9-201">For example, suppose an interface `ICounter` contains a method `Increment` which can be used to modify a value.</span></span> <span data-ttu-id="47fe9-202">如果`ICounter`用作约束的实现`Increment`使用对变量的引用来调用方法的`Increment`装箱副本永远不会对调用。</span><span class="sxs-lookup"><span data-stu-id="47fe9-202">If `ICounter` is used as a constraint, the implementation of the `Increment` method is called with a reference to the variable that `Increment` was called on, never a boxed copy.</span></span>

```csharp
using System;

interface ICounter
{
    void Increment();
}

struct Counter: ICounter
{
    int value;

    public override string ToString() {
        return value.ToString();
    }

    void ICounter.Increment() {
        value++;
    }
}

class Program
{
    static void Test<T>() where T: ICounter, new() {
        T x = new T();
        Console.WriteLine(x);
        x.Increment();                    // Modify x
        Console.WriteLine(x);
        ((ICounter)x).Increment();        // Modify boxed copy of x
        Console.WriteLine(x);
    }

    static void Main() {
        Test<Counter>();
    }
}
```

<span data-ttu-id="47fe9-203">首次调用`Increment`修改变量中的值`x`。</span><span class="sxs-lookup"><span data-stu-id="47fe9-203">The first call to `Increment` modifies the value in the variable `x`.</span></span> <span data-ttu-id="47fe9-204">这并不等同于第二次调用`Increment`，它修改的已装箱副本中的值`x`。</span><span class="sxs-lookup"><span data-stu-id="47fe9-204">This is not equivalent to the second call to `Increment`, which modifies the value in a boxed copy of `x`.</span></span> <span data-ttu-id="47fe9-205">因此，程序的输出为：</span><span class="sxs-lookup"><span data-stu-id="47fe9-205">Thus, the output of the program is:</span></span>
```
0
1
1
```

<span data-ttu-id="47fe9-206">装箱和取消装箱的更多详细信息，请参阅[装箱和取消装箱](types.md#boxing-and-unboxing)。</span><span class="sxs-lookup"><span data-stu-id="47fe9-206">For further details on boxing and unboxing, see [Boxing and unboxing](types.md#boxing-and-unboxing).</span></span>

### <a name="meaning-of-this"></a><span data-ttu-id="47fe9-207">这其中的含义</span><span class="sxs-lookup"><span data-stu-id="47fe9-207">Meaning of this</span></span>

<span data-ttu-id="47fe9-208">实例构造函数或类的实例函数成员`this`分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="47fe9-208">Within an instance constructor or instance function member of a class, `this` is classified as a value.</span></span> <span data-ttu-id="47fe9-209">因此，尽管`this`可用于引用的实例为其函数成员调用，它不能将分配给`this`函数成员的类中。</span><span class="sxs-lookup"><span data-stu-id="47fe9-209">Thus, while `this` can be used to refer to the instance for which the function member was invoked, it is not possible to assign to `this` in a function member of a class.</span></span>

<span data-ttu-id="47fe9-210">中的结构，实例构造函数`this`对应于`out`参数的结构类型，和中的结构，实例函数成员`this`对应于`ref`结构类型的参数。</span><span class="sxs-lookup"><span data-stu-id="47fe9-210">Within an instance constructor of a struct, `this` corresponds to an `out` parameter of the struct type, and within an instance function member of a struct, `this` corresponds to a `ref` parameter of the struct type.</span></span> <span data-ttu-id="47fe9-211">在这两种情况下，`this`分类为变量，并且可能需要修改整个结构通过将分配给为其调用的函数成员`this`或者通过将此作为`ref`或`out`参数。</span><span class="sxs-lookup"><span data-stu-id="47fe9-211">In both cases, `this` is classified as a variable, and it is possible to modify the entire struct for which the function member was invoked by assigning to `this` or by passing this as a `ref` or `out` parameter.</span></span>

### <a name="field-initializers"></a><span data-ttu-id="47fe9-212">字段初始值设定项</span><span class="sxs-lookup"><span data-stu-id="47fe9-212">Field initializers</span></span>

<span data-ttu-id="47fe9-213">如中所述[默认值](structs.md#default-values)，将所有值类型字段都设置为其默认值和所有引用类型字段而得出的值的结构的默认值包括`null`。</span><span class="sxs-lookup"><span data-stu-id="47fe9-213">As described in [Default values](structs.md#default-values), the default value of a struct consists of the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span> <span data-ttu-id="47fe9-214">出于此原因，结构不允许实例字段声明中包含变量的初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="47fe9-214">For this reason, a struct does not permit instance field declarations to include variable initializers.</span></span> <span data-ttu-id="47fe9-215">此限制仅适用于实例字段。</span><span class="sxs-lookup"><span data-stu-id="47fe9-215">This restriction applies only to instance fields.</span></span> <span data-ttu-id="47fe9-216">结构的静态字段都可以包含变量的初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="47fe9-216">Static fields of a struct are permitted to include variable initializers.</span></span>

<span data-ttu-id="47fe9-217">该示例</span><span class="sxs-lookup"><span data-stu-id="47fe9-217">The example</span></span>
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
<span data-ttu-id="47fe9-218">错误是因为实例字段声明包括变量的初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="47fe9-218">is in error because the instance field declarations include variable initializers.</span></span>

### <a name="constructors"></a><span data-ttu-id="47fe9-219">构造函数</span><span class="sxs-lookup"><span data-stu-id="47fe9-219">Constructors</span></span>

<span data-ttu-id="47fe9-220">与类不同，结构不允许声明一个无参数实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="47fe9-220">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="47fe9-221">相反，每个结构都隐式具有一个无参数实例构造函数，它始终返回将所有值类型字段都设置为其默认值和所有引用类型字段为 null 而得出的值 ([默认构造函数](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="47fe9-221">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to null ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="47fe9-222">结构可以声明具有参数的实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="47fe9-222">A struct can declare instance constructors having parameters.</span></span> <span data-ttu-id="47fe9-223">例如</span><span class="sxs-lookup"><span data-stu-id="47fe9-223">For example</span></span>
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

<span data-ttu-id="47fe9-224">已知上面的声明语句</span><span class="sxs-lookup"><span data-stu-id="47fe9-224">Given the above declaration, the statements</span></span>
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
<span data-ttu-id="47fe9-225">二者都可以创建`Point`与`x`和`y`初始化为零。</span><span class="sxs-lookup"><span data-stu-id="47fe9-225">both create a `Point` with `x` and `y` initialized to zero.</span></span>

<span data-ttu-id="47fe9-226">不允许使用结构实例构造函数以包括窗体的构造函数初始值设定项`base(...)`。</span><span class="sxs-lookup"><span data-stu-id="47fe9-226">A struct instance constructor is not permitted to include a constructor initializer of the form `base(...)`.</span></span>

<span data-ttu-id="47fe9-227">如果结构实例构造函数不会指定构造函数初始值设定项`this`变量对应于`out`参数的结构类型，并类似于`out`参数，`this`必须明确赋值 （[明确赋值](variables.md#definite-assignment)) 在构造函数将返回其中每个位置。</span><span class="sxs-lookup"><span data-stu-id="47fe9-227">If the struct instance constructor doesn't specify a constructor initializer, the `this` variable corresponds to an `out` parameter of the struct type, and similar to an `out` parameter, `this` must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at every location where the constructor returns.</span></span> <span data-ttu-id="47fe9-228">如果结构实例构造函数指定的构造函数初始值设定项，`this`变量对应于`ref`参数的结构类型，并类似于`ref`参数，`this`被视为上明确赋值向构造函数主体的项。</span><span class="sxs-lookup"><span data-stu-id="47fe9-228">If the struct instance constructor specifies a constructor initializer, the `this` variable corresponds to a `ref` parameter of the struct type, and similar to a `ref` parameter, `this` is considered definitely assigned on entry to the constructor body.</span></span> <span data-ttu-id="47fe9-229">请考虑下面的实例构造函数实现：</span><span class="sxs-lookup"><span data-stu-id="47fe9-229">Consider the instance constructor implementation below:</span></span>
```csharp
struct Point
{
    int x, y;

    public int X {
        set { x = value; }
    }

    public int Y {
        set { y = value; }
    }

    public Point(int x, int y) {
        X = x;        // error, this is not yet definitely assigned
        Y = y;        // error, this is not yet definitely assigned
    }
}
```

<span data-ttu-id="47fe9-230">任何实例成员函数 (包括属性 set 访问器`X`和`Y`) 可以调用，直到已明确赋值正在构造的结构的所有字段。</span><span class="sxs-lookup"><span data-stu-id="47fe9-230">No instance member function (including the set accessors for the properties `X` and `Y`) can be called until all fields of the struct being constructed have been definitely assigned.</span></span> <span data-ttu-id="47fe9-231">唯一的例外涉及自动实现的属性 ([自动实现的属性](classes.md#automatically-implemented-properties))。</span><span class="sxs-lookup"><span data-stu-id="47fe9-231">The only exception involves automatically implemented properties ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="47fe9-232">明确赋值规则 ([简单的赋值表达式](variables.md#simple-assignment-expressions)) 专门免除该结构类型的结构类型的实例构造函数中的自动属性赋值： 此类赋值被视为决定性自动属性的隐藏的支持字段赋值。</span><span class="sxs-lookup"><span data-stu-id="47fe9-232">The definite assignment rules ([Simple assignment expressions](variables.md#simple-assignment-expressions)) specifically exempt assignment to an auto-property of a struct type within an instance constructor of that struct type: such an assignment is considered a definite assignment of the hidden backing field of the auto-property.</span></span> <span data-ttu-id="47fe9-233">因此，以下被允许使用：</span><span class="sxs-lookup"><span data-stu-id="47fe9-233">Thus, the following is allowed:</span></span>

```csharp
struct Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public Point(int x, int y) {
        X = x;      // allowed, definitely assigns backing field
        Y = y;      // allowed, definitely assigns backing field
    }
```

### <a name="destructors"></a><span data-ttu-id="47fe9-234">析构函数</span><span class="sxs-lookup"><span data-stu-id="47fe9-234">Destructors</span></span>

<span data-ttu-id="47fe9-235">结构不是允许声明析构函数。</span><span class="sxs-lookup"><span data-stu-id="47fe9-235">A struct is not permitted to declare a destructor.</span></span>

### <a name="static-constructors"></a><span data-ttu-id="47fe9-236">静态构造函数</span><span class="sxs-lookup"><span data-stu-id="47fe9-236">Static constructors</span></span>

<span data-ttu-id="47fe9-237">结构的静态构造函数所遵循的与类相同的规则。</span><span class="sxs-lookup"><span data-stu-id="47fe9-237">Static constructors for structs follow most of the same rules as for classes.</span></span> <span data-ttu-id="47fe9-238">结构类型的静态构造函数的执行由第一次以下事件发生在应用程序域内触发：</span><span class="sxs-lookup"><span data-stu-id="47fe9-238">The execution of a static constructor for a struct type is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="47fe9-239">引用结构类型的静态成员。</span><span class="sxs-lookup"><span data-stu-id="47fe9-239">A static member of the struct type is referenced.</span></span>
*  <span data-ttu-id="47fe9-240">结构类型的显式声明的构造函数调用。</span><span class="sxs-lookup"><span data-stu-id="47fe9-240">An explicitly declared constructor of the struct type is called.</span></span>

<span data-ttu-id="47fe9-241">创建的默认值 ([默认值](structs.md#default-values)) 结构的类型不会触发该静态构造函数。</span><span class="sxs-lookup"><span data-stu-id="47fe9-241">The creation of default values ([Default values](structs.md#default-values)) of struct types does not trigger the static constructor.</span></span> <span data-ttu-id="47fe9-242">（此示例是数组中元素的初始值。）</span><span class="sxs-lookup"><span data-stu-id="47fe9-242">(An example of this is the initial value of elements in an array.)</span></span>

## <a name="struct-examples"></a><span data-ttu-id="47fe9-243">结构示例</span><span class="sxs-lookup"><span data-stu-id="47fe9-243">Struct examples</span></span>

<span data-ttu-id="47fe9-244">下面显示了使用的两个重要示例`struct`类型来创建可用于对预定义的类型的语言，但使用修改后的语义的同样的类型。</span><span class="sxs-lookup"><span data-stu-id="47fe9-244">The following shows two significant examples of using `struct` types to create types that can be used similarly to the predefined types of the language, but with modified semantics.</span></span>

### <a name="database-integer-type"></a><span data-ttu-id="47fe9-245">数据库的整数类型</span><span class="sxs-lookup"><span data-stu-id="47fe9-245">Database integer type</span></span>

<span data-ttu-id="47fe9-246">`DBInt`下面结构实现整数类型可表示的值的完整集`int`类型，以及指示未知的值的其他状态。</span><span class="sxs-lookup"><span data-stu-id="47fe9-246">The `DBInt` struct below implements an integer type that can represent the complete set of values of the `int` type, plus an additional state that indicates an unknown value.</span></span> <span data-ttu-id="47fe9-247">在数据库中通常使用具有这些特征的类型。</span><span class="sxs-lookup"><span data-stu-id="47fe9-247">A type with these characteristics is commonly used in databases.</span></span>

```csharp
using System;

public struct DBInt
{
    // The Null member represents an unknown DBInt value.

    public static readonly DBInt Null = new DBInt();

    // When the defined field is true, this DBInt represents a known value
    // which is stored in the value field. When the defined field is false,
    // this DBInt represents an unknown value, and the value field is 0.

    int value;
    bool defined;

    // Private instance constructor. Creates a DBInt with a known value.

    DBInt(int value) {
        this.value = value;
        this.defined = true;
    }

    // The IsNull property is true if this DBInt represents an unknown value.

    public bool IsNull { get { return !defined; } }

    // The Value property is the known value of this DBInt, or 0 if this
    // DBInt represents an unknown value.

    public int Value { get { return value; } }

    // Implicit conversion from int to DBInt.

    public static implicit operator DBInt(int x) {
        return new DBInt(x);
    }

    // Explicit conversion from DBInt to int. Throws an exception if the
    // given DBInt represents an unknown value.

    public static explicit operator int(DBInt x) {
        if (!x.defined) throw new InvalidOperationException();
        return x.value;
    }

    public static DBInt operator +(DBInt x) {
        return x;
    }

    public static DBInt operator -(DBInt x) {
        return x.defined ? -x.value : Null;
    }

    public static DBInt operator +(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value + y.value: Null;
    }

    public static DBInt operator -(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value - y.value: Null;
    }

    public static DBInt operator *(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value * y.value: Null;
    }

    public static DBInt operator /(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value / y.value: Null;
    }

    public static DBInt operator %(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value % y.value: Null;
    }

    public static DBBool operator ==(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value == y.value: DBBool.Null;
    }

    public static DBBool operator !=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value != y.value: DBBool.Null;
    }

    public static DBBool operator >(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value > y.value: DBBool.Null;
    }

    public static DBBool operator <(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value < y.value: DBBool.Null;
    }

    public static DBBool operator >=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value >= y.value: DBBool.Null;
    }

    public static DBBool operator <=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value <= y.value: DBBool.Null;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBInt)) return false;
        DBInt x = (DBInt)obj;
        return value == x.value && defined == x.defined;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        return defined? value.ToString(): "DBInt.Null";
    }
}
```

### <a name="database-boolean-type"></a><span data-ttu-id="47fe9-248">数据库的布尔值类型</span><span class="sxs-lookup"><span data-stu-id="47fe9-248">Database boolean type</span></span>

<span data-ttu-id="47fe9-249">`DBBool`下面结构实现三值逻辑类型。</span><span class="sxs-lookup"><span data-stu-id="47fe9-249">The `DBBool` struct below implements a three-valued logical type.</span></span> <span data-ttu-id="47fe9-250">此类型的可能的值为`DBBool.True`， `DBBool.False`，并`DBBool.Null`，其中`Null`成员指示未知的值。</span><span class="sxs-lookup"><span data-stu-id="47fe9-250">The possible values of this type are `DBBool.True`, `DBBool.False`, and `DBBool.Null`, where the `Null` member indicates an unknown value.</span></span> <span data-ttu-id="47fe9-251">在数据库中通常使用此类三值逻辑类型。</span><span class="sxs-lookup"><span data-stu-id="47fe9-251">Such three-valued logical types are commonly used in databases.</span></span>

```csharp
using System;

public struct DBBool
{
    // The three possible DBBool values.

    public static readonly DBBool Null = new DBBool(0);
    public static readonly DBBool False = new DBBool(-1);
    public static readonly DBBool True = new DBBool(1);

    // Private field that stores -1, 0, 1 for False, Null, True.

    sbyte value;

    // Private instance constructor. The value parameter must be -1, 0, or 1.

    DBBool(int value) {
        this.value = (sbyte)value;
    }

    // Properties to examine the value of a DBBool. Return true if this
    // DBBool has the given value, false otherwise.

    public bool IsNull { get { return value == 0; } }

    public bool IsFalse { get { return value < 0; } }

    public bool IsTrue { get { return value > 0; } }

    // Implicit conversion from bool to DBBool. Maps true to DBBool.True and
    // false to DBBool.False.

    public static implicit operator DBBool(bool x) {
        return x? True: False;
    }

    // Explicit conversion from DBBool to bool. Throws an exception if the
    // given DBBool is Null, otherwise returns true or false.

    public static explicit operator bool(DBBool x) {
        if (x.value == 0) throw new InvalidOperationException();
        return x.value > 0;
    }

    // Equality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator ==(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value == y.value? True: False;
    }

    // Inequality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator !=(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value != y.value? True: False;
    }

    // Logical negation operator. Returns True if the operand is False, Null
    // if the operand is Null, or False if the operand is True.

    public static DBBool operator !(DBBool x) {
        return new DBBool(-x.value);
    }

    // Logical AND operator. Returns False if either operand is False,
    // otherwise Null if either operand is Null, otherwise True.

    public static DBBool operator &(DBBool x, DBBool y) {
        return new DBBool(x.value < y.value? x.value: y.value);
    }

    // Logical OR operator. Returns True if either operand is True, otherwise
    // Null if either operand is Null, otherwise False.

    public static DBBool operator |(DBBool x, DBBool y) {
        return new DBBool(x.value > y.value? x.value: y.value);
    }

    // Definitely true operator. Returns true if the operand is True, false
    // otherwise.

    public static bool operator true(DBBool x) {
        return x.value > 0;
    }

    // Definitely false operator. Returns true if the operand is False, false
    // otherwise.

    public static bool operator false(DBBool x) {
        return x.value < 0;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBBool)) return false;
        return value == ((DBBool)obj).value;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        if (value > 0) return "DBBool.True";
        if (value < 0) return "DBBool.False";
        return "DBBool.Null";
    }
}
```
