---
ms.openlocfilehash: 77c9ffda3a59cc5f3dcc3ec9848600c6c5e03eed
ms.sourcegitcommit: 27487fa0294f4cdb7e5f2478884149e05314fd8a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2019
ms.locfileid: "79483900"
---
# <a name="primary-constructors"></a><span data-ttu-id="b6d3e-101">主构造函数</span><span class="sxs-lookup"><span data-stu-id="b6d3e-101">Primary constructors</span></span>

* <span data-ttu-id="b6d3e-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="b6d3e-102">[x] Proposed</span></span>
* <span data-ttu-id="b6d3e-103">[] 原型：未启动</span><span class="sxs-lookup"><span data-stu-id="b6d3e-103">[ ] Prototype: Not started</span></span>
* <span data-ttu-id="b6d3e-104">[] 实现：未启动</span><span class="sxs-lookup"><span data-stu-id="b6d3e-104">[ ] Implementation: Not started</span></span>
* <span data-ttu-id="b6d3e-105">[] 规范：未启动</span><span class="sxs-lookup"><span data-stu-id="b6d3e-105">[ ] Specification: Not started</span></span>

## <a name="summary"></a><span data-ttu-id="b6d3e-106">总结</span><span class="sxs-lookup"><span data-stu-id="b6d3e-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="b6d3e-107">类可以具有参数列表，在它们执行时，它们的基类规范可以有参数列表。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-107">Classes can have a parameter list, and when they do, their base class specification can have an argument list.</span></span>
<span data-ttu-id="b6d3e-108">主构造函数参数在整个类声明的范围内，如果它们是由函数成员或匿名函数捕获的，它们将作为私有字段存储在类中。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-108">Primary constructor parameters are in scope throughout the class declaration, and if they are captured by a function member or anonymous function, they are stored as private fields in the class.</span></span>

## <a name="motivation"></a><span data-ttu-id="b6d3e-109">动机</span><span class="sxs-lookup"><span data-stu-id="b6d3e-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="b6d3e-110">程序初始化代码中有很多样板。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-110">It is common to have a lot of boilerplate in program initialization code.</span></span> <span data-ttu-id="b6d3e-111">通常情况下，会多次提到一段给定的数据 `x`：</span><span class="sxs-lookup"><span data-stu-id="b6d3e-111">In the general case, a given piece of data `x` is mentioned many times:</span></span>

- <span data-ttu-id="b6d3e-112">作为专用字段 `_x`</span><span class="sxs-lookup"><span data-stu-id="b6d3e-112">As a private field `_x`</span></span>
- <span data-ttu-id="b6d3e-113">作为参数 `x` 到构造函数</span><span class="sxs-lookup"><span data-stu-id="b6d3e-113">As a parameter `x` to a constructor</span></span>
- <span data-ttu-id="b6d3e-114">在中，从构造函数中的参数对字段的赋值 `_x = x;`</span><span class="sxs-lookup"><span data-stu-id="b6d3e-114">In an assignment `_x = x;` of the field from the parameter in the constructor</span></span>
- <span data-ttu-id="b6d3e-115">作为属性 `X`</span><span class="sxs-lookup"><span data-stu-id="b6d3e-115">As a property `X`</span></span>
- <span data-ttu-id="b6d3e-116">在属性 setter 中 `x = value;`</span><span class="sxs-lookup"><span data-stu-id="b6d3e-116">In the property setter `x = value;`</span></span>
- <span data-ttu-id="b6d3e-117">在属性 getter 中 `return x;`</span><span class="sxs-lookup"><span data-stu-id="b6d3e-117">In the property getter `return x;`</span></span>

``` c#
class C
{
    private string _x;
    
    public C(string x)
    {
        _x = x;
    }
    public string X
    {
        get => _x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); _x = value; }
    }
}
```

<span data-ttu-id="b6d3e-118">对于不需要验证或计算的属性，可以使用自动属性减少麻烦，从而不必为属性声明显式支持字段。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-118">For properties that don't require validation or computation, the tedium can be reduced using auto-properties, thus cutting out the need to declare an explicit backing field for the property.</span></span> <span data-ttu-id="b6d3e-119">但是，如果您的属性需要任何类型的逻辑，而不是自动属性提供的任何类型，则可以使用以上哪种方式。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-119">But if your property requires any sort of logic beyond what an auto-property provides, the above is the best you an do.</span></span>

