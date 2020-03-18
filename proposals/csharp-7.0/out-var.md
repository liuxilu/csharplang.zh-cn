---
ms.openlocfilehash: a0d80afc47e9f0073237db9b8d7a4f0b045c1b0b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483504"
---
# <a name="out-variable-declarations"></a><span data-ttu-id="5d13b-101">Out 变量声明</span><span class="sxs-lookup"><span data-stu-id="5d13b-101">Out variable declarations</span></span>

<span data-ttu-id="5d13b-102">*Out 变量声明*功能允许在将变量作为 `out` 参数传递的位置声明该变量。</span><span class="sxs-lookup"><span data-stu-id="5d13b-102">The *out variable declaration* feature enables a variable to be declared at the location that it is being passed as an `out` argument.</span></span>

```antlr
argument_value
    : 'out' type identifier
    | ...
    ;
```

<span data-ttu-id="5d13b-103">用这种方式声明的变量称为*out 变量*。</span><span class="sxs-lookup"><span data-stu-id="5d13b-103">A variable declared this way is called an *out variable*.</span></span> <span data-ttu-id="5d13b-104">您可以对变量的类型使用上下文关键字 `var`。</span><span class="sxs-lookup"><span data-stu-id="5d13b-104">You may use the contextual keyword `var` for the variable's type.</span></span> <span data-ttu-id="5d13b-105">作用域将与通过模式匹配引入的*模式变量*的作用域相同。</span><span class="sxs-lookup"><span data-stu-id="5d13b-105">The scope will be the same as for a *pattern-variable* introduced via pattern-matching.</span></span>

<span data-ttu-id="5d13b-106">根据语言规范（7.6.7 元素访问部分），元素访问的参数列表（索引表达式）不包含 ref 或 out 参数。</span><span class="sxs-lookup"><span data-stu-id="5d13b-106">According to Language Specification (section 7.6.7 Element access) the argument-list of an element-access (indexing expression) does not contain ref or out arguments.</span></span> <span data-ttu-id="5d13b-107">但编译器允许它们用于各种方案，例如，在接受 `out`的元数据中声明的索引器。</span><span class="sxs-lookup"><span data-stu-id="5d13b-107">However, they are permitted by the compiler for various scenarios, for example indexers declared in metadata that accept `out`.</span></span>

<span data-ttu-id="5d13b-108">在 argument_value 引入的本地变量的范围内，在其声明之前的文本位置引用该局部变量是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="5d13b-108">Within the scope of a local variable introduced by an argument_value, it is a compile-time error to refer to that local variable in a textual position that precedes its declaration.</span></span>

<span data-ttu-id="5d13b-109">在立即包含其声明的同一个参数列表中引用隐式类型化的（第8.5.1） out 变量也是错误的。</span><span class="sxs-lookup"><span data-stu-id="5d13b-109">It is also an error to reference an implicitly-typed (§8.5.1) out variable in the same argument list that immediately contains its declaration.</span></span>

<span data-ttu-id="5d13b-110">重载决策的修改方式如下：</span><span class="sxs-lookup"><span data-stu-id="5d13b-110">Overload resolution is modified as follows:</span></span>

<span data-ttu-id="5d13b-111">我们添加了一个新的转换：</span><span class="sxs-lookup"><span data-stu-id="5d13b-111">We add a new conversion:</span></span>

> <span data-ttu-id="5d13b-112">存在从隐式类型化的 out 变量声明到每个类型的*表达式的转换*。</span><span class="sxs-lookup"><span data-stu-id="5d13b-112">There is a *conversion from expression* from an implicitly-typed out variable declaration to every type.</span></span>

<span data-ttu-id="5d13b-113">又</span><span class="sxs-lookup"><span data-stu-id="5d13b-113">Also</span></span>

> <span data-ttu-id="5d13b-114">显式类型化出变量的类型是声明的类型。</span><span class="sxs-lookup"><span data-stu-id="5d13b-114">The type of an explicitly-typed out variable argument is the declared type.</span></span>

<span data-ttu-id="5d13b-115">and</span><span class="sxs-lookup"><span data-stu-id="5d13b-115">and</span></span>

> <span data-ttu-id="5d13b-116">隐式类型化出变量没有类型。</span><span class="sxs-lookup"><span data-stu-id="5d13b-116">An implicitly-typed out variable argument has no type.</span></span>

<span data-ttu-id="5d13b-117">从隐式类型化出变量声明中的*表达式的转换*不会被视为比*从表达式*进行的任何其他转换都好。</span><span class="sxs-lookup"><span data-stu-id="5d13b-117">The *conversion from expression* from an implicitly-typed out variable declaration is not considered better than any other *conversion from expression*.</span></span>

<span data-ttu-id="5d13b-118">隐式类型化 out 变量的类型是重载决策选择的方法签名中的相应参数的类型。</span><span class="sxs-lookup"><span data-stu-id="5d13b-118">The type of an implicitly-typed out variable is the type of the corresponding parameter in the signature of the method selected by overload resolution.</span></span>

<span data-ttu-id="5d13b-119">添加了新的语法节点 `DeclarationExpressionSyntax` 以表示 out var 参数中的声明。</span><span class="sxs-lookup"><span data-stu-id="5d13b-119">The new syntax node `DeclarationExpressionSyntax` is added to represent the declaration in an out var argument.</span></span>
