---
ms.openlocfilehash: d2064ec1637e50962262c9380281abd5e1711402
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483564"
---
# <a name="declaration-expressions"></a>声明表达式

支持作为表达式的声明赋值。

## <a name="motivation"></a>动机
[motivation]: #motivation

在更多情况下，允许在声明点进行初始化，简化代码，并允许使用 `var`。

```csharp
SpecialType ReferenceType =>
    (var st = _type.SpecialType).IsValueType() ? SpecialType.None : st;
```

允许 `ref` 参数的声明，类似于 `out var`。

```csharp
Convert(source, destination, ref List<Diagnostic> diagnostics = null);
```

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

表达式将扩展为包含声明赋值。 优先级与赋值相同。

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

声明分配是一个本地的。

声明赋值表达式的类型是声明的类型。
如果类型 `var`，则推断出的类型是初始化表达式的类型。 

声明赋值表达式可以是左值，用于特定 `ref` 参数值。

如果声明赋值表达式声明一个值类型，并且该表达式为 r 值，则该表达式的值为副本。

声明赋值表达式可以声明本地 `ref`。
当 `ref` 用于 `ref` 参数中的声明表达式时，存在歧义。
局部变量初始值设定项确定声明是否为本地 `ref`。

```csharp
F(ref int x = IntFunc());    // int x;
F(ref int y = RefIntFunc()); // ref int y;
```

声明赋值表达式中声明的局部变量的作用域与 C # 7.0 中的相应声明表达式的作用域相同。

引用声明表达式前面的文本中的局部变量是编译时错误。

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives
无更改。 此功能只是语法上的简写形式。

更多一般序列表达式：请参见[#377](https://github.com/dotnet/csharplang/issues/377)。

若要允许在更多情况下使用 `var`，请允许 `var` 局部变量的单独声明和分配，并从所有代码路径的赋值推断类型。

## <a name="see-also"></a>另请参阅
[see-also]: #see-also
请参阅[#595](https://github.com/dotnet/csharplang/issues/595)中的基本声明表达式。

请参阅[析构](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md)功能中的析构声明。
