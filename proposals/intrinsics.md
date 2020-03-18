---
ms.openlocfilehash: ac4c8760e3b6a0934f01ae634f666af60aa1c0fe
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483552"
---
# <a name="compiler-intrinsics"></a><span data-ttu-id="00b4f-101">编译器内部函数</span><span class="sxs-lookup"><span data-stu-id="00b4f-101">Compiler Intrinsics</span></span>

## <a name="summary"></a><span data-ttu-id="00b4f-102">总结</span><span class="sxs-lookup"><span data-stu-id="00b4f-102">Summary</span></span>

<span data-ttu-id="00b4f-103">此建议提供的语言构造公开了当前无法有效访问的低级别 IL 操作码，或者根本不能访问： `ldftn`、`ldvirtftn`、`ldtoken` 和 `calli`。</span><span class="sxs-lookup"><span data-stu-id="00b4f-103">This proposal provides language constructs that expose low level IL opcodes that cannot currently be accessed efficiently, or at all: `ldftn`, `ldvirtftn`, `ldtoken` and `calli`.</span></span> <span data-ttu-id="00b4f-104">这些低级别操作码在高性能代码中很重要，开发人员需要一种高效的访问方式。</span><span class="sxs-lookup"><span data-stu-id="00b4f-104">These low level opcodes can be important in high performance code and developers need an efficient way to access them.</span></span>

## <a name="motivation"></a><span data-ttu-id="00b4f-105">动机</span><span class="sxs-lookup"><span data-stu-id="00b4f-105">Motivation</span></span>

<span data-ttu-id="00b4f-106">以下问题中介绍了此功能的动机和背景（这是此功能的一个潜在实现方式）：</span><span class="sxs-lookup"><span data-stu-id="00b4f-106">The motivations and background for this feature are described in the following issue (as is a potential implementation of the feature):</span></span> 

https://github.com/dotnet/csharplang/issues/191

<span data-ttu-id="00b4f-107">这一备用设计建议在查看原提议的原型实现后，还会通过 @msjabby，以及在整个重要的基本代码中使用。</span><span class="sxs-lookup"><span data-stu-id="00b4f-107">This alternate design proposal comes after reviewing a prototype implementation of the original proposal by @msjabby as well as the use throughout a significant code base.</span></span> <span data-ttu-id="00b4f-108">此设计是通过 @mjsabby、@tmat 和 @jkotas的重要输入来完成的。</span><span class="sxs-lookup"><span data-stu-id="00b4f-108">This design was done with significant input from @mjsabby, @tmat and @jkotas.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="00b4f-109">详细设计</span><span class="sxs-lookup"><span data-stu-id="00b4f-109">Detailed Design</span></span> 

### <a name="allow-address-of-to-target-methods"></a><span data-ttu-id="00b4f-110">允许目标方法的地址</span><span class="sxs-lookup"><span data-stu-id="00b4f-110">Allow address of to target methods</span></span>

<span data-ttu-id="00b4f-111">现在将允许方法组作为表达式的参数。</span><span class="sxs-lookup"><span data-stu-id="00b4f-111">Method groups will now be allowed as arguments to an address-of expression.</span></span> <span data-ttu-id="00b4f-112">此类表达式的类型将 `void*`。</span><span class="sxs-lookup"><span data-stu-id="00b4f-112">The type of such an expression will be `void*`.</span></span> 

``` csharp
class Util { 
    public static void Log() { } 
}

// ldftn Util.Log
void* ptr = &Util.Log; 
```

<span data-ttu-id="00b4f-113">如果此处没有委托转换，则筛选方法组中的成员的唯一机制是通过静态/实例访问。</span><span class="sxs-lookup"><span data-stu-id="00b4f-113">Given there is no delegate conversion here the only mechanism for filtering members in the method group is by static / instance access.</span></span> <span data-ttu-id="00b4f-114">如果无法区分成员，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="00b4f-114">If that cannot distinguish the members then a compile time error will occur.</span></span>

``` csharp
class Util { 
    public void Log() { } 
    public void Log(string p1) { } 
    public static void Log(int i) { };
}

unsafe {
    // Error: Method group Log has more than one applicable candidate.
    void* ptr1 = &Log; 

    // Okay: only one static member to consider here.
    void* ptr2 = &Util.Log;
}
```

