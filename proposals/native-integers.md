---
ms.openlocfilehash: 42839a8c233468dd0b5ec6dad436dc71f056a6d9
ms.sourcegitcommit: d414836632ba2730545e0b058373a94696bba5e4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2020
ms.locfileid: "81447802"
---
# <a name="native-sized-integers"></a><span data-ttu-id="8ea40-101">本机大小整数</span><span class="sxs-lookup"><span data-stu-id="8ea40-101">Native-sized integers</span></span>

## <a name="summary"></a><span data-ttu-id="8ea40-102">总结</span><span class="sxs-lookup"><span data-stu-id="8ea40-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="8ea40-103">对本机大小的签名和无符号整数类型的语言支持。</span><span class="sxs-lookup"><span data-stu-id="8ea40-103">Language support for a native-sized signed and unsigned integer types.</span></span>

<span data-ttu-id="8ea40-104">动机是互操作场景和低级别库。</span><span class="sxs-lookup"><span data-stu-id="8ea40-104">The motivation is for interop scenarios and for low-level libraries.</span></span>

## <a name="design"></a><span data-ttu-id="8ea40-105">设计</span><span class="sxs-lookup"><span data-stu-id="8ea40-105">Design</span></span>
[design]: #design

<span data-ttu-id="8ea40-106">标识符`nint`和`nuint`是表示本机签名和无符号整数类型的新的上下文关键字。</span><span class="sxs-lookup"><span data-stu-id="8ea40-106">The identifiers `nint` and `nuint` are new contextual keywords that represent native signed and unsigned integer types.</span></span>
<span data-ttu-id="8ea40-107">仅当名称查找在该程序位置找不到可行结果时，标识符才会被视为关键字。</span><span class="sxs-lookup"><span data-stu-id="8ea40-107">The identifiers are only treated as keywords when name lookup does not find a viable result at that program location.</span></span>
```C#
nint x = 3;
string y = nameof(nuint);
_ = nint.Equals(x, 3);
```

<span data-ttu-id="8ea40-108">类型`nint`和`nuint`由基础类型`System.IntPtr`表示，编译器`System.UIntPtr`将这些类型的其他转换和操作作为本机 ints 表示。</span><span class="sxs-lookup"><span data-stu-id="8ea40-108">The types `nint` and `nuint` are represented by the underlying types `System.IntPtr` and `System.UIntPtr` with compiler surfacing additional conversions and operations for those types as native ints.</span></span>

### <a name="constants"></a><span data-ttu-id="8ea40-109">常量</span><span class="sxs-lookup"><span data-stu-id="8ea40-109">Constants</span></span>

<span data-ttu-id="8ea40-110">常量表达式可以是 类型`nint`或`nuint`。</span><span class="sxs-lookup"><span data-stu-id="8ea40-110">Constant expressions may be of type `nint` or `nuint`.</span></span>
<span data-ttu-id="8ea40-111">本机 int 文本没有直接语法。</span><span class="sxs-lookup"><span data-stu-id="8ea40-111">There is no direct syntax for native int literals.</span></span> <span data-ttu-id="8ea40-112">可以改用其他积分常量值的隐式或显式强制转换`const nint i = (nint)42;`：</span><span class="sxs-lookup"><span data-stu-id="8ea40-112">Implicit or explicit casts of other integral constant values can be used instead: `const nint i = (nint)42;`.</span></span>

<span data-ttu-id="8ea40-113">`nint`常量在 [ ， `int.MinValue` `int.MaxValue` ] 范围内。</span><span class="sxs-lookup"><span data-stu-id="8ea40-113">`nint` constants are in the range [ `int.MinValue`, `int.MaxValue` ].</span></span>

<span data-ttu-id="8ea40-114">`nuint`常量在 [ ， `uint.MinValue` `uint.MaxValue` ] 范围内。</span><span class="sxs-lookup"><span data-stu-id="8ea40-114">`nuint` constants are in the range [ `uint.MinValue`, `uint.MaxValue` ].</span></span>

<span data-ttu-id="8ea40-115">`MinValue`没有`MaxValue`或字段`nint`，或`nuint`因为，除了`nuint.MinValue`，这些值不能作为常量发射。</span><span class="sxs-lookup"><span data-stu-id="8ea40-115">There are no `MinValue` or `MaxValue` fields on `nint` or `nuint` because, other than `nuint.MinValue`, those values cannot be emitted as constants.</span></span>

<span data-ttu-id="8ea40-116">所有未元`+`运算符 \* 、 `-`、 `~` \* 和二进制运算符`+` `-`\* `*` `/`、 `%` `==`、 `!=` `<`、 `<=` `>`、 `>=` `&`、 `|` `^` `<<` `>>` 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、</span><span class="sxs-lookup"><span data-stu-id="8ea40-116">Constant folding is supported for all unary operators { `+`, `-`, `~` } and binary operators { `+`, `-`, `*`, `/`, `%`, `==`, `!=`, `<`, `<=`, `>`, `>=`, `&`, `|`, `^`, `<<`, `>>` }.</span></span>
<span data-ttu-id="8ea40-117">无论编译器平台如何，都`Int32`使用`UInt32`和 操作数而不是本机 int 计算恒定的折叠操作，以便进行一致的行为。</span><span class="sxs-lookup"><span data-stu-id="8ea40-117">Constant folding operations are evaluated with `Int32` and `UInt32` operands rather than native ints for consistent behavior regardless of compiler platform.</span></span>
<span data-ttu-id="8ea40-118">如果操作在 32 位中导致恒定值，则在编译时执行恒定折叠。</span><span class="sxs-lookup"><span data-stu-id="8ea40-118">If the operation results in a constant value in 32-bits, constant folding is performed at compile-time.</span></span>
<span data-ttu-id="8ea40-119">否则，操作在运行时执行，不被视为常量。</span><span class="sxs-lookup"><span data-stu-id="8ea40-119">Otherwise the operation is executed at runtime and not considered a constant.</span></span>

