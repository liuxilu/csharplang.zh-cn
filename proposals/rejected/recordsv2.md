---
ms.openlocfilehash: 5636157ba54e93587847d6f2f7aac2dc675f3112
ms.sourcegitcommit: ae114131069ca76e4a1ec8149b7bab81fce8965c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/30/2020
ms.locfileid: "84228183"
---

# <a name="records-v2"></a>记录 v2

过去，我们已将记录看作一项功能，以便能够处理数据。

"处理数据" 是包含很多方面的大型组，因此，每个方面都可能会有兴趣。 首先，让我们看一看今天的记录及其一些缺点。

例如，按如下所示定义一条简单记录

```C#
public class UserInfo
{
    public string Username { get; set; }
    public string Email { get; set; }
    public bool IsAdmin  { get; set; } = false;
}
```

使用情况将读取

```C#
void M()
{
    var userInfo = new UserInfo() 
    {
        Username = "andy",
        Email = "angocke@microsoft.com",
        IsAdmin = true
    };
}
```

此代码有明显的优点：

1. 定义是版本复原的，可以轻松地添加或移动属性
2. 可以按任意顺序设置属性，并且初始化中的名称始终与访问器匹配
3. 只能跳过具有默认值的属性

第一个缺陷是属性现在必须是可变的。

# <a name="mutability"></a>可变性

对于 c #，我们想要提供一种方法来设置 `readonly` 对象初始值设定项中的成员。
由于某些类型可能尚未设计此初始化，因此，我们也喜欢选择加入。

建议的解决方案是新的修饰符， `initonly` 可应用于属性和字段：

```C#
public class UserInfo
{
    public initonly string Username { get; }
    public initonly string Email { get; }
    public initonly bool IsAdmin { get; } = false;
}
```

这是 codegen 的：我们只是设置了只读字段。
具体而言，降低的属性如下所示：

```C#
public class UserInfo
{
    private readonly string <Backing>_username;
    public string get_Username() => <Backing>_username;
    [return: modreq(initonly)]
    public void set_Username(string value) { <Backing>_username = value; }
    ...
}
```

CLR 认为将 readonly 字段设置为不可验证，但不是安全的。 若要支持更高级的验证程序，建议使用以下规则：只读字段只能在方法内修改 `initonly` ，或在 CLR 堆栈上的新对象上进行修改，并且尚未通过存储或方法调用发布。

这应该解决示例中可变性的许多问题 `UserInfo` ，而不需要复杂的或脆弱的发出策略。 但这种情况不会带来新的问题：轻松地使用更改构造对象。

# <a name="with-ing"></a>与-ing

使用不永久性进行编程时，通过使用更改构造副本来完成对对象的更改，而不是直接对对象进行更改。 遗憾的是，即使对于当前样式的记录，也不能方便地在 c # 中执行此操作。 之前已建议为实现该功能的记录提供某种类型的自动生成方法。 如果有这样一种机制，则必须与成员一起使用 `initonly` 。 为此，建议添加一个 `with` 表达式，这与对象初始值设定项类似。 示例用法如下所示：

```C#
var userInfo = new UserInfo() 
{
    Username = "andy",
    Email = "angocke@microsoft.com",
    IsAdmin = true
};
var newUserName = userInfo with { Username = "angocke" };
```

生成的 `newUserName` 对象将是的副本 `userInfo` ，并 `Username` 将设置为 "angocke"。
表达式上的 codegen `with` 也与对象初始值设定项类似：构造一个新的对象，然后 `initonly` `Username` 在方法体中调用 setter。

当然，两者的区别在于，要构造的新对象不是简单的新对象创建，它是原始对象的副本。 为了提供此功能，我们要求对象提供提供重复对象的 "With 构造函数"。 示例 `With` 构造函数应如下所示：

```C#
class UserInfo
{
    ...
    [WithConstructor] // placeholder syntax, up for debate
    public UserInfo With()
    {
        return new UserInfo() { Username = this.Username, Email = this.Email, IsAdmin = this.IsAdmin };
    }
}
```

特别要注意的 `with` 是，表达式将设置 `initonly` 成员，与对象初始值设定项一样，因此，为了支持验证，必须确保在设置成员之前不能发布该对象 `initonly` 。 若要强制执行此 `WithConstructor` 操作，属性（或等效语法）将对方法强制实施新规则：所有 return 语句都必须直接包含对象创建表达式，可能使用对象初始值设定项。

如果 `With` 构造函数要求验证，则用户可能会引入用于执行该验证的构造函数，例如

