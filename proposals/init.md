---
ms.openlocfilehash: ebbdab6d121f3001ac34a953b3de09768cda6344
ms.sourcegitcommit: 3f177e90b12e39d4d28f8bb1064df81a8e5912ca
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "81726052"
---
<a name="init-only-members"></a>仅 Init 会员
=====

## <a name="summary"></a>总结
此建议将仅 init 属性的概念添加到 C# 中。 这些属性可以在对象创建点设置，但仅在对象创建完成后才`get`有效。 这允许在 C# 中建立更灵活的不可变模型。 

## <a name="motivation"></a>动机
自 1.0 以来，在 C# 中构建不可变数据的基本机制一直没有改变。 它们仍然存在：

1. 将字段声明`readonly`为 。
1. 声明仅包含`get`访问器的属性。

这些机制有效地允许构造不可变数据，但它们通过增加类型样板代码的成本，并选择此类类型，使其退出对象和集合初始化器等功能。 这意味着开发人员必须在易用性和不变性之间做出选择。

简单的不可变对象（如）`Point`需要两倍于声明类型的锅炉板代码来支持构造。 类型越大，锅炉板的成本越大：

```cs
struct Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int X, int Y)
    {
        this.X = x;
        this.Y = y;
    }
}
```

访问`init`器允许调用方在构造过程中更改成员，从而使不可变对象更加灵活。 这意味着对象的不可变属性可以参与对象初始化，从而不再需要类型中的所有构造函数样板。 该`Point`类型现在很简单：

```cs
struct Point
{
    public int X { get; init; }
    public int Y { get; init; }
}
```

然后，使用者可以使用对象初始化程序创建对象

```cs
var p = new Point() { X = 42, Y = 13 };
```

## <a name="detailed-design"></a>详细设计

### <a name="init-members"></a>init 成员
仅使用`init`访问器代替`set`访问器声明 init 属性：

```cs
class Student
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
}
```

在以下情况下，包含`init`访问器的实例属性被视为可设置的：

- 在对象初始化过程中
- 在包含或派生类型的实例构造函数中，在`this`或`base`
- 在任何属性`init`的检修器内，在`this`或`base`

本文档将`init`访问器设置为可设置的时间统称为对象的构造阶段。

这意味着类`Student`可以通过以下方式使用：

```cs
var s = new Student()
{
    FirstName = "Jared",
    LastName = "Parosns",
};
s.LastName = "Parsons"; // Error: LastName is not settable
```

访问器可设置`init`时的规则跨类型层次结构扩展。 如果成员是可访问的，并且已知对象处于构造阶段，则该成员是可设置的。 这特别允许以下事项：

```cs
class Base
{
    public bool Value { get; init; }
}

class Derived : Base
{
    Derived()
    {
        // Not allowed with get only properties but allowed with init
        Value = true;
    }
}

class Consumption
{
    void Example()
    {
        var d = new Derived() { Value = true; };
    }
}

```

在调用`init`访问器时，已知实例处于打开构造阶段。 因此，`init`除了普通`set`访问器可以执行的操作外，允许访问器执行以下操作：

1. 通过`init``this`或 调用其他访问器`base`
1. 分配`readonly`在同一类型上声明的字段

```cs
class Complex
{
    readonly int Field1;
    int Field2;
    int Prop1 { get; init ; }
    int Prop2
    {
        get => 42;
        init
        {
            Field1 = 13; // okay
            Field2 = 13; // okay
            Prop1 = 13; // okay
        }
    }
}
```

从`init`访问器分配`readonly`字段的能力仅限于与访问器类型相同的字段。 它不能用于分配`readonly`基类型的字段。 此规则可确保类型作者始终控制其类型的可变性行为。 不希望利用`init`的开发人员不能受到选择这样做的其他类型的影响：

```cs
class Base
{
    internal readonly int Field;
    internal int Property
    {
        get => Field;
        init => Field = value; // Okay
    }

    internal int OtherProperty { get; init; }
}

class Derived : Base
{
    internal readonly int DerivedField;
    internal int DerivedProperty
    {
        get => DerivedField;
        init
        {
            DerivedField = 42;  // Okay
            Property = 0;       // Okay
            Field = 13;         // Error Field is readonly
        }
    }

    public Derived()
    {
        Property = 42;  // Okay 
        Field = 13;     // Error Field is readonly
    }
}
```

