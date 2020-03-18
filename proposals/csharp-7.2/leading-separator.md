---
ms.openlocfilehash: 6ec94aaabb2c52393a87ee450dbae972b6a50bd5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483678"
---
# <a name="allow-digit-separator-after-0b-or-0x"></a>在0b 或0x 后允许数字分隔符

在C# 7.2 中，我们扩展了数字分隔符（下划线字符）可出现在整数文本中的位置集。 [从C# 7.0 开始，文本的数字之间允许使用分隔符](../csharp-7.0/digit-separators.md)。 现在，在C# 7.2 中，我们还允许在二进制或十六进制文本的第一个有效位之前，在前缀后允许使用数字分隔符。

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

不允许十进制整数文本具有前导下划线。 标记（如 `_123`）是一个标识符。
