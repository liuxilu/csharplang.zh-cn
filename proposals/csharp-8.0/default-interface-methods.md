---
ms.openlocfilehash: b0d0fa70d90f7493c6c23be576155a77cec36cf8
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/14/2020
ms.locfileid: "79484080"
---
# <a name="default-interface-methods"></a>默认接口方法

* [x] 建议
* [] 原型：[正在进行](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)
* [] 实现：无
* [] 规范：正在进行，如下所示

## <a name="summary"></a>总结
[summary]: #summary

添加对_虚拟扩展方法_的支持-具有具体实现的接口中的方法。 实现此类接口的类或结构需要具有接口方法的单个_特定_实现，该实现由类或结构实现，或从其基类或接口继承。 利用虚拟扩展方法，API 作者可以在未来版本中将方法添加到接口，而不会对该接口的现有实现中断源或二进制文件的兼容性。

它们类似于 Java 的["默认方法"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)。

（基于可能的实现方法）此功能需要 CLI/CLR 中的相应支持。 利用此功能的程序不能在早期版本的平台上运行。

## <a name="motivation"></a>动机
[motivation]: #motivation

此功能的主要动机是

- 使用默认接口方法，API 作者可以在未来版本中将方法添加到接口，而不会对该接口的现有实现中断源或二进制文件的兼容性。
- 利用此功能C# ，可以与面向[Android （Java）](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)和[IOS （Swift）](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267)的 api 进行互操作，这种功能支持相似的功能。
- 事实证明，添加默认接口实现提供 "特征" 语言功能的元素（<https://en.wikipedia.org/wiki/Trait_(computer_programming)>）。 特性证明是一种功能强大的编程技术（<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>）。

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

接口的语法已扩展为允许

- 声明常量、运算符、静态构造函数和嵌套类型的成员声明;
- 方法或索引器、属性或事件访问器（即 "默认" 实现）的*主体*;
- 声明静态字段、方法、属性、索引器和事件的成员声明;
- 使用显式接口实现语法的成员声明;与
- 显式访问修饰符（`public`的默认访问权限）。

具有主体的成员允许接口为不提供重写实现的类和结构中的方法提供 "默认" 实现。

接口不能包含实例状态。 虽然现在允许使用静态字段，但接口中不允许使用实例字段。 接口中不支持实例自动属性，因为它们会隐式声明隐藏的字段。

静态方法和私有方法允许使用用于实现接口的公共 API 的代码的实用重构和组织。

接口中的方法重写必须使用显式接口实现语法。

在使用*variance_annotation*声明的类型形参的范围内声明类类型、结构类型或枚举类型是错误的。  例如，下面的 `C` 的声明是错误的。

```csharp
interface IOuter<out T>
{
    class C { } // error: class declaration within the scope of variant type parameter 'T'
}
```

### <a name="concrete-methods-in-interfaces"></a>接口中的具体方法

此功能的最简单形式是在接口中声明*具体方法*，该方法是具有主体的方法。

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
```

实现此接口的类不需要实现其具体方法。

```csharp
class C : IA { } // OK

