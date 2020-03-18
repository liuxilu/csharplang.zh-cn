---
ms.openlocfilehash: ac2b233eb703b5eea3bd2dfdbeeadd7494b0c695
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483642"
---
# <a name="non-trailing-named-arguments"></a><span data-ttu-id="b33c8-101">非尾随命名参数</span><span class="sxs-lookup"><span data-stu-id="b33c8-101">Non-trailing named arguments</span></span>

## <a name="summary"></a><span data-ttu-id="b33c8-102">总结</span><span class="sxs-lookup"><span data-stu-id="b33c8-102">Summary</span></span>
[summary]: #summary
<span data-ttu-id="b33c8-103">如果命名参数用于其正确位置，则允许在非尾随位置使用该参数。</span><span class="sxs-lookup"><span data-stu-id="b33c8-103">Allow named arguments to be used in non-trailing position, as long as they are used in their correct position.</span></span> <span data-ttu-id="b33c8-104">例如：`DoSomething(isEmployed:true, name, age);`。</span><span class="sxs-lookup"><span data-stu-id="b33c8-104">For example: `DoSomething(isEmployed:true, name, age);`.</span></span>

## <a name="motivation"></a><span data-ttu-id="b33c8-105">动机</span><span class="sxs-lookup"><span data-stu-id="b33c8-105">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="b33c8-106">主要动机是避免键入冗余信息。</span><span class="sxs-lookup"><span data-stu-id="b33c8-106">The main motivation is to avoid typing redundant information.</span></span> <span data-ttu-id="b33c8-107">为了阐明代码而不是按顺序传递参数，通常可以将作为文本的参数（例如 `null`、`true`）命名为。</span><span class="sxs-lookup"><span data-stu-id="b33c8-107">It is common to name an argument that is a literal (such as `null`, `true`) for the purpose of clarifying the code, rather than of passing arguments out-of-order.</span></span>
<span data-ttu-id="b33c8-108">当前不允许（`CS1738`），除非还将所有以下参数命名为。</span><span class="sxs-lookup"><span data-stu-id="b33c8-108">That is currently disallowed (`CS1738`) unless all the following arguments are also named.</span></span>

```csharp
DoSomething(isEmployed:true, name, age); // currently disallowed, even though all arguments are in position
// CS1738 "Named argument specifications must appear after all fixed arguments have been specified"
```

<span data-ttu-id="b33c8-109">一些其他示例：</span><span class="sxs-lookup"><span data-stu-id="b33c8-109">Some additional examples:</span></span>
```csharp
public void DoSomething(bool isEmployed, string personName, int personAge) { ... }

DoSomething(isEmployed:true, name, age); // currently CS1738, but would become legal
DoSomething(true, personName:name, age); // currently CS1738, but would become legal
DoSomething(name, isEmployed:true, age); // remains illegal
DoSomething(name, age, isEmployed:true); // remains illegal
DoSomething(true, personAge:age, personName:name); // already legal
```

<span data-ttu-id="b33c8-110">这也适用于 params：</span><span class="sxs-lookup"><span data-stu-id="b33c8-110">This would also work with params:</span></span>
```csharp
public class Task
{
    public static Task When(TaskStatus all, TaskStatus any, params Task[] tasks);
}
Task.When(all: TaskStatus.RanToCompletion, any: TaskStatus.Faulted, task1, task2)
```

## <a name="detailed-design"></a><span data-ttu-id="b33c8-111">详细设计</span><span class="sxs-lookup"><span data-stu-id="b33c8-111">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="b33c8-112">在§7.5.1 （自变量列表）中，规范当前指出：</span><span class="sxs-lookup"><span data-stu-id="b33c8-112">In §7.5.1 (Argument lists), the spec currently says:</span></span>
> <span data-ttu-id="b33c8-113">带有*参数名*的*参数*称为__命名参数__，而没有*参数名* *的参数是*__位置参数__。</span><span class="sxs-lookup"><span data-stu-id="b33c8-113">An *argument* with an *argument-name* is referred to as a __named argument__, whereas an *argument* without an *argument-name* is a __positional argument__.</span></span> <span data-ttu-id="b33c8-114">位置参数出现在*参数列表*中的命名参数之后的错误。</span><span class="sxs-lookup"><span data-stu-id="b33c8-114">It is an error for a positional argument to appear after a named argument in an *argument-list*.</span></span>

<span data-ttu-id="b33c8-115">建议删除此错误并更新用于查找自变量的相应参数的规则（第7.5.1.1）：</span><span class="sxs-lookup"><span data-stu-id="b33c8-115">The proposal is to remove this error and update the rules for finding the corresponding parameter for an argument (§7.5.1.1):</span></span>

<span data-ttu-id="b33c8-116">参数中的参数-实例构造函数、方法、索引器和委托的列表：</span><span class="sxs-lookup"><span data-stu-id="b33c8-116">Arguments in the argument-list of instance constructors, methods, indexers and delegates:</span></span>
- <span data-ttu-id="b33c8-117">[现有规则]</span><span class="sxs-lookup"><span data-stu-id="b33c8-117">[existing rules]</span></span>
- <span data-ttu-id="b33c8-118">未命名的参数在不在指定位置的命名参数或命名的 params 参数之后，不对应于参数。</span><span class="sxs-lookup"><span data-stu-id="b33c8-118">An unnamed argument corresponds to no parameter when it is after an out-of-position named argument or a named params argument.</span></span>