<span data-ttu-id="b6d3e-120">主构造函数通过将构造函数参数直接放置在整个类的范围中来降低开销，并再次避免需要显式声明支持字段。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-120">Primary constructors instead reduce the overhead by putting constructor arguments directly in scope throughout the class, again obviating the need to explicitly declare a backing field.</span></span> <span data-ttu-id="b6d3e-121">因此，上面的示例将变为：</span><span class="sxs-lookup"><span data-stu-id="b6d3e-121">Thus, the above example would become:</span></span>

``` c#
class C(string x)
{
    public string X
    {
        get => x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); x = value; }
    }
}
```

<span data-ttu-id="b6d3e-122">在此示例中，主构造函数会将 `x` 的命名实体数从三个减少到两个，即 `_x` 支持字段避免。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-122">In this example, the primary constructor reduces the number of named entities for `x` from three to two, obviating the `_x` backing field.</span></span> <span data-ttu-id="b6d3e-123">它消除了三个成员声明中的两个（仅保留属性声明本身），并减少了从八到五台的 `x`/`_x``X` /的总数量。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-123">It removes two out of three member declarations (keeping only the property declaration itself), and reduces the total number of mentions of `x`/`_x`/`X` from eight to five.</span></span>


## <a name="detailed-design"></a><span data-ttu-id="b6d3e-124">详细设计</span><span class="sxs-lookup"><span data-stu-id="b6d3e-124">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="b6d3e-125">类可以具有参数列表：</span><span class="sxs-lookup"><span data-stu-id="b6d3e-125">Classes can have a parameter list:</span></span>

``` c#
public class C(int i, string s)
{
    ...
}
```

<span data-ttu-id="b6d3e-126">参数列表导致为类隐式声明构造函数，并且具有与类本身相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-126">The parameter list causes a constructor to be implicitly declared for the class, with the same accessibility as the class itself.</span></span>

``` c#
new C(5, "Hello");
```

<span data-ttu-id="b6d3e-127">主构造函数参数在整个类体内的范围内。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-127">Primary constructor parameters are in scope throughout the class body.</span></span> <span data-ttu-id="b6d3e-128">如果由函数成员或匿名函数捕获它们，则它们将作为私有字段存储在类中。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-128">If they are captured by a function member or anonymous function, they become stored as private fields in the class.</span></span> <span data-ttu-id="b6d3e-129">如果它们仅在初始化期间使用，则不会将它们存储在对象中。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-129">If they are only used during initialization they will not be stored in the object.</span></span>

``` c#
public class C(int i, string s)
{
    int[] a = new int[i]; // i not captured
    public int S => s;    // s captured
}
```

<span data-ttu-id="b6d3e-130">如果具有主构造函数的类具有一个基类规范，则它可以有一个参数列表。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-130">If a class with a primary constructor has a base class specification, that one can have an argument list.</span></span> <span data-ttu-id="b6d3e-131">这作为隐式声明的构造函数的 `base(...)` 初始值设定项的参数列表。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-131">This serves as the argument list to a `base(...)` initializer of the implicitly declared constructor.</span></span> <span data-ttu-id="b6d3e-132">如果未提供自变量列表，则假定为空。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-132">If no argument list is provided, an empty one is assumed.</span></span>

``` c#
public class C(int i, string s) : B(s)
{
    ...
}
```
<span data-ttu-id="b6d3e-133">类也可以具有显式定义的构造函数，但它们都必须使用 `this(...)` 初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-133">The class can have explicitly defined constructors as well, but those all have to use a `this(...)` initializer.</span></span> <span data-ttu-id="b6d3e-134">这可确保在构造新的实例时始终调用主构造函数。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-134">This ensures that the primary constructor is always called when a new instance is constructed.</span></span>

