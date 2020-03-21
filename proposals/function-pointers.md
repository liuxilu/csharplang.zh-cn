---
ms.openlocfilehash: 8bf3a18dc42e225e64bd3ccda2106aed89b421ed
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108384"
---
# <a name="function-pointers"></a><span data-ttu-id="7a88a-101">函数指针</span><span class="sxs-lookup"><span data-stu-id="7a88a-101">Function Pointers</span></span>

## <a name="summary"></a><span data-ttu-id="7a88a-102">总结</span><span class="sxs-lookup"><span data-stu-id="7a88a-102">Summary</span></span>

<span data-ttu-id="7a88a-103">此建议提供的语言构造可公开当前无法有效访问的 IL 操作码，或C#目前为： `ldftn` 和 `calli`。</span><span class="sxs-lookup"><span data-stu-id="7a88a-103">This proposal provides language constructs that expose IL opcodes that cannot currently be accessed efficiently, or at all, in C# today: `ldftn` and `calli`.</span></span> <span data-ttu-id="7a88a-104">这些 IL 操作码在高性能代码中非常重要，开发人员需要一种高效的访问方式。</span><span class="sxs-lookup"><span data-stu-id="7a88a-104">These IL opcodes can be important in high performance code and developers need an efficient way to access them.</span></span>

## <a name="motivation"></a><span data-ttu-id="7a88a-105">动机</span><span class="sxs-lookup"><span data-stu-id="7a88a-105">Motivation</span></span>

<span data-ttu-id="7a88a-106">以下问题中介绍了此功能的动机和背景（这是此功能的一个潜在实现方式）：</span><span class="sxs-lookup"><span data-stu-id="7a88a-106">The motivations and background for this feature are described in the following issue (as is a potential implementation of the feature):</span></span>

https://github.com/dotnet/csharplang/issues/191

<span data-ttu-id="7a88a-107">这是[编译器内部函数](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)的替代设计建议</span><span class="sxs-lookup"><span data-stu-id="7a88a-107">This is an alternate design proposal to [compiler intrinsics](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)</span></span>

## <a name="detailed-design"></a><span data-ttu-id="7a88a-108">详细设计</span><span class="sxs-lookup"><span data-stu-id="7a88a-108">Detailed Design</span></span>

### <a name="function-pointers"></a><span data-ttu-id="7a88a-109">函数指针</span><span class="sxs-lookup"><span data-stu-id="7a88a-109">Function pointers</span></span>

<span data-ttu-id="7a88a-110">该语言将允许使用 `delegate*` 语法来声明函数指针。</span><span class="sxs-lookup"><span data-stu-id="7a88a-110">The language will allow for the declaration of function pointers using the `delegate*` syntax.</span></span> <span data-ttu-id="7a88a-111">下一节中详细介绍了完整的语法，但它与 `Func` 和 `Action` 类型声明所使用的语法类似。</span><span class="sxs-lookup"><span data-stu-id="7a88a-111">The full syntax is described in detail in the next section but it is meant to resemble the syntax used by `Func` and `Action` type declarations.</span></span>

``` csharp
unsafe class Example {
    void Example(Action<int> a, delegate*<int, void> f) {
        a(42);
        f(42);
    }
}
```

<span data-ttu-id="7a88a-112">这些类型使用 ECMA-335 中所述的函数指针类型来表示。</span><span class="sxs-lookup"><span data-stu-id="7a88a-112">These types are represented using the function pointer type as outlined in ECMA-335.</span></span> <span data-ttu-id="7a88a-113">这意味着调用 `delegate*` 将使用 `calli`，在这种情况下，`delegate` 的调用将对 `Invoke` 方法使用 `callvirt`。</span><span class="sxs-lookup"><span data-stu-id="7a88a-113">This means invocation of a `delegate*` will use `calli` where invocation of a `delegate` will use `callvirt` on the `Invoke` method.</span></span>
<span data-ttu-id="7a88a-114">当然，对于这两种构造，调用是相同的。</span><span class="sxs-lookup"><span data-stu-id="7a88a-114">Syntactically though invocation is identical for both constructs.</span></span>

<span data-ttu-id="7a88a-115">方法指针的 ECMA-335 定义将调用约定包含为类型签名（第7.1 节）的一部分。</span><span class="sxs-lookup"><span data-stu-id="7a88a-115">The ECMA-335 definition of method pointers includes the calling convention as part of the type signature (section 7.1).</span></span>
<span data-ttu-id="7a88a-116">将 `managed`默认调用约定。</span><span class="sxs-lookup"><span data-stu-id="7a88a-116">The default calling convention will be `managed`.</span></span> <span data-ttu-id="7a88a-117">可以通过在 `delegate*` 语法之后添加适当的修饰符来指定替代形式： `managed`、`cdecl`、`stdcall`、`thiscall`或 `unmanaged`。</span><span class="sxs-lookup"><span data-stu-id="7a88a-117">Alternate forms can be specified by adding the appropriate modifier after the `delegate*` syntax: `managed`, `cdecl`, `stdcall`, `thiscall`, or `unmanaged`.</span></span> <span data-ttu-id="7a88a-118">示例：</span><span class="sxs-lookup"><span data-stu-id="7a88a-118">Example:</span></span>

``` csharp
// This method will be invoked using the cdecl calling convention
delegate* cdecl<int, int>;

// This method will be invoked using the stdcall calling convention
delegate* stdcall<int, int>;
```

<span data-ttu-id="7a88a-119">`delegate*` 类型之间的转换是根据其签名（包括调用约定）完成的。</span><span class="sxs-lookup"><span data-stu-id="7a88a-119">Conversions between `delegate*` types is done based on their signature including the calling convention.</span></span>

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

<span data-ttu-id="7a88a-120">`delegate*` 类型是指针类型，这意味着它具有标准指针类型的所有功能和限制：</span><span class="sxs-lookup"><span data-stu-id="7a88a-120">A `delegate*` type is a pointer type which means it has all of the capabilities and restrictions of a standard pointer type:</span></span>

