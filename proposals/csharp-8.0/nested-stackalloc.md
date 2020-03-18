---
ms.openlocfilehash: b8d975a8fc95af6980feaae6be35160646fe2cd2
ms.sourcegitcommit: 32abf01f2e43be29114bfcf8f8ed1cc4e3eaded2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2019
ms.locfileid: "79484044"
---
# <a name="permit-stackalloc-in-nested-contexts"></a><span data-ttu-id="22288-101">允许在嵌套上下文中 `stackalloc`</span><span class="sxs-lookup"><span data-stu-id="22288-101">Permit `stackalloc` in nested contexts</span></span>

### <a name="stack-allocation"></a><span data-ttu-id="22288-102">堆栈分配</span><span class="sxs-lookup"><span data-stu-id="22288-102">Stack allocation</span></span>

<span data-ttu-id="22288-103">我们修改C#语言规范的部分[*堆栈分配*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation)，以便在出现 `stackalloc` 表达式时放松位置。</span><span class="sxs-lookup"><span data-stu-id="22288-103">We modify the section [*Stack allocation*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation) of the C# language specification to relax the places when a `stackalloc` expression may appear.</span></span> <span data-ttu-id="22288-104">我们删除</span><span class="sxs-lookup"><span data-stu-id="22288-104">We delete</span></span>

``` antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="22288-105">并将其替换为</span><span class="sxs-lookup"><span data-stu-id="22288-105">and replace them with</span></span>

``` antlr
primary_no_array_creation_expression
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression? ']' array_initializer?
    | 'stackalloc' '[' expression? ']' array_initializer
    ;
```

<span data-ttu-id="22288-106">请注意，将*array_initializer*添加到*stackalloc_initializer* （并使索引表达式可选）是[7.3 中C#的扩展](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md)，此处未进行描述。</span><span class="sxs-lookup"><span data-stu-id="22288-106">Note that the addition of an *array_initializer* to *stackalloc_initializer* (and making the index expression optional) was an [extension in C# 7.3](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md) and is not described here.</span></span>

<span data-ttu-id="22288-107">`stackalloc` 表达式的*元素类型*是在 stackalloc 表达式中命名的*unmanaged_type* （如果有），否则为*array_initializer*元素中的通用类型。</span><span class="sxs-lookup"><span data-stu-id="22288-107">The *element type* of the `stackalloc` expression is the *unmanaged_type* named in the stackalloc expression, if any, or the common type among the elements of the *array_initializer* otherwise.</span></span>

<span data-ttu-id="22288-108">*元素类型*`K` *stackalloc_initializer*的类型取决于其语法上下文：</span><span class="sxs-lookup"><span data-stu-id="22288-108">The type of the *stackalloc_initializer* with *element type* `K` depends on its syntactic context:</span></span>
- <span data-ttu-id="22288-109">如果*stackalloc_initializer*直接显示为*local_variable_declaration*语句或*for_initializer*的*local_variable_initializer* ，则其类型为 `K*`。</span><span class="sxs-lookup"><span data-stu-id="22288-109">If the *stackalloc_initializer* appears directly as the *local_variable_initializer* of a *local_variable_declaration* statement or a *for_initializer*, then its type is `K*`.</span></span>
- <span data-ttu-id="22288-110">否则，其类型为 `System.Span<K>`。</span><span class="sxs-lookup"><span data-stu-id="22288-110">Otherwise its type is `System.Span<K>`.</span></span>

### <a name="stackalloc-conversion"></a><span data-ttu-id="22288-111">Stackalloc 转换</span><span class="sxs-lookup"><span data-stu-id="22288-111">Stackalloc Conversion</span></span>

<span data-ttu-id="22288-112">*Stackalloc 转换*是来自 expression 的新的内置隐式转换。</span><span class="sxs-lookup"><span data-stu-id="22288-112">The *stackalloc conversion* is a new built-in implicit conversion from expression.</span></span> <span data-ttu-id="22288-113">当 `K*`*stackalloc_initializer*的类型时，将从*stackalloc_initializer*到类型 `System.Span<K>`的隐式*stackalloc 转换*。</span><span class="sxs-lookup"><span data-stu-id="22288-113">When the type of a *stackalloc_initializer* is `K*`, there is an implicit *stackalloc conversion* from the *stackalloc_initializer* to the type `System.Span<K>`.</span></span>
