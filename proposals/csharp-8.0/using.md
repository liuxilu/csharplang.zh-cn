---
ms.openlocfilehash: 91afbc3e3412049cd183c36c8035f1862c520413
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/05/2019
ms.locfileid: "79483972"
---
# <a name="pattern-based-using-and-using-declarations"></a><span data-ttu-id="7c66a-101">"基于模式使用" 和 "using 声明"</span><span class="sxs-lookup"><span data-stu-id="7c66a-101">"pattern-based using" and "using declarations"</span></span>

## <a name="summary"></a><span data-ttu-id="7c66a-102">总结</span><span class="sxs-lookup"><span data-stu-id="7c66a-102">Summary</span></span>

<span data-ttu-id="7c66a-103">语言将添加两个新功能来围绕 `using` 语句，以便简化资源管理： `using` 应识别 `IDisposable` 的可释放模式，并向语言添加 `using` 声明。</span><span class="sxs-lookup"><span data-stu-id="7c66a-103">The language will add two new capabilities around the `using` statement in order to make resource management simpler: `using` should recognize a disposable pattern in addition to `IDisposable` and add a `using` declaration to the language.</span></span>

## <a name="motivation"></a><span data-ttu-id="7c66a-104">动机</span><span class="sxs-lookup"><span data-stu-id="7c66a-104">Motivation</span></span>

<span data-ttu-id="7c66a-105">`using` 语句是当今资源管理的有效工具，但它需要很多工作。</span><span class="sxs-lookup"><span data-stu-id="7c66a-105">The `using` statement is an effective tool for resource management today but it requires quite a bit of ceremony.</span></span> <span data-ttu-id="7c66a-106">具有多个要管理的资源的方法可以在语法上陷入一系列的 `using` 语句。</span><span class="sxs-lookup"><span data-stu-id="7c66a-106">Methods that have a number of resources to manage can get syntactically bogged down with a series of `using` statements.</span></span> <span data-ttu-id="7c66a-107">这种语法负担足以表明，大多数编码样式准则对此方案在大括号附近有一个例外。</span><span class="sxs-lookup"><span data-stu-id="7c66a-107">This syntax burden is enough that most coding style guidelines explicitly have an exception around braces for this scenario.</span></span> 

<span data-ttu-id="7c66a-108">`using` 声明消除了这里的许多仪式，并C#与包含资源管理块的其他语言相同。</span><span class="sxs-lookup"><span data-stu-id="7c66a-108">The `using` declaration removes much of the ceremony here and gets C# on par with other languages that include resource management blocks.</span></span> <span data-ttu-id="7c66a-109">此外，基于模式的 `using` 允许开发人员展开可参与的类型集。</span><span class="sxs-lookup"><span data-stu-id="7c66a-109">Additionally the pattern-based `using` lets developers expand the set of types that can participate here.</span></span> <span data-ttu-id="7c66a-110">在许多情况下，无需创建只存在以允许值在 `using` 语句中使用的包装类型。</span><span class="sxs-lookup"><span data-stu-id="7c66a-110">In many cases removing the need to create wrapper types that only exist to allow for a values use in a `using` statement.</span></span> 

<span data-ttu-id="7c66a-111">借助这些功能，开发人员可简化和扩展可应用 `using` 的方案。</span><span class="sxs-lookup"><span data-stu-id="7c66a-111">Together these features allow developers to simplify and expand the scenarios where `using` can be applied.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="7c66a-112">详细设计</span><span class="sxs-lookup"><span data-stu-id="7c66a-112">Detailed Design</span></span> 

### <a name="using-declaration"></a><span data-ttu-id="7c66a-113">using 声明</span><span class="sxs-lookup"><span data-stu-id="7c66a-113">using declaration</span></span>

<span data-ttu-id="7c66a-114">语言将允许 `using` 添加到局部变量声明。</span><span class="sxs-lookup"><span data-stu-id="7c66a-114">The language will allow for `using` to be added to a local variable declaration.</span></span> <span data-ttu-id="7c66a-115">此类声明与在同一位置的 `using` 语句中声明变量的效果相同。</span><span class="sxs-lookup"><span data-stu-id="7c66a-115">Such a declaration will have the same effect as declaring the variable in a `using` statement at the same location.</span></span>

```csharp
if (...) 
{ 
   using FileStream f = new FileStream(@"C:\users\jaredpar\using.md");
   // statements
}

// Equivalent to 
if (...) 
{ 
   using (FileStream f = new FileStream(@"C:\users\jaredpar\using.md")) 
   {
    // statements
   }
}
```

<span data-ttu-id="7c66a-116">`using` local 的生存期将扩展到声明它的范围的末尾。</span><span class="sxs-lookup"><span data-stu-id="7c66a-116">The lifetime of a `using` local will extend to the end of the scope in which it is declared.</span></span> <span data-ttu-id="7c66a-117">然后，将按声明的反向顺序处理 `using` 局部变量。</span><span class="sxs-lookup"><span data-stu-id="7c66a-117">The `using` locals will then be disposed in the reverse order in which they are declared.</span></span> 