### <a name="conversions"></a><span data-ttu-id="8ea40-120">转换</span><span class="sxs-lookup"><span data-stu-id="8ea40-120">Conversions</span></span>
<span data-ttu-id="8ea40-121">和`nint``IntPtr`之间存在标识转换，和 和`nuint``UIntPtr`之间有标识转换。</span><span class="sxs-lookup"><span data-stu-id="8ea40-121">There is an identity conversion between `nint` and `IntPtr`, and between `nuint` and `UIntPtr`.</span></span>
<span data-ttu-id="8ea40-122">复合类型之间存在标识转换，这些类型仅因本机 ints 和基础类型而异：数组、`Nullable<>`构造类型和元组。</span><span class="sxs-lookup"><span data-stu-id="8ea40-122">There is an identity conversion between compound types that differ by native ints and underlying types only: arrays, `Nullable<>`, constructed types, and tuples.</span></span>

<span data-ttu-id="8ea40-123">下表涵盖了特殊类型之间的转换。</span><span class="sxs-lookup"><span data-stu-id="8ea40-123">The tables below cover the conversions between special types.</span></span>
<span data-ttu-id="8ea40-124">（每个转换的 IL 包括 不同的`unchecked`变体和`checked`上下文（如果不同）。</span><span class="sxs-lookup"><span data-stu-id="8ea40-124">(The IL for each conversion includes the variants for `unchecked` and `checked` contexts if different.)</span></span>

| <span data-ttu-id="8ea40-125">操作数</span><span class="sxs-lookup"><span data-stu-id="8ea40-125">Operand</span></span> | <span data-ttu-id="8ea40-126">目标</span><span class="sxs-lookup"><span data-stu-id="8ea40-126">Target</span></span> | <span data-ttu-id="8ea40-127">转换</span><span class="sxs-lookup"><span data-stu-id="8ea40-127">Conversion</span></span> | <span data-ttu-id="8ea40-128">IL</span><span class="sxs-lookup"><span data-stu-id="8ea40-128">IL</span></span> |
|:---:|:---:|:---:|:---:|
| `object` | `nint` | <span data-ttu-id="8ea40-129">取消装箱</span><span class="sxs-lookup"><span data-stu-id="8ea40-129">Unboxing</span></span> | `unbox` |
| `void*` | `nint` | <span data-ttu-id="8ea40-130">指针到虚空</span><span class="sxs-lookup"><span data-stu-id="8ea40-130">PointerToVoid</span></span> | `conv.i` |
| `sbyte` | `nint` | <span data-ttu-id="8ea40-131">隐式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-131">ImplicitNumeric</span></span> | `conv.i` |
| `byte` | `nint` | <span data-ttu-id="8ea40-132">隐式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-132">ImplicitNumeric</span></span> | `conv.u` |
| `short` | `nint` | <span data-ttu-id="8ea40-133">隐式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-133">ImplicitNumeric</span></span> | `conv.i` |
| `ushort` | `nint` | <span data-ttu-id="8ea40-134">隐式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-134">ImplicitNumeric</span></span> | `conv.u` |
| `int` | `nint` | <span data-ttu-id="8ea40-135">隐式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-135">ImplicitNumeric</span></span> | `conv.i` |
| `uint` | `nint` | <span data-ttu-id="8ea40-136">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-136">ExplicitNumeric</span></span> | `conv.u` / `conv.ovf.u` |
| `long` | `nint` | <span data-ttu-id="8ea40-137">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-137">ExplicitNumeric</span></span> | `conv.i` / `conv.ovf.i` |
| `ulong` | `nint` | <span data-ttu-id="8ea40-138">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-138">ExplicitNumeric</span></span> | `conv.i` / `conv.ovf.i` |
| `char` | `nint` | <span data-ttu-id="8ea40-139">隐式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-139">ImplicitNumeric</span></span> | `conv.i` |
| `float` | `nint` | <span data-ttu-id="8ea40-140">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-140">ExplicitNumeric</span></span> | `conv.i` / `conv.ovf.i` |
| `double` | `nint` | <span data-ttu-id="8ea40-141">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-141">ExplicitNumeric</span></span> | `conv.i` / `conv.ovf.i` |
| `decimal` | `nint` | <span data-ttu-id="8ea40-142">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-142">ExplicitNumeric</span></span> | `long decimal.op_Explicit(decimal) conv.i` / `... conv.ovf.i` |
| `IntPtr` | `nint` | <span data-ttu-id="8ea40-143">标识</span><span class="sxs-lookup"><span data-stu-id="8ea40-143">Identity</span></span> | |
| `UIntPtr` | `nint` | <span data-ttu-id="8ea40-144">无</span><span class="sxs-lookup"><span data-stu-id="8ea40-144">None</span></span> | |
| `object` | `nuint` | <span data-ttu-id="8ea40-145">取消装箱</span><span class="sxs-lookup"><span data-stu-id="8ea40-145">Unboxing</span></span> | `unbox` |
| `void*` | `nuint` | <span data-ttu-id="8ea40-146">指针到虚空</span><span class="sxs-lookup"><span data-stu-id="8ea40-146">PointerToVoid</span></span> | `conv.u` |
| `sbyte` | `nuint` | <span data-ttu-id="8ea40-147">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-147">ExplicitNumeric</span></span> | `conv.u` / `conv.ovf.u` |
| `byte` | `nuint` | <span data-ttu-id="8ea40-148">隐式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-148">ImplicitNumeric</span></span> | `conv.u` |
| `short` | `nuint` | <span data-ttu-id="8ea40-149">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-149">ExplicitNumeric</span></span> | `conv.u` / `conv.ovf.u` |
| `ushort` | `nuint` | <span data-ttu-id="8ea40-150">隐式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-150">ImplicitNumeric</span></span> | `conv.u` |
| `int` | `nuint` | <span data-ttu-id="8ea40-151">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-151">ExplicitNumeric</span></span> | `conv.u` / `conv.ovf.u` |
| `uint` | `nuint` | <span data-ttu-id="8ea40-152">隐式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-152">ImplicitNumeric</span></span> | `conv.u` |
| `long` | `nuint` | <span data-ttu-id="8ea40-153">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-153">ExplicitNumeric</span></span> | `conv.u` / `conv.ovf.u` |
| `ulong` | `nuint` | <span data-ttu-id="8ea40-154">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-154">ExplicitNumeric</span></span> | `conv.u` / `conv.ovf.u` |
| `char` | `nuint` | <span data-ttu-id="8ea40-155">隐式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-155">ImplicitNumeric</span></span> | `conv.u` |
| `float` | `nuint` | <span data-ttu-id="8ea40-156">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-156">ExplicitNumeric</span></span> | `conv.u` / `conv.ovf.u` |
| `double` | `nuint` | <span data-ttu-id="8ea40-157">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-157">ExplicitNumeric</span></span> | `conv.u` / `conv.ovf.u` |
| `decimal` | `nuint` | <span data-ttu-id="8ea40-158">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-158">ExplicitNumeric</span></span> | `ulong decimal.op_Explicit(decimal) conv.u` / `... conv.ovf.u.un`  |
| `IntPtr` | `nuint` | <span data-ttu-id="8ea40-159">无</span><span class="sxs-lookup"><span data-stu-id="8ea40-159">None</span></span> | |
| `UIntPtr` | `nuint` | <span data-ttu-id="8ea40-160">标识</span><span class="sxs-lookup"><span data-stu-id="8ea40-160">Identity</span></span> | |

