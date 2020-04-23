---
ms.openlocfilehash: 54f9a372ca0329a284f06876f544e0b4936af02a
ms.sourcegitcommit: 356ee04506a2a82292be25d7b029e7ce2a39e63a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/22/2020
ms.locfileid: "82037397"
---
# <a name="nullable-reference-types-in-c"></a><span data-ttu-id="5cfee-101">C 中的空引用类型#</span><span class="sxs-lookup"><span data-stu-id="5cfee-101">Nullable reference types in C#</span></span> #

<span data-ttu-id="5cfee-102">此功能的目标是：</span><span class="sxs-lookup"><span data-stu-id="5cfee-102">The goal of this feature is to:</span></span>

* <span data-ttu-id="5cfee-103">允许开发人员表示引用类型的变量、参数或结果是否为空。</span><span class="sxs-lookup"><span data-stu-id="5cfee-103">Allow developers to express whether a variable, parameter or result of a reference type is intended to be null or not.</span></span>
* <span data-ttu-id="5cfee-104">当此类变量、参数和结果未按照该意图使用时，提供警告。</span><span class="sxs-lookup"><span data-stu-id="5cfee-104">Provide warnings when such variables, parameters and results are not used according to that intent.</span></span>

## <a name="expression-of-intent"></a><span data-ttu-id="5cfee-105">意向表达</span><span class="sxs-lookup"><span data-stu-id="5cfee-105">Expression of intent</span></span>

<span data-ttu-id="5cfee-106">语言已包含值类型的`T?`语法。</span><span class="sxs-lookup"><span data-stu-id="5cfee-106">The language already contains the `T?` syntax for value types.</span></span> <span data-ttu-id="5cfee-107">将此语法扩展到引用类型非常简单。</span><span class="sxs-lookup"><span data-stu-id="5cfee-107">It is straightforward to extend this syntax to reference types.</span></span>

<span data-ttu-id="5cfee-108">假定未修饰引用类型`T`的意图是将其为非 null。</span><span class="sxs-lookup"><span data-stu-id="5cfee-108">It is assumed that the intent of an unadorned reference type `T` is for it to be non-null.</span></span>

## <a name="checking-of-nullable-references"></a><span data-ttu-id="5cfee-109">检查可取消的引用</span><span class="sxs-lookup"><span data-stu-id="5cfee-109">Checking of nullable references</span></span>

<span data-ttu-id="5cfee-110">流分析跟踪可无值引用变量。</span><span class="sxs-lookup"><span data-stu-id="5cfee-110">A flow analysis tracks nullable reference variables.</span></span> <span data-ttu-id="5cfee-111">如果分析认为它们不为空（例如，在检查或分配之后），其值将被视为非空引用。</span><span class="sxs-lookup"><span data-stu-id="5cfee-111">Where the analysis deems that they would not be null (e.g. after a check or an assignment), their value will be considered a non-null reference.</span></span>

<span data-ttu-id="5cfee-112">对于流分析无法建立开发人员知道存在的非空情况时，可以使用后修复`x!`运算符（"damnit"运算符）显式将空引用视为非空引用。</span><span class="sxs-lookup"><span data-stu-id="5cfee-112">A nullable reference can also explicitly be treated as non-null with the postfix `x!` operator (the "damnit" operator), for when flow analysis cannot establish a non-null situation that the developer knows is there.</span></span>

<span data-ttu-id="5cfee-113">否则，如果取消引用的空引用或转换为非空类型，则发出警告。</span><span class="sxs-lookup"><span data-stu-id="5cfee-113">Otherwise, a warning is given if a nullable reference is dereferenced, or is converted to a non-null type.</span></span>

<span data-ttu-id="5cfee-114">从`S[]`转换到`T?[]`和`S?[]`时，将发出警告。 `T[]`</span><span class="sxs-lookup"><span data-stu-id="5cfee-114">A warning is given when converting from `S[]` to `T?[]` and from `S?[]` to `T[]`.</span></span>

<span data-ttu-id="5cfee-115">`C<S>`当从`C<T?>`转换 到 ，当类型参数为协变量 （）`out`时，以及从`C<S?>`转换`C<T>`到 ，当类型参数是反变量`in`（） 时，将发出警告。</span><span class="sxs-lookup"><span data-stu-id="5cfee-115">A warning is given when converting from `C<S>` to `C<T?>` except when the type parameter is covariant (`out`), and when converting from `C<S?>` to `C<T>` except when the type parameter is contravariant (`in`).</span></span>