IA i = new C();
i.M(); // prints "IA.M"
```

类 `C` 中 `IA.M` 的最终替代是 `M` 在 `IA`中声明的具体方法。 请注意，类不会从其接口继承成员;这不会被此功能更改：

```csharp
new C().M(); // error: class 'C' does not contain a member 'M'
```

在接口的实例成员内，`this` 具有封闭接口的类型。

### <a name="modifiers-in-interfaces"></a>接口中的修饰符

接口的语法非常宽松，允许其成员具有修饰符。 允许以下内容： `private`、`protected`、`internal`、`public`、`virtual`、`abstract`、`sealed`、`static`、`extern`和 `partial`。

> ***TODO***：检查其他修饰符是否存在。

除非使用 `sealed` 或 `private` 修饰符，否则，其声明中包含正文的接口成员是 `virtual` 成员。 `virtual` 修饰符可用于本来将隐式 `virtual`的函数成员。 同样，尽管 `abstract` 在没有主体的接口成员上是默认值，但也可以显式指定修饰符。 可以使用 `sealed` 关键字声明非虚拟成员。

如果接口的 `private` 或 `sealed` 函数成员没有正文，则是错误的。 `private` 函数成员不能 `sealed`修饰符。

访问修饰符可用于允许的所有种类成员的接口成员。 访问级别 `public` 是默认值，但它可以显式给定。

> ***打开问题：*** 需要指定访问修饰符（如 `protected` 和 `internal`）的精确含义，以及哪些声明执行和不重写它们（在派生接口中）或实现它们（在实现接口的类中）。

接口可以声明 `static` 成员，包括嵌套类型、方法、索引器、属性、事件和静态构造函数。 所有接口成员的默认访问级别为 `public`。

接口不能声明实例构造函数、析构函数或字段。

> ***已关闭问题：*** 是否允许在接口中声明运算符？ 可能不是转换运算符，但其他运算符怎么样呢？ ***决策***：除转换、相等和不等运算符*之外*，允许使用运算符。

> ***已关闭问题：*** 对于隐藏基接口中的成员的接口成员声明，是否应允许 `new`？ ***决策***：是。

> ***已关闭问题：*** 目前不允许对某个接口或其成员执行 `partial`。 这需要单独建议。 ***决策***：是。 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>

### <a name="overrides-in-interfaces"></a>接口中的替代

重写声明（即包含 `override` 修饰符的声明）允许编程人员在编译器或运行时不会找到一个的接口中提供最具体的虚拟成员实现。 它还允许将抽象成员从超级接口转换为派生接口中的默认成员。 允许重写声明通过使用接口名称（在这种情况下不允许使用访问修饰符）来限定声明，从而*显式*重写特定基接口方法。 不允许使用隐式重写。

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); } // explicitly named
}
interface IC : IA
{
    override void M() { WriteLine("IC.M"); } // implicitly named
}
```

接口中的重写声明不能 `sealed`声明。

接口中的公共 `virtual` 函数成员可以在派生接口中显式重写（通过使用最初声明方法的接口类型在重写声明中限定名称，并省略访问修饰符）。

接口中 `virtual` 函数成员只能在派生接口中显式重写（而非隐式），并且未 `public` 的成员只能在类或结构中显式实现（而不是隐式）。 在任一情况下，重写或实现的成员都必须可在重写时*访问*。

### <a name="reabstraction"></a>Reabstraction

在接口中声明的虚拟（具体）方法可能会被重写为在派生接口中是抽象的

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    abstract void IA.M();
}
class C : IB { } // error: class 'C' does not implement 'IA.M'.
```

`IB.M` （这是接口中的默认值）的声明中不需要 `abstract` 修饰符，但最好是在重写声明中显式操作。

这适用于方法的默认实现不恰当并且应通过实现类提供更合适的实现的派生接口。

> ***打开问题：*** 是否应允许 reabstraction？

### <a name="the-most-specific-override-rule"></a>最特定的替代规则

我们要求每个接口和类在类型或其直接和间接接口中出现的重写中的每个虚拟成员都有一个*最特定的重写*。 *最具体的替代*是比每个其他重写更具体的替代。 如果没有替代，则将成员本身视为最特定的重写。

如果在类型 `T1`上声明了 `M1`，则将一个重写 `M1` 视为比另一个重写 `M2`*更为具体*，`M2` 在类型 `T2`上声明，并在

1. `T1` 在其直接或间接接口之间包含 `T2`，或
2. `T2` 是接口类型，但 `T1` 不是接口类型。

例如：

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    void IA.M() { WriteLine("IC.M"); }
}
interface ID : IB, IC { } // error: no most specific override for 'IA.M'
abstract class C : IB, IC { } // error: no most specific override for 'IA.M'
abstract class D : IA, IB, IC // ok
{
    public abstract void M();
}

```

最具体的替代规则确保程序员在发生冲突的情况下显式解析冲突（即，由菱形继承引起的歧义）。

由于我们在接口中支持显式抽象替代，因此也可以在类中执行此操作

