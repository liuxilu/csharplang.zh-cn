---
ms.openlocfilehash: 11e9d21bda9e69be692c5c5f5aee80c2da1894ab
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483600"
---
# <a name="unmanaged-type-constraint"></a>非托管类型约束

## <a name="summary"></a>总结
[summary]: #summary

非托管约束功能将向C#语言规范中称为 "非托管类型" 的类型的类强制执行语言强制。 这在第18.2 节中定义为类型，该类型不是引用类型，并且在任何嵌套级别都不包含引用类型字段。  

## <a name="motivation"></a>动机
[motivation]: #motivation

主要动机是使在中C#轻松编写低级别互操作代码。 非托管类型是互操作代码的核心构建基块之一，但缺乏泛型支持将无法跨所有非托管类型创建可重复使用的例程。 开发人员会被迫为库中的每个非托管类型创作相同的样板板代码：

```csharp
int Hash(Point point) { ... } 
int Hash(TimeSpan timeSpan) { ... } 
```

若要启用此类型的方案，语言会引入新的约束：非托管：

```csharp
void Hash<T>(T value) where T : unmanaged
{
    ...
}
```

仅适用于C#语言规范中的非托管类型定义的类型才能满足此约束。查看它的另一种方法是，类型满足非托管约束 iff 它也可用作指针。 

```csharp
Hash(new Point()); // Okay 
Hash(42); // Okay
Hash("hello") // Error: Type string does not satisfy the unmanaged constraint
```

具有非托管约束的类型参数可以使用所有可用于非托管类型的功能：指针、固定等。 

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

此约束还会使结构化数据和字节流之间能够有效地进行转换。 这是网络堆栈和序列化层中常见的一项操作：

```csharp
Span<byte> Convert<T>(ref T value) where T : unmanaged 
{
    ...
}
```

这样的例程非常有用，因为它们在编译时是认定安全的，而分配是免费的。  目前，互操作性作者不能执行此操作（即使它位于性能至关重要的层）。  相反，他们需要依赖于分配具有昂贵运行时检查的例程来验证值是否正确地被管理。

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

该语言将引入名为 `unmanaged`的新约束。 为了满足此约束，类型必须为结构，并且类型的所有字段必须属于以下类别之一：

- 类型为 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`、`bool`、`IntPtr` 或 `UIntPtr`。
- 为任意 `enum` 类型。
- 是指针类型。
- 是 satsifies `unmanaged` 约束的用户定义的结构。

编译器生成的实例字段（如支持自动实现的属性的实例字段）还必须满足这些约束。 

例如：

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

`unmanaged` 约束不能与 `struct`、`class` 或 `new()`组合。 此限制派生自 `unmanaged` 隐含 `struct` 因此其他约束并无意义的事实。

`unmanaged` 约束不由 CLR 强制执行，只由语言执行。 若要防止其他语言出现错误，则具有此约束的方法将受 mod 要求保护。这会阻止其他语言使用非托管类型的类型参数。

约束中 `unmanaged` 的令牌不是关键字，也不是上下文关键字。 相反，它与 `var`，因为它是在该位置进行评估，并将：

- 绑定到名为 `unmanaged`的用户定义的类型或引用类型：此操作将被视为处理任何其他命名类型约束。 
- 绑定到无类型：这将被解释为 `unmanaged` 约束。

如果有一个名为 `unmanaged` 的类型，并且该类型在当前上下文中没有任何限制，则将无法使用 `unmanaged` 约束。 这就是围绕功能 `var` 和用户定义类型的相同名称的规则。 

## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

此功能的主要缺点是它提供少量的开发人员：通常是低级别库作者或框架。  因此，对于少量开发人员而言，它花费了很多宝贵的语言时间。 

但这些框架通常是大多数 .NET 应用程序的基础。  因此，在此级别上 wins 会对 .NET 生态系统产生波纹效果。  这使得功能更值得考虑，即使是拥有有限的受众。

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

有几种方法可以考虑：

- 现状：此功能并不在其自身的优点上，开发人员继续使用隐式选择加入行为。

## <a name="questions"></a>问题
[quesions]: #questions

### <a name="metadata-representation"></a>元数据表示形式

F#语言对签名文件中的约束进行编码，这C#意味着不能重复使用其表示形式。 需要为此约束选择新属性。 此外，具有此约束的方法必须由 mod 请求保护。

### <a name="blittable-vs-unmanaged"></a>直接复制和非托管
该F#语言具有一个非常[类似的功能](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints)，该功能使用关键字非托管。 可直接复制的名称来自 Midori 中的使用。  可能想要在此处查看优先级，并改为使用非托管。 

**解决方法**语言决定使用非托管 

### <a name="verifier"></a>符

验证程序/运行时是否需要更新以了解如何使用指向泛型类型参数的指针？  或者，它是否可以正常工作，而无需进行任何更改？

**解决方法**无需更改。 所有指针类型都是不可验证的。 

## <a name="design-meetings"></a>设计会议

不适用
