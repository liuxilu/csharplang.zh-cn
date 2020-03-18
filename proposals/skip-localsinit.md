---
ms.openlocfilehash: 52b43abd2d8fb56088a68c7169289a63c43ce96f
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483486"
---
# <a name="suppress-emitting-of-localsinit-flag"></a><span data-ttu-id="881ac-101">禁止发出 `localsinit` 标志。</span><span class="sxs-lookup"><span data-stu-id="881ac-101">Suppress emitting of `localsinit` flag.</span></span>

* <span data-ttu-id="881ac-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="881ac-102">[x] Proposed</span></span>
* <span data-ttu-id="881ac-103">[] 原型：未启动</span><span class="sxs-lookup"><span data-stu-id="881ac-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="881ac-104">[] 实现：未启动</span><span class="sxs-lookup"><span data-stu-id="881ac-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="881ac-105">[] 规范：未启动</span><span class="sxs-lookup"><span data-stu-id="881ac-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="881ac-106">总结</span><span class="sxs-lookup"><span data-stu-id="881ac-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="881ac-107">允许通过 `SkipLocalsInitAttribute` 属性取消发出 `localsinit` 标志。</span><span class="sxs-lookup"><span data-stu-id="881ac-107">Allow suppressing emit of `localsinit` flag via `SkipLocalsInitAttribute` attribute.</span></span> 

## <a name="motivation"></a><span data-ttu-id="881ac-108">动机</span><span class="sxs-lookup"><span data-stu-id="881ac-108">Motivation</span></span>
[motivation]: #motivation


### <a name="background"></a><span data-ttu-id="881ac-109">背景</span><span class="sxs-lookup"><span data-stu-id="881ac-109">Background</span></span>
<span data-ttu-id="881ac-110">每个不包含引用的 CLR 规范局部变量不会通过 VM/JIT 初始化为特定值。</span><span class="sxs-lookup"><span data-stu-id="881ac-110">Per CLR spec local variables that do not contain references are not initialized to a particular value by the VM/JIT.</span></span> <span data-ttu-id="881ac-111">在不初始化的情况下读取此类变量是类型安全的，但此行为是未定义的，并且是特定于实现的。</span><span class="sxs-lookup"><span data-stu-id="881ac-111">Reading from such variables without initialization is type-safe, but otherwise the behavior is undefined and implementation specific.</span></span> <span data-ttu-id="881ac-112">通常未初始化的局部变量包含保留在堆栈帧现在占用的内存中的任何值。</span><span class="sxs-lookup"><span data-stu-id="881ac-112">Typically uninitialized locals contain whatever values were left in the memory that is now occupied by the stack frame.</span></span> <span data-ttu-id="881ac-113">这可能导致不确定的行为，并且难以重现 bug。</span><span class="sxs-lookup"><span data-stu-id="881ac-113">That could lead to nondeterministic behavior and hard to reproduce bugs.</span></span> 

<span data-ttu-id="881ac-114">可以通过两种方法来 "分配" 局部变量：</span><span class="sxs-lookup"><span data-stu-id="881ac-114">There are two ways to "assign" a local variable:</span></span> 
- <span data-ttu-id="881ac-115">通过存储值或</span><span class="sxs-lookup"><span data-stu-id="881ac-115">by storing a value or</span></span> 
- <span data-ttu-id="881ac-116">通过指定 `localsinit` 标志，将分配给本地内存池的所有内容转换为零初始化说明：这包括本地变量和 `stackalloc` 数据。</span><span class="sxs-lookup"><span data-stu-id="881ac-116">by specifying `localsinit` flag which forces everything that is allocated form the local memory pool to be zero-initialized NOTE: this includes both local variables and `stackalloc` data.</span></span>    

<span data-ttu-id="881ac-117">不建议使用未初始化的数据，并且在可验证代码中不允许使用。</span><span class="sxs-lookup"><span data-stu-id="881ac-117">Use of uninitialized data is discouraged and is not allowed in verifiable code.</span></span> <span data-ttu-id="881ac-118">尽管可以通过流分析的方法来证明这一点，但允许验证算法非常保守，只需设置 `localsinit` 即可。</span><span class="sxs-lookup"><span data-stu-id="881ac-118">While it might be possible to prove that by the means of flow analysis, it is permitted for the verification algorithm to be conservative and simply require that `localsinit` is set.</span></span>