<span data-ttu-id="5cfee-116">`C<T?>`如果类型参数具有非 null 约束，则发出警告。</span><span class="sxs-lookup"><span data-stu-id="5cfee-116">A warning is given on `C<T?>` if the type parameter has non-null constraints.</span></span>

## <a name="checking-of-non-null-references"></a><span data-ttu-id="5cfee-117">检查非空引用</span><span class="sxs-lookup"><span data-stu-id="5cfee-117">Checking of non-null references</span></span>

<span data-ttu-id="5cfee-118">如果将空文本分配给非空变量或作为非空参数传递，则发出警告。</span><span class="sxs-lookup"><span data-stu-id="5cfee-118">A warning is given if a null literal is assigned to a non-null variable or passed as a non-null parameter.</span></span>

<span data-ttu-id="5cfee-119">如果构造函数未显式初始化非空引用字段，也会发出警告。</span><span class="sxs-lookup"><span data-stu-id="5cfee-119">A warning is also given if a constructor does not explicitly initialize non-null reference fields.</span></span>

<span data-ttu-id="5cfee-120">我们无法充分跟踪非空引用数组的所有元素是否初始化。</span><span class="sxs-lookup"><span data-stu-id="5cfee-120">We cannot adequately track that all elements of an array of non-null references are initialized.</span></span> <span data-ttu-id="5cfee-121">但是，如果在读取或传递数组之前未分配新创建的数组的元素，我们可以发出警告。</span><span class="sxs-lookup"><span data-stu-id="5cfee-121">However, we could issue a warning if no element of a newly created array is assigned to before the array is read from or passed on.</span></span> <span data-ttu-id="5cfee-122">这可能处理常见的情况，而不会太吵。</span><span class="sxs-lookup"><span data-stu-id="5cfee-122">That might handle the common case without being too noisy.</span></span>

<span data-ttu-id="5cfee-123">我们需要决定是`default(T)`生成警告，还是简单地被视为类型`T?`。</span><span class="sxs-lookup"><span data-stu-id="5cfee-123">We need to decide whether `default(T)` generates a warning, or is simply treated as being of the type `T?`.</span></span>

## <a name="metadata-representation"></a><span data-ttu-id="5cfee-124">元数据表示</span><span class="sxs-lookup"><span data-stu-id="5cfee-124">Metadata representation</span></span>

<span data-ttu-id="5cfee-125">可空装饰应在元数据中表示为属性。</span><span class="sxs-lookup"><span data-stu-id="5cfee-125">Nullability adornments should be represented in metadata as attributes.</span></span> <span data-ttu-id="5cfee-126">这意味着低级编译器将忽略它们。</span><span class="sxs-lookup"><span data-stu-id="5cfee-126">This means that downlevel compilers will ignore them.</span></span>

<span data-ttu-id="5cfee-127">我们需要决定是否只包括空注释，或者在程序集中是否"打开"也显示非 null。</span><span class="sxs-lookup"><span data-stu-id="5cfee-127">We need to decide if only nullable annotations are included, or there's also some indication of whether non-null was "on" in the assembly.</span></span>

## <a name="generics"></a><span data-ttu-id="5cfee-128">泛型</span><span class="sxs-lookup"><span data-stu-id="5cfee-128">Generics</span></span>

<span data-ttu-id="5cfee-129">如果类型参数`T`具有不可取消的约束，则在其范围内将其视为不可消除。</span><span class="sxs-lookup"><span data-stu-id="5cfee-129">If a type parameter `T` has non-nullable constraints, it is treated as non-nullable within its scope.</span></span>

<span data-ttu-id="5cfee-130">如果类型参数不受限制或仅具有可无约束约束，则情况会稍微复杂一些：这意味着相应的类型*参数可以是*null 的，也可以是非 null 的。</span><span class="sxs-lookup"><span data-stu-id="5cfee-130">If a type parameter is unconstrained or has only nullable constraints, the situation is a little more complex: this means that the corresponding type argument could be *either* nullable or non-nullable.</span></span> <span data-ttu-id="5cfee-131">在这种情况下，安全的做法是将类型*参数视为可*取消和不可取消，当违反任一时发出警告。</span><span class="sxs-lookup"><span data-stu-id="5cfee-131">The safe thing to do in that situation is to treat the type parameter as *both* nullable and non-nullable, giving warnings when either is violated.</span></span> 

