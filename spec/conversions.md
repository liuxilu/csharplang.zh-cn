---
ms.openlocfilehash: 4d6d28a3127bc701867afe157aa5496377a06f69
ms.sourcegitcommit: 63d276488c9770a565fd787020783ffc1d2af9d6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2019
ms.locfileid: "74867999"
---
# <a name="conversions"></a><span data-ttu-id="56ea5-101">转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-101">Conversions</span></span>

<span data-ttu-id="56ea5-102">***转换***允许将表达式视为特定类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-102">A ***conversion*** enables an expression to be treated as being of a particular type.</span></span> <span data-ttu-id="56ea5-103">转换可能会导致将给定类型的表达式视为具有不同的类型，也可能导致不具有类型的表达式获得类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-103">A conversion may cause an expression of a given type to be treated as having a different type, or it may cause an expression without a type to get a type.</span></span> <span data-ttu-id="56ea5-104">转换可以是***隐式***的或***显式***的，这确定是否需要显式强制转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-104">Conversions can be ***implicit*** or ***explicit***, and this determines whether an explicit cast is required.</span></span> <span data-ttu-id="56ea5-105">例如，从类型 `int` 到类型 `long` 的转换是隐式的，因此类型 `int` 的表达式可隐式视为类型 `long`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-105">For instance, the conversion from type `int` to type `long` is implicit, so expressions of type `int` can implicitly be treated as type `long`.</span></span> <span data-ttu-id="56ea5-106">从类型 `long` 到类型 `int`，相反的转换是显式的，因此需要显式强制转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-106">The opposite conversion, from type `long` to type `int`, is explicit and so an explicit cast is required.</span></span>

```csharp
int a = 123;
long b = a;         // implicit conversion from int to long
int c = (int) b;    // explicit conversion from long to int
```

