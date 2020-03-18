---
ms.openlocfilehash: 922353d043653ddb651534a01f3fb98f85cd756e
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483480"
---
# <a name="readonly-locals-and-parameters"></a><span data-ttu-id="850cb-101">readonly 局部变量和参数</span><span class="sxs-lookup"><span data-stu-id="850cb-101">readonly locals and parameters</span></span>

* <span data-ttu-id="850cb-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="850cb-102">[x] Proposed</span></span>
* <span data-ttu-id="850cb-103">[] 原型</span><span class="sxs-lookup"><span data-stu-id="850cb-103">[ ] Prototype</span></span>
* <span data-ttu-id="850cb-104">[] 实现</span><span class="sxs-lookup"><span data-stu-id="850cb-104">[ ] Implementation</span></span>
* <span data-ttu-id="850cb-105">[] 规范</span><span class="sxs-lookup"><span data-stu-id="850cb-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="850cb-106">总结</span><span class="sxs-lookup"><span data-stu-id="850cb-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="850cb-107">允许将局部变量和参数批注为 readonly，以防止这些局部变量和参数的浅变化。</span><span class="sxs-lookup"><span data-stu-id="850cb-107">Allow locals and parameters to be annotated as readonly in order to prevent shallow mutation of those locals and parameters.</span></span>

## <a name="motivation"></a><span data-ttu-id="850cb-108">动机</span><span class="sxs-lookup"><span data-stu-id="850cb-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="850cb-109">现在，可将 `readonly` 关键字应用于字段;这样做的效果是确保字段只能在构造过程中写入（静态构造，对于静态字段，对于实例字段则为实例构造），这有助于开发人员通过意外覆盖不应修改的状态来避免出错。</span><span class="sxs-lookup"><span data-stu-id="850cb-109">Today, the `readonly` keyword can be applied to fields; this has the effect of ensuring that a field can only be written to during construction (static construction in the case of a static field, or instance construction in the case of an instance field), which helps developers avoid mistakes by accidentally overwriting state which should not be modified.</span></span> <span data-ttu-id="850cb-110">不过，开发人员并不是要确保值不会改变的唯一地方。</span><span class="sxs-lookup"><span data-stu-id="850cb-110">But fields aren't the only places developers want to ensure that values aren't mutated.</span></span> <span data-ttu-id="850cb-111">特别是，通常会创建一个本地变量来存储临时状态，并意外更新该临时状态可能导致错误的计算和其他此类错误，尤其是在 lambda 中捕获此类 "局部变量" 时，它们会升级到字段，但目前没有办法将此类提升字段标记为 `readonly`。</span><span class="sxs-lookup"><span data-stu-id="850cb-111">In particular, it's common to create a local variable to store temporary state, and accidentally updating that temporary state can result in erroneous calculations and other such bugs, especially when such "locals" are captured in lambdas, at which point they are lifted to fields, but there's no way today to mark such lifted fields as `readonly`.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="850cb-112">详细设计</span><span class="sxs-lookup"><span data-stu-id="850cb-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="850cb-113">局部变量也将可批注为 `readonly`，编译器确保它们只在声明时设置（在中C# ，某些局部变量已经隐式只读，如 "foreach" 循环中的迭代变量或 "using" 块中已使用的变量，但目前开发人员无法将其他局部变量标记为 `readonly`）。</span><span class="sxs-lookup"><span data-stu-id="850cb-113">Locals will be annotatable as `readonly` as well, with the compiler ensuring that they're only set at the time of declaration (certain locals in C# are already implicitly readonly, such as the iteration variable in a 'foreach' loop or the used variable in a 'using' block, but currently a developer has no ability to mark other locals as `readonly`).</span></span> <span data-ttu-id="850cb-114">此类 `readonly` 局部变量必须有初始值设定项：</span><span class="sxs-lookup"><span data-stu-id="850cb-114">Such `readonly` locals must have an initializer:</span></span>

```csharp
readonly long maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

<span data-ttu-id="850cb-115">作为 `readonly var`的速记，可以使用现有的上下文关键字 `let`，例如</span><span class="sxs-lookup"><span data-stu-id="850cb-115">And as shorthand for `readonly var`, the existing contextual keyword `let` may be used, e.g.</span></span>

```csharp
let maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

<span data-ttu-id="850cb-116">对于初始值设定项，没有任何特殊约束，可以是当前作为局部变量的初始值设定项的任何内容，例如</span><span class="sxs-lookup"><span data-stu-id="850cb-116">There are no special constraints for what the initializer can be, and can be anything currently valid as an initializer for locals, e.g.</span></span>

```csharp
readonly T data = arg1 ?? arg2;
```

<span data-ttu-id="850cb-117">使用 lambda 和闭包时，局部变量上的 `readonly` 特别有用。</span><span class="sxs-lookup"><span data-stu-id="850cb-117">`readonly` on locals is particularly valuable when working with lambdas and closures.</span></span> <span data-ttu-id="850cb-118">当匿名方法或 lambda 访问封闭范围中的本地状态时，编译器将该状态捕获到闭包中，由 "显示类" 表示。</span><span class="sxs-lookup"><span data-stu-id="850cb-118">When an anonymous method or lambda accesses local state from the enclosing scope, that state is captured into a closure by the compiler, which is represented by a "display class."</span></span>  <span data-ttu-id="850cb-119">捕获的每个 "本地" 都是此类中的一个字段，但由于编译器以你的名义生成此字段，因此你不会有机会将其批注为 `readonly`，以便阻止 lambda 错误地写入 "local" （在引号中，因为它确实不是本地的，至少是在生成的 MSIL 中）。</span><span class="sxs-lookup"><span data-stu-id="850cb-119">Each "local" that's captured is a field in this class, yet because the compiler is generating this field on your behalf, you have no opportunity to annotate it as `readonly` in order to prevent the lambda from erroneously writing to the "local" (in quotes because it's really not a local, at least not in the resulting MSIL).</span></span> <span data-ttu-id="850cb-120">使用 `readonly` 局部变量，编译器可以阻止 lambda 向本地写入，这在涉及多线程的情况下特别有用，在这种情况下，错误写入可能会导致出现危险但难以发现的并发 bug。</span><span class="sxs-lookup"><span data-stu-id="850cb-120">With `readonly` locals, the compiler can prevent the lambda from writing to local, which is particularly valuable in scenarios involving multithreading where an erroneous write could result in a dangerous but rare and hard-to-find concurrency bug.</span></span>

