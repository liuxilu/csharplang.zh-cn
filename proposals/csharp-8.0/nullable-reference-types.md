---
ms.openlocfilehash: ecdad8c863d0695bc901e4d96d9ca3decbc248eb
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483594"
---
# <a name="nullable-reference-types-in-c"></a><span data-ttu-id="bbb66-101">中的可以为 null 的引用类型C#</span><span class="sxs-lookup"><span data-stu-id="bbb66-101">Nullable reference types in C#</span></span> #

<span data-ttu-id="bbb66-102">此功能的目标是：</span><span class="sxs-lookup"><span data-stu-id="bbb66-102">The goal of this feature is to:</span></span>

* <span data-ttu-id="bbb66-103">允许开发人员表达引用类型的变量、参数或结果是否应为空。</span><span class="sxs-lookup"><span data-stu-id="bbb66-103">Allow developers to express whether a variable, parameter or result of a reference type is intended to be null or not.</span></span>
* <span data-ttu-id="bbb66-104">当不根据此目的使用此类变量、参数和结果时提供警告。</span><span class="sxs-lookup"><span data-stu-id="bbb66-104">Provide warnings when such variables, parameters and results are not used according to that intent.</span></span>

## <a name="expression-of-intent"></a><span data-ttu-id="bbb66-105">意向表达式</span><span class="sxs-lookup"><span data-stu-id="bbb66-105">Expression of intent</span></span>

<span data-ttu-id="bbb66-106">该语言已包含值类型的 `T?` 语法。</span><span class="sxs-lookup"><span data-stu-id="bbb66-106">The language already contains the `T?` syntax for value types.</span></span> <span data-ttu-id="bbb66-107">向引用类型扩展此语法非常简单。</span><span class="sxs-lookup"><span data-stu-id="bbb66-107">It is straightforward to extend this syntax to reference types.</span></span>

<span data-ttu-id="bbb66-108">假设未修饰引用类型 `T` 的目的是将其视为非 null。</span><span class="sxs-lookup"><span data-stu-id="bbb66-108">It is assumed that the intent of an unadorned reference type `T` is for it to be non-null.</span></span>

## <a name="checking-of-nullable-references"></a><span data-ttu-id="bbb66-109">检查可以为 null 的引用</span><span class="sxs-lookup"><span data-stu-id="bbb66-109">Checking of nullable references</span></span>

<span data-ttu-id="bbb66-110">流分析跟踪可以为 null 的引用变量。</span><span class="sxs-lookup"><span data-stu-id="bbb66-110">A flow analysis tracks nullable reference variables.</span></span> <span data-ttu-id="bbb66-111">如果分析认为它们不会为 null （例如，在检查或分配之后），则将其值视为非空引用。</span><span class="sxs-lookup"><span data-stu-id="bbb66-111">Where the analysis deems that they would not be null (e.g. after a check or an assignment), their value will be considered a non-null reference.</span></span>

<span data-ttu-id="bbb66-112">如果流分析无法建立开发人员知道的非空情况，则可以使用后缀 `x!` 运算符（"dammit" 运算符）将可为 null 的引用显式视为非 null。</span><span class="sxs-lookup"><span data-stu-id="bbb66-112">A nullable reference can also explicitly be treated as non-null with the postfix `x!` operator (the "dammit" operator), for when flow analysis cannot establish a non-null situation that the developer knows is there.</span></span>

<span data-ttu-id="bbb66-113">否则，如果取消引用可为 null 的引用或者将其转换为非 null 类型，则会发出警告。</span><span class="sxs-lookup"><span data-stu-id="bbb66-113">Otherwise, a warning is given if a nullable reference is dereferenced, or is converted to a non-null type.</span></span>

<span data-ttu-id="bbb66-114">从 `S[]` 转换到 `T?[]` 以及从 `S?[]` 转换到 `T[]`时，将发出警告。</span><span class="sxs-lookup"><span data-stu-id="bbb66-114">A warning is given when converting from `S[]` to `T?[]` and from `S?[]` to `T[]`.</span></span>