<span data-ttu-id="00b4f-115">此上下文中的 addressof 表达式将通过以下方式实现：</span><span class="sxs-lookup"><span data-stu-id="00b4f-115">The addressof expression in this context will be implemented in the following manner:</span></span>

- <span data-ttu-id="00b4f-116">ldftn：当方法是非虚拟时。</span><span class="sxs-lookup"><span data-stu-id="00b4f-116">ldftn: when the method is non-virtual.</span></span>
- <span data-ttu-id="00b4f-117">ldvirtftn：当方法为虚拟时。</span><span class="sxs-lookup"><span data-stu-id="00b4f-117">ldvirtftn: when the method is virtual.</span></span>

<span data-ttu-id="00b4f-118">此功能的限制：</span><span class="sxs-lookup"><span data-stu-id="00b4f-118">Restrictions of this feature:</span></span>

- <span data-ttu-id="00b4f-119">仅当对值使用调用表达式时，才能指定实例方法</span><span class="sxs-lookup"><span data-stu-id="00b4f-119">Instance methods can only be specified when using an invocation expression on a value</span></span>
- <span data-ttu-id="00b4f-120">无法在 `&`中使用本地函数。</span><span class="sxs-lookup"><span data-stu-id="00b4f-120">Local functions cannot be used in `&`.</span></span> <span data-ttu-id="00b4f-121">这些方法的实现细节特意不由语言指定。</span><span class="sxs-lookup"><span data-stu-id="00b4f-121">The implementation details of these methods are deliberately not specified by the language.</span></span> <span data-ttu-id="00b4f-122">这包括它们是静态的还是实例的，或者是否与它们一起发出的签名完全相同。</span><span class="sxs-lookup"><span data-stu-id="00b4f-122">This includes whether they are static vs. instance or exactly what signature they are emitted with.</span></span>

### <a name="handleof"></a><span data-ttu-id="00b4f-123">handleof</span><span class="sxs-lookup"><span data-stu-id="00b4f-123">handleof</span></span>

<span data-ttu-id="00b4f-124">`handleof` 上下文关键字将使用 `ldtoken` 指令将字段、成员或类型转换为其等效的 `RuntimeHandle` 类型。</span><span class="sxs-lookup"><span data-stu-id="00b4f-124">The `handleof` contextual keyword will translate a field, member or type into their equivalent `RuntimeHandle` type using the `ldtoken` instruction.</span></span> <span data-ttu-id="00b4f-125">表达式的确切类型将取决于 `handleof`中的名称类型：</span><span class="sxs-lookup"><span data-stu-id="00b4f-125">The exact type of the expression will depend on the kind of the name in `handleof`:</span></span>

- <span data-ttu-id="00b4f-126">字段： `RuntimeFieldHandle`</span><span class="sxs-lookup"><span data-stu-id="00b4f-126">field: `RuntimeFieldHandle`</span></span>
- <span data-ttu-id="00b4f-127">类型： `RuntimeTypeHandle`</span><span class="sxs-lookup"><span data-stu-id="00b4f-127">type: `RuntimeTypeHandle`</span></span>
- <span data-ttu-id="00b4f-128">方法： `RuntimeMethodHandle`</span><span class="sxs-lookup"><span data-stu-id="00b4f-128">method: `RuntimeMethodHandle`</span></span>

<span data-ttu-id="00b4f-129">`handleof` 的参数与 `nameof`相同。</span><span class="sxs-lookup"><span data-stu-id="00b4f-129">The arguments to `handleof` are identical to `nameof`.</span></span> <span data-ttu-id="00b4f-130">它必须是简单名称、限定名称、成员访问、指定成员的基访问或指定成员的此访问权限。</span><span class="sxs-lookup"><span data-stu-id="00b4f-130">It must be a simple name, qualified name, member access, base access with a specified member, or this access with a specified member.</span></span> <span data-ttu-id="00b4f-131">参数表达式标识代码定义，但从不进行计算。</span><span class="sxs-lookup"><span data-stu-id="00b4f-131">The argument expression identifies a code definition, but it is never evaluated.</span></span>