```csharp
readonly long index = ...;
Parallel.ForEach(data, item => {
    T element = item[index];
    index = 0; // Error: can't assign to readonly locals outside of declaration
});
```

<span data-ttu-id="850cb-121">作为本地的一种特殊形式，参数也将可批注为 `readonly`。</span><span class="sxs-lookup"><span data-stu-id="850cb-121">As a special form of local, parameters will also be annotatable as `readonly`.</span></span> <span data-ttu-id="850cb-122">这不会影响该方法的调用方能够传递到参数的方式（就像在 `readonly` 字段中存储的值没有任何限制），而与任何 `readonly` 本地一样，编译器将禁止代码在声明后写入参数，这意味着禁止将方法的主体写入参数。</span><span class="sxs-lookup"><span data-stu-id="850cb-122">This would have no effect on what the caller of the method is able to pass to the parameter (just as there's no constraint on what values may be stored into a `readonly` field), but as with any `readonly` local, the compiler would prohibit code from writing to the parameter after declaration, which means the body of the method is prohibited from writing to the parameter.</span></span>

```csharp
public void Update(readonly int index = 0) // Default values are ok though not required
{
    ...
    index = 0; // Error: can't assign to readonly parameters
    ...
}
```

<span data-ttu-id="850cb-123">`readonly` 参数不会影响编译器为该方法发出的签名/元数据，只会影响编译器处理方法主体的编译的方式。</span><span class="sxs-lookup"><span data-stu-id="850cb-123">`readonly` parameters do not affect the signature/metadata emitted by the compiler for that method, and simply affect how the compiler handles the compilation of the method's body.</span></span> <span data-ttu-id="850cb-124">例如，基虚方法可以有 `readonly` 参数，并且该参数在重写中可以是可写的。</span><span class="sxs-lookup"><span data-stu-id="850cb-124">Thus, for example, a base virtual method could have a `readonly` parameter, and that parameter could be writable in an override.</span></span>

<span data-ttu-id="850cb-125">与字段一样，局部变量和参数 `readonly` 都是浅层，这会影响存储位置，但不会对对象图产生间接影响。</span><span class="sxs-lookup"><span data-stu-id="850cb-125">As with fields, `readonly` for locals and parameters is shallow, affecting the storage location but not transitively affecting the object graph.</span></span> <span data-ttu-id="850cb-126">但是，对于字段，调用 `readonly` 本地/参数结构的方法实际上将创建结构的副本并对副本调用方法，以避免 `this`的内部变化。</span><span class="sxs-lookup"><span data-stu-id="850cb-126">However, also as with fields, calling a method on a `readonly` local/parameter struct will actually make a copy of the struct and call the method on the copy, in order to avoid internal mutation of `this`.</span></span>

<span data-ttu-id="850cb-127">`readonly` 局部变量和参数不能作为 `ref` 或 `out` 参数传递，除非也支持 `ref readonly`。</span><span class="sxs-lookup"><span data-stu-id="850cb-127">`readonly` locals and parameters can't be passed as `ref` or `out` arguments, unless/until `ref readonly` is also supported.</span></span>

## <a name="alternatives"></a><span data-ttu-id="850cb-128">备选项</span><span class="sxs-lookup"><span data-stu-id="850cb-128">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="850cb-129">`val` 可以用作 `let`的替代简写。</span><span class="sxs-lookup"><span data-stu-id="850cb-129">`val` could be used as an alternative shorthand to `let`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="850cb-130">未解决的问题</span><span class="sxs-lookup"><span data-stu-id="850cb-130">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="850cb-131">`readonly ref` / `ref readonly` / `readonly ref readonly`：我已将有关如何处理 `ref readonly` 的问题留给了此建议。</span><span class="sxs-lookup"><span data-stu-id="850cb-131">`readonly ref` / `ref readonly` / `readonly ref readonly`: I've left the question of how to handle `ref readonly` as separate from this proposal.</span></span>
- <span data-ttu-id="850cb-132">此建议不会处理 readonly 结构/不可变类型。</span><span class="sxs-lookup"><span data-stu-id="850cb-132">This proposal does not tackle readonly structs / immutable types.</span></span> <span data-ttu-id="850cb-133">这留给了单独的提议。</span><span class="sxs-lookup"><span data-stu-id="850cb-133">That is left for a separate proposal.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="850cb-134">设计会议</span><span class="sxs-lookup"><span data-stu-id="850cb-134">Design meetings</span></span>

- <span data-ttu-id="850cb-135">2015年1月21日（<https://github.com/dotnet/roslyn/issues/98>）</span><span class="sxs-lookup"><span data-stu-id="850cb-135">Briefly discussed on Jan 21, 2015 (<https://github.com/dotnet/roslyn/issues/98>)</span></span>
