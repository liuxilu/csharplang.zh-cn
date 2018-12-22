# <a name="types"></a><span data-ttu-id="e46e0-101">类型</span><span class="sxs-lookup"><span data-stu-id="e46e0-101">Types</span></span>

<span data-ttu-id="e46e0-102">C# 语言的类型分为两个主要类别：***值类型***并***引用类型***。</span><span class="sxs-lookup"><span data-stu-id="e46e0-102">The types of the C# language are divided into two main categories: ***value types*** and ***reference types***.</span></span> <span data-ttu-id="e46e0-103">值类型和引用类型可能***泛型类型***，这需要一个或多个***类型参数***。</span><span class="sxs-lookup"><span data-stu-id="e46e0-103">Both value types and reference types may be ***generic types***, which take one or more ***type parameters***.</span></span> <span data-ttu-id="e46e0-104">类型参数可以指定这两种值类型和引用类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-104">Type parameters can designate both value types and reference types.</span></span>

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

<span data-ttu-id="e46e0-105">仅在不安全代码中提供的类型、 指针、 的最后一个类别。</span><span class="sxs-lookup"><span data-stu-id="e46e0-105">The final category of types, pointers, is available only in unsafe code.</span></span> <span data-ttu-id="e46e0-106">对此进行讨论中进一步[指针类型](unsafe-code.md#pointer-types)。</span><span class="sxs-lookup"><span data-stu-id="e46e0-106">This is discussed further in [Pointer types](unsafe-code.md#pointer-types).</span></span>

<span data-ttu-id="e46e0-107">值类型不同于引用类型，值类型的变量直接包含其数据，而变量引用的类型应用商店***引用***对其数据，后者称为***对象***.</span><span class="sxs-lookup"><span data-stu-id="e46e0-107">Value types differ from reference types in that variables of the value types directly contain their data, whereas variables of the reference types store ***references*** to their data, the latter being known as ***objects***.</span></span> <span data-ttu-id="e46e0-108">对于引用类型，它是两个变量来引用同一对象，并因此可能会对一个变量以影响其他变量所引用的对象的操作。</span><span class="sxs-lookup"><span data-stu-id="e46e0-108">With reference types, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="e46e0-109">对于值类型，每个变量都有自己的数据的副本并不是可能会影响另一个操作。</span><span class="sxs-lookup"><span data-stu-id="e46e0-109">With value types, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other.</span></span>

<span data-ttu-id="e46e0-110">C# 类型系统被统一的这样可以任何类型的值视为对象。</span><span class="sxs-lookup"><span data-stu-id="e46e0-110">C#'s type system is unified such that a value of any type can be treated as an object.</span></span> <span data-ttu-id="e46e0-111">每种 C# 类型都直接或间接地派生自 `object` 类类型，而 `object` 是所有类型的最终基类。</span><span class="sxs-lookup"><span data-stu-id="e46e0-111">Every type in C# directly or indirectly derives from the `object` class type, and `object` is the ultimate base class of all types.</span></span> <span data-ttu-id="e46e0-112">只需将值视为类型 `object`，即可将引用类型的值视为对象。</span><span class="sxs-lookup"><span data-stu-id="e46e0-112">Values of reference types are treated as objects simply by viewing the values as type `object`.</span></span> <span data-ttu-id="e46e0-113">值类型的值作为对象处理通过执行装箱和取消装箱操作 ([装箱和取消装箱](types.md#boxing-and-unboxing))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-113">Values of value types are treated as objects by performing boxing and unboxing operations ([Boxing and unboxing](types.md#boxing-and-unboxing)).</span></span>

## <a name="value-types"></a><span data-ttu-id="e46e0-114">值类型</span><span class="sxs-lookup"><span data-stu-id="e46e0-114">Value types</span></span>

<span data-ttu-id="e46e0-115">值类型是结构类型或枚举类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-115">A value type is either a struct type or an enumeration type.</span></span> <span data-ttu-id="e46e0-116">C# 提供了一组预定义的结构类型称为***简单类型***。</span><span class="sxs-lookup"><span data-stu-id="e46e0-116">C# provides a set of predefined struct types called the ***simple types***.</span></span> <span data-ttu-id="e46e0-117">通过保留字，简单类型进行标识。</span><span class="sxs-lookup"><span data-stu-id="e46e0-117">The simple types are identified through reserved words.</span></span>

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

<span data-ttu-id="e46e0-118">与引用类型的变量，不同的变量的值类型可以包含值`null`仅当值类型为 null 的类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-118">Unlike a variable of a reference type, a variable of a value type can contain the value `null` only if the value type is a nullable type.</span></span>  <span data-ttu-id="e46e0-119">对于每个不可为 null 的值类型没有相应的可以为 null 值类型表示一组相同的值以及值`null`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-119">For every non-nullable value type there is a corresponding nullable value type denoting the same set of values plus the value `null`.</span></span>

<span data-ttu-id="e46e0-120">值类型的变量进行赋值会创建一份所赋的值。</span><span class="sxs-lookup"><span data-stu-id="e46e0-120">Assignment to a variable of a value type creates a copy of the value being assigned.</span></span> <span data-ttu-id="e46e0-121">这不同于分配给变量的引用类型，复制引用而不是由引用标识的对象。</span><span class="sxs-lookup"><span data-stu-id="e46e0-121">This differs from assignment to a variable of a reference type, which copies the reference but not the object identified by the reference.</span></span>

### <a name="the-systemvaluetype-type"></a><span data-ttu-id="e46e0-122">System.ValueType 类型</span><span class="sxs-lookup"><span data-stu-id="e46e0-122">The System.ValueType type</span></span>

<span data-ttu-id="e46e0-123">所有值类型隐式都继承自类`System.ValueType`，而后者又继承自类`object`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-123">All value types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="e46e0-124">不能为任何类型派生自值类型，并因此隐式密封的值类型 ([密封类](classes.md#sealed-classes))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-124">It is not possible for any type to derive from a value type, and value types are thus implicitly sealed ([Sealed classes](classes.md#sealed-classes)).</span></span>

<span data-ttu-id="e46e0-125">请注意，`System.ValueType`本身不是*value_type*。</span><span class="sxs-lookup"><span data-stu-id="e46e0-125">Note that `System.ValueType` is not itself a *value_type*.</span></span> <span data-ttu-id="e46e0-126">相反，它是*class_type*所有*value_type*会自动派生 s。</span><span class="sxs-lookup"><span data-stu-id="e46e0-126">Rather, it is a *class_type* from which all *value_type*s are automatically derived.</span></span>

### <a name="default-constructors"></a><span data-ttu-id="e46e0-127">默认构造函数</span><span class="sxs-lookup"><span data-stu-id="e46e0-127">Default constructors</span></span>

<span data-ttu-id="e46e0-128">所有值类型隐式都声明公共无参数实例构造函数调用***默认构造函数***。</span><span class="sxs-lookup"><span data-stu-id="e46e0-128">All value types implicitly declare a public parameterless instance constructor called the ***default constructor***.</span></span> <span data-ttu-id="e46e0-129">默认构造函数将返回零初始化的实例称为***默认值***对于值类型：</span><span class="sxs-lookup"><span data-stu-id="e46e0-129">The default constructor returns a zero-initialized instance known as the ***default value*** for the value type:</span></span>

*  <span data-ttu-id="e46e0-130">为所有*simple_type*s，默认值是生成全为零的位模式的值：</span><span class="sxs-lookup"><span data-stu-id="e46e0-130">For all *simple_type*s, the default value is the value produced by a bit pattern of all zeros:</span></span>
    * <span data-ttu-id="e46e0-131">有关`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`，并且`ulong`，默认值是`0`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-131">For `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, and `ulong`, the default value is `0`.</span></span>
    * <span data-ttu-id="e46e0-132">有关`char`，默认值是`'\x0000'`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-132">For `char`, the default value is `'\x0000'`.</span></span>
    * <span data-ttu-id="e46e0-133">有关`float`，默认值是`0.0f`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-133">For `float`, the default value is `0.0f`.</span></span>
    * <span data-ttu-id="e46e0-134">有关`double`，默认值是`0.0d`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-134">For `double`, the default value is `0.0d`.</span></span>
    * <span data-ttu-id="e46e0-135">有关`decimal`，默认值是`0.0m`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-135">For `decimal`, the default value is `0.0m`.</span></span>
    * <span data-ttu-id="e46e0-136">有关`bool`，默认值是`false`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-136">For `bool`, the default value is `false`.</span></span>
*  <span data-ttu-id="e46e0-137">有关*enum_type* `E`，默认值为`0`转换为的类型、 `E`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-137">For an *enum_type* `E`, the default value is `0`, converted to the type `E`.</span></span>
*  <span data-ttu-id="e46e0-138">有关*struct_type*，默认值是生成通过将所有值类型字段都设置为其默认值和所有引用类型字段的值`null`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-138">For a *struct_type*, the default value is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>
*  <span data-ttu-id="e46e0-139">有关*nullable_type*的默认值是为其实例`HasValue`属性为 false 和`Value`未定义属性。</span><span class="sxs-lookup"><span data-stu-id="e46e0-139">For a *nullable_type* the default value is an instance for which the `HasValue` property is false and the `Value` property is undefined.</span></span> <span data-ttu-id="e46e0-140">默认值是也称为***null 值***的为 null 的类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-140">The default value is also known as the ***null value*** of the nullable type.</span></span>

<span data-ttu-id="e46e0-141">值类型的默认构造函数调用使用任何其他实例构造函数，如`new`运算符。</span><span class="sxs-lookup"><span data-stu-id="e46e0-141">Like any other instance constructor, the default constructor of a value type is invoked using the `new` operator.</span></span> <span data-ttu-id="e46e0-142">出于效率考虑，此要求不适用于实际具有生成的构造函数调用的实现。</span><span class="sxs-lookup"><span data-stu-id="e46e0-142">For efficiency reasons, this requirement is not intended to actually have the implementation generate a constructor call.</span></span> <span data-ttu-id="e46e0-143">在以下示例中，变量`i`和`j`均被初始化为零。</span><span class="sxs-lookup"><span data-stu-id="e46e0-143">In the example below, variables `i` and `j` are both initialized to zero.</span></span>

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

<span data-ttu-id="e46e0-144">每个值类型隐式具有公共无参数实例构造函数，因为它不能包含无参数构造函数的显式声明为结构类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-144">Because every value type implicitly has a public parameterless instance constructor, it is not possible for a struct type to contain an explicit declaration of a parameterless constructor.</span></span> <span data-ttu-id="e46e0-145">但是允许结构类型声明参数化的实例构造函数 ([构造函数](structs.md#constructors))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-145">A struct type is however permitted to declare parameterized instance constructors ([Constructors](structs.md#constructors)).</span></span>

### <a name="struct-types"></a><span data-ttu-id="e46e0-146">结构类型</span><span class="sxs-lookup"><span data-stu-id="e46e0-146">Struct types</span></span>

<span data-ttu-id="e46e0-147">结构类型是常量、 字段、 方法、 属性、 索引器、 运算符、 实例构造函数、 静态构造函数和嵌套的类型可以声明的值类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-147">A struct type is a value type that can declare constants, fields, methods, properties, indexers, operators, instance constructors, static constructors, and nested types.</span></span> <span data-ttu-id="e46e0-148">结构类型的声明中所述[结构声明](structs.md#struct-declarations)。</span><span class="sxs-lookup"><span data-stu-id="e46e0-148">The declaration of struct types is described in [Struct declarations](structs.md#struct-declarations).</span></span>

### <a name="simple-types"></a><span data-ttu-id="e46e0-149">简单类型</span><span class="sxs-lookup"><span data-stu-id="e46e0-149">Simple types</span></span>

<span data-ttu-id="e46e0-150">C# 提供了一组预定义的结构类型称为***简单类型***。</span><span class="sxs-lookup"><span data-stu-id="e46e0-150">C# provides a set of predefined struct types called the ***simple types***.</span></span> <span data-ttu-id="e46e0-151">简单类型进行标识通过保留字，但这些保留的字是只需在预定义的结构类型的别名`System`命名空间下, 表中所述。</span><span class="sxs-lookup"><span data-stu-id="e46e0-151">The simple types are identified through reserved words, but these reserved words are simply aliases for predefined struct types in the `System` namespace, as described in the table below.</span></span>


| <span data-ttu-id="e46e0-152">__保留的字__</span><span class="sxs-lookup"><span data-stu-id="e46e0-152">__Reserved word__</span></span> | <span data-ttu-id="e46e0-153">__使用别名类型__</span><span class="sxs-lookup"><span data-stu-id="e46e0-153">__Aliased type__</span></span> |
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

<span data-ttu-id="e46e0-154">简单类型的结构类型别名，因为每个简单类型都具有成员。</span><span class="sxs-lookup"><span data-stu-id="e46e0-154">Because a simple type aliases a struct type, every simple type has members.</span></span> <span data-ttu-id="e46e0-155">例如，`int`具有中声明成员`System.Int32`和成员继承自`System.Object`，并允许使用以下语句：</span><span class="sxs-lookup"><span data-stu-id="e46e0-155">For example, `int` has the members declared in `System.Int32` and the members inherited from `System.Object`, and the following statements are permitted:</span></span>

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

<span data-ttu-id="e46e0-156">简单类型不同于其他结构类型，简单类型允许某些附加操作：</span><span class="sxs-lookup"><span data-stu-id="e46e0-156">The simple types differ from other struct types in that they permit certain additional operations:</span></span>

*  <span data-ttu-id="e46e0-157">大多数简单类型允许值，若要创建的编写*文字*([文本](lexical-structure.md#literals))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-157">Most simple types permit values to be created by writing *literals* ([Literals](lexical-structure.md#literals)).</span></span> <span data-ttu-id="e46e0-158">例如，`123`是类型的文字`int`并`'a'`是类型的文本`char`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-158">For example, `123` is a literal of type `int` and `'a'` is a literal of type `char`.</span></span> <span data-ttu-id="e46e0-159">C# 一般情况下，可以为文本的结构类型没有预配和其他结构类型的非默认值最终始终通过创建这些结构类型的实例构造函数。</span><span class="sxs-lookup"><span data-stu-id="e46e0-159">C# makes no provision for literals of struct types in general, and non-default values of other struct types are ultimately always created through instance constructors of those struct types.</span></span>
*  <span data-ttu-id="e46e0-160">所有简单类型常量表达式的操作数时，就可以为编译器在编译时表达式进行求值。</span><span class="sxs-lookup"><span data-stu-id="e46e0-160">When the operands of an expression are all simple type constants, it is possible for the compiler to evaluate the expression at compile-time.</span></span> <span data-ttu-id="e46e0-161">此类表达式被称为*constant_expression* ([常量表达式](expressions.md#constant-expressions))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-161">Such an expression is known as a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)).</span></span> <span data-ttu-id="e46e0-162">包含由其他结构类型定义运算符的表达式都不是常量表达式。</span><span class="sxs-lookup"><span data-stu-id="e46e0-162">Expressions involving operators defined by other struct types are not considered to be constant expressions.</span></span>
*  <span data-ttu-id="e46e0-163">通过`const`声明就可以声明的简单类型的常量 ([常量](classes.md#constants))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-163">Through `const` declarations it is possible to declare constants of the simple types ([Constants](classes.md#constants)).</span></span> <span data-ttu-id="e46e0-164">不能具有常量的其他结构类型，但未提供类似的效果`static readonly`字段。</span><span class="sxs-lookup"><span data-stu-id="e46e0-164">It is not possible to have constants of other struct types, but a similar effect is provided by `static readonly` fields.</span></span>
*  <span data-ttu-id="e46e0-165">涉及简单类型的转换可以参与的其他结构类型，所定义的转换运算符的计算，但用户定义的转换运算符可以永远不会参与的另一个用户定义运算符的计算 ([的评估用户定义的转换](conversions.md#evaluation-of-user-defined-conversions))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-165">Conversions involving simple types can participate in evaluation of conversion operators defined by other struct types, but a user-defined conversion operator can never participate in evaluation of another user-defined operator ([Evaluation of user-defined conversions](conversions.md#evaluation-of-user-defined-conversions)).</span></span>

### <a name="integral-types"></a><span data-ttu-id="e46e0-166">整型</span><span class="sxs-lookup"><span data-stu-id="e46e0-166">Integral types</span></span>

<span data-ttu-id="e46e0-167">C# 支持九个整型类型： `sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`，和`char`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-167">C# supports nine integral types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, and `char`.</span></span> <span data-ttu-id="e46e0-168">整型类型具有以下大小和范围的值：</span><span class="sxs-lookup"><span data-stu-id="e46e0-168">The integral types have the following sizes and ranges of values:</span></span>

*  <span data-ttu-id="e46e0-169">`sbyte`类型表示有符号 8 位整数的值介于-128 到 127 之间。</span><span class="sxs-lookup"><span data-stu-id="e46e0-169">The `sbyte` type represents signed 8-bit integers with values between -128 and 127.</span></span>
*  <span data-ttu-id="e46e0-170">`byte`类型表示其值介于 0 和 255 之间的 8 位无符号的整数。</span><span class="sxs-lookup"><span data-stu-id="e46e0-170">The `byte` type represents unsigned 8-bit integers with values between 0 and 255.</span></span>
*  <span data-ttu-id="e46e0-171">`short`类型表示有符号 16 位整数值介于-32768 和 32767 之间。</span><span class="sxs-lookup"><span data-stu-id="e46e0-171">The `short` type represents signed 16-bit integers with values between -32768 and 32767.</span></span>
*  <span data-ttu-id="e46e0-172">`ushort`类型表示无符号的 16 位整数，其值介于 0 和 65535 之间。</span><span class="sxs-lookup"><span data-stu-id="e46e0-172">The `ushort` type represents unsigned 16-bit integers with values between 0 and 65535.</span></span>
*  <span data-ttu-id="e46e0-173">`int`类型表示有符号 32 位整数值介于-2147483648 到 2147483647 之间。</span><span class="sxs-lookup"><span data-stu-id="e46e0-173">The `int` type represents signed 32-bit integers with values between -2147483648 and 2147483647.</span></span>
*  <span data-ttu-id="e46e0-174">`uint`类型表示值介于 0 到 4294967295 之间的 32 位无符号的整数。</span><span class="sxs-lookup"><span data-stu-id="e46e0-174">The `uint` type represents unsigned 32-bit integers with values between 0 and 4294967295.</span></span>
*  <span data-ttu-id="e46e0-175">`long`类型表示有符号 64 位整数的值介于-9223372036854775808 到 9223372036854775807 之间。</span><span class="sxs-lookup"><span data-stu-id="e46e0-175">The `long` type represents signed 64-bit integers with values between -9223372036854775808 and 9223372036854775807.</span></span>
*  <span data-ttu-id="e46e0-176">`ulong`类型表示值介于 0 到 18446744073709551615 之间的 64 位无符号的整数。</span><span class="sxs-lookup"><span data-stu-id="e46e0-176">The `ulong` type represents unsigned 64-bit integers with values between 0 and 18446744073709551615.</span></span>
*  <span data-ttu-id="e46e0-177">`char`类型表示无符号的 16 位整数，其值介于 0 和 65535 之间。</span><span class="sxs-lookup"><span data-stu-id="e46e0-177">The `char` type represents unsigned 16-bit integers with values between 0 and 65535.</span></span> <span data-ttu-id="e46e0-178">可能值的集`char`类型对应于 Unicode 字符集。</span><span class="sxs-lookup"><span data-stu-id="e46e0-178">The set of possible values for the `char` type corresponds to the Unicode character set.</span></span> <span data-ttu-id="e46e0-179">尽管`char`具有相同的表示形式`ushort`上的其他允许一种类型上允许的不是所有操作。</span><span class="sxs-lookup"><span data-stu-id="e46e0-179">Although `char` has the same representation as `ushort`, not all operations permitted on one type are permitted on the other.</span></span>

<span data-ttu-id="e46e0-180">整型类型一元和二元运算符总是对有符号的 32 位精度、 无符号的 32 位精度，有符号的 64 位精度，或无符号的 64 位精度操作：</span><span class="sxs-lookup"><span data-stu-id="e46e0-180">The integral-type unary and binary operators always operate with signed 32-bit precision, unsigned 32-bit precision, signed 64-bit precision, or unsigned 64-bit precision:</span></span>

*  <span data-ttu-id="e46e0-181">对于一元`+`并`~`运算符，操作数将转换为类型`T`，其中`T`是第一种`int`， `uint`， `long`，和`ulong`，可以充分表示所有可能的操作数的值。</span><span class="sxs-lookup"><span data-stu-id="e46e0-181">For the unary `+` and `~` operators, the operand is converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="e46e0-182">然后使用类型的精度执行该操作`T`，并且结果的类型为`T`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-182">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span>
*  <span data-ttu-id="e46e0-183">对于一元`-`运算符，操作数将转换为类型`T`，其中`T`是第一种`int`和`long`，可以完全表示操作数的所有可能的值。</span><span class="sxs-lookup"><span data-stu-id="e46e0-183">For the unary `-` operator, the operand is converted to type `T`, where `T` is the first of `int` and `long` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="e46e0-184">然后使用类型的精度执行该操作`T`，并且结果的类型为`T`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-184">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span> <span data-ttu-id="e46e0-185">一元`-`运算符不能应用于类型的操作数`ulong`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-185">The unary `-` operator cannot be applied to operands of type `ulong`.</span></span>
*  <span data-ttu-id="e46e0-186">二进制文件`+`， `-`， `*`， `/`， `%`， `&`， `^`， `|`， `==`， `!=`， `>`， `<`， `>=`，并`<=`运算符，操作数转换为类型`T`，其中`T`是第一种`int`， `uint`， `long`，和`ulong`，可以完全表示所有可能这两个操作数的值。</span><span class="sxs-lookup"><span data-stu-id="e46e0-186">For the binary `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`, and `<=` operators, the operands are converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of both operands.</span></span> <span data-ttu-id="e46e0-187">然后使用类型的精度执行该操作`T`，并且结果的类型为`T`(或`bool`对于关系运算符)。</span><span class="sxs-lookup"><span data-stu-id="e46e0-187">The operation is then performed using the precision of type `T`, and the type of the result is `T` (or `bool` for the relational operators).</span></span> <span data-ttu-id="e46e0-188">它不允许的一个操作数的类型是`long`和其他类型`ulong`使用二元运算符。</span><span class="sxs-lookup"><span data-stu-id="e46e0-188">It is not permitted for one operand to be of type `long` and the other to be of type `ulong` with the binary operators.</span></span>
*  <span data-ttu-id="e46e0-189">二进制文件`<<`并`>>`运算符的左操作数将转换为类型`T`，其中`T`是第一种`int`， `uint`， `long`，和`ulong`，可以充分表示所有可能的操作数的值。</span><span class="sxs-lookup"><span data-stu-id="e46e0-189">For the binary `<<` and `>>` operators, the left operand is converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="e46e0-190">然后使用类型的精度执行该操作`T`，并且结果的类型为`T`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-190">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span>

<span data-ttu-id="e46e0-191">`char`类型分类为一种整型类型，但它不同于其他整数类型以两种方式：</span><span class="sxs-lookup"><span data-stu-id="e46e0-191">The `char` type is classified as an integral type, but it differs from the other integral types in two ways:</span></span>

*  <span data-ttu-id="e46e0-192">不存在隐式转换为其他类型`char`类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-192">There are no implicit conversions from other types to the `char` type.</span></span> <span data-ttu-id="e46e0-193">具体而言，即使`sbyte`， `byte`，并`ushort`类型具有的是完全可表示使用的值范围`char`类型，隐式转换从`sbyte`， `byte`，或`ushort`到`char`不存在。</span><span class="sxs-lookup"><span data-stu-id="e46e0-193">In particular, even though the `sbyte`, `byte`, and `ushort` types have ranges of values that are fully representable using the `char` type, implicit conversions from `sbyte`, `byte`, or `ushort` to `char` do not exist.</span></span>
*  <span data-ttu-id="e46e0-194">常数`char`类型必须写为*character_literal*s 或被*integer_literal*结合强制转换为类型 s `char`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-194">Constants of the `char` type must be written as *character_literal*s or as *integer_literal*s in combination with a cast to type `char`.</span></span> <span data-ttu-id="e46e0-195">例如，`(char)10` 和 `'\x000A'` 相同。</span><span class="sxs-lookup"><span data-stu-id="e46e0-195">For example, `(char)10` is the same as `'\x000A'`.</span></span>

<span data-ttu-id="e46e0-196">`checked`并`unchecked`运算符和语句用于控制溢出检查的整型类型算术运算和转换 ([checked 和 unchecked 运算符](expressions.md#the-checked-and-unchecked-operators))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-196">The `checked` and `unchecked` operators and statements are used to control overflow checking for integral-type arithmetic operations and conversions ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span> <span data-ttu-id="e46e0-197">在中`checked`上下文，溢出会生成编译时错误，或导致`System.OverflowException`引发。</span><span class="sxs-lookup"><span data-stu-id="e46e0-197">In a `checked` context, an overflow produces a compile-time error or causes a `System.OverflowException` to be thrown.</span></span> <span data-ttu-id="e46e0-198">在`unchecked`上下文中，将忽略溢出并不适合目标类型中的任何高序位将被丢弃。</span><span class="sxs-lookup"><span data-stu-id="e46e0-198">In an `unchecked` context, overflows are ignored and any high-order bits that do not fit in the destination type are discarded.</span></span>

### <a name="floating-point-types"></a><span data-ttu-id="e46e0-199">浮点类型</span><span class="sxs-lookup"><span data-stu-id="e46e0-199">Floating point types</span></span>

<span data-ttu-id="e46e0-200">C# 支持两个浮点类型：`float`和`double`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-200">C# supports two floating point types: `float` and `double`.</span></span> <span data-ttu-id="e46e0-201">`float`和`double`类型都使用为 32 位单精度和 64 位双精度 IEEE 754 格式提供下列值集来表示：</span><span class="sxs-lookup"><span data-stu-id="e46e0-201">The `float` and `double` types are represented using the 32-bit single-precision and 64-bit double-precision IEEE 754 formats, which provide the following sets of values:</span></span>

*  <span data-ttu-id="e46e0-202">正零和负零。</span><span class="sxs-lookup"><span data-stu-id="e46e0-202">Positive zero and negative zero.</span></span> <span data-ttu-id="e46e0-203">在大多数情况下，正零和负零具有相同行为简单的值为零，但某些操作区分两个 ([除法运算符](expressions.md#division-operator))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-203">In most situations, positive zero and negative zero behave identically as the simple value zero, but certain operations distinguish between the two ([Division operator](expressions.md#division-operator)).</span></span>
*  <span data-ttu-id="e46e0-204">正无穷大和负无穷大。</span><span class="sxs-lookup"><span data-stu-id="e46e0-204">Positive infinity and negative infinity.</span></span> <span data-ttu-id="e46e0-205">无穷大会生成被零除一个非零数字作为此类操作。</span><span class="sxs-lookup"><span data-stu-id="e46e0-205">Infinities are produced by such operations as dividing a non-zero number by zero.</span></span> <span data-ttu-id="e46e0-206">例如，`1.0 / 0.0`产生正无穷和`-1.0 / 0.0`产生负无穷大。</span><span class="sxs-lookup"><span data-stu-id="e46e0-206">For example, `1.0 / 0.0` yields positive infinity, and `-1.0 / 0.0` yields negative infinity.</span></span>
*  <span data-ttu-id="e46e0-207">***Not 数字***值，通常缩写的 NaN。</span><span class="sxs-lookup"><span data-stu-id="e46e0-207">The ***Not-a-Number*** value, often abbreviated NaN.</span></span> <span data-ttu-id="e46e0-208">无效的浮点运算，如零被零除会生成 Nan。</span><span class="sxs-lookup"><span data-stu-id="e46e0-208">NaNs are produced by invalid floating-point operations, such as dividing zero by zero.</span></span>
*  <span data-ttu-id="e46e0-209">一组有限的窗体的值为非零`s * m * 2^e`，其中`s`为 1 或-1，并`m`和`e`取决于特定的浮点类型：有关`float`，`0 < m < 2^24`并`-149 <= e <= 104`，并为`double`，`0 < m < 2^53`和`1075 <= e <= 970`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-209">The finite set of non-zero values of the form `s * m * 2^e`, where `s` is 1 or -1, and `m` and `e` are determined by the particular floating-point type: For `float`, `0 < m < 2^24` and `-149 <= e <= 104`, and for `double`, `0 < m < 2^53` and `1075 <= e <= 970`.</span></span> <span data-ttu-id="e46e0-210">非标准化浮点数被视为有效的非零值。</span><span class="sxs-lookup"><span data-stu-id="e46e0-210">Denormalized floating-point numbers are considered valid non-zero values.</span></span>

<span data-ttu-id="e46e0-211">`float`类型可以表示其值范围从大约`1.5 * 10^-45`到`3.4 * 10^38`且精度为 7 位数字。</span><span class="sxs-lookup"><span data-stu-id="e46e0-211">The `float` type can represent values ranging from approximately `1.5 * 10^-45` to `3.4 * 10^38` with a precision of 7 digits.</span></span>

<span data-ttu-id="e46e0-212">`double`类型可以表示其值范围从大约`5.0 * 10^-324`到`1.7 × 10^308`且精度为 15 到 16 位。</span><span class="sxs-lookup"><span data-stu-id="e46e0-212">The `double` type can represent values ranging from approximately `5.0 * 10^-324` to `1.7 × 10^308` with a precision of 15-16 digits.</span></span>

<span data-ttu-id="e46e0-213">如果浮点类型的二元运算符的操作数之一，然后另一个操作数必须是整型或浮点型，并且运算，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e46e0-213">If one of the operands of a binary operator is of a floating-point type, then the other operand must be of an integral type or a floating-point type, and the operation is evaluated as follows:</span></span>

*  <span data-ttu-id="e46e0-214">如果其中一个操作数的整数类型，则该操作数转换为另一个操作数的浮点类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-214">If one of the operands is of an integral type, then that operand is converted to the floating-point type of the other operand.</span></span>
*  <span data-ttu-id="e46e0-215">然后，如果任一操作数的类型`double`，另一个操作数转换为`double`，使用至少执行该操作`double`范围和精度和结果的类型是`double`(或`bool`为关系运算符）。</span><span class="sxs-lookup"><span data-stu-id="e46e0-215">Then, if either of the operands is of type `double`, the other operand is converted to `double`, the operation is performed using at least `double` range and precision, and the type of the result is `double` (or `bool` for the relational operators).</span></span>
*  <span data-ttu-id="e46e0-216">否则，该操作将执行使用至少`float`范围和精度和结果的类型是`float`(或`bool`对于关系运算符)。</span><span class="sxs-lookup"><span data-stu-id="e46e0-216">Otherwise, the operation is performed using at least `float` range and precision, and the type of the result is `float` (or `bool` for the relational operators).</span></span>

<span data-ttu-id="e46e0-217">浮点运算符，包括赋值运算符，永远不会生成异常。</span><span class="sxs-lookup"><span data-stu-id="e46e0-217">The floating-point operators, including the assignment operators, never produce exceptions.</span></span> <span data-ttu-id="e46e0-218">相反，在异常情况下，浮点运算生成零个、 无穷大或 NaN，如下所述：</span><span class="sxs-lookup"><span data-stu-id="e46e0-218">Instead, in exceptional situations, floating-point operations produce zero, infinity, or NaN, as described below:</span></span>

*  <span data-ttu-id="e46e0-219">如果目标格式太小浮点运算的结果，操作的结果将成为正零或负零。</span><span class="sxs-lookup"><span data-stu-id="e46e0-219">If the result of a floating-point operation is too small for the destination format, the result of the operation becomes positive zero or negative zero.</span></span>
*  <span data-ttu-id="e46e0-220">如果浮点运算的结果太大，目标格式，该操作的结果将成为正无穷或负无穷大。</span><span class="sxs-lookup"><span data-stu-id="e46e0-220">If the result of a floating-point operation is too large for the destination format, the result of the operation becomes positive infinity or negative infinity.</span></span>
*  <span data-ttu-id="e46e0-221">如果浮点运算无效，操作的结果将成为 NaN。</span><span class="sxs-lookup"><span data-stu-id="e46e0-221">If a floating-point operation is invalid, the result of the operation becomes NaN.</span></span>
*  <span data-ttu-id="e46e0-222">如果浮点运算的一个或两个操作数为 NaN，操作的结果将成为 NaN。</span><span class="sxs-lookup"><span data-stu-id="e46e0-222">If one or both operands of a floating-point operation is NaN, the result of the operation becomes NaN.</span></span>

<span data-ttu-id="e46e0-223">可能会执行该操作的结果类型比更高精度的浮点运算。</span><span class="sxs-lookup"><span data-stu-id="e46e0-223">Floating-point operations may be performed with higher precision than the result type of the operation.</span></span> <span data-ttu-id="e46e0-224">例如，某些硬件体系结构支持"扩展"或"长双精度"浮点类型，具有更大的范围和精度比`double`类型，并且隐式地执行所有使用此较高的精度类型的浮点运算。</span><span class="sxs-lookup"><span data-stu-id="e46e0-224">For example, some hardware architectures support an "extended" or "long double" floating-point type with greater range and precision than the `double` type, and implicitly perform all floating-point operations using this higher precision type.</span></span> <span data-ttu-id="e46e0-225">仅在性能开销过多此类硬件体系结构可用于执行在精度较低，浮点运算，而不是需要实现只好性能和精度，C# 允许更高的精度类型为用于所有浮点运算。</span><span class="sxs-lookup"><span data-stu-id="e46e0-225">Only at excessive cost in performance can such hardware architectures be made to perform floating-point operations with less precision, and rather than require an implementation to forfeit both performance and precision, C# allows a higher precision type to be used for all floating-point operations.</span></span> <span data-ttu-id="e46e0-226">不提供更精确的结果，这很少会有任何显著的影响。</span><span class="sxs-lookup"><span data-stu-id="e46e0-226">Other than delivering more precise results, this rarely has any measurable effects.</span></span> <span data-ttu-id="e46e0-227">但是，在窗体的表达式`x * y / z`，其中该乘法运算生成结果超出`double`范围，但后面的除法使临时结果返回到`double`范围，该表达式是这一事实计算在更高版本范围格式可能会导致有限生成而不是无限的结果。</span><span class="sxs-lookup"><span data-stu-id="e46e0-227">However, in expressions of the form `x * y / z`, where the multiplication produces a result that is outside the `double` range, but the subsequent division brings the temporary result back into the `double` range, the fact that the expression is evaluated in a higher range format may cause a finite result to be produced instead of an infinity.</span></span>

### <a name="the-decimal-type"></a><span data-ttu-id="e46e0-228">十进制类型</span><span class="sxs-lookup"><span data-stu-id="e46e0-228">The decimal type</span></span>

<span data-ttu-id="e46e0-229">`decimal` 类型是适用于财务和货币计算的 128 位数据类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-229">The `decimal` type is a 128-bit data type suitable for financial and monetary calculations.</span></span> <span data-ttu-id="e46e0-230">`decimal`类型可以表示值介于`1.0 * 10^-28`到大约`7.9 * 10^28`具有 28-29 个有效数字。</span><span class="sxs-lookup"><span data-stu-id="e46e0-230">The `decimal` type can represent values ranging from `1.0 * 10^-28` to approximately `7.9 * 10^28` with 28-29 significant digits.</span></span>

<span data-ttu-id="e46e0-231">一组有限的类型的值`decimal`的形式`(-1)^s * c * 10^-e`，其中符号`s`为 0 或 1，决定系数`c`由给定`0 <= *c* < 2^96`，和规模`e`是以便`0 <= e <= 28`。`decimal`类型不支持签名的零、 无穷大或 NaN 的。</span><span class="sxs-lookup"><span data-stu-id="e46e0-231">The finite set of values of type `decimal` are of the form `(-1)^s * c * 10^-e`, where the sign `s` is 0 or 1, the coefficient `c` is given by `0 <= *c* < 2^96`, and the scale `e` is such that `0 <= e <= 28`.The `decimal` type does not support signed zeros, infinities, or NaN's.</span></span> <span data-ttu-id="e46e0-232">一个`decimal`表示为 10 的幂缩放有 96 位整数。</span><span class="sxs-lookup"><span data-stu-id="e46e0-232">A `decimal` is represented as a 96-bit integer scaled by a power of ten.</span></span> <span data-ttu-id="e46e0-233">有关`decimal`s，使用绝对值小于`1.0m`，值为精确到 28 位小数，但不要再。</span><span class="sxs-lookup"><span data-stu-id="e46e0-233">For `decimal`s with an absolute value less than `1.0m`, the value is exact to the 28th decimal place, but no further.</span></span> <span data-ttu-id="e46e0-234">有关`decimal`使用绝对值大于或等于 s `1.0m`，值为精确到 28 或 29 位数。</span><span class="sxs-lookup"><span data-stu-id="e46e0-234">For `decimal`s with an absolute value greater than or equal to `1.0m`, the value is exact to 28 or 29 digits.</span></span> <span data-ttu-id="e46e0-235">到 contrary`float`并`double`数据类型，如 0.1 十进制小数数字可表示严格按照`decimal`表示形式。</span><span class="sxs-lookup"><span data-stu-id="e46e0-235">Contrary to the `float` and `double` data types, decimal fractional numbers such as 0.1 can be represented exactly in the `decimal` representation.</span></span> <span data-ttu-id="e46e0-236">在中`float`和`double`表示形式，此类数字都是通常无限小，这使得更容易以轮关闭这些表示形式的错误。</span><span class="sxs-lookup"><span data-stu-id="e46e0-236">In the `float` and `double` representations, such numbers are often infinite fractions, making those representations more prone to round-off errors.</span></span>

<span data-ttu-id="e46e0-237">如果其中一个二元运算符的操作数是类型`decimal`，则另一个操作数必须是一种整型类型或类型`decimal`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-237">If one of the operands of a binary operator is of type `decimal`, then the other operand must be of an integral type or of type `decimal`.</span></span> <span data-ttu-id="e46e0-238">如果存在整型操作数，则它将转换为`decimal`执行此操作之前。</span><span class="sxs-lookup"><span data-stu-id="e46e0-238">If an integral type operand is present, it is converted to `decimal` before the operation is performed.</span></span>

<span data-ttu-id="e46e0-239">类型的值的运算的结果`decimal`是这会导致从计算确切的结果 （保留缩放、 按每个运算符的定义），然后舍入以适合表示形式。</span><span class="sxs-lookup"><span data-stu-id="e46e0-239">The result of an operation on values of type `decimal` is that which would result from calculating an exact result (preserving scale, as defined for each operator) and then rounding to fit the representation.</span></span> <span data-ttu-id="e46e0-240">结果将舍入为最近的可表示的值，以及当结果同样接近两个可表示值，以最低有效位数位置 （这名为"银行家的舍入"） 中具有偶数数目的值。</span><span class="sxs-lookup"><span data-stu-id="e46e0-240">Results are rounded to the nearest representable value, and, when a result is equally close to two representable values, to the value that has an even number in the least significant digit position (this is known as "banker's rounding").</span></span> <span data-ttu-id="e46e0-241">零个结果始终具有 0 的符号和小数位数为 0。</span><span class="sxs-lookup"><span data-stu-id="e46e0-241">A zero result always has a sign of 0 and a scale of 0.</span></span>

<span data-ttu-id="e46e0-242">如果十进制算术运算产生的值小于或等于`5 * 10^-29`中绝对值的数值，该操作的结果则为零。</span><span class="sxs-lookup"><span data-stu-id="e46e0-242">If a decimal arithmetic operation produces a value less than or equal to `5 * 10^-29` in absolute value, the result of the operation becomes zero.</span></span> <span data-ttu-id="e46e0-243">如果`decimal`算术运算产生的结果太大`decimal`格式，`System.OverflowException`引发。</span><span class="sxs-lookup"><span data-stu-id="e46e0-243">If a `decimal` arithmetic operation produces a result that is too large for the `decimal` format, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="e46e0-244">`decimal`类型具有更高的精度比浮点类型的较小范围。</span><span class="sxs-lookup"><span data-stu-id="e46e0-244">The `decimal` type has greater precision but smaller range than the floating-point types.</span></span> <span data-ttu-id="e46e0-245">因此，对浮点类型转换`decimal`可能导致溢出异常，以及从转换`decimal`到浮点类型可能会导致丢失精度。</span><span class="sxs-lookup"><span data-stu-id="e46e0-245">Thus, conversions from the floating-point types to `decimal` might produce overflow exceptions, and conversions from `decimal` to the floating-point types might cause loss of precision.</span></span> <span data-ttu-id="e46e0-246">出于这些原因，隐式之间不存在转换浮点类型和`decimal`，而无需显式强制转换，则可以混合使用浮点和`decimal`在同一表达式中的操作数。</span><span class="sxs-lookup"><span data-stu-id="e46e0-246">For these reasons, no implicit conversions exist between the floating-point types and `decimal`, and without explicit casts, it is not possible to mix floating-point and `decimal` operands in the same expression.</span></span>

### <a name="the-bool-type"></a><span data-ttu-id="e46e0-247">Bool 类型</span><span class="sxs-lookup"><span data-stu-id="e46e0-247">The bool type</span></span>

<span data-ttu-id="e46e0-248">`bool`类型表示布尔逻辑的数量。</span><span class="sxs-lookup"><span data-stu-id="e46e0-248">The `bool` type represents boolean logical quantities.</span></span> <span data-ttu-id="e46e0-249">可能的值的类型`bool`都`true`和`false`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-249">The possible values of type `bool` are `true` and `false`.</span></span>

<span data-ttu-id="e46e0-250">标准之间不存在转换`bool`和其他类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-250">No standard conversions exist between `bool` and other types.</span></span> <span data-ttu-id="e46e0-251">具体而言，`bool`类型是不同，独立于整型类型和一个`bool`值不能代替整数值，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="e46e0-251">In particular, the `bool` type is distinct and separate from the integral types, and a `bool` value cannot be used in place of an integral value, and vice versa.</span></span>

<span data-ttu-id="e46e0-252">在 C 和 c + + 语言中，零整型或浮点值或 null 指针可以转换为布尔值`false`，和一个非零的整数或浮点值或非 null 指针可以转换为布尔值`true`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-252">In the C and C++ languages, a zero integral or floating-point value, or a null pointer can be converted to the boolean value `false`, and a non-zero integral or floating-point value, or a non-null pointer can be converted to the boolean value `true`.</span></span> <span data-ttu-id="e46e0-253">在 C# 中，都完成此类转换，通过显式地将整数或浮点数值为零，或通过显式比较的对象引用`null`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-253">In C#, such conversions are accomplished by explicitly comparing an integral or floating-point value to zero, or by explicitly comparing an object reference to `null`.</span></span>

### <a name="enumeration-types"></a><span data-ttu-id="e46e0-254">枚举类型</span><span class="sxs-lookup"><span data-stu-id="e46e0-254">Enumeration types</span></span>

<span data-ttu-id="e46e0-255">枚举类型是具有已命名常数的不同类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-255">An enumeration type is a distinct type with named constants.</span></span> <span data-ttu-id="e46e0-256">每个枚举类型有一个基础类型，它必须是`byte`， `sbyte`， `short`， `ushort`， `int`， `uint`，`long`或`ulong`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-256">Every enumeration type has an underlying type, which must be `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="e46e0-257">枚举类型的值集是相同的基础类型的值集。</span><span class="sxs-lookup"><span data-stu-id="e46e0-257">The set of values of the enumeration type is the same as the set of values of the underlying type.</span></span> <span data-ttu-id="e46e0-258">枚举类型的值不限于已命名常数的值。</span><span class="sxs-lookup"><span data-stu-id="e46e0-258">Values of the enumeration type are not restricted to the values of the named constants.</span></span> <span data-ttu-id="e46e0-259">枚举类型定义通过枚举声明 ([枚举声明](enums.md#enum-declarations))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-259">Enumeration types are defined through enumeration declarations ([Enum declarations](enums.md#enum-declarations)).</span></span>

### <a name="nullable-types"></a><span data-ttu-id="e46e0-260">可以为 null 的类型</span><span class="sxs-lookup"><span data-stu-id="e46e0-260">Nullable types</span></span>

<span data-ttu-id="e46e0-261">可以为 null 的类型可以表示的所有值及其***基础类型***加上其他的 null 值。</span><span class="sxs-lookup"><span data-stu-id="e46e0-261">A nullable type can represent all values of its ***underlying type*** plus an additional null value.</span></span> <span data-ttu-id="e46e0-262">可以为 null 的类型被写`T?`，其中`T`是基础类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-262">A nullable type is written `T?`, where `T` is the underlying type.</span></span> <span data-ttu-id="e46e0-263">此语法是简写`System.Nullable<T>`，并且可以互换使用两种形式。</span><span class="sxs-lookup"><span data-stu-id="e46e0-263">This syntax is shorthand for `System.Nullable<T>`, and the two forms can be used interchangeably.</span></span>

<span data-ttu-id="e46e0-264">一个***不可以为 null 的值类型***相反而不是任意值类型`System.Nullable<T>`和其简写`T?`(对于任何`T`)，以及任何类型参数约束为不可为 null 的值类型 （即，任何类型参数与`struct`约束)。</span><span class="sxs-lookup"><span data-stu-id="e46e0-264">A ***non-nullable value type*** conversely is any value type other than `System.Nullable<T>` and its shorthand `T?` (for any `T`), plus any type parameter that is constrained to be a non-nullable value type (that is, any type parameter with a `struct` constraint).</span></span> <span data-ttu-id="e46e0-265">`System.Nullable<T>`类型指定的值类型约束`T`([类型参数约束](classes.md#type-parameter-constraints))，这意味着可以为 null 的类型的基础类型可以是任何非 null 的值类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-265">The `System.Nullable<T>` type specifies the value type constraint for `T` ([Type parameter constraints](classes.md#type-parameter-constraints)), which means that the underlying type of a nullable type can be any non-nullable value type.</span></span> <span data-ttu-id="e46e0-266">可以为 null 的类型或引用类型，不能为 null 的类型的基础类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-266">The underlying type of a nullable type cannot be a nullable type or a reference type.</span></span> <span data-ttu-id="e46e0-267">例如，`int??`和`string?`是无效的类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-267">For example, `int??` and `string?` are invalid types.</span></span>

<span data-ttu-id="e46e0-268">可以为 null 的类型的实例`T?`具有两个公共只读属性：</span><span class="sxs-lookup"><span data-stu-id="e46e0-268">An instance of a nullable type `T?` has two public read-only properties:</span></span>

*  <span data-ttu-id="e46e0-269">一个`HasValue`类型的属性 `bool`</span><span class="sxs-lookup"><span data-stu-id="e46e0-269">A `HasValue` property of type `bool`</span></span>
*  <span data-ttu-id="e46e0-270">一个`Value`类型的属性 `T`</span><span class="sxs-lookup"><span data-stu-id="e46e0-270">A `Value` property of type `T`</span></span>

<span data-ttu-id="e46e0-271">为其实例`HasValue`是 true 则称其为非 null。</span><span class="sxs-lookup"><span data-stu-id="e46e0-271">An instance for which `HasValue` is true is said to be non-null.</span></span> <span data-ttu-id="e46e0-272">非 null 实例包含已知的值和`Value`返回该值。</span><span class="sxs-lookup"><span data-stu-id="e46e0-272">A non-null instance contains a known value and `Value` returns that value.</span></span>

<span data-ttu-id="e46e0-273">为其实例`HasValue`是 false 则称其为 null。</span><span class="sxs-lookup"><span data-stu-id="e46e0-273">An instance for which `HasValue` is false is said to be null.</span></span> <span data-ttu-id="e46e0-274">空实例有未定义的值。</span><span class="sxs-lookup"><span data-stu-id="e46e0-274">A null instance has an undefined value.</span></span> <span data-ttu-id="e46e0-275">尝试读取`Value`的空实例会导致`System.InvalidOperationException`引发。</span><span class="sxs-lookup"><span data-stu-id="e46e0-275">Attempting to read the `Value` of a null instance causes a `System.InvalidOperationException` to be thrown.</span></span> <span data-ttu-id="e46e0-276">访问的过程`Value`可以为 null 实例的属性被称为***解包***。</span><span class="sxs-lookup"><span data-stu-id="e46e0-276">The process of accessing the `Value` property of a nullable instance is referred to as ***unwrapping***.</span></span>

<span data-ttu-id="e46e0-277">除了默认构造函数中，每个可以为 null 的类型`T?`具有一个自变量的类型的公共构造函数`T`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-277">In addition to the default constructor, every nullable type `T?` has a public constructor that takes a single argument of type `T`.</span></span> <span data-ttu-id="e46e0-278">赋值`x`类型的`T`，窗体的构造函数调用</span><span class="sxs-lookup"><span data-stu-id="e46e0-278">Given a value `x` of type `T`, a constructor invocation of the form</span></span>

```csharp
new T?(x)
```
<span data-ttu-id="e46e0-279">创建的非 null 实例`T?`为其`Value`属性是`x`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-279">creates a non-null instance of `T?` for which the `Value` property is `x`.</span></span> <span data-ttu-id="e46e0-280">对于给定的值被称为创建可以为 null 的类型的非 null 实例的过程***包装***。</span><span class="sxs-lookup"><span data-stu-id="e46e0-280">The process of creating a non-null instance of a nullable type for a given value is referred to as ***wrapping***.</span></span>

<span data-ttu-id="e46e0-281">隐式转换都可通过从`null`文字`T?`([Null 文字转换](conversions.md#null-literal-conversions)) 和从`T`到`T?`([可以为 null 的隐式转换](conversions.md#implicit-nullable-conversions)).</span><span class="sxs-lookup"><span data-stu-id="e46e0-281">Implicit conversions are available from the `null` literal to `T?` ([Null literal conversions](conversions.md#null-literal-conversions)) and from `T` to `T?` ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)).</span></span>

## <a name="reference-types"></a><span data-ttu-id="e46e0-282">引用类型</span><span class="sxs-lookup"><span data-stu-id="e46e0-282">Reference types</span></span>

<span data-ttu-id="e46e0-283">引用类型是类类型、 接口类型、 数组类型或委托类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-283">A reference type is a class type, an interface type, an array type, or a delegate type.</span></span>

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

<span data-ttu-id="e46e0-284">引用类型值是指***实例***的类型，后者称为***对象***。</span><span class="sxs-lookup"><span data-stu-id="e46e0-284">A reference type value is a reference to an ***instance*** of the type, the latter known as an ***object***.</span></span> <span data-ttu-id="e46e0-285">特殊值`null`与所有引用类型兼容，并表示实例不存在。</span><span class="sxs-lookup"><span data-stu-id="e46e0-285">The special value `null` is compatible with all reference types and indicates the absence of an instance.</span></span>

### <a name="class-types"></a><span data-ttu-id="e46e0-286">类类型</span><span class="sxs-lookup"><span data-stu-id="e46e0-286">Class types</span></span>

<span data-ttu-id="e46e0-287">类类型定义包含数据成员 （常量和字段），函数成员 （方法、 属性、 事件、 索引器、 运算符、 实例构造函数、 析构函数和静态构造函数） 和嵌套的类型的数据结构。</span><span class="sxs-lookup"><span data-stu-id="e46e0-287">A class type defines a data structure that contains data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="e46e0-288">类类型支持继承，因此派生的类可以扩展和专门针对基类的机制。</span><span class="sxs-lookup"><span data-stu-id="e46e0-288">Class types support inheritance, a mechanism whereby derived classes can extend and specialize base classes.</span></span> <span data-ttu-id="e46e0-289">使用创建类类型的实例*object_creation_expression*s ([对象创建表达式](expressions.md#object-creation-expressions))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-289">Instances of class types are created using *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>

<span data-ttu-id="e46e0-290">中介绍了类类型[类](classes.md)。</span><span class="sxs-lookup"><span data-stu-id="e46e0-290">Class types are described in [Classes](classes.md).</span></span>

<span data-ttu-id="e46e0-291">某些预定义的类类型具有特殊含义，在 C# 语言中下, 表中所述。</span><span class="sxs-lookup"><span data-stu-id="e46e0-291">Certain predefined class types have special meaning in the C# language, as described in the table below.</span></span>


| <span data-ttu-id="e46e0-292">__类类型__</span><span class="sxs-lookup"><span data-stu-id="e46e0-292">__Class type__</span></span>     | <span data-ttu-id="e46e0-293">__说明__</span><span class="sxs-lookup"><span data-stu-id="e46e0-293">__Description__</span></span>                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | <span data-ttu-id="e46e0-294">所有其他类型的最终基类。</span><span class="sxs-lookup"><span data-stu-id="e46e0-294">The ultimate base class of all other types.</span></span> <span data-ttu-id="e46e0-295">请参阅[的对象类型](types.md#the-object-type)。</span><span class="sxs-lookup"><span data-stu-id="e46e0-295">See [The object type](types.md#the-object-type).</span></span> | 
| `System.String`    | <span data-ttu-id="e46e0-296">C# 语言的字符串类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-296">The string type of the C# language.</span></span> <span data-ttu-id="e46e0-297">请参阅[字符串类型](types.md#the-string-type)。</span><span class="sxs-lookup"><span data-stu-id="e46e0-297">See [The string type](types.md#the-string-type).</span></span>         |
| `System.ValueType` | <span data-ttu-id="e46e0-298">所有值类型的基类。</span><span class="sxs-lookup"><span data-stu-id="e46e0-298">The base class of all value types.</span></span> <span data-ttu-id="e46e0-299">请参阅[System.ValueType 类型](types.md#the-systemvaluetype-type)。</span><span class="sxs-lookup"><span data-stu-id="e46e0-299">See [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>          |
| `System.Enum`      | <span data-ttu-id="e46e0-300">所有枚举类型的基类。</span><span class="sxs-lookup"><span data-stu-id="e46e0-300">The base class of all enum types.</span></span> <span data-ttu-id="e46e0-301">请参阅[枚举](enums.md)。</span><span class="sxs-lookup"><span data-stu-id="e46e0-301">See [Enums](enums.md).</span></span>              |
| `System.Array`     | <span data-ttu-id="e46e0-302">所有数组类型的基类。</span><span class="sxs-lookup"><span data-stu-id="e46e0-302">The base class of all array types.</span></span> <span data-ttu-id="e46e0-303">请参阅[数组](arrays.md)。</span><span class="sxs-lookup"><span data-stu-id="e46e0-303">See [Arrays](arrays.md).</span></span>             |
| `System.Delegate`  | <span data-ttu-id="e46e0-304">所有委托类型的基类。</span><span class="sxs-lookup"><span data-stu-id="e46e0-304">The base class of all delegate types.</span></span> <span data-ttu-id="e46e0-305">请参阅[委托](delegates.md)。</span><span class="sxs-lookup"><span data-stu-id="e46e0-305">See [Delegates](delegates.md).</span></span>          |
| `System.Exception` | <span data-ttu-id="e46e0-306">所有异常类型的基类。</span><span class="sxs-lookup"><span data-stu-id="e46e0-306">The base class of all exception types.</span></span> <span data-ttu-id="e46e0-307">请参阅[异常](exceptions.md)。</span><span class="sxs-lookup"><span data-stu-id="e46e0-307">See [Exceptions](exceptions.md).</span></span>         |

### <a name="the-object-type"></a><span data-ttu-id="e46e0-308">对象类型</span><span class="sxs-lookup"><span data-stu-id="e46e0-308">The object type</span></span>

<span data-ttu-id="e46e0-309">`object`类类型是所有其他类型的最基本基类。</span><span class="sxs-lookup"><span data-stu-id="e46e0-309">The `object` class type is the ultimate base class of all other types.</span></span> <span data-ttu-id="e46e0-310">每个类型在 C# 中的直接或间接派生`object`类类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-310">Every type in C# directly or indirectly derives from the `object` class type.</span></span>

<span data-ttu-id="e46e0-311">关键字`object`是只需为预定义的类的别名`System.Object`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-311">The keyword `object` is simply an alias for the predefined class `System.Object`.</span></span>

### <a name="the-dynamic-type"></a><span data-ttu-id="e46e0-312">动态类型</span><span class="sxs-lookup"><span data-stu-id="e46e0-312">The dynamic type</span></span>

<span data-ttu-id="e46e0-313">`dynamic`类型，如`object`，可以引用任何其他对象。</span><span class="sxs-lookup"><span data-stu-id="e46e0-313">The `dynamic` type, like `object`, can reference any object.</span></span> <span data-ttu-id="e46e0-314">当将运算符应用于类型的表达式`dynamic`，其解决推迟到运行该程序。</span><span class="sxs-lookup"><span data-stu-id="e46e0-314">When operators are applied to expressions of type `dynamic`, their resolution is deferred until the program is run.</span></span> <span data-ttu-id="e46e0-315">因此，如果合法不能将运算符应用于所引用对象，会在编译期间发出错误信息。</span><span class="sxs-lookup"><span data-stu-id="e46e0-315">Thus, if the operator cannot legally be applied to the referenced object, no error is given during compilation.</span></span> <span data-ttu-id="e46e0-316">而是运算符的解析在运行时失败时，将引发异常。</span><span class="sxs-lookup"><span data-stu-id="e46e0-316">Instead an exception will be thrown when resolution of the operator fails at run-time.</span></span>

<span data-ttu-id="e46e0-317">其目的是允许动态绑定，在详细信息中所述[动态绑定](expressions.md#dynamic-binding)。</span><span class="sxs-lookup"><span data-stu-id="e46e0-317">Its purpose is to allow dynamic binding, which is described in detail in [Dynamic binding](expressions.md#dynamic-binding).</span></span>

<span data-ttu-id="e46e0-318">`dynamic` 被视为等于`object`在以下方面除外：</span><span class="sxs-lookup"><span data-stu-id="e46e0-318">`dynamic` is considered identical to `object` except in the following respects:</span></span>

*  <span data-ttu-id="e46e0-319">类型的表达式上的操作`dynamic`可以为动态绑定 ([动态绑定](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-319">Operations on expressions of type `dynamic` can be dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span>
*  <span data-ttu-id="e46e0-320">类型推理 ([类型推理](expressions.md#type-inference)) 会更喜欢`dynamic`转移`object`如果两者都是候选项。</span><span class="sxs-lookup"><span data-stu-id="e46e0-320">Type inference ([Type inference](expressions.md#type-inference)) will prefer `dynamic` over `object` if both are candidates.</span></span>

<span data-ttu-id="e46e0-321">由于此等效性，以下包含：</span><span class="sxs-lookup"><span data-stu-id="e46e0-321">Because of this equivalence, the following holds:</span></span>

*  <span data-ttu-id="e46e0-322">没有之间的隐式标识转换`object`并`dynamic`，并替换时是相同的构造类型之间`dynamic`与 `object`</span><span class="sxs-lookup"><span data-stu-id="e46e0-322">There is an implicit identity conversion between `object` and `dynamic`, and between constructed types that are the same when replacing `dynamic` with `object`</span></span>
*  <span data-ttu-id="e46e0-323">之间的隐式和显式转换`object`还将应用与`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-323">Implicit and explicit conversions to and from `object` also apply to and from `dynamic`.</span></span>
*  <span data-ttu-id="e46e0-324">替换时是相同的方法签名`dynamic`与`object`被视为相同的签名</span><span class="sxs-lookup"><span data-stu-id="e46e0-324">Method signatures that are the same when replacing `dynamic` with `object` are considered the same signature</span></span>
*  <span data-ttu-id="e46e0-325">类型`dynamic`相区别`object`在运行时。</span><span class="sxs-lookup"><span data-stu-id="e46e0-325">The type `dynamic` is indistinguishable from `object` at run-time.</span></span>
*  <span data-ttu-id="e46e0-326">类型的表达式`dynamic`被称为***动态表达式***。</span><span class="sxs-lookup"><span data-stu-id="e46e0-326">An expression of the type `dynamic` is referred to as a ***dynamic expression***.</span></span>

### <a name="the-string-type"></a><span data-ttu-id="e46e0-327">字符串类型</span><span class="sxs-lookup"><span data-stu-id="e46e0-327">The string type</span></span>

<span data-ttu-id="e46e0-328">`string`类型是密封的类类型直接继承`object`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-328">The `string` type is a sealed class type that inherits directly from `object`.</span></span> <span data-ttu-id="e46e0-329">实例`string`类表示 Unicode 字符字符串。</span><span class="sxs-lookup"><span data-stu-id="e46e0-329">Instances of the `string` class represent Unicode character strings.</span></span>

<span data-ttu-id="e46e0-330">值`string`类型可以写为字符串文本 ([的字符串文本](lexical-structure.md#string-literals))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-330">Values of the `string` type can be written as string literals ([String literals](lexical-structure.md#string-literals)).</span></span>

<span data-ttu-id="e46e0-331">关键字`string`是只需为预定义的类的别名`System.String`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-331">The keyword `string` is simply an alias for the predefined class `System.String`.</span></span>

### <a name="interface-types"></a><span data-ttu-id="e46e0-332">接口类型</span><span class="sxs-lookup"><span data-stu-id="e46e0-332">Interface types</span></span>

<span data-ttu-id="e46e0-333">接口定义一个协定。</span><span class="sxs-lookup"><span data-stu-id="e46e0-333">An interface defines a contract.</span></span> <span data-ttu-id="e46e0-334">类或结构实现的接口必须遵守其协定。</span><span class="sxs-lookup"><span data-stu-id="e46e0-334">A class or struct that implements an interface must adhere to its contract.</span></span> <span data-ttu-id="e46e0-335">一个接口可能从多个基接口继承，类或结构可以实现多个接口。</span><span class="sxs-lookup"><span data-stu-id="e46e0-335">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="e46e0-336">接口类型所述[接口](interfaces.md)。</span><span class="sxs-lookup"><span data-stu-id="e46e0-336">Interface types are described in [Interfaces](interfaces.md).</span></span>

### <a name="array-types"></a><span data-ttu-id="e46e0-337">数组类型</span><span class="sxs-lookup"><span data-stu-id="e46e0-337">Array types</span></span>

<span data-ttu-id="e46e0-338">数组是包含零个或多个可通过计算索引访问的变量的数据结构。</span><span class="sxs-lookup"><span data-stu-id="e46e0-338">An array is a data structure that contains zero or more variables which are accessed through computed indices.</span></span> <span data-ttu-id="e46e0-339">一个数组，也称为数组的元素中包含的变量包括所有相同的类型，而这种类型称为数组的元素类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-339">The variables contained in an array, also called the elements of the array, are all of the same type, and this type is called the element type of the array.</span></span>

<span data-ttu-id="e46e0-340">数组类型中所述[数组](arrays.md)。</span><span class="sxs-lookup"><span data-stu-id="e46e0-340">Array types are described in [Arrays](arrays.md).</span></span>

### <a name="delegate-types"></a><span data-ttu-id="e46e0-341">委托类型</span><span class="sxs-lookup"><span data-stu-id="e46e0-341">Delegate types</span></span>

<span data-ttu-id="e46e0-342">委托是指一个或多个方法的数据结构。</span><span class="sxs-lookup"><span data-stu-id="e46e0-342">A delegate is a data structure that refers to one or more methods.</span></span> <span data-ttu-id="e46e0-343">对于实例方法，它还指其对应的对象实例。</span><span class="sxs-lookup"><span data-stu-id="e46e0-343">For instance methods, it also refers to their corresponding object instances.</span></span>

<span data-ttu-id="e46e0-344">最接近的 C 或 c + + 中的委托是函数指针，但函数指针只能引用静态函数，而委托可以引用静态和实例方法。</span><span class="sxs-lookup"><span data-stu-id="e46e0-344">The closest equivalent of a delegate in C or C++ is a function pointer, but whereas a function pointer can only reference static functions, a delegate can reference both static and instance methods.</span></span> <span data-ttu-id="e46e0-345">在后一种情况下，委托不仅存储存储的引用方法的入口点，还对在其上调用该方法的对象实例的引用。</span><span class="sxs-lookup"><span data-stu-id="e46e0-345">In the latter case, the delegate stores not only a reference to the method's entry point, but also a reference to the object instance on which to invoke the method.</span></span>

<span data-ttu-id="e46e0-346">中介绍了委托类型[委托](delegates.md)。</span><span class="sxs-lookup"><span data-stu-id="e46e0-346">Delegate types are described in [Delegates](delegates.md).</span></span>

## <a name="boxing-and-unboxing"></a><span data-ttu-id="e46e0-347">装箱和取消装箱</span><span class="sxs-lookup"><span data-stu-id="e46e0-347">Boxing and unboxing</span></span>

<span data-ttu-id="e46e0-348">装箱和取消装箱的概念是 C# 类型系统的核心。</span><span class="sxs-lookup"><span data-stu-id="e46e0-348">The concept of boxing and unboxing is central to C#'s type system.</span></span> <span data-ttu-id="e46e0-349">它提供了之间的桥梁*value_type*s 和*reference_type*允许存在的任何值的 s *value_type*要转换到和从类型`object`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-349">It provides a bridge between *value_type*s and *reference_type*s by permitting any value of a *value_type* to be converted to and from type `object`.</span></span> <span data-ttu-id="e46e0-350">装箱和取消装箱使类型系统，其中任何类型的值最终都可以作为对象处理的统一的视图。</span><span class="sxs-lookup"><span data-stu-id="e46e0-350">Boxing and unboxing enables a unified view of the type system wherein a value of any type can ultimately be treated as an object.</span></span>

### <a name="boxing-conversions"></a><span data-ttu-id="e46e0-351">装箱转换</span><span class="sxs-lookup"><span data-stu-id="e46e0-351">Boxing conversions</span></span>

<span data-ttu-id="e46e0-352">装箱转换允许*value_type*隐式转换为*reference_type*。</span><span class="sxs-lookup"><span data-stu-id="e46e0-352">A boxing conversion permits a *value_type* to be implicitly converted to a *reference_type*.</span></span> <span data-ttu-id="e46e0-353">存在以下装箱转换：</span><span class="sxs-lookup"><span data-stu-id="e46e0-353">The following boxing conversions exist:</span></span>

*  <span data-ttu-id="e46e0-354">从任何*value_type*为类型`object`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-354">From any *value_type* to the type `object`.</span></span>
*  <span data-ttu-id="e46e0-355">从任何*value_type*为类型`System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-355">From any *value_type* to the type `System.ValueType`.</span></span>
*  <span data-ttu-id="e46e0-356">从任何*non_nullable_value_type*任何*interface_type*由实现*value_type*。</span><span class="sxs-lookup"><span data-stu-id="e46e0-356">From any *non_nullable_value_type* to any *interface_type* implemented by the *value_type*.</span></span>
*  <span data-ttu-id="e46e0-357">从任何*nullable_type*任何*interface_type*实现的基础类型的*nullable_type*。</span><span class="sxs-lookup"><span data-stu-id="e46e0-357">From any *nullable_type* to any *interface_type* implemented by the underlying type of the *nullable_type*.</span></span>
*  <span data-ttu-id="e46e0-358">从任何*enum_type*为类型`System.Enum`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-358">From any *enum_type* to the type `System.Enum`.</span></span>
*  <span data-ttu-id="e46e0-359">从任何*nullable_type*与基础*enum_type*为类型`System.Enum`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-359">From any *nullable_type* with an underlying *enum_type* to the type `System.Enum`.</span></span>
*  <span data-ttu-id="e46e0-360">请注意从类型参数的隐式转换将执行作为装箱转换中，是否在运行时其结果是从值类型转换为引用类型 ([涉及类型参数的隐式转换](conversions.md#implicit-conversions-involving-type-parameters))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-360">Note that an implicit conversion from a type parameter will be executed as a boxing conversion if at run-time it ends up converting from a value type to a reference type ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters)).</span></span>

<span data-ttu-id="e46e0-361">装箱的值*non_nullable_value_type*分配给对象实例和复制组成*non_nullable_value_type*到该实例的值。</span><span class="sxs-lookup"><span data-stu-id="e46e0-361">Boxing a value of a *non_nullable_value_type* consists of allocating an object instance and copying the *non_nullable_value_type* value into that instance.</span></span>

<span data-ttu-id="e46e0-362">装箱的值*nullable_type*生成一个空引用，它是否`null`值 (`HasValue`是`false`)，或解包和否则装箱的基础值的结果。</span><span class="sxs-lookup"><span data-stu-id="e46e0-362">Boxing a value of a *nullable_type* produces a null reference if it is the `null` value (`HasValue` is `false`), or the result of unwrapping and boxing the underlying value otherwise.</span></span>

<span data-ttu-id="e46e0-363">装箱的值的实际过程*non_nullable_value_type*很好地解释由专门的泛型是否存在***装箱类***，其行为就像它已声明，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e46e0-363">The actual process of boxing a value of a *non_nullable_value_type* is best explained by imagining the existence of a generic ***boxing class***, which behaves as if it were declared as follows:</span></span>

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

<span data-ttu-id="e46e0-364">值的装箱`v`类型的`T`现在包含执行表达式`new Box<T>(v)`，并返回生成的实例类型的值作为`object`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-364">Boxing of a value `v` of type `T` now consists of executing the expression `new Box<T>(v)`, and returning the resulting instance as a value of type `object`.</span></span> <span data-ttu-id="e46e0-365">因此，下面的语句</span><span class="sxs-lookup"><span data-stu-id="e46e0-365">Thus, the statements</span></span>
```csharp
int i = 123;
object box = i;
```
<span data-ttu-id="e46e0-366">从概念上讲对应于</span><span class="sxs-lookup"><span data-stu-id="e46e0-366">conceptually correspond to</span></span>
```csharp
int i = 123;
object box = new Box<int>(i);
```

<span data-ttu-id="e46e0-367">装箱类，如`Box<T>`更高版本并不实际存在和装箱值的动态类型不是实际的类类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-367">A boxing class like `Box<T>` above doesn't actually exist and the dynamic type of a boxed value isn't actually a class type.</span></span> <span data-ttu-id="e46e0-368">相反，类型的装箱的值`T`具有动态类型`T`，和动态类型检查使用`is`运算符只是引用类型`T`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-368">Instead, a boxed value of type `T` has the dynamic type `T`, and a dynamic type check using the `is` operator can simply reference type `T`.</span></span> <span data-ttu-id="e46e0-369">例如，应用于对象的</span><span class="sxs-lookup"><span data-stu-id="e46e0-369">For example,</span></span>
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
<span data-ttu-id="e46e0-370">将输出字符串"`Box contains an int`"控制台上。</span><span class="sxs-lookup"><span data-stu-id="e46e0-370">will output the string "`Box contains an int`" on the console.</span></span>

<span data-ttu-id="e46e0-371">装箱转换隐含着一份待装箱的值。</span><span class="sxs-lookup"><span data-stu-id="e46e0-371">A boxing conversion implies making a copy of the value being boxed.</span></span> <span data-ttu-id="e46e0-372">这与不同的转换*reference_type*键入`object`，将继续引用同一个实例以及仅被视为派生程度更小的类型值中`object`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-372">This is different from a conversion of a *reference_type* to type `object`, in which the value continues to reference the same instance and simply is regarded as the less derived type `object`.</span></span> <span data-ttu-id="e46e0-373">例如，给定声明</span><span class="sxs-lookup"><span data-stu-id="e46e0-373">For example, given the declaration</span></span>
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
<span data-ttu-id="e46e0-374">下面的语句</span><span class="sxs-lookup"><span data-stu-id="e46e0-374">the following statements</span></span>
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
<span data-ttu-id="e46e0-375">将输出在控制台上的值 10，因为发生的分配中的隐式装箱操作`p`到`box`的值将导致`p`要复制。</span><span class="sxs-lookup"><span data-stu-id="e46e0-375">will output the value 10 on the console because the implicit boxing operation that occurs in the assignment of `p` to `box` causes the value of `p` to be copied.</span></span> <span data-ttu-id="e46e0-376">有`Point`已声明`class`相反，值 20 会是输出，因为`p`和`box`将引用同一个实例。</span><span class="sxs-lookup"><span data-stu-id="e46e0-376">Had `Point` been declared a `class` instead, the value 20 would be output because `p` and `box` would reference the same instance.</span></span>

### <a name="unboxing-conversions"></a><span data-ttu-id="e46e0-377">取消装箱转换</span><span class="sxs-lookup"><span data-stu-id="e46e0-377">Unboxing conversions</span></span>

<span data-ttu-id="e46e0-378">取消装箱转换允许*reference_type*显式转换为*value_type*。</span><span class="sxs-lookup"><span data-stu-id="e46e0-378">An unboxing conversion permits a *reference_type* to be explicitly converted to a *value_type*.</span></span> <span data-ttu-id="e46e0-379">存在以下取消装箱转换：</span><span class="sxs-lookup"><span data-stu-id="e46e0-379">The following unboxing conversions exist:</span></span>

*  <span data-ttu-id="e46e0-380">从类型`object`到任何*value_type*。</span><span class="sxs-lookup"><span data-stu-id="e46e0-380">From the type `object` to any *value_type*.</span></span>
*  <span data-ttu-id="e46e0-381">从类型`System.ValueType`到任何*value_type*。</span><span class="sxs-lookup"><span data-stu-id="e46e0-381">From the type `System.ValueType` to any *value_type*.</span></span>
*  <span data-ttu-id="e46e0-382">从任何*interface_type*任何*non_nullable_value_type*实现*interface_type*。</span><span class="sxs-lookup"><span data-stu-id="e46e0-382">From any *interface_type* to any *non_nullable_value_type* that implements the *interface_type*.</span></span>
*  <span data-ttu-id="e46e0-383">从任何*interface_type*任何*nullable_type*其基础类型实现*interface_type*。</span><span class="sxs-lookup"><span data-stu-id="e46e0-383">From any *interface_type* to any *nullable_type* whose underlying type implements the *interface_type*.</span></span>
*  <span data-ttu-id="e46e0-384">从类型`System.Enum`到任何*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="e46e0-384">From the type `System.Enum` to any *enum_type*.</span></span>
*  <span data-ttu-id="e46e0-385">从类型`System.Enum`到任何*nullable_type*与基础*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="e46e0-385">From the type `System.Enum` to any *nullable_type* with an underlying *enum_type*.</span></span>
*  <span data-ttu-id="e46e0-386">请注意类型参数的显式转换将执行作为取消装箱转换中，是否在运行时其结果是从引用类型转换为值类型 ([显式动态转换](conversions.md#explicit-dynamic-conversions))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-386">Note that an explicit conversion to a type parameter will be executed as an unboxing conversion if at run-time it ends up converting from a reference type to a value type ([Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions)).</span></span>

<span data-ttu-id="e46e0-387">取消装箱操作向*non_nullable_value_type*组成的对象实例的装箱的值的第一个检查给定*non_nullable_value_type*，然后复制出的值实例。</span><span class="sxs-lookup"><span data-stu-id="e46e0-387">An unboxing operation to a *non_nullable_value_type* consists of first checking that the object instance is a boxed value of the given *non_nullable_value_type*, and then copying the value out of the instance.</span></span>

<span data-ttu-id="e46e0-388">到取消装箱*nullable_type*生成的 null 值*nullable_type*如果源操作数为`null`，或取消装箱的基础类型的对象实例的已包装的结果*nullable_type*否则为。</span><span class="sxs-lookup"><span data-stu-id="e46e0-388">Unboxing to a *nullable_type* produces the null value of the *nullable_type* if the source operand is `null`, or the wrapped result of unboxing the object instance to the underlying type of the *nullable_type* otherwise.</span></span>

<span data-ttu-id="e46e0-389">引用在上一部分中，取消装箱转换的对象中所述的虚部装箱类`box`到*value_type* `T`组成执行表达式`((Box<T>)box).value`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-389">Referring to the imaginary boxing class described in the previous section, an unboxing conversion of an object `box` to a *value_type* `T` consists of executing the expression `((Box<T>)box).value`.</span></span> <span data-ttu-id="e46e0-390">因此，下面的语句</span><span class="sxs-lookup"><span data-stu-id="e46e0-390">Thus, the statements</span></span>
```csharp
object box = 123;
int i = (int)box;
```
<span data-ttu-id="e46e0-391">从概念上讲对应于</span><span class="sxs-lookup"><span data-stu-id="e46e0-391">conceptually correspond to</span></span>
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

<span data-ttu-id="e46e0-392">取消装箱转换为给定*non_nullable_value_type*若要成功运行时，源操作数的值必须是对已装箱值，该值的引用*non_nullable_value_type*。</span><span class="sxs-lookup"><span data-stu-id="e46e0-392">For an unboxing conversion to a given *non_nullable_value_type* to succeed at run-time, the value of the source operand must be a reference to a boxed value of that *non_nullable_value_type*.</span></span> <span data-ttu-id="e46e0-393">如果源操作数`null`、`System.NullReferenceException`引发。</span><span class="sxs-lookup"><span data-stu-id="e46e0-393">If the source operand is `null`, a `System.NullReferenceException` is thrown.</span></span> <span data-ttu-id="e46e0-394">如果源操作数是一个不兼容的对象的引用`System.InvalidCastException`引发。</span><span class="sxs-lookup"><span data-stu-id="e46e0-394">If the source operand is a reference to an incompatible object, a `System.InvalidCastException` is thrown.</span></span>

<span data-ttu-id="e46e0-395">取消装箱转换为给定*nullable_type*若要成功运行时，源操作数的值必须是`null`或对基础的装箱值的引用*non_nullable_value_type*的*nullable_type*。</span><span class="sxs-lookup"><span data-stu-id="e46e0-395">For an unboxing conversion to a given *nullable_type* to succeed at run-time, the value of the source operand must be either `null` or a reference to a boxed value of the underlying *non_nullable_value_type* of the *nullable_type*.</span></span> <span data-ttu-id="e46e0-396">如果源操作数是一个不兼容的对象的引用`System.InvalidCastException`引发。</span><span class="sxs-lookup"><span data-stu-id="e46e0-396">If the source operand is a reference to an incompatible object, a `System.InvalidCastException` is thrown.</span></span>

## <a name="constructed-types"></a><span data-ttu-id="e46e0-397">构造的类型</span><span class="sxs-lookup"><span data-stu-id="e46e0-397">Constructed types</span></span>

<span data-ttu-id="e46e0-398">泛型类型声明，其本身而言，表示***未绑定的泛型类型***用作"蓝图"以形成许多不同类型，通过应用***类型参数***。</span><span class="sxs-lookup"><span data-stu-id="e46e0-398">A generic type declaration, by itself, denotes an ***unbound generic type*** that is used as a "blueprint" to form many different types, by way of applying ***type arguments***.</span></span> <span data-ttu-id="e46e0-399">类型实参编写在尖括号 (`<`和`>`) 紧跟泛型类型的名称。</span><span class="sxs-lookup"><span data-stu-id="e46e0-399">The type arguments are written within angle brackets (`<` and `>`) immediately following the name of the generic type.</span></span> <span data-ttu-id="e46e0-400">包含至少一个类型参数的类型称为***构造类型***。</span><span class="sxs-lookup"><span data-stu-id="e46e0-400">A type that includes at least one type argument is called a ***constructed type***.</span></span> <span data-ttu-id="e46e0-401">可在类型名称可以在其中显示的语言中的大多数位置构造的类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-401">A constructed type can be used in most places in the language in which a type name can appear.</span></span> <span data-ttu-id="e46e0-402">未绑定的泛型类型仅可在*typeof_expression* ([typeof 运算符](expressions.md#the-typeof-operator))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-402">An unbound generic type can only be used within a *typeof_expression* ([The typeof operator](expressions.md#the-typeof-operator)).</span></span>

<span data-ttu-id="e46e0-403">构造的类型还可在表达式中作为简单名称 ([的简单名称](expressions.md#simple-names)) 或访问成员时 ([成员访问](expressions.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-403">Constructed types can also be used in expressions as simple names ([Simple names](expressions.md#simple-names)) or when accessing a member ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="e46e0-404">当*namespace_or_type_name*是使用正确的类型参数被视为数计算，仅泛型类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-404">When a *namespace_or_type_name* is evaluated, only generic types with the correct number of type parameters are considered.</span></span> <span data-ttu-id="e46e0-405">因此，它是可以使用相同的标识符来标识不同类型，只要类型具有不同数量的类型参数。</span><span class="sxs-lookup"><span data-stu-id="e46e0-405">Thus, it is possible to use the same identifier to identify different types, as long as the types have different numbers of type parameters.</span></span> <span data-ttu-id="e46e0-406">混合在同一程序中的泛型和非泛型类时，这非常有用：</span><span class="sxs-lookup"><span data-stu-id="e46e0-406">This is useful when mixing generic and non-generic classes in the same program:</span></span>

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

<span data-ttu-id="e46e0-407">一个*type_name*可能会标识构造的类型，即使它不直接指定类型参数。</span><span class="sxs-lookup"><span data-stu-id="e46e0-407">A *type_name* might identify a constructed type even though it doesn't specify type parameters directly.</span></span> <span data-ttu-id="e46e0-408">这可能是其中一种类型嵌套在泛型类声明中，并且包含声明的实例类型隐式用于名称查找 ([嵌套在泛型类类型](classes.md#nested-types-in-generic-classes)):</span><span class="sxs-lookup"><span data-stu-id="e46e0-408">This can occur where a type is nested within a generic class declaration, and the instance type of the containing declaration is implicitly used for name lookup ([Nested types in generic classes](classes.md#nested-types-in-generic-classes)):</span></span>

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

<span data-ttu-id="e46e0-409">不安全代码中构造的类型不能用作*unmanaged_type* ([指针类型](unsafe-code.md#pointer-types))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-409">In unsafe code, a constructed type cannot be used as an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

### <a name="type-arguments"></a><span data-ttu-id="e46e0-410">类型参数</span><span class="sxs-lookup"><span data-stu-id="e46e0-410">Type arguments</span></span>

<span data-ttu-id="e46e0-411">类型实参列表中的每个参数是只需*类型*。</span><span class="sxs-lookup"><span data-stu-id="e46e0-411">Each argument in a type argument list is simply a *type*.</span></span>

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

<span data-ttu-id="e46e0-412">在不安全代码 ([不安全代码](unsafe-code.md))、 一个*type_argument*可能不是指针类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-412">In unsafe code ([Unsafe code](unsafe-code.md)), a *type_argument* may not be a pointer type.</span></span> <span data-ttu-id="e46e0-413">每个类型实参必须满足在相应的所有约束都类型参数 ([都类型参数约束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-413">Each type argument must satisfy any constraints on the corresponding type parameter ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>

### <a name="open-and-closed-types"></a><span data-ttu-id="e46e0-414">打开和关闭类型</span><span class="sxs-lookup"><span data-stu-id="e46e0-414">Open and closed types</span></span>

<span data-ttu-id="e46e0-415">可以为所有类型进行都分类***开放类型***或***封闭类型***。</span><span class="sxs-lookup"><span data-stu-id="e46e0-415">All types can be classified as either ***open types*** or ***closed types***.</span></span> <span data-ttu-id="e46e0-416">开放类型是涉及类型参数的类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-416">An open type is a type that involves type parameters.</span></span> <span data-ttu-id="e46e0-417">具体而言：</span><span class="sxs-lookup"><span data-stu-id="e46e0-417">More specifically:</span></span>

*  <span data-ttu-id="e46e0-418">类型参数定义开放类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-418">A type parameter defines an open type.</span></span>
*  <span data-ttu-id="e46e0-419">数组类型是开放类型，当且仅当其元素类型是开放类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-419">An array type is an open type if and only if its element type is an open type.</span></span>
*  <span data-ttu-id="e46e0-420">构造的类型是开放类型，当且仅当一个或多个其类型参数是开放类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-420">A constructed type is an open type if and only if one or more of its type arguments is an open type.</span></span> <span data-ttu-id="e46e0-421">当且仅当一个或多个其类型参数或其包含类型的类型参数是开放类型，构造的嵌套的类型是开放类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-421">A constructed nested type is an open type if and only if one or more of its type arguments or the type arguments of its containing type(s) is an open type.</span></span>

<span data-ttu-id="e46e0-422">关闭的类型是不是开放类型的类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-422">A closed type is a type that is not an open type.</span></span>

<span data-ttu-id="e46e0-423">在运行时，所有的泛型类型声明中的代码是通过将类型参数应用于泛型声明创建封闭式构造类型的上下文中执行。</span><span class="sxs-lookup"><span data-stu-id="e46e0-423">At run-time, all of the code within a generic type declaration is executed in the context of a closed constructed type that was created by applying type arguments to the generic declaration.</span></span> <span data-ttu-id="e46e0-424">泛型类型中的每个类型参数是绑定到特定的运行时类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-424">Each type parameter within the generic type is bound to a particular run-time type.</span></span> <span data-ttu-id="e46e0-425">运行时处理的所有语句和表达式始终是使用封闭类型，并打开类型仅出现在编译时处理。</span><span class="sxs-lookup"><span data-stu-id="e46e0-425">The run-time processing of all statements and expressions always occurs with closed types, and open types occur only during compile-time processing.</span></span>

<span data-ttu-id="e46e0-426">每个封闭的构造的类型都有自己的静态变量，不与任何其他封闭式构造类型共享的一组。</span><span class="sxs-lookup"><span data-stu-id="e46e0-426">Each closed constructed type has its own set of static variables, which are not shared with any other closed constructed types.</span></span> <span data-ttu-id="e46e0-427">在运行时，打开类型不存在，因为没有静态变量与开放类型相关联。</span><span class="sxs-lookup"><span data-stu-id="e46e0-427">Since an open type does not exist at run-time, there are no static variables associated with an open type.</span></span> <span data-ttu-id="e46e0-428">如果它们从同一个未绑定泛型类型，构造和其相应的类型参数具有相同的类型，则两个封闭式构造的类型具有相同的类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-428">Two closed constructed types are the same type if they are constructed from the same unbound generic type, and their corresponding type arguments are the same type.</span></span>

### <a name="bound-and-unbound-types"></a><span data-ttu-id="e46e0-429">绑定和未绑定类型</span><span class="sxs-lookup"><span data-stu-id="e46e0-429">Bound and unbound types</span></span>

<span data-ttu-id="e46e0-430">术语***未绑定类型***指非泛型类型或未绑定的泛型类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-430">The term ***unbound type*** refers to a non-generic type or an unbound generic type.</span></span> <span data-ttu-id="e46e0-431">术语***绑定类型***指非泛型类型或构造的类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-431">The term ***bound type*** refers to a non-generic type or a constructed type.</span></span>

<span data-ttu-id="e46e0-432">未绑定的类型是指声明的类型声明的实体。</span><span class="sxs-lookup"><span data-stu-id="e46e0-432">An unbound type refers to the entity declared by a type declaration.</span></span> <span data-ttu-id="e46e0-433">未绑定的泛型类型本身不是一种类型，并为变量、 参数或返回值的类型或基类型不能使用。</span><span class="sxs-lookup"><span data-stu-id="e46e0-433">An unbound generic type is not itself a type, and cannot be used as the type of a variable, argument or return value, or as a base type.</span></span> <span data-ttu-id="e46e0-434">可以在其中引用未绑定的泛型类型的唯一构造是`typeof`表达式 ([typeof 运算符](expressions.md#the-typeof-operator))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-434">The only construct in which an unbound generic type can be referenced is the `typeof` expression ([The typeof operator](expressions.md#the-typeof-operator)).</span></span>

### <a name="satisfying-constraints"></a><span data-ttu-id="e46e0-435">令人满意的约束</span><span class="sxs-lookup"><span data-stu-id="e46e0-435">Satisfying constraints</span></span>

<span data-ttu-id="e46e0-436">每当引用构造的类型或泛型方法时，都会根据泛型类型或方法上声明的类型参数约束检查提供的类型实参 ([类型参数约束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-436">Whenever a constructed type or generic method is referenced, the supplied type arguments are checked against the type parameter constraints declared on the generic type or method ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="e46e0-437">每个`where`子句中，类型参数`A`对应于已命名类型参数检查针对每个约束，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e46e0-437">For each `where` clause, the type argument `A` that corresponds to the named type parameter is checked against each constraint as follows:</span></span>

*  <span data-ttu-id="e46e0-438">如果约束为类类型、 接口类型或类型参数，则假设`C`表示具有提供的类型参数约束替换为该约束中出现的任何类型参数。</span><span class="sxs-lookup"><span data-stu-id="e46e0-438">If the constraint is a class type, an interface type, or a type parameter, let `C` represent that constraint with the supplied type arguments substituted for any type parameters that appear in the constraint.</span></span> <span data-ttu-id="e46e0-439">若要满足的约束，它必须是类型的用例`A`可转换为类型`C`由以下项之一：</span><span class="sxs-lookup"><span data-stu-id="e46e0-439">To satisfy the constraint, it must be the case that type `A` is convertible to type `C` by one of the following:</span></span>
    * <span data-ttu-id="e46e0-440">标识转换 ([标识转换](conversions.md#identity-conversion))</span><span class="sxs-lookup"><span data-stu-id="e46e0-440">An identity conversion ([Identity conversion](conversions.md#identity-conversion))</span></span>
    * <span data-ttu-id="e46e0-441">隐式引用转换 ([隐式引用转换](conversions.md#implicit-reference-conversions))</span><span class="sxs-lookup"><span data-stu-id="e46e0-441">An implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions))</span></span>
    * <span data-ttu-id="e46e0-442">装箱转换 ([装箱转换](conversions.md#boxing-conversions))，前提是类型 A 是不可以为 null 的值类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-442">A boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)), provided that type A is a non-nullable value type.</span></span>
    * <span data-ttu-id="e46e0-443">从类型参数的隐式引用、 装箱或类型参数转换`A`到`C`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-443">An implicit reference, boxing or type parameter conversion from a type parameter `A` to `C`.</span></span>
*  <span data-ttu-id="e46e0-444">如果约束是引用类型约束 (`class`)，该类型`A`必须满足以下项之一：</span><span class="sxs-lookup"><span data-stu-id="e46e0-444">If the constraint is the reference type constraint (`class`), the type `A` must satisfy one of the following:</span></span>
    * <span data-ttu-id="e46e0-445">`A` 接口类型、 类类型、 委托类型或数组类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-445">`A` is an interface type, class type, delegate type or array type.</span></span> <span data-ttu-id="e46e0-446">请注意，`System.ValueType`和`System.Enum`是满足此约束的引用类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-446">Note that `System.ValueType` and `System.Enum` are reference types that satisfy this constraint.</span></span>
    * <span data-ttu-id="e46e0-447">`A` 是一个已知为引用类型的类型参数 ([类型参数约束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-447">`A` is a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="e46e0-448">如果约束是值类型约束 (`struct`)，该类型`A`必须满足以下项之一：</span><span class="sxs-lookup"><span data-stu-id="e46e0-448">If the constraint is the value type constraint (`struct`), the type `A` must satisfy one of the following:</span></span>
    * <span data-ttu-id="e46e0-449">`A` 为结构类型或枚举类型，但不是为 null 的类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-449">`A` is a struct type or enum type, but not a nullable type.</span></span> <span data-ttu-id="e46e0-450">请注意，`System.ValueType`和`System.Enum`是引用类型，不满足此约束。</span><span class="sxs-lookup"><span data-stu-id="e46e0-450">Note that `System.ValueType` and `System.Enum` are reference types that do not satisfy this constraint.</span></span>
    * <span data-ttu-id="e46e0-451">`A` 为具有值类型约束的类型形参 ([类型参数约束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-451">`A` is a type parameter having the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="e46e0-452">如果约束是构造函数约束`new()`，类型`A`不能`abstract`并且必须具有公共无参数构造函数。</span><span class="sxs-lookup"><span data-stu-id="e46e0-452">If the constraint is the constructor constraint `new()`, the type `A` must not be `abstract` and must have a public parameterless constructor.</span></span> <span data-ttu-id="e46e0-453">如果以下项之一为 true，条件则满足此问题：</span><span class="sxs-lookup"><span data-stu-id="e46e0-453">This is satisfied if one of the following is true:</span></span>
    * <span data-ttu-id="e46e0-454">`A` 为值类型，因为所有值类型都具有公共默认构造函数 ([默认构造函数](types.md#default-constructors))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-454">`A` is a value type, since all value types have a public default constructor ([Default constructors](types.md#default-constructors)).</span></span>
    * <span data-ttu-id="e46e0-455">`A` 为具有构造函数约束的类型形参 ([类型参数约束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-455">`A` is a type parameter having the constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
    * <span data-ttu-id="e46e0-456">`A` 为具有值类型约束的类型形参 ([类型参数约束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-456">`A` is a type parameter having the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
    * <span data-ttu-id="e46e0-457">`A` 是一个类，不是`abstract`和包含显式声明`public`不带任何参数的构造函数。</span><span class="sxs-lookup"><span data-stu-id="e46e0-457">`A` is a class that is not `abstract` and contains an explicitly declared `public` constructor with no parameters.</span></span>
    * <span data-ttu-id="e46e0-458">`A` 不是`abstract`并具有默认构造函数 ([默认构造函数](classes.md#default-constructors))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-458">`A` is not `abstract` and has a default constructor ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="e46e0-459">如果按给定的类型参数不满足一个或多个类型形参的约束，则，发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="e46e0-459">A compile-time error occurs if one or more of a type parameter's constraints are not satisfied by the given type arguments.</span></span>

<span data-ttu-id="e46e0-460">约束类型参数不继承的因为是永远不会继承任何一个。</span><span class="sxs-lookup"><span data-stu-id="e46e0-460">Since type parameters are not inherited, constraints are never inherited either.</span></span> <span data-ttu-id="e46e0-461">在以下示例中，`D`需要指定其类型形参上的约束`T`以便`T`满足所施加的类的基类约束`B<T>`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-461">In the example below, `D` needs to specify the constraint on its type parameter `T` so that `T` satisfies the constraint imposed by the base class `B<T>`.</span></span> <span data-ttu-id="e46e0-462">与此相反，类`E`不需要指定一个约束，因为`List<T>`实现`IEnumerable`任何`T`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-462">In contrast, class `E` need not specify a constraint, because `List<T>` implements `IEnumerable` for any `T`.</span></span>

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a><span data-ttu-id="e46e0-463">类型参数</span><span class="sxs-lookup"><span data-stu-id="e46e0-463">Type parameters</span></span>

<span data-ttu-id="e46e0-464">类型参数是值类型或引用类型的参数在运行时绑定到指定的标识符。</span><span class="sxs-lookup"><span data-stu-id="e46e0-464">A type parameter is an identifier designating a value type or reference type that the parameter is bound to at run-time.</span></span>

```antlr
type_parameter
    : identifier
    ;
```

<span data-ttu-id="e46e0-465">由于类型参数可以具有多个不同的实际类型参数实例化，类型参数具有与其他类型稍有不同的操作和限制。</span><span class="sxs-lookup"><span data-stu-id="e46e0-465">Since a type parameter can be instantiated with many different actual type arguments, type parameters have slightly different operations and restrictions than other types.</span></span> <span data-ttu-id="e46e0-466">这些问题包括：</span><span class="sxs-lookup"><span data-stu-id="e46e0-466">These include:</span></span>

*  <span data-ttu-id="e46e0-467">类型参数不能用于直接声明基类 ([基类](classes.md#base-class)) 或接口 ([变体类型的参数列表](interfaces.md#variant-type-parameter-lists))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-467">A type parameter cannot be used directly to declare a base class ([Base class](classes.md#base-class)) or interface ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)).</span></span>
*  <span data-ttu-id="e46e0-468">参数取决于该约束，如果有，成员查找类型上的规则应用于类型参数。</span><span class="sxs-lookup"><span data-stu-id="e46e0-468">The rules for member lookup on type parameters depend on the constraints, if any, applied to the type parameter.</span></span> <span data-ttu-id="e46e0-469">中详细介绍[成员查找](expressions.md#member-lookup)。</span><span class="sxs-lookup"><span data-stu-id="e46e0-469">They are detailed in [Member lookup](expressions.md#member-lookup).</span></span>
*  <span data-ttu-id="e46e0-470">可用的转换为类型参数取决于该约束，如果有的话，应用于类型参数。</span><span class="sxs-lookup"><span data-stu-id="e46e0-470">The available conversions for a type parameter depend on the constraints, if any, applied to the type parameter.</span></span> <span data-ttu-id="e46e0-471">中详细介绍[涉及类型参数的隐式转换](conversions.md#implicit-conversions-involving-type-parameters)并[显式动态转换](conversions.md#explicit-dynamic-conversions)。</span><span class="sxs-lookup"><span data-stu-id="e46e0-471">They are detailed in [Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters) and [Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions).</span></span>
*  <span data-ttu-id="e46e0-472">文字`null`不能转换为如果类型参数已知的引用类型的类型参数，除给定类型 ([涉及类型参数的隐式转换](conversions.md#implicit-conversions-involving-type-parameters))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-472">The literal `null` cannot be converted to a type given by a type parameter, except if the type parameter is known to be a reference type ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters)).</span></span> <span data-ttu-id="e46e0-473">但是，`default`表达式 ([默认值表达式](expressions.md#default-value-expressions)) 可改为使用。</span><span class="sxs-lookup"><span data-stu-id="e46e0-473">However, a `default` expression ([Default value expressions](expressions.md#default-value-expressions)) can be used instead.</span></span> <span data-ttu-id="e46e0-474">此外，具有类型参数提供的类型的值可以与进行比较`null`使用`==`并`!=`([引用类型相等运算符](expressions.md#reference-type-equality-operators)) 除非类型参数的值类型约束。</span><span class="sxs-lookup"><span data-stu-id="e46e0-474">In addition, a value with a type given by a type parameter can be compared with `null` using `==` and `!=` ([Reference type equality operators](expressions.md#reference-type-equality-operators)) unless the type parameter has the value type constraint.</span></span>
*  <span data-ttu-id="e46e0-475">一个`new`表达式 ([对象创建表达式](expressions.md#object-creation-expressions)) 可以仅将其用于类型参数如果类型形参受*constructor_constraint*或值类型约束 ([类型参数约束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-475">A `new` expression ([Object creation expressions](expressions.md#object-creation-expressions)) can only be used with a type parameter if the type parameter is constrained by a *constructor_constraint* or the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="e46e0-476">类型参数不能在特性中的任意位置使用。</span><span class="sxs-lookup"><span data-stu-id="e46e0-476">A type parameter cannot be used anywhere within an attribute.</span></span>
*  <span data-ttu-id="e46e0-477">类型参数不能使用成员访问中 ([成员访问](expressions.md#member-access)) 或类型名称 ([Namespace 和类型名称](basic-concepts.md#namespace-and-type-names)) 来标识的静态成员或嵌套的类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-477">A type parameter cannot be used in a member access ([Member access](expressions.md#member-access)) or type name ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) to identify a static member or a nested type.</span></span>
*  <span data-ttu-id="e46e0-478">在不安全代码中，类型参数不能用作*unmanaged_type* ([指针类型](unsafe-code.md#pointer-types))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-478">In unsafe code, a type parameter cannot be used as an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

<span data-ttu-id="e46e0-479">作为一种类型，类型参数都是纯粹是一个编译时构造。</span><span class="sxs-lookup"><span data-stu-id="e46e0-479">As a type, type parameters are purely a compile-time construct.</span></span> <span data-ttu-id="e46e0-480">在运行时，每个类型参数绑定到提供的泛型类型声明的类型参数指定的运行时类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-480">At run-time, each type parameter is bound to a run-time type that was specified by supplying a type argument to the generic type declaration.</span></span> <span data-ttu-id="e46e0-481">因此，使用类型参数将在运行时，声明一个变量的类型为封闭式构造的类型 ([打开和关闭类型](types.md#open-and-closed-types))。</span><span class="sxs-lookup"><span data-stu-id="e46e0-481">Thus, the type of a variable declared with a type parameter will, at run-time, be a closed constructed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="e46e0-482">运行时执行的所有语句和包含类型参数的表达式使用提供的实际类型作为该参数的类型参数。</span><span class="sxs-lookup"><span data-stu-id="e46e0-482">The run-time execution of all statements and expressions involving type parameters uses the actual type that was supplied as the type argument for that parameter.</span></span>

## <a name="expression-tree-types"></a><span data-ttu-id="e46e0-483">表达式树类型</span><span class="sxs-lookup"><span data-stu-id="e46e0-483">Expression tree types</span></span>

<span data-ttu-id="e46e0-484">***表达式树***允许 lambda 表达式，表示为数据结构，而不是可执行代码。</span><span class="sxs-lookup"><span data-stu-id="e46e0-484">***Expression trees*** permit lambda expressions to be represented as data structures instead of executable code.</span></span> <span data-ttu-id="e46e0-485">表达式树是值***表达式树类型***窗体`System.Linq.Expressions.Expression<D>`，其中`D`是任何委托类型。</span><span class="sxs-lookup"><span data-stu-id="e46e0-485">Expression trees are values of ***expression tree types*** of the form `System.Linq.Expressions.Expression<D>`, where `D` is any delegate type.</span></span> <span data-ttu-id="e46e0-486">此规范的剩余时间内我们将把这些类型使用速记`Expression<D>`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-486">For the remainder of this specification we will refer to these types using the shorthand `Expression<D>`.</span></span>

<span data-ttu-id="e46e0-487">如果转换为委托类型从 lambda 表达式存在`D`，转换为表达式树类型也存在于`Expression<D>`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-487">If a conversion exists from a lambda expression to a delegate type `D`, a conversion also exists to the expression tree type `Expression<D>`.</span></span> <span data-ttu-id="e46e0-488">为委托类型的 lambda 表达式转换生成引用 lambda 表达式的可执行代码的委托，而转换为表达式树类型创建 lambda 表达式的表达式树表示形式。</span><span class="sxs-lookup"><span data-stu-id="e46e0-488">Whereas the conversion of a lambda expression to a delegate type generates a delegate that references executable code for the lambda expression, conversion to an expression tree type creates an expression tree representation of the lambda expression.</span></span>

<span data-ttu-id="e46e0-489">表达式树的 lambda 表达式的高效内存中数据表示形式，并使 lambda 表达式结构的透明和显式。</span><span class="sxs-lookup"><span data-stu-id="e46e0-489">Expression trees are efficient in-memory data representations of lambda expressions and make the structure of the lambda expression transparent and explicit.</span></span>

<span data-ttu-id="e46e0-490">就像是委托类型`D`，`Expression<D>`称其具有参数和返回类型，它们是相同的`D`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-490">Just like a delegate type `D`, `Expression<D>` is said to have parameter and return types, which are the same as those of `D`.</span></span>

<span data-ttu-id="e46e0-491">下面的示例表示 lambda 表达式作为可执行代码和表达式树。</span><span class="sxs-lookup"><span data-stu-id="e46e0-491">The following example represents a lambda expression both as executable code and as an expression tree.</span></span> <span data-ttu-id="e46e0-492">由于存在到转换`Func<int,int>`，转换也存在到`Expression<Func<int,int>>`:</span><span class="sxs-lookup"><span data-stu-id="e46e0-492">Because a conversion exists to `Func<int,int>`, a conversion also exists to `Expression<Func<int,int>>`:</span></span>

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

<span data-ttu-id="e46e0-493">遵循这些分配委托`del`引用返回的方法`x + 1`，并在表达式树`exp`引用描述表达式的数据结构`x => x + 1`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-493">Following these assignments, the delegate `del` references a method that returns `x + 1`, and the expression tree `exp` references a data structure that describes the expression `x => x + 1`.</span></span>

<span data-ttu-id="e46e0-494">泛型类型的精确定义`Expression<D>`构造表达式树 lambda 表达式转换为表达式目录树类型时的精确规则，以及将此规范的范围之内。</span><span class="sxs-lookup"><span data-stu-id="e46e0-494">The exact definition of the generic type `Expression<D>` as well as the precise rules for constructing an expression tree when a lambda expression is converted to an expression tree type, are both outside the scope of this specification.</span></span>

<span data-ttu-id="e46e0-495">以下两种情况非常重要明确：</span><span class="sxs-lookup"><span data-stu-id="e46e0-495">Two things are important to make explicit:</span></span>

*  <span data-ttu-id="e46e0-496">并非所有的 lambda 表达式可以转换为表达式树。</span><span class="sxs-lookup"><span data-stu-id="e46e0-496">Not all lambda expressions can be converted to expression trees.</span></span> <span data-ttu-id="e46e0-497">例如，不能表示具有语句体的 lambda 表达式和其中包含赋值表达式的 lambda 表达式。</span><span class="sxs-lookup"><span data-stu-id="e46e0-497">For instance, lambda expressions with statement bodies, and lambda expressions containing assignment expressions cannot be represented.</span></span> <span data-ttu-id="e46e0-498">在这些情况下，转换仍存在，但将在编译时失败。</span><span class="sxs-lookup"><span data-stu-id="e46e0-498">In these cases, a conversion still exists, but will fail at compile-time.</span></span> <span data-ttu-id="e46e0-499">中详细介绍了这些异常[匿名函数转换](conversions.md#anonymous-function-conversions)。</span><span class="sxs-lookup"><span data-stu-id="e46e0-499">These exceptions are detailed in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>
*   <span data-ttu-id="e46e0-500">`Expression<D>` 提供的实例方法`Compile`可生成类型的委托`D`:</span><span class="sxs-lookup"><span data-stu-id="e46e0-500">`Expression<D>` offers an instance method `Compile` which produces a delegate of type `D`:</span></span>

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    <span data-ttu-id="e46e0-501">调用此委托会导致要执行表达式树所表示的代码。</span><span class="sxs-lookup"><span data-stu-id="e46e0-501">Invoking this delegate causes the code represented by the expression tree to be executed.</span></span> <span data-ttu-id="e46e0-502">因此，给定上述定义后，del 和 del2 等效的并且在以下两个语句都将具有相同的效果：</span><span class="sxs-lookup"><span data-stu-id="e46e0-502">Thus, given the definitions above, del and del2 are equivalent, and the following two statements will have the same effect:</span></span>

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    <span data-ttu-id="e46e0-503">执行此代码后`i1`并`i2`都将拥有值`2`。</span><span class="sxs-lookup"><span data-stu-id="e46e0-503">After executing this code,  `i1` and `i2` will both have the value `2`.</span></span>