```C#
class UserInfo
{
    ...
    private UserInfo(UserInfo original)
    {
        // validation code
    }
    [WithConstructor]
    public UserInfo With() => new UserInfo(this);
}
```

与关联的最后一种复杂性 `With` 是继承。 如果记录可扩展，则需要为子类提供新 `With` 的。 可按如下所示实现此目的：

```C#
class Base
{
    ...
    protected Base(Base original)
    {
        // validation
    }
    [WithConstructor]
    public virtual Base With() => new Base(this);
}
class Derived : Base
{
    ...
    protected Derived(Derived original)
    : base(original)
    {
        // validation
    }
    [WithConstructor]
    public override Derived With() => new Derived(this);
}
```

请注意，此处有一个额外的复杂性：为了 `With` 用派生类型重写构造函数，语言还需要支持重写中的协变返回类型。
[此处](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md)已经有了此功能的单独建议。

**弊端**

- 使中的所有 return 语句 `WithConstructor` 包含新的对象表达式具有限制性。
  这可能会被流分析缓解，这可确保新对象不会转义方法
- 通过编译器技巧支持重写中的变体将需要存根方法，这会将几率与继承深度一起增长。 存根方法的需要是因为运行时要求覆盖签名完全匹配。 如果运行时要求放松，则根本不需要存根方法。
- 使用窗体的链接构造函数可以 `Type(Type original)` 有效地保留该构造函数，以供使用。 由于构造函数具有唯一语义并且不能重新命名，因此这可能会受到限制。


## <a name="wrapping-it-all-up-records"></a>全部包装：记录

上述功能实现了之前非常困难的编程样式。 但即使是在新功能中，它也可能会非常冗长，并容易出错。 还有一些项（如 Equals 和 GetHashCode）现在已经编写，只是太费力了。
此外，在这两个新基元之上实现相等性的一个重要缺陷是，结构相等性是指在添加新数据时，应随数据类型一起更改的内容，但当手动处理数据时，可能会导致这些内容不同步。

因此，建议使用 c # 来支持记录的新语法，而不是提供新功能，而是设置默认值和生成设计为在记录中使用的代码。 示例语法如下所示

```C#
data class UserInfo
{
    public string Username { get; }
    public string Email { get; }
    public bool IsAdmin { get; } = false;
}
```

此类生成的代码会将所有公共字段和自动属性视为记录的结构成员。 可以使用可 `RecordMember(bool)` 用于包括或排除成员的新属性自定义记录成员。 `initonly`默认情况下，记录成员将基于记录成员自动生成类。 在任何时候，只需在源中声明这些成员的行为即可进行自定义。 用户编写的实现将替换所有模式用法中的默认实现。

请注意，继承面临的相等性是很复杂的，但似乎已在[其他记录建议](records.md)中充分解决。

## <a name="primary-constructors"></a>主构造函数

上一条记录提议还为类型本身中的参数列表提供了一个新语法，例如

```C#
class Point(int X, int Y);
```

在新设计中，参数列表为正交 c # 功能，可与记录完全集成。 如果记录中包含了主构造函数，则它将具有新的默认值，就像公共字段和自动属性一样：主构造函数中的参数将用于生成具有相同名称的公共记录成员属性。 此外，主构造函数现在可用于自动生成 deconstructor。

例如，具有主构造函数的以下记录

```C#
data class Point(int X, int Y);
```

将等效于

```C#
data class Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }
}
```

最后一代将是

```C#
class Point
{
    public initonly int X { get; }
    public initonly int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    protected Point(Point other)
    : this(other.X, other.Y)
    { }

    [WithConstructor]
    public virtual Point With() => new Point(this);

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }

    // Generated equality
}
```

请注意，我们使用主构造函数为数据类创建了一个其他信息片段：而不是在生成的受保护的构造函数中设置主字段，而是委托给主构造函数。 如果 Point 类有其他非主要记录成员，例如

```C#
data class Point(int X, int Y)
{
    public int Z { get; }
}
```

然后，会更改生成的受保护的构造函数，如下所示：

```C#
class Point
{
    // ...
    protected Point(Point other)
    : this(other.X, other.Y)
    {
        Z = other.Z;
    }
    // ...
}
```

特别要注意的是，这并不回答与主构造函数的记录继承相关的操作。 例如，

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A;
```

更明确的方法可能要求使用基列表提供参数列表，而不是采用任意方式解析，例如

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A(X, Y);
```

然后，基列表中的参数列表将应用到生成的 `base` 主构造函数中的调用：

```C#
class B
{
    // ..
    public B(int x, int y, int z)
    : base(x, y)
    // ..
}
```

对于主构造函数在记录外的含义，仍可以进一步提出建议。
