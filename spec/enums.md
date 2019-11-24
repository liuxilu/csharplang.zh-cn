---
ms.openlocfilehash: 3b142d7dbda8a94a4cf2c973a2e380065dcbf5ee
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703970"
---
# <a name="enums"></a><span data-ttu-id="2bfa5-101">枚举</span><span class="sxs-lookup"><span data-stu-id="2bfa5-101">Enums</span></span>

<span data-ttu-id="2bfa5-102">***枚举类型***是声明一组命名常量的非重复值类型（[值类型](types.md#value-types)）。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-102">An ***enum type*** is a distinct value type ([Value types](types.md#value-types)) that declares a set of named constants.</span></span>

<span data-ttu-id="2bfa5-103">示例</span><span class="sxs-lookup"><span data-stu-id="2bfa5-103">The example</span></span>

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="2bfa5-104">使用成员 `Red`、`Green`和 `Blue`声明名为 `Color` 的枚举类型。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-104">declares an enum type named `Color` with members `Red`, `Green`, and `Blue`.</span></span>

## <a name="enum-declarations"></a><span data-ttu-id="2bfa5-105">枚举声明</span><span class="sxs-lookup"><span data-stu-id="2bfa5-105">Enum declarations</span></span>

<span data-ttu-id="2bfa5-106">枚举声明声明一个新的枚举类型。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-106">An enum declaration declares a new enum type.</span></span> <span data-ttu-id="2bfa5-107">枚举声明以关键字 `enum`开头，并定义枚举的名称、可访问性、基础类型和成员。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-107">An enum declaration begins with the keyword `enum`, and defines the name, accessibility, underlying type, and members of the enum.</span></span>

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

<span data-ttu-id="2bfa5-108">每个枚举类型都有一个对应的整型类型，称为枚举类型的***基础类型***。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-108">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="2bfa5-109">此基础类型必须能够表示枚举中定义的所有枚举器值。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-109">This underlying type must be able to represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="2bfa5-110">枚举声明可以显式声明基础类型的 `byte`、`sbyte`、`short`、`ushort`、`int`、`uint`、`long` 或 `ulong`。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-110">An enum declaration may explicitly declare an underlying type of `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="2bfa5-111">请注意，`char` 不能用作基础类型。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-111">Note that `char` cannot be used as an underlying type.</span></span> <span data-ttu-id="2bfa5-112">不显式声明基础类型的枚举声明具有基础类型的 `int`。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-112">An enum declaration that does not explicitly declare an underlying type has an underlying type of `int`.</span></span>

<span data-ttu-id="2bfa5-113">示例</span><span class="sxs-lookup"><span data-stu-id="2bfa5-113">The example</span></span>

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="2bfa5-114">使用 `long`的基础类型声明枚举。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-114">declares an enum with an underlying type of `long`.</span></span> <span data-ttu-id="2bfa5-115">开发人员可以选择使用基础类型的 `long`（如本示例所示），以允许使用 `long` 范围内但不在 `int`范围内的值，或保留此选项以供将来使用。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-115">A developer might choose to use an underlying type of `long`, as in the example, to enable the use of values that are in the range of `long` but not in the range of `int`, or to preserve this option for the future.</span></span>

## <a name="enum-modifiers"></a><span data-ttu-id="2bfa5-116">枚举修饰符</span><span class="sxs-lookup"><span data-stu-id="2bfa5-116">Enum modifiers</span></span>

<span data-ttu-id="2bfa5-117">*Enum_declaration*可以选择性地包含一系列枚举修饰符：</span><span class="sxs-lookup"><span data-stu-id="2bfa5-117">An *enum_declaration* may optionally include a sequence of enum modifiers:</span></span>

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

<span data-ttu-id="2bfa5-118">同一修饰符在一个枚举声明中多次出现是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-118">It is a compile-time error for the same modifier to appear multiple times in an enum declaration.</span></span>

<span data-ttu-id="2bfa5-119">枚举声明的修饰符与类声明（[类修饰符](classes.md#class-modifiers)）的修饰符具有相同的含义。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-119">The modifiers of an enum declaration have the same meaning as those of a class declaration ([Class modifiers](classes.md#class-modifiers)).</span></span> <span data-ttu-id="2bfa5-120">但请注意，在枚举声明中不允许使用 `abstract` 和 `sealed` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-120">Note, however, that the `abstract` and `sealed` modifiers are not permitted in an enum declaration.</span></span> <span data-ttu-id="2bfa5-121">枚举不能是抽象的并且不允许派生。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-121">Enums cannot be abstract and do not permit derivation.</span></span>

## <a name="enum-members"></a><span data-ttu-id="2bfa5-122">枚举成员</span><span class="sxs-lookup"><span data-stu-id="2bfa5-122">Enum members</span></span>

<span data-ttu-id="2bfa5-123">枚举类型声明的主体定义零个或多个枚举成员，这是枚举类型的命名常量。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-123">The body of an enum type declaration defines zero or more enum members, which are the named constants of the enum type.</span></span> <span data-ttu-id="2bfa5-124">两个枚举成员不能具有相同的名称。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-124">No two enum members can have the same name.</span></span>

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

<span data-ttu-id="2bfa5-125">每个枚举成员都有一个关联的常量值。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-125">Each enum member has an associated constant value.</span></span> <span data-ttu-id="2bfa5-126">此值的类型是包含枚举的基础类型。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-126">The type of this value is the underlying type for the containing enum.</span></span> <span data-ttu-id="2bfa5-127">每个枚举成员的常数值必须在该枚举的基础类型的范围内。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-127">The constant value for each enum member must be in the range of the underlying type for the enum.</span></span> <span data-ttu-id="2bfa5-128">示例</span><span class="sxs-lookup"><span data-stu-id="2bfa5-128">The example</span></span>

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

<span data-ttu-id="2bfa5-129">导致编译时错误，因为 `-1`、`-2`和 `-3` 的常量值不在基础整型类型 `uint`的范围内。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-129">results in a compile-time error because the constant values `-1`, `-2`, and `-3` are not in the range of the underlying integral type `uint`.</span></span>

<span data-ttu-id="2bfa5-130">多个枚举成员可能共享相同的关联值。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-130">Multiple enum members may share the same associated value.</span></span> <span data-ttu-id="2bfa5-131">示例</span><span class="sxs-lookup"><span data-stu-id="2bfa5-131">The example</span></span>

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

<span data-ttu-id="2bfa5-132">显示枚举，其中两个枚举成员--`Blue` 和 `Max` 具有相同的关联值。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-132">shows an enum in which two enum members -- `Blue` and `Max` -- have the same associated value.</span></span>

<span data-ttu-id="2bfa5-133">枚举成员的关联值是隐式或显式分配的。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-133">The associated value of an enum member is assigned either implicitly or explicitly.</span></span> <span data-ttu-id="2bfa5-134">如果枚举成员的声明具有*constant_expression*初始值设定项，则该常量表达式的值将隐式转换为枚举的基础类型，这是枚举成员的关联值。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-134">If the declaration of the enum member has a *constant_expression* initializer, the value of that constant expression, implicitly converted to the underlying type of the enum, is the associated value of the enum member.</span></span> <span data-ttu-id="2bfa5-135">如果枚举成员的声明没有初始值设定项，则将隐式设置其关联值，如下所示：</span><span class="sxs-lookup"><span data-stu-id="2bfa5-135">If the declaration of the enum member has no initializer, its associated value is set implicitly, as follows:</span></span>

*  <span data-ttu-id="2bfa5-136">如果枚举成员是枚举类型中声明的第一个枚举成员，则其关联值为零。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-136">If the enum member is the first enum member declared in the enum type, its associated value is zero.</span></span>
*  <span data-ttu-id="2bfa5-137">否则，枚举成员的关联值将通过将每个按日的枚举成员的关联值增加一个来获得。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-137">Otherwise, the associated value of the enum member is obtained by increasing the associated value of the textually preceding enum member by one.</span></span> <span data-ttu-id="2bfa5-138">这一增加的值必须在可由基础类型表示的值范围内，否则将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-138">This increased value must be within the range of values that can be represented by the underlying type, otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="2bfa5-139">示例</span><span class="sxs-lookup"><span data-stu-id="2bfa5-139">The example</span></span>

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

<span data-ttu-id="2bfa5-140">输出枚举成员名称及其关联值。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-140">prints out the enum member names and their associated values.</span></span> <span data-ttu-id="2bfa5-141">输出为：</span><span class="sxs-lookup"><span data-stu-id="2bfa5-141">The output is:</span></span>

```console
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="2bfa5-142">原因如下：</span><span class="sxs-lookup"><span data-stu-id="2bfa5-142">for the following reasons:</span></span>

*  <span data-ttu-id="2bfa5-143">枚举成员 `Red` 会自动赋予零值（因为它没有初始值设定项，并且是第一个枚举成员）;</span><span class="sxs-lookup"><span data-stu-id="2bfa5-143">the enum member `Red` is automatically assigned the value zero (since it has no initializer and is the first enum member);</span></span>
*  <span data-ttu-id="2bfa5-144">为枚举成员 `Green` 显式赋予 `10`值;</span><span class="sxs-lookup"><span data-stu-id="2bfa5-144">the enum member `Green` is explicitly given the value `10`;</span></span>
*  <span data-ttu-id="2bfa5-145">和枚举成员 `Blue` 会自动赋给一个值，其值大于在一个上按其进行的成员。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-145">and the enum member `Blue` is automatically assigned the value one greater than the member that textually precedes it.</span></span>

<span data-ttu-id="2bfa5-146">枚举成员的关联值不能直接或间接地使用其自己的关联枚举成员的值。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-146">The associated value of an enum member may not, directly or indirectly, use the value of its own associated enum member.</span></span> <span data-ttu-id="2bfa5-147">除了此循环限制之外，枚举成员初始值设定项可以随意引用其他枚举成员初始值设定项，而不考虑其文本位置。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-147">Other than this circularity restriction, enum member initializers may freely refer to other enum member initializers, regardless of their textual position.</span></span> <span data-ttu-id="2bfa5-148">在枚举成员初始值设定项中，其他枚举成员的值始终被视为具有其基础类型的类型，因此在引用其他枚举成员时不需要强制转换。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-148">Within an enum member initializer, values of other enum members are always treated as having the type of their underlying type, so that casts are not necessary when referring to other enum members.</span></span>

<span data-ttu-id="2bfa5-149">示例</span><span class="sxs-lookup"><span data-stu-id="2bfa5-149">The example</span></span>

```csharp
enum Circular
{
    A = B,
    B
}
```

<span data-ttu-id="2bfa5-150">导致编译时错误，因为 `A` 和 `B` 的声明是循环的。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-150">results in a compile-time error because the declarations of `A` and `B` are circular.</span></span> <span data-ttu-id="2bfa5-151">`A` 显式依赖于 `B`，`B` 依赖于 `A` 隐式。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-151">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

<span data-ttu-id="2bfa5-152">枚举成员的命名方式与类中的字段完全类似。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-152">Enum members are named and scoped in a manner exactly analogous to fields within classes.</span></span> <span data-ttu-id="2bfa5-153">枚举成员的作用域是其包含的枚举类型的正文。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-153">The scope of an enum member is the body of its containing enum type.</span></span> <span data-ttu-id="2bfa5-154">在该范围内，枚举成员可以通过其简单名称引用。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-154">Within that scope, enum members can be referred to by their simple name.</span></span> <span data-ttu-id="2bfa5-155">对于所有其他代码，枚举成员的名称必须用其枚举类型的名称进行限定。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-155">From all other code, the name of an enum member must be qualified with the name of its enum type.</span></span> <span data-ttu-id="2bfa5-156">枚举成员不具有任何已声明的可访问性-如果其包含枚举类型可访问，则枚举成员是可访问的。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-156">Enum members do not have any declared accessibility -- an enum member is accessible if its containing enum type is accessible.</span></span>

## <a name="the-systemenum-type"></a><span data-ttu-id="2bfa5-157">System.object 类型</span><span class="sxs-lookup"><span data-stu-id="2bfa5-157">The System.Enum type</span></span>

<span data-ttu-id="2bfa5-158">类型 `System.Enum` 是所有枚举类型的抽象基类（这是截然不同的，不同于枚举类型的基础类型），并且从 `System.Enum` 继承的成员可用于任何枚举类型。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-158">The type `System.Enum` is the abstract base class of all enum types (this is distinct and different from the underlying type of the enum type), and the members inherited from `System.Enum` are available in any enum type.</span></span> <span data-ttu-id="2bfa5-159">从任何枚举类型到 `System.Enum`都存在装箱转换（[装箱](types.md#boxing-conversions)转换），并且从 `System.Enum` 到任何枚举类型都存在取消装箱转换（[取消装箱](types.md#unboxing-conversions)转换）。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-159">A boxing conversion ([Boxing conversions](types.md#boxing-conversions)) exists from any enum type to `System.Enum`, and an unboxing conversion ([Unboxing conversions](types.md#unboxing-conversions)) exists from `System.Enum` to any enum type.</span></span>

<span data-ttu-id="2bfa5-160">请注意，`System.Enum` 本身不是*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-160">Note that `System.Enum` is not itself an *enum_type*.</span></span> <span data-ttu-id="2bfa5-161">相反，它是派生所有*enum_type*的*class_type* 。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-161">Rather, it is a *class_type* from which all *enum_type*s are derived.</span></span> <span data-ttu-id="2bfa5-162">类型 `System.Enum` 从类型 `System.ValueType` （[系统类型](types.md#the-systemvaluetype-type)）继承而来，后者又继承自类型 `object`。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-162">The type `System.Enum` inherits from the type `System.ValueType` ([The System.ValueType type](types.md#the-systemvaluetype-type)), which, in turn, inherits from type `object`.</span></span> <span data-ttu-id="2bfa5-163">在运行时，可以 `null` 类型 `System.Enum` 的值或对任何枚举类型的装箱值的引用。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-163">At run-time, a value of type `System.Enum` can be `null` or a reference to a boxed value of any enum type.</span></span>

## <a name="enum-values-and-operations"></a><span data-ttu-id="2bfa5-164">枚举值和运算</span><span class="sxs-lookup"><span data-stu-id="2bfa5-164">Enum values and operations</span></span>

<span data-ttu-id="2bfa5-165">每个枚举类型都定义了一个不同的类型;若要在枚举类型和整型之间进行转换，或在两个枚举类型之间进行转换，必须使用显式枚举转换（[显式枚举转换](conversions.md#explicit-enumeration-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-165">Each enum type defines a distinct type; an explicit enumeration conversion ([Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)) is required to convert between an enum type and an integral type, or between two enum types.</span></span> <span data-ttu-id="2bfa5-166">枚举类型可以采用的值集不受其枚举成员限制。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-166">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="2bfa5-167">特别是，枚举的基础类型的任何值都可以转换为枚举类型，并且是该枚举类型的非重复有效值。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-167">In particular, any value of the underlying type of an enum can be cast to the enum type, and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="2bfa5-168">枚举成员具有其包含枚举类型的类型（在其他枚举成员初始值设定项中除外：请参见[枚举成员](enums.md#enum-members)）。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-168">Enum members have the type of their containing enum type (except within other enum member initializers: see [Enum members](enums.md#enum-members)).</span></span> <span data-ttu-id="2bfa5-169">使用关联值 `v` 在枚举类型 `E` 中声明的枚举成员的值 `(E)v`。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-169">The value of an enum member declared in enum type `E` with associated value `v` is `(E)v`.</span></span>

<span data-ttu-id="2bfa5-170">以下运算符可用于枚举类型的值： `==`、`!=`、`<`、`>`、`<=`、`>=` （[枚举比较运算符](expressions.md#enumeration-comparison-operators)）、二进制 `+` （[加法运算符](expressions.md#addition-operator)[）、](expressions.md#subtraction-operator)`-`、 、`^``&`（[枚举逻辑运算符](expressions.md#enumeration-logical-operators)）、`|` （[按位求补运算符](expressions.md#bitwise-complement-operator)）、`~`和  `++` （[后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators)以及[前缀增量和减量运算符](expressions.md#prefix-increment-and-decrement-operators)）。`--` </span><span class="sxs-lookup"><span data-stu-id="2bfa5-170">The following operators can be used on values of enum types: `==`, `!=`, `<`, `>`, `<=`, `>=` ([Enumeration comparison operators](expressions.md#enumeration-comparison-operators)), binary `+` ([Addition operator](expressions.md#addition-operator)), binary `-` ([Subtraction operator](expressions.md#subtraction-operator)), `^`, `&`, `|` ([Enumeration logical operators](expressions.md#enumeration-logical-operators)), `~` ([Bitwise complement operator](expressions.md#bitwise-complement-operator)), `++` and `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="2bfa5-171">每个枚举类型都自动派生自类 `System.Enum` （反过来，派生自 `System.ValueType` 和 `object`）。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-171">Every enum type automatically derives from the class `System.Enum` (which, in turn, derives from `System.ValueType` and `object`).</span></span> <span data-ttu-id="2bfa5-172">因此，此类的继承方法和属性可用于枚举类型的值。</span><span class="sxs-lookup"><span data-stu-id="2bfa5-172">Thus, inherited methods and properties of this class can be used on values of an enum type.</span></span>
