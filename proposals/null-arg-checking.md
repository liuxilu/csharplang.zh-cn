---
ms.openlocfilehash: 76065293f652979ab395e131d657e44899c5a65b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483540"
---
# <a name="simplified-null-argument-checking"></a>简化的 Null 参数检查

## <a name="summary"></a>总结
此提议提供简化的语法，用于验证方法自变量不会 `null` 并相应地引发 `ArgumentNullException`。

## <a name="motivation"></a>动机
设计可以为 null 的引用类型的工作导致我们检查 `null` 参数验证所需的代码。 假设 NRT 不会影响代码执行开发人员仍必须在完全 `null` 清理的项目中添加 `if (arg is null) throw` 样板板代码。 这样，我们就可以使用语言中的参数 `null` 验证的最小语法来探究。 

尽管这 `null` 参数验证语法应经常与 NRT 配对，但建议完全独立于该建议。 语法可独立于 `#nullable` 指令使用。

## <a name="detailed-design"></a>详细设计 

### <a name="null-validation-parameter-syntax"></a>空验证参数语法
感叹号运算符 `!`可以放置在参数列表中的参数名之后，这将导致C#编译器发出此参数的标准 `null` 检查代码。 这称为 `null` 验证参数语法。 例如：

``` csharp
void M(string name!) {
    ...
}
```

将转换为：

``` csharp
void M(string name) {
    if (name is null) {
        throw new ArgumentNullException(nameof(name));
    }
    ...
}
```

生成的 `null` 检查将在任何开发人员在方法中编写代码之前发生。 当多个参数包含 `!` 运算符时，将按照声明参数的顺序执行检查。

``` csharp
void M(string p1, string p2) {
    if (p1 is null) {
        throw new ArgumentNullException(nameof(p1));
    }
    if (p2 is null) {
        throw new ArgumentNullException(nameof(p2));
    }
    ...
}
```

检查将专门用于引用相等性 `null`，而不会调用 `==` 或任何用户定义的运算符。 这也意味着，只能将 `!` 运算符添加到可以根据 `null`测试其类型是否相等的参数。 这意味着它不能用于其类型已知为值类型的参数。

``` csharp
// Error: Cannot use ! on parameters who types derive from System.ValueType
void G<T>(T arg!) where T : struct {

}
```

对于构造函数，将在构造函数中的任何其他代码之前发生 `null` 验证。 其中包括： 

- 链接到具有 `this` 或 `base` 的其他构造函数 
- 隐式出现在构造函数中的字段初始值设定项

例如：

``` csharp
class C {
    string field = GetString();
    C(string name!): this(name) {
        ...
    }
}
```

将大致转换为以下内容：

``` csharp
class C {
    C(string name)
        if (name is null) {
            throw new ArgumentNullException(nameof(name));
        }
        field = GetString();
        :this(name);
        ...
}
```

注意：这不是合法C#代码，而只是实现实现的近似值。 

`null` 验证参数语法在 lambda 参数列表上也是有效的。 即使在缺少括号的单个参数语法中，这也是有效的。

``` csharp
void G() {
    // An identity lambda which throws on a null input
    Func<string, string> s = x! => x;
}
```

语法在迭代器方法的参数上也是有效的。 与迭代器中的其他代码不同，`null` 验证会在调用迭代器方法时，而不是在遍历基础枚举器时进行。 这适用于传统迭代器或 `async` 迭代器。

``` csharp
class Iterators {
    IEnumerable<char> GetCharacters(string s!) {
        foreach (var c in s) {
            yield return c;
        }
    }

    void Use() {
        // The invocation of GetCharacters will throw
        IEnumerable<char> e = GetCharacters(null);
    }
}
```

`!` 运算符仅可用于具有关联方法体的参数列表。 这意味着不能在 `abstract` 方法、`interface`、`delegate` 或 `partial` 方法定义中使用它。

### <a name="extending-is-null"></a>扩展为 null
表达式 `is null` 的有效类型将进行扩展以包含不受约束的类型参数。 这将允许它填写检查 `null` 检查是否有效的所有类型的 `null` 的意图。 具体来说，就是不能知道其值类型的类型。 例如，约束为 `struct` 的类型形参不能与此语法一起使用。