- <span data-ttu-id="7a88a-121">仅在 `unsafe` 上下文中有效。</span><span class="sxs-lookup"><span data-stu-id="7a88a-121">Only valid in an `unsafe` context.</span></span>
- <span data-ttu-id="7a88a-122">只能从 `unsafe` 的上下文中调用包含 `delegate*` 参数或返回类型的方法。</span><span class="sxs-lookup"><span data-stu-id="7a88a-122">Methods which contain a `delegate*` parameter or return type can only be called from an `unsafe` context.</span></span>
- <span data-ttu-id="7a88a-123">无法转换为 `object`。</span><span class="sxs-lookup"><span data-stu-id="7a88a-123">Cannot be converted to `object`.</span></span>
- <span data-ttu-id="7a88a-124">不能用作泛型参数。</span><span class="sxs-lookup"><span data-stu-id="7a88a-124">Cannot be used as a generic argument.</span></span>
- <span data-ttu-id="7a88a-125">可以将 `delegate*` 隐式转换为 `void*`。</span><span class="sxs-lookup"><span data-stu-id="7a88a-125">Can implicitly convert `delegate*` to `void*`.</span></span>
- <span data-ttu-id="7a88a-126">可以从 `void*` 显式转换为 `delegate*`。</span><span class="sxs-lookup"><span data-stu-id="7a88a-126">Can explicitly convert from `void*` to `delegate*`.</span></span>

<span data-ttu-id="7a88a-127">限制：</span><span class="sxs-lookup"><span data-stu-id="7a88a-127">Restrictions:</span></span>

- <span data-ttu-id="7a88a-128">自定义特性不能应用于 `delegate*` 或它的任何元素。</span><span class="sxs-lookup"><span data-stu-id="7a88a-128">Custom attributes cannot be applied to a `delegate*` or any of its elements.</span></span>
- <span data-ttu-id="7a88a-129">不能将 `delegate*` 参数标记为 `params`</span><span class="sxs-lookup"><span data-stu-id="7a88a-129">A `delegate*` parameter cannot be marked as `params`</span></span>
- <span data-ttu-id="7a88a-130">`delegate*` 类型具有正常指针类型的所有限制。</span><span class="sxs-lookup"><span data-stu-id="7a88a-130">A `delegate*` type has all of the restrictions of a normal pointer type.</span></span>

### <a name="function-pointer-syntax"></a><span data-ttu-id="7a88a-131">函数指针语法</span><span class="sxs-lookup"><span data-stu-id="7a88a-131">Function pointer syntax</span></span>

<span data-ttu-id="7a88a-132">完整的函数指针语法由以下语法表示：</span><span class="sxs-lookup"><span data-stu-id="7a88a-132">The full function pointer syntax is represented by the following grammar:</span></span>

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

<span data-ttu-id="7a88a-133">`unmanaged` 调用约定表示当前平台上的本机代码的默认调用约定，并且编码为 winapi。</span><span class="sxs-lookup"><span data-stu-id="7a88a-133">The `unmanaged` calling convention represents the default calling convention for native code on the current platform, and is encoded as winapi.</span></span>
<span data-ttu-id="7a88a-134">所有 `calling_convention`都是上下文关键字，在 `delegate*`之前。</span><span class="sxs-lookup"><span data-stu-id="7a88a-134">All `calling_convention`s are contextual keywords when preceded by a `delegate*`.</span></span>

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

### <a name="function-pointer-conversions"></a><span data-ttu-id="7a88a-135">函数指针转换</span><span class="sxs-lookup"><span data-stu-id="7a88a-135">Function pointer conversions</span></span>

