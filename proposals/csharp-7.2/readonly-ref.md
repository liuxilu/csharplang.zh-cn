---
ms.openlocfilehash: 39fb0aab5e8bb0d422f25fd2e92ab3d8256d3f59
ms.sourcegitcommit: b8f1103eb686c5d82e294837c9386d9b667da292
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2019
ms.locfileid: "79484020"
---
# <a name="readonly-references"></a><span data-ttu-id="4d0d0-101">只读引用</span><span class="sxs-lookup"><span data-stu-id="4d0d0-101">Readonly references</span></span>

* <span data-ttu-id="4d0d0-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="4d0d0-102">[x] Proposed</span></span>
* <span data-ttu-id="4d0d0-103">[x] 原型</span><span class="sxs-lookup"><span data-stu-id="4d0d0-103">[x] Prototype</span></span>
* <span data-ttu-id="4d0d0-104">[x] 实现：已启动</span><span class="sxs-lookup"><span data-stu-id="4d0d0-104">[x] Implementation: Started</span></span>
* <span data-ttu-id="4d0d0-105">[] 规范：未启动</span><span class="sxs-lookup"><span data-stu-id="4d0d0-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="4d0d0-106">总结</span><span class="sxs-lookup"><span data-stu-id="4d0d0-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="4d0d0-107">"只读引用" 功能实际上是一组功能，这些功能利用通过引用传递变量的效率，但不会将数据公开给修改：</span><span class="sxs-lookup"><span data-stu-id="4d0d0-107">The "readonly references" feature is actually a group of features that leverage the efficiency of passing variables by reference, but without exposing the data to modifications:</span></span>  
- <span data-ttu-id="4d0d0-108">`in` 参数</span><span class="sxs-lookup"><span data-stu-id="4d0d0-108">`in` parameters</span></span>
- <span data-ttu-id="4d0d0-109">`ref readonly` 返回</span><span class="sxs-lookup"><span data-stu-id="4d0d0-109">`ref readonly` returns</span></span>
- <span data-ttu-id="4d0d0-110">`readonly` 结构</span><span class="sxs-lookup"><span data-stu-id="4d0d0-110">`readonly` structs</span></span>
- <span data-ttu-id="4d0d0-111">`ref`/`in` 扩展方法</span><span class="sxs-lookup"><span data-stu-id="4d0d0-111">`ref`/`in` extension methods</span></span>
- <span data-ttu-id="4d0d0-112">`ref readonly` 局部变量</span><span class="sxs-lookup"><span data-stu-id="4d0d0-112">`ref readonly` locals</span></span>
- <span data-ttu-id="4d0d0-113">`ref` 条件表达式</span><span class="sxs-lookup"><span data-stu-id="4d0d0-113">`ref` conditional expressions</span></span>

## <a name="passing-arguments-as-readonly-references"></a><span data-ttu-id="4d0d0-114">作为 readonly 引用传递参数。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-114">Passing arguments as readonly references.</span></span>

<span data-ttu-id="4d0d0-115">现有的建议与本主题 https://github.com/dotnet/roslyn/issues/115 为只读参数的特例，而不会有许多详细信息。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-115">There is an existing proposal that touches this topic https://github.com/dotnet/roslyn/issues/115 as a special case of readonly parameters without going into many details.</span></span>
<span data-ttu-id="4d0d0-116">在这里，我只是要承认自己的想法并不是很新的。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-116">Here I just want to acknowledge that the idea by itself is not very new.</span></span>

### <a name="motivation"></a><span data-ttu-id="4d0d0-117">动机</span><span class="sxs-lookup"><span data-stu-id="4d0d0-117">Motivation</span></span>

