---
ms.openlocfilehash: 36f3e6204d12c2569b3a55f3a47f58337e8a08e4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483606"
---
# <a name="indexing-fixed-fields-should-not-require-pinning-regardless-of-the-movableunmovable-context"></a>与可移动/不可移动的上下文无关，索引 `fixed` 字段不应进行固定。 #

更改的大小为 bug 修复。 它可以处于7.3，并且不会与我们更进一步的方向发生冲突。
这一更改只是为了允许下列方案工作，即使 `s` 可移动。 当 `s` 不可可移动时，它已经有效。 

注意：在任何一种情况下，它仍需要 `unsafe` 上下文。 可以读取未初始化的数据或甚至超出范围。 这不会更改。

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int p = s.myFixedField[5]; // indexing fixed-size array fields would be ok
    }
}
```

我在这里看到的主要 "挑战" 是如何解释规范中的 relaxation。特别是，由于以下情况仍需要固定。 （因为 `s` 是可移动的，并将该字段显式用作指针）

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int* ptr = s.myFixedField; // taking a pointer explicitly still requires pinning.
        int p = ptr[5];
    }
}
```

需要固定目标的原因之一是代码生成策略的项目，我们总是转换为非托管指针，从而强制用户通过 `fixed` 语句进行固定。 但是，在执行索引时，不需要转换为非托管。 当使用托管指针形式的接收方时，相同的 unsafe 指针公式同样适用。 如果这样做，则会管理中间引用（GC 跟踪），无需进行固定。

更改 https://github.com/dotnet/roslyn/pull/24966 是放宽此要求的原型 PR。