<span data-ttu-id="56ea5-107">某些转换是由语言定义的。</span><span class="sxs-lookup"><span data-stu-id="56ea5-107">Some conversions are defined by the language.</span></span> <span data-ttu-id="56ea5-108">程序还可以定义自己的转换（[用户定义的转换](conversions.md#user-defined-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-108">Programs may also define their own conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

## <a name="implicit-conversions"></a><span data-ttu-id="56ea5-109">隐式转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-109">Implicit conversions</span></span>

<span data-ttu-id="56ea5-110">以下转换归类为隐式转换：</span><span class="sxs-lookup"><span data-stu-id="56ea5-110">The following conversions are classified as implicit conversions:</span></span>

*  <span data-ttu-id="56ea5-111">标识转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-111">Identity conversions</span></span>
*  <span data-ttu-id="56ea5-112">隐式数值转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-112">Implicit numeric conversions</span></span>
*  <span data-ttu-id="56ea5-113">隐式枚举转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-113">Implicit enumeration conversions</span></span>
*  <span data-ttu-id="56ea5-114">隐式内插字符串转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-114">Implicit interpolated string conversions</span></span>
*  <span data-ttu-id="56ea5-115">隐式可为 null 转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-115">Implicit nullable conversions</span></span>
*  <span data-ttu-id="56ea5-116">Null 文本转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-116">Null literal conversions</span></span>
*  <span data-ttu-id="56ea5-117">隐式引用转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-117">Implicit reference conversions</span></span>
*  <span data-ttu-id="56ea5-118">装箱转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-118">Boxing conversions</span></span>
*  <span data-ttu-id="56ea5-119">隐式动态转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-119">Implicit dynamic conversions</span></span>
*  <span data-ttu-id="56ea5-120">隐式常量表达式转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-120">Implicit constant expression conversions</span></span>
*  <span data-ttu-id="56ea5-121">用户定义的隐式转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-121">User-defined implicit conversions</span></span>
*  <span data-ttu-id="56ea5-122">匿名函数转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-122">Anonymous function conversions</span></span>
*  <span data-ttu-id="56ea5-123">方法组转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-123">Method group conversions</span></span>

<span data-ttu-id="56ea5-124">隐式转换可能会在多种情况下发生，包括函数成员调用（[动态重载决策的编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）、强制转换表达式（[强制](expressions.md#cast-expressions)转换表达式）和赋值（[赋值运算符](expressions.md#assignment-operators)）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-124">Implicit conversions can occur in a variety of situations, including function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), cast expressions ([Cast expressions](expressions.md#cast-expressions)), and assignments ([Assignment operators](expressions.md#assignment-operators)).</span></span>

<span data-ttu-id="56ea5-125">预定义的隐式转换始终会成功，并且永远不会引发异常。</span><span class="sxs-lookup"><span data-stu-id="56ea5-125">The pre-defined implicit conversions always succeed and never cause exceptions to be thrown.</span></span> <span data-ttu-id="56ea5-126">正确设计的用户定义隐式转换也应显示这些特征。</span><span class="sxs-lookup"><span data-stu-id="56ea5-126">Properly designed user-defined implicit conversions should exhibit these characteristics as well.</span></span>

<span data-ttu-id="56ea5-127">出于转换目的，`object` 和 `dynamic` 类型被视为等效类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-127">For the purposes of conversion, the types `object` and `dynamic` are considered equivalent.</span></span>

<span data-ttu-id="56ea5-128">但是，动态转换（[隐式动态转换](conversions.md#implicit-dynamic-conversions)和[显式动态转换](conversions.md#explicit-dynamic-conversions)）仅适用于 `dynamic` 类型（[动态类型](types.md#the-dynamic-type)）的表达式。</span><span class="sxs-lookup"><span data-stu-id="56ea5-128">However, dynamic conversions ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions) and [Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions)) apply only to expressions of type `dynamic` ([The dynamic type](types.md#the-dynamic-type)).</span></span>

### <a name="identity-conversion"></a><span data-ttu-id="56ea5-129">标识转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-129">Identity conversion</span></span>

<span data-ttu-id="56ea5-130">标识转换从任何类型转换为同一类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-130">An identity conversion converts from any type to the same type.</span></span> <span data-ttu-id="56ea5-131">此转换存在，因此，已具有所需类型的实体可被视为可转换为该类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-131">This conversion exists such that an entity that already has a required type can be said to be convertible to that type.</span></span>

*  <span data-ttu-id="56ea5-132">因为 `object` 和 `dynamic` 被视为等效的，所以在 `object` 和 `dynamic`之间存在标识转换，并且在用 `dynamic` 替换所有匹配项时，构造类型之间是相同的。</span><span class="sxs-lookup"><span data-stu-id="56ea5-132">Because `object` and `dynamic` are considered equivalent there is an identity conversion between `object` and `dynamic`, and between constructed types that are the same when replacing all occurrences of `dynamic` with `object`.</span></span>

### <a name="implicit-numeric-conversions"></a><span data-ttu-id="56ea5-133">隐式数值转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-133">Implicit numeric conversions</span></span>

<span data-ttu-id="56ea5-134">隐式数值转换为：</span><span class="sxs-lookup"><span data-stu-id="56ea5-134">The implicit numeric conversions are:</span></span>

*  <span data-ttu-id="56ea5-135">从 `sbyte` 到 `short`、`int`、`long`、`float`、`double`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-135">From `sbyte` to `short`, `int`, `long`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="56ea5-136">从 `byte` 到 `short`、`ushort`、`int`、`uint`、`long`、`ulong`、`float`、`double`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-136">From `byte` to `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="56ea5-137">从 `short` 到 `int`、`long`、`float`、`double`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-137">From `short` to `int`, `long`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="56ea5-138">从 `ushort` 到 `int`、`uint`、`long`、`ulong`、`float`、`double`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-138">From `ushort` to `int`, `uint`, `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="56ea5-139">从 `int` 到 `long`、`float`、`double`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-139">From `int` to `long`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="56ea5-140">从 `uint` 到 `long`、`ulong`、`float`、`double`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-140">From `uint` to `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="56ea5-141">从 `long` 到 `float`、`double`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-141">From `long` to `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="56ea5-142">从 `ulong` 到 `float`、`double`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-142">From `ulong` to `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="56ea5-143">从 `char` 到 `ushort`、`int`、`uint`、`long`、`ulong`、`float`、`double`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-143">From `char` to `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="56ea5-144">从 `float` 到 `double`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-144">From `float` to `double`.</span></span>

<span data-ttu-id="56ea5-145">从 `int`、`uint`、`long`或 `ulong` 到 `float` 以及从 `long` 或 `ulong` 到 `double` 的转换可能会导致精度损失，但永远不会导致数量级损失。</span><span class="sxs-lookup"><span data-stu-id="56ea5-145">Conversions from `int`, `uint`, `long`, or `ulong` to `float` and from `long` or `ulong` to `double` may cause a loss of precision, but will never cause a loss of magnitude.</span></span> <span data-ttu-id="56ea5-146">其他隐式数值转换不会丢失任何信息。</span><span class="sxs-lookup"><span data-stu-id="56ea5-146">The other implicit numeric conversions never lose any information.</span></span>

<span data-ttu-id="56ea5-147">不存在到 `char` 类型的隐式转换，因此其他整型类型的值不会自动转换为 `char` 类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-147">There are no implicit conversions to the `char` type, so values of the other integral types do not automatically convert to the `char` type.</span></span>

### <a name="implicit-enumeration-conversions"></a><span data-ttu-id="56ea5-148">隐式枚举转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-148">Implicit enumeration conversions</span></span>

<span data-ttu-id="56ea5-149">隐式枚举转换允许*decimal_integer_literal* `0` 转换为任何*enum_type* ，以及基础类型为*enum_type*的任何*nullable_type* 。</span><span class="sxs-lookup"><span data-stu-id="56ea5-149">An implicit enumeration conversion permits the *decimal_integer_literal* `0` to be converted to any *enum_type* and to any *nullable_type* whose underlying type is an *enum_type*.</span></span> <span data-ttu-id="56ea5-150">在后一种情况下，转换将通过转换为基础*enum_type*并包装结果（[可以为 null 的类型](types.md#nullable-types)）来进行计算。</span><span class="sxs-lookup"><span data-stu-id="56ea5-150">In the latter case the conversion is evaluated by converting to the underlying *enum_type* and wrapping the result ([Nullable types](types.md#nullable-types)).</span></span>

### <a name="implicit-interpolated-string-conversions"></a><span data-ttu-id="56ea5-151">隐式内插字符串转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-151">Implicit interpolated string conversions</span></span>

<span data-ttu-id="56ea5-152">隐式内插字符串转换允许*interpolated_string_expression* （内[插的字符串](expressions.md#interpolated-strings)）转换为 `System.IFormattable` 或 `System.FormattableString` （实现 `System.IFormattable`）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-152">An implicit interpolated string conversion permits an *interpolated_string_expression* ([Interpolated strings](expressions.md#interpolated-strings)) to be converted to `System.IFormattable` or `System.FormattableString` (which implements `System.IFormattable`).</span></span>

<span data-ttu-id="56ea5-153">应用此转换时，字符串值不是由内插字符串组成的。</span><span class="sxs-lookup"><span data-stu-id="56ea5-153">When this conversion is applied a string value is not composed from the interpolated string.</span></span> <span data-ttu-id="56ea5-154">相反，将创建一个 `System.FormattableString` 实例，如内[插字符串](expressions.md#interpolated-strings)中所述。</span><span class="sxs-lookup"><span data-stu-id="56ea5-154">Instead an instance of `System.FormattableString` is created, as further described in [Interpolated strings](expressions.md#interpolated-strings).</span></span>

### <a name="implicit-nullable-conversions"></a><span data-ttu-id="56ea5-155">隐式可为 null 转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-155">Implicit nullable conversions</span></span>

<span data-ttu-id="56ea5-156">对不可以为 null 的值类型进行操作的预定义隐式转换也可用于这些类型的可以为 null 的形式。</span><span class="sxs-lookup"><span data-stu-id="56ea5-156">Predefined implicit conversions that operate on non-nullable value types can also be used with nullable forms of those types.</span></span> <span data-ttu-id="56ea5-157">对于从不可为 null 的值类型转换为不可以为 null 的值类型的每个预定义隐式标识和数值转换 `S` `T`，以下隐式可为 null 的转换：</span><span class="sxs-lookup"><span data-stu-id="56ea5-157">For each of the predefined implicit identity and numeric conversions that convert from a non-nullable value type `S` to a non-nullable value type `T`, the following implicit nullable conversions exist:</span></span>

*  <span data-ttu-id="56ea5-158">从 `S?` 到 `T?`的隐式转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-158">An implicit conversion from `S?` to `T?`.</span></span>
*  <span data-ttu-id="56ea5-159">从 `S` 到 `T?`的隐式转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-159">An implicit conversion from `S` to `T?`.</span></span>

<span data-ttu-id="56ea5-160">基于从 `S` 到 `T` 的基础转换计算隐式可为 null 的转换，如下所示：</span><span class="sxs-lookup"><span data-stu-id="56ea5-160">Evaluation of an implicit nullable conversion based on an underlying conversion from `S` to `T` proceeds as follows:</span></span>

*  <span data-ttu-id="56ea5-161">如果可为 null 的转换来自 `S?` 到 `T?`：</span><span class="sxs-lookup"><span data-stu-id="56ea5-161">If the nullable conversion is from `S?` to `T?`:</span></span>
    * <span data-ttu-id="56ea5-162">如果源值为 null （`HasValue` 属性为 false），则结果为类型 `T?`的 null 值。</span><span class="sxs-lookup"><span data-stu-id="56ea5-162">If the source value is null (`HasValue` property is false), the result is the null value of type `T?`.</span></span>
    * <span data-ttu-id="56ea5-163">否则，转换将作为从 `S?` 到 `S`的解包进行计算，后跟从 `S` 到 `T`的底层转换，后跟从 `T` 到 `T?`的包装（[可为 null 的类型](types.md#nullable-types)）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-163">Otherwise, the conversion is evaluated as an unwrapping from `S?` to `S`, followed by the underlying conversion from `S` to `T`, followed by a wrapping ([Nullable types](types.md#nullable-types)) from `T` to `T?`.</span></span>

*  <span data-ttu-id="56ea5-164">如果将可为 null 的转换来自 `S` 到 `T?`，则会将转换计算为从 `S` 到 `T` 后跟从 `T` 到 `T?`的基础转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-164">If the nullable conversion is from `S` to `T?`, the conversion is evaluated as the underlying conversion from `S` to `T` followed by a wrapping from `T` to `T?`.</span></span>

### <a name="null-literal-conversions"></a><span data-ttu-id="56ea5-165">Null 文本转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-165">Null literal conversions</span></span>

<span data-ttu-id="56ea5-166">存在从 `null` 文本到任何可以为 null 的类型的隐式转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-166">An implicit conversion exists from the `null` literal to any nullable type.</span></span> <span data-ttu-id="56ea5-167">此转换生成给定可以为 null 的类型的 null 值（[可为](types.md#nullable-types)null 的类型）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-167">This conversion produces the null value ([Nullable types](types.md#nullable-types)) of the given nullable type.</span></span>

### <a name="implicit-reference-conversions"></a><span data-ttu-id="56ea5-168">隐式引用转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-168">Implicit reference conversions</span></span>

<span data-ttu-id="56ea5-169">隐式引用转换为：</span><span class="sxs-lookup"><span data-stu-id="56ea5-169">The implicit reference conversions are:</span></span>

*  <span data-ttu-id="56ea5-170">从任何*reference_type*到 `object` 和 `dynamic`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-170">From any *reference_type* to `object` and `dynamic`.</span></span>
*  <span data-ttu-id="56ea5-171">从任何*class_type* `S` 到任何*class_type* `T`，提供的 `S` 派生自 `T`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-171">From any *class_type* `S` to any *class_type* `T`, provided `S` is derived from `T`.</span></span>
*  <span data-ttu-id="56ea5-172">从任何*class_type* `S` 到任何*interface_type* `T`，提供 `S` 实现 `T`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-172">From any *class_type* `S` to any *interface_type* `T`, provided `S` implements `T`.</span></span>
*  <span data-ttu-id="56ea5-173">从任何*interface_type* `S` 到任何*interface_type* `T`，提供的 `S` 派生自 `T`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-173">From any *interface_type* `S` to any *interface_type* `T`, provided `S` is derived from `T`.</span></span>
*  <span data-ttu-id="56ea5-174">在元素类型为的*array_type* `S` 中 `SE` 到元素类型 `T` 的*array_type* `TE`，前提是满足以下所有条件：</span><span class="sxs-lookup"><span data-stu-id="56ea5-174">From an *array_type* `S` with an element type `SE` to an *array_type* `T` with an element type `TE`, provided all of the following are true:</span></span>
    * <span data-ttu-id="56ea5-175">`S` 和 `T` 仅在元素类型上存在差异。</span><span class="sxs-lookup"><span data-stu-id="56ea5-175">`S` and `T` differ only in element type.</span></span> <span data-ttu-id="56ea5-176">换句话说，`S` 和 `T` 具有相同的维数。</span><span class="sxs-lookup"><span data-stu-id="56ea5-176">In other words, `S` and `T` have the same number of dimensions.</span></span>
    * <span data-ttu-id="56ea5-177">`SE` 和 `TE` *reference_type*。</span><span class="sxs-lookup"><span data-stu-id="56ea5-177">Both `SE` and `TE` are *reference_type*s.</span></span>
    * <span data-ttu-id="56ea5-178">存在从 `SE` 到 `TE`的隐式引用转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-178">An implicit reference conversion exists from `SE` to `TE`.</span></span>
*  <span data-ttu-id="56ea5-179">从任何*array_type*到 `System.Array` 及其实现的接口。</span><span class="sxs-lookup"><span data-stu-id="56ea5-179">From any *array_type* to `System.Array` and the interfaces it implements.</span></span>
*  <span data-ttu-id="56ea5-180">从一维数组类型 `S[]` 到 `System.Collections.Generic.IList<T>` 及其基接口，前提是存在从 `S` 到 `T`的隐式标识或引用转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-180">From a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its base interfaces, provided that there is an implicit identity or reference conversion from `S` to `T`.</span></span>
*  <span data-ttu-id="56ea5-181">从任何*delegate_type*到 `System.Delegate` 及其实现的接口。</span><span class="sxs-lookup"><span data-stu-id="56ea5-181">From any *delegate_type* to `System.Delegate` and the interfaces it implements.</span></span>
*  <span data-ttu-id="56ea5-182">从 null 文本到任何*reference_type*。</span><span class="sxs-lookup"><span data-stu-id="56ea5-182">From the null literal to any *reference_type*.</span></span>
*  <span data-ttu-id="56ea5-183">从任何*reference_type*到*reference_type* `T` 如果它具有隐式标识或到*reference_type* `T0` 的引用转换，并且 `T0` 的标识转换为 `T`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-183">From any *reference_type* to a *reference_type* `T` if it has an implicit identity or reference conversion to a *reference_type* `T0` and `T0` has an identity conversion to `T`.</span></span>
*  <span data-ttu-id="56ea5-184">从任何*reference_type*到接口或委托类型 `T` 如果它具有隐式标识或到接口或委托类型的引用转换 `T0` 并且 `T0` 可转换为 `T`的变体（[差异转换](interfaces.md#variance-conversion)）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-184">From any *reference_type* to an interface or delegate type `T` if it has an implicit identity or reference conversion to an interface or delegate type `T0` and `T0` is variance-convertible ([Variance conversion](interfaces.md#variance-conversion)) to `T`.</span></span>
*  <span data-ttu-id="56ea5-185">涉及称为引用类型的类型参数的隐式转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-185">Implicit conversions involving type parameters that are known to be reference types.</span></span> <span data-ttu-id="56ea5-186">有关涉及类型参数的隐式转换的更多详细信息，请参阅[涉及类型参数的隐式转换](conversions.md#implicit-conversions-involving-type-parameters)。</span><span class="sxs-lookup"><span data-stu-id="56ea5-186">See [Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters) for more details on implicit conversions involving type parameters.</span></span>

<span data-ttu-id="56ea5-187">隐式引用转换是*reference_type*之间的转换，这些转换可证明始终成功，因此不需要在运行时进行检查。</span><span class="sxs-lookup"><span data-stu-id="56ea5-187">The implicit reference conversions are those conversions between *reference_type*s that can be proven to always succeed, and therefore require no checks at run-time.</span></span>

<span data-ttu-id="56ea5-188">引用转换、隐式或显式转换决不会更改正在转换的对象的引用标识。</span><span class="sxs-lookup"><span data-stu-id="56ea5-188">Reference conversions, implicit or explicit, never change the referential identity of the object being converted.</span></span> <span data-ttu-id="56ea5-189">换言之，虽然引用转换可以更改引用的类型，但它不会更改引用的对象的类型或值。</span><span class="sxs-lookup"><span data-stu-id="56ea5-189">In other words, while a reference conversion may change the type of the reference, it never changes the type or value of the object being referred to.</span></span>

### <a name="boxing-conversions"></a><span data-ttu-id="56ea5-190">装箱转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-190">Boxing conversions</span></span>

<span data-ttu-id="56ea5-191">装箱转换允许*value_type*隐式转换为引用类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-191">A boxing conversion permits a *value_type* to be implicitly converted to a reference type.</span></span> <span data-ttu-id="56ea5-192">存在从任何*non_nullable_value_type*到 `object` 和 `dynamic`的装箱转换，以 `System.ValueType` 和*interface_type*实现的任何*non_nullable_value_type* 。</span><span class="sxs-lookup"><span data-stu-id="56ea5-192">A boxing conversion exists from any *non_nullable_value_type* to `object` and `dynamic`, to `System.ValueType` and to any *interface_type* implemented by the *non_nullable_value_type*.</span></span> <span data-ttu-id="56ea5-193">此外，可以将*enum_type*转换为类型 `System.Enum`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-193">Furthermore an *enum_type* can be converted to the type `System.Enum`.</span></span>

<span data-ttu-id="56ea5-194">如果且仅当存在从基础*non_nullable_value_type*到引用类型的装箱转换，则从*nullable_type*到引用类型的装箱转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-194">A boxing conversion exists from a *nullable_type* to a reference type, if and only if a boxing conversion exists from the underlying *non_nullable_value_type* to the reference type.</span></span>

<span data-ttu-id="56ea5-195">如果某个值类型具有到接口类型的装箱转换，则该值类型具有到该接口类型的装箱转换 `I` `I0` 并且 `I0` 具有到 `I`的标识转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-195">A value type has a boxing conversion to an interface type `I` if it has a boxing conversion to an interface type `I0` and `I0` has an identity conversion to `I`.</span></span>

<span data-ttu-id="56ea5-196">如果某个值类型具有到接口类型的装箱转换或委托类型的装箱转换，则该值类型会将其装箱转换 `I` `I0` 并 `I0` 区分方差（[变化转换](interfaces.md#variance-conversion)） `I`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-196">A value type has a boxing conversion to an interface type `I` if it has a boxing conversion to an interface or delegate type `I0` and `I0` is variance-convertible ([Variance conversion](interfaces.md#variance-conversion)) to `I`.</span></span>

<span data-ttu-id="56ea5-197">将*non_nullable_value_type*的值装箱包括分配对象实例并将*value_type*值复制到该实例中。</span><span class="sxs-lookup"><span data-stu-id="56ea5-197">Boxing a value of a *non_nullable_value_type* consists of allocating an object instance and copying the *value_type* value into that instance.</span></span> <span data-ttu-id="56ea5-198">结构可以装箱到类型 `System.ValueType`，因为这是所有结构（[继承](structs.md#inheritance)）的基类。</span><span class="sxs-lookup"><span data-stu-id="56ea5-198">A struct can be boxed to the type `System.ValueType`, since that is a base class for all structs ([Inheritance](structs.md#inheritance)).</span></span>

<span data-ttu-id="56ea5-199">将*nullable_type*的值装箱将继续执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="56ea5-199">Boxing a value of a *nullable_type* proceeds as follows:</span></span>

*  <span data-ttu-id="56ea5-200">如果源值为 null （`HasValue` 属性为 false），则结果为目标类型的空引用。</span><span class="sxs-lookup"><span data-stu-id="56ea5-200">If the source value is null (`HasValue` property is false), the result is a null reference of the target type.</span></span>
*  <span data-ttu-id="56ea5-201">否则，结果是对通过解包和装箱源值生成的装箱 `T` 的引用。</span><span class="sxs-lookup"><span data-stu-id="56ea5-201">Otherwise, the result is a reference to a boxed `T` produced by unwrapping and boxing the source value.</span></span>

<span data-ttu-id="56ea5-202">[装箱转换中进一步](types.md#boxing-conversions)介绍了装箱转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-202">Boxing conversions are described further in [Boxing conversions](types.md#boxing-conversions).</span></span>

### <a name="implicit-dynamic-conversions"></a><span data-ttu-id="56ea5-203">隐式动态转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-203">Implicit dynamic conversions</span></span>

<span data-ttu-id="56ea5-204">存在从类型 `dynamic` 到任何类型 `T`的表达式的隐式动态转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-204">An implicit dynamic conversion exists from an expression of type `dynamic` to any type `T`.</span></span> <span data-ttu-id="56ea5-205">转换是动态绑定的（[动态绑定](expressions.md#dynamic-binding)），这意味着将在运行时从表达式的运行时类型中查找隐式转换，以便 `T`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-205">The conversion is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), which means that an implicit conversion will be sought at run-time from the run-time type of the expression to `T`.</span></span> <span data-ttu-id="56ea5-206">如果未找到任何转换，则会引发运行时异常。</span><span class="sxs-lookup"><span data-stu-id="56ea5-206">If no conversion is found, a run-time exception is thrown.</span></span>

<span data-ttu-id="56ea5-207">请注意，此隐式转换似乎违反了隐式转换开始时的[建议，隐](conversions.md#implicit-conversions)式转换应永远不会引发异常。</span><span class="sxs-lookup"><span data-stu-id="56ea5-207">Note that this implicit conversion seemingly violates the advice in the beginning of [Implicit conversions](conversions.md#implicit-conversions) that an implicit conversion should never cause an exception.</span></span> <span data-ttu-id="56ea5-208">但它不是转换本身，而是*查找*导致异常的转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-208">However it is not the conversion itself, but the *finding* of the conversion that causes the exception.</span></span> <span data-ttu-id="56ea5-209">运行时异常的风险在使用动态绑定时是固有的。</span><span class="sxs-lookup"><span data-stu-id="56ea5-209">The risk of run-time exceptions is inherent in the use of dynamic binding.</span></span> <span data-ttu-id="56ea5-210">如果不需要转换的动态绑定，则可以先将表达式转换为 `object`，然后转换为所需的类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-210">If dynamic binding of the conversion is not desired, the expression can be first converted to `object`, and then to the desired type.</span></span>

<span data-ttu-id="56ea5-211">下面的示例阐释了隐式动态转换：</span><span class="sxs-lookup"><span data-stu-id="56ea5-211">The following example illustrates implicit dynamic conversions:</span></span>

```csharp
object o  = "object"
dynamic d = "dynamic";

string s1 = o; // Fails at compile-time -- no conversion exists
string s2 = d; // Compiles and succeeds at run-time
int i     = d; // Compiles but fails at run-time -- no conversion exists
```

<span data-ttu-id="56ea5-212">`s2` 和 `i` 的分配都采用隐式动态转换，在这种情况下，将在运行时暂停操作的绑定。</span><span class="sxs-lookup"><span data-stu-id="56ea5-212">The assignments to `s2` and `i` both employ implicit dynamic conversions, where the binding of the operations is suspended until run-time.</span></span> <span data-ttu-id="56ea5-213">在运行时，隐式转换是从 `d` -- `string`--到目标类型的运行时类型中查找的。</span><span class="sxs-lookup"><span data-stu-id="56ea5-213">At run-time, implicit conversions are sought from the run-time type of `d` -- `string` -- to the target type.</span></span> <span data-ttu-id="56ea5-214">找到 `string` 但不能 `int`的转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-214">A conversion is found to `string` but not to `int`.</span></span>

### <a name="implicit-constant-expression-conversions"></a><span data-ttu-id="56ea5-215">隐式常量表达式转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-215">Implicit constant expression conversions</span></span>

<span data-ttu-id="56ea5-216">隐式常数表达式转换允许以下转换：</span><span class="sxs-lookup"><span data-stu-id="56ea5-216">An implicit constant expression conversion permits the following conversions:</span></span>

*  <span data-ttu-id="56ea5-217">如果 *`short`* 的值在目标类型的范围内，则可以将 `int` 类型的*constant_expression* （[常数表达式](expressions.md#constant-expressions)）转换为类型 `sbyte`、`byte`、`ushort`、`uint`、`ulong`或 constant_expression。</span><span class="sxs-lookup"><span data-stu-id="56ea5-217">A *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) of type `int` can be converted to type `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the *constant_expression* is within the range of the destination type.</span></span>
*  <span data-ttu-id="56ea5-218">`long` 类型的*constant_expression*可转换为类型 `ulong`，前提是*constant_expression*的值不是负数。</span><span class="sxs-lookup"><span data-stu-id="56ea5-218">A *constant_expression* of type `long` can be converted to type `ulong`, provided the value of the *constant_expression* is not negative.</span></span>

### <a name="implicit-conversions-involving-type-parameters"></a><span data-ttu-id="56ea5-219">涉及类型参数的隐式转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-219">Implicit conversions involving type parameters</span></span>

<span data-ttu-id="56ea5-220">`T`的给定类型参数存在以下隐式转换：</span><span class="sxs-lookup"><span data-stu-id="56ea5-220">The following implicit conversions exist for a given type parameter `T`:</span></span>

*  <span data-ttu-id="56ea5-221">从 `T` 到其有效的基类 `C`，从 `T` 到 `C`的任何基类，从 `T` 到 `C`实现的任何接口。</span><span class="sxs-lookup"><span data-stu-id="56ea5-221">From `T` to its effective base class `C`, from `T` to any base class of `C`, and from `T` to any interface implemented by `C`.</span></span> <span data-ttu-id="56ea5-222">在运行时，如果 `T` 是值类型，则转换将作为装箱转换执行。</span><span class="sxs-lookup"><span data-stu-id="56ea5-222">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="56ea5-223">否则，转换将作为隐式引用转换或标识转换执行。</span><span class="sxs-lookup"><span data-stu-id="56ea5-223">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="56ea5-224">从 `T` 到接口类型 `I` 在 `T`的有效接口集中，并从 `T` 到 `I`的任何基接口。</span><span class="sxs-lookup"><span data-stu-id="56ea5-224">From `T` to an interface type `I` in `T`'s effective interface set and from `T` to any base interface of `I`.</span></span> <span data-ttu-id="56ea5-225">在运行时，如果 `T` 是值类型，则转换将作为装箱转换执行。</span><span class="sxs-lookup"><span data-stu-id="56ea5-225">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="56ea5-226">否则，转换将作为隐式引用转换或标识转换执行。</span><span class="sxs-lookup"><span data-stu-id="56ea5-226">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="56ea5-227">从 `T` 到类型形参 `U`，提供 `T` 取决于 `U` （[类型形参约束](classes.md#type-parameter-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-227">From `T` to a type parameter `U`, provided `T` depends on `U` ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="56ea5-228">在运行时，如果 `U` 是值类型，则 `T` 和 `U` 都必须是相同的类型，并且不执行任何转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-228">At run-time, if `U` is a value type, then `T` and `U` are necessarily the same type and no conversion is performed.</span></span> <span data-ttu-id="56ea5-229">否则，如果 `T` 是值类型，则转换将作为装箱转换执行。</span><span class="sxs-lookup"><span data-stu-id="56ea5-229">Otherwise, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="56ea5-230">否则，转换将作为隐式引用转换或标识转换执行。</span><span class="sxs-lookup"><span data-stu-id="56ea5-230">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="56ea5-231">从 null 文本到 `T`，前提是已知 `T` 为引用类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-231">From the null literal to `T`, provided `T` is known to be a reference type.</span></span>
*  <span data-ttu-id="56ea5-232">从 `T` 到引用类型 `I` 如果它具有到引用类型的隐式转换 `S0` 并且 `S0` 具有到 `S`的标识转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-232">From `T` to a reference type `I` if it has an implicit conversion to a reference type `S0` and `S0` has an identity conversion to `S`.</span></span> <span data-ttu-id="56ea5-233">在运行时，转换的执行方式与转换到 `S0`的方式相同。</span><span class="sxs-lookup"><span data-stu-id="56ea5-233">At run-time the conversion is executed the same way as the conversion to `S0`.</span></span>
*  <span data-ttu-id="56ea5-234">从 `T` 到接口类型 `I` 如果它具有到接口或委托类型的隐式转换 `I0` 并且 `I0` 可以转换为 `I` （[差异转换](interfaces.md#variance-conversion)）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-234">From `T` to an interface type `I` if it has an implicit conversion to an interface or delegate type `I0` and `I0` is variance-convertible to `I` ([Variance conversion](interfaces.md#variance-conversion)).</span></span> <span data-ttu-id="56ea5-235">在运行时，如果 `T` 是值类型，则转换将作为装箱转换执行。</span><span class="sxs-lookup"><span data-stu-id="56ea5-235">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion.</span></span> <span data-ttu-id="56ea5-236">否则，转换将作为隐式引用转换或标识转换执行。</span><span class="sxs-lookup"><span data-stu-id="56ea5-236">Otherwise, the conversion is executed as an implicit reference conversion or identity conversion.</span></span>

<span data-ttu-id="56ea5-237">如果已知 `T` 是引用类型（[类型参数约束](classes.md#type-parameter-constraints)），则上述转换全都归类为隐式引用转换（[隐式引用转换](conversions.md#implicit-reference-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-237">If `T` is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)), the conversions above are all classified as implicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions)).</span></span> <span data-ttu-id="56ea5-238">如果 `T` 不知道是引用类型，则上述转换归类为装箱转换（[装箱转换](conversions.md#boxing-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-238">If `T` is not known to be a reference type, the conversions above are classified as boxing conversions ([Boxing conversions](conversions.md#boxing-conversions)).</span></span>

### <a name="user-defined-implicit-conversions"></a><span data-ttu-id="56ea5-239">用户定义的隐式转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-239">User-defined implicit conversions</span></span>

<span data-ttu-id="56ea5-240">用户定义的隐式转换包括一个可选的标准隐式转换，然后执行用户定义的隐式转换运算符，然后执行另一个可选的标准隐式转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-240">A user-defined implicit conversion consists of an optional standard implicit conversion, followed by execution of a user-defined implicit conversion operator, followed by another optional standard implicit conversion.</span></span> <span data-ttu-id="56ea5-241">用于评估用户定义的隐式转换的确切规则在[处理用户定义的隐式转换](conversions.md#processing-of-user-defined-implicit-conversions)中进行了介绍。</span><span class="sxs-lookup"><span data-stu-id="56ea5-241">The exact rules for evaluating user-defined implicit conversions are described in [Processing of user-defined implicit conversions](conversions.md#processing-of-user-defined-implicit-conversions).</span></span>

### <a name="anonymous-function-conversions-and-method-group-conversions"></a><span data-ttu-id="56ea5-242">匿名函数转换和方法组转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-242">Anonymous function conversions and method group conversions</span></span>

<span data-ttu-id="56ea5-243">匿名函数和方法组本身没有类型，但可能会隐式转换为委托类型或表达式树类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-243">Anonymous functions and method groups do not have types in and of themselves, but may be implicitly converted to delegate types or expression tree types.</span></span> <span data-ttu-id="56ea5-244">[匿名函数转换和](conversions.md#anonymous-function-conversions)[方法组转换](conversions.md#method-group-conversions)中的方法组转换更详细地介绍了匿名函数转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-244">Anonymous function conversions are described in more detail in [Anonymous function conversions](conversions.md#anonymous-function-conversions) and method group conversions in [Method group conversions](conversions.md#method-group-conversions).</span></span>

## <a name="explicit-conversions"></a><span data-ttu-id="56ea5-245">显式转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-245">Explicit conversions</span></span>

<span data-ttu-id="56ea5-246">以下转换归类为显式转换：</span><span class="sxs-lookup"><span data-stu-id="56ea5-246">The following conversions are classified as explicit conversions:</span></span>

*  <span data-ttu-id="56ea5-247">所有隐式转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-247">All implicit conversions.</span></span>
*  <span data-ttu-id="56ea5-248">显式数值转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-248">Explicit numeric conversions.</span></span>
*  <span data-ttu-id="56ea5-249">显式枚举转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-249">Explicit enumeration conversions.</span></span>
*  <span data-ttu-id="56ea5-250">可以为 null 的显式转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-250">Explicit nullable conversions.</span></span>
*  <span data-ttu-id="56ea5-251">显式引用转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-251">Explicit reference conversions.</span></span>
*  <span data-ttu-id="56ea5-252">显式接口转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-252">Explicit interface conversions.</span></span>
*  <span data-ttu-id="56ea5-253">取消装箱转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-253">Unboxing conversions.</span></span>
*  <span data-ttu-id="56ea5-254">显式动态转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-254">Explicit dynamic conversions</span></span>
*  <span data-ttu-id="56ea5-255">用户定义的显式转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-255">User-defined explicit conversions.</span></span>

<span data-ttu-id="56ea5-256">显式转换可以出现在强制转换表达式中（[强制转换表达式](expressions.md#cast-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-256">Explicit conversions can occur in cast expressions ([Cast expressions](expressions.md#cast-expressions)).</span></span>

<span data-ttu-id="56ea5-257">显式转换集包括所有隐式转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-257">The set of explicit conversions includes all implicit conversions.</span></span> <span data-ttu-id="56ea5-258">这意味着允许冗余强制转换表达式。</span><span class="sxs-lookup"><span data-stu-id="56ea5-258">This means that redundant cast expressions are allowed.</span></span>

<span data-ttu-id="56ea5-259">不是隐式转换的显式转换是转换，无法证明始终成功，已知的转换可能会丢失信息，并且跨类型域的转换非常不同于显式图解.</span><span class="sxs-lookup"><span data-stu-id="56ea5-259">The explicit conversions that are not implicit conversions are conversions that cannot be proven to always succeed, conversions that are known to possibly lose information, and conversions across domains of types sufficiently different to merit explicit notation.</span></span>

### <a name="explicit-numeric-conversions"></a><span data-ttu-id="56ea5-260">显式数值转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-260">Explicit numeric conversions</span></span>

<span data-ttu-id="56ea5-261">显式数值转换是指从*numeric_type*到另一个*numeric_type*的转换，隐式数值转换（[隐式数值转换](conversions.md#implicit-numeric-conversions)）尚不存在：</span><span class="sxs-lookup"><span data-stu-id="56ea5-261">The explicit numeric conversions are the conversions from a *numeric_type* to another *numeric_type* for which an implicit numeric conversion ([Implicit numeric conversions](conversions.md#implicit-numeric-conversions)) does not already exist:</span></span>

*  <span data-ttu-id="56ea5-262">从 `sbyte` 到 `byte`、`ushort`、`uint`、`ulong`或 `char`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-262">From `sbyte` to `byte`, `ushort`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="56ea5-263">从 `byte` 到 `sbyte` 和 `char`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-263">From `byte` to `sbyte` and `char`.</span></span>
*  <span data-ttu-id="56ea5-264">从 `short` 到 `sbyte`、`byte`、`ushort`、`uint`、`ulong`或 `char`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-264">From `short` to `sbyte`, `byte`, `ushort`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="56ea5-265">从 `ushort` 到 `sbyte`、`byte`、`short`或 `char`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-265">From `ushort` to `sbyte`, `byte`, `short`, or `char`.</span></span>
*  <span data-ttu-id="56ea5-266">从 `int` 到 `sbyte`、`byte`、`short`、`ushort`、`uint`、`ulong`或 `char`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-266">From `int` to `sbyte`, `byte`, `short`, `ushort`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="56ea5-267">从 `uint` 到 `sbyte`、`byte`、`short`、`ushort`、`int`或 `char`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-267">From `uint` to `sbyte`, `byte`, `short`, `ushort`, `int`, or `char`.</span></span>
*  <span data-ttu-id="56ea5-268">从 `long` 到 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`ulong`或 `char`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-268">From `long` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `ulong`, or `char`.</span></span>
*  <span data-ttu-id="56ea5-269">从 `ulong` 到 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`或 `char`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-269">From `ulong` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `char`.</span></span>
*  <span data-ttu-id="56ea5-270">从 `char` 到 `sbyte`、`byte`或 `short`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-270">From `char` to `sbyte`, `byte`, or `short`.</span></span>
*  <span data-ttu-id="56ea5-271">从 `float` 到 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-271">From `float` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, or `decimal`.</span></span>
*  <span data-ttu-id="56ea5-272">从 `double` 到 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-272">From `double` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, or `decimal`.</span></span>
*  <span data-ttu-id="56ea5-273">从 `decimal` 到 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`或 `double`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-273">From `decimal` to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, or `double`.</span></span>

<span data-ttu-id="56ea5-274">由于显式转换包括所有隐式和显式数字转换，因此始终可以使用强制转换表达式（[强制](expressions.md#cast-expressions)转换表达式）将任何*numeric_type*转换为任何其他*numeric_type* 。</span><span class="sxs-lookup"><span data-stu-id="56ea5-274">Because the explicit conversions include all implicit and explicit numeric conversions, it is always possible to convert from any *numeric_type* to any other *numeric_type* using a cast expression ([Cast expressions](expressions.md#cast-expressions)).</span></span>

<span data-ttu-id="56ea5-275">显式数值转换可能会丢失信息，或可能导致引发异常。</span><span class="sxs-lookup"><span data-stu-id="56ea5-275">The explicit numeric conversions possibly lose information or possibly cause exceptions to be thrown.</span></span> <span data-ttu-id="56ea5-276">显式数值转换的处理方式如下：</span><span class="sxs-lookup"><span data-stu-id="56ea5-276">An explicit numeric conversion is processed as follows:</span></span>

*  <span data-ttu-id="56ea5-277">对于从整型转换为另一整型类型的转换，处理过程取决于发生转换的溢出检查上下文（[检查和未检查的运算符](expressions.md#the-checked-and-unchecked-operators)）：</span><span class="sxs-lookup"><span data-stu-id="56ea5-277">For a conversion from an integral type to another integral type, the processing depends on the overflow checking context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)) in which the conversion takes place:</span></span>
    * <span data-ttu-id="56ea5-278">在 `checked` 上下文中，如果源操作数的值在目标类型的范围内，则转换成功; 但如果源操作数的值超出了目标类型的范围，则会引发 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-278">In a `checked` context, the conversion succeeds if the value of the source operand is within the range of the destination type, but throws a `System.OverflowException` if the value of the source operand is outside the range of the destination type.</span></span>
    * <span data-ttu-id="56ea5-279">在 `unchecked` 上下文中，转换始终会成功，并按如下所示继续。</span><span class="sxs-lookup"><span data-stu-id="56ea5-279">In an `unchecked` context, the conversion always succeeds, and proceeds as follows.</span></span>
        * <span data-ttu-id="56ea5-280">如果源类型大于目标类型，则通过放弃其“额外”最高有效位来截断源值。</span><span class="sxs-lookup"><span data-stu-id="56ea5-280">If the source type is larger than the destination type, then the source value is truncated by discarding its "extra" most significant bits.</span></span> <span data-ttu-id="56ea5-281">结果会被视为目标类型的值。</span><span class="sxs-lookup"><span data-stu-id="56ea5-281">The result is then treated as a value of the destination type.</span></span>
        * <span data-ttu-id="56ea5-282">如果源类型小于目标类型，则源值是符号扩展或零扩展，以使其与目标类型的大小相同。</span><span class="sxs-lookup"><span data-stu-id="56ea5-282">If the source type is smaller than the destination type, then the source value is either sign-extended or zero-extended so that it is the same size as the destination type.</span></span> <span data-ttu-id="56ea5-283">如果源类型带符号，则是符号扩展；如果源类型是无符号的，则是零扩展。</span><span class="sxs-lookup"><span data-stu-id="56ea5-283">Sign-extension is used if the source type is signed; zero-extension is used if the source type is unsigned.</span></span> <span data-ttu-id="56ea5-284">结果会被视为目标类型的值。</span><span class="sxs-lookup"><span data-stu-id="56ea5-284">The result is then treated as a value of the destination type.</span></span>
        * <span data-ttu-id="56ea5-285">如果源类型与目标类型的大小相同，则源值将被视为目标类型的值。</span><span class="sxs-lookup"><span data-stu-id="56ea5-285">If the source type is the same size as the destination type, then the source value is treated as a value of the destination type.</span></span>
*  <span data-ttu-id="56ea5-286">对于从 `decimal` 到整数类型的转换，将源值向零舍入到最接近的整数值，并且此整数值将成为转换的结果。</span><span class="sxs-lookup"><span data-stu-id="56ea5-286">For a conversion from `decimal` to an integral type, the source value is rounded towards zero to the nearest integral value, and this integral value becomes the result of the conversion.</span></span> <span data-ttu-id="56ea5-287">如果生成的整数值超出了目标类型的范围，则会引发 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-287">If the resulting integral value is outside the range of the destination type, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="56ea5-288">对于从 `float` 或 `double` 到整数类型的转换，处理取决于发生转换的溢出检查上下文（[检查和未检查的运算符](expressions.md#the-checked-and-unchecked-operators)）：</span><span class="sxs-lookup"><span data-stu-id="56ea5-288">For a conversion from `float` or `double` to an integral type, the processing depends on the overflow checking context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)) in which the conversion takes place:</span></span>
    * <span data-ttu-id="56ea5-289">在 `checked` 上下文中，转换过程如下所示：</span><span class="sxs-lookup"><span data-stu-id="56ea5-289">In a `checked` context, the conversion proceeds as follows:</span></span>
        * <span data-ttu-id="56ea5-290">如果操作数的值为 NaN 或无穷大，则会引发 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-290">If the value of the operand is NaN or infinite, a `System.OverflowException` is thrown.</span></span>
        * <span data-ttu-id="56ea5-291">否则，源操作数向零舍入到最接近的整数值。</span><span class="sxs-lookup"><span data-stu-id="56ea5-291">Otherwise, the source operand is rounded towards zero to the nearest integral value.</span></span> <span data-ttu-id="56ea5-292">如果此整数值在目标类型的范围内，则此值为转换的结果。</span><span class="sxs-lookup"><span data-stu-id="56ea5-292">If this integral value is within the range of the destination type then this value is the result of the conversion.</span></span>
        * <span data-ttu-id="56ea5-293">否则，将会引发 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-293">Otherwise, a `System.OverflowException` is thrown.</span></span>
    * <span data-ttu-id="56ea5-294">在 `unchecked` 上下文中，转换始终会成功，并按如下所示继续。</span><span class="sxs-lookup"><span data-stu-id="56ea5-294">In an `unchecked` context, the conversion always succeeds, and proceeds as follows.</span></span>
        * <span data-ttu-id="56ea5-295">如果操作数的值为 NaN 或无穷大，则转换的结果是目标类型的未指定值。</span><span class="sxs-lookup"><span data-stu-id="56ea5-295">If the value of the operand is NaN or infinite, the result of the conversion is an unspecified value of the destination type.</span></span>
        * <span data-ttu-id="56ea5-296">否则，源操作数向零舍入到最接近的整数值。</span><span class="sxs-lookup"><span data-stu-id="56ea5-296">Otherwise, the source operand is rounded towards zero to the nearest integral value.</span></span> <span data-ttu-id="56ea5-297">如果此整数值在目标类型的范围内，则此值为转换的结果。</span><span class="sxs-lookup"><span data-stu-id="56ea5-297">If this integral value is within the range of the destination type then this value is the result of the conversion.</span></span>
        * <span data-ttu-id="56ea5-298">否则，转换的结果是目标类型的未指定值。</span><span class="sxs-lookup"><span data-stu-id="56ea5-298">Otherwise, the result of the conversion is an unspecified value of the destination type.</span></span>
*  <span data-ttu-id="56ea5-299">对于从 `double` 到 `float`的转换，`double` 值舍入为最接近的 `float` 值。</span><span class="sxs-lookup"><span data-stu-id="56ea5-299">For a conversion from `double` to `float`, the `double` value is rounded to the nearest `float` value.</span></span> <span data-ttu-id="56ea5-300">如果 `double` 值太小而无法表示为 `float`，则结果将变为零或负零。</span><span class="sxs-lookup"><span data-stu-id="56ea5-300">If the `double` value is too small to represent as a `float`, the result becomes positive zero or negative zero.</span></span> <span data-ttu-id="56ea5-301">如果 `double` 值太大而无法表示为 `float`，则结果将变为正无穷或负无穷。</span><span class="sxs-lookup"><span data-stu-id="56ea5-301">If the `double` value is too large to represent as a `float`, the result becomes positive infinity or negative infinity.</span></span> <span data-ttu-id="56ea5-302">如果 `double` 值为 NaN，则结果也为 NaN。</span><span class="sxs-lookup"><span data-stu-id="56ea5-302">If the `double` value is NaN, the result is also NaN.</span></span>
*  <span data-ttu-id="56ea5-303">对于从 `float` 或 `double` 到 `decimal`的转换，将源值转换为 `decimal` 表示形式，并在需要时舍入到第28位小数后最接近的数（[decimal 类型](types.md#the-decimal-type)）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-303">For a conversion from `float` or `double` to `decimal`, the source value is converted to `decimal` representation and rounded to the nearest number after the 28th decimal place if required ([The decimal type](types.md#the-decimal-type)).</span></span> <span data-ttu-id="56ea5-304">如果源值太小而无法表示为 `decimal`，则结果将变为零。</span><span class="sxs-lookup"><span data-stu-id="56ea5-304">If the source value is too small to represent as a `decimal`, the result becomes zero.</span></span> <span data-ttu-id="56ea5-305">如果源值为 NaN、无限大或太大而无法表示为 `decimal`，则会引发 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-305">If the source value is NaN, infinity, or too large to represent as a `decimal`, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="56ea5-306">对于从 `decimal` 到 `float` 或 `double`的转换，`decimal` 值舍入为最接近的 `double` 或 `float` 值。</span><span class="sxs-lookup"><span data-stu-id="56ea5-306">For a conversion from `decimal` to `float` or `double`, the `decimal` value is rounded to the nearest `double` or `float` value.</span></span> <span data-ttu-id="56ea5-307">虽然这种转换可能会丢失精度，但它永远不会引发异常。</span><span class="sxs-lookup"><span data-stu-id="56ea5-307">While this conversion may lose precision, it never causes an exception to be thrown.</span></span>

### <a name="explicit-enumeration-conversions"></a><span data-ttu-id="56ea5-308">显式枚举转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-308">Explicit enumeration conversions</span></span>

<span data-ttu-id="56ea5-309">显式枚举转换为：</span><span class="sxs-lookup"><span data-stu-id="56ea5-309">The explicit enumeration conversions are:</span></span>

*  <span data-ttu-id="56ea5-310">从 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`或 `decimal` 到任何*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="56ea5-310">From `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `decimal` to any *enum_type*.</span></span>
*  <span data-ttu-id="56ea5-311">从任何*enum_type*到 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`或 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-311">From any *enum_type* to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `decimal`.</span></span>
*  <span data-ttu-id="56ea5-312">从任何*enum_type*到任何其他*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="56ea5-312">From any *enum_type* to any other *enum_type*.</span></span>

<span data-ttu-id="56ea5-313">处理两种类型之间的显式枚举转换的方式是将任何参与的*enum_type*视为*enum_type*的基础类型，然后在生成的类型之间执行隐式或显式数字转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-313">An explicit enumeration conversion between two types is processed by treating any participating *enum_type* as the underlying type of that *enum_type*, and then performing an implicit or explicit numeric conversion between the resulting types.</span></span> <span data-ttu-id="56ea5-314">例如，给定*enum_type* `E`，以及基础类型 `int`，则 `E` 从 `byte` 到 `int` 的显式数字转换（从 `byte`到[`byte` 的显式](conversions.md#explicit-numeric-conversions)数字转换处理），并将从 `E` 到 `byte` 的转换处理为从 `int`到的隐式数值转换（[隐式数值](conversions.md#implicit-numeric-conversions)转换）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-314">For example, given an *enum_type* `E` with and underlying type of `int`, a conversion from `E` to `byte` is processed as an explicit numeric conversion ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from `int` to `byte`, and a conversion from `byte` to `E` is processed as an implicit numeric conversion ([Implicit numeric conversions](conversions.md#implicit-numeric-conversions)) from `byte` to `int`.</span></span>

### <a name="explicit-nullable-conversions"></a><span data-ttu-id="56ea5-315">显式可为 null 的转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-315">Explicit nullable conversions</span></span>

<span data-ttu-id="56ea5-316">***显式可以为 null 的转换***允许对不可以为 null 的值类型进行操作的预定义显式转换也可用于这些类型的可以为 null 的形式。</span><span class="sxs-lookup"><span data-stu-id="56ea5-316">***Explicit nullable conversions*** permit predefined explicit conversions that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="56ea5-317">对于从不可为 null 的值类型转换为不可以为 null 的值类型的每个预定义的显式转换 `S` `T` （[标识转换](conversions.md#identity-conversion)、[隐式数值转换](conversions.md#implicit-numeric-conversions)、[隐式枚举转换](conversions.md#implicit-enumeration-conversions)、[显式数字转换](conversions.md#explicit-numeric-conversions)和[显式枚举转换](conversions.md#explicit-enumeration-conversions)），可以为 null 的转换如下：</span><span class="sxs-lookup"><span data-stu-id="56ea5-317">For each of the predefined explicit conversions that convert from a non-nullable value type `S` to a non-nullable value type `T` ([Identity conversion](conversions.md#identity-conversion), [Implicit numeric conversions](conversions.md#implicit-numeric-conversions), [Implicit enumeration conversions](conversions.md#implicit-enumeration-conversions), [Explicit numeric conversions](conversions.md#explicit-numeric-conversions), and [Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)), the following nullable conversions exist:</span></span>

*  <span data-ttu-id="56ea5-318">从 `S?` 到 `T?`的显式转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-318">An explicit conversion from `S?` to `T?`.</span></span>
*  <span data-ttu-id="56ea5-319">从 `S` 到 `T?`的显式转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-319">An explicit conversion from `S` to `T?`.</span></span>
*  <span data-ttu-id="56ea5-320">从 `S?` 到 `T`的显式转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-320">An explicit conversion from `S?` to `T`.</span></span>

<span data-ttu-id="56ea5-321">基于从 `S` 到 `T` 的基础转换计算可以为 null 的转换，如下所示：</span><span class="sxs-lookup"><span data-stu-id="56ea5-321">Evaluation of a nullable conversion based on an underlying conversion from `S` to `T` proceeds as follows:</span></span>

*  <span data-ttu-id="56ea5-322">如果可为 null 的转换来自 `S?` 到 `T?`：</span><span class="sxs-lookup"><span data-stu-id="56ea5-322">If the nullable conversion is from `S?` to `T?`:</span></span>
    * <span data-ttu-id="56ea5-323">如果源值为 null （`HasValue` 属性为 false），则结果为类型 `T?`的 null 值。</span><span class="sxs-lookup"><span data-stu-id="56ea5-323">If the source value is null (`HasValue` property is false), the result is the null value of type `T?`.</span></span>
    * <span data-ttu-id="56ea5-324">否则，转换将作为从 `S?` 到 `S`的解包进行计算，后跟从 `S` 到 `T`的底层转换，然后是从 `T` 到 `T?`的包装。</span><span class="sxs-lookup"><span data-stu-id="56ea5-324">Otherwise, the conversion is evaluated as an unwrapping from `S?` to `S`, followed by the underlying conversion from `S` to `T`, followed by a wrapping from `T` to `T?`.</span></span>
*  <span data-ttu-id="56ea5-325">如果将可为 null 的转换来自 `S` 到 `T?`，则会将转换计算为从 `S` 到 `T` 后跟从 `T` 到 `T?`的基础转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-325">If the nullable conversion is from `S` to `T?`, the conversion is evaluated as the underlying conversion from `S` to `T` followed by a wrapping from `T` to `T?`.</span></span>
*  <span data-ttu-id="56ea5-326">如果将可为 null 的转换来自 `S?` 到 `T`，则会将转换计算为从 `S?` 到 `S` 的解包，后跟从 `S` 到 `T`的基础转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-326">If the nullable conversion is from `S?` to `T`, the conversion is evaluated as an unwrapping from `S?` to `S` followed by the underlying conversion from `S` to `T`.</span></span>

<span data-ttu-id="56ea5-327">请注意，如果值为 `null`，则尝试解包的值会引发异常。</span><span class="sxs-lookup"><span data-stu-id="56ea5-327">Note that an attempt to unwrap a nullable value will throw an exception if the value is `null`.</span></span>

### <a name="explicit-reference-conversions"></a><span data-ttu-id="56ea5-328">显式引用转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-328">Explicit reference conversions</span></span>

<span data-ttu-id="56ea5-329">显式引用转换为：</span><span class="sxs-lookup"><span data-stu-id="56ea5-329">The explicit reference conversions are:</span></span>

*  <span data-ttu-id="56ea5-330">从 `object` 和 `dynamic` 到任何其他*reference_type*。</span><span class="sxs-lookup"><span data-stu-id="56ea5-330">From `object` and `dynamic` to any other *reference_type*.</span></span>
*  <span data-ttu-id="56ea5-331">从任何*class_type* `S` 到任何*class_type* `T`，提供的 `S` 是 `T`的基类。</span><span class="sxs-lookup"><span data-stu-id="56ea5-331">From any *class_type* `S` to any *class_type* `T`, provided `S` is a base class of `T`.</span></span>
*  <span data-ttu-id="56ea5-332">从任何*class_type* `S` 到任何*interface_type* `T`，提供的 `S` 不是密封的，`S` 未实现 `T`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-332">From any *class_type* `S` to any *interface_type* `T`, provided `S` is not sealed and provided `S` does not implement `T`.</span></span>
*  <span data-ttu-id="56ea5-333">从任何*interface_type* `S` 到任何*class_type* `T`，提供的 `T` 未密封或未提供 `T` 实现 `S`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-333">From any *interface_type* `S` to any *class_type* `T`, provided `T` is not sealed or provided `T` implements `S`.</span></span>
*  <span data-ttu-id="56ea5-334">从任何*interface_type* `S` 到任何*interface_type* `T`，提供的 `S` 不是从 `T`派生的。</span><span class="sxs-lookup"><span data-stu-id="56ea5-334">From any *interface_type* `S` to any *interface_type* `T`, provided `S` is not derived from `T`.</span></span>
*  <span data-ttu-id="56ea5-335">在元素类型为的*array_type* `S` 中 `SE` 到元素类型 `T` 的*array_type* `TE`，前提是满足以下所有条件：</span><span class="sxs-lookup"><span data-stu-id="56ea5-335">From an *array_type* `S` with an element type `SE` to an *array_type* `T` with an element type `TE`, provided all of the following are true:</span></span>
    * <span data-ttu-id="56ea5-336">`S` 和 `T` 仅在元素类型上存在差异。</span><span class="sxs-lookup"><span data-stu-id="56ea5-336">`S` and `T` differ only in element type.</span></span> <span data-ttu-id="56ea5-337">换句话说，`S` 和 `T` 具有相同的维数。</span><span class="sxs-lookup"><span data-stu-id="56ea5-337">In other words, `S` and `T` have the same number of dimensions.</span></span>
    * <span data-ttu-id="56ea5-338">`SE` 和 `TE` *reference_type*。</span><span class="sxs-lookup"><span data-stu-id="56ea5-338">Both `SE` and `TE` are *reference_type*s.</span></span>
    * <span data-ttu-id="56ea5-339">存在从 `SE` 到 `TE`的显式引用转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-339">An explicit reference conversion exists from `SE` to `TE`.</span></span>
*  <span data-ttu-id="56ea5-340">从 `System.Array` 及其实现的接口到任何*array_type*。</span><span class="sxs-lookup"><span data-stu-id="56ea5-340">From `System.Array` and the interfaces it implements to any *array_type*.</span></span>
*  <span data-ttu-id="56ea5-341">从一维数组类型 `S[]` 到 `System.Collections.Generic.IList<T>` 及其基接口，前提是存在从 `S` 到 `T`的显式引用转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-341">From a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its base interfaces, provided that there is an explicit reference conversion from `S` to `T`.</span></span>
*  <span data-ttu-id="56ea5-342">从 `System.Collections.Generic.IList<S>` 及其基接口到一维数组类型 `T[]`，前提是有显式标识或从 `S` 到 `T`的引用转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-342">From `System.Collections.Generic.IList<S>` and its base interfaces to a single-dimensional array type `T[]`, provided that there is an explicit identity or reference conversion from `S` to `T`.</span></span>
*  <span data-ttu-id="56ea5-343">从 `System.Delegate` 及其实现的接口到任何*delegate_type*。</span><span class="sxs-lookup"><span data-stu-id="56ea5-343">From `System.Delegate` and the interfaces it implements to any *delegate_type*.</span></span>
*  <span data-ttu-id="56ea5-344">从引用类型到引用类型 `T` 如果它具有到引用类型的显式引用转换 `T0` 并且 `T0` 具有 `T`的标识转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-344">From a reference type to a reference type `T` if it has an explicit reference conversion to a reference type `T0` and `T0` has an identity conversion `T`.</span></span>
*  <span data-ttu-id="56ea5-345">从引用类型到接口或委托类型 `T` 如果它具有到接口或委托类型的显式引用转换 `T0` 并且 `T0` 可以转换为 `T` 或 `T` 变体转换为 `T0` （[变体转换](interfaces.md#variance-conversion)）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-345">From a reference type to an interface or delegate type `T` if it has an explicit reference conversion to an interface or delegate type `T0` and either `T0` is variance-convertible to `T` or `T` is variance-convertible to `T0` ([Variance conversion](interfaces.md#variance-conversion)).</span></span>
*  <span data-ttu-id="56ea5-346">从 `D<S1...Sn>` 到 `D<T1...Tn>`，其中 `D<X1...Xn>` 是一个泛型委托类型，`D<S1...Sn>` 与 `D<T1...Tn>`不兼容或等同于 `Xi`，而对于每个类型参数 `D`，则以下保留：</span><span class="sxs-lookup"><span data-stu-id="56ea5-346">From `D<S1...Sn>` to `D<T1...Tn>` where `D<X1...Xn>` is a generic delegate type, `D<S1...Sn>` is not compatible with or identical to `D<T1...Tn>`, and for each type parameter `Xi` of `D` the following holds:</span></span>
    * <span data-ttu-id="56ea5-347">如果 `Xi` 固定，则 `Si` 与 `Ti`相同。</span><span class="sxs-lookup"><span data-stu-id="56ea5-347">If `Xi` is invariant, then `Si` is identical to `Ti`.</span></span>
    * <span data-ttu-id="56ea5-348">如果 `Xi` 是协变的，则存在从 `Si` 到 `Ti`的隐式或显式标识或引用转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-348">If `Xi` is covariant, then there is an implicit or explicit identity or reference conversion from `Si` to `Ti`.</span></span>
    * <span data-ttu-id="56ea5-349">如果 `Xi` 为逆变，则 `Si` 和 `Ti` 均为相同或同时为这两个引用类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-349">If `Xi` is contravariant, then `Si` and `Ti` are either identical or both reference types.</span></span>
*  <span data-ttu-id="56ea5-350">涉及称为引用类型的类型参数的显式转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-350">Explicit conversions involving type parameters that are known to be reference types.</span></span> <span data-ttu-id="56ea5-351">有关涉及类型参数的显式转换的详细信息，请参阅[涉及类型参数的显式转换](conversions.md#explicit-conversions-involving-type-parameters)。</span><span class="sxs-lookup"><span data-stu-id="56ea5-351">For more details on explicit conversions involving type parameters, see [Explicit conversions involving type parameters](conversions.md#explicit-conversions-involving-type-parameters).</span></span>

<span data-ttu-id="56ea5-352">显式引用转换是需要运行时检查以确保它们正确的引用类型之间的转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-352">The explicit reference conversions are those conversions between reference-types that require run-time checks to ensure they are correct.</span></span>

<span data-ttu-id="56ea5-353">若要在运行时成功进行显式引用转换，源操作数的值必须为 `null`，或源操作数引用的对象的实际类型必须是可通过隐式引用转换（[隐式引用](conversions.md#implicit-reference-conversions)转换）或装箱转换（[装箱](conversions.md#boxing-conversions)转换）转换为目标类型的类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-353">For an explicit reference conversion to succeed at run-time, the value of the source operand must be `null`, or the actual type of the object referenced by the source operand must be a type that can be converted to the destination type by an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) or boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)).</span></span> <span data-ttu-id="56ea5-354">如果显式引用转换失败，则会引发 `System.InvalidCastException`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-354">If an explicit reference conversion fails, a `System.InvalidCastException` is thrown.</span></span>

<span data-ttu-id="56ea5-355">引用转换、隐式或显式转换决不会更改正在转换的对象的引用标识。</span><span class="sxs-lookup"><span data-stu-id="56ea5-355">Reference conversions, implicit or explicit, never change the referential identity of the object being converted.</span></span> <span data-ttu-id="56ea5-356">换言之，虽然引用转换可以更改引用的类型，但它不会更改引用的对象的类型或值。</span><span class="sxs-lookup"><span data-stu-id="56ea5-356">In other words, while a reference conversion may change the type of the reference, it never changes the type or value of the object being referred to.</span></span>

### <a name="unboxing-conversions"></a><span data-ttu-id="56ea5-357">取消装箱转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-357">Unboxing conversions</span></span>

<span data-ttu-id="56ea5-358">取消装箱转换允许将引用类型显式转换为*value_type*。</span><span class="sxs-lookup"><span data-stu-id="56ea5-358">An unboxing conversion permits a reference type to be explicitly converted to a *value_type*.</span></span> <span data-ttu-id="56ea5-359">从类型 `object`、`dynamic` 和 `System.ValueType` 到任何*non_nullable_value_type*，以及从任何*interface_type*到实现*non_nullable_value_type*的任意*interface_type*的取消装箱转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-359">An unboxing conversion exists from the types `object`, `dynamic` and `System.ValueType` to any *non_nullable_value_type*, and from any *interface_type* to any *non_nullable_value_type* that implements the *interface_type*.</span></span> <span data-ttu-id="56ea5-360">此外，可以将 `System.Enum` 类型取消装箱到任何*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="56ea5-360">Furthermore type `System.Enum` can be unboxed to any *enum_type*.</span></span>

<span data-ttu-id="56ea5-361">如果从引用类型到*nullable_type*的基础*non_nullable_value_type*的取消装箱转换存在，则取消装箱转换将从引用类型转换为*nullable_type* 。</span><span class="sxs-lookup"><span data-stu-id="56ea5-361">An unboxing conversion exists from a reference type to a *nullable_type* if an unboxing conversion exists from the reference type to the underlying *non_nullable_value_type* of the *nullable_type*.</span></span>

<span data-ttu-id="56ea5-362">值类型 `S` 具有从接口类型的取消装箱转换 `I` 如果它具有从接口类型的取消装箱转换 `I0` 并且 `I0` 具有到 `I`的标识转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-362">A value type `S` has an unboxing conversion from an interface type `I` if it has an unboxing conversion from an interface type `I0` and `I0` has an identity conversion to `I`.</span></span>

<span data-ttu-id="56ea5-363">值类型 `S` 具有从接口类型的取消装箱转换 `I` 如果该类型具有从接口或委托类型的取消装箱转换 `I0` 并且 `I0` 可以转换为 `I` 或 `I` 可转换为 `I0` （[变体转换](interfaces.md#variance-conversion)）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-363">A value type `S` has an unboxing conversion from an interface type `I` if it has an unboxing conversion from an interface or delegate type `I0` and either `I0` is variance-convertible to `I` or `I` is variance-convertible to `I0` ([Variance conversion](interfaces.md#variance-conversion)).</span></span>

<span data-ttu-id="56ea5-364">取消装箱操作包括：首先检查对象实例是否是给定*value_type*的装箱值，然后将值复制到该实例之外。</span><span class="sxs-lookup"><span data-stu-id="56ea5-364">An unboxing operation consists of first checking that the object instance is a boxed value of the given *value_type*, and then copying the value out of the instance.</span></span> <span data-ttu-id="56ea5-365">取消装箱对*nullable_type*的空引用将生成*nullable_type*的 null 值。</span><span class="sxs-lookup"><span data-stu-id="56ea5-365">Unboxing a null reference to a *nullable_type* produces the null value of the *nullable_type*.</span></span> <span data-ttu-id="56ea5-366">结构可以从类型 `System.ValueType`取消装箱，因为这是所有结构（[继承](structs.md#inheritance)）的基类。</span><span class="sxs-lookup"><span data-stu-id="56ea5-366">A struct can be unboxed from the type `System.ValueType`, since that is a base class for all structs ([Inheritance](structs.md#inheritance)).</span></span>

<span data-ttu-id="56ea5-367">取消装箱转换中进一步介绍[了取消装箱](types.md#unboxing-conversions)转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-367">Unboxing conversions are described further in [Unboxing conversions](types.md#unboxing-conversions).</span></span>

### <a name="explicit-dynamic-conversions"></a><span data-ttu-id="56ea5-368">显式动态转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-368">Explicit dynamic conversions</span></span>

<span data-ttu-id="56ea5-369">存在从 `dynamic` 类型的表达式到任何类型 `T`的显式动态转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-369">An explicit dynamic conversion exists from an expression of type `dynamic` to any type `T`.</span></span> <span data-ttu-id="56ea5-370">转换是动态绑定的（[动态绑定](expressions.md#dynamic-binding)），这意味着将在运行时从表达式的运行时类型中查找显式转换，以便 `T`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-370">The conversion is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), which means that an explicit conversion will be sought at run-time from the run-time type of the expression to `T`.</span></span> <span data-ttu-id="56ea5-371">如果未找到任何转换，则会引发运行时异常。</span><span class="sxs-lookup"><span data-stu-id="56ea5-371">If no conversion is found, a run-time exception is thrown.</span></span>

<span data-ttu-id="56ea5-372">如果不需要转换的动态绑定，则可以先将表达式转换为 `object`，然后转换为所需的类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-372">If dynamic binding of the conversion is not desired, the expression can be first converted to `object`, and then to the desired type.</span></span>

<span data-ttu-id="56ea5-373">假定定义了下面的类：</span><span class="sxs-lookup"><span data-stu-id="56ea5-373">Assume the following class is defined:</span></span>
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

<span data-ttu-id="56ea5-374">下面的示例阐释了显式动态转换：</span><span class="sxs-lookup"><span data-stu-id="56ea5-374">The following example illustrates explicit dynamic conversions:</span></span>
```csharp
object o  = "1";
dynamic d = "2";

var c1 = (C)o; // Compiles, but explicit reference conversion fails
var c2 = (C)d; // Compiles and user defined conversion succeeds
```

<span data-ttu-id="56ea5-375">在编译时可以找到 `o` 到 `C` 的最佳转换，以便成为显式引用转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-375">The best conversion of `o` to `C` is found at compile-time to be an explicit reference conversion.</span></span> <span data-ttu-id="56ea5-376">这会在运行时失败，因为 `"1"` 事实上并不是 `C`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-376">This fails at run-time, because `"1"` is not in fact a `C`.</span></span> <span data-ttu-id="56ea5-377">`d` 转换为 `C` 但是，作为显式动态转换，会将其挂起到运行时，其中，用户从 `d` -- `string` 到 `C` 的运行时类型进行了定义的转换，并成功了。</span><span class="sxs-lookup"><span data-stu-id="56ea5-377">The conversion of `d` to `C` however, as an explicit dynamic conversion, is suspended to run-time, where a user defined conversion from the run-time type of `d` -- `string` -- to `C` is found, and succeeds.</span></span>

### <a name="explicit-conversions-involving-type-parameters"></a><span data-ttu-id="56ea5-378">涉及类型参数的显式转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-378">Explicit conversions involving type parameters</span></span>

<span data-ttu-id="56ea5-379">`T`的给定类型参数存在以下显式转换：</span><span class="sxs-lookup"><span data-stu-id="56ea5-379">The following explicit conversions exist for a given type parameter `T`:</span></span>

*  <span data-ttu-id="56ea5-380">从有效的基类 `C` 的 `T` 到 `T` 和从 `C` 的任何基类到 `T`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-380">From the effective base class `C` of `T` to `T` and from any base class of `C` to `T`.</span></span> <span data-ttu-id="56ea5-381">在运行时，如果 `T` 是值类型，则转换将作为取消装箱转换执行。</span><span class="sxs-lookup"><span data-stu-id="56ea5-381">At run-time, if `T` is a value type, the conversion is executed as an unboxing conversion.</span></span> <span data-ttu-id="56ea5-382">否则，转换将作为显式引用转换或标识转换执行。</span><span class="sxs-lookup"><span data-stu-id="56ea5-382">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="56ea5-383">从任何接口类型到 `T`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-383">From any interface type to `T`.</span></span> <span data-ttu-id="56ea5-384">在运行时，如果 `T` 是值类型，则转换将作为取消装箱转换执行。</span><span class="sxs-lookup"><span data-stu-id="56ea5-384">At run-time, if `T` is a value type, the conversion is executed as an unboxing conversion.</span></span> <span data-ttu-id="56ea5-385">否则，转换将作为显式引用转换或标识转换执行。</span><span class="sxs-lookup"><span data-stu-id="56ea5-385">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="56ea5-386">从 `T` 到任何*interface_type* `I` 如果尚未从 `T` 到 `I`的隐式转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-386">From `T` to any *interface_type* `I` provided there is not already an implicit conversion from `T` to `I`.</span></span> <span data-ttu-id="56ea5-387">在运行时，如果 `T` 是值类型，则转换将作为装箱转换执行，后跟显式引用转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-387">At run-time, if `T` is a value type, the conversion is executed as a boxing conversion followed by an explicit reference conversion.</span></span> <span data-ttu-id="56ea5-388">否则，转换将作为显式引用转换或标识转换执行。</span><span class="sxs-lookup"><span data-stu-id="56ea5-388">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>
*  <span data-ttu-id="56ea5-389">从类型参数 `U` 到 `T`，提供 `T` 依赖于 `U` （[类型参数约束](classes.md#type-parameter-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-389">From a type parameter `U` to `T`, provided `T` depends on `U` ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="56ea5-390">在运行时，如果 `U` 是值类型，则 `T` 和 `U` 都必须是相同的类型，并且不执行任何转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-390">At run-time, if `U` is a value type, then `T` and `U` are necessarily the same type and no conversion is performed.</span></span> <span data-ttu-id="56ea5-391">否则，如果 `T` 是值类型，则转换将作为取消装箱转换执行。</span><span class="sxs-lookup"><span data-stu-id="56ea5-391">Otherwise, if `T` is a value type, the conversion is executed as an unboxing conversion.</span></span> <span data-ttu-id="56ea5-392">否则，转换将作为显式引用转换或标识转换执行。</span><span class="sxs-lookup"><span data-stu-id="56ea5-392">Otherwise, the conversion is executed as an explicit reference conversion or identity conversion.</span></span>

<span data-ttu-id="56ea5-393">如果已知 `T` 是引用类型，则上述转换全都归类为显式引用转换（[显式引用转换](conversions.md#explicit-reference-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-393">If `T` is known to be a reference type, the conversions above are all classified as explicit reference conversions ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span> <span data-ttu-id="56ea5-394">如果 `T` 不知道是引用类型，则上述转换归类为取消装箱转换（[取消装箱转换](conversions.md#unboxing-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-394">If `T` is not known to be a reference type, the conversions above are classified as unboxing conversions ([Unboxing conversions](conversions.md#unboxing-conversions)).</span></span>

<span data-ttu-id="56ea5-395">以上规则不允许从不受约束的类型形参直接转换为非接口类型，这可能会令人吃惊。</span><span class="sxs-lookup"><span data-stu-id="56ea5-395">The above rules do not permit a direct explicit conversion from an unconstrained type parameter to a non-interface type, which might be surprising.</span></span> <span data-ttu-id="56ea5-396">此规则的原因是为了防止混淆并使此类转换的语义清晰。</span><span class="sxs-lookup"><span data-stu-id="56ea5-396">The reason for this rule is to prevent confusion and make the semantics of such conversions clear.</span></span> <span data-ttu-id="56ea5-397">例如，请考虑以下声明：</span><span class="sxs-lookup"><span data-stu-id="56ea5-397">For example, consider the following declaration:</span></span>
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)t;                // Error 
    }
}
```

<span data-ttu-id="56ea5-398">如果允许 `t` 到 `int` 的直接显式转换，则很可能会认为 `X<int>.F(7)` 会返回 `7L`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-398">If the direct explicit conversion of `t` to `int` were permitted, one might easily expect that `X<int>.F(7)` would return `7L`.</span></span> <span data-ttu-id="56ea5-399">但是，它不会这样，因为仅当已知类型在绑定时是数字时才考虑标准数字转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-399">However, it would not, because the standard numeric conversions are only considered when the types are known to be numeric at binding-time.</span></span> <span data-ttu-id="56ea5-400">为了使语义清晰明了，必须改为编写以上示例：</span><span class="sxs-lookup"><span data-stu-id="56ea5-400">In order to make the semantics clear, the above example must instead be written:</span></span>
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)(object)t;        // Ok, but will only work when T is long
    }
}
```

<span data-ttu-id="56ea5-401">此代码现在将进行编译，但执行 `X<int>.F(7)` 稍后会在运行时引发异常，因为无法直接将装箱 `int` 转换为 `long`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-401">This code will now compile but executing `X<int>.F(7)` would then throw an exception at run-time, since a boxed `int` cannot be converted directly to a `long`.</span></span>

### <a name="user-defined-explicit-conversions"></a><span data-ttu-id="56ea5-402">用户定义的显式转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-402">User-defined explicit conversions</span></span>

<span data-ttu-id="56ea5-403">用户定义的显式转换包含一个可选的标准显式转换，然后执行用户定义的隐式或显式转换运算符，然后执行另一个可选的标准显式转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-403">A user-defined explicit conversion consists of an optional standard explicit conversion, followed by execution of a user-defined implicit or explicit conversion operator, followed by another optional standard explicit conversion.</span></span> <span data-ttu-id="56ea5-404">用于评估用户定义的显式转换的确切规则在[处理用户定义的显式转换](conversions.md#processing-of-user-defined-explicit-conversions)中进行了介绍。</span><span class="sxs-lookup"><span data-stu-id="56ea5-404">The exact rules for evaluating user-defined explicit conversions are described in [Processing of user-defined explicit conversions](conversions.md#processing-of-user-defined-explicit-conversions).</span></span>

## <a name="standard-conversions"></a><span data-ttu-id="56ea5-405">标准转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-405">Standard conversions</span></span>

<span data-ttu-id="56ea5-406">标准转换是那些预定义的转换，这些转换可作为用户定义的转换的一部分出现。</span><span class="sxs-lookup"><span data-stu-id="56ea5-406">The standard conversions are those pre-defined conversions that can occur as part of a user-defined conversion.</span></span>

### <a name="standard-implicit-conversions"></a><span data-ttu-id="56ea5-407">标准隐式转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-407">Standard implicit conversions</span></span>

<span data-ttu-id="56ea5-408">以下隐式转换归类为标准隐式转换：</span><span class="sxs-lookup"><span data-stu-id="56ea5-408">The following implicit conversions are classified as standard implicit conversions:</span></span>

*  <span data-ttu-id="56ea5-409">标识转换（[标识转换](conversions.md#identity-conversion)）</span><span class="sxs-lookup"><span data-stu-id="56ea5-409">Identity conversions ([Identity conversion](conversions.md#identity-conversion))</span></span>
*  <span data-ttu-id="56ea5-410">隐式数值转换（[隐式数值转换](conversions.md#implicit-numeric-conversions)）</span><span class="sxs-lookup"><span data-stu-id="56ea5-410">Implicit numeric conversions ([Implicit numeric conversions](conversions.md#implicit-numeric-conversions))</span></span>
*  <span data-ttu-id="56ea5-411">隐式可为 null 转换（[隐式可为 null 转换](conversions.md#implicit-nullable-conversions)）</span><span class="sxs-lookup"><span data-stu-id="56ea5-411">Implicit nullable conversions ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions))</span></span>
*  <span data-ttu-id="56ea5-412">隐式引用转换（[隐式引用转换](conversions.md#implicit-reference-conversions)）</span><span class="sxs-lookup"><span data-stu-id="56ea5-412">Implicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions))</span></span>
*  <span data-ttu-id="56ea5-413">装箱转换（[装箱转换](conversions.md#boxing-conversions)）</span><span class="sxs-lookup"><span data-stu-id="56ea5-413">Boxing conversions ([Boxing conversions](conversions.md#boxing-conversions))</span></span>
*  <span data-ttu-id="56ea5-414">隐式常量表达式转换（[隐式动态转换](conversions.md#implicit-dynamic-conversions)）</span><span class="sxs-lookup"><span data-stu-id="56ea5-414">Implicit constant expression conversions ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions))</span></span>
*  <span data-ttu-id="56ea5-415">涉及类型参数的隐式转换（[涉及类型参数的隐式转换](conversions.md#implicit-conversions-involving-type-parameters)）</span><span class="sxs-lookup"><span data-stu-id="56ea5-415">Implicit conversions involving type parameters ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters))</span></span>

<span data-ttu-id="56ea5-416">标准隐式转换专门排除用户定义的隐式转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-416">The standard implicit conversions specifically exclude user-defined implicit conversions.</span></span>

### <a name="standard-explicit-conversions"></a><span data-ttu-id="56ea5-417">标准显式转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-417">Standard explicit conversions</span></span>

<span data-ttu-id="56ea5-418">标准显式转换均为标准隐式转换，以及与之相反的标准隐式转换的显式转换的子集。</span><span class="sxs-lookup"><span data-stu-id="56ea5-418">The standard explicit conversions are all standard implicit conversions plus the subset of the explicit conversions for which an opposite standard implicit conversion exists.</span></span> <span data-ttu-id="56ea5-419">换言之，如果存在从类型 `A` 到类型 `B`的标准隐式转换，则从类型 `A` 到类型 `B`，并从类型 `B` 到类型 `A`，都存在标准的显式转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-419">In other words, if a standard implicit conversion exists from a type `A` to a type `B`, then a standard explicit conversion exists from type `A` to type `B` and from type `B` to type `A`.</span></span>

## <a name="user-defined-conversions"></a><span data-ttu-id="56ea5-420">用户定义的转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-420">User-defined conversions</span></span>

<span data-ttu-id="56ea5-421">C#允许使用***用户定义的转换***来扩充预定义的隐式和显式转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-421">C# allows the pre-defined implicit and explicit conversions to be augmented by ***user-defined conversions***.</span></span> <span data-ttu-id="56ea5-422">用户定义的转换通过在类和结构类型中声明转换运算符（[转换运算符](classes.md#conversion-operators)）引入。</span><span class="sxs-lookup"><span data-stu-id="56ea5-422">User-defined conversions are introduced by declaring conversion operators ([Conversion operators](classes.md#conversion-operators)) in class and struct types.</span></span>

### <a name="permitted-user-defined-conversions"></a><span data-ttu-id="56ea5-423">允许的用户定义的转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-423">Permitted user-defined conversions</span></span>

<span data-ttu-id="56ea5-424">C#仅允许声明某些用户定义的转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-424">C# permits only certain user-defined conversions to be declared.</span></span> <span data-ttu-id="56ea5-425">特别是，不能重新定义已存在的隐式或显式转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-425">In particular, it is not possible to redefine an already existing implicit or explicit conversion.</span></span>

<span data-ttu-id="56ea5-426">对于给定的源类型 `S` 和目标类型 `T`，如果 `S` 或 `T` 是可以为 null 的类型，则允许 `S0` 和 `T0` 引用它们的基础类型，否则 `S0` 和 `T0` 分别等于 `S` 和 `T`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-426">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="56ea5-427">仅当满足以下所有条件时，才允许类或结构声明从源类型 `S` 转换为目标类型 `T`：</span><span class="sxs-lookup"><span data-stu-id="56ea5-427">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="56ea5-428">`S0` 和 `T0` 属于不同类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-428">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="56ea5-429">`S0` 或 `T0` 是发生运算符声明的类或结构类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-429">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="56ea5-430">`S0` 和 `T0` 都不是*interface_type*的。</span><span class="sxs-lookup"><span data-stu-id="56ea5-430">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="56ea5-431">如果不包括用户定义的转换，则从 `S` 到 `T` 或从 `T` 到 `S`的转换不存在。</span><span class="sxs-lookup"><span data-stu-id="56ea5-431">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="56ea5-432">适用于用户定义的转换的限制将在[转换运算符](classes.md#conversion-operators)中进一步讨论。</span><span class="sxs-lookup"><span data-stu-id="56ea5-432">The restrictions that apply to user-defined conversions are discussed further in [Conversion operators](classes.md#conversion-operators).</span></span>

### <a name="lifted-conversion-operators"></a><span data-ttu-id="56ea5-433">提升的转换运算符</span><span class="sxs-lookup"><span data-stu-id="56ea5-433">Lifted conversion operators</span></span>

<span data-ttu-id="56ea5-434">给定一个用户定义的转换运算符，该运算符将不可以为 null 的值类型 `S` 转换为不可以为 null 的值类型 `T`，存在从 `S?` 转换为 `T?`的***提升转换运算符***。</span><span class="sxs-lookup"><span data-stu-id="56ea5-434">Given a user-defined conversion operator that converts from a non-nullable value type `S` to a non-nullable value type `T`, a ***lifted conversion operator*** exists that converts from `S?` to `T?`.</span></span> <span data-ttu-id="56ea5-435">此提升转换运算符执行从 `S?` 解包到 `S`，然后执行从 `S` 到 `T` 的用户定义的转换，后跟从 `T` 到 `T?`的包装，只不过 null 值 `S?` 直接转换为 null 值 `T?`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-435">This lifted conversion operator performs an unwrapping from `S?` to `S` followed by the user-defined conversion from `S` to `T` followed by a wrapping from `T` to `T?`, except that a null valued `S?` converts directly to a null valued `T?`.</span></span>

<span data-ttu-id="56ea5-436">提升的转换运算符与其基础用户定义转换运算符具有相同的隐式或显式分类。</span><span class="sxs-lookup"><span data-stu-id="56ea5-436">A lifted conversion operator has the same implicit or explicit classification as its underlying user-defined conversion operator.</span></span> <span data-ttu-id="56ea5-437">"用户定义的转换" 一词适用于用户定义的转换运算符和提升的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="56ea5-437">The term "user-defined conversion" applies to the use of both user-defined and lifted conversion operators.</span></span>

### <a name="evaluation-of-user-defined-conversions"></a><span data-ttu-id="56ea5-438">计算用户定义的转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-438">Evaluation of user-defined conversions</span></span>

<span data-ttu-id="56ea5-439">用户定义的转换将值从其类型（称为***源类型***）转换为另一种类型，称为***目标类型***。</span><span class="sxs-lookup"><span data-stu-id="56ea5-439">A user-defined conversion converts a value from its type, called the ***source type***, to another type, called the ***target type***.</span></span> <span data-ttu-id="56ea5-440">计算用户定义的转换中心，查找特定源和目标类型的***最特定***用户定义转换运算符。</span><span class="sxs-lookup"><span data-stu-id="56ea5-440">Evaluation of a user-defined conversion centers on finding the ***most specific*** user-defined conversion operator for the particular source and target types.</span></span> <span data-ttu-id="56ea5-441">此确定分为几个步骤：</span><span class="sxs-lookup"><span data-stu-id="56ea5-441">This determination is broken into several steps:</span></span>

*  <span data-ttu-id="56ea5-442">查找要将用户定义的转换运算符视为其中的一组类和结构。</span><span class="sxs-lookup"><span data-stu-id="56ea5-442">Finding the set of classes and structs from which user-defined conversion operators will be considered.</span></span> <span data-ttu-id="56ea5-443">此集由源类型及其基类和目标类型及其基类组成（隐式假设只有类和结构可以声明用户定义的运算符，而非类类型没有基类）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-443">This set consists of the source type and its base classes and the target type and its base classes (with the implicit assumptions that only classes and structs can declare user-defined operators, and that non-class types have no base classes).</span></span> <span data-ttu-id="56ea5-444">对于此步骤，如果源类型或目标类型为*nullable_type*，则改为使用它们的基础类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-444">For the purposes of this step, if either the source or target type is a *nullable_type*, their underlying type is used instead.</span></span>
*  <span data-ttu-id="56ea5-445">从该类型集中确定哪些用户定义的转换运算符适用。</span><span class="sxs-lookup"><span data-stu-id="56ea5-445">From that set of types, determining which user-defined and lifted conversion operators are applicable.</span></span> <span data-ttu-id="56ea5-446">要使转换运算符适用，必须可以执行标准转换（[标准](conversions.md#standard-conversions)转换），使其从源类型转换为运算符的操作数类型，并且必须能够执行从运算符的结果类型到目标类型的标准转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-446">For a conversion operator to be applicable, it must be possible to perform a standard conversion ([Standard conversions](conversions.md#standard-conversions)) from the source type to the operand type of the operator, and it must be possible to perform a standard conversion from the result type of the operator to the target type.</span></span>
*  <span data-ttu-id="56ea5-447">从一组适用的用户定义的运算符，确定哪个运算符明确是最具体的。</span><span class="sxs-lookup"><span data-stu-id="56ea5-447">From the set of applicable user-defined operators, determining which operator is unambiguously the most specific.</span></span> <span data-ttu-id="56ea5-448">一般来说，最特定的运算符是操作数类型与源类型 "最接近" 的运算符，其结果类型与目标类型 "最近"。</span><span class="sxs-lookup"><span data-stu-id="56ea5-448">In general terms, the most specific operator is the operator whose operand type is "closest" to the source type and whose result type is "closest" to the target type.</span></span> <span data-ttu-id="56ea5-449">用户定义的转换运算符优先于提升转换运算符。</span><span class="sxs-lookup"><span data-stu-id="56ea5-449">User-defined conversion operators are preferred over lifted conversion operators.</span></span> <span data-ttu-id="56ea5-450">以下部分定义了用于建立最具体的用户定义转换运算符的确切规则。</span><span class="sxs-lookup"><span data-stu-id="56ea5-450">The exact rules for establishing the most specific user-defined conversion operator are defined in the following sections.</span></span>

<span data-ttu-id="56ea5-451">确定了最特定的用户定义转换运算符后，用户定义的转换的实际执行操作包括多达三个步骤：</span><span class="sxs-lookup"><span data-stu-id="56ea5-451">Once a most specific user-defined conversion operator has been identified, the actual execution of the user-defined conversion involves up to three steps:</span></span>

*  <span data-ttu-id="56ea5-452">首先，如果需要，执行从源类型到用户定义或提升的转换运算符的操作数类型的标准转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-452">First, if required, performing a standard conversion from the source type to the operand type of the user-defined or lifted conversion operator.</span></span>
*  <span data-ttu-id="56ea5-453">接下来，调用用户定义或提升的转换运算符来执行转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-453">Next, invoking the user-defined or lifted conversion operator to perform the conversion.</span></span>
*  <span data-ttu-id="56ea5-454">最后，如果需要，执行从用户定义转换运算符的结果类型到目标类型的标准转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-454">Finally, if required, performing a standard conversion from the result type of the user-defined or lifted conversion operator to the target type.</span></span>

<span data-ttu-id="56ea5-455">用户定义的转换的计算从不涉及多个用户定义的转换运算符或提升的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="56ea5-455">Evaluation of a user-defined conversion never involves more than one user-defined or lifted conversion operator.</span></span> <span data-ttu-id="56ea5-456">换句话说，从类型 `S` 到类型 `T` 的转换永远不会首先执行从 `S` 到 `X` 的用户定义的转换，然后执行从 `X` 到 `T`的用户定义的转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-456">In other words, a conversion from type `S` to type `T` will never first execute a user-defined conversion from `S` to `X` and then execute a user-defined conversion from `X` to `T`.</span></span>

<span data-ttu-id="56ea5-457">以下各节提供了用户定义的隐式或显式转换的确切计算定义。</span><span class="sxs-lookup"><span data-stu-id="56ea5-457">Exact definitions of evaluation of user-defined implicit or explicit conversions are given in the following sections.</span></span> <span data-ttu-id="56ea5-458">定义使用以下术语：</span><span class="sxs-lookup"><span data-stu-id="56ea5-458">The definitions make use of the following terms:</span></span>

*  <span data-ttu-id="56ea5-459">如果从类型 `A` 到类型 `B`存在标准隐式转换（[标准隐](conversions.md#standard-implicit-conversions)式转换），并且 `A` 和 `B` 都不*interface_type*，则 `A` 被视为***包含***`B`，`B` 称为***包含***`A`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-459">If a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) exists from a type `A` to a type `B`, and if neither `A` nor `B` are *interface_type*s, then `A` is said to be ***encompassed by*** `B`, and `B` is said to ***encompass*** `A`.</span></span>
*  <span data-ttu-id="56ea5-460">一组类型中包含的***最大的类型***是一种包含集内所有其他类型的类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-460">The ***most encompassing type*** in a set of types is the one type that encompasses all other types in the set.</span></span> <span data-ttu-id="56ea5-461">如果没有一种类型包含所有其他类型，则该集没有最大的包含类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-461">If no single type encompasses all other types, then the set has no most encompassing type.</span></span> <span data-ttu-id="56ea5-462">更直观地说，最包含的类型是集中的 "最大" 类型，每个其他类型都可以隐式转换为一种类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-462">In more intuitive terms, the most encompassing type is the "largest" type in the set—the one type to which each of the other types can be implicitly converted.</span></span>
*  <span data-ttu-id="56ea5-463">类型集中***最常被包含的类型***是一种类型，它由集中的所有其他类型所包含。</span><span class="sxs-lookup"><span data-stu-id="56ea5-463">The ***most encompassed type*** in a set of types is the one type that is encompassed by all other types in the set.</span></span> <span data-ttu-id="56ea5-464">如果所有其他类型都不包含任何一种类型，则该集不包含大多数被包含的类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-464">If no single type is encompassed by all other types, then the set has no most encompassed type.</span></span> <span data-ttu-id="56ea5-465">更直观地说，最包含的类型是集中的 "最小" 类型，这种类型可以隐式转换为其他类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-465">In more intuitive terms, the most encompassed type is the "smallest" type in the set—the one type that can be implicitly converted to each of the other types.</span></span>

### <a name="processing-of-user-defined-implicit-conversions"></a><span data-ttu-id="56ea5-466">处理用户定义的隐式转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-466">Processing of user-defined implicit conversions</span></span>

<span data-ttu-id="56ea5-467">用户定义的从类型 `S` 到类型的隐式转换 `T` 的处理方式如下：</span><span class="sxs-lookup"><span data-stu-id="56ea5-467">A user-defined implicit conversion from type `S` to type `T` is processed as follows:</span></span>

*  <span data-ttu-id="56ea5-468">确定 `S0` 和 `T0`的类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-468">Determine the types `S0` and `T0`.</span></span> <span data-ttu-id="56ea5-469">如果 `S` 或 `T` 是可以为 null 的类型，`S0` 和 `T0` 是其基础类型，则 `S0` 和 `T0` 分别等于 `S` 和 `T`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-469">If `S` or `T` are nullable types, `S0` and `T0` are their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span>
*  <span data-ttu-id="56ea5-470">查找要将用户定义的转换运算符视为 `D`类型集。</span><span class="sxs-lookup"><span data-stu-id="56ea5-470">Find the set of types, `D`, from which user-defined conversion operators will be considered.</span></span> <span data-ttu-id="56ea5-471">此集由 `S0` （如果 `S0` 为类或结构）、`S0` 的基本类（如果 `S0` 是类）和 `T0` （如果 `T0` 是类或结构）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-471">This set consists of `S0` (if `S0` is a class or struct), the base classes of `S0` (if `S0` is a class), and `T0` (if `T0` is a class or struct).</span></span>
*  <span data-ttu-id="56ea5-472">查找一组适用的用户定义的转换运算符，`U`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-472">Find the set of applicable user-defined and lifted conversion operators, `U`.</span></span> <span data-ttu-id="56ea5-473">此集由由 `D` 中的类或结构声明的用户定义的和提升的隐式转换运算符组成，它们从包含 `S` 的类型转换为 `T`包含的类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-473">This set consists of the user-defined and lifted implicit conversion operators declared by the classes or structs in `D` that convert from a type encompassing `S` to a type encompassed by `T`.</span></span> <span data-ttu-id="56ea5-474">如果 `U` 为空，则转换未定义，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="56ea5-474">If `U` is empty, the conversion is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="56ea5-475">查找 `U`中运算符的最特定的源类型 `SX`：</span><span class="sxs-lookup"><span data-stu-id="56ea5-475">Find the most specific source type, `SX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="56ea5-476">如果 `U` 中的任何运算符从 `S`转换，则 `SX` 为 `S`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-476">If any of the operators in `U` convert from `S`, then `SX` is `S`.</span></span>
    * <span data-ttu-id="56ea5-477">否则，`SX` 是 `U`中运算符的组合源类型集中最常包含的类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-477">Otherwise, `SX` is the most encompassed type in the combined set of source types of the operators in `U`.</span></span> <span data-ttu-id="56ea5-478">如果找不到完全包含的类型，则转换是不明确的，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="56ea5-478">If exactly one most encompassed type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="56ea5-479">查找 `U`中运算符的最特定目标类型 `TX`：</span><span class="sxs-lookup"><span data-stu-id="56ea5-479">Find the most specific target type, `TX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="56ea5-480">如果 `U` 中的任何运算符转换为 `T`，则 `TX` 为 `T`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-480">If any of the operators in `U` convert to `T`, then `TX` is `T`.</span></span>
    * <span data-ttu-id="56ea5-481">否则，`TX` 是 `U`中运算符的组合目标类型集中最包含的类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-481">Otherwise, `TX` is the most encompassing type in the combined set of target types of the operators in `U`.</span></span> <span data-ttu-id="56ea5-482">如果找不到完全包含的类型，则转换是不明确的，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="56ea5-482">If exactly one most encompassing type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="56ea5-483">查找最具体的转换运算符：</span><span class="sxs-lookup"><span data-stu-id="56ea5-483">Find the most specific conversion operator:</span></span>
    * <span data-ttu-id="56ea5-484">如果 `U` 只包含一个从 `SX` 转换为 `TX`的用户定义的转换运算符，则这是最具体的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="56ea5-484">If `U` contains exactly one user-defined conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="56ea5-485">否则，如果 `U` 只包含一个从 `SX` 转换为 `TX`的提升转换运算符，则这是最具体的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="56ea5-485">Otherwise, if `U` contains exactly one lifted conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="56ea5-486">否则，转换是不明确的，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="56ea5-486">Otherwise, the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="56ea5-487">最后，应用转换：</span><span class="sxs-lookup"><span data-stu-id="56ea5-487">Finally, apply the conversion:</span></span>
    * <span data-ttu-id="56ea5-488">如果未 `SX``S`，则执行从 `S` 到 `SX` 的标准隐式转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-488">If `S` is not `SX`, then a standard implicit conversion from `S` to `SX` is performed.</span></span>
    * <span data-ttu-id="56ea5-489">调用最特定的转换运算符，以将 `SX` 转换为 `TX`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-489">The most specific conversion operator is invoked to convert from `SX` to `TX`.</span></span>
    * <span data-ttu-id="56ea5-490">如果未 `T``TX`，则执行从 `TX` 到 `T` 的标准隐式转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-490">If `TX` is not `T`, then a standard implicit conversion from `TX` to `T` is performed.</span></span>

### <a name="processing-of-user-defined-explicit-conversions"></a><span data-ttu-id="56ea5-491">处理用户定义的显式转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-491">Processing of user-defined explicit conversions</span></span>

<span data-ttu-id="56ea5-492">用户定义的从类型 `S` 到类型 `T` 的显式转换的处理方式如下：</span><span class="sxs-lookup"><span data-stu-id="56ea5-492">A user-defined explicit conversion from type `S` to type `T` is processed as follows:</span></span>

*  <span data-ttu-id="56ea5-493">确定 `S0` 和 `T0`的类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-493">Determine the types `S0` and `T0`.</span></span> <span data-ttu-id="56ea5-494">如果 `S` 或 `T` 是可以为 null 的类型，`S0` 和 `T0` 是其基础类型，则 `S0` 和 `T0` 分别等于 `S` 和 `T`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-494">If `S` or `T` are nullable types, `S0` and `T0` are their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span>
*  <span data-ttu-id="56ea5-495">查找要将用户定义的转换运算符视为 `D`类型集。</span><span class="sxs-lookup"><span data-stu-id="56ea5-495">Find the set of types, `D`, from which user-defined conversion operators will be considered.</span></span> <span data-ttu-id="56ea5-496">此集由 `S0` （如果 `S0` 为类或结构）、`S0` （如果 `S0` 是类）的基类、`T0` （如果 `T0` 是类或结构）以及 `T0` （如果 `T0` 是类）的基类。</span><span class="sxs-lookup"><span data-stu-id="56ea5-496">This set consists of `S0` (if `S0` is a class or struct), the base classes of `S0` (if `S0` is a class), `T0` (if `T0` is a class or struct), and the base classes of `T0` (if `T0` is a class).</span></span>
*  <span data-ttu-id="56ea5-497">查找一组适用的用户定义的转换运算符，`U`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-497">Find the set of applicable user-defined and lifted conversion operators, `U`.</span></span> <span data-ttu-id="56ea5-498">此集由 `D` 中的类或结构声明的用户定义的隐式或显式转换运算符，由 `S` 转换为包含 `T`包含或包含的类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-498">This set consists of the user-defined and lifted implicit or explicit conversion operators declared by the classes or structs in `D` that convert from a type encompassing or encompassed by `S` to a type encompassing or encompassed by `T`.</span></span> <span data-ttu-id="56ea5-499">如果 `U` 为空，则转换未定义，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="56ea5-499">If `U` is empty, the conversion is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="56ea5-500">查找 `U`中运算符的最特定的源类型 `SX`：</span><span class="sxs-lookup"><span data-stu-id="56ea5-500">Find the most specific source type, `SX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="56ea5-501">如果 `U` 中的任何运算符从 `S`转换，则 `SX` 为 `S`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-501">If any of the operators in `U` convert from `S`, then `SX` is `S`.</span></span>
    * <span data-ttu-id="56ea5-502">否则，如果 `U` 中的任何运算符从包含 `S`的类型进行转换，则 `SX` 是这些运算符的一组源类型中包含程度最高的类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-502">Otherwise, if any of the operators in `U` convert from types that encompass `S`, then `SX` is the most encompassed type in the combined set of source types of those operators.</span></span> <span data-ttu-id="56ea5-503">如果找不到最能包含的类型，则转换是不明确的，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="56ea5-503">If no most encompassed type can be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
    * <span data-ttu-id="56ea5-504">否则，`SX` 是 `U`中运算符的组合源类型集中的最包含的类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-504">Otherwise, `SX` is the most encompassing type in the combined set of source types of the operators in `U`.</span></span> <span data-ttu-id="56ea5-505">如果找不到完全包含的类型，则转换是不明确的，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="56ea5-505">If exactly one most encompassing type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="56ea5-506">查找 `U`中运算符的最特定目标类型 `TX`：</span><span class="sxs-lookup"><span data-stu-id="56ea5-506">Find the most specific target type, `TX`, of the operators in `U`:</span></span>
    * <span data-ttu-id="56ea5-507">如果 `U` 中的任何运算符转换为 `T`，则 `TX` 为 `T`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-507">If any of the operators in `U` convert to `T`, then `TX` is `T`.</span></span>
    * <span data-ttu-id="56ea5-508">否则，如果 `U` 中的任何运算符转换为 `T`包含的类型，则在这些运算符的组合目标类型集中，`TX` 是最包含的类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-508">Otherwise, if any of the operators in `U` convert to types that are encompassed by `T`, then `TX` is the most encompassing type in the combined set of target types of those operators.</span></span> <span data-ttu-id="56ea5-509">如果找不到完全包含的类型，则转换是不明确的，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="56ea5-509">If exactly one most encompassing type cannot be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
    * <span data-ttu-id="56ea5-510">否则，`TX` 是 `U`中运算符的组合目标类型集中最常被包含的类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-510">Otherwise, `TX` is the most encompassed type in the combined set of target types of the operators in `U`.</span></span> <span data-ttu-id="56ea5-511">如果找不到最能包含的类型，则转换是不明确的，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="56ea5-511">If no most encompassed type can be found, then the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="56ea5-512">查找最具体的转换运算符：</span><span class="sxs-lookup"><span data-stu-id="56ea5-512">Find the most specific conversion operator:</span></span>
    * <span data-ttu-id="56ea5-513">如果 `U` 只包含一个从 `SX` 转换为 `TX`的用户定义的转换运算符，则这是最具体的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="56ea5-513">If `U` contains exactly one user-defined conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="56ea5-514">否则，如果 `U` 只包含一个从 `SX` 转换为 `TX`的提升转换运算符，则这是最具体的转换运算符。</span><span class="sxs-lookup"><span data-stu-id="56ea5-514">Otherwise, if `U` contains exactly one lifted conversion operator that converts from `SX` to `TX`, then this is the most specific conversion operator.</span></span>
    * <span data-ttu-id="56ea5-515">否则，转换是不明确的，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="56ea5-515">Otherwise, the conversion is ambiguous and a compile-time error occurs.</span></span>
*  <span data-ttu-id="56ea5-516">最后，应用转换：</span><span class="sxs-lookup"><span data-stu-id="56ea5-516">Finally, apply the conversion:</span></span>
    * <span data-ttu-id="56ea5-517">如果未 `SX``S`，则执行从 `S` 到 `SX` 的标准显式转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-517">If `S` is not `SX`, then a standard explicit conversion from `S` to `SX` is performed.</span></span>
    * <span data-ttu-id="56ea5-518">调用最特定的用户定义转换运算符，以将 `SX` 转换为 `TX`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-518">The most specific user-defined conversion operator is invoked to convert from `SX` to `TX`.</span></span>
    * <span data-ttu-id="56ea5-519">如果未 `T``TX`，则执行从 `TX` 到 `T` 的标准显式转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-519">If `TX` is not `T`, then a standard explicit conversion from `TX` to `T` is performed.</span></span>

## <a name="anonymous-function-conversions"></a><span data-ttu-id="56ea5-520">匿名函数转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-520">Anonymous function conversions</span></span>

<span data-ttu-id="56ea5-521">*Anonymous_method_expression*或*lambda_expression*归类为匿名函数（[匿名函数表达式](expressions.md#anonymous-function-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-521">An *anonymous_method_expression* or *lambda_expression* is classified as an anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="56ea5-522">表达式没有类型，但可以隐式转换为兼容的委托类型或表达式树类型。</span><span class="sxs-lookup"><span data-stu-id="56ea5-522">The expression does not have a type but can be implicitly converted to a compatible delegate type or expression tree type.</span></span> <span data-ttu-id="56ea5-523">具体而言，`F` 的匿名函数与 `D` 提供的委托类型兼容：</span><span class="sxs-lookup"><span data-stu-id="56ea5-523">Specifically, an anonymous function `F` is compatible with a delegate type `D` provided:</span></span>

*  <span data-ttu-id="56ea5-524">如果 `F` 包含*anonymous_function_signature*，则 `D` 和 `F` 具有相同数量的参数。</span><span class="sxs-lookup"><span data-stu-id="56ea5-524">If `F` contains an *anonymous_function_signature*, then `D` and `F` have the same number of parameters.</span></span>
*  <span data-ttu-id="56ea5-525">如果 `F` 不包含*anonymous_function_signature*，则 `D` 可能具有零个或多个任意类型的参数，前提是没有 `D` 的参数具有 `out` 参数修饰符。</span><span class="sxs-lookup"><span data-stu-id="56ea5-525">If `F` does not contain an *anonymous_function_signature*, then `D` may have zero or more parameters of any type, as long as no parameter of `D` has the `out` parameter modifier.</span></span>
*  <span data-ttu-id="56ea5-526">如果 `F` 具有显式类型化参数列表，则 `D` 中的每个参数都具有与 `F`中的相应参数相同的类型和修饰符。</span><span class="sxs-lookup"><span data-stu-id="56ea5-526">If `F` has an explicitly typed parameter list, each parameter in `D` has the same type and modifiers as the corresponding parameter in `F`.</span></span>
*  <span data-ttu-id="56ea5-527">如果 `F` 具有隐式类型参数列表，则 `D` 没有 `ref` 或 `out` 参数。</span><span class="sxs-lookup"><span data-stu-id="56ea5-527">If `F` has an implicitly typed parameter list, `D` has no `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="56ea5-528">如果 `F` 的主体是一个表达式，并且 `D` 具有 `void` 返回类型或 `F` 为 async 并且 `D` 具有返回类型 `Task`，则当 `F` 的每个参数都给定 `D`中的相应参数的类型时，`F` 的主体将被视为*statement_expression* （[expression 语句](statements.md#expression-statements) [）。](expressions.md)</span><span class="sxs-lookup"><span data-stu-id="56ea5-528">If the body of `F` is an expression, and either `D` has a `void` return type or `F` is async and `D` has the return type `Task`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid expression (wrt [Expressions](expressions.md)) that would be permitted as a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>
*  <span data-ttu-id="56ea5-529">如果 `F` 的主体是一个语句块，并且 `D` 具有 `void` 返回类型或 `F` 为 async 并且 `D` 具有返回类型 `Task`，则当 `F` 的每个参数都给定 `D`中对应参数的类型时，`F` 的主体为有效语句块（wrt[块](statements.md#blocks)），其中没有 `return` 语句指定表达式。</span><span class="sxs-lookup"><span data-stu-id="56ea5-529">If the body of `F` is a statement block, and either `D` has a `void` return type or `F` is async and `D` has the return type `Task`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid statement block (wrt [Blocks](statements.md#blocks)) in which no `return` statement specifies an expression.</span></span>
*  <span data-ttu-id="56ea5-530">如果 `F` 的主体是一个表达式，*并且 `F` 为非 async 并且 `D`* 具有非 void 返回类型 `T`，*或*`F` 为 async 并且 `D` 具有返回类型 `Task<T>`，则当 `F` 的每个参数都给定 `D`中对应参数的类型时，`F` 的主体是可隐式转换为 `T`的有效表达式（wrt[表达式](expressions.md)）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-530">If the body of `F` is an expression, and *either* `F` is non-async and `D` has a non-void return type `T`, *or* `F` is async and `D` has a return type `Task<T>`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid expression (wrt [Expressions](expressions.md)) that is implicitly convertible to `T`.</span></span>
*  <span data-ttu-id="56ea5-531">如果 `F` 的主体是一个语句块，*并且 `F` 为非 async 并且 `D`* 具有非 void 返回类型 `T`，*或*`F` 为 async 并且 `D` 具有返回类型 `Task<T>`，则当 `F` 的每个参数都给定 `D`中对应参数的类型时，`F` 的主体为有效语句块（wrt[块](statements.md#blocks)），其中每个 `return` 语句指定一个可隐式转换为 `T`的表达式。</span><span class="sxs-lookup"><span data-stu-id="56ea5-531">If the body of `F` is a statement block, and *either* `F` is non-async and `D` has a non-void return type `T`, *or* `F` is async and `D` has a return type `Task<T>`, then when each parameter of `F` is given the type of the corresponding parameter in `D`, the body of `F` is a valid statement block (wrt [Blocks](statements.md#blocks)) with a non-reachable end point in which each `return` statement specifies an expression that is implicitly convertible to `T`.</span></span>

<span data-ttu-id="56ea5-532">为了简洁起见，本部分使用了任务类型 `Task` 和 `Task<T>` （[异步函数](classes.md#async-functions)）的缩写形式。</span><span class="sxs-lookup"><span data-stu-id="56ea5-532">For the purpose of brevity, this section uses the short form for the task types `Task` and `Task<T>` ([Async functions](classes.md#async-functions)).</span></span>

<span data-ttu-id="56ea5-533">如果 `F` 与 `D`的委托类型兼容，则 lambda 表达式 `F` 与表达式树类型兼容 `Expression<D>`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-533">A lambda expression `F` is compatible with an expression tree type `Expression<D>` if `F` is compatible with the delegate type `D`.</span></span> <span data-ttu-id="56ea5-534">请注意，这不适用于匿名方法，只适用于 lambda 表达式。</span><span class="sxs-lookup"><span data-stu-id="56ea5-534">Note that this does not apply to anonymous methods, only lambda expressions.</span></span>

<span data-ttu-id="56ea5-535">某些 lambda 表达式不能转换为表达式树类型：即使转换*存在*，它也会在编译时失败。</span><span class="sxs-lookup"><span data-stu-id="56ea5-535">Certain lambda expressions cannot be converted to expression tree types: Even though the conversion *exists*, it fails at compile-time.</span></span> <span data-ttu-id="56ea5-536">如果 lambda 表达式为，则为这种情况：</span><span class="sxs-lookup"><span data-stu-id="56ea5-536">This is the case if the lambda expression:</span></span>

*  <span data-ttu-id="56ea5-537">具有*块*体</span><span class="sxs-lookup"><span data-stu-id="56ea5-537">Has a *block* body</span></span>
*  <span data-ttu-id="56ea5-538">包含简单赋值运算符或复合赋值运算符</span><span class="sxs-lookup"><span data-stu-id="56ea5-538">Contains simple or compound assignment operators</span></span>
*  <span data-ttu-id="56ea5-539">包含动态绑定的表达式</span><span class="sxs-lookup"><span data-stu-id="56ea5-539">Contains a dynamically bound expression</span></span>
*  <span data-ttu-id="56ea5-540">为异步</span><span class="sxs-lookup"><span data-stu-id="56ea5-540">Is async</span></span>

<span data-ttu-id="56ea5-541">下面的示例使用泛型委托类型 `Func<A,R>` 它表示一个函数，该函数采用 `A` 类型的参数并返回 `R`类型的值：</span><span class="sxs-lookup"><span data-stu-id="56ea5-541">The examples that follow use a generic delegate type `Func<A,R>` which represents a function that takes an argument of type `A` and returns a value of type `R`:</span></span>
```csharp
delegate R Func<A,R>(A arg);
```

<span data-ttu-id="56ea5-542">在分配中</span><span class="sxs-lookup"><span data-stu-id="56ea5-542">In the assignments</span></span>
```csharp
Func<int,int> f1 = x => x + 1;                 // Ok

Func<int,double> f2 = x => x + 1;              // Ok

Func<double,int> f3 = x => x + 1;              // Error

Func<int, Task<int>> f4 = async x => x + 1;    // Ok
```
<span data-ttu-id="56ea5-543">每个匿名函数的参数和返回类型都是从匿名函数所分配到的变量类型确定的。</span><span class="sxs-lookup"><span data-stu-id="56ea5-543">the parameter and return types of each anonymous function are determined from the type of the variable to which the anonymous function is assigned.</span></span>

<span data-ttu-id="56ea5-544">第一个分配成功将匿名函数转换为委托类型 `Func<int,int>` 因为当 `x` 提供类型 `int`时，`x+1` 是可隐式转换为类型 `int`的有效表达式。</span><span class="sxs-lookup"><span data-stu-id="56ea5-544">The first assignment successfully converts the anonymous function to the delegate type `Func<int,int>` because, when `x` is given type `int`, `x+1` is a valid expression that is implicitly convertible to type `int`.</span></span>

<span data-ttu-id="56ea5-545">同样，第二个赋值语句会成功将匿名函数转换为委托类型 `Func<int,double>` 因为 `x+1` （类型 `int`）的结果可隐式转换为类型 `double`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-545">Likewise, the second assignment successfully converts the anonymous function to the delegate type `Func<int,double>` because the result of `x+1` (of type `int`) is implicitly convertible to type `double`.</span></span>

<span data-ttu-id="56ea5-546">但是，第三个赋值是编译时错误，因为当 `double``x` 给定类型时，`x+1` 的结果（类型 `double`）无法隐式转换为类型 `int`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-546">However, the third assignment is a compile-time error because, when `x` is given type `double`, the result of `x+1` (of type `double`) is not implicitly convertible to type `int`.</span></span>

<span data-ttu-id="56ea5-547">第四个赋值成功将匿名 async 函数转换为委托类型 `Func<int, Task<int>>` 因为 `x+1` （类型 `int`）的结果可隐式转换为任务类型 `Task<int>`的结果类型 `int`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-547">The fourth assignment successfully converts the anonymous async function to the delegate type `Func<int, Task<int>>` because the result of `x+1` (of type `int`) is implicitly convertible to the result type `int` of the task type `Task<int>`.</span></span>

<span data-ttu-id="56ea5-548">匿名函数可能会影响重载决策，并参与类型推理。</span><span class="sxs-lookup"><span data-stu-id="56ea5-548">Anonymous functions may influence overload resolution, and participate in type inference.</span></span> <span data-ttu-id="56ea5-549">有关更多详细信息，请参阅[函数成员](expressions.md#function-members)。</span><span class="sxs-lookup"><span data-stu-id="56ea5-549">See [Function members](expressions.md#function-members) for further details.</span></span>

### <a name="evaluation-of-anonymous-function-conversions-to-delegate-types"></a><span data-ttu-id="56ea5-550">匿名函数转换到委托类型的计算</span><span class="sxs-lookup"><span data-stu-id="56ea5-550">Evaluation of anonymous function conversions to delegate types</span></span>

<span data-ttu-id="56ea5-551">将匿名函数转换为委托类型会生成一个委托实例，该实例引用匿名函数和在计算时处于活动状态的已捕获外部变量的（可能为空）集。</span><span class="sxs-lookup"><span data-stu-id="56ea5-551">Conversion of an anonymous function to a delegate type produces a delegate instance which references the anonymous function and the (possibly empty) set of captured outer variables that are active at the time of the evaluation.</span></span> <span data-ttu-id="56ea5-552">调用委托时，将执行匿名函数的主体。</span><span class="sxs-lookup"><span data-stu-id="56ea5-552">When the delegate is invoked, the body of the anonymous function is executed.</span></span> <span data-ttu-id="56ea5-553">使用委托引用的捕获外部变量集来执行正文中的代码。</span><span class="sxs-lookup"><span data-stu-id="56ea5-553">The code in the body is executed using the set of captured outer variables referenced by the delegate.</span></span>

<span data-ttu-id="56ea5-554">从匿名函数生成的委托的调用列表包含单个项。</span><span class="sxs-lookup"><span data-stu-id="56ea5-554">The invocation list of a delegate produced from an anonymous function contains a single entry.</span></span> <span data-ttu-id="56ea5-555">委托的确切目标对象和目标方法未指定。</span><span class="sxs-lookup"><span data-stu-id="56ea5-555">The exact target object and target method of the delegate are unspecified.</span></span> <span data-ttu-id="56ea5-556">具体而言，它不指定是 `null`委托的目标对象、封闭函数成员的 `this` 值还是其他某个对象。</span><span class="sxs-lookup"><span data-stu-id="56ea5-556">In particular, it is unspecified whether the target object of the delegate is `null`, the `this` value of the enclosing function member, or some other object.</span></span>

<span data-ttu-id="56ea5-557">允许（但不要求）将语义完全相同的匿名函数转换为同一委托类型（但不是必需的），以返回相同的委托实例。</span><span class="sxs-lookup"><span data-stu-id="56ea5-557">Conversions of semantically identical anonymous functions with the same (possibly empty) set of captured outer variable instances to the same delegate types are permitted (but not required) to return the same delegate instance.</span></span> <span data-ttu-id="56ea5-558">此处使用的术语在语义上是相同的，这意味着在所有情况下，匿名函数的执行将在给定相同参数的情况下生成相同的效果。</span><span class="sxs-lookup"><span data-stu-id="56ea5-558">The term semantically identical is used here to mean that execution of the anonymous functions will, in all cases, produce the same effects given the same arguments.</span></span> <span data-ttu-id="56ea5-559">此规则允许对如下代码进行优化。</span><span class="sxs-lookup"><span data-stu-id="56ea5-559">This rule permits code such as the following to be optimized.</span></span>

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

<span data-ttu-id="56ea5-560">由于两个匿名函数委托具有相同的（空）捕获的外部变量集，而且由于匿名函数在语义上是相同的，因此编译器允许委托引用相同的目标方法。</span><span class="sxs-lookup"><span data-stu-id="56ea5-560">Since the two anonymous function delegates have the same (empty) set of captured outer variables, and since the anonymous functions are semantically identical, the compiler is permitted to have the delegates refer to the same target method.</span></span> <span data-ttu-id="56ea5-561">的确，允许编译器从两个匿名函数表达式返回完全相同的委托实例。</span><span class="sxs-lookup"><span data-stu-id="56ea5-561">Indeed, the compiler is permitted to return the very same delegate instance from both anonymous function expressions.</span></span>

### <a name="evaluation-of-anonymous-function-conversions-to-expression-tree-types"></a><span data-ttu-id="56ea5-562">对表达式树类型的匿名函数转换的计算</span><span class="sxs-lookup"><span data-stu-id="56ea5-562">Evaluation of anonymous function conversions to expression tree types</span></span>

<span data-ttu-id="56ea5-563">将匿名函数转换为表达式树类型会生成一个表达式树（[表达式树类型](types.md#expression-tree-types)）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-563">Conversion of an anonymous function to an expression tree type produces an expression tree ([Expression tree types](types.md#expression-tree-types)).</span></span> <span data-ttu-id="56ea5-564">更准确地说，对匿名函数转换的评估导致了表示匿名函数本身的结构的对象结构。</span><span class="sxs-lookup"><span data-stu-id="56ea5-564">More precisely, evaluation of the anonymous function conversion leads to the construction of an object structure that represents the structure of the anonymous function itself.</span></span> <span data-ttu-id="56ea5-565">表达式树的准确结构以及用于创建它的确切过程是定义的实现。</span><span class="sxs-lookup"><span data-stu-id="56ea5-565">The precise structure of the expression tree, as well as the exact process for creating it, are implementation defined.</span></span>

### <a name="implementation-example"></a><span data-ttu-id="56ea5-566">实现示例</span><span class="sxs-lookup"><span data-stu-id="56ea5-566">Implementation example</span></span>

<span data-ttu-id="56ea5-567">本部分介绍了可能实现的匿名函数转换（其他C#构造）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-567">This section describes a possible implementation of anonymous function conversions in terms of other C# constructs.</span></span> <span data-ttu-id="56ea5-568">此处所述的实现基于 Microsoft C#编译器所使用的相同原则，但它并不是强制性实现，也不是唯一可行的方法。</span><span class="sxs-lookup"><span data-stu-id="56ea5-568">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation, nor is it the only one possible.</span></span> <span data-ttu-id="56ea5-569">它只是简单地提到了转换为表达式树，因为其确切语义超出了本规范的范围。</span><span class="sxs-lookup"><span data-stu-id="56ea5-569">It only briefly mentions conversions to expression trees, as their exact semantics are outside the scope of this specification.</span></span>

<span data-ttu-id="56ea5-570">本部分的其余部分提供了一些代码示例，其中包含具有不同特征的匿名函数。</span><span class="sxs-lookup"><span data-stu-id="56ea5-570">The remainder of this section gives several examples of code that contains anonymous functions with different characteristics.</span></span> <span data-ttu-id="56ea5-571">对于每个示例，提供了仅使用其他C#构造的代码的相应转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-571">For each example, a corresponding translation to code that uses only other C# constructs is provided.</span></span> <span data-ttu-id="56ea5-572">在示例中，假设标识符 `D` 表示以下委托类型：</span><span class="sxs-lookup"><span data-stu-id="56ea5-572">In the examples, the identifier `D` is assumed by represent the following delegate type:</span></span>
```csharp
public delegate void D();
```

<span data-ttu-id="56ea5-573">匿名函数的最简单形式是捕获无外部变量：</span><span class="sxs-lookup"><span data-stu-id="56ea5-573">The simplest form of an anonymous function is one that captures no outer variables:</span></span>
```csharp
class Test
{
    static void F() {
        D d = () => { Console.WriteLine("test"); };
    }
}
```

<span data-ttu-id="56ea5-574">这可以转换为引用编译器生成的静态方法（在其中放置匿名函数的代码）的委托实例化：</span><span class="sxs-lookup"><span data-stu-id="56ea5-574">This can be translated to a delegate instantiation that references a compiler generated static method in which the code of the anonymous function is placed:</span></span>
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

<span data-ttu-id="56ea5-575">在下面的示例中，匿名函数引用 `this`的实例成员：</span><span class="sxs-lookup"><span data-stu-id="56ea5-575">In the following example, the anonymous function references instance members of `this`:</span></span>
```csharp
class Test
{
    int x;

    void F() {
        D d = () => { Console.WriteLine(x); };
    }
}
```

<span data-ttu-id="56ea5-576">这可以转换为包含匿名函数代码的编译器生成的实例方法：</span><span class="sxs-lookup"><span data-stu-id="56ea5-576">This can be translated to a compiler generated instance method containing the code of the anonymous function:</span></span>
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

<span data-ttu-id="56ea5-577">在此示例中，匿名函数捕获本地变量：</span><span class="sxs-lookup"><span data-stu-id="56ea5-577">In this example, the anonymous function captures a local variable:</span></span>
```csharp
class Test
{
    void F() {
        int y = 123;
        D d = () => { Console.WriteLine(y); };
    }
}
```

<span data-ttu-id="56ea5-578">局部变量的生存期现在必须至少扩展到匿名函数委托的生存期。</span><span class="sxs-lookup"><span data-stu-id="56ea5-578">The lifetime of the local variable must now be extended to at least the lifetime of the anonymous function delegate.</span></span> <span data-ttu-id="56ea5-579">这可以通过将本地变量 "提升" 到编译器生成的类的字段来实现。</span><span class="sxs-lookup"><span data-stu-id="56ea5-579">This can be achieved by "hoisting" the local variable into a field of a compiler generated class.</span></span> <span data-ttu-id="56ea5-580">局部变量的实例化（[局部变量的实例化](expressions.md#instantiation-of-local-variables)）随后对应于创建编译器生成的类的实例，并且访问本地变量对应于访问编译器生成的类的实例中的字段。</span><span class="sxs-lookup"><span data-stu-id="56ea5-580">Instantiation of the local variable ([Instantiation of local variables](expressions.md#instantiation-of-local-variables)) then corresponds to creating an instance of the compiler generated class, and accessing the local variable corresponds to accessing a field in the instance of the compiler generated class.</span></span> <span data-ttu-id="56ea5-581">此外，匿名函数会成为编译器生成的类的实例方法：</span><span class="sxs-lookup"><span data-stu-id="56ea5-581">Furthermore, the anonymous function becomes an instance method of the compiler generated class:</span></span>
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

<span data-ttu-id="56ea5-582">最后，下面的匿名函数捕获 `this`，以及两个具有不同生存期的局部变量：</span><span class="sxs-lookup"><span data-stu-id="56ea5-582">Finally, the following anonymous function captures `this` as well as two local variables with different lifetimes:</span></span>
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

<span data-ttu-id="56ea5-583">在这里，将为每个在其中捕获局部变量的语句块创建一个编译器生成的类，以便不同块中的局部变量可以具有独立生存期。</span><span class="sxs-lookup"><span data-stu-id="56ea5-583">Here, a compiler generated class is created for each statement block in which locals are captured such that the locals in the different blocks can have independent lifetimes.</span></span> <span data-ttu-id="56ea5-584">`__Locals2`的实例（内部语句块的编译器生成的类）包含局部变量 `z` 和引用 `__Locals1`的实例的字段。</span><span class="sxs-lookup"><span data-stu-id="56ea5-584">An instance of `__Locals2`, the compiler generated class for the inner statement block, contains the local variable `z` and a field that references an instance of `__Locals1`.</span></span>  <span data-ttu-id="56ea5-585">`__Locals1`的实例（外部语句块的编译器生成的类）包含局部变量 `y` 和引用封闭函数成员 `this` 的字段。</span><span class="sxs-lookup"><span data-stu-id="56ea5-585">An instance of `__Locals1`, the compiler generated class for the outer statement block, contains the local variable `y` and a field that references `this` of the enclosing function member.</span></span> <span data-ttu-id="56ea5-586">利用这些数据结构，可以通过 `__Local2`的实例访问所有捕获的外部变量，并且可以将匿名函数的代码实现为该类的实例方法。</span><span class="sxs-lookup"><span data-stu-id="56ea5-586">With these data structures it is possible to reach all captured outer variables through an instance of `__Local2`, and the code of the anonymous function can thus be implemented as an instance method of that class.</span></span>

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

<span data-ttu-id="56ea5-587">在此处用于捕获局部变量的相同技术也可以在将匿名函数转换为表达式树时使用：对编译器生成的对象的引用可以存储在表达式树中，对局部变量的访问可以是表示为这些对象上的字段访问。</span><span class="sxs-lookup"><span data-stu-id="56ea5-587">The same technique applied here to capture local variables can also be used when converting anonymous functions to expression trees: References to the compiler generated objects can be stored in the expression tree, and access to the local variables can be represented as field accesses on these objects.</span></span> <span data-ttu-id="56ea5-588">此方法的优点是允许在委托和表达式树之间共享 "提升" 的局部变量。</span><span class="sxs-lookup"><span data-stu-id="56ea5-588">The advantage of this approach is that it allows the "lifted" local variables to be shared between delegates and expression trees.</span></span>

## <a name="method-group-conversions"></a><span data-ttu-id="56ea5-589">方法组转换</span><span class="sxs-lookup"><span data-stu-id="56ea5-589">Method group conversions</span></span>

<span data-ttu-id="56ea5-590">存在从方法组（[表达式分类](expressions.md#expression-classifications)）到兼容委托类型的隐式转换（[隐式](conversions.md#implicit-conversions)转换）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-590">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from a method group ([Expression classifications](expressions.md#expression-classifications)) to a compatible delegate type.</span></span> <span data-ttu-id="56ea5-591">给定委托类型 `D` 和归类为方法组的表达式 `E` 时 `D` `E`，如果 `E` 至少包含一个适用于其正常窗体（[适用函数成员](expressions.md#applicable-function-member)）的方法，该方法可用于通过使用 `D`的参数类型和修饰符构造的参数列表，如下所述。</span><span class="sxs-lookup"><span data-stu-id="56ea5-591">Given a delegate type `D` and an expression `E` that is classified as a method group, an implicit conversion exists from `E` to `D` if `E` contains at least one method that is applicable in its normal form ([Applicable function member](expressions.md#applicable-function-member)) to an argument list constructed by use of the parameter types and modifiers of `D`, as described in the following.</span></span>

<span data-ttu-id="56ea5-592">以下中描述了从方法组 `E` 到委托类型的转换的编译时应用程序 `D`。</span><span class="sxs-lookup"><span data-stu-id="56ea5-592">The compile-time application of a conversion from a method group `E` to a delegate type `D` is described in the following.</span></span> <span data-ttu-id="56ea5-593">请注意，是否存在从 `E` 到 `D` 的隐式转换，并不保证转换的编译时应用程序将成功且不出错。</span><span class="sxs-lookup"><span data-stu-id="56ea5-593">Note that the existence of an implicit conversion from `E` to `D` does not guarantee that the compile-time application of the conversion will succeed without error.</span></span>

*  <span data-ttu-id="56ea5-594">选择与窗体 `E(A)`的方法调用（[方法](expressions.md#method-invocations)调用）相对应 `M` 单个方法，并进行以下修改：</span><span class="sxs-lookup"><span data-stu-id="56ea5-594">A single method `M` is selected corresponding to a method invocation ([Method invocations](expressions.md#method-invocations)) of the form `E(A)`, with the following modifications:</span></span>
    * <span data-ttu-id="56ea5-595">参数列表 `A` 是一个表达式列表，每个表达式都分类为一个变量，并且具有 `D`的*formal_parameter_list*中相应参数的类型和修饰符（`ref` 或 `out`）。</span><span class="sxs-lookup"><span data-stu-id="56ea5-595">The argument list `A` is a list of expressions, each classified as a variable and with the type and modifier (`ref` or `out`) of the corresponding parameter in the *formal_parameter_list* of `D`.</span></span>
    * <span data-ttu-id="56ea5-596">被视为的候选方法只是那些适用于其正常窗体（[适用函数成员](expressions.md#applicable-function-member)）的方法，而不是仅适用于其扩展窗体的方法。</span><span class="sxs-lookup"><span data-stu-id="56ea5-596">The candidate methods considered are only those methods that are applicable in their normal form ([Applicable function member](expressions.md#applicable-function-member)), not those applicable only in their expanded form.</span></span>
*  <span data-ttu-id="56ea5-597">如果[方法调用](expressions.md#method-invocations)的算法产生错误，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="56ea5-597">If the algorithm of [Method invocations](expressions.md#method-invocations) produces an error, then a compile-time error occurs.</span></span> <span data-ttu-id="56ea5-598">否则，算法将生成单个最佳方法，`M` 与 `D` 具有相同数量的参数，并将转换视为存在。</span><span class="sxs-lookup"><span data-stu-id="56ea5-598">Otherwise the algorithm produces a single best method `M` having the same number of parameters as `D` and the conversion is considered to exist.</span></span>
*  <span data-ttu-id="56ea5-599">所选方法 `M` 必须兼容（[委托兼容性](delegates.md#delegate-compatibility)） `D`委托类型，否则将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="56ea5-599">The selected method `M` must be compatible ([Delegate compatibility](delegates.md#delegate-compatibility)) with the delegate type `D`, or otherwise, a compile-time error occurs.</span></span>
*  <span data-ttu-id="56ea5-600">如果所选方法 `M` 是实例方法，则与 `E` 关联的实例表达式确定委托的目标对象。</span><span class="sxs-lookup"><span data-stu-id="56ea5-600">If the selected method `M` is an instance method, the instance expression associated with `E` determines the target object of the delegate.</span></span>
*  <span data-ttu-id="56ea5-601">如果所选方法 M 是通过实例表达式上的成员访问方式表示的扩展方法，则该实例表达式将确定委托的目标对象。</span><span class="sxs-lookup"><span data-stu-id="56ea5-601">If the selected method M is an extension method which is denoted by means of a member access on an instance expression, that instance expression determines the target object of the delegate.</span></span>
*  <span data-ttu-id="56ea5-602">转换结果为类型 `D`的值，即引用所选方法和目标对象的新创建的委托。</span><span class="sxs-lookup"><span data-stu-id="56ea5-602">The result of the conversion is a value of type `D`, namely a newly created delegate that refers to the selected method and target object.</span></span>
*  <span data-ttu-id="56ea5-603">请注意，如果[方法调用](expressions.md#method-invocations)的算法未能找到实例方法，但在处理 `E(A)` 的调用作为扩展方法调用（[扩展方法](expressions.md#extension-method-invocations)调用）时，此过程可能会导致创建扩展方法的委托。</span><span class="sxs-lookup"><span data-stu-id="56ea5-603">Note that this process can lead to the creation of a delegate to an extension method, if the algorithm of [Method invocations](expressions.md#method-invocations) fails to find an instance method but succeeds in processing the invocation of `E(A)` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="56ea5-604">因此，创建的委托将捕获扩展方法以及其第一个参数。</span><span class="sxs-lookup"><span data-stu-id="56ea5-604">A delegate thus created captures the extension method as well as its first argument.</span></span>

<span data-ttu-id="56ea5-605">下面的示例演示方法组转换：</span><span class="sxs-lookup"><span data-stu-id="56ea5-605">The following example demonstrates method group conversions:</span></span>
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

<span data-ttu-id="56ea5-606">赋值 `d1` 会将方法组 `F` 隐式转换为类型 `D1`的值。</span><span class="sxs-lookup"><span data-stu-id="56ea5-606">The assignment to `d1` implicitly converts the method group `F` to a value of type `D1`.</span></span>

<span data-ttu-id="56ea5-607">对 `d2` 的赋值演示了如何创建一个委托，该委托指向的派生程度较低（逆变）的参数类型和派生程度更高（协变）返回类型的方法。</span><span class="sxs-lookup"><span data-stu-id="56ea5-607">The assignment to `d2` shows how it is possible to create a delegate to a method that has less derived (contravariant) parameter types and a more derived (covariant) return type.</span></span>

<span data-ttu-id="56ea5-608">如果该方法不适用，则分配给 `d3` 说明不存在转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-608">The assignment to `d3` shows how no conversion exists if the method is not applicable.</span></span>

<span data-ttu-id="56ea5-609">对 `d4` 的赋值显示了方法必须以其正常形式适用的方式。</span><span class="sxs-lookup"><span data-stu-id="56ea5-609">The assignment to `d4` shows how the method must be applicable in its normal form.</span></span>

<span data-ttu-id="56ea5-610">对 `d5` 的赋值显示了如何允许委托和方法的参数和返回类型仅与引用类型不同。</span><span class="sxs-lookup"><span data-stu-id="56ea5-610">The assignment to `d5` shows how parameter and return types of the delegate and method are allowed to differ only for reference types.</span></span>

<span data-ttu-id="56ea5-611">与所有其他隐式和显式转换一样，转换运算符可用于显式执行方法组转换。</span><span class="sxs-lookup"><span data-stu-id="56ea5-611">As with all other implicit and explicit conversions, the cast operator can be used to explicitly perform a method group conversion.</span></span> <span data-ttu-id="56ea5-612">因此，示例</span><span class="sxs-lookup"><span data-stu-id="56ea5-612">Thus, the example</span></span>
```csharp
object obj = new EventHandler(myDialog.OkClick);
```
<span data-ttu-id="56ea5-613">可以改为写入</span><span class="sxs-lookup"><span data-stu-id="56ea5-613">could instead be written</span></span>
```csharp
object obj = (EventHandler)myDialog.OkClick;
```

<span data-ttu-id="56ea5-614">方法组可能会影响重载决策，并参与类型推理。</span><span class="sxs-lookup"><span data-stu-id="56ea5-614">Method groups may influence overload resolution, and participate in type inference.</span></span> <span data-ttu-id="56ea5-615">有关更多详细信息，请参阅[函数成员](expressions.md#function-members)。</span><span class="sxs-lookup"><span data-stu-id="56ea5-615">See [Function members](expressions.md#function-members) for further details.</span></span>

<span data-ttu-id="56ea5-616">方法组转换的运行时计算如下所示：</span><span class="sxs-lookup"><span data-stu-id="56ea5-616">The run-time evaluation of a method group conversion proceeds as follows:</span></span>

*  <span data-ttu-id="56ea5-617">如果在编译时选择的方法是实例方法，或者它是作为实例方法访问的扩展方法，则将从与 `E`关联的实例表达式确定委托的目标对象：</span><span class="sxs-lookup"><span data-stu-id="56ea5-617">If the method selected at compile-time is an instance method, or it is an extension method which is accessed as an instance method, the target object of the delegate is determined from the instance expression associated with `E`:</span></span>
    * <span data-ttu-id="56ea5-618">计算实例表达式。</span><span class="sxs-lookup"><span data-stu-id="56ea5-618">The instance expression is evaluated.</span></span> <span data-ttu-id="56ea5-619">如果此计算导致异常，则不执行其他步骤。</span><span class="sxs-lookup"><span data-stu-id="56ea5-619">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="56ea5-620">如果实例表达式为*reference_type*，则由实例表达式计算的值将成为目标对象。</span><span class="sxs-lookup"><span data-stu-id="56ea5-620">If the instance expression is of a *reference_type*, the value computed by the instance expression becomes the target object.</span></span> <span data-ttu-id="56ea5-621">如果所选的方法是实例方法，并且目标对象是 `null`的，则会引发 `System.NullReferenceException`，而不执行进一步的步骤。</span><span class="sxs-lookup"><span data-stu-id="56ea5-621">If the selected method is an instance method and the target object is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="56ea5-622">如果实例表达式为*value_type*，则将执行装箱操作（[装箱转换](types.md#boxing-conversions)）以将值转换为对象，此对象将成为目标对象。</span><span class="sxs-lookup"><span data-stu-id="56ea5-622">If the instance expression is of a *value_type*, a boxing operation ([Boxing conversions](types.md#boxing-conversions)) is performed to convert the value to an object, and this object becomes the target object.</span></span>
*  <span data-ttu-id="56ea5-623">否则，所选方法为静态方法调用的一部分，并且 `null`委托的目标对象。</span><span class="sxs-lookup"><span data-stu-id="56ea5-623">Otherwise the selected method is part of a static method call, and the target object of the delegate is `null`.</span></span>
*  <span data-ttu-id="56ea5-624">分配 `D` 委托类型的新实例。</span><span class="sxs-lookup"><span data-stu-id="56ea5-624">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="56ea5-625">如果内存不足，无法分配新的实例，则会引发 `System.OutOfMemoryException`，而不执行进一步的步骤。</span><span class="sxs-lookup"><span data-stu-id="56ea5-625">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="56ea5-626">使用对在编译时确定的方法以及对上面计算的目标对象的引用来初始化新的委托实例。</span><span class="sxs-lookup"><span data-stu-id="56ea5-626">The new delegate instance is initialized with a reference to the method that was determined at compile-time and a reference to the target object computed above.</span></span>