在`init`虚拟属性中使用时，还必须将所有重写标记为`init`。 同样，不可能用 重写简单`set`。 `init`

```cs
class Base
{
    public virtual int Property { get; init; }
}

class C1 : Base
{
    public override int Property { get; init; }
}

class C2 : Base
{
    // Error: Property must have init to override Base.Property
    public override int Property { get; set; }
}
```

声明`interface`还可以通过以下模式参与`init`样式初始化：

```cs
interface IPerson
{
    string Name { get; init; }
}

class Init
{
    void M<T>() where T : IPerson, new()
    {
        var local = new T()
        {
            Name = "Jared"
        };
        local.Name = "Jraed"; // Error
    }
}
```

此功能的限制：
- `init`访问器只能在实例属性上使用
- 属性不能同时包含 和`init``set`访问器
- 如果基础具有 ，则属性的所有`init`重写都必须具有`init`。 此规则也适用于接口实现。

### <a name="metadata-encoding"></a>元数据编码 
属性`init`访问器将作为标准`set`访问器发出，返回类型标有 modreq `IsInitOnly`。 这是一种具有以下定义的新类型：

```cs
namespace System.Runtime.CompilerServices
{
    public sealed class IsInitOnly
    {
    }
}
```

编译器将按全名匹配类型。 没有要求它出现在核心库中。 如果此名称有多个类型，则编译器将按以下顺序绑定中断：

1. 正在编译的项目中定义的一个
1. 在核心利库中定义的那个

如果两者都不存在，则将发出类型歧义错误。