```csharp
abstract class E : IA, IB, IC // ok
{
    abstract void IA.M();
}
```

> ***打开问题***：我们是否应在类中支持显式接口抽象替代？

此外，如果在类声明中，某些接口方法的最特定重写是在接口中声明的抽象重写，则是错误的。 这是生效首先使用新术语的现有规则。

```csharp
interface IF
{
    void M();
}
abstract class F : IF { } // error: 'F' does not implement 'IF.M'
```

在接口中声明的虚拟属性有可能在一个接口中具有对其 `get` 访问器的最特定的重写，并且在另一个接口中具有对其 `set` 访问器的最特定的重写。 这被视为违反*最具体的替代*规则。

### <a name="static-and-private-methods"></a>`static` 和 `private` 方法

由于接口现在可以包含可执行代码，因此将常见代码抽象到私有和静态方法非常有用。 现在，我们在接口中允许这些。

> ***已关闭问题***：是否应支持私有方法？ 是否应支持静态方法？ **决策：是**

> ***打开问题***：是否应允许接口方法 `protected` 或 `internal` 或其他访问权限？ 如果是，语义是什么？ 它们是否默认 `virtual`？ 如果是，有没有办法使它们成为非虚拟的？

> ***打开问题***：如果我们支持静态方法，是否应支持（静态）运算符？

### <a name="base-interface-invocations"></a>基接口调用

使用默认方法从接口派生的类型中的代码可以显式调用该接口的 "base" 实现。

```csharp
interface I0
{
   void M() { Console.WriteLine("I0"); }
}
interface I1 : I0
{
   override void M() { Console.WriteLine("I1"); }
}
interface I2 : I0
{
   override void M() { Console.WriteLine("I2"); }
}
interface I3 : I1, I2
{
   // an explicit override that invoke's a base interface's default method
   void I0.M() { I2.base.M(); }
}

```

实例（非静态）方法允许通过使用语法 `base(Type).M`来命名直接基接口 nonvirtually 中可访问实例方法的实现。 此方法在以下情况下很有用：通过委托给某个特定基实现来解析由于菱形继承而需要提供的重写。

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    override void IA.M() { WriteLine("IC.M"); }
}

class D : IA, IB, IC
{
    void IA.M() { base(IB).M(); }
}
```

当使用语法 `base(Type).M`访问 `virtual` 或 `abstract` 成员时，`Type` 包含 `M`的唯一*特定的重写*。

### <a name="binding-base-clauses"></a>绑定基子句

接口现在包含类型。  这些类型可以在基子句中用作基接口。  绑定基子句时，可能需要知道要绑定这些类型的基接口集（例如，在这些类型中查找和解析受保护的访问）。  因此，会循环定义接口的基子句的含义。  为了打破循环，我们添加了与类中已有的类似规则相对应的新语言规则。

在确定接口的*interface_base*含义时，将暂时假设基接口为空。 这可以确保基本子句的含义无法以递归方式依赖于自身。 

**我们使用以下规则：**

"当类 B 从类 A 派生时，它是依赖于 B 的编译时错误。类**直接依赖于**其直接基类（如果有），并且**直接依赖于**直接嵌套它的~~**类**~~（如果有）。 根据此定义，类所依赖的完整~~**类**~~集是**直接依赖**关系的反身和可传递闭包。

接口直接或间接从自身继承时，会发生编译时错误。
接口的**基接口**是显式基接口及其基接口。 换言之，基接口集是显式基接口的完全可传递的闭包、其显式基接口等。

**我们将调整它们，如下所示：**

当类 B 从类 A 派生时，它是依赖于 B 的编译时错误。类**直接依赖于**其直接基类（如果有），并且**直接依赖于**直接嵌套它的 _**类型**_ （如果有）。

接口 IB 扩展接口 IA 时，IA 依赖于 IB 会出现编译时错误。 接口**直接依赖于**其直接基接口（如果有），并且**直接依赖于**直接嵌套它的类型（如果有）。

考虑到这些定义，类型所依赖的完整**类型**集是**直接依赖**关系的反身和可传递闭包。

### <a name="effect-on-existing-programs"></a>对现有程序的影响

此处介绍的规则不会影响现有程序的含义。

示例 1：

```csharp
interface IA
{
    void M();
}
class C: IA // Error: IA.M has no concrete most specific override in C
{
    public static void M() { } // method unrelated to 'IA.M' because static
}
```

示例 2：

```csharp
interface IA
{
    void M();
}
class Base: IA
{
    void IA.M() { }
}
class Derived: Base, IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

