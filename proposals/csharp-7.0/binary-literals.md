---
ms.openlocfilehash: 6f6c24e826e9fe9b9e8c97549add1029f00bcf60
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483432"
---
# <a name="binary-literals"></a>二进制文本

向C#和 VB 添加二进制文本的请求相对较为常见。 对于位掩码（例如标志枚举），这似乎真正有用，但它也只是为了便于教学。

二进制文本如下所示：

```csharp
int nineteen = 0b10011;
```

在语法和语义上，它们与十六进制文本相同，不同之处在于，使用 `b`/`B`，而不是 `x`/`X`，只需使用数字 `0` 和 `1`，并在 base 2 中进行解释，而不是16。

实现这些和对语言用户的概念性开销没有太大的代价。

## <a name="syntax"></a>语法

语法如下所示：

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