```csharp
{ 
    using var f1 = new FileStream("...");
    using var f2 = new FileStream("..."), f3 = new FileStream("...");
    ...
    // Dispose f3
    // Dispose f2 
    // Dispose f1
}
```

<span data-ttu-id="7c66a-118">`using` 声明的人脸 `goto`或任何其他控制流构造没有任何限制。</span><span class="sxs-lookup"><span data-stu-id="7c66a-118">There are no restrictions around `goto`, or any other control flow construct in the face of a `using` declaration.</span></span> <span data-ttu-id="7c66a-119">相反，代码的行为与等效的 `using` 语句一样：</span><span class="sxs-lookup"><span data-stu-id="7c66a-119">Instead the code acts just as it would for the equivalent `using` statement:</span></span>

```csharp
{
    using var f1 = new FileStream("...");
  target:
    using var f2 = new FileStream("...");
    if (someCondition) 
    {
        // Causes f2 to be disposed but has no effect on f1
        goto target;
    }
}
```

<span data-ttu-id="7c66a-120">在 `using` 本地声明中声明的局部变量将隐式为只读。</span><span class="sxs-lookup"><span data-stu-id="7c66a-120">A local declared in a `using` local declaration will be implicitly read-only.</span></span> <span data-ttu-id="7c66a-121">这与 `using` 语句中声明的局部变量的行为匹配。</span><span class="sxs-lookup"><span data-stu-id="7c66a-121">This matches the behavior of locals declared in a `using` statement.</span></span> 

<span data-ttu-id="7c66a-122">`using` 声明的语言语法如下：</span><span class="sxs-lookup"><span data-stu-id="7c66a-122">The language grammar for `using` declarations will be the following:</span></span>

```antlr
local-using-declaration:
  using type using-declarators

using-declarators:
  using-declarator
  using-declarators , using-declarator
  
using-declarator:
  identifier = expression
```

<span data-ttu-id="7c66a-123">围绕 `using` 声明的限制：</span><span class="sxs-lookup"><span data-stu-id="7c66a-123">Restrictions around `using` declaration:</span></span>

- <span data-ttu-id="7c66a-124">不能直接显示在 `case` 标签内，而必须位于 `case` 标签内的块内。</span><span class="sxs-lookup"><span data-stu-id="7c66a-124">May not appear directly inside a `case` label but instead must be within a block inside the `case` label.</span></span>
- <span data-ttu-id="7c66a-125">不能显示为 `out` 变量声明的一部分。</span><span class="sxs-lookup"><span data-stu-id="7c66a-125">May not appear as part of an `out` variable declaration.</span></span> 
- <span data-ttu-id="7c66a-126">必须具有每个声明符的初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="7c66a-126">Must have an initializer for each declarator.</span></span>
- <span data-ttu-id="7c66a-127">局部类型必须可隐式转换为 `IDisposable` 或满足 `using` 模式。</span><span class="sxs-lookup"><span data-stu-id="7c66a-127">The local type must be implicitly convertible to `IDisposable` or fulfill the `using` pattern.</span></span>

### <a name="pattern-based-using"></a><span data-ttu-id="7c66a-128">基于模式的使用</span><span class="sxs-lookup"><span data-stu-id="7c66a-128">pattern-based using</span></span>

<span data-ttu-id="7c66a-129">语言将添加可释放模式的概念：这是具有可访问的 `Dispose` 实例方法的类型。</span><span class="sxs-lookup"><span data-stu-id="7c66a-129">The language will add the notion of a disposable pattern: that is a type which has an accessible `Dispose` instance method.</span></span> <span data-ttu-id="7c66a-130">适合可释放模式的类型可以参与 `using` 语句或声明，而无需实现 `IDisposable`。</span><span class="sxs-lookup"><span data-stu-id="7c66a-130">Types which fit the disposable pattern can participate in a `using` statement or declaration without being required to implement `IDisposable`.</span></span> 

```csharp
class Resource
{ 
    public void Dispose() { ... }
}

using (var r = new Resource())
{
    // statements
}
```

<span data-ttu-id="7c66a-131">这将允许开发人员在许多新方案中利用 `using`：</span><span class="sxs-lookup"><span data-stu-id="7c66a-131">This will allow developers to leverage `using` in a number of new scenarios:</span></span>

- <span data-ttu-id="7c66a-132">`ref struct`：这些类型现在无法实现接口，因此无法参与 `using` 语句。</span><span class="sxs-lookup"><span data-stu-id="7c66a-132">`ref struct`: These types can't implement interfaces today and hence can't participate in `using` statements.</span></span>
- <span data-ttu-id="7c66a-133">扩展方法将允许开发人员增加其他程序集中的类型以参与 `using` 语句。</span><span class="sxs-lookup"><span data-stu-id="7c66a-133">Extension methods will allow developers to augment types in other assemblies to participate in `using` statements.</span></span>