对于涉及默认接口方法的类似情况，相同的规则将产生类似的结果：

```csharp
interface IA
{
    void M() { }
}
class Derived: IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

> ***已关闭问题***：确认这是规范的预期结果。 **决策：是**

### <a name="runtime-method-resolution"></a>运行时方法解析

> ***已关闭问题：*** 规范应描述接口默认方法的运行时方法解析算法。 我们需要确保语义与语言语义一致，例如，已声明的方法的执行操作，并且不会重写或实现 `internal` 方法。

### <a name="clr-support-api"></a>CLR 支持 API

为了使编译器能够在为支持此功能的运行时进行检测，将修改此类运行时的库，以便通过 <https://github.com/dotnet/corefx/issues/17116>中所述的 API 播发该事实。 我们添加

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeFeature
    {
        // Presence of the field indicates runtime support
        public const string DefaultInterfaceImplementation = nameof(DefaultInterfaceImplementation);
    }
}
```

> ***打开问题***： *CLR*功能的最佳名称是吗？ CLR 功能不仅仅是这样（例如放宽保护约束、支持接口中的重写等）。 它可能称为 "接口中的具体方法" 或 "特征" 之类的内容？

### <a name="further-areas-to-be-specified"></a>要指定的其他区域

- [] 通过向现有接口添加默认的接口方法和替代，对导致源和二进制兼容性的各种影响进行编目是非常有用的。

## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

此建议需要对 CLR 规范进行协调更新（以支持接口和方法解析中的具体方法）。 因此，这种情况相当 "昂贵"，可能需要将其与其他可能需要 CLR 更改的功能结合在一起。

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

无。

## <a name="unresolved-questions"></a>未解决的问题
[unresolved]: #unresolved-questions

- 在上面的方案中，我们提出了开放式问题。
- 有关打开的问题的列表，另请参阅 <https://github.com/dotnet/csharplang/issues/406>。
- 详细规范必须描述运行时用于选择要调用的精确方法的解析机制。
- 新编译器所生成的元数据与旧编译器使用的元数据交互需要进行详细处理。 例如，我们需要确保使用的元数据表示形式不会导致接口中添加默认实现，以在由较旧的编译器编译时破坏实现该接口的现有类。 这可能会影响可使用的元数据表示形式。
- 设计必须考虑与其他语言的互操作性和其他语言的现有编译器。

## <a name="resolved-questions"></a>解决的问题

### <a name="abstract-override"></a>Abstract 重写