<span data-ttu-id="b6d3e-135">类体中的所有初始值设定项都将成为生成的构造函数中的赋值。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-135">All initializers in the class body will become assignments in the generated constructor.</span></span> <span data-ttu-id="b6d3e-136">这意味着，与其他类不同，初始值设定项将在调用基构造函数*之后*（而不是在之前）运行。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-136">This means that, unlike other classes, initializers will run *after* the base constructor has been invoked, not before.</span></span> <span data-ttu-id="b6d3e-137">此外，生成的类将包含用于初始化任何私有字段的分配，这些私有字段是为存储由成员捕获的主构造函数参数而生成的。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-137">In addition, the generated class will contain assignments to initialize any private fields that were generated to store primary constructor parameters that were captured by members.</span></span> <span data-ttu-id="b6d3e-138">将这些成员重写为使用私有字段，而不是以类似于 lambda 表达式的闭包的方式使用该参数。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-138">Those members are rewritten to use the private field instead of the parameter in a manner similar to closures for lambda expressions.</span></span> <span data-ttu-id="b6d3e-139">首先初始化生成的主字段，然后在类中以外观的顺序执行生成的初始化分配。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-139">The generated primary fields are initialized first, and then the initializer-generated assignments are executed in the order of appearance in the class.</span></span>

<span data-ttu-id="b6d3e-140">在上面的示例中，类声明的作用类似于重写如下：</span><span class="sxs-lookup"><span data-stu-id="b6d3e-140">For the above example, the effect of the class declaration is as if rewritten like this:</span></span>

``` c#
public class C : B
{
    public C(int i, string s) : base(s)
    {
        __s = s;        // store parameter s for captured use
        a = new int[i]; // initialize a
    }
    int __s; // generated field for capture of s
    
    int[] a;
    public int S => __s; // s replaced with captured __s
}
```

<span data-ttu-id="b6d3e-141">捕获与 lambda 表达式捕获本地变量的限制相同。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-141">The capture has similar restrictions to the capture of local variables by lambda expressions.</span></span> <span data-ttu-id="b6d3e-142">例如，主构造函数中允许 `ref` 和 `out` 参数，但无法捕获我的成员正文。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-142">For instance, `ref` and `out` parameters are allowed in primary constructors, but cannot be captured my member bodies.</span></span>


## <a name="drawbacks"></a><span data-ttu-id="b6d3e-143">缺点</span><span class="sxs-lookup"><span data-stu-id="b6d3e-143">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="b6d3e-144">按重要性排序。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-144">In rough order of significance.</span></span>

* <span data-ttu-id="b6d3e-145">提议使用的语法也已针对位置记录进行了建议。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-145">The proposal uses syntax that has also been proposed for positional records.</span></span> <span data-ttu-id="b6d3e-146">如果我们需要这两项功能，则需要一些便利设施。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-146">If we desire both features, some accommodation is required.</span></span> <span data-ttu-id="b6d3e-147">例如</span><span class="sxs-lookup"><span data-stu-id="b6d3e-147">E.g.</span></span> <span data-ttu-id="b6d3e-148">建议对记录使用 `data` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-148">a `data` modifier on records has been proposed.</span></span>
* <span data-ttu-id="b6d3e-149">构造对象的分配大小不太明显，因为编译器会根据类的完整文本来确定是否为主构造函数参数分配字段。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-149">The allocation size of constructed objects is less obvious, as the compiler determines whether to allocate a field for a primary constructor parameter based on the full text of the class.</span></span> <span data-ttu-id="b6d3e-150">此风险类似于隐式捕获 lambda 表达式中的变量。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-150">This risk is similar to the implicit capture of variables by lambda expressions.</span></span>
* <span data-ttu-id="b6d3e-151">常见的方法（或意外模式）可能是捕获多个继承级别上的 "相同" 参数，因为它是在传递构造函数链时，而不是在基类上将其 allotting用于对象中的相同数据。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-151">A common temptation (or accidental pattern) might be to capture the "same" parameter at multiple levels of inheritance as it is passed up the constructor chain instead of explicitly allotting it a protected field at the base class, leading to duplicated allocations for the same data in objects.</span></span> <span data-ttu-id="b6d3e-152">这非常类似于如今通过自动属性重写自动属性的风险。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-152">This is very similar to today's risk of overriding auto-properties with auto-properties.</span></span> 
* <span data-ttu-id="b6d3e-153">如以上所述，对于可能通常在构造函数主体中表示的其他逻辑，没有任何位置。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-153">As proposed above, there is no place for additional logic that might usually expressed in constructor bodies.</span></span> <span data-ttu-id="b6d3e-154">下面的 "主构造函数正文" 扩展可解决。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-154">The "Primary constructor bodies" extension below addresses that.</span></span>
* <span data-ttu-id="b6d3e-155">如建议，执行顺序语义与普通构造函数稍有不同。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-155">As proposed, execution order semantics are subtly different than with ordinary constructors.</span></span> <span data-ttu-id="b6d3e-156">这可能会被修正，但代价是某些扩展方案（特别是 "主构造函数主体"）的成本。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-156">This could probably be remedied, but at the cost of some of the extension proposals (notably "Primary constructor bodies").</span></span>
* <span data-ttu-id="b6d3e-157">建议仅在可以将单个构造函数指定为主构造函数时才起作用。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-157">The proposal only works when a single constructor can be designated primary.</span></span>
* <span data-ttu-id="b6d3e-158">无法对类和主构造函数进行单独的可访问性。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-158">There is no way to have separate accessibility of the class and the primary constructor.</span></span> <span data-ttu-id="b6d3e-159">例如，公共构造函数都委托给一个需要的私有 "生成-全部" 构造函数。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-159">For instance, in situations where public constructors all delegate to one private "build-it-all" constructor that would be needed.</span></span> <span data-ttu-id="b6d3e-160">如有必要，稍后可能会推荐语法。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-160">If necessary, syntax could be proposed for that later.</span></span>


