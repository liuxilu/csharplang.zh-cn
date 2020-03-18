---
ms.openlocfilehash: 76065293f652979ab395e131d657e44899c5a65b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483540"
---
# <a name="simplified-null-argument-checking"></a><span data-ttu-id="0739f-101">简化的 Null 参数检查</span><span class="sxs-lookup"><span data-stu-id="0739f-101">Simplified Null Argument Checking</span></span>

## <a name="summary"></a><span data-ttu-id="0739f-102">总结</span><span class="sxs-lookup"><span data-stu-id="0739f-102">Summary</span></span>
<span data-ttu-id="0739f-103">此提议提供简化的语法，用于验证方法自变量不会 `null` 并相应地引发 `ArgumentNullException`。</span><span class="sxs-lookup"><span data-stu-id="0739f-103">This proposal provides a simplified syntax for validating method arguments are not `null` and throwing `ArgumentNullException` appropriately.</span></span>

## <a name="motivation"></a><span data-ttu-id="0739f-104">动机</span><span class="sxs-lookup"><span data-stu-id="0739f-104">Motivation</span></span>
<span data-ttu-id="0739f-105">设计可以为 null 的引用类型的工作导致我们检查 `null` 参数验证所需的代码。</span><span class="sxs-lookup"><span data-stu-id="0739f-105">The work on designing nullable reference types has caused us to examine the code necessary for `null` argument validation.</span></span> <span data-ttu-id="0739f-106">假设 NRT 不会影响代码执行开发人员仍必须在完全 `null` 清理的项目中添加 `if (arg is null) throw` 样板板代码。</span><span class="sxs-lookup"><span data-stu-id="0739f-106">Given that NRT doesn't affect code execution developers still must add `if (arg is null) throw` boiler plate code even in projects which are fully `null` clean.</span></span> <span data-ttu-id="0739f-107">这样，我们就可以使用语言中的参数 `null` 验证的最小语法来探究。</span><span class="sxs-lookup"><span data-stu-id="0739f-107">This gave us the desire to explore a minimal syntax for argument `null` validation in the language.</span></span> 

<span data-ttu-id="0739f-108">尽管这 `null` 参数验证语法应经常与 NRT 配对，但建议完全独立于该建议。</span><span class="sxs-lookup"><span data-stu-id="0739f-108">While this `null` parameter validation syntax is expected to pair frequently with NRT the proposal is fully independent of it.</span></span> <span data-ttu-id="0739f-109">语法可独立于 `#nullable` 指令使用。</span><span class="sxs-lookup"><span data-stu-id="0739f-109">The syntax can be used independent of `#nullable` directives.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="0739f-110">详细设计</span><span class="sxs-lookup"><span data-stu-id="0739f-110">Detailed Design</span></span> 

### <a name="null-validation-parameter-syntax"></a><span data-ttu-id="0739f-111">空验证参数语法</span><span class="sxs-lookup"><span data-stu-id="0739f-111">Null validation parameter syntax</span></span>
<span data-ttu-id="0739f-112">感叹号运算符 `!`可以放置在参数列表中的参数名之后，这将导致C#编译器发出此参数的标准 `null` 检查代码。</span><span class="sxs-lookup"><span data-stu-id="0739f-112">The bang operator, `!`, can be positioned after a parameter name in a parameter list and this will cause the C# compiler to emit standard `null` checking code for that parameter.</span></span> <span data-ttu-id="0739f-113">这称为 `null` 验证参数语法。</span><span class="sxs-lookup"><span data-stu-id="0739f-113">This is referred to as `null` validation parameter syntax.</span></span> <span data-ttu-id="0739f-114">例如：</span><span class="sxs-lookup"><span data-stu-id="0739f-114">For example:</span></span>

``` csharp
void M(string name!) {
    ...
}
```

<span data-ttu-id="0739f-115">将转换为：</span><span class="sxs-lookup"><span data-stu-id="0739f-115">Will be translated into:</span></span>