早期草案规范包含了 "reabstract" 继承方法的能力：

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { }
}
interface IC : IB
{
    override void M(); // make it abstract again
}
```

我的说明2017-03-20 显示我们决定不允许这样做。 但是，其中至少有两个用例：

1. Java Api，此功能的某些用户希望进行互操作，这取决于此功能。
2. 利用这*一优势来*编程。 Reabstraction 是 "特征" 语言功能（ https://en.wikipedia.org/wiki/Trait_(computer_programming))的元素之一。 以下类允许使用类：

```csharp
public abstract class Base
{
    public abstract void M();
}
public abstract class A : Base
{
    public override void M() { }
}
public abstract class B : A
{
    public override abstract void M(); // reabstract Base.M
}
```

遗憾的是，除非允许这样做，否则不能将此代码重构为一组接口（特征）。 贪婪的*Jared 原则*应允许这样做。

> ***已关闭问题：*** 是否应允许 reabstraction？ [我的笔记出错了。 [LDM 说明](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)指出接口中允许 reabstraction。 不在类中。

### <a name="virtual-modifier-vs-sealed-modifier"></a>虚拟修饰符与密封修饰符

From [Aleksey Tsingauz](https://github.com/AlekseyTs)：

> 我们决定允许在接口成员上显式声明修饰符，除非有理由禁止这样做。 这就为虚拟修饰符带来了一个有趣的问题。 是否应在具有默认实现的成员上需要？
>
> 我们可能会说：
>
> - 如果没有任何实现，也没有指定虚拟和密封，则假定该成员是抽象成员。
> - 如果有一个实现并且未指定任何抽象和密封，则假定该成员是虚拟的。
> - 要使方法既不是虚拟的也不是抽象的，必须使用 sealed 修饰符。
>
> 另外，我们也可能会说虚拟修饰符对于虚拟成员是必需的。 例如，如果存在未使用虚拟修饰符显式标记的实现的成员，则它既不是虚拟的也不是抽象的。 当方法从类移到接口时，此方法可能会提供更好的体验：
>
> - 抽象方法保持抽象。
> - 虚方法保持为虚方法。
> - 没有任何修饰符的方法将保持不变，也不会成为抽象方法。
> - 密封修饰符不能应用于不是重写的方法。
>
> 你觉得怎么样？

> ***已关闭问题：*** 是否应隐式地 `virtual`具体的方法（具有实现）？ [

***决策：*** 在 LDM 2017-04-05 中创建：

1. 非虚拟应通过 `sealed` 或 `private`显式表达。
2. `sealed` 是使接口实例成员具有非虚拟主体的关键字
3. 我们希望允许接口中的所有修饰符  
4. 接口成员的默认可访问性是公共的，其中包括嵌套类型
5. 接口中的私有函数成员是隐式密封的，不允许在这些成员上使用 `sealed`。
6. 允许使用私有类（接口中的），可以将其密封，这意味着密封在密封类的类中。
7. 缺少良好的建议，不允许在接口或其成员上使用 partial。

### <a name="binary-compatibility-1"></a>二进制兼容性1

当库提供默认实现时

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
}
class C : I2
{
}
```

我们了解到 `C` 中的 `I1.M` 实现是 `I1.M`的。 如果包含 `I2` 的程序集按如下方式更改并重新编译，会发生什么情况呢？

```csharp
interface I2 : I1
{
    override void M() { Impl2 }
}
```

但不会重新编译 `C`。 程序运行时会发生什么情况？ 调用 `(C as I1).M()`

1. 运行 `I1.M`
2. 运行 `I2.M`
3. 引发某种类型的运行时错误

***决策：*** 进行2017-04-11：运行 `I2.M`，这是运行时明确的最具体替代。

### <a name="event-accessors-closed"></a>事件访问器（关闭）

> ***已关闭问题：*** 事件是否可以重写 "分段"？

请考虑这种情况：

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        // error: "remove" accessor missing
    }
}
```

不允许使用事件的 "部分" 实现，因为在类中，事件声明的语法不允许只有一个访问器;必须同时提供两个（或两个）。 通过允许语法中的 abstract remove 访问器在缺少正文时隐式抽象，可以实现相同的目的：

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        remove; // implicitly abstract
    }
}
```

请注意，*这是一个新的（建议的）语法*。 在当前语法中，事件访问器具有强制正文。

> ***已关闭问题：*** 事件访问器可以通过省略主体来确定（隐式）抽象，类似于通过省略主体来确定接口和属性访问器中的方法（隐式）抽象的方法。

***决策：*** （2017-04-18）否，事件声明需要两个具体的访问器（或均不需要）。

### <a name="reabstraction-in-a-class-closed"></a>类中的 Reabstraction （已关闭）

***已关闭问题：*** 我们应确认允许这样做（否则添加默认实现将是一项重大更改）：

```csharp
interface I1
{
    void M() { }
}
abstract class C : I1
{
    public abstract void M(); // implement I1.M with an abstract method in C
}
```

***决策：*** （2017-04-18）是，将主体添加到接口成员声明不应中断 C。

### <a name="sealed-override-closed"></a>密封的替代（已关闭）

