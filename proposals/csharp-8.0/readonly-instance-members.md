---
ms.openlocfilehash: d0bb80305ccc755a555cf47a1d010fc4cb9a7bcd
ms.sourcegitcommit: 5688b13e66dd77b224a1223338de9e3b1f66d7f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79484368"
---
# <a name="readonly-instance-members"></a><span data-ttu-id="1ce09-101">只读实例成员</span><span class="sxs-lookup"><span data-stu-id="1ce09-101">Readonly Instance Members</span></span>

<span data-ttu-id="1ce09-102">倡导问题： <https://github.com/dotnet/csharplang/issues/1710></span><span class="sxs-lookup"><span data-stu-id="1ce09-102">Championed Issue: <https://github.com/dotnet/csharplang/issues/1710></span></span>

## <a name="summary"></a><span data-ttu-id="1ce09-103">总结</span><span class="sxs-lookup"><span data-stu-id="1ce09-103">Summary</span></span>
[summary]: #summary

<span data-ttu-id="1ce09-104">提供一种方法来指定结构上的单个实例成员不修改状态，其方式与 `readonly struct` 指定无实例成员修改状态的方式相同。</span><span class="sxs-lookup"><span data-stu-id="1ce09-104">Provide a way to specify individual instance members on a struct do not modify state, in the same way that `readonly struct` specifies no instance members modify state.</span></span>

<span data-ttu-id="1ce09-105">值得注意的是，`readonly instance member`！ = `pure instance member`。</span><span class="sxs-lookup"><span data-stu-id="1ce09-105">It is worth noting that `readonly instance member` != `pure instance member`.</span></span> <span data-ttu-id="1ce09-106">`pure` 实例成员保证不会修改任何状态。</span><span class="sxs-lookup"><span data-stu-id="1ce09-106">A `pure` instance member guarantees no state will be modified.</span></span> <span data-ttu-id="1ce09-107">`readonly` 实例成员只保证不会修改实例状态。</span><span class="sxs-lookup"><span data-stu-id="1ce09-107">A `readonly` instance member only guarantees that instance state will not be modified.</span></span>

<span data-ttu-id="1ce09-108">`readonly struct` 上的所有实例成员均可隐式 `readonly instance members`。</span><span class="sxs-lookup"><span data-stu-id="1ce09-108">All instance members on a `readonly struct` could be considered implicitly `readonly instance members`.</span></span> <span data-ttu-id="1ce09-109">在非只读结构上声明的显式 `readonly instance members` 的行为方式相同。</span><span class="sxs-lookup"><span data-stu-id="1ce09-109">Explicit `readonly instance members` declared on non-readonly structs would behave in the same manner.</span></span> <span data-ttu-id="1ce09-110">例如，如果您调用实例成员（在当前实例上或实例的某个字段上），则它们仍会创建隐藏的副本。</span><span class="sxs-lookup"><span data-stu-id="1ce09-110">For example, they would still create hidden copies if you called an instance member (on the current instance or on a field of the instance) which was itself not-readonly.</span></span>

## <a name="motivation"></a><span data-ttu-id="1ce09-111">动机</span><span class="sxs-lookup"><span data-stu-id="1ce09-111">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="1ce09-112">现在，用户可以创建 `readonly struct` 类型，编译器强制所有字段都是只读的（而不是由任何实例成员修改状态）。</span><span class="sxs-lookup"><span data-stu-id="1ce09-112">Today, users have the ability to create `readonly struct` types which the compiler enforces that all fields are readonly (and by extension, that no instance members modify the state).</span></span> <span data-ttu-id="1ce09-113">但是，在某些情况下，你有一个公开可访问字段或混合了转变和非转变成员的现有 API。</span><span class="sxs-lookup"><span data-stu-id="1ce09-113">However, there are some scenarios where you have an existing API that exposes accessible fields or that has a mix of mutating and non-mutating members.</span></span> <span data-ttu-id="1ce09-114">在这些情况下，不能将类型标记为 `readonly` （这是一项重大更改）。</span><span class="sxs-lookup"><span data-stu-id="1ce09-114">Under these circumstances, you cannot mark the type as `readonly` (it would be a breaking change).</span></span>

<span data-ttu-id="1ce09-115">这通常不会产生很大的影响，但 `in` 参数的情况除外。</span><span class="sxs-lookup"><span data-stu-id="1ce09-115">This normally doesn't have much impact, except in the case of `in` parameters.</span></span> <span data-ttu-id="1ce09-116">对于非只读结构 `in` 参数，编译器将为每个实例成员调用创建参数的副本，因为它无法保证调用不会修改内部状态。</span><span class="sxs-lookup"><span data-stu-id="1ce09-116">With `in` parameters for non-readonly structs, the compiler will make a copy of the parameter for each instance member invocation, since it cannot guarantee that the invocation does not modify internal state.</span></span> <span data-ttu-id="1ce09-117">与刚刚通过值直接传递结构相比，这可能会导致大量的副本和总体性能更差。</span><span class="sxs-lookup"><span data-stu-id="1ce09-117">This can lead to a multitude of copies and worse overall performance than if you had just passed the struct directly by value.</span></span> <span data-ttu-id="1ce09-118">有关示例，请参阅[sharplab](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==)上的此代码</span><span class="sxs-lookup"><span data-stu-id="1ce09-118">For an example, see this code on [sharplab](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==)</span></span>

