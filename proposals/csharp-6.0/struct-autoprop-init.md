---
ms.openlocfilehash: ae8b583d82f99135e2ba731604cd1febff4fade7
ms.sourcegitcommit: 125539b88d970bb2924d17a997c47cc0c7b806fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/19/2020
ms.locfileid: "83566736"
---
# <a name="relaxed-rules-for-auto-properties-in-structs"></a><span data-ttu-id="1f64b-101">结构中自动属性的宽松规则</span><span class="sxs-lookup"><span data-stu-id="1f64b-101">Relaxed rules for auto-properties in structs</span></span> 

<span data-ttu-id="1f64b-102">在 c # 6.0 之前，你无法编写如下代码：</span><span class="sxs-lookup"><span data-stu-id="1f64b-102">Before C# 6.0 you could not write code like this:</span></span> 

```csharp
struct S 
{ 
    int X { get; set; } 
    int Y { get; set; } 
    public S(int x, int y) 
    { 
        this.X = x; 
        this.Y = y; 
    } 
} 
```

```none
error CS0188: The 'this' object cannot be used before all of its fields are assigned to
error CS0843: Backing field for automatically implemented property 'S.X' must be fully assigned before control is returned to the caller. Consider calling the default constructor from a constructor initializer. 
error CS0843: Backing field for automatically implemented property 'S.Y' must be fully assigned before control is returned to the caller. Consider calling the default constructor from a constructor initializer. 
```
 
<span data-ttu-id="1f64b-103">解决方法是调用默认构造函数：</span><span class="sxs-lookup"><span data-stu-id="1f64b-103">A workaround was to invoke the default constructor:</span></span> 

```csharp
struct S 
{ 
    int X { get; set; } 
    int Y { get; set; } 
    public S(int x, int y) : this() 
    { 
        this.X = x; 
        this.Y = y; 
    } 
} 
```

<span data-ttu-id="1f64b-104">或者将其初始化为默认值：</span><span class="sxs-lookup"><span data-stu-id="1f64b-104">or to initialize this to the default value:</span></span> 

```csharp
struct S 
{ 
    int X { get; set; } 
    int Y { get; set; } 
    public S(int x, int y) 
    { 
        this = default(S); 
        this.X = x; 
        this.Y = y; 
    } 
} 
```

<span data-ttu-id="1f64b-105">遗憾的是，这两种方法都有效禁用了给定构造函数实例字段的明确赋值分析。</span><span class="sxs-lookup"><span data-stu-id="1f64b-105">Unfortunately, both methods effectively disabled definite assignment analysis for instance fields for the given constructor.</span></span> 

<span data-ttu-id="1f64b-106">C # 6.0 通过跟踪实例自动属性的赋值（就好像它们是它们的支持字段的赋值）来放宽这些规则，因此本部分开头的代码现在有效。</span><span class="sxs-lookup"><span data-stu-id="1f64b-106">C# 6.0 relaxes these rules by tracking assignments to instance auto-properties as if they were assignments to their backing fields, so the code in the beginning of this section is now valid.</span></span> 