<span data-ttu-id="881ac-119">过去C#的编译器将在声明局部变量的所有方法上发出 `localsinit` 标志。</span><span class="sxs-lookup"><span data-stu-id="881ac-119">Historically C# compiler emits `localsinit` flag on all methods that declare locals.</span></span>

<span data-ttu-id="881ac-120">虽然C#使用的是明确的赋值分析，这种分析比 CLR 规范所需C#的方法更严格（还需要考虑对局部变量的范围），但并不完全保证最终的代码是可以正式验证的：</span><span class="sxs-lookup"><span data-stu-id="881ac-120">While C# employs definite-assignment analysis which is more strict than what CLR spec would require (C# also needs to consider scoping of locals), it is not strictly guaranteed that the resulting code would be formally verifiable:</span></span>
- <span data-ttu-id="881ac-121">CLR 和C#规则可能不同意将 local 作为 `out` 参数传递是否为 `use`。</span><span class="sxs-lookup"><span data-stu-id="881ac-121">CLR and C# rules may not agree on whether passing a local as `out` argument is a `use`.</span></span>
- <span data-ttu-id="881ac-122">已知条件C#时，CLR 和规则可能不同意条件分支的处理（常量传播）。</span><span class="sxs-lookup"><span data-stu-id="881ac-122">CLR and C# rules may not agree on treatment of conditional branches when conditions are known (constant propagation).</span></span>
- <span data-ttu-id="881ac-123">CLR 可能也只需要 `localinits`，因为这是允许的。</span><span class="sxs-lookup"><span data-stu-id="881ac-123">CLR could as well simply require `localinits`, since that is permitted.</span></span>  

### <a name="problem"></a><span data-ttu-id="881ac-124">问题</span><span class="sxs-lookup"><span data-stu-id="881ac-124">Problem</span></span>
<span data-ttu-id="881ac-125">在高性能的应用程序中，强制的零初始化的成本可能比较明显。</span><span class="sxs-lookup"><span data-stu-id="881ac-125">In high-performance application the cost of forced zero-initialization could be noticeable.</span></span> <span data-ttu-id="881ac-126">当使用 `stackalloc` 时，尤其明显。</span><span class="sxs-lookup"><span data-stu-id="881ac-126">It is particularly noticeable when `stackalloc` is used.</span></span>

<span data-ttu-id="881ac-127">在某些情况下，在后续赋值时，JIT 可以 elide 单个局部变量的初始零初始化。</span><span class="sxs-lookup"><span data-stu-id="881ac-127">In some cases JIT can elide initial zero-initialization of individual locals when such initialization is "killed" by subsequent assignments.</span></span> <span data-ttu-id="881ac-128">并非所有 Jit 都这样做，因此这种优化有限制。</span><span class="sxs-lookup"><span data-stu-id="881ac-128">Not all JITs do this and such optimization has limits.</span></span> <span data-ttu-id="881ac-129">它不有助于 `stackalloc`。</span><span class="sxs-lookup"><span data-stu-id="881ac-129">It does not help with `stackalloc`.</span></span>

<span data-ttu-id="881ac-130">为了说明问题是真正的，有一个已知 bug，其中不包含任何 `IL` 局部变量的方法将不会 `localsinit` 标志。</span><span class="sxs-lookup"><span data-stu-id="881ac-130">To illustrate that the problem is real - there is a known bug where a method not containing any `IL` locals would not have `localsinit` flag.</span></span> <span data-ttu-id="881ac-131">用户通过将 `stackalloc` 放入此类方法来避免初始化成本，从而使该 bug 被用户利用。</span><span class="sxs-lookup"><span data-stu-id="881ac-131">The bug is already being exploited by users by putting `stackalloc` into such methods - intentionally to avoid initialization costs.</span></span> <span data-ttu-id="881ac-132">这是因为，缺少 `IL` 局部变量是不稳定的指标，可能因 codegen 策略中的更改而异。</span><span class="sxs-lookup"><span data-stu-id="881ac-132">That is despite the fact that absence of `IL` locals is an unstable metric and may vary depending on changes in codegen strategy.</span></span> <span data-ttu-id="881ac-133">Bug 应该是固定的，用户应获得更多记录并可靠的方式来禁止标志。</span><span class="sxs-lookup"><span data-stu-id="881ac-133">The bug should be fixed and users should get a more documented and reliable way of suppressing the flag.</span></span> 