<span data-ttu-id="00b4f-132">`handleof` 表达式是在运行时计算的，且返回类型为 `RuntimeHandle`。</span><span class="sxs-lookup"><span data-stu-id="00b4f-132">The `handleof` expression is evaluated at runtime and has a return type of `RuntimeHandle`.</span></span> <span data-ttu-id="00b4f-133">这可以在安全代码中执行，也可以不安全执行。</span><span class="sxs-lookup"><span data-stu-id="00b4f-133">This can be executed in safe code as well as unsafe.</span></span> 

``` 
RuntimeHandle stringHandle = handleof(string);
```

<span data-ttu-id="00b4f-134">此功能的限制：</span><span class="sxs-lookup"><span data-stu-id="00b4f-134">Restrictions of this feature:</span></span>

- <span data-ttu-id="00b4f-135">在 `handleof` 表达式中不能使用属性。</span><span class="sxs-lookup"><span data-stu-id="00b4f-135">Properties cannot be used in a `handleof` expression.</span></span>
- <span data-ttu-id="00b4f-136">当作用域中存在现有 `handleof` 名称时，不能使用 `handleof` 表达式。</span><span class="sxs-lookup"><span data-stu-id="00b4f-136">The `handleof` expression cannot be used when there is an existing `handleof` name in scope.</span></span> <span data-ttu-id="00b4f-137">例如，类型、命名空间等。</span><span class="sxs-lookup"><span data-stu-id="00b4f-137">For example a type, namespace, etc ...</span></span>

### <a name="calli"></a><span data-ttu-id="00b4f-138">calli</span><span class="sxs-lookup"><span data-stu-id="00b4f-138">calli</span></span>

<span data-ttu-id="00b4f-139">编译器将添加对有效转换为 `.calli` 指令的一种新 `extern` 函数的支持。</span><span class="sxs-lookup"><span data-stu-id="00b4f-139">The compiler will add support for a new type of `extern` function that efficiently translates into a `.calli` instruction.</span></span> <span data-ttu-id="00b4f-140">Extern 属性将使用以下形状的属性进行标记：</span><span class="sxs-lookup"><span data-stu-id="00b4f-140">The extern attribute will be marked with an attribute of the following shape:</span></span>

``` csharp
[AttributeUsage(AttributeTargets.Method)]
public sealed class CallIndirectAttribute : Attribute
{
    public CallingConvention CallingConvention { get; }
    public CallIndirectAttribute(CallingConvention callingConvention)
    {
        CallingConvention = callingConvention;
    }
}
```

<span data-ttu-id="00b4f-141">这允许开发人员以以下形式定义方法：</span><span class="sxs-lookup"><span data-stu-id="00b4f-141">This allows developers to define methods in the following form:</span></span>

``` csharp
[CallIndirect(CallingConvention.Cdecl)]
static extern int MapValue(string s, void *ptr);

unsafe {
    var i = MapValue("42", &int.Parse);
    Console.WriteLine(i);
}
```

<span data-ttu-id="00b4f-142">应用了 `CallIndirect` 特性的方法的限制：</span><span class="sxs-lookup"><span data-stu-id="00b4f-142">Restrictions on the method which has the `CallIndirect` attribute applied:</span></span>

- <span data-ttu-id="00b4f-143">不能有 `DllImport` 特性。</span><span class="sxs-lookup"><span data-stu-id="00b4f-143">Cannot have a `DllImport` attribute.</span></span>
- <span data-ttu-id="00b4f-144">不能为泛型。</span><span class="sxs-lookup"><span data-stu-id="00b4f-144">Cannot be generic.</span></span>

## <a name="open-issues"></a><span data-ttu-id="00b4f-145">打开的问题</span><span class="sxs-lookup"><span data-stu-id="00b4f-145">Open Issues</span></span>

### <a name="callingconvention"></a><span data-ttu-id="00b4f-146">CallingConvention</span><span class="sxs-lookup"><span data-stu-id="00b4f-146">CallingConvention</span></span>

<span data-ttu-id="00b4f-147">所设计的 `CallIndirectAttribute` 使用的是缺少托管调用约定条目的 `CallingConvention` 枚举。</span><span class="sxs-lookup"><span data-stu-id="00b4f-147">The `CallIndirectAttribute` as designed uses the `CallingConvention` enum which lacks an entry for managed calling conventions.</span></span> <span data-ttu-id="00b4f-148">枚举需要进行扩展以包含此调用约定，否则该属性需要采用不同的方法。</span><span class="sxs-lookup"><span data-stu-id="00b4f-148">The enum either needs to be extended to include this calling convention or the attribute needs to take a different approach.</span></span>