<span data-ttu-id="1ce09-119">可能发生隐藏副本的其他情况包括 `static readonly fields` 和 `literals`。</span><span class="sxs-lookup"><span data-stu-id="1ce09-119">Some other scenarios where hidden copies can occur include `static readonly fields` and `literals`.</span></span> <span data-ttu-id="1ce09-120">如果将来支持，`blittable constants` 将在同一船上结束;这就是，如果结构未标记为 `readonly`，则当前所有必要都有完整的副本（在实例成员调用上）。</span><span class="sxs-lookup"><span data-stu-id="1ce09-120">If they are supported in the future, `blittable constants` would end up in the same boat; that is they all currently necessitate a full copy (on instance member invocation) if the struct is not marked `readonly`.</span></span>

## <a name="design"></a><span data-ttu-id="1ce09-121">设计</span><span class="sxs-lookup"><span data-stu-id="1ce09-121">Design</span></span>
[design]: #design

<span data-ttu-id="1ce09-122">允许用户指定实例成员本身 `readonly`，而不修改实例的状态（当然，由编译器完成的所有适当验证）。</span><span class="sxs-lookup"><span data-stu-id="1ce09-122">Allow a user to specify that an instance member is, itself, `readonly` and does not modify the state of the instance (with all the appropriate verification done by the compiler, of course).</span></span> <span data-ttu-id="1ce09-123">例如：</span><span class="sxs-lookup"><span data-stu-id="1ce09-123">For example:</span></span>

```csharp
public struct Vector2
{
    public float x;
    public float y;

    public readonly float GetLengthReadonly()
    {
        return MathF.Sqrt(LengthSquared);
    }

    public float GetLength()
    {
        return MathF.Sqrt(LengthSquared);
    }

    public readonly float GetLengthIllegal()
    {
        var tmp = MathF.Sqrt(LengthSquared);

        x = tmp;    // Compiler error, cannot write x
        y = tmp;    // Compiler error, cannot write y

        return tmp;
    }

    public float LengthSquared
    {
        readonly get
        {
            return (x * x) +
                   (y * y);
        }
    }
}

public static class MyClass
{
    public static float ExistingBehavior(in Vector2 vector)
    {
        // This code causes a hidden copy, the compiler effectively emits:
        //    var tmpVector = vector;
        //    return tmpVector.GetLength();
        //
        // This is done because the compiler doesn't know that `GetLength()`
        // won't mutate `vector`.

        return vector.GetLength();
    }

    public static float ReadonlyBehavior(in Vector2 vector)
    {
        // This code is emitted exactly as listed. There are no hidden
        // copies as the `readonly` modifier indicates that the method
        // won't mutate `vector`.

        return vector.GetLengthReadonly();
    }
}
```

<span data-ttu-id="1ce09-124">可以将 Readonly 应用于属性访问器，以便指示不会在访问器中改变 `this`。</span><span class="sxs-lookup"><span data-stu-id="1ce09-124">Readonly can be applied to property accessors to indicate that `this` will not be mutated in the accessor.</span></span> <span data-ttu-id="1ce09-125">下面的示例具有 readonly 资源库，因为这些访问器修改了成员字段的状态，但不修改该成员字段的值。</span><span class="sxs-lookup"><span data-stu-id="1ce09-125">The following examples have readonly setters because those accessors modify the state of member field, but do not modify the value of that member field.</span></span>

```csharp
public int Prop1
{
    readonly get
    {
        return this._store["Prop1"];
    }
    readonly set
    {
        this._store["Prop1"] = value;
    }
}
```

<span data-ttu-id="1ce09-126">将 `readonly` 应用于属性语法时，这意味着所有访问器都 `readonly`。</span><span class="sxs-lookup"><span data-stu-id="1ce09-126">When `readonly` is applied to the property syntax, it means that all accessors are `readonly`.</span></span>

```csharp
public readonly int Prop2
{
    get
    {
        return this._store["Prop2"];
    }
    set
    {
        this._store["Prop2"] = value;
    }
}
```

<span data-ttu-id="1ce09-127">Readonly 只能应用于不会改变包含类型的访问器。</span><span class="sxs-lookup"><span data-stu-id="1ce09-127">Readonly can only be applied to accessors which do not mutate the containing type.</span></span>

```csharp
public int Prop3
{
    readonly get
    {
        return this._prop3;
    }
    set
    {
        this._prop3 = value;
    }
}
```

<span data-ttu-id="1ce09-128">Readonly 可以应用于某些自动实现的属性，但它不会产生有意义的效果。</span><span class="sxs-lookup"><span data-stu-id="1ce09-128">Readonly can be applied to some auto-implemented properties, but it won't have a meaningful effect.</span></span> <span data-ttu-id="1ce09-129">不管 `readonly` 关键字是否存在，编译器都将所有自动实现的 getter 视为只读。</span><span class="sxs-lookup"><span data-stu-id="1ce09-129">The compiler will treat all auto-implemented getters as readonly whether or not the `readonly` keyword is present.</span></span>