| <span data-ttu-id="8ea40-161">操作数</span><span class="sxs-lookup"><span data-stu-id="8ea40-161">Operand</span></span> | <span data-ttu-id="8ea40-162">目标</span><span class="sxs-lookup"><span data-stu-id="8ea40-162">Target</span></span> | <span data-ttu-id="8ea40-163">转换</span><span class="sxs-lookup"><span data-stu-id="8ea40-163">Conversion</span></span> | <span data-ttu-id="8ea40-164">IL</span><span class="sxs-lookup"><span data-stu-id="8ea40-164">IL</span></span> |
|:---:|:---:|:---:|:---:|
| `nint` | `object` | <span data-ttu-id="8ea40-165">装箱</span><span class="sxs-lookup"><span data-stu-id="8ea40-165">Boxing</span></span> | `box` |
| `nint` | `void*` | <span data-ttu-id="8ea40-166">指针到虚空</span><span class="sxs-lookup"><span data-stu-id="8ea40-166">PointerToVoid</span></span> | `conv.i` |
| `nint` | `nuint` | <span data-ttu-id="8ea40-167">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-167">ExplicitNumeric</span></span> | `conv.u` / `conv.ovf.u` |
| `nint` | `sbyte` | <span data-ttu-id="8ea40-168">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-168">ExplicitNumeric</span></span> | `conv.i1` / `conv.ovf.i1` |
| `nint` | `byte` | <span data-ttu-id="8ea40-169">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-169">ExplicitNumeric</span></span> | `conv.u1` / `conv.ovf.u1` |
| `nint` | `short` | <span data-ttu-id="8ea40-170">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-170">ExplicitNumeric</span></span> | `conv.i2` / `conv.ovf.i2` |
| `nint` | `ushort` | <span data-ttu-id="8ea40-171">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-171">ExplicitNumeric</span></span> | `conv.u2` / `conv.ovf.u2` |
| `nint` | `int` | <span data-ttu-id="8ea40-172">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-172">ExplicitNumeric</span></span> | `conv.i4` / `conv.ovf.i4` |
| `nint` | `uint` | <span data-ttu-id="8ea40-173">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-173">ExplicitNumeric</span></span> | `conv.u4` / `conv.ovf.u4` |
| `nint` | `long` | <span data-ttu-id="8ea40-174">隐式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-174">ImplicitNumeric</span></span> | `conv.i8` / `conv.ovf.i8` |
| `nint` | `ulong` | <span data-ttu-id="8ea40-175">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-175">ExplicitNumeric</span></span> | `conv.i8` / `conv.ovf.i8` |
| `nint` | `char` | <span data-ttu-id="8ea40-176">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-176">ExplicitNumeric</span></span> | `conv.u2` / `conv.ovf.u2` |
| `nint` | `float` | <span data-ttu-id="8ea40-177">隐式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-177">ImplicitNumeric</span></span> | `conv.r4` |
| `nint` | `double` | <span data-ttu-id="8ea40-178">隐式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-178">ImplicitNumeric</span></span> | `conv.r8` |
| `nint` | `decimal` | <span data-ttu-id="8ea40-179">隐式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-179">ImplicitNumeric</span></span> | `conv.i8 decimal decimal.op_Implicit(long)` |
| `nint` | `IntPtr` | <span data-ttu-id="8ea40-180">标识</span><span class="sxs-lookup"><span data-stu-id="8ea40-180">Identity</span></span> | |
| `nint` | `UIntPtr` | <span data-ttu-id="8ea40-181">无</span><span class="sxs-lookup"><span data-stu-id="8ea40-181">None</span></span> | |
| `nuint` | `object` | <span data-ttu-id="8ea40-182">装箱</span><span class="sxs-lookup"><span data-stu-id="8ea40-182">Boxing</span></span> | `box` |
| `nuint` | `void*` | <span data-ttu-id="8ea40-183">指针到虚空</span><span class="sxs-lookup"><span data-stu-id="8ea40-183">PointerToVoid</span></span> | `conv.u` |
| `nuint` | `nint` | <span data-ttu-id="8ea40-184">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-184">ExplicitNumeric</span></span> | `conv.i` / `conv.ovf.i` |
| `nuint` | `sbyte` | <span data-ttu-id="8ea40-185">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-185">ExplicitNumeric</span></span> | `conv.i1` / `conv.ovf.i1` |
| `nuint` | `byte` | <span data-ttu-id="8ea40-186">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-186">ExplicitNumeric</span></span> | `conv.u1` / `conv.ovf.u1` |
| `nuint` | `short` | <span data-ttu-id="8ea40-187">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-187">ExplicitNumeric</span></span> | `conv.i2` / `conv.ovf.i2` |
| `nuint` | `ushort` | <span data-ttu-id="8ea40-188">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-188">ExplicitNumeric</span></span> | `conv.u2` / `conv.ovf.u2` |
| `nuint` | `int` | <span data-ttu-id="8ea40-189">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-189">ExplicitNumeric</span></span> | `conv.i4` / `conv.ovf.i4` |
| `nuint` | `uint` | <span data-ttu-id="8ea40-190">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-190">ExplicitNumeric</span></span> | `conv.u4` / `conv.ovf.u4` |
| `nuint` | `long` | <span data-ttu-id="8ea40-191">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-191">ExplicitNumeric</span></span> | `conv.i8` / `conv.ovf.i8` |
| `nuint` | `ulong` | <span data-ttu-id="8ea40-192">隐式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-192">ImplicitNumeric</span></span> | `conv.u8` / `conv.ovf.u8` |
| `nuint` | `char` | <span data-ttu-id="8ea40-193">显式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-193">ExplicitNumeric</span></span> | `conv.u2` / `conv.ovf.u2.un` |
| `nuint` | `float` | <span data-ttu-id="8ea40-194">隐式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-194">ImplicitNumeric</span></span> | `conv.r.un conv.r4` |
| `nuint` | `double` | <span data-ttu-id="8ea40-195">隐式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-195">ImplicitNumeric</span></span> | `conv.r.un conv.r8` |
| `nuint` | `decimal` | <span data-ttu-id="8ea40-196">隐式数字</span><span class="sxs-lookup"><span data-stu-id="8ea40-196">ImplicitNumeric</span></span> | `conv.u8 decimal decimal.op_Implicit(ulong)` |
| `nuint` | `IntPtr` | <span data-ttu-id="8ea40-197">无</span><span class="sxs-lookup"><span data-stu-id="8ea40-197">None</span></span> | |
| `nuint` | `UIntPtr` | <span data-ttu-id="8ea40-198">标识</span><span class="sxs-lookup"><span data-stu-id="8ea40-198">Identity</span></span> | |