<span data-ttu-id="7a88a-136">在不安全的上下文中，可以使用隐式转换集（隐式转换）进行扩展，以包括以下隐式指针转换：</span><span class="sxs-lookup"><span data-stu-id="7a88a-136">In an unsafe context, the set of available implicit conversions (Implicit conversions) is extended to include the following implicit pointer conversions:</span></span>
- [<span data-ttu-id="7a88a-137">_现有转换_</span><span class="sxs-lookup"><span data-stu-id="7a88a-137">_Existing conversions_</span></span>](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#pointer-conversions)
- <span data-ttu-id="7a88a-138">如果满足以下所有条件，则从_funcptr\_类型_`F0` 到其他_funcptr\_类型_`F1`：</span><span class="sxs-lookup"><span data-stu-id="7a88a-138">From _funcptr\_type_ `F0` to another _funcptr\_type_ `F1`, provided all of the following are true:</span></span>
    - <span data-ttu-id="7a88a-139">`F0` 和 `F1` 具有相同数量的参数，并且 `F0` 中的每个参数 `D0n` 与 `ref`中的相应参数 `out`、`in` 或 `D1n` 修饰符相同。`F1`</span><span class="sxs-lookup"><span data-stu-id="7a88a-139">`F0` and `F1` have the same number of parameters, and each parameter `D0n` in `F0` has the same `ref`, `out`, or `in` modifiers as the corresponding parameter `D1n` in `F1`.</span></span>
    - <span data-ttu-id="7a88a-140">对于每个值参数（不带 `ref`、`out`或 `in` 修饰符的参数），从 `F0` 中的参数类型到 `F1`中的相应参数类型存在标识转换、隐式引用转换或隐式指针转换。</span><span class="sxs-lookup"><span data-stu-id="7a88a-140">For each value parameter (a parameter with no `ref`, `out`, or `in` modifier), an identity conversion, implicit reference conversion, or implicit pointer conversion exists from the parameter type in `F0` to the corresponding parameter type in `F1`.</span></span>
    - <span data-ttu-id="7a88a-141">对于每个 `ref`、`out`或 `in` 参数，`F0` 中的参数类型与 `F1`中的相应参数类型相同。</span><span class="sxs-lookup"><span data-stu-id="7a88a-141">For each `ref`, `out`, or `in` parameter, the parameter type in `F0` is the same as the corresponding parameter type in `F1`.</span></span>
    - <span data-ttu-id="7a88a-142">如果返回类型是 by 值（无 `ref` 或 `ref readonly`），则从 `F1` 的返回类型到 `F0`的返回类型存在标识、隐式引用或隐式指针转换。</span><span class="sxs-lookup"><span data-stu-id="7a88a-142">If the return type is by value (no `ref` or `ref readonly`), an identity, implicit reference, or implicit pointer conversion exists from the return type of `F1` to the return type of `F0`.</span></span>
    - <span data-ttu-id="7a88a-143">如果返回类型是按引用（`ref` 或 `ref readonly`），则 `F1` 的返回类型和 `ref` 修饰符与 `ref` 的返回类型和 `F0`修饰符相同。</span><span class="sxs-lookup"><span data-stu-id="7a88a-143">If the return type is by reference (`ref` or `ref readonly`), the return type and `ref` modifiers of `F1` are the same as the return type and `ref` modifiers of `F0`.</span></span>
    - <span data-ttu-id="7a88a-144">`F0` 的调用约定与 `F1`的调用约定相同。</span><span class="sxs-lookup"><span data-stu-id="7a88a-144">The calling convention of `F0` is the same as the calling convention of `F1`.</span></span>

### <a name="allow-address-of-to-target-methods"></a><span data-ttu-id="7a88a-145">允许目标方法的地址</span><span class="sxs-lookup"><span data-stu-id="7a88a-145">Allow address-of to target methods</span></span>

<span data-ttu-id="7a88a-146">现在将允许方法组作为表达式的参数。</span><span class="sxs-lookup"><span data-stu-id="7a88a-146">Method groups will now be allowed as arguments to an address-of expression.</span></span> <span data-ttu-id="7a88a-147">此类表达式的类型将为 `delegate*`，它具有目标方法的等效签名和托管调用约定：</span><span class="sxs-lookup"><span data-stu-id="7a88a-147">The type of such an expression will be a `delegate*` which has the equivalent signature of the target method and a managed calling convention:</span></span>

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

<span data-ttu-id="7a88a-148">在不安全的上下文中，如果满足以下所有条件，则方法 `M` 与函数指针类型兼容 `F`：</span><span class="sxs-lookup"><span data-stu-id="7a88a-148">In an unsafe context, a method `M` is compatible with a function pointer type `F` if all of the following are true:</span></span>
- <span data-ttu-id="7a88a-149">`M` 和 `F` 具有相同数量的参数，并且 `D` 中的每个参数都具有与 `out`中的相应参数相同的 `ref`、`in` 或 `F`修饰符。</span><span class="sxs-lookup"><span data-stu-id="7a88a-149">`M` and `F` have the same number of parameters, and each parameter in `D` has the same `ref`, `out`, or `in` modifiers as the corresponding parameter in `F`.</span></span>
- <span data-ttu-id="7a88a-150">对于每个值参数（不带 `ref`、`out`或 `in` 修饰符的参数），从 `M` 中的参数类型到 `F`中的相应参数类型存在标识转换、隐式引用转换或隐式指针转换。</span><span class="sxs-lookup"><span data-stu-id="7a88a-150">For each value parameter (a parameter with no `ref`, `out`, or `in` modifier), an identity conversion, implicit reference conversion, or implicit pointer conversion exists from the parameter type in `M` to the corresponding parameter type in `F`.</span></span>
- <span data-ttu-id="7a88a-151">对于每个 `ref`、`out`或 `in` 参数，`M` 中的参数类型与 `F`中的相应参数类型相同。</span><span class="sxs-lookup"><span data-stu-id="7a88a-151">For each `ref`, `out`, or `in` parameter, the parameter type in `M` is the same as the corresponding parameter type in `F`.</span></span>
- <span data-ttu-id="7a88a-152">如果返回类型是 by 值（无 `ref` 或 `ref readonly`），则从 `F` 的返回类型到 `M`的返回类型存在标识、隐式引用或隐式指针转换。</span><span class="sxs-lookup"><span data-stu-id="7a88a-152">If the return type is by value (no `ref` or `ref readonly`), an identity, implicit reference, or implicit pointer conversion exists from the return type of `F` to the return type of `M`.</span></span>
- <span data-ttu-id="7a88a-153">如果返回类型是按引用（`ref` 或 `ref readonly`），则 `F` 的返回类型和 `ref` 修饰符与 `ref` 的返回类型和 `M`修饰符相同。</span><span class="sxs-lookup"><span data-stu-id="7a88a-153">If the return type is by reference (`ref` or `ref readonly`), the return type and `ref` modifiers of `F` are the same as the return type and `ref` modifiers of `M`.</span></span>
- <span data-ttu-id="7a88a-154">`M` 的调用约定与 `F`的调用约定相同。</span><span class="sxs-lookup"><span data-stu-id="7a88a-154">The calling convention of `M` is the same as the calling convention of `F`.</span></span>
- <span data-ttu-id="7a88a-155">`M` 是一种静态方法。</span><span class="sxs-lookup"><span data-stu-id="7a88a-155">`M` is a static method.</span></span>

<span data-ttu-id="7a88a-156">在不安全的上下文中，从地址为的表达式中存在隐式转换，其目标为方法组 `E` 到兼容的函数指针类型 `F` 如果 `E` 至少包含一个方法，该方法适用于通过使用 `F`的参数类型和修饰符构建的参数列表，如下所述。</span><span class="sxs-lookup"><span data-stu-id="7a88a-156">In an unsafe context, an implicit conversion exists from an address-of expression whose target is a method group `E` to a compatible function pointer type `F` if `E` contains at least one method that is applicable in its normal form to an argument list constructed by use of the parameter types and modifiers of `F`, as described in the following.</span></span>
- <span data-ttu-id="7a88a-157">选择单个方法 `M` 与 `E(A)` 窗体的方法调用对应，并进行以下修改：</span><span class="sxs-lookup"><span data-stu-id="7a88a-157">A single method `M` is selected corresponding to a method invocation of the form `E(A)` with the following modifications:</span></span>
   - <span data-ttu-id="7a88a-158">参数列表 `A` 是一个表达式列表，每个表达式都归类为一个变量，并使用相应_正式\_参数_的类型和修饰符（`ref`、`out`或 `in`）\_`D`列表。</span><span class="sxs-lookup"><span data-stu-id="7a88a-158">The arguments list `A` is a list of expressions, each classified as a variable and with the type and modifier (`ref`, `out`, or `in`) of the corresponding _formal\_parameter\_list_ of `D`.</span></span>
   - <span data-ttu-id="7a88a-159">候选方法只是那些适用于其普通窗体的方法，而不是以其展开形式适用的方法。</span><span class="sxs-lookup"><span data-stu-id="7a88a-159">The candidate methods are only those methods that are applicable in their normal form, not those applicable in their expanded form.</span></span>
   - <span data-ttu-id="7a88a-160">候选方法仅为静态方法。</span><span class="sxs-lookup"><span data-stu-id="7a88a-160">The candidate methods are only those methods that are static.</span></span>
- <span data-ttu-id="7a88a-161">如果方法调用的算法产生错误，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="7a88a-161">If the algorithm of Method invocations produces an error, then a compile-time error occurs.</span></span> <span data-ttu-id="7a88a-162">否则，该算法将生成单个最佳方法 `M` 与 `F` 具有相同数量的参数，并将转换视为存在。</span><span class="sxs-lookup"><span data-stu-id="7a88a-162">Otherwise, the algorithm produces a single best method `M` having the same number of parameters as `F` and the conversion is considered to exist.</span></span>
- <span data-ttu-id="7a88a-163">所选方法 `M` 必须与函数指针类型兼容（如上定义） `F`。</span><span class="sxs-lookup"><span data-stu-id="7a88a-163">The selected method `M` must be compatible (as defined above) with the function pointer type `F`.</span></span> <span data-ttu-id="7a88a-164">否则，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="7a88a-164">Otherwise, a compile-time error occurs.</span></span>
- <span data-ttu-id="7a88a-165">转换的结果是 `F`类型的函数指针。</span><span class="sxs-lookup"><span data-stu-id="7a88a-165">The result of the conversion is a function pointer of type `F`.</span></span>

<span data-ttu-id="7a88a-166">如果 `E`中只有一个静态方法 `M`，则目标为方法组 `void*` `E` 的地址表达式中存在隐式转换。</span><span class="sxs-lookup"><span data-stu-id="7a88a-166">An implicit conversion exists from an address-of expression whose target is a method group `E` to `void*` if there is only one static method `M` in `E`.</span></span>
<span data-ttu-id="7a88a-167">如果有一个静态方法，则 `M``E` 的单个最佳方法。</span><span class="sxs-lookup"><span data-stu-id="7a88a-167">If there is one static method, then the single best method from `E` is `M`.</span></span>
<span data-ttu-id="7a88a-168">否则，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="7a88a-168">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="7a88a-169">这意味着开发人员可以依赖重载决策规则与地址运算符结合使用：</span><span class="sxs-lookup"><span data-stu-id="7a88a-169">This means developers can depend on overload resolution rules to work in conjunction with the address-of operator:</span></span>

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

<span data-ttu-id="7a88a-170">将使用 `ldftn` 指令来实现此地址的操作员。</span><span class="sxs-lookup"><span data-stu-id="7a88a-170">The address-of operator will be implemented using the `ldftn` instruction.</span></span>

<span data-ttu-id="7a88a-171">此功能的限制：</span><span class="sxs-lookup"><span data-stu-id="7a88a-171">Restrictions of this feature:</span></span>

- <span data-ttu-id="7a88a-172">仅适用于标记为 `static`的方法。</span><span class="sxs-lookup"><span data-stu-id="7a88a-172">Only applies to methods marked as `static`.</span></span>
- <span data-ttu-id="7a88a-173">不能在 `&`中使用非`static` 本地函数。</span><span class="sxs-lookup"><span data-stu-id="7a88a-173">Non-`static` local functions cannot be used in `&`.</span></span> <span data-ttu-id="7a88a-174">这些方法的实现细节特意不由语言指定。</span><span class="sxs-lookup"><span data-stu-id="7a88a-174">The implementation details of these methods are deliberately not specified by the language.</span></span> <span data-ttu-id="7a88a-175">这包括它们是静态的还是实例的，或者是否与它们一起发出的签名完全相同。</span><span class="sxs-lookup"><span data-stu-id="7a88a-175">This includes whether they are static vs. instance or exactly what signature they are emitted with.</span></span>

### <a name="better-function-member"></a><span data-ttu-id="7a88a-176">更好的函数成员</span><span class="sxs-lookup"><span data-stu-id="7a88a-176">Better function member</span></span>

<span data-ttu-id="7a88a-177">更好的函数成员规范将更改为包含以下行：</span><span class="sxs-lookup"><span data-stu-id="7a88a-177">The better function member specification will be changed to include the following line:</span></span>

> <span data-ttu-id="7a88a-178">`delegate*` 比 `void*` 更为具体</span><span class="sxs-lookup"><span data-stu-id="7a88a-178">A `delegate*` is more specific than `void*`</span></span>

<span data-ttu-id="7a88a-179">这意味着，可以在 `void*` 和 `delegate*` 上重载，但仍揭示使用地址运算符。</span><span class="sxs-lookup"><span data-stu-id="7a88a-179">This means that it is possible to overload on `void*` and a `delegate*` and still sensibly use the address-of operator.</span></span>

## <a name="open-issues"></a><span data-ttu-id="7a88a-180">打开的问题</span><span class="sxs-lookup"><span data-stu-id="7a88a-180">Open Issues</span></span>

### <a name="nativecallableattribute"></a><span data-ttu-id="7a88a-181">NativeCallableAttribute</span><span class="sxs-lookup"><span data-stu-id="7a88a-181">NativeCallableAttribute</span></span>

<span data-ttu-id="7a88a-182">这是 CLR 在调用时用于避免托管到本机序言的属性。</span><span class="sxs-lookup"><span data-stu-id="7a88a-182">This is an attribute used by the CLR to avoid the managed to native prologue when invoking.</span></span> <span data-ttu-id="7a88a-183">此特性标记的方法只能从本机代码中调用，而不能从托管（无法调用方法，创建委托，等等）中调用。</span><span class="sxs-lookup"><span data-stu-id="7a88a-183">Methods marked by this attribute are only callable from native code, not managed (can’t call methods, create a delegate, etc …).</span></span> <span data-ttu-id="7a88a-184">特性并非 mscorlib 专用;运行时会将具有此名称的任何属性视为相同的语义。</span><span class="sxs-lookup"><span data-stu-id="7a88a-184">The attribute is not special to mscorlib; the runtime will treat any attribute with this name with the same semantics.</span></span>

<span data-ttu-id="7a88a-185">运行时和语言可以协同工作以完全支持此操作。</span><span class="sxs-lookup"><span data-stu-id="7a88a-185">It's possible for the runtime and language to work together to fully support this.</span></span> <span data-ttu-id="7a88a-186">该语言可以选择将具有 `NativeCallable` 属性的 `static` 成员作为具有指定调用约定的 `delegate*` 进行处理。</span><span class="sxs-lookup"><span data-stu-id="7a88a-186">The language could choose to treat address-of `static` members with a `NativeCallable` attribute as a `delegate*` with the specified calling convention.</span></span>

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

<span data-ttu-id="7a88a-187">另外，该语言可能还需要：</span><span class="sxs-lookup"><span data-stu-id="7a88a-187">Additionally the language would likely also want to:</span></span>

- <span data-ttu-id="7a88a-188">将使用 `NativeCallable` 标记的方法的任何托管调用标记为错误。</span><span class="sxs-lookup"><span data-stu-id="7a88a-188">Flag any managed calls to a method tagged with `NativeCallable` as an error.</span></span> <span data-ttu-id="7a88a-189">如果无法从托管代码调用函数，编译器应会阻止开发人员尝试此类调用。</span><span class="sxs-lookup"><span data-stu-id="7a88a-189">Given the function can't be invoked from managed code the compiler should prevent developers from attempting such an invocation.</span></span>
- <span data-ttu-id="7a88a-190">当方法标记为 `NativeCallable`时，阻止方法组转换为 `delegate`。</span><span class="sxs-lookup"><span data-stu-id="7a88a-190">Prevent method group conversions to `delegate` when the method is tagged with `NativeCallable`.</span></span>

<span data-ttu-id="7a88a-191">不过，这并不是支持 `NativeCallable` 的必要条件。</span><span class="sxs-lookup"><span data-stu-id="7a88a-191">This is not necessary to support `NativeCallable` though.</span></span> <span data-ttu-id="7a88a-192">编译器可以像使用现有语法一样支持 `NativeCallable` 特性。</span><span class="sxs-lookup"><span data-stu-id="7a88a-192">The compiler can support the `NativeCallable` attribute as is using the existing syntax.</span></span> <span data-ttu-id="7a88a-193">在强制转换为正确的 `delegate*` 签名之前，程序只需强制转换为 `void*`。</span><span class="sxs-lookup"><span data-stu-id="7a88a-193">The program would simply need to cast to `void*` before casting to the correct `delegate*` signature.</span></span> <span data-ttu-id="7a88a-194">这并不比今天的支持更糟。</span><span class="sxs-lookup"><span data-stu-id="7a88a-194">That would be no worse than the support today.</span></span>

``` csharp
void* v = &CloseHandle;
delegate* cdecl<IntPtr, bool> f1 = (delegate* cdecl<IntPtr, bool>)v;
```

### <a name="extensible-set-of-unmanaged-calling-conventions"></a><span data-ttu-id="7a88a-195">可扩展的非托管调用约定集</span><span class="sxs-lookup"><span data-stu-id="7a88a-195">Extensible set of unmanaged calling conventions</span></span>

<span data-ttu-id="7a88a-196">当前 ECMA 335 编码支持的非托管调用约定集已过时。</span><span class="sxs-lookup"><span data-stu-id="7a88a-196">The set of unmanaged calling conventions supported by the current ECMA-335 encodings is outdated.</span></span> <span data-ttu-id="7a88a-197">我们已了解到添加对更多非托管调用约定的支持的请求，例如：</span><span class="sxs-lookup"><span data-stu-id="7a88a-197">We have seen requests to add support for more unmanaged calling conventions, for example:</span></span>

- <span data-ttu-id="7a88a-198">[vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall) https://github.com/dotnet/coreclr/issues/12120</span><span class="sxs-lookup"><span data-stu-id="7a88a-198">[vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall) https://github.com/dotnet/coreclr/issues/12120</span></span>
- <span data-ttu-id="7a88a-199">StdCall，此 https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750</span><span class="sxs-lookup"><span data-stu-id="7a88a-199">StdCall with explicit this https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750</span></span>

<span data-ttu-id="7a88a-200">此功能的设计应允许在将来根据需要扩展一组非托管调用约定。</span><span class="sxs-lookup"><span data-stu-id="7a88a-200">The design of this feature should allow extending the set of unmanaged calling conventions as needed in future.</span></span> <span data-ttu-id="7a88a-201">这些问题包括限制调用约定的有限空间（`IMAGE_CEE_CS_CALLCONV_MASK`中采用了16个值中的12个值）和需要接触的位置数，以便添加新的调用约定。</span><span class="sxs-lookup"><span data-stu-id="7a88a-201">The problems include limited space for encoding calling conventions (12 out of 16 values are taken in `IMAGE_CEE_CS_CALLCONV_MASK`) and number of places that need to be touched in order to add a new calling convention.</span></span> <span data-ttu-id="7a88a-202">一种可能的解决方案是使用[`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention)枚举引入表示调用约定的新编码。</span><span class="sxs-lookup"><span data-stu-id="7a88a-202">A potential solution is to introduce a new encoding that represents the calling convention using [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) enum.</span></span>

<span data-ttu-id="7a88a-203">https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h 提供了 LLVM 支持的调用约定列表作为参考。</span><span class="sxs-lookup"><span data-stu-id="7a88a-203">For reference, https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h has the list of calling conventions supported by LLVM.</span></span> <span data-ttu-id="7a88a-204">虽然 .NET 不太可能需要支持所有这些文件，但它也会表明调用约定的空间非常丰富。</span><span class="sxs-lookup"><span data-stu-id="7a88a-204">While it is unlikely that .NET will ever need to support all of them, it demonstrates that the space of calling conventions is very rich.</span></span>

## <a name="considerations"></a><span data-ttu-id="7a88a-205">注意事项</span><span class="sxs-lookup"><span data-stu-id="7a88a-205">Considerations</span></span>

### <a name="allow-instance-methods"></a><span data-ttu-id="7a88a-206">允许实例方法</span><span class="sxs-lookup"><span data-stu-id="7a88a-206">Allow instance methods</span></span>

<span data-ttu-id="7a88a-207">通过利用 `EXPLICITTHIS` CLI 调用约定（在代码中C#指定为 `instance`），可以将该建议扩展为支持实例方法。</span><span class="sxs-lookup"><span data-stu-id="7a88a-207">The proposal could be extended to support instance methods by taking advantage of the `EXPLICITTHIS` CLI calling convention (named `instance` in C# code).</span></span> <span data-ttu-id="7a88a-208">这种形式的 CLI 函数指针将 `this` 参数作为函数指针语法的显式第一个参数。</span><span class="sxs-lookup"><span data-stu-id="7a88a-208">This form of CLI function pointers puts the `this` parameter as an explicit first parameter of the function pointer syntax.</span></span>

``` csharp
unsafe class Instance {
    void Use() {
        delegate* instance<Instance, string> f = &ToString;
        f(this);
    }
}
```

<span data-ttu-id="7a88a-209">这听起来很复杂，但在提议中增加一些复杂化。</span><span class="sxs-lookup"><span data-stu-id="7a88a-209">This is sound but adds some complication to the proposal.</span></span> <span data-ttu-id="7a88a-210">特别是，由于与调用约定不同的函数指针 `instance` 和 `managed` 会不兼容，即使这两种情况都用于使用相同C#的签名调用托管方法。</span><span class="sxs-lookup"><span data-stu-id="7a88a-210">Particularly because function pointers which differed by the calling convention `instance` and `managed` would be incompatible even though both cases are used to invoke managed methods with the same C# signature.</span></span> <span data-ttu-id="7a88a-211">此外，在每种情况下，在这种情况下，有一个简单的解决方法是很有价值的：使用 `static` 本地函数。</span><span class="sxs-lookup"><span data-stu-id="7a88a-211">Also in every case considered where this would be valuable to have there was a simple work around: use a `static` local function.</span></span>

``` csharp
unsafe class Instance {
    void Use() {
        static string toString(Instance i) = i.ToString();
        delgate*<Instance, string> f = &toString;
        f(this);
    }
}
```

### <a name="dont-require-unsafe-at-declaration"></a><span data-ttu-id="7a88a-212">声明时不需要 unsafe</span><span class="sxs-lookup"><span data-stu-id="7a88a-212">Don't require unsafe at declaration</span></span>

<span data-ttu-id="7a88a-213">不需要每次使用 `delegate*`都需要 `unsafe`，只需在方法组转换为 `delegate*`时才需要。</span><span class="sxs-lookup"><span data-stu-id="7a88a-213">Instead of requiring `unsafe` at every use of a `delegate*`, only require it at the point where a method group is converted to a `delegate*`.</span></span> <span data-ttu-id="7a88a-214">这就是核心安全问题的播放（知道在值处于活动状态时不能卸载包含程序集）。</span><span class="sxs-lookup"><span data-stu-id="7a88a-214">This is where the core safety issues come into play (knowing that the containing assembly cannot be unloaded while the value is alive).</span></span> <span data-ttu-id="7a88a-215">需要在其他位置上 `unsafe`。</span><span class="sxs-lookup"><span data-stu-id="7a88a-215">Requiring `unsafe` on the other locations can be seen as excessive.</span></span>

<span data-ttu-id="7a88a-216">这就是设计的最初设计方式。</span><span class="sxs-lookup"><span data-stu-id="7a88a-216">This is how the design was originally intended.</span></span> <span data-ttu-id="7a88a-217">但产生的语言规则觉得非常笨拙。</span><span class="sxs-lookup"><span data-stu-id="7a88a-217">But the resulting language rules felt very awkward.</span></span> <span data-ttu-id="7a88a-218">无法隐藏这一事实：这是一个指针值，即使没有 `unsafe` 关键字，它仍可查看。</span><span class="sxs-lookup"><span data-stu-id="7a88a-218">It's impossible to hide the fact that this is a pointer value and it kept peeking through even without the `unsafe` keyword.</span></span> <span data-ttu-id="7a88a-219">例如，不能允许转换为 `object`，它不能是 `class`的成员，等等。这C#种设计需要对所有指针使用 `unsafe`，因此这种设计遵循这一设计。</span><span class="sxs-lookup"><span data-stu-id="7a88a-219">For example the conversion to `object` can't be allowed, it can't be a member of a `class`, etc ... The C# design is to require `unsafe` for all pointer uses and hence this design follows that.</span></span>

<span data-ttu-id="7a88a-220">开发人员仍可以在 `delegate*` 的值的基础上呈现_安全_包装器，其方式与当今普通指针类型的方式相同。</span><span class="sxs-lookup"><span data-stu-id="7a88a-220">Developers will still be capable of presenting a _safe_ wrapper on top of `delegate*` values the same way that they do for normal pointer types today.</span></span> <span data-ttu-id="7a88a-221">请考虑：</span><span class="sxs-lookup"><span data-stu-id="7a88a-221">Consider:</span></span>

``` csharp
unsafe struct Action {
    delegate*<void> _ptr;

    Action(delegate*<void> ptr) => _ptr = ptr;
    public void Invoke() => _ptr();
}
```

### <a name="using-delegates"></a><span data-ttu-id="7a88a-222">使用委托</span><span class="sxs-lookup"><span data-stu-id="7a88a-222">Using delegates</span></span>

<span data-ttu-id="7a88a-223">`delegate*`，不使用新的语法元素，只需将现有 `delegate` 类型与 `*` 后的类型结合使用：</span><span class="sxs-lookup"><span data-stu-id="7a88a-223">Instead of using a new syntax element, `delegate*`, simply use existing `delegate` types with a `*` following the type:</span></span>

``` csharp
Func<object, object, bool>* ptr = &object.ReferenceEquals;
```

<span data-ttu-id="7a88a-224">可以通过使用指定 `CallingConvention` 值的属性来注释 `delegate` 类型来处理调用约定。</span><span class="sxs-lookup"><span data-stu-id="7a88a-224">Handling calling convention can be done by annotating the `delegate` types with an attribute that specifies a `CallingConvention` value.</span></span> <span data-ttu-id="7a88a-225">缺少特性会表示托管调用约定。</span><span class="sxs-lookup"><span data-stu-id="7a88a-225">The lack of an attribute would signify the managed calling convention.</span></span>

<span data-ttu-id="7a88a-226">在 IL 中对此进行编码会出现问题。</span><span class="sxs-lookup"><span data-stu-id="7a88a-226">Encoding this in IL is problematic.</span></span> <span data-ttu-id="7a88a-227">基础值需要表示为一个指针，但它也必须：</span><span class="sxs-lookup"><span data-stu-id="7a88a-227">The underlying value needs to be represented as a pointer yet it also must:</span></span>

1. <span data-ttu-id="7a88a-228">具有唯一的类型，以允许具有不同函数指针类型的重载。</span><span class="sxs-lookup"><span data-stu-id="7a88a-228">Have a unique type to allow for overloads with different function pointer types.</span></span>
1. <span data-ttu-id="7a88a-229">对于跨程序集边界的 OHI 是等效的。</span><span class="sxs-lookup"><span data-stu-id="7a88a-229">Be equivalent for OHI purposes across assembly boundaries.</span></span>

<span data-ttu-id="7a88a-230">最后一点特别有问题。</span><span class="sxs-lookup"><span data-stu-id="7a88a-230">The last point is particularly problematic.</span></span> <span data-ttu-id="7a88a-231">这意味着，使用 `Func<int>*` 的每个程序集都必须在元数据中编码等效的类型，即使 `Func<int>*` 是在程序集中定义的，但不进行控制。</span><span class="sxs-lookup"><span data-stu-id="7a88a-231">This mean that every assembly which uses `Func<int>*` must encode an equivalent type in metadata even though `Func<int>*` is defined in an assembly though don't control.</span></span>
<span data-ttu-id="7a88a-232">此外，在非 mscorlib 的程序集中，使用名称 `System.Func<T>` 定义的任何其他类型必须不同于 mscorlib 中定义的版本。</span><span class="sxs-lookup"><span data-stu-id="7a88a-232">Additionally any other type which is defined with the name `System.Func<T>` in an assembly that is not mscorlib must be different than the version defined in mscorlib.</span></span>

<span data-ttu-id="7a88a-233">研究的一个选项是发出 `mod_req(Func<int>) void*`的指针。</span><span class="sxs-lookup"><span data-stu-id="7a88a-233">One option that was explored was emitting such a pointer as `mod_req(Func<int>) void*`.</span></span> <span data-ttu-id="7a88a-234">但这不起作用，因为 `mod_req` 无法绑定到 `TypeSpec`，因此不能以泛型实例化为目标。</span><span class="sxs-lookup"><span data-stu-id="7a88a-234">This doesn't work though as a `mod_req` cannot bind to a `TypeSpec` and hence cannot target generic instantiations.</span></span>

### <a name="named-function-pointers"></a><span data-ttu-id="7a88a-235">命名函数指针</span><span class="sxs-lookup"><span data-stu-id="7a88a-235">Named function pointers</span></span>

<span data-ttu-id="7a88a-236">函数指针语法可能比较繁琐，尤其是在复杂情况下，例如嵌套函数指针。</span><span class="sxs-lookup"><span data-stu-id="7a88a-236">The function pointer syntax can be cumbersome, particularly in complex cases like nested function pointers.</span></span> <span data-ttu-id="7a88a-237">不是让开发人员在每次 `delegate`使用时都允许函数指针的命名声明，而不是每次都键入签名。</span><span class="sxs-lookup"><span data-stu-id="7a88a-237">Rather than have developers type out the signature every time the language could allow for named declarations of function pointers as is done with `delegate`.</span></span>

``` csharp
func* void Action();

unsafe class NamedExample {
    void M(Action a) {
        a();
    }
}
```

<span data-ttu-id="7a88a-238">此处的问题部分是基础 CLI 基元没有名称，因此，这只是一C#种发明，需要使用一种元数据才能实现。</span><span class="sxs-lookup"><span data-stu-id="7a88a-238">Part of the problem here is the underlying CLI primitive doesn't have names hence this would be purely a C# invention and require a bit of metadata work to enable.</span></span> <span data-ttu-id="7a88a-239">这是可行的，但这是一个重要的工作。</span><span class="sxs-lookup"><span data-stu-id="7a88a-239">That is doable but is a significant about of work.</span></span> <span data-ttu-id="7a88a-240">实质上， C#只需将类型定义表与这些名称一起使用。</span><span class="sxs-lookup"><span data-stu-id="7a88a-240">It essentially requires C# to have a companion to the type def table purely for these names.</span></span>

<span data-ttu-id="7a88a-241">此外，当检查命名函数指针的参数时，我们发现它们可以同样适用于许多其他方案。</span><span class="sxs-lookup"><span data-stu-id="7a88a-241">Also when the arguments for named function pointers were examined we found they could apply equally well to a number of other scenarios.</span></span> <span data-ttu-id="7a88a-242">例如，将命名元组声明为在所有情况下都可以轻松地在所有情况下都需要键入完整的签名。</span><span class="sxs-lookup"><span data-stu-id="7a88a-242">For example it would be just as convenient to declare named tuples to reduce the need to type out the full signature in all cases.</span></span>

``` csharp
(int x, int y) Point;

class NamedTupleExample {
    void M(Point p) {
        Console.WriteLine(p.x);
    }
}
```

<span data-ttu-id="7a88a-243">讨论后，我们决定不允许 `delegate*` 类型的命名声明。</span><span class="sxs-lookup"><span data-stu-id="7a88a-243">After discussion we decided to not allow named declaration of `delegate*` types.</span></span> <span data-ttu-id="7a88a-244">如果我们发现，根据客户使用反馈，需要特别需要此功能，我们将调查适用于函数指针、元组、泛型等的命名解决方案。这种形式的格式可能与其他建议类似，如语言的完整 `typedef` 支持。</span><span class="sxs-lookup"><span data-stu-id="7a88a-244">If we find there is significant need for this based on customer usage feedback then we will investigate a naming solution that works for function pointers, tuples, generics, etc ... This is likely to be similar in form to other suggestions like full `typedef` support in the language.</span></span>

## <a name="future-considerations"></a><span data-ttu-id="7a88a-245">未来的注意事项</span><span class="sxs-lookup"><span data-stu-id="7a88a-245">Future Considerations</span></span>

### <a name="static-local-functions"></a><span data-ttu-id="7a88a-246">静态本地函数</span><span class="sxs-lookup"><span data-stu-id="7a88a-246">static local functions</span></span>

<span data-ttu-id="7a88a-247">这是指允许对本地函数进行 `static` 修饰符的[提议](https://github.com/dotnet/csharplang/issues/1565)。</span><span class="sxs-lookup"><span data-stu-id="7a88a-247">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/1565) to allow the `static` modifier on local functions.</span></span> <span data-ttu-id="7a88a-248">此类函数将保证作为 `static` 和在源代码中指定的确切签名发出。</span><span class="sxs-lookup"><span data-stu-id="7a88a-248">Such a function would be guaranteed to be emitted as `static` and with the exact signature specified in source code.</span></span> <span data-ttu-id="7a88a-249">此类函数应为 `&` 的有效参数，因为它不包含本地函数当前所具有的任何问题</span><span class="sxs-lookup"><span data-stu-id="7a88a-249">Such a function should be a valid argument to `&` as it contains none of the problems local functions have today</span></span>

### <a name="static-delegates"></a><span data-ttu-id="7a88a-250">静态委托</span><span class="sxs-lookup"><span data-stu-id="7a88a-250">static delegates</span></span>

<span data-ttu-id="7a88a-251">这是指允许 `delegate` 类型声明的[建议](https://github.com/dotnet/csharplang/issues/302)，这些类型只能引用 `static` 成员。</span><span class="sxs-lookup"><span data-stu-id="7a88a-251">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/302) to allow for the declaration of `delegate` types which can only refer to `static` members.</span></span> <span data-ttu-id="7a88a-252">其优势在于，此类 `delegate` 实例可在性能敏感的情况下免费分配和更好地分配。</span><span class="sxs-lookup"><span data-stu-id="7a88a-252">The advantage being that such `delegate` instances can be allocation free and better in performance sensitive scenarios.</span></span>

<span data-ttu-id="7a88a-253">如果实现函数指针功能，`static delegate` 建议可能会关闭。此功能的建议优点是分配免费性质。</span><span class="sxs-lookup"><span data-stu-id="7a88a-253">If the function pointer feature is implemented the `static delegate` proposal will likely be closed out. The proposed advantage of that feature is the allocation free nature.</span></span> <span data-ttu-id="7a88a-254">但最近的调查发现，由于程序集卸载，无法实现。</span><span class="sxs-lookup"><span data-stu-id="7a88a-254">However recent investigations have found that is not possible to achieve due to assembly unloading.</span></span> <span data-ttu-id="7a88a-255">必须有一个从 `static delegate` 到它所引用的方法的强句柄，以便使程序集无法从其下卸载。</span><span class="sxs-lookup"><span data-stu-id="7a88a-255">There must be a strong handle from the `static delegate` to the method it refers to in order to keep the assembly from being unloaded out from under it.</span></span>

<span data-ttu-id="7a88a-256">若要维护每个 `static delegate` 实例，需要分配一个运行该句柄目标的新句柄。</span><span class="sxs-lookup"><span data-stu-id="7a88a-256">To maintain every `static delegate` instance would be required to allocate a new handle which runs counter to the goals of the proposal.</span></span> <span data-ttu-id="7a88a-257">在某些设计中，分配可能会分摊到每个调用站点的单个分配，但这有点复杂，不值得权衡。</span><span class="sxs-lookup"><span data-stu-id="7a88a-257">There were some designs where the allocation could be amortized to a single allocation per call-site but that was a bit complex and didn't seem worth the trade off.</span></span>

<span data-ttu-id="7a88a-258">这意味着开发人员实质上必须决定以下权衡：</span><span class="sxs-lookup"><span data-stu-id="7a88a-258">That means developers essentially have to decide between the following trade offs:</span></span>

1. <span data-ttu-id="7a88a-259">程序集卸载面上的安全性：这需要分配，因此 `delegate` 已是一个足够的选项。</span><span class="sxs-lookup"><span data-stu-id="7a88a-259">Safety in the face of assembly unloading: this requires allocations and hence `delegate` is already a sufficient option.</span></span>
1. <span data-ttu-id="7a88a-260">程序集卸载没有任何安全：使用 `delegate*`。</span><span class="sxs-lookup"><span data-stu-id="7a88a-260">No safety in face of assembly unloading: use a `delegate*`.</span></span> <span data-ttu-id="7a88a-261">这可以包装在 `struct` 中，以允许在其余代码的外部使用 `unsafe` 上下文。</span><span class="sxs-lookup"><span data-stu-id="7a88a-261">This can be wrapped in a `struct` to allow usage outside an `unsafe` context in the rest of the code.</span></span>
