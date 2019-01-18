---
ms.openlocfilehash: a8ad8a8b3eda1d00fa745bd92e4371eacc36b79f
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229508"
---
# <a name="attributes"></a><span data-ttu-id="d1aa3-101">特性</span><span class="sxs-lookup"><span data-stu-id="d1aa3-101">Attributes</span></span>

<span data-ttu-id="d1aa3-102">C# 语言的许多使程序员能够指定有关在程序中定义的实体的声明性信息。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-102">Much of the C# language enables the programmer to specify declarative information about the entities defined in the program.</span></span> <span data-ttu-id="d1aa3-103">例如，在类中方法的可访问性通过修饰来指定其与*method_modifier*s `public`， `protected`， `internal`，并`private`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-103">For example, the accessibility of a method in a class is specified by decorating it with the *method_modifier*s `public`, `protected`, `internal`, and `private`.</span></span>

<span data-ttu-id="d1aa3-104">C# 允许程序员发明的声明性信息，名为的新类型***属性***。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-104">C# enables programmers to invent new kinds of declarative information, called ***attributes***.</span></span> <span data-ttu-id="d1aa3-105">然后，程序员可以将属性附加到各种程序实体，并检索在运行时环境中的属性信息。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-105">Programmers can then attach attributes to various program entities, and retrieve attribute information in a run-time environment.</span></span> <span data-ttu-id="d1aa3-106">例如，可以定义一个框架`HelpAttribute`可以放在特定程序元素 （如类和方法） 以提供从这些程序元素到其文档的映射的属性。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-106">For instance, a framework might define a `HelpAttribute` attribute that can be placed on certain program elements (such as classes and methods) to provide a mapping from those program elements to their documentation.</span></span>