``` csharp
void M(string name) {
    if (name is null) {
        throw new ArgumentNullException(nameof(name));
    }
    ...
}
```

<span data-ttu-id="0739f-116">生成的 `null` 检查将在任何开发人员在方法中编写代码之前发生。</span><span class="sxs-lookup"><span data-stu-id="0739f-116">The generated `null` check will occur before any developer authored code in the method.</span></span> <span data-ttu-id="0739f-117">当多个参数包含 `!` 运算符时，将按照声明参数的顺序执行检查。</span><span class="sxs-lookup"><span data-stu-id="0739f-117">When multiple parameters contain the `!` operator then the checks will occur in the same order as the parameters are declared.</span></span>

``` csharp
void M(string p1, string p2) {
    if (p1 is null) {
        throw new ArgumentNullException(nameof(p1));
    }
    if (p2 is null) {
        throw new ArgumentNullException(nameof(p2));
    }
    ...
}
```

<span data-ttu-id="0739f-118">检查将专门用于引用相等性 `null`，而不会调用 `==` 或任何用户定义的运算符。</span><span class="sxs-lookup"><span data-stu-id="0739f-118">The check will be specifically for reference equality to `null`, it does not invoke `==` or any user defined operators.</span></span> <span data-ttu-id="0739f-119">这也意味着，只能将 `!` 运算符添加到可以根据 `null`测试其类型是否相等的参数。</span><span class="sxs-lookup"><span data-stu-id="0739f-119">This also means the `!` operator can only be added to parameters whose type can be tested for equality against `null`.</span></span> <span data-ttu-id="0739f-120">这意味着它不能用于其类型已知为值类型的参数。</span><span class="sxs-lookup"><span data-stu-id="0739f-120">This means it can't be used on a parameter whose type is known to be a value type.</span></span>

``` csharp
// Error: Cannot use ! on parameters who types derive from System.ValueType
void G<T>(T arg!) where T : struct {

}
```

<span data-ttu-id="0739f-121">对于构造函数，将在构造函数中的任何其他代码之前发生 `null` 验证。</span><span class="sxs-lookup"><span data-stu-id="0739f-121">In the case of a constructor the `null` validation will occur before any other code in the constructor.</span></span> <span data-ttu-id="0739f-122">其中包括：</span><span class="sxs-lookup"><span data-stu-id="0739f-122">That includes:</span></span> 

- <span data-ttu-id="0739f-123">链接到具有 `this` 或 `base` 的其他构造函数</span><span class="sxs-lookup"><span data-stu-id="0739f-123">Chaining to other constructors with `this` or `base`</span></span> 
- <span data-ttu-id="0739f-124">隐式出现在构造函数中的字段初始值设定项</span><span class="sxs-lookup"><span data-stu-id="0739f-124">Field initializers which implicitly occur in the constructor</span></span>

<span data-ttu-id="0739f-125">例如：</span><span class="sxs-lookup"><span data-stu-id="0739f-125">For example:</span></span>

``` csharp
class C {
    string field = GetString();
    C(string name!): this(name) {
        ...
    }
}
```

<span data-ttu-id="0739f-126">将大致转换为以下内容：</span><span class="sxs-lookup"><span data-stu-id="0739f-126">Will be roughly translated into the following:</span></span>

``` csharp
class C {
    C(string name)
        if (name is null) {
            throw new ArgumentNullException(nameof(name));
        }
        field = GetString();
        :this(name);
        ...
}
```

<span data-ttu-id="0739f-127">注意：这不是合法C#代码，而只是实现实现的近似值。</span><span class="sxs-lookup"><span data-stu-id="0739f-127">Note: this is not legal C# code but instead just an approximation of what the implementation does.</span></span> 

<span data-ttu-id="0739f-128">`null` 验证参数语法在 lambda 参数列表上也是有效的。</span><span class="sxs-lookup"><span data-stu-id="0739f-128">The `null` validation parameter syntax will also be valid on lambda parameter lists.</span></span> <span data-ttu-id="0739f-129">即使在缺少括号的单个参数语法中，这也是有效的。</span><span class="sxs-lookup"><span data-stu-id="0739f-129">This is valid even in the single parameter syntax that lacks parens.</span></span>