## <a name="considerations"></a><span data-ttu-id="00b4f-149">注意事项</span><span class="sxs-lookup"><span data-stu-id="00b4f-149">Considerations</span></span>

### <a name="disambiguating-method-groups"></a><span data-ttu-id="00b4f-150">歧义方法组</span><span class="sxs-lookup"><span data-stu-id="00b4f-150">Disambiguating method groups</span></span>

<span data-ttu-id="00b4f-151">有一些围绕功能的讨论，可以更轻松地消除传递到表达式的方法组的歧义。</span><span class="sxs-lookup"><span data-stu-id="00b4f-151">There was some discussion around features that would make it easier to disambiguate method groups passed to an address-of expression.</span></span> <span data-ttu-id="00b4f-152">例如，可能会将签名元素添加到语法：</span><span class="sxs-lookup"><span data-stu-id="00b4f-152">For instance potentially adding signature elements to the syntax:</span></span>

``` csharp
class Util {
    public static void Log() { ... }
    public static void Log(string) { ... }
}

unsafe {
    // Error: ambiguous Log
    void *ptr1 = &Util.Log;

    // Use Util.Log();
    void *ptr2 = &Util.Log();
}
```

<span data-ttu-id="00b4f-153">此操作被拒绝，因为不能做出极具说服力的情况，也不会在此处构想到简单的语法。</span><span class="sxs-lookup"><span data-stu-id="00b4f-153">This was rejected because a compelling case could not be made nor could a simple syntax be envisioned here.</span></span> <span data-ttu-id="00b4f-154">此外，还有一个相当直接的方法：简单地定义其他明确方法，并使用C#代码调入所需的函数。</span><span class="sxs-lookup"><span data-stu-id="00b4f-154">Also there is a fairly straight forward work around: simple define another method that is unambiguous and uses C# code to call into the desired function.</span></span> 

``` csharp
class Workaround {
    public static void LocalLog() => Util.Log();
}
unsafe { 
    void* ptr = &Workaround.LocalLog;
}
```

<span data-ttu-id="00b4f-155">如果 `static` 本地函数进入语言，则此功能将变得更简单。</span><span class="sxs-lookup"><span data-stu-id="00b4f-155">This becomes even simpler if `static` local functions enter the language.</span></span> <span data-ttu-id="00b4f-156">然后，可以在使用不明确的操作地址的同一函数中定义解决方法：</span><span class="sxs-lookup"><span data-stu-id="00b4f-156">Then the work around could be defined in the same function that used the ambiguous address-of operation:</span></span>

``` csharp
unsafe { 
    static void LocalLog() => Util.Log();
    void* ptr = &Workaround.LocalLog;
}
```

### <a name="loadtypetokenint32"></a><span data-ttu-id="00b4f-157">LoadTypeTokenInt32</span><span class="sxs-lookup"><span data-stu-id="00b4f-157">LoadTypeTokenInt32</span></span>

<span data-ttu-id="00b4f-158">允许在编译时将元数据令牌作为 `int` 值加载的原始提议。</span><span class="sxs-lookup"><span data-stu-id="00b4f-158">The original proposal allowed for metadata tokens to be loaded as `int` values at compile time.</span></span> <span data-ttu-id="00b4f-159">实质上具有与 `handleof` 具有相同参数的 `tokenof`，但在编译时计算到 `int` 常数。</span><span class="sxs-lookup"><span data-stu-id="00b4f-159">Essentially have `tokenof` that has the same arguments as `handleof` but is evaluated at compile time to an `int` constant.</span></span> 

<span data-ttu-id="00b4f-160">此操作被拒绝，因为它会导致 IL 重写（.NET 具有多个）出现重大问题。</span><span class="sxs-lookup"><span data-stu-id="00b4f-160">This was rejected as it causes significant problem for IL rewrites (of which .NET has many).</span></span> <span data-ttu-id="00b4f-161">此类 rewriters 通常以可以使这些值无效的方式操作元数据表。</span><span class="sxs-lookup"><span data-stu-id="00b4f-161">Such rewriters often manipulate the metadata tables in a way that could invalidate these values.</span></span> <span data-ttu-id="00b4f-162">如果将这些值存储为简单 `int` 值，则无法合理地更新这些值。</span><span class="sxs-lookup"><span data-stu-id="00b4f-162">There is no reasonable way for such rewriters to update these values when they are stored as simple `int` values.</span></span>