<span data-ttu-id="bbb66-115">从 `C<S>` 转换到 `C<T?>` 时，除了类型参数是协变的（`out`）以及在从 `C<S?>` 转换为 `C<T>` 时（在类型参数为逆变（`in`）时除外），会发出警告。</span><span class="sxs-lookup"><span data-stu-id="bbb66-115">A warning is given when converting from `C<S>` to `C<T?>` except when the type parameter is covariant (`out`), and when converting from `C<S?>` to `C<T>` except when the type parameter is contravariant (`in`).</span></span>

<span data-ttu-id="bbb66-116">如果类型参数具有非 null 约束，则 `C<T?>` 上提供警告。</span><span class="sxs-lookup"><span data-stu-id="bbb66-116">A warning is given on `C<T?>` if the type parameter has non-null constraints.</span></span> 

## <a name="checking-of-non-null-references"></a><span data-ttu-id="bbb66-117">检查非空引用</span><span class="sxs-lookup"><span data-stu-id="bbb66-117">Checking of non-null references</span></span>

<span data-ttu-id="bbb66-118">如果将 null 文本分配给非 null 变量或作为非 null 参数传递，则会发出警告。</span><span class="sxs-lookup"><span data-stu-id="bbb66-118">A warning is given if a null literal is assigned to a non-null variable or passed as a non-null parameter.</span></span>

<span data-ttu-id="bbb66-119">如果构造函数未显式初始化非空引用字段，则还会发出警告。</span><span class="sxs-lookup"><span data-stu-id="bbb66-119">A warning is also given if a constructor does not explicitly initialize non-null reference fields.</span></span>

<span data-ttu-id="bbb66-120">我们无法充分跟踪非空引用数组的所有元素是否已初始化。</span><span class="sxs-lookup"><span data-stu-id="bbb66-120">We cannot adequately track that all elements of an array of non-null references are initialized.</span></span> <span data-ttu-id="bbb66-121">但是，如果在从或传入数组之前没有将新创建的数组的元素分配给，我们可能会发出警告。</span><span class="sxs-lookup"><span data-stu-id="bbb66-121">However, we could issue a warning if no element of a newly created array is assigned to before the array is read from or passed on.</span></span> <span data-ttu-id="bbb66-122">这可能会处理常见情况，而不会干扰。</span><span class="sxs-lookup"><span data-stu-id="bbb66-122">That might handle the common case without being too noisy.</span></span>

<span data-ttu-id="bbb66-123">我们需要决定 `default(T)` 生成警告，还是只将其视为 `T?`类型。</span><span class="sxs-lookup"><span data-stu-id="bbb66-123">We need to decide whether `default(T)` generates a warning, or is simply treated as being of the type `T?`.</span></span>

## <a name="metadata-representation"></a><span data-ttu-id="bbb66-124">元数据表示形式</span><span class="sxs-lookup"><span data-stu-id="bbb66-124">Metadata representation</span></span>

<span data-ttu-id="bbb66-125">应为空性修饰在元数据中表示为属性。</span><span class="sxs-lookup"><span data-stu-id="bbb66-125">Nullability adornments should be represented in metadata as attributes.</span></span> <span data-ttu-id="bbb66-126">这意味着下层编译器将忽略它们。</span><span class="sxs-lookup"><span data-stu-id="bbb66-126">This means that downlevel compilers will ignore them.</span></span>

<span data-ttu-id="bbb66-127">我们需要确定是否只包含可为 null 的批注，或者程序集中是否存在非 null 的指示。</span><span class="sxs-lookup"><span data-stu-id="bbb66-127">We need to decide if only nullable annotations are included, or there's also some indication of whether non-null was "on" in the assembly.</span></span>

## <a name="generics"></a><span data-ttu-id="bbb66-128">泛型</span><span class="sxs-lookup"><span data-stu-id="bbb66-128">Generics</span></span>

<span data-ttu-id="bbb66-129">如果某个类型参数 `T` 具有不可为 null 的约束，则该类型参数在其作用域内被视为不可为 null。</span><span class="sxs-lookup"><span data-stu-id="bbb66-129">If a type parameter `T` has non-nullable constraints, it is treated as non-nullable within its scope.</span></span>