<span data-ttu-id="4d0d0-118">在此功能C#之前，没有一种有效的方法来表示将结构变量传递到方法调用以实现 readonly，无需修改。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-118">Prior to this feature C# did not have an efficient way of expressing a desire to pass struct variables into method calls for readonly purposes with no intention of modifying.</span></span> <span data-ttu-id="4d0d0-119">标准的按值参数传递意味着复制，这会增加不必要的成本。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-119">Regular by-value argument passing implies copying, which adds unnecessary costs.</span></span>  <span data-ttu-id="4d0d0-120">这会使用户使用按引用参数传递，并依赖注释/文档来指示数据不应由被调用方转变。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-120">That drives users to use by-ref argument passing and rely on comments/documentation to indicate that the data is not supposed to be mutated by the callee.</span></span> <span data-ttu-id="4d0d0-121">这不是一个很好的解决方案，原因很多。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-121">It is not a good solution for many reasons.</span></span>  
<span data-ttu-id="4d0d0-122">这些示例是图形库中的大量矢量/矩阵数学运算符，[例如，](https://msdn.microsoft.com/library/bb194944.aspx)由于性能方面的考虑，它会知道，只需使用 ref 操作数。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-122">The examples are numerous - vector/matrix math operators in graphics libraries like [XNA](https://msdn.microsoft.com/library/bb194944.aspx) are known to have ref operands purely because of performance considerations.</span></span> <span data-ttu-id="4d0d0-123">Roslyn 编译器本身有一些代码，它使用结构来避免分配，并按引用传递它们，以避免复制成本。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-123">There is code in Roslyn compiler itself that uses structs to avoid allocations and then passes them by reference to avoid copying costs.</span></span>

### <a name="solution-in-parameters"></a><span data-ttu-id="4d0d0-124">解决方案（`in` 参数）</span><span class="sxs-lookup"><span data-stu-id="4d0d0-124">Solution (`in` parameters)</span></span>

<span data-ttu-id="4d0d0-125">与 `out` 参数类似，`in` 参数作为托管引用传递，并具有来自被调用方的其他保证。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-125">Similarly to the `out` parameters, `in` parameters are passed as managed references with additional guarantees from the callee.</span></span>  
<span data-ttu-id="4d0d0-126">与在任何其他使用之前_必须_由被调用方分配的 `out` 参数不同，`in` 的参数无法由被调用方分配。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-126">Unlike `out` parameters which _must_ be assigned by the callee before any other use, `in` parameters cannot be assigned by the callee at all.</span></span>

<span data-ttu-id="4d0d0-127">因此，`in` 参数可实现间接参数传递的有效性，而不会向被调用方公开突变的参数。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-127">As a result `in` parameters allow for effectiveness of indirect argument passing without exposing arguments to mutations by the callee.</span></span>

### <a name="declaring-in-parameters"></a><span data-ttu-id="4d0d0-128">声明 `in` 参数</span><span class="sxs-lookup"><span data-stu-id="4d0d0-128">Declaring `in` parameters</span></span>

<span data-ttu-id="4d0d0-129">`in` 参数是使用 `in` 关键字作为参数签名中的修饰符来声明的。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-129">`in` parameters are declared by using `in` keyword as a modifier in the parameter signature.</span></span>

<span data-ttu-id="4d0d0-130">出于所有目的，`in` 参数被视为 `readonly` 变量。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-130">For all purposes the `in` parameter is treated as a `readonly` variable.</span></span> <span data-ttu-id="4d0d0-131">在方法中对 `in` 参数的使用的大多数限制与 `readonly` 字段相同。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-131">Most of the restrictions on the use of `in` parameters inside the method are the same as with `readonly` fields.</span></span>

> <span data-ttu-id="4d0d0-132">的确 `in` 参数可以表示 `readonly` 字段。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-132">Indeed an `in` parameter may represent a `readonly` field.</span></span> <span data-ttu-id="4d0d0-133">限制的相似性并不是一种巧合。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-133">Similarity of restrictions is not a coincidence.</span></span>

<span data-ttu-id="4d0d0-134">对于包含结构类型 `in` 参数的示例字段，它们都以递归方式分类为 `readonly` 变量。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-134">For example fields of an `in` parameter which has a struct type are all recursively classified as `readonly` variables .</span></span>

```csharp
static Vector3 Add (in Vector3 v1, in Vector3 v2)
{
    // not OK!!
    v1 = default(Vector3);

    // not OK!!
    v1.X = 0;

    // not OK!!
    foo(ref v1.X);

    // OK
    return new Vector3(v1.X + v2.X, v1.Y + v2.Y, v1.Z + v2.Z);
}
```

- <span data-ttu-id="4d0d0-135">允许使用普通 byval 参数的任何位置都允许 `in` 参数。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-135">`in` parameters are allowed anywhere where ordinary byval parameters are allowed.</span></span> <span data-ttu-id="4d0d0-136">这包括索引器、运算符（包括转换）、委托、lambda、本地函数。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-136">This includes indexers, operators (including conversions), delegates, lambdas, local functions.</span></span>

> ```csharp
>  (in int x) => x                                                     // lambda expression  
>  TValue this[in TKey index];                                         // indexer
>  public static Vector3 operator +(in Vector3 x, in Vector3 y) => ... // operator
>  ```

- <span data-ttu-id="4d0d0-137">不允许将 `in` 与 `out` 或 `out` 未合并的任何内容结合使用。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-137">`in` is not allowed in combination with `out` or with anything that `out` does not combine with.</span></span>

- <span data-ttu-id="4d0d0-138">不允许重载 `ref`/`out`/`in` 差异。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-138">It is not permitted to overload on `ref`/`out`/`in` differences.</span></span>

- <span data-ttu-id="4d0d0-139">允许重载普通 byval，并 `in` 差异。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-139">It is permitted to overload on ordinary byval and `in` differences.</span></span>

- <span data-ttu-id="4d0d0-140">出于 OHI （重载、隐藏和实现）的目的，`in` 的行为类似于 `out` 参数。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-140">For the purpose of OHI (Overloading, Hiding, Implementing), `in` behaves similarly to an `out` parameter.</span></span>
<span data-ttu-id="4d0d0-141">所有相同的规则都适用。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-141">All the same rules apply.</span></span>
<span data-ttu-id="4d0d0-142">例如，重写方法必须与具有可转换类型的 `in` 参数的 `in` 参数匹配。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-142">For example the overriding method will have to match `in` parameters with `in` parameters of an identity-convertible type.</span></span>

- <span data-ttu-id="4d0d0-143">出于委托/lambda/方法组转换的目的，`in` 的行为类似于 `out` 参数。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-143">For the purpose of delegate/lambda/method group conversions, `in` behaves similarly to an `out` parameter.</span></span>
<span data-ttu-id="4d0d0-144">Lambda 和适用的方法组转换候选项必须与具有可转换类型的 `in` 参数的目标委托 `in` 参数匹配。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-144">Lambdas and applicable method group conversion candidates will have to match `in` parameters of the target delegate with `in` parameters of an identity-convertible type.</span></span>

- <span data-ttu-id="4d0d0-145">出于泛型方差的目的，`in` 参数是 nonvariant 的。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-145">For the purpose of generic variance, `in` parameters are nonvariant.</span></span>

> <span data-ttu-id="4d0d0-146">注意：对于具有引用类型或基元类型 `in` 参数没有警告。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-146">NOTE: There are no warnings on `in` parameters that have reference or primitives types.</span></span>
<span data-ttu-id="4d0d0-147">通常，它可能是毫无意义的，但在某些情况下，用户必须/希望将基元作为 `in`传递。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-147">It may be pointless in general, but in some cases user must/want to pass primitives as `in`.</span></span> <span data-ttu-id="4d0d0-148">示例-在 `T` 被替换为 `int`时，或在具有 `Volatile.Read(in int location)` 类似的方法时重写泛型方法，如 `Method(in T param)`</span><span class="sxs-lookup"><span data-stu-id="4d0d0-148">Examples - overriding a generic method like `Method(in T param)` when `T` was substituted to be `int`, or when having methods like `Volatile.Read(in int location)`</span></span>
>
> <span data-ttu-id="4d0d0-149">可以让分析器在低效使用 `in` 参数的情况下发出警告，但这种分析的规则会过于模糊，无法成为语言规范的一部分。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-149">It is conceivable to have an analyzer that warns in cases of inefficient use of `in` parameters, but the rules for such analysis would be too fuzzy to be a part of a language specification.</span></span>

### <a name="use-of-in-at-call-sites-in-arguments"></a><span data-ttu-id="4d0d0-150">在调用站点使用 `in`。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-150">Use of `in` at call sites.</span></span> <span data-ttu-id="4d0d0-151">（`in` 参数）</span><span class="sxs-lookup"><span data-stu-id="4d0d0-151">(`in` arguments)</span></span>

<span data-ttu-id="4d0d0-152">可以通过两种方式将参数传递给 `in` 参数。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-152">There are two ways to pass arguments to `in` parameters.</span></span>

#### <a name="in-arguments-can-match-in-parameters"></a><span data-ttu-id="4d0d0-153">`in` 参数可以与 `in` 参数匹配：</span><span class="sxs-lookup"><span data-stu-id="4d0d0-153">`in` arguments can match `in` parameters:</span></span>

<span data-ttu-id="4d0d0-154">调用站点上带有 `in` 修饰符的参数可以与 `in` 参数匹配。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-154">An argument with an `in` modifier at the call site can match `in` parameters.</span></span>

```csharp
int x = 1;

void M1<T>(in T x)
{
  // . . .
}

var x = M1(in x);  // in argument to a method

class D
{
    public string this[in Guid index];
}

D dictionary = . . . ;
var y = dictionary[in Guid.Empty]; // in argument to an indexer
```

- <span data-ttu-id="4d0d0-155">`in` 参数必须是_可读_的左值（\*）。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-155">`in` argument must be a _readable_ LValue(\*).</span></span>
<span data-ttu-id="4d0d0-156">示例： `M1(in 42)` 无效</span><span class="sxs-lookup"><span data-stu-id="4d0d0-156">Example: `M1(in 42)` is invalid</span></span>

> <span data-ttu-id="4d0d0-157">(\*)[LValue/RValue](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue)的概念因语言而异。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-157">(\*) The notion of [LValue/RValue](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue) vary between languages.</span></span>  
<span data-ttu-id="4d0d0-158">此处，按左值 I 表示一个表达式，该表达式表示可以直接引用的位置。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-158">Here, by LValue I mean an expression that represent a location that can be referred to directly.</span></span>
<span data-ttu-id="4d0d0-159">和 RValue 表示一个表达式，该表达式产生的临时结果不会自行保存。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-159">And RValue means an expression that yields a temporary result which does not persist on its own.</span></span>  

- <span data-ttu-id="4d0d0-160">具体而言，将 `readonly` 字段、`in` 参数或其他正式 `readonly` 变量作为 `in` 参数传递是有效的。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-160">In particular it is valid to pass `readonly` fields, `in` parameters or other formally `readonly` variables as `in` arguments.</span></span>
<span data-ttu-id="4d0d0-161">示例： `dictionary[in Guid.Empty]` 是合法的。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-161">Example: `dictionary[in Guid.Empty]` is legal.</span></span> <span data-ttu-id="4d0d0-162">`Guid.Empty` 为静态只读字段。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-162">`Guid.Empty` is a static readonly field.</span></span>

- <span data-ttu-id="4d0d0-163">`in` 参数的类型必须能够_转换_为参数的类型。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-163">`in` argument must have type _identity-convertible_ to the type of the parameter.</span></span>
<span data-ttu-id="4d0d0-164">示例： `M1<object>(in Guid.Empty)` 无效。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-164">Example: `M1<object>(in Guid.Empty)` is invalid.</span></span> <span data-ttu-id="4d0d0-165">`Guid.Empty` 不能_转换_为 `object`</span><span class="sxs-lookup"><span data-stu-id="4d0d0-165">`Guid.Empty` is not _identity-convertible_ to `object`</span></span>

<span data-ttu-id="4d0d0-166">上述规则的动机是 `in` 参数可保证参数变量的_别名_。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-166">The motivation for the above rules is that `in` arguments guarantee _aliasing_ of the argument variable.</span></span> <span data-ttu-id="4d0d0-167">被调用方始终接收对由参数表示的同一位置的直接引用。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-167">The callee always receives a direct reference to the same location as represented by the argument.</span></span>

- <span data-ttu-id="4d0d0-168">在极少数情况下，由于用作相同调用的操作数的 `await` 表达式，`in` 参数必须为堆栈溢出，因此该行为与 `out` 和 `ref` 参数相同-如果无法以引用透明方式溢出变量，则会报告错误。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-168">in rare situations when `in` arguments must be stack-spilled due to `await` expressions used as operands of the same call, the behavior is the same as with `out` and `ref` arguments - if the variable cannot be spilled in referentially-transparent manner, an error is reported.</span></span>

<span data-ttu-id="4d0d0-169">示例：</span><span class="sxs-lookup"><span data-stu-id="4d0d0-169">Examples:</span></span>
1. <span data-ttu-id="4d0d0-170">`M1(in staticField, await SomethingAsync())` 有效。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-170">`M1(in staticField, await SomethingAsync())`  is valid.</span></span>
<span data-ttu-id="4d0d0-171">`staticField` 是一种静态字段，可在不具有明显副作用的情况下多次访问。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-171">`staticField` is a static field which can be accessed more than once without observable side effects.</span></span> <span data-ttu-id="4d0d0-172">因此，可以提供副作用和别名要求的顺序。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-172">Therefore both the order of side effects and aliasing requirements can be provided.</span></span>
2. <span data-ttu-id="4d0d0-173">`M1(in RefReturningMethod(), await SomethingAsync())` 将产生一个错误。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-173">`M1(in RefReturningMethod(), await SomethingAsync())`  will produce an error.</span></span>
<span data-ttu-id="4d0d0-174">`RefReturningMethod()` 是 `ref` 返回方法。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-174">`RefReturningMethod()` is a `ref` returning method.</span></span> <span data-ttu-id="4d0d0-175">方法调用可能具有可观察到的副作用，因此必须在 `SomethingAsync()` 操作数之前计算。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-175">A method call may have observable side effects, therefore it must be evaluated before the `SomethingAsync()` operand.</span></span> <span data-ttu-id="4d0d0-176">但是，调用的结果是不能在 `await` 挂起点上保留的引用，这不会导致直接引用要求。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-176">However the result of the invocation is a reference that cannot be preserved across the `await` suspension point which make the direct reference requirement impossible.</span></span>

> <span data-ttu-id="4d0d0-177">注意： stack 溢出错误被视为特定于实现的限制。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-177">NOTE: the stack spilling errors are considered to be implementation-specific limitations.</span></span> <span data-ttu-id="4d0d0-178">因此，它们不会影响重载决策或 lambda 推理。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-178">Therefore they do not have effect on overload resolution or lambda inference.</span></span>

#### <a name="ordinary-byval-arguments-can-match-in-parameters"></a><span data-ttu-id="4d0d0-179">普通 byval 参数可以匹配 `in` 参数：</span><span class="sxs-lookup"><span data-stu-id="4d0d0-179">Ordinary byval arguments can match `in` parameters:</span></span>

<span data-ttu-id="4d0d0-180">不带修饰符的普通参数可以与 `in` 参数匹配。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-180">Regular arguments without modifiers can match `in` parameters.</span></span> <span data-ttu-id="4d0d0-181">在这种情况下，参数具有的宽松约束与普通 byval 参数相同。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-181">In such case the arguments have the same relaxed constraints as an ordinary byval arguments would have.</span></span>

<span data-ttu-id="4d0d0-182">此方案的动机是，如果不能将参数传递为直接引用（例如：文本、计算或 `await`的结果，或发生更具体的类型的参数），则 Api 中的 `in` 参数可能会导致不便用户。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-182">The motivation for this scenario is that `in` parameters in APIs may result in inconveniences for the user when arguments cannot be passed as a direct reference - ex: literals, computed or `await`-ed results or arguments that happen to have more specific types.</span></span>  
<span data-ttu-id="4d0d0-183">所有这些情况都有一种简单的解决方案，可将参数值存储在适当类型的临时本地并作为 `in` 参数传递该本地。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-183">All these cases have a trivial solution of storing the argument value in a temporary local of appropriate type and passing that local as an `in` argument.</span></span>  
<span data-ttu-id="4d0d0-184">若要减少此类样板代码的需求，可以在需要时执行相同的转换（如果需要） `in` 修饰符在调用站点上不存在。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-184">To reduce the need for such boilerplate code compiler can perform the same transformation, if needed, when `in` modifier is not present at the call site.</span></span>  

<span data-ttu-id="4d0d0-185">此外，在某些情况下（例如运算符或 `in` 扩展方法的调用），根本无法指定 `in`。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-185">In addition, in some cases, such as invocation of operators, or `in` extension methods, there is no syntactical way to specify `in` at all.</span></span> <span data-ttu-id="4d0d0-186">只需在匹配 `in` 参数时指定普通 byval 参数的行为。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-186">That alone requires specifying the behavior of ordinary byval arguments when they match `in` parameters.</span></span>

<span data-ttu-id="4d0d0-187">具体而言：</span><span class="sxs-lookup"><span data-stu-id="4d0d0-187">In particular:</span></span>

- <span data-ttu-id="4d0d0-188">传递右是有效的。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-188">it is valid to pass RValues.</span></span>
<span data-ttu-id="4d0d0-189">在这种情况下，将传递对临时的引用。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-189">A reference to a temporary is passed in such case.</span></span>
<span data-ttu-id="4d0d0-190">示例：</span><span class="sxs-lookup"><span data-stu-id="4d0d0-190">Example:</span></span>
```csharp
Print("hello");      // not an error.

void Print<T>(in T x)
{
  //. . .
}
```

- <span data-ttu-id="4d0d0-191">允许使用隐式转换。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-191">implicit conversions are allowed.</span></span>

> <span data-ttu-id="4d0d0-192">这实际上是传递 RValue 的一种特殊情况</span><span class="sxs-lookup"><span data-stu-id="4d0d0-192">This is actually a special case of passing an RValue</span></span>  

<span data-ttu-id="4d0d0-193">在这种情况下，将传递对临时保存转换值的引用。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-193">A reference to a temporary holding converted value is passed in such case.</span></span>
<span data-ttu-id="4d0d0-194">示例：</span><span class="sxs-lookup"><span data-stu-id="4d0d0-194">Example:</span></span>
```csharp
Print<int>(Short.MaxValue)     // not an error.
```

- <span data-ttu-id="4d0d0-195">如果 `in` 扩展方法的接收方（而不是 `ref` 扩展方法），则允许使用右或隐式的_参数转换_。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-195">in a case of a receiver of an `in` extension method (as opposed to `ref` extension methods), RValues or implicit _this-argument-conversions_ are allowed.</span></span>
<span data-ttu-id="4d0d0-196">在这种情况下，将传递对临时保存转换值的引用。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-196">A reference to a temporary holding converted value is passed in such case.</span></span>
<span data-ttu-id="4d0d0-197">示例：</span><span class="sxs-lookup"><span data-stu-id="4d0d0-197">Example:</span></span>
```csharp
public static IEnumerable<T> Concat<T>(in this (IEnumerable<T>, IEnumerable<T>) arg)  => . . .;

("aa", "bb").Concat<char>()    // not an error.
```
<span data-ttu-id="4d0d0-198">本文档中进一步介绍了 `ref`/`in` 扩展方法的详细信息。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-198">More information on `ref`/`in` extension methods is provided further in this document.</span></span>

- <span data-ttu-id="4d0d0-199">由于 `await` 操作数可能会在必要时溢出 "按值"，因此参数溢出。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-199">argument spilling due to `await` operands could spill "by-value", if necessary.</span></span>
<span data-ttu-id="4d0d0-200">在提供对参数的直接引用不可能的情况下，由于介入 `await` 将改为溢出参数值的副本。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-200">In scenarios where providing a direct reference to the argument is not possible due to intervening `await` a copy of the argument's value is spilled instead.</span></span>  
<span data-ttu-id="4d0d0-201">示例：</span><span class="sxs-lookup"><span data-stu-id="4d0d0-201">Example:</span></span>
```csharp
M1(RefReturningMethod(), await SomethingAsync())   // not an error.
```
<span data-ttu-id="4d0d0-202">由于有副作用的调用的结果是不能跨 `await` 挂起保留的引用，因此将保留包含实际值的临时值（与在普通 byval 参数情况下的情况相同）。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-202">Since the result of a side-effecting invocation is a reference that cannot be preserved across `await` suspension, a temporary containing the actual value will be preserved instead (as it would in an ordinary byval parameter case).</span></span>

#### <a name="omitted-optional-arguments"></a><span data-ttu-id="4d0d0-203">省略的可选参数</span><span class="sxs-lookup"><span data-stu-id="4d0d0-203">Omitted optional arguments</span></span>

<span data-ttu-id="4d0d0-204">允许 `in` 参数指定默认值。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-204">It is permitted for an `in` parameter to specify a default value.</span></span> <span data-ttu-id="4d0d0-205">这使相应参数成为可选参数。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-205">That makes the corresponding argument optional.</span></span>

<span data-ttu-id="4d0d0-206">在调用站点省略可选参数会导致通过临时传递默认值。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-206">Omitting optional argument at the call site results in passing the default value via a temporary.</span></span>

```csharp
Print("hello");      // not an error, same as
Print("hello", c: Color.Black);

void Print(string s, in Color c = Color.Black)
{
    // . . .
}
```

### <a name="aliasing-behavior-in-general"></a><span data-ttu-id="4d0d0-207">别名行为概述</span><span class="sxs-lookup"><span data-stu-id="4d0d0-207">Aliasing behavior in general</span></span>

<span data-ttu-id="4d0d0-208">与 `ref` 和 `out` 变量一样，`in` 变量是现有位置的引用/别名。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-208">Just like `ref` and `out` variables, `in` variables are references/aliases to existing locations.</span></span>

<span data-ttu-id="4d0d0-209">当被调用方不允许向其中写入时，读取 `in` 参数可能会将不同的值视为其他计算的副作用。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-209">While callee is not allowed to write into them, reading an `in` parameter can observe different values as a side effect of other evaluations.</span></span>

<span data-ttu-id="4d0d0-210">示例：</span><span class="sxs-lookup"><span data-stu-id="4d0d0-210">Example:</span></span>

```C#
static Vector3 v = Vector3.UnitY;

static void Main()
{
    Test(v);
}

static void Test(in Vector3 v1)
{
    Debug.Assert(v1 == Vector3.UnitY);
    // changes v1 deterministically (no races required)
    ChangeV();
    Debug.Assert(v1 == Vector3.UnitX);
}

static void ChangeV()
{
    v = Vector3.UnitX;
}
```

### <a name="in-parameters-and-capturing-of-local-variables"></a><span data-ttu-id="4d0d0-211">`in` 参数和捕获局部变量。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-211">`in` parameters and capturing of local variables.</span></span>  
<span data-ttu-id="4d0d0-212">出于 lambda/async 捕获的目的 `in` 参数的行为与 `out` 和 `ref` 参数相同。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-212">For the purpose of lambda/async capturing `in` parameters behave the same as `out` and `ref` parameters.</span></span>

- <span data-ttu-id="4d0d0-213">无法在闭包中捕获 `in` 参数</span><span class="sxs-lookup"><span data-stu-id="4d0d0-213">`in` parameters cannot be captured in a closure</span></span>
- <span data-ttu-id="4d0d0-214">迭代器方法中不允许使用 `in` 参数</span><span class="sxs-lookup"><span data-stu-id="4d0d0-214">`in` parameters are not allowed in iterator methods</span></span>
- <span data-ttu-id="4d0d0-215">异步方法中不允许使用 `in` 参数</span><span class="sxs-lookup"><span data-stu-id="4d0d0-215">`in` parameters are not allowed in async methods</span></span>

### <a name="temporary-variables"></a><span data-ttu-id="4d0d0-216">临时变量。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-216">Temporary variables.</span></span>  
<span data-ttu-id="4d0d0-217">某些使用 `in` 参数传递可能需要间接使用临时本地变量：</span><span class="sxs-lookup"><span data-stu-id="4d0d0-217">Some uses of `in` parameter passing may require indirect use of a temporary local variable:</span></span>  
- <span data-ttu-id="4d0d0-218">当调用站点使用 `in`时，`in` 参数将始终作为直接别名传递。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-218">`in` arguments are always passed as direct aliases when call-site uses `in`.</span></span> <span data-ttu-id="4d0d0-219">在这种情况下，永远不会使用暂时。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-219">Temporary is never used in such case.</span></span>
- <span data-ttu-id="4d0d0-220">如果调用站点不使用 `in`，则 `in` 参数不需要是直接别名。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-220">`in` arguments are not required to be direct aliases when call-site does not use `in`.</span></span> <span data-ttu-id="4d0d0-221">如果参数不是左值，则可以使用临时参数。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-221">When argument is not an LValue, a temporary may be used.</span></span>
- <span data-ttu-id="4d0d0-222">`in` 参数可能具有默认值。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-222">`in` parameter may have default value.</span></span> <span data-ttu-id="4d0d0-223">当在调用站点上省略相应的参数时，默认值将通过一个临时值传递。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-223">When corresponding argument is omitted at the call site, the default value are passed via a temporary.</span></span>
- <span data-ttu-id="4d0d0-224">`in` 参数可能具有隐式转换，包括不保留标识的参数。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-224">`in` arguments may have implicit conversions, including those that do not preserve identity.</span></span> <span data-ttu-id="4d0d0-225">在这种情况下，将使用一个临时。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-225">A temporary is used in those cases.</span></span>
- <span data-ttu-id="4d0d0-226">普通结构调用的接收方可能不是可写的左值（**现有事例！** ）。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-226">receivers of ordinary struct calls may not be writeable LValues (**existing case!**).</span></span> <span data-ttu-id="4d0d0-227">在这种情况下，将使用一个临时。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-227">A temporary is used in those cases.</span></span>

<span data-ttu-id="4d0d0-228">自变量临时内存的生存时间与调用站点最接近的范围。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-228">The life time of the argument temporaries matches the closest encompassing scope of the call-site.</span></span>

<span data-ttu-id="4d0d0-229">临时变量的正式生存时间对于涉及引用返回的变量的转义分析的方案，在语义上非常重要。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-229">The formal life time of temporary variables is semantically significant in scenarios involving escape analysis of variables returned by reference.</span></span>

### <a name="metadata-representation-of-in-parameters"></a><span data-ttu-id="4d0d0-230">`in` 参数的元数据表示形式。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-230">Metadata representation of `in` parameters.</span></span>
<span data-ttu-id="4d0d0-231">将 `System.Runtime.CompilerServices.IsReadOnlyAttribute` 应用于 byref 参数时，这意味着该参数是一个 `in` 参数。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-231">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to a byref parameter, it means that the parameter is an `in` parameter.</span></span>

<span data-ttu-id="4d0d0-232">此外，如果该方法为*抽象*方法或*虚*方法，则此类参数（并且只有这样的参数）的签名必须具有 `modreq[System.Runtime.InteropServices.InAttribute]`。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-232">In addition, if the method is *abstract* or *virtual*, then the signature of such parameters (and only such parameters) must have `modreq[System.Runtime.InteropServices.InAttribute]`.</span></span>

<span data-ttu-id="4d0d0-233">**动机**：这样做是为了确保在方法重写/实现 `in` 参数匹配的情况下。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-233">**Motivation**: this is done to ensure that in a case of method overriding/implementing the `in` parameters match.</span></span>

<span data-ttu-id="4d0d0-234">相同要求适用于委托中的 `Invoke` 方法。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-234">Same requirements apply to `Invoke` methods in delegates.</span></span>

<span data-ttu-id="4d0d0-235">**动机**：这是为了确保在创建或分配委托时现有编译器不能简单地忽略 `readonly`。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-235">**Motivation**: this is to ensure that existing compilers cannot simply ignore `readonly` when creating or assigning delegates.</span></span>

## <a name="returning-by-readonly-reference"></a><span data-ttu-id="4d0d0-236">由 readonly 引用返回。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-236">Returning by readonly reference.</span></span>

### <a name="motivation"></a><span data-ttu-id="4d0d0-237">动机</span><span class="sxs-lookup"><span data-stu-id="4d0d0-237">Motivation</span></span>
<span data-ttu-id="4d0d0-238">此子功能的动机大致对称到 `in` 参数的原因-避免复制，而是在返回方。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-238">The motivation for this sub-feature is roughly symmetrical to the reasons for the `in` parameters - avoiding copying, but on the returning side.</span></span> <span data-ttu-id="4d0d0-239">在此功能之前，方法或索引器有两个选项：1）返回引用并公开给可能的突变或2）返回的值会导致复制。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-239">Prior to this feature, a method or an indexer had two options: 1) return by reference and be exposed to possible mutations or 2) return by value which results in copying.</span></span>

