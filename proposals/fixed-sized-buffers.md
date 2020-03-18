---
ms.openlocfilehash: b51f27b2f58fd19851c80beb9cedcbd32b80b165
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483570"
---
# <a name="fixed-sized-buffers"></a>固定大小的缓冲区

* [x] 建议
* [] 原型：未启动
* [] 实现：未启动
* [] 规范：未启动

## <a name="summary"></a>总结
[summary]: #summary

提供用于将固定大小的缓冲区声明为C#语言的常规用途和安全机制。

## <a name="motivation"></a>动机
[motivation]: #motivation

目前，用户能够在不安全的上下文中创建固定大小的缓冲区。 但是，这需要用户处理指针、手动执行边界检查并仅支持有限的一组类型（`bool`、`byte`、`char`、`short`、`int`、`long`、`sbyte`、`ushort`、`uint`、`ulong`、`float`和 `double`）。

最常见的问题是，固定大小的缓冲区无法在安全代码中编制索引。 无法使用更多的类型。

只需稍作调整，就能提供通用固定大小的缓冲区（支持任何类型），可以在安全的上下文中使用，并执行自动边界检查。

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

其中一种方法是通过以下内容声明安全固定大小的缓冲区：

```csharp
public fixed DXGI_RGB GammaCurve[1025];
```

编译器会将声明转换为类似于以下内容的内部表示形式

```csharp
[FixedBuffer(typeof(DXGI_RGB), 1024)]
public ConsoleApp1.<Buffer>e__FixedBuffer_1024<DXGI_RGB> GammaCurve;

// Pack = 0 is the default packing and should result in indexable layout.
[CompilerGenerated, UnsafeValueType, StructLayout(LayoutKind.Sequential, Pack = 0)]
struct <Buffer>e__FixedBuffer_1024<T>
{
    private T _e0;
    private T _e1;
    // _e2 ... _e1023
    private T _e1024;

    public ref T this[int index] => ref (uint)index <= 1024u ?
                                         ref RefAdd<T>(ref _e0, index):
                                         throw new IndexOutOfRange();
}
```

由于固定大小的缓冲区不再需要使用 `fixed`，因此允许任何元素类型是有意义的。  

> 注意：仍将支持 `fixed`，但仅当元素类型为 `blittable`

## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

* 向后兼容性可能存在一些挑战，但由于现有固定大小的缓冲区仅适用于选择的基元类型，因此，如果用户将固定缓冲区视为，则编译器可能会继续 "只工作"变为.
* 不兼容的构造可能需要使用略微不同 `v2` 编码才能隐藏旧编译器中的字段。
* 对于泛型类型，在 IL 规范中未定义打包。 尽管此方法应可行，但我们将边上未记录的行为。 我们应该将其记录下来，确保像 Mono 这样的其他 Jit 具有相同的行为。
* 为每个长度指定一个单独的类型（如果支持，则可能为 `readonly` 字段指定一个类型）将对元数据产生影响。 它将受给定应用中不同大小的数组数目的限制。
* `ref` math 不是正式可验证的（因为它不安全）。 我们需要寻找一种方法来更新验证规则，以了解我们的使用是否正常。

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

手动声明你的结构，并使用不安全代码来构造索引器。

## <a name="unresolved-questions"></a>未解决的问题
[unresolved]: #unresolved-questions

- 是否应允许 `readonly`？  （具有 readonly 索引器）
- 是否应允许数组初始值设定项？
- 是否需要 `fixed` 关键字？
- `foreach`?
- 仅结构中的实例字段？

## <a name="design-meetings"></a>设计会议

链接到影响此建议的设计说明，并在一个句子中介绍它们所导致的更改。