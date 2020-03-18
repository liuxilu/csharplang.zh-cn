---
ms.openlocfilehash: c1a77d9337ecd16f5ea1c30d18f6422552b0dfb2
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483582"
---
# <a name="ref-local-reassignment"></a><span data-ttu-id="6f097-101">引用本地重新分配</span><span class="sxs-lookup"><span data-stu-id="6f097-101">Ref Local Reassignment</span></span>

<span data-ttu-id="6f097-102">在C# 7.3 中，我们添加了对重新绑定 ref 局部变量或 ref 参数的引用的支持。</span><span class="sxs-lookup"><span data-stu-id="6f097-102">In C# 7.3, we add support for rebinding the referent of a ref local variable or a ref parameter.</span></span>

<span data-ttu-id="6f097-103">我们将以下项添加到 `assignment_operator`集。</span><span class="sxs-lookup"><span data-stu-id="6f097-103">We add the following to the set of `assignment_operator`s.</span></span>

```antlr
assignment_operator
    : '=' 'ref'
    ;
```

<span data-ttu-id="6f097-104">`=ref` 运算符称为***ref 赋值运算符***。</span><span class="sxs-lookup"><span data-stu-id="6f097-104">The `=ref` operator is called the ***ref assignment operator***.</span></span> <span data-ttu-id="6f097-105">它不是*复合赋值运算符*。</span><span class="sxs-lookup"><span data-stu-id="6f097-105">It is not a *compound assignment operator*.</span></span> <span data-ttu-id="6f097-106">左操作数必须是绑定到 ref 局部变量的表达式、ref 参数（不是 `this`）或 out 参数。</span><span class="sxs-lookup"><span data-stu-id="6f097-106">The left operand must be an expression that binds to a ref local variable, a ref parameter (other than `this`), or an out parameter.</span></span> <span data-ttu-id="6f097-107">右操作数必须是生成左值的表达式，该表达式指定与左操作数类型相同的值。</span><span class="sxs-lookup"><span data-stu-id="6f097-107">The right operand must be an expression that yields an lvalue designating a value of the same type as the left operand.</span></span>

<span data-ttu-id="6f097-108">右操作数必须在 ref 赋值时明确赋值。</span><span class="sxs-lookup"><span data-stu-id="6f097-108">The right operand must be definitely assigned at the point of the ref assignment.</span></span>

<span data-ttu-id="6f097-109">当左操作数绑定到 `out` 参数时，如果未在 ref 赋值运算符的开头明确赋值 `out` 参数，则是错误的。</span><span class="sxs-lookup"><span data-stu-id="6f097-109">When the left operand binds to an `out` parameter, it is an error if that `out` parameter has not been definitely assigned at the beginning of the ref assignment operator.</span></span>

<span data-ttu-id="6f097-110">如果左操作数为可写引用（即它指定了除 `ref readonly` 本地或 `in` 参数以外的任何内容），则右操作数必须是可写的左值。</span><span class="sxs-lookup"><span data-stu-id="6f097-110">If the left operand is a writeable ref (i.e. it designates anything other than a `ref readonly` local or  `in` parameter), then the right operand must be a writeable lvalue.</span></span>

<span data-ttu-id="6f097-111">Ref 赋值运算符生成已分配类型的左值。</span><span class="sxs-lookup"><span data-stu-id="6f097-111">The ref assignment operator yields an lvalue of the assigned type.</span></span> <span data-ttu-id="6f097-112">如果左操作数可写（即不 `ref readonly` 或 `in`），则它是可写的。</span><span class="sxs-lookup"><span data-stu-id="6f097-112">It is writeable if the left operand is writeable (i.e. not `ref readonly` or `in`).</span></span>

<span data-ttu-id="6f097-113">此操作员的安全规则如下：</span><span class="sxs-lookup"><span data-stu-id="6f097-113">The safety rules for this operator are:</span></span>

- <span data-ttu-id="6f097-114">对于引用重新分配 `e1 = ref e2`，`e2` 的*ref 安全到转义符*的范围必须至少与 `e1`的*ref 安全对转义*。</span><span class="sxs-lookup"><span data-stu-id="6f097-114">For a ref reassignment `e1 = ref e2`, the *ref-safe-to-escape* of `e2` must be at least as wide a scope as the *ref-safe-to-escape* of `e1`.</span></span>

<span data-ttu-id="6f097-115">在中，引用*安全到转义*的定义在[安全性上适用于类似引用类型](../csharp-7.2/span-safety.md)</span><span class="sxs-lookup"><span data-stu-id="6f097-115">Where *ref-safe-to-escape* is defined in [Safety for ref-like types](../csharp-7.2/span-safety.md)</span></span>