<span data-ttu-id="5cfee-132">值得考虑的是，是否应允许显式空引用约束。</span><span class="sxs-lookup"><span data-stu-id="5cfee-132">It is worth considering whether explicit nullable reference constraints should be allowed.</span></span> <span data-ttu-id="5cfee-133">但是请注意，在某些情况下，我们无法避免将可隐含的引用类型*隐式*为约束（继承约束）。</span><span class="sxs-lookup"><span data-stu-id="5cfee-133">Note, however, that we cannot avoid having nullable reference types *implicitly* be constraints in certain cases (inherited constraints).</span></span>

<span data-ttu-id="5cfee-134">约束`class`为非空。</span><span class="sxs-lookup"><span data-stu-id="5cfee-134">The `class` constraint is non-null.</span></span> <span data-ttu-id="5cfee-135">我们可以考虑是否应`class?`是表示"空引用类型"的有效空约束。</span><span class="sxs-lookup"><span data-stu-id="5cfee-135">We can consider whether `class?` should be a valid nullable constraint denoting "nullable reference type".</span></span>

## <a name="type-inference"></a><span data-ttu-id="5cfee-136">类型推理</span><span class="sxs-lookup"><span data-stu-id="5cfee-136">Type inference</span></span>

<span data-ttu-id="5cfee-137">在类型推理中，如果贡献类型是空引用类型，则生成的类型应为空。</span><span class="sxs-lookup"><span data-stu-id="5cfee-137">In type inference, if a contributing type is a nullable reference type, the resulting type should be nullable.</span></span> <span data-ttu-id="5cfee-138">换句话说，nullin 是传播的。</span><span class="sxs-lookup"><span data-stu-id="5cfee-138">In other words, nullness is propagated.</span></span>

<span data-ttu-id="5cfee-139">我们应该考虑作为参与表达式`null`的字面是否应导致无效。</span><span class="sxs-lookup"><span data-stu-id="5cfee-139">We should consider whether the `null` literal as a participating expression should contribute nullness.</span></span> <span data-ttu-id="5cfee-140">它今天不是：对于值类型，它会导致错误，而对于引用类型，null 成功转换为普通类型。</span><span class="sxs-lookup"><span data-stu-id="5cfee-140">It doesn't today: for value types it leads to an error, whereas for reference types the null successfully converts to the plain type.</span></span>

```csharp
string? n = "world";
var x = b ? "Hello" : n; // string?
var y = b ? "Hello" : null; // string? or error
var z = b ? 7 : null; // Error today, could be int?
```

## <a name="null-guard-guidance"></a><span data-ttu-id="5cfee-141">空防护制导</span><span class="sxs-lookup"><span data-stu-id="5cfee-141">Null guard guidance</span></span>

<span data-ttu-id="5cfee-142">作为一项功能，可无效的引用类型允许开发人员表达其意图，并在该意图与该意图相矛盾时通过流分析发出警告。</span><span class="sxs-lookup"><span data-stu-id="5cfee-142">As a feature, nullable reference types allow developers to express their intent, and provide warnings through flow analysis if that intent is contradicted.</span></span> <span data-ttu-id="5cfee-143">对于是否有必要进行零空防护，存在一个常见的问题。</span><span class="sxs-lookup"><span data-stu-id="5cfee-143">There is a common question as to whether or not null guards are necessary.</span></span>

### <a name="example-of-null-guard"></a><span data-ttu-id="5cfee-144">空防护示例</span><span class="sxs-lookup"><span data-stu-id="5cfee-144">Example of null guard</span></span>

```csharp
public void DoWork(Worker worker)
{
    // Guard against worker being null
    if (worker is null)
    {
        throw new ArgumentNullException(nameof(worker));
    }

    // Otherwise use worker argument
}
```