### <a name="solution-ref-readonly-returns"></a><span data-ttu-id="4d0d0-240">解决方案（`ref readonly` 返回）</span><span class="sxs-lookup"><span data-stu-id="4d0d0-240">Solution (`ref readonly` returns)</span></span>  
<span data-ttu-id="4d0d0-241">此功能允许成员按引用返回变量，而不会将它们公开给突变。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-241">The feature allows a member to return variables by reference without exposing them to mutations.</span></span>

### <a name="declaring-ref-readonly-returning-members"></a><span data-ttu-id="4d0d0-242">声明 `ref readonly` 返回成员</span><span class="sxs-lookup"><span data-stu-id="4d0d0-242">Declaring `ref readonly` returning members</span></span>

<span data-ttu-id="4d0d0-243">返回签名 `ref readonly` 修饰符的组合用于指示成员返回 readonly 引用。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-243">A combination of modifiers `ref readonly` on the return signature is used to to indicate that the member returns a readonly reference.</span></span>

<span data-ttu-id="4d0d0-244">对于所有目的，将 `ref readonly` 成员视为 `readonly` 变量-类似于 `readonly` 字段和 `in` 参数。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-244">For all purposes a `ref readonly` member is treated as a `readonly` variable - similar to `readonly` fields and `in` parameters.</span></span>