## <a name="detailed-design"></a><span data-ttu-id="881ac-134">详细设计</span><span class="sxs-lookup"><span data-stu-id="881ac-134">Detailed design</span></span>

<span data-ttu-id="881ac-135">允许指定 `System.Runtime.CompilerServices.SkipLocalsInitAttribute`，以指示编译器不发出 `localsinit` 标志。</span><span class="sxs-lookup"><span data-stu-id="881ac-135">Allow specifying `System.Runtime.CompilerServices.SkipLocalsInitAttribute` as a way to tell the compiler to not emit `localsinit` flag.</span></span>
 
<span data-ttu-id="881ac-136">这种情况的最终结果将是 JIT 不能初始化局部变量，这在大多数情况下 unobservable C#。</span><span class="sxs-lookup"><span data-stu-id="881ac-136">The end result of this will be that the locals may not be zero-initialized by the JIT, which is in most cases unobservable in C#.</span></span>  
<span data-ttu-id="881ac-137">除了 `stackalloc` 的数据将不会初始化为零。</span><span class="sxs-lookup"><span data-stu-id="881ac-137">In addition to that `stackalloc` data will not be zero-initialized.</span></span> <span data-ttu-id="881ac-138">这无疑是显而易见的，但也是最具打动的方案。</span><span class="sxs-lookup"><span data-stu-id="881ac-138">That is definitely observable, but also is the most motivating scenario.</span></span>

<span data-ttu-id="881ac-139">允许和识别的属性目标为： `Method`、`Property`、`Module`、`Class`、`Struct`、`Interface`、`Constructor`。</span><span class="sxs-lookup"><span data-stu-id="881ac-139">Permitted and recognized attribute targets are: `Method`, `Property`, `Module`, `Class`, `Struct`, `Interface`, `Constructor`.</span></span> <span data-ttu-id="881ac-140">但是，编译器不需要为列出的目标定义该属性，也不会小心定义该属性的程序集。</span><span class="sxs-lookup"><span data-stu-id="881ac-140">However compiler will not require that attribute is defined with the listed targets nor it will care in which assembly the attribute is defined.</span></span> 

<span data-ttu-id="881ac-141">如果在容器上指定了 attribute （`class`、`module`、包含嵌套方法的方法），则该标志会影响容器中包含的所有方法。</span><span class="sxs-lookup"><span data-stu-id="881ac-141">When attribute is specified on a container (`class`, `module`, containing method for a nested method, ...), the flag affects all methods contained within the container.</span></span>

<span data-ttu-id="881ac-142">合成方法从逻辑容器/所有者继承标志。</span><span class="sxs-lookup"><span data-stu-id="881ac-142">Synthesized methods "inherit" the flag from the logical container/owner.</span></span> 

<span data-ttu-id="881ac-143">标志仅影响实际方法体的 codegen 策略。</span><span class="sxs-lookup"><span data-stu-id="881ac-143">The flag affects only codegen strategy for actual method bodies.</span></span> <span data-ttu-id="881ac-144">I.E.</span><span class="sxs-lookup"><span data-stu-id="881ac-144">I.E.</span></span> <span data-ttu-id="881ac-145">标志对抽象方法不起作用，并且不会传播到重写/实现方法。</span><span class="sxs-lookup"><span data-stu-id="881ac-145">the flag has no effect on abstract methods and is not propagated to overriding/implementing methods.</span></span>

