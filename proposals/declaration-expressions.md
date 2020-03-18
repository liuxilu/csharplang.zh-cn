---
ms.openlocfilehash: d2064ec1637e50962262c9380281abd5e1711402
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483564"
---
# <a name="declaration-expressions"></a><span data-ttu-id="376d1-101">声明表达式</span><span class="sxs-lookup"><span data-stu-id="376d1-101">Declaration expressions</span></span>

<span data-ttu-id="376d1-102">支持作为表达式的声明赋值。</span><span class="sxs-lookup"><span data-stu-id="376d1-102">Support declaration assignments as expressions.</span></span>

## <a name="motivation"></a><span data-ttu-id="376d1-103">动机</span><span class="sxs-lookup"><span data-stu-id="376d1-103">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="376d1-104">在更多情况下，允许在声明点进行初始化，简化代码，并允许使用 `var`。</span><span class="sxs-lookup"><span data-stu-id="376d1-104">Allow initialization at the point of declaration in more cases, simplifying code, and allowing `var` to be used.</span></span>

```csharp
SpecialType ReferenceType =>
    (var st = _type.SpecialType).IsValueType() ? SpecialType.None : st;
```

<span data-ttu-id="376d1-105">允许 `ref` 参数的声明，类似于 `out var`。</span><span class="sxs-lookup"><span data-stu-id="376d1-105">Allow declarations for `ref` arguments, similar to `out var`.</span></span>

```csharp
Convert(source, destination, ref List<Diagnostic> diagnostics = null);
```

## <a name="detailed-design"></a><span data-ttu-id="376d1-106">详细设计</span><span class="sxs-lookup"><span data-stu-id="376d1-106">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="376d1-107">表达式将扩展为包含声明赋值。</span><span class="sxs-lookup"><span data-stu-id="376d1-107">Expressions are extended to include declaration assignment.</span></span> <span data-ttu-id="376d1-108">优先级与赋值相同。</span><span class="sxs-lookup"><span data-stu-id="376d1-108">Precedence is the same as assignment.</span></span>

```antlr
expression
    : non_assignment_expression
    | assignment
    | declaration_assignment_expression // new
    ;
declaration_assignment_expression // new
    : declaration_expression '=' local_variable_initializer
    ;
declaration_expression // C# 7.0
    | type variable_designation
    ;
```

<span data-ttu-id="376d1-109">声明分配是一个本地的。</span><span class="sxs-lookup"><span data-stu-id="376d1-109">The declaration assignment is of a single local.</span></span>

<span data-ttu-id="376d1-110">声明赋值表达式的类型是声明的类型。</span><span class="sxs-lookup"><span data-stu-id="376d1-110">The type of a declaration assignment expression is the type of the declaration.</span></span>
<span data-ttu-id="376d1-111">如果类型 `var`，则推断出的类型是初始化表达式的类型。</span><span class="sxs-lookup"><span data-stu-id="376d1-111">If the type is `var`, the inferred type is the type of the initializing expression.</span></span> 

<span data-ttu-id="376d1-112">声明赋值表达式可以是左值，用于特定 `ref` 参数值。</span><span class="sxs-lookup"><span data-stu-id="376d1-112">The declaration assignment expression may be an l-value, for `ref` argument values in particular.</span></span>

<span data-ttu-id="376d1-113">如果声明赋值表达式声明一个值类型，并且该表达式为 r 值，则该表达式的值为副本。</span><span class="sxs-lookup"><span data-stu-id="376d1-113">If the declaration assignment expression declares a value type, and the expression is an r-value, the value of the expression is a copy.</span></span>

<span data-ttu-id="376d1-114">声明赋值表达式可以声明本地 `ref`。</span><span class="sxs-lookup"><span data-stu-id="376d1-114">The declaration assignment expression may declare a `ref` local.</span></span>
<span data-ttu-id="376d1-115">当 `ref` 用于 `ref` 参数中的声明表达式时，存在歧义。</span><span class="sxs-lookup"><span data-stu-id="376d1-115">There is an ambiguity when `ref` is used for a declaration expression in a `ref` argument.</span></span>
<span data-ttu-id="376d1-116">局部变量初始值设定项确定声明是否为本地 `ref`。</span><span class="sxs-lookup"><span data-stu-id="376d1-116">The local variable initializer determines whether the declaration is a `ref` local.</span></span>

```csharp
F(ref int x = IntFunc());    // int x;
F(ref int y = RefIntFunc()); // ref int y;
```

<span data-ttu-id="376d1-117">声明赋值表达式中声明的局部变量的作用域与 C # 7.0 中的相应声明表达式的作用域相同。</span><span class="sxs-lookup"><span data-stu-id="376d1-117">The scope of locals declared in declaration assignment expressions is the same the scope of corresponding declaration expressions from C#7.0.</span></span>

<span data-ttu-id="376d1-118">引用声明表达式前面的文本中的局部变量是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="376d1-118">It is a compile time error to refer to a local in text preceding the declaration expression.</span></span>

## <a name="alternatives"></a><span data-ttu-id="376d1-119">备选项</span><span class="sxs-lookup"><span data-stu-id="376d1-119">Alternatives</span></span>
[alternatives]: #alternatives
<span data-ttu-id="376d1-120">无更改。</span><span class="sxs-lookup"><span data-stu-id="376d1-120">No change.</span></span> <span data-ttu-id="376d1-121">此功能只是语法上的简写形式。</span><span class="sxs-lookup"><span data-stu-id="376d1-121">This feature is just syntactic shorthand after all.</span></span>

<span data-ttu-id="376d1-122">更多一般序列表达式：请参见[#377](https://github.com/dotnet/csharplang/issues/377)。</span><span class="sxs-lookup"><span data-stu-id="376d1-122">More general sequence expressions: see [#377](https://github.com/dotnet/csharplang/issues/377).</span></span>

<span data-ttu-id="376d1-123">若要允许在更多情况下使用 `var`，请允许 `var` 局部变量的单独声明和分配，并从所有代码路径的赋值推断类型。</span><span class="sxs-lookup"><span data-stu-id="376d1-123">To allow use of `var` in more cases, allow separate declaration and assignment of `var` locals, and infer the type from assignments from all code paths.</span></span>

## <a name="see-also"></a><span data-ttu-id="376d1-124">另请参阅</span><span class="sxs-lookup"><span data-stu-id="376d1-124">See also</span></span>
[see-also]: #see-also
<span data-ttu-id="376d1-125">请参阅[#595](https://github.com/dotnet/csharplang/issues/595)中的基本声明表达式。</span><span class="sxs-lookup"><span data-stu-id="376d1-125">See Basic Declaration Expression in [#595](https://github.com/dotnet/csharplang/issues/595).</span></span>

<span data-ttu-id="376d1-126">请参阅[析构](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md)功能中的析构声明。</span><span class="sxs-lookup"><span data-stu-id="376d1-126">See Deconstruction Declaration in the [deconstruction](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md) feature.</span></span>
