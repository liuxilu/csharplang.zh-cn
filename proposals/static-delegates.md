---
ms.openlocfilehash: a8822137c85f449444ed675c6f2912315c041691
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483474"
---
# <a name="static-delegates"></a>静态委托

* [x] 建议
* [] 原型：未启动
* [] 实现：未启动
* [] 规范：未启动

## <a name="summary"></a>总结
[summary]: #summary

为C#语言提供常规用途的轻型回调功能。

## <a name="motivation"></a>动机
[motivation]: #motivation

目前，用户能够使用 `System.Delegate` 类型创建回调。 不过，这些都是相当重型的（例如，需要一个堆分配，并始终对将回调链接在一起）。

此外，`System.Delegate` 不会提供与非托管函数指针的最佳互操作性，也就是说，它在跨托管/非托管边界时无需直接复制并需要封送处理。

只需稍作调整，就能提供一种新类型的委托，该委托类型为轻型、通用和 interops，适用于本机代码。

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

可以通过以下方法声明静态委托：

```C#
static delegate int Func()
```

另外，还可以通过类似于 `System.Runtime.InteropServices.UnmanagedFunctionPointer` 的内容来特性声明，以便可以控制调用约定、字符串封送处理和设置的上一错误行为。 注意：使用 `System.Runtime.InteropServices.UnmanagedFunctionPointer` 本身将不起作用，因为它仅可用于委托。

编译器会将声明转换为类似于以下内容的内部表示形式

```C#
struct <Func>e__StaticDelegate
{
    IntPtr pFunction;

    static int WellKnownCompilerName();
}
```

也就是说，它由一个结构内部表示，该结构具有 `IntPtr` 类型的单个成员（此结构可直接复制到所有堆分配，不会产生任何堆分配）：
* 成员包含要作为回调的函数的地址。
* 类型声明与回调的方法签名匹配的方法。
* 结构的名称不应是用户可构造（与在内部生成的其他支持结构中的情况相同）。
 * 例如：固定大小缓冲区生成名称格式为 `<name>e__FixedBuffer` （`<` 并且 `>` 为标识符一部分的结构，并使标识符在中C#不可构造，但在 IL 中仍可用。
* 方法声明的名称应为在所有静态委托类型中使用的已知名称（这允许编译器在确定签名时知道要查找的名称）。

静态委托的值只能绑定到与回调签名匹配的静态方法。

不支持将回调链接在一起。

回调的调用将由 `calli` 指令实现。

## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

静态委托不能与使用常规委托的现有 Api 一起使用（需要将所说的静态委托包装在同一签名的常规委托中）。
* 假设 `System.Delegate` 在内部表示为一组 `object` 并 `IntPtr` 字段（ http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs)，则可以允许将静态委托隐式转换为具有匹配方法签名的 `System.Delegate`。 还可以按相反方向提供显式转换，前提是 `System.Delegate` 符合作为静态委托的所有要求。

需要额外的工作，以使静态委托在核心框架中轻松使用。

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

TBD

## <a name="unresolved-questions"></a>未解决的问题
[unresolved]: #unresolved-questions

设计的哪些部分仍是待定的？

## <a name="design-meetings"></a>设计会议

链接到影响此建议的设计说明，并在一个句子中介绍它们所导致的更改。


