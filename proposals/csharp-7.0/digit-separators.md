---
ms.openlocfilehash: 5476f4438ad79a26b3615154f789d8ed04cb61aa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483498"
---
# <a name="digit-separators"></a><span data-ttu-id="a47af-101">数字分隔符</span><span class="sxs-lookup"><span data-stu-id="a47af-101">Digit separators</span></span>

<span data-ttu-id="a47af-102">能够在大数值中对数字进行分组会给您带来很大的可读性影响，并且没有明显的缺点。</span><span class="sxs-lookup"><span data-stu-id="a47af-102">Being able to group digits in large numeric literals would have great readability impact and no significant downside.</span></span> 

<span data-ttu-id="a47af-103">添加二进制文本（#215）会增加数值文本的可能性，因此这两个功能互相增强。</span><span class="sxs-lookup"><span data-stu-id="a47af-103">Adding binary literals (#215) would increase the likelihood of numeric literals being long, so the two features enhance each other.</span></span> 

<span data-ttu-id="a47af-104">我们将遵循 Java 和其他方法，并使用下划线 `_` 作为数字分隔符。</span><span class="sxs-lookup"><span data-stu-id="a47af-104">We would follow Java and others, and use an underscore `_` as a digit separator.</span></span> <span data-ttu-id="a47af-105">它可以在数字文本中的任何位置发生（除了第一个字符和最后一个字符），因为不同的分组在不同方案中可能有意义，特别是对于不同的数字基准：</span><span class="sxs-lookup"><span data-stu-id="a47af-105">It would be able to occur everywhere in a numeric literal (except as the first and last character), since different groupings may make sense in different scenarios and especially for different numeric bases:</span></span>

```csharp
int bin = 0b1001_1010_0001_0100;
int hex = 0x1b_a0_44_fe;
int dec = 33_554_432;
int weird = 1_2__3___4____5_____6______7_______8________9;
double real = 1_000.111_1e-1_000;
```

<span data-ttu-id="a47af-106">任何数字序列都可以用下划线分隔，两个连续数字之间可能有多个下划线。</span><span class="sxs-lookup"><span data-stu-id="a47af-106">Any sequence of digits may be separated by underscores, possibly more than one underscore between two consecutive digits.</span></span> <span data-ttu-id="a47af-107">它们在小数和指数中是允许的，但在前面的规则后，它们可能不会出现在指数字符（`1.1e_1`）旁边或类型说明符（`10_f`）旁的小数点（`10_.0`）的旁边。</span><span class="sxs-lookup"><span data-stu-id="a47af-107">They are allowed in decimals as well as exponents, but following the previous rule, they may not appear next to the decimal (`10_.0`), next to the exponent character (`1.1e_1`), or next to the type specifier (`10_f`).</span></span> <span data-ttu-id="a47af-108">在二进制和十六进制文本中使用时，它们可能不会立即出现在 `0x` 或 `0b`之后。</span><span class="sxs-lookup"><span data-stu-id="a47af-108">When used in binary and hexadecimal literals, they may not appear immediately following the `0x` or `0b`.</span></span>

<span data-ttu-id="a47af-109">语法非常简单，分隔符没有语义影响，它们只是被忽略。</span><span class="sxs-lookup"><span data-stu-id="a47af-109">The syntax is straightforward, and the separators have no semantic impact - they are simply ignored.</span></span>

<span data-ttu-id="a47af-110">这有很大的价值，并且很容易实现。</span><span class="sxs-lookup"><span data-stu-id="a47af-110">This has broad value and is easy to implement.</span></span>