<span data-ttu-id="5cfee-145">在前面的示例中，函数`DoWork`接受 和`Worker`，防止它可能是 。 `null`</span><span class="sxs-lookup"><span data-stu-id="5cfee-145">In the previous example, the `DoWork` function accepts a `Worker` and guards against it potentially being `null`.</span></span> <span data-ttu-id="5cfee-146">如果`worker`参数为`null`，则`DoWork`函数将`throw`。</span><span class="sxs-lookup"><span data-stu-id="5cfee-146">If the `worker` argument is `null`, the `DoWork` function will `throw`.</span></span> <span data-ttu-id="5cfee-147">对于可 null 引用类型，上一示例中的代码使`Worker`参数的用意*不是*`null`。</span><span class="sxs-lookup"><span data-stu-id="5cfee-147">With nullable reference types, the code in the previous example makes the intent that the `Worker` parameter would *not* be `null`.</span></span> <span data-ttu-id="5cfee-148">如果`DoWork`函数是公共 API（如 NuGet 包或共享库），则作为指南，应保留空防护装置。</span><span class="sxs-lookup"><span data-stu-id="5cfee-148">If the `DoWork` function was a public API, such as a NuGet package or a shared library - as guidance you should leave null guards in place.</span></span> <span data-ttu-id="5cfee-149">作为公共 API，呼叫者不传递`null`的唯一保证是防止它。</span><span class="sxs-lookup"><span data-stu-id="5cfee-149">As a public API, the only guarantee that a caller isn't passing `null` is to guard against it.</span></span>

### <a name="express-intent"></a><span data-ttu-id="5cfee-150">明确意图</span><span class="sxs-lookup"><span data-stu-id="5cfee-150">Express intent</span></span>

<span data-ttu-id="5cfee-151">在前面的例子中更令人信服的用法是表示`Worker`参数可以是`null`，从而使空保护更合适。</span><span class="sxs-lookup"><span data-stu-id="5cfee-151">A more compelling use of the previous example is to express that the `Worker` parameter could be `null`, thus making the null guard more appropriate.</span></span> <span data-ttu-id="5cfee-152">如果在下面的示例中删除了 null 保护，编译器将警告您可能正在取消引用 null。</span><span class="sxs-lookup"><span data-stu-id="5cfee-152">If you remove the null guard in the following example, the compiler warns that you may be dereferencing null.</span></span> <span data-ttu-id="5cfee-153">不管怎样，两个空防护仍然有效。</span><span class="sxs-lookup"><span data-stu-id="5cfee-153">Regardless, both null guards are still valid.</span></span>

```csharp
public void DoWork(Worker? worker)
{
    // Guard against worker being null
    if (worker is null)
    {
        throw new ArgumentNullException(nameof(worker));
    }

    // Otherwise use worker argument
}
```

<span data-ttu-id="5cfee-154">对于非公共 API（如完全由开发人员或开发团队控制的源代码），可取消的引用类型可以允许安全删除空防护，开发人员可以保证不需要。</span><span class="sxs-lookup"><span data-stu-id="5cfee-154">For non-public APIs, such as source code entirely in control by a developer or dev team - the nullable reference types could allow for the safe removal of null guards where the developers can guarantee it is not necessary.</span></span> <span data-ttu-id="5cfee-155">此功能可以帮助处理警告，但它不能保证在运行时执行代码可能会导致 。 `NullReferenceException`</span><span class="sxs-lookup"><span data-stu-id="5cfee-155">The feature can help with warnings, but it cannot guarantee that at runtime code execution could result in a `NullReferenceException`.</span></span>

## <a name="breaking-changes"></a><span data-ttu-id="5cfee-156">中断性变更</span><span class="sxs-lookup"><span data-stu-id="5cfee-156">Breaking changes</span></span>

<span data-ttu-id="5cfee-157">非空警告是现有代码的明显突破更改，应附带选择加入机制。</span><span class="sxs-lookup"><span data-stu-id="5cfee-157">Non-null warnings are an obvious breaking change on existing code, and should be accompanied with an opt-in mechanism.</span></span>

<span data-ttu-id="5cfee-158">更明显的是，在可消除性为隐式的特定方案中，来自可撤销类型的警告（如上所述）是对现有代码的一种重大更改：</span><span class="sxs-lookup"><span data-stu-id="5cfee-158">Less obviously, warnings from nullable types (as described above) are a breaking change on existing code in certain scenarios where the nullability is implicit:</span></span>

