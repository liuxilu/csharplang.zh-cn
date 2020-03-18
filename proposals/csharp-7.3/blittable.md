---
ms.openlocfilehash: 11e9d21bda9e69be692c5c5f5aee80c2da1894ab
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483600"
---
# <a name="unmanaged-type-constraint"></a><span data-ttu-id="a16a1-101">非托管类型约束</span><span class="sxs-lookup"><span data-stu-id="a16a1-101">Unmanaged type constraint</span></span>

## <a name="summary"></a><span data-ttu-id="a16a1-102">总结</span><span class="sxs-lookup"><span data-stu-id="a16a1-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="a16a1-103">非托管约束功能将向C#语言规范中称为 "非托管类型" 的类型的类强制执行语言强制。 这在第18.2 节中定义为类型，该类型不是引用类型，并且在任何嵌套级别都不包含引用类型字段。</span><span class="sxs-lookup"><span data-stu-id="a16a1-103">The unmanaged constraint feature will give language enforcement to the class of types known as "unmanaged types" in the C# language spec.  This is defined in section 18.2 as a type which is not a reference type and doesn't contain reference type fields at any level of nesting.</span></span>  

## <a name="motivation"></a><span data-ttu-id="a16a1-104">动机</span><span class="sxs-lookup"><span data-stu-id="a16a1-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="a16a1-105">主要动机是使在中C#轻松编写低级别互操作代码。</span><span class="sxs-lookup"><span data-stu-id="a16a1-105">The primary motivation is to make it easier to author low level interop code in C#.</span></span> <span data-ttu-id="a16a1-106">非托管类型是互操作代码的核心构建基块之一，但缺乏泛型支持将无法跨所有非托管类型创建可重复使用的例程。</span><span class="sxs-lookup"><span data-stu-id="a16a1-106">Unmanaged types are one of the core building blocks for interop code, yet the lack of support in generics makes it impossible to create re-usable routines across all unmanaged types.</span></span> <span data-ttu-id="a16a1-107">开发人员会被迫为库中的每个非托管类型创作相同的样板板代码：</span><span class="sxs-lookup"><span data-stu-id="a16a1-107">Instead developers are forced to author the same boiler plate code for every unmanaged type in their library:</span></span>

```csharp
int Hash(Point point) { ... } 
int Hash(TimeSpan timeSpan) { ... } 
```

<span data-ttu-id="a16a1-108">若要启用此类型的方案，语言会引入新的约束：非托管：</span><span class="sxs-lookup"><span data-stu-id="a16a1-108">To enable this type of scenario the language will be introducing a new constraint: unmanaged:</span></span>

```csharp
void Hash<T>(T value) where T : unmanaged
{
    ...
}
```

<span data-ttu-id="a16a1-109">仅适用于C#语言规范中的非托管类型定义的类型才能满足此约束。查看它的另一种方法是，类型满足非托管约束 iff 它也可用作指针。</span><span class="sxs-lookup"><span data-stu-id="a16a1-109">This constraint can only be met by types which fit into the unmanaged type definition in the C# language spec. Another way of looking at it is that a type satisfies the unmanaged constraint iff it can also be used as a pointer.</span></span> 

```csharp
Hash(new Point()); // Okay 
Hash(42); // Okay
Hash("hello") // Error: Type string does not satisfy the unmanaged constraint
```

<span data-ttu-id="a16a1-110">具有非托管约束的类型参数可以使用所有可用于非托管类型的功能：指针、固定等。</span><span class="sxs-lookup"><span data-stu-id="a16a1-110">Type parameters with the unmanaged constraint can use all the features available to unmanaged types: pointers, fixed, etc ...</span></span> 

```csharp
void Hash<T>(T value) where T : unmanaged
{
    // Okay
    fixed (T* p = &value) 
    { 
        ...
    }
}
```

<span data-ttu-id="a16a1-111">此约束还会使结构化数据和字节流之间能够有效地进行转换。</span><span class="sxs-lookup"><span data-stu-id="a16a1-111">This constraint will also make it possible to have efficient conversions between structured data and streams of bytes.</span></span> <span data-ttu-id="a16a1-112">这是网络堆栈和序列化层中常见的一项操作：</span><span class="sxs-lookup"><span data-stu-id="a16a1-112">This is an operation that is common in networking stacks and serialization layers:</span></span>

