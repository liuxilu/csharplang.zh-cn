---
ms.openlocfilehash: 1852356b830e29c3537a4de559fef32fd2c2f8b6
ms.sourcegitcommit: f7952cdddf85316a4beec493a0ecc14fcb3241c8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "79483912"
---
# <a name="target-typed-default-literal"></a><span data-ttu-id="98631-101">目标类型 "默认" 文本</span><span class="sxs-lookup"><span data-stu-id="98631-101">Target-typed "default" literal</span></span>

* <span data-ttu-id="98631-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="98631-102">[x] Proposed</span></span>
* <span data-ttu-id="98631-103">[x] 原型</span><span class="sxs-lookup"><span data-stu-id="98631-103">[x] Prototype</span></span>
* <span data-ttu-id="98631-104">[x] 实现</span><span class="sxs-lookup"><span data-stu-id="98631-104">[x] Implementation</span></span>
* <span data-ttu-id="98631-105">[] 规范</span><span class="sxs-lookup"><span data-stu-id="98631-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="98631-106">总结</span><span class="sxs-lookup"><span data-stu-id="98631-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="98631-107">目标类型化 `default` 功能是 `default(T)` 运算符的较短形式，这允许省略类型。</span><span class="sxs-lookup"><span data-stu-id="98631-107">The target-typed `default` feature is a shorter form variation of the `default(T)` operator, which allows the type to be omitted.</span></span> <span data-ttu-id="98631-108">改为通过目标类型推断其类型。</span><span class="sxs-lookup"><span data-stu-id="98631-108">Its type is inferred by target-typing instead.</span></span> <span data-ttu-id="98631-109">除此之外，它的行为类似于 `default(T)`。</span><span class="sxs-lookup"><span data-stu-id="98631-109">Aside from that, it behaves like `default(T)`.</span></span>

## <a name="motivation"></a><span data-ttu-id="98631-110">动机</span><span class="sxs-lookup"><span data-stu-id="98631-110">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="98631-111">主要动机是避免键入冗余信息。</span><span class="sxs-lookup"><span data-stu-id="98631-111">The main motivation is to avoid typing redundant information.</span></span>

<span data-ttu-id="98631-112">例如，在调用 `void Method(ImmutableArray<SomeType> array)`时，*默认*文本允许 `M(default)` 代替 `M(default(ImmutableArray<SomeType>))`。</span><span class="sxs-lookup"><span data-stu-id="98631-112">For instance, when invoking `void Method(ImmutableArray<SomeType> array)`, the *default* literal allows `M(default)` in place of `M(default(ImmutableArray<SomeType>))`.</span></span>

<span data-ttu-id="98631-113">这适用于许多方案，例如：</span><span class="sxs-lookup"><span data-stu-id="98631-113">This is applicable in a number of scenarios, such as:</span></span>

- <span data-ttu-id="98631-114">声明局部变量（`ImmutableArray<SomeType> x = default;`）</span><span class="sxs-lookup"><span data-stu-id="98631-114">declaring locals (`ImmutableArray<SomeType> x = default;`)</span></span>
- <span data-ttu-id="98631-115">三元运算（`var x = flag ? default : ImmutableArray<SomeType>.Empty;`）</span><span class="sxs-lookup"><span data-stu-id="98631-115">ternary operations (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)</span></span>
- <span data-ttu-id="98631-116">方法和 lambda 中的返回（`return default;`）</span><span class="sxs-lookup"><span data-stu-id="98631-116">returning in methods and lambdas (`return default;`)</span></span>
- <span data-ttu-id="98631-117">声明可选参数的默认值（`void Method(ImmutableArray<SomeType> arrayOpt = default)`）</span><span class="sxs-lookup"><span data-stu-id="98631-117">declaring default values for optional parameters (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)</span></span>
- <span data-ttu-id="98631-118">在数组创建表达式中包含默认值（`var x = new[] { default, ImmutableArray.Create(y) };`）</span><span class="sxs-lookup"><span data-stu-id="98631-118">including default values in array creation expressions (`var x = new[] { default, ImmutableArray.Create(y) };`)</span></span>


## <a name="detailed-design"></a><span data-ttu-id="98631-119">详细设计</span><span class="sxs-lookup"><span data-stu-id="98631-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="98631-120">引入了一个新的表达式，即*默认*文本。</span><span class="sxs-lookup"><span data-stu-id="98631-120">A new expression is introduced, the *default* literal.</span></span> <span data-ttu-id="98631-121">具有此分类的表达式可通过*默认文本转换*隐式转换为任何类型。</span><span class="sxs-lookup"><span data-stu-id="98631-121">An expression with this classification can be implicitly converted to any type, by a *default literal conversion*.</span></span> 