<span data-ttu-id="4d0d0-245">例如，具有结构类型 `ref readonly` 成员的字段将以递归方式分类为 `readonly` 变量。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-245">For example fields of `ref readonly` member which has a struct type are all recursively classified as `readonly` variables.</span></span> <span data-ttu-id="4d0d0-246">-允许将它们作为 `in` 参数传递，但不能作为 `ref` 或 `out` 参数传递。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-246">- It is permitted to pass them as `in` arguments, but not as `ref` or `out` arguments.</span></span>

```csharp
ref readonly Guid Method1()
{
}

Method2(in Method1()); // valid. Can pass as `in` argument.

Method3(ref Method1()); // not valid. Cannot pass as `ref` argument
```

- <span data-ttu-id="4d0d0-247">同一位置允许 `ref readonly` 返回 `ref` 允许返回。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-247">`ref readonly` returns are allowed in the same places were `ref` returns are allowed.</span></span>
<span data-ttu-id="4d0d0-248">这包括索引器、委托、lambda、本地函数。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-248">This includes indexers, delegates, lambdas, local functions.</span></span>

- <span data-ttu-id="4d0d0-249">不允许 /`ref readonly`/差异 `ref`重载。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-249">It is not permitted to overload on `ref`/`ref readonly` /  differences.</span></span>

- <span data-ttu-id="4d0d0-250">允许重载普通 byval，并 `ref readonly` 返回差异。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-250">It is permitted to overload on ordinary byval and `ref readonly` return differences.</span></span>

- <span data-ttu-id="4d0d0-251">出于 OHI （重载、隐藏和实现）的目的，`ref readonly` 类似于 `ref`，但不同于。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-251">For the purpose of OHI (Overloading, Hiding, Implementing), `ref readonly` is similar but distinct from `ref`.</span></span>
<span data-ttu-id="4d0d0-252">例如，一种替代 `ref readonly` 方法，它本身必须 `ref readonly` 并且具有可转换的类型。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-252">For example the a method that overrides `ref readonly` one, must itself be `ref readonly` and have identity-convertible type.</span></span>

- <span data-ttu-id="4d0d0-253">出于委托/lambda/方法组转换的目的，`ref readonly` 类似，但不同于 `ref`。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-253">For the purpose of delegate/lambda/method group conversions, `ref readonly` is similar but distinct from `ref`.</span></span>
<span data-ttu-id="4d0d0-254">Lambda 和适用的方法组转换候选项必须匹配 `ref readonly` 返回目标委托，并且该类型的 `ref readonly` 返回为可转换的类型。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-254">Lambdas and applicable method group conversion candidates have to match `ref readonly` return of the target delegate with `ref readonly` return of the type that is identity-convertible.</span></span>

- <span data-ttu-id="4d0d0-255">出于泛型方差的目的，`ref readonly` 返回是 nonvariant 的。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-255">For the purpose of generic variance, `ref readonly` returns are nonvariant.</span></span>

> <span data-ttu-id="4d0d0-256">注意：对于具有引用类型或基元类型的 `ref readonly` 返回，没有警告。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-256">NOTE: There are no warnings on `ref readonly` returns that have reference or primitives types.</span></span>
<span data-ttu-id="4d0d0-257">通常，它可能是毫无意义的，但在某些情况下，用户必须/希望将基元作为 `in`传递。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-257">It may be pointless in general, but in some cases user must/want to pass primitives as `in`.</span></span> <span data-ttu-id="4d0d0-258">示例-在 `T` 替换为 `int`时，重写泛型方法，如 `ref readonly T Method()`。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-258">Examples - overriding a generic method like `ref readonly T Method()` when `T` was substituted to be `int`.</span></span>
>
><span data-ttu-id="4d0d0-259">可以让分析器在低效使用 `ref readonly` 返回的情况下发出警告，但这种分析的规则过于模糊，无法成为语言规范的一部分。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-259">It is conceivable to have an analyzer that warns in cases of inefficient use of `ref readonly` returns, but the rules for such analysis would be too fuzzy to be a part of a language specification.</span></span>

### <a name="returning-from-ref-readonly-members"></a><span data-ttu-id="4d0d0-260">从 `ref readonly` 成员返回</span><span class="sxs-lookup"><span data-stu-id="4d0d0-260">Returning from `ref readonly` members</span></span>
<span data-ttu-id="4d0d0-261">在方法体中，语法与常规引用返回相同。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-261">Inside the method body the syntax is the same as with regular ref returns.</span></span> <span data-ttu-id="4d0d0-262">将从包含方法推断 `readonly`。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-262">The `readonly` will be inferred from the containing method.</span></span>

<span data-ttu-id="4d0d0-263">动机是 `return ref readonly <expression>` 不必要长时间，只允许 `readonly` 部件上出现不匹配的情况，这将始终导致错误。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-263">The motivation is that `return ref readonly <expression>` is unnecessary long and only allows for mismatches on the `readonly` part that would always result in errors.</span></span>
<span data-ttu-id="4d0d0-264">但是，与通过严格的别名与按值传递的其他方案相比，`ref` 是一致的。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-264">The `ref` is, however, required for consistency with other scenarios where something is passed via strict aliasing vs. by value.</span></span>

> <span data-ttu-id="4d0d0-265">与 `in` 参数的情况不同，`ref readonly` 返回从不通过本地副本返回的。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-265">Unlike the case with `in` parameters, `ref readonly` returns never return via a local copy.</span></span> <span data-ttu-id="4d0d0-266">考虑到，在返回此类做法后，复制会立即停止存在是毫无意义的。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-266">Considering that the copy would cease to exist immediately upon returning such practice would be pointless and dangerous.</span></span> <span data-ttu-id="4d0d0-267">因此 `ref readonly` 返回始终是直接引用。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-267">Therefore `ref readonly` returns are always direct references.</span></span>

