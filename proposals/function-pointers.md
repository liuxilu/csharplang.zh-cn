---
ms.openlocfilehash: e5ab385f498c0d96c55e60751bb204e7217f6eab
ms.sourcegitcommit: 95f5f86ba2e2a23cd4fb37bd9d1ff690c83d1191
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "81646688"
---
# <a name="function-pointers"></a><span data-ttu-id="e5ebc-101">函数指针</span><span class="sxs-lookup"><span data-stu-id="e5ebc-101">Function Pointers</span></span>

## <a name="summary"></a><span data-ttu-id="e5ebc-102">总结</span><span class="sxs-lookup"><span data-stu-id="e5ebc-102">Summary</span></span>

<span data-ttu-id="e5ebc-103">此建议提供语言构造，用于公开当前无法高效访问的 IL 操作代码，或者当前在 C# 中完全访问`ldftn` `calli`IL 操作代码：和 。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-103">This proposal provides language constructs that expose IL opcodes that cannot currently be accessed efficiently, or at all, in C# today: `ldftn` and `calli`.</span></span> <span data-ttu-id="e5ebc-104">这些 IL 操作代码在高性能代码中非常重要，开发人员需要一种有效的方法来访问它们。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-104">These IL opcodes can be important in high performance code and developers need an efficient way to access them.</span></span>

## <a name="motivation"></a><span data-ttu-id="e5ebc-105">动机</span><span class="sxs-lookup"><span data-stu-id="e5ebc-105">Motivation</span></span>

<span data-ttu-id="e5ebc-106">此功能的动机和背景在以下问题中描述（该功能的潜在实现）：</span><span class="sxs-lookup"><span data-stu-id="e5ebc-106">The motivations and background for this feature are described in the following issue (as is a potential implementation of the feature):</span></span>

https://github.com/dotnet/csharplang/issues/191

<span data-ttu-id="e5ebc-107">这是[编译器内部函数](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)的替代设计建议</span><span class="sxs-lookup"><span data-stu-id="e5ebc-107">This is an alternate design proposal to [compiler intrinsics](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)</span></span>

## <a name="detailed-design"></a><span data-ttu-id="e5ebc-108">详细设计</span><span class="sxs-lookup"><span data-stu-id="e5ebc-108">Detailed Design</span></span>

### <a name="function-pointers"></a><span data-ttu-id="e5ebc-109">函数指针</span><span class="sxs-lookup"><span data-stu-id="e5ebc-109">Function pointers</span></span>

<span data-ttu-id="e5ebc-110">该语言将允许使用`delegate*`语法声明函数指针。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-110">The language will allow for the declaration of function pointers using the `delegate*` syntax.</span></span> <span data-ttu-id="e5ebc-111">下一节将详细介绍完整的语法，但它旨在类似于 和`Func``Action`类型声明使用的语法。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-111">The full syntax is described in detail in the next section but it is meant to resemble the syntax used by `Func` and `Action` type declarations.</span></span>

``` csharp
unsafe class Example {
    void Example(Action<int> a, delegate*<int, void> f) {
        a(42);
        f(42);
    }
}
```

<span data-ttu-id="e5ebc-112">这些类型使用 ECMA-335 中概述的函数指针类型表示。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-112">These types are represented using the function pointer type as outlined in ECMA-335.</span></span> <span data-ttu-id="e5ebc-113">这意味着`delegate*`调用 将在`calli`方法上`delegate``callvirt``Invoke`使用 调用 。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-113">This means invocation of a `delegate*` will use `calli` where invocation of a `delegate` will use `callvirt` on the `Invoke` method.</span></span>
<span data-ttu-id="e5ebc-114">语法虽然调用是相同的两个构造。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-114">Syntactically though invocation is identical for both constructs.</span></span>

<span data-ttu-id="e5ebc-115">方法指针的 ECMA-335 定义包括调用约定作为类型签名的一部分（第 7.1 节）。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-115">The ECMA-335 definition of method pointers includes the calling convention as part of the type signature (section 7.1).</span></span>
<span data-ttu-id="e5ebc-116">默认调用约定将为`managed`。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-116">The default calling convention will be `managed`.</span></span> <span data-ttu-id="e5ebc-117">可以通过在`delegate*`语法： `managed`、 `cdecl` `stdcall` `thiscall`、 或`unmanaged`上添加相应的修饰符来指定备用窗体。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-117">Alternate forms can be specified by adding the appropriate modifier after the `delegate*` syntax: `managed`, `cdecl`, `stdcall`, `thiscall`, or `unmanaged`.</span></span> <span data-ttu-id="e5ebc-118">示例：</span><span class="sxs-lookup"><span data-stu-id="e5ebc-118">Example:</span></span>

``` csharp
// This method will be invoked using the cdecl calling convention
delegate* cdecl<int, int>;

// This method will be invoked using the stdcall calling convention
delegate* stdcall<int, int>;
```

<span data-ttu-id="e5ebc-119">类型之间的`delegate*`转换基于其签名（包括调用约定）进行。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-119">Conversions between `delegate*` types is done based on their signature including the calling convention.</span></span>

``` csharp
unsafe class Example {
    void Conversions() {
        delegate*<int, int, int> p1 = ...;
        delegate* managed<int, int, int> p2 = ...;
        delegate* cdecl<int, int, int> p3 = ...;

        p1 = p2; // okay p1 and p2 have compatible signatures
        Console.WriteLine(p2 == p1); // True
        p2 = p3; // error: calling conventions are incompatible
    }
}
```

<span data-ttu-id="e5ebc-120">类型`delegate*`是指针类型，这意味着它具有标准指针类型的所有功能和限制：</span><span class="sxs-lookup"><span data-stu-id="e5ebc-120">A `delegate*` type is a pointer type which means it has all of the capabilities and restrictions of a standard pointer type:</span></span>

- <span data-ttu-id="e5ebc-121">仅在`unsafe`上下文中有效。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-121">Only valid in an `unsafe` context.</span></span>
- <span data-ttu-id="e5ebc-122">只能从`unsafe`上下文中调用`delegate*`包含参数或返回类型的方法。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-122">Methods which contain a `delegate*` parameter or return type can only be called from an `unsafe` context.</span></span>
- <span data-ttu-id="e5ebc-123">无法转换为`object`。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-123">Cannot be converted to `object`.</span></span>
- <span data-ttu-id="e5ebc-124">不能用作泛型参数。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-124">Cannot be used as a generic argument.</span></span>
- <span data-ttu-id="e5ebc-125">可以隐式`delegate*`转换为`void*`。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-125">Can implicitly convert `delegate*` to `void*`.</span></span>
- <span data-ttu-id="e5ebc-126">可以显式从`void*`转换为`delegate*`。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-126">Can explicitly convert from `void*` to `delegate*`.</span></span>