本期[中](https://github.com/dotnet/runtime/issues/34978)包括`IsInitOnly`的"设计"

## <a name="questions"></a>问题

### <a name="breaking-changes"></a>重大更改
如何编码此功能的主要枢轴点之一将归结为以下问题： 

> 替换为二进制分解更改`init``set`吗？

替换`init``set`属性并使属性完全可写，从来都不是对非虚拟属性的开源更改。 它只是展开可以写入属性的方案集。 唯一的行为是，这是否仍然是二进制突破更改。

如果我们想要对源和二进制兼容`init`更改`set`进行更改，那么它将迫使我们手在 modreq 与下面的属性决策，因为它将排除 modreqs 作为灵魂。 另一方面，如果这被视为一个不感兴趣，那么这将使modreq与属性决策的影响较小。

**分辨率**LDM 认为此方案并不令人信服。

### <a name="modreqs-vs-attributes"></a>Modreqs vs. 属性
属性访问器的`init`发出策略必须在在元数据期间发出时使用属性或 modreq 之间进行选择。 这些有不同的权衡，需要考虑。

使用 modreq 声明对属性集访问器进行说明意味着符合 CLI 的编译器将忽略访问器，除非它了解 modreq。 这意味着只有知道的`init`编译器才会读取成员。 不知道会忽略该`init`成员的编译器`set`，因此不会意外地将属性视为读/写。 

modreq 的缺点是`init`成为`set`访问器的二进制签名的一部分。 添加或删除`init`将破坏应用程序的二进制兼容性。

使用属性对`set`访问器进行说明意味着只有了解该属性的编译器才能知道限制对该属性的访问。 不知道的`init`编译器会将其视为一个简单的读取/写入属性并允许访问。

这似乎意味着这个决定是在以牺牲二进制兼容性为代价的额外安全性之间做出选择。 挖掘一点额外的安全性不完全是它看起来。 它无法防止出现以下情况：

1. 对成员`public`的反思
1. 使用`dynamic` 
1. 无法识别 modreq 的编译器

还应考虑，当我们完成 .NET 5 的 IL 验证规则时，`init`将是这些规则之一。 这意味着只需验证发出可验证 IL 的编译器，就会获得额外的强制。

将更新 .NET（C#、F# 和 VB）的主要语言以识别这些`init`访问器。 因此，这里唯一现实的情况是，当 C# 9`init`编译器发出属性，并且它们被较旧的工具集（如 C# 8、VB 15 等）看到时...C# 8. 这是考虑和权衡二进制兼容性的权衡。

**注意**此讨论主要适用于成员，不适用于字段。 虽然`init`字段被 LDM 拒绝，但它们在 modreq 与属性讨论中仍值得考虑。 字段`init`的特征是放宽了现有的限制`readonly`。 这意味着，如果我们将字段作为`readonly`属性发出，则没有使用该字段的旧编译器的风险，因为它们已经识别`readonly`。 因此，在此处使用 modreq 不会添加任何额外的保护。

**分辨率**该功能将使用 modreq 对属性`init`设置器进行编码。 引人注目的因素是（没有特别的顺序）：

* 希望阻止较旧的编译器违反`init`语义
* 希望在`virtual`声明中或`interface`同时添加或删除`init`源和二进制中断更改。

鉴于也没有显著的支持删除`init`为二进制兼容的更改，它作出了使用modreq直接前进的选择。

### <a name="init-vs-initonly"></a>init vs. 只
在我们的 LDM 会议上，有三种语法形式得到了重要的考虑：

```cs
// 1. Use init 
int Option1 { get; init; }
// 2. Use init set
int Option2 { get; init set; }
// 3. Use initonly
int Option3 { get; initonly; }
```

**分辨率**在 LDM 中，没有压倒性地青睐的语法。

备受关注的一点是，语法的选择将如何影响我们将来将成员作为一项常规功能`init`的能力。
选择选项 1 意味着将来很难定义具有`init`样式`get`方法的属性。 最终决定，如果我们决定在未来与一般`init`成员一起前进，我们可以允许`init`成为属性访问者列表中的修改者，以及 . `init set` 基本上，以下两个声明将是相同的。

```cs
int Property1 { get; init; }
int Property1 { get; init set; }
```

决定作为属性访问器列表中的独立访问`init`器前进。

### <a name="warn-on-failed-init"></a>警告失败
请考虑以下场景。 类型声明构造函数中`init`未设置的唯一成员。 构造对象的代码是否应收到警告，如果他们未能初始化该值？

此时，很明显，该字段永远不会被设置，因此与有关未能初始化`private`数据的警告有许多相似之处。 因此，警告似乎在这里会有一些价值？

不过，此警告有显著的缺点：
1. 它使更改为`readonly`的兼容性故事复杂化`init`。 
1. 它要求携带其他元数据来表示调用方需要初始化的成员。

此外，如果我们认为在迫使对象创建者被警告/错误有关特定字段的总体方案中有价值，那么这很可能作为一般功能有意义。 没有理由只局限于`init`成员。

**分辨率**字段和属性的`init`消耗不会发出警告。

LDM 希望就所需字段和属性的概念进行更广泛的讨论。 这可能导致我们回来重新考虑我们对`init`成员和验证的立场。

## <a name="allow-init-as-a-field-modifier"></a>允许以"以天为名"修改器
以同样的方式`init`也可以作为属性访问者，它也可以作为字段的指定，以赋予它们与`init`属性类似的行为。
这将允许在构造完成之前按类型、派生类型或对象初始化器分配字段。

```cs
class Student
{
    public init string FirstName;
    public init string LastName;
}

var s = new Student()
{
    FirstName = "Jarde",
    LastName = "Parsons",
}

s.FirstName = "Jared"; // Error FirstName is readonly
```

在元数据中，这些字段的标记方式与`readonly`字段相同，但使用附加属性或 modreq 来指示它们是`init`样式字段。 

**分辨率**LDM同意这一建议是健全的，但总体而言，情况感觉与属性脱节。 决定目前只处理`init`属性。 这具有适当的灵活性级别，`init`因为属性可以在属性的声明类型上更改`readonly`字段。 如果有重要的客户反馈证明方案是否有效，将重新考虑这一点。

### <a name="allow-init-as-a-type-modifier"></a>允许以类型修改器表示 init
`readonly`同样，`struct`修改器可以应用于 自动声明所有字段`readonly`为 ，唯一`init``struct`的修改器可以在 上声明或`class`自动将所有字段标记为 。 `init`
这意味着以下两种类型声明等效：

```cs
struct Point
{
    public init int X;
    public init int Y;
}

// vs. 

init struct Point
{
    public int X;
    public int Y;
}
```

**分辨率**此功能在这里太*可爱*了，`readonly struct`并且与它所基于的功能相冲突。 该`readonly struct`功能很简单，因为它适用于`readonly`所有成员：字段、方法等...该`init struct`功能仅适用于属性。 这实际上最终使用户感到困惑。 

鉴于这`init`仅在类型的某些方面有效，我们拒绝了将其用作类型修饰符的想法。

## <a name="considerations"></a>注意事项

### <a name="compatibility"></a>兼容性
该`init`功能设计为与现有的仅属性`get`兼容。 具体来说，它意味着一个完全附加的更改属性，这是`get`今天，但希望更多的弹性对象创建语义。

例如，请考虑以下类型：

```cs
class Name
{
    public string First { get; }
    public string Last { get; }

    public Name(string first, string last)
    {
        First = first;
        Last = last;
    }
}
```

添加到`init`这些属性是一个非突破性的更改：

```cs
class Name
{
    public string First { get; init; }
    public string Last { get; init; }

    public Name(string first, string last)
    {
        First = first;
        Last = last;
    }
}
```

### <a name="il-verification"></a>IL 验证
当 .NET Core 决定重新实施 IL 验证时，需要调整规则以考虑`init`成员。 这将需要包含在规则更改中，以便非对`readonly`数据的更改。

IL 验证规则需要分为两部分： 

1. 允许`init`成员设置`readonly`字段。
1. 确定何时可以`init`合法地调用成员。

第一种是对现有规则的简单调整。 IL 验证器可以被教导识别`init`成员，从那里它只需要考虑在这样的成员`readonly``this`中设置一个字段。

第二个规则更为复杂。 在对象初始化的简单情况下，规则是直截了当的。 当表达式的结果仍在堆栈`init`上时，`new`调用成员应该是合法的。 也就是说，直到该值存储在本地、数组元素或字段中，或作为参数传递给另一种方法，调用`init`成员仍然是合法的。 这可确保一旦`new`表达式的结果发布到命名标识符（而不是`this`），则调用`init`成员将不再合法。 

但更复杂的情况是，当我们混合`init`成员、对象初始化器和`await`时。 这可能会导致新创建的对象暂时被吊入状态机，从而放入字段中。

```cs
var student = new Student() 
{
    Name = await SomeMethod()
};
```

在这里，结果`new Student()`将作为字段在`Name`发生之前被分到状态机中。 编译器需要以 IL 验证器了解它们不可访问的方式标记这些提升字段，因此不违反`init`的预期语义。

### <a name="init-members"></a>init 成员
修改`init`器可以扩展到所有实例成员。 这将概括对象构造期间的概`init`念，并允许类型声明可在构造过程中分段的帮助器方法初始化`init`字段和属性。

此类成员将具有`init`访问器在此设计中所做的所有休息。 需求是值得怀疑的，但是，这可以安全地添加到未来的语言版本，以兼容的方式。

### <a name="generate-three-accessors"></a>生成三个访问器
属性的一`init`个潜在实现是完全`init`独立于`set`。 这意味着属性可能具有三个不同的访问器：`get`和`set``init`。

这具有允许使用 modreq 来强制正确性，同时保持二进制兼容性的潜在优势。 实现大致如下：

1. 如果存在`init` `set`，则始终发出访问器。 当开发人员未定义时，它只是对`set`的引用。 
1. 对象初始化器中的属性集将始终使用`init`（如果存在，但如果丢失则回退）。 `set`

这意味着开发人员始终可以安全地从属性中删除`init`。 

此设计的缺点是，只有在`init`始终发出时，才有用。 **always** `set` 语言不能知道是否`init`删除了过去，它必须假设它是，`init`因此必须始终发出。 这将导致大量的元数据扩展，并且根本不值得在这里的兼容性成本。
