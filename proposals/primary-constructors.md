---
ms.openlocfilehash: 77c9ffda3a59cc5f3dcc3ec9848600c6c5e03eed
ms.sourcegitcommit: 27487fa0294f4cdb7e5f2478884149e05314fd8a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2019
ms.locfileid: "79483900"
---
# <a name="primary-constructors"></a>主构造函数

* [x] 建议
* [] 原型：未启动
* [] 实现：未启动
* [] 规范：未启动

## <a name="summary"></a>总结
[summary]: #summary

类可以具有参数列表，在它们执行时，它们的基类规范可以有参数列表。
主构造函数参数在整个类声明的范围内，如果它们是由函数成员或匿名函数捕获的，它们将作为私有字段存储在类中。

## <a name="motivation"></a>动机
[motivation]: #motivation

程序初始化代码中有很多样板。 通常情况下，会多次提到一段给定的数据 `x`：

- 作为专用字段 `_x`
- 作为参数 `x` 到构造函数
- 在中，从构造函数中的参数对字段的赋值 `_x = x;`
- 作为属性 `X`
- 在属性 setter 中 `x = value;`
- 在属性 getter 中 `return x;`

``` c#
class C
{
    private string _x;
    
    public C(string x)
    {
        _x = x;
    }
    public string X
    {
        get => _x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); _x = value; }
    }
}
```

对于不需要验证或计算的属性，可以使用自动属性减少麻烦，从而不必为属性声明显式支持字段。 但是，如果您的属性需要任何类型的逻辑，而不是自动属性提供的任何类型，则可以使用以上哪种方式。

主构造函数通过将构造函数参数直接放置在整个类的范围中来降低开销，并再次避免需要显式声明支持字段。 因此，上面的示例将变为：

``` c#
class C(string x)
{
    public string X
    {
        get => x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); x = value; }
    }
}
```

在此示例中，主构造函数会将 `x` 的命名实体数从三个减少到两个，即 `_x` 支持字段避免。 它消除了三个成员声明中的两个（仅保留属性声明本身），并减少了从八到五台的 `x`/`_x``X` /的总数量。


## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

类可以具有参数列表：

``` c#
public class C(int i, string s)
{
    ...
}
```

参数列表导致为类隐式声明构造函数，并且具有与类本身相同的可访问性。

``` c#
new C(5, "Hello");
```

主构造函数参数在整个类体内的范围内。 如果由函数成员或匿名函数捕获它们，则它们将作为私有字段存储在类中。 如果它们仅在初始化期间使用，则不会将它们存储在对象中。

``` c#
public class C(int i, string s)
{
    int[] a = new int[i]; // i not captured
    public int S => s;    // s captured
}
```

如果具有主构造函数的类具有一个基类规范，则它可以有一个参数列表。 这作为隐式声明的构造函数的 `base(...)` 初始值设定项的参数列表。 如果未提供自变量列表，则假定为空。

``` c#
public class C(int i, string s) : B(s)
{
    ...
}
```
类也可以具有显式定义的构造函数，但它们都必须使用 `this(...)` 初始值设定项。 这可确保在构造新的实例时始终调用主构造函数。

类体中的所有初始值设定项都将成为生成的构造函数中的赋值。 这意味着，与其他类不同，初始值设定项将在调用基构造函数*之后*（而不是在之前）运行。 此外，生成的类将包含用于初始化任何私有字段的分配，这些私有字段是为存储由成员捕获的主构造函数参数而生成的。 将这些成员重写为使用私有字段，而不是以类似于 lambda 表达式的闭包的方式使用该参数。 首先初始化生成的主字段，然后在类中以外观的顺序执行生成的初始化分配。

在上面的示例中，类声明的作用类似于重写如下：

``` c#
public class C : B
{
    public C(int i, string s) : base(s)
    {
        __s = s;        // store parameter s for captured use
        a = new int[i]; // initialize a
    }
    int __s; // generated field for capture of s
    
    int[] a;
    public int S => __s; // s replaced with captured __s
}
```

捕获与 lambda 表达式捕获本地变量的限制相同。 例如，主构造函数中允许 `ref` 和 `out` 参数，但无法捕获我的成员正文。


## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

按重要性排序。