<span data-ttu-id="e5ebc-127">限制：</span><span class="sxs-lookup"><span data-stu-id="e5ebc-127">Restrictions:</span></span>

- <span data-ttu-id="e5ebc-128">自定义属性不能应用于 或其`delegate*`任何元素。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-128">Custom attributes cannot be applied to a `delegate*` or any of its elements.</span></span>
- <span data-ttu-id="e5ebc-129">参数`delegate*`不能标记为`params`</span><span class="sxs-lookup"><span data-stu-id="e5ebc-129">A `delegate*` parameter cannot be marked as `params`</span></span>
- <span data-ttu-id="e5ebc-130">类型`delegate*`具有法线指针类型的所有限制。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-130">A `delegate*` type has all of the restrictions of a normal pointer type.</span></span>

### <a name="function-pointer-syntax"></a><span data-ttu-id="e5ebc-131">函数指针语法</span><span class="sxs-lookup"><span data-stu-id="e5ebc-131">Function pointer syntax</span></span>

<span data-ttu-id="e5ebc-132">完整的函数指针语法由以下语法表示：</span><span class="sxs-lookup"><span data-stu-id="e5ebc-132">The full function pointer syntax is represented by the following grammar:</span></span>

```antlr
pointer_type
    : ...
    | funcptr_type
    ;

funcptr_type
    : 'delegate' '*' calling_convention? '<' (funcptr_parameter_modifier? type ',')* funcptr_return_modifier? return_type '>'
    ;

calling_convention
    : 'cdecl'
    | 'managed'
    | 'stdcall'
    | 'thiscall'
    | 'unmanaged'
    ;

funcptr_parameter_modifier
    : 'ref'
    | 'out'
    | 'in'
    ;

funcptr_return_modifier
    : 'ref'
    | 'ref readonly'
    ;
```

<span data-ttu-id="e5ebc-133">调用`unmanaged`约定表示当前平台上本机代码的默认调用约定，并编码为 winapi。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-133">The `unmanaged` calling convention represents the default calling convention for native code on the current platform, and is encoded as winapi.</span></span>
<span data-ttu-id="e5ebc-134">所有`calling_convention`s 都是上下文关键字，前面是`delegate*`。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-134">All `calling_convention`s are contextual keywords when preceded by a `delegate*`.</span></span>

``` csharp
delegate int Func1(string s);
delegate Func1 Func2(Func1 f);

// Function pointer equivalent without calling convention
delegate*<string, int>;
delegate*<delegate*<string, int>, delegate*<string, int>>;

// Function pointer equivalent with calling convention
delegate* managed<string, int>;
delegate*<delegate* managed<string, int>, delegate*<string, int>>;
```

### <a name="function-pointer-conversions"></a><span data-ttu-id="e5ebc-135">函数指针转换</span><span class="sxs-lookup"><span data-stu-id="e5ebc-135">Function pointer conversions</span></span>

