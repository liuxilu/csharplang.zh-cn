---
ms.openlocfilehash: 088c4a77cecde490c556c44c239a3496f896582e
ms.sourcegitcommit: 4ddf18d000734c1b6d0a48127bf338086fc3f2c3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2019
ms.locfileid: "73616143"
---
# <a name="types"></a><span data-ttu-id="350f9-101">类型</span><span class="sxs-lookup"><span data-stu-id="350f9-101">Types</span></span>

<span data-ttu-id="350f9-102">C#语言的类型分为两个主要类别：***值类型***和***引用类型***。</span><span class="sxs-lookup"><span data-stu-id="350f9-102">The types of the C# language are divided into two main categories: ***value types*** and ***reference types***.</span></span> <span data-ttu-id="350f9-103">值类型和引用类型都可以是采用一个或多个***类型参数***的***泛型类型***。</span><span class="sxs-lookup"><span data-stu-id="350f9-103">Both value types and reference types may be ***generic types***, which take one or more ***type parameters***.</span></span> <span data-ttu-id="350f9-104">类型参数可以同时指定值类型和引用类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-104">Type parameters can designate both value types and reference types.</span></span>

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

<span data-ttu-id="350f9-105">类型（指针）的最终类别仅在不安全代码中提供。</span><span class="sxs-lookup"><span data-stu-id="350f9-105">The final category of types, pointers, is available only in unsafe code.</span></span> <span data-ttu-id="350f9-106">[指针类型](unsafe-code.md#pointer-types)中对此进行了进一步讨论。</span><span class="sxs-lookup"><span data-stu-id="350f9-106">This is discussed further in [Pointer types](unsafe-code.md#pointer-types).</span></span>

<span data-ttu-id="350f9-107">值类型在值类型的变量中直接包含其数据时不同于引用类型，而引用类型的变量存储对其数据的***引用***，后者被称为***对象***。</span><span class="sxs-lookup"><span data-stu-id="350f9-107">Value types differ from reference types in that variables of the value types directly contain their data, whereas variables of the reference types store ***references*** to their data, the latter being known as ***objects***.</span></span> <span data-ttu-id="350f9-108">对于引用类型，两个变量可以引用同一对象，因此，对一个变量执行的运算可能会影响另一个变量所引用的对象。</span><span class="sxs-lookup"><span data-stu-id="350f9-108">With reference types, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="350f9-109">对于值类型，每个变量都有自己的数据副本，对一个变量执行的操作不可能影响另一个。</span><span class="sxs-lookup"><span data-stu-id="350f9-109">With value types, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other.</span></span>

<span data-ttu-id="350f9-110">C#的类型系统是统一的，因此任何类型的值都可以被视为对象。</span><span class="sxs-lookup"><span data-stu-id="350f9-110">C#'s type system is unified such that a value of any type can be treated as an object.</span></span> <span data-ttu-id="350f9-111">每种 C# 类型都直接或间接地派生自 `object` 类类型，而 `object` 是所有类型的最终基类。</span><span class="sxs-lookup"><span data-stu-id="350f9-111">Every type in C# directly or indirectly derives from the `object` class type, and `object` is the ultimate base class of all types.</span></span> <span data-ttu-id="350f9-112">只需将值视为类型 `object`，即可将引用类型的值视为对象。</span><span class="sxs-lookup"><span data-stu-id="350f9-112">Values of reference types are treated as objects simply by viewing the values as type `object`.</span></span> <span data-ttu-id="350f9-113">通过执行装箱和取消装箱操作（[装箱和取消](types.md#boxing-and-unboxing)装箱），值类型的值被视为对象。</span><span class="sxs-lookup"><span data-stu-id="350f9-113">Values of value types are treated as objects by performing boxing and unboxing operations ([Boxing and unboxing](types.md#boxing-and-unboxing)).</span></span>

## <a name="value-types"></a><span data-ttu-id="350f9-114">值类型</span><span class="sxs-lookup"><span data-stu-id="350f9-114">Value types</span></span>

<span data-ttu-id="350f9-115">值类型是结构类型或枚举类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-115">A value type is either a struct type or an enumeration type.</span></span> <span data-ttu-id="350f9-116">C#提供一组称为***简单类型***的预定义的结构类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-116">C# provides a set of predefined struct types called the ***simple types***.</span></span> <span data-ttu-id="350f9-117">简单类型通过保留字进行标识。</span><span class="sxs-lookup"><span data-stu-id="350f9-117">The simple types are identified through reserved words.</span></span>

```antlr
value_type
    : struct_type
    | enum_type
    ;

struct_type
    : type_name
    | simple_type
    | nullable_type
    ;

simple_type
    : numeric_type
    | 'bool'
    ;

numeric_type
    : integral_type
    | floating_point_type
    | 'decimal'
    ;

integral_type
    : 'sbyte'
    | 'byte'
    | 'short'
    | 'ushort'
    | 'int'
    | 'uint'
    | 'long'
    | 'ulong'
    | 'char'
    ;

floating_point_type
    : 'float'
    | 'double'
    ;

nullable_type
    : non_nullable_value_type '?'
    ;

non_nullable_value_type
    : type
    ;

enum_type
    : type_name
    ;
```

<span data-ttu-id="350f9-118">与引用类型的变量不同，值类型的变量仅在值类型是可以为 null 的类型时才可以包含值 `null`。</span><span class="sxs-lookup"><span data-stu-id="350f9-118">Unlike a variable of a reference type, a variable of a value type can contain the value `null` only if the value type is a nullable type.</span></span>  <span data-ttu-id="350f9-119">对于每个不可为 null 的值类型，都有一个对应的可以为 null 的值类型，用于表示相同的一组值加上 `null`的值。</span><span class="sxs-lookup"><span data-stu-id="350f9-119">For every non-nullable value type there is a corresponding nullable value type denoting the same set of values plus the value `null`.</span></span>

<span data-ttu-id="350f9-120">对值类型的变量赋值会创建要分配的值的副本。</span><span class="sxs-lookup"><span data-stu-id="350f9-120">Assignment to a variable of a value type creates a copy of the value being assigned.</span></span> <span data-ttu-id="350f9-121">这不同于对引用类型的变量的赋值，后者复制引用，而不是由引用标识的对象。</span><span class="sxs-lookup"><span data-stu-id="350f9-121">This differs from assignment to a variable of a reference type, which copies the reference but not the object identified by the reference.</span></span>

### <a name="the-systemvaluetype-type"></a><span data-ttu-id="350f9-122">System.object 类型</span><span class="sxs-lookup"><span data-stu-id="350f9-122">The System.ValueType type</span></span>

<span data-ttu-id="350f9-123">所有值类型都隐式继承自类 `System.ValueType`，而该类又从类 `object`继承。</span><span class="sxs-lookup"><span data-stu-id="350f9-123">All value types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="350f9-124">任何类型都不能从值类型派生，因此值类型将隐式密封（[密封类](classes.md#sealed-classes)）。</span><span class="sxs-lookup"><span data-stu-id="350f9-124">It is not possible for any type to derive from a value type, and value types are thus implicitly sealed ([Sealed classes](classes.md#sealed-classes)).</span></span>

<span data-ttu-id="350f9-125">请注意，`System.ValueType` 本身并不*value_type*。</span><span class="sxs-lookup"><span data-stu-id="350f9-125">Note that `System.ValueType` is not itself a *value_type*.</span></span> <span data-ttu-id="350f9-126">相反，它是一个*class_type* ，所有*value_type*都自动派生。</span><span class="sxs-lookup"><span data-stu-id="350f9-126">Rather, it is a *class_type* from which all *value_type*s are automatically derived.</span></span>

### <a name="default-constructors"></a><span data-ttu-id="350f9-127">默认构造函数</span><span class="sxs-lookup"><span data-stu-id="350f9-127">Default constructors</span></span>

<span data-ttu-id="350f9-128">所有值类型都隐式声明称为***默认构造函数***的公共无参数实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="350f9-128">All value types implicitly declare a public parameterless instance constructor called the ***default constructor***.</span></span> <span data-ttu-id="350f9-129">默认构造函数返回一个名为的零初始化实例，该实例称为 "值类型" 的***默认值***：</span><span class="sxs-lookup"><span data-stu-id="350f9-129">The default constructor returns a zero-initialized instance known as the ***default value*** for the value type:</span></span>

*  <span data-ttu-id="350f9-130">对于所有*simple_type*，默认值是由所有零的位模式生成的值：</span><span class="sxs-lookup"><span data-stu-id="350f9-130">For all *simple_type*s, the default value is the value produced by a bit pattern of all zeros:</span></span>
    * <span data-ttu-id="350f9-131">对于 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`和 `ulong`，默认值为 `0`。</span><span class="sxs-lookup"><span data-stu-id="350f9-131">For `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, and `ulong`, the default value is `0`.</span></span>
    * <span data-ttu-id="350f9-132">对于 `char`，默认值为 "`'\x0000'`"。</span><span class="sxs-lookup"><span data-stu-id="350f9-132">For `char`, the default value is `'\x0000'`.</span></span>
    * <span data-ttu-id="350f9-133">对于 `float`，默认值为 "`0.0f`"。</span><span class="sxs-lookup"><span data-stu-id="350f9-133">For `float`, the default value is `0.0f`.</span></span>
    * <span data-ttu-id="350f9-134">对于 `double`，默认值为 "`0.0d`"。</span><span class="sxs-lookup"><span data-stu-id="350f9-134">For `double`, the default value is `0.0d`.</span></span>
    * <span data-ttu-id="350f9-135">对于 `decimal`，默认值为 "`0.0m`"。</span><span class="sxs-lookup"><span data-stu-id="350f9-135">For `decimal`, the default value is `0.0m`.</span></span>
    * <span data-ttu-id="350f9-136">对于 `bool`，默认值为 "`false`"。</span><span class="sxs-lookup"><span data-stu-id="350f9-136">For `bool`, the default value is `false`.</span></span>
*  <span data-ttu-id="350f9-137">对于*为*`E`，默认值为 `0`，转换为类型 `E`。</span><span class="sxs-lookup"><span data-stu-id="350f9-137">For an *enum_type* `E`, the default value is `0`, converted to the type `E`.</span></span>
*  <span data-ttu-id="350f9-138">对于*struct_type*，默认值是通过将所有值类型的字段设置为其默认值，并将所有引用类型字段设置为 `null`生成的值。</span><span class="sxs-lookup"><span data-stu-id="350f9-138">For a *struct_type*, the default value is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>
*  <span data-ttu-id="350f9-139">对于*nullable_type* ，默认值为 `HasValue` 属性为 false 且 `Value` 属性未定义的实例。</span><span class="sxs-lookup"><span data-stu-id="350f9-139">For a *nullable_type* the default value is an instance for which the `HasValue` property is false and the `Value` property is undefined.</span></span> <span data-ttu-id="350f9-140">默认值也称为可以为 null 的类型的***null 值***。</span><span class="sxs-lookup"><span data-stu-id="350f9-140">The default value is also known as the ***null value*** of the nullable type.</span></span>

<span data-ttu-id="350f9-141">与任何其他实例构造函数一样，值类型的默认构造函数使用 `new` 运算符进行调用。</span><span class="sxs-lookup"><span data-stu-id="350f9-141">Like any other instance constructor, the default constructor of a value type is invoked using the `new` operator.</span></span> <span data-ttu-id="350f9-142">出于效率方面的考虑，此要求实际上不会使实现生成构造函数调用。</span><span class="sxs-lookup"><span data-stu-id="350f9-142">For efficiency reasons, this requirement is not intended to actually have the implementation generate a constructor call.</span></span> <span data-ttu-id="350f9-143">在下面的示例中，变量 `i` 和 `j` 都初始化为零。</span><span class="sxs-lookup"><span data-stu-id="350f9-143">In the example below, variables `i` and `j` are both initialized to zero.</span></span>

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

<span data-ttu-id="350f9-144">由于每个值类型都隐式具有一个公共的无参数实例构造函数，因此结构类型不可能包含无参数构造函数的显式声明。</span><span class="sxs-lookup"><span data-stu-id="350f9-144">Because every value type implicitly has a public parameterless instance constructor, it is not possible for a struct type to contain an explicit declaration of a parameterless constructor.</span></span> <span data-ttu-id="350f9-145">但允许结构类型声明参数化实例构造函数（[构造函数](structs.md#constructors)）。</span><span class="sxs-lookup"><span data-stu-id="350f9-145">A struct type is however permitted to declare parameterized instance constructors ([Constructors](structs.md#constructors)).</span></span>

### <a name="struct-types"></a><span data-ttu-id="350f9-146">结构类型</span><span class="sxs-lookup"><span data-stu-id="350f9-146">Struct types</span></span>

<span data-ttu-id="350f9-147">结构类型是一种值类型，可以声明常量、字段、方法、属性、索引器、运算符、实例构造函数、静态构造函数和嵌套类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-147">A struct type is a value type that can declare constants, fields, methods, properties, indexers, operators, instance constructors, static constructors, and nested types.</span></span> <span data-ttu-id="350f9-148">结构[声明](structs.md#struct-declarations)中描述了结构类型的声明。</span><span class="sxs-lookup"><span data-stu-id="350f9-148">The declaration of struct types is described in [Struct declarations](structs.md#struct-declarations).</span></span>

### <a name="simple-types"></a><span data-ttu-id="350f9-149">简单类型</span><span class="sxs-lookup"><span data-stu-id="350f9-149">Simple types</span></span>

<span data-ttu-id="350f9-150">C#提供一组称为***简单类型***的预定义的结构类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-150">C# provides a set of predefined struct types called the ***simple types***.</span></span> <span data-ttu-id="350f9-151">简单类型通过保留字标识，但是这些保留字只是 `System` 命名空间中预定义的结构类型的别名，如下表中所述。</span><span class="sxs-lookup"><span data-stu-id="350f9-151">The simple types are identified through reserved words, but these reserved words are simply aliases for predefined struct types in the `System` namespace, as described in the table below.</span></span>


| <span data-ttu-id="350f9-152">__保留字__</span><span class="sxs-lookup"><span data-stu-id="350f9-152">__Reserved word__</span></span> | <span data-ttu-id="350f9-153">__别名类型__</span><span class="sxs-lookup"><span data-stu-id="350f9-153">__Aliased type__</span></span> |
|-------------------|------------------|
| `sbyte`           | `System.SByte`   | 
| `byte`            | `System.Byte`    | 
| `short`           | `System.Int16`   | 
| `ushort`          | `System.UInt16`  | 
| `int`             | `System.Int32`   | 
| `uint`            | `System.UInt32`  | 
| `long`            | `System.Int64`   | 
| `ulong`           | `System.UInt64`  | 
| `char`            | `System.Char`    | 
| `float`           | `System.Single`  | 
| `double`          | `System.Double`  | 
| `bool`            | `System.Boolean` | 
| `decimal`         | `System.Decimal` | 

<span data-ttu-id="350f9-154">因为简单类型是结构类型的别名，所以每个简单类型都具有成员。</span><span class="sxs-lookup"><span data-stu-id="350f9-154">Because a simple type aliases a struct type, every simple type has members.</span></span> <span data-ttu-id="350f9-155">例如，`int` 在 `System.Int32` 中声明了成员，并且从 `System.Object`继承了成员，并且允许以下语句：</span><span class="sxs-lookup"><span data-stu-id="350f9-155">For example, `int` has the members declared in `System.Int32` and the members inherited from `System.Object`, and the following statements are permitted:</span></span>

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

<span data-ttu-id="350f9-156">简单类型不同于其他结构类型，简单类型允许某些附加操作：</span><span class="sxs-lookup"><span data-stu-id="350f9-156">The simple types differ from other struct types in that they permit certain additional operations:</span></span>

*  <span data-ttu-id="350f9-157">大多数简单类型允许通过编写*文本*（[文本](lexical-structure.md#literals)）来创建值。</span><span class="sxs-lookup"><span data-stu-id="350f9-157">Most simple types permit values to be created by writing *literals* ([Literals](lexical-structure.md#literals)).</span></span> <span data-ttu-id="350f9-158">例如，`123` 是类型 `int` 的文字，`'a'` 是 `char`类型的文字。</span><span class="sxs-lookup"><span data-stu-id="350f9-158">For example, `123` is a literal of type `int` and `'a'` is a literal of type `char`.</span></span> <span data-ttu-id="350f9-159">C#通常不会对结构类型的文本进行任何设置，并且最终总是通过这些结构类型的实例构造函数创建其他结构类型的非默认值。</span><span class="sxs-lookup"><span data-stu-id="350f9-159">C# makes no provision for literals of struct types in general, and non-default values of other struct types are ultimately always created through instance constructors of those struct types.</span></span>
*  <span data-ttu-id="350f9-160">如果表达式的操作数都是简单类型常量，则编译器可能会在编译时计算表达式。</span><span class="sxs-lookup"><span data-stu-id="350f9-160">When the operands of an expression are all simple type constants, it is possible for the compiler to evaluate the expression at compile-time.</span></span> <span data-ttu-id="350f9-161">此类表达式称为*constant_expression* （[常量表达式](expressions.md#constant-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="350f9-161">Such an expression is known as a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)).</span></span> <span data-ttu-id="350f9-162">涉及其他结构类型定义的运算符的表达式不会被视为常量表达式。</span><span class="sxs-lookup"><span data-stu-id="350f9-162">Expressions involving operators defined by other struct types are not considered to be constant expressions.</span></span>
*  <span data-ttu-id="350f9-163">通过 `const` 声明，可以声明简单类型（[常量](classes.md#constants)）的常量。</span><span class="sxs-lookup"><span data-stu-id="350f9-163">Through `const` declarations it is possible to declare constants of the simple types ([Constants](classes.md#constants)).</span></span> <span data-ttu-id="350f9-164">不能有其他结构类型的常量，但 `static readonly` 字段提供了类似的效果。</span><span class="sxs-lookup"><span data-stu-id="350f9-164">It is not possible to have constants of other struct types, but a similar effect is provided by `static readonly` fields.</span></span>
*  <span data-ttu-id="350f9-165">涉及简单类型的转换可以参与对由其他结构类型定义的转换运算符的计算，但用户定义的转换运算符永远不能参与对另一个用户定义的运算符的求值（[计算为用户定义的转换](conversions.md#evaluation-of-user-defined-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="350f9-165">Conversions involving simple types can participate in evaluation of conversion operators defined by other struct types, but a user-defined conversion operator can never participate in evaluation of another user-defined operator ([Evaluation of user-defined conversions](conversions.md#evaluation-of-user-defined-conversions)).</span></span>

### <a name="integral-types"></a><span data-ttu-id="350f9-166">整型</span><span class="sxs-lookup"><span data-stu-id="350f9-166">Integral types</span></span>

<span data-ttu-id="350f9-167">C#支持9整型类型： `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`和 `char`。</span><span class="sxs-lookup"><span data-stu-id="350f9-167">C# supports nine integral types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, and `char`.</span></span> <span data-ttu-id="350f9-168">整数类型具有以下大小和值范围：</span><span class="sxs-lookup"><span data-stu-id="350f9-168">The integral types have the following sizes and ranges of values:</span></span>

*  <span data-ttu-id="350f9-169">`sbyte` 类型表示有符号的8位整数，其值介于-128 和127之间。</span><span class="sxs-lookup"><span data-stu-id="350f9-169">The `sbyte` type represents signed 8-bit integers with values between -128 and 127.</span></span>
*  <span data-ttu-id="350f9-170">`byte` 类型表示值介于0到255之间的无符号8位整数。</span><span class="sxs-lookup"><span data-stu-id="350f9-170">The `byte` type represents unsigned 8-bit integers with values between 0 and 255.</span></span>
*  <span data-ttu-id="350f9-171">`short` 类型表示有符号的16位整数，其值介于-32768 和32767之间。</span><span class="sxs-lookup"><span data-stu-id="350f9-171">The `short` type represents signed 16-bit integers with values between -32768 and 32767.</span></span>
*  <span data-ttu-id="350f9-172">`ushort` 类型表示值介于0到65535之间的16位无符号整数。</span><span class="sxs-lookup"><span data-stu-id="350f9-172">The `ushort` type represents unsigned 16-bit integers with values between 0 and 65535.</span></span>
*  <span data-ttu-id="350f9-173">`int` 类型表示有符号32位整数，其值介于-2147483648 和2147483647之间。</span><span class="sxs-lookup"><span data-stu-id="350f9-173">The `int` type represents signed 32-bit integers with values between -2147483648 and 2147483647.</span></span>
*  <span data-ttu-id="350f9-174">`uint` 类型表示无符号32位整数，其值介于0和4294967295之间。</span><span class="sxs-lookup"><span data-stu-id="350f9-174">The `uint` type represents unsigned 32-bit integers with values between 0 and 4294967295.</span></span>
*  <span data-ttu-id="350f9-175">`long` 类型表示有符号64位整数，其值介于-9223372036854775808 和9223372036854775807之间。</span><span class="sxs-lookup"><span data-stu-id="350f9-175">The `long` type represents signed 64-bit integers with values between -9223372036854775808 and 9223372036854775807.</span></span>
*  <span data-ttu-id="350f9-176">`ulong` 类型表示值介于0和18446744073709551615之间的未签名64位整数。</span><span class="sxs-lookup"><span data-stu-id="350f9-176">The `ulong` type represents unsigned 64-bit integers with values between 0 and 18446744073709551615.</span></span>
*  <span data-ttu-id="350f9-177">`char` 类型表示值介于0到65535之间的16位无符号整数。</span><span class="sxs-lookup"><span data-stu-id="350f9-177">The `char` type represents unsigned 16-bit integers with values between 0 and 65535.</span></span> <span data-ttu-id="350f9-178">`char` 类型的可能值集对应于 Unicode 字符集。</span><span class="sxs-lookup"><span data-stu-id="350f9-178">The set of possible values for the `char` type corresponds to the Unicode character set.</span></span> <span data-ttu-id="350f9-179">尽管 `char` 与 `ushort`具有相同的表示形式，但并不允许对一种类型允许的所有操作。</span><span class="sxs-lookup"><span data-stu-id="350f9-179">Although `char` has the same representation as `ushort`, not all operations permitted on one type are permitted on the other.</span></span>

<span data-ttu-id="350f9-180">整型一元运算符和二元运算符始终操作有符号32位精度、无符号32位精度、有符号64位精度或无符号的64位精度：</span><span class="sxs-lookup"><span data-stu-id="350f9-180">The integral-type unary and binary operators always operate with signed 32-bit precision, unsigned 32-bit precision, signed 64-bit precision, or unsigned 64-bit precision:</span></span>

*  <span data-ttu-id="350f9-181">对于一元 `+` 和 `~` 运算符，操作数转换为类型 `T`，其中 `T` 是 `int`、`uint`、`long`和 `ulong` 中的第一个，可以完全表示操作数的所有可能值。</span><span class="sxs-lookup"><span data-stu-id="350f9-181">For the unary `+` and `~` operators, the operand is converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="350f9-182">然后，使用 `T`类型的精度执行运算，并 `T`结果的类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-182">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span>
*  <span data-ttu-id="350f9-183">对于一元 `-` 运算符，操作数转换为类型 `T`，其中 `T` 是 `int` 和 `long` 中的第一个，可以完全表示操作数的所有可能值。</span><span class="sxs-lookup"><span data-stu-id="350f9-183">For the unary `-` operator, the operand is converted to type `T`, where `T` is the first of `int` and `long` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="350f9-184">然后，使用 `T`类型的精度执行运算，并 `T`结果的类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-184">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span> <span data-ttu-id="350f9-185">一元 `-` 运算符不能应用于 `ulong`类型的操作数。</span><span class="sxs-lookup"><span data-stu-id="350f9-185">The unary `-` operator cannot be applied to operands of type `ulong`.</span></span>
*  <span data-ttu-id="350f9-186">对于二进制 `+`，`-`、`*`、`/`、`%`、`&`、`^`、`|`、`==`、`!=`、`>`、`<`、`>=`和 `<=` 运算符，操作数将转换为类型 `T`，其中 `T` 是 `int`、`uint`、`long`和 `ulong` 中的第一个，可以完全表示这两个操作数的所有可能值。</span><span class="sxs-lookup"><span data-stu-id="350f9-186">For the binary `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`, and `<=` operators, the operands are converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of both operands.</span></span> <span data-ttu-id="350f9-187">然后，使用 `T`类型的精度执行该操作，结果的类型为 `T` （或关系运算符的 `bool`）。</span><span class="sxs-lookup"><span data-stu-id="350f9-187">The operation is then performed using the precision of type `T`, and the type of the result is `T` (or `bool` for the relational operators).</span></span> <span data-ttu-id="350f9-188">不允许一个操作数的类型为 `long`，另一个操作数的类型 `ulong` 为二元运算符。</span><span class="sxs-lookup"><span data-stu-id="350f9-188">It is not permitted for one operand to be of type `long` and the other to be of type `ulong` with the binary operators.</span></span>
*  <span data-ttu-id="350f9-189">对于二进制 `<<` 和 `>>` 运算符，左操作数转换为类型 `T`，其中 `T` 是 `int`、`uint`、`long`和 `ulong` 中的第一个，可以完全表示操作数的所有可能值。</span><span class="sxs-lookup"><span data-stu-id="350f9-189">For the binary `<<` and `>>` operators, the left operand is converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="350f9-190">然后，使用 `T`类型的精度执行运算，并 `T`结果的类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-190">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span>

<span data-ttu-id="350f9-191">`char` 类型分类为整型类型，但它在两个方面与其他整型类型不同：</span><span class="sxs-lookup"><span data-stu-id="350f9-191">The `char` type is classified as an integral type, but it differs from the other integral types in two ways:</span></span>

*  <span data-ttu-id="350f9-192">无法将其他类型隐式转换为 `char` 类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-192">There are no implicit conversions from other types to the `char` type.</span></span> <span data-ttu-id="350f9-193">具体而言，即使 `sbyte`、`byte`和 `ushort` 类型都具有使用 `char` 类型完全表示的值范围，从 `sbyte`、`byte`或 `ushort` 到 `char` 的隐式转换不存在。</span><span class="sxs-lookup"><span data-stu-id="350f9-193">In particular, even though the `sbyte`, `byte`, and `ushort` types have ranges of values that are fully representable using the `char` type, implicit conversions from `sbyte`, `byte`, or `ushort` to `char` do not exist.</span></span>
*  <span data-ttu-id="350f9-194">必须将 `char` 类型的常量编写为*character_literal*s 或*integer_literal*s，并将强制转换为类型 `char`。</span><span class="sxs-lookup"><span data-stu-id="350f9-194">Constants of the `char` type must be written as *character_literal*s or as *integer_literal*s in combination with a cast to type `char`.</span></span> <span data-ttu-id="350f9-195">例如，`(char)10` 和 `'\x000A'` 相同。</span><span class="sxs-lookup"><span data-stu-id="350f9-195">For example, `(char)10` is the same as `'\x000A'`.</span></span>

<span data-ttu-id="350f9-196">`checked` 和 `unchecked` 运算符和语句用于控制整型算术运算和转换的溢出检查（[选中和未选中的运算符](expressions.md#the-checked-and-unchecked-operators)）。</span><span class="sxs-lookup"><span data-stu-id="350f9-196">The `checked` and `unchecked` operators and statements are used to control overflow checking for integral-type arithmetic operations and conversions ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span> <span data-ttu-id="350f9-197">在 `checked` 上下文中，溢出会生成编译时错误或引发 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="350f9-197">In a `checked` context, an overflow produces a compile-time error or causes a `System.OverflowException` to be thrown.</span></span> <span data-ttu-id="350f9-198">在 `unchecked` 上下文中，将忽略溢出，并且不会丢弃任何不适合目标类型的高序位。</span><span class="sxs-lookup"><span data-stu-id="350f9-198">In an `unchecked` context, overflows are ignored and any high-order bits that do not fit in the destination type are discarded.</span></span>

### <a name="floating-point-types"></a><span data-ttu-id="350f9-199">浮点类型</span><span class="sxs-lookup"><span data-stu-id="350f9-199">Floating point types</span></span>

<span data-ttu-id="350f9-200">C#支持两种浮点类型： `float` 和 `double`。</span><span class="sxs-lookup"><span data-stu-id="350f9-200">C# supports two floating point types: `float` and `double`.</span></span> <span data-ttu-id="350f9-201">`float` 和 `double` 类型使用32位单精度和64位双精度 IEEE 754 格式表示，这些格式提供以下值集：</span><span class="sxs-lookup"><span data-stu-id="350f9-201">The `float` and `double` types are represented using the 32-bit single-precision and 64-bit double-precision IEEE 754 formats, which provide the following sets of values:</span></span>

*  <span data-ttu-id="350f9-202">正零和负零。</span><span class="sxs-lookup"><span data-stu-id="350f9-202">Positive zero and negative zero.</span></span> <span data-ttu-id="350f9-203">在大多数情况下，正零和负零的行为与简单值零的行为相同，但某些运算区分这两个（[除法运算符](expressions.md#division-operator)）。</span><span class="sxs-lookup"><span data-stu-id="350f9-203">In most situations, positive zero and negative zero behave identically as the simple value zero, but certain operations distinguish between the two ([Division operator](expressions.md#division-operator)).</span></span>
*  <span data-ttu-id="350f9-204">正无穷大和负无穷大。</span><span class="sxs-lookup"><span data-stu-id="350f9-204">Positive infinity and negative infinity.</span></span> <span data-ttu-id="350f9-205">此类操作会产生无穷大，将非零数除以零。</span><span class="sxs-lookup"><span data-stu-id="350f9-205">Infinities are produced by such operations as dividing a non-zero number by zero.</span></span> <span data-ttu-id="350f9-206">例如，`1.0 / 0.0` 产生正无穷，`-1.0 / 0.0` 产生负无穷。</span><span class="sxs-lookup"><span data-stu-id="350f9-206">For example, `1.0 / 0.0` yields positive infinity, and `-1.0 / 0.0` yields negative infinity.</span></span>
*  <span data-ttu-id="350f9-207">***非数字***值，通常缩写为 NaN。</span><span class="sxs-lookup"><span data-stu-id="350f9-207">The ***Not-a-Number*** value, often abbreviated NaN.</span></span> <span data-ttu-id="350f9-208">Nan 由无效的浮点运算生成，如将零除以零。</span><span class="sxs-lookup"><span data-stu-id="350f9-208">NaNs are produced by invalid floating-point operations, such as dividing zero by zero.</span></span>
*  <span data-ttu-id="350f9-209">`s * m * 2^e`形式的非零的有限值集，其中 `s` 为1或-1，`m` 和 `e` 由特定浮点类型确定：对于 `float`、`0 < m < 2^24` 和 `-149 <= e <= 104`对于 `double`，`0 < m < 2^53` 和 `-1075 <= e <= 970`。</span><span class="sxs-lookup"><span data-stu-id="350f9-209">The finite set of non-zero values of the form `s * m * 2^e`, where `s` is 1 or -1, and `m` and `e` are determined by the particular floating-point type: For `float`, `0 < m < 2^24` and `-149 <= e <= 104`, and for `double`, `0 < m < 2^53` and `-1075 <= e <= 970`.</span></span> <span data-ttu-id="350f9-210">非标准化浮点数被视为有效的非零值。</span><span class="sxs-lookup"><span data-stu-id="350f9-210">Denormalized floating-point numbers are considered valid non-zero values.</span></span>

<span data-ttu-id="350f9-211">`float` 类型可以表示从大约 `1.5 * 10^-45` 到 `3.4 * 10^38` 精度为7位的值。</span><span class="sxs-lookup"><span data-stu-id="350f9-211">The `float` type can represent values ranging from approximately `1.5 * 10^-45` to `3.4 * 10^38` with a precision of 7 digits.</span></span>

<span data-ttu-id="350f9-212">`double` 类型可以表示从大约 `5.0 * 10^-324` 到 `1.7 × 10^308` 精度为15-16 位的值。</span><span class="sxs-lookup"><span data-stu-id="350f9-212">The `double` type can represent values ranging from approximately `5.0 * 10^-324` to `1.7 × 10^308` with a precision of 15-16 digits.</span></span>

<span data-ttu-id="350f9-213">如果二元运算符的一个操作数为浮点类型，则另一个操作数必须为整型类型或浮点类型，并且运算将按如下方式计算：</span><span class="sxs-lookup"><span data-stu-id="350f9-213">If one of the operands of a binary operator is of a floating-point type, then the other operand must be of an integral type or a floating-point type, and the operation is evaluated as follows:</span></span>

*  <span data-ttu-id="350f9-214">如果其中一个操作数为整型类型，则该操作数将转换为另一个操作数的浮点类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-214">If one of the operands is of an integral type, then that operand is converted to the floating-point type of the other operand.</span></span>
*  <span data-ttu-id="350f9-215">然后，如果任意一个操作数为 `double`类型，则另一个操作数将转换为 `double`，则使用至少 `double` 范围和精度执行操作，并且结果的类型为 `double` （或关系运算符的 `bool`）。</span><span class="sxs-lookup"><span data-stu-id="350f9-215">Then, if either of the operands is of type `double`, the other operand is converted to `double`, the operation is performed using at least `double` range and precision, and the type of the result is `double` (or `bool` for the relational operators).</span></span>
*  <span data-ttu-id="350f9-216">否则，使用至少 `float` 范围和精度执行操作，并且结果的类型为 `float` （或关系运算符的 `bool`）。</span><span class="sxs-lookup"><span data-stu-id="350f9-216">Otherwise, the operation is performed using at least `float` range and precision, and the type of the result is `float` (or `bool` for the relational operators).</span></span>

<span data-ttu-id="350f9-217">浮点运算符（包括赋值运算符）从不产生异常。</span><span class="sxs-lookup"><span data-stu-id="350f9-217">The floating-point operators, including the assignment operators, never produce exceptions.</span></span> <span data-ttu-id="350f9-218">相反，在异常情况下，浮点运算产生零、无穷或 NaN，如下所述：</span><span class="sxs-lookup"><span data-stu-id="350f9-218">Instead, in exceptional situations, floating-point operations produce zero, infinity, or NaN, as described below:</span></span>

*  <span data-ttu-id="350f9-219">如果浮点运算的结果对于目标格式来说太小，则运算的结果将变为正零或负零。</span><span class="sxs-lookup"><span data-stu-id="350f9-219">If the result of a floating-point operation is too small for the destination format, the result of the operation becomes positive zero or negative zero.</span></span>
*  <span data-ttu-id="350f9-220">如果浮点运算的结果对于目标格式来说太大，则运算的结果将变为正无穷或负无穷。</span><span class="sxs-lookup"><span data-stu-id="350f9-220">If the result of a floating-point operation is too large for the destination format, the result of the operation becomes positive infinity or negative infinity.</span></span>
*  <span data-ttu-id="350f9-221">如果浮点运算无效，则运算的结果将变成 NaN。</span><span class="sxs-lookup"><span data-stu-id="350f9-221">If a floating-point operation is invalid, the result of the operation becomes NaN.</span></span>
*  <span data-ttu-id="350f9-222">如果浮点运算的一个或两个操作数为 NaN，则运算结果变成 NaN。</span><span class="sxs-lookup"><span data-stu-id="350f9-222">If one or both operands of a floating-point operation is NaN, the result of the operation becomes NaN.</span></span>

<span data-ttu-id="350f9-223">与运算的结果类型相比，浮点运算的精度可能更高。</span><span class="sxs-lookup"><span data-stu-id="350f9-223">Floating-point operations may be performed with higher precision than the result type of the operation.</span></span> <span data-ttu-id="350f9-224">例如，某些硬件体系结构支持比 `double` 类型更大的范围和精度的 "扩展" 或 "long double" 浮点类型，并使用这种更高的精度类型隐式执行所有浮点运算。</span><span class="sxs-lookup"><span data-stu-id="350f9-224">For example, some hardware architectures support an "extended" or "long double" floating-point type with greater range and precision than the `double` type, and implicitly perform all floating-point operations using this higher precision type.</span></span> <span data-ttu-id="350f9-225">仅在性能较高的情况下，才可以使用较小的精度来执行浮点运算，而不需要实现使， C#而不是要求实现更高的精度类型用于所有浮点运算。</span><span class="sxs-lookup"><span data-stu-id="350f9-225">Only at excessive cost in performance can such hardware architectures be made to perform floating-point operations with less precision, and rather than require an implementation to forfeit both performance and precision, C# allows a higher precision type to be used for all floating-point operations.</span></span> <span data-ttu-id="350f9-226">除了提供更精确的结果，这种情况很少会带来任何实实在在的影响。</span><span class="sxs-lookup"><span data-stu-id="350f9-226">Other than delivering more precise results, this rarely has any measurable effects.</span></span> <span data-ttu-id="350f9-227">但是，在形式 `x * y / z`的表达式中，乘法产生的结果不在 `double` 范围内，而后续除法会将临时结果返回到 `double` 范围中，这就是在中计算表达式的事实较大范围格式可能导致生成有限的结果而不是无穷。</span><span class="sxs-lookup"><span data-stu-id="350f9-227">However, in expressions of the form `x * y / z`, where the multiplication produces a result that is outside the `double` range, but the subsequent division brings the temporary result back into the `double` range, the fact that the expression is evaluated in a higher range format may cause a finite result to be produced instead of an infinity.</span></span>

### <a name="the-decimal-type"></a><span data-ttu-id="350f9-228">Decimal 类型</span><span class="sxs-lookup"><span data-stu-id="350f9-228">The decimal type</span></span>

<span data-ttu-id="350f9-229">`decimal` 类型是适用于财务和货币计算的 128 位数据类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-229">The `decimal` type is a 128-bit data type suitable for financial and monetary calculations.</span></span> <span data-ttu-id="350f9-230">`decimal` 类型可以表示从 `1.0 * 10^-28` 到大约 `7.9 * 10^28` 28-29 有效位的值。</span><span class="sxs-lookup"><span data-stu-id="350f9-230">The `decimal` type can represent values ranging from `1.0 * 10^-28` to approximately `7.9 * 10^28` with 28-29 significant digits.</span></span>

<span data-ttu-id="350f9-231">`decimal` 类型的有限值集采用 `(-1)^s * c * 10^-e`形式，其中符号 `s` 是0或1，系数 `c` 由 `0 <= *c* < 2^96`给定，小数位数 `e` 为 `0 <= e <= 28`。`decimal` 类型不支持有符号的零、无穷大或 NaN。</span><span class="sxs-lookup"><span data-stu-id="350f9-231">The finite set of values of type `decimal` are of the form `(-1)^s * c * 10^-e`, where the sign `s` is 0 or 1, the coefficient `c` is given by `0 <= *c* < 2^96`, and the scale `e` is such that `0 <= e <= 28`.The `decimal` type does not support signed zeros, infinities, or NaN's.</span></span> <span data-ttu-id="350f9-232">`decimal` 表示为96位整数，由10的幂进行缩放。</span><span class="sxs-lookup"><span data-stu-id="350f9-232">A `decimal` is represented as a 96-bit integer scaled by a power of ten.</span></span> <span data-ttu-id="350f9-233">对于绝对值小于 `1.0m`的 `decimal`，该值精确到28个小数位，但再也不是。</span><span class="sxs-lookup"><span data-stu-id="350f9-233">For `decimal`s with an absolute value less than `1.0m`, the value is exact to the 28th decimal place, but no further.</span></span> <span data-ttu-id="350f9-234">对于绝对值大于或等于 `1.0m``decimal`s，该值精确到28或29位。</span><span class="sxs-lookup"><span data-stu-id="350f9-234">For `decimal`s with an absolute value greater than or equal to `1.0m`, the value is exact to 28 or 29 digits.</span></span> <span data-ttu-id="350f9-235">与 `float` 和 `double` 数据类型相反，十进制小数值（如0.1）可精确表示 `decimal` 表示形式。</span><span class="sxs-lookup"><span data-stu-id="350f9-235">Contrary to the `float` and `double` data types, decimal fractional numbers such as 0.1 can be represented exactly in the `decimal` representation.</span></span> <span data-ttu-id="350f9-236">在 `float` 和 `double` 表示形式中，此类数字通常为无限小数，使这些表示形式更容易出现舍入错误。</span><span class="sxs-lookup"><span data-stu-id="350f9-236">In the `float` and `double` representations, such numbers are often infinite fractions, making those representations more prone to round-off errors.</span></span>

<span data-ttu-id="350f9-237">如果二元运算符的一个操作数为 `decimal`类型，则另一个操作数必须为整型类型或 `decimal`类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-237">If one of the operands of a binary operator is of type `decimal`, then the other operand must be of an integral type or of type `decimal`.</span></span> <span data-ttu-id="350f9-238">如果存在整数类型操作数，则在执行操作之前，它将转换为 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="350f9-238">If an integral type operand is present, it is converted to `decimal` before the operation is performed.</span></span>

<span data-ttu-id="350f9-239">对类型 `decimal` 的值执行运算的结果是：计算确切的结果（根据每个运算符的定义保留小数位数），然后舍入以适合表示形式。</span><span class="sxs-lookup"><span data-stu-id="350f9-239">The result of an operation on values of type `decimal` is that which would result from calculating an exact result (preserving scale, as defined for each operator) and then rounding to fit the representation.</span></span> <span data-ttu-id="350f9-240">结果将舍入为最接近的可表示值，并且，当结果同样接近两个可表示值时，为在最小有效位位置（称为 "银行家舍入"）中具有偶数的值。</span><span class="sxs-lookup"><span data-stu-id="350f9-240">Results are rounded to the nearest representable value, and, when a result is equally close to two representable values, to the value that has an even number in the least significant digit position (this is known as "banker's rounding").</span></span> <span data-ttu-id="350f9-241">如果结果为零，则其符号始终为0，小数位数为0。</span><span class="sxs-lookup"><span data-stu-id="350f9-241">A zero result always has a sign of 0 and a scale of 0.</span></span>

<span data-ttu-id="350f9-242">如果十进制算术运算生成的值小于或等于绝对值 `5 * 10^-29`，则运算结果将变为零。</span><span class="sxs-lookup"><span data-stu-id="350f9-242">If a decimal arithmetic operation produces a value less than or equal to `5 * 10^-29` in absolute value, the result of the operation becomes zero.</span></span> <span data-ttu-id="350f9-243">如果 `decimal` 算术运算生成的结果对 `decimal` 格式而言太大，则会引发 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="350f9-243">If a `decimal` arithmetic operation produces a result that is too large for the `decimal` format, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="350f9-244">`decimal` 类型具有比浮点类型更高的精度，但范围更小。</span><span class="sxs-lookup"><span data-stu-id="350f9-244">The `decimal` type has greater precision but smaller range than the floating-point types.</span></span> <span data-ttu-id="350f9-245">因此，从浮点类型到 `decimal` 的转换可能会产生溢出异常，而从 `decimal` 到浮点类型的转换可能会导致精度损失。</span><span class="sxs-lookup"><span data-stu-id="350f9-245">Thus, conversions from the floating-point types to `decimal` might produce overflow exceptions, and conversions from `decimal` to the floating-point types might cause loss of precision.</span></span> <span data-ttu-id="350f9-246">由于这些原因，浮点类型和 `decimal`之间不存在隐式转换，并且没有显式强制转换，因此不能在同一个表达式中混合使用浮点和 `decimal` 操作数。</span><span class="sxs-lookup"><span data-stu-id="350f9-246">For these reasons, no implicit conversions exist between the floating-point types and `decimal`, and without explicit casts, it is not possible to mix floating-point and `decimal` operands in the same expression.</span></span>

### <a name="the-bool-type"></a><span data-ttu-id="350f9-247">Bool 类型</span><span class="sxs-lookup"><span data-stu-id="350f9-247">The bool type</span></span>

<span data-ttu-id="350f9-248">`bool` 类型表示布尔的逻辑数量。</span><span class="sxs-lookup"><span data-stu-id="350f9-248">The `bool` type represents boolean logical quantities.</span></span> <span data-ttu-id="350f9-249">`bool` 类型的可能值 `true` 和 `false`。</span><span class="sxs-lookup"><span data-stu-id="350f9-249">The possible values of type `bool` are `true` and `false`.</span></span>

<span data-ttu-id="350f9-250">`bool` 和其他类型之间不存在标准转换。</span><span class="sxs-lookup"><span data-stu-id="350f9-250">No standard conversions exist between `bool` and other types.</span></span> <span data-ttu-id="350f9-251">特别是，`bool` 类型是不同的，并且不同于整型，`bool` 并且不能用于替代整数值，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="350f9-251">In particular, the `bool` type is distinct and separate from the integral types, and a `bool` value cannot be used in place of an integral value, and vice versa.</span></span>

<span data-ttu-id="350f9-252">在 C 和C++语言中，零整数或浮点值，或者 null 指针可以转换为布尔值 `false`、非零整数或浮点值，或者可以将非 null 指针转换为布尔值 `true`。</span><span class="sxs-lookup"><span data-stu-id="350f9-252">In the C and C++ languages, a zero integral or floating-point value, or a null pointer can be converted to the boolean value `false`, and a non-zero integral or floating-point value, or a non-null pointer can be converted to the boolean value `true`.</span></span> <span data-ttu-id="350f9-253">在C#中，此类转换通过将整数或浮点值显式比较为零来实现，或通过将对象引用显式比较为 `null`来实现。</span><span class="sxs-lookup"><span data-stu-id="350f9-253">In C#, such conversions are accomplished by explicitly comparing an integral or floating-point value to zero, or by explicitly comparing an object reference to `null`.</span></span>

### <a name="enumeration-types"></a><span data-ttu-id="350f9-254">枚举类型</span><span class="sxs-lookup"><span data-stu-id="350f9-254">Enumeration types</span></span>

<span data-ttu-id="350f9-255">枚举类型是具有已命名常数的不同类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-255">An enumeration type is a distinct type with named constants.</span></span> <span data-ttu-id="350f9-256">每个枚举类型都有一个基础类型，该类型必须是 `byte`、`sbyte`、`short`、`ushort`、`int`、`uint`、`long` 或 `ulong`。</span><span class="sxs-lookup"><span data-stu-id="350f9-256">Every enumeration type has an underlying type, which must be `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="350f9-257">枚举类型的值集与基础类型的值集相同。</span><span class="sxs-lookup"><span data-stu-id="350f9-257">The set of values of the enumeration type is the same as the set of values of the underlying type.</span></span> <span data-ttu-id="350f9-258">枚举类型的值不局限于命名常量的值。</span><span class="sxs-lookup"><span data-stu-id="350f9-258">Values of the enumeration type are not restricted to the values of the named constants.</span></span> <span data-ttu-id="350f9-259">枚举类型是通过枚举声明（[枚举声明](enums.md#enum-declarations)）定义的。</span><span class="sxs-lookup"><span data-stu-id="350f9-259">Enumeration types are defined through enumeration declarations ([Enum declarations](enums.md#enum-declarations)).</span></span>

### <a name="nullable-types"></a><span data-ttu-id="350f9-260">可以为 null 的类型</span><span class="sxs-lookup"><span data-stu-id="350f9-260">Nullable types</span></span>

<span data-ttu-id="350f9-261">可以为 null 的类型可以表示其***基础类型***的所有值以及其他 null 值。</span><span class="sxs-lookup"><span data-stu-id="350f9-261">A nullable type can represent all values of its ***underlying type*** plus an additional null value.</span></span> <span data-ttu-id="350f9-262">可以为 null 的类型写入 `T?`，其中 `T` 为基础类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-262">A nullable type is written `T?`, where `T` is the underlying type.</span></span> <span data-ttu-id="350f9-263">此语法是 `System.Nullable<T>`的速记，这两种形式可互换使用。</span><span class="sxs-lookup"><span data-stu-id="350f9-263">This syntax is shorthand for `System.Nullable<T>`, and the two forms can be used interchangeably.</span></span>

<span data-ttu-id="350f9-264">与此相反，不***可以为 null 的值类型***为除 `System.Nullable<T>` 和其速记 `T?` 以外的任何值类型（适用于任何 `T`）以及任何被约束为不可为 null 的值类型的类型参数（即，具有 `struct` 的任何类型参数约束）。</span><span class="sxs-lookup"><span data-stu-id="350f9-264">A ***non-nullable value type*** conversely is any value type other than `System.Nullable<T>` and its shorthand `T?` (for any `T`), plus any type parameter that is constrained to be a non-nullable value type (that is, any type parameter with a `struct` constraint).</span></span> <span data-ttu-id="350f9-265">`System.Nullable<T>` 类型为 `T` （[类型参数约束](classes.md#type-parameter-constraints)）指定值类型约束，这意味着可以为 null 的类型的基础类型可以是任何不可以为 null 的值类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-265">The `System.Nullable<T>` type specifies the value type constraint for `T` ([Type parameter constraints](classes.md#type-parameter-constraints)), which means that the underlying type of a nullable type can be any non-nullable value type.</span></span> <span data-ttu-id="350f9-266">可以为 null 的类型的基础类型不能是可以为 null 的类型或引用类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-266">The underlying type of a nullable type cannot be a nullable type or a reference type.</span></span> <span data-ttu-id="350f9-267">例如，`int??` 和 `string?` 是无效类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-267">For example, `int??` and `string?` are invalid types.</span></span>

<span data-ttu-id="350f9-268">可以为 null 的类型的实例 `T?` 有两个公共只读属性：</span><span class="sxs-lookup"><span data-stu-id="350f9-268">An instance of a nullable type `T?` has two public read-only properties:</span></span>

*  <span data-ttu-id="350f9-269">`bool` 类型的 `HasValue` 属性</span><span class="sxs-lookup"><span data-stu-id="350f9-269">A `HasValue` property of type `bool`</span></span>
*  <span data-ttu-id="350f9-270">`T` 类型的 `Value` 属性</span><span class="sxs-lookup"><span data-stu-id="350f9-270">A `Value` property of type `T`</span></span>

<span data-ttu-id="350f9-271">`HasValue` 为 true 的实例被称为非 null。</span><span class="sxs-lookup"><span data-stu-id="350f9-271">An instance for which `HasValue` is true is said to be non-null.</span></span> <span data-ttu-id="350f9-272">非 null 实例包含已知值，`Value` 返回该值。</span><span class="sxs-lookup"><span data-stu-id="350f9-272">A non-null instance contains a known value and `Value` returns that value.</span></span>

<span data-ttu-id="350f9-273">`HasValue` 为 false 的实例称为 null。</span><span class="sxs-lookup"><span data-stu-id="350f9-273">An instance for which `HasValue` is false is said to be null.</span></span> <span data-ttu-id="350f9-274">空实例具有未定义的值。</span><span class="sxs-lookup"><span data-stu-id="350f9-274">A null instance has an undefined value.</span></span> <span data-ttu-id="350f9-275">尝试读取 null 实例的 `Value` 将导致引发 `System.InvalidOperationException`。</span><span class="sxs-lookup"><span data-stu-id="350f9-275">Attempting to read the `Value` of a null instance causes a `System.InvalidOperationException` to be thrown.</span></span> <span data-ttu-id="350f9-276">访问可以为 null 的实例的 `Value` 属性的过程称为***解包***。</span><span class="sxs-lookup"><span data-stu-id="350f9-276">The process of accessing the `Value` property of a nullable instance is referred to as ***unwrapping***.</span></span>

<span data-ttu-id="350f9-277">除了默认构造函数，每个可以为 null 的类型 `T?` 都有一个公共构造函数，该构造函数采用 `T`类型的单个自变量。</span><span class="sxs-lookup"><span data-stu-id="350f9-277">In addition to the default constructor, every nullable type `T?` has a public constructor that takes a single argument of type `T`.</span></span> <span data-ttu-id="350f9-278">给定 `T`类型 `x` 的值，该窗体的构造函数调用</span><span class="sxs-lookup"><span data-stu-id="350f9-278">Given a value `x` of type `T`, a constructor invocation of the form</span></span>

```csharp
new T?(x)
```
<span data-ttu-id="350f9-279">创建 `Value` 属性 `x`的 `T?` 的非 null 实例。</span><span class="sxs-lookup"><span data-stu-id="350f9-279">creates a non-null instance of `T?` for which the `Value` property is `x`.</span></span> <span data-ttu-id="350f9-280">为给定的值创建可为 null 的类型的非 null 实例的过程称为***包装***。</span><span class="sxs-lookup"><span data-stu-id="350f9-280">The process of creating a non-null instance of a nullable type for a given value is referred to as ***wrapping***.</span></span>

<span data-ttu-id="350f9-281">可从 `null` 文本使用隐式转换，以 `T?` （[Null 文本转换](conversions.md#null-literal-conversions)）和从 `T` 到 `T?` （[隐式可为 null 转换](conversions.md#implicit-nullable-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="350f9-281">Implicit conversions are available from the `null` literal to `T?` ([Null literal conversions](conversions.md#null-literal-conversions)) and from `T` to `T?` ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)).</span></span>

## <a name="reference-types"></a><span data-ttu-id="350f9-282">引用类型</span><span class="sxs-lookup"><span data-stu-id="350f9-282">Reference types</span></span>

<span data-ttu-id="350f9-283">引用类型是类类型、接口类型、数组类型或委托类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-283">A reference type is a class type, an interface type, an array type, or a delegate type.</span></span>

```antlr
reference_type
    : class_type
    | interface_type
    | array_type
    | delegate_type
    ;

class_type
    : type_name
    | 'object'
    | 'dynamic'
    | 'string'
    ;

interface_type
    : type_name
    ;

array_type
    : non_array_type rank_specifier+
    ;

non_array_type
    : type
    ;

rank_specifier
    : '[' dim_separator* ']'
    ;

dim_separator
    : ','
    ;

delegate_type
    : type_name
    ;
```

<span data-ttu-id="350f9-284">引用类型值是对类型***实例***的引用，后者称为***对象***。</span><span class="sxs-lookup"><span data-stu-id="350f9-284">A reference type value is a reference to an ***instance*** of the type, the latter known as an ***object***.</span></span> <span data-ttu-id="350f9-285">`null` 的特殊值与所有引用类型兼容，并指示缺少实例。</span><span class="sxs-lookup"><span data-stu-id="350f9-285">The special value `null` is compatible with all reference types and indicates the absence of an instance.</span></span>

### <a name="class-types"></a><span data-ttu-id="350f9-286">类类型</span><span class="sxs-lookup"><span data-stu-id="350f9-286">Class types</span></span>

<span data-ttu-id="350f9-287">类类型定义一个数据结构，该结构包含数据成员（常量和字段）、函数成员（方法、属性、事件、索引器、运算符、实例构造函数、析构函数和静态构造函数）和嵌套类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-287">A class type defines a data structure that contains data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="350f9-288">类类型支持继承，这是一个派生类可以扩展和特殊化基类的机制。</span><span class="sxs-lookup"><span data-stu-id="350f9-288">Class types support inheritance, a mechanism whereby derived classes can extend and specialize base classes.</span></span> <span data-ttu-id="350f9-289">类类型的实例是使用*object_creation_expression*s （[对象创建表达式](expressions.md#object-creation-expressions)）创建的。</span><span class="sxs-lookup"><span data-stu-id="350f9-289">Instances of class types are created using *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>

<span data-ttu-id="350f9-290">类中对类类型进行了[说明。](classes.md)</span><span class="sxs-lookup"><span data-stu-id="350f9-290">Class types are described in [Classes](classes.md).</span></span>

<span data-ttu-id="350f9-291">某些预定义的C#类类型在语言中具有特殊含义，如下表中所述。</span><span class="sxs-lookup"><span data-stu-id="350f9-291">Certain predefined class types have special meaning in the C# language, as described in the table below.</span></span>


| <span data-ttu-id="350f9-292">__类类型__</span><span class="sxs-lookup"><span data-stu-id="350f9-292">__Class type__</span></span>     | <span data-ttu-id="350f9-293">__描述__</span><span class="sxs-lookup"><span data-stu-id="350f9-293">__Description__</span></span>                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | <span data-ttu-id="350f9-294">所有其他类型的最终基类。</span><span class="sxs-lookup"><span data-stu-id="350f9-294">The ultimate base class of all other types.</span></span> <span data-ttu-id="350f9-295">请参阅[对象类型](types.md#the-object-type)。</span><span class="sxs-lookup"><span data-stu-id="350f9-295">See [The object type](types.md#the-object-type).</span></span> | 
| `System.String`    | <span data-ttu-id="350f9-296">C#语言的字符串类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-296">The string type of the C# language.</span></span> <span data-ttu-id="350f9-297">请参阅[字符串类型](types.md#the-string-type)。</span><span class="sxs-lookup"><span data-stu-id="350f9-297">See [The string type](types.md#the-string-type).</span></span>         |
| `System.ValueType` | <span data-ttu-id="350f9-298">所有值类型的基类。</span><span class="sxs-lookup"><span data-stu-id="350f9-298">The base class of all value types.</span></span> <span data-ttu-id="350f9-299">请参阅[system.object 类型](types.md#the-systemvaluetype-type)。</span><span class="sxs-lookup"><span data-stu-id="350f9-299">See [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>          |
| `System.Enum`      | <span data-ttu-id="350f9-300">所有枚举类型的基类。</span><span class="sxs-lookup"><span data-stu-id="350f9-300">The base class of all enum types.</span></span> <span data-ttu-id="350f9-301">请参阅[枚举](enums.md)。</span><span class="sxs-lookup"><span data-stu-id="350f9-301">See [Enums](enums.md).</span></span>              |
| `System.Array`     | <span data-ttu-id="350f9-302">所有数组类型的基类。</span><span class="sxs-lookup"><span data-stu-id="350f9-302">The base class of all array types.</span></span> <span data-ttu-id="350f9-303">请参阅[数组](arrays.md)。</span><span class="sxs-lookup"><span data-stu-id="350f9-303">See [Arrays](arrays.md).</span></span>             |
| `System.Delegate`  | <span data-ttu-id="350f9-304">所有委托类型的基类。</span><span class="sxs-lookup"><span data-stu-id="350f9-304">The base class of all delegate types.</span></span> <span data-ttu-id="350f9-305">请参阅[委托](delegates.md)。</span><span class="sxs-lookup"><span data-stu-id="350f9-305">See [Delegates](delegates.md).</span></span>          |
| `System.Exception` | <span data-ttu-id="350f9-306">所有异常类型的基类。</span><span class="sxs-lookup"><span data-stu-id="350f9-306">The base class of all exception types.</span></span> <span data-ttu-id="350f9-307">请参见[异常](exceptions.md)。</span><span class="sxs-lookup"><span data-stu-id="350f9-307">See [Exceptions](exceptions.md).</span></span>         |

### <a name="the-object-type"></a><span data-ttu-id="350f9-308">对象类型</span><span class="sxs-lookup"><span data-stu-id="350f9-308">The object type</span></span>

<span data-ttu-id="350f9-309">`object` 类类型是所有其他类型的最终基类。</span><span class="sxs-lookup"><span data-stu-id="350f9-309">The `object` class type is the ultimate base class of all other types.</span></span> <span data-ttu-id="350f9-310">C#直接或间接派生自 `object` 类类型的每个类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-310">Every type in C# directly or indirectly derives from the `object` class type.</span></span>

<span data-ttu-id="350f9-311">关键字 `object` 只是预定义的类 `System.Object`的别名。</span><span class="sxs-lookup"><span data-stu-id="350f9-311">The keyword `object` is simply an alias for the predefined class `System.Object`.</span></span>

### <a name="the-dynamic-type"></a><span data-ttu-id="350f9-312">动态类型</span><span class="sxs-lookup"><span data-stu-id="350f9-312">The dynamic type</span></span>

<span data-ttu-id="350f9-313">`dynamic` 类型（如 `object`）可引用任何对象。</span><span class="sxs-lookup"><span data-stu-id="350f9-313">The `dynamic` type, like `object`, can reference any object.</span></span> <span data-ttu-id="350f9-314">将运算符应用于类型 `dynamic`的表达式时，其解决方法会推迟到程序运行。</span><span class="sxs-lookup"><span data-stu-id="350f9-314">When operators are applied to expressions of type `dynamic`, their resolution is deferred until the program is run.</span></span> <span data-ttu-id="350f9-315">因此，如果运算符无法合法地应用于被引用对象，则在编译过程中不会提供错误。</span><span class="sxs-lookup"><span data-stu-id="350f9-315">Thus, if the operator cannot legally be applied to the referenced object, no error is given during compilation.</span></span> <span data-ttu-id="350f9-316">当运算符的解析在运行时失败时，将引发异常。</span><span class="sxs-lookup"><span data-stu-id="350f9-316">Instead an exception will be thrown when resolution of the operator fails at run-time.</span></span>

<span data-ttu-id="350f9-317">其目的是允许动态绑定，这在[动态绑定](expressions.md#dynamic-binding)中进行了详细说明。</span><span class="sxs-lookup"><span data-stu-id="350f9-317">Its purpose is to allow dynamic binding, which is described in detail in [Dynamic binding](expressions.md#dynamic-binding).</span></span>

<span data-ttu-id="350f9-318">`dynamic` 被视为等同于 `object`，但以下方面除外：</span><span class="sxs-lookup"><span data-stu-id="350f9-318">`dynamic` is considered identical to `object` except in the following respects:</span></span>

*  <span data-ttu-id="350f9-319">`dynamic` 类型的表达式的操作可以动态绑定（[动态绑定](expressions.md#dynamic-binding)）。</span><span class="sxs-lookup"><span data-stu-id="350f9-319">Operations on expressions of type `dynamic` can be dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span>
*  <span data-ttu-id="350f9-320">如果两者都是候选项，则类型推理（[类型推理](expressions.md#type-inference)）优先于 `object` `dynamic`。</span><span class="sxs-lookup"><span data-stu-id="350f9-320">Type inference ([Type inference](expressions.md#type-inference)) will prefer `dynamic` over `object` if both are candidates.</span></span>

<span data-ttu-id="350f9-321">由于这种等效性，以下内容包含：</span><span class="sxs-lookup"><span data-stu-id="350f9-321">Because of this equivalence, the following holds:</span></span>

*  <span data-ttu-id="350f9-322">`object` 和 `dynamic`之间存在隐式标识转换，在将 `dynamic` 替换为 `object`</span><span class="sxs-lookup"><span data-stu-id="350f9-322">There is an implicit identity conversion between `object` and `dynamic`, and between constructed types that are the same when replacing `dynamic` with `object`</span></span>
*  <span data-ttu-id="350f9-323">与 `object` 的隐式和显式转换也适用于 `dynamic`。</span><span class="sxs-lookup"><span data-stu-id="350f9-323">Implicit and explicit conversions to and from `object` also apply to and from `dynamic`.</span></span>
*  <span data-ttu-id="350f9-324">将 `dynamic` 替换为 `object` 时相同的方法签名被视为相同的签名</span><span class="sxs-lookup"><span data-stu-id="350f9-324">Method signatures that are the same when replacing `dynamic` with `object` are considered the same signature</span></span>
*  <span data-ttu-id="350f9-325">类型 `dynamic` 在运行时与 `object` 不区分。</span><span class="sxs-lookup"><span data-stu-id="350f9-325">The type `dynamic` is indistinguishable from `object` at run-time.</span></span>
*  <span data-ttu-id="350f9-326">`dynamic` 类型的表达式称为***动态表达式***。</span><span class="sxs-lookup"><span data-stu-id="350f9-326">An expression of the type `dynamic` is referred to as a ***dynamic expression***.</span></span>

### <a name="the-string-type"></a><span data-ttu-id="350f9-327">字符串类型</span><span class="sxs-lookup"><span data-stu-id="350f9-327">The string type</span></span>

<span data-ttu-id="350f9-328">`string` 类型是直接从 `object`继承的密封类类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-328">The `string` type is a sealed class type that inherits directly from `object`.</span></span> <span data-ttu-id="350f9-329">`string` 类的实例表示 Unicode 字符串。</span><span class="sxs-lookup"><span data-stu-id="350f9-329">Instances of the `string` class represent Unicode character strings.</span></span>

<span data-ttu-id="350f9-330">`string` 类型的值可以编写为字符串文本（[字符串文字](lexical-structure.md#string-literals)）。</span><span class="sxs-lookup"><span data-stu-id="350f9-330">Values of the `string` type can be written as string literals ([String literals](lexical-structure.md#string-literals)).</span></span>

<span data-ttu-id="350f9-331">关键字 `string` 只是预定义的类 `System.String`的别名。</span><span class="sxs-lookup"><span data-stu-id="350f9-331">The keyword `string` is simply an alias for the predefined class `System.String`.</span></span>

### <a name="interface-types"></a><span data-ttu-id="350f9-332">接口类型</span><span class="sxs-lookup"><span data-stu-id="350f9-332">Interface types</span></span>

<span data-ttu-id="350f9-333">接口定义协定。</span><span class="sxs-lookup"><span data-stu-id="350f9-333">An interface defines a contract.</span></span> <span data-ttu-id="350f9-334">实现接口的类或结构必须遵循它的协定。</span><span class="sxs-lookup"><span data-stu-id="350f9-334">A class or struct that implements an interface must adhere to its contract.</span></span> <span data-ttu-id="350f9-335">接口可以从多个基接口继承，类或结构可以实现多个接口。</span><span class="sxs-lookup"><span data-stu-id="350f9-335">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="350f9-336">接口类型在[接口](interfaces.md)中进行了介绍。</span><span class="sxs-lookup"><span data-stu-id="350f9-336">Interface types are described in [Interfaces](interfaces.md).</span></span>

### <a name="array-types"></a><span data-ttu-id="350f9-337">数组类型</span><span class="sxs-lookup"><span data-stu-id="350f9-337">Array types</span></span>

<span data-ttu-id="350f9-338">数组是一种数据结构，其中包含零个或多个通过计算索引访问的变量。</span><span class="sxs-lookup"><span data-stu-id="350f9-338">An array is a data structure that contains zero or more variables which are accessed through computed indices.</span></span> <span data-ttu-id="350f9-339">数组中包含的变量（也称为数组的元素）都属于同一类型，并且此类型称为数组的元素类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-339">The variables contained in an array, also called the elements of the array, are all of the same type, and this type is called the element type of the array.</span></span>

<span data-ttu-id="350f9-340">数组中介绍了数组[类型。](arrays.md)</span><span class="sxs-lookup"><span data-stu-id="350f9-340">Array types are described in [Arrays](arrays.md).</span></span>

### <a name="delegate-types"></a><span data-ttu-id="350f9-341">委托类型</span><span class="sxs-lookup"><span data-stu-id="350f9-341">Delegate types</span></span>

<span data-ttu-id="350f9-342">委托是指一个或多个方法的数据结构。</span><span class="sxs-lookup"><span data-stu-id="350f9-342">A delegate is a data structure that refers to one or more methods.</span></span> <span data-ttu-id="350f9-343">对于实例方法，它还引用其相应的对象实例。</span><span class="sxs-lookup"><span data-stu-id="350f9-343">For instance methods, it also refers to their corresponding object instances.</span></span>

<span data-ttu-id="350f9-344">C 或C++中委托的最接近等效项是函数指针，但函数指针只能引用静态函数，而委托可以引用静态和实例方法。</span><span class="sxs-lookup"><span data-stu-id="350f9-344">The closest equivalent of a delegate in C or C++ is a function pointer, but whereas a function pointer can only reference static functions, a delegate can reference both static and instance methods.</span></span> <span data-ttu-id="350f9-345">在后一种情况下，委托不仅存储对方法入口点的引用，还存储对调用方法的对象实例的引用。</span><span class="sxs-lookup"><span data-stu-id="350f9-345">In the latter case, the delegate stores not only a reference to the method's entry point, but also a reference to the object instance on which to invoke the method.</span></span>

<span data-ttu-id="350f9-346">[委托中介绍](delegates.md)了委托类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-346">Delegate types are described in [Delegates](delegates.md).</span></span>

## <a name="boxing-and-unboxing"></a><span data-ttu-id="350f9-347">装箱和取消装箱</span><span class="sxs-lookup"><span data-stu-id="350f9-347">Boxing and unboxing</span></span>

<span data-ttu-id="350f9-348">装箱和取消装箱的概念是类型系统C#的核心。</span><span class="sxs-lookup"><span data-stu-id="350f9-348">The concept of boxing and unboxing is central to C#'s type system.</span></span> <span data-ttu-id="350f9-349">它通过允许将*value_type*的任何值相互转换为类型 `object`，在*value_type*s 和*reference_type*之间提供桥梁。</span><span class="sxs-lookup"><span data-stu-id="350f9-349">It provides a bridge between *value_type*s and *reference_type*s by permitting any value of a *value_type* to be converted to and from type `object`.</span></span> <span data-ttu-id="350f9-350">装箱和取消装箱可实现类型系统的统一视图，其中，任何类型的值最终都可以被视为对象。</span><span class="sxs-lookup"><span data-stu-id="350f9-350">Boxing and unboxing enables a unified view of the type system wherein a value of any type can ultimately be treated as an object.</span></span>

### <a name="boxing-conversions"></a><span data-ttu-id="350f9-351">装箱转换</span><span class="sxs-lookup"><span data-stu-id="350f9-351">Boxing conversions</span></span>

<span data-ttu-id="350f9-352">装箱转换允许将*value_type*隐式转换为*reference_type*。</span><span class="sxs-lookup"><span data-stu-id="350f9-352">A boxing conversion permits a *value_type* to be implicitly converted to a *reference_type*.</span></span> <span data-ttu-id="350f9-353">存在以下装箱转换：</span><span class="sxs-lookup"><span data-stu-id="350f9-353">The following boxing conversions exist:</span></span>

*  <span data-ttu-id="350f9-354">从任何*value_type*到类型 `object`。</span><span class="sxs-lookup"><span data-stu-id="350f9-354">From any *value_type* to the type `object`.</span></span>
*  <span data-ttu-id="350f9-355">从任何*value_type*到类型 `System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="350f9-355">From any *value_type* to the type `System.ValueType`.</span></span>
*  <span data-ttu-id="350f9-356">从任何*non_nullable_value_type*到*value_type*实现的任何*interface_type* 。</span><span class="sxs-lookup"><span data-stu-id="350f9-356">From any *non_nullable_value_type* to any *interface_type* implemented by the *value_type*.</span></span>
*  <span data-ttu-id="350f9-357">从任何*nullable_type*到由*nullable_type*的基础类型实现的任何*interface_type* 。</span><span class="sxs-lookup"><span data-stu-id="350f9-357">From any *nullable_type* to any *interface_type* implemented by the underlying type of the *nullable_type*.</span></span>
*  <span data-ttu-id="350f9-358">从任何*为*到类型 `System.Enum`。</span><span class="sxs-lookup"><span data-stu-id="350f9-358">From any *enum_type* to the type `System.Enum`.</span></span>
*  <span data-ttu-id="350f9-359">从具有基础*为*的任何*nullable_type*到类型 `System.Enum`。</span><span class="sxs-lookup"><span data-stu-id="350f9-359">From any *nullable_type* with an underlying *enum_type* to the type `System.Enum`.</span></span>
*  <span data-ttu-id="350f9-360">请注意，如果在运行时从值类型到引用类型的转换（[涉及类型参数的隐式转换](conversions.md#implicit-conversions-involving-type-parameters)）结束，则从类型参数进行的隐式转换将作为装箱转换执行。</span><span class="sxs-lookup"><span data-stu-id="350f9-360">Note that an implicit conversion from a type parameter will be executed as a boxing conversion if at run-time it ends up converting from a value type to a reference type ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters)).</span></span>

<span data-ttu-id="350f9-361">将*non_nullable_value_type*的值装箱包括分配对象实例，并将*non_nullable_value_type*值复制到该实例中。</span><span class="sxs-lookup"><span data-stu-id="350f9-361">Boxing a value of a *non_nullable_value_type* consists of allocating an object instance and copying the *non_nullable_value_type* value into that instance.</span></span>

<span data-ttu-id="350f9-362">如果将*nullable_type*的值装箱为 `null` 值（`HasValue` 为 `false`）或解包和装箱基础值的结果，则会生成空引用。</span><span class="sxs-lookup"><span data-stu-id="350f9-362">Boxing a value of a *nullable_type* produces a null reference if it is the `null` value (`HasValue` is `false`), or the result of unwrapping and boxing the underlying value otherwise.</span></span>

<span data-ttu-id="350f9-363">将*non_nullable_value_type*的值装箱的实际过程最好通过应该构想泛型***装箱类***的存在进行说明，该类的行为类似于声明为以下形式：</span><span class="sxs-lookup"><span data-stu-id="350f9-363">The actual process of boxing a value of a *non_nullable_value_type* is best explained by imagining the existence of a generic ***boxing class***, which behaves as if it were declared as follows:</span></span>

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

<span data-ttu-id="350f9-364">`T` 类型的值 `v` 的装箱现在包含执行表达式 `new Box<T>(v)`，并将生成的实例作为类型 `object`的值返回。</span><span class="sxs-lookup"><span data-stu-id="350f9-364">Boxing of a value `v` of type `T` now consists of executing the expression `new Box<T>(v)`, and returning the resulting instance as a value of type `object`.</span></span> <span data-ttu-id="350f9-365">因此，语句</span><span class="sxs-lookup"><span data-stu-id="350f9-365">Thus, the statements</span></span>
```csharp
int i = 123;
object box = i;
```
<span data-ttu-id="350f9-366">概念上对应于</span><span class="sxs-lookup"><span data-stu-id="350f9-366">conceptually correspond to</span></span>
```csharp
int i = 123;
object box = new Box<int>(i);
```

<span data-ttu-id="350f9-367">上面的装箱类（如 `Box<T>`）实际上不存在，装箱值的动态类型实际上不是类类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-367">A boxing class like `Box<T>` above doesn't actually exist and the dynamic type of a boxed value isn't actually a class type.</span></span> <span data-ttu-id="350f9-368">相反，类型 `T` 的装箱值具有动态类型 `T`，使用 `is` 运算符的动态类型检查只需引用类型 `T`即可。</span><span class="sxs-lookup"><span data-stu-id="350f9-368">Instead, a boxed value of type `T` has the dynamic type `T`, and a dynamic type check using the `is` operator can simply reference type `T`.</span></span> <span data-ttu-id="350f9-369">例如，应用于对象的</span><span class="sxs-lookup"><span data-stu-id="350f9-369">For example,</span></span>
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
<span data-ttu-id="350f9-370">会在控制台上输出字符串 "`Box contains an int`"。</span><span class="sxs-lookup"><span data-stu-id="350f9-370">will output the string "`Box contains an int`" on the console.</span></span>

<span data-ttu-id="350f9-371">装箱转换表示生成装箱的值的副本。</span><span class="sxs-lookup"><span data-stu-id="350f9-371">A boxing conversion implies making a copy of the value being boxed.</span></span> <span data-ttu-id="350f9-372">这不同于*reference_type*到类型 `object`的转换，在这种情况下，该值会继续引用相同的实例，并且只被视为 `object`派生程度较小的类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-372">This is different from a conversion of a *reference_type* to type `object`, in which the value continues to reference the same instance and simply is regarded as the less derived type `object`.</span></span> <span data-ttu-id="350f9-373">例如，给定声明</span><span class="sxs-lookup"><span data-stu-id="350f9-373">For example, given the declaration</span></span>
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
<span data-ttu-id="350f9-374">以下语句</span><span class="sxs-lookup"><span data-stu-id="350f9-374">the following statements</span></span>
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
<span data-ttu-id="350f9-375">会在控制台上输出值10，因为在 `p` 分配给 `box` 时发生的隐式装箱操作会导致复制 `p` 的值。</span><span class="sxs-lookup"><span data-stu-id="350f9-375">will output the value 10 on the console because the implicit boxing operation that occurs in the assignment of `p` to `box` causes the value of `p` to be copied.</span></span> <span data-ttu-id="350f9-376">改为 `Point` 声明 `class`，因为 `p` 和 `box` 将引用同一个实例，所以值为20。</span><span class="sxs-lookup"><span data-stu-id="350f9-376">Had `Point` been declared a `class` instead, the value 20 would be output because `p` and `box` would reference the same instance.</span></span>

### <a name="unboxing-conversions"></a><span data-ttu-id="350f9-377">取消装箱转换</span><span class="sxs-lookup"><span data-stu-id="350f9-377">Unboxing conversions</span></span>

<span data-ttu-id="350f9-378">取消装箱转换允许将*reference_type*显式转换为*value_type*。</span><span class="sxs-lookup"><span data-stu-id="350f9-378">An unboxing conversion permits a *reference_type* to be explicitly converted to a *value_type*.</span></span> <span data-ttu-id="350f9-379">存在以下取消装箱转换：</span><span class="sxs-lookup"><span data-stu-id="350f9-379">The following unboxing conversions exist:</span></span>

*  <span data-ttu-id="350f9-380">从类型 `object` 到任何*value_type*。</span><span class="sxs-lookup"><span data-stu-id="350f9-380">From the type `object` to any *value_type*.</span></span>
*  <span data-ttu-id="350f9-381">从类型 `System.ValueType` 到任何*value_type*。</span><span class="sxs-lookup"><span data-stu-id="350f9-381">From the type `System.ValueType` to any *value_type*.</span></span>
*  <span data-ttu-id="350f9-382">从任何*interface_type*到任何实现*interface_type*的*non_nullable_value_type* 。</span><span class="sxs-lookup"><span data-stu-id="350f9-382">From any *interface_type* to any *non_nullable_value_type* that implements the *interface_type*.</span></span>
*  <span data-ttu-id="350f9-383">从任何*interface_type*到任何基础类型都实现*interface_type*的*nullable_type* 。</span><span class="sxs-lookup"><span data-stu-id="350f9-383">From any *interface_type* to any *nullable_type* whose underlying type implements the *interface_type*.</span></span>
*  <span data-ttu-id="350f9-384">从类型 `System.Enum` 到任何*为*。</span><span class="sxs-lookup"><span data-stu-id="350f9-384">From the type `System.Enum` to any *enum_type*.</span></span>
*  <span data-ttu-id="350f9-385">从类型 `System.Enum` 到具有基础*为*的任何*nullable_type* 。</span><span class="sxs-lookup"><span data-stu-id="350f9-385">From the type `System.Enum` to any *nullable_type* with an underlying *enum_type*.</span></span>
*  <span data-ttu-id="350f9-386">请注意，如果在运行时结束将引用类型转换为值类型（[显式动态转换](conversions.md#explicit-dynamic-conversions)），则会将显式转换为类型参数，以取消装箱转换。</span><span class="sxs-lookup"><span data-stu-id="350f9-386">Note that an explicit conversion to a type parameter will be executed as an unboxing conversion if at run-time it ends up converting from a reference type to a value type ([Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions)).</span></span>

<span data-ttu-id="350f9-387">对*non_nullable_value_type*的取消装箱操作包括：首先检查对象实例是否是给定*non_nullable_value_type*的装箱值，然后将值复制到该实例之外。</span><span class="sxs-lookup"><span data-stu-id="350f9-387">An unboxing operation to a *non_nullable_value_type* consists of first checking that the object instance is a boxed value of the given *non_nullable_value_type*, and then copying the value out of the instance.</span></span>

<span data-ttu-id="350f9-388">如果 `null`的源操作数为 null，则为对*nullable_type*的取消装箱将生成*nullable_type*的 null 值; 否则，会将对象实例取消装箱的结果返回到*nullable_type*的基础类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-388">Unboxing to a *nullable_type* produces the null value of the *nullable_type* if the source operand is `null`, or the wrapped result of unboxing the object instance to the underlying type of the *nullable_type* otherwise.</span></span>

<span data-ttu-id="350f9-389">使用上一节中所述的虚部装箱类，`box` 到*value_type* `T` 的对象的取消装箱转换包含执行表达式 `((Box<T>)box).value`。</span><span class="sxs-lookup"><span data-stu-id="350f9-389">Referring to the imaginary boxing class described in the previous section, an unboxing conversion of an object `box` to a *value_type* `T` consists of executing the expression `((Box<T>)box).value`.</span></span> <span data-ttu-id="350f9-390">因此，语句</span><span class="sxs-lookup"><span data-stu-id="350f9-390">Thus, the statements</span></span>
```csharp
object box = 123;
int i = (int)box;
```
<span data-ttu-id="350f9-391">概念上对应于</span><span class="sxs-lookup"><span data-stu-id="350f9-391">conceptually correspond to</span></span>
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

<span data-ttu-id="350f9-392">若要在运行时成功转换到给定的*non_nullable_value_type* ，源操作数的值必须是该*non_nullable_value_type*的装箱值的引用。</span><span class="sxs-lookup"><span data-stu-id="350f9-392">For an unboxing conversion to a given *non_nullable_value_type* to succeed at run-time, the value of the source operand must be a reference to a boxed value of that *non_nullable_value_type*.</span></span> <span data-ttu-id="350f9-393">如果 `null`源操作数，则会引发 `System.NullReferenceException`。</span><span class="sxs-lookup"><span data-stu-id="350f9-393">If the source operand is `null`, a `System.NullReferenceException` is thrown.</span></span> <span data-ttu-id="350f9-394">如果源操作数是对不兼容的对象的引用，则会引发 `System.InvalidCastException`。</span><span class="sxs-lookup"><span data-stu-id="350f9-394">If the source operand is a reference to an incompatible object, a `System.InvalidCastException` is thrown.</span></span>

<span data-ttu-id="350f9-395">若要在运行时成功转换到给定的*nullable_type* ，源操作数的值必须是 `null` 或对*nullable_type*基础*non_nullable_value_type*的装箱值的引用。</span><span class="sxs-lookup"><span data-stu-id="350f9-395">For an unboxing conversion to a given *nullable_type* to succeed at run-time, the value of the source operand must be either `null` or a reference to a boxed value of the underlying *non_nullable_value_type* of the *nullable_type*.</span></span> <span data-ttu-id="350f9-396">如果源操作数是对不兼容的对象的引用，则会引发 `System.InvalidCastException`。</span><span class="sxs-lookup"><span data-stu-id="350f9-396">If the source operand is a reference to an incompatible object, a `System.InvalidCastException` is thrown.</span></span>

## <a name="constructed-types"></a><span data-ttu-id="350f9-397">构造类型</span><span class="sxs-lookup"><span data-stu-id="350f9-397">Constructed types</span></span>

<span data-ttu-id="350f9-398">泛型类型声明本身表示***未绑定的泛型类型***，该类型用作 "蓝图" 以形成多种不同类型，方法是应用***类型参数***。</span><span class="sxs-lookup"><span data-stu-id="350f9-398">A generic type declaration, by itself, denotes an ***unbound generic type*** that is used as a "blueprint" to form many different types, by way of applying ***type arguments***.</span></span> <span data-ttu-id="350f9-399">类型参数以紧跟在泛型类型名称后面的尖括号（`<` 和 `>`）来编写。</span><span class="sxs-lookup"><span data-stu-id="350f9-399">The type arguments are written within angle brackets (`<` and `>`) immediately following the name of the generic type.</span></span> <span data-ttu-id="350f9-400">包含至少一个类型参数的类型称为***构造类型***。</span><span class="sxs-lookup"><span data-stu-id="350f9-400">A type that includes at least one type argument is called a ***constructed type***.</span></span> <span data-ttu-id="350f9-401">构造类型可用于语言中可显示类型名称的大多数位置。</span><span class="sxs-lookup"><span data-stu-id="350f9-401">A constructed type can be used in most places in the language in which a type name can appear.</span></span> <span data-ttu-id="350f9-402">未绑定的泛型类型只能在*typeof_expression* （[typeof 运算符](expressions.md#the-typeof-operator)）中使用。</span><span class="sxs-lookup"><span data-stu-id="350f9-402">An unbound generic type can only be used within a *typeof_expression* ([The typeof operator](expressions.md#the-typeof-operator)).</span></span>

<span data-ttu-id="350f9-403">构造类型还可以在表达式中用作简单名称（[简单名称](expressions.md#simple-names)），也可以在访问成员（[成员访问](expressions.md#member-access)）时使用。</span><span class="sxs-lookup"><span data-stu-id="350f9-403">Constructed types can also be used in expressions as simple names ([Simple names](expressions.md#simple-names)) or when accessing a member ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="350f9-404">计算*namespace_or_type_name*时，只考虑具有正确数量的类型参数的泛型类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-404">When a *namespace_or_type_name* is evaluated, only generic types with the correct number of type parameters are considered.</span></span> <span data-ttu-id="350f9-405">因此，只要类型具有不同数目的类型参数，就可以使用相同的标识符来识别不同的类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-405">Thus, it is possible to use the same identifier to identify different types, as long as the types have different numbers of type parameters.</span></span> <span data-ttu-id="350f9-406">在同一程序中混合泛型和非泛型类时，这很有用：</span><span class="sxs-lookup"><span data-stu-id="350f9-406">This is useful when mixing generic and non-generic classes in the same program:</span></span>

```csharp
namespace Widgets
{
    class Queue {...}
    class Queue<TElement> {...}
}

namespace MyApplication
{
    using Widgets;

    class X
    {
        Queue q1;            // Non-generic Widgets.Queue
        Queue<int> q2;       // Generic Widgets.Queue
    }
}
```

<span data-ttu-id="350f9-407">*Type_name*可以标识构造的类型，即使它不直接指定类型参数也是如此。</span><span class="sxs-lookup"><span data-stu-id="350f9-407">A *type_name* might identify a constructed type even though it doesn't specify type parameters directly.</span></span> <span data-ttu-id="350f9-408">如果类型嵌套在泛型类声明中，并且包含声明的实例类型隐式用于名称查找（[泛型类中的嵌套类型](classes.md#nested-types-in-generic-classes)），则可能会发生这种情况。</span><span class="sxs-lookup"><span data-stu-id="350f9-408">This can occur where a type is nested within a generic class declaration, and the instance type of the containing declaration is implicitly used for name lookup ([Nested types in generic classes](classes.md#nested-types-in-generic-classes)):</span></span>

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

<span data-ttu-id="350f9-409">在不安全代码中，构造类型不能用作*unmanaged_type* （[指针类型](unsafe-code.md#pointer-types)）。</span><span class="sxs-lookup"><span data-stu-id="350f9-409">In unsafe code, a constructed type cannot be used as an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

### <a name="type-arguments"></a><span data-ttu-id="350f9-410">类型参数</span><span class="sxs-lookup"><span data-stu-id="350f9-410">Type arguments</span></span>

<span data-ttu-id="350f9-411">类型实参列表中的每个实参只是一*种类型*。</span><span class="sxs-lookup"><span data-stu-id="350f9-411">Each argument in a type argument list is simply a *type*.</span></span>

```antlr
type_argument_list
    : '<' type_arguments '>'
    ;

type_arguments
    : type_argument (',' type_argument)*
    ;

type_argument
    : type
    ;
```

<span data-ttu-id="350f9-412">在不安全代码（[不安全](unsafe-code.md)代码）中， *type_argument*可能不是指针类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-412">In unsafe code ([Unsafe code](unsafe-code.md)), a *type_argument* may not be a pointer type.</span></span> <span data-ttu-id="350f9-413">每个类型参数都必须满足相应类型参数的任何约束（[类型参数约束](classes.md#type-parameter-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="350f9-413">Each type argument must satisfy any constraints on the corresponding type parameter ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>

### <a name="open-and-closed-types"></a><span data-ttu-id="350f9-414">打开和关闭的类型</span><span class="sxs-lookup"><span data-stu-id="350f9-414">Open and closed types</span></span>

<span data-ttu-id="350f9-415">所有类型都可以分类为***开放式类型***或***关闭类型***。</span><span class="sxs-lookup"><span data-stu-id="350f9-415">All types can be classified as either ***open types*** or ***closed types***.</span></span> <span data-ttu-id="350f9-416">开放式类型是一种涉及类型参数的类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-416">An open type is a type that involves type parameters.</span></span> <span data-ttu-id="350f9-417">更具体地说：</span><span class="sxs-lookup"><span data-stu-id="350f9-417">More specifically:</span></span>

*  <span data-ttu-id="350f9-418">类型参数定义开放类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-418">A type parameter defines an open type.</span></span>
*  <span data-ttu-id="350f9-419">当且仅当数组的元素类型为开放类型时，该类型才是开放类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-419">An array type is an open type if and only if its element type is an open type.</span></span>
*  <span data-ttu-id="350f9-420">当且仅当一个或多个类型参数是开放类型时，构造类型是开放类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-420">A constructed type is an open type if and only if one or more of its type arguments is an open type.</span></span> <span data-ttu-id="350f9-421">当且仅当其包含类型的一个或多个类型参数或其包含类型的类型参数是开放类型时，构造的嵌套类型是开放类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-421">A constructed nested type is an open type if and only if one or more of its type arguments or the type arguments of its containing type(s) is an open type.</span></span>

<span data-ttu-id="350f9-422">关闭的类型是不是开放类型的类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-422">A closed type is a type that is not an open type.</span></span>

<span data-ttu-id="350f9-423">在运行时，泛型类型声明中的所有代码在通过将类型自变量应用于泛型声明而创建的封闭式构造类型的上下文中执行。</span><span class="sxs-lookup"><span data-stu-id="350f9-423">At run-time, all of the code within a generic type declaration is executed in the context of a closed constructed type that was created by applying type arguments to the generic declaration.</span></span> <span data-ttu-id="350f9-424">泛型类型中的每个类型形参都绑定到特定的运行时类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-424">Each type parameter within the generic type is bound to a particular run-time type.</span></span> <span data-ttu-id="350f9-425">所有语句和表达式的运行时处理始终出现在关闭的类型中，并且打开的类型仅在编译时处理期间出现。</span><span class="sxs-lookup"><span data-stu-id="350f9-425">The run-time processing of all statements and expressions always occurs with closed types, and open types occur only during compile-time processing.</span></span>

<span data-ttu-id="350f9-426">每个封闭式构造类型都有自己的静态变量集，它们不与任何其他封闭构造类型共享。</span><span class="sxs-lookup"><span data-stu-id="350f9-426">Each closed constructed type has its own set of static variables, which are not shared with any other closed constructed types.</span></span> <span data-ttu-id="350f9-427">由于打开的类型在运行时不存在，因此没有与开放式类型关联的静态变量。</span><span class="sxs-lookup"><span data-stu-id="350f9-427">Since an open type does not exist at run-time, there are no static variables associated with an open type.</span></span> <span data-ttu-id="350f9-428">如果两个封闭式构造类型是从同一个未绑定的泛型类型构造的，则这两个封闭式构造类型都是相同的类型，并且其对应的类型参数是相同的类型</span><span class="sxs-lookup"><span data-stu-id="350f9-428">Two closed constructed types are the same type if they are constructed from the same unbound generic type, and their corresponding type arguments are the same type.</span></span>

### <a name="bound-and-unbound-types"></a><span data-ttu-id="350f9-429">绑定类型和未绑定类型</span><span class="sxs-lookup"><span data-stu-id="350f9-429">Bound and unbound types</span></span>

<span data-ttu-id="350f9-430">术语 "***未绑定类型***" 引用非泛型类型或未绑定的泛型类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-430">The term ***unbound type*** refers to a non-generic type or an unbound generic type.</span></span> <span data-ttu-id="350f9-431">术语 "***绑定类型***" 是指非泛型类型或构造类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-431">The term ***bound type*** refers to a non-generic type or a constructed type.</span></span>

<span data-ttu-id="350f9-432">未绑定类型引用由类型声明声明的实体。</span><span class="sxs-lookup"><span data-stu-id="350f9-432">An unbound type refers to the entity declared by a type declaration.</span></span> <span data-ttu-id="350f9-433">未绑定的泛型类型本身不是类型，不能用作变量、参数或返回值的类型，也不能用作基类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-433">An unbound generic type is not itself a type, and cannot be used as the type of a variable, argument or return value, or as a base type.</span></span> <span data-ttu-id="350f9-434">无法引用未绑定的泛型类型的唯一构造是 `typeof` 表达式（[typeof 运算符](expressions.md#the-typeof-operator)）。</span><span class="sxs-lookup"><span data-stu-id="350f9-434">The only construct in which an unbound generic type can be referenced is the `typeof` expression ([The typeof operator](expressions.md#the-typeof-operator)).</span></span>

### <a name="satisfying-constraints"></a><span data-ttu-id="350f9-435">满足约束</span><span class="sxs-lookup"><span data-stu-id="350f9-435">Satisfying constraints</span></span>

<span data-ttu-id="350f9-436">只要引用构造类型或泛型方法，就会对照对泛型类型或方法（[类型参数约束](classes.md#type-parameter-constraints)）声明的类型参数约束来检查提供的类型参数。</span><span class="sxs-lookup"><span data-stu-id="350f9-436">Whenever a constructed type or generic method is referenced, the supplied type arguments are checked against the type parameter constraints declared on the generic type or method ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="350f9-437">对于每个 `where` 子句，对照每个约束检查对应于命名类型参数 `A` 类型参数，如下所示：</span><span class="sxs-lookup"><span data-stu-id="350f9-437">For each `where` clause, the type argument `A` that corresponds to the named type parameter is checked against each constraint as follows:</span></span>

*  <span data-ttu-id="350f9-438">如果约束是类类型、接口类型或类型参数，则让 `C` 用提供的类型参数替换约束中出现的任何类型参数的约束。</span><span class="sxs-lookup"><span data-stu-id="350f9-438">If the constraint is a class type, an interface type, or a type parameter, let `C` represent that constraint with the supplied type arguments substituted for any type parameters that appear in the constraint.</span></span> <span data-ttu-id="350f9-439">若要满足约束，必须为类型 `A` 可转换为以下类型之一 `C`：</span><span class="sxs-lookup"><span data-stu-id="350f9-439">To satisfy the constraint, it must be the case that type `A` is convertible to type `C` by one of the following:</span></span>
    * <span data-ttu-id="350f9-440">标识转换（[标识转换](conversions.md#identity-conversion)）</span><span class="sxs-lookup"><span data-stu-id="350f9-440">An identity conversion ([Identity conversion](conversions.md#identity-conversion))</span></span>
    * <span data-ttu-id="350f9-441">隐式引用转换（[隐式引用](conversions.md#implicit-reference-conversions)转换）</span><span class="sxs-lookup"><span data-stu-id="350f9-441">An implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions))</span></span>
    * <span data-ttu-id="350f9-442">如果类型 A 是不可为 null 的值类型，则装箱转换（[装箱](conversions.md#boxing-conversions)转换）。</span><span class="sxs-lookup"><span data-stu-id="350f9-442">A boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)), provided that type A is a non-nullable value type.</span></span>
    * <span data-ttu-id="350f9-443">从类型参数 `A` 到 `C`的隐式引用、装箱或类型参数转换。</span><span class="sxs-lookup"><span data-stu-id="350f9-443">An implicit reference, boxing or type parameter conversion from a type parameter `A` to `C`.</span></span>
*  <span data-ttu-id="350f9-444">如果约束是引用类型约束（`class`），则类型 `A` 必须满足以下条件之一：</span><span class="sxs-lookup"><span data-stu-id="350f9-444">If the constraint is the reference type constraint (`class`), the type `A` must satisfy one of the following:</span></span>
    * <span data-ttu-id="350f9-445">`A` 是接口类型、类类型、委托类型或数组类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-445">`A` is an interface type, class type, delegate type or array type.</span></span> <span data-ttu-id="350f9-446">请注意，`System.ValueType` 和 `System.Enum` 是满足此约束的引用类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-446">Note that `System.ValueType` and `System.Enum` are reference types that satisfy this constraint.</span></span>
    * <span data-ttu-id="350f9-447">`A` 是已知为引用类型的类型参数（[类型参数约束](classes.md#type-parameter-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="350f9-447">`A` is a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="350f9-448">如果约束是值类型约束（`struct`），则类型 `A` 必须满足以下条件之一：</span><span class="sxs-lookup"><span data-stu-id="350f9-448">If the constraint is the value type constraint (`struct`), the type `A` must satisfy one of the following:</span></span>
    * <span data-ttu-id="350f9-449">`A` 是结构类型或枚举类型，但不能是可以为 null 的类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-449">`A` is a struct type or enum type, but not a nullable type.</span></span> <span data-ttu-id="350f9-450">请注意，`System.ValueType` 和 `System.Enum` 是不满足此约束的引用类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-450">Note that `System.ValueType` and `System.Enum` are reference types that do not satisfy this constraint.</span></span>
    * <span data-ttu-id="350f9-451">`A` 是具有值类型约束（[类型参数约束](classes.md#type-parameter-constraints)）的类型参数。</span><span class="sxs-lookup"><span data-stu-id="350f9-451">`A` is a type parameter having the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="350f9-452">如果约束是 `new()`的构造函数约束，则类型 `A` 不得 `abstract`，并且必须具有公共的无参数构造函数。</span><span class="sxs-lookup"><span data-stu-id="350f9-452">If the constraint is the constructor constraint `new()`, the type `A` must not be `abstract` and must have a public parameterless constructor.</span></span> <span data-ttu-id="350f9-453">如果满足以下条件之一，则满足此要求：</span><span class="sxs-lookup"><span data-stu-id="350f9-453">This is satisfied if one of the following is true:</span></span>
    * <span data-ttu-id="350f9-454">`A` 是一种值类型，因为所有值类型都具有公共的默认构造函数（[默认构造](types.md#default-constructors)函数）。</span><span class="sxs-lookup"><span data-stu-id="350f9-454">`A` is a value type, since all value types have a public default constructor ([Default constructors](types.md#default-constructors)).</span></span>
    * <span data-ttu-id="350f9-455">`A` 是具有构造函数约束的类型参数（[类型参数约束](classes.md#type-parameter-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="350f9-455">`A` is a type parameter having the constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
    * <span data-ttu-id="350f9-456">`A` 是具有值类型约束（[类型参数约束](classes.md#type-parameter-constraints)）的类型参数。</span><span class="sxs-lookup"><span data-stu-id="350f9-456">`A` is a type parameter having the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
    * <span data-ttu-id="350f9-457">`A` 是一种不 `abstract` 的类，包含不带参数的显式声明的 `public` 构造函数。</span><span class="sxs-lookup"><span data-stu-id="350f9-457">`A` is a class that is not `abstract` and contains an explicitly declared `public` constructor with no parameters.</span></span>
    * <span data-ttu-id="350f9-458">`A` 不 `abstract` 并且具有默认的构造函数（[默认构造函数](classes.md#default-constructors)）。</span><span class="sxs-lookup"><span data-stu-id="350f9-458">`A` is not `abstract` and has a default constructor ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="350f9-459">如果给定的类型参数不满足一个或多个类型参数的约束，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="350f9-459">A compile-time error occurs if one or more of a type parameter's constraints are not satisfied by the given type arguments.</span></span>

<span data-ttu-id="350f9-460">由于类型参数不是继承的，因此永远不会继承约束。</span><span class="sxs-lookup"><span data-stu-id="350f9-460">Since type parameters are not inherited, constraints are never inherited either.</span></span> <span data-ttu-id="350f9-461">在下面的示例中，`D` 需要指定其类型参数上的约束 `T` 以便 `T` 满足由基类 `B<T>`强制的约束。</span><span class="sxs-lookup"><span data-stu-id="350f9-461">In the example below, `D` needs to specify the constraint on its type parameter `T` so that `T` satisfies the constraint imposed by the base class `B<T>`.</span></span> <span data-ttu-id="350f9-462">与此相反，类 `E` 无需指定约束，因为 `List<T>` 为任何 `T`实现 `IEnumerable`。</span><span class="sxs-lookup"><span data-stu-id="350f9-462">In contrast, class `E` need not specify a constraint, because `List<T>` implements `IEnumerable` for any `T`.</span></span>

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a><span data-ttu-id="350f9-463">类型参数</span><span class="sxs-lookup"><span data-stu-id="350f9-463">Type parameters</span></span>

<span data-ttu-id="350f9-464">类型参数是一个标识符，它指定在运行时参数绑定到的值类型或引用类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-464">A type parameter is an identifier designating a value type or reference type that the parameter is bound to at run-time.</span></span>

```antlr
type_parameter
    : identifier
    ;
```

<span data-ttu-id="350f9-465">由于类型参数可以使用许多不同的实际类型参数进行实例化，因此类型参数与其他类型相比，操作和限制略有不同。</span><span class="sxs-lookup"><span data-stu-id="350f9-465">Since a type parameter can be instantiated with many different actual type arguments, type parameters have slightly different operations and restrictions than other types.</span></span> <span data-ttu-id="350f9-466">这些方法包括：</span><span class="sxs-lookup"><span data-stu-id="350f9-466">These include:</span></span>

*  <span data-ttu-id="350f9-467">类型参数不能直接用于声明基类（[基类](classes.md#base-class)）或接口（[变量类型参数列表](interfaces.md#variant-type-parameter-lists)）。</span><span class="sxs-lookup"><span data-stu-id="350f9-467">A type parameter cannot be used directly to declare a base class ([Base class](classes.md#base-class)) or interface ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)).</span></span>
*  <span data-ttu-id="350f9-468">对类型参数进行成员查找的规则取决于应用于该类型参数的约束（如果有）。</span><span class="sxs-lookup"><span data-stu-id="350f9-468">The rules for member lookup on type parameters depend on the constraints, if any, applied to the type parameter.</span></span> <span data-ttu-id="350f9-469">[成员查找](expressions.md#member-lookup)中对它们进行了详细介绍。</span><span class="sxs-lookup"><span data-stu-id="350f9-469">They are detailed in [Member lookup](expressions.md#member-lookup).</span></span>
*  <span data-ttu-id="350f9-470">类型参数的可用转换取决于应用于该类型参数的约束（如果有）。</span><span class="sxs-lookup"><span data-stu-id="350f9-470">The available conversions for a type parameter depend on the constraints, if any, applied to the type parameter.</span></span> <span data-ttu-id="350f9-471">其中详细介绍了[涉及类型参数](conversions.md#implicit-conversions-involving-type-parameters)和[显式动态转换](conversions.md#explicit-dynamic-conversions)的隐式转换。</span><span class="sxs-lookup"><span data-stu-id="350f9-471">They are detailed in [Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters) and [Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions).</span></span>
*  <span data-ttu-id="350f9-472">文本 `null` 无法转换为类型参数给定的类型，除非已知该类型参数是引用类型（[涉及类型参数的隐式转换](conversions.md#implicit-conversions-involving-type-parameters)）。</span><span class="sxs-lookup"><span data-stu-id="350f9-472">The literal `null` cannot be converted to a type given by a type parameter, except if the type parameter is known to be a reference type ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters)).</span></span> <span data-ttu-id="350f9-473">但是，可以改为使用 `default` 表达式（[默认值表达式](expressions.md#default-value-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="350f9-473">However, a `default` expression ([Default value expressions](expressions.md#default-value-expressions)) can be used instead.</span></span> <span data-ttu-id="350f9-474">此外，如果类型参数具有值类型约束，则可以使用 `==` 和 `!=` （[引用类型相等运算符](expressions.md#reference-type-equality-operators)）与 `null` 比较类型参数指定的类型的值。</span><span class="sxs-lookup"><span data-stu-id="350f9-474">In addition, a value with a type given by a type parameter can be compared with `null` using `==` and `!=` ([Reference type equality operators](expressions.md#reference-type-equality-operators)) unless the type parameter has the value type constraint.</span></span>
*  <span data-ttu-id="350f9-475">如果类型参数受*constructor_constraint*或值类型约束（[类型参数约束](classes.md#type-parameter-constraints)）的约束，则只能将 `new` 表达式（[对象创建表达式](expressions.md#object-creation-expressions)）与类型参数一起使用。</span><span class="sxs-lookup"><span data-stu-id="350f9-475">A `new` expression ([Object creation expressions](expressions.md#object-creation-expressions)) can only be used with a type parameter if the type parameter is constrained by a *constructor_constraint* or the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="350f9-476">类型参数不能在属性中的任何位置使用。</span><span class="sxs-lookup"><span data-stu-id="350f9-476">A type parameter cannot be used anywhere within an attribute.</span></span>
*  <span data-ttu-id="350f9-477">在成员访问（[成员访问](expressions.md#member-access)）或类型名称（[命名空间和类型名称](basic-concepts.md#namespace-and-type-names)）中不能使用类型参数来标识静态成员或嵌套类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-477">A type parameter cannot be used in a member access ([Member access](expressions.md#member-access)) or type name ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) to identify a static member or a nested type.</span></span>
*  <span data-ttu-id="350f9-478">在不安全代码中，类型参数不能用作*unmanaged_type* （[指针类型](unsafe-code.md#pointer-types)）。</span><span class="sxs-lookup"><span data-stu-id="350f9-478">In unsafe code, a type parameter cannot be used as an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

<span data-ttu-id="350f9-479">类型参数只是一种编译时构造。</span><span class="sxs-lookup"><span data-stu-id="350f9-479">As a type, type parameters are purely a compile-time construct.</span></span> <span data-ttu-id="350f9-480">在运行时，每个类型参数都绑定到一个运行时类型，该类型是通过向泛型类型声明提供类型参数来指定的。</span><span class="sxs-lookup"><span data-stu-id="350f9-480">At run-time, each type parameter is bound to a run-time type that was specified by supplying a type argument to the generic type declaration.</span></span> <span data-ttu-id="350f9-481">因此，使用类型参数声明的变量的类型将在运行时是封闭式构造类型（[开放和闭合类型](types.md#open-and-closed-types)）。</span><span class="sxs-lookup"><span data-stu-id="350f9-481">Thus, the type of a variable declared with a type parameter will, at run-time, be a closed constructed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="350f9-482">所有涉及类型参数的语句和表达式的运行时执行都使用作为该参数的类型参数提供的实际类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-482">The run-time execution of all statements and expressions involving type parameters uses the actual type that was supplied as the type argument for that parameter.</span></span>

## <a name="expression-tree-types"></a><span data-ttu-id="350f9-483">表达式树类型</span><span class="sxs-lookup"><span data-stu-id="350f9-483">Expression tree types</span></span>

<span data-ttu-id="350f9-484">***表达式树***允许将 lambda 表达式表示为数据结构而不是可执行代码。</span><span class="sxs-lookup"><span data-stu-id="350f9-484">***Expression trees*** permit lambda expressions to be represented as data structures instead of executable code.</span></span> <span data-ttu-id="350f9-485">表达式树是格式为 `System.Linq.Expressions.Expression<D>`的***表达式树类型***的值，其中 `D` 为任意委托类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-485">Expression trees are values of ***expression tree types*** of the form `System.Linq.Expressions.Expression<D>`, where `D` is any delegate type.</span></span> <span data-ttu-id="350f9-486">对于本规范的其余部分，我们将使用速记 `Expression<D>`引用这些类型。</span><span class="sxs-lookup"><span data-stu-id="350f9-486">For the remainder of this specification we will refer to these types using the shorthand `Expression<D>`.</span></span>

<span data-ttu-id="350f9-487">如果从 lambda 表达式到委托类型 `D`存在转换，则 `Expression<D>`的表达式树类型也存在转换。</span><span class="sxs-lookup"><span data-stu-id="350f9-487">If a conversion exists from a lambda expression to a delegate type `D`, a conversion also exists to the expression tree type `Expression<D>`.</span></span> <span data-ttu-id="350f9-488">尽管将 lambda 表达式转换为委托类型会生成一个委托，该委托引用 lambda 表达式的可执行代码，但转换为表达式树类型会创建 lambda 表达式的表达式树表示形式。</span><span class="sxs-lookup"><span data-stu-id="350f9-488">Whereas the conversion of a lambda expression to a delegate type generates a delegate that references executable code for the lambda expression, conversion to an expression tree type creates an expression tree representation of the lambda expression.</span></span>

<span data-ttu-id="350f9-489">表达式树是 lambda 表达式的有效内存中数据表示形式，并使 lambda 表达式的结构透明和显式。</span><span class="sxs-lookup"><span data-stu-id="350f9-489">Expression trees are efficient in-memory data representations of lambda expressions and make the structure of the lambda expression transparent and explicit.</span></span>

<span data-ttu-id="350f9-490">与委托类型 `D`一样，`Expression<D>` 称为具有参数和返回类型，它们与 `D`的类型相同。</span><span class="sxs-lookup"><span data-stu-id="350f9-490">Just like a delegate type `D`, `Expression<D>` is said to have parameter and return types, which are the same as those of `D`.</span></span>

<span data-ttu-id="350f9-491">下面的示例将 lambda 表达式表示为可执行代码，并表示为表达式树。</span><span class="sxs-lookup"><span data-stu-id="350f9-491">The following example represents a lambda expression both as executable code and as an expression tree.</span></span> <span data-ttu-id="350f9-492">由于 `Func<int,int>`存在转换，因此 `Expression<Func<int,int>>`也存在转换：</span><span class="sxs-lookup"><span data-stu-id="350f9-492">Because a conversion exists to `Func<int,int>`, a conversion also exists to `Expression<Func<int,int>>`:</span></span>

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

<span data-ttu-id="350f9-493">按照这些分配，委托 `del` 引用返回 `x + 1`的方法，并且表达式树 `exp` 引用用于描述表达式 `x => x + 1`的数据结构。</span><span class="sxs-lookup"><span data-stu-id="350f9-493">Following these assignments, the delegate `del` references a method that returns `x + 1`, and the expression tree `exp` references a data structure that describes the expression `x => x + 1`.</span></span>

<span data-ttu-id="350f9-494">在将 lambda 表达式转换为表达式树类型时，泛型类型 `Expression<D>` 的确切定义以及用于构造表达式树的准确规则都超出了此规范的范围。</span><span class="sxs-lookup"><span data-stu-id="350f9-494">The exact definition of the generic type `Expression<D>` as well as the precise rules for constructing an expression tree when a lambda expression is converted to an expression tree type, are both outside the scope of this specification.</span></span>

<span data-ttu-id="350f9-495">要做出显式操作，需要注意以下两项：</span><span class="sxs-lookup"><span data-stu-id="350f9-495">Two things are important to make explicit:</span></span>

*  <span data-ttu-id="350f9-496">并非所有 lambda 表达式都可以转换为表达式树。</span><span class="sxs-lookup"><span data-stu-id="350f9-496">Not all lambda expressions can be converted to expression trees.</span></span> <span data-ttu-id="350f9-497">例如，具有语句体的 lambda 表达式和包含赋值表达式的 lambda 表达式不能表示。</span><span class="sxs-lookup"><span data-stu-id="350f9-497">For instance, lambda expressions with statement bodies, and lambda expressions containing assignment expressions cannot be represented.</span></span> <span data-ttu-id="350f9-498">在这些情况下，转换仍存在，但会在编译时失败。</span><span class="sxs-lookup"><span data-stu-id="350f9-498">In these cases, a conversion still exists, but will fail at compile-time.</span></span> <span data-ttu-id="350f9-499">[匿名函数转换](conversions.md#anonymous-function-conversions)中详细介绍了这些异常。</span><span class="sxs-lookup"><span data-stu-id="350f9-499">These exceptions are detailed in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>
*   <span data-ttu-id="350f9-500">`Expression<D>` 提供了一个实例方法 `Compile`，该方法生成类型 `D`的委托：</span><span class="sxs-lookup"><span data-stu-id="350f9-500">`Expression<D>` offers an instance method `Compile` which produces a delegate of type `D`:</span></span>

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    <span data-ttu-id="350f9-501">调用此委托将导致执行表达式树所表示的代码。</span><span class="sxs-lookup"><span data-stu-id="350f9-501">Invoking this delegate causes the code represented by the expression tree to be executed.</span></span> <span data-ttu-id="350f9-502">因此，根据上面的定义，del 和 del2 是等效的，以下两个语句将具有相同的效果：</span><span class="sxs-lookup"><span data-stu-id="350f9-502">Thus, given the definitions above, del and del2 are equivalent, and the following two statements will have the same effect:</span></span>

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    <span data-ttu-id="350f9-503">执行此代码后，`i1` 和 `i2` 都具有 `2`的值。</span><span class="sxs-lookup"><span data-stu-id="350f9-503">After executing this code,  `i1` and `i2` will both have the value `2`.</span></span>

