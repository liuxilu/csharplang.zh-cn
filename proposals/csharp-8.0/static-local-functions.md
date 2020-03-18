---
ms.openlocfilehash: fecd5a6c1e0f6c7a7a7beac0e4e60445281c7846
ms.sourcegitcommit: 1b488e76c2c07aafc377bc5e8a7197252c82f425
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "79484008"
---
# <a name="static-local-functions"></a>静态本地函数

## <a name="summary"></a>总结

支持禁止从封闭范围捕获状态的本地函数。

## <a name="motivation"></a>动机

避免意外捕获来自封闭上下文的状态。
允许在需要 `static` 方法的情况下使用本地函数。

## <a name="detailed-design"></a>详细设计

`static` 声明的本地函数无法从封闭范围捕获状态。
因此，封闭范围中的局部变量、参数和 `this` 在 `static` 本地函数中不可用。

`static` 局部函数不能通过隐式或显式 `this` 或 `base` 引用来引用实例成员。

`static` 本地函数可以引用封闭范围 `static` 成员。

`static` 本地函数可以从封闭范围中引用 `constant` 定义。

`static` 的本地函数中的 `nameof()` 可以引用局部变量、参数或 `this` 或从封闭范围中 `base`。

封闭范围中 `private` 成员的可访问性规则与 `static` 和非`static` 本地函数相同。

在元数据中作为 `static` 方法发出 `static` 本地函数定义，即使仅在委托中使用也是如此。

非`static` 本地函数或 lambda 可以从封闭 `static` 本地函数捕获状态，但不能在封闭 `static` 本地函数之外捕获状态。

不能在表达式树中调用 `static` 本地函数。

无论本地函数是否 `static`，都将以 `call` 而不是 `callvirt`的方式发出对本地函数的调用。

本地函数内的调用的重载解析不受 `static`本地函数的影响。

从有效程序的本地函数中删除 `static` 修饰符不会更改程序的含义。

## <a name="design-meetings"></a>设计会议

https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-09-10.md#static-local-functions