<span data-ttu-id="98631-122">*默认*文本的类型的推理与*null*文本的类型的推理相同，不同之处在于允许任何类型（而不仅仅是引用类型）。</span><span class="sxs-lookup"><span data-stu-id="98631-122">The inference of the type for the *default* literal works the same as that for the *null* literal, except that any type is allowed (not just reference types).</span></span>

<span data-ttu-id="98631-123">此转换将生成推断类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="98631-123">This conversion produces the default value of the inferred type.</span></span>

<span data-ttu-id="98631-124">*默认*文本可能具有常量值，具体取决于推断类型。</span><span class="sxs-lookup"><span data-stu-id="98631-124">The *default* literal may have a constant value, depending on the inferred type.</span></span> <span data-ttu-id="98631-125">`const int x = default;` 是合法的，但 `const int? y = default;` 不是这样。</span><span class="sxs-lookup"><span data-stu-id="98631-125">So `const int x = default;` is legal, but `const int? y = default;` is not.</span></span>

<span data-ttu-id="98631-126">只要另一个操作数具有类型，*默认*文本就可以是相等运算符的操作数。</span><span class="sxs-lookup"><span data-stu-id="98631-126">The *default* literal can be the operand of equality operators, as long as the other operand has a type.</span></span> <span data-ttu-id="98631-127">因此 `default == x` 和 `x == default` 是有效的表达式，但 `default == default` 是非法的。</span><span class="sxs-lookup"><span data-stu-id="98631-127">So `default == x` and `x == default` are valid expressions, but `default == default` is illegal.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="98631-128">缺点</span><span class="sxs-lookup"><span data-stu-id="98631-128">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="98631-129">一个次要缺点是，*默认*文本可用于在大多数上下文中替代*null*文本。</span><span class="sxs-lookup"><span data-stu-id="98631-129">A minor drawback is that *default* literal can be used in place of *null* literal in most contexts.</span></span> <span data-ttu-id="98631-130">两个异常 `throw null;` 和 `null == null`，这两个异常允许用于*null*文本，但不是*默认*文本。</span><span class="sxs-lookup"><span data-stu-id="98631-130">Two of the exceptions are `throw null;` and `null == null`, which are allowed for the *null* literal, but not the *default* literal.</span></span>

## <a name="alternatives"></a><span data-ttu-id="98631-131">备选项</span><span class="sxs-lookup"><span data-stu-id="98631-131">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="98631-132">有几种方法可以考虑：</span><span class="sxs-lookup"><span data-stu-id="98631-132">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="98631-133">现状：此功能并不在其自身的优点上进行调整，开发人员继续使用具有显式类型的默认运算符。</span><span class="sxs-lookup"><span data-stu-id="98631-133">The status quo:  The feature is not justified on its own merits and developers continue to use the default operator with an explicit type.</span></span>
- <span data-ttu-id="98631-134">扩展 null 文本：这是 `Nothing`的 VB 方法。</span><span class="sxs-lookup"><span data-stu-id="98631-134">Extending the null literal: This is the VB approach with `Nothing`.</span></span> <span data-ttu-id="98631-135">我们可能允许 `int x = null;`。</span><span class="sxs-lookup"><span data-stu-id="98631-135">We could allow `int x = null;`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="98631-136">未解决的问题</span><span class="sxs-lookup"><span data-stu-id="98631-136">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="98631-137">[x] 应*默认*允许*作为 or 运算符的操作数*？ *as*</span><span class="sxs-lookup"><span data-stu-id="98631-137">[x] Should *default* be allowed as the operand of the *is* or *as* operators?</span></span> <span data-ttu-id="98631-138">答：不允许 `default is T`、允许 `x is default`、允许 `default as RefType` （始终为空的警告）</span><span class="sxs-lookup"><span data-stu-id="98631-138">Answer:  disallow `default is T`, allow `x is default`, allow `default as RefType` (with always-null warning)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="98631-139">设计会议</span><span class="sxs-lookup"><span data-stu-id="98631-139">Design meetings</span></span>

- [<span data-ttu-id="98631-140">LDM 3/7/2017</span><span class="sxs-lookup"><span data-stu-id="98631-140">LDM 3/7/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-07.md)
- [<span data-ttu-id="98631-141">LDM 3/28/2017</span><span class="sxs-lookup"><span data-stu-id="98631-141">LDM 3/28/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-28.md)
- [<span data-ttu-id="98631-142">LDM 5/31/2017</span><span class="sxs-lookup"><span data-stu-id="98631-142">LDM 5/31/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md#default-in-operators)