``` csharp
void G() {
    // An identity lambda which throws on a null input
    Func<string, string> s = x! => x;
}
```

<span data-ttu-id="0739f-130">语法在迭代器方法的参数上也是有效的。</span><span class="sxs-lookup"><span data-stu-id="0739f-130">The syntax is also valid on parameters to iterator methods.</span></span> <span data-ttu-id="0739f-131">与迭代器中的其他代码不同，`null` 验证会在调用迭代器方法时，而不是在遍历基础枚举器时进行。</span><span class="sxs-lookup"><span data-stu-id="0739f-131">Unlike other code in the iterator the `null` validation will occur when the iterator method is invoked, not when the underlying enumerator is walked.</span></span> <span data-ttu-id="0739f-132">这适用于传统迭代器或 `async` 迭代器。</span><span class="sxs-lookup"><span data-stu-id="0739f-132">This is true for traditional or `async` iterators.</span></span>

``` csharp
class Iterators {
    IEnumerable<char> GetCharacters(string s!) {
        foreach (var c in s) {
            yield return c;
        }
    }

    void Use() {
        // The invocation of GetCharacters will throw
        IEnumerable<char> e = GetCharacters(null);
    }
}
```

<span data-ttu-id="0739f-133">`!` 运算符仅可用于具有关联方法体的参数列表。</span><span class="sxs-lookup"><span data-stu-id="0739f-133">The `!` operator can only be used for parameter lists which have an associated method body.</span></span> <span data-ttu-id="0739f-134">这意味着不能在 `abstract` 方法、`interface`、`delegate` 或 `partial` 方法定义中使用它。</span><span class="sxs-lookup"><span data-stu-id="0739f-134">This means it cannot be used in an `abstract` method, `interface`, `delegate` or `partial` method definition.</span></span>

### <a name="extending-is-null"></a><span data-ttu-id="0739f-135">扩展为 null</span><span class="sxs-lookup"><span data-stu-id="0739f-135">Extending is null</span></span>
<span data-ttu-id="0739f-136">表达式 `is null` 的有效类型将进行扩展以包含不受约束的类型参数。</span><span class="sxs-lookup"><span data-stu-id="0739f-136">The types for which the expression `is null` is valid will be extended to include unconstrained type parameters.</span></span> <span data-ttu-id="0739f-137">这将允许它填写检查 `null` 检查是否有效的所有类型的 `null` 的意图。</span><span class="sxs-lookup"><span data-stu-id="0739f-137">This will allow it to fill the intent of checking for `null` on all types which a `null` check is valid.</span></span> <span data-ttu-id="0739f-138">具体来说，就是不能知道其值类型的类型。</span><span class="sxs-lookup"><span data-stu-id="0739f-138">Specifically that is types which are not definitely known to be value types.</span></span> <span data-ttu-id="0739f-139">例如，约束为 `struct` 的类型形参不能与此语法一起使用。</span><span class="sxs-lookup"><span data-stu-id="0739f-139">For example Type parameters which are constrained to `struct` cannot be used with this syntax.</span></span>

``` csharp
void NullCheck<T1, T2>(T1 p1, T2 p2) where T2 : struct {
    // Okay: T1 could be a class or struct here.
    if (p1 is null) {
        ...
    }

    // Error 
    if (p2 is null) { 
        ...
    }
}
```

<span data-ttu-id="0739f-140">在类型参数上 `is null` 的行为将与今天 `== null` 相同。</span><span class="sxs-lookup"><span data-stu-id="0739f-140">The behavior of `is null` on a type parameter will be the same as `== null` today.</span></span> <span data-ttu-id="0739f-141">在将类型参数实例化为值类型的情况下，代码将被评估为 `false`。</span><span class="sxs-lookup"><span data-stu-id="0739f-141">In the cases where the type parameter is instantiated as a value type the code will be evaluated as `false`.</span></span> <span data-ttu-id="0739f-142">对于引用类型，代码将执行正确的 `is null` 检查。</span><span class="sxs-lookup"><span data-stu-id="0739f-142">For cases where it is a reference type the code will do a proper `is null` check.</span></span>