<span data-ttu-id="b33c8-119">具体而言，这会阻止调用 `M(c: false, valueB);``void M(bool a = true, bool b = true, bool c = true, );`。</span><span class="sxs-lookup"><span data-stu-id="b33c8-119">In particular, this prevents invoking `void M(bool a = true, bool b = true, bool c = true, );` with `M(c: false, valueB);`.</span></span> <span data-ttu-id="b33c8-120">第一个参数在位置上使用（参数在第一个位置使用，但名为 "c" 的参数在第三个位置），因此应将以下参数命名为。</span><span class="sxs-lookup"><span data-stu-id="b33c8-120">The first argument is used out-of-position (the argument is used in first position, but the parameter named "c" is in third position), so the following arguments should be named.</span></span>

<span data-ttu-id="b33c8-121">换言之，仅当名称和位置导致查找相同的相应参数时，才允许使用非尾随命名参数。</span><span class="sxs-lookup"><span data-stu-id="b33c8-121">In other words, non-trailing named arguments are only allowed when the name and the position result in finding the same corresponding parameter.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="b33c8-122">缺点</span><span class="sxs-lookup"><span data-stu-id="b33c8-122">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="b33c8-123">此建议在重载决策中 exacerbates 现有微妙之处和命名参数。</span><span class="sxs-lookup"><span data-stu-id="b33c8-123">This proposal exacerbates existing subtleties with named arguments in overload resolution.</span></span> <span data-ttu-id="b33c8-124">例如：</span><span class="sxs-lookup"><span data-stu-id="b33c8-124">For instance:</span></span>

```csharp
void M(int x, int y) { }
void M<T>(T y, int x) { }

void M2()
{
    M(3, 4);
    M(y: 3, x: 4); // Invokes M(int, int)
    M(y: 3, 4); // Invokes M<T>(T, int)
}
```

<span data-ttu-id="b33c8-125">你现在可以通过交换参数来实现这种情况：</span><span class="sxs-lookup"><span data-stu-id="b33c8-125">You could get this situation today by swapping the parameters:</span></span>

```csharp
void M(int y, int x) { }
void M<T>(int x, T y) { }

void M2()
{
    M(3, 4);
    M(x: 3, y: 4); // Invokes M(int, int)
    M(3, y: 4); // Invokes M<T>(int, T)
}
```

<span data-ttu-id="b33c8-126">同样，如果你有两种方法 `void M(int a, int b)` 和 `void M(int x, string y)`，则被误认为调用 `M(x: 1, 2)` 将基于第二个重载生成诊断（"无法从 ' int ' 转换为 ' string '"）。</span><span class="sxs-lookup"><span data-stu-id="b33c8-126">Similarly, if you have two methods `void M(int a, int b)` and `void M(int x, string y)`, the mistaken invocation `M(x: 1, 2)` will produce a diagnostic based on the second overload ("cannot convert from 'int' to 'string'").</span></span> <span data-ttu-id="b33c8-127">如果在尾随位置使用命名参数，则已存在此问题。</span><span class="sxs-lookup"><span data-stu-id="b33c8-127">This problem already exists when the named argument is used in a trailing position.</span></span>

## <a name="alternatives"></a><span data-ttu-id="b33c8-128">备选项</span><span class="sxs-lookup"><span data-stu-id="b33c8-128">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="b33c8-129">有几种方法可以考虑：</span><span class="sxs-lookup"><span data-stu-id="b33c8-129">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="b33c8-130">现状</span><span class="sxs-lookup"><span data-stu-id="b33c8-130">The status quo</span></span>
- <span data-ttu-id="b33c8-131">当你在中间键入特定名称时，提供 IDE 协助来填充尾随参数的所有名称。</span><span class="sxs-lookup"><span data-stu-id="b33c8-131">Providing IDE assistance to fill-in all the names of trailing arguments when you type specific a name in the middle.</span></span>

<span data-ttu-id="b33c8-132">这两种方法都受到更详细程度的影响，因为它们会引入多个命名参数，即使只需要参数列表开头有一个文本名称，也是如此。</span><span class="sxs-lookup"><span data-stu-id="b33c8-132">Both of those suffer from more verbosity, as they introduce multiple named arguments even if you just need one name of a literal at the beginning of the argument list.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="b33c8-133">未解决的问题</span><span class="sxs-lookup"><span data-stu-id="b33c8-133">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="b33c8-134">设计会议</span><span class="sxs-lookup"><span data-stu-id="b33c8-134">Design meetings</span></span>
[ldm]: #ldm
<span data-ttu-id="b33c8-135">此功能在5月 2017 16 日的 LDM 中进行了简单的介绍，并按原则进行审批（可转到提议/原型）。</span><span class="sxs-lookup"><span data-stu-id="b33c8-135">The feature was briefly discussed in LDM on May 16th 2017, with approval in principle (ok to move to proposal/prototype).</span></span> <span data-ttu-id="b33c8-136">它还在2017年6月28日讨论。</span><span class="sxs-lookup"><span data-stu-id="b33c8-136">It was also briefly discussed on June 28th 2017.</span></span>

<span data-ttu-id="b33c8-137">与倡导问题相关的初始讨论 https://github.com/dotnet/csharplang/issues/518 https://github.com/dotnet/csharplang/issues/570</span><span class="sxs-lookup"><span data-stu-id="b33c8-137">Relates to initial discussion https://github.com/dotnet/csharplang/issues/518 Relates to championed issue https://github.com/dotnet/csharplang/issues/570</span></span>
