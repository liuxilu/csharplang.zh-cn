---
ms.openlocfilehash: 91afbc3e3412049cd183c36c8035f1862c520413
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/05/2019
ms.locfileid: "79483972"
---
# <a name="pattern-based-using-and-using-declarations"></a>"基于模式使用" 和 "using 声明"

## <a name="summary"></a>总结

语言将添加两个新功能来围绕 `using` 语句，以便简化资源管理： `using` 应识别 `IDisposable` 的可释放模式，并向语言添加 `using` 声明。

## <a name="motivation"></a>动机

`using` 语句是当今资源管理的有效工具，但它需要很多工作。 具有多个要管理的资源的方法可以在语法上陷入一系列的 `using` 语句。 这种语法负担足以表明，大多数编码样式准则对此方案在大括号附近有一个例外。 

`using` 声明消除了这里的许多仪式，并C#与包含资源管理块的其他语言相同。 此外，基于模式的 `using` 允许开发人员展开可参与的类型集。 在许多情况下，无需创建只存在以允许值在 `using` 语句中使用的包装类型。 

借助这些功能，开发人员可简化和扩展可应用 `using` 的方案。

## <a name="detailed-design"></a>详细设计 

### <a name="using-declaration"></a>using 声明

语言将允许 `using` 添加到局部变量声明。 此类声明与在同一位置的 `using` 语句中声明变量的效果相同。

```csharp
if (...) 
{ 
   using FileStream f = new FileStream(@"C:\users\jaredpar\using.md");
   // statements
}

// Equivalent to 
if (...) 
{ 
   using (FileStream f = new FileStream(@"C:\users\jaredpar\using.md")) 
   {
    // statements
   }
}
```

`using` local 的生存期将扩展到声明它的范围的末尾。 然后，将按声明的反向顺序处理 `using` 局部变量。 

```csharp
{ 
    using var f1 = new FileStream("...");
    using var f2 = new FileStream("..."), f3 = new FileStream("...");
    ...
    // Dispose f3
    // Dispose f2 
    // Dispose f1
}
```

`using` 声明的人脸 `goto`或任何其他控制流构造没有任何限制。 相反，代码的行为与等效的 `using` 语句一样：

```csharp
{
    using var f1 = new FileStream("...");
  target:
    using var f2 = new FileStream("...");
    if (someCondition) 
    {
        // Causes f2 to be disposed but has no effect on f1
        goto target;
    }
}
```

在 `using` 本地声明中声明的局部变量将隐式为只读。 这与 `using` 语句中声明的局部变量的行为匹配。 

`using` 声明的语言语法如下：

```antlr
local-using-declaration:
  using type using-declarators

using-declarators:
  using-declarator
  using-declarators , using-declarator
  
using-declarator:
  identifier = expression
```

围绕 `using` 声明的限制：

- 不能直接显示在 `case` 标签内，而必须位于 `case` 标签内的块内。
- 不能显示为 `out` 变量声明的一部分。 
- 必须具有每个声明符的初始值设定项。
- 局部类型必须可隐式转换为 `IDisposable` 或满足 `using` 模式。

### <a name="pattern-based-using"></a>基于模式的使用

语言将添加可释放模式的概念：这是具有可访问的 `Dispose` 实例方法的类型。 适合可释放模式的类型可以参与 `using` 语句或声明，而无需实现 `IDisposable`。 

```csharp
class Resource
{ 
    public void Dispose() { ... }
}

using (var r = new Resource())
{
    // statements
}
```

这将允许开发人员在许多新方案中利用 `using`：

- `ref struct`：这些类型现在无法实现接口，因此无法参与 `using` 语句。
- 扩展方法将允许开发人员增加其他程序集中的类型以参与 `using` 语句。

如果某个类型可以隐式转换为 `IDisposable` 并且还适合于可释放模式，则 `IDisposable` 将是首选的。 尽管这采用相反的方法 `foreach` （模式优先于接口），但这是实现向后兼容性的必要条件。

与传统 `using` 语句相同的限制同样适用： `using` 中声明的局部变量是只读的，`null` 值不会导致引发异常，等等 .。。仅在调用 Dispose 之前，不会有强制转换到 `IDisposable` 这一生成代码：

```csharp
{
      Resource r = new Resource();
      try {
            // statements
      }
      finally {
            if (resource != null) resource.Dispose();
      }
}
```

为了适应可释放模式，`Dispose` 方法必须可访问、无参数且具有 `void` 返回类型。 没有其他限制。 这显式表示可在此处使用扩展方法。

## <a name="considerations"></a>注意事项

### <a name="case-labels-without-blocks"></a>不带块的 case 标签

`using declaration` 在 `case` 标签内直接是非法的，因为它的实际生存期很复杂。 一种可能的解决方案是在同一位置为其指定与 `out var` 相同的生存期。 这会被视为功能实现的额外复杂性，并使工作变得轻松（只需将块添加到 `case` 标签），并不是采用此路线的理由。

## <a name="future-expansions"></a>未来扩展

### <a name="fixed-locals"></a>固定局部变量

`fixed` 语句具有 `using` 语句的所有属性，这些属性可使 `using` 局部变量。 应考虑将此功能扩展到 `fixed` 局部变量。 生存期和顺序规则应该同样适用于 `using` 和 `fixed`。