## <a name="alternatives"></a><span data-ttu-id="b6d3e-161">备选项</span><span class="sxs-lookup"><span data-stu-id="b6d3e-161">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="b6d3e-162">完整的位置记录可以是替代项，也可以与主构造函数共存，具体取决于具体的具体情况。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-162">Full-on positional records may be an alternative, or may coexist with primary constructors, depending on the specifics.</span></span> <span data-ttu-id="b6d3e-163">它们允许在更*少*的方案中使用*更多*的缩写。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-163">They would allow for *more* abbreviation in a *smaller* number of scenarios.</span></span> <span data-ttu-id="b6d3e-164">这两者都可能很有用，但同时也可能会多余，除非它们可以彼此地彼此集成。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-164">So both are potentially useful, but having both may be overkill, unless they can be somewhat neatly integrated with each other.</span></span>


## <a name="possible-extensions"></a><span data-ttu-id="b6d3e-165">可能的延伸</span><span class="sxs-lookup"><span data-stu-id="b6d3e-165">Possible extensions</span></span>
[extensions]: #possible-extensions

<span data-ttu-id="b6d3e-166">它们是核心提议的变体或补充，可能会将其与之结合，或在更高的阶段（如果认为有用）。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-166">These are variations or additions to the core proposal that may be considered in conjunction with it, or at a later stage if deemed useful.</span></span>

### <a name="primary-constructor-bodies"></a><span data-ttu-id="b6d3e-167">主构造函数主体</span><span class="sxs-lookup"><span data-stu-id="b6d3e-167">Primary constructor bodies</span></span>

<span data-ttu-id="b6d3e-168">构造函数本身通常包含参数验证逻辑，或不能表示为初始值设定项的其他重要初始化代码。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-168">Constructors themselves often contain parameter validation logic or other nontrivial initialization code that cannot be expressed as initializers.</span></span>

<span data-ttu-id="b6d3e-169">可以扩展主构造函数以允许语句块直接出现在类体中。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-169">Primary constructors could be extended to allow statement blocks to appear directly in the class body.</span></span> <span data-ttu-id="b6d3e-170">这些语句会在生成的构造函数中插入到它们在初始化赋值之间出现的位置，因此会执行与初始值设定项的交错。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-170">Those statements would be inserted in the generated constructor at the point where they appear between initializing assignments, and would thus be executed interspersed with initializers.</span></span> <span data-ttu-id="b6d3e-171">例如：</span><span class="sxs-lookup"><span data-stu-id="b6d3e-171">For instance:</span></span>

``` c#
public class C(int i, string s) : B(s)
{
    {
        if (i < 0) throw new ArgumentOutOfRangeException(nameof(i));
    }
    int[] a = new int[i];
    public int S => s;
}
```

### <a name="initializer-fields-and-initializer-functions"></a><span data-ttu-id="b6d3e-172">初始值设定项字段和初始值设定项函数</span><span class="sxs-lookup"><span data-stu-id="b6d3e-172">Initializer fields and initializer functions</span></span>

<span data-ttu-id="b6d3e-173">在具有主构造函数的类中，我们可以将没有可访问性修饰符的字段和方法声明视为更像局部变量和本地函数：</span><span class="sxs-lookup"><span data-stu-id="b6d3e-173">In a class with a primary constructor we could consider field and method declarations without accessibility modifiers to be more like local variables and local functions:</span></span>

