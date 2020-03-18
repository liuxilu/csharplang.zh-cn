---
ms.openlocfilehash: 922353d043653ddb651534a01f3fb98f85cd756e
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483480"
---
# <a name="readonly-locals-and-parameters"></a>readonly 局部变量和参数

* [x] 建议
* [] 原型
* [] 实现
* [] 规范

## <a name="summary"></a>总结
[summary]: #summary

允许将局部变量和参数批注为 readonly，以防止这些局部变量和参数的浅变化。

## <a name="motivation"></a>动机
[motivation]: #motivation

现在，可将 `readonly` 关键字应用于字段;这样做的效果是确保字段只能在构造过程中写入（静态构造，对于静态字段，对于实例字段则为实例构造），这有助于开发人员通过意外覆盖不应修改的状态来避免出错。 不过，开发人员并不是要确保值不会改变的唯一地方。 特别是，通常会创建一个本地变量来存储临时状态，并意外更新该临时状态可能导致错误的计算和其他此类错误，尤其是在 lambda 中捕获此类 "局部变量" 时，它们会升级到字段，但目前没有办法将此类提升字段标记为 `readonly`。

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

局部变量也将可批注为 `readonly`，编译器确保它们只在声明时设置（在中C# ，某些局部变量已经隐式只读，如 "foreach" 循环中的迭代变量或 "using" 块中已使用的变量，但目前开发人员无法将其他局部变量标记为 `readonly`）。 此类 `readonly` 局部变量必须有初始值设定项：

```csharp
readonly long maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

作为 `readonly var`的速记，可以使用现有的上下文关键字 `let`，例如

```csharp
let maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

对于初始值设定项，没有任何特殊约束，可以是当前作为局部变量的初始值设定项的任何内容，例如

```csharp
readonly T data = arg1 ?? arg2;
```

使用 lambda 和闭包时，局部变量上的 `readonly` 特别有用。 当匿名方法或 lambda 访问封闭范围中的本地状态时，编译器将该状态捕获到闭包中，由 "显示类" 表示。  捕获的每个 "本地" 都是此类中的一个字段，但由于编译器以你的名义生成此字段，因此你不会有机会将其批注为 `readonly`，以便阻止 lambda 错误地写入 "local" （在引号中，因为它确实不是本地的，至少是在生成的 MSIL 中）。 使用 `readonly` 局部变量，编译器可以阻止 lambda 向本地写入，这在涉及多线程的情况下特别有用，在这种情况下，错误写入可能会导致出现危险但难以发现的并发 bug。

```csharp
readonly long index = ...;
Parallel.ForEach(data, item => {
    T element = item[index];
    index = 0; // Error: can't assign to readonly locals outside of declaration
});
```

作为本地的一种特殊形式，参数也将可批注为 `readonly`。 这不会影响该方法的调用方能够传递到参数的方式（就像在 `readonly` 字段中存储的值没有任何限制），而与任何 `readonly` 本地一样，编译器将禁止代码在声明后写入参数，这意味着禁止将方法的主体写入参数。

```csharp
public void Update(readonly int index = 0) // Default values are ok though not required
{
    ...
    index = 0; // Error: can't assign to readonly parameters
    ...
}
```

`readonly` 参数不会影响编译器为该方法发出的签名/元数据，只会影响编译器处理方法主体的编译的方式。 例如，基虚方法可以有 `readonly` 参数，并且该参数在重写中可以是可写的。

与字段一样，局部变量和参数 `readonly` 都是浅层，这会影响存储位置，但不会对对象图产生间接影响。 但是，对于字段，调用 `readonly` 本地/参数结构的方法实际上将创建结构的副本并对副本调用方法，以避免 `this`的内部变化。

`readonly` 局部变量和参数不能作为 `ref` 或 `out` 参数传递，除非也支持 `ref readonly`。

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

- `val` 可以用作 `let`的替代简写。

## <a name="unresolved-questions"></a>未解决的问题
[unresolved]: #unresolved-questions

- `readonly ref` / `ref readonly` / `readonly ref readonly`：我已将有关如何处理 `ref readonly` 的问题留给了此建议。
- 此建议不会处理 readonly 结构/不可变类型。 这留给了单独的提议。

## <a name="design-meetings"></a>设计会议

- 2015年1月21日（<https://github.com/dotnet/roslyn/issues/98>）