<span data-ttu-id="7c66a-134">如果某个类型可以隐式转换为 `IDisposable` 并且还适合于可释放模式，则 `IDisposable` 将是首选的。</span><span class="sxs-lookup"><span data-stu-id="7c66a-134">In the situation where a type can be implicitly converted to `IDisposable` and also fits the disposable pattern, then `IDisposable` will be preferred.</span></span> <span data-ttu-id="7c66a-135">尽管这采用相反的方法 `foreach` （模式优先于接口），但这是实现向后兼容性的必要条件。</span><span class="sxs-lookup"><span data-stu-id="7c66a-135">While this takes the opposite approach of `foreach` (pattern preferred over interface) it is necessary for backwards compatibility.</span></span>

<span data-ttu-id="7c66a-136">与传统 `using` 语句相同的限制同样适用： `using` 中声明的局部变量是只读的，`null` 值不会导致引发异常，等等 .。。仅在调用 Dispose 之前，不会有强制转换到 `IDisposable` 这一生成代码：</span><span class="sxs-lookup"><span data-stu-id="7c66a-136">The same restrictions from a traditional `using` statement apply here as well: local variables declared in the `using` are read-only, a `null` value will not cause an exception to be thrown, etc ... The code generation will be different only in that there will not be a cast to `IDisposable` before calling Dispose:</span></span>

```csharp
{
      Resource r = new Resource();
      try {
            // statements
      }
      finally {
            if (resource != null) resource.Dispose();
      }
}
```

<span data-ttu-id="7c66a-137">为了适应可释放模式，`Dispose` 方法必须可访问、无参数且具有 `void` 返回类型。</span><span class="sxs-lookup"><span data-stu-id="7c66a-137">In order to fit the disposable pattern the `Dispose` method must be accessible, parameterless and have a `void` return type.</span></span> <span data-ttu-id="7c66a-138">没有其他限制。</span><span class="sxs-lookup"><span data-stu-id="7c66a-138">There are no other restrictions.</span></span> <span data-ttu-id="7c66a-139">这显式表示可在此处使用扩展方法。</span><span class="sxs-lookup"><span data-stu-id="7c66a-139">This explicitly means that extension methods can be used here.</span></span>

## <a name="considerations"></a><span data-ttu-id="7c66a-140">注意事项</span><span class="sxs-lookup"><span data-stu-id="7c66a-140">Considerations</span></span>

### <a name="case-labels-without-blocks"></a><span data-ttu-id="7c66a-141">不带块的 case 标签</span><span class="sxs-lookup"><span data-stu-id="7c66a-141">case labels without blocks</span></span>

<span data-ttu-id="7c66a-142">`using declaration` 在 `case` 标签内直接是非法的，因为它的实际生存期很复杂。</span><span class="sxs-lookup"><span data-stu-id="7c66a-142">A `using declaration` is illegal directly inside a `case` label due to complications around its actual lifetime.</span></span> <span data-ttu-id="7c66a-143">一种可能的解决方案是在同一位置为其指定与 `out var` 相同的生存期。</span><span class="sxs-lookup"><span data-stu-id="7c66a-143">One potential solution is to simply give it the same lifetime as an `out var` in the same location.</span></span> <span data-ttu-id="7c66a-144">这会被视为功能实现的额外复杂性，并使工作变得轻松（只需将块添加到 `case` 标签），并不是采用此路线的理由。</span><span class="sxs-lookup"><span data-stu-id="7c66a-144">It was deemed the extra complexity to the feature implementation and the ease of the work around (just add a block to the `case` label) didn't justify taking this route.</span></span>

## <a name="future-expansions"></a><span data-ttu-id="7c66a-145">未来扩展</span><span class="sxs-lookup"><span data-stu-id="7c66a-145">Future Expansions</span></span>

### <a name="fixed-locals"></a><span data-ttu-id="7c66a-146">固定局部变量</span><span class="sxs-lookup"><span data-stu-id="7c66a-146">fixed locals</span></span>

<span data-ttu-id="7c66a-147">`fixed` 语句具有 `using` 语句的所有属性，这些属性可使 `using` 局部变量。</span><span class="sxs-lookup"><span data-stu-id="7c66a-147">A `fixed` statement has all of the properties of `using` statements that motivated the ability to have `using` locals.</span></span> <span data-ttu-id="7c66a-148">应考虑将此功能扩展到 `fixed` 局部变量。</span><span class="sxs-lookup"><span data-stu-id="7c66a-148">Consideration should be given to extending this feature to `fixed` locals as well.</span></span> <span data-ttu-id="7c66a-149">生存期和顺序规则应该同样适用于 `using` 和 `fixed`。</span><span class="sxs-lookup"><span data-stu-id="7c66a-149">The lifetime and ordering rules should apply equally well for `using` and `fixed` here.</span></span>