<span data-ttu-id="8ea40-199">转换为`A``Nullable<B>`：</span><span class="sxs-lookup"><span data-stu-id="8ea40-199">Conversion from `A` to `Nullable<B>` is:</span></span>
- <span data-ttu-id="8ea40-200">如果存在从`A`转换为 的标识转换或隐式转换，则隐`B`式空转换。</span><span class="sxs-lookup"><span data-stu-id="8ea40-200">an implicit nullable conversion if there is an identity conversion or implicit conversion from `A` to `B`;</span></span>
- <span data-ttu-id="8ea40-201">如果显式转换为`A``B`</span><span class="sxs-lookup"><span data-stu-id="8ea40-201">an explicit nullable conversion if there is an explicit conversion from `A` to `B`;</span></span>
- <span data-ttu-id="8ea40-202">否则无效。</span><span class="sxs-lookup"><span data-stu-id="8ea40-202">otherwise invalid.</span></span>

<span data-ttu-id="8ea40-203">转换为`Nullable<A>``B`：</span><span class="sxs-lookup"><span data-stu-id="8ea40-203">Conversion from `Nullable<A>` to `B` is:</span></span>
- <span data-ttu-id="8ea40-204">如果存在从 转换为 的标识转换或隐式或显式数字转换`A`，则显式可`B`取消转换。</span><span class="sxs-lookup"><span data-stu-id="8ea40-204">an explicit nullable conversion if there is an identity conversion or implicit or explicit numeric conversion from `A` to `B`;</span></span>
- <span data-ttu-id="8ea40-205">否则无效。</span><span class="sxs-lookup"><span data-stu-id="8ea40-205">otherwise invalid.</span></span>

<span data-ttu-id="8ea40-206">转换为`Nullable<A>``Nullable<B>`：</span><span class="sxs-lookup"><span data-stu-id="8ea40-206">Conversion from `Nullable<A>` to `Nullable<B>` is:</span></span>
- <span data-ttu-id="8ea40-207">如果从`A``B`转换为</span><span class="sxs-lookup"><span data-stu-id="8ea40-207">an identity conversion if there is an identity conversion from `A` to `B`;</span></span>
- <span data-ttu-id="8ea40-208">如果从`A``B`</span><span class="sxs-lookup"><span data-stu-id="8ea40-208">an explicit nullable conversion if there is an implicit or explicit numeric conversion from `A` to `B`;</span></span>
- <span data-ttu-id="8ea40-209">否则无效。</span><span class="sxs-lookup"><span data-stu-id="8ea40-209">otherwise invalid.</span></span>

### <a name="operators"></a><span data-ttu-id="8ea40-210">运算符</span><span class="sxs-lookup"><span data-stu-id="8ea40-210">Operators</span></span>