<span data-ttu-id="4d0d0-268">示例：</span><span class="sxs-lookup"><span data-stu-id="4d0d0-268">Example:</span></span>

```csharp
struct ImmutableArray<T>
{
    private readonly T[] array;

    public ref readonly T ItemRef(int i)
    {
        // returning a readonly reference to an array element
        return ref this.array[i];
    }
}

```

- <span data-ttu-id="4d0d0-269">`return ref` 的参数必须是左值（**现有规则**）</span><span class="sxs-lookup"><span data-stu-id="4d0d0-269">An argument of `return ref` must be an LValue (**existing rule**)</span></span>
- <span data-ttu-id="4d0d0-270">`return ref` 的参数必须是 "可安全返回" （**现有规则**）</span><span class="sxs-lookup"><span data-stu-id="4d0d0-270">An argument of `return ref` must be "safe to return" (**existing rule**)</span></span>
- <span data-ttu-id="4d0d0-271">在 `ref readonly` 成员中，`return ref` 的参数_不需要是可写_的。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-271">In a `ref readonly` member an argument of `return ref` is _not required to be writeable_ .</span></span>
<span data-ttu-id="4d0d0-272">例如，此类成员可以引用 readonly 字段或其 `in` 参数之一。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-272">For example such member can ref-return a readonly field or one of its `in` parameters.</span></span>

### <a name="safe-to-return-rules"></a><span data-ttu-id="4d0d0-273">返回规则的安全。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-273">Safe to Return rules.</span></span>
<span data-ttu-id="4d0d0-274">对于引用的正常返回规则也适用于 readonly 引用。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-274">Normal safe to return rules for references will apply to readonly references as well.</span></span>

<span data-ttu-id="4d0d0-275">请注意，`ref readonly` 可以从常规的 `ref` 本地/参数/返回，但不能通过其他方法获取。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-275">Note that a `ref readonly` can be obtained from a regular `ref` local/parameter/return, but not the other way around.</span></span> <span data-ttu-id="4d0d0-276">否则，`ref readonly` 返回的安全性的推断方式与常规 `ref` 返回的安全性相同。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-276">Otherwise the safety of `ref readonly` returns is inferred the same way as for regular `ref` returns.</span></span>

<span data-ttu-id="4d0d0-277">考虑到右可以作为 `in` 参数传递并返回，`ref readonly` 我们需要另一个规则-**右不安全地按引用返回**。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-277">Considering that RValues can be passed as `in` parameter and returned as `ref readonly` we need one more rule - **RValues are not safe-to-return by reference**.</span></span>

> <span data-ttu-id="4d0d0-278">当右值通过复制传递到 `in` 参数，然后以 `ref readonly`形式返回时，请考虑这种情况。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-278">Consider the situation when an RValue is passed to an `in` parameter via a copy and then returned back in a form of a `ref readonly`.</span></span> <span data-ttu-id="4d0d0-279">在调用方的上下文中，此类调用的结果是对本地数据的引用，因此不安全返回。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-279">In the context of the caller the result of such invocation is a reference to local data and as such is unsafe to return.</span></span>
> <span data-ttu-id="4d0d0-280">右不安全返回后，现有规则 `#6` 已经处理了这种情况。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-280">Once RValues are not safe to return, the existing rule `#6` already handles this case.</span></span>

<span data-ttu-id="4d0d0-281">示例：</span><span class="sxs-lookup"><span data-stu-id="4d0d0-281">Example:</span></span>
```csharp
ref readonly Vector3 Test1()
{
    // can pass an RValue as "in" (via a temp copy)
    // but the result is not safe to return
    // because the RValue argument was not safe to return by reference
    return ref Test2(default(Vector3));
}

ref readonly Vector3 Test2(in Vector3 r)
{
    // this is ok, r is returnable
    return ref r;
}
```

<span data-ttu-id="4d0d0-282">更新 `safe to return` 规则：</span><span class="sxs-lookup"><span data-stu-id="4d0d0-282">Updated `safe to return` rules:</span></span>

1.  <span data-ttu-id="4d0d0-283">**可以安全地返回堆中的变量引用**</span><span class="sxs-lookup"><span data-stu-id="4d0d0-283">**refs to variables on the heap are safe to return**</span></span>
2.  <span data-ttu-id="4d0d0-284">可以**安全地返回
`in` 参数的 ref/in 参数**，该参数只能作为 readonly 返回。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-284">**ref/in parameters are safe to return**
`in` parameters naturally can only be returned as readonly.</span></span>
3.  <span data-ttu-id="4d0d0-285">**out 参数可以安全返回**（但必须明确赋值，因为现在已经是这样）</span><span class="sxs-lookup"><span data-stu-id="4d0d0-285">**out parameters are safe to return** (but must be definitely assigned, as is already the case today)</span></span>
4.  <span data-ttu-id="4d0d0-286">**只要接收方可以安全返回，就可以安全地返回实例结构字段**</span><span class="sxs-lookup"><span data-stu-id="4d0d0-286">**instance struct fields are safe to return as long as the receiver is safe to return**</span></span>
5.  <span data-ttu-id="4d0d0-287">**"this" 不能安全地从结构成员返回**</span><span class="sxs-lookup"><span data-stu-id="4d0d0-287">**'this' is not safe to return from struct members**</span></span>
6.  <span data-ttu-id="4d0d0-288">**如果作为形参传递给该方法的所有 refs/out 都可以安全返回，则从另一种方法返回的 ref 将是安全的。** *具体而言
，无论接收方是结构、类还是类型为泛型类型参数，都是不相关的。*</span><span class="sxs-lookup"><span data-stu-id="4d0d0-288">**a ref, returned from another method is safe to return if all refs/outs passed to that method as formal parameters were safe to return.**
*Specifically it is irrelevant if receiver is safe to return, regardless whether receiver is a struct, class or typed as a generic type parameter.*</span></span>
7.  <span data-ttu-id="4d0d0-289">**右无法安全地按引用返回。** 
*具体说来，作为 in 参数传递是安全的右。*</span><span class="sxs-lookup"><span data-stu-id="4d0d0-289">**RValues are not safe to return by reference.**
*Specifically RValues are safe to pass as in parameters.*</span></span>

> <span data-ttu-id="4d0d0-290">注意：当涉及引用类型和引用操作时，有关于安全的返回的其他规则。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-290">NOTE: There are additional rules regarding safety of returns that come into play when ref-like types and ref-reassignments are involved.</span></span>
> <span data-ttu-id="4d0d0-291">这些规则同样适用于 `ref` 和 `ref readonly` 成员，因此不在此处提及。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-291">The rules equally apply to `ref` and `ref readonly` members and therefore are not mentioned here.</span></span>

### <a name="aliasing-behavior"></a><span data-ttu-id="4d0d0-292">别名行为。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-292">Aliasing behavior.</span></span>
<span data-ttu-id="4d0d0-293">`ref readonly` 成员提供与普通 `ref` 成员相同的别名行为（只读）。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-293">`ref readonly` members provide the same aliasing behavior as ordinary `ref` members (except for being readonly).</span></span>
<span data-ttu-id="4d0d0-294">因此，为了捕获 lambda、async、迭代器、stack 溢出等 .。。同样的限制也适用。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-294">Therefore for the purpose of capturing in lambdas, async, iterators, stack spilling etc... the same restrictions apply.</span></span> <span data-ttu-id="4d0d0-295">亦.</span><span class="sxs-lookup"><span data-stu-id="4d0d0-295">- I.E.</span></span> <span data-ttu-id="4d0d0-296">由于无法捕获实际引用和成员计算的副作用性质，因此不允许这样做。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-296">due to inability to capture the actual references and due to side-effecting nature of member evaluation such scenarios are disallowed.</span></span>

> <span data-ttu-id="4d0d0-297">当 `ref readonly` 返回是正则结构方法的接收方时，允许和需要进行复制，这会将 `this` 视为普通的可写引用。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-297">It is permitted and required to make a copy when `ref readonly` return is a receiver of regular struct methods, which take `this` as an ordinary writeable reference.</span></span> <span data-ttu-id="4d0d0-298">在这种情况下，在所有情况下，都会对只读变量应用本地副本。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-298">Historically in all cases where such invocations are applied to readonly variable a local copy is made.</span></span>

### <a name="metadata-representation"></a><span data-ttu-id="4d0d0-299">元数据表示形式。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-299">Metadata representation.</span></span>
<span data-ttu-id="4d0d0-300">当 `System.Runtime.CompilerServices.IsReadOnlyAttribute` 应用于返回 byref 返回方法的时，这意味着该方法返回 readonly 引用。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-300">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to the return of a byref returning method, it means that the method returns a readonly reference.</span></span>