<span data-ttu-id="d1aa3-107">通过用于特性类的声明定义特性 ([属性类](attributes.md#attribute-classes))，这可以具有位置和命名参数 ([位置和命名的参数](attributes.md#positional-and-named-parameters))。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-107">Attributes are defined through the declaration of attribute classes ([Attribute classes](attributes.md#attribute-classes)), which may have positional and named parameters ([Positional and named parameters](attributes.md#positional-and-named-parameters)).</span></span> <span data-ttu-id="d1aa3-108">特性附加到使用特性规范的 C# 程序中的实体 ([属性规范](attributes.md#attribute-specification))，并且可以作为属性实例在运行时检索 ([特性实例](attributes.md#attribute-instances))。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-108">Attributes are attached to entities in a C# program using attribute specifications ([Attribute specification](attributes.md#attribute-specification)), and can be retrieved at run-time as attribute instances ([Attribute instances](attributes.md#attribute-instances)).</span></span>

## <a name="attribute-classes"></a><span data-ttu-id="d1aa3-109">属性类</span><span class="sxs-lookup"><span data-stu-id="d1aa3-109">Attribute classes</span></span>

<span data-ttu-id="d1aa3-110">从抽象类派生的类`System.Attribute`，无论直接或间接地，是***特性类***。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-110">A class that derives from the abstract class `System.Attribute`, whether directly or indirectly, is an ***attribute class***.</span></span> <span data-ttu-id="d1aa3-111">特性类的声明类型的定义一个新***特性***可放置在声明。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-111">The declaration of an attribute class defines a new kind of ***attribute*** that can be placed on a declaration.</span></span> <span data-ttu-id="d1aa3-112">按照约定，特性类命名为后缀`Attribute`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-112">By convention, attribute classes are named with a suffix of `Attribute`.</span></span> <span data-ttu-id="d1aa3-113">使用属性可以包括或省略此后缀。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-113">Uses of an attribute may either include or omit this suffix.</span></span>

### <a name="attribute-usage"></a><span data-ttu-id="d1aa3-114">特性用法</span><span class="sxs-lookup"><span data-stu-id="d1aa3-114">Attribute usage</span></span>

<span data-ttu-id="d1aa3-115">该属性`AttributeUsage`([AttributeUsage 特性](attributes.md#the-attributeusage-attribute)) 用于描述如何使用特性类。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-115">The attribute `AttributeUsage` ([The AttributeUsage attribute](attributes.md#the-attributeusage-attribute)) is used to describe how an attribute class can be used.</span></span>

<span data-ttu-id="d1aa3-116">`AttributeUsage` 具有位置参数 ([位置和命名的参数](attributes.md#positional-and-named-parameters)) 使特性类来指定在其可以使用的声明的类型。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-116">`AttributeUsage` has a positional parameter ([Positional and named parameters](attributes.md#positional-and-named-parameters)) that enables an attribute class to specify the kinds of declarations on which it can be used.</span></span> <span data-ttu-id="d1aa3-117">该示例</span><span class="sxs-lookup"><span data-stu-id="d1aa3-117">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Interface)]
public class SimpleAttribute: Attribute 
{
    ...
}
```

<span data-ttu-id="d1aa3-118">定义一个名为属性类`SimpleAttribute`可在放置*class_declaration*s 和*interface_declaration*仅。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-118">defines an attribute class named `SimpleAttribute` that can be placed on *class_declaration*s and *interface_declaration*s only.</span></span> <span data-ttu-id="d1aa3-119">该示例</span><span class="sxs-lookup"><span data-stu-id="d1aa3-119">The example</span></span>

```csharp
[Simple] class Class1 {...}

[Simple] interface Interface1 {...}
```

<span data-ttu-id="d1aa3-120">显示的几种用法`Simple`属性。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-120">shows several uses of the `Simple` attribute.</span></span> <span data-ttu-id="d1aa3-121">尽管此特性定义的名称`SimpleAttribute`，当使用此属性时，`Attribute`可能会省略后缀，从而导致的短名称`Simple`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-121">Although this attribute is defined with the name `SimpleAttribute`, when this attribute is used, the `Attribute` suffix may be omitted, resulting in the short name `Simple`.</span></span> <span data-ttu-id="d1aa3-122">因此，上面的示例在语义上等效于下面：</span><span class="sxs-lookup"><span data-stu-id="d1aa3-122">Thus, the example above is semantically equivalent to the following:</span></span>

```csharp
[SimpleAttribute] class Class1 {...}

[SimpleAttribute] interface Interface1 {...}
```

<span data-ttu-id="d1aa3-123">`AttributeUsage` 有一个命名的参数 ([位置和命名的参数](attributes.md#positional-and-named-parameters)) 调用`AllowMultiple`，指示是否可以多次指定该属性为给定实体。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-123">`AttributeUsage` has a named parameter ([Positional and named parameters](attributes.md#positional-and-named-parameters)) called `AllowMultiple`, which indicates whether the attribute can be specified more than once for a given entity.</span></span> <span data-ttu-id="d1aa3-124">如果`AllowMultiple`类的属性为 true，则该特性类是***多用途特性类***，并且在实体上多次指定。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-124">If `AllowMultiple` for an attribute class is true, then that attribute class is a ***multi-use attribute class***, and can be specified more than once on an entity.</span></span> <span data-ttu-id="d1aa3-125">如果`AllowMultiple`类的属性为 false 或是未指定，则该特性类是***一次性特性类***，并且在上一个实体最多一次指定。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-125">If `AllowMultiple` for an attribute class is false or it is unspecified, then that attribute class is a ***single-use attribute class***, and can be specified at most once on an entity.</span></span>

<span data-ttu-id="d1aa3-126">该示例</span><span class="sxs-lookup"><span data-stu-id="d1aa3-126">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class, AllowMultiple = true)]
public class AuthorAttribute: Attribute
{
    private string name;

    public AuthorAttribute(string name) {
        this.name = name;
    }

    public string Name {
        get { return name; }
    }
}
```

<span data-ttu-id="d1aa3-127">定义一个名为的多用途特性类`AuthorAttribute`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-127">defines a multi-use attribute class named `AuthorAttribute`.</span></span> <span data-ttu-id="d1aa3-128">该示例</span><span class="sxs-lookup"><span data-stu-id="d1aa3-128">The example</span></span>

```csharp
[Author("Brian Kernighan"), Author("Dennis Ritchie")] 
class Class1
{
    ...
}
```

<span data-ttu-id="d1aa3-129">演示了一个类声明的两种用法与`Author`属性。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-129">shows a class declaration with two uses of the `Author` attribute.</span></span>

<span data-ttu-id="d1aa3-130">`AttributeUsage` 具有名为的另一个命名的参数`Inherited`，指示是否属性，当指定为基类，也由派生自该基类的类继承。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-130">`AttributeUsage` has another named parameter called `Inherited`, which indicates whether the attribute, when specified on a base class, is also inherited by classes that derive from that base class.</span></span> <span data-ttu-id="d1aa3-131">如果`Inherited`类的属性为 true，则继承该属性。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-131">If `Inherited` for an attribute class is true, then that attribute is inherited.</span></span> <span data-ttu-id="d1aa3-132">如果`Inherited`类的属性为 false，则不会继承该属性。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-132">If `Inherited` for an attribute class is false then that attribute is not inherited.</span></span> <span data-ttu-id="d1aa3-133">如果未指定，其默认值为 true。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-133">If it is unspecified, its default value is true.</span></span>

<span data-ttu-id="d1aa3-134">特性类`X`不具有`AttributeUsage`属性附加到它，如</span><span class="sxs-lookup"><span data-stu-id="d1aa3-134">An attribute class `X` not having an `AttributeUsage` attribute attached to it, as in</span></span>

```csharp
using System;

class X: Attribute {...}
```

<span data-ttu-id="d1aa3-135">等效于以下：</span><span class="sxs-lookup"><span data-stu-id="d1aa3-135">is equivalent to the following:</span></span>

```csharp
using System;

[AttributeUsage(
    AttributeTargets.All,
    AllowMultiple = false,
    Inherited = true)
]
class X: Attribute {...}
```

### <a name="positional-and-named-parameters"></a><span data-ttu-id="d1aa3-136">定位和命名参数</span><span class="sxs-lookup"><span data-stu-id="d1aa3-136">Positional and named parameters</span></span>

<span data-ttu-id="d1aa3-137">特性类可以具有***位置参数***并***命名参数***。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-137">Attribute classes can have ***positional parameters*** and ***named parameters***.</span></span> <span data-ttu-id="d1aa3-138">每个公共实例构造函数，为该特性类定义的该特性类的位置参数是有效的序列。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-138">Each public instance constructor for an attribute class defines a valid sequence of positional parameters for that attribute class.</span></span> <span data-ttu-id="d1aa3-139">每个非静态公共读写字段和属性的特性类定义特性类的命名的参数。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-139">Each non-static public read-write field and property for an attribute class defines a named parameter for the attribute class.</span></span>

<span data-ttu-id="d1aa3-140">该示例</span><span class="sxs-lookup"><span data-stu-id="d1aa3-140">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpAttribute: Attribute
{
    public HelpAttribute(string url) {        // Positional parameter
        ...
    }

    public string Topic {                     // Named parameter
        get {...}
        set {...}
    }

    public string Url {
        get {...}
    }
}
```

<span data-ttu-id="d1aa3-141">定义一个名为属性类`HelpAttribute`它具有一个位置参数`url`，和一个命名的参数， `Topic`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-141">defines an attribute class named `HelpAttribute` that has one positional parameter, `url`, and one named parameter, `Topic`.</span></span> <span data-ttu-id="d1aa3-142">尽管非静态和 public，该属性，则`Url`不定义一个命名的参数，因为它不是读写。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-142">Although it is non-static and public, the property `Url` does not define a named parameter, since it is not read-write.</span></span>

<span data-ttu-id="d1aa3-143">可能按如下所示使用此特性类：</span><span class="sxs-lookup"><span data-stu-id="d1aa3-143">This attribute class might be used as follows:</span></span>

```csharp
[Help("http://www.mycompany.com/.../Class1.htm")]
class Class1
{
    ...
}

[Help("http://www.mycompany.com/.../Misc.htm", Topic = "Class2")]
class Class2
{
    ...
}
```

### <a name="attribute-parameter-types"></a><span data-ttu-id="d1aa3-144">特性参数类型</span><span class="sxs-lookup"><span data-stu-id="d1aa3-144">Attribute parameter types</span></span>

<span data-ttu-id="d1aa3-145">为属性类的位置和命名参数的类型仅限于***属性参数类型***，它们是：</span><span class="sxs-lookup"><span data-stu-id="d1aa3-145">The types of positional and named parameters for an attribute class are limited to the ***attribute parameter types***, which are:</span></span>

*  <span data-ttu-id="d1aa3-146">以下类型之一： `bool`， `byte`， `char`， `double`， `float`， `int`， `long`， `sbyte`， `short`， `string`， `uint`， `ulong`，`ushort`.</span><span class="sxs-lookup"><span data-stu-id="d1aa3-146">One of the following types: `bool`, `byte`, `char`, `double`, `float`, `int`, `long`, `sbyte`, `short`, `string`, `uint`, `ulong`, `ushort`.</span></span>
*  <span data-ttu-id="d1aa3-147">`object` 类型。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-147">The type `object`.</span></span>
*  <span data-ttu-id="d1aa3-148">`System.Type` 类型。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-148">The type `System.Type`.</span></span>
*  <span data-ttu-id="d1aa3-149">枚举类型，提供具有公共可访问性，并在其中它嵌套 （如果有） 的类型还具有公共可访问性 ([属性规范](attributes.md#attribute-specification))。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-149">An enum type, provided it has public accessibility and the types in which it is nested (if any) also have public accessibility ([Attribute specification](attributes.md#attribute-specification)).</span></span>
*  <span data-ttu-id="d1aa3-150">上述类型的一维数组。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-150">Single-dimensional arrays of the above types.</span></span>
*  <span data-ttu-id="d1aa3-151">构造函数参数或公共字段不包含其中一种类型，不能用作特性规范中的位置或命名参数。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-151">A constructor argument or public field which does not have one of these types, cannot be used as a positional or named parameter in an attribute specification.</span></span>

## <a name="attribute-specification"></a><span data-ttu-id="d1aa3-152">特性规范</span><span class="sxs-lookup"><span data-stu-id="d1aa3-152">Attribute specification</span></span>

<span data-ttu-id="d1aa3-153">***属性规范***是到声明此前定义的属性的应用程序。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-153">***Attribute specification*** is the application of a previously defined attribute to a declaration.</span></span> <span data-ttu-id="d1aa3-154">属性是一种声明的指定的其他声明性信息。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-154">An attribute is a piece of additional declarative information that is specified for a declaration.</span></span> <span data-ttu-id="d1aa3-155">可以在全局范围内 （以指定包含程序集或模块上的属性） 指定特性和有关*type_declaration*s ([类型声明](namespaces.md#type-declarations))， *class_member_declaration*s ([类型参数约束](classes.md#type-parameter-constraints))， *interface_member_declaration*s ([接口成员](interfaces.md#interface-members))， *struct_member_declaration*s ([结构成员](structs.md#struct-members))， *enum_member_declaration*s ([枚举成员](enums.md#enum-members))， *accessor_declarations* ([访问器](classes.md#accessors))， *event_accessor_declarations* ([类似字段的事件](classes.md#field-like-events))，和*formal_parameter_list*s ([方法参数](classes.md#method-parameters))。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-155">Attributes can be specified at global scope (to specify attributes on the containing assembly or module) and for *type_declaration*s ([Type declarations](namespaces.md#type-declarations)), *class_member_declaration*s ([Type parameter constraints](classes.md#type-parameter-constraints)), *interface_member_declaration*s ([Interface members](interfaces.md#interface-members)), *struct_member_declaration*s ([Struct members](structs.md#struct-members)), *enum_member_declaration*s ([Enum members](enums.md#enum-members)), *accessor_declarations* ([Accessors](classes.md#accessors)), *event_accessor_declarations* ([Field-like events](classes.md#field-like-events)), and *formal_parameter_list*s ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="d1aa3-156">以指定属性***属性部分***。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-156">Attributes are specified in ***attribute sections***.</span></span> <span data-ttu-id="d1aa3-157">属性部分包含的一对方括号，环绕一个或多个属性的逗号分隔列表。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-157">An attribute section consists of a pair of square brackets, which surround a comma-separated list of one or more attributes.</span></span> <span data-ttu-id="d1aa3-158">在此类列表中，指定属性和在其中部分附加到同一个程序实体的顺序排列的顺序并不重要。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-158">The order in which attributes are specified in such a list, and the order in which sections attached to the same program entity are arranged, is not significant.</span></span> <span data-ttu-id="d1aa3-159">例如，属性规范`[A][B]`， `[B][A]`， `[A,B]`，和`[B,A]`是等效的。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-159">For instance, the attribute specifications `[A][B]`, `[B][A]`, `[A,B]`, and `[B,A]` are equivalent.</span></span>

```antlr
global_attributes
    : global_attribute_section+
    ;

global_attribute_section
    : '[' global_attribute_target_specifier attribute_list ']'
    | '[' global_attribute_target_specifier attribute_list ',' ']'
    ;

global_attribute_target_specifier
    : global_attribute_target ':'
    ;

global_attribute_target
    : 'assembly'
    | 'module'
    ;

attributes
    : attribute_section+
    ;

attribute_section
    : '[' attribute_target_specifier? attribute_list ']'
    | '[' attribute_target_specifier? attribute_list ',' ']'
    ;

attribute_target_specifier
    : attribute_target ':'
    ;

attribute_target
    : 'field'
    | 'event'
    | 'method'
    | 'param'
    | 'property'
    | 'return'
    | 'type'
    ;

attribute_list
    : attribute (',' attribute)*
    ;

attribute
    : attribute_name attribute_arguments?
    ;

attribute_name
    : type_name
    ;

attribute_arguments
    : '(' positional_argument_list? ')'
    | '(' positional_argument_list ',' named_argument_list ')'
    | '(' named_argument_list ')'
    ;

positional_argument_list
    : positional_argument (',' positional_argument)*
    ;

positional_argument
    : attribute_argument_expression
    ;

named_argument_list
    : named_argument (','  named_argument)*
    ;

named_argument
    : identifier '=' attribute_argument_expression
    ;

attribute_argument_expression
    : expression
    ;
```

<span data-ttu-id="d1aa3-160">属性组成*attribute_name*和位置和命名参数的可选列表。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-160">An attribute consists of an *attribute_name* and an optional list of positional and named arguments.</span></span> <span data-ttu-id="d1aa3-161">位置参数 （如果有） 优先于命名的参数。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-161">The positional arguments (if any) precede the named arguments.</span></span> <span data-ttu-id="d1aa3-162">位置自变量组成*attribute_argument_expression*; 命名的参数包含的名称后, 跟一个等号后, 跟*attribute_argument_expression*，而后者在一起与简单赋值的相同规则约束。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-162">A positional argument consists of an *attribute_argument_expression*; a named argument consists of a name, followed by an equal sign, followed by an *attribute_argument_expression*, which, together, are constrained by the same rules as simple assignment.</span></span> <span data-ttu-id="d1aa3-163">命名参数的顺序并不重要。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-163">The order of named arguments is not significant.</span></span>

<span data-ttu-id="d1aa3-164">*Attribute_name*标识一个特性类。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-164">The *attribute_name* identifies an attribute class.</span></span> <span data-ttu-id="d1aa3-165">如果的窗体*attribute_name*是*type_name* ，请参阅此名称必须是特性类。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-165">If the form of *attribute_name* is *type_name* then this name must refer to an attribute class.</span></span> <span data-ttu-id="d1aa3-166">否则，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-166">Otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="d1aa3-167">该示例</span><span class="sxs-lookup"><span data-stu-id="d1aa3-167">The example</span></span>

```csharp
class Class1 {}

[Class1] class Class2 {}    // Error
```

<span data-ttu-id="d1aa3-168">因为它尝试使用会导致编译时错误`Class1`作为特性类时`Class1`不是特性类。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-168">results in a compile-time error because it attempts to use `Class1` as an attribute class when `Class1` is not an attribute class.</span></span>

<span data-ttu-id="d1aa3-169">某些上下文中允许多个目标属性的规范。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-169">Certain contexts permit the specification of an attribute on more than one target.</span></span> <span data-ttu-id="d1aa3-170">程序可以显式指定目标，通过包括*attribute_target_specifier*。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-170">A program can explicitly specify the target by including an *attribute_target_specifier*.</span></span> <span data-ttu-id="d1aa3-171">当属性放置在全局级别*global_attribute_target_specifier*是必需的。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-171">When an attribute is placed at the global level, a *global_attribute_target_specifier* is required.</span></span> <span data-ttu-id="d1aa3-172">在所有其他位置，将应用合理的默认值，但*attribute_target_specifier*可用于确认或重写中某些不明确的情况下的默认值 （或只需确认中非不明确的情况下的默认值）。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-172">In all other locations, a reasonable default is applied, but an *attribute_target_specifier* can be used to affirm or override the default in certain ambiguous cases (or to just affirm the default in non-ambiguous cases).</span></span> <span data-ttu-id="d1aa3-173">因此，通常情况下， *attribute_target_specifier*s 可以在全局级别省略除外。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-173">Thus, typically, *attribute_target_specifier*s can be omitted except at the global level.</span></span> <span data-ttu-id="d1aa3-174">可能不明确的上下文已得到解决，如下所示：</span><span class="sxs-lookup"><span data-stu-id="d1aa3-174">The potentially ambiguous contexts are resolved as follows:</span></span>

*  <span data-ttu-id="d1aa3-175">在全局范围内指定的属性可以应用到目标程序集或目标模块。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-175">An attribute specified at global scope can apply either to the target assembly or the target module.</span></span> <span data-ttu-id="d1aa3-176">无默认值为此上下文中，因此存在*attribute_target_specifier*始终需要在此上下文中。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-176">No default exists for this context, so an *attribute_target_specifier* is always required in this context.</span></span> <span data-ttu-id="d1aa3-177">是否存在`assembly` *attribute_target_specifier*指示该特性应用于目标程序集; 是否存在`module` *attribute_target_specifier*指示该属性应用到目标模块。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-177">The presence of the `assembly` *attribute_target_specifier* indicates that the attribute applies to the target assembly; the presence of the `module` *attribute_target_specifier* indicates that the attribute applies to the target module.</span></span>
*  <span data-ttu-id="d1aa3-178">在委托声明上指定的属性可以应用到所声明的委托或其返回值。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-178">An attribute specified on a delegate declaration can apply either to the delegate being declared or to its return value.</span></span> <span data-ttu-id="d1aa3-179">如果没有*attribute_target_specifier*，该属性应用于该委托。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-179">In the absence of an *attribute_target_specifier*, the attribute applies to the delegate.</span></span> <span data-ttu-id="d1aa3-180">是否存在`type` *attribute_target_specifier*指示该特性应用于该委托; 是否存在`return` *attribute_target_specifier*指示该特性应用于返回值。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-180">The presence of the `type` *attribute_target_specifier* indicates that the attribute applies to the delegate; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="d1aa3-181">方法声明上指定的属性可以应用到所声明的方法或其返回值。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-181">An attribute specified on a method declaration can apply either to the method being declared or to its return value.</span></span> <span data-ttu-id="d1aa3-182">如果没有*attribute_target_specifier*，该特性应用于该方法。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-182">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="d1aa3-183">是否存在`method` *attribute_target_specifier*指示该特性应用于方法; 是否存在`return` *attribute_target_specifier*指示该特性所应用于返回值。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-183">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="d1aa3-184">在运算符声明上指定的属性可以应用到所声明的运算符或其返回值。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-184">An attribute specified on an operator declaration can apply either to the operator being declared or to its return value.</span></span> <span data-ttu-id="d1aa3-185">如果没有*attribute_target_specifier*，则此属性应用于该运算符。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-185">In the absence of an *attribute_target_specifier*, the attribute applies to the operator.</span></span> <span data-ttu-id="d1aa3-186">是否存在`method` *attribute_target_specifier*指示该特性应用于该运算符; 是否存在`return` *attribute_target_specifier*指示该特性应用于返回值。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-186">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the operator; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="d1aa3-187">指定在事件声明的省略了事件访问器的一个特性可以应用于所声明的事件、 关联的字段 （如果该事件不是抽象的），或关联添加和删除方法。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-187">An attribute specified on an event declaration that omits event accessors can apply to the event being declared, to the associated field (if the event is not abstract), or to the associated add and remove methods.</span></span> <span data-ttu-id="d1aa3-188">如果没有*attribute_target_specifier*，该特性应用于该事件。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-188">In the absence of an *attribute_target_specifier*, the attribute applies to the event.</span></span> <span data-ttu-id="d1aa3-189">是否存在`event` *attribute_target_specifier*指示该特性应用于事件; 是否存在`field` *attribute_target_specifier*指示该属性应用于字段;是否存在`method` *attribute_target_specifier*指示该特性应用于方法。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-189">The presence of the `event` *attribute_target_specifier* indicates that the attribute applies to the event; the presence of the `field` *attribute_target_specifier* indicates that the attribute applies to the field; and the presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the methods.</span></span>
*  <span data-ttu-id="d1aa3-190">在属性或索引器声明上的 get 访问器声明上指定的属性可以应用到关联的方法或其返回值。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-190">An attribute specified on a get accessor declaration for a property or indexer declaration can apply either to the associated method or to its return value.</span></span> <span data-ttu-id="d1aa3-191">如果没有*attribute_target_specifier*，该特性应用于该方法。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-191">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="d1aa3-192">是否存在`method` *attribute_target_specifier*指示该特性应用于方法; 是否存在`return` *attribute_target_specifier*指示该特性所应用于返回值。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-192">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="d1aa3-193">Set 访问器属性或索引器声明上指定的属性可以应用到关联的方法或其单独的隐式参数。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-193">An attribute specified on a set accessor for a property or indexer declaration can apply either to the associated method or to its lone implicit parameter.</span></span> <span data-ttu-id="d1aa3-194">如果没有*attribute_target_specifier*，该特性应用于该方法。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-194">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="d1aa3-195">是否存在`method` *attribute_target_specifier*指示该特性应用于方法; 是否存在`param` *attribute_target_specifier*指示此特性应用于参数;是否存在`return` *attribute_target_specifier*指示该特性应用于返回值。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-195">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `param` *attribute_target_specifier* indicates that the attribute applies to the parameter; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="d1aa3-196">为事件声明可以应用于关联的方法或其单独的参数指定在 add 或 remove 访问器声明属性。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-196">An attribute specified on an add or remove accessor declaration for an event declaration can apply either to the associated method or to its lone parameter.</span></span> <span data-ttu-id="d1aa3-197">如果没有*attribute_target_specifier*，该特性应用于该方法。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-197">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="d1aa3-198">是否存在`method` *attribute_target_specifier*指示该特性应用于方法; 是否存在`param` *attribute_target_specifier*指示此特性应用于参数;是否存在`return` *attribute_target_specifier*指示该特性应用于返回值。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-198">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `param` *attribute_target_specifier* indicates that the attribute applies to the parameter; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>

<span data-ttu-id="d1aa3-199">在其他上下文中，包含*attribute_target_specifier*允许但不必要。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-199">In other contexts, inclusion of an *attribute_target_specifier* is permitted but unnecessary.</span></span> <span data-ttu-id="d1aa3-200">例如，类声明可能包括或省略说明符`type`:</span><span class="sxs-lookup"><span data-stu-id="d1aa3-200">For instance, a class declaration may either include or omit the specifier `type`:</span></span>

```csharp
[type: Author("Brian Kernighan")]
class Class1 {}

[Author("Dennis Ritchie")]
class Class2 {}
```

<span data-ttu-id="d1aa3-201">它是错误指定了一个无效*attribute_target_specifier*。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-201">It is an error to specify an invalid *attribute_target_specifier*.</span></span> <span data-ttu-id="d1aa3-202">例如，说明符`param`不能在类声明中使用：</span><span class="sxs-lookup"><span data-stu-id="d1aa3-202">For instance, the specifier `param` cannot be used on a class declaration:</span></span>

```csharp
[param: Author("Brian Kernighan")]        // Error
class Class1 {}
```

<span data-ttu-id="d1aa3-203">按照约定，特性类命名为后缀`Attribute`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-203">By convention, attribute classes are named with a suffix of `Attribute`.</span></span> <span data-ttu-id="d1aa3-204">*Attribute_name*窗体*type_name*可以包括或省略此后缀。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-204">An *attribute_name* of the form *type_name* may either include or omit this suffix.</span></span> <span data-ttu-id="d1aa3-205">如果使用或不带此后缀找到特性类，则产生歧义，且会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-205">If an attribute class is found both with and without this suffix, an ambiguity is present, and a compile-time error results.</span></span> <span data-ttu-id="d1aa3-206">如果*attribute_name*拼写，以便其右*标识符*是逐字字符串标识符 ([标识符](lexical-structure.md#identifiers))，则只能将不带后缀的属性匹配时，从而使这种多义性会得到解决。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-206">If the *attribute_name* is spelled such that its right-most *identifier* is a verbatim identifier ([Identifiers](lexical-structure.md#identifiers)), then only an attribute without a suffix is matched, thus enabling such an ambiguity to be resolved.</span></span> <span data-ttu-id="d1aa3-207">该示例</span><span class="sxs-lookup"><span data-stu-id="d1aa3-207">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class X: Attribute
{}

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Error: ambiguity
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Refers to X
class Class3 {}

[@XAttribute]           // Refers to XAttribute
class Class4 {}
```

<span data-ttu-id="d1aa3-208">显示两个属性名为的类`X`和`XAttribute`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-208">shows two attribute classes named `X` and `XAttribute`.</span></span> <span data-ttu-id="d1aa3-209">该属性`[X]`不明确，因为它可以为引用`X`或`XAttribute`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-209">The attribute `[X]` is ambiguous, since it could refer to either `X` or `XAttribute`.</span></span> <span data-ttu-id="d1aa3-210">通过使用逐字字符串标识符，在这种极少数情况下指定确切的意图。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-210">Using a verbatim identifier allows the exact intent to be specified in such rare cases.</span></span> <span data-ttu-id="d1aa3-211">该属性`[XAttribute]`不是不明确 (尽管它会很是否有一个名为属性类`XAttributeAttribute`！)。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-211">The attribute `[XAttribute]` is not ambiguous (although it would be if there was an attribute class named `XAttributeAttribute`!).</span></span> <span data-ttu-id="d1aa3-212">如果类的声明`X`被删除，则这两个属性引用名为的特性类`XAttribute`，按如下所示：</span><span class="sxs-lookup"><span data-stu-id="d1aa3-212">If the declaration for class `X` is removed, then both attributes refer to the attribute class named `XAttribute`, as follows:</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Refers to XAttribute
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Error: no attribute named "X"
class Class3 {}
```

<span data-ttu-id="d1aa3-213">它是相同的实体超过一次使用单用途特性类的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-213">It is a compile-time error to use a single-use attribute class more than once on the same entity.</span></span> <span data-ttu-id="d1aa3-214">该示例</span><span class="sxs-lookup"><span data-stu-id="d1aa3-214">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpStringAttribute: Attribute
{
    string value;

    public HelpStringAttribute(string value) {
        this.value = value;
    }

    public string Value {
        get {...}
    }
}

[HelpString("Description of Class1")]
[HelpString("Another description of Class1")]
public class Class1 {}
```

<span data-ttu-id="d1aa3-215">因为它尝试使用会导致编译时错误`HelpString`，它是一个单次使用的特性类，一次以上的声明上`Class1`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-215">results in a compile-time error because it attempts to use `HelpString`, which is a single-use attribute class, more than once on the declaration of `Class1`.</span></span>

<span data-ttu-id="d1aa3-216">一个表达式`E`是*attribute_argument_expression*如果满足所有以下语句：</span><span class="sxs-lookup"><span data-stu-id="d1aa3-216">An expression `E` is an *attribute_argument_expression* if all of the following statements are true:</span></span>

*  <span data-ttu-id="d1aa3-217">类型`E`特性参数类型 ([属性参数类型](attributes.md#attribute-parameter-types))。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-217">The type of `E` is an attribute parameter type ([Attribute parameter types](attributes.md#attribute-parameter-types)).</span></span>
*  <span data-ttu-id="d1aa3-218">在编译时，值`E`可以解析为以下值之一：</span><span class="sxs-lookup"><span data-stu-id="d1aa3-218">At compile-time, the value of `E` can be resolved to one of the following:</span></span>
   * <span data-ttu-id="d1aa3-219">常量的值。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-219">A constant value.</span></span>
   * <span data-ttu-id="d1aa3-220">一个 `System.Type` 对象。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-220">A `System.Type` object.</span></span>
   * <span data-ttu-id="d1aa3-221">一维数组*attribute_argument_expression*s。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-221">A one-dimensional array of *attribute_argument_expression*s.</span></span>

<span data-ttu-id="d1aa3-222">例如：</span><span class="sxs-lookup"><span data-stu-id="d1aa3-222">For example:</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class TestAttribute: Attribute
{
    public int P1 {
        get {...}
        set {...}
    }

    public Type P2 {
        get {...}
        set {...}
    }

    public object P3 {
        get {...}
        set {...}
    }
}

[Test(P1 = 1234, P3 = new int[] {1, 3, 5}, P2 = typeof(float))]
class MyClass {}
```

<span data-ttu-id="d1aa3-223">一个*typeof_expression* ([typeof 运算符](expressions.md#the-typeof-operator)) 用作属性参数表达式可以引用非泛型类型、 封闭式构造的类型或未绑定的泛型类型，但它不能引用开放类型。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-223">A *typeof_expression* ([The typeof operator](expressions.md#the-typeof-operator)) used as an attribute argument expression can reference a non-generic type, a closed constructed type, or an unbound generic type, but it cannot reference an open type.</span></span> <span data-ttu-id="d1aa3-224">这是为了确保可以在编译时解析表达式。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-224">This is to ensure that the expression can be resolved at compile-time.</span></span>

```csharp
class A: Attribute
{
    public A(Type t) {...}
}

class G<T>
{
    [A(typeof(T))] T t;                  // Error, open type in attribute
}

class X
{
    [A(typeof(List<int>))] int x;        // Ok, closed constructed type
    [A(typeof(List<>))] int y;           // Ok, unbound generic type
}
```

## <a name="attribute-instances"></a><span data-ttu-id="d1aa3-225">特性实例</span><span class="sxs-lookup"><span data-stu-id="d1aa3-225">Attribute instances</span></span>

<span data-ttu-id="d1aa3-226">***特性实例***是表示在运行时的特性的实例。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-226">An ***attribute instance*** is an instance that represents an attribute at run-time.</span></span> <span data-ttu-id="d1aa3-227">属性是使用属性类，位置参数，定义和命名参数。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-227">An attribute is defined with an attribute class, positional arguments, and named arguments.</span></span> <span data-ttu-id="d1aa3-228">一个特性实例是使用位置和命名参数初始化特性类的实例。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-228">An attribute instance is an instance of the attribute class that is initialized with the positional and named arguments.</span></span>

<span data-ttu-id="d1aa3-229">检索一个特性实例涉及编译时和运行时处理，如以下各节中所述。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-229">Retrieval of an attribute instance involves both compile-time and run-time processing, as described in the following sections.</span></span>

### <a name="compilation-of-an-attribute"></a><span data-ttu-id="d1aa3-230">属性的编译</span><span class="sxs-lookup"><span data-stu-id="d1aa3-230">Compilation of an attribute</span></span>

<span data-ttu-id="d1aa3-231">编译*特性*特性类与`T`， *positional_argument_list* `P`并*named_argument_list* `N`，包括以下步骤：</span><span class="sxs-lookup"><span data-stu-id="d1aa3-231">The compilation of an *attribute* with attribute class `T`, *positional_argument_list* `P` and *named_argument_list* `N`, consists of the following steps:</span></span>

*  <span data-ttu-id="d1aa3-232">请按照用于编译的编译时处理步骤*object_creation_expression*窗体的`new T(P)`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-232">Follow the compile-time processing steps for compiling an *object_creation_expression* of the form `new T(P)`.</span></span> <span data-ttu-id="d1aa3-233">这些步骤导致编译时错误，或确定实例构造函数`C`上`T`，可以在运行时调用。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-233">These steps either result in a compile-time error, or determine an instance constructor `C` on `T` that can be invoked at run-time.</span></span>
*  <span data-ttu-id="d1aa3-234">如果`C`不具有公共可访问性，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-234">If `C` does not have public accessibility, then a compile-time error occurs.</span></span>
*  <span data-ttu-id="d1aa3-235">每个*named_argument* `Arg`中`N`:</span><span class="sxs-lookup"><span data-stu-id="d1aa3-235">For each *named_argument* `Arg` in `N`:</span></span>
   * <span data-ttu-id="d1aa3-236">让`Name`进行*标识符*的*named_argument* `Arg`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-236">Let `Name` be the *identifier* of the *named_argument* `Arg`.</span></span>
   * <span data-ttu-id="d1aa3-237">`Name` 必须标识的非静态读写公共字段或属性上`T`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-237">`Name` must identify a non-static read-write public field or property on `T`.</span></span> <span data-ttu-id="d1aa3-238">如果`T`有无此类字段或属性，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-238">If `T` has no such field or property, then a compile-time error occurs.</span></span>
*  <span data-ttu-id="d1aa3-239">保留的该属性的运行时实例化的以下信息： 特性类`T`，实例构造函数`C`上`T`，则*positional_argument_list* `P`并*named_argument_list* `N`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-239">Keep the following information for run-time instantiation of the attribute: the attribute class `T`, the instance constructor `C` on `T`, the *positional_argument_list* `P` and the *named_argument_list* `N`.</span></span>

### <a name="run-time-retrieval-of-an-attribute-instance"></a><span data-ttu-id="d1aa3-240">运行时检索的特性实例</span><span class="sxs-lookup"><span data-stu-id="d1aa3-240">Run-time retrieval of an attribute instance</span></span>

<span data-ttu-id="d1aa3-241">编译*特性*产生一个属性类`T`，实例构造函数`C`上`T`、 一个*positional_argument_list* `P`，和一个*named_argument_list* `N`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-241">Compilation of an *attribute* yields an attribute class `T`, an instance constructor `C` on `T`, a *positional_argument_list* `P`, and a *named_argument_list* `N`.</span></span> <span data-ttu-id="d1aa3-242">提供此信息，一个特性实例，可以检索在运行时使用以下步骤：</span><span class="sxs-lookup"><span data-stu-id="d1aa3-242">Given this information, an attribute instance can be retrieved at run-time using the following steps:</span></span>

*  <span data-ttu-id="d1aa3-243">请按照用于执行运行时处理步骤*object_creation_expression*窗体`new T(P)`，使用的实例构造函数`C`在编译时确定。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-243">Follow the run-time processing steps for executing an *object_creation_expression* of the form `new T(P)`, using the instance constructor `C` as determined at compile-time.</span></span> <span data-ttu-id="d1aa3-244">这些步骤导致异常，或生成一个实例`O`的`T`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-244">These steps either result in an exception, or produce an instance `O` of `T`.</span></span>
*  <span data-ttu-id="d1aa3-245">每个*named_argument* `Arg`中`N`，按顺序：</span><span class="sxs-lookup"><span data-stu-id="d1aa3-245">For each *named_argument* `Arg` in `N`, in order:</span></span>
   * <span data-ttu-id="d1aa3-246">让`Name`进行*标识符*的*named_argument* `Arg`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-246">Let `Name` be the *identifier* of the *named_argument* `Arg`.</span></span> <span data-ttu-id="d1aa3-247">如果`Name`不会在标识的非静态公共读写字段或属性`O`，则会引发异常。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-247">If `Name` does not identify a non-static public read-write field or property on `O`, then an exception is thrown.</span></span>
   * <span data-ttu-id="d1aa3-248">让`Value`进行的计算结果*attribute_argument_expression*的`Arg`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-248">Let `Value` be the result of evaluating the *attribute_argument_expression* of `Arg`.</span></span>
   * <span data-ttu-id="d1aa3-249">如果`Name`标识的字段上`O`，然后将此字段设置为`Value`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-249">If `Name` identifies a field on `O`, then set this field to `Value`.</span></span>
   * <span data-ttu-id="d1aa3-250">否则为`Name`标识属性上`O`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-250">Otherwise, `Name` identifies a property on `O`.</span></span> <span data-ttu-id="d1aa3-251">将此属性设置为`Value`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-251">Set this property to `Value`.</span></span>
   * <span data-ttu-id="d1aa3-252">结果是`O`，特性类的实例`T`已初始化， *positional_argument_list* `P`并*named_argument_list*`N`.</span><span class="sxs-lookup"><span data-stu-id="d1aa3-252">The result is `O`, an instance of the attribute class `T` that has been initialized with the *positional_argument_list* `P` and the *named_argument_list* `N`.</span></span>

## <a name="reserved-attributes"></a><span data-ttu-id="d1aa3-253">保留的属性</span><span class="sxs-lookup"><span data-stu-id="d1aa3-253">Reserved attributes</span></span>

<span data-ttu-id="d1aa3-254">少量的属性会影响以某种方式的语言。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-254">A small number of attributes affect the language in some way.</span></span> <span data-ttu-id="d1aa3-255">这些属性包括：</span><span class="sxs-lookup"><span data-stu-id="d1aa3-255">These attributes include:</span></span>

*  <span data-ttu-id="d1aa3-256">`System.AttributeUsageAttribute` ([AttributeUsage 特性](attributes.md#the-attributeusage-attribute))，用于描述了可以在其中使用的特性类的方法。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-256">`System.AttributeUsageAttribute` ([The AttributeUsage attribute](attributes.md#the-attributeusage-attribute)), which is used to describe the ways in which an attribute class can be used.</span></span>
*  <span data-ttu-id="d1aa3-257">`System.Diagnostics.ConditionalAttribute` ([Conditional 特性](attributes.md#the-conditional-attribute))，用于定义条件的方法。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-257">`System.Diagnostics.ConditionalAttribute` ([The Conditional attribute](attributes.md#the-conditional-attribute)), which is used to define conditional methods.</span></span>
*  <span data-ttu-id="d1aa3-258">`System.ObsoleteAttribute` ([Obsolete 特性](attributes.md#the-obsolete-attribute))，用于将成员标记为已过时。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-258">`System.ObsoleteAttribute` ([The Obsolete attribute](attributes.md#the-obsolete-attribute)), which is used to mark a member as obsolete.</span></span>
*  <span data-ttu-id="d1aa3-259">`System.Runtime.CompilerServices.CallerLineNumberAttribute``System.Runtime.CompilerServices.CallerFilePathAttribute`并`System.Runtime.CompilerServices.CallerMemberNameAttribute`([调用方信息特性](attributes.md#caller-info-attributes))，用于提供有关于可选参数的调用上下文的信息。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-259">`System.Runtime.CompilerServices.CallerLineNumberAttribute`, `System.Runtime.CompilerServices.CallerFilePathAttribute` and `System.Runtime.CompilerServices.CallerMemberNameAttribute` ([Caller info attributes](attributes.md#caller-info-attributes)), which are used to supply information about the calling context to optional parameters.</span></span>

### <a name="the-attributeusage-attribute"></a><span data-ttu-id="d1aa3-260">AttributeUsage 特性</span><span class="sxs-lookup"><span data-stu-id="d1aa3-260">The AttributeUsage attribute</span></span>

<span data-ttu-id="d1aa3-261">该属性`AttributeUsage`用于描述可以在其中使用特性类的方式。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-261">The attribute `AttributeUsage` is used to describe the manner in which the attribute class can be used.</span></span>

<span data-ttu-id="d1aa3-262">使用修饰的类`AttributeUsage`属性必须派生自`System.Attribute`，直接或间接。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-262">A class that is decorated with the `AttributeUsage` attribute must derive from `System.Attribute`, either directly or indirectly.</span></span> <span data-ttu-id="d1aa3-263">否则，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-263">Otherwise, a compile-time error occurs.</span></span>

```csharp
namespace System
{
    [AttributeUsage(AttributeTargets.Class)]
    public class AttributeUsageAttribute: Attribute
    {
        public AttributeUsageAttribute(AttributeTargets validOn) {...}
        public virtual bool AllowMultiple { get {...} set {...} }
        public virtual bool Inherited { get {...} set {...} }
        public virtual AttributeTargets ValidOn { get {...} }
    }

    public enum AttributeTargets
    {
        Assembly     = 0x0001,
        Module       = 0x0002,
        Class        = 0x0004,
        Struct       = 0x0008,
        Enum         = 0x0010,
        Constructor  = 0x0020,
        Method       = 0x0040,
        Property     = 0x0080,
        Field        = 0x0100,
        Event        = 0x0200,
        Interface    = 0x0400,
        Parameter    = 0x0800,
        Delegate     = 0x1000,
        ReturnValue  = 0x2000,

        All = Assembly | Module | Class | Struct | Enum | Constructor | 
            Method | Property | Field | Event | Interface | Parameter | 
            Delegate | ReturnValue
    }
}
```

### <a name="the-conditional-attribute"></a><span data-ttu-id="d1aa3-264">Conditional 特性</span><span class="sxs-lookup"><span data-stu-id="d1aa3-264">The Conditional attribute</span></span>

<span data-ttu-id="d1aa3-265">该属性`Conditional`，您可以***条件方法***并***条件特性类***。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-265">The attribute `Conditional` enables the definition of ***conditional methods*** and ***conditional attribute classes***.</span></span>

```csharp
namespace System.Diagnostics
{
    [AttributeUsage(AttributeTargets.Method | AttributeTargets.Class, AllowMultiple = true)]
    public class ConditionalAttribute: Attribute
    {
        public ConditionalAttribute(string conditionString) {...}
        public string ConditionString { get {...} }
    }
}
```

#### <a name="conditional-methods"></a><span data-ttu-id="d1aa3-266">条件方法</span><span class="sxs-lookup"><span data-stu-id="d1aa3-266">Conditional methods</span></span>

<span data-ttu-id="d1aa3-267">一种方法使用修饰`Conditional`属性是有条件的方法。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-267">A method decorated with the `Conditional` attribute is a conditional method.</span></span> <span data-ttu-id="d1aa3-268">`Conditional`属性指示条件通过测试的条件编译符号。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-268">The `Conditional` attribute indicates a condition by testing a conditional compilation symbol.</span></span> <span data-ttu-id="d1aa3-269">对条件方法的调用是包含或省略具体取决于是否在调用时能够定义此符号。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-269">Calls to a conditional method are either included or omitted depending on whether this symbol is defined at the point of the call.</span></span> <span data-ttu-id="d1aa3-270">如果定义了符号，则将包括调用;否则，省略 （包括评估的接收方和调用的参数） 的调用。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-270">If the symbol is defined, the call is included; otherwise, the call (including evaluation of the receiver and parameters of the call) is omitted.</span></span>

<span data-ttu-id="d1aa3-271">条件方法受到以下限制：</span><span class="sxs-lookup"><span data-stu-id="d1aa3-271">A conditional method is subject to the following restrictions:</span></span>

*  <span data-ttu-id="d1aa3-272">条件方法必须是中的方法*class_declaration*或*struct_declaration*。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-272">The conditional method must be a method in a *class_declaration* or *struct_declaration*.</span></span> <span data-ttu-id="d1aa3-273">如果会发生编译时错误`Conditional`接口声明中的方法上指定属性。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-273">A compile-time error occurs if the `Conditional` attribute is specified on a method in an interface declaration.</span></span>
*  <span data-ttu-id="d1aa3-274">条件方法必须具有返回类型为`void`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-274">The conditional method must have a return type of `void`.</span></span>
*  <span data-ttu-id="d1aa3-275">条件方法必须未标有`override`修饰符。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-275">The conditional method must not be marked with the `override` modifier.</span></span> <span data-ttu-id="d1aa3-276">可以使用标记，条件方法`virtual`修饰符，但是。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-276">A conditional method may be marked with the `virtual` modifier, however.</span></span> <span data-ttu-id="d1aa3-277">此类方法的重写是隐式条件元素，不能显式标记与`Conditional`属性。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-277">Overrides of such a method are implicitly conditional, and must not be explicitly marked with a `Conditional` attribute.</span></span>
*  <span data-ttu-id="d1aa3-278">条件方法不得接口方法的实现。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-278">The conditional method must not be an implementation of an interface method.</span></span> <span data-ttu-id="d1aa3-279">否则，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-279">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="d1aa3-280">如果条件方法中使用，此外，发生编译时错误*delegate_creation_expression*。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-280">In addition, a compile-time error occurs if a conditional method is used in a *delegate_creation_expression*.</span></span> <span data-ttu-id="d1aa3-281">该示例</span><span class="sxs-lookup"><span data-stu-id="d1aa3-281">The example</span></span>

```csharp
#define DEBUG

using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void M() {
        Console.WriteLine("Executed Class1.M");
    }
}

class Class2
{
    public static void Test() {
        Class1.M();
    }
}
```

<span data-ttu-id="d1aa3-282">声明`Class1.M`作为条件方法。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-282">declares `Class1.M` as a conditional method.</span></span> <span data-ttu-id="d1aa3-283">`Class2``Test`方法调用此方法。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-283">`Class2`'s `Test` method calls this method.</span></span> <span data-ttu-id="d1aa3-284">由于条件编译符号`DEBUG`定义，如果`Class2.Test`是调用，它将调用`M`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-284">Since the conditional compilation symbol `DEBUG` is defined, if `Class2.Test` is called, it will call `M`.</span></span> <span data-ttu-id="d1aa3-285">如果符号`DEBUG`有未定义，然后`Class2.Test`不会调用`Class1.M`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-285">If the symbol `DEBUG` had not been defined, then `Class2.Test` would not call `Class1.M`.</span></span>

<span data-ttu-id="d1aa3-286">请务必注意，包含或排除条件方法调用受进行调用的条件编译符号。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-286">It is important to note that the inclusion or exclusion of a call to a conditional method is controlled by the conditional compilation symbols at the point of the call.</span></span> <span data-ttu-id="d1aa3-287">在示例</span><span class="sxs-lookup"><span data-stu-id="d1aa3-287">In the example</span></span>

<span data-ttu-id="d1aa3-288">文件`class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="d1aa3-288">File `class1.cs`:</span></span>

```csharp
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void F() {
        Console.WriteLine("Executed Class1.F");
    }
}
```

<span data-ttu-id="d1aa3-289">文件`class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="d1aa3-289">File `class2.cs`:</span></span>

```csharp
#define DEBUG

class Class2
{
    public static void G() {
        Class1.F();                // F is called
    }
}
```

<span data-ttu-id="d1aa3-290">文件`class3.cs`:</span><span class="sxs-lookup"><span data-stu-id="d1aa3-290">File `class3.cs`:</span></span>

```csharp
#undef DEBUG

class Class3
{
    public static void H() {
        Class1.F();                // F is not called
    }
}
```

<span data-ttu-id="d1aa3-291">类`Class2`并`Class3`每个包含对条件方法的调用`Class1.F`，这是有条件基于是否`DEBUG`定义。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-291">the classes `Class2` and `Class3` each contain calls to the conditional method `Class1.F`, which is conditional based on whether or not `DEBUG` is defined.</span></span> <span data-ttu-id="d1aa3-292">由于的上下文中定义此符号`Class2`但不是`Class3`，在调用`F`中`Class2`包括，则在调用时`F`中`Class3`省略。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-292">Since this symbol is defined in the context of `Class2` but not `Class3`, the call to `F` in `Class2` is included, while the call to `F` in `Class3` is omitted.</span></span>

<span data-ttu-id="d1aa3-293">使用继承链中的条件方法可以是令人困惑。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-293">The use of conditional methods in an inheritance chain can be confusing.</span></span> <span data-ttu-id="d1aa3-294">对通过条件方法的调用`base`，窗体的`base.M`，可能会有所条件的普通方法调用规则。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-294">Calls made to a conditional method through `base`, of the form `base.M`, are subject to the normal conditional method call rules.</span></span> <span data-ttu-id="d1aa3-295">在示例</span><span class="sxs-lookup"><span data-stu-id="d1aa3-295">In the example</span></span>

<span data-ttu-id="d1aa3-296">文件`class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="d1aa3-296">File `class1.cs`:</span></span>

```csharp
using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public virtual void M() {
        Console.WriteLine("Class1.M executed");
    }
}
```

<span data-ttu-id="d1aa3-297">文件`class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="d1aa3-297">File `class2.cs`:</span></span>

```csharp
using System;

class Class2: Class1
{
    public override void M() {
        Console.WriteLine("Class2.M executed");
        base.M();                        // base.M is not called!
    }
}
```

<span data-ttu-id="d1aa3-298">文件`class3.cs`:</span><span class="sxs-lookup"><span data-stu-id="d1aa3-298">File `class3.cs`:</span></span>

```csharp
#define DEBUG

using System;

class Class3
{
    public static void Test() {
        Class2 c = new Class2();
        c.M();                            // M is called
    }
}
```

<span data-ttu-id="d1aa3-299">`Class2` 包含对的调用`M`其基类中定义。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-299">`Class2` includes a call to the `M` defined in its base class.</span></span> <span data-ttu-id="d1aa3-300">此调用省略，因为基方法是有条件根据的符号是否存在`DEBUG`，这是未定义。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-300">This call is omitted because the base method is conditional based on the presence of the symbol `DEBUG`, which is undefined.</span></span> <span data-ttu-id="d1aa3-301">因此，该方法将写入控制台"`Class2.M executed`"仅。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-301">Thus, the method writes to the console "`Class2.M executed`" only.</span></span> <span data-ttu-id="d1aa3-302">明智地使用*pp_declaration*s 可以消除此类问题。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-302">Judicious use of *pp_declaration*s can eliminate such problems.</span></span>

#### <a name="conditional-attribute-classes"></a><span data-ttu-id="d1aa3-303">条件属性类</span><span class="sxs-lookup"><span data-stu-id="d1aa3-303">Conditional attribute classes</span></span>

<span data-ttu-id="d1aa3-304">特性类 ([属性类](attributes.md#attribute-classes)) 使用一个或多个修饰`Conditional`属性是***conditional 特性类***。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-304">An attribute class ([Attribute classes](attributes.md#attribute-classes)) decorated with one or more `Conditional` attributes is a ***conditional attribute class***.</span></span> <span data-ttu-id="d1aa3-305">Conditional 特性类是因此与相关联的条件编译符号中声明其`Conditional`属性。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-305">A conditional attribute class is thus associated with the conditional compilation symbols declared in its `Conditional` attributes.</span></span> <span data-ttu-id="d1aa3-306">此示例中：</span><span class="sxs-lookup"><span data-stu-id="d1aa3-306">This example:</span></span>

```csharp
using System;
using System.Diagnostics;
[Conditional("ALPHA")]
[Conditional("BETA")]
public class TestAttribute : Attribute {}
```

<span data-ttu-id="d1aa3-307">声明`TestAttribute`作为条件属性类与条件编译符号`ALPHA`和`BETA`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-307">declares `TestAttribute` as a conditional attribute class associated with the conditional compilations symbols `ALPHA` and `BETA`.</span></span>

<span data-ttu-id="d1aa3-308">属性规范 ([属性规范](attributes.md#attribute-specification)) 的条件属性都将包含如果一个或多个其关联的条件编译符号定义在规范中，否则该属性省略规范。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-308">Attribute specifications ([Attribute specification](attributes.md#attribute-specification)) of a conditional attribute are included if one or more of its associated conditional compilation symbols is defined at the point of specification, otherwise the attribute specification is omitted.</span></span>

<span data-ttu-id="d1aa3-309">请务必注意，包含或排除条件特性类的属性规范受该规范所在位置的条件编译符号。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-309">It is important to note that the inclusion or exclusion of an attribute specification of a conditional attribute class is controlled by the conditional compilation symbols at the point of the specification.</span></span> <span data-ttu-id="d1aa3-310">在示例</span><span class="sxs-lookup"><span data-stu-id="d1aa3-310">In the example</span></span>

<span data-ttu-id="d1aa3-311">文件`test.cs`:</span><span class="sxs-lookup"><span data-stu-id="d1aa3-311">File `test.cs`:</span></span>

```csharp
using System;
using System.Diagnostics;

[Conditional("DEBUG")]

public class TestAttribute : Attribute {}
```

<span data-ttu-id="d1aa3-312">文件`class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="d1aa3-312">File `class1.cs`:</span></span>

```csharp
#define DEBUG

[Test]                // TestAttribute is specified

class Class1 {}
```

<span data-ttu-id="d1aa3-313">文件`class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="d1aa3-313">File `class2.cs`:</span></span>

```csharp
#undef DEBUG

[Test]                 // TestAttribute is not specified

class Class2 {}
```

<span data-ttu-id="d1aa3-314">类`Class1`并`Class2`是每个使用属性修饰`Test`，这是有条件基于是否`DEBUG`定义。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-314">the classes `Class1` and `Class2` are each decorated with attribute `Test`, which is conditional based on whether or not `DEBUG` is defined.</span></span> <span data-ttu-id="d1aa3-315">由于的上下文中定义此符号`Class1`但不是`Class2`的规范`Test`特性，可以在`Class1`包含，时的规范`Test`特性，可以在`Class2`省略。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-315">Since this symbol is defined in the context of `Class1` but not `Class2`, the specification of the `Test` attribute on `Class1` is included, while the specification of the `Test` attribute on `Class2` is omitted.</span></span>

### <a name="the-obsolete-attribute"></a><span data-ttu-id="d1aa3-316">Obsolete 特性</span><span class="sxs-lookup"><span data-stu-id="d1aa3-316">The Obsolete attribute</span></span>

<span data-ttu-id="d1aa3-317">该属性`Obsolete`用于标记应不再使用的类型的类型和成员。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-317">The attribute `Obsolete` is used to mark types and members of types that should no longer be used.</span></span>

```csharp
namespace System
{
    [AttributeUsage(
        AttributeTargets.Class | 
        AttributeTargets.Struct |
        AttributeTargets.Enum | 
        AttributeTargets.Interface | 
        AttributeTargets.Delegate |
        AttributeTargets.Method | 
        AttributeTargets.Constructor |
        AttributeTargets.Property | 
        AttributeTargets.Field |
        AttributeTargets.Event,
        Inherited = false)
    ]
    public class ObsoleteAttribute: Attribute
    {
        public ObsoleteAttribute() {...}
        public ObsoleteAttribute(string message) {...}
        public ObsoleteAttribute(string message, bool error) {...}
        public string Message { get {...} }
        public bool IsError { get {...} }
    }
}
```

<span data-ttu-id="d1aa3-318">如果程序使用的类型或成员，使用修饰`Obsolete`属性，则编译器会发出警告或错误。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-318">If a program uses a type or member that is decorated with the `Obsolete` attribute, the compiler issues a warning or an error.</span></span> <span data-ttu-id="d1aa3-319">具体而言，编译器会发出警告如果没有提供任何错误参数，或如果错误参数提供了，并且具有值`false`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-319">Specifically, the compiler issues a warning if no error parameter is provided, or if the error parameter is provided and has the value `false`.</span></span> <span data-ttu-id="d1aa3-320">如果指定了错误参数，并且具有值，则编译器会发出错误`true`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-320">The compiler issues an error if the error parameter is specified and has the value `true`.</span></span>

<span data-ttu-id="d1aa3-321">在示例</span><span class="sxs-lookup"><span data-stu-id="d1aa3-321">In the example</span></span>

```csharp
[Obsolete("This class is obsolete; use class B instead")]
class A
{
    public void F() {}
}

class B
{
    public void F() {}
}

class Test
{
    static void Main() {
        A a = new A();         // Warning
        a.F();
    }
}
```

<span data-ttu-id="d1aa3-322">该类`A`用修饰`Obsolete`属性。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-322">the class `A` is decorated with the `Obsolete` attribute.</span></span> <span data-ttu-id="d1aa3-323">每次使用都`A`在`Main`导致一条警告，包括指定的消息，"此类已过时;改为使用类 B。"</span><span class="sxs-lookup"><span data-stu-id="d1aa3-323">Each use of `A` in `Main` results in a warning that includes the specified message, "This class is obsolete; use class B instead."</span></span>

### <a name="caller-info-attributes"></a><span data-ttu-id="d1aa3-324">调用方信息属性</span><span class="sxs-lookup"><span data-stu-id="d1aa3-324">Caller info attributes</span></span>

<span data-ttu-id="d1aa3-325">对于如日志记录和报告目的，是有时很有用的函数成员，才能获取有关调用代码的某些编译时信息。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-325">For purposes such as logging and reporting, it is sometimes useful for a function member to obtain certain compile-time information about the calling code.</span></span> <span data-ttu-id="d1aa3-326">调用方信息特性提供一种方法来以透明方式传递此类信息。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-326">The caller info attributes provide a way to pass such information transparently.</span></span>

<span data-ttu-id="d1aa3-327">一个可选参数，加上批注与调用方信息特性之一，省略的调用中的相应参数不一定会导致要替换的默认参数值。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-327">When an optional parameter is annotated with one of the caller info attributes, omitting the corresponding argument in a call does not necessarily cause the default parameter value to be substituted.</span></span> <span data-ttu-id="d1aa3-328">相反，如果有关调用上下文的指定的信息不可用，该信息将作为参数值传递。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-328">Instead, if the specified information about the calling context is available, that information will be passed as the argument value.</span></span>

<span data-ttu-id="d1aa3-329">例如：</span><span class="sxs-lookup"><span data-stu-id="d1aa3-329">For example:</span></span>

```csharp
using System.Runtime.CompilerServices

...

public void Log(
    [CallerLineNumber] int line = -1,
    [CallerFilePath]   string path = null,
    [CallerMemberName] string name = null
)
{
    Console.WriteLine((line < 0) ? "No line" : "Line "+ line);
    Console.WriteLine((path == null) ? "No file path" : path);
    Console.WriteLine((name == null) ? "No member name" : name);
}
```

<span data-ttu-id="d1aa3-330">调用`Log()`不带任何参数将打印行号和文件路径的调用，以及在其中发生在调用的成员的名称。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-330">A call to `Log()` with no arguments would print the line number and file path of the call, as well as the name of the member within which the call occurred.</span></span>

<span data-ttu-id="d1aa3-331">调用方信息特性上可以出现任意位置，可选参数包括在委托声明中。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-331">Caller info attributes can occur on optional parameters anywhere, including in delegate declarations.</span></span> <span data-ttu-id="d1aa3-332">但是，特定的调用方信息特性限制对有可以属性的参数的类型，以便将始终从被替换的值为参数类型的隐式转换。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-332">However, the specific caller info attributes have restrictions on the types of the parameters they can attribute, so that there will always be an implicit conversion from a substituted value to the parameter type.</span></span>

<span data-ttu-id="d1aa3-333">它是错误的同时定义和实现分部方法声明的一部分的参数具有相同的调用方信息属性。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-333">It is an error to have the same caller info attribute on a parameter of both the defining and implementing part of a partial method declaration.</span></span> <span data-ttu-id="d1aa3-334">应用仅在定义的一部分的调用方信息特性，而忽略调用方信息特性只能在实现部分中发生。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-334">Only caller info attributes in the defining part are applied, whereas caller info attributes occurring only in the implementing part are ignored.</span></span>

<span data-ttu-id="d1aa3-335">调用方信息不会影响重载决策。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-335">Caller information does not affect overload resolution.</span></span> <span data-ttu-id="d1aa3-336">重载决策特性化的可选参数仍从调用方的源代码中省略，因为它会忽略其他省略可选参数的方式相同忽略这些参数 ([重载决策](expressions.md#overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-336">As the attributed optional parameters are still omitted from the source code of the caller, overload resolution ignores those parameters in the same way it ignores other omitted optional parameters ([Overload resolution](expressions.md#overload-resolution)).</span></span>

<span data-ttu-id="d1aa3-337">在源代码中显式调用函数时，将仅替换调用方信息。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-337">Caller information is only substituted when a function is explicitly invoked in source code.</span></span> <span data-ttu-id="d1aa3-338">例如隐式父构造函数调用的隐式调用没有源位置，将不替换调用方信息。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-338">Implicit invocations such as implicit parent constructor calls do not have a source location and will not substitute caller information.</span></span> <span data-ttu-id="d1aa3-339">此外，动态绑定的调用将不替换调用方信息。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-339">Also, calls that are dynamically bound will not substitute caller information.</span></span> <span data-ttu-id="d1aa3-340">当调用方信息特性化的参数中这种情况下省略时，改为使用参数指定的默认值。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-340">When a caller info attributed parameter is omitted in such cases, the specified default value of the parameter is used instead.</span></span>

<span data-ttu-id="d1aa3-341">一个例外是查询表达式。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-341">One exception is query-expressions.</span></span> <span data-ttu-id="d1aa3-342">这些地址被视为语法的扩展，并调用它们展开此项可以省略可选参数与调用方信息特性，如果调用方信息将被替换。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-342">These are considered syntactic expansions, and if the calls they expand to omit optional parameters with caller info attributes, caller information will be substituted.</span></span> <span data-ttu-id="d1aa3-343">使用的位置是在查询子句从生成调用的位置。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-343">The location used is the location of the query clause which the call was generated from.</span></span>

<span data-ttu-id="d1aa3-344">如果为给定的参数指定多个调用方信息特性，则它们首选按以下顺序： `CallerLineNumber`， `CallerFilePath`， `CallerMemberName`。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-344">If more than one caller info attribute is specified on a given parameter, they are preferred in the following order: `CallerLineNumber`, `CallerFilePath`, `CallerMemberName`.</span></span>

#### <a name="the-callerlinenumber-attribute"></a><span data-ttu-id="d1aa3-345">CallerLineNumber 属性</span><span class="sxs-lookup"><span data-stu-id="d1aa3-345">The CallerLineNumber attribute</span></span>

<span data-ttu-id="d1aa3-346">`System.Runtime.CompilerServices.CallerLineNumberAttribute`标准的隐式转换时允许可选参数 ([标准隐式转换](conversions.md#standard-implicit-conversions)) 的常量值从`int.MaxValue`到参数的类型。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-346">The `System.Runtime.CompilerServices.CallerLineNumberAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from the constant value `int.MaxValue` to the parameter's type.</span></span> <span data-ttu-id="d1aa3-347">这可确保最多该值的任何非负行号，可以传递不会出错。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-347">This ensures that any non-negative line number up to that value can be passed without error.</span></span>

<span data-ttu-id="d1aa3-348">如果从源代码中的某个位置的函数调用中省略可选参数， `CallerLineNumberAttribute`，则作为而不是默认参数值调用的参数使用一个数值，表示该位置的行号。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-348">If a function invocation from a location in source code omits an optional parameter with the `CallerLineNumberAttribute`, then a numeric literal representing that location's line number is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="d1aa3-349">如果调用跨多个行，选择的行是依赖于实现的。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-349">If the invocation spans multiple lines, the line chosen is implementation-dependent.</span></span>

<span data-ttu-id="d1aa3-350">请注意，可能会影响的行号`#line`指令 ([行指令](lexical-structure.md#line-directives))。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-350">Note that the line number may be affected by `#line` directives ([Line directives](lexical-structure.md#line-directives)).</span></span>

#### <a name="the-callerfilepath-attribute"></a><span data-ttu-id="d1aa3-351">CallerFilePath 属性</span><span class="sxs-lookup"><span data-stu-id="d1aa3-351">The CallerFilePath attribute</span></span>

<span data-ttu-id="d1aa3-352">`System.Runtime.CompilerServices.CallerFilePathAttribute`标准的隐式转换时允许可选参数 ([标准隐式转换](conversions.md#standard-implicit-conversions)) 从`string`到参数的类型。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-352">The `System.Runtime.CompilerServices.CallerFilePathAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from `string` to the parameter's type.</span></span>

<span data-ttu-id="d1aa3-353">如果从源代码中的某个位置的函数调用中省略可选参数， `CallerFilePathAttribute`，则表示该位置的文件路径的字符串文字用作而不是默认参数值调用的参数。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-353">If a function invocation from a location in source code omits an optional parameter with the `CallerFilePathAttribute`, then a string literal representing that location's file path is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="d1aa3-354">文件路径的格式是依赖于实现的。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-354">The format of the file path is implementation-dependent.</span></span>

<span data-ttu-id="d1aa3-355">请注意，文件路径可能会受到`#line`指令 ([行指令](lexical-structure.md#line-directives))。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-355">Note that the file path may be affected by `#line` directives ([Line directives](lexical-structure.md#line-directives)).</span></span>

#### <a name="the-callermembername-attribute"></a><span data-ttu-id="d1aa3-356">CallerMemberName 属性</span><span class="sxs-lookup"><span data-stu-id="d1aa3-356">The CallerMemberName attribute</span></span>

<span data-ttu-id="d1aa3-357">`System.Runtime.CompilerServices.CallerMemberNameAttribute`标准的隐式转换时允许可选参数 ([标准隐式转换](conversions.md#standard-implicit-conversions)) 从`string`到参数的类型。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-357">The `System.Runtime.CompilerServices.CallerMemberNameAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from `string` to the parameter's type.</span></span>

<span data-ttu-id="d1aa3-358">如果函数调用的函数成员正文中或在特性中的位置从应用于的函数成员本身或其返回类型、 参数或类型参数在源代码省略了可选参数， `CallerMemberNameAttribute`，则表示该成员的名称的字符串文字用作而不是默认参数值调用的参数。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-358">If a function invocation from a location within the body of a function member or within an attribute applied to the function member itself or its return type, parameters or type parameters in source code omits an optional parameter with the `CallerMemberNameAttribute`, then a string literal representing the name of that member is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="d1aa3-359">调用泛型方法中发生的仅对方法名称本身使用，而无需的类型参数列表。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-359">For invocations that occur within generic methods, only the method name itself is used, without the type parameter list.</span></span>

<span data-ttu-id="d1aa3-360">显式接口成员实现代码中发生的调用，只能使用方法名称本身，而无需上述接口限定。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-360">For invocations that occur within explicit interface member implementations, only the method name itself is used, without the preceding interface qualification.</span></span>

<span data-ttu-id="d1aa3-361">对于属性或事件访问器内发生的调用，使用的成员名称是属性或事件本身。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-361">For invocations that occur within property or event accessors, the member name used is that of the property or event itself.</span></span>

<span data-ttu-id="d1aa3-362">有关索引器访问器内发生的调用，使用的成员名称是提供的`IndexerNameAttribute`([IndexerName 特性](attributes.md#the-indexername-attribute)) 上的索引器成员，如果存在或默认名称`Item`否则为。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-362">For invocations that occur within indexer accessors, the member name used is that supplied by an `IndexerNameAttribute` ([The IndexerName attribute](attributes.md#the-indexername-attribute)) on the indexer member, if present, or the default name `Item` otherwise.</span></span>

<span data-ttu-id="d1aa3-363">对于实例构造函数、 静态构造函数、 析构函数和运算符的声明中出现该成员的调用使用的名称是依赖于实现的。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-363">For invocations that occur within declarations of instance constructors, static constructors, destructors and operators the member name used is implementation-dependent.</span></span>

## <a name="attributes-for-interoperation"></a><span data-ttu-id="d1aa3-364">互操作的特性</span><span class="sxs-lookup"><span data-stu-id="d1aa3-364">Attributes for Interoperation</span></span>

<span data-ttu-id="d1aa3-365">注意:本部分是仅适用于的 Microsoft.NET 实现的C#。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-365">Note: This section is applicable only to the Microsoft .NET implementation of C#.</span></span>

### <a name="interoperation-with-com-and-win32-components"></a><span data-ttu-id="d1aa3-366">与 COM 和 Win32 组件互操作</span><span class="sxs-lookup"><span data-stu-id="d1aa3-366">Interoperation with COM and Win32 components</span></span>

<span data-ttu-id="d1aa3-367">.NET 运行时提供了大量的属性，可使 C# 程序可以与使用 COM 和 Win32 Dll 编写的组件进行互操作。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-367">The .NET run-time provides a large number of attributes that enable C# programs to interoperate with components written using COM and Win32 DLLs.</span></span> <span data-ttu-id="d1aa3-368">例如，`DllImport`属性可用于`static extern`方法以指示要在 Win32 DLL 中找到该方法的实现。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-368">For example, the `DllImport` attribute can be used on a `static extern` method to indicate that the implementation of the method is to be found in a Win32 DLL.</span></span> <span data-ttu-id="d1aa3-369">中找不到这些属性`System.Runtime.InteropServices`命名空间，以及为这些属性的详细的文档位于.NET 运行时文档。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-369">These attributes are found in the `System.Runtime.InteropServices` namespace, and detailed documentation for these attributes is found in the .NET runtime documentation.</span></span>

### <a name="interoperation-with-other-net-languages"></a><span data-ttu-id="d1aa3-370">与其他.NET 语言的互操作</span><span class="sxs-lookup"><span data-stu-id="d1aa3-370">Interoperation with other .NET languages</span></span>

#### <a name="the-indexername-attribute"></a><span data-ttu-id="d1aa3-371">IndexerName 特性</span><span class="sxs-lookup"><span data-stu-id="d1aa3-371">The IndexerName attribute</span></span>

<span data-ttu-id="d1aa3-372">索引器中使用索引的属性的.NET 实现和.NET 元数据中具有的名称。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-372">Indexers are implemented in .NET using indexed properties, and have a name in the .NET metadata.</span></span> <span data-ttu-id="d1aa3-373">如果没有`IndexerName`属性时，将提供索引器，然后是名称`Item`默认情况下使用。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-373">If no `IndexerName` attribute is present for an indexer, then the name `Item` is used by default.</span></span> <span data-ttu-id="d1aa3-374">`IndexerName`属性使开发人员将覆盖此默认值并指定一个不同的名称。</span><span class="sxs-lookup"><span data-stu-id="d1aa3-374">The `IndexerName` attribute enables a developer to override this default and specify a different name.</span></span>

```csharp
namespace System.Runtime.CompilerServices.CSharp
{
    [AttributeUsage(AttributeTargets.Property)]
    public class IndexerNameAttribute: Attribute
    {
        public IndexerNameAttribute(string indexerName) {...}
        public string Value { get {...} } 
    }
}
```
