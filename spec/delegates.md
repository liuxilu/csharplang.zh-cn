---
ms.openlocfilehash: d162d4b6a489032dcdfca9094a39d88fd03d4013
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704097"
---
# <a name="delegates"></a>委派

委托可实现其他语言（例如C++，Pascal 和 Modula）使用函数指针进行寻址的方案。 但C++与函数指针不同，委托是完全面向对象的，不同C++于指向成员函数的指针，委托封装对象实例和方法。

委托声明定义派生自类 `System.Delegate`的类。 委托实例封装一个调用列表，它是一个或多个方法的列表，其中每个方法都称为可调用实体。 对于实例方法，可调用实体包含实例和该实例上的方法。 对于静态方法，可调用实体仅包含方法。 使用适当的参数集调用委托实例会导致使用给定的参数集调用每个委托的可调用实体。

委托实例的一个有趣且有用的属性是它不知道或关心它所封装的方法的类;重要的是，这些方法与委托的类型兼容（[委托声明](delegates.md#delegate-declarations)）。 这使委托非常适合用于 "匿名" 调用。

## <a name="delegate-declarations"></a>委托声明

*Delegate_declaration*是声明新委托类型的*type_declaration* （[类型声明](namespaces.md#type-declarations)）。

```antlr
delegate_declaration
    : attributes? delegate_modifier* 'delegate' return_type
      identifier variant_type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;

delegate_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | delegate_modifier_unsafe
    ;
```

同一修饰符在一个委托声明中多次出现是编译时错误。

仅允许在另一种类型中声明的委托上使用 `new` 修饰符，在这种情况下，它指定此类委托隐藏同名的继承成员，如[new 修饰符](classes.md#the-new-modifier)中所述。

`public`、`protected`、`internal`和 `private` 修饰符控制委托类型的可访问性。 根据委托声明发生的上下文，可能无法使用其中某些修饰符（[声明的可访问性](basic-concepts.md#declared-accessibility)）。

委托的类型名称为*标识符*。

可选*formal_parameter_list*指定委托的参数， *return_type*指示委托的返回类型。

可选*variant_type_parameter_list* （[变体类型参数列表](interfaces.md#variant-type-parameter-lists)）指定委托本身的类型参数。

委托类型的返回类型必须是 `void`或输出安全的（[差异安全](interfaces.md#variance-safety)）。

委托类型的所有形参类型都必须为输入安全类型。 此外，任何 `out` 或 `ref` 参数类型也必须是输出安全的。 请注意，由于基础执行平台的限制，甚至 `out` 参数都需要是输入安全的参数。

中的C#委托类型是等效的名称，而不是结构上等效的。 具体而言，具有相同参数列表和返回类型的两个不同的委托类型被视为不同的委托类型。 不过，两个不同但结构等效的委托类型的实例可以比较为等于（[委托相等运算符](expressions.md#delegate-equality-operators)）。

例如：

```csharp
delegate int D1(int i, double d);

class A
{
    public static int M1(int a, double b) {...}
}

class B
{
    delegate int D2(int c, double d);
    public static int M1(int f, double g) {...}
    public static void M2(int k, double l) {...}
    public static int M3(int g) {...}
    public static void M4(int g) {...}
}
```

`A.M1` 和 `B.M1` 的方法与委托类型 `D1` 和 `D2` 兼容，因为它们具有相同的返回类型和参数列表;不过，这些委托类型是两种不同的类型，因此它们不能互换。 `B.M2`、`B.M3`和 `B.M4` 方法与委托类型 `D1` 和 `D2`不兼容，因为它们具有不同的返回类型或参数列表。

与其他泛型类型声明一样，必须提供类型参数来创建构造的委托类型。 构造委托类型的参数类型和返回类型是通过将委托声明中的每个类型参数替换为构造委托类型的相应类型参数来创建的。 生成的返回类型和参数类型用于确定哪些方法与构造的委托类型兼容。 例如：

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

方法 `X.F` 与委托类型兼容 `Predicate<int>` 并且方法 `X.G` 与委托类型 `Predicate<string>` 兼容。

声明委托类型的唯一方法是通过*delegate_declaration*。 委托类型是派生自 `System.Delegate`的类类型。 委托类型是隐式 `sealed`的，因此不允许从委托类型派生任何类型。 也不允许从 `System.Delegate`派生非委托类类型。 请注意，`System.Delegate` 自身不是委托类型;它是派生所有委托类型的类类型。

C#提供委托实例化和调用的特殊语法。 除实例化外，可以应用于类或类实例的任何操作也可以分别应用于委托类或实例。 具体而言，可以通过常规成员访问语法来访问 `System.Delegate` 类型的成员。

委托实例封装的方法集称为调用列表。 当从单个方法创建委托实例（[委托兼容性](delegates.md#delegate-compatibility)）时，它会封装该方法，并且它的调用列表只包含一个条目。 但是，如果两个非 null 委托实例组合在一起，则会将其调用列表串联在顺序左操作数和右操作数之间，以形成包含两个或多个条目的新调用列表。

使用二进制 `+` （[加法运算符](expressions.md#addition-operator)）和 `+=` 运算符（[复合赋值](expressions.md#compound-assignment)）组合委托。 可以使用二进制 `-` （[减法运算符](expressions.md#subtraction-operator)）和 `-=` 运算符（[复合赋值](expressions.md#compound-assignment)）从委托组合中删除委托。 可以比较委托是否相等（[委托相等运算符](expressions.md#delegate-equality-operators)）。

下面的示例演示了多个委托的实例化及其相应的调用列表：

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public static void M2(int i) {...}

}

class Test
{
    static void Main() {
        D cd1 = new D(C.M1);      // M1
        D cd2 = new D(C.M2);      // M2
        D cd3 = cd1 + cd2;        // M1 + M2
        D cd4 = cd3 + cd1;        // M1 + M2 + M1
        D cd5 = cd4 + cd3;        // M1 + M2 + M1 + M1 + M2
    }

}
```

实例化 `cd1` 和 `cd2` 时，它们将封装一个方法。 当 `cd3` 实例化时，它具有两个方法的调用列表，按该顺序 `M1` 和 `M2`。 `cd4`的调用列表按该顺序包含 `M1`、`M2`和 `M1`。 最后，`cd5`的调用列表按该顺序包含 `M1`、`M2`、`M1`、`M1`和 `M2`。 有关组合（以及删除）委托的更多示例，请参阅[委托调用](delegates.md#delegate-invocation)。

## <a name="delegate-compatibility"></a>委托兼容性

如果满足以下所有条件，则方法或委托 `M` 与委托类型 `D`***兼容***：

*  `D` 和 `M` 具有相同数量的参数，并且 `D` 中的每个参数都具有与 `out` 中的相应参数相同的 `ref` 或 `M`修饰符。
*  对于每个值参数（不带 `ref` 或 `out` 修饰符的参数），都存在从 `D` 中的参数类型到 `M`中的相应参数类型的标识转换（[标识转换](conversions.md#identity-conversion)）或隐式引用转换（[隐式引用](conversions.md#implicit-reference-conversions)转换）。
*  对于每个 `ref` 或 `out` 参数，`D` 中的参数类型与 `M`中的参数类型相同。
*  存在从 `M` 的返回类型到 `D`的返回类型的标识或隐式引用转换。

## <a name="delegate-instantiation"></a>委托实例化

委托的实例是由*delegate_creation_expression* （[委托创建表达式](expressions.md#delegate-creation-expressions)）或到委托类型的转换创建的。 然后，新创建的委托实例指的是：

*  *Delegate_creation_expression*中引用的静态方法，或
*  *Delegate_creation_expression*或中引用的目标对象（不能是 `null`）和实例方法
*  其他委托。

例如：

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public void M2(int i) {...}
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);        // static method
        C t = new C();
        D cd2 = new D(t.M2);        // instance method
        D cd3 = new D(cd2);        // another delegate
    }
}
```

实例化后，委托实例始终引用相同的目标对象和方法。 请记住，当合并两个委托或从另一个委托移除时，将使用其自己的调用列表生成新的委托结果;合并或移除的委托的调用列表保持不变。

## <a name="delegate-invocation"></a>委托调用

C#提供调用委托的特殊语法。 当调用列表包含一个项的非 null 委托实例调用时，它将调用一个方法，该方法具有给定的参数，并返回与被引用方法相同的值。 （有关委托调用的详细信息，请参阅[委托调用](expressions.md#delegate-invocations)。）如果在调用此类委托的过程中发生异常，且未在调用的方法中捕获该异常，则会在调用委托的方法中继续搜索异常 catch 子句，就好像该方法直接调用了委托引用的方法一样。

调用列表中包含多个项的委托实例的调用将按顺序同步调用调用列表中的每个方法。 调用的每个方法都被传递给为委托实例指定的相同的一组参数。 如果此类委托调用包括引用参数（[引用参数](classes.md#reference-parameters)），则将发生每个方法调用，同时引用同一变量;调用列表中的方法对该变量所做的更改将对调用列表中的方法可见。 如果委托调用包含输出参数或返回值，则其最终值将来自列表中最后一个委托的调用。

如果在处理此类委托的调用过程中发生异常，且未在调用的方法中捕获该异常，则会在调用委托的方法中继续执行异常 catch 子句的搜索，并使任何方法不会调用调用列表。

尝试调用值为 null 的委托实例会导致类型 `System.NullReferenceException`的异常。

下面的示例演示如何实例化、组合、移除和调用委托：

```csharp
using System;

delegate void D(int x);

class C
{
    public static void M1(int i) {
        Console.WriteLine("C.M1: " + i);
    }

    public static void M2(int i) {
        Console.WriteLine("C.M2: " + i);
    }

    public void M3(int i) {
        Console.WriteLine("C.M3: " + i);
    }
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);
        cd1(-1);                // call M1

        D cd2 = new D(C.M2);
        cd2(-2);                // call M2

        D cd3 = cd1 + cd2;
        cd3(10);                // call M1 then M2

        cd3 += cd1;
        cd3(20);                // call M1, M2, then M1

        C c = new C();
        D cd4 = new D(c.M3);
        cd3 += cd4;
        cd3(30);                // call M1, M2, M1, then M3

        cd3 -= cd1;             // remove last M1
        cd3(40);                // call M1, M2, then M3

        cd3 -= cd4;
        cd3(50);                // call M1 then M2

        cd3 -= cd2;
        cd3(60);                // call M1

        cd3 -= cd2;             // impossible removal is benign
        cd3(60);                // call M1

        cd3 -= cd1;             // invocation list is empty so cd3 is null

        cd3(70);                // System.NullReferenceException thrown

        cd3 -= cd1;             // impossible removal is benign
    }
}
```

如语句 `cd3 += cd1;`中所示，一个委托可以在调用列表中多次出现。 在这种情况下，它只会在每次出现时调用一次。 在此类调用列表中，移除该委托时，调用列表中的最后一个匹配项将被实际删除。

在执行最后一个语句之前，`cd3 -= cd1;`，委托 `cd3` 引用一个空的调用列表。 尝试从空列表中删除委托（或从非空列表中删除不存在的委托）不是错误。

生成的输出为：

```console
C.M1: -1
C.M2: -2
C.M1: 10
C.M2: 10
C.M1: 20
C.M2: 20
C.M1: 20
C.M1: 30
C.M2: 30
C.M1: 30
C.M3: 30
C.M1: 40
C.M2: 40
C.M3: 40
C.M1: 50
C.M2: 50
C.M1: 60
C.M1: 60
```