<span data-ttu-id="4d0d0-301">此外，此类方法的结果签名（并且只有那些方法）必须有 `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-301">In addition, the result signature of such methods (and only those methods) must have `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`.</span></span>

<span data-ttu-id="4d0d0-302">**动机**：这是为了确保在调用方法时现有编译器不能简单地忽略 `readonly` `ref readonly` 返回</span><span class="sxs-lookup"><span data-stu-id="4d0d0-302">**Motivation**: this is to ensure that existing compilers cannot simply ignore `readonly` when invoking methods with `ref readonly` returns</span></span>

## <a name="readonly-structs"></a><span data-ttu-id="4d0d0-303">Readonly 结构</span><span class="sxs-lookup"><span data-stu-id="4d0d0-303">Readonly structs</span></span>
<span data-ttu-id="4d0d0-304">简短的一项功能，它使结构的所有实例成员（构造函数除外） `this` 参数，而 `in` 参数。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-304">In short - a feature that makes `this` parameter of all instance members of a struct, except for constructors, an `in` parameter.</span></span>

### <a name="motivation"></a><span data-ttu-id="4d0d0-305">动机</span><span class="sxs-lookup"><span data-stu-id="4d0d0-305">Motivation</span></span>
<span data-ttu-id="4d0d0-306">编译器必须假定对结构实例的任何方法调用都可以修改该实例。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-306">Compiler must assume that any method call on a struct instance may modify the instance.</span></span> <span data-ttu-id="4d0d0-307">确实会将可写引用作为 `this` 参数传递给方法，并完全启用此行为。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-307">Indeed a writeable reference is passed to the method as `this` parameter and fully enables this behavior.</span></span> <span data-ttu-id="4d0d0-308">若要允许在 `readonly` 变量上进行此类调用，请将调用应用到临时副本。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-308">To allow such invocations on `readonly` variables, the invocations are applied to temp copies.</span></span> <span data-ttu-id="4d0d0-309">这可能是 unintuitive 的，有时出于性能方面的原因，会强制用户放弃 `readonly`。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-309">That could be unintuitive and sometimes forces people to abandon `readonly` for performance reasons.</span></span>  
<span data-ttu-id="4d0d0-310">示例： https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/</span><span class="sxs-lookup"><span data-stu-id="4d0d0-310">Example: https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/</span></span>

<span data-ttu-id="4d0d0-311">添加对 `in` 参数的支持后，`ref readonly` 返回防御性复制的问题会变得更糟，因为 readonly 变量将变得更常见。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-311">After adding support for `in` parameters and `ref readonly` returns the problem of defensive copying will get worse since readonly variables will become more common.</span></span>

### <a name="solution"></a><span data-ttu-id="4d0d0-312">解决方案</span><span class="sxs-lookup"><span data-stu-id="4d0d0-312">Solution</span></span>
<span data-ttu-id="4d0d0-313">允许对结构声明执行 `readonly` 修饰符，这将导致 `this` 被视为除构造函数之外的所有结构实例方法上的 `in` 参数。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-313">Allow `readonly` modifier on struct declarations which would result in `this` being treated as `in` parameter on all struct instance methods except for constructors.</span></span>

```csharp
static void Test(in Vector3 v1)
{
    // no need to make a copy of v1 since Vector3 is a readonly struct
    System.Console.WriteLine(v1.ToString());
}

readonly struct Vector3
{
    . . .

    public override string ToString()
    {
        // not OK!!  `this` is an `in` parameter
        foo(ref this.X);

        // OK
        return $"X: {X}, Y: {Y}, Z: {Z}";
    }
}
```

### <a name="restrictions-on-members-of-readonly-struct"></a><span data-ttu-id="4d0d0-314">对 readonly 结构成员的限制</span><span class="sxs-lookup"><span data-stu-id="4d0d0-314">Restrictions on members of readonly struct</span></span>
- <span data-ttu-id="4d0d0-315">只读结构的实例字段必须是只读的。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-315">Instance fields of a readonly struct must be readonly.</span></span>  
<span data-ttu-id="4d0d0-316">**动机：** 只能从外部写入，而不能通过成员写入。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-316">**Motivation:** can only be written to externally, but not through members.</span></span>
- <span data-ttu-id="4d0d0-317">只读结构的实例 autoproperties 必须是只是获取的。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-317">Instance autoproperties of a readonly struct must be get-only.</span></span>  
<span data-ttu-id="4d0d0-318">**动机：** 对实例字段的限制的影响。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-318">**Motivation:** consequence of restriction on instance fields.</span></span>
- <span data-ttu-id="4d0d0-319">Readonly 结构不能声明类似于字段的事件。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-319">Readonly struct may not declare field-like events.</span></span>  
<span data-ttu-id="4d0d0-320">**动机：** 对实例字段的限制的影响。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-320">**Motivation:** consequence of restriction on instance fields.</span></span>

### <a name="metadata-representation"></a><span data-ttu-id="4d0d0-321">元数据表示形式。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-321">Metadata representation.</span></span>
<span data-ttu-id="4d0d0-322">将 `System.Runtime.CompilerServices.IsReadOnlyAttribute` 应用于值类型时，这意味着该类型为 `readonly struct`。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-322">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to a value type, it means that the type is a `readonly struct`.</span></span>

<span data-ttu-id="4d0d0-323">具体而言：</span><span class="sxs-lookup"><span data-stu-id="4d0d0-323">In particular:</span></span>
-  <span data-ttu-id="4d0d0-324">`IsReadOnlyAttribute` 类型的标识不重要。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-324">The identity of the `IsReadOnlyAttribute` type is unimportant.</span></span> <span data-ttu-id="4d0d0-325">事实上，如果需要，它可以在包含程序集中由编译器嵌入。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-325">In fact it can be embedded by the compiler in the containing assembly if needed.</span></span>

## <a name="refin-extension-methods"></a><span data-ttu-id="4d0d0-326">`ref`/`in` 扩展方法</span><span class="sxs-lookup"><span data-stu-id="4d0d0-326">`ref`/`in` extension methods</span></span>
<span data-ttu-id="4d0d0-327">实际上现有的提议（ https://github.com/dotnet/roslyn/issues/165) 和相应的原型 PR （ https://github.com/dotnet/roslyn/pull/15650)）。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-327">There is actually an existing proposal (https://github.com/dotnet/roslyn/issues/165) and corresponding prototype PR (https://github.com/dotnet/roslyn/pull/15650).</span></span>
<span data-ttu-id="4d0d0-328">我只想确认这种想法并不完全新。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-328">I just want to acknowledge that this idea is not entirely new.</span></span> <span data-ttu-id="4d0d0-329">但在这里，这一点与此相关，因为 `ref readonly` 完美地删除了有关此类方法的最争用资源问题-如何处理右值接收器。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-329">It is, however, relevant here since `ref readonly` elegantly removes the most contentious issue about such methods - what to do with RValue receivers.</span></span>

<span data-ttu-id="4d0d0-330">一般思路允许扩展方法按引用获取 `this` 参数，前提是该类型已知为结构类型。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-330">The general idea is allowing extension methods to take the `this` parameter by reference, as long as the type is known to be a struct type.</span></span>

```csharp
public static void Extension(ref this Guid self)
{
    // do something
}
```

<span data-ttu-id="4d0d0-331">编写这种扩展方法的原因主要是：</span><span class="sxs-lookup"><span data-stu-id="4d0d0-331">The reasons for writing such extension methods are primarily:</span></span>  
1.  <span data-ttu-id="4d0d0-332">接收方是大型结构时避免复制</span><span class="sxs-lookup"><span data-stu-id="4d0d0-332">Avoid copying when receiver is a large struct</span></span>
2.  <span data-ttu-id="4d0d0-333">允许在结构上改变扩展方法</span><span class="sxs-lookup"><span data-stu-id="4d0d0-333">Allow mutating extension methods on structs</span></span>

<span data-ttu-id="4d0d0-334">我们不想在类上允许此操作的原因</span><span class="sxs-lookup"><span data-stu-id="4d0d0-334">The reasons why we do not want to allow this on classes</span></span>  
1.  <span data-ttu-id="4d0d0-335">它的用途非常有限。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-335">It would be of very limited purpose.</span></span>
2.  <span data-ttu-id="4d0d0-336">这会中断长时间的固定，方法调用无法将非`null` 接收方转换为在调用后变为 `null`。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-336">It would break long standing invariant that a method call cannot turn non-`null` receiver to become `null` after invocation.</span></span>
> <span data-ttu-id="4d0d0-337">事实上，除非 `ref` 或 `out`_显式_赋值或传递，否则当前无法 `null` 非`null` 变量。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-337">In fact, currently a non-`null` variable cannot become `null` unless _explicitly_ assigned or passed by `ref` or `out`.</span></span>
> <span data-ttu-id="4d0d0-338">这可以极大帮助提高可读性或其他形式的 "这在此处为空"。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-338">That greatly aids readability or other forms of "can this be a null here" analysis.</span></span>
3.  <span data-ttu-id="4d0d0-339">这种情况很难与 "计算一次" 对 null 条件访问语义进行协调。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-339">It would be hard to reconcile with "evaluate once" semantics of null-conditional accesses.</span></span>
<span data-ttu-id="4d0d0-340">示例： `obj.stringField?.RefExtension(...)`-需要捕获 `stringField` 的副本以使 null 检查有意义，但随后不会将对 RefExtension 内 `this` 的赋值反映回字段。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-340">Example: `obj.stringField?.RefExtension(...)` - need to capture a copy of `stringField` to make the null check meaningful, but then assignments to `this` inside RefExtension would not be reflected back to the field.</span></span>

<span data-ttu-id="4d0d0-341">在采用第一个参数的**结构**上声明扩展方法的能力是长时间的请求。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-341">An ability to declare extension methods on **structs** that take the first argument by reference was a long-standing request.</span></span> <span data-ttu-id="4d0d0-342">其中一个阻止性注意事项是 "如果接收方不是左值，会发生什么情况？"。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-342">One of the blocking consideration was "what happens if receiver is not an LValue?".</span></span>

- <span data-ttu-id="4d0d0-343">有一个引用者，还可以将任何扩展方法作为静态方法调用（有时这是解析多义性的唯一方法）。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-343">There is a precedent that any extension method could also be called as a static method (sometimes it is the only way to resolve ambiguity).</span></span> <span data-ttu-id="4d0d0-344">它会规定应禁止 RValue 接收方。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-344">It would dictate that RValue receivers should be disallowed.</span></span>
- <span data-ttu-id="4d0d0-345">另一方面，在涉及结构实例方法时，可以在类似情况下对副本进行调用。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-345">On the other hand there is a practice of making invocation on a copy in similar situations when struct instance methods are involved.</span></span>

<span data-ttu-id="4d0d0-346">"隐式复制" 之所以存在的原因是因为大多数结构方法不会实际修改结构，而不能指示。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-346">The reason why the "implicit copying" exists is because the majority of struct methods do not actually modify the struct while not being able to indicate that.</span></span> <span data-ttu-id="4d0d0-347">因此，最可行的解决方案是只对副本进行调用，但这种做法已知会对性能造成损害并引发 bug。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-347">Therefore the most practical solution was to just make the invocation on a copy, but this practice is known for harming performance and causing bugs.</span></span>

<span data-ttu-id="4d0d0-348">现在，在 `in` 参数的可用性的情况下，扩展可以指示意向。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-348">Now, with availability of `in` parameters, it is possible for an extension to signal the intent.</span></span> <span data-ttu-id="4d0d0-349">因此，可以通过使用可写接收方调用 `ref` 扩展来解决难题，并在必要时 `in` 扩展允许隐式复制。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-349">Therefore the conundrum can be resolved by requiring `ref` extensions to be called with writeable receivers while `in` extensions permit implicit copying if necessary.</span></span>

```csharp
// this can be called on either RValue or an LValue
public static void Reader(in this Guid self)
{
    // do something nonmutating.
    WriteLine(self == default(Guid));
}

