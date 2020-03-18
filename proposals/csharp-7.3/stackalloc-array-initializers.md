---
ms.openlocfilehash: 7ea62713416ef82034963aef06f3cb11703342ed
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483588"
---
# <a name="stackalloc-array-initializers"></a>Stackalloc 数组初始值设定项

## <a name="summary"></a>总结
[summary]: #summary

允许将数组初始值设定项语法用于 `stackalloc`。

## <a name="motivation"></a>动机
[motivation]: #motivation

普通数组可以在创建时初始化其元素。 在 `stackalloc` 情况下，似乎合理。

不允许使用这种语法的问题，这种情况的发生方式 `stackalloc` 相对频繁。  
请参阅，例如[#1112](https://github.com/dotnet/csharplang/issues/1112)

## <a name="detailed-design"></a>详细设计

可以通过以下语法创建普通的数组：

```csharp
new int[3]
new int[3] { 1, 2, 3 }
new int[] { 1, 2, 3 }
new[] { 1, 2, 3 }
```

我们应该允许通过创建堆栈分配的数组：  

```csharp
stackalloc int[3]               // currently allowed
stackalloc int[3] { 1, 2, 3 }
stackalloc int[] { 1, 2, 3 }
stackalloc[] { 1, 2, 3 }
```

所有事例的语义大致与数组的语义相同。  
例如：在最后一种情况下，元素类型从初始值设定项中推断出来，并且必须为 "非托管" 类型。

注意：该功能不依赖于要 `Span<T>`的目标。 它就像在 `T*` 情况下一样适用，因此在 `Span<T>` 情况下将其谓语视为合理。  

## <a name="translation"></a>翻译

简单的实现可以在创建后通过一系列基于元素的赋值直接初始化数组。  

与使用数组的情况相似，检测所有或大多数元素都是可直接复制到类型的情况，并通过复制所有常量元素的预先创建的状态来使用更有效的方法。 

## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

这是一项便利功能。 可以不执行任何操作。

## <a name="unresolved-questions"></a>未解决的问题
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>设计会议

尚无。 