```csharp
// Allowed
public readonly int Prop4 { get; }
public int Prop5 { readonly get; }
public int Prop6 { readonly get; set; }

// Not allowed
public readonly int Prop7 { get; set; }
public int Prop8 { get; readonly set; }
```

<span data-ttu-id="1ce09-130">Readonly 可应用于手动实现的事件，但不能应用于类似于字段的事件。</span><span class="sxs-lookup"><span data-stu-id="1ce09-130">Readonly can be applied to manually-implemented events, but not field-like events.</span></span> <span data-ttu-id="1ce09-131">Readonly 不能应用于单个事件访问器（添加/删除）。</span><span class="sxs-lookup"><span data-stu-id="1ce09-131">Readonly cannot be applied to individual event accessors (add/remove).</span></span>

```csharp
// Allowed
public readonly event Action<EventArgs> Event1
{
    add { }
    remove { }
}

// Not allowed
public readonly event Action<EventArgs> Event2;
public event Action<EventArgs> Event3
{
    readonly add { }
    readonly remove { }
}
public static readonly event Event4
{
    add { }
    remove { }
}
```

<span data-ttu-id="1ce09-132">其他一些语法示例：</span><span class="sxs-lookup"><span data-stu-id="1ce09-132">Some other syntax examples:</span></span>

* <span data-ttu-id="1ce09-133">Expression expression-bodied 成员： `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`</span><span class="sxs-lookup"><span data-stu-id="1ce09-133">Expression bodied members: `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`</span></span>
* <span data-ttu-id="1ce09-134">泛型约束： `public static readonly void GenericMethod<T>(T value) where T : struct { }`</span><span class="sxs-lookup"><span data-stu-id="1ce09-134">Generic constraints: `public static readonly void GenericMethod<T>(T value) where T : struct { }`</span></span>

<span data-ttu-id="1ce09-135">通常情况下，编译器将发出实例成员，并且还会发出一个编译器可识别的属性，指示该实例成员不会修改状态。</span><span class="sxs-lookup"><span data-stu-id="1ce09-135">The compiler would emit the instance member, as usual, and would additionally emit a compiler recognized attribute indicating that the instance member does not modify state.</span></span> <span data-ttu-id="1ce09-136">这实际上会导致隐藏的 `this` 参数成为 `in T` 而不是 `ref T`。</span><span class="sxs-lookup"><span data-stu-id="1ce09-136">This effectively causes the hidden `this` parameter to become `in T` instead of `ref T`.</span></span>

<span data-ttu-id="1ce09-137">这样，用户就可以安全地调用所说的实例方法，无需编译器创建副本。</span><span class="sxs-lookup"><span data-stu-id="1ce09-137">This would allow the user to safely call said instance method without the compiler needing to make a copy.</span></span>

<span data-ttu-id="1ce09-138">这些限制包括：</span><span class="sxs-lookup"><span data-stu-id="1ce09-138">The restrictions would include:</span></span>

* <span data-ttu-id="1ce09-139">`readonly` 修饰符不能应用于静态方法、构造函数或析构函数。</span><span class="sxs-lookup"><span data-stu-id="1ce09-139">The `readonly` modifier cannot be applied to static methods, constructors or destructors.</span></span>
* <span data-ttu-id="1ce09-140">`readonly` 修饰符不能应用于委托。</span><span class="sxs-lookup"><span data-stu-id="1ce09-140">The `readonly` modifier cannot be applied to delegates.</span></span>
* <span data-ttu-id="1ce09-141">`readonly` 修饰符不能应用于类或接口的成员。</span><span class="sxs-lookup"><span data-stu-id="1ce09-141">The `readonly` modifier cannot be applied to members of class or interface.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="1ce09-142">缺点</span><span class="sxs-lookup"><span data-stu-id="1ce09-142">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="1ce09-143">目前 `readonly struct` 方法存在相同的缺点。</span><span class="sxs-lookup"><span data-stu-id="1ce09-143">Same drawbacks as exist with `readonly struct` methods today.</span></span> <span data-ttu-id="1ce09-144">某些代码可能仍会导致隐藏的副本。</span><span class="sxs-lookup"><span data-stu-id="1ce09-144">Certain code may still cause hidden copies.</span></span>

## <a name="notes"></a><span data-ttu-id="1ce09-145">说明</span><span class="sxs-lookup"><span data-stu-id="1ce09-145">Notes</span></span>
[notes]: #notes

<span data-ttu-id="1ce09-146">还可以使用属性或其他关键字。</span><span class="sxs-lookup"><span data-stu-id="1ce09-146">Using an attribute or another keyword may also be possible.</span></span>

<span data-ttu-id="1ce09-147">此建议与（但更多是） `functional purity` 和/或 `constant expressions`的一部分相关，这两者都有一些现有的提议。</span><span class="sxs-lookup"><span data-stu-id="1ce09-147">This proposal is somewhat related to (but is more a subset of) `functional purity` and/or `constant expressions`, both of which have had some existing proposals.</span></span>