前面的问题隐式假定 `sealed` 修饰符可应用于接口中的 `override`。 这与草案规范相抵触。 是否允许密封替代？ 应考虑密封的源和二进制兼容性影响。

> ***已关闭问题：*** 是否应允许密封替代？

***决策：*** （2017-04-18）不允许在接口中的替代 `sealed`。 对接口成员的 `sealed` 唯一使用是在其初始声明中使其成为非虚拟的。

### <a name="diamond-inheritance-and-classes-closed"></a>菱形继承和类（闭合）

提议的草稿在菱形继承方案中，类优先于接口替代：

> 我们要求每个接口和类在类型或其直接和间接接口中出现的重写中都有对每个接口方法的*最特定的重写*。 *最具体的替代*是比每个其他重写更具体的替代。 如果没有替代，则将方法本身视为最特定的重写。
>
> 如果在类型 `T1`上声明了 `M1`，则将一个重写 `M1` 视为比另一个重写 `M2`*更为具体*，`M2` 在类型 `T2`上声明，并在
>
> 1. `T1` 在其直接或间接接口之间包含 `T2`，或
> 2. `T2` 是接口类型，但 `T1` 不是接口类型。

此方案是

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { WriteLine("IB"); }
}
class Base : IA
{
    void IA.M() { WriteLine("Base"); }
}
class Derived : Base, IB // allowed?
{
    static void Main()
    {
        Ia a = new Derived();
        a.M();           // what does it do?
    }
}
```

我们应确认此行为（或自行决定）

> ***已关闭问题：*** 在上面的草案规范适用于混合类和接口（类优先于接口）的情况下，针对*大多数特定的替代*，进行确认。 请参阅 <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>。

### <a name="interface-methods-vs-structs-closed"></a>接口方法 vs 结构（闭合）

默认的接口方法和结构之间存在一些不幸的交互。

```csharp
interface IA
{
    public void M() { }
}
struct S : IA
{
}
```

请注意，不会继承接口成员：

```csharp
var s = default(S);
s.M(); // error: 'S' does not contain a member 'M'
```

因此，客户端必须对该结构进行方框才能调用接口方法

```csharp
IA s = default(S); // an S, boxed
s.M(); // ok
```

采用这种方式进行装箱将违背 `struct` 类型的主要优点。 而且，任何变化方法都不会有明显的影响，因为它们在结构的已*装箱副本*上操作：

```csharp
interface IB
{
    public void Increment() { P += 1; }
    public int P { get; set; }
}
struct T : IB
{
    public int P { get; set; } // auto-property
}

T t = default(T);
Console.WriteLine(t.P); // prints 0
(t as IB).Increment();
Console.WriteLine(t.P); // prints 0
```

> ***已关闭问题：*** 我们可以对此做些什么：
>
> 1. 禁止 `struct` 继承默认实现。 在 `struct`中，所有接口方法均被视为抽象方法。 稍后，我们可能需要花费一段时间来决定如何使其更好地工作。
> 2. 提供了一种可避免装箱的代码生成策略。 在 `IB.Increment`之类的方法中，`this` 的类型可能类似于类型参数，该类型参数被约束为 `IB`。 与此相结合，若要避免调用方中的装箱，将从接口继承非抽象方法。 这会显著提高编译器和 CLR 实现的工作效果。
> 3. 不用担心，只是将其保留为 wart。
> 4. 其他想法？

***决策：*** 不用担心，只是将其保留为 wart。 请参阅 <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>。

### <a name="base-interface-invocations-closed"></a>基接口调用（已关闭）

草案规范建议使用 Java 的基本接口调用的语法： `Interface.base.M()`。 我们需要至少为初始原型选择一种语法。 我喜欢 `base<Interface>.M()`。

> ***已关闭问题：*** 基本成员调用的语法是什么？

***决策：*** 语法为 `base(Interface).M()`。 请参阅 <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>。 接口如此命名必须是基接口，但不需要是直接基接口。

> ***打开问题：*** 是否应允许在类成员中调用基接口？

***决策***：是。 <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>

### <a name="overriding-non-public-interface-members-closed"></a>重写非公共接口成员（已关闭）

在接口中，通过使用 `override` 修饰符重写基接口中的非公共成员。 如果它是命名包含成员的接口的 "显式" 重写，则省略访问修饰符。

> ***已关闭问题：*** 如果它是一个不命名接口的 "隐式" 重写，则访问修饰符是否必须匹配？

***决策：*** 仅公共成员可以隐式重写，并且访问必须匹配。 请参阅 <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>。

> ***打开问题：*** 访问修饰符是必需的还是可选的，或在显式重写（如 `override void IB.M() {}`）上省略。

> ***打开问题：*** 在显式重写（如 `void IB.M() {}`）上 `override` required、optional 还是省略？

如何实现类中的非公共接口成员？ 这可能是必须显式完成的吗？

```csharp
interface IA
{
    internal void MI();
    protected void MP();
}
class C : IA
{
    // are these implementations?
    internal void MI() {}
    protected void MP() {}
}
```

> ***已关闭问题：*** 如何实现类中的非公共接口成员？

***决策：*** 您只能显式实现非公共接口成员。 请参阅 <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>。

***决策***：不允许在接口成员上使用 `override` 关键字。 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>

### <a name="binary-compatibility-2-closed"></a>二进制兼容性2（关闭）

请考虑以下代码，其中每个类型都在单独的程序集中

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
    override void M() { Impl2 }
}
interface I3 : I1
{
}
class C : I2, I3
{
}
```

