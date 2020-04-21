---
ms.openlocfilehash: 80ccdb75e2f5a022e367f3c023ea4464195deaaf
ms.sourcegitcommit: 95f5f86ba2e2a23cd4fb37bd9d1ff690c83d1191
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "81647118"
---
# <a name="target-typed-conditional-expression"></a>目标类型条件表达式

对于条件表达式`c ? e1 : e2`，

1. 和 ， 没有通用`e1`类型`e2`， 或
2. 公共类型存在，但其中一个表达式`e1`或`e2`没有隐式转换为该类型

我们定义一个新的*条件表达式转换*，允许隐式从条件`T`表达式转换为任何类型，其中`e1``T``e2``T`有从 到 和  如果条件表达式之间`e1`既没有公共类型，也`e2`不受*条件表达式转换*的约束，则这是一个错误。

### <a name="open-issues"></a>Open Issues

我们希望将此目标类型扩展到条件表达式具有公共类型`e1`，`e2`但没有从该公共类型转换为目标类型的情况。 这将使条件表达式的目标类型与交换机表达式的目标类型对齐。 然而，我们担心这将是一个重大的变化：

```csharp
M(b ? 1 : 2); // calls M(long) without this feature; calls M(short) with this feature

void M(short);
void M(long);
```

我们可以通过修改规则来缩小中断更改的范围，以便*更好地从表达式转换*：如果转换为 T1 不是*条件表达式转换*，并且转换为 T2 是*条件表达式转换*，则从表达式转换为 T1 比转换 T2 更好。  这解决了上述程序中的突发性更改（无论是否使用此功能调用`M(long)`）。 这种方法确实有两个小缺点。  首先，它与交换机表达式不完全相同：

```csharp
M(b ? 1 : 2); // calls M(long)
M(b switch { true => 1, false => 2 }); // calls M(short)
```

这仍然是一个重大的变化，但其范围不太可能影响实际程序：

```csharp
M(b ? 1 : 2, 1); // calls M(long, long) without this feature; ambiguous with this feature.

M(short, short);
M(long, long);
```

这变得模棱两可，`long`因为转换对第一个参数更好（因为它不使用*条件表达式转换*），`short`但转换为对第二个参数更好（因为`short`转换目标比 ）*better conversion target*`long`更好。 这种重大更改似乎不太严重，因为它不会默默地更改现有程序的行为。

如果我们选择对提案作这种改变，我们就会改变

> #### <a name="better-conversion-from-expression"></a>更好地从表达式转换
> 
> `C1`给定从表达式`E`转换为类型的`T1`隐式转换，以及从表达式`C2``E`转换为类型的`T2`隐式转换`C1`，与不完全匹配`C2``E``T2`且至少包含以下一项的***转换相比，它是一种更好的转换***：
> 
> * `E`完全匹配`T1`（[完全匹配表达式](expressions.md#exactly-matching-expression)）
> * `T1`是一个更好的转换目标比`T2`（[更好的转换目标](expressions.md#better-conversion-target)）

to

> #### <a name="better-conversion-from-expression"></a>更好地从表达式转换
> 
> `C1`给定从表达式`E`转换为类型的`T1`隐式转换，以及从表达式`C2``E`转换为类型的`T2`隐式转换`C1`，与不完全匹配`C2``E``T2`且至少包含以下一项的***转换相比，它是一种更好的转换***：
> 
> * `E`完全匹配`T1`（[完全匹配表达式](expressions.md#exactly-matching-expression)）
> * `T1``T2`是比 （[更好的转换目标](expressions.md#better-conversion-target)） 更好的转换目标`C1`， 和 不是*条件表达式转换*或`C2`[条件表达式转换] 更好。