<span data-ttu-id="00b4f-163">运行时团队将继续探讨具有元数据条目的不透明句柄的基本概念。</span><span class="sxs-lookup"><span data-stu-id="00b4f-163">The underlying idea of having an opaque handle for metadata entries will continue to be explored by the runtime team.</span></span> 

## <a name="future-considerations"></a><span data-ttu-id="00b4f-164">未来的注意事项</span><span class="sxs-lookup"><span data-stu-id="00b4f-164">Future Considerations</span></span>

### <a name="static-local-functions"></a><span data-ttu-id="00b4f-165">静态本地函数</span><span class="sxs-lookup"><span data-stu-id="00b4f-165">static local functions</span></span>

<span data-ttu-id="00b4f-166">这是指允许对本地函数进行 `static` 修饰符的[提议](https://github.com/dotnet/csharplang/issues/1565)。</span><span class="sxs-lookup"><span data-stu-id="00b4f-166">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/1565) to allow the `static` modifier on local functions.</span></span> <span data-ttu-id="00b4f-167">此类函数将保证作为 `static` 和在源代码中指定的确切签名发出。</span><span class="sxs-lookup"><span data-stu-id="00b4f-167">Such a function would be guaranteed to be emitted as `static` and with the exact signature specified in source code.</span></span> <span data-ttu-id="00b4f-168">此类函数应为 `&` 的有效参数，因为它不包含本地函数当前所拥有的任何问题。</span><span class="sxs-lookup"><span data-stu-id="00b4f-168">Such a function should be a valid argument to `&` as it contains none of the problems local functions have today.</span></span>

### <a name="nativecallableattribute"></a><span data-ttu-id="00b4f-169">NativeCallableAttribute</span><span class="sxs-lookup"><span data-stu-id="00b4f-169">NativeCallableAttribute</span></span>

<span data-ttu-id="00b4f-170">CLR 具有一项功能，该功能允许以这样一种方式发出托管方法：直接从本机代码中调用这些方法。</span><span class="sxs-lookup"><span data-stu-id="00b4f-170">The CLR has a feature that allows for managed methods to be emitted in such a way that they are directly callable from native code.</span></span> <span data-ttu-id="00b4f-171">这是通过将 `NativeCallableAttribute` 添加到方法来完成的。</span><span class="sxs-lookup"><span data-stu-id="00b4f-171">This is done by adding the `NativeCallableAttribute` to methods.</span></span> <span data-ttu-id="00b4f-172">此类方法只能从本机代码调用，因此必须仅包含签名中的本机类型。</span><span class="sxs-lookup"><span data-stu-id="00b4f-172">Such a method is only callable from native code and hence must contain only blittable types in the signature.</span></span> <span data-ttu-id="00b4f-173">从托管代码调用会导致运行时错误。</span><span class="sxs-lookup"><span data-stu-id="00b4f-173">Calling from managed code results in a runtime error.</span></span> 

<span data-ttu-id="00b4f-174">此功能可与此建议一起进行模式处理，因为这样可以：</span><span class="sxs-lookup"><span data-stu-id="00b4f-174">This feature would pattern well with this proposal as it would allow:</span></span>

- <span data-ttu-id="00b4f-175">将托管代码中定义的函数作为函数指针（通过地址）传递给本机代码，而不会在托管代码或本机代码中产生开销。</span><span class="sxs-lookup"><span data-stu-id="00b4f-175">Passing a function defined in managed code to native code as a function pointer (via address-of) with no overhead in managed or native code.</span></span> 
- <span data-ttu-id="00b4f-176">运行时可在托管代码中引入此类函数的使用站点错误，以防在编译时调用这些函数。</span><span class="sxs-lookup"><span data-stu-id="00b4f-176">Runtime can introduce use site errors for such functions in managed code to prevent them from being invoked at compile time.</span></span>