### <a name="intersection-with-nullable-reference-types"></a><span data-ttu-id="0739f-143">具有可以为 Null 的引用类型的交集</span><span class="sxs-lookup"><span data-stu-id="0739f-143">Intersection with Nullable Reference Types</span></span>
<span data-ttu-id="0739f-144">应用于名称的 `!` 运算符的任何参数将以可以为 null 的状态从 "未 `null`" 开始。</span><span class="sxs-lookup"><span data-stu-id="0739f-144">Any parameter which has a `!` operator applied to it's name will start with the nullable state being not `null`.</span></span> <span data-ttu-id="0739f-145">即使参数本身的类型可能 `null`，也是如此。</span><span class="sxs-lookup"><span data-stu-id="0739f-145">This is true even if the type of the parameter itself is potentially `null`.</span></span> <span data-ttu-id="0739f-146">使用显式可以为 null 的类型（如 `string?`）或使用不受约束的类型参数可能出现这种情况。</span><span class="sxs-lookup"><span data-stu-id="0739f-146">That can occur with an explicitly nullable type, such as say `string?`, or with an unconstrained type parameter.</span></span> 

<span data-ttu-id="0739f-147">当参数的 `!` 语法与参数上的可为 null 的类型组合时，编译器将发出警告：</span><span class="sxs-lookup"><span data-stu-id="0739f-147">When a `!` syntax on parameters is combined with an explicitly nullable type on the parameter then a warning will be issued by the compiler:</span></span>

``` csharp
void WarnCase<T>(
    string? name!, // Warning: combining explicit null checking with a nullable type
    T value1 // Okay
)
```

## <a name="open-issues"></a><span data-ttu-id="0739f-148">打开的问题</span><span class="sxs-lookup"><span data-stu-id="0739f-148">Open Issues</span></span>
<span data-ttu-id="0739f-149">无</span><span class="sxs-lookup"><span data-stu-id="0739f-149">None</span></span>

## <a name="considerations"></a><span data-ttu-id="0739f-150">注意事项</span><span class="sxs-lookup"><span data-stu-id="0739f-150">Considerations</span></span>

### <a name="constructors"></a><span data-ttu-id="0739f-151">构造函数</span><span class="sxs-lookup"><span data-stu-id="0739f-151">Constructors</span></span>
<span data-ttu-id="0739f-152">构造函数的代码生成意味着在当前从标准 `null` 验证转换为 `null` 验证参数语法（`!`）时，有一个小但可观察的行为更改。</span><span class="sxs-lookup"><span data-stu-id="0739f-152">The code generation for constructors means there is a small, but observable, behavior change when moving from standard `null` validation today and the `null` validation parameter syntax (`!`).</span></span> <span data-ttu-id="0739f-153">`null` 签入标准验证在字段初始值设定项和任何 `base` 或 `this` 调用之后发生。</span><span class="sxs-lookup"><span data-stu-id="0739f-153">The `null` check in standard validation occurs after both field initializers and any `base` or `this` calls.</span></span> <span data-ttu-id="0739f-154">这意味着开发人员不一定要将100% 的 `null` 验证迁移到新的语法。</span><span class="sxs-lookup"><span data-stu-id="0739f-154">This means a developer can't necessarily migrate 100% of their `null` validation to the new syntax.</span></span> <span data-ttu-id="0739f-155">构造函数至少需要一些检查。</span><span class="sxs-lookup"><span data-stu-id="0739f-155">Constructors at least require some inspection.</span></span>