// this can be called only on an LValue
public static void Mutator(ref this Guid self)
{
    // can mutate self
    self = new Guid();
}
```

### <a name="in-extensions-and-generics"></a><span data-ttu-id="4d0d0-350">`in` 扩展和泛型。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-350">`in` extensions and generics.</span></span>
<span data-ttu-id="4d0d0-351">`ref` 扩展方法的目的是直接或通过调用变异成员来改变接收方。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-351">The purpose of `ref` extension methods is to mutate the receiver directly or by invoking mutating members.</span></span> <span data-ttu-id="4d0d0-352">因此，只要 `T` 被约束为结构，就允许 `ref this T` 扩展。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-352">Therefore `ref this T` extensions are allowed as long as `T` is constrained to be a struct.</span></span>

<span data-ttu-id="4d0d0-353">另一方面 `in` 扩展方法的存在是为了减少隐式复制。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-353">On the other hand `in` extension methods exist specifically to reduce implicit copying.</span></span> <span data-ttu-id="4d0d0-354">但 `in T` 参数的任何使用都必须通过接口成员来完成。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-354">However any use of an `in T` parameter will have to be done through an interface member.</span></span> <span data-ttu-id="4d0d0-355">由于所有接口成员都被视为改变，因此任何此类使用都需要复制。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-355">Since all interface members are considered mutating, any such use would require a copy.</span></span> <span data-ttu-id="4d0d0-356">-不是减少复制，而是相反。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-356">- Instead of reducing copying, the effect would be the opposite.</span></span> <span data-ttu-id="4d0d0-357">因此，当 `T` 是泛型类型参数，而不考虑约束时，不允许 `in this T`。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-357">Therefore `in this T` is not allowed when `T` is a generic type parameter regardless of constraints.</span></span>

### <a name="valid-kinds-of-extension-methods-recap"></a><span data-ttu-id="4d0d0-358">有效的扩展方法类型（概述）：</span><span class="sxs-lookup"><span data-stu-id="4d0d0-358">Valid kinds of extension methods (recap):</span></span>
<span data-ttu-id="4d0d0-359">现在允许在扩展方法中使用以下形式的 `this` 声明：</span><span class="sxs-lookup"><span data-stu-id="4d0d0-359">The following forms of `this` declaration in an extension method are now allowed:</span></span>
1) <span data-ttu-id="4d0d0-360">`this T arg` 规则 byval 扩展。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-360">`this T arg` - regular byval extension.</span></span> <span data-ttu-id="4d0d0-361">（**现有事例**）</span><span class="sxs-lookup"><span data-stu-id="4d0d0-361">(**existing case**)</span></span>
- <span data-ttu-id="4d0d0-362">T 可以是任何类型，包括引用类型或类型参数。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-362">T can be any type, including reference types or type parameters.</span></span>
<span data-ttu-id="4d0d0-363">实例在调用后将是相同的变量。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-363">Instance will be the same variable after the call.</span></span>
<span data-ttu-id="4d0d0-364">允许对_此参数转换_类型进行隐式转换。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-364">Allows implicit conversions of _this-argument-conversion_ kind.</span></span>
<span data-ttu-id="4d0d0-365">可在右上调用。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-365">Can be called on RValues.</span></span>

- <span data-ttu-id="4d0d0-366">`in this T self` - `in` 扩展。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-366">`in this T self` - `in` extension.</span></span>
<span data-ttu-id="4d0d0-367">T 必须是实际的结构类型。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-367">T must be an actual struct type.</span></span>
<span data-ttu-id="4d0d0-368">实例在调用后将是相同的变量。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-368">Instance will be the same variable after the call.</span></span>
<span data-ttu-id="4d0d0-369">允许对_此参数转换_类型进行隐式转换。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-369">Allows implicit conversions of _this-argument-conversion_ kind.</span></span>
<span data-ttu-id="4d0d0-370">可在右上调用（如果需要，可以在 temp 上调用）。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-370">Can be called on RValues (may be invoked on a temp if needed).</span></span>

- <span data-ttu-id="4d0d0-371">`ref this T self` - `ref` 扩展。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-371">`ref this T self` - `ref` extension.</span></span>
<span data-ttu-id="4d0d0-372">T 必须是结构类型或约束为结构的泛型类型参数。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-372">T must be a struct type or a generic type parameter constrained to be a struct.</span></span>
<span data-ttu-id="4d0d0-373">实例可以通过调用来写入。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-373">Instance may be written to by the invocation.</span></span>
<span data-ttu-id="4d0d0-374">仅允许标识转换。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-374">Allows only identity conversions.</span></span>
<span data-ttu-id="4d0d0-375">必须在可写的左值上调用。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-375">Must be called on writeable LValue.</span></span> <span data-ttu-id="4d0d0-376">（从不通过 temp 调用）。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-376">(never invoked via a temp).</span></span>

## <a name="readonly-ref-locals"></a><span data-ttu-id="4d0d0-377">Readonly 引用局部变量。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-377">Readonly ref locals.</span></span>

### <a name="motivation"></a><span data-ttu-id="4d0d0-378">推动.</span><span class="sxs-lookup"><span data-stu-id="4d0d0-378">Motivation.</span></span>
<span data-ttu-id="4d0d0-379">一旦引入 `ref readonly` 成员后，就清楚地需要将它们与适当的本地类型成对使用。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-379">Once `ref readonly` members were introduced, it was clear from the use that they need to be paired with appropriate kind of local.</span></span> <span data-ttu-id="4d0d0-380">成员的计算可能会产生或观察到副作用，因此，如果必须多次使用该结果，则需要对其进行存储。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-380">Evaluation of a member may produce or observe side effects, therefore if the result must be used more than once, it needs to be stored.</span></span> <span data-ttu-id="4d0d0-381">普通 `ref` 局部变量在此处不起作用，因为无法为它们分配 `readonly` 引用。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-381">Ordinary `ref` locals do not help here since they cannot be assigned a `readonly` reference.</span></span>   

### <a name="solution"></a><span data-ttu-id="4d0d0-382">解决方案.</span><span class="sxs-lookup"><span data-stu-id="4d0d0-382">Solution.</span></span>
<span data-ttu-id="4d0d0-383">允许声明 `ref readonly` 局部变量。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-383">Allow declaring `ref readonly` locals.</span></span> <span data-ttu-id="4d0d0-384">这是一种新的 `ref` 不能写入的局部变量。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-384">This is a new kind of `ref` locals that is not writeable.</span></span> <span data-ttu-id="4d0d0-385">因此 `ref readonly` 局部变量可以接受对只读变量的引用，而不会公开这些变量来写入。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-385">As a result `ref readonly` locals can accept references to readonly variables without exposing these variables to writes.</span></span>

### <a name="declaring-and-using-ref-readonly-locals"></a><span data-ttu-id="4d0d0-386">声明和使用局部变量 `ref readonly`。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-386">Declaring and using `ref readonly` locals.</span></span>

<span data-ttu-id="4d0d0-387">此类局部变量的语法使用声明站点（以该特定顺序） `ref readonly` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-387">The syntax of such locals uses `ref readonly` modifiers at declaration site (in that specific order).</span></span> <span data-ttu-id="4d0d0-388">类似于普通 `ref` 局部变量，`ref readonly` 局部变量必须在声明中进行引用初始化。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-388">Similarly to ordinary `ref` locals, `ref readonly` locals must be ref-initialized at declaration.</span></span> <span data-ttu-id="4d0d0-389">与常规 `ref` 局部变量不同，`ref readonly` 局部变量可以引用 `readonly` 左值，如 `in` 参数、`readonly` 字段 `ref readonly` 方法。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-389">Unlike regular `ref` locals, `ref readonly` locals can refer to `readonly` LValues like `in` parameters, `readonly` fields, `ref readonly` methods.</span></span>

<span data-ttu-id="4d0d0-390">对于所有目的，将 `ref readonly` local 视为 `readonly` 变量。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-390">For all purposes a `ref readonly` local is treated as a `readonly` variable.</span></span> <span data-ttu-id="4d0d0-391">对使用的大多数限制都与 `readonly` 字段或 `in` 参数相同。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-391">Most of the restrictions on the use are the same as with `readonly` fields or `in` parameters.</span></span>

<span data-ttu-id="4d0d0-392">对于包含结构类型 `in` 参数的示例字段，它们都以递归方式分类为 `readonly` 变量。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-392">For example fields of an `in` parameter which has a struct type are all recursively classified as `readonly` variables .</span></span>   

```csharp
static readonly ref Vector3 M1() => . . .

