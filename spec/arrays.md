# <a name="arrays"></a><span data-ttu-id="075c4-101">数组</span><span class="sxs-lookup"><span data-stu-id="075c4-101">Arrays</span></span>

<span data-ttu-id="075c4-102">数组是包含许多通过计算索引访问的变量的数据结构。</span><span class="sxs-lookup"><span data-stu-id="075c4-102">An array is a data structure that contains a number of variables which are accessed through computed indices.</span></span> <span data-ttu-id="075c4-103">一个数组，也称为数组的元素中包含的变量包括所有相同的类型，而这种类型称为数组的元素类型。</span><span class="sxs-lookup"><span data-stu-id="075c4-103">The variables contained in an array, also called the elements of the array, are all of the same type, and this type is called the element type of the array.</span></span>

<span data-ttu-id="075c4-104">一个数组，具有用于确定与每个数组元素相关联的索引数的排名。</span><span class="sxs-lookup"><span data-stu-id="075c4-104">An array has a rank which determines the number of indices associated with each array element.</span></span> <span data-ttu-id="075c4-105">数组的秩也称为数组的维数。</span><span class="sxs-lookup"><span data-stu-id="075c4-105">The rank of an array is also referred to as the dimensions of the array.</span></span> <span data-ttu-id="075c4-106">调用具有一个级别的数组***维数组***。</span><span class="sxs-lookup"><span data-stu-id="075c4-106">An array with a rank of one is called a ***single-dimensional array***.</span></span> <span data-ttu-id="075c4-107">数组大于一项称为排名***多维数组***。</span><span class="sxs-lookup"><span data-stu-id="075c4-107">An array with a rank greater than one is called a ***multi-dimensional array***.</span></span> <span data-ttu-id="075c4-108">特定大小的多维数组通常称为二维数组、 三维数组等。</span><span class="sxs-lookup"><span data-stu-id="075c4-108">Specific sized multi-dimensional arrays are often referred to as two-dimensional arrays, three-dimensional arrays, and so on.</span></span>

<span data-ttu-id="075c4-109">一个数组的每个维度具有一个关联的长度，它是大于或等于零的整数。</span><span class="sxs-lookup"><span data-stu-id="075c4-109">Each dimension of an array has an associated length which is an integral number greater than or equal to zero.</span></span> <span data-ttu-id="075c4-110">维的长度不是数组，该类型的一部分，但在运行时创建的数组类型的实例时而建立。</span><span class="sxs-lookup"><span data-stu-id="075c4-110">The dimension lengths are not part of the type of the array, but rather are established when an instance of the array type is created at run-time.</span></span> <span data-ttu-id="075c4-111">维度的长度决定了该维度的索引的有效范围：维度的长度`N`，索引的范围可以从`0`到`N - 1`非独占。</span><span class="sxs-lookup"><span data-stu-id="075c4-111">The length of a dimension determines the valid range of indices for that dimension: For a dimension of length `N`, indices can range from `0` to `N - 1` inclusive.</span></span> <span data-ttu-id="075c4-112">数组中元素的总数是数组中每个维度的长度的产品。</span><span class="sxs-lookup"><span data-stu-id="075c4-112">The total number of elements in an array is the product of the lengths of each dimension in the array.</span></span> <span data-ttu-id="075c4-113">如果一个或多个数组的维度的长度为零，则称该数组为空。</span><span class="sxs-lookup"><span data-stu-id="075c4-113">If one or more of the dimensions of an array have a length of zero, the array is said to be empty.</span></span>

<span data-ttu-id="075c4-114">数组的元素类型可以是任意类型（包括数组类型）。</span><span class="sxs-lookup"><span data-stu-id="075c4-114">The element type of an array can be any type, including an array type.</span></span>

## <a name="array-types"></a><span data-ttu-id="075c4-115">数组类型</span><span class="sxs-lookup"><span data-stu-id="075c4-115">Array types</span></span>

<span data-ttu-id="075c4-116">数组类型被写为*non_array_type*跟一个或多个*rank_specifier*s:</span><span class="sxs-lookup"><span data-stu-id="075c4-116">An array type is written as a *non_array_type* followed by one or more *rank_specifier*s:</span></span>

```antlr
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
```

<span data-ttu-id="075c4-117">一个*non_array_type*可以是任何*类型*，它是本身不*array_type*。</span><span class="sxs-lookup"><span data-stu-id="075c4-117">A *non_array_type* is any *type* that is not itself an *array_type*.</span></span>

