---
ms.openlocfilehash: b8d975a8fc95af6980feaae6be35160646fe2cd2
ms.sourcegitcommit: 32abf01f2e43be29114bfcf8f8ed1cc4e3eaded2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2019
ms.locfileid: "79484044"
---
# <a name="permit-stackalloc-in-nested-contexts"></a>允许在嵌套上下文中 `stackalloc`

### <a name="stack-allocation"></a>堆栈分配

我们修改C#语言规范的部分[*堆栈分配*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation)，以便在出现 `stackalloc` 表达式时放松位置。 我们删除

``` antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

并将其替换为

``` antlr
primary_no_array_creation_expression
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression? ']' array_initializer?
    | 'stackalloc' '[' expression? ']' array_initializer
    ;
```

请注意，将*array_initializer*添加到*stackalloc_initializer* （并使索引表达式可选）是[7.3 中C#的扩展](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md)，此处未进行描述。

`stackalloc` 表达式的*元素类型*是在 stackalloc 表达式中命名的*unmanaged_type* （如果有），否则为*array_initializer*元素中的通用类型。

*元素类型*`K` *stackalloc_initializer*的类型取决于其语法上下文：
- 如果*stackalloc_initializer*直接显示为*local_variable_declaration*语句或*for_initializer*的*local_variable_initializer* ，则其类型为 `K*`。
- 否则，其类型为 `System.Span<K>`。

### <a name="stackalloc-conversion"></a>Stackalloc 转换

*Stackalloc 转换*是来自 expression 的新的内置隐式转换。 当 `K*`*stackalloc_initializer*的类型时，将从*stackalloc_initializer*到类型 `System.Span<K>`的隐式*stackalloc 转换*。