<span data-ttu-id="8ea40-211">预定义的运算符如下所示。</span><span class="sxs-lookup"><span data-stu-id="8ea40-211">The predefined operators are as follows.</span></span>
<span data-ttu-id="8ea40-212">_如果至少一个操作数是类型`nint`或`nuint`，_ 则根据隐式转换的正常规则在重载解析期间考虑这些运算符。</span><span class="sxs-lookup"><span data-stu-id="8ea40-212">These operators are considered during overload resolution based on normal rules for implicit conversions _if at least one of the operands is of type `nint` or `nuint`_.</span></span>

<span data-ttu-id="8ea40-213">（每个运算符的 IL 包括 的`unchecked`变体和`checked`上下文（如果不同）。</span><span class="sxs-lookup"><span data-stu-id="8ea40-213">(The IL for each operator includes the variants for `unchecked` and `checked` contexts if different.)</span></span>

| <span data-ttu-id="8ea40-214">一元</span><span class="sxs-lookup"><span data-stu-id="8ea40-214">Unary</span></span> | <span data-ttu-id="8ea40-215">操作员签名</span><span class="sxs-lookup"><span data-stu-id="8ea40-215">Operator Signature</span></span> | <span data-ttu-id="8ea40-216">IL</span><span class="sxs-lookup"><span data-stu-id="8ea40-216">IL</span></span> |
|:---:|:---:|:---:|
| `+` | `nint operator +(nint value)` | `nop` |
| `+` | `nuint operator +(nuint value)` | `nop` |
| `-` | `nint operator -(nint value)` | `neg` |
| `~` | `nint operator ~(nint value)` | `not` |
| `~` | `nuint operator ~(nuint value)` | `not` |

| <span data-ttu-id="8ea40-217">Binary</span><span class="sxs-lookup"><span data-stu-id="8ea40-217">Binary</span></span> | <span data-ttu-id="8ea40-218">操作员签名</span><span class="sxs-lookup"><span data-stu-id="8ea40-218">Operator Signature</span></span> | <span data-ttu-id="8ea40-219">IL</span><span class="sxs-lookup"><span data-stu-id="8ea40-219">IL</span></span> |
|:---:|:---:|:---:|
| `+` | `nint operator +(nint left, nint right)` | `add` / `add.ovf` |
| `+` | `nuint operator +(nuint left, nuint right)` | `add` / `add.ovf.un` |
| `-` | `nint operator -(nint left, nint right)` | `sub` / `sub.ovf` |
| `-` | `nuint operator -(nuint left, nuint right)` | `sub` / `sub.ovf.un` |
| `*` | `nint operator *(nint left, nint right)` | `mul` / `mul.ovf` |
| `*` | `nuint operator *(nuint left, nuint right)` | `mul` / `mul.ovf.un` |
| `/` | `nint operator /(nint left, nint right)` | `div` |
| `/` | `nuint operator /(nuint left, nuint right)` | `div.un` |
| `%` | `nint operator %(nint left, nint right)` | `rem` |
| `%` | `nuint operator %(nuint left, nuint right)` | `rem.un` |
| `==` | `bool operator ==(nint left, nint right)` | `beq` / `ceq` |
| `==` | `bool operator ==(nuint left, nuint right)` | `beq` / `ceq` |
| `!=` | `bool operator !=(nint left, nint right)` | `bne` |
| `!=` | `bool operator !=(nuint left, nuint right)` | `bne` |
| `<` | `bool operator <(nint left, nint right)` | `blt` / `clt` |
| `<` | `bool operator <(nuint left, nuint right)` | `blt.un` / `clt.un` |
| `<=` | `bool operator <=(nint left, nint right)` | `ble` |
| `<=` | `bool operator <=(nuint left, nuint right)` | `ble.un` |
| `>` | `bool operator >(nint left, nint right)` | `bgt` / `cgt` |
| `>` | `bool operator >(nuint left, nuint right)` | `bgt.un` / `cgt.un` |
| `>=` | `bool operator >=(nint left, nint right)` | `bge` |
| `>=` | `bool operator >=(nuint left, nuint right)` | `bge.un` |
| `&` | `nint operator &(nint left, nint right)` | `and` |
| `&` | `nuint operator &(nuint left, nuint right)` | `and` |
| <code>&#124;</code> | <code>nint operator &#124;(nint left, nint right)</code> | `or` |
| <code>&#124;</code> | <code>nuint operator &#124;(nuint left, nuint right)</code> | `or` |
| `^` | `nint operator ^(nint left, nint right)` | `xor` |
| `^` | `nuint operator ^(nuint left, nuint right)` | `xor` |
| `<<` | `nint operator <<(nint left, int right)` | `shl` |
| `<<` | `nuint operator <<(nuint left, int right)` | `shl` |
| `>>` | `nint operator >>(nint left, int right)` | `shr` |
| `>>` | `nuint operator >>(nuint left, int right)` | `shr.un` |

<span data-ttu-id="8ea40-220">对于某些二进制运算符，IL 运算符支持其他操作数类型（请参阅[ECMA-335](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf) III.1.5 操作数类型表）。</span><span class="sxs-lookup"><span data-stu-id="8ea40-220">For some binary operators, the IL operators support additional operand types (see [ECMA-335](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf) III.1.5 Operand type table).</span></span>
<span data-ttu-id="8ea40-221">但是，为了简单起见和与语言中现有运算符的一致性，C# 支持的一组操作数类型受到限制。</span><span class="sxs-lookup"><span data-stu-id="8ea40-221">But the set of operand types supported by C# is limited for simplicity and for consistency with existing operators in the language.</span></span>