* 提议使用的语法也已针对位置记录进行了建议。 如果我们需要这两项功能，则需要一些便利设施。 例如 建议对记录使用 `data` 修饰符。
* 构造对象的分配大小不太明显，因为编译器会根据类的完整文本来确定是否为主构造函数参数分配字段。 此风险类似于隐式捕获 lambda 表达式中的变量。
* 常见的方法（或意外模式）可能是捕获多个继承级别上的 "相同" 参数，因为它是在传递构造函数链时，而不是在基类上将其 allotting用于对象中的相同数据。 这非常类似于如今通过自动属性重写自动属性的风险。 
* 如以上所述，对于可能通常在构造函数主体中表示的其他逻辑，没有任何位置。 下面的 "主构造函数正文" 扩展可解决。
* 如建议，执行顺序语义与普通构造函数稍有不同。 这可能会被修正，但代价是某些扩展方案（特别是 "主构造函数主体"）的成本。
* 建议仅在可以将单个构造函数指定为主构造函数时才起作用。
* 无法对类和主构造函数进行单独的可访问性。 例如，公共构造函数都委托给一个需要的私有 "生成-全部" 构造函数。 如有必要，稍后可能会推荐语法。


## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

完整的位置记录可以是替代项，也可以与主构造函数共存，具体取决于具体的具体情况。 它们允许在更*少*的方案中使用*更多*的缩写。 这两者都可能很有用，但同时也可能会多余，除非它们可以彼此地彼此集成。


## <a name="possible-extensions"></a>可能的延伸
[extensions]: #possible-extensions

它们是核心提议的变体或补充，可能会将其与之结合，或在更高的阶段（如果认为有用）。

### <a name="primary-constructor-bodies"></a>主构造函数主体

构造函数本身通常包含参数验证逻辑，或不能表示为初始值设定项的其他重要初始化代码。

可以扩展主构造函数以允许语句块直接出现在类体中。 这些语句会在生成的构造函数中插入到它们在初始化赋值之间出现的位置，因此会执行与初始值设定项的交错。 例如：

``` c#
public class C(int i, string s) : B(s)
{
    {
        if (i < 0) throw new ArgumentOutOfRangeException(nameof(i));
    }
    int[] a = new int[i];
    public int S => s;
}
```

### <a name="initializer-fields-and-initializer-functions"></a>初始值设定项字段和初始值设定项函数

在具有主构造函数的类中，我们可以将没有可访问性修饰符的字段和方法声明视为更像局部变量和本地函数：

* 与主构造函数参数一样，"初始值设定项字段" 仅在函数成员中使用时才会捕获到实际的私有字段。
* 仅当主构造函数参数和初始值设定项字段在其他函数成员中使用时，才会将 "初始值设定项函数" 视为捕获它们。 如果未捕获，则可以以更好的方式生成它们，如本地函数。
* 与主构造函数参数一样，它们不能通过成员访问获得，只是简单名称。

这可以用于只与初始化相关的临时变量和 helper 函数：

``` c#
public class C(string s)
{
    int size = s.Length;             // not captured
    int[] Create() => new int[size]; // not captured
    int[] a = Create();
    ...
}
```

这可能太微妙，特别是由于缺少辅助功能修饰符只是 `private`。 

### <a name="initializer-statements"></a>初始值设定项语句

以上到扩展的根式组合只是直接允许在类体中使用语句。 此类语句与上面建议的交错构造函数体完全相同，只不过它们不需要包含在 `{ }`中。 为了充分利用这一点，"本地" 变量和帮助程序函数还需要在类的顶级以 "初始值设定项字段和初始值设定项函数" 中所述的方式来表达。


### <a name="member-access"></a>成员访问

核心建议将主构造函数参数视为只能称为简单名称的参数。 一种替代方法是允许将它们作为私有字段（即使用成员访问）进行引用，即使它们有时不作为字段生成也是*如此*。 这将允许在按本地变量进行隐藏时将其作为 `this.x` 引用，并从不同的实例中访问 `other.x`。

如果应用于 "初始值设定项字段和初始值设定项函数" 扩展，这也会降低与普通私有成员不同的程度。 唯一的区别是，如果仅在初始化过程中使用，编译器可以自由地从对象中 elide 它们。