<span data-ttu-id="0739f-156">讨论完毕后，这种情况很不可能导致任何重大的采用问题。</span><span class="sxs-lookup"><span data-stu-id="0739f-156">After discussion though it was decided that this is very unlikely to cause any significant adoption issues.</span></span> <span data-ttu-id="0739f-157">`null` 检查在构造函数中的任何逻辑之前都要运行的逻辑更多。</span><span class="sxs-lookup"><span data-stu-id="0739f-157">It's more logical that the `null` check run before any logic in the constructor does.</span></span> <span data-ttu-id="0739f-158">如果发现了重要的兼容性问题，则可以重新访问。</span><span class="sxs-lookup"><span data-stu-id="0739f-158">Can revisit if significant compat issues are discovered.</span></span>

### <a name="warning-when-mixing--and-"></a><span data-ttu-id="0739f-159">混合时出现警告？</span><span class="sxs-lookup"><span data-stu-id="0739f-159">Warning when mixing ?</span></span> <span data-ttu-id="0739f-160">与!</span><span class="sxs-lookup"><span data-stu-id="0739f-160">and !</span></span>
<span data-ttu-id="0739f-161">在将 `!` 语法应用于显式键入为可为 null 的类型的参数时，是否应发出警告。</span><span class="sxs-lookup"><span data-stu-id="0739f-161">There was a lengthy discussion on whether or not a warning should be issued when the `!` syntax is applied to a parameter which is explicitly typed to a nullable type.</span></span> <span data-ttu-id="0739f-162">在表面上，开发人员似乎是过程声明，但在某些情况下，类型层次结构可能会强制开发人员进入这种情况。</span><span class="sxs-lookup"><span data-stu-id="0739f-162">On the surface it seems like a nonsensical declaration by the developer but there are cases where type hierarchies could force developers into such a situation.</span></span> 

<span data-ttu-id="0739f-163">请考虑在一系列程序集中使用以下类层次结构（假设所有项都是在启用 `null` 检查的情况下编译的）：</span><span class="sxs-lookup"><span data-stu-id="0739f-163">Consider the following class hierarchy across a series of assemblies (assuming all are compiled with `null` checking enabled):</span></span>

``` csharp
// Assembly1
abstract class C1 {
    protected abstract void M(object o); 
}

// Assembly2
abstract class C2 : C1 {

}

// Assembly3
abstract class C3 : C2 { 
    protected override void M(object o!) {
        ...
    }
}
```

<span data-ttu-id="0739f-164">此处的作者 `C3` 决定向参数 `o`添加 `null` 验证。</span><span class="sxs-lookup"><span data-stu-id="0739f-164">Here the author of `C3` decided to add `null` validation to the parameter `o`.</span></span> <span data-ttu-id="0739f-165">这完全与如何使用该功能有关。</span><span class="sxs-lookup"><span data-stu-id="0739f-165">This is completely in line with how the feature is intended to be used.</span></span>

<span data-ttu-id="0739f-166">现在，假设 Assembly2 的作者决定添加以下替代：</span><span class="sxs-lookup"><span data-stu-id="0739f-166">Now imagine at a later date the author of Assembly2 decides to add the following override:</span></span>

``` csharp
// Assembly2
abstract class C2 : C1 {
   protected override void M(object? o) { 
       ...
   }
}
```

<span data-ttu-id="0739f-167">这是可空引用类型允许的，因为它是合法的，使得协定对于输入位置更灵活。</span><span class="sxs-lookup"><span data-stu-id="0739f-167">This is allowed by nullable reference types as it's legal to make the contract more flexible for input positions.</span></span> <span data-ttu-id="0739f-168">通常，NRT 功能允许在参数/返回为空性上具有合理的 co/逆变。</span><span class="sxs-lookup"><span data-stu-id="0739f-168">The NRT feature in general allows for reasonable co/contravariance on parameter / return nullability.</span></span> <span data-ttu-id="0739f-169">不过，该语言根据最具体的替代（而不是原始声明）进行 co/逆变检查。</span><span class="sxs-lookup"><span data-stu-id="0739f-169">However the language does the co/contravariance checking based on the most specific override, not the original declaration.</span></span> <span data-ttu-id="0739f-170">这意味着 Assembly3 的作者将收到一条有关不匹配 `o` 类型的警告，需要将签名更改为以下内容，以消除此类情况：</span><span class="sxs-lookup"><span data-stu-id="0739f-170">This means the author of Assembly3 will get a warning about the type of `o` not matching and will need to change the signature to the following to eliminate it:</span></span> 