* <span data-ttu-id="5cfee-159">无约束类型参数将被视为隐式空参数，因此将它们分配给`object`或访问这些参数`ToString`将产生警告。</span><span class="sxs-lookup"><span data-stu-id="5cfee-159">Unconstrained type parameters will be treated as implicitly nullable, so assigning them to `object` or accessing e.g. `ToString` will yield warnings.</span></span>
* <span data-ttu-id="5cfee-160">如果类型推理推断表达式中的`null`空，则现有代码有时会生成空类型，而不是非空类型，这可能导致新的警告。</span><span class="sxs-lookup"><span data-stu-id="5cfee-160">if type inference infers nullness from `null` expressions, then existing code will sometimes yield nullable rather than non-nullable types, which can lead to new warnings.</span></span>

<span data-ttu-id="5cfee-161">因此，可撤销的警告也需要可选</span><span class="sxs-lookup"><span data-stu-id="5cfee-161">So nullable warnings also need to be optional</span></span>

<span data-ttu-id="5cfee-162">最后，向现有 API 添加注释对选择在警告中的用户升级库时将是一个重大更改。</span><span class="sxs-lookup"><span data-stu-id="5cfee-162">Finally, adding annotations to an existing API will be a breaking change to users who have opted in to warnings, when they upgrade the library.</span></span> <span data-ttu-id="5cfee-163">这也是值得选择加入或退出的能力。"我想要错误修复，但我不准备处理它们的新注释"</span><span class="sxs-lookup"><span data-stu-id="5cfee-163">This, too, merits the ability to opt in or out. "I want the bug fixes, but I am not ready to deal with their new annotations"</span></span>

<span data-ttu-id="5cfee-164">总之，您需要能够选择加入/退出：</span><span class="sxs-lookup"><span data-stu-id="5cfee-164">In summary, you need to be able to opt in/out of:</span></span>
* <span data-ttu-id="5cfee-165">空警告</span><span class="sxs-lookup"><span data-stu-id="5cfee-165">Nullable warnings</span></span>
* <span data-ttu-id="5cfee-166">非空警告</span><span class="sxs-lookup"><span data-stu-id="5cfee-166">Non-null warnings</span></span>
* <span data-ttu-id="5cfee-167">来自其他文件中注释的警告</span><span class="sxs-lookup"><span data-stu-id="5cfee-167">Warnings from annotations in other files</span></span>

<span data-ttu-id="5cfee-168">选择加入的粒度暗示了类似分析器的模型，其中大量代码可以选择使用实用模式输入和退出，用户可以选择严重性级别。</span><span class="sxs-lookup"><span data-stu-id="5cfee-168">The granularity of the opt-in suggests an analyzer-like model, where swaths of code can opt in and out with pragmas and severity levels can be chosen by the user.</span></span> <span data-ttu-id="5cfee-169">此外，每个库选项（"忽略JSON.NET注释，直到我准备好处理掉号"）可以在代码中作为属性表示。</span><span class="sxs-lookup"><span data-stu-id="5cfee-169">Additionally, per-library options ("ignore the annotations from JSON.NET until I'm ready to deal with the fall out") may be expressible in code as attributes.</span></span>

<span data-ttu-id="5cfee-170">选择加入/过渡体验的设计对于此功能的成功和有用性至关重要。</span><span class="sxs-lookup"><span data-stu-id="5cfee-170">The design of the opt-in/transition experience is crucial to the success and usefulness of this feature.</span></span> <span data-ttu-id="5cfee-171">我们需要确保：</span><span class="sxs-lookup"><span data-stu-id="5cfee-171">We need to make sure that:</span></span>

* <span data-ttu-id="5cfee-172">用户可以根据需要逐步采用无效检查</span><span class="sxs-lookup"><span data-stu-id="5cfee-172">Users can adopt nullability checking gradually as they want to</span></span>
* <span data-ttu-id="5cfee-173">库作者可以添加空注释，而不必担心破坏客户</span><span class="sxs-lookup"><span data-stu-id="5cfee-173">Library authors can add nullability annotations without fear of breaking customers</span></span>
* <span data-ttu-id="5cfee-174">尽管如此，没有"配置噩梦"的感觉</span><span class="sxs-lookup"><span data-stu-id="5cfee-174">Despite these, there is not a sense of "configuration nightmare"</span></span>

