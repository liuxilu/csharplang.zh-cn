---
ms.openlocfilehash: 61eeae6173eaa19f9cf6d6e985f3dc107d4c3ac9
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488962"
---
# <a name="conversions"></a><span data-ttu-id="5c202-101">转换</span><span class="sxs-lookup"><span data-stu-id="5c202-101">Conversions</span></span>

<span data-ttu-id="5c202-102">一个***转换***使表达式能够被视为是指特定类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-102">A ***conversion*** enables an expression to be treated as being of a particular type.</span></span> <span data-ttu-id="5c202-103">转换可能会导致要被视为具有不同类型的给定类型的表达式或它可能会导致没有要获得一种类型的类型的表达式。</span><span class="sxs-lookup"><span data-stu-id="5c202-103">A conversion may cause an expression of a given type to be treated as having a different type, or it may cause an expression without a type to get a type.</span></span> <span data-ttu-id="5c202-104">转换可以是***隐式***或***显式***，这将确定是否需要显式强制转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-104">Conversions can be ***implicit*** or ***explicit***, and this determines whether an explicit cast is required.</span></span> <span data-ttu-id="5c202-105">例如，从类型转换`int`键入`long`是隐式的因此表达式的类型`int`隐式视为类型`long`。</span><span class="sxs-lookup"><span data-stu-id="5c202-105">For instance, the conversion from type `int` to type `long` is implicit, so expressions of type `int` can implicitly be treated as type `long`.</span></span> <span data-ttu-id="5c202-106">从类型相反转换`long`键入`int`，是显式的因此显式强制转换为所需。</span><span class="sxs-lookup"><span data-stu-id="5c202-106">The opposite conversion, from type `long` to type `int`, is explicit and so an explicit cast is required.</span></span>

```csharp
int a = 123;
long b = a;         // implicit conversion from int to long
int c = (int) b;    // explicit conversion from long to int
```

