---
ms.openlocfilehash: 63dfdfee9ea6c16e162f483aa1298feed297daef
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483666"
---
# <a name="conditional-ref-expressions"></a>条件 ref 表达式

当前不能在中C#使用将 ref 变量绑定到一个或另一个表达式的模式。

典型的解决方法是引入如下方法：

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

请注意，这不会完全替换三元，因为所有参数都必须在调用站点进行计算。

以下操作不会按预期方式工作：

```csharp
       // will crash with NRE because 'arr[0]' will be executed unconditionally
      ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

建议的语法如下所示：

```csharp
     <condition> ? ref <consequence> : ref <alternative>;
```

可以使用 ref 三元_正确_编写以上带有 "Choice" 的尝试，如下所示：

```csharp
     ref var r = ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

与选择的不同之处在于，可通过_真正_的条件方式访问结果和替代表达式，因此，如果 ```arr == null```

三元 ref 只是一种三元，其中，替代项和结果都是 refs。 这自然会要求结果/备用操作数为左值。 它还要求结果和替代类型的类型相互转换。

表达式的类型的计算方式与常规三元的类型类似。 I.E. 在这种情况下，如果 "结果" 和 "替代" 具有转换标识，但类型不同，则将应用现有的类型合并规则。

将从条件操作数中放心地假定安全返回。 如果任何一个不安全，则返回全部内容不安全。

Ref 三元是左值，因此它可以通过引用传递/赋值/返回;

```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

作为 LValue，还可以分配给。 

```csharp
    // assign to
    (arr != null ? ref arr[0]: ref otherArr[0]) = 1;
```

Ref 三元也可以在常规（而非引用）上下文中使用。 尽管这不是很常见，但你也可以直接使用常规三元。

```csharp
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```


___

实现说明： 

实现的复杂性似乎是从中等到大的 bug 修复的大小。 -I. 开销不大。
我认为我们需要对语法或分析进行任何更改。
对于元数据或互操作不起作用。 此功能完全基于表达式。
对调试/PDB 不起任何作用