## <a name="tweaks"></a><span data-ttu-id="5cfee-175">调整</span><span class="sxs-lookup"><span data-stu-id="5cfee-175">Tweaks</span></span>

<span data-ttu-id="5cfee-176">我们可以考虑不使用局部变量的`?`注释，而只观察它们是否按照分配给他们的内容使用。</span><span class="sxs-lookup"><span data-stu-id="5cfee-176">We could consider not using the `?` annotations on locals, but just observing whether they are used in accordance with what gets assigned to them.</span></span> <span data-ttu-id="5cfee-177">我不喜欢这样，我认为我们应该统一地让人们表达他们的意图。</span><span class="sxs-lookup"><span data-stu-id="5cfee-177">I don't favor this; I think we should uniformly let people express their intent.</span></span>

<span data-ttu-id="5cfee-178">我们可以考虑参数的速记`T! x`，即自动生成运行时空检查。</span><span class="sxs-lookup"><span data-stu-id="5cfee-178">We could consider a shorthand `T! x` on parameters, that auto-generates a runtime null check.</span></span>

<span data-ttu-id="5cfee-179">泛型类型（如`FirstOrDefault`或`TryGet`）上的某些模式具有具有非空类型参数的稍微奇怪的行为，因为它们在某些情况下显式生成默认值。</span><span class="sxs-lookup"><span data-stu-id="5cfee-179">Certain patterns on generic types, such as `FirstOrDefault` or `TryGet`, have slightly weird behavior with non-nullable type arguments, because they explicitly yield default values in certain situations.</span></span> <span data-ttu-id="5cfee-180">我们可以尝试对类型系统进行细微差别，以更好地适应这些类型系统。</span><span class="sxs-lookup"><span data-stu-id="5cfee-180">We could try to nuance the type system to accommodate these better.</span></span> <span data-ttu-id="5cfee-181">例如，我们可以允许`?`无约束类型参数，即使类型参数已经为空。</span><span class="sxs-lookup"><span data-stu-id="5cfee-181">For instance, we could allow `?` on unconstrained type parameters, even though the type argument could already be nullable.</span></span> <span data-ttu-id="5cfee-182">我怀疑它是值得的，它导致与空*值*类型的交互相关的怪异。</span><span class="sxs-lookup"><span data-stu-id="5cfee-182">I doubt that it is worth it, and it leads to weirdness related to interaction with nullable *value* types.</span></span> 

## <a name="nullable-value-types"></a><span data-ttu-id="5cfee-183">可以为 null 的值类型</span><span class="sxs-lookup"><span data-stu-id="5cfee-183">Nullable value types</span></span>

<span data-ttu-id="5cfee-184">我们可以考虑对空值类型采用上述一些语义。</span><span class="sxs-lookup"><span data-stu-id="5cfee-184">We could consider adopting some of the above semantics for nullable value types as well.</span></span>

<span data-ttu-id="5cfee-185">我们已经提到类型推理，我们可以`int?`从`(7, null)`推断，而不是只给出一个错误。</span><span class="sxs-lookup"><span data-stu-id="5cfee-185">We already mentioned type inference, where we could infer `int?` from `(7, null)`, instead of just giving an error.</span></span>

<span data-ttu-id="5cfee-186">另一个机会是将流分析应用于空值类型。</span><span class="sxs-lookup"><span data-stu-id="5cfee-186">Another opportunity is to apply the flow analysis to nullable value types.</span></span> <span data-ttu-id="5cfee-187">当它们被视为非空时，我们实际上可以允许以特定方式用作非空类型（例如成员访问）。</span><span class="sxs-lookup"><span data-stu-id="5cfee-187">When they are deemed non-null, we could actually allow using as the non-nullable type in certain ways (e.g. member access).</span></span> <span data-ttu-id="5cfee-188">我们只需要小心，你*已经在*空值类型上可以做的事情将是首选的，因为背对了的原因。</span><span class="sxs-lookup"><span data-stu-id="5cfee-188">We just have to be careful that the things that you can *already* do on a nullable value type will be preferred, for back compat reasons.</span></span>