* <span data-ttu-id="b6d3e-174">与主构造函数参数一样，"初始值设定项字段" 仅在函数成员中使用时才会捕获到实际的私有字段。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-174">Just like primary constructor parameters the "initializer fields" would only be captured into an actual private field if they were used in function members.</span></span>
* <span data-ttu-id="b6d3e-175">仅当主构造函数参数和初始值设定项字段在其他函数成员中使用时，才会将 "初始值设定项函数" 视为捕获它们。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-175">The "initializer functions" would only be considered to capture primary constructor parameters and initializer fields if they were themselves used in other function members.</span></span> <span data-ttu-id="b6d3e-176">如果未捕获，则可以以更好的方式生成它们，如本地函数。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-176">If not captured, they could be generated in a more optimal fashion, like local functions.</span></span>
* <span data-ttu-id="b6d3e-177">与主构造函数参数一样，它们不能通过成员访问获得，只是简单名称。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-177">Just like primary constructor parameters they would not be available via member access, but only as a simple name.</span></span>

<span data-ttu-id="b6d3e-178">这可以用于只与初始化相关的临时变量和 helper 函数：</span><span class="sxs-lookup"><span data-stu-id="b6d3e-178">This could be used for temporary variables and helper functions that are only relevant to initialization:</span></span>

``` c#
public class C(string s)
{
    int size = s.Length;             // not captured
    int[] Create() => new int[size]; // not captured
    int[] a = Create();
    ...
}
```

<span data-ttu-id="b6d3e-179">这可能太微妙，特别是由于缺少辅助功能修饰符只是 `private`。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-179">This may be too subtle, especially since the absence of accessibility modifiers elsewhere simply means `private`.</span></span> 

### <a name="initializer-statements"></a><span data-ttu-id="b6d3e-180">初始值设定项语句</span><span class="sxs-lookup"><span data-stu-id="b6d3e-180">Initializer statements</span></span>

<span data-ttu-id="b6d3e-181">以上到扩展的根式组合只是直接允许在类体中使用语句。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-181">A radical combination of the above to extensions would be to simply allow statements directly in the class body.</span></span> <span data-ttu-id="b6d3e-182">此类语句与上面建议的交错构造函数体完全相同，只不过它们不需要包含在 `{ }`中。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-182">Such statements are exactly as the interspersed constructor bodies proposed above, except they don't need to be enclosed in `{ }`.</span></span> <span data-ttu-id="b6d3e-183">为了充分利用这一点，"本地" 变量和帮助程序函数还需要在类的顶级以 "初始值设定项字段和初始值设定项函数" 中所述的方式来表达。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-183">For this to be sufficiently useful, "local" variables and helper functions would need to also be expressible at the top level of the class, in the manner explored in the extension "Initializer fields and initializer functions" above.</span></span>


### <a name="member-access"></a><span data-ttu-id="b6d3e-184">成员访问</span><span class="sxs-lookup"><span data-stu-id="b6d3e-184">Member access</span></span>

<span data-ttu-id="b6d3e-185">核心建议将主构造函数参数视为只能称为简单名称的参数。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-185">The core proposal treats primary constructor parameters as parameters that can only be referred as simple names.</span></span> <span data-ttu-id="b6d3e-186">一种替代方法是允许将它们作为私有字段（即使用成员访问）进行引用，即使它们有时不作为字段生成也是*如此*。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-186">An alternative is to allow them to be referenced as if they were private fields, i.e. with a member access, *even* if they are sometimes not generated as fields.</span></span> <span data-ttu-id="b6d3e-187">这将允许在按本地变量进行隐藏时将其作为 `this.x` 引用，并从不同的实例中访问 `other.x`。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-187">This would allow them to be referenced as `this.x` when shadowed by local variables, and accessed from a different instance as `other.x`.</span></span>

<span data-ttu-id="b6d3e-188">如果应用于 "初始值设定项字段和初始值设定项函数" 扩展，这也会降低与普通私有成员不同的程度。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-188">If applied to the "initializer fields and initializer functions" extension this would also reduce the degree to which those were different from ordinary private members.</span></span> <span data-ttu-id="b6d3e-189">唯一的区别是，如果仅在初始化过程中使用，编译器可以自由地从对象中 elide 它们。</span><span class="sxs-lookup"><span data-stu-id="b6d3e-189">The only difference would then be that the compiler is free to elide them from the object if only used during initialization.</span></span>