<span data-ttu-id="5c202-107">某些转换是由语言定义的。</span><span class="sxs-lookup"><span data-stu-id="5c202-107">Some conversions are defined by the language.</span></span> <span data-ttu-id="5c202-108">程序也可以定义其自己的转换 ([用户定义的转换](conversions.md#user-defined-conversions))。</span><span class="sxs-lookup"><span data-stu-id="5c202-108">Programs may also define their own conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

## <a name="implicit-conversions"></a><span data-ttu-id="5c202-109">隐式转换</span><span class="sxs-lookup"><span data-stu-id="5c202-109">Implicit conversions</span></span>

<span data-ttu-id="5c202-110">以下转换被归类为隐式转换：</span><span class="sxs-lookup"><span data-stu-id="5c202-110">The following conversions are classified as implicit conversions:</span></span>

*  <span data-ttu-id="5c202-111">标识转换</span><span class="sxs-lookup"><span data-stu-id="5c202-111">Identity conversions</span></span>
*  <span data-ttu-id="5c202-112">隐式数值转换</span><span class="sxs-lookup"><span data-stu-id="5c202-112">Implicit numeric conversions</span></span>
*  <span data-ttu-id="5c202-113">枚举隐式转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-113">Implicit enumeration conversions.</span></span>
*  <span data-ttu-id="5c202-114">可以为 null 的隐式转换</span><span class="sxs-lookup"><span data-stu-id="5c202-114">Implicit nullable conversions</span></span>
*  <span data-ttu-id="5c202-115">Null 文本转换</span><span class="sxs-lookup"><span data-stu-id="5c202-115">Null literal conversions</span></span>
*  <span data-ttu-id="5c202-116">隐式引用转换</span><span class="sxs-lookup"><span data-stu-id="5c202-116">Implicit reference conversions</span></span>
*  <span data-ttu-id="5c202-117">装箱转换</span><span class="sxs-lookup"><span data-stu-id="5c202-117">Boxing conversions</span></span>
*  <span data-ttu-id="5c202-118">隐式动态转换</span><span class="sxs-lookup"><span data-stu-id="5c202-118">Implicit dynamic conversions</span></span>
*  <span data-ttu-id="5c202-119">常量表达式隐式转换</span><span class="sxs-lookup"><span data-stu-id="5c202-119">Implicit constant expression conversions</span></span>
*  <span data-ttu-id="5c202-120">用户定义的隐式转换</span><span class="sxs-lookup"><span data-stu-id="5c202-120">User-defined implicit conversions</span></span>
*  <span data-ttu-id="5c202-121">匿名函数转换</span><span class="sxs-lookup"><span data-stu-id="5c202-121">Anonymous function conversions</span></span>
*  <span data-ttu-id="5c202-122">方法组转换</span><span class="sxs-lookup"><span data-stu-id="5c202-122">Method group conversions</span></span>

<span data-ttu-id="5c202-123">隐式转换可能在很多情况下，其中包括函数成员调用 ([进行编译时检查的动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution))，强制转换表达式 ([强制转换表达式](expressions.md#cast-expressions))，和分配 ([赋值运算符](expressions.md#assignment-operators))。</span><span class="sxs-lookup"><span data-stu-id="5c202-123">Implicit conversions can occur in a variety of situations, including function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), cast expressions ([Cast expressions](expressions.md#cast-expressions)), and assignments ([Assignment operators](expressions.md#assignment-operators)).</span></span>

<span data-ttu-id="5c202-124">预定义隐式转换始终成功并永远不会导致引发异常。</span><span class="sxs-lookup"><span data-stu-id="5c202-124">The pre-defined implicit conversions always succeed and never cause exceptions to be thrown.</span></span> <span data-ttu-id="5c202-125">正确设计的用户定义的隐式转换应表现出这些特征也。</span><span class="sxs-lookup"><span data-stu-id="5c202-125">Properly designed user-defined implicit conversions should exhibit these characteristics as well.</span></span>

<span data-ttu-id="5c202-126">用于转换，类型`object`和`dynamic`被视为等效。</span><span class="sxs-lookup"><span data-stu-id="5c202-126">For the purposes of conversion, the types `object` and `dynamic` are considered equivalent.</span></span>

<span data-ttu-id="5c202-127">但是，动态转换 ([隐式动态转换](conversions.md#implicit-dynamic-conversions)并[显式动态转换](conversions.md#explicit-dynamic-conversions)) 仅适用于类型的表达式`dynamic`([动态类型](types.md#the-dynamic-type)).</span><span class="sxs-lookup"><span data-stu-id="5c202-127">However, dynamic conversions ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions) and [Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions)) apply only to expressions of type `dynamic` ([The dynamic type](types.md#the-dynamic-type)).</span></span>

### <a name="identity-conversion"></a><span data-ttu-id="5c202-128">标识转换</span><span class="sxs-lookup"><span data-stu-id="5c202-128">Identity conversion</span></span>

<span data-ttu-id="5c202-129">标识转换将从任何类型转换为同一类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-129">An identity conversion converts from any type to the same type.</span></span> <span data-ttu-id="5c202-130">这样可以认为已具有所需的类型的实体可以转换为该类型已存在此转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-130">This conversion exists such that an entity that already has a required type can be said to be convertible to that type.</span></span>

*  <span data-ttu-id="5c202-131">因为对象和动态被视为等效之间不存在标识转换`object`并`dynamic`，并替换的所有匹配项时都是相同的构造类型之间`dynamic`与`object`。</span><span class="sxs-lookup"><span data-stu-id="5c202-131">Because object and dynamic are considered equivalent there is an identity conversion between `object` and `dynamic`, and between constructed types that are the same when replacing all occurrences of `dynamic` with `object`.</span></span>

### <a name="implicit-numeric-conversions"></a><span data-ttu-id="5c202-132">隐式数值转换</span><span class="sxs-lookup"><span data-stu-id="5c202-132">Implicit numeric conversions</span></span>

<span data-ttu-id="5c202-133">隐式数值转换为：</span><span class="sxs-lookup"><span data-stu-id="5c202-133">The implicit numeric conversions are:</span></span>

*  <span data-ttu-id="5c202-134">从`sbyte`到`short`， `int`， `long`， `float`， `double`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="5c202-134">From `sbyte` to `short`, `int`, `long`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="5c202-135">从`byte`到`short`， `ushort`， `int`， `uint`， `long`， `ulong`， `float`， `double`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="5c202-135">From `byte` to `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="5c202-136">从`short`到`int`， `long`， `float`， `double`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="5c202-136">From `short` to `int`, `long`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="5c202-137">从`ushort`到`int`， `uint`， `long`， `ulong`， `float`， `double`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="5c202-137">From `ushort` to `int`, `uint`, `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="5c202-138">从`int`到`long`， `float`， `double`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="5c202-138">From `int` to `long`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="5c202-139">从`uint`到`long`， `ulong`， `float`， `double`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="5c202-139">From `uint` to `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="5c202-140">从`long`到`float`， `double`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="5c202-140">From `long` to `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="5c202-141">从`ulong`到`float`， `double`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="5c202-141">From `ulong` to `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="5c202-142">从`char`到`ushort`， `int`， `uint`， `long`， `ulong`， `float`， `double`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="5c202-142">From `char` to `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="5c202-143">从`float`到`double`。</span><span class="sxs-lookup"><span data-stu-id="5c202-143">From `float` to `double`.</span></span>

<span data-ttu-id="5c202-144">从转换`int`， `uint`， `long`，或`ulong`到`float`来回`long`或`ulong`到`double`可能会导致丢失精度，但将永远不会导致丢失量值。</span><span class="sxs-lookup"><span data-stu-id="5c202-144">Conversions from `int`, `uint`, `long`, or `ulong` to `float` and from `long` or `ulong` to `double` may cause a loss of precision, but will never cause a loss of magnitude.</span></span> <span data-ttu-id="5c202-145">其他隐式数值转换永远不会丢失任何信息。</span><span class="sxs-lookup"><span data-stu-id="5c202-145">The other implicit numeric conversions never lose any information.</span></span>

<span data-ttu-id="5c202-146">不存在隐式转换到`char`类型，因此其他整数类型的值没有自动转换为`char`类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-146">There are no implicit conversions to the `char` type, so values of the other integral types do not automatically convert to the `char` type.</span></span>

### <a name="implicit-enumeration-conversions"></a><span data-ttu-id="5c202-147">枚举隐式转换</span><span class="sxs-lookup"><span data-stu-id="5c202-147">Implicit enumeration conversions</span></span>

<span data-ttu-id="5c202-148">隐式枚举转换允许*decimal_integer_literal* `0`可转换为任何*enum_type*和任何*nullable_type*其基础类型是*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="5c202-148">An implicit enumeration conversion permits the *decimal_integer_literal* `0` to be converted to any *enum_type* and to any *nullable_type* whose underlying type is an *enum_type*.</span></span> <span data-ttu-id="5c202-149">在后一种情况中，转换计算通过将转换为基础*enum_type*包装结果和 ([为 Null 的类型](types.md#nullable-types))。</span><span class="sxs-lookup"><span data-stu-id="5c202-149">In the latter case the conversion is evaluated by converting to the underlying *enum_type* and wrapping the result ([Nullable types](types.md#nullable-types)).</span></span>

### <a name="implicit-interpolated-string-conversions"></a><span data-ttu-id="5c202-150">隐式的内插的字符串转换</span><span class="sxs-lookup"><span data-stu-id="5c202-150">Implicit interpolated string conversions</span></span>

<span data-ttu-id="5c202-151">隐式的内插字符串转换允许*interpolated_string_expression* ([内插字符串](expressions.md#interpolated-strings)) 转换为`System.IFormattable`或`System.FormattableString`（它可实现`System.IFormattable`).</span><span class="sxs-lookup"><span data-stu-id="5c202-151">An implicit interpolated string conversion permits an *interpolated_string_expression* ([Interpolated strings](expressions.md#interpolated-strings)) to be converted to `System.IFormattable` or `System.FormattableString` (which implements `System.IFormattable`).</span></span>

<span data-ttu-id="5c202-152">应用此转换时不的内插字符串组成的字符串值。</span><span class="sxs-lookup"><span data-stu-id="5c202-152">When this conversion is applied a string value is not composed from the interpolated string.</span></span> <span data-ttu-id="5c202-153">改为实例`System.FormattableString`创建，为进一步中所述[内插字符串](expressions.md#interpolated-strings)。</span><span class="sxs-lookup"><span data-stu-id="5c202-153">Instead an instance of `System.FormattableString` is created, as further described in [Interpolated strings](expressions.md#interpolated-strings).</span></span>

### <a name="implicit-nullable-conversions"></a><span data-ttu-id="5c202-154">可以为 null 的隐式转换</span><span class="sxs-lookup"><span data-stu-id="5c202-154">Implicit nullable conversions</span></span>

<span data-ttu-id="5c202-155">此外可以为这些类型的 null 的形式使用不可为 null 的值类型运行的预定义隐式转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-155">Predefined implicit conversions that operate on non-nullable value types can also be used with nullable forms of those types.</span></span> <span data-ttu-id="5c202-156">为每个预定义隐式标识和不可以为 null 的值类型转换的数值转换`S`为非 null 的值类型`T`，存在以下隐式可以为 null 的转换：</span><span class="sxs-lookup"><span data-stu-id="5c202-156">For each of the predefined implicit identity and numeric conversions that convert from a non-nullable value type `S` to a non-nullable value type `T`, the following implicit nullable conversions exist:</span></span>

*  <span data-ttu-id="5c202-157">隐式转换`S?`到`T?`。</span><span class="sxs-lookup"><span data-stu-id="5c202-157">An implicit conversion from `S?` to `T?`.</span></span>
*  <span data-ttu-id="5c202-158">隐式转换`S`到`T?`。</span><span class="sxs-lookup"><span data-stu-id="5c202-158">An implicit conversion from `S` to `T?`.</span></span>

<span data-ttu-id="5c202-159">可以为 null 的隐式转换的计算基于一种从基础转换`S`到`T`继续，如下所示：</span><span class="sxs-lookup"><span data-stu-id="5c202-159">Evaluation of an implicit nullable conversion based on an underlying conversion from `S` to `T` proceeds as follows:</span></span>

*  <span data-ttu-id="5c202-160">如果可以为 null 的转换是从`S?`到`T?`:</span><span class="sxs-lookup"><span data-stu-id="5c202-160">If the nullable conversion is from `S?` to `T?`:</span></span>
    * <span data-ttu-id="5c202-161">如果源值为 null (`HasValue`属性为 false)，结果是类型的 null 值`T?`。</span><span class="sxs-lookup"><span data-stu-id="5c202-161">If the source value is null (`HasValue` property is false), the result is the null value of type `T?`.</span></span>
    * <span data-ttu-id="5c202-162">否则，则转换计算过程为从`S?`到`S`后, 跟从基础转换`S`到`T`后, 跟一个包装 ([为 Null 的类型](types.md#nullable-types)) 从`T`到`T?`。</span><span class="sxs-lookup"><span data-stu-id="5c202-162">Otherwise, the conversion is evaluated as an unwrapping from `S?` to `S`, followed by the underlying conversion from `S` to `T`, followed by a wrapping ([Nullable types](types.md#nullable-types)) from `T` to `T?`.</span></span>

*  <span data-ttu-id="5c202-163">如果可以为 null 的转换是从`S`到`T?`，转换计算为从基础转换`S`到`T`跟从一个包装`T`到`T?`。</span><span class="sxs-lookup"><span data-stu-id="5c202-163">If the nullable conversion is from `S` to `T?`, the conversion is evaluated as the underlying conversion from `S` to `T` followed by a wrapping from `T` to `T?`.</span></span>

### <a name="null-literal-conversions"></a><span data-ttu-id="5c202-164">Null 文本转换</span><span class="sxs-lookup"><span data-stu-id="5c202-164">Null literal conversions</span></span>

<span data-ttu-id="5c202-165">隐式转换`null`文字转换为 null 的类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-165">An implicit conversion exists from the `null` literal to any nullable type.</span></span> <span data-ttu-id="5c202-166">这种转换产生 null 值 ([为 Null 的类型](types.md#nullable-types)) 的给定为 null 的类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-166">This conversion produces the null value ([Nullable types](types.md#nullable-types)) of the given nullable type.</span></span>

### <a name="implicit-reference-conversions"></a><span data-ttu-id="5c202-167">隐式引用转换</span><span class="sxs-lookup"><span data-stu-id="5c202-167">Implicit reference conversions</span></span>

<span data-ttu-id="5c202-168">隐式引用转换为：</span><span class="sxs-lookup"><span data-stu-id="5c202-168">The implicit reference conversions are:</span></span>

*  <span data-ttu-id="5c202-169">从任何*reference_type*到`object`和`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="5c202-169">From any *reference_type* to `object` and `dynamic`.</span></span>
*  <span data-ttu-id="5c202-170">从任何*class_type* `S`任何*class_type* `T`提供`S`派生自`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-170">From any *class_type* `S` to any *class_type* `T`, provided `S` is derived from `T`.</span></span>
*  <span data-ttu-id="5c202-171">从任何*class_type* `S`任何*interface_type* `T`提供`S`实现`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-171">From any *class_type* `S` to any *interface_type* `T`, provided `S` implements `T`.</span></span>
*  <span data-ttu-id="5c202-172">从任何*interface_type* `S`任何*interface_type* `T`提供`S`派生自`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-172">From any *interface_type* `S` to any *interface_type* `T`, provided `S` is derived from `T`.</span></span>
*  <span data-ttu-id="5c202-173">从*array_type* `S`具有元素类型`SE`到*array_type* `T`具有元素类型`TE`，提供以下所有项都为 true:</span><span class="sxs-lookup"><span data-stu-id="5c202-173">From an *array_type* `S` with an element type `SE` to an *array_type* `T` with an element type `TE`, provided all of the following are true:</span></span>
    * <span data-ttu-id="5c202-174">`S` 和`T`的区别只在于元素类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-174">`S` and `T` differ only in element type.</span></span> <span data-ttu-id="5c202-175">换而言之，`S`和`T`具有相同的维数。</span><span class="sxs-lookup"><span data-stu-id="5c202-175">In other words, `S` and `T` have the same number of dimensions.</span></span>
    * <span data-ttu-id="5c202-176">这两`SE`并`TE`都*reference_type*s。</span><span class="sxs-lookup"><span data-stu-id="5c202-176">Both `SE` and `TE` are *reference_type*s.</span></span>
    * <span data-ttu-id="5c202-177">从存在的隐式引用转换`SE`到`TE`。</span><span class="sxs-lookup"><span data-stu-id="5c202-177">An implicit reference conversion exists from `SE` to `TE`.</span></span>
*  <span data-ttu-id="5c202-178">从任何*array_type*到`System.Array`和其实现的接口。</span><span class="sxs-lookup"><span data-stu-id="5c202-178">From any *array_type* to `System.Array` and the interfaces it implements.</span></span>
*  <span data-ttu-id="5c202-179">从一维数组类型`S[]`到`System.Collections.Generic.IList<T>`及其基接口，提供从的隐式标识或引用转换`S`到`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-179">From a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its base interfaces, provided that there is an implicit identity or reference conversion from `S` to `T`.</span></span>
*  <span data-ttu-id="5c202-180">从任何*delegate_type*到`System.Delegate`和其实现的接口。</span><span class="sxs-lookup"><span data-stu-id="5c202-180">From any *delegate_type* to `System.Delegate` and the interfaces it implements.</span></span>
*  <span data-ttu-id="5c202-181">从任何 null 字面值*reference_type*。</span><span class="sxs-lookup"><span data-stu-id="5c202-181">From the null literal to any *reference_type*.</span></span>
*  <span data-ttu-id="5c202-182">从任何*reference_type*到*reference_type* `T`如果它具有隐式标识或引用转换为*reference_type* `T0`和`T0`标识转换为`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-182">From any *reference_type* to a *reference_type* `T` if it has an implicit identity or reference conversion to a *reference_type* `T0` and `T0` has an identity conversion to `T`.</span></span>
*  <span data-ttu-id="5c202-183">从任何*reference_type*到接口或委托类型`T`是否为接口或委托类型的隐式标识或引用转换`T0`和`T0`可变化转换 ([变化转换](interfaces.md#variance-conversion)) 到`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-183">From any *reference_type* to an interface or delegate type `T` if it has an implicit identity or reference conversion to an interface or delegate type `T0` and `T0` is variance-convertible ([Variance conversion](interfaces.md#variance-conversion)) to `T`.</span></span>
*  <span data-ttu-id="5c202-184">涉及已知为引用类型的类型参数的隐式转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-184">Implicit conversions involving type parameters that are known to be reference types.</span></span> <span data-ttu-id="5c202-185">请参阅[涉及类型参数的隐式转换](conversions.md#implicit-conversions-involving-type-parameters)有关涉及类型参数的隐式转换的详细信息。</span><span class="sxs-lookup"><span data-stu-id="5c202-185">See [Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters) for more details on implicit conversions involving type parameters.</span></span>

<span data-ttu-id="5c202-186">隐式引用转换指那些之间*reference_type*s 可以保证始终成功，并因此需要在运行时没有检查。</span><span class="sxs-lookup"><span data-stu-id="5c202-186">The implicit reference conversions are those conversions between *reference_type*s that can be proven to always succeed, and therefore require no checks at run-time.</span></span>

<span data-ttu-id="5c202-187">引用转换，隐式或显式的永远不会更改要转换的对象的引用标识。</span><span class="sxs-lookup"><span data-stu-id="5c202-187">Reference conversions, implicit or explicit, never change the referential identity of the object being converted.</span></span> <span data-ttu-id="5c202-188">换而言之，而是引用转换可能会更改引用的类型，它永远不会更改的类型或所引用对象的值。</span><span class="sxs-lookup"><span data-stu-id="5c202-188">In other words, while a reference conversion may change the type of the reference, it never changes the type or value of the object being referred to.</span></span>

### <a name="boxing-conversions"></a><span data-ttu-id="5c202-189">装箱转换</span><span class="sxs-lookup"><span data-stu-id="5c202-189">Boxing conversions</span></span>

<span data-ttu-id="5c202-190">装箱转换允许*value_type*隐式转换为引用类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-190">A boxing conversion permits a *value_type* to be implicitly converted to a reference type.</span></span> <span data-ttu-id="5c202-191">从任何已存在的装箱转换*non_nullable_value_type*到`object`并`dynamic`到`System.ValueType`和任何*interface_type*由实现*non_nullable_value_type*。</span><span class="sxs-lookup"><span data-stu-id="5c202-191">A boxing conversion exists from any *non_nullable_value_type* to `object` and `dynamic`, to `System.ValueType` and to any *interface_type* implemented by the *non_nullable_value_type*.</span></span> <span data-ttu-id="5c202-192">此外*enum_type*可以转换为类型`System.Enum`。</span><span class="sxs-lookup"><span data-stu-id="5c202-192">Furthermore an *enum_type* can be converted to the type `System.Enum`.</span></span>

<span data-ttu-id="5c202-193">存在从装箱转换*nullable_type*为引用类型，当且仅当装箱转换存在从基础*non_nullable_value_type*为引用类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-193">A boxing conversion exists from a *nullable_type* to a reference type, if and only if a boxing conversion exists from the underlying *non_nullable_value_type* to the reference type.</span></span>

<span data-ttu-id="5c202-194">值类型都具有为接口类型的装箱转换`I`是否为接口类型的装箱转换`I0`并`I0`标识转换为`I`。</span><span class="sxs-lookup"><span data-stu-id="5c202-194">A value type has a boxing conversion to an interface type `I` if it has a boxing conversion to an interface type `I0` and `I0` has an identity conversion to `I`.</span></span>

<span data-ttu-id="5c202-195">值类型都具有为接口类型的装箱转换`I`是否为接口或委托类型的装箱转换`I0`并`I0`可变化转换 ([变化转换](interfaces.md#variance-conversion)) 到`I`.</span><span class="sxs-lookup"><span data-stu-id="5c202-195">A value type has a boxing conversion to an interface type `I` if it has a boxing conversion to an interface or delegate type `I0` and `I0` is variance-convertible ([Variance conversion](interfaces.md#variance-conversion)) to `I`.</span></span>

<span data-ttu-id="5c202-196">装箱的值*non_nullable_value_type*分配给对象实例和复制组成*value_type*到该实例的值。</span><span class="sxs-lookup"><span data-stu-id="5c202-196">Boxing a value of a *non_nullable_value_type* consists of allocating an object instance and copying the *value_type* value into that instance.</span></span> <span data-ttu-id="5c202-197">结构可以被封装到类型`System.ValueType`，因为它是所有的结构的基类 ([继承](structs.md#inheritance))。</span><span class="sxs-lookup"><span data-stu-id="5c202-197">A struct can be boxed to the type `System.ValueType`, since that is a base class for all structs ([Inheritance](structs.md#inheritance)).</span></span>

<span data-ttu-id="5c202-198">装箱的值*nullable_type*继续，如下所示：</span><span class="sxs-lookup"><span data-stu-id="5c202-198">Boxing a value of a *nullable_type* proceeds as follows:</span></span>

*  <span data-ttu-id="5c202-199">如果源值为 null (`HasValue`属性为 false)，结果为 null 引用的目标类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-199">If the source value is null (`HasValue` property is false), the result is a null reference of the target type.</span></span>
*  <span data-ttu-id="5c202-200">否则，结果是一个装箱的引用`T`解包和装箱源值生成的。</span><span class="sxs-lookup"><span data-stu-id="5c202-200">Otherwise, the result is a reference to a boxed `T` produced by unwrapping and boxing the source value.</span></span>

<span data-ttu-id="5c202-201">装箱转换中进行了描述进一步[装箱转换](types.md#boxing-conversions)。</span><span class="sxs-lookup"><span data-stu-id="5c202-201">Boxing conversions are described further in [Boxing conversions](types.md#boxing-conversions).</span></span>

### <a name="implicit-dynamic-conversions"></a><span data-ttu-id="5c202-202">隐式动态转换</span><span class="sxs-lookup"><span data-stu-id="5c202-202">Implicit dynamic conversions</span></span>

<span data-ttu-id="5c202-203">存在从类型的表达式的隐式的动态转换`dynamic`为任何类型`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-203">An implicit dynamic conversion exists from an expression of type `dynamic` to any type `T`.</span></span> <span data-ttu-id="5c202-204">动态绑定转换 ([动态绑定](expressions.md#dynamic-binding))，这意味着，会在运行时从运行时类型的表达式与要求的隐式转换`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-204">The conversion is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), which means that an implicit conversion will be sought at run-time from the run-time type of the expression to `T`.</span></span> <span data-ttu-id="5c202-205">如果不找到任何转换，则引发运行时异常。</span><span class="sxs-lookup"><span data-stu-id="5c202-205">If no conversion is found, a run-time exception is thrown.</span></span>

<span data-ttu-id="5c202-206">请注意此隐式转换看似违反开头中的建议[隐式转换](conversions.md#implicit-conversions)的隐式转换时应永远不会导致异常。</span><span class="sxs-lookup"><span data-stu-id="5c202-206">Note that this implicit conversion seemingly violates the advice in the beginning of [Implicit conversions](conversions.md#implicit-conversions) that an implicit conversion should never cause an exception.</span></span> <span data-ttu-id="5c202-207">但是它不是转换本身，但*查找*会导致异常的转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-207">However it is not the conversion itself, but the *finding* of the conversion that causes the exception.</span></span> <span data-ttu-id="5c202-208">运行时异常的风险是固有的动态绑定的用法。</span><span class="sxs-lookup"><span data-stu-id="5c202-208">The risk of run-time exceptions is inherent in the use of dynamic binding.</span></span> <span data-ttu-id="5c202-209">如果不想转换的动态绑定，该表达式可以首先转换为`object`，然后到所需的类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-209">If dynamic binding of the conversion is not desired, the expression can be first converted to `object`, and then to the desired type.</span></span>

<span data-ttu-id="5c202-210">下面的示例说明了动态的隐式转换：</span><span class="sxs-lookup"><span data-stu-id="5c202-210">The following example illustrates implicit dynamic conversions:</span></span>

```csharp
object o  = "object"
dynamic d = "dynamic";

string s1 = o; // Fails at compile-time -- no conversion exists
string s2 = d; // Compiles and succeeds at run-time
int i     = d; // Compiles but fails at run-time -- no conversion exists
```

<span data-ttu-id="5c202-211">对分配`s2`和`i`同时采用隐式动态转换，直到运行时中挂起的操作的绑定。</span><span class="sxs-lookup"><span data-stu-id="5c202-211">The assignments to `s2` and `i` both employ implicit dynamic conversions, where the binding of the operations is suspended until run-time.</span></span> <span data-ttu-id="5c202-212">在运行时，隐式转换将从运行时类型的寻求`d`  --  `string` -为目标类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-212">At run-time, implicit conversions are sought from the run-time type of `d` -- `string` -- to the target type.</span></span> <span data-ttu-id="5c202-213">为发现转换`string`而不适用于`int`。</span><span class="sxs-lookup"><span data-stu-id="5c202-213">A conversion is found to `string` but not to `int`.</span></span>

### <a name="implicit-constant-expression-conversions"></a><span data-ttu-id="5c202-214">常量表达式隐式转换</span><span class="sxs-lookup"><span data-stu-id="5c202-214">Implicit constant expression conversions</span></span>

<span data-ttu-id="5c202-215">隐式的常量表达式转换允许以下转换：</span><span class="sxs-lookup"><span data-stu-id="5c202-215">An implicit constant expression conversion permits the following conversions:</span></span>

*  <span data-ttu-id="5c202-216">一个*constant_expression* ([常量表达式](expressions.md#constant-expressions)) 的类型`int`可转换为类型`sbyte`， `byte`， `short`， `ushort`， `uint`，或`ulong`，提供的值*constant_expression*目标类型的范围内。</span><span class="sxs-lookup"><span data-stu-id="5c202-216">A *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) of type `int` can be converted to type `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the *constant_expression* is within the range of the destination type.</span></span>
*  <span data-ttu-id="5c202-217">一个*constant_expression*类型的`long`可转换为类型`ulong`，提供的值*constant_expression*不是负数。</span><span class="sxs-lookup"><span data-stu-id="5c202-217">A *constant_expression* of type `long` can be converted to type `ulong`, provided the value of the *constant_expression* is not negative.</span></span>

### <a name="implicit-conversions-involving-type-parameters"></a><span data-ttu-id="5c202-218">涉及类型参数的隐式转换</span><span class="sxs-lookup"><span data-stu-id="5c202-218">Implicit conversions involving type parameters</span></span>

<span data-ttu-id="5c202-219">以下隐式转换为给定的类型参数存在`T`:</span><span class="sxs-lookup"><span data-stu-id="5c202-219">The following implicit conversions exist for a given type parameter `T`:</span></span>

*  <span data-ttu-id="5c202-220">从`T`向其有效的基类`C`，从`T`到的任何基类`C`，并从`T`实现任何接口到`C`。</span><span class="sxs-lookup"><span data-stu-id="5c202-220">From `T` to its effective base class `C`, from `T` to any base class of `C`, and from `T` to any interface implemented by `C`.</span></span> <span data-ttu-id="5c202-221">在运行时，如果`T`是值类型，则转换将执行作为装箱转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-221">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="5c202-222">否则，转换将执行作为隐式引用转换或标识转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-222">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="5c202-223">从`T`为接口类型`I`中`T`的有效接口集并从`T`到的任何基接口`I`。</span><span class="sxs-lookup"><span data-stu-id="5c202-223">From `T` to an interface type `I` in `T`'s effective interface set and from `T` to any base interface of `I`.</span></span> <span data-ttu-id="5c202-224">在运行时，如果`T`是值类型，则转换将执行作为装箱转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-224">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="5c202-225">否则，转换将执行作为隐式引用转换或标识转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-225">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="5c202-226">从`T`给类型形参`U`提供`T`取决于`U`([类型参数约束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="5c202-226">From `T` to a type parameter `U`, provided `T` depends on `U` ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="5c202-227">在运行时，如果`U`为值类型，则`T`和`U`一定具有相同的类型并不执行任何转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-227">At run-time, if `U` is a value type, then `T` and `U` are necessarily the same type and no conversion is performed.</span></span> <span data-ttu-id="5c202-228">否则为如果`T`是值类型，则转换将执行作为装箱转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-228">Otherwise, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="5c202-229">否则，转换将执行作为隐式引用转换或标识转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-229">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="5c202-230">从为 null 字面值`T`提供`T`已知为引用类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-230">From the null literal to `T`, provided `T` is known to be a reference type.</span></span>
*  <span data-ttu-id="5c202-231">从`T`为引用类型`I`是否为引用类型的隐式转换`S0`并`S0`标识转换为`S`。</span><span class="sxs-lookup"><span data-stu-id="5c202-231">From `T` to a reference type `I` if it has an implicit conversion to a reference type `S0` and `S0` has an identity conversion to `S`.</span></span> <span data-ttu-id="5c202-232">转换将在运行时执行方式与转换为相同`S0`。</span><span class="sxs-lookup"><span data-stu-id="5c202-232">At run-time the conversion is executed the same way as the conversion to `S0`.</span></span>
*  <span data-ttu-id="5c202-233">从`T`为接口类型`I`是否为接口或委托类型的隐式转换`I0`并`I0`可变化转换为`I`([变化转换](interfaces.md#variance-conversion)).</span><span class="sxs-lookup"><span data-stu-id="5c202-233">From `T` to an interface type `I` if it has an implicit conversion to an interface or delegate type `I0` and `I0` is variance-convertible to `I` ([Variance conversion](interfaces.md#variance-conversion)).</span></span> <span data-ttu-id="5c202-234">在运行时，如果`T`是值类型，则转换将执行作为装箱转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-234">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="5c202-235">否则，转换将执行作为隐式引用转换或标识转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-235">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>

<span data-ttu-id="5c202-236">如果`T`已知为引用类型 ([类型参数约束](classes.md#type-parameter-constraints))，则上面的转换归类为隐式引用转换 ([隐式引用转换](conversions.md#implicit-reference-conversions))。</span><span class="sxs-lookup"><span data-stu-id="5c202-236">If `T` is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)), the conversions above are all classified as implicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions)).</span></span> <span data-ttu-id="5c202-237">如果`T`是已知不是引用类型，则上面的转换归类为装箱转换 ([装箱转换](conversions.md#boxing-conversions))。</span><span class="sxs-lookup"><span data-stu-id="5c202-237">If `T` is not known to be a reference type, the conversions above are classified as boxing conversions ([Boxing conversions](conversions.md#boxing-conversions)).</span></span>

### <a name="user-defined-implicit-conversions"></a><span data-ttu-id="5c202-238">用户定义的隐式转换</span><span class="sxs-lookup"><span data-stu-id="5c202-238">User-defined implicit conversions</span></span>

<span data-ttu-id="5c202-239">用户定义的隐式转换包含的可选的标准隐式转换后, 跟的一个用户定义的隐式转换运算符后, 跟另一个可选的标准隐式转换的执行。</span><span class="sxs-lookup"><span data-stu-id="5c202-239">A user-defined implicit conversion consists of an optional standard implicit conversion, followed by execution of a user-defined implicit conversion operator, followed by another optional standard implicit conversion.</span></span> <span data-ttu-id="5c202-240">用于评估用户定义的隐式转换的确切规则所述[处理的用户定义的隐式转换](conversions.md#processing-of-user-defined-implicit-conversions)。</span><span class="sxs-lookup"><span data-stu-id="5c202-240">The exact rules for evaluating user-defined implicit conversions are described in [Processing of user-defined implicit conversions](conversions.md#processing-of-user-defined-implicit-conversions).</span></span>

### <a name="anonymous-function-conversions-and-method-group-conversions"></a><span data-ttu-id="5c202-241">匿名函数转换和方法组转换</span><span class="sxs-lookup"><span data-stu-id="5c202-241">Anonymous function conversions and method group conversions</span></span>

<span data-ttu-id="5c202-242">匿名函数和方法组不具有类型自身，但可能会隐式转换为委托类型或表达式树类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-242">Anonymous functions and method groups do not have types in and of themselves, but may be implicitly converted to delegate types or expression tree types.</span></span> <span data-ttu-id="5c202-243">中更详细地描述了匿名函数转换[匿名函数转换](conversions.md#anonymous-function-conversions)中的和方法组转换[方法组转换](conversions.md#method-group-conversions)。</span><span class="sxs-lookup"><span data-stu-id="5c202-243">Anonymous function conversions are described in more detail in [Anonymous function conversions](conversions.md#anonymous-function-conversions) and method group conversions in [Method group conversions](conversions.md#method-group-conversions).</span></span>

## <a name="explicit-conversions"></a><span data-ttu-id="5c202-244">显式转换</span><span class="sxs-lookup"><span data-stu-id="5c202-244">Explicit conversions</span></span>

<span data-ttu-id="5c202-245">以下转换被归类为显式转换：</span><span class="sxs-lookup"><span data-stu-id="5c202-245">The following conversions are classified as explicit conversions:</span></span>

*  <span data-ttu-id="5c202-246">所有隐式转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-246">All implicit conversions.</span></span>
*  <span data-ttu-id="5c202-247">显式数值转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-247">Explicit numeric conversions.</span></span>
*  <span data-ttu-id="5c202-248">显式枚举转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-248">Explicit enumeration conversions.</span></span>
*  <span data-ttu-id="5c202-249">可以为 null 的显式转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-249">Explicit nullable conversions.</span></span>
*  <span data-ttu-id="5c202-250">显式引用转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-250">Explicit reference conversions.</span></span>
*  <span data-ttu-id="5c202-251">显式接口转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-251">Explicit interface conversions.</span></span>
*  <span data-ttu-id="5c202-252">取消装箱转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-252">Unboxing conversions.</span></span>
*  <span data-ttu-id="5c202-253">动态的显式转换</span><span class="sxs-lookup"><span data-stu-id="5c202-253">Explicit dynamic conversions</span></span>
*  <span data-ttu-id="5c202-254">用户定义的显式转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-254">User-defined explicit conversions.</span></span>

<span data-ttu-id="5c202-255">显式转换可以出现在强制转换表达式 ([强制转换表达式](expressions.md#cast-expressions))。</span><span class="sxs-lookup"><span data-stu-id="5c202-255">Explicit conversions can occur in cast expressions ([Cast expressions](expressions.md#cast-expressions)).</span></span>

<span data-ttu-id="5c202-256">显式转换该集包含所有隐式转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-256">The set of explicit conversions includes all implicit conversions.</span></span> <span data-ttu-id="5c202-257">这意味着允许多余的强制转换表达式。</span><span class="sxs-lookup"><span data-stu-id="5c202-257">This means that redundant cast expressions are allowed.</span></span>

<span data-ttu-id="5c202-258">不是隐式转换的显式转换为跨类型采用显式明显不同的域不能保证始终成功的转换、 转换已知有可能丢失信息和转换表示法。</span><span class="sxs-lookup"><span data-stu-id="5c202-258">The explicit conversions that are not implicit conversions are conversions that cannot be proven to always succeed, conversions that are known to possibly lose information, and conversions across domains of types sufficiently different to merit explicit notation.</span></span>

### <a name="explicit-numeric-conversions"></a><span data-ttu-id="5c202-259">显式数值转换</span><span class="sxs-lookup"><span data-stu-id="5c202-259">Explicit numeric conversions</span></span>

<span data-ttu-id="5c202-260">显式数值转换是从转换*numeric_type*到另一个*numeric_type*为其隐式数值转换 ([隐式数值转换](conversions.md#implicit-numeric-conversions))尚不存在：</span><span class="sxs-lookup"><span data-stu-id="5c202-260">The explicit numeric conversions are the conversions from a *numeric_type* to another *numeric_type* for which an implicit numeric conversion ([Implicit numeric conversions](conversions.md#implicit-numeric-conversions)) does not already exist:</span></span>

*  <span data-ttu-id="5c202-261">从`sbyte`到`byte`， `ushort`， `uint`， `ulong`，或`char`。</span><span class="sxs-lookup"><span data-stu-id="5c202-261">From `sbyte` to `byte`, `ushort`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="5c202-262">从`byte`到`sbyte`和`char`。</span><span class="sxs-lookup"><span data-stu-id="5c202-262">From `byte` to `sbyte` and `char`.</span></span>
*  <span data-ttu-id="5c202-263">从`short`到`sbyte`， `byte`， `ushort`， `uint`， `ulong`，或`char`。</span><span class="sxs-lookup"><span data-stu-id="5c202-263">From `short` to `sbyte`, `byte`, `ushort`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="5c202-264">从`ushort`到`sbyte`， `byte`， `short`，或`char`。</span><span class="sxs-lookup"><span data-stu-id="5c202-264">From `ushort` to `sbyte`, `byte`, `short`, or `char`.</span></span>
*  <span data-ttu-id="5c202-265">从`int`到`sbyte`， `byte`， `short`， `ushort`， `uint`， `ulong`，或`char`。</span><span class="sxs-lookup"><span data-stu-id="5c202-265">From `int` to `sbyte`, `byte`, `short`, `ushort`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="5c202-266">从`uint`到`sbyte`， `byte`， `short`， `ushort`， `int`，或`char`。</span><span class="sxs-lookup"><span data-stu-id="5c202-266">From `uint` to `sbyte`, `byte`, `short`, `ushort`, `int`, or `char`.</span></span>
*  <span data-ttu-id="5c202-267">从`long`到`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `ulong`，或`char`。</span><span class="sxs-lookup"><span data-stu-id="5c202-267">From `long` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="5c202-268">从`ulong`到`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`，或`char`。</span><span class="sxs-lookup"><span data-stu-id="5c202-268">From `ulong` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `char`.</span></span>
*  <span data-ttu-id="5c202-269">从`char`到`sbyte`， `byte`，或`short`。</span><span class="sxs-lookup"><span data-stu-id="5c202-269">From `char` to `sbyte`, `byte`, or `short`.</span></span>
*  <span data-ttu-id="5c202-270">从`float`到`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="5c202-270">From `float` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, or `decimal`.</span></span>
*  <span data-ttu-id="5c202-271">从`double`到`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="5c202-271">From `double` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, or `decimal`.</span></span>
*  <span data-ttu-id="5c202-272">从`decimal`到`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`，或`double`。</span><span class="sxs-lookup"><span data-stu-id="5c202-272">From `decimal` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, or `double`.</span></span>

<span data-ttu-id="5c202-273">因为显式转换包括所有隐式和显式数值转换，并总是能够从任何转换*numeric_type*到任何其他*numeric_type*使用强制转换表达式 （[强制转换表达式](expressions.md#cast-expressions))。</span><span class="sxs-lookup"><span data-stu-id="5c202-273">Because the explicit conversions include all implicit and explicit numeric conversions, it is always possible to convert from any *numeric_type* to any other *numeric_type* using a cast expression ([Cast expressions](expressions.md#cast-expressions)).</span></span>

<span data-ttu-id="5c202-274">显式数值转换可能会丢失信息，或可能会导致引发异常。</span><span class="sxs-lookup"><span data-stu-id="5c202-274">The explicit numeric conversions possibly lose information or possibly cause exceptions to be thrown.</span></span> <span data-ttu-id="5c202-275">显式数值转换进行处理，如下所示：</span><span class="sxs-lookup"><span data-stu-id="5c202-275">An explicit numeric conversion is processed as follows:</span></span>

*  <span data-ttu-id="5c202-276">对于从一种整型类型转换为另一种整型类型，在处理取决于溢出检查上下文 ([checked 和 unchecked 运算符](expressions.md#the-checked-and-unchecked-operators)) 都位于其将转换的位置：</span><span class="sxs-lookup"><span data-stu-id="5c202-276">For a conversion from an integral type to another integral type, the processing depends on the overflow checking context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)) in which the conversion takes place:</span></span>
    * <span data-ttu-id="5c202-277">在中`checked`上下文中，转换成功，如果源操作数的值是目标类型的范围内，但会引发`System.OverflowException`如果源操作数的值超出目标类型的范围。</span><span class="sxs-lookup"><span data-stu-id="5c202-277">In a `checked` context, the conversion succeeds if the value of the source operand is within the range of the destination type, but throws a `System.OverflowException` if the value of the source operand is outside the range of the destination type.</span></span>
    * <span data-ttu-id="5c202-278">在`unchecked`上下文中，转换始终成功，并继续，如下所示。</span><span class="sxs-lookup"><span data-stu-id="5c202-278">In an `unchecked` context, the conversion always succeeds, and proceeds as follows.</span></span>
        * <span data-ttu-id="5c202-279">如果源类型大于目标类型，则通过放弃其“额外”最高有效位来截断源值。</span><span class="sxs-lookup"><span data-stu-id="5c202-279">If the source type is larger than the destination type, then the source value is truncated by discarding its "extra" most significant bits.</span></span> <span data-ttu-id="5c202-280">结果会被视为目标类型的值。</span><span class="sxs-lookup"><span data-stu-id="5c202-280">The result is then treated as a value of the destination type.</span></span>
        * <span data-ttu-id="5c202-281">如果源类型小于目标类型，则源值是符号扩展或零扩展，以使其与目标类型的大小相同。</span><span class="sxs-lookup"><span data-stu-id="5c202-281">If the source type is smaller than the destination type, then the source value is either sign-extended or zero-extended so that it is the same size as the destination type.</span></span> <span data-ttu-id="5c202-282">如果源类型带符号，则是符号扩展；如果源类型是无符号的，则是零扩展。</span><span class="sxs-lookup"><span data-stu-id="5c202-282">Sign-extension is used if the source type is signed; zero-extension is used if the source type is unsigned.</span></span> <span data-ttu-id="5c202-283">结果会被视为目标类型的值。</span><span class="sxs-lookup"><span data-stu-id="5c202-283">The result is then treated as a value of the destination type.</span></span>
        * <span data-ttu-id="5c202-284">如果源类型与目标类型的大小相同，则源值将被视为目标类型的值。</span><span class="sxs-lookup"><span data-stu-id="5c202-284">If the source type is the same size as the destination type, then the source value is treated as a value of the destination type.</span></span>
*  <span data-ttu-id="5c202-285">对于从`decimal`为整型类型，源值向零舍入为最接近的整数值，并且此整数值将成为转换的结果。</span><span class="sxs-lookup"><span data-stu-id="5c202-285">For a conversion from `decimal` to an integral type, the source value is rounded towards zero to the nearest integral value, and this integral value becomes the result of the conversion.</span></span> <span data-ttu-id="5c202-286">如果生成的整数值超出目标类型的范围`System.OverflowException`引发。</span><span class="sxs-lookup"><span data-stu-id="5c202-286">If the resulting integral value is outside the range of the destination type, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="5c202-287">对于从`float`或`double`为整型类型，在处理取决于溢出检查上下文 ([checked 和 unchecked 运算符](expressions.md#the-checked-and-unchecked-operators)) 都位于其将转换的位置：</span><span class="sxs-lookup"><span data-stu-id="5c202-287">For a conversion from `float` or `double` to an integral type, the processing depends on the overflow checking context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)) in which the conversion takes place:</span></span>
    * <span data-ttu-id="5c202-288">在`checked`上下文中，转换将继续，如下所示：</span><span class="sxs-lookup"><span data-stu-id="5c202-288">In a `checked` context, the conversion proceeds as follows:</span></span>
        * <span data-ttu-id="5c202-289">如果操作数的值为 NaN 或无穷大，`System.OverflowException`引发。</span><span class="sxs-lookup"><span data-stu-id="5c202-289">If the value of the operand is NaN or infinite, a `System.OverflowException` is thrown.</span></span>
        * <span data-ttu-id="5c202-290">否则，源操作数是向零到最接近的整数值舍入。</span><span class="sxs-lookup"><span data-stu-id="5c202-290">Otherwise, the source operand is rounded towards zero to the nearest integral value.</span></span> <span data-ttu-id="5c202-291">如果此整数值处于目标类型的范围内的此值是转换的结果。</span><span class="sxs-lookup"><span data-stu-id="5c202-291">If this integral value is within the range of the destination type then this value is the result of the conversion.</span></span>
        * <span data-ttu-id="5c202-292">否则，将会引发 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="5c202-292">Otherwise, a `System.OverflowException` is thrown.</span></span>
    * <span data-ttu-id="5c202-293">在`unchecked`上下文中，转换始终成功，并继续，如下所示。</span><span class="sxs-lookup"><span data-stu-id="5c202-293">In an `unchecked` context, the conversion always succeeds, and proceeds as follows.</span></span>
        * <span data-ttu-id="5c202-294">如果操作数的值为 NaN 或无限的转换的结果是目标类型的未指定的值。</span><span class="sxs-lookup"><span data-stu-id="5c202-294">If the value of the operand is NaN or infinite, the result of the conversion is an unspecified value of the destination type.</span></span>
        * <span data-ttu-id="5c202-295">否则，源操作数是向零到最接近的整数值舍入。</span><span class="sxs-lookup"><span data-stu-id="5c202-295">Otherwise, the source operand is rounded towards zero to the nearest integral value.</span></span> <span data-ttu-id="5c202-296">如果此整数值处于目标类型的范围内的此值是转换的结果。</span><span class="sxs-lookup"><span data-stu-id="5c202-296">If this integral value is within the range of the destination type then this value is the result of the conversion.</span></span>
        * <span data-ttu-id="5c202-297">否则，转换的结果是目标类型的未指定的值。</span><span class="sxs-lookup"><span data-stu-id="5c202-297">Otherwise, the result of the conversion is an unspecified value of the destination type.</span></span>
*  <span data-ttu-id="5c202-298">对于从`double`到`float`，则`double`值舍入为最接近`float`值。</span><span class="sxs-lookup"><span data-stu-id="5c202-298">For a conversion from `double` to `float`, the `double` value is rounded to the nearest `float` value.</span></span> <span data-ttu-id="5c202-299">如果`double`值因过小而无法表示为`float`，结果将成为正零或负零。</span><span class="sxs-lookup"><span data-stu-id="5c202-299">If the `double` value is too small to represent as a `float`, the result becomes positive zero or negative zero.</span></span> <span data-ttu-id="5c202-300">如果`double`该值太大而无法表示为`float`，其结果成为正无穷或负无穷大。</span><span class="sxs-lookup"><span data-stu-id="5c202-300">If the `double` value is too large to represent as a `float`, the result becomes positive infinity or negative infinity.</span></span> <span data-ttu-id="5c202-301">如果`double`值为 NaN，则结果也为 NaN。</span><span class="sxs-lookup"><span data-stu-id="5c202-301">If the `double` value is NaN, the result is also NaN.</span></span>
*  <span data-ttu-id="5c202-302">对于从`float`或`double`到`decimal`，源值转换为`decimal`表示形式和舍入为最接近的数字 28 位小数后根据需要 ([十进制类型](types.md#the-decimal-type)).</span><span class="sxs-lookup"><span data-stu-id="5c202-302">For a conversion from `float` or `double` to `decimal`, the source value is converted to `decimal` representation and rounded to the nearest number after the 28th decimal place if required ([The decimal type](types.md#the-decimal-type)).</span></span> <span data-ttu-id="5c202-303">如果源值就太小，无法表示为`decimal`，结果则为零。</span><span class="sxs-lookup"><span data-stu-id="5c202-303">If the source value is too small to represent as a `decimal`, the result becomes zero.</span></span> <span data-ttu-id="5c202-304">如果源值为 NaN、 无穷大或太大而无法表示为`decimal`、`System.OverflowException`引发。</span><span class="sxs-lookup"><span data-stu-id="5c202-304">If the source value is NaN, infinity, or too large to represent as a `decimal`, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="5c202-305">对于从`decimal`到`float`或`double`，则`decimal`值舍入为最接近`double`或`float`值。</span><span class="sxs-lookup"><span data-stu-id="5c202-305">For a conversion from `decimal` to `float` or `double`, the `decimal` value is rounded to the nearest `double` or `float` value.</span></span> <span data-ttu-id="5c202-306">此转换可能会丢失精度，尽管它从不会导致引发异常。</span><span class="sxs-lookup"><span data-stu-id="5c202-306">While this conversion may lose precision, it never causes an exception to be thrown.</span></span>

### <a name="explicit-enumeration-conversions"></a><span data-ttu-id="5c202-307">显式枚举转换</span><span class="sxs-lookup"><span data-stu-id="5c202-307">Explicit enumeration conversions</span></span>

<span data-ttu-id="5c202-308">显式枚举转换为：</span><span class="sxs-lookup"><span data-stu-id="5c202-308">The explicit enumeration conversions are:</span></span>

*  <span data-ttu-id="5c202-309">从`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`， `double`，或`decimal`到任何*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="5c202-309">From `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `decimal` to any *enum_type*.</span></span>
*  <span data-ttu-id="5c202-310">从任何*enum_type*到`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`， `double`，或`decimal`。</span><span class="sxs-lookup"><span data-stu-id="5c202-310">From any *enum_type* to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="5c202-311">从任何*enum_type*到任何其他*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="5c202-311">From any *enum_type* to any other *enum_type*.</span></span>

<span data-ttu-id="5c202-312">两个类型之间的显式枚举转换处理方法是将任何参与*enum_type*的基础类型为*enum_type*，，然后再执行隐式或显式在结果类型之间的数值转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-312">An explicit enumeration conversion between two types is processed by treating any participating *enum_type* as the underlying type of that *enum_type*, and then performing an implicit or explicit numeric conversion between the resulting types.</span></span> <span data-ttu-id="5c202-313">例如，给定*enum_type* `E`使用和的基础类型`int`，从转换`E`到`byte`作为显式数值转换处理 ([Explicit数值转换](conversions.md#explicit-numeric-conversions)) 从`int`到`byte`，并从转换`byte`到`E`被处理为隐式数值转换 ([隐式数值转换](conversions.md#implicit-numeric-conversions))从`byte`到`int`。</span><span class="sxs-lookup"><span data-stu-id="5c202-313">For example, given an *enum_type* `E` with and underlying type of `int`, a conversion from `E` to `byte` is processed as an explicit numeric conversion ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from `int` to `byte`, and a conversion from `byte` to `E` is processed as an implicit numeric conversion ([Implicit numeric conversions](conversions.md#implicit-numeric-conversions)) from `byte` to `int`.</span></span>

### <a name="explicit-nullable-conversions"></a><span data-ttu-id="5c202-314">可以为 null 的显式转换</span><span class="sxs-lookup"><span data-stu-id="5c202-314">Explicit nullable conversions</span></span>

<span data-ttu-id="5c202-315">***可以为 null 的显式转换***允许预定义的操作不可以为 null 的值类型也可用于这些类型的可以为 null 的窗体的显式转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-315">***Explicit nullable conversions*** permit predefined explicit conversions that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="5c202-316">为每个预定义的显式转换将从非 null 的值类型转换`S`为非 null 的值类型`T`([标识转换](conversions.md#identity-conversion)，[隐式数值转换](conversions.md#implicit-numeric-conversions)，[隐式枚举转换](conversions.md#implicit-enumeration-conversions)，[显式数值转换](conversions.md#explicit-numeric-conversions)，并且[显式枚举转换](conversions.md#explicit-enumeration-conversions))，以下可以为 null 的转换存在：</span><span class="sxs-lookup"><span data-stu-id="5c202-316">For each of the predefined explicit conversions that convert from a non-nullable value type `S` to a non-nullable value type `T` ([Identity conversion](conversions.md#identity-conversion), [Implicit numeric conversions](conversions.md#implicit-numeric-conversions), [Implicit enumeration conversions](conversions.md#implicit-enumeration-conversions), [Explicit numeric conversions](conversions.md#explicit-numeric-conversions), and [Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)), the following nullable conversions exist:</span></span>

*  <span data-ttu-id="5c202-317">从的显式转换`S?`到`T?`。</span><span class="sxs-lookup"><span data-stu-id="5c202-317">An explicit conversion from `S?` to `T?`.</span></span>
*  <span data-ttu-id="5c202-318">从的显式转换`S`到`T?`。</span><span class="sxs-lookup"><span data-stu-id="5c202-318">An explicit conversion from `S` to `T?`.</span></span>
*  <span data-ttu-id="5c202-319">从的显式转换`S?`到`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-319">An explicit conversion from `S?` to `T`.</span></span>

<span data-ttu-id="5c202-320">可以为 null 的转换的计算基于一种从基础转换`S`到`T`继续，如下所示：</span><span class="sxs-lookup"><span data-stu-id="5c202-320">Evaluation of a nullable conversion based on an underlying conversion from `S` to `T` proceeds as follows:</span></span>

*  <span data-ttu-id="5c202-321">如果可以为 null 的转换是从`S?`到`T?`:</span><span class="sxs-lookup"><span data-stu-id="5c202-321">If the nullable conversion is from `S?` to `T?`:</span></span>
    * <span data-ttu-id="5c202-322">如果源值为 null (`HasValue`属性为 false)，结果是类型的 null 值`T?`。</span><span class="sxs-lookup"><span data-stu-id="5c202-322">If the source value is null (`HasValue` property is false), the result is the null value of type `T?`.</span></span>
    * <span data-ttu-id="5c202-323">否则，则转换计算过程为从`S?`到`S`后, 跟从基础转换`S`到`T`后, 跟从包装`T`到`T?`。</span><span class="sxs-lookup"><span data-stu-id="5c202-323">Otherwise, the conversion is evaluated as an unwrapping from `S?` to `S`, followed by the underlying conversion from `S` to `T`, followed by a wrapping from `T` to `T?`.</span></span>
*  <span data-ttu-id="5c202-324">如果可以为 null 的转换是从`S`到`T?`，转换计算为从基础转换`S`到`T`跟从一个包装`T`到`T?`。</span><span class="sxs-lookup"><span data-stu-id="5c202-324">If the nullable conversion is from `S` to `T?`, the conversion is evaluated as the underlying conversion from `S` to `T` followed by a wrapping from `T` to `T?`.</span></span>
*  <span data-ttu-id="5c202-325">如果可以为 null 的转换是从`S?`到`T`，则转换过程为从计算`S?`到`S`跟从基础转换`S`到`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-325">If the nullable conversion is from `S?` to `T`, the conversion is evaluated as an unwrapping from `S?` to `S` followed by the underlying conversion from `S` to `T`.</span></span>

<span data-ttu-id="5c202-326">请注意，尝试打开可以为 null 值将引发异常，是否值为`null`。</span><span class="sxs-lookup"><span data-stu-id="5c202-326">Note that an attempt to unwrap a nullable value will throw an exception if the value is `null`.</span></span>

### <a name="explicit-reference-conversions"></a><span data-ttu-id="5c202-327">显式引用转换</span><span class="sxs-lookup"><span data-stu-id="5c202-327">Explicit reference conversions</span></span>

<span data-ttu-id="5c202-328">显式引用转换为：</span><span class="sxs-lookup"><span data-stu-id="5c202-328">The explicit reference conversions are:</span></span>

*  <span data-ttu-id="5c202-329">从`object`并`dynamic`到任何其他*reference_type*。</span><span class="sxs-lookup"><span data-stu-id="5c202-329">From `object` and `dynamic` to any other *reference_type*.</span></span>
*  <span data-ttu-id="5c202-330">从任何*class_type* `S`任何*class_type* `T`提供`S`是基类的`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-330">From any *class_type* `S` to any *class_type* `T`, provided `S` is a base class of `T`.</span></span>
*  <span data-ttu-id="5c202-331">从任何*class_type* `S`任何*interface_type* `T`提供`S`未密封的并提供`S`不实现`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-331">From any *class_type* `S` to any *interface_type* `T`, provided `S` is not sealed and provided `S` does not implement `T`.</span></span>
*  <span data-ttu-id="5c202-332">从任何*interface_type* `S`任何*class_type* `T`提供`T`未密封的或未提供`T`实现`S`。</span><span class="sxs-lookup"><span data-stu-id="5c202-332">From any *interface_type* `S` to any *class_type* `T`, provided `T` is not sealed or provided `T` implements `S`.</span></span>
*  <span data-ttu-id="5c202-333">从任何*interface_type* `S`任何*interface_type* `T`提供`S`不派生自`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-333">From any *interface_type* `S` to any *interface_type* `T`, provided `S` is not derived from `T`.</span></span>
*  <span data-ttu-id="5c202-334">从*array_type* `S`具有元素类型`SE`到*array_type* `T`具有元素类型`TE`，提供以下所有项都为 true:</span><span class="sxs-lookup"><span data-stu-id="5c202-334">From an *array_type* `S` with an element type `SE` to an *array_type* `T` with an element type `TE`, provided all of the following are true:</span></span>
    * <span data-ttu-id="5c202-335">`S` 和`T`的区别只在于元素类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-335">`S` and `T` differ only in element type.</span></span> <span data-ttu-id="5c202-336">换而言之，`S`和`T`具有相同的维数。</span><span class="sxs-lookup"><span data-stu-id="5c202-336">In other words, `S` and `T` have the same number of dimensions.</span></span>
    * <span data-ttu-id="5c202-337">这两`SE`并`TE`都*reference_type*s。</span><span class="sxs-lookup"><span data-stu-id="5c202-337">Both `SE` and `TE` are *reference_type*s.</span></span>
    * <span data-ttu-id="5c202-338">存在从的显式引用转换`SE`到`TE`。</span><span class="sxs-lookup"><span data-stu-id="5c202-338">An explicit reference conversion exists from `SE` to `TE`.</span></span>
*  <span data-ttu-id="5c202-339">从`System.Array`和它对任何实现的接口*array_type*。</span><span class="sxs-lookup"><span data-stu-id="5c202-339">From `System.Array` and the interfaces it implements to any *array_type*.</span></span>
*  <span data-ttu-id="5c202-340">从一维数组类型`S[]`到`System.Collections.Generic.IList<T>`并提供没有显式引用转换从其基接口`S`到`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-340">From a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its base interfaces, provided that there is an explicit reference conversion from `S` to `T`.</span></span>
*  <span data-ttu-id="5c202-341">从`System.Collections.Generic.IList<S>`到一维数组类型及其基接口`T[]`，前提是没有从的显式标识或引用转换`S`到`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-341">From `System.Collections.Generic.IList<S>` and its base interfaces to a single-dimensional array type `T[]`, provided that there is an explicit identity or reference conversion from `S` to `T`.</span></span>
*  <span data-ttu-id="5c202-342">从`System.Delegate`和它对任何实现的接口*delegate_type*。</span><span class="sxs-lookup"><span data-stu-id="5c202-342">From `System.Delegate` and the interfaces it implements to any *delegate_type*.</span></span>
*  <span data-ttu-id="5c202-343">从引用类型的引用类型`T`是否为引用类型的显式引用转换`T0`并`T0`标识转换`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-343">From a reference type to a reference type `T` if it has an explicit reference conversion to a reference type `T0` and `T0` has an identity conversion `T`.</span></span>
*  <span data-ttu-id="5c202-344">从接口或委托类型的引用类型`T`是否为接口或委托类型的显式引用转换`T0`并将`T0`可变化转换到`T`或`T`是变化转换到`T0`([变化转换](interfaces.md#variance-conversion))。</span><span class="sxs-lookup"><span data-stu-id="5c202-344">From a reference type to an interface or delegate type `T` if it has an explicit reference conversion to an interface or delegate type `T0` and either `T0` is variance-convertible to `T` or `T` is variance-convertible to `T0` ([Variance conversion](interfaces.md#variance-conversion)).</span></span>
*  <span data-ttu-id="5c202-345">从`D<S1...Sn>`到`D<T1...Tn>`其中`D<X1...Xn>`是泛型委托类型，`D<S1...Sn>`不符合或相同`D<T1...Tn>`，并为每个类型参数`Xi`的`D`以下包含：</span><span class="sxs-lookup"><span data-stu-id="5c202-345">From `D<S1...Sn>` to `D<T1...Tn>` where `D<X1...Xn>` is a generic delegate type, `D<S1...Sn>` is not compatible with or identical to `D<T1...Tn>`, and for each type parameter `Xi` of `D` the following holds:</span></span>
    * <span data-ttu-id="5c202-346">如果`Xi`不变，然后`Si`等同于`Ti`。</span><span class="sxs-lookup"><span data-stu-id="5c202-346">If `Xi` is invariant, then `Si` is identical to `Ti`.</span></span>
    * <span data-ttu-id="5c202-347">如果`Xi`是协变，则隐式或显式标识或引用转换从`Si`到`Ti`。</span><span class="sxs-lookup"><span data-stu-id="5c202-347">If `Xi` is covariant, then there is an implicit or explicit identity or reference conversion from `Si` to `Ti`.</span></span>
    * <span data-ttu-id="5c202-348">如果`Xi`然后为逆变`Si`和`Ti`为相同或同时引用类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-348">If `Xi` is contravariant, then `Si` and `Ti` are either identical or both reference types.</span></span>
*  <span data-ttu-id="5c202-349">涉及已知为引用类型的类型参数的显式转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-349">Explicit conversions involving type parameters that are known to be reference types.</span></span> <span data-ttu-id="5c202-350">有关涉及类型参数的显式转换的更多详细信息，请参阅[涉及类型参数的显式转换](conversions.md#explicit-conversions-involving-type-parameters)。</span><span class="sxs-lookup"><span data-stu-id="5c202-350">For more details on explicit conversions involving type parameters, see [Explicit conversions involving type parameters](conversions.md#explicit-conversions-involving-type-parameters).</span></span>

<span data-ttu-id="5c202-351">显式引用转换指那些需要运行时检查，以确保它们正确无误的引用类型之间。</span><span class="sxs-lookup"><span data-stu-id="5c202-351">The explicit reference conversions are those conversions between reference-types that require run-time checks to ensure they are correct.</span></span>

<span data-ttu-id="5c202-352">对于在运行时成功的显式引用转换，源操作数的值必须`null`，或者源操作数所引用的对象的实际类型必须是可通过隐式引用转换为目标类型的类型转换 ([隐式引用转换](conversions.md#implicit-reference-conversions)) 或装箱转换 ([装箱转换](conversions.md#boxing-conversions))。</span><span class="sxs-lookup"><span data-stu-id="5c202-352">For an explicit reference conversion to succeed at run-time, the value of the source operand must be `null`, or the actual type of the object referenced by the source operand must be a type that can be converted to the destination type by an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) or boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)).</span></span> <span data-ttu-id="5c202-353">如果显式引用转换失败，`System.InvalidCastException`引发。</span><span class="sxs-lookup"><span data-stu-id="5c202-353">If an explicit reference conversion fails, a `System.InvalidCastException` is thrown.</span></span>

<span data-ttu-id="5c202-354">引用转换，隐式或显式的永远不会更改要转换的对象的引用标识。</span><span class="sxs-lookup"><span data-stu-id="5c202-354">Reference conversions, implicit or explicit, never change the referential identity of the object being converted.</span></span> <span data-ttu-id="5c202-355">换而言之，而是引用转换可能会更改引用的类型，它永远不会更改的类型或所引用对象的值。</span><span class="sxs-lookup"><span data-stu-id="5c202-355">In other words, while a reference conversion may change the type of the reference, it never changes the type or value of the object being referred to.</span></span>

### <a name="unboxing-conversions"></a><span data-ttu-id="5c202-356">取消装箱转换</span><span class="sxs-lookup"><span data-stu-id="5c202-356">Unboxing conversions</span></span>

<span data-ttu-id="5c202-357">取消装箱转换允许显式转换为引用类型*value_type*。</span><span class="sxs-lookup"><span data-stu-id="5c202-357">An unboxing conversion permits a reference type to be explicitly converted to a *value_type*.</span></span> <span data-ttu-id="5c202-358">取消装箱转换存在从类型`object`，`dynamic`并`System.ValueType`任何*non_nullable_value_type*，并从任何*interface_type*任何*non_nullable_value_type* ，它实现*interface_type*。</span><span class="sxs-lookup"><span data-stu-id="5c202-358">An unboxing conversion exists from the types `object`, `dynamic` and `System.ValueType` to any *non_nullable_value_type*, and from any *interface_type* to any *non_nullable_value_type* that implements the *interface_type*.</span></span> <span data-ttu-id="5c202-359">此外键入`System.Enum`可以是任何未装箱*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="5c202-359">Furthermore type `System.Enum` can be unboxed to any *enum_type*.</span></span>

<span data-ttu-id="5c202-360">取消装箱转换从引用类型到存在*nullable_type*如果取消装箱转换从引用类型存在于基础*non_nullable_value_type*的*nullable_type*。</span><span class="sxs-lookup"><span data-stu-id="5c202-360">An unboxing conversion exists from a reference type to a *nullable_type* if an unboxing conversion exists from the reference type to the underlying *non_nullable_value_type* of the *nullable_type*.</span></span>

<span data-ttu-id="5c202-361">值类型`S`已从接口类型的取消装箱转换`I`它是否有从接口类型的取消装箱转换`I0`并`I0`标识转换为`I`。</span><span class="sxs-lookup"><span data-stu-id="5c202-361">A value type `S` has an unboxing conversion from an interface type `I` if it has an unboxing conversion from an interface type `I0` and `I0` has an identity conversion to `I`.</span></span>

<span data-ttu-id="5c202-362">值类型`S`已从接口类型的取消装箱转换`I`如果有从接口或委托类型的取消装箱转换`I0`并将`I0`可变化转换到`I`或`I`可变化转换到`I0`([变化转换](interfaces.md#variance-conversion))。</span><span class="sxs-lookup"><span data-stu-id="5c202-362">A value type `S` has an unboxing conversion from an interface type `I` if it has an unboxing conversion from an interface or delegate type `I0` and either `I0` is variance-convertible to `I` or `I` is variance-convertible to `I0` ([Variance conversion](interfaces.md#variance-conversion)).</span></span>

<span data-ttu-id="5c202-363">取消装箱操作组成的对象实例的装箱的值的第一个检查给定*value_type*，然后复制实例以外的值。</span><span class="sxs-lookup"><span data-stu-id="5c202-363">An unboxing operation consists of first checking that the object instance is a boxed value of the given *value_type*, and then copying the value out of the instance.</span></span> <span data-ttu-id="5c202-364">取消装箱对的 null 引用*nullable_type*生成的 null 值*nullable_type*。</span><span class="sxs-lookup"><span data-stu-id="5c202-364">Unboxing a null reference to a *nullable_type* produces the null value of the *nullable_type*.</span></span> <span data-ttu-id="5c202-365">结构可以是从类型未装箱`System.ValueType`，因为它是所有的结构的基类 ([继承](structs.md#inheritance))。</span><span class="sxs-lookup"><span data-stu-id="5c202-365">A struct can be unboxed from the type `System.ValueType`, since that is a base class for all structs ([Inheritance](structs.md#inheritance)).</span></span>

<span data-ttu-id="5c202-366">取消装箱转换都是中作了进一步介绍[取消装箱转换](types.md#unboxing-conversions)。</span><span class="sxs-lookup"><span data-stu-id="5c202-366">Unboxing conversions are described further in [Unboxing conversions](types.md#unboxing-conversions).</span></span>

### <a name="explicit-dynamic-conversions"></a><span data-ttu-id="5c202-367">动态的显式转换</span><span class="sxs-lookup"><span data-stu-id="5c202-367">Explicit dynamic conversions</span></span>

<span data-ttu-id="5c202-368">类型的表达式中存在一个显式的动态转换`dynamic`为任何类型`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-368">An explicit dynamic conversion exists from an expression of type `dynamic` to any type `T`.</span></span> <span data-ttu-id="5c202-369">动态绑定转换 ([动态绑定](expressions.md#dynamic-binding))，这意味着，会在运行时从运行时类型的表达式与要求的显式转换`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-369">The conversion is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), which means that an explicit conversion will be sought at run-time from the run-time type of the expression to `T`.</span></span> <span data-ttu-id="5c202-370">如果不找到任何转换，则引发运行时异常。</span><span class="sxs-lookup"><span data-stu-id="5c202-370">If no conversion is found, a run-time exception is thrown.</span></span>

<span data-ttu-id="5c202-371">如果不想转换的动态绑定，该表达式可以首先转换为`object`，然后到所需的类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-371">If dynamic binding of the conversion is not desired, the expression can be first converted to `object`, and then to the desired type.</span></span>

<span data-ttu-id="5c202-372">假设以下类的定义：</span><span class="sxs-lookup"><span data-stu-id="5c202-372">Assume the following class is defined:</span></span>
```csharp
class C
{
    int i;

    public C(int i) { this.i = i; }

    public static explicit operator C(string s) 
    {
        return new C(int.Parse(s));
    }
}
```

<span data-ttu-id="5c202-373">下面的示例说明了动态的显式转换：</span><span class="sxs-lookup"><span data-stu-id="5c202-373">The following example illustrates explicit dynamic conversions:</span></span>
```csharp
object o  = "1";
dynamic d = "2";

var c1 = (C)o; // Compiles, but explicit reference conversion fails
var c2 = (C)d; // Compiles and user defined conversion succeeds
```

<span data-ttu-id="5c202-374">最佳的转换`o`到`C`找到在编译时为显式引用转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-374">The best conversion of `o` to `C` is found at compile-time to be an explicit reference conversion.</span></span> <span data-ttu-id="5c202-375">因为在运行时失败`"1"`实际上不是`C`。</span><span class="sxs-lookup"><span data-stu-id="5c202-375">This fails at run-time, because `"1"` is not in fact a `C`.</span></span> <span data-ttu-id="5c202-376">转换`d`到`C`但是，为显式的动态转换，挂起到运行时，其中的用户定义的运行时类型转换`d`  --  `string` -为`C`找到，则将会成功。</span><span class="sxs-lookup"><span data-stu-id="5c202-376">The conversion of `d` to `C` however, as an explicit dynamic conversion, is suspended to run-time, where a user defined conversion from the run-time type of `d` -- `string` -- to `C` is found, and succeeds.</span></span>

### <a name="explicit-conversions-involving-type-parameters"></a><span data-ttu-id="5c202-377">涉及类型参数的显式转换</span><span class="sxs-lookup"><span data-stu-id="5c202-377">Explicit conversions involving type parameters</span></span>

<span data-ttu-id="5c202-378">以下的显式转换为给定的类型参数存在`T`:</span><span class="sxs-lookup"><span data-stu-id="5c202-378">The following explicit conversions exist for a given type parameter `T`:</span></span>

*  <span data-ttu-id="5c202-379">从有效基类`C`的`T`到`T`并从任何基类`C`到`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-379">From the effective base class `C` of `T` to `T` and from any base class of `C` to `T`.</span></span> <span data-ttu-id="5c202-380">在运行时，如果`T`是值类型，则转换将执行作为取消装箱转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-380">At run-time, if `T` is a value type, the conversion is executed as an unboxing conversion.</span></span> <span data-ttu-id="5c202-381">否则，转换将执行作为显式引用转换或标识转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-381">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="5c202-382">从任何接口类型到`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-382">From any interface type to `T`.</span></span> <span data-ttu-id="5c202-383">在运行时，如果`T`是值类型，则转换将执行作为取消装箱转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-383">At run-time, if `T` is a value type, the conversion is executed as an unboxing conversion.</span></span> <span data-ttu-id="5c202-384">否则，转换将执行作为显式引用转换或标识转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-384">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="5c202-385">从`T`到任何*interface_type* `I`尚没有隐式转换提供`T`到`I`。</span><span class="sxs-lookup"><span data-stu-id="5c202-385">From `T` to any *interface_type* `I` provided there is not already an implicit conversion from `T` to `I`.</span></span> <span data-ttu-id="5c202-386">在运行时，如果`T`是值类型，则转换将执行作为显式引用转换后跟装箱转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-386">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion followed by an explicit reference conversion.</span></span> <span data-ttu-id="5c202-387">否则，转换将执行作为显式引用转换或标识转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-387">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="5c202-388">从类型参数`U`到`T`提供`T`取决于`U`([类型参数约束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="5c202-388">From a type parameter `U` to `T`, provided `T` depends on `U` ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="5c202-389">在运行时，如果`U`为值类型，则`T`和`U`一定具有相同的类型并不执行任何转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-389">At run-time, if `U` is a value type, then `T` and `U` are necessarily the same type and no conversion is performed.</span></span> <span data-ttu-id="5c202-390">否则为如果`T`是值类型，则转换将执行作为取消装箱转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-390">Otherwise, if `T` is a value type, the conversion is executed as an unboxing conversion.</span></span> <span data-ttu-id="5c202-391">否则，转换将执行作为显式引用转换或标识转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-391">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>

<span data-ttu-id="5c202-392">如果`T`是已知为引用类型，则上面的转换所有归类为显式引用转换 ([显式引用转换](conversions.md#explicit-reference-conversions))。</span><span class="sxs-lookup"><span data-stu-id="5c202-392">If `T` is known to be a reference type, the conversions above are all classified as explicit reference conversions ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span> <span data-ttu-id="5c202-393">如果`T`是已知不是引用类型，则上面的转换将归类为取消装箱转换 ([取消装箱转换](conversions.md#unboxing-conversions))。</span><span class="sxs-lookup"><span data-stu-id="5c202-393">If `T` is not known to be a reference type, the conversions above are classified as unboxing conversions ([Unboxing conversions](conversions.md#unboxing-conversions)).</span></span>

<span data-ttu-id="5c202-394">上述规则不允许为非接口类型，直接从受约束的类型参数显式转换可能会令人惊讶。</span><span class="sxs-lookup"><span data-stu-id="5c202-394">The above rules do not permit a direct explicit conversion from an unconstrained type parameter to a non-interface type, which might be surprising.</span></span> <span data-ttu-id="5c202-395">此规则的原因是防止产生混乱并请清除此类转换的语义。</span><span class="sxs-lookup"><span data-stu-id="5c202-395">The reason for this rule is to prevent confusion and make the semantics of such conversions clear.</span></span> <span data-ttu-id="5c202-396">例如，请考虑以下声明：</span><span class="sxs-lookup"><span data-stu-id="5c202-396">For example, consider the following declaration:</span></span>
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)t;                // Error 
    }
}
```

<span data-ttu-id="5c202-397">如果直接显式转换`t`到`int`允许，极有可能认为该`X<int>.F(7)`将返回 `7L`。</span><span class="sxs-lookup"><span data-stu-id="5c202-397">If the direct explicit conversion of `t` to `int` were permitted, one might easily expect that `X<int>.F(7)` would return `7L`.</span></span> <span data-ttu-id="5c202-398">但是，不这样做，因为已知类型将是在绑定时数字时，将仅考虑标准数值转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-398">However, it would not, because the standard numeric conversions are only considered when the types are known to be numeric at binding-time.</span></span> <span data-ttu-id="5c202-399">为了使语义清晰、 上面的示例中必须改为编写：</span><span class="sxs-lookup"><span data-stu-id="5c202-399">In order to make the semantics clear, the above example must instead be written:</span></span>
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)(object)t;        // Ok, but will only work when T is long
    }
}
```

<span data-ttu-id="5c202-400">此代码现在可以正常编译但正在执行`X<int>.F(7)`将然后引发一个异常在运行时，由于一个装箱`int`不能直接转换`long`。</span><span class="sxs-lookup"><span data-stu-id="5c202-400">This code will now compile but executing `X<int>.F(7)` would then throw an exception at run-time, since a boxed `int` cannot be converted directly to a `long`.</span></span>

### <a name="user-defined-explicit-conversions"></a><span data-ttu-id="5c202-401">用户定义的显式转换</span><span class="sxs-lookup"><span data-stu-id="5c202-401">User-defined explicit conversions</span></span>

<span data-ttu-id="5c202-402">用户定义的显式转换包含的可选的标准显式转换后, 跟的一个用户定义的隐式或显式转换运算符后, 跟另一个可选的标准显式转换的执行。</span><span class="sxs-lookup"><span data-stu-id="5c202-402">A user-defined explicit conversion consists of an optional standard explicit conversion, followed by execution of a user-defined implicit or explicit conversion operator, followed by another optional standard explicit conversion.</span></span> <span data-ttu-id="5c202-403">用于评估用户定义的显式转换的确切规则所述[处理的用户定义的显式转换](conversions.md#processing-of-user-defined-explicit-conversions)。</span><span class="sxs-lookup"><span data-stu-id="5c202-403">The exact rules for evaluating user-defined explicit conversions are described in [Processing of user-defined explicit conversions](conversions.md#processing-of-user-defined-explicit-conversions).</span></span>

## <a name="standard-conversions"></a><span data-ttu-id="5c202-404">标准转换</span><span class="sxs-lookup"><span data-stu-id="5c202-404">Standard conversions</span></span>

<span data-ttu-id="5c202-405">标准转换将这些预定义的转换可能出现的用户定义的转换的一部分。</span><span class="sxs-lookup"><span data-stu-id="5c202-405">The standard conversions are those pre-defined conversions that can occur as part of a user-defined conversion.</span></span>

### <a name="standard-implicit-conversions"></a><span data-ttu-id="5c202-406">标准隐式转换</span><span class="sxs-lookup"><span data-stu-id="5c202-406">Standard implicit conversions</span></span>

<span data-ttu-id="5c202-407">以下隐式转换被归类为标准的隐式转换：</span><span class="sxs-lookup"><span data-stu-id="5c202-407">The following implicit conversions are classified as standard implicit conversions:</span></span>

*  <span data-ttu-id="5c202-408">标识转换 ([标识转换](conversions.md#identity-conversion))</span><span class="sxs-lookup"><span data-stu-id="5c202-408">Identity conversions ([Identity conversion](conversions.md#identity-conversion))</span></span>
*  <span data-ttu-id="5c202-409">隐式数值转换 ([隐式数值转换](conversions.md#implicit-numeric-conversions))</span><span class="sxs-lookup"><span data-stu-id="5c202-409">Implicit numeric conversions ([Implicit numeric conversions](conversions.md#implicit-numeric-conversions))</span></span>
*  <span data-ttu-id="5c202-410">可以为 null 的隐式转换 ([可以为 null 的隐式转换](conversions.md#implicit-nullable-conversions))</span><span class="sxs-lookup"><span data-stu-id="5c202-410">Implicit nullable conversions ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions))</span></span>
*  <span data-ttu-id="5c202-411">隐式引用转换 ([隐式引用转换](conversions.md#implicit-reference-conversions))</span><span class="sxs-lookup"><span data-stu-id="5c202-411">Implicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions))</span></span>
*  <span data-ttu-id="5c202-412">装箱转换 ([装箱转换](conversions.md#boxing-conversions))</span><span class="sxs-lookup"><span data-stu-id="5c202-412">Boxing conversions ([Boxing conversions](conversions.md#boxing-conversions))</span></span>
*  <span data-ttu-id="5c202-413">常量表达式隐式转换 ([隐式动态转换](conversions.md#implicit-dynamic-conversions))</span><span class="sxs-lookup"><span data-stu-id="5c202-413">Implicit constant expression conversions ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions))</span></span>
*  <span data-ttu-id="5c202-414">隐式转换涉及类型参数 ([涉及类型参数的隐式转换](conversions.md#implicit-conversions-involving-type-parameters))</span><span class="sxs-lookup"><span data-stu-id="5c202-414">Implicit conversions involving type parameters ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters))</span></span>

<span data-ttu-id="5c202-415">标准的隐式转换专门排除用户定义的隐式转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-415">The standard implicit conversions specifically exclude user-defined implicit conversions.</span></span>

### <a name="standard-explicit-conversions"></a><span data-ttu-id="5c202-416">标准的显式转换</span><span class="sxs-lookup"><span data-stu-id="5c202-416">Standard explicit conversions</span></span>

<span data-ttu-id="5c202-417">标准的显式转换包括所有标准的隐式转换以及子网的相反标准隐式转换为其存在显式转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-417">The standard explicit conversions are all standard implicit conversions plus the subset of the explicit conversions for which an opposite standard implicit conversion exists.</span></span> <span data-ttu-id="5c202-418">换而言之，如果标准隐式转换存在从类型`A`为某种`B`，然后从类型存在标准的显式转换`A`键入`B`和从类型`B`键入`A`。</span><span class="sxs-lookup"><span data-stu-id="5c202-418">In other words, if a standard implicit conversion exists from a type `A` to a type `B`, then a standard explicit conversion exists from type `A` to type `B` and from type `B` to type `A`.</span></span>

## <a name="user-defined-conversions"></a><span data-ttu-id="5c202-419">用户定义的转换</span><span class="sxs-lookup"><span data-stu-id="5c202-419">User-defined conversions</span></span>

<span data-ttu-id="5c202-420">C# 允许将预定义的隐式和显式转换，以通过扩充***用户定义的转换***。</span><span class="sxs-lookup"><span data-stu-id="5c202-420">C# allows the pre-defined implicit and explicit conversions to be augmented by ***user-defined conversions***.</span></span> <span data-ttu-id="5c202-421">用户定义的转换引入的声明转换运算符 ([转换运算符](classes.md#conversion-operators)) 中类和结构类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-421">User-defined conversions are introduced by declaring conversion operators ([Conversion operators](classes.md#conversion-operators)) in class and struct types.</span></span>

### <a name="permitted-user-defined-conversions"></a><span data-ttu-id="5c202-422">允许用户定义的转换</span><span class="sxs-lookup"><span data-stu-id="5c202-422">Permitted user-defined conversions</span></span>

<span data-ttu-id="5c202-423">C# 允许仅特定用户定义的转换声明。</span><span class="sxs-lookup"><span data-stu-id="5c202-423">C# permits only certain user-defined conversions to be declared.</span></span> <span data-ttu-id="5c202-424">具体而言，不能重新定义已存在的隐式或显式转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-424">In particular, it is not possible to redefine an already existing implicit or explicit conversion.</span></span>

<span data-ttu-id="5c202-425">给定的源类型`S`和目标类型`T`，如果`S`或`T`是可以为 null 的类型，让`S0`并`T0`指其基础类型，否则`S0`和`T0`是等于`S`和`T`分别。</span><span class="sxs-lookup"><span data-stu-id="5c202-425">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="5c202-426">类或结构允许以声明的源类型转换`S`为目标类型`T`只有在满足以下所有时：</span><span class="sxs-lookup"><span data-stu-id="5c202-426">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="5c202-427">`S0` 和`T0`是不同的类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-427">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="5c202-428">要么`S0`或`T0`是在运算符声明发生的类或结构类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-428">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="5c202-429">既不`S0`也不`T0`是*interface_type*。</span><span class="sxs-lookup"><span data-stu-id="5c202-429">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="5c202-430">除用户定义的转换，转换不存在从`S`到`T`或从`T`到`S`。</span><span class="sxs-lookup"><span data-stu-id="5c202-430">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="5c202-431">应用到用户定义的转换的限制的讨论中进一步[转换运算符](classes.md#conversion-operators)。</span><span class="sxs-lookup"><span data-stu-id="5c202-431">The restrictions that apply to user-defined conversions are discussed further in [Conversion operators](classes.md#conversion-operators).</span></span>

### <a name="lifted-conversion-operators"></a><span data-ttu-id="5c202-432">提升的转换运算符</span><span class="sxs-lookup"><span data-stu-id="5c202-432">Lifted conversion operators</span></span>

<span data-ttu-id="5c202-433">提供将从非 null 的值类型转换的用户定义的转换运算符`S`给不可以为 null 的值类型`T`、 一个***提升转换运算符***存在从将`S?`到`T?`.</span><span class="sxs-lookup"><span data-stu-id="5c202-433">Given a user-defined conversion operator that converts from a non-nullable value type `S` to a non-nullable value type `T`, a ***lifted conversion operator*** exists that converts from `S?` to `T?`.</span></span> <span data-ttu-id="5c202-434">此提升的转换运算符执行从`S?`到`S`从用户定义的转换后跟`S`到`T`跟从一个包装`T`到`T?`，只不过 null值`S?`直接与 null 值转换为`T?`。</span><span class="sxs-lookup"><span data-stu-id="5c202-434">This lifted conversion operator performs an unwrapping from `S?` to `S` followed by the user-defined conversion from `S` to `T` followed by a wrapping from `T` to `T?`, except that a null valued `S?` converts directly to a null valued `T?`.</span></span>

<span data-ttu-id="5c202-435">提升的转换运算符具有相同隐式或显式作为其基础的用户定义的转换运算符的分类。</span><span class="sxs-lookup"><span data-stu-id="5c202-435">A lifted conversion operator has the same implicit or explicit classification as its underlying user-defined conversion operator.</span></span> <span data-ttu-id="5c202-436">术语"用户定义的转换"适用于对这两者的使用用户定义和提升转换运算符。</span><span class="sxs-lookup"><span data-stu-id="5c202-436">The term "user-defined conversion" applies to the use of both user-defined and lifted conversion operators.</span></span>

### <a name="evaluation-of-user-defined-conversions"></a><span data-ttu-id="5c202-437">用户定义的转换的评估</span><span class="sxs-lookup"><span data-stu-id="5c202-437">Evaluation of user-defined conversions</span></span>

<span data-ttu-id="5c202-438">用户定义的转换将值转换其名为的类型从***源类型***，到另一种类型，称为***目标类型***。</span><span class="sxs-lookup"><span data-stu-id="5c202-438">A user-defined conversion converts a value from its type, called the ***source type***, to another type, called the ***target type***.</span></span> <span data-ttu-id="5c202-439">有关查找的用户定义的转换的评估中心***最具体***特定的源和目标类型的用户定义的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="5c202-439">Evaluation of a user-defined conversion centers on finding the ***most specific*** user-defined conversion operator for the particular source and target types.</span></span> <span data-ttu-id="5c202-440">此决定分为几个步骤：</span><span class="sxs-lookup"><span data-stu-id="5c202-440">This determination is broken into several steps:</span></span>

*  <span data-ttu-id="5c202-441">查找类和结构将被视为用户定义的转换运算符的集。</span><span class="sxs-lookup"><span data-stu-id="5c202-441">Finding the set of classes and structs from which user-defined conversion operators will be considered.</span></span> <span data-ttu-id="5c202-442">此集包含的源类型和及其基类和目标类型和及其基类 （与仅类和结构可以声明用户定义的运算符，并且非类类型不具有没有基类的隐式假设）。</span><span class="sxs-lookup"><span data-stu-id="5c202-442">This set consists of the source type and its base classes and the target type and its base classes (with the implicit assumptions that only classes and structs can declare user-defined operators, and that non-class types have no base classes).</span></span> <span data-ttu-id="5c202-443">此步骤中，如果源或目标类型是出于*nullable_type*，其基础类型改为使用。</span><span class="sxs-lookup"><span data-stu-id="5c202-443">For the purposes of this step, if either the source or target type is a *nullable_type*, their underlying type is used instead.</span></span>
*  <span data-ttu-id="5c202-444">从该集中的类型确定的用户和提升转换运算符都适用。</span><span class="sxs-lookup"><span data-stu-id="5c202-444">From that set of types, determining which user-defined and lifted conversion operators are applicable.</span></span> <span data-ttu-id="5c202-445">转换运算符适用，它必须能够执行的标准转换 ([标准转换](conversions.md#standard-conversions)) 从源类型到操作数的运算符和它的类型必须是可以执行的标准转换从为目标类型的运算符的结果类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-445">For a conversion operator to be applicable, it must be possible to perform a standard conversion ([Standard conversions](conversions.md#standard-conversions)) from the source type to the operand type of the operator, and it must be possible to perform a standard conversion from the result type of the operator to the target type.</span></span>
*  <span data-ttu-id="5c202-446">从适用的用户定义运算符，确定哪一个运算符明确最具体的组。</span><span class="sxs-lookup"><span data-stu-id="5c202-446">From the set of applicable user-defined operators, determining which operator is unambiguously the most specific.</span></span> <span data-ttu-id="5c202-447">概括地说，最具体的运算符是的运算符的操作数类型是"最靠近"的源类型，并且结果类型为"最靠近"目标类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-447">In general terms, the most specific operator is the operator whose operand type is "closest" to the source type and whose result type is "closest" to the target type.</span></span> <span data-ttu-id="5c202-448">用户定义的转换运算符是首选，通过提升的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="5c202-448">User-defined conversion operators are preferred over lifted conversion operators.</span></span> <span data-ttu-id="5c202-449">以下各节中定义的建立最具体的用户定义的转换运算符的确切规则。</span><span class="sxs-lookup"><span data-stu-id="5c202-449">The exact rules for establishing the most specific user-defined conversion operator are defined in the following sections.</span></span>

<span data-ttu-id="5c202-450">一旦确定最特定的用户定义的转换运算符，实际执行用户定义的转换涉及到最多三个步骤：</span><span class="sxs-lookup"><span data-stu-id="5c202-450">Once a most specific user-defined conversion operator has been identified, the actual execution of the user-defined conversion involves up to three steps:</span></span>

*  <span data-ttu-id="5c202-451">首先，如果需要，请执行从源类型到用户定义的或提升转换运算符的操作数类型的标准转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-451">First, if required, performing a standard conversion from the source type to the operand type of the user-defined or lifted conversion operator.</span></span>
*  <span data-ttu-id="5c202-452">接下来，调用用户定义或提升转换运算符来执行此转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-452">Next, invoking the user-defined or lifted conversion operator to perform the conversion.</span></span>
*  <span data-ttu-id="5c202-453">最后，如果需要，请执行用户定义的或提升转换运算符的结果类型中的标准转换为目标类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-453">Finally, if required, performing a standard conversion from the result type of the user-defined or lifted conversion operator to the target type.</span></span>

<span data-ttu-id="5c202-454">计算的用户定义转换永远不会涉及多个用户定义或提升转换运算符。</span><span class="sxs-lookup"><span data-stu-id="5c202-454">Evaluation of a user-defined conversion never involves more than one user-defined or lifted conversion operator.</span></span> <span data-ttu-id="5c202-455">换而言之，从类型转换`S`键入`T`永远不会首先执行中的用户定义的转换`S`到`X`，然后执行用户定义的转换，从`X`到`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-455">In other words, a conversion from type `S` to type `T` will never first execute a user-defined conversion from `S` to `X` and then execute a user-defined conversion from `X` to `T`.</span></span>

<span data-ttu-id="5c202-456">以下各节给出的用户定义的隐式或显式转换的确切定义。</span><span class="sxs-lookup"><span data-stu-id="5c202-456">Exact definitions of evaluation of user-defined implicit or explicit conversions are given in the following sections.</span></span> <span data-ttu-id="5c202-457">定义可使用以下术语：</span><span class="sxs-lookup"><span data-stu-id="5c202-457">The definitions make use of the following terms:</span></span>

*  <span data-ttu-id="5c202-458">如果标准隐式转换 ([标准隐式转换](conversions.md#standard-implicit-conversions)) 存在从类型`A`为某种`B`，和如果既没有`A`也不`B`是*interface_type*s，然后`A`称为***包含*** `B`，并且`B`可以说负责***包含*** `A`。</span><span class="sxs-lookup"><span data-stu-id="5c202-458">If a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) exists from a type `A` to a type `B`, and if neither `A` nor `B` are *interface_type*s, then `A` is said to be ***encompassed by*** `B`, and `B` is said to ***encompass*** `A`.</span></span>
*  <span data-ttu-id="5c202-459">***最大的类型***中的一组类型是一个包含在集中的所有其他类型的类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-459">The ***most encompassing type*** in a set of types is the one type that encompasses all other types in the set.</span></span> <span data-ttu-id="5c202-460">如果没有一个类型包含所有其他类型，然后集都有没有最大的类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-460">If no single type encompasses all other types, then the set has no most encompassing type.</span></span> <span data-ttu-id="5c202-461">更直观地讲，最大的类型是在集中的"最大"类型，每个其他类型可以隐式转换成一种类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-461">In more intuitive terms, the most encompassing type is the "largest" type in the set—the one type to which each of the other types can be implicitly converted.</span></span>
*  <span data-ttu-id="5c202-462">***最包含类型***在一组类型是在集中的所有其他类型包含一个类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-462">The ***most encompassed type*** in a set of types is the one type that is encompassed by all other types in the set.</span></span> <span data-ttu-id="5c202-463">如果没有一个类型被包含所有其他类型，一组具有最不包含类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-463">If no single type is encompassed by all other types, then the set has no most encompassed type.</span></span> <span data-ttu-id="5c202-464">更直观地讲，包含程度最大的类型是在集中的"最小"类型，可以隐式转换为其他类型的每一种类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-464">In more intuitive terms, the most encompassed type is the "smallest" type in the set—the one type that can be implicitly converted to each of the other types.</span></span>

### <a name="processing-of-user-defined-implicit-conversions"></a><span data-ttu-id="5c202-465">处理的用户定义的隐式转换</span><span class="sxs-lookup"><span data-stu-id="5c202-465">Processing of user-defined implicit conversions</span></span>

<span data-ttu-id="5c202-466">从类型的用户定义的隐式转换`S`键入`T`进行处理，如下所示：</span><span class="sxs-lookup"><span data-stu-id="5c202-466">A user-defined implicit conversion from type `S` to type `T` is processed as follows:</span></span>

*  <span data-ttu-id="5c202-467">确定的类型`S0`和`T0`。</span><span class="sxs-lookup"><span data-stu-id="5c202-467">Determine the types `S0` and `T0`.</span></span> <span data-ttu-id="5c202-468">如果`S`或`T`是可以为 null 的类型，`S0`和`T0`是它们的基础类型，否则`S0`并`T0`等于`S`和`T`分别。</span><span class="sxs-lookup"><span data-stu-id="5c202-468">If `S` or `T` are nullable types, `S0` and `T0` are their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span>
*  <span data-ttu-id="5c202-469">查找组的类型， `D`，将被视为哪些用户定义的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="5c202-469">Find the set of types, `D`, from which user-defined conversion operators will be considered.</span></span> <span data-ttu-id="5c202-470">此集组成`S0`(如果`S0`是类或结构)，类的基类`S0`(如果`S0`是一个类)，并`T0`(如果`T0`是类或结构)。</span><span class="sxs-lookup"><span data-stu-id="5c202-470">This set consists of `S0` (if `S0` is a class or struct), the base classes of `S0` (if `S0` is a class), and `T0` (if `T0` is a class or struct).</span></span>
*  <span data-ttu-id="5c202-471">查找适用的用户和提升转换运算符的组`U`。</span><span class="sxs-lookup"><span data-stu-id="5c202-471">Find the set of applicable user-defined and lifted conversion operators, `U`.</span></span> <span data-ttu-id="5c202-472">此集包含的类或结构中的声明的用户和提升隐式转换运算符`D`，将从包含的类型转换`S`包含一个类型为`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-472">This set consists of the user-defined and lifted implicit conversion operators declared by the classes or structs in `D` that convert from a type encompassing `S` to a type encompassed by `T`.</span></span> <span data-ttu-id="5c202-473">如果`U`是空的转换未定义，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="5c202-473">If `U` is empty, the conversion is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="5c202-474">查找最具体的源类型`SX`中的运算符`U`:</span><span class="sxs-lookup"><span data-stu-id="5c202-474">Find the most specific source type, `SX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="5c202-475">如果任何中的运算符`U`将从转换`S`，然后`SX`是`S`。</span><span class="sxs-lookup"><span data-stu-id="5c202-475">If any of the operators in `U` convert from `S`, then `SX` is `S`.</span></span>
    * <span data-ttu-id="5c202-476">否则为`SX`是包含程度最大的类型中的运算符的源类型的组合集`U`。</span><span class="sxs-lookup"><span data-stu-id="5c202-476">Otherwise, `SX` is the most encompassed type in the combined set of source types of the operators in `U`.</span></span> <span data-ttu-id="5c202-477">如果一个最包含找不到类型，然后转换不明确，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="5c202-477">If exactly one most encompassed type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="5c202-478">查找最具体的目标类型`TX`中的运算符`U`:</span><span class="sxs-lookup"><span data-stu-id="5c202-478">Find the most specific target type, `TX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="5c202-479">如果任何中的运算符`U`将转换为`T`，然后`TX`是`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-479">If any of the operators in `U` convert to `T`, then `TX` is `T`.</span></span>
    * <span data-ttu-id="5c202-480">否则为`TX`是最大的类型中的目标类型中的运算符的组合集`U`。</span><span class="sxs-lookup"><span data-stu-id="5c202-480">Otherwise, `TX` is the most encompassing type in the combined set of target types of the operators in `U`.</span></span> <span data-ttu-id="5c202-481">如果找不到一个最大的类型，则转换是不明确，并将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="5c202-481">If exactly one most encompassing type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="5c202-482">查找最精确的转换运算符：</span><span class="sxs-lookup"><span data-stu-id="5c202-482">Find the most specific conversion operator:</span></span>
    * <span data-ttu-id="5c202-483">如果`U`包含一个将从转换的用户定义的转换运算符`SX`到`TX`，则表明这是最具体的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="5c202-483">If `U` contains exactly one user-defined conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="5c202-484">否则为如果`U`包含一个从转换的提升的转换运算符`SX`到`TX`，则表明这是最具体的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="5c202-484">Otherwise, if `U` contains exactly one lifted conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="5c202-485">否则为转换不明确，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="5c202-485">Otherwise, the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="5c202-486">最后，将应用转换：</span><span class="sxs-lookup"><span data-stu-id="5c202-486">Finally, apply the conversion:</span></span>
    * <span data-ttu-id="5c202-487">如果`S`不是`SX`，然后从标准隐式转换`S`到`SX`执行。</span><span class="sxs-lookup"><span data-stu-id="5c202-487">If `S` is not `SX`, then a standard implicit conversion from `S` to `SX` is performed.</span></span>
    * <span data-ttu-id="5c202-488">最具体的转换运算符调用以从转换`SX`到`TX`。</span><span class="sxs-lookup"><span data-stu-id="5c202-488">The most specific conversion operator is invoked to convert from `SX` to `TX`.</span></span>
    * <span data-ttu-id="5c202-489">如果`TX`不是`T`，然后从标准隐式转换`TX`到`T`执行。</span><span class="sxs-lookup"><span data-stu-id="5c202-489">If `TX` is not `T`, then a standard implicit conversion from `TX` to `T` is performed.</span></span>

### <a name="processing-of-user-defined-explicit-conversions"></a><span data-ttu-id="5c202-490">处理的用户定义的显式转换</span><span class="sxs-lookup"><span data-stu-id="5c202-490">Processing of user-defined explicit conversions</span></span>

<span data-ttu-id="5c202-491">从类型的用户定义的显式转换`S`键入`T`进行处理，如下所示：</span><span class="sxs-lookup"><span data-stu-id="5c202-491">A user-defined explicit conversion from type `S` to type `T` is processed as follows:</span></span>

*  <span data-ttu-id="5c202-492">确定的类型`S0`和`T0`。</span><span class="sxs-lookup"><span data-stu-id="5c202-492">Determine the types `S0` and `T0`.</span></span> <span data-ttu-id="5c202-493">如果`S`或`T`是可以为 null 的类型，`S0`和`T0`是它们的基础类型，否则`S0`并`T0`等于`S`和`T`分别。</span><span class="sxs-lookup"><span data-stu-id="5c202-493">If `S` or `T` are nullable types, `S0` and `T0` are their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span>
*  <span data-ttu-id="5c202-494">查找组的类型， `D`，将被视为哪些用户定义的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="5c202-494">Find the set of types, `D`, from which user-defined conversion operators will be considered.</span></span> <span data-ttu-id="5c202-495">此集组成`S0`(如果`S0`是类或结构)，类的基类`S0`(如果`S0`是一个类)， `T0` (如果`T0`是类或结构)，和的基类`T0`(如果`T0`是一个类)。</span><span class="sxs-lookup"><span data-stu-id="5c202-495">This set consists of `S0` (if `S0` is a class or struct), the base classes of `S0` (if `S0` is a class), `T0` (if `T0` is a class or struct), and the base classes of `T0` (if `T0` is a class).</span></span>
*  <span data-ttu-id="5c202-496">查找适用的用户和提升转换运算符的组`U`。</span><span class="sxs-lookup"><span data-stu-id="5c202-496">Find the set of applicable user-defined and lifted conversion operators, `U`.</span></span> <span data-ttu-id="5c202-497">此集包含用户和提升的隐式或显式转换运算符声明为类或结构中的`D`，将从包含的类型转换或包含`S`到包含自或包含的类型`T`.</span><span class="sxs-lookup"><span data-stu-id="5c202-497">This set consists of the user-defined and lifted implicit or explicit conversion operators declared by the classes or structs in `D` that convert from a type encompassing or encompassed by `S` to a type encompassing or encompassed by `T`.</span></span> <span data-ttu-id="5c202-498">如果`U`是空的转换未定义，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="5c202-498">If `U` is empty, the conversion is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="5c202-499">查找最具体的源类型`SX`中的运算符`U`:</span><span class="sxs-lookup"><span data-stu-id="5c202-499">Find the most specific source type, `SX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="5c202-500">如果任何中的运算符`U`将从转换`S`，然后`SX`是`S`。</span><span class="sxs-lookup"><span data-stu-id="5c202-500">If any of the operators in `U` convert from `S`, then `SX` is `S`.</span></span>
    * <span data-ttu-id="5c202-501">否则为如果中的运算符的任何`U`从包含的类型转换`S`，然后`SX`是中的源类型的这些运算符的组合集包含程度最大的类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-501">Otherwise, if any of the operators in `U` convert from types that encompass `S`, then `SX` is the most encompassed type in the combined set of source types of those operators.</span></span> <span data-ttu-id="5c202-502">如果最直接包含找不到类型，然后转换不明确，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="5c202-502">If no most encompassed type can be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
    * <span data-ttu-id="5c202-503">否则为`SX`是最大的类型中的源类型中的运算符的组合集`U`。</span><span class="sxs-lookup"><span data-stu-id="5c202-503">Otherwise, `SX` is the most encompassing type in the combined set of source types of the operators in `U`.</span></span> <span data-ttu-id="5c202-504">如果找不到一个最大的类型，则转换是不明确，并将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="5c202-504">If exactly one most encompassing type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="5c202-505">查找最具体的目标类型`TX`中的运算符`U`:</span><span class="sxs-lookup"><span data-stu-id="5c202-505">Find the most specific target type, `TX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="5c202-506">如果任何中的运算符`U`将转换为`T`，然后`TX`是`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-506">If any of the operators in `U` convert to `T`, then `TX` is `T`.</span></span>
    * <span data-ttu-id="5c202-507">否则为如果中的运算符的任何`U`将转换为类型通过产生了哪些`T`，然后`TX`是最大的类型中的目标类型的这些运算符的组合集。</span><span class="sxs-lookup"><span data-stu-id="5c202-507">Otherwise, if any of the operators in `U` convert to types that are encompassed by `T`, then `TX` is the most encompassing type in the combined set of target types of those operators.</span></span> <span data-ttu-id="5c202-508">如果找不到一个最大的类型，则转换是不明确，并将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="5c202-508">If exactly one most encompassing type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
    * <span data-ttu-id="5c202-509">否则为`TX`是包含程度最大的类型的目标类型中的运算符的组合集`U`。</span><span class="sxs-lookup"><span data-stu-id="5c202-509">Otherwise, `TX` is the most encompassed type in the combined set of target types of the operators in `U`.</span></span> <span data-ttu-id="5c202-510">如果最直接包含找不到类型，然后转换不明确，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="5c202-510">If no most encompassed type can be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="5c202-511">查找最精确的转换运算符：</span><span class="sxs-lookup"><span data-stu-id="5c202-511">Find the most specific conversion operator:</span></span>
    * <span data-ttu-id="5c202-512">如果`U`包含一个将从转换的用户定义的转换运算符`SX`到`TX`，则表明这是最具体的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="5c202-512">If `U` contains exactly one user-defined conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="5c202-513">否则为如果`U`包含一个从转换的提升的转换运算符`SX`到`TX`，则表明这是最具体的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="5c202-513">Otherwise, if `U` contains exactly one lifted conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="5c202-514">否则为转换不明确，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="5c202-514">Otherwise, the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="5c202-515">最后，将应用转换：</span><span class="sxs-lookup"><span data-stu-id="5c202-515">Finally, apply the conversion:</span></span>
    * <span data-ttu-id="5c202-516">如果`S`不是`SX`，然后从标准显式转换`S`到`SX`执行。</span><span class="sxs-lookup"><span data-stu-id="5c202-516">If `S` is not `SX`, then a standard explicit conversion from `S` to `SX` is performed.</span></span>
    * <span data-ttu-id="5c202-517">调用的最特定的用户定义的转换运算符，以转换从`SX`到`TX`。</span><span class="sxs-lookup"><span data-stu-id="5c202-517">The most specific user-defined conversion operator is invoked to convert from `SX` to `TX`.</span></span>
    * <span data-ttu-id="5c202-518">如果`TX`不是`T`，然后从标准显式转换`TX`到`T`执行。</span><span class="sxs-lookup"><span data-stu-id="5c202-518">If `TX` is not `T`, then a standard explicit conversion from `TX` to `T` is performed.</span></span>

## <a name="anonymous-function-conversions"></a><span data-ttu-id="5c202-519">匿名函数转换</span><span class="sxs-lookup"><span data-stu-id="5c202-519">Anonymous function conversions</span></span>

<span data-ttu-id="5c202-520">*Anonymous_method_expression*或*lambda_expression*归类为匿名函数 ([匿名函数表达式](expressions.md#anonymous-function-expressions))。</span><span class="sxs-lookup"><span data-stu-id="5c202-520">An *anonymous_method_expression* or *lambda_expression* is classified as an anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="5c202-521">表达式不具有一种类型，但可以隐式转换为兼容的委托类型或表达式树类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-521">The expression does not have a type but can be implicitly converted to a compatible delegate type or expression tree type.</span></span> <span data-ttu-id="5c202-522">具体而言，一个匿名函数`F`委托类型与兼容`D`提供：</span><span class="sxs-lookup"><span data-stu-id="5c202-522">Specifically, an anonymous function `F` is compatible with a delegate type `D` provided:</span></span>

*  <span data-ttu-id="5c202-523">如果`F`包含*anonymous_function_signature*，然后`D`和`F`具有相同数量的参数。</span><span class="sxs-lookup"><span data-stu-id="5c202-523">If `F` contains an *anonymous_function_signature*, then `D` and `F` have the same number of parameters.</span></span>
*  <span data-ttu-id="5c202-524">如果`F`不包含*anonymous_function_signature*，然后`D`可能有零个或多个参数的任何类型的任何参数，只要`D`具有`out`参数修饰符。</span><span class="sxs-lookup"><span data-stu-id="5c202-524">If `F` does not contain an *anonymous_function_signature*, then `D` may have zero or more parameters of any type, as long as no parameter of `D` has the `out` parameter modifier.</span></span>
*  <span data-ttu-id="5c202-525">如果`F`具有显式类型化的参数列表中，在每个参数`D`中的相应参数具有相同的类型和修饰符`F`。</span><span class="sxs-lookup"><span data-stu-id="5c202-525">If `F` has an explicitly typed parameter list, each parameter in `D` has the same type and modifiers as the corresponding parameter in `F`.</span></span>
*  <span data-ttu-id="5c202-526">如果`F`具有隐式类型化的参数列表，`D`不具有`ref`或`out`参数。</span><span class="sxs-lookup"><span data-stu-id="5c202-526">If `F` has an implicitly typed parameter list, `D` has no `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="5c202-527">如果正文`F`是一个表达式，并且要么`D`具有`void`返回类型或`F`是异步和`D`具有返回类型`Task`，然后当的每个参数`F`给定的类型中的相应参数`D`的正文`F`是有效的表达式 (wrt[表达式](expressions.md))，将允许作为*statement_expression* ([表达式语句](statements.md#expression-statements))。</span><span class="sxs-lookup"><span data-stu-id="5c202-527">If the body of `F` is an expression, and either `D` has a `void` return type or `F` is async and `D` has the return type `Task`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid expression (wrt [Expressions](expressions.md)) that would be permitted as a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>
*  <span data-ttu-id="5c202-528">如果正文`F`是语句块，并且要么`D`已`void`返回类型或`F`是异步和`D`具有返回类型`Task`，然后当的每个参数`F`给定的类型中的相应参数`D`的正文`F`是一个有效的语句块 (wrt[块](statements.md#blocks)) 无`return`语句指定一个表达式。</span><span class="sxs-lookup"><span data-stu-id="5c202-528">If the body of `F` is a statement block, and either `D` has a `void` return type or `F` is async and `D` has the return type `Task`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid statement block (wrt [Blocks](statements.md#blocks)) in which no `return` statement specifies an expression.</span></span>
*  <span data-ttu-id="5c202-529">如果正文`F`是一个表达式，并*任一*`F`是非异步并`D`具有非 void 返回类型`T`，*或*`F`是异步和`D`具有返回类型`Task<T>`，然后当的每个参数`F`提供的中的相应参数类型`D`，正文`F`是有效的表达式 (wrt [表达式](expressions.md)) 这是隐式转换为`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-529">If the body of `F` is an expression, and *either* `F` is non-async and `D` has a non-void return type `T`, *or* `F` is async and `D` has a return type `Task<T>`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid expression (wrt [Expressions](expressions.md)) that is implicitly convertible to `T`.</span></span>
*  <span data-ttu-id="5c202-530">如果正文`F`是一个语句块，并*任一*`F`是非异步和`D`具有非 void 返回类型`T`，*或*`F`是异步和`D`具有返回类型`Task<T>`，然后当的每个参数`F`提供的中的相应参数类型`D`，正文`F`是有效的语句块 (wrt[块](statements.md#blocks)) 使用了非可访问的终结点，其中，每个`return`语句指定一个表达式，隐式转换为`T`。</span><span class="sxs-lookup"><span data-stu-id="5c202-530">If the body of `F` is a statement block, and *either* `F` is non-async and `D` has a non-void return type `T`, *or* `F` is async and `D` has a return type `Task<T>`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid statement block (wrt [Blocks](statements.md#blocks)) with a non-reachable end point in which each `return` statement specifies an expression that is implicitly convertible to `T`.</span></span>

<span data-ttu-id="5c202-531">为了简洁起见，本部分使用的缩写形式的任务类型`Task`并`Task<T>`([异步函数](classes.md#async-functions))。</span><span class="sxs-lookup"><span data-stu-id="5c202-531">For the purpose of brevity, this section uses the short form for the task types `Task` and `Task<T>` ([Async functions](classes.md#async-functions)).</span></span>

<span data-ttu-id="5c202-532">Lambda 表达式`F`与表达式目录树类型兼容`Expression<D>`如果`F`与委托类型兼容`D`。</span><span class="sxs-lookup"><span data-stu-id="5c202-532">A lambda expression `F` is compatible with an expression tree type `Expression<D>` if `F` is compatible with the delegate type `D`.</span></span> <span data-ttu-id="5c202-533">请注意，这不适用于于匿名方法，只有 lambda 表达式。</span><span class="sxs-lookup"><span data-stu-id="5c202-533">Note that this does not apply to anonymous methods, only lambda expressions.</span></span>

<span data-ttu-id="5c202-534">某些 lambda 表达式无法转换为表达式树类型：即使转换*存在*，它在编译时失败。</span><span class="sxs-lookup"><span data-stu-id="5c202-534">Certain lambda expressions cannot be converted to expression tree types: Even though the conversion *exists*, it fails at compile-time.</span></span> <span data-ttu-id="5c202-535">这是如果 lambda 表达式：</span><span class="sxs-lookup"><span data-stu-id="5c202-535">This is the case if the lambda expression:</span></span>

*  <span data-ttu-id="5c202-536">具有*块*正文</span><span class="sxs-lookup"><span data-stu-id="5c202-536">Has a *block* body</span></span>
*  <span data-ttu-id="5c202-537">包含简单或复合赋值运算符</span><span class="sxs-lookup"><span data-stu-id="5c202-537">Contains simple or compound assignment operators</span></span>
*  <span data-ttu-id="5c202-538">包含动态绑定的表达式</span><span class="sxs-lookup"><span data-stu-id="5c202-538">Contains a dynamically bound expression</span></span>
*  <span data-ttu-id="5c202-539">为异步</span><span class="sxs-lookup"><span data-stu-id="5c202-539">Is async</span></span>

<span data-ttu-id="5c202-540">下面的示例使用泛型委托类型`Func<A,R>`它表示采用类型的自变量的函数`A`，并返回类型的值`R`:</span><span class="sxs-lookup"><span data-stu-id="5c202-540">The examples that follow use a generic delegate type `Func<A,R>` which represents a function that takes an argument of type `A` and returns a value of type `R`:</span></span>
```csharp
delegate R Func<A,R>(A arg);
```

<span data-ttu-id="5c202-541">在下面的赋值</span><span class="sxs-lookup"><span data-stu-id="5c202-541">In the assignments</span></span>
```csharp
Func<int,int> f1 = x => x + 1;                 // Ok

Func<int,double> f2 = x => x + 1;              // Ok

Func<double,int> f3 = x => x + 1;              // Error

Func<int, Task<int>> f4 = async x => x + 1;    // Ok
```
<span data-ttu-id="5c202-542">每个匿名函数的参数和返回类型将根据向其分配的匿名函数的变量的类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-542">the parameter and return types of each anonymous function are determined from the type of the variable to which the anonymous function is assigned.</span></span>

<span data-ttu-id="5c202-543">第一个分配已成功将匿名函数转换为委托类型`Func<int,int>`因为，当`x`给定类型`int`，`x+1`是隐式转换为键入的有效表达式`int`。</span><span class="sxs-lookup"><span data-stu-id="5c202-543">The first assignment successfully converts the anonymous function to the delegate type `Func<int,int>` because, when `x` is given type `int`, `x+1` is a valid expression that is implicitly convertible to type `int`.</span></span>

<span data-ttu-id="5c202-544">同样，第二个分配已成功转换为匿名函数的委托类型`Func<int,double>`因为的结果`x+1`(类型的`int`) 是隐式转换为键入`double`。</span><span class="sxs-lookup"><span data-stu-id="5c202-544">Likewise, the second assignment successfully converts the anonymous function to the delegate type `Func<int,double>` because the result of `x+1` (of type `int`) is implicitly convertible to type `double`.</span></span>

<span data-ttu-id="5c202-545">但是，第三个分配是编译时错误，因为，当`x`给定类型`double`的结果`x+1`(类型的`double`) 不是隐式转换为键入`int`。</span><span class="sxs-lookup"><span data-stu-id="5c202-545">However, the third assignment is a compile-time error because, when `x` is given type `double`, the result of `x+1` (of type `double`) is not implicitly convertible to type `int`.</span></span>

<span data-ttu-id="5c202-546">第四个分配已成功将匿名异步函数转换为委托类型`Func<int, Task<int>>`因为的结果`x+1`(类型的`int`) 隐式转换的结果类型为`int`的任务类型`Task<int>`.</span><span class="sxs-lookup"><span data-stu-id="5c202-546">The fourth assignment successfully converts the anonymous async function to the delegate type `Func<int, Task<int>>` because the result of `x+1` (of type `int`) is implicitly convertible to the result type `int` of the task type `Task<int>`.</span></span>

<span data-ttu-id="5c202-547">匿名函数可能会影响重载决策，并参与类型推理。</span><span class="sxs-lookup"><span data-stu-id="5c202-547">Anonymous functions may influence overload resolution, and participate in type inference.</span></span> <span data-ttu-id="5c202-548">请参阅[函数成员](expressions.md#function-members)的更多详细信息。</span><span class="sxs-lookup"><span data-stu-id="5c202-548">See [Function members](expressions.md#function-members) for further details.</span></span>

### <a name="evaluation-of-anonymous-function-conversions-to-delegate-types"></a><span data-ttu-id="5c202-549">匿名函数转换为委托类型的计算</span><span class="sxs-lookup"><span data-stu-id="5c202-549">Evaluation of anonymous function conversions to delegate types</span></span>

<span data-ttu-id="5c202-550">匿名函数转换为委托类型生成一个委托实例引用匿名函数，并捕获在计算时处于活动状态的外部变量的 （可能为空） 集。</span><span class="sxs-lookup"><span data-stu-id="5c202-550">Conversion of an anonymous function to a delegate type produces a delegate instance which references the anonymous function and the (possibly empty) set of captured outer variables that are active at the time of the evaluation.</span></span> <span data-ttu-id="5c202-551">当调用委托时，执行匿名函数的主体。</span><span class="sxs-lookup"><span data-stu-id="5c202-551">When the delegate is invoked, the body of the anonymous function is executed.</span></span> <span data-ttu-id="5c202-552">在正文中的代码执行中使用的捕获由该委托引用的外部变量。</span><span class="sxs-lookup"><span data-stu-id="5c202-552">The code in the body is executed using the set of captured outer variables referenced by the delegate.</span></span>

<span data-ttu-id="5c202-553">将匿名函数中生成的委托的调用列表包含单个条目。</span><span class="sxs-lookup"><span data-stu-id="5c202-553">The invocation list of a delegate produced from an anonymous function contains a single entry.</span></span> <span data-ttu-id="5c202-554">未指定确切的目标对象和目标方法的委托。</span><span class="sxs-lookup"><span data-stu-id="5c202-554">The exact target object and target method of the delegate are unspecified.</span></span> <span data-ttu-id="5c202-555">具体而言，没有指定的委托的目标对象是`null`，则`this`封闭函数成员或某个其他对象的值。</span><span class="sxs-lookup"><span data-stu-id="5c202-555">In particular, it is unspecified whether the target object of the delegate is `null`, the `this` value of the enclosing function member, or some other object.</span></span>

<span data-ttu-id="5c202-556">转换的语义上完全相同的匿名函数具有相同的捕获外部变量的实例 （可能为空） 集于相同的委托类型都是允许 （但不是必需的） 返回相同的委托实例。</span><span class="sxs-lookup"><span data-stu-id="5c202-556">Conversions of semantically identical anonymous functions with the same (possibly empty) set of captured outer variable instances to the same delegate types are permitted (but not required) to return the same delegate instance.</span></span> <span data-ttu-id="5c202-557">此处用语义上完全相同的术语来表示，在所有情况下，匿名函数的执行将产生相同的结果提供了相同的自变量。</span><span class="sxs-lookup"><span data-stu-id="5c202-557">The term semantically identical is used here to mean that execution of the anonymous functions will, in all cases, produce the same effects given the same arguments.</span></span> <span data-ttu-id="5c202-558">此规则允许如下所示进行优化的代码。</span><span class="sxs-lookup"><span data-stu-id="5c202-558">This rule permits code such as the following to be optimized.</span></span>

```csharp
delegate double Function(double x);

class Test
{
    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void F(double[] a, double[] b) {
        a = Apply(a, (double x) => Math.Sin(x));
        b = Apply(b, (double y) => Math.Sin(y));
        ...
    }
}
```

<span data-ttu-id="5c202-559">由于两个匿名函数委托具有相同 （空） 的集捕获的外部变量，并且由于是在语义上是相同的匿名函数，被允许编译器具有引用相同的目标方法的委托。</span><span class="sxs-lookup"><span data-stu-id="5c202-559">Since the two anonymous function delegates have the same (empty) set of captured outer variables, and since the anonymous functions are semantically identical, the compiler is permitted to have the delegates refer to the same target method.</span></span> <span data-ttu-id="5c202-560">实际上，编译器允许从这两种匿名函数表达式返回完全相同的委托实例。</span><span class="sxs-lookup"><span data-stu-id="5c202-560">Indeed, the compiler is permitted to return the very same delegate instance from both anonymous function expressions.</span></span>

### <a name="evaluation-of-anonymous-function-conversions-to-expression-tree-types"></a><span data-ttu-id="5c202-561">匿名函数转换为表达式树类型的计算</span><span class="sxs-lookup"><span data-stu-id="5c202-561">Evaluation of anonymous function conversions to expression tree types</span></span>

<span data-ttu-id="5c202-562">匿名函数转换为表达式目录树类型生成表达式树 ([表达式树类型](types.md#expression-tree-types))。</span><span class="sxs-lookup"><span data-stu-id="5c202-562">Conversion of an anonymous function to an expression tree type produces an expression tree ([Expression tree types](types.md#expression-tree-types)).</span></span> <span data-ttu-id="5c202-563">更确切地说，匿名函数转换的计算会导致构造的对象结构，它表示匿名函数本身的结构。</span><span class="sxs-lookup"><span data-stu-id="5c202-563">More precisely, evaluation of the anonymous function conversion leads to the construction of an object structure that represents the structure of the anonymous function itself.</span></span> <span data-ttu-id="5c202-564">表达式树中，以及用于创建它，确切的过程的确切结构是定义的实现。</span><span class="sxs-lookup"><span data-stu-id="5c202-564">The precise structure of the expression tree, as well as the exact process for creating it, are implementation defined.</span></span>

### <a name="implementation-example"></a><span data-ttu-id="5c202-565">实现示例</span><span class="sxs-lookup"><span data-stu-id="5c202-565">Implementation example</span></span>

<span data-ttu-id="5c202-566">本部分介绍从其他 C# 构造的角度的匿名函数转换的一个可能实现。</span><span class="sxs-lookup"><span data-stu-id="5c202-566">This section describes a possible implementation of anonymous function conversions in terms of other C# constructs.</span></span> <span data-ttu-id="5c202-567">此处所述的实现基于 Microsoft C# 编译器所使用的相同的原则，但它不是强制性的实现，也不是只有一个可能的。</span><span class="sxs-lookup"><span data-stu-id="5c202-567">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation, nor is it the only one possible.</span></span> <span data-ttu-id="5c202-568">它仅简要说明了一些转换为表达式树，因为它们的准确语义超出了本规范的范围。</span><span class="sxs-lookup"><span data-stu-id="5c202-568">It only briefly mentions conversions to expression trees, as their exact semantics are outside the scope of this specification.</span></span>

<span data-ttu-id="5c202-569">本部分的其余部分提供了多个包含具有不同特征的匿名函数的代码示例。</span><span class="sxs-lookup"><span data-stu-id="5c202-569">The remainder of this section gives several examples of code that contains anonymous functions with different characteristics.</span></span> <span data-ttu-id="5c202-570">对于每个示例中，提供了使用仅其他 C# 构造的代码到相应的翻译。</span><span class="sxs-lookup"><span data-stu-id="5c202-570">For each example, a corresponding translation to code that uses only other C# constructs is provided.</span></span> <span data-ttu-id="5c202-571">在示例中，标识符`D`假定由表示以下委托类型：</span><span class="sxs-lookup"><span data-stu-id="5c202-571">In the examples, the identifier `D` is assumed by represent the following delegate type:</span></span>
```csharp
public delegate void D();
```

<span data-ttu-id="5c202-572">匿名函数的最简单形式是捕获任何外部变量：</span><span class="sxs-lookup"><span data-stu-id="5c202-572">The simplest form of an anonymous function is one that captures no outer variables:</span></span>
```csharp
class Test
{
    static void F() {
        D d = () => { Console.WriteLine("test"); };
    }
}
```

<span data-ttu-id="5c202-573">这可以转换为引用在其中放置的匿名函数的代码的编译器生成的静态方法的委托实例化：</span><span class="sxs-lookup"><span data-stu-id="5c202-573">This can be translated to a delegate instantiation that references a compiler generated static method in which the code of the anonymous function is placed:</span></span>
```csharp
class Test
{
    static void F() {
        D d = new D(__Method1);
    }

    static void __Method1() {
        Console.WriteLine("test");
    }
}
```

<span data-ttu-id="5c202-574">以下示例中，在匿名函数引用的实例成员`this`:</span><span class="sxs-lookup"><span data-stu-id="5c202-574">In the following example, the anonymous function references instance members of `this`:</span></span>
```csharp
class Test
{
    int x;

    void F() {
        D d = () => { Console.WriteLine(x); };
    }
}
```

<span data-ttu-id="5c202-575">这可以转换为编译器生成的实例方法，其中包含匿名函数的代码：</span><span class="sxs-lookup"><span data-stu-id="5c202-575">This can be translated to a compiler generated instance method containing the code of the anonymous function:</span></span>
```csharp
class Test
{
    int x;

    void F() {
        D d = new D(__Method1);
    }

    void __Method1() {
        Console.WriteLine(x);
    }
}
```

<span data-ttu-id="5c202-576">在此示例中，匿名函数会捕获本地变量：</span><span class="sxs-lookup"><span data-stu-id="5c202-576">In this example, the anonymous function captures a local variable:</span></span>
```csharp
class Test
{
    void F() {
        int y = 123;
        D d = () => { Console.WriteLine(y); };
    }
}
```

<span data-ttu-id="5c202-577">本地变量的生存期现在必须至少扩展到匿名函数委托的生存期。</span><span class="sxs-lookup"><span data-stu-id="5c202-577">The lifetime of the local variable must now be extended to at least the lifetime of the anonymous function delegate.</span></span> <span data-ttu-id="5c202-578">这可以通过本地变量"提升"到编译器生成类的字段来实现。</span><span class="sxs-lookup"><span data-stu-id="5c202-578">This can be achieved by "hoisting" the local variable into a field of a compiler generated class.</span></span> <span data-ttu-id="5c202-579">本地变量的实例化 ([的本地变量实例化](expressions.md#instantiation-of-local-variables)) 然后对应于创建实例的编译器生成类，并访问本地变量对应于访问的实例中的字段编译器生成的类。</span><span class="sxs-lookup"><span data-stu-id="5c202-579">Instantiation of the local variable ([Instantiation of local variables](expressions.md#instantiation-of-local-variables)) then corresponds to creating an instance of the compiler generated class, and accessing the local variable corresponds to accessing a field in the instance of the compiler generated class.</span></span> <span data-ttu-id="5c202-580">此外，匿名函数会成为编译器生成类的实例方法：</span><span class="sxs-lookup"><span data-stu-id="5c202-580">Furthermore, the anonymous function becomes an instance method of the compiler generated class:</span></span>
```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.y = 123;
        D d = new D(__locals1.__Method1);
    }

    class __Locals1
    {
        public int y;

        public void __Method1() {
            Console.WriteLine(y);
        }
    }
}
```

<span data-ttu-id="5c202-581">最后，以下匿名函数捕获`this`以及使用不同的生存期的两个本地变量：</span><span class="sxs-lookup"><span data-stu-id="5c202-581">Finally, the following anonymous function captures `this` as well as two local variables with different lifetimes:</span></span>
```csharp
class Test
{
    int x;

    void F() {
        int y = 123;
        for (int i = 0; i < 10; i++) {
            int z = i * 2;
            D d = () => { Console.WriteLine(x + y + z); };
        }
    }
}
```

<span data-ttu-id="5c202-582">在这里，为每个语句创建一个编译器生成的类中，以便在不同的块中的局部变量可以有独立的生存期将为捕获局部变量阻止。</span><span class="sxs-lookup"><span data-stu-id="5c202-582">Here, a compiler generated class is created for each statement block in which locals are captured such that the locals in the different blocks can have independent lifetimes.</span></span> <span data-ttu-id="5c202-583">实例`__Locals2`，内部语句块中，编译器生成类包含本地变量`z`引用的实例的字段以及`__Locals1`。</span><span class="sxs-lookup"><span data-stu-id="5c202-583">An instance of `__Locals2`, the compiler generated class for the inner statement block, contains the local variable `z` and a field that references an instance of `__Locals1`.</span></span>  <span data-ttu-id="5c202-584">实例`__Locals1`，外部语句块中，编译器生成类包含本地变量`y`引用的字段以及`this`封闭函数成员。</span><span class="sxs-lookup"><span data-stu-id="5c202-584">An instance of `__Locals1`, the compiler generated class for the outer statement block, contains the local variable `y` and a field that references `this` of the enclosing function member.</span></span> <span data-ttu-id="5c202-585">使用可以访问这些数据结构所有捕获外部变量的实例通过`__Local2`，并因此可以作为该类的实例方法实现的匿名函数的代码。</span><span class="sxs-lookup"><span data-stu-id="5c202-585">With these data structures it is possible to reach all captured outer variables through an instance of `__Local2`, and the code of the anonymous function can thus be implemented as an instance method of that class.</span></span>

```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.__this = this;
        __locals1.y = 123;
        for (int i = 0; i < 10; i++) {
            __Locals2 __locals2 = new __Locals2();
            __locals2.__locals1 = __locals1;
            __locals2.z = i * 2;
            D d = new D(__locals2.__Method1);
        }
    }

    class __Locals1
    {
        public Test __this;
        public int y;
    }

    class __Locals2
    {
        public __Locals1 __locals1;
        public int z;

        public void __Method1() {
            Console.WriteLine(__locals1.__this.x + __locals1.y + z);
        }
    }
}
```

<span data-ttu-id="5c202-586">将匿名函数转换为表达式树时，还可以使用相同的技术应用于此处捕获本地变量：对编译器生成对象的引用可以存储在表达式树中，并且可以表示本地变量的访问权限，因为字段访问这些对象。</span><span class="sxs-lookup"><span data-stu-id="5c202-586">The same technique applied here to capture local variables can also be used when converting anonymous functions to expression trees: References to the compiler generated objects can be stored in the expression tree, and access to the local variables can be represented as field accesses on these objects.</span></span> <span data-ttu-id="5c202-587">此方法的优点是它允许"提升"的本地变量的委托和表达式树之间共享。</span><span class="sxs-lookup"><span data-stu-id="5c202-587">The advantage of this approach is that it allows the "lifted" local variables to be shared between delegates and expression trees.</span></span>

## <a name="method-group-conversions"></a><span data-ttu-id="5c202-588">方法组转换</span><span class="sxs-lookup"><span data-stu-id="5c202-588">Method group conversions</span></span>

<span data-ttu-id="5c202-589">隐式转换 ([隐式转换](conversions.md#implicit-conversions)) 从一个方法组存在 ([表达式分类](expressions.md#expression-classifications)) 到兼容的委托类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-589">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from a method group ([Expression classifications](expressions.md#expression-classifications)) to a compatible delegate type.</span></span> <span data-ttu-id="5c202-590">对于给定的委托类型`D`和表达式`E`型方法组，从存在的隐式转换`E`到`D`如果`E`包含在其正常形式 （适用的至少一种方法[适用函数成员](expressions.md#applicable-function-member)) 到参数列表构造的使用的参数类型及修饰符的`D`，如在下面的示例所述。</span><span class="sxs-lookup"><span data-stu-id="5c202-590">Given a delegate type `D` and an expression `E` that is classified as a method group, an implicit conversion exists from `E` to `D` if `E` contains at least one method that is applicable in its normal form ([Applicable function member](expressions.md#applicable-function-member)) to an argument list constructed by use of the parameter types and modifiers of `D`, as described in the following.</span></span>

<span data-ttu-id="5c202-591">从方法组转换的编译时应用程序`E`为委托类型`D`在下面的示例所述。</span><span class="sxs-lookup"><span data-stu-id="5c202-591">The compile-time application of a conversion from a method group `E` to a delegate type `D` is described in the following.</span></span> <span data-ttu-id="5c202-592">请注意，是否存在隐式转换`E`到`D`不保证转换的编译时应用程序将成功且没有错误。</span><span class="sxs-lookup"><span data-stu-id="5c202-592">Note that the existence of an implicit conversion from `E` to `D` does not guarantee that the compile-time application of the conversion will succeed without error.</span></span>

*  <span data-ttu-id="5c202-593">单个方法`M`已选中对应于方法调用 ([方法调用](expressions.md#method-invocations)) 的窗体`E(A)`，并进行以下修改：</span><span class="sxs-lookup"><span data-stu-id="5c202-593">A single method `M` is selected corresponding to a method invocation ([Method invocations](expressions.md#method-invocations)) of the form `E(A)`, with the following modifications:</span></span>
    * <span data-ttu-id="5c202-594">参数列表`A`是一系列表达式中，每个已分类为变量和使用的类型和修饰符 (`ref`或`out`) 中的相应参数*formal_parameter_list* 的`D`.</span><span class="sxs-lookup"><span data-stu-id="5c202-594">The argument list `A` is a list of expressions, each classified as a variable and with the type and modifier (`ref` or `out`) of the corresponding parameter in the *formal_parameter_list* of `D`.</span></span>
    * <span data-ttu-id="5c202-595">考虑的候选方法是适用于其正常的窗体方法 ([适用函数成员](expressions.md#applicable-function-member))，不是那些仅在其扩展形式中。</span><span class="sxs-lookup"><span data-stu-id="5c202-595">The candidate methods considered are only those methods that are applicable in their normal form ([Applicable function member](expressions.md#applicable-function-member)), not those applicable only in their expanded form.</span></span>
*  <span data-ttu-id="5c202-596">如果的算法[方法调用](expressions.md#method-invocations)产生一个错误，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="5c202-596">If the algorithm of [Method invocations](expressions.md#method-invocations) produces an error, then a compile-time error occurs.</span></span> <span data-ttu-id="5c202-597">否则，该算法将生成单个的最佳方法`M`具有相同数量的参数作为`D`和转换被视为存在。</span><span class="sxs-lookup"><span data-stu-id="5c202-597">Otherwise the algorithm produces a single best method `M` having the same number of parameters as `D` and the conversion is considered to exist.</span></span>
*  <span data-ttu-id="5c202-598">所选的方法`M`必须是兼容的 ([委托兼容性](delegates.md#delegate-compatibility)) 与委托类型`D`，否则，发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="5c202-598">The selected method `M` must be compatible ([Delegate compatibility](delegates.md#delegate-compatibility)) with the delegate type `D`, or otherwise, a compile-time error occurs.</span></span>
*  <span data-ttu-id="5c202-599">如果所选的方法`M`是实例方法，与关联的实例表达式`E`确定委托的目标对象。</span><span class="sxs-lookup"><span data-stu-id="5c202-599">If the selected method `M` is an instance method, the instance expression associated with `E` determines the target object of the delegate.</span></span>
*  <span data-ttu-id="5c202-600">如果选定的方法 M 表示通过成员访问的实例表达式上的扩展方法，该实例表达式确定委托的目标对象。</span><span class="sxs-lookup"><span data-stu-id="5c202-600">If the selected method M is an extension method which is denoted by means of a member access on an instance expression, that instance expression determines the target object of the delegate.</span></span>
*  <span data-ttu-id="5c202-601">转换的结果是类型的值 `D`，即一个引用所选的方法和目标对象的新创建的委托。</span><span class="sxs-lookup"><span data-stu-id="5c202-601">The result of the conversion is a value of type `D`, namely a newly created delegate that refers to the selected method and target object.</span></span>
*  <span data-ttu-id="5c202-602">请注意，如果此过程可能会导致扩展方法的委托创建的算法[方法调用](expressions.md#method-invocations)无法找到的实例方法，但在处理的调用成功`E(A)`作为扩展方法调用 ([扩展方法调用](expressions.md#extension-method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="5c202-602">Note that this process can lead to the creation of a delegate to an extension method, if the algorithm of [Method invocations](expressions.md#method-invocations) fails to find an instance method but succeeds in processing the invocation of `E(A)` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="5c202-603">因此创建的委托捕获扩展方法，以及其第一个参数。</span><span class="sxs-lookup"><span data-stu-id="5c202-603">A delegate thus created captures the extension method as well as its first argument.</span></span>

<span data-ttu-id="5c202-604">下面的示例演示方法组转换：</span><span class="sxs-lookup"><span data-stu-id="5c202-604">The following example demonstrates method group conversions:</span></span>
```csharp
delegate string D1(object o);

delegate object D2(string s);

delegate object D3();

delegate string D4(object o, params object[] a);

delegate string D5(int i);

class Test
{
    static string F(object o) {...}

    static void G() {
        D1 d1 = F;            // Ok
        D2 d2 = F;            // Ok
        D3 d3 = F;            // Error -- not applicable
        D4 d4 = F;            // Error -- not applicable in normal form
        D5 d5 = F;            // Error -- applicable but not compatible

    }
}
```

<span data-ttu-id="5c202-605">分配给`d1`隐式将方法组转换`F`类型的值为`D1`。</span><span class="sxs-lookup"><span data-stu-id="5c202-605">The assignment to `d1` implicitly converts the method group `F` to a value of type `D1`.</span></span>

<span data-ttu-id="5c202-606">分配给`d2`演示如何可以创建具有较少派生 （逆变） 参数类型的方法的委托并更派生 （协变） 的返回类型。</span><span class="sxs-lookup"><span data-stu-id="5c202-606">The assignment to `d2` shows how it is possible to create a delegate to a method that has less derived (contravariant) parameter types and a more derived (covariant) return type.</span></span>

<span data-ttu-id="5c202-607">分配给`d3`显示了如何不存在转换方法不适用。</span><span class="sxs-lookup"><span data-stu-id="5c202-607">The assignment to `d3` shows how no conversion exists if the method is not applicable.</span></span>

<span data-ttu-id="5c202-608">分配给`d4`显示了如何，该方法必须是以普通形式适用。</span><span class="sxs-lookup"><span data-stu-id="5c202-608">The assignment to `d4` shows how the method must be applicable in its normal form.</span></span>

<span data-ttu-id="5c202-609">分配给`d5`显示了如何允许参数和返回类型的委托和方法仅为引用类型不同。</span><span class="sxs-lookup"><span data-stu-id="5c202-609">The assignment to `d5` shows how parameter and return types of the delegate and method are allowed to differ only for reference types.</span></span>

<span data-ttu-id="5c202-610">与所有其他隐式和显式转换，强制转换运算符可以用于显式执行方法组转换。</span><span class="sxs-lookup"><span data-stu-id="5c202-610">As with all other implicit and explicit conversions, the cast operator can be used to explicitly perform a method group conversion.</span></span> <span data-ttu-id="5c202-611">因此，示例</span><span class="sxs-lookup"><span data-stu-id="5c202-611">Thus, the example</span></span>
```csharp
object obj = new EventHandler(myDialog.OkClick);
```
<span data-ttu-id="5c202-612">可以改为编写</span><span class="sxs-lookup"><span data-stu-id="5c202-612">could instead be written</span></span>
```csharp
object obj = (EventHandler)myDialog.OkClick;
```

<span data-ttu-id="5c202-613">方法组可能会影响重载决策，并参与类型推理。</span><span class="sxs-lookup"><span data-stu-id="5c202-613">Method groups may influence overload resolution, and participate in type inference.</span></span> <span data-ttu-id="5c202-614">请参阅[函数成员](expressions.md#function-members)的更多详细信息。</span><span class="sxs-lookup"><span data-stu-id="5c202-614">See [Function members](expressions.md#function-members) for further details.</span></span>

<span data-ttu-id="5c202-615">方法组转换运行时计算过程继续执行，如下所示：</span><span class="sxs-lookup"><span data-stu-id="5c202-615">The run-time evaluation of a method group conversion proceeds as follows:</span></span>

*  <span data-ttu-id="5c202-616">如果在编译时选择的方法是实例方法，或者它是作为实例方法来访问该扩展方法，从与关联的实例表达式确定目标对象的委托的`E`:</span><span class="sxs-lookup"><span data-stu-id="5c202-616">If the method selected at compile-time is an instance method, or it is an extension method which is accessed as an instance method, the target object of the delegate is determined from the instance expression associated with `E`:</span></span>
    * <span data-ttu-id="5c202-617">计算实例表达式。</span><span class="sxs-lookup"><span data-stu-id="5c202-617">The instance expression is evaluated.</span></span> <span data-ttu-id="5c202-618">如果此计算导致了异常，不执行任何进一步的操作。</span><span class="sxs-lookup"><span data-stu-id="5c202-618">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="5c202-619">如果实例表达式属于*reference_type*，计算实例表达式的值将成为目标对象。</span><span class="sxs-lookup"><span data-stu-id="5c202-619">If the instance expression is of a *reference_type*, the value computed by the instance expression becomes the target object.</span></span> <span data-ttu-id="5c202-620">如果所选的方法是实例方法，目标对象是`null`、`System.NullReferenceException`引发并执行任何进一步的操作。</span><span class="sxs-lookup"><span data-stu-id="5c202-620">If the selected method is an instance method and the target object is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="5c202-621">如果实例表达式属于*value_type*，装箱操作 ([装箱转换](types.md#boxing-conversions)) 执行将值转换为一个对象，此对象将成为目标对象。</span><span class="sxs-lookup"><span data-stu-id="5c202-621">If the instance expression is of a *value_type*, a boxing operation ([Boxing conversions](types.md#boxing-conversions)) is performed to convert the value to an object, and this object becomes the target object.</span></span>
*  <span data-ttu-id="5c202-622">否则所选的方法是静态方法调用的一部分，委托的目标对象是`null`。</span><span class="sxs-lookup"><span data-stu-id="5c202-622">Otherwise the selected method is part of a static method call, and the target object of the delegate is `null`.</span></span>
*  <span data-ttu-id="5c202-623">委托类型的新实例`D`分配。</span><span class="sxs-lookup"><span data-stu-id="5c202-623">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="5c202-624">如果没有足够内存可用于分配新实例，`System.OutOfMemoryException`引发并执行任何进一步的操作。</span><span class="sxs-lookup"><span data-stu-id="5c202-624">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="5c202-625">对已在编译时确定的方法的引用初始化新的委托实例和上面计算对目标对象的引用。</span><span class="sxs-lookup"><span data-stu-id="5c202-625">The new delegate instance is initialized with a reference to the method that was determined at compile-time and a reference to the target object computed above.</span></span>