<span data-ttu-id="075c4-118">数组类型的秩的给定的最左侧*rank_specifier*中*array_type*:一个*rank_specifier*指示数组为排名数加一的数组"`,`"的令牌*rank_specifier*。</span><span class="sxs-lookup"><span data-stu-id="075c4-118">The rank of an array type is given by the leftmost *rank_specifier* in the *array_type*: A *rank_specifier* indicates that the array is an array with a rank of one plus the number of "`,`" tokens in the *rank_specifier*.</span></span>

<span data-ttu-id="075c4-119">数组类型的元素类型是删除最左侧所得到的类型*rank_specifier*:</span><span class="sxs-lookup"><span data-stu-id="075c4-119">The element type of an array type is the type that results from deleting the leftmost *rank_specifier*:</span></span>

*  <span data-ttu-id="075c4-120">数组类型的窗体`T[R]`是带有秩的数组`R`和非数组元素类型`T`。</span><span class="sxs-lookup"><span data-stu-id="075c4-120">An array type of the form `T[R]` is an array with rank `R` and a non-array element type `T`.</span></span>
*  <span data-ttu-id="075c4-121">数组类型的窗体`T[R][R1]...[Rn]`是带有秩的数组`R`和元素类型`T[R1]...[Rn]`。</span><span class="sxs-lookup"><span data-stu-id="075c4-121">An array type of the form `T[R][R1]...[Rn]` is an array with rank `R` and an element type `T[R1]...[Rn]`.</span></span>

<span data-ttu-id="075c4-122">实际上， *rank_specifier*s 从左到右前的最后一个非数组元素类型读取。</span><span class="sxs-lookup"><span data-stu-id="075c4-122">In effect, the *rank_specifier*s are read from left to right before the final non-array element type.</span></span> <span data-ttu-id="075c4-123">类型`int[][,,][,]`是一个一维数组的三维数组的二维数组的元素`int`。</span><span class="sxs-lookup"><span data-stu-id="075c4-123">The type `int[][,,][,]` is a single-dimensional array of three-dimensional arrays of two-dimensional arrays of `int`.</span></span>

<span data-ttu-id="075c4-124">在运行时数组类型的值可以是`null`或对该数组类型的实例的引用。</span><span class="sxs-lookup"><span data-stu-id="075c4-124">At run-time, a value of an array type can be `null` or a reference to an instance of that array type.</span></span>

### <a name="the-systemarray-type"></a><span data-ttu-id="075c4-125">System.Array 类型</span><span class="sxs-lookup"><span data-stu-id="075c4-125">The System.Array type</span></span>

