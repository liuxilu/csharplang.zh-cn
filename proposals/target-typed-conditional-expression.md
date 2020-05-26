---
ms.openlocfilehash: cd73a3d7289205f65f5144a98d32da06dfed3efc
ms.sourcegitcommit: e355841daad8c4672fae6a49c98653952d89a9cb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2020
ms.locfileid: "83775415"
---
# <a name="target-typed-conditional-expression"></a>目标类型的条件表达式

对于条件表达式 `c ? e1 : e2` ，当

1. 和没有通用类型 `e1` `e2` ，或
2. ，它的通用类型为，但其中一个表达式 `e1` 为 `e2` ，或者没有到该类型的隐式转换

我们定义了一个新的*条件表达式转换*，该转换允许从条件表达式到的任何类型的隐式转换 `T` （从到的转换到 `e1` `T` ，以及从到的转换） `e2` `T` 。  如果条件表达式在和之间既没有通用类型，也不符合 `e1` `e2` *条件表达式转换*，则是错误的。

### <a name="open-issues"></a>未结的问题

我们想要将此目标类型扩展到条件表达式具有通用类型的情况 `e1` ， `e2` 但不存在从该通用类型到目标类型的转换。 这会使条件表达式的目标类型化成为 switch 表达式的目标类型的对齐方式。 但是，我们担心会是一项重大更改：

```csharp
M(b ? 1 : 2); // calls M(long) without this feature; calls M(short) with this feature

void M(short);
void M(long);
```

我们可以通过修改规则以*更好地从表达式转换*来减少重大更改的范围：如果转换为 t1 不是*条件表达式转换*，则从表达式转换为 t1 是比转换到 t2 更好的转换，而转换到 t2 是*条件表达式转换*。  这解决了上述程序中的重大更改（它 `M(long)` 通过或不带此功能调用）。 此方法有两个小缺点。  首先，它与 switch 表达式并不完全相同：

```csharp
M(b ? 1 : 2); // calls M(long)
M(b switch { true => 1, false => 2 }); // calls M(short)
```

这仍是一项重大更改，但其作用域不太可能影响实际程序：

```csharp
M(b ? 1 : 2, 1); // calls M(long, long) without this feature; ambiguous with this feature.

M(short, short);
M(long, long);
```

这种方法变得不明确，因为的转换 `long` 更适合第一个参数（因为它不使用*条件表达式转换*），但转换为 `short` 更适合于第二个参数（因为 `short` 比*更好的转换目标* `long` ）。 此重大更改看起来不太严重，因为它不会在无提示的情况下更改现有程序的行为。

如果我们选择对建议进行此更改，我们将更改

> #### <a name="better-conversion-from-expression"></a>表达式的更好转换
> 
> 给定了 `C1` 从表达式转换为类型的隐式转换 `E` `T1` ，以及 `C2` 从表达式转换为类型的隐式转换， `E` `T2` `C1` 是比***better conversion*** `C2` `E` 不完全匹配 `T2` 且至少包含以下其中一项的更好的转换：
> 
> * `E`完全匹配 `T1` （[完全匹配的表达式](expressions.md#exactly-matching-expression)）
> * `T1`比更好的转换目标 `T2` （[更好的转换目标](expressions.md#better-conversion-target)）

更改为

> #### <a name="better-conversion-from-expression"></a>表达式的更好转换
> 
> 给定了 `C1` 从表达式转换为类型的隐式转换 `E` `T1` ，以及 `C2` 从表达式转换为类型的隐式转换， `E` `T2` `C1` 是比***better conversion*** `C2` `E` 不完全匹配 `T2` 且至少包含以下其中一项的更好的转换：
> 
> * `E`完全匹配 `T1` （[完全匹配的表达式](expressions.md#exactly-matching-expression)）
> * **`C1`不是一个*条件表达式转换*，并且 `C2` 是一个 * 条件表达式转换 * * *。
> * `T1`比 `T2` （[更好的转换目标](expressions.md#better-conversion-target)） * * 更好的转换目标，并且 `C1` 和 `C2` 都是*条件表达式*转换，或者两者都不是 * 条件表达式转换 * * *。