我们了解到 `C` 中的 `I1.M` 实现是 `I2.M`的。 如果包含 `I3` 的程序集按如下方式更改并重新编译，会发生什么情况呢？

```csharp
interface I3 : I1
{
    override void M() { Impl3 }
}
```

但不会重新编译 `C`。 程序运行时会发生什么情况？ 调用 `(C as I1).M()`

1. 运行 `I1.M`
2. 运行 `I2.M`
3. 运行 `I3.M`
4. 2或3，确定性
5. 引发某种类型的运行时异常

***决策***：引发异常（5）。 请参阅 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>。

### <a name="permit-partial-in-interface-closed"></a>是否允许在接口中 `partial`？ 闭

如果接口的使用方式类似于使用抽象类的方式，则将它们声明 `partial`可能会很有用。 这对于生成器而言特别有用。

> ***建议：*** 删除接口和接口成员不能 `partial`声明的语言限制。

***决策***：是。 请参阅 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>。

### <a name="main-in-an-interface-closed"></a>在接口中 `Main`？ 闭

> ***打开问题：*** 接口中的 `static Main` 方法是程序入口点的候选项吗？

***决策***：是。 请参阅 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>。

### <a name="confirm-intent-to-support-public-non-virtual-methods-closed"></a>确认意图以支持公共非虚方法（已关闭）

我们是否可以确认（或反转）我们决定是否允许接口中的非虚拟公共方法？

```csharp
interface IA
{
    public sealed void M() { }
}
```

> ***半闭问题：*** （2017-04-18）我们认为它将有用，但会返回到它。 这是一种精神模型的工作块。

***决策***：是。 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods> 列中的一个值匹配。

### <a name="does-an-override-in-an-interface-introduce-a-new-member-closed"></a>接口中的 `override` 是否引入了新成员？ 闭

有几种方法可以观察重写声明是否引入了新成员。

```csharp
interface IA
{
    void M(int x) { }
}
interface IB : IA
{
    override void M(int y) { }
}
interface IC : IB
{
    static void M2()
    {
        M(y: 3); // permitted?
    }
    override void IB.M(int z) { } // permitted? What does it override?
}
```

> ***打开问题：*** 接口中的重写声明是否引入了新成员？ 闭

在类中，重写方法在某种方式上为 "visible"。 例如，其参数的名称优先于重写方法中的参数的名称。 可以在接口中重复该行为，因为始终存在最特定的重写。 但我们想要复制该行为吗？

此外，还可以 "重写" 重写方法呢？ 差异

***决策***：不允许在接口成员上使用 `override` 关键字。 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member> 列中的一个值匹配。

### <a name="properties-with-a-private-accessor-closed"></a>具有专用访问器的属性（关闭）

