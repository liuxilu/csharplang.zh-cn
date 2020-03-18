---
ms.openlocfilehash: 2532a24e867930d2f27614f19c77585dbce80dfa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483534"
---
# <a name="throw-expression"></a><span data-ttu-id="62ca0-101">Throw 表达式</span><span class="sxs-lookup"><span data-stu-id="62ca0-101">Throw expression</span></span>

<span data-ttu-id="62ca0-102">我们扩展了要包含的表达式窗体集</span><span class="sxs-lookup"><span data-stu-id="62ca0-102">We extend the set of expression forms to include</span></span>

```antlr
throw_expression
    : 'throw' null_coalescing_expression
    ;

null_coalescing_expression
    : throw_expression
    ;
```

<span data-ttu-id="62ca0-103">类型规则如下所示：</span><span class="sxs-lookup"><span data-stu-id="62ca0-103">The type rules are as follows:</span></span>

- <span data-ttu-id="62ca0-104">*Throw_expression*没有类型。</span><span class="sxs-lookup"><span data-stu-id="62ca0-104">A *throw_expression* has no type.</span></span>
- <span data-ttu-id="62ca0-105">*Throw_expression*可以通过隐式转换转换为每种类型。</span><span class="sxs-lookup"><span data-stu-id="62ca0-105">A *throw_expression* is convertible to every type by an implicit conversion.</span></span>

<span data-ttu-id="62ca0-106">*Throw 表达式*引发的值是通过计算 null_coalescing_expression 得出的值，该*null_coalescing_expression*必须表示从 `System.Exception` 派生的类类型 `System.Exception`的类类型的值，或者是具有其有效基类的 `System.Exception` （或子类）的类型参数类型的值。</span><span class="sxs-lookup"><span data-stu-id="62ca0-106">A *throw expression* throws the value produced by evaluating the *null_coalescing_expression*, which must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="62ca0-107">如果表达式的计算生成 `null`，则改为引发 `System.NullReferenceException`。</span><span class="sxs-lookup"><span data-stu-id="62ca0-107">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="62ca0-108">计算*引发表达式*时的行为与为[ *throw 语句*指定](../../spec/statements.md#the-throw-statement)的行为相同。</span><span class="sxs-lookup"><span data-stu-id="62ca0-108">The behavior at runtime of the evaluation of a *throw expression* is the same [as specified for a *throw statement*](../../spec/statements.md#the-throw-statement).</span></span>

<span data-ttu-id="62ca0-109">流分析规则如下所示：</span><span class="sxs-lookup"><span data-stu-id="62ca0-109">The flow-analysis rules are as follows:</span></span>

- <span data-ttu-id="62ca0-110">对于每个*变量 v*，都将在*throw_expression* iff 的*null_coalescing_expression*之前明确赋值*v* ， *throw_expression*之前，该变量已明确赋值。</span><span class="sxs-lookup"><span data-stu-id="62ca0-110">For every variable *v*, *v* is definitely assigned before the *null_coalescing_expression* of a *throw_expression* iff it is definitely assigned before the *throw_expression*.</span></span>
- <span data-ttu-id="62ca0-111">对于每个变量*v*， *v*在*throw_expression*后都是明确赋值。</span><span class="sxs-lookup"><span data-stu-id="62ca0-111">For every variable *v*, *v* is definitely assigned after *throw_expression*.</span></span>

<span data-ttu-id="62ca0-112">仅在以下语法上下文中允许*引发表达式*：</span><span class="sxs-lookup"><span data-stu-id="62ca0-112">A *throw expression* is permitted in only the following syntactic contexts:</span></span>
- <span data-ttu-id="62ca0-113">作为三元条件运算符的第二个或第三个操作数 `?:`</span><span class="sxs-lookup"><span data-stu-id="62ca0-113">As the second or third operand of a ternary conditional operator `?:`</span></span>
- <span data-ttu-id="62ca0-114">作为 null 合并运算符的第二个操作数 `??`</span><span class="sxs-lookup"><span data-stu-id="62ca0-114">As the second operand of a null coalescing operator `??`</span></span>
- <span data-ttu-id="62ca0-115">作为 expression-bodied lambda 或方法的主体。</span><span class="sxs-lookup"><span data-stu-id="62ca0-115">As the body of an expression-bodied lambda or method.</span></span>