<span data-ttu-id="881ac-146">这是显式的 **_编译器功能_** ，而 **_不是语言功能_** 。</span><span class="sxs-lookup"><span data-stu-id="881ac-146">This is explicitly a **_compiler feature_** and **_not a language feature_**.</span></span>  
<span data-ttu-id="881ac-147">类似于编译器命令行开关，该功能控制特定 codegen 策略的实现细节，而不需要C#规范。</span><span class="sxs-lookup"><span data-stu-id="881ac-147">Similarly to compiler command line switches the feature controls implementation details of a particular codegen strategy and does not need to be required by the C# spec.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="881ac-148">缺点</span><span class="sxs-lookup"><span data-stu-id="881ac-148">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="881ac-149">旧的/其他编译器可能不接受属性。</span><span class="sxs-lookup"><span data-stu-id="881ac-149">Old/other compilers may not honor the attribute.</span></span>
<span data-ttu-id="881ac-150">忽略属性的行为是兼容的。</span><span class="sxs-lookup"><span data-stu-id="881ac-150">Ignoring the attribute is compatible behavior.</span></span> <span data-ttu-id="881ac-151">只有可能会导致轻微的性能下降。</span><span class="sxs-lookup"><span data-stu-id="881ac-151">Only may result in a slight perf hit.</span></span>

- <span data-ttu-id="881ac-152">没有 `localinits` 标志的代码可能会触发验证失败。</span><span class="sxs-lookup"><span data-stu-id="881ac-152">The code without `localinits` flag may trigger verification failures.</span></span>
<span data-ttu-id="881ac-153">要求提供此功能的用户通常不关心具有可验证性。</span><span class="sxs-lookup"><span data-stu-id="881ac-153">Users that ask for this feature are generally unconcerned with verifiability.</span></span> 
 
- <span data-ttu-id="881ac-154">在比单个方法更高的级别应用特性具有非本地效果，在使用 `stackalloc` 时，这是可观察的。</span><span class="sxs-lookup"><span data-stu-id="881ac-154">Applying the attribute at higher levels than an individual method has nonlocal effect, which is observable when `stackalloc` is used.</span></span> <span data-ttu-id="881ac-155">但这是最常请求的方案。</span><span class="sxs-lookup"><span data-stu-id="881ac-155">Yet, this is the most requested scenario.</span></span>

## <a name="alternatives"></a><span data-ttu-id="881ac-156">备选项</span><span class="sxs-lookup"><span data-stu-id="881ac-156">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="881ac-157">`unsafe` 上下文中声明方法时省略 `localinits` 标志。</span><span class="sxs-lookup"><span data-stu-id="881ac-157">omit `localinits` flag when method is declared in `unsafe` context.</span></span> <span data-ttu-id="881ac-158">在 `stackalloc`的情况下，这可能会导致从确定性到非确定性的静默和危险的行为更改。</span><span class="sxs-lookup"><span data-stu-id="881ac-158">That could cause silent and dangerous behavior change from deterministic to nondeterministic in a case of `stackalloc`.</span></span>

- <span data-ttu-id="881ac-159">忽略 `localinits` 标志。</span><span class="sxs-lookup"><span data-stu-id="881ac-159">omit `localinits` flag always.</span></span>
<span data-ttu-id="881ac-160">甚至更糟。</span><span class="sxs-lookup"><span data-stu-id="881ac-160">Even worse than above.</span></span>

- <span data-ttu-id="881ac-161">省略 `localinits` 标志，除非在方法体中使用 `stackalloc`。</span><span class="sxs-lookup"><span data-stu-id="881ac-161">omit `localinits` flag unless `stackalloc` is used in the method body.</span></span>
<span data-ttu-id="881ac-162">不处理请求最多的方案，可能会打开无法验证的代码，而无需恢复返回的选项。</span><span class="sxs-lookup"><span data-stu-id="881ac-162">Does not address the most requested scenario and may turn code unverifiable with no option to revert that back.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="881ac-163">未解决的问题</span><span class="sxs-lookup"><span data-stu-id="881ac-163">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="881ac-164">该属性是否应实际发出到元数据？</span><span class="sxs-lookup"><span data-stu-id="881ac-164">Should the attribute be actually emitted to metadata?</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="881ac-165">设计会议</span><span class="sxs-lookup"><span data-stu-id="881ac-165">Design meetings</span></span>

<span data-ttu-id="881ac-166">尚无。</span><span class="sxs-lookup"><span data-stu-id="881ac-166">None yet.</span></span> 