<span data-ttu-id="bbb66-130">如果类型参数不受约束或只有可为 null 的约束，则这种情况稍微复杂一些：这意味着相应的类型参数可以*为 null，也可以是*不可为 null 的。</span><span class="sxs-lookup"><span data-stu-id="bbb66-130">If a type parameter is unconstrained or has only nullable constraints, the situation is a little more complex: this means that the corresponding type argument could be *either* nullable or non-nullable.</span></span> <span data-ttu-id="bbb66-131">在这种情况下要执行的安全操作是将类型*参数视为可以为 null 和*不可为 null，并在违反时发出警告。</span><span class="sxs-lookup"><span data-stu-id="bbb66-131">The safe thing to do in that situation is to treat the type parameter as *both* nullable and non-nullable, giving warnings when either is violated.</span></span> 

<span data-ttu-id="bbb66-132">需要考虑是否应允许显式可为 null 的引用约束。</span><span class="sxs-lookup"><span data-stu-id="bbb66-132">It is worth considering whether explicit nullable reference constraints should be allowed.</span></span> <span data-ttu-id="bbb66-133">但请注意，在某些情况下，我们无法避免将可为 null 的引用类型*隐式*作为约束（继承的约束）。</span><span class="sxs-lookup"><span data-stu-id="bbb66-133">Note, however, that we cannot avoid having nullable reference types *implicitly* be constraints in certain cases (inherited constraints).</span></span>

<span data-ttu-id="bbb66-134">`class` 约束为非 null。</span><span class="sxs-lookup"><span data-stu-id="bbb66-134">The `class` constraint is non-null.</span></span> <span data-ttu-id="bbb66-135">我们可以考虑 `class?` 是否应该是表示 "可以为 null 的引用类型" 的有效可为 null 的约束。</span><span class="sxs-lookup"><span data-stu-id="bbb66-135">We can consider whether `class?` should be a valid nullable constraint denoting "nullable reference type".</span></span>

## <a name="type-inference"></a><span data-ttu-id="bbb66-136">类型推理</span><span class="sxs-lookup"><span data-stu-id="bbb66-136">Type inference</span></span>

<span data-ttu-id="bbb66-137">在类型推理中，如果参与类型是可以为 null 的引用类型，则结果类型应为 null。</span><span class="sxs-lookup"><span data-stu-id="bbb66-137">In type inference, if a contributing type is a nullable reference type, the resulting type should be nullable.</span></span> <span data-ttu-id="bbb66-138">换句话说，非 null 是传播的。</span><span class="sxs-lookup"><span data-stu-id="bbb66-138">In other words, nullness is propagated.</span></span>

<span data-ttu-id="bbb66-139">应考虑 `null` 文本是否为参与表达式应非 null。</span><span class="sxs-lookup"><span data-stu-id="bbb66-139">We should consider whether the `null` literal as a participating expression should contribute nullness.</span></span> <span data-ttu-id="bbb66-140">目前不是：对于值类型，它会导致错误，而对于引用类型，null 成功转换为纯类型。</span><span class="sxs-lookup"><span data-stu-id="bbb66-140">It doesn't today: for value types it leads to an error, whereas for reference types the null successfully converts to the plain type.</span></span> 

```csharp
string? n = "world";
var x = b ? "Hello" : n; // string?
var y = b ? "Hello" : null; // string? or error
var z = b ? 7 : null; // Error today, could be int?
```

## <a name="breaking-changes"></a><span data-ttu-id="bbb66-141">重大更改</span><span class="sxs-lookup"><span data-stu-id="bbb66-141">Breaking changes</span></span>

<span data-ttu-id="bbb66-142">非 null 警告是对现有代码的明显重大更改，并应附带有选择的机制。</span><span class="sxs-lookup"><span data-stu-id="bbb66-142">Non-null warnings are an obvious breaking change on existing code, and should be accompanied with an opt-in mechanism.</span></span>

<span data-ttu-id="bbb66-143">不太明显，可以为 null 的类型的警告（如上所述）在可为 null 性为隐式的某些情况下，对现有代码进行重大更改：</span><span class="sxs-lookup"><span data-stu-id="bbb66-143">Less obviously, warnings from nullable types (as described above) are a breaking change on existing code in certain scenarios where the nullability is implicit:</span></span>