<span data-ttu-id="075c4-126">类型`System.Array`是所有数组类型的抽象基类型。</span><span class="sxs-lookup"><span data-stu-id="075c4-126">The type `System.Array` is the abstract base type of all array types.</span></span> <span data-ttu-id="075c4-127">隐式引用转换 ([隐式引用转换](conversions.md#implicit-reference-conversions)) 存在从任何数组类型设置为`System.Array`，并显式引用转换 ([显式引用转换](conversions.md#explicit-reference-conversions)) 存在从`System.Array`到任何数组类型。</span><span class="sxs-lookup"><span data-stu-id="075c4-127">An implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from any array type to `System.Array`, and an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from `System.Array` to any array type.</span></span> <span data-ttu-id="075c4-128">请注意，`System.Array`本身不是*array_type*。</span><span class="sxs-lookup"><span data-stu-id="075c4-128">Note that `System.Array` is not itself an *array_type*.</span></span> <span data-ttu-id="075c4-129">相反，它是*class_type*所有*array_type*s 派生。</span><span class="sxs-lookup"><span data-stu-id="075c4-129">Rather, it is a *class_type* from which all *array_type*s are derived.</span></span>

<span data-ttu-id="075c4-130">在运行时类型的值`System.Array`可以是`null`或对任何数组类型的实例的引用。</span><span class="sxs-lookup"><span data-stu-id="075c4-130">At run-time, a value of type `System.Array` can be `null` or a reference to an instance of any array type.</span></span>

### <a name="arrays-and-the-generic-ilist-interface"></a><span data-ttu-id="075c4-131">数组和泛型 IList 接口</span><span class="sxs-lookup"><span data-stu-id="075c4-131">Arrays and the generic IList interface</span></span>

<span data-ttu-id="075c4-132">一维数组`T[]`实现接口`System.Collections.Generic.IList<T>`(`IList<T>`简称) 及其基接口。</span><span class="sxs-lookup"><span data-stu-id="075c4-132">A one-dimensional array `T[]` implements the interface `System.Collections.Generic.IList<T>` (`IList<T>` for short) and its base interfaces.</span></span> <span data-ttu-id="075c4-133">相应地，没有隐式转换`T[]`到`IList<T>`及其基接口。</span><span class="sxs-lookup"><span data-stu-id="075c4-133">Accordingly, there is an implicit conversion from `T[]` to `IList<T>` and its base interfaces.</span></span> <span data-ttu-id="075c4-134">此外，如果没有从的隐式引用转换`S`到`T`然后`S[]`实现`IList<T>`，并且没有一种从隐式引用转换`S[]`到`IList<T>`及其基接口 （[隐式引用转换](conversions.md#implicit-reference-conversions))。</span><span class="sxs-lookup"><span data-stu-id="075c4-134">In addition, if there is an implicit reference conversion from `S` to `T` then `S[]` implements `IList<T>` and there is an implicit reference conversion from `S[]` to `IList<T>` and its base interfaces ([Implicit reference conversions](conversions.md#implicit-reference-conversions)).</span></span> <span data-ttu-id="075c4-135">如果没有显式引用转换从`S`到`T`则没有显式引用转换从`S[]`到`IList<T>`及其基接口 ([显式引用转换](conversions.md#explicit-reference-conversions)).</span><span class="sxs-lookup"><span data-stu-id="075c4-135">If there is an explicit reference conversion from `S` to `T` then there is an explicit reference conversion from `S[]` to `IList<T>` and its base interfaces ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span> <span data-ttu-id="075c4-136">例如：</span><span class="sxs-lookup"><span data-stu-id="075c4-136">For example:</span></span>
```csharp
using System.Collections.Generic;

class Test
{
    static void Main() {
        string[] sa = new string[5];
        object[] oa1 = new object[5];
        object[] oa2 = sa;

        IList<string> lst1 = sa;                    // Ok
        IList<string> lst2 = oa1;                   // Error, cast needed
        IList<object> lst3 = sa;                    // Ok
        IList<object> lst4 = oa1;                   // Ok

        IList<string> lst5 = (IList<string>)oa1;    // Exception
        IList<string> lst6 = (IList<string>)oa2;    // Ok
    }
}
```

<span data-ttu-id="075c4-137">赋值`lst2 = oa1`以来从转换将生成编译时错误`object[]`到`IList<string>`是不隐式的显式转换。</span><span class="sxs-lookup"><span data-stu-id="075c4-137">The assignment `lst2 = oa1` generates a compile-time error since the conversion from `object[]` to `IList<string>` is an explicit conversion, not implicit.</span></span> <span data-ttu-id="075c4-138">强制转换`(IList<string>)oa1`将导致在运行时自引发异常`oa1`引用`object[]`而不`string[]`。</span><span class="sxs-lookup"><span data-stu-id="075c4-138">The cast `(IList<string>)oa1` will cause an exception to be thrown at run-time since `oa1` references an `object[]` and not a `string[]`.</span></span> <span data-ttu-id="075c4-139">但是该强制转换`(IList<string>)oa2`不会因为引发异常`oa2`引用`string[]`。</span><span class="sxs-lookup"><span data-stu-id="075c4-139">However the cast `(IList<string>)oa2` will not cause an exception to be thrown since `oa2` references a `string[]`.</span></span>

<span data-ttu-id="075c4-140">只要没有从的隐式或显式引用转换`S[]`到`IList<T>`，此外，还有一种从显式引用转换`IList<T>`及其基接口到`S[]`([显式引用转换](conversions.md#explicit-reference-conversions))。</span><span class="sxs-lookup"><span data-stu-id="075c4-140">Whenever there is an implicit or explicit reference conversion from `S[]` to `IList<T>`, there is also an explicit reference conversion from `IList<T>` and its base interfaces to `S[]` ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span>

<span data-ttu-id="075c4-141">当数组类型`S[]`实现`IList<T>`，某些实现的接口的成员可能会引发异常。</span><span class="sxs-lookup"><span data-stu-id="075c4-141">When an array type `S[]` implements `IList<T>`, some of the members of the implemented interface may throw exceptions.</span></span> <span data-ttu-id="075c4-142">接口的实现的确切行为已超出本规范的范畴。</span><span class="sxs-lookup"><span data-stu-id="075c4-142">The precise behavior of the implementation of the interface is beyond the scope of this specification.</span></span>

## <a name="array-creation"></a><span data-ttu-id="075c4-143">数组创建</span><span class="sxs-lookup"><span data-stu-id="075c4-143">Array creation</span></span>

<span data-ttu-id="075c4-144">通过创建数组实例*array_creation_expression*s ([数组创建表达式](expressions.md#array-creation-expressions)) 或由字段或局部变量声明包含*array_initializer*([数组初始值设定项](arrays.md#array-initializers))。</span><span class="sxs-lookup"><span data-stu-id="075c4-144">Array instances are created by *array_creation_expression*s ([Array creation expressions](expressions.md#array-creation-expressions)) or by field or local variable declarations that include an *array_initializer* ([Array initializers](arrays.md#array-initializers)).</span></span>

<span data-ttu-id="075c4-145">创建数组实例后，级别和每个维的长度建立和实例的整个生存期内保持不变。</span><span class="sxs-lookup"><span data-stu-id="075c4-145">When an array instance is created, the rank and length of each dimension are established and then remain constant for the entire lifetime of the instance.</span></span> <span data-ttu-id="075c4-146">换而言之，不能更改现有数组实例，秩也不可能以调整其尺寸。</span><span class="sxs-lookup"><span data-stu-id="075c4-146">In other words, it is not possible to change the rank of an existing array instance, nor is it possible to resize its dimensions.</span></span>

<span data-ttu-id="075c4-147">数组实例始终是数组类型。</span><span class="sxs-lookup"><span data-stu-id="075c4-147">An array instance is always of an array type.</span></span> <span data-ttu-id="075c4-148">`System.Array`类型为抽象类型，不能实例化。</span><span class="sxs-lookup"><span data-stu-id="075c4-148">The `System.Array` type is an abstract type that cannot be instantiated.</span></span>

<span data-ttu-id="075c4-149">创建的数组的元素*array_creation_expression*s 始终将初始化为其默认值 ([默认值](variables.md#default-values))。</span><span class="sxs-lookup"><span data-stu-id="075c4-149">Elements of arrays created by *array_creation_expression*s are always initialized to their default value ([Default values](variables.md#default-values)).</span></span>

## <a name="array-element-access"></a><span data-ttu-id="075c4-150">数组元素访问</span><span class="sxs-lookup"><span data-stu-id="075c4-150">Array element access</span></span>

<span data-ttu-id="075c4-151">使用访问数组元素*element_access*表达式 ([数组访问](expressions.md#array-access)) 的窗体`A[I1, I2, ..., In]`，其中`A`的数组类型和每个表达式`Ix`是类型的表达式`int`， `uint`， `long`， `ulong`，或可以隐式转换为一个或多个这些类型。</span><span class="sxs-lookup"><span data-stu-id="075c4-151">Array elements are accessed using *element_access* expressions ([Array access](expressions.md#array-access)) of the form `A[I1, I2, ..., In]`, where `A` is an expression of an array type and each `Ix` is an expression of type `int`, `uint`, `long`, `ulong`, or can be implicitly converted to one or more of these types.</span></span> <span data-ttu-id="075c4-152">数组元素访问的结果是一个变量，即所选的索引的数组元素。</span><span class="sxs-lookup"><span data-stu-id="075c4-152">The result of an array element access is a variable, namely the array element selected by the indices.</span></span>

<span data-ttu-id="075c4-153">可以使用枚举数组的元素`foreach`语句 ([foreach 语句](statements.md#the-foreach-statement))。</span><span class="sxs-lookup"><span data-stu-id="075c4-153">The elements of an array can be enumerated using a `foreach` statement ([The foreach statement](statements.md#the-foreach-statement)).</span></span>

## <a name="array-members"></a><span data-ttu-id="075c4-154">数组成员</span><span class="sxs-lookup"><span data-stu-id="075c4-154">Array members</span></span>

<span data-ttu-id="075c4-155">每个数组类型继承的成员声明的`System.Array`类型。</span><span class="sxs-lookup"><span data-stu-id="075c4-155">Every array type inherits the members declared by the `System.Array` type.</span></span>

## <a name="array-covariance"></a><span data-ttu-id="075c4-156">数组协方差</span><span class="sxs-lookup"><span data-stu-id="075c4-156">Array covariance</span></span>

<span data-ttu-id="075c4-157">对于任何两个*reference_type*s`A`并`B`，则隐式引用转换 ([隐式引用转换](conversions.md#implicit-reference-conversions)) 或显式引用转换 ([显式引用转换](conversions.md#explicit-reference-conversions)) 中存在`A`到`B`，然后从数组类型也存在相同的引用转换`A[R]`为数组类型`B[R]`，其中`R`可以是任何给定*rank_specifier* （但相同数组类型）。</span><span class="sxs-lookup"><span data-stu-id="075c4-157">For any two *reference_type*s `A` and `B`, if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) or explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from `A` to `B`, then the same reference conversion also exists from the array type `A[R]` to the array type `B[R]`, where `R` is any given *rank_specifier* (but the same for both array types).</span></span> <span data-ttu-id="075c4-158">这种关系称为***数组协方差***。</span><span class="sxs-lookup"><span data-stu-id="075c4-158">This relationship is known as ***array covariance***.</span></span> <span data-ttu-id="075c4-159">数组协方差尤其意味着，数组类型的值`A[R]`实际上可能是对的数组类型实例的引用`B[R]`，则从存在的隐式引用转换`B`到`A`。</span><span class="sxs-lookup"><span data-stu-id="075c4-159">Array covariance in particular means that a value of an array type `A[R]` may actually be a reference to an instance of an array type `B[R]`, provided an implicit reference conversion exists from `B` to `A`.</span></span>

<span data-ttu-id="075c4-160">数组协方差，由于引用类型数组的元素赋值中包括一个运行时检查，以确保分配给数组元素的值确实是允许的类型 ([简单的赋值](expressions.md#simple-assignment))。</span><span class="sxs-lookup"><span data-stu-id="075c4-160">Because of array covariance, assignments to elements of reference type arrays include a run-time check which ensures that the value being assigned to the array element is actually of a permitted type ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="075c4-161">例如：</span><span class="sxs-lookup"><span data-stu-id="075c4-161">For example:</span></span>
```csharp
class Test
{
    static void Fill(object[] array, int index, int count, object value) {
        for (int i = index; i < index + count; i++) array[i] = value;
    }

    static void Main() {
        string[] strings = new string[100];
        Fill(strings, 0, 100, "Undefined");
        Fill(strings, 0, 10, null);
        Fill(strings, 90, 10, 0);
    }
}
```

<span data-ttu-id="075c4-162">分配给`array[i]`中`Fill`方法隐式包含一个运行时检查，以所引用的对象可确保`value`是`null`的实际元素类型与兼容实例或`array`.</span><span class="sxs-lookup"><span data-stu-id="075c4-162">The assignment to `array[i]` in the `Fill` method implicitly includes a run-time check which ensures that the object referenced by `value` is either `null` or an instance that is compatible with the actual element type of `array`.</span></span> <span data-ttu-id="075c4-163">在中`Main`的前两个调用`Fill`成功，但第三个调用会导致`System.ArrayTypeMismatchException`引发时执行的第一个分配到`array[i]`。</span><span class="sxs-lookup"><span data-stu-id="075c4-163">In `Main`, the first two invocations of `Fill` succeed, but the third invocation causes a `System.ArrayTypeMismatchException` to be thrown upon executing the first assignment to `array[i]`.</span></span> <span data-ttu-id="075c4-164">发生异常的一个装箱`int`不能存储在`string`数组。</span><span class="sxs-lookup"><span data-stu-id="075c4-164">The exception occurs because a boxed `int` cannot be stored in a `string` array.</span></span>

<span data-ttu-id="075c4-165">数组协方差专门未扩展到的数组*value_type*s。</span><span class="sxs-lookup"><span data-stu-id="075c4-165">Array covariance specifically does not extend to arrays of *value_type*s.</span></span> <span data-ttu-id="075c4-166">例如，不存在转换召开`int[]`作为要视作`object[]`。</span><span class="sxs-lookup"><span data-stu-id="075c4-166">For example, no conversion exists that permits an `int[]` to be treated as an `object[]`.</span></span>

## <a name="array-initializers"></a><span data-ttu-id="075c4-167">数组初始值设定项</span><span class="sxs-lookup"><span data-stu-id="075c4-167">Array initializers</span></span>

<span data-ttu-id="075c4-168">可能在字段声明中指定数组初始值设定项 ([字段](classes.md#fields))，本地变量声明 ([局部变量声明](statements.md#local-variable-declarations))，和数组创建表达式 ([数组创建表达式](expressions.md#array-creation-expressions)):</span><span class="sxs-lookup"><span data-stu-id="075c4-168">Array initializers may be specified in field declarations ([Fields](classes.md#fields)), local variable declarations ([Local variable declarations](statements.md#local-variable-declarations)), and array creation expressions ([Array creation expressions](expressions.md#array-creation-expressions)):</span></span>

```antlr
array_initializer
    : '{' variable_initializer_list? '}'
    | '{' variable_initializer_list ',' '}'
    ;

variable_initializer_list
    : variable_initializer (',' variable_initializer)*
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

<span data-ttu-id="075c4-169">数组初始值设定项包含变量的初始值设定项，括在一系列"`{`"和"`}`"标记和分隔"`,`"令牌。</span><span class="sxs-lookup"><span data-stu-id="075c4-169">An array initializer consists of a sequence of variable initializers, enclosed by "`{`" and "`}`" tokens and separated by "`,`" tokens.</span></span> <span data-ttu-id="075c4-170">每个变量的初始值设定项是一个表达式或对于多维数组，嵌套的数组初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="075c4-170">Each variable initializer is an expression or, in the case of a multi-dimensional array, a nested array initializer.</span></span>

<span data-ttu-id="075c4-171">在其中使用数组初始值设定项的上下文确定正在初始化数组的类型。</span><span class="sxs-lookup"><span data-stu-id="075c4-171">The context in which an array initializer is used determines the type of the array being initialized.</span></span> <span data-ttu-id="075c4-172">在数组创建表达式中，数组类型立即之前初始值设定项，或从数组初始值设定项中的表达式推断。</span><span class="sxs-lookup"><span data-stu-id="075c4-172">In an array creation expression, the array type immediately precedes the initializer, or is inferred from the expressions in the array initializer.</span></span> <span data-ttu-id="075c4-173">在字段或变量声明中，数组类型是字段或声明变量的类型。</span><span class="sxs-lookup"><span data-stu-id="075c4-173">In a field or variable declaration, the array type is the type of the field or variable being declared.</span></span> <span data-ttu-id="075c4-174">数组初始值设定项中使用时的字段或变量声明，如：</span><span class="sxs-lookup"><span data-stu-id="075c4-174">When an array initializer is used in a field or variable declaration, such as:</span></span>
```csharp
int[] a = {0, 2, 4, 6, 8};
```
<span data-ttu-id="075c4-175">它是简写为等效的数组创建表达式：</span><span class="sxs-lookup"><span data-stu-id="075c4-175">it is simply shorthand for an equivalent array creation expression:</span></span>
```csharp
int[] a = new int[] {0, 2, 4, 6, 8};
```

<span data-ttu-id="075c4-176">对于一维数组，数组初始值设定项必须包含的一系列数组的元素类型与赋值兼容的表达式。</span><span class="sxs-lookup"><span data-stu-id="075c4-176">For a single-dimensional array, the array initializer must consist of a sequence of expressions that are assignment compatible with the element type of the array.</span></span> <span data-ttu-id="075c4-177">表达式初始化按升序排列，从索引零开始的元素开始的数组元素。</span><span class="sxs-lookup"><span data-stu-id="075c4-177">The expressions initialize array elements in increasing order, starting with the element at index zero.</span></span> <span data-ttu-id="075c4-178">数组初始值设定项中的表达式数决定要创建的数组实例的长度。</span><span class="sxs-lookup"><span data-stu-id="075c4-178">The number of expressions in the array initializer determines the length of the array instance being created.</span></span> <span data-ttu-id="075c4-179">例如，上面的数组初始值设定项创建`int[]`长度为 5 的实例并初始化该实例使用以下值：</span><span class="sxs-lookup"><span data-stu-id="075c4-179">For example, the array initializer above creates an `int[]` instance of length 5 and then initializes the instance with the following values:</span></span>
```csharp
a[0] = 0; a[1] = 2; a[2] = 4; a[3] = 6; a[4] = 8;
```

<span data-ttu-id="075c4-180">对于多维数组，数组初始值设定项必须尽可能多的嵌套数组中没有维度级别。</span><span class="sxs-lookup"><span data-stu-id="075c4-180">For a multi-dimensional array, the array initializer must have as many levels of nesting as there are dimensions in the array.</span></span> <span data-ttu-id="075c4-181">最外层嵌套级别对应于最左边的维度，最内部的嵌套级别对应于最右边的维度。</span><span class="sxs-lookup"><span data-stu-id="075c4-181">The outermost nesting level corresponds to the leftmost dimension and the innermost nesting level corresponds to the rightmost dimension.</span></span> <span data-ttu-id="075c4-182">数组的每个维的长度确定数组初始值设定项中相应嵌套级别的元素数。</span><span class="sxs-lookup"><span data-stu-id="075c4-182">The length of each dimension of the array is determined by the number of elements at the corresponding nesting level in the array initializer.</span></span> <span data-ttu-id="075c4-183">对于每个嵌套的数组初始值设定项中，元素的数目必须与同一级别的其他数组初始值设定项相同。</span><span class="sxs-lookup"><span data-stu-id="075c4-183">For each nested array initializer, the number of elements must be the same as the other array initializers at the same level.</span></span> <span data-ttu-id="075c4-184">下面的示例：</span><span class="sxs-lookup"><span data-stu-id="075c4-184">The example:</span></span>
```csharp
int[,] b = {{0, 1}, {2, 3}, {4, 5}, {6, 7}, {8, 9}};
```
<span data-ttu-id="075c4-185">创建一个二维数组的长度为 5 为最左侧的维度的两个最右边的维度的长度：</span><span class="sxs-lookup"><span data-stu-id="075c4-185">creates a two-dimensional array with a length of five for the leftmost dimension and a length of two for the rightmost dimension:</span></span>
```csharp
int[,] b = new int[5, 2];
```
<span data-ttu-id="075c4-186">然后初始化该数组实例设置以下值：</span><span class="sxs-lookup"><span data-stu-id="075c4-186">and then initializes the array instance with the following values:</span></span>
```csharp
b[0, 0] = 0; b[0, 1] = 1;
b[1, 0] = 2; b[1, 1] = 3;
b[2, 0] = 4; b[2, 1] = 5;
b[3, 0] = 6; b[3, 1] = 7;
b[4, 0] = 8; b[4, 1] = 9;
```

<span data-ttu-id="075c4-187">如果之外最右边的维度的长度为零，则假定后续维度还具有长度为零。</span><span class="sxs-lookup"><span data-stu-id="075c4-187">If a dimension other than the rightmost is given with length zero, the subsequent dimensions are assumed to also have length zero.</span></span> <span data-ttu-id="075c4-188">下面的示例：</span><span class="sxs-lookup"><span data-stu-id="075c4-188">The example:</span></span>
```csharp
int[,] c = {};
```
<span data-ttu-id="075c4-189">使用长度为零的最左边和右边的维度中创建一个二维数组：</span><span class="sxs-lookup"><span data-stu-id="075c4-189">creates a two-dimensional array with a length of zero for both the leftmost and the rightmost dimension:</span></span>
```csharp
int[,] c = new int[0, 0];
```

<span data-ttu-id="075c4-190">当数组创建表达式中包含显式维的长度和数组初始值设定项时，长度必须是常量表达式，并在每个嵌套级别的元素的数目必须与相应的维度长度。</span><span class="sxs-lookup"><span data-stu-id="075c4-190">When an array creation expression includes both explicit dimension lengths and an array initializer, the lengths must be constant expressions and the number of elements at each nesting level must match the corresponding dimension length.</span></span> <span data-ttu-id="075c4-191">下面是一些可能的恶意活动：</span><span class="sxs-lookup"><span data-stu-id="075c4-191">Here are some examples:</span></span>
```csharp
int i = 3;
int[] x = new int[3] {0, 1, 2};        // OK
int[] y = new int[i] {0, 1, 2};        // Error, i not a constant
int[] z = new int[3] {0, 1, 2, 3};     // Error, length/initializer mismatch
```

<span data-ttu-id="075c4-192">此处的初始值设定项`y`会导致编译时错误，因为维度长度的表达式不是常量和的初始值设定项`z`导致编译时错误，因为长度和中的元素数初始值设定项不一致。</span><span class="sxs-lookup"><span data-stu-id="075c4-192">Here, the initializer for `y` results in a compile-time error because the dimension length expression is not a constant, and the initializer for `z` results in a compile-time error because the length and the number of elements in the initializer do not agree.</span></span>