``` csharp
void NullCheck<T1, T2>(T1 p1, T2 p2) where T2 : struct {
    // Okay: T1 could be a class or struct here.
    if (p1 is null) {
        ...
    }

    // Error 
    if (p2 is null) { 
        ...
    }
}
```

在类型参数上 `is null` 的行为将与今天 `== null` 相同。 在将类型参数实例化为值类型的情况下，代码将被评估为 `false`。 对于引用类型，代码将执行正确的 `is null` 检查。

### <a name="intersection-with-nullable-reference-types"></a>具有可以为 Null 的引用类型的交集
应用于名称的 `!` 运算符的任何参数将以可以为 null 的状态从 "未 `null`" 开始。 即使参数本身的类型可能 `null`，也是如此。 使用显式可以为 null 的类型（如 `string?`）或使用不受约束的类型参数可能出现这种情况。 

当参数的 `!` 语法与参数上的可为 null 的类型组合时，编译器将发出警告：

``` csharp
void WarnCase<T>(
    string? name!, // Warning: combining explicit null checking with a nullable type
    T value1 // Okay
)
```

## <a name="open-issues"></a>打开的问题
无

## <a name="considerations"></a>注意事项

### <a name="constructors"></a>构造函数
构造函数的代码生成意味着在当前从标准 `null` 验证转换为 `null` 验证参数语法（`!`）时，有一个小但可观察的行为更改。 `null` 签入标准验证在字段初始值设定项和任何 `base` 或 `this` 调用之后发生。 这意味着开发人员不一定要将100% 的 `null` 验证迁移到新的语法。 构造函数至少需要一些检查。

讨论完毕后，这种情况很不可能导致任何重大的采用问题。 `null` 检查在构造函数中的任何逻辑之前都要运行的逻辑更多。 如果发现了重要的兼容性问题，则可以重新访问。

### <a name="warning-when-mixing--and-"></a>混合时出现警告？ 与!
在将 `!` 语法应用于显式键入为可为 null 的类型的参数时，是否应发出警告。 在表面上，开发人员似乎是过程声明，但在某些情况下，类型层次结构可能会强制开发人员进入这种情况。 

请考虑在一系列程序集中使用以下类层次结构（假设所有项都是在启用 `null` 检查的情况下编译的）：

``` csharp
// Assembly1
abstract class C1 {
    protected abstract void M(object o); 
}

// Assembly2
abstract class C2 : C1 {

}

// Assembly3
abstract class C3 : C2 { 
    protected override void M(object o!) {
        ...
    }
}
```

此处的作者 `C3` 决定向参数 `o`添加 `null` 验证。 这完全与如何使用该功能有关。

现在，假设 Assembly2 的作者决定添加以下替代：

``` csharp
// Assembly2
abstract class C2 : C1 {
   protected override void M(object? o) { 
       ...
   }
}
```

这是可空引用类型允许的，因为它是合法的，使得协定对于输入位置更灵活。 通常，NRT 功能允许在参数/返回为空性上具有合理的 co/逆变。 不过，该语言根据最具体的替代（而不是原始声明）进行 co/逆变检查。 这意味着 Assembly3 的作者将收到一条有关不匹配 `o` 类型的警告，需要将签名更改为以下内容，以消除此类情况： 

``` csharp
// Assembly3
abstract class C3 : C2 { 
    protected override void M(object? o!) {
        ...
    }
}
```

此时，Assembly3 的作者有几个选择：

- 它们可以接受/禁止显示关于 `object?` 和 `object` 不匹配的警告。
- 它们可以接受/禁止显示关于 `object?` 和 `!` 不匹配的警告。
- 它们只需删除 `null` 验证检查（删除 `!` 并执行显式检查）

这是一个真实的方案，但现在这一理念是继续使用警告。 如果发现警告的发生频率比我们预测的更频繁，则可以在以后删除（相反）。

### <a name="implicit-property-setter-arguments"></a>隐式属性 setter 参数
参数的 `value` 参数是隐式的，并且不会出现在任何参数列表中。 这意味着它不能是此功能的目标。 可以扩展属性 setter 语法，使其包含参数列表，以便应用 `!` 运算符。 但这种方法会使 `null` 验证变得更简单。 因此，隐式 `value` 参数不会使用此功能。

## <a name="future-considerations"></a>未来的注意事项
无
