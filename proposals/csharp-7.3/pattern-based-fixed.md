---
ms.openlocfilehash: 2070cf3b3269585055791adc3427cbd134df444d
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483630"
---
# <a name="pattern-based-fixed-statement"></a>基于模式的 `fixed` 语句

## <a name="summary"></a>总结
[summary]: #summary

引入允许类型参与 `fixed` 语句的模式。 

## <a name="motivation"></a>动机
[motivation]: #motivation

语言提供了一种机制，用于固定托管数据并获取指向基础缓冲区的本机指针。

```csharp
fixed(byte* ptr = byteArray)
{
   // ptr is a native pointer to the first element of the array
   // byteArray is protected from being moved/collected by the GC for the duration of this block 
}

```

可以参与 `fixed` 的一组类型是硬编码的，并仅限于数组和 `System.String`。 硬编码 "特殊" 类型不会在引入 `ImmutableArray<T>`、`Span<T>``Utf8String` 的新基元时进行缩放。 

此外，`System.String` 的当前解决方案依赖于相当严格的 API。 API 的形状表示，`System.String` 是一个连续的对象，它将 UTF16 编码数据嵌入对象标头的固定偏移量。 在多个可能需要更改基础布局的建议中发现此类方法存在问题。 需要能够切换到一些更灵活的方法，以便在非托管互操作目的上将 `System.String` 对象与其内部表示分离。 

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

## <a name="pattern"></a>*模式* ##
可行的基于模式的 "固定" 需要：
-   提供托管引用以固定实例并初始化指针（最好是相同的引用）
-   明确传达非托管元素的类型（即 "char" for "string"）
-   当没有任何引用时，规定 "empty" 情况下的行为。 
-   不应将 API 作者推送到会损害 `fixed`之外的类型的设计决策。

我认为上述内容可以通过识别一个特殊命名的引用成员： `ref [readonly] T GetPinnableReference()`来实现。

为了 `fixed` 语句使用，必须满足以下条件：

1. 只为一个类型提供了一个这样的成员。
1. 按 `ref` 或 `ref readonly`返回。 （允许`readonly` 使得不可变/只读类型的作者可以实现模式，而无需添加可在安全代码中使用的可写 API）
1. T 是非托管类型。
（因为 `T*` 成为指针类型。 如果展开 "非托管" 的概念，则该限制将自然展开。
1. 如果没有要固定的数据，则返回托管 `nullptr` –可能是传达空的最便宜的方式。
（请注意，"" 字符串返回对 "\ 0" 的引用，因为字符串以 null 结尾）

如果是 `#3`，我们可以允许空事例中的结果为未定义或特定于实现的结果。 但是，这可能会使 API 更具危险性，并且容易受到滥用和意外的兼容性负担。 

## <a name="translation"></a>*翻译* ##

```csharp
fixed(byte* ptr = thing)
{ 
    // <BODY>
}
```

成为以下伪代码（并非中C#的全部可表达）

```csharp
byte* ptr;
// specially decorated "pinned" IL local slot, not visible to user code.
pinned ref byte _pinned;

try
{
    // NOTE: null check is omitted for value types 
    // NOTE: `thing` is evaluated only once (temporary is introduced if necessary) 
    if (thing != null)
    {
        // obtain and "pin" the reference
        _pinned = ref thing.GetPinnableReference();

        // unsafe cast in IL
        ptr = (byte*)_pinned;
    }
    else
    {
        ptr = default(byte*);
    }

    // <BODY> 
}
finally   // finally can be omitted when not observable
{
    // "unpin" the object
    _pinned = nullptr;
}
```

## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

- GetPinnableReference 仅用于 `fixed`中，但不会阻止它在安全代码中使用，因此实现器必须记住这一点。

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

用户可以引入 GetPinnableReference 或类似成员，并将其用作
 
```csharp
fixed(byte* ptr = thing.GetPinnableReference())
{ 
    // <BODY>
}
```

如果需要备用解决方案，则没有适用于 `System.String` 的解决方案。

## <a name="unresolved-questions"></a>未解决的问题
[unresolved]: #unresolved-questions

- [] 在 "empty" 状态中的行为。 - `nullptr` 或 `undefined`？ 
- [] 是否应考虑扩展方法？ 
- [] 如果在 `System.String`上检测到模式，它是否会赢得此模式？ 

## <a name="design-meetings"></a>设计会议

尚无。 