* <span data-ttu-id="bbb66-144">不受约束的类型参数将被视为隐式的，因此将其分配给 `object` 或访问，例如 `ToString` 将产生警告。</span><span class="sxs-lookup"><span data-stu-id="bbb66-144">Unconstrained type parameters will be treated as implicitly nullable, so assigning them to `object` or accessing e.g. `ToString` will yield warnings.</span></span>
* <span data-ttu-id="bbb66-145">如果类型推理从 `null` 表达式推断出非 null，则现有代码有时会产生可为 null 的类型，而不是不可为 null 的类型，这可能会导致新的警告。</span><span class="sxs-lookup"><span data-stu-id="bbb66-145">if type inference infers nullness from `null` expressions, then existing code will sometimes yield nullable rather than non-nullable types, which can lead to new warnings.</span></span>

<span data-ttu-id="bbb66-146">因此，可以为 null 的警告也需要是可选的</span><span class="sxs-lookup"><span data-stu-id="bbb66-146">So nullable warnings also need to be optional</span></span>

<span data-ttu-id="bbb66-147">最后，将批注添加到现有 API 将是在升级库时选择了警告的用户的重大更改。</span><span class="sxs-lookup"><span data-stu-id="bbb66-147">Finally, adding annotations to an existing API will be a breaking change to users who have opted in to warnings, when they upgrade the library.</span></span> <span data-ttu-id="bbb66-148">这也可以选择加入或退出。"我想修复 bug，但未准备好处理其新注释"</span><span class="sxs-lookup"><span data-stu-id="bbb66-148">This, too, merits the ability to opt in or out. "I want the bug fixes, but I am not ready to deal with their new annotations"</span></span>

<span data-ttu-id="bbb66-149">总之，你需要能够选择加入/退出：</span><span class="sxs-lookup"><span data-stu-id="bbb66-149">In summary, you need to be able to opt in/out of:</span></span>
* <span data-ttu-id="bbb66-150">可以为 null 的警告</span><span class="sxs-lookup"><span data-stu-id="bbb66-150">Nullable warnings</span></span>
* <span data-ttu-id="bbb66-151">非 null 警告</span><span class="sxs-lookup"><span data-stu-id="bbb66-151">Non-null warnings</span></span>
* <span data-ttu-id="bbb66-152">其他文件中的批注警告</span><span class="sxs-lookup"><span data-stu-id="bbb66-152">Warnings from annotations in other files</span></span>

<span data-ttu-id="bbb66-153">选择的粒度建议使用类似于分析器的模型，在该模型中，大量的代码可以选择使用杂注，而用户可以选择严重级别。</span><span class="sxs-lookup"><span data-stu-id="bbb66-153">The granularity of the opt-in suggests an analyzer-like model, where swaths of code can opt in and out with pragmas and severity levels can be chosen by the user.</span></span> <span data-ttu-id="bbb66-154">此外，每个库选项（"从 JSON.NET 中忽略批注，直到我准备好处理"）可以在代码中作为属性来表达。</span><span class="sxs-lookup"><span data-stu-id="bbb66-154">Additionally, per-library options ("ignore the annotations from JSON.NET until I'm ready to deal with the fall out") may be expressible in code as attributes.</span></span>

<span data-ttu-id="bbb66-155">选择加入/过渡体验的设计对于此功能的成功和有用性至关重要。</span><span class="sxs-lookup"><span data-stu-id="bbb66-155">The design of the opt-in/transition experience is crucial to the success and usefulness of this feature.</span></span> <span data-ttu-id="bbb66-156">我们需要确保：</span><span class="sxs-lookup"><span data-stu-id="bbb66-156">We need to make sure that:</span></span>

* <span data-ttu-id="bbb66-157">用户可以根据需要逐步采用空性检查</span><span class="sxs-lookup"><span data-stu-id="bbb66-157">Users can adopt nullability checking gradually as they want to</span></span>
* <span data-ttu-id="bbb66-158">库作者可以添加可为 null 性的注释，而无需担心用户中断</span><span class="sxs-lookup"><span data-stu-id="bbb66-158">Library authors can add nullability annotations without fear of breaking customers</span></span>
* <span data-ttu-id="bbb66-159">尽管如此，这并不是 "配置不足"</span><span class="sxs-lookup"><span data-stu-id="bbb66-159">Despite these, there is not a sense of "configuration nightmare"</span></span>