<span data-ttu-id="8ea40-222">支持运算符的已提升版本，其中参数和返回类型为`nint?`和`nuint?`。</span><span class="sxs-lookup"><span data-stu-id="8ea40-222">Lifted versions of the operators, where the arguments and return types are `nint?` and `nuint?`, are supported.</span></span>

<span data-ttu-id="8ea40-223">`x`本机`y`int 的复合赋值操作`x op= y`遵循与具有预定义运算符的其他基元类型相同的规则。</span><span class="sxs-lookup"><span data-stu-id="8ea40-223">Compound assignment operations `x op= y` where `x` or `y` are native ints follow the same rules as with other primitive types with pre-defined operators.</span></span>
<span data-ttu-id="8ea40-224">具体来说，`x = (T)(x op y)`表达式绑定为 类型`T``x`和位置`x`仅计算一次。</span><span class="sxs-lookup"><span data-stu-id="8ea40-224">Specifically the expression is bound as `x = (T)(x op y)` where `T` is the type of `x` and where `x` is only evaluated once.</span></span>

<span data-ttu-id="8ea40-225">换档操作员应屏蔽要移动的位数 - 如果`sizeof(nint)`为 4，则为 5 位，如果`sizeof(nint)`为 8，则为 6 位。</span><span class="sxs-lookup"><span data-stu-id="8ea40-225">The shift operators should mask the number of bits to shift - to 5 bits if `sizeof(nint)` is 4, and to 6 bits if `sizeof(nint)` is 8.</span></span>
<span data-ttu-id="8ea40-226">（请参阅 C# 规范中的[移位运算符](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#shift-operators)）。</span><span class="sxs-lookup"><span data-stu-id="8ea40-226">(see [shift operators](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#shift-operators) in C# spec).</span></span>

### <a name="dynamic"></a><span data-ttu-id="8ea40-227">动态</span><span class="sxs-lookup"><span data-stu-id="8ea40-227">Dynamic</span></span>

<span data-ttu-id="8ea40-228">转换和运算符由编译器合成，不是基础`IntPtr`和`UIntPtr`类型的一部分。</span><span class="sxs-lookup"><span data-stu-id="8ea40-228">The conversions and operators are synthesized by the compiler and are not part of the underlying `IntPtr` and `UIntPtr` types.</span></span>
<span data-ttu-id="8ea40-229">因此，这些转换和运算符从 的运行时活页夹`dynamic`_中不可用_。</span><span class="sxs-lookup"><span data-stu-id="8ea40-229">As a result those conversions and operators _are not available_ from the runtime binder for `dynamic`.</span></span> 

```C#
nint x = 2;
nint y = x + x; // ok
dynamic d = x;
nint z = d + x; // RuntimeBinderException: '+' cannot be applied 'System.IntPtr' and 'System.IntPtr'
```

### <a name="type-members"></a><span data-ttu-id="8ea40-230">类型成员</span><span class="sxs-lookup"><span data-stu-id="8ea40-230">Type members</span></span>

<span data-ttu-id="8ea40-231">或`nint``nuint`的唯一构造函数是 无参数构造函数。</span><span class="sxs-lookup"><span data-stu-id="8ea40-231">The only constructor for `nint` or `nuint` is the parameter-less constructor.</span></span>

<span data-ttu-id="8ea40-232">的以下`System.IntPtr`成员和`System.UIntPtr`_被明确排除_或`nint`： `nuint`</span><span class="sxs-lookup"><span data-stu-id="8ea40-232">The following members of `System.IntPtr` and `System.UIntPtr` _are explicitly excluded_ from `nint` or `nuint`:</span></span>
```C#
// constructors
// arithmetic operators
// implicit and explicit conversions
public static readonly IntPtr Zero; // use 0 instead
public static int Size { get; }     // use sizeof() instead
public static IntPtr Add(IntPtr pointer, int offset);
public static IntPtr Subtract(IntPtr pointer, int offset);
public int ToInt32();
public long ToInt64();
public void* ToPointer();
```

<span data-ttu-id="8ea40-233">的`System.IntPtr``System.UIntPtr`其余成员_被隐式包含在_和`nint``nuint`中。</span><span class="sxs-lookup"><span data-stu-id="8ea40-233">The remaining members of `System.IntPtr` and `System.UIntPtr` _are implicitly included_ in `nint` and `nuint`.</span></span> <span data-ttu-id="8ea40-234">对于 .NET 框架 4.7.2：</span><span class="sxs-lookup"><span data-stu-id="8ea40-234">For .NET Framework 4.7.2:</span></span>
```C#
public override bool Equals(object obj);
public override int GetHashCode();
public override string ToString();
public string ToString(string format);
```

<span data-ttu-id="8ea40-235">实现的`System.IntPtr``System.UIntPtr`接口_被隐式包含在_和`nint``nuint`中，基础类型的出现被相应的本机整数类型替换。</span><span class="sxs-lookup"><span data-stu-id="8ea40-235">Interfaces implemented by `System.IntPtr` and `System.UIntPtr` _are implicitly included_ in `nint` and `nuint`, with occurrences of the underlying types replaced by the corresponding native integer types.</span></span>
<span data-ttu-id="8ea40-236">例如，如果`IntPtr`实现`ISerializable, IEquatable<IntPtr>, IComparable<IntPtr>`，`nint`则`ISerializable, IEquatable<nint>, IComparable<nint>`实现 。</span><span class="sxs-lookup"><span data-stu-id="8ea40-236">For instance if `IntPtr` implements `ISerializable, IEquatable<IntPtr>, IComparable<IntPtr>`, then `nint` implements `ISerializable, IEquatable<nint>, IComparable<nint>`.</span></span>

