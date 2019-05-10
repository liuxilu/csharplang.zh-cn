---
ms.openlocfilehash: 0a09585f4f885647230354c66a2449abb7ef1f44
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488779"
---
# <a name="interfaces"></a>接口

接口定义一个协定。 类或结构实现的接口必须遵守其协定。 一个接口可能从多个基接口继承，类或结构可以实现多个接口。

接口可以包含方法、 属性、 事件和索引器。 接口本身不提供实现，它定义的成员。 接口只是指定必须由实现接口的类或结构提供的成员。

## <a name="interface-declarations"></a>接口声明

*Interface_declaration*是*type_declaration* ([类型声明](namespaces.md#type-declarations))，其用于声明新的接口类型。

```antlr
interface_declaration
    : attributes? interface_modifier* 'partial'? 'interface'
      identifier variant_type_parameter_list? interface_base?
      type_parameter_constraints_clause* interface_body ';'?
    ;
```

*Interface_declaration*包含一组可选*特性*([特性](attributes.md)) 后, 跟一组可选的*interface_modifier*s ([接口修饰符](interfaces.md#interface-modifiers)) 后, 跟一个可选`partial`修饰符，关键字后跟`interface`和一个*标识符*命名接口，跟一个可选*variant_type_parameter_list*规范 ([变体类型的参数列表](interfaces.md#variant-type-parameter-lists)) 后, 跟一个可选*interface_base*规范 ([的基接口](interfaces.md#base-interfaces)) 后, 跟一个可选*type_parameter_constraints_clause*s 规范 ([类型参数约束](classes.md#type-parameter-constraints))后, 跟*interface_body* ([接口正文](interfaces.md#interface-body)) 后, 跟分号 （可选）。

### <a name="interface-modifiers"></a>接口修饰符

*Interface_declaration*可以根据需要包含一系列接口修饰符：

```antlr
interface_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | interface_modifier_unsafe
    ;
```

它是同一修饰符在接口声明中出现多次的编译时错误。

`new`修饰符仅允许在类中定义的接口上。 它指定该接口通过相同的名称，隐藏继承的成员中所述[的新修饰符](classes.md#the-new-modifier)。

`public`， `protected`， `internal`，和`private`修饰符控制接口的可访问性。 具体取决于接口声明发生的上下文，可能会允许仅在部分这些修饰符 ([声明可访问性](basic-concepts.md#declared-accessibility))。

### <a name="partial-modifier"></a>Partial 修饰符

`partial`修饰符指示这*interface_declaration*是分部类型声明。 具有相同名称的封闭命名空间或类型声明中的多个分部接口声明组合到窗体一个接口声明，遵循的规则中指定[分部类型](classes.md#partial-types)。

### <a name="variant-type-parameter-lists"></a>变体类型参数列表

变体类型参数列表只能出现在接口和委托类型。 与普通的差别*type_parameter_list*是可选的 s *variance_annotation*上每个类型参数。

```antlr
variant_type_parameter_list
    : '<' variant_type_parameters '>'
    ;

variant_type_parameters
    : attributes? variance_annotation? type_parameter
    | variant_type_parameters ',' attributes? variance_annotation? type_parameter
    ;

variance_annotation
    : 'in'
    | 'out'
    ;
```

如果方差批注`out`，类型参数称为***协变***。 如果方差批注`in`，类型参数称为***逆变***。 如果没有任何变化批注，类型参数称***固定***。

在示例
```csharp
interface C<out X, in Y, Z> 
{
  X M(Y y);
  Z P { get; set; }
}
```
`X` 为协变`Y`是逆变和`Z`是固定不变。

#### <a name="variance-safety"></a>变体安全

方差批注类型形参列表中的一种类型的匹配项将限制类型的类型声明中可能发生的位置的位置。

一种类型`T`是***输出不安全***如果下列之一保存：

*  `T` 是逆变类型参数
*  `T` 为具有输出不安全的元素类型的数组类型
*  `T` 是一个接口或委托类型`S<A1,...,Ak>`从泛型类型构造`S<X1,...,Xk>`至少一个 where`Ai`以下之一保存：
   * `Xi` 协变或固定和`Ai`是不安全的输出。
   * `Xi` 是逆变或固定条件和`Ai`是输入安全的。
   
一种类型`T`是***输入不安全***如果下列之一保存：

*  `T` 是协变类型参数
*  `T` 是输入不安全的元素类型的数组类型
*  `T` 是一个接口或委托类型`S<A1,...,Ak>`从泛型类型构造`S<X1,...,Xk>`至少一个 where`Ai`以下之一保存：
   * `Xi` 协变或固定和`Ai`是不安全的输入。
   * `Xi` 是逆变或固定条件和`Ai`是不安全的输出。

直观地说，输出不安全类型禁止的中的输出位置，并在输入位置禁止的输入不安全类型。

一种类型是***输出安全***如果不是输出不安全，并***输入安全***如果不输入不安全。

#### <a name="variance-conversion"></a>变化转换

方差批注的目的是提供更宽松 （但仍类型安全的） 转换成接口和委托类型。 到此结束的隐式定义 ([隐式转换](conversions.md#implicit-conversions)) 和显式转换 ([显式转换](conversions.md#explicit-conversions)) 进行可转换，按如下所示定义这一概念的使用：

一种类型`T<A1,...,An>`可变化转换为某种`T<B1,...,Bn>`如果`T`接口或委托类型使用变体类型参数声明`T<X1,...,Xn>`，并为每个变体类型参数`Xi`以下项之一包含：

*  `Xi` 是协变和隐式引用或标识转换存在从`Ai`到 `Bi`
*  `Xi` 是逆变和隐式引用或标识转换存在从`Bi`到 `Ai`
*  `Xi` 是固定条件和标识转换存在从`Ai`到 `Bi`

### <a name="base-interfaces"></a>基接口

接口可以继承自零个或多个接口类型，称为***显式基接口***的接口。 时接口具有一个或多个显式基接口，然后在该接口的声明中的接口标识符后跟冒号和逗号分隔的基接口类型的列表。

```antlr
interface_base
    : ':' interface_type_list
    ;
```

对于构造的接口类型，显式基接口构成方法为泛型类型声明中，对其执行显式基接口声明替换为每个*type_parameter*中的基接口相应的声明*type_argument*构造类型。

接口的显式基接口必须至少与接口本身相同的可访问性 ([可访问性约束](basic-concepts.md#accessibility-constraints))。 例如，它是指定的编译时错误`private`或`internal`接口中*interface_base*的`public`接口。

它是一个接口以直接或间接从自身继承的编译时错误。

***的基接口***已显式基接口并其基接口的接口。 换而言之，组的基接口是完全的显式基接口，其显式基接口，诸如此类的可传递闭包。 接口继承其基接口的所有成员。 在示例
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
接口的基`IComboBox`都`IControl`， `ITextBox`，和`IListBox`。

换而言之，`IComboBox`上述接口继承的成员`SetText`并`SetItems`以及`Paint`。

每个基接口的接口必须是安全的输出 ([变体安全](interfaces.md#variance-safety))。 类或结构还将隐式实现接口实现的所有接口的基接口。

### <a name="interface-body"></a>接口体内

*Interface_body*接口的定义接口的成员。

```antlr
interface_body
    : '{' interface_member_declaration* '}'
    ;
```

## <a name="interface-members"></a>接口成员

接口的成员是继承自基接口的成员和成员声明由接口本身。

```antlr
interface_member_declaration
    : interface_method_declaration
    | interface_property_declaration
    | interface_event_declaration
    | interface_indexer_declaration
    ;
```

接口声明可以声明零个或多个成员。 接口成员必须是方法、 属性、 事件或索引器。 接口不能包含常量、 字段、 运算符、 实例构造函数、 析构函数或类型，也不能包含任何类型的静态成员。

所有接口成员隐式都具有公共访问权限。 它是接口成员声明中包含任何修饰符的编译时错误。 具体而言，不能具有修饰符声明接口成员`abstract`， `public`， `protected`， `internal`， `private`， `virtual`， `override`，或`static`。

该示例
```csharp
public delegate void StringListEvent(IStringList sender);

public interface IStringList
{
    void Add(string s);
    int Count { get; }
    event StringListEvent Changed;
    string this[int index] { get; set; }
}
```
声明包含之一的每个可能类型的成员的接口：一种方法、 属性、 事件和索引器。

*Interface_declaration*创建一个新的声明空间 ([声明](basic-concepts.md#declarations))，并且*interface_member_declaration*s 立即包含*interface_declaration*引入此声明空间的新成员。 以下规则适用于*interface_member_declaration*s:

*  方法的名称必须不同于所有属性和相同的接口中声明的事件的名称。 此外，签名 ([签名和超载](basic-concepts.md#signatures-and-overloading)) 的一种方法必须不同于同一接口中声明的所有其他方法的签名和签名不能在同一个接口中声明的两种方法，完全由不同`ref`和`out`。
*  属性或事件的名称必须不同于在相同的接口中声明的所有其他成员的名称。
*  索引器的签名必须不同于同一接口中声明的所有其他索引器的签名。

接口的继承的成员是专门不属于该接口声明空间。 因此，接口可以声明一个具有相同名称或签名为继承的成员的成员。 当发生这种情况时，则称该派生的接口成员，若要隐藏基接口成员。 隐藏继承的成员不被视为错误，但它确实会导致编译器发出警告。 若要禁止显示警告，请在派生的接口成员的声明必须包括`new`修饰符以指示派生的成员用于隐藏基类成员。 本主题讨论中进一步[通过继承隐藏](basic-concepts.md#hiding-through-inheritance)。

如果`new`修饰符包含在不隐藏继承的成员的声明，该结果发出警告。 通过删除禁止显示此警告`new`修饰符。

请注意，在类成员`object`不严格地讲，任何接口的成员 ([接口成员](interfaces.md#interface-members))。 但是，在类成员`object`可通过成员查找中任何接口类型 ([成员查找](expressions.md#member-lookup))。

### <a name="interface-methods"></a>接口方法

使用声明接口方法*interface_method_declaration*s:

```antlr
interface_method_declaration
    : attributes? 'new'? return_type identifier type_parameter_list
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;
```

*特性*， *return_type*，*标识符*，以及*formal_parameter_list*的接口方法声明具有相同这意味着与方法声明在类中的 ([方法](classes.md#methods))。 接口方法声明不允许指定方法体中，并声明因此始终以分号结尾。

每种形式参数类型的接口方法必须是输入安全 ([变体安全](interfaces.md#variance-safety))，并返回类型必须为`void`或输出安全。 此外，每个类类型约束、 接口类型约束和类型参数约束上方法的任何类型参数必须是输入安全的。

这些规则可确保任何协变或逆变的接口的使用情况保持类型安全。 例如，应用于对象的
```csharp
interface I<out T> { void M<U>() where U : T; }
```
是非法的因为的使用情况`T`用作类型参数约束上`U`不是输入安全的。

已不存在此限制可能会违反类型安全方式如下：
```csharp
class B {}
class D : B{}
class E : B {}
class C : I<D> { public void M<U>() {...} }
...
I<B> b = new C();
b.M<E>();
```
这是实际调用`C.M<E>`。 但该调用需要`E`派生自`D`，因此此处违反类型安全。

### <a name="interface-properties"></a>接口属性

接口属性的声明使用*interface_property_declaration*s:

```antlr
interface_property_declaration
    : attributes? 'new'? type identifier '{' interface_accessors '}'
    ;

interface_accessors
    : attributes? 'get' ';'
    | attributes? 'set' ';'
    | attributes? 'get' ';' attributes? 'set' ';'
    | attributes? 'set' ';' attributes? 'get' ';'
    ;
```

*特性*，*类型*，并*标识符*接口属性声明的类中具有相同含义的属性声明 ([属性](classes.md#properties))。

接口属性声明的访问器对应于类属性声明的访问器 ([访问器](classes.md#accessors))，只不过访问器正文必须始终是一个分号。 因此，访问器只需指示属性是读写、 只读的或只写。

接口属性的类型必须是输出安全是否存在 get 访问器，并且必须是输入安全如果没有 set 访问器。

### <a name="interface-events"></a>接口事件

使用声明接口事件*interface_event_declaration*s:

```antlr
interface_event_declaration
    : attributes? 'new'? 'event' type identifier ';'
    ;
```

*特性*，*类型*，并*标识符*接口事件声明的类中具有相同的事件声明的含义 ([事件](classes.md#events)).

接口事件的类型必须是输入安全。

### <a name="interface-indexers"></a>接口索引器

使用声明接口索引器*interface_indexer_declaration*s:

```antlr
interface_indexer_declaration
    : attributes? 'new'? type 'this' '[' formal_parameter_list ']' '{' interface_accessors '}'
    ;
```

*特性*，*类型*，并*formal_parameter_list*接口索引器声明的类 (中具有与索引器声明相同的含义[索引器](classes.md#indexers))。

接口索引器声明的访问器对应于类索引器声明的访问器 ([索引器](classes.md#indexers))，只不过访问器正文必须始终是一个分号。 因此，访问器只需指示索引器为读写、 只读的或只写。

接口索引器的所有形式参数类型必须是输入安全。 此外，任何`out`或`ref`形参类型还必须是安全的输出。 请注意，甚至`out`参数需是输入安全的由于基础执行平台的限制。

接口索引器的类型必须是输出安全是否存在 get 访问器，并且必须是输入安全如果没有 set 访问器。

### <a name="interface-member-access"></a>接口成员访问

成员访问通过访问接口成员 ([成员访问](expressions.md#member-access)) 和索引器访问 ([索引器访问](expressions.md#indexer-access)) 形式的表达式`I.M`并`I[A]`，其中`I`是一个接口类型，`M`是方法、 属性或事件的接口类型，和`A`是索引器参数列表。

接口的仅限单一继承 （继承链中的每个接口都恰好零个或一个直接的基接口）、 成员查找的效果 ([成员查找](expressions.md#member-lookup))，方法调用 ([方法调用](expressions.md#method-invocations))，和索引器访问 ([索引器访问](expressions.md#indexer-access)) 规则是完全与类和结构相同：派生成员隐藏较少派生的成员具有相同名称或签名。 但是，对于多个继承的接口，二义性可能发生，当两个或多个不相关的基接口声明具有相同名称或签名的成员。 本部分介绍这种情况下的几个示例。 在所有情况下，可以使用显式强制转换来解决歧义。

在示例
```csharp
interface IList
{
    int Count { get; set; }
}

interface ICounter
{
    void Count(int i);
}

interface IListCounter: IList, ICounter {}

class C
{
    void Test(IListCounter x) {
        x.Count(1);                  // Error
        x.Count = 1;                 // Error
        ((IList)x).Count = 1;        // Ok, invokes IList.Count.set
        ((ICounter)x).Count(1);      // Ok, invokes ICounter.Count
    }
}
```
前两个语句会导致编译时错误，因为成员查找 ([成员查找](expressions.md#member-lookup)) 的`Count`中`IListCounter`不明确。 如示例所示，通过强制转换来解决二义性`x`为适当的基接口类型。 这种转换具有不运行时成本 — 它们只是包含的作为派生程度更小类型在编译时查看的实例。

在示例
```csharp
interface IInteger
{
    void Add(int i);
}

interface IDouble
{
    void Add(double d);
}

interface INumber: IInteger, IDouble {}

class C
{
    void Test(INumber n) {
        n.Add(1);                // Invokes IInteger.Add
        n.Add(1.0);              // Only IDouble.Add is applicable
        ((IInteger)n).Add(1);    // Only IInteger.Add is a candidate
        ((IDouble)n).Add(1);     // Only IDouble.Add is a candidate
    }
}
```
在调用`n.Add(1)`中选择`IInteger.Add`通过应用的重载决策规则[重载决策](expressions.md#overload-resolution)。 同样的调用`n.Add(1.0)`选择`IDouble.Add`。 插入了显式强制转换，就只有一个候选方法，因此没有歧义。

在示例
```csharp
interface IBase
{
    void F(int i);
}

interface ILeft: IBase
{
    new void F(int i);
}

interface IRight: IBase
{
    void G();
}

interface IDerived: ILeft, IRight {}

class A
{
    void Test(IDerived d) {
        d.F(1);                 // Invokes ILeft.F
        ((IBase)d).F(1);        // Invokes IBase.F
        ((ILeft)d).F(1);        // Invokes ILeft.F
        ((IRight)d).F(1);       // Invokes IBase.F
    }
}
```
`IBase.F`情况下隐藏成员`ILeft.F`成员。 在调用`d.F(1)`因此选择`ILeft.F`，即使`IBase.F`似乎未隐藏，引导完成的访问路径中`IRight`。

多个继承接口中的隐藏的直观规则只是这样：如果在任何一个访问路径，则隐藏该成员，它是隐藏在所有的访问路径中。 因为从的访问路径`IDerived`到`ILeft`到`IBase`隐藏`IBase.F`，从访问路径中还隐藏该成员`IDerived`到`IRight`到`IBase`。

## <a name="fully-qualified-interface-member-names"></a>完全限定的接口成员名称

接口成员有时会通过其***完全限定的名称***。 接口成员的完全限定的名包含的名称的接口中的成员是声明，跟一个点，然后是的成员的名称。 成员的完全限定的名称引用在其中声明成员的接口。 例如，给定的声明
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}
```
完全限定的名称`Paint`是`IControl.Paint`和完全限定的名称`SetText`是`ITextBox.SetText`。

在上面的示例中，它不能是指`Paint`作为`ITextBox.Paint`。

如果接口是命名空间的一部分，接口成员的完全限定的名包括命名空间名称。 例如
```csharp
namespace System
{
    public interface ICloneable
    {
        object Clone();
    }
}
```

此处，完全限定的名称`Clone`方法是`System.ICloneable.Clone`。

## <a name="interface-implementations"></a>接口实现

可能由类和结构实现接口。 若要指示类或结构直接实现一个接口，类或结构的基列表中包含的接口标识符。 例如：
```csharp
interface ICloneable
{
    object Clone();
}

interface IComparable
{
    int CompareTo(object other);
}

class ListEntry: ICloneable, IComparable
{
    public object Clone() {...}
    public int CompareTo(object other) {...}
}
```

类或结构直接实现的接口直接隐式实现的所有接口的基接口。 即使类或结构不会显式列出基类列表中的所有基接口，这是如此。 例如：
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    public void Paint() {...}
    public void SetText(string text) {...}
}
```

在这里，类`TextBox`同时实现`IControl`和`ITextBox`。

当类`C`直接实现一个接口，从 C 派生的所有类还隐式实现接口。 在类声明中指定的基接口可以是构造的接口类型 ([构造类型](types.md#constructed-types))。 基接口不能是自身、 类型参数，但它可能涉及到作用域中的类型参数。 下面的代码演示如何对一个类可以实现和扩展构造的类型：
```csharp
class C<U,V> {}

interface I1<V> {}

class D: C<string,int>, I1<string> {}

class E<T>: C<int,T>, I1<T> {}
```

泛型类声明的基接口必须满足唯一性规则中所述[实现的接口的唯一性](interfaces.md#uniqueness-of-implemented-interfaces)。

### <a name="explicit-interface-member-implementations"></a>显式接口成员实现代码

为了实现接口，类或结构可以声明***显式接口成员实现代码***。 显式接口成员实现是引用完全限定的接口成员名称的方法、 属性、 事件或索引器声明。 例如
```csharp
interface IList<T>
{
    T[] GetElements();
}

interface IDictionary<K,V>
{
    V this[K key];
    void Add(K key, V value);
}

class List<T>: IList<T>, IDictionary<int,T>
{
    T[] IList<T>.GetElements() {...}
    T IDictionary<int,T>.this[int index] {...}
    void IDictionary<int,T>.Add(int index, T value) {...}
}
```

这里`IDictionary<int,T>.this`和`IDictionary<int,T>.Add`是显式接口成员实现。

在某些情况下，接口成员的名称可能不是适用于实现的类，将在其中用例的接口成员可能会实现使用显式接口成员实现。 类实现文件抽象，例如，可能会实现`Close`成员函数，释放文件资源的影响并实现了`Dispose`方法的`IDisposable`接口使用显式接口成员的实现：
```csharp
interface IDisposable
{
    void Dispose();
}

class MyFile: IDisposable
{
    void IDisposable.Dispose() {
        Close();
    }

    public void Close() {
        // Do what's necessary to close the file
        System.GC.SuppressFinalize(this);
    }
}
```

不能通过在方法调用、 属性访问或索引器访问其完全限定名称访问的显式接口成员实现。 显式接口成员实现只能通过接口实例访问，并且在这种情况下引用只需通过其成员名称。

它是显式接口成员实现包括访问修饰符的编译时错误，它会导致编译时错误包含修饰符`abstract`， `virtual`， `override`，或`static`。

显式接口成员实现具有不同的可访问性特征与其他成员。 由于显式接口成员实现代码永远不能通过在方法调用或属性访问其完全限定名称，它们是在专用的意义上。 但是，由于它们可以通过将接口实例访问，因此可以在某种意义上也是公共。

显式接口成员实现代码有两个主要用途：

*  显式接口成员实现代码不是通过类或结构的实例可访问的因为它们允许接口实现，若要从公共接口的类或结构中排除。 此功能特别有用，当类或结构实现此类或结构的使用者不感兴趣的内部接口。
*  显式接口成员实现允许消除具有相同签名的接口成员的歧义。 没有显式接口成员实现代码是类或结构不可能具有不同的实现的接口具有相同签名的成员，并作为返回类型，它是类或结构不可能具有任何实现在所有接口成员具有相同的签名，但使用不同的返回类型。

有效的显式接口成员实现，类或结构必须命名其基类列表中包含其完全限定的名称、 类型和参数类型完全匹配的显式接口成员的成员的接口实现。 因此，在下面的类
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
    int IComparable.CompareTo(object other) {...}    // invalid
}
```
声明`IComparable.CompareTo`会导致编译时错误，因为`IComparable`的基类列表中未列出`Shape`并不是基接口的`ICloneable`。 同样，在声明中
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
}

class Ellipse: Shape
{
    object ICloneable.Clone() {...}    // invalid
}
```
声明`ICloneable.Clone`中`Ellipse`导致编译时错误，因为`ICloneable`的基类列表中未显式列出`Ellipse`。

接口成员的完全限定的名称必须引用在其中声明成员的接口。 因此，在下面的声明
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
}
```
显式接口成员实现的`Paint`必须编写为`IControl.Paint`。

### <a name="uniqueness-of-implemented-interfaces"></a>实现的接口的唯一性

实现泛型类型声明的接口必须保持唯一的所有可能的构造类型。 如果没有此规则，将无法确定要调用的某些构造的类型的正确方法。 例如，假设一个泛型类声明了允许按以下方式编写：
```csharp
interface I<T>
{
    void F();
}

class X<U,V>: I<U>, I<V>                    // Error: I<U> and I<V> conflict
{
    void I<U>.F() {...}
    void I<V>.F() {...}
}
```

如果允许这样，它无法确定要在以下情况下执行的代码：
```csharp
I<int> x = new X<int,int>();
x.F();
```

若要确定泛型类型声明的接口列表是否有效，请执行以下步骤：

*  让`L`是直接在泛型类、 结构或接口声明中指定的接口的列表`C`。
*  将添加到`L`任何基接口的接口已在`L`。
*  删除任何重复项`L`。
*  如果任何可能构造从创建类型`C`将类型参数会代入后`L`，会导致在两个接口`L`完全相同，然后的声明`C`无效。 确定所有可能的构造的类型时，不会考虑约束声明。

在类声明中`X`接口列表上方`L`组成`I<U>`和`I<V>`。 声明无效，因为任何构造具有类型`U`和`V`正在相同的类型会导致这两个接口是相同的类型。

此外，可以在不同的继承级别统一上指定的接口：
```csharp
interface I<T>
{
    void F();
}

class Base<U>: I<U>
{
    void I<U>.F() {...}
}

class Derived<U,V>: Base<U>, I<V>    // Ok
{
    void I<V>.F() {...}
}
```

此代码是有效的即使`Derived<U,V>`同时实现`I<U>`和`I<V>`。 代码
```csharp
I<int> x = new Derived<int,int>();
x.F();
```
调用中的方法`Derived`，因为`Derived<int,int>`有效地重新实现`I<int>`([接口重新实现](interfaces.md#interface-re-implementation))。

### <a name="implementation-of-generic-methods"></a>泛型方法的实现

当泛型方法隐式实现接口方法，提供每个方法类型参数必须是两个声明中的等效 （在任何接口类型参数替换为适当的类型参数），其中的约束方法类型参数进行标识的序号位置，从左到右。

当泛型方法显式实现接口方法时，但是，实现方法上不允许使用任何约束。 相反，约束被继承的接口方法

```csharp
interface I<A,B,C>
{
    void F<T>(T t) where T: A;
    void G<T>(T t) where T: B;
    void H<T>(T t) where T: C;
}

class C: I<object,C,string>
{
    public void F<T>(T t) {...}                    // Ok
    public void G<T>(T t) where T: C {...}         // Ok
    public void H<T>(T t) where T: string {...}    // Error
}
```

该方法`C.F<T>`隐式实现`I<object,C,string>.F<T>`。 在这种情况下，`C.F<T>`不是需要 （也不允许） 来指定该约束`T:object`由于`object`是隐式所有类型参数约束。 该方法`C.G<T>`隐式实现`I<object,C,string>.G<T>`由于约束匹配那些在界面中，在接口类型参数替换为相应类型参数之后。 方法的约束`C.H<T>`是一个错误因为密封类型 (`string`这种情况下) 不能用作约束。 由于隐式接口方法实现的约束需要匹配省略该约束也将是一个错误。 因此，就无法隐式实现`I<object,C,string>.H<T>`。 仅可以使用显式接口成员实现来实现此接口方法：
```csharp
class C: I<object,C,string>
{
    ...

    public void H<U>(U u) where U: class {...}

    void I<object,C,string>.H<T>(T t) {
        string s = t;    // Ok
        H<T>(t);
    }
}
```

在此示例中，显式接口成员实现调用具有更严格弱约束的公共方法。 请注意，从赋值`t`到`s`无效，因为`T`继承的约束`T:string`，即使此约束不可表示源代码中。

### <a name="interface-mapping"></a>接口映射

类或结构必须提供的接口的类或结构的基列表中列出的所有成员的实现。 查找实现接口成员的实现的类或结构中的过程被称为***接口映射***。

类或结构的接口映射`C`为每个接口的基的类列表中指定的每个成员定位实现`C`。 特定的接口成员的实现`I.M`，其中`I`是在其中的接口成员`M`声明，确定通过检查每个类或结构`S`，从开始`C`和为每个后续基类重复`C`，直到找到匹配项：

*  如果`S`包含声明的匹配的显式接口成员实现`I`并`M`，则此成员是实现`I.M`。
*  否则为如果`S`包含非静态公共成员相匹配的声明`M`，则此成员是实现`I.M`。 如果多个匹配项有一个成员，它是未指定哪个成员是实现`I.M`。 这种情况下只能如果`S`为构造的类型，其中泛型类型中声明的两个成员具有不同的签名，但类型参数可以让它们的签名完全相同。

如果实现找不到指定的基类列表中的所有接口的所有成员会发生编译时错误`C`。 请注意接口的成员，包括那些继承自基接口的成员。

有关接口映射，类成员的含义`A`与接口成员匹配`B`时：

*  `A` 并`B`方法，和名称、 类型和形参表的`A`和`B`完全相同。
*  `A` 和`B`是属性、 名称和类型的`A`和`B`是相同的并且`A`具有相同的访问器作为`B`(`A`允许具有其他访问器，如果它不是显式接口成员实现）。
*  `A` 并`B`是事件，以及名称和类型的`A`和`B`完全相同。
*  `A` 和`B`是索引器、 类型和形参列表`A`并`B`是相同的和`A`具有相同的访问器作为`B`(`A`允许具有其他访问器，如果不是显式接口成员实现）。

值得注意的接口映射算法含义是：

*  确定实现接口成员的类或结构成员时，显式接口成员实现代码优先于同一类或结构中的其他成员。
*  非公共既不静态成员参与接口映射。

在示例
```csharp
interface ICloneable
{
    object Clone();
}

class C: ICloneable
{
    object ICloneable.Clone() {...}
    public object Clone() {...}
}
```
`ICloneable.Clone`的成员`C`的实现变得`Clone`中`ICloneable`因为显式接口成员实现代码优先于其他成员。

如果类或结构实现两个或多个接口包含具有相同的名称、 类型和参数类型的成员，则可以将每个到单个类或结构成员上这些接口成员。 例如
```csharp
interface IControl
{
    void Paint();
}

interface IForm
{
    void Paint();
}

class Page: IControl, IForm
{
    public void Paint() {...}
}
```

在这里，`Paint`两个方法`IControl`并`IForm`映射到`Paint`中的方法`Page`。 当然也很可能有两个方法的单独的显式接口成员实现代码。

如果类或结构实现包含隐藏的成员的接口，然后某些成员必须通过显式接口成员实现来实现。 例如
```csharp
interface IBase
{
    int P { get; }
}

interface IDerived: IBase
{
    new int P();
}
```

此接口的实现将需要至少一个显式接口成员实现，并将采用以下形式之一
```csharp
class C: IDerived
{
    int IBase.P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    public int P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    int IBase.P { get {...} }
    public int P() {...}
}
```

当一个类实现多个具有相同的基接口的接口时，可以只有一个基接口的实现。 在示例
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

class ComboBox: IControl, ITextBox, IListBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
    void IListBox.SetItems(string[] items) {...}
}
```
不能具有单独实现`IControl`在基的类列表中，名为`IControl`由继承`ITextBox`，和`IControl`由继承`IListBox`。 实际上，是标识的没有不同，这些接口的概念。 相反，实现`ITextBox`和`IListBox`共享的相同实现`IControl`，和`ComboBox`只需实现三个接口，被视为`IControl`， `ITextBox`，和`IListBox`。

基类的成员参与接口映射。 在示例
```csharp
interface Interface1
{
    void F();
}

class Class1
{
    public void F() {}
    public void G() {}
}

class Class2: Class1, Interface1
{
    new public void G() {}
}
```
该方法`F`中`Class1`中使用`Class2`的实现`Interface1`。

### <a name="interface-implementation-inheritance"></a>接口实现继承

一个类继承其基类所提供的所有接口实现。

无需显式***重新实现***接口，派生的类不能以任何方式更改它从其基类继承的接口映射。 例如，在声明中
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public void Paint() {...}
}

class TextBox: Control
{
    new public void Paint() {...}
}
```
`Paint`中的方法`TextBox`隐藏`Paint`中的方法`Control`，但它不会更改的映射`Control.Paint`拖动到`IControl.Paint`，以及对调用`Paint`通过类实例和接口实例将具有以下效果
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes Control.Paint();
```

但是，当接口方法映射到类中的虚方法时，就可以为派生类重写虚拟方法并更改接口的实现。 例如，重写到上面的声明
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public virtual void Paint() {...}
}

class TextBox: Control
{
    public override void Paint() {...}
}
```
现在观察到以下影响
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes TextBox.Paint();
```

显式接口成员实现代码不能声明为虚拟函数，因为它不能重写显式接口成员实现。 但是，它是完全有效的显式接口成员实现调用另一个方法，并且，其他方法可以声明为虚拟函数以允许派生类重写它。 例如
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() { PaintControl(); }
    protected virtual void PaintControl() {...}
}

class TextBox: Control
{
    protected override void PaintControl() {...}
}
```

在这里，类派生自`Control`可以专用化的实现`IControl.Paint`通过重写`PaintControl`方法。

### <a name="interface-re-implementation"></a>接口重新实现

继承的接口实现的类允许***重新实现***通过基类列表中包含的接口。

接口重新实现遵循完全相同接口映射的规则的初始实现的接口。 因此，继承的接口映射不起作用的接口映射上建立的重新实现的接口。 例如，在声明中
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() {...}
}

class MyControl: Control, IControl
{
    public void Paint() {}
}
```
这一事实，`Control`映射`IControl.Paint`拖动到`Control.IControl.Paint`不会影响在重新实现`MyControl`，映射`IControl.Paint`拖到`MyControl.Paint`。

继承的公共成员声明和继承的显式接口成员声明参与重新实现的接口的接口映射过程。 例如
```csharp
interface IMethods
{
    void F();
    void G();
    void H();
    void I();
}

class Base: IMethods
{
    void IMethods.F() {}
    void IMethods.G() {}
    public void H() {}
    public void I() {}
}

class Derived: Base, IMethods
{
    public void F() {}
    void IMethods.H() {}
}
```

此处，实现`IMethods`中`Derived`映射到的接口方法`Derived.F`， `Base.IMethods.G`， `Derived.IMethods.H`，并`Base.I`。

当类实现一个接口，它隐式还实现了所有该接口的基接口。 同样，一个接口重新实现还隐式是重新实现的所有接口的基接口。 例如
```csharp
interface IBase
{
    void F();
}

interface IDerived: IBase
{
    void G();
}

class C: IDerived
{
    void IBase.F() {...}
    void IDerived.G() {...}
}

class D: C, IDerived
{
    public void F() {...}
    public void G() {...}
}
```

此处，重新实现`IDerived`还重新实现`IBase`映射`IBase.F`拖动到`D.F`。

### <a name="abstract-classes-and-interfaces"></a>抽象类和接口

就像一个非抽象类，抽象类必须提供的接口的类的基类列表中列出的所有成员的实现。 但是，一个抽象类被允许接口方法映射到抽象方法。 例如
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    public abstract void F();
    public abstract void G();
}
```

此处，实现`IMethods`映射`F`并`G`到抽象方法，其必须重写的派生自非抽象类中`C`。

请注意，显式接口成员实现代码不能为抽象的但当然允许显式接口成员实现调用抽象方法。 例如
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    void IMethods.F() { FF(); }
    void IMethods.G() { GG(); }
    protected abstract void FF();
    protected abstract void GG();
}
```

此处，派生自非抽象类`C`所需重写`FF`并`GG`，从而提供的实际实现`IMethods`。
