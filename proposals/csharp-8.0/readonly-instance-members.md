---
ms.openlocfilehash: d0bb80305ccc755a555cf47a1d010fc4cb9a7bcd
ms.sourcegitcommit: 5688b13e66dd77b224a1223338de9e3b1f66d7f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79484368"
---
# <a name="readonly-instance-members"></a>只读实例成员

倡导问题： <https://github.com/dotnet/csharplang/issues/1710>

## <a name="summary"></a>总结
[summary]: #summary

提供一种方法来指定结构上的单个实例成员不修改状态，其方式与 `readonly struct` 指定无实例成员修改状态的方式相同。

值得注意的是，`readonly instance member`！ = `pure instance member`。 `pure` 实例成员保证不会修改任何状态。 `readonly` 实例成员只保证不会修改实例状态。

`readonly struct` 上的所有实例成员均可隐式 `readonly instance members`。 在非只读结构上声明的显式 `readonly instance members` 的行为方式相同。 例如，如果您调用实例成员（在当前实例上或实例的某个字段上），则它们仍会创建隐藏的副本。

## <a name="motivation"></a>动机
[motivation]: #motivation

现在，用户可以创建 `readonly struct` 类型，编译器强制所有字段都是只读的（而不是由任何实例成员修改状态）。 但是，在某些情况下，你有一个公开可访问字段或混合了转变和非转变成员的现有 API。 在这些情况下，不能将类型标记为 `readonly` （这是一项重大更改）。

这通常不会产生很大的影响，但 `in` 参数的情况除外。 对于非只读结构 `in` 参数，编译器将为每个实例成员调用创建参数的副本，因为它无法保证调用不会修改内部状态。 与刚刚通过值直接传递结构相比，这可能会导致大量的副本和总体性能更差。 有关示例，请参阅[sharplab](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==)上的此代码

可能发生隐藏副本的其他情况包括 `static readonly fields` 和 `literals`。 如果将来支持，`blittable constants` 将在同一船上结束;这就是，如果结构未标记为 `readonly`，则当前所有必要都有完整的副本（在实例成员调用上）。

## <a name="design"></a>设计
[design]: #design

允许用户指定实例成员本身 `readonly`，而不修改实例的状态（当然，由编译器完成的所有适当验证）。 例如：

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

可以将 Readonly 应用于属性访问器，以便指示不会在访问器中改变 `this`。 下面的示例具有 readonly 资源库，因为这些访问器修改了成员字段的状态，但不修改该成员字段的值。

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

将 `readonly` 应用于属性语法时，这意味着所有访问器都 `readonly`。

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

Readonly 只能应用于不会改变包含类型的访问器。

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

Readonly 可以应用于某些自动实现的属性，但它不会产生有意义的效果。 不管 `readonly` 关键字是否存在，编译器都将所有自动实现的 getter 视为只读。

```csharp
// Allowed
public readonly int Prop4 { get; }
public int Prop5 { readonly get; }
public int Prop6 { readonly get; set; }

// Not allowed
public readonly int Prop7 { get; set; }
public int Prop8 { get; readonly set; }
```

Readonly 可应用于手动实现的事件，但不能应用于类似于字段的事件。 Readonly 不能应用于单个事件访问器（添加/删除）。

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

其他一些语法示例：

* Expression expression-bodied 成员： `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`
* 泛型约束： `public static readonly void GenericMethod<T>(T value) where T : struct { }`

通常情况下，编译器将发出实例成员，并且还会发出一个编译器可识别的属性，指示该实例成员不会修改状态。 这实际上会导致隐藏的 `this` 参数成为 `in T` 而不是 `ref T`。

这样，用户就可以安全地调用所说的实例方法，无需编译器创建副本。

这些限制包括：

* `readonly` 修饰符不能应用于静态方法、构造函数或析构函数。
* `readonly` 修饰符不能应用于委托。
* `readonly` 修饰符不能应用于类或接口的成员。

## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

目前 `readonly struct` 方法存在相同的缺点。 某些代码可能仍会导致隐藏的副本。

## <a name="notes"></a>说明
[notes]: #notes

还可以使用属性或其他关键字。

此建议与（但更多是） `functional purity` 和/或 `constant expressions`的一部分相关，这两者都有一些现有的提议。