## <a name="tweaks"></a><span data-ttu-id="bbb66-160">调整</span><span class="sxs-lookup"><span data-stu-id="bbb66-160">Tweaks</span></span>

<span data-ttu-id="bbb66-161">我们可以考虑不在局部变量上使用 `?` 注释，而只是根据分配给它们的内容来观察是否使用它们。</span><span class="sxs-lookup"><span data-stu-id="bbb66-161">We could consider not using the `?` annotations on locals, but just observing whether they are used in accordance with what gets assigned to them.</span></span> <span data-ttu-id="bbb66-162">我不愿意这样做;我想我们应该统一让人们表达自己的意图。</span><span class="sxs-lookup"><span data-stu-id="bbb66-162">I don't favor this; I think we should uniformly let people express their intent.</span></span>

<span data-ttu-id="bbb66-163">对于参数，可以考虑使用简写 `T! x` 来自动生成运行时 null 检查。</span><span class="sxs-lookup"><span data-stu-id="bbb66-163">We could consider a shorthand `T! x` on parameters, that auto-generates a runtime null check.</span></span>

<span data-ttu-id="bbb66-164">某些泛型类型上的特定模式（如 `FirstOrDefault` 或 `TryGet`）具有不可以为 null 的类型参数的轻微行为，因为在某些情况下它们会显式生成默认值。</span><span class="sxs-lookup"><span data-stu-id="bbb66-164">Certain patterns on generic types, such as `FirstOrDefault` or `TryGet`, have slightly weird behavior with non-nullable type arguments, because they explicitly yield default values in certain situations.</span></span> <span data-ttu-id="bbb66-165">我们可以尝试对类型系统进行细微差别，以使其更好地适应。</span><span class="sxs-lookup"><span data-stu-id="bbb66-165">We could try to nuance the type system to accommodate these better.</span></span> <span data-ttu-id="bbb66-166">例如，我们可以允许对不受约束的类型参数进行 `?`，即使类型参数可以是可以为 null 的。</span><span class="sxs-lookup"><span data-stu-id="bbb66-166">For instance, we could allow `?` on unconstrained type parameters, even though the type argument could already be nullable.</span></span> <span data-ttu-id="bbb66-167">我怀疑这是值得的，这会导致与可为 null 的*值*类型的交互相关的问题。</span><span class="sxs-lookup"><span data-stu-id="bbb66-167">I doubt that it is worth it, and it leads to weirdness related to interaction with nullable *value* types.</span></span> 

## <a name="nullable-value-types"></a><span data-ttu-id="bbb66-168">可以为 null 的值类型</span><span class="sxs-lookup"><span data-stu-id="bbb66-168">Nullable value types</span></span>

<span data-ttu-id="bbb66-169">对于可以为 null 的值类型，我们可以考虑采用上述语义。</span><span class="sxs-lookup"><span data-stu-id="bbb66-169">We could consider adopting some of the above semantics for nullable value types as well.</span></span>

<span data-ttu-id="bbb66-170">我们已提到类型推理，可在其中推断 `(7, null)``int?`，而不是仅仅提供错误。</span><span class="sxs-lookup"><span data-stu-id="bbb66-170">We already mentioned type inference, where we could infer `int?` from `(7, null)`, instead of just giving an error.</span></span>

<span data-ttu-id="bbb66-171">另一种机会是将流分析应用于可为 null 的值类型。</span><span class="sxs-lookup"><span data-stu-id="bbb66-171">Another opportunity is to apply the flow analysis to nullable value types.</span></span> <span data-ttu-id="bbb66-172">如果被视为非 null，我们实际上可以允许在某些方面（例如，成员访问）使用作为不可为 null 的类型。</span><span class="sxs-lookup"><span data-stu-id="bbb66-172">When they are deemed non-null, we could actually allow using as the non-nullable type in certain ways (e.g. member access).</span></span> <span data-ttu-id="bbb66-173">我们只需小心，你可以在可以为 null 的值类型*上执行的操作就*是出于后向兼容的原因。</span><span class="sxs-lookup"><span data-stu-id="bbb66-173">We just have to be careful that the things that you can *already* do on a nullable value type will be preferred, for back compat reasons.</span></span>