### <a name="overriding-hiding-and-implementing"></a><span data-ttu-id="8ea40-237">重写、隐藏和实现</span><span class="sxs-lookup"><span data-stu-id="8ea40-237">Overriding, hiding, and implementing</span></span>

<span data-ttu-id="8ea40-238">`nint`和`System.IntPtr`和`nuint``System.UIntPtr`和 被视为等效于重写、隐藏和实现。</span><span class="sxs-lookup"><span data-stu-id="8ea40-238">`nint` and `System.IntPtr`, and `nuint` and `System.UIntPtr`, are considered equivalent for overriding, hiding, and implementing.</span></span>

<span data-ttu-id="8ea40-239">过载不能单独因`nint``System.IntPtr`和`nuint`和`System.UIntPtr`和 而不同。</span><span class="sxs-lookup"><span data-stu-id="8ea40-239">Overloads cannot differ by `nint` and `System.IntPtr`, and `nuint` and `System.UIntPtr`, alone.</span></span>
<span data-ttu-id="8ea40-240">重写和实现可能因`nint`和`System.IntPtr`、 或`nuint`和`System.UIntPtr`单独而异。</span><span class="sxs-lookup"><span data-stu-id="8ea40-240">Overrides and implementations may differ by `nint` and `System.IntPtr`, or `nuint` and `System.UIntPtr`, alone.</span></span>
<span data-ttu-id="8ea40-241">方法隐藏`nint`其他单独因 和`System.IntPtr`或`nuint``System.UIntPtr`和 而 不同的方法。</span><span class="sxs-lookup"><span data-stu-id="8ea40-241">Methods hide other methods that differ by `nint` and `System.IntPtr`, or `nuint` and `System.UIntPtr`, alone.</span></span>

### <a name="miscellaneous"></a><span data-ttu-id="8ea40-242">杂项</span><span class="sxs-lookup"><span data-stu-id="8ea40-242">Miscellaneous</span></span>

<span data-ttu-id="8ea40-243">`nint`用作`nuint`数组索引的表达式无需转换即可发出。</span><span class="sxs-lookup"><span data-stu-id="8ea40-243">`nint` and `nuint` expressions used as array indices are emitted without conversion.</span></span>
```C#
static object GetItem(object[] array, nint index)
{
    return array[index]; // ok
}
```

<span data-ttu-id="8ea40-244">`nint`并`nuint`可用作`enum`基类型。</span><span class="sxs-lookup"><span data-stu-id="8ea40-244">`nint` and `nuint` can be used as an `enum` base type.</span></span>
```C#
enum E : nint // ok
{
}
```

<span data-ttu-id="8ea40-245">读取`nint`和写入对于类型`nuint`、 和`enum`基类型`nint`或`nuint`是原子的。</span><span class="sxs-lookup"><span data-stu-id="8ea40-245">Reads and writes are atomic for types `nint`, `nuint`, and `enum` with base type `nint` or `nuint`.</span></span>

<span data-ttu-id="8ea40-246">字段可以标记为`volatile`类型`nint`和`nuint`。</span><span class="sxs-lookup"><span data-stu-id="8ea40-246">Fields may be marked `volatile` for types `nint` and `nuint`.</span></span>
<span data-ttu-id="8ea40-247">[ECMA-334](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-334.pdf) 15.5.4 不包括`enum`基型`System.IntPtr`或`System.UIntPtr`。但是。</span><span class="sxs-lookup"><span data-stu-id="8ea40-247">[ECMA-334](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-334.pdf) 15.5.4 does not include `enum` with base type `System.IntPtr` or `System.UIntPtr` however.</span></span>

<span data-ttu-id="8ea40-248">`default(nint)`并`new nint()`等效于`(nint)0`。</span><span class="sxs-lookup"><span data-stu-id="8ea40-248">`default(nint)` and `new nint()` are equivalent to `(nint)0`.</span></span>

<span data-ttu-id="8ea40-249">`typeof(nint)` 为 `typeof(IntPtr)`。</span><span class="sxs-lookup"><span data-stu-id="8ea40-249">`typeof(nint)` is `typeof(IntPtr)`.</span></span>

<span data-ttu-id="8ea40-250">`sizeof(nint)`受支持，但需要在不安全的上下文中进行编译（同样如此`sizeof(IntPtr)`）。</span><span class="sxs-lookup"><span data-stu-id="8ea40-250">`sizeof(nint)` is supported but requires compiling in an unsafe context (as does `sizeof(IntPtr)`).</span></span>
<span data-ttu-id="8ea40-251">该值不是编译时间常量。</span><span class="sxs-lookup"><span data-stu-id="8ea40-251">The value is not a compile-time constant.</span></span>
<span data-ttu-id="8ea40-252">`sizeof(nint)`作为`sizeof(IntPtr)`而不是`IntPtr.Size`实现 。</span><span class="sxs-lookup"><span data-stu-id="8ea40-252">`sizeof(nint)` is implemented as `sizeof(IntPtr)` rather than `IntPtr.Size`.</span></span>

<span data-ttu-id="8ea40-253">涉及`nint`或`nuint`报告`nint`或`nuint`而不是 或 的类型`IntPtr`引用的`UIntPtr`编译器诊断。</span><span class="sxs-lookup"><span data-stu-id="8ea40-253">Compiler diagnostics for type references involving `nint` or `nuint` report `nint` or `nuint` rather than `IntPtr` or `UIntPtr`.</span></span>

### <a name="metadata"></a><span data-ttu-id="8ea40-254">元数据</span><span class="sxs-lookup"><span data-stu-id="8ea40-254">Metadata</span></span>

<span data-ttu-id="8ea40-255">`nint`并在`nuint`元数据中表示为`System.IntPtr``System.UIntPtr`和 。</span><span class="sxs-lookup"><span data-stu-id="8ea40-255">`nint` and `nuint` are represented in metadata as `System.IntPtr` and `System.UIntPtr`.</span></span>