我们说，private 成员不是虚拟的，不允许结合虚拟和私有。 但对于具有专用访问器的属性怎么办呢？

```csharp
interface IA
{
    public virtual int P
    {
        get => 3;
        private set => { }
    }
}
```

是否允许这样做？ `set` 访问器是否 `virtual`？ 它可以在可访问的位置重写吗？ 下面是否隐式仅实现 `get` 访问器？

```csharp
class C : IA
{
    public int P
    {
        get => 4;
        set { }
    }
}
```

由于 IA，以下原因可能会导致错误。设置不是虚拟的，也可能是因为它无法访问？

```csharp
class C : IA
{
    int IA.P
    {
        get => 4;
        set { }
    }
}
```

***决策***：第一个示例看起来有效，而最后一个示例则无效。 这会被解析为类似在中的C#工作方式。 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#properties-with-a-private-accessor>

### <a name="base-interface-invocations-round-2-closed"></a>基接口调用，舍入2（关闭）

前面的 "解决方法" 是如何处理基本调用，实际上不提供足够的表现力。 事实证明，在和C# CLR 中，与 Java 不同，您需要指定包含方法声明的接口以及要调用的实现的位置。

我建议将以下语法用于接口中的基本调用。 我不喜欢它，但它说明了什么语法必须能够表达：

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I4 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>(I1).M(); // calls I3's implementation of I1.M
        base<I4>(I1).M(); // calls I4's implementation of I1.M
    }
    void I2.M()
    {
        base<I3>(I2).M(); // calls I3's implementation of I2.M
        base<I4>(I2).M(); // calls I4's implementation of I2.M
    }
}
```

如果没有歧义，可以更简单地编写

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I4 : I1 { void I1.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>.M(); // calls I3's implementation of I1.M
        base<I4>.M(); // calls I4's implementation of I1.M
    }
}
```

或

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base(I1).M(); // calls I3's implementation of I1.M
    }
    void I2.M()
    {
        base(I2).M(); // calls I3's implementation of I2.M
    }
}
```

或

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base.M(); // calls I3's implementation of I1.M
    }
}
```

***决策***：决定 `base(N.I1<T>).M(s)`conceding，如果有调用绑定，稍后会出现问题。 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md#default-interface-implementations>

### <a name="warning-for-struct-not-implementing-default-method-closed"></a>结构未实现默认方法时的警告？ 闭

@vancem 断言，如果值类型声明未能重写某些接口方法，则应认真考虑生成警告，即使它会从接口继承该方法的实现。 因为这会导致装箱和会破坏约束调用。

***决策***：看起来就像是更适合分析器的内容。 这似乎会干扰此警告，因为即使从未调用默认接口方法，也不会出现任何装箱。 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#warning-for-struct-not-implementing-default-method>

### <a name="interface-static-constructors-closed"></a>接口静态构造函数（已关闭）

何时运行接口静态构造函数？  当前 CLI 草稿建议在访问第一个静态方法或字段时发生。 如果没有这样做，则可能永远不会运行它？

[2018-10-09 CLR 团队建议 "转而对具有进行镜像（对每个实例方法的访问进行 .cctor 检查）"]

***决策***：如果未 `beforefieldinit`静态构造函数，则静态构造函数也会在输入到实例方法时运行，在这种情况下，将在访问第一个静态字段之前运行静态构造函数。 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#when-are-interface-static-constructors-run>

## <a name="design-meetings"></a>设计会议

[2017-03-08 Ldm 会议说明](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md)
[2017-03-21 ldm 会议说明](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)
[2017-03-23 会议 "默认接口方法的 CLR 行为"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)
[2017-04-05 LDM 会议](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md)笔记
[2017-04-11 ldm 会议](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md)笔记
2017-04-18 [ldm](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md)会议笔记
2017-04-19 [ldm](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md)会议笔记
2017-05-17 ldm[会议笔记](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md)
[2017-05-31 ldm](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md) [会议说明](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md)
2018-10-17 [2018-11-14](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)
的[ldm 会议笔记](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)

