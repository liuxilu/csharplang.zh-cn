---
ms.openlocfilehash: 63dfdfee9ea6c16e162f483aa1298feed297daef
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483666"
---
# <a name="conditional-ref-expressions"></a><span data-ttu-id="b6b10-101">条件 ref 表达式</span><span class="sxs-lookup"><span data-stu-id="b6b10-101">Conditional ref expressions</span></span>

<span data-ttu-id="b6b10-102">当前不能在中C#使用将 ref 变量绑定到一个或另一个表达式的模式。</span><span class="sxs-lookup"><span data-stu-id="b6b10-102">The pattern of binding a ref variable to one or another expression conditionally is not currently expressible in C#.</span></span>

<span data-ttu-id="b6b10-103">典型的解决方法是引入如下方法：</span><span class="sxs-lookup"><span data-stu-id="b6b10-103">The typical workaround is to introduce a method like:</span></span>

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

<span data-ttu-id="b6b10-104">请注意，这不会完全替换三元，因为所有参数都必须在调用站点进行计算。</span><span class="sxs-lookup"><span data-stu-id="b6b10-104">Note that this is not an exact replacement of a ternary since all arguments must be evaluated at the call site.</span></span>

<span data-ttu-id="b6b10-105">以下操作不会按预期方式工作：</span><span class="sxs-lookup"><span data-stu-id="b6b10-105">The following will not work as expected:</span></span>

```csharp
       // will crash with NRE because 'arr[0]' will be executed unconditionally
      ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

<span data-ttu-id="b6b10-106">建议的语法如下所示：</span><span class="sxs-lookup"><span data-stu-id="b6b10-106">The proposed syntax would look like:</span></span>

```csharp
     <condition> ? ref <consequence> : ref <alternative>;
```

<span data-ttu-id="b6b10-107">可以使用 ref 三元_正确_编写以上带有 "Choice" 的尝试，如下所示：</span><span class="sxs-lookup"><span data-stu-id="b6b10-107">The above attempt with "Choice" can be _correctly_ written using ref ternary as:</span></span>

```csharp
     ref var r = ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="b6b10-108">与选择的不同之处在于，可通过_真正_的条件方式访问结果和替代表达式，因此，如果 ```arr == null```</span><span class="sxs-lookup"><span data-stu-id="b6b10-108">The difference from Choice is that consequence and alternative expressions are accessed in a _truly_ conditional manner, so we do not see a crash if ```arr == null```</span></span>

<span data-ttu-id="b6b10-109">三元 ref 只是一种三元，其中，替代项和结果都是 refs。</span><span class="sxs-lookup"><span data-stu-id="b6b10-109">The ternary ref is just a ternary where both alternative and consequence are refs.</span></span> <span data-ttu-id="b6b10-110">这自然会要求结果/备用操作数为左值。</span><span class="sxs-lookup"><span data-stu-id="b6b10-110">It will naturally require that consequence/alternative operands are LValues.</span></span> <span data-ttu-id="b6b10-111">它还要求结果和替代类型的类型相互转换。</span><span class="sxs-lookup"><span data-stu-id="b6b10-111">It will also require that consequence and alternative have types that are identity convertible to each other.</span></span>

<span data-ttu-id="b6b10-112">表达式的类型的计算方式与常规三元的类型类似。</span><span class="sxs-lookup"><span data-stu-id="b6b10-112">The type of the expression will be computed similarly to the one for the regular ternary.</span></span> <span data-ttu-id="b6b10-113">I.E.</span><span class="sxs-lookup"><span data-stu-id="b6b10-113">I.E.</span></span> <span data-ttu-id="b6b10-114">在这种情况下，如果 "结果" 和 "替代" 具有转换标识，但类型不同，则将应用现有的类型合并规则。</span><span class="sxs-lookup"><span data-stu-id="b6b10-114">in a case if consequence and alternative have identity convertible, but different types, the existing type-merging rules will apply.</span></span>

<span data-ttu-id="b6b10-115">将从条件操作数中放心地假定安全返回。</span><span class="sxs-lookup"><span data-stu-id="b6b10-115">Safe-to-return will be assumed conservatively from the conditional operands.</span></span> <span data-ttu-id="b6b10-116">如果任何一个不安全，则返回全部内容不安全。</span><span class="sxs-lookup"><span data-stu-id="b6b10-116">If either is unsafe to return the whole thing is unsafe to return.</span></span>

<span data-ttu-id="b6b10-117">Ref 三元是左值，因此它可以通过引用传递/赋值/返回;</span><span class="sxs-lookup"><span data-stu-id="b6b10-117">Ref ternary is an LValue and as such it can be passed/assigned/returned by reference;</span></span>

```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="b6b10-118">作为 LValue，还可以分配给。</span><span class="sxs-lookup"><span data-stu-id="b6b10-118">Being an LValue, it can also be assigned to.</span></span> 

```csharp
    // assign to
    (arr != null ? ref arr[0]: ref otherArr[0]) = 1;
```

<span data-ttu-id="b6b10-119">Ref 三元也可以在常规（而非引用）上下文中使用。</span><span class="sxs-lookup"><span data-stu-id="b6b10-119">Ref ternary can be used in a regular (not ref) context as well.</span></span> <span data-ttu-id="b6b10-120">尽管这不是很常见，但你也可以直接使用常规三元。</span><span class="sxs-lookup"><span data-stu-id="b6b10-120">Although it would not be common since you could as well just use a regular ternary.</span></span>

```csharp
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```


___

<span data-ttu-id="b6b10-121">实现说明：</span><span class="sxs-lookup"><span data-stu-id="b6b10-121">Implementation notes:</span></span> 

<span data-ttu-id="b6b10-122">实现的复杂性似乎是从中等到大的 bug 修复的大小。</span><span class="sxs-lookup"><span data-stu-id="b6b10-122">The complexity of the implementation would seem to be the size of a moderate-to-large bug fix.</span></span> <span data-ttu-id="b6b10-123">-I. 开销不大。</span><span class="sxs-lookup"><span data-stu-id="b6b10-123">- I.E not very expensive.</span></span>
<span data-ttu-id="b6b10-124">我认为我们需要对语法或分析进行任何更改。</span><span class="sxs-lookup"><span data-stu-id="b6b10-124">I do not think we need any changes to the syntax or parsing.</span></span>
<span data-ttu-id="b6b10-125">对于元数据或互操作不起作用。</span><span class="sxs-lookup"><span data-stu-id="b6b10-125">There is no effect on metadata or interop.</span></span> <span data-ttu-id="b6b10-126">此功能完全基于表达式。</span><span class="sxs-lookup"><span data-stu-id="b6b10-126">The feature is completely expression based.</span></span>
<span data-ttu-id="b6b10-127">对调试/PDB 不起任何作用</span><span class="sxs-lookup"><span data-stu-id="b6b10-127">No effect on debugging/PDB either</span></span>
