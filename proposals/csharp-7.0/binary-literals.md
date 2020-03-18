---
ms.openlocfilehash: 6f6c24e826e9fe9b9e8c97549add1029f00bcf60
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483432"
---
# <a name="binary-literals"></a><span data-ttu-id="568a8-101">二进制文本</span><span class="sxs-lookup"><span data-stu-id="568a8-101">Binary literals</span></span>

<span data-ttu-id="568a8-102">向C#和 VB 添加二进制文本的请求相对较为常见。</span><span class="sxs-lookup"><span data-stu-id="568a8-102">There’s a relatively common request to add binary literals to C# and VB.</span></span> <span data-ttu-id="568a8-103">对于位掩码（例如标志枚举），这似乎真正有用，但它也只是为了便于教学。</span><span class="sxs-lookup"><span data-stu-id="568a8-103">For bitmasks (e.g. flag enums) this seems genuinely useful, but it would also be great just for educational purposes.</span></span>

<span data-ttu-id="568a8-104">二进制文本如下所示：</span><span class="sxs-lookup"><span data-stu-id="568a8-104">Binary literals would look like this:</span></span>

```csharp
int nineteen = 0b10011;
```

<span data-ttu-id="568a8-105">在语法和语义上，它们与十六进制文本相同，不同之处在于，使用 `b`/`B`，而不是 `x`/`X`，只需使用数字 `0` 和 `1`，并在 base 2 中进行解释，而不是16。</span><span class="sxs-lookup"><span data-stu-id="568a8-105">Syntactically and semantically they are identical to hexadecimal literals, except for using `b`/`B` instead of `x`/`X`, having only digits `0` and `1` and being interpreted in base 2 instead of 16.</span></span>

<span data-ttu-id="568a8-106">实现这些和对语言用户的概念性开销没有太大的代价。</span><span class="sxs-lookup"><span data-stu-id="568a8-106">There’s little cost to implementing these, and little conceptual overhead to users of the language.</span></span>

## <a name="syntax"></a><span data-ttu-id="568a8-107">语法</span><span class="sxs-lookup"><span data-stu-id="568a8-107">Syntax</span></span>

<span data-ttu-id="568a8-108">语法如下所示：</span><span class="sxs-lookup"><span data-stu-id="568a8-108">The grammar would be as follows:</span></span>

```antlr
integer-literal:
    : ...
    | binary-integer-literal
    ;
binary-integer-literal:
    : `0b` binary-digits integer-type-suffix-opt
    | `0B` binary-digits integer-type-suffix-opt
    ;
binary-digits:
    : binary-digit
    | binary-digits binary-digit
    ;
binary-digit:
    : `0`
    | `1`
    ;
```
