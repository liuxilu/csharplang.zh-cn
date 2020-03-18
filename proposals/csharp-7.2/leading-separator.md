---
ms.openlocfilehash: 6ec94aaabb2c52393a87ee450dbae972b6a50bd5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483678"
---
# <a name="allow-digit-separator-after-0b-or-0x"></a><span data-ttu-id="f49a3-101">在0b 或0x 后允许数字分隔符</span><span class="sxs-lookup"><span data-stu-id="f49a3-101">Allow digit separator after 0b or 0x</span></span>

<span data-ttu-id="f49a3-102">在C# 7.2 中，我们扩展了数字分隔符（下划线字符）可出现在整数文本中的位置集。</span><span class="sxs-lookup"><span data-stu-id="f49a3-102">In C# 7.2, we extend the set of places that digit separators (the underscore character) can appear in integral literals.</span></span> <span data-ttu-id="f49a3-103">[从C# 7.0 开始，文本的数字之间允许使用分隔符](../csharp-7.0/digit-separators.md)。</span><span class="sxs-lookup"><span data-stu-id="f49a3-103">[Beginning in C# 7.0, separators are permitted between the digits of a literal](../csharp-7.0/digit-separators.md).</span></span> <span data-ttu-id="f49a3-104">现在，在C# 7.2 中，我们还允许在二进制或十六进制文本的第一个有效位之前，在前缀后允许使用数字分隔符。</span><span class="sxs-lookup"><span data-stu-id="f49a3-104">Now, in C# 7.2, we also permit digit separators before the first significant digit of a binary or hexadecimal literal, after the prefix.</span></span>

```csharp
    123      // permitted in C# 1.0 and later
    1_2_3    // permitted in C# 7.0 and later
    0x1_2_3  // permitted in C# 7.0 and later
    0b101    // binary literals added in C# 7.0
    0b1_0_1  // permitted in C# 7.0 and later

    // in C# 7.2, _ is permitted after the `0x` or `0b`
    0x_1_2   // permitted in C# 7.2 and later
    0b_1_0_1 // permitted in C# 7.2 and later
```

<span data-ttu-id="f49a3-105">不允许十进制整数文本具有前导下划线。</span><span class="sxs-lookup"><span data-stu-id="f49a3-105">We do not permit a decimal integer literal to have a leading underscore.</span></span> <span data-ttu-id="f49a3-106">标记（如 `_123`）是一个标识符。</span><span class="sxs-lookup"><span data-stu-id="f49a3-106">A token such as `_123` is an identifier.</span></span>
