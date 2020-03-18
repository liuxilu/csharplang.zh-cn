---
ms.openlocfilehash: 6f05bbc1365e59d737103b586db9d4a242c6d306
ms.sourcegitcommit: e9afb74cc1abd56db93b4b50bd5e6765e27c1c5d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2019
ms.locfileid: "79483996"
---
# <a name="static-lambdas"></a>静态 lambda

## <a name="summary"></a>总结

支持禁止从封闭范围捕获状态的 lambda。

## <a name="motivation"></a>动机

避免意外捕获来自封闭上下文的状态。

## <a name="detailed-design"></a>详细设计

具有 `static` 的 lambda 无法从封闭范围中捕获状态。
因此，封闭范围中的局部变量、参数和 `this` 在 `static` lambda 中不可用。

`static` lambda 无法通过隐式或显式 `this` 或 `base` 引用引用实例成员。

`static` lambda 可以引用封闭范围 `static` 成员。

`static` lambda 可以引用来自封闭范围 `constant` 定义。

`static` lambda 中的 `nameof()` 可以引用局部变量、参数或 `this` 或从封闭范围中 `base`。

对于 `static` 和非`static` lambda，封闭范围中 `private` 成员的可访问性规则是相同的。

不保证是否在元数据中将 `static` lambda 定义作为 `static` 方法发出。 这留给了编译器实现进行优化。

非`static` 本地函数或 lambda 可以从封闭的 `static` lambda 捕获状态，但不能在封闭 `static` lambda 之外捕获状态。

`static` lambda 可以在表达式树中使用。

从有效程序的 lambda 中删除 `static` 修饰符不会更改程序的含义。