```csharp
Span<byte> Convert<T>(ref T value) where T : unmanaged 
{
    ...
}
```

<span data-ttu-id="a16a1-113">这样的例程非常有用，因为它们在编译时是认定安全的，而分配是免费的。</span><span class="sxs-lookup"><span data-stu-id="a16a1-113">Such routines are advantageous because they are provably safe at compile time and allocation free.</span></span>  <span data-ttu-id="a16a1-114">目前，互操作性作者不能执行此操作（即使它位于性能至关重要的层）。</span><span class="sxs-lookup"><span data-stu-id="a16a1-114">Interop authors today can not do this (even though it's at a layer where perf is critical).</span></span>  <span data-ttu-id="a16a1-115">相反，他们需要依赖于分配具有昂贵运行时检查的例程来验证值是否正确地被管理。</span><span class="sxs-lookup"><span data-stu-id="a16a1-115">Instead they need to rely on allocating routines that have expensive runtime checks to verify values are correctly unmanaged.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="a16a1-116">详细设计</span><span class="sxs-lookup"><span data-stu-id="a16a1-116">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="a16a1-117">该语言将引入名为 `unmanaged`的新约束。</span><span class="sxs-lookup"><span data-stu-id="a16a1-117">The language will introduce a new constraint named `unmanaged`.</span></span> <span data-ttu-id="a16a1-118">为了满足此约束，类型必须为结构，并且类型的所有字段必须属于以下类别之一：</span><span class="sxs-lookup"><span data-stu-id="a16a1-118">In order to satisfy this constraint a type must be a struct and all the fields of the type must fall into one of the following categories:</span></span>

- <span data-ttu-id="a16a1-119">类型为 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`、`bool`、`IntPtr` 或 `UIntPtr`。</span><span class="sxs-lookup"><span data-stu-id="a16a1-119">Have the type `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `IntPtr` or `UIntPtr`.</span></span>
- <span data-ttu-id="a16a1-120">为任意 `enum` 类型。</span><span class="sxs-lookup"><span data-stu-id="a16a1-120">Be any `enum` type.</span></span>
- <span data-ttu-id="a16a1-121">是指针类型。</span><span class="sxs-lookup"><span data-stu-id="a16a1-121">Be a pointer type.</span></span>
- <span data-ttu-id="a16a1-122">是 satsifies `unmanaged` 约束的用户定义的结构。</span><span class="sxs-lookup"><span data-stu-id="a16a1-122">Be a user defined struct that satsifies the `unmanaged` constraint.</span></span>

<span data-ttu-id="a16a1-123">编译器生成的实例字段（如支持自动实现的属性的实例字段）还必须满足这些约束。</span><span class="sxs-lookup"><span data-stu-id="a16a1-123">Compiler generated instance fields, such as those backing auto-implemented properties, must also meet these constraints.</span></span> 

<span data-ttu-id="a16a1-124">例如：</span><span class="sxs-lookup"><span data-stu-id="a16a1-124">For example:</span></span>

```csharp
// Unmanaged type
struct Point 
{ 
    int X;
    int Y {get; set;}
}

// Not an unmanaged type
struct Student 
{ 
    string FirstName;
    string LastName;
}
``` 

<span data-ttu-id="a16a1-125">`unmanaged` 约束不能与 `struct`、`class` 或 `new()`组合。</span><span class="sxs-lookup"><span data-stu-id="a16a1-125">The `unmanaged` constraint cannot be combined with `struct`, `class` or `new()`.</span></span> <span data-ttu-id="a16a1-126">此限制派生自 `unmanaged` 隐含 `struct` 因此其他约束并无意义的事实。</span><span class="sxs-lookup"><span data-stu-id="a16a1-126">This restriction derives from the fact that `unmanaged` implies `struct` hence the other constraints do not make sense.</span></span>

<span data-ttu-id="a16a1-127">`unmanaged` 约束不由 CLR 强制执行，只由语言执行。</span><span class="sxs-lookup"><span data-stu-id="a16a1-127">The `unmanaged` constraint is not enforced by CLR, only by the language.</span></span> <span data-ttu-id="a16a1-128">若要防止其他语言出现错误，则具有此约束的方法将受 mod 要求保护。这会阻止其他语言使用非托管类型的类型参数。</span><span class="sxs-lookup"><span data-stu-id="a16a1-128">To prevent mis-use by other languages, methods which have this constraint will be protected by a mod-req. This will prevent other languages from using type arguments which are not unmanaged types.</span></span>

<span data-ttu-id="a16a1-129">约束中 `unmanaged` 的令牌不是关键字，也不是上下文关键字。</span><span class="sxs-lookup"><span data-stu-id="a16a1-129">The token `unmanaged` in the constraint is not a keyword, nor a contextual keyword.</span></span> <span data-ttu-id="a16a1-130">相反，它与 `var`，因为它是在该位置进行评估，并将：</span><span class="sxs-lookup"><span data-stu-id="a16a1-130">Instead it is like `var` in that it is evaluated at that location and will either:</span></span>

- <span data-ttu-id="a16a1-131">绑定到名为 `unmanaged`的用户定义的类型或引用类型：此操作将被视为处理任何其他命名类型约束。</span><span class="sxs-lookup"><span data-stu-id="a16a1-131">Bind to user defined or referenced type named `unmanaged`: This will be treated just as any other named type constraint is treated.</span></span> 
- <span data-ttu-id="a16a1-132">绑定到无类型：这将被解释为 `unmanaged` 约束。</span><span class="sxs-lookup"><span data-stu-id="a16a1-132">Bind to no type: This will be interpreted as the `unmanaged` constraint.</span></span>

<span data-ttu-id="a16a1-133">如果有一个名为 `unmanaged` 的类型，并且该类型在当前上下文中没有任何限制，则将无法使用 `unmanaged` 约束。</span><span class="sxs-lookup"><span data-stu-id="a16a1-133">In the case there is a type named `unmanaged` and it is available without qualification in the current context, then there will be no way to use the `unmanaged` constraint.</span></span> <span data-ttu-id="a16a1-134">这就是围绕功能 `var` 和用户定义类型的相同名称的规则。</span><span class="sxs-lookup"><span data-stu-id="a16a1-134">This parallels the rules surrounding the feature `var` and user defined types of the same name.</span></span> 

## <a name="drawbacks"></a><span data-ttu-id="a16a1-135">缺点</span><span class="sxs-lookup"><span data-stu-id="a16a1-135">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="a16a1-136">此功能的主要缺点是它提供少量的开发人员：通常是低级别库作者或框架。</span><span class="sxs-lookup"><span data-stu-id="a16a1-136">The primary drawback of this feature is that it serves a small number of developers: typically low level library authors or frameworks.</span></span>  <span data-ttu-id="a16a1-137">因此，对于少量开发人员而言，它花费了很多宝贵的语言时间。</span><span class="sxs-lookup"><span data-stu-id="a16a1-137">Hence it's spending precious language time for a small number of developers.</span></span> 

<span data-ttu-id="a16a1-138">但这些框架通常是大多数 .NET 应用程序的基础。</span><span class="sxs-lookup"><span data-stu-id="a16a1-138">Yet these frameworks are often the basis for the majority of .NET applications out there.</span></span>  <span data-ttu-id="a16a1-139">因此，在此级别上 wins 会对 .NET 生态系统产生波纹效果。</span><span class="sxs-lookup"><span data-stu-id="a16a1-139">Hence performance / correctness wins at this level can have a ripple effect on the .NET ecosystem.</span></span>  <span data-ttu-id="a16a1-140">这使得功能更值得考虑，即使是拥有有限的受众。</span><span class="sxs-lookup"><span data-stu-id="a16a1-140">This makes the feature worth considering even with the limited audience.</span></span>

## <a name="alternatives"></a><span data-ttu-id="a16a1-141">备选项</span><span class="sxs-lookup"><span data-stu-id="a16a1-141">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="a16a1-142">有几种方法可以考虑：</span><span class="sxs-lookup"><span data-stu-id="a16a1-142">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="a16a1-143">现状：此功能并不在其自身的优点上，开发人员继续使用隐式选择加入行为。</span><span class="sxs-lookup"><span data-stu-id="a16a1-143">The status quo:  The feature is not justified on its own merits and developers continue to use the implicit opt in behavior.</span></span>

## <a name="questions"></a><span data-ttu-id="a16a1-144">问题</span><span class="sxs-lookup"><span data-stu-id="a16a1-144">Questions</span></span>
[quesions]: #questions

### <a name="metadata-representation"></a><span data-ttu-id="a16a1-145">元数据表示形式</span><span class="sxs-lookup"><span data-stu-id="a16a1-145">Metadata Representation</span></span>

<span data-ttu-id="a16a1-146">F#语言对签名文件中的约束进行编码，这C#意味着不能重复使用其表示形式。</span><span class="sxs-lookup"><span data-stu-id="a16a1-146">The F# language encodes the constraint in the signature file which means C# cannot re-use their representation.</span></span> <span data-ttu-id="a16a1-147">需要为此约束选择新属性。</span><span class="sxs-lookup"><span data-stu-id="a16a1-147">A new attribute will need to be chosen for this constraint.</span></span> <span data-ttu-id="a16a1-148">此外，具有此约束的方法必须由 mod 请求保护。</span><span class="sxs-lookup"><span data-stu-id="a16a1-148">Additionally a method which has this constraint must be protected by a mod-req.</span></span>

### <a name="blittable-vs-unmanaged"></a><span data-ttu-id="a16a1-149">直接复制和非托管</span><span class="sxs-lookup"><span data-stu-id="a16a1-149">Blittable vs. Unmanaged</span></span>
<span data-ttu-id="a16a1-150">该F#语言具有一个非常[类似的功能](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints)，该功能使用关键字非托管。</span><span class="sxs-lookup"><span data-stu-id="a16a1-150">The F# language has a very [similar feature](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints) which uses the keyword unmanaged.</span></span> <span data-ttu-id="a16a1-151">可直接复制的名称来自 Midori 中的使用。</span><span class="sxs-lookup"><span data-stu-id="a16a1-151">The blittable name comes from the use in Midori.</span></span>  <span data-ttu-id="a16a1-152">可能想要在此处查看优先级，并改为使用非托管。</span><span class="sxs-lookup"><span data-stu-id="a16a1-152">May want to look to precedence here and use unmanaged instead.</span></span> 

<span data-ttu-id="a16a1-153">**解决方法**语言决定使用非托管</span><span class="sxs-lookup"><span data-stu-id="a16a1-153">**Resolution** The language decide to use unmanaged</span></span> 

### <a name="verifier"></a><span data-ttu-id="a16a1-154">符</span><span class="sxs-lookup"><span data-stu-id="a16a1-154">Verifier</span></span>

<span data-ttu-id="a16a1-155">验证程序/运行时是否需要更新以了解如何使用指向泛型类型参数的指针？</span><span class="sxs-lookup"><span data-stu-id="a16a1-155">Does the verifier / runtime need to be updated to understand the use of pointers to generic type parameters?</span></span>  <span data-ttu-id="a16a1-156">或者，它是否可以正常工作，而无需进行任何更改？</span><span class="sxs-lookup"><span data-stu-id="a16a1-156">Or can it simply work as is without changes?</span></span>

<span data-ttu-id="a16a1-157">**解决方法**无需更改。</span><span class="sxs-lookup"><span data-stu-id="a16a1-157">**Resolution** No changes needed.</span></span> <span data-ttu-id="a16a1-158">所有指针类型都是不可验证的。</span><span class="sxs-lookup"><span data-stu-id="a16a1-158">All pointer types are simply unverifiable.</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="a16a1-159">设计会议</span><span class="sxs-lookup"><span data-stu-id="a16a1-159">Design meetings</span></span>

<span data-ttu-id="a16a1-160">不适用</span><span class="sxs-lookup"><span data-stu-id="a16a1-160">n/a</span></span>
