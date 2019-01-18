---
ms.openlocfilehash: 08c14d9ef2afe30580f456995066c141653ede92
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229521"
---
# <a name="delegates"></a>委托

委托实现方案的其他语言，如 c + +、 Pascal 和 Modula-已，解决与函数指针。 与 c + + 函数指针不同，委托完全是面向对象的而且与 c + + 成员函数的指针，不同的委托封装的对象实例和方法。

在委托声明定义了从类派生一个类`System.Delegate`。 委托实例封装一个调用列表，这是一个或多个方法，其中每个被称为可调用实体的列表。 对于实例方法，可调用实体实例和包含该实例上的方法。 对于静态方法，可调用实体包含的只是一种方法。 调用具有一组适当的参数的委托实例会导致每个委托的可调用实体使用给定的参数集的调用。

委托实例的一个有趣且有用的属性是它不会知道或关心封装; 方法的类最重要的就是这些方法为兼容 ([委托声明](delegates.md#delegate-declarations)) 与委托的类型。 这样，非常适合于"匿名"调用的委托。

## <a name="delegate-declarations"></a>委托声明

一个*delegate_declaration*是*type_declaration* ([类型声明](namespaces.md#type-declarations))，其用于声明新的委托类型。

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

它是在委托声明中多次出现同一修饰符的编译时错误。

`new`修饰符仅允许在委托声明中另一种类型，它指定此类委托在这种情况下隐藏继承的成员通过相同的名称，如中所述[的新修饰符](classes.md#the-new-modifier)。

`public`， `protected`， `internal`，和`private`修饰符控制委托类型的可访问性。 具体取决于与委托声明发生的上下文，其中某些修饰符可能不允许使用 ([声明可访问性](basic-concepts.md#declared-accessibility))。

委托的类型名称是*标识符*。

可选*formal_parameter_list*指定的参数的委托，并*return_type*指示委托的返回类型。

可选*variant_type_parameter_list* ([变体类型的参数列表](interfaces.md#variant-type-parameter-lists)) 指定自身的委托的类型参数。

委托类型的返回类型必须是`void`，或输出安全 ([变体安全](interfaces.md#variance-safety))。

所有委托类型的正式参数类型必须都是输入安全。 此外，如果`out`或`ref`参数类型还必须是安全的输出。 请注意，甚至`out`参数需是输入安全的由于基础执行平台的限制。

等效的而不是结构等效的 C# 中的委托类型是名称。 具体而言，具有相同的参数的两种不同的委托类型列出并返回类型被视为不同的委托类型。 但是，两个不同但结构等效的委托类型的实例可能会比较结果相等 ([委托相等运算符](expressions.md#delegate-equality-operators))。

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

方法`A.M1`并`B.M1 `与委托类型兼容`D1`和`D2`，因为它们具有相同的返回类型和参数列表; 但是，这些委托类型是两种不同类型，因此它们不是可互换。 方法`B.M2`， `B.M3`，并`B.M4`与委托类型不兼容`D1`和`D2`，因为它们具有不同的返回类型或参数列表。

如其他泛型类型声明，必须提供类型参数以创建构造的委托类型。 通过将替换为在委托声明中，每个类型参数的构造的委托类型的相应类型参数创建的参数类型和构造的委托类型的返回类型。 生成的返回类型和参数类型用于确定使用哪些方法将与构造的委托类型兼容。 例如：

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

该方法`X.F`委托类型与兼容`Predicate<int>`和方法`X.G`与委托类型兼容`Predicate<string>`。

声明委托类型的唯一方法是通过*delegate_declaration*。 委托类型是派生自的类类型`System.Delegate`。 委托类型是隐式`sealed`，因此不允许从委托类型派生的任何类型。 此外，也不允许派生类非委托类型从`System.Delegate`。 请注意，`System.Delegate`是委托类型本身不; 它是所有委托类型都派生的类类型。

C# 提供特殊语法委托实例化和调用。 实例化，除了可以应用于类或类实例的任何操作可以也应用于委托类或实例，分别。 具体而言，就可以访问的成员`System.Delegate`通过常用的成员访问语法类型。

委托实例封装的方法集称为一个调用列表。 创建委托实例时 ([委托兼容性](delegates.md#delegate-compatibility)) 的单个方法，它封装此方法，并且其调用列表中包含只有一个条目。 但是，当组合两个非 null 委托实例时，它们的调用列表串联起来，-顺序从左操作数，则右操作数-以形成新的调用列表，其中包含两个或多个条目。

使用二进制组合委托`+`([加法运算符](expressions.md#addition-operator)) 和`+=`运算符 ([复合赋值](expressions.md#compound-assignment))。 委托可以从委托，使用二进制文件的组合`-`([减法运算符](expressions.md#subtraction-operator)) 和`-=`运算符 ([复合赋值](expressions.md#compound-assignment))。 委托可以比较它们是否相等 ([委托相等运算符](expressions.md#delegate-equality-operators))。

下面的示例演示实例化多个委托，并列出其对应的调用：

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

当`cd1`和`cd2`是实例化，它们分别封装一个方法。 当`cd3`是实例化，它具有两种方法的调用列表`M1`和`M2`按顺序。 `cd4`调用列表包含`M1`， `M2`，和`M1`按顺序。 最后，`cd5`的调用列表包含`M1`， `M2`， `M1`， `M1`，并`M2`按顺序。 组合 （也为删除） 委托的更多示例，请参阅[委托调用](delegates.md#delegate-invocation)。

## <a name="delegate-compatibility"></a>委托兼容性

方法或委托`M`是***兼容***与委托类型`D`如果满足所有以下：

*  `D` 并`M`具有相同数目的参数，以及在每个参数`D`具有相同`ref`或`out`与中的相应参数的修饰符`M`。
*  对于每个值参数 (不带参数`ref`或`out`修饰符)，标识转换 ([标识转换](conversions.md#identity-conversion)) 或隐式引用转换 ([隐式引用转换](conversions.md#implicit-reference-conversions))存在从中的参数类型`D`中的相应参数类型为`M`。
*  每个`ref`或`out`中的参数，参数类型`D`中的参数类型与相同`M`。
*  存在从的返回类型的标识或隐式引用转换`M`的返回类型为`D`。

## <a name="delegate-instantiation"></a>委托实例化

通过创建委托的实例*delegate_creation_expression* ([委托创建表达式](expressions.md#delegate-creation-expressions)) 或转换为委托类型。 为然后引用新创建的委托实例：

*  中引用的静态方法*delegate_creation_expression*，或
*  目标对象 (这不能`null`) 和实例方法中引用*delegate_creation_expression*，或
*  另一个委托。

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

实例化后，委托实例始终引用相同的目标对象和方法。 请记住，当结合使用两个委托，或一个已从另一种，使用它自己的调用列表; 的新委托结果合并或移除的委托的调用列表将保持不变。

## <a name="delegate-invocation"></a>委托调用

C# 提供特殊的语法调用委托。 当调用其调用列表包含一项的非 null 委托实例时，它调用具有它提供，并返回相同的值均与相同的参数的方法到方法。 (请参阅[委托调用](expressions.md#delegate-invocations)的委托调用的详细信息。)如果此类委托的调用期间发生异常，并且未调用的方法中捕获该异常，就像已直接调用该方法，该异常的 catch 子句的搜索将继续在调用该委托的方法方法的委托到的引用。

通过调用每个调用列表中的方法以同步方式，按顺序将继续对其调用列表包含多个条目的委托实例的调用。 所谓的每个方法传递一组相同的参数与提供给委托实例。 如果此类委托调用包括引用参数 ([引用参数](classes.md#reference-parameters))，每个方法调用都将使用相同的变量的引用; 将更改该变量的调用列表中的另一种方法到后面的方法调用列表中向下可见。 如果委托调用包含输出参数或返回值，其最终值将来自列表中的最后一个委托的调用。

如果在处理此类委托的调用期间发生异常，并且未调用的方法中捕获该异常，该异常的 catch 子句的搜索将继续中调用该委托的方法和任何方法的后面部分不会调用调用列表。

尝试调用的委托实例，其值为 null 类型的异常将导致`System.NullReferenceException`。

下面的示例演示如何实例化、 合并、 删除和调用委托：

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

该语句中所示`cd3 += cd1;`，多个时间时，可调用列表中出现一个委托。 在这种情况下，它是只需调用一次每个匹配项。 在这样的调用列表，删除该委托，调用列表中的最后一个匹配项是实际删除。

最后一条语句执行之前立即`cd3 -= cd1;`，该委托`cd3`引用了一个空的调用列表。 尝试从空白列表删除委托 （或从非空列表中删除不存在委托） 不是错误。

生成的输出是：

```
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
