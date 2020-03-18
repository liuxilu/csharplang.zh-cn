---
ms.openlocfilehash: 9647fff40a1e45bef917f140612ae4e91abea958
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483558"
---
# <a name="lambda-attributes"></a>Lambda 特性

* [x] 建议
* [] 原型
* [] 实现
* [] 规范

## <a name="summary"></a>总结
[summary]: #summary

允许将特性应用于 lambda （和匿名方法）以及 lambda/匿名方法参数，因为它们可以在常规方法上。

## <a name="motivation"></a>动机
[motivation]: #motivation

两个主要动机：

1. 在编译时提供分析器的元数据可见性。
2. 若为，则在运行时提供对反射和工具的可见性。

作为（1）的一个示例：对于性能敏感的代码，能够使分析器在为关闭状态的 lambda 分配闭包和委托时能够标记。  通常，此类代码的开发人员将不再能够避免捕获任何状态，以便编译器可以为该方法生成静态方法和可缓存的委托，或者开发人员将确保只 `this`关闭的唯一状态，从而至少允许编译器避免分配闭包对象。  但是，如果没有语言支持来限制可能捕获的内容，则很容易意外关闭状态。  如果开发人员可以使用特性批注 lambda 以指示允许关闭的状态，这会很有用，例如：

```csharp
[CaptureNone] // can't close over any instance state
[CaptureThis] // can only capture `this` and no other instance state
[CaptureAny] // can close over any instance state
```

当错误捕获状态时，可以编写分析器来标记，例如：

```csharp
var results = collection.Select([CaptureNone](i) => Process(item)); // Analyzer error: [CaptureNone] lambdas captures `this`
...
private U Process(T item) { ... }
```

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

- 使用与普通方法相同的特性语法，特性可应用于 lambda 或匿名方法的开头，例如：

```csharp
[SomeAttribute(...)] () => { ... }
[SomeAttribute(...)] delegate (int i) { ... }
```

- 若要避免对某个特性是应用于 lambda 方法还是应用于其中一个参数的歧义，只能在将括号用于任何参数时使用特性，例如：

```csharp
[SomeAttribute] i => { ... } // ERROR
[SomeAttribute] (i) => { ... } // Ok
[SomeAttribute] (int i) => { ... } // Ok
```

- 对于匿名方法，在 `delegate` 关键字之前，无需将属性应用于方法，例如：

```csharp
[SomeAttribute] delegate { ... } // Ok
[SomeAttribute] delegate (int i) => { ... } // Ok
```

- 可以应用多个属性，无论是通过标准的逗号分隔语法，还是通过完整属性语法，例如：

```csharp
[FirstAttribute, SecondAttribute] (i) => { ... } // Ok
[FirstAttribute] [SecondAttribute] (i) => { .... } // Ok
```

- 特性可应用于匿名方法或 lambda 的参数，但仅当使用括号时，才可用于任何参数，例如：

```csharp
[SomeAttribute] i => { ... } // ERROR
([SomeAttribute] i) => { .... } // Ok
([SomeAttribute] int i) => { ... } // Ok
([SomeAttribute] i, [SomeOtherAttribute] j) => { ... } // Ok
```

- 使用逗号分隔或完整属性语法，可以将多个属性应用于匿名方法或 lambda 的参数，例如：

```csharp
([FirstAttribute, SecondAttribute] i) => { ... } // Ok
([FirstAttribute] [SecondAttribute] i) => { ... } // Ok
```

- `return`目标特性还可用于 lambda，例如：

```csharp
([return: SomeAttribute] (i) => { ... }) // Ok
```

- 编译器会将属性输出到生成的方法，将参数输出到这些方法，就像对任何其他方法一样。

## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

不适用

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

不适用

## <a name="unresolved-questions"></a>未解决的问题
[unresolved]: #unresolved-questions

不适用

## <a name="design-meetings"></a>设计会议

不适用