``` csharp
// Assembly3
abstract class C3 : C2 { 
    protected override void M(object? o!) {
        ...
    }
}
```

<span data-ttu-id="0739f-171">此时，Assembly3 的作者有几个选择：</span><span class="sxs-lookup"><span data-stu-id="0739f-171">At this point the author of Assembly3 has a few choices:</span></span>

- <span data-ttu-id="0739f-172">它们可以接受/禁止显示关于 `object?` 和 `object` 不匹配的警告。</span><span class="sxs-lookup"><span data-stu-id="0739f-172">They can accept / suppress the warning about `object?` and `object` mismatch.</span></span>
- <span data-ttu-id="0739f-173">它们可以接受/禁止显示关于 `object?` 和 `!` 不匹配的警告。</span><span class="sxs-lookup"><span data-stu-id="0739f-173">They can accept / suppress the warning about `object?` and `!` mismatch.</span></span>
- <span data-ttu-id="0739f-174">它们只需删除 `null` 验证检查（删除 `!` 并执行显式检查）</span><span class="sxs-lookup"><span data-stu-id="0739f-174">They can just remove the `null` validation check (delete `!` and do explicit checking)</span></span>

<span data-ttu-id="0739f-175">这是一个真实的方案，但现在这一理念是继续使用警告。</span><span class="sxs-lookup"><span data-stu-id="0739f-175">This is a real scenario but for now the idea is to move forward with the warning.</span></span> <span data-ttu-id="0739f-176">如果发现警告的发生频率比我们预测的更频繁，则可以在以后删除（相反）。</span><span class="sxs-lookup"><span data-stu-id="0739f-176">If it turns out the warning happens more frequently than we anticipate then we can remove it later (the reverse is not true).</span></span>

### <a name="implicit-property-setter-arguments"></a><span data-ttu-id="0739f-177">隐式属性 setter 参数</span><span class="sxs-lookup"><span data-stu-id="0739f-177">Implicit property setter arguments</span></span>
<span data-ttu-id="0739f-178">参数的 `value` 参数是隐式的，并且不会出现在任何参数列表中。</span><span class="sxs-lookup"><span data-stu-id="0739f-178">The `value` argument of a parameter is implicit and does not appear in any parameter list.</span></span> <span data-ttu-id="0739f-179">这意味着它不能是此功能的目标。</span><span class="sxs-lookup"><span data-stu-id="0739f-179">That means it cannot be a target of this feature.</span></span> <span data-ttu-id="0739f-180">可以扩展属性 setter 语法，使其包含参数列表，以便应用 `!` 运算符。</span><span class="sxs-lookup"><span data-stu-id="0739f-180">The property setter syntax could be extended to include a parameter list to allow the `!` operator to be applied.</span></span> <span data-ttu-id="0739f-181">但这种方法会使 `null` 验证变得更简单。</span><span class="sxs-lookup"><span data-stu-id="0739f-181">But that cuts against the idea of this feature making `null` validation simpler.</span></span> <span data-ttu-id="0739f-182">因此，隐式 `value` 参数不会使用此功能。</span><span class="sxs-lookup"><span data-stu-id="0739f-182">As such the implicit `value` argument just won't work with this feature.</span></span>

## <a name="future-considerations"></a><span data-ttu-id="0739f-183">未来的注意事项</span><span class="sxs-lookup"><span data-stu-id="0739f-183">Future Considerations</span></span>
<span data-ttu-id="0739f-184">无</span><span class="sxs-lookup"><span data-stu-id="0739f-184">None</span></span>