<span data-ttu-id="e5ebc-136">在不安全的上下文中，一组可用的隐式转换（隐式转换）将扩展为包括以下隐式指针转换：</span><span class="sxs-lookup"><span data-stu-id="e5ebc-136">In an unsafe context, the set of available implicit conversions (Implicit conversions) is extended to include the following implicit pointer conversions:</span></span>
- [<span data-ttu-id="e5ebc-137">_现有转换_</span><span class="sxs-lookup"><span data-stu-id="e5ebc-137">_Existing conversions_</span></span>](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#pointer-conversions)
- <span data-ttu-id="e5ebc-138">从_funcptr\_类型_`F0`到另一种_funcptr\_类型_`F1`，前提是以下所有类型都是正确的：</span><span class="sxs-lookup"><span data-stu-id="e5ebc-138">From _funcptr\_type_ `F0` to another _funcptr\_type_ `F1`, provided all of the following are true:</span></span>
    - <span data-ttu-id="e5ebc-139">`F0`和`F1`具有相同的参数数，并且 中的每个`D0n``F0`参数具有相同的`ref`、`out`或`in`修改器与 中的`D1n``F1`相应参数相同。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-139">`F0` and `F1` have the same number of parameters, and each parameter `D0n` in `F0` has the same `ref`, `out`, or `in` modifiers as the corresponding parameter `D1n` in `F1`.</span></span>
    - <span data-ttu-id="e5ebc-140">对于每个值`ref`参数（没有 的`out`参数， 或`in`修改器），标识转换、隐式引用转换或隐式指针转换从 中的`F0`参数类型到 中的`F1`相应参数类型存在。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-140">For each value parameter (a parameter with no `ref`, `out`, or `in` modifier), an identity conversion, implicit reference conversion, or implicit pointer conversion exists from the parameter type in `F0` to the corresponding parameter type in `F1`.</span></span>
    - <span data-ttu-id="e5ebc-141">对于每个`ref` `out`，`in`或 参数 中`F0`参数类型与 中的`F1`相应参数类型相同。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-141">For each `ref`, `out`, or `in` parameter, the parameter type in `F0` is the same as the corresponding parameter type in `F1`.</span></span>
    - <span data-ttu-id="e5ebc-142">如果返回类型按值（`ref`否或`ref readonly`），标识、隐式引用或隐式指针转换从 返回`F1`类型到`F0`返回类型存在。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-142">If the return type is by value (no `ref` or `ref readonly`), an identity, implicit reference, or implicit pointer conversion exists from the return type of `F1` to the return type of `F0`.</span></span>
    - <span data-ttu-id="e5ebc-143">`ref`如果返回类型是引用 （ 或`ref readonly`），则 返回`ref``F1`类型和修饰符与`ref``F0`的返回类型和修饰符相同。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-143">If the return type is by reference (`ref` or `ref readonly`), the return type and `ref` modifiers of `F1` are the same as the return type and `ref` modifiers of `F0`.</span></span>
    - <span data-ttu-id="e5ebc-144">的`F0`调用约定与 的`F1`调用约定相同。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-144">The calling convention of `F0` is the same as the calling convention of `F1`.</span></span>

### <a name="allow-address-of-to-target-methods"></a><span data-ttu-id="e5ebc-145">允许对目标方法的地址</span><span class="sxs-lookup"><span data-stu-id="e5ebc-145">Allow address-of to target methods</span></span>

<span data-ttu-id="e5ebc-146">方法组现在将被允许作为表达式地址的参数。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-146">Method groups will now be allowed as arguments to an address-of expression.</span></span> <span data-ttu-id="e5ebc-147">此类表达式的类型将是`delegate*`具有目标方法和托管调用约定的等效签名的 表达式的类型：</span><span class="sxs-lookup"><span data-stu-id="e5ebc-147">The type of such an expression will be a `delegate*` which has the equivalent signature of the target method and a managed calling convention:</span></span>

``` csharp
unsafe class Util {
    public static void Log() { }

    void Use() {
        delegate*<void> ptr1 = &Util.Log;

        // Error: type "delegate*<void>" not compatible with "delegate*<int>";
        delegate*<int> ptr2 = &Util.Log;

        // Okay. Conversion to void* is always allowed.
        void* v = &Util.Log;
   }
}
```

<span data-ttu-id="e5ebc-148">在不安全的上下文中，如果以下所有`M`方法都为 true，则`F`方法与函数指针类型兼容：</span><span class="sxs-lookup"><span data-stu-id="e5ebc-148">In an unsafe context, a method `M` is compatible with a function pointer type `F` if all of the following are true:</span></span>
- <span data-ttu-id="e5ebc-149">`M`和`F`具有相同的参数数，并且 中的每个`D`参数具有相同的`ref`、`out`或`in`修改器与 中的`F`相应参数相同。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-149">`M` and `F` have the same number of parameters, and each parameter in `D` has the same `ref`, `out`, or `in` modifiers as the corresponding parameter in `F`.</span></span>
- <span data-ttu-id="e5ebc-150">对于每个值`ref`参数（没有 的`out`参数， 或`in`修改器），标识转换、隐式引用转换或隐式指针转换从 中的`M`参数类型到 中的`F`相应参数类型存在。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-150">For each value parameter (a parameter with no `ref`, `out`, or `in` modifier), an identity conversion, implicit reference conversion, or implicit pointer conversion exists from the parameter type in `M` to the corresponding parameter type in `F`.</span></span>
- <span data-ttu-id="e5ebc-151">对于每个`ref` `out`，`in`或 参数 中`M`参数类型与 中的`F`相应参数类型相同。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-151">For each `ref`, `out`, or `in` parameter, the parameter type in `M` is the same as the corresponding parameter type in `F`.</span></span>
- <span data-ttu-id="e5ebc-152">如果返回类型按值（`ref`否或`ref readonly`），标识、隐式引用或隐式指针转换从 返回`F`类型到`M`返回类型存在。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-152">If the return type is by value (no `ref` or `ref readonly`), an identity, implicit reference, or implicit pointer conversion exists from the return type of `F` to the return type of `M`.</span></span>
- <span data-ttu-id="e5ebc-153">`ref`如果返回类型是引用 （ 或`ref readonly`），则 返回`ref``F`类型和修饰符与`ref``M`的返回类型和修饰符相同。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-153">If the return type is by reference (`ref` or `ref readonly`), the return type and `ref` modifiers of `F` are the same as the return type and `ref` modifiers of `M`.</span></span>
- <span data-ttu-id="e5ebc-154">的`M`调用约定与 的`F`调用约定相同。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-154">The calling convention of `M` is the same as the calling convention of `F`.</span></span>
- <span data-ttu-id="e5ebc-155">`M` 是静态方法。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-155">`M` is a static method.</span></span>

<span data-ttu-id="e5ebc-156">在不安全的`E`上下文中，隐式转换从目标为方法组到兼容函数指针类型的`F`表达式存在，如果`E`包含至少一个以正常形式适用于使用 参数类型和修饰符构建的`F`参数列表的方法，如下文所述。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-156">In an unsafe context, an implicit conversion exists from an address-of expression whose target is a method group `E` to a compatible function pointer type `F` if `E` contains at least one method that is applicable in its normal form to an argument list constructed by use of the parameter types and modifiers of `F`, as described in the following.</span></span>
- <span data-ttu-id="e5ebc-157">选择与窗体`M``E(A)`的方法调用对应的单个方法，并进行以下修改：</span><span class="sxs-lookup"><span data-stu-id="e5ebc-157">A single method `M` is selected corresponding to a method invocation of the form `E(A)` with the following modifications:</span></span>
   - <span data-ttu-id="e5ebc-158">参数`A`列表是表达式的列表，每个表达式都归类为变量，并且具有`ref``out``in``D`相应的_正式\_参数\_列表_的类型和修饰符 （、或 ）。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-158">The arguments list `A` is a list of expressions, each classified as a variable and with the type and modifier (`ref`, `out`, or `in`) of the corresponding _formal\_parameter\_list_ of `D`.</span></span>
   - <span data-ttu-id="e5ebc-159">候选方法只是以正常形式适用的方法，而不是以扩展形式适用的方法。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-159">The candidate methods are only those methods that are applicable in their normal form, not those applicable in their expanded form.</span></span>
   - <span data-ttu-id="e5ebc-160">候选方法仅是静态方法。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-160">The candidate methods are only those methods that are static.</span></span>
- <span data-ttu-id="e5ebc-161">如果方法调用算法生成错误，则会发生编译时间错误。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-161">If the algorithm of Method invocations produces an error, then a compile-time error occurs.</span></span> <span data-ttu-id="e5ebc-162">否则，算法将生成具有相同参数数的`M`单个最佳方法`F`，并且转换被视为存在。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-162">Otherwise, the algorithm produces a single best method `M` having the same number of parameters as `F` and the conversion is considered to exist.</span></span>
- <span data-ttu-id="e5ebc-163">所选方法`M`必须与函数指针类型`F`兼容（如上所述）。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-163">The selected method `M` must be compatible (as defined above) with the function pointer type `F`.</span></span> <span data-ttu-id="e5ebc-164">否则，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-164">Otherwise, a compile-time error occurs.</span></span>
- <span data-ttu-id="e5ebc-165">转换的结果是类型的`F`函数指针。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-165">The result of the conversion is a function pointer of type `F`.</span></span>

<span data-ttu-id="e5ebc-166">隐式转换从目标为方法`E`组的表达式中存在，`void*`如果 中`M``E`只有一个静态方法。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-166">An implicit conversion exists from an address-of expression whose target is a method group `E` to `void*` if there is only one static method `M` in `E`.</span></span>
<span data-ttu-id="e5ebc-167">如果有一个静态方法，则 中的`E`单个最佳方法为`M`。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-167">If there is one static method, then the single best method from `E` is `M`.</span></span>
<span data-ttu-id="e5ebc-168">否则，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-168">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="e5ebc-169">这意味着开发人员可以依赖重载解析规则与运营商的地址一起使用：</span><span class="sxs-lookup"><span data-stu-id="e5ebc-169">This means developers can depend on overload resolution rules to work in conjunction with the address-of operator:</span></span>

``` csharp
unsafe class Util {
    public static void Log() { }
    public static void Log(string p1) { }
    public static void Log(int i) { };

    void Use() {
        delegate*<void> a1 = &Log; // Log()
        delegate*<int, void> a2 = &Log; // Log(int i)

        // Error: ambiguous conversion from method group Log to "void*"
        void* v = &Log;
    }
```

<span data-ttu-id="e5ebc-170">将使用`ldftn`该指令实现运算符的地址。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-170">The address-of operator will be implemented using the `ldftn` instruction.</span></span>

<span data-ttu-id="e5ebc-171">此功能的限制：</span><span class="sxs-lookup"><span data-stu-id="e5ebc-171">Restrictions of this feature:</span></span>

- <span data-ttu-id="e5ebc-172">仅适用于标记为`static`的方法。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-172">Only applies to methods marked as `static`.</span></span>
- <span data-ttu-id="e5ebc-173">非`static`本地函数不能在 中使用`&`。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-173">Non-`static` local functions cannot be used in `&`.</span></span> <span data-ttu-id="e5ebc-174">这些方法的实现细节是故意不由语言指定的。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-174">The implementation details of these methods are deliberately not specified by the language.</span></span> <span data-ttu-id="e5ebc-175">这包括它们是静态的还是实例的，或者它们发出的签名的确切内容。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-175">This includes whether they are static vs. instance or exactly what signature they are emitted with.</span></span>


### <a name="operators-on-function-pointer-types"></a><span data-ttu-id="e5ebc-176">函数指针类型的运算符</span><span class="sxs-lookup"><span data-stu-id="e5ebc-176">Operators on Function Pointer Types</span></span>

<span data-ttu-id="e5ebc-177">对运算符的不安全代码中的部分进行了修改：</span><span class="sxs-lookup"><span data-stu-id="e5ebc-177">The section in unsafe code on operators is modified as such:</span></span>

> <span data-ttu-id="e5ebc-178">在不安全的上下文中，可以使用多个构造在所有_pointertype_s\_上操作，这些构造不_funcptrtype_s：\_</span><span class="sxs-lookup"><span data-stu-id="e5ebc-178">In an unsafe context, several constructs are available for operating on all _pointer\_type_s that are not _funcptr\_type_s:</span></span>
>
> *  <span data-ttu-id="e5ebc-179">运算符`*`可用于执行指针间接 （[指针间接](unsafe-code.md#pointer-indirection)）。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-179">The `*` operator may be used to perform pointer indirection ([Pointer indirection](unsafe-code.md#pointer-indirection)).</span></span>
> *  <span data-ttu-id="e5ebc-180">运算符`->`可用于通过指针（[指针成员访问](unsafe-code.md#pointer-member-access)）访问结构的成员。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-180">The `->` operator may be used to access a member of a struct through a pointer ([Pointer member access](unsafe-code.md#pointer-member-access)).</span></span>
> *  <span data-ttu-id="e5ebc-181">运算符`[]`可用于索引指针 （[指针元素访问](unsafe-code.md#pointer-element-access)）。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-181">The `[]` operator may be used to index a pointer ([Pointer element access](unsafe-code.md#pointer-element-access)).</span></span>
> *  <span data-ttu-id="e5ebc-182">运算符`&`可用于获取变量的地址（[运算符的地址](unsafe-code.md#the-address-of-operator)）。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-182">The `&` operator may be used to obtain the address of a variable ([The address-of operator](unsafe-code.md#the-address-of-operator)).</span></span>
> *  <span data-ttu-id="e5ebc-183">和 运算符可用于增量和递减指针（[指针增量和递减）。](unsafe-code.md#pointer-increment-and-decrement) `++` `--`</span><span class="sxs-lookup"><span data-stu-id="e5ebc-183">The `++` and `--` operators may be used to increment and decrement pointers ([Pointer increment and decrement](unsafe-code.md#pointer-increment-and-decrement)).</span></span>
> *  <span data-ttu-id="e5ebc-184">`+`和`-`运算符可用于执行指针算术 （[指针算术](unsafe-code.md#pointer-arithmetic)）。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-184">The `+` and `-` operators may be used to perform pointer arithmetic ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span>
> *  <span data-ttu-id="e5ebc-185">`==` `!=`、、、、`>``<``<=`和`=>`运算符可用于比较指针（[指针比较](unsafe-code.md#pointer-comparison)）。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-185">The `==`, `!=`, `<`, `>`, `<=`, and `=>` operators may be used to compare pointers ([Pointer comparison](unsafe-code.md#pointer-comparison)).</span></span>
> *  <span data-ttu-id="e5ebc-186">运算符`stackalloc`可用于从调用堆栈（[固定大小缓冲区](unsafe-code.md#fixed-size-buffers)）分配内存。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-186">The `stackalloc` operator may be used to allocate memory from the call stack ([Fixed size buffers](unsafe-code.md#fixed-size-buffers)).</span></span>
> *  <span data-ttu-id="e5ebc-187">语句`fixed`可用于临时修复变量，以便可以获取其地址 （[固定语句](unsafe-code.md#the-fixed-statement)）。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-187">The `fixed` statement may be used to temporarily fix a variable so its address can be obtained ([The fixed statement](unsafe-code.md#the-fixed-statement)).</span></span>
> 
> <span data-ttu-id="e5ebc-188">在不安全的上下文中，可以使用多个构造在所有_funcptrtype_s\_上运行：</span><span class="sxs-lookup"><span data-stu-id="e5ebc-188">In an unsafe context, several constructs are available for operating on all _funcptr\_type_s:</span></span>
> *  <span data-ttu-id="e5ebc-189">运算符`&`可用于获取静态方法的地址（[允许对目标方法的地址](function-pointers.md#allow-address-of-to-target-methods)）</span><span class="sxs-lookup"><span data-stu-id="e5ebc-189">The `&` operator may be used to obtain the address of static methods ([Allow address-of to target methods](function-pointers.md#allow-address-of-to-target-methods))</span></span>
> *  <span data-ttu-id="e5ebc-190">`==` `!=`、、、、`>``<``<=`和`=>`运算符可用于比较指针（[指针比较](unsafe-code.md#pointer-comparison)）。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-190">The `==`, `!=`, `<`, `>`, `<=`, and `=>` operators may be used to compare pointers ([Pointer comparison](unsafe-code.md#pointer-comparison)).</span></span>

<span data-ttu-id="e5ebc-191">此外，我们修改中的所有`Pointers in expressions`节以禁止函数指针类型，除 和`Pointer comparison``The sizeof operator`。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-191">Additionally, we modify all the sections in `Pointers in expressions` to forbid function pointer types, except `Pointer comparison` and `The sizeof operator`.</span></span>

### <a name="better-function-member"></a><span data-ttu-id="e5ebc-192">更好的功能成员</span><span class="sxs-lookup"><span data-stu-id="e5ebc-192">Better function member</span></span>

<span data-ttu-id="e5ebc-193">更好的功能成员规范将更改为包括以下行：</span><span class="sxs-lookup"><span data-stu-id="e5ebc-193">The better function member specification will be changed to include the following line:</span></span>

> <span data-ttu-id="e5ebc-194">A`delegate*`比`void*`</span><span class="sxs-lookup"><span data-stu-id="e5ebc-194">A `delegate*` is more specific than `void*`</span></span>

<span data-ttu-id="e5ebc-195">这意味着可以重载`void*`和 和 仍然`delegate*`明智地使用运算符的地址。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-195">This means that it is possible to overload on `void*` and a `delegate*` and still sensibly use the address-of operator.</span></span>

## <a name="metadata-representation-of-in-out-and-ref-readonly-parameters-and-return-types"></a><span data-ttu-id="e5ebc-196">的元数据表示形式，`ref readonly`和 参数和返回类型`out``in`</span><span class="sxs-lookup"><span data-stu-id="e5ebc-196">Metadata representation of `in`, `out`, and `ref readonly` parameters and return types</span></span>

<span data-ttu-id="e5ebc-197">函数指针签名没有参数标志位置，因此我们必须编码参数和返回类型是`in`，`out`还是`ref readonly`使用 modreqs。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-197">Function pointer signatures have no parameter flags location, so we must encode whether parameters and the return type are `in`, `out`, or `ref readonly` by using modreqs.</span></span>

### `in`

<span data-ttu-id="e5ebc-198">我们重用`System.Runtime.InteropServices.InAttribute`，作为`modreq`引用指定器应用于参数或返回类型，表示以下内容：</span><span class="sxs-lookup"><span data-stu-id="e5ebc-198">We reuse `System.Runtime.InteropServices.InAttribute`, applied as a `modreq` to the ref specifier on a parameter or return type, to mean the following:</span></span>
* <span data-ttu-id="e5ebc-199">如果应用于参数引用指定器，则此参数将被视为`in`。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-199">If applied to a parameter ref specifier, this parameter is treated as `in`.</span></span>
* <span data-ttu-id="e5ebc-200">如果应用于返回类型 ref 指定器，则返回类型将被视为`ref readonly`。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-200">If applied to the return type ref specifier, the return type is treated as `ref readonly`.</span></span>

### `out`

<span data-ttu-id="e5ebc-201">我们使用`System.Runtime.InteropServices.OutAttribute`、作为`modreq`应用于参数类型的 ref 指定器，表示参数是`out`参数。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-201">We use `System.Runtime.InteropServices.OutAttribute`, applied as a `modreq` to the ref specifier on a parameter type, to mean that the parameter is an `out` parameter.</span></span>

### <a name="errors"></a><span data-ttu-id="e5ebc-202">错误</span><span class="sxs-lookup"><span data-stu-id="e5ebc-202">Errors</span></span>

* <span data-ttu-id="e5ebc-203">将 modreq`OutAttribute`应用于返回类型是错误的。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-203">It is an error to apply `OutAttribute` as a modreq to a return type.</span></span>
* <span data-ttu-id="e5ebc-204">将两者`InAttribute`以及`OutAttribute`作为 modreq 应用于参数类型是错误的。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-204">It is an error to apply both `InAttribute` and `OutAttribute` as a modreq to a parameter type.</span></span>
* <span data-ttu-id="e5ebc-205">如果通过 modopt 指定任一，则忽略它们。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-205">If either are specified via modopt, they are ignored.</span></span>

## <a name="open-issues"></a><span data-ttu-id="e5ebc-206">Open Issues</span><span class="sxs-lookup"><span data-stu-id="e5ebc-206">Open Issues</span></span>

### <a name="nativecallableattribute"></a><span data-ttu-id="e5ebc-207">本机可调用属性</span><span class="sxs-lookup"><span data-stu-id="e5ebc-207">NativeCallableAttribute</span></span>

<span data-ttu-id="e5ebc-208">这是 CLR 用于避免调用时托管到本机序言的属性。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-208">This is an attribute used by the CLR to avoid the managed to native prologue when invoking.</span></span> <span data-ttu-id="e5ebc-209">使用此属性标记的方法只能从本机代码调用，不受托管（无法调用方法、创建委托等）。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-209">Methods marked by this attribute are only callable from native code, not managed (can’t call methods, create a delegate, etc …).</span></span> <span data-ttu-id="e5ebc-210">该属性对 mscorlib 不特殊;因此，该属性对 mscorlib 不一应有。运行时将使用此名称处理具有相同语义的任何属性。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-210">The attribute is not special to mscorlib; the runtime will treat any attribute with this name with the same semantics.</span></span>

<span data-ttu-id="e5ebc-211">运行时和语言可以协同工作以完全支持这一点。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-211">It's possible for the runtime and language to work together to fully support this.</span></span> <span data-ttu-id="e5ebc-212">该语言可以选择使用属性`static``NativeCallable`将成员的地址视为`delegate*`具有指定调用约定的成员的地址。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-212">The language could choose to treat address-of `static` members with a `NativeCallable` attribute as a `delegate*` with the specified calling convention.</span></span>

``` csharp
unsafe class NativeCallableExample {
    [NativeCallable(CallingConvention.CDecl)]
    static void CloseHandle(IntPtr p) => Marshal.FreeHGlobal(p);

    void Use() {
        delegate*<IntPtr, void> p1 = &CloseHandle; // Error: Invalid calling convention

        delegate* cdecl<IntPtr, void> p2 = &CloseHandle; // Okay
    }
}

```

<span data-ttu-id="e5ebc-213">此外，语言可能还希望：</span><span class="sxs-lookup"><span data-stu-id="e5ebc-213">Additionally the language would likely also want to:</span></span>

- <span data-ttu-id="e5ebc-214">标记对标记为`NativeCallable`错误的方法的任何托管调用。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-214">Flag any managed calls to a method tagged with `NativeCallable` as an error.</span></span> <span data-ttu-id="e5ebc-215">给定无法从托管代码调用函数，编译器应阻止开发人员尝试此类调用。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-215">Given the function can't be invoked from managed code the compiler should prevent developers from attempting such an invocation.</span></span>
- <span data-ttu-id="e5ebc-216">防止方法组转换到`delegate`使用`NativeCallable`标记 方法时。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-216">Prevent method group conversions to `delegate` when the method is tagged with `NativeCallable`.</span></span>

<span data-ttu-id="e5ebc-217">不过，这没有必要支持`NativeCallable`。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-217">This is not necessary to support `NativeCallable` though.</span></span> <span data-ttu-id="e5ebc-218">编译器可以像使用现有`NativeCallable`语法一样支持该属性。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-218">The compiler can support the `NativeCallable` attribute as is using the existing syntax.</span></span> <span data-ttu-id="e5ebc-219">在转换为正确的`delegate*`签名之前，程序只需`void*`强制转换为。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-219">The program would simply need to cast to `void*` before casting to the correct `delegate*` signature.</span></span> <span data-ttu-id="e5ebc-220">这不会比今天的支持更糟糕。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-220">That would be no worse than the support today.</span></span>

``` csharp
void* v = &CloseHandle;
delegate* cdecl<IntPtr, bool> f1 = (delegate* cdecl<IntPtr, bool>)v;
```

### <a name="extensible-set-of-unmanaged-calling-conventions"></a><span data-ttu-id="e5ebc-221">可扩展的一组非托管调用约定</span><span class="sxs-lookup"><span data-stu-id="e5ebc-221">Extensible set of unmanaged calling conventions</span></span>

<span data-ttu-id="e5ebc-222">当前 ECMA-335 编码支持的一组非托管调用约定已过时。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-222">The set of unmanaged calling conventions supported by the current ECMA-335 encodings is outdated.</span></span> <span data-ttu-id="e5ebc-223">我们已经看到请求添加对更多非托管调用约定的支持，例如：</span><span class="sxs-lookup"><span data-stu-id="e5ebc-223">We have seen requests to add support for more unmanaged calling conventions, for example:</span></span>

- <span data-ttu-id="e5ebc-224">[矢量调用](https://docs.microsoft.com/cpp/cpp/vectorcall)https://github.com/dotnet/coreclr/issues/12120</span><span class="sxs-lookup"><span data-stu-id="e5ebc-224">[vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall) https://github.com/dotnet/coreclr/issues/12120</span></span>
- <span data-ttu-id="e5ebc-225">StdCall 与显式此https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750</span><span class="sxs-lookup"><span data-stu-id="e5ebc-225">StdCall with explicit this https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750</span></span>

<span data-ttu-id="e5ebc-226">此功能的设计应允许将来根据需要扩展一组非托管调用约定。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-226">The design of this feature should allow extending the set of unmanaged calling conventions as needed in future.</span></span> <span data-ttu-id="e5ebc-227">这些问题包括编码调用约定的空间有限（16 个值中有 12 个在`IMAGE_CEE_CS_CALLCONV_MASK`），以及需要触摸的位置数以添加新调用约定。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-227">The problems include limited space for encoding calling conventions (12 out of 16 values are taken in `IMAGE_CEE_CS_CALLCONV_MASK`) and number of places that need to be touched in order to add a new calling convention.</span></span> <span data-ttu-id="e5ebc-228">一个可能的解决方案是引入一[`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention)种使用枚举表示调用约定的新编码。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-228">A potential solution is to introduce a new encoding that represents the calling convention using [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) enum.</span></span>

<span data-ttu-id="e5ebc-229">作为参考，https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h有 LLVM 支持的调用约定列表。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-229">For reference, https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h has the list of calling conventions supported by LLVM.</span></span> <span data-ttu-id="e5ebc-230">虽然 .NET 不太可能需要支持所有这些，但它表明调用约定的空间非常丰富。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-230">While it is unlikely that .NET will ever need to support all of them, it demonstrates that the space of calling conventions is very rich.</span></span>

## <a name="considerations"></a><span data-ttu-id="e5ebc-231">注意事项</span><span class="sxs-lookup"><span data-stu-id="e5ebc-231">Considerations</span></span>

### <a name="allow-instance-methods"></a><span data-ttu-id="e5ebc-232">允许实例方法</span><span class="sxs-lookup"><span data-stu-id="e5ebc-232">Allow instance methods</span></span>

<span data-ttu-id="e5ebc-233">该建议可以通过利用`EXPLICITTHIS`CLI 调用约定（在 C# 代码中命名`instance`）扩展以支持实例方法。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-233">The proposal could be extended to support instance methods by taking advantage of the `EXPLICITTHIS` CLI calling convention (named `instance` in C# code).</span></span> <span data-ttu-id="e5ebc-234">CLI 函数指针的这种形式将`this`参数作为函数指针语法的显式第一个参数。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-234">This form of CLI function pointers puts the `this` parameter as an explicit first parameter of the function pointer syntax.</span></span>

``` csharp
unsafe class Instance {
    void Use() {
        delegate* instance<Instance, string> f = &ToString;
        f(this);
    }
}
```

<span data-ttu-id="e5ebc-235">这是合理的，但增加了一些复杂的建议。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-235">This is sound but adds some complication to the proposal.</span></span> <span data-ttu-id="e5ebc-236">特别是因为函数指针因调用约定`instance`而异，即使`managed`这两种情况都用于使用相同的 C# 签名调用托管方法，该指针也是不兼容的。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-236">Particularly because function pointers which differed by the calling convention `instance` and `managed` would be incompatible even though both cases are used to invoke managed methods with the same C# signature.</span></span> <span data-ttu-id="e5ebc-237">此外，在每种情况下，考虑到这样做是有价值的，有一个简单的工作围绕：使用`static`本地函数。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-237">Also in every case considered where this would be valuable to have there was a simple work around: use a `static` local function.</span></span>

``` csharp
unsafe class Instance {
    void Use() {
        static string toString(Instance i) = i.ToString();
        delgate*<Instance, string> f = &toString;
        f(this);
    }
}
```

### <a name="dont-require-unsafe-at-declaration"></a><span data-ttu-id="e5ebc-238">在申报时不需要不安全</span><span class="sxs-lookup"><span data-stu-id="e5ebc-238">Don't require unsafe at declaration</span></span>

<span data-ttu-id="e5ebc-239">每次使用`unsafe``delegate*`时都不需要，而是在方法组转换为`delegate*`的点要求它。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-239">Instead of requiring `unsafe` at every use of a `delegate*`, only require it at the point where a method group is converted to a `delegate*`.</span></span> <span data-ttu-id="e5ebc-240">这是核心安全问题发挥作用的地方（知道在值处于活动状态时无法卸载包含的程序集）。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-240">This is where the core safety issues come into play (knowing that the containing assembly cannot be unloaded while the value is alive).</span></span> <span data-ttu-id="e5ebc-241">对其他`unsafe`位置要求可能被视为过高。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-241">Requiring `unsafe` on the other locations can be seen as excessive.</span></span>

<span data-ttu-id="e5ebc-242">这就是设计的初衷。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-242">This is how the design was originally intended.</span></span> <span data-ttu-id="e5ebc-243">但由此产生的语言规则感到很尴尬。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-243">But the resulting language rules felt very awkward.</span></span> <span data-ttu-id="e5ebc-244">这是一个指针值，即使没有`unsafe`关键字，它也会不断偷看这一事实， 这是不可能掩盖的。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-244">It's impossible to hide the fact that this is a pointer value and it kept peeking through even without the `unsafe` keyword.</span></span> <span data-ttu-id="e5ebc-245">例如，不能允许转换为`object`，它不能是`class`的成员，等等...C# 设计是所有指针使用`unsafe`的要求，因此此设计遵循此设计。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-245">For example the conversion to `object` can't be allowed, it can't be a member of a `class`, etc ... The C# design is to require `unsafe` for all pointer uses and hence this design follows that.</span></span>

<span data-ttu-id="e5ebc-246">开发人员仍然能够像今天对普通指针类型_safe_一样，在`delegate*`值之上呈现安全包装器。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-246">Developers will still be capable of presenting a _safe_ wrapper on top of `delegate*` values the same way that they do for normal pointer types today.</span></span> <span data-ttu-id="e5ebc-247">请考虑：</span><span class="sxs-lookup"><span data-stu-id="e5ebc-247">Consider:</span></span>

``` csharp
unsafe struct Action {
    delegate*<void> _ptr;

    Action(delegate*<void> ptr) => _ptr = ptr;
    public void Invoke() => _ptr();
}
```

### <a name="using-delegates"></a><span data-ttu-id="e5ebc-248">使用委托</span><span class="sxs-lookup"><span data-stu-id="e5ebc-248">Using delegates</span></span>

<span data-ttu-id="e5ebc-249">而不是使用新的语法元素，`delegate*`只需使用具有以下`delegate``*`类型的现有类型：</span><span class="sxs-lookup"><span data-stu-id="e5ebc-249">Instead of using a new syntax element, `delegate*`, simply use existing `delegate` types with a `*` following the type:</span></span>

``` csharp
Func<object, object, bool>* ptr = &object.ReferenceEquals;
```

<span data-ttu-id="e5ebc-250">处理调用约定可以通过使用指定`delegate``CallingConvention`值的属性对类型进行说明来完成。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-250">Handling calling convention can be done by annotating the `delegate` types with an attribute that specifies a `CallingConvention` value.</span></span> <span data-ttu-id="e5ebc-251">缺少属性表示托管调用约定。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-251">The lack of an attribute would signify the managed calling convention.</span></span>

<span data-ttu-id="e5ebc-252">在 IL 中编码此是有问题的。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-252">Encoding this in IL is problematic.</span></span> <span data-ttu-id="e5ebc-253">基础值需要表示为指针，但还必须：</span><span class="sxs-lookup"><span data-stu-id="e5ebc-253">The underlying value needs to be represented as a pointer yet it also must:</span></span>

1. <span data-ttu-id="e5ebc-254">具有唯一类型，允许具有不同函数指针类型的重载。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-254">Have a unique type to allow for overloads with different function pointer types.</span></span>
1. <span data-ttu-id="e5ebc-255">在装配边界上为 OHI 目的等效。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-255">Be equivalent for OHI purposes across assembly boundaries.</span></span>

<span data-ttu-id="e5ebc-256">最后一点特别成问题。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-256">The last point is particularly problematic.</span></span> <span data-ttu-id="e5ebc-257">这意味着使用的每个程序集`Func<int>*`都必须在元数据中对等效类型进行编码，即使`Func<int>*`是在程序集中定义的，尽管不控制。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-257">This mean that every assembly which uses `Func<int>*` must encode an equivalent type in metadata even though `Func<int>*` is defined in an assembly though don't control.</span></span>
<span data-ttu-id="e5ebc-258">此外，在程序集中使用名称`System.Func<T>`定义的不是 mscorlib 的任何其他类型必须不同于 mscorlib 中定义的版本。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-258">Additionally any other type which is defined with the name `System.Func<T>` in an assembly that is not mscorlib must be different than the version defined in mscorlib.</span></span>

<span data-ttu-id="e5ebc-259">被探讨的一个选项是发出这样的`mod_req(Func<int>) void*`指针，</span><span class="sxs-lookup"><span data-stu-id="e5ebc-259">One option that was explored was emitting such a pointer as `mod_req(Func<int>) void*`.</span></span> <span data-ttu-id="e5ebc-260">这不起作用，因为`mod_req`不能绑定到 ，`TypeSpec`因此不能以通用实例化为目标。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-260">This doesn't work though as a `mod_req` cannot bind to a `TypeSpec` and hence cannot target generic instantiations.</span></span>

### <a name="named-function-pointers"></a><span data-ttu-id="e5ebc-261">命名函数指针</span><span class="sxs-lookup"><span data-stu-id="e5ebc-261">Named function pointers</span></span>

<span data-ttu-id="e5ebc-262">函数指针语法可能很麻烦，尤其是在嵌套函数指针等复杂情况下。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-262">The function pointer syntax can be cumbersome, particularly in complex cases like nested function pointers.</span></span> <span data-ttu-id="e5ebc-263">而不是让开发人员键入签名，每次语言可以允许命名声明的函数指针，就像使用`delegate`一样。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-263">Rather than have developers type out the signature every time the language could allow for named declarations of function pointers as is done with `delegate`.</span></span>

``` csharp
func* void Action();

unsafe class NamedExample {
    void M(Action a) {
        a();
    }
}
```

<span data-ttu-id="e5ebc-264">此处的部分问题是底层 CLI 基元没有名称，因此这纯粹是 C# 发明，需要一些元数据工作才能启用。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-264">Part of the problem here is the underlying CLI primitive doesn't have names hence this would be purely a C# invention and require a bit of metadata work to enable.</span></span> <span data-ttu-id="e5ebc-265">这是可行的，但很重要的工作。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-265">That is doable but is a significant about of work.</span></span> <span data-ttu-id="e5ebc-266">它基本上要求 C# 具有仅针对这些名称的类型定义表的配套。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-266">It essentially requires C# to have a companion to the type def table purely for these names.</span></span>

<span data-ttu-id="e5ebc-267">此外，当检查命名函数指针的参数时，我们发现它们可以同样很好地应用于许多其他方案。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-267">Also when the arguments for named function pointers were examined we found they could apply equally well to a number of other scenarios.</span></span> <span data-ttu-id="e5ebc-268">例如，声明命名 tup 同样方便，以减少在所有情况下键入完整签名的需要。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-268">For example it would be just as convenient to declare named tuples to reduce the need to type out the full signature in all cases.</span></span>

``` csharp
(int x, int y) Point;

class NamedTupleExample {
    void M(Point p) {
        Console.WriteLine(p.x);
    }
}
```

<span data-ttu-id="e5ebc-269">经过讨论，我们决定不允许命名`delegate*`类型声明。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-269">After discussion we decided to not allow named declaration of `delegate*` types.</span></span> <span data-ttu-id="e5ebc-270">如果我们发现有基于客户使用情况反馈的重大需求，那么我们将调查一个命名解决方案，适用于函数指针，tups，泛型等...这在形式上可能与其他建议类似，如语言中的完全支持`typedef`。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-270">If we find there is significant need for this based on customer usage feedback then we will investigate a naming solution that works for function pointers, tuples, generics, etc ... This is likely to be similar in form to other suggestions like full `typedef` support in the language.</span></span>

## <a name="future-considerations"></a><span data-ttu-id="e5ebc-271">未来的注意事项</span><span class="sxs-lookup"><span data-stu-id="e5ebc-271">Future Considerations</span></span>

### <a name="static-local-functions"></a><span data-ttu-id="e5ebc-272">静态局部函数</span><span class="sxs-lookup"><span data-stu-id="e5ebc-272">static local functions</span></span>

<span data-ttu-id="e5ebc-273">这是指允许[the proposal](https://github.com/dotnet/csharplang/issues/1565)`static`修改器对局部函数进行的建议。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-273">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/1565) to allow the `static` modifier on local functions.</span></span> <span data-ttu-id="e5ebc-274">这种函数将保证在源代码中指定的确切签名`static`时发出。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-274">Such a function would be guaranteed to be emitted as `static` and with the exact signature specified in source code.</span></span> <span data-ttu-id="e5ebc-275">这样的函数应该是一个有效的参数，`&`因为它不包含任何问题，本地函数今天</span><span class="sxs-lookup"><span data-stu-id="e5ebc-275">Such a function should be a valid argument to `&` as it contains none of the problems local functions have today</span></span>

### <a name="static-delegates"></a><span data-ttu-id="e5ebc-276">静态委托</span><span class="sxs-lookup"><span data-stu-id="e5ebc-276">static delegates</span></span>

<span data-ttu-id="e5ebc-277">这是指允许声明只能引用`delegate``static`成员的类型[的建议](https://github.com/dotnet/csharplang/issues/302)。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-277">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/302) to allow for the declaration of `delegate` types which can only refer to `static` members.</span></span> <span data-ttu-id="e5ebc-278">优点是，此类`delegate`实例可以在性能敏感方案中自由分配，更好分配。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-278">The advantage being that such `delegate` instances can be allocation free and better in performance sensitive scenarios.</span></span>

<span data-ttu-id="e5ebc-279">如果实现了函数指针功能，`static delegate`则建议可能会关闭。该功能的建议优点是分配自由性质。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-279">If the function pointer feature is implemented the `static delegate` proposal will likely be closed out. The proposed advantage of that feature is the allocation free nature.</span></span> <span data-ttu-id="e5ebc-280">然而，最近的调查发现，由于装配卸载，无法实现这一目标。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-280">However recent investigations have found that is not possible to achieve due to assembly unloading.</span></span> <span data-ttu-id="e5ebc-281">必须有一个强句柄，从`static delegate`它引用的方法，以防止从它下卸载程序集。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-281">There must be a strong handle from the `static delegate` to the method it refers to in order to keep the assembly from being unloaded out from under it.</span></span>

<span data-ttu-id="e5ebc-282">为了维护每个`static delegate`实例，需要分配一个与建议目标背道而驰的新句柄。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-282">To maintain every `static delegate` instance would be required to allocate a new handle which runs counter to the goals of the proposal.</span></span> <span data-ttu-id="e5ebc-283">有些设计可以摊销到每个调用站点的单个分配，但这有点复杂，似乎不值得权衡。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-283">There were some designs where the allocation could be amortized to a single allocation per call-site but that was a bit complex and didn't seem worth the trade off.</span></span>

<span data-ttu-id="e5ebc-284">这意味着开发人员基本上必须决定以下权衡：</span><span class="sxs-lookup"><span data-stu-id="e5ebc-284">That means developers essentially have to decide between the following trade offs:</span></span>

1. <span data-ttu-id="e5ebc-285">面对装配体卸载时的安全性：这需要分配，因此`delegate`已经是一个足够的选择。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-285">Safety in the face of assembly unloading: this requires allocations and hence `delegate` is already a sufficient option.</span></span>
1. <span data-ttu-id="e5ebc-286">在装配卸载时没有安全性：使用`delegate*`。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-286">No safety in face of assembly unloading: use a `delegate*`.</span></span> <span data-ttu-id="e5ebc-287">这可以包装在 中`struct`，以允许在`unsafe`代码的其余部分中的上下文外部使用。</span><span class="sxs-lookup"><span data-stu-id="e5ebc-287">This can be wrapped in a `struct` to allow usage outside an `unsafe` context in the rest of the code.</span></span>