static readonly ref Vector3 M1_Trace()
{
    // OK
    ref readonly var r1 = ref M1();

    // Not valid. Need an LValue
    ref readonly Vector3 r2 = ref default(Vector3);

    // Not valid. r1 is readonly.
    Mutate(ref r1);

    // OK.
    Print(in r1);

    // OK.
    return ref r1;
}
```

### <a name="restrictions-on-use-of-ref-readonly-locals"></a><span data-ttu-id="4d0d0-393">`ref readonly` 局部变量的使用限制</span><span class="sxs-lookup"><span data-stu-id="4d0d0-393">Restrictions on use of `ref readonly` locals</span></span>
<span data-ttu-id="4d0d0-394">除了其 `readonly` 性质外，`ref readonly` 局部变量的行为类似于普通 `ref` 局部变量，并且受到完全相同的限制。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-394">Except for their `readonly` nature, `ref readonly` locals behave like ordinary `ref` locals and are subject to exactly same restrictions.</span></span>  
<span data-ttu-id="4d0d0-395">有关与在闭包中捕获相关的限制，`async` 方法或 `safe-to-return` 分析中的声明同样适用于 `ref readonly` 局部变量。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-395">For example restrictions related to capturing in closures, declaring in `async` methods or the `safe-to-return` analysis equally applies to `ref readonly` locals.</span></span>

## <a name="ternary-ref-expressions-aka-conditional-lvalues"></a><span data-ttu-id="4d0d0-396">三元 `ref` 表达式。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-396">Ternary `ref` expressions.</span></span> <span data-ttu-id="4d0d0-397">（也称为 "条件左值"）</span><span class="sxs-lookup"><span data-stu-id="4d0d0-397">(aka "Conditional LValues")</span></span>

### <a name="motivation"></a><span data-ttu-id="4d0d0-398">动机</span><span class="sxs-lookup"><span data-stu-id="4d0d0-398">Motivation</span></span>
<span data-ttu-id="4d0d0-399">使用 `ref` 和 `ref readonly` 局部变量时，需要根据条件将此类局部变量替换为一个或另一个目标变量。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-399">Use of `ref` and `ref readonly` locals exposed a need to ref-initialize such locals with one or another target variable based on a condition.</span></span>

<span data-ttu-id="4d0d0-400">典型的解决方法是引入如下方法：</span><span class="sxs-lookup"><span data-stu-id="4d0d0-400">A typical workaround is to introduce a method like:</span></span>

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

<span data-ttu-id="4d0d0-401">请注意，`Choice` 不是三元的完全替换，因为必须在调用站点中计算_所有_参数，这将导致 unintuitive 行为和 bug。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-401">Note that `Choice` is not an exact replacement of a ternary since _all_ arguments must be evaluated at the call site, which was leading to unintuitive behavior and bugs.</span></span>

<span data-ttu-id="4d0d0-402">以下操作不会按预期方式工作：</span><span class="sxs-lookup"><span data-stu-id="4d0d0-402">The following will not work as expected:</span></span>

```csharp
    // will crash with NRE because 'arr[0]' will be executed unconditionally
    ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

### <a name="solution"></a><span data-ttu-id="4d0d0-403">解决方案</span><span class="sxs-lookup"><span data-stu-id="4d0d0-403">Solution</span></span>
<span data-ttu-id="4d0d0-404">允许特殊类型的条件表达式，该表达式的计算结果为基于条件的左值参数之一的引用。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-404">Allow special kind of conditional expression that evaluates to a reference to one of LValue argument based on a condition.</span></span>

### <a name="using-ref-ternary-expression"></a><span data-ttu-id="4d0d0-405">使用 `ref` 三元表达式。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-405">Using `ref` ternary expression.</span></span>

<span data-ttu-id="4d0d0-406">条件表达式的 `ref` 风格的语法 `<condition> ? ref <consequence> : ref <alternative>;`</span><span class="sxs-lookup"><span data-stu-id="4d0d0-406">The syntax for the `ref` flavor of a conditional expression is `<condition> ? ref <consequence> : ref <alternative>;`</span></span>

<span data-ttu-id="4d0d0-407">就像普通的条件表达式一样，只根据布尔条件表达式的结果计算 `<consequence>` 或 `<alternative>`。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-407">Just like with the ordinary conditional expression only `<consequence>` or `<alternative>` is evaluated depending on result of the boolean condition expression.</span></span>

<span data-ttu-id="4d0d0-408">与普通的条件表达式不同，`ref` 条件表达式：</span><span class="sxs-lookup"><span data-stu-id="4d0d0-408">Unlike ordinary conditional expression, `ref` conditional expression:</span></span>
- <span data-ttu-id="4d0d0-409">要求 `<consequence>` 和 `<alternative>` 左值。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-409">requires that `<consequence>` and `<alternative>` are LValues.</span></span>
- <span data-ttu-id="4d0d0-410">`ref` 条件表达式本身是左值和</span><span class="sxs-lookup"><span data-stu-id="4d0d0-410">`ref` conditional expression itself is an LValue and</span></span>
- <span data-ttu-id="4d0d0-411">如果 `<consequence>` 和 `<alternative>` 都是可写的左值，则 `ref` 条件表达式是可写的</span><span class="sxs-lookup"><span data-stu-id="4d0d0-411">`ref` conditional expression is writeable if both `<consequence>` and `<alternative>` are writeable LValues</span></span>

<span data-ttu-id="4d0d0-412">示例：</span><span class="sxs-lookup"><span data-stu-id="4d0d0-412">Examples:</span></span>  
<span data-ttu-id="4d0d0-413">`ref` 三元是左值，因此它可以通过引用传递/赋值/返回;</span><span class="sxs-lookup"><span data-stu-id="4d0d0-413">`ref` ternary is an LValue and as such it can be passed/assigned/returned by reference;</span></span>
```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="4d0d0-414">作为 LValue，还可以分配给。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-414">Being an LValue, it can also be assigned to.</span></span>
```csharp
     // assign to
     (arr != null ? ref arr[0]: ref otherArr[0]) = 1;

     // error. readOnlyField is readonly and thus conditional expression is readonly
     (arr != null ? ref arr[0]: ref obj.readOnlyField) = 1;
```

<span data-ttu-id="4d0d0-415">可用作方法调用的接收方，并在必要时跳过复制。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-415">Can be used as a receiver of a method call and skip copying if necessary.</span></span>
```csharp
     // no copies
     (arr != null ? ref arr[0]: ref otherArr[0]).StructMethod();

     // invoked on a copy.
     // The receiver is `readonly` because readOnlyField is readonly.
     (arr != null ? ref arr[0]: ref obj.readOnlyField).StructMethod();

     // no copies. `ReadonlyStructMethod` is a method on a `readonly` struct
     // and can be invoked directly on a readonly receiver
     (arr != null ? ref arr[0]: ref obj.readOnlyField).ReadonlyStructMethod();
```

<span data-ttu-id="4d0d0-416">也可以在常规（不引用）上下文中使用三元 `ref`。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-416">`ref` ternary can be used in a regular (not ref) context as well.</span></span>
```csharp
     // only an example
     // a regular ternary could work here just the same
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```

### <a name="drawbacks"></a><span data-ttu-id="4d0d0-417">缺点</span><span class="sxs-lookup"><span data-stu-id="4d0d0-417">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="4d0d0-418">对于引用和只读引用的增强支持，我可以看到两个主要参数：</span><span class="sxs-lookup"><span data-stu-id="4d0d0-418">I can see two major arguments against enhanced support for references and readonly references:</span></span>

1) <span data-ttu-id="4d0d0-419">此处解决的问题很旧。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-419">The problems that are solved here are very old.</span></span> <span data-ttu-id="4d0d0-420">为什么现在突然解决它们，尤其是因为它不会帮助现有代码？</span><span class="sxs-lookup"><span data-stu-id="4d0d0-420">Why suddenly solve them now, especially since it would not help existing code?</span></span>

<span data-ttu-id="4d0d0-421">正如我们C#在新域中使用的一样，一些问题会变得更加突出。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-421">As we find C# and .Net used in new domains, some problems become more prominent.</span></span>  
<span data-ttu-id="4d0d0-422">作为比计算开销平均更重要的环境的示例，我可以列出</span><span class="sxs-lookup"><span data-stu-id="4d0d0-422">As examples of environments that are more critical than average about computation overheads, I can list</span></span>

* <span data-ttu-id="4d0d0-423">用于计算费用和响应能力的云/数据中心方案是一种竞争优势。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-423">cloud/datacenter scenarios where computation is billed for and responsiveness is a competitive advantage.</span></span>
* <span data-ttu-id="4d0d0-424">游戏/VR/AR，其中包含对延迟的软实时要求</span><span class="sxs-lookup"><span data-stu-id="4d0d0-424">Games/VR/AR with soft-realtime requirements on latencies</span></span>     

<span data-ttu-id="4d0d0-425">此功能不会牺牲任何现有的优势（如类型安全），同时允许在某些常见情况下降低开销。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-425">This feature does not sacrifice any of the existing strengths such as type-safety, while allowing to lower overheads in some common scenarios.</span></span>

2) <span data-ttu-id="4d0d0-426">我们是否可以合理保证被调用方将在选择 `readonly` 合同时由规则进行播放？</span><span class="sxs-lookup"><span data-stu-id="4d0d0-426">Can we reasonably guarantee that the callee will play by the rules when it opts into `readonly` contracts?</span></span>

<span data-ttu-id="4d0d0-427">使用 `out`时，有类似的信任关系。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-427">We have similar trust when using `out`.</span></span> <span data-ttu-id="4d0d0-428">`out` 的实现不正确可能会导致未指定的行为，但在现实中，这种情况很少发生。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-428">Incorrect implementation of `out` can cause unspecified behavior, but in reality it rarely happens.</span></span>  

<span data-ttu-id="4d0d0-429">使正式验证规则熟悉 `ref readonly` 会进一步降低信任问题。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-429">Making the formal verification rules familiar with `ref readonly` would further mitigate the trust issue.</span></span>

### <a name="alternatives"></a><span data-ttu-id="4d0d0-430">备选项</span><span class="sxs-lookup"><span data-stu-id="4d0d0-430">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="4d0d0-431">主要的竞争设计实际上是 "无任何操作"。</span><span class="sxs-lookup"><span data-stu-id="4d0d0-431">The main competing design is really "do nothing".</span></span>

### <a name="unresolved-questions"></a><span data-ttu-id="4d0d0-432">未解决的问题</span><span class="sxs-lookup"><span data-stu-id="4d0d0-432">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

### <a name="design-meetings"></a><span data-ttu-id="4d0d0-433">设计会议</span><span class="sxs-lookup"><span data-stu-id="4d0d0-433">Design meetings</span></span>

<span data-ttu-id="4d0d0-434">https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md</span><span class="sxs-lookup"><span data-stu-id="4d0d0-434">https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md</span></span>
