---
ms.openlocfilehash: f238a711e710bbac7f5b7400fa938bd85dec00c6
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "79484140"
---
# <a name="support-for--and--on-tuple-types"></a>元组类型支持 = = 和！ =

允许表达式 `t1 == t2`，其中 `t1` 和 `t2` 是相同基数的元组或可为 null 的元组类型，并将其计算大致为 `temp1.Item1 == temp2.Item1 && temp1.Item2 == temp2.Item2` （假设 `var temp1 = t1; var temp2 = t2;`）。

相反，它会允许 `t1 != t2`，并将其评估为 `temp1.Item1 != temp2.Item1 || temp1.Item2 != temp2.Item2`。

在可为 null 的情况下，将使用 `temp1.HasValue` 和 `temp2.HasValue` 的其他检查。 例如，`nullableT1 == nullableT2` 计算为 `temp1.HasValue == temp2.HasValue ? (temp1.HasValue ? ... : true) : false`。

当元素比较返回非 bool 结果时（例如，当使用非布尔用户定义的 `operator ==` 或 `operator !=`，或在动态比较中）时，该结果将转换为 `bool` 或通过 `operator true` 或 `operator false` 运行以获取 `bool`。 元组比较始终最终返回 `bool`。

从C# 7.2 到，此类代码会产生错误（`error CS0019: Operator '==' cannot be applied to operands of type '(...)' and '(...)'`），除非存在用户定义的 `operator==`。

## <a name="details"></a>详细信息

绑定 `==` （或 `!=`）运算符时，现有规则为：（1）动态大小写，（2）重载决策，（3）失败。
此建议在（1）和（2）之间添加一个元组事例：如果比较运算符的两个操作数都是元组（具有元组类型或是元组文本），并且具有匹配基数，则比较是按元素执行的。 此元组相等性也会提升到可为 null 的元组。

将按从左到右的顺序计算两个操作数（在元组文本的情况下，以及它们的元素）。 然后，将每对元素用作操作数以递归方式绑定运算符 `==` （或 `!=`）。 任何具有编译时类型的元素都 `dynamic` 会导致错误。 这些元素的比较结果将用作条件 AND （or）运算符链中的操作数。

例如，在 `(int, (int, int)) t1, t2;`的上下文中，`t1 == (1, (2, 3))` 的计算结果为 `temp1.Item1 == temp2.Item1 && temp1.Item2.Item1 == temp2.Item2.Item1 && temp2.Item2.Item2 == temp2.Item2.Item2`。

当使用元组文本作为操作数（在任一侧）时，它会收到转换后的元组类型，这些类型是在绑定运算符 `==` （或 `!=`）元素时引入的按元素转换组成的。 

例如，在 `(1L, 2, "hello") == (1, 2L, null)`中，这两个元组文本的转换类型都是 `(long, long, string)`，第二个文本没有自然类型。


### <a name="deconstruction-and-conversions-to-tuple"></a>析构和到元组的转换
在 `(a, b) == x`中，`x` 可以析构到两个元素中的事实不起作用。 这可能是未来的建议，尽管它会提出有关 `x == y` 的问题（这是一种简单的比较或按元素比较，如果使用基数呢？）。
同样，转换为元组不起作用。

### <a name="tuple-element-names"></a>元组元素名称

转换元组文本时，如果在文本中提供了显式元组元素名称，则会发出警告，但它不与目标元组元素名称匹配。
我们在元组比较中使用相同的规则，因此假定 `(int a, int b) t` 在 `t == (c, d: 0)``d` 时发出警告。

### <a name="non-bool-element-wise-comparison-results"></a>非布尔元素比较结果

如果元素比较在元组相等性中是动态的，我们将使用运算符的动态调用 `false`，并将其取反以获取 `bool` 并继续进行更多元素比较。 

如果按元素比较在元组相等性中返回某些其他非布尔类型，则有两种情况：
- 如果非 bool 类型转换为 `bool`，将应用该转换，
- 如果没有此类转换，但该类型具有运算符 `false`，则将使用该运算符并使结果无效。

在元组不相等时，相同的规则同样适用，但我们将使用运算符 `true` （无否定）而不是运算符 `false`。

这些规则类似于在 `if` 语句和其他一些现有上下文中使用非布尔类型所涉及的规则。

## <a name="evaluation-order-and-special-cases"></a>计算顺序和特殊情况
首先计算左侧的值，然后计算右侧的值，然后根据从左到右的顺序进行元素比较（包括转换，并根据条件和/或运算符的现有规则提前退出）。

例如，如果存在从类型 `A` 到类型的转换 `B` 和方法 `(A, A) GetTuple()`，则计算 `(new A(1), (new B(2), new B(3))) == (new B(4), GetTuple())` 意味着：
- `new A(1)`
- `new B(2)`
- `new B(3)`
- `new B(4)`
- `GetTuple()`
- 然后计算元素的转换和比较和条件逻辑（将 `new A(1)` 转换为类型 `B`，然后将其与 `new B(4)`等进行比较。

### <a name="comparing-null-to-null"></a>比较 `null` 与 `null`

这是常规比较中的一种特殊情况，可以执行元组比较。 允许 `null == null` 比较，并且 `null` 文本不会获得任何类型。
在元组相等性中，这意味着也允许 `(0, null) == (0, null)`，且 `null` 和元组文本也不会获取类型。

### <a name="comparing-a-nullable-struct-to-null-without-operator"></a>将可为 null 的结构与 `null` 进行比较而不 `operator==`

这是常规比较的另一种特殊情况，它会执行元组比较。
如果没有 `operator==``struct S`，则允许 `(S?)x == null` 比较，并将其解释为 `((S?).x).HasValue`。
在元组相等时，将应用相同的规则，因此允许 `(0, (S?)x) == (0, null)`。

## <a name="compatibility"></a>兼容性

如果有人使用比较运算符的实现来编写自己的 `ValueTuple` 类型，则重载决策之前会选取该运算符。 但由于新的元组事例出现在重载决策之前，我们将用元组比较来处理这种情况，而不是依赖于用户定义的比较。

----

与[关系和类型测试运算符](../../spec/expressions.md#relational-and-type-testing-operators)相关，并与[#190](https://github.com/dotnet/csharplang/issues/190)相关
