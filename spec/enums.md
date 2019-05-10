---
ms.openlocfilehash: c3b716e6eb331be2ee33fffeb42c1e2406cd3a5c
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488760"
---
# <a name="enums"></a><span data-ttu-id="bafdc-101">枚举</span><span class="sxs-lookup"><span data-stu-id="bafdc-101">Enums</span></span>

<span data-ttu-id="bafdc-102">***枚举类型***是一个非重复值类型 ([值类型](types.md#value-types))，其用于声明一系列已命名常数。</span><span class="sxs-lookup"><span data-stu-id="bafdc-102">An ***enum type*** is a distinct value type ([Value types](types.md#value-types)) that declares a set of named constants.</span></span>

<span data-ttu-id="bafdc-103">该示例</span><span class="sxs-lookup"><span data-stu-id="bafdc-103">The example</span></span>

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="bafdc-104">声明名为枚举类型`Color`与成员`Red`， `Green`，和`Blue`。</span><span class="sxs-lookup"><span data-stu-id="bafdc-104">declares an enum type named `Color` with members `Red`, `Green`, and `Blue`.</span></span>

## <a name="enum-declarations"></a><span data-ttu-id="bafdc-105">枚举声明</span><span class="sxs-lookup"><span data-stu-id="bafdc-105">Enum declarations</span></span>

<span data-ttu-id="bafdc-106">枚举声明声明新的枚举类型。</span><span class="sxs-lookup"><span data-stu-id="bafdc-106">An enum declaration declares a new enum type.</span></span> <span data-ttu-id="bafdc-107">枚举声明以关键字开头`enum`，并定义名称、 可访问性、 基础类型和枚举的成员。</span><span class="sxs-lookup"><span data-stu-id="bafdc-107">An enum declaration begins with the keyword `enum`, and defines the name, accessibility, underlying type, and members of the enum.</span></span>

```antlr
enum_declaration
    : attributes? enum_modifier* 'enum' identifier enum_base? enum_body ';'?
    ;

enum_base
    : ':' integral_type
    ;

enum_body
    : '{' enum_member_declarations? '}'
    | '{' enum_member_declarations ',' '}'
    ;
```

<span data-ttu-id="bafdc-108">每个枚举类型具有对应的整型类型称为***基础类型***的枚举类型。</span><span class="sxs-lookup"><span data-stu-id="bafdc-108">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="bafdc-109">此基础类型必须能够表示枚举中定义的所有枚举器值。</span><span class="sxs-lookup"><span data-stu-id="bafdc-109">This underlying type must be able to represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="bafdc-110">枚举声明可以显式声明的基础类型`byte`， `sbyte`， `short`， `ushort`， `int`， `uint`，`long`或`ulong`。</span><span class="sxs-lookup"><span data-stu-id="bafdc-110">An enum declaration may explicitly declare an underlying type of `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="bafdc-111">请注意，`char`不能用作的基础类型。</span><span class="sxs-lookup"><span data-stu-id="bafdc-111">Note that `char` cannot be used as an underlying type.</span></span> <span data-ttu-id="bafdc-112">未显式声明基础类型的枚举声明都有的基础类型`int`。</span><span class="sxs-lookup"><span data-stu-id="bafdc-112">An enum declaration that does not explicitly declare an underlying type has an underlying type of `int`.</span></span>

<span data-ttu-id="bafdc-113">该示例</span><span class="sxs-lookup"><span data-stu-id="bafdc-113">The example</span></span>

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="bafdc-114">声明枚举的基础类型`long`。</span><span class="sxs-lookup"><span data-stu-id="bafdc-114">declares an enum with an underlying type of `long`.</span></span> <span data-ttu-id="bafdc-115">开发人员可以选择使用的基础类型`long`，如下所示的示例中，若要启用的范围内的值的用法`long`但不是在的范围内`int`，还是保留此选项可在将来。</span><span class="sxs-lookup"><span data-stu-id="bafdc-115">A developer might choose to use an underlying type of `long`, as in the example, to enable the use of values that are in the range of `long` but not in the range of `int`, or to preserve this option for the future.</span></span>

## <a name="enum-modifiers"></a><span data-ttu-id="bafdc-116">枚举修饰符</span><span class="sxs-lookup"><span data-stu-id="bafdc-116">Enum modifiers</span></span>

<span data-ttu-id="bafdc-117">*Enum_declaration*可以根据需要包含一系列枚举修饰符：</span><span class="sxs-lookup"><span data-stu-id="bafdc-117">An *enum_declaration* may optionally include a sequence of enum modifiers:</span></span>

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

<span data-ttu-id="bafdc-118">它是要在枚举声明中多次出现的相同修饰符的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="bafdc-118">It is a compile-time error for the same modifier to appear multiple times in an enum declaration.</span></span>

<span data-ttu-id="bafdc-119">枚举声明修饰符具有相同的含义的类声明 ([类修饰符](classes.md#class-modifiers))。</span><span class="sxs-lookup"><span data-stu-id="bafdc-119">The modifiers of an enum declaration have the same meaning as those of a class declaration ([Class modifiers](classes.md#class-modifiers)).</span></span> <span data-ttu-id="bafdc-120">但请注意，该`abstract`和`sealed`枚举声明中不允许使用修饰符。</span><span class="sxs-lookup"><span data-stu-id="bafdc-120">Note, however, that the `abstract` and `sealed` modifiers are not permitted in an enum declaration.</span></span> <span data-ttu-id="bafdc-121">枚举不能是抽象的不允许派生。</span><span class="sxs-lookup"><span data-stu-id="bafdc-121">Enums cannot be abstract and do not permit derivation.</span></span>

## <a name="enum-members"></a><span data-ttu-id="bafdc-122">枚举成员</span><span class="sxs-lookup"><span data-stu-id="bafdc-122">Enum members</span></span>

<span data-ttu-id="bafdc-123">枚举类型声明的主体定义零个或多个枚举成员，它们是枚举类型的命名的常量。</span><span class="sxs-lookup"><span data-stu-id="bafdc-123">The body of an enum type declaration defines zero or more enum members, which are the named constants of the enum type.</span></span> <span data-ttu-id="bafdc-124">没有两个枚举成员可以具有相同的名称。</span><span class="sxs-lookup"><span data-stu-id="bafdc-124">No two enum members can have the same name.</span></span>

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

<span data-ttu-id="bafdc-125">每个枚举成员具有关联的常数值。</span><span class="sxs-lookup"><span data-stu-id="bafdc-125">Each enum member has an associated constant value.</span></span> <span data-ttu-id="bafdc-126">此值的类型是包含枚举的基础类型。</span><span class="sxs-lookup"><span data-stu-id="bafdc-126">The type of this value is the underlying type for the containing enum.</span></span> <span data-ttu-id="bafdc-127">每个枚举成员的常量值必须是枚举的基础类型范围内。</span><span class="sxs-lookup"><span data-stu-id="bafdc-127">The constant value for each enum member must be in the range of the underlying type for the enum.</span></span> <span data-ttu-id="bafdc-128">该示例</span><span class="sxs-lookup"><span data-stu-id="bafdc-128">The example</span></span>

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

<span data-ttu-id="bafdc-129">导致编译时错误，因为常量值`-1`， `-2`，并`-3`不在的基础整型类型的范围内`uint`。</span><span class="sxs-lookup"><span data-stu-id="bafdc-129">results in a compile-time error because the constant values `-1`, `-2`, and `-3` are not in the range of the underlying integral type `uint`.</span></span>

<span data-ttu-id="bafdc-130">多个枚举成员可能会共享相同的关联的值。</span><span class="sxs-lookup"><span data-stu-id="bafdc-130">Multiple enum members may share the same associated value.</span></span> <span data-ttu-id="bafdc-131">该示例</span><span class="sxs-lookup"><span data-stu-id="bafdc-131">The example</span></span>

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

<span data-ttu-id="bafdc-132">显示枚举中的两个枚举成员--`Blue`和`Max`-具有相同的关联值。</span><span class="sxs-lookup"><span data-stu-id="bafdc-132">shows an enum in which two enum members -- `Blue` and `Max` -- have the same associated value.</span></span>

<span data-ttu-id="bafdc-133">隐式或显式分配的枚举成员关联的值。</span><span class="sxs-lookup"><span data-stu-id="bafdc-133">The associated value of an enum member is assigned either implicitly or explicitly.</span></span> <span data-ttu-id="bafdc-134">如果枚举成员的声明具有*constant_expression*初始值设定项，隐式转换为枚举的基础类型的常量表达式的值是枚举成员的关联的值。</span><span class="sxs-lookup"><span data-stu-id="bafdc-134">If the declaration of the enum member has a *constant_expression* initializer, the value of that constant expression, implicitly converted to the underlying type of the enum, is the associated value of the enum member.</span></span> <span data-ttu-id="bafdc-135">如果枚举成员的声明有没有初始值设定项，隐式，按如下所示设置其关联的值：</span><span class="sxs-lookup"><span data-stu-id="bafdc-135">If the declaration of the enum member has no initializer, its associated value is set implicitly, as follows:</span></span>

*  <span data-ttu-id="bafdc-136">如果该枚举成员是枚举类型中声明的第一个枚举成员，其关联的值为零。</span><span class="sxs-lookup"><span data-stu-id="bafdc-136">If the enum member is the first enum member declared in the enum type, its associated value is zero.</span></span>
*  <span data-ttu-id="bafdc-137">否则，枚举成员的关联的值被获取的原文前的枚举成员的关联的值加 1。</span><span class="sxs-lookup"><span data-stu-id="bafdc-137">Otherwise, the associated value of the enum member is obtained by increasing the associated value of the textually preceding enum member by one.</span></span> <span data-ttu-id="bafdc-138">增加的该值必须在基础类型可以表示的值的范围内，否则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="bafdc-138">This increased value must be within the range of values that can be represented by the underlying type, otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="bafdc-139">该示例</span><span class="sxs-lookup"><span data-stu-id="bafdc-139">The example</span></span>

```csharp
using System;

enum Color
{
    Red,
    Green = 10,
    Blue
}

class Test
{
    static void Main() {
        Console.WriteLine(StringFromColor(Color.Red));
        Console.WriteLine(StringFromColor(Color.Green));
        Console.WriteLine(StringFromColor(Color.Blue));
    }

    static string StringFromColor(Color c) {
        switch (c) {
            case Color.Red: 
                return String.Format("Red = {0}", (int) c);

            case Color.Green:
                return String.Format("Green = {0}", (int) c);

            case Color.Blue:
                return String.Format("Blue = {0}", (int) c);

            default:
                return "Invalid color";
        }
    }
}
```

<span data-ttu-id="bafdc-140">将输出的枚举成员名称和其关联的值。</span><span class="sxs-lookup"><span data-stu-id="bafdc-140">prints out the enum member names and their associated values.</span></span> <span data-ttu-id="bafdc-141">输出为：</span><span class="sxs-lookup"><span data-stu-id="bafdc-141">The output is:</span></span>

```
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="bafdc-142">原因如下：</span><span class="sxs-lookup"><span data-stu-id="bafdc-142">for the following reasons:</span></span>

*  <span data-ttu-id="bafdc-143">枚举成员`Red`自动分配值 0 （因为它没有初始值设定项，并且是第一个枚举成员）。</span><span class="sxs-lookup"><span data-stu-id="bafdc-143">the enum member `Red` is automatically assigned the value zero (since it has no initializer and is the first enum member);</span></span>
*  <span data-ttu-id="bafdc-144">枚举成员`Green`显式提供了值`10`;</span><span class="sxs-lookup"><span data-stu-id="bafdc-144">the enum member `Green` is explicitly given the value `10`;</span></span>
*  <span data-ttu-id="bafdc-145">和枚举成员`Blue`自动分配的值比文本上位于前面的成员。</span><span class="sxs-lookup"><span data-stu-id="bafdc-145">and the enum member `Blue` is automatically assigned the value one greater than the member that textually precedes it.</span></span>

<span data-ttu-id="bafdc-146">枚举成员的关联的值可能无法直接或间接使用其自身关联的枚举成员的值。</span><span class="sxs-lookup"><span data-stu-id="bafdc-146">The associated value of an enum member may not, directly or indirectly, use the value of its own associated enum member.</span></span> <span data-ttu-id="bafdc-147">此循环除了限制外，其他的枚举成员初始值设定项，而不考虑它们的文本位置可以自由地引用枚举成员初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="bafdc-147">Other than this circularity restriction, enum member initializers may freely refer to other enum member initializers, regardless of their textual position.</span></span> <span data-ttu-id="bafdc-148">枚举成员初始值设定项，在其他枚举成员的值始终被当作具有其基础类型的类型，以便引用其他枚举成员时，可能不是必要的强制转换。</span><span class="sxs-lookup"><span data-stu-id="bafdc-148">Within an enum member initializer, values of other enum members are always treated as having the type of their underlying type, so that casts are not necessary when referring to other enum members.</span></span>

<span data-ttu-id="bafdc-149">该示例</span><span class="sxs-lookup"><span data-stu-id="bafdc-149">The example</span></span>

```csharp
enum Circular
{
    A = B,
    B
}
```

<span data-ttu-id="bafdc-150">因为会导致编译时错误的声明`A`和`B`是循环的。</span><span class="sxs-lookup"><span data-stu-id="bafdc-150">results in a compile-time error because the declarations of `A` and `B` are circular.</span></span> <span data-ttu-id="bafdc-151">`A` 取决于`B`显式，并且`B`取决于`A`隐式。</span><span class="sxs-lookup"><span data-stu-id="bafdc-151">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

<span data-ttu-id="bafdc-152">名为枚举成员，作用域以完全等同于类中的字段的方式。</span><span class="sxs-lookup"><span data-stu-id="bafdc-152">Enum members are named and scoped in a manner exactly analogous to fields within classes.</span></span> <span data-ttu-id="bafdc-153">枚举成员的作用域是其包含的枚举类型的正文。</span><span class="sxs-lookup"><span data-stu-id="bafdc-153">The scope of an enum member is the body of its containing enum type.</span></span> <span data-ttu-id="bafdc-154">在该范围内，可以通过它们的简单名称称为枚举成员。</span><span class="sxs-lookup"><span data-stu-id="bafdc-154">Within that scope, enum members can be referred to by their simple name.</span></span> <span data-ttu-id="bafdc-155">从所有其他代码，必须具有其枚举类型名称限定枚举成员的名称。</span><span class="sxs-lookup"><span data-stu-id="bafdc-155">From all other code, the name of an enum member must be qualified with the name of its enum type.</span></span> <span data-ttu-id="bafdc-156">枚举成员不具有任何声明可访问性--枚举成员是可访问其包含的枚举类型是否可访问。</span><span class="sxs-lookup"><span data-stu-id="bafdc-156">Enum members do not have any declared accessibility -- an enum member is accessible if its containing enum type is accessible.</span></span>

## <a name="the-systemenum-type"></a><span data-ttu-id="bafdc-157">System.Enum 类型</span><span class="sxs-lookup"><span data-stu-id="bafdc-157">The System.Enum type</span></span>

<span data-ttu-id="bafdc-158">类型`System.Enum`是所有枚举类型 （这是不同，这不同于枚举类型的基础类型） 的抽象基类和成员继承自`System.Enum`任何枚举类型中可用。</span><span class="sxs-lookup"><span data-stu-id="bafdc-158">The type `System.Enum` is the abstract base class of all enum types (this is distinct and different from the underlying type of the enum type), and the members inherited from `System.Enum` are available in any enum type.</span></span> <span data-ttu-id="bafdc-159">装箱转换 ([装箱转换](types.md#boxing-conversions)) 存在从任何枚举类型设置为`System.Enum`，和取消装箱转换 ([取消装箱转换](types.md#unboxing-conversions)) 存在从`System.Enum`为任何枚举类型。</span><span class="sxs-lookup"><span data-stu-id="bafdc-159">A boxing conversion ([Boxing conversions](types.md#boxing-conversions)) exists from any enum type to `System.Enum`, and an unboxing conversion ([Unboxing conversions](types.md#unboxing-conversions)) exists from `System.Enum` to any enum type.</span></span>

<span data-ttu-id="bafdc-160">请注意，`System.Enum`本身不是*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="bafdc-160">Note that `System.Enum` is not itself an *enum_type*.</span></span> <span data-ttu-id="bafdc-161">相反，它是*class_type*所有*enum_type*s 派生。</span><span class="sxs-lookup"><span data-stu-id="bafdc-161">Rather, it is a *class_type* from which all *enum_type*s are derived.</span></span> <span data-ttu-id="bafdc-162">类型`System.Enum`继承自类型`System.ValueType`([System.ValueType 类型](types.md#the-systemvaluetype-type))，而后者又继承自类型`object`。</span><span class="sxs-lookup"><span data-stu-id="bafdc-162">The type `System.Enum` inherits from the type `System.ValueType` ([The System.ValueType type](types.md#the-systemvaluetype-type)), which, in turn, inherits from type `object`.</span></span> <span data-ttu-id="bafdc-163">在运行时类型的值`System.Enum`可以是`null`或对任何枚举类型的装箱值的引用。</span><span class="sxs-lookup"><span data-stu-id="bafdc-163">At run-time, a value of type `System.Enum` can be `null` or a reference to a boxed value of any enum type.</span></span>

## <a name="enum-values-and-operations"></a><span data-ttu-id="bafdc-164">枚举值和操作</span><span class="sxs-lookup"><span data-stu-id="bafdc-164">Enum values and operations</span></span>

<span data-ttu-id="bafdc-165">每个枚举类型定义不同的类型;显式枚举转换 ([显式枚举转换](conversions.md#explicit-enumeration-conversions)) 所需的枚举类型和整型类型，之间或两个枚举类型之间转换。</span><span class="sxs-lookup"><span data-stu-id="bafdc-165">Each enum type defines a distinct type; an explicit enumeration conversion ([Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)) is required to convert between an enum type and an integral type, or between two enum types.</span></span> <span data-ttu-id="bafdc-166">枚举类型可以采用上的值的集不受其枚举成员。</span><span class="sxs-lookup"><span data-stu-id="bafdc-166">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="bafdc-167">具体而言，枚举的基础类型的任何值可强制转换为枚举类型，并为该枚举类型的不同有效值。</span><span class="sxs-lookup"><span data-stu-id="bafdc-167">In particular, any value of the underlying type of an enum can be cast to the enum type, and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="bafdc-168">枚举的成员具有其包含的枚举类型的类型 (在其他枚举成员初始值设定项除外： 请参阅[枚举成员](enums.md#enum-members))。</span><span class="sxs-lookup"><span data-stu-id="bafdc-168">Enum members have the type of their containing enum type (except within other enum member initializers: see [Enum members](enums.md#enum-members)).</span></span> <span data-ttu-id="bafdc-169">枚举类型中声明的枚举成员值`E`相关联的值与`v`是`(E)v`。</span><span class="sxs-lookup"><span data-stu-id="bafdc-169">The value of an enum member declared in enum type `E` with associated value `v` is `(E)v`.</span></span>

<span data-ttu-id="bafdc-170">以下运算符可以用于枚举类型的值： `==`， `!=`， `<`， `>`， `<=`， `>=`  ([枚举比较运算符](expressions.md#enumeration-comparison-operators))二进制`+`  ([加法运算符](expressions.md#addition-operator))，二进制`-`  ([减法运算符](expressions.md#subtraction-operator))， `^`， `&``|`  ([枚举逻辑运算符](expressions.md#enumeration-logical-operators))， `~`  ([按位求补运算符](expressions.md#bitwise-complement-operator))，`++`和`--` ([后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators)并[前缀递增和递减运算符](expressions.md#prefix-increment-and-decrement-operators))。</span><span class="sxs-lookup"><span data-stu-id="bafdc-170">The following operators can be used on values of enum types: `==`, `!=`, `<`, `>`, `<=`, `>=` ([Enumeration comparison operators](expressions.md#enumeration-comparison-operators)), binary `+` ([Addition operator](expressions.md#addition-operator)), binary `-` ([Subtraction operator](expressions.md#subtraction-operator)), `^`, `&`, `|` ([Enumeration logical operators](expressions.md#enumeration-logical-operators)), `~` ([Bitwise complement operator](expressions.md#bitwise-complement-operator)), `++` and `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="bafdc-171">每个枚举类型自动派生自类`System.Enum`(其中，反过来，派生`System.ValueType`和`object`)。</span><span class="sxs-lookup"><span data-stu-id="bafdc-171">Every enum type automatically derives from the class `System.Enum` (which, in turn, derives from `System.ValueType` and `object`).</span></span> <span data-ttu-id="bafdc-172">因此，此类的继承的方法和属性可以使用枚举类型的值。</span><span class="sxs-lookup"><span data-stu-id="bafdc-172">Thus, inherited methods and properties of this class can be used on values of an enum type.</span></span>