<span data-ttu-id="8ea40-256">包含`nint`或`nuint`用 发出的类型引用`System.Runtime.CompilerServices.NativeIntegerAttribute`，以指示类型引用的哪些部分是本机 int。</span><span class="sxs-lookup"><span data-stu-id="8ea40-256">Type references that include `nint` or `nuint` are emitted with a `System.Runtime.CompilerServices.NativeIntegerAttribute` to indicate which parts of the type reference are native ints.</span></span>

```C#
namespace System.Runtime.CompilerServices
{
    [AttributeUsage(
        AttributeTargets.Class |
        AttributeTargets.Event |
        AttributeTargets.Field |
        AttributeTargets.GenericParameter |
        AttributeTargets.Parameter |
        AttributeTargets.Property |
        AttributeTargets.ReturnValue,
        AllowMultiple = false,
        Inherited = false)]
    public sealed class NativeIntegerAttribute : Attribute
    {
        public NativeIntegerAttribute()
        {
            TransformFlags = new[] { true };
        }
        public NativeIntegerAttribute(bool[] flags)
        {
            TransformFlags = flags;
        }
        public IList<bool> TransformFlags { get; }
    }
}
```

<span data-ttu-id="8ea40-257">编码使用用于编码`DynamicAttribute`的方法，尽管显然`DynamicAttribute`编码类型引用中的哪些类型`dynamic`是本机 ints，而不是哪些类型是本机 int。</span><span class="sxs-lookup"><span data-stu-id="8ea40-257">The encoding uses the approach as used to encode `DynamicAttribute`, although obviously `DynamicAttribute` is encoding which types within the type reference are `dynamic` rather than which types are native ints.</span></span>
<span data-ttu-id="8ea40-258">如果编码导致`false`值数组，则不需要。 `NativeIntegerAttribute`</span><span class="sxs-lookup"><span data-stu-id="8ea40-258">If the encoding results in an array of `false` values, no `NativeIntegerAttribute` is needed.</span></span>
<span data-ttu-id="8ea40-259">无`NativeIntegerAttribute`参数构造函数生成具有单个`true`值的编码。</span><span class="sxs-lookup"><span data-stu-id="8ea40-259">The parameterless `NativeIntegerAttribute` constructor generates an encoding with a single `true` value.</span></span>

```C#
nuint A;                    // [NativeInteger] UIntPtr A
(Stream, nint) B;           // [NativeInteger(new[] { false, false, true })] ValueType<Stream, IntPtr> B
```

## <a name="alternatives"></a><span data-ttu-id="8ea40-260">备选项</span><span class="sxs-lookup"><span data-stu-id="8ea40-260">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="8ea40-261">与上述"类型擦除"方法的替代方法是引入新类型：`System.NativeInt`和`System.NativeUInt`。</span><span class="sxs-lookup"><span data-stu-id="8ea40-261">An alternative to the "type erasure" approach above is to introduce new types: `System.NativeInt` and `System.NativeUInt`.</span></span>
```C#
public readonly struct NativeInt
{
    public IntPtr Value;
}
```
<span data-ttu-id="8ea40-262">不同的类型将允许重载与 和`IntPtr`不同，允许不同的分析和`ToString()`。</span><span class="sxs-lookup"><span data-stu-id="8ea40-262">Distinct types would allow overloading distinct from `IntPtr` and would allow distinct parsing and `ToString()`.</span></span>
<span data-ttu-id="8ea40-263">但是，CLR 将有更多的工作能够有效地处理这些类型，从而破坏功能的主要目的 - 效率。</span><span class="sxs-lookup"><span data-stu-id="8ea40-263">But there would be more work for the CLR to handle these types efficiently which defeats the primary purpose of the feature - efficiency.</span></span>
<span data-ttu-id="8ea40-264">与使用`IntPtr`的现有本机 int 代码进行交互将更加困难。</span><span class="sxs-lookup"><span data-stu-id="8ea40-264">And interop with existing native int code that uses `IntPtr` would be more difficult.</span></span> 

<span data-ttu-id="8ea40-265">另一种方法是在框架`IntPtr`中添加更多本机 int 支持，但没有任何特定的编译器支持。</span><span class="sxs-lookup"><span data-stu-id="8ea40-265">Another alternative is to add more native int support for `IntPtr` in the framework but without any specific compiler support.</span></span>
<span data-ttu-id="8ea40-266">编译器将自动支持任何新的转换和算术运算。</span><span class="sxs-lookup"><span data-stu-id="8ea40-266">Any new conversions and arithmetic operations would be supported by the compiler automatically.</span></span>
<span data-ttu-id="8ea40-267">但是，该语言不会提供关键字、常量或`checked`操作。</span><span class="sxs-lookup"><span data-stu-id="8ea40-267">But the language would not provide keywords, constants, or `checked` operations.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="8ea40-268">设计会议</span><span class="sxs-lookup"><span data-stu-id="8ea40-268">Design meetings</span></span>

<span data-ttu-id="8ea40-269">https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-26.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-13.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-07-05.md#native-int-and-intptr-operators https://github.com/dotnet/csharplang/blob/master/meetings/2019/LDM-2019-10-23.md https://github.com/dotnet/csharplang/blob/master/meetings/2020/LDM-2020-03-25.md</span><span class="sxs-lookup"><span data-stu-id="8ea40-269">https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-26.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-13.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-07-05.md#native-int-and-intptr-operators https://github.com/dotnet/csharplang/blob/master/meetings/2019/LDM-2019-10-23.md https://github.com/dotnet/csharplang/blob/master/meetings/2020/LDM-2020-03-25.md</span></span